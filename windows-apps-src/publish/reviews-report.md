---
Description: Le rapport d’évaluation de l’espace partenaires vous permet de consulter les avis (commentaires) que les clients ont entrés lors de l’évaluation de votre application dans le Store.
title: Rapport sur les révisions
ms.assetid: E50C3A4D-1D8A-4E5B-8182-3FAD049F2A2D
ms.date: 08/16/2018
ms.topic: article
keywords: Windows 10, UWP, revue, commentaire, réviseur
ms.localizationpriority: medium
ms.openlocfilehash: feec3577dde9144892ea1ff9f9755074d2af9429
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167333"
---
# <a name="reviews-report"></a>Rapport sur les révisions


Le **rapport d’évaluation de l'** [espace partenaires](https://partner.microsoft.com/dashboard) vous permet de consulter les avis (commentaires) que les clients ont entrés lors de l’évaluation de votre application dans le Store.

Vous pouvez afficher ces données dans l’espace partenaires ou [Télécharger le rapport](download-analytic-reports.md) en mode hors connexion. Vous pouvez également récupérer ces données par programme à l’aide de la méthode [obtenir des révisions d’application](../monetize/get-app-reviews.md) dans l' [API REST Microsoft Store Analytics](../monetize/access-analytics-data-using-windows-store-services.md).

Vous pouvez également répondre aux avis [des clients directement à partir de cette page](respond-to-customer-reviews.md) ou par programme [par le biais de l’API Microsoft stores](../monetize/submit-responses-to-app-reviews.md).

> [!TIP]
> Pour consulter rapidement les avis, les évaluations et les commentaires des utilisateurs pour l’ensemble de vos applications au cours des 30 derniers jours, développez **engagement** dans le menu de navigation de gauche, puis sélectionnez **critiques et commentaires.** 


## <a name="apply-filters"></a>Appliquer des filtres

Près du haut de la page, vous pouvez sélectionner la période pour laquelle vous souhaitez afficher les révisions. La sélection par défaut est **durée de vie**, mais vous pouvez choisir d’afficher les révisions pour 30 jours, 3 mois, 6 mois ou 12 mois, ou pour une plage de données personnalisée que vous spécifiez.

Vous pouvez développer des **filtres** pour filtrer les révisions affichées sur cette page à l’aide des options suivantes. Ces filtres ne s’appliquent pas à la **répartition des évaluations** et à l' **évaluation moyenne dans** les graphiques de temps.

