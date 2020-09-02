---
ms.assetid: C5623861-6280-4352-8F22-80EB009D662C
description: Cet article vous montre comment utiliser MediaSource, qui offre une méthode courante de référencement et de lecture de contenu multimédia à partir de différentes sources telles que des fichiers locaux ou distants, et présente un modèle commun d’accès aux données multimédias, quel que soit le format multimédia sous-jacent.
title: Éléments, playlists et pistes multimédias
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f1f428bb8beb4bb933387a77e5a74819016a4c64
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363882"
---
# <a name="media-items-playlists-and-tracks"></a>Éléments, playlists et pistes multimédias


 Cet article explique comment utiliser la classe [**MediaSource**](/uwp/api/Windows.Media.Core.MediaSource) , qui permet de référencer et de lire des médias de différentes sources, tels que des fichiers locaux ou distants, et d’exposer un modèle commun pour l’accès aux données multimédias, quel que soit le format de média sous-jacent. La classe [**MediaPlaybackItem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem) étend les fonctionnalités de **MediaSource**, vous permettant ainsi de gérer et de sélectionner à partir de plusieurs pistes audio, vidéo et de métadonnées contenues dans un élément multimédia. [**MediaPlaybackList**](/uwp/api/Windows.Media.Playback.MediaPlaybackList) vous permet de créer des listes de lecture à partir d’un ou de plusieurs éléments de lecture multimédia.


## <a name="create-and-play-a-mediasource"></a>Créer et lire un MediaSource

Créez une instance de **MediaSource** en appelant l’une des méthodes de fabrique exposées par la classe :

-   [**CreateFromAdaptiveMediaSource**](/uwp/api/windows.media.core.mediasource.createfromadaptivemediasource)
-   [**CreateFromIMediaSource**](/uwp/api/windows.media.core.mediasource.createfromimediasource)
-   [**CreateFromMediaStreamSource**](/uwp/api/windows.media.core.mediasource.createfrommediastreamsource)
-   [**CreateFromMseStreamSource**](/uwp/api/windows.media.core.mediasource.createfrommsestreamsource)
-   [**CreateFromStorageFile**](/uwp/api/windows.media.core.mediasource.createfromstoragefile)
-   [**CreateFromStream**](/uwp/api/windows.media.core.mediasource.createfromstream)
-   [**CreateFromStreamReference**](/uwp/api/windows.media.core.mediasource.createfromstreamreference)
-   [**CreateFromUri**](/uwp/api/windows.media.core.mediasource.createfromuri)
-   [**CreateFromDownloadOperation**](/uwp/api/windows.media.core.mediasource.createfromdownloadoperation)

Après avoir créé un **MediaSource**, vous pouvez le lire avec un [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) en définissant la propriété [**Source**](/uwp/api/windows.media.playback.mediaplayer.source). À compter de Windows 10, version 1607, vous pouvez attribuer un **MediaPlayer** à un [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) en appelant [**SetMediaPlayer**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.setmediaplayer) pour afficher le contenu du lecteur multimédia dans une page XAML. Cette méthode est préférée à l’utilisation de **MediaElement**. Pour plus d’informations sur l’utilisation de **MediaPlayer**, voir [**Lire du contenu audio et vidéo avec MediaPlayer**](play-audio-and-video-with-mediaplayer.md).

L’exemple suivant montre comment lire un fichier multimédia sélectionné par l’utilisateur dans un **MediaPlayer** à l’aide de **MediaSource**.

Pour effectuer ce scénario, vous devez inclure les espaces de noms [**Windows. Media. Core**](/uwp/api/Windows.Media.Core) et [**Windows. Media. playback**](/uwp/api/Windows.Media.Playback) .

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetUsing":::

Déclarez une variable de type **MediaSource**. Pour les exemples de cet article, la source multimédia est déclarée en tant que membre de classe. Elle est donc accessible à partir de plusieurs emplacements.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareMediaSource":::

Déclarez une variable pour stocker l’objet **MediaPlayer** et, si vous voulez afficher le contenu multimédia en XAML, ajoutez un contrôle **MediaPlayerElement** à votre page.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareMediaPlayer":::

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml" id="SnippetMediaPlayerElement":::

Pour permettre à l’utilisateur de sélectionner un fichier multimédia à lire, utilisez un [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker). Avec l’objet [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) retourné par la méthode [**PickSingleFileAsync**](/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync) du sélecteur, initialisez un nouveau MediaObject en appelant [**MediaSource. CreateFromStorageFile**](/uwp/api/windows.media.core.mediasource.createfromstoragefile). Enfin, définissez la source du média en tant que source de lecture de **MediaElement** en appelant la méthode [**SetPlaybackSource**](/uwp/api/windows.ui.xaml.controls.mediaelement.setplaybacksource).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetPlayMediaSource":::

