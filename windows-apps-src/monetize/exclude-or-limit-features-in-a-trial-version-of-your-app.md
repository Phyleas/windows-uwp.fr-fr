---
description: Si vous donnez aux clients la possibilité d’utiliser votre application gratuitement pendant une période d’évaluation, vous pouvez leur donner envie de mettre à niveau vers la version complète de votre application en excluant ou en limitant certaines fonctionnalités pendant la période d’évaluation.
title: Exclure ou limiter des fonctionnalités de la version d’évaluation
ms.assetid: 1B62318F-9EF5-432A-8593-F3E095CA7056
keywords: Windows 10, UWP, version d’évaluation, achat dans l’application, IAP, Windows. ApplicationModel. Store
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 701769386f9637574cfb7f38f7458a1434796730
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033972"
---
# <a name="exclude-or-limit-features-in-a-trial-version"></a>Exclure ou limiter des fonctionnalités de la version d’évaluation

Si vous donnez aux clients la possibilité d’utiliser votre application gratuitement pendant une période d’évaluation, vous pouvez leur donner envie de mettre à niveau vers la version complète de votre application en excluant ou en limitant certaines fonctionnalités pendant la période d’évaluation. Choisissez les fonctionnalités à limiter avant de commencer à coder, puis faites en sorte que votre application ne les rende disponibles qu’à l’achat de la licence complète. Vous pouvez également activer certaines fonctionnalités, telles que des bannières ou des filigranes, qui ne s’afficheront que pendant la période d’évaluation, avant l’achat de votre application par un client.

> [!IMPORTANT]
> Cet article explique comment utiliser les membres de l’espace de noms [Windows. ApplicationModel. Store](/uwp/api/windows.applicationmodel.store) pour implémenter la fonctionnalité d’évaluation. Cet espace de noms n’est plus mis à jour avec les nouvelles fonctionnalités et nous vous recommandons d’utiliser à la place l’espace de noms [Windows. services. Store](/uwp/api/windows.services.store) . L’espace de noms **Windows. services. Store** prend en charge les types de compléments les plus récents, tels que les modules complémentaires et les abonnements consommables gérés par le magasin, et est conçu pour être compatible avec les futurs types de produits et fonctionnalités pris en charge par l’espace partenaires et le Store. L’espace de noms **Windows. services. Store** a été introduit dans Windows 10, version 1607, et il ne peut être utilisé que dans les projets qui ciblent l' **édition anniversaire windows 10 (10,0 ; Build 14393)** ou une version ultérieure dans Visual Studio. Pour plus d’informations sur l’implémentation de la fonctionnalité d’évaluation à l’aide de l’espace de noms **Windows. services. Store** , consultez [cet article](implement-a-trial-version-of-your-app.md).

## <a name="prerequisites"></a>Prérequis

Application Windows dans laquelle ajouter des fonctionnalités que les clients peuvent acheter.

## <a name="step-1-pick-the-features-you-want-to-enable-or-disable-during-the-trial-period"></a>Étape 1 : Sélection des fonctionnalités à activer ou désactiver durant la période d’évaluation

L’état actuel de la licence de votre application est stocké dans les propriétés de la classe [LicenseInformation](/uwp/api/Windows.ApplicationModel.Store.LicenseInformation). D’une manière générale, vous placez les fonctions qui dépendent de l’état de la licence dans un bloc conditionnel, comme nous le décrivons à l’étape suivante. Lorsque vous développez ces fonctionnalités, assurez-vous de pouvoir les implémenter de telle manière qu’elles fonctionnent avec tous les états de la licence.

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

Si vous voulez détecter la modification de la licence et exécuter une action quelconque dans votre application, vous devez ajouter un gestionnaire d’événements, comme décrit à l’étape suivante.

## <a name="step-2-initialize-the-license-info"></a>Étape 2 : Initialisation des informations de licence

Lors de l’initialisation de votre application, recherchez l’objet [LicenseInformation](/uwp/api/Windows.ApplicationModel.Store.LicenseInformation) de votre application, comme indiqué dans cet exemple. Nous supposons que **licenseInformation** est une variable globale ou un champ global de type **LicenseInformation** .

Pour le moment, vous obtenez des informations de licence simulées, en utilisant [CurrentAppSimulator](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) à la place de [CurrentApp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp). Avant de soumettre la version finale de votre application au **Windows Store** , vous devez remplacer tous les références **CurrentAppSimulator** par des références **CurrentApp** dans votre code.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/TrialVersion.cs" id="InitializeLicenseTest":::

