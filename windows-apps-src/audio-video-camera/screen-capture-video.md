---
title: Capture d’écran sur vidéo
description: Cet article explique comment encoder des frames capturés à partir de l’écran avec les API Windows. Graphics. capture dans un fichier vidéo.
ms.date: 07/28/2020
ms.topic: article
dev_langs:
- csharp
keywords: Windows 10, UWP, capture d’écran, vidéo
ms.localizationpriority: medium
ms.openlocfilehash: ae1eb68e480b4c9b4b4fc88452a68f39f8461a79
ms.sourcegitcommit: 14c0b1ea2447a81ddf31982b40e19a74ecc6d59e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89310083"
---
# <a name="screen-capture-to-video"></a>Capture d’écran sur vidéo

Cet article explique comment encoder des frames capturés à partir de l’écran avec les API Windows. Graphics. capture dans un fichier vidéo. Pour plus d’informations sur la capture d’écran d’images fixes, consultez [screeen capture](screen-capture-video). 

## <a name="overview-of-the-video-capture-process"></a>Vue d’ensemble du processus de capture vidéo
Cet article fournit une procédure pas à pas d’un exemple d’application qui enregistre le contenu d’une fenêtre dans un fichier vidéo. Bien qu’il semble qu’il y ait un grand nombre de code requis pour implémenter ce scénario, la structure de haut niveau d’une application d’enregistreur d’écran est relativement simple. Le processus de capture d’écran utilise trois fonctionnalités UWP principales :

- Les API [Windows. GraphicsCapture](/uwp/api/windows.graphics.capture) effectuent le travail qui consiste à saisir les pixels de l’écran. La classe [GraphicsCaptureItem](/uwp/api/windows.graphics.capture.graphicscaptureitem) représente la fenêtre ou l’affichage en cours de capture. [GraphicsCaptureSession](/uwp/api/windows.graphics.capture.graphicscapturesession) est utilisé pour démarrer et arrêter l’opération de capture. La classe [Direct3D11CaptureFramePool](/uwp/api/windows.graphics.capture.direct3d11captureframepool) gère une mémoire tampon des frames dans lesquels le contenu de l’écran est copié.
- La classe [source](/uwp/api/windows.media.core.mediastreamsource) reçoit les frames capturés et génère un flux vidéo.
- La classe [MediaTranscoder](/uwp/api/windows.media.transcoding.mediatranscoder) reçoit le flux produit par le **source** et l’encode dans un fichier vidéo.

L’exemple de code présenté dans cet article peut être classé en plusieurs tâches différentes :

- **Initialisation** -cela comprend la configuration des classes UWP décrites ci-dessus, l’initialisation des interfaces de périphériques graphiques, la sélection d’une fenêtre à capturer et la configuration des paramètres d’encodage tels que la résolution et la fréquence d’images.
- **Gestionnaires d’événements et Threading** : le pilote principal de la boucle de capture principale est le **source** qui demande des frames périodiquement via l’événement [SampleRequested](/uwp/api/windows.media.core.mediastreamsource.samplerequested) . Cet exemple utilise des événements pour coordonner les demandes de nouveaux frames entre les différents composants de l’exemple. La synchronisation est importante pour permettre de capturer et d’encoder des frames simultanément.
- La **copie** des frames-frames est copié à partir de la mémoire tampon de frame de capture dans une surface Direct3D distincte qui peut être transmise à **source** afin que la ressource ne soit pas remplacée lors de son encodage. Les API Direct3D sont utilisées pour effectuer cette opération de copie rapidement. 

