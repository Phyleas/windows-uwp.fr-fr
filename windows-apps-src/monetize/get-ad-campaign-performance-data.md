---
ms.assetid: A26A287C-B4B0-49E9-BB28-6F02472AE1BA
description: Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir des données agrégées des performances des campagnes AD pour l’application spécifiée pendant une plage de dates donnée et d’autres filtres facultatifs.
title: Obtenir les données relatives aux performances de la campagne publicitaire
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, services Store, API Microsoft Store Analytics, campagnes ad
ms.localizationpriority: medium
ms.openlocfilehash: fd933f103bf8964997102731b653e125fae89e12
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162443"
---
# <a name="get-ad-campaign-performance-data"></a>Obtenir les données relatives aux performances de la campagne publicitaire


Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir un résumé agrégé des données de performances des campagnes publicitaires promotionnelles pour vos applications pendant une plage de dates donnée et d’autres filtres facultatifs. Cette méthode renvoie les données au format JSON.

Cette méthode retourne les mêmes données que celles fournies par le [rapport de la campagne Active Directory](/windows/uwp/publish/ad-campaign-report) dans l’espace partenaires. Pour plus d’informations sur les campagnes publicitaires, consultez la page [Création d’une campagne de publicité pour votre application](../publish/create-an-ad-campaign-for-your-app.md).

Pour créer, mettre à jour ou récupérer des détails pour des campagnes publicitaires, vous pouvez utiliser les méthodes de [gestion des campagnes ad](manage-ad-campaigns.md) dans l' [API Microsoft Store promotions](run-ad-campaigns-using-windows-store-services.md).

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) pour l’API Microsoft Store Analytics.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de demande                                                              |
|--------|--------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                |
|---------------|--------|---------------|
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la demande

Pour récupérer les données de performances des campagnes publicitaires d’une application spécifique, utilisez le paramètre *applicationId*. Pour récupérer les données de performances publicitaires de l’ensemble des applications associées à votre compte de développeur, omettez le paramètre *applicationId*.

