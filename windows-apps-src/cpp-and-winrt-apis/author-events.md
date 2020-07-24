---
description: Cette rubrique montre comment créer un composant Windows Runtime contenant une classe runtime qui déclenche des événements. Elle montre également une application qui utilise le composant et gère les événements.
title: Créer des événements en C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, créer, événement
ms.localizationpriority: medium
ms.openlocfilehash: 980f39f20de369bce226c4d8c1070bda851480c2
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493654"
---
# <a name="author-events-in-cwinrt"></a>Créer des événements en C++/WinRT

Cette rubrique vient compléter les explications fournies sur le composant Windows Runtime et l’application consommatrice, dont la génération vous est expliquée dans la rubrique [Composants Windows Runtime avec C++/WinRT](/windows/uwp/winrt-components/create-a-windows-runtime-component-in-cppwinrt).

Voici les nouvelles fonctionnalités traitées dans cette rubrique.
- Mise à jour de la classe runtime de compte bancaire pour déclencher un événement quand son solde est débiteur
- Mise à jour de l’application Core qui consomme la classe runtime de compte bancaire pour qu’elle gère cet événement.

> [!NOTE]
> Pour plus d’informations sur l’installation et l’utilisation de l’extension VSIX (Visual Studio Extension) [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) et du package NuGet (qui fournissent ensemble la prise en charge des modèles et des builds de projet), consultez [Prise en charge de Visual Studio pour C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

> [!IMPORTANT]
> Pour obtenir les principaux concepts et termes facilitant votre compréhension pour utiliser et créer des classes runtime avec C++/WinRT, voir [Utiliser des API avec C++/WinRT](consume-apis.md) et [Créer des API avec C++/WinRT](author-apis.md).

## <a name="create-bankaccountwrc-and-bankaccountcoreapp"></a>Créer **BankAccountWRC** et **BankAccountCoreApp**

Si vous souhaitez procéder aux mises à jour présentées dans cette rubrique pour pouvoir générer et exécuter le code, la première étape consiste à suivre la procédure pas à pas de la rubrique [Composants Windows Runtime avec C++/WinRT](/windows/uwp/winrt-components/create-a-windows-runtime-component-in-cppwinrt). De cette façon, vous disposerez du composant Windows Runtime **BankAccountWRC** et de l’application Core **BankAccountCoreApp** qui le consomme.

## <a name="update-bankaccountwrc-to-raise-an-event"></a>Mettre à jour **BankAccountWRC** pour déclencher un événement

Mettez à jour `BankAccount.idl` de sorte qu’il se présente comme dans le listing ci-dessous. Voici comment déclarer un événement dont le type délégué est [**EventHandler**](/uwp/api/windows.foundation.eventhandler-1) avec, comme argument, un nombre à virgule flottante simple précision.

```idl
// BankAccountWRC.idl
namespace BankAccountWRC
{
    runtimeclass BankAccount
    {
        BankAccount();
        void AdjustBalance(Single value);
        event Windows.Foundation.EventHandler<Single> AccountIsInDebit;
    };
}
```

Enregistrez le fichier. Dans son état actuel, le projet ne sera pas généré complètement. Effectuez cependant une génération dès à présent pour générer des versions mises à jour des fichiers stub `\BankAccountWRC\BankAccountWRC\Generated Files\sources\BankAccount.h` et `BankAccount.cpp`. Vous pouvez maintenant voir dans ces fichiers les implémentations stub de l’événement **AccountIsInDebit**. Dans C++/WinRT, un événement déclaré dans le fichier IDL est implémenté comme un ensemble de fonctions surchargées (de la même manière qu'une propriété est implémentée comme une paire de fonctions Get et Set surchargées). Une surcharge prend un délégué à inscrire et retourne un jeton ([**winrt::event_token**](/uwp/cpp-ref-for-winrt/event-token)). L’autre prend un jeton et révoque l’inscription du délégué associé.

Ouvrez à présent `BankAccount.h` et `BankAccount.cpp`, et mettez à jour l’implémentation de la classe runtime **BankAccount**. Dans `BankAccount.h`, ajoutez les deux fonctions **AccountIsInDebit** surchargées ainsi qu’un membre de donnée d’événement privé à utiliser dans l’implémentation de ces fonctions.

```cppwinrt
// BankAccount.h
...
namespace winrt::BankAccountWRC::implementation
{
    struct BankAccount : BankAccountT<BankAccount>
    {
        ...
        winrt::event_token AccountIsInDebit(Windows::Foundation::EventHandler<float> const& handler);
        void AccountIsInDebit(winrt::event_token const& token) noexcept;

    private:
        winrt::event<Windows::Foundation::EventHandler<float>> m_accountIsInDebitEvent;
        ...
    };
}
...
```

Comme vous pouvez le voir ci-dessus, un événement est représenté par le modèle struct [**winrt::event**](/uwp/cpp-ref-for-winrt/event), paramétré par un type délégué particulier (qui peut lui-même être paramétré par un type args).

Dans `BankAccount.cpp`, implémentez les deux fonctions **AccountIsInDebit** surchargées.

```cppwinrt
// BankAccount.cpp
...
namespace winrt::BankAccountWRC::implementation
{
    winrt::event_token BankAccount::AccountIsInDebit(Windows::Foundation::EventHandler<float> const& handler)
    {
        return m_accountIsInDebitEvent.add(handler);
    }

    void BankAccount::AccountIsInDebit(winrt::event_token const& token) noexcept
    {
        m_accountIsInDebitEvent.remove(token);
    }

    void BankAccount::AdjustBalance(float value)
    {
        m_balance += value;
        if (m_balance < 0.f) m_accountIsInDebitEvent(*this, m_balance);
    }
}
```

> [!NOTE]
> Pour savoir ce qu’est un revoker d’événement automatique, consultez [Révoquer un délégué inscrit](handle-events.md#revoke-a-registered-delegate). L’implémentation d’un revoker d’événement automatique est gratuite pour votre événement. En d’autres termes, vous n’avez pas besoin d’implémenter la surcharge pour le revoker d’événement puisque l’implémentation vous est fournie par la projection C++/WinRT.

Les autres surcharges (inscription et révocation manuelle) *ne sont pas* intégrées à la projection. Vous pouvez donc les implémenter de manière optimale pour votre scénario. Le fait d’appeler [**event::add**](/uwp/cpp-ref-for-winrt/event#eventadd-function) et [**event::remove**](/uwp/cpp-ref-for-winrt/event#eventremove-function) comme indiqué dans ces implémentations constitue une valeur par défaut efficace et concurrency/thread-safe. Mais si vous avez un très grand nombre d’événements, vous pouvez ne pas souhaiter un champ d’événement pour chacun, mais plutôt opter pour un type d’implémentation fragmentée à la place.

Vous pouvez également voir ci-dessus que l’implémentation de la fonction **AdjustBalance** a été mise à jour pour déclencher l’événement **AccountIsInDebit** si le solde devient négatif.

## <a name="update-bankaccountcoreapp-to-handle-the-event"></a>Mettre à jour **BankAccountCoreApp** pour gérer l’événement

Dans le projet **BankAccountCoreApp**, dans `App.cpp`, apportez les modifications suivantes au code pour inscrire un gestionnaire d’événements, puis faites en sorte que le compte passe en débit.

`WINRT_ASSERT` est une définition de macro, qui se développe en [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros).

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    winrt::event_token m_eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        m_eventToken = m_bankAccount.AccountIsInDebit([](const auto &, float balance)
        {
            WINRT_ASSERT(balance < 0.f); // Put a breakpoint here.
        });
    }
    ...

    void Uninitialize()
    {
        m_bankAccount.AccountIsInDebit(m_eventToken);
    }
    ...
    
    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_bankAccount.AdjustBalance(-1.f);
        ...
    }
    ...
};
```

Tenez compte de la modification de la méthode **OnPointerPressed**. Désormais, chaque fois que vous cliquez sur la fenêtre, vous *retranchez* 1 du solde du compte bancaire. À présent, l’application gère l’événement qui est déclenché quand le solde devient négatif. Pour démontrer que l’événement est déclenché comme prévu, insérez un point d’arrêt à l’intérieur de l’expression lambda qui gère l’événement **AccountIsInDebit**, exécutez l’application, puis cliquez à l’intérieur de la fenêtre.

## <a name="parameterized-delegates-across-an-abi"></a>Délégués paramétrables dans une interface ABI

Si votre événement doit être accessible via une interface binaire d'application (ABI)&mdash;par exemple entre un composant et son application consommatrice&mdash;votre événement doit utiliser un type de délégué Windows Runtime. L’exemple ci-dessus utilise le type de délégué Windows Runtime [**Windows::Foundation::EventHandler\<T\>** ](/uwp/api/windows.foundation.eventhandler). [**TypedEventHandler\<TSender, TResult\>** ](/uwp/api/windows.foundation.eventhandler) est un autre exemple de type de délégué Windows Runtime.

Comme les paramètres de type pour ces deux types de délégués doivent traverser l'ABI, ils doivent donc aussi être de type Windows Runtime. Cela inclut les classes runtime Windows et tierces ainsi que les types primitifs tels que les nombres et les chaînes. Le compilateur vous aide en affichant une erreur « *must be WinRT type* » (doit être de type WinRT) si vous oubliez cette contrainte.

Vous trouverez ci-dessous un exemple sous forme de listes de code. Commencez par les projets **BankAccountWRC** et **BankAccountCoreApp** que vous avez créés précédemment dans cette rubrique, puis modifiez le code de ces projets en reproduisant le code figurant dans ces listes.

La première liste concerne le projet **BankAccountWRC**. Après avoir modifié `BankAccountWRC.idl` comme indiqué ci-dessous, générez le projet, puis copiez `MyEventArgs.h` et `.cpp` dans le projet (à partir du dossier `Generated Files`) comme vous l’avez fait précédemment avec `BankAccount.h` et `.cpp`.

```cppwinrt
// BankAccountWRC.idl
namespace BankAccountWRC
{
    [default_interface]
    runtimeclass MyEventArgs
    {
        Single Balance{ get; };
    }

    [default_interface]
    runtimeclass BankAccount
    {
        ...
        event Windows.Foundation.EventHandler<BankAccountWRC.MyEventArgs> AccountIsInDebit;
        ...
    };
}

// MyEventArgs.h
#pragma once
#include "MyEventArgs.g.h"

namespace winrt::BankAccountWRC::implementation
{
    struct MyEventArgs : MyEventArgsT<MyEventArgs>
    {
        MyEventArgs() = default;
        MyEventArgs(float balance);
        float Balance();

    private:
        float m_balance{ 0.f };
    };
}

// MyEventArgs.cpp
#include "pch.h"
#include "MyEventArgs.h"
#include "MyEventArgs.g.cpp"

namespace winrt::BankAccountWRC::implementation
{
    MyEventArgs::MyEventArgs(float balance) : m_balance(balance)
    {
    }

    float MyEventArgs::Balance()
    {
        return m_balance;
    }
}

// BankAccount.h
...
struct BankAccount : BankAccountT<BankAccount>
{
...
    winrt::event_token AccountIsInDebit(Windows::Foundation::EventHandler<BankAccountWRC::MyEventArgs> const& handler);
...
private:
    winrt::event<Windows::Foundation::EventHandler<BankAccountWRC::MyEventArgs>> m_accountIsInDebitEvent;
...
}
...

// BankAccount.cpp
#include "MyEventArgs.h"
...
winrt::event_token BankAccount::AccountIsInDebit(Windows::Foundation::EventHandler<BankAccountWRC::MyEventArgs> const& handler) { ... }
...
void BankAccount::AdjustBalance(float value)
{
    m_balance += value;

    if (m_balance < 0.f)
    {
        auto args = winrt::make_self<winrt::BankAccountWRC::implementation::MyEventArgs>(m_balance);
        m_accountIsInDebitEvent(*this, *args);
    }
}
...
```

Cette liste concerne le projet **BankAccountCoreApp**.

```cppwinrt
// App.cpp
...
void Initialize(CoreApplicationView const&)
{
    m_eventToken = m_bankAccount.AccountIsInDebit([](const auto&, BankAccountWRC::MyEventArgs args)
    {
        float balance = args.Balance();
        WINRT_ASSERT(balance < 0.f); // Put a breakpoint here.
    });
}
...
```

## <a name="simple-signals-across-an-abi"></a>Signaux simples dans une interface ABI

Si vous n'avez pas besoin de passer des paramètres ou des arguments avec votre événement, vous pouvez définir votre propre type de délégué Windows Runtime simple. L’exemple ci-dessous montre une version plus simple de la classe runtime **BankAccount**. Il déclare un type de délégué nommé **SignalDelegate** puis l'utilise pour déclencher un événement de type signal au lieu d'un événement avec un paramètre.

```idl
// BankAccountWRC.idl
namespace BankAccountWRC
{
    delegate void SignalDelegate();

    runtimeclass BankAccount
    {
        BankAccount();
        event BankAccountWRC.SignalDelegate SignalAccountIsInDebit;
        void AdjustBalance(Single value);
    };
}
```

```cppwinrt
// BankAccount.h
...
namespace winrt::BankAccountWRC::implementation
{
    struct BankAccount : BankAccountT<BankAccount>
    {
        ...

        winrt::event_token SignalAccountIsInDebit(BankAccountWRC::SignalDelegate const& handler);
        void SignalAccountIsInDebit(winrt::event_token const& token);
        void AdjustBalance(float value);

    private:
        winrt::event<BankAccountWRC::SignalDelegate> m_signal;
        float m_balance{ 0.f };
    };
}
```

```cppwinrt
// BankAccount.cpp
...
namespace winrt::BankAccountWRC::implementation
{
    winrt::event_token BankAccount::SignalAccountIsInDebit(BankAccountWRC::SignalDelegate const& handler)
    {
        return m_signal.add(handler);
    }

    void BankAccount::SignalAccountIsInDebit(winrt::event_token const& token)
    {
        m_signal.remove(token);
    }

    void BankAccount::AdjustBalance(float value)
    {
        m_balance += value;
        if (m_balance < 0.f)
        {
            m_signal();
        }
    }
}
```

```cppwinrt
// App.cpp
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount m_bankAccount;
    winrt::event_token m_eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        m_eventToken = m_bankAccount.SignalAccountIsInDebit([] { /* ... */ });
    }
    ...

    void Uninitialize()
    {
        m_bankAccount.SignalAccountIsInDebit(m_eventToken);
    }
    ...

    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_bankAccount.AdjustBalance(-1.f);
        ...
    }
    ...
};
```

## <a name="parameterized-delegates-simple-signals-and-callbacks-within-a-project"></a>Délégués paramétrés, signaux simples et rappels au sein d'un projet

Si vous avez besoin d’événements internes à votre projet Visual Studio (pas entre binaires), où ces événements ne sont pas limités aux types Windows Runtime, vous pouvez toujours utiliser le modèle de classe [**winrt::event**](/uwp/cpp-ref-for-winrt/event)\<Delegate\>. Utilisez simplement [**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate) au lieu d’un type délégué Windows Runtime réel, car **winrt::delegate** prend également en charge les paramètres non-Windows Runtime.

L'exemple ci-dessous montre d'abord une signature de délégué qui ne prend aucun paramètre (essentiellement un simple signal), puis une signature qui prend une chaîne.

```cppwinrt
winrt::event<winrt::delegate<>> signal;
signal.add([] { std::wcout << L"Hello, "; });
signal.add([] { std::wcout << L"World!" << std::endl; });
signal();

