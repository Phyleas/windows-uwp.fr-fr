---
ms.assetid: ''
description: Cet article vous montre comment vous connecter à des caméras distantes et comment obtenir un MediaFrameSourceGroup pour récupérer des images à partir de chaque caméra.
title: Se connecter à des caméras à distance
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 253eea00ba6c4188197224111909c28a53932b88
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257351"
---
# <a name="connect-to-remote-cameras"></a>Se connecter à des caméras à distance

Cet article vous montre comment vous connecter à une ou plusieurs caméras distantes et comment obtenir un objet [**MediaFrameSourceGroup**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup) qui vous permet de lire des images à partir de chaque caméra. Pour plus d’informations sur la lecture des frames à partir d’une source multimédia, consultez [traiter des frames multimédias avec MediaFrameReader](process-media-frames-with-mediaframereader.md). Pour plus d’informations sur le couplage avec des appareils, consultez [coupler des appareils](https://docs.microsoft.com/windows/uwp/devices-sensors/pair-devices).

> [!NOTE] 
> Les fonctionnalités présentées dans cet article sont disponibles uniquement à partir de Windows 10, version 1903.

## <a name="create-a-devicewatcher-class-to-watch-for-available-remote-cameras"></a>Créer une classe DeviceWatcher pour surveiller les caméras distantes disponibles

La classe [**DeviceWatcher**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher) surveille les périphériques disponibles pour votre application et informe votre application de l’ajout ou de la suppression d’appareils. Pour obtenir une instance de **DeviceWatcher** , appelez [**DeviceInformation. CreateWatcher**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher#Windows_Devices_Enumeration_DeviceInformation_CreateWatcher_System_String_), en passant une chaîne de syntaxe de requête avancée (AQS) qui identifie le type de périphériques que vous souhaitez analyser. La chaîne AQS spécifiant les périphériques de caméra réseau est la suivante :

```
@"System.Devices.InterfaceClassGuid:=""{B8238652-B500-41EB-B4F3-4234F7F5AE99}"" AND System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True"
```

> [!NOTE] 
> La méthode d’assistance [**MediaFrameSourceGroup. GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.getdeviceselector) retourne une chaîne AQS qui surveille les caméras réseau distantes et connectées localement. Pour surveiller uniquement les caméras réseau, vous devez utiliser la chaîne AQS indiquée ci-dessus.


Quand vous démarrez le **DeviceWatcher** retourné en appelant la méthode [**Start**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.start) , il déclenche l’événement [**added**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added) pour chaque caméra réseau actuellement disponible. Tant que vous n’avez pas arrêté l’observateur en appelant [**Stop**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.stop), l’événement **added** est déclenché quand de nouveaux périphériques de caméra réseau sont disponibles et l’événement [**removed**](https://docs.microsoft.com/en-us/uwp/api/windows.devices.enumeration.devicewatcher.removed) est déclenché quand un appareil photo n’est plus disponible.

Les arguments d’événement passés dans les gestionnaires d’événements **added** et **removed** sont un objet [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) ou [**DeviceInformationUpdate**](https://docs.microsoft.com/en-us/uwp/api/windows.devices.enumeration.deviceinformationupdate) , respectivement. Chacun de ces objets a une propriété **ID** qui est l’identificateur de la caméra réseau pour laquelle l’événement a été déclenché. Transmettez cet ID dans la méthode [**MediaFrameSourceGroup. FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync) pour obtenir un objet [**MediaFrameSourceGroup**](https://docs.microsoft.com/en-us/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync) que vous pouvez utiliser pour récupérer des frames à partir de l’appareil photo.

## <a name="remote-camera-pairing-helper-class"></a>Classe d’assistance de l’appariement de la caméra distante

L’exemple suivant montre une classe d’assistance qui utilise un **DeviceWatcher** pour créer et mettre à jour un **ObservableCollection** d’objets **MediaFrameSourceGroup** pour prendre en charge la liaison de données à la liste des caméras. Les applications typiques encapsulent le **MediaFrameSourceGroup** dans une classe de modèle personnalisée. Notez que la classe d’assistance gère une référence à l' [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) de l’application et met à jour la collection de caméras dans les appels à [**RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) pour s’assurer que l’interface utilisateur liée à la collection est mise à jour sur le thread d’interface utilisateur.

En outre, cet exemple gère l’événement [**DeviceWatcher. Updated**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.updated) en plus des événements **added** et **removed** . Dans le gestionnaire **mis à jour** , l’appareil photo distant associé est supprimé de, puis rajouté à la collection.

[!code-cs[SnippetRemoteCameraPairingHelper](./code/Frames_Win10/Frames_Win10/RemoteCameraPairingHelper.cs#SnippetRemoteCameraPairingHelper)]


## <a name="related-topics"></a>Rubriques connexes

* [Appareil photo](camera.md)
* [Capture de photos, vidéo et audio de base avec MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Exemple de trames d’appareil photo](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)
* [Traiter des frames multimédias avec MediaFrameReader](process-media-frames-with-mediaframereader.md)
 

 




