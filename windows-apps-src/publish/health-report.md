---
description: Le rapport d’intégrité de l’espace partenaires vous permet d’accéder aux données relatives aux performances et à la qualité de votre application, y compris les incidents et les événements qui ne répondent pas.
title: Rapport sur l’intégrité
ms.assetid: 4F671543-1E91-4E59-88A3-638E3E64539A
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, intégrité, incidents, événements non réactifs, intégrité de l’application, données d’intégrité, trace de la pile, fichier CAB, échec, échecs, PDB, symboles
ms.localizationpriority: medium
ms.openlocfilehash: a345ad9fb6f2ed5c540c6e52ef0b95e43d1adc6d
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032822"
---
# <a name="health-report"></a>Rapport sur l’intégrité

Le rapport d' **intégrité** de l' [espace partenaires](https://partner.microsoft.com/dashboard) vous permet d’accéder aux données relatives aux performances et à la qualité de votre application, y compris les incidents et les événements qui ne répondent pas. Vous pouvez afficher ces données dans l’espace partenaires ou [Télécharger le rapport](download-analytic-reports.md) en mode hors connexion. Le cas échéant, vous pouvez afficher les traces de la pile et/ou les fichiers CAB pour un débogage supplémentaire.

Vous pouvez également récupérer par programme les données de ce rapport à l’aide de l' [API REST Microsoft Store Analytics](../monetize/access-analytics-data-using-windows-store-services.md).


## <a name="apply-filters"></a>Appliquer des filtres

Près du haut de la page, vous pouvez sélectionner la période pour laquelle vous souhaitez afficher les données. La sélection par défaut est **72H** (72 heures), mais vous pouvez choisir **30D** à la place pour afficher les données au cours des 30 derniers jours. Notez que les données sont affichées dans votre fuseau horaire local pour la vue **72H** et au format UTC pour la vue **30D** .

Vous pouvez également développer des **filtres** pour filtrer toutes les données de cette page par version de package, marché et/ou type de périphérique.

-   **Version du package**  : la valeur par défaut de ce filtre est **Toutes** . Si votre application comporte plusieurs packages, vous pouvez en choisir un ici.
-   **Marché** : le filtre par défaut est **tous les marchés** , mais vous pouvez limiter les données à un ou plusieurs marchés.
-   **Type d’appareil** : le paramètre par défaut est **Tous** , mais vous pouvez choisir d’afficher les données pour un seul type d’appareil donné. Notez que l' **autre** catégorie inclut les appareils où le modèle/la marque est reconnu, mais nous ne pouvons pas l’inclure dans l’une des catégories prédéfinies indiquées dans ce filtre. Pour ces appareils, le modèle d’appareil peut être affiché dans la section **Journal des échecs** du rapport Détails de l' **échec** .  
-   **Version du système d’exploitation**  : la valeur par défaut est **Toutes les versions de système d’exploitation** , mais vous pouvez choisir une version de système d’exploitation spécifique.
-   **Version du système d’exploitation** : la valeur par défaut est **toutes les versions release du système d’exploitation** , mais vous pouvez choisir une version Release spécifique de la **version du système d’exploitation** sélectionné.
-   **Bac à sable (sandbox** ) : la valeur par défaut est **Retail** , mais pour les produits qui utilisent plusieurs bacs à sable (sandbox) de développement (tels que les jeux qui s’intègrent à Xbox Live), vous pouvez en choisir un ici spécifique. (Si votre produit n’utilise pas de bac à sable (sandbox), ce filtre affiche uniquement la **version commerciale** et ne s’applique pas.)
-   **Architecture** : la valeur par défaut est **toutes les architectures** , mais vous pouvez choisir un type d’architecture système spécifique. Ce filtre est disponible uniquement lorsque **30D** est sélectionné.
-   **Praid** : le paramètre par défaut est **tout** , mais si vous avez défini plusieurs ID d’application relatifs aux packages (PRAIDs) lors de la création de votre package d’application, vous pouvez choisir d’afficher uniquement les données relatives à un Praid. Ce filtre n’apparaît pas si vous n’avez pas défini plusieurs PRAIDs.

Les informations de tous les graphiques listés ci-dessous reflètent la plage de dates et les filtres que vous avez sélectionnés. Certaines sections vous permettent également d’appliquer des filtres supplémentaires.


## <a name="failure-hits"></a>Échecs d’accès

Le graphique sur les **échecs** affiche le nombre de blocages quotidiens et les événements rencontrés par les clients lors de l’utilisation de votre application pendant la période sélectionnée. Chaque type d’événement rencontré par votre application est suivi séparément : incidents, blocages, exceptions JavaScript et défaillances de mémoire.

Une fois la période **30D** sélectionnée, vous pouvez voir des marqueurs de cercle. Celles-ci représentent une augmentation ou une diminution significatives d’une valeur donnée que nous pensons avoir à connaître. La date à laquelle le cercle apparaît représente la fin de la semaine au cours de laquelle nous avons détecté une augmentation ou une diminution significative par rapport à la semaine qui précède. Pour obtenir plus de détails sur les modifications, pointez sur le cercle.  

> [!TIP]
> Vous pouvez afficher plus d’informations relatives aux modifications importantes au cours des 30 derniers jours dans le [rapport Insights](insights-report.md).

## <a name="failure-hits-by-market"></a>Échecs d’accès par marché

Le graphique sur les **échecs de marché** affiche le nombre total de pannes et d’événements sur la période de temps sélectionnée par le marché.

Vous pouvez afficher ces données sous forme de **carte** visuelle ou basculer le paramètre pour l’afficher sous forme de **tableau** . Le formulaire de table présentera cinq marchés à la fois, triés par ordre alphabétique ou par le plus grand nombre de sessions utilisateur. Vous pouvez également télécharger les données pour afficher des informations sur tous les marchés ensemble.


## <a name="package-version"></a>Version du package

Le graphique **Version du package** présente le nombre total d’incidents et d’événements survenus par version du package au cours de la période sélectionnée. Par défaut, la version du package ayant donné lieu au plus grand nombre d’occurrences apparaît en premier dans le graphique. Vous pouvez inverser cet ordre en cliquant sur la flèche dans la colonne **Occurrences** de ce graphique.

## <a name="failures"></a>Échecs

Le graphique **Échecs** présente le nombre total d’incidents et d’événements survenus par nom d’échec au cours de la période sélectionnée. Chaque nom d’erreur se compose de quatre parties : une ou plusieurs classes de problèmes, un code de vérification des exceptions/bogues, le nom de l’image/du pilote où l’échec s’est produit et le nom de la fonction associée. Par défaut, l’échec ayant donné lieu au plus grand nombre d’occurrences apparaît en premier dans le graphique. Vous pouvez inverser cet ordre en cliquant sur la flèche dans la colonne **Occurrences** de ce graphique. Pour chaque échec, le pourcentage du nombre total d’échecs apparaît également.

> [!TIP]
> Dans certains cas, vous pouvez voir une entrée **inconnue** dans cette section. Cela se produit quand, en dépit de nos meilleurs efforts, nous ne parvenons pas à collecter des détails complets pour une ou plusieurs défaillances, qui sont toutes regroupées sous un **inconnu** . La plupart du temps, cela se produit en raison de contraintes de stockage, mais il peut également résulter des paramètres de confidentialité d’un appareil, des problèmes de connexion réseau, des vidages sur incident partiels/incorrects et d’autres facteurs.
>
> Si vous voyez **! Unknown** dans le cas d’un nom d’échec, cela signifie que les symboles n’étaient pas présents, nous n’avons donc pas pu identifier le nom de l’échec. Veillez à inclure des symboles dans votre package pour bénéficier d’une analyse de défaillance précise. Consultez [configurer un package d’application](/windows/msix/package/packaging-uwp-apps#configure-an-app-package). En revanche, les noms d’échec qui incluent **! unknown_error_in_** et **! unknown_function** signifient que nous n’avons pas pu rassembler des détails complets pour différentes autres raisons.

Pour afficher le rapport **Détails de l’échec** d’un échec particulier, sélectionnez le nom de l’échec. Si vous avez inclus des fichiers de symboles, le rapport Détails de l' **échec** inclut le nombre d’échecs au cours du dernier mois, ainsi qu’un journal des échecs qui répertorie les détails de l’occurrence (date, version du package, type d’appareil, modèle de périphérique, version du système d’exploitation) et un lien vers la trace de la pile et/ou le fichier CAB, si disponible

> [!TIP]
> Les fichiers CAB ne sont disponibles que lorsque l’échec s’est produit sur un ordinateur utilisant une version Windows Insider, si bien que tous les échecs ne comportent pas l’option de téléchargement CAB. Pour afficher uniquement les échecs qui ont des fichiers CAB, sélectionnez **échecs avec téléchargements** dans la section filtre. Vous pouvez également cliquer sur l’en-tête **des liens** dans le **Journal des échecs** pour trier les résultats afin que les erreurs qui incluent des fichiers CAB apparaissent en haut de la liste.

Dans la page Détails de l' **échec** , vous verrez également le graphique de l’importance de la **pile** , qui montre les piles supérieures ayant contribué à l’échec, classées par pourcentage et le graphique Configuration de l' **appareil (30D)** , qui fournit des détails sur la configuration des appareils qui ont subi la défaillance. 


## <a name="crash-free-sessions-and-devices-30d"></a>Sessions et appareils sans blocage (30D)

Le graphique **sessions et appareils sans incident** montre le pourcentage d’appareils ou de sessions utilisateur qui n’ont pas subi de panne au cours des 30 derniers jours. Ces informations vous aident à comprendre la façon dont vos incidents affectent vos utilisateurs. Par exemple, une application peut avoir 10 000 pannes en une journée. Si 90% de vos appareils sont affectés, vous pouvez probablement les classer comme critiques et agir pour résoudre le problème immédiatement. Toutefois, si cela représente seulement 5% des appareils utilisant votre application, la priorité peut être inférieure.

Ce graphique comporte deux onglets :
- **Appareils sans incident** : affiche le pourcentage d’appareils uniques qui n’ont pas subi une défaillance chaque jour (au cours des 30 derniers jours).
- **Sessions sans blocage** : affiche le pourcentage de sessions utilisateur uniques qui n’ont pas subi une défaillance chaque jour (au cours des 30 derniers jours).


 

 
