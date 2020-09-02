---
ms.assetid: D6A785C6-DF28-47E6-BDC1-7A7129EC40A0
description: Cet article explique comment utiliser un MediaFrameReader avec MediaCapture pour obtenir des AudioFrames contenant des données audio à partir d’une source de capture.
title: Traiter des trames audio avec MediaFrameReader
ms.date: 04/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 18ef0ee1efb7a69a8b305c9b95e84938fe6fde32
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363892"
---
# <a name="process-audio-frames-with-mediaframereader"></a>Traiter des trames audio avec MediaFrameReader

Cet article explique comment utiliser un [**MediaFrameReader**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) avec [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) pour obtenir des données audio à partir d’une source de cadre de média. Pour en savoir plus sur l’utilisation d’un **MediaFrameReader** pour obtenir des données d’image, par exemple à partir d’une caméra couleur, infrarouge ou profondeur, consultez [traiter des frames multimédias avec MediaFrameReader](process-media-frames-with-mediaframereader.md). Cet article fournit une vue d’ensemble générale du modèle d’utilisation du lecteur de frames et présente quelques fonctionnalités supplémentaires de la classe **MediaFrameReader** , telles que l’utilisation de **MediaFrameSourceGroup** pour récupérer des frames de plusieurs sources en même temps. 

> [!NOTE] 
> Les fonctionnalités présentées dans cet article sont disponibles uniquement à partir de Windows 10, version 1803.

> [!NOTE] 
> Il existe un exemple d’application Windows universelle qui illustre l’utilisation de **MediaFrameReader** pour afficher des images de différentes sources, notamment d’appareils photos couleur, de profondeur et infrarouges. Pour plus d’informations voir [Profils d’appareil photo](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames).

## <a name="setting-up-your-project"></a>Configuration de votre projet
Le processus d’acquisition des images audio est en grande partie le même que l’acquisition d’autres types de trames multimédias. Comme avec toute application utilisant **MediaCapture**, vous devez déclarer que votre application utilise la fonctionnalité *webcam* avant de tenter d’accéder à un appareil photo. Si votre application capture à partir d’un périphérique audio, vous devez également déclarer la fonctionnalité *microphone*. 

**Ajouter des fonctionnalités au manifeste de l’application**

1.  Dans Microsoft Visual Studio, dans l’**Explorateur de solutions**, ouvrez le concepteur pour le manifeste de l’application en double-cliquant sur l’élément **package.appxmanifest**.
2.  Sélectionnez l’onglet **Fonctionnalités**.
3.  Activez les cases à cocher **Webcam** et **Microphone**.
4.  Pour accéder à la bibliothèque d’images et à la vidéothèque, cochez les cases correspondant à **Bibliothèque d’images** et **Vidéothèque**.



## <a name="select-frame-sources-and-frame-source-groups"></a>Sélectionnez des sources d’images et des groupes de sources d’images

La première étape de la capture d’images audio consiste à initialiser un [**MediaFrameSource**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSource) représentant la source des données audio, par exemple un microphone ou un autre périphérique de capture audio. Pour ce faire, vous devez créer une nouvelle instance de l’objet [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) . Pour cet exemple, le seul paramètre d’initialisation de **MediaCapture** consiste à définir le [**StreamingCaptureMode**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.streamingcapturemode) pour indiquer que nous voulons diffuser des données audio à partir du périphérique de capture. 

Après avoir appelé [**MediaCapture.InitializeAsync**](/uwp/api/windows.media.capture.mediacapture.initializeasync), vous pouvez récupérer la liste des sources de cadre de média accessibles à l’aide de la propriété [**FrameSources**](/uwp/api/windows.media.capture.mediacapture.framesources) . Cet exemple utilise une requête LINQ pour sélectionner toutes les sources de frame où le [**MediaFrameSourceInfo**](/uwp/api/windows.media.capture.frames.mediaframesourceinfo) décrivant la source du frame a un  [**MediaStreamType**](/uwp/api/windows.media.capture.frames.mediaframesourceinfo.mediastreamtype) **audio**, indiquant que la source du média produit des données audio.

Si la requête retourne une ou plusieurs sources de frame, vous pouvez vérifier la propriété [**CurrentFormat**](/uwp/api/windows.media.capture.frames.mediaframesource.currentformat) pour voir si la source prend en charge le format audio souhaité, dans cet exemple, des données audio flottantes. Vérifiez le [**AudioEncodingProperties**](/uwp/api/windows.media.capture.frames.mediaframeformat.audioencodingproperties) pour vous assurer que l’encodage audio que vous souhaitez est pris en charge par la source.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetInitAudioFrameSource":::

## <a name="create-and-start-the-mediaframereader"></a>Créer et démarrer le MediaFrameReader

