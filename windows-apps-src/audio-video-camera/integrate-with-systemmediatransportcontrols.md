---
ms.assetid: eb690f2b-3bf8-4a65-99a4-2a3a8c7760b7
description: Cet article vous explique comment interagir avec les contrôles de transport de média système.
title: Intégration avec les contrôles de transport de média système
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 449c8b445e70ffb68d0bc95f96e2b33c57e3b38f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163933"
---
# <a name="integrate-with-the-system-media-transport-controls"></a>Intégration avec les contrôles de transport de média système

Cet article vous explique comment interagir avec les contrôles de transport de média système. Il s’agit d’un ensemble de contrôles communs à l’ensemble des appareils Windows 10 et qui octroient aux utilisateurs un moyen simple de contrôler la lecture des éléments multimédias pour les applications exécutées qui recourent à [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) pour la lecture.

Les contrôles de transport des médias système permettent aux développeurs d’applications multimédias de s’intégrer à l’interface utilisateur du système intégrée pour afficher les métadonnées du média, telles que l’artiste, le titre de l’album ou le titre du chapitre. Le contrôle de transport du système permet également à un utilisateur de contrôler la lecture d’une application multimédia à l’aide de l’interface utilisateur système intégrée, telle que la suspension de la lecture et l’ignorance et l’arrière dans une sélection.

<img alt="System Media Transtport Controls" src="images/smtc.png" />


Pour obtenir un exemple complet illustrant l’intégration avec les contrôles de transport de média système, consultez l’[exemple de contrôle de transport de média système sur github](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls).
                    
## <a name="automatic-integration-with-smtc"></a>Intégration automatique avec les contrôles de transport de média système
À partir de Windows 10, version 1607, les applications UWP qui utilisent la classe [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) pour lire le contenu multimédia sont automatiquement intégrées par défaut avec les contrôles de transport de média système. Instanciez simplement une nouvelle instance de**MediaPlayer** et affectez un objet [**MediaSource**](/uwp/api/Windows.Media.Core.MediaSource), [**MediaPlaybackItem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem) ou [**MediaPlaybackList**](/uwp/api/Windows.Media.Playback.MediaPlaybackList) sur la propriété [**Source**](/uwp/api/windows.media.playback.mediaplayer.source) du lecteur. Dès lors, l’utilisateur verra le nom de votre application dans les contrôles de transport de média système et sera en mesure de lire et de suspendre le contenu lu, et de se déplacer dans la liste de lecture à l’aide des contrôles de transport de média système. 

Votre application peut créer et utiliser plusieurs objets **MediaPlayer** simultanément. Pour chaque instance **MediaPlayer** active dans votre application, un onglet séparé est créé dans les contrôles de transport de média système. Ici, l’utilisateur peut basculer entre vos différents lecteurs multimédias actifs et ceux d’autres applications en cours d’exécution. Les contrôles de transport de média système affectent le lecteur multimédia actuellement sélectionné.

Pour plus d’informations sur l’utilisation de **MediaPlayer** dans votre application, notamment sur sa liaison à un objet [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) dans votre page XAML, consultez la section [Lire du contenu audio et vidéo avec MediaPlayer](play-audio-and-video-with-mediaplayer.md). 

Pour plus d’informations sur l’utilisation de **MediaSource**, **MediaPlaybackItem** et **MediaPlaybackList**, consultez la section [Éléments, playlists et pistes multimédias](media-playback-with-mediasource.md).

## <a name="add-metadata-to-be-displayed-by-the-smtc"></a>Ajouter des métadonnées à afficher par les contrôles de transport de média système
Si vous souhaitez ajouter ou modifier des métadonnées affichées pour vos éléments multimédias dans les contrôles de transport de média système, comme une vidéo ou un morceau de musique, vous devez mettre à jour les propriétés d’affichage de l’objet **MediaPlaybackItem** représentant votre élément multimédia. Commencez par obtenir une référence à l’objet [**MediaItemDisplayProperties**](/uwp/api/Windows.Media.Playback.MediaItemDisplayProperties) en appelant [**GetDisplayProperties**](/uwp/api/windows.media.playback.mediaplaybackitem.getdisplayproperties). Ensuite, définissez le type de média, de musique ou de vidéo pour l’élément avec la propriété [**type**](/uwp/api/windows.media.playback.mediaitemdisplayproperties.type) . Vous pouvez ensuite remplir les champs [**MusicProperties**](/uwp/api/windows.media.playback.mediaitemdisplayproperties.musicproperties) ou [**VideoProperties**](/uwp/api/windows.media.playback.mediaitemdisplayproperties.videoproperties), en fonction du type de média spécifié. Enfin, mettez à jour les métadonnées associées à l’élément multimédia en appelant [**ApplyDisplayProperties**](/uwp/api/windows.media.playback.mediaplaybackitem.applydisplayproperties).

