---
ms.assetid: 831123A7-1F40-4B74-AE9F-69AC9883B4AD
description: Cet article vous montre comment utiliser les contrôles des appareils manuels pour activer les scénarios de capture photo et vidéo, y compris la stabilisation d’image optique et le zoom fluide.
title: Contrôles d’appareil photo manuel pour la capture photo et vidéo
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d7248b4f3fe515a164410305bac074f44cae53e6
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362862"
---
# <a name="manual-camera-controls-for-photo-and-video-capture"></a>Contrôles d’appareil photo manuel pour la capture photo et vidéo



Cet article vous montre comment utiliser les contrôles des appareils manuels pour activer les scénarios de capture photo et vidéo, y compris la stabilisation d’image optique et le zoom fluide.

Les contrôles mentionnés dans cet article sont tous ajoutés à votre application en utilisant le même modèle. Tout d’abord, vérifiez si le contrôle est pris en charge sur l’appareil sur lequel votre application est en cours d’exécution. Si le contrôle est pris en charge, définissez le mode de votre choix pour le contrôle. En règle générale, si un contrôle particulier n’est pas pris en charge sur l’appareil actuel, vous devez désactiver ou masquer l’élément d’interface utilisateur qui permet à l’utilisateur d’activer la fonctionnalité.

Le code figurant dans cet article a été adapté à partir de l’exemple [Kit de développement logiciel (SDK) de contrôles manuels d’appareil photo](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraManualControls). Vous pouvez télécharger l’exemple pour voir le code utilisé en contexte ou pour vous en servir comme point de départ pour votre propre application.

> [!NOTE]
> Cet article repose sur les concepts et le code décrits dans [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), qui décrit comment implémenter la capture photo et vidéo de base. Nous vous recommandons de vous familiariser avec le modèle de capture simple de contenu multimédia de cet article avant d’adopter des scénarios de capture plus avancés. Le code de cet article part du principe que votre application possède déjà une instance de MediaCapture initialisée correctement.

Toutes les API de contrôle d’appareil décrites dans cet article sont des membres de l’espace de noms [**Windows. Media. Devices**](/uwp/api/Windows.Media.Devices) .

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetVideoControllersUsing":::

## <a name="exposure"></a>Exposition :

[**ExposureControl**](/uwp/api/Windows.Media.Devices.ExposureControl) vous permet de définir la vitesse d’obturation utilisée pendant la capture photo ou vidéo.

Cet exemple utilise un contrôle [**Slider**](/uwp/api/Windows.UI.Xaml.Controls.Slider) pour ajuster la valeur d’exposition actuelle et une case à cocher pour activer ou désactiver le réglage automatique de l’exposition.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetExposureXAML":::

Vérifiez si l’appareil de capture actuel prend en charge **ExposureControl** en vérifiant la propriété [**Supported**](/uwp/api/windows.media.devices.exposurecontrol.supported). Si le contrôle est pris en charge, vous pouvez afficher et activer l’interface utilisateur de cette fonctionnalité. Définissez l’état activé de la case à cocher pour indiquer si l’ajustement automatique de l’exposition est actuellement actif à la valeur de la propriété [**auto**](/uwp/api/windows.media.devices.exposurecontrol.auto) .

La valeur d’exposition doit être comprise dans la plage prise en charge par l’appareil et doit être un incrément de la taille de pas prise en charge. Obtenir les valeurs prises en charge pour l’appareil actuel en vérifiant les propriétés [**min**](/uwp/api/windows.media.devices.exposurecontrol.min), [**Max**](/uwp/api/windows.media.devices.exposurecontrol.max)et [**Step**](/uwp/api/windows.media.devices.exposurecontrol.step) , qui sont utilisées pour définir les propriétés correspondantes du contrôle Slider.

Définissez la valeur du contrôle de curseur sur la valeur actuelle de **ExposureControl** après la suppression de l’enregistrement du gestionnaire d’événements [**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) afin que l’événement ne soit pas déclenché lorsque la valeur est définie.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetExposureControl":::

Dans le gestionnaire d’événements **ValueChanged**, obtenez la valeur actuelle du contrôle et définissez la valeur d’exposition en appelant [**SetValueAsync**](/uwp/api/windows.media.devices.exposurecontrol.setvalueasync).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetExposureSlider":::

Dans le gestionnaire d’événements **CheckedChanged** de la case à cocher d’exposition automatique, activez ou désactivez le réglage d’exposition automatique en appelant [**SetAutoAsync**](/uwp/api/windows.media.devices.exposurecontrol.setautoasync) et en indiquant une valeur booléenne.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetExposureCheckBox":::

> [!IMPORTANT]
> Le mode d’exposition automatique est uniquement pris en charge pendant l’exécution du flux d’aperçu. Vérifiez que le flux d’aperçu est en cours d’exécution avant d’activer l’exposition automatique.

## <a name="exposure-compensation"></a>Compensation de l’exposition

[**ExposureCompensationControl**](/uwp/api/Windows.Media.Devices.ExposureCompensationControl) vous permet de définir la compensation d’exposition utilisée pendant la capture photo ou vidéo.

Cet exemple utilise un contrôle [**Slider**](/uwp/api/Windows.UI.Xaml.Controls.Slider) pour ajuster la valeur actuelle de compensation de l’exposition.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetEvXAML":::

