---
ms.assetid: AE98C22B-A071-4206-ABBB-C0F0FB7EF33C
description: Cet article décrit comment ajouter la lecture de contenu multimédia en streaming adaptatif à une application de plateforme Windows universelle (UWP). Cette fonctionnalité prend actuellement en charge la lecture de contenu vidéo en streaming HTTP (HLS) et de contenu en streaming dynamique sur HTTP (DASH).
title: Diffusion en continu adaptative
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 11e960699c64b005324d99b89b61dc8ee97961f7
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363002"
---
# <a name="adaptive-streaming"></a>Diffusion en continu adaptative


Cet article décrit comment ajouter la lecture de contenu multimédia en streaming adaptatif à une application de plateforme Windows universelle (UWP). Cette fonctionnalité prend en charge la lecture de contenu http live streaming (TLS) et streaming dynamique sur HTTP (DASH). À compter de Windows 10, la version 1803, Smooth Streaming est pris en charge par  **[AdaptiveMediaSource](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource)**.

Pour obtenir la liste des balises de protocole HLS prises en charge, voir [Prise en charge des balises HLS](hls-tag-support.md). 

Pour obtenir la liste des profils de TIREts pris en charge, consultez [prise en charge des profils Dash](dash-profile-support.md). 

> [!NOTE] 
> Le code de cet article a été adapté à partir de l’[exemple de streaming adaptatif](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/AdaptiveStreaming) UWP.

## <a name="simple-adaptive-streaming-with-mediaplayer-and-mediaplayerelement"></a>Streaming adaptatif avec MediaPlayer et MediaPlayerElement

Pour lire du contenu multimédia en streaming adaptatif dans une application UWP, créez un objet **URI** pointant sur un fichier manifeste HLS ou DASH. Créez une instance de la classe [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) . Appelez [**MediaSource.CreateFromUri**](/uwp/api/windows.media.core.mediasource.createfromuri) pour créer un objet **MediaSource**, puis définissez-le sur la propriété [**Source**](/uwp/api/windows.media.playback.mediaplayer.source) de **MediaPlayer**. Appelez [**Play**](/uwp/api/windows.media.playback.mediaplayer.play) pour démarrer la lecture du contenu multimédia.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareMediaPlayer":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs" id="SnippetManifestSourceNoUI":::

L’exemple ci-dessus lit l’audio du contenu multimédia, mais n’affiche pas automatiquement le contenu dans votre interface Utilisateur. La plupart des applications qui lisent du contenu vidéo veulent restituer le contenu dans une page XAML.  Pour ce faire, ajoutez un contrôle [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) à votre page XAML.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/AdaptiveStreaming_RS1/cs/MainPage.xaml" id="SnippetMediaPlayerElementXAML":::

Appelez [**MediaSource.CreateFromUri**](/uwp/api/windows.media.core.mediasource.createfromuri) pour créer un **MediaSource** à partir de l’URI d’un fichier manifeste HLS ou DASH. Ensuite, définissez la propriété [**Source**](/uwp/api/windows.ui.xaml.controls.mediaelement.sourceproperty) de **MediaPlayerElement**. **MediaPlayerElement** crée automatiquement un objet **MediaPlayer** pour le contenu. Vous pouvez appeler **Play** sur **MediaPlayer** pour démarrer la lecture du contenu.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs" id="SnippetManifestSource":::

> [!NOTE] 
> À compter de Windows 10, version 1607, nous vous recommandons d’utiliser la classe **MediaPlayer** pour lire des éléments multimédias. **MediaPlayerElement** est un contrôle XAML léger utilisé pour afficher le contenu d’un **MediaPlayer** dans une page XAML. Le contrôle **MediaElement** continue d’être pris en charge pour la compatibilité descendante. Pour plus d’informations sur l’utilisation de **MediaPlayer** et **MediaPlayerElement** pour lire du contenu multimédia, voir [Lire du contenu audio et vidéo avec MediaPlayer](play-audio-and-video-with-mediaplayer.md). Pour plus d’informations sur l’utilisation de **MediaSource** et des API associées pour utiliser du contenu multimédia, voir [Éléments, playlists et pistes multimédias](media-playback-with-mediasource.md).

## <a name="adaptive-streaming-with-adaptivemediasource"></a>Streaming adaptatif avec AdaptiveMediaSource

