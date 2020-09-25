---
title: Capture d'écran
description: L’espace de noms Windows.Graphics.Capture fournit des API pour l’acquisition d’images à partir d’une fenêtre d’affichage ou d’application, afin de créer des flux vidéo ou des captures instantanées pour créer des expériences collaboratives et interactives.
ms.assetid: 349C959D-9C74-44E7-B5F6-EBDB5CA87B9F
ms.date: 06/14/2019
ms.topic: article
dev_langs:
- csharp
- vb
keywords: Windows 10, UWP, capture d’écran
ms.localizationpriority: medium
ms.openlocfilehash: 26de7699f9f261bba6e02bc3664e335c46e4ac3d
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91218662"
---
# <a name="screen-capture"></a>Capture d'écran

À partir de Windows 10, version 1803, l’espace de noms [Windows. Graphics. capture](/uwp/api/windows.graphics.capture) fournit des API pour obtenir des frames à partir d’une fenêtre d’affichage ou d’application, pour créer des flux de vidéos ou des captures instantanées afin de créer des expériences collaboratives et interactives.

Avec la capture d’écran, les développeurs appellent une interface utilisateur de système sécurisée pour permettre aux utilisateurs finaux de sélectionner la fenêtre d’affichage ou d’application à capturer, et une bordure jaune de notification est dessinée par le système autour de l’élément activement capturé. Dans le cas de plusieurs sessions de capture simultanées, une bordure jaune est dessinée autour de chaque élément capturé.

> [!NOTE]
> Les API de capture d’écran sont uniquement prises en charge sur les casques et les casques immersifs de Windows Mixed Reality.

Cet article décrit la capture d’une seule image de la fenêtre d’affichage ou d’application. Pour plus d’informations sur l’encodage des frames capturés à partir de l’écran dans un fichier vidéo, voir [capture d’écran sur vidéo](screen-capture-video.md)

## <a name="add-the-screen-capture-capability"></a>Ajouter la fonctionnalité de capture d’écran

Les API trouvées dans l’espace de noms **Windows. Graphics. capture** nécessitent une capacité générale à être déclarée dans le manifeste de votre application :

1. Ouvrez **Package. appxmanifest** dans le **Explorateur de solutions**.
2. Sélectionnez l’onglet **Fonctionnalités**.
3. Vérifiez la **capture des graphiques**.

![Capture graphique](images/screen-capture-1.png)

## <a name="launch-the-system-ui-to-start-screen-capture"></a>Lancer l’interface utilisateur du système pour démarrer la capture d’écran

Avant de lancer l’interface utilisateur du système, vous pouvez vérifier si votre application est actuellement en mesure de prendre des captures d’écran. Il existe plusieurs raisons pour lesquelles votre application peut ne pas être en mesure d’utiliser la capture d’écran, notamment si l’appareil ne répond pas à la configuration matérielle requise ou si l’application cible la capture d’écran des blocs de capture. Utilisez la méthode **IsSupported** dans la classe [GraphicsCaptureSession](/uwp/api/windows.graphics.capture.graphicscapturesession) pour déterminer si la capture d’écran UWP est prise en charge :

```csharp
// This runs when the application starts.
public void OnInitialization()
{
    if (!GraphicsCaptureSession.IsSupported())
    {
        // Hide the capture UI if screen capture is not supported.
        CaptureButton.Visibility = Visibility.Collapsed;
    }
}
```

```vb
Public Sub OnInitialization()
    If Not GraphicsCaptureSession.IsSupported Then
        CaptureButton.Visibility = Visibility.Collapsed
    End If
End Sub
```

Une fois que vous avez vérifié que la capture d’écran est prise en charge, utilisez la classe [GraphicsCapturePicker](/uwp/api/windows.graphics.capture.graphicscapturepicker) pour appeler l’interface utilisateur du sélecteur de système. L’utilisateur final utilise cette interface utilisateur pour sélectionner la fenêtre d’affichage ou d’application dont les captures d’écran doivent être applicatives. Le sélecteur renverra un [GraphicsCaptureItem](/uwp/api/windows.graphics.capture.graphicscaptureitem) qui sera utilisé pour créer un **GraphicsCaptureSession**:

