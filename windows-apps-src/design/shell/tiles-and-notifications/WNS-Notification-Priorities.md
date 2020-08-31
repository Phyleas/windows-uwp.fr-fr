---
title: Priorités de notification WNS
description: Description des différentes priorités que vous pouvez définir sur une notification
ms.date: 01/10/2017
ms.topic: article
keywords: Windows 10, UWP, API WinRT, WNS
localizationpriority: medium
ms.openlocfilehash: 3b6642054f9c63a03764267e5886b67fd4a9ac7d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89169213"
---
# <a name="wns-notification-priorities"></a>Priorités de notification WNS
En définissant la priorité d’une notification avec un en-tête simple pour les messages WNS, vous pouvez contrôler la façon dont les notifications sont remises dans des situations sensibles à la batterie.

## <a name="power-on-windows"></a>Mise sous tension de Windows
Étant donné que plus d’utilisateurs travaillent uniquement sur des appareils fonctionnant sur batterie, la réduction de l’utilisation de l’alimentation est devenue une exigence standard pour toutes les applications. Si les applications consomment plus d’énergie que la valeur qu’elles fournissent, les utilisateurs peuvent désinstaller les applications. Alors que le système d’exploitation Windows réduit la consommation d’énergie sur la batterie dans la mesure du possible, il est de la responsabilité de l’application de travailler efficacement. 

Les priorités WNS constituent un moyen de déplacer le travail non critique de la batterie. Les priorités WNS indiquent au système quelles notifications doivent être envoyées instantanément et qui peuvent attendre jusqu’à ce que l’appareil soit branché sur une source d’alimentation. Avec ces indicateurs, le système peut fournir les notifications à l’heure exacte où elles sont les plus précieuses pour l’utilisateur et l’application. 

## <a name="power-modes-on-the-device"></a>Modes d’alimentation de l’appareil
Chaque appareil Windows fonctionne par le biais de divers modes d’alimentation (batterie, économiseur de batterie et charge) et les utilisateurs attendent des comportements différents des applications dans différents modes d’alimentation. Lorsque l’appareil est activé, toutes les notifications doivent être remises. En mode économiseur de batterie, seules les notifications les plus importantes doivent être remises. Lorsque l’appareil est branché, les opérations critiques de synchronisation ou de non-temps peuvent être effectuées.

Windows ne sait pas quelles notifications sont importantes pour un utilisateur ou une application. le système s’appuie donc entièrement sur les applications pour définir la priorité appropriée pour leurs notifications. 

## <a name="priorities"></a>Priorités
Une application peut utiliser quatre priorités pour l’envoi de notifications push. La priorité est définie sur des notifications individuelles, ce qui vous permet de choisir les notifications à remettre instantanément (par exemple, un message de messagerie instantanée) et celles qui peuvent être patientées (par exemple, contacter des mises à jour de photos).

Les priorités sont les suivantes : 

|    Priorité    |    Remplacement de l’utilisateur    |    Description    |    Exemple    |
|----------------|---------------------|-------------------|---------------|
|    Élevé    |    Oui : l’utilisateur peut bloquer toutes les notifications d’une application ou empêcher une application d’être limitée en mode économiseur de batterie.    |    Les notifications les plus importantes doivent être remises en tout cas lorsque l’appareil peut recevoir des notifications. Par exemple, les appels VoIP ou les alertes critiques qui doivent sortir de veille de l’appareil appartiennent à cette catégorie.    |    Appels VoIP, alertes à durée critique    |
|    Moyenne    |    Oui : l’utilisateur peut bloquer toutes les notifications d’une application ou empêcher une application d’être limitée en mode économiseur de batterie.    |    Ce sont des éléments qui ne sont pas aussi importants que les choses qui n’ont pas besoin de se produire immédiatement, mais les utilisateurs sont importuns s’ils ne s’exécutent pas en arrière-plan.    |    Synchronisation du compte de messagerie secondaire, mises à jour de vignettes dynamiques.    |
|    Faible    |    Oui : l’utilisateur peut bloquer toutes les notifications d’une application ou empêcher une application d’être limitée en mode économiseur de batterie.    |    Notifications qui n’ont de sens que lorsque l’utilisateur utilise l’appareil ou lorsque l’activité d’arrière-plan est logique. Celles-ci sont mises en cache et ne sont pas traitées jusqu’à ce que l’utilisateur se connecte ou se connecte à son appareil.    |    Statut du contact (en ligne/hors connexion)    |
|    Très faible     |    Non : il ne peut pas empêcher les notifications de très faible priorité d’être limitées en mode économiseur de batterie.    |    Cela est presque identique à la priorité basse, sauf que les utilisateurs ne peuvent pas remplacer la stratégie d’économiseur de batterie. Ces notifications ne seront jamais fournies dans l’économiseur de batterie.    |    Synchronisation des fichiers pour un service de synchronisation.    |

