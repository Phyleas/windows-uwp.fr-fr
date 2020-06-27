---
title: Étendre l’exemple de jeu
description: Découvrez comment implémenter une superposition XAML pour un jeu DirectX UWP.
keywords: DirectX, XAML
ms.date: 10/24/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 06b52e5b6fdba1db83c941e770cd49360085accf
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409548"
---
# <a name="extend-the-sample-game"></a>Étendre l’exemple de jeu

> [!NOTE]
> Cette rubrique fait partie de la série de didacticiels [créer un jeu de plateforme Windows universelle simple (UWP) avec DirectX](tutorial--create-your-first-uwp-directx-game.md) . La rubrique de ce lien définit le contexte de la série.

À ce stade, nous avons abordé les principaux composants d’un jeu DirectX 3D de base plateforme Windows universelle (UWP). Vous pouvez configurer l’infrastructure pour un jeu, y compris le fournisseur d’affichage et le pipeline de rendu, et implémenter une boucle de jeu de base. Vous pouvez également créer une superposition d’interface utilisateur de base, incorporer des sons et implémenter des contrôles. Vous avez la possibilité de créer un jeu de votre choix, mais si vous avez besoin d’aide et d’informations supplémentaires, consultez ces ressources.

-   [Graphiques et jeux DirectX](/windows/desktop/directx)
-   [Vue d’ensemble de Direct3D 11](/windows/desktop/direct3d11/dx-graphics-overviews)
-   [Référence de Direct3D 11](/windows/desktop/direct3d11/d3d11-graphics-reference)

## <a name="using-xaml-for-the-overlay"></a>Utilisation de XAML pour la superposition

Nous n’avons pas abordé en détail l’utilisation de XAML au lieu de [Direct2D](/windows/desktop/Direct2D/direct2d-portal) pour la superposition. XAML présente de nombreux avantages par rapport à Direct2D pour le dessin des éléments d’interface utilisateur. L’avantage le plus important est qu’il rend l’intégration de l’apparence de Windows 10 à votre jeu DirectX plus pratique. Un grand nombre des éléments, styles et comportements communs qui définissent une application du Windows Store sont étroitement intégrés au modèle XAML, ce qui réduit les tâches d’implémentation d’un développeur de jeux. Si la conception de votre propre jeu a une interface utilisateur complexe, envisagez d’utiliser XAML à la place de Direct2D.

Avec XAML, nous pouvons créer une interface de jeu qui ressemble à celle de Direct2D effectuée précédemment.

### <a name="xaml"></a>XAML
![Superposition XAML](./images/simple-dx-game-extend-xaml.PNG)

### <a name="direct2d"></a>Direct2D
![Chevauchement D2D](./images/simple-dx-game-extend-d2d.PNG)

Bien qu’ils aient des résultats de fin similaires, il existe un certain nombre de différences entre l’implémentation d’interfaces Direct2D et XAML.

Fonctionnalité | XAML| Direct2D
:----------|:----------- | :-----------
Définition de la superposition | Défini dans un fichier XAML, `\*.xaml` . Une fois que vous avez compris XAML, la création et la configuration de superpositions plus complexes sont effectuées simpiler par rapport à Direct2D.| Défini en tant que collection de primitives Direct2D et de chaînes [DirectWrite](/windows/desktop/DirectWrite/direct-write-portal) manuellement placées et écrites dans une mémoire tampon cible Direct2D. 
Options de l’interface utilisateur | Les éléments de l’interface utilisateur XAML proviennent d’éléments standardisés qui font partie des API XAML Windows Runtime, notamment [**Windows :: UI :: XAML**](/uwp/api/Windows.UI.Xaml) et [**Windows :: UI :: XAML :: Controls**](/uwp/api/Windows.UI.Xaml.Controls). Le code qui gère le comportement des éléments de l’interface utilisateur XAML est défini dans un fichier code-behind, Main.xaml.cpp. | Les formes simples peuvent être dessinées comme des rectangles et des ellipses.
Redimensionnement de fenêtre | Gère naturellement les événements de changement d’état de redimensionnement et d’affichage, transformant ainsi la superposition en conséquence | Vous devez spécifier manuellement comment redessiner les composants de la superposition.

