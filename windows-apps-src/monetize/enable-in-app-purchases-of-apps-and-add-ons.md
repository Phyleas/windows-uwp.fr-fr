---
ms.assetid: B356C442-998F-4B2C-B550-70070C5E4487
description: Découvrez comment utiliser l’espace de noms Windows.Services.Store pour acheter une application ou l’une de ses extensions.
title: Activer les achats in-app d’applications et de modules complémentaires
keywords: Windows 10, UWP, modules complémentaires, achats dans l’application, IAPs, Windows. services. Store
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3064a651715329a3602c0c3539394d2ce72f90a7
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362812"
---
# <a name="enable-in-app-purchases-of-apps-and-add-ons"></a>Activer les achats in-app d’applications et de modules complémentaires

Cet article montre comment utiliser des membres dans l’espace de noms [Windows. services. Store](/uwp/api/windows.services.store) pour demander l’achat de l’application actuelle ou de l’un de ses composants additionnels pour l’utilisateur. Par exemple, si l’utilisateur possède actuellement une version d’évaluation de l’application, vous pouvez utiliser ce processus pour acheter une licence complète pour cet utilisateur. Vous pouvez aussi utiliser ce processus pour acheter une extension, comme un nouveau niveau de jeu pour l’utilisateur.

Pour demander l’achat d’une application ou d’un module complémentaire, l’espace de noms [Windows. services. Store](/uwp/api/windows.services.store) fournit plusieurs méthodes différentes :
* Si vous connaissez l’[ID Windows Store](in-app-purchases-and-trials.md#store_ids) de l’application ou de l’extension, vous pouvez utiliser la méthode [RequestPurchaseAsync](/uwp/api/windows.services.store.storecontext.requestpurchaseasync) de la classe [StoreContext](/uwp/api/windows.services.store.storecontext).
* Si vous disposez déjà d’un [objet **StoreProduct**, **StoreSku**ou **StoreAvailability** ](in-app-purchases-and-trials.md#products-skus) qui représente l’application ou le module complémentaire, vous pouvez utiliser les méthodes **RequestPurchaseAsync** de ces objets. Pour obtenir des exemples de différentes façons de récupérer un **StoreProduct** dans votre code, consultez [obtenir des informations sur le produit pour les applications et les modules](get-product-info-for-apps-and-add-ons.md)complémentaires.

Chaque méthode présente une interface utilisateur d’achat standard à l’utilisateur et s’exécute de manière asynchrone une fois la transaction terminée. La méthode retourne un objet qui indique si la transaction a réussi.

> [!NOTE]
> L’espace de noms **Windows. services. Store** a été introduit dans Windows 10, version 1607, et il ne peut être utilisé que dans les projets qui ciblent l' **édition anniversaire windows 10 (10,0 ; Build 14393)** ou une version ultérieure dans Visual Studio. Si votre application cible une version antérieure de Windows 10, vous devez utiliser l’espace de noms [Windows.ApplicationModel.Store](/uwp/api/windows.applicationmodel.store) à la place de l’espace de noms **Windows.Services.Store**. Pour plus d’informations, consultez [cet article](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Prérequis

La configuration requise pour cet exemple est la suivante :
* Projet Visual Studio pour une application plateforme Windows universelle (UWP) qui cible **Windows 10 édition anniversaire (10,0 ; Build 14393)** ou version ultérieure.
* Vous avez [créé une soumission d’application](../publish/app-submissions.md) dans l’espace partenaires et cette application est publiée dans le Windows Store. Vous pouvez éventuellement configurer l’application afin qu’elle ne soit pas détectable dans le magasin pendant que vous la Testez. Pour plus d’informations, consultez notre [Guide de test](in-app-purchases-and-trials.md#testing).
* Si vous souhaitez activer des achats dans l’application pour un module complémentaire pour l’application, vous devez également [créer le module complémentaire dans l’espace partenaires](../publish/add-on-submissions.md).

Le code de cet exemple se base sur les hypothèses suivantes :
* Le code s’exécute dans le contexte d’une [Page](/uwp/api/windows.ui.xaml.controls.page) qui contient un [ProgressRing](/uwp/api/windows.ui.xaml.controls.progressring) nommé ```workingProgressRing``` et un [TextBlock](/uwp/api/windows.ui.xaml.controls.textblock) nommé ```textBlock```. Ces objets sont utilisés pour respectivement indiquer qu’une opération asynchrone est en cours et afficher les messages de sortie.
* Le fichier de code contient une instruction **using** pour l’espace de noms **Windows.Services.Store**.
* Cette application mono-utilisateur ne s’exécute que dans le contexte de l’utilisateur qui l’a lancée. Pour plus d’informations, consultez [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Si vous disposez d’une application de bureau qui utilise le [pont Desktop](https://developer.microsoft.com/windows/bridges/desktop), vous devrez peut-être ajouter du code supplémentaire non présenté dans cet exemple pour configurer l’objet [StoreContext](/uwp/api/windows.services.store.storecontext) . Pour plus d’informations, voir [Utilisation de la classe StoreContext dans une application de bureau qui utilise Desktop Bridge](in-app-purchases-and-trials.md#desktop).

## <a name="code-example"></a>Exemple de code

Cet exemple montre comment utiliser la méthode [RequestPurchaseAsync](/uwp/api/windows.services.store.storecontext.requestpurchaseasync) de la classe [StoreContext](/uwp/api/windows.services.store.storecontext) pour acheter une application ou une extension avec un [ID Windows Store](in-app-purchases-and-trials.md#store-ids) connu. Pour obtenir un exemple d’application complète, consultez [Exemple Windows Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/cs/PurchaseAddOnPage.xaml.cs" id="PurchaseAddOn":::

## <a name="video"></a>Vidéo

Regardez la vidéo suivante pour obtenir une vue d’ensemble de l’implémentation des achats dans l’application dans votre application.
<br/>
<br/>
> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Adding-In-App-Purchases-to-Your-UWP-App/player]

## <a name="related-topics"></a>Rubriques connexes

* [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md)
* [Obtenir les informations produit des applications et des extensions](get-product-info-for-apps-and-add-ons.md)
* [Obtenir les informations de licence des applications et des modules complémentaires](get-license-info-for-apps-and-add-ons.md)
* [Activer les achats de modules complémentaires consommables](enable-consumable-add-on-purchases.md)
* [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md)
* [Exemple Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
