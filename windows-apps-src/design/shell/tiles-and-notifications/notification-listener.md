---
description: Découvrez comment utiliser l’écouteur de notification pour accéder à toutes les notifications de l’utilisateur.
title: Écouteur de notification
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Chaseable tiles
template: detail.hbs
ms.date: 06/13/2017
ms.topic: article
keywords: Windows 10, UWP, écouteur de notifications, usernotificationlistener, documentation, notifications d’accès
ms.localizationpriority: medium
ms.openlocfilehash: 90d1d0757bad5a62a987144e6a81d0a6550db384
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033662"
---
# <a name="notification-listener-access-all-notifications"></a>Écouteur de notification : accéder à toutes les notifications

L’écouteur de notification fournit l’accès aux notifications d’un utilisateur. Smartwatches et d’autres appareils, peuvent utiliser l’écouteur de notification pour envoyer les notifications du téléphone à l’appareil portable. Les applications domotiques peuvent utiliser l’écouteur de notification pour effectuer des actions spécifiques lors de la réception de notifications, par exemple faire clignoter les voyants quand vous recevez un appel. 

> [!IMPORTANT]
> **Nécessite une mise à jour anniversaire** : vous devez cibler le SDK 14393 et exécuter Build 14393 ou version ultérieure pour utiliser l’écouteur de notification.


> **API importantes** : [classe UserNotificationListener](/uwp/api/Windows.UI.Notifications.Management.UserNotificationListener), [classe UserNotificationChangedTrigger](/uwp/api/Windows.ApplicationModel.Background.UserNotificationChangedTrigger)


## <a name="enable-the-listener-by-adding-the-user-notification-capability"></a>Activer l’écouteur en ajoutant la fonctionnalité de notification de l’utilisateur 

Pour utiliser l’écouteur de notification, vous devez ajouter la fonctionnalité d’écouteur de notification utilisateur au manifeste de votre application.

1. Dans Visual Studio, dans la Explorateur de solutions, double-cliquez sur votre `Package.appxmanifest` fichier pour ouvrir le concepteur de manifeste.
2. Ouvrez l’onglet Capacités.
3. Vérifiez la capacité de l' **écouteur de notification utilisateur** .


## <a name="check-whether-the-listener-is-supported"></a>Vérifier si l’écouteur est pris en charge

Si votre application prend en charge des versions antérieures de Windows 10, vous devez utiliser la [classe ApiInformation](/uwp/api/Windows.Foundation.Metadata.ApiInformation) pour vérifier si l’écouteur est pris en charge.  Si l’écouteur n’est pas pris en charge, évitez d’exécuter des appels aux API d’écouteur.

```csharp
if (ApiInformation.IsTypePresent("Windows.UI.Notifications.Management.UserNotificationListener"))
{
    // Listener supported!
}
 
else
{
    // Older version of Windows, no Listener
}
```


## <a name="requesting-access-to-the-listener"></a>Demande d’accès à l’écouteur

Étant donné que l’écouteur autorise l’accès aux notifications de l’utilisateur, les utilisateurs doivent accorder à votre application l’autorisation d’accéder à leurs notifications. Lors de la première exécution de votre application, vous devez demander l’accès pour utiliser l’écouteur de notification. Si vous le souhaitez, vous pouvez afficher une interface utilisateur préliminaire qui explique pourquoi votre application a besoin d’accéder aux notifications de l’utilisateur avant d’appeler [RequestAccessAsync](/uwp/api/windows.ui.notifications.management.usernotificationlistener.RequestAccessAsync), afin que l’utilisateur comprenne la raison pour laquelle il doit autoriser l’accès.

```csharp
// Get the listener
UserNotificationListener listener = UserNotificationListener.Current;
 
// And request access to the user's notifications (must be called from UI thread)
UserNotificationListenerAccessStatus accessStatus = await listener.RequestAccessAsync();
 
switch (accessStatus)
{
    // This means the user has granted access.
    case UserNotificationListenerAccessStatus.Allowed:
 
        // Yay! Proceed as normal
        break;
 
    // This means the user has denied access.
    // Any further calls to RequestAccessAsync will instantly
    // return Denied. The user must go to the Windows settings
    // and manually allow access.
    case UserNotificationListenerAccessStatus.Denied:
 
        // Show UI explaining that listener features will not
        // work until user allows access.
        break;
 
    // This means the user closed the prompt without
    // selecting either allow or deny. Further calls to
    // RequestAccessAsync will show the dialog again.
    case UserNotificationListenerAccessStatus.Unspecified:
 
        // Show UI that allows the user to bring up the prompt again
        break;
}
```