Par défaut, le **MediaPlayer** ne commence pas automatiquement la lecture quand la source du média est définie. Vous pouvez commencer la lecture manuellement en appelant [**Play**](/uwp/api/windows.media.playback.mediaplayer.play).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetPlay":::

Vous pouvez également définir la propriété [**AutoPlay**](/uwp/api/windows.media.playback.mediaplayer.autoplay) de **MediaPlayer** sur true pour indiquer au lecteur de commencer la lecture dès que la source du média est définie.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetAutoPlay":::

### <a name="create-a-mediasource-from-a-downloadoperation"></a>Créer un MediaSource à partir d’un DownloadOperation
À compter de Windows, version 1803, vous pouvez créer un objet **MediaSource** à partir d’un **DownloadOperation**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetCreateMediaSourceFromDownload":::

Notez que, bien que vous puissiez créer un **MediaSource** à partir d’un téléchargement sans le démarrer ou définir sa propriété **IsRandomAccessRequired** sur true, vous devez effectuer ces deux opérations avant de tenter d’attacher le **MediaSource** à un **MediaPlayer** ou à un **MediaPlayerElement** pour la lecture.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetStartDownload":::


## <a name="handle-multiple-audio-video-and-metadata-tracks-with-mediaplaybackitem"></a>Gérer plusieurs pistes audio, vidéo et de métadonnées avec MediaPlaybackItem

L’utilisation d’un objet [**MediaSource**](/uwp/api/Windows.Media.Core.MediaSource) pour la lecture est pratique car il offre une méthode courante de lecture multimédia à partir de différents types de sources. Un comportement plus avancé est toutefois disponible en créant un [**MediaPlaybackItem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem) à partir de **MediaSource**. Il inclut la possibilité d’accéder et de gérer plusieurs pistes audio, vidéo et de données pour un élément multimédia.

Déclarez une variable pour stocker votre **MediaPlaybackItem**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareMediaPlaybackItem":::

Créez un **MediaPlaybackItem** en appelant le constructeur et en le transmettant à un objet **MediaSource** initialisé.

Si votre application prend en charge plusieurs pistes audio, vidéo ou de données dans un élément de lecture multimédia, inscrivez les gestionnaires d’événements pour les événements [**AudioTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.audiotrackschanged), [**VideoTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.videotrackschanged)ou [**TimedMetadataTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.timedmetadatatrackschanged) .

Enfin, définissez la source de la lecture de **MediaElement** ou **MediaPlayer** sur votre **MediaPlaybackItem**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetPlayMediaPlaybackItem":::

> [!NOTE] 
> Un **MediaSource** ne peut être associé qu’à un seul **MediaPlaybackItem**. Après avoir créé un **MediaPlaybackItem** à partir d’une source, toute tentative de création d’un autre élément de lecture à partir de la même source entraîne une erreur. De plus, après avoir créé un **MediaPlaybackItem** à partir d’une source de média, vous ne pouvez pas définir l’objet **MediaSource** directement en tant que source d’un **MediaPlayer**, mais vous devez plutôt utiliser le **MediaPlaybackItem**.

L’événement [**VideoTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.videotrackschanged) est déclenché après qu’un **MediaPlaybackItem** contenant plusieurs pistes vidéo a été attribué en tant que source de lecture, et peut être redéclenché si la liste des pistes vidéo change pour l’élément. Le gestionnaire de cet événement vous permet de mettre à jour votre interface utilisateur, permettant ainsi à l’utilisateur de basculer entre les pistes disponibles. Cet exemple utilise une [**zone de liste déroulante**](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) pour afficher les pistes vidéo disponibles.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml" id="SnippetVideoComboBox":::

Dans le gestionnaire **VideoTracksChanged**, parcourez toutes les pistes de la liste [**VideoTracks**](/uwp/api/windows.media.playback.mediaplaybackitem.videotracks) de l’élément de lecture. Un nouveau [**ComboBoxItem**](/uwp/api/Windows.UI.Xaml.Controls.ComboBoxItem) est créé pour chaque piste. Si la piste ne porte déjà une étiquette, une étiquette est générée à partir de l’index de piste. La propriété [**Tag**](/uwp/api/windows.ui.xaml.frameworkelement.tag) de l’élément de zone de liste déroulante est définie sur l’index de piste pour pouvoir l’identifier ultérieurement. Enfin, l’élément est ajouté à la zone de liste déroulante. Notez que ces opérations sont effectuées au sein d’un appel [**CoreDispatcher. RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) , car toutes les modifications de l’interface utilisateur doivent être effectuées sur le thread d’interface utilisateur et cet événement est déclenché sur un thread différent.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetVideoTracksChanged":::

