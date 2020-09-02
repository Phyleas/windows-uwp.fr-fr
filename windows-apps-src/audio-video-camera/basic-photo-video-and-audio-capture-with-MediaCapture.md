---
ms.assetid: 66d0c3dc-81f6-4d9a-904b-281f8a334dd0
description: Cet article vous présente le moyen le plus simple de capturer des photos et des vidéos à l’aide de la classe MediaCapture.
title: Capture photo, vidéo et audio de base à l’aide de MediaCapture
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: aecd25eb7f3d9c9f08e2b07d6bd425a00686e0ea
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362952"
---
# <a name="basic-photo-video-and-audio-capture-with-mediacapture"></a>Capture photo, vidéo et audio de base à l’aide de MediaCapture


Cet article présente la méthode la plus simple pour capturer des photos et des vidéos à l’aide de la classe [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) . La classe **MediaCapture** expose un jeu robuste d’API qui fournit un contrôle de niveau inférieur sur le pipeline de capture et prend en charge des scénarios de capture avancés, mais cet article est destiné à vous aider à ajouter rapidement et facilement la capture multimédia à votre application. Pour en savoir plus sur les fonctionnalités fournies par  **MediaCapture** , voir [**camera**](camera.md).

Si vous souhaitez simplement capturer une photo ou une vidéo et que vous n’envisagez pas d’ajouter des fonctionnalités de capture de supports supplémentaires, ou si vous ne souhaitez pas créer votre propre interface utilisateur d’appareil photo, vous pouvez utiliser la classe [**CameraCaptureUI**](/uwp/api/Windows.Media.Capture.CameraCaptureUI) , qui vous permet de lancer simplement l’application de caméra intégrée Windows et de recevoir le fichier photo ou vidéo qui a été capturé. Pour plus d’informations, consultez [ **capturer des photos et des vidéos avec l’interface utilisateur de l’appareil photo intégré Windows**](capture-photos-and-video-with-cameracaptureui.md)

Le code de cet article a été adapté à partir de l’exemple de [**Starter Kit d’appareil photo**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraStarterKit) . Vous pouvez télécharger l’exemple pour voir le code utilisé en contexte ou pour vous en servir comme point de départ pour votre propre application.

## <a name="add-capability-declarations-to-the-app-manifest"></a>Ajouter des déclarations de fonctionnalités au manifeste de l’application

Afin que votre application puisse accéder à l’appareil photo d’un appareil, vous devez déclarer que cette application utilise les fonctionnalités de l’appareil *webcam* et *microphone*. Si vous souhaitez enregistrer dans la bibliothèque d’images ou dans la vidéothèque de l’utilisateur des photos et des vidéos capturées, vous devez également déclarer les fonctionnalités *picturesLibrary* et *videosLibrary*.

**Pour ajouter des fonctionnalités au manifeste de l’application**

1.  Dans Microsoft Visual Studio, dans l’**Explorateur de solutions**, ouvrez le concepteur pour le manifeste de l’application en double-cliquant sur l’élément **package.appxmanifest**.
2.  Sélectionnez l’onglet **Fonctionnalités**.
3.  Activez les cases à cocher **Webcam** et **Microphone**.
4.  Pour accéder à la bibliothèque d’images et à la vidéothèque, cochez les cases correspondant à **Bibliothèque d’images** et **Vidéothèque**.


## <a name="initialize-the-mediacapture-object"></a>Initialiser l’objet MediaCapture
Toutes les méthodes de capture décrites dans cet article nécessitent la première étape d’initialisation de l’objet [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture), exécutée via l’appel du constructeur, puis de [**InitializeAsync**](/uwp/api/windows.media.capture.mediacapture.initializeasync). Dans la mesure où l’objet **MediaCapture** est accessible depuis plusieurs emplacements de votre application, déclarez une variable de classe pour stocker l’objet.  Implémentez un gestionnaire pour l’objet [**Failed**](/uwp/api/windows.media.capture.mediacapture.failed) de la classe **MediaCapture** afin d’être informé d’un éventuel échec de l’opération de capture.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetDeclareMediaCapture":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetInitMediaCapture":::

