---
ms.assetid: 3FD2AA71-EF67-47B2-9332-3FFA5D3703EA
description: Cet article explique comment charger et enregistrer des fichiers image à l’aide de BitmapDecoder et de BitmapEncoder, et comment utiliser l’objet SoftwareBitmap pour représenter des images bitmap.
title: Créer, modifier et enregistrer des images bitmap
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 22053ebe8940053094f704d52b19be2ed3f7bb97
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362612"
---
# <a name="create-edit-and-save-bitmap-images"></a>Créer, modifier et enregistrer des images bitmap



Cet article explique comment charger et enregistrer des fichiers image à l’aide de [**BitmapDecoder**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) et de [**BitmapEncoder**](/uwp/api/Windows.Graphics.Imaging.BitmapEncoder), et comment utiliser l’objet [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) pour représenter des images bitmap.

La classe **SoftwareBitmap** est une API polyvalente qui peut être créée à partir de plusieurs sources, y compris les fichiers image, les objets [**WriteableBitmap**](/uwp/api/Windows.UI.Xaml.Media.Imaging.WriteableBitmap), les surfaces Direct3D et le code. **SoftwareBitmap** permet de convertir facilement entre les différents formats de pixels et les modes alpha, et offre l’accès de bas niveau aux données de pixel. En outre, **SoftwareBitmap** est une interface commune utilisée par plusieurs fonctionnalités de Windows, notamment :

-   [**CapturedFrame**](/uwp/api/Windows.Media.Capture.CapturedFrame) qui vous permet d’obtenir des images capturées par l’appareil photo en tant que **SoftwareBitmap**.

-   [**VideoFrame**](/uwp/api/Windows.Media.VideoFrame) qui vous permet d’obtenir une représentation **SoftwareBitmap** d’un élément **VideoFrame**.

-   [**FaceDetector**](/uwp/api/Windows.Media.FaceAnalysis.FaceDetector) qui vous permet de détecter les visages d’un élément **SoftwareBitmap**.

L’exemple de code dans cet article utilise les API des espaces de noms suivants.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetNamespaces":::

## <a name="create-a-softwarebitmap-from-an-image-file-with-bitmapdecoder"></a>Créer un élément SoftwareBitmap à partir d’un fichier image avec BitmapDecoder

Pour créer un [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) à partir d’un fichier, vous pouvez obtenir une instance de [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) contenant les données d’image. Cet exemple utilise un [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) pour permettre à l’utilisateur de sélectionner un fichier image.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetPickInputFile":::

Appelez la méthode [**OpenAsync**](/uwp/api/windows.storage.istoragefile.openasync) de l’objet **StorageFile** pour obtenir un flux d’accès aléatoire contenant les données d’image. Appelez la méthode statique [**BitmapDecoder. CreateAsync**](/uwp/api/windows.graphics.imaging.bitmapdecoder.createasync) pour obtenir une instance de la classe [**BitmapDecoder**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) pour le flux spécifié. Appelez [**GetSoftwareBitmapAsync**](/uwp/api/windows.graphics.imaging.bitmapdecoder.getsoftwarebitmapasync) pour obtenir un objet [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) contenant l’image.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetCreateSoftwareBitmapFromFile":::

## <a name="save-a-softwarebitmap-to-a-file-with-bitmapencoder"></a>Enregistrer un élément SoftwareBitmap dans un fichier avec BitmapEncoder

Pour enregistrer un élément **SoftwareBitmap** dans un fichier, obtenez une instance de **StorageFile** sur laquelle l’image sera enregistrée. Cet exemple utilise un élément [**FileSavePicker**](/uwp/api/Windows.Storage.Pickers.FileSavePicker) pour permettre à l’utilisateur de sélectionner un fichier de sortie.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetPickOutputFile":::

Appelez la méthode [**OpenAsync**](/uwp/api/windows.storage.istoragefile.openasync) de l’objet **StorageFile** pour obtenir un flux d’accès aléatoire sur lequel écrire l’image. Appelez la méthode statique [**BitmapEncoder.CreateAsync**](/uwp/api/windows.graphics.imaging.bitmapencoder.createasync) pour obtenir une instance de la classe [**BitmapEncoder**](/uwp/api/Windows.Graphics.Imaging.BitmapEncoder) pour le flux spécifié. Le premier paramètre de **CreateAsync** est un GUID représentant le codec qui doit être utilisé pour encoder l’image. La classe **BitmapEncoder** expose une propriété qui contient l’ID de chaque codec pris en charge par l’encodeur, telle que [**JpegEncoderId**](/uwp/api/windows.graphics.imaging.bitmapencoder.jpegencoderid).

