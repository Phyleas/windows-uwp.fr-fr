---
Description: Le rapport d’évaluation de l’espace partenaires vous permet de consulter les avis (commentaires) que les clients ont entrés lors de l’évaluation de votre application dans le Store.
title: Rapport sur les révisions
ms.assetid: E50C3A4D-1D8A-4E5B-8182-3FAD049F2A2D
ms.date: 08/16/2018
ms.topic: article
keywords: Windows 10, UWP, revue, commentaire, réviseur
ms.localizationpriority: medium
ms.openlocfilehash: 7ec883e7bcb98d69673b520df918e085182d35ec
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210985"
---
# <a name="reviews-report"></a>Rapport sur les révisions


Le **rapport d’évaluation de l'** [espace partenaires](https://partner.microsoft.com/dashboard) vous permet de consulter les avis (commentaires) que les clients ont entrés lors de l’évaluation de votre application dans le Store.

Vous pouvez afficher ces données dans l’espace partenaires ou [Télécharger le rapport](download-analytic-reports.md) en mode hors connexion. Vous pouvez également récupérer ces données par programme à l’aide de la méthode [obtenir des révisions d’application](../monetize/get-app-reviews.md) dans l' [API REST Microsoft Store Analytics](../monetize/access-analytics-data-using-windows-store-services.md).

Vous pouvez également répondre aux avis [des clients directement à partir de cette page](respond-to-customer-reviews.md) ou par programme [par le biais de l’API Microsoft stores](../monetize/submit-responses-to-app-reviews.md).

> [!TIP]
> Pour obtenir un aperçu des avis, des évaluations et des commentaires des utilisateurs formulés pour l’ensemble de vos applications au cours des 30 derniers jours, développez **Engager** dans le menu de navigation de gauche, puis sélectionnez **Reviews and feedback**. 


## <a name="apply-filters"></a>Appliquer les filtres

Dans la zone supérieure de la page, vous pouvez sélectionner la période sur laquelle portent les avis qui vous intéressent. La valeur par défaut est **Durée de vie**, mais vous pouvez choisir d’afficher les avis portant sur des périodes de 30 jours ou de 3, 6 ou 12 mois, ou sur une plage de dates personnalisée que vous spécifiez.

Vous pouvez développer **Filtres** pour filtrer les avis affichés sur cette page en fonction des options suivantes. Ces filtres ne s’appliqueront pas aux graphiques **Répartition des classifications** et **Évaluation moyenne au fil du temps**.

