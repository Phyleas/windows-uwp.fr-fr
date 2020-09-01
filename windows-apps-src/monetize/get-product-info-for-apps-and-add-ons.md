---
ms.assetid: 89178FD9-850B-462F-9016-1AD86D1F6F7F
description: Découvrez comment utiliser l’espace de noms Windows.Services.Store pour obtenir les informations du Windows Store concernant l’application active ou l’un de ses modules complémentaires.
title: Obtenir les informations produit des applications et des extensions
ms.date: 02/08/2018
ms.topic: article
keywords: Windows 10, UWP, achats dans l’application, IAPs, compléments, Windows. services. Store
ms.localizationpriority: medium
ms.openlocfilehash: a46d9b0049e0dc9456a36c726a611cbaf00e9616
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164603"
---
# <a name="get-product-info-for-apps-and-add-ons"></a>Obtenir les informations produit des applications et des extensions

Cet article montre comment utiliser les méthodes de la classe [StoreContext](/uwp/api/windows.services.store.storecontext) dans l’espace de noms [Windows. services. Store](/uwp/api/windows.services.store) pour accéder aux informations relatives au magasin pour l’application actuelle ou l’un de ses modules complémentaires.

Pour obtenir un exemple d’application complète, consultez [Exemple Windows Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

> [!NOTE]
> L’espace de noms **Windows. services. Store** a été introduit dans Windows 10, version 1607, et il ne peut être utilisé que dans les projets qui ciblent l' **édition anniversaire windows 10 (10,0 ; Build 14393)** ou une version ultérieure dans Visual Studio. Si votre application cible une version antérieure de Windows 10, vous devez utiliser l’espace de noms [Windows.ApplicationModel.Store](/uwp/api/windows.applicationmodel.store) à la place de l’espace de noms **Windows.Services.Store**. Pour plus d’informations, consultez [cet article](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Prérequis

Les conditions préalables de ces exemples sont les suivantes :
* Projet Visual Studio pour une application plateforme Windows universelle (UWP) qui cible **Windows 10 édition anniversaire (10,0 ; Build 14393)** ou version ultérieure.
* Vous avez [créé une soumission d’application](../publish/app-submissions.md) dans l’espace partenaires et cette application est publiée dans le Windows Store. Vous pouvez éventuellement configurer l’application afin qu’elle ne soit pas détectable dans le magasin pendant que vous la Testez. Pour plus d’informations, consultez notre [Guide de test](in-app-purchases-and-trials.md#testing).
* Si vous souhaitez obtenir des informations sur le produit d’un module complémentaire pour l’application, vous devez également [créer le module complémentaire dans l’espace partenaires](../publish/add-on-submissions.md).

Le code de ces exemples respecte les présupposés suivants :
* Le code s’exécute dans le contexte d’une [Page](/uwp/api/windows.ui.xaml.controls.page) qui contient un [ProgressRing](/uwp/api/windows.ui.xaml.controls.progressring) nommé ```workingProgressRing``` et un [TextBlock](/uwp/api/windows.ui.xaml.controls.textblock) nommé ```textBlock```. Ces objets sont utilisés pour respectivement indiquer qu’une opération asynchrone est en cours et afficher les messages de sortie.
* Le fichier de code contient une instruction **using** pour l’espace de noms **Windows.Services.Store**.
* Cette application mono-utilisateur ne s’exécute que dans le contexte de l’utilisateur qui l’a lancée. Pour plus d’informations, consultez [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Si vous disposez d’une application de bureau qui utilise le [pont Desktop](https://developer.microsoft.com/windows/bridges/desktop), vous devrez peut-être ajouter du code supplémentaire non présenté dans ces exemples pour configurer l’objet [StoreContext](/uwp/api/windows.services.store.storecontext) . Pour plus d’informations, voir [Utilisation de la classe StoreContext dans une application de bureau qui utilise Desktop Bridge](in-app-purchases-and-trials.md#desktop).

## <a name="get-info-for-the-current-app"></a>Obtenir les informations de l’application en cours

Pour obtenir des informations du Windows Store concernant l’application actuelle, utilisez la méthode [GetStoreProductForCurrentAppAsync](/uwp/api/windows.services.store.storecontext.getstoreproductforcurrentappasync). Cette méthode asynchrone renvoie un objet [StoreProduct](/uwp/api/windows.services.store.storeproduct) que vous pouvez utiliser pour obtenir des informations telles que le prix.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetAppInfoPage.xaml.cs#GetAppInfo)]

## <a name="get-info-for-add-ons-with-known-store-ids-that-are-associated-with-the-current-app"></a>Obtenir des informations sur les modules complémentaires avec des ID de magasin connus associés à l’application actuelle

Pour obtenir les informations du produit du magasin pour les modules complémentaires associés à l’application actuelle et pour lesquels vous connaissez déjà les [ID de magasin](in-app-purchases-and-trials.md#store_ids), utilisez la méthode [GetStoreProductsAsync](/uwp/api/windows.services.store.storecontext.getstoreproductsasync) . Cette méthode asynchrone renvoie un ensemble d’objets [StoreProduct](/uwp/api/windows.services.store.storeproduct) qui représentent chaque module complémentaire. Outre les ID Windows Store, vous devez transmettre à cette méthode une liste de chaînes qui identifient les types de modules complémentaires. Pour obtenir la liste des valeurs de chaîne prises en charge, consultez la propriété [ProductKind](/uwp/api/windows.services.store.storeproduct.productkind).

> [!NOTE]
> La méthode **GetStoreProductsAsync** retourne des informations sur les produits pour les modules complémentaires spécifiés qui sont associés à l’application, que les modules complémentaires soient actuellement disponibles pour l’achat ou non. Pour récupérer des informations pour tous les modules complémentaires de l’application actuelle qui peuvent être achetés actuellement, utilisez la méthode **GetAssociatedStoreProductsAsync** comme décrit dans la [section suivante](#get-info-for-add-ons-that-are-available-for-purchase-from-the-current-app) à la place.

Cet exemple récupère des informations sur les modules complémentaires durables avec les ID de magasin spécifiés qui sont associés à l’application actuelle.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetProductInfoPage.xaml.cs#GetProductInfo)]

## <a name="get-info-for-add-ons-that-are-available-for-purchase-from-the-current-app"></a>Obtenir des informations sur les modules complémentaires qui peuvent être achetés à partir de l’application actuelle

Pour obtenir les informations du produit du magasin pour les modules complémentaires actuellement disponibles à l’achat à partir de l’application actuelle, utilisez la méthode [GetAssociatedStoreProductsAsync](/uwp/api/windows.services.store.storecontext.getassociatedstoreproductsasync) . Cette méthode asynchrone renvoie un ensemble d’objets [StoreProduct](/uwp/api/windows.services.store.storeproduct) qui représentent chaque module complémentaire disponible. Vous devez transmettre à cette méthode une liste de chaînes qui identifient les types de modules complémentaires que vous souhaitez récupérer. Pour obtenir la liste des valeurs de chaîne prises en charge, consultez la propriété [ProductKind](/uwp/api/windows.services.store.storeproduct.productkind).

> [!NOTE]
> Si l’application a de nombreux modules complémentaires disponibles à l’achat, vous pouvez également utiliser la méthode [GetAssociatedStoreProductsWithPagingAsync](/uwp/api/Windows.Services.Store.StoreContext.GetAssociatedStoreProductsWithPagingAsync) pour utiliser la pagination afin de retourner les résultats du module complémentaire.

L’exemple suivant récupère des informations pour tous les modules complémentaires durables, les modules complémentaires consommables gérés par le magasin et les modules complémentaires consommables gérés par le développeur qui peuvent être achetés à partir de l’application actuelle.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetAddOnInfoPage.xaml.cs#GetAddOnInfo)]


## <a name="get-info-for-add-ons-for-the-current-app-that-the-user-has-purchased"></a>Obtenir des informations sur les modules complémentaires pour l’application actuelle que l’utilisateur a achetée

Pour obtenir les informations du produit du magasin pour les modules complémentaires que l’utilisateur actuel a achetés, utilisez la méthode [GetUserCollectionAsync](/uwp/api/windows.services.store.storecontext.getusercollectionasync) . Il s’agit d’une méthode asynchrone qui retourne une collection d’objets  [StoreProduct](/uwp/api/windows.services.store.storeproduct) qui représentent chacun des modules complémentaires. Vous devez transmettre à cette méthode une liste de chaînes qui identifient les types de modules complémentaires que vous souhaitez récupérer. Pour obtenir la liste des valeurs de chaîne prises en charge, consultez la propriété [ProductKind](/uwp/api/windows.services.store.storeproduct.productkind).

> [!NOTE]
> Si l’application possède de nombreux modules complémentaires, vous pouvez également utiliser la méthode [GetUserCollectionWithPagingAsync](/uwp/api/windows.services.store.storecontext.getusercollectionwithpagingasync) pour utiliser la pagination afin de retourner les résultats du module complémentaire.

L’exemple suivant récupère des informations pour les modules complémentaires durables avec les [ID de magasin](in-app-purchases-and-trials.md#store_ids)spécifiés.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetUserCollectionPage.xaml.cs#GetUserCollection)]

## <a name="related-topics"></a>Rubriques connexes

* [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md)
* [Obtenir les informations de licence des applications et des modules complémentaires](get-license-info-for-apps-and-add-ons.md)
* [Activer les achats in-app d’applications et de modules complémentaires](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Activer les achats de modules complémentaires consommables](enable-consumable-add-on-purchases.md)
* [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md)
* [Exemple Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)