Vérifiez si l’appareil de capture actuel prend en charge **ExposureCompensationControl** en vérifiant la propriété [Supported](supported-codecs.md). Si le contrôle est pris en charge, vous pouvez afficher et activer l’interface utilisateur de cette fonctionnalité.

La valeur de compensation d’exposition doit être comprise dans la plage prise en charge par l’appareil et doit être un incrément de la taille de pas prise en charge. Obtenir les valeurs prises en charge pour l’appareil actuel en vérifiant les propriétés [**min**](/uwp/api/windows.media.devices.exposurecompensationcontrol.min), [**Max**](/uwp/api/windows.media.devices.exposurecompensationcontrol.max)et [**Step**](/uwp/api/windows.media.devices.exposurecompensationcontrol.step) , qui sont utilisées pour définir les propriétés correspondantes du contrôle Slider.

Définissez la valeur du contrôle de curseur sur la valeur actuelle de **ExposureCompensationControl** après la suppression de l’enregistrement du gestionnaire d’événements [**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) afin que l’événement ne soit pas déclenché lorsque la valeur est définie.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetEvControl":::

Dans le gestionnaire d’événements **ValueChanged**, obtenez la valeur actuelle du contrôle et définissez la valeur d’exposition en appelant [**SetValueAsync**](/uwp/api/windows.media.devices.exposurecompensationcontrol.setvalueasync).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetEvValueChanged":::

## <a name="flash"></a>Clignote

[**FlashControl**](/uwp/api/Windows.Media.Devices.FlashControl) vous permet d’activer ou de désactiver le flash ou d’activer le flash automatique, auquel cas le système détermine de manière dynamique l’utilisation ou non du flash. Ce contrôle vous permet également d’activer la réduction automatique des yeux rouges sur les appareils la prenant en charge. Ces paramètres s’appliquent tous à la capture de photos. [**TorchControl**](/uwp/api/Windows.Media.Devices.TorchControl) est un autre contrôle d’activation ou de désactivation de la torche pour la capture vidéo.

Cet exemple utilise un ensemble de cases d’option permettant à l’utilisateur de basculer entre les paramètres d’activation, de désactivation et de flash automatique. Une case à cocher permet en outre d’activer/de désactiver la réduction des yeux rouges et la torche vidéo.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetFlashXAML":::

Vérifiez si l’appareil de capture actuel prend en charge **FlashControl** en vérifiant la propriété [**Supported**](/uwp/api/windows.media.devices.focuscontrol.supported). Si le contrôle est pris en charge, vous pouvez afficher et activer l’interface utilisateur de cette fonctionnalité. Si **FlashControl** est pris en charge, la réduction automatique des yeux rouges peut être prise en charge ou non. Par conséquent, vérifiez la propriété [**RedEyeReductionSupported**](/uwp/api/windows.media.devices.flashcontrol.redeyereductionsupported) avant d’activer l’interface utilisateur. **TorchControl** étant distinct du contrôle de flash, vous devez également vérifier sa propriété [**Supported**](/uwp/api/windows.media.devices.torchcontrol.supported) avant de l’utiliser.

Dans le gestionnaire d’événements [**Checked**](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.checked) de chaque case d’option du flash, activez ou désactivez le paramètre de flash correspondant approprié. Notez que pour définir le flash toujours actif, vous devez définir la propriété [**Enabled**](/uwp/api/windows.media.devices.flashcontrol.enabled) sur true et la propriété [**Auto**](/uwp/api/windows.media.devices.flashcontrol.auto) sur false.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetFlashControl":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetFlashRadioButtons":::

Dans le gestionnaire de la case à cocher de la réduction des yeux rouges, définissez la propriété [**RedEyeReduction**](/uwp/api/windows.media.devices.flashcontrol.redeyereduction) sur la valeur appropriée.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetRedEye":::

Enfin, dans la case à cocher Gestionnaire de la vidéo, affectez la valeur appropriée à la propriété [**Enabled**](/uwp/api/windows.media.devices.torchcontrol.enabled) .

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetTorch":::

> [!NOTE] 
>  Sur certains appareils, l’chalumeau n’émet pas de lumière, même si [**TorchControl. Enabled**](/uwp/api/windows.media.devices.torchcontrol.enabled) a la valeur true, sauf si l’appareil a un flux de préversion en cours d’exécution et qu’il capture activement de la vidéo. L’ordre des opérations recommandé consiste à activer l’aperçu vidéo, à activer la torche en définissant **Enabled** sur true, puis à lancer la capture vidéo. Sur certains appareils, la torche s’allume après le démarrage de l’aperçu. Sur d’autres appareils, la torche peut ne pas s’allumer tant qu’une capture vidéo n’est pas démarrée.

## <a name="focus"></a>Priorité

Trois méthodes couramment utilisées pour le réglage de la mise au point de l’appareil photo sont prises en charge par l’objet [**FocusControl**](/uwp/api/Windows.Media.Devices.FocusControl) : l’autofocus en continu, appuyer pour mettre au point la mise au point manuelle. Une application d’appareil photo peut prendre en charge ces méthodes, mais pour une meilleure lisibilité, cet article traite de chaque technique séparément. Cette section décrit également comment activer la lumière d’aide à la mise au point.

### <a name="continuous-autofocus"></a>Autofocus en continu

L’activation de l’autofocus en continu indique à l’appareil photo qu’il doit régler la mise au point de manière dynamique afin de maintenir la mise au point sur le sujet de la photo ou vidéo. Cet exemple utilise une case d’option d’activation/de désactivation de l’autofocus en continu.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetCAFXAML":::