-   **Évaluation**: par défaut, les révisions avec toutes les évaluations en étoile sont vérifiées, mais vous pouvez cocher et décocher des évaluations spécifiques (de 1 à 5 étoiles) si vous souhaitez afficher uniquement les révisions associées à des évaluations d’étoiles particulières.
- **Examiner le contenu**: le paramètre par défaut est **ratings with Review content**, ce qui signifie que seuls les évaluations avec le contenu Review sont affichées. Vous pouvez sélectionner **tous** pour afficher toutes les évaluations, même celles qui n’incluent pas de texte de révision écrit. Notez que le graphique de **répartition des évaluations** affiche toujours toutes les révisions, quel que soit votre choix.
-   **Version du système d’exploitation**: le paramètre par défaut est **All**. Vous pouvez choisir une version de système d’exploitation spécifique si vous souhaitez que cette page affiche uniquement les révisions laissées par les clients sur cette version du système d’exploitation.
-   **Version du package** : la valeur par défaut de ce filtre est **Toutes**. Si votre application comprend plusieurs packages, vous pouvez en choisir un ici pour afficher uniquement les révisions laissées par les clients qui avaient ce package lors de la révision de votre application.
-   **Réponses** : la valeur par défaut de ce filtre est **Tous**. Vous pouvez choisir de ne visualiser que les avis pour lesquels vous avez [répondu aux clients](respond-to-customer-reviews.md), ou uniquement ceux auxquels vous n'avez pas encore répondu.
-   **Mises à jour** : la valeur par défaut de ce filtre est **Tous**. Vous pouvez choisir de filtrer les révisions pour afficher uniquement les révisions qui ont été mises à jour par le client depuis que vous avez [répondu à une révision](respond-to-customer-reviews.md)ou uniquement celles qui n’ont pas encore été mises à jour par le client.
-   **Marché** : la valeur par défaut de ce filtre est **Tous les marchés**. Vous pouvez choisir un marché spécifique si vous ne souhaitez visualiser que les avis de clients appartenant à ce marché.
-   **Type d’appareil** : le filtre par défaut est **Tous les appareils**. Vous pouvez choisir un type d’appareil spécifique si vous souhaitez que cette page affiche uniquement les avis laissés par les clients utilisant celui-ci.
-   **Nom**de la catégorie : le filtre par défaut est **All**. Vous pouvez choisir une [catégorie d’analyse de révision](#review-insight-categories) spécifique pour afficher uniquement les révisions que nous avons associées à cette catégorie. 

> [!TIP]
> Si vous ne voyez aucune critique sur la page, vérifiez que vos filtres n’ont pas exclu toutes vos évaluations. Par exemple, si vous filtrez les avis en fonction d’un système d’exploitation non pris en charge par votre application, aucun avis n’apparaîtra sur cette page.


## <a name="ratings-breakdown"></a>Répartition des évaluations

Le graphique **répartition des évaluations** apparaît en haut de ce rapport pour vous permettre d’obtenir un aperçu rapide des éléments suivants : 
- Évaluation moyenne des étoiles d’évaluation pour l’application.
- Nombre total d’évaluations de votre application au cours des 12 derniers mois.
- Nombre total d’évaluations pour chaque évaluation d’étoile.
- Nombre d’évaluations pour chaque type d’évaluation (nouvelle ou révisée) par étoile au cours des 12 derniers mois.
 - Les **nouvelles évaluations** sont des évaluations que les clients ont soumises, mais qui n’ont pas changé.
 - Les **évaluations révisées** sont des évaluations qui ont été modifiées par le client, même en modifiant simplement le texte de la révision.

> [!TIP]
> L’évaluation moyenne qu’un client voit dans le magasin prend en compte le marché du client et le type d’appareil. il peut donc différer de ce que vous voyez dans ce rapport.

Notez que ce graphique comprend toujours toutes vos révisions, même si vous avez sélectionné **évaluations avec** l’option vérifier le contenu dans le filtre de la page **vérifier le contenu** .

Ce graphique peut également être consulté dans le [rapport d’évaluation](ratings-report.md), ainsi que des informations supplémentaires sur les évaluations de votre application.


<span id = "review-insight-categories" />

## <a name="insight-categories"></a>Catégories d’Insight

Le graphique **catégories d’Insight** regroupe vos évaluations en fonction des catégories que nous avons déterminées.

> [!NOTE]
> Les révisions datant de moins de 24 heures et/ou dans une langue autre que l’anglais ne sont pas incluses lors de la consultation des révisions par catégorie.

Près du haut de la page, vous verrez des blocs de couleur représentant les révisions par catégorie. Sélectionnez l’une de ces catégories pour afficher uniquement les révisions que nous avons associées à cette catégorie. Vous pouvez également utiliser les [filtres de page](#apply-filters) pour filtrer par catégorie.

Pour afficher une répartition du nombre de révisions par catégorie, sélectionnez **afficher les détails**. 


## <a name="reviews"></a>Révisions

Chaque avis de client contient les éléments suivants :

-   Le titre et le texte de l'avis rédigés par le client. (Les avis rédigés par les clients qui utilisent Windows Phone 8.1 et les versions antérieures sont dépourvus de titre.)
-   La date de l'avis.
-   Nom du réviseur tel qu’il apparaît dans la Microsoft Store.
-   Le pays ou la région de l'auteur de l'avis.
-   La version du package de l'application installée sur l'appareil du client au moment de la rédaction de l'avis. (Cette information n'est pas disponible pour les avis soumis par les clients qui utilisent Windows 8.1 ou une version antérieure.)
-   La version du système d'exploitation installée sur l'appareil du client au moment de la rédaction de l'avis.
-   Le nom de l'appareil utilisé par le client au moment de la rédaction de l'avis. (Cette information n'est pas disponible pour les avis soumis par les clients qui utilisent Windows 8.1 ou une version antérieure.)
-   La « note d'utilité » de l'avis signalant le nombre de fois où l'avis a été considéré comme utile par d'autres clients. Cette information est indiquée sous la forme de deux valeurs : la première spécifie le nombre de clients ayant jugé l'avis utile, la seconde correspond au nombre total de clients ayant laissé une évaluation. Par exemple, une note d'utilité de 4/10 signifie que sur 10 personnes, 4 ont trouvé l'avis utile, et 6 non. (Si aucun utilisateur n’a évalué l’utilité d’un avis, aucune note d’utilité ne s'affiche.)

Notez que les clients peuvent évaluer votre application sans formuler de commentaires. Le nombre d'avis sera donc généralement inférieur au nombre d'évaluations.

Vous pouvez trier les avis sur la page par date et/ou par évaluation, dans l'ordre croissant ou décroissant. Cliquez sur le lien **Trier par** pour afficher les options permettant de trier par **Date** et/ou **évaluation**.

Vous pouvez également utiliser la zone de recherche pour rechercher des mots ou des expressions spécifiques dans les révisions de votre application. Notez que seul le texte de révision d’origine écrit par le client est recherché, même si la révision a été écrite dans une autre langue. Le texte de révision traduit n’est pas recherché.

> [!NOTE]
> Vous pouvez parfois remarquer que les révisions disparaissent de ce rapport. Cela peut se produire lorsque Microsoft supprime des avis du Windows Store qui ont été rédigés par des clients qui utilisent certaines versions préliminaires ou builds de Windows 10 réservées aux Insiders. Nous procédons ainsi afin de réduire le risque de publication d’avis négatifs liés à un problème d’une version préliminaire de Windows. Nous pouvons également supprimer les avis du Windows Store qui ont été identifiés comme indésirables, inappropriés, offensants ou contraires à notre politique. Nous pensons que cette action va améliorer l’expérience utilisateur.


## <a name="translating-reviews"></a>Traduction des avis

Les commentaires qui n'ont pas été rédigés dans votre langue sont traduits par défaut. Si vous le souhaitez, vous pouvez désactiver la traduction des avis en décochant la case **Traduire les avis** située en haut à droite au-dessus de la liste des avis.

Notez que les évaluations sont traduites par un système de traduction automatique et que le résultat de la traduction n’est pas toujours précis. Le texte d’origine est fourni si vous souhaitez comparer la traduction ou utiliser un autre moyen de traduction.

Comme indiqué ci-dessus, lors de la recherche de vos révisions, seul le texte d’origine laissé par le client est recherché (et non pas le texte traduit), même si la case **traduire les revues** est cochée.


## <a name="responding-to-customer-reviews"></a>Réponse aux avis des clients

Vous pouvez utiliser l' [espace partenaires](https://partner.microsoft.com/dashboard) ou l' [API de révisions de Microsoft Store](../monetize/submit-responses-to-app-reviews.md) pour envoyer des réponses à la plupart des révisions de vos clients. Pour plus d'informations, voir l'article [Répondre aux avis des clients](respond-to-customer-reviews.md).

Vous découvrirez ci-après certaines actions supplémentaires à envisager en fonction des évaluations et des avis que vous voyez.

-   Si vous notez de nombreux avis suggérant une nouvelle fonctionnalité ou la modification d'une fonctionnalité existante, ou se plaignant d'un problème, envisagez de publier une nouvelle version répondant spécifiquement à ces commentaires. (Veillez alors à mettre à jour la [description](./create-app-store-listings.md) de votre application pour indiquer que le problème a été résolu.)
-   Si l’évaluation moyenne est élevée, mais que le nombre de téléchargements est faible, vous pouvez rechercher des façons d’[élargir le public cible de votre application](attract-customers-and-promote-your-apps.md), puisque celle-ci a été bien accueillie par les utilisateurs qui l’ont essayée.


 

 

 