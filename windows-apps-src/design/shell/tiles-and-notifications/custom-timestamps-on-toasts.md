---
title: Horodateurs personnalisés sur les toasts
description: Découvrez comment remplacer l’horodateur par défaut sur une notification Toast par un horodateur personnalisé qui indique le moment où le message/les informations/le contenu a été généré.
label: Custom timestamps on toasts
template: detail.hbs
ms.date: 12/15/2017
ms.topic: article
keywords: Windows 10, UWP, toast, horodateur personnalisé, horodatage, notification, centre de maintenance
ms.localizationpriority: medium
ms.openlocfilehash: 11d9064d39d4e8ecd74229afc4eee325297f246b
ms.sourcegitcommit: e273e5901bfa6596dfef4cc741bb1c42614c25ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89238304"
---
# <a name="custom-timestamps-on-toasts"></a>Horodateurs personnalisés sur les toasts

Par défaut, l’horodateur des notifications Toast (visible dans le centre de notifications) est défini sur l’heure à laquelle la notification a été envoyée.

<img alt="Toast with custom timestamp" src="images/toast-customtimestamp.jpg" width="396"/>

Vous pouvez éventuellement remplacer l’horodatage par vos propres date et heure personnalisées, afin que l’horodatage représente l’heure à laquelle le message/les informations/contenu a été réellement créé, plutôt que l’heure à laquelle la notification a été envoyée. Cela garantit également que vos notifications s’affichent dans l’ordre approprié dans le centre de maintenance (qui sont triées par heure). Nous recommandons que la plupart des applications spécifient un horodateur personnalisé.

> [!IMPORTANT]
> **Nécessite les créateurs Update et 1.4.0 de la bibliothèque de notifications**: vous devez exécuter la version 15063 ou une version ultérieure pour afficher les horodateurs personnalisés. Vous devez utiliser la version 1.4.0 ou supérieure de la [bibliothèque de notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) de la boîte à outils de la communauté UWP pour assigner l’horodateur sur le contenu de votre toast.

Pour utiliser un horodateur personnalisé, affectez simplement la propriété **DisplayTimestamp** sur votre **ToastContent**.

```csharp
ToastContent toastContent = new ToastContent()
{
    DisplayTimestamp = new DateTime(2017, 04, 15, 19, 45, 00, DateTimeKind.Utc),
    ...
};
```

```xml
<toast displayTimestamp="2017-04-15T19:45:00Z">
  ...
</toast>
```

Si vous utilisez XML, la date doit être au format [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601).

> [!NOTE]
> Vous ne pouvez utiliser que 3 décimales au maximum sur les secondes (bien qu’il n’y ait pas de valeur pour fournir tout ce qui est précis). Si vous en fournissez davantage, la charge utile n’est pas valide et vous recevrez la notification « nouvelle notification ».


## <a name="usage-guidance"></a>Conseils d’utilisation

En général, nous recommandons que la plupart des applications spécifient un horodateur personnalisé. Cela garantit que l’horodateur de la notification représente avec précision le moment où le message/les informations/contenu a été généré, quels que soient les retards réseau, le mode avion ou l’intervalle fixe des tâches d’arrière-plan périodiques.

Par exemple, une application de News peut exécuter une tâche en arrière-plan toutes les 15 minutes pour rechercher de nouveaux articles et afficher des notifications. Avant les horodateurs personnalisés, l’horodateur correspond à la date de génération de la notification Toast (par conséquent, toujours dans les intervalles de 15 minutes). Toutefois, l’application peut désormais définir l’horodateur à l’heure de publication de l’article. De même, les applications de messagerie et les applications de réseau social peuvent tirer parti de cette fonctionnalité si un modèle similaire d’extraction périodique est utilisé pour les notifications.

En outre, le fait de fournir un horodateur personnalisé garantit que l’horodateur est correct même si l’utilisateur a été déconnecté d’Internet. Par exemple, lorsque l’utilisateur met son ordinateur sous tension et que votre tâche en arrière-plan est exécutée, vous pouvez enfin vous assurer que l’horodateur de vos notifications représente l’heure à laquelle les messages ont été envoyés, plutôt que l’heure à laquelle l’utilisateur a activé son ordinateur.


## <a name="default-timestamp"></a>Horodateur par défaut

Si vous ne fournissez pas un horodateur personnalisé, nous utilisons l’heure à laquelle votre notification a été envoyée.

Si vous avez envoyé une notification Push via WNS, nous utilisons l’heure à laquelle la notification a été reçue par le serveur WNS (de sorte que toute latence lors de la transmission de la notification à l’appareil n’aura pas d’impact sur l’horodateur).

Si vous avez envoyé une notification locale, nous utilisons l’heure à laquelle la plateforme de notification a reçu la notification (qui doit être immédiatement).


## <a name="related-topics"></a>Rubriques connexes

- [Envoyer un toast local](send-local-toast.md)
- [Documentation sur le contenu Toast](adaptive-interactive-toasts.md)