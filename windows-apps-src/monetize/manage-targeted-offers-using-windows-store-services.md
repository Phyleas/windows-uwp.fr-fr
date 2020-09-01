---
ms.assetid: 9F0A59A1-FAD7-4AD5-B78B-C1280F215D23
description: Utilisez l’API Microsoft Store ciblée pour obtenir des offres ciblées qui sont disponibles pour l’utilisateur actuel de votre application.
title: Gérer les offres ciblées à l’aide des services du Store
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP, services Store, Microsoft Store offres ciblées API, offres ciblées
ms.localizationpriority: medium
ms.openlocfilehash: 6cb429168e82419223f354bdb6548ab9a9e60dd1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155483"
---
# <a name="manage-targeted-offers-using-store-services"></a>Gérer les offres ciblées à l’aide des services du Store

Si vous créez une *offre ciblée* dans la page **participer > offres ciblées** de votre application dans l’espace partenaires, utilisez l' *API Microsoft Store ciblée offres* dans le code de votre application pour récupérer des informations qui vous aideront à implémenter l’expérience dans l’application pour l’offre ciblée. Pour plus d’informations sur les offres ciblées et la façon de les créer dans le tableau de bord, consultez [utiliser des offres ciblées pour optimiser l’engagement et les conversions](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md).

L’API offres ciblées est une API REST simple que vous pouvez utiliser pour obtenir les offres ciblées qui sont disponibles pour l’utilisateur actuel, selon que l’utilisateur fait ou non partie du segment du client pour l’offre ciblée. Pour utiliser cette API dans le code de votre application, procédez comme suit :

1.  [Obtenir un jeton de compte Microsoft](#obtain-a-microsoft-account-token) pour l’utilisateur actuellement connecté à votre application.
2.  [Obtenir les offres ciblées pour l’utilisateur actuel](#get-targeted-offers).
3.  Implémentez l’expérience d’achat dans l’application pour le module complémentaire associé à l’une des offres ciblées. Pour plus d’informations sur l’implémentation des achats dans l’application, consultez [cet article](enable-in-app-purchases-of-apps-and-add-ons.md).

Pour obtenir un exemple de code complet qui illustre toutes ces étapes, consultez l' [exemple de code](#code-example) à la fin de cet article. Les sections suivantes fournissent plus de détails sur chaque étape.

<span id="obtain-a-microsoft-account-token" />

## <a name="get-a-microsoft-account-token-for-the-current-user"></a>Obtenir un jeton de compte Microsoft pour l’utilisateur actuel

Dans le code de votre application, obtenir un jeton de compte Microsoft (MSA) pour l’utilisateur actuellement connecté. Vous devez passer ce jeton dans l' ```Authorization``` en-tête de demande pour le Microsoft Store API offres ciblées. Ce jeton est utilisé par le magasin pour récupérer les offres ciblées qui sont disponibles pour l’utilisateur actuel.

Pour obtenir le jeton MSA, utilisez la classe [WebAuthenticationCoreManager](/uwp/api/windows.security.authentication.web.core.webauthenticationcoremanager) pour demander un jeton à l’aide de la portée ```devcenter_implicit.basic,wl.basic``` . L'exemple suivant illustre la procédure à suivre pour réaliser cette opération. Cet exemple est un extrait de l' [exemple complet](#code-example)et requiert **l’utilisation** d’instructions fournies dans l’exemple complet.

[!code-csharp[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetMSAToken)]

Pour plus d’informations sur l’obtention des jetons MSA, consultez [Gestionnaire de comptes Web](../security/web-account-manager.md).

<span id="get-targeted-offers" />

## <a name="get-the-targeted-offers-for-the-current-user"></a>Obtenir les offres ciblées pour l’utilisateur actuel

Une fois que vous avez un jeton MSA pour l’utilisateur actuel, appelez la méthode d’extraction de l' ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` URI pour obtenir les offres ciblées disponibles pour l’utilisateur actuel. Pour plus d’informations sur cette méthode REST, consultez [obtenir des offres ciblées](get-targeted-offers.md).

Cette méthode retourne les ID de produit des modules complémentaires qui sont associés aux offres ciblées qui sont disponibles pour l’utilisateur actuel. Avec ces informations, vous pouvez proposer une ou plusieurs des offres ciblées en tant qu’achat dans l’application à l’utilisateur.

L’exemple suivant montre comment obtenir les offres ciblées pour l’utilisateur actuel. Cet exemple est un extrait de l' [exemple complet](#code-example). Il requiert la bibliothèque [JSON.net](https://www.newtonsoft.com/json) de Newtonsoft et des classes supplémentaires et **l’utilisation** des instructions fournies dans l’exemple complet.

[!code-csharp[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetTargetedOffers)]

<span id="code-example" />

## <a name="complete-code-example"></a>Exemple de code complet

L’exemple de code suivant illustre les tâches suivantes :

* Obtenir un jeton MSA pour l’utilisateur actuel.
* Obtenir toutes les offres ciblées pour l’utilisateur actuel à l’aide de la méthode [obtenir des offres ciblées](get-targeted-offers.md) .
* Achetez le module complémentaire associé à une offre ciblée.

Cet exemple requiert la bibliothèque [JSON.net](https://www.newtonsoft.com/json) de Newtonsoft. L’exemple utilise cette bibliothèque pour sérialiser et désérialiser des données au format JSON.

[!code-csharp[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetTargetedOffersSample)]

## <a name="related-topics"></a>Rubriques connexes

* [Utiliser des offres ciblées pour optimiser l’engagement et les conversions](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md)
* [Obtenir des offres ciblées](get-targeted-offers.md)