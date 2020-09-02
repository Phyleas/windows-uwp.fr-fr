---
ms.assetid: a128edc8-8a80-4645-ac29-908ede2d1c72
description: Utiliser une instance MediaFrameReader avec MediaCapture pour récupérer des images à partir d’appareils photos couleur, de profondeur et infrarouges, de périphériques audio ou même de sources d’images personnalisés telles que celles qui produisent des images de suivi des squelettes.
title: Traiter des images multimédias avec MediaFrameReader
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b53103f0d0c67bd18b71ac94812f4cef53ca8ac0
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363812"
---
# <a name="process-media-frames-with-mediaframereader"></a>Traiter des images multimédias avec MediaFrameReader

Cet article explique comment utiliser un [**MediaFrameReader**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) avec [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) pour obtenir des images multimédias à partir d’une ou de plusieurs sources disponibles, notamment des caméras de couleur, de profondeur et infrarouges, des périphériques audio ou même des sources de frame personnalisées telles que celles qui produisent des frames de suivi squelettiques. Cette fonctionnalité est conçue pour être utilisée par les applications qui effectuent le traitement en temps réel des images multimédias, telles que les applications de caméra prenant en charge la profondeur et de réalité augmentée.

Si vous souhaitez simplement capturer du contenu vidéo ou des photos, comme c’est possible avec une application standard de photographie, vous avez probablement tout intérêt à valoriser l’une des autres techniques de capture prises en charge par [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture). Pour consulter une liste des techniques de capture multimédia disponibles et des articles vous expliquant comment les utiliser, consultez la page [**Appareil photo**](camera.md).

> [!NOTE] 
> Les fonctionnalités décrites dans cet article sont disponibles uniquement à partir de Windows 10, version 1607.

> [!NOTE] 
> Il existe un exemple d’application Windows universelle qui illustre l’utilisation de **MediaFrameReader** pour afficher des images de différentes sources, notamment d’appareils photos couleur, de profondeur et infrarouges. Pour plus d’informations voir [Profils d’appareil photo](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames).

> [!NOTE] 
> Un nouvel ensemble d’API pour l’utilisation de **MediaFrameReader** avec des données audio a été introduit dans Windows 10, version 1803. Pour plus d’informations, consultez [traiter des frames audio avec MediaFrameReader](process-audio-frames-with-mediaframereader.md).


## <a name="setting-up-your-project"></a>Configuration de votre projet
Comme avec toute application utilisant **MediaCapture**, vous devez déclarer que votre application utilise la fonctionnalité *webcam* avant de tenter d’accéder à un appareil photo. Si votre application capture à partir d’un périphérique audio, vous devez également déclarer la fonctionnalité *microphone*. 

**Ajouter des fonctionnalités au manifeste de l’application**

1.  Dans Microsoft Visual Studio, dans l’**Explorateur de solutions**, ouvrez le concepteur pour le manifeste de l’application en double-cliquant sur l’élément **package.appxmanifest**.
2.  Sélectionnez l’onglet **Fonctionnalités**.
3.  Activez les cases à cocher **Webcam** et **Microphone**.
4.  Pour accéder à la bibliothèque d’images et à la vidéothèque, cochez les cases correspondant à **Bibliothèque d’images** et **Vidéothèque**.

L’exemple de code de cet article utilise des API des espaces de noms suivants, en plus de celles fournies par le modèle de projet par défaut.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetFramesUsing":::

## <a name="select-frame-sources-and-frame-source-groups"></a>Sélectionnez des sources d’images et des groupes de sources d’images
De nombreuses applications qui traitent des images multimédias doivent récupérer ces éléments de plusieurs sources simultanément, comme des appareils photos couleur et de profondeur d’un appareil. L’objet [**MediaFrameSourceGroup**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup) représente un ensemble de sources de cadre de média qui peuvent être utilisées simultanément. Appelez la méthode statique [**MediaFrameSourceGroup.FindAllAsync**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.findallasync) afin de récupérer une liste de l’ensemble des groupes de sources d’images pris en charge par l’appareil actuel.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetFindAllAsync":::

Vous pouvez également créer un [**DeviceWatcher**](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) à l’aide de [**DeviceInformation. CreateWatcher**](/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher) et la valeur retournée à partir de [**MediaFrameSourceGroup. GetDeviceSelector**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.getdeviceselector) pour recevoir des notifications lorsque les groupes de sources de frame disponibles sur l’appareil changent, par exemple lorsqu’une caméra externe est branchée. Pour plus d’informations, consultez la page [**Énumérer les appareils**](../devices-sensors/enumerate-devices.md).

Un [**MediaFrameSourceGroup**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup) a une collection d’objets [**MediaFrameSourceInfo**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceInfo) qui décrivent les sources de frame incluses dans le groupe. Une fois les groupes de sources d’images disponibles sur cet appareil récupérés, vous pouvez sélectionner le groupe exposant les sources d’images qui vous intéressent.

