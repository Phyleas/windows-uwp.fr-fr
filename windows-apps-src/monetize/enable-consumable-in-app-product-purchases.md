---
Description: Proposez des produits dans l’application&\# 8212 ; qui peuvent être achetés, utilisés et achetés à nouveau&\# 8212 ; via la plate-forme commerciale du Store, afin de fournir à vos clients une expérience d’achat fiable et fiable.
title: Activer les achats de produits consommables in-app
ms.assetid: F79EE369-ACFC-4156-AF6A-72D1C7D3BDA4
keywords: UWP, consommable, modules complémentaires, achats dans l’application, IAPs, Windows. ApplicationModel. Store
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7fd4bc4d21a5f292cd50655c452522e07424f920
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172923"
---
# <a name="enable-consumable-in-app-product-purchases"></a>Activer les achats de produits consommables in-app

Proposez des produits consommables dans l’application qui peuvent être achetés, utilisés et rachetés via la plateforme commerciale du Windows Store, afin d’offrir à vos clients une expérience d’achat à la fois solide et fiable au sein de l’application. Cette fonction est particulièrement utile pour différents aspects du jeu, comme les devises (or, pièces, etc.) susceptibles d’être achetées, puis utilisées pour acheter certaines améliorations.

> [!IMPORTANT]
> Cet article montre comment utiliser les membres de l’espace de noms [Windows. ApplicationModel. Store](/uwp/api/windows.applicationmodel.store) pour permettre les achats de produits consommables dans l’application. Cet espace de noms n’est plus mis à jour avec les nouvelles fonctionnalités et nous vous recommandons d’utiliser à la place l’espace de noms [Windows. services. Store](/uwp/api/windows.services.store) . L’espace de noms **Windows. services. Store** prend en charge les types de compléments les plus récents, tels que les modules complémentaires et les abonnements consommables gérés par le magasin, et est conçu pour être compatible avec les futurs types de produits et fonctionnalités pris en charge par l’espace partenaires et le Store. L’espace de noms **Windows. services. Store** a été introduit dans Windows 10, version 1607, et il ne peut être utilisé que dans les projets qui ciblent l' **édition anniversaire windows 10 (10,0 ; Build 14393)** ou une version ultérieure dans Visual Studio. Pour plus d’informations sur l’activation des achats de produits consommables dans l’application à l’aide de l’espace de noms **Windows. services. Store** , consultez [cet article](enable-consumable-add-on-purchases.md).

## <a name="prerequisites"></a>Prérequis

-   Cette rubrique porte sur les rapports relatifs aux achats et acquisitions de produits in-app consommables. Si vous ne connaissez pas les produits in-app, consultez [Activer les achats de produits in-app](enable-in-app-product-purchases.md) pour en savoir plus sur les licences et obtenir la liste des produits in-app dans le Windows Store.
-   Lorsque vous codez et testez de nouveaux produits in-app pour la première fois, vous devez utiliser l’objet [CurrentAppSimulator](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) au lieu de l’objet [CurrentApp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp). Cela vous permet de vérifier votre logique de licence à l’aide d’appels simulés au serveur de licences au lieu d’appels au serveur Windows Live. Pour ce faire, vous devez personnaliser le fichier nommé WindowsStoreProxy.xml dans le dossier% UserProfile% \\ AppData \\ local \\ packages \\ &lt; name &gt; \\ LocalState \\ Microsoft \\ Windows Store \\ ApiData. Le simulateur Microsoft Visual Studio crée ce fichier quand vous exécutez votre application pour la première fois. Vous pouvez également charger un fichier personnalisé au moment de l’exécution. Pour plus d’informations, consultez [Utilisation du fichier WindowsStoreProxy.xml avec CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy).
-   Cette rubrique fait également référence à des exemples de code fournis dans [Exemple Windows Store](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store). Cet exemple représente un excellent moyen d’obtenir une expérience pratique avec les différentes options de monétisation fournies pour les applications UWP.

## <a name="step-1-making-the-purchase-request"></a>Étape 1 : Établissement de la demande d’achat

