---
Description: Lorsque vous créez un module complémentaire dans l’espace partenaires, vous devez spécifier un type de produit et lui affecter un ID de produit.
title: Définir le type et l’ID de produit d’un module complémentaire
ms.assetid: 59497B0F-82F0-4CEE-B628-040EF9ED8D3D
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, modules complémentaires, IAP, durable, consommable, abonnement, type de produit, ID de produit, achat dans l’application, produit dans l’application
ms.localizationpriority: medium
ms.openlocfilehash: f62ac8c7ad366505d6730493212e7f8ba588012f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170993"
---
# <a name="set-your-add-on-product-type-and-product-id"></a>Définir le type et l’ID de produit d’un module complémentaire

Un module complémentaire doit être associé à une application que vous avez créée dans l' [espace partenaires](https://partner.microsoft.com/dashboard) (même si vous ne l’avez pas encore envoyée). Vous pouvez trouver le bouton permettant de **créer un module complémentaire** sur la page **vue d’ensemble** de votre application ou sur la page **modules** complémentaires.

Une fois que vous avez sélectionné **créer un module complémentaire**, vous êtes invité à spécifier un type de produit et à affecter un ID de produit pour votre module complémentaire.

## <a name="product-type"></a>Type de produit

Vous devez commencer par indiquer le type de module complémentaire que vous proposez. Cette sélection détermine la façon dont le client pourra utiliser votre module complémentaire.

> [!NOTE]
> Vous ne pourrez pas modifier le type de produit une fois que vous aurez enregistré cette page pour créer le module complémentaire. Si vous choisissez un type de produit incorrect, vous pouvez toujours supprimer votre soumission de module complémentaire en cours et recommencer en créant un nouveau module complémentaire.

<span id="durable" />

### <a name="durable"></a>Durable

Sélectionnez **durable** comme type de produit si votre module complémentaire n’est généralement acheté qu’une seule fois. Ces modules complémentaires sont souvent utilisés pour déverrouiller des fonctionnalités supplémentaires dans une application.

Par défaut, le champ **Durée de vie du produit** d’un module complémentaire durable affiche **Toujours**, ce qui signifie que ce module n’expire jamais. Vous avez la possibilité de définir la durée de **vie du produit** sur une durée différente à l’étape [Propriétés](enter-add-on-properties.md) du processus d’envoi du module complémentaire. Dans ce cas, le module complémentaire expire après la durée que vous spécifiez (avec des options de 1-365 jours), auquel cas un client peut l’acheter à nouveau après son expiration.

### <a name="consumable"></a>Consommables

Si le module complémentaire peut être acheté, utilisé (consommé), puis acheté à nouveau, vous souhaiterez sélectionner l’un des types de produits **consommables** . Les modules complémentaires consommables sont souvent utilisés pour la monnaie d’un jeu par exemple (or, pièces, etc.), qui peuvent être achetés en montants prédéfinis puis dépensés par le client. Pour plus d’informations, consultez [activer les achats de modules complémentaires consommables](../monetize/enable-consumable-add-on-purchases.md).

Il existe deux types de modules complémentaires consommables :
- **Consommable géré par le développeur**: l’équilibrage et l’avancement doivent être gérés au sein de votre application. Pris en charge sur toutes les versions de système d’exploitation.
- **Consommable géré par le Windows store :** Microsoft assure le suivi du solde sur tous les appareils du client fonctionnant sous Windows 10, version 1607 ou ultérieure ; non pris en charge sur les versions antérieures du système d’exploitation. Pour utiliser cette option, le produit parent doit être compilé à l’aide du Kit de développement logiciel Windows 10 version 14393 ou ultérieure. Notez également que vous ne pouvez pas envoyer un module complémentaire de consommation géré par le magasin au Store tant que le produit parent n’a pas été publié (vous pouvez créer l’envoi dans l’espace partenaires et commencer à l’utiliser à tout moment). Vous devez entrer la quantité de votre module complémentaire consommable géré par le magasin à l’étape **Propriétés** de votre envoi.

### <a name="subscription"></a>Abonnement

Si vous souhaitez que les clients soient facturés sur une base récurrente pour votre module complémentaire, choisissez **abonnement**.

Une fois qu’un module complémentaire d’abonnement est acquis initialement par un client, il continue à être facturé à intervalles réguliers afin de continuer à utiliser le module complémentaire. Le client peut annuler l’abonnement à tout moment pour éviter d’autres frais. Vous devez spécifier la période d’abonnement et s’il faut offrir une version d’évaluation gratuite, à l’étape **Propriétés** de votre envoi.

Les modules complémentaires d’abonnement sont uniquement pris en charge pour les clients exécutant Windows 10, version 1607 ou ultérieure. L’application parente doit être compilée à l’aide du kit de développement logiciel (SDK) Windows 10 version 14393 ou ultérieure, et elle doit utiliser l’API d’achat dans l’application dans l’espace de noms **Windows. services. Store** au lieu de l’espace de noms **Windows. ApplicationModel. Store** . Pour plus d’informations, consultez [activer les modules complémentaires d’abonnement pour votre application](../monetize/enable-subscription-add-ons-for-your-app.md).

Vous devez soumettre le produit parent avant de pouvoir publier des modules complémentaires d’abonnement dans le magasin (vous pouvez néanmoins créer l’envoi dans l’espace partenaires et commencer à l’utiliser à tout moment).

## <a name="product-id"></a>Product ID

Quel que soit le type de produit que vous choisissez, vous devrez entrer un ID de produit unique pour votre module complémentaire. Ce nom sera utilisé pour identifier votre module complémentaire dans l’espace partenaires. vous pouvez utiliser cet identificateur pour [faire référence au module complémentaire de votre code](../monetize/in-app-purchases-and-trials.md#how-to-use-product-ids-for-add-ons-in-your-code).

Voici quelques éléments à prendre en considération lors du choix d’un ID produit :

-   Un ID de produit doit être unique dans le produit parent.
-   Vous ne pouvez plus modifier ni supprimer l’ID produit d’un module complémentaire après la publication de ce dernier.
-   Un ID produit ne peut pas comporter plus de 100 caractères.
-   Un ID de produit ne peut pas inclure les caractères suivants : ** &lt; &gt; \* % & : \\ ? +,**
-   Les clients ne verront pas l’ID du produit. (Par la suite, vous pourrez entrer un [titre et une description](./create-app-store-listings.md) qui seront visibles par les clients.)
-   Si votre application précédemment publiée prend en charge Windows Phone 8,1 ou une version antérieure, vous devez utiliser uniquement des caractères alphanumériques, des points et/ou des traits de soulignement dans votre ID de produit. Si vous utilisez d’autres types de caractère, le module complémentaire ne sera pas disponible à l’achat pour les clients utilisant Windows Phone 8.1 ou une version antérieure.

 