Dans le gestionnaire [**SelectionChanged**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) de la zone de liste déroulante, l’index de piste est récupéré à partir de la propriété **Tag** de l’élément sélectionné. La définition de la propriété [**SelectedIndex**](/uwp/api/windows.media.playback.mediaplaybackvideotracklist.selectedindex) de la liste [**VideoTracks**](/uwp/api/windows.media.playback.mediaplaybackitem.videotracks) de l’élément de lecture multimédia amène **MediaElement** ou **MediaPlayer** à basculer la piste vidéo active sur l’index spécifié.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetVideoTracksSelectionChanged":::

La gestion des éléments multimédias avec des pistes audio multiples est strictement identique à celle des pistes vidéo. Gérez le [**AudioTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.audiotrackschanged) pour mettre à jour votre interface utilisateur avec les pistes audio trouvées dans la liste [**AudioTracks**](/uwp/api/windows.media.playback.mediaplaybackitem.audiotracks) de l’élément de lecture. Lorsque l’utilisateur sélectionne une piste audio, définissez la propriété [**SelectedIndex**](/uwp/api/windows.media.playback.mediaplaybackaudiotracklist.selectedindex) de la liste **AudioTracks** pour que **MediaElement** ou **MediaPlayer** bascule la piste audio active sur l’index spécifié.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml" id="SnippetAudioComboBox":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetAudioTracksChanged":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetAudioTracksSelectionChanged":::

En plus de l’audio et de la vidéo, un objet **MediaPlaybackItem** peut contenir zéro ou plusieurs objets [**TimedMetadataTrack**](/uwp/api/Windows.Media.Core.TimedMetadataTrack). Une piste de métadonnées synchronisée peut contenir du texte de sous-titre. Elle peut également contenir des données personnalisées propriétaires de votre application. Une piste de métadonnées minutée contient une liste de signaux représentés par des objets qui héritent de [**IMediaCue**](/uwp/api/Windows.Media.Core.IMediaCue), tels qu’un [**DataCue**](/uwp/api/Windows.Media.Core.DataCue) ou un [**TimedTextCue**](/uwp/api/Windows.Media.Core.TimedTextCue). Chaque indicateur a une heure de début et une durée qui déterminent quand l’indicateur est activé et pour combien de temps.

Comme les pistes audio et vidéo, les pistes de métadonnées synchronisées d’un élément multimédia peuvent être détectées en gérant l’événement [**TimedMetadataTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.timedmetadatatrackschanged) d’un **MediaPlaybackItem**. Toutefois, avec les pistes de métadonnées synchronisées, l’utilisateur souhaitera peut-être activer plusieurs pistes de métadonnées simultanément. De plus, en fonction de votre scénario d’application, vous souhaiterez peut-être activer ou désactiver automatiquement des pistes de métadonnées, sans l’intervention de l’utilisateur. À des fins d’illustration, cet exemple ajoute un [**ToggleButton**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton) pour chaque piste de métadonnées dans un élément multimédia pour permettre à l’utilisateur d’activer et de désactiver la piste. La propriété **tag** de chaque bouton est définie sur l’index de la piste des métadonnées associée afin qu’il puisse être identifié lorsque le bouton est activé.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml" id="SnippetMetaStackPanel":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetTimedMetadataTrackschanged":::

Étant donné que plusieurs pistes de métadonnées peuvent être actives simultanément, vous ne définissez pas simplement l’index actif de la liste de pistes de métadonnées. Appelez plutôt le **MediaPlaybackItem** de la méthode [**SetPresentationMode**](/previous-versions/windows/dn986977(v=win.10)) de l’objet, en indiquant l’index de la piste que vous souhaitez activer/désactiver, puis en spécifiant une valeur à partir de l’énumération [**TimedMetadataTrackPresentationMode**](/uwp/api/Windows.Media.Playback.TimedMetadataTrackPresentationMode). Le mode de présentation que vous choisissez dépend de l’implémentation de votre application. Dans cet exemple, la piste de métadonnées est définie sur **PlatformPresented** lorsqu’elle est activée. Pour les pistes textuelles, cela signifie que le système affichera automatiquement les indications de texte dans la piste. Quand le bouton bascule est désactivé, le mode de présentation est défini sur **désactivé**, ce qui signifie qu’aucun texte n’est affiché et qu’aucun événement de signal n’est déclenché. Les événements d’indicateur sont présentés plus loin dans cet article.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetToggleChecked":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetToggleUnchecked":::

