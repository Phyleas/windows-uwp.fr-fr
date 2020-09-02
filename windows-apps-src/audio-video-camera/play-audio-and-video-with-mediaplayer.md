---
ms.assetid: 58af5e9d-37a1-4f42-909c-db7cb02a0d12
description: Cet article vous explique comment lire du contenu multimédia dans votre application Windows universelle avec MediaPlayer.
title: Lire du contenu audio et vidéo avec MediaPlayer
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ce223d4d70f883545114507ec49fcd9d7084d2a5
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363902"
---
# <a name="play-audio-and-video-with-mediaplayer"></a>Lire du contenu audio et vidéo avec MediaPlayer

Cet article explique comment lire des médias dans votre application Windows universelle à l’aide de la classe  [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) . Avec Windows 10, version 1607, des améliorations significatives ont été apportées aux API de lecture multimédia, y compris une conception simplifiée à processus unique pour l’audio en arrière-plan, l’intégration automatique aux contrôles de transport des médias système (SMTC), la possibilité de synchroniser plusieurs lecteurs multimédias, la possibilité de rendre des images vidéo sur une surface Windows. Pour tirer parti de ces améliorations, la meilleure pratique recommandée pour la lecture de contenus multimédias consiste à utiliser la classe **MediaPlayer** en lieu et place de **MediaElement** pour la lecture de contenu multimédia. Le contrôle XAML léger, [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement), a été introduit pour vous permettre d’afficher le contenu multimédia dans une page XAML. Nombre des API de statut et de contrôle de la lecture fournies par **MediaElement** sont désormais disponibles via le nouvel objet [**MediaPlaybackSession**](/uwp/api/Windows.Media.Playback.MediaPlaybackSession). **MediaElement** continue de fonctionner pour prendre en charge la compatibilité descendante, mais aucune fonction supplémentaire ne sera ajoutée à cette classe.

Cet article vous présente les fonctions **MediaPlayer** qu’une application standard de lecture de contenu multimédia utilise. Notez que **MediaPlayer** utilise la classe [**MediaSource**](/uwp/api/Windows.Media.Core.MediaSource) en tant que conteneur pour l’ensemble des éléments multimédias. Cette classe vous permet de charger et de lire le contenu multimédia à partir de multiples sources différentes utilisant une interface unique, notamment les fichiers locaux, les flux de mémoire et les sources réseau. Il existe également des classes de niveau supérieur compatibles avec **MediaSource**, comme [**MediaPlaybackItem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem) et [**MediaPlaybackList**](/uwp/api/Windows.Media.Playback.MediaPlaybackList), qui fournissent des fonctions plus avancées comme des playlists et la capacité de gestion de sources multimédias avec plusieurs pistes audio, vidéo et de métadonnées. Pour plus d’informations sur **MediaSource** et les API associées, consultez la page [Éléments, playlists et pistes multimédias](media-playback-with-mediasource.md).

