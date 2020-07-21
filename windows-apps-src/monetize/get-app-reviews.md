---
ms.assetid: 2967C757-9D8A-4B37-8AA4-A325F7A060C5
description: Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir des données de révision pour une plage de dates donnée et d’autres filtres facultatifs.
title: Obtenir les avis sur les applications
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, services Store, API Microsoft Store Analytics, révisions
ms.localizationpriority: medium
ms.openlocfilehash: 01a22d22245882454fce6eb53b67c4fec0f8072b
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493104"
---
# <a name="get-app-reviews"></a>Obtenir les avis sur les applications


Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir des données de révision au format JSON pour une plage de dates donnée et d’autres filtres facultatifs. Ces informations sont également disponibles dans le [rapport de révisions](../publish/reviews-report.md) dans l’espace partenaires.

Une fois que vous avez récupéré les révisions, vous pouvez utiliser les méthodes [obtenir les informations de réponse pour les révisions](get-response-info-for-app-reviews.md) de l’application et [Envoyer des réponses aux](submit-responses-to-app-reviews.md) révisions d’application dans l’API de révisions de l’Microsoft Store pour répondre par programmation aux

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) pour l’API Microsoft Store Analytics.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête

### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de demande                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|---------------------|
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la demande

| Paramètre        | Type   |  Description      |  Obligatoire  
|---------------|--------|---------------|------|
| applicationId | string | [ID de stockage](in-app-purchases-and-trials.md#store-ids) de l’application dont vous souhaitez récupérer les données de révision.  |  Oui  |
| startDate | Date | Dans la plage de dates, la date de début de la récupération des avis. La valeur par défaut est la date actuelle. |  Non  |
| endDate | Date | Dans la plage de dates, la date de début de la récupération des avis. La valeur par défaut est la date actuelle. |  Non  |
| top | int | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | int | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite. |  Non  |
| Filter |string  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Pour plus d’informations, voir la section [Champs de filtre](#filter-fields) ci-dessous. | Non   |
| orderby | string | Instruction commandant les valeurs des données de résultats. La syntaxe est <em>orderby = Field [Order], champ [Order],...</em>. Le paramètre <em>Field</em> peut être l’une des chaînes suivantes :<ul><li><strong>date</strong></li><li><strong>osVersion</strong></li><li><strong>négoci</strong></li><li><strong>deviceType</strong></li><li><strong>isRevised</strong></li><li><strong>packageVersion</strong></li><li><strong>DeviceModel au</strong></li><li><strong>productFamily</strong></li><li><strong>deviceScreenResolution</strong></li><li><strong>isTouchEnabled</strong></li><li><strong>reviewerName</strong></li><li><strong>reviewTitle</strong></li><li><strong>reviewText</strong></li><li><strong>helpfulCount</strong></li><li><strong>notHelpfulCount</strong></li><li><strong>responseDate</strong></li><li><strong>responseText</strong></li><li><strong>deviceRAM</strong></li><li><strong>deviceStorageCapacity</strong></li><li><strong>audience</strong></li></ul><p>Le paramètre <em>order</em>, facultatif, peut comporter les valeurs <strong>asc</strong> ou <strong>desc</strong> afin de spécifier l’ordre croissant ou décroissant pour chaque champ. La valeur par défaut est <strong>ASC</strong>.</p><p>Voici un exemple de chaîne <em>orderby</em> : <em>orderby = date, Market</em></p> |  Non  |


### <a name="filter-fields"></a>Champs de filtrage

Le paramètre *filter* de la requête contient une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ et une valeur qui sont associés aux opérateurs **eq** ou **ne**, et certains champs prennent également en charge les opérateurs **contains**, **gt**, **lt**, **ge** et **le**. Les instructions peuvent être combinées à l’aide des opérateurs **and** ou **or**.

Voici un exemple de chaîne *filter* : *filter=contains(reviewText,’great’) and contains(reviewText,’ads’) and deviceRAM lt 2048 and market eq ’US’*

Pour obtenir une liste des champs pris en charge et des opérateurs associés à chacun d’entre eux, consultez le tableau suivant. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*.

| Champs        | Opérateurs pris en charge   |  Description        |
|---------------|--------|-----------------|
| market | eq, ne | Chaîne contenant le code pays ISO 3166 du marché des appareils. |
| osVersion  | eq, ne  | Une des chaînes suivantes :<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows 8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Unknown</strong></li></ul>  |
| deviceType  | eq, ne  | Une des chaînes suivantes :<ul><li><strong>PC</strong></li><li><strong>Numéros</strong></li><li><strong>Console-Xbox One</strong></li><li><strong>Console-série Xbox X</strong></li><li><strong>IoT</strong></li><li><strong>Hologrammes</strong></li><li><strong>Unknown</strong></li></ul>  |
| isRevised  | eq, ne  | Spécifiez la valeur <strong>true</strong> pour filtrer les avis révisés ; sinon, définissez la valeur <strong>false</strong>.  |
| packageVersion  | eq, ne  | La version du package d’application qui a été passée en revue.  |
| deviceModel  | eq, ne  | Le type d’appareil sur lequel l’application a été révisée.  |
| productFamily  | eq, ne  | Une des chaînes suivantes :<ul><li><strong>PC</strong></li><li><strong>Tablet</strong></li><li><strong>Numéros</strong></li><li><strong>Wearable</strong></li><li><strong>Serveur</strong></li><li><strong>CCM</strong></li><li><strong>Autre</strong></li></ul>  |
| deviceRAM  | eq, ne, gt, lt, ge, le  | Mémoire RAM physique, en Mo.  |
| deviceScreenResolution  | eq, ne  | Résolution de l’écran de l’appareil dans la hauteur de la &quot; <em>largeur</em> x <em>height</em> &quot; .   |
| deviceStorageCapacity  | eq, ne, gt, lt, ge, le   | Capacité du disque de stockage principal, en Go.  |
| isTouchEnabled  | eq, ne  | Spécifiez la valeur <strong>true</strong> pour filtrer les appareils tactiles ; sinon, définissez la valeur <strong>false</strong>.   |
| reviewerName  | eq, ne  |  Nom de réviseur. |
| rating  | eq, ne, gt, lt, ge, le  | Classification de l’application, en étoiles.  |
| reviewTitle  | eq, ne, contains  | Le titre de l’avis.  |
| reviewText  | eq, ne, contains  |  Texte de l’avis. |
| helpfulCount  | eq, ne  |  Le nombre d’occurrences où l’avis a été marqué comme utile. |
| notHelpfulCount  | eq, ne  | Nombre d’occurrences où l’avis a été signalé comme inutile.  |
| responseDate  | eq, ne  | Date à laquelle la réponse a été soumise.  |
| responseText  | eq, ne, contains  | Texte de la réponse.  |
| id  | eq, ne  | ID de la révision (il s’agit d’un GUID).        |


### <a name="request-example"></a>Exemple de requête

Les exemples suivants illustrent plusieurs requêtes de récupération des avis. Remplacez la valeur *applicationId* par l’ID Windows Store de votre application.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=contains(reviewText,'great') and contains(reviewText,'ads') and deviceRAM lt 2048 and market eq 'US' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>response


### <a name="response-body"></a>Response body

| Valeur      | Type   | Description      |
|------------|--------|------------------|
| Valeur      | tableau  | Un tableau d’objets comportant des avis. Pour plus d’informations sur les données de chaque objet, consultez la section [Valeurs d’avis](#review-values) ci-dessous.       |
| @nextLink  | string | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête est défini sur 10000, mais que plus de 10000 lignes de données d’avis sont associées à la requête. |
| TotalCount | int    | Nombre total de lignes dans les résultats de la requête.  |

 
### <a name="review-values"></a>Valeurs d’avis

Les éléments du tableau *Value* comportent les valeurs suivantes :

| Valeur           | Type    | Description       |
|-----------------|---------|-------------------|
| Date            | string  | Première date dans la plage de dates des données d’avis. Si la requête spécifiait un jour précis, cette valeur correspond à la date. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| applicationId   | string  | ID Windows Store de l’application dont vous récupérez les données d’avis.         |
| applicationName | string  | Nom d’affichage de l’application.    |
| market          | string  | Code pays ISO 3166 du marché où l’avis a été soumis.        |
| osVersion       | string  | Version du système d’exploitation sur lequel l’avis a été soumis. Pour obtenir la liste des chaînes prises en charge, consultez la section [Champs de filtrage](#filter-fields) ci-dessus.            |
| deviceType      | string  | Type d’appareil sur lequel l’avis a été soumis. Pour obtenir la liste des chaînes prises en charge, consultez la section [Champs de filtrage](#filter-fields) ci-dessus.            |
| isRevised       | Booléen | La valeur **true** indique que l’avis a été révisé ; sinon, la valeur **false** est affichée.   |
| packageVersion  | string  | La version du package d’application qui a été passée en revue.        |
| deviceModel        | string  |Le type d’appareil sur lequel l’application a été révisée.     |
| productFamily      | string  | Le nom de la famille d’appareils. Pour obtenir la liste des chaînes prises en charge, consultez la section [Champs de filtrage](#filter-fields) ci-dessus.   |
| deviceRAM       | nombre  | Mémoire RAM physique, en Mo.    |
| deviceScreenResolution       | string  | Résolution de l’écran de l’appareil au format «*largeur* x *hauteur*».    |
| deviceStorageCapacity | nombre | Capacité du disque de stockage principal, en Go. |
| isTouchEnabled | Booléen | La valeur **true** indique l’activation de l’interaction tactile ; sinon, la valeur **false** est affichée. |
| reviewerName | string | Nom de réviseur. |
| rating | nombre | Classification de l’application, en étoiles. |
| reviewTitle | string | Le titre de l’avis. |
| reviewText | string | Texte de l’avis. |
| helpfulCount | nombre | Le nombre d’occurrences où l’avis a été marqué comme utile. |
| notHelpfulCount | nombre | Nombre d’occurrences où l’avis a été signalé comme inutile. |
| responseDate | string | Date de soumission d’une réponse. |
| responseText | string | Texte de la réponse. |
| id | string | ID de la révision (il s’agit d’un GUID). Vous pouvez utiliser cet ID dans les méthodes [obtenir les informations de réponse pour les révisions d’application](get-response-info-for-app-reviews.md) et envoyer des [réponses aux révisions d’application](submit-responses-to-app-reviews.md) . |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "date": "2015-07-29",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso demo",
      "market": "US",
      "osVersion": "10.0.10240.16410",
      "deviceType": "PC",
      "isRevised": true,
      "packageVersion": "",
      "deviceModel": "Microsoft Corporation-Virtual Machine",
      "productFamily": "PC",
      "deviceRAM": -1,
      "deviceScreenResolution": "1024 x 768",
      "deviceStorageCapacity": 51200,
      "isTouchEnabled": false,
      "reviewerName": "ALeksandra",
      "rating": 5,
      "reviewTitle": "Love it",
      "reviewText": "Great app for demos and great dev response.",
      "helpfulCount": 0,
      "notHelpfulCount": 0,
      "responseDate": "2015-08-07T01:50:22.9874488Z",
      "responseText": "1",
      "id": "6be543ff-1c9c-4534-aced-af8b4fbe0316"
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Rapport Avis](../publish/reviews-report.md)
* [Accéder aux données d’analyse à l’aide des services Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir des informations de réponse pour les révisions d’application](get-response-info-for-app-reviews.md)
* [Envoyer des réponses aux révisions d’application](submit-responses-to-app-reviews.md)
* [Obtenir des acquisitions d’applications](get-app-acquisitions.md)
* [Obtenir des acquisitions d’extensions](get-in-app-acquisitions.md)
* [Obtenir les données de rapport d’erreurs](get-error-reporting-data.md)
* [Obtenir les classifications des applications](get-app-ratings.md)
