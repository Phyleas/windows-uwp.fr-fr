---
ms.assetid: 4E4CB1E3-D213-4324-91E4-7D4A0EA19C53
description: Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir les données d’utilisation d’application mensuelle pour une plage de dates donnée et d’autres filtres facultatifs.
title: Obtenir l’utilisation mensuelle des applications
ms.date: 08/15/2018
ms.topic: article
keywords: Windows 10, UWP, services Store, API Microsoft Store Analytics, utilisation
ms.localizationpriority: medium
ms.openlocfilehash: a1b82e1f538a0abfda8cb8d4f7ac677464025c8e
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86492814"
---
# <a name="get-monthly-app-usage"></a>Obtenir l’utilisation mensuelle des applications

Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir des données d’utilisation agrégées (à l’exclusion de la Xbox multijoueur) au format JSON pour une application pendant une plage de dates donnée (les 90 derniers jours uniquement) et d’autres filtres facultatifs. Ces informations sont également disponibles dans le [rapport d’utilisation](../publish/usage-report.md) dans l’espace partenaires.

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) pour l’API Microsoft Store Analytics.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête

### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de demande                                                                 |
|--------|-----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagemonthly``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la demande

| Paramètre     | Type   |  Description                                                                                                    |  Obligatoire  |
|---------------|--------|-----------------------------------------------------------------------------------------------------------------|------------|
| applicationId | string | [ID de stockage](in-app-purchases-and-trials.md#store-ids) de l’application dont vous souhaitez récupérer les données de révision. |  Oui       |
| startDate     | Date   | Dans la plage de dates, la date de début de la récupération des avis. La valeur par défaut est la date actuelle.                   |  Non        |
| endDate       | Date   | Dans la plage de dates, la date de début de la récupération des avis. La valeur par défaut est la date actuelle.                     |  Non        |
| top           | int    | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données.                          |  Non        |
| skip          | int    | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite.                         |  Non        |  
| Filter        |string  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction contient un nom de champ du corps de la réponse et une valeur qui sont associés aux opérateurs EQ ou ne, et les instructions peuvent être combinées à l’aide de and ou de ou de. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre filter. Vous pouvez spécifier les champs suivants dans le corps de la réponse : <ul><li>**négoci**</li><li>**deviceType**</li><li>**packageVersion**</li></ul>                                                                                                                                              | Non         |  
| orderby       | string | Instruction commandant les valeurs des données de résultats. La syntaxe est <em>orderby = Field [Order], champ [Order],...</em>. Le paramètre <em>Field</em> peut être l’une des chaînes suivantes :<ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**négoci**</li><li>**packageVersion**</li><li>**deviceType**</li><li>**subscriptionName**</li><li>**monthlySessionCount**</li><li>**engagementDurationMinutes**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li><li>**averageDailyActiveUsers**</li><li>**averageDailyActiveDevices**</li></ul><p>Le paramètre <em>order</em>, facultatif, peut comporter les valeurs **asc** ou **desc** afin de spécifier l’ordre croissant ou décroissant pour chaque champ. La valeur par défaut est **ASC**.</p><p>Voici un exemple de chaîne <em>orderby</em> : <em>orderby = date, Market</em></p> |  Non        |
| groupby       | string | Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants dans le corps de la réponse :<ul><li>**applicationName**</li><li>**subscriptionName**</li><li>**deviceType**</li><li>**packageVersion**</li><li>**négoci**</li><li>**date**</li></ul><p>Les lignes de données renvoyées comportent les champs spécifiés dans le paramètre <em>groupby</em>, ainsi que dans les paramètres suivants :</p><ul><li>**applicationId**</li><li>**subscriptionName**</li><li>**monthlySessionCount**</li><li>**engagementDurationMinutes**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li><li>**averageDailyActiveUsers**</li><li>**averageDailyActiveDevices**</li></ul><p>Le paramètre <em>groupby</em> peut être utilisé avec le paramètre <em>aggregationLevel</em>. Par exemple : <em> &amp; GroupBy = ageGroup, Market &amp; aggregationLevel = week</em></p> |  Non        |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre une demande d’obtention des données d’utilisation d’application mensuelle. Remplacez la valeur *applicationId* par l’ID Windows Store de votre application.

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagemonthly?applicationId=XXXXXXXXXXXX&startDate=2018-06-01&endDate=2018-07-01 HTTP/1.1  
Authorization: Bearer <your access token>
```

## <a name="response"></a>response


### <a name="response-body"></a>Response body

| Valeur      | Type   | Description                                                                                                                         |
|------------|--------|-------------------------------------------------------------------------------------------------------------------------------------|
| Valeur      | tableau  | Tableau d’objets qui contiennent des données d’utilisation d’agrégation. Pour plus d’informations sur les données de chaque objet, consultez le tableau suivant. |
| @nextLink  | string | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête est défini sur 10000, mais que plus de 10000 lignes de données d’avis sont associées à la requête.                 |
| TotalCount | int    | Nombre total de lignes dans les résultats de la requête.                                                                          |

 
### <a name="usage-values"></a>Valeurs d’utilisation

Les éléments du tableau *Value* comportent les valeurs suivantes :

| Valeur                     | Type    | Description                                                                                 |
|---------------------------|---------|---------------------------------------------------------------------------------------------|
| Date                      | string  | Première date dans la plage de dates pour les données d’utilisation. Si la requête spécifiait un jour précis, cette valeur correspond à la date. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates.                          |
| applicationId             | string  | ID de stockage de l’application pour laquelle vous récupérez des données d’utilisation.                            |
| applicationName           | string  | Nom d’affichage de l’application.                                                                |
| market                    | string  | Code du pays ISO 3166 du marché où le client a utilisé votre application.                   |
| packageVersion            | string  | Version du package où l’utilisation s’est produite.                                            |
| deviceType                | string  | L’une des chaînes suivantes qui spécifie le type d’appareil où l’utilisation s’est produite :<ul><li>**PC**</li><li>**Numéros**</li><li>**Console-Xbox One**</li><li>**Console-série Xbox X**</li><li>**Tablet**</li><li>**IoT**</li><li>**Serveur**</li><li>**Hologrammes**</li><li>**Unknown**</li></ul>                                                                                                                           |
| subscriptionName          | string  | Indique si l’utilisation s’est produite via la passe de jeu Xbox.                                              |
| monthlySessionCount       | long    | Nombre de sessions utilisateur au cours de ce mois.                                              |
| engagementDurationMinutes | double  | Les minutes où les utilisateurs utilisent activement votre application sont mesurés sur une période de temps distincte, en commençant à la fin de l’exécution de l’application (démarrage du processus) et en finissant lorsqu’elle se termine (fin du processus) ou après une période d’inactivité.                               |
| monthlyActiveUsers        | long    | Le nombre de clients utilisant l’application le mois.                                           |
| monthlyActiveDevices      | long    | Nombre d’appareils exécutant votre application pendant une période distincte, en commençant à la fin de l’exécution de l’application (démarrer le processus) et en finissant lorsqu’elle se termine (fin du processus) ou après une période d’inactivité.                                                        |
| monthlyNewUsers           | long    | Le nombre de clients qui ont utilisé votre application pour la première fois ce mois-ci.                    |
| averageDailyActiveUsers   | double  | Nombre moyen de clients utilisant l’application sur une base quotidienne.                             |
| averageDailyActiveDevices | double  | Nombre moyen d’appareils utilisés pour interagir quotidiennement avec votre application par tous les utilisateurs. |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```http
{
  "Value": [
    {
      "date": "2018-06-01",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "monthlySessionCount": 582973,
      "engagementDurationMinutes": 16941418.7,
      "monthlyActiveUsers": 139604,
      "monthlyActiveDevices": 132296,
      "monthlyNewUsers": 88127,
      "averageDailyActiveUsers": 9099.23,
      "averageDailyActiveDevices": 8999.0
    },
    {
      "date": "2018-07-01",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "monthlySessionCount": 681460,
      "engagementDurationMinutes": 21656645.3,
      "monthlyActiveUsers": 130481,
      "monthlyActiveDevices": 123583,
      "monthlyNewUsers": 78465,
      "averageDailyActiveUsers": 8257.55,
      "averageDailyActiveDevices": 8170.58
    }
  ],
  "@nextLink": null,
  "TotalCount": 2
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Accéder aux données d’analyse à l’aide des services Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtient le ussage d’application quotidien](get-app-usage-daily.md)
* [Obtenir des acquisitions d’applications](get-app-acquisitions.md)
* [Obtenir des acquisitions d’extensions](get-in-app-acquisitions.md)
* [Obtenir les données de rapport d’erreurs](get-error-reporting-data.md)
