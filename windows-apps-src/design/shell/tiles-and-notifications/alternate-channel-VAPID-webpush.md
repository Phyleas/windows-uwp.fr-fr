---
title: Autres canaux Push utilisant VAPID dans UWP
description: Instructions pour l’utilisation de canaux de transmission alternatifs avec le protocole VAPID à partir d’une application Windows
ms.date: 01/10/2017
ms.topic: article
keywords: Windows 10, UWP, API WinRT, WNS
localizationpriority: medium
ms.openlocfilehash: 4bca7e4159c0a4950c95d5d5ef2f34362175a8a7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173303"
---
# <a name="alternate-push-channels-using-vapid-in-windows"></a>Autres canaux Push utilisant VAPID dans Windows 
À partir de la mise à jour des créateurs de automne, les applications Windows peuvent utiliser l’authentification VAPID pour envoyer des notifications push.  

> [!NOTE]
> Ces API sont destinées aux navigateurs Web qui hébergent d’autres sites Web et créent des canaux en leur nom.  Si vous envisagez d’ajouter des notifications webpush à votre application Web, nous vous recommandons de suivre les normes W3C et WhatWG pour créer un service Worker et envoyer une notification.

## <a name="introduction"></a>Introduction
L’introduction de la norme push sur le Web permet aux sites Web d’agir plus comme les applications, en envoyant des notifications même si les utilisateurs ne se trouvent pas sur le site Web.

Le protocole d’authentification VAPID a été créé pour permettre aux sites Web de s’authentifier avec des serveurs émetteurs de manière indépendante du fournisseur. Avec tous les fournisseurs utilisant le protocole VAPID, les sites Web peuvent envoyer des notifications push sans connaître le navigateur sur lequel elles s’exécutent. Il s’agit d’une amélioration significative par rapport à l’implémentation d’un protocole Push différent pour chaque plateforme. 

Les applications Windows peuvent également utiliser VAPID pour envoyer des notifications Push avec ces avantages. Ces protocoles peuvent réduire le temps de développement pour les nouvelles applications et simplifier la prise en charge multiplateforme pour les applications existantes. En outre, les applications d’entreprise ou faisant peuvent désormais envoyer des notifications sans inscription dans le Microsoft Store. J’espère que cela ouvre de nouvelles façons de s’adresser aux utilisateurs sur toutes les plateformes.  

## <a name="alternate-channels"></a>Autres canaux 
Dans UWP, ces canaux VAPID sont appelés canaux de remplacement et offrent des fonctionnalités similaires à un canal Push Web. Ils peuvent déclencher l’exécution d’une tâche en arrière-plan d’application, activer le chiffrement des messages et autoriser plusieurs canaux à partir d’une seule application. Pour plus d’informations sur la différence entre les différents types de canaux, consultez [choix du canal approprié](channel-types.md).

L’utilisation de canaux alternatifs est un excellent moyen d’accéder aux notifications push si votre application ne peut pas utiliser un canal principal ou si vous souhaitez partager du code entre votre site Web et votre application. La configuration d’un canal est facile et familière à toute personne qui a utilisé la norme Push Web ou qui fonctionnait auparavant avec les notifications push Windows.

## <a name="code-example"></a>Exemple de code

Le processus de base de la configuration d’un canal alternatif pour une application Windows est semblable à la configuration d’un canal principal ou secondaire. Tout d’abord, inscrivez-vous pour un canal avec le [serveur WNS](windows-push-notification-services--wns--overview.md). Inscrivez-vous ensuite pour exécuter en tant que tâche en arrière-plan. Une fois la notification envoyée et la tâche en arrière-plan déclenchée, gérez l’événement.  

### <a name="get-a-channel"></a>Obtenir un canal 
Pour créer un autre canal, l’application doit fournir deux informations : la clé publique de son serveur et le nom du canal qu’elle crée. Les détails sur les clés de serveur sont disponibles dans la spécification Push Web, mais nous vous recommandons d’utiliser une bibliothèque Push Web standard sur le serveur pour générer les clés.  

L’ID de canal est particulièrement important, car une application peut créer plusieurs autres canaux. Chaque canal doit être identifié par un ID unique qui sera inclus avec les charges utiles envoyées le long de ce canal.  

