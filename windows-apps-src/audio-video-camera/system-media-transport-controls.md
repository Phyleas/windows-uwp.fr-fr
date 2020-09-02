---
ms.assetid: EFCF84D0-2F4C-454D-97DA-249E9EAA806C
description: La classe SystemMediaTransportControls permet à votre application d’utiliser les contrôles dédiés intégrés à Windows et de mettre à jour les métadonnées affichées par les contrôles concernant le média lu actuellement par votre application.
title: Contrôle manuel des contrôles de transport de média système
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d592b516db32c2602c8b51d82f3ea56c037e5164
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363782"
---
# <a name="manual-control-of-the-system-media-transport-controls"></a>Contrôle manuel des contrôles de transport de média système


À compter de Windows 10, version 1607, les applications UWP qui utilisent la classe [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) pour lire des médias sont automatiquement intégrées aux contrôles de transport de média système (SMTC) par défaut. Il s’agit de la méthode recommandée d’interaction avec les contrôles de transport de média système, pour la plupart des scénarios. Pour plus d’informations sur la personnalisation de l’intégration par défaut des contrôles de transport de média système avec **MediaPlayer**, consultez la section [Intégration avec les contrôles de transport de média système](integrate-with-systemmediatransportcontrols.md).

Certains scénarios peuvent nécessiter l’implémentation d’un contrôle manuel des contrôles de transport de média système. Cela inclut si vous utilisez un [**MediaTimelineController**](/uwp/api/Windows.Media.MediaTimelineController) pour contrôler la lecture d’un ou plusieurs lecteurs multimédias. Cette implémentation est également nécessaire si vous utilisez plusieurs lecteurs multimédias et souhaitez obtenir une seule instance des contrôles de transport de média système pour votre application. Vous devez contrôler manuellement les contrôles de transport de média système si vous utilisez [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) pour lire du contenu multimédia.

## <a name="set-up-transport-controls"></a>Installer les contrôles de transport
Si vous utilisez **MediaPlayer** pour lire du contenu multimédia, vous pouvez obtenir une instance de la classe [**SystemMediaTransportControls**](/uwp/api/Windows.Media.SystemMediaTransportControls) en accédant à la propriété [**MediaPlayer.SystemMediaTransportControls**](/uwp/api/windows.media.playback.mediaplayer.systemmediatransportcontrols). SI vous envisagez de contrôler manuellement les contrôles de transport de média système, vous devez désactiver l’intégration automatique fournie par **MediaPlayer** en définissant la propriété [**CommandManager.IsEnabled**](/uwp/api/windows.media.playback.mediaplaybackcommandmanager.isenabled) sur False.

> [!NOTE] 
> Si vous désactivez l’élément [**MediaPlaybackCommandManager**](/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManager) de l’instance [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) en définissant [**IsEnabled**](/uwp/api/windows.media.playback.mediaplaybackcommandmanager.isenabled) sur false, le lien entre **MediaPlayer** et [**TransportControls**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.transportcontrols) fourni par **MediaPlayerElement** est rompu ; autrement dit, les contrôles de transport intégrés ne contrôleront plus automatiquement la lecture du lecteur. Vous devrez donc implémenter vos propres contrôles pour pouvoir contrôler le **MediaPlayer**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetInitSMTCMediaPlayer":::

Vous pouvez également récupérer une instance de la classe [**SystemMediaTransportControls**](/uwp/api/Windows.Media.SystemMediaTransportControls) en appelant [**GetForCurrentView**](/uwp/api/windows.media.systemmediatransportcontrols.getforcurrentview). Vous devez récupérer l’objet avec cette méthode si vous utilisez **MediaElement** pour lire du contenu multimédia.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetInitSMTCMediaElement":::