L’utilisateur peut révoquer l’accès à tout moment via les paramètres Windows. Par conséquent, votre application doit toujours vérifier l’état de l’accès à l’aide de la méthode [GetAccessStatus](/uwp/api/windows.ui.notifications.management.usernotificationlistener.GetAccessStatus) avant d’exécuter du code qui utilise l’écouteur de notification. Si l’utilisateur révoque l’accès, les API échouent silencieusement au lieu de lever une exception (par exemple, l’API pour obtenir toutes les notifications renverra simplement une liste vide).


## <a name="access-the-users-notifications"></a>Accéder aux notifications de l’utilisateur

Avec l’écouteur de notification, vous pouvez obtenir la liste des notifications actuelles de l’utilisateur. Appelez simplement la méthode [GetNotificationsAsync](/uwp/api/windows.ui.notifications.management.usernotificationlistener.GetNotificationsAsync) et spécifiez le type de notifications que vous souhaitez obtenir (actuellement, le seul type de notifications prises en charge sont les notifications Toast).

```csharp
// Get the toast notifications
IReadOnlyList<UserNotification> notifs = await listener.GetNotificationsAsync(NotificationKinds.Toast);
```


## <a name="displaying-the-notifications"></a>Affichage des notifications

Chaque notification est représentée sous la forme d’un [UserNotification](/uwp/api/windows.ui.notifications.usernotification), qui fournit des informations sur l’application dont provient la notification, l’heure à laquelle la notification a été créée, l’ID de la notification et la notification elle-même.

```csharp
public sealed class UserNotification
{
    public AppInfo AppInfo { get; }
    public DateTimeOffset CreationTime { get; }
    public uint Id { get; }
    public Notification Notification { get; }
}
```

La propriété [appinfo](/uwp/api/windows.ui.notifications.usernotification.AppInfo) fournit les informations dont vous avez besoin pour afficher la notification.

> [!NOTE]
> Nous vous recommandons de placer tout votre code pour traiter une seule notification dans un bloc try/catch, si une exception inattendue se produit lorsque vous capturez une seule notification. Vous ne devriez pas complètement faire échouer l’affichage d’autres notifications en raison d’un problème avec une notification spécifique.

```csharp
// Select the first notification
UserNotification notif = notifs[0];
 
// Get the app's display name
string appDisplayName = notif.AppInfo.DisplayInfo.DisplayName;
 
// Get the app's logo
BitmapImage appLogo = new BitmapImage();
RandomAccessStreamReference appLogoStream = notif.AppInfo.DisplayInfo.GetLogo(new Size(16, 16));
await appLogo.SetSourceAsync(await appLogoStream.OpenReadAsync());
```

Le contenu de la notification lui-même, tel que le texte de notification, est contenu dans la propriété de [notification](/uwp/api/windows.ui.notifications.usernotification.Notification) . Cette propriété contient la partie visuelle de la notification. (Si vous êtes familiarisé avec l’envoi de notifications sur Windows, vous remarquerez que les propriétés [visuelles](/uwp/api/windows.ui.notifications.notification.Visual) et visuelles [. Bindings](/uwp/api/windows.ui.notifications.notificationvisual.Bindings) de l’objet de [notification](/uwp/api/windows.ui.notifications.notification) correspondent à ce que les développeurs envoient lorsqu’ils dépassent une notification.)

Nous souhaitons Rechercher la liaison Toast (pour le code de vérification d’erreur, vous devez vérifier que la liaison n’est pas null). À partir de la liaison, vous pouvez obtenir les éléments de texte. Vous pouvez choisir d’afficher autant d’éléments de texte que vous le souhaitez. (Idéalement, vous devriez les afficher tous.) Vous pouvez choisir de traiter les éléments de texte différemment. par exemple, considérez la première comme un texte de titre et les éléments suivants comme corps de texte.

