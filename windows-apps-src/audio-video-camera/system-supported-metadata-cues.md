---
ms.assetid: F28162D4-AACC-4EE0-B243-5878F870F87F
description: Découvrez comment tirer parti de plusieurs formats de métadonnées chronométrées qui peuvent être incorporées dans des fichiers multimédias ou des flux.
title: Indicateurs de métadonnées synchronisées pris en charge par le système
ms.date: 04/18/2017
ms.topic: article
keywords: Windows 10, UWP, métadonnées, pile, discours, chapitre
ms.localizationpriority: medium
ms.openlocfilehash: 4ac8140abc1c93e8e2b249bb6040cef3789f1d6a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175673"
---
# <a name="system-supported-timed-metadata-cues"></a>Indicateurs de métadonnées synchronisées pris en charge par le système
Cet article explique comment tirer parti de plusieurs formats de métadonnées chronométrées qui peuvent être incorporées dans des fichiers multimédias ou des flux. Les applications UWP peuvent s’inscrire aux événements déclenchés par le pipeline de média lors de la lecture à chaque fois que ces indications de métadonnées sont rencontrées. À l’aide de la classe [**DataCue**](/uwp/api/Windows.Media.Core.DataCue) , les applications peuvent implémenter leurs propres indications de métadonnées personnalisées, mais cet article se concentre sur plusieurs normes de métadonnées qui sont détectées automatiquement par le pipeline de média, y compris :

* Sous-titres basés sur une image au format VobSub
* Signaux vocaux, y compris les limites de mots, les limites de phrases et les signets SSML (speech synthesis Markup Language)
* Indications sur les chapitres
* Commentaires M3U étendus
* Balises ID3
* Zones EMSG MP4 fragmentées


Cet article s’appuie sur les concepts abordés dans l’article [éléments multimédias, playlists et suivis](media-playback-with-mediasource.md), qui comprend les principes fondamentaux de l’utilisation des classes [**MediaSource**](/uwp/api/windows.media.core.mediasource), [**MediaPlaybackItem**](/uwp/api/windows.media.playback.mediaplaybackitem)et [**TimedMetadataTrack**](/uwp/api/Windows.Media.Core.TimedMetadataTrack) , ainsi que des conseils généraux pour l’utilisation des métadonnées chronométrées dans votre application.

Les étapes de mise en œuvre de base sont les mêmes pour tous les différents types de métadonnées chronométrées décrites dans cet article :

1. Créez un [**MediaSource**](/uwp/api/windows.media.core.mediasource) , puis un [**MediaPlaybackItem**](/uwp/api/windows.media.playback.mediaplaybackitem) pour le contenu à lire.
2. Inscrivez-vous à l’événement [**MediaPlaybackItem. TimedMetadataTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) , qui se produit lorsque les sous-pistes de l’élément multimédia sont résolues par le pipeline multimédia.
3. Inscrivez-vous aux événements [**TimedMetadataTrack. CueEntered**](/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) et [**TimedMetadataTrack. CueExited**](/uwp/api/windows.media.core.timedmetadatatrack.CueExited) pour les pistes de métadonnées chronométrées que vous souhaitez utiliser.
4. Dans le gestionnaire d’événements **CueEntered** , mettez à jour votre interface utilisateur en fonction des métadonnées passées dans les arguments d’événement. Vous pouvez à nouveau mettre à jour l’interface utilisateur pour supprimer le texte du sous-titre actuel, par exemple, dans l’événement **CueExited** .

Dans cet article, le traitement de chaque type de métadonnées s’affiche sous la forme d’un scénario distinct, mais il est possible de gérer (ou d’ignorer) les différents types de métadonnées à l’aide du code partagé. Vous pouvez vérifier la propriété [**TimedMetadataKind**](/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) de l’objet [**TimedMetadataTrack**](/uwp/api/windows.media.core.timedmetadatatrack) à plusieurs points du processus. Par exemple, vous pouvez choisir de vous inscrire à l’événement **CueEntered** pour les suivis de métadonnées qui ont la valeur **TimedMetadataKind. ImageSubtitle**, mais pas pour les pistes ayant la valeur **TimedMetadataKind. Speech**. Au lieu de cela, vous pouvez inscrire un gestionnaire pour tous les types de suivi de métadonnées, puis vérifier la valeur **TimedMetadataKind** dans le gestionnaire **CueEntered** pour déterminer l’action à entreprendre en réponse à la file d’attente.