```csharp
public async Task StartCaptureAsync()
{
    // The GraphicsCapturePicker follows the same pattern the
    // file pickers do.
    var picker = new GraphicsCapturePicker();
    GraphicsCaptureItem item = await picker.PickSingleItemAsync();

    // The item may be null if the user dismissed the
    // control without making a selection or hit Cancel.
    if (item != null)
    {
        // We'll define this method later in the document.
        StartCaptureInternal(item);
    }
}
```

```vb
Public Async Function StartCaptureAsync() As Task
    ' The GraphicsCapturePicker follows the same pattern the
    ' file pickers do.
    Dim picker As New GraphicsCapturePicker
    Dim item As GraphicsCaptureItem = Await picker.PickSingleItemAsync()

    ' The item may be null if the user dismissed the
    ' control without making a selection or hit Cancel.
    If item IsNot Nothing Then
        StartCaptureInternal(item)
    End If
End Function
```

Comme il s’agit du code d’interface utilisateur, il doit être appelé sur le thread d’interface utilisateur. Si vous l’appelez à partir du code-behind d’une page de votre application (par exemple, **MainPage.Xaml.cs**), cette opération est effectuée automatiquement, mais dans le cas contraire, vous pouvez la forcer à s’exécuter sur le thread d’interface utilisateur avec le code suivant :

```csharp
CoreWindow window = CoreApplication.MainView.CoreWindow;

await window.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, async () =>
{
    await StartCaptureAsync();
});
```

```vb
Dim window As CoreWindow = CoreApplication.MainView.CoreWindow
Await window.Dispatcher.RunAsync(CoreDispatcherPriority.Normal,
                                 Async Sub() Await StartCaptureAsync())
```

## <a name="create-a-capture-frame-pool-and-capture-session"></a>Créer un pool de frames de capture et une session de capture

À l’aide de **GraphicsCaptureItem**, vous allez créer un [Direct3D11CaptureFramePool](/uwp/api/windows.graphics.capture.direct3d11captureframepool) avec votre appareil D3D, le format de pixel pris en charge (**dxgi \_ format \_ B8G8R8A8 \_ UNORM**), le nombre de frames souhaités (qui peut être n’importe quel entier) et la taille du frame. La propriété de **contenu** de la classe **GraphicsCaptureItem** peut être utilisée comme taille de votre Frame :

```csharp
private GraphicsCaptureItem _item;
private Direct3D11CaptureFramePool _framePool;
private CanvasDevice _canvasDevice;
private GraphicsCaptureSession _session;

public void StartCaptureInternal(GraphicsCaptureItem item)
{
    _item = item;

    _framePool = Direct3D11CaptureFramePool.Create(
        _canvasDevice, // D3D device
        DirectXPixelFormat.B8G8R8A8UIntNormalized, // Pixel format
        2, // Number of frames
        _item.Size); // Size of the buffers
}
```

```vb
WithEvents CaptureItem As GraphicsCaptureItem
WithEvents FramePool As Direct3D11CaptureFramePool
Private _canvasDevice As CanvasDevice
Private _session As GraphicsCaptureSession

Private Sub StartCaptureInternal(item As GraphicsCaptureItem)
    CaptureItem = item

    FramePool = Direct3D11CaptureFramePool.Create(
        _canvasDevice, ' D3D device
        DirectXPixelFormat.B8G8R8A8UIntNormalized, ' Pixel format
        2, '  Number of frames
        CaptureItem.Size) ' Size of the buffers
End Sub
```

Ensuite, récupérez une instance de la classe **GraphicsCaptureSession** pour votre **Direct3D11CaptureFramePool** en passant le **GraphicsCaptureItem** à la méthode **CreateCaptureSession** :

```csharp
_session = _framePool.CreateCaptureSession(_item);
```

```vb
_session = FramePool.CreateCaptureSession(CaptureItem)
```

Une fois que l’utilisateur a explicitement donné son consentement pour capturer une fenêtre d’application ou un affichage dans l’interface utilisateur du système, le **GraphicsCaptureItem** peut être associé à plusieurs objets **CaptureSession** . De cette façon, votre application peut choisir de capturer le même élément pour différentes expériences.

Pour capturer plusieurs éléments en même temps, votre application doit créer une session de capture pour chaque élément à capturer, ce qui nécessite l’appel de l’interface utilisateur du sélecteur pour chaque élément à capturer.

## <a name="acquire-capture-frames"></a>Acquérir des frames de capture

