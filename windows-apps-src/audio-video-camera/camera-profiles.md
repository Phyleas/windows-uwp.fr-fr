---
ms.assetid: 42A06423-670F-4CCC-88B7-3DCEEDDEBA57
description: Cet article explique comment utiliser les profils d’appareil photo pour découvrir et gérer les capacités des différents appareils de capture vidéo. Cela inclut les tâches telles que la sélection des profils qui prennent en charge des résolutions ou de fréquences d’images spécifiques, des profils qui prennent en charge un accès simultané à plusieurs appareils photos et des profils qui prennent en charge la capture HDR.
title: Détecter et sélectionner des fonctions de l’appareil photo à l’aide de profils d’appareil photo
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f42b58794b62753ff325cb4fc23202e5a48047d4
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364022"
---
# <a name="discover-and-select-camera-capabilities-with-camera-profiles"></a>Détecter et sélectionner des fonctions de l’appareil photo à l’aide de profils d’appareil photo



Cet article explique comment utiliser les profils d’appareil photo pour découvrir et gérer les capacités des différents appareils de capture vidéo. Cela inclut les tâches telles que la sélection des profils qui prennent en charge des résolutions ou de fréquences d’images spécifiques, des profils qui prennent en charge un accès simultané à plusieurs appareils photos et des profils qui prennent en charge la capture HDR.

> [!NOTE] 
> Cet article repose sur les concepts et le code décrits dans [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), qui décrit comment implémenter la capture photo et vidéo de base. Nous vous recommandons de vous familiariser avec le modèle de capture multimédia de base dans cet article avant de passer à des scénarios de capture plus avancés. Le code de cet article part du principe que votre application possède déjà une instance de MediaCapture initialisée correctement.

 

## <a name="about-camera-profiles"></a>À propos des profils d’appareil photo

Les appareils photo offrent différentes fonctionnalités, notamment l’ensemble des résolutions de capture, la fréquence d’images pour la capture vidéo, et si les vidéos HDR ou les captures de fréquence d’images variable sont prises en charge. L’infrastructure de capture de média plateforme Windows universelle (UWP) stocke cet ensemble de fonctionnalités dans un [**MediaCaptureVideoProfileMediaDescription**](/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfileMediaDescription). Un profil d’appareil photo, représenté par un objet [**MediaCaptureVideoProfile**](/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfile), comporte trois ensembles de descriptions du média : un premier pour la capture photo, un deuxième pour la capture vidéo et un dernier pour l’aperçu vidéo.

Avant d’initialiser votre objet [MediaCapture](./index.md), vous pouvez interroger les appareils de capture de l’appareil actuel pour découvrir les profils pris en charge. Lorsque vous sélectionnez un profil pris en charge, vous savez que l’appareil de capture prend en charge toutes les fonctionnalités dans les descriptions du média du profil. Cela permet de déterminer directement les combinaisons de capacités prises en charge sur un appareil particulier.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetBasicInitExample":::

Les exemples de code dans cet article remplacent cette initialisation minimale par la découverte de profils d’appareil photo prenant en charge différentes fonctionnalités, qui sont ensuite utilisées pour initialiser l’appareil de capture multimédia.

## <a name="find-a-video-device-that-supports-camera-profiles"></a>Trouver un appareil vidéo prenant en charge des profils d’appareil photo

Avant de rechercher des profils d’appareil photo pris en charge, vous devez trouver un appareil de capture qui prend en charge l’utilisation de profils d’appareil photo. La méthode d’assistance **GetVideoProfileSupportedDeviceIdAsync** définie dans l’exemple ci-dessous utilise la méthode [**DeviceInformaion.FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) pour récupérer la liste de tous les appareils de capture vidéo disponibles. Elle parcourt tous les appareils de la liste en appelant la méthode statique [**IsVideoProfileSupported**](/uwp/api/windows.media.capture.mediacapture.isvideoprofilesupported) pour chaque appareil afin de déterminer s’il prend en charge les profils vidéo. En outre, la propriété [**EnclosureLocation.Panel**](/uwp/api/windows.devices.enumeration.enclosurelocation.panel) vous permet de spécifier pour chaque appareil si vous préférez que l’appareil photo se situe à l’avant ou à l’arrière.

Si un appareil prenant en charge des profils d’appareil photo est détecté dans le panneau de configuration spécifié, la valeur [**Id**](/uwp/api/windows.devices.enumeration.deviceinformation.id) contenant la chaîne d’ID de l’appareil, est renvoyée.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetGetVideoProfileSupportedDeviceIdAsync":::

