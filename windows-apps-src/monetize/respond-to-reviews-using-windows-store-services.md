---
ms.assetid: c92c0ea8-f742-4fc1-a3d7-e90aac11953e
description: Utilisez l’API Microsoft Store revues pour soumettre par programmation des réponses aux révisions de votre application dans le Windows Store.
title: Répondre aux révisions à l’aide des services du Store
ms.date: 06/04/2018
ms.topic: article
keywords: API Windows 10, UWP, Microsoft Store revues, répondre aux revues
ms.localizationpriority: medium
ms.openlocfilehash: 5b39ec67c4821b870a0f404e7199b69152b3a89c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174953"
---
# <a name="respond-to-reviews-using-store-services"></a>Répondre aux révisions à l’aide des services du Store

Utilisez l' *API de révisions de Microsoft Store* pour répondre par programmation aux révisions de votre application dans le Windows Store. Cette API est particulièrement utile pour les développeurs qui souhaitent répondre en bloc à de nombreuses révisions sans utiliser l’espace partenaires. Cette API utilise Azure Active Directory (Azure AD) pour authentifier les appels en provenance de votre application ou service.

Les étapes suivantes décrivent le processus de bout en bout :

1.  Vérifiez que vous avez rempli toutes les [conditions préalables](#prerequisites).
2.  Avant d’appeler une méthode dans l’API de révisions Microsoft Store, [obtenez un jeton d’accès Azure ad](#obtain-an-azure-ad-access-token). Une fois que vous avez obtenu un jeton, vous avez 60 minutes pour utiliser ce jeton dans les appels à l’API de révisions Microsoft Store avant l’expiration du jeton. Une fois le jeton arrivé à expiration, vous pouvez en générer un nouveau.
3.  [Appelez l’API de révisions de Microsoft Store](#call-the-windows-store-reviews-api).

> [!NOTE]
> Outre l’utilisation de l’API de révisions de Microsoft Store pour répondre par programmation aux révisions, vous pouvez également répondre aux révisions à [l’aide de l’espace partenaires](../publish/respond-to-customer-reviews.md).

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-reviews-api"></a>Étape 1 : remplir les conditions préalables à l’utilisation de l’API de révisions de Microsoft Store

Avant de commencer à écrire du code pour appeler l’API de révisions Microsoft Store, assurez-vous que vous avez rempli les conditions préalables suivantes.

* Vous (ou votre organisation) devez disposer d’un annuaire Azure AD et de l’autorisation [Administrateur général](/azure/active-directory/users-groups-roles/directory-assign-admin-roles) sur l’annuaire. Si vous utilisez déjà Microsoft 365 ou d’autres services professionnels de Microsoft, vous disposez déjà d’un annuaire Azure AD. Dans le cas contraire, vous pouvez [créer un nouveau Azure ad dans l’espace partenaires](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) sans frais supplémentaires.

* Vous devez associer une application Azure AD à votre compte espace partenaires, récupérer l’ID de locataire et l’ID client pour l’application et générer une clé. L’application Azure AD représente l’application ou le service à partir duquel vous souhaitez appeler l’API de révision Microsoft Store. Il vous faut l’ID tenant, l’ID client et la clé pour obtenir un jeton d’accès Azure AD à transmettre à l’API.
    > [!NOTE]
    > Vous ne devez effectuer cette tâche qu’une seule fois. Une fois que vous les avez, vous pouvez réutiliser l’ID tenant, l’ID client et la clé chaque fois que vous devez créer un jeton d’accès Azure AD.

Pour associer une application Azure AD à votre compte espace partenaires et récupérer les valeurs requises :

1.  Dans l’Espace partenaires, [associez le compte Espace partenaires de votre organisation à l’annuaire Azure AD de votre organisation](../publish/associate-azure-ad-with-partner-center.md).

2.  Ensuite, dans la page **utilisateurs** de la section **paramètres du compte** de l’espace partenaires, [Ajoutez l’application Azure ad](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) qui représente l’application ou le service que vous utiliserez pour répondre aux révisions. Veillez à attribuer à cette application le rôle **Gestionnaire**. Si l’application n’existe pas encore dans votre annuaire Azure AD, vous pouvez [créer une application Azure AD dans l’Espace partenaires](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account). 

3.  Revenez à la page **Utilisateurs**, cliquez sur le nom de votre application Azure AD pour accéder à ses paramètres, puis copiez les valeurs **ID tenant** et **ID client**.

4. Cliquez sur **Ajouter une nouvelle clé**. Sur l’écran suivant, copiez la valeur **Clé**. Vous ne pourrez plus accéder à cette information une fois que vous aurez quitté cette page. Pour plus d’informations, voir [Gérer les clés pour une application Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Étape 2 : Obtenir un jeton d’accès Azure AD

Avant d’appeler l’une des méthodes de l’API Microsoft Stores critiques, vous devez d’abord obtenir un jeton d’accès Azure AD que vous transmettez à l’en-tête **authorization** de chaque méthode de l’API. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez l’actualiser pour pouvoir continuer à l’utiliser dans d’autres appels à l’API.

Pour obtenir le jeton d’accès, suivez les instructions présentées dans l’article [Appels de service à service à l’aide des informations d’identification du client](/azure/active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow) pour envoyer une requête HTTP POST au point de terminaison ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```. Voici un exemple de requête.

```syntax
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

Pour la *valeur \_ ID de locataire* dans l’URI de publication et les paramètres * \_ ID client* et clé * \_ secrète client* , spécifiez l’ID de locataire, l’ID client et la clé de votre application que vous avez récupéré dans l’espace partenaires dans la section précédente. Pour le paramètre *resource*, vous devez spécifier ```https://manage.devcenter.microsoft.com```.

Une fois votre jeton d’accès arrivé à expiration, vous pouvez l’actualiser en suivant les instructions fournies [ici](/azure/active-directory/azuread-dev/v1-protocols-oauth-code#refreshing-the-access-tokens).

<span id="call-the-windows-store-reviews-api" />

## <a name="step-3-call-the-microsoft-store-reviews-api"></a>Étape 3 : appeler l’API de révisions de Microsoft Store

Une fois que vous disposez d’un jeton d’accès Azure AD, vous êtes prêt à appeler l’API de révision Microsoft Store. Vous devez transmettre le jeton d’accès à l’en-tête **Authorization** de chaque méthode.

L’API Microsoft Store Reviews contient plusieurs méthodes que vous pouvez utiliser pour déterminer si vous êtes autorisé à répondre à une révision donnée et à envoyer des réponses à une ou plusieurs analyses. Suivez ce processus pour utiliser cette API :

1. Obtenir les ID des révisions auxquelles vous souhaitez répondre. Les ID de révision sont disponibles dans les données de réponse de la méthode [obtenir les révisions](get-app-reviews.md) de l’application dans l’API Microsoft Store Analytics et dans le [Téléchargement hors connexion](../publish/download-analytic-reports.md) du [rapport de révisions](../publish/reviews-report.md).
2. Appelez la méthode [obtenir les informations de réponse pour les révisions d’application](get-response-info-for-app-reviews.md) pour déterminer si vous êtes autorisé à répondre aux révisions. Lorsqu’un client soumet une revue, il peut choisir de ne pas recevoir de réponse à sa révision. Vous ne pouvez pas répondre aux révisions soumises par des clients qui ont choisi de ne pas recevoir de réponses de révision.
3. Appelez la méthode [Envoyer des réponses aux révisions d’application](submit-responses-to-app-reviews.md) pour répondre par programmation aux révisions.


## <a name="related-topics"></a>Rubriques connexes

* [Obtenir les avis sur les applications](get-app-reviews.md)
* [Obtenir des informations de réponse pour les révisions d’application](get-response-info-for-app-reviews.md)
* [Envoyer des réponses aux révisions d’application](submit-responses-to-app-reviews.md)

 