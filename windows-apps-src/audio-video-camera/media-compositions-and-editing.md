---
ms.assetid: C4DB495D-1F91-40EF-A55C-5CABBF3269A2
description: Les API de l’espace de noms Windows.Media.Editing permettent de développer rapidement des applications permettant aux utilisateurs de créer des compositions multimédias à partir de fichiers sources audio et vidéo.
title: Compositions multimédias et modification
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 91b0418555e4d46c15bd43816ec6b01ebbd27d8b
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363872"
---
# <a name="media-compositions-and-editing"></a>Compositions multimédias et modification



Cet article explique comment utiliser les API de l’espace de noms [**Windows. Media. Editing**](/uwp/api/Windows.Media.Editing) pour développer rapidement des applications qui permettent aux utilisateurs de créer des compositions de média à partir de fichiers sources audio et vidéo. Les fonctionnalités de l’infrastructure incluent la possibilité d’ajouter simultanément plusieurs clips vidéo, d’ajouter des superpositions d’images et de vidéos, d’ajouter du son en arrière-plan et d’appliquer des effets audio et vidéos, par programme. Une fois créées, les compositions multimédias peuvent être rendues dans un fichier multimédia plat pour lecture ou partage, mais il est également possible de les sérialiser et désérialiser à partir du disque en permettant à l’utilisateur de charger et de modifier les compositions précédemment créées. Toutes ces fonctionnalités sont proposées dans l’interface simple d’utilisation Windows Runtime, qui réduit considérablement la quantité de code requis et sa complexité pour effectuer ces tâches, en comparaison avec l’APi [Microsoft Media Foundation](/windows/desktop/medfound/microsoft-media-foundation-sdk) de bas niveau.

## <a name="create-a-new-media-composition"></a>Créer une composition multimédia

La classe [**MediaComposition**](/uwp/api/Windows.Media.Editing.MediaComposition) est le conteneur de tous les clips multimédias qui composent la composition et est responsable du rendu de la composition finale, du chargement et de l’enregistrement des compositions sur le disque et de la fourniture d’un flux d’aperçu de la composition afin que l’utilisateur puisse l’afficher dans l’interface utilisateur. Pour utiliser **MediaComposition** dans votre application, vous devez inclure l’espace de noms [**Windows.Media.Editing**](/uwp/api/Windows.Media.Editing), ainsi que l’espace de noms [**Windows.Media.Core**](/uwp/api/Windows.Media.Core) qui fournit les API associées dont vous aurez besoin.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaEditing/cs/MainPage.xaml.cs" id="SnippetNamespace1":::


L’objet **MediaComposition** est accessible à partir de plusieurs points dans votre code. Vous allez généralement déclarer une variable de membre qui permettra de le stocker.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaEditing/cs/MainPage.xaml.cs" id="SnippetDeclareMediaComposition":::

Le constructeur de **MediaComposition** ne prend pas d’arguments.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaEditing/cs/MainPage.xaml.cs" id="SnippetMediaCompositionConstructor":::

## <a name="add-media-clips-to-a-composition"></a>Ajouter des clips multimédias à une composition

Les compositions multimédia contiennent généralement un ou plusieurs clips vidéo. Vous pouvez utiliser un [**FileOpenPicker**](/uwp/schemas/appxpackage/appxmanifestschema/element-fileopenpicker) pour permettre à l’utilisateur de sélectionner un fichier vidéo. Une fois le fichier sélectionné, créez un nouvel objet [**MediaClip**](/uwp/api/Windows.Media.Editing.MediaClip) pour contenir le clip vidéo en appelant [**MediaClip. CreateFromFileAsync**](/uwp/api/windows.media.editing.mediaclip.createfromfileasync). Ajoutez ensuite le clip à la liste [**Clips**](/uwp/api/windows.media.editing.mediacomposition.clips) de l’objet **MediaComposition**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaEditing/cs/MainPage.xaml.cs" id="SnippetPickFileAndAddClip":::

