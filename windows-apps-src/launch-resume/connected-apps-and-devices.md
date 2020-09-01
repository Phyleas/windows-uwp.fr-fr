---
title: Applications et appareils connectés (projet Rome)
description: Cette section explique comment utiliser la plateforme Systèmes distants pour identifier les appareils distants, lancer une application sur un appareil distant et communiquer avec un service d’application sur un appareil distant.
ms.date: 06/08/2018
ms.topic: article
keywords: Windows 10, UWP, appareils connectés, systèmes distants, Rome, projet Rome
ms.assetid: 7f39d080-1fff-478c-8c51-526472c1326a
ms.localizationpriority: medium
ms.openlocfilehash: 08004f22575be69fff3f8d8017ea34327d9a0b7a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156033"
---
# <a name="connected-apps-and-devices-project-rome"></a>Applications et appareils connectés (projet Rome)

Cette section explique comment connecter des applications sur des appareils et des plateformes à l’aide du [projet Rome](https://developer.microsoft.com/windows/project-rome). Pour savoir comment implémenter le projet Rome dans un scénario multiplateforme, visitez la [page principale docs pour le projet Rome](/windows/project-rome/).

La plupart des utilisateurs disposent de plusieurs appareils et commencent souvent une activité sur un appareil et le terminent sur un autre. Pour répondre à cette situation, les applications doivent être accessibles sur plusieurs appareils et plateformes. Le projet Rome vous permet de découvrir des appareils distants, de lancer une application sur un appareil distant et de communiquer avec un service d’application sur un appareil distant.

Les [API de systèmes distants](/uwp/api/Windows.System.RemoteSystems) introduites dans Windows 10, version 1607 vous permettent d’écrire des applications qui permettent aux utilisateurs de démarrer une tâche sur un appareil et de le terminer sur un autre. La tâche reste le point central, et les utilisateurs peuvent faire leur travail sur l’appareil le plus pratique pour eux. Par exemple, un utilisateur peut écouter la radio sur son téléphone dans la voiture, mais lorsqu’il s’y connecte, il peut vouloir transférer la lecture sur son réseau Xbox qui est raccordé au système stéréo d’habitation.

Vous pouvez également utiliser le projet Rome pour les appareils compagnons ou les scénarios de contrôle à distance. Utilisez les API de messagerie App service pour créer un canal d’application entre deux appareils pour envoyer et recevoir des messages personnalisés. Par exemple, vous pouvez écrire une application pour votre téléphone qui contrôle la lecture sur votre téléviseur, ou une application auxiliaire qui fournit des informations sur les caractères d’une émission télévisée que vous regardez dans une autre application.  

Les appareils peuvent être connectés de façon proximale par le biais de Bluetooth et sans fil, ou à distance via le Cloud. ils sont liés par le compte Microsoft (MSA) de la personne qui les utilise.

Consultez l' [exemple UWP](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems ) sur les systèmes distants pour obtenir des exemples montrant comment découvrir le système distant, lancer une application sur un système distant et utiliser app services pour envoyer des messages entre des applications exécutées sur deux systèmes.

Pour plus d’informations sur le projet Rome en général, y compris les ressources pour l’intégration multiplateforme, accédez à [aka.MS/Project-Rome](https://developer.microsoft.com/windows/project-rome).

| Rubrique | Description |
|-------|-------------|
| [Lancer une application sur un appareil distant](launch-a-remote-app.md) | Découvrez comment lancer une application sur un appareil distant. Cette rubrique couvre le cas d’utilisation le plus simple et l’installation préliminaire.  |
| [Détecter des appareils distants](discover-remote-devices.md)  | Découvrez comment détecter les appareils auxquels vous pouvez vous connecter. |
| [Communiquer avec un service d’application distant](communicate-with-a-remote-app-service.md) | Découvrez comment interagir avec une application sur un appareil distant. |
| [Connecter des appareils par le biais de sessions à distance](remote-sessions.md) | Créez des expériences partagées sur plusieurs appareils en les rejoignant dans une session à distance. |
| [Poursuivre l’activité utilisateur, même sur différents appareils](useractivities.md)| Aidez les utilisateurs à reprendre ce qu’ils faisaient dans votre application, même sur plusieurs appareils.|
| [Bonnes pratiques concernant les activités utilisateur](useractivities-best-practices.md)| Découvrez les pratiques recommandées pour la création et la mise à jour des activités des utilisateurs.|