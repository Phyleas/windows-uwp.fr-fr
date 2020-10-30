---
description: Découvrez comment inscrire votre application UWP pour recevoir des notifications push que vous envoyez à partir de l’espace partenaires.
title: Configurer votre application pour les notifications Push ciblées
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, kit de développement logiciel (SDK) Microsoft Store services, notifications push ciblées, espace partenaires
ms.assetid: 30c832b7-5fbe-4852-957f-7941df8eb85a
ms.localizationpriority: medium
ms.openlocfilehash: 2296ae29ddcfd868e31c294f8859d4f4b925fca8
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033532"
---
# <a name="configure-your-app-for-targeted-push-notifications"></a>Configurer votre application pour les notifications Push ciblées

Vous pouvez utiliser la page **notifications push** dans l’espace partenaires pour contacter directement les clients en envoyant des notifications push ciblées aux appareils sur lesquels votre application plateforme Windows universelle (UWP) est installée. Vous pouvez utiliser des notifications push ciblées afin d’inciter vos clients à effectuer une action, par exemple évaluer une application ou essayer une nouvelle fonctionnalité. Vous pouvez envoyer différents types de notifications Push, dont les notifications toast, les notifications par vignette et les notifications XML brutes. Vous pouvez également effectuer le suivi des lancements d’applications provoqués par vos notifications Push. Pour plus d’informations sur cette fonctionnalité, consultez la page [Envoyer des notifications Push aux clients de vos applications](../publish/send-push-notifications-to-your-apps-customers.md).

Avant de pouvoir envoyer des notifications push ciblées à vos clients à partir de l’espace partenaires, vous devez utiliser une méthode de la classe [StoreServicesEngagementManager](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager) dans le kit de développement logiciel (SDK) Microsoft Store services pour inscrire votre application afin de recevoir des notifications. Vous pouvez utiliser d’autres méthodes de cette classe pour notifier le Centre des partenaires que votre application a été lancée en réponse à une notification push ciblée (si vous souhaitez suivre le taux de lancement d’application qui résultent de vos notifications) et pour cesser de recevoir des notifications.

## <a name="configure-your-project"></a>Configurer votre projet

Avant d’écrire du code, suivez ces étapes afin d’ajouter une référence au Microsoft Store Services SDK dans votre projet :

1. Si vous ne l’avez pas encore fait, [Installez le Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk) sur votre ordinateur de développement. 
2. Ouvrez votre projet dans Visual Studio.
3. Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le nœud **Références** , puis sélectionnez **Ajouter une référence** .
4. Dans le **Gestionnaire de références** , développez **Windows universel** , puis cliquez sur **Extensions** .
5. Dans la liste des kits de développement logiciel (SDK), cochez la case en regard de **Microsoft Engagement Framework** et cliquez sur **OK** .

## <a name="register-for-push-notifications"></a>Inscription aux notifications Push

Pour inscrire votre application afin de recevoir des notifications push ciblées de l’espace partenaires :

1. Dans votre projet, localisez une section de code qui s’exécute au démarrage, dans laquelle vous pouvez inscrire votre application pour recevoir des notifications.
2. Ajoutez l’instruction suivante en haut du fichier de code.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/DevCenterNotifications.cs" id="EngagementNamespace":::

