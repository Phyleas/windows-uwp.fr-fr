---
description: Le rapport d’évaluation de l’espace partenaires vous permet de voir comment les clients ont évalué votre application dans le Store.
title: Rapport sur les évaluations
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, notation, taux, étoile, étoiles, évalué
ms.localizationpriority: medium
ms.openlocfilehash: 4d1d0f9ceaa660245c537c4caf1f1770f1c5be6d
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030342"
---
# <a name="ratings-report"></a>Rapport sur les évaluations


Le rapport d' **évaluation** de l' [espace partenaires](https://partner.microsoft.com/dashboard) vous permet de voir comment les clients ont évalué votre application dans le Store. 

Vous pouvez afficher ces données dans l’espace partenaires ou [Télécharger le rapport](download-analytic-reports.md) en mode hors connexion. Vous pouvez également récupérer ces données par programme à l’aide de la méthode [obtenir des révisions d’application](../monetize/get-app-reviews.md) dans l' [API REST Microsoft Store Analytics](../monetize/access-analytics-data-using-windows-store-services.md).

> [!TIP]
> Pour consulter rapidement les avis, les évaluations et les commentaires des utilisateurs pour l’ensemble de vos applications au cours des 30 derniers jours, développez **engagement** dans le menu de navigation de gauche, puis sélectionnez **critiques et commentaires.** 

## <a name="apply-filters"></a>Appliquer des filtres

Près du haut de la page, vous pouvez sélectionner la période pour laquelle vous souhaitez afficher les révisions. La sélection par défaut est **durée de vie** , mais vous pouvez choisir d’afficher les révisions pour 30 jours, 3 mois, 6 mois ou 12 mois, ou pour une plage de données personnalisée que vous spécifiez. Notez que la **répartition des évaluations** et l' **évaluation moyenne sur** les graphiques de temps affichent toujours les données au cours des 12 derniers mois. ces options de période n’affectent pas ces graphiques.

Vous pouvez développer des **filtres** pour filtrer les révisions affichées sur cette page à l’aide des options suivantes. Ces filtres ne s’appliquent pas à la **répartition des évaluations** et à l' **évaluation moyenne dans** les graphiques de temps.

-   **Marché** : la valeur par défaut de ce filtre est **Tous les marchés** . Vous pouvez choisir un marché spécifique si vous souhaitez ne visualiser que les évaluations des clients sur ce marché.
-   **Type d’appareil** : le filtre par défaut est **Tous les appareils** . Vous pouvez choisir un type d’appareil spécifique si vous souhaitez que cette page affiche uniquement l’évaluation laissée par les clients utilisant celui-ci.
-   **Version du système d’exploitation** : le paramètre par défaut est **All** . Vous pouvez choisir une version de système d’exploitation spécifique si vous souhaitez que cette page affiche uniquement les évaluations laissées par les clients sur cette version du système d’exploitation.
-   **Nom** de la catégorie : le filtre par défaut est **All** . Vous pouvez choisir d’afficher uniquement les évaluations associées aux révisions que nous avons identifiées comme appartenant à une catégorie d’informations de [révision](reviews-report.md#insight-categories) spécifique pour afficher uniquement les révisions que nous avons associées à cette catégorie. 

> [!TIP]
> Si vous ne voyez aucune évaluation sur la page, vérifiez que vos filtres n’ont pas exclu tous vos évaluations. Par exemple, si vous filtrez par un système d’exploitation cible que votre application ne prend pas en charge, vous ne verrez aucune évaluation.


## <a name="rating-breakdown"></a>Répartition des évaluations

Le graphique **répartition des évaluations** affiche : 
- Évaluation moyenne des étoiles d’évaluation pour l’application.
- Nombre total d’évaluations de votre application au cours des 12 derniers mois.
- Nombre total d’évaluations pour chaque évaluation d’étoile.
- Nombre d’évaluations pour chaque type d’évaluation (nouvelle ou révisée) par étoile au cours des 12 derniers mois.
 - Les **nouvelles évaluations** sont des évaluations que les clients ont soumises, mais qui n’ont pas changé.
 - Les **évaluations révisées** sont des évaluations qui ont été modifiées par le client, même en modifiant simplement le texte de la révision.

> [!TIP]
> L’évaluation moyenne qu’un client voit dans le magasin prend en compte le marché du client et le type d’appareil. il peut donc différer de ce que vous voyez dans ce rapport.


## <a name="average-rating"></a>Évaluation moyenne

Le graphique d' **évaluation moyenne** montre comment l’évaluation moyenne de l’application a changé au cours des 12 derniers mois.

Au lieu de calculer la moyenne de toutes les évaluations laissées au cours des 12 derniers mois (comme dans le graphique de **répartition des évaluations** ), le graphique d' **évaluation moyenne** vous montre comment les clients ont évalué l’application une semaine donnée. Cela peut vous aider à identifier des tendances ou à déterminer si des évaluations ont été affectées par des mises à jour ou d’autres facteurs.

## <a name="rating-by-market"></a>Évaluation par marché

Le graphique **évaluation par marché** montre une répartition des évaluations moyennes sur chaque marché sur la période sélectionnée. Par défaut, nous affichons ces données dans un formulaire de **carte** visuelle, mais vous pouvez également activer ou désactiver le contrôle dans l’angle supérieur droit pour l’afficher sous forme de **tableau** .

La vue **table** affiche cinq marchés à la fois, triés par ordre alphabétique ou par évaluation moyenne la plus élevée/la plus basse. Vous pouvez également télécharger les données de ce graphique pour afficher des informations sur tous les marchés ensemble.

Vous pouvez également filtrer ce graphique par **évaluation** . Par défaut, les révisions avec toutes les évaluations en étoile sont vérifiées, mais vous pouvez cocher et décocher des évaluations spécifiques (de 1 à 5 étoiles) si vous souhaitez afficher uniquement les révisions associées à des évaluations d’étoiles particulières.