Activez les boutons utilisés par votre application en définissant la propriété « is enabled » correspondante de l’objet **SystemMediaTransportControls**, telle que [**IsPlayEnabled**](/uwp/api/windows.media.systemmediatransportcontrols.isplayenabled), [**IsPauseEnabled**](/uwp/api/windows.media.systemmediatransportcontrols.ispauseenabled), [**IsNextEnabled**](/uwp/api/windows.media.systemmediatransportcontrols.isnextenabled) et [**IsPreviousEnabled**](/uwp/api/windows.media.systemmediatransportcontrols.ispreviousenabled). Voir la documentation de référence de **SystemMediaTransportControls** pour obtenir la liste complète des contrôles disponibles.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetEnableContols":::

Inscrire un gestionnaire pour l’événement [**ButtonPressed**](/uwp/api/windows.media.systemmediatransportcontrols.buttonpressed) pour recevoir des notifications lorsque l’utilisateur appuie sur un bouton.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetRegisterButtonPressed":::

## <a name="handle-system-media-transport-controls-button-presses"></a>Gérer les pressions sur les boutons de contrôles de transport de média système

L’événement [**ButtonPressed**](/uwp/api/windows.media.systemmediatransportcontrols.buttonpressed) est déclenché par les contrôles de transport système lorsque l’un des boutons activés est enfoncé. La propriété [**Button**](/uwp/api/windows.media.systemmediatransportcontrolsbuttonpressedeventargs.button) du [**SystemMediaTransportControlsButtonPressedEventArgs**](/uwp/api/Windows.Media.SystemMediaTransportControlsButtonPressedEventArgs) passé dans le gestionnaire d’événements est un membre de l’énumération [**SystemMediaTransportControlsButton**](/uwp/api/Windows.Media.SystemMediaTransportControlsButton) qui indique les boutons activés qui ont été enfoncés.

Pour mettre à jour des objets sur le thread d’interface utilisateur à partir du gestionnaire d’événements [**ButtonPressed**](/uwp/api/windows.media.systemmediatransportcontrols.buttonpressed) , tel qu’un objet [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) , vous devez marshaler les appels par le biais de [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher). Cela vient du fait que le gestionnaire d’événements **ButtonPressed** n’est pas appelé à partir du thread d’interface utilisateur. Par conséquent, une exception est générée si vous tentez de modifier directement l’interface utilisateur.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetSystemMediaTransportControlsButtonPressed":::

## <a name="update-the-system-media-transport-controls-with-the-current-media-status"></a>Mettre à jour les contrôles de transport de média système en tenant compte de l’état actuel du média

Vous devez avertir le [**SystemMediaTransportControls**](/uwp/api/Windows.Media.SystemMediaTransportControls) lorsque l’état du média a changé afin que le système puisse mettre à jour les contrôles pour refléter l’état actuel. Pour ce faire, définissez la propriété [**PlaybackStatus**](/uwp/api/windows.media.systemmediatransportcontrols.playbackstatus) sur la valeur [**MediaPlaybackStatus**](/uwp/api/Windows.Media.MediaPlaybackStatus) appropriée depuis l’événement [**CurrentStateChanged**](/uwp/api/windows.ui.xaml.controls.mediaelement.currentstatechanged) de la classe [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement), qui est déclenché dès que l’état du média change.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetSystemMediaTransportControlsStateChange":::

## <a name="update-the-system-media-transport-controls-with-media-info-and-thumbnails"></a>Mettre à jour les contrôles de transport de média système en tenant compte des informations relatives au média et des miniatures

Utilisez la classe [**SystemMediaTransportControlsDisplayUpdater**](/uwp/api/Windows.Media.SystemMediaTransportControlsDisplayUpdater) pour mettre à jour les informations de média affichées par les contrôles de transport, telles que le titre de la chanson ou la pochette de l’album pour l’élément multimédia en cours de diffusion. Récupérez une instance de cette classe avec la propriété [**SystemMediaTransportControls. DisplayUpdater**](/uwp/api/windows.media.systemmediatransportcontrols.displayupdater) . Pour les scénarios classiques, le mode de transmission des métadonnées recommandé consiste à appeler [**CopyFromFileAsync**](/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.copyfromfileasync) en transmettant le fichier multimédia en cours de lecture. L’outil de mise à jour de l’affichage extrait automatiquement les métadonnées et l’image miniature du fichier.