Lorsque vous traitez les pistes de métadonnées, vous pouvez accéder à l’ensemble de signaux au sein de la piste en accédant aux propriétés des [**indications**](/uwp/api/windows.media.core.timedmetadatatrack.cues) ou [**ActiveCues**](/uwp/api/windows.media.core.timedmetadatatrack.activecues) . Pour ce faire, mettez à jour votre interface utilisateur pour afficher les emplacements de repère d’un élément multimédia.

## <a name="handle-unsupported-codecs-and-unknown-errors-when-opening-media-items"></a>Gérer les codecs non pris en charge et les erreurs inconnues à l’ouverture des éléments multimédias
À compter de Windows 10, version 1607, vous pouvez vérifier si le codec nécessaire pour lire un élément multimédia est pris en charge entièrement ou partiellement sur l’appareil sur lequel s’exécute votre application. Dans le gestionnaire d’événements pour les événements **MediaPlaybackItem** Tracks-Changed, tels que [**AudioTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.audiotrackschanged), commencez par vérifier si la modification de piste est l’insertion d’une nouvelle piste. Dans ce cas, vous pouvez obtenir une référence à la piste en cours d’insertion à l’aide de l’index passé dans le paramètre **IVectorChangedEventArgs. index** avec la collection de pistes appropriée du paramètre **MediaPlaybackItem** , telle que la collection [**AudioTracks**](/uwp/api/windows.media.playback.mediaplaybackitem.audiotracks) .

Une fois que vous avez une référence à la piste insérée, vérifiez le [**DecoderStatus**](/uwp/api/windows.media.core.audiotracksupportinfo.decoderstatus) de la propriété [**SupportInfo**](/uwp/api/windows.media.core.audiotrack.supportinfo) de la piste. Si la valeur est [**FullySupported**](/uwp/api/Windows.Media.Core.MediaDecoderStatus), le codec approprié nécessaire pour lire la piste est présent sur l’appareil. Si la valeur est [**Degraded**](/uwp/api/Windows.Media.Core.MediaDecoderStatus), la piste peut être lue par le système, mais la lecture est détériorée d’une certaine façon. Par exemple, une piste audio 5.1 peut être lue à la place comme une piste stéréo bicanale. Si c’est le cas, vous pouvez mettre à jour votre interface utilisateur pour alerter l’utilisateur de la détérioration. Si la valeur est [**UnsupportedSubtype**](/uwp/api/Windows.Media.Core.MediaDecoderStatus) ou [**UnsupportedEncoderProperties**](/uwp/api/Windows.Media.Core.MediaDecoderStatus), la piste ne peut pas être lue avec les codecs actuels sur l’appareil. Vous pouvez alerter l’utilisateur et ignorez la lecture de l’élément ou implémenter une interface utilisateur pour permettre à l’utilisateur de télécharger le codec correct. La méthode [**GetEncodingProperties**](/uwp/api/windows.media.core.audiotrack.getencodingproperties) de la piste peut être utilisée pour déterminer le codec requis pour la lecture.

Enfin, vous pouvez vous inscrire à l’événement [**OpenFailed**](/uwp/api/windows.media.core.audiotrack.openfailed) de la piste, qui sera déclenché si la piste est prise en charge sur l’appareil mais n’a pas pu s’ouvrir en raison d’une erreur inconnue dans le pipeline.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetAudioTracksChanged_CodecCheck":::

Dans le gestionnaire d’événements [**OpenFailed**](/uwp/api/windows.media.core.audiotrack.openfailed), vous pouvez vérifier si l’état de **MediaSource** est inconnu et si tel est le cas, vous pouvez sélectionner par programmation une autre piste à lire, autoriser l’utilisateur à choisir une autre piste ou abandonner la lecture.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetOpenFailed":::