Si votre application nécessite des fonctionnalités de diffusion en continu adaptative plus avancées, telles que la mise à disposition d’en-têtes HTTP personnalisés, la surveillance des débits de téléchargement et de lecture actuels, ou l’ajustement des ratios déterminant quand le système commute les débits binaires du flux adaptatif, utilisez l’objet **[AdaptiveMediaSource](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource)** .

Les API de diffusion adaptative se trouvent dans l’espace de noms [**Windows. Media. streaming. adaptative**](/uwp/api/Windows.Media.Streaming.Adaptive) . Les exemples de cet article utilisent des API provenant des espaces de noms suivants.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs" id="SnippetAdaptiveStreamingUsing":::

## <a name="initialize-an-adaptivemediasource-from-a-uri"></a>Initialise un AdaptiveMediaSource à partir d’un URI.

Initialisez l’élément **AdaptiveMediaSource** avec l’URI d’un fichier manifeste de streaming adaptatif en appelant la méthode [**CreateFromUriAsync**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.createfromuriasync). La valeur [**AdaptiveMediaSourceCreationStatus**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceCreationStatus) retournée par cette méthode vous permet de savoir si la source du média a été créée avec succès. Si c’est le cas, vous pouvez définir l’objet comme source de flux pour votre **MediaPlayer** en créant un objet **MediaSource** en appelant  [**MediaSource. CreateFromAdaptiveMediaSource**](/uwp/api/Windows.Media.Core.MediaSource.AdaptiveMediaSource), puis en l’affectant à la propriété [**source**](/uwp/api/windows.media.playback.mediaplayer.Source) du lecteur multimédia. Dans cet exemple, la propriété [**AvailableBitrates**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.availablebitrates) est interrogée pour déterminer le débit maximal pris en charge pour ce flux, puis cette valeur est définie en tant que vitesse de transmission initiale. Cet exemple inscrit également des gestionnaires pour les différents événements **AdaptiveMediaSource** décrits plus loin dans cet article.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs" id="SnippetInitializeAMS":::

## <a name="initialize-an-adaptivemediasource-using-httpclient"></a>Initialiser un AdaptiveMediaSource à l’aide de HttpClient

Si vous avez besoin de définir des en-têtes HTTP personnalisés pour l’obtention du fichier manifeste, vous pouvez créer un objet [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient), définir les en-têtes souhaités, puis transmettre l’objet dans la surcharge de **CreateFromUriAsync**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs" id="SnippetInitializeAMSWithHttpClient":::

L’événement [**DownloadRequested**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.downloadrequested) est déclenché lorsque le système est sur le du serveur de récupérer une ressource. La classe [**AdaptiveMediaSourceDownloadRequestedEventArgs**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadRequestedEventArgs) transmise dans le gestionnaire d’événements expose les propriétés qui fournissent des informations sur la ressource demandée, telles que le type et l’URI de la ressource.

## <a name="modify-resource-request-properties-using-the-downloadrequested-event"></a>Modifier les propriétés d’une demande de ressource à l’aide de l’événement DownloadRequested

Vous pouvez utiliser le gestionnaire d’événements **DownloadRequested** pour modifier la demande de ressource en mettant à jour les propriétés de l’objet [**AdaptiveMediaSourceDownloadResult**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadResult) fourni par les arguments d’événement. Dans l’exemple ci-dessous, l’URI à partir duquel la ressource sera récupérée est modifié en mettant à jour les propriétés [**resourceuri**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadresult.resourceuri) de l’objet de résultat. Vous pouvez également réécrire le décalage de la plage d’octets et la longueur des segments de média ou, comme illustré ci-dessous, modifier l’URI de ressource pour télécharger la ressource complète et définir le décalage de la plage d’octets et la longueur à null.

Vous pouvez remplacer le contenu de la ressource demandée en définissant les propriétés [**buffer**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadresult.buffer) ou [**InputStream**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadresult.inputstream) de l’objet de résultat. Dans l’exemple ci-après, le contenu de la ressource de manifeste est remplacé par la définition de la propriété **Buffer**. Notez que si vous mettez à jour la demande de ressource avec des données obtenues en mode asynchrone, par exemple dans le cadre d’une récupération de données à partir d’un serveur distant ou d’une authentification utilisateur asynchrone, vous devez appeler [**AdaptiveMediaSourceDownloadRequestedEventArgs.GetDeferral**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadrequestedeventargs.getdeferral) pour obtenir un report, puis [**Complete**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadrequesteddeferral.complete) une fois l’opération terminée pour signaler au système que l’opération de demande de téléchargement peut continuer.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs" id="SnippetAMSDownloadRequested":::

