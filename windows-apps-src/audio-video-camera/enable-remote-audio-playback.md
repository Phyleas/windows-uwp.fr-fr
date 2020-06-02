---
description: Cet article explique comment utiliser AudioPlaybackConnection pour permettre aux appareils distants connectés à Bluetooth de lire des données audio sur l’ordinateur local.
title: Activer la lecture audio à partir d’appareils connectés en Bluetooth à distance
ms.date: 05/03/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3d4a4ab7664833308fe059e8bf07f68adea82b3e
ms.sourcegitcommit: cc645386b996f6e59f1ee27583dcd4310f8fb2a6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84262750"
---
# <a name="enable-audio-playback-from-remote-bluetooth-connected-devices"></a>Activer la lecture audio à partir d’appareils connectés en Bluetooth à distance

Cet article explique comment utiliser [AudioPlaybackConnection](/uwp/api/windows.media.audio.audioplaybackconnection) pour permettre aux appareils distants connectés à Bluetooth de lire des données audio sur l’ordinateur local.

À compter de Windows 10, les sources audio distantes de la version 2004 peuvent diffuser du contenu audio sur des appareils Windows, ce qui permet des scénarios tels que la configuration d’un PC pour se comporter comme un haut-parleur Bluetooth et permettre aux utilisateurs d’entendre du son sur leur téléphone. L’implémentation utilise les composants Bluetooth dans le système d’exploitation pour traiter les données audio entrantes et les lire sur les points de terminaison audio du système sur le système, tels que les haut-parleurs de PC intégrés ou les casques câblés. L’activation du récepteur Bluetooth A2DP sous-jacent est gérée par les applications, qui sont responsables du scénario de l’utilisateur final, plutôt que par le système.

La classe [AudioPlaybackConnection](/uwp/api/windows.media.audio.audioplaybackconnection) est utilisée pour activer et désactiver les connexions à partir d’un appareil distant, ainsi que pour créer la connexion, ce qui permet de démarrer la lecture audio à distance.

## <a name="add-a-user-interface"></a>Ajouter une interface utilisateur

Pour les exemples de cet article, nous allons utiliser l’interface utilisateur XAML simple suivante qui définit le contrôle **ListView** pour afficher les périphériques distants disponibles, un **TextBlock** pour afficher l’état de la connexion et trois boutons pour l’activation, la désactivation et l’ouverture des connexions.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml" id="snippet_AudioPlaybackConnectionXAML":::

## <a name="use-devicewatcher-to-monitor-for-remote-devices"></a>Utiliser DeviceWatcher pour surveiller les appareils distants

La classe [DeviceWatcher](/uwp/api/windows.devices.enumeration.devicewatcher) vous permet de détecter des appareils connectés. La méthode [AudioPlaybackConnection. GetDeviceSelector](/uwp/api/windows.media.audio.audioplaybackconnection.getdeviceselector) retourne une chaîne qui indique à l’observateur de l’appareil les genres d’appareils à surveiller. Transmettez cette chaîne dans le constructeur **DeviceWatcher** . 

L’événement [DeviceWatcher. Added](/uwp/api/windows.devices.enumeration.devicewatcher.added) est déclenché pour chaque périphérique connecté lorsque l’observateur de périphérique est démarré, ainsi que pour tout périphérique connecté pendant l’exécution de l’observateur de périphérique. L’événement [DeviceWatcher. removed](/uwp/api/windows.devices.enumeration.devicewatcher.removed) est déclenché si un appareil précédemment connecté se déconnecte. 

Appelez [DeviceWatcher. Start](/uwp/api/windows.devices.enumeration.devicewatcher.start) pour commencer à regarder les appareils connectés qui prennent en charge les connexions de lecture audio. Dans cet exemple, nous allons démarrer le gestionnaire de périphériques lorsque le contrôle de **grille** principal de l’interface utilisateur est chargé. Pour plus d’informations sur l’utilisation de **DeviceWatcher**, consultez [énumérer des appareils](/windows/uwp/devices-sensors/enumerate-devices).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_MainGridLoaded":::


Dans l’événement **ajouté** de l’observateur d’appareil, chaque appareil découvert est représenté par un objet [DeviceInformation](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) . Ajoutez chaque appareil détecté à une collection observable liée au contrôle **ListView** dans l’interface utilisateur.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_DeclareDevices":::


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_DeviceWatcher_Added":::


## <a name="enable-and-release-audio-playback-connections"></a>Activer et libérer des connexions de lecture audio

Avant d’ouvrir une connexion à un appareil, la connexion doit être activée. Cela indique au système qu’il existe une nouvelle application qui veut que l’audio de l’appareil distant soit lu sur le PC, mais que la lecture audio ne commence pas tant que la connexion n’est pas ouverte, ce qui est indiqué dans une étape ultérieure.

