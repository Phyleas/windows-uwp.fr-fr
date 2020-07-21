---
ms.assetid: 1599605B-4243-4081-8D14-40F6F7734E25
description: Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir des données d’acquisition agrégées pour un module complémentaire pendant une plage de dates donnée et d’autres filtres facultatifs.
title: Obtenir des acquisitions d’extensions
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, services Store, API Microsoft Store Analytics, acquisitions complémentaires
ms.localizationpriority: medium
ms.openlocfilehash: 64605b044c93ee4744d09fe4f44d07bca39285ad
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493214"
---
# <a name="get-add-on-acquisitions"></a>Obtenir des acquisitions d’extensions

Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir des données d’acquisition agrégées pour les modules complémentaires de votre application au format JSON pendant une plage de dates donnée et d’autres filtres facultatifs. Ces informations sont également disponibles dans le [rapport acquisitions du module complémentaire](../publish/add-on-acquisitions-report.md) de l’espace partenaires.

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) pour l’API Microsoft Store Analytics.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de demande                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description          |
|---------------|--------|--------------|
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la demande

Le paramètre *applicationId* ou *inAppProductId* est obligatoire. Pour récupérer les données d’acquisition de toutes les extensions inscrites dans l’application, spécifiez le paramètre *applicationId*. Pour récupérer les données d’acquisition d’une seule extension, spécifiez le paramètre *inAppProductId*. Si vous spécifiez les deux valeurs, le paramètre *applicationId* est ignoré.

