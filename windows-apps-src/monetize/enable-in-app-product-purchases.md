---
Description: Que votre application soit gratuite ou non, vous pouvez vendre du contenu, d’autres applications ou de nouvelles fonctionnalités applicatives (par exemple le déverrouillage d’un nouveau niveau de jeu) directement dans l’application. Nous allons vous montrer comment activer ces produits dans votre application.
title: Activer les achats de produits in-app
ms.assetid: D158E9EB-1907-4173-9889-66507957BD6B
keywords: UWP, modules complémentaires, achats dans l’application, IAPs, Windows. ApplicationModel. Store
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ab2b5937746f006c0f5efd296e9b4a4f3cb82696
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171573"
---
# <a name="enable-in-app-product-purchases"></a>Activer les achats de produits in-app

Que votre application soit gratuite ou non, vous pouvez vendre du contenu, d’autres applications ou de nouvelles fonctionnalités applicatives (par exemple le déverrouillage d’un nouveau niveau de jeu) directement dans l’application. Nous allons vous montrer comment activer ces produits dans votre application.

> [!IMPORTANT]
> Cet article explique comment utiliser les membres de l’espace de noms [Windows. ApplicationModel. Store](/uwp/api/windows.applicationmodel.store) pour activer les achats de produits dans l’application. Cet espace de noms n’est plus mis à jour avec les nouvelles fonctionnalités et nous vous recommandons d’utiliser à la place l’espace de noms [Windows. services. Store](/uwp/api/windows.services.store) . L’espace de noms **Windows. services. Store** prend en charge les types de compléments les plus récents, tels que les modules complémentaires et les abonnements consommables gérés par le magasin, et est conçu pour être compatible avec les futurs types de produits et fonctionnalités pris en charge par l’espace partenaires et le Store. L’espace de noms **Windows. services. Store** a été introduit dans Windows 10, version 1607, et il ne peut être utilisé que dans les projets qui ciblent l' **édition anniversaire windows 10 (10,0 ; Build 14393)** ou une version ultérieure dans Visual Studio. Pour plus d’informations sur l’activation des achats de produits dans l’application à l’aide de l’espace de noms **Windows. services. Store** , consultez [cet article](enable-in-app-purchases-of-apps-and-add-ons.md).

> [!NOTE]
> Les produits dans l’application ne peuvent pas être proposés à partir d’une version d’évaluation d’une application. Les clients qui utilisent une version d’évaluation de votre application ne peuvent acheter un produit in-app que s’ils achètent une version complète de votre application.

## <a name="prerequisites"></a>Prérequis

-   Application Windows dans laquelle ajouter des fonctionnalités que les clients peuvent acheter.
-   Lorsque vous codez et testez de nouveaux produits in-app pour la première fois, vous devez utiliser l’objet [CurrentAppSimulator](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) au lieu de l’objet [CurrentApp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp). Cela vous permet de vérifier votre logique de licence à l’aide d’appels simulés au serveur de licences au lieu d’appels au serveur Windows Live. Pour ce faire, vous devez personnaliser le fichier nommé WindowsStoreProxy.xml dans le dossier% UserProfile% \\ AppData \\ local \\ packages \\ &lt; name &gt; \\ LocalState \\ Microsoft \\ Windows Store \\ ApiData. Le simulateur Microsoft Visual Studio crée ce fichier quand vous exécutez votre application pour la première fois. Vous pouvez également charger un fichier personnalisé au moment de l’exécution. Pour plus d’informations, consultez [Utilisation du fichier WindowsStoreProxy.xml avec CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy).
-   Cette rubrique fait également référence à des exemples de code fournis dans [Exemple Windows Store](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store). Cet exemple représente un excellent moyen d’obtenir une expérience pratique avec les différentes options de monétisation fournies pour les applications UWP.

## <a name="step-1-initialize-the-license-info-for-your-app"></a>Étape 1 : Initialisation des informations de licence de votre application