| Paramètre     | Type   | Description     | Obligatoire |
|---------------|--------|-----------------|----------|
| applicationId   | string    | [ID de stockage](in-app-purchases-and-trials.md#store-ids) de l’application pour laquelle vous souhaitez récupérer les données de performances de la campagne Active Directory. |    Non      |
|  startDate  |  Date   |  La date de début dans la plage de dates des données de performances des campagnes publicitaires à récupérer, au format AAAA/MM/JJ. La valeur par défaut correspond à la date antérieure de 30 jours à la date actuelle.   |   Non    |
| endDate   |  Date   |  La date de fin dans la plage de dates des données de performances des campagnes publicitaires à récupérer, au format AAAA/MM/JJ. La valeur par défaut correspond à la date antérieure d’un jour à la date actuelle.   |   Non    |
| top   |  int   |  Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données.   |   Non    |
| skip   | int    |  Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite.   |   Non    |
| Filter   |  string   |  Une ou plusieurs instructions qui filtrent les lignes de la réponse. Le seul filtre pris en charge est **campaignId**. Chaque instruction peut utiliser les opérateurs **eq** ou **ne** ; les instructions peuvent être combinées à l’aide de **and** ou **or**.  Voici un exemple de paramètre *filter* : ```filter=campaignId eq '100023'```.   |   Non    |
|  aggregationLevel  |  string   | Indique la plage de temps pendant laquelle récupérer les données agrégées. Il peut s’agit des chaînes suivantes : <strong>day</strong>, <strong>week</strong> ou <strong>month</strong>. Par défaut, la valeur est <strong>day</strong>.    |   Non    |
| orderby   |  string   |  <p>Une instruction qui commande les valeurs des données de performances des campagnes publicitaires. La syntaxe est <em>orderby = Field [Order], champ [Order],...</em>. Le paramètre <em>Field</em> peut être l’une des chaînes suivantes :</p><ul><li><strong>date</strong></li><li><strong>campaignId</strong></li></ul><p>Le paramètre <em>order</em>, facultatif, peut comporter les valeurs <strong>asc</strong> ou <strong>desc</strong> afin de spécifier l’ordre croissant ou décroissant pour chaque champ. La valeur par défaut est <strong>ASC</strong>.</p><p>Voici un exemple de chaîne <em>orderby</em> : <em>orderby = date, campaignId</em></p>   |   Non    |
|  groupby  |  string   |  <p>Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants :</p><ul><li><strong>campaignId</strong></li><li><strong>applicationId</strong></li><li><strong>date</strong></li><li><strong>currencyCode</strong></li></ul><p>Le paramètre <em>groupby</em> peut être utilisé avec le paramètre <em>aggregationLevel</em>. Par exemple : <em> &amp; GroupBy = ApplicationId &amp; aggregationLevel = week</em></p>   |   Non    |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre plusieurs requêtes de récupération des données de performances des campagnes publicitaires.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion?aggregationLevel=week&groupby=applicationId,campaignId,date  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion?applicationId=9NBLGGH0XK8Z&startDate=2015/1/20&endDate=2016/8/31&skip=0&filter=campaignId eq '31007388' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>response


### <a name="response-body"></a>Response body

| Valeur      | Type   | Description  |
|------------|--------|---------------|
| Valeur      | tableau  | Un tableau d’objets qui comporte les données agrégées de performances des campagnes publicitaires. Pour plus d’informations sur les données de chaque objet, consultez la section [Objet de performances des campagnes](#campaign-performance-object) ci-dessous.          |
| @nextLink  | string | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête est défini sur 5 mais que la requête présente plus de 5 éléments de données. |
| TotalCount | int    | Nombre total de lignes dans les résultats de la requête.                                |


<span id="campaign-performance-object" />


### <a name="campaign-performance-object"></a>Objet de performances des campagnes

Les éléments du tableau *Value* comportent les valeurs suivantes :

| Valeur               | Type   | Description            |
|---------------------|--------|------------------------|
| Date                | string | La première date dans la plage de dates des données de performances des campagnes publicitaires. Si la requête spécifiait un jour précis, cette valeur correspond à la date. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| applicationId       | string | L’ID Windows Store de l’application pour laquelle vous récupérez les données de performances des campagnes publicitaires.                     |
| campaignId     | string | L’ID de la campagne publicitaire.           |
| lineId     | string |    ID de la [ligne de livraison](manage-delivery-lines-for-ad-campaigns.md) de la campagne Active Directory qui a généré ces données de performances.        |
| currencyCode              | string | Le code de devise du budget de la campagne.              |
| spend          | string |  Le volume budgétaire consacré à la campagne publicitaire.     |
| impressions           | long | Le nombre d’expositions publicitaires pour la campagne.        |
| installs              | long | Le nombre d’installations d’applications associées à la campagne.   |
| clicks            | long | Le nombre de clics sur les publicités de la campagne.      |
| iapInstalls            | long | Le nombre d’installations du module complémentaire (également appelées achats dans l’application ou IAP) liées à la campagne.      |
| activeUsers            | long | Nombre d’utilisateurs ayant cliqué sur une publicité qui fait partie de la campagne et renvoyés à l’application.      |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "date": "2015-04-12",
      "applicationId": "9WZDNCRFJ31Q",
      "campaignId": "4568",
      "lineId": "0001",
      "currencyCode": "USD",
      "spend": 700.6,
      "impressions": 200,
      "installs": 30,
      "clicks": 8,
      "iapInstalls": 0,
      "activeUsers": 0
    },
    {
      "date": "2015-05-12",
      "applicationId": "9WZDNCRFJ31Q",
      "campaignId": "1234",
      "lineId": "0002",
      "currencyCode": "USD",
      "spend": 325.3,
      "impressions": 20,
      "installs": 2,
      "clicks": 5,
      "iapInstalls": 0,
      "activeUsers": 0
    }
  ],
  "@nextLink": "promotion?applicationId=9NBLGGGZ5QDR&aggregationLevel=day&startDate=2015/1/20&endDate=2016/8/31&top=2&skip=2",
  "TotalCount": 1917
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Création d’une campagne de publicité pour votre application](../publish/create-an-ad-campaign-for-your-app.md)
* [Exécuter des campagnes Active Directory à l’aide des services Microsoft Store](run-ad-campaigns-using-windows-store-services.md)
* [Accéder aux données d’analyse à l’aide des services Microsoft Store](access-analytics-data-using-windows-store-services.md)