## <a name="set-up-the-camera-preview"></a>Définissez l’aperçu de l’appareil photo
Il est possible de capturer des photos, vidéos et audio à l’aide de **MediaCapture** sans afficher l’aperçu de l’appareil photo, mais en règle générale, vous souhaitez afficher le flux d’aperçu de manière à ce que l’utilisateur n’ait aucune visibilité sur le contenu capturé. Par ailleurs, quelques fonctions **MediaCapture** nécessitent l’exécution du flux d’aperçu pour être activées. Il s’agit notamment la mise au point, l’exposition et la balance des blancs automatiques. Pour savoir comment configurer l’aperçu de l’appareil photo, consultez la page [**Afficher l’aperçu de l’appareil photo**](simple-camera-preview-access.md).

## <a name="capture-a-photo-to-a-softwarebitmap"></a>Capturer une photo sur une classe SoftwareBitmap
La classe [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) a été introduite dans Windows 10 pour fournir une représentation commune des images sur plusieurs fonctionnalités. Si vous souhaitez capturer une photo avant de l’utiliser immédiatement dans votre application, en l’affichant par exemple au format XAML au lieu de la capturer dans un fichier, vous devez la capturer dans une classe **SoftwareBitmap**. Vous avez toujours la possibilité d’enregistrer l’image sur un disque ultérieurement.

Après l’initialisation de l’objet **MediaCapture**, vous pouvez capturer une photo dans une méthode **SoftwareBitmap** à l’aide de la classe [**LowLagPhotoCapture**](/uwp/api/Windows.Media.Capture.LowLagPhotoCapture). Récupérez une instance de cette classe en appelant [**PrepareLowLagPhotoCaptureAsync**](/uwp/api/windows.media.capture.mediacapture.preparelowlagphotocaptureasync), en passant un objet [**ImageEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.ImageEncodingProperties) spécifiant le format d’image souhaité. [**CreateUncompressed**](/uwp/api/windows.media.mediaproperties.imageencodingproperties.createuncompressed) génère un encodage non compressé avec le format de pixel spécifié. Capturez une photo en appelant [**CaptureAsync**](/uwp/api/windows.media.capture.lowlagphotocapture.captureasync), qui retourne un objet [**CapturedPhoto**](/uwp/api/Windows.Media.Capture.CapturedPhoto) . Récupérez une méthode **SoftwareBitmap** en accédant à la propriété [**Frame**](/uwp/api/windows.media.capture.capturedphoto.frame), puis à la propriété [**SoftwareBitmap**](/uwp/api/windows.media.capture.capturedframe.softwarebitmap).

Si vous le souhaitez, vous pouvez capturer plusieurs photos en appelant à plusieurs reprises **CaptureAsync**. Quand vous avez terminé la capture, appelez [**FinishAsync**](/uwp/api/windows.media.capture.advancedphotocapture.finishasync) afin d’arrêter la session **LowLagPhotoCapture** et de libérer les ressources associées. Après avoir appelé **FinishAsync**, pour recommencer à capturer les photos, vous devrez rappeler [**PrepareLowLagPhotoCaptureAsync**](/uwp/api/windows.media.capture.mediacapture.preparelowlagphotocaptureasync) afin de réinitialiser la session de capture avant d’appeler [**CaptureAsync**](/uwp/api/windows.media.capture.lowlagphotocapture.captureasync).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetCaptureToSoftwareBitmap":::

À compter de Windows, version 1803, vous pouvez accéder à la propriété [**BitmapProperties**](/uwp/api/windows.media.capture.capturedframe.bitmapproperties) de la classe **CapturedFrame** retournée à partir de **CaptureAsync** pour récupérer les métadonnées relatives à la photo capturée. Vous pouvez transmettre ces données dans un **BitmapEncoder** pour enregistrer les métadonnées dans un fichier. Auparavant, il n’existait aucun moyen d’accéder à ces données pour les formats d’image non compressés. Vous pouvez également accéder à la propriété [**ControlValues**](/uwp/api/windows.media.capture.capturedframe.controlvalues) pour récupérer un objet [**CapturedFrameControlValues**](/uwp/api/windows.media.capture.capturedframecontrolvalues) qui décrit les valeurs du contrôle, telles que l’exposition et l’équilibre des blancs, pour le frame capturé.

