---
ms.assetid: 708170E1-777A-4E4A-9F77-5AB28B88B107
description: Cet article vous montre comment utiliser les contrôles des appareils manuels pour activer les scénarios de capture vidéo, y compris la vidéo HDR et la priorité de l’exposition.
title: Contrôles d’appareil photo manuel pour la capture vidéo
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ac3a286a8e3961b66a8fd0e4cf20fa7665b6b081
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364012"
---
# <a name="manual-camera-controls-for-video-capture"></a>Contrôles d’appareil photo manuel pour la capture vidéo



Cet article vous montre comment utiliser les contrôles des appareils manuels pour activer les scénarios de capture vidéo, y compris la vidéo HDR et la priorité de l’exposition.

Les contrôles des appareils vidéo mentionnés dans cet article sont tous ajoutés à votre application en utilisant le même modèle. Tout d’abord, vérifiez si le contrôle est pris en charge sur l’appareil sur lequel votre application est en cours d’exécution. Si le contrôle est pris en charge, définissez le mode de votre choix pour le contrôle. En règle générale, si un contrôle particulier n’est pas pris en charge sur l’appareil actuel, vous devez désactiver ou masquer l’élément d’interface utilisateur qui permet à l’utilisateur d’activer la fonctionnalité.

Toutes les API de contrôle d’appareil décrites dans cet article sont des membres de l’espace de noms [**Windows. Media. Devices**](/uwp/api/Windows.Media.Devices) .

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetVideoControllersUsing":::

> [!NOTE] 
> Cet article repose sur les concepts et le code décrits dans [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), qui décrit comment implémenter la capture photo et vidéo de base. Nous vous recommandons de vous familiariser avec le modèle de capture simple de contenu multimédia de cet article avant d’adopter des scénarios de capture plus avancés. Le code de cet article part du principe que votre application possède déjà une instance de MediaCapture initialisée correctement.

## <a name="hdr-video"></a>Vidéo HDR

La fonctionnalité vidéo HDR (High Dynamic Range) applique le traitement HDR au flux vidéo de l’appareil de capture. Déterminez si la vidéo HDR est prise en charge en vérifiant la propriété [**HdrVideoControl.Supported**](/uwp/api/windows.media.devices.hdrvideocontrol.supported).

Le contrôle vidéo HDR prend en charge trois modes : activé, désactivé et automatique, ce qui signifie que l’appareil détermine dynamiquement si le traitement vidéo HDR est susceptible d’améliorer la capture multimédia et l’active, le cas échéant. Pour déterminer si un mode particulier est pris en charge sur l’appareil actuel, vérifiez si la collection [**HdrVideoControl. SupportedModes**](/uwp/api/windows.media.devices.hdrvideocontrol.supportedmodes) contient le mode souhaité.

Activez ou désactivez le traitement vidéo HDR en définissant le [**HdrVideoControl.Mode**](/uwp/api/windows.media.devices.hdrvideocontrol.mode) sur le mode de votre choix.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetSetHdrVideoMode":::

## <a name="exposure-priority"></a>Priorité d’exposition

La [**ExposurePriorityVideoControl**](/uwp/api/Windows.Media.Devices.ExposurePriorityVideoControl), lorsqu’elle est activée, évalue les images vidéo à partir du périphérique de capture pour déterminer si la vidéo capture une scène de faible luminosité. Dans ce cas, le contrôle réduit la fréquence d’images de la vidéo capturée pour augmenter le temps d’exposition pour chaque image et améliorer la qualité visuelle de la vidéo capturée.

Déterminez si le contrôle de priorité d’exposition est pris en charge sur l’appareil actuel en vérifiant la propriété [**ExposurePriorityVideoControl.Supported**](/uwp/api/windows.media.devices.exposurepriorityvideocontrol.supported).

Activez ou désactivez le contrôle de priorité d’exposition en définissant [**ExposurePriorityVideoControl. Enabled**](/uwp/api/windows.media.devices.exposurepriorityvideocontrol.enabled) sur le mode souhaité.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetEnableExposurePriority":::

