---
ms.assetid: 5E722AFF-539D-456E-8C4A-ADE90CF7674A
description: Si votre application propose un vaste catalogue de produits in-app, vous pouvez éventuellement suivre la procédure décrite dans cette rubrique pour faciliter la gestion de votre catalogue.
title: Gérer un vaste catalogue de produits in-app
ms.date: 08/25/2017
ms.topic: article
keywords: Windows 10, UWP, achats dans l’application, IAPs, modules complémentaires, catalogue, Windows. ApplicationModel. Store
ms.localizationpriority: medium
ms.openlocfilehash: a6bd4d95365e33ee30df87247b3aec72f70fc5b1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158423"
---
# <a name="manage-a-large-catalog-of-in-app-products"></a>Gérer un vaste catalogue de produits in-app

Si votre application propose un vaste catalogue de produits in-app, vous pouvez éventuellement suivre la procédure décrite dans cette rubrique pour faciliter la gestion de votre catalogue. Avant Windows 10, le Windows Store avait une limite de 200 produits par compte de développeur et la procédure décrite dans cette rubrique permettait de contourner cette limite. Depuis Windows 10, le Windows Store n’a aucune limite quant au nombre de produits listés par compte de développeur. La procédure décrite dans cette rubrique n’est donc plus nécessaire.

> [!IMPORTANT]
> Cet article montre comment utiliser les membres de l’espace de noms [Windows. ApplicationModel. Store](/uwp/api/windows.applicationmodel.store) . Cet espace de noms n’est plus mis à jour avec les nouvelles fonctionnalités et nous vous recommandons d’utiliser à la place l’espace de noms [Windows. services. Store](/uwp/api/windows.services.store) . L’espace de noms **Windows. services. Store** prend en charge les types de compléments les plus récents, tels que les modules complémentaires et les abonnements consommables gérés par le magasin, et est conçu pour être compatible avec les futurs types de produits et fonctionnalités pris en charge par l’espace partenaires et le Store. L’espace de noms **Windows. services. Store** a été introduit dans Windows 10, version 1607, et il ne peut être utilisé que dans les projets qui ciblent l' **édition anniversaire windows 10 (10,0 ; Build 14393)** ou une version ultérieure dans Visual Studio. Pour plus d’informations, consultez [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md).

Pour activer cette fonctionnalité, vous allez créer plusieurs entrées pour certaines fourchettes de prix, chacune d’elles pouvant représenter des centaines de produits dans un catalogue. Utilisez la surcharge de la méthode [RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) qui spécifie une offre définie par une application et associée à un produit in-app répertorié dans le Windows Store. En plus de spécifier une association entre une offre et un produit pendant l’appel, votre application doit transférer un objet [ProductPurchaseDisplayProperties](/uwp/api/Windows.ApplicationModel.Store.ProductPurchaseDisplayProperties) qui contient les détails de l’offre du grand catalogue. Si ces informations ne sont pas fournies, elles sont remplacées par celles du produit listé.

Le Windows Store n’utilise que le paramètre *offerId* de la demande d’achat dans les résultats [PurchaseResults](/uwp/api/Windows.ApplicationModel.Store.PurchaseResults). Ce processus ne modifie pas directement les informations fournies à l’origine lors de l’[intégration du produit in-app dans le Windows Store](../publish/add-on-submissions.md).

## <a name="prerequisites"></a>Prérequis

-   Cette rubrique couvre la prise en charge par le Windows Store de la représentation de plusieurs offres in-app à l’aide d’un simple produit in-app listé dans le Windows Store. Si vous ne connaissez pas les achats in-app, consultez [Activer les achats de produits in-app](enable-in-app-product-purchases.md) pour en savoir plus sur les informations de licence et pour répertorier correctement votre achat in-app dans le Windows Store.
-   Lorsque vous codez et testez de nouvelles offres in-app pour la première fois, vous devez utiliser l’objet [CurrentAppSimulator](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) au lieu de l’objet [CurrentApp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp). Cela vous permet de vérifier votre logique de licence à l’aide d’appels simulés au serveur de licences au lieu d’appels au serveur Windows Live. Pour ce faire, vous devez personnaliser le fichier nommé WindowsStoreProxy.xml dans le dossier% UserProfile% \\ AppData \\ local \\ packages \\ &lt; name &gt; \\ LocalState \\ Microsoft \\ Windows Store \\ ApiData. Le simulateur Microsoft Visual Studio crée ce fichier quand vous exécutez votre application pour la première fois. Vous pouvez également charger un fichier personnalisé au moment de l’exécution. Pour plus d’informations, consultez [Utilisation du fichier WindowsStoreProxy.xml avec CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy).
-   Cette rubrique fait également référence à des exemples de code fournis dans [Exemple Windows Store](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store). Cet exemple représente un excellent moyen d’obtenir une expérience pratique avec les différentes options de monétisation fournies pour les applications UWP.

## <a name="make-the-purchase-request-for-the-in-app-product"></a>Effectuer une demande d’achat d’un produit in-app

La demande d’achat d’un produit spécifique dans un vaste catalogue est traitée de la même manière que n’importe quelle autre demande d’achat dans l’application. Lorsque votre application appelle la nouvelle surcharge de méthode [RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync), votre application fournit à la fois un élément *OfferId* et un objet [ProductPurchaseDisplayProperties](/uwp/api/windows.applicationmodel.store.productpurchasedisplayproperties) contenant le nom du produit in-app.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[ManageCatalog](./code/InAppPurchasesAndLicenses/cs/ManageCatalog.cs#MakePurchaseRequest)]

## <a name="report-fulfillment-of-the-in-app-offer"></a>Signaler l’acquisition de l’offre in-app

Votre application devra signaler l’acquisition du produit au Store dès que l’offre aura été acquise localement. Dans le cas d’un grand catalogue, si votre application ne signale pas l’acquisition de l’offre, l’utilisateur ne pourra pas acheter d’offres dans l’application à l’aide de la même liste de produits du Windows Store.

Comme mentionné précédemment, le Windows Store n’utilise que les informations de l’offre pour renseigner l’élément [PurchaseResults](/uwp/api/Windows.ApplicationModel.Store.PurchaseResults), sans créer d’association durable entre une offre d’un vaste catalogue et la liste de produits du Windows Store. Par conséquent, vous devez vérifier que les utilisateurs sont autorisés à accéder aux produits et fournir un contexte spécifique (comme le nom de l’article acheté ou des détails le concernant) à l’utilisateur hors de l’opération [RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync).

Le code suivant illustre l’appel d’acquisition et un schéma de message d’interface utilisateur contenant les informations de l’offre. En l’absence de ces informations sur ce produit, l’exemple utilise les informations du produit [ListingInformation](/uwp/api/Windows.ApplicationModel.Store.ListingInformation).

> [!div class="tabbedCodeSnippets"]
[!code-csharp[ManageCatalog](./code/InAppPurchasesAndLicenses/cs/ManageCatalog.cs#ReportFulfillment)]

## <a name="related-topics"></a>Rubriques connexes

* [Activer les achats de produits in-app](enable-in-app-product-purchases.md)
* [Activer les achats de produits consommables in-app](enable-consumable-in-app-product-purchases.md)
* [Exemple du Windows Store (montre des versions d’évaluation et des achats in-app)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync)
* [ProductPurchaseDisplayProperties](/uwp/api/Windows.ApplicationModel.Store.ProductPurchaseDisplayProperties)