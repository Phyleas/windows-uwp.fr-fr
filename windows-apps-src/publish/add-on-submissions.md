---
Description: Les modules complémentaires (ou produits dans l’application) sont publiés via l’espace partenaires.
title: Soumissions de modules complémentaires
ms.assetid: E175AF9E-A1D4-45DF-B353-5E24E573AE67
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, iap, achat in-app, produit in-app, soumission iap
ms.localizationpriority: medium
ms.openlocfilehash: 87e5298f677a078eb1d6f89b72af3ea15d44b75f
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259061"
---
# <a name="add-on-submissions"></a>Soumissions de modules complémentaires

Les modules complémentaires (parfois appelés produits in-app) sont des éléments qui complètent votre application et qui peuvent être achetés par les clients. Un module complémentaire peut être une nouvelle fonctionnalité amusante, un nouveau niveau de jeu, ou tout autre élément que vous pensez garder les utilisateurs engagés. Les modules complémentaires constituent non seulement une excellente façon de gagner de l’argent, mais également un bon moyen de renforcer le niveau d’interactivité et d’implication des clients avec votre application.

Les modules complémentaires sont publiés via l' [espace partenaires](https://partner.microsoft.com/dashboard)et nécessitent un [compte de développeur](https://developer.microsoft.com/store/register)actif. Vous devez également [activer les modules complémentaires](../monetize/in-app-purchases-and-trials.md) dans le code de votre application.

