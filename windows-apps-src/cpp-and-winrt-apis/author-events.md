---
description: Cette rubrique montre comment créer un composant Windows Runtime contenant une classe runtime qui déclenche des événements. Elle montre également une application qui utilise le composant et gère les événements.
title: Créer des événements en C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, créer, événement
ms.localizationpriority: medium
ms.openlocfilehash: 66691b1cd75a27e683261c12b7a3056c53160079
ms.sourcegitcommit: 7aaf0740a5d3a17ebf9214aa5e5d056924317673
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92297627"
---
# <a name="author-events-in-cwinrt"></a>Créer des événements en C++/WinRT

Cette rubrique vient compléter les explications fournies sur le composant Windows Runtime et l’application consommatrice, dont la génération vous est expliquée dans la rubrique [Composants Windows Runtime avec C++/WinRT](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md).

Voici les nouvelles fonctionnalités traitées dans cette rubrique.
- Mettez à jour la classe runtime du thermomètre pour déclencher un événement lorsque sa température passe en dessous de 0.
- Mise à jour de l’application Core qui consomme la classe runtime du thermomètre pour qu’elle gère cet événement.

> [!NOTE]
> Pour plus d’informations sur l’installation et l’utilisation de l’extension VSIX (Visual Studio Extension) [C++/WinRT](./intro-to-using-cpp-with-winrt.md) et du package NuGet (qui fournissent ensemble la prise en charge des modèles et des builds de projet), consultez [Prise en charge de Visual Studio pour C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

> [!IMPORTANT]
> Pour obtenir les principaux concepts et termes facilitant votre compréhension pour utiliser et créer des classes runtime avec C++/WinRT, voir [Utiliser des API avec C++/WinRT](consume-apis.md) et [Créer des API avec C++/WinRT](author-apis.md).

## <a name="create-thermometerwrc-and-thermometercoreapp"></a>Créer **ThermometerWRC** et **ThermometerCoreApp**

Si vous souhaitez procéder aux mises à jour présentées dans cette rubrique pour pouvoir générer et exécuter le code, la première étape consiste à suivre la procédure pas à pas de la rubrique [Composants Windows Runtime avec C++/WinRT](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md). De cette façon, vous disposerez du composant Windows Runtime **ThermometerWRC** et de l’application Core **ThermometerCoreApp** qui le consomme.

## <a name="update-thermometerwrc-to-raise-an-event"></a>Mettre à jour **ThermometerWRC** pour déclencher un événement

Mettez à jour `Thermometer.idl` de sorte qu’il se présente comme dans le listing ci-dessous. Voici comment déclarer un événement dont le type délégué est [**EventHandler**](/uwp/api/windows.foundation.eventhandler-1) avec, comme argument, un nombre à virgule flottante simple précision.

```idl
// Thermometer.idl
namespace ThermometerWRC
{
    runtimeclass Thermometer
    {
        Thermometer();
        void AdjustTemperature(Single deltaFahrenheit);
        event Windows.Foundation.EventHandler<Single> TemperatureIsBelowFreezing;
    };
}
```

Enregistrez le fichier. Dans son état actuel, le projet ne sera pas généré complètement. Effectuez cependant une génération dès à présent pour générer des versions mises à jour des fichiers stub `\ThermometerWRC\ThermometerWRC\Generated Files\sources\Thermometer.h` et `Thermometer.cpp`. Vous pouvez maintenant voir dans ces fichiers les implémentations stub de l’événement **TemperatureIsBelowFreezing**. Dans C++/WinRT, un événement déclaré dans le fichier IDL est implémenté comme un ensemble de fonctions surchargées (de la même manière qu'une propriété est implémentée comme une paire de fonctions Get et Set surchargées). Une surcharge prend un délégué à inscrire et retourne un jeton ([**winrt::event_token**](/uwp/cpp-ref-for-winrt/event-token)). L’autre prend un jeton et révoque l’inscription du délégué associé.

Ouvrez à présent `Thermometer.h` et `Thermometer.cpp`, et mettez à jour l’implémentation de la classe runtime **Thermometer**. Dans `Thermometer.h`, ajoutez les deux fonctions **TemperatureIsBelowFreezing** surchargées, ainsi qu’un membre de données d’événement privé à utiliser dans l’implémentation de ces fonctions.

```cppwinrt
// Thermometer.h
...
namespace winrt::ThermometerWRC::implementation
{
    struct Thermometer : ThermometerT<Thermometer>
    {
        ...
        winrt::event_token TemperatureIsBelowFreezing(Windows::Foundation::EventHandler<float> const& handler);
        void TemperatureIsBelowFreezing(winrt::event_token const& token) noexcept;

    private:
        winrt::event<Windows::Foundation::EventHandler<float>> m_temperatureIsBelowFreezingEvent;
        ...
    };
}
...
```

Comme vous pouvez le voir ci-dessus, un événement est représenté par le modèle struct [**winrt::event**](/uwp/cpp-ref-for-winrt/event), paramétré par un type délégué particulier (qui peut lui-même être paramétré par un type args).

Dans `Thermometer.cpp`, implémentez les deux fonctions **TemperatureIsBelowFreezing** surchargées.

```cppwinrt
// Thermometer.cpp
...
namespace winrt::ThermometerWRC::implementation
{
    winrt::event_token Thermometer::TemperatureIsBelowFreezing(Windows::Foundation::EventHandler<float> const& handler)
    {
        return m_temperatureIsBelowFreezingEvent.add(handler);
    }

    void Thermometer::TemperatureIsBelowFreezing(winrt::event_token const& token) noexcept
    {
        m_temperatureIsBelowFreezingEvent.remove(token);
    }

    void Thermometer::AdjustTemperature(float deltaFahrenheit)
    {
        m_temperatureFahrenheit += deltaFahrenheit;
        if (m_temperatureFahrenheit < 32.f) m_temperatureIsBelowFreezingEvent(*this, m_temperatureFahrenheit);
    }
}
```

> [!NOTE]
> Pour savoir ce qu’est un revoker d’événement automatique, consultez [Révoquer un délégué inscrit](handle-events.md#revoke-a-registered-delegate). L’implémentation d’un revoker d’événement automatique est gratuite pour votre événement. En d’autres termes, vous n’avez pas besoin d’implémenter la surcharge pour le revoker d’événement puisque l’implémentation vous est fournie par la projection C++/WinRT.

Les autres surcharges (inscription et révocation manuelle) *ne sont pas* intégrées à la projection. Vous pouvez donc les implémenter de manière optimale pour votre scénario. Le fait d’appeler [**event::add**](/uwp/cpp-ref-for-winrt/event#eventadd-function) et [**event::remove**](/uwp/cpp-ref-for-winrt/event#eventremove-function) comme indiqué dans ces implémentations constitue une valeur par défaut efficace et concurrency/thread-safe. Mais si vous avez un très grand nombre d’événements, vous pouvez ne pas souhaiter un champ d’événement pour chacun, mais plutôt opter pour un type d’implémentation fragmentée à la place.

Vous pouvez également voir ci-dessus que l’implémentation de la fonction **AdjustTemperature** a été mise à jour pour déclencher l’événement **TemperatureIsBelowFreezing** quand la température passe en dessous de 0.

## <a name="update-thermometercoreapp-to-handle-the-event"></a>Mettre à jour **ThermometerCoreApp** pour gérer l’événement

Dans le projet **ThermometerCoreApp**, dans `App.cpp`, apportez les modifications suivantes au code pour inscrire un gestionnaire d’événements, puis provoquez une température inférieure à 0.

`WINRT_ASSERT` est une définition de macro, qui se développe en [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros).

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    winrt::event_token m_eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        m_eventToken = m_thermometer.TemperatureIsBelowFreezing([](const auto &, float temperatureFahrenheit)
        {
            WINRT_ASSERT(temperatureFahrenheit < 32.f); // Put a breakpoint here.
        });
    }
    ...

    void Uninitialize()
    {
        m_thermometer.TemperatureIsBelowFreezing(m_eventToken);
    }
    ...
    
    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_thermometer.AdjustTemperature(-1.f);
        ...
    }
    ...
};
```

Tenez compte de la modification de la méthode **OnPointerPressed**. À présent, chaque fois que vous cliquez sur la fenêtre, vous *enlevez* 1 degré Fahrenheit à la température du thermomètre. À présent, l’application gère l’événement qui est déclenché quand la température passe en dessous de 0. Pour démontrer que l’événement est déclenché comme prévu, insérez un point d’arrêt à l’intérieur de l’expression lambda qui gère l’événement **TemperatureIsBelowFreezing**, exécutez l’application, puis cliquez à l’intérieur de la fenêtre.

## <a name="parameterized-delegates-across-an-abi"></a>Délégués paramétrables dans une interface ABI

Si votre événement doit être accessible via une interface binaire d'application (ABI)&mdash;par exemple entre un composant et son application consommatrice&mdash;votre événement doit utiliser un type de délégué Windows Runtime. L’exemple ci-dessus utilise le type de délégué Windows Runtime [**Windows::Foundation::EventHandler\<T\>** ](/uwp/api/windows.foundation.eventhandler). [**TypedEventHandler\<TSender, TResult\>** ](/uwp/api/windows.foundation.eventhandler) est un autre exemple de type de délégué Windows Runtime.

Comme les paramètres de type pour ces deux types de délégués doivent traverser l'ABI, ils doivent donc aussi être de type Windows Runtime. Cela inclut les classes runtime Windows et tierces ainsi que les types primitifs tels que les nombres et les chaînes. Le compilateur vous aide en affichant une erreur « *T must be WinRT type* » (T doit être de type WinRT) si vous oubliez cette contrainte.

Vous trouverez ci-dessous un exemple sous forme de listes de code. Commencez par les projets **ThermometerWRC** et **ThermometerCoreApp** que vous avez créés précédemment dans cette rubrique, puis modifiez le code de ces projets en reproduisant le code figurant dans ces listes.

La première liste concerne le projet **ThermometerWRC**. Après avoir modifié `ThermometerWRC.idl` comme indiqué ci-dessous, générez le projet, puis copiez `MyEventArgs.h` et `.cpp` dans le projet (à partir du dossier `Generated Files`) comme vous l’avez fait précédemment avec `Thermometer.h` et `.cpp`.

```cppwinrt
// ThermometerWRC.idl
namespace ThermometerWRC
{
    [default_interface]
    runtimeclass MyEventArgs
    {
        Single TemperatureFahrenheit{ get; };
    }

    [default_interface]
    runtimeclass Thermometer
    {
        ...
        event Windows.Foundation.EventHandler<ThermometerWRC.MyEventArgs> TemperatureIsBelowFreezing;
        ...
    };
}

// MyEventArgs.h
#pragma once
#include "MyEventArgs.g.h"

namespace winrt::ThermometerWRC::implementation
{
    struct MyEventArgs : MyEventArgsT<MyEventArgs>
    {
        MyEventArgs() = default;
        MyEventArgs(float temperatureFahrenheit);
        float TemperatureFahrenheit();

    private:
        float m_temperatureFahrenheit{ 0.f };
    };
}

// MyEventArgs.cpp
#include "pch.h"
#include "MyEventArgs.h"
#include "MyEventArgs.g.cpp"

namespace winrt::ThermometerWRC::implementation
{
    MyEventArgs::MyEventArgs(float temperatureFahrenheit) : m_temperatureFahrenheit(temperatureFahrenheit)
    {
    }

    float MyEventArgs::TemperatureFahrenheit()
    {
        return m_temperatureFahrenheit;
    }
}

// Thermometer.h
...
struct Thermometer : ThermometerT<Thermometer>
{
...
    winrt::event_token TemperatureIsBelowFreezing(Windows::Foundation::EventHandler<ThermometerWRC::MyEventArgs> const& handler);
...
private:
    winrt::event<Windows::Foundation::EventHandler<ThermometerWRC::MyEventArgs>> m_temperatureIsBelowFreezingEvent;
...
}
...

// Thermometer.cpp
#include "MyEventArgs.h"
...
winrt::event_token Thermometer::TemperatureIsBelowFreezing(Windows::Foundation::EventHandler<ThermometerWRC::MyEventArgs> const& handler) { ... }
...
void Thermometer::AdjustTemperature(float deltaFahrenheit)
{
    m_temperatureFahrenheit += deltaFahrenheit;

    if (m_temperatureFahrenheit < 32.f)
    {
        auto args = winrt::make_self<winrt::ThermometerWRC::implementation::MyEventArgs>(m_temperatureFahrenheit);
        m_temperatureIsBelowFreezingEvent(*this, *args);
    }
}
...
```

Cette liste concerne le projet **ThermometerCoreApp**.

```cppwinrt
// App.cpp
...
void Initialize(CoreApplicationView const&)
{
    m_eventToken = m_thermometer.TemperatureIsBelowFreezing([](const auto&, ThermometerWRC::MyEventArgs args)
    {
        float degrees = args.TemperatureFahrenheit();
        WINRT_ASSERT(degrees < 32.f); // Put a breakpoint here.
    });
}
...
```

## <a name="simple-signals-across-an-abi"></a>Signaux simples dans une interface ABI

Si vous n'avez pas besoin de passer des paramètres ou des arguments avec votre événement, vous pouvez définir votre propre type de délégué Windows Runtime simple. L’exemple ci-dessous montre une version plus simple de la classe runtime **Thermometer**. Il déclare un type de délégué nommé **SignalDelegate** puis l'utilise pour déclencher un événement de type signal au lieu d'un événement avec un paramètre.

```idl
// ThermometerWRC.idl
namespace ThermometerWRC
{
    delegate void SignalDelegate();

    runtimeclass Thermometer
    {
        Thermometer();
        event ThermometerWRC.SignalDelegate SignalTemperatureIsBelowFreezing;
        void AdjustTemperature(Single value);
    };
}
```

```cppwinrt
// Thermometer.h
...
namespace winrt::ThermometerWRC::implementation
{
    struct Thermometer : ThermometerT<Thermometer>
    {
        ...

        winrt::event_token SignalTemperatureIsBelowFreezing(ThermometerWRC::SignalDelegate const& handler);
        void SignalTemperatureIsBelowFreezing(winrt::event_token const& token);
        void AdjustTemperature(float deltaFahrenheit);

    private:
        winrt::event<ThermometerWRC::SignalDelegate> m_signal;
        float m_temperatureFahrenheit{ 0.f };
    };
}
```

```cppwinrt
// Thermometer.cpp
...
namespace winrt::ThermometerWRC::implementation
{
    winrt::event_token Thermometer::SignalTemperatureIsBelowFreezing(ThermometerWRC::SignalDelegate const& handler)
    {
        return m_signal.add(handler);
    }

    void Thermometer::SignalTemperatureIsBelowFreezing(winrt::event_token const& token)
    {
        m_signal.remove(token);
    }

    void Thermometer::AdjustTemperature(float deltaFahrenheit)
    {
        m_temperatureFahrenheit += deltaFahrenheit;
        if (m_temperatureFahrenheit < 32.f)
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
    ThermometerWRC::Thermometer m_thermometer;
    winrt::event_token m_eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        m_eventToken = m_thermometer.SignalTemperatureIsBelowFreezing([] { /* ... */ });
    }
    ...

    void Uninitialize()
    {
        m_thermometer.SignalTemperatureIsBelowFreezing(m_eventToken);
    }
    ...

    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_thermometer.AdjustTemperature(-1.f);
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
* [Composants Windows Runtime avec C++/WinRT](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md)