3. Récupérez un objet [StoreServicesEngagementManager](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager) et appelez l’une des surcharges [RegisterNotificationChannelAsync](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) dans le code de démarrage identifié plus tôt. Cette méthode doit être appelée à chaque lancement de l’application.

  * Si vous souhaitez que l’espace partenaires crée son propre URI de canal pour les notifications, appelez la surcharge [RegisterNotificationChannelAsync ()](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) .

      :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/DevCenterNotifications.cs" id="RegisterNotificationChannelAsync1":::
      > [!IMPORTANT]
      > Si votre application appelle également [CreatePushNotificationChannelForApplicationAsync](/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync) pour créer un canal de notification pour WNS, assurez-vous que votre code n’appelle pas [CreatePushNotificationChannelForApplicationAsync](/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync) et la surcharge [RegisterNotificationChannelAsync ()](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) simultanément. Si vous devez appeler les deux méthodes, assurez-vous de les appeler séquentiellement et d’attendre le retour d’une méthode avant d’appeler l’autre.

  * Si vous souhaitez spécifier l’URI de canal à utiliser pour les notifications push ciblées à partir de l’espace partenaires, appelez la surcharge [RegisterNotificationChannelAsync (StoreServicesNotificationChannelParameters)](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) . Par exemple, vous pouvez procéder ainsi si votre application utilise déjà les services de notifications Push Windows (WNS) et que vous souhaitez utiliser le même URI de canal. Vous devez dans un premier temps créer l’objet [StoreServicesNotificationChannelParameters](/uwp/api/microsoft.services.store.engagement.storeservicesnotificationchannelparameters) et affecter la propriété [CustomNotificationChannelUri](/uwp/api/microsoft.services.store.engagement.storeservicesnotificationchannelparameters.customnotificationchanneluri) à votre URI de canal.

      :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/DevCenterNotifications.cs" id="RegisterNotificationChannelAsync2":::

> [!NOTE]
> Quand vous appelez la méthode **RegisterNotificationChannelAsync** , un fichier nommé MicrosoftStoreEngagementSDKId.txt est créé dans le magasin de données de l’application locale pour votre application (le dossier renvoyé par la propriété [ApplicationData. LocalFolder](/uwp/api/Windows.Storage.ApplicationData.LocalFolder) ). Ce fichier contient un ID qui est utilisé par l’infrastructure de notifications push ciblée. Assurez-vous que votre application ne modifie pas ou ne supprime pas ce fichier. Dans le cas contraire, vos utilisateurs peuvent recevoir plusieurs instances de notifications, ou les notifications peuvent ne pas se comporter correctement d’une autre manière.

<span id="notification-customers" />

### <a name="how-targeted-push-notifications-are-routed-to-customers"></a>Comment les notifications push ciblées sont routées vers les clients

Lorsque votre application appelle **RegisterNotificationChannelAsync** , cette méthode collecte les compte Microsoft du client qui est actuellement connecté à l’appareil. Plus tard, lorsque vous envoyez une notification push ciblée à un segment qui comprend ce client, l’espace partenaires envoie la notification aux appareils associés à la compte Microsoft de ce client.

