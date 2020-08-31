---
Description: Le rapport Xbox Analytics de l’espace partenaires affiche des statistiques sur la façon dont vos clients sont en contact avec les fonctionnalités Xbox de votre produit.
title: Rapport d’analytique Xbox
ms.date: 03/21/2019
ms.topic: article
keywords: Windows 10, UWP, Xbox Analytics, Xbox Live Analytics, statistiques Xbox
ms.localizationpriority: medium
ms.openlocfilehash: d38e60fbe99db09f5fb49e440249ed9454d44c35
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157933"
---
# <a name="xbox-analytics-report"></a>Rapport d’analytique Xbox

Le rapport **Xbox Analytics** de l' [espace partenaires](https://partner.microsoft.com/dashboard) affiche des statistiques sur la façon dont vos clients sont en contact avec les fonctionnalités Xbox de votre jeu. Il fournit également des informations sur l’intégrité du service pour vous aider à résoudre les erreurs du client.

> [!IMPORTANT]
> Ce rapport s’affiche uniquement si vous publiez un jeu pour Xbox ou un jeu qui utilise les services Xbox Live. Pour ce faire, vous devez passer par le [processus d’approbation du concept](../gaming/concept-approval.md), qui comprend les jeux publiés par les [partenaires Microsoft](/gaming/xbox-live/developer-program-overview#microsoft-partners) et les jeux soumis par le biais du [ ID@Xbox programme](/gaming/xbox-live/developer-program-overview#id). Les jeux publiés par le biais du [programme Xbox Live Creators](/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators) ne sont actuellement pas visibles dans ce rapport.

Vous pouvez afficher le rapport **Xbox Analytics** dans le menu de navigation gauche de votre jeu en développant **analyser** et en sélectionnant **Xbox Analytics**.  Vous pouvez afficher ces données dans l’espace partenaires ou [Télécharger le rapport](download-analytic-reports.md) en mode hors connexion.


## <a name="overview-tab"></a>Onglet Overview

Les sections de l’onglet **vue d’ensemble** affichent des informations sur l’identité de vos joueurs et sur la façon dont ils sont en contact avec les fonctionnalités Xbox Live.

Pour la plupart de ces statistiques, nous affichons également la **moyenne Xbox** afin que vous puissiez facilement voir comment vos clients interagissent avec Xbox par rapport au client Xbox moyen.

> [!NOTE]
> Ces statistiques proviennent de clients qui sont connectés à Xbox Live, et non à tous les clients Xbox.


### <a name="concurrent-usage"></a>Utilisation simultanée

Cette section affiche les données d’utilisation en temps quasi réel (avec une latence de 5-15 minutes) sur le nombre moyen de clients qui jouent votre jeu chaque minute ou heure. Vous pouvez choisir l’intervalle de temps (de la **dernière heure** jusqu’aux **7 derniers jours**) en sélectionnant l’icône de filtre dans le coin supérieur droit de cette section.


### <a name="gamerscore-distribution"></a>Distribution joueur

Cette section contient des informations sur les joueur de vos clients. Vous pouvez sélectionner **tous les jeux** pour voir la répartition de la joueur totale sur vos clients ou sélectionner **ce jeu** pour afficher la distribution des joueur obtenues par le biais de votre jeu.


### <a name="achievement-unlocks"></a>Déverrouillages de la réussite

Cette section indique le nombre total de clients qui ont déverrouillé chaque réalisation dans l’intervalle de temps spécifié. Vous pouvez choisir l’intervalle de temps (**dernier jour**, **30 derniers jours**ou **durée de vie**) en sélectionnant l’icône de filtre dans le coin supérieur droit de cette section.


### <a name="game-statistics"></a>Statistiques du jeu

Cette section comprend des onglets que vous pouvez sélectionner pour afficher des données différentes pour les clients de votre jeu. Notez que les statistiques de cette section font référence à l’utilisation des fonctionnalités en général et non dans votre produit spécifique.

- L’onglet **utilisation sociale** affiche les données relatives à la façon dont vos clients interagissent socialement.
   - Les **invites de jeu** affichent le pourcentage de vos clients qui ont envoyé des invitations (pour tout jeu).
   - **Conversation de tiers** affiche le pourcentage de vos clients qui utilisent la conversation de tiers (pour tout jeu).
   - **SMS** affiche le pourcentage de vos clients qui envoient des messages via Xbox Shell (pour tout jeu).
- L’onglet utilisation de la **diffusion en continu** affiche les pourcentages des clients de votre jeu qui regardent ou diffusent des jeux (pour n’importe quel jeu) sur Twitch et YouTube.
- L’onglet **utilisation du DVR de jeu** affiche les données relatives à la manière dont vos clients enregistrent et affichent le jeu. Vous pouvez voir les pourcentages de clients qui ont consulté et téléchargé des clips de jeu et des captures d’écran de jeu (pour n’importe quel jeu).


### <a name="friends-and-followers"></a>Amis et abonnés

Cette section montre le **nombre moyen d’amis** et le **nombre moyen de abonnés** pour les clients qui jouent votre jeu.


### <a name="accessory-usage"></a>Utilisation des accessoires

Ce graphique montre les pourcentages des clients de votre jeu qui utilisent des disques durs externes et qui utilisent des contrôleurs sans fil Xbox Elite (sur Xbox).

Ces données ne signifient pas que ces clients installaient votre produit sur des disques durs externes ou utilisaient un contrôleur Elite lors de la diffusion. Elle fait référence au nombre de clients de votre produit qui utilisent ces fonctionnalités en général.


### <a name="connection-type"></a>Type de connexion

Ce graphique montre les pourcentages des clients de votre produit qui utilisent des connexions Internet **filaires** et **sans fil** (sur Xbox).


## <a name="xbox-live-service-health-tab"></a>Onglet intégrité du service Xbox Live

Les sections de l' **onglet intégrité du service Xbox Live** vous aident à comprendre l’impact des erreurs du client Xbox Live, y compris la limitation du débit. Il vous permet également d’accéder à un point de terminaison et à un code d’État pour obtenir des informations qui vous aideront à résoudre ces problèmes et vous informe de la disponibilité des services Xbox Live spécifiques aux appels de votre produit.

> [!NOTE]
> Lorsque vous examinez ces problèmes d’informations et d’adressage, nous vous recommandons de hiérarchiser la limitation du débit, car ces erreurs ont généralement le plus grand impact sur le client.


### <a name="apply-filters"></a>Appliquer des filtres

Près du haut de l’onglet, vous pouvez sélectionner la période pour laquelle vous souhaitez afficher des données. La sélection par défaut est **30D** (30 jours), mais vous pouvez choisir d’afficher les données pour **7D** (7 jours) ou une plage de dates personnalisée que vous spécifiez (de plus de 30 jours). Pour une plage de dates personnalisée, Notez que tous les graphiques découpent la plage du graphique jusqu’au premier et au dernier jour des données fournies dans la plage de dates que vous entrez.

Vous pouvez également développer des **filtres** pour filtrer toutes les données de cette page par version de package, type de périphérique et/ou bac à sable (sandbox).
- **Version du package**: le filtre par défaut est **toutes les versions**, mais vous pouvez limiter les données d’intégrité du service à une version de package spécifique.
- **Type d’appareil**: le paramètre par défaut est **tous les appareils**, mais vous pouvez limiter les données d’intégrité du service à un type d’appareil spécifique.
- **Bac à sable (sandbox)**: le paramètre par défaut est **Retail**, mais vous pouvez limiter les données d’intégrité du service à un bac à sable (sandbox) spécifique.

Les informations de tous les graphiques listés ci-dessous reflètent la plage de dates et les filtres que vous avez sélectionnés. Certaines sections vous permettent également d’appliquer des filtres supplémentaires.


### <a name="client-errors-by-service"></a>Erreurs client par service

Le graphique **erreurs client par service** affiche le nombre d’erreurs quotidiennes du client (4xx) sur chaque service Xbox Live sur la période sélectionnée.

Vous pouvez également afficher uniquement les erreurs de limitation du débit en sélectionnant la **limitation du débit**. Cela indique le nombre d’erreurs de limitation du débit quotidien (429) et de limitation du débit (429E) sur chaque service Xbox Live sur la période sélectionnée.

> [!NOTE]
> Un code d’État 429E a été correctement retourné en tant que code d’état 200, mais cela aurait été limité si le service avait un volume élevé à ce moment-là. nous vous recommandons donc de le traiter exactement comme s’il était appliqué (429).

Par défaut, ce graphique affiche les six premiers services par nombre d’erreurs. Vous pouvez sélectionner l’icône de filtre dans le coin supérieur droit de cette section pour choisir des services différents. Vous pouvez afficher des erreurs pour un maximum de six services à la fois.

> [!NOTE]
> La légende affiche uniquement le préfixe distinctif pour chaque service (par exemple, **présence** au lieu de **Presence.Xboxlive.com**). Vous pouvez trouver l’adresse complète du service dans la table **Erreurs du client par point de terminaison** plus bas dans l’onglet **intégrité du service Xbox Live** .


### <a name="service-availability"></a>Disponibilité du service

Le graphique de **disponibilité du service** affiche la disponibilité quotidienne pour chaque service Xbox Live sur la période sélectionnée. Cela est calculé comme suit *: 1-(nombre total d’erreurs de serveur (5xx)/nombre total de réponses)* et est spécifique à votre produit, et non à l’ensemble de Xbox Live.

Par défaut, ce graphique affiche les six services qui ont connu la disponibilité la plus faible. Vous pouvez sélectionner l’icône de filtre dans le coin supérieur droit de cette section pour choisir des services différents. Vous pouvez afficher la disponibilité de six services maximum à la fois.

> [!NOTE]
> La légende affiche uniquement le préfixe distinctif pour chaque service (par exemple, **présence** au lieu de **Presence.Xboxlive.com**). Vous pouvez trouver l’adresse complète du service dans la table **Erreurs du client par point de terminaison** plus bas dans l’onglet **intégrité du service Xbox Live** .


### <a name="client-errors-by-endpoint"></a>Erreurs client par point de terminaison

La table **erreurs client par point de terminaison** affiche le nombre d’erreurs quotidiennes du client (4xx) décomposées par chaque service Xbox Live, point de terminaison et code d’État sur la période sélectionnée. Par défaut, la table est triée par le nombre total de réponses de service dans l’ordre décroissant, mais vous pouvez modifier l’ordre de tri en cliquant sur l’un des en-têtes de colonne.

Vous pouvez également afficher uniquement les erreurs de limitation du débit en sélectionnant la **limitation du débit**. Cela indique le nombre d’erreurs de limitation du débit quotidien (429) et de limitation du débit (429E) sur chaque service Xbox Live, point de terminaison et code d’État sur la période sélectionnée.

> [!NOTE]
> Un code d’État 429E a été correctement retourné en tant que code d’état 200, mais cela aurait été limité si le service avait un volume élevé à ce moment-là. nous vous recommandons donc de le traiter exactement comme s’il était appliqué (429).










 

 