```csharp
private async void AppCreateVAPIDChannelAsync(string appChannelId, IBuffer applicationServerKey) 
{ 
    // From the spec: applicationServer Key (p256dh):  
    //               An Elliptic curve Diffie–Hellman public key on the P-256 curve 
    //               (that is, the NIST secp256r1 elliptic curve).   
    //               The resulting key is an uncompressed point in ANSI X9.62 format             
    // ChannelId is an app provided value for it to identify the channel later.  
    // case of this app it is from the set { "Football", "News", "Baseball" } 
    PushNotificationChannel webChannel = await PushNotificationChannelManager.GetDefault().CreateRawPushNotificationChannelWithAlternateKeyForApplicationAsync(applicationServerKey, appChannelId); 
 
    //Save the channel  
    AppUpdateChannelMapping(appChannelId, webChannel); 
             
    //Tell our web service that we have a new channel to push notifications to 
    AppPassChannelToSite(webChannel.Uri); 
} 
```
L’application envoie le canal à son serveur et l’enregistre localement. L’enregistrement de l’ID de canal localement permet à l’application de faire la différence entre les canaux et de renouveler les canaux afin d’éviter qu’ils n’expirent.

Comme tous les autres types de canaux de notification push, les canaux Web peuvent expirer. Pour empêcher l’expiration des canaux sans que votre application ne sache, créez un nouveau canal chaque fois que votre application est lancée.    

### <a name="register-for-a-background-task"></a>S’inscrire pour une tâche en arrière-plan 

Une fois que votre application a créé un autre canal, elle doit s’inscrire pour recevoir les notifications au premier plan ou en arrière-plan. L’exemple ci-dessous s’inscrit pour utiliser le modèle à processus unique pour recevoir les notifications en arrière-plan.  

```csharp
var builder = new BackgroundTaskBuilder(); 
builder.Name = "Push Trigger"; 
builder.SetTrigger(new PushNotificationTrigger()); 
BackgroundTaskRegistration task = builder.Register(); 
```
### <a name="receive-the-notifications"></a>Recevoir les notifications 

Une fois que l’application s’est inscrite pour recevoir les notifications, elle doit être en mesure de traiter les notifications entrantes. Dans la mesure où une seule application peut inscrire plusieurs canaux, veillez à vérifier l’ID de canal avant de traiter la notification.  

```csharp
protected override void OnBackgroundActivated(BackgroundActivatedEventArgs args) 
{ 
    base.OnBackgroundActivated(args); 
    var raw = args.TaskInstance.TriggerDetails as RawNotification; 
    switch (raw.ChannelId) 
    { 
        case "Football": 
            AppPostFootballScore(raw.Content); 
            break; 
        case "News": 
            AppProcessNewsItem(raw.Content); 
            break; 
        case "Baseball": 
            AppProcessBaseball(raw.Content); 
            break; 
 
        default: 
            //We don't have the channelID registered, should only happen in the case of a 
            //caching issue on the application server 
            break; 
    }                           
} 
```

Notez que si la notification provient d’un canal principal, l’ID de canal n’est pas défini.  

## <a name="note-on-encryption"></a>Remarque sur le chiffrement 

Vous pouvez utiliser n’importe quel schéma de chiffrement plus utile pour votre application. Dans certains cas, il suffit de s’appuyer sur la norme TLS entre le serveur et n’importe quel appareil Windows. Dans d’autres cas, il peut être plus judicieux d’utiliser le schéma de chiffrement Push Web, ou un autre schéma de votre conception.  

Si vous souhaitez utiliser une autre forme de chiffrement, la clé est l’utilisation du brut. Propriété des en-têtes. Il contient tous les en-têtes de chiffrement qui ont été inclus dans la demande de publication au serveur Push. À partir de là, votre application peut utiliser les clés pour déchiffrer le message.  

## <a name="related-topics"></a>Rubriques connexes
- [Types de canaux de notification](channel-types.md)
- [Services de notifications Push Windows (WNS)](windows-push-notification-services--wns--overview.md)
- [PushNotificationChannel, classe](/uwp/api/windows.networking.pushnotifications.pushnotificationchannel)
- [PushNotificationChannelManager, classe](/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager)