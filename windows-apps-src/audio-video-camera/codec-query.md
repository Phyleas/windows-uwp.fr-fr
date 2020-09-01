---
ms.assetid: 0A360481-B649-4E90-9BC4-4449BA7445EF
description: Interrogation des encodeurs et décodeurs audio et vidéo installés sur un appareil.
title: Rechercher les codecs installés sur un appareil
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, codec, encodeur, décodeur, requête
ms.localizationpriority: medium
ms.openlocfilehash: ff1199651ceb9cf6ab3ef88cfdfa7dfbf25b67ee
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175733"
---
# <a name="query-for-codecs-installed-on-a-device"></a>Requête sur les codecs installés sur un appareil
La classe **[CodecQuery](/uwp/api/windows.media.core.codecquery)** vous permet d’interroger les codecs installés sur l’appareil actuel. La liste des codecs inclus avec Windows 10 pour les différentes familles d’appareils est répertoriée dans l’article [codecs pris en charge](supported-codecs.md), mais étant donné que les utilisateurs et les applications peuvent installer des codecs supplémentaires sur un appareil, vous pouvez interroger la prise en charge des codecs au moment de l’exécution pour déterminer quels codecs sont disponibles sur l’appareil actuel.

L’API CodecQuery est un membre de l’espace de noms **[Windows. Media. Core](/uwp/api/windows.media.core)** . vous devrez donc inclure cet espace de noms dans votre application.

L’API CodecQuery est un membre de l’espace de noms **[Windows. Media. Core](/uwp/api/windows.media.core)** . vous devrez donc inclure cet espace de noms dans votre application.

[!code-cs[CodecQueryUsing](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetCodecQueryUsing)]

Initialisez une nouvelle instance de la classe **CodecQuery** en appelant le constructeur.

[!code-cs[NewCodecQuery](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetNewCodecQuery)]

La méthode **[FindAllAsync](/uwp/api/windows.media.core.codecquery.findallasync)** retourne tous les codecs installés qui correspondent aux paramètres fournis. Ces paramètres incluent une valeur **[CodecKind](/uwp/api/windows.media.core.codeckind)** spécifiant si vous interrogez des codecs audio ou vidéo, ou les deux, une valeur **[CodecCategory](/uwp/api/windows.media.core.codeccategory)** spécifiant si vous interrogez des encodeurs ou des décodeurs, et une chaîne qui représente le sous-type d’encodage de média pour lequel vous interrogez, comme H. 264 vidéo ou audio MP3.

Spécifiez une chaîne vide ou null pour la valeur de sous-type pour retourner des codecs pour tous les sous-types. L’exemple suivant répertorie tous les encodeurs vidéo installés sur l’appareil.

[!code-cs[FindAllEncoders](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetFindAllEncoders)]

La chaîne de sous-type que vous transmettez dans **FindAllAsync** peut être une représentation sous forme de chaîne du GUID de sous-type défini par le système ou un code FourCC pour le sous-type. L’ensemble de GUID de sous-type de média pris en charge est répertorié dans les articles GUID de sous- [type audio](/windows/desktop/medfound/audio-subtype-guids) et GUID de sous-type de [vidéo](/windows/desktop/medfound/video-subtype-guids), mais la classe **[CodecSubtypes](/uwp/api/windows.media.core.codecsubtypes)** fournit des propriétés qui retournent les valeurs GUID pour chaque sous-type pris en charge. Pour plus d’informations sur les Codes FOURCC, consultez [codes FourCC](/windows/desktop/DirectShow/fourcc-codes) 

L’exemple suivant spécifie le code FOURCC « H264 – » pour déterminer si un décodeur vidéo H. 264 est installé sur l’appareil. Vous pouvez exécuter cette requête avant d’essayer de lire le contenu vidéo H. 264. Vous pouvez également gérer les codecs non pris en charge au moment de la lecture. Pour plus d’informations, consultez [gérer les codecs non pris en charge et les erreurs inconnues lors de l’ouverture d’éléments multimédias](./media-playback-with-mediasource.md#handle-unsupported-codecs-and-unknown-errors-when-opening-media-items).

[!code-cs[IsH264Supported](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetIsH264Supported)]

Les exemples de requêtes suivants permettent de déterminer si un encodeur audio FLAC est installé sur l’appareil en cours et, si tel est le cas, si un **[MediaEncodingProfile](/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile)** est créé pour le sous-type qui peut être utilisé pour la capture de l’audio dans un fichier ou pour le transcodage d’un fichier audio d’un autre format en fichier audio FLAC.

[!code-cs[IsFLACSupported](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetIsFLACSupported)]

## <a name="related-topics"></a>Rubriques connexes

* [Lecture de contenu multimédia](media-playback.md)
* [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Transcoder des fichiers multimédias](transcode-media-files.md)
* [Codecs pris en charge](supported-codecs.md)
 

 