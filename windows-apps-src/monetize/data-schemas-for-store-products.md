---
description: Décrit le schéma de données JSON étendu pour les produits du Windows Store dans l’espace de noms Windows. services. Store.
title: Schémas de données des produits du Store
ms.date: 09/26/2017
ms.topic: article
keywords: Windows 10, UWP, ExtendedJsonData, produits du Store, schéma
ms.localizationpriority: medium
ms.openlocfilehash: 46feac06745cd875aaf99985d45ea1b5b126b540
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175093"
---
# <a name="data-schemas-for-store-products"></a>Schémas de données des produits du Store

Lorsque vous soumettez un produit tel qu’une application ou un module complémentaire au Store, le magasin conserve un ensemble complet de données pour le produit et ses licences. Dans le code de votre application, vous pouvez accéder par programme à certaines de ces données à l’aide des propriétés de l’espace de noms [Windows. services. Store](/uwp/api/windows.services.store) . Par exemple, vous pouvez récupérer la description et le prix de l’application actuelle ou d’un module complémentaire pour l’application actuelle à l’aide des propriétés [StoreProduct. Description](/uwp/api/windows.services.store.storeproduct.Description) et [StoreProduct. Price](/uwp/api/windows.services.store.storeproduct.Price) .

Toutefois, une grande partie des données des produits du magasin ne sont pas exposées par des propriétés prédéfinies dans l’espace de noms [Windows. services. Store](/uwp/api/windows.services.store) . Pour accéder aux données complètes d’un produit dans votre code, vous pouvez utiliser les propriétés générales suivantes à la place :

* [StoreProduct.ExtendedJsonData](/uwp/api/windows.services.store.storeproduct.ExtendedJsonData)
* [StoreSku.ExtendedJsonData](/uwp/api/windows.services.store.storesku.ExtendedJsonData)
* [StoreAvailability.ExtendedJsonData](/uwp/api/windows.services.store.storeavailability.ExtendedJsonData)
*   [StoreCollectionData.ExtendedJsonData](/uwp/api/windows.services.store.storecollectiondata.ExtendedJsonData)
*   [StoreAppLicense.ExtendedJsonData](/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData)
* [StoreLicense.ExtendedJsonData](/uwp/api/windows.services.store.storelicense.ExtendedJsonData)
*   [StorePurchaseProperties.ExtendedJsonData](/uwp/api/windows.services.store.storepurchaseproperties.ExtendedJsonData)

Ces propriétés retournent les données complètes de l’objet correspondant sous la forme d’une chaîne au format JSON. Cet article fournit le schéma complet pour les données retournées par ces propriétés.

> [!NOTE]
> Les produits du Store sont organisés dans une hiérarchie d’objets de [produit](/uwp/api/windows.services.store.storeproduct), de [référence SKU](/uwp/api/windows.services.store.storesku)et de [disponibilité](/uwp/api/windows.services.store.storeavailability) . Pour plus d’informations, consultez [Products, SKU et availabilities](in-app-purchases-and-trials.md#products-skus).

## <a name="schema-for-storeproduct-storesku-storeavailability-and-storecollectiondata"></a>Schéma pour StoreProduct, StoreSku, StoreAvailability et StoreCollectionData

Le schéma suivant décrit la chaîne au format JSON retournée par [StoreProduct. ExtendedJsonData](/uwp/api/windows.services.store.storeproduct.ExtendedJsonData). Les propriétés [StoreSku. ExtendedJsonData](/uwp/api/windows.services.store.storesku.ExtendedJsonData), [StoreAvailability. ExtendedJsonData](/uwp/api/windows.services.store.storeavailability.ExtendedJsonData)et [StoreCollectionData. ExtendedJsonData](/uwp/api/windows.services.store.storecollectiondata.ExtendedJsonData) retournent uniquement les parties du schéma définies sous les `DisplaySkuAvailabilities\Sku` `DisplaySkuAvailabilities\Availabilities` chemins, et `DisplaySkuAvailabilities\Sku\CollectionData` , respectivement.

Pour obtenir un exemple de chaîne au format JSON retournée par [StoreProduct. ExtendedJsonData](/uwp/api/windows.services.store.storeproduct.ExtendedJsonData), consultez [cette section](#product-example).

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreProduct.ExtendedJsonData.json#L1-L729)]

<span id="product-example" />

### <a name="example"></a>Exemple

L’exemple suivant illustre une chaîne au format JSON retournée par la propriété [StoreProduct. ExtendedJsonData](/uwp/api/windows.services.store.storeproduct.ExtendedJsonData) pour l’application.

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreProduct.ExtendedJsonDataExample.json#L1-L268)]

## <a name="schema-for-storeapplicense-and-storelicense"></a>Schéma pour StoreAppLicense et StoreLicense

Le schéma suivant décrit la chaîne au format JSON retournée par [StoreAppLicense. ExtendedJsonData](/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData). La propriété [StoreLicense. ExtendedJsonData](/uwp/api/windows.services.store.storelicense.ExtendedJsonData) retourne uniquement les parties du schéma qui sont définies sous le `productAddOns` chemin d’accès.

Pour obtenir un exemple de chaîne au format JSON retournée par [StoreAppLicense. ExtendedJsonData](/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData), consultez [cette section](#license-example).

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreAppLicense.ExtendedJsonData.json#L1-L80)]

<span id="license-example" />

### <a name="example"></a>Exemple

L’exemple suivant illustre une chaîne au format JSON retournée par la propriété [StoreAppLicense. ExtendedJsonData](/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData) pour l’application.

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreAppLicense.ExtendedJsonDataExample.json#L1-L28)]

## <a name="schema-for-storepurchaseproperties"></a>Schéma pour StorePurchaseProperties

Le schéma suivant décrit la chaîne au format JSON retournée par [StorePurchaseProperties. ExtendedJsonData](/uwp/api/windows.services.store.storepurchaseproperties.ExtendedJsonData).

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StorePurchaseProperties.ExtendedJsonData.json#L1-L12)]

## <a name="related-topics"></a>Rubriques connexes

* [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md)
* [Obtenir les informations produit des applications et des extensions](get-product-info-for-apps-and-add-ons.md)
* [Obtenir les informations de licence des applications et des modules complémentaires](get-license-info-for-apps-and-add-ons.md)
* [Activer les achats in-app d’applications et de modules complémentaires](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Activer les achats de modules complémentaires consommables](enable-consumable-add-on-purchases.md)
* [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md)