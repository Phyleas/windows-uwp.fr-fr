---
description: Ces points d’extension en C++/WinRT 2.0 vous permettent de différer la destruction de vos types d’implémentation, d’interroger en toute sécurité pendant la destruction et de raccorder l’entrée et la sortie de vos méthodes projetées.
title: Points d’extension pour vos types d’implémentation
ms.date: 09/26/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, destruction différée, requêtes sécurisées
ms.localizationpriority: medium
ms.openlocfilehash: 6b15c32bb35bec1f6a8e8d59e6aefe17ebf74b5d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170326"
---
# <a name="extension-points-for-your-implementation-types"></a>Points d’extension pour vos types d’implémentation

Le modèle [modèle de struct winrt::implements](/uwp/cpp-ref-for-winrt/implements) est la base à partir de laquelle vos propres implémentations [C++/WinRT](./intro-to-using-cpp-with-winrt.md) (de classes runtime et de fabriques d’activation) dérivent directement ou indirectement.

Cette rubrique décrit les points d’extension de **winrt::implements** dans C++/WinRT 2.0. Vous pouvez choisir d’implémenter ces points d’extension sur vos types d’implémentation pour personnaliser le comportement par défaut des objets inspectable (*inspectable* au sens de l’interface [IInspectable](/windows/win32/api/inspectable/nn-inspectable-iinspectable)).

Ces points d’extension vous permettent de différer la destruction de vos types d’implémentation, de lancer des requêtes de manière sécurisée pendant la destruction et de raccorder l’entrée et la sortie de vos méthodes projetées. Cette rubrique décrit ces fonctionnalités et explique plus en détail quand et comment les utiliser.

## <a name="deferred-destruction"></a>Destruction différée

Dans la rubrique [Diagnostic des allocations directes](./diag-direct-alloc.md), nous avons souligné le fait que votre type d’implémentation ne peut avoir de destructeur privé.

Un destructeur public présente l'avantage de permettre une destruction différée, à savoir la possibilité de détecter le dernier appel [**IUnknown::Release**](/windows/win32/api/unknwn/nf-unknwn-iunknown-release) sur votre objet, puis de s'approprier cet objet pour différer indéfiniment sa destruction.

Rappelez-vous que les objets COM classiques sont soumis à un décompte intrinsèque des références ; ce décompte de références est géré via les fonctions [**IUnknown::AddRef**](/windows/win32/api/unknwn/nf-unknwn-iunknown-addref) et **IUnknown::Release**. Dans une implémentation traditionnelle de **Release**, le destructeur C++ de l'objet COM classique est appelé lorsque le décompte de références atteint 0.

```cppwinrt
uint32_t WINRT_CALL Release() noexcept
{
    uint32_t const remaining{ subtract_reference() };
 
    if (remaining == 0)
    {
        delete this;
    }
 
    return remaining;
}
```

`delete this;` appelle le destructeur de l’objet avant de libérer la mémoire occupée par l’objet. Cela fonctionne plutôt bien , sous réserve que n’ayez rien à faire d'intéressant dans votre destructeur.

```cppwinrt
using namespace winrt::Windows::Foundation;
... 
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    ~Sample() noexcept
    {
        // Too late to do anything interesting.
    }
};
```

Qu'entendez-vous par *intéressant* ? En fait, un destructeur est intrinsèquement synchrone. Vous ne pouvez pas basculer de threads&mdash; pour détruire des ressources spécifiques aux threads dans un contexte différent. Vous ne pouvez pas interroger avec fiabilité l’objet pour une autre interface dont vous pourriez avoir besoin afin de libérer certaines ressources. Et ce n'est pas tout. Dans le cas d'une destruction non triviale, il vous faut une solution plus flexible. C'est là qu'intervient la fonction **final_release** de C++/WinRT.

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    static void final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        // This is the first stop...
    }
 
    ~Sample() noexcept
    {
        // ...And this happens only when *unique_ptr* finally deletes the object.
    }
};
```

Nous avons mis à jour l'implémentation C++/WinRT de **Release** pour appeler votre **final_release** au moment où le décompte des références de votre objet atteint 0. L’objet sait ainsi qu'il n'existe pas d'autres références en suspens et qu'il détient sa propre propriété. Dès lors, il peut transférer sa propre propriété à la fonction **final_release** statique.

En d’autres termes, l’objet a opéré sa propre transformation, d’un objet prenant en charge une propriété partagée en objet détenant sa propre propriété. **std::unique_ptr** détient la propriété exclusive de l’objet, et détruira naturellement cet objet dans le cadre de sa sémantique&mdash;, d'où le besoin d'un destructeur public&mdash;lorsque **STD::unique_ptr** est hors de portée (sous réserve de ne pas être, au préalable, déplacé ailleurs). C’est la clé. Vous pouvez utiliser l’objet indéfiniment, à condition que **std::unique_ptr** le maintienne actif. Voici une illustration de la façon dont vous pouvez déplacer l’objet ailleurs.

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    static void final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        gc.push_back(std::move(ptr));
    }
};
```