```csharp
// Get the toast binding, if present
NotificationBinding toastBinding = notif.Notification.Visual.GetBinding(KnownNotificationBindings.ToastGeneric);
 
if (toastBinding != null)
{
    // And then get the text elements from the toast binding
    IReadOnlyList<AdaptiveNotificationText> textElements = toastBinding.GetTextElements();
 
    // Treat the first text element as the title text
    string titleText = textElements.FirstOrDefault()?.Text;
 
    // We'll treat all subsequent text elements as body text,
    // joining them together via newlines.
    string bodyText = string.Join("\n", textElements.Skip(1).Select(t => t.Text));
}
```


## <a name="remove-a-specific-notification"></a>Supprimer une notification spécifique

Si votre service ou portable autorise l’utilisateur à ignorer les notifications, vous pouvez supprimer la notification proprement dite afin que l’utilisateur ne l’affiche plus par la suite sur son téléphone ou sur son PC. Il vous suffit de fournir l’ID de notification (obtenu à partir de l’objet [UserNotification](/uwp/api/windows.ui.notifications.usernotification) ) de la notification que vous souhaitez supprimer : 

```csharp
// Remove the notification
listener.RemoveNotification(notifId);
```


## <a name="clear-all-notifications"></a>Effacer toutes les notifications

La méthode [UserNotificationListener. ClearNotifications](/uwp/api/windows.ui.notifications.management.usernotificationlistener.ClearNotifications) efface toutes les notifications de l’utilisateur. Utilisez cette méthode avec précaution. Vous ne devez effacer que toutes les notifications si votre service ou portable affiche toutes les notifications. Si votre service ou portable affiche uniquement certaines notifications, lorsque l’utilisateur clique sur le bouton « Effacer les notifications », l’utilisateur ne s’attend à supprimer que les notifications spécifiques. Toutefois, l’appel de la méthode [ClearNotifications](/uwp/api/windows.ui.notifications.management.usernotificationlistener.ClearNotifications) entraînerait la suppression de toutes les notifications, y compris celles que votre service ou portable n’affichait pas.

```csharp
// Clear all notifications. Use with caution.
listener.ClearNotifications();
```


## <a name="background-task-for-notification-addeddismissed"></a>Tâche en arrière-plan pour la notification ajoutée/fermée

Une méthode courante pour permettre à une application d’écouter les notifications consiste à configurer une tâche en arrière-plan, afin que vous puissiez savoir quand une notification a été ajoutée ou fermée, que votre application soit en cours d’exécution ou non.

Grâce au [modèle à processus unique](../../../launch-resume/create-and-register-an-inproc-background-task.md) ajouté dans la mise à jour anniversaire, l’ajout de tâches en arrière-plan est relativement simple. Dans le code de votre application principale, après avoir obtenu l’accès de l’utilisateur à l’écouteur de notifications et obtenu l’accès pour exécuter des tâches en arrière-plan, il vous suffit d’enregistrer une nouvelle tâche en arrière-plan et de définir [UserNotificationChangedTrigger](/uwp/api/windows.applicationmodel.background.usernotificationchangedtrigger) à l’aide du [type de notification Toast](/uwp/api/windows.ui.notifications.notificationkinds).

```csharp
// TODO: Request/check Listener access via UserNotificationListener.Current.RequestAccessAsync
 
// TODO: Request/check background task access via BackgroundExecutionManager.RequestAccessAsync
 
// If background task isn't registered yet
if (!BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name.Equals("UserNotificationChanged")))
{
    // Specify the background task
    var builder = new BackgroundTaskBuilder()
    {
        Name = "UserNotificationChanged"
    };
 
    // Set the trigger for Listener, listening to Toast Notifications
    builder.SetTrigger(new UserNotificationChangedTrigger(NotificationKinds.Toast));
 
    // Register the task
    builder.Register();
}
```

Ensuite, dans votre App.xaml.cs, remplacez la méthode [OnBackgroundActivated](/uwp/api/windows.ui.xaml.application.OnBackgroundActivated) si vous ne l’avez pas encore fait, et utilisez une instruction switch sur le nom de la tâche pour déterminer lequel de vos nombreux déclencheurs de tâche en arrière-plan a été appelé.

