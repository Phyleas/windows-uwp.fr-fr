---
title: Technologie interop DirectX et XAML
description: Vous pouvez utiliser XAML (Extensible Application Markup Language) et Microsoft DirectX conjointement dans votre jeu de plateforme Windows universelle (UWP).
ms.assetid: 0fb2819a-61ed-129d-6564-0b67debf5c6b
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, jeux, DirectX, interopérabilité XAML
ms.localizationpriority: medium
ms.openlocfilehash: fc5e5323f509759754822849bd7dc93e7a45eaee
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156473"
---
# <a name="directx-and-xaml-interop"></a>Technologie interop DirectX et XAML

Vous pouvez utiliser XAML (Extensible Application Markup Language) et Microsoft DirectX conjointement dans votre jeu ou application de plateforme Windows universelle (UWP). En combinant XAML et DirectX, vous pouvez générer des infrastructures d’interface utilisateur flexibles qui interopèrent avec votre contenu DirectX, ce qui s’avère particulièrement utile dans le cas d’applications qui utilisent beaucoup de ressources graphiques. Cette rubrique décrit la structure d’une application UWP qui utilise DirectX et identifie les types qu’il est important d’utiliser au moment de générer votre application UWP pour la faire fonctionner avec DirectX.

Si votre application se concentre principalement sur le rendu 2D, il peut être judicieux d’utiliser la bibliothèque Windows Runtime [Win2D](https://github.com/microsoft/win2d). Gérée par Microsoft, cette bibliothèque est basée sur les technologies Direct2D principales. Elle simplifie considérablement simplifie le modèle d’utilisation pour implémenter des graphismes 2D et comprend des abstractions utiles pour certaines des techniques décrites dans ce document. Pour en savoir plus, voir la page du projet. Ce document fournit des recommandations pour les développeurs d’applications qui choisissent de ne *pas* utiliser Win2D.

> [!NOTE]
> Les API DirectX ne sont pas définies en tant que types de Windows Runtime, mais vous pouvez généralement utiliser [C++/WinRT](../cpp-and-winrt-apis/index.md) pour développer des composants XAML UWP qui interagissent avec DirectX. En outre, vous pouvez créer une application UWP en XAML et C# qui utilise DirectX, à condition d’inclure les appels DirectX dans un wrapper au sein d’un fichier de métadonnées Windows Runtime distinct.

## <a name="xaml-and-directx"></a>XAML et DirectX

DirectX fournit deux bibliothèques complètes pour les graphismes 2D et 3D : Direct2D et Microsoft Direct3D. Bien que XAML assure une prise en charge des primitives et des effets 2D de base, nombreuses sont les applications, notamment de modélisation et de jeux, qui nécessitent une prise en charge de graphismes plus complexes. Pour celles-ci, vous pouvez utiliser Direct2D et Direct3D pour restituer tout ou partie des graphismes et utiliser XAML pour tout le reste.

Si vous implémentez une interopérabilité de XAML et DirectX personnalisée, vous devez connaître les deux concepts suivants :

-   Les surfaces partagées sont des régions dimensionnées de l’affichage, définies par XAML, dans lesquelles vous pouvez dessiner indirectement à l’aide de DirectX en utilisant les types [Windows::UI::Xaml::Media::ImageSource](/uwp/api/windows.ui.xaml.media.imagesource). Pour les surfaces partagées, vous ne contrôlez pas le moment précis où le nouveau contenu s’affiche à l’écran. Au lieu de cela, les mises à jour appliquées à une surface partagée sont synchronisées avec les mises à jour de l’infrastructure XAML.
-   Les [chaînes d’échange](/windows/desktop/direct3d9/what-is-a-swap-chain-) constituent une collection de mémoires tampons servant à afficher les graphismes avec une latence minimale. En règle générale, les chaînes d’échange sont mises à jour à une cadence de 60 images par seconde séparément du thread d’interface utilisateur. Cependant, les chaînes d’échange consomment plus de mémoire et de ressources processeur pour prendre en charge des mises à jour rapides, et sont plus difficiles à utiliser dans la mesure où vous devez gérer plusieurs threads.

Déterminez à quelle fin vous utilisez la technologie DirectX. Voulez-vous l’utiliser pour composer ou animer un contrôle unique qui doit tenir dans les dimensions de la fenêtre d’affichage ? Contiendra-t-elle une sortie qui doit être rendue et contrôlée en temps réel, comme dans un jeu ? Si c’est le cas, vous devrez probablement implémenter une chaîne d’échange. Sinon, une surface partagée devrait faire l’affaire.

Une fois que vous avez déterminé la manière dont vous envisagez d’utiliser DirectX, vous utilisez l’un de ces types de Windows Runtime pour incorporer le rendu DirectX dans votre application UWP :

-   Si vous voulez composer une image statique ou dessiner une image complexe selon des intervalles basés sur des événements, dessinez sur une surface partagée avec [Windows::UI::Xaml::Media::Imaging::SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource). Ce type gère une surface de dessin DirectX dimensionnée. En général, ce type est utilisé pour composer une image ou une texture sous forme d’image bitmap à afficher dans un document ou un élément d’interface. En revanche, il s’avère peu efficace dans un contexte d’interactivité en temps réel, comme les jeux élaborés. Cela s’explique par le fait que les mises à jour appliquées à un objet **SurfaceImageSource** sont synchronisées avec les mises à jour de l’interface utilisateur XAML, ce qui peut introduire un temps de latence dans la rétroaction visuelle à l’utilisateur avec, par exemple, une fréquence d’images fluctuante ou un sentiment de faible réactivité aux entrées en temps réel. Les mises à jour restent néanmoins suffisamment rapides pour les contrôles dynamiques ou les simulations de données.

-   Si l’image est plus grande que l’espace fourni par l’écran et si elle peut être recentrée ou zoomée par l’utilisateur, utilisez [Windows::UI::Xaml::Media::Imaging::VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource). Ce type gère une surface de dessin DirectX dimensionnée de format supérieur à l’écran. Tout comme [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource), il s’avère utile pour composer dynamiquement une image ou un contrôle complexe. De même, à la manière de **SurfaceImageSource**, il se révèle peu efficace pour les jeux élaborés. Parmi les éléments XAML susceptibles d’utiliser un **VirtualSurfaceImageSource**, citons les contrôles de carte ou une visionneuse de documents volumineux et riches en images.

-   Si vous utilisez DirectX pour présenter des graphismes mis à jour en temps réel ou dans un contexte où les mises à jour doivent se produire à intervalles réguliers et avec peu de latence, utilisez la classe [SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel). Les graphismes seront ainsi actualisés sans être synchronisés avec le minuteur d’actualisation de l’infrastructure XAML. Ce type vous permet d’accéder directement à la chaîne d’échange du périphérique graphique ([IDXGISwapChain1](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1)) et de disposer la couche XAML sur la cible de rendu. Ce type est tout indiqué pour les jeux et les applications DirectX en plein écran qui nécessitent une interface utilisateur XAML. Pour utiliser cette approche, vous devez bien connaître DirectX, notamment les technologies Microsoft DXGI (DirectX Graphics Infrastructure), Direct2D et Direct3D. Pour plus d’informations, voir [Guide de programmation pour Direct3D 11](/windows/desktop/direct3d11/dx-graphics-overviews).

## <a name="surfaceimagesource"></a>SurfaceImageSource

[SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) propose des surfaces partagées DirectX dans lesquelles il est possible de dessiner et compose les éléments de contenu de l’application.

Voici le processus de base pour la création et la mise à jour d’un objet [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) dans le code-behind :

1.  Définissez la taille de la surface partagée en passant la hauteur et la largeur au constructeur [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource). Vous pouvez également indiquer si la surface a besoin d’une prise en charge alpha (opacité).

    Par exemple :

    `SurfaceImageSource^ surfaceImageSource = ref new SurfaceImageSource(400, 300);`

2.  Obtient un pointeur vers [ISurfaceImageSourceNativeWithD2D](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d). Effectuez un cast de l’objet [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) en [IInspectable](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (ou **IUnknown**), puis appelez **QueryInterface** dessus pour accéder à l’implémentation **ISurfaceImageSourceNativeWithD2D** sous-jacente. Les méthodes définies dans cette implémentation permettent de définir l’appareil et d’exécuter les opérations de dessin.

    ```cpp
    Microsoft::WRL::ComPtr<ISurfaceImageSourceNativeWithD2D> m_sisNativeWithD2D;

    // ...

    IInspectable* sisInspectable = 
        (IInspectable*) reinterpret_cast<IInspectable*>(surfaceImageSource);

    sisInspectable->QueryInterface(
        __uuidof(ISurfaceImageSourceNativeWithD2D), 
        (void **)&m_sisNativeWithD2D);
    ```

3.  Créez les périphériques DXGI et D2D en appelant d’abord [D3D11CreateDevice](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) et [D2D1CreateDevice](https://docs.microsoft.com/windows/desktop/api/d2d1_1/nf-d2d1_1-d2d1createdevice) , puis en passant l’appareil et le contexte à [ISurfaceImageSourceNativeWithD2D :: SetDevice](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d-setdevice). 

    > [!NOTE]
    > Si vous dessinez dans votre **SurfaceImageSource** à partir d’un thread d’arrière-plan, vous devrez également vous assurer que l’appareil dxgi a activé l’accès multithread. Cette opération ne doit être effectuée que si vous dessinez à partir d’un thread d’arrière-plan, pour des raisons de performances.

    Par exemple :

    ```cpp
    Microsoft::WRL::ComPtr<ID3D11Device> m_d3dDevice;
    Microsoft::WRL::ComPtr<ID3D11DeviceContext> m_d3dContext;
    Microsoft::WRL::ComPtr<ID2D1Device> m_d2dDevice;

    // Create the DXGI device
    D3D11CreateDevice(
            NULL,
            D3D_DRIVER_TYPE_HARDWARE,
            NULL,
            flags,
            featureLevels,
            ARRAYSIZE(featureLevels),
            D3D11_SDK_VERSION,
            &m_d3dDevice,
            NULL,
            &m_d3dContext);

    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);

    // To enable multi-threaded access (optional)
    Microsoft::WRL::ComPtr<ID3D10Multithread> d3dMultiThread;

    m_d3dDevice->QueryInterface(
        __uuidof(ID3D10Multithread), 
        (void **) &d3dMultiThread);

    d3dMultiThread->SetMultithreadProtected(TRUE);

    // Create the D2D device
    D2D1CreateDevice(m_dxgiDevice.Get(), NULL, &m_d2dDevice);

    // Set the D2D device
    m_sisNativeWithD2D->SetDevice(m_d2dDevice.Get());
    ```

4.  Fournissez un pointeur vers l’objet [ID2D1DeviceContext](https://docs.microsoft.com/windows/desktop/api/dxgi/nn-dxgi-idxgisurface) à [ISurfaceImageSourceNativeWithD2D :: BeginDraw](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d-begindraw), puis utilisez le contexte de dessin retourné pour dessiner le contenu du rectangle souhaité dans le **SurfaceImageSource**. **ISurfaceImageSourceNativeWithD2D :: BeginDraw** et les commandes de dessin peuvent être appelées à partir d’un thread d’arrière-plan. Seule est dessinée la zone désignée pour la mise à jour dans le paramètre *updateRect*.

    Cette méthode retourne le décalage de points (x,y) du rectangle cible mis à jour dans le paramètre *offset*. Vous utilisez ce décalage pour déterminer où dessiner le contenu mis à jour avec **ID2D1DeviceContext**.

    ```cpp
    Microsoft::WRL::ComPtr<ID2D1DeviceContext> drawingContext;

    HRESULT beginDrawHR = m_sisNative->BeginDraw(
        updateRect, 
        &drawingContext, 
        &offset);

    if (beginDrawHR == DXGI_ERROR_DEVICE_REMOVED 
        || beginDrawHR == DXGI_ERROR_DEVICE_RESET
        || beginDrawHR == D2DERR_RECREATE_TARGET)
    {
        // The D3D and D2D device was lost and need to be re-created.
        // Recovery steps are:
        // 1) Re-create the D3D and D2D devices
        // 2) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the new D2D
        //    device
        // 3) Redraw the contents of the SurfaceImageSource
    }
    else if (beginDrawHR == E_SURFACE_CONTENTS_LOST)
    {
        // The devices were not lost but the entire contents of the surface
        // were. Recovery steps are:
        // 1) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the D2D 
        //    device again
        // 2) Redraw the entire contents of the SurfaceImageSource
    }
    else 
    {
        // Draw your updated rectangle with the drawingContext
    }
    ```

5. Appelez [ISurfaceImageSourceNativeWithD2D :: EndDraw](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d-enddraw) pour terminer l’image bitmap. L’image bitmap peut être utilisée en tant que source pour un élément XAML [Image](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image) ou [ImageBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush). **ISurfaceImageSourceNativeWithD2D :: EndDraw** doit être appelé uniquement à partir du thread d’interface utilisateur.

    ```cpp
    m_sisNative->EndDraw();

    // ...
    // The SurfaceImageSource object's underlying 
    // ISurfaceImageSourceNativeWithD2D object contains the completed bitmap.

    ImageBrush^ brush = ref new ImageBrush();
    brush->ImageSource = surfaceImageSource;
    ```

    > [!NOTE]
    > L’appel de [SurfaceImageSource :: SetSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsource) (héritée de **IBitmapSource :: SetSource**) lève actuellement une exception. Ne l’appelez pas à partir de votre objet [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource).

    > [!NOTE]
    > Les applications doivent éviter les dessins sur **SurfaceImageSource** tandis que leur [fenêtre](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) associée est masquée, sinon les API **ISurfaceImageSourceNativeWithD2D** échouent. Pour ce faire, inscrivez-vous en tant qu’écouteur d’événements pour l’événement [Window. VisibilityChanged](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window.VisibilityChanged) pour suivre les modifications de visibilité.

## <a name="virtualsurfaceimagesource"></a>VirtualSurfaceImageSource

[VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) étend [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) lorsque le contenu est potentiellement plus volumineux pour l’écran et donc que le contenu doit être virtualisé pour un rendu optimal.

[VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) est différent de [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) dans le sens où il utilise un rappel, [IVirtualSurfaceImageSourceCallbacksNative::UpdatesNeeded](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceupdatescallbacknative-updatesneeded), que vous implémentez pour mettre à jour des régions de la surface à mesure qu’elles s’affichent à l’écran. Vous n’avez pas besoin d’effacer les régions masquées, car l’infrastructure XAML s’en charge à votre place.

Voici le processus de base pour la création et la mise à jour d’un objet [VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) dans le code-behind :

1.  Créez une instance de [VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) avec la taille de votre choix. Par exemple :

    ```cpp
    VirtualSurfaceImageSource^ virtualSIS = 
        ref new VirtualSurfaceImageSource(2000, 2000);
    ```

2.  Obtient les pointeurs vers [IVirtualSurfaceImageSourceNative](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative) et [ISurfaceImageSourceNativeWithD2D](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-isurfaceimagesourcenativewithd2d). Effectuez un cast de l’objet [VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) en [IInspectable](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) ou [IUnknown](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown), puis appelez [QueryInterface](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) sur ce dernier pour accéder aux implémentations **IVirtualSurfaceImageSourceNative** et **ISurfaceImageSourceNativeWithD2D** sous-jacentes. Vous utilisez les méthodes définies sur ces implémentations pour définir le périphérique et exécuter les opérations de dessin.

    ```cpp
    Microsoft::WRL::ComPtr<IVirtualSurfaceImageSourceNative>  m_vsisNative;
    Microsoft::WRL::ComPtr<ISurfaceImageSourceNativeWithD2D> m_sisNativeWithD2D;

    // ...

    IInspectable* vsisInspectable = 
        (IInspectable*) reinterpret_cast<IInspectable*>(virtualSIS);

    vsisInspectable->QueryInterface(
        __uuidof(IVirtualSurfaceImageSourceNative), 
        (void **) &m_vsisNative);

    vsisInspectable->QueryInterface(
        __uuidof(ISurfaceImageSourceNativeWithD2D), 
        (void **) &m_sisNativeWithD2D);
    ```

3.  Créez les périphériques DXGI et D2D en appelant d’abord **D3D11CreateDevice** et **D2D1CreateDevice**, puis transmettez l’appareil D2D à **ISurfaceImageSourceNativeWithD2D :: SetDevice**.

    > [!NOTE]
    > Si vous dessinez dans votre **VirtualSurfaceImageSource** à partir d’un thread d’arrière-plan, vous devrez également vous assurer que l’appareil dxgi a activé l’accès multithread. Cette opération ne doit être effectuée que si vous dessinez à partir d’un thread d’arrière-plan, pour des raisons de performances.

    Par exemple :

    ```cpp
    Microsoft::WRL::ComPtr<ID3D11Device> m_d3dDevice;
    Microsoft::WRL::ComPtr<ID3D11DeviceContext> m_d3dContext;
    Microsoft::WRL::ComPtr<ID2D1Device> m_d2dDevice;

    // Create the DXGI device
    D3D11CreateDevice(
            NULL,
            D3D_DRIVER_TYPE_HARDWARE,
            NULL,
            flags,
            featureLevels,
            ARRAYSIZE(featureLevels),
            D3D11_SDK_VERSION,
            &m_d3dDevice,
            NULL,
            &m_d3dContext);  

    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);
    
    // To enable multi-threaded access (optional)
    Microsoft::WRL::ComPtr<ID3D10Multithread> d3dMultiThread;

    m_d3dDevice->QueryInterface(
        __uuidof(ID3D10Multithread), 
        (void **) &d3dMultiThread);

    d3dMultiThread->SetMultithreadProtected(TRUE);

    // Create the D2D device
    D2D1CreateDevice(m_dxgiDevice.Get(), NULL, &m_d2dDevice);

    // Set the D2D device
    m_vsisNativeWithD2D->SetDevice(m_d2dDevice.Get());

    m_vsisNative->SetDevice(dxgiDevice.Get());
    ```

4.  Appelez [IVirtualSurfaceImageSourceNative::RegisterForUpdatesNeeded](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-registerforupdatesneeded) en passant une référence à votre implémentation de [IVirtualSurfaceUpdatesCallbackNative](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-ivirtualsurfaceupdatescallbacknative).

    ```cpp
    class MyContentImageSource : public IVirtualSurfaceUpdatesCallbackNative
    {
    // ...
      private:
         virtual HRESULT STDMETHODCALLTYPE UpdatesNeeded() override;
    }

    // ...

    HRESULT STDMETHODCALLTYPE MyContentImageSource::UpdatesNeeded()
    {
      // .. Perform drawing here ...
    }

    void MyContentImageSource::Initialize()
    {
      // ...
      m_vsisNative->RegisterForUpdatesNeeded(this);
      // ...
    }
    ```

    L’infrastructure appelle votre implémentation de [IVirtualSurfaceUpdatesCallbackNative::UpdatesNeeded](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-registerforupdatesneeded) lorsqu’une région de [VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource) doit être mise à jour.

    Cela se produit lorsque l’infrastructure détermine que la région doit être dessinée (par exemple, lorsque l’utilisateur recentre ou zoome la vue de la surface) ou après que l’application a appelé [IVirtualSurfaceImageSourceNative::Invalidate](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-invalidate) sur cette région.

5.  Dans [IVirtualSurfaceImageSourceNative::UpdatesNeeded](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceupdatescallbacknative-updatesneeded), utilisez les méthodes [IVirtualSurfaceImageSourceNative::GetUpdateRectCount](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-getupdaterectcount) et [IVirtualSurfaceImageSourceNative::GetUpdateRects](https://docs.microsoft.com/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-ivirtualsurfaceimagesourcenative-getupdaterects) pour déterminer la ou les régions de la surface à dessiner.

    ```cpp
    HRESULT STDMETHODCALLTYPE MyContentImageSource::UpdatesNeeded()
    {
        HRESULT hr = S_OK;

        try
        {
            ULONG drawingBoundsCount = 0;  
            m_vsisNative->GetUpdateRectCount(&drawingBoundsCount);

            std::unique_ptr<RECT[]> drawingBounds(
                new RECT[drawingBoundsCount]);

            m_vsisNative->GetUpdateRects(
                drawingBounds.get(), 
                drawingBoundsCount);
            
            for (ULONG i = 0; i < drawingBoundsCount; ++i)
            {
                // Drawing code here ...
            }
        }
        catch (Platform::Exception^ exception)
        {
            hr = exception->HResult;
        }

        return hr;
    }
    ```

6.  Enfin, pour chaque région à mettre à jour :

    1.  Fournissez un pointeur vers l’objet **ID2D1DeviceContext** à **ISurfaceImageSourceNativeWithD2D :: BeginDraw**et utilisez le contexte de dessin retourné pour dessiner le contenu du rectangle souhaité dans le **SurfaceImageSource**. **ISurfaceImageSourceNativeWithD2D :: BeginDraw** et les commandes de dessin peuvent être appelées à partir d’un thread d’arrière-plan. Seule est dessinée la zone désignée pour la mise à jour dans le paramètre *updateRect*.

        Cette méthode retourne le décalage de points (x,y) du rectangle cible mis à jour dans le paramètre *offset*. Vous utilisez ce décalage pour déterminer où dessiner le contenu mis à jour avec **ID2D1DeviceContext**.

        ```cpp
        Microsoft::WRL::ComPtr<ID2D1DeviceContext> drawingContext;

        HRESULT beginDrawHR = m_sisNative->BeginDraw(
            updateRect, 
            &drawingContext, 
            &offset);

        if (beginDrawHR == DXGI_ERROR_DEVICE_REMOVED 
            || beginDrawHR == DXGI_ERROR_DEVICE_RESET 
            || beginDrawHR == D2DERR_RECREATE_TARGET)
        {
            // The D3D and D2D devices were lost and need to be re-created.
            // Recovery steps are:
            // 1) Re-create the D3D and D2D devices
            // 2) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the 
            //    new D2D device
            // 3) Redraw the contents of the VirtualSurfaceImageSource
        }
        else if (beginDrawHR == E_SURFACE_CONTENTS_LOST)
        {
            // The devices were not lost but the entire contents of the 
            // surface were lost. Recovery steps are:
            // 1) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the 
            //    D2D device again
            // 2) Redraw the entire contents of the VirtualSurfaceImageSource
        }
        else
        {
            // Draw your updated rectangle with the drawingContext
        }
        ```

    2.  Dessinez le contenu spécifique dans cette région, mais pour des performances optimales, laissez votre dessin dans les limites de la région.

    3.  Appelez **ISurfaceImageSourceNativeWithD2D :: EndDraw**. Le résultat est une image bitmap.

> [!NOTE]
> Les applications doivent éviter les dessins sur **SurfaceImageSource** tandis que leur [fenêtre](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) associée est masquée, sinon les API **ISurfaceImageSourceNativeWithD2D** échouent. Pour ce faire, inscrivez-vous en tant qu’écouteur d’événements pour l’événement [Window. VisibilityChanged](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window.VisibilityChanged) pour suivre les modifications de visibilité.

## <a name="swapchainpanel-and-gaming"></a>SwapChainPanel et jeux


[SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) est le type Windows Runtime conçu pour prendre en charge les graphismes et les jeux élaborés, où vous gérez directement la chaîne de permutation. Dans ce cas, vous devez créer votre propre chaîne d’échange DirectX et gérer la présentation du contenu rendu.

Afin de garantir de bonnes performances, le type [SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) impose certaines limitations :

-   Le nombre maximal d’instances de [SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) par application est de 4.
-   Vous devez définir la hauteur et la largeur de la chaîne d’échange DirectX (dans la [ \_ chaîne de permutation dxgi \_ \_ DESC1](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/ns-dxgi1_2-dxgi_swap_chain_desc1)) sur les dimensions actuelles de l’élément de chaîne de permutation. Si vous ne le faites pas, le contenu d’affichage sera mis à l’échelle (à l’aide de l' ** \_ \_ étirement dxgi**).
-   Vous devez définir le mode de mise à l’échelle de la chaîne d’échange DirectX (dans la [ \_ chaîne de permutation dxgi \_ \_ DESC1](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/ns-dxgi1_2-dxgi_swap_chain_desc1)) sur **dxgi \_ \_ Stretch**.
-   Vous devez créer la chaîne de permutation DirectX en appelant [IDXGIFactory2::CreateSwapChainForComposition](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcomposition).

Vous mettez à jour [SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) en fonction des besoins de votre application et non des mises à jour de l’infrastructure XAML. Si vous avez besoin de synchroniser les mises à jour de **SwapChainPanel** avec celles de l’infrastructure XAML, inscrivez-vous à l’événement [Windows::UI::Xaml::Media::CompositionTarget::Rendering](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.compositiontarget.rendering). Sinon, vous devez prévoir des problèmes inter-threads si vous essayez de mettre à jour les éléments XAML à partir d’un thread différent de celui qui met à jour **SwapChainPanel**.

Si vous avez besoin de recevoir des entrées de pointeur à faible latence sur votre élément **SwapChainPanel**, utilisez [SwapChainPanel::CreateCoreIndependentInputSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.swapchainpanel.createcoreindependentinputsource). Cette méthode retourne un objet [CoreIndependentInputSource](https://docs.microsoft.com/uwp/api/windows.ui.core.coreindependentinputsource) qui peut être utilisé pour recevoir les événements d’entrée avec une latence minimale sur un thread d’arrière-plan. Notez qu’une fois cette méthode appelée, les événements d’entrée de pointeur XAML normaux ne sont pas déclenchés pour l’élément **SwapChainPanel**, car toutes les entrées sont redirigées vers le thread d’arrière-plan.


> **Remarque**   En général, vos applications DirectX doivent créer des chaînes de permutation en orientation paysage et égal à la taille de la fenêtre d’affichage (généralement la résolution d’écran native dans la plupart des Microsoft Store jeux). Cela garantit que votre application utilise l’implémentation de chaîne de permutation optimale lorsqu’elle ne possède aucune superposition XAML visible. Si l’application est pivotée en mode portrait, elle doit appeler [IDXGISwapChain1::SetRotation](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-setrotation) sur la chaîne de permutation existante, appliquer une transformation au contenu, si nécessaire, puis appeler de nouveau [SetSwapChain](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-iswapchainpanelnative-setswapchain) sur la même chaîne de permutation. De même, votre application doit appeler de nouveau **SetSwapChain** sur la même chaîne de permutation chaque fois que celle-ci est redimensionnée à la suite d’un appel de [IDXGISwapChain::ResizeBuffers](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-resizebuffers).


 

Voici un processus de base pour la création et la mise à jour d’un objet [SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) dans le code-behind :

1.  Obtenez une instance d’un panneau de chaîne de permutation pour votre application. Les instances sont indiquées dans votre code XAML par la balise `<SwapChainPanel>`.

    `Windows::UI::Xaml::Controls::SwapChainPanel^ swapChainPanel;`

    Voici un exemple de balise `<SwapChainPanel>`.

    ```xml
    <SwapChainPanel x:Name="swapChainPanel">
        <SwapChainPanel.ColumnDefinitions>
            <ColumnDefinition Width="300*"/>
            <ColumnDefinition Width="1069*"/>
        </SwapChainPanel.ColumnDefinitions>
    …
    ```

2.  Obtenez un pointeur vers [ISwapChainPanelNative](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-iswapchainpanelnative). Effectuez un cast de l’objet [SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) en tant qu’[IInspectable](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (ou **IUnknown**), puis appelez **QueryInterface** dessus pour obtenir l’implémentation d’**ISwapChainPanelNative** sous-jacente.

    ```cpp
    Microsoft::WRL::ComPtr<ISwapChainPanelNative> m_swapChainNative;
    // ...
    IInspectable* panelInspectable = (IInspectable*) reinterpret_cast<IInspectable*>(swapChainPanel);
    panelInspectable->QueryInterface(__uuidof(ISwapChainPanelNative), (void **)&m_swapChainNative);
    ```

3.  Créez le périphérique DXGI et la chaîne de permutation et attribuez à cette dernière la valeur [ISwapChainPanelNative](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-iswapchainpanelnative) en la passant à [SetSwapChain](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-iswapchainpanelnative-setswapchain).

    ```cpp
    Microsoft::WRL::ComPtr<IDXGISwapChain1>               m_swapChain;    
    // ...
    DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};
            swapChainDesc.Width = m_bounds.Width;
            swapChainDesc.Height = m_bounds.Height;
            swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;           // This is the most common swapchain format.
            swapChainDesc.Stereo = false; 
            swapChainDesc.SampleDesc.Count = 1;                          // Don't use multi-sampling.
            swapChainDesc.SampleDesc.Quality = 0;
            swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
            swapChainDesc.BufferCount = 2;
            swapChainDesc.Scaling = DXGI_SCALING_STRETCH;
            swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL; // We recommend using this swap effect for all. applications
            swapChainDesc.Flags = 0;
                    
    // QI for DXGI device
    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);

    // Get the DXGI adapter.
    Microsoft::WRL::ComPtr<IDXGIAdapter> dxgiAdapter;
    dxgiDevice->GetAdapter(&dxgiAdapter);

    // Get the DXGI factory.
    Microsoft::WRL::ComPtr<IDXGIFactory2> dxgiFactory;
    dxgiAdapter->GetParent(__uuidof(IDXGIFactory2), &dxgiFactory);
    // Create a swap chain by calling CreateSwapChainForComposition.
    dxgiFactory->CreateSwapChainForComposition(
                m_d3dDevice.Get(),
                &swapChainDesc,
                nullptr,        // Allow on any display. 
                &m_swapChain
                );
            
    m_swapChainNative->SetSwapChain(m_swapChain.Get());
    ```

4.  Dessinez sur la chaîne de permutation DirectX et présentez-la pour afficher le contenu.

    ```cpp
    HRESULT hr = m_swapChain->Present(1, 0);
    ```

    Les éléments XAML sont actualisés lorsque la logique de disposition/rendu de Windows Runtime signale une mise à jour.

## <a name="related-topics"></a>Rubriques connexes

* [Win2D](https://microsoft.github.io/Win2D/html/Introduction.htm)
* [SurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource)
* [VirtualSurfaceImageSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.VirtualSurfaceImageSource)
* [SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)
* [ISwapChainPanelNative](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-iswapchainpanelnative)
* [Guide de programmation pour Direct3D 11](/windows/desktop/direct3d11/dx-graphics-overviews)