## <a name="use-bitrate-events-to-manage-and-respond-to-bitrate-changes"></a>Utiliser des événements de débit binaire pour gérer les modifications de débit et y répondre

L’objet **AdaptiveMediaSource** fournit des événements qui vous permettent de réagir lorsque les débits de téléchargement ou de lecture changent. Dans cet exemple, les débits actuels sont simplement mis à jour dans l’interface utilisateur. Notez que vous pouvez modifier les ratios qui déterminent le moment où le système change les débits du flux adaptatif. Pour plus d’informations, consultez la propriété [**AdvancedSettings**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.advancedsettings) .

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs" id="SnippetAMSBitrateEvents":::

## <a name="handle-download-completion-and-failure-events"></a>Gérer les événements de réussite et d’échec du téléchargement
L’objet **AdaptiveMediaSource** déclenche l’événement [**DownloadFailed**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource.DownloadFailed) lorsque le téléchargement d’une ressource demandée échoue. Vous pouvez utiliser cet événement pour mettre à jour votre interface utilisateur en réponse à l’échec. Vous pouvez également utiliser l’événement pour enregistrer des informations statistiques sur l’opération de téléchargement et l’échec. 

L’objet [**AdaptiveMediaSourceDownloadFailedEventArgs**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs) passé dans le gestionnaire d’événements contient des métadonnées sur le téléchargement des ressources qui ont échoué, telles que le type de ressource, l’URI de la ressource et la position dans le flux où l’échec s’est produit. [**RequestId**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs.RequestId) obtient un identificateur unique généré par le système pour la demande qui peut être utilisée pour mettre en corrélation les informations d’État relatives à une demande individuelle sur plusieurs événements.

La propriété [**Statistics**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs.Statistics) retourne un objet [**AdaptiveMediaSourceDownloadStatistics**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadstatistics) qui fournit des informations détaillées sur le nombre d’octets reçus au moment de l’événement et le minutage des différentes étapes majeures dans l’opération de téléchargement. Vous pouvez consigner ces informations dans l’ordre pour identifier les problèmes de performances dans votre implémentation de streaming adaptative.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs" id="SnippetAMSDownloadFailed":::


L’événement  [**DownloadCompleted**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource.DownloadCompleted) se produit lorsqu’un téléchargement de ressource est terminé et provdes des données similaires à l’événement **DownloadFailed** . Une fois encore, un **ID** de requête est fourni pour la corrélation des événements pour une demande unique. En outre, un objet **AdaptiveMediaSourceDownloadStatistics** est fourni pour activer la journalisation des statistiques de téléchargement.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs" id="SnippetAMSDownloadCompleted":::

## <a name="gather-adaptive-streaming-telemetry-data-with-adaptivemediasourcediagnostics"></a>Collectez des données de télémétrie de diffusion en continu adaptative avec AdaptiveMediaSourceDiagnostics
Le **AdaptiveMediaSource** expose une propriété de [**diagnostic**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource) qui retourne un objet [**AdaptiveMediaSourceDiagnostics**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostics) . Utilisez cet objet pour vous inscrire à l’événement [**DiagnosticAvailable**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostics.DiagnosticAvailable) . Cet événement est destiné à être utilisé pour la collecte de données de télémétrie et ne doit pas être utilisé pour modifier le comportement de l’application au moment de l’exécution. Cet événement de diagnostic est déclenché pour de nombreuses raisons différentes. Vérifiez la propriété [**DiagnosticType**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnosticavailableeventargs.DiagnosticType) de l’objet [**AdaptiveMediaSourceDiagnosticAvailableEventArgs**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnosticavailableeventargs) passé dans l’événement pour déterminer la raison pour laquelle l’événement a été déclenché. Les raisons possibles incluent des erreurs lors de l’accès à la ressource demandée et des erreurs lors de l’analyse du fichier manifeste de diffusion en continu. Pour obtenir la liste des situations pouvant déclencher un événement de diagnostic, consultez [**AdaptiveMediaSourceDiagnosticType**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostictype). À l’instar des arguments pour d’autres événements de diffusion en continu adaptative, **AdaptiveMediaSourceDiagnosticAvailableEventArgs** fournit un **ID** de requête pour la corrélation des informations de demande entre différents événements.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs" id="SnippetAMSDiagnosticAvailable":::

