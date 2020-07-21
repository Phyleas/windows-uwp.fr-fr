---
description: Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir des conversions d’agrégats par données de canal pour une application pendant une plage de dates donnée et d’autres filtres facultatifs.
title: Obtenir les conversions d’applications par canal
ms.date: 08/04/2017
ms.topic: article
keywords: Windows 10, UWP, services Store, API Microsoft Store Analytics, conversions d’applications, canal
ms.localizationpriority: medium
ms.openlocfilehash: 58eda007ad82431734028231e8f5ae02cc704fab
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493534"
---
# <a name="get-app-conversions-by-channel"></a>Obtenir les conversions d’applications par canal

Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir des conversions d’agrégats par canal pour une application pendant une plage de dates donnée et d’autres filtres facultatifs.

* Une *conversion* signifie qu’un client (connecté à l’aide d’un compte Microsoft) a récemment obtenu une licence pour votre application (que vous ayez ou non facturé un argent).
* Le *canal* est la méthode dans laquelle un client est arrivé sur la page de liste de votre application (par exemple, via la campagne de promotion de la boutique ou d’une [application personnalisée](../publish/create-a-custom-app-promotion-campaign.md)).

Ces informations sont également disponibles dans le [rapport acquisitions](../publish/acquisitions-report.md#app-page-views-and-conversions-by-channel) de l’espace partenaires.

## <a name="prerequisites"></a>Prérequis


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) pour l’API Microsoft Store Analytics.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de demande       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/appchannelconversions``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la demande