-   Les clips multimédias s’affichent dans l’élément **MediaComposition** dans l’ordre dans lequel ils apparaissent dans la liste [**Clips**](/uwp/api/windows.media.editing.mediacomposition.clips).

-   Un élément **MediaClip** ne peut être inclus qu’une seule fois dans une composition. L’ajout d’un élément **MediaClip** déjà utilisé par la composition se traduit par une erreur. Pour réutiliser un clip vidéo plusieurs fois dans une composition, appelez [**Clone**](/uwp/api/windows.media.editing.mediaclip.clone) pour créer de nouveaux objets **MediaClip**, qui peuvent ensuite être ajoutés à la composition.

-   Les applications Windows universelles ne sont pas autorisées à accéder à l’ensemble du système de fichiers. La propriété [**FutureAccessList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) de la classe [**StorageApplicationPermissions**](/uwp/api/Windows.Storage.AccessCache.StorageApplicationPermissions) permet à votre application de stocker un enregistrement d’un fichier qui a été sélectionné par l’utilisateur afin que vous puissiez conserver les autorisations d’accès au fichier. La propriété **FutureAccessList** comporte 1000 entrées maximum. Votre application a donc besoin de gérer la liste pour s’assurer qu’elle n’est pas complète. Cela est particulièrement important si vous envisagez de prendre en charge le chargement et la modification de compositions déjà créées.

-   Un élément **MediaComposition** prend en charge les clips vidéo au format MP4.

-   Si un fichier vidéo contient plusieurs pistes audio incorporées, vous pouvez sélectionner la piste audio utilisée dans la composition en définissant la propriété [**SelectedEmbeddedAudioTrackIndex**](/uwp/api/windows.media.editing.mediaclip.selectedembeddedaudiotrackindex) .

-   Créez un élément **MediaClip** avec une seule couleur de remplissage de la trame tout entière en appelant [**CreateFromColor**](/uwp/api/windows.media.editing.mediaclip.createfromcolor) et en spécifiant une couleur et une durée pour le clip.

-   Créez un élément **MediaClip** à partir d’un fichier image en appelant [**CreateFromImageFileAsync**](/uwp/api/windows.media.editing.mediaclip.createfromimagefileasync) et en spécifiant un fichier image et une durée pour le clip.

-   Créez un élément **MediaClip** à partir d’une [**IDirect3DSurface**](/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface) en appelant [**CreateFromSurface**](/uwp/api/windows.media.editing.mediaclip.createfromsurface) et en spécifiant une surface et une durée à partir du clip.

## <a name="preview-the-composition-in-a-mediaelement"></a>Afficher un aperçu de la composition dans un objet MediaElement

Pour permettre à l’utilisateur d’afficher la composition multimédia, ajoutez une classe [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) au fichier XAML qui définit votre interface utilisateur.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaEditing/cs/MainPage.xaml" id="SnippetMediaElement":::

Déclarez une variable de membre de type [**MediaStreamSource**](/uwp/api/Windows.Media.Core.MediaStreamSource).


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaEditing/cs/MainPage.xaml.cs" id="SnippetDeclareMediaStreamSource":::

Appelez la méthode [**GeneratePreviewMediaStreamSource**](/uwp/api/windows.media.editing.mediacomposition.generatepreviewmediastreamsource) de l’objet **MediaComposition** pour créer un élément **MediaStreamSource**. Créez un objet [**MediaSource**](/uwp/api/Windows.Media.Core.MediaSource) en appelant la méthode de fabrique [**CreateFromMediaStreamSource**](/uwp/api/windows.media.core.mediasource.createfrommediastreamsource) et affectez-le à la propriété [**Source**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) de **MediaPlayerElement**. La composition peut désormais être affichée dans l’interface utilisateur.


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaEditing/cs/MainPage.xaml.cs" id="SnippetUpdateMediaElementSource":::

-   L’élément **MediaComposition** doit contenir au moins un clip multimédia avant d’appeler [**GeneratePreviewMediaStreamSource**](/uwp/api/windows.media.editing.mediacomposition.generatepreviewmediastreamsource), sans quoi l’objet renvoyé sera null.

