---
Description: Découvrez comment planifier l’affichage d’une notification Toast locale à un moment ultérieur.
title: Planifier une notification Toast
label: Schedule a toast notification
template: detail.hbs
ms.date: 04/09/2020
ms.topic: article
keywords: Windows 10, UWP, notification Toast planifiée, scheduledtoastnotification, Guide de démarrage rapide, prise en main, exemple de code, procédure pas à pas
ms.localizationpriority: medium
ms.openlocfilehash: 07339cf793bdada51f79d70d9e9e6b6d4a41851b
ms.sourcegitcommit: 017f2f1492f3220da0fae8b4c99de7206a185dff
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81386428"
---
# <a name="schedule-a-toast-notification"></a>Planifier une notification Toast

Les notifications Toast planifiées vous permettent de planifier l’affichage d’une notification à un moment ultérieur, que votre application s’exécute à ce moment-là ou non. Cela est utile pour des scénarios tels que l’affichage de rappels ou d’autres tâches de suivi pour l’utilisateur, où l’heure et le contenu de la notification sont connus à l’avance.

Notez que les notifications Toast planifiées ont une fenêtre de remise de 5 minutes. Si l’ordinateur est éteint pendant l’heure de remise planifiée et reste inactif pendant plus de 5 minutes, la notification est « supprimée », car elle n’est plus pertinente pour l’utilisateur. Si vous avez besoin de garantir la remise des notifications, quelle que soit la durée de l’arrêt de l’ordinateur, nous vous recommandons d’utiliser une tâche en arrière-plan avec un déclencheur de temps, comme illustré dans [cet exemple de code](https://github.com/WindowsNotifications/quickstart-snoozable-toasts-even-if-computer-is-off).

> [!IMPORTANT]
> Les applications de bureau (packages MSIX/épars et Win32 classiques) présentent des étapes légèrement différentes pour l’envoi de notifications et la gestion de l’activation. Suivez les instructions ci-dessous, mais remplacez `ToastNotificationManager` par la classe `DesktopNotificationManagerCompat` de la documentation sur les [applications de bureau](toast-desktop-apps.md) .

> **API importantes**: [classe ScheduledToastNotification](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledToastNotification)


## <a name="prerequisites"></a>Composants requis

Pour bien comprendre cette rubrique, il est utile de disposer de ce qui suit...

* Une connaissance pratique des concepts et des termes relatifs aux notifications toast. Pour plus d’informations, consultez [Toast and Action Center Overview](https://blogs.msdn.microsoft.com/tiles_and_toasts/2015/07/08/toast-notification-and-action-center-overview-for-windows-10/).
* Une bonne connaissance du contenu d’une notification toast Windows 10. Pour plus d’informations, voir la [documentation sur le contenu des toasts](adaptive-interactive-toasts.md).
* Un projet d’application UWP Windows 10


## <a name="install-nuget-packages"></a>Installer des packages NuGet

Nous vous recommandons d’installer les deux packages NuGet suivants à votre projet. Notre exemple de code les utilise.

* [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) : générez des charges utiles de toast via des objets à la place de code XML brut.
* [QueryString.NET](https://www.nuget.org/packages/QueryString.NET/) : générez et analysez les chaînes de requête en C#.


## <a name="add-namespace-declarations"></a>Ajouter des déclarations d’espace de noms

`Windows.UI.Notifications` comprend les API Toast.

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
using Microsoft.QueryStringDotNET; // QueryString.NET
```


## <a name="construct-the-toast-content"></a>Construire le contenu Toast

Dans Windows 10, le contenu de votre notification toast est décrit à l’aide d’un langage adaptatif qui permet une grande flexibilité en termes d’apparence de notification. Pour plus d’informations, voir la [documentation sur le contenu des toasts](adaptive-interactive-toasts.md).

Grâce à la bibliothèque de notifications, la génération du contenu XML est simple. Si vous n’installez pas la bibliothèque Notifications à partir de NuGet, vous devez construire le code XML manuellement, ce qui peut prêter aux erreurs.

Vous devez toujours définir la propriété **Launch** de manière à ce que lorsque l’utilisateur appuie sur le corps du toast et que votre application est lancée, cette dernière sait quel contenu elle doit afficher.

```csharp
// In a real app, these would be initialized with actual data
string title = "ASTR 170B1";
string content = "You have 3 items due today!";

// Now we can construct the final toast content
ToastContent toastContent = new ToastContent()
{
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = title
                },
     
                new AdaptiveText()
                {
                    Text = content
                }
            }
        }
    },
 
    // Arguments when the user taps body of toast
    Launch = new QueryString()
    {
        { "action", "viewClass" },
        { "classId", "3910938180" }
 
    }.ToString()
};
```

## <a name="create-the-scheduled-toast"></a>Créer le Toast planifié

Une fois que vous avez initialisé votre contenu Toast, créez un nouveau [ScheduledToastNotification](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledToastNotification) et transmettez le code XML du contenu, ainsi que l’heure à laquelle vous souhaitez que la notification soit remise.

```csharp
// Create the scheduled notification
var toast = new ScheduledToastNotification(toastContent.GetXml(), DateTime.Now.AddSeconds(5));
```


## <a name="provide-a-primary-key-for-your-toast"></a>Fournir une clé primaire pour votre toast

Si vous souhaitez annuler, supprimer ou remplacer par programmation la notification planifiée, vous devez utiliser la propriété Tag (et éventuellement la propriété Group) pour fournir une clé primaire pour votre notification. Ensuite, vous pouvez utiliser cette clé primaire à l’avenir pour annuler, supprimer ou remplacer la notification.

Pour plus d’informations sur le remplacement/la suppression des notifications toast déjà remises, voir [Démarrage rapide : gestion des notifications toast dans le centre de notifications (XAML)](https://docs.microsoft.com/previous-versions/windows/apps/dn631260(v=win.10)).

Les propriétés Tag et Group combinées font office de clé primaire composite. La propriété Group est l’identificateur le plus générique, où vous pouvez affecter des groupes tels que « wallPosts », « messages », « friendRequests », etc. tandis que la propriété Tag doit identifier de manière unique la notification proprement dite au sein du groupe. En utilisant un groupe générique, vous pouvez ensuite supprimer toutes les notifications de ce groupe à l’aide de l’[API RemoveGroup](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_).

```csharp
toast.Tag = "18365";
toast.Group = "ASTR 170B1";
```


## <a name="schedule-the-notification"></a>Planifier la notification

Enfin, créez un [ToastNotifier](https://docs.microsoft.com/uwp/api/windows.ui.notifications.toastnotifier) et appelez AddToSchedule (), en transmettant votre notification Toast planifiée.

```csharp
// And your scheduled toast to the schedule
ToastNotificationManager.CreateToastNotifier().AddToSchedule(toast);
```


## <a name="cancel-scheduled-notifications"></a>Annuler les notifications planifiées

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
* [ScheduledToastNotification, classe](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledToastNotification)
