---
ms.assetid: B5D915E4-4280-422C-BA0E-D574C534410B
description: Cet article décrit comment utiliser les classes SceneAnalysisEffect et FaceDetectionEffect pour analyser le contenu du flux d’aperçu de capture multimédia.
title: Effets d’analyse de cadres caméra
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 56f828268980eb7ccc63a84729365bb5d17a609c
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363752"
---
# <a name="effects-for-analyzing-camera-frames"></a>Effets d’analyse de cadres caméra



Cet article décrit comment utiliser les classes [**SceneAnalysisEffect**](/uwp/api/Windows.Media.Core.SceneAnalysisEffect) et [**FaceDetectionEffect**](/uwp/api/Windows.Media.Core.FaceDetectionEffect) pour analyser le contenu du flux d’aperçu de capture multimédia.

## <a name="scene-analysis-effect"></a>Effet d’analyse de scène

La classe [**SceneAnalysisEffect**](/uwp/api/Windows.Media.Core.SceneAnalysisEffect) analyse les trames vidéo au sein du flux d’aperçu de capture multimédia et recommande le traitement des options pour améliorer le résultat de la capture vidéo. Actuellement, l’effet prend en charge la fonctionnalité visant à détecter si la capture serait améliorée par l’utilisation du traitement HDR (High Dynamic Range).

Si l’effet recommande l’utilisation du traitement HDR, vous pouvez procéder comme suit :

-   Utilisez la classe [**AdvancedPhotoCapture**](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) pour capturer des photos à l’aide de l’algorithme de traitement HDR intégré de Windows. Pour plus d’informations, voir [Capture photo avec plage dynamique élevée (HDR)](high-dynamic-range-hdr-photo-capture.md).

-   Utilisez la classe [**HdrVideoControl**](/uwp/api/Windows.Media.Devices.HdrVideoControl) pour capturer une vidéo à l’aide de l’algorithme de traitement HDR intégré de Windows. Pour plus d’informations, voir [Contrôles de l’appareil de capture pour la vidéo](capture-device-controls-for-video-capture.md).

-   Utilisez la classe [**VariablePhotoSequenceControl**](/uwp/api/Windows.Media.Devices.Core.VariablePhotoSequenceController) pour capturer une séquence de trames que vous pouvez ensuite composer à l’aide d’une implémentation HDR personnalisée. Pour plus d’informations, voir [Séquence de photos variables](variable-photo-sequence.md).

### <a name="scene-analysis-namespaces"></a>Espaces de noms d’analyse de scène

Pour utiliser l’analyse de scène, votre application doit inclure les espaces de noms suivants en plus de ceux nécessaires à la capture multimédia de base.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetSceneAnalysisUsing":::

### <a name="initialize-the-scene-analysis-effect-and-add-it-to-the-preview-stream"></a>Initialiser l’effet d’analyse de scène et l’ajouter au flux d’aperçu

Les effets vidéo sont implémentés à l’aide de deux API, une dédiée à la définition d’effets, qui fournit les paramètres dont l’appareil de capture a besoin pour initialiser l’effet, et l’autre dédiée à l’instance d’effet, qui peut être utilisée pour contrôler l’effet. Pour accéder à l’instance d’effet à partir de plusieurs emplacements au sein de votre code, vous devez généralement déclarer une variable de membre pour stocker l’objet.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetDeclareSceneAnalysisEffect":::

Dans votre application, une fois l’objet **MediaCapture** initialisé, créez une instance de [**SceneAnalysisEffectDefinition**](/uwp/api/Windows.Media.Core.SceneAnalysisEffectDefinition).

Enregistrez l’effet avec l’appareil de capture en appelant [**AddVideoEffectAsync**](/uwp/api/windows.media.capture.mediacapture.addvideoeffectasync) sur votre objet **MediaCapture**, en fournissant l’élément **SceneAnalysisEffectDefinition** et en spécifiant [**MediaStreamType.VideoPreview**](/uwp/api/Windows.Media.Capture.MediaStreamType) pour indiquer que l’effet doit être appliqué au flux d’aperçu vidéo et non au flux de capture. **AddVideoEffectAsync** renvoie une instance de l’effet ajouté. Étant donné que cette méthode peut être utilisée avec plusieurs types d’effets, vous devez diffuser l’instance renvoyée sur un objet [**SceneAnalysisEffect**](/uwp/api/Windows.Media.Core.SceneAnalysisEffect).

Pour recevoir les résultats de l’analyse des scènes, vous devez inscrire un gestionnaire pour l’événement [**SceneAnalyzed**](/uwp/api/windows.media.core.sceneanalysiseffect.sceneanalyzed) .

Actuellement, l’effet d’analyse de scène inclut uniquement l’analyseur HDR. Activez l’analyse HDR en définissant la propriété [**HighDynamicRangeControl.Enabled**](/uwp/api/windows.media.core.highdynamicrangecontrol.enabled) de l’effet sur true.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCreateSceneAnalysisEffectAsync":::

### <a name="implement-the-sceneanalyzed-event-handler"></a>Implémenter le gestionnaire d’événements SceneAnalyzed

Les résultats de l’analyse de scène sont renvoyés dans le gestionnaire d’événements **SceneAnalyzed**. L’objet [**SceneAnalyzedEventArgs**](/uwp/api/Windows.Media.Core.SceneAnalyzedEventArgs) passé dans le gestionnaire a un objet [**SceneAnalysisEffectFrame**](/uwp/api/Windows.Media.Core.SceneAnalysisEffectFrame) qui a un objet [**HighDynamicRangeOutput**](/uwp/api/Windows.Media.Core.HighDynamicRangeOutput) . La propriété [**Certainty**](/uwp/api/windows.media.core.highdynamicrangeoutput.certainty) de la sortie HDR fournit une valeur comprise entre 0 et 1,0, où 0 indique que le traitement HDR ne permettrait pas d’améliorer le résultat de la capture et 1,0 que le traitement HDR pourrait l’améliorer. Vous pouvez choisir le seuil au niveau duquel vous voulez utiliser HDR ou montrer les résultats à l’utilisateur pour le laisser choisir.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetSceneAnalyzed":::

L’objet [**HighDynamicRangeOutput**](/uwp/api/Windows.Media.Core.HighDynamicRangeOutput) passé dans le gestionnaire a également une propriété [**FrameControllers**](/uwp/api/windows.media.core.highdynamicrangeoutput.framecontrollers) qui contient les contrôleurs d’image suggérés pour la capture d’une séquence de photos variable pour le traitement HDR. Pour plus d’informations, voir [Séquence de photos variables](variable-photo-sequence.md).

### <a name="clean-up-the-scene-analysis-effect"></a>Nettoyer l’effet d’analyse de scène

Une fois que votre application a terminé la capture et avant de supprimer l’objet **MediaCapture**, vous devez désactiver l’effet d’analyse de scène en définissant la propriété [**HighDynamicRangeAnalyzer.Enabled**](/uwp/api/windows.media.core.highdynamicrangecontrol.enabled) de l’effet sur false et annuler l’enregistrement de votre gestionnaire d’événements [**SceneAnalyzed**](/uwp/api/windows.media.core.sceneanalysiseffect.sceneanalyzed). Appelez [**MediaCapture.ClearEffectsAsync**](/uwp/api/windows.media.capture.mediacapture.cleareffectsasync) en indiquant le flux d’aperçu vidéo étant donné qu’il s’agissait du flux auquel l’effet a été ajouté. Enfin, définissez la variable de membre sur null.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCleanUpSceneAnalysisEffectAsync":::

## <a name="face-detection-effect"></a>Effet de détection des visages

La classe [**FaceDetectionEffect**](/uwp/api/Windows.Media.Core.FaceDetectionEffect) identifie la position des visages au sein du flux d’aperçu de capture multimédia. L’effet vous permet de recevoir une notification chaque fois qu’un visage est détecté dans le flux d’aperçu et fournit le cadre englobant pour chaque visage détecté dans la trame d’aperçu. Sur les appareils pris en charge, l’effet de détection des visages propose également une amélioration de l’exposition et une mise au point sur le visage le plus important de la scène.

### <a name="face-detection-namespaces"></a>Espaces de noms de détection des visages

Pour utiliser la détection des visages, votre application doit inclure les espaces de noms suivants en plus de ceux nécessaires à la capture multimédia de base.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetFaceDetectionUsing":::

### <a name="initialize-the-face-detection-effect-and-add-it-to-the-preview-stream"></a>Initialiser l’effet de détection des visages et l’ajouter au flux d’aperçu

Les effets vidéo sont implémentés à l’aide de deux API, une dédiée à la définition d’effets, qui fournit les paramètres dont l’appareil de capture a besoin pour initialiser l’effet, et l’autre dédiée à l’instance d’effet, qui peut être utilisée pour contrôler l’effet. Pour accéder à l’instance d’effet à partir de plusieurs emplacements au sein de votre code, vous devez généralement déclarer une variable de membre pour stocker l’objet.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetDeclareFaceDetectionEffect":::

Dans votre application, une fois l’objet **MediaCapture** initialisé, créez une instance de [**FaceDetectionEffectDefinition**](/uwp/api/Windows.Media.Core.FaceDetectionEffectDefinition). Définissez la propriété [**DetectionMode**](/uwp/api/windows.media.core.facedetectioneffectdefinition.detectionmode) pour hiérarchiser la détection de visage plus rapide ou la détection de visage plus précise. Définissez [**SynchronousDetectionEnabled**](/uwp/api/windows.media.core.facedetectioneffectdefinition.synchronousdetectionenabled) pour spécifier que les trames entrantes ne sont pas retardées en attendant la fin de la détection de visage, car cela peut entraîner une expérience de préversion saccadée.