-   La chronologie de **MediaElement** n’est pas automatiquement mise à jour de manière à refléter les changements relatifs à la composition. Il est recommandé à la fois d’appeler **GeneratePreviewMediaStreamSource** et de définir la propriété **Source de ** **MediaPlayerElement** chaque fois que vous apportez des modifications à la composition et que vous voulez mettre à jour l’interface utilisateur.

Il est recommandé de définir l’objet **MediaStreamSource** et la propriété [**Source**](/uwp/api/windows.ui.xaml.controls.mediaelement.source) de **MediaPlayerElement** sur null lorsque l’utilisateur quitte la page afin de libérer les ressources associées.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaEditing/cs/MainPage.xaml.cs" id="SnippetOnNavigatedFrom":::

## <a name="render-the-composition-to-a-video-file"></a>Restituer la composition dans un fichier vidéo

Pour afficher une composition de média dans un fichier vidéo plat afin qu’elle puisse être partagée et affichée sur d’autres appareils, vous devez utiliser des API de l’espace de noms [**Windows. Media. transcodage**](/uwp/api/Windows.Media.Transcoding) . Pour mettre à jour l’interface utilisateur en fonction de la progression de l’opération asynchrone, vous aurez également besoin des API de l’espace de noms [**Windows.UI.Core**](/uwp/api/Windows.UI.Core).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaEditing/cs/MainPage.xaml.cs" id="SnippetNamespace2":::

Après avoir autorisé l’utilisateur à sélectionner un fichier de sortie à l’aide d’une classe [**FileSavePicker**](/uwp/api/Windows.Storage.Pickers.FileSavePicker), restituez la composition dans le fichier sélectionné en appelant la méthode [**RenderToFileAsync**](/uwp/api/windows.media.editing.mediacomposition.rendertofileasync) de l’objet **MediaComposition**. Le reste du code de l’exemple suivant suit simplement le modèle de gestion d’un [**AsyncOperationWithProgress**](/previous-versions/br205807(v=vs.85)).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaEditing/cs/MainPage.xaml.cs" id="SnippetRenderCompositionToFile":::

-   Le [**MediaTrimmingPreference**](/uwp/api/Windows.Media.Editing.MediaTrimmingPreference) vous permet de hiérarchiser la vitesse de l’opération de transcodage par rapport à la précision de la suppression des clips multimédias adjacents. **Fast** accélère le transcodage par un découpage moins précis et **Precise** le ralentit par une augmentation de la précision du découpage.

## <a name="trim-a-video-clip"></a>Découper un clip vidéo

Ajustez la durée d’un clip vidéo dans une composition en définissant la propriété [**TrimTimeFromStart**](/uwp/api/windows.media.editing.mediaclip.trimtimefromstart) des objets [**MediaClip**](/uwp/api/Windows.Media.Editing.MediaClip) , la propriété [**TrimTimeFromEnd**](/uwp/api/windows.media.editing.mediaclip.trimtimefromend) , ou les deux.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaEditing/cs/MainPage.xaml.cs" id="SnippetTrimClipBeforeCurrentPosition":::