L’exemple suivant vous présente le moyen le plus simple de sélectionner un groupe de sources d’images. Ce code effectue simplement une boucle sur tous les groupes disponibles, puis effectue une boucle sur chaque élément de la collection [**SourceInfos**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.sourceinfos) . Chacune des instances **MediaFrameSourceInfo** est examinée, afin de vérifier qu’elles prennent en charge les fonctionnalités recherchées. Dans ce cas, la propriété [**MediaStreamType**](/uwp/api/windows.media.capture.frames.mediaframesourceinfo.mediastreamtype) est examinée pour vérifier qu’elle contient la valeur [**VideoPreview**](/uwp/api/Windows.Media.Capture.MediaStreamType), synonyme de production d’un flux d’aperçu vidéo par l’appareil, et la propriété [**SourceKind**](/uwp/api/windows.media.capture.frames.mediaframesourceinfo.sourcekind) est examinée à la recherche de la valeur [**Color**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceKind), indiquant que la source fournit des images couleur.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetSimpleSelect":::

Cette méthode d’identification du groupe des sources d’images et des sources d’images fonctionne efficacement avec des cas simples, mais si vous souhaitez sélectionner des sources d’images en fonction de critères plus complexes, le processus peut rapidement devenir très lourd. Une autre méthode consiste à effectuer cette sélection à l’aide de la syntaxe Linq et d’objets anonymes. L’exemple suivant utilise la méthode d’extension **Select** afin de transformer les objets **MediaFrameSourceGroup** de la liste *frameSourceGroups* en objets anonymes avec deux champs : *sourceGroup*, représentant le groupe en soi et *colorSourceInfo*, qui est associé à la source des images couleur du groupe. Le champ *colorSourceInfo* est défini sur le résultat de **FirstOrDefault**, qui sélectionne le premier objet pour lequel le prédicat fourni a la valeur TRUE. Dans ce cas, le prédicat a la valeur TRUE si le type de flux est **VideoPreview**, le type de source est **Color** et si l’appareil photo se trouve sur le panneau avant de l’appareil.

À partir de la liste des objets anonymes renvoyés de la requête décrite ci-dessus, la méthode d’extension **Where**, la méthode d’extension est utilisée pour sélectionner uniquement les objets dont le champ *colorSourceInfo* n’a pas la valeur NULL. Enfin, **FirstOrDefault** est appelé pour sélectionner le premier élément de la liste.

Vous pouvez désormais utiliser les champs de l’objet sélectionné afin d’obtenir les références à l’instance **MediaFrameSourceGroup** et à l’objet **MediaFrameSourceInfo** sélectionnés représentant l’appareil photo couleur. Ces éléments seront utilisés ultérieurement pour initialiser l’objet **MediaCapture** et créer une instance **MediaFrameReader** pour la source sélectionnée. Enfin, vous avez intérêt à procéder à un test afin de déterminer si le groupe de sources a la valeur NULL, ce qui signifie que l’appareil actuel ne présente pas vos sources de capture demandées.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetSelectColor":::

L’exemple suivant utilise une technique similaire à ce qui est décrit plus haut pour sélectionner un groupe de sources comportant des appareils photos couleur, de profondeur et infrarouges.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetColorInfraredDepth":::

> [!NOTE]
> À compter de Windows 10, version 1803, vous pouvez utiliser la classe [**MediaCaptureVideoProfile**](/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfile) pour sélectionner une source de cadre de support avec un ensemble de fonctionnalités souhaitées. Pour plus d’informations, consultez la section **utiliser des profils vidéo pour sélectionner une source de cadre** plus loin dans cet article.


## <a name="initialize-the-mediacapture-object-to-use-the-selected-frame-source-group"></a>Initialiser l’objet MediaCapture afin d’utiliser le groupe de sources d’images sélectionné
L’étape suivante consiste à initialiser l’objet **MediaCapture** afin d’utiliser le groupe de sources d’images sélectionné à l’étape précédente.

L’objet **MediaCapture** étant généralement utilisé à partir de multiples emplacements de votre application, vous devez déclarer une variable de membre de classe pour le contenir.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetDeclareMediaCapture":::

Créez une instance de l’objet **MediaCapture** en appelant le constructeur. Ensuite, créez un objet [**MediaCaptureInitializationSettings**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings) qui sera utilisé pour initialiser l’objet **MediaCapture** . Dans cet exemple, les paramètres suivants sont utilisés :