Utilisez la méthode [**SetSoftwareBitmap**](/uwp/api/windows.graphics.imaging.bitmapencoder.setsoftwarebitmap) pour définir l’image qui sera encodée. Vous pouvez définir les valeurs de la propriété [**BitmapTransform**](/uwp/api/Windows.Graphics.Imaging.BitmapTransform) pour appliquer des transformations de base à l’image pendant qu’elle est encodée. La propriété [**IsThumbnailGenerated**](/uwp/api/windows.graphics.imaging.bitmapencoder.isthumbnailgenerated) détermine si une miniature est générée par l’encodeur. Veuillez noter que tous les formats de fichier ne prennent pas en charge les miniatures. Si vous utilisez cette fonctionnalité, vous devez intercepter l’erreur d’opération non prise en charge qui sera levée si les miniatures ne sont pas prises en charge.

Appelez [**FlushAsync**](/uwp/api/windows.graphics.imaging.bitmapencoder.flushasync) pour faire en sorte que l’encodeur écrive les données d’image dans le fichier spécifié.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetSaveSoftwareBitmapToFile":::

Vous pouvez spécifier des options d’encodage supplémentaires lorsque vous créez l’élément **BitmapEncoder** en créant un objet [**BitmapPropertySet**](/uwp/api/Windows.Graphics.Imaging.BitmapPropertySet) et en le remplissant avec un ou plusieurs objets [**BitmapTypedValue**](/uwp/api/Windows.Graphics.Imaging.BitmapTypedValue) représentant les paramètres d’encodeur. Pour obtenir la liste des options d’encodeur prises en charge, voir [Références des options BitmapEncoder](bitmapencoder-options-reference.md).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetUseEncodingOptions":::

## <a name="use-softwarebitmap-with-a-xaml-image-control"></a>Utiliser SoftwareBitmap avec un contrôle XAML Image

Pour afficher une image dans une page XAML à l’aide du contrôle [**Image**](/uwp/api/Windows.UI.Xaml.Controls.Image), commencez par définir un contrôle **Image** dans votre page XAML.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml" id="SnippetImageControl":::

Actuellement, le contrôle **Image** prend en charge uniquement les images qui utilisent le codage BGRA8 et le canal prémultiplié ou non alpha. Avant d’essayer d’afficher une image, testez-la pour vérifier qu’elle a le format correct, sinon, utilisez la méthode statique [**Convert**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert) de **SoftwareBitmap** pour convertir l’image au format pris en charge.

Créez un nouvel objet [**SoftwareBitmapSource**](/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) . Définissez le contenu de l’objet source en appelant [**SetBitmapAsync**](/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync), en transmettant un élément **SoftwareBitmap**. Vous pouvez ensuite définir la propriété [**Source**](/uwp/api/windows.ui.xaml.controls.image.source) du contrôle **Image** sur l’élément **SoftwareBitmapSource** créé.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetSoftwareBitmapToWriteableBitmap":::

Vous pouvez également utiliser **SoftwareBitmapSource** pour définir un élément **SoftwareBitmap** comme [**ImageSource**](/uwp/api/windows.ui.xaml.media.imagebrush.imagesource) pour [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush).

## <a name="create-a-softwarebitmap-from-a-writeablebitmap"></a>Créer un élément SoftwareBitmap à partir d’un élément WriteableBitmap

Vous pouvez créer un élément **SoftwareBitmap** à partir d’un élément **WriteableBitmap** existant en appelant [**SoftwareBitmap.CreateCopyFromBuffer**](/uwp/api/windows.graphics.imaging.softwarebitmap.createcopyfrombuffer) et en fournissant la propriété **PixelBuffer** de la classe **WriteableBitmap** pour définir les données de pixel. Le deuxième argument permet de demander un format de pixel pour l’élément **WriteableBitmap** créé. Vous pouvez utiliser les propriétés [**PixelWidth**](/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.pixelwidth) et [**PixelHeight**](/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.pixelheight) de **WriteableBitmap** pour spécifier les dimensions de la nouvelle image.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetWriteableBitmapToSoftwareBitmap":::

## <a name="create-or-edit-a-softwarebitmap-programmatically"></a>Créer ou modifier un élément SoftwareBitmap par programme

Jusqu’à présent, cette rubrique faisait référence aux fichiers image. Vous pouvez également créer un élément **SoftwareBitmap** par programme dans le code et utiliser la même technique pour accéder aux données de pixel de **SoftwareBitmap** et les modifier.

**SoftwareBitmap** utilise COM Interop pour exposer la mémoire tampon brute contenant les données de pixel.

Pour utiliser COM Interop, vous devez inclure une référence à l’espace de noms **System.Runtime.InteropServices** dans votre projet.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetInteropNamespace":::

Initialisez l’interface COM [**IMemoryBufferByteAccess**](/previous-versions/mt297505(v=vs.85)) en ajoutant le code suivant dans votre espace de noms.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetCOMImport":::

