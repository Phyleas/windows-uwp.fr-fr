---
description: Le rapport acquisitions de l’espace partenaires vous permet de voir qui a acquis et installé votre application, ainsi que les détails démographiques et la plateforme.
title: Rapport sur les acquisitions
ms.assetid: 21126362-F3CD-4006-AD3F-82FC88E3B862
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, acquisitions, ventes d’applications, téléchargements d’applications, installations, entonnoir, acquisition, conversions, canaux, affichages de pages d’application
ms.localizationpriority: medium
ms.openlocfilehash: cabdbd1433da64c927fe5e8326b7906095573497
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033282"
---
# <a name="acquisitions-report"></a>Rapport sur les acquisitions


Le rapport **acquisitions** de l' [espace partenaires](https://partner.microsoft.com/dashboard) vous permet de voir qui a acquis et installé votre application, ainsi que les détails démographiques et la plateforme, et affiche des informations sur la façon dont les clients sur Windows 10 (y compris la Xbox) sont arrivés à la liste de votre application. Vous pouvez également afficher des données d’acquisition en temps quasi réel pour la dernière heure ou une période de 72 heures. 

Vous pouvez afficher ces données dans l’espace partenaires ou [Télécharger le rapport](download-analytic-reports.md) en mode hors connexion. Vous pouvez également récupérer ces données par programme à l’aide de notre [API REST Analytics](../monetize/access-analytics-data-using-windows-store-services.md).

Dans ce rapport, une **acquisition** signifie qu’un nouveau client a obtenu une licence pour votre application (que vous avez facturé ou que vous en ayez proposé gratuitement). Une **installation** fait référence à l’application en cours d’installation sur un appareil Windows 10.

> [!IMPORTANT]
> Le rapport sur les **acquisitions** n’inclut pas les données relatives aux remboursements, aux contrepassations, aux refacturations, etc. Pour estimer le déroulement de votre application, consultez [Résumé du paiement](payout-summary.md). Dans la section **Réservé** , cliquez sur le lien **Télécharger les transactions réservées** .
>
> À l’exception des données d’affichage de page (comme décrit ci-dessous), ce rapport n’inclut pas les données relatives aux clients qui acquièrent une application sans être connectées à un compte Microsoft.


## <a name="apply-filters"></a>Appliquer des filtres

Près du haut de la page, vous pouvez sélectionner la période pour laquelle vous souhaitez afficher les données. La sélection par défaut est **30D** (30 jours), mais vous pouvez choisir d’afficher les données pour 3, 6 ou 12 mois, ou pour une plage de données personnalisée que vous spécifiez. Des données en temps quasi réel s’affichent pour toutes les options (sauf dans les données **cumulatives** de l’application). Les périodes **1H** et **72H** s’appliquent uniquement à l’onglet **quotidien application** du graphique **acquisitions** et à l’onglet **acquisitions** du graphique **marchés** . 

Vous pouvez également développer des **filtres** pour filtrer toutes les données de cette page par marché et/ou par type de périphérique.

-   **Marché** : le filtre par défaut est **tous les marchés** , mais vous pouvez limiter les données aux acquisitions sur un ou plusieurs marchés.
-   **Type d'appareil** : le paramètre par défaut est **Tous les appareils** . Si vous souhaitez afficher des données pour les acquisitions à partir d’un certain type d’appareil uniquement (comme un PC, une console ou une tablette), vous pouvez en choisir un ici spécifique.

Les informations de tous les graphiques listés ci-dessous reflètent la plage de dates et les filtres que vous avez sélectionnés. Certaines sections vous permettent également d’appliquer des filtres supplémentaires.


## <a name="acquisitions"></a>Acquisitions

Le graphique **acquisitions** affiche le nombre d’acquisitions quotidiennes ou hebdomadaires (un nouveau client obtenant une licence pour votre application) sur la période sélectionnée. (Lorsque vous utilisez **appliquer des filtres** pour afficher les données pour une durée plus longue, les données d’acquisition sont regroupées par semaine.) Seules les acquisitions effectuées par les clients qui sont connectés avec un compte Microsoft valide sont incluses dans ce graphique. 

Par défaut, nous affichons la vue quotidienne de l' **application** , qui comprend des données en temps quasi réel. Vous pouvez également voir le nombre d’acquisitions de la durée de vie de votre application en sélectionnant **application cumulative** . Vous avez alors accès au total cumulé de l'ensemble des acquisitions effectuées depuis la première publication de votre application.

Les **ventes brutes** pour votre application (d’octobre 2016) sont également disponibles dans ce graphique, avec la quantité totale obtenue à partir de l’application Sales (en USD). Notez que ce montant ne prend pas en compte les remboursements, les contrepassations, les refacturations, etc.