Ensuite, ajoutez un gestionnaire d’événements pour recevoir des notifications en cas de modification de la licence lorsque l’application est en cours d’exécution. La licence de l’application peut notamment changer si la période d’évaluation arrive à expiration ou si le client achète l’application dans un Windows Store.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/TrialVersion.cs" id="InitializeLicenseTestWithEvent":::

## <a name="step-3-code-the-features-in-conditional-blocks"></a>Étape 3 : Coder les fonctionnalités dans des blocs conditionnels

Lorsque l’événement de changement de licence est déclenché, votre application doit appeler l’API de licence pour déterminer si l’état de la version d’évaluation a changé. Le code de cette étape indique comment structurer votre gestionnaire pour cet événement. À ce stade, si un utilisateur a acheté l’application, il est de bonne pratique de lui indiquer que l’état de sa licence a été modifié. Demandez à l’utilisateur de redémarrer l’application si vous avez conçu votre application ainsi. Rendez cependant cette transition aussi simple que possible.

Cet exemple montre comment évaluer l’état de la licence d’une application pour activer ou désactiver une fonctionnalité particulière de votre application.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/TrialVersion.cs" id="ReloadLicense":::

## <a name="step-4-get-an-apps-trial-expiration-date"></a>Étape 4 : Obtenir la date d’expiration de la version d’évaluation d’une application

Ajoutez le code permettant de déterminer la date d’expiration de la version d’évaluation de l’application.

Le code de cet exemple définit une fonction pour obtenir la date d’expiration de la licence pour la version d’évaluation de l’application. Si cette licence est toujours valide, affichez la date d’expiration avec le nombre de jours restants avant l’échéance.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/TrialVersion.cs" id="DisplayTrialVersionExpirationTime":::

## <a name="step-5-test-the-features-using-simulated-calls-to-the-license-api"></a>Étape 5 : Test des fonctionnalités à l’aide d’appels simulés à l’API de la licence

Maintenant, testez votre application à l’aide des données simulées. **CurrentAppSimulator** obtient les informations de licence spécifiques au test à partir d’un fichier XML appelé WindowsStoreProxy.xml, situé dans% UserProfile% \\ AppData \\ \\ packages locaux nom du \\ &lt; package &gt; \\ LocalState \\ Microsoft \\ Windows Store \\ ApiData. Vous pouvez modifier les dates d’expiration simulées de votre application et de ses fonctionnalités dans le fichier WindowsStoreProxy.xml. Testez l’ensemble de vos configurations d’expiration et de licence possibles, afin de vous assurer que tout fonctionne comme vous le souhaitez. Pour plus d’informations, consultez [Utilisation du fichier WindowsStoreProxy.xml avec CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy).

Si ce chemin d’accès et ce fichier n’existent pas, vous devez les créer lors de l’installation ou de l’exécution. Si vous essayez d’accéder à la propriété [CurrentAppSimulator.LicenseInformation](/uwp/api/windows.applicationmodel.store.currentappsimulator.licenseinformation) mais que le fichier WindowsStoreProxy.xml n’est pas présent à cet emplacement, une erreur se produit.

## <a name="step-6-replace-the-simulated-license-api-methods-with-the-actual-api"></a>Étape 6 : Remplacement des méthodes d’API de licence simulées par l’API réelle

Après avoir testé votre application à l’aide du serveur de licences simulées et avant d’envoyer votre application à un Windows Store à des fins de certification, remplacez **CurrentAppSimulator** par **CurrentApp** , comme indiqué dans l’exemple de code suivant.

> [!IMPORTANT]
> Votre application doit utiliser l’objet **CurrentApp** lorsque vous soumettez votre application à un magasin, ou la certification échouera.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/TrialVersion.cs" id="InitializeLicenseRetailWithEvent":::

## <a name="step-7-describe-how-the-free-trial-works-to-your-customers"></a>Étape 7 : Description du fonctionnement de la version d’évaluation à vos clients

Prenez soin d’expliquer à vos clients comment votre application se comportera pendant et après la période d’évaluation gratuite afin qu’ils ne soient pas surpris.

Pour plus d’informations sur la description de votre application, voir [Créer des descriptions d’application](../publish/create-app-store-listings.md).

## <a name="related-topics"></a>Rubriques connexes

* [Exemple du Windows Store (montre des versions d’évaluation et des achats in-app)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [Définir la tarification et la disponibilité d’une application](../publish/set-app-pricing-and-availability.md)
* [CurrentApp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp)
* [CurrentAppSimulator](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator)
 

 
