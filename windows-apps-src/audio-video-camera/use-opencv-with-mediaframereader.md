---
ms.assetid: ''
description: Cet article explique comment utiliser la bibliothèque open source Vision par ordinateur (OpenCV) avec la classe MediaFrameReader.
title: Utiliser OpenCV avec MediaFrameReader
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, openCV
ms.localizationpriority: medium
ms.openlocfilehash: 2128313c48f8d279a23cf63278b3e853c00348e4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175653"
---
# <a name="use-the-open-source-computer-vision-library-opencv-with-mediaframereader"></a>Utiliser la bibliothèque open source Vision par ordinateur (OpenCV) avec MediaFrameReader

Cet article explique comment utiliser la bibliothèque open source Vision par ordinateur (OpenCV), une bibliothèque de code natif qui fournit un large éventail d’algorithmes de traitement d’image, avec la classe [**MediaFrameReader**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) qui peut lire des frames multimédias de plusieurs sources simulataneously. L’exemple de code de cet article vous guide dans la création d’une application simple qui obtient des frames à partir d’un capteur de couleurs, brouille chaque frame à l’aide de la bibliothèque OpenCV, puis affiche l’image traitée dans un contrôle **image** XAML. 

>[!NOTE]
>OpenCV. win. Core et OpenCV. win. ImgProc ne sont pas régulièrement mis à jour et ne réussissent pas les vérifications de conformité de la Banque. par conséquent, ces packages sont destinés uniquement à l’expérimentation.

Cet article s’appuie sur le contenu de deux autres articles :

* [Traiter des frames multimédias avec MediaFrameReader](process-media-frames-with-mediaframereader.md) : cet article fournit des informations détaillées sur l’utilisation de **MediaFrameReader** pour obtenir des frames à partir d’une ou plusieurs sources de cadre de média et décrit, en détail, la plupart des exemples de code de cet article. En particulier, le **traitement des frames multimédias avec MediaFrameReader** fournit la liste de code pour une classe d’assistance, **FrameRenderer**, qui gère la présentation des frames multimédias dans un élément **image** XAML. L’exemple de code de cet article utilise également cette classe d’assistance.

* [Traitement des bitmaps logicielles avec OpenCV](process-software-bitmaps-with-opencv.md) : cet article vous guide tout au long de la création d’un composant Windows Runtime de code natif, **OpenCVBridge**, qui permet de convertir l’objet **SoftwareBitmap** , utilisé par **MediaFrameReader**et le type de **tapis** utilisé par la bibliothèque OpenCV. L’exemple de code de cet article suppose que vous avez suivi les étapes d’ajout du composant **OpenCVBridge** à votre solution d’application UWP.

En plus de ces articles, pour afficher et télécharger un exemple fonctionnel complet de bout en bout du scénario décrit dans cet article, consultez l' [exemple frames de caméra + OpenCV](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraOpenCV) dans le référentiel GitHub des exemples universels Windows.

