---
Description: Découvrez comment planifier l’affichage d’une notification Toast locale à un moment ultérieur.
title: Planifier une notification Toast
label: Schedule a toast notification
template: detail.hbs
ms.date: 04/09/2020
ms.topic: article
keywords: Windows 10, UWP, notification Toast planifiée, scheduledtoastnotification, Guide de démarrage rapide, prise en main, exemple de code, procédure pas à pas
ms.localizationpriority: medium
ms.openlocfilehash: 8ef849490df82d7677e19d1f1619d98111611be3
ms.sourcegitcommit: c5df8832e9df8749d0c3eee9e85f4c2d04f8b27b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2020
ms.locfileid: "92100307"
---
# <a name="schedule-a-toast-notification"></a>Planifier une notification Toast

Les notifications Toast planifiées vous permettent de planifier l’affichage d’une notification à un moment ultérieur, que votre application s’exécute à ce moment-là ou non. Cela est utile pour des scénarios tels que l’affichage de rappels ou d’autres tâches de suivi pour l’utilisateur, où l’heure et le contenu de la notification sont connus à l’avance.

Notez que les notifications Toast planifiées ont une fenêtre de remise de 5 minutes. Si l’ordinateur est éteint pendant l’heure de remise planifiée et reste inactif pendant plus de 5 minutes, la notification est « supprimée », car elle n’est plus pertinente pour l’utilisateur. Si vous avez besoin de garantir la remise des notifications, quelle que soit la durée de l’arrêt de l’ordinateur, nous vous recommandons d’utiliser une tâche en arrière-plan avec un déclencheur de temps, comme illustré dans [cet exemple de code](https://github.com/WindowsNotifications/quickstart-snoozable-toasts-even-if-computer-is-off).

> [!IMPORTANT]
> Les applications de bureau (packages MSIX/Sparse et bureau classique) présentent des étapes légèrement différentes pour l’envoi de notifications et la gestion de l’activation. Suivez les instructions ci-dessous, mais remplacez `ToastNotificationManager` par la `DesktopNotificationManagerCompat` classe de la documentation sur les [applications de bureau](toast-desktop-apps.md) .

> **API importantes**: [classe ScheduledToastNotification](/uwp/api/Windows.UI.Notifications.ScheduledToastNotification)


## <a name="prerequisites"></a>Prérequis

Pour bien comprendre cette rubrique, les éléments suivants sont utiles...

* Une connaissance pratique des termes et des concepts de notification Toast. Pour plus d’informations, consultez [Toast and Action Center Overview](/archive/blogs/tiles_and_toasts/toast-notification-and-action-center-overview-for-windows-10).
* Une bonne connaissance du contenu de notification Windows 10 Toast. Pour plus d’informations, consultez [la documentation sur le contenu Toast](adaptive-interactive-toasts.md).
* Un projet d’application UWP Windows 10


## <a name="step-1-install-nuget-package"></a>Étape 1 : installer le package NuGet

Installez le [package NuGet Microsoft. Toolkit. UWP. notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/). Notre exemple de code utilisera ce package. À la fin de l’article, nous fournirons les extraits de code « ordinaires » qui n’utilisent pas de packages NuGet. Ce package vous permet de créer des notifications de Toast sans utiliser XML.


## <a name="step-2-add-namespace-declarations"></a>Étape 2 : ajouter des déclarations d’espace de noms

`Windows.UI.Notifications` comprend les API Toast.

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
```


## <a name="step-3-schedule-the-notification"></a>Étape 3 : planifier la notification

Nous utiliserons une notification textuelle simple rappelant à un étudiant les devoirs qu’il a dû aujourd’hui. Construisez la notification et planifiez-la !

```csharp
// Construct the content
var content = new ToastContentBuilder()
    .AddToastActivationInfo("itemsDueToday", ToastActivationType.Foreground)
    .AddText("ASTR 170B1")
    .AddText("You have 3 items due today!");
    .GetToastContent();

    
// Create the scheduled notification
var toast = new ScheduledToastNotification(content.GetXml(), DateTime.Now.AddSeconds(5));


// Add your scheduled toast to the schedule
ToastNotificationManager.CreateToastNotifier().AddToSchedule(toast);
```


## <a name="provide-a-primary-key-for-your-toast"></a>Fournir une clé primaire pour votre toast

Si vous souhaitez annuler, supprimer ou remplacer par programmation la notification planifiée, vous devez utiliser la propriété Tag (et éventuellement la propriété Group) pour fournir une clé primaire pour votre notification. Ensuite, vous pouvez utiliser cette clé primaire à l’avenir pour annuler, supprimer ou remplacer la notification.

Pour plus d’informations sur le remplacement ou la suppression des notifications Toast déjà fournies, consultez [démarrage rapide : gestion des notifications Toast dans le centre de maintenance (XAML)](/previous-versions/windows/apps/dn631260(v=win.10)).

Les balises et les groupes fonctionnent comme une clé primaire composite. Group est l’identificateur plus générique, dans lequel vous pouvez assigner des groupes tels que « wallPosts », « messages », « friendRequests », etc. La balise doit ensuite identifier de manière unique la notification proprement dite à partir du groupe. À l’aide d’un groupe générique, vous pouvez supprimer toutes les notifications de ce groupe à l’aide de l' [API RemoveGroup](/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_).

```csharp
toast.Tag = "18365";
toast.Group = "ASTR 170B1";
```


## <a name="cancel-scheduled-notifications"></a>Annuler des notifications planifiées

Pour annuler une notification planifiée, vous devez d’abord récupérer la liste de toutes les notifications planifiées.

Recherchez ensuite votre toast planifié correspondant à la balise (et éventuellement au groupe) que vous avez spécifié précédemment, puis appelez RemoveFromSchedule ().

```csharp
// Create the toast notifier
ToastNotifier notifier = ToastNotificationManager.CreateToastNotifier();

// Get the list of scheduled toasts that haven't appeared yet
IReadOnlyList<ScheduledToastNotification> scheduledToasts = notifier.GetScheduledToastNotifications();

// Find our scheduled toast we want to cancel
var toRemove = scheduledToasts.FirstOrDefault(i => i.Tag == "18365" && i.Group == "ASTR 170B1");
if (toRemove != null)
{
    // And remove it from the schedule
    notifier.RemoveFromSchedule(toRemove);
}
```


## <a name="activation-handling"></a>Gestion de l’activation

Pour en savoir plus sur la gestion de l’activation, consultez la documentation [Envoyer un toast local](send-local-toast.md) . L’activation d’une notification Toast planifiée est gérée de la même façon que l’activation d’une notification Toast locale.


## <a name="adding-actions-inputs-and-more"></a>Ajout d’actions, d’entrées, etc.

Consultez la documentation [Envoyer un toast local](send-local-toast.md) pour en savoir plus sur les rubriques avancées comme les actions et les entrées. Les actions et les entrées fonctionnent de la même manière dans les toasts locaux, comme dans les toasts planifiés.


## <a name="resources"></a>Ressources

* [Documentation sur le contenu Toast](adaptive-interactive-toasts.md)
* [ScheduledToastNotification, classe](/uwp/api/Windows.UI.Notifications.ScheduledToastNotification)