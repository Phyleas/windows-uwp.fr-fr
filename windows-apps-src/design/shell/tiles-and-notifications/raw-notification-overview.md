---
description: En savoir plus sur les notifications brutes, qui sont de courtes notifications push à usage général qui sont strictement pédagogiques et n’incluent pas de composant d’interface utilisateur.
title: Vue d’ensemble des notifications brutes
ms.assetid: A867C75D-D16E-4AB5-8B44-614EEB9179C7
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e6b01f961a28e3a6db52725c4f2f26dc3b3aaee9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172363"
---
# <a name="raw-notification-overview"></a>Vue d’ensemble des notifications brutes


Les notifications brutes sont des notifications Push courtes à usage général. Elles ont une finalité exclusivement didactique et n’incluent aucun composant d’interface utilisateur. Comme avec d’autres notifications Push, la fonctionnalité des services de notifications Push Windows (WNS) fournit des notifications brutes de votre service cloud à votre application.

Les notifications brutes peuvent être employées à diverses fins, notamment pour inciter votre application à exécuter une tâche en arrière-plan si l’utilisateur a autorisé l’application à le faire. En faisant appel à la fonctionnalité WNS pour communiquer avec votre application, vous pouvez éviter la surcharge de traitement liée à la création de connexions de sockets permanentes, à l’envoi de messages HTTP GET et à d’autres connexions entre services et applications.

> [!IMPORTANT]
> Pour comprendre ce que sont les notifications brutes, nous vous recommandons de vous familiariser avec les concepts abordés dans la rubrique [Vue d’ensemble des services de notifications Windows Push (WNS)](windows-push-notification-services--wns--overview.md).

 

Comme pour les notifications Push par toast, vignette et badge, une notification brute est transmise à partir du service cloud de votre application vers WNS par le biais d’un URI (Uniform Resource Identifier) de canal affecté. WNS, à son tour, transmet la notification à l’appareil et au compte d’utilisateur associés à ce canal. Contrairement à d’autres notifications Push, les notifications brutes n’adoptent aucun format spécifique. Le contenu de la charge utile est entièrement défini par l’application.

Pour donner un exemple d’application pouvant bénéficier de notifications brutes, examinons une application de collaboration documentaire théorique. Considérons deux utilisateurs qui modifient le même document simultanément. Le service cloud qui héberge le document partagé peut utiliser des notifications brutes pour informer chaque utilisateur dès qu’un autre utilisateur apporte des modifications. Les notifications brutes ne doivent pas nécessairement contenir les modifications apportées au document mais doit plutôt inciter chaque copie de l’application de l’utilisateur à contacter l’emplacement central et à synchroniser les modifications disponibles. À l’aide de notifications brutes, l’application et son service cloud peut enregistrer la surcharge inhérente au maintien de connexions permanentes tant que le document reste ouvert.

## <a name="how-raw-notifications-work"></a>Fonctionnement des notifications brutes


Toutes les notifications brutes sont des notifications Push. C’est pourquoi la configuration requise pour envoyer et recevoir des notifications Push concerne également les notifications brutes :

-   Vous devez disposer d’un canal WNS valide pour envoyer des notifications brutes. Pour plus d’informations sur l’acquisition d’un canal de notification Push, voir [Comment demander, créer et enregistrer un canal de notification](/previous-versions/windows/apps/hh465412(v=win.10)).
-   Vous devez inclure la fonctionnalité **Internet** dans le manifeste de votre application. Vous trouverez cette option sous la forme **Internet (client)** dans l’onglet **Capacités** de l’éditeur de manifeste Microsoft Visual Studio. Pour plus d’informations, consultez [**fonctionnalités**](/uwp/schemas/appxpackage/appxmanifestschema/element-capabilities).

Le corps de la notification apparaît dans un format défini par l’application. Le client reçoit les données sous la forme d’une chaîne (**HSTRING**) terminée par un caractère NULL que seule l’application doit comprendre.

Si le client est hors connexion, les notifications brutes seront mises en cache par la fonctionnalité WNS uniquement si l’en-tête [X-WNS-Cache-Policy](/previous-versions/windows/apps/hh465435(v=win.10)) est inclus dans la notification. Cependant, une seule notification brute sera mise en cache et publiée dès que le périphérique est de nouveau en ligne.

Il existe trois options de traitement d’une notification brute sur le client : la remise à votre application via un événement de remise de notification, l’envoi à une tâche en arrière-plan ou l’abandon. Par conséquent, si le client est hors connexion et si WNS tente d’émettre une notification brute, la notification est annulée.

## <a name="creating-a-raw-notification"></a>Création d’une notification brute


L’envoi d’une notification brute est similaire à l’envoi d’une notification push par vignette, toast ou badge avec les différences suivantes :

-   L’en-tête HTTP Content-Type doit être défini sur « application/octet-stream ».
-   L’en-tête HTTP [X-WNS-Type](/previous-versions/windows/apps/hh465435(v=win.10)) doit être défini sur « wns/raw ».
-   Le corps de la notification doit contenir une charge utile de chaînes d’une taille inférieure à 5 Ko.