| Paramètre        | Type   |  Description      |  Obligatoire  
|---------------|--------|---------------|------|
| applicationId | string | [ID de stockage](in-app-purchases-and-trials.md#store-ids) de l’application pour laquelle vous souhaitez récupérer les données d’acquisition du module complémentaire.  |  Oui  |
| inAppProductId | string | [ID de stockage](in-app-purchases-and-trials.md#store-ids) du module complémentaire pour lequel vous souhaitez récupérer les données d’acquisition.  | Oui  |
| startDate | Date | Dans la plage de dates, date de début de la récupération des données d’acquisition. La valeur par défaut est la date actuelle. |  Non  |
| endDate | Date | Dans la plage de dates, date de fin de la récupération des données d’acquisition. La valeur par défaut est la date actuelle. |  Non  |
| top | int | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | int | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite. |  Non  |
| Filter |string  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Pour plus d’informations, voir la section [Champs de filtre](#filter-fields) ci-dessous. | Non   |
| aggregationLevel | string | Indique la plage de temps pendant laquelle récupérer les données agrégées. Il peut s’agit des chaînes suivantes : <strong>day</strong>, <strong>week</strong> ou <strong>month</strong>. Par défaut, la valeur est <strong>day</strong>. | Non |
| orderby | string | Instruction qui commande les valeurs de données de résultats pour chaque acquisition d’extension. La syntaxe est <em>orderby = Field [Order], champ [Order],...</em>. Le paramètre <em>Field</em> peut être l’une des chaînes suivantes :<ul><li><strong>date</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>négoci</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p>Le paramètre <em>order</em>, facultatif, peut comporter les valeurs <strong>asc</strong> ou <strong>desc</strong> afin de spécifier l’ordre croissant ou décroissant pour chaque champ. La valeur par défaut est <strong>ASC</strong>.</p><p>Voici un exemple de chaîne <em>orderby</em> : <em>orderby = date, Market</em></p> |  Non  |
| groupby | string | Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants :<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>inAppProductName</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>négoci</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p>Les lignes de données renvoyées comportent les champs spécifiés dans le paramètre <em>groupby</em>, ainsi que dans les paramètres suivants :</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>inAppProductId</strong></li><li><strong>acquisitionQuantity</strong></li></ul><p>Le paramètre <em>groupby</em> peut être utilisé avec le paramètre <em>aggregationLevel</em>. Par exemple : <em> &amp; GroupBy = ageGroup, Market &amp; aggregationLevel = week</em></p> |  Non  |


### <a name="filter-fields"></a>Champs de filtrage

Le paramètre *filter* de la requête contient une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ et une valeur qui sont associés aux opérateurs **eq** ou **ne**, et les instructions peuvent être combinées à l’aide des opérateurs **and** ou **or**. Voici quelques exemples de paramètres *filter* :

-   *filter=market eq ’US’ and gender eq ’m’*
-   *filter=(market ne ’US’) and (gender ne ’Unknown’) and (gender ne ’m’) and (market ne ’NO’) and (ageGroup ne ’greater than 55’ or ageGroup ne ‘less than 13’)*

Pour obtenir la liste des champs pris en charge, consultez le tableau suivant : Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*.

| Champs        |  Description        |
|---------------|-----------------|
| acquisitionType | Une des chaînes suivantes :<ul><li><strong>Gratuit</strong></li><li><strong>Evaluation</strong></li><li><strong>litige</strong></li><li><strong>promotional code</strong></li><li><strong>iap</strong></li></ul> |
| ageGroup | Une des chaînes suivantes :<ul><li><strong>less than 13</strong></li><li><strong>13-17</strong></li><li><strong>18-24</strong></li><li><strong>25-34</strong></li><li><strong>35-44</strong></li><li><strong>44-55</strong></li><li><strong>greater than 55</strong></li><li><strong>Unknown</strong></li></ul> |
| storeClient | Une des chaînes suivantes :<ul><li><strong>Windows Phone Store (client)</strong></li><li><strong>Microsoft Store (client)</strong></li><li><strong>Microsoft Store (Web)</strong></li><li><strong>Volume purchase by organizations</strong></li><li><strong>Autre</strong></li></ul> |
| gender | Une des chaînes suivantes :<ul><li><strong>lecteur</strong></li><li><strong>FA</strong></li><li><strong>Unknown</strong></li></ul> |
| market | Chaîne contenant le code pays ISO 3166 du marché de l’acquisition. |
| osVersion | Une des chaînes suivantes :<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows 8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Unknown</strong></li></ul> |
| deviceType | Une des chaînes suivantes :<ul><li><strong>PC</strong></li><li><strong>Numéros</strong></li><li><strong>Console-Xbox One</strong></li><li><strong>Console-série Xbox X</strong></li><li><strong>IoT</strong></li><li><strong>Hologrammes</strong></li><li><strong>Unknown</strong></li></ul> |
| orderName | Chaîne spécifiant le nom de la commande correspondant au code promotionnel utilisé pour l’acquisition du module complémentaire (elle s’applique uniquement si l’utilisateur a acquis le module complémentaire à l’aide d’un code promotionnel). |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre quelques requêtes d’obtention de données d’acquisition d’extensions. Remplacez les valeurs *inAppProductId* et *applicationId* par l’ID Windows Store qui correspond à votre extension ou application.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?inAppProductId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?inAppProductId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=7/3/2015&top=100&skip=0&filter=market ne 'US' and gender ne 'Unknown' and gender ne 'm' and market ne 'NO' and ageGroup ne '>55' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>response


### <a name="response-body"></a>Response body

| Valeur      | Type   | Description         |
|------------|--------|------------------|
| Valeur      | tableau  | Tableau d’objets contenant des données d’acquisition agrégées d’extensions. Pour plus d’informations sur les données incluses dans chaque objet, voir [Valeurs d’acquisition d’extensions](#add-on-acquisition-values) ci-dessous.                                                                                                              |
| @nextLink  | string | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête a la valeur 10000, mais qu’il existe plus de 10 000 lignes de données d’acquisition d’extensions pour la demande. |
| TotalCount | int    | Nombre total de lignes dans les résultats de la requête.    |


<span id="add-on-acquisition-values" />

### <a name="add-on-acquisition-values"></a>Valeurs d’acquisition d’extensions

Les éléments du tableau *Value* comportent les valeurs suivantes :

| Valeur               | Type    | Description        |
|---------------------|---------|---------------------|
| Date                | string  | Première date dans la plage de dates des données d’acquisition. Si la requête spécifiait un jour précis, cette valeur correspond à la date. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| inAppProductId      | string  | ID Windows Store de l’extension pour laquelle vous récupérez des données d’acquisition.                                                                                                                                                                 |
| inAppProductName    | string  | Nom d’affichage de l’extension. Cette valeur apparaît dans les données de la réponse uniquement si le paramètre *aggregationLevel* est défini sur **day**, sauf si vous définissez le champ **inAppProductName** dans le paramètre *groupby*.                                                                                                                                                                                                            |
| applicationId       | string  | ID Windows Store de l’application pour laquelle vous voulez récupérer des données d’acquisition d’extension.                                                                                                                                                           |
| applicationName     | string  | Nom d’affichage de l’application.                                                                                                                                                                                                             |
| deviceType          | string  | Le type d’appareil ayant effectué l’acquisition. Pour obtenir la liste des chaînes prises en charge, consultez la section [Champs de filtrage](#filter-fields) ci-dessus.                                                                                                  |
| orderName           | string  | Le nom de la commande.                                                                                                                                                                                                                   |
| storeClient         | string  | La version du Store dans laquelle l’acquisition s’est produite. Pour obtenir la liste des chaînes prises en charge, consultez la section [Champs de filtrage](#filter-fields) ci-dessus.                                                                                            |
| osVersion           | string  | La version de système d’exploitation sur laquelle l’acquisition s’est produite. Pour obtenir la liste des chaînes prises en charge, consultez la section [Champs de filtrage](#filter-fields) ci-dessus.                                                                                                   |
| market              | string  | Le code pays ISO 3166 du marché dans lequel l’acquisition s’est produite.                                                                                                                                                                  |
| gender              | string  | Le sexe de l’utilisateur qui a effectué l’acquisition. Pour obtenir la liste des chaînes prises en charge, consultez la section [Champs de filtrage](#filter-fields) ci-dessus.                                                                                                    |
| ageGroup            | string  | Le groupe d’âge de l’utilisateur qui a effectué l’acquisition. Pour obtenir la liste des chaînes prises en charge, consultez la section [Champs de filtrage](#filter-fields) ci-dessus.                                                                                                 |
| acquisitionType     | string  | Le type d’acquisition (gratuite, payante, etc.). Pour obtenir la liste des chaînes prises en charge, consultez la section [Champs de filtrage](#filter-fields) ci-dessus.                                                                                                    |
| acquisitionQuantity | entier | Le nombre d’acquisitions qui se sont produites.                        |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "date": "2015-01-02",
      "inAppProductId": "9NBLGGH3LHKL",
      "inAppProductName": "Contoso add-on 7",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Demo",
      "deviceType": "Phone",
      "orderName": "",
      "storeClient": "Windows Phone Store (client)",
      "osVersion": "Windows Phone 8.1",
      "market": "GB",
      "gender": "m",
      "ageGroup": "50orover",
      "acquisitionType": "iap",
      "acquisitionQuantity": 1
    }
  ],
  "@nextLink": "inappacquisitions?applicationId=9NBLGGGZ5QDR&inAppProductId=&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 33677
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Rapport sur les acquisitions des extensions](../publish/add-on-acquisitions-report.md)
* [Accéder aux données d’analyse à l’aide des services Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir les conversions d’extensions par canal](get-add-on-conversions-by-channel.md)
* [Obtenir des acquisitions d’applications](get-app-acquisitions.md)
* [Obtenir les données d’entonnoir d’acquisition d’applications](get-acquisition-funnel-data.md)
* [Obtenir les conversions d’applications par canal](get-app-conversions-by-channel.md)

 

 
