---
ms.assetid: 84729E44-10E9-4D7D-8575-6A9D97467ECD
description: Cette rubrique montre comment utiliser FaceDetector pour détecter des visages dans une images. FaceTracker est optimisé pour suivre les visages au fil du temps dans une séquence d’images vidéo.
title: Détecter les visages dans des images ou des vidéos
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b6b2cf937646f41a2e51e6a109d272965817665f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89160883"
---
# <a name="detect-faces-in-images-or-videos"></a>Détecter les visages dans des images ou des vidéos



\[Certaines informations relatives aux produits précommercialisés peuvent être substantiellement modifiées avant leur commercialisation. Microsoft exclut toute garantie, expresse ou implicite, concernant les informations fournies ici.\]

Cette rubrique montre comment utiliser [**FaceDetector**](/uwp/api/Windows.Media.FaceAnalysis.FaceDetector) pour détecter des visages dans une image. Le [**FaceTracker**](/uwp/api/Windows.Media.FaceAnalysis.FaceTracker) est optimisé pour le suivi des visages dans le temps dans une séquence de trames vidéo.

Pour une autre méthode de suivi des visages à l’aide de [**FaceDetectionEffect**](/uwp/api/Windows.Media.Core.FaceDetectionEffect), voir [Analyse de scène pour la capture multimédia](scene-analysis-for-media-capture.md).

Le code contenu dans cet article a été adapté à partir des exemples [Détection des visages de base](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicFaceDetection) et [Suivi des visages de base](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicFaceTracking). Vous pouvez télécharger ces exemples pour voir le code utilisé en contexte ou pour vous en servir comme point de départ pour votre propre application.

## <a name="detect-faces-in-a-single-image"></a>Détecter des visages dans une seule image

La classe [**FaceDetector**](/uwp/api/Windows.Media.FaceAnalysis.FaceDetector) vous permet de détecter une ou plusieurs faces dans une image continue.

Cet exemple utilise des API à partir des espaces de noms suivants.

[!code-cs[FaceDetectionUsing](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetFaceDetectionUsing)]

Déclarez une variable de membre de classe pour l’objet [**FaceDetector**](/uwp/api/Windows.Media.FaceAnalysis.FaceDetector) et pour la liste d’objets [**DetectedFace**](/uwp/api/Windows.Media.FaceAnalysis.DetectedFace) qui seront détectés dans l’image.

[!code-cs[ClassVariables1](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetClassVariables1)]

La détection de visage fonctionne sur un objet [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) qui peut être créé de différentes façons. Dans cet exemple un élément [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) est utilisé pour permettre à l’utilisateur de choisir un fichier image dans lequel des visages sont détectés. Pour plus d’informations sur l’utilisation d’images bitmap de logiciel, voir [Acquisition d’images](imaging.md).

[!code-cs[Picker](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetPicker)]

Utilisez la classe [**BitmapDecoder**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) pour décoder le fichier image dans un **SoftwareBitmap**. Le processus de détection des visages est plus rapide avec une image plus petite, c’est pourquoi vous voudrez peut-être réduire la taille de l’image source. Cette opération peut être réalisée lors du décodage en créant un objet [**BitmapTransform**](/uwp/api/Windows.Graphics.Imaging.BitmapTransform), en définissant les propriétés [**ScaledWidth**](/uwp/api/windows.graphics.imaging.bitmaptransform.scaledwidth) et [**ScaledHeight**](/uwp/api/windows.graphics.imaging.bitmaptransform.scaledheight) et en les transmettant à l’appel à [**GetSoftwareBitmapAsync**](/uwp/api/windows.graphics.imaging.bitmapdecoder.getsoftwarebitmapasync), qui renvoie l’élément **SoftwareBitmap** décodé et mis à l’échelle.

[!code-cs[Decode](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetDecode)]

Dans la version actuelle, la classe **FaceDetector** prend uniquement en charge les images dans Gray8 ou Nv12. La classe **SoftwareBitmap** fournit la méthode [**Convert**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert), qui convertit une image bitmap d’un format vers un autre. Cet exemple convertit l’image source au format de pixel Gray8 si elle n’est pas déjà dans ce format. Si vous le souhaitez, vous pouvez utiliser les méthodes [**GetSupportedBitmapPixelFormats**](/uwp/api/windows.media.faceanalysis.facedetector.getsupportedbitmappixelformats) et [**IsBitmapPixelFormatSupported**](/uwp/api/windows.media.faceanalysis.facedetector.isbitmappixelformatsupported) pour déterminer au moment de l’exécution si un format de pixel est pris en charge, au cas où le jeu de formats pris en charge sera développé dans les versions ultérieures.

[!code-cs[Format](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetFormat)]

Instanciez l’objet **FaceDetector** en appelant [**CreateAsync**](/uwp/api/windows.media.faceanalysis.facedetector.createasync), puis appelez [**DetectFacesAsync**](/uwp/api/windows.media.faceanalysis.facedetector.detectfacesasync), en transmettant l’image bitmap mise à l’échelle à une taille raisonnable et convertie en un format de pixel pris en charge. Cette méthode renvoie une liste d’objets [**DetectedFace**](/uwp/api/Windows.Media.FaceAnalysis.DetectedFace). **ShowDetectedFaces** est une méthode d’assistance, indiquée ci-dessous, qui dessine des cadres autour des visages de l’image.

[!code-cs[Detect](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetDetect)]

Veillez à supprimer les objets qui ont été créés au cours du processus de détection des visages.

[!code-cs[Dispose](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetDispose)]

Pour afficher l’image et dessiner des cadres autour des visages détectés, ajoutez un élément [**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) à votre page XAML.

[!code-xml[Canvas](./code/FaceDetection_Win10/cs/MainPage.xaml#SnippetCanvas)]

Définissez des variables de membre pour styliser les cadres dessinés.

[!code-cs[ClassVariables2](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetClassVariables2)]

Dans la méthode d’assistance **ShowDetectedFaces**, un nouvel élément [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) est créé et la source est définie sur un élément [**SoftwareBitmapSource**](/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) créé à partir de **SoftwareBitmap** représentant l’image source. L’arrière-plan du contrôle **Canvas** XAML est défini sur le pinceau image.

Si la liste de visages transmise à la méthode d’assistance n’est pas vide, parcourez chaque face de la liste et utilisez la propriété [**FaceBox**](/uwp/api/windows.media.faceanalysis.detectedface.facebox) de la classe [**DetectedFace**](/uwp/api/Windows.Media.FaceAnalysis.DetectedFace) pour déterminer la position et la taille du rectangle dans l’image qui contient la face. Dans la mesure où la taille du contrôle **Canvas** est très probablement différente de celle de l’image source, vous devez multiplier les coordonnées X et Y, ainsi que la largeur et la hauteur de **FaceBox** par une valeur de mise à l’échelle qui est le rapport entre la taille de l’image source et la taille réelle du contrôle **Canvas**.

[!code-cs[ShowDetectedFaces](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetShowDetectedFaces)]

## <a name="track-faces-in-a-sequence-of-frames"></a>Suivre les visages dans une séquence d’images

Si vous voulez détecter les visages d’une vidéo, il est plus efficace d’utiliser la classe [**FaceTracker**](/uwp/api/Windows.Media.FaceAnalysis.FaceTracker) que la classe [**FaceDetector**](/uwp/api/Windows.Media.FaceAnalysis.FaceDetector), bien que les étapes d’implémentation soient très similaires. **FaceTracker** utilise les informations d’images traitées précédemment pour optimiser le processus de détection.

[!code-cs[FaceTrackingUsing](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetFaceTrackingUsing)]

Déclarez une variable de classe pour l’objet **FaceTracker**. Cet exemple utilise un élément [**ThreadPoolTimer**](/uwp/api/Windows.System.Threading.ThreadPoolTimer) pour lancer un suivi des visages sur un intervalle défini. [SemaphoreSlim](/dotnet/api/system.threading.semaphoreslim) est utilisé pour s’assurer qu’une seule opération de détection des visages s’exécute à la fois.

[!code-cs[ClassVariables3](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetClassVariables3)]

Pour initialiser l’opération de suivi des visages, créez un nouvel objet **FaceTracker** en appelant [**CreateAsync**](/uwp/api/windows.media.faceanalysis.facetracker.createasync). Initialisez l’intervalle de minuterie souhaité, puis créez le minuteur. La méthode d’assistance **ProcessCurrentVideoFrame** est appelée dès que l’intervalle spécifié est écoulé.

[!code-cs[TrackingInit](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetTrackingInit)]

L’application d’assistance **ProcessCurrentVideoFrame** est appelée de manière asynchrone par le minuteur, si bien qu’elle appelle d’abord la méthode **Wait** du sémaphore pour voir si une opération de suivi est en cours, et si tel est le cas, la méthode revient sans tenter de détecter les visages. À la fin de cette méthode, la méthode **Release** du sémaphore est appelée, ce qui permet la poursuite de l’appel consécutif à **ProcessCurrentVideoFrame**.

La classe [**FaceTracker**](/uwp/api/Windows.Media.FaceAnalysis.FaceTracker) fonctionne sur les objets [**VideoFrame**](/uwp/api/Windows.Media.VideoFrame) . Vous pouvez obtenir un élément **VideoFrame** en capturant une image d’aperçu à partir d’un objet [MediaCapture](./index.md) en cours d’exécution ou en implémentant la méthode [**ProcessFrame**](/uwp/api/windows.media.effects.ibasicaudioeffect.processframe) de [**IBasicVideoEffect**](/uwp/api/Windows.Media.Effects.IBasicVideoEffect). Cet exemple utilise une méthode d’assistance non définie qui renvoie une image vidéo, **GetLatestFrame**, sous la forme d’un espace réservé pour cette opération. Pour en savoir plus sur l’obtention d’images vidéo à partir du flux d’aperçu d’un appareil de capture multimédia en cours d’exécution, voir [Obtenir une image d’aperçu](get-a-preview-frame.md).

Comme avec **FaceDetector**, **FaceTracker** prend en charge un ensemble limité de formats de pixels. Cet exemple abandonne la détection des visages si l’image fournie n’est pas au format Nv12.

Appelez [**ProcessNextFrameAsync**](/uwp/api/windows.media.faceanalysis.facetracker.processnextframeasync) pour récupérer une liste d’objets [**DetectedFace**](/uwp/api/Windows.Media.FaceAnalysis.DetectedFace) représentant les visages dans le frame. Une fois que vous disposez de la liste des visages, vous pouvez les afficher de la même manière que celle décrite ci-dessus pour la détection des visages. Notez que, étant donné que la méthode d’assistance de suivi des visages n’est pas appelée sur le thread d’interface utilisateur, vous devez effectuer toutes les mises à jour de l’interface utilisateur dans un appel de [**CoreDispatcher. RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync).

[!code-cs[ProcessCurrentVideoFrame](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetProcessCurrentVideoFrame)]

## <a name="related-topics"></a>Rubriques connexes

* [Analyse de scène de capture multimédia](scene-analysis-for-media-capture.md)
* [Exemple de détection des visages de base](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicFaceDetection)
* [Exemple de suivi des visages de base](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicFaceTracking)
* [Appareil photo](camera.md)
* [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Lecture de contenu multimédia](media-playback.md)