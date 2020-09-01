---
description: Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir des données Insights pour votre application de bureau.
title: Obtenir les données d’insights pour votre application de bureau
ms.date: 07/31/2018
ms.topic: article
keywords: Windows 10, UWP, services Store, API Microsoft Store Analytics, Insights
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: bd60425a5ec3c040417aded818c766db80eb59eb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172893"
---
# <a name="get-insights-data-for-your-desktop-application"></a>Obtenir les données d’insights pour votre application de bureau

Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir des informations relatives aux mesures d’intégrité d’une application de bureau que vous avez ajoutée au programme d' [application de bureau Windows](/windows/desktop/appxpkg/windows-desktop-application-program). Ces données sont également disponibles dans le [rapport d’intégrité](/windows/desktop/appxpkg/windows-desktop-application-program#health-report) pour les applications de bureau dans l’espace partenaires.

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) pour l’API Microsoft Store Analytics.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de demande       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la demande

| Paramètre        | Type   |  Description      |  Obligatoire  
|---------------|--------|---------------|------|
| applicationId | string | ID de produit de l’application de bureau pour laquelle vous souhaitez obtenir des données Insights. Pour obtenir l’ID de produit d’une application de bureau, ouvrez un [rapport analytique pour votre application de bureau dans l’espace partenaires](/windows/desktop/appxpkg/windows-desktop-application-program) (par exemple, le **rapport d’intégrité**) et récupérez l’ID de produit à partir de l’URL. Si vous ne spécifiez pas ce paramètre, le corps de la réponse contient des données d’analyse pour toutes les applications inscrites dans votre compte.  |  Non  |
| startDate | Date | Date de début dans la plage de dates des données Insights à récupérer. La valeur par défaut est de 30 jours avant la date actuelle. |  Non  |
| endDate | Date | Date de fin dans la plage de dates des données à récupérer. La valeur par défaut est la date actuelle. |  Non  |
| Filter | string  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction contient un nom de champ du corps de la réponse et une valeur qui sont associés aux **opérateurs** **EQ** ou ne, et les instructions peuvent être combinées à l’aide **de and** ou **de ou de**. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*. Par exemple, *Filter = DataType EQ’acquisition'*. <p/><p/>Actuellement, cette méthode prend uniquement en charge l' **intégrité**du filtre.  | Non   |

### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre une demande d’obtention de données Insights. Remplacez la valeur *ApplicationID* par la valeur appropriée pour votre application de bureau.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights?applicationId=10238467886765136388&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'health' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>response

### <a name="response-body"></a>Response body

| Valeur      | Type   | Description                  |
|------------|--------|-------------------------------------------------------|
| Valeur      | tableau  | Tableau d’objets qui contiennent des données Insights pour l’application. Pour plus d’informations sur les données de chaque objet, consultez la section [Insight values](#insight-values) ci-dessous.                                                                                                                      |
| TotalCount | int    | Nombre total de lignes dans les résultats de la requête.                 |


### <a name="insight-values"></a>Valeurs Insight

Les éléments du tableau *Value* comportent les valeurs suivantes :

| Valeur               | Type   | Description                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | string | ID de produit de l’application de bureau pour laquelle vous avez extrait des données Insights.     |
| insightDate                | string | Date à laquelle nous avons identifié la modification d’une mesure spécifique. Cette date représente la fin de la semaine au cours de laquelle nous avons détecté une augmentation ou une diminution significative d’une mesure par rapport à la semaine qui précède. |
| dataType     | string | Chaîne qui spécifie la zone d’analyse générale que ce Insight informe. Actuellement, cette méthode ne prend en charge que l' **intégrité**.    |
| insightDetail          | tableau | Une ou plusieurs [valeurs InsightDetail](#insightdetail-values) qui représentent les détails de l’analyse actuelle.    |


### <a name="insightdetail-values"></a>Valeurs InsightDetail

| Valeur               | Type   | Description                           |
|---------------------|--------|-------------------------------------------|
| FactName           | string | Chaîne qui indique la mesure décrite par la dimension Insight ou Current actuelle. Actuellement, cette méthode prend en charge uniquement la valeur **HitCount**.  |
| Sous-dimensions         | tableau |  Un ou plusieurs objets qui décrivent une mesure unique pour l’analyse.   |
| PercentChange            | string |  Pourcentage de modification de la mesure sur l’ensemble de votre base de clients.  |
| DimensionName           | string |  Nom de la métrique décrite dans la dimension actuelle. Exemples : **eventType**, **Market**, **DeviceType**et **PackageVersion**.   |
| DimensionValue              | string | Valeur de la métrique qui est décrite dans la dimension actuelle. Par exemple, si **DimensionName** est **eventType**, **DimensionValue** **peut se bloquer** ou **se bloquer**.   |
| FactValue     | string | Valeur absolue de la mesure à la date à laquelle l’analyse a été détectée.  |
| Direction | string |  Direction de la modification (**positive** ou **négative**).   |
| Date              | string |  Date à laquelle nous avons identifié la modification relative à l’analyse actuelle ou à la dimension actuelle.   |

### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR",
      "insightDate": "2018-06-03T00:00:00",
      "dataType": "health",
      "insightDetail": [
        {
          "FactName": "HitCount",
          "SubDimensions": [
            {
              "FactName:": "HitCount",
              "PercentChange": "21",
              "DimensionValue:": "DE",
              "FactValue": "109",
              "Direction": "Positive",
              "Date": "6/3/2018 12:00:00 AM",
              "DimensionName": "Market"
            }
          ],
          "DimensionValue": "crash",
          "Date": "6/3/2018 12:00:00 AM",
          "DimensionName": "EventType"
        },
        {
          "FactName": "HitCount",
          "SubDimensions": [
            {
              "FactName:": "HitCount",
              "PercentChange": "71",
              "DimensionValue:": "JP",
              "FactValue": "112",
              "Direction": "Positive",
              "Date": "6/3/2018 12:00:00 AM",
              "DimensionName": "Market"
            }
          ],
          "DimensionValue": "hang",
          "Date": "6/3/2018 12:00:00 AM",
          "DimensionName": "EventType"
        },
      ],
      "insightId": "9CY0F3VBT1AS942AFQaeyO0k2zUKfyOhrOHc0036Iwc="
    }
  ],
  "@nextLink": null,
  "TotalCount": 2
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Programme d’application de bureau Windows](/windows/desktop/appxpkg/windows-desktop-application-program)
* [Rapport sur l’intégrité](/windows/desktop/appxpkg/windows-desktop-application-program#health-report)
* [Accéder aux données d’analyse à l’aide des services Microsoft Store](access-analytics-data-using-windows-store-services.md)