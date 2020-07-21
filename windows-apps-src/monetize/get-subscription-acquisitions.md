---
description: Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir les données d’acquisition d’un abonnement complémentaire pendant une plage de dates donnée et d’autres filtres facultatifs.
title: Obtenir les acquisitions d’extensions d’inscription
ms.date: 01/25/2018
ms.topic: article
keywords: Windows 10, UWP, services Store, API Microsoft Store Analytics, abonnements
ms.openlocfilehash: 4c307dbf7d17251e3d3c5f8792d8ea3d049924d9
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493544"
---
# <a name="get-subscription-add-on-acquisitions"></a>Obtenir les acquisitions d’extensions d’inscription

Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir des données d’acquisition agrégées pour les abonnements complémentaires de votre application pendant une plage de dates donnée et d’autres filtres facultatifs.

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) pour l’API Microsoft Store Analytics.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de demande                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/subscriptions``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description          |
|---------------|--------|--------------|
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la demande

| Paramètre        | Type   |  Description      |  Obligatoire  
|---------------|--------|---------------|------|
| applicationId | string | [ID de stockage](in-app-purchases-and-trials.md#store-ids) de l’application pour laquelle vous souhaitez récupérer les données d’acquisition du module complémentaire d’abonnement. |  Oui  |
| subscriptionProductId  | string | [ID de stockage](in-app-purchases-and-trials.md#store-ids) du module complémentaire d’abonnement pour lequel vous souhaitez récupérer les données d’acquisition. Si vous ne spécifiez pas cette valeur, cette méthode retourne les données d’acquisition pour tous les modules complémentaires d’abonnement de l’application spécifiée.  | Non  |
| startDate | Date | Date de début dans la plage de dates des données d’acquisition du module complémentaire d’abonnement à récupérer. La valeur par défaut est la date actuelle. |  Non  |
| endDate | Date | Date de fin dans la plage de dates des données d’acquisition du module complémentaire d’abonnement à récupérer. La valeur par défaut est la date actuelle. |  Non  |
| top | int | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut s’il n’est pas spécifié sont 100. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | int | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=100 et skip=0 pour obtenir les 100 premières lignes de données, top=100 et skip=100 pour obtenir les 100 lignes suivantes, et ainsi de suite. |  Non  |
| Filter | string  | Une ou plusieurs instructions qui filtrent le corps de la réponse. Chaque instruction peut utiliser les opérateurs **eq** ou **ne** ; les instructions peuvent être combinées à l’aide de **and** ou **or**. Vous pouvez spécifier les chaînes suivantes dans les instructions de filtre (celles-ci correspondent aux [valeurs dans le corps de la réponse](#subscription-acquisition-values)) : <ul><li><strong>date</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>applicationName</strong></li><li><strong>skuId</strong></li><li><strong>négoci</strong></li><li><strong>deviceType</strong></li></ul><p>Voici un exemple de paramètre de *filtre* : <em>Filter = date EQ' 2017-07-08 '</em>.</p> | Non   |
| aggregationLevel | string | Indique la plage de temps pendant laquelle récupérer les données agrégées. Il peut s’agit des chaînes suivantes : <strong>day</strong>, <strong>week</strong> ou <strong>month</strong>. Par défaut, la valeur est <strong>day</strong>. | Non |
| orderby | string | Instruction qui classe les valeurs des données de résultat pour chaque acquisition du module complémentaire d’abonnement. La syntaxe est <em>orderby = Field [Order], champ [Order],...</em>. Le paramètre <em>Field</em> peut être l’une des chaînes suivantes :<ul><li><strong>date</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>applicationName</strong></li><li><strong>skuId</strong></li><li><strong>négoci</strong></li><li><strong>deviceType</strong></li></ul><p>Le paramètre <em>order</em>, facultatif, peut comporter les valeurs <strong>asc</strong> ou <strong>desc</strong> afin de spécifier l’ordre croissant ou décroissant pour chaque champ. La valeur par défaut est <strong>ASC</strong>.</p><p>Voici un exemple de chaîne <em>orderby</em> : <em>orderby = date, Market</em></p> |  Non  |
| groupby | string | Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants :<ul><li><strong>date</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>applicationName</strong></li><li><strong>skuId</strong></li><li><strong>négoci</strong></li><li><strong>deviceType</strong></li></ul><p>Le paramètre <em>groupby</em> peut être utilisé avec le paramètre <em>aggregationLevel</em>. Par exemple : <em>GroupBy = Market &amp; aggregationLevel = week</em></p> |  Non  |


### <a name="request-example"></a>Exemple de requête

Les exemples suivants montrent comment récupérer des données d’acquisition de compléments d’abonnement. Remplacez la valeur *ApplicationID* par l’ID de magasin approprié pour votre application.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/subscriptions?applicationId=9NBLGGGZ5QDR&startDate=2017-07-07&endDate=2017-07-08 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>response


### <a name="response-body"></a>Response body

| Valeur      | Type   | Description         |
|------------|--------|------------------|
| Valeur      | tableau  | Tableau d’objets qui contiennent les données d’acquisition du module complémentaire d’abonnement de l’agrégat. Pour plus d’informations sur les données de chaque objet, consultez la section [valeurs d’acquisition d’abonnement](#subscription-acquisition-values) ci-dessous.             |
| @nextLink  | string | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est retournée si le paramètre **supérieur** de la demande est défini sur 100, mais qu’il y a plus de 100 lignes de données d’acquisition du module complémentaire d’abonnement pour la requête. |
| TotalCount | int    | Nombre total de lignes dans les résultats de la requête.       |


<span id="subscription-acquisition-values" />

### <a name="subscription-acquisition-values"></a>Valeurs d’acquisition d’abonnement

Les éléments du tableau *Value* comportent les valeurs suivantes :

| Valeur               | Type    | Description        |
|---------------------|---------|---------------------|
| Date                | string  | Première date dans la plage de dates des données d’acquisition. Si la requête spécifiait un jour précis, cette valeur correspond à la date. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| subscriptionProductId      | string  | [ID de stockage](in-app-purchases-and-trials.md#store-ids) du module complémentaire d’abonnement pour lequel vous extrayez des données d’acquisition.    |
| subscriptionProductName    | string  | Nom complet du module complémentaire d’abonnement.         |
| applicationId       | string  | [ID de stockage](in-app-purchases-and-trials.md#store-ids) de l’application pour laquelle vous récupérez les données d’acquisition du module complémentaire d’abonnement.   |
| applicationName     | string  | Nom d’affichage de l’application.     |
| skuId     | string  | ID de la [référence SKU](in-app-purchases-and-trials.md#products-skus) du module complémentaire d’abonnement pour lequel vous extrayez des données d’acquisition.     |
| deviceType          | string  |  L’une des chaînes suivantes qui spécifie le type de périphérique qui a effectué l’acquisition :<ul><li><strong>PC</strong></li><li><strong>Numéros</strong></li><li><strong>Console-Xbox One</strong></li><li><strong>Console-série Xbox X</strong></li><li><strong>IoT</strong></li><li><strong>Hologrammes</strong></li><li><strong>Unknown</strong></li></ul>       |
| market           | string  | Le code pays ISO 3166 du marché dans lequel l’acquisition s’est produite.     |
| currencyCode         | string  | Code de la devise au format ISO 4217 pour les ventes brutes avant taxes.       |
| grossSalesBeforeTax           | entier  | Ventes brutes dans la devise locale spécifiée par la valeur *currencyCode* .     |
| totalActiveCount             | entier  | Nombre total d’abonnements actifs au cours de la période spécifiée. Cela équivaut à la somme des valeurs *goodStandingActiveCount*, *pendingGraceActiveCount*, *graceActiveCount*et *lockedActiveCount* .  |
| totalChurnCount              | entier  | Nombre total d’abonnements qui ont été désactivés au cours de la période spécifiée. Cela équivaut à la somme des valeurs *billingChurnCount*, *nonRenewalChurnCount*, *refundChurnCount*, *chargebackChurnCount*, *earlyChurnCount*et *otherChurnCount* .   |
| newCount            | entier  | Nombre de nouvelles acquisitions d’abonnements au cours de la période spécifiée, y compris les versions d’évaluation.    |
| renewCount     | entier  | Nombre de renouvellements d’abonnements au cours de la période spécifiée, y compris les renouvellements initiés par l’utilisateur et les renouvellements automatiques.        |
| goodStandingActiveCount | entier | Nombre d’abonnements qui étaient actifs au cours de la période spécifiée et où la date d’expiration est >= la valeur de date de fin *de la requête* .    |
| pendingGraceActiveCount | entier | Nombre d’abonnements actifs au cours de la période spécifiée, mais avec un échec de facturation, et où la date d’expiration de l’abonnement est >= *la valeur de la date* de fin de la requête.     |
| graceActiveCount | entier | Nombre d’abonnements qui ont été actifs au cours de la période spécifiée mais dont la facturation a échoué, et où :<ul><li>La date d’expiration de l’abonnement est < la valeur de *EndDate* pour la requête.</li><li>La fin de la période de grâce est >= la valeur de *EndDate* .</li></ul>        |
| lockedActiveCount | entier | Le nombre d’abonnements qui se trouvaient dans *rappels* (autrement dit, l’abonnement est proche de l’expiration et Microsoft tente d’acquérir des fonds pour renouveler automatiquement l’abonnement) au cours de la période spécifiée, et où :<ul><li>La date d’expiration de l’abonnement est < la valeur de *EndDate* pour la requête.</li><li>La fin de la période de grâce est <= la valeur de *EndDate* .</li></ul>        |
| billingChurnCount | entier | Nombre d’abonnements désactivés au cours de la période spécifiée en raison d’un échec de traitement des frais de facturation et de l’emplacement des abonnements précédemment dans rappels.        |
| nonRenewalChurnCount | entier | Nombre d’abonnements qui ont été désactivés au cours de la période spécifiée, car ils n’ont pas été renouvelés.        |
| refundChurnCount | entier | Nombre d’abonnements qui ont été désactivés au cours de la période spécifiée, car ils ont été remboursés.        |
| chargebackChurnCount | entier | Nombre d’abonnements désactivés au cours de la période spécifiée en raison d’une facturation.        |
| earlyChurnCount | entier | Nombre d’abonnements qui ont été désactivés au cours de la période spécifiée alors qu’ils étaient en bonne position.        |
| otherChurnCount | entier | Nombre d’abonnements qui ont été désactivés au cours de la période spécifiée pour d’autres raisons.        |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "date": "2017-07-08",
      "subscriptionProductId": "9KDLGHH6R365",
      "subscriptionProductName": "Contoso App Subscription with One Month Free Trial",
      "applicationId": "9NBLGGH4R315",
      "applicationName": "Contoso App",
      "skuId": "0020",
      "market": "Unknown",
      "deviceType": "PC",
      "currencyCode": "USD",
      "grossSalesBeforeTax": 0.0,
      "totalActiveCount": 1,
      "totalChurnCount": 0,
      "newCount": 0,
      "renewCount": 0,
      "goodStandingActiveCount": 1,
      "pendingGraceActiveCount": 0,
      "graceActiveCount": 0,
      "lockedActiveCount": 0,
      "billingChurnCount": 0,
      "nonRenewalChurnCount": 0,
      "refundChurnCount": 0,
      "chargebackChurnCount": 0,
      "earlyChurnCount": 0,
      "otherChurnCount": 0
    },
    {
      "date": "2017-07-08",
      "subscriptionProductId": "9JJFDHG4R478",
      "subscriptionProductName": "Contoso App Monthly Subscription",
      "applicationId": "9NBLGGH4R315",
      "applicationName": "Contoso App",
      "skuId": "0020",
      "market": "US",
      "deviceType": "PC",
      "currencyCode": "USD",
      "grossSalesBeforeTax": 0.0,
      "totalActiveCount": 1,
      "totalChurnCount": 0,
      "newCount": 0,
      "renewCount": 0,
      "goodStandingActiveCount": 1,
      "pendingGraceActiveCount": 0,
      "graceActiveCount": 0,
      "lockedActiveCount": 0,
      "billingChurnCount": 0,
      "nonRenewalChurnCount": 0,
      "refundChurnCount": 0,
      "chargebackChurnCount": 0,
      "earlyChurnCount": 0,
      "otherChurnCount": 0
    }
  ],
  "@nextLink": null,
  "TotalCount": 2
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Rapport sur les acquisitions des extensions](../publish/add-on-acquisitions-report.md)
* [Accéder aux données d’analyse à l’aide des services Microsoft Store](access-analytics-data-using-windows-store-services.md)

 

 