Les notifications brutes sont des messages brefs conçus pour inciter votre application à agir, notamment en contactant directement le service pour synchroniser une grande quantité de données ou modifier un état local fondé sur le contenu des notifications. Notez que la remise des notifications Push WNS est impossible à garantir ; votre application et votre service cloud doivent donc tenir compte de l’éventualité que la notification brute ne parvienne pas au client, notamment lorsque celui-ci est hors connexion.

Pour plus d’informations sur l’envoi de notifications Push, voir [Démarrage rapide : envoi d’une notification Push](/previous-versions/windows/apps/hh868252(v=win.10)).

## <a name="receiving-a-raw-notification"></a>Réception d’une notification brute


Il existe deux méthodes par lesquelles votre application peut recevoir des notifications brutes :

-   Par le biais d’[événements de remise de notification](#notification-delivery-events) lorsque votre application est en cours d’exécution.
-   Au moyen de [tâches en arrière-plan déclenchées par la notification brute](#background-tasks-triggered-by-raw-notifications) si votre application autorise l’exécution de tâches en arrière-plan.

Une application peut recourir à ces deux mécanismes pour recevoir des notifications brutes. Si une application implémente à la fois le gestionnaire d’événements de remise de notification et les tâches en arrière-plan déclenchées par les notifications brutes, l’événement de remise de notification sera prioritaire lorsque l’application sera exécutée.

-   Si l’application est en cours d’exécution, l’événement de remise de notification aura priorité sur la tâche en arrière-plan, et l’application aura l’occasion de traiter la notification.
-   En définissant la propriété [**PushNotificationReceivedEventArgs.Cancel**](/uwp/api/Windows.Networking.PushNotifications.PushNotificationReceivedEventArgs.Cancel) de l’événement sur **true**, le gestionnaire d’événements de remise de notification peut préciser de ne pas transmettre la notification brute à sa tâche en arrière-plan dès que le gestionnaire se ferme. Si la propriété **Cancel** est définie sur **false** ou n’est pas définie (la valeur par défaut est **false**), la notification brute déclenchera la tâche en arrière-plan une fois le travail du gestionnaire d’événements de remise de notification terminé.

### <a name="notification-delivery-events"></a>Événements de remise de notification

Votre application peut utiliser un événement de remise des notifications ([**PushNotificationReceived**](/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannel.PushNotificationReceived)) pour recevoir des notifications brutes pendant que l’application est en cours d’utilisation. Quand le service cloud envoie une notification brute, l’application est en cours d’exécution peut la recevoir en gérant l’événement de remise de notification sur l’URI de canal.

Si votre application n’est pas en cours d’exécution et n’utilise pas de [tâches en arrière-plan](#background-tasks-triggered-by-raw-notifications)), toute notification brute envoyée à cette application est supprimée par WNS à la réception. Pour éviter de gaspiller les ressources de votre service cloud, vous pouvez envisager la mise en place d’une logique sur le service pour contrôler si l’application est active ou non. Pour ces informations, il existe deux sources : une application peut explicitement indiquer au service qu’elle est prête à recevoir des notifications et la fonctionnalité WNS peut indiquer au service quand arrêter.

-   **L’application informe le service cloud** : l’application peut contacter son service pour l’informer que l’application fonctionne au premier plan. L’inconvénient de cette approche est que l’application peut finir par contacter votre service de manière très fréquente. En revanche, elle présente l’avantage que le service saura toujours lorsque l’application est prête à recevoir des notifications brutes entrantes. Un autre avantage réside dans le fait que, lorsque l’application contacte son service, celui-ci sait alors qu’il faut envoyer des notifications brutes à l’instance spécifique de cette application plutôt que procéder à une diffusion.
-   **Le service cloud répond aux messages de réponse WNS** : le service de votre application peut utiliser les informations [X-WNS-NotificationStatus](/previous-versions/windows/apps/hh465435(v=win.10)) et [X-WNS-DeviceConnectionStatus](/previous-versions/windows/apps/hh465435(v=win.10)) renvoyées par la fonctionnalité WNS pour déterminer à quel moment cesser l’envoi de notifications brutes à l’application. Lorsque votre service envoie une notification à un canal sous la forme d’une demande POST HTTP, il peut recevoir l’un des messages suivants en guise de réponse :

    -   **X-WNS-NotificationStatus: dropped** : cela signifie que le client n’a pas reçu la notification. On peut raisonnablement supposer que la réponse **dropped** est générée en raison du fait que votre application ne figure plus au premier plan de l’appareil de l’utilisateur.
    -   **X-WNS-DeviceConnectionStatus: disconnected** ou **X-WNS-DeviceConnectionStatus: tempconnected** : ceci indique que le client Windows ne dispose plus d’une connexion à WNS. Notez que pour recevoir ce message de WNS, vous devez le demander en définissant l’en-tête [X-WNS-RequestForStatus](/previous-versions/windows/apps/hh465435(v=win.10)) dans la demande POST HTTP de la notification.

    Le service cloud de votre application peut exploiter les informations incluses dans ces messages d’état pour cesser toute tentative de communication par le biais de notifications brutes. Le service peut reprendre l’envoi de notifications brutes dès que l’application le contacte, quand celle-ci revient au premier plan.

    Notez qu’il est préférable de ne pas se fier aux informations [X-WNS-NotificationStatus](/previous-versions/windows/apps/hh465435(v=win.10)) pour déterminer si la notification a été correctement remise au client.

    Pour plus d’informations, voir [En-têtes des demandes et des réponses des services de notifications Push](/previous-versions/windows/apps/hh465435(v=win.10)).

### <a name="background-tasks-triggered-by-raw-notifications"></a>Tâches en arrière-plan déclenchées par des notifications brutes

> [!IMPORTANT]
> Avant d’utiliser des tâches en arrière-plan de notification brute, une application doit bénéficier d’un accès en arrière-plan par le biais d’un élément [**BackgroundExecutionManager.RequestAccessAsync**](/uwp/api/Windows.ApplicationModel.Background.BackgroundExecutionManager#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessAsync_System_String_).

 

Votre tâche en arrière-plan doit être inscrite avec un élément [**PushNotificationTrigger**](/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger). Si elle ne l’est pas, la tâche ne sera pas exécutée dès qu’une notification brute sera reçue.

Une tâche en arrière-plan qui est déclenchée par une notification brute permet au service cloud de votre application de contacter cette dernière même quand elle n’est pas exécutée (il peut toutefois déclencher son exécution). Cela se produit sans que l’application ne doive maintenir une connexion permanente. Les notifications brutes sont le seul type de notification capable de déclencher des tâches en arrière-plan. Cependant, s’il est impossible pour les notifications Push par vignette, toast et badge de déclencher des tâches en arrière-plan, les tâches en arrière-plan déclenchées par les notifications brutes peuvent mettre à jour les vignettes et appeler des notifications toast via des appels d’API locaux.

Pour donner un exemple illustrant la manière dont fonctionnent les tâches en arrière-plan déclenchées par des notifications brutes, imaginons une application conçue pour lire des livres électroniques. Pour commencer, un utilisateur achète un livre en ligne, peut-être sur un autre périphérique. En guise de réponse, le service cloud de l’application peut envoyer une notification brute à chacun des périphériques de l’utilisateur, avec une charge utile qui indique que le livre a été acheté et que l’application doit le télécharger. L’application contacte ensuite directement le service cloud de l’application pour lancer un téléchargement en arrière-plan du nouveau livre afin pour que, plus tard, dès que l’utilisateur démarre l’application, le livre soit déjà présent et prêt à être lu.

Pour utiliser une notification brute déclenchant une tâche en arrière-plan, votre application doit effectuer les opérations suivantes :

1.  Demandez l’autorisation d’exécuter des tâches en arrière-plan (que l’utilisateur peut révoquer à tout moment) à l’aide de [**BackgroundExecutionManager. RequestAccessAsync**](/uwp/api/Windows.ApplicationModel.Background.BackgroundExecutionManager#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessAsync_System_String_).
2.  implémenter la tâche en arrière-plan. Pour plus d’informations, consultez [prendre en charge votre application avec des tâches en arrière-plan](../../../launch-resume/support-your-app-with-background-tasks.md)

Votre tâche en arrière-plan est ensuite appelée en réponse à [**PushNotificationTrigger**](/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger), chaque fois qu’une notification brute est reçue pour votre application. Votre tâche en arrière-plan interprète alors la charge utile spécifique à l’application de la notification brute et intervient en conséquence.

Pour chaque application, une seule tâche en arrière-plan peut être exécutée à la fois. Si vous déclenchez une tâche en arrière-plan pour une application pour laquelle une tâche en arrière-plan est déjà en cours d’exécution, la première tâche en arrière-plan doit arriver à terme avant que la nouvelle ne soit exécutée.

## <a name="other-resources"></a>Autres ressources


Pour plus d’informations, téléchargez l’[exemple de notifications brutes](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Raw%20notifications%20sample%20(Windows%208)) pour Windows 8.1 et l’[exemple de notifications Push et périodiques](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Push%20and%20periodic%20notifications%20client-side%20sample%20(Windows%208)) pour Windows 8.1, puis réutilisez leur code source dans votre application Windows 10.

## <a name="related-topics"></a>Rubriques connexes

* [Recommandations en matière de notifications brutes]()
* [Démarrage rapide : création et inscription d’une tâche de notification brute en arrière-plan](/previous-versions/windows/apps/jj676800(v=win.10))
* [Démarrage rapide : interception de notifications Push dans des applications en cours d’exécution](/previous-versions/windows/apps/jj709908(v=win.10))
* [**RawNotification**](/uwp/api/Windows.Networking.PushNotifications.RawNotification)
* [**BackgroundExecutionManager.RequestAccessAsync**](/uwp/api/Windows.ApplicationModel.Background.BackgroundExecutionManager#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessAsync_System_String_)
 

 