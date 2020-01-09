---
ms.assetid: A1A0D99A-DCBF-4A14-80B9-7106BEF045EC
description: Vous pouvez utiliser les API Windows.Media.Transcoding pour transcoder des fichiers vidéo d’un format vers un autre.
title: Transcoder des fichiers multimédias
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 38aef2779908e173712bda0f35ca9e0651fb786b
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75683872"
---
# <a name="transcode-media-files"></a>Transcoder des fichiers multimédias



Vous pouvez utiliser les API [**Windows.Media.Transcoding**](https://docs.microsoft.com/uwp/api/Windows.Media.Transcoding) pour transcoder des fichiers vidéo d’un format vers un autre.

Le *transcodage* consiste à convertir un fichier multimédia numérique, tel qu’un fichier vidéo ou audio, d’un format vers un autre. Lors de ce processus, le fichier est généralement décodé puis réencodé. Par exemple, vous pouvez souhaiter convertir un fichier Windows Media au format MP4 pour qu’il puisse être lu sur un appareil mobile qui prend en charge le format MP4. Ou encore, vous pouvez souhaiter convertir un fichier vidéo à haute définition en fichier de résolution inférieure. Dans ce cas, le fichier réencodé peut utiliser le même codec que le fichier d’origine, mais il aura un profil d’encodage différent.

## <a name="set-up-your-project-for-transcoding"></a>Configurer votre projet pour le transcodage

Outre les espaces de noms référencés par le modèle de projet par défaut, vous devrez référencer ces espaces de noms pour transcoder les fichiers multimédias en utilisant le code dans cet article.

[!code-cs[Using](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetUsing)]

## <a name="select-source-and-destination-files"></a>Sélectionner les fichiers source et de destination

La façon dont votre application détermine les fichiers source et de destination pour le transcodage dépend de votre implémentation. Cet exemple utilise un élément [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) et un élément [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker) pour permettre à l’utilisateur de choisir un fichier source et de destination.

[!code-cs[TranscodeGetFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeGetFile)]

## <a name="create-a-media-encoding-profile"></a>Créer un profil d’encodage multimédia

Le profil d’encodage contient les paramètres qui déterminent le mode d’encodage du fichier de destination. C’est à ce stade du processus de transcodage d’un fichier que les options proposées sont les plus nombreuses.

La classe [**MediaEncodingProfile**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile) fournit des méthodes statiques pour la création de profils d’encodage prédéfinis :

### <a name="methods-for-creating-audio-only-encoding-profiles"></a>Méthodes de création de profils d’encodage exclusif audio

Méthode  |Profil  |
---------|---------|
[**CreateAlac**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createalac)     |Audio Apple Lossless Audio Codec (ALAC)         |
[**CreateFlac**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createflac)     |Audio Free Lossless Audio Codec (FLAC).         |
[**CreateM4a**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createm4a)     |Audio AAC (M4A)         |
[**CreateMp3**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp3)     |Audio MP3         |
[**CreateWav**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwav)     |Audio WAV         |
[**CreateWmv**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwmv)     |Audio Windows Media (WMA)         |

### <a name="methods-for-creating-audio--video-encoding-profiles"></a>Méthodes de création de profils d’encodage audio/vidéo

Méthode  |Profil  |
---------|---------|
[**CreateAvi**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createavi) |AVI |
[**CreateHevc**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createhevc) |Vidéo High Efficiency Video Coding (HEVC), également appelée vidéo H.265 |
[**CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4) |Vidéo MP4 (vidéo H.264 plus audio AAC) |
[**CreateWmv**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwmv) |Windows Media Video (WMV) |


Le code suivant crée un profil pour la vidéo MP4.

[!code-cs[TranscodeMediaProfile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeMediaProfile)]

La méthode statique [**CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4) permet de créer un profil d’encodage MP4. Le paramètre de cette méthode détermine la résolution cible de la vidéo. Dans ce cas, [**VideoEncodingQuality.hd720p**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.VideoEncodingQuality) signifie 1 280 x 720 pixels à 30 images par seconde. (« 720p » correspond à un balayage progressif de 720 lignes par image.) Les autres méthodes de création de profils prédéfinis suivent toutes ce modèle.

Une autre possibilité consiste à créer un profil qui correspond à un fichier multimédia existant à l’aide de la méthode [**MediaEncodingProfile.CreateFromFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createfromfileasync). Ou bien, si vous avez une idée précise des paramètres d’encodage que vous voulez utiliser, vous pouvez créer un objet [**MediaEncodingProfile**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile) et compléter les détails du profil.

## <a name="transcode-the-file"></a>Transcoder le fichier

Pour transcoder le fichier, créez un objet [**MediaTranscoder**](https://docs.microsoft.com/uwp/api/Windows.Media.Transcoding.MediaTranscoder) et appelez la méthode [**MediaTranscoder.PrepareFileTranscodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.transcoding.mediatranscoder.preparefiletranscodeasync). Transmettez le fichier source, le fichier de destination et le profil d’encodage. Appelez ensuite la méthode [**TranscodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.transcoding.preparetranscoderesult.transcodeasync) sur l’objet [**PrepareTranscodeResult**](https://docs.microsoft.com/uwp/api/Windows.Media.Transcoding.PrepareTranscodeResult) qui a été renvoyé par l’opération de transcodage asynchrone.

[!code-cs[TranscodeTranscodeFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeTranscodeFile)]

## <a name="respond-to-transcoding-progress"></a>Répondre à la progression du transcodage

Vous pouvez enregistrer des événements pour répondre en cas de modification de la progression de l’élément [**TranscodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.transcoding.preparetranscoderesult.transcodeasync) asynchrone. Ces événements font partie de l’infrastructure de programmation asynchrone pour les applications de plateforme Windows universelle (UWP) et ne sont pas propres à l’API de transcodage.

[!code-cs[TranscodeCallbacks](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeCallbacks)]


## <a name="encode-a-metadata-stream"></a>Encoder un flux de métadonnées
À compter de Windows 10, version 1803, vous pouvez inclure des métadonnées chronométrées lors du transcodage de fichiers multimédias. Contrairement aux exemples de transcodage vidéo ci-dessus, qui utilisent les méthodes de création de profil d’encodage multimédia intégrées, comme [**MediaEncodingProfile. CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4), vous devez créer manuellement le profil d’encodage des métadonnées pour prendre en charge le type de métadonnées que vous encodez.

La première étape de la création d’un profil d’encodage des métadonnées consiste à créer un objet [**TimedMetadataEncodingProperties**] qui décrit l’encodage des métadonnées à transcoder. La propriété SubType est un GUID qui spécifie le type des métadonnées. Les détails d’encodage pour chaque type de métadonnées sont propriétaires et ne sont pas fournis par Windows. Dans cet exemple, le GUID pour les métadonnées GoPro (GPRS) est utilisé. Ensuite, [**SetFormatUserData**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.timedmetadataencodingproperties.setformatuserdata) est appelé pour définir un blob binaire de données décrivant le format de flux qui est spécifique au format de métadonnées. Ensuite, un **TimedMetadataStreamDescriptor**(https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatastreamdescriptor) est créé à partir des attributions d’encodage, et une étiquette et un nom de suivi permettent à une application qui lit le flux endcoded d’identifier le flux de métadonnées et d’afficher éventuellement le nom du flux dans l’interface utilisateur. 
 
[!code-cs[GetStreamDescriptor](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetGetStreamDescriptor)]

Après avoir créé le **TimedMetadataStreamDescriptor**, vous pouvez créer un **MediaEncodingProfile** qui décrit la vidéo, l’audio et les métadonnées à encoder dans le fichier. Le **TimedMetadataStreamDescriptor** créé dans le dernier exemple est passé dans cet exemple de fonction d’assistance et est ajouté au **MediaEncodingProfile** en appelant [**SetTimedMetadataTracks**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.settimedmetadatatracks).

[!code-cs[GetMediaEncodingProfile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetGetMediaEncodingProfile)]
 

 