Lors de l’initialisation de votre application, obtenez l’objet [LicenseInformation](/uwp/api/Windows.ApplicationModel.Store.LicenseInformation) de votre application en initialisant l’élément [CurrentApp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) ou [CurrentAppSimulator](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) pour activer les achats d’un produit in-app.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[EnableInAppPurchases](./code/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs#InitializeLicenseTest)]

## <a name="step-2-add-the-in-app-offers-to-your-app"></a>Étape 2 : Ajout d’offres in-app à votre application

Pour chaque fonctionnalité que vous voulez proposer par le biais d’un produit dans l’application, créez une offre et ajoutez-la à votre application.

> [!IMPORTANT]
> Vous devez ajouter tous les produits dans l’application que vous souhaitez présenter à vos clients à votre application avant de la soumettre au Windows Store. Pour ajouter ultérieurement de nouveaux produits dans l’application, vous devez mettre à jour votre application et en soumettre une nouvelle version.

1.  **Création d’un jeton d’offre dans l’application**

    Identifiez chaque produit dans l’application, via un jeton. Ce jeton est une chaîne que vous définissez et utilisez dans votre application, ainsi que dans le Windows Store, pour identifier un produit spécifique dans l’application. Donnez à votre jeton un nom unique et évocateur (dans l’application) afin de pouvoir rapidement identifier la fonctionnalité qu’il représente, lors de l’écriture du code. Voici quelques exemples de noms :

    * « SpaceMissionLevel4 »
    * « ContosoCloudSave »
    * « RainbowThemePack »

  > [!NOTE]
  > Le jeton d’offre dans l’application que vous utilisez dans votre code doit correspondre à la valeur d' [ID de produit](../publish/set-your-add-on-product-id.md#product-id) que vous spécifiez lorsque vous [Définissez le module complémentaire correspondant pour votre application dans l’espace partenaires](../publish/add-on-submissions.md).

2.  **Codage de la fonctionnalité dans un bloc conditionnel**

    Vous devez placer dans un bloc conditionnel le code de chaque fonctionnalité associée à un produit dans l’application. Ce bloc vérifie si le client possède une licence lui permettant d’utiliser cette fonctionnalité.

    Voici un exemple indiquant comment vous pouvez coder une fonctionnalité de produit nommée **featureName** dans le bloc conditionnel propre à une licence. La chaîne, **NomFonctionnalité**, est le jeton qui identifie de façon unique ce produit dans l’application et est également utilisé pour l’identifier dans le magasin.

    > [!div class="tabbedCodeSnippets"]
    [!code-csharp[EnableInAppPurchases](./code/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs#CodeFeature)]

3.  **Ajout de l’interface utilisateur d’achat pour cette fonctionnalité**

    Votre application doit également fournir à vos clients un moyen d’acheter le composant ou la fonctionnalité proposés par le produit dans l’application. En effet, ils ne peuvent pas les acheter par l’intermédiaire du Windows Store, de la même façon qu’ils ont acheté l’application complète.

    Voici comment vérifier si votre client possède déjà un produit dans l’application et, si tel n’est pas le cas, comment afficher la boîte de dialogue d’achat lui permettant de l’acheter. Remplacez le commentaire « Afficher la boîte de dialogue d’achat » par votre code personnalisé pour la boîte de dialogue d’achat (par exemple, une page présentant un bouton « Acheter cette application » convivial).

    > [!div class="tabbedCodeSnippets"]
    [!code-csharp[EnableInAppPurchases](./code/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs#BuyFeature)]

## <a name="step-3-change-the-test-code-to-the-final-calls"></a>Étape 3 : Modification du code de test jusqu’aux appels finaux

C’est simple : remplacez chaque référence à [CurrentAppSimulator](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) par une référence à [CurrentApp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) dans le code de votre application. Le fichier WindowsStoreProxy.xml n’est plus nécessaire. Vous pouvez le supprimer du chemin d’accès de votre application (mais vous pouvez l’enregistrer à titre de référence pour la configuration de l’offre in-app à l’étape suivante).

## <a name="step-4-configure-the-in-app-product-offer-in-the-store"></a>Étape 4 : Configuration dans le Windows Store de l’offre de produit in-app

Dans l’espace partenaires, accédez à votre application et [créez un module complémentaire](../publish/add-on-submissions.md) qui correspond à votre offre de produit dans l’application. Définissez l’ID de produit, le type, le prix et d’autres propriétés de votre module complémentaire. Veillez à effectuer les différents réglages en respectant la configuration définie dans le fichier WindowsStoreProxy.xml pendant le test.

  > [!NOTE]
  > Le jeton d’offre dans l’application que vous utilisez dans votre code doit correspondre à la valeur d' [ID de produit](../publish/set-your-add-on-product-id.md#product-id) que vous spécifiez pour le module complémentaire correspondant dans l’espace partenaires.

## <a name="remarks"></a>Remarques

Si vous envisagez de fournir à vos clients des options de produits consommables intégrés à l’application (éléments pouvant être achetés, utilisés, puis rachetés si nécessaire), passez à la rubrique [Activer les achats de produits consommables intégrés à l’application](enable-consumable-in-app-product-purchases.md).

Si vous avez besoin de reçus pour vérifier que l’utilisateur a bien effectué un achat in-app, consultez la rubrique [Utiliser des reçus pour vérifier les achats de produits](use-receipts-to-verify-product-purchases.md).

## <a name="related-topics"></a>Rubriques connexes


* [Activer les achats de produits consommables in-app](enable-consumable-in-app-product-purchases.md)
* [Gérer un vaste catalogue de produits in-app](manage-a-large-catalog-of-in-app-products.md)
* [Utiliser des reçus pour vérifier les achats de produits](use-receipts-to-verify-product-purchases.md)
* [Exemple du Windows Store (montre des versions d’évaluation et des achats in-app)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)