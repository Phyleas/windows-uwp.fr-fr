---
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: Utilisez l’API Microsoft Store Analytics pour récupérer par programme les données d’analyse pour les applications qui sont inscrites auprès du compte de l’espace partenaires Windows de votre organisation ou de votre organisation.
title: Accéder aux données d’analyse à l’aide des services Store
ms.date: 03/06/2019
ms.topic: article
keywords: Windows 10, UWP, services Store, API Microsoft Store Analytics
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: bf83105dc3b5a49746e0fb6e1d01db4accdb5c41
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363622"
---
# <a name="access-analytics-data-using-store-services"></a>Accéder aux données d’analyse à l’aide des services Store

Utilisez l' *API Microsoft Store Analytics* pour récupérer par programme les données d’analyse pour les applications qui sont inscrites auprès du compte de l’espace partenaires Windows de votre organisation ou de votre organisation. Cette API permet de récupérer des données sur les acquisitions, les erreurs, les évaluations et les avis sur les applications et les extensions (également connues sous le nom PIA, produit in-app). Cette API utilise Azure Active Directory (Azure AD) pour authentifier les appels en provenance de votre application ou service.

Les étapes suivantes décrivent le processus de bout en bout :

1.  Vérifiez que vous avez rempli toutes les [conditions préalables](#prerequisites).
2.  Avant d’appeler une méthode dans l’API Microsoft Store Analytics, [obtenez un jeton d’accès Azure ad](#obtain-an-azure-ad-access-token). Une fois que vous avez obtenu un jeton, vous avez 60 minutes pour utiliser ce jeton dans les appels à l’API Microsoft Store Analytics avant l’expiration du jeton. Une fois le jeton arrivé à expiration, vous pouvez en générer un nouveau.
3.  [Appelez l’API Microsoft Store Analytics](#call-the-windows-store-analytics-api).

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-analytics-api"></a>Étape 1 : remplir les conditions préalables à l’utilisation de l’API Microsoft Store Analytics

Avant de commencer à écrire du code pour appeler l’API Microsoft Store Analytics, assurez-vous que vous avez rempli les conditions préalables suivantes.

* Vous (ou votre organisation) devez disposer d’un annuaire Azure AD et de l’autorisation [Administrateur général](/azure/active-directory/users-groups-roles/directory-assign-admin-roles) sur l’annuaire. Si vous utilisez déjà Microsoft 365 ou d’autres services professionnels de Microsoft, vous disposez déjà d’un annuaire Azure AD. Dans le cas contraire, vous pouvez [créer un nouveau Azure ad dans l’espace partenaires](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) sans frais supplémentaires.

* Vous devez associer une application Azure AD à votre compte espace partenaires, récupérer l’ID de locataire et l’ID client pour l’application et générer une clé. L’application Azure AD représente l’application ou le service à partir duquel vous souhaitez appeler l’API Microsoft Store Analytics. Il vous faut l’ID tenant, l’ID client et la clé pour obtenir un jeton d’accès Azure AD à transmettre à l’API.
    > [!NOTE]
    > Vous ne devez effectuer cette tâche qu’une seule fois. Une fois que vous les avez, vous pouvez réutiliser l’ID tenant, l’ID client et la clé chaque fois que vous devez créer un jeton d’accès Azure AD.

Pour associer une application Azure AD à votre compte espace partenaires et récupérer les valeurs requises :

1.  Dans l’Espace partenaires, [associez le compte Espace partenaires de votre organisation à l’annuaire Azure AD de votre organisation](../publish/associate-azure-ad-with-partner-center.md).

2.  Ensuite, dans la page **utilisateurs** de la section **paramètres du compte** de l’espace partenaires, [Ajoutez l’application Azure ad](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) qui représente l’application ou le service que vous allez utiliser pour accéder aux données d’analyse de votre compte espace partenaires. Veillez à attribuer à cette application le rôle **Gestionnaire**. Si l’application n’existe pas encore dans votre annuaire Azure AD, vous pouvez [créer une application Azure AD dans l’Espace partenaires](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account).

3.  Revenez à la page **Utilisateurs**, cliquez sur le nom de votre application Azure AD pour accéder à ses paramètres, puis copiez les valeurs **ID tenant** et **ID client**.

4. Cliquez sur **Ajouter une nouvelle clé**. Sur l’écran suivant, copiez la valeur **Clé**. Vous ne pourrez plus accéder à cette information une fois que vous aurez quitté cette page. Pour plus d’informations, voir [Gérer les clés pour une application Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Étape 2 : Obtenir un jeton d’accès Azure AD

Avant d’appeler l’une des méthodes de l’API Microsoft Store Analytics, vous devez d’abord obtenir un jeton d’accès Azure AD que vous transmettez à l’en-tête **authorization** de chaque méthode de l’API. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez l’actualiser pour pouvoir continuer à l’utiliser dans d’autres appels à l’API.

Pour obtenir le jeton d’accès, suivez les instructions présentées dans l’article [Appels de service à service à l’aide des informations d’identification du client](/azure/active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow) pour envoyer une requête HTTP POST au point de terminaison ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```. Voici un exemple de requête.

```json
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

<span id="call-the-windows-store-analytics-api" />

## <a name="step-3-call-the-microsoft-store-analytics-api"></a>Étape 3 : appeler l’API Microsoft Store Analytics

Une fois que vous disposez d’un jeton d’accès Azure AD, vous êtes prêt à appeler l’API Microsoft Store Analytics. Vous devez transmettre le jeton d’accès à l’en-tête **Authorization** de chaque méthode.

### <a name="methods-for-uwp-apps-and-games"></a>Méthodes pour les applications et les jeux UWP
Les méthodes suivantes sont disponibles pour les acquisitions d’applications et de jeux et les acquisitions de module complémentaire : 

* [Obtenir des données d’acquisitions pour vos applications et vos jeux](acquisitions-data.md)
* [Obtenir des données d’acquisitions d’extension pour vos applications et vos jeux](add-on-acquisitions-data.md)

### <a name="methods-for-uwp-apps"></a>Méthodes pour les applications UWP 

Les méthodes d’analyse suivantes sont disponibles pour les applications UWP dans l’espace partenaires.

| Scénario       | Méthodes      |
|---------------|--------------------|
| Acquisitions, conversions, installations et utilisation |  <ul><li>[Obtention d’acquisitions d’applications](get-app-acquisitions.md) (héritées)</li><li>[Obtention des données de synthèse](get-acquisition-funnel-data.md) de l’acquisition d’applications (hérité)</li><li>[Obtenir les conversions d’applications par canal](get-app-conversions-by-channel.md)</li><li>[Obtenir des acquisitions d’extensions](get-in-app-acquisitions.md)</li><li>[Obtenir les acquisitions d’extensions d’inscription](get-subscription-acquisitions.md)</li><li>[Obtenir les conversions d’extensions par canal](get-add-on-conversions-by-channel.md)</li><li>[Obtenir les installations d’applications](get-app-installs.md)</li><li>[Obtenir l’utilisation quotidienne des applications](get-app-usage-daily.md)</li><li>[Obtenir l’utilisation mensuelle des applications](get-app-usage-monthly.md)</li></ul> |
| Erreurs d’application | <ul><li>[Obtenir les données de rapport d’erreurs](get-error-reporting-data.md)</li><li>[Obtenir les informations sur une erreur de votre application](get-details-for-an-error-in-your-app.md)</li><li>[Obtenir la trace de pile concernant une erreur dans votre application](get-the-stack-trace-for-an-error-in-your-app.md)</li><li>[Télécharger le fichier CAB concernant une erreur dans votre application](download-the-cab-file-for-an-error-in-your-app.md)</li></ul> |
| Insights | <ul><li>[Obtenir les données d’insights pour votre application](get-insights-data-for-your-app.md)</li></ul>  |
| Évaluations et avis | <ul><li>[Obtenir les classifications des applications](get-app-ratings.md)</li><li>[Obtenir les avis sur les applications](get-app-reviews.md)</li></ul> |
| Annonces dans l’application et campagnes publicitaires | <ul><li>[Obtenir les données relatives aux performances publicitaires](get-ad-performance-data.md)</li><li>[Obtenir les données relatives aux performances de la campagne publicitaire](get-ad-campaign-performance-data.md)</li></ul> |

### <a name="methods-for-desktop-applications"></a>Méthodes pour les applications de bureau

Les méthodes d’analyse suivantes peuvent être utilisées par les comptes de développeurs qui appartiennent au [programme d’application de bureau Windows](/windows/desktop/appxpkg/windows-desktop-application-program).

| Scénario       | Méthodes      |
|---------------|--------------------|
| Installe |  <ul><li>[Obtenir les installations d’applications de bureau](get-desktop-app-installs.md)</li></ul> |
| Blocs |  <ul><li>[Obtenir des blocs de mise à niveau pour votre application de bureau](get-desktop-block-data.md)</li><li>[Obtenir des informations concernant les blocs de mise à niveau pour votre application de bureau](get-desktop-block-data-details.md)</li></ul> |
| Erreurs d’application |  <ul><li>[Obtenir les données de signalement d’erreurs pour votre application de bureau](get-desktop-application-error-reporting-data.md)</li><li>[Obtenir les informations sur une erreur de votre application de bureau](get-details-for-an-error-in-your-desktop-application.md)</li><li>[Obtenir la trace de pile concernant une erreur dans votre application de bureau](get-the-stack-trace-for-an-error-in-your-desktop-application.md)</li><li>[Télécharger le fichier CAB concernant une erreur dans votre application de bureau](download-the-cab-file-for-an-error-in-your-desktop-application.md)</li></ul> |
| Insights | <ul><li>[Obtenir des données d'insights pour votre application de bureau](get-insights-data-for-your-desktop-app.md)</li></ul>  |

### <a name="methods-for-xbox-live-services"></a>Méthodes pour les services Xbox Live

Les autres méthodes suivantes peuvent être utilisées par les comptes de développeurs avec des jeux qui utilisent les [services Xbox Live](/gaming/xbox-live/developer-program-overview.md).

| Scénario       | Méthodes      |
|---------------|--------------------|
| Analyse générale |  <ul><li>[Obtenir des données d’analyse Xbox Live](get-xbox-live-analytics.md)</li><li>[Obtenir des données de réussite Xbox Live](get-xbox-live-achievements-data.md)</li><li>[Obtenir les données d’utilisation simultanée Xbox Live](get-xbox-live-concurrent-usage-data.md)</li></ul> |
| Analyse d’intégrité |  <ul><li>[Obtenir des données d’intégrité Xbox Live](get-xbox-live-health-data.md)</li></ul> |
| Analytique de la communauté |  <ul><li>[Obtenir les données du hub de jeux Xbox Live](get-xbox-live-game-hub-data.md)</li><li>[Obtenir des données de club Xbox Live](get-xbox-live-club-data.md)</li><li>[Obtenir des données multijoueur Xbox Live](get-xbox-live-multiplayer-data.md)</li></ul>  |

### <a name="methods-for-hardware-and-drivers"></a>Méthodes pour le matériel et les pilotes

Les comptes de développeurs qui appartiennent au [programme de tableau de bord matériel Windows](/windows-hardware/drivers/dashboard/get-started-with-the-hardware-dashboard) ont accès à un ensemble supplémentaire de méthodes pour récupérer des données d’analyse pour le matériel et les pilotes. Pour plus d’informations, consultez [API du tableau de bord matériel](/windows-hardware/drivers/dashboard/dashboard-api).

## <a name="code-example"></a>Exemple de code

L’exemple de code suivant montre comment obtenir un jeton d’accès Azure AD et appeler l’API Microsoft Store Analytics à partir d’une application console C#. Pour utiliser cet exemple de code, affectez les variables *tenantId*, *clientId*, *clientSecret*, et *appID* aux valeurs appropriées pour votre scénario. Cet exemple nécessite que le [package JSON.net](https://www.newtonsoft.com/json) de Newtonsoft désérialise les données JSON retournées par l’API Microsoft Store Analytics.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Analytics/cs/Program.cs" id="AnalyticsApiExample":::

## <a name="error-responses"></a>Réponses d’erreur

L’API Microsoft Store Analytics retourne les réponses d’erreur dans un objet JSON qui contient les codes d’erreur et les messages. L’exemple suivant montre une réponse d’erreur causée par un paramètre non valide.

```json
{
    "code":"BadRequest",
    "data":[],
    "details":[],
    "innererror":{
        "code":"InvalidQueryParameters",
        "data":[
            "top parameter cannot be more than 10000"
        ],
        "details":[],
        "message":"One or More Query Parameters has invalid values.",
        "source":"AnalyticsAPI"
    },
    "message":"The calling client sent a bad request to the service.",
    "source":"AnalyticsAPI"
}
```