## <a name="image-based-subtitles"></a>Sous-titres basés sur une image
À compter de Windows 10, version 1703, les applications UWP peuvent prendre en charge des sous-titres basés sur des images externes au format VobSub. Pour utiliser cette fonctionnalité, commencez par créer un objet [**MediaSource**](/uwp/api/windows.media.core.mediasource) pour le contenu multimédia pour lequel des sous-titres d’image seront affichés. Ensuite, créez un objet [**TimedTextSource**](/uwp/api/windows.media.core.timedtextsource) en appelant [**CreateFromUriWithIndex**](/uwp/api/windows.media.core.timedtextsource.CreateFromUriWithIndex) ou [**CreateFromStreamWithIndex**](/uwp/api/windows.media.core.timedtextsource.CreateFromStreamWithIndex), en passant l’URI du fichier. Sub contenant les données d’image de sous-titre et le fichier. idx contenant les informations de minutage pour les sous-titres. Ajoutez le **TimedTextSource** au **MediaSource** en l’ajoutant à la collection [**ExternalTimedTextSources**](/uwp/api/windows.media.core.mediasource.ExternalTimedTextSources) de la source. Créez un [**MediaPlaybackItem**](/uwp/api/windows.media.playback.mediaplaybackitem) à partir de **MediaSource**.

[!code-cs[ImageSubtitleLoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetImageSubtitleLoadContent)]

Inscrivez-vous aux événements de métadonnées de sous-titre d’image à l’aide de l’objet **MediaPlaybackItem** créé à l’étape précédente. Cet exemple utilise une méthode d’assistance, **RegisterMetadataHandlerForImageSubtitles**, pour s’inscrire aux événements. Une expression lambda est utilisée pour implémenter un gestionnaire pour l’événement [**TimedMetadataTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) , qui se produit lorsque le système détecte une modification dans les pistes de métadonnées associées à un **MediaPlaybackItem**. Dans certains cas, les suivis de métadonnées peuvent être disponibles lorsque l’élément de lecture est initialement résolu. par conséquent, en dehors du gestionnaire **TimedMetadataTracksChanged** , nous parcourons également les pistes de métadonnées disponibles et appelons **RegisterMetadataHandlerForImageSubtitles**.

[!code-cs[ImageSubtitleTracksChanged](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetImageSubtitleTracksChanged)]

Après s’être inscrit pour les événements de métadonnées de sous-titre d’image, **MediaItem** est assigné à un [**MediaPlayer**](/uwp/api/windows.media.playback.mediaplayer) pour la lecture dans un [**MediaPlayerElement**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement).

[!code-cs[ImageSubtitlePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetImageSubtitlePlay)]

Dans la méthode d’assistance **RegisterMetadataHandlerForImageSubtitles** , récupérez une instance de la classe [**TimedMetadataTrack**](/uwp/api/windows.media.core.timedmetadatatrack) en l’indexant dans la collection **TimedMetadataTracks** de **MediaPlaybackItem**. Inscrivez-vous à l’événement [**CueEntered**](/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) et à l’événement [**CueExited**](/uwp/api/windows.media.core.timedmetadatatrack.CueExited) . Ensuite, vous devez appeler [**SetPresentationMode**](/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode) sur la collection **TimedMetadataTracks** de l’élément de lecture pour indiquer au système que l’application souhaite recevoir des événements de pile de métadonnées pour cet élément de lecture.

[!code-cs[RegisterMetadataHandlerForImageSubtitles](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForImageSubtitles)]

Dans le gestionnaire de l’événement **CueEntered** , vous pouvez vérifier la [**TimedMetadataKind**](/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) de l’objet [**TimedMetadataTrack**](/uwp/api/windows.media.core.timedmetadatatrack) passé dans le gestionnaire pour voir si les métadonnées sont pour les sous-titres d’image. Cela est nécessaire si vous utilisez le même gestionnaire d’événements Data CUE pour plusieurs types de métadonnées. Si la piste de métadonnées associée est de type **TimedMetadataKind. ImageSubtitle**, effectuez un cast de la pile de données contenue dans la propriété de **pile** de [**MediaCueEventArgs**](/uwp/api/windows.media.core.mediacueeventargs) en [**ImageCue**](/uwp/api/windows.media.core.imagecue). La propriété [**SoftwareBitmap**](/uwp/api/windows.media.core.imagecue.SoftwareBitmap) de **ImageCue** contient une représentation [**SoftwareBitmap**](/uwp/api/windows.graphics.imaging.softwarebitmap) de l’image de sous-titre. Créez un [**SoftwareBitmapSource**](/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource) et appelez [**SetBitmapAsync**](/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.SetBitmapAsync) pour assigner l’image à un contrôle [**image**](/uwp/api/windows.ui.xaml.controls.image) XAML. Les propriétés d' [**étendue**](/uwp/api/windows.media.core.imagecue.Extent) et de [**position**](/uwp/api/windows.media.core.imagecue.Position) du **ImageCue** fournissent des informations sur la taille et la position de l’image de sous-titre.

