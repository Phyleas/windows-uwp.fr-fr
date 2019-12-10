---
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: Utilisez l’API Microsoft Store Analytics pour récupérer par programme les données d’analyse pour les applications qui sont inscrites auprès du compte de l’espace partenaires Windows de votre organisation ou de votre organisation.
title: Accéder aux données d’analyse à l’aide des services du Windows Store
ms.date: 03/06/2019
ms.topic: article
keywords: windows 10, uwp, services du Microsoft Store, API d'analyse du Microsoft Store
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 3b732da8f92c258647f905e6939dc3cb1b9c9f87
ms.sourcegitcommit: 3e47987fb4f86a6349ffe8262675f50971c77472
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74954063"
---
# <a name="access-analytics-data-using-store-services"></a>Accéder aux données d’analyse à l’aide des services du Windows Store

Utilisez l' *API Microsoft Store Analytics* pour récupérer par programme les données d’analyse pour les applications qui sont inscrites auprès du compte de l’espace partenaires Windows de votre organisation ou de votre organisation. Cette API permet de récupérer des données sur les acquisitions, les erreurs, les évaluations et les avis sur les applications et les extensions (également connues sous le nom PIA, produit in-app). Cette API utilise Azure Active Directory (Azure AD) pour authentifier les appels en provenance de votre application ou service.

Les étapes suivantes décrivent le processus de bout en bout :

