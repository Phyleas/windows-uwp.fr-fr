---
ms.assetid: F45E6F35-BC18-45C8-A8A5-193D528E2A4E
description: Découvrez les tâches et les concepts de base nécessaires pour implémenter et tester les achats dans l’application et les fonctionnalités d’évaluation dans les applications UWP.
title: Versions d’évaluation et achats in-app
ms.date: 05/09/2018
ms.topic: article
keywords: Windows 10, UWP, achats dans l’application, IAPs, modules complémentaires, versions d’évaluation, consommables, durables, abonnement
ms.localizationpriority: medium
ms.openlocfilehash: 1027159d250610a7d358f536d34f9c72fde1e940
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155543"
---
# <a name="in-app-purchases-and-trials"></a>Versions d’évaluation et achats in-app

Le Windows SDK fournit des API que vous pouvez utiliser pour implémenter les fonctionnalités suivantes afin de générer davantage de revenus avec votre application UWP :

* **In-app purchases** &nbsp; Achats &nbsp; dans l’application Que votre application soit libre ou non, vous pouvez vendre du contenu ou de nouvelles fonctionnalités d’application (telles que le déverrouillage du niveau suivant d’un jeu) à partir de l’application.

* **Trial functionality** &nbsp; Fonctionnalité &nbsp; d’évaluation Si vous [configurez votre application comme une version d’évaluation gratuite dans l’espace partenaires](../publish/set-app-pricing-and-availability.md#free-trial), vous pouvez inciter vos clients à acheter la version complète de votre application en excluant ou limitant certaines fonctionnalités pendant la période d’évaluation. Vous pouvez également activer certaines fonctionnalités, telles que des bannières ou des filigranes, qui ne s’afficheront que pendant la période d’évaluation, avant l’achat de votre application par un client.

Cet article vous fournit une vue d’ensemble du fonctionnement des achats in-app et des versions d’évaluation dans les applications UWP.

<span id="choose-namespace" />

## <a name="choose-which-namespace-to-use"></a>Choisir l’espace de noms à utiliser

Il existe deux espaces de noms différents à utiliser pour ajouter des achats in-app et des fonctionnalités de version d’évaluation dans vos applications UWP, en fonction de la version de Windows 10 ciblée par vos applications. Bien que les API dans ces espaces de noms servent les mêmes objectifs, elles sont conçues légèrement différemment et leur code n’est pas compatible.

* **[Windows. services. Store](/uwp/api/windows.services.store)** &nbsp; &nbsp; À compter de Windows 10, version 1607, les applications peuvent utiliser l’API dans cet espace de noms pour implémenter des achats et des versions d’évaluation dans l’application. Nous vous recommandons d’utiliser les membres de cet espace de noms si votre projet d’application cible l' **édition anniversaire Windows 10 (10,0 ; Build 14393)** ou une version ultérieure dans Visual Studio. Cet espace de noms prend en charge les types de compléments les plus récents, tels que les compléments consommables gérés par le magasin, et est conçu pour être compatible avec les futurs types de produits et fonctionnalités pris en charge par l’espace partenaires et le Store. Pour plus d’informations sur cet espace de noms, consultez la section [Achats dans l’application et versions d’évaluation utilisant l’espace de noms Windows.Services.Store](#api_intro) de cet article.

* **[Windows. ApplicationModel. Store](/uwp/api/windows.applicationmodel.store)** &nbsp; &nbsp; Toutes les versions de Windows 10 prennent également en charge une API plus ancienne pour les achats dans l’application et les versions d’évaluation dans cet espace de noms. Pour plus d’informations sur l’espace de noms **Windows. ApplicationModel. Store** , consultez [achats et évaluations dans l’application à l’aide de l’espace de noms Windows. ApplicationModel. Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

> [!IMPORTANT]
> L’espace de noms **Windows. ApplicationModel. Store** n’est plus mis à jour avec les nouvelles fonctionnalités et nous vous recommandons d’utiliser à la place l’espace de noms **Windows. services. Store** , si possible pour votre application. L’espace de noms **Windows. ApplicationModel. Store** n’est pas pris en charge dans les applications de bureau Windows qui utilisent le [pont de bureau](https://developer.microsoft.com/windows/bridges/desktop) ou dans des applications ou des jeux qui utilisent un bac à sable (sandbox) de développement dans l’espace partenaires (par exemple, c’est le cas pour tout jeu qui s’intègre à Xbox Live).

<span id="concepts" />

## <a name="basic-concepts"></a>Concepts de base

En général, chacun des éléments proposés dans le Windows Store est appelé *produit*. La plupart des développeurs ne fonctionnent qu’avec les types de produits suivants : les *applications* et les *modules*complémentaires.

Un module complémentaire est un produit ou une fonctionnalité que vous mettez à la disposition de vos clients dans le contexte de votre application : par exemple, la devise à utiliser dans une application ou un jeu, les nouvelles cartes ou armes pour un jeu, la possibilité d’utiliser votre application sans publicité, ou un contenu numérique tel que de la musique ou des vidéos pour les applications qui ont la possibilité de proposer ce type Chaque application ou module complémentaire a une licence qui indique si l’utilisateur est autorisé à l’utiliser. Si l’utilisateur est autorisé à utiliser l’application ou le module complémentaire en tant que version d’évaluation, la licence fournit également des informations supplémentaires sur la version d’évaluation.

Pour offrir un module complémentaire aux clients de votre application, vous devez [définir le module complémentaire de votre application dans l’espace partenaires, de](../publish/add-on-submissions.md) sorte que le magasin en sache. Ensuite, votre application peut utiliser les API des espaces de noms **Windows.Services.Store** ou **Windows.ApplicationModel.Store** pour proposer le module complémentaire à la vente à l’utilisateur, en tant qu’achat in-app.

Les applications UWP peuvent offrir des types de modules complémentaires suivants.

| Type de module complémentaire |  Description  |
|---------|-------------------|
| Durable  |  Module complémentaire qui persiste pendant la durée de vie que vous [spécifiez dans l’espace partenaires](../publish/enter-add-on-properties.md). <p/><p/>Par défaut, les modules complémentaires durables n’ont pas de date d’expiration et ne peuvent donc être achetés qu’une seule fois. Si vous spécifiez une durée pour le module complémentaire, l’utilisateur peut racheter le module complémentaire après l’expiration de ce dernier. |
| Consommable géré par le développeur  |  Module complémentaire qui peut être acheté, utilisé, puis acheté à nouveau après avoir été consommé. Vous êtes chargé de suivre le solde des éléments que le module complémentaire représente.<p/><p/>Lorsque l’utilisateur consomme des éléments associés au module complémentaire, il vous incombe de maintenir le solde de l’utilisateur et de signaler l’achat du module complémentaire comme étant satisfait au magasin une fois que l’utilisateur a consommé tous les éléments. L’utilisateur ne peut pas acheter à nouveau le module complémentaire, tant que votre application n’a pas signalé le module complémentaire précédent comme épuisé. <p/><p/>Par exemple, si votre module complémentaire représente 100 pièces dans un jeu et que l’utilisateur en consomme 10, votre application ou votre service doit maintenir le nouveau solde restant de 90 pièces pour l’utilisateur. Une fois que l’utilisateur a consommé les 100 pièces, votre application doit signaler le module complémentaire comme épuisé. Ensuite, l’utilisateur peut racheter le module complémentaire de 100 pièces.    |
| Consommable géré par le Windows Store  |  Un module complémentaire qui peut être acheté, utilisé et acheté à tout moment. Le magasin effectue le suivi du solde des éléments que le module complémentaire représente.<p/><p/>Lorsque l’utilisateur consomme des éléments associés au module complémentaire, vous êtes responsable de la création de rapports pour les éléments qui sont satisfaits par le magasin, et le magasin met à jour le solde de l’utilisateur. L’utilisateur peut acheter le module complémentaire autant de fois qu’il le souhaite (il n’a pas besoin d’utiliser les éléments en premier). Votre application peut demander le solde actuel de l’utilisateur à tout moment. <p/><p/> Par exemple, si votre module complémentaire représente une quantité initiale de 100 pièces dans un jeu et que l’utilisateur consomme 50 pièces, votre application signale au magasin que 50 unités du module complémentaire ont été remplies et le magasin met à jour le solde restant. Si l’utilisateur achète ensuite votre module complémentaire pour acquérir 100 de pièces supplémentaires, il dispose désormais de 150 pièces. <p/><p/>**Note** &nbsp; Remarque &nbsp; Pour utiliser les consommables gérés par le magasin, votre application doit cibler l' **édition anniversaire Windows 10 (10,0 ; Build 14393)** ou une version ultérieure dans Visual Studio, et elle doit utiliser l’espace de noms **Windows. services. Store** au lieu de l’espace de noms **Windows. ApplicationModel. Store** .  |
| Abonnement | Un module complémentaire durable où le client continue à être facturé à des intervalles récurrents afin de continuer à utiliser le module complémentaire. Le client peut annuler l’abonnement à tout moment pour éviter d’autres frais. <p/><p/>**Note** &nbsp; Remarque &nbsp; Pour utiliser des modules complémentaires d’abonnement, votre application doit cibler l' **édition anniversaire Windows 10 (10,0 ; Build 14393)** ou une version ultérieure dans Visual Studio, et elle doit utiliser l’espace de noms **Windows. services. Store** au lieu de l’espace de noms **Windows. ApplicationModel. Store** .  |

<span />

> [!NOTE]
> D’autres types de modules complémentaires, tels que des modules complémentaires durables avec des packages (également appelés contenu téléchargeable ou DLC), ne sont disponibles que pour un ensemble restreint de développeurs et ne sont pas traités dans cette documentation.

<span id="api_intro" />

## <a name="in-app-purchases-and-trials-using-the-windowsservicesstore-namespace"></a>Achats dans l’application et versions d’évaluation utilisant l’espace de noms Windows.Services.Store

Cette section fournit une vue d’ensemble des tâches et concepts importants pour l’espace de noms [Windows. services. Store](/uwp/api/windows.services.store) . Cet espace de noms est disponible uniquement pour les applications qui ciblent l' **édition anniversaire Windows 10 (10,0 ; Build 14393)** ou version ultérieure dans Visual Studio (cela correspond à Windows 10, version 1607). Nous recommandons que les applications utilisent l’espace de noms **Windows. services. Store** au lieu de l’espace de noms [Windows. ApplicationModel. Store](/uwp/api/windows.applicationmodel.store) , si possible. Pour plus d’informations sur l’espace de noms **Windows. ApplicationModel. Store** , consultez [cet article](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

**Dans cette section**

* [Vidéo](#video)
* [Démarrer avec la classe StoreContext](#get-started-storecontext)
* [Implémenter des achats dans l’application](#implement-iap)
* [Implémenter les fonctionnalités des versions d’évaluation](#implement-trial)
* [Tester l’implémentation de votre achat dans l’application ou de votre version d’évaluation](#testing)
* [Reçus des achats dans l’application](#receipts)
* [Utilisation de la classe StoreContext avec Desktop Bridge](#desktop)
* [Produits, références et disponibilités](#products-skus)
* [ID Windows Store](#store-ids)

<span id="video" />

### <a name="video"></a>Vidéo

Regardez la vidéo suivante pour obtenir une vue d’ensemble de l’implémentation des achats dans l’application dans votre application à l’aide de l’espace de noms [Windows. services. Store](/uwp/api/windows.services.store) .
<br/>
<br/>
> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Adding-In-App-Purchases-to-Your-UWP-App/player]

<span id="get-started-storecontext" />

### <a name="get-started-with-the-storecontext-class"></a>Démarrer avec la classe StoreContext

Le point d’entrée principal de l’espace de noms **Windows.Services.Store** est la classe [StoreContext](/uwp/api/windows.services.store.storecontext). Cette classe fournit des méthodes que vous pouvez utiliser, entre autres, pour obtenir des informations sur l’application active et ses modules complémentaires disponibles, acheter une application ou un module complémentaire pour l’utilisateur actuel, et obtenir des informations sur la licence de l’application en cours ou de ses modules complémentaires. Pour obtenir un objet [StoreContext](/uwp/api/windows.services.store.storecontext), effectuez l’une des opérations suivantes :

* Dans une application mono-utilisateur (autrement dit, une application qui s’exécute uniquement dans le contexte de l’utilisateur qui a lancé l’application), utilisez la méthode statique [GetDefault](/uwp/api/windows.services.store.storecontext.getdefault) pour obtenir un objet **StoreContext** que vous pouvez utiliser pour accéder aux données relatives à Microsoft Store pour l’utilisateur. La plupart des applications de plateforme Windows universelle (UWP) sont des applications mono-utilisateur.

  ```csharp
  Windows.Services.Store.StoreContext context = StoreContext.GetDefault();
  ```

* Dans une [application multi-utilisateur](../xbox-apps/multi-user-applications.md), utilisez la méthode statique [GetForUser](/uwp/api/windows.services.store.storecontext.getforuser) pour obtenir un objet **StoreContext** que vous pouvez utiliser pour accéder aux données relatives à Microsoft Store pour un utilisateur spécifique qui est connecté avec son compte Microsoft lors de l’utilisation de l’application. L’exemple suivant obtient un objet **StoreContext** pour le premier utilisateur disponible.

  ```csharp
  var users = await Windows.System.User.FindAllAsync();
  Windows.Services.Store.StoreContext context = StoreContext.GetForUser(users[0]);
  ```

> [!NOTE]
> Les applications de bureau Windows qui utilisent le [pont Desktop](https://developer.microsoft.com/windows/bridges/desktop) doivent effectuer des étapes supplémentaires pour configurer l’objet [StoreContext](/uwp/api/windows.services.store.storecontext) avant de pouvoir utiliser cet objet. Pour plus d’informations, consultez [cette section](#desktop).

Une fois que vous possédez un objet [StoreContext](/uwp/api/windows.services.store.storecontext), vous pouvez commencer à appeler des méthodes de cet objet pour obtenir des informations produit du Windows Store pour l’application actuelle et ses extensions, récupérer des informations de licence de l’application actuelle et de ses extensions, acheter une application ou une extension pour l’utilisateur actuel, et effectuer d’autres tâches. Pour plus d’informations sur les tâches courantes que vous pouvez exécuter à l’aide de cet objet, consultez les articles suivants :

* [Obtenir les informations produit des applications et des extensions](get-product-info-for-apps-and-add-ons.md)
* [Obtenir les informations de licence des applications et des modules complémentaires](get-license-info-for-apps-and-add-ons.md)
* [Activer les achats in-app d’applications et de modules complémentaires](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Activer les achats de modules complémentaires consommables](enable-consumable-add-on-purchases.md)
* [Activer les extensions d’abonnement de votre application](enable-subscription-add-ons-for-your-app.md)
* [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md)

Pour consulter un exemple d’application illustrant l’utilisation de l’objet **StoreContext** et d’autres types dans l’espace de noms **Windows.Services.Store**, consultez [Exemple Windows Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

<span id="implement-iap" />

### <a name="implement-in-app-purchases"></a>Implémenter des achats dans l’application

Pour proposer un achat dans l’application aux clients de votre application à l’aide de l’espace de noms **Windows.Services.Store** :

1. Si votre application propose des modules complémentaires que les clients peuvent acheter, [créez des soumissions complémentaires pour votre application dans l’espace partenaires ](../publish/add-on-submissions.md).

2. Écrivez du code dans votre application afin de [récupérer les informations de produit pour votre application ou un module complémentaire offert par votre application](get-product-info-for-apps-and-add-ons.md), puis [déterminez si la licence est active](get-license-info-for-apps-and-add-ons.md) (c’est-à-dire, si l’utilisateur dispose d’une licence lui permettant d’utiliser l’application ou le module complémentaire). Si la licence n’est pas active, affichez une interface utilisateur qui propose à l’utilisateur l’application ou le module complémentaire à la vente en tant qu’achat in-app.

3. Si l’utilisateur choisit d’acheter votre application ou extension, utilisez la méthode d’achat appropriée du produit :

    * Si l’utilisateur achète votre application ou une extension durable, suivez les instructions de la rubrique [Activer les achats in-app d’applications et d’extensions](enable-in-app-purchases-of-apps-and-add-ons.md).
    * Si l’utilisateur achète un module complémentaire de consommable, suivez les instructions de la rubrique [Activer les achats de modules complémentaires consommables](enable-consumable-add-on-purchases.md).
    * Si l’utilisateur achète un module complémentaire d’abonnement, suivez les instructions indiquées dans [activer les modules complémentaires d’abonnement pour votre application](enable-subscription-add-ons-for-your-app.md).

4. Pour tester votre implémentation, suivez les [conseils de test](#testing) de cet article.

<span id="implement-trial" />

### <a name="implement-trial-functionality"></a>Implémenter les fonctionnalités des versions d’évaluation

Pour exclure ou limiter les fonctionnalités d’une version d’évaluation de votre application à l’aide de l’espace de noms **Windows.Services.Store** :

1. [Configurez votre application comme une version d’évaluation gratuite dans l’espace partenaires](../publish/set-app-pricing-and-availability.md#free-trial).

2. Écrivez du code dans votre application afin de [récupérer les informations de produit de votre application et d’un module complémentaire pris en charge par votre application](get-product-info-for-apps-and-add-ons.md), puis [déterminez si la licence associée à l’application est une licence d’évaluation](get-license-info-for-apps-and-add-ons.md).

3. Excluez ou limitez certaines fonctionnalités de votre application s’il s’agit d’une version d’évaluation, puis activez les fonctionnalités quand l’utilisateur acquiert une licence complète. Pour obtenir plus d’informations et un exemple de code, voir [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md).

4. Pour tester votre implémentation, suivez les [conseils de test](#testing) de cet article.

<span id="testing" />

### <a name="test-your-in-app-purchase-or-trial-implementation"></a>Tester l’implémentation de votre achat dans l’application ou de votre version d’évaluation

Si votre application utilise des API dans l’espace de noms **Windows. services. Store** pour implémenter une fonctionnalité d’évaluation ou d’achat dans l’application, vous devez publier votre application dans le magasin et télécharger l’application sur votre appareil de développement afin d’utiliser sa licence de test. Suivez ce processus pour tester votre code :

1. Si votre application n’est pas encore publiée et disponible dans le Windows Store, assurez-vous que votre application répond aux exigences de [Kit de certification des applications Windows](https://developer.microsoft.com/windows/develop/app-certification-kit) minimales, [soumettez votre application](../publish/app-submissions.md) dans l’espace partenaires et assurez-vous que votre application passe le processus de certification. Vous pouvez [configurer votre application afin qu’elle ne soit pas détectable dans le magasin](../publish/set-app-pricing-and-availability.md) pendant que vous la Testez. Veuillez noter la configuration appropriée des [vols de packages](../publish/package-flights.md). Les vols de packages configurés de manière incorrecte peuvent ne pas pouvoir être téléchargés.

2. Ensuite, assurez-vous d’exécuter les actions suivantes :

    * Écrivez du code dans votre application qui utilise la classe [StoreContext](/uwp/api/windows.services.store.storecontext) et d’autres types associés dans l’espace de noms **Windows.Services.Store** afin d’implémenter les [achats dans l’application](#implement-iap) ou une [fonctionnalité de version d’évaluation](#implement-trial).
    * Si votre application propose un module complémentaire que les clients peuvent acheter, [créez une soumission de module complémentaire pour votre application dans l’espace partenaires](../publish/add-on-submissions.md).
    * Si vous souhaitez exclure ou limiter certaines fonctionnalités dans une version d’évaluation de votre application, [configurez votre application en tant qu’évaluation gratuite dans l’espace partenaires](../publish/set-app-pricing-and-availability.md#free-trial).

3. Avec votre projet ouvert dans Visual Studio, cliquez sur le **menu Projet**, pointez sur **Store**, puis cliquez sur **Associer l’application au Windows Store**. Suivez les instructions de l’Assistant pour associer le projet d’application à l’application de votre compte espace partenaires que vous souhaitez utiliser pour le test.
    > [!NOTE]
    > Si vous n’associez pas votre projet à une application du Windows Store, les méthodes [StoreContext](/uwp/api/windows.services.store.storecontext) définissent la propriété **ExtendedError** de leurs valeurs de retour sur la valeur de code d’erreur 0x803F6107. Cette valeur indique que le Windows Store ne connaît pas l’application.
4. Si ce n’est déjà fait, installez l’application à partir du Windows Store que vous avez spécifié à l’étape précédente, exécutez l’application une fois, puis fermez-la. Ceci garantit l’installation d’une licence valide de l’application sur votre appareil de développement.

5. Dans Visual Studio, exécutez ou déboguez votre projet. Votre code doit récupérer les données d’application et de module complémentaire, à partir de l’application Windows Store que vous avez associée à votre projet local. Si vous êtes invité à réinstaller l’application, suivez les instructions, puis exécutez ou déboguez votre projet.
    > [!NOTE]
    > Une fois ces étapes terminées, vous pouvez continuer à mettre à jour le code de votre application, puis déboguer votre projet mis à jour sur votre ordinateur de développement sans envoyer de nouveaux packages d’application au Windows Store. Il vous suffit de télécharger la version Windows Store de votre application sur votre ordinateur de développement pour obtenir la licence locale qui sera utilisée pour les tests. Vous devez uniquement soumettre les nouveaux packages d’application au Windows Store après avoir terminé vos tests et proposer à vos clients les fonctionnalités liées aux achats in-app ou à la version d’évaluation de votre application.

Si votre application utilise l’espace de noms **Windows. ApplicationModel. Store** , vous pouvez utiliser la classe [CurrentAppSimulator](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) dans votre application pour simuler les informations de licence lors du test avant de soumettre votre application au Windows Store. Pour plus d’informations, consultez [prise en main des classes CurrentApp et CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#get-started-with-the-currentapp-and-currentappsimulator-classes).  

> [!NOTE]
> L’espace de noms **Windows.Services.Store** ne fournit pas une classe que vous pouvez utiliser pour simuler des informations de licence lors d’un test. Si vous utilisez l’espace de noms **Windows. services. Store** pour implémenter des achats ou des versions d’évaluation dans l’application, vous devez publier votre application dans le Windows Store et télécharger l’application sur votre appareil de développement afin d’utiliser sa licence de test, comme décrit ci-dessus.

<span id="receipts" />

### <a name="receipts-for-in-app-purchases"></a>Reçus des achats dans l’application

L’espace de noms **Windows.Services.Store** ne fournit aucune API vous permettant d’obtenir un reçu de transaction pour les achats effectués dans le code de votre application. Il s’agit d’une expérience différente de celles des applications qui utilisent l’espace de noms **Windows.ApplicationModel.Store**, qui peuvent elles [utiliser une API côté client pour récupérer un reçu de transaction](use-receipts-to-verify-product-purchases.md).

Si vous implémentez des achats dans l’application à l’aide de l’espace de noms **Windows. services. Store** et que vous souhaitez vérifier si un client donné a acheté une application ou un module complémentaire, vous pouvez utiliser la [méthode Query for Products](query-for-products.md) dans l' [API REST de collection Microsoft Store](view-and-grant-products-from-a-service.md). Les données renvoyées avec cette méthode confirment si l’utilisateur considéré possède des droits pour un produit donné, et fournissent des données pour la transaction dans laquelle l’utilisateur a acquis le produit. L’API de collection Microsoft Store utilise l’authentification Azure AD pour récupérer ces informations.

<span id="desktop" />

### <a name="using-the-storecontext-class-with-the-desktop-bridge"></a>Utilisation de la classe StoreContext avec Desktop Bridge

Les applications de bureau qui utilisent [Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop) peuvent utiliser la classe [StoreContext](/uwp/api/windows.services.store.storecontext) pour implémenter des achats dans l’application et des versions d’évaluation. Toutefois, si vous possédez une application de bureau Win32 ou une application de bureau présentant un handle Windows associé à l’infrastructure de rendu (comme une application WPF), votre application doit configurer l’objet **StoreContext** pour identifier la fenêtre applicative du propriétaire pour les boîtes de dialogue modales affichées par l’objet.

De nombreux membres **StoreContext** (et des membres d’autres types associés accessibles via l’objet **StoreContext**) affichent une boîte de dialogue modale à l’utilisateur, pour les opérations associées au Windows Store telle que l’achat d’un produit. Si une application de bureau ne configure pas l’objet **StoreContext** afin de spécifier la fenêtre du propriétaire pour les boîtes de dialogue modales, cet objet renvoie des données inappropriées ou des erreurs.

Pour configurer un objet **StoreContext** dans une application de bureau qui utilise Desktop Bridge, procédez comme suit.

1. Pour permettre à votre application d’accéder à l’interface [IInitializeWithWindow](/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iinitializewithwindow), effectuez l’une des actions suivantes :

    * Si votre application est écrite dans un langage géré comme C# ou Visual Basic, déclarez l’interface **IInitializeWithWindow** dans le code de votre application avec l’attribut [ComImport](/dotnet/api/system.runtime.interopservices.comimportattribute), comme représenté dans l’exemple suivant en C#. Cet exemple suppose que votre fichier de code présente une instruction **using** pour l’espace de noms **System.Runtime.InteropServices**.

        ```csharp
        [ComImport]
        [Guid("3E68D4BD-7135-4D10-8018-9FB6D9F33FA1")]
        [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
        public interface IInitializeWithWindow
        {
            void Initialize(IntPtr hwnd);
        }
        ```

    * Si votre application est écrite en C++, ajoutez une référence au fichier d’en-tête shobjidl.h dans votre code. Ce fichier d’en-tête contient la déclaration de l’interface **IInitializeWithWindow**.

2. Récupérez un objet [StoreContext](/uwp/api/windows.services.store.storecontext) en utilisant la méthode [GetDefault](/uwp/api/windows.services.store.storecontext.getdefault) (ou la méthode [GetForUser](/uwp/api/windows.services.store.storecontext.getforuser) si votre application est une [application multi-utilisateur](../xbox-apps/multi-user-applications.md)) comme décrit plus haut dans cet article, puis convertissez cet objet en objet [IInitializeWithWindow](/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iinitializewithwindow). Ensuite, appelez la méthode [IInitializeWithWindow.Initialize](/windows/desktop/api/shobjidl_core/nf-shobjidl_core-iinitializewithwindow-initialize), puis transmettez le handle de la fenêtre que vous souhaitez configurer comme propriétaire pour toutes les boîtes de dialogue modales affichées par les méthodes **StoreContext**. L’exemple suivant en C# vous explique comment transmettre le handle de la fenêtre principale de votre application à la méthode.
    ```csharp
    StoreContext context = StoreContext.GetDefault();
    IInitializeWithWindow initWindow = (IInitializeWithWindow)(object)context;
    initWindow.Initialize(System.Diagnostics.Process.GetCurrentProcess().MainWindowHandle);
    ```

<span id="products-skus" />

### <a name="products-skus-and-availabilities"></a>Produits, références et disponibilités

Chaque produit dans le Windows Store a au moins une *référence (SKU)*, et chaque référence (SKU) a au moins une *disponibilité*. Ces concepts sont extraits de la plupart des développeurs dans l’espace partenaires, et la plupart des développeurs ne définissent jamais les références SKU ou les availabilities pour leurs applications ou modules complémentaires. Toutefois, étant donné que le modèle objet pour les produits du Store dans l’espace de noms **Windows. services. Store** comprend des références SKU et des availabilities, une compréhension élémentaire de ces concepts peut être utile pour certains scénarios.

| Object |  Description  |
|---------|-------------------|
| Produit  |  Un *produit* fait référence à n’importe quel type de produit disponible dans le magasin, y compris une application ou un module complémentaire. <p/><p/> Chaque produit du magasin a un objet [StoreProduct](/uwp/api/windows.services.store.storeproduct) correspondant. Cette classe fournit des propriétés qui vous permettent d’accéder à des données, telles que l’ID Windows Store du produit, les images et vidéos de la liste du Windows Store, ainsi que des informations tarifaires. Elle fournit également des méthodes que vous pouvez utiliser pour acheter le produit. |
| SKU |  Une *référence (SKU* ) est une version spécifique d’un produit avec sa propre description, son propre prix et d’autres détails uniques sur le produit. Chaque application ou module complémentaire a une référence (SKU) par défaut. Le seul cas où la plupart des développeurs ont plusieurs références (SKU) pour une application, c’est lorsqu’ils publient une version complète de leur application et une version d’évaluation. Dans le catalogue du Windows Store, chacune de ces versions a une référence (SKU) différente de la même application. <p/><p/> Certains éditeurs peuvent définir leurs propres références (SKU). Par exemple, un grand éditeur de jeux peut commercialiser un jeu avec une référence (SKU) qui affiche du sang vert sur les marchés qui n’autorisent pas le sang rouge et une autre référence (SKU) montrant du sang rouge sur tous les autres marchés. Autre possibilité, un éditeur qui vend des contenus vidéo numériques peut publier deux références (SKU) pour une vidéo : une pour la version haute définition et une autre pour la version en définition standard. <p/><p/> Chaque référence (SKU) du magasin a un objet [StoreSku](/uwp/api/windows.services.store.storesku) correspondant. Chaque [StoreProduct](/uwp/api/windows.services.store.storeproduct) a une propriété [SKU](/uwp/api/windows.services.store.storeproduct.skus) que vous pouvez utiliser pour accéder aux références SKU du produit. |
| Disponibilité  |  Une *disponibilité* est une version spécifique d’une référence (SKU) avec ses propres informations de tarification uniques. Chaque référence (SKU) a une disponibilité par défaut. Certains éditeurs peuvent définir leurs propres disponibilités pour proposer des prix différents pour une référence (SKU) donnée. <p/><p/> Chaque disponibilité dans le magasin a un objet [StoreAvailability](/uwp/api/windows.services.store.storeavailability) correspondant. Chaque [StoreSku](/uwp/api/windows.services.store.storesku) a une propriété [availabilities](/uwp/api/windows.services.store.storesku.availabilities) que vous pouvez utiliser pour accéder aux availabilities pour la référence SKU. Pour la plupart des développeurs, chaque référence (SKU) a une disponibilité unique par défaut.  |

<span id="store_ids" />

### <a name="store-ids"></a>ID Windows Store

Chaque application, module complémentaire ou autre produit du Windows Store est associé à un **ID de magasin** (parfois appelé ID de magasin de *produits*). De nombreuses API requièrent l’ID du magasin pour effectuer une opération sur une application ou un module complémentaire.

L’ID Windows Store d’un produit dans le Windows Store est composé de 12 caractères alphanumériques, comme ```9NBLGGH4R315```. Il existe plusieurs façons d’obtenir l’ID de magasin d’un produit dans le magasin :

* Pour une application, vous pouvez obtenir l’ID du magasin sur la [page identité](../publish/view-app-identity-details.md) de l’application dans l’espace partenaires.
* Pour un module complémentaire, vous pouvez obtenir l’ID du magasin sur la page vue d’ensemble du module complémentaire dans l’espace partenaires.
* Pour tous les produits, vous pouvez également obtenir l’ID du magasin par programme en utilisant la propriété [StoreID](/uwp/api/windows.services.store.storeproduct.storeid) de l’objet [StoreProduct](/uwp/api/windows.services.store.storeproduct) qui représente le produit.

Pour les produits avec SKU et availabilities, les SKU et les disponibilités ont également leurs propres ID de magasin avec différents formats.

| Objet |  Format de l’ID Windows Store  |
|---------|-------------------|
| SKU |  L’ID de magasin pour une référence (SKU) a le format ```<product Store ID>/xxxx``` , où ```xxxx``` est une chaîne alphanumérique de 4 caractères qui identifie une référence (SKU) pour le produit. Par exemple : ```9NBLGGH4R315/000N```. Cet ID est retourné par la propriété [StoreID](/uwp/api/windows.services.store.storesku.storeid) d’un objet  [StoreSku](/uwp/api/windows.services.store.storesku) , et il est parfois appelé ID du *magasin de références SKU*. |
| Disponibilité  |  L’ID de magasin pour une disponibilité a le format ```<product Store ID>/xxxx/yyyyyyyyyyyy``` , où ```xxxx``` est une chaîne alphanumérique de 4 caractères qui identifie une référence (SKU) pour le produit et ```yyyyyyyyyyyy``` est une chaîne alphanumérique de 12 caractères qui identifie une disponibilité pour la référence (SKU). Par exemple : ```9NBLGGH4R315/000N/4KW6QZD2VN6X```. Cet ID est retourné par la propriété [StoreID](/uwp/api/windows.services.store.storeavailability.storeid) d’un objet  [StoreAvailability](/uwp/api/windows.services.store.storeavailability) , et il est parfois appelé ID du *magasin de disponibilité*.  |

<span id="product-ids" />

## <a name="how-to-use-product-ids-for-add-ons-in-your-code"></a>Comment utiliser les ID de produit pour les modules complémentaires dans votre code

Si vous souhaitez rendre un module complémentaire accessible à vos clients dans le contexte de votre application, vous devez [entrer un ID de produit unique](../publish/set-your-add-on-product-id.md#product-id) pour votre module complémentaire lors de la [création de votre soumission de module](../publish/add-on-submissions.md) complémentaire dans l’espace partenaires. Vous pouvez utiliser cet ID de produit pour faire référence au module complémentaire de votre code, bien que les scénarios spécifiques dans lesquels vous pouvez utiliser l’ID de produit dépendent de l’espace de noms que vous utilisez pour les achats dans l’application dans votre application.

> [!NOTE]
> L’ID de produit que vous entrez dans l’espace partenaires pour un module complémentaire est différent de l' [ID de magasin](#store-ids)du module complémentaire. L’ID du magasin est généré par l’espace partenaires.

### <a name="apps-that-use-the-windowsservicesstore-namespace"></a>Applications qui utilisent l’espace de noms Windows. services. Store

Si votre application utilise l’espace de noms **Windows. services. Store** , vous pouvez utiliser l’ID de produit pour identifier facilement le [StoreProduct](/uwp/api/Windows.Services.Store.StoreProduct) qui représente votre module complémentaire ou le [StoreLicense](/uwp/api/windows.services.store.storelicense) qui représente la licence de votre module complémentaire. L’ID de produit est exposé par les propriétés [StoreProduct. InAppOfferToken](/uwp/api/Windows.Services.Store.StoreProduct.InAppOfferToken) et [StoreLicense. InAppOfferToken](/uwp/api/windows.services.store.storelicense.InAppOfferToken) .

> [!NOTE]
> Bien que l’ID de produit soit un moyen utile d’identifier un module complémentaire dans votre code, la plupart des opérations dans l’espace de noms **Windows. services. Store** utilisent l' [ID de magasin](#store-ids) d’un module complémentaire au lieu de l’ID de produit. Par exemple, pour récupérer par programme un ou plusieurs modules complémentaires connus pour une application, transmettez les ID de magasin (plutôt que les ID de produit) des modules complémentaires à la méthode [GetStoreProductsAsync](/uwp/api/windows.services.store.storecontext.getstoreproductsasync) . De même, pour signaler qu’un module complémentaire utilisable est rempli, transmettez l’ID du magasin du module complémentaire (plutôt que l’ID du produit) à la méthode [ReportConsumableFulfillmentAsync](/uwp/api/windows.services.store.storecontext.reportconsumablefulfillmentasync) .

### <a name="apps-that-use-the-windowsapplicationmodelstore-namespace"></a>Applications qui utilisent l’espace de noms Windows. ApplicationModel. Store

Si votre application utilise l’espace de noms **Windows. ApplicationModel. Store** , vous devez utiliser l’ID de produit que vous attribuez à un module complémentaire dans l’espace partenaires pour la plupart des opérations. Par exemple :

* Utilisez l’ID de produit pour identifier le [ProductListing](/uwp/api/windows.applicationmodel.store.productlisting) qui représente votre module complémentaire ou le [ProductLicense](/uwp/api/windows.applicationmodel.store.productlicense) qui représente la licence de votre module complémentaire. L’ID de produit est exposé par les propriétés [ProductListing. ProductID](/uwp/api/windows.applicationmodel.store.productlisting.ProductId) et [ProductLicense. ProductID](/uwp/api/windows.applicationmodel.store.productlicense.ProductId) .

* Spécifiez l’ID de produit lorsque vous achetez votre module complémentaire pour l’utilisateur à l’aide de la méthode [RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) . Pour plus d’informations, voir [Activer l’achat de produits dans l’application](enable-in-app-product-purchases.md).

* Spécifiez l’ID de produit lorsque vous signalez votre module complémentaire utilisable comme étant rempli à l’aide de la méthode [ReportConsumableFulfillmentAsync](/uwp/api/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync) . Pour plus d’informations, consultez [activer les achats de produits consommables dans l’application](enable-consumable-in-app-product-purchases.md).

## <a name="related-topics"></a>Rubriques connexes

* [Obtenir les informations produit des applications et des extensions](get-product-info-for-apps-and-add-ons.md)
* [Obtenir les informations de licence des applications et des modules complémentaires](get-license-info-for-apps-and-add-ons.md)
* [Activer les achats in-app d’applications et de modules complémentaires](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Activer les achats de modules complémentaires consommables](enable-consumable-add-on-purchases.md)
* [Activer les extensions d’abonnement de votre application](enable-subscription-add-ons-for-your-app.md)
* [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md)
* [Codes d’erreur pour les opérations du Store](error-codes-for-store-operations.md)
* [Versions d’évaluation et achats in-app à l’aide de l’espace de noms Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)