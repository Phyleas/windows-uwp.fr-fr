---
Description: Si vous avez fourni des packages ciblant différents systèmes d’exploitation, vous pouvez personnaliser certaines parties de votre description dans le Windows Store pour ces différents systèmes.
title: Création d’annonces Windows Store spécifiques à la plateforme
ms.assetid: 5BE66BE2-669C-49E0-8915-60F1027EF94A
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, personnaliser, liste, description, plus haut
ms.localizationpriority: medium
ms.openlocfilehash: 475526662737eaa5b95267e36016e342c6652a89
ms.sourcegitcommit: 96b7be654a0922eeb421b5fa51ebfc586abe74fe
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84945957"
---
# <a name="create-platform-specific-store-listings"></a>Création d’annonces Windows Store spécifiques à la plateforme


Si votre application publiée précédemment contient des packages qui ciblent des systèmes d’exploitation différents, vous avez la possibilité de personnaliser des parties de votre liste de magasins pour les clients sur des versions de système d’exploitation antérieures (Windows 8. x ou version antérieure et/ou Windows Phone 8. x ou version ultérieure). 

Les clients sur Windows 10 (y compris la Xbox) verront toujours la [liste des boutiques](create-app-store-listings.md)par défaut. Vous ne verrez pas l’option permettant de créer des listes de magasins spécifiques à la plateforme, sauf si vous avez déjà publié votre application avec des packages qui prennent en charge une ou plusieurs versions antérieures du système d’exploitation. 

> [!IMPORTANT]
> Vous ne pouvez plus charger de nouveaux packages XAP générés à l’aide du ou des kits de développement logiciel (SDK) Windows Phone 8. x. Les applications qui sont déjà dans Store avec des packages XAP continuent de fonctionner sur les appareils Windows 10 mobile. Pour plus d’informations, consultez ce billet de [blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

Les listes de magasins spécifiques à la plateforme peuvent être utiles si vous souhaitez mentionner les fonctionnalités qui apparaissent uniquement dans une version du système d’exploitation ou si vous souhaitez fournir des captures d’écran spécifiques à un système d’exploitation particulier (indépendamment du type d’appareil).

> [!NOTE]
> La création d’une liste de magasins spécifique à une plateforme dans une langue ne crée pas de liste de magasins spécifique à la plateforme dans d’autres langues que votre application prend en charge. Il vous faudra créer la description dans le Windows Store spécifique à la plateforme pour chacune des langues. Notez également que vous ne pouvez pas [Importer et exporter des données de liste de magasins](import-and-export-store-listings.md) pour des listes spécifiques à la plateforme.


## <a name="creating-a-platform-specific-store-listing"></a>Création d’une description dans le Windows Store spécifique à la plateforme

En haut de la page de liste de votre **Boutique** , si votre application précédemment publiée comprend des packages qui prennent en charge des versions antérieures du système d’exploitation ((Windows 8. x ou une version antérieure et/ou Windows Phone 8. x ou une version antérieure), vous pouvez sélectionner **créer un listing de l’App Store spécifique à la plateforme**. Après avoir sélectionné cette option, vous êtes invité à choisir parmi les versions de système d’exploitation ciblées prises en charge par votre envoi. Une fois que vous avez déjà créé des listes de magasins spécifiques à la plateforme pour toutes les versions antérieures du système d’exploitation ciblées par votre application, vous ne pouvez pas effectuer une autre sélection.

Vous pouvez utiliser votre liste de magasins par défaut (Windows 10) comme point de départ, qui placera le texte et les images applicables que vous avez entrés pour votre liste de magasins par défaut. vous serez alors en mesure d’apporter les modifications souhaitées avant l’enregistrement. Vous pouvez également choisir de partir d’une description vide.

Une fois que vous avez cliqué sur **Continuer**, la page de liste de votre **Boutique** inclut désormais une section pour la liste des magasins spécifiques à la plateforme que vous venez de créer. Cette section inclut son propre ensemble de champs pour **Description** (obligatoire), **Nouveautés de cette version**, **captures d’écran**, icône de vignette d' **application**, **fonctionnalités d’application**et **Configuration requise supplémentaire**. Veillez à entrer des informations dans chaque champ où vous souhaitez afficher des informations pour la description personnalisée dans le Windows Store, même si ces informations sont identiques à celles de la description par défaut. Si vous laissez un de ces champs vide, aucune information ne s’affiche pour ce champ dans la description personnalisée dans le Windows Store.

> [!IMPORTANT]
> Les champs de la section [informations supplémentaires](create-app-store-listings.md#additional-information) de la liste des magasins ne peuvent pas être personnalisés pour différentes versions de système d’exploitation.
> 
> En outre, étant donné que certains champs de la page de liste de la [Boutique](create-app-store-listings.md) par défaut s’appliquent uniquement aux clients sur Windows 10, vous ne verrez pas toutes les mêmes options lors de la création d’une liste de magasins spécifique à la plateforme. Par exemple, vous ne pouvez pas ajouter de codes de fin à un listing de magasins spécifique à une plateforme, car les codes de fin sont affichés uniquement aux clients sur Windows 10, version 1607 ou ultérieure. 

Vous pouvez continuer à modifier les annonces spécifiques à la plateforme en fonction des besoins pour apporter des modifications aux clients sur une certaine version du système d’exploitation.


## <a name="removing-a-platform-specific-store-listing"></a>Suppression d’une description dans le Windows Store spécifique à la plateforme

Si vous créez une liste de magasins spécifique à une plateforme et que vous décidez ultérieurement d’afficher votre annonce par défaut pour les clients de ce système d’exploitation, sélectionnez le lien **supprimer** en regard de cette liste.

Une fois que vous avez confirmé que vous souhaitez afficher la liste de vos clients par défaut, sélectionnez **OK**. La liste de la boutique propre à la plateforme sera supprimée (pour toutes les langues dans lesquelles elle existait) et les clients sur cette version du système d’exploitation verront désormais votre liste de magasins par défaut. Si vous changez d’avis, vous pouvez créer une autre liste de magasins propre à la plateforme pour ce système d’exploitation en suivant les étapes indiquées ci-dessus.
 

 




