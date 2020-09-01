---
title: Configurer des ressources DirectX et afficher une image
description: Voici comment créer un périphérique Direct3D, une chaîne d’échange et un affichage de cible de rendu, et comment présenter l’image rendue à l’écran.
ms.assetid: d54d96fe-3522-4acb-35f4-bb11c3a5b064
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, jeux, DirectX, ressources, images
ms.localizationpriority: medium
ms.openlocfilehash: cced3b5cb6ad9c3e1ffe077887c5f23ce95745bd
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159193"
---
# <a name="set-up-directx-resources-and-display-an-image"></a>Configurer des ressources DirectX et afficher une image



Voici comment créer un périphérique Direct3D, une chaîne d’échange et un affichage de cible de rendu, et comment présenter l’image rendue à l’écran.

**Objectif :** configurer des ressources DirectX dans une application de plateforme Windows universelle (UWP) en C++ et afficher une couleur unie.

## <a name="prerequisites"></a>Prérequis


Nous partons du principe que vous êtes familiarisé avec C++. Vous avez également besoin d’une expérience de base dans les concepts de programmation graphique.

**Durée de réalisation :** 20 minutes.

## <a name="instructions"></a>Instructions

### <a name="1-declaring-direct3d-interface-variables-with-comptr"></a>1. Déclaration de variables d’interface Direct3D à l’aide de ComPtr

Nous déclarons les variables d’interface Direct3D à l’aide du modèle de [pointeur intelligent](/cpp/cpp/smart-pointers-modern-cpp) ComPtr issu de la bibliothèque de modèles C++ Windows Runtime (WRL) afin de pouvoir gérer la durée de vie de ces variables en parant à toute exception. Nous pouvons ensuite utiliser ces variables pour accéder à la classe [**ComPtr class**](/cpp/windows/comptr-class) et à ses membres. Par exemple :

```cpp
    ComPtr<ID3D11RenderTargetView> m_renderTargetView;
    m_d3dDeviceContext->OMSetRenderTargets(
        1,
        m_renderTargetView.GetAddressOf(),
        nullptr // Use no depth stencil.
        );
```

Si vous déclarez [**ID3D11RenderTargetView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) avec ComPtr, vous pouvez utiliser la méthode **getaddressof,** de ComPtr pour que l’adresse du pointeur **ID3D11RenderTargetView** ( \* \* ID3D11RenderTargetView) passe à [**ID3D11DeviceContext :: OMSetRenderTargets**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets). **OMSetRenderTargets** établit la liaison entre la cible de rendu et l’[étape de fusion/sortie](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-merger-stage) pour indiquer la cible de rendu comme cible de sortie.

Après le démarrage de l’exemple d’application, celui-ci s’initialise et se charge, puis est ensuite prêt à s’exécuter.

### <a name="2-creating-the-direct3d-device"></a>2. Création du périphérique Direct3D

Pour utiliser l’API Direct3D pour effectuer le rendu d’une scène, nous devons créer tout d’abord un périphérique Direct3D qui représente la carte graphique. Pour créer le périphérique Direct3D, nous devons appeler la fonction [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice). Nous spécifions les niveaux 9,1 à 11,1 dans le tableau des valeurs de [** \_ \_ niveau de fonctionnalité D3D**](/windows/desktop/api/d3dcommon/ne-d3dcommon-d3d_feature_level) . Direct3D parcourt le tableau dans l’ordre et retourne le niveau de fonctionnalité le plus élevé pris en charge. Par conséquent, pour que le niveau de fonctionnalité le plus élevé soit disponible, nous répertorions les entrées de tableau de ** \_ \_ niveau de fonctionnalité D3D** de la plus élevée à la plus faible. Nous passons l’indicateur de [** \_ \_ \_ \_ prise en charge d3d11 Create Device BGRA**](/windows/desktop/api/d3d11/ne-d3d11-d3d11_create_device_flag) au paramètre *Flags* pour que les ressources Direct3D interagissent avec Direct2D. Si nous utilisons la version Debug, nous passons également l’indicateur de [** \_ \_ \_ débogage d3d11 Create Device**](/windows/desktop/api/d3d11/ne-d3d11-d3d11_create_device_flag) . Pour plus d’informations sur le débogage d’applications, voir [Utilisation de la couche de débogage pour déboguer des applications](/windows/desktop/direct3d11/using-the-debug-layer-to-test-apps).

Nous obtenons le périphérique Direct3D 11.1 ([**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1)) et le contexte du périphérique ([**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)) en interrogeant le périphérique et son contexte retournés de [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice).