[!code-cs[SetVideoProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetVideoProperties)]

[!code-cs[SetMusicProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetMusicProperties)]


> [!Note]
> Les applications doivent définir une valeur pour la propriété [**type**](/uwp/api/windows.media.playback.mediaitemdisplayproperties.type) , même si elles ne fournissent pas d’autres métadonnées de média devant être affichées par les contrôles de transport de média système. Cette valeur aide le système à gérer correctement votre contenu multimédia, y compris empêcher l’activation de l’économiseur d’écran pendant la lecture.


## <a name="use-commandmanager-to-modify-or-override-the-default-smtc-commands"></a>Utilisez CommandManager pour modifier ou remplacer les commandes par défaut des contrôles de transport de média système.
Votre application peut modifier ou remplacer complètement le comportement des contrôles de transport de média système avec la classe [**MediaPlaybackCommandManager**](/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManager). Une instance du gestionnaire de commandes peut être récupérée pour chaque instance de la classe **MediaPlayer**, en accédant à la propriété [**CommandManager**](/uwp/api/windows.media.playback.mediaplayer.commandmanager).

Pour chaque commande, comme la commande *Next*, qui par défaut passe à l’élément suivant d’une classe **MediaPlaybackList**, le gestionnaire de commandes expose un événement reçu, comme [**NextReceived**](/uwp/api/windows.media.playback.mediaplaybackcommandmanager.nextreceived), et un objet qui gère le comportement de la commande, comme [**NextBehavior**](/uwp/api/windows.media.playback.mediaplaybackcommandmanager.nextbehavior). 

L’exemple suivant enregistre un gestionnaire pour l’événement **NextReceived** et pour l’événement [**IsEnabledChanged**](/uwp/api/windows.media.playback.mediaplaybackcommandmanagercommandbehavior.isenabledchanged) de **NextBehavior**.

[!code-cs[AddNextHandler](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetAddNextHandler)]

L’exemple suivant illustre un scénario au cours duquel une application tente de désactiver la commande *Next* après que l’utilisateur a cliqué sur cinq éléments de la playlist, en nécessitant peut-être un certain niveau d’interaction de l’utilisateur avant de poursuivre la lecture du contenu. Chaque fois qu’un événement **NextReceived** est déclenché, un compteur est incrémenté. Une fois que le compteur atteint le nombre cible, l’objet [**EnablingRule**](/uwp/api/windows.media.playback.mediaplaybackcommandmanagercommandbehavior.enablingrule) de la commande *Next* est défini sur [**Never**](/uwp/api/Windows.Media.Playback.MediaCommandEnablingRule), auquel cas la commande est désactivée. 

[!code-cs[NextReceived](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetNextReceived)]

Vous pouvez également définir la commande sur **Always**, ce qui signifieu qu’elle sera toujours activée même si, comme c’est le cas avec l’exemple de la commande *Next*, la playlist ne comporte plus d’éléments. Sinon, vous pouvez définir la commande sur **Auto**, auquel cas le système détermine si la commande doit être activée en fonction du contenu actuellement lu.

Pour le scénario décrit ci-dessus, à un moment donné l’application tentera de réactiver la commande *Next* en définissant **EnablingRule** sur **Auto**.

[!code-cs[EnableNextButton](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetEnableNextButton)]

Étant donné que votre application peut avoir sa propre interface utilisateur pour contrôler la lecture pendant qu’elle se trouve au premier plan, vous pouvez utiliser les événements [**IsEnabledChanged**](/uwp/api/windows.media.playback.mediaplaybackcommandmanagercommandbehavior.isenabledchanged) pour mettre à jour votre propre interface utilisateur afin qu’elle corresponde au SMTC, car les commandes sont activées ou désactivées en accédant au [**IsEnabled**](/uwp/api/windows.media.playback.mediaplaybackcommandmanagercommandbehavior.isenabled) du [**MediaPlaybackCommandManagerCommandBehavior**](/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManagerCommandBehavior) passé dans le gestionnaire.

