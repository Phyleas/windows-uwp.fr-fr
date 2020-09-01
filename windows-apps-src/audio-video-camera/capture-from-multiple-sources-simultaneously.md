---
ms.assetid: ''
description: Cet article vous montre comment capturer une vidéo de plusieurs sources simulataneously à un seul fichier avec plusieurs pistes vidéo incorporées.
title: Capture à partir de plusieurs sources à l’aide de MediaFrameSourceGroup
ms.date: 09/12/2017
ms.topic: article
keywords: Windows 10, UWP, capture, vidéo
ms.localizationpriority: medium
ms.openlocfilehash: 78f12137bcb6dbb9984648ce141b4351d592902b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89160913"
---
# <a name="capture-from-multiple-sources-using-mediaframesourcegroup"></a>Capture à partir de plusieurs sources à l’aide de MediaFrameSourceGroup

Cet article vous montre comment capturer des vidéos à partir de plusieurs sources simultanément à un seul fichier avec plusieurs pistes vidéo incorporées. À partir de RS3, vous pouvez spécifier plusieurs objets **[VideoStreamDescriptor](/uwp/api/windows.media.core.videostreamdescriptor)** pour un seul **[MediaEncodingProfile](/uwp/api/windows.media.mediaproperties.mediaencodingprofile)**. Cela vous permet d’encoder plusieurs flux simultanément en un seul fichier. Les flux vidéo qui sont encodés dans cette opération doivent être inclus dans un **[MediaFrameSourceGroup](/uwp/api/windows.media.capture.frames.mediaframesourcegroup)** unique qui spécifie un ensemble de caméras sur l’appareil actuel qui peut être utilisé en même temps. 

Pour plus d’informations sur l’utilisation de **[MediaFrameSourceGroup](/uwp/api/windows.media.capture.frames.mediaframesourcegroup)** avec la classe **[MediaFrameReader](/uwp/api/windows.media.capture.frames.mediaframereader)** pour activer des scénarios de vision d’ordinateur en temps réel qui utilisent plusieurs caméras, consultez [traiter des frames multimédias avec MediaFrameReader](process-media-frames-with-mediaframereader.md).

Le reste de cet article vous guide tout au long des étapes d’enregistrement vidéo de deux caméras de couleur dans un fichier unique avec plusieurs pistes vidéo.

## <a name="find-available-sensor-groups"></a>Rechercher les groupes de capteurs disponibles
Un **MediaFrameSourceGroup** représente une collection de sources de frame, généralement des appareils photo, accessibles à simulataneously. L’ensemble de groupes de sources de frame disponibles est différent pour chaque appareil. la première étape de cet exemple consiste donc à obtenir la liste des groupes de sources de frame disponibles et à en trouver un qui contient les caméras nécessaires pour le scénario, ce qui, dans ce cas, requiert deux caméras couleur.

La méthode **[MediaFrameSourceGroup. FindAllAsync](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.FindAllAsync)** retourne tous les groupes sources disponibles sur l’appareil actuel. Chaque **MediaFrameSourceGroup** retourné contient une liste d’objets **[MediaFrameSourceInfo](/uwp/api/windows.media.capture.frames.mediaframesourceinfo)** qui décrit chaque source de frame dans le groupe. Une requête LINQ est utilisée pour rechercher un groupe source qui contient deux caméras de couleur, l’une sur le panneau avant et l’autre à l’arrière. Un objet anonyme qui contient les **MediaFrameSourceGroup** sélectionnés et les **MediaFrameSourceInfo** pour chaque caméra de couleur est retourné. Au lieu d’utiliser la syntaxe LINQ, vous pouvez à la place parcourir chaque groupe, puis chaque **MediaFrameSourceInfo** pour rechercher un groupe qui répond à vos besoins.

Notez que tous les appareils ne contiennent pas un groupe source qui contient deux caméras couleur. vous devez donc vérifier qu’un groupe source a été trouvé avant de tenter de capturer la vidéo.

[!code-cs[MultiRecordFindSensorGroups](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordFindSensorGroups)]

## <a name="initialize-the-mediacapture-object"></a>Initialiser l’objet MediaCapture
La classe **[MediaCapture](/uwp/api/windows.media.capture.mediacapture)** est la classe principale utilisée pour la plupart des opérations de capture audio, vidéo et photo dans des applications UWP. Initialisez l’objet en appelant **[InitializeAsync](/uwp/api/windows.media.capture.mediacapture.InitializeAsync)**, en passant un objet **[MediaCaptureInitializationSettings](/uwp/api/windows.media.capture.mediacaptureinitializationsettings)** qui contient des paramètres d’initialisation. Dans cet exemple, le seul paramètre spécifié est la propriété **[SourceGroup](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.SourceGroup)** , qui est définie sur le **MediaFrameSourceGroup** récupéré dans l’exemple de code précédent.

Pour plus d’informations sur les autres opérations que vous pouvez effectuer avec **MediaCapture** et d’autres fonctionnalités d’application UWP pour la capture de médias, consultez [camera](camera.md).

[!code-cs[MultiRecordInitMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordInitMediaCapture)]