Vérifiez si l’appareil de capture actuel prend en charge **FocusControl** en vérifiant la propriété [**Supported**](/uwp/api/windows.media.devices.flashcontrol.supported). Ensuite, déterminez si la focalisation automatique continue est prise en charge en consultant la liste [**SupportedFocusModes**](/uwp/api/windows.media.devices.focuscontrol.supportedfocusmodes) pour voir si elle contient la valeur [**FocusMode. Continuous**](/uwp/api/Windows.Media.Devices.FocusMode)et, le cas échéant, affichez la case d’option autofocus continue.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetCAF":::

Dans le gestionnaire d’événements [**Checked**](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.checked) de la case d’option d’autofocus en continu, utilisez la propriété [**VideoDeviceController.FocusControl**](/uwp/api/windows.media.devices.videodevicecontroller.focuscontrol) pour obtenir une instance du contrôle. Appelez [**UnlockAsync**](/uwp/api/windows.media.devices.focuscontrol.unlockasync) pour déverrouiller le contrôle si votre application a appelé [**LockAsync**](/uwp/api/windows.media.devices.focuscontrol.lockasync) précédemment afin d’activer l’un des autres modes de mise au point.

Créez un nouvel objet [**FocusSettings**](/uwp/api/Windows.Media.Devices.FocusSettings) et définissez la propriété [**Mode**](/uwp/api/windows.media.devices.focussettings.mode) sur **Continuous**. Affectez à la propriété [**AutoFocusRange**](/uwp/api/windows.media.devices.focussettings.autofocusrange) une valeur appropriée pour votre scénario d’application ou sélectionnée par l’utilisateur à partir de votre interface utilisateur. Transmettez votre objet **FocusSettings** à la méthode [**Configure**](/uwp/api/windows.media.devices.focuscontrol.configure), puis appelez [**FocusAsync**](/uwp/api/windows.media.devices.focuscontrol.focusasync) pour démarrer l’autofocus en continu.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetCafFocusRadioButton":::

> [!IMPORTANT]
> Le mode d’autofocus est uniquement pris en charge pendant l’exécution du flux d’aperçu. Vérifiez que le flux d’aperçu est en cours d’exécution avant d’activer l’autofocus en continu.

### <a name="tap-to-focus"></a>Appuyer pour mettre au point

La technique appuyer pour mettre au point utilise [**FocusControl**](/uwp/api/Windows.Media.Devices.FocusControl) et [**RegionsOfInterestControl**](/uwp/api/Windows.Media.Devices.RegionsOfInterestControl) pour spécifier une sous-région de la trame de capture dans laquelle l’appareil de capture doit mettre au point. La région de mise au point est déterminée par l’utilisateur lorsqu’il appuie sur l’écran affichant le flux d’aperçu.

Cet exemple utilise une case d’option d’activation et de désactivation du mode appuyer pour mettre au point.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetTapFocusXAML":::

Vérifiez si l’appareil de capture actuel prend en charge **FocusControl** en vérifiant la propriété [**Supported**](/uwp/api/windows.media.devices.flashcontrol.supported). **RegionsOfInterestControl** doit être pris en charge, et doit prendre en charge au moins une région, pour pouvoir utiliser cette technique. Vérifiez les propriétés [**AutoFocusSupported**](/uwp/api/windows.media.devices.regionsofinterestcontrol.autofocussupported) et [**MaxRegions**](/uwp/api/windows.media.devices.regionsofinterestcontrol.maxregions) pour déterminer s’il faut afficher ou masquer la case d’option appuyer pour mettre au point.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetTapFocus":::

Dans le gestionnaire d’événements [**activé**](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.checked) pour la case d’option tap-to-focus, utilisez la propriété [**VideoDeviceController. FocusControl**](/uwp/api/windows.media.devices.videodevicecontroller.focuscontrol) pour obtenir une instance du contrôle. Appelez [**LockAsync**](/uwp/api/windows.media.devices.focuscontrol.lockasync) pour verrouiller le contrôle si votre application a précédemment appelé [**UnlockAsync**](/uwp/api/windows.media.devices.focuscontrol.unlockasync) pour activer la focalisation autofocus, puis attendez que l’utilisateur appuie sur l’écran pour changer le focus.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetTapFocusRadioButton":::

Cet exemple met au point sur une région lorsque l’utilisateur appuie sur l’écran, puis supprime la mise au point de cette région lorsque l’utilisateur appuie à nouveau, comme une bascule. Utilisez une variable booléenne pour suivre l’état activé/désactivé.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetIsFocused":::

L’étape suivante consiste à écouter l’événement lorsque l’utilisateur appuie sur l’écran en gérant l’événement [**taraudé**](/uwp/api/windows.ui.xaml.uielement.tapped) du [**CaptureElement**](/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) qui affiche actuellement le flux d’aperçu de capture. Si l’appareil photo n’est pas en cours d’aperçu ou si le mode appuyer pour mettre au point est désactivé, quittez le gestionnaire sans rien faire.