Appelez la [**mise à jour**](/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.update) pour obliger les contrôles du transport du système multimédia à mettre à jour son interface utilisateur avec les nouvelles métadonnées et miniatures.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetSystemMediaTransportControlsUpdater":::

Si votre scénario l’exige, vous pouvez mettre à jour manuellement les métadonnées affichées par les contrôles de transport de média système en définissant les valeurs des objets [**MusicProperties**](/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.musicproperties), [**ImageProperties**](/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.imageproperties) ou [**VideoProperties**](/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.videoproperties) exposés par la classe [**DisplayUpdater**](/uwp/api/windows.media.systemmediatransportcontrols.displayupdater).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SystemMediaTransportControlsUpdaterManual":::

> [!Note]
> Les applications doivent définir une valeur pour la propriété [SystemMediaTransportControlsDisplayUpdater. type](/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.type#Windows_Media_SystemMediaTransportControlsDisplayUpdater_Type
) , même si elles ne fournissent pas d’autres métadonnées de média devant être affichées par les contrôles de transport de média système. Cette valeur aide le système à gérer correctement votre contenu multimédia, y compris empêcher l’activation de l’économiseur d’écran pendant la lecture.


## <a name="update-the-system-media-transport-controls-timeline-properties"></a>Mettre à jour les propriétés de chronologie des contrôles de transport de média système

Les contrôles de transport système affichent des informations sur la chronologie de l’élément multimédia en cours de lecture, y compris la position de lecture actuelle, son heure de début et son heure de fin. Pour mettre à jour les propriétés de chronologie des contrôles de transport système, créez un nouvel objet [**SystemMediaTransportControlsTimelineProperties**](/uwp/api/Windows.Media.SystemMediaTransportControlsTimelineProperties). Définissez les propriétés de l’objet afin de refléter l’état actuel de l’élément multimédia en cours de lecture. Appelez [**SystemMediaTransportControls. UpdateTimelineProperties**](/uwp/api/windows.media.systemmediatransportcontrols.updatetimelineproperties) pour obliger les contrôles à mettre à jour la chronologie.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetUpdateTimelineProperties":::

-   Vous devez fournir une valeur pour les [**StartTime**](/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.starttime), [**EndTime**](/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.endtime) et [**position**](/uwp/api/windows.media.systemmediatransportcontrols.playbackpositionchangerequested) afin que les contrôles système affichent une chronologie pour votre élément en cours d’exécution.

-   [**MinSeekTime**](/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.minseektime) et [**MaxSeekTime**](/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.maxseektime) vous permettent de spécifier la plage de la chronologie dans laquelle l’utilisateur peut effectuer une recherche. Le scénario classique dans ce cas consiste à permettre aux fournisseurs de contenus d’inclure des pauses publicitaires dans leur contenu multimédia.

    Vous devez définir [**MinSeekTime**](/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.minseektime) et [**MaxSeekTime**](/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.maxseektime) afin de déclencher l’événement [**PositionChangeRequest**](/uwp/api/windows.media.systemmediatransportcontrols.playbackpositionchangerequested).

-   Il est recommandé de synchroniser les contrôles système avec la lecture multimédia en mettant à jour ces propriétés environ toutes les 5 secondes pendant la lecture et à nouveau lors de chaque changement d’état de la lecture, par exemple, lorsque cette dernière est mise en pause ou en cas de recherche d’une nouvelle position.

## <a name="respond-to-player-property-changes"></a>Répondre aux modifications des propriétés du lecteur

Il existe un ensemble de propriétés de contrôles de transport système qui se rapportent à l’état actuel du lecteur multimédia lui-même, plutôt qu’à l’état de l’élément multimédia en cours de lecture. Chacune de ces propriétés est mise en correspondance avec un événement qui est déclenché lorsque l’utilisateur ajuste le contrôle associé. Ces propriétés et événements sont les suivants :

