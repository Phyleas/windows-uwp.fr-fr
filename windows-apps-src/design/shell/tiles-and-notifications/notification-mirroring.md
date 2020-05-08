---
Description: Découvrez comment utiliser la mise en miroir des notifications sur vos notifications Toast.
title: Mise en miroir des notifications
label: Notification mirroring
template: detail.hbs
ms.date: 12/15/2017
ms.topic: article
keywords: Windows 10, UWP, toast, centre de maintenance dans le Cloud, mise en miroir des notifications, notification, Cross Device
ms.localizationpriority: medium
ms.openlocfilehash: b897c6574f6cbfe78406d1c624f2e3b7286ef582
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82971054"
---
# <a name="notification-mirroring"></a>Mise en miroir des notifications

La mise en miroir des notifications, alimentée par le centre de maintenance dans le Cloud, vous permet de voir les notifications de votre téléphone sur votre PC.

> [!IMPORTANT]
> **Nécessite une mise à jour anniversaire**: vous devez exécuter la version 14393 ou une version ultérieure pour voir le travail de mise en miroir des notifications. Si vous souhaitez annuler la mise en miroir de notifications de votre application, vous devez cibler le kit de développement logiciel (SDK) 14393 pour accéder aux API de mise en miroir.

Avec la mise en miroir des notifications et Cortana, les utilisateurs peuvent recevoir et agir sur les notifications de leur téléphone (Windows Mobile et Android) à partir de leur ordinateur. En tant que développeur, vous n’avez rien à faire pour activer la mise en miroir des notifications ; la mise en miroir fonctionne automatiquement. Un clic sur les boutons du Toast mis en miroir, comme les réponses rapides du message, est routé vers le téléphone, en appelant la tâche en arrière-plan ou en lançant votre application de premier plan.

<img alt="Notification mirroring diagram" src="images/toast-mirroring.gif" width="350"/>

Les développeurs bénéficient de deux grands avantages de la mise en miroir des notifications : les notifications mises en miroir entraînent davantage d’engagement utilisateur avec votre service et elles aident également les utilisateurs à découvrir votre application de bureau Microsoft Store. Vos utilisateurs peuvent même ne pas savoir que vous disposez d’une application Windows extraordinaire pour leur bureau Windows 10. Lorsque les utilisateurs reçoivent la notification mise en miroir à partir de leur téléphone, ils peuvent cliquer sur la notification toast à envoyer au Microsoft Store, où ils peuvent installer votre application Windows.

La mise en miroir fonctionne avec Windows Phone et Android. Les utilisateurs doivent être connectés à Cortana sur leur téléphone et leur bureau pour que la mise en miroir des notifications fonctionne.


## <a name="what-if-the-app-is-installed-on-both-devices"></a>Que se passe-t-il si l’application est installée sur les deux appareils ?

Si l’utilisateur a déjà votre application sur son ordinateur, nous muets automatiquement la notification par téléphone en miroir afin qu’elle ne convoit pas les notifications en double. Les notifications mises en miroir seront automatiquement mises en sourdine en fonction des critères suivants...

1. Une application sur le PC existe avec le **même nom complet ou le même PFN** (nom de la famille de packages)
2. Cette application PC a envoyé une notification Toast

Si l’application PC n’a pas encore envoyé de Toast, nous affichons toujours les notifications par téléphone, car il y a de fortes chances que l’utilisateur n’ait pas encore lancé l’application PC.


## <a name="how-to-opt-out-of-mirroring"></a>Comment refuser la mise en miroir

Les développeurs d’applications Windows, les entreprises et les utilisateurs peuvent choisir de désactiver la mise en miroir des notifications.

> [!NOTE]
> La désactivation de la mise en miroir désactive également l’opération de fermeture [universelle](universal-dismiss.md).


### <a name="as-a-developer-opt-out-an-individual-notification"></a>En tant que développeur, désabonnez une notification individuelle

Vous pouvez parfois avoir une notification spécifique à l’appareil que vous ne souhaitez pas mettre en miroir sur d’autres appareils. Vous pouvez empêcher la mise en miroir d’une notification spécifique en définissant la propriété de **mise en miroir** sur la notification Toast. Actuellement, cette propriété de mise en miroir ne peut être définie que sur les notifications locales (elle ne peut pas être spécifiée lors de l’envoi d’une notification push WNS).

**Problème connu**: la récupération de la propriété de mise en `ToastNotificationHistory.GetHistory()` miroir via les API retourne toujours la valeur par défaut (**autorisée**) au lieu de l’option que vous avez spécifiée. Ne vous inquiétez pas, tout est fonctionnel : il récupère uniquement la valeur qui est rompue.

```csharp
var toast = new ToastNotification(xml)
{
    // Disable mirroring of this notification
    Mirroring = NotificationMirroring.Disabled
};
  
ToastNotificationManager.CreateToastNotifier().Show(toast);
```


### <a name="as-a-developer-opt-out-completely"></a>En tant que développeur, désinscrivez-vous entièrement

Certains développeurs peuvent choisir d’annuler complètement la mise en miroir de notifications de leur application. Bien que nous pensons que toutes les applications tirent parti de la mise en miroir, nous pouvons facilement les refuser. Appelez simplement la méthode suivante une seule fois, et votre application sera désactivée. Par exemple, vous pouvez placer cet appel dans le constructeur de votre application `App.xaml.cs`à l’intérieur de...

```csharp
public App()
{
    this.InitializeComponent();
    this.Suspending += OnSuspending;
 
    // Disable notification mirroring for entire app
    ToastNotificationManager.ConfigureNotificationMirroring(NotificationMirroring.Disabled);
}
```


### <a name="as-an-enterprise-how-do-i-opt-out"></a>En tant qu’entreprise, comment puis-je choisir ?

Les entreprises peuvent choisir de désactiver complètement la mise en miroir des notifications. Pour ce faire, il suffit de modifier les stratégie de groupe pour désactiver la mise en miroir des notifications.


### <a name="as-a-user-how-do-i-opt-out"></a>En tant qu’utilisateur, comment puis-je me désabonner ?

Les utilisateurs sont en mesure de s’abonner à des applications individuelles ou de désactiver complètement en désactivant la fonctionnalité. Vous ne souhaiterez peut-être pas que les notifications d’une application spécifique soient mises en miroir sur votre bureau. vous pouvez donc simplement désactiver cette application spécifique. Ces options se trouvent dans les paramètres de Cortana sur votre téléphone et votre ordinateur.