> [!NOTE] 
> Les éditions Windows 10 N et Windows 10 KN n’incluent pas les fonctionnalités de média nécessaires à l’utilisation de **MediaPlayer** pour la lecture. Ces fonctionnalités peuvent être installées manuellement. Pour plus d’informations, consultez [Media Feature Pack for Windows 10 N et Windows 10 kN Editions](https://support.microsoft.com/help/3010081/media-feature-pack-for-windows-10-n-and-windows-10-kn-editions).

## <a name="play-a-media-file-with-mediaplayer"></a>Lire un fichier multimédia avec MediaPlayer  
La lecture de contenu multimédia de base avec **MediaPlayer** est très simple à implémenter. Tout d’abord, créez une nouvelle instance de la classe **MediaPlayer**. Votre application peut présenter plusieurs instances **MediaPlayer** actives simultanément. Ensuite, définissez la propriété [**source**](/uwp/api/windows.media.playback.mediaplayer.source) du lecteur sur un objet qui implémente [**IMediaPlaybackSource**](/uwp/api/Windows.Media.Playback.IMediaPlaybackSource), tel qu’un [**MediaSource**](/uwp/api/Windows.Media.Core.MediaSource), un [**MediaPlaybackItem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem)ou un [**MediaPlaybackList**](/uwp/api/Windows.Media.Playback.MediaPlaybackList). Dans cet exemple, un objet **MediaSource** est créé à partir d’un fichier dans le stockage local de l’application, puis un objet **MediaPlaybackItem** est créé à partir de la source, puis affecté à la propriété **Source** du lecteur.

Contrairement à l’objet **MediaElement**, **MediaPlayer** ne démarre pas automatiquement la lecture par défaut. Vous pouvez démarrer la lecture en appelant [**Play**](/uwp/api/windows.media.playback.mediaplayer.play), en affectant à la propriété [**AutoPlay**](/uwp/api/windows.media.playback.mediaplayer.autoplay) la valeur true, ou en attendant que l’utilisateur lance la lecture avec les contrôles multimédia intégrés.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetSimpleFilePlayback":::

Lorsque vous avez terminé d’utiliser une instance **MediaPlayer** sur l’application, vous devez appeler la méthode [**Close**](/uwp/api/windows.media.playback.mediaplayer.close) (projetée vers **Dispose** en C#) afin de nettoyer les ressources utilisées par le lecteur.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetCloseMediaPlayer":::

## <a name="use-mediaplayerelement-to-render-video-in-xaml"></a>Utiliser MediaPlayerElement afin d’afficher du contenu vidéo dans XAML
Vous pouvez lire du contenu multimédia dans une instance **MediaPlayer** sans l’afficher au format XAML, mais de nombreuses applications de lecture de contenus multimédias sont définies pour ce type d’affichage. Pour ce faire, utilisez le contrôle léger [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement). Tout comme **MediaElement**, **MediaPlayerElement** vous permet de spécifier si les contrôles intégrés de transport doivent être affichés.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml" id="SnippetMediaPlayerElementXAML":::

Vous pouvez définir l’instance **MediaPlayer** à laquelle l’élément est lié en appelant [**SetMediaPlayer**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.setmediaplayer).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetSetMediaPlayer":::

Vous pouvez également définir la source de lecture sur l’instance **MediaPlayerElement**. Le cas échéant, l’élément crée automatiquement une nouvelle instance **MediaPlayer** à laquelle vous pouvez accéder à l’aide de la propriété [**MediaPlayer**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.mediaplayer).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetGetPlayerFromElement":::

> [!NOTE] 
> Si vous désactivez l’élément [**MediaPlaybackCommandManager**](/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManager) de l’instance [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) en définissant [**IsEnabled**](/uwp/api/windows.media.playback.mediaplaybackcommandmanager.isenabled) sur false, le lien entre **MediaPlayer** et [**TransportControls**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.transportcontrols) fourni par **MediaPlayerElement** est rompu ; autrement dit, les contrôles de transport intégrés ne contrôleront plus automatiquement la lecture du lecteur. Vous devrez donc implémenter vos propres contrôles pour pouvoir contrôler le **MediaPlayer**.

## <a name="common-mediaplayer-tasks"></a>Tâches courantes de MediaPlayer
Cette section vous explique comment utiliser certaines des fonctionnalités de l’instance **MediaPlayer**.

### <a name="set-the-audio-category"></a>Définir la catégorie audio
Définissez la propriété [**AudioCategory**](/uwp/api/windows.media.playback.mediaplayer.audiocategory) d’une instance **MediaPlayer** sur l’une des valeurs de l’énumération [**MediaPlayerAudioCategory**](/uwp/api/Windows.Media.Playback.MediaPlayerAudioCategory) afin d’indiquer au système le type de contenu multimédia lu. Les jeux doivent classer leurs flux musicaux en tant qu’éléments **GameMedia**, de manière à ce que la musique du jeu soit désactivée automatiquement si de la musique s’active sur une autre application en arrière-plan. Les applications de musique ou de vidéo doivent classer leurs flux en tant qu’éléments **Media** ou **Movie**, de manière à les rendre prioritaires par rapport aux flux **GameMedia**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetSetAudioCategory":::

### <a name="output-to-a-specific-audio-endpoint"></a>Sortie vers un point de terminaison audio spécifique
Par défaut, la sortie audio d’une instance **MediaPlayer** est acheminée vers le point de terminaison audio par défaut du système, mais vous pouvez définir un point de terminaison audio spécifique que l’instance **MediaPlayer** doit utiliser pour la sortie. Dans l’exemple ci-dessous, [**MediaDevice. GetAudioRenderSelector**](/uwp/api/windows.media.devices.mediadevice.getaudiorenderselector) retourne une chaîne qui idenfies de manière unique la catégorie de rendu audio des appareils. Ensuite, la [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) méthode DeviceInformation [**FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) est appelée pour obtenir la liste de tous les appareils disponibles du type sélectionné. Vous pouvez déterminer par programmation l’appareil que vous souhaitez utiliser ou ajouter les périphériques retournés à un [**contrôle ComboBox**](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) pour permettre à l’utilisateur de sélectionner un appareil.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetSetAudioEndpointEnumerate":::

Dans l’événement [**SelectionChanged**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) de la zone déroulante des appareils, la propriété [**AudioDevice**](/uwp/api/windows.media.playback.mediaplayer.audiodevice) de l’instance **MediaPlayer** est définie sur l’appareil sélectionné, qui était stocké dans la propriété [**Tag**](/uwp/api/windows.ui.xaml.frameworkelement.tag) de l’instance **ComboBoxItem**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetSetAudioEndpontSelectionChanged":::

### <a name="playback-session"></a>Session de lecture
Comme décrit précédemment dans cet article, nombre des fonctions exposées par la classe **MediaElement** ont été déplacées vers la classe [**MediaPlaybackSession**](/uwp/api/Windows.Media.Playback.MediaPlaybackSession). Cela concerne les informations sur l’état de lecture du lecteur, comme la position actuelle de lecture, l’action du lecteur (pause ou lecture) et la vitesse de lecture actuelle. **MediaPlaybackSession** fournit également plusieurs événements vous procurant des informations sur les modifications de l’état, notamment sur le statut actuel de mise en mémoire tampon et de téléchargement du contenu lu et sur la taille naturelle et les proportions du contenu vidéo actuellement lu.

L’exemple suivant vous illustre l’implémentation d’un gestionnaire de clic du bouton qui permet d’avancer de 10 secondes dans le contenu. Tout d’abord, l’objet **MediaPlaybackSession** associé au lecteur est récupéré avec la propriété [**PlaybackSession**](/uwp/api/windows.media.playback.mediaplayer.playbacksession). Ensuite, la propriété [**Position**](/uwp/api/windows.media.playback.mediaplaybacksession.position) est définie sur la position actuelle de lecture plus 10 secondes.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetSkipForwardClick":::

L’exemple suivant illustre l’utilisation d’un bouton bascule permettant de passer de la vitesse de lecture normale à la vitesse double, en définissant la propriété [**PlaybackRate**](/uwp/api/windows.media.playback.mediaplaybacksession.playbackrate) de la session.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetSpeedChecked":::

À compter de Windows 10, version 1803, vous pouvez définir la rotation avec laquelle la vidéo est présentée dans le **MediaPlayer** par incréments de 90 degrés.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetSetRotation":::

### <a name="detect-expected-and-unexpected-buffering"></a>Détecter la mise en mémoire tampon attendue et inattendue
L’objet **MediaPlaybackSession** décrit dans la section précédente fournit deux événements pour détecter la date de début et la fin de la mise en mémoire tampon des fichiers multimédias, **[BufferingStarted](/uwp/api/windows.media.playback.mediaplaybacksession.BufferingStarted)** et **[BufferingEnded](/uwp/api/windows.media.playback.mediaplaybacksession.BufferingEnded)**. Cela vous permet de mettre à jour votre interface utilisateur pour indiquer à l’utilisateur que la mise en mémoire tampon se produit. La mise en mémoire tampon initiale est attendue lorsqu’un fichier multimédia est ouvert pour la première fois ou lorsque l’utilisateur bascule vers un nouvel élément dans une sélection. Une mise en mémoire tampon inattendue peut se produire lorsque la vitesse du réseau est dégradée ou si le système de gestion de contenu qui fournit le contenu rencontre des problèmes techniques. À partir de RS3, vous pouvez utiliser l’événement **BufferingStarted** pour déterminer si l’événement de mise en mémoire tampon est attendu ou s’il est inattendu et interrompt la lecture. Vous pouvez utiliser ces informations comme données de télémétrie pour votre application ou le service de distribution des médias. 

Inscrire des gestionnaires pour les événements **BufferingStarted** et **BufferingEnded** pour recevoir des notifications d’état de mise en mémoire tampon.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetRegisterBufferingHandlers":::

Dans le gestionnaire d’événements **BufferingStarted** , effectuez un cast des arguments d’événement passés dans l’événement vers un objet **[MediaPlaybackSessionBufferingStartedEventArgs](/uwp/api/windows.media.playback.mediaplaybacksessionbufferingstartedeventargs)** et vérifiez la propriété **[IsPlaybackInterruption](/uwp/api/windows.media.playback.mediaplaybacksessionbufferingstartedeventargs.IsPlaybackInterruption)** . Si cette valeur est true, la mise en mémoire tampon qui a déclenché l’événement est inattendue et interrompt la lecture. Dans le cas contraire, il s’agit d’une mise en mémoire tampon initiale attendue. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetBufferingHandlers":::


### <a name="pinch-and-zoom-video"></a>Pincement et zoom sur du contenu vidéo
**MediaPlayer** vous permet de spécifier le rectangle source au sein du contenu vidéo à afficher. Dès lors, vous pouvez effectuer un zoom dans la vidéo. Le rectangle que vous spécifiez est relatif à un rectangle normalisé (0,0,1,1), où 0,0 correspond au coin supérieur gauche de l’image et 1,1 spécifie la largeur et la hauteur complètes de l’image. Ainsi, par exemple, pour définir le rectangle de zoom de manière à afficher le quadrant supérieur droit de la vidéo, vous définiriez le rectangle (0,5 ; 0 ; 0,5 ; 0,5).  Il est important que vous vérifiiez vos valeurs afin de vous assurer que votre rectangle source se trouve dans le rectangle normalisé (0,0,1,1). Toute tentative de définition de la valeur en dehors de cette plage provoquera l’envoi d’une exception.

Pour implémenter les fonctionnalités de pincement et de zoom à l’aide d’entrées tactiles multipoint, vous devez dans un premier temps spécifier les entrées prises en charge. Dans cet exemple, les entrées de mise à l’échelle et de translation sont demandées. L’événement [**ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta) est déclenché lorsque l’une des entrées enregistrées se produit. L’événement [**DoubleTapped**](/uwp/api/windows.ui.xaml.uielement.doubletapped) sera utilisé pour réinitialiser le zoom sur l’image complète. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetRegisterPinchZoomEvents":::

Ensuite, déclarez un objet **Rect** qui stockera le rectangle source actuel de zoom.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareSourceRect":::

Le gestionnaire **ManipulationDelta** ajuste la mise à l’échelle ou la translation du rectangle de zoom. Si la valeur delta de mise à l’échelle est différente de 1, cela signifie que l’utilisateur à effectuer un pincement. Si la valeur est supérieure à 1, le rectangle source doit être réduit pour la prise en charge du zoom dans le contenu. Si la valeur est inférieure à 1, le rectangle source doit être agrandi pour effectuer un zoom arrière. Avant de définir les nouvelles valeurs de mise à l’échelle, le rectangle résultant est vérifié pour s’assurer qu’il se situe entièrement dans les limites (0, 0, 1, 1).

Si la valeur de mise à l’échelle est 1, l’entrée de translation est traitée. Le rectangle est simplement translaté selon la division du nombre de pixels de l’entrée par la valeur de largeur et de hauteur du contrôle. Là encore, le rectangle obtenu est examiné afin de garantir qu’il s’intègre parfaitement dans les limites (0,0,1,1).

Enfin, l’élément [**NormalizedSourceRect**](/uwp/api/windows.media.playback.mediaplaybacksession.normalizedsourcerect) de l’instance **MediaPlaybackSession** est défini sur le nouveau rectangle ajusté ; il spécifie la zone de la vidéo à afficher.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetManipulationDelta":::

Dans le gestionnaire d’événements [**DoubleTapped**](/uwp/api/windows.ui.xaml.uielement.doubletapped) , le rectangle source est défini sur (0, 0, 1, 1) pour provoquer le rendu de la totalité de l’image vidéo.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetDoubleTapped":::

**Remarque** Cette section décrit les entrées tactiles. Le pavé tactile envoie des événements de pointeur et n’envoie pas d’événements de manipulation.

### <a name="handling-policy-based-playback-degradation"></a>Gestion de la dégradation de la lecture basée sur des stratégies

Dans certains cas, le système peut dégrader la lecture d’un élément multimédia, par exemple réduire la résolution (la restriction), en fonction d’une stratégie plutôt que d’un problème de performances. Par exemple, la vidéo peut être détériorée par le système si elle est lue à l’aide d’un pilote vidéo non signé. Vous pouvez appeler [**MediaPlaybackSession. GetOutputDegradationPolicyState**](/uwp/api/windows.media.playback.mediaplaybacksession.getoutputdegradationpolicystate#Windows_Media_Playback_MediaPlaybackSession_GetOutputDegradationPolicyState) pour déterminer si et pourquoi cette dégradation basée sur une stratégie est en cours et avertir l’utilisateur ou enregistrer la raison de la télémétrie.

L’exemple suivant montre une implémentation d’un gestionnaire pour l’événement **MediaPlayer. MediaOpened** qui est déclenché lorsque le joueur ouvre un nouvel élément multimédia. **GetOutputDegradationPolicyState** est appelé sur le **MediaPlayer** passé dans le gestionnaire. La valeur de [**VideoConstrictionReason**](/uwp/api/windows.media.playback.mediaplaybacksessionoutputdegradationpolicystate.videoconstrictionreason#Windows_Media_Playback_MediaPlaybackSessionOutputDegradationPolicyState_VideoConstrictionReason) indique la raison de la stratégie pour laquelle la vidéo est restreinte. Si la valeur n’est pas **None**, cet exemple journalise la raison de la dégradation à des fins de télémétrie. Cet exemple montre également comment définir le débit binaire de la **AdaptiveMediaSource** actuellement jouée sur la bande passante la plus basse pour enregistrer l’utilisation des données, car la vidéo est restreinte et ne s’affiche pas quand même à la haute résolution. Pour plus d’informations sur l’utilisation de **AdaptiveMediaSource**, consultez [streaming adaptative](adaptive-streaming.md).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetPolicyDegradation":::
        
## <a name="use-mediaplayersurface-to-render-video-to-a-windowsuicomposition-surface"></a>Utilisez MediaPlayerSurface afin d’afficher la vidéo sur une surface Windows.UI.Composition
À partir de Windows 10, version 1607, vous pouvez utiliser l’instance **MediaPlayer** afin d’afficher le contenu vidéo sur une interface [**ICompositionSurface**](/uwp/api/Windows.UI.Composition.ICompositionSurface), ce qui permet au lecteur d’intergir avec les API dans l’espace de noms [**Windows.UI.Composition**](/uwp/api/Windows.UI.Composition). L’infrastructure de composition peut être mise à profit pour travailler avec des graphiques dans la couche visuelle située entre les API XAML et les API graphiques DirectX de niveau inférieur. Cela permet la prise en charge de scénarios tels que le rendu de contenus vidéo dans tout contrôle XAML. Pour plus d’informations sur l’utilisation des API de composition, consultez la page [Couche visuelle](../composition/visual-layer.md).

L’exemple suivant illustre l’affichage d’un contenu de lecteur vidéo sur un contrôle [**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas). Les appels spécifiques au lecteur multimédias de cet exemple sont [**SetSurfaceSize**](/uwp/api/windows.media.playback.mediaplayer.setsurfacesize) et [**GetSurface**](/uwp/api/windows.media.playback.mediaplayer.getsurface). **SetSurfaceSize** indique au système la talle de la mémoire tampon à allouer pour l’affichage du contenu. **GetSurface** prend un élément [**Compositor**](/uwp/api/Windows.UI.Composition.Compositor) en tant qu’argument et récupère une instance de la classe [**MediaPlayerSurface**](/uwp/api/Windows.Media.Playback.MediaPlayerSurface). Cette classe procure un accès aux éléments **MediaPlayer** et **Compositor** utilisés pour créer la surface et l’expose via la propriété [**CompositionSurface**](/uwp/api/windows.media.playback.mediaplayersurface.compositionsurface).

Le reste du code de cet exemple crée un [**SpriteVisual**](/uwp/api/Windows.UI.Composition.SpriteVisual) dans lequel la vidéo est rendue et définit la taille de l’élément Canvas qui affichera l’élément visuel. Ensuite, un élément [**CompositionBrush**](/uwp/api/Windows.UI.Composition.CompositionBrush) est créé à partir de la classe [**MediaPlayerSurface**](/uwp/api/Windows.Media.Playback.MediaPlayerSurface) et affecté à la propriété [**Brush**](/uwp/api/windows.ui.composition.spritevisual.brush) des éléments visuels. À ce stade, un élément [**ContainerVisual**](/uwp/api/Windows.UI.Composition.ContainerVisual) est créé et l’instance **SpriteVisual** est insérée dans la partie supérieure de son arborescence visuelle. Enfin, [**SetElementChildVisual**](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.setelementchildvisual) est appelé afin d’affecter les éléments visuels du conteneur au contrôle **Canvas**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetCompositor":::
        
## <a name="use-mediatimelinecontroller-to-synchronize-content-across-multiple-players"></a>Utilisez MediaTimelineController afin de synchroniser du contenu entre plusieurs couches.
Comme indiqué précédemment dans cet article, vous application peut disposer de plusieurs objets **MediaPlayer** actifs simultanément. Par défaut, chaque instance **MediaPlayer** créée fonctionne indépendamment. Pour certains scénarios, tels que la synchronisation d’une piste de commentaires sur une vidéo, vous voudrez synchroniser l’état du lecteur, la position de lecture et la vitesse de lecture de plusieurs couches. À partir de Windows 10, version 1607, vous pouvez implémenter ce comportement à l’aide de la classe [**MediaTimelineController**](/uwp/api/Windows.Media.MediaTimelineController).

### <a name="implement-playback-controls"></a>Implémenter des contrôles de lecture
L’exemple suivant vous explique l’utilisation d’une classe **MediaTimelineController** pour contrôler deux instances de **MediaPlayer**. Tout d’abord, chaque instance de **MediaPlayer** est instanciée et l’objet **Source** est défini sur un fichier multimédia. Ensuite, une nouvelle classe **MediaTimelineController** est créée. Pour chaque instance **MediaPlayer**, la classe [**MediaPlaybackCommandManager**](/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManager) associée à chaque lecteur est désactivée par la définition de la propriété [**IsEnabled**](/uwp/api/windows.media.playback.mediaplaybackcommandmanager.isenabled) sur False. La propriété [**TimelineController**](/uwp/api/windows.media.playback.mediaplayer.timelinecontroller) est ensuite définie sur l’objet contrôleur de chronologie.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareMediaTimelineController":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetSetTimelineController":::

**Attention** La classe [**MediaPlaybackCommandManager**](/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManager) procure une intégration automatique entre **MediaPlayer** et les contrôles de transport de média système, mais cette intégration automatique ne peut pas être utilisée avec des lecteurs multimédias contrôlés avec une classe **MediaTimelineController**. Par conséquent, vous devez désactiver le gestionnaire de commande du lecteur multimédia avant de définir son contrôleur de chronologie. À défaut, une exception sera transmise avec un message vous informant du blocage de l’association du contrôleur de chronologie de médias en raison de l’état actuel de l’objet. Pour plus d’informations sur l’intégration des lecteurs multimédias avec les contrôles de transport de média système, consultez la page [Intégrer avec les contrôles de transport de média système](integrate-with-systemmediatransportcontrols.md). Si vous utilisez une classe **MediaTimelineController**, vous pouvez toujours contrôler manuellement les contrôles de transport de média système. Pour plus d’informations, consultez la page [Contrôles de transport de média système](system-media-transport-controls.md).

Une fois que vous avez associé une classe **MediaTimelineController** à un ou plusieurs lecteurs multimédias, vous pouvez contrôler l’état de lecture à l’aide des méthodes exposées par le contrôleur. L’exemple suivant appelle [**Start**](/uwp/api/windows.media.mediatimelinecontroller.start) pour commencer à lire tous les lecteurs multimédias associés au début du support.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetPlayButtonClick":::

Cet exemple illustre l’interruption et la reprise de la lecture sur l’ensemble des lecteurs multimédias associés.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetPauseButtonClick":::

Pour définir l’avance rapide sur l’ensemble des lecteurs multimédias connectés, définissez la vitesse de lecture sur une valeur supérieure à 1.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetFastForwardButtonClick":::

L’exemple suivant illustre l’utilisation d’un contrôle **Slider** afin d’afficher la position de lecture actuelle du contrôleur de chronologie par rapport à la durée du contenu sur l’un des lecteurs multimédias associés. Tout d’abord, un nouvel élément **MediaSource** est créé et un gestionnaire est enregistré pour l’événement [**OpenOperationCompleted**](/uwp/api/windows.media.core.mediasource.openoperationcompleted) de la source multimédia. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetCreateSourceWithOpenCompleted":::

Le gestionnaire **OpenOperationCompleted** est utilisé en tant qu’opportunité de découverte de la durée du contenu de la source multimédia. Une fois que la durée est déterminée, la valeur maximale du contrôle **Slider** est définie sur le nombre total de secondes de l’élément multimédia. La valeur est définie au sein d’un appel à [**RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync), afin de garantir l’exécution sur le thread d’interface utilisateur.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareDuration":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetOpenCompleted":::

Ensuite, un gestionnaire pour l’événement  [**PositionChanged**](/uwp/api/windows.media.mediatimelinecontroller.positionchanged) du contrôleur de chronologie est enregistré. L’appel est effectué régulièrement par le système, environ 4 fois par seconde.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetRegisterPositionChanged":::

Dans le gestionnaire de **PositionChanged**, la valeur Slider est mise à jour en fonction de la position actuelle du contrôleur de chronologie.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetPositionChanged":::

### <a name="offset-the-playback-position-from-the-timeline-position"></a>Décaler la position de lecture de la position de la chronologie
Dans certains cas, il est possible que vous vouliez décaler la position de lecture d’un ou de plusieurs lecteurs multimédias des autres lecteurs. Pour ce faire, définissez la propriété [**TimelineControllerPositionOffset**](/uwp/api/windows.media.playback.mediaplayer.timelinecontrollerpositionoffset) de l’objet **MediaPlayer** que vous voulez décaler. L’exemple suivant utilise les durées du contenu de deux lecteurs multimédias pour définir les valeurs minimum et maximum du contrôle à deux curseurs sur « plus » ou « moins » la longueur de l’élément.  

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetOffsetSliders":::

Dans l’événement [**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) associé à chaque curseur, la propriété **TimelineControllerPositionOffset** de chaque lecteur est définie sur la valeur correspondante.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetTimelineOffset":::

Notez que si la valeur de décalage d’un lecteur correspond à une position de lecture négative, le contenu demeure interrompu jusqu’à ce que le décalage atteigne la valeur de 0, puis la lecture redémarre. De la même manière, si la valeur de décalage correspond à une position de lecture supérieure à la durée de l’élément multimédia, l’image finale s’affiche, comme cela se produit à la fin de la lecture du contenu d’un lecteur multimédia.

## <a name="play-spherical-video-with-mediaplayer"></a>Lire une vidéo sphérique avec MediaPlayer
À compter de Windows 10, la version 1703, **MediaPlayer** prend en charge la projection équirectangulaire pour la lecture de vidéos sphériques. Le contenu vidéo sphérique n’est pas différent de la vidéo ordinaire et plate dans la mesure où le **MediaPlayer** affiche la vidéo tant que l’encodage vidéo est pris en charge. Pour une vidéo sphérique qui contient une balise de métadonnées qui spécifie que la vidéo utilise la projection équirectangulaire, **MediaPlayer** peut afficher la vidéo à l’aide d’une orientation de champ et d’affichage spécifiée. Cela permet des scénarios tels que la lecture vidéo de la réalité virtuelle avec un affichage monté en tête ou simplement la possibilité pour l’utilisateur de se déplacer dans le contenu vidéo sphérique à l’aide de la souris ou du clavier.

Pour lire une vidéo sphérique, suivez les étapes de lecture du contenu vidéo décrit précédemment dans cet article. La première étape consiste à inscrire un gestionnaire pour l’événement [**MediaPlayer. MediaOpened**](/uwp/api/Windows.Media.Playback.MediaPlayer#Windows_Media_Playback_MediaPlayer_MediaOpened) . Cet événement vous donne la possibilité d’activer et de contrôler les paramètres de lecture de la vidéo sphérique.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetOpenSphericalVideo":::

Dans le gestionnaire **MediaOpened** , vérifiez tout d’abord le format de cadre de l’élément multimédia qui vient d’être ouvert en vérifiant la propriété [**PlaybackSession. SphericalVideoProjection. FrameFormat**](/uwp/api/windows.media.playback.mediaplaybacksphericalvideoprojection.FrameFormat) . Si cette valeur est [**SphericaVideoFrameFormat. équirectangulaire**](/uwp/api/windows.media.mediaproperties.sphericalvideoframeformat), le système peut automatiquement projeter le contenu vidéo. Tout d’abord, définissez la propriété [**PlaybackSession. SphericalVideoProjection. IsEnabled**](/uwp/api/windows.media.playback.mediaplaybacksphericalvideoprojection.IsEnabled) sur **true**. Vous pouvez également ajuster des propriétés telles que l’orientation de l’affichage et le champ de vue que le lecteur multimédia utilisera pour projeter le contenu vidéo. Dans cet exemple, le champ de la vue est défini sur une valeur étendue de 120 degrés en définissant la propriété [**HorizontalFieldOfViewInDegrees**](/uwp/api/windows.media.playback.mediaplaybacksphericalvideoprojection.HorizontalFieldOfViewInDegrees) .

Si le contenu de la vidéo est sphérique, mais est dans un format autre que équirectangulaire, vous pouvez implémenter votre propre algorithme de projection à l’aide du mode serveur de frames du lecteur multimédia pour recevoir et traiter des frames individuels.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetSphericalMediaOpened":::

L’exemple de code suivant montre comment ajuster l’orientation de l’affichage vidéo sphérique à l’aide des touches de direction gauche et droite.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetSphericalOnKeyDown":::

Si votre application prend en charge les sélections de vidéos, vous souhaiterez peut-être identifier les éléments de lecture qui contiennent une vidéo sphérique dans votre interface utilisateur. Les sélections multimédias sont décrites en détail dans l’article, les [éléments multimédias, les sélections et les pistes](media-playback-with-mediasource.md). L’exemple suivant illustre la création d’une nouvelle sélection, l’ajout d’un élément et l’inscription d’un gestionnaire pour l’événement [**MediaPlaybackItem. VideoTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.VideoTracksChanged) , qui se produit lorsque les pistes vidéo d’un élément multimédia sont résolues.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetSphericalList":::

Dans le gestionnaire d’événements **VideoTracksChanged** , récupérez les propriétés d’encodage des pistes vidéo ajoutées en appelant [**VideoTrack. GetEncodingProperties**](/uwp/api/windows.media.core.videotrack.GetEncodingProperties). Si la valeur de la propriété [**SphericalVideoFrameFormat**](/uwp/api/windows.media.mediaproperties.videoencodingproperties.SphericalVideoFrameFormat) des propriétés d’encodage est différente de [**SphericaVideoFrameFormat. None**](/uwp/api/windows.media.mediaproperties.sphericalvideoframeformat), la piste vidéo contient une vidéo sphérique et vous pouvez mettre à jour votre interface utilisateur en conséquence si vous le souhaitez.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetSphericalTracksChanged":::

## <a name="use-mediaplayer-in-frame-server-mode"></a>Utiliser MediaPlayer en mode de serveur de frames
À compter de Windows 10, version 1703, vous pouvez utiliser **MediaPlayer** en mode de serveur de trame. Dans ce mode, le **MediaPlayer** ne rend pas automatiquement les frames à un **MediaPlayerElement**associé. Au lieu de cela, votre application copie le frame actuel du **MediaPlayer** vers un objet qui implémente [**IDirect3DSurface**](/uwp/api/windows.graphics.directx.direct3d11.idirect3dsurface). Le scénario principal que cette fonctionnalité permet d’utiliser utilise des nuanceurs de pixels pour traiter les trames vidéo fournies par le **MediaPlayer**. Votre application est chargée d’afficher chaque frame après son traitement, par exemple en affichant le frame dans un contrôle d' [**image**](/uwp/api/windows.ui.xaml.controls.image) XAML.

Dans l’exemple suivant, un nouveau **MediaPlayer** est initialisé et le contenu vidéo est chargé. Ensuite, un gestionnaire pour [**VideoFrameAvailable**](/uwp/api/windows.media.playback.mediaplayer.VideoFrameAvailable) est inscrit. Le mode de serveur de frames est activé en affectant à la propriété [**IsVideoFrameServerEnabled**](/uwp/api/windows.media.playback.mediaplayer.IsVideoFrameServerEnabled) de l’objet **MediaPlayer** la **valeur true**. Enfin, la lecture du média démarre avec un appel à [**Play**](/uwp/api/windows.media.playback.mediaplayer.Play).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetFrameServerInit":::

L’exemple suivant montre un gestionnaire pour **VideoFrameAvailable** qui utilise [Win2D](https://github.com/Microsoft/Win2D) pour ajouter un effet de flou simple à chaque image d’une vidéo, puis affiche les frames traités dans un contrôle [image](/uwp/api/windows.ui.xaml.controls.image) XAML.

Chaque fois que le gestionnaire **VideoFrameAvailable** est appelé, la méthode [**CopyFrameToVideoSurface**](/uwp/api/windows.media.playback.mediaplayer.copyframetovideosurface) est utilisée pour copier le contenu du frame dans un [**IDirect3DSurface**](/uwp/api/windows.graphics.directx.direct3d11.idirect3dsurface). Vous pouvez également utiliser [**CopyFrameToStereoscopicVideoSurfaces**](/uwp/api/windows.media.playback.mediaplayer.copyframetostereoscopicvideosurfaces) pour copier le contenu 3D dans deux surfaces, pour traiter le contenu de l’œil gauche et du bon œil séparément. Pour obtenir un objet qui implémente **IDirect3DSurface**  , cet exemple crée un [**SoftwareBitmap**](/uwp/api/windows.graphics.imaging.softwarebitmap) , puis utilise cet objet pour créer un **CanvasBitmap**Win2D, qui implémente l’interface nécessaire. Un **CanvasImageSource** est un objet Win2D qui peut être utilisé comme source pour un contrôle **image** . un nouveau est créé et défini en tant que source de l' **image** dans laquelle le contenu doit être affiché. Ensuite, un **CanvasDrawingSession** est créé. Utilisé par Win2D pour restituer l’effet de flou.

Une fois que tous les objets nécessaires ont été instanciés, **CopyFrameToVideoSurface** est appelé, ce qui copie le frame actuel à partir du **MediaPlayer** dans le **CanvasBitmap**. Ensuite, un **GaussianBlurEffect** Win2D est créé, avec le **CanvasBitmap** défini comme source de l’opération. Enfin, **CanvasDrawingSession. DrawImage** est appelé pour dessiner l’image source, avec l’effet de flou appliqué, dans le **CanvasImageSource** qui a été associé au contrôle **image** , provoquant son dessin dans l’interface utilisateur.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetVideoFrameAvailable":::

Pour plus d’informations sur Win2D, consultez le [référentiel GitHub Win2D](https://github.com/Microsoft/Win2D). Pour tester l’exemple de code indiqué ci-dessus, vous devez ajouter le package NuGet Win2D à votre projet en suivant les instructions ci-dessous.

**Pour ajouter le package NuGet Win2D à votre projet d’effet**

1.  Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Gérer les packages NuGet**.
2.  En haut de la fenêtre, sélectionnez l’onglet **Explorer**.
3.  Dans la zone de recherche, entrez **Win2D**.
4.  Sélectionnez **Win2D.uwp**, puis **Installer** dans le volet droit.
5.  La boîte de dialogue **Examiner les modifications** vous indique le package à installer. Cliquez sur **OK**.
6.  Acceptez la licence de package.

## <a name="detect-and-respond-to-audio-level-changes-by-the-system"></a>Détecter les modifications de niveau audio et y répondre par le système
À compter de Windows 10, version 1803, votre application peut détecter si le système diminue ou diminue le niveau audio d’un **MediaPlayer**en cours de lecture. Par exemple, le système peut réduire, ou « canard », le niveau de lecture audio lors de la sonnerie d’une alarme. Le système désactive votre application lorsqu’elle passe en arrière-plan si votre application n’a pas déclaré la fonctionnalité *backgroundMediaPlayback* dans le manifeste de l’application. La classe [**AudioStateMonitor**](./uwp/api/windows.media.audio.audiostatemonitor) vous permet de vous inscrire pour recevoir un événement lorsque le système modifie le volume d’un flux audio. Accédez à la propriété **AudioStateMonitor** d’un **MediaPlayer** et enregistrez un gestionnaire pour l’événement [**SoundLevelChanged**](/uwp/api/windows.media.audio.audiostatemonitor.soundlevelchanged) à notifier lorsque le niveau audio de ce **MediaPlayer** est modifié par le système.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetRegisterAudioStateMonitor":::

Lors du traitement de l’événement **SoundLevelChanged** , vous pouvez effectuer différentes actions en fonction du type de contenu en cours de lecture. Si vous jouez actuellement de la musique, vous souhaiterez peut-être laisser la musique continuer à jouer pendant que le volume est présent. Toutefois, si vous jouez à un podcast, vous souhaiterez probablement suspendre la lecture pendant que l’audio est mis en place, de sorte que l’utilisateur n’a pas de contenu.

Cet exemple déclare une variable pour suivre si le contenu en cours de création est un podcast, il est supposé que vous définissez cette valeur sur la valeur appropriée lors de la sélection du contenu pour le **MediaPlayer**. Nous créons également une variable de classe pour effectuer le suivi lorsque la lecture est suspendue par programmation lorsque le niveau audio change.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetAudioStateVars":::

Dans le gestionnaire d’événements **SoundLevelChanged** , vérifiez la propriété [**SoundLevel**](/uwp/api/windows.media.audio.audiostatemonitor.soundlevel) de l’expéditeur **AudioStateMonitor** pour déterminer le nouveau niveau sonore. Cet exemple vérifie si le nouveau niveau de son est Full Volume, ce qui signifie que le système a cessé de fonctionner en sourdine ou qu’il est en train de le faire, ou si le niveau sonore a été abaissé mais qu’il s’agit d’un contenu qui n’est pas un podcast. Si l’une ou l’autre est vraie et que le contenu a été mis en pause par programme, la lecture est reprise. Si le nouveau niveau audio est muet ou si le contenu actuel est un podcast et que le niveau sonore est faible, la lecture est suspendue et la variable est définie pour assurer le suivi de l’initialisation de la pause par programmation.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetSoundLevelChanged":::

L’utilisateur peut décider qu’il souhaite suspendre ou continuer la lecture, même si l’audio est mis en place par le système. Cet exemple montre les gestionnaires d’événements pour un bouton de lecture et d’interruption. Dans le bouton suspendre, le gestionnaire de clic est suspendu, si la lecture avait déjà été suspendue par programme, nous mettons à jour la variable pour indiquer que l’utilisateur a suspendu le contenu. Dans le gestionnaire de clic de clic sur le bouton de lecture, nous reprenons la lecture et effacons notre variable de suivi.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetButtonUserClick":::

## <a name="related-topics"></a>Rubriques connexes
* [Lecture de contenu multimédia](media-playback.md)
* [Éléments, playlists et pistes multimédias](media-playback-with-mediasource.md)
* [Intégrer avec les contrôles de transport de média système](integrate-with-systemmediatransportcontrols.md)
* [Créer, planifier et gérer des coupures de médias](create-schedule-and-manage-media-breaks.md)
* [Lire du contenu multimédia en arrière-plan](background-audio.md)





 

 
