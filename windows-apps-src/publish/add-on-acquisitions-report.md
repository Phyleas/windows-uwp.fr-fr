---
description: Le rapport sur les acquisitions du module complémentaire de l’espace partenaires vous permet de connaître le nombre de modules complémentaires que vous avez vendus, ainsi que les détails démographiques et la plateforme.
title: Rapport sur les acquisitions des extensions
ms.assetid: F2DF9188-0A98-4AC3-81C0-3E2C37B15582
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, ventes complémentaires, acquisitions de module complémentaire, ventes de IAP, produits dans l’application, IAPS, modules complémentaires
ms.localizationpriority: medium
ms.openlocfilehash: 042922db077edb83b365a70e4451abe130614afe
ms.sourcegitcommit: afc4ff2c89f148d32073ab1cc42063ccdc573a8c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104500"
---
# <a name="add-on-acquisitions-report"></a>Rapport sur les acquisitions des extensions


Le rapport **sur les acquisitions du module complémentaire** dans l' [espace partenaires](https://partner.microsoft.com/dashboard) vous permet de voir le nombre de modules complémentaires que vous avez vendus, ainsi que les détails démographiques et la plateforme, et d’afficher les informations de conversion pour les clients sur Windows 10 (y compris la Xbox). Vous pouvez également afficher des données d’acquisition en temps quasi réel pour la dernière heure ou une période de 72 heures.

Vous pouvez afficher ces données dans l’espace partenaires ou [Télécharger le rapport](download-analytic-reports.md) en mode hors connexion. Vous pouvez également récupérer ces données par programme à l’aide de la méthode [obtenir des acquisitions de module complémentaire](../monetize/get-in-app-acquisitions.md) dans l' [API REST Microsoft Store Analytics](../monetize/access-analytics-data-using-windows-store-services.md).

Dans ce rapport, une acquisition de module complémentaire signifie qu’un client a acheté un module complémentaire à partir de vous (ou l’a acquis sans payer, si vous l’avez offert gratuitement). Plusieurs achats d’un même module complémentaire de type consommable effectués par le même client sont comptabilisés comme différentes acquisitions de modules complémentaires.

> [!IMPORTANT]
> Le rapport sur les **acquisitions du module complémentaire** n’inclut pas les données relatives aux remboursements, aux contrepassations, aux refacturations, etc. Pour estimer le déroulement de votre application, consultez [Résumé du paiement](/partner-center/payout-statement). Dans la section **Réservé**, cliquez sur le lien **Télécharger les transactions réservées**.


## <a name="apply-filters"></a>Appliquer des filtres

Près du haut de la page, vous pouvez sélectionner la période pour laquelle vous souhaitez afficher les données. La sélection par défaut est **30D** (30 jours), mais vous pouvez choisir d’afficher les données pour 3, 6 ou 12 mois, ou pour une plage de données personnalisée que vous spécifiez. Vous pouvez également sélectionner **1H** ou **72H** pour afficher les données d’acquisition en temps quasi-réel pendant une heure ou 72 heures ; ces périodes s’appliquent uniquement à l’onglet **quotidien du module complémentaire** du graphique des **acquisitions des modules complémentaires** et à l’onglet **acquisitions** du graphique **marchés** . 

Vous pouvez également développer des **filtres** pour filtrer toutes les données de cette page en fonction d’un ou de plusieurs compléments, par marché et/ou par type de périphérique.

-   **Module complémentaire**: le filtre par défaut est **tous les modules** complémentaires, mais vous pouvez limiter les données à un ou plusieurs des modules complémentaires de l’application.
-   **Marché**: le filtre par défaut est **tous les marchés**, mais vous pouvez limiter les données aux acquisitions sur un ou plusieurs marchés.
-   **Type d'appareil** : le paramètre par défaut est **Tous les appareils**. Si vous souhaitez afficher des données pour les acquisitions à partir d’un certain type d’appareil uniquement (comme un PC, une console ou une tablette), vous pouvez en choisir un ici spécifique.

Les informations de tous les graphiques listés ci-dessous reflètent la plage de dates et les filtres que vous avez sélectionnés. Certaines sections vous permettent également d’appliquer des filtres supplémentaires.


## <a name="add-on-acquisitions"></a>Acquisitions de modules complémentaires

