---
ms.assetid: FD381669-F962-465E-940B-AED9C8D19C90
description: Découvrez comment utiliser l’espace de noms Windows.Services.Store pour utiliser les modules complémentaires consommables.
title: Activer les achats de modules complémentaires consommables
keywords: Windows 10, UWP, consommable, modules complémentaires, achats dans l’application, IAPs, Windows. services. Store
ms.date: 05/09/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4f09b9a5c1f53c6a33f830c72514e061dc893348
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171583"
---
# <a name="enable-consumable-add-on-purchases"></a>Activer les achats de modules complémentaires consommables

Cet article montre comment utiliser les méthodes de la classe [StoreContext](/uwp/api/windows.services.store.storecontext) dans l’espace de noms [Windows. services. Store](/uwp/api/windows.services.store) pour gérer l’accomplissement de l’utilisateur des modules complémentaires consommables dans vos applications UWP. Utilisez les modules complémentaires consommables pour les éléments qu’il est possible d’acheter, d’utiliser et d’acheter à nouveau. Cette fonction est particulièrement utile pour différents aspects du jeu, comme les devises (or, pièces, etc.) susceptibles d’être achetées, puis utilisées pour acheter certaines améliorations.

> [!NOTE]
> L’espace de noms **Windows. services. Store** a été introduit dans Windows 10, version 1607, et il ne peut être utilisé que dans les projets qui ciblent l' **édition anniversaire windows 10 (10,0 ; Build 14393)** ou une version ultérieure dans Visual Studio. Si votre application cible une version antérieure de Windows 10, vous devez utiliser l’espace de noms [Windows.ApplicationModel.Store](/uwp/api/windows.applicationmodel.store) à la place de l’espace de noms **Windows.Services.Store**. Pour plus d’informations, consultez [cet article](enable-consumable-in-app-product-purchases.md).

## <a name="overview-of-consumable-add-ons"></a>Présentation des modules complémentaires consommables

Les applications peuvent offrir deux types de modules complémentaires consommables qui diffèrent selon la façon dont les opérations sont gérées :

* **Consommable géré par le développeur**. Pour ce type de consommable, vous devez gérer le solde d’éléments de l’utilisateur, qui représente le module complémentaire, et signaler l’achat du module complémentaire comme épuisé dans le Windows Store une fois que l’utilisateur a consommé tous les éléments. L’utilisateur ne peut pas acheter à nouveau le module complémentaire, tant que votre application n’a pas signalé le module complémentaire précédent comme épuisé.

  Par exemple, si votre module complémentaire représente 100 pièces dans un jeu et que l’utilisateur en consomme 10, votre application ou votre service doit maintenir le nouveau solde restant de 90 pièces pour l’utilisateur. Une fois que l’utilisateur a consommé les 100 pièces, votre application doit signaler le module complémentaire comme épuisé. Ensuite, l’utilisateur peut racheter le module complémentaire de 100 pièces.

* **Consommable géré par le magasin**. Pour ce type de consommable, le Windows Store gère le solde d’éléments de l’utilisateur, que le module complémentaire représente. Lorsque l’utilisateur utilise des éléments, vous devez les déclarer comme consommés dans le Store qui met à jour le solde de l’utilisateur. L’utilisateur peut acheter le module complémentaire autant de fois qu’il le souhaite (il n’a pas besoin d’utiliser les éléments en premier). Votre application peut interroger le magasin pour obtenir le solde actuel de l’utilisateur à tout moment.

  Par exemple, si votre module complémentaire représente une quantité initiale de 100 pièces dans un jeu et que l’utilisateur consomme 50 pièces, votre application signale au magasin que 50 unités du module complémentaire ont été remplies et le magasin met à jour le solde restant. Si l’utilisateur achète ensuite votre module complémentaire pour acquérir 100 de pièces supplémentaires, il dispose désormais de 150 pièces.
    > [!NOTE]
    > Les consommables gérés par le Store ont été introduits dans Windows 10, version 1607.

Pour offrir un module complémentaire consommable à un utilisateur, suivez cette procédure générale :