Une fois le pool de frames et la session de capture créés, appelez la méthode **StartCapture** sur votre instance **GraphicsCaptureSession** pour notifier au système de commencer à envoyer des frames de capture à votre application :

```csharp
_session.StartCapture();
```

```vb
_session.StartCapture()
```

Pour acquérir ces frames de capture, qui sont des objets [Direct3D11CaptureFrame](/uwp/api/windows.graphics.capture.direct3d11captureframe) , vous pouvez utiliser l’événement **Direct3D11CaptureFramePool. FrameArrived** :

```csharp
_framePool.FrameArrived += (s, a) =>
{
    // The FrameArrived event fires for every frame on the thread that
    // created the Direct3D11CaptureFramePool. This means we don't have to
    // do a null-check here, as we know we're the only one  
    // dequeueing frames in our application.  

    // NOTE: Disposing the frame retires it and returns  
    // the buffer to the pool.
    using (var frame = _framePool.TryGetNextFrame())
    {
        // We'll define this method later in the document.
        ProcessFrame(frame);
    }  
};
```

```vb
Private Sub FramePool_FrameArrived(sender As Direct3D11CaptureFramePool, args As Object) Handles FramePool.FrameArrived
    ' The FrameArrived event is raised for every frame on the thread
    ' that created the Direct3D11CaptureFramePool. This means we
    ' don't have to do a null-check here, as we know we're the only
    ' one dequeueing frames in our application.  

    ' NOTE Disposing the frame retires it And returns  
    ' the buffer to the pool.

    Using frame = FramePool.TryGetNextFrame()
        ProcessFrame(frame)
    End Using
End Sub
```

Il est recommandé d’éviter d’utiliser le thread d’interface utilisateur si possible pour **FrameArrived**, car cet événement est déclenché chaque fois qu’un nouveau frame est disponible, ce qui est fréquent. Si vous choisissez d’écouter **FrameArrived** sur le thread d’interface utilisateur, gardez à l’esprit la quantité de travail que vous effectuez chaque fois que l’événement se déclenche.

Vous pouvez également extraire manuellement des frames avec la méthode **Direct3D11CaptureFramePool. TryGetNextFrame** jusqu’à ce que vous obteniez tous les frames dont vous avez besoin.

L’objet **Direct3D11CaptureFrame** contient les propriétés **contentive**, **surface**et **SystemRelativeTime**. **SystemRelativeTime** est l’heure QPC ([QueryPerformanceCounter](/windows/desktop/api/profileapi/nf-profileapi-queryperformancecounter)) qui peut être utilisée pour synchroniser d’autres éléments multimédias.

## <a name="process-capture-frames"></a>Traiter les frames de capture

Chaque frame du **Direct3D11CaptureFramePool** est extrait lors de l’appel de **TryGetNextFrame**, puis réintégré en fonction de la durée de vie de l’objet **Direct3D11CaptureFrame** . Pour les applications natives, la libération de l’objet **Direct3D11CaptureFrame** suffit pour réactiver le frame dans le pool de frames. Pour les applications managées, il est recommandé d’utiliser la méthode **Direct3D11CaptureFrame. dispose** (**Close** en C++). **Direct3D11CaptureFrame** implémente l’interface [IClosable](/uwp/api/Windows.Foundation.IClosable) , qui est projetée comme [IDisposable](/dotnet/api/system.idisposable) pour les appelants C#.

Les applications ne doivent pas enregistrer les références aux objets **Direct3D11CaptureFrame** , ni enregistrer les références à la surface Direct3D sous-jacente une fois que le frame a été réintégré.

Lors du traitement d’un frame, il est recommandé que les applications prennent le verrou [ID3D11Multithread](/windows/desktop/api/d3d11_4/nn-d3d11_4-id3d11multithread) sur le même appareil que celui associé à l’objet **Direct3D11CaptureFramePool** .

La surface Direct3D sous-jacente sera toujours la taille spécifiée lors de la création (ou de la recréation) du **Direct3D11CaptureFramePool**. Si le contenu est plus grand que le cadre, le contenu est découpé à la taille du cadre. Si le contenu est plus petit que le frame, le reste du frame contient des données non définies. Il est recommandé que les applications copient un sous-rectangle à l’aide de la propriété de **contenu** pour ce **Direct3D11CaptureFrame** afin d’éviter d’avoir un contenu non défini.