Créez un élément **SoftwareBitmap** avec le format de pixel et la taille souhaités. Vous pouvez aussi utiliser un élément **SoftwareBitmap** existant pour lequel vous voulez modifier les données de pixel. Appelez [**SoftwareBitmap. LockBuffer**](/uwp/api/windows.graphics.imaging.softwarebitmap.lockbuffer) pour obtenir une instance de la classe [**BitmapBuffer**](/uwp/api/Windows.Graphics.Imaging.BitmapBuffer) représentant la mémoire tampon de données de pixels. Diffusez l’élément **BitmapBuffer** sur l’interface COM **IMemoryBufferByteAccess**, puis appelez [**IMemoryBufferByteAccess.GetBuffer**](/windows/desktop/WinRT/imemorybufferbyteaccess-getbuffer) pour remplir un tableau d’octets avec des données. Utilisez la méthode [**BitmapBuffer. GetPlaneDescription**](/uwp/api/windows.graphics.imaging.bitmapbuffer.getplanedescription) pour obtenir un objet [**BitmapPlaneDescription**](/uwp/api/Windows.Graphics.Imaging.BitmapPlaneDescription) qui vous permettra de calculer le décalage dans la mémoire tampon pour chaque pixel.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetCreateNewSoftwareBitmap":::

Étant donné que cette méthode accède à la mémoire tampon brute sous-jacente des types Windows Runtime, elle doit être déclarée à l’aide du mot clé **unsafe**. Vous devez également configurer votre projet dans Microsoft Visual Studio pour permettre la compilation du code non sécurisé en ouvrant la page **Propriétés** du projet, en cliquant sur la page de propriétés **Générer**, puis en sélectionnant la case à cocher **Autoriser les blocs de code unsafe**.

## <a name="create-a-softwarebitmap-from-a-direct3d-surface"></a>Créer un élément SoftwareBitmap à partir d’une surface Direct3D

Pour créer un objet **SoftwareBitmap** à partir d’une surface Direct3D, vous devez inclure l’espace de noms [**Windows.Graphics.DirectX.Direct3D11**](/uwp/api/Windows.Graphics.DirectX.Direct3D11) dans votre projet.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetDirect3DNamespace":::

Appelez [**CreateCopyFromSurfaceAsync**](/uwp/api/windows.graphics.imaging.softwarebitmap.createcopyfromsurfaceasync) pour créer un élément **SoftwareBitmap** à partir de la surface. Comme son nom l’indique, le nouvel élément **SoftwareBitmap** contient une copie distincte des données d’image. Les modifications apportées à **SoftwareBitmap** n’auront pas d’effet sur la surface Direct3D.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetCreateSoftwareBitmapFromSurface":::

## <a name="convert-a-softwarebitmap-to-a-different-pixel-format"></a>Convertir un élément SoftwareBitmap dans un format de pixel différent

La classe **SoftwareBitmap** fournit la méthode statique [**Convert**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert) permettant de créer facilement un nouvel élément **SoftwareBitmap**, qui utilise le format pixel et le mode alpha spécifiés à partir d’un élément **SoftwareBitmap** existant. Veuillez noter que l’image bitmap créée contient une copie distincte des données d’image. Les modifications apportées à la nouvelle image bitmap n’affectent pas l’image bitmap source.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetConvert":::

## <a name="transcode-an-image-file"></a>Transcoder un fichier image

Vous pouvez transcoder un fichier image directement à partir d’un [**BitmapDecoder**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) en [**BitmapEncoder**](/uwp/api/Windows.Graphics.Imaging.BitmapEncoder). Créez un [**IRandomAccessStream**](/uwp/api/Windows.Storage.Streams.IRandomAccessStream) à partir du fichier à transcoder. Créez un **BitmapDecoder** à partir du flux d’entrée. Créez un nouveau [**InMemoryRandomAccessStream**](/uwp/api/Windows.Storage.Streams.InMemoryRandomAccessStream) pour l’encodeur à écrire dans et appelez [**BitmapEncoder. CreateForTranscodingAsync**](/uwp/api/windows.graphics.imaging.bitmapencoder.createfortranscodingasync), en passant le flux en mémoire et l’objet décodeur. Les options encode ne sont pas prises en charge lors du transcodage ; au lieu de cela, vous devez utiliser [**CreateAsync**](/uwp/api/windows.graphics.imaging.bitmapencoder.createasync). Toutes les propriétés dans le fichier image d’entrée que vous ne définissez pas spécifiquement sur l’encodeur seront écrites dans le fichier de sortie inchangé. Appelez [**FlushAsync**](/uwp/api/windows.graphics.imaging.bitmapencoder.flushasync) pour faire en sorte que l’encodeur encode le flux en mémoire. Enfin, recherchez le début du flux de fichier et du flux en mémoire et appelez [**CopyAsync**](/uwp/api/windows.storage.streams.randomaccessstream.copyasync) pour écrire le flux en mémoire dans le flux de fichier.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetTranscodeImageFile":::

## <a name="related-topics"></a>Rubriques connexes

* [Référence des options BitmapEncoder](bitmapencoder-options-reference.md)
* [Métadonnées de l’image](image-metadata.md)
 

 
