---
description: Utilisez cette méthode dans l’API Microsoft Store Analytics pour récupérer des données multijoueurs Xbox Live.
title: Obtenir des données multijoueur Xbox Live
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, services Store, API Microsoft Store Analytics, Xbox Live Analytics, multijoueur
ms.localizationpriority: medium
ms.openlocfilehash: 546151e1cf95adc8ecbb64513a66671067869143
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171383"
---
# <a name="get-xbox-live-multiplayer-data"></a>Obtenir des données multijoueur Xbox Live


Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir des données multijoueur pour votre [jeu Xbox Live](/gaming/xbox-live/index.md) sur une base quotidienne ou mensuelle. Ces informations sont également disponibles dans le [rapport Xbox Analytics](../publish/xbox-analytics-report.md) dans l’espace partenaires.

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
| applicationId | string | [ID de stockage](in-app-purchases-and-trials.md#store-ids) du jeu pour lequel vous souhaitez récupérer des données multijoueurs Xbox Live.  |  Oui  |
| metricType | string | Chaîne qui spécifie le type de données Xbox Live Analytics à récupérer. Pour cette méthode, spécifiez la valeur **multiplayerdaily** pour obtenir des données multijoueurs quotidiennes ou **multiplayermonthly** pour obtenir des données de multijoueur mensuelles.  |  Oui  |
| startDate | Date | Date de début dans la plage de dates des données multijoueur à récupérer. Pour **multiplayerdaily**, la valeur par défaut est 3 mois avant la date actuelle. Pour **multiplayermonthly**, la valeur par défaut est 1 an avant la date actuelle. |  Non  |
| endDate | Date | Date de fin dans la plage de dates des données multijoueur à récupérer. La valeur par défaut est la date actuelle. |  Non  |
| top | int | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | int | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite. |  Non  |
| Filter | string  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction contient un nom de champ du corps de la réponse et une valeur qui sont associés aux **opérateurs** **EQ** ou ne, et les instructions peuvent être combinées à l’aide **de and** ou **de ou de**. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*. Vous pouvez spécifier les champs suivants dans le corps de la réponse :<p/><ul><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>négoci</strong></li><li><strong>subscriptionName</strong></li></ul> | Non   |
| groupby | string | Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants dans le corps de la réponse :<p/><ul><li><strong>date</strong></li><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>négoci</strong></li><li><strong>subscriptionName</strong></li></ul><p/>Si vous spécifiez un ou plusieurs champs *GroupBy* , tous les autres champs *GroupBy* que vous ne spécifiez pas auront la valeur **All** dans le corps de la réponse. |  Non  |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre une demande d’obtention de données multijoueur pour votre jeu Xbox Live. Remplacez la valeur *ApplicationID* par l’ID du magasin de votre jeu.


```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=multiplayerdaily&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>response

| Valeur      | Type   | Description                  |
|------------|--------|-------------------------------------------------------|
| Valeur      | tableau  | Tableau d’objets qui contiennent des données multijoueurs, où chaque objet représente un jeu de données pour la période quotidienne ou mensuelle spécifiée et est organisé selon le **filtre** et les valeurs **GroupBy** spécifiés. Pour plus d’informations sur les données de chaque objet, consultez les sections [analyse quotidienne multijoueur](#daily-multiplayer-analytics) et [analyse multijoueur mensuelle](#monthly-multiplayer-analytics) .     |
| @nextLink  | string | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est retournée si le paramètre **supérieur** de la demande est défini sur 10000, mais qu’il y a plus de 10000 lignes de données pour la requête. |
| TotalCount | int    | Nombre total de lignes dans les résultats de la requête. |


### <a name="daily-multiplayer-analytics"></a>Analyse multijoueur quotidienne

Les éléments du tableau de *valeurs* contiennent les valeurs suivantes lorsque vous demandez des données d’analyse multijoueur quotidiennes (autrement dit, lorsque vous spécifiez **multiplayerdaily** pour le paramètre **metricType** ).

| Valeur               | Type   | Description                           |
|---------------------|--------|-------------------------------------------|
| Date                | string | Date des données multijoueurs. |
| applicationId       | string | ID de stockage du jeu pour lequel vous récupérez des données multijoueurs.     |
| applicationName       | string |  Nom du jeu pour lequel vous récupérez des données multijoueurs.     |
| market       | string | Code de pays 3166 ISO à deux lettres du marché où proviennent les données multijoueurs.       |
| packageVersion     | string |  Version de package en quatre parties pour le jeu.  |
| deviceType          | string | L’une des chaînes suivantes qui spécifie le type d’appareil d’où proviennent les données multijoueurs :<p/><ul><li><strong>Console</strong></li><li><strong>ORDINATEURS</strong></li><li>**Unknown**</li></ul>  |
| subscriptionName     | string |  Nom de l’abonnement utilisé pour les données multijoueurs. Les valeurs possibles sont notamment **réussite du jeu Xbox** et **«»** (pour aucun abonnement).  |
| dailySessionCount     | nombre |  Nombre de sessions multijoueur pour le jeu à la date spécifiée.  |
| engagementDurationMinutes     | nombre |  Nombre total de minutes pendant lesquelles les clients ont été engagés avec des sessions multijoueur pour le jeu à la date spécifiée.  |
| dailyActiveUsers     | nombre |  Nombre total d’utilisateurs multijoueurs actifs pour le jeu à la date spécifiée.  |
| dailyActiveDevices     | nombre |  Nombre total de périphériques actifs qui ont joué des sessions multijoueur pour le jeu à la date spécifiée.  |
| dailyNewUsers     | nombre |  Nombre total de nouveaux utilisateurs multijoueur pour le jeu à la date spécifiée.  |
| monthlyActiveUsers     | nombre |  Nombre total d’utilisateurs multijoueurs actifs pour le mois au cours duquel la date spécifiée s’est produite.  |
| monthlyActiveDevices     | nombre | Nombre total d’appareils actifs qui ont joué des sessions multijoueur pour le jeu pendant le mois où la date spécifiée s’est produite.   |
| monthlyNewUsers     | nombre |  Nombre total de nouveaux utilisateurs multijoueur pour le jeu pour le mois où la date spécifiée s’est produite.  |


### <a name="monthly-multiplayer-analytics"></a>Analytique multijoueur mensuelle

Les éléments du tableau de *valeurs* contiennent les valeurs suivantes lorsque vous demandez des données d’analyse multijoueur mensuelles (autrement dit, lorsque vous spécifiez **multiplayermonthly** pour le paramètre **metricType** ).

| Valeur               | Type   | Description                           |
|---------------------|--------|-------------------------------------------|
| Date                | string | Première date du mois pour les données multijoueurs. |
| applicationId       | string | ID de stockage du jeu pour lequel vous récupérez des données multijoueurs.     |
| applicationName       | string |  Nom du jeu pour lequel vous récupérez des données multijoueurs.     |
| market       | string | Code de pays 3166 ISO à deux lettres du marché où proviennent les données multijoueurs.       |
| packageVersion     | string |  Version de package en quatre parties pour le jeu.  |
| deviceType          | string | L’une des chaînes suivantes qui spécifie le type d’appareil d’où proviennent les données multijoueurs :<p/><ul><li><strong>Console</strong></li><li><strong>ORDINATEURS</strong></li><li>**Unknown**</li></ul>  |
| subscriptionName     | string |  Nom de l’abonnement utilisé pour les données multijoueurs. Les valeurs possibles sont notamment **réussite du jeu Xbox** et **«»** (pour aucun abonnement).  |
| monthlySessionCount     | nombre |  Nombre de sessions multijoueur pour le jeu pendant le mois spécifié.   |
| engagementDurationMinutes     | nombre |  Nombre total de minutes pendant lesquelles les clients ont été engagés avec des sessions multijoueur pour le jeu pendant le mois spécifié.  |
| monthlyActiveUsers     | nombre | Nombre total d’utilisateurs multijoueurs actifs au cours du mois spécifié.   |
| monthlyActiveDevices     | nombre | Nombre total d’appareils actifs qui ont joué des sessions multijoueur pour le jeu pendant le mois spécifié.   |
| monthlyNewUsers     | nombre |  Nombre total de nouveaux utilisateurs multijoueur pour le jeu au cours du mois spécifié.  |
| averageDailyActiveUsers     | nombre |  Nombre moyen d’utilisateurs multijoueurs actifs quotidiens pour le jeu pendant le mois spécifié.  |
| averageDailyActiveDevices     | nombre |  Nombre moyen de périphériques actifs qui ont joué des sessions multijoueur pour le jeu pendant le mois spécifié.  |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant illustre un exemple de corps de réponse JSON pour la variante de mesures journalières de cette demande (autrement dit, lorsque vous spécifiez **multiplayerdaily** pour le paramètre **metricType** ).

```json
{
  "Value": [
    {
      "date": "2018-01-07",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Sports",
      "market": "All",
      "packageVersion": "",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 976711,
      "engagementDurationMinutes": 16836064.5,
      "dailyActiveUsers": 180377,
      "dailyActiveDevices": 153359,
      "dailyNewUsers": 8638,
      "monthlyActiveUsers": 779984,
      "monthlyActiveDevices": 606495,
      "monthlyNewUsers": 212093
    },
    {
      "date": "2018-01-05",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Sports",
      "market": "All",
      "packageVersion": "",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 857433,
      "engagementDurationMinutes": 14087724.5,
      "dailyActiveUsers": 166630,
      "dailyActiveDevices": 143065,
      "dailyNewUsers": 9646,
      "monthlyActiveUsers": 764947,
      "monthlyActiveDevices": 595368,
      "monthlyNewUsers": 204248
    },
  ],
  "@nextLink": null,
  "TotalCount":2
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Accéder aux données d’analyse à l’aide des services Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir des données d’analyse Xbox Live](get-xbox-live-analytics.md)
* [Obtenir des données de réussite Xbox Live](get-xbox-live-achievements-data.md)
* [Obtenir des données d’intégrité Xbox Live](get-xbox-live-health-data.md)
* [Obtenir les données du hub de jeux Xbox Live](get-xbox-live-game-hub-data.md)
* [Obtenir des données de club Xbox Live](get-xbox-live-club-data.md)