| Paramètre        | Type   |  Description      |  Obligatoire  
|---------------|--------|---------------|------|
| applicationId | string | [ID de stockage](in-app-purchases-and-trials.md#store-ids) de l’application dont vous souhaitez récupérer les données de conversion. Exemple d’ID Windows Store : 9WZDNCRFJ3Q8. |  Oui  |
| startDate | Date | Date de début dans la plage de dates des données de conversion à récupérer. La valeur par défaut est 1/1/2016. |  Non  |
| endDate | Date | Date de fin dans la plage de dates des données de conversion à récupérer. La valeur par défaut est la date actuelle. |  Non  |
| top | int | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | int | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite. |  Non  |
| Filter | string  | Une ou plusieurs instructions qui filtrent le corps de la réponse. Chaque instruction peut utiliser les opérateurs **eq** ou **ne** ; les instructions peuvent être combinées à l’aide de **and** ou **or**. Vous pouvez spécifier les chaînes suivantes dans les instructions de filtre. Pour obtenir des descriptions, consultez la section [valeurs de conversion](#conversion-values) dans cet article. <ul><li><strong>applicationName</strong></li><li><strong>appType</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>storeClient</strong></li><li><strong>deviceType</strong></li><li><strong>négoci</strong></li></ul><p>Voici un exemple de paramètre de *filtre* : <em>Filter = DEVICETYPE EQ’PC'</em>.</p> | Non   |
| aggregationLevel | string | Indique la plage de temps pendant laquelle récupérer les données agrégées. Il peut s’agit des chaînes suivantes : <strong>day</strong>, <strong>week</strong> ou <strong>month</strong>. Par défaut, la valeur est <strong>day</strong>. | Non |
| orderby | string | Instruction qui classe les valeurs de données de résultat pour chaque conversion. La syntaxe est <em>orderby = Field [Order], champ [Order],...</em>. Le paramètre <em>Field</em> peut être l’une des chaînes suivantes :<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>appType</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>storeClient</strong></li><li><strong>deviceType</strong></li><li><strong>négoci</strong></li></ul><p>Le paramètre <em>order</em>, facultatif, peut comporter les valeurs <strong>asc</strong> ou <strong>desc</strong> afin de spécifier l’ordre croissant ou décroissant pour chaque champ. La valeur par défaut est <strong>ASC</strong>.</p><p>Voici un exemple de chaîne <em>orderby</em> : <em>orderby = date, Market</em></p> |  Non  |
| groupby | string | Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants :<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>appType</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>storeClient</strong></li><li><strong>deviceType</strong></li><li><strong>négoci</strong></li></ul><p>Les lignes de données renvoyées comportent les champs spécifiés dans le paramètre <em>groupby</em>, ainsi que dans les paramètres suivants :</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>conversionCount</strong></li><li><strong>clickCount</strong></li></ul><p>Le paramètre <em>groupby</em> peut être utilisé avec le paramètre <em>aggregationLevel</em>. Par exemple : <em>GroupBy = ageGroup, Market &amp; aggregationLevel = week</em></p> |  Non  |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre plusieurs demandes d’obtention de données de conversion d’application. Remplacez la valeur *applicationId* par l’ID Windows Store de votre application.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/appchannelconversions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=2/1/2017&top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/appchannelconversions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=4/31/2017&skip=0&filter=market eq 'US'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>response


### <a name="response-body"></a>Response body

| Valeur      | Type   | Description                  |
|------------|--------|-------------------------------------------------------|
| Valeur      | tableau  | Tableau d’objets qui contient les données de conversion d’agrégation pour l’application. Pour plus d’informations sur les données de chaque objet, consultez la section [conversion des valeurs](#conversion-values) ci-dessous.                                                                                                                      |
| @nextLink  | string | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est retournée si le paramètre **supérieur** de la requête a la valeur 10, mais qu’il y a plus de 10 lignes de données de conversion pour la requête. |
| TotalCount | int    | Nombre total de lignes dans les résultats de la requête.      |


### <a name="conversion-values"></a>Valeurs de conversion

Les objets du tableau de *valeurs* contiennent les valeurs suivantes.

| Valeur               | Type   | Description                           |
|---------------------|--------|-------------------------------------------|
| Date                | string | Première date dans la plage de dates pour les données de conversion. Si la requête spécifiait un jour précis, cette valeur correspond à la date. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| applicationId       | string | [ID de stockage](in-app-purchases-and-trials.md#store-ids) de l’application pour laquelle vous extrayez des données de conversion.     |
| applicationName     | string | Nom complet de l’application pour laquelle vous extrayez des données de conversion.        |
| appType          | string |  Type du produit pour lequel vous extrayez des données de conversion. Pour cette méthode, la seule valeur prise en charge est **application**.            |
| customCampaignId           | string |  Chaîne d’ID pour une [campagne de promotion d’application personnalisée](../publish/create-a-custom-app-promotion-campaign.md) qui est associée à l’application.   |
| referrerUriDomain           | string |  Spécifie le domaine dans lequel la liste d’applications avec l’ID de campagne de promotion d’application personnalisée a été activée.   |
| channelType           | string |  L’une des chaînes suivantes qui spécifient le canal pour la conversion :<ul><li><strong>CustomCampaignId</strong></li><li><strong>Stocker le trafic</strong></li><li><strong>Autre</strong></li></ul>    |
| storeClient         | string | Version du magasin dans lequel la conversion s’est produite. Actuellement, la seule valeur prise en charge est **SFC**.    |
| deviceType          | string | Une des chaînes suivantes :<ul><li><strong>PC</strong></li><li><strong>Numéros</strong></li><li><strong>Console-Xbox One</strong></li><li><strong>Console-série Xbox X</strong></li><li><strong>IoT</strong></li><li><strong>Hologrammes</strong></li><li><strong>Unknown</strong></li></ul>            |
| market              | string | Code du pays ISO 3166 du marché où la conversion s’est produite.    |
| clickCount              | nombre  |     Le nombre de clics du client sur le lien de votre liste d’applications.      |           
| conversionCount            | nombre  |   Nombre de conversions client.         |          


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "date": "2016-01-01",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso App",
      "appType": "App",
      "customCampaignId": "",
      "referrerUriDomain": "Universal Client Store",
      "channelType": "Store Traffic",
      "storeClient": "SFC",
      "deviceType": "PC",
      "market": "US",
      "clickCount": 7,
      "conversionCount": 0
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Rapport sur les acquisitions](../publish/acquisitions-report.md)
* [Accéder aux données d’analyse à l’aide des services Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir des acquisitions d’applications](get-app-acquisitions.md)