[!code-cs[IsEnabledChanged](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetIsEnabledChanged)]

Dans certains cas, vous voudrez remplacer complètement le comportement de la commande des contrôles de transport de média système. L’exemple suivant illustre un scénario dans lequel une application utilise les commandes *Next* et *Previous* pour basculer entre les différentes stations de radio sur Internet, au lieu de transiter entre les pistes de la playlist actuelle. Comme dans l’exemple précédent, un gestionnaire est inscrit pour lors de la réception d’une commande. dans ce cas, il s’agit de l’événement [**PreviousReceived**](/uwp/api/windows.media.playback.mediaplaybackcommandmanager.previousreceived) .

[!code-cs[AddPreviousHandler](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetAddPreviousHandler)]

Dans le gestionnaire **PreviousReceived** , un [**Report**](/uwp/api/Windows.Foundation.Deferral) est obtenu en appelant le  [**GetDeferral**](/uwp/api/windows.media.playback.mediaplaybackcommandmanagerpreviousreceivedeventargs.getdeferral) du [**MediaPlaybackCommandManagerPreviousReceivedEventArgs**](/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManagerPreviousReceivedEventArgs) passé dans le gestionnaire. Cela signale au système d’attendre la fin du report pour exécuter la commande. Cette procédure est extrêmement importante si vous envisagez de passer des appels asynchrones dans le gestionnaire. À ce stade, l’exemple appelle une méthode personnalisée qui renvoie une classe **MediaPlaybackItem** représentant la station de radio précédente.

Ensuite, la propriété [**Handled**](/uwp/api/windows.media.playback.mediaplaybackcommandmanagerpreviousreceivedeventargs.handled) est examinée afin de garantir que l’événement n’a pas été précédemment traité par un autre gestionnaire. Si ce n’est pas le cas, la propriété **Handled** est définie sur true. Ainsi, les contrôles de transport de média système et tous les autres gestionnaires inscrits savent que cette commande déjà traitée ne doit pas être exécutée. Ensuite, le code définit la nouvelle source pour le lecteur multimédia, qu’il démarre.

Enfin, la commande [**Complete**](/uwp/api/windows.foundation.deferral.complete) est appelée sur l’objet de report, afin d’indiquer au système que le traitement de la commande est terminé.

[!code-cs[PreviousReceived](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetPreviousReceived)]
                 
## <a name="manual-control-of-the-smtc"></a>Contrôle manuel des contrôles de transport de média système
Comme mentionné précédemment dans cet article, les contrôles de transport de média système détectent et affichent automatiquement les informations pour chaque instance de **MediaPlayer** créée par votre application. SI vous souhaitez utiliser plusieurs instances de **MediaPlayer** mais voulez que les contrôles de transport de média système fournissent une seule entrée pour votre application, vous devez définir manuellement le comportement des contrôles de transport de média système, au lieu de vous appuyer sur l’intégration automatique. En outre, si vous utilisez [**MediaTimelineController**](/uwp/api/Windows.Media.MediaTimelineController) pour contrôler un ou plusieurs lecteurs multimédias, vous devez utiliser l’intégration SMTC manuelle. En outre, si votre application utilise une API différente de **MediaPlayer**, comme la classe [**AudioGraph**](/uwp/api/Windows.Media.Audio.AudioGraph) pour lire du contenu multimédia, vous devez implémenter l’intégration manuelle des contrôles de transport de média système pour que l’utilisateur les utilise pour contrôler votre application. Pour plus d’informations sur le contrôle manuel des contrôles de transport de média système, consultez la section [Contrôle manuel des contrôles de transport de média système](system-media-transport-controls.md).



## <a name="related-topics"></a>Rubriques connexes
* [Lecture de contenu multimédia](media-playback.md)
* [Lire du contenu audio et vidéo avec MediaPlayer](play-audio-and-video-with-mediaplayer.md)
* [Contrôle manuel des contrôles de transport de média système](system-media-transport-controls.md)
* [Exemple de contrôles de transport de média système sur github](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls)
 

 