Si la variable de suivi * \_ isFocused* est basculée sur false et si l’appareil photo n’est pas actuellement actif (déterminé par la propriété [**FocusState**](/uwp/api/windows.media.devices.focuscontrol.focusstate) de **FocusControl**), commencez le processus tap-to-focus. Obtenez la position sur laquelle l’utilisateur a appuyé dans les arguments d’événement transmis au gestionnaire. Cet exemple illustre également la possibilité de sélectionner la taille de la région qui sera mise au point. Dans ce cas, la taille correspond à 1/4 de la plus petite dimension de l’élément de capture. Indiquez la position d’appui et la taille de la région dans la méthode d’assistance **TapToFocus** définie dans la section suivante.

Si le bouton bascule * \_ isFocused* a la valeur true, l’appui de l’utilisateur doit effacer le focus de la région précédente. Cette opération est effectuée dans la méthode d’assistance **TapUnfocus** illustrée ci-dessous.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetTapFocusPreviewControl":::

Dans la méthode d’assistance **TapToFocus** , affectez d’abord la valeur true à la propriété * \_ isFocused* de manière à ce que le TAP d’écran suivant libère le focus de la région exploitée.

La tâche suivante de cette méthode d’assistance consiste à déterminer le rectangle dans le flux d’aperçu auquel le contrôle de mise au point sera affecté. Cela nécessite deux étapes. La première étape consiste à déterminer le rectangle que le flux d’aperçu occupe dans le contrôle [**CaptureElement**](/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) . Il dépend des dimensions du flux d’aperçu et de l’orientation de l’appareil. La méthode d’assistance **GetPreviewStreamRectInControl**, comme illustrée à la fin de cette section, exécute cette tâche et retourne le rectangle contenant le flux d’aperçu.

La tâche suivante de **TapToFocus** consiste à convertir l’emplacement activé et la taille du rectangle de mise au point souhaitée, qui ont été déterminés dans le gestionnaire d’événements **CaptureElement.Tapped**, en coordonnées dans le flux de capture. La méthode d’assistance **ConvertUiTapToPreviewRect**, présentée plus loin dans cette section, exécute cette conversion et retourne le rectangle, dans les coordonnés du flux de capture, où la mise au point sera demandée.

Maintenant que vous disposez du rectangle cible, créez un objet [**RegionOfInterest**](/uwp/api/Windows.Media.Devices.RegionOfInterest), en définissant la propriété [**Bounds**](/uwp/api/windows.media.devices.regionofinterest.bounds) sur le rectangle cible obtenu aux étapes précédentes.

Obtenir le [**FocusControl**](/uwp/api/Windows.Media.Devices.FocusControl)de l’appareil de capture. Créez un objet [**FocusSettings**](/uwp/api/Windows.Media.Devices.FocusSettings) et définissez [**Mode**](/uwp/api/windows.media.devices.focuscontrol.mode) et [**AutoFocusRange**](/uwp/api/windows.media.devices.focussettings.autofocusrange) sur les valeurs de votre choix après avoir vérifié qu’ils sont pris en charge par **FocusControl**. Appelez [**Configure**](/uwp/api/windows.media.devices.focuscontrol.configure) sur **FocusControl** pour activer vos paramètres et indiquer à l’appareil de commencer la mise au point sur la région spécifiée.

Obtenez ensuite le [**RegionsOfInterestControl**](/uwp/api/Windows.Media.Devices.RegionsOfInterestControl) de l’appareil de capture et appelez [**SetRegionsAsync**](/uwp/api/windows.media.devices.regionsofinterestcontrol.setregionsasync) pour définir la région active. Plusieurs régions d’intérêt peuvent être définies sur les appareils les prenant en charge, mais cet exemple ne définit qu’une seule région.

Enfin, appelez [**FocusAsync**](/uwp/api/windows.media.devices.focuscontrol.focusasync) sur **FocusControl** pour démarrer la mise au point.

> [!IMPORTANT]
> Quand vous implémentez la technique appuyer pour mettre au point, l’ordre des opérations est important. Vous devez appeler ces API dans l’ordre suivant :
>
> 1. [**FocusControl.Configure**](/uwp/api/windows.media.devices.focuscontrol.configure)
> 2. [**RegionsOfInterestControl.SetRegionsAsync**](/uwp/api/windows.media.devices.regionsofinterestcontrol.setregionsasync)
> 3. [**FocusControl.FocusAsync**](/uwp/api/windows.media.devices.focuscontrol.focusasync)

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetTapToFocus":::

Dans la méthode d’assistance **TapUnfocus**, obtenez **RegionsOfInterestControl** et appelez [**ClearRegionsAsync**](/uwp/api/windows.media.devices.regionsofinterestcontrol.clearregionsasync) pour supprimer la région inscrite avec le contrôle dans la méthode d’assistance **TapToFocus**. Obtenez ensuite **FocusControl** et appelez [**FocusAsync**](/uwp/api/windows.media.devices.focuscontrol.focusasync) pour que l’appareil réexécute la mise au point sans région d’intérêt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetTapUnfocus":::

