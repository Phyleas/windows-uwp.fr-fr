---
title: Préversion d’hébergement pour le scanneur de codes-barres de l’appareil
description: Découvrez comment héberger une préversion du scanneur de codes-barres de l’appareil photo dans votre application
ms.date: 05/02/2018
ms.topic: article
keywords: Windows 10, UWP, point de service, pos
ms.localizationpriority: medium
ms.openlocfilehash: 6657fa52a5e3cac0821def7102b4bc93089b2ffe
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159633"
---
# <a name="hosting-a-camera-barcode-scanner-preview-in-your-application"></a>Hébergement d’un aperçu du scanneur de codes-barres de l’appareil photo dans votre application
## <a name="step-1-setup-your-camera-preview"></a>Étape 1 : configurer votre aperçu de l’appareil photo
La première étape de l’ajout d’une préversion à votre application pour le scanneur de codes-barres de l’appareil peut être effectuée en suivant les instructions de la rubrique [afficher la version d’évaluation de l’appareil photo](../audio-video-camera/simple-camera-preview-access.md) .  Une fois cette étape terminée, revenez à cette rubrique pour obtenir des modifications spécifiques au scanneur de codes-barres de l’appareil.

## <a name="step-2-update-capability-declarations"></a>Étape 2 : mettre à jour les déclarations de fonction
Pour empêcher les utilisateurs de recevoir l’invite de consentement du microphone, vous pouvez exclure cela des fonctionnalités listées dans le manifeste de votre application.

1. Dans Microsoft Visual Studio, dans l’**Explorateur de solutions**, ouvrez le concepteur pour le manifeste de l’application en double-cliquant sur l’élément **package.appxmanifest**.
2. Sélectionnez l’onglet **Fonctionnalités**.
3. Décochez la case du **microphone**

 ## <a name="step-3-add-additional-using-directive-for-media-capture"></a>Étape 3 : ajouter une directive using supplémentaire pour la capture de média

```Csharp
using Windows.Media.Capture;
```

## <a name="step-4-set-up-your-mediacapture-initialization-settings"></a>Étape 4 : configurer vos paramètres d’initialisation de MediaCapture
L’exemple suivant initialise [**MediaCaptureInitializationSettings**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings). 

```Csharp
 private void InitCaptureSettings()
{
    _captureInitSettings = new MediaCaptureInitializationSettings();
    _captureInitSettings.VideoDeviceId = BarcodeScanner.VideoDeviceId;
    _captureInitSettings.StreamingCaptureMode = StreamingCaptureMode.Video;
    _captureInitSettings.PhotoCaptureSource = PhotoCaptureSource.VideoPreview;
}
```
## <a name="step-5-associate-your-mediacapture-object-with-the-camera-barcode-scanner"></a>Étape 5 : associer votre objet MediaCapture au scanneur de codes-barres de l’appareil photo
Remplacez le mediaCapture.Iniexistant tializeAsync () dans *StartPreviewAsync ()* par ce qui suit :

```Csharp
try
    {

        mediaCapture = new MediaCapture();
        await mediaCapture.InitializeAsync(InitCaptureSettings());

        displayRequest.RequestActive();
        DisplayInformation.AutoRotationPreferences = DisplayOrientations.Landscape;
    }
```

> [!TIP]
> Pour obtenir des rubriques plus avancées sur l’hébergement d’une préversion d’appareil photo dans votre application, consultez [afficher la version préliminaire de l’appareil](../audio-video-camera/simple-camera-preview-access.md#add-capability-declarations-to-the-app-manifest) photo.

## <a name="see-also"></a>Voir aussi

### <a name="samples"></a>Exemples

- [Exemple de scanneur de codes-barres](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)