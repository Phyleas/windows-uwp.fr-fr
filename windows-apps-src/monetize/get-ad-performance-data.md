---
ms.assetid: 235EBA39-8F64-4499-9833-4CCA9C737477
description: Utilisez cette méthode dans l’API d’analyse du Microsoft Store pour obtenir les données agrégés de performances publicitaires d’une application pour une plage de dates données, et en fonction de filtres facultatifs.
title: Obtenir les données relatives aux performances publicitaires
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, services du Microsoft Store, API d'analyse du Microsoft Store, publicités, performances
ms.localizationpriority: medium
ms.openlocfilehash: c6bec86929284e49e4e882597422d316276c0a33
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627884"
---
# <a name="get-ad-performance-data"></a>Obtenir les données relatives aux performances publicitaires


Utilisez cette méthode dans l’API d’analyse du Microsoft Store pour obtenir les données agrégés de performances publicitaires de vos applications pour une plage de dates données, et en fonction de filtres facultatifs. Cette méthode renvoie les données au format JSON.

Cette méthode retourne les mêmes données sont fournies par le [publier le rapport de performances](../publish/advertising-performance-report.md) dans Partner Center.

## <a name="prerequisites"></a>Conditions préalables


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

Pour plus d’informations, voir [Accéder aux données d’analyse à l’aide des services du Microsoft Store](access-analytics-data-using-windows-store-services.md).

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de requête                                                              |
|--------|--------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/adsperformance``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description           |
|---------------|--------|--------------------------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

Pour récupérer les données de performances publicitaires d’une application spécifique, utilisez le paramètre *applicationId*. Pour récupérer les données de performances publicitaires de l’ensemble des applications associées à votre compte de développeur, omettez le paramètre *applicationId*.