```cpp
        // First, create the Direct3D device.

        // This flag is required in order to enable compatibility with Direct2D.
        UINT creationFlags = D3D11_CREATE_DEVICE_BGRA_SUPPORT;

#if defined(_DEBUG)
        // If the project is in a debug build, enable debugging via SDK Layers with this flag.
        creationFlags |= D3D11_CREATE_DEVICE_DEBUG;
#endif

        // This array defines the ordering of feature levels that D3D should attempt to create.
        D3D_FEATURE_LEVEL featureLevels[] =
        {
            D3D_FEATURE_LEVEL_11_1,
            D3D_FEATURE_LEVEL_11_0,
            D3D_FEATURE_LEVEL_10_1,
            D3D_FEATURE_LEVEL_10_0,
            D3D_FEATURE_LEVEL_9_3,
            D3D_FEATURE_LEVEL_9_1
        };

        ComPtr<ID3D11Device> d3dDevice;
        ComPtr<ID3D11DeviceContext> d3dDeviceContext;
        DX::ThrowIfFailed(
            D3D11CreateDevice(
                nullptr,                    // Specify nullptr to use the default adapter.
                D3D_DRIVER_TYPE_HARDWARE,
                nullptr,                    // leave as nullptr if hardware is used
                creationFlags,              // optionally set debug and Direct2D compatibility flags
                featureLevels,
                ARRAYSIZE(featureLevels),
                D3D11_SDK_VERSION,          // always set this to D3D11_SDK_VERSION
                &d3dDevice,
                nullptr,
                &d3dDeviceContext
                )
            );

        // Retrieve the Direct3D 11.1 interfaces.
        DX::ThrowIfFailed(
            d3dDevice.As(&m_d3dDevice)
            );

        DX::ThrowIfFailed(
            d3dDeviceContext.As(&m_d3dDeviceContext)
            );
```

### <a name="3-creating-the-swap-chain"></a>3. Création de la chaîne d’échange