Le graphique **Acquisitions de modules complémentaires** affiche le nombre total d’acquisitions quotidiennes ou hebdomadaires de vos modules complémentaires au cours de la période sélectionnée. (Lorsque vous utilisez **appliquer des filtres** pour afficher les données pour une durée plus longue, les données d’acquisition sont regroupées par semaine.)

Vous pouvez également voir le nombre d’acquisitions de la durée de vie de votre application en sélectionnant **cumul de module complémentaire**. Vous avez alors accès au total cumulé de l'ensemble des acquisitions effectuées depuis la première publication de votre application.

Vous pouvez éventuellement filtrer les résultats en fonction du fait que l’acquisition du module complémentaire provient du client ou du magasin Web et/ou de la version du système d’exploitation.


## <a name="customer-demographic"></a>Données démographiques sur les clients

Le graphique **démographique des clients** affiche des informations démographiques sur les personnes qui ont acquis vos modules complémentaires. Vous pouvez voir le nombre d’acquisitions (au cours de la période sélectionnée) effectuées par des personnes appartenant à un certain groupe d’âge et par sexe.

> [!NOTE]
> Certains clients ont choisi de ne pas partager ces informations. Si nous n’avons pas pu déterminer le groupe d’âge ni le sexe, l’acquisition entre dans la catégorie **Inconnu**.


## <a name="markets"></a>Marchés

Le graphique **marchés** affiche le nombre total d’acquisitions de module complémentaire sur la période sélectionnée pour chaque marché dans lequel votre application est disponible. 

Vous pouvez afficher ces données sous forme de **carte** visuelle ou basculer le paramètre pour l’afficher sous forme de **tableau** . Le formulaire de table présentera cinq marchés à la fois, triés par ordre alphabétique ou par le nombre d’acquisitions ou d’installations le plus élevé. Vous pouvez également télécharger les données pour afficher des informations sur tous les marchés ensemble.


## <a name="add-on-page-views-and-conversions-by-campaign-id"></a>Affichages et conversions de la page du module complémentaire par ID de campagne

Le tableau des **affichages et des conversions des pages du module complémentaire par campagne ID** affiche le nombre total de conversions de module complémentaire (acquisitions) par ID de campagne sur la période sélectionnée, ce qui vous permet de suivre les conversions et les consultations de page des clients sur Windows 10 (y compris la Xbox) pour chacune de vos [campagnes de promotion personnalisées](create-a-custom-app-promotion-campaign.md). Seules les conversions de module complémentaire sont affichées dans ce graphique.

> [!NOTE]
> Les clients peuvent accéder à la liste de votre application en cliquant sur une campagne personnalisée que vous n’avez pas créée. Nous avons marqué chaque vue de page au sein d’une session avec l’ID de campagne à partir duquel le client a accédé pour la première fois au magasin. Nous attribuons ensuite des conversions à cet ID de campagne pour toutes les acquisitions effectuées en 24 heures. Pour cette raison, vous pouvez voir un plus grand nombre de conversions totales que les conversions totales pour vos ID de campagne, et vous pouvez avoir des conversions ou des conversions de module complémentaire qui n’ont pas de vue de page nulle. 


## <a name="conversions-breakdown-by-campaign-id"></a>Répartition des conversions par ID de campagne

Le tableau **répartition des conversions par ID de campagne** vous permet de suivre les conversions et les affichages de pages des clients sur Windows 10 pour chacune de vos campagnes de [promotion personnalisées](create-a-custom-app-promotion-campaign.md). Les conversions d’application et de module complémentaire sont affichées par ID de campagne.

Dans ce graphique, un *affichage de page* signifie qu’un client a affiché la liste de la boutique de l’application. Une *conversion* signifie qu’un client a récemment obtenu une licence pour l’application ou le module complémentaire (si vous avez facturé de l’argent ou si vous en avez proposé gratuitement).

Gardez à l’esprit que ces affichages de pages et les numéros de conversion ne sont pas des nombres de clients uniques. 


## <a name="top-add-ons"></a>Top des modules complémentaires

Le graphique **en haut des modules complémentaires** affiche le nombre total d’acquisitions pour chacun de vos modules complémentaires sur la période sélectionnée. vous pouvez ainsi voir quels sont les modules complémentaires les plus populaires. 



 

 