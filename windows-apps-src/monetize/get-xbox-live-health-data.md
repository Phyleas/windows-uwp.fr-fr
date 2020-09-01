---
description: Utilisez cette méthode dans l’API Microsoft Store Analytics pour récupérer des données d’intégrité Xbox Live.
title: Obtenir des données d’intégrité Xbox Live
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, services Store, API Microsoft Store Analytics, Xbox Live Analytics, intégrité, erreurs du client
ms.localizationpriority: medium
ms.openlocfilehash: 3cf2432dfa9b4882f6fe878e3a1b832240735cb5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174993"
---
# <a name="get-xbox-live-health-data"></a>Obtenir des données d’intégrité Xbox Live


Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir les données d’intégrité de votre [jeu Xbox Live](/gaming/xbox-live/index.md). Ces informations sont également disponibles dans le [rapport Xbox Analytics](../publish/xbox-analytics-report.md) dans l’espace partenaires.

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
| applicationId | string | [ID de stockage](in-app-purchases-and-trials.md#store-ids) du jeu pour lequel vous souhaitez récupérer les données d’intégrité Xbox Live.  |  Oui  |
| metricType | string | Chaîne qui spécifie le type de données Xbox Live Analytics à récupérer. Pour cette méthode, spécifiez la valeur **callingpattern**.  |  Oui  |
| startDate | Date | Date de début dans la plage de dates des données d’intégrité à récupérer. La valeur par défaut est de 30 jours avant la date actuelle. |  Non  |
| endDate | Date | Date de fin dans la plage de dates des données d’intégrité à récupérer. La valeur par défaut est la date actuelle. |  Non  |
| top | int | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | int | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite. |  Non  |
| Filter | string  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction contient un nom de champ du corps de la réponse et une valeur qui sont associés aux **opérateurs** **EQ** ou ne, et les instructions peuvent être combinées à l’aide **de and** ou **de ou de**. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*. Vous pouvez spécifier les champs suivants dans le corps de la réponse :<p/><ul><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>sandboxId</strong></li></ul> | Non   |
| groupby | string | Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants dans le corps de la réponse :<p/><ul><li><strong>date</strong></li><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>sandboxId</strong></li></ul><p/>Si vous spécifiez un ou plusieurs champs *GroupBy* , tous les autres champs *GroupBy* que vous ne spécifiez pas auront la valeur **All** dans le corps de la réponse. |  Non  |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre une demande d’obtention de données d’intégrité pour votre jeu Xbox Live. Remplacez la valeur *ApplicationID* par l’ID du magasin de votre jeu.


```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=callingpattern&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>response

| Valeur      | Type   | Description                  |
|------------|--------|-------------------------------------------------------|
| Valeur      | tableau  | Tableau d’objets qui contiennent les données d’intégrité. Pour plus d’informations sur les données de chaque objet, consultez le tableau suivant.                                                                                                                      |
| @nextLink  | string | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est retournée si le paramètre **supérieur** de la demande est défini sur 10000, mais qu’il y a plus de 10000 lignes de données pour la requête. |
| TotalCount | int    | Nombre total de lignes dans les résultats de la requête.   |


Les éléments du tableau *Value* comportent les valeurs suivantes :

| Valeur               | Type   | Description                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | string | ID de stockage du jeu pour lequel vous récupérez les données d’intégrité.     |
| Date                | string | Première date de la plage de dates pour les données d’intégrité. Si la requête spécifiait un jour précis, cette valeur correspond à la date. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| deviceType          | string | L’une des chaînes suivantes qui spécifie le type d’appareil sur lequel votre jeu a été utilisé :<p/><ul><li><strong>XboxOne</strong></li><li><strong>WindowsOneCore</strong> (cette valeur indique un PC)</li><li><strong>Unknown</strong></li></ul>  |
| sandboxId     | string |   ID du bac à sable (sandbox) créé pour le jeu. Il peut s’agir de la valeur Retail ou de l’ID d’un bac à sable privé.   |
| packageVersion     | string |  Version de package en quatre parties pour le jeu.  |
| callingPattern     | object |  Objet [callingPattern](#callingpattern) qui fournit des réponses de service, des appareils et des données utilisateur pour chaque code d’état retourné par chaque service Xbox Live utilisé par votre jeu pour la plage de dates spécifiée.     |


### <a name="callingpattern"></a>callingPattern

| Valeur      | Type   | Description                  |
|------------|--------|-------------------------------------------------------|
| service      | string  |   Nom du service Xbox Live auquel les données d’intégrité se réfèrent.       |
| endpoint      | string  |   Point de terminaison du service Xbox Live auquel les données d’intégrité se réfèrent.        |
| httpStatusCode      | string  |  Code d’état HTTP pour cet ensemble de données d’intégrité.<p/><p/>**Note** &nbsp; Remarque &nbsp; Le code d’état **429E** indique que l’appel de service a réussi uniquement parce que la [limitation du débit](/gaming/xbox-live/using-xbox-live/best-practices/fine-grained-rate-limiting.md) affiné a été exemptée pendant l’appel. Le taux de granularité limité peut être appliqué à l’avenir si le service connaît un volume élevé et, dans ce cas, l’appel génère un [code d’état HTTP 429](/gaming/xbox-live/using-xbox-live/best-practices/fine-grained-rate-limiting.md#http-429-response-object).         |
| serviceResponses      | nombre  | Nombre de réponses de service qui ont renvoyé le code d’état spécifié.         |
| uniqueDevices      | nombre  |  Nombre d’appareils uniques ayant appelé le service et ayant reçu le code d’état spécifié.       |
| uniqueUsers      | nombre  |   Nombre d’utilisateurs uniques qui ont reçu le code d’état spécifié.       |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête. Les noms de service et les points de terminaison indiqués dans cet exemple ne sont pas réels et sont utilisés à titre d’exemple uniquement.

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR",
      "date": "2018-01-30",
      "deviceType": "All",
      "sandboxId": "RETAIL",
      "packageVersion": "Unknown",
      "callingpattern": [
        {
          "service": "userstats",
          "endpoint": "UserStats.BatchReadHandler.POST",
          "httpStatusCode": "200",
          "serviceResponses": 160891,
          "uniqueDevices": 410,
          "uniqueUsers": 410
        },
        {
          "service": "userstats",
          "endpoint": "UserStats.BatchStatReadHandler.GET",
          "httpStatusCode": "200",
          "serviceResponses": 422,
          "uniqueDevices": 0,
          "uniqueUsers": 30
        }
      ],
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Accéder aux données d’analyse à l’aide des services Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir des données d’analyse Xbox Live](get-xbox-live-analytics.md)
* [Obtenir des données de réussite Xbox Live](get-xbox-live-achievements-data.md)
* [Obtenir les données du hub de jeux Xbox Live](get-xbox-live-game-hub-data.md)
* [Obtenir des données de club Xbox Live](get-xbox-live-club-data.md)
* [Obtenir des données multijoueur Xbox Live](get-xbox-live-multiplayer-data.md)