Si l’ID d’appareil renvoyé par la méthode d’assistance **GetVideoProfileSupportedDeviceIdAsync** est null ou une chaîne vide, il n’existe aucun appareil sur le panneau de configuration spécifié prenant en charge des profils d’appareil photo. Dans ce cas, vous devez initialiser votre appareil de capture multimédia sans utiliser de profils.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetGetDeviceWithProfileSupport":::

## <a name="select-a-profile-based-on-supported-resolution-and-frame-rate"></a>Sélectionner un profil en fonction de la résolution et de la fréquence d’images prises en charge

Pour sélectionner un profil avec des fonctionnalités particulières, comme la possibilité d’atteindre une résolution et une fréquence d’images données, vous devez d’abord appeler la méthode d’assistance définie ci-dessus pour obtenir l’ID d’un appareil de capture qui prend en charge l’utilisation des profils de l’appareil photo.

Créez un objet [**MediaCaptureInitializationSettings**](/uwp/api/Windows.Media.Capture.MediaCaptureInitializationSettings), en passant l’ID d’appareil sélectionné. Ensuite, appelez la méthode statique [**MediaCapture. FindAllVideoProfiles**](/uwp/api/windows.media.capture.mediacapture.findallvideoprofiles) pour obtenir la liste de tous les profils d’appareil photo pris en charge par l’appareil.

Cet exemple utilise une méthode de requête Linq, incluse dans l’espace de noms using **System.Linq**, pour sélectionner un profil qui contient un objet [**SupportedRecordMediaDescription**](/uwp/api/windows.media.capture.mediacapturevideoprofile.supportedrecordmediadescription) dans lequel les propriétés [**Width**](/uwp/api/windows.media.capture.mediacapturevideoprofilemediadescription.width), [**Height**](/uwp/api/windows.media.capture.mediacapturevideoprofilemediadescription.height) et [**FrameRate**](/uwp/api/windows.media.capture.mediacapturevideoprofilemediadescription.framerate) correspondent aux valeurs demandées. Si une correspondance est trouvée, [**VideoProfile**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.videoprofile) et [**RecordMediaDescription**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.recordmediadescription) du **MediaCaptureInitializationSettings** sont définies sur les valeurs de type anonyme renvoyées par la requête Linq. Si aucune correspondance n’est trouvée, le profil par défaut est utilisé.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetFindWVGA30FPSProfile":::

Une fois que vous avez rempli **MediaCaptureInitializationSettings** avec le profil d’appareil photo souhaité, il vous suffit d’appeler [**InitializeAsync**](/uwp/api/windows.media.capture.mediacapture.initializeasync) sur l’objet de capture multimédia pour le configurer sur le profil de votre choix.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetInitCaptureWithProfile":::

## <a name="use-media-frame-source-groups-to-get-profiles"></a>Utiliser des groupes de sources de cadre de média pour récupérer des profils

À compter de Windows 10, version 1803, vous pouvez utiliser la classe [**MediaFrameSourceGroup**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup) pour récupérer des profils d’appareil photo avec des fonctionnalités spécifiques avant d’initialiser l’objet **MediaCapture** . Les groupes de sources de frame permettent aux fabricants de périphériques de représenter des groupes de capteurs ou des fonctionnalités de capture comme un seul appareil virtuel. Cela permet des scénarios de photographie de calcul tels que l’utilisation conjointe d’appareils photo de profondeur et de couleur, mais peut également être utilisé pour sélectionner des profils d’appareil photo pour des scénarios de capture simples. Pour plus d’informations sur l’utilisation de **MediaFrameSourceGroup**, consultez [traiter des frames multimédias avec MediaFrameReader](process-media-frames-with-mediaframereader.md).