[!code-cs[ImageSubtitleCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetImageSubtitleCueEntered)]

## <a name="speech-cues"></a>Signaux vocaux
À compter de Windows 10, version 1703, les applications UWP peuvent s’inscrire pour recevoir des événements en réponse à des limites de mots, des limites de phrases et des signets SSML dans des médias lus. Cela vous permet de lire des flux audio générés avec la classe [**SpeechSynthesizer**](/uwp/api/Windows.Media.SpeechSynthesis.SpeechSynthesizer) et de mettre à jour votre interface utilisateur en fonction de ces événements, tels que l’affichage du texte du mot ou de la phrase en cours de lecture.

L’exemple présenté dans cette section utilise une variable de membre de classe pour stocker une chaîne de texte qui sera synthétisée et lue.

[!code-cs[SpeechInputText](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSpeechInputText)]

Créez une nouvelle instance de la classe **SpeechSynthesizer** . Définissez les options [**IncludeWordBoundaryMetadata**](/uwp/api/windows.media.speechsynthesis.speechsynthesizeroptions.IncludeWordBoundaryMetadata) et [**IncludeSentenceBoundaryMetadata**](/uwp/api/windows.media.speechsynthesis.speechsynthesizeroptions.IncludeSentenceBoundaryMetadata) pour le synthétiseur sur **true** pour spécifier que les métadonnées doivent être incluses dans le flux de média généré. Appelez [**SynthesizeTextToStreamAsync**](/uwp/api/Windows.Media.SpeechSynthesis.SpeechSynthesizer.SynthesizeTextToStreamAsync) pour générer un flux contenant la parole synthétisée et les métadonnées correspondantes. Créez un [**MediaSource**](/uwp/api/windows.media.core.mediasource) et un [**MediaPlaybackItem**](/uwp/api/windows.media.playback.mediaplaybackitem) à partir du flux synthétisé.

[!code-cs[SynthesizeSpeech](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSynthesizeSpeech)]

Inscrivez-vous aux événements de métadonnées vocales à l’aide de l’objet **MediaPlaybackItem** . Cet exemple utilise une méthode d’assistance, **RegisterMetadataHandlerForSpeech**, pour s’inscrire aux événements. Une expression lambda est utilisée pour implémenter un gestionnaire pour l’événement [**TimedMetadataTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) , qui se produit lorsque le système détecte une modification dans les pistes de métadonnées associées à un **MediaPlaybackItem**.  Dans certains cas, les suivis de métadonnées peuvent être disponibles lorsque l’élément de lecture est initialement résolu. par conséquent, en dehors du gestionnaire **TimedMetadataTracksChanged** , nous parcourons également les pistes de métadonnées disponibles et appelons **RegisterMetadataHandlerForSpeech**.

[!code-cs[SpeechTracksChanged](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSpeechTracksChanged)]

Une fois inscrit pour les événements de métadonnées vocaux, **MediaItem** est affecté à un [**MediaPlayer**](/uwp/api/windows.media.playback.mediaplayer) pour la lecture dans un [**MediaPlayerElement**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement).

[!code-cs[SpeechPlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSpeechPlay)]

Dans la méthode d’assistance **RegisterMetadataHandlerForSpeech** , récupérez une instance de la classe [**TimedMetadataTrack**](/uwp/api/windows.media.core.timedmetadatatrack) en l’indexant dans la collection **TimedMetadataTracks** de **MediaPlaybackItem**. Inscrivez-vous à l’événement [**CueEntered**](/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) et à l’événement [**CueExited**](/uwp/api/windows.media.core.timedmetadatatrack.CueExited) . Ensuite, vous devez appeler [**SetPresentationMode**](/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode) sur la collection **TimedMetadataTracks** de l’élément de lecture pour indiquer au système que l’application souhaite recevoir des événements de pile de métadonnées pour cet élément de lecture.