* [**SourceGroup**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.sourcegroup) - Ce paramètre indique au système le groupe de sources utilisé pour récupérer les images. N’oubliez pas que le groupe de sources définir un ensemble de sources d’images multimédias pouvant être utilisées simultanément.
* [**SharingMode**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.sharingmode) : indique au système si vous avez besoin d’un contrôle exclusif sur les appareils source de capture. Si vous définissez cette valeur sur  [**ExclusiveControl**](/uwp/api/Windows.Media.Capture.MediaCaptureSharingMode), cela signifie que vous pouvez modifier les paramètres du périphérique de capture, comme le format des frames qu’il produit, mais cela signifie que si une autre application a déjà un contrôle exclusif, votre application échouera lorsqu’elle tentera d’initialiser l’appareil de capture de média. Si vous définissez cette valeur sur [**SharedReadOnly**](/uwp/api/Windows.Media.Capture.MediaCaptureSharingMode), vous pouvez recevoir des frames à partir des sources de frame, même si elles sont utilisées par une autre application, mais vous ne pouvez pas modifier les paramètres des appareils.
* [**MemoryPreference**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.memorypreference) : Si vous spécifiez l' [**UC**](/uwp/api/Windows.Media.Capture.MediaCaptureMemoryPreference), le système utilise la mémoire de l’UC, ce qui garantit que lorsque les trames arrivent, elles sont disponibles en tant qu’objets [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) . Si vous spécifiez [**auto**](/uwp/api/Windows.Media.Capture.MediaCaptureMemoryPreference), le système choisit dynamiquement l’emplacement de mémoire optimal pour stocker les frames. Si le système choisit d’utiliser la mémoire GPU, les images multimédias arrivent en tant qu’objets [**IDirect3DSurface**](/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface), et non en tant qu’instances **SoftwareBitmap**.
* [**StreamingCaptureMode**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.streamingcapturemode) : définissez cette valeur sur [**Video**](/uwp/api/Windows.Media.Capture.StreamingCaptureMode) pour indiquer que l’audio n’a pas besoin d’être diffusé en continu.

Appelez [**InitializeAsync**](/uwp/api/windows.media.capture.mediacapture.initializeasync) afin d’initialiser **MediaCapture** avec vos paramètres souhaités. Assurez-vous d’effectuer votre appel au sein d’un bloc *try*, ceci pour vous protéger en cas de mise en échec de l’initialisation.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetInitMediaCapture":::

## <a name="set-the-preferred-format-for-the-frame-source"></a>Définir le format préféré de la source d’images
Pour définir le format préféré d’une source d’images, vous devez obtenir un objet [**MediaFrameSource**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSource) représentant la source. Pour obtenir cet objet, accédez au dictionnaire [**Frames**](/previous-versions/windows/apps/phone/jj207578(v=win.10)) de l’objet **MediaCapture** initialisé, en spécifiant l’identificateur de la source d’images que vous souhaitez utiliser. C’est la raison pour laquelle nous avons enregistré l’objet [**MediaFrameSourceInfo**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceInfo) lors de la sélection d’un groupe de sources Frame.

La propriété [**MediaFrameSource.SupportedFormats**](/uwp/api/windows.media.capture.frames.mediaframesource.supportedformats) comporte une liste d’objets [**MediaFrameFormat**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameFormat) décrivant les formats pris en charge pour la source d’images. Utilisez la méthode d’extension Linq **Where** pour sélectionner un format basé sur les propriétés souhaitées. Dans cet exemple, le format sélectionné présente une largeur de 1 080 pixels et peut fournir des images au format RVB 32 bits. La méthode d’extension **FirstOrDefault** sélectionne la première entrée de la liste. Si le format sélectionné est NULL, le format demandé n’est pas pris en charge par la source des images. Si le format est pris en charge, vous pouvez demander que la source l’utilise en appelant [**SetFormatAsync**](../develop/index.md).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetGetPreferredFormat":::