Une autre grande différence concerne la [chaîne de permutation](/windows/uwp/graphics-concepts/swap-chains). Vous n’êtes pas obligé d’attacher la chaîne de permutation à un objet [**Windows :: UI :: Core :: CoreWindow**](/uwp/api/windows.ui.core.corewindow) . Au lieu de cela, une application DirectX qui incorpore du code XAML associe une chaîne de permutation lorsqu’un nouvel objet [**SwapChainPanel**](/uwp/api/windows.ui.xaml.controls.swapchainpanel) est construit. 

L’extrait de code suivant montre comment déclarer du code XAML pour **SwapChainPanel** dans le fichier [**DirectXPage. Xaml**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/DirectXPage.xaml) .
```xml
<Page
    x:Class="Simple3DGameXaml.DirectXPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:Simple3DGameXaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">
    <SwapChainPanel x:Name="DXSwapChainPanel">

    <!-- ... XAML user controls and elements -->

    </SwapChainPanel>
</Page>
```

L’objet **SwapChainPanel** est défini en tant que propriété de [**contenu**](/uwp/api/Windows.UI.Xaml.Window.Content) de l’objet de fenêtre actuel créé [au lancement](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/App.xaml.cpp#L45-L51) par le singleton de l’application.

```cpp
void App::OnLaunched(_In_ LaunchActivatedEventArgs^ /* args */)
{
    m_mainPage = ref new DirectXPage();

    Window::Current->Content = m_mainPage;
    // Bring the application to the foreground so that it's visible
    Window::Current->Activate();
}
```

Pour attacher la chaîne d’échange configurée à l’instance [**SwapChainPanel**](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) définie par votre code XAML, vous devez obtenir un pointeur vers l’implémentation de l’interface [**ISwapChainPanelNative**](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-iswapchainpanelnative) Native sous-jacente et appeler [**ISwapChainPanelNative :: SetSwapChain**](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-iswapchainpanelnative-setswapchain) dessus, en lui transmettant votre chaîne d’échange configurée. 

L’extrait de code suivant de [**DX ::D eviceresources :: CreateWindowSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/Common/DeviceResources.cpp#L218-L521) pour plus d’informations sur l’interopérabilité DirectX/XAML :

```cpp
        ComPtr<IDXGIDevice3> dxgiDevice;
        DX::ThrowIfFailed(
            m_d3dDevice.As(&dxgiDevice)
            );

        ComPtr<IDXGIAdapter> dxgiAdapter;
        DX::ThrowIfFailed(
            dxgiDevice->GetAdapter(&dxgiAdapter)
            );

        ComPtr<IDXGIFactory2> dxgiFactory;
        DX::ThrowIfFailed(
            dxgiAdapter->GetParent(IID_PPV_ARGS(&dxgiFactory))
            );

        // When using XAML interop, the swap chain must be created for composition.
        DX::ThrowIfFailed(
            dxgiFactory->CreateSwapChainForComposition(
                m_d3dDevice.Get(),
                &swapChainDesc,
                nullptr,
                &m_swapChain
                )
            );

        // Associate swap chain with SwapChainPanel
        // UI changes will need to be dispatched back to the UI thread
        m_swapChainPanel->Dispatcher->RunAsync(CoreDispatcherPriority::High, ref new DispatchedHandler([=]()
        {
            // Get backing native interface for SwapChainPanel
            ComPtr<ISwapChainPanelNative> panelNative;
            DX::ThrowIfFailed(
                reinterpret_cast<IUnknown*>(m_swapChainPanel)->QueryInterface(IID_PPV_ARGS(&panelNative))
                );
            DX::ThrowIfFailed(
                panelNative->SetSwapChain(m_swapChain.Get())
                );
        }, CallbackContext::Any));

        // Ensure that DXGI does not queue more than one frame at a time. This both reduces latency and
        // ensures that the application will only render after each VSync, minimizing power consumption.
        DX::ThrowIfFailed(
            dxgiDevice->SetMaximumFrameLatency(1)
            );
    }
```

Pour plus d’informations sur ce processus, voir [Technologie interop DirectX et XAML](directx-and-xaml-interop.md).

## <a name="sample"></a>Exemple

Pour télécharger la version de ce jeu qui utilise XAML pour la superposition, accédez à l’exemple de jeu de prise en charge [Direct3D (XAML)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameXaml).

Contrairement à la version de l’exemple de jeu présentée dans le reste de ces rubriques, la version XAML définit son infrastructure dans les fichiers [app. Xaml. cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/App.xaml.cpp) et [DirectXPage. Xaml. cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/DirectXPage.xaml.cpp) , au lieu de [app. cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameDX/cpp/App.cpp) et [GameInfoOverlay. cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp), respectivement.