Pour obtenir une nouvelle instance de **MediaFrameReader** , appelez [**MediaCapture. CreateFrameReaderAsync**](/uwp/api/windows.media.capture.mediacapture.createframereaderasync#Windows_Media_Capture_MediaCapture_CreateFrameReaderAsync_Windows_Media_Capture_Frames_MediaFrameSource_), en passant l’objet **MediaFrameSource** que vous avez sélectionné à l’étape précédente. Par défaut, les trames audio sont obtenues en mode de mise en mémoire tampon, ce qui réduit le risque de suppression des frames, bien que cela puisse toujours se produire si vous ne traitez pas les trames audio suffisamment rapidement et que vous remplissez la mémoire tampon d’allocation du système.

Inscrire un gestionnaire pour l’événement [**MediaFrameReader. FrameArrived**](/uwp/api/windows.media.capture.frames.mediaframereader.framearrived) , déclenché par le système lorsqu’un nouveau frame de données audio est disponible. Appelez [**StartAsync**](/uwp/api/windows.media.capture.frames.mediaframereader.startasync) pour commencer l’acquisition des images audio. Si le lecteur de frame ne parvient pas à démarrer, la valeur d’État retournée par l’appel aura une valeur autre que [**réussite**](/uwp/api/windows.media.capture.frames.mediaframereaderstartstatus).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetCreateAudioFrameReader":::

Dans le gestionnaire d’événements **FrameArrived** , appelez [**TryAcquireLatestFrame**](/uwp/api/windows.media.capture.frames.mediaframereader.tryacquirelatestframe) sur l’objet **MediaFrameReader** passé comme expéditeur au gestionnaire pour tenter de récupérer une référence au dernier frame multimédia. Notez que cet objet peut avoir la valeur null. vous devez donc toujours vérifier avant d’utiliser l’objet. Le types de la trame média encapsulé dans le **MediaFrameReference** retourné par **TryAcquireLatestFrame** dépend du type de source de trame ou de sources que vous avez configurées pour acquérir le lecteur de frame. Étant donné que le lecteur de frames de cet exemple a été configuré pour acquérir des images audio, il obtient le frame sous-jacent à l’aide de la propriété [**AudioMediaFrame**](/uwp/api/windows.media.capture.frames.mediaframereference.audiomediaframe) . 

Cette méthode d’assistance **ProcessAudioFrame** dans l’exemple ci-dessous montre comment obtenir un [**AudioFrame**](/uwp/api/windows.media.audioframe) qui fournit des informations telles que l’horodatage du frame et s’il est discontinu de l’objet **AudioMediaFrame** . Pour lire ou traiter les exemples de données audio, vous devez obtenir l’objet [**AudioBuffer**](/uwp/api/windows.media.audiobuffer) à partir de l’objet **AudioMediaFrame** , créer un [**IMemoryBufferReference**](/uwp/api/windows.foundation.imemorybufferreference), puis appeler la méthode com **IMemoryBufferByteAccess :: GetBuffer** pour récupérer les données. Pour plus d’informations sur l’accès aux tampons natifs, consultez la remarque sous la liste de code.

Le format des données dépend de la source du frame. Dans cet exemple, lors de la sélection d’une source de cadre de média, nous avons explicitement fait en sorte que la source du frame sélectionné utilise un canal unique de données float. Le reste de l’exemple de code montre comment déterminer la durée et le nombre d’échantillons pour les données audio dans le frame.  

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetProcessAudioFrame":::

> [!NOTE] 
> Pour pouvoir utiliser les données audio, vous devez accéder à une mémoire tampon de mémoire native. Pour ce faire, vous devez utiliser l’interface com **IMemoryBufferByteAccess** en incluant la liste de code ci-dessous. Les opérations sur la mémoire tampon Native doivent être exécutées dans une méthode qui utilise le mot clé **unsafe** . Vous devez également activer la case à cocher pour autoriser le code unsafe sous l’onglet **générer** de la boîte de dialogue **Propriétés du > du projet** .

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/FrameRenderer.cs" id="SnippetIMemoryBufferByteAccess":::

## <a name="additional-information-on-using-mediaframereader-with-audio-data"></a>Informations supplémentaires sur l’utilisation de MediaFrameReader avec des données audio

Vous pouvez récupérer les [**AudioDeviceController**](/uwp/api/Windows.Media.Devices.AudioDeviceController) associés à la source de trame audio en accédant à la propriété [**MediaFrameSource. Controller**](/uwp/api/windows.media.capture.frames.mediaframesource.controller) . Cet objet peut être utilisé pour obtenir ou définir les propriétés de flux du périphérique de capture ou pour contrôler le niveau de capture. L’exemple suivant active le périphérique audio de manière à ce que les frames continuent d’être acquis par le lecteur de frames, mais tous les exemples ont la valeur 0.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetAudioDeviceControllerMute":::

Vous pouvez utiliser un objet [**AudioFrame**](/uwp/api/windows.media.audioframe) pour transmettre des données audio capturées par une source de cadre multimédia dans un [**AudioGraph**](/uwp/api/windows.media.audio.audiograph). Transmettez le frame dans la méthode [**AddFrame**](/uwp/api/windows.media.audio.audioframeinputnode.addframe) d’un [**AudioFrameInputNode**](/uwp/api/windows.media.audio.audioframeinputnode). Pour plus d’informations sur l’utilisation de graphiques audio pour capturer, traiter et mélanger des signaux audio, consultez [graphiques audio](audio-graphs.md).

## <a name="related-topics"></a>Rubriques connexes

* [Traiter des images multimédias avec MediaFrameReader](process-media-frames-with-mediaframereader.md)
* [Appareil photo](camera.md)
* [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Profils d’appareil photo](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)
* [Graphiques audio](audio-graphs.md)
 
