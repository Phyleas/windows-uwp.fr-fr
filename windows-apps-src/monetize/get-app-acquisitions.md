---
ms.assetid: C1E42E8B-B97D-4B09-9326-25E968680A0F
description: Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir des données d’acquisition agrégées pour une application pendant une plage de dates donnée et d’autres filtres facultatifs.
title: Obtenir des acquisitions d’applications
ms.date: 03/23/2018
ms.topic: article
keywords: Windows 10, UWP, services Store, API Microsoft Store Analytics, acquisitions d’applications
ms.localizationpriority: medium
ms.openlocfilehash: 9d0b19ee837debfa7807de7c594ae076271d3537
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493344"
---
# <a name="get-app-acquisitions"></a>Obtenir des acquisitions d’applications


Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir des données d’acquisition agrégées au format JSON pour une application pendant une plage de dates donnée et d’autres filtres facultatifs. Ces informations sont également disponibles dans le [rapport acquisitions](../publish/acquisitions-report.md) de l’espace partenaires.

## <a name="prerequisites"></a>Prérequis


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) pour l’API Microsoft Store Analytics.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de demande       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la demande

| Paramètre        | Type   |  Description      |  Obligatoire  
|---------------|--------|---------------|------|
| applicationId | string | [ID de stockage](in-app-purchases-and-trials.md#store-ids) de l’application pour laquelle vous souhaitez récupérer des données d’acquisition.  |  Oui  |
| startDate | Date | Dans la plage de dates, la date de début de la récupération des données d’acquisition. La valeur par défaut est la date actuelle. |  Non  |
| endDate | Date | Dans la plage de dates, la date de fin de la récupération des données d’acquisition. La valeur par défaut est la date actuelle. |  Non  |
| top | int | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | int | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite. |  Non  |
| Filter | string  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction contient un nom de champ du corps de la réponse et une valeur qui sont associés aux **opérateurs** **EQ** ou ne, et les instructions peuvent être combinées à l’aide **de and** ou **de ou de**. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*. Par exemple, *filtre = l’égalisation du marché « États-Unis » et « EQ de sexe*». <p/><p/>Vous pouvez spécifier les champs suivants dans le corps de la réponse :<p/><ul><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>négoci</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul> | Non   |
| aggregationLevel | string | Indique la plage de temps pendant laquelle récupérer les données agrégées. Il peut s’agit des chaînes suivantes : <strong>day</strong>, <strong>week</strong> ou <strong>month</strong>. Par défaut, la valeur est <strong>day</strong>. | Non |
| orderby | string | Une instruction qui commande les valeurs de données de résultats pour chaque acquisition. La syntaxe est <em>orderby = Field [Order], champ [Order],...</em>. Le paramètre <em>Field</em> peut être l’une des chaînes suivantes :<ul><li><strong>date</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>négoci</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p>Le paramètre <em>order</em>, facultatif, peut comporter les valeurs <strong>asc</strong> ou <strong>desc</strong> afin de spécifier l’ordre croissant ou décroissant pour chaque champ. La valeur par défaut est <strong>ASC</strong>.</p><p>Voici un exemple de chaîne <em>orderby</em> : <em>orderby = date, Market</em></p> |  Non  |
| groupby | string | Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants :<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>négoci</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p>Les lignes de données renvoyées comportent les champs spécifiés dans le paramètre <em>groupby</em>, ainsi que dans les paramètres suivants :</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>acquisitionQuantity</strong></li></ul><p>Le paramètre <em>groupby</em> peut être utilisé avec le paramètre <em>aggregationLevel</em>. Par exemple : <em> &amp; GroupBy = ageGroup, Market &amp; aggregationLevel = week</em></p> |  Non  |

### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre plusieurs demandes d’obtention des données d’acquisition d’applications. Remplacez la valeur *applicationId* par l’ID Windows Store de votre application.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and gender eq 'm'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>response


### <a name="response-body"></a>Response body

| Valeur      | Type   | Description                  |
|------------|--------|-------------------------------------------------------|
| Valeur      | tableau  | Tableau d’objets qui contiennent les données d’acquisition d’agrégats pour l’application. Pour plus d’informations sur les données de chaque objet, consultez la section [Valeurs d’acquisition](#acquisition-values) ci-dessous.                                                                                                                      |
| @nextLink  | string | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est renvoyée si le paramètre **top** de la demande est défini sur 10000, mais que plus de 10 000 lignes de données d’acquisition sont associées à la requête. |
| TotalCount | int    | Nombre total de lignes dans les résultats de la requête.                 |


### <a name="acquisition-values"></a>Valeurs d’acquisition

Les éléments du tableau *Value* comportent les valeurs suivantes :

| Valeur               | Type   | Description                           |
|---------------------|--------|-------------------------------------------|
| Date                | string | Première date dans la plage de dates des données d’acquisition. Si la requête spécifiait un jour précis, cette valeur correspond à la date. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| applicationId       | string | L’ID Windows Store de l’application pour laquelle vous récupérez les données d’acquisition.     |
| applicationName     | string | Nom d’affichage de l’application.   |
| deviceType          | string | L’une des chaînes suivantes qui spécifie le type d’appareil sur lequel l’acquisition s’est produite :<ul><li><strong>PC</strong></li><li><strong>Numéros</strong></li><li><strong>Console-Xbox One</strong></li><li><strong>Console-série Xbox X</strong></li><li><strong>IoT</strong></li><li><strong>Hologrammes</strong></li><li><strong>Unknown</strong></li></ul>    |
| orderName           | string | Le nom de la commande.  |
| storeClient         | string | L’une des chaînes suivantes qui indique la version du magasin dans lequel l’acquisition s’est produite :<ul><li>**Windows Phone Store (client)**</li><li>**Microsoft Store (client)** (ou **Windows Store (client)** si vous interrogez des données avant le 23 mars 2018)</li><li>**Microsoft Store (Web)** (ou **Windows Store (Web)** si vous interrogez des données avant le 23 mars 2018)</li><li>**Volume purchase by organizations**</li><li>**Autre**</li></ul>                                                                                            |
| osVersion           | string | L’une des chaînes suivantes qui spécifie la version du système d’exploitation sur laquelle l’acquisition s’est produite :<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows 8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Unknown</strong></li></ul>  |
| market              | string | Le code pays ISO 3166 du marché dans lequel l’acquisition s’est produite.  |
| gender              | string | L’une des chaînes suivantes qui spécifie le sexe de l’utilisateur qui a effectué l’acquisition :<ul><li><strong>lecteur</strong></li><li><strong>FA</strong></li><li><strong>Unknown</strong></li></ul>    |
| ageGroup            | string | L’une des chaînes suivantes qui spécifient le groupe d’âge de l’utilisateur qui a effectué l’acquisition :<ul><li><strong>less than 13</strong></li><li><strong>13-17</strong></li><li><strong>18-24</strong></li><li><strong>25-34</strong></li><li><strong>35-44</strong></li><li><strong>44-55</strong></li><li><strong>greater than 55</strong></li><li><strong>Unknown</strong></li></ul>  |
| acquisitionType     | string | Une des chaînes suivantes qui indique le type d’acquisition :<ul><li><strong>Gratuit</strong></li><li><strong>Version d’évaluation</strong></li><li><strong>Payé</strong></li><li><strong>Code promotionnel</strong></li><li><strong>Acquisitionquantity</strong></li><li><strong>IAP d’abonnement</strong></li><li><strong>Public privé</strong></li><li><strong>Ordre préalable</strong></li><li><strong>Réussite du jeu Xbox</strong> (ou <strong>Pass</strong> si vous interrogez des données avant le 23 mars 2018)</li><li><strong>Disque</strong></li><li><strong>Code prépayé</strong></li></ul>   |
| acquisitionQuantity | nombre | Le nombre d’acquisitions qui se sont produites durant le niveau d’agrégation spécifié.    |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "date": "2016-02-01",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Demo",
      "deviceType": "Phone",
      "orderName": "",
      "storeClient": "Windows Phone Store (client)",
      "osVersion": "Windows Phone 8.1",
      "market": "IT",
      "gender": "m",
      "ageGroup": "0-17",
      "acquisitionType": "Free",
      "acquisitionQuantity": 1
    }
  ],
  "@nextLink": "appacquisitions?applicationId=9NBLGGGZ5QDR&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1&orderby=date desc",
  "TotalCount": 466766
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Rapport sur les acquisitions](../publish/acquisitions-report.md)
* [Accéder aux données d’analyse à l’aide des services Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir les données d’entonnoir d’acquisition d’applications](get-acquisition-funnel-data.md)
* [Obtenir les conversions d’applications par canal](get-app-conversions-by-channel.md)
* [Obtenir des acquisitions d’extensions](get-in-app-acquisitions.md)