La méthode d’assistance **GetPreviewStreamRectInControl** utilise la résolution du flux d’aperçu et l’orientation de l’appareil pour déterminer le rectangle dans l’élément d’aperçu qui contient le flux d’aperçu. Elle supprime tout remplissage de cadre que le contrôle peut fournir afin de conserver les proportions du flux. Cette méthode utilise des variables de membre de classe définies dans le code d’exemple de capture de contenu multimédia de base trouvé dans [Capture photo, vidéo et audio à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetGetPreviewStreamRectInControl":::

La méthode d’assistance **ConvertUiTapToPreviewRect** utilise en tant qu’arguments l’emplacement de l’événement d’appui, la taille souhaitée de la région de mise au point et le rectangle contenant le flux d’aperçu obtenus de la méthode d’assistance **GetPreviewStreamRectInControl**. Cette méthode utilise ces valeurs et l’orientation actuelle de l’appareil pour calculer le rectangle dans le flux d’aperçu contenant la région souhaitée. Là encore, cette méthode utilise des variables de membre de classe définies dans le code d’exemple de capture de contenu multimédia de base trouvé dans [Capturer des photos et des vidéos à l’aide de MediaCapture](./index.md).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetConvertUiTapToPreviewRect":::

### <a name="manual-focus"></a>Mise au point manuelle

La technique de mise au point manuelle utilise un contrôle **Slider** pour définir la profondeur de mise au point actuelle de l’appareil de capture. Une case d’option est utilisée pour activer/désactiver la mise au point manuelle.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetManualFocusXAML":::

Vérifiez si l’appareil de capture actuel prend en charge **FocusControl** en vérifiant la propriété [**Supported**](/uwp/api/windows.media.devices.focuscontrol.supported). Si le contrôle est pris en charge, vous pouvez afficher et activer l’interface utilisateur de cette fonctionnalité.

La valeur de mise au point doit être comprise dans la plage prise en charge par l’appareil et doit être un incrément de la taille de pas prise en charge. Obtenir les valeurs prises en charge pour l’appareil actuel en vérifiant les propriétés [**min**](/uwp/api/windows.media.devices.focuscontrol.min), [**Max**](/uwp/api/windows.media.devices.focuscontrol.max)et [**Step**](/uwp/api/windows.media.devices.focuscontrol.step) , qui sont utilisées pour définir les propriétés correspondantes du contrôle Slider.

Définissez la valeur du contrôle de curseur sur la valeur actuelle de **FocusControl** après la suppression de l’enregistrement du gestionnaire d’événements [**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) afin que l’événement ne soit pas déclenché lorsque la valeur est définie.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetFocus":::

Dans le gestionnaire d’événements **Checked** de la case d’option de mise au point manuelle, obtenez l’objet **FocusControl** et appelez [**LockAsync**](/uwp/api/windows.media.devices.focuscontrol.lockasync) si votre application a déverrouillé précédemment la mise au point avec un appel à [**UnlockAsync**](/uwp/api/windows.media.devices.focuscontrol.unlockasync).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetManualFocusChecked":::

Dans le gestionnaire d’événements **ValueChanged** du curseur de mise au point manuelle, obtenez la valeur actuelle du contrôle et définissez la valeur de mise au point en appelant [**SetValueAsync**](/uwp/api/windows.media.devices.focuscontrol.setvalueasync).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetFocusSlider":::

### <a name="enable-the-focus-light"></a>Activer la lumière de mise au point

Sur les appareils qui la prennent en charge, vous pouvez activer une lumière d’aide à la mise au point pour faciliter la mise au point de l’appareil. Cet exemple utilise une case à cocher pour activer ou désactiver la lumière d’aide à la mise au point.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetFocusLightXAML":::

Vérifiez si l’appareil de capture actuel prend en charge **FlashControl** en vérifiant la propriété [**Supported**](/uwp/api/windows.media.devices.flashcontrol.supported). Vérifiez également le [**AssistantLightSupported**](/uwp/api/windows.media.devices.flashcontrol.assistantlightsupported) pour vous assurer que le voyant assistance est également pris en charge. Si les deux sont pris en charge, vous pouvez afficher et activer l’interface utilisateur de cette fonctionnalité.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetFocusLight":::

Dans le gestionnaire d’événements **CheckedChanged**, obtenez l’objet [**FlashControl**](/uwp/api/Windows.Media.Devices.FlashControl) des appareils de capture. Définissez la propriété [**AssistantLightEnabled**](/uwp/api/windows.media.devices.flashcontrol.assistantlightenabled) pour activer ou désactiver la lumière du focus.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetFocusLightCheckBox":::

## <a name="iso-speed"></a>Sensibilité ISO

Le [**IsoSpeedControl**](/uwp/api/Windows.Media.Devices.IsoSpeedControl) vous permet de définir la vitesse ISO utilisée lors de la capture de photos ou de vidéos.

Cet exemple utilise un contrôle [**Slider**](/uwp/api/Windows.UI.Xaml.Controls.Slider) pour ajuster la valeur actuelle de compensation de l’exposition et une case à cocher pour activer/désactiver l’ajustement automatique de la vitesse ISO.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetIsoXAML":::

Vérifiez si l’appareil de capture actuel prend en charge **IsoSpeedControl** en vérifiant la propriété [**Supported**](/uwp/api/windows.media.devices.isospeedcontrol.supported). Si le contrôle est pris en charge, vous pouvez afficher et activer l’interface utilisateur de cette fonctionnalité. Définissez l’état activé de la case à cocher pour indiquer si l’ajustement automatique de la vitesse ISO est actuellement actif pour la valeur de la propriété [**auto**](/uwp/api/windows.media.devices.isospeedcontrol.auto) .

La valeur de sensibilité ISO doit être comprise dans la plage prise en charge par l’appareil et doit être un incrément de la taille de pas prise en charge. Obtenir les valeurs prises en charge pour l’appareil actuel en vérifiant les propriétés [**min**](/uwp/api/windows.media.devices.isospeedcontrol.min), [**Max**](/uwp/api/windows.media.devices.isospeedcontrol.max)et [**Step**](/uwp/api/windows.media.devices.isospeedcontrol.step) , qui sont utilisées pour définir les propriétés correspondantes du contrôle Slider.

Définissez la valeur du contrôle de curseur sur la valeur actuelle de **IsoSpeedControl** après la suppression de l’enregistrement du gestionnaire d’événements [**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) afin que l’événement ne soit pas déclenché lorsque la valeur est définie.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetIsoControl":::

Dans le gestionnaire d’événements **ValueChanged**, obtenez la valeur actuelle du contrôle et définissez la valeur de sensibilité ISO en appelant [**SetValueAsync**](/uwp/api/windows.media.devices.isospeedcontrol.setvalueasync).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetIsoSlider":::

Dans le gestionnaire d’événements **CheckedChanged** de la case à cocher de sensibilité ISO automatique, activez le réglage automatique de la sensibilité ISO en appelant [**SetAutoAsync**](/uwp/api/windows.media.devices.isospeedcontrol.setautoasync). Désactivez le réglage automatique de la sensibilité ISO en appelant [**SetValueAsync**](/uwp/api/windows.media.devices.isospeedcontrol.setvalueasync) et en indiquant la valeur actuelle du contrôle de curseur.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetIsoCheckBox":::

## <a name="optical-image-stabilization"></a>Stabilisation d’image optique

La stabilisation d’image optique (OIS, Optical image stabilization) stabilise le flux vidéo capturé en manipulant mécaniquement l’appareil de capture matérielle, et offre un résultat supérieur à celui de la stabilisation numérique. Sur les appareils qui ne prennent pas en charge OIS, vous pouvez utiliser l’effet VideoStabilizationEffect pour stabiliser numériquement la vidéo capturée. Pour plus d’informations, voir [Effets de capture vidéo](effects-for-video-capture.md).

Déterminez si la fonctionnalité OIS est prise en charge sur l’appareil actuel en vérifiant la propriété [**OpticalImageStabilizationControl.Supported**](/uwp/api/windows.media.devices.opticalimagestabilizationcontrol.supported).

Le contrôle OIS prend en charge trois modes : activé, désactivé et automatique, ce qui signifie que l’appareil détermine dynamiquement si la fonctionnalité OIS est susceptible d’améliorer la capture multimédia et l’active, le cas échéant. Pour déterminer si un mode particulier est pris en charge sur un appareil, vérifiez que la collection [**OpticalImageStabilizationControl.SupportedModes**](/uwp/api/windows.media.devices.opticalimagestabilizationcontrol.supportedmodes) contient le mode de votre choix.

Activez ou désactivez la fonctionnalité OIS en définissant le [**OpticalImageStabilizationControl.Mode**](/uwp/api/Windows.Media.Devices.OpticalImageStabilizationMode) sur le mode de votre choix.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetSetOpticalImageStabilizationMode":::

## <a name="powerline-frequency"></a>Fréquence du courant
Certains appareils photo prennent en charge le traitement anti scintillement qui implique de connaître la fréquence du courant alternatif (CA) dans l’environnement actuel. Certains appareils prennent en charge la détermination automatique de la fréquence du courant, tandis que d’autres nécessitent que la fréquence soit définie manuellement. L’exemple de code suivant montre comment déterminer la prise en charge de la fréquence du courant sur l’appareil et, si nécessaire, comment définir la fréquence manuellement. 

Tout d’abord, appelez la méthode [**TryGetPowerlineFrequency**](/uwp/api/windows.media.devices.videodevicecontroller.trygetpowerlinefrequency) de **VideoDeviceController**, en transmettant un paramètre de sortie de type [**PowerlineFrequency**](/uwp/api/Windows.Media.Capture.PowerlineFrequency). Si cet appel échoue, le contrôle de la fréquence du courant n’est pas pris en charge sur l’appareil actuel. Si la fonctionnalité est prise en charge, vous pouvez déterminer si le mode automatique est disponible sur l’appareil en essayant de définir ce mode. Pour ce faire, appelez [**TrySetPowerlineFrequency**](/uwp/api/windows.media.devices.videodevicecontroller.trysetpowerlinefrequency) et passez la valeur **auto**. Si l’appel s’effectue correctement, cela signifie que la fréquence de votre courant automatique est prise en charge. Si le contrôleur de fréquence du courant est pris en charge sur l’appareil, mais que la détection de fréquence automatique ne l’est pas, vous pouvez définir manuellement la fréquence à l’aide de **TrySetPowerlineFrequency**. Dans cet exemple, **MyCustomFrequencyLookup** est une méthode personnalisée que vous implémentez pour déterminer la fréquence appropriée pour l’emplacement actuel de l’appareil. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetPowerlineFrequency":::

## <a name="white-balance"></a>Balance des blancs

Le [**WhiteBalanceControl**](/uwp/api/windows.media.devices.videodevicecontroller.whitebalancecontrol) vous permet de définir l’équilibre blanc utilisé lors de la capture de photos ou de vidéos.

Cet exemple utilise un contrôle [**ComboBox**](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) pour sélectionner des présélections de température de couleur intégrées et un contrôle [**Slider**](/uwp/api/Windows.UI.Xaml.Controls.Slider) pour l’ajustement manuel de l’équilibre des blancs.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetWhiteBalanceXAML":::

Vérifiez si l’appareil de capture actuel prend en charge **WhiteBalanceControl** en vérifiant la propriété [**Supported**](/uwp/api/windows.media.devices.whitebalancecontrol.supported). Si le contrôle est pris en charge, vous pouvez afficher et activer l’interface utilisateur de cette fonctionnalité. Définissez les éléments de la zone de liste déroulante sur les valeurs de l’énumération [**ColorTemperaturePreset**](/uwp/api/Windows.Media.Devices.ColorTemperaturePreset) . Et affectez à l’élément sélectionné la valeur actuelle de la propriété [**prédéfinie**](/uwp/api/windows.media.devices.whitebalancecontrol.preset) .

Pour le contrôle manuel, la valeur de balance des blancs doit être comprise dans la plage prise en charge par l’appareil et doit être un incrément de la taille de pas prise en charge. Obtenir les valeurs prises en charge pour l’appareil actuel en vérifiant les propriétés [**min**](/uwp/api/windows.media.devices.whitebalancecontrol.min), [**Max**](/uwp/api/windows.media.devices.whitebalancecontrol.max)et [**Step**](/uwp/api/windows.media.devices.whitebalancecontrol.step) , qui sont utilisées pour définir les propriétés correspondantes du contrôle Slider. Avant d’activer le contrôle manuel, vérifiez que la plage entre les valeurs minimales et maximales prises en charge est supérieure à la taille de pas. Si ce n’est pas le cas, le contrôle manuel n’est pas pris en charge sur l’appareil actuel.

Définissez la valeur du contrôle de curseur sur la valeur actuelle de **WhiteBalanceControl** après la suppression de l’enregistrement du gestionnaire d’événements [**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) afin que l’événement ne soit pas déclenché lorsque la valeur est définie.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetWhiteBalance":::

Dans le gestionnaire d’événements [**SelectionChanged**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) de la zone de liste déroulante température de couleur prédéfinie, récupérez la présélection actuellement sélectionnée et définissez la valeur du contrôle en appelant [**SetPresetAsync**](/uwp/api/windows.media.devices.whitebalancecontrol.setpresetasync). Si la valeur prédéfinie sélectionnée n’est pas **Manual**, désactivez le curseur de balance des blancs manuelle.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetWhiteBalanceComboBox":::

Dans le gestionnaire d’événements **ValueChanged**, obtenez la valeur actuelle du contrôle et définissez la valeur de balance des blancs en appelant [**SetValueAsync**](/uwp/api/windows.media.devices.exposurecontrol.setvalueasync).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetWhiteBalanceSlider":::

> [!IMPORTANT]
> Le réglage de la balance des blancs est uniquement pris en charge pendant l’exécution du flux d’aperçu. Vérifiez que le flux d’aperçu est en cours d’exécution avant de définir la valeur de balance des blancs ou une valeur prédéfinie.

> [!IMPORTANT]
> La valeur prédéfinie **ColorTemperaturePreset.Auto** indique au système de régler automatiquement le niveau de balance des blancs. Dans certains scénarios, comme la capture d’une séquence de photos où les niveaux de balance des blancs doivent être identiques pour chaque cliché, vous pouvez verrouiller le contrôle sur la valeur automatique actuelle. Pour ce faire, appelez [**SetPresetAsync**](/uwp/api/windows.media.devices.whitebalancecontrol.setpresetasync) et spécifiez le **Manual** prédéfini et ne définissez pas de valeur sur le contrôle à l’aide de [**SetValueAsync**](/uwp/api/windows.media.devices.whitebalancecontrol.setvalueasync). Ceci entraînera un verrouillage de l’appareil sur la valeur actuelle. Ne tentez pas de lire la valeur du contrôle actuelle et de transmettre la valeur retournée à **SetValueAsync** car l’exactitude de cette valeur n’est pas garantie.

## <a name="zoom"></a>Zoom

Le [**ZoomControl**](/uwp/api/Windows.Media.Devices.ZoomControl) vous permet de définir le niveau de zoom utilisé lors de la capture de photos ou de vidéos.

Cet exemple utilise un contrôle [**Slider**](/uwp/api/Windows.UI.Xaml.Controls.Slider) pour ajuster le niveau de zoom actuel. La section suivante montre comment régler le zoom par un geste de pincement à l’écran.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetZoomXAML":::

Vérifiez si l’appareil de capture actuel prend en charge **ZoomControl** en vérifiant la propriété [**Supported**](/uwp/api/windows.media.devices.zoomcontrol.supported). Si le contrôle est pris en charge, vous pouvez afficher et activer l’interface utilisateur de cette fonctionnalité.

La valeur de niveau de zoom doit être comprise dans la plage prise en charge par l’appareil et doit être un incrément de la taille de pas prise en charge. Obtenir les valeurs prises en charge pour l’appareil actuel en vérifiant les propriétés [**min**](/uwp/api/windows.media.devices.zoomcontrol.min), [**Max**](/uwp/api/windows.media.devices.zoomcontrol.max)et [**Step**](/uwp/api/windows.media.devices.zoomcontrol.step) , qui sont utilisées pour définir les propriétés correspondantes du contrôle Slider.

Définissez la valeur du contrôle de curseur sur la valeur actuelle de **ZoomControl** après la suppression de l’enregistrement du gestionnaire d’événements [**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) afin que l’événement ne soit pas déclenché lorsque la valeur est définie.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetZoomControl":::

Dans le gestionnaire d’événements **ValueChanged**, créez une instance de la classe [**ZoomSettings**](/uwp/api/Windows.Media.Devices.ZoomSettings) en définissant la propriété [**Value**](/uwp/api/windows.media.devices.zoomsettings.value) sur la valeur actuelle du contrôle de curseur de zoom. Si la propriété [**SupportedModes**](/uwp/api/windows.media.devices.zoomcontrol.supportedmodes) de **ZoomControl** contient [**ZoomTransitionMode.Smooth**](/uwp/api/Windows.Media.Devices.ZoomTransitionMode), cela signifie que l’appareil prend en charge les transitions fluides entre les niveaux de zoom. Étant donné que ce mode offre une meilleure expérience utilisateur, vous devrez généralement utiliser cette valeur pour la propriété [**Mode**](/uwp/api/windows.media.devices.zoomsettings.mode) de l’objet **ZoomSettings**.

Enfin, modifiez les paramètres de zoom actuel en transmettant votre objet **ZoomSettings** à la méthode [**Configure**](/uwp/api/windows.media.devices.zoomcontrol.configure) de l’objet **ZoomControl**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetZoomSlider":::

### <a name="smooth-zoom-using-pinch-gesture"></a>Zoom fluide à l’aide du mouvement de pincement

Comme indiqué dans la section précédente, lorsqu’il est pris en charge, le mode de zoom fluide offre à l’appareil de capture une transition aisée entre les niveaux de zoom numérique, permettant à l’utilisateur d’ajuster dynamiquement le niveau de zoom pendant l’opération de capture sans secousse et en toute discrétion. Cette section décrit comment ajuster le niveau de zoom suite à un mouvement de pincement.

Déterminez si le contrôle de zoom numérique est pris en charge sur l’appareil actuel en vérifiant la propriété [**ZoomControl.Supported**](/uwp/api/windows.media.devices.zoomcontrol.supported). Ensuite, déterminez si le mode de zoom fluide est disponible en vérifiant la [**ZoomControl.SupportedModes**](/uwp/api/windows.media.devices.zoomcontrol.supportedmodes) pour voir si elle contient la valeur [**ZoomTransitionMode.Smooth**](/uwp/api/Windows.Media.Devices.ZoomTransitionMode).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetIsSmoothZoomSupported":::

Sur un appareil tactile multipoint, un scénario courant consiste à ajuster le facteur de zoom avec un mouvement de pincement effectué avec deux doigts. Affectez à la propriété [**ManipulationMode**](/uwp/api/windows.ui.xaml.uielement.manipulationmode) du contrôle [**CaptureElement**](/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) la valeur [**ManipulationModes. Scale**](/uwp/api/Windows.UI.Xaml.Input.ManipulationModes) pour activer le mouvement de pincement. Ensuite, inscrivez-vous pour l’événement [**ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta) qui est déclenché lorsque le geste de pincement change de taille.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetRegisterPinchGestureHandler":::

Dans le gestionnaire pour l’événement **ManipulationDelta**, mettez à jour le facteur de zoom basé sur la modification du mouvement de pincement de l’utilisateur. La valeur [**ManipulationDelta.Scale**](/uwp/api/Windows.UI.Input.ManipulationDelta) représente la modification de l’échelle de pincement (une faible augmentation de la taille du pincement correspondra à un nombre légèrement supérieur à 1 et une faible réduction de la taille du pincement correspondra à un nombre légèrement inférieur à 1). Dans cet exemple, la valeur actuelle du contrôle de zoom est multipliée par le delta de mise à l’échelle.

Avant de définir le facteur de zoom, vous devez vous assurer que la valeur n’est pas inférieure à la valeur minimale prise en charge par l’appareil, comme indiqué par la propriété [**ZoomControl. min**](/uwp/api/windows.media.devices.zoomcontrol.min) . En outre, assurez-vous que la valeur est inférieure ou égale à la valeur [**ZoomControl. Max**](/uwp/api/windows.media.devices.zoomcontrol.max) . Enfin, vous devez vous assurer que le facteur de zoom est un multiple de la taille de l’étape de zoom prise en charge par l’appareil, comme indiqué par la propriété [**Step**](/uwp/api/windows.media.devices.zoomcontrol.step) . Si le facteur de zoom ne répond pas à ces critères, une exception sera levée lorsque vous tenterez de définir le niveau de zoom sur l’appareil de capture.

Définissez le niveau de zoom sur l’appareil de capture en créant un nouvel objet [**ZoomSettings**](/uwp/api/Windows.Media.Devices.ZoomSettings) . Définissez la propriété [**Mode**](/uwp/api/windows.media.devices.zoomsettings.mode) sur [**ZoomTransitionMode.Smooth**](/uwp/api/Windows.Media.Devices.ZoomTransitionMode), puis définissez la propriété [**Value**](/uwp/api/windows.media.devices.zoomsettings.value) sur votre facteur de zoom souhaité. Enfin, appelez [**ZoomControl.Configure**](/uwp/api/windows.media.devices.zoomcontrol.configure) pour définir la nouvelle valeur de zoom sur l’appareil. L’appareil fera une transition harmonieuse vers la nouvelle valeur de zoom.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetManipulationDelta":::

## <a name="related-topics"></a>Rubriques connexes

* [Appareil photo](camera.md)
* [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
