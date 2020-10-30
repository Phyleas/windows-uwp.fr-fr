---
description: Découvrez comment créer des segments de clients afin de cibler un sous-ensemble de votre clientèle à des fins de promotion ou d’engagement.
title: Créer des segments de clients
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, segment, segments, groupe ciblé, clients
ms.assetid: 58185f6c-d61f-478b-ab24-753d8986cd5a
ms.localizationpriority: medium
ms.openlocfilehash: 563cf6767a9e7b175c861b11bd125ae6f45a3bba
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93029752"
---
# <a name="create-customer-segments"></a>Créer des segments de clients

Parfois, vous avez besoin de cibler un sous-ensemble de votre clientèle à des fins de promotion et d’engagement. Vous pouvez le faire dans l' [espace partenaires](https://partner.microsoft.com/dashboard) en créant un type de [groupe de clients](create-customer-groups.md) appelé *segment* qui comprend les clients Windows 10 qui répondent aux critères démographiques ou de revenus que vous choisissez.

Par exemple, vous pouvez créer un segment qui comprend uniquement les clients âgés de 50 ou plus, ou qui incluent les clients qui ont passé plus de $10 dans le Microsoft Store. Vous pouvez également combiner ces critères et créer un segment qui inclut tous les clients de plus de 50 ans qui ont dépensé plus de 10 $ dans le Windows Store. 

Nous fournissons quelques modèles de segment pour démarrer, mais vous pouvez définir et combiner les critères comme bon vous semble.

> [!TIP]
> Les segments peuvent être utilisés pour envoyer des [notifications ciblées](send-push-notifications-to-your-apps-customers.md) ou des [offres ciblées](use-targeted-offers-to-maximize-engagement-and-conversions.md) à un groupe de clients dans le cadre de vos campagnes d’engagement.

Éléments à se rappeler concernant les segments de clients :
- Après avoir enregistré un segment, vous devez patienter 24 heures avant de pouvoir l’utiliser pour les [notifications push ciblées](send-push-notifications-to-your-apps-customers.md).
- Les résultats du segment sont actualisés quotidiennement. Vous pouvez donc voir le nombre total de clients d’un segment changer d’un jour sur l’autre selon que les clients satisfont ou non les critères du segment.
- La plupart des attributs de segment sont calculés à l’aide de toutes les données d’historique, bien qu’il existe des exceptions. Par exemple, les champs **Date d’acquisition de l’application** , **ID de campagne** , **Stocker la date d’affichage de la page** et **Domaine URI référent** sont limités aux 90 derniers jours de données.
- Les segments incluent uniquement les clients qui ont acquis votre application sur Windows 10 tout en étant connecté avec un compte Microsoft valide. 
- Les segments n’incluent pas les clients âgés de plus de 17 ans.

## <a name="to-create-a-customer-segment"></a>Pour créer un segment de clients

1.  Dans l' [espace partenaires](https://partner.microsoft.com/dashboard), développez **engagement** dans le menu de navigation gauche, puis sélectionnez **groupes de clients** .
2.  Sur la page **Groupes de clients** , effectuez l’une des opérations suivantes :
 - Dans la section **Mes groupes de clients** , sélectionnez **Créer un groupe** pour définir entièrement un segment. Sur la page suivante, sélectionnez la case d’option **segment** .
 - Dans la section **modèles de segments** , sélectionnez **copier** en regard de l’un des segments prédéfinis (que vous pouvez utiliser tels quels ou modifier en fonction de vos besoins).
3.  Dans la zone **nom du groupe** , entrez un nom pour votre segment.
4.  Dans la liste **Inclure des clients à partir de cette application** , sélectionnez l’une de vos applications à cibler.
5.  Dans la section **définir les conditions d’inclusion** , spécifiez les critères de filtre pour le segment.

    Vous pouvez choisir parmi différents critères de filtre, y compris les **acquisitions** , la **source d’acquisition** , les **données démographiques** , la **classification** , la **prédiction d’évolution** , les achats de **magasins** , les **acquisitions de magasins** et les dépenses de **stockage** .

    Par exemple, si vous voulez créer un segment ne comprenant que les clients de votre application âgés de 18 à 24 ans, vous devez sélectionner les critères de filtre [ **Démographie** ] [ **Groupe d’âges** ] [ **est** ] [ **18 à 24** ] dans les listes déroulantes.

    Vous pouvez générer des segments plus complexes à l’aide des requêtes AND/OR pour inclure et exclure des clients en fonction de différents attributs. Pour ajouter une requête OR, sélectionnez **+ instruction OR** . Pour ajouter une requête ADD, sélectionnez **Ajouter un autre filtre** .

    Par conséquent, si vous souhaitez affiner ce segment pour inclure seulement des clients masculins se trouvant dans la tranche d’âge spécifiée, sélectionnez **Ajouter un autre filtre** , puis sélectionnez les critères de filtre supplémentaires [ **Démographie** ] [ **Sexe** ] [ **est** ] [ **Masculin** ]. Dans cet exemple, **Définition du segment** affiche **Groupe d’âge == 18 à 24 &&amp;amp; Sexe == Masculin** .

    ![Exemple de critères de filtre pour un segment](images/create-segment-inclusions.png)
6. Sélectionnez **Enregistrer** .

> [!IMPORTANT]
> Vous ne pouvez pas utiliser un segment qui comprend trop peu de clients. Si votre définition de segment n’inclut pas suffisamment de clients, vous pouvez ajuster les critères du segment ou réessayer plus tard, si votre application a acquis plus de clients répondant à vos critères de segment.


## <a name="app-statistics"></a>Statistiques de l’application

La section **Statistiques de l’application** du segment fournit des informations sur votre application, ainsi que la taille du segment que vous venez de créer.

Notez que le champ **Clients de l’application disponibles** ne reflète pas le nombre réel de clients qui ont acquis votre application, mais uniquement le nombre de clients disponibles pouvant être inclus dans les segments (c’est-à-dire, les clients dont nous pouvons déterminer qu’ils satisfont aux conditions d’âge, ont acquis votre application sur Windows 10 et qui sont associés à un compte Microsoft valide).

Si vous affichez les résultats et que les **clients de ce segment** indiquent **petit** , le segment n’inclut pas suffisamment de clients et le segment est marqué comme étant inactif. Vous ne pouvez pas utiliser les segments inactifs pour les notifications ou d’autres fonctionnalités. Vous pouvez activer et utiliser un segment en effectuant l’une des opérations suivantes :

- Dans la section **Définir des conditions d’inclusion** , ajustez les critères de filtre pour que le segment inclut davantage de clients.
- Sur la page **groupes de clients** , dans la section **segments inactifs** , sélectionnez **Actualiser** pour voir si le segment contient maintenant suffisamment de clients (par exemple, si plus de clients qui répondent à vos critères de segment ont téléchargé votre application depuis que vous avez créé le segment pour la première fois, ou si davantage de clients existants répondent à vos critères de segment).
