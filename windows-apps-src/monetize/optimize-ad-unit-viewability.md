---
description: En savoir plus sur les instructions d’optimisation de l’affichage de vos unités ad et sur la mesure de vos mesures d’affichage avec le rapport de performances de publicité.
title: Optimiser la visibilité de vos unités publicitaires
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, ADS, publicité, recommandations, vue d’ensemble
ms.localizationpriority: medium
ms.openlocfilehash: 1653651c41128d359fcacddf0c0d7d408e798d07
ms.sourcegitcommit: 8e0e4cac79554e86dc7f035c4b32cb1f229142b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88942789"
---
# <a name="optimize-the-viewability-of-your-ad-units"></a>Optimiser la visibilité de vos unités publicitaires

>[!WARNING]
> Depuis le 1er juin 2020, la plateforme de monétisation Microsoft AD pour les applications Windows UWP sera arrêtée. [En savoir plus](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Le [rapport de performances de publicité](../publish/advertising-performance-report.md) comprend des métriques d’affichage pour vos unités ad. La vue est une mesure importante, car le secteur de la publicité se déplace vers l’évaluation des expositions affichables plutôt que de fournir des impressions. Les annonceurs ont tendance à enchérir sur des expositions affichables, car ils ont une probabilité accrue que leurs publicités soient visibles par les utilisateurs.  

En alignement avec les recommandations d’affichage de l’IAB, l’impression d’une bannière publicitaire est comptabilisée comme affichable si elle répond aux critères suivants :

* Exigence en pixels : supérieure ou égale à 50% des pixels dans la publication étaient sur l’espace affichable de l’application.
* Durée requise : l’heure à laquelle l’exigence en pixels est remplie est supérieure ou égale à une seconde continue, publication de la publication.

L’impression d’une vidéo publicitaire est comptabilisée comme affichable si elle répond aux critères suivants :

* Exigence en pixels : supérieure ou égale à 50% des pixels dans la publication étaient sur la partie visible de l’application.
* Durée requise : la vidéo répond à l’exigence en pixels et est jouée pendant deux secondes continues après le rendu ad.

La vue est calculée à l’aide de la formule suivante :

**Affichage = [impressions affichées] * 100/[nombre total d’impressions AD]**

## <a name="guidelines-to-improve-ad-unit-viewability"></a>Instructions pour améliorer l’affichage des unités ad

Pour optimiser les impressions visibles pour vos unités ad, suivez ces instructions.

1. **Mesurer les performances** &nbsp; &nbsp; Utilisez le [rapport des performances de publicité](../publish/advertising-performance-report.md) pour passer en revue les métriques d’affichage de chacune de vos unités ad.
2.  **Positionner votre contrôle publicitaire de manière appropriée** &nbsp; &nbsp; En général, la position la plus visible pour une publicité est juste au-dessus du pli (autrement dit, en bas de la partie visible de la page que les utilisateurs peuvent voir sans avoir à faire défiler). Les publicités qui s’affichent en haut de la page ne sont pas visibles. Examinez chaque élément de votre page pour voir comment vous pouvez optimiser l’espace affichable dans votre application. Assurez-vous que vos contrôles ad ne sont pas placés dans l’arrière-plan de l’application.
3.  **Gérer la longueur** &nbsp; &nbsp; de page Un contenu de page plus concis permet une meilleure affichage. Implémentez le défilement infini si les pages de votre application doivent être plus longues.
4.  **Corriger la position** &nbsp; &nbsp; de l’annonce Assurez-vous que vos publicités restent à une position fixe même lorsque l’utilisateur fait défiler. Cela vous permet d’obtenir un temps d’affichage plus élevé et d’augmenter votre taux d’affichage.
5.  **Définir l’opacité** &nbsp; &nbsp; Assurez-vous que l’opacité du conteneur qui contient le contrôle Active Directory est de 100%.
6.  **Implémenter le chargement différé** &nbsp; &nbsp; Ne chargez pas vos publicités tant que les utilisateurs ne font pas défiler la vue qui contient le contrôle Active Directory. Cela permet de s’assurer que la publicité s’affiche plus longtemps pour que l’utilisateur l’affiche.
7.  **Expérimentez différentes tailles** &nbsp; &nbsp; d’annonces Sélectionnez quelques tailles de publicité et mesurez l’affichage de chacune d’elles à l’aide du [rapport de performances de publicité](../publish/advertising-performance-report.md). Choisissez celui qui vous convient le mieux.
8.  Revoir la **conception** &nbsp; &nbsp; de l’application Améliorez la conception de votre page d’application pour réduire le temps de chargement des pages. Les utilisateurs ont tendance à rester plus longtemps sur les pages qui se chargent plus rapidement.
