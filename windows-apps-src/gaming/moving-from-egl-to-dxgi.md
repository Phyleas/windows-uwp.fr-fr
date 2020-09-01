---
title: Comparer le code EGL avec DXGI et Direct3D
description: L’interface graphique DirectX (DXGI) et certaines API Direct3D jouent le même rôle qu’EGL. Cette rubrique vous aidera à comprendre le fonctionnement de DXGI et Direct3D 11 sous l’ange d’EGL.
ms.assetid: 90f5ecf1-dd5d-fea3-bed8-57a228898d2a
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, EGL, DXGI, Direct3D
ms.localizationpriority: medium
ms.openlocfilehash: a9e521810c5220d412afb98d25a00f5ac499af1b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165213"
---
# <a name="compare-egl-code-to-dxgi-and-direct3d"></a>Comparer le code EGL avec DXGI et Direct3D




**API importantes**

-   [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1)
-   [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)
-   [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow)

L’interface graphique DirectX (DXGI) et certaines API Direct3D jouent le même rôle qu’EGL. Cette rubrique vous aidera à comprendre le fonctionnement de DXGI et Direct3D 11 sous l’ange d’EGL.

À l’instar d’EGL, DXGI et Direct3D fournissent diverses méthodes qui vous permettent de configurer des ressources graphiques, d’obtenir un contexte de rendu pour le dessin des nuanceurs et d’afficher les résultats dans une fenêtre. Toutefois, DXGI et Direct3D comportent quelques options supplémentaires qui nécessitent une configuration particulière dans le cadre du portage à partir d’EGL.