Pour plus d’informations sur l’utilisation de **BitmapEncoder** et sur l’utilisation de l’objet **SoftwareBitmap** , y compris sur la façon d’en afficher un dans une page XAML, consultez [**créer, modifier et enregistrer des images bitmap**](imaging.md). 

Pour plus d’informations sur la définition des valeurs de contrôle de périphérique de capture, consultez [capturer des contrôles de périphérique pour les photos et les vidéos](capture-device-controls-for-photo-and-video-capture.md).

À compter de Windows 10, version 1803, vous pouvez obtenir les métadonnées, telles que les informations EXIF, pour les photos capturées dans un format non compressé en accédant à la propriété [**BitmapProperties**](/uwp/api/windows.media.capture.capturedframe.bitmapproperties) du **CapturedFrame** retourné par **MediaCapture**. Dans les versions précédentes, ces données étaient accessibles uniquement dans l’en-tête des photos capturées dans un format de fichier compressé. Vous pouvez fournir ces données à un [**BitmapEncoder**](/uwp/api/windows.graphics.imaging.bitmapencoder) lors de l’écriture manuelle d’un fichier image. Pour plus d’informations sur l’encodage des bitmaps, consultez [créer, modifier et enregistrer des images bitmap](imaging.md).  Vous pouvez également accéder aux valeurs du contrôle Frame, telles que les paramètres Exposure et Flash, utilisées lorsque l’image a été capturée en accédant à la propriété [**ControlValues**](/uwp/api/windows.media.capture.capturedframe.controlvalues) . Pour plus d’informations, consultez [capturer des contrôles de périphérique pour la capture de photos et de vidéos](capture-device-controls-for-photo-and-video-capture.md).

## <a name="capture-a-photo-to-a-file"></a>Capturer une photo dans un fichier
Une application de photographie classique enregistre une photo capturée sur un disque ou sur un stockage cloud et doit ajouter des métadonnées, comme l’orientation de la photo, au fichier. L’exemple suivant vous explique comment capturer une photo dans un fichier. Vous avez toujours la possibilité de créer ultérieurement une méthode **SoftwareBitmap** à partir du fichier image. 

La technique décrite dans cet exemple capture la photo dans un flux en mémoire, puis procède à son transcodage sur un fichier sur disque. Cet exemple utilise [**GetLibraryAsync**](/uwp/api/windows.storage.storagelibrary.getlibraryasync) pour obtenir la bibliothèque d’images de l’utilisateur, puis la propriété [**SaveFolder**](/uwp/api/windows.storage.storagelibrary.savefolder) pour obtenir un dossier d’enregistrement par défaut de référence. N’oubliez pas d’ajouter la fonctionnalité **Bibliothèque d’images** à votre manifeste d’application pour accéder à ce dossier. [**CreateFileAsync**](/uwp/api/windows.storage.storagefolder.createfileasync) crée un nouvel élément [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) sur lequel enregistrer la photo.

Créez une classe [**InMemoryRandomAccessStream**](/uwp/api/Windows.Storage.Streams.InMemoryRandomAccessStream), puis appelez [**CapturePhotoToStreamAsync**](/uwp/api/windows.media.capture.mediacapture.capturephototostreamasync) pour capturer une photo dans le flux, en passant le flux et un objet [**ImageEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.ImageEncodingProperties) spécifiant le format d’image à utiliser. Vous pouvez créer des propriétés d’encodage personnalisées en initialisant l’objet vous-même, mais la classe fournit des méthodes statiques, telles que [**ImageEncodingProperties. CreateJpeg**](/uwp/api/windows.media.mediaproperties.imageencodingproperties.createjpeg) pour les formats d’encodage courants. Ensuite, créez un flux de fichier dans le fichier de sortie en appelant [**OpenAsync**](/uwp/api/windows.storage.storagefile.openasync). Créez une classe [**BitmapDecoder**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) pour décoder l’image du flux en mémoire, puis créez une classe [**BitmapEncoder**](/uwp/api/Windows.Graphics.Imaging.BitmapEncoder) afin d’encoder l’image sur un fichier en appelant [**CreateForTranscodingAsync**](/uwp/api/windows.graphics.imaging.bitmapencoder.createfortranscodingasync).