[!code-cs[RegisterMetadataHandlerForWords](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForWords)]

Dans le gestionnaire de l’événement **CueEntered** , vous pouvez vérifier la [**TimedMetadataKind**](/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) de l’objet [**TimedMetadataTrack**](/uwp/api/windows.media.core.timedmetadatatrack) passé dans le gestionnaire pour voir si les métadonnées sont vocales. Cela est nécessaire si vous utilisez le même gestionnaire d’événements Data CUE pour plusieurs types de métadonnées. Si la piste de métadonnées associée est de type **TimedMetadataKind. Speech**, effectuez un cast de la pile de données contenue dans la propriété de **pile** de [**MediaCueEventArgs**](/uwp/api/windows.media.core.mediacueeventargs) en [**SpeechCue**](/uwp/api/windows.media.core.speechcue). Pour les signaux vocaux, le type de signalement vocal inclus dans la piste des métadonnées est déterminé par la vérification de la propriété **label** . La valeur de cette propriété sera « SpeechWord » pour les limites de mots, « SpeechSentence » pour les limites de phrases, ou « SpeechBookmark » pour les signets SSML. Dans cet exemple, nous vérifions la valeur « SpeechWord » et, si cette valeur est trouvée, les propriétés [**StartPositionInInput**](/uwp/api/windows.media.core.speechcue.StartPositionInInput) et [**EndPositionInInput**](/uwp/api/windows.media.core.speechcue.EndPositionInInput) de l' **SpeechCue** sont utilisées pour déterminer l’emplacement dans le texte d’entrée du mot en cours de lecture. Cet exemple renvoie simplement chaque mot à la sortie de débogage.

[!code-cs[SpeechWordCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetSpeechWordCueEntered)]

## <a name="chapter-cues"></a>Indications sur les chapitres
À compter de Windows 10, version 1703, les applications UWP peuvent s’inscrire à des signaux qui correspondent aux chapitres d’un élément multimédia. Pour utiliser cette fonctionnalité, créez un objet [**MediaSource**](/uwp/api/windows.media.core.mediasource) pour le contenu multimédia, puis créez un [**MediaPlaybackItem**](/uwp/api/windows.media.playback.mediaplaybackitem) à partir de **MediaSource**.

[!code-cs[ChapterCueLoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetChapterCueLoadContent)]

Inscrivez-vous au chapitre événements de métadonnées à l’aide de l’objet **MediaPlaybackItem** créé à l’étape précédente. Cet exemple utilise une méthode d’assistance, **RegisterMetadataHandlerForChapterCues**, pour s’inscrire aux événements. Une expression lambda est utilisée pour implémenter un gestionnaire pour l’événement [**TimedMetadataTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) , qui se produit lorsque le système détecte une modification dans les pistes de métadonnées associées à un **MediaPlaybackItem**. Dans certains cas, les suivis de métadonnées peuvent être disponibles lorsque l’élément de lecture est initialement résolu. par conséquent, en dehors du gestionnaire **TimedMetadataTracksChanged** , nous parcourons également les pistes de métadonnées disponibles et appelons **RegisterMetadataHandlerForChapterCues**.

[!code-cs[ChapterCueTracksChanged](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetChapterCueTracksChanged)]

Après l’inscription des événements de métadonnées du chapitre, **MediaItem** est affecté à un [**MediaPlayer**](/uwp/api/windows.media.playback.mediaplayer) pour la lecture dans un [**MediaPlayerElement**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement).

[!code-cs[ChapterCuePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetChapterCuePlay)]

Dans la méthode d’assistance **RegisterMetadataHandlerForChapterCues** , récupérez une instance de la classe [**TimedMetadataTrack**](/uwp/api/windows.media.core.timedmetadatatrack) en l’indexant dans la collection **TimedMetadataTracks** de **MediaPlaybackItem**. Inscrivez-vous à l’événement [**CueEntered**](/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) et à l’événement [**CueExited**](/uwp/api/windows.media.core.timedmetadatatrack.CueExited) . Ensuite, vous devez appeler [**SetPresentationMode**](/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode) sur la collection **TimedMetadataTracks** de l’élément de lecture pour indiquer au système que l’application souhaite recevoir des événements de pile de métadonnées pour cet élément de lecture.

[!code-cs[RegisterMetadataHandlerForChapterCues](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForChapterCues)]

