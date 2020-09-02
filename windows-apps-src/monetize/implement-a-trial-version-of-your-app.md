---
ms.assetid: 571697B7-6064-4C50-9A68-1374F2C3F931
description: Découvrez comment utiliser l’espace de noms Windows.Services.Store pour implémenter une version d’évaluation de votre application.
title: Implémenter une version d’évaluation de votre application
keywords: Windows 10, UWP, version d’évaluation, achats dans l’application, Windows. services. Store
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c07be51de312ab5a8483cb67537e809d3a55e14a
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363641"
---
# <a name="implement-a-trial-version-of-your-app"></a>Implémenter une version d’évaluation de votre application

Si vous [configurez votre application comme une version d’évaluation gratuite dans l’espace partenaires](../publish/set-app-pricing-and-availability.md#free-trial) afin que les clients puissent utiliser votre application gratuitement pendant une période d’évaluation, vous pouvez inciter vos clients à effectuer une mise à niveau vers la version complète de votre application en excluant ou limitant certaines fonctionnalités pendant la période d’évaluation. Choisissez les fonctionnalités à limiter avant de commencer à coder, puis faites en sorte que votre application ne les rende disponibles qu’à l’achat de la licence complète. Vous pouvez également activer certaines fonctionnalités, telles que des bannières ou des filigranes, qui ne s’afficheront que pendant la période d’évaluation, avant l’achat de votre application par un client.

Cet article montre comment utiliser les membres de la classe [StoreContext](/uwp/api/windows.services.store.storecontext) dans l’espace de noms [Windows. services. Store](/uwp/api/windows.services.store) pour déterminer si l’utilisateur dispose d’une licence d’évaluation pour votre application et être informé si l’état de la licence change pendant l’exécution de votre application. 

> [!NOTE]
> L’espace de noms **Windows. services. Store** a été introduit dans Windows 10, version 1607, et il ne peut être utilisé que dans les projets qui ciblent l' **édition anniversaire windows 10 (10,0 ; Build 14393)** ou une version ultérieure dans Visual Studio. Si votre application cible une version antérieure de Windows 10, vous devez utiliser l’espace de noms [Windows.ApplicationModel.Store](/uwp/api/windows.applicationmodel.store) à la place de l’espace de noms **Windows.Services.Store**. Pour plus d’informations, consultez [cet article](exclude-or-limit-features-in-a-trial-version-of-your-app.md).

## <a name="guidelines-for-implementing-a-trial-version"></a>Recommandations en matière d’implémentation d’une version d’évaluation

L’état de licence actuel de votre application est stocké dans les propriétés de la classe [StoreAppLicense](/uwp/api/windows.services.store.storeapplicense). D’une manière générale, vous placez les fonctions qui dépendent de l’état de la licence dans un bloc conditionnel, comme nous le décrivons à l’étape suivante. Lorsque vous développez ces fonctionnalités, assurez-vous de pouvoir les implémenter de telle manière qu’elles fonctionnent avec tous les états de la licence.

Décidez également comment vous voulez gérer les modifications apportées à la licence de l’application lorsque celle-ci est en cours d’exécution. La version d’évaluation de votre application peut être entièrement fonctionnelle, mais comporter des bandeaux publicitaires intégrés à l’application, contrairement à la version payante. La version d’évaluation de votre application peut également être privée de certaines fonctionnalités ou afficher régulièrement des messages demandant à l’utilisateur de l’acheter.

Pensez au type d’application que vous développez et demandez-vous quelle est la meilleure stratégie d’évaluation ou d’expiration. Pour la version d’évaluation d’un jeu, une bonne stratégie est de limiter le contenu du jeu auquel un utilisateur peut avoir accès. Dans le cas de la version d’évaluation d’un utilitaire, vous pouvez envisager de définir une date d’expiration ou de limiter les fonctionnalités qu’un acheteur potentiel peut utiliser.

Pour la plupart des applications qui ne sont pas des jeux, la définition d’une date d’expiration est une bonne méthode, car cela permet aux utilisateurs d’avoir une idée correcte de l’application complète. Voici quelques scénarios d’expiration classiques et comment les gérer.

-   **La licence d’évaluation expire tandis que l’application est en cours d’exécution.**

    Si la version d’évaluation expire tandis que votre application est en cours d’exécution, cette dernière peut :

    -   Ne rien faire.
    -   Afficher un message à l’attention de votre client.
    -   C’est presque ça.
    -   Inviter votre client à acheter l’application.

    La pratique recommandée est d’afficher un message invitant l’utilisateur à acheter l’application, et si c’est le cas, de poursuivre l’exécution avec toutes les fonctionnalités activées. Sinon, fermez l’application ou redemandez régulièrement à l’utilisateur s’il souhaite acheter l’application.

-   **La licence d’évaluation expire avant le lancement de l’application.**

    Si la version d’évaluation expire avant que l’utilisateur ne lance l’application, cette dernière ne démarre pas. Au lieu de cela, une boîte de dialogue s’affiche pour donner à l’utilisateur la possibilité d’acheter votre application dans le Windows Store.

-   **Le client achète l’application tandis que celle-ci est en cours d’exécution.**

    Si le client achète votre application tandis que celle-ci est en cours d’exécution, voici quelques actions possibles.

    -   Ne rien faire et continuer en mode d’évaluation jusqu’au redémarrage de l’application.
    -   Les remercier de leur achat ou afficher un message.
    -   Activer de manière silencieuse les fonctionnalités qui sont disponibles avec une licence complète (ou désactiver les notifications réservées à la version d’évaluation).

Prenez soin d’expliquer à vos clients comment votre application se comportera pendant et après la période d’évaluation gratuite afin qu’ils ne soient pas surpris. Pour plus d’informations sur la description de votre application, voir [Créer des descriptions d’application](../publish/create-app-store-listings.md).

## <a name="prerequisites"></a>Prérequis

La configuration requise pour cet exemple est la suivante :
* Projet Visual Studio pour une application plateforme Windows universelle (UWP) qui cible **Windows 10 édition anniversaire (10,0 ; Build 14393)** ou version ultérieure.
* Vous avez créé une application dans l’espace partenaires qui est configurée en tant qu' [évaluation gratuite](../publish/set-app-pricing-and-availability.md) sans limite de durée et cette application est publiée dans le Windows Store. Vous pouvez éventuellement configurer l’application afin qu’elle ne soit pas détectable dans le magasin pendant que vous la Testez. Pour plus d’informations, consultez notre [Guide de test](in-app-purchases-and-trials.md#testing).

Le code de cet exemple se base sur les hypothèses suivantes :
* Le code s’exécute dans le contexte d’une [Page](/uwp/api/windows.ui.xaml.controls.page) qui contient un [ProgressRing](/uwp/api/windows.ui.xaml.controls.progressring) nommé ```workingProgressRing``` et un [TextBlock](/uwp/api/windows.ui.xaml.controls.textblock) nommé ```textBlock```. Ces objets sont utilisés pour respectivement indiquer qu’une opération asynchrone est en cours et afficher les messages de sortie.
* Le fichier de code contient une instruction **using** pour l’espace de noms **Windows.Services.Store**.
* Cette application mono-utilisateur ne s’exécute que dans le contexte de l’utilisateur qui l’a lancée. Pour plus d’informations, consultez [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Si vous disposez d’une application de bureau qui utilise le [pont Desktop](https://developer.microsoft.com/windows/bridges/desktop), vous devrez peut-être ajouter du code supplémentaire non présenté dans cet exemple pour configurer l’objet [StoreContext](/uwp/api/windows.services.store.storecontext) . Pour plus d’informations, voir [Utilisation de la classe StoreContext dans une application de bureau qui utilise Desktop Bridge](in-app-purchases-and-trials.md#desktop).

## <a name="code-example"></a>Exemple de code

Lors du lancement de votre application, obtenez l’objet [StoreAppLicense](/uwp/api/windows.services.store.storeapplicense) de votre application et gérez l’événement [OfflineLicensesChanged](/uwp/api/windows.services.store.storecontext.offlinelicenseschanged) pour recevoir des notifications de modification de la licence pendant l’exécution de l’application. Par exemple, la licence de l’application peut changer si la période d’évaluation arrive à expiration ou si le client achète l’application par le biais du Windows Store. Quand la licence change, obtenez la nouvelle et activez ou désactivez une fonctionnalité de votre application en conséquence.

À ce stade, si un utilisateur a acheté l’application, il est de bonne pratique de lui indiquer que l’état de sa licence a été modifié. Demandez à l’utilisateur de redémarrer l’application si vous avez conçu votre application ainsi. Rendez cependant cette transition aussi simple que possible.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses_RS1/cs/ImplementTrialPage.xaml.cs" id="ImplementTrial":::

Pour obtenir un exemple d’application complète, consultez [Exemple Windows Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

## <a name="related-topics"></a>Rubriques connexes

* [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md)
* [Obtenir les informations produit des applications et des extensions](get-product-info-for-apps-and-add-ons.md)
* [Obtenir les informations de licence des applications et des modules complémentaires](get-license-info-for-apps-and-add-ons.md)
* [Activer les achats in-app d’applications et de modules complémentaires](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Activer les achats de modules complémentaires consommables](enable-consumable-add-on-purchases.md)
* [Exemple Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