-   Vous pouvez utiliser l’interface utilisateur que vous souhaitez pour permettre à l’utilisateur de spécifier les valeurs de découpage de début et de fin. L’exemple ci-dessus utilise la propriété [**Position**](/uwp/api/windows.media.playback.mediaplaybacksession.position) de l’objet [**MediaPlaybackSession**](/uwp/api/Windows.Media.Playback.MediaPlaybackSession) associé à l’élément **MediaPlayerElement**, pour commencer par déterminer quel **MediaClip** est lu au niveau de la position actuelle de la composition en vérifiant les propriétés [**StartTimeInComposition**](/uwp/api/windows.media.editing.mediaclip.starttimeincomposition) et [**EndTimeInComposition**](/uwp/api/windows.media.editing.mediaclip.endtimeincomposition). Les propriétés **Position** et **StartTimeInComposition** servent à nouveau pour calculer la durée à découper depuis le début du clip. La méthode **FirstOrDefault** est une méthode d’extension à partir de l’espace de noms **System.Linq** qui simplifie le code permettant de sélectionner les éléments d’une liste.
-   La propriété [**OriginalDuration**](/uwp/api/windows.media.editing.mediaclip.originalduration) de l’objet **MediaClip** vous permet de connaître la durée du clip multimédia sans découpage.
-   La propriété [**TrimmedDuration**](/uwp/api/windows.media.editing.mediaclip.trimmedduration) vous permet de connaître la durée du clip multimédia après découpage.
-   La spécification d’une valeur de découpage supérieure à la durée initiale du clip ne génère pas d’erreur. Toutefois, si une composition ne contient qu’un seul clip, et que celui-ci a subi un découpage complet le ramenant à une durée égale à zéro dû à la spécification d’une valeur de découpage importante, un appel ultérieur à [**GeneratePreviewMediaStreamSource**](/uwp/api/windows.media.editing.mediacomposition.generatepreviewmediastreamsource) renvoie la valeur null, comme si la composition ne comportait aucun clip.

## <a name="add-a-background-audio-track-to-a-composition"></a>Ajouter une piste audio en arrière-plan à une composition

Pour ajouter une piste d’arrière-plan à une composition, chargez un fichier audio, puis créez un objet [**BackgroundAudioTrack**](/uwp/api/Windows.Media.Editing.BackgroundAudioTrack) en appelant la méthode de fabrique [**BackgroundAudioTrack. CreateFromFileAsync**](/uwp/api/windows.media.editing.backgroundaudiotrack.createfromfileasync). Ajoutez ensuite l’élément **BackgroundAudioTrack** à la propriété [**BackgroundAudioTracks**](/uwp/api/windows.media.editing.mediacomposition.backgroundaudiotracks) de la composition.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaEditing/cs/MainPage.xaml.cs" id="SnippetAddBackgroundAudioTrack":::

-   Un élément **MediaComposition** prend en charge les pistes audio en arrière-plan aux formats suivants : MP3, WAV, FLAC.

-   Une piste audio en arrière-plan

-   Comme pour les fichiers vidéo, vous devez utiliser la classe [**StorageApplicationPermissions**](/uwp/api/Windows.Storage.AccessCache.StorageApplicationPermissions) pour conserver l’accès aux fichiers de la composition.

-   Comme pour **MediaClip**, un élément **BackgroundAudioTrack** ne peut être inclus qu’une seule fois dans une composition. L’ajout d’un élément **BackgroundAudioTrack** déjà utilisé par la composition se traduit par une erreur. Pour réutiliser une piste audio plusieurs fois dans une composition, appelez [**Clone**](/uwp/api/windows.media.editing.mediaclip.clone) pour créer de nouveaux objets **MediaClip**, qui peuvent ensuite être ajoutés à la composition.

-   Par défaut, la lecture des pistes audio en arrière-plan commence au début de la composition. En présence de plusieurs pistes en arrière-plan, la lecture de toutes les pistes commence au début de la composition. Pour que la lecture d’une piste audio d’arrière-plan commence à un autre moment, définissez la propriété [**delay**](/uwp/api/windows.media.editing.backgroundaudiotrack.delay) sur le décalage de temps souhaité.

## <a name="add-an-overlay-to-a-composition"></a>Ajouter une superposition à une composition

