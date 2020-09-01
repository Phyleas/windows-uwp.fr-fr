---
Description: Le rapport d’utilisation de l’espace partenaires vous permet de voir comment les clients utilisent votre application.
title: Rapport d’utilisation
ms.assetid: 5F0E7F94-D121-4AD3-A6E5-9C0DEC437BD3
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, utilisation, événement personnalisé, rapport, télémétrie, sessions utilisateur
ms.localizationpriority: medium
ms.openlocfilehash: d2839112d36822be5eb8297b838cdc38bab5c71b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167273"
---
# <a name="usage-report"></a>Rapport d’utilisation


Le rapport d' **utilisation** de l' [espace partenaires](https://partner.microsoft.com/dashboard) vous permet de voir comment les clients sur Windows 10 (y compris la Xbox) utilisent votre application et affichent des informations sur les événements personnalisés que vous avez définis. Vous pouvez afficher ces données dans l’espace partenaires ou [Télécharger le rapport](download-analytic-reports.md) en mode hors connexion.


## <a name="apply-filters"></a>Appliquer des filtres

Près du haut de la page, vous pouvez sélectionner la période pour laquelle vous souhaitez afficher les données. La sélection par défaut est **30D** (30 jours), mais vous pouvez choisir d’afficher les données pour 3, 6 ou 12 mois, ou pour une plage de données personnalisée que vous spécifiez.

Vous pouvez également développer des **filtres** pour filtrer les données sur cette page par version de package, marché et/ou type de périphérique.

-   **Version du package** : la valeur par défaut de ce filtre est **Toutes**. Si votre application comporte plusieurs packages, vous pouvez en choisir un ici.
-   **Marché**: le filtre par défaut est **tous les marchés**, mais vous pouvez limiter les données à un ou plusieurs marchés.
-   **Type d’appareil**: le paramètre par défaut est tout, mais vous pouvez choisir d’afficher **les**données pour un seul type d’appareil spécifique (PC, console, tablette, etc.).

Les informations de tous les graphiques répertoriés ci-dessous reflètent la plage de dates et les filtres que vous avez sélectionnés (à l’exception des **nouveaux utilisateurs** dans le graphique d' **utilisation** , qui ne s’affichent pas si des filtres sont sélectionnés). Certaines sections vous permettent également d’appliquer des filtres supplémentaires.

> [!IMPORTANT]
> Ce rapport inclut uniquement les données d’utilisation des clients sur Windows 10 (y compris la Xbox) qui n’ont pas choisi de fournir des informations de télémétrie. Les données d’utilisation pour les jeux Xbox sont incluses ici, que le client ait été connecté à Xbox Live ou non. 


## <a name="usage"></a>Utilisation

Le graphique **utilisation** affiche des détails sur la façon dont vos clients utilisent votre application pendant la période sélectionnée. Notez que ce graphique n’effectue pas le suivi des utilisateurs uniques pour votre application ou des sessions utilisateur uniques (autrement dit, un utilisateur est représenté dans ce graphique, qu’il ait utilisé votre application une seule fois ou plusieurs fois).

Ce graphique comporte des onglets distincts que vous pouvez afficher en indiquant l’utilisation par jour ou par semaine (selon la durée sélectionnée).

- **Utilisateurs**: affiche le nombre total de **sessions utilisateur** sur la période sélectionnée. Chaque session utilisateur représente une période distincte, en commençant à la fin de l’exécution de l’application (démarrer le processus) et jusqu’à la fin de la procédure (fin du processus) ou après une période d’inactivité. Pour cette raison, un seul client peut avoir plusieurs sessions utilisateur au cours du même jour ou de la même semaine. Le nombre total d' **utilisateurs actifs** (n’importe quel client utilisant l’application le jour ou la semaine) et les **nouveaux utilisateurs** (un client qui a utilisé votre application pour la première fois ce jour ou cette semaine) sont également affichés. Notez que si vous avez appliqué des filtres à la page, vous ne verrez pas de **nouveaux utilisateurs** dans ce graphique.
- **Appareils**: affiche le nombre d’appareils quotidiens utilisés pour interagir avec votre application par tous les utilisateurs.
- **Durée**: affiche le nombre total d’heures d’engagement (heures où un utilisateur utilise activement votre application).
- **Engagement**: affiche le nombre moyen de minutes d’engagement par utilisateur (durée moyenne de toutes les sessions utilisateur). 
- **Rétention**: affiche le nombre total de **UAQ/Mau** (utilisateurs actifs quotidiens/utilisateurs actifs mensuels) sur la période sélectionnée.

Lorsque la période **30D** est sélectionnée, vous pouvez voir des marqueurs de cercle quand vous affichez les onglets **utilisateurs**, **appareils**ou **durée** . Celles-ci représentent une augmentation ou une diminution significatives d’une valeur donnée que nous pensons avoir à connaître. La date à laquelle le cercle apparaît représente la fin de la semaine au cours de laquelle nous avons détecté une augmentation ou une diminution significative par rapport à la semaine qui précède. Pour obtenir plus de détails sur les modifications, pointez sur le cercle.  

> [!TIP]
> Vous pouvez afficher plus d’informations relatives aux modifications importantes au cours des 30 derniers jours dans le [rapport Insights](insights-report.md).


## <a name="user-sessions"></a>Sessions utilisateur

Le graphique **sessions utilisateur** affiche le nombre total de sessions utilisateur pour votre application par marché, sur la période sélectionnée.

Comme avec les informations sur les **sessions utilisateur** dans le graphique d' **utilisation** , une session utilisateur représente une période distincte où un client interagit avec votre application, et ce graphique n’effectue pas le suivi des utilisateurs uniques pour votre application.

Vous pouvez afficher ces données sous forme de **carte** visuelle ou basculer le paramètre pour l’afficher sous forme de **tableau** . Le formulaire de table présentera cinq marchés à la fois, triés par ordre alphabétique ou par le plus grand nombre de sessions utilisateur. Vous pouvez également télécharger les données pour afficher des informations sur tous les marchés ensemble.


## <a name="package-version"></a>Version du package

Le graphique **sessions utilisateur** affiche le nombre total de sessions utilisateur quotidiennes pour votre application par version de package sur la période sélectionnée.

Comme pour le graphique **sessions utilisateur** , une session utilisateur représente une période distincte pendant laquelle un client interagit avec votre application, et ce graphique n’effectue pas le suivi des utilisateurs uniques pour votre application.


## <a name="custom-events"></a>Événements personnalisés

Le graphique **événements personnalisés** affiche le nombre total d’occurrences pour les événements personnalisés que vous avez définis pour votre application. Ces informations peuvent concerner plusieurs occurrences pour un même client. Vous pouvez utiliser les filtres pour sélectionner les événements personnalisés spécifiques pour lesquels vous souhaitez afficher ces données.

Les événements personnalisés sont implémentés à l’aide de la méthode [StoreServicesCustomEventLogger.Log](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) de [Microsoft Store Services SDK](../monetize/microsoft-store-services-sdk.md).

Pour plus d’informations, consultez [consigner les événements personnalisés pour le centre de développement](../monetize/log-custom-events-for-dev-center.md).


## <a name="custom-events-breakdown"></a>Répartition des événements personnalisés

Le graphique **répartition des événements personnalisés** affiche plus de détails sur la fréquence à laquelle chacun de vos événements personnalisés s’est produit. Cela peut vous aider à déterminer si les événements se produisent plus souvent pour un marché particulier, un type d’appareil ou des versions de package.

Pour chaque événement, vous verrez le nom de l’événement et un nombre d’événements qui correspondent à une combinaison spécifique du marché de l’utilisateur, du type d’appareil et de la version du package. En règle générale, vous verrez un événement répertorié plusieurs fois avec différentes combinaisons de ces facteurs. 




 