winrt::event<winrt::delegate<std::wstring>> log;
log.add([](std::wstring const& message) { std::wcout << message.c_str() << std::endl; });
log.add([](std::wstring const& message) { Persist(message); });
log(L"Hello, World!");
```

Notez comment vous pouvez ajouter à l'événement autant de délégués que vous le souhaitez. Mais un événement implique une surcharge de travail. Si vous n'avez besoin que d'un simple rappel avec un seul délégué abonné, alors vous pouvez utiliser [**winrt::delegate&lt;... T&gt;** ](/uwp/cpp-ref-for-winrt/delegate) seul.

```cppwinrt
winrt::delegate<> signalCallback;
signalCallback = [] { std::wcout << L"Hello, World!" << std::endl; };
signalCallback();

winrt::delegate<std::wstring> logCallback;
logCallback = [](std::wstring const& message) { std::wcout << message.c_str() << std::endl; }f;
logCallback(L"Hello, World!");
```

Si vous effectuez un portage à partir d'un code base C++/CX où les événements et les délégués sont utilisés en interne au sein d’un projet, **winrt::delegate** vous aidera à répliquer ce modèle en C++/WinRT.

## <a name="design-guidelines"></a>Recommandations en matière de conception

Nous vous recommandons de passer les événements, et non les délégués, comme paramètres de fonction. La fonction **add** de [**winrt::event**](/uwp/cpp-ref-for-winrt/event) est la seule exception car vous devez passer un délégué dans ce cas. Cette directive s'explique par le fait que les délégués peuvent prendre différentes formes dans différents langages Windows Runtime (s’ils prennent en charge une seule ou plusieurs inscriptions de clients). Les événements, avec leur modèle à abonnés multiples, constituent une option beaucoup plus prévisible et cohérente.

La signature d'un délégué de gestionnaire d'événement doit être composée de deux paramètres : *sender* (**IInspectable**) et *args* (un type d'argument d'événement, par exemple [**RoutedEventArgs**](/uwp/api/windows.ui.xaml.routedeventargs)).

Notez que ces instructions ne s’appliquent pas obligatoirement si vous concevez une API interne. Bien que les API internes finissent souvent par devenir publiques.

## <a name="related-topics"></a>Rubriques connexes
* [Créer des API avec C++/WinRT](author-apis.md)
* [Utiliser des API avec C++/WinRT](consume-apis.md)
* [Gérer des événements en utilisant des délégués en C++/WinRT](handle-events.md)
* [Composants Windows Runtime avec C++/WinRT](/windows/uwp/winrt-components/create-a-windows-runtime-component-in-cppwinrt)
