---
description: Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir les données de l’entonnoir d’acquisition pour une application pendant une plage de dates donnée et d’autres filtres facultatifs.
title: Obtenir les données d’entonnoir d’acquisition d’applications
ms.date: 08/04/2017
ms.topic: article
keywords: Windows 10, UWP, services Store, API Microsoft Store Analytics, acquisition, entonnoir
ms.localizationpriority: medium
ms.openlocfilehash: 817ad85bae642d6b09cf77bb5e7b4bf71b08f594
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493554"
---
# <a name="get-app-acquisition-funnel-data"></a>Obtenir les données d’entonnoir d’acquisition d’applications

Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir les données de l’entonnoir d’acquisition pour une application pendant une plage de dates donnée et d’autres filtres facultatifs. Ces informations sont également disponibles dans le [rapport acquisitions](../publish/acquisitions-report.md#acquisition-funnel) de l’espace partenaires.

## <a name="prerequisites"></a>Prérequis


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) pour l’API Microsoft Store Analytics.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de demande       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la demande

| Paramètre        | Type   |  Description      |  Obligatoire  
|---------------|--------|---------------|------|
| applicationId | string | [ID de stockage](in-app-purchases-and-trials.md#store-ids) de l’application pour laquelle vous souhaitez récupérer les données de l’entonnoir d’acquisition. Exemple d’ID Windows Store : 9WZDNCRFJ3Q8. |  Oui  |
| startDate | Date | Date de début dans la plage de dates des données de l’entonnoir d’acquisition à récupérer. La valeur par défaut est la date actuelle. |  Non  |
| endDate | Date | Date de fin dans la plage de dates des données de l’entonnoir d’acquisition à récupérer. La valeur par défaut est la date actuelle. |  Non  |
| Filter | string  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Pour plus d’informations, voir la section [Champs de filtre](#filter-fields) ci-dessous. | Non   |

 
### <a name="filter-fields"></a>Champs de filtrage

Le paramètre *filter* de la requête contient une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ et une valeur qui sont associés aux opérateurs **eq** ou **ne**, et les instructions peuvent être combinées à l’aide des opérateurs **and** ou **or**.

Les champs de filtre suivants sont pris en charge. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*.

| Champs        |  Description        |
|---------------|-----------------|
| campaignId | Chaîne d’ID pour une [campagne de promotion d’application personnalisée](../publish/create-a-custom-app-promotion-campaign.md) associée à l’acquisition. |
| market | Chaîne contenant le code pays ISO 3166 du marché de l’acquisition. |
| deviceType | L’une des chaînes suivantes qui spécifie le type d’appareil sur lequel l’acquisition s’est produite :<ul><li><strong>PC</strong></li><li><strong>Numéros</strong></li><li><strong>Console-Xbox One</strong></li><li><strong>Console-série Xbox X</strong></li><li><strong>IoT</strong></li><li><strong>Hologrammes</strong></li><li><strong>Unknown</strong></li></ul> |
| ageGroup | L’une des chaînes suivantes qui spécifient le groupe d’âge de l’utilisateur qui a effectué l’acquisition :<ul><li><strong>0 – 17</strong></li><li><strong>18 – 24</strong></li><li><strong>25 – 34</strong></li><li><strong>35 – 49</strong></li><li><strong>50 ou supérieur à</strong></li><li><strong>Unknown</strong></li></ul> |
| gender | L’une des chaînes suivantes qui spécifie le sexe de l’utilisateur qui a effectué l’acquisition :<ul><li><strong>M</strong></li><li><strong>FA</strong></li><li><strong>Unknown</strong></li></ul> |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre plusieurs demandes d’obtention de données de entonnoir d’acquisition pour une application. Remplacez la valeur *applicationId* par l’ID Windows Store de votre application.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=2/1/2017  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel?applicationId=9NBLGGGZ5QDR&startDate=8/1/2016&endDate=8/31/2016&filter=market eq 'US' and gender eq 'm'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>response


### <a name="response-body"></a>Response body

| Valeur      | Type   | Description                  |
|------------|--------|-------------------------------------------------------|
| Valeur      | tableau  | Tableau d’objets qui contiennent les données de l’entonnoir d’acquisition pour l’application. Pour plus d’informations sur les données de chaque objet, consultez la section [valeurs de entonnoir](#funnel-values) ci-dessous.                  |
| TotalCount | int    | Nombre total d’objets dans le tableau de *valeurs* .        |


### <a name="funnel-values"></a>Valeurs de entonnoir

Les objets du tableau de *valeurs* contiennent les valeurs suivantes.

| Valeur               | Type   | Description                           |
|---------------------|--------|-------------------------------------------|
| MetricType                | string | L’une des chaînes suivantes qui spécifie le [type de données de entonnoir](../publish/acquisitions-report.md#acquisition-funnel) inclus dans cet objet :<ul><li><strong>PageView</strong></li><li><strong>Acquisition</strong></li><li><strong>Installer</strong></li><li><strong>Utilisation</strong></li></ul> |
| UserCount       | string | Nombre d’utilisateurs ayant effectué l’étape entonnoir spécifiée par la valeur *MetricType* .             |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "MetricType": "PageView",
      "UserCount": 100
    },
    {
      "MetricType": "Acquisition",
      "UserCount": 80
    },
    {
      "MetricType": "Install",
      "UserCount": 50
    },
    {
      "MetricType": "Usage",
      "UserCount": 10
    }
  ],
  "TotalCount": 4
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Rapport sur les acquisitions](../publish/acquisitions-report.md)
* [Accéder aux données d’analyse à l’aide des services Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir des acquisitions d’applications](get-app-acquisitions.md)