La première étape du processus d’envoi de complément consiste à créer le module complémentaire dans l’espace partenaires en [définissant son type de produit et son ID de produit](set-your-add-on-product-id.md). Après cela, vous allez créer une soumission afin que votre module complémentaire puisse être acheté via le Microsoft Store. Vous pouvez soumettre un module complémentaire au moment où vous [soumettez votre application](app-submissions.md), ou le soumettre séparément. Enfin, après avoir publié une application dans le Windows Store, vous pouvez proposer des [mises à jour](#updating-an-add-on-after-publication) des modules complémentaires sans avoir à soumettre une nouvelle fois l’application.

> [!NOTE]
> Cette section de la documentation explique comment envoyer des modules complémentaires dans l’espace partenaires. Sinon, vous pouvez utiliser [l’API de soumission au Microsoft Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) pour automatiser la soumission des extensions.


## <a name="checklist-for-submitting-an-add-on"></a>Liste de vérification relative à la soumission d’un module complémentaire

Voici la liste des informations que vous fournissez quand vous créez la soumission d’un module complémentaire. Les éléments que vous devez obligatoirement spécifier sont signalés ci-dessous. Quelques-uns sont facultatifs ou présentent déjà des valeurs par défaut que vous pouvez modifier selon vos besoins.


### <a name="create-a-new-add-on-page"></a>Créer une nouvelle page de module complémentaire

| Nom du champ                    | Remarques                            |
|-------------------------------|----------------------------------|
| [**Type de produit**](set-your-add-on-product-id.md#product-type)      | Obligatoire |  
| [**ID du produit**](set-your-add-on-product-id.md#product-id)          | Obligatoire |        


### <a name="properties-page"></a>Page Propriétés

| Nom du champ                    | Remarques                              |   
|-------------------------------|------------------------------------|
| [**Durée de vie du produit**](enter-add-on-properties.md#product-lifetime)  | Obligatoire si le type de produit est **Durable**. Non applicable à d’autres types de produit. |
| [**Spécifiée**](enter-add-on-properties.md#quantity)  | Obligatoire si le type de produit est **Consommable géré par le Windows Store**. Non applicable à d’autres types de produit. |
| [**Période d’abonnement**](enter-add-on-properties.md#subscription-period)          | Obligatoire si le type de produit est **Abonnement**. Non applicable à d’autres types de produit.       |  
| [**Essai gratuit**](enter-add-on-properties.md#free-trial)          | Obligatoire si le type de produit est **Abonnement**. Non applicable à d’autres types de produit.       |
| [**Type de contenu**](enter-add-on-properties.md#content-type)          | Obligatoire    |               
| [**Mot**](enter-add-on-properties.md#keywords)                  | Facultatif (jusqu’à 10 mots clés d’un maximum de 30 caractères chacun) |
| [**Données personnalisées pour les développeurs**](enter-add-on-properties.md#custom-developer-data)   | Facultatif (3 000 caractères maximum)            |


### <a name="pricing-and-availability-page"></a>Page Tarification et disponibilité

| Nom du champ                    | Remarques                                       |
|-------------------------------|---------------------------------------------|
| [**Secteurs**](set-add-on-pricing-and-availability.md#markets)  | Par défaut : tous les marchés possibles |
| [**Vue**](set-add-on-pricing-and-availability.md#visibility)   | Par défaut : disponible à l’achat. Affichable dans la description de votre application |
| [**Programmateur**](set-add-on-pricing-and-availability.md#schedule)    | Par défaut : publication dès que possible
| [**Tarifaire**](set-add-on-pricing-and-availability.md#pricing)                | Obligatoire                                    |
| [**Tarification des ventes**](put-apps-and-add-ons-on-sale.md)               | Facultatif                    |


### <a name="store-listings"></a>Descriptions dans le Windows Store

Une description est requise dans le Windows Store. Nous vous recommandons de fournir une description dans le Windows Store pour chaque [langue](create-add-on-store-listings.md#store-listing-languages) prise en charge par votre application.

| Nom du champ                    | Remarques                                       |
|-------------------------------|---------------------------------------------|
| [**Bonhomme**](create-add-on-store-listings.md#title)                    | Obligatoire (100 caractères maximum)           |
| [**Descriptive**](create-add-on-store-listings.md#description)       | Facultatif (200 caractères maximum)            |
| [**Située**](create-add-on-store-listings.md#icon)                    | Facultatif (fichier .png, 300 x 300 pixels)            |


Quand vous avez terminé d'entrer ces informations, cliquez sur **Soumettre au Windows Store**. Dans la plupart des cas, le processus de certification prend environ une heure. Au terme de ce processus, votre module complémentaire est publié sur le Windows Store et devient aussitôt disponible à l’achat par les clients.

> [!NOTE]
> Les extensions doivent également être implémentées dans le code de votre application. Pour plus d’informations, consultez l’article [Achats dans l’application et versions d’évaluation](../monetize/in-app-purchases-and-trials.md).


## <a name="updating-an-add-on-after-publication"></a>Mise à jour d’un module complémentaire après sa publication

Un module complémentaire est modifiable à tout moment après sa publication. Les modifications du module complémentaire sont soumises et publiées indépendamment de votre application. vous n’avez donc généralement pas besoin de mettre à jour l’application entière pour apporter des modifications à un module complémentaire, telles que la mise à jour de son prix ou de sa description.

Pour envoyer des mises à jour, accédez à la page du module complémentaire dans l’espace partenaires, puis cliquez sur **mettre à jour**. Cette opération crée une nouvelle soumission pour le module complémentaire, en utilisant les informations de votre soumission précédente comme point de départ. Apportez les modifications souhaitées, puis cliquez sur **Envoyer au magasin**.

Si vous voulez supprimer une extension précédemment proposé, vous pouvez créer une soumission et modifier l’option [Distribution et visibilité](set-add-on-pricing-and-availability.md) en la définissant sur **Hidden in the Store** (Plus disponible à l’achat) avec l'option **Stop acquisition** (arrêt acquisition). Veillez à mettre à jour le code de votre application en fonction des besoins pour supprimer également les références au module complémentaire (surtout si votre application précédemment publiée prend en charge Windows 8.1 plus tôt ; ce paramètre de visibilité ne s’appliquera pas à ces clients).

> [!IMPORTANT]
> Si votre application précédemment publiée est disponible pour les clients sur Windows 8. x, vous devez créer et publier une nouvelle soumission d’application pour que les mises à jour du module complémentaire soient visibles pour ces clients. De même, si vous ajoutez de nouveaux modules complémentaires dans une application ciblant Windows 8.x après la publication de cette dernière, vous devrez mettre à jour le code de votre application pour référencer ces modules, puis soumettre de nouveau l’application. Dans le cas contraire, les nouveaux modules complémentaires ne seront pas visibles par les clients utilisant Windows 8.x.