## <a name="create-a-frame-reader-for-the-frame-source"></a>Créer un lecteur d’images pour la source d’images
Pour recevoir les images pour une source d’images multimédias, utilisez une instance [**MediaFrameReader**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetDeclareMediaFrameReader":::

Instanciez le lecteur d’images en appelant [**CreateFrameReaderAsync**](/uwp/api/windows.media.capture.mediacapture.createframereaderasync) sur votre objet **MediaCapture** initialisé. Le premier argument à appliquer à cette méthode est la source d’images à partir de laquelle vous souhaitez recevoir des images. Vous pouvez créer un lecteur séparé d’images pour chaque source d’images que vous souhaitez utiliser. Le second argument indique au système le format de sortie dans lequel vous souhaitez que les images arrivent. Cela peut vous éviter d’avoir à faire vos propres conversions sur les images en entrée. Notez que si vous spécifiez un format qui n’est pas pris en charge par la source d’images, une exception sera transmise. Aussi, assurez-vous que cette valeur se trouve dans la collection [**SupportedFormats**](/uwp/api/windows.media.capture.frames.mediaframesource.supportedformats).  

Après avoir créé le lecteur d’images, enregistrez un gestionnaire pour l’événement [**FrameArrived**](/uwp/api/windows.media.capture.frames.mediaframereader.framearrived) qui est déclenché quand une nouvelle image est disponible à partir de la source.

Demandez au système de commencer à lire des frames à partir de la source en appelant [**StartAsync**](/uwp/api/windows.media.capture.frames.mediaframereader.startasync).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetCreateFrameReader":::

## <a name="handle-the-frame-arrived-event"></a>Gérer l’événement déclenché à l’arrivée des images
L’événement [**MediaFrameReader. FrameArrived**](/uwp/api/windows.media.capture.frames.mediaframereader.framearrived) est déclenché chaque fois qu’un nouveau frame est disponible. Vous pouvez choisir de traiter toutes les images qui arrivent ou seulement celles dont vous avez besoin. Étant donné que le lecteur d’images déclenche l’événement sur son propre thread, il vous faudra éventuellement implémenter une logique de synchronisation afin de garantir que vous n’essayez pas d’accéder aux mêmes données à partir de plusieurs threads. Cette section vous indique comment synchroniser les images couleur des dessins sur un contrôle d’images dans une page XAML. Ce scénario traite de la contrainte de synchronisation supplémentaire qui implique que l’ensemble des mises à jour des contrôles XAML soient effectuées sur le même thread d’interface utilisateur.

La première étape de l’affichage des images au format XAML consiste à créer un contrôle Image. 

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml" id="SnippetImageElementXAML":::

Dans votre page code-behind, déclarez une variable de membre de classe de type **SoftwareBitmap**, qui sera utilisée en tant que mémoire tampon d’arrière-plan sur laquelle seront copiées l’ensemble des images entrantes. Notez que les données d’images en soi ne sont pas copiées, ce sont les références d’objet qui le sont. Par ailleurs, déclarez une valeur booléenne détectant l’exécution de votre opération d’interface utilisateur.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetDeclareBackBuffer":::

Les images arrivant en tant qu’objets **SoftwareBitmap**, vous devez créer un objet [**SoftwareBitmapSource**](/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) qui vous permet d’utiliser une instance **SoftwareBitmap** en tant que source pour un élément **Control** XAML. Vous devez définir la source d’images au sein de votre code avant de démarrer le lecteur d’images.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetImageElementSource":::

Il est désormais temps d’implémenter le gestionnaire d’événements **FrameArrived**. Quand le gestionnaire est appelé, le paramètre *sender* contient une référence à l’objet **MediaFrameReader** qui déclenche l’événement. Appelez [**TryAcquireLatestFrame**](/uwp/api/windows.media.capture.frames.mediaframereader.tryacquirelatestframe) sur cet objet pour tenter d’accéder au dernier frame. Comme son nom l’indique, **TryAcquireLatestFrame** peut ne pas réussir à renvoyer une image. Par conséquent, quand vous accédez aux propriétés VideoMediaFrame puis SoftwareBitmap, assurez-vous de détecter la définition de la valeur NULL. Dans cet exemple, l’opérateur conditionnel NULL ? est utilisé pour accéder à l’instance **SoftwareBitmap**. Ensuite, la valeur NULL est recherchée sur l’objet récupéré.

Le contrôle **Image** peut afficher des images uniquement au format BRGA8, avec aucune valeur alpha ou avec des valeurs alpha prémultipliées. Si l’image arrivante n’est pas dans ce format, la méthode statique [**Convert**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert) est utilisée pour convertir l’image bitmap logicielle au format approprié.

Ensuite, la méthode [**Interlocked.Exchange**](/dotnet/api/system.threading.interlocked.exchange#System_Threading_Interlocked_Exchange__1___0____0_) est utilisée pour remplacer la référence de l’image bitmap en entrée par l’image bitmap de la mémoire tampon d’arrière-plan. Cette méthode échange des références au cours d’une opération atomique thread-safe. Après le remplacement, l’ancienne image de mémoire tampon d’arrière-plan, désormais dans la variable *softwareBitmap*, est supprimée à des fins de nettoyage de ses ressources.

Ensuite, l’instance [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) associée à l’élément **Image** est utilisée pour créer une tâche qui s’exécutera sur le thread d’interface utilisateur en appelant [**RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync). Étant donné que les tâches asynchrones seront exécutées au sein de la tâche, l’expression lambda transmise à **RunAsync** est déclarée avec le mot-clé *async*.

Au sein de la tâche, la variable *_taskRunning* est examinée afin de garantir qu’une seule instance de la tâche s’exécute à la fois. SI la tâche n’est pas en cours d’exécution, l’élément *_taskRunning* est défini sur True, ceci pour prévenir toute nouvelle réexécution. Dans une boucle *while*, **Interlocked.Exchange** est appelé pour l’exécution d’une copie d’une mémoire tampon d’arrière-plan sur une instance **SoftwareBitmap** temporaire, jusqu’à ce que l’image de la mémoire tampon d’arrière-plan soit définie sur NULL. À chaque fois que l’image bitmap temporaire est remplie, la propriété **Source** du contrôle **Image** est castée en instance **SoftwareBitmapSource**, puis [**SetBitmapAsync**](/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync) est appelée pour définir la source de l’image.

Enfin, la variable *_taskRunning* est redéfinie sur False, afin que la tâche puisse à nouveau s’exécuter lors du prochain appel du gestionnaire.

> [!NOTE] 
> Si vous accédez aux objets [**SoftwareBitmap**](/uwp/api/windows.media.capture.frames.videomediaframe.softwarebitmap) ou [**Direct3DSurface**](/uwp/api/windows.media.capture.frames.videomediaframe.direct3dsurface) fournis par la propriété [**VideoMediaFrame**](/uwp/api/windows.media.capture.frames.mediaframereference.videomediaframe) d’un [**MediaFrameReference**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameReference), le système crée une référence forte à ces objets. Autrement dit, ils ne sont pas supprimés lorsque vous appelez [**Dispose**](/uwp/api/windows.media.capture.frames.mediaframereference.close) sur le conteneur **MediaFrameReference**. Vous devez appeler la méthode **Dispose** de **SoftwareBitmap** ou de **Direct3DSurface** explicitement et directement pour les objets à supprimer immédiatement. Sinon, le récupérateur de mémoire va libérer de la mémoire pour ces objets. Mais vous ne pouvez pas savoir quand cela se produit, et si le nombre de surfaces ou d’images bitmap allouées dépasse la quantité maximale autorisée par le système, le flux de nouvelles images s’arrête. Vous pouvez copier des frames récupérés, à l’aide de la méthode [**SoftwareBitmap. Copy**](/uwp/api/windows.graphics.imaging.softwarebitmap.copy) , par exemple, puis libérer les images d’origine pour surmonter cette limitation. En outre, si vous créez le **MediaFrameReader** à l’aide de la surcharge [CreateFrameReaderAsync (Windows. Media. capture. frames. MediaFrameSource inputSource, System. String outputSubtype, Windows. Graphics. Imaging. BitmapSize deputs)](/uwp/api/windows.media.capture.mediacapture.createframereaderasync#Windows_Media_Capture_MediaCapture_CreateFrameReaderAsync_Windows_Media_Capture_Frames_MediaFrameSource_System_String_Windows_Graphics_Imaging_BitmapSize_) ou [CreateFrameReaderAsync (Windows. Media. capture. frames. MediaFrameSource InputSource, System. String outputSubtype)](/uwp/api/windows.media.capture.mediacapture.createframereaderasync#Windows_Media_Capture_MediaCapture_CreateFrameReaderAsync_Windows_Media_Capture_Frames_MediaFrameSource_System_String_), les frames retournés sont des copies des données de frame d’origine et ne provoquent pas l’arrêt de l’acquisition de frame lorsqu’ils sont conservés. 


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetFrameArrived":::

## <a name="cleanup-resources"></a>Nettoyer les ressources
Lorsque vous avez fini de lire les images, assurez-vous d’arrêter le lecteur d’images multimédias en appelant [**StopAsync**](/uwp/api/windows.media.capture.frames.mediaframereader.stopasync), en annulant l’enregistrement du gestionnaire **FrameArrived** et en supprimant l’objet **MediaCapture**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetCleanup":::

Pour plus d’informations sur le nettoyage des objets de capture de média lorsque votre application est suspendue, consultez [**afficher l’aperçu de la caméra**](simple-camera-preview-access.md).

## <a name="the-framerenderer-helper-class"></a>La classe d’assistance FrameRenderer
Le [profil d’appareil photo](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames) Windows universel fournit une classe d’assistance qui facilite l’affichage d’images de sources couleur, infrarouges et de profondeur dans votre application. Généralement, vous ne vous contentez pas d’afficher les données infrarouge et de profondeur à l’écran, mais cette classe d’assistance est un outil utile permettant d’illustrer la fonctionnalité du lecteur d’images et de déboguer votre propre implémentation du lecteur d’images.

La classe d’assistance **FrameRenderer** implémente les méthodes suivantes.

* Constructeur **FrameRenderer** - Le constructeur initialise la classe d’assistance afin d’utiliser l’élément XAML [**Image**](/uwp/api/Windows.UI.Xaml.Controls.Image) transmis pour l’affichage des images multimédias.
* **ProcessFrame** - Cette méthode affiche une image multimédia, représentée par une instance [**MediaFrameReference**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameReference), dans l’élément **Image** transmis dans le constructeur. En général, vous devez appeler cette méthode à partir de votre gestionnaire d’événements [**FrameArrived**](/uwp/api/windows.media.capture.frames.mediaframereader.framearrived) , en passant le frame retourné par [**TryAcquireLatestFrame**](/uwp/api/windows.media.capture.frames.mediaframereader.tryacquirelatestframe).
* **ConvertToDisplayableImage** - Cette méthode vérifie le format de l’image multimédia et si nécessaire la convertit en format affichable. Pour les images couleur, elle garantit que le format couleur est BGRA8 et que le mode alpha bitmap est prémultiplié. Pour les images de profondeur et infrarouges, chaque ligne de balayage est traitée pour convertir les valeurs de profondeur et infrarouges en gradient pseudocolor, à l’aide de la classe **PseudoColorHelper** qui est également incluse dans l’exemple et répertoriée ci-dessous.

> [!NOTE] 
> Pour procéder à une manipulation de pixels sur les images **SoftwareBitmap**, vous devez accéder à une mémoire tampon native. Pour ce faire, vous devez utiliser l’interface COM IMemoryBufferByteAccess COM incluse dans l’ensemble du code ci-dessous, et vous devez mettre à jour vos propriétés de projet afin de permettre la compilation du code non sécurisé. Pour plus d’informations, consultez la page [Créer, modifier et enregistrer des images bitmap](imaging.md).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/FrameRenderer.cs" id="SnippetIMemoryBufferByteAccess":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/FrameRenderer.cs" id="SnippetFrameRenderer":::

## <a name="use-multisourcemediaframereader-to-get-time-corellated-frames-from-multiple-sources"></a>Utiliser MultiSourceMediaFrameReader pour recevoir des frames corellated à partir de plusieurs sources
À compter de Windows 10, version 1607, vous pouvez utiliser [**MultiSourceMediaFrameReader**](/uwp/api/windows.media.capture.frames.multisourcemediaframereader) pour recevoir des frames corellated à partir de plusieurs sources. Cette API facilite le traitement qui nécessite des frames provenant de plusieurs sources qui ont été prises dans une proximité temporelle proche, par exemple l’utilisation de la classe [**DepthCorrelatedCoordinateMapper**](/uwp/api/windows.media.devices.core.depthcorrelatedcoordinatemapper) . L’une des limitations de l’utilisation de cette nouvelle méthode est que les événements arrivés à la trame ne sont déclenchés qu’au taux de la source de capture la plus lente. Les images supplémentaires issues de sources plus rapides seront supprimées. En outre, étant donné que le système s’attend à ce que les trames proviennent de différentes sources à des taux différents, il ne reconnaît pas automatiquement si une source a cessé de générer des frames entièrement. L’exemple de code de cette section montre comment utiliser un événement pour créer votre propre logique de délai d’attente qui est appelée si les frames corrélés n’arrivent pas dans une limite de temps définie par l’application.

Les étapes d’utilisation de [**MultiSourceMediaFrameReader**](/uwp/api/windows.media.capture.frames.multisourcemediaframereader) sont similaires à [**celles décrites précédemment**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) dans cet article. Cet exemple utilise une source de couleur et une source de profondeur. Déclarez des variables de chaîne pour stocker les ID de source de cadre de support qui seront utilisés pour sélectionner des frames à partir de chaque source. Ensuite, déclarez un [**ManualResetEventSlim**](/dotnet/api/system.threading.manualreseteventslim), un [**CancellationTokenSource**](/dotnet/api/system.threading.cancellationtokensource)et un [**EventHandler**](/dotnet/api/system.eventhandler) qui sera utilisé pour implémenter la logique de délai d’attente pour l’exemple. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetMultiFrameDeclarations":::

À l’aide des techniques décrites précédemment dans cet article, interrogez un [**MediaFrameSourceGroup**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup) qui comprend les sources de couleur et de profondeur requises pour cet exemple de scénario. Après avoir sélectionné le groupe de sources Frame souhaité, récupérez le [**MediaFrameSourceInfo**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceInfo) pour chaque source de frame.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetSelectColorAndDepth":::

Créez et initialisez un objet **MediaCapture** , en passant le groupe source Frame sélectionné dans les paramètres d’initialisation.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetMultiFrameInitMediaCapture":::

Après l’initialisation de l’objet **MediaCapture** , récupérez les objets [**MediaFrameSource**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSource) pour les caméras couleur et profondeur. Stockez l’ID de chaque source afin de pouvoir sélectionner le frame arrivant pour la source correspondante.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetGetColorAndDepthSource":::

Créez et initialisez le **MultiSourceMediaFrameReader** en appelant [**CreateMultiSourceFrameReaderAsync**](/uwp/api/windows.media.capture.mediacapture.createmultisourceframereaderasync) et en passant un tableau de sources de frame que le lecteur utilisera. Inscrivez un gestionnaire d’événements pour l’événement [**FrameArrived**](/uwp/api/windows.media.capture.frames.multisourcemediaframereader.FrameArrived) . Cet exemple crée une instance de la classe d’assistance **FrameRenderer** , décrite précédemment dans cet article, pour restituer des frames dans un contrôle **image** . Démarrez le lecteur de frames en appelant [**StartAsync**](/uwp/api/windows.media.capture.frames.multisourcemediaframereader.StartAsync).

Inscrivez un gestionnaire d’événements pour l’événement **CorellationFailed** déclaré précédemment dans l’exemple. Nous signalerons cet événement si l’une des sources de trame de support utilisées cesse de produire des frames. Enfin, appelez [**Task. Run**](/dotnet/api/system.threading.tasks.task.run#System_Threading_Tasks_Task_Run_System_Action_) pour appeler la méthode d’assistance du délai d’attente, **NotifyAboutCorrelationFailure**, sur un thread distinct. L’implémentation de cette méthode est présentée plus loin dans cet article.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetInitMultiFrameReader":::

L’événement **FrameArrived** est déclenché chaque fois qu’un nouveau frame est disponible à partir de toutes les sources de frame multimédia qui sont gérées par le **MultiSourceMediaFrameReader**. Cela signifie que l’événement sera déclenché sur la cadence de la source de média la plus lente. Si une source génère plusieurs frames au moment où une source plus lente produit une trame, les images supplémentaires de la source Fast sont supprimées. 

Obtient le [**MultiSourceMediaFrameReference**](/uwp/api/windows.media.capture.frames.multisourcemediaframereference) associé à l’événement en appelant [**TryAcquireLatestFrame**](/uwp/api/windows.media.capture.frames.multisourcemediaframereader.TryAcquireLatestFrame). Obtient le **MediaFrameReference** associé à chaque source de frame de média en appelant [**TryGetFrameReferenceBySourceId**](/uwp/api/windows.media.capture.frames.multisourcemediaframereference.trygetframereferencebysourceid), en passant les chaînes d’ID stockées lors de l’initialisation du lecteur de frame.

Appelez la méthode [**Set**](/dotnet/api/system.threading.manualreseteventslim.set#System_Threading_ManualResetEventSlim_Set) de l’objet **ManualResetEventSlim** pour signaler que les trames sont arrivées. Nous allons vérifier cet événement dans la méthode **NotifyCorrelationFailure** qui s’exécute dans un thread distinct. 

Enfin, effectuez tout traitement sur les trames de médias corrélées. Cet exemple affiche simplement le frame de la source de profondeur.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetMultiFrameArrived":::

La méthode d’assistance **NotifyCorrelationFailure** a été exécutée sur un thread séparé après le démarrage du lecteur de frame. Dans cette méthode, vérifiez si l’événement Frame reçu a été signalé. N’oubliez pas que dans le gestionnaire **FrameArrived** , nous définissons cet événement chaque fois qu’un ensemble de frames corrélés arrive. Si l’événement n’a pas été signalé pour une période de temps définie par l’application-5 secondes est une valeur raisonnable et que la tâche n’a pas été annulée à l’aide de **CancellationToken**, il est probable que l’une des sources de cadre de média a cessé de lire les frames. Dans ce cas, vous souhaitez généralement arrêter le lecteur de frames, donc déclencher l’événement **CorrelationFailed** défini par l’application. Dans le gestionnaire de cet événement, vous pouvez arrêter le lecteur de frame et nettoyer ses ressources associées, comme indiqué précédemment dans cet article.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetNotifyCorrelationFailure":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetCorrelationFailure":::

## <a name="use-buffered-frame-acquisition-mode-to-preserve-the-sequence-of-acquired-frames"></a>Utilisez le mode d’acquisition de frame mis en mémoire tampon pour conserver la séquence des frames acquis
À compter de Windows 10, version 1709, vous pouvez définir la propriété **[AcquisitionMode](/uwp/api/windows.media.capture.frames.mediaframereader.AcquisitionMode)** d’un **MediaFrameReader** ou d’un **MultiSourceMediaFrameReader** à mettre en **mémoire tampon** pour conserver la séquence de frames passée dans votre application à partir de la source du frame.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetSetBufferedFrameAcquisitionMode":::

Dans le mode d’acquisition par défaut, en **temps réel**, si plusieurs trames sont acquises à partir de la source alors que votre application gère toujours l’événement **FrameArrived** pour un frame précédent, le système envoie à votre application le frame le plus récemment acquis et supprime les images supplémentaires en attente dans la mémoire tampon. Cela permet à votre application d’utiliser le frame le plus récent disponible en permanence. Il s’agit généralement du mode le plus utile pour les applications de vision d’ordinateur en temps réel. 

En mode d’acquisition **mis en mémoire tampon** , le système conserve toutes les trames dans le tampon et les fournit à votre application via l’événement **FrameArrived** dans l’ordre de réception. Notez que dans ce mode, lorsque la mémoire tampon du système pour les frames est remplie, le système cesse d’acquérir de nouvelles images jusqu’à ce que votre application termine l’événement **FrameArrived** pour les trames précédentes, libérant ainsi de l’espace dans la mémoire tampon.

## <a name="use-mediasource-to-display-frames-in-a-mediaplayerelement"></a>Utilisation de MediaSource pour afficher des frames dans un MediaPlayerElement
À compter de Windows, version 1709, vous pouvez afficher les images acquises à partir d’un **MediaFrameReader** directement dans un contrôle **[MediaPlayerElement](/uwp/api/windows.ui.xaml.controls.mediaplayerelement)** de votre page XAML. Pour ce faire, utilisez la fonction **[MediaSource. CreateFromMediaFrameSource](/uwp/api/windows.media.core.mediasource.createfrommediaframesource)** pour créer un objet **[MediaSource](/uwp/api/windows.media.core.mediasource)** qui peut être utilisé directement par un **[MediaPlayer](/uwp/api/windows.media.playback.mediaplayer)** associé à un **MediaPlayerElement**. Pour plus d’informations sur l’utilisation de **MediaPlayer** et **MediaPlayerElement**, consultez [lire des fichiers audio et vidéo avec MediaPlayer](play-audio-and-video-with-mediaplayer.md).

Les exemples de code suivants montrent une implémentation simple qui affiche simultanément les images d’une caméra frontale et arrière dans une page XAML.

Tout d’abord, ajoutez deux contrôles **MediaPlayerElement** à votre page XAML.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml" id="SnippetMediaPlayerElement1XAML":::

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml" id="SnippetMediaPlayerElement2XAML":::

Ensuite, à l’aide des techniques présentées dans les sections précédentes de cet article, sélectionnez un **MediaFrameSourceGroup** qui contient des objets **MediaFrameSourceInfo** pour les caméras couleur sur le panneau avant et arrière. Notez que le **MediaPlayer** ne convertit pas automatiquement les images de formats non-couleurs, tels qu’une profondeur ou des données infrarouges, en données de couleur. L’utilisation d’autres types de capteurs peut produire des résultats inattendus. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetMediaSourceSelectGroup":::

Initialise l’objet **MediaCapture** pour utiliser le **MediaFrameSourceGroup**sélectionné.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetMediaSourceInitMediaCapture":::

Enfin, appelez **[MediaSource. CreateFromMediaFrameSource](/uwp/api/windows.media.core.mediasource.createfrommediaframesource)** pour créer un **MediaSource** pour chaque source de frame à l’aide de la propriété **[ID](/uwp/api/windows.media.capture.frames.mediaframesourceinfo.Id)** de l’objet **MediaFrameSourceInfo** associé afin de sélectionner l’une des sources de frame dans la collection **[FrameSources](/uwp/api/windows.media.capture.mediacapture.FrameSources)** de l’objet **MediaCapture** . Initialisez un nouvel objet **MediaPlayer** et affectez-le à un **MediaPlayerElement** en appelant **[SetMediaPlayer](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.MediaPlayer)**. Définissez ensuite la propriété **[source](/uwp/api/windows.media.playback.mediaplayer.Source)** sur l’objet **MediaSource** nouvellement créé.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetMediaSourceMediaPlayer":::

## <a name="use-video-profiles-to-select-a-frame-source"></a>Utiliser des profils vidéo pour sélectionner une source de frame

Un profil d’appareil photo, représenté par un objet [**MediaCaptureVideoProfile**](/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfile) , représente un ensemble de fonctionnalités fournies par un périphérique de capture particulier, telles que des fréquences d’images, des résolutions ou des fonctionnalités avancées telles que la capture HDR. Un périphérique de capture peut prendre en charge plusieurs profils, ce qui vous permet de sélectionner celui qui est optimisé pour votre scénario de capture. À compter de Windows 10, version 1803, vous pouvez utiliser **MediaCaptureVideoProfile** pour sélectionner une source de cadre de média avec des fonctionnalités particulières avant d’initialiser l’objet **MediaCapture** . L’exemple de méthode suivant recherche un profil vidéo qui prend en charge HDR avec une large gamme de couleurs (WCG) et retourne un objet **MediaCaptureInitializationSettings** qui peut être utilisé pour initialiser le **MediaCapture** afin d’utiliser l’appareil et le profil sélectionnés.

Tout d’abord, appelez [**MediaFrameSourceGroup. FindAllAsync**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.findallasync) pour obtenir la liste de tous les groupes de sources de frame de média disponibles sur l’appareil actuel. Parcourez chaque groupe source et appelez [**MediaCapture. FindKnownVideoProfiles**](/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles) pour obtenir la liste de tous les profils vidéo du groupe source actuel qui prend en charge le profil spécifié, en l’occurrence HDR avec WCG photo. Si un profil qui répond aux critères est trouvé, créez un nouvel objet **MediaCaptureInitializationSettings** et définissez le **VideoProfile** sur le profil SELECT et le **VideoDeviceId** sur la propriété **ID** du groupe de sources frame de média actuel.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetGetSettingsWithProfile":::

Pour plus d’informations sur l’utilisation des profils d’appareil photo, consultez la rubrique [profils d’appareil photo](camera-profiles.md).

## <a name="related-topics"></a>Rubriques connexes

* [Appareil photo](camera.md)
* [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Profils d’appareil photo](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)
 

 