| Paramètre     | Type   | Description     | Obligatoire |
|---------------|--------|-----------------|----------|
| applicationId   | chaîne    | L'[ID Store](in-app-purchases-and-trials.md#store-ids) de l’app pour laquelle vous souhaitez récupérer les données de performances publicitaires.  |    Non      |
| startDate   | date    | Date de début, dans la plage de dates, des données de performances publicitaires à récupérer, au format AAAA/MM/JJ. La valeur par défaut correspond à la date antérieure de 30 jours à la date actuelle. |    Non      |
| endDate   | date    | Date de fin dans la plage de dates, des données de performances publicitaires à récupérer, au format AAAA/MM/JJ. La valeur par défaut correspond à la date antérieure d’un jour à la date actuelle. |    Non      |
| top   | entier    | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |    Non      |
| skip   | entier    | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite. |    Non      |
| filter   | chaîne    | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Pour plus d’informations, voir la section [Champs de filtre](#filter-fields) ci-dessous. |    Non      |
| aggregationLevel   | chaîne    | Indique la plage de temps pendant laquelle récupérer les données agrégées. Il peut s’agit des chaînes suivantes : <strong>day</strong>, <strong>week</strong> ou <strong>month</strong>. Par défaut, la valeur est <strong>day</strong>. |    Non      |
| orderby   | chaîne    | Instruction commandant les valeurs des données de résultats. Syntaxe : <em>orderby=field [order],field [order],...</em>. Le paramètre <em>field</em> peut comporter l’une des chaînes suivantes :<ul><li><strong>Date</strong></li><li><strong>sur le marché</strong></li><li><strong>type d’appareil</strong></li><li><strong>adUnitId</strong></li></ul><p>Le paramètre <em>order</em>, facultatif, peut comporter les valeurs <strong>asc</strong> ou <strong>desc</strong> afin de spécifier l’ordre croissant ou décroissant pour chaque champ. La valeur par défaut est <strong>asc</strong>.</p><p>Voici un exemple de chaîne <em>orderby</em> : <em>orderby=date,market</em></p> |    Non      |
| groupby   | chaîne    | Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants :</p><ul><li><strong>applicationId</strong></li><li><strong>ApplicationName</strong></li><li><strong>Date</strong></li><li><strong>accountCurrencyCode</strong></li><li><strong>sur le marché</strong></li><li><strong>type d’appareil</strong></li><li><strong>adUnitName</strong></li><li><strong>adUnitId</strong></li><li><strong>pubCenterAppName</strong></li><li><strong>adProvider</strong></li></ul><p>Le paramètre <em>groupby</em> peut être utilisé avec le paramètre <em>aggregationLevel</em>. Par exemple : <em>&amp;groupby=applicationId&amp;aggregationLevel=week</em></p> |    Non      |


### <a name="filter-fields"></a>Champs de filtrage

Le paramètre *filter* du corps de la demande contient une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ et une valeur qui sont associés aux opérateurs **eq** ou **ne**, et les instructions peuvent être combinées à l’aide des opérateurs **and** ou **or**. Voici un exemple de paramètre *filter* :

-   *Filtre = marché eq 'US' et deviceType eq « téléphone »*

Pour obtenir la liste des champs pris en charge, consultez le tableau suivant : Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*.

| Champ | Description                                                              |
|--------|--------------------------------------------------------------------------|
| market    | Chaîne contenant le code pays ISO 3166 du marché au sein duquel les publicités sont diffusées. |
| deviceType    | Une des chaînes suivantes : <strong>Tablet PC/PC</strong> ou <strong>téléphone</strong>. |
| adUnitId    | Chaîne spécifiant un ID d’unité publicitaire à appliquer sur le filtre. |
| pubCenterAppName    | Chaîne spécifiant le nom pubCenter de l’application actuelle à appliquer au filtre. |
| adProvider    | Chaîne spécifiant un nom de fournisseur publicitaire à appliquer au filtre. |
| date    | Chaîne spécifiant une date au format AAAA/MM/JJ à appliquer au filtre. |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre plusieurs requêtes de récupération des données de performances publicitaires. Remplacez la valeur *applicationId* par l’ID Windows Store de votre application.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/adsperformance?applicationId=9NBLGGH4R315&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/adsperformance?applicationId=9NBLGGH4R315&startDate=8/1/2015&endDate=8/31/2015&skip=0&$filter=market eq 'US' and deviceType eq 'phone’ eq 'US'; and gender eq 'm'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse


### <a name="response-body"></a>Corps de la réponse

| Valeur      | Type   | Description                                                                                                                                                                                                                                                                            |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Valeur      | tableau  | Tableau d’objets comportant les données agrégées de performances publicitaires. Pour plus d’informations sur les données de chaque objet, consultez la section [Valeurs de performances publicitaires](#ad-performance-values) ci-dessous.                                                                                                                      |
| @nextLink  | chaîne | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête est défini sur 5 mais que la requête présente plus de 5 éléments de données. |
| TotalCount | entier    | Nombre total de lignes dans les résultats de la requête.                          |


### <a name="ad-performance-values"></a>Valeurs de performances publicitaires

Les éléments du tableau *Value* comportent les valeurs suivantes :

| Valeur               | Type   | Description                                                                                                                                                                                                                              |
|---------------------|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| date                | chaîne | Première date dans la plage de dates des données de performances publicitaires. Si la requête spécifiait un jour précis, cette valeur correspond à la date. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| applicationId       | chaîne | ID Windows Store de l’application pour laquelle vous récupérez les données de performances publicitaires.     |
| applicationName     | chaîne | Nom d’affichage de l’application.                         |
| adUnitId           | chaîne | ID de l’unité publicitaire.        |
| adUnitName           | chaîne | Le nom de l’unité d’ad, comme spécifié par le développeur dans l’espace partenaires.              |
| adProvider           |  chaîne  |  Nom du fournisseur publicitaire.   |
| deviceType          | chaîne | Nom de l’appareil sur lequel les publicités sont diffusées. Pour obtenir la liste des chaînes prises en charge, consultez la section [Champs de filtrage](#filter-fields) ci-dessus.                              |
| market              | chaîne | Code pays ISO 3166 du marché au sein duquel les publicités sont diffusées.             |
| accountCurrencyCode     | chaîne | Code de devise du compte.        |
| pubCenterAppName       |  chaîne  |   Le nom de l’application de pubCenter associé à l’application Centre de partenaires.   |
| adProviderRequests        | entier | Nombre de requêtes publicitaires pour le fournisseur publicitaire spécifié.                 |
| impressions           | entier | Nombre d’expositions publicitaires.        |
| clicks            | entier | Nombre de clics sur les publicités.       |
| revenueInAccountCurrency       | nombre | Revenu, dans la devise du pays/de la région du compte.       |
| requests              | entier | Nombre de requêtes publicitaires.                 |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "date": "2015-03-09",
      "applicationId": "9NBLGGH4R315",
      "applicationName": "Contoso Demo",
      "market": "US",
      "deviceType": "phone",
      "adUnitId":"10765920",
      "adUnitName":"TestAdUnit",
      "revenueInAccountCurrency": 10.0,
      "impressions": 1000,
      "requests": 10000,
      "clicks": 1,
      "accountCurrencyCode":"USD"
    },
    {
      "date": "2015-03-09",
      "applicationId": "9NBLGGH4R315",
      "applicationName": "Contoso Demo",
      "market": "US",
      "deviceType": "phone",
      "adUnitId":"10795110",
      "adUnitName":"TestAdUnit2",
      "revenueInAccountCurrency": 20.0,
      "impressions": 2000,
      "requests": 20000,
      "clicks": 3,
      "accountCurrencyCode":"USD"
    },
  ],
  "@nextLink": "adsperformance?applicationId=9NBLGGH4R315&aggregationLevel=week&startDate=2015/03/01&endDate=2016/02/01&top=2&skip=2",
  "TotalCount": 191753
}

```

## <a name="related-topics"></a>Rubriques connexes

* [Rapport sur les performances publicitaires](../publish/advertising-performance-report.md)
* [Accéder aux données d’analytique à l’aide des services de Microsoft Store](access-analytics-data-using-windows-store-services.md)