Dans le gestionnaire de l’événement **CueEntered** , vous pouvez vérifier la [**TimedMetadataKind**](/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) de l’objet [**TimedMetadataTrack**](/uwp/api/windows.media.core.timedmetadatatrack) passé dans le gestionnaire pour voir si les métadonnées sont destinées à des indications de chapitre. Cela est nécessaire si vous utilisez le même gestionnaire d’événements Data CUE pour plusieurs types de métadonnées. Si la piste de métadonnées associée est de type **TimedMetadataKind. Chapter**, effectuez un cast de la pile de données contenue dans la propriété de **pile** de [**MediaCueEventArgs**](/uwp/api/windows.media.core.mediacueeventargs) en [**ChapterCue**](/uwp/api/windows.media.core.chaptercue). La propriété [**title**](/uwp/api/windows.media.core.chaptercue.Title) de la **ChapterCue** contient le titre du chapitre qui vient d’être atteint dans la lecture.

[!code-cs[ChapterCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetChapterCueEntered)]

### <a name="seek-to-the-next-chapter-using-chapter-cues"></a>Rechercher le chapitre suivant à l’aide des indications de chapitre
En plus de recevoir des notifications lorsque le chapitre actuel est modifié dans un élément joué, vous pouvez également utiliser des indications de chapitre pour rechercher le chapitre suivant dans un élément joué. L’exemple de méthode ci-dessous prend comme arguments un [**MediaPlayer**](/uwp/api/windows.media.playback.mediaplayer) et un [**MediaPlaybackItem**](/uwp/api/windows.media.playback.mediaplaybackitem) représentant l’élément multimédia en cours de diffusion. La collection [**TimedMetadataTracks**](/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracks) est recherchée pour vérifier si l’une des pistes a [**TimedMetadataKind**](/uwp/api/windows.media.core.timedmetadatatrack.TimedMetadataKind) propre à la valeur [**TimedMetadataTrack**](/uwp/api/windows.media.core.timedmetadatatrack) de **TimedMetadataKind. Chapter**.  Si un suivi de chapitre est trouvé, la méthode parcourt chaque pile de la collection de [**signaux**](/uwp/api/windows.media.core.timedmetadatatrack.Cues) de la piste pour trouver la première pile qui a une [**heure de début**](/uwp/api/windows.media.core.chaptercue.StartTime) supérieure à la [**position**](/uwp/api/windows.media.playback.mediaplaybacksession.Position) actuelle de la session de lecture du lecteur multimédia. Une fois la file d’attente correcte trouvée, la position de la session de lecture est mise à jour et le titre du chapitre est mis à jour dans l’interface utilisateur.

[!code-cs[GoToNextChapter](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetGoToNextChapter)]

## <a name="extended-m3u-comments"></a>Commentaires M3U étendus
À compter de Windows 10, version 1703, les applications UWP peuvent s’inscrire pour les signaux qui correspondent aux commentaires dans un fichier manifeste M3U étendu. Cet exemple utilise [**AdaptiveMediaSource**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) pour lire le contenu multimédia. Pour plus d’informations, consultez [streaming adaptative](adaptive-streaming.md). Créez un **AdaptiveMediaSource** pour le contenu en appelant [**CreateFromUriAsync**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromUriAsync) ou [**CreateFromStreamAsync**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromStreamAsync). Créez un objet  [**MediaSource**](/uwp/api/windows.media.core.mediasource) en appelant [**CreateFromAdaptiveMediaSource**](/uwp/api/windows.media.core.mediasource.CreateFromAdaptiveMediaSource) , puis créez un [**MediaPlaybackItem**](/uwp/api/windows.media.playback.mediaplaybackitem) à partir de **MediaSource**.

[!code-cs[EXTM3ULoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3ULoadContent)]

Inscrivez-vous aux événements de métadonnées M3U à l’aide de l’objet **MediaPlaybackItem** créé à l’étape précédente. Cet exemple utilise une méthode d’assistance, **RegisterMetadataHandlerForEXTM3UCues**, pour s’inscrire aux événements. Une expression lambda est utilisée pour implémenter un gestionnaire pour l’événement [**TimedMetadataTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) , qui se produit lorsque le système détecte une modification dans les pistes de métadonnées associées à un **MediaPlaybackItem**. Dans certains cas, les suivis de métadonnées peuvent être disponibles lorsque l’élément de lecture est initialement résolu. par conséquent, en dehors du gestionnaire **TimedMetadataTracksChanged** , nous parcourons également les pistes de métadonnées disponibles et appelons **RegisterMetadataHandlerForEXTM3UCues**.