## <a name="temporal-denoising"></a>Denoising temporelle
À compter de Windows 10, version 1803, vous pouvez activer la denoising temporelle pour la vidéo sur les appareils qui le prennent en charge. Cette fonctionnalité fusionne les données d’image de plusieurs images adjacentes en temps réel pour produire des images vidéo qui présentent moins de bruit visuel.

Le [**VideoTemporalDenoisingControl**](/uwp/api/windows.media.devices.videotemporaldenoisingcontrol) permet à votre application de déterminer si le denoising temporel est pris en charge sur l’appareil actuel et, le cas échéant, les modes de denoising pris en charge. Les modes denoising disponibles sont [**off**](/uwp/api/windows.media.devices.videotemporaldenoisingmode), [**on**](/uwp/api/windows.media.devices.videotemporaldenoisingmode)et [**auto**](/uwp/api/windows.media.devices.videotemporaldenoisingmode). Un périphérique peut ne pas prendre en charge tous les modes, mais chaque appareil doit prendre en charge l' **option automatique** **ou activé ou** **désactivé**.

L’exemple suivant utilise une interface utilisateur simple pour fournir des cases d’option permettant à l’utilisateur de basculer entre les modes denoising.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetDenoiseXAML":::

Dans la méthode suivante, la propriété [**VideoTemporalDenoisingControl. Supported**](/uwp/api/windows.media.devices.videotemporaldenoisingcontrol.supported) est vérifiée pour déterminer si le denoising temporel est pris en charge sur l’appareil actuel. Si c’est le cas, nous vérifions que **off** et **auto** ou **on** sont pris en charge. dans ce cas, nous rendons les cases d’option visibles. Ensuite, les boutons **auto** et **on** sont rendus visibles si ces méthodes sont prises en charge.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetUpdateDenoiseCapabilities":::

Dans le gestionnaire d’événements **activés** pour les cases d’option, le nom du bouton est activé et le mode correspondant est défini en définissant la propriété [**VideoTemporalDenoisingControl. mode**](/uwp/api/windows.media.devices.videotemporaldenoisingcontrol.mode) .

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetDenoiseButtonChecked":::

### <a name="disabling-temporal-denoising-while-processing-frames"></a>Désactivation des denoising temporelles pendant le traitement des frames
La vidéo qui a été traitée à l’aide de denoising temporelles peut être plus agréable à l’oeil humain. Toutefois, étant donné que les denoising temporelles peuvent avoir un impact sur la cohérence des images et réduire la quantité de détails dans le frame, les applications qui effectuent le traitement d’image sur les frames, telles que l’enregistrement ou la reconnaissance optique de caractères, peuvent désactiver les denoising par programmation lorsque le traitement d’image est activé.

L’exemple suivant détermine quels modes denoising sont pris en charge et stocke ces informations dans certaines variables de classe.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetDenoiseFrameReaderVars":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetDenoiseCapabilitiesForFrameProcessing":::

Lorsque l’application active le traitement des frames, elle définit le mode denoising sur **off** si ce mode est pris en charge, de sorte que le traitement des frames peut utiliser des frames bruts qui n’ont pas été débruyants.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetEnableFrameProcessing":::

Lorsque l’application désactive le prcessing de frame, elle définit le mode denoising **sur on** ou **auto**, selon le mode pris en charge.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetDisableFrameProcessing":::

Pour plus d’informations sur l’obtention de trames vidéo pour le traitement d’images, consultez [traiter des frames multimédias avec MediaFrameReader](process-media-frames-with-mediaframereader.md).

## <a name="related-topics"></a>Rubriques connexes

* [Appareil photo](camera.md)
* [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Traiter des images multimédias avec MediaFrameReader](process-media-frames-with-mediaframereader.md)
*  [**VideoTemporalDenoisingControl**](/uwp/api/windows.media.devices.videotemporaldenoisingcontrol)
 