1.  Vérifiez que vous avez rempli toutes les [conditions préalables](#prerequisites).
2.  Avant d’appeler une méthode dans l’API d’analyse du Microsoft Store, [procurez-vous un jeton d’accès Azure AD](#obtain-an-azure-ad-access-token). Une fois le jeton obtenu, vous avez 60 minutes pour l’utiliser dans les appels à l’API d’analyse du Microsoft Store avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en générer un nouveau.
3.  [Appelez l’API d’analyse du Microsoft Store](#call-the-windows-store-analytics-api).

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-analytics-api"></a>Étape : Remplir les conditions préalables à l’utilisation de l’API d’analyse du Microsoft Store

Avant d’écrire le code d’appel de l’API d’analyse du Microsoft Store, vérifiez que vous remplissez bien les conditions préalables suivantes.

* Vous (ou votre organisation) devez disposer d’un annuaire Azure AD et d’une autorisation [Administrateur global](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) pour l’annuaire. Si vous utilisez déjà Office 365 ou d’autres services professionnels de Microsoft, vous disposez déjà d’un annuaire Azure AD. Dans le cas contraire, vous pouvez [créer un nouveau Azure ad dans l’espace partenaires](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) sans frais supplémentaires.

* Vous devez associer une application Azure AD à votre compte espace partenaires, récupérer l’ID de locataire et l’ID client pour l’application et générer une clé. L’application Azure AD est l’app ou le service à partir duquel vous allez appeler l’API d’analyse du Microsoft Store. Vous avez besoin de l’ID de locataire, de l’ID client et de la clé pour obtenir le jeton d’accès Azure AD à transmettre à l’API.
    > [!NOTE]
    > Cette tâche ne doit être effectuée qu’une seule fois. Une fois que vous avez l’ID de locataire, l’ID client et la clé à disposition, vous pouvez les réutiliser chaque fois que vous avez besoin de créer un nouveau jeton d’accès Azure AD.

Pour associer une application Azure AD à votre compte espace partenaires et récupérer les valeurs requises :

1.  Dans l’espace partenaires, [associez le compte de l’espace partenaires de votre organisation au répertoire Azure AD de votre organisation](../publish/associate-azure-ad-with-partner-center.md).

2.  Ensuite, dans la page **utilisateurs** de la section **paramètres du compte** de l’espace partenaires, [Ajoutez l’application Azure ad](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) qui représente l’application ou le service que vous allez utiliser pour accéder aux données d’analyse de votre compte espace partenaires. Assurez-vous d'attribuer à cette application le rôle **Manager**. Si l’application n’existe pas encore dans votre répertoire Azure AD, vous pouvez [créer une nouvelle application Azure ad dans l’espace partenaires](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account).

3.  Revenez à la page **Utilisateurs**, cliquez sur le nom de votre application Azure AD pour accéder aux paramètres de l’application, puis notez les valeurs des champs **ID de locataire** et **ID client**.

4. Cliquez sur **Ajouter une clé**. Dans l’écran suivant, notez la valeur du champ **Clé**. Vous ne pourrez plus accéder à ces informations une fois que vous aurez quitté cette page. Pour plus d’informations, voir [Gérer les clés pour une application Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Étape 2 : Obtenir un jeton d’accès Azure AD

Avant d’appeler l’une des méthodes dans l’API d’analyse du Microsoft Store, vous devez d’abord obtenir un jeton d’accès Azure AD pour le passer à l’en-tête **Autorisation** de chaque méthode de l’API. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez l’actualiser pour pouvoir continuer à l’utiliser dans d’autres appels à l’API.

Pour obtenir le jeton d’accès, suivez les instructions présentées dans l’article [Appels de service à service à l’aide des informations d’identification du client](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/) pour envoyer une requête HTTP POST au point de terminaison ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```. Voici un exemple de requête.

```json
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

Pour la valeur de l' *ID de\_* du client dans l’URI de publication et les paramètres client *\_ID* et *\_de secret client* , spécifiez l’ID de locataire, l’ID client et la clé de votre application que vous avez récupérée dans l’espace partenaires dans la section précédente. Pour le paramètre *resource*, vous devez spécifier ```https://manage.devcenter.microsoft.com```.

Une fois votre jeton d’accès arrivé à expiration, vous pouvez l’actualiser en suivant les instructions fournies [ici](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens).

<span id="call-the-windows-store-analytics-api" />

## <a name="step-3-call-the-microsoft-store-analytics-api"></a>Étape 3 : Appeler l’API d’analyse du Microsoft Store

Une fois que vous disposez d’un jeton d’accès Azure AD, vous pouvez appeler l’API d’analyse du Microsoft Store. Vous devez transmettre le jeton d’accès à l’en-tête **Authorization** de chaque méthode.

### <a name="methods-for-uwp-apps-and-games"></a>Méthodes pour les applications et les jeux UWP
Les méthodes suivantes sont disponibles pour les acquisitions d’applications et de jeux et les acquisitions de module complémentaire : 

* [Obtenir des données d’acquisitions pour vos jeux et applications](acquisitions-data.md)
* [Obtenir des données d’acquisition de module complémentaire pour vos jeux et applications](add-on-acquisitions-data.md)

### <a name="methods-for-uwp-apps"></a>Méthodes d'analyse des apps UWP 

Les méthodes d’analyse suivantes sont disponibles pour les applications UWP dans l’espace partenaires.

| Scénario       | Méthodes      |
|---------------|--------------------|
| Acquisitions, conversions, installations et utilisation |  <ul><li>[Obtention d’acquisitions d’applications](get-app-acquisitions.md) (héritées)</li><li>[Obtention des données de synthèse](get-acquisition-funnel-data.md) de l’acquisition d’applications (hérité)</li><li>[Récupération des conversions d’applications par canal](get-app-conversions-by-channel.md)</li><li>[Obtention des acquisitions du module complémentaire](get-in-app-acquisitions.md)</li><li>[Obtention d’acquisitions du module complémentaire d’abonnement](get-subscription-acquisitions.md)</li><li>[Obtient les conversions de module complémentaire par canal](get-add-on-conversions-by-channel.md)</li><li>[Télécharger les installations d’applications](get-app-installs.md)</li><li>[Recevoir l’utilisation quotidienne des applications](get-app-usage-daily.md)</li><li>[Recevoir l’utilisation mensuelle des applications](get-app-usage-monthly.md)</li></ul> |
| Erreurs d’application | <ul><li>[Obtient les données de signalement d’erreurs](get-error-reporting-data.md)</li><li>[Obtenir les détails d’une erreur dans votre application](get-details-for-an-error-in-your-app.md)</li><li>[Obtenir la trace de la pile pour une erreur dans votre application](get-the-stack-trace-for-an-error-in-your-app.md)</li><li>[Télécharger le fichier CAB pour une erreur dans votre application](download-the-cab-file-for-an-error-in-your-app.md)</li></ul> |
| Insights | <ul><li>[Obtenir des données Insights pour votre application](get-insights-data-for-your-app.md)</li></ul>  |
| Évaluations et avis | <ul><li>[Récupération des évaluations d’applications](get-app-ratings.md)</li><li>[Recevoir des révisions d’application](get-app-reviews.md)</li></ul> |
| Publicités dans l'application et campagnes publicitaires | <ul><li>[Obtient les données de performances ad](get-ad-performance-data.md)</li><li>[Obtient les données de performances de la campagne Active Directory](get-ad-campaign-performance-data.md)</li></ul> |

### <a name="methods-for-desktop-applications"></a>Méthodes pour les applications de bureau

Les méthodes d'analyse suivantes peuvent être utilisées par des comptes de développeur appartenant au [programme d'application de bureau Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program).

| Scénario       | Méthodes      |
|---------------|--------------------|
| Installations |  <ul><li>[Accéder aux installations d’applications de bureau](get-desktop-app-installs.md)</li></ul> |
| Blocks |  <ul><li>[Obtenir des blocs de mise à niveau pour votre application de bureau](get-desktop-block-data.md)</li><li>[Obtenir les détails du bloc de mise à niveau pour votre application de bureau](get-desktop-block-data-details.md)</li></ul> |
| Erreurs des applications |  <ul><li>[Obtenir des données de rapports d’erreurs pour votre application de bureau](get-desktop-application-error-reporting-data.md)</li><li>[Obtenir les détails d’une erreur dans votre application de bureau](get-details-for-an-error-in-your-desktop-application.md)</li><li>[Obtenir la trace de la pile pour une erreur dans votre application de bureau](get-the-stack-trace-for-an-error-in-your-desktop-application.md)</li><li>[Télécharger le fichier CAB pour une erreur dans votre application de bureau](download-the-cab-file-for-an-error-in-your-desktop-application.md)</li></ul> |
| Insights | <ul><li>[Obtenir des données Insights pour votre application de bureau](get-insights-data-for-your-desktop-app.md)</li></ul>  |

### <a name="methods-for-xbox-live-services"></a>Méthodes pour les services Xbox Live

Les méthodes supplémentaires suivantes sont disponibles pour les comptes de développeur, avec les jeux utilisant les [services Xbox Live ](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview.md).

| Scénario       | Méthodes      |
|---------------|--------------------|
| Analyse générale |  <ul><li>[Recevoir des données Xbox Live Analytics](get-xbox-live-analytics.md)</li><li>[Recevoir des données de résultats Xbox Live](get-xbox-live-achievements-data.md)</li><li>[Recevoir des données sur l’utilisation simultanée de Xbox Live](get-xbox-live-concurrent-usage-data.md)</li></ul> |
| Analyse de l’intégrité |  <ul><li>[Recevoir des données d’intégrité Xbox Live](get-xbox-live-health-data.md)</li></ul> |
| Analyses de la communauté |  <ul><li>[Recevoir des données du Hub de jeu Xbox Live](get-xbox-live-game-hub-data.md)</li><li>[Recevoir des données de Club Xbox Live](get-xbox-live-club-data.md)</li><li>[Recevoir des données multijoueurs Xbox Live](get-xbox-live-multiplayer-data.md)</li></ul>  |

### <a name="methods-for-hardware-and-drivers"></a>Méthodes pour le matériel et les pilotes

Les comptes de développeurs qui appartiennent au [programme de tableau de bord matériel Windows](https://docs.microsoft.com/windows-hardware/drivers/dashboard/get-started-with-the-hardware-dashboard) ont accès à un ensemble supplémentaire de méthodes pour récupérer des données d’analyse pour le matériel et les pilotes. Pour plus d’informations, consultez [API du tableau de bord matériel](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-api).

## <a name="code-example"></a>Exemple de code

L’exemple de code suivant montre comment obtenir un jeton d’accès Azure AD et appeler l’API d’analyse du Microsoft Store à partir d’une app de console C#. Pour utiliser cet exemple de code, affectez les variables *tenantId*, *clientId*, *clientSecret*, et *appID* aux valeurs appropriées pour votre scénario. Cet exemple requiert le [package Json.NET](https://www.newtonsoft.com/json) de Newtonsoft afin de désérialiser les données JSON renvoyées par l’API d’analyse du Microsoft Store.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[AnalyticsApi](./code/StoreServicesExamples_Analytics/cs/Program.cs#AnalyticsApiExample)]

## <a name="error-responses"></a>Réponses d’erreur

L’API d’analyse du Microsoft Store renvoie les réponses d’erreur dans un objet JSON qui contient des messages et des codes d’erreur. L’exemple suivant montre une réponse d’erreur causée par un paramètre non valide.

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