[!code-cs[EXTM3UCueTracksChanged](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3UCueTracksChanged)]

Une fois inscrit pour les événements de métadonnées M3U, **MediaItem** est assigné à un [**MediaPlayer**](/uwp/api/windows.media.playback.mediaplayer) pour la lecture dans un [**MediaPlayerElement**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement).

[!code-cs[EXTM3UCuePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3UCuePlay)]

Dans la méthode d’assistance **RegisterMetadataHandlerForEXTM3UCues** , récupérez une instance de la classe [**TimedMetadataTrack**](/uwp/api/windows.media.core.timedmetadatatrack) en l’indexant dans la collection **TimedMetadataTracks** de **MediaPlaybackItem**. Vérifiez la propriété DispatchType de la piste des métadonnées, qui aura la valeur « EXTM3U » si la piste représente des commentaires M3U. Inscrivez-vous à l’événement [**CueEntered**](/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) et à l’événement [**CueExited**](/uwp/api/windows.media.core.timedmetadatatrack.CueExited) . Ensuite, vous devez appeler [**SetPresentationMode**](/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode) sur la collection **TimedMetadataTracks** de l’élément de lecture pour indiquer au système que l’application souhaite recevoir des événements de pile de métadonnées pour cet élément de lecture.

[!code-cs[RegisterMetadataHandlerForEXTM3UCues](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForEXTM3UCues)]

Dans le gestionnaire de l’événement **CueEntered** , effectuez un cast de la file d’attente de données contenue dans la propriété **CUE** de [**MediaCueEventArgs**](/uwp/api/windows.media.core.mediacueeventargs) en [**DataCue**](/uwp/api/windows.media.core.datacue).  Vérifiez que **DataCue** et la propriété [**Data**](/uwp/api/windows.media.core.datacue.Data) de la file d’attente n’ont pas la valeur null. Les commentaires UME étendus sont fournis sous la forme d’UTF-16, Little endian, chaînes terminées par la valeur null. Créez un **DataReader** pour lire les données de la pile en appelant [**DataReader. FromBuffer**](/uwp/api/windows.storage.streams.datareader.FromBuffer). Définissez la propriété [**UnicodeEncoding**](/uwp/api/windows.storage.streams.datareader.UnicodeEncoding) du lecteur sur [**Utf16LE**](/uwp/api/windows.storage.streams.unicodeencoding) pour lire les données dans le format approprié. Appelez [**ReadString**](/uwp/api/windows.storage.streams.datareader.ReadString) pour lire les données, en spécifiant la moitié de la longueur du champ de **données** , car chaque caractère a une taille de deux octets et en soustraire un pour supprimer le caractère null de fin. Dans cet exemple, le commentaire M3U est simplement écrit dans la sortie de débogage.

[!code-cs[EXTM3UCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3UCueEntered)]

## <a name="id3-tags"></a>Balises ID3
À compter de Windows 10, version 1703, les applications UWP peuvent s’inscrire à des signaux qui correspondent à des balises ID3 au sein du contenu http live streaming (TLS). Cet exemple utilise [**AdaptiveMediaSource**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) pour lire le contenu multimédia. Pour plus d’informations, consultez [streaming adaptative](adaptive-streaming.md). Créez un **AdaptiveMediaSource** pour le contenu en appelant [**CreateFromUriAsync**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromUriAsync) ou [**CreateFromStreamAsync**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromStreamAsync). Créez un objet  [**MediaSource**](/uwp/api/windows.media.core.mediasource) en appelant [**CreateFromAdaptiveMediaSource**](/uwp/api/windows.media.core.mediasource.CreateFromAdaptiveMediaSource) , puis créez un [**MediaPlaybackItem**](/uwp/api/windows.media.playback.mediaplaybackitem) à partir de **MediaSource**.

[!code-cs[EXTM3ULoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEXTM3ULoadContent)]

Inscrivez-vous aux événements de balise ID3 à l’aide de l’objet **MediaPlaybackItem** créé à l’étape précédente. Cet exemple utilise une méthode d’assistance, **RegisterMetadataHandlerForID3Cues**, pour s’inscrire aux événements. Une expression lambda est utilisée pour implémenter un gestionnaire pour l’événement [**TimedMetadataTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) , qui se produit lorsque le système détecte une modification dans les pistes de métadonnées associées à un **MediaPlaybackItem**. Dans certains cas, les suivis de métadonnées peuvent être disponibles lorsque l’élément de lecture est initialement résolu. par conséquent, en dehors du gestionnaire **TimedMetadataTracksChanged** , nous parcourons également les pistes de métadonnées disponibles et appelons **RegisterMetadataHandlerForID3Cues**.

