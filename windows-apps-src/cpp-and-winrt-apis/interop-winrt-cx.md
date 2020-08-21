---
description: Cette rubrique montre deux fonctions d’assistance qui peuvent être utilisées pour effectuer des conversions entre des objets [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) et [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).
title: Interopérabilité entre C++/WinRT et C++/CX
ms.date: 10/09/2018
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, port, migrer, interopérabilité, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: d3fa04f0aabe001dc87ce4292dff7557432583a6
ms.sourcegitcommit: 99100b58a5b49d8ba78905b15b076b2c5cffbe49
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88502284"
---
# <a name="interop-between-cwinrt-and-ccx"></a>Interopérabilité entre C++/WinRT et C++/CX

Avant de lire cette rubrique, vous aurez besoin des informations de la rubrique [Se déplacer de C++/WinRT vers C++/CX](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx). Cette rubrique présente deux options de stratégie principales pour le déplacement de votre projet [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) vers [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

- Déplacer tout le projet en une seule passe. L’option la plus simple pour un projet pas trop important. Si vous avez un projet de composant Windows Runtime, cette stratégie est alors votre seule option.
- Déplacez le projet progressivement (la taille ou la complexité de votre codebase peut rendre cela nécessaire). Toutefois, cette stratégie vous invite à suivre un processus de déplacement dans lequel un code C++/CX et C++/WinRT existent pendant un moment côte à côte dans le même projet. Pour un projet XAML, à un moment donné, vos types de pages XAML doivent être *soit* tous les C++/WinRT *soit* tous les C++/CX.

Cette rubrique relative à l’interopérabilité s’applique à cette *deuxième* stratégie&mdash;pour les cas où vous avez besoin de déplacer progressivement votre projet. Cette rubrique présente deux fonctions d’application auxiliaire que vous pouvez utiliser pour convertir un objet C++/CX en un objet C++/WinRT (et inversement) dans le même projet.

Ces fonctions d’application auxiliaire sont très utiles lorsque vous déplacez progressivement votre code de C++/CX vers C++/WinRT. Vous pouvez également choisir d’utiliser à la fois les projections de langage C++/WinRT et C++/CX dans le même projet, que vous vous déplaciez ou non, et d’utiliser ces fonctions d’application auxiliaire pour interagir entre les deux.

Après avoir lu cette rubrique, pour obtenir des informations et des exemples de code montrant comment prendre en charge les tâches et les coroutines PPL côte à côte dans le même projet (par exemple, l’appel de coroutines à partir de chaînes de tâches), consultez la rubrique plus avancée [Asynchronisme et interopérabilité entre C++/WinRT et C++/CX](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx-async).

## <a name="the-from_cx-and-to_cx-functions"></a>Les fonctions **from_cx** et **to_cx**

Voici une liste de codes sources d’un fichier d’en-tête nommé `interop_helpers.h`, contenant deux fonctions d’application auxiliaire de conversion. Au fur et à mesure du portage de votre projet, certaines parties resteront en C++/CX, et d’autres en C++/WinRT. Vous pouvez utiliser ces fonctions auxiliaires pour convertir des objets de et vers C++/CX et C++/WinRT dans votre projet aux points limites entre ces deux parties.

Les sections qui suivent la liste des codes expliquent les deux fonctions et comment créer et utiliser le fichier d’en-tête dans votre projet.

```cppwinrt
// interop_helpers.h
#pragma once

template <typename T>
T from_cx(Platform::Object^ from)
{
    T to{ nullptr };

    winrt::check_hresult(reinterpret_cast<::IUnknown*>(from)
        ->QueryInterface(winrt::guid_of<T>(), winrt::put_abi(to)));

    return to;
}

template <typename T>
T^ to_cx(winrt::Windows::Foundation::IUnknown const& from)
{
    return safe_cast<T^>(reinterpret_cast<Platform::Object^>(winrt::get_abi(from)));
}
```

### <a name="the-from_cx-function"></a>La fonction **from_cx**

La fonction d’application auxiliaire **from_cx** convertit un objet C++/CX en un objet équivalent C++/WinRT. La fonction caste un objet C++/CX vers son pointeur d’interface [**IUnknown**](/windows/win32/api/unknwn/nn-unknwn-iunknown) sous-jacent. Elle appelle ensuite [**QueryInterface**](/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) sur ce pointeur pour rechercher l’interface par défaut de l’objet C++/WinRT. **QueryInterface** est l’équivalent de l’interface binaire d’application (ABI) Windows Runtime de l’extension `safe_cast` C++/CX. Puis, la fonction [**winrt::put_abi**](/uwp/cpp-ref-for-winrt/put-abi) récupère l’adresse du pointeur d’interface **IUnknown** sous-jacent d’un objet C++/WinRT afin qu’elle puisse être définie sur une autre valeur.

### <a name="the-to_cx-function"></a>La fonction **to_cx**

La fonction d’application auxiliaire **to_cx** convertit un objet C++/WinRT en un objet équivalent C++/CX. La fonction [**winrt::get_abi**](/uwp/cpp-ref-for-winrt/get-abi) extrait un pointeur vers l’interface **IUnknown** sous-jacente d’un objet C++/WinRT. La fonction caste ce pointeur vers un objet C++/CX avant d’utiliser l’extension `safe_cast` C++/CX pour rechercher le type C++/CX demandé.

### <a name="the-interop_helpersh-header-file"></a>Le fichier d'en-tête `interop_helpers.h`

Pour utiliser les deux fonctions d’application auxiliaire dans votre projet, procédez comme suit.

- Ajoutez un nouvel élément **Fichier d’en-tête (. h)** à votre projet et nommez-le `interop_helpers.h`.
- Remplacez le contenu de `interop_helpers.h` par la liste de codes ci-dessus.
- Ajoutez ces inclusions à `pch.h`.

```cppwinrt
// pch.h
...
#include <unknwn.h>
// Include C++/WinRT projected Windows API headers here.
...
#include <interop_helpers.h>
```

## <a name="taking-a-ccx-project-and-adding-cwinrt-support"></a>Prise en charge d’un projet C++/CX et ajout du support C++/WinRT

Cette section décrit la procédure à suivre si vous avez décidé de prendre en charge votre projet C++/CX existant, y ajouter le support C++/WinRT et de faire en sorte que votre déplacement fonctionne. Consultez également [Support Visual Studio pour C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

Pour mélanger C++/CX et C++/WinRT dans un projet C++/CX&mdash;notamment l’utilisation des fonctions d’application auxiliaire **from_cx** et **to_cx** dans le projet&mdash;vous devez ajouter manuellement le support C++/WinRT au projet.

Ouvrez d’abord votre projet C++/CX dans Visual Studio et confirmez que la propriété de ce projet **Général** \> **Version de la plateforme cible** est définie sur 10.0.17134.0 (Windows 10, version 1803) ou une version supérieure.

### <a name="install-the-cwinrt-nuget-package"></a>Installation du package NuGet C++/WinRT

Le [package NuGet Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) fournit un support de build C++/WinRT (propriétés et cibles MSBuild). Pour l’installer, cliquez sur l’élément de menu **Projet** \> **Gérer des packages NuGet...** \> **Parcourir**, tapez ou collez **Microsoft.Windows.CppWinRT** dans la zone de recherche, sélectionnez l’élément dans les résultats de la recherche, puis cliquez sur **Installer** pour installer le package correspondant à ce projet.

> [!IMPORTANT]
> L’installation du package NuGet C++/WinRT entraîne la désactivation du support de C++/CX dans le projet. Si vous déplacez en une seule passe, il est alors recommandé de laisser le support désactivé afin que les messages de la build vous aident à trouver (et à déplacer) toutes vos dépendances sur C++/CX (en transformant éventuellement ce qui est un pur projet C++/CX en un pur projet C++/WinRT). Mais consultez la section suivante pour plus d’informations sur la réactivation.

### <a name="turn-ccx-support-back-on"></a>Réactiver le support C++/CX

Si vous déplacez en une seule passe, cela n’est pas nécessaire. Toutefois, si vous devez effectuer un déplacement progressif, à ce stade, vous devez réactiver le support C++/CX dans votre projet. Dans les propriétés du projet, sélectionnez **C/C++** \> **Général** \> **Consommer l’extension Windows Runtime** \> **Oui (/ZW)** ).

Sinon (ou, pour un projet XAML, en outre) vous pouvez également ajouter le support C++/CX à l’aide de la page de propriétés du projet C++/WinRT dans Visual Studio. Dans les propriétés du projet, **Propriétés communes** \> **C++/WinRT** \> **Langue du projet** \> **C++/CX**. Si vous procédez ainsi, vous ajouterez la propriété suivante à votre fichier `.vcxproj`.

```xml
  <PropertyGroup Label="Globals">
    <CppWinRTProjectLanguage>C++/CX</CppWinRTProjectLanguage>
  </PropertyGroup>
```

> [!IMPORTANT]
> Dès que vous avez besoin de générer le traitement du contenu d’un **Fichier Midl (.idl)** dans des fichiers stub, vous devrez modifier **Langue du projet** en **C++/WinRT**. Une fois ces stubs générés par la build, modifiez **Langue du projet** en **C++/CX**.

Pour obtenir la liste des options de personnalisation similaires (permettant d’ajuster le comportement de l’outil `cppwinrt.exe`), consultez le fichier [Lisez-moi](https://github.com/microsoft/cppwinrt/blob/master/nuget/readme.md#customizing) du package NuGet Microsoft.Windows.CppWinRT.

### <a name="include-cwinrt-header-files"></a>Inclure des fichiers d’en-tête C++/WinRT

Le minimum à faire, dans votre fichier d’en-tête précompilé (généralement `pch.h`), est d’inclure `winrt/base.h` comme indiqué ci-dessous.

```cppwinrt
// pch.h
...
#include <winrt/base.h>
...
```

Mais vous aurez certainement besoin des types de l’espace de noms **winrt::Windows::Foundation**. Vous connaissez peut-être déjà d’autres espaces de noms dont vous aurez besoin. Incluez donc les en-têtes d’API Windows projetées C++/WinRT qui correspondent à ces espaces de noms (il n’est pas nécessaire d’inclure explicitement `winrt/base.h` à présent car il le sera automatiquement pour vous).

```cppwinrt
// pch.h
...
#include <winrt/Windows.Foundation.h>
// Include any other C++/WinRT projected Windows API headers here.
...
```

L’exemple de code est également nécessaire dans la section suivante (*Prise en charge d’un projet C++/WinRT et ajout du support C++/CX*) pour une technique utilisant les alias d’espace de noms `namespace cx` et `namespace winrt`. Cette technique vous permet de gérer des collisions d’espaces de noms potentiels entre la projection C++/WinRT et la projection C++/CX.

### <a name="add-interop_helpersh-to-the-project"></a>Ajouter `interop_helpers.h` au projet

Vous pouvez maintenant ajouter les fonctions **from_cx** et **to_cx** à votre projet C++/CX. Pour obtenir des instructions sur la procédure à suivre, consultez la section ci-dessus sur les fonctions [**from_cx** et **to_cx** ](#the-from_cx-and-to_cx-functions).

## <a name="taking-a-cwinrt-project-and-adding-ccx-support"></a>Prise en charge d’un projet C++/WinRT et ajout du support C++/CX

Cette section décrit la procédure à suivre si vous avez décidé de créer un nouveau projet C++/WinRT et de faire en sorte que votre déplacement fonctionne.

Pour mélanger C++/WinRT et C++/CX dans un projet C++/WinRT&mdash;notamment à l’aide des fonctions d’application auxiliaire **from_cx** et **to_cx** dans le projet&mdash;vous devez ajouter manuellement le support C++/CX au projet.

- Créez un nouveau projet C++/WinRT dans Visual Studio à l’aide de l’un des modèles de projet C++t/WinRT (consultez [Support Visual Studio pour C++ /WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)).
- Activer un support de projet pour C++/CX. Dans les propriétés du projet, sélectionnez **C/C++** \> **Général** \> **Consommer l'extension Windows Runtime** \> **Oui (/ZW)**.

### <a name="an-example-cwinrt-project-showing-the-two-helper-functions-in-use"></a>Exemple de projet C++/WinRT affichant les deux fonctions d’application auxiliaire en cours d’utilisation

Dans cette section, vous pouvez créer un exemple de projet C++/WinRT qui démontre comment utiliser **from_cx** et **to_cx**. Il illustre également comment utiliser des alias d’espaces de noms pour les différents îlots de code afin d’éviter les collisions potentielles d’espaces de noms entre la projection C++/WinRT et la projection C++/CX.

- Créez un projet **Visual C++** \> **Windows Universal** \> **Core App (C++/WinRT)**.
- Dans les propriétés du projet, sélectionnez **C/C++** \> **Général** \> **Consommer l'extension Windows Runtime** \> **Oui (/ZW)**.
- Ajoutez `interop_helpers.h` au projet. Pour obtenir des instructions sur la procédure à suivre, consultez la section ci-dessus sur les fonctions [**from_cx** et **to_cx** ](#the-from_cx-and-to_cx-functions).
- Remplacez le contenu de `App.cpp` par la liste de codes ci-dessous, build et exécution.

`WINRT_ASSERT` est une définition de macro qui s’étend à [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros).

```cppwinrt
// App.cpp
#include "pch.h"
#include <sstream>

namespace cx
{
    using namespace Windows::Foundation;
}

namespace winrt
{
    using namespace Windows;
    using namespace Windows::ApplicationModel::Core;
    using namespace Windows::Foundation;
    using namespace Windows::Foundation::Numerics;
    using namespace Windows::UI;
    using namespace Windows::UI::Core;
    using namespace Windows::UI::Composition;
}

struct App : winrt::implements<App, winrt::IFrameworkViewSource, winrt::IFrameworkView>
{
    winrt::CompositionTarget m_target{ nullptr };
    winrt::VisualCollection m_visuals{ nullptr };
    winrt::Visual m_selected{ nullptr };
    winrt::float2 m_offset{};

    winrt::IFrameworkView CreateView()
    {
        return *this;
    }

    void Initialize(winrt::CoreApplicationView const &)
    {
    }

    void Load(winrt::hstring const&)
    {
    }

    void Uninitialize()
    {
    }

    void Run()
    {
        winrt::CoreWindow window = winrt::CoreWindow::GetForCurrentThread();
        window.Activate();

        winrt::CoreDispatcher dispatcher = window.Dispatcher();
        dispatcher.ProcessEvents(winrt::CoreProcessEventsOption::ProcessUntilQuit);
    }

    void SetWindow(winrt::CoreWindow const & window)
    {
        winrt::Compositor compositor;
        winrt::ContainerVisual root = compositor.CreateContainerVisual();
        m_target = compositor.CreateTargetForCurrentView();
        m_target.Root(root);
        m_visuals = root.Children();

        window.PointerPressed({ this, &App::OnPointerPressed });
        window.PointerMoved({ this, &App::OnPointerMoved });

        window.PointerReleased([&](auto && ...)
        {
            m_selected = nullptr;
        });
    }

    void OnPointerPressed(IInspectable const &, winrt::PointerEventArgs const & args)
    {
        winrt::float2 const point = args.CurrentPoint().Position();

        for (winrt::Visual visual : m_visuals)
        {
            winrt::float3 const offset = visual.Offset();
            winrt::float2 const size = visual.Size();

            if (point.x >= offset.x &&
                point.x < offset.x + size.x &&
                point.y >= offset.y &&
                point.y < offset.y + size.y)
            {
                m_selected = visual;
                m_offset.x = offset.x - point.x;
                m_offset.y = offset.y - point.y;
            }
        }

        if (m_selected)
        {
            m_visuals.Remove(m_selected);
            m_visuals.InsertAtTop(m_selected);
        }
        else
        {
            AddVisual(point);
        }
    }

    void OnPointerMoved(IInspectable const &, winrt::PointerEventArgs const & args)
    {
        if (m_selected)
        {
            winrt::float2 const point = args.CurrentPoint().Position();

            m_selected.Offset(
            {
                point.x + m_offset.x,
                point.y + m_offset.y,
                0.0f
            });
        }
    }

    void AddVisual(winrt::float2 const point)
    {
        winrt::Compositor compositor = m_visuals.Compositor();
        winrt::SpriteVisual visual = compositor.CreateSpriteVisual();

        static winrt::Color colors[] =
        {
            { 0xDC, 0x5B, 0x9B, 0xD5 },
            { 0xDC, 0xED, 0x7D, 0x31 },
            { 0xDC, 0x70, 0xAD, 0x47 },
            { 0xDC, 0xFF, 0xC0, 0x00 }
        };

        static unsigned last = 0;
        unsigned const next = ++last % _countof(colors);
        visual.Brush(compositor.CreateColorBrush(colors[next]));

        float const BlockSize = 100.0f;

        visual.Size(
        {
            BlockSize,
            BlockSize
        });

        visual.Offset(
        {
            point.x - BlockSize / 2.0f,
            point.y - BlockSize / 2.0f,
            0.0f,
        });

        m_visuals.InsertAtTop(visual);

        m_selected = visual;
        m_offset.x = -BlockSize / 2.0f;
        m_offset.y = -BlockSize / 2.0f;
    }
};

int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    winrt::init_apartment();

    winrt::Uri uri(L"http://aka.ms/cppwinrt");
    std::wstringstream wstringstream;
    wstringstream << L"C++/WinRT: " << uri.Domain().c_str() << std::endl;

    // Convert from a C++/WinRT type to a C++/CX type.
    cx::Uri^ cx = to_cx<cx::Uri>(uri);
    wstringstream << L"C++/CX: " << cx->Domain->Data() << std::endl;
    ::OutputDebugString(wstringstream.str().c_str());

    // Convert from a C++/CX type to a C++/WinRT type.
    winrt::Uri uri_from_cx = from_cx<winrt::Uri>(cx);
    WINRT_ASSERT(uri.Domain() == uri_from_cx.Domain());
    WINRT_ASSERT(uri == uri_from_cx);

    winrt::CoreApplication::Run(winrt::make<App>());
}
```

## <a name="important-apis"></a>API importantes
* [Interface IUnknown](/windows/win32/api/unknwn/nn-unknwn-iunknown)
* [Fonction QueryInterface](/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))
* [Fonction winrt::get_abi](/uwp/cpp-ref-for-winrt/get-abi)
* [Fonction winrt::put_abi](/uwp/cpp-ref-for-winrt/put-abi)

## <a name="related-topics"></a>Rubriques connexes
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Passer de C++/CX à C++/WinRT](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx)
* [Asynchronisme, interopérabilité entre C++/WinRT et C++/CX](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx-async)