### <a name="about-the-direct3d-apis"></a>À propos des API Direct3D
Comme indiqué ci-dessus, la copie de chaque trame capturée est probablement la partie la plus complexe de l’implémentation présentée dans cet article. À un niveau bas, cette opération est effectuée à l’aide de Direct3D. Pour cet exemple, nous utilisons la bibliothèque [SharpDX](http://sharpdx.org/) pour effectuer les opérations Direct3D à partir de C#. Cette bibliothèque n’est plus prise en charge officiellement, mais elle a été choisie, car ses performances pour les opérations de copie de bas niveau sont bien adaptées à ce scénario. Nous avons essayé de conserver les opérations Direct3D aussi discrètes que possible pour vous permettre de remplacer plus facilement votre propre code ou d’autres bibliothèques pour ces tâches.

## <a name="setting-up-your-project"></a>Configuration de votre projet
L’exemple de code de cette procédure pas à pas a été créé à l’aide du modèle de projet **application vide (Universal Windows)** C# dans Visual Studio 2019. Pour pouvoir utiliser les API **Windows. Graphics. capture** dans votre application, vous devez inclure la fonctionnalité de **capture de graphiques** dans le fichier Package. appxmanifest de votre projet. Cet exemple enregistre les fichiers vidéo générés dans la bibliothèque vidéos de l’appareil. Pour accéder à ce dossier, vous devez inclure la fonctionnalité de la **bibliothèque de vidéos** .

Pour installer le package NuGet SharpDX, dans Visual Studio, sélectionnez **gérer les packages NuGet**. Dans l’onglet Parcourir, recherchez le package « SharpDX. Direct3D11 », puis cliquez sur **installer**.

Notez que, afin de réduire la taille des listes de code dans cet article, le code de la procédure pas à pas ci-dessous omet les références d’espaces de noms explicites et la déclaration des variables membres de la classe MainPage nommées avec un trait de soulignement de début, « _ ».

## <a name="setup-for-encoding"></a>Configuration de l’encodage

La méthode **SetupEncoding** décrite dans cette section Initialise certains des objets principaux qui seront utilisés pour capturer et encoder des images vidéo et définit les paramètres d’encodage de la vidéo capturée. Cette méthode peut être appelée par programmation ou en réponse à une interaction utilisateur comme un clic sur un bouton. La liste de code pour **SetupEncoding** est illustrée ci-dessous après les descriptions des étapes d’initialisation.

- **Vérifiez la prise en charge de capture.** Avant de commencer le processus de capture, vous devez appeler [GraphicsCaptureSession. IsSupported](/uwp/api/windows.graphics.capture.graphicscapturesession.issupported) pour vous assurer que la fonctionnalité de capture d’écran est prise en charge sur l’appareil actuel.

- **Initialisez les interfaces Direct3D.** Cet exemple utilise Direct3D pour copier les pixels capturés à partir de l’écran dans une texture encodée sous la forme d’une image vidéo. Les méthodes d’assistance utilisées pour initialiser les interfaces Direct3D, **CreateD3DDevice** et **CreateSharpDXDevice**, sont présentées plus loin dans cet article.

- **Initialisez un GraphicsCaptureItem.** Un [GraphicsCaptureItem](/uwp/api/windows.graphics.capture.graphicscaptureitem) représente un élément à l’écran qui va être capturé, qu’il s’agisse d’une fenêtre ou de l’écran entier. Permet à l’utilisateur de choisir un élément à capturer en créant un [GraphicsCapturePicker](/uwp/api/windows.graphics.capture.graphicscapturepicker) et en appelant [PickSingleItemAsync](/uwp/api/windows.graphics.capture.graphicscapturepicker.picksingleitemasync).

- **Créer une texture de composition.** Créez une ressource de texture et une vue de cible de rendu associée qui seront utilisées pour copier chaque image vidéo. Impossible de créer cette texture tant que le **GraphicsCaptureItem** n’a pas été créé et que nous connaissons ses dimensions. Consultez la description du **WaitForNewFrame** pour voir comment cette texture de composition est utilisée. La méthode d’assistance pour créer cette texture est également indiquée plus loin dans cet article.

- **Créez un MediaEncodingProfile et VideoStreamDescriptor.** Une instance de la classe [source](/uwp/api/windows.media.core.mediastreamsource) prend les images capturées à partir de l’écran et les encode dans un flux vidéo. Le flux vidéo est ensuite transcodé dans un fichier vidéo par la classe [MediaTranscoder](/uwp/api/windows.media.transcoding.mediatranscoder) . Un [VideoStreamDecriptor](/uwp/api/windows.media.core.videostreamdescriptor) fournit des paramètres d’encodage, tels que la résolution et la fréquence d’images, pour le **source**. Les paramètres d’encodage de fichier vidéo pour **MediaTranscoder** sont spécifiés avec un [MediaEncodingProfile](/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile). Notez que la taille utilisée pour l’encodage vidéo ne doit pas nécessairement être identique à la taille de la fenêtre capturée, mais pour que cet exemple reste simple, les paramètres d’encodage sont codés en dur pour utiliser les dimensions réelles de l’élément de capture.

- **Créez les objets source et MediaTranscoder.** Comme indiqué ci-dessus, l’objet **source** encode des frames individuels dans un flux vidéo. Appelez le constructeur pour cette classe, en passant le **MediaEncodingProfile** créé à l’étape précédente. Définissez la durée de la mémoire tampon sur zéro et enregistrez les gestionnaires des événements de [démarrage](uwp/api/windows.media.core.mediastreamsource.starting) et [SampleRequested](/uwp/api/windows.media.core.mediastreamsource.samplerequested) , qui seront présentés plus loin dans cet article. Ensuite, construisez une nouvelle instance de la classe **MediaTranscoder** et activez l’accélération matérielle.

- **Créer un fichier de sortie** La dernière étape de cette méthode consiste à créer un fichier dans lequel la vidéo sera transcodée. Pour cet exemple, nous allons simplement créer un fichier portant un nom unique dans le dossier de bibliothèque vidéos sur l’appareil. Notez que pour accéder à ce dossier, votre application doit spécifier la fonctionnalité « bibliothèque de vidéos » dans le manifeste de l’application. Une fois le fichier créé, ouvrez-le en lecture et en écriture, puis transmettez le flux résultant dans la méthode **EncodeAsync** qui sera affichée ensuite.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_SetupEncoding":::

## <a name="start-encoding"></a>Démarrer l’encodage
Maintenant que les objets principaux ont été initialisés, la méthode **EncodeAsync** est implémentée pour déclencher l’opération de capture. Cette méthode vérifie d’abord que nous n’avons pas encore enregistré et, si ce n’est pas le cas, elle appelle la méthode d’assistance **StartCapture** pour commencer à capturer des frames à partir de l’écran. Cette méthode est présentée plus loin dans cet article. Ensuite, [PrepareMediaStreamSourceTranscodeAsync](/uwp/api/windows.media.transcoding.mediatranscoder.preparemediastreamsourcetranscodeasync) est appelé pour que le **MediaTranscoder** soit prêt à Transcoder le flux vidéo produit par l’objet **source** dans le flux de fichier de sortie, à l’aide du profil d’encodage que nous avons créé dans la section précédente. Une fois que le transcodeur a été préparé, appelez [TranscodeAsync](/uwp/api/windows.media.transcoding.preparetranscoderesult.transcodeasync) pour démarrer le transcodage. Pour plus d’informations sur l’utilisation de **MediaTranscoder**, consultez [transcodage de fichiers multimédias](/windows/uwp/audio-video-camera/transcode-media-files).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_EncodeAsync":::

## <a name="handle-mediastreamsource-events"></a>Gérer les événements source
L’objet **source** accepte les trames capturées à partir de l’écran et les transforme en un flux vidéo qui peut être enregistré dans un fichier à l’aide de **MediaTranscoder**. Nous passons les frames au **source** via les gestionnaires des événements de l’objet.

L’événement [SampleRequested](/uwp/api/windows.media.core.mediastreamsource.samplerequested) est déclenché lorsque le **source** est prêt pour une nouvelle image vidéo. Après avoir vérifié que nous enregistrons actuellement, la méthode d’assistance **WaitForNewFrame** est appelée pour obtenir un nouveau Frame capturé à partir de l’écran. Cette méthode, présentée plus loin dans cet article, retourne un objet [ID3D11Surface](/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface) contenant le frame capturé. Pour cet exemple, nous encapsulons l’interface **IDirect3DSurface** dans une classe d’assistance qui stocke également l’heure système à laquelle le frame a été capturé. Le frame et l’heure système sont transmis à la méthode de fabrique [MediaStreamSample. CreateFromDirect3D11Surface](/uwp/api/windows.media.core.mediastreamsample.createfromdirect3d11surface) et le [MediaStreamSample](/uwp/api/windows.media.core.mediastreamsample) résultant est défini sur la propriété [MediaStreamSourceSampleRequest. Sample](MediaStreamSourceSampleRequest.Sample) du [MediaStreamSourceSampleRequestedEventArgs](/uwp/api/windows.media.core.mediastreamsourcesamplerequestedeventargs). Voici comment le frame capturé est fourni à **source**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_OnMediaStreamSourceSampleRequested":::

Dans le gestionnaire de l’événement de [départ](/uwp/api/windows.media.core.mediastreamsource.starting) , nous appelons **WaitForNewFrame**, mais passons uniquement l’heure système à laquelle le frame a été capturé dans la méthode [MediaStreamSourceStartingRequest. SetActualStartPosition](/uwp/api/windows.media.core.mediastreamsourcestartingrequest.setactualstartposition) , que le **source** utilise pour encoder correctement le minutage des frames suivants.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_OnMediaStreamSourceStarting":::

## <a name="start-capturing"></a>Démarrer la capture
La méthode **StartCapture** indiquée dans cette étape est appelée à partir de la méthode d’assistance **EncodeAsync** indiquée dans une étape précédente. Tout d’abord, cette méthode initialise un ensemble d’objets d’événement utilisés pour contrôler le déroulement de l’opération de capture.

- **_multithread** est une classe d’assistance encapsulant l’objet **multithread** de la bibliothèque SharpDX qui sera utilisée pour s’assurer qu’aucun autre thread n’accède à la texture SharpDX pendant sa copie.
- **_frameEvent** est utilisé pour signaler qu’une nouvelle image a été capturée et peut être passée à **source**
- **_closedEvent** signale que l’enregistrement s’est arrêté et que nous ne devrions pas attendre de nouveaux frames.

L’événement Frame et l’événement Closed sont ajoutés à un tableau afin que nous puissions attendre l’un d’eux dans la boucle de capture.

Le reste de la méthode **StartCapture** configure les API Windows. Graphics. capture qui effectuent la capture d’écran réelle. Tout d’abord, un événement est enregistré pour l’événement **CaptureItem. Closed** . Ensuite, un [Direct3D11CaptureFramePool](/uwp/api/windows.graphics.capture.direct3d11captureframepool) est créé, ce qui permet la mise en mémoire tampon de plusieurs frames capturés à la fois. La méthode [CreateFreeThreaded](/uwp/api/windows.graphics.capture.direct3d11captureframepool.createfreethreaded) est utilisée pour créer le pool de frames afin que l’événement [FrameArrived](/uwp/api/windows.graphics.capture.direct3d11captureframepool.framearrived) soit appelé sur le thread de travail du pool, plutôt que sur le thread principal de l’application. Ensuite, un gestionnaire est inscrit pour l’événement **FrameArrived** . Enfin, un [GraphicsCaptureSession](/uwp/api/windows.graphics.capture.graphicscapturesession) est créé pour le **CaptureItem** sélectionné et la capture des frames est initiée par l’appel de [StartCapture](/uwp/api/windows.graphics.capture.graphicscapturesession).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_StartCapture":::

## <a name="handle-graphics-capture-events"></a>Gérer les événements de capture graphique
À l’étape précédente, nous avons inscrit deux gestionnaires pour les événements de capture graphique et configuré certains événements pour faciliter la gestion du déroulement de la boucle de capture.

L’événement **FrameArrived** est déclenché quand un nouveau Frame capturé est disponible dans le **Direct3D11CaptureFramePool** . Dans le gestionnaire de cet événement, appelez [TryGetNextFrame](/uwp/api/windows.graphics.capture.direct3d11captureframepool.trygetnextframe) sur l’expéditeur pour obtenir le frame capturé suivant. Une fois le frame récupéré, nous définissons la **_frameEvent** afin que notre boucle de capture sache qu’un nouveau frame est disponible.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_OnFrameArrived":::

Dans le gestionnaire d’événements **Closed** , nous signalons le **_closedEvent** afin que la boucle de capture sache quand arrêter.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_OnClosed":::

## <a name="wait-for-new-frames"></a>Attendre de nouveaux frames
La méthode d’assistance **WaitForNewFrame** décrite dans cette section est l’endroit où se produit le gros levage de la boucle de capture. N’oubliez pas que cette méthode est appelée à partir du gestionnaire d’événements **OnMediaStreamSourceSampleRequested** chaque fois que le **source** est prêt pour qu’un nouveau Frame soit ajouté au flux vidéo. À un niveau élevé, cette fonction copie simplement chaque trame vidéo capturée à partir d’une surface Direct3D vers une autre, afin qu’elle puisse être transmise dans le **source** pour l’encodage lors de la capture d’une nouvelle trame. Cet exemple utilise la bibliothèque SharpDX pour effectuer l’opération de copie réelle.

Avant d’attendre un nouveau Frame, la méthode supprime tout Frame précédent stocké dans la variable de classe, **_currentFrame**et réinitialise le **_frameEvent**. Ensuite, la méthode attend que la **_frameEvent** ou la **_closedEvent** soit signalée. Si l’événement Closed est défini, l’application appelle une méthode d’assistance pour nettoyer les ressources de capture. Cette méthode est présentée plus loin dans cet article.

Si l’événement frame est défini, nous savons que le gestionnaire d’événements **FrameArrived** défini à l’étape précédente a été appelé et que nous commençons le processus de copie des données de frame capturées dans une surface Direct3D 11 qui sera transmise au **source**.

Cet exemple utilise une classe d’assistance, **SurfaceWithInfo**, qui nous permet de transmettre le frame vidéo et l’heure système de la trame, les deux requises par le **source** , sous la forme d’un objet unique. La première étape du processus de copie de frame consiste à instancier cette classe et à définir l’heure système.

Les étapes suivantes font partie de cet exemple qui repose spécifiquement sur la bibliothèque SharpDX. Les fonctions d’assistance utilisées ici sont définies à la fin de cet article. Tout d’abord, nous utilisons le **MultiThreadLock** pour nous assurer qu’aucun autre thread n’accède à la mémoire tampon de trame vidéo pendant que nous effectuons la copie. Ensuite, nous appelons la méthode d’assistance **CreateSharpDXTexture2D** pour créer un objet SharpDX **Texture2D** à partir de la trame vidéo. Il s’agit de la texture source pour l’opération de copie. 

Nous allons ensuite copier à partir de l’objet **Texture2D** créé à l’étape précédente vers la texture de composition que nous avons créée précédemment dans le processus. Cette texture de composition agit comme un tampon d’échange afin que le processus d’encodage puisse fonctionner sur les pixels pendant la capture de l’image suivante. Pour effectuer la copie, nous effacons la vue de la cible de rendu associée à la texture de la composition, puis nous définissons la région dans la texture que vous souhaitez copier, la texture entière dans ce cas, puis nous appelons **CopySubresourceRegion** pour copier les pixels dans la texture de la composition.

Nous créons une copie de la description de la texture à utiliser lorsque nous créons notre texture cible, mais la description est modifiée, en affectant à **BindFlags** la valeur **renderTarget** afin que la nouvelle texture dispose d’un accès en écriture. La définition de **cpuaccessflags a** sur **None** permet au système d’optimiser l’opération de copie. La description de la texture est utilisée pour créer une ressource de texture et la ressource de texture de la composition est copiée dans cette nouvelle ressource à l’aide d’un appel à **CopyResource**. Enfin, **CreateDirect3DSurfaceFromSharpDXTexture** est appelé pour créer l’objet **IDirect3DSurface** retourné à partir de cette méthode.


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_WaitForNewFrame":::

## <a name="stop-capture-and-clean-up-resources"></a>Arrêter la capture et nettoyer les ressources
La méthode **Stop** permet d’arrêter l’opération de capture. Votre application peut l’appeler par programme ou en réponse à une interaction de l’utilisateur, comme un clic sur un bouton. Cette méthode définit simplement la **_closedEvent**. La méthode **WaitForNewFrame** définie dans les étapes précédentes recherche cet événement et, si elle est définie, arrête l’opération de capture.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_Stop":::

La méthode **Cleanup** est utilisée pour supprimer correctement les ressources qui ont été créées pendant l’opération de copie. notamment :

- Objet **Direct3D11CaptureFramePool** utilisé par la session de capture
- **GraphicsCaptureSession** et **GraphicsCaptureItem**
- Les appareils Direct3D et SharpDX
- La texture SharpDX et la vue de cible de rendu utilisées dans l’opération de copie.
- **Direct3D11CaptureFrame** utilisé pour stocker le frame actuel.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_Cleanup":::

## <a name="helper-wrapper-classes"></a>Classes wrapper d’assistance
Les classes d’assistance suivantes ont été définies pour faciliter l’exemple de code dans cet article.

La classe d’assistance **MultithreadLock** encapsule la classe **multithread** SharpDX qui permet de s’assurer que les autres threads n’accèdent pas aux ressources de texture en cours de copie.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_MultithreadLock":::

**SurfaceWithInfo** est utilisé pour associer un **IDirect3DSurface** à un **SystemRelativeTime** représentant le frame capturé et l’heure à laquelle il a été capturé, respectivement.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_SurfaceWithInfo":::

## <a name="direct3d-and-sharpdx-helper-apis"></a>API d’assistance Direct3D et SharpDX
Les API d’assistance suivantes sont définies pour soustraire la création de ressources Direct3D et SharpDX. Une explication détaillée de ces technologies n’entre pas dans le cadre de cet article, mais le code est fourni ici pour vous permettre d’implémenter l’exemple de code présenté dans la procédure pas à pas.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/Direct3D11Helpers.cs" id="snippet_Direct3D11Helpers":::

## <a name="see-also"></a>Voir aussi

* [Espace de noms Windows. Graphics. capture](https://docs.microsoft.com/uwp/api/windows.graphics.capture)
* [Capture d'écran](screen-capture.md)