[!code-cs[ID3LoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetID3LoadContent)]

Une fois inscrit pour les événements de métadonnées ID3, **MediaItem** est assigné à un [**MediaPlayer**](/uwp/api/windows.media.playback.mediaplayer) pour la lecture dans un [**MediaPlayerElement**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement).

[!code-cs[ID3CuePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetID3CuePlay)]


Dans la méthode d’assistance **RegisterMetadataHandlerForID3Cues** , récupérez une instance de la classe [**TimedMetadataTrack**](/uwp/api/windows.media.core.timedmetadatatrack) en l’indexant dans la collection **TimedMetadataTracks** de **MediaPlaybackItem**. Vérifiez la propriété DispatchType de la piste des métadonnées, qui aura une valeur contenant la chaîne GUID « 15260DFFFF49443320FF49443320000F » si la piste représente des balises ID3. Inscrivez-vous à l’événement [**CueEntered**](/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) et à l’événement [**CueExited**](/uwp/api/windows.media.core.timedmetadatatrack.CueExited) . Ensuite, vous devez appeler [**SetPresentationMode**](/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode) sur la collection **TimedMetadataTracks** de l’élément de lecture pour indiquer au système que l’application souhaite recevoir des événements de pile de métadonnées pour cet élément de lecture.

[!code-cs[RegisterMetadataHandlerForID3Cues](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForID3Cues)]