Si le client qui a démarré votre application envoie son appareil à une autre personne pour qu’il l’utilise alors qu’il est toujours connecté à l’appareil avec son compte Microsoft, sachez que l’autre personne peut voir la notification ciblée sur le client d’origine. Cela peut avoir des conséquences inattendues, en particulier pour les applications qui offrent des services que les clients peuvent utiliser pour se connecter. Pour empêcher d’autres utilisateurs de voir vos notifications ciblées dans ce scénario, appelez la méthode [UnregisterNotificationChannelAsync](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.unregisternotificationchannelasync) lorsque les clients se déconnectent de votre application. Pour plus d’informations, consultez [Annuler l’inscription aux notifications push](#unregister) plus loin dans cet article.

### <a name="how-your-app-responds-when-the-user-launches-your-app"></a>Comment votre application répond quand l’utilisateur lance votre application

Une fois que votre application est inscrite pour recevoir des notifications et que vous [envoyez une notification push aux clients de votre application à partir de l’espace partenaires](../publish/send-push-notifications-to-your-apps-customers.md), l’un des points d’entrée suivants dans votre application est appelé quand l’utilisateur lance votre application en réponse à votre notification push. Si vous possédez du code à exécuter lorsque l’utilisateur lance votre application, vous pouvez l’ajouter à l’un de ces points d’entrée dans votre application.

  * Si la notification Push présente un type d’activation au premier plan, supprimez la méthode [OnActivated](/uwp/api/windows.ui.xaml.application.onactivated) de la classe **App** dans votre projet et ajoutez votre code à cette méthode.

  * Si la notification Push présente un type d’activation en arrière-plan, ajoutez votre code à la méthode [Run](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) de votre [tâche en arrière-plan](../launch-resume/support-your-app-with-background-tasks.md).

Par exemple, vous pouvez récompenser les utilisateurs de votre application qui ont fait l’acquisition d’extensions payantes en leur octroyant gratuitement une autre extension. Dans ce cas, vous pouvez envoyer une notification Push à un [segment de clients](../publish/create-customer-segments.md) ciblant ces utilisateurs. Ensuite, vous pouvez ajouter du code afin de leur procurer un [achat in-app](in-app-purchases-and-trials.md) dans l’un des points d’entrée répertoriés ci-dessus.

## <a name="notify-partner-center-of-your-app-launch"></a>Notifier le centre partenaires de votre lancement d’application

Si vous sélectionnez l’option **suivre le taux de lancement des applications** pour votre notification push ciblée dans l’espace partenaires, appelez la méthode [ParseArgumentsAndTrackAppLaunch](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch) à partir du point d’entrée approprié dans votre application pour notifier le Centre des partenaires que votre application a été lancée en réponse à une notification push.

Cette méthode renvoie également les arguments de lancement d’origine associés à votre application. Lorsque vous choisissez d’effectuer le suivi du taux de lancement d’application pour votre notification push, un ID de suivi opaque est ajouté aux arguments de lancement pour faciliter le suivi du lancement de l’application dans l’espace partenaires. Vous devez passer les arguments de lancement de votre application à la méthode [ParseArgumentsAndTrackAppLaunch](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch) , et cette méthode envoie l’ID de suivi à l’espace partenaires, supprime l’ID de suivi des arguments de lancement et retourne les arguments de lancement d’origine dans votre code.

La façon dont vous appelez cette méthode dépend du type d’activation de la notification push :

* Si la notification Push présente un type d’activation au premier plan, appelez cette méthode depuis la substitution de méthode [OnActivated](/uwp/api/windows.ui.xaml.application.onactivated) de votre application et communiquez les arguments disponibles dans l’objet [ToastNotificationActivatedEventArgs](/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs) transmis à cette méthode. L’exemple de code suivant suppose que votre fichier de code **utilise** des instructions pour les espaces de noms **Microsoft. services. Store. engagement** et  **Windows. ApplicationModel. activation** .

  :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/App.xaml.cs" id="OnActivated":::

* Si la notification Push présente un type d’activation en arrière-plan, appelez cette méthode depuis la méthode [Run](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) de votre [tâche en arrière-plan](../launch-resume/support-your-app-with-background-tasks.md) et transmettez les arguments qui sont disponibles dans l’objet [ToastNotificationActionTriggerDetail](/uwp/api/Windows.UI.Notifications.ToastNotificationActionTriggerDetail) transmis à cette méthode. L’exemple de code suivant suppose que votre fichier de code contient des instructions **using** pour les espaces de noms **Microsoft.Services.Store.Engagement** , **Windows.ApplicationModel.Background** et **Windows.UI.Notifications** .

  :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/DevCenterNotifications.cs" id="Run":::

<span id="unregister" />

## <a name="unregister-for-push-notifications"></a>Annuler l’inscription aux notifications Push

Si vous souhaitez que votre application cesse de recevoir des notifications push ciblées à partir de l’espace partenaires, appelez la méthode [UnregisterNotificationChannelAsync](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.unregisternotificationchannelasync) .

:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/DevCenterNotifications.cs" id="UnregisterNotificationChannelAsync":::

Notez que cette méthode invalide le canal utilisé pour les notifications et donc que l’application ne reçoit plus de notifications Push d’ *aucun* service. Une fois fermé, le canal ne peut plus être réutilisé pour aucun service, y compris les notifications push ciblées de l’espace partenaires et d’autres notifications utilisant WNS. Pour réactiver l’envoi des notifications Push à cette application, l’application doit demander un nouveau canal.

## <a name="related-topics"></a>Rubriques connexes

* [Envoyer des notifications Push aux clients de votre application](../publish/send-push-notifications-to-your-apps-customers.md)
* [Vue d’ensemble des services de notifications Push Windows (WNS)](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)
* [Comment demander, créer et enregistrer un canal de notification](/previous-versions/windows/apps/hh868221(v=win.10))
* [Microsoft Store Services SDK](./microsoft-store-services-sdk.md)
