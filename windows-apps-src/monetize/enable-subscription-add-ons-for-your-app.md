---
description: Découvrez comment utiliser l’espace de noms Windows. services. Store pour implémenter des modules complémentaires d’abonnement.
title: Activer les extensions d’abonnement de votre application
keywords: Windows 10, UWP, abonnements, modules complémentaires, achats dans l’application, IAPs, Windows. services. Store
ms.date: 12/06/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 39f319d272e4dde465af68d4c5b7af7fb7a17799
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167713"
---
# <a name="enable-subscription-add-ons-for-your-app"></a>Activer les extensions d’abonnement de votre application

Votre application de plateforme Windows universelle (UWP) peut proposer des achats dans l’application de modules complémentaires d' *abonnement* à vos clients. Vous pouvez utiliser des abonnements pour vendre des produits numériques dans votre application (tels que des fonctionnalités d’application ou du contenu numérique) avec des périodes de facturation périodiques automatisées.

> [!NOTE]
> Pour permettre l’achat de modules complémentaires d’abonnement dans votre application, votre projet doit cibler l' **édition anniversaire Windows 10 (10,0 ; Build 14393)** ou une version ultérieure dans Visual Studio (cela correspond à Windows 10, version 1607) et il doit utiliser les API de l’espace de noms **Windows. services. Store** pour implémenter l’expérience d’achat dans l’application au lieu de l’espace de noms **Windows. ApplicationModel. Store** . Pour plus d’informations sur les différences entre ces espaces de noms, consultez [achats et évaluations dans l’application](in-app-purchases-and-trials.md).

## <a name="feature-highlights"></a>Présentation des fonctionnalités

Les modules complémentaires d’abonnement pour les applications UWP prennent en charge les fonctionnalités suivantes :