```csharp
protected override async void OnBackgroundActivated(BackgroundActivatedEventArgs args)
{
    var deferral = args.TaskInstance.GetDeferral();
 
    switch (args.TaskInstance.Task.Name)
    {
        case "UserNotificationChanged":
            // Call your own method to process the new/removed notifications
            // The next section of documentation discusses this code
            await MyWearableHelpers.SyncNotifications();
            break;
    }
 
    deferral.Complete();
}
```

La tâche en arrière-plan est simplement un « robinet d’épaule » : elle ne fournit pas d’informations sur la notification spécifique qui a été ajoutée ou supprimée. Lorsque votre tâche en arrière-plan est déclenchée, vous devez synchroniser les notifications sur votre portable afin qu’elles reflètent les notifications dans la plateforme. Cela garantit que si votre tâche en arrière-plan échoue, les notifications sur votre portable peuvent toujours être récupérées lors de la prochaine exécution de la tâche en arrière-plan.

`SyncNotifications` est une méthode que vous implémentez ; la section suivante montre comment procéder. 


## <a name="determining-which-notifications-were-added-and-removed"></a>Détermination des notifications qui ont été ajoutées et supprimées

Dans votre `SyncNotifications` méthode, pour déterminer quelles notifications ont été ajoutées ou supprimées (en synchronisant des notifications avec votre portable), vous devez calculer le delta entre votre collection de notifications actuelle et les notifications dans la plateforme.

```csharp
// Get all the current notifications from the platform
IReadOnlyList<UserNotification> userNotifications = await listener.GetNotificationsAsync(NotificationKinds.Toast);
 
// Obtain the notifications that our wearable currently has displayed
IList<uint> wearableNotificationIds = GetNotificationsOnWearable();
 
// Copy the currently displayed into a list of notification ID's to be removed
var toBeRemoved = new List<uint>(wearableNotificationIds);
 
// For each notification in the platform
foreach (UserNotification userNotification in userNotifications)
{
    // If we've already displayed this notification
    if (wearableNotificationIds.Contains(userNotification.Id))
    {
        // We want to KEEP it displayed, so take it out of the list
        // of notifications to remove.
        toBeRemoved.Remove(userNotification.Id);
    }
 
    // Otherwise it's a new notification
    else
    {
        // Display it on the Wearable
        SendNotificationToWearable(userNotification);
    }
}
 
// Now our toBeRemoved list only contains notification ID's that no longer exist in the platform.
// So we will remove all those notifications from the wearable.
foreach (uint id in toBeRemoved)
{
    RemoveNotificationFromWearable(id);
}
```


## <a name="foreground-event-for-notification-addeddismissed"></a>Événement de premier plan pour la notification ajoutée/ignorée

> [!IMPORTANT] 
> Problème connu : dans les builds antérieures à la version 17763/octobre 2018 mise à jour/version 1809, l’événement de premier plan entraîne une boucle d’UC et/ou n’a pas fonctionné. Si vous avez besoin d’une prise en charge sur ces versions antérieures, utilisez la tâche en arrière-plan à la place.

Vous pouvez également écouter les notifications à partir d’un gestionnaire d’événements en mémoire...

```csharp
// Subscribe to foreground event
listener.NotificationChanged += Listener_NotificationChanged;
 
private void Listener_NotificationChanged(UserNotificationListener sender, UserNotificationChangedEventArgs args)
{
    // Your code for handling the notification
}
```


## <a name="how-to-fix-delays-in-the-background-task"></a>Comment corriger des retards dans la tâche en arrière-plan

Lorsque vous testez votre application, vous remarquerez peut-être que la tâche en arrière-plan est parfois retardée et ne se déclenche pas pendant plusieurs minutes. Pour résoudre ce problème, invitez l’utilisateur à accéder aux paramètres système-> système-> > l’utilisation de la batterie par l’application, recherchez votre application dans la liste, sélectionnez-la, puis définissez-la comme « toujours autorisée en arrière-plan ». Après cela, la tâche en arrière-plan doit toujours être déclenchée dans environ une seconde de la notification en cours de réception.
