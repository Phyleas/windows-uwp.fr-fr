---
ms.assetid: EE0C1B28-EF9C-4BD9-A3C0-BDF11E75C752
description: Cet article explique comment les applications UWP peuvent détecter et répondre aux changements initiés par le système dans les niveaux de flux audio.
title: Détecter les changements d’état audio et y répondre
ms.date: 04/03/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a997a3c4d46bbd71c6ac4ae0d78b60edd8bd3ca1
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362722"
---
# <a name="detect-and-respond-to-audio-state-changes"></a>Détecter les changements d’état audio et y répondre
À compter de Windows 10, version 1803, votre application peut détecter si le système diminue ou diminue le niveau audio d’un flux audio utilisé par votre application. Vous pouvez recevoir des notifications pour les flux de capture et de rendu, pour un périphérique audio et une catégorie audio particuliers, ou pour un objet [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) utilisé par votre application pour la lecture du média. Par exemple, le système peut réduire, ou « canard », le niveau de lecture audio lors de la sonnerie d’une alarme. Le système désactive votre application lorsqu’elle passe en arrière-plan si votre application n’a pas déclaré la fonctionnalité *backgroundMediaPlayback* dans le manifeste de l’application. 

Le modèle de gestion des modifications d’État audio est le même pour tous les flux audio pris en charge. Tout d’abord, créez une instance de la classe [**AudioStateMonitor**](/uwp/api/windows.media.audio.audiostatemonitor) . Dans l’exemple suivant, l’application utilise la classe [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) pour capturer l’audio pour Game chat. Une méthode de fabrique est appelée pour obtenir un moniteur d’État audio associé au flux de capture audio de Game chat du périphérique de communication par défaut.  Ensuite, un gestionnaire est inscrit pour l’événement [**SoundLevelChanged**](/uwp/api/windows.media.audio.audiostatemonitor.soundlevelchanged) , qui est déclenché lorsque le niveau audio du flux associé est modifié par le système.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetDeviceIdCategoryVars":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetSoundLevelDeviceIdCategory":::

Dans le gestionnaire d’événements **SoundLevelChanged** , vérifiez la propriété [**SoundLevel**](/uwp/api/windows.media.audio.audiostatemonitor.soundlevel) de l’expéditeur **AudioStateMonitor** passé dans le gestionnaire pour déterminer le nouveau niveau audio du flux. Dans cet exemple, l’application cesse de capturer l’audio lorsque le niveau sonore est muet et reprend la capture lorsque le niveau audio revient au volume complet.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetGameChatSoundLevelChanged":::

Pour plus d’informations sur la capture de données audio avec **MediaCapture**, consultez [capture de photos, de vidéos et de fichiers audio de base avec MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md).

Chaque instance de la classe [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) possède un **AudioStateMonitor** qui lui est associé, que vous pouvez utiliser pour détecter le moment où le système modifie le niveau de volume du contenu en cours de lecture. Vous pouvez décider de gérer les modifications de l’État audio différemment en fonction du type de contenu en cours de lecture. Par exemple, vous pouvez décider de suspendre la lecture d’un podcast lorsque l’audio est abaissé, mais de continuer la lecture si le contenu est musical. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetAudioStateVars":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetSoundLevelChanged":::

Pour plus d’informations sur l’utilisation de **MediaPlayer**, voir [Lire du contenu audio et vidéo avec MediaPlayer](play-audio-and-video-with-mediaplayer.md). 

## <a name="related-topics"></a>Rubriques connexes