Dans le gestionnaire de l’événement **CueEntered** , effectuez un cast de la file d’attente de données contenue dans la propriété **CUE** de [**MediaCueEventArgs**](/uwp/api/windows.media.core.mediacueeventargs) en [**DataCue**](/uwp/api/windows.media.core.datacue).  Vérifiez que **DataCue** et la propriété [**Data**](/uwp/api/windows.media.core.datacue.Data) de la file d’attente n’ont pas la valeur null. Les commentaires d’ume étendus sont fournis sous la forme d’octets bruts dans le flux de transport (consultez [http://id3.org/id3v2.4.0-structure](https://id3.org/id3v2.4.0-structure) ). Créez un **DataReader** pour lire les données de la pile en appelant [**DataReader. FromBuffer**](/uwp/api/windows.storage.streams.datareader.FromBuffer).  Dans cet exemple, les valeurs d’en-tête de la balise ID3 sont lues à partir des données de la pile et écrites dans la sortie de débogage.

[!code-cs[ID3CueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetID3CueEntered)]

## <a name="fragmented-mp4-emsg-boxes"></a>Zones EMSG MP4 fragmentées
À compter de Windows 10, version 1703, les applications UWP peuvent s’inscrire à des signaux qui correspondent à des zones EMSG dans des flux MP4 fragmentés. Un exemple d’utilisation de ce type de métadonnées est que les fournisseurs de contenu signalent aux applications clientes qu’elles lisent une publicité pendant le contenu de diffusion en continu. Cet exemple utilise [**AdaptiveMediaSource**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) pour lire le contenu multimédia. Pour plus d’informations, consultez [streaming adaptative](adaptive-streaming.md). Créez un **AdaptiveMediaSource** pour le contenu en appelant [**CreateFromUriAsync**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromUriAsync) ou [**CreateFromStreamAsync**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.CreateFromStreamAsync). Créez un objet  [**MediaSource**](/uwp/api/windows.media.core.mediasource) en appelant [**CreateFromAdaptiveMediaSource**](/uwp/api/windows.media.core.mediasource.CreateFromAdaptiveMediaSource) , puis créez un [**MediaPlaybackItem**](/uwp/api/windows.media.playback.mediaplaybackitem) à partir de **MediaSource**.

[!code-cs[EmsgLoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEmsgLoadContent)]

Inscrivez-vous pour les événements Box EMSG à l’aide de l’objet **MediaPlaybackItem** créé à l’étape précédente. Cet exemple utilise une méthode d’assistance, **RegisterMetadataHandlerForEmsgCues**, pour s’inscrire aux événements. Une expression lambda est utilisée pour implémenter un gestionnaire pour l’événement [**TimedMetadataTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.TimedMetadataTracksChanged) , qui se produit lorsque le système détecte une modification dans les pistes de métadonnées associées à un **MediaPlaybackItem**. Dans certains cas, les suivis de métadonnées peuvent être disponibles lorsque l’élément de lecture est initialement résolu. par conséquent, en dehors du gestionnaire **TimedMetadataTracksChanged** , nous parcourons également les pistes de métadonnées disponibles et appelons **RegisterMetadataHandlerForEmsgCues**.

[!code-cs[ID3LoadContent](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetID3LoadContent)]

Après l’inscription des événements de métadonnées de la boîte de EMSG, le **MediaItem** est assigné à un [**MediaPlayer**](/uwp/api/windows.media.playback.mediaplayer) pour la lecture dans un [**MediaPlayerElement**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement).

[!code-cs[EmsgCuePlay](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEmsgCuePlay)]


Dans la méthode d’assistance **RegisterMetadataHandlerForEmsgCues** , récupérez une instance de la classe [**TimedMetadataTrack**](/uwp/api/windows.media.core.timedmetadatatrack) en l’indexant dans la collection **TimedMetadataTracks** de **MediaPlaybackItem**. Vérifiez la propriété DispatchType de la piste des métadonnées, qui aura la valeur « EMSG : MP4 » si la piste représente des zones de EMSG. Inscrivez-vous à l’événement [**CueEntered**](/uwp/api/windows.media.core.timedmetadatatrack.CueEntered) et à l’événement [**CueExited**](/uwp/api/windows.media.core.timedmetadatatrack.CueExited) . Ensuite, vous devez appeler [**SetPresentationMode**](/uwp/api/windows.media.playback.mediaplaybacktimedmetadatatracklist.SetPresentationMode) sur la collection **TimedMetadataTracks** de l’élément de lecture pour indiquer au système que l’application souhaite recevoir des événements de pile de métadonnées pour cet élément de lecture.


[!code-cs[RegisterMetadataHandlerForEmsgCues](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetRegisterMetadataHandlerForEmsgCues)]


Dans le gestionnaire de l’événement **CueEntered** , effectuez un cast de la file d’attente de données contenue dans la propriété **CUE** de [**MediaCueEventArgs**](/uwp/api/windows.media.core.mediacueeventargs) en [**DataCue**](/uwp/api/windows.media.core.datacue).  Vérifiez que l’objet **DataCue** n’est pas null. Les corrections de la zone EMSG sont fournies par le pipeline de média en tant que propriétés personnalisées dans la collection de [**Propriétés**](/uwp/api/windows.media.core.datacue.Properties) de l’objet DataCue. Cet exemple tente d’extraire plusieurs valeurs de propriété différentes à l’aide de la méthode **[TryGetValue](/uwp/api/windows.foundation.collections.propertyset.trygetvalue)** . Si cette méthode retourne la valeur null, cela signifie que le correcteur demandé n’est pas présent dans la zone EMSG. par conséquent, une valeur par défaut est définie à la place.

La partie suivante de l’exemple illustre le scénario dans lequel la lecture Active Directory est déclenchée, ce qui est le cas lorsque la propriété *scheme_id_uri* , obtenue à l’étape précédente, a la valeur « urn : SCTE : scte35:2013 : XML » (consultez [http://dashif.org/identifiers/event-schemes/](https://dashif.org/identifiers/event-schemes/) ). Notez que la norme recommande d’envoyer plusieurs fois ce EMSG pour la redondance. cet exemple gère donc une liste des ID EMSG qui ont déjà été traités et traite uniquement les nouveaux messages. Créez un **DataReader** pour lire les données de la file d’attente en appelant [**DataReader. FromBuffer**](/uwp/api/windows.storage.streams.datareader.FromBuffer) et définissez l’encodage UTF-8 en définissant la propriété [**UnicodeEncoding**](/uwp/api/windows.storage.streams.datareader.UnicodeEncoding) , puis lisez les données. Dans cet exemple, la charge utile de message est écrite dans la sortie de débogage. Une application réelle utilise les données de charge utile pour planifier la lecture d’une publicité.

[!code-cs[EmsgCueEntered](./code/MediaSource_RS1/cs/MainPage_Cues.xaml.cs#SnippetEmsgCueEntered)]


## <a name="related-topics"></a>Rubriques connexes

* [Lecture de contenu multimédia](media-playback.md)
* [Éléments, playlists et pistes multimédias](media-playback-with-mediasource.md)


 