## <a name="take-a-screenshot"></a>Prendre une capture d’écran

Dans notre exemple, nous convertissons chaque **Direct3D11CaptureFrame** en [CanvasBitmap](https://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_CanvasBitmap.htm), qui fait partie des [API Win2D](https://microsoft.github.io/Win2D/html/Introduction.htm).

```csharp
// Convert our D3D11 surface into a Win2D object.
CanvasBitmap canvasBitmap = CanvasBitmap.CreateFromDirect3D11Surface(
    _canvasDevice,
    frame.Surface);
```

Une fois que nous avons le **CanvasBitmap**, nous pouvons l’enregistrer en tant que fichier image. Dans l’exemple suivant, nous l’enregistrons sous la forme d’un fichier PNG dans le dossier **images enregistrées** de l’utilisateur.

```csharp
StorageFolder pictureFolder = KnownFolders.SavedPictures;
StorageFile file = await pictureFolder.CreateFileAsync("test.png", CreationCollisionOption.ReplaceExisting);

using (var fileStream = await file.OpenAsync(FileAccessMode.ReadWrite))
{
    await canvasBitmap.SaveAsync(fileStream, CanvasBitmapFileFormat.Png, 1f);
}
```

## <a name="react-to-capture-item-resizing-or-device-lost"></a>Réagir pour capturer le redimensionnement de l’élément ou l’appareil perdu

Pendant le processus de capture, les applications peuvent souhaiter modifier les aspects de leur **Direct3D11CaptureFramePool**. Cela comprend la fourniture d’un nouveau périphérique Direct3D, la modification de la taille des mémoires tampons de trame, voire la modification du nombre de mémoires tampons dans le pool. Dans chacun de ces scénarios, la méthode **recreate** sur l’objet **Direct3D11CaptureFramePool** est l’outil recommandé.

Lorsque **recreate** est appelé, tous les frames existants sont ignorés. Cela permet d’empêcher le transfert des frames dont les surfaces Direct3D sous-jacentes appartiennent à un appareil auquel l’application n’a peut-être plus accès. Pour cette raison, il peut être judicieux de traiter tous les frames en attente avant d’appeler **recreate**.

## <a name="putting-it-all-together"></a>Exemple complet

L’extrait de code suivant est un exemple de bout en bout illustrant comment implémenter la capture d’écran dans une application UWP. Dans cet exemple, nous avons deux boutons dans le front-end : un appelle **Button_ClickAsync**, et les autres appels **ScreenshotButton_ClickAsync**.

> [!NOTE]
> Cet extrait de code utilise [Win2D](https://microsoft.github.io/Win2D/html/Introduction.htm), une bibliothèque pour le rendu graphique 2D. Pour plus d’informations sur la façon de le configurer pour votre projet, consultez leur documentation.

```csharp
using Microsoft.Graphics.Canvas;
using Microsoft.Graphics.Canvas.UI.Composition;
using System;
using System.Numerics;
using System.Threading.Tasks;
using Windows.Foundation;
using Windows.Graphics;
using Windows.Graphics.Capture;
using Windows.Graphics.DirectX;
using Windows.Storage;
using Windows.UI;
using Windows.UI.Composition;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Hosting;

namespace ScreenCaptureTest
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        // Capture API objects.
        private SizeInt32 _lastSize;
        private GraphicsCaptureItem _item;
        private Direct3D11CaptureFramePool _framePool;
        private GraphicsCaptureSession _session;

        // Non-API related members.
        private CanvasDevice _canvasDevice;
        private CompositionGraphicsDevice _compositionGraphicsDevice;
        private Compositor _compositor;
        private CompositionDrawingSurface _surface;
        private CanvasBitmap _currentFrame;
        private string _screenshotFilename = "test.png";

        public MainPage()
        {
            this.InitializeComponent();
            Setup();
        }

        private void Setup()
        {
            _canvasDevice = new CanvasDevice();

            _compositionGraphicsDevice = CanvasComposition.CreateCompositionGraphicsDevice(
                Window.Current.Compositor,
                _canvasDevice);

            _compositor = Window.Current.Compositor;

            _surface = _compositionGraphicsDevice.CreateDrawingSurface(
                new Size(400, 400),
                DirectXPixelFormat.B8G8R8A8UIntNormalized,
                DirectXAlphaMode.Premultiplied);    // This is the only value that currently works with
                                                    // the composition APIs.

            var visual = _compositor.CreateSpriteVisual();
            visual.RelativeSizeAdjustment = Vector2.One;
            var brush = _compositor.CreateSurfaceBrush(_surface);
            brush.HorizontalAlignmentRatio = 0.5f;
            brush.VerticalAlignmentRatio = 0.5f;
            brush.Stretch = CompositionStretch.Uniform;
            visual.Brush = brush;
            ElementCompositionPreview.SetElementChildVisual(this, visual);
        }

        public async Task StartCaptureAsync()
        {
            // The GraphicsCapturePicker follows the same pattern the
            // file pickers do.
            var picker = new GraphicsCapturePicker();
            GraphicsCaptureItem item = await picker.PickSingleItemAsync();

            // The item may be null if the user dismissed the
            // control without making a selection or hit Cancel.
            if (item != null)
            {
                StartCaptureInternal(item);
            }
        }

        private void StartCaptureInternal(GraphicsCaptureItem item)
        {
            // Stop the previous capture if we had one.
            StopCapture();

            _item = item;
            _lastSize = _item.Size;

            _framePool = Direct3D11CaptureFramePool.Create(
               _canvasDevice, // D3D device
               DirectXPixelFormat.B8G8R8A8UIntNormalized, // Pixel format
               2, // Number of frames
               _item.Size); // Size of the buffers

            _framePool.FrameArrived += (s, a) =>
            {
                // The FrameArrived event is raised for every frame on the thread
                // that created the Direct3D11CaptureFramePool. This means we
                // don't have to do a null-check here, as we know we're the only
                // one dequeueing frames in our application.  

                // NOTE: Disposing the frame retires it and returns  
                // the buffer to the pool.

                using (var frame = _framePool.TryGetNextFrame())
                {
                    ProcessFrame(frame);
                }
            };

            _item.Closed += (s, a) =>
            {
                StopCapture();
            };

            _session = _framePool.CreateCaptureSession(_item);
            _session.StartCapture();
        }

        public void StopCapture()
        {
            _session?.Dispose();
            _framePool?.Dispose();
            _item = null;
            _session = null;
            _framePool = null;
        }

        private void ProcessFrame(Direct3D11CaptureFrame frame)
        {
            // Resize and device-lost leverage the same function on the
            // Direct3D11CaptureFramePool. Refactoring it this way avoids
            // throwing in the catch block below (device creation could always
            // fail) along with ensuring that resize completes successfully and
            // isn’t vulnerable to device-lost.
            bool needsReset = false;
            bool recreateDevice = false;

            if ((frame.ContentSize.Width != _lastSize.Width) ||
                (frame.ContentSize.Height != _lastSize.Height))
            {
                needsReset = true;
                _lastSize = frame.ContentSize;
            }

            try
            {
                // Take the D3D11 surface and draw it into a  
                // Composition surface.

                // Convert our D3D11 surface into a Win2D object.
                CanvasBitmap canvasBitmap = CanvasBitmap.CreateFromDirect3D11Surface(
                    _canvasDevice,
                    frame.Surface);

                _currentFrame = canvasBitmap;

                // Helper that handles the drawing for us.
                FillSurfaceWithBitmap(canvasBitmap);
            }

            // This is the device-lost convention for Win2D.
            catch (Exception e) when (_canvasDevice.IsDeviceLost(e.HResult))
            {
                // We lost our graphics device. Recreate it and reset
                // our Direct3D11CaptureFramePool.  
                needsReset = true;
                recreateDevice = true;
            }

            if (needsReset)
            {
                ResetFramePool(frame.ContentSize, recreateDevice);
            }
        }

        private void FillSurfaceWithBitmap(CanvasBitmap canvasBitmap)
        {
            CanvasComposition.Resize(_surface, canvasBitmap.Size);

            using (var session = CanvasComposition.CreateDrawingSession(_surface))
            {
                session.Clear(Colors.Transparent);
                session.DrawImage(canvasBitmap);
            }
        }

        private void ResetFramePool(SizeInt32 size, bool recreateDevice)
        {
            do
            {
                try
                {
                    if (recreateDevice)
                    {
                        _canvasDevice = new CanvasDevice();
                    }

                    _framePool.Recreate(
                        _canvasDevice,
                        DirectXPixelFormat.B8G8R8A8UIntNormalized,
                        2,
                        size);
                }
                // This is the device-lost convention for Win2D.
                catch (Exception e) when (_canvasDevice.IsDeviceLost(e.HResult))
                {
                    _canvasDevice = null;
                    recreateDevice = true;
                }
            } while (_canvasDevice == null);
        }

        private async void Button_ClickAsync(object sender, RoutedEventArgs e)
        {
            await StartCaptureAsync();
        }

        private async void ScreenshotButton_ClickAsync(object sender, RoutedEventArgs e)
        {
            await SaveImageAsync(_screenshotFilename, _currentFrame);
        }

        private async Task SaveImageAsync(string filename, CanvasBitmap frame)
        {
            StorageFolder pictureFolder = KnownFolders.SavedPictures;

            StorageFile file = await pictureFolder.CreateFileAsync(
                filename,
                CreationCollisionOption.ReplaceExisting);

            using (var fileStream = await file.OpenAsync(FileAccessMode.ReadWrite))
            {
                await frame.SaveAsync(fileStream, CanvasBitmapFileFormat.Png, 1f);
            }
        }
    }
}
```

```vb
Imports System.Numerics
Imports Microsoft.Graphics.Canvas
Imports Microsoft.Graphics.Canvas.UI.Composition
Imports Windows.Graphics
Imports Windows.Graphics.Capture
Imports Windows.Graphics.DirectX
Imports Windows.UI
Imports Windows.UI.Composition
Imports Windows.UI.Xaml.Hosting

Partial Public NotInheritable Class MainPage
    Inherits Page

    ' Capture API objects.
    WithEvents CaptureItem As GraphicsCaptureItem
    WithEvents FramePool As Direct3D11CaptureFramePool

    Private _lastSize As SizeInt32
    Private _session As GraphicsCaptureSession

    ' Non-API related members.
    Private _canvasDevice As CanvasDevice
    Private _compositionGraphicsDevice As CompositionGraphicsDevice
    Private _compositor As Compositor
    Private _surface As CompositionDrawingSurface

    Sub New()
        InitializeComponent()
        Setup()
    End Sub

    Private Sub Setup()
        _canvasDevice = New CanvasDevice()
        _compositionGraphicsDevice = CanvasComposition.CreateCompositionGraphicsDevice(Window.Current.Compositor, _canvasDevice)
        _compositor = Window.Current.Compositor
        _surface = _compositionGraphicsDevice.CreateDrawingSurface(
            New Size(400, 400), DirectXPixelFormat.B8G8R8A8UIntNormalized, DirectXAlphaMode.Premultiplied)
        Dim visual = _compositor.CreateSpriteVisual()
        visual.RelativeSizeAdjustment = Vector2.One
        Dim brush = _compositor.CreateSurfaceBrush(_surface)
        brush.HorizontalAlignmentRatio = 0.5F
        brush.VerticalAlignmentRatio = 0.5F
        brush.Stretch = CompositionStretch.Uniform
        visual.Brush = brush
        ElementCompositionPreview.SetElementChildVisual(Me, visual)
    End Sub

    Public Async Function StartCaptureAsync() As Task
        ' The GraphicsCapturePicker follows the same pattern the
        ' file pickers do.
        Dim picker As New GraphicsCapturePicker
        Dim item As GraphicsCaptureItem = Await picker.PickSingleItemAsync()

        ' The item may be null if the user dismissed the
        ' control without making a selection or hit Cancel.
        If item IsNot Nothing Then
            StartCaptureInternal(item)
        End If
    End Function

    Private Sub StartCaptureInternal(item As GraphicsCaptureItem)
        ' Stop the previous capture if we had one.
        StopCapture()

        CaptureItem = item
        _lastSize = CaptureItem.Size

        FramePool = Direct3D11CaptureFramePool.Create(
            _canvasDevice, ' D3D device
            DirectXPixelFormat.B8G8R8A8UIntNormalized, ' Pixel format
            2, '  Number of frames
            CaptureItem.Size) ' Size of the buffers

        _session = FramePool.CreateCaptureSession(CaptureItem)
        _session.StartCapture()
    End Sub

    Private Sub FramePool_FrameArrived(sender As Direct3D11CaptureFramePool, args As Object) Handles FramePool.FrameArrived
        ' The FrameArrived event is raised for every frame on the thread
        ' that created the Direct3D11CaptureFramePool. This means we
        ' don't have to do a null-check here, as we know we're the only
        ' one dequeueing frames in our application.  

        ' NOTE Disposing the frame retires it And returns  
        ' the buffer to the pool.

        Using frame = FramePool.TryGetNextFrame()
            ProcessFrame(frame)
        End Using
    End Sub

    Private Sub CaptureItem_Closed(sender As GraphicsCaptureItem, args As Object) Handles CaptureItem.Closed
        StopCapture()
    End Sub

    Public Sub StopCapture()
        _session?.Dispose()
        FramePool?.Dispose()
        CaptureItem = Nothing
        _session = Nothing
        FramePool = Nothing
    End Sub

    Private Sub ProcessFrame(frame As Direct3D11CaptureFrame)
        ' Resize and device-lost leverage the same function on the
        ' Direct3D11CaptureFramePool. Refactoring it this way avoids
        ' throwing in the catch block below (device creation could always
        ' fail) along with ensuring that resize completes successfully And
        ' isn't vulnerable to device-lost.

        Dim needsReset As Boolean = False
        Dim recreateDevice As Boolean = False

        If (frame.ContentSize.Width <> _lastSize.Width) OrElse
            (frame.ContentSize.Height <> _lastSize.Height) Then
            needsReset = True
            _lastSize = frame.ContentSize
        End If

        Try
            ' Take the D3D11 surface and draw it into a  
            ' Composition surface.

            ' Convert our D3D11 surface into a Win2D object.
            Dim bitmap = CanvasBitmap.CreateFromDirect3D11Surface(
                _canvasDevice,
                frame.Surface)

            ' Helper that handles the drawing for us.
            FillSurfaceWithBitmap(bitmap)
            ' This is the device-lost convention for Win2D.
        Catch e As Exception When _canvasDevice.IsDeviceLost(e.HResult)
            ' We lost our graphics device. Recreate it and reset
            ' our Direct3D11CaptureFramePool.  
            needsReset = True
            recreateDevice = True
        End Try

        If needsReset Then
            ResetFramePool(frame.ContentSize, recreateDevice)
        End If
    End Sub

    Private Sub FillSurfaceWithBitmap(canvasBitmap As CanvasBitmap)
        CanvasComposition.Resize(_surface, canvasBitmap.Size)

        Using session = CanvasComposition.CreateDrawingSession(_surface)
            session.Clear(Colors.Transparent)
            session.DrawImage(canvasBitmap)
        End Using
    End Sub

    Private Sub ResetFramePool(size As SizeInt32, recreateDevice As Boolean)
        Do
            Try
                If recreateDevice Then
                    _canvasDevice = New CanvasDevice()
                End If
                FramePool.Recreate(_canvasDevice, DirectXPixelFormat.B8G8R8A8UIntNormalized, 2, size)
                ' This is the device-lost convention for Win2D.
            Catch e As Exception When _canvasDevice.IsDeviceLost(e.HResult)
                _canvasDevice = Nothing
                recreateDevice = True
            End Try
        Loop While _canvasDevice Is Nothing
    End Sub

    Private Async Sub Button_ClickAsync(sender As Object, e As RoutedEventArgs) Handles CaptureButton.Click
        Await StartCaptureAsync()
    End Sub

End Class
```

## <a name="record-a-video"></a>Enregistrer une vidéo

Si vous souhaitez enregistrer une vidéo de votre application, vous pouvez suivre la procédure pas à pas présentée dans la [capture d’écran](screen-capture-video.md)de l’article à la vidéo. Vous pouvez utiliser l' [espace de noms Windows. Media. AppRecording](/uwp/api/windows.media.apprecording). Cela fait partie du kit de développement logiciel (SDK) d’extension de bureau. il ne fonctionne donc que sur Desktop et requiert que vous y ajoutiez une référence à partir de votre projet. Pour plus d’informations, consultez [programmation avec les kits de développement](/uwp/extension-sdks/device-families-overview) logiciel (SDK) d’extension.

## <a name="see-also"></a>Voir aussi

* [Espace de noms Windows. Graphics. capture](/uwp/api/windows.graphics.capture)
* [Capture d’écran sur vidéo](screen-capture-video.md)