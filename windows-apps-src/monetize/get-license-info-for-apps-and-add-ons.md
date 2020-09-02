---
ms.assetid: 9630AF6D-6887-4BE3-A3CB-D058F275B58F
description: Découvrez comment utiliser l’espace de noms Windows.Services.Store pour obtenir les informations de licence de l’application active et de ses modules complémentaires.
title: Obtenir les informations de licence de votre application et des extensions
ms.date: 12/04/2017
ms.topic: article
keywords: Windows 10, UWP, licences, applications, modules complémentaires, achats dans l’application, IAPs, Windows. services. Store
ms.localizationpriority: medium
ms.openlocfilehash: 29f2e5ceee6d7d9c779c7fb544d507e31456b3fd
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363162"
---
# <a name="get-license-info-for-apps-and-add-ons"></a>Obtenir les informations de licence des applications et des modules complémentaires

Cet article montre comment utiliser les méthodes de la classe [StoreContext](/uwp/api/windows.services.store.storecontext) dans l’espace de noms [Windows. services. Store](/uwp/api/windows.services.store) pour obtenir des informations de licence pour l’application actuelle et ses modules complémentaires. Par exemple, vous pouvez utiliser ces informations pour déterminer si les licences de l’application ou de ses modules complémentaires sont actives ou s’il s’agit de licences d’évaluation.

> [!NOTE]
> L’espace de noms **Windows. services. Store** a été introduit dans Windows 10, version 1607, et il ne peut être utilisé que dans les projets qui ciblent l' **édition anniversaire windows 10 (10,0 ; Build 14393)** ou une version ultérieure dans Visual Studio. Si votre application cible une version antérieure de Windows 10, vous devez utiliser l’espace de noms [Windows.ApplicationModel.Store](/uwp/api/windows.applicationmodel.store) à la place de l’espace de noms **Windows.Services.Store**. Pour plus d’informations, consultez [cet article](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Prérequis

La configuration requise pour cet exemple est la suivante :
* Projet Visual Studio pour une application plateforme Windows universelle (UWP) qui cible **Windows 10 édition anniversaire (10,0 ; Build 14393)** ou version ultérieure.
* Vous avez [créé une soumission d’application](../publish/app-submissions.md) dans l’espace partenaires et cette application est publiée dans le Windows Store. Vous pouvez éventuellement configurer l’application afin qu’elle ne soit pas détectable dans le magasin pendant que vous la Testez. Pour plus d’informations, consultez notre [Guide de test](in-app-purchases-and-trials.md#testing).
* Si vous souhaitez obtenir les informations de licence d’un module complémentaire pour l’application, vous devez également [créer le module complémentaire dans l’espace partenaires](../publish/add-on-submissions.md).

Le code de cet exemple se base sur les hypothèses suivantes :
* Le code s’exécute dans le contexte d’une [Page](/uwp/api/windows.ui.xaml.controls.page) qui contient un [ProgressRing](/uwp/api/windows.ui.xaml.controls.progressring) nommé ```workingProgressRing``` et un [TextBlock](/uwp/api/windows.ui.xaml.controls.textblock) nommé ```textBlock```. Ces objets sont utilisés pour respectivement indiquer qu’une opération asynchrone est en cours et afficher les messages de sortie.
* Le fichier de code contient une instruction **using** pour l’espace de noms **Windows.Services.Store**.
* Cette application mono-utilisateur ne s’exécute que dans le contexte de l’utilisateur qui l’a lancée. Pour plus d’informations, consultez [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Si vous disposez d’une application de bureau qui utilise le [pont Desktop](https://developer.microsoft.com/windows/bridges/desktop), vous devrez peut-être ajouter du code supplémentaire non présenté dans cet exemple pour configurer l’objet [StoreContext](/uwp/api/windows.services.store.storecontext) . Pour plus d’informations, voir [Utilisation de la classe StoreContext dans une application de bureau qui utilise Desktop Bridge](in-app-purchases-and-trials.md#desktop).

## <a name="code-example"></a>Exemple de code

Pour récupérer les informations de licence pour l’application actuelle, utilisez la méthode [GetAppLicenseAsync](/uwp/api/windows.services.store.storecontext.getapplicenseasync). Il s’agit d’une méthode asynchrone qui retourne un objet [StoreAppLicense](/uwp/api/windows.services.store.storeapplicense) qui fournit des informations de licence pour l’application, y compris des propriétés qui indiquent si l’utilisateur dispose actuellement d’une licence valide pour utiliser l’application ([IsActive](/uwp/api/windows.services.store.storeapplicense.isactive)) et si la licence est pour une version d’évaluation ([IsTrial](/uwp/api/windows.services.store.storeapplicense.istrial)).

Pour accéder aux licences des modules complémentaires durables de l’application actuelle pour laquelle l’utilisateur a le droit d’utiliser, utilisez la propriété [AddOnLicenses](/uwp/api/windows.services.store.storeapplicense.addonlicenses) de l’objet [StoreAppLicense](/uwp/api/windows.services.store.storeapplicense) . Cette propriété retourne une collection d’objets [StoreLicense](/uwp/api/windows.services.store.storelicense) qui représentent les licences du module complémentaire.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/cs/GetLicenseInfoPage.xaml.cs" id="GetLicenseInfo":::

Pour obtenir un exemple d’application complète, consultez [Exemple Windows Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

## <a name="related-topics"></a>Rubriques connexes

* [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md)
* [Obtenir les informations produit des applications et des extensions](get-product-info-for-apps-and-add-ons.md)
* [Activer les achats in-app d’applications et de modules complémentaires](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Activer les achats de modules complémentaires consommables](enable-consumable-add-on-purchases.md)
* [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md)
* [Exemple Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
