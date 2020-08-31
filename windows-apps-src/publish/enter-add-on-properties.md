---
Description: Quand vous envoyez un module complémentaire, les options de la page Propriétés vous aident à déterminer le comportement de ce module lorsqu’il est proposé aux clients.
title: Définir les propriétés d’un module complémentaire
ms.assetid: 26D2139F-66FD-479E-940B-7491238ADCAE
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, module complémentaire, propriétés, période d’abonnement, durée de vie du produit, type de contenu, IAP, achat dans l’application, produit dans l’application
ms.localizationpriority: medium
ms.openlocfilehash: 9d092f443ab643b74cdd0221c96540fed0c7d474
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157993"
---
# <a name="enter-add-on-properties"></a>Définir les propriétés d’un module complémentaire

Lors de l’envoi d’un module complémentaire, les options de la page **Propriétés** aident à déterminer le comportement de votre module complémentaire lorsqu’il est proposé aux clients.

## <a name="product-type"></a>Type de produit

Vous sélectionnez le type de produit lors de la [création initiale du module complémentaire](set-your-add-on-product-id.md). Le type de produit que vous avez sélectionné s’affiche ici, mais vous ne pouvez pas le modifier.

> [!TIP]
> Si vous n’avez pas publié le module complémentaire, vous pouvez supprimer l’envoi et recommencer si vous souhaitez choisir un autre type de produit.

Les champs que vous voyez sur cette page varient en fonction du type de produit de votre module complémentaire.


## <a name="product-lifetime"></a>Durée de vie du produit

Si vous avez sélectionné l’option **durable** pour votre type de produit, la **durée de vie du produit** est indiquée ici. Par défaut, le champ **Durée de vie du produit** d’un module complémentaire durable affiche **Toujours**, ce qui signifie que ce module n’expire jamais. Si vous préférez, vous pouvez modifier la **durée de vie du produit** afin que le module complémentaire expire après une durée définie (avec des options de 1-365 jours).


## <a name="quantity"></a>Quantité

Si vous avez sélectionné **consommable géré** par le magasin pour votre type de produit, la **quantité** est indiquée ici. Vous devez entrer un numéro compris entre 1 et 1000000. Cette quantité est accordée au client au moment d’acquérir le module complémentaire. Ensuite, le Windows Store la rééquilibre en fonction de la consommation du module complémentaire par le client signalée par l’application.


## <a name="subscription-period"></a>Période d’abonnement

Si vous avez sélectionné **abonnement** pour le type de produit, la **période d’abonnement** est indiquée ici. Choisissez une option pour spécifier la fréquence à laquelle un client est facturé pour l’abonnement. L’option par défaut est **mensuelle**, mais vous pouvez également **Sélectionner 3 mois**, **6 mois**, **annuellement**ou **24 mois**.

> [!IMPORTANT]
> Une fois votre module complémentaire publié, vous ne pouvez plus modifier votre sélection de **période d’abonnement** .


## <a name="free-trial"></a>Essai gratuit

Si vous avez sélectionné **abonnement** pour votre type de produit, la **version d’évaluation gratuite** est également indiquée ici. L’option par défaut n’est **pas une version d’évaluation gratuite.** Si vous préférez, vous pouvez permettre aux clients d’utiliser le module complémentaire gratuitement pendant une période définie (soit **1 semaine** , soit **1 mois**). 

> [!IMPORTANT]
> Une fois votre module complémentaire publié, vous ne pouvez plus modifier votre sélection **d’évaluation gratuite** .


## <a name="content-type"></a>Type de contenu

Quel que soit le type de produit de votre module complémentaire, vous devez indiquer le type de contenu que vous offrez. Pour la plupart des modules complémentaires, le type de contenu doit être **Téléchargement de logiciels électroniques**. Si une autre option de la liste décrit mieux votre module complémentaire (par exemple, si vous proposez un téléchargement musical ou un livre électronique), sélectionnez cette option à la place.