Dans le gestionnaire de clic du bouton **activer la connexion de lecture audio** , récupérez l’ID d’appareil associé à l’appareil actuellement sélectionné dans le contrôle **ListView** . Cet exemple gère un dictionnaire d’objets **AudioPlaybackConnection** qui ont été activés. Cette méthode vérifie d’abord s’il existe déjà une entrée dans le dictionnaire pour l’appareil sélectionné. Ensuite, la méthode tente de créer un **AudioPlaybackConnection** pour l’appareil sélectionné en appelant [TryCreateFromId](/uwp/api/windows.media.audio.audioplaybackconnection.trycreatefromid) et en passant l’ID de périphérique sélectionné. 

Si la connexion est correctement créée, ajoutez le nouvel objet **AudioPlaybackConnection** au dictionnaire de l’application, enregistrez un gestionnaire pour l’événement [StateChanged](/uwp/api/windows.media.audio.audioplaybackconnection.statechanged) de l’objet, puis appelez[StartAsync](/uwp/api/windows.media.audio.audioplaybackconnection.startasync) pour informer le système que la nouvelle connexion est activée. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_DeclareConnections":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_EnableAudioPlaybackConnection":::


## <a name="open-the-audio-playback-connection"></a>Ouvrir la connexion de lecture audio

À l’étape précédente, une connexion de lecture audio a été créée, mais le son ne commence pas tant que la connexion n’est pas ouverte en appelant [Open](/uwp/api/windows.media.audio.audioplaybackconnection.open) ou [OpenAsync](/uwp/api/windows.media.audio.audioplaybackconnection.openasync). Dans le bouton de clic de la **connexion de lecture audio ouverte** , récupérez l’appareil actuellement sélectionné et utilisez l’ID pour récupérer le **AudioPlaybackConnection** à partir du dictionnaire de connexions de l’application. Await un appel à **OpenAsync** et vérifiez la valeur d' **État** de l’objet [AudioPlaybackConnectionOpenResultStatus](/uwp/api/windows.media.audio.audioplaybackconnectionopenresult) retourné pour déterminer si la connexion a été ouverte avec succès et, le cas échéant, mettez à jour la zone de texte état de la connexion.


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_OpenAudioPlaybackConnectionButton":::

## <a name="monitor-audio-playback-connection-state"></a>Surveiller l’état de la connexion de lecture audio

L’événement [AudioPlaybackConnection. ConnectionStateChanged](/uwp/api/windows.media.audio.audioplaybackconnection.statechanged) est déclenché chaque fois que l’état de la connexion change. Dans cet exemple, le gestionnaire de cet événement met à jour la zone de texte d’État. N’oubliez pas de mettre à jour l’interface utilisateur à l’intérieur d’un appel à [Dispatcher. RunAsync](/uwp/api/windows.ui.core.coredispatcher.runasync) pour vous assurer que la mise à jour est effectuée sur le thread d’interface utilisateur.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_ConnectionStateChanged":::

## <a name="release-connections-and-handle-removed-devices"></a>Libérer les connexions et gérer les appareils supprimés

Cet exemple fournit un bouton de **connexion de lecture audio** pour permettre à l’utilisateur de libérer une connexion de lecture audio. Dans le gestionnaire de cet événement, nous obtenons l’appareil actuellement sélectionné et utilisons l’ID de l’appareil pour rechercher le **AudioPlaybackConnection** dans le dictionnaire. Appelez **dispose** pour libérer la référence et libérer les ressources associées et supprimer la connexion du dictionnaire.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_ReleaseAudioPlaybackConnectionButton":::

Vous devez gérer le cas où un appareil est supprimé lors de l’activation ou de l’ouverture d’une connexion. Pour ce faire, implémentez un gestionnaire pour l’événement [DeviceWatcher. removed](/uwp/api/windows.devices.enumeration.devicewatcher.removed) de l’observateur de périphérique. Tout d’abord, l’ID de l’appareil supprimé est utilisé pour supprimer l’appareil de la collection observable liée au contrôle **ListView** de l’application. Ensuite, si une connexion associée à ce périphérique se trouve dans le dictionnaire de l’application, **dispose** est appelé pour libérer les ressources associées, puis la connexion est supprimée du dictionnaire. Tout cela est effectué dans un appel à **Dispatcher. RunAsync** pour s’assurer que les mises à jour de l’interface utilisateur sont effectuées sur le thread d’interface utilisateur.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_DeviceWatcher_Removed":::

## <a name="related-topics"></a>Rubriques connexes

[Lecture du média](media-playback.md)


 