Nous créons ensuite une chaîne d’échange que le périphérique utilise pour le rendu et l’affichage. Nous déclarons et initialiserons une structure [** \_ DESC1 de \_ chaîne \_ de permutation dxgi**](/windows/desktop/api/dxgi1_2/ns-dxgi1_2-dxgi_swap_chain_desc1) pour décrire la chaîne de permutation. Ensuite, nous définissons la chaîne de permutation comme « Flip-Model » (autrement dit, une chaîne de permutation dont l' [**effet d’échange dxgi retourne une valeur \_ \_ \_ \_ séquentielle**](/windows/desktop/api/dxgi/ne-dxgi-dxgi_swap_effect) définie dans le membre **SwapEffect** ) et vous définissez le membre de **format** sur [**dxgi \_ \_ B8G8R8A8 \_ UNORM**](/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format). Nous avons défini le membre **Count** de la structure [** \_ exemple \_ desc dxgi**](/windows/desktop/api/dxgicommon/ns-dxgicommon-dxgi_sample_desc) que le membre **SampleDesc** spécifie à 1 et le membre **Quality** de **dxgi \_ Sample \_ desc** à zéro, car Flip-Model ne prend pas en charge plusieurs exemples d’anticrénelage (MSAA). Nous attribuons au membre **BufferCount** la valeur 2 afin que la chaîne d’échange puisse utiliser un tampon d’affichage à présenter au périphérique d’affichage et une mémoire tampon d’arrière-plan qui sert de cible de rendu.

Nous obtenons le périphérique DXGI sous-jacent en interrogeant le périphérique Direct3D 11.1. Pour réduire le plus possible la consommation d’énergie, ce qui est important sur les périphériques à batterie tels que les ordinateurs portables et les tablettes, nous appelons la méthode [**IDXGIDevice1::SetMaximumFrameLatency**](/windows/desktop/api/dxgi/nf-dxgi-idxgidevice1-setmaximumframelatency) avec la valeur 1 comme nombre maximal d’images de mémoire tampon d’arrière-plan que l’infrastructure DXGI peut placer en file d’attente. Cela permet de s’assurer que le rendu de l’application ne se fait qu’après le vide vertical.

Pour créer enfin la chaîne d’échange, nous devons obtenir la fabrique parente du périphérique DXGI. Nous appelons [**IDXGIDevice :: GetAdapter**](/windows/desktop/api/dxgi/nf-dxgi-idxgidevice-getadapter) pour obtenir l’adaptateur pour l’appareil, puis appelons [**IDXGIObject :: GetParent**](/windows/desktop/api/dxgi/nf-dxgi-idxgiobject-getparent) sur l’adaptateur pour obtenir la fabrique parente ([**IDXGIFactory2**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2)). Pour créer la chaîne de permutation, nous appelons [**IDXGIFactory2 :: CreateSwapChainForCoreWindow**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow) avec le descripteur de chaîne d’échange et la fenêtre principale de l’application.

```cpp
            // If the swap chain does not exist, create it.
            DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};

            swapChainDesc.Stereo = false;
            swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
            swapChainDesc.Scaling = DXGI_SCALING_NONE;
            swapChainDesc.Flags = 0;

            // Use automatic sizing.
            swapChainDesc.Width = 0;
            swapChainDesc.Height = 0;

            // This is the most common swap chain format.
            swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;

            // Don't use multi-sampling.
            swapChainDesc.SampleDesc.Count = 1;
            swapChainDesc.SampleDesc.Quality = 0;

            // Use two buffers to enable the flip effect.
            swapChainDesc.BufferCount = 2;

            // We recommend using this swap effect for all applications.
            swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL;


            // Once the swap chain description is configured, it must be
            // created on the same adapter as the existing D3D Device.

            // First, retrieve the underlying DXGI Device from the D3D Device.
            ComPtr<IDXGIDevice2> dxgiDevice;
            DX::ThrowIfFailed(
                m_d3dDevice.As(&dxgiDevice)
                );

            // Ensure that DXGI does not queue more than one frame at a time. This both reduces
            // latency and ensures that the application will only render after each VSync, minimizing
            // power consumption.
            DX::ThrowIfFailed(
                dxgiDevice->SetMaximumFrameLatency(1)
                );

            // Next, get the parent factory from the DXGI Device.
            ComPtr<IDXGIAdapter> dxgiAdapter;
            DX::ThrowIfFailed(
                dxgiDevice->GetAdapter(&dxgiAdapter)
                );

            ComPtr<IDXGIFactory2> dxgiFactory;
            DX::ThrowIfFailed(
                dxgiAdapter->GetParent(IID_PPV_ARGS(&dxgiFactory))
                );

            // Finally, create the swap chain.
            CoreWindow^ window = m_window.Get();
            DX::ThrowIfFailed(
                dxgiFactory->CreateSwapChainForCoreWindow(
                    m_d3dDevice.Get(),
                    reinterpret_cast<IUnknown*>(window),
                    &swapChainDesc,
                    nullptr, // Allow on all displays.
                    &m_swapChain
                    )
                );
```

### <a name="4-creating-the-render-target-view"></a>4. Création de l’affichage de cible de rendu

Pour effectuer le rendu de graphismes dans la fenêtre, nous devons créer un affichage de cible de rendu. Nous appelons [**IDXGISwapChain :: GetBuffer**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-getbuffer) pour obtenir la mémoire tampon d’arrière-plan de la chaîne d’échange à utiliser lors de la création de l’affichage de la cible de rendu. Nous spécifions la mémoire tampon d’arrière-plan en tant que texture 2D ([**ID3D11Texture2D**](/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d)). Pour créer l’affichage de cible de rendu, nous appelons [**ID3D11Device::CreateRenderTargetView**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createrendertargetview) avec la mémoire tampon d’arrière-plan de la chaîne d’échange. Nous devons spécifier pour dessiner dans l’intégralité de la fenêtre principale en spécifiant le port de vue ([**d3d11 \_ VIEWPORT**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_viewport)) comme taille complète de la mémoire tampon d’arrière-plan de la chaîne d’échange. Nous utilisons la fenêtre d’affichage dans un appel à [**ID3D11DeviceContext::RSSetViewports**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetviewports) pour lier la fenêtre d’affichage à l’[étape du module de rastérisation](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-rasterizer-stage) du pipeline. L’étape du module de rastérisation convertit les informations sur les vecteurs en une image raster. Dans ce cas, nous n’avons pas besoin de conversion, car nous cherchons simplement à afficher une couleur unie.

```cpp
        // Once the swap chain is created, create a render target view.  This will
        // allow Direct3D to render graphics to the window.

        ComPtr<ID3D11Texture2D> backBuffer;
        DX::ThrowIfFailed(
            m_swapChain->GetBuffer(0, IID_PPV_ARGS(&backBuffer))
            );

        DX::ThrowIfFailed(
            m_d3dDevice->CreateRenderTargetView(
                backBuffer.Get(),
                nullptr,
                &m_renderTargetView
                )
            );


        // After the render target view is created, specify that the viewport,
        // which describes what portion of the window to draw to, should cover
        // the entire window.

        D3D11_TEXTURE2D_DESC backBufferDesc = {0};
        backBuffer->GetDesc(&backBufferDesc);

        D3D11_VIEWPORT viewport;
        viewport.TopLeftX = 0.0f;
        viewport.TopLeftY = 0.0f;
        viewport.Width = static_cast<float>(backBufferDesc.Width);
        viewport.Height = static_cast<float>(backBufferDesc.Height);
        viewport.MinDepth = D3D11_MIN_DEPTH;
        viewport.MaxDepth = D3D11_MAX_DEPTH;

        m_d3dDeviceContext->RSSetViewports(1, &viewport);
```

### <a name="5-presenting-the-rendered-image"></a>5. Présentation de l’image rendue

L’exécution du code entre dans une boucle sans fin pour effectuer le rendu de façon continue et afficher la scène.

Dans cette boucle, nous appelons :

1.  [**ID3D11DeviceContext::OMSetRenderTargets**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) pour préciser que la cible de sortie correspond à la cible de rendu ;
2.  [**ID3D11DeviceContext::ClearRenderTargetView**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-clearrendertargetview) pour effacer la cible de rendu et lui attribuer une couleur unie bleue ;
3.  [**IDXGISwapChain ::P renvoyé**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) pour présenter l’image rendue à la fenêtre.

Étant donné que nous avons préalablement défini une latence d’image maximale de 1, Windows ralentit habituellement la boucle de rendu pour la caler à la fréquence de rafraîchissement de l’écran, généralement autour de 60 Hz. Windows ralentit la boucle de rendu en mettant l’application en veille quand l’application appelle la [**présente**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present). Windows met l’application en veille jusqu’à ce que l’écran soit rafraîchi.

```cpp
        // Enter the render loop.  Note that UWP apps should never exit.
        while (true)
        {
            // Process events incoming to the window.
            m_window->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            // Specify the render target we created as the output target.
            m_d3dDeviceContext->OMSetRenderTargets(
                1,
                m_renderTargetView.GetAddressOf(),
                nullptr // Use no depth stencil.
                );

            // Clear the render target to a solid color.
            const float clearColor[4] = { 0.071f, 0.04f, 0.561f, 1.0f };
            m_d3dDeviceContext->ClearRenderTargetView(
                m_renderTargetView.Get(),
                clearColor
                );

            // Present the rendered image to the window.  Because the maximum frame latency is set to 1,
            // the render loop will generally be throttled to the screen refresh rate, typically around
            // 60 Hz, by sleeping the application on Present until the screen is refreshed.
            DX::ThrowIfFailed(
                m_swapChain->Present(1, 0)
                );
        }
```

### <a name="6-resizing-the-app-window-and-the-swap-chains-buffer"></a>6. Redimensionnement de la fenêtre d’application et du tampon de la chaîne d’échange

Si la taille de la fenêtre de l’application change, l’application doit redimensionner les tampons de la chaîne d’échange, recréer l’affichage de cible de rendu, puis présenter l’image rendue redimensionnée. Pour redimensionner les tampons de la chaîne d’échange, nous appelons [**IDXGISwapChain :: ResizeBuffers**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-resizebuffers). Dans cet appel, nous laissons le nombre de mémoires tampons et le format des mémoires tampons inchangés (le paramètre *BufferCount* en deux et le paramètre *NewFormat* au [** \_ format dxgi \_ B8G8R8A8 \_ UNORM**](/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format)). Nous calquons la taille de la mémoire tampon d’arrière-plan de la chaîne d’échange sur la taille de la fenêtre redimensionnée. Après le redimensionnement des tampons, nous devons créer la nouvelle cible de rendu et présenter la nouvelle image rendue de façon similaire à l’initialisation de l’application.

```cpp
            // If the swap chain already exists, resize it.
            DX::ThrowIfFailed(
                m_swapChain->ResizeBuffers(
                    2,
                    0,
                    0,
                    DXGI_FORMAT_B8G8R8A8_UNORM,
                    0
                    )
                );
```

## <a name="summary-and-next-steps"></a>Résumé et étapes suivantes


Nous avons créé un périphérique Direct3D, une chaîne d’échange et un affichage de cible de rendu, puis avons présenté l’image rendue à l’écran.

À présent, nous devons également tracer un triangle à l’écran.

[Création de nuanceurs et traçage de primitives](creating-shaders-and-drawing-primitives.md)

 

 