Vous pouvez considérer qu'il s'agit d'un récupérateur de mémoire déterministe.

Normalement, l’objet est détruit quand **std::unique_ptr** est détruit. Vous pouvez toutefois accélérer sa destruction en appelant **std::unique_ptr::reset** ou la différer en enregistrant **std::unique_ptr** quelque part.

De manière plus concrète et plus performante, vous pouvez transformer la fonction **final_release** en coroutine et gérer son éventuelle destruction à un même emplacement, tout en conservant la possibilité de suspendre et de basculer les threads, si besoin.

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    static winrt::fire_and_forget final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        co_await winrt::resume_background(); // Unwind the calling thread.
 
        // Safely perform complex teardown here.
    }
};
```

Une suspension pointera les causes du thread appelant&mdash;qui a initié l'appel à la **fonction** IUnknown::Release&mdash; à retourner, et donc à signaler à l'appelant que l’objet qu’il conservait n'est plus disponible via ce pointeur d’interface. Les infrastructures d’interface utilisateur doivent souvent s’assurer que les objets sont détruits sur le thread d’interface utilisateur spécifique qui a créé l'objet. Cette fonctionnalité rend la satisfaction de cette exigence triviale, car la destruction est séparée de la libération de l’objet.

## <a name="safe-queries-during-destruction"></a>Requêtes sécurisées lors de la destruction

En s'appuyant sur la notion de destruction différée, il est possible d'interroger en toute sécurité les interfaces lors de la destruction.

COM classique repose sur deux concepts centraux. Le premier correspond au décompte des références, et le second à l'interrogation des interfaces. Outre **AddRef** et **Release**, l’interface **IUnknown** fournit [**QueryInterface**](/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(refiid_void)). Cette méthode est particulièrement sollicitée par certaines infrastructures d'interface utilisateur&mdash;telles que XAML, pour parcourir la hiérarchie XAML lorsqu’elle simule son système de type composable. Voici un exemple simple.

```cppwinrt
struct MainPage : PageT<MainPage>
{
    ~MainPage()
    {
        DataContext(nullptr);
    }
};
```

Cela peut *paraître* anodin. Cette page XAML entend effacer son contexte de données dans son destructeur. Mais [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext) est une propriété de la classe de base **FrameworkElement** et réside dans l'interface **IFrameworkElement** distincte. Dès lors, C++/WinRT doit injecter un appel dans **QueryInterface** pour rechercher la vtable qui convient avant de pouvoir appeler la propriété **DataContext**. Mais le fait que le décompte des références ait atteint 0 explique notre présence dans le destructeur. L'appel de **QueryInterface** perturbe temporairement le décompte des références et, lorsque celui-ci atteint à nouveau 0, l’objet est une nouvelle fois détruit.

C++/WinRT 2.0 a été renforcé pour prendre en charge cela. Voici l'implémentation C++/WinRT 2.0 de Release, sous une forme simplifiée.

```cppwinrt
uint32_t Release() noexcept
{
    uint32_t const remaining{ subtract_reference() };
 
    if (remaining == 0)
    {
        m_references = 1; // Debouncing!
        T::final_release(...);
    }
 
    return remaining;
}
```

Comme vous l'aviez peut-être envisagé, il décrémente d’abord le décompte des références et n'intervient qu'en l'absence de références en suspens. Toutefois, avant d’appeler la fonction **final_release** décrite précédemment dans cette rubrique, il stabilise le décompte des références en le définissant sur 1. Nous appelons cela *l'anti-rebond* (un terme emprunté au domaine électrique). C'est essentiel pour empêcher la libération de la dernière référence. Si cela venait à se produire, le décompte de références serait instable et incapable de prendre efficacement en charge un appel à **QueryInterface**.

Appeler **QueryInterface** est dangereux une fois la dernière référence libérée, car le décompte des références est alors susceptible d'augmenter indéfiniment. Il vous incombe d’appeler uniquement des chemins de code connus qui ne prolongeront pas la vie de l’objet. C++/WinRT vous y aide en veillant à ce que ces appels **QueryInterface** s'effectuent de manière fiable.

Pour ce faire, il stabilise le décompte des références. Une fois la dernière référence libérée, le décompte des références réel affiche 0 ou une valeur totalement imprévisible. Ce dernier cas peut se produire en présence de références faibles. Une telle situation n’est pas viable si un nouvel appel à **QueryInterface** intervient car cela provoque nécessairement une augmentation temporaire du décompte des références&mdash; d'où la référence à l'anti-rebond. Définir le décompte sur 1 permet de s'assurer qu'un dernier appel de **Release** n'intervienne jamais sur cet objet. Et c'est précisément ce que nous recherchons, puisque **std::unique_ptr** détient désormais l'objet, et que les appels limités aux paires **QueryInterface**/**Release** seront sécurisés.

Prenons un exemple plus intéressant encore.

```cppwinrt
struct MainPage : PageT<MainPage>
{
    ~MainPage()
    {
        DataContext(nullptr);
    }