Vous pouvez éventuellement créer un objet [**BitmapPropertySet**](/uwp/api/Windows.Graphics.Imaging.BitmapPropertySet), puis appeler [**SetPropertiesAsync**](/uwp/api/windows.graphics.imaging.bitmapproperties.setpropertiesasync) sur l’encodeur d’image afin d’inclure les métadonnées sur la photo dans le fichier image. Pour plus d’informations sur les propriétés d’encodage, consultez [**métadonnées d’image**](image-metadata.md). Pour la plupart des applications de photographie, une bonne gestion de l’orientation de l’appareil est essentielle. Pour plus d’informations, consultez [**gérer l’orientation de l’appareil avec MediaCapture**](handle-device-orientation-with-mediacapture.md).

Enfin, appelez [**FlushAsync**](/uwp/api/windows.graphics.imaging.bitmapencoder.flushasync) sur l’objet encodeur pour transcoder la photo du flux en mémoire dans le fichier.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetCaptureToFile":::

Pour plus d’utilisation sur l’utilisation des fichiers et des dossiers, consultez la section [**Fichiers, dossiers et bibliothèques**](../files/index.md).

## <a name="capture-a-video"></a>Capturer une vidéo
Ajoutez rapidement une capture vidéo à votre application à l’aide de la classe [**LowLagMediaRecording**](/uwp/api/Windows.Media.Capture.LowLagMediaRecording). Tout d’abord, déclarez une variable de classe associée à l’objet.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetLowLagMediaRecording":::

Ensuite, créez un objet **StorageFile** sur lequel enregistrer la vidéo. Notez que pour procéder à un enregistrement sur la vidéothèque de l’utilisateur, vous devez ajouter la fonctionnalité **Vidéothèque** à votre manifeste d’application. Appelez [**PrepareLowLagRecordToStorageFileAsync**](/uwp/api/windows.media.capture.mediacapture.preparelowlagrecordtostoragefileasync) afin d’initialiser l’enregistrement du contenu multimédia, en passant un fichier de stockage et un objet [**MediaEncodingProfile**](/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile) spécifiant l’encodage pour la vidéo. La classe fournit des méthodes statiques, comme [**CreateMp4**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4), pour créer des profils d’encodage vidéo courants.

Enfin, appelez [**StartAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.startasync) afin de commencer à capturer la vidéo.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetStartVideoCapture":::

Pour arrêter l’enregistrement de la vidéo, appelez [**StopAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.stopasync).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetStopRecording":::

Vous pouvez continuer à appeler **StartAsync** et **StopAsync** pour capturer des vidéos supplémentaires. Lorsque vous avez terminé de capturer des vidéos, appelez [**FinishAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.finishasync) pour supprimer la session de capture et nettoyer les ressources associées. Après cet appel, vous devez appeler de nouveau **PrepareLowLagRecordToStorageFileAsync** pour réinitialiser la session de capture avant d’appeler **StartAsync**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetFinishAsync":::

Lorsque vous capturez de la vidéo, vous devez enregistrer un gestionnaire pour l’événement [**RecordLimitationExceeded**](/uwp/api/windows.media.capture.mediacapture.recordlimitationexceeded) de l’objet **MediaCapture**, qui sera déclenché par la système d’exploitation si vous dépassez la limite d’un enregistrement unique, généralement de trois heures. Dans le gestionnaire de l’événement, vous devez finaliser votre enregistrement en appelant [**StopAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.stopasync).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetRecordLimitationExceeded":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetRecordLimitationExceededHandler":::

### <a name="play-and-edit-captured-video-files"></a>Lire et modifier les fichiers vidéo capturés
Une fois que vous avez capturé une vidéo dans un fichier, vous souhaiterez peut-être charger le fichier et le lire dans l’interface utilisateur de votre application. Pour ce faire, vous pouvez utiliser le contrôle XAML **[MediaPlayerElement](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement)** et un **[MediaPlayer](/uwp/api/windows.media.playback.mediaplayer)** associé. Pour plus d’informations sur la lecture de médias dans une page XAML, consultez [lire des fichiers audio et vidéo avec MediaPlayer](play-audio-and-video-with-mediaplayer.md).