Les superpositions permettent d’empiler plusieurs couches de vidéos les unes sur les autres au sein d’une composition. Une composition peut contenir plusieurs couches de superposition, qui peuvent elles-mêmes inclure plusieurs superpositions. Créez un objet [**MediaOverlay**](/uwp/api/Windows.Media.Editing.MediaOverlay) en transmettant un élément **MediaClip** à son constructeur. Définissez la position et l’opacité de la superposition, puis créez une classe [**MediaOverlayLayer**](/uwp/api/Windows.Media.Editing.MediaOverlayLayer) et ajoutez l’élément **MediaOverlay** à sa liste [**Overlays**](/windows/desktop/api/dxgi1_3/nf-dxgi1_3-idxgioutput2-supportsoverlays). Ajoutez enfin l’élément **MediaOverlayLayer** à la liste [**OverlayLayers**](/uwp/api/windows.media.editing.mediacomposition.overlaylayers) de la composition.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaEditing/cs/MainPage.xaml.cs" id="SnippetAddOverlay":::

-   Les superpositions au sein d’une couche sont classées selon l’ordre-z en fonction de leur ordre dans la liste **Overlays** de la couche qui les contient. Les indices supérieurs de la liste sont restitués au-dessus des indices inférieurs. Il en va de même pour les couches de superposition d’une composition. Une couche dont l’indice est supérieur dans la liste **OverlayLayers** de la composition sera restituée au-dessus des couches à indice inférieur.

-   Dans la mesure où les superpositions sont empilées les unes sur les autres au lieu d’être lues séquentiellement, toutes les superpositions commencent par défaut la lecture au début de la composition. Pour faire en sorte qu’une superposition démarre à un autre moment, définissez la propriété [**delay**](/uwp/api/windows.media.editing.mediaoverlay.delay) sur le décalage de temps souhaité.

## <a name="add-effects-to-a-media-clip"></a>Ajouter des effets à un clip multimédia

Chaque **MediaClip** d’une composition possède une liste d’effets audio et vidéo auxquels plusieurs effets peuvent être ajoutés. Les effets doivent implémenter respectivement [**IAudioEffectDefinition**](/uwp/api/Windows.Media.Effects.IAudioEffectDefinition) et [**IVideoEffectDefinition**](/uwp/api/Windows.Media.Effects.IVideoEffectDefinition) . L’exemple suivant utilise la position actuelle de l’élément **MediaPlayerElement** de manière à choisir l’élément **MediaClip** affiché actuellement, puis crée une nouvelle instance de la classe [**VideoStabilizationEffectDefinition**](/uwp/api/Windows.Media.Core.VideoStabilizationEffectDefinition) et l’ajoute à la liste [**VideoEffectDefinitions**](/uwp/api/windows.media.editing.mediaclip.videoeffectdefinitions) du clip multimédia.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaEditing/cs/MainPage.xaml.cs" id="SnippetAddVideoEffect":::

## <a name="save-a-composition-to-a-file"></a>Enregistrer une composition dans un fichier

Les compositions multimédia peuvent être sérialisées dans un fichier pour être modifiées ultérieurement. Sélectionnez un fichier de sortie, puis appelez la méthode [**MediaComposition**](/uwp/api/Windows.Media.Editing.MediaComposition) [**SaveAsync**](/uwp/api/windows.media.editing.mediacomposition.saveasync) pour enregistrer la composition.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaEditing/cs/MainPage.xaml.cs" id="SnippetSaveComposition":::

## <a name="load-a-composition-from-a-file"></a>Charger une composition à partir d’un fichier

Les compositions multimédia peuvent être désérialisées à partir d’un fichier pour permettre à l’utilisateur d’afficher et de modifier la composition. Sélectionnez un fichier de composition, puis appelez la méthode [**MediaComposition**](/uwp/api/Windows.Media.Editing.MediaComposition) [**LoadAsync**](/uwp/api/windows.media.editing.mediacomposition.loadasync) pour charger la composition.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaEditing/cs/MainPage.xaml.cs" id="SnippetOpenComposition":::

-   Si un fichier multimédia de la composition ne se trouve pas dans un emplacement auquel votre application a accès et ne se trouve pas dans la propriété [**FutureAccessList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) de la classe [**StorageApplicationPermissions**](/uwp/api/Windows.Storage.AccessCache.StorageApplicationPermissions) pour votre application, une erreur sera générée lors du chargement de la composition.

 

 
