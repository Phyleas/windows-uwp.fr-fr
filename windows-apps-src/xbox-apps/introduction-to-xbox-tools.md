---
title: Présentation des outils Xbox One
description: Découvrez comment accéder au portail Xbox One Device à l’aide de l’application dev domotique dans le kit de développement Xbox One.
ms.date: 10/04/2017
ms.topic: article
keywords: Windows 10, UWP, Xbox One, outils
ms.assetid: 6eaf376f-0d7c-49de-ad78-38e689b43658
ms.localizationpriority: medium
ms.openlocfilehash: ed9df02ba929d170eca5b37e4376220e93e4902f
ms.sourcegitcommit: e273e5901bfa6596dfef4cc741bb1c42614c25ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89238314"
---
# <a name="introduction-to-xbox-one-tools"></a>Présentation des outils Xbox One

Cette section explique comment accéder au portail de l’appareil Xbox via l’application de démarrage.

## <a name="dev-home"></a>Accueil du développeur

Accueil du développeur permet d’expérimenter des outils dans le Kit de développement Xbox One qui sont destinés à améliorer la productivité des développeurs. L’outil Accueil du développeur offre des fonctionnalités vous permettant de gérer et de configurer votre kit de développement.

Dev orig est l’application par défaut qui s’ouvre lorsque votre console en mode développeur démarre. Vous pouvez également ouvrir la page d’accueil dev en sélectionnant la vignette **Accueil dev** sur l’écran d’accueil. Si cette vignette est absente, la console n’est pas en mode développeur.

Pour plus d’informations sur dev orig, consultez page d’hébergement [des développeurs sur la console (dev orig)](dev-home.md).

## <a name="xbox-device-portal"></a>Portail de périphérique Xbox
Le portail d’appareils Xbox est un outil de gestion des appareils basé sur un navigateur qui vous permet d’ajouter des jeux et des applications, d’ajouter des comptes de test Xbox Live, de modifier des bacs à sable et bien plus encore.

Pour activer le portail d’appareil Xbox sur votre console Xbox One :

1. Sélectionnez la vignette **dev Accueil** sur l’écran d’accueil.

  ![Sélection de la vignette Accueil du développeur](images/introduction-to-xbox-one-tools-1.png)

2. Dans la page d’hébergement dev, accédez à l’onglet **démarrage** , puis dans la section **accès à distance** , sélectionnez **paramètres d’accès à distance**.

  ![Outil Gestion à distance](images/introduction-to-xbox-one-tools-2.png)

3. Cochez la case **activer le portail du périphérique Xbox** .

4. Sous **authentification**, activez la case à cocher **exiger l’authentification pour accéder à cette console à partir des outils Web ou PC** .

5. Entrez un **nom d’utilisateur** et un __mot de passe__, puis sélectionnez **Enregistrer**. Ces informations d’identification sont utilisées pour authentifier l’accès à votre kit de développement à partir d’un navigateur.

6. Sélectionnez **Fermer**, puis, sous l’onglet dossier de **démarrage** , notez l’URL indiquée dans l’outil **accès à distance** .

7. Entrez l’URL dans votre navigateur. Vous recevrez un avertissement sur le certificat qui a été fourni, comme dans la capture d’écran suivante, car le certificat de sécurité signé par votre Xbox One console n’est pas considéré comme un serveur de publication approuvé et connu. Sur Edge, cliquez sur **Détails** , puis **accédez à la page Web** pour accéder au portail de l’appareil Xbox.

    ![Avertissement concernant le certificat de sécurité](images/introduction-to-xbox-one-tools-3.png)

8. Connectez-vous avec les informations d’identification que vous avez configurées.

## <a name="xbox-dev-mode-companion"></a>Compagnon du mode de développement Xbox
Le Compagnon du mode de développement Xbox est un outil qui vous permet de travailler sur votre console sans quitter votre PC. L’application vous permet d’afficher l’écran de la console et d’y envoyer des données. Pour plus d’informations, voir [Compagnon du mode de développement Xbox](xbox-dev-mode-companion.md).

## <a name="see-also"></a>Voir aussi
- [Utilisation de Fiddler avec Xbox One lors du développement pour UWP](uwp-fiddler.md)
- [Vue d’ensemble du Portail d'appareil Windows](../debug-test-perf/device-portal.md)
- [UWP sur Xbox One](index.md)