Vous pouvez également créer un objet **[MediaClip](/uwp/api/windows.media.editing.mediaclip)** à partir d’un fichier vidéo en appelant **[CreateFromFileAsync](/uwp/api/windows.media.editing.mediaclip.createfromfileasync)**.  Un **[MediaComposition](/uwp/api/windows.media.editing.mediacomposition)** fournit des fonctionnalités de montage vidéo de base telles que la disposition de la séquence d’objets **MediaClip** , la réduction de la longueur de la vidéo, la création de couches, l’ajout de musique en arrière-plan et l’application d’effets vidéo. Pour plus d’informations sur l’utilisation des compositions multimédias, consultez [compositions et modification de supports](media-compositions-and-editing.md).

## <a name="pause-and-resume-video-recording"></a>Interrompre et reprendre l’enregistrement vidéo
Vous pouvez suspendre un enregistrement vidéo, puis reprendre l’enregistrement sans créer de fichier de sortie distinct en appelant [**PauseAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.pauseasync) , puis en appelant [**ResumeAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.resumeasync).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetPauseRecordingSimple":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetResumeRecordingSimple":::

À partir de Windows 10, version 1607, vous pouvez suspendre un enregistrement vidéo et recevoir la dernière image capturée avant l’interruption. Vous pouvez alors superposer cette image sur l’aperçu de l’appareil photo afin de permettre à l’utilisateur d’aligner l’appareil photo avec l’image interrompue avant de reprendre l’enregistrement. L’appel de [**PauseWithResultAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.pausewithresultasync) retourne un objet [**MediaCapturePauseResult**](/uwp/api/Windows.Media.Capture.MediaCapturePauseResult) . La propriété [**LastFrame**](/uwp/api/windows.media.capture.mediacapturepauseresult.lastframe) est un objet [**VideoFrame**](/uwp/api/Windows.Media.VideoFrame) qui représente le dernier frame. Pour afficher l’image au format XAML, récupérez la représentation **SoftwareBitmap** de l’image vidéo. Actuellement, seules les images au format BGRA8 avec un canal alpha prémultiplié ou vide sont prises en charge, aussi appelez [**Convert**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert) pour récupérer le format approprié, si nécessaire.  Créez un nouvel objet [**SoftwareBitmapSource**](/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource), puis appelez [**SetBitmapAsync**](/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync) pour l’initialiser. Enfin, définissez la propriété **Source** d’un contrôle XAML [**Image**](/uwp/api/Windows.UI.Xaml.Controls.Image) afin d’afficher l’image. Pour que cette astuce fonctionne, votre image doit être alignée sur le contrôle **CaptureElement** et présenter une valeur d’opacité inférieure à 1. N’oubliez pas que vous ne pouvez modifier l’interface utilisateur que sur le thread d’interface utilisateur, par conséquent, effectuez cet appel à l’intérieur de [**RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync).

**PauseWithResultAsync** renvoie également la durée de la vidéo enregistrée dans le segment précédent, si vous souhaitez effectuer un suivi de la durée totale enregistrée.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetPauseCaptureWithResult":::

Quand vous reprenez l’enregistrement, vous pouvez définir la source de l’image sur Null, puis la masquer.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetResumeCaptureWithResult":::

Notez que vous pouvez également récupérer une image résultante quand vous arrêtez la vidéo en appelant [**StopWithResultAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.stopwithresultasync).


## <a name="capture-audio"></a>Capturer l’audio 
Vous pouvez rapidement ajouter la capture audio à votre application en appliquant la technique décrite ci-dessus relative à la capture vidéo. L’exemple ci-dessous est relatif à la création d’un élément **StorageFile** dans le dossier des données de l’application. Appelez [**PrepareLowLagRecordToStorageFileAsync**](/uwp/api/windows.media.capture.mediacapture.preparelowlagrecordtostoragefileasync) pour initialiser la session de capture, en passant le fichier et une classe [**MediaEncodingProfile**](/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile) générée dans l’exemple par la méthode statique [**CreateMp3**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp3). Pour démarrer l’enregistrement, appelez [**StartAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.startasync).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetStartAudioCapture":::


Appelez [**StopAsync**](/uwp/api/windows.media.capture.lowlagphotosequencecapture.stopasync) pour arrêter l’enregistrement audio.

## <a name="related-topics"></a>Rubriques connexes

* [Appareil photo](camera.md)  
:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetStopRecording":::

Vous pouvez appeler **StartAsync** et **StopAsync** à plusieurs reprises pour enregistrer plusieurs fichiers audio. Quand vous avez terminé la capture audio, appelez [**FinishAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.finishasync) pour supprimer la session de capture et nettoyer les ressources associées. Après cet appel, vous devez appeler de nouveau **PrepareLowLagRecordToStorageFileAsync** pour réinitialiser la session de capture avant d’appeler **StartAsync**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetFinishAsync":::


## <a name="detect-and-respond-to-audio-level-changes-by-the-system"></a>Détecter les modifications de niveau audio et y répondre par le système
À compter de Windows 10, version 1803, votre application peut détecter le moment où le système diminue ou désactive le niveau audio de la capture audio de votre application et des flux de rendu audio. Par exemple, le système peut désactiver les flux de votre application lorsqu’il passe en arrière-plan. La classe [**AudioStateMonitor**](/uwp/api/windows.media.audio.audiostatemonitor) vous permet de vous inscrire pour recevoir un événement lorsque le système modifie le volume d’un flux audio. Obtenir une instance de **AudioStateMonitor** pour la surveillance des flux de capture audio en appelant [**CreateForCaptureMonitoring**](/uwp/api/windows.media.audio.audiostatemonitor.createforcapturemonitoring#Windows_Media_Audio_AudioStateMonitor_CreateForCaptureMonitoring). Obtenir une instance pour la surveillance des flux de rendu audio en appelant [**CreateForRenderMonitoring**](/uwp/api/windows.media.audio.audiostatemonitor.createforrendermonitoring). Inscrire un gestionnaire pour l’événement [**SoundLevelChanged**](/uwp/api/windows.media.audio.audiostatemonitor.soundlevelchanged) de chaque analyse à notifier lorsque le système audio pour la catégorie de flux correspondante est modifié par le système.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetAudioStateMonitorUsing":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetAudioStateVars":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetRegisterAudioStateMonitor":::

Dans le gestionnaire **SoundLevelChanged** du flux de capture, vous pouvez vérifier la propriété [**SoundLevel**](/uwp/api/windows.media.audio.audiostatemonitor.soundlevel) de l’expéditeur **AudioStateMonitor** pour déterminer le nouveau niveau sonore. Notez qu’un flux de capture ne doit jamais être abaissé, ou « normalement », par le système. Elle doit uniquement être désactivée ou basculée sur le volume complet. Si le flux audio est muet, vous pouvez arrêter une capture en cours. Si le flux audio est restauré en volume complet, vous pouvez recommencer la capture. L’exemple suivant utilise des variables de classe booléennes pour déterminer si l’application capture actuellement l’audio et si la capture a été arrêtée en raison de la modification de l’État audio. Ces variables sont utilisées pour déterminer quand il est approprié d’arrêter ou de démarrer la capture audio par programmation.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetCaptureSoundLevelChanged":::

L’exemple de code suivant illustre une implémentation du gestionnaire **SoundLevelChanged** pour le rendu audio. En fonction de votre scénario d’application et du type de contenu que vous jouez, vous souhaiterez peut-être suspendre la lecture audio lorsque le niveau sonore est projeté. Pour plus d’informations sur la gestion des modifications de niveau sonore pour la lecture de médias, consultez [lire des fichiers audio et vidéo avec MediaPlayer](play-audio-and-video-with-mediaplayer.md).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetRenderSoundLevelChanged":::


* [Capturer des photos et des vidéos à l’aide de l’interface utilisateur de l’appareil photo intégré à Windows](capture-photos-and-video-with-cameracaptureui.md)
* [Gérer l’orientation de l’appareil à l’aide de MediaCapture](handle-device-orientation-with-mediacapture.md)
* [Créer, modifier et enregistrer des images bitmap](imaging.md)
* [Fichiers, dossiers et bibliothèques](../files/index.md)