Vous pouvez éventuellement filtrer les résultats en fonction du fait que l’acquisition provient du client ou du magasin Web et/ou de la version du système d’exploitation.

> [!NOTE]
> Vous pouvez également récupérer ces données par programme à l’aide de la méthode [obtenir des acquisitions d’applications](../monetize/get-app-acquisitions.md) dans notre [API REST Analytics](../monetize/access-analytics-data-using-windows-store-services.md).

Dans la vue quotidienne de l' **application** , lorsque la période **30D** est sélectionnée, vous pouvez voir des marqueurs de cercle. Celles-ci représentent une augmentation ou une diminution significatives d’une valeur donnée que nous pensons avoir à connaître. La date à laquelle le cercle apparaît représente la fin de la semaine au cours de laquelle nous avons détecté une augmentation ou une diminution significative par rapport à la semaine qui précède. Pour obtenir plus de détails sur les modifications, pointez sur le cercle.  

> [!TIP]
> Vous pouvez afficher plus d’informations relatives aux modifications importantes au cours des 30 derniers jours dans le [rapport Insights](insights-report.md).

## <a name="installs"></a>Installe

Le graphique **installations** affiche le nombre de fois que nous avons détecté que les clients ont correctement installé votre application sur les appareils Windows 10 (y compris les consoles Xbox One) pendant la période sélectionnée. Le nombre total est indiqué, avec un graphique présentant les installations par jour ou par semaine (selon la durée que vous avez sélectionnée). Vous pouvez éventuellement filtrer les résultats selon une version de package spécifique.

Le total d’installation comprend les éléments suivants :
-   **S’installe sur plusieurs appareils Windows 10.** Par exemple, si le même client installe votre application sur deux PC Windows 10 et sur une console Xbox One, cela compte comme trois installations.
-   **Réinstalle.** Par exemple, si un client installe votre application aujourd’hui, désinstalle votre application demain, puis réinstalle votre application le mois prochain, qui compte comme deux installations.

Le total d’installation n’inclut pas ou ne reflète pas :
-   **S’installe sur des appareils autres que Windows 10.** Si votre application prend en charge des versions de système d’exploitation antérieures telles que Windows 8. x ou Windows Phone 8. x, nous ne comptons aucune installation sur ces appareils.
-   **Désinstalle.** Quand un client désinstalle votre application de son appareil, nous ne la déduisons pas du nombre total d’installations.
-   **Mises à jour.** Par exemple, si un client installe votre application aujourd’hui, puis installe une mise à jour d’application une semaine plus tard, qui ne compte qu’une seule installation.
-   **Préinstalle.** Si un client achète un appareil sur lequel votre application est préinstallée, nous ne le comptons pas comme une installation.
-   **Installations initiées par le système.** Si Windows installe automatiquement votre application pour une raison quelconque, nous ne le comptons pas comme une installation.

> [!NOTE]
> Vous pouvez également récupérer ces données par programme à l’aide de la méthode d’installation de l' [application obtenir](../monetize/get-app-installs.md) dans notre [API REST Analytics](../monetize/access-analytics-data-using-windows-store-services.md).

## <a name="acquisition-funnel"></a>Entonnoir d’acquisition

L' **entonnoir d’acquisition** vous montre le nombre de clients qui ont effectué chaque étape de l’entonnoir, de l’affichage de la page de magasin à l’utilisation de l’application, ainsi que le taux de conversion. Ces données peuvent vous aider à identifier les domaines où vous souhaiterez peut-être investir davantage pour augmenter vos acquisitions, installations ou utilisations.

> [!IMPORTANT]
> L' **entonnoir d’acquisition** affiche des données uniquement pour les clients sur Windows 10 (y compris la Xbox) au cours des 90 derniers jours.

Les étapes de l’entonnoir sont les suivantes :

- **Pages** consultées : ce nombre représente le nombre total de vues de la liste des boutiques de votre application, y compris les personnes qui ne sont pas connectées avec un compte Microsoft. Cela n’inclut pas les données des clients qui n’ont pas choisi de fournir ces informations à Microsoft.
- **Acquisitions** : nombre de nouveaux clients qui ont obtenu une licence pour votre application (quand ils sont connectés avec leur compte Microsoft) dans un délai de 48 heures après l’affichage de la liste de leur boutique.
- **Installations** : le nombre de clients qui ont installé l’application après l’avoir acquise.
- **Utilisation** : le nombre de clients qui ont utilisé l’application après l’avoir installée.

Vous pouvez éventuellement filtrer les résultats par sexe et/ou par groupe d’âge, ainsi que par ID de campagne personnalisé.

> [!NOTE]
> Vous pouvez également récupérer ces données par programme à l’aide de la méthode [obtenir des données de synthèse](../monetize/get-acquisition-funnel-data.md) de l’acquisition d’applications dans notre [API REST Analytics](../monetize/access-analytics-data-using-windows-store-services.md).

