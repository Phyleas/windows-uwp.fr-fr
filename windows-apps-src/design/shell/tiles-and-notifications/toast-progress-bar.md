---
description: Utilisez une barre de progression à l’intérieur de votre notification toast pour transmettre l’état des opérations de longue durée à l’utilisateur.
title: Barre de progression Toast et liaison de données
label: Toast progress bar and data binding
template: detail.hbs
ms.date: 12/07/2017
ms.topic: article
keywords: Windows 10, UWP, toast, barre de progression, barre de progression Toast, notification, liaison de données Toast
ms.localizationpriority: medium
ms.openlocfilehash: 8df76af7dd26792d8d1af0fa8641d76e007ef8e3
ms.sourcegitcommit: 140bbbab0f863a7a1febee85f736b0412bff1ae7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91984525"
---
# <a name="toast-progress-bar-and-data-binding"></a>Barre de progression Toast et liaison de données

L’utilisation d’une barre de progression à l’intérieur de votre notification Toast vous permet de transmettre l’état des opérations de longue durée à l’utilisateur, comme les téléchargements, le rendu vidéo, les objectifs de l’exercice, et bien plus encore.

> [!IMPORTANT]
> **Nécessite les créateurs Update et 1.4.0 de la bibliothèque de notifications**: vous devez cibler le kit de développement logiciel 15063 et exécuter la version 15063 ou une version ultérieure pour utiliser les barres de progression sur les toasts. Vous devez utiliser la version 1.4.0 ou supérieure de la bibliothèque du kit de développement de la [série UWP notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) pour construire la barre de progression dans le contenu de votre toast.

Une barre de progression à l’intérieur d’un toast peut être « indéterminée » (aucune valeur spécifique, des points animés indiquent qu’une opération se produit) ou « se terminer » (un pourcentage spécifique de la barre est rempli, comme 60%).

> **API importantes**: [classe NotificationData](/uwp/api/windows.ui.notifications.notificationdata), [méthode ToastNotifier. Update](/uwp/api/Windows.UI.Notifications.ToastNotifier.Update), [ToastNotification, classe](/uwp/api/Windows.UI.Notifications.ToastNotification)

> [!NOTE]
> Seul Desktop prend en charge les barres de progression dans les notifications Toast. Sur les autres appareils, la barre de progression sera supprimée de votre notification.

L’image ci-dessous montre une barre de progression qui se termine avec toutes les propriétés correspondantes étiquetées.

<img alt="Toast with progress bar properties labeled" src="images/toast-progressbar-annotated.png" width="626"/>