L’exemple de méthode ci-dessous montre comment utiliser des objets **MediaFrameSourceGroup** pour rechercher un profil de caméra qui prend en charge un profil vidéo connu, tel qu’un profil qui prend en charge HDR ou une séquence de photos variable. Tout d’abord, appelez [**MediaFrameSourceGroup. FindAllAsync**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.findallasync) pour obtenir la liste de tous les groupes de sources de frame de média disponibles sur l’appareil actuel. Parcourez chaque groupe source et appelez [**MediaCapture. FindKnownVideoProfiles**](/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles) pour obtenir la liste de tous les profils vidéo du groupe source actuel qui prend en charge le profil spécifié, en l’occurrence HDR avec WCG photo. Si un profil qui répond aux critères est trouvé, créez un nouvel objet **MediaCaptureInitializationSettings** et définissez le **VideoProfile** sur le profil SELECT et le **VideoDeviceId** sur la propriété **ID** du groupe de sources frame de média actuel. Par exemple, vous pouvez passer la valeur **KnownVideoProfile. HdrWithWcgVideo** dans cette méthode pour obtenir les paramètres de capture de média qui prennent en charge la vidéo HDR. Transmettez **KnownVideoProfile. VariablePhotoSequence** pour récupérer les paramètres qui prennent en charge la séquence de photos variable.

 :::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetFindKnownVideoProfile":::

## <a name="use-known-profiles-to-find-a-profile-that-supports-hdr-video-legacy-technique"></a>Utiliser des profils connus pour trouver un profil qui prend en charge la vidéo HDR (technique héritée)

> [!NOTE] 
> Les API décrites dans cette section sont déconseillées à compter de Windows 10, version 1803. Consultez la section précédente, **utiliser des groupes de sources de cadre de média pour obtenir des profils**.

La sélection d’un profil prenant en charge la vidéo HDR commence comme tous les autres scénarios. Créez un **MediaCaptureInitializationSettings** et une chaîne pour contenir l’ID de l’appareil de capture. Ajoutez une variable booléenne qui déterminera si la vidéo HDR est prise en charge.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetGetHdrProfileSetup":::

Utilisez la méthode d’assistance **GetVideoProfileSupportedDeviceIdAsync** définie ci-dessus pour obtenir l’ID d’un appareil de capture prenant en charge les profils d’appareil photo.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetFindDeviceHDR":::

La méthode statique [**MediaCapture. FindKnownVideoProfiles**](/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles) retourne les profils d’appareil photo pris en charge par l’appareil spécifié et catégorisés par la valeur [**KnownVideoProfile**](/uwp/api/Windows.Media.Capture.KnownVideoProfile) spécifiée. Pour ce scénario, la valeur **VideoRecording** est spécifiée de manière à limiter les profils d’appareil photo renvoyés à ceux prenant en charge l’enregistrement vidéo.

Parcourez la liste renvoyée des profils d’appareil photo. Pour chaque profil d’appareil photo, parcourez chaque [**VideoProfileMediaDescription**](/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfileMediaDescription) de la vérification du profil pour voir si la propriété [**IsHdrVideoSupported**](/uwp/api/windows.media.capture.mediacapturevideoprofilemediadescription.ishdrvideosupported) a la valeur true. Une fois qu’une description du média appropriée est détectée, quittez la boucle et attribuez le profil et les objets de description à l’objet **MediaCaptureInitializationSettings**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetFindHDRProfile":::

## <a name="determine-if-a-device-supports-simultaneous-photo-and-video-capture"></a>Déterminer si un appareil prend en charge la capture simultanée de photo et de vidéo

De nombreux appareils prennent en charge la capture simultanée de photos et de vidéos. Pour déterminer si un périphérique de capture prend cela en charge, appelez [**MediaCapture. FindAllVideoProfiles**](/uwp/api/windows.media.capture.mediacapture.findallvideoprofiles) pour obtenir tous les profils d’appareil photo pris en charge par l’appareil. Utilisez une requête Linq pour rechercher un profil comportant au moins une entrée pour [**SupportedPhotoMediaDescription**](/uwp/api/windows.media.capture.mediacapturevideoprofile.supportedphotomediadescription) et pour [**SupportedRecordMediaDescription**](/uwp/api/windows.media.capture.mediacapturevideoprofile.supportedrecordmediadescription), ce qui indiquera que le profil prend en charge la capture simultanée.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetGetPhotoAndVideoSupport":::

Vous pouvez affiner la requête pour rechercher des profils qui prennent en charge des résolutions spécifiques ou autres fonctionnalités en plus de l’enregistrement vidéo simultané. Vous pouvez également utiliser [**MediaCapture. FindKnownVideoProfiles**](/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles) et spécifier la valeur [**BalancedVideoAndPhoto**](/uwp/api/Windows.Media.Capture.KnownVideoProfile) pour récupérer des profils qui prennent en charge la capture simultanée, mais l’interrogation de tous les profils fournit des résultats plus complets.

## <a name="related-topics"></a>Rubriques connexes

* [Appareil photo](camera.md)
* [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 