> **Remarque**    Ce guide est basé sur la spécification ouverte du groupe Khronos pour EGL 1,4, disponible ici : [Khronos Native Platform Graphics interface (EGL Version 1,4-6 avril 2011) \[ PDF \] ](https://www.khronos.org/registry/egl/specs/eglspec.1.4.20110406.pdf). Les variantes de syntaxe propres à d’autres plateformes et langages de développement ne sont pas traitées dans cette rubrique.

 

## <a name="how-does-dxgi-and-direct3d-compare"></a>Comparaison avec DXGI et Direct3D


Avec EGL, il est relativement facile de commencer à dessiner dans une surface de fenêtre. C’est l’un des atouts d’EGL par rapport à DXGI et Direct3D. Cela s’explique par le fait qu’OpenGL ES 2.0 (et donc EGL) est une spécification implémentée par de nombreux fournisseurs de plateforme, alors que DXGI et Direct3D constituent une référence unique à laquelle les pilotes des fournisseurs de matériel doivent se conformer. Microsoft doit donc implémenter un ensemble d’API capables de prendre en charge le plus de fonctionnalités tierces possibles, au lieu de se limiter à un groupe de fonctionnalités d’un fournisseur donné ou encore de combiner des commandes d’installation propres à un fournisseur dans des API plus simples. À l’inverse, Direct3D offre un ensemble unique d’API capables de prendre en charge une très grande variété de plateformes matérielles vidéo et de niveaux de fonctionnalité. En cela, Direct3D se montre plus flexible pour les développeurs sachant parfaitement utiliser la plateforme en question.

Comme EGL, DXGI et Direct3D fournissent des API pour les opérations suivantes :

-   Obtention des données, et lecture/écriture de ces données dans un tampon de trame (appelé « chaîne de permutation » dans DXGI).
-   Association du tampon de trame avec une fenêtre d’interface utilisateur.
-   Obtention et configuration des contextes de rendu destinés au dessin.
-   Envoi des commandes au pipeline graphique pour un contexte de rendu spécifique.
-   Création et gestion des ressources des nuanceurs, et association de ces dernières avec un contexte de rendu.
-   Affichage sur des cibles de rendu spécifiques (telles que les textures).
-   Actualisation de la surface d’affichage de la fenêtre afin de présenter les résultats du rendu avec les ressources graphiques.

Pour connaître le processus général de configuration du pipeline graphique dans Direct3D, reportez-vous au modèle d’application DirectX 11 (Windows universelle), disponible dans Microsoft Visual Studio 2015. La classe de rendu de base fournie dans ce modèle peut servir de référence pour créer l’infrastructure graphique Direct3D 11 et y configurer les principales ressources nécessaires. Elle offre également une prise en charge de certaines fonctionnalités des applications de plateforme Windows universelle (UWP), telles que la rotation de l’écran.

EGL comprend très peu d’API en comparaison de Direct3D 11. La navigation entre les différentes API de Direct3D 11 peut se révéler compliquée si vous n’êtes pas familiarisé avec les noms et le langage technique propres à cette plateforme. Nous vous proposons ici une vue d’ensemble pour vous aider à démarrer.

En premier lieu, sachez identifier à quel élément d’interface Direct3D correspond chaque objet EGL de base :

| Abstraction EGL | Représentation Direct3D similaire                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **EGLDisplay**  | Dans Direct3D (pour les applications UWP), le handle d’affichage est fourni par l’API [**Windows::UI::CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) (ou l’interface **ICoreWindowInterop** qui expose le HWND). La carte et le matériel sont configurés par le biais des interfaces COM [**IDXGIAdapter**](/windows/desktop/api/dxgi/nn-dxgi-idxgiadapter) et [**IDXGIDevice1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgidevice2), respectivement.                                                                                                                                                                                                                                                           |
| **EGLSurface**  | Dans Direct3D, les mémoires tampons et les autres ressources de fenêtre (visibles ou hors écran) sont créées et configurées par des interfaces DXGI spécifiques, y compris [**IDXGIFactory2**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2) (une implémentation de modèle de fabrique utilisée pour acquérir des ressources dxgi comme[**IDXGISwapChain1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1) (mémoires tampons d’affichage). [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1), qui représente le périphérique graphique et ses ressources, s’obtient avec [**D3D11Device::CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice). Pour les cibles de rendu, utilisez l’interface [**ID3D11RenderTargetView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview). |
| **EGLContext**  | Dans Direct3D, utilisez l’interface [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) pour définir et envoyer des commandes au pipeline graphique.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| **EGLConfig**   | Dans Direct3D 11, utilisez les méthodes de l’interface [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) pour créer et configurer les ressources graphiques, telles que les mémoires tampons, les textures, les gabarits et les nuanceurs.                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

 

Voici maintenant la procédure générale pour créer un affichage graphique simple, des ressources et un contexte dans DXGI et Direct3D pour une application UWP.

1.  Obtenez un handle vers l’objet [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) pour le thread d’interface utilisateur principal de l’application en appelant [**CoreWindow :: GetForCurrentThread**](/uwp/api/windows.ui.core.corewindow.getforcurrentthread).
2.  Pour les applications UWP, demandez une chaîne de permutation à [**IDXGIAdapter2**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiadapter2) à l’aide de la méthode [**IDXGIFactory2::CreateSwapChainForCoreWindow**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow), puis transmettez-la à la référence [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) obtenue à l’étape 1. Vous obtiendrez une instance [**IDXGISwapChain1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1) en retour. Définissez l’étendue de cette instance à l’objet convertisseur et au thread de rendu associé.
3.  Obtenez des instances [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) et [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) en appelant la méthode [**D3D11Device :: CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) . Définissez également l’étendue de ces instances à l’objet convertisseur.
4.  Créez les nuanceurs, textures et autres ressources à l’aide des méthodes sur l’objet [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) du convertisseur.
5.  Définir des mémoires tampons, exécuter des nuanceurs et gérer les étapes de pipeline à l’aide de méthodes sur l’objet [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) de votre convertisseur.
6.  Lorsque le pipeline a été exécuté et qu’un frame est dessiné vers la mémoire tampon d’arrière-plan, présentez-le à l’écran avec [**IDXGISwapChain1 ::P resent1**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1).

Pour plus d’informations sur ce processus, consultez [Prise en main de DirectX Graphics](/windows/desktop/getting-started-with-directx-graphics). La suite du présent article couvre la plupart des étapes que vous aurez généralement à effectuer pour configurer et gérer un pipeline graphique de base.
> **Remarque**    Les applications de bureau Windows ont des API différentes pour l’obtention d’une chaîne de permutation Direct3D, telle que [**D3D11Device :: CreateDeviceAndSwapChain**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdeviceandswapchain), et n’utilisent pas un objet [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) .

 

## <a name="obtaining-a-window-for-display"></a>Obtention d’une fenêtre d’affichage


Dans l’exemple ci-dessous, un HWND est transmis à eglGetDisplay pour obtenir une ressource de fenêtre adaptée à la plateforme Microsoft Windows. D’autres plateformes, comme iOS d’Apple (Cocoa) et Android de Google, utilisent des handles ou références différents vers les ressources de fenêtre. La syntaxe d’appel requise peut donc varier. Une fois que vous avez obtenu un affichage, vous devez l’initialiser, définir la configuration par défaut, puis créer une surface avec une mémoire tampon d’arrière-plan destinée au dessin.

Obtention d’un affichage et configuration de ce dernier avec EGL :

``` syntax
// Obtain an EGL display object.
EGLDisplay display = eglGetDisplay(GetDC(hWnd));
if (display == EGL_NO_DISPLAY)
{
  return EGL_FALSE;
}

// Initialize the display
if (!eglInitialize(display, &majorVersion, &minorVersion))
{
  return EGL_FALSE;
}

// Obtain the display configs
if (!eglGetConfigs(display, NULL, 0, &numConfigs))
{
  return EGL_FALSE;
}

// Choose the display config
if (!eglChooseConfig(display, attribList, &config, 1, &numConfigs))
{
  return EGL_FALSE;
}

// Create a surface
surface = eglCreateWindowSurface(display, config, (EGLNativeWindowType)hWnd, NULL);
if (surface == EGL_NO_SURFACE)
{
  return EGL_FALSE;
}
```

Dans Direct3D, la fenêtre principale d’une application UWP est représentée par l’objet [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) , qui peut être obtenu à partir de l’objet App en appelant [**CoreWindow :: GetForCurrentThread**](/uwp/api/windows.ui.core.corewindow.getforcurrentthread) dans le cadre du processus d’initialisation du « fournisseur d’affichage » que vous construisez pour Direct3D. (Avec l’interopérabilité Direct3D-XAML, vous utilisez le fournisseur d’affichage de l’infrastructure XAML.) Pour plus d’informations sur la création d’un fournisseur d’affichage Direct3D, voir la rubrique [Comment configurer votre application pour présenter un affichage](/previous-versions/windows/apps/hh465077(v=win.10)).

Obtention d’un objet CoreWindow pour Direct3D :

``` syntax
CoreWindow::GetForCurrentThread();
```

Après avoir obtenu la référence [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow), activez la fenêtre afin que celle-ci exécute la méthode **Run** de votre objet principal et commence le traitement de l’événement fenêtre. Après cela, créez un [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) et un [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1), et utilisez-les pour obtenir le [**IDXGIDevice1**](/windows/desktop/api/dxgi/nn-dxgi-idxgidevice1) et le [**IDXGIAdapter**](/windows/desktop/api/dxgi/nn-dxgi-idxgiadapter) sous-jacents afin de pouvoir obtenir un objet [**IDXGIFactory2**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2) pour créer une ressource de chaîne de permutation basée sur la configuration dxgi de [** \_ \_ chaîne \_ de permutation**](/windows/desktop/api/dxgi1_2/ns-dxgi1_2-dxgi_swap_chain_desc1) DESC1.

Configuration et définition de la chaîne de permutation DXGI sur l’objet CoreWindow pour Direct3D :

``` syntax
// Called when the CoreWindow object is created (or re-created).
void SimpleDirect3DApp::SetWindow(CoreWindow^ window)
{
  // Register event handlers with the CoreWindow object.
  // ...

  // Obtain your ID3D11Device1 and ID3D11DeviceContext1 objects
  // In this example, m_d3dDevice contains the scoped ID3D11Device1 object
  // ...

  ComPtr<IDXGIDevice1>  dxgiDevice;
  // Get the underlying DXGI device of the Direct3D device.
  m_d3dDevice.As(&dxgiDevice);

  ComPtr<IDXGIAdapter> dxgiAdapter;
  dxgiDevice->GetAdapter(&dxgiAdapter);

  ComPtr<IDXGIFactory2> dxgiFactory;
  dxgiAdapter->GetParent(
    __uuidof(IDXGIFactory2), 
    &dxgiFactory);

  DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};
  swapChainDesc.Width = static_cast<UINT>(m_d3dRenderTargetSize.Width); // Match the size of the window.
  swapChainDesc.Height = static_cast<UINT>(m_d3dRenderTargetSize.Height);
  swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM; // This is the most common swap chain format.
  swapChainDesc.Stereo = false;
  swapChainDesc.SampleDesc.Count = 1; // Don't use multi-sampling.
  swapChainDesc.SampleDesc.Quality = 0;
  swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
  swapChainDesc.BufferCount = 2; // Use double-buffering to minimize latency.
  swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL; // All UWP apps must use this SwapEffect.
  swapChainDesc.Flags = 0;

  // ...

  Windows::UI::Core::CoreWindow^ window = m_window.Get();
  dxgiFactory->CreateSwapChainForCoreWindow(
    m_d3dDevice.Get(),
    reinterpret_cast<IUnknown*>(window),
    &swapChainDesc,
    nullptr, // Allow on all displays.
    &m_swapChainCoreWindow);
}
```

Appelez la méthode [**IDXGISwapChain1::Present1**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1) après avoir préparé la trame à afficher.

Notez que Direct3D 11 ne propose pas d’abstraction identique à EGLSurface. ([**IDXGISurface1**](/windows/desktop/api/dxgi/nn-dxgi-idxgisurface1) existe, mais son utilisation diffère.) La représentation conceptuelle la plus proche est l’objet [**ID3D11RenderTargetView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview), qui permet de définir une texture ([**ID3D11Texture2D**](/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d)) en tant que mémoire tampon d’arrière-plan dans laquelle le pipeline de nuanceurs pourra dessiner.

Configuration de la mémoire tampon d’arrière-plan pour la chaîne de permutation dans Direct3D 11 :

``` syntax
ComPtr<ID3D11RenderTargetView>    m_d3dRenderTargetViewWin; // scoped to renderer object

// ...

ComPtr<ID3D11Texture2D> backBuffer2;
    
m_swapChainCoreWindow->GetBuffer(0, IID_PPV_ARGS(&backBuffer2));

m_d3dDevice->CreateRenderTargetView(
  backBuffer2.Get(),
  nullptr,
    &m_d3dRenderTargetViewWin);
```

Nous vous recommandons d’appeler ce code dès que vous créez une fenêtre ou modifiez une taille de fenêtre. Lors du processus de rendu, définissez l’affichage de la cible de rendu à l’aide de la méthode [**ID3D11DeviceContext1::OMSetRenderTargets**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets). Effectuez cette opération avant de configurer les autres sous-ressources, telles que les mémoires tampons de vertex ou les nuanceurs.

``` syntax
// Set the render target for the draw operation.
m_d3dContext->OMSetRenderTargets(
        1,
        d3dRenderTargetView.GetAddressOf(),
        nullptr);
```

## <a name="creating-a-rendering-context"></a>Création d’un contexte de rendu


Dans EGL 1.4, un « affichage » représente un ensemble de ressources de fenêtre. En règle générale, vous configurez une « surface » d’affichage en affectant un ensemble d’attributs à l’objet d’affichage et en obtenant une surface en retour. Pour afficher le contenu de la surface, vous devez créer un contexte, puis le lier à la surface et à l’affichage.

Votre flux d’appels se présente en principe comme suit :

-   Appelez eglGetDisplay avec le handle vers une ressource d’affichage ou de fenêtre, puis obtenez un objet d’affichage.
-   Initialisez l’affichage avec eglInitialize.
-   Obtenez les configurations d’affichage disponibles et sélectionnez-en une à l’aide des méthodes eglGetConfigs et eglChooseConfig.
-   Créez une surface d’affichage avec eglCreateWindowSurface.
-   Créez un contexte d’affichage pour le dessin en utilisant eglCreateContext.
-   Liez le contexte d’affichage à l’affichage et à la surface avec eglMakeCurrent.

Dans la section précédente, nous avons créé les objets EGLDisplay et EGLSurface. Nous allons maintenant utiliser EGLDisplay pour créer un contexte et l’associer à l’affichage, à l’aide de l’objet EGLSurface configuré pour paramétrer la sortie.

Obtention d’un contexte de rendu avec EGL 1.4 :

```cpp
// Configure your EGLDisplay and obtain an EGLSurface here ...
// ...

// Create a drawing context from the EGLDisplay
context = eglCreateContext(display, config, EGL_NO_CONTEXT, contextAttribs);
if (context == EGL_NO_CONTEXT)
{
  return EGL_FALSE;
}   
   
// Make the context current
if (!eglMakeCurrent(display, surface, surface, context))
{
  return EGL_FALSE;
}
```

Dans Direct3D 11, un contexte de rendu est représenté par deux objets : l’objet [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1), qui représente la carte et sert à créer des ressources pour Direct3D (par exemple, les mémoires tampons d’arrière-plan et les nuanceurs) ; l’objet [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1), que vous utilisez pour gérer le pipeline graphique et exécuter les nuanceurs.

N’oubliez pas que les niveaux de fonctionnalité Direct3D sont destinés à garantir la prise en charge des versions antérieures des plateformes matérielles Direct3D (DirectX 9.1 à DirectX 11). De nombreuses plateformes fonctionnant avec du matériel vidéo à faible consommation d’énergie, comme les tablettes, n’ont accès qu’aux fonctionnalités DirectX 9.1. En outre, la prise en charge du matériel vidéo est assurée pour les versions 9.1 à 11.

Création d’un contexte de rendu avec DXGI et Direct3D

```cpp

// ... 

UINT creationFlags = D3D11_CREATE_DEVICE_BGRA_SUPPORT;
ComPtr<IDXGIDevice> dxgiDevice;

D3D_FEATURE_LEVEL featureLevels[] = 
{
        D3D_FEATURE_LEVEL_11_1,
        D3D_FEATURE_LEVEL_11_0,
        D3D_FEATURE_LEVEL_10_1,
        D3D_FEATURE_LEVEL_10_0,
        D3D_FEATURE_LEVEL_9_3,
        D3D_FEATURE_LEVEL_9_2,
        D3D_FEATURE_LEVEL_9_1
};

// Create the Direct3D 11 API device object and a corresponding context.
ComPtr<ID3D11Device> device;
ComPtr<ID3D11DeviceContext> d3dContext;

D3D11CreateDevice(
  nullptr, // Specify nullptr to use the default adapter.
  D3D_DRIVER_TYPE_HARDWARE,
  nullptr,
  creationFlags, // Set set debug and Direct2D compatibility flags.
  featureLevels, // List of feature levels this app can support.
  ARRAYSIZE(featureLevels),
  D3D11_SDK_VERSION, // Always set this to D3D11_SDK_VERSION for UWP apps.
  &device, // Returns the Direct3D device created.
  &m_featureLevel, // Returns feature level of device created.
  &d3dContext // Returns the device immediate context.
);
```

## <a name="drawing-into-a-texture-or-pixmap-resource"></a>Dessin dans une ressource de type texture ou pixmap


Pour dessiner dans une texture avec OpenGL ES 2.0, configurez une mémoire tampon de pixels (PBuffer). Après avoir créé et configuré un EGLSurface pour cette mémoire tampon, affectez-lui un contexte de rendu, puis exécutez le pipeline de nuanceurs qui dessinera dans la texture.

Dessin dans une mémoire tampon de pixels avec OpenGL ES 2.0 :

``` syntax
// Create a pixel buffer surface to draw into
EGLConfig pBufConfig;
EGLint totalpBufAttrs;

const EGLint pBufConfigAttrs[] =
{
    // Configure the pBuffer here...
};
 
eglChooseConfig(eglDsplay, pBufConfigAttrs, &pBufConfig, 1, &totalpBufAttrs);
EGLSurface pBuffer = eglCreatePbufferSurface(eglDisplay, pBufConfig, EGL_TEXTURE_RGBA); 
```

Dans Direct3D 11, créez une ressource [**ID3D11Texture2D**](/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d), puis définissez-la en tant que cible de rendu. Configurez la cible de rendu à l’aide de la [** \_ vue de cible de rendu d3d11 \_ \_ \_ desc**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_render_target_view_desc). Quand vous appelez la méthode [**ID3D11DeviceContext ::D RAW**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw) (ou une opération de dessin similaire \* sur le contexte de périphérique) à l’aide de cette cible de rendu, les résultats sont dessinés dans une texture.

Dessin dans une texture avec Direct3D 11 :

``` syntax
ComPtr<ID3D11Texture2D> renderTarget1;

D3D11_RENDER_TARGET_VIEW_DESC renderTargetDesc = {0};
// Configure renderTargetDesc here ...

m_d3dDevice->CreateRenderTargetView(
  renderTarget1.Get(),
  nullptr,
  &m_d3dRenderTargetViewWin);

// Later, in your render loop...

// Set the render target for the draw operation.
m_d3dContext->OMSetRenderTargets(
        1,
        d3dRenderTargetView.GetAddressOf(),
        nullptr);
```

Cette texture peut être transmise à un nuanceur si elle est associée à un objet [**ID3D11ShaderResourceView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11shaderresourceview).

## <a name="drawing-to-the-screen"></a>Dessin à l’écran


Une fois que vous avez configuré vos mémoires tampons et mis à jour vos données à l’aide de votre objet EGLContext, exécutez les nuanceurs liés à ce dernier, puis dessinez les résultats dans la mémoire tampon d’arrière-plan avec glDrawElements. Appelez eglSwapBuffers pour afficher la mémoire tampon d’arrière-plan.

Dessin à l’écran avec OpenGL ES 2.0 :

``` syntax
glDrawElements(GL_TRIANGLES, renderer->numIndices, GL_UNSIGNED_INT, 0);

eglSwapBuffers(drawContext->eglDisplay, drawContext->eglSurface);
```

Dans Direct3D 11, vous configurez vos mémoires tampons et liez les nuanceurs à votre [**IDXGISwapChain ::P resent1**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1). Ensuite, vous appelez l’une des méthodes [**ID3D11DeviceContext1 ::D RAW**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw) \* pour exécuter les nuanceurs et dessiner les résultats sur une cible de rendu configurée comme mémoire tampon d’arrière-plan pour la chaîne de permutation. Enfin, vous présentez la mémoire tampon d’arrière-plan à l’affichage en appelant la méthode **IDXGISwapChain::Present1**.

Dessin à l’écran avec Direct3D 11 :

``` syntax

m_d3dContext->DrawIndexed(
        m_indexCount,
        0,
        0);

// ...

m_swapChainCoreWindow->Present1(1, 0, &parameters);
```

## <a name="releasing-graphics-resources"></a>Libération des ressources graphiques


Dans EGL, vous libérez les ressources de fenêtre en transmettant EGLDisplay à eglTerminate.

Arrêt d’un affichage avec EGL 1.4 :

```cpp
EGLBoolean eglTerminate(eglDisplay);
```

Dans une application UWP, vous pouvez fermer le CoreWindow avec [**CoreWindow::Close**](/uwp/api/windows.ui.core.corewindow.close). Cela n’est possible que pour les fenêtres d’interface utilisateur secondaires. Le thread d’interface utilisateur principal et le CoreWindow associé ne peuvent pas être fermés de la sorte. C’est le système d’exploitation qui les classe comme ayant expiré. En revanche, la fermeture d’un CoreWindow secondaire déclenche l’événement [**CoreWindow::Closed**](/uwp/api/windows.ui.core.corewindow.closed).

## <a name="api-reference-mapping-for-egl-to-direct3d-11"></a>Index de mappage des API EGL et Direct3D 11


| API EGL                          | API ou comportement Direct3D 11 similaire                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| eglBindAPI                       | N/A.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| eglBindTexImage                  | Appelez [**ID3D11Device :: CreateTexture2D**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createtexture2d) pour définir une texture 2D.                                                                                                                                                                                                                                                                                                                                                                                          |
| eglChooseConfig                  | Direct3D ne fournit pas de jeux de configurations par défaut pour les tampons de trame. Configuration de la chaîne de permutation.                                                                                                                                                                                                                                                                                                                                                                                           |
| eglCopyBuffers                   | Pour copier des données de mémoire tampon, appelez [**ID3D11DeviceContext :: CopyStructureCount**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-copystructurecount). Pour copier une ressource, appelez la méthode [**ID3DDeviceCOntext::CopyResource**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-copyresource).                                                                                                                                                                                                                                                      |
| eglCreateContext                 | Créez un contexte de périphérique Direct3D en appelant [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice), qui renvoie un handle vers un périphérique Direct3D ainsi qu’un contexte immédiat Direct3D par défaut (objet [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)). Vous pouvez également créer un contexte différé Direct3D en appelant [**ID3D11Device2 :: CreateDeferredContext**](/windows/desktop/api/d3d11_2/nf-d3d11_2-id3d11device2-createdeferredcontext2) sur l’objet [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) retourné. |
| eglCreatePbufferFromClientBuffer | Chaque mémoire tampon est écrite et lue comme une sous-ressource Direct3D ([**ID3D11Texture2D**](/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d), par exemple). Copie d’un type de sous-ressource compatible avec une méthode telle que [**ID3D11DeviceContext1 : CopyResource**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-copyresource).                                                                                                                                                                                                     |
| eglCreatePbufferSurface          | Pour créer un appareil Direct3D sans chaîne de permutation, appelez la méthode statique [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) . Pour créer un affichage de la cible de rendu Direct3D, appelez la méthode [**ID3D11Device::CreateRenderTargetView**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createrendertargetview).                                                                                                                                                                                                                               |
| eglCreatePixmapSurface           | Pour créer un appareil Direct3D sans chaîne de permutation, appelez la méthode statique [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) . Pour créer un affichage de la cible de rendu Direct3D, appelez la méthode [**ID3D11Device::CreateRenderTargetView**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createrendertargetview).                                                                                                                                                                                                                               |
| eglCreateWindowSurface           | Ontain un [**IDXGISwapChain1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1) (pour les mémoires tampons d’affichage) et un [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) (une interface virtuelle pour le périphérique graphique et ses ressources). Utilisez **ID3D11Device1** pour définir un objet [**ID3D11RenderTargetView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) permettant ensuite de créer le tampon de trame requis par **IDXGISwapChain1**.                                                                                         |
| eglDestroyContext                | N/A. Utilisez [**ID3D11DeviceContext::DiscardView1**](/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-discardview1) pour supprimer un affichage de la cible de rendu. Pour fermer le [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)parent, affectez la valeur null à l’instance et attendez que la plateforme récupère ses ressources. Vous ne pouvez pas supprimer directement le contexte de périphérique.                                                                                                                                                |
| eglDestroySurface                | N/A. Les ressources graphiques sont supprimées lorsque la plateforme ferme le CoreWindow dans l’application UWP.                                                                                                                                                                                                                                                                                                                                                                                                 |
| eglGetCurrentDisplay             | Appelez [**CoreWindow::GetForCurrentThread**](/uwp/api/windows.ui.core.corewindow.getforcurrentthread) pour obtenir une référence à la fenêtre principale actuelle de l’application.                                                                                                                                                                                                                                                                                                                                                         |
| eglGetCurrentSurface             | Il s’agit de l’objet [**ID3D11RenderTargetView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) actuel, dont l’étendue est généralement définie sur l’objet de votre convertisseur.                                                                                                                                                                                                                                                                                                                                                         |
| eglGetError                      | Les erreurs sont obtenues sous forme de valeurs HRESULT, qui sont renvoyées par la plupart des méthodes sur les interfaces DirectX. Si la méthode ne retourne pas de valeur HRESULT, appelez [**GetLastError**](/windows/desktop/api/errhandlingapi/nf-errhandlingapi-getlasterror). Pour convertir une erreur système en valeur HRESULT, utilisez la macro [**HRESULT \_ de \_ Win32**](/windows/desktop/api/winerror/nf-winerror-hresult_from_win32)   .                                                                                                                                                                                                  |
| eglInitialize                    | Appelez [**CoreWindow::GetForCurrentThread**](/uwp/api/windows.ui.core.corewindow.getforcurrentthread) pour obtenir une référence à la fenêtre principale actuelle de l’application.                                                                                                                                                                                                                                                                                                                                                         |
| eglMakeCurrent                   | Définissez une cible de rendu pour le dessin sur le contexte actuel avec [**ID3D11DeviceContext1 :: OMSetRenderTargets**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets).                                                                                                                                                                                                                                                                                                                                  |
| eglQueryContext                  | N/A. Toutefois, vous pouvez acquérir des cibles de rendu à partir d’une instance [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) , ainsi que des données de configuration. Pour obtenir une liste des méthodes disponibles, cliquez sur le lien.                                                                                                                                                                                                                                                                                           |
| eglQuerySurface                  | N/A. Néanmoins, vous pouvez obtenir des données sur les fenêtres d’affichage et sur le matériel vidéo actuel à partir des méthodes d’une instance [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1). Pour obtenir une liste des méthodes disponibles, cliquez sur le lien.                                                                                                                                                                                                                                                                               |
| eglReleaseTexImage               | N/A.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| eglReleaseThread                 | Pour obtenir des informations générales sur le multithreading du GPU, voir la rubrique [Multithreading](/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-intro).                                                                                                                                                                                                                                                                                                                                                                              |
| eglSurfaceAttrib                 | Utiliser [**la \_ vue de cible de rendu d3d11 \_ \_ \_ desc**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_render_target_view_desc) pour configurer une vue de cible de rendu Direct3D,                                                                                                                                                                                                                                                                                                                                                               |
| eglSwapBuffers                   | Utilisez [**IDXGISwapChain1 ::P resent1**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1).                                                                                                                                                                                                                                                                                                                                                                                                                     |
| eglSwapInterval                  | Voir [**IDXGISwapChain1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1).                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| eglTerminate                     | L’objet CoreWindow utilisé pour afficher la sortie du pipeline graphique est géré par le système d’exploitation.                                                                                                                                                                                                                                                                                                                                                                                          |
| eglWaitClient                    | Pour les surfaces partagées, utilisez IDXGIKeyedMutex. Pour obtenir des informations générales sur le multithreading du GPU, voir la rubrique [Multithreading](/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-intro).                                                                                                                                                                                                                                                                                                                                    |
| eglWaitGL                        | Pour les surfaces partagées, utilisez IDXGIKeyedMutex. Pour obtenir des informations générales sur le multithreading du GPU, voir la rubrique [Multithreading](/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-intro).                                                                                                                                                                                                                                                                                                                                    |
| eglWaitNative                    | Pour les surfaces partagées, utilisez IDXGIKeyedMutex. Pour obtenir des informations générales sur le multithreading du GPU, voir la rubrique [Multithreading](/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-intro).                                                                                                                                                                                                                                                                                                                                    |

 

 

 