---
description: Une fois que vous avez défini votre expérience dans l’espace partenaires et que vous avez codé votre expérience dans votre application, vous êtes prêt à activer votre expérience et à utiliser l’espace partenaires pour passer en revue les résultats de votre expérience.
title: Gérer votre expérience dans l’Espace partenaires
ms.assetid: D48EE0B4-47F2-455C-8FB9-630769AC5ACE
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, kit de développement logiciel (SDK) Microsoft Store services, tests A/B, expériences
ms.localizationpriority: medium
ms.openlocfilehash: dfcd9819940d21dcc81c5ac698b76381adf05af6
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030552"
---
# <a name="manage-your-experiment-in-partner-center"></a>Gérer votre expérience dans l’Espace partenaires

Une fois que vous avez [défini votre expérience dans l’espace partenaires](define-your-experiment-in-the-dev-center-dashboard.md) et que vous avez [codé votre application pour l’expérimentation](code-your-experiment-in-your-app.md), vous êtes prêt à activer votre expérience et à utiliser l’espace partenaires pour passer en revue les résultats de votre expérience. Après avoir obtenu toutes les données dont vous avez besoin, vous pourrez mettre fin à votre expérience et décider si vous souhaitez continuer à utiliser les valeurs des variables dans la variante de contrôle pour toutes vos applications, ou si vous voulez utiliser les valeurs des variables dans l’une de vos autres variantes.

> [!NOTE]
> Lorsque vous activez une expérience, l’espace partenaires commence immédiatement à collecter des données à partir de toutes les applications instrumentées pour enregistrer les données de votre expérience. Toutefois, plusieurs heures peuvent être nécessaires pour que les données d’expérimentation s’affichent dans l’espace partenaires.

Pour découvrir une procédure pas à pas illustrant le processus de création et d’exécution d’une expérience de bout en bout, voir [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

## <a name="activate-your-experiment"></a>Activer votre expérience

Lorsque vous êtes satisfait des paramètres de votre expérience dans l’espace partenaires et que vous avez mis à jour votre code d’application, vous êtes prêt à activer votre expérience afin de pouvoir commencer à collecter des données d’expérimentation à partir de votre application. Lorsque l’expérience est active, votre application peut récupérer des valeurs de variation et des événements d’affichage de rapport et de conversion dans l’espace partenaires.

1. Connectez-vous à l’[Espace partenaires](https://partner.microsoft.com/dashboard).
2. Sous **Vos applications** , sélectionnez l’application présentant l’expérience que vous souhaitez activer.
3. Dans le volet de navigation, sélectionnez **Services** , puis **Expérimentation** .
4. Dans la table des projets de la section **Projets** , développez le projet qui contient votre expérience, puis effectuez l’une des opérations suivantes :
  * Cliquez sur le lien **Activer** lien correspondant à votre expérience. Votre expérience est ajoutée à la section **Active experiments** (Expériences actives) en haut de la page.
  * Cliquez sur le nom de l’expérience, faites défiler la page de l’expérience vers le bas, puis cliquez sur **Activer** .

> [!IMPORTANT]
> Après avoir activé une expérience, vous ne pouvez plus modifier les paramètres d’expérimentation, sauf si vous avez cliqué sur la case à cocher **expérience modifiable** lors de la création de l’expérience. Nous vous recommandons de coder l’expérience dans votre application avant d’activer votre expérience.

## <a name="review-the-results-of-your-experiment"></a>Passer en revue les résultats de votre expérience

1. Dans l’espace partenaires, revenez à la page **expérimentation** de votre application.
2. Dans la section **Active experiments** (Expériences actives), cliquez sur le nom de votre expérience active pour accéder à la page correspondante.
3. Dans le cas d’une expérience active ou terminée, les deux premières sections de cette page fournissent les résultats de votre expérience :
  * La section **Résumé des résultats** répertorie les objectifs de votre expérience et le taux de conversion pour chaque variante.
  * La section **Détails des résultats** fournit des informations supplémentaires sur chacun des objectifs de votre expérience, notamment les vues, les conversions, les utilisateurs uniques, le taux de conversion, le pourcentage d’écart, la confiance et l’importance. La *confiance* est une mesure statistique de la fiabilité d’une estimation, qui calcule la marge d’erreur. L’ *importance* est une mesure statistique, reposant sur la taille de l’échantillon, qui détermine la probabilité qu’un résultat ne soit pas dû au hasard, mais qu’il soit plutôt attribué à une cause spécifique.

> [!NOTE]
> L’espace partenaires signale uniquement le premier événement de conversion pour chaque utilisateur sur une période de 24 heures. Si un utilisateur déclenche plusieurs événements de conversion dans votre application au cours d’une période de 24 heures, seul le premier événement de conversion est signalé. Cette approche est destinée à éviter qu’un utilisateur unique avec de nombreux événements de conversion ne fausse les résultats de l’expérience pour un groupe représentatif d’utilisateurs.


## <a name="complete-your-experiment"></a>Terminer votre expérience

1. Dans l’espace partenaires, revenez à votre page d’expérience. Pour obtenir les instructions correspondantes, voir la section précédente.
2. Dans la section **Résumé des résultats** , effectuez l’une des opérations suivantes :
  * Si vous souhaitez mettre fin à l’expérience et continuer à utiliser les valeurs des variables dans la variante de contrôle de votre application, cliquez sur **Conserver** .
  * Si vous souhaitez mettre fin à l’expérience, mais utiliser les valeurs des variables dans une autre variante de votre application, cliquez sur **Basculer** sous la variante vers laquelle vous voulez basculer.
3. Cliquez sur **OK** pour confirmer que vous souhaitez mettre fin à l’expérience.


## <a name="related-topics"></a>Rubriques connexes

* [Créer un projet et définir des variables distantes dans l’espace partenaires](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Coder votre application à des fins d’expérimentation](code-your-experiment-in-your-app.md)
* [Définir votre expérience dans l’Espace partenaires](define-your-experiment-in-the-dev-center-dashboard.md)
* [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Exécuter des expériences d’application avec des tests A/B](run-app-experiments-with-a-b-testing.md)