## <a name="set-display-properties-used-by-the-system-media-transport-controls"></a>Définir les propriétés d’affichage utilisées par les contrôles de transport de média système
À compter de Windows 10, version 1607, le support lu dans un [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) est automatiquement intégré dans les contrôles de transport des médias système (SMTC) par défaut. Vous pouvez spécifier les métadonnées que les contrôles de transport de média système doivent afficher en mettant à jour les propriétés d’affichage d’un **MediaPlaybackItem**. Obtient un objet représentant les propriétés d’affichage d’un élément en appelant [**GetDisplayProperties**](/uwp/api/windows.media.playback.mediaplaybackitem.getdisplayproperties). Déterminez si l’élément de lecture est de la musique ou une vidéo en définissant la propriété [**Type**](/uwp/api/windows.media.playback.mediaitemdisplayproperties.type). Ensuite, définissez les propriétés [**VideoProperties**](/uwp/api/windows.media.playback.mediaitemdisplayproperties.videoproperties) ou [**MusicProperties**](/uwp/api/windows.media.playback.mediaitemdisplayproperties.musicproperties) de l’objet. Appelez [**ApplyDisplayProperties**](/uwp/api/windows.media.playback.mediaplaybackitem.applydisplayproperties) pour mettre à jour les propriétés de l’élément sur les valeurs que vous avez indiquées. En règle générale, une application récupère les valeurs d’affichage de manière dynamique à partir d’un service web, mais l’exemple suivant illustre ce processus avec des valeurs codées en dur.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetSetVideoProperties":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetSetMusicProperties":::

## <a name="add-external-timed-text-with-timedtextsource"></a>Ajouter du texte synchronisé externe avec TimedTextSource

Dans certains scénarios, vous pouvoir disposer de fichiers externes contenant du texte synchronisé associé à un élément multimédia, des fichiers distincts contenant des sous-titres pour différents paramètres régionaux par exemple. Utilisez la classe [**TimedTextSource**](/uwp/api/Windows.Media.Core.TimedTextSource) pour charger les fichiers de texte synchronisé externe à partir d’un flux ou d’une URI.

Cet exemple utilise une collection **Dictionary** pour stocker une liste de sources de texte synchronisé pour l’élément multimédia à l’aide de l’URI source et de l’objet **TimedTextSource** en tant que paire clé/valeur afin d’identifier les pistes une fois qu’elles ont été résolues.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetTimedTextSourceMap":::

Créez un **TimedTextSource** pour chaque fichier de texte synchronisé externe en appelant la méthode [**CreateFromUri**](/uwp/api/windows.media.core.mediasource.createfromuri). Ajoutez une entrée pour le **Dictionary** de la source de texte synchronisé. Ajoutez un gestionnaire pour l’événement [**TimedTextSource.Resolved**](/uwp/api/windows.media.core.timedtextsource.resolved) à gérer si l’élément n’a pas pu être chargé ou pour définir des propriétés supplémentaires lorsque l’élément a bien été chargé.

Inscrivez tous vos objets **TimedTextSource** avec **MediaSource** en les ajoutant à la collection [**ExternalTimedTextSources**](/uwp/api/windows.media.core.mediasource.externaltimedtextsources). Notez que les sources de texte synchronisé externe sont ajoutées directement à **MediaSource** et non au **MediaPlaybackItem** créé à partir de la source. Pour mettre à jour votre interface utilisateur afin de refléter les pistes de texte externe, inscrivez et gérez l’événement **TimedMetadataTracksChanged** comme décrit précédemment dans cet article.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetTimedTextSource":::

Dans le gestionnaire de l’événement [**TimedTextSource.Resolved**](/uwp/api/windows.media.core.timedtextsource.resolved), vérifiez la propriété **Error** de [**TimedTextSourceResolveResultEventArgs**](/uwp/api/Windows.Media.Core.TimedTextSourceResolveResultEventArgs) transmise au gestionnaire pour déterminer si une erreur s’est produite lors de la tentative de chargement des données de texte synchronisé. Si l’élément a été résolu avec succès, vous pouvez utiliser ce gestionnaire pour mettre à jour les propriétés supplémentaires de la piste résolue. Cet exemple ajoute une étiquette pour chaque piste en fonction de l’URI précédemment stocké dans le **dictionnaire**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetTimedTextSourceResolved":::

## <a name="add-additional-metadata-tracks"></a>Ajouter des pistes de métadonnées supplémentaires

Vous pouvez créer des pistes de métadonnées personnalisées de manière dynamique dans le code et les associer à une source de média. Les pistes que vous créez peuvent contenir du texte de sous-titre. Elles peuvent également contenir vos données d’application propriétaires.

Créez un [**TimedMetadataTrack**](/uwp/api/Windows.Media.Core.TimedMetadataTrack) en appelant le constructeur et en spécifiant un ID, l’identificateur de langue et une valeur de l’énumération [**TimedMetadataKind**](/uwp/api/Windows.Media.Core.TimedMetadataKind). Enregistrez des gestionnaires pour les événements [**CueEntered**](/uwp/api/windows.media.core.timedmetadatatrack.cueentered) et [**CueExited**](/uwp/api/windows.media.core.timedmetadatatrack.cueexited). Ces événements sont déclenchés lorsque l’heure de début d’un indicateur est atteinte et lorsque la durée d’un indicateur s’est écoulée, respectivement.