La demande d’achat initiale est effectuée avec le paramètre [RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync), comme tout autre achat effectué dans le Windows Store. En ce qui concerne les produits in-app, la différence réside dans le fait qu’après avoir effectué un achat, un client ne peut pas acheter le même produit tant que l’application n’a pas averti le Windows Store que l’achat précédent a été correctement effectué. Il revient à votre application d’acquérir les consommables achetés et d’avertir le Windows Store de l’opération.

L’exemple suivant représente une demande d’achat de produits consommables dans l’application. Vous noterez la présence de commentaires de code indiquant le moment où votre application doit effectuer l’acquisition locale du produit in-app consommable, selon deux scénarios différents : lorsque la demande aboutit ou lorsqu’elle échoue suite à un achat non finalisé du même produit.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#MakePurchaseRequest)]

## <a name="step-2-tracking-local-fulfillment-of-the-consumable"></a>Étape 2 : Suivi de l’acquisition locale du consommable

Quand vous accordez à votre client un accès au produit in-app consommable, il est important de garder une trace du produit acquis (*productId*) et de chaque transaction associée à cette acquisition (*transactionId*).

> [!IMPORTANT]
> Votre application est chargée de signaler avec précision la satisfaction au magasin. Cette étape est essentielle pour assurer une expérience d’achat fiable et juste à vos clients.

L’exemple suivant illustre l’utilisation des propriétés [PurchaseResults](/uwp/api/Windows.ApplicationModel.Store.PurchaseResults) de l’appel [RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) à l’étape précédente, pour identifier le produit acheté à acquérir. Un tableau est utilisé pour stocker les informations du produit à un emplacement référençable ultérieurement et permettant de confirmer que l’acquisition locale a abouti.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#GrantFeatureLocally)]

L’exemple qui suit montre comment utiliser le tableau de l’exemple précédent pour accéder à l’ID du produit et à l’ID de transaction, deux informations qui sont utilisées plus tard pour signaler la finalisation de l’opération au Windows Store.

> [!IMPORTANT]
> Quelle que soit la méthodologie utilisée par votre application pour suivre et confirmer l’accomplissement, votre application doit faire preuve de diligence pour s’assurer que vos clients ne sont pas facturés pour les éléments qu’ils n’ont pas reçus.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#IsLocallyFulfilled)]

## <a name="step-3-reporting-product-fulfillment-to-the-store"></a>Étape 3 : Signalement de l’acquisition de produits au Windows Store

Une fois l’acquisition locale effectuée, votre application doit passer un appel [ReportConsumableFulfillmentAsync](/uwp/api/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync) incluant l’élément *productId* et la transaction comprenant l’achat du produit.

> [!IMPORTANT]
> Si vous ne parvenez pas à signaler le produit dans l’application dans le magasin, l’utilisateur ne pourra plus acheter ce produit tant qu’il n’aura pas fait l’objet d’un paiement.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#ReportFulfillment)]

## <a name="step-4-identifying-unfulfilled-purchases"></a>Étape 4 : Identification des achats non acquis

Votre application peut utiliser la méthode [GetUnfulfilledConsumablesAsync](/uwp/api/windows.applicationmodel.store.currentapp.getunfulfilledconsumablesasync) pour rechercher les produits in-app consommables non acquis dans l’application et ce, à tout moment. Cette méthode doit être appelée régulièrement pour rechercher les consommables non acquis suite à des événements imprévus de l’application (interruption de la connectivité réseau, arrêt de l’application, etc.).

L’exemple suivant montre comment la méthode [GetUnfulfilledConsumablesAsync](/uwp/api/windows.applicationmodel.store.currentapp.getunfulfilledconsumablesasync) permet d’énumérer les consommables non acquis et comment votre application peut parcourir cette liste pour effectuer l’acquisition locale.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#GetUnfulfilledConsumables)]

## <a name="related-topics"></a>Rubriques connexes

* [Activer les achats de produits in-app](enable-in-app-product-purchases.md)
* [Exemple du Windows Store (montre des versions d’évaluation et des achats in-app)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [Windows.ApplicationModel.Store](/uwp/api/Windows.ApplicationModel.Store)
 

 