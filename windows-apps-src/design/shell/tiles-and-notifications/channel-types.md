---
Description: Windows Push Notification Services (WNS) permet aux développeurs tiers d’envoyer des toasts, des vignettes, des badges et des mises à jour brutes à partir de leur propre service Cloud. Il existe de nombreuses façons d’envoyer les notifications en fonction des besoins de votre application
title: Choix du type de canal de notification push approprié
ms.date: 07/07/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 502395d1daa698e1b05e40f355e65f074219e9a5
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970854"
---
# <a name="choosing-the-right-push-notification-channel-type"></a>Choix du type de canal de notification push approprié

Cet article traite des trois types de canaux de notifications push Windows (principaux, secondaires et secondaires) qui vous permettent de fournir du contenu à votre application. 

(Pour plus d’informations sur la création de notifications push, consultez la [vue d’ensemble de Windows push Notification Services (WNS)](../tiles-and-notifications/windows-push-notification-services--wns--overview.md).) 

## <a name="types-of-push-channels"></a>Types de canaux Push 

Il existe trois types de canaux push qui peuvent être utilisés pour envoyer des notifications à une application Windows. Il s'agit de : 

[Canal principal](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanagerforuser#Methods_) : canal Push « traditionnel ». Peut être utilisé par n’importe quelle application du Store pour envoyer des notifications Toast, vignette, brut ou badge. [Pour en savoir plus](windows-push-notification-services--wns--overview.md), cliquez ici.

[Canal de vignette secondaire](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanagerforuser#Methods_) -utilisé pour envoyer des mises à jour de vignette à une vignette secondaire. Peut uniquement être utilisé pour envoyer des notifications de vignette ou de badge à une vignette secondaire épinglée sur l’écran d’accueil de l’utilisateur

[Autre canal](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanagerforuser#Methods_) : un nouveau type de canal ajouté dans Creators Update. Il permet d’envoyer des notifications brutes à n’importe quelle application Windows, y compris celles qui ne sont pas inscrites dans le magasin. 

> [!NOTE]
> Quel que soit le canal Push que vous utilisez, une fois que votre application est exécutée sur l’appareil, elle est toujours en mesure d’envoyer des notifications Toast, vignette ou badge locales. Il peut envoyer des notifications locales à partir des processus de l’application de premier plan ou d’une tâche en arrière-plan. 


## <a name="primary-channels"></a>Canaux principaux

Il s’agit des canaux les plus couramment utilisés sur Windows, qui sont corrects pour presque tous les scénarios où votre application va être distribuée via le Microsoft Store. Elles vous permettent d’envoyer tous les types de notifications à l’application. 

### <a name="what-do-primary-channels-enable"></a>Que les canaux principaux activent-ils ?

-   **Envoi des mises à jour des vignettes ou des badges à la vignette principale.** Si l’utilisateur a choisi d’épingler votre vignette sur l’écran d’accueil, vous avez la possibilité de l’afficher. Envoyer des mises à jour avec des informations utiles ou des rappels d’expériences au sein de votre application. 
-   **Envoi de notifications Toast.** Les notifications Toast permettent d’obtenir immédiatement des informations devant l’utilisateur. Ils sont peints par le shell au-dessus de la plupart des applications et se trouvent dans le centre de maintenance afin que l’utilisateur puisse y revenir et interagir ultérieurement. 
-   **Envoi de notifications brutes pour déclencher une tâche en arrière-plan.** Parfois, vous souhaitez effectuer un travail au nom de l’utilisateur en fonction d’une notification. Les notifications brutes permettent l’exécution des tâches en arrière-plan de votre application 
-   **Chiffrement des messages en transit fourni par Windows à l’aide de TLS.** Les messages sont chiffrés sur le câble à la fois dans WNS et sur l’appareil de l’utilisateur.  

### <a name="limitations-of-primary-channels"></a>Limitations des canaux principaux

-   Requiert l’utilisation de l’API REST WNS pour envoyer des notifications, ce qui n’est pas standard entre les fournisseurs de périphériques. 
-   Un seul canal peut être créé par application 
-   Nécessite l’inscription de votre application dans le Microsoft Store

### <a name="creating-a-primary-channel"></a>Création d’un canal principal 

```csharp
PushNotificationChannel channel = 
    await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
```

## <a name="secondary-tile-channels"></a>Canaux de vignettes secondaires

Il s’agit de canaux qui peuvent être utilisés pour envoyer des mises à jour de vignettes et de badges à une vignette secondaire. Ils sont utilisés par les applications pour informer les utilisateurs d’actions ou d’informations intéressantes avec lesquelles ils peuvent interagir dans l’application, telles que les nouveaux messages dans une conversation de groupe ou un score sportif mis à jour. 

### <a name="what-do-secondary-tile-channels-enable"></a>Quels sont les canaux de vignette secondaires activés ?

-   Envoi de notifications de vignette ou de badge à des vignettes secondaires. Les vignettes secondaires sont un excellent moyen de replacer les utilisateurs dans votre application. Ils constituent un lien profond vers les informations qui les intéressent et le fait de placer des informations pertinentes sur les vignettes permet de les ramener et de les replacer.
-   Séparation des canaux (et des expirations) entre les différentes vignettes. Cela vous permet de séparer la logique du backend entre les différents types de vignettes secondaires qu’un utilisateur peut épingler à l’écran d’accueil. 
-   Chiffrement des messages en transit fourni par Windows à l’aide de TLS. Les messages sont chiffrés sur le câble à la fois dans WNS et sur l’appareil de l’utilisateur.  

### <a name="limitations-of-secondary-tile-channels"></a>Limitations des canaux de vignettes secondaires

-   Aucun Toast ou notifications brutes n’est autorisé. Les notifications Toast ou brutes envoyées à une vignette secondaire sont ignorées par le système.
-   Nécessite l’inscription de votre application dans le Microsoft Store


### <a name="creating-a-secondary-tile-channel"></a>Création d’un canal de vignette secondaire 

```csharp
PushNotificationChannel channel = 
    await PushNotificationChannelManager.CreatePushNotificationChannelForSecondaryTileAsync(tileId);
```

## <a name="alternate-channels"></a>Autres canaux

Les autres canaux permettent aux applications d’envoyer des notifications push sans s’inscrire au Microsoft Store ou de créer des canaux Push en dehors du serveur principal utilisé pour l’application. 
 
### <a name="what-do-alternate-channels-enable"></a>Quels sont les autres canaux activés ?
-   Envoyer des notifications push brutes à une fenêtre qui s’exécute sur n’importe quel appareil Windows. Les autres canaux autorisent uniquement les notifications brutes (Toutefois, vous pouvez toujours mettre en éveil une tâche en arrière-plan pour afficher localement les notifications de Toast ou de vignette).
-   Permet aux applications de créer plusieurs canaux Push bruts pour différentes fonctionnalités au sein de l’application. Une application peut créer jusqu’à 1000 autres canaux et chacun d’entre eux est valable pendant 30 jours. Chacun de ces canaux peut être géré ou révoqué séparément par l’application.
-   Vous pouvez créer d’autres canaux Push sans inscrire une application auprès du Microsoft Store. Si vous souhaitez installer l’application sur des appareils sans l’inscrire dans le Microsoft Store, elle sera toujours en mesure de recevoir des notifications push.
-   Les serveurs peuvent envoyer des notifications à l’aide des API REST standard du W3C et du protocole VAPID. Les autres canaux utilisent le protocole W3C standard, ce qui vous permet de simplifier la logique du serveur qui doit être maintenue.
-   Chiffrement complet de bout en bout des messages. Tandis que le canal principal fournit un chiffrement en transit, si vous souhaitez bénéficier d’une sécurité supplémentaire, les autres canaux permettent à votre application de transmettre des en-têtes de chiffrement pour protéger un message. 

### <a name="limitations-of-alternate-channels"></a>Limitations des autres canaux
-   Le serveur de votre application ne peut pas envoyer de notifications push Toast, de vignette ou de type badge. Vous pouvez envoyer des notifications push brutes uniquement. Votre application est toujours en mesure d’envoyer des notifications locales à partir de votre tâche en arrière-plan. 
-   Requiert une API REST différente de celle des canaux de vignette primaire ou secondaire. L’utilisation de l’API REST W3C standard signifie que votre application doit avoir une logique différente pour l’envoi des mises à jour de la grille ou du Toast Push

### <a name="creating-an-alternate-channel"></a>Création d’un canal de remplacement 

```csharp
PushNotificationChannel webChannel = 
    await PushNotificationChannelManager.GetDefault().CreateRawPushNotificationChannelWithAlternateKeyForApplicationAsync(applicationServerKey, appChannelId);
```

## <a name="channel-type-comparison"></a>Comparaison du type de canal
Voici une comparaison rapide entre les différents types de canaux :

<table>

<tr class="header">
<th align="left"><b>Type</b></th>
<th align="left"><b>Envoyer un toast ?</b></th>
<th align="left"><b>Vignette/badge Push ?</b></th>
<th align="left"><b>Envoyer des notifications push brutes ?</b></th>
<th align="left"><b>Authentification</b></th>
<th align="left"><b>API</b></th>
<th align="left"><b>Enregistrer l’inscription nécessaire ?</b></th>
<th align="left"><b>Canaux</b></th>
<th align="left"><b>Chiffrement</b></th>
</tr>


<tr class="odd">
<td align="left">Principal</td>
<td align="left">Oui</td>
<td align="left">Oui-vignette principale uniquement</td>
<td align="left">Oui</td>
<td align="left">OAuth</td>
<td align="left">API REST WNS</td>
<td align="left">Oui</td>
<td align="left">Une par application</td>
<td align="left">En transit</td>
</tr>
<tr class="even">
<td align="left">Vignette secondaire</td>
<td align="left">Non </td>
<td align="left">Oui-vignette secondaire uniquement</td>
<td align="left">Non </td>
<td align="left">OAuth</td>
<td align="left">API REST WNS</td>
<td align="left">Oui</td>
<td align="left">Un par vignette secondaire</td>
<td align="left">En transit</td>
</tr>
<tr class="odd">
<td align="left">Autre</td>
<td align="left">Non </td>
<td align="left">Non</td>
<td align="left">Oui</td>
<td align="left">VAPID</td>
<td align="left">Webpush W3C standard</td>
<td align="left">Non </td>
<td align="left">1 000 par application</td>
<td align="left">En transit + chiffrement de bout en bout possible avec la transmission d’en-tête (requiert du code d’application)</td>
</tr>
</table>

## <a name="choosing-the-right-channel"></a>Choix du canal approprié

En général, nous vous recommandons d’utiliser le canal principal dans votre application, à quelques exceptions près : 

1. Si vous effectuez un push d’une mise à jour de vignette sur une vignette secondaire, utilisez le canal push de vignette secondaire.
2. Si vous transmettez des canaux à d’autres services (par exemple, dans le cas d’un navigateur), utilisez l’autre canal.
3. Si vous créez une application qui n’est pas répertoriée dans le Windows Store (comme une application métier), utilisez un autre canal.
4. Si vous disposez d’un code Push Web existant sur le serveur que vous souhaitez réutiliser ou si vous avez besoin de plusieurs canaux dans votre service principal, utilisez d’autres canaux.

## <a name="related-articles"></a>Articles connexes

* [Envoyer une notification par vignette locale](../tiles-and-notifications/sending-a-local-tile-notification.md)
* [Notifications toast adaptatives et interactives](../tiles-and-notifications/adaptive-interactive-toasts.md)
* [Démarrage rapide : envoi d’une notification Push](https://docs.microsoft.com/previous-versions/windows/apps/hh868252(v=win.10))
* [Comment mettre à jour un badge via des notifications Push](https://docs.microsoft.com/previous-versions/windows/apps/hh465450(v=win.10))
* [Comment demander, créer et enregistrer un canal de notification](https://docs.microsoft.com/previous-versions/windows/apps/hh465412(v=win.10))
* [Comment intercepter les notifications pour les applications en cours d’exécution](https://docs.microsoft.com/previous-versions/windows/apps/hh465450(v=win.10))
* [Comment s’authentifier auprès des services de notifications Push Windows (WNS)](https://docs.microsoft.com/previous-versions/windows/apps/hh465407(v=win.10))
* [En-têtes des demandes et des réponses du service de notifications Push](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10))
* [Recommandations et liste de vérification sur les notifications Push](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview)
* [Notifications brutes](raw-notification-overview.md)