Créez un objet de signal, approprié pour le type de suivi de métadonnées que vous avez créé, et définissez l’ID, l’heure de début et la durée de la piste. Cet exemple crée une piste de données, donc un ensemble d’objets [**DataCue**](/uwp/api/Windows.Media.Core.DataCue) sont générés et une mémoire tampon contenant des données spécifiques à l’application est fournie pour chaque pile. Pour inscrire la nouvelle piste, ajoutez-la à la collection [**ExternalTimedMetadataTracks**](/uwp/api/windows.media.core.mediasource.externaltimedmetadatatracks) de l’objet **MediaSource**.

À compter de Windows 10, version 1703, la propriété **DataCue. Properties** expose un [**PropertySet**](/uwp/api/windows.foundation.collections.propertyset) que vous pouvez utiliser pour stocker des propriétés personnalisées dans des paires clé/données qui peuvent être récupérées dans les événements **CueEntered** et **CueExited** .  

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetAddDataTrack":::

L’événement **CueEntered** est déclenché lorsque l’heure de début d’un indicateur est atteinte alors que la piste associée dispose d’un mode de présentation **ApplicationPresented**, **Hidden** ou **PlatformPresented.**. Les événements d’indicateur ne sont pas déclenchés pour les pistes de métadonnées lorsque le mode de présentation de la piste est **Disabled**. Cet exemple présente simplement les données personnalisées associées à l’indicateur dans la fenêtre de débogage.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetDataCueEntered":::

Cet exemple ajoute une piste de texte personnalisé en spécifiant **TimedMetadataKind.Caption** lors de la création de la piste et de l’utilisation d’objets [**TimedTextCue**](/uwp/api/Windows.Media.Core.TimedTextCue) pour ajouter des indicateurs à la piste.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetAddTextTrack":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetTextCueEntered":::

## <a name="play-a-list-of-media-items-with-mediaplaybacklist"></a>Lire une liste d’éléments multimédias avec MediaPlaybackList

[**MediaPlaybackList**](/uwp/api/Windows.Media.Playback.MediaPlaybackList) vous permet de créer une playlist d’éléments multimédias, qui sont représentés par des objets **MediaPlaybackItem**.

**Remarque**    Les éléments d’un [**MediaPlaybackList**](/uwp/api/Windows.Media.Playback.MediaPlaybackList) sont rendus à l’aide de la lecture ininterrompue. Le système utilise les métadonnées fournies dans les fichiers codés MP3 ou AAC pour déterminer la compensation de délai ou de remplissage nécessaire pour la lecture sans blanc. Si les fichiers codés MP3 ou AAC ne fournissent pas ces métadonnées, le système détermine alors le délai ou le remplissage de manière heuristique. Pour les formats sans perte, tels que PCM, FLAC ou ALAC, le système n’exécute aucune action, car ces encodeurs n’introduisent ni retard ni remplissage.

Pour commencer, déclarez une variable pour stocker votre **MediaPlaybackList**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareMediaPlaybackList":::

Créez un **MediaPlaybackItem** pour chaque élément multimédia que vous voulez ajouter à votre liste en suivant la procédure décrite précédemment dans cet article. Initialisez votre objet **MediaPlaybackList** et ajoutez-y les éléments de lecture multimédia. Inscrivez un gestionnaire pour l’événement [**CurrentItemChanged**](/uwp/api/windows.media.playback.mediaplaybacklist.currentitemchanged). Cet événement vous permet de mettre à jour votre interface utilisateur afin de refléter l’élément multimédia en cours de lecture. Vous pouvez également vous inscrire à l’événement [ItemOpened](/uwp/api/Windows.Media.Playback.MediaPlaybackList.ItemOpened) , qui est déclenché lorsqu’un élément de la liste est ouvert avec succès, et l’événement [ItemFailed](/uwp/api/Windows.Media.Playback.MediaPlaybackList.ItemFailed) , qui est déclenché lorsqu’un élément de la liste ne peut pas être ouvert.

À compter de Windows 10, version 1703, vous pouvez spécifier le nombre maximal d’objets **MediaPlaybackItem** dans le **MediaPlaybackList** que le système reste ouvert après avoir été lu en définissant la propriété [MaxPlayedItemsToKeepOpen](/uwp/api/Windows.Media.Playback.MediaPlaybackList.MaxPlayedItemsToKeepOpen) . Quand un **MediaPlaybackItem** est maintenu ouvert, la lecture de l’élément peut démarrer instantanément lorsque l’utilisateur bascule sur cet élément, car l’élément n’a pas besoin d’être rechargé. Toutefois, conserver les éléments ouverts augmente également la consommation de mémoire de votre application. vous devez donc prendre en compte l’équilibre entre la réactivité et l’utilisation de la mémoire lors de la définition de cette valeur. 

Pour activer la lecture de votre liste, définissez la source de lecture du **MediaPlayer** sur votre **MediaPlaybackList**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetPlayMediaPlaybackList":::

Dans le gestionnaire d’événements **CurrentItemChanged**, mettez à jour votre interface utilisateur afin de refléter l’élément en cours de lecture, qui peut être récupéré à l’aide de la propriété [**NewItem**](/uwp/api/windows.media.playback.currentmediaplaybackitemchangedeventargs.newitem) de l’objet [**CurrentMediaPlaybackItemChangedEventArgs**](/uwp/api/Windows.Media.Playback.CurrentMediaPlaybackItemChangedEventArgs) transmis dans l’événement. N’oubliez pas que si vous mettez à jour l’interface utilisateur à partir de cet événement, vous devez le faire dans le cadre d’un appel à [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) afin que les mises à jour soient effectuées sur le thread d’interface utilisateur.

À compter de Windows 10, la version 1703, vous pouvez vérifier la propriété [CurrentMediaPlaybackItemChangedEventArgs. Reason](/uwp/api/windows.media.playback.currentmediaplaybackitemchangedeventargs.Reason) pour obtenir une valeur qui indique la raison pour laquelle l’élément a changé, par exemple les éléments de changement d’application par programmation, l’élément en cours d’exécution qui a atteint sa fin, ou une erreur se produisant.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetMediaPlaybackListItemChanged":::


Appelez [**MovePrevious**](/uwp/api/windows.media.playback.mediaplaybacklist.moveprevious) ou [**MoveNext**](/uwp/api/windows.media.playback.mediaplaybacklist.movenext) pour que le lecteur multimédia lise l’élément précédent ou suivant de votre **MediaPlaybackList**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetPrevButton":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetNextButton":::

Définissez la propriété [**ShuffleEnabled**](/uwp/api/windows.media.playback.mediaplaybacklist.shuffleenabled) pour spécifier si le lecteur multimédia doit lire les éléments de votre liste dans un ordre aléatoire.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetShuffleButton":::

Définissez la propriété [**AutoRepeatEnabled**](/uwp/api/windows.media.playback.mediaplaybacklist.autorepeatenabled) pour spécifier si le lecteur multimédia doit lire votre liste en boucle.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetRepeatButton":::


### <a name="handle-the-failure-of-media-items-in-a-playback-list"></a>Gérer l’échec d’éléments multimédias dans une liste de lecture
L’événement [**ItemFailed**](/uwp/api/windows.media.playback.mediaplaybacklist.itemfailed) est déclenché lorsqu’un élément de la liste ne parvient pas à s’ouvrir. La propriété [**ErrorCode**](/uwp/api/windows.media.playback.mediaplaybackitemerror.errorcode) de l’objet [**MediaPlaybackItemError**](/uwp/api/Windows.Media.Playback.MediaPlaybackItemError) transmis au gestionnaire énumère la cause spécifique de l’échec dans la mesure du possible, y compris les erreurs réseau, les erreurs de décodage ou les erreurs de chiffrement.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetItemFailed":::

### <a name="disable-playback-of-items-in-a-playback-list"></a>Désactiver la lecture des éléments dans une liste de lecture
À compter de Windows 10, version 1703, vous pouvez désactiver la lecture d’un ou plusieurs éléments dans un **MediaPlaybackItemList** en affectant à la propriété [IsDisabledInPlaybackList](/uwp/api/Windows.Media.Playback.MediaPlaybackItem.IsDisabledInPlaybackList) d’un [MediaPlaybackItem](/uwp/api/Windows.Media.Playback.MediaPlaybackItem) la valeur false. 

Un scénario classique pour cette fonctionnalité est pour les applications qui lisent de la musique en continu à partir d’Internet. L’application peut écouter les modifications de l’état de la connexion réseau de l’appareil et désactiver la lecture des éléments qui ne sont pas entièrement téléchargés. Dans l’exemple suivant, un gestionnaire est inscrit pour l’événement [NetworkInformation. NetworkStatusChanged](/uwp/api/Windows.Networking.Connectivity.NetworkInformation.NetworkStatusChanged) .

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetRegisterNetworkStatusChanged":::

Dans le gestionnaire de **NetworkStatusChanged**, vérifiez si [GetInternetConnectionProfile](/uwp/api/Windows.Networking.Connectivity.NetworkInformation.GetInternetConnectionProfile) retourne la valeur null, ce qui indique que le réseau n’est pas connecté. Si c’est le cas, Parcourez tous les éléments de la liste de lecture et, si la [TotalDownloadProgress](/uwp/api/windows.media.playback.mediaplaybackitem.TotalDownloadProgress) de l’élément est inférieure à 1, ce qui signifie que l’élément n’a pas été entièrement téléchargé, désactivez l’élément. Si la connexion réseau est activée, Parcourez tous les éléments de la liste de lecture et activez chaque élément.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetNetworkStatusChanged":::

### <a name="defer-binding-of-media-content-for-items-in-a-playback-list-by-using-mediabinder"></a>Différer la liaison du contenu multimédia pour les éléments d’une liste de lecture à l’aide de MediaBinder
Dans les exemples précédents, une **MediaSource** est créée à partir d’un fichier, d’une URL ou d’un flux, après quoi un **MediaPlaybackItem** est créé et ajouté à un **MediaPlaybackList**. Pour certains scénarios, par exemple, si l’utilisateur est facturé pour afficher du contenu, vous pouvez différer la récupération du contenu d’un **MediaSource** jusqu’à ce que l’élément de la liste de lecture soit prêt à être lu. Pour implémenter ce scénario, créez une instance de la classe [**MediaBinder**](/uwp/api/Windows.Media.Core.MediaBinder) . Affectez à la propriété [**Token**](/uwp/api/Windows.Media.Core.MediaBinder.Token) une chaîne définie par l’application qui identifie le contenu pour lequel vous souhaitez différer la récupération, puis enregistrez un gestionnaire pour l’événement de [**liaison**](/uwp/api/Windows.Media.Core.MediaBinder.Binding) . Créez ensuite un **MediaSource** à partir du **Binder** en appelant [**MediaSource. CreateFromMediaBinder**](/uwp/api/windows.media.core.mediasource.createfrommediabinder). Créez ensuite un **MediaPlaybackItem** à partir du **MediaSource** et ajoutez-le à la liste de lecture comme d’habitude.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetInitMediaBinder":::

Lorsque le système détermine que le contenu associé à l' **MediaBinder** doit être récupéré, il déclenche l’événement de **liaison** . Dans le gestionnaire de cet événement, vous pouvez récupérer l’instance **MediaBinder** à partir du [**MediaBindingEventArgs**](/uwp/api/windows.media.core.mediabindingeventargs) passé dans l’événement. Récupérez la chaîne que vous avez spécifiée pour la propriété de **jeton** et utilisez-la pour déterminer le contenu à récupérer. **MediaBindingEventArgs** fournit des méthodes pour définir le contenu lié dans plusieurs représentations différentes, y compris [**SetStorageFile**](/uwp/api/windows.media.core.mediabindingeventargs.setstoragefile), [**SetStream**](/uwp/api/windows.media.core.mediabindingeventargs.setstream), [**SetStreamReference**](/uwp/api/windows.media.core.mediabindingeventargs.setstreamreference)et [**SetUri**](/uwp/api/windows.media.core.mediabindingeventargs.seturi). 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetBinderBinding":::

Notez que si vous effectuez des opérations asynchrones, telles que des requêtes Web, dans le gestionnaire d’événements de **liaison** , vous devez appeler la méthode [**MediaBindingEventArgs. GetDeferral**](/uwp/api/windows.media.core.mediabindingeventargs.GetDeferral) pour indiquer au système d’attendre que l’opération se termine avant de continuer. Appelez [**Report. Complete**](/uwp/api/windows.foundation.deferral.Complete) une fois l’opération terminée pour demander au système de continuer.

À compter de Windows 10, version 1703, vous pouvez fournir un [**AdaptiveMediaSource**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) comme contenu lié en appelant [**SetAdaptiveMediaSource**](/uwp/api/windows.media.core.mediabindingeventargs.setadaptivemediasource). Pour plus d’informations sur l’utilisation de la diffusion adaptative en continu dans votre application, consultez [streaming adaptative](adaptive-streaming.md).



## <a name="related-topics"></a>Rubriques connexes
* [Lecture de contenu multimédia](media-playback.md)
* [Lire du contenu audio et vidéo avec MediaPlayer](play-audio-and-video-with-mediaplayer.md)
* [Intégrer avec les contrôles de transport de média système](integrate-with-systemmediatransportcontrols.md)
* [Lire du contenu multimédia en arrière-plan](background-audio.md)
