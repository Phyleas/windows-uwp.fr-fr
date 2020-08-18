---
description: Il s’agit d’une rubrique avancée relative au portage progressif de [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) vers [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt). Elle montre comment les coroutines et les tâches PPL (Parallel Patterns Library) peuvent exister côte à côte dans le même projet.
title: Asynchronisme, interopérabilité entre C++/WinRT et C++/CX
ms.date: 08/06/2020
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, déplacer, migrer, interop, C++/CX, PPL, tâche, coroutine
ms.localizationpriority: medium
ms.openlocfilehash: daa263836e9024a0aaa55b239b1a0db9437f1cd5
ms.sourcegitcommit: a9f44bbb23f0bc3ceade3af7781d012b9d6e5c9a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88181071"
---
# <a name="asynchrony-and-interop-between-cwinrt-and-ccx"></a>Asynchronisme, interopérabilité entre C++/WinRT et C++/CX

> [!TIP]
> Même si nous vous recommandons de lire cette rubrique depuis le début, vous pouvez accéder directement à un résumé des techniques d’interopérabilité dans la section [Vue d’ensemble du déplacement de C++/CX asynchrone vers C++/WinRT](#overview-of-porting-ccx-async-to-cwinrt).

Il s’agit d’une rubrique avancée relative au déplacement progressif vers [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) de [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx). Cette rubrique commence là où la rubrique [Interopérabilité entre C++/WinRT et C++/CX](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx) s’arrête.

Si la taille ou la complexité de votre codebase rend nécessaire le déplacement progressif de votre projet, vous aurez besoin d’un processus de déplacement dans lequel, le code C++/CX et C++/WinRT existent côte à côte pendant un moment dans le même projet. Si vous avez un code asynchrone, vous devrez peut-être avoir des chaînes de tâches de bibliothèque de modèles parallèles (PPL) et des coroutines existant côte à côte dans votre projet à mesure que vous déplacez progressivement votre code source. Cette rubrique se concentre sur les techniques d’interopérabilité entre le code asynchrone C++/CX et le code asynchrone C++/WinRT. Vous pouvez utiliser ces techniques individuellement ou ensemble.

Avant de lire cette rubrique, il est judicieux de lire [Interopérabilité entre C++/WinRT et C++/CX](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx). Cette rubrique vous montre comment préparer votre projet pour le déplacement progressif. Elle présente également deux fonctions d’application auxiliaire que vous pouvez utiliser pour convertir un objet C++/CX en un objet C++/WinRT (et inversement). Cette rubrique sur l’asynchronisme s’appuie sur ces informations et utilise ces fonctions d’application auxiliaire.

> [!NOTE]
> Le déplacement progressif présente certaines limites. Si vous avez un projet de composant Windows Runtime, le déplacement progressif est impossible et vous devrez déplacer le projet en une seule passe. Et pour un projet XAML, à un moment donné, vos types de pages XAML doivent être *soit* tous les C++/WinRT *soit* tous les C++/CX. Pour plus d’informations, consultez [Déplacer vers C++/WinRT à partir de C++/CX](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx).

## <a name="the-reason-an-entire-topic-is-dedicated-to-asynchronous-code-interop"></a>La raison pour laquelle une rubrique entière est dédiée à l’interopérabilité du code asynchrone

Le déplacement à partir de C++/CX vers C++/WinRT est généralement simple, à l’exception du déplacement depuis des tâches [Bibliothèque de modèles parallèles (PPL)](/cpp/parallel/concrt/parallel-patterns-library-ppl) vers des coroutines. Les modèles sont différents. Il n’existe pas de mappage un-à-un naturel entre les tâches PPL et les coroutines et il n’existe aucun moyen simple de déplacer mécaniquement le code qui fonctionne pour tous les cas.

La bonne nouvelle, c’est que la conversion des tâches en coroutines entraîne des simplifications significatives. En outre, les équipes de développement signalent régulièrement qu’une fois qu’elles dépassent le seuil de déplacement de leur code asynchrone, le reste du travail de déplacement est essentiellement mécanique.

Souvent, un algorithme a été initialement écrit pour répondre à des API synchrones. Ensuite, cela a été traduit en tâches et en continuations explicites&mdash;le résultat est souvent un obscurcissement par inadvertance de la logique sous-jacente. Par exemple, les boucles deviennent récursivité ; les branches if-else se transforment en arborescence imbriquée (une chaîne) de tâches ; les variables partagées deviennent **shared_ptr**. Pour déconstruire la structure souvent non naturelle du code source PPL, nous vous recommandons de commencer par revenir en arrière et de comprendre l’intention du code d’origine (c’est-à-dire de découvrir la version synchrone d’origine). Puis insérez `co_await` dans les emplacements appropriés.

Pour cette raison, si vous disposez d’une version C# (au lieu de C++/CX) du code asynchrone à partir duquel commencer votre déplacement, cela peut vous permettre d’obtenir une gestion du temps plus simple et un déplacement plus propre. Le code C# utilise `await`. Ainsi, le code C# suit déjà essentiellement une philosophie de début avec une version synchrone, puis insère `await` dans les espaces appropriés.

## <a name="some-background-in-asynchronous-programming"></a>Un arrière-plan en programmation asynchrone

Afin d’avoir un cadre de référence ferme pour des concepts et une terminologie de la programmation asynchrone, nous allons rapidement définir la scène concernant la programmation asynchrone Windows Runtime en général, ainsi que la façon dont les deux projections de langage C++ sont chacune superposées au-dessus de manière différente.

Votre projet a des méthodes qui fonctionnent de manière asynchrone. Dans de nombreux cas, vous souhaiterez attendre la fin de ce travail avant d’effectuer une autre opération. Pour que vous puissiez attendre sur une méthode asynchrone, la méthode retourne un objet d’opération asynchrone. Mais parfois, vous ne souhaitez pas ou devez attendre la fin du travail effectué de façon asynchrone. Dans ce cas, la méthode n’a pas besoin de retourner un objet d’opération asynchrone. Une méthode asynchrone que vous n’attendez pas est connue comme une méthode de type *tire et oublie*.

### <a name="windows-runtime-async-objects-iasyncxxx"></a>Objets asynchrones Windows Runtime (**IAsyncXxx**)

L’espace de noms Windows Runtime **Windows::Foundation** contient quatre types d’objets d’opérations asynchrones.

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction),
- [**IAsyncActionWithProgress&lt;TProgress&gt;**](/uwp/api/windows.foundation.iasyncactionwithprogress-1),
- [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation-1) et
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress-2).