| Propriété                                                                  | Événement                                                                                                   |
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| [**AutoRepeatMode**](/uwp/api/windows.media.systemmediatransportcontrols.autorepeatmode) | [**AutoRepeatModeChangeRequested**](/uwp/api/windows.media.systemmediatransportcontrols.autorepeatmodechangerequested) |
| [**PlaybackRate**](/uwp/api/windows.media.systemmediatransportcontrols.playbackrate)     | [**PlaybackRateChangeRequested**](/uwp/api/windows.media.systemmediatransportcontrols.playbackratechangerequested)     |
| [**ShuffleEnabled**](/uwp/api/windows.media.systemmediatransportcontrols.shuffleenabled) | [**ShuffleEnabledChangeRequested**](/uwp/api/windows.media.systemmediatransportcontrols.shuffleenabledchangerequested) |

 
Pour gérer l’interaction de l’utilisateur avec un de ces contrôles, commencez par enregistrer un gestionnaire pour l’événement associé.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetRegisterPlaybackChangedHandler":::

Dans le gestionnaire de cet événement, commencez par vérifier que la valeur demandée est comprise dans une plage valide et attendue. Si c’est le cas, définissez la propriété correspondante sur [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) , puis définissez la propriété correspondante sur l’objet [**SystemMediaTransportControls**](/uwp/api/Windows.Media.SystemMediaTransportControls) .

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SMTCWin10/cs/MainPage.xaml.cs" id="SnippetPlaybackChangedHandler":::

-   Afin de déclencher un de ces événements de propriété de lecteur, vous devez définir une valeur initiale pour cette propriété. Par exemple, [**PlaybackRateChangeRequested**](/uwp/api/windows.media.systemmediatransportcontrols.playbackratechangerequested) n’est pas déclenché tant que vous n’avez pas défini une valeur pour la propriété [**PlaybackRate**](/uwp/api/windows.media.systemmediatransportcontrols.playbackrate) au moins une fois.

## <a name="use-the-system-media-transport-controls-for-background-audio"></a>Utiliser les contrôles de transport de média système pour le son en arrière-plan

Si vous n’utilisez pas l’intégration automatique des contrôles de transport de média système fournie par **MediaPlayer**, vous devez procéder à une intégration manuelle pour activer le son en arrière-plan. Au minimum, votre application doit activer les boutons lecture et pause en affectant à [**IsPlayEnabled**](/uwp/api/windows.media.systemmediatransportcontrols.isplayenabled) et à [**IsPauseEnabled**](/uwp/api/windows.media.systemmediatransportcontrols.ispauseenabled) la valeur true. Votre application doit également gérer l’événement [**ButtonPressed**](/uwp/api/windows.media.systemmediatransportcontrols.buttonpressed) . Si votre application ne satisfait pas ces exigences, la lecture audio s’arrête quand votre application est déplacée vers l’arrière-plan.

Les applications utilisant le nouveau modèle à processus unique pour l’audio d’arrière-plan doivent récupérer une instance de la classe [**SystemMediaTransportControls**](/uwp/api/Windows.Media.SystemMediaTransportControls), en appelant [**GetForCurrentView**](/uwp/api/windows.media.systemmediatransportcontrols.getforcurrentview). Les applications qui utilisent le modèle à deux processus hérité pour l’audio en arrière-plan doivent utiliser [**BackgroundMediaPlayer.Current.SystemMediaTransportControls**](/uwp/api/windows.media.playback.mediaplayer.systemmediatransportcontrols) pour accéder au SMTC à partir de leur processus en arrière-plan.

Pour plus d’informations sur la lecture de l’audio dans l’arrière-plan, consultez la section [Contenu audio en arrière-plan](background-audio.md).

## <a name="related-topics"></a>Rubriques connexes
* [Lecture de contenu multimédia](media-playback.md)
* [Intégration avec les contrôles de transport de média système](integrate-with-systemmediatransportcontrols.md) 
* [Exemple de transport de média système](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls) 

 