1. Autorisez les utilisateurs à [acheter le module complémentaire](enable-in-app-purchases-of-apps-and-add-ons.md) à partir de votre application.
3. Lorsque l’utilisateur consomme le module complémentaire (par exemple, en dépensant des pièces dans un jeu), [signalez le module comme consommé](enable-consumable-add-on-purchases.md#report_fulfilled).

À tout moment, vous pouvez également [consulter le solde restant](enable-consumable-add-on-purchases.md#get_balance) d’un consommable géré par le Windows Store.

## <a name="prerequisites"></a>Prérequis

Les conditions préalables de ces exemples sont les suivantes :
* Projet Visual Studio pour une application plateforme Windows universelle (UWP) qui cible **Windows 10 édition anniversaire (10,0 ; Build 14393)** ou version ultérieure.
* Vous avez [créé une soumission d’application](../publish/app-submissions.md) dans l’espace partenaires et cette application est publiée dans le Windows Store. Vous pouvez éventuellement configurer l’application afin qu’elle ne soit pas détectable dans le magasin pendant que vous la Testez. Pour plus d’informations, consultez notre [Guide de test](in-app-purchases-and-trials.md#testing).
* Vous avez [créé un module complémentaire utilisable pour l’application dans l'](../publish/add-on-submissions.md) espace partenaires.

Le code de ces exemples respecte les présupposés suivants :
* Le code s’exécute dans le contexte d’une [Page](/uwp/api/windows.ui.xaml.controls.page) qui contient un [ProgressRing](/uwp/api/windows.ui.xaml.controls.progressring) nommé ```workingProgressRing``` et un [TextBlock](/uwp/api/windows.ui.xaml.controls.textblock) nommé ```textBlock```. Ces objets sont utilisés pour respectivement indiquer qu’une opération asynchrone est en cours et afficher les messages de sortie.
* Le fichier de code contient une instruction **using** pour l’espace de noms **Windows.Services.Store**.
* Cette application mono-utilisateur ne s’exécute que dans le contexte de l’utilisateur qui l’a lancée. Pour plus d’informations, consultez [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md#api_intro).

Pour obtenir un exemple d’application complète, consultez [Exemple Windows Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

> [!NOTE]
> Si vous disposez d’une application de bureau qui utilise le [pont Desktop](https://developer.microsoft.com/windows/bridges/desktop), vous devrez peut-être ajouter du code supplémentaire non présenté dans ces exemples pour configurer l’objet [StoreContext](/uwp/api/windows.services.store.storecontext) . Pour plus d’informations, voir [Utilisation de la classe StoreContext dans une application de bureau qui utilise Desktop Bridge](in-app-purchases-and-trials.md#desktop).

<span id="report_fulfilled" />

## <a name="report-a-consumable-add-on-as-fulfilled"></a>Signaler un composant additionnel consommable comme épuisé

Lorsque l’utilisateur [achète le module complémentaire](enable-in-app-purchases-of-apps-and-add-ons.md) à votre application et qu’il le consomme, votre application doit le signaler comme épuisé en appelant la méthode [ReportConsumableFulfillmentAsync](/uwp/api/windows.services.store.storecontext.reportconsumablefulfillmentasync) de la classe [StoreContext](/uwp/api/windows.services.store.storecontext). Vous devez transmettre les informations suivantes à cette méthode :

* L’[ID Windows Store](in-app-purchases-and-trials.md#store-ids) du module complémentaire que vous souhaitez signaler comme consommé.
* Les unités du module complémentaire que vous souhaitez signaler comme épuisées.
  * Pour un consommable géré par le développeur, attribuez la valeur 1 au paramètre de *quantité*. Ceci signale au Windows Store que le consommable est épuisé et que le client peut l’acheter à nouveau. L’utilisateur ne peut pas acheter le consommable à nouveau tant que votre application n’a pas notifié au Windows Store qu’il a été épuisé.
  * Pour un consommable géré par le Windows Store, indiquez le nombre réel d’unités consommées. Le Windows Store met à jour le solde restant du consommable.
* L’ID de suivi de l’acquisition. Il s’agit d’un GUID fourni par le développeur, qui identifie la transaction spécifique à laquelle l’opération d’acquisition est associée, à des fins de suivi. Pour plus d’informations, consultez les remarques dans [ReportConsumableFulfillmentAsync](/uwp/api/windows.services.store.storecontext.reportconsumablefulfillmentasync).

Cet exemple montre comment signaler un consommable géré par le Windows Store comme épuisé.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[EnableConsumables](./code/InAppPurchasesAndLicenses_RS1/cs/ConsumeAddOnPage.xaml.cs#ConsumeAddOn)]

<span id="get_balance" />

## <a name="get-the-remaining-balance-for-a-store-managed-consumable"></a>Obtenir le solde restant d’un consommable géré par le Windows Store

Cet exemple montre comment utiliser la méthode [GetConsumableBalanceRemainingAsync](/uwp/api/windows.services.store.storecontext.getconsumablebalanceremainingasync) de la classe [StoreContext](/uwp/api/windows.services.store.storecontext) pour obtenir le solde restant d’un consommable géré par le Windows Store.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[EnableConsumables](./code/InAppPurchasesAndLicenses_RS1/cs/GetRemainingAddOnBalancePage.xaml.cs#GetRemainingAddOnBalance)]

## <a name="related-topics"></a>Rubriques connexes

* [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md)
* [Obtenir les informations produit des applications et des extensions](get-product-info-for-apps-and-add-ons.md)
* [Obtenir les informations de licence des applications et des modules complémentaires](get-license-info-for-apps-and-add-ons.md)
* [Activer les achats in-app d’applications et de modules complémentaires](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md)
* [Exemple Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)