Dans cette rubrique, lorsque nous utilisons le raccourci pratique de **IAsyncXxx**, nous faisons référence à ces types collectivement ou nous parlons de l’un des quatre types sans avoir à spécifier lequel.

### <a name="ccx-async"></a>C++/CX asynchrone

Le code C++/CX asynchrone utilise les tâches de [Bibliothèque de modèles parallèles (PPL)](/cpp/parallel/concrt/parallel-patterns-library-ppl). Une tâche PPL est représentée par la classe [**concurrency::task**](/cpp/parallel/concrt/reference/task-class).

En règle générale, une méthode C++/CX asynchrone associe des tâches PPL en utilisant des fonctions lambda avec [**concurrency::create_task**](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_task) et [**concurrency::task::then**](/cpp/parallel/concrt/reference/task-class#then). Chacune de ces fonctions lambda retourne une tâche qui, lorsqu’elle se termine, produit une valeur qui est ensuite passée dans le lambda de la *continuation* de la tâche.

Sinon, au lieu d’appeler **create_task** pour créer une tâche, une méthode C++/CX asynchrone peut appeler [**concurrency::create_async**](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_async) pour créer un **IAsyncXxx**\^.

Le type de retour d’une méthode C++/CX asynchrone peut être une tâche PPL ou un **IAsyncXxx**\^.

Dans les deux cas, la méthode elle-même utilise le mot-clé `return` pour retourner un objet asynchrone qui, une fois terminé, produit la valeur que l’appelant souhaite réellement (peut-être un fichier, un tableau d’octets ou un booléen).

> [!NOTE]
> Si une méthode C++/CX asynchrone retourne un **IAsyncXxx**\^, le **TResult** est limité à être un type Windows Runtime. Une valeur booléenne, par exemple, est un type Windows Runtime ; Toutefois, un type projeté C++/CX (par exemple, **Platform::Array<byte>** ^) ne l’est pas.

### <a name="cwinrt-async"></a>C++/WinRT asynchrone

C++/WinRT intègre des coroutines C++ dans le modèle de programmation. Les coroutines et l’instruction `co_await` fournissent un moyen naturel d’attendre un résultat de manière coopérative.

Chaque type **IAsyncXxx** est projeté dans un type correspondant dans l’espace de noms C++/WinRT **winrt::Windows::Foundation**. Appelons-les **winrt::IAsyncXxx**. Le type de retour d’une coroutine C++/WinRT est un **winrt::IAsyncXxx** ou [**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget). Et plutôt que d’utiliser le mot-clé `return` pour retourner un objet asynchrone qui, une coroutine utilise le mot-clé `co_return` pour retourner la valeur que l’appelant souhaite réellement (peut-être un fichier, un tableau d’octets ou un booléen).

Si une méthode contient au moins une instruction `co_await` (ou au moins `co_return` ou `co_yield`), la méthode est une coroutine pour cette raison.

Pour plus d’informations, consultez [Opérations concurrentes et asynchrones avec C++/WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency).

## <a name="the-direct3d-game-sample-simple3dgamedx"></a>Exemple de jeu Direct3D (**Simple3DGameDX**)

Cette rubrique contient plusieurs procédures pas à pas qui illustrent comment déplacer progressivement un code asynchrone. Pour faire office d’étude de cas, nous allons utiliser la version C++/CX de l’[exemple de jeu Direct3D](/samples/microsoft/windows-universal-samples/simple3dgamedx/) (qui est appelé **Simple3DGameDX**). Nous vous présenterons des exemples sur la façon dont vous pouvez prendre le code source C++/CX d’origine dans ce projet et déplacer progressivement son code asynchrone vers C++/WinRT.

- Téléchargez le fichier ZIP à partir du lien ci-dessus et décompressez-le.
- Ouvrez le projet C++/CX (il se trouve dans le dossier nommé `cpp`) dans Visual Studio.
- Vous devrez ensuite ajouter le support C++/WinRT au projet. Les étapes que vous suivez sont décrites dans [Prise en charge du projet et ajout du support C++/WinRT](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx#taking-a-ccx-project-and-adding-cwinrt-support). L’étape de l’ajout du fichier d’en-tête `interop_helpers.h` à votre projet est particulièrement importante, car nous allons dépendre de ces fonctions d’application auxiliaire dans cette rubrique.
- Enfin, ajoutez `#include <pplawait.h>` à `pch.h`. Cela vous donne un support de coroutine pour la bibliothèque de modèles parallèles (plus d’informations sur ce support dans la section suivante).

Lorsque vous générez, vous obtiendrez des erreurs sur un **octet** ambigu. Comment résoudre cela.

- Ouvrir `BasicLoader.cpp` et commenter `using namespace std;`.
- Dans ce même fichier de code source, vous devez qualifier **shared_ptr** comme **std::shared_ptr**. Vous pouvez le faire avec une recherche et un remplacement dans ce fichier.
- Qualifiez un **vecteur** comme **std::vector** et une **chaîne** comme **std::string**.

Le projet est maintenant de nouveau généré, a un support C++/WinRT et contient les fonctions d’application auxiliaire d’interopérabilité **from_cx** et **to_cx**.

Vous avez maintenant le projet **Simple3DGameDX** prêt à suivre, ainsi que les procédures pas à pas de code de cette rubrique.

## <a name="overview-of-porting-ccx-async-to-cwinrt"></a>Vue d’ensemble du déplacement asynchrone de C++/CX vers C++/WinRT

En résumé, comme nous nous déplaçons, nous allons modifier les chaînes de tâches PPL en appels à `co_await`. Nous allons modifier la valeur de retour d’une méthode à partir d’une tâche PPL par un objet C++/WinRT **IAsyncXxx** ou **IAsyncXxx**\^. Nous allons modifier tout **IAsyncXxx**\^ par un **IAsyncXxx** C++/WinRT.

Vous vous souviendrez qu’une coroutine est une méthode qui appelle `co_xxx`. La coroutine C++/WinRT utilise `co_return` pour retourner sa valeur. Grâce au support de la coroutine pour PPL (fourni par `pplawait.h`), vous pouvez également utiliser `co_return` pour retourner une tâche PPL à partir d’une coroutine. Vous pouvez également `co_await` les tâches et **IAsyncXxx**. Toutefois, vous ne pouvez pas utiliser `co_return` pour retourner un objet **IAsyncXxx**\^. Le tableau ci-dessous décrit le support d’interopérabilité entre les différentes techniques asynchrones.

|Méthode|Peut-il `co_await` ?|Peut-il en `co_return` ?|
|-|-|-|
|La méthode retourne la tâche\<void\>|Oui|Oui|
|La méthode retourne la tâche\<T\>|Non|Oui|
|La méthode retourne IAsyncXxx\^|Oui|Faux. Mais vous incluez **create_async** dans un wrapper autour d’une tâche qui utilise `co_return`.|
|La méthode retourne winrt::IAsyncXxx|Oui|Oui|

Utilisez ce tableau pour accéder directement à une technique d’interopérabilité qui vous intéresse.

|Technique d’interopérabilité asynchrone|Section dans cette rubrique|
|-|-|
|Utilisez `co_await` pour attendre une méthode de **tâche\<void\>** à partir d’une méthode tire et oublie.|[Attendre une **tâche\<void\>** dans une méthode tire et oublie](#await-taskvoid-within-a-fire-and-forget-method)|
|Utilisez `co_await` pour attendre une méthode de **tâche\<void\>** à partir d’une méthode de **tâche\<void\>** .|[Attendre une **tâche\<void\>** dans une méthode de **tâche\<void\>** ](#await-taskvoid-within-a-taskvoid-method)|
|Utilisez `co_await` pour attendre une méthode de **tâche\<void\>** à partir d’une méthode de **tâche\<T\>** .|[Attendre une **tâche\<void\>** dans une méthode de **tâche\<T\>** ](#await-taskvoid-within-a-taskt-method)|
|Utilisez `co_await` pour attendre une méthode **IAsyncXxx**^.|[Attendre une **IAsyncXxx**^ dans une méthode de **tâche**, en laissant le reste du projet inchangé](#await-an-iasyncxxx-in-a-task-method-leaving-the-rest-of-the-project-unchanged)|
|Utilisez `co_return` dans une méthode de **tâche\<void\>** .|[Attendre une **tâche\<void\>** dans une méthode de **tâche\<void\>** ](#await-taskvoid-within-a-taskvoid-method)|
|Utilisez `co_return` dans une méthode de **tâche\<T\>**|[Attendre une **IAsyncXxx**^ dans une méthode de **tâche**, en laissant le reste du projet inchangé](#await-an-iasyncxxx-in-a-task-method-leaving-the-rest-of-the-project-unchanged)|
|Incluez **create_async** dans un wrapper autour d’une tâche qui utilise `co_return`|[Incluez **create_async** dans un wrapper autour d’une tâche qui utilise `co_return`](#wrap-create_async-around-a-task-that-uses-co_return)|
|Déplacer **concurrency::wait**|[Déplacer **concurrency::wait** vers `co_await winrt::resume_after`](#port-concurrencywait-to-co_await-winrtresume_after)|
|Retournez **winrt::IAsyncXxx** au lieu de la **tâche\<void\>** .|[Déplacer un type de retour **tâche\<void\>** vers **winrt::IAsyncXxx**](#port-a-taskvoid-return-type-to-winrtiasyncxxx)|

Et voici un bref exemple de code illustrant une partie du support.

```cppcx
#include <ppltasks.h>
#include <pplawait.h>
#include <winrt/Windows.Foundation.h>

concurrency::task<bool> TaskAsync()
{
    co_return true;
}

Windows::Foundation::IAsyncOperation<bool>^ IAsyncXxxCppCXAsync()
{
    // co_return true; // Error! Can't do that.
    return concurrency::create_async([=]() -> task<bool> {
        co_return true;
    });
}

winrt::Windows::Foundation::IAsyncOperation<bool> IAsyncXxxCppWinRTAsync()
{
    co_return true;
}

concurrency::task<bool> CppCXAsync()
{
    bool b1 = co_await TaskAsync();
    bool b2 = co_await IAsyncXxxCppCXAsync();
    bool b3 = co_await IAsyncXxxCppWinRTAsync();
}

winrt::fire_and_forget CppWinRTAsync()
{
    bool b1 = co_await TaskAsync();
    bool b2 = co_await IAsyncXxxCppCXAsync();
    bool b3 = co_await IAsyncXxxCppWinRTAsync();
}
```

Même avec ces options d’interopérabilité exceptionnelles, le déplacement dépend progressivement du choix des modifications que nous pouvons apporter de manière chirurgicale et qui n’affectent pas le reste du système. Nous voulons éviter les tiraillements à une extrémité libre arbitraire et ainsi annuler l’ensemble du projet. Pour cela, nous devons effectuer des tâches dans un ordre particulier. Examinons quelques exemples de modifications des déplacements asynchrones.

## <a name="await-a-taskvoid-method-leaving-the-rest-of-the-project-unchanged"></a>Attendre une méthode de **tâche\<void\>** , en laissant le reste du projet inchangé

Une méthode qui retourne une **tâche\<void\>** effectue un travail de façon asynchrone, mais ne produit finalement pas de valeur. Nous pouvons `co_await` une méthode comme cela.

C’est pourquoi il est judicieux de commencer à déplacer un code asynchrone progressivement en recherchant des emplacements où appeler ces méthodes. Ces emplacements impliquent la création et/ou le renvoi d’une tâche ; et/ou une chaîne de tâches où aucune valeur n’est passée de chaque tâche à sa continuation. Dans des emplacements comme celui-ci, vous pouvez simplement remplacer le code asynchrone par des instructions `co_await`, comme nous le verrons.

Prenons quelques exemples. Ouvrez le projet **Simple3DGameDX** (consultez [l’exemple de jeu Direct3D](#the-direct3d-game-sample-simple3dgamedx)).

> [!IMPORTANT]
> Dans les exemples qui suivent, à mesure que vous voyez les implémentations de méthodes modifiées, gardez à l’esprit que nous n’avons pas besoin de modifier les *appelants* des méthodes que nous modifions. Ces modifications sont localisées et ne sont pas mises en cascade dans le projet.

### <a name="await-taskvoid-within-a-fire-and-forget-method"></a>Attendre une **tâche\<void\>** dans une méthode tire et oublie

Commençons par attente une **tâche\<void\>** dans des méthodes *tire et oublie*, puisque c’est le cas le plus simple. Il s’agit de méthodes qui fonctionnent de manière asynchrone, mais l’appelant de la méthode n’attend pas que ce travail soit terminé. Il vous suffit d’appeler la méthode et de l’oublier, malgré le fait qu’elle se termine de façon asynchrone.

Recherchez dans la racine du graphique de dépendance de votre projet les méthodes de `void` qui contiennent **create_task** et/ou des chaînes de tâches où seules les méthodes de  **tâche\<void\>** sont appelées.

Dans **Simple3DGameDX**, vous trouverez une correspondance dans l’implémentation de la méthode **GameMain::Update**. Il se trouve dans le fichier de code source `GameMain.cpp`.

#### <a name="gamemainupdate"></a>**GameMain::Update**

Voici un extrait de la version C++/CX de la méthode qui présente les deux parties qui se terminent de manière asynchrone.

```cppcx
void GameMain::Update()
{
    ...
    case UpdateEngineState::WaitingForPress:
        ...
        m_game->LoadLevelAsync().then([this]()
        {
            m_game->FinalizeLoadLevel();
            m_updateState = UpdateEngineState::ResourcesLoaded;
        }, task_continuation_context::use_current());
        ...
    case UpdateEngineState::Dynamics:
        ...
        m_game->LoadLevelAsync().then([this]()
        {
            m_game->FinalizeLoadLevel();
            m_updateState = UpdateEngineState::ResourcesLoaded;
        }, task_continuation_context::use_current());
        ...
    ...
}
```

Vous pouvez voir un appel à la méthode **Simple3DGame::LoadLevelAsync** (qui retourne une **tâche\<void\>** PPL). Une *continuation* qui effectue un travail synchrone suit. **LoadLevelAsync** est asynchrone, mais ne retourne pas de valeur. Donc aucune valeur n’est passée de la tâche à la continuation.

Nous pouvons modifier le code dans ces deux emplacements de la même façon. Le code est expliqué après la liste ci-dessous. Nous pourrions avoir une discussion ici sur la manière sûre d’accéder au pointeur *this* dans une coroutine de membre de classe. Mais revenons pour une section ultérieure ; pour le moment, ce code fonctionne.

```cppcx
winrt::fire_and_forget GameMain::Update()
{
    ...
    case UpdateEngineState::WaitingForPress:
        ...
        co_await m_game->LoadLevelAsync();
        m_game->FinalizeLoadLevel();
        m_updateState = UpdateEngineState::ResourcesLoaded;
        ...
    case UpdateEngineState::Dynamics:
        ...
        co_await m_game->LoadLevelAsync();
        m_game->FinalizeLoadLevel();
        m_updateState = UpdateEngineState::ResourcesLoaded;
        ...
    ...
}
```

Comme vous pouvez le voir, étant donné que **LoadLevelAsync** retourne une tâche, nous pouvons la `co_await`. Et nous n’avons pas besoin d’une continuation explicite&mdash;le code qui suit un `co_await` s’exécute uniquement lorsque **LoadLevelAsync** se termine.

L’introduction de `co_await` transforme la méthode en une coroutine, de sorte qu’elle ne peut pas retourner `void`. Il s’agit d’une méthode tire et oublie, retournons donc [**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget).

Vous devez également modifier le type de retour de **GameMain::Update** de `void` à **winrt::fire_and_forget** dans son instruction dans `GameMain.h`.

Vous pouvez apporter cette modification à votre copie du projet et le jeu est toujours généré et s’exécute de la même façon. Le code source est toujours C++/CX, mais il utilise maintenant les mêmes modèles que C++/WinRT, ce qui nous a un peu plus permis de déplacer la suite du code de manière mécanique.

#### <a name="gamemainresetgame"></a>**GameMain::ResetGame**

**GameMain::ResetGame** est une autre méthode de tire et oublie ; elle appelle également **LoadLevelAsync**. Nous pouvons donc apporter la même modification.

#### <a name="gamemainondevicerestored"></a>**GameMain::OnDeviceRestored**

Les choses sont un peu plus intéressantes dans **GameMain::OnDeviceRestored** en raison de l’imbrication plus profonde du code asynchrone, y compris une tâche non-opérationnelle. Voici un aperçu des parties asynchrones de la méthode (avec le code synchrone le moins intéressant représenté par des ellipses).

```cppcx
void GameMain::OnDeviceRestored()
{
    ...
    create_task([this]()
    {
        return m_renderer->CreateGameDeviceResourcesAsync(m_game);
    }).then([this]()
    {
        ...
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            ...
            return m_game->LoadLevelAsync().then([this]()
            {
                ...
            }, task_continuation_context::use_current());
        }
        else
        {
            return create_task([]()
            {
                // Return a no-op task.
            });
        }
    }, task_continuation_context::use_current()).then([this]()
    {
        ...
    }, task_continuation_context::use_current());
}
```

Tout d’abord, modifiez le type de retour de **GameMain:: OnDeviceRestored** de `void` par **winrt::fire_and_forget** dans `GameMain.h` et `.cpp`. Vous devez également ouvrir `DeviceResources.h` et apporter la même modification au type de retour de **IDeviceNotify:: OnDeviceRestored**.

Pour déplacer le code asynchrone, supprimez tous les appels **create_task** et **then** et leurs accolades et simplifiez la méthode dans une série d’instructions.

Modifiez les `return` (qui renvoient une tâche) en `co_await`. Vous allez laisser un `return` qui ne retourne rien. par conséquent, supprimez-le. Lorsque vous avez terminé, la tâche non opérationnelle a disparu et le contour des parties asynchrones de la méthode ressemblera à ceci. Là encore, le code synchrone le moins intéressant est représenté par des ellipses.

```cppcx
winrt::fire_and_forget GameMain::OnDeviceRestored()
{
    ...
    co_await m_renderer->CreateGameDeviceResourcesAsync(m_game);
    ...
    if (m_updateState == UpdateEngineState::WaitingForResources)
    {
        co_await m_game->LoadLevelAsync();
        ...
    }
    ...
}
```

Comme vous pouvez le voir, il est beaucoup plus simple et plus facile à lire.

#### <a name="gamemaingamemain"></a>**GameMain::GameMain**

Le constructeur **GameMain::GameMain** exécute le travail de façon asynchrone et aucune partie du projet n’attend la fin de ce travail. Là encore, cette liste décrit les parties asynchrones.

```cppcx
GameMain::GameMain(...) : ...
{
    ...
    create_task([this]()
    {
        ...
        return m_renderer->CreateGameDeviceResourcesAsync(m_game);
    }).then([this]()
    {
        ...
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            return m_game->LoadLevelAsync().then([this]()
            {
                ...
            }, task_continuation_context::use_current());
        }
        else
        {
            return create_task([]()
            {
                // Return a no-op task.
            });
        }
    }, task_continuation_context::use_current()).then([this]()
    {
        ....
    }, task_continuation_context::use_current());
}
```

Toutefois, un constructeur ne peut pas retourner **winrt:: fire_and_forget**, nous allons donc déplacer le code asynchrone dans une nouvelle méthode tire et oublie **GameMain::ConstructInBackground**, aplatir le code dans des instructions `co_await` et appeler la nouvelle méthode à partir du constructeur.

```cppcx
GameMain::GameMain(...) : ...
{
    ...
    ConstructInBackground();
}

winrt::fire_and_forget GameMain::ConstructInBackground()
{
    ...
    co_await m_renderer->CreateGameDeviceResourcesAsync(m_game);
    ...
    if (m_updateState == UpdateEngineState::WaitingForResources)
    {
        ...
        co_await m_game->LoadLevelAsync();
        ...
    }
    ...
}
```

Il s’agit de toutes les méthodes tire et oublie&mdash;en fait, l’ensemble du code asynchrone&mdash; dans **GameMain** est converti en coroutines. Si vous pensez que cela s’est bien incliné, vous pourriez peut-être rechercher des méthodes tire et oublie dans d’autres classes et apporter des modifications similaires.

### <a name="the-deferred-discussion-about-co_await-and-the-this-pointer"></a>Discussion différée sur `co_await` et le pointeur *this*

Lorsque nous avons apporté des modifications à **GameMain:: Update**, j’ai différé une discussion sur le pointeur *this*. Voyons ici cette discussion.

Cela s’applique à toutes les méthodes que nous avons modifiées jusqu’à présent et s’applique à *toutes* les coroutines, pas seulement les coroutines tire et oublie. L’introduction d’un `co_await` en une méthode introduit des *points de suspension*. Et pour cette raison, nous devons faire attention au pointeur *this*, que nous utilisons, bien sûr, *après* les points de suspension chaque fois que nous accéderons à un membre de classe.

Finalement, la solution consiste à appeler [**implémente::get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function). Toutefois, pour une présentation complète du problème et de la solution, consultez [Accès en toute sécurité au pointeur *this* dans une coroutine de membre de classe ](/windows/uwp/cpp-and-winrt-apis/weak-references#safely-accessing-the-this-pointer-in-a-class-member-coroutine).

Vous pouvez **implémente::get_strong** uniquement dans une classe qui dérive de [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements).

#### <a name="derive-gamemain-from-winrtimplements"></a>Dériver **GameMain** de **winrt::implements**

La première modification à apporter est dans `GameMain.h`.

```cppcx
class GameMain :
    public DX::IDeviceNotify
```

**GameMain** continuera à implémenter **DX::IDeviceNotify**, mais nous allons le modifier pour qu’il dérive de [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements).

```cppwinrt
class GameMain : 
    public winrt::implements<GameMain, winrt::Windows::Foundation::IInspectable>,
    DX::IDeviceNotify
```

Ensuite, dans `App.cpp`, vous trouverez cette méthode.

```cppcx
void App::Load(Platform::String^)
{
    if (!m_main)
    {
        m_main = std::unique_ptr<GameMain>(new GameMain(m_deviceResources));
    }
}
```

Mais maintenant que **GameMain** dérive de **winrt::implements**, nous devons le construire d’une manière différente. Dans ce cas, nous utiliserons le modèle de fonction [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self). Pour plus d’informations, consultez [Instanciation et retour des types et interfaces d’implémentation](/windows/uwp/cpp-and-winrt-apis/author-apis#instantiating-and-returning-implementation-types-and-interfaces).

```cppwinrt
    ...
    m_main = winrt::make_self<GameMain>(m_deviceResources);
    ...
```

Pour fermer la boucle sur cette modification, vous devez également modifier le type de *m_main*. Dans `App.h`, vous trouverez ce code.

```cppcx
ref class App sealed :
    public Windows::ApplicationModel::Core::IFrameworkView
{
    ...
private:
    ...
    std::unique_ptr<GameMain> m_main;
};
```

Remplacez la déclaration de *m_main* par cela.

```cppwinrt
    ...
    winrt::com_ptr<GameMain> m_main;
    ...
```

#### <a name="we-can-now-call-implementsget_strong"></a>Nous pouvons maintenant appeler **Implements::get_strong**

Pour **GameMain::Update** et, pour toutes les autres méthodes, nous avons ajouté un `co_await` à, voici comment vous pouvez appeler **get_strong** au début d’une coroutine pour vous assurer qu’une référence forte subsiste jusqu’à ce que la coroutine se termine.

```cppcx
winrt::fire_and_forget GameMain::Update()
{
    auto strong_this{ get_strong() }; // Keep *this* alive.
    ...
        co_await ...
    ...
}
```

### <a name="await-taskvoid-within-a-taskvoid-method"></a>Attendre la **tâche\<void\>** dans une méthode de **tâche\<void\>**

Le cas le plus simple suivant est d’attendre la **tâche\<void\>** dans une méthode qui retourne elle-même **tâche\<void\>** , car nous pouvons `co_await` une **tâche\<void\>** et nous pouvons `co_return` à partir d’une tâche.

Vous trouverez un exemple très simple dans l’implémentation de la méthode **Simple3DGame::LoadLevelAsync**. Il se trouve dans le fichier de code source `Simple3DGame.cpp`.

```cppcx
task<void> Simple3DGame::LoadLevelAsync()
{
    m_level[m_currentLevel]->Initialize(m_objects);
    m_levelDuration = m_level[m_currentLevel]->TimeLimit() + m_levelBonusTime;
    return m_renderer->LoadLevelResourcesAsync();
}
```

Il y a simplement du code synchrone, suivi par le retour de la tâche créée par **GameRenderer::LoadLevelResourcesAsync**.

Au lieu de retourner cette tâche, nous la `co_await`, puis `co_return` le `void` résultant.

```cppcx
task<void> Simple3DGame::LoadLevelAsync()
{
    m_level[m_currentLevel]->Initialize(m_objects);
    m_levelDuration = m_level[m_currentLevel]->TimeLimit() + m_levelBonusTime;
    co_return co_await m_renderer->LoadLevelResourcesAsync();
}
```

### <a name="await-taskvoid-within-a-taskt-method"></a>Attendre la **tâche\<void\>** dans une méthode de **tâche\<T\>**

Bien qu’il ne soit pas possible de trouver des exemples appropriés dans **Simple3DGameDX**, nous pouvons combiner un exemple hypothétique simplement pour illustrer le modèle.

L’exemple de code ci-dessous illustre simplement la `co_await` simple d’une **tâche\<void\>** . Ensuite, afin de satisfaire le type de retour de la **tâche\<T\>** , nous renvoyons de manière asynchrone un **StorageFile\^** en co_awaiting une méthode Windows Runtime et `co_return` le fichier résultant.

```cppcx
task<StorageFile^> Simple3DGame::LoadLevelAndRetrieveFileAsync(
    StorageFolder^ location,
    Platform::String^ filename)
{
    co_await m_renderer->LoadLevelResourcesAsync();
    co_return co_await location->GetFileAsync(filename);
}
```

L’étape suivante serait le déplacement de la méthode vers C++/WinRT comme suit.

```cppcx
winrt::Windows::Foundation::IAsyncOperation<winrt::Windows::Storage::StorageFile>
Simple3DGame::LoadLevelAndRetrieveFileAsync(
    StorageFolder location,
    std::wstring filename)
{
    co_await m_renderer->LoadLevelResourcesAsync();
    co_return co_await location.GetFileAsync(filename);
}
```

Le membre de données *m_renderer* est toujours C++/CX dans cet exemple.

## <a name="await-an-iasyncxxx-in-a-task-method-leaving-the-rest-of-the-project-unchanged"></a>Attendre une **IAsyncXxx**^ dans une méthode de **tâche**, en laissant le reste du projet inchangé

Nous avons vu comment vous pouvez `co_await` la **tâche\<void\>** . Vous pouvez également `co_await` une méthode qui retourne **IAsyncXxx**, qu’il s’agisse d’une méthode de votre projet ou d’une API Windows asynchrone (par exemple, [**StorageFolder. GetFileAsync**](/uwp/api/windows.storage.storagefolder.getfileasync)).

Pour obtenir un exemple de la façon dont nous pouvons modifier ce code, observons **BasicReaderWriter::ReadDataAsync** (vous le trouverez implémenté dans `BasicReaderWriter.cpp`). Voici la version C++/CX d’origine.

```cppcx
task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename
    )
{
    return task<StorageFile^>(m_location->GetFileAsync(filename)).then([=](StorageFile^ file)
    {
        return FileIO::ReadBufferAsync(file);
    }).then([=](IBuffer^ buffer)
    {
        auto fileData = ref new Platform::Array<byte>(buffer->Length);
        DataReader::FromBuffer(buffer)->ReadBytes(fileData);
        return fileData;
    });
}
```

Non seulement nous pouvons `co_await` les API Windows, mais nous pouvons également `co_return` la valeur renvoyée par la méthode de manière asynchrone (dans ce cas, un tableau d’octets). Cette première étape montre comment apporter simplement ces modifications. Nous allons en fait déplacer le code C++/CX vers C++/WinRT dans la section suivante.

```cppcx
task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename
)
{
    StorageFile^ file = co_await m_location->GetFileAsync(filename);
    IBuffer^ buffer = co_await FileIO::ReadBufferAsync(file);
    auto fileData = ref new Platform::Array<byte>(buffer->Length);
    DataReader::FromBuffer(buffer)->ReadBytes(fileData);
    co_return fileData;
}
```

Là encore, nous n’avons pas besoin de modifier les *appelants* des méthodes que nous modifions, car nous n’avons pas modifié le type de retour.

### <a name="port-readdataasync-mostly-to-cwinrt-leaving-the-rest-of-the-project-unchanged"></a>Déplacer **ReadDataAsync** (principalement) vers C++/WinRT, en laissant le reste du projet inchangé

Nous pouvons aller plus loin et déplacer la méthode *presque entièrement* vers C++/WinRT sans avoir à modifier une autre partie du projet.

La seule dépendance de cette méthode sur le reste du projet est le membre de données **BasicReaderWriter::m_location**, qui est un **StorageFolder**^ C++/CX. Pour que ce membre de données reste inchangé et pour ne pas modifier le type de paramètre et le type de retour, nous devons uniquement effectuer quelques conversions&mdash;une au début de la méthode et une à la fin. Pour cela, nous pouvons utiliser les fonctions d’application auxiliaire d’interopérabilité **from_cx** et **to_cx**.

Voici comment **BasicReaderWriter::ReadDataAsync** surveille le déplacement de son implémentation de manière prépondérante vers C++/WinRT. Il s’agit d’un bon exemple de *déplacement progressif*. Cette méthode se trouve à l’étape où nous pouvons ne plus la considérer comme *une méthode C++/CX qui utilise certaines techniques C++/WinRT* et la voir comme *une méthode C++/WinRT qui interagit avec C++/CX*.

```cppwinrt
#include <winrt/Windows.Storage.h>
#include <winrt/Windows.Storage.Streams.h>
#include <robuffer.h>
...
task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename)
{
    auto location_from_cx = from_cx<winrt::Windows::Storage::StorageFolder>(m_location);

    auto file = co_await location_from_cx.GetFileAsync(filename->Data());
    auto buffer = co_await winrt::Windows::Storage::FileIO::ReadBufferAsync(file);
    byte* bytes;
    auto byteAccess = buffer.as<Windows::Storage::Streams::IBufferByteAccess>();
    winrt::check_hresult(byteAccess->Buffer(&bytes));

    co_return ref new Platform::Array<byte>(bytes, buffer.Length());
}
```

> [!NOTE]
> Dans **ReadDataAsync**, nous avons construit et retourné un nouveau tableau C++/CX. Et bien sûr, nous avons fait cela pour satisfaire le type de retour de la méthode (afin de ne pas avoir à modifier le reste du projet).
>
> Vous pouvez rencontrer d’autres exemples dans votre propre projet où, après le déplacement, vous atteignez la fin de la méthode et tout ce que vous avez est un objet C++/WinRT. Pour `co_return` cela, appelez simplement **to_cx** pour le convertir. Voici un exemple hypothétique.

```cppcx
task<Windows::Storage::StorageFile^> RetrieveFileAsync()
{
    winrt::Windows::Storage::StorageFile storageFile = co_await ...
    co_return to_cx<Windows::Storage::StorageFile>(storageFile);
}
```

## <a name="wrap-create_async-around-a-task-that-uses-co_return"></a>Incluez **create_async** dans un wrapper autour d’une tâche qui utilise `co_return`

Vous ne pouvez pas `co_return` un **IAsyncXxx**\^ directement, mais vous pouvez obtenir un résultat similaire. Si vous avez une tâche qui utilise `co_return`, vous pouvez inclure [**concurrency::create_async** ](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_async) dans un wrapper autour de cela.

Voici un exemple hypothétique, car il n’existe pas d’exemple que nous pouvons lever à partir de **Simple3DGameDX**.

```cppcx
Windows::Foundation::IAsyncOperation<bool>^ Simple3DGame::ReturnBooleanAsync()
{
    return concurrency::create_async(
        [this]() -> concurrency::task<bool> {
            bool result = co_await BooleanMemberFunctionAsync();
            co_return result;
        });
}
```

Comme vous pouvez le voir, vous pouvez obtenir la valeur de retour à partir de n’importe quelle méthode que vous pouvez `co_await`.

## <a name="port-concurrencywait-to-co_await-winrtresume_after"></a>Déplacer **concurrency::wait** vers `co_await winrt::resume_after`

Il existe plusieurs emplacements où **Simple3DGameDX** utilise [**concurrency::wait**](/cpp/parallel/concrt/reference/concurrency-namespace-functions#wait) pour suspendre le thread pendant un court laps de temps. Voici un exemple.

```cppcx
// GameConstants.h
namespace GameConstants
{
    ...
    static const int InitialLoadingDelay = 2000;
    ...
}

// GameRenderer.cpp
task<void> GameRenderer::CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game)
{
    std::vector<task<void>> tasks;
    ...
    tasks.push_back(create_task([]()
    {
        wait(GameConstants::InitialLoadingDelay);
    }));
    ...
}
```

La version C++/WinRT de **concurrency::wait** et la structure **winrt::resume_after**. Nous pouvons `co_await` cette structure à l’intérieur d’une tâche PPL. Voici un exemple de code :

```
// GameConstants.h
namespace GameConstants
{
    using namespace std::literals::chrono_literals;
    ...
    static const auto InitialLoadingDelay = 2000ms;
    ...
}

// GameRenderer.cpp
task<void> GameRenderer::CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game)
{
    std::vector<task<void>> tasks;
    ...
    tasks.push_back(create_task([]() -> task<void>
    {
        co_await winrt::resume_after(GameConstants::InitialLoadingDelay);
    }));
    ...
}
```

Notez les deux autres modifications que nous devions effectuer. Nous avons modifié le type de **GameConstants::InitialLoadingDelay** en **std::chrono::duration** et nous avons rendu le type de retour de la fonction lambda explicite, car le compilateur n’est plus en mesure de le déduire.

## <a name="port-a-taskvoid-return-type-to-winrtiasyncxxx"></a>Déplacer un type de retour de **tâche\<void\>** vers **winrt::IAsyncXxx**

### <a name="simple3dgameloadlevelasync"></a>**Simple3DGame::LoadLevelAsync**

À ce niveau de notre travail avec **Simple3DGameDX**, tous les emplacements du projet qui appellent **Simple3DGame::LoadLevelAsync** utilisent `co_await` pour l’appeler.

Cela signifie que nous pouvons modifier le type de retour de cette méthode à partir de la **tâche\<void\>** par **winrt::Windows::Foundation::IAsyncAction**.

```cppcx
winrt::Windows::Foundation::IAsyncAction Simple3DGame::LoadLevelAsync()
{
    m_level[m_currentLevel]->Initialize(m_objects);
    m_levelDuration = m_level[m_currentLevel]->TimeLimit() + m_levelBonusTime;
    co_return co_await m_renderer->LoadLevelResourcesAsync();
}
```

À présent, le déplacement du reste de la méthode et de ses dépendances vers C++/WinRT doit se faire de façon assez mécanique.

### <a name="gamerendererloadlevelresourcesasync"></a>**GameRenderer::LoadLevelResourcesAsync**

Voici la version C++/CX d’origine de **GameRenderer::LoadLevelResourcesAsync**.

```cppcx
// GameConstants.h
namespace GameConstants
{
    ...
    static const int LevelLoadingDelay = 500;
    ...
}

// GameRenderer.cpp
task<void> GameRenderer::LoadLevelResourcesAsync()
{
    m_levelResourcesLoaded = false;

    return create_task([this]()
    {
        wait(GameConstants::LevelLoadingDelay);
    });
}
```

**Simple3DGame::LoadLevelAsync** est le seul emplacement du projet qui appelle **GameRenderer::LoadLevelResourcesAsync** et il utilise déjà `co_await` pour l’appeler.

Par conséquent, il n’est plus nécessaire pour **GameRenderer::LoadLevelResourcesAsync** de retourner une tâche&mdash;il peut retourner un **winrt::Windows::Foundation::IAsyncAction** à la place. Et l’implémentation elle-même est assez simple pour déplacer complètement vers C++/WinRT. Cela implique d’apporter la même modification que celle que nous avons apportée dans [Déplacer **concurrency::wait** vers `co_await winrt::resume_after`](#port-concurrencywait-to-co_await-winrtresume_after). Et il n’y a aucune dépendance significative sur le reste du projet pour laquelle s’inquiéter.

Voici à quoi ressemble la méthode après le déplacement intégrale vers C++/WinRT.

```cppwinrt
// GameConstants.h
namespace GameConstants
{
    using namespace std::literals::chrono_literals;
    ...
    static const auto LevelLoadingDelay = 500ms;
    ...
}

// GameRenderer.cpp
winrt::Windows::Foundation::IAsyncAction GameRenderer::LoadLevelResourcesAsync()
{
    m_levelResourcesLoaded = false;
    co_return co_await winrt::resume_after(GameConstants::LevelLoadingDelay);
}
```

### <a name="basicreaderwriterreaddataasync"></a>**BasicReaderWriter::ReadDataAsync**

Nous allons terminer cette procédure pas à pas en effectuant un déplacement complet de **BasicReaderWriter::ReadDataAsync** vers C++/WinRT. La dernière fois que nous l’avons consulté (dans [Déplacer **ReadDataAsync** (principalement) vers C++/WinRT, en laissant le reste du projet inchangé](#port-readdataasync-mostly-to-cwinrt-leaving-the-rest-of-the-project-unchanged)]), il a été essentiellement déplacé vers C++/WinRT. Toutefois, il retournait toujours une tâche de **Platform::Array\<byte\>** ^.

```cppwinrt
task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename)
{
    auto location_from_cx = from_cx<winrt::Windows::Storage::StorageFolder>(m_location);

    auto file = co_await location_from_cx.GetFileAsync(filename->Data());
    auto buffer = co_await winrt::Windows::Storage::FileIO::ReadBufferAsync(file);
    byte* bytes;
    auto byteAccess = buffer.as<Windows::Storage::Streams::IBufferByteAccess>();
    winrt::check_hresult(byteAccess->Buffer(&bytes));

    co_return ref new Platform::Array<byte>(bytes, buffer.Length());
}
```

Au lieu de retourner une tâche, nous allons la modifier pour retourner de manière asynchrone un objet [**IBuffer**](/uwp/api/windows.storage.streams.ibuffer) C++/WinRT, même si cela signifie modifier le code sur les sites d’appel.

Voici à quoi ressemble la méthode après le déplacement de son implémentation, de son paramètre et du membre de données *m_location* pour utiliser la syntaxe et les objets C++/WinRT.

```cppwinrt
winrt::Windows::Foundation::IAsyncOperation<winrt::Windows::Storage::Streams::IBuffer>
BasicReaderWriter::ReadDataAsync(
    _In_ winrt::hstring const& filename)
{
    StorageFile file{ co_await m_location.GetFileAsync(filename) };
    co_return co_await FileIO::ReadBufferAsync(file);
}

winrt::array_view<byte> BasicLoader::GetBufferView(
    winrt::Windows::Storage::Streams::IBuffer const& buffer)
{
    byte* bytes;
    auto byteAccess = buffer.as<Windows::Storage::Streams::IBufferByteAccess>();
    winrt::check_hresult(byteAccess->Buffer(&bytes));
    return { bytes, bytes + buffer.Length() };
}
```

Comme vous pouvez le voir, **BasicReaderWriter::ReadDataAsync** lui-même est bien plus simple, car nous avons pris en compte dans sa propre méthode la logique synchrone qui récupère les octets de la mémoire tampon.

Mais maintenant, nous devons déplacer les sites d’appel à partir de ce type de structure dans C++/CX.

```cppcx
task<void> BasicLoader::LoadTextureAsync(...)
{
    return m_basicReaderWriter->ReadDataAsync(filename).then(
        [=](const Platform::Array<byte>^ textureData)
    {
        CreateTexture(...);
    });
}
```

Pour ce modèle dans C++/WinRT.

```cppwinrt
winrt::Windows::Foundation::IAsyncAction BasicLoader::LoadTextureAsync(...)
{
    auto textureBuffer = co_await m_basicReaderWriter.ReadDataAsync(filename);
    auto textureData = GetBufferView(textureBuffer);
    CreateTexture(...);
}
```

## <a name="important-apis"></a>API importantes

* [IAsyncAction](/uwp/api/windows.foundation.iasyncaction),
* [IAsyncActionWithProgress&lt;TProgress&gt;](/uwp/api/windows.foundation.iasyncactionwithprogress-1),
* [IAsyncOperation&lt;TResult&gt;](/uwp/api/windows.foundation.iasyncoperation-1) et
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](/uwp/api/windows.foundation.iasyncoperationwithprogress-2).
* [implements::get_strong](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function)
* [concurrency::create_async](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_async)
* [concurrency::create_task](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_task)
* [concurrency::task](/cpp/parallel/concrt/reference/task-class)
* [concurrency::task::then](/cpp/parallel/concrt/reference/task-class#then)
* [concurrency::wait](/cpp/parallel/concrt/reference/concurrency-namespace-functions#wait)
* [winrt::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)
* [winrt::make_self](/uwp/cpp-ref-for-winrt/make-self)

## <a name="related-topics"></a>Rubriques connexes

* [Passer de C++/CX à C++/WinRT](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx)
* [Interopérabilité entre C++/WinRT et C++/CX](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx)
* [Opérations concurrentes et asynchrones avec C++/WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency)
* [Références fortes et faibles en C++/WinRT](/windows/uwp/cpp-and-winrt-apis/weak-references)
* [Créer des API avec C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis)