| Propriété | Type | Obligatoire | Description |
|---|---|---|---|
| **Titre** | chaîne ou [BindableString](toast-schema.md#bindablestring) | false | Obtient ou définit une chaîne de titre facultative. Prend en charge la liaison de données. |
| **Valeur** | double ou [AdaptiveProgressBarValue](toast-schema.md#adaptiveprogressbarvalue) ou [BindableProgressBarValue](toast-schema.md#bindableprogressbarvalue) | false | Obtient ou définit la valeur de la barre de progression. Prend en charge la liaison de données. La valeur par défaut est 0. Peut être une valeur double comprise entre 0,0 et 1,0, `AdaptiveProgressBarValue.Indeterminate` ou `new BindableProgressBarValue("myProgressValue")` . |
| **ValueStringOverride** | chaîne ou [BindableString](toast-schema.md#bindablestring) | false | Obtient ou définit une chaîne facultative à afficher à la place de la chaîne de pourcentage par défaut. Si cette valeur n’est pas fournie, un nom similaire à « 70% » s’affiche. |
| **État** | chaîne ou [BindableString](toast-schema.md#bindablestring) | true | Obtient ou définit une chaîne d’État (obligatoire), qui s’affiche sous la barre de progression sur la gauche. Cette chaîne doit refléter l’état de l’opération, par exemple « téléchargement... » ou « installation... » |


Voici comment générer la notification illustrée ci-dessus...

#### <a name="builder-syntax"></a>[Syntaxe du générateur](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    .AddText("Downloading your weekly playlist...")
    .AddVisualChild(new AdaptiveProgressBar()
    {
        Title = "Weekly playlist",
        Value = 0.6,
        ValueStringOverride = "15/26 songs",
        Status = "Downloading..."
    });
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<toast>
    <visual>
        <binding template="ToastGeneric">
            <text>Downloading your weekly playlist...</text>
            <progress
                title="Weekly playlist"
                value="0.6"
                valueStringOverride="15/26 songs"
                status="Downloading..."/>
        </binding>
    </visual>
</toast>
```

---

Toutefois, vous devez mettre à jour dynamiquement les valeurs de la barre de progression pour qu’elles soient réellement « actives ». Pour ce faire, vous pouvez utiliser la liaison de données pour mettre à jour le Toast.


## <a name="using-data-binding-to-update-a-toast"></a>Utilisation de la liaison de données pour mettre à jour un toast

L’utilisation de la liaison de données implique les étapes suivantes...

1. Construire un contenu Toast qui utilise des champs liés aux données
2. Affecter une **balise** (et éventuellement un **Groupe**) à votre **ToastNotification**
3. Définir vos valeurs de **données** initiales sur votre **ToastNotification**
4. Envoyer le Toast
5. Utiliser la **balise** et le **groupe** pour mettre à jour les valeurs de **données** avec les nouvelles valeurs

L’extrait de code suivant montre les étapes 1-4. L’extrait de code suivant indique comment mettre à jour les valeurs des **données** de Toast.

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications;
 
public void SendUpdatableToastWithProgress()
{
    // Define a tag (and optionally a group) to uniquely identify the notification, in order update the notification data later;
    string tag = "weekly-playlist";
    string group = "downloads";
 
    // Construct the toast content with data bound fields
    var content = new ToastContentBuilder()
        .AddText("Downloading your weekly playlist...")
        .AddVisualChild(new AdaptiveProgressBar()
        {
            Title = "Weekly playlist",
            Value = new BindableProgressBarValue("progressValue"),
            ValueStringOverride = new BindableString("progressValueString"),
            Status = new BindableString("progressStatus")
        })
        .GetToastContent();
 
    // Generate the toast notification
    var toast = new ToastNotification(content.GetXml());
 
    // Assign the tag and group
    toast.Tag = tag;
    toast.Group = group;
 
    // Assign initial NotificationData values
    // Values must be of type string
    toast.Data = new NotificationData();
    toast.Data.Values["progressValue"] = "0.6";
    toast.Data.Values["progressValueString"] = "15/26 songs";
    toast.Data.Values["progressStatus"] = "Downloading...";
 
    // Provide sequence number to prevent out-of-order updates, or assign 0 to indicate "always update"
    toast.Data.SequenceNumber = 1;
 
    // Show the toast notification to the user
    ToastNotificationManager.CreateToastNotifier().Show(toast);
}
```

Ensuite, lorsque vous souhaitez modifier vos valeurs de **données** , utilisez la méthode [**Update**](/uwp/api/Windows.UI.Notifications.ToastNotifier.Update) pour fournir les nouvelles données sans reconstruire l’intégralité de la charge du Toast.

```csharp
using Windows.UI.Notifications;
 
public void UpdateProgress()
{
    // Construct a NotificationData object;
    string tag = "weekly-playlist";
    string group = "downloads";
 
    // Create NotificationData and make sure the sequence number is incremented
    // since last update, or assign 0 for updating regardless of order
    var data = new NotificationData
    {
        SequenceNumber = 2
    };

    // Assign new values
    // Note that you only need to assign values that changed. In this example
    // we don't assign progressStatus since we don't need to change it
    data.Values["progressValue"] = "0.7";
    data.Values["progressValueString"] = "18/26 songs";

    // Update the existing notification's data by using tag/group
    ToastNotificationManager.CreateToastNotifier().Update(data, tag, group);
}
```

L’utilisation de la méthode de **mise à jour** au lieu de remplacer l’intégralité du Toast garantit également que la notification Toast reste à la même position dans le centre de maintenance et ne se déplace pas vers le haut ou vers le haut. Il serait tout à fait confus pour l’utilisateur si le Toast s’est maintenu vers le haut du centre de maintenance toutes les quelques secondes alors que la barre de progression était pleine.

La méthode **Update** retourne une énumération, [**NotificationUpdateResult**](/uwp/api/windows.ui.notifications.notificationupdateresult), qui vous permet de savoir si la mise à jour a réussi ou si la notification est introuvable (ce qui signifie que l’utilisateur a probablement rejeté votre notification et vous devez arrêter d’envoyer des mises à jour à celle-ci). Nous vous déconseillons d’utiliser un autre Toast jusqu’à ce que votre opération de progression soit terminée (comme lorsque le téléchargement est terminé).


## <a name="elements-that-support-data-binding"></a>Éléments qui prennent en charge la liaison de données
Les éléments suivants dans les notifications Toast prennent en charge la liaison de données

- Toutes les propriétés sur **AdaptiveProgress**
- Propriété **Text** sur les éléments **AdaptiveText** de niveau supérieur


## <a name="update-or-replace-a-notification"></a>Mettre à jour ou remplacer une notification

Depuis Windows 10, vous pouviez toujours **remplacer** une notification en envoyant un nouveau Toast avec la même **balise** et le même **groupe**. Alors, quelle est la différence entre le **remplacement** du Toast et la **mise à jour** des données du Toast ?

| | Changement | Mise à jour |
| -- | -- | --
| **Position dans le centre de maintenance** | Déplace la notification vers le haut du centre de maintenance. | Laisse la notification en place dans le centre de maintenance. |
| **Modification du contenu** | Peut modifier complètement tout le contenu/la mise en page du Toast | Ne peut modifier que les propriétés qui prennent en charge la liaison de données (barre de progression et texte de niveau supérieur) |
| **Réapparaît comme Popup** | Peut réapparaître sous la forme d’une fenêtre contextuelle Toast si vous laissez **SuppressPopup** défini sur `false` (ou sur true pour l’envoyer silencieusement au centre de maintenance) | Ne réapparaît pas sous forme de fenêtre contextuelle. les données du Toast sont mises à jour silencieusement dans le centre de notifications |
| **Utilisateur rejeté** | Que l’utilisateur ait rejeté votre notification précédente, votre toast de remplacement est toujours envoyé | Si l’utilisateur a fermé votre toast, la mise à jour Toast échoue |

En général, la **mise à jour est utile pour...**

- Des informations qui changent fréquemment sur une période de temps et qui ne nécessitent pas d’être placées au début de l’attention de l’utilisateur
- Modifications subtiles apportées à votre contenu Toast, telles que la modification de 50% en 65%

Souvent, une fois que la séquence des mises à jour est terminée (comme le fichier a été téléchargé), nous vous recommandons de remplacer pour la dernière étape, car...

- Votre dernière notification a probablement des modifications de disposition drastiques, telles que la suppression de la barre de progression, l’ajout de nouveaux boutons, etc.
- L’utilisateur a peut-être ignoré votre notification de progression en attente, car il ne se soucie pas de regarder le téléchargement, mais souhaite toujours être notifié avec un toast de la fenêtre contextuelle lorsque l’opération est terminée.


## <a name="related-topics"></a>Rubriques connexes

- [Exemple de code complet sur GitHub](https://github.com/WindowsNotifications/quickstart-toast-progress-bar)
- [Documentation sur le contenu Toast](adaptive-interactive-toasts.md)