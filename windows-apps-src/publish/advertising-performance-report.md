---
Description: Pour afficher les données de performances des unités ad dans vos applications, utilisez le rapport des performances de publication dans l’espace partenaires.
title: Rapport sur les performances publicitaires
ms.assetid: 32E555C3-C34D-4503-82BB-4C3F5CAE4500
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bb9c6a43b9411b2297cc4cbf35dfdb2177e5dab7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155373"
---
# <a name="advertising-performance-report"></a>Rapport sur les performances publicitaires


Le **rapport** sur les performances de la publication dans l' [espace partenaires](https://partner.microsoft.com/dashboard) montre comment vos [unités publicitaires](in-app-ads.md) fonctionnent, y compris les publicités de la communauté. Ce rapport comprend les données de plusieurs fournisseurs ad dans les applications UWP qui utilisent la [médiation ad](in-app-ads.md#mediation).

Pour afficher ce rapport, développez **analyser** dans le menu de navigation de gauche, puis sélectionnez **performances Active Directory**. Vous pouvez afficher ces données dans l’espace partenaires ou télécharger les données de rapport pour les afficher hors connexion en cliquant sur les icônes de flèche dans la page. Vous pouvez également récupérer ces données par programme à l’aide de la méthode d' [extraction des données de performances Active Directory](../monetize/get-ad-performance-data.md) dans notre [API REST Analytics](../monetize/access-analytics-data-using-windows-store-services.md).

Lorsque vous affichez les rapports de performances publicitaires, sachez que les données de rapport pour les trois derniers jours peuvent changer à mesure que nous recevons et traiterons de nouvelles données à partir de différentes sources. En outre, les reportions de données peuvent se produire jusqu’à 90 jours dans le passé.

## <a name="apply-filters"></a>Appliquer des filtres

Près du haut de la page, vous pouvez sélectionner la période pour laquelle vous souhaitez afficher les données. La sélection par défaut est 30D (30 jours), mais vous pouvez choisir d’afficher les données pour 3, 6 ou 12 mois, ou pour une plage de données personnalisée que vous spécifiez.

Vous pouvez également développer des **filtres** pour filtrer toutes les données de cette page par unité publicitaire, application, fournisseur ad et type d’appareil. Vous pouvez choisir parmi les options suivantes :

* **Agrégation**: choisissez la façon dont les données du rapport sont agrégées et comment elles peuvent être filtrées plus en détail. Par défaut, ce filtre est défini sur **toutes les unités ad**. Vous pouvez éventuellement modifier ce filtre pour **toutes les applications** ou **tous les fournisseurs ad**, ou vous pouvez choisir d’effectuer une agrégation par une application spécifique dans laquelle vous utilisez des publicités.
* **Fournisseurs d’annuaires Active Directory**: filtrez le rapport sur les données de performances de certains [fournisseurs ad](in-app-ads.md#paid-networks). Par défaut, le rapport affiche les données de tous les fournisseurs ad. Cette option est désactivée si vous avez choisi **tous les fournisseurs ad** dans la liste déroulante **agrégation** .
* **Appareil**: filtrez le rapport sur les données de performances de certains types d’appareils. Par défaut, le rapport affiche les données de tous les types d’appareils.

## <a name="overall-performance"></a>Performances globales

Cette section affiche les métriques de performances Active Directory dans un graphique ou une vue cartographique du monde pour les unités ad, les applications et les fournisseurs Active Directory que vous avez sélectionnés dans les filtres de rapport.

Pour basculer vers une vue différente des données, cliquez sur **graphique** ou **mapper** en haut de la section **performances globales** . Dans la vue carte, les nuances plus foncées représentent des valeurs plus élevées et les nuances plus légères représentent des valeurs inférieures. Vous pouvez pointer sur un pays ou une région sur la carte pour analyser la valeur de la métrique sélectionnée. Vous pouvez également effectuer un zoom avant sur une zone de la carte pour afficher les données des plus petits pays.

Vous pouvez affiner les données affichées dans le graphique ou la carte en cliquant sur l’icône de filtre en regard de la liste déroulante du **graphique** ou de la **carte** . Ce filtre vous permet de choisir parmi jusqu’à six unités ad, applications ou fournisseurs ad différentes à comparer dans le graphique ou la vue cartographique. Le type de données que vous pouvez choisir dans ce filtre dépend de ce que vous avez sélectionné pour le filtre de niveau supérieur de l' **agrégation** en haut du rapport.


## <a name="overall-performance-breakdown"></a>Répartition globale des performances

Cette section affiche une table qui contient toutes les métriques de performances Active Directory pour le jeu de données spécifié par les filtres que vous avez sélectionnés dans le rapport.

## <a name="performance-metrics"></a>Mesures de performances

Le rapport de **performances de publicité** comprend des données pour les mesures de performances suivantes. Certaines mesures sont affichées uniquement pour certains fournisseurs d’annuaires Active Directory.

|  Métrique  |  Description  |
|----------|---------------|
| Revenu estimé  |  Estimation de la quantité d’argent reçue des publicités exécutées sur votre application. |
| eCPM  |  Coût effectif par mille impressions. |
| Demandes  | Nombre de fois où une demande ad a été envoyée à partir de votre application.  |
| Impressions  | Nombre de fois où une publicité a été affichée dans votre application.  |
| Taux de remplissage  | Pourcentage de demandes ad envoyées par votre application dans laquelle une publicité a été affichée.  |
| Clicks  |  Nombre de fois où un utilisateur a cliqué sur une publicité dans votre application. |
| CTR  |  Taux de clics, qui correspond au nombre de clics effectués sur une publicité, divisé par le nombre d’impressions. |
| Affichage | Pourcentage d’impressions publicitaires visibles dans votre application. Pour plus d’informations sur la façon dont cette valeur est calculée, consultez [optimiser la vue de vos unités ad](../monetize/optimize-ad-unit-viewability.md). |
| Crédits acquis  | Si vous exécutez une campagne de [publicité communautaire](./about-community-ads.md) , cela indique le nombre de crédits que vous avez gagnés pour l’espace publicitaire promotionnel en présentant les publicités de la Communauté dans votre application.  |
| Crédits dépensé  | Si vous exécutez une campagne de [publicité communautaire](./about-community-ads.md) , cela indique le nombre de crédits que vous avez consacrés aux publicités pour votre application.  |

## <a name="related-topics"></a>Rubriques connexes

* [Publicités dans l’application](in-app-ads.md)
* [Afficher des publicités dans votre application avec le SDK Microsoft Advertising](../monetize/display-ads-in-your-app.md)
* [Optimiser la visibilité de vos unités publicitaires](../monetize/optimize-ad-unit-viewability.md)


 