Pour commencer à développer rapidement, vous pouvez inclure la bibliothèque OpenCV dans un projet d’application UWP à l’aide de packages NuGet, mais ces packages peuvent ne pas réussir le processus de certficication de l’application lorsque vous envoyez votre application au Windows Store. il est donc recommandé de télécharger le code source de la bibliothèque OpenCV et de générer les binaires vous-même avant de soumettre votre application. Vous trouverez des informations sur le développement avec OpenCV à l’adresse [https://opencv.org](https://opencv.org)


## <a name="implement-the-opencvhelper-native-windows-runtime-component"></a>Implémenter le composant Windows Runtime natif OpenCVHelper
Suivez les étapes décrites dans [traiter les bitmaps logicielles avec OpenCV](process-software-bitmaps-with-opencv.md) pour créer le composant d’assistance OpenCV Windows Runtime et ajouter une référence au projet de composant à votre solution d’application UWP.

## <a name="find-available-frame-source-groups"></a>Rechercher des groupes de sources de frame disponibles
Tout d’abord, vous devez rechercher un groupe de sources de cadre de média à partir duquel les images multimédias seront obtenues. Obtient la liste des groupes de sources disponibles sur l’appareil actuel en appelant **[MediaFrameSourceGroup. FindAllAsync](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.FindAllAsync)**. Sélectionnez ensuite les groupes de sources qui fournissent les types de capteurs requis pour votre scénario d’application. Pour cet exemple, nous avons simplement besoin d’un groupe source qui fournit des frames à partir d’une caméra RVB.

[!code-cs[OpenCVFrameSourceGroups](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVFrameSourceGroups)]

## <a name="initialize-the-mediacapture-object"></a>Initialiser l’objet MediaCapture
Ensuite, vous devez initialiser l’objet **MediaCapture** pour utiliser le groupe de sources Frame sélectionné à l’étape précédente en définissant la propriété **[SourceGroup](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.SourceGroup)** de **MediaCaptureInitializationSettings**.

> [!NOTE] 
> La technique utilisée par le composant OpenCVHelper, décrite en détail dans [traiter les bitmaps logicielles avec OpenCV](process-software-bitmaps-with-opencv.md), exige que les données de l’image résident dans la mémoire de l’UC, et non dans la mémoire du GPU. Vous devez donc spécifier **MemoryPreference. CPU** pour le champ **[MemoryPreference](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.MemoryPreference)** de l' **MediaCaptureInitializationSettings**.

Après l’initialisation de **MediaCapture** , obtenir une référence à la source du frame RGB en accédant à la propriété **[MediaCapture. FrameSources](/uwp/api/windows.media.capture.mediacapture.FrameSources)** .

[!code-cs[OpenCVInitMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVInitMediaCapture)]

## <a name="initialize-the-mediaframereader"></a>Initialiser MediaFrameReader
Ensuite, créez un [**MediaFrameReader**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) pour la source de frame RVB Récupérée à l’étape précédente. Pour conserver une fréquence d’images correcte, vous souhaiterez peut-être traiter les trames qui ont une résolution inférieure à la résolution du capteur. Cet exemple fournit l’argument **[BitmapSize](/uwp/api/windows.graphics.imaging.bitmapsize)** facultatif à la méthode **[MediaCapture. CreateFrameReaderAsync](/uwp/api/windows.media.capture.mediacapture.createframereaderasync)** pour demander que les frames fournis par le lecteur de frame soient redimensionnés à 640 x 480 pixels.

Après avoir créé le lecteur de frames, inscrivez un gestionnaire pour l’événement **[FrameArrived](/uwp/api/windows.media.capture.frames.mediaframereader.FrameArrived)** . Créez ensuite un nouvel objet **[SoftwareBitmapSource](/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource)** , que la classe d’assistance **FrameRenderer** utilisera pour présenter l’image traitée. Appelez ensuite le constructeur pour **FrameRenderer**. Initialisez l’instance de la classe **OpenCVHelper** définie dans le composant OpenCVBridge Windows Runtime. Cette classe d’assistance est utilisée dans le gestionnaire **FrameArrived** pour traiter chaque frame. Enfin, démarrez le lecteur de frames en appelant **[StartAsync](/uwp/api/windows.media.capture.frames.mediaframereader.StartAsync)**.

[!code-cs[OpenCVFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVFrameReader)]


## <a name="handle-the-framearrived-event"></a>Gérer l’événement FrameArrived
L’événement **FrameArrived** est déclenché chaque fois qu’un nouveau frame est disponible à partir du lecteur de frame. Appelez **[TryAcquireLatestFrame](/uwp/api/windows.media.capture.frames.mediaframereader.TryAcquireLatestFrame)** pour récupérer le frame, s’il existe. Obtient le **SoftwareBitmap** à partir du **[MediaFrameReference](/uwp/api/windows.media.capture.frames.mediaframereference)**. Notez que la classe **CVHelper** utilisée dans cet exemple requiert que les images utilisent le format de pixel BRGA8 avec l’alpha prémultiplié. Si le format du frame passé dans l’événement est différent, convertissez le **SoftwareBitmap** au format approprié. Ensuite, créez un **SoftwareBitmap** à utiliser comme cible de l’opération de flou. Les propriétés de l’image source sont utilisées comme arguments pour le constructeur afin de créer une image bitmap avec le format correspondant. Appelez la méthode **Blur** de la classe d’assistance pour traiter le frame. Enfin, transmettez l’image de sortie de l’opération de flou dans **PresentSoftwareBitmap**, la méthode de la classe d’assistance **FrameRenderer** qui affiche l’image dans le contrôle d' **image** XAML avec lequel elle a été initialisée.

[!code-cs[OpenCVFrameArrived](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVFrameArrived)]

## <a name="related-topics"></a>Rubriques connexes

* [Appareil photo](camera.md)
* [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Traiter des images multimédias avec MediaFrameReader](process-media-frames-with-mediaframereader.md)
* [Traitement des bitmaps logicielles avec OpenCV](process-software-bitmaps-with-opencv.md)
* [Profils d’appareil photo](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)
* [Trames de l’appareil photo + OpenCV](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraOpenCV)
 

 