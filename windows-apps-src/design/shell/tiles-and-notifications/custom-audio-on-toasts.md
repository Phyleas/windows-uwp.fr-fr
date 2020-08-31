---
description: Découvrez comment utiliser l’audio personnalisé sur vos notifications toast pour permettre à votre application d’exprimer les effets audio uniques de votre image.
title: Audio personnalisé sur les toasts
label: Custom audio on toasts
template: detail.hbs
ms.date: 12/15/2017
ms.topic: article
keywords: Windows 10, UWP, toast, audio personnalisé, notification, audio, son
ms.localizationpriority: medium
ms.openlocfilehash: 81bec439f17cadb7db0576dafcf4299f0978b192
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89054459"
---
# <a name="custom-audio-on-toasts"></a>Audio personnalisé sur les toasts

Les notifications Toast peuvent utiliser du son personnalisé, ce qui permet à votre application d’exprimer les effets audio uniques de votre image. Par exemple, une application de messagerie peut utiliser son propre son de messagerie sur les notifications de Toast, afin que l’utilisateur sache instantanément qu’il a reçu une notification de l’application, au lieu d’entendre le son de notification générique.

## <a name="install-uwp-community-toolkit-nuget-package"></a>Installer le package NuGet Community Toolkit pour UWP

Pour créer des notifications par le biais du code, nous vous recommandons vivement d’utiliser la bibliothèque de notifications du kit de modèles de la communauté UWP, qui fournit un modèle d’objet pour le contenu XML de la notification. Vous pouvez construire manuellement le document XML de notification, mais cela peut être source d’erreurs et confus. La bibliothèque de notifications dans la boîte à outils de la communauté UWP est construite et gérée par l’équipe qui possède les notifications chez Microsoft.

Installez [Microsoft. Toolkit. UWP. notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) à partir de NuGet (nous utilisons la version 1.0.0 dans cette documentation).


## <a name="add-namespace-declarations"></a>Ajout de déclarations d'espaces de noms

`Windows.UI.Notifications` comprend les API Tile et Toast. `Microsoft.Toolkit.Uwp.Notifications` comprend la bibliothèque de notifications.

```csharp
using Microsoft.Toolkit.Uwp.Notifications;
using Windows.UI.Notifications;
```


## <a name="construct-the-notification"></a>Construire la notification

Le contenu de la notification Toast comprend du texte et des images, ainsi que des boutons et des entrées. Consultez [Envoyer un toast local](send-local-toast.md) pour voir un extrait de code complet.

```csharp
ToastContent toastContent = new ToastContent()
{
    Visual = new ToastVisual()
    {
        ... (omitted)
    }
};
```


## <a name="add-the-custom-audio"></a>Ajouter l’audio personnalisé

Windows Mobile a toujours pris en charge l’audio personnalisée dans les notifications Toast. Toutefois, Desktop a ajouté uniquement la prise en charge de l’audio personnalisé dans la version 1511 (Build 10586). Si vous envoyez un toast qui contient du son personnalisé à un appareil de bureau avant la version 1511, le Toast sera silencieux. Par conséquent, pour les ordinateurs de bureau antérieurs à la version 1511, vous ne devez pas inclure l’audio personnalisé dans votre notification Toast, afin que la notification utilise au moins le son de notification par défaut.

**Problème connu**: Si vous utilisez la version 1511 du bureau, le fichier audio Toast personnalisé ne fonctionnera que si votre application est installée via le Windows Store. Cela signifie que vous ne pouvez pas tester localement votre audio personnalisé sur le Bureau avant de l’envoyer au Store, mais le son fonctionnera correctement une fois installé à partir du Store. Nous avons résolu cela dans la mise à jour anniversaire, afin que l’audio personnalisé de votre application déployée localement fonctionne correctement.

```csharp
?
bool supportsCustomAudio = true;
 
// If we're running on Desktop before Version 1511, do NOT include custom audio
// since it was not supported until Version 1511, and would result in a silent toast.
if (AnalyticsInfo.VersionInfo.DeviceFamily.Equals("Windows.Desktop")
    && !ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 2))
{
    supportsCustomAudio = false;
}
 
if (supportsCustomAudio)
{
    toastContent.Audio = new ToastAudio()
    {
        Src = new Uri("ms-appx:///Assets/Audio/CustomToastAudio.m4a")
    };
}
```

Les types de fichiers audio pris en charge incluent...

- .aac
- . FLAC
- .m4a
- .mp3
- .wav
- .wma


## <a name="send-the-notification"></a>Envoyer la notification

Maintenant que votre contenu Toast est terminé, l’envoi de la notification est assez simple.

```csharp
// Create the toast notification from the previous toast content
ToastNotification notification = new ToastNotification(toastContent.GetXml());
             
// And then send the toast
ToastNotificationManager.CreateToastNotifier().Show(notification);
```


## <a name="related-topics"></a>Rubriques connexes

- [Exemple de code complet sur GitHub](https://github.com/WindowsNotifications/quickstart-toast-with-custom-audio)
- [Envoyer un toast local](send-local-toast.md)
- [Documentation sur le contenu Toast](adaptive-interactive-toasts.md)