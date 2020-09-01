---
description: Utilisez cette méthode dans l’API Microsoft Store Analytics pour récupérer les données d’utilisation simultanée de Xbox Live.
title: Obtenir les données d’utilisation simultanée Xbox Live
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, services Store, API Microsoft Store Analytics, Xbox Live Analytics, utilisation simultanée
ms.localizationpriority: medium
ms.openlocfilehash: 9e8d36ce9336d5fc65a73a19233b8a1f0a05a8bf
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167583"
---
# <a name="get-xbox-live-concurrent-usage-data"></a>Obtenir les données d’utilisation simultanée Xbox Live


Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir des données d’utilisation en temps quasi réel (avec une latence de 5-15 minutes) sur le nombre moyen de clients qui lisent votre [jeu Xbox Live](/gaming/xbox-live/index.md) toutes les minutes, toutes les heures ou tous les jours pendant une période spécifiée. Ces informations sont également disponibles dans le [rapport Xbox Analytics](../publish/xbox-analytics-report.md) dans l’espace partenaires.

> [!IMPORTANT]
> Cette méthode prend uniquement en charge les jeux pour Xbox ou les jeux qui utilisent les services Xbox Live. Ces jeux doivent passer par le [processus d’approbation du concept](../gaming/concept-approval.md), qui comprend les jeux publiés par les [partenaires Microsoft](/gaming/xbox-live/developer-program-overview.md#microsoft-partners) et les jeux soumis par le biais du [ ID@Xbox programme](/gaming/xbox-live/developer-program-overview.md#id). Actuellement, cette méthode ne prend pas en charge les jeux publiés par le biais du [programme Xbox Live Creators](/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md).

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) pour l’API Microsoft Store Analytics.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de demande       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la demande


| Paramètre        | Type   |  Description      |  Obligatoire  
|---------------|--------|---------------|------|
| applicationId | string | [ID de stockage](in-app-purchases-and-trials.md#store-ids) du jeu pour lequel vous souhaitez récupérer les données d’utilisation simultanée Xbox Live.  |  Oui  |
| metricType | string | Chaîne qui spécifie le type de données Xbox Live Analytics à récupérer. Pour cette méthode, spécifiez la valeur **concurrentielle**.  |  Oui  |
| startDate | Date | Date de début dans la plage de dates des données d’utilisation simultanées à récupérer. Consultez la description de *aggregationLevel* pour connaître le comportement par défaut. |  Non  |
| endDate | Date | Date de fin dans la plage de dates des données d’utilisation simultanées à récupérer. Consultez la description de *aggregationLevel* pour connaître le comportement par défaut. |  Non  |
| aggregationLevel | string | Indique la plage de temps pendant laquelle récupérer les données agrégées. Il peut s’agir de l’une des chaînes suivantes : **minute**, **Hour**ou **Day**. Par défaut, la valeur est **day**. <p/><p/>Si vous ne spécifiez pas *StartDate* ou *EndDate*, le corps de la réponse est défini par défaut comme suit : <ul><li>**minute**: les 60 derniers enregistrements de données disponibles.</li><li>**heure**: les 24 derniers enregistrements des données disponibles.</li><li>**Day**: les 7 derniers enregistrements des données disponibles.</li></ul><p/>Les niveaux d’agrégation suivants ont des limites de taille sur le nombre d’enregistrements qui peuvent être retournés. Les enregistrements seront tronqués si l’intervalle de temps demandé est trop important. <ul><li>**minute**: jusqu’à 1440 enregistrements (24 heures de données).</li><li>**heure**: jusqu’à 720 enregistrements (30 jours de données).</li><li>**jour**: jusqu’à 60 enregistrements (60 jours de données).</li></ul>  |  Non  |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre une demande d’obtention de données d’utilisation simultanées pour votre jeu Xbox Live. Cette demande récupère des données toutes les minutes entre le 1 2018 février et le 2 2018 février. Remplacez la valeur *ApplicationID* par l’ID du magasin de votre jeu.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=concurrency&aggregationLevel=hour&startDate=2018-02-01&endData=2018-02-02 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>response

Le corps de la réponse contient un tableau d’objets contenant chacun un ensemble de données d’utilisation simultanées pour une minute, une heure ou un jour spécifié. Chaque objet contient les valeurs suivantes.

| Valeur      | Type   | Description                  |
|------------|--------|-------------------------------------------------------|
| Count      | nombre  | Nombre moyen de clients qui lisent votre Xbox en direct pour la minute, l’heure ou le jour spécifiés. <p/><p/>**Note** &nbsp; Remarque &nbsp; La valeur 0 indique qu’il n’y avait pas d’utilisateurs simultanés pendant l’intervalle spécifié, ou qu’il y a eu une erreur lors de la collecte des données utilisateur simultanées pour le jeu pendant l’intervalle spécifié. |
| Date  | string | Date et heure qui spécifient la minute, l’heure ou le jour au cours desquels les données d’utilisation simultanées se sont produites.  |
| SeriesName | string    | Elle a toujours la valeur **UserConcurrency**. |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant illustre un exemple de corps de réponse JSON pour cette demande avec l’agrégation de données par minute.

```json
[   {
        "Count": 418.0,
        "Date": "2018-02-02T04:42:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 418.0,
        "Date": "2018-02-02T04:43:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 415.0,
        "Date": "2018-02-02T04:44:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 412.0,
        "Date": "2018-02-02T04:45:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 414.0,
        "Date": "2018-02-02T04:46:13.65Z",
        "SeriesName": "UserConcurrency"
    }
]
```

## <a name="related-topics"></a>Rubriques connexes

* [Accéder aux données d’analyse à l’aide des services Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir des données d’analyse Xbox Live](get-xbox-live-analytics.md)
* [Obtenir des données de réussite Xbox Live](get-xbox-live-achievements-data.md)
* [Obtenir des données d’intégrité Xbox Live](get-xbox-live-health-data.md)
* [Recevoir des données du Hub de jeu Xbox Live](get-xbox-live-game-hub-data.md)
* [Obtenir des données de club Xbox Live](get-xbox-live-club-data.md)
* [Obtenir des données multijoueur Xbox Live](get-xbox-live-multiplayer-data.md)