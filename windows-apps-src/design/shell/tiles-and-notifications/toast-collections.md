---
title: Collections Toast
description: Découvrez comment organiser les notifications toast pour votre application en créant, en mettant à jour ou en supprimant des regroupements de notification dans le centre de maintenance.
label: Toast Collections
template: detail.hbs
ms.date: 05/16/2018
ms.topic: article
keywords: Windows 10, UWP, notification, collections, collection, notifications de groupe, notifications de regroupement, groupe, organiser, centre de maintenance, Toast
ms.localizationpriority: medium
ms.openlocfilehash: aff6b933e04611013761c10ad7a76824f7347855
ms.sourcegitcommit: eb725a47c700131f5975d737bd9d8a809e04943b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88970067"
---
# <a name="grouping-toast-notifications-with-collections"></a>Regroupement de notifications toast avec des collections
Utilisez des regroupements pour organiser les toasts de votre application dans le centre de maintenance. Les regroupements aident les utilisateurs à localiser plus facilement des informations dans le centre de maintenance et permettent aux développeurs de mieux gérer leurs notifications.  Les API ci-dessous permettent de supprimer, de créer et de mettre à jour les collections de notifications.

> [!IMPORTANT]
> **Nécessite Creators Update**: vous devez cibler le kit de développement logiciel (SDK) 15063 et exécuter la build 15063 ou une version ultérieure pour utiliser les collections Toast. Les API associées sont [Windows. UI. notifications. ToastCollection](https://docs.microsoft.com/uwp/api/windows.ui.notifications.toastcollection)et [Windows. UI. notifications. ToastCollectionManager](https://docs.microsoft.com/uwp/api/windows.ui.notifications.toastcollectionmanager)

Vous pouvez voir l’exemple ci-dessous avec une application de messagerie qui sépare les notifications en fonction du groupe de conversation. chaque titre (COMP SCI 160A Project chat, direct messages, Lacrosse Team Chat) est un regroupement distinct.  Notez que les notifications sont regroupées de manière distincte comme si elles se trouvaient dans une application distincte, même s’il s’agit de toutes les notifications de la même application.  Si vous recherchez une façon plus subtile d’organiser vos notifications, consultez [en-têtes Toast](toast-headers.md).  
![Exemple de collection avec deux groupes différents de notifications](images/toast-collection-example.png)

## <a name="creating-collections"></a>Créer des collections
Lorsque vous créez chaque collection, vous devez fournir un nom d’affichage et une icône qui s’affichent dans le centre de maintenance dans le cadre du titre de la collection, comme indiqué dans l’image ci-dessus. Les collections requièrent également un argument Launch pour aider l’application à accéder à l’emplacement approprié dans l’application lorsque l’utilisateur clique sur le titre de la collection.  

### <a name="create-a-collection"></a>Création d'une collection

``` csharp 
public const string toastCollectionId = "ToastCollection";

// Create a toast collection
public async void CreateToastCollection()
{
    string displayName = "Work Email"; 
    string launchArg = "NavigateToWorkEmailInbox"; 
    Uri icon = new Windows.Foundation.Uri("ms-appx:///Assets/workEmail.png");

    // Constructor
    ToastCollection workEmailToastCollection = new ToastCollection(MainPage.toastCollectionId, 
        displayName,
        launchArg, 
        icon);

    // Calls the platform to create the collection
    await ToastNotificationManager.GetDefault().GetToastCollectionManager().SaveToastCollectionAsync(workEmailToastCollection);                                 
}
```

## <a name="sending-notifications-to-a-collection"></a>Envoi de notifications à un regroupement
Nous traiterons de l’envoi de notifications à partir de trois pipelines Toast différents : local, planifié et Push.  Pour chacun de ces exemples, nous allons créer un exemple de toast à envoyer avec le code immédiatement après, puis nous allons nous concentrer sur la façon d’ajouter le toast à une collection via chaque pipeline.

Construisez la charge utile de notification :

``` csharp
public const string toastCollectionId = "MyToastCollection";

public async void SendToastToToastCollection()
{
    // Construct the notification Content
    string toastXmlString = 
        $@"<toast launch=’’>
            <visual>
                <binding template=’ToastGeneric’>
                    <text>Hello,</text>
                    <text>it’s me</text>
                </binding>
            </visual>
        </toast>";
    // Convert to XML
    XmlDocument toastXml = new XmlDocment();
    toastXml.LoadXml(toastXmlString);
    ToastNotification toast = new ToastNotification(toastXml);
```

### <a name="send-a-toast-to-a-collection"></a>Envoyer un toast à une collection

```csharp
// Get the collection notifier
var notifier = await ToastNotificationManager.GetDefault().GetToastNotifierForToastCollectionIdAsync(MainPage.toastCollectionId);

// And show the toast
notifier.Show(toast);
```

### <a name="add-a-scheduled-toast-to-a-collection"></a>Ajouter un toast planifié à un regroupement

``` csharp
// Create scheduled toast from XML above
ScheduledToastNotification scheduledToast = new ScheduledToastNotification(toastXml, DateTimeOffset.Now.AddSeconds(10));

// Get notifier
var notifier = await ToastNotificationManager.GetDefault().GetToastNotifierForToastCollectionIdAsync(MainPage.toastCollectionId);
    
// Add to schedule
notifier.AddToSchedule(scheduledToast);
```

### <a name="send-a-push-toast-to-a-collection"></a>Envoyer un toast de transmission de type push à une collection
Pour les toasts push, vous devez ajouter l’en-tête X-WNS-type au message de publication.
```csharp
// Add header to HTTP request
request.Headers.Add("X-WNS-CollectionId", collectionId); 

```

## <a name="managing-collections"></a>Gestion des regroupements
#### <a name="create-the-toast-collection-manager"></a>Créer le gestionnaire de collections Toast
Pour le reste des extraits de code de cette section « gestion des collections », nous allons utiliser le collectionManager ci-dessous.
```csharp
ToastCollectionManger collectionManager = ToastNotificationManager.GetDefault().GetToastCollectionManager();
```

#### <a name="get-all-collections"></a>Obtenir tous les regroupements

``` csharp
IReadOnlyList<ToastCollection> collections = await collectionManager.FindAllToastCollectionsAsync();
``` 

#### <a name="get-the-number-of-collections-created"></a>Obtient le nombre de collections créées

``` csharp
int toastCollectionCount = (await collectionManager.FindAllToastCollectionsAsync()).Count;
```

#### <a name="remove-a-collection"></a>Supprimer une collection

``` csharp
await collectionManager.RemoveToastCollectionAsync(MainPage.toastCollectionId);
```

#### <a name="update-a-collection"></a>Mettre à jour un regroupement
Vous pouvez mettre à jour les collections en créant une nouvelle collection avec le même ID et en enregistrant la nouvelle instance de la collection.
``` csharp
string displayName = "Updated Display Name"; 
string launchArg = "UpdatedLaunchArgs"; 
Uri icon = new Windows.Foundation.Uri("ms-appx:///Assets/updatedPicture.png");

// Construct a new toast collection with the same collection id
ToastCollection updatedToastCollection = new ToastCollection(MainPage.toastCollectionId, 
            displayName,
            launchArg, 
            icon);

// Calls the platform to update the collection by saving the new instance
await collectionManager.SaveToastCollectionAsync(updatedToastCollection);                               
```
## <a name="managing-toasts-within-a-collection"></a>Gestion des toasts au sein d’une collection
#### <a name="group-and-tag-properties"></a>Propriétés de groupe et de balise
Les propriétés Group et tag identifient ensemble de façon unique une notification dans une collection.  Le groupe (et la balise) sert de clé primaire composite (plusieurs identificateurs) pour identifier votre notification. Par exemple, si vous souhaitez supprimer ou remplacer une notification, vous devez être en mesure de spécifier la *notification* à supprimer ou à remplacer ; pour ce faire, spécifiez la balise et le groupe. Par exemple, une application de messagerie.  Le développeur peut utiliser l’ID de conversation comme groupe et l’ID de message en tant que balise.

#### <a name="remove-a-toast-from-a-collection"></a>Supprimer un toast d’une collection
Vous pouvez supprimer des toasts individuels à l’aide de la balise et des ID de groupe, ou effacer tous les toasts d’une collection.
``` csharp
// Get the history
var collectionHistory = await ToastNotificationManager.GetDefault().GetHistoryForToastCollectionAsync(MainPage.toastCollectionId);

// Remove toast
collectionHistory.Remove(tag, group); 
```

#### <a name="clear-all-toasts-within-a-collection"></a>Effacer tous les toasts au sein d’une collection
``` csharp
// Get the history
var collectionHistory = await ToastNotificationManager.GetDefault().GetHistoryForToastCollectionAsync(MainPage.toastCollectionId);

// Remove toast
collectionHistory.Clear();
```


## <a name="collections-in-notifications-visualizer"></a>Collections dans le visualiseur de notifications
Vous pouvez utiliser l’outil de [visualisation des notifications](notifications-visualizer.md) pour vous aider à concevoir vos collections. Effectuez les étapes ci-dessous :

* Cliquez sur l’icône d’engrenage dans le coin inférieur droit. 
* Sélectionnez « collections Toast ».
* Au-dessus de l’aperçu du Toast, il existe un menu déroulant « Toast collection ». Sélectionnez gérer les regroupements.
* Cliquez sur « Ajouter une collection », renseignez les détails de la collection et enregistrez.
* Vous pouvez ajouter d’autres regroupements ou cliquer sur la zone gérer les regroupements pour revenir à l’écran principal.
* Sélectionnez le regroupement auquel vous souhaitez ajouter le Toast dans le menu déroulant « regroupement Toast ».
* Quand vous activez le toast, il est ajouté à la collection appropriée dans le centre de maintenance.


## <a name="other-details"></a>Autres détails
Les collections Toast que vous créez sont également reflétées dans les paramètres de notification de l’utilisateur.  Les utilisateurs peuvent activer ou désactiver les paramètres de chaque regroupement pour activer ou désactiver ces sous-groupes.  Si les notifications sont désactivées au niveau supérieur de l’application, toutes les notifications de regroupements sont également désactivées.  En outre, chaque regroupement affiche par défaut 3 notifications dans le centre de maintenance, et l’utilisateur peut le développer pour afficher jusqu’à 20 notifications.

## <a name="related-topics"></a>Rubriques connexes

* [Contenu des toasts](adaptive-interactive-toasts.md)
* [En-têtes Toast](toast-headers.md)
* [Bibliothèque de notifications sur GitHub (qui fait partie de la boîte à outils de la communauté Windows)](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)