## <a name="create-a-mediaencodingprofile"></a>Créer un MediaEncodingProfile
La classe **[MediaEncodingProfile](/uwp/api/windows.media.mediaproperties.mediaencodingprofile)** indique au pipeline de capture multimédia la manière dont les données audio et vidéo capturées doivent être encodées au fur et à mesure de leur écriture dans un fichier. Pour les scénarios de capture et de transcodage classiques, cette classe fournit un ensemble de méthodes statiques pour créer des profils communs, tels que **[CreateAvi](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createavi)** et **[CreateMp3](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp3)**. Pour cet exemple, un profil d’encodage est créé manuellement à l’aide d’un conteneur MPEG4 et d’un encodage vidéo H264 –. Les paramètres d’encodage vidéo sont spécifiés à l’aide d’un objet **[VideoEncodingProperties](/uwp/api/windows.media.mediaproperties.videoencodingproperties)** . Pour chaque caméra de couleur utilisée dans ce scénario, un objet **VideoStreamDescriptor** est configuré. Le descripteur est construit avec l’objet **VideoEncodingProperties** qui spécifie l’encodage. La propriété **[label](/uwp/api/windows.media.core.videostreamdescriptor.Label)** de **VideoStreamDescriptor** doit être définie sur l’ID de la source de cadre de média qui sera capturée dans le flux. C’est ainsi que le pipeline de capture sait quel descripteur de flux et quelles propriétés d’encodage doivent être utilisés pour chaque caméra. L’ID de la source du frame est exposé par les objets **[MediaFrameSourceInfo](/uwp/api/windows.media.capture.frames.mediaframesourceinfo)** qui ont été trouvés dans la section précédente, lorsqu’un **MediaFrameSourceGroup** a été sélectionné.


À compter de Windows 10, version 1709, vous pouvez définir plusieurs propriétés d’encodage sur un **MediaEncodingProfile** en appelant **[SetVideoTracks](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.setvideotracks)**. Vous pouvez récupérer la liste des descripteurs de flux vidéo en appelant **[GetVideoTracks](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.GetVideoTracks)**. Notez que si vous définissez la propriété **[Video](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.Video)** , qui stocke un descripteur de flux unique, la liste de descripteurs que vous avez définie en appelant **SetVideoTracks** sera remplacée par une liste contenant le seul descripteur que vous avez spécifié.


[!code-cs[MultiRecordMediaEncodingProfile](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordMediaEncodingProfile)]

### <a name="encode-timed-metadata-in-media-files"></a>Encoder les métadonnées chronométrées dans les fichiers multimédias

À compter de Windows 10, version 1803, en plus de l’audio et de la vidéo, vous pouvez encoder les métadonnées chronométrées dans un fichier multimédia pour lequel le format de données est pris en charge. Par exemple, les métadonnées GoPro (GPMD) peuvent être stockées dans des fichiers MP4 pour transmettre l’emplacement géographique corrélé à un flux vidéo. 

Les métadonnées d’encodage utilisent un modèle qui est parallèle à l’encodage audio ou vidéo. La classe [**TimedMetadataEncodingProperties**](/uwp/api/windows.media.mediaproperties.timedmetadataencodingproperties) décrit les propriétés de type, de sous-type et d’encodage des métadonnées, comme **VideoEncodingProperties** pour la vidéo. Le [**TimedMetadataStreamDescriptor**](/uwp/api/windows.media.core.timedmetadatastreamdescriptor) identifie un flux de métadonnées, tout comme le **VideoStreamDescriptor** pour les flux vidéo.  

L’exemple suivant montre comment initialiser un objet **TimedMetadataStreamDescriptor** . Tout d’abord, un objet **TimedMetadataEncodingProperties** est créé et le sous- **type** est défini sur un GUID qui identifie le type de métadonnées qui sera inclus dans le flux. Cet exemple utilise le GUID pour les métadonnées GoPro (GPMD). La méthode [**SetFormatUserData**](/uwp/api/windows.media.mediaproperties.timedmetadataencodingproperties.setformatuserdata) est appelée pour définir des données spécifiques au format. Pour les fichiers MP4, les données spécifiques au format sont stockées dans la zone SampleDescription (STSD). Ensuite, une nouvelle **TimedMetadataStreamDescriptor** est créée à partir des propriétés d’encodage. Les propriétés d' **étiquette** et de **nom** sont définies pour identifier le flux à encoder. 

[!code-cs[GetStreamDescriptor](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetGetStreamDescriptor)]

Appelez [**MediaEncodingProfile. SetTimedMetadataTracks**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.settimedmetadatatracks) pour ajouter le descripteur de flux de métadonnées au profil d’encodage. L’exemple suivant montre une méthode d’assistance qui prend deux descripteurs de flux vidéo, un descripteur de flux audio et un descripteur de flux de métadonnées minutaux et retourne un **MediaEncodingProfile** qui peut être utilisé pour encoder les flux.

[!code-cs[GetMediaEncodingProfile](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetGetMediaEncodingProfile)]

## <a name="record-using-the-multi-stream-mediaencodingprofile"></a>Enregistrement à l’aide du MediaEncodingProfile à plusieurs flux
La dernière étape de cet exemple consiste à lancer la capture vidéo en appelant **[StartRecordToStorageFileAsync](/uwp/api/windows.media.capture.mediacapture.startrecordtostoragefileasync)**, en transmettant le **StorageFile** sur lequel le média capturé est écrit et le **MediaEncodingProfile** créé dans l’exemple de code précédent. Après avoir attendu quelques secondes, l’enregistrement est arrêté avec un appel à **[StopRecordAsync](/uwp/api/windows.media.capture.mediacapture.StopRecordAsync)**.

[!code-cs[MultiRecordToFile](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordToFile)]

Une fois l’opération terminée, un fichier vidéo contenant la vidéo capturée à partir de chaque caméra encodée en tant que flux distinct dans le fichier est créé. Pour plus d’informations sur la lecture de fichiers multimédias contenant plusieurs pistes vidéo, consultez [éléments multimédias, sélections et pistes](media-playback-with-mediasource.md).

## <a name="related-topics"></a>Rubriques connexes

* [Appareil photo](camera.md)
* [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Traiter des images multimédias avec MediaFrameReader](process-media-frames-with-mediaframereader.md)
* [Éléments, playlists et pistes multimédias](media-playback-with-mediasource.md)


 

 