---
description: Découvrez comment utiliser toast avec l’activation de mise à jour en attente pour créer des interactions à plusieurs étapes dans vos notifications Toast.
title: Toast avec activation de mise à jour en attente
label: Toast with pending update activation
template: detail.hbs
ms.date: 12/14/2017
ms.topic: article
keywords: Windows 10, UWP, toast, mise à jour en attente, pendingupdate, interactivité en plusieurs étapes, interactions avec plusieurs étapes
ms.localizationpriority: medium
ms.openlocfilehash: 00551414fbefe5591813731337653964bd2524f3
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89054539"
---
# <a name="toast-with-pending-update-activation"></a>Toast avec activation de mise à jour en attente

Vous pouvez utiliser **PendingUpdate** pour créer des interactions à plusieurs étapes dans vos notifications Toast. Par exemple, comme indiqué ci-dessous, vous pouvez créer une série de toasts où les toasts suivants dépendent des réponses des toasts précédents.

![Toast avec mise à jour en attente](images/toast-pendingupdate.gif)

> [!IMPORTANT]
> **Requiert Desktop place Creators Update et 2.0.0 de la bibliothèque de notifications**: vous devez exécuter desktop Build 16299 ou une version ultérieure pour voir le travail de mise à jour en attente. Vous devez utiliser la version 2.0.0 ou une version ultérieure de la bibliothèque de la [boîte à outils des notifications de la communauté UWP](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) pour affecter **PendingUpdate** sur vos boutons. **PendingUpdate** est uniquement pris en charge sur le bureau et sera ignoré sur les autres appareils.


## <a name="prerequisites"></a>Prérequis

Cet article suppose une connaissance du fonctionnement de...

- [Construction de contenu Toast](adaptive-interactive-toasts.md)
- [Envoi d’un toast et gestion de l’activation en arrière-plan](send-local-toast.md)


## <a name="overview"></a>Vue d'ensemble

Pour implémenter un toast qui utilise la mise à jour en attente comme son comportement d’activation après...

1. Sur vos boutons d’activation en arrière-plan Toast, spécifiez un **AfterActivationBehavior** de **PendingUpdate**

2. Assigner une **balise** (et éventuellement un **groupe**) lors de l’envoi de votre toast

3. Quand l’utilisateur clique sur le bouton, votre tâche en arrière-plan est activée et le toast est conservé à l’écran dans un état de mise à jour en attente

4. Dans votre tâche en arrière-plan, envoyez un nouveau Toast avec votre nouveau contenu, à l’aide de la même **balise** et du même **groupe**


## <a name="assign-pendingupdate"></a>Affecter PendingUpdate

Sur les boutons d’activation en arrière-plan, définissez **AfterActivationBehavior** sur **PendingUpdate**. Notez que cela ne fonctionne que pour les boutons qui ont un **ActivationType** de l' **arrière-plan**.

```csharp
new ToastButton("Yes", "action=orderLunch")
{
    ActivationType = ToastActivationType.Background,

    ActivationOptions = new ToastActivationOptions()
    {
        AfterActivationBehavior = ToastAfterActivationBehavior.PendingUpdate
    }
}
```

```xml
<action
    content='Yes'
    arguments='action=orderLunch'
    activationType='background'
    afterActivationBehavior='pendingUpdate' />
```


## <a name="use-a-tag-on-the-notification"></a>Utiliser une balise dans la notification

Pour pouvoir remplacer ultérieurement la notification, nous devons affecter la **balise** (et éventuellement le **groupe**) à la notification.

```csharp
// Create the notification
var notif = new ToastNotification(content.GetXml())
{
    Tag = "lunch"
};

// And show it
ToastNotificationManager.CreateToastNotifier().Show(notif);
```


## <a name="replace-the-toast-with-new-content"></a>Remplacer le Toast par le nouveau contenu

En réponse à l’utilisateur qui clique sur le bouton, votre tâche en arrière-plan est déclenchée et vous devez remplacer le Toast par un nouveau contenu. Vous remplacez le Toast en envoyant simplement un nouveau Toast avec la même **balise** et le même **groupe**.

Nous vous recommandons vivement de **définir le son sur Silent** sur les remplacements en réponse à un clic de bouton, puisque l’utilisateur interagit déjà avec votre toast.

```csharp
// Generate new content
ToastContent content = new ToastContent()
{
    ...

    // We disable audio on all subsequent toasts since they appear right after the user
    // clicked something, so the user's attention is already captured
    Audio = new ToastAudio() { Silent = true }
};

// Create the new notification
var notif = new ToastNotification(content.GetXml())
{
    Tag = "lunch"
};

// And replace the old one with this one
ToastNotificationManager.CreateToastNotifier().Show(notif);
```


## <a name="related-topics"></a>Rubriques connexes

- [Exemple de code complet sur GitHub](https://github.com/WindowsNotifications/quickstart-toast-pending-update)
- [Envoyer un toast local et gérer l’activation](send-local-toast.md)
- [Documentation sur le contenu Toast](adaptive-interactive-toasts.md)
- [Barre de progression Toast](toast-progress-bar.md)