## <a name="defer-binding-of-adaptive-streaming-content-for-items-in-a-playback-list-by-using-mediabinder"></a>Différer la liaison du contenu de diffusion en continu adaptative pour les éléments d’une liste de lecture à l’aide de MediaBinder
La classe [**MediaBinder**](/uwp/api/Windows.Media.Core.MediaBinder) vous permet de différer la liaison de contenu multimédia dans un [**MediaPlaybackList**](/uwp/api/Windows.Media.Playback.MediaPlaybackList). À compter de Windows 10, version 1703, vous pouvez fournir un [**AdaptiveMediaSource**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) comme contenu lié. Le processus de liaison différée d’une source de média adaptative est en grande partie identique à la liaison d’autres types de médias, qui est décrit dans [éléments multimédias, sélections et pistes](media-playback-with-mediasource.md). 

Créez une instance **MediaBinder** , définissez une chaîne de [**jeton**](/uwp/api/Windows.Media.Core.MediaBinder.Token) définie par l’application pour identifier le contenu à lier et inscrivez-vous pour l’événement de [**liaison**](/uwp/api/Windows.Media.Core.MediaBinder.Binding) . Créez un **MediaSource** à partir du **Binder** en appelant [**MediaSource. CreateFromMediaBinder**](/uwp/api/windows.media.core.mediasource.createfrommediabinder). Créez ensuite un **MediaPlaybackItem** à partir de **MediaSource** et ajoutez-le à la liste de lecture.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetInitMediaBinder":::

Dans le gestionnaire d’événements de **liaison** , utilisez la chaîne de jeton pour identifier le contenu à lier, puis créez la source de média adaptative en appelant l’une des surcharges de **[CreateFromStreamAsync](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.createfromstreamasync)** ou **[CreateFromUriAsync](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.createfromuriasync)**. Étant donné qu’il s’agit de méthodes asynchrones, vous devez d’abord appeler la méthode [**MediaBindingEventArgs. GetDeferral**](/uwp/api/windows.media.core.mediabindingeventargs.GetDeferral) pour indiquer au système d’attendre que l’opération se termine avant de continuer.  Définissez la source de média adaptative comme contenu lié en appelant **[SetAdaptiveMediaSource](/uwp/api/windows.media.core.mediabindingeventargs.setadaptivemediasource)**. Enfin, appelez [**Report. Complete**](/uwp/api/windows.foundation.deferral.Complete) une fois l’opération terminée pour demander au système de continuer.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetBinderBindingAMS":::

Si vous souhaitez inscrire des gestionnaires d’événements pour la source de média adaptative liée, vous pouvez le faire dans le gestionnaire pour l’événement [**CurrentItemChanged**](/uwp/api/windows.media.playback.mediaplaybacklist.CurrentItemChanged) de **MediaPlaybackList**. La propriété [**CurrentMediaPlaybackItemChangedEventArgs. newItem**](/uwp/api/windows.media.playback.currentmediaplaybackitemchangedeventargs.NewItem) contient le nouveau **MediaPlaybackItem** en cours de création dans la liste. Récupérez une instance du **AdaptiveMediaSource** représentant le nouvel élément en accédant à la propriété [**source**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem.Source) de **MediaPlaybackItem** , puis à la propriété [**AdaptiveMediaSource**](/uwp/api/windows.media.core.mediasource.AdaptiveMediaSource) de la source du média. Cette propriété a la valeur null si le nouvel élément de lecture n’est pas un **AdaptiveMediaSource**. vous devez donc tester la valeur null avant d’essayer d’enregistrer les gestionnaires pour les événements de l’objet.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetAMSBindingCurrentItemChanged":::

## <a name="related-topics"></a>Rubriques connexes
* [Lecture de contenu multimédia](media-playback.md)
* [Prise en charge des balises HLS](hls-tag-support.md) 
* [Prise en charge des profils Dash](dash-profile-support.md) 
* [Lire du contenu audio et vidéo avec MediaPlayer](play-audio-and-video-with-mediaplayer.md)
* [Lire du contenu multimédia en arrière-plan](background-audio.md) 