* Vous pouvez choisir entre des périodes d’abonnement de 1 mois, 3 mois, 6 mois, 1 an ou 2 ans.
* Vous pouvez ajouter des périodes d’essai gratuites de 1 semaine ou 1 mois à votre abonnement.
* Le SDK Windows [fournit des API](#code-examples) que vous pouvez utiliser dans votre application pour obtenir des informations sur les modules complémentaires d’abonnement disponibles pour l’application et activer l’achat d’un module complémentaire d’abonnement. Nous fournissons également des API REST que vous pouvez appeler à partir de vos services pour [gérer les abonnements d’un utilisateur](#manage-subscriptions).
* Vous pouvez afficher des rapports analytiques qui indiquent le nombre d’acquisitions d’abonnements, d’abonnés actifs et d’abonnements annulés au cours d’une période donnée.
* Les clients peuvent gérer leur abonnement sur la [https://account.microsoft.com/services](https://account.microsoft.com/services) page pour leur compte Microsoft. Les clients peuvent utiliser cette page pour afficher tous les abonnements qu’ils ont acquis, annuler un abonnement et modifier la forme de paiement associée à leur abonnement.

## <a name="steps-to-enable-a-subscription-add-on-for-your-app"></a>Procédure d’activation d’un module complémentaire d’abonnement pour votre application

Pour activer l’achat de modules complémentaires d’abonnement dans votre application, procédez comme suit.

1. [Créez une soumission de module complémentaire](../publish/add-on-submissions.md) pour votre abonnement dans l’espace partenaires et publiez l’envoi. Lorsque vous suivez le processus de soumission du module complémentaire, portez une attention particulière aux propriétés suivantes :

    * [Type de produit](../publish/set-your-add-on-product-id.md#product-type): Veillez à sélectionner **abonnement**.

    * [Période d’abonnement](../publish/enter-add-on-properties.md#subscription-period): choisissez la période de facturation récurrente pour votre abonnement. Vous ne pouvez pas modifier la période d’abonnement après avoir publié votre module complémentaire.

        Chaque module complémentaire d’abonnement prend en charge une période d’abonnement et une période d’essai uniques. Vous devez créer un module complémentaire d’abonnement différent pour chaque type d’abonnement que vous souhaitez proposer dans votre application. Par exemple, si vous souhaitez proposer un abonnement mensuel sans évaluation, un abonnement mensuel avec un essai d’un mois, un abonnement annuel sans essai et un abonnement annuel avec un essai d’un mois, vous devez créer quatre modules complémentaires d’abonnement.

    * [Période d’essai](../publish/enter-add-on-properties.md#free-trial): vous pouvez choisir une période d’essai d’une semaine ou d’un mois pour votre abonnement afin de permettre aux utilisateurs d’essayer le contenu de votre abonnement avant de l’acheter. Vous ne pouvez pas modifier ou supprimer la période d’évaluation après avoir publié votre module complémentaire d’abonnement.

        Pour obtenir une version d’évaluation gratuite de votre abonnement, un utilisateur doit acheter votre abonnement par le biais du processus d’achat dans l’application standard, y compris un formulaire de paiement valide. Ils ne sont facturés à aucun montant pendant la période d’évaluation. À la fin de la période d’évaluation, l’abonnement est automatiquement converti en abonnement complet et l’instrument de paiement de l’utilisateur est facturé pour la première période de l’abonnement payant. Si l’utilisateur choisit d’annuler son abonnement pendant la période d’évaluation, l’abonnement reste actif jusqu’à la fin de la période d’évaluation. Certaines périodes d’essai ne sont pas disponibles pour toutes les périodes d’abonnement.

        > [!NOTE]
        > Chaque client peut acquérir une version d’évaluation gratuite d’un module complémentaire d’abonnement une seule fois. Une fois qu’un client a acquis une version d’évaluation gratuite d’un abonnement, le magasin empêche le même client d’acquérir à nouveau le même abonnement d’essai gratuit.

    * [Visibilité](../publish/set-add-on-pricing-and-availability.md#visibility): Si vous créez un module complémentaire de test que vous utiliserez uniquement pour tester l’expérience d’achat dans l’application pour votre abonnement, nous vous recommandons de sélectionner l’un des éléments **masqués dans les options du Windows Store** . Dans le cas contraire, vous pouvez sélectionner la meilleure option de visibilité pour votre scénario.

    * [Tarification](../publish/set-add-on-pricing-and-availability.md?#pricing): choisissez le prix de votre abonnement dans cette section. Vous ne pouvez pas augmenter le prix de l’abonnement après avoir publié le module complémentaire. Toutefois, vous pouvez réduire le prix ultérieurement.
        > [!IMPORTANT]
        > Par défaut, lorsque vous créez un module complémentaire, le tarif est initialement défini sur **gratuit**. Étant donné que vous ne pouvez pas augmenter le prix d’un module complémentaire d’abonnement une fois l’envoi du module complémentaire terminé, veillez à choisir le prix de votre abonnement ici.

2. Dans votre application, utilisez les API de l’espace de noms [**Windows. services. Store**](/uwp/api/windows.services.store) pour déterminer si l’utilisateur actuel a déjà acquis votre module complémentaire d’abonnement, puis l’offrir pour vente à l’utilisateur en tant qu’achat dans l’application. Pour plus d’informations, consultez les [exemples de code](#code-examples) dans cet article.

3. Testez l’implémentation de l’achat dans l’application de votre abonnement dans votre application. Vous devrez télécharger votre application une fois à partir du Store sur votre appareil de développement pour utiliser sa licence de test. Pour plus d’informations, consultez notre [Guide de test](in-app-purchases-and-trials.md#testing) des achats dans l’application.  

4. Créez et publiez une soumission d’application qui inclut votre package d’application mis à jour, y compris votre code testé. Pour plus d’informations, consultez [envois d’applications](../publish/app-submissions.md).

<span id="code-examples"/>

## <a name="code-examples"></a>Exemples de code

Les exemples de code de cette section montrent comment utiliser les API de l’espace de noms [**Windows. services. Store**](/uwp/api/windows.services.store) pour obtenir des informations sur les modules complémentaires d’abonnement pour l’application actuelle et demander l’achat d’un module complémentaire d’abonnement pour le compte de l’utilisateur actuel.

Les conditions préalables de ces exemples sont les suivantes :
* Projet Visual Studio pour une application plateforme Windows universelle (UWP) qui cible **Windows 10 édition anniversaire (10,0 ; Build 14393)** ou version ultérieure.
* Vous avez [créé une soumission d’application](../publish/app-submissions.md) dans l’espace partenaires et cette application est publiée dans le Windows Store. Vous pouvez éventuellement configurer l’application afin qu’elle ne soit pas détectable dans le magasin pendant que vous la Testez. Pour plus d’informations, consultez les [conseils de test](in-app-purchases-and-trials.md#testing).
* Vous avez [créé un module complémentaire d’abonnement pour l’application dans l'](../publish/add-on-submissions.md) espace partenaires.

Le code dans les exemples suivants part du principe que :
* Le fichier de code **utilise** des instructions pour les espaces de noms **Windows. services. Store** et **System. Threading. Tasks** .
* Cette application mono-utilisateur ne s’exécute que dans le contexte de l’utilisateur qui l’a lancée. Pour plus d’informations, consultez [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Si vous disposez d’une application de bureau qui utilise le [pont Desktop](https://developer.microsoft.com/windows/bridges/desktop), vous devrez peut-être ajouter du code supplémentaire non présenté dans ces exemples pour configurer l’objet [**StoreContext**](/uwp/api/Windows.Services.Store.StoreContext) . Pour plus d’informations, voir [Utilisation de la classe StoreContext dans une application de bureau qui utilise Desktop Bridge](in-app-purchases-and-trials.md#desktop).

### <a name="purchase-a-subscription-add-on"></a>Acheter un module complémentaire d’abonnement

Cet exemple montre comment demander l’achat d’un module complémentaire d’abonnement connu pour votre application pour le compte du client actuel. Cet exemple montre également comment gérer le cas où l’abonnement a une période d’essai.

1. Le code détermine d’abord si le client dispose déjà d’une licence active pour l’abonnement. Si le client dispose déjà d’une licence active, votre code doit déverrouiller les fonctionnalités d’abonnement en fonction des besoins (car il est propriétaire de votre application, il est identifié par un commentaire dans l’exemple).
2. Ensuite, le code obtient l’objet [**StoreProduct**](/uwp/api/windows.services.store.storeproduct) qui représente l’abonnement que vous souhaitez acheter pour le compte du client. Le code suppose que vous connaissez déjà l' [ID de magasin](in-app-purchases-and-trials.md#store-ids) du module complémentaire d’abonnement que vous souhaitez acheter, et que vous avez affecté cette valeur à la variable *subscriptionStoreId* .
3. Le code détermine ensuite si une version d’évaluation est disponible pour l’abonnement. Si vous le souhaitez, votre application peut utiliser ces informations pour afficher des détails sur la version d’évaluation disponible ou l’abonnement complet au client.
4. Enfin, le code appelle la méthode [**RequestPurchaseAsync**](/uwp/api/windows.services.store.storeproduct.RequestPurchaseAsync) pour demander l’achat de l’abonnement. Si une version d’évaluation est disponible pour l’abonnement, la version d’évaluation sera proposée au client pour achat. Dans le cas contraire, l’abonnement complet sera proposé à l’achat.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[Subscriptions](./code/InAppPurchasesAndLicenses_RS1/cs/PurchaseSubscriptionAddOnTrialPage.xaml.cs#PurchaseTrialSubscription)]

### <a name="get-info-about-subscription-add-ons-for-the-current-app"></a>Obtenir des informations sur les modules complémentaires d’abonnement pour l’application actuelle

Cet exemple de code montre comment obtenir des informations pour tous les modules complémentaires d’abonnement disponibles dans votre application. Pour obtenir ces informations, utilisez d’abord la méthode [**GetAssociatedStoreProductsAsync**](/uwp/api/Windows.Services.Store.StoreContext.GetAssociatedStoreProductsAsync) pour obtenir la collection d’objets [**StoreProduct**](/uwp/api/Windows.Services.Store.StoreProduct) qui représentent chacun des modules complémentaires disponibles pour l’application. Ensuite, récupérez le [**StoreSku**](/uwp/api/windows.services.store.storesku) pour chaque produit et utilisez les propriétés [**IsSubscription**](/uwp/api/windows.services.store.storesku.IsSubscription) et [**SubscriptionInfo**](/uwp/api/windows.services.store.storesku.SubscriptionInfo) pour accéder aux informations d’abonnement.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[Subscriptions](./code/InAppPurchasesAndLicenses_RS1/cs/GetSubscriptionAddOnsPage.xaml.cs#GetSubscriptions)]

<span id="manage-subscriptions" />

## <a name="manage-subscriptions-from-your-services"></a>Gérer les abonnements à partir de vos services

Une fois que votre application mise à jour se trouve dans le Store et que les clients peuvent acheter votre module complémentaire d’abonnement, vous pouvez avoir des scénarios dans lesquels vous devez gérer l’abonnement pour un client. Nous fournissons des API REST que vous pouvez appeler à partir de vos services pour effectuer les tâches de gestion des abonnements suivantes :

* [Obtenir les abonnements qu’un utilisateur a le droit d’utiliser](get-subscriptions-for-a-user.md). Si votre abonnement fait partie d’un service multiplateforme, vous pouvez appeler cette API pour déterminer si l’utilisateur spécifié a droit à votre abonnement et l’état de son abonnement dans le contexte de votre application UWP. Vous pouvez ensuite utiliser ces informations pour mettre à jour l’état de l’abonnement sur d’autres plateformes prises en charge par votre service.

* [Modifier l’état de facturation d’un abonnement pour un utilisateur donné](change-the-billing-state-of-a-subscription-for-a-user.md). Utilisez cette API pour annuler, étendre ou désactiver le renouvellement automatique d’un abonnement.

## <a name="cancellations"></a>Annulations

Les clients peuvent utiliser la [https://account.microsoft.com/services](https://account.microsoft.com/services) page de leurs compte Microsoft pour afficher tous les abonnements qu’ils ont acquis, annuler un abonnement et modifier la forme de paiement associée à leur abonnement. Lorsqu’un client annule un abonnement à l’aide de cette page, il continue à avoir accès à l’abonnement pendant la période de facturation en cours. Ils ne sont pas remboursés pour une partie de la période de facturation actuelle. À la fin de la période de facturation en cours, leur abonnement est désactivé.

Vous pouvez également annuler un abonnement pour le compte d’un utilisateur à l’aide de notre API REST pour [modifier l’état de facturation d’un abonnement pour un utilisateur donné](change-the-billing-state-of-a-subscription-for-a-user.md).

## <a name="subscription-renewals-and-grace-periods"></a>Renouvellements d’abonnement et périodes de grâce

À un moment donné au cours de chaque période de facturation, nous essaierons de facturer la carte de crédit du client pour la prochaine période de facturation. Si les frais échouent, l’abonnement du client passe à l’état *rappels* . Cela signifie que leur abonnement est toujours actif pour le reste de la période de facturation actuelle, mais nous essaierons régulièrement de facturer leur carte de crédit pour renouveler automatiquement l’abonnement. Cet État peut durer jusqu’à deux semaines, jusqu’à la fin de la période de facturation actuelle et la date de renouvellement de la prochaine période de facturation.

Nous n’offrons pas de période de grâce pour la facturation de l’abonnement. Si nous ne parvenons pas à facturer la carte de crédit du client à la fin de la période de facturation actuelle, l’abonnement sera annulé et le client n’aura plus accès à l’abonnement après la période de facturation en cours.

## <a name="unsupported-scenarios"></a>Scénarios non pris en charge

Les scénarios suivants ne sont actuellement pas pris en charge pour les modules complémentaires d’abonnement.

* La vente d’abonnements à des clients directement via le Store n’est pas prise en charge pour le moment. Les abonnements sont disponibles pour les achats dans l’application des produits numériques uniquement.
* Les clients ne peuvent pas changer de période d’abonnement à l’aide [https://account.microsoft.com/services](https://account.microsoft.com/services) de la page de leur compte Microsoft. Pour passer à une période d’abonnement différente, les clients doivent annuler leur abonnement actuel, puis acheter un abonnement avec une période d’abonnement différente de celle de votre application.
* Le changement de niveau n’est actuellement pas pris en charge pour les modules complémentaires d’abonnement (par exemple, en basculant un client d’un abonnement de base à un abonnement Premium avec plus de fonctionnalités).
* Les codes de [ventes](../publish/put-apps-and-add-ons-on-sale.md) et de [promotions](../publish/generate-promotional-codes.md) ne sont actuellement pas pris en charge pour les modules complémentaires d’abonnement.
* Renouvellement des abonnements existants après avoir défini la visibilité de votre module complémentaire d’abonnement pour **arrêter l’acquisition**. Pour plus d’informations, consultez [définir la tarification et la disponibilité des modules complémentaires](../publish/set-add-on-pricing-and-availability.md) .

## <a name="related-topics"></a>Rubriques connexes

* [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md)
* [Obtenir les informations produit des applications et des extensions](get-product-info-for-apps-and-add-ons.md)