## <a name="markets"></a>Marchés

Le graphique **marchés** indique le nombre total d’acquisitions ou d’installations sur la période sélectionnée pour chaque marché dans lequel votre application est disponible. Vous pouvez choisir d’afficher ou non les données pour les **acquisitions** ou les **installations** .

Vous pouvez afficher ces données sous forme de **carte** visuelle ou basculer le paramètre pour l’afficher sous forme de **tableau** . Le formulaire de table présentera cinq marchés à la fois, triés par ordre alphabétique ou par le nombre d’acquisitions ou d’installations le plus élevé. Vous pouvez également télécharger les données pour afficher des informations sur tous les marchés ensemble.


## <a name="customer-demographic"></a>Données démographiques sur les clients

Le graphique **Données démographiques sur les clients** présente des informations démographiques sur les personnes ayant acheté votre application. Vous pouvez voir le nombre d’acquisitions (au cours de la période sélectionnée) effectuées par des personnes appartenant à un certain groupe d’âge et par sexe.

> [!NOTE]
> Certains clients ont choisi de ne pas partager ces informations. Si nous n’avons pas pu déterminer le groupe d’âge ni le sexe, l’acquisition entre dans la catégorie **Inconnu** .

 

## <a name="app-page-views-and-conversions-by-channel"></a>Vues et conversions de la page de l’application par canal

Le graphique **affichages de pages d’application et conversions par canal** vous permet de voir comment les clients sur Windows 10 sont arrivés dans la liste de votre application sur la période sélectionnée.

Dans ce graphique, un *canal* fait référence à la méthode dans laquelle un client est arrivé sur la page de listage de votre application (par exemple, la consultation et la recherche dans le Windows Store, un lien à partir d’un site Web externe, un lien de l’une de vos campagnes personnalisées, etc.). Les canaux suivants sont inclus :

-   **Trafic du magasin :** le client naviguait ou effectuait une recherche dans le Windows Store lorsqu'ils a consulté la description de votre application.
-   **Campagne personnalisée :** le client suivait un lien qui a utilisé un [ID de campagne personnalisée](create-a-custom-app-promotion-campaign.md).
-   **Autres :** Le client a suivi un lien externe (sans ID de campagne personnalisé) à partir d’un site Web vers le Listing de votre application ou le client a suivi un lien entre un moteur de recherche et la liste de votre application.

Une *vue de page* signifie qu’un client a consulté la description de votre application dans le Windows Store, soit via le Windows Store sur le web, soit depuis l’application du Windows Store sur Windows 10. Cela comprend les affichages des personnes qui ne sont pas connectées avec un compte Microsoft. Certains clients ont refusé de fournir ces informations à Microsoft.

Une *conversion* signifie qu’un client (connecté à l’aide d’un compte Microsoft) a récemment obtenu une licence pour votre application (que vous ayez ou non facturé un argent).

Les numéros de mode page et de conversion ne sont pas des nombres de clients uniques. Pour obtenir des informations sur le taux de conversion, consultez le graphique en [entonnoir d’acquisition](#acquisition-funnel) .

> [!NOTE]
> Les clients peuvent accéder à la liste de votre application en cliquant sur une campagne personnalisée que vous n’avez pas créée. Nous avons marqué chaque vue de page au sein d’une session avec l’ID de campagne à partir duquel le client a accédé pour la première fois au magasin. Nous attribuons ensuite des conversions à cet ID de campagne pour toutes les acquisitions effectuées en 24 heures. Pour cette raison, vous pouvez voir un plus grand nombre de conversions totales que les conversions totales pour vos ID de campagne, et vous pouvez avoir des conversions ou des conversions de module complémentaire qui n’ont pas de vue de page nulle.

> [!NOTE]
> Vous pouvez également récupérer ces données par programme à l’aide de la méthode [obtenir des conversions d’application par canal](../monetize/get-app-conversions-by-channel.md) dans notre [API REST Analytics](../monetize/access-analytics-data-using-windows-store-services.md).

## <a name="app-page-views-and-conversions-by-campaign-id"></a>Vues et conversions de la page de l’application par ID de campagne

Le tableau **affichages de la page d’application et conversions par ID de campagne** vous permet de suivre les conversions et les affichages de page, comme décrit ci-dessus, pour chacune de vos [campagnes de promotion personnalisées](create-a-custom-app-promotion-campaign.md). Les principaux ID de campagne s’affichent, et vous pouvez utiliser les filtres pour exclure ou inclure des ID de campagne spécifiques.

## <a name="total-campaign-conversions"></a>Total des conversions de campagnes

Le tableau des **conversions totales** de la campagne affiche le nombre total de conversions d’application et de module complémentaire à partir de toutes les campagnes personnalisées pendant la période sélectionnée.





 

 