Enregistrez l’effet avec l’appareil de capture en appelant [**AddVideoEffectAsync**](/uwp/api/windows.media.capture.mediacapture.addvideoeffectasync) sur votre objet **MediaCapture**, en fournissant l’élément **FaceDetectionEffectDefinition** et en spécifiant [**MediaStreamType.VideoPreview**](/uwp/api/Windows.Media.Capture.MediaStreamType) pour indiquer que l’effet doit être appliqué au flux d’aperçu vidéo et non au flux de capture. **AddVideoEffectAsync** renvoie une instance de l’effet ajouté. Étant donné que cette méthode peut être utilisée avec plusieurs types d’effets, vous devez effectuer un cast de l’instance retournée en objet [**FaceDetectionEffect**](/uwp/api/Windows.Media.Core.FaceDetectionEffect) .

Activez ou désactivez l’effet en définissant la propriété [**FaceDetectionEffect. Enabled**](/uwp/api/windows.media.core.facedetectioneffect.enabled) . Ajustez la fréquence à laquelle l’effet analyse les trames en définissant la propriété [**FaceDetectionEffect.DesiredDetectionInterval**](/uwp/api/windows.media.core.facedetectioneffect.desireddetectioninterval). Ces deux propriétés peuvent être ajustées lors de la capture multimédia.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCreateFaceDetectionEffectAsync":::

### <a name="receive-notifications-when-faces-are-detected"></a>Recevoir des notifications une fois les visages détectés

Si vous voulez effectuer des actions une fois la détection effectuée, comme dessiner un cadre autour des visages détectés dans l’aperçu vidéo, vous pouvez vous enregistrer pour l’événement [**FaceDetected**](/uwp/api/windows.media.core.facedetectioneffect.facedetected).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetRegisterFaceDetectionHandler":::

Dans le gestionnaire de l’événement, vous pouvez obtenir une liste de tous les visages détectés dans une trame en accédant à la propriété [**FaceDetectionEffectFrame.DetectedFaces**](/uwp/api/windows.media.core.facedetectioneffectframe.detectedfaces) de la classe [**FaceDetectedEventArgs**](/uwp/api/Windows.Media.Core.FaceDetectedEventArgs). La propriété [**FaceBox**](/uwp/api/windows.media.faceanalysis.detectedface.facebox) est une structure [**BitmapBounds**](/uwp/api/Windows.Graphics.Imaging.BitmapBounds) qui décrit le rectangle contenant le visage détecté sous forme d’unités par rapport aux dimensions du flux d’aperçu. Pour afficher l’exemple de code qui transforme les coordonnés du flux d’aperçu en coordonnées d’écran, voir l’[exemple de détection des visages UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFaceDetection).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetFaceDetected":::

### <a name="clean-up-the-face-detection-effect"></a>Nettoyer l’effet de détection des visages

Une fois que votre application a terminé la capture et avant de supprimer l’objet **MediaCapture**, vous devez désactiver l’effet de détection des visages à l’aide de l’élément [**FaceDetectionEffect.Enabled**](/uwp/api/windows.media.core.facedetectioneffect.enabled) et annuler l’enregistrement de votre gestionnaire d’événements [**FaceDetected**](/uwp/api/windows.media.core.facedetectioneffect.facedetected), si vous en aviez enregistré un. Appelez [**MediaCapture.ClearEffectsAsync**](/uwp/api/windows.media.capture.mediacapture.cleareffectsasync) en indiquant le flux d’aperçu vidéo étant donné qu’il s’agissait du flux auquel l’effet a été ajouté. Enfin, définissez la variable de membre sur null.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCleanUpFaceDetectionEffectAsync":::

### <a name="check-for-focus-and-exposure-support-for-detected-faces"></a>Vérifier la prise en charge de la mise au point et de l’exposition pour la détection des visages

Tous les appareils ne disposent pas d’un appareil de capture permettant d’ajuster la mise au point et l’exposition en se basant sur les visages détectés. Dans la mesure où la détection des visages consomme les ressources de l’appareil, vous pouvez l’activer uniquement sur les appareils capables d’utiliser la fonctionnalité pour améliorer la capture. Pour savoir si l’optimisation de la capture basée sur les visages est disponible, obtenez la classe [**VideoDeviceController**](/uwp/api/Windows.Media.Devices.VideoDeviceController) pour votre [MediaCapture](./index.md) initialisé, puis procurez-vous la classe [**RegionsOfInterestControl**](/uwp/api/Windows.Media.Devices.RegionsOfInterestControl) du contrôleur d’appareil vidéo. Vérifiez si le [**MaxRegions**](/uwp/api/windows.media.devices.regionsofinterestcontrol.maxregions) prend en charge au moins une région. Vérifiez ensuite si [**AutoExposureSupported**](/uwp/api/windows.media.devices.regionsofinterestcontrol.autoexposuresupported) ou [**AutoFocusSupported**](/uwp/api/windows.media.devices.regionsofinterestcontrol.autofocussupported) ont la valeur true. Si ces conditions sont réunies, l’appareil peut tirer parti de la détection des visages pour améliorer la capture.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetAreFaceFocusAndExposureSupported":::

## <a name="related-topics"></a>Rubriques connexes

* [Appareil photo](camera.md)
* [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 
