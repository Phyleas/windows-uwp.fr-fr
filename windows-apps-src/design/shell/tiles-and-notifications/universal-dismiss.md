---
title: Masquage universel
description: Découvrez comment utiliser la fonction de masquage universel pour faire disparaître une notification Toast d’un appareil et faire en sorte que la même notification soit ignorée sur d’autres appareils.
label: Universal Dismiss
template: detail.hbs
ms.date: 12/15/2017
ms.topic: article
keywords: Windows 10, UWP, toast, centre de maintenance dans le Cloud, rejet universel, notification, Cross Device, ignorer une fois l’opération ignorer partout
ms.localizationpriority: medium
ms.openlocfilehash: fff9315b9a5645d8a981ca899c1c37acb04de07c
ms.sourcegitcommit: cb5af00af05e838621c270173e7fde1c5d2168ef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89043451"
---
# <a name="universal-dismiss"></a>Masquage universel

L’opération de fermeture universelle, optimisée par le centre de maintenance dans le Cloud, signifie que lorsque vous ignorez une notification d’un appareil, la même notification sur vos autres appareils est également fermée.

> [!IMPORTANT]
> **Nécessite une mise à jour anniversaire**: vous devez cibler le SDK 14393 et exécuter la version 14393 ou ultérieure pour utiliser la fermeture universelle.

L’exemple courant de ce scénario est les rappels du calendrier... vous avez une application de calendrier sur vos deux appareils... vous recevez un rappel sur votre téléphone et votre bureau... vous cliquez sur Ignorer sur votre bureau... Grâce à la fermeture universelle, le rappel sur votre téléphone est également supprimé. **L’activation de la fermeture de la chaîne universelle nécessite une seule ligne de code.**

<img alt="Diagram of Universal Dismiss" src="images/universal-dismiss.gif" width="406"/>

Dans ce scénario, le fait clé est que **la même application est installée sur plusieurs appareils**, ce qui signifie que **chaque appareil reçoit déjà des notifications**. Une application de calendrier est l’exemple sous forme, car vous avez généralement la même application de calendrier installée sur votre PC Windows et votre téléphone, et chaque instance de l’application envoie déjà des rappels sur chaque appareil. En ajoutant la prise en charge de la fermeture universelle, ces instances des mêmes rappels peuvent être liées entre les appareils.


## <a name="how-to-enable-universal-dismiss"></a>Comment activer la fermeture de la configuration universelle

En tant que développeur, l’activation de la fermeture de la variable universelle est très facile. Vous devez simplement fournir un ID qui nous permet de lier chaque notification à travers les appareils, de sorte que lorsque l’utilisateur ignore une notification d’un appareil, la notification liée correspondante est rejetée de l’autre appareil.

![Diagramme de RemoteId d’un rejet universel](images/universal-dismiss-remoteid.jpg)

> **RemoteId**: identificateur qui identifie de façon unique une notification *sur les appareils*.

t ne prend qu’une seule ligne de code pour ajouter RemoteId, ce qui active la prise en charge de la fermeture universelle. La façon dont vous générez votre RemoteId dépend toutefois de vous, vous devez vous assurer qu’elle identifie de manière unique votre notification sur tous les appareils, et que le même identificateur peut être généré à partir de différentes instances de votre application exécutées sur différents appareils.

Par exemple, dans mon application collégiens Planner, je génère mon RemoteId en disant qu’il est de type « rappel », puis j’inclut l’ID de compte en ligne et l’identificateur en ligne de l’élément de la maison. Je peux générer de manière cohérente le même RemoteId, quel que soit l’appareil qui envoie la notification, puisque ces ID en ligne sont partagés entre les appareils.

```csharp
var toast = new ScheduledToastNotification(content.GetXml(), startTime);
 
// If the RemoteId property is present
if (ApiInformation.IsPropertyPresent(typeof(ScheduledToastNotification).FullName, nameof(ScheduledToastNotification.RemoteId)))
{
    // Assign the RemoteId to add support for Universal Dismiss
    toast.RemoteId = $"reminder_{account.AccountId}_{homework.Identifier}"
}
  
ToastNotificationManager.CreateToastNotifier().AddToSchedule(toast);
```

Le code suivant s’exécute à la fois sur mon téléphone et sur l’application de bureau, ce qui signifie que la notification sur les deux appareils aura le même RemoteId.

C’est tout ce que vous avez à faire ! Lorsque l’utilisateur ignore (ou clique sur) une notification, nous vérifions s’il a un RemoteId et, si c’est le cas, nous allons dépanner ce RemoteId sur tous les appareils de l’utilisateur.

**Problème connu**: la récupération du **RemoteId** via les `ToastNotificationHistory.GetHistory()` API renverra toujours une chaîne vide plutôt que le **RemoteId** que vous avez spécifié. Ne vous inquiétez pas, tout est fonctionnel : il récupère uniquement la valeur qui est rompue.

> [!NOTE]
> Si l’utilisateur ou l’entreprise désactive la [mise en miroir des notifications](notification-mirroring.md) pour votre application (ou désactive complètement la mise en miroir des notifications), l’opération de rejet universelle ne fonctionnera pas, car nous n’avons pas vos notifications dans le Cloud.


## <a name="supported-devices"></a>Appareils pris en charge

Depuis la mise à jour anniversaire, le rejet universel est pris en charge sur Windows Mobile et Windows Desktop. La fermeture de la fermeture universelle fonctionne dans les deux sens, entre PC PC, PC-Phone et téléphone-téléphone.