Notez que de nombreuses applications auront des notifications de priorité différente tout au long de leur cycle de vie. Étant donné que la priorité est définie pour chaque notification, ce n’est pas un problème. Une application VoIP peut envoyer une notification de priorité élevée pour un appel entrant, puis la suivre avec une priorité basse lorsqu’un contact est mis en ligne. 

## <a name="setting-the-priority"></a>Définition de la priorité

La définition de la priorité sur la demande de notification est effectuée via un en-tête supplémentaire sur la demande de publication, `X-WNS-PRIORITY` . Il s’agit d’une valeur entière comprise entre 1 et 4, qui correspond à une priorité : 

| Nom de la priorité | X-WNS-valeur PRIORITy | Valeur par défaut pour : |
|---------------|----------------------|------------------|
| Élevé | 1 | Toasts |
| Moyen | 2 | Vignettes et badges |
| Faible | 3 | Brut |
| Très faible | 4 |  |

Pour une compatibilité descendante, il n’est pas nécessaire de définir une priorité. Si une application ne définit pas la priorité de ses notifications, le système fournira une priorité par défaut. Les valeurs par défaut sont indiquées dans le graphique ci-dessus et correspondent au comportement des versions existantes de Windows. 

## <a name="detailed-listing-of-desktop-behavior"></a>Liste détaillée du comportement du Bureau 

Si vous expédiez votre application sur différentes références SKU de Windows, il est généralement préférable de suivre le graphique de la section ci-dessus. 

Les comportements recommandés plus spécifiques pour chaque priorité sont répertoriés ci-dessous. Cela ne garantit pas que chaque appareil fonctionnera exactement en fonction du graphique. Les fabricants d’ordinateurs OEM sont libres de configurer le comportement différemment, mais la plupart sont proches de ce graphique. 

| État de l’appareil    | PRIORITÉ : haute    |    PRIORITÉ : moyenne        | PRIORITÉ : faible    |    PRIORITÉ : très faible    |
|-------------------------------------------------------|----------------------------------------------------|----------------------------------------------------|----------------------------------------------------|--------------------------|
|    Écran allumé ou branché    |    Remettre    |    Remettre    |    Remettre    |    Remettre    |
|    Éteindre l’écran et la batterie    |    Remettre    |    Si l’utilisateur est exempté : fournir autre chose : batch     |    Si l’utilisateur est exempté : fournir autre chose : cache *    |    d'instance/de clé    |
|    Économiseur de batterie activé    |    Si l’utilisateur est exempté : fournir autre chose : cache    |    Si l’utilisateur est exempté : fournir autre chose : cache    |    Si l’utilisateur est exempté : fournir autre chose : cache    |    d'instance/de clé     |
|    Sur batterie + économiseur de batterie activé + écran désactivé    |    Si l’utilisateur est exempté : fournir autre chose : cache    |    Si l’utilisateur est exempté : fournir autre chose : cache    |    Si l’utilisateur est exempté : fournir autre chose : cache    |    d'instance/de clé    |

Notez que les notifications de faible priorité sont remises par défaut pour l’écran et la batterie uniquement pour les appareils Windows Phone. Cela permet de maintian la compatibilité avec la stratégie MPNS préexistante. Notez également que les quatrième et cinquième lignes sont identiques, en appelant simplement des scénarios différents.

Pour exempter une application dans l’économiseur de batterie, les utilisateurs doivent accéder à l’option « utilisation de la batterie par l’application » dans paramètres, puis sélectionner « autoriser l’application à exécuter des tâches en arrière-plan ». Cette sélection d’utilisateur exempte l’application de l’économiseur de batterie pour les notifications haute, moyenne et basse priorité. Vous pouvez également appeler l' [API BackgroundExecutionManager](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccesskindasync#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessKindAsync_Windows_ApplicationModel_Background_BackgroundAccessRequestKind_System_String_) pour demander l’autorisation de l’utilisateur.  

## <a name="related-topics"></a>Rubriques connexes
- [Vue d’ensemble des services de notifications Push Windows (WNS)](windows-push-notification-services--wns--overview.md)
- [Demande d’autorisation pour s’exécuter en arrière-plan](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccesskindasync#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessKindAsync_Windows_ApplicationModel_Background_BackgroundAccessRequestKind_System_String_)