---
ms.assetid: 40B97E0C-EB1B-40C2-A022-1AB95DFB085E
description: Cet article vous montre comment procéder à une diffusion multimédia sur des appareils distants à partir d’une application Windows universelle.
title: Diffusion multimédia
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a23e6c58325a8679a8df2ec0d3f429f75a230602
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363922"
---
# <a name="media-casting"></a>Diffusion multimédia



Cet article vous montre comment procéder à une diffusion multimédia sur des appareils distants à partir d’une application Windows universelle.

## <a name="built-in-media-casting-with-mediaplayerelement"></a>Diffusion multimédia intégrée avec MediaPlayerElement

La façon la plus simple d’effectuer un cast de médias à partir d’une application Windows universelle consiste à utiliser la fonctionnalité de cast intégrée du contrôle [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) .

Pour permettre à l’utilisateur d’ouvrir un fichier vidéo à lire dans le contrôle **MediaPlayerElement**, ajoutez les espaces de noms suivants à votre projet.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetBuiltInCastingUsing":::

Dans le fichier XAML de votre application, ajoutez un élément **MediaPlayerElement** et définissez [**AreTransportControlsEnabled**](/uwp/api/windows.ui.xaml.controls.mediaelement.aretransportcontrolsenabled) sur true.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml" id="SnippetMediaElement":::

Ajoutez un bouton pour permettre à l’utilisateur de lancer la sélection d’un fichier.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml" id="SnippetOpenButton":::

Dans le gestionnaire d’événements [**Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) du bouton, créez une nouvelle instance du [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker), ajoutez des types de fichiers vidéo à la collection [**FileTypeFilter**](/uwp/api/windows.storage.pickers.fileopenpicker.filetypefilter) , puis définissez l’emplacement de départ sur la bibliothèque vidéos de l’utilisateur.

Appelez [**PickSingleFileAsync**](/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync) pour ouvrir la boîte de dialogue du sélecteur de fichiers. Lorsque cette méthode est retournée, le résultat est un objet [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) représentant le fichier vidéo. Vérifiez que la valeur de ce fichier n’est pas null, ce qui est le cas si l’utilisateur annule l’opération de sélection. Appelez la méthode [**OpenAsync**](/uwp/api/windows.storage.storagefile.openasync) du fichier pour obtenir un [**IRandomAccessStream**](/uwp/api/Windows.Storage.Streams.IRandomAccessStream) pour le fichier. Enfin, créez un objet **MediaSource** à partir du fichier sélectionné en appelant [**CreateFromStorageFile**](/uwp/api/windows.media.core.mediasource.createfromstoragefile) et attribuez-le à la propriété [**Source**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) de l’objet **MediaPlayerElement** pour définir le fichier source du contrôle.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetOpenButtonClick":::

Une fois la vidéo chargée dans l’élément **MediaPlayerElement**, il suffit à l’utilisateur d’appuyer sur le bouton de diffusion sur les contrôles de transport pour lancer une boîte de dialogue intégrée qui lui permet de choisir un appareil sur lequel diffuser le média chargé.

![Bouton de diffusion MediaElement](images/media-element-casting-button.png)

> [!NOTE] 
> À compter de Windows 10, version 1607, nous vous recommandons d’utiliser la classe **MediaPlayer** pour lire des éléments multimédias. **MediaPlayerElement** est un contrôle XAML léger utilisé pour afficher le contenu d’un **MediaPlayer** dans une page XAML. Le contrôle **MediaElement** continue d’être pris en charge pour la compatibilité descendante. Pour plus d’informations sur l’utilisation de **MediaPlayer** et **MediaPlayerElement** pour lire du contenu multimédia, voir [Lire du contenu audio et vidéo avec MediaPlayer](play-audio-and-video-with-mediaplayer.md). Pour plus d’informations sur l’utilisation de **MediaSource** et des API associées pour utiliser du contenu multimédia, voir [Éléments, playlists et pistes multimédias](media-playback-with-mediasource.md).

## <a name="media-casting-with-the-castingdevicepicker"></a>Diffusion multimédia à l’aide de la classe CastingDevicePicker

Une deuxième façon de convertir un média en appareil consiste à utiliser [**CastingDevicePicker**](/uwp/api/Windows.Media.Casting.CastingDevicePicker). Pour utiliser cette classe, incluez l’espace de noms [**Windows. Media. Casting**](/uwp/api/Windows.Media.Casting) dans votre projet.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetCastingNamespace":::

Déclarez une variable de membre pour l’objet **CastingDevicePicker**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareCastingPicker":::

Lorsque vous initialisez la page, créez une nouvelle instance du sélecteur de Cast et définissez la propriété [**Filter**](/uwp/api/windows.media.casting.castingdevicepicker.filter) sur [**SupportsVideo**](/uwp/api/Windows.Media.Casting.CastingDevicePickerFilter) pour indiquer que les périphériques de cast listés par le sélecteur doivent prendre en charge la vidéo. Enregistrez un gestionnaire pour l’événement [**CastingDeviceSelected**](/uwp/api/windows.media.casting.castingdevicepicker.castingdeviceselected), qui est déclenché lorsque l’utilisateur sélectionne un appareil pour la diffusion.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetInitCastingPicker":::

Dans votre fichier XAML, ajoutez un bouton pour permettre à l’utilisateur de lancer le sélecteur.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml" id="SnippetCastPickerButton":::

Dans le gestionnaire d’événements **Click** du bouton, appelez [**TransformToVisual**](/uwp/api/windows.ui.xaml.uielement.transformtovisual) pour obtenir la transformation d’un élément d’interface utilisateur par rapport à un autre élément. Dans cet exemple, la transformation est la position du bouton du sélecteur de diffusion par rapport à la racine visuelle de la fenêtre d’application. Appelez la méthode [**Show**](/uwp/api/windows.media.casting.castingdevicepicker.show) de l’objet [**CastingDevicePicker**](/uwp/api/Windows.Media.Casting.CastingDevicePicker) pour lancer la boîte de dialogue de sélecteur de cast. Indiquez l’emplacement et les dimensions du bouton du sélecteur de diffusion afin que le système puisse afficher la boîte de dialogue à partir du bouton sur lequel l’utilisateur a appuyé.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetCastPickerButtonClick":::

Dans le gestionnaire d’événements **CastingDeviceSelected**, appelez la méthode [**CreateCastingConnection**](/uwp/api/windows.media.casting.castingdevice.createcastingconnection) de la propriété [**SelectedCastingDevice**](/uwp/api/windows.media.casting.castingdeviceselectedeventargs.selectedcastingdevice) des arguments d’événement, qui représente l’appareil de diffusion sélectionné par l’utilisateur. Enregistrez des gestionnaires pour les événements [**ErrorOccurred**](/uwp/api/windows.media.casting.castingconnection.erroroccurred) et [**StateChanged**](/uwp/api/windows.media.casting.castingconnection.statechanged). Enfin, appelez [**RequestStartCastingAsync**](/uwp/api/windows.media.casting.castingconnection.requeststartcastingasync) pour lancer la diffusion en transmettant le résultat à la méthode [**GetAsCastingSource**](/uwp/api/windows.ui.xaml.controls.mediaelement.getascastingsource) de l’objet **MediaPlayer** du contrôle **MediaPlayerElement** pour indiquer que le contenu multimédia à diffuser correspond à celui du **MediaPlayer** associé à **MediaPlayerElement**.

> [!NOTE] 
> La connexion de la diffusion doit être lancée sur le thread d’interface utilisateur. Étant donné que **CastingDeviceSelected** n’est pas appelé sur le thread d’interface utilisateur, vous devez placer ces appels à l’intérieur d’un appel à [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) pour changer cela.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetCastingDeviceSelected":::

Dans les gestionnaires d’événements **ErrorOccurred** et **StateChanged**, vous devez mettre à jour votre interface utilisateur pour informer l’utilisateur de l’état de diffusion actuel. Ces événements sont décrits en détail dans la section suivante portant sur la création d’un sélecteur d’appareil de diffusion personnalisé.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetEmptyStateHandlers":::

## <a name="media-casting-with-a-custom-device-picker"></a>Diffusion multimédia à l’aide d’un sélecteur d’appareil personnalisé

La section suivante décrit comment créer votre propre interface utilisateur de sélecteur d’appareil de diffusion par l’énumération des appareils de diffusion et le lancement de la connexion à partir de votre code.

Pour énumérer les périphériques de cast disponibles, incluez l’espace de noms [**Windows. Devices. Enumeration**](/uwp/api/Windows.Devices.Enumeration) dans votre projet.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetEnumerationNamespace":::

Ajoutez les contrôles suivants à votre page XAML pour implémenter l’interface utilisateur rudimentaire pour cet exemple :

-   Un bouton pour démarrer l’observateur d’appareils qui recherche les appareils de diffusion disponibles.
-   Contrôle [**ProgressRing**](/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) pour fournir des commentaires à l’utilisateur pour lequel l’énumération est en cours.
-   Une classe [**ListBox**](/uwp/api/Windows.UI.Xaml.Controls.ListBox) pour répertorier les appareils de diffusion détectés. Définissez un [**ItemTemplate**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) pour le contrôle afin que nous puissions assigner les objets de périphérique de cast directement au contrôle et afficher toujours la propriété [**FriendlyName**](/uwp/api/windows.media.casting.castingdevice.friendlyname) .
-   Un bouton permettant à l’utilisateur de déconnecter l’appareil de diffusion.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml" id="SnippetCustomPickerXAML":::

Dans le code sous-jacent, déclarez des variables de membre pour [**DeviceWatcher**](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) et [**CastingConnection**](/uwp/api/Windows.Media.Casting.CastingConnection).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareDeviceWatcher":::

Dans le gestionnaire **Click** de l’événement *startWatcherButton*, commencez par mettre à jour l’interface utilisateur en désactivant le bouton et en activant l’anneau de progression pendant l’énumération des appareils. Effacez la zone de liste des appareils de diffusion.

Ensuite, créez un observateur d’appareils en appelant [**DeviceInformation.CreateWatcher**](/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher). Cette méthode peut être utilisée pour observer de nombreux types d’appareils différents. Indiquez que vous souhaitez observer les appareils prenant en charge la diffusion vidéo en utilisant la chaîne de sélecteur d’appareil renvoyée par [**CastingDevice.GetDeviceSelector**](/uwp/api/windows.media.casting.castingdevice.getdeviceselector).

Enfin, enregistrez les gestionnaires d’événements pour les événements [**added**](/uwp/api/windows.devices.enumeration.devicewatcher.added), [**removed**](/uwp/api/windows.devices.enumeration.devicewatcher.removed), [**EnumerationCompleted**](/uwp/api/windows.devices.enumeration.devicewatcher.enumerationcompleted)et [**Stopped**](/uwp/api/windows.devices.enumeration.devicewatcher.stopped) .

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetStartWatcherButtonClick":::

L’événement **Added** est déclenché lorsqu’un nouvel appareil est détecté par l’observateur. Dans le gestionnaire de cet événement, créez un objet [**CastingDevice**](/uwp/api/Windows.Media.Casting.CastingDevice) en appelant [**CastingDevice.FromIdAsync**](/uwp/api/windows.media.casting.castingdevice.fromidasync) et en transmettant l’ID de l’appareil de diffusion détecté, qui est contenu dans l’objet **DeviceInformation** transmis au gestionnaire.

Ajoutez le **CastingDevice** à l’appareil de diffusion **ListBox** afin que l’utilisateur puisse le sélectionner. En raison du [**ItemTemplate**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) défini dans le XAML, la propriété [**FriendlyName**](/uwp/api/windows.media.casting.castingdevice.friendlyname) sera utilisée comme texte de l’élément dans la zone de liste. Étant donné que ce gestionnaire d’événements n’est pas appelé sur le thread d’interface utilisateur, vous devez mettre à jour l’interface utilisateur à partir d’un appel à [**CoreDispatcher. RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetWatcherAdded":::

L’événement **Removed** est déclenché lorsque l’observateur détecte qu’un appareil de diffusion n’est plus présent. Comparez la propriété de l’ID de l’objet **Added** transmis par le biais du gestionnaire à l’ID de chaque **Added** de la collection [**Items**](/uwp/api/windows.ui.xaml.controls.itemscontrol.items) de la zone de liste. Si l’ID correspond, supprimez cet objet de la collection. Là encore, dans la mesure où l’interface utilisateur est mise à jour, cet appel doit être effectué depuis un appel **RunAsync**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetWatcherRemoved":::

L’événement **EnumerationCompleted** est déclenché lorsque l’observateur a terminé la détection des appareils. Dans le gestionnaire de cet événement, mettez à jour l’interface utilisateur pour permettre à l’utilisateur de savoir que l’énumération des appareils est terminée et arrêtez l’observateur d’appareils en appelant [**Stop**](/uwp/api/windows.devices.enumeration.devicewatcher.stop).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetWatcherEnumerationCompleted":::

L’événement Stopped est déclenché lorsque l’observateur d’appareils a terminé l’arrêt. Dans le gestionnaire de cet événement, arrêtez le contrôle [**ProgressRing**](/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) et réactivez le paramètre *startWatcherButton* afin que l’utilisateur puisse redémarrer le processus d’énumération des appareils.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetWatcherStopped":::

L’événement [**SelectionChanged**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) est déclenché, dès que l’utilisateur sélectionne un des appareils de diffusion à partir de la zone de liste. La création de la connexion de la diffusion et le démarrage de la diffusion s’effectuent dans ce gestionnaire.

Tout d’abord, assurez-vous que l’observateur d’appareils est arrêté afin que l’énumération des appareils n’interfère pas avec la diffusion multimédia. Créez une connexion de diffusion en appelant [**CreateCastingConnection**](/uwp/api/windows.media.casting.castingdevice.createcastingconnection) sur l’objet **CastingDevice** sélectionné par l’utilisateur. Ajoutez des gestionnaires d’événements pour les événements [**StateChanged**](/uwp/api/windows.media.casting.castingconnection.statechanged) et [**ErrorOccurred**](/uwp/api/windows.media.casting.castingconnection.erroroccurred) .

Lancez la diffusion multimédia en appelant [**RequestStartCastingAsync**](/uwp/api/windows.media.casting.castingconnection.requeststartcastingasync) et en transmettant la source de diffusion renvoyée en appelant la méthode [**GetAsCastingSource**](/uwp/api/windows.ui.xaml.controls.mediaelement.getascastingsource) de **MediaPlayer**. Enfin, faites en sorte que le bouton de déconnexion soit visible pour permettre à l’utilisateur d’arrêter la diffusion multimédia.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetSelectionChanged":::

Dans le gestionnaire de changement d’état, l’action effectuée dépend du nouvel état de la connexion de diffusion :

-   Si l’état est **Connected** ou **Rendering**, assurez-vous que le contrôle **ProgressRing** est inactif et que le bouton de déconnexion est visible.
-   Si l’état est **Disconnected**, désélectionnez l’appareil de diffusion actuel dans la zone de liste, assurez-vous que le contrôle **ProgressRing** est inactif et masquez le bouton de déconnexion.
-   Si l’état est **Connecting**, activez le contrôle **ProgressRing** et masquez le bouton de déconnexion.
-   Si l’état est **Disconnecting**, activez le contrôle **ProgressRing** et masquez le bouton de déconnexion.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetStateChanged":::

Dans le gestionnaire de l’événement **ErrorOccurred**, mettez à jour votre interface utilisateur pour informer l’utilisateur qu’une erreur de diffusion s’est produite et désélectionnez l’objet **CastingDevice** dans la zone de liste.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetErrorOccurred":::

Enfin, implémentez le gestionnaire pour le bouton de déconnexion. Arrêtez la diffusion multimédia et déconnectez l’appareil de diffusion en appelant la méthode [**DisconnectAsync**](/uwp/api/windows.media.casting.castingconnection.disconnectasync) de l’objet **CastingConnection**. Cet appel doit être transmis au thread d’interface utilisateur en appelant [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetDisconnectButton":::

 

 