Voici les options disponibles pour le type de contenu d’un module complémentaire :

-   Téléchargement de logiciels électroniques
-   Livres électroniques
-   Magazine électronique - édition unique
-   Journal électronique - édition unique
-   Téléchargement de musique
-   Musique en continu
-   Services de stockage de données en ligne
-   Software as a Service
-   Téléchargement de vidéos
-   Téléchargement vidéo


## <a name="additional-properties"></a>Propriétés supplémentaires

Ces champs sont facultatifs pour tous les types de modules complémentaires.

<span id="keywords" />

### <a name="keywords"></a>Mots clés

Vous pouvez fournir jusqu’à dix mots clés d’un maximum de 30 caractères chacun pour chaque module complémentaire envoyé. Votre application peut ensuite rechercher les modules complémentaires qui correspondent à ces mots clés. Cette fonctionnalité vous permet de générer dans votre application des écrans qui peuvent charger les modules complémentaires, sans avoir à spécifier directement l’ID du produit dans le code de l’application. Vous pouvez ensuite modifier à tout moment les mots clés d’un module complémentaire sans toucher le code de votre application ni soumettre l’application à nouveau.

Pour interroger ce champ, utilisez la propriété [StoreProduct. Keywords](/uwp/api/windows.services.store.storeproduct.Keywords) dans l' [espace de noms Windows. services. Store](/uwp/api/Windows.Services.Store). (Ou, si vous utilisez l' [espace de noms Windows. ApplicationModel. Store](/uwp/api/Windows.ApplicationModel.Store), utilisez la propriété [ProductListing. Keywords](/uwp/api/windows.applicationmodel.store.productlisting.Keywords) .)

> [!NOTE]
> Les mots clés ne sont pas utilisables dans les packages ciblant Windows 8 et Windows 8.1.

<span id="custom-developer-data" />

### <a name="custom-developer-data"></a>Données personnalisées du développeur

Vous pouvez entrer jusqu’à 3000 caractères dans le champ de **données de développement personnalisé** (anciennement appelé **étiquette**) pour fournir un contexte supplémentaire pour votre produit dans l’application. Le plus souvent, il se présente sous la forme d’une chaîne XML, mais vous pouvez entrer tout ce que vous souhaitez dans ce champ. Votre application peut ensuite interroger ce champ pour lire son contenu (même si l’application ne peut pas modifier les données et les renvoyer.)

Par exemple, imaginons que vous avez un jeu et que vous vendiez un module complémentaire qui autorise le client à accéder à des niveaux supplémentaires. À l’aide du champ de **données Custom Developer** , l’application peut interroger pour voir quels niveaux sont disponibles lorsqu’un client possède ce module complémentaire. Vous pouvez ajuster la valeur à tout moment (dans ce cas, les niveaux inclus), sans avoir à apporter de modifications au code dans votre application, ni soumettre à nouveau l’application, en mettant à jour les informations dans le champ des **données de développeur personnalisé** du module complémentaire, puis en publiant une soumission mise à jour pour le module complémentaire.

Pour interroger ce champ, utilisez la propriété [StoreSku. CustomDeveloperData](/uwp/api/windows.services.store.storesku.customdeveloperdata#Windows_Services_Store_StoreSku_CustomDeveloperData) dans l' [espace de noms Windows. services. Store](/uwp/api/Windows.Services.Store). (Ou, si vous utilisez l' [espace de noms Windows. ApplicationModel. Store](/uwp/api/Windows.ApplicationModel.Store), utilisez la propriété [ProductListing. Tag](/uwp/api/windows.applicationmodel.store.productlisting.tag#Windows_ApplicationModel_Store_ProductListing_Tag) .)

> [!NOTE]
> Le champ de **données développeur personnalisé** n’est pas disponible pour une utilisation dans les packages ciblant Windows 8 et Windows 8.1.

 

 

 