-   **Évaluation** : les avis avec des évaluations incluant toutes les étoiles sont sélectionnés par défaut, mais vous pouvez sélectionner ou désélectionner des évaluations spécifiques (comprenant entre 1 et 5 étoiles) si vous souhaitez ne visualiser que les avis associés à des évaluations données.
- **Examiner le contenu**: le paramètre par défaut est **ratings with Review content**, ce qui signifie que seuls les évaluations avec le contenu Review sont affichées. Vous pouvez sélectionner **tous** pour afficher toutes les évaluations, même celles qui n’incluent pas de texte de révision écrit. Notez que le graphique de **répartition des évaluations** affiche toujours toutes les révisions, quel que soit votre choix.
-   **Version du système d’exploitation** : la valeur par défaut de ce filtre est **Toutes**. Vous pouvez choisir une version de système d’exploitation spécifique si vous souhaitez que cette page affiche uniquement les avis laissés par les clients qui utilisent cette version.
-   **Version du package** : la valeur par défaut de ce filtre est **Toutes**. Si votre application comprend plusieurs packages, vous pouvez choisir un package spécifique ici si vous souhaitez visualiser uniquement les avis laissés par les clients qui disposaient de ce package lorsqu’ils ont évalué votre application.
-   **Réponses** : la valeur par défaut de ce filtre est **Tous**. Vous pouvez choisir de ne visualiser que les avis pour lesquels vous avez [répondu aux clients](respond-to-customer-reviews.md), ou uniquement ceux auxquels vous n'avez pas encore répondu.
-   **Mises à jour** : la valeur par défaut de ce filtre est **Tous**. Vous pouvez choisir de ne visualiser que les avis qui ont été mis à jour par le client depuis que vous avez [répondu à ces avis](respond-to-customer-reviews.md), ou uniquement ceux qui n’ont pas encore été mis à jour par le client.
-   **Marché** : la valeur par défaut de ce filtre est **Tous les marchés**. Vous pouvez choisir un marché spécifique si vous ne souhaitez visualiser que les avis de clients appartenant à ce marché.
-   **Type d’appareil** : le filtre par défaut est **Tous les appareils**. Vous pouvez choisir un type d’appareil spécifique si vous souhaitez que cette page affiche uniquement les avis laissés par les clients utilisant celui-ci.
-   **Nom de catégorie** : la valeur par défaut de ce filtre est **Tous**. Vous pouvez choisir une [catégorie d’analyse de révision](#review-insight-categories) spécifique pour afficher uniquement les révisions que nous avons associées à cette catégorie. 

> [!TIP]
> Si cette page ne contient aucun avis, assurez-vous que vos filtres n’ont pas exclu la totalité des avis concernant votre application. Par exemple, si vous filtrez les avis en fonction d’un système d’exploitation non pris en charge par votre application, aucun avis n’apparaîtra sur cette page.


## <a name="ratings-breakdown"></a>Répartition des classifications

Le graphique **répartition des évaluations** apparaît en haut de ce rapport pour vous permettre d’obtenir un aperçu rapide des éléments suivants : 
- Évaluation moyenne à l’aide d’étoiles concernant l’application
- Nombre total d’évaluations de votre application au cours des 12 derniers mois.
- Nombre total d’évaluations correspondant à chaque nombre d’étoiles.
- Nombre d’évaluations de chaque type (nouvelles ou révisées) par nombre d’étoiles au cours des 12 derniers mois.
 - La section **Nouvelles évaluations** spécifie les évaluations qui ont été soumises par les clients et qui n’ont absolument pas été modifiées.
 - La section **Évaluations révisées** spécifie les évaluations qui ont été modifiées d’une façon quelconque par le client, même si cette modification porte uniquement sur le texte de l’avis.

> [!TIP]
> L’évaluation moyenne mise à disposition d’un client dans le Windows Store tient compte du marché et du type d’appareil du client. Dès lors, elle peut différer du contenu présenté dans ce rapport.

Notez que ce graphique comprend toujours toutes vos révisions, même si vous avez sélectionné **évaluations avec** l’option vérifier le contenu dans le filtre de la page **vérifier le contenu** .

Ce graphique peut également être consulté dans le [rapport d’évaluation](ratings-report.md), ainsi que des informations supplémentaires sur les évaluations de votre application.


<span id = "review-insight-categories" />

## <a name="insight-categories"></a>Catégories d’Insight

Le graphique **catégories d’Insight** regroupe vos évaluations en fonction des catégories que nous avons déterminées.

> [!NOTE]
> Lorsque vous visualisez les avis par catégorie, les avis datant de moins de 24 heures et/ou formulés dans une langue autre que l’anglais ne sont pas inclus.

Dans la zone supérieure de la page, des blocs colorés représentent les avis par catégorie. Sélectionnez l’une de ces catégories pour visualiser uniquement les avis que nous lui avons associés. Vous pouvez également utiliser les [filtres de page](#apply-filters) pour effectuer un filtrage par catégorie.

Pour visualiser une répartition du nombre d’avis par catégorie, sélectionnez **Afficher les détails**. 


## <a name="reviews"></a>Avis

Chaque avis de client contient les éléments suivants :

-   Le titre et le texte de l'avis rédigés par le client. (Les avis rédigés par les clients qui utilisent Windows Phone 8.1 et les versions antérieures sont dépourvus de titre.)
-   La date de l'avis.
-   Nom du réviseur tel qu’il apparaît dans la Microsoft Store.
-   Le pays ou la région de l'auteur de l'avis.
-   La version du package de l'application installée sur l'appareil du client au moment de la rédaction de l'avis. (Ces informations ne sont pas disponibles pour les révisions envoyées en ligne ou envoyées par les clients sur Windows 8.1 et versions antérieures.)
-   La version du système d'exploitation installée sur l'appareil du client au moment de la rédaction de l'avis.
-   Le nom de l'appareil utilisé par le client au moment de la rédaction de l'avis. (Ces informations ne sont pas disponibles pour les révisions envoyées en ligne ou envoyées par les clients sur Windows 8.1 et versions antérieures.)
-   La « note d'utilité » de l'avis signalant le nombre de fois où l'avis a été considéré comme utile par d'autres clients. Cette information est indiquée sous la forme de deux valeurs : la première spécifie le nombre de clients ayant jugé l'avis utile, la seconde correspond au nombre total de clients ayant laissé une évaluation. Par exemple, une note d'utilité de 4/10 signifie que sur 10 personnes, 4 ont trouvé l'avis utile, et 6 non. (Si aucun utilisateur n’a évalué l’utilité d’un avis, aucune note d’utilité ne s'affiche.)

Notez que les clients peuvent évaluer votre application sans formuler de commentaires. Le nombre d'avis sera donc généralement inférieur au nombre d'évaluations.

Vous pouvez trier les avis sur la page par date et/ou par évaluation, dans l'ordre croissant ou décroissant. Cliquez sur le lien **Trier par** pour afficher les options permettant de trier par **Date** et/ou **évaluation**.

Vous pouvez également utiliser la zone de recherche pour rechercher des mots ou des expressions spécifiques dans les révisions de votre application. Notez que seul le texte de révision d’origine écrit par le client est recherché, même si la révision a été écrite dans une autre langue. Le texte de révision traduit n’est pas recherché.

> [!NOTE]
> Les avis peuvent disparaître ponctuellement de ce rapport. Cela peut se produire lorsque Microsoft supprime des avis du Windows Store qui ont été rédigés par des clients qui utilisent certaines versions préliminaires ou builds de Windows 10 réservées aux Insiders. Nous procédons ainsi afin de réduire le risque de publication d’avis négatifs liés à un problème d’une version préliminaire de Windows. Nous pouvons également supprimer les avis du Windows Store qui ont été identifiés comme indésirables, inappropriés, offensants ou contraires à notre politique. Nous pensons que cette action va améliorer l’expérience utilisateur.


## <a name="translating-reviews"></a>Traduction des avis

Les commentaires qui n'ont pas été rédigés dans votre langue sont traduits par défaut. Si vous le souhaitez, vous pouvez désactiver la traduction des avis en décochant la case **Traduire les avis** située en haut à droite au-dessus de la liste des avis.

Notez que les évaluations sont traduites par un système de traduction automatique et que le résultat de la traduction n’est pas toujours précis. Le texte d’origine est fourni si vous souhaitez comparer la traduction ou utiliser un autre moyen de traduction.

Comme indiqué ci-dessus, lors de la recherche de vos révisions, seul le texte d’origine laissé par le client est recherché (et non pas le texte traduit), même si la case **traduire les revues** est cochée.


## <a name="responding-to-customer-reviews"></a>Réponse aux avis des clients

Vous pouvez utiliser l' [espace partenaires](https://partner.microsoft.com/dashboard) ou l' [API de révisions de Microsoft Store](../monetize/submit-responses-to-app-reviews.md) pour envoyer des réponses à la plupart des révisions de vos clients. Pour plus d'informations, voir l'article [Répondre aux avis des clients](respond-to-customer-reviews.md).

Vous découvrirez ci-après certaines actions supplémentaires à envisager en fonction des évaluations et des avis que vous voyez.

-   Si vous notez de nombreux avis suggérant une nouvelle fonctionnalité ou la modification d'une fonctionnalité existante, ou se plaignant d'un problème, envisagez de publier une nouvelle version répondant spécifiquement à ces commentaires. (Veillez alors à mettre à jour la [description](create-app-descriptions.md) de votre application pour indiquer que le problème a été résolu.)
-   Si l’évaluation moyenne est élevée, mais que le nombre de téléchargements est faible, vous pouvez rechercher des façons d’[élargir le public cible de votre application](attract-customers-and-promote-your-apps.md), puisque celle-ci a été bien accueillie par les utilisateurs qui l’ont essayée.


 

 

 