    static winrt::fire_and_forget final_release(std::unique_ptr<MainPage> ptr)
    {
        co_await 5s;
        co_await winrt::resume_foreground(ptr->Dispatcher());
        ptr = nullptr;
    }
};
```

Tout d’abord, la fonction **final_release** est appelée, ce qui informe l'implémentation que le moment est venu de procéder au nettoyage. Ici, **final_release** relève d'une coroutine. Pour simuler un premier point de suspension, il commence par attendre le pool de threads pendant quelques secondes. Il reprend ensuite sur le thread du répartiteur de la page. Cette dernière étape implique une requête, puisque [**Dispatcher**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher) est une propriété de la classe de base **DependencyObject**. Enfin, la page est supprimée suite à l'affectation de `nullptr` à **std::unique_ptr**. Cela appelle ensuite le destructeur de la page.

À l’intérieur du destructeur, nous effaçons le contexte de données, ce qui, comme nous le savons, requiert une requête pour la classe de base **FrameworkElement**.

Tout cela est possible grâce à l'anti-rebond du décompte des références (ou à la stabilisation du décompte des références) offert par C++/WinRT 2.0.

## <a name="method-entry-and-exit-hooks"></a>Raccordements d’entrée et de sortie de méthode

Parmi les points d’extension moins utilisés, citons le struct **abi_guard** et les fonctions **abi_enter** et **abi_exit**.

Si votre type d’implémentation définit une fonction **abi_enter**, celle-ci est appelée à l’entrée de chacune de vos méthodes d’interface projetée (sans compter les méthodes de [IInspectable](/windows/win32/api/inspectable/nn-inspectable-iinspectable)).

De même, si vous définissez une fonction **abi_exit**, celle-ci est appelée à la sortie de chaque méthode de ce type. Toutefois, elle n’est pas appelée si votre **abi_enter** lève une exception. Elle *est* appelée si une exception est levée par votre méthode d’interface projetée.

Par exemple, vous pouvez utiliser **abi_enter** pour lever une exception hypothétique **invalid_state_error** si un client essaie d’utiliser un objet alors que celui-ci vient d’être placé dans un état inutilisable (par exemple, après un appel de méthode **Shut­Down** ou **Disconnect**). Les classes d’itérateurs C++/WinRT utilisent cette fonctionnalité pour lever une exception d’état non valide dans la fonction **abi_enter** si la collection sous-jacente a changé.

Au-delà des simples fonctions **abi_enter** et **abi_exit**, vous pouvez définir un type imbriqué nommé **abi_guard**. Dans ce cas, une instance d’**abi_guard** est créée à l’entrée à chacune de vos méthodes d’interface projetée (non-**IInspectable**), avec une référence à l’objet comme paramètre de constructeur. Le type **abi_guard** est ensuite détruit à la sortie de la méthode. Vous pouvez placer d’autres états de votre choix dans votre type **abi_guard**.

Si vous ne définissez pas votre propre type **abi_guard**, un type par défaut appelle **abi_enter** au moment de la construction et **abi_exit** au moment de la destruction.

Ces protections sont uniquement utilisées quand une méthode est appelée *par le biais de l’interface projetée*. Si vous appelez des méthodes directement sur l’objet d’implémentation, ces appels passent directement à l’implémentation sans aucune protection.

Voici un exemple de code.

```cppwinrt
struct Sample : SampleT<Sample, IClosable>
{
    void abi_enter();
    void abi_exit();

    void Close();
};

void example1()
{
    auto sampleObj1{ winrt::make<Sample>() };
    sampleObj1.Close(); // Calls abi_enter and abi_exit.
}

void example2()
{
    auto sampleObj2{ winrt::make_self<Sample>() };
    sampleObj2->Close(); // Doesn't call abi_enter nor abi_exit.
}

// A guard is used only for the duration of the method call.
// If the method is a coroutine, then the guard applies only until
// the IAsyncXxx is returned; not until the coroutine completes.

IAsyncAction CloseAsync()
{
    // Guard is active here.
    DoWork();

    // Guard becomes inactive once DoOtherWorkAsync
    // returns an IAsyncAction.
    co_await DoOtherWorkAsync();

    // Guard is not active here.
}
```