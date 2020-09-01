---
ms.assetid: 8D4AE532-22EF-4743-9555-A828B24B8F16
description: Utilisez ces méthodes dans l’API de soumission Microsoft Store pour récupérer des données pour les applications qui sont inscrites auprès de votre compte espace partenaires.
title: Obtenir des données d’application
ms.date: 02/28/2018
ms.topic: article
keywords: Windows 10, UWP, API de soumission Microsoft Store, données d’application
ms.localizationpriority: medium
ms.openlocfilehash: 7dfbad9d0aa2bfb69479f168ec262fe67bedb49c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162413"
---
# <a name="get-app-data"></a>Obtenir des données d’application

Utilisez les méthodes suivantes dans l’API Microsoft Store soumission pour obtenir des données pour les applications existantes dans votre compte espace partenaires. Pour obtenir une présentation de l’API de soumission Microsoft Store, y compris les conditions préalables à l’utilisation de l’API, consultez [créer et gérer des soumissions à l’aide des services Microsoft Store](create-and-manage-submissions-using-windows-store-services.md).

Avant de pouvoir utiliser ces méthodes, l’application doit déjà exister dans votre compte espace partenaires. Pour créer ou gérer des soumissions pour des applications, voir les méthodes dans [Gérer les soumissions d’applications](manage-app-submissions.md).

| Méthode | URI                                                                                             | Description                                                 |
|------- |------------------------------------------------------------------------------------------------ |------------------------------------------------------------ |
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications`                                   | [Obtenir des données pour toutes vos applications](get-all-apps.md)               |
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}`                   | [Obtenir des données pour une application spécifique](get-an-app.md)                |
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listinappproducts` | [Obtenir des extensions pour une application](get-add-ons-for-an-app.md)         |
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listflights`       | [Obtenir des versions d’évaluation du package pour une application](get-flights-for-an-app.md) |

## <a name="prerequisites"></a>Prérequis

Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) pour l’API de soumission Microsoft Store avant d’essayer d’utiliser l’une de ces méthodes.

## <a name="data-resources"></a>Ressources de données

Les méthodes d’API de soumission Microsoft Store pour obtenir des données d’application utilisent les ressources de données JSON suivantes.

<span id="application_object" />

### <a name="application-resource"></a>Ressource d'application

Cette ressource représente une application inscrite dans votre compte.

```json
{
  "id": "9NBLGGH4R315",
  "primaryName": "ApiTestApp",
  "packageFamilyName": "30481DevCenterAPITester.ApiTestAppForDevbox_ng6try80pwt52",
  "packageIdentityName": "30481DevCenterAPITester.ApiTestAppForDevbox",
  "publisherName": "CN=…",
  "firstPublishedDate": "1601-01-01T00:00:00Z",
  "lastPublishedApplicationSubmission": {
    "id": "1152921504621086517",
    "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621086517"
  },
  "pendingApplicationSubmission": {
    "id": "1152921504621243487",
    "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621243487"
  },
  "hasAdvancedListingPermission": true
}
```

Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description       |
|-----------------|---------|---------------------|
| id            | string  | ID Windows Store de l’application. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](../publish/view-app-identity-details.md).   |
| primaryName   | string  | Nom principal de l’application.      |
| packageFamilyName | string  | Nom de la famille de packages de l’application.      |
| packageIdentityName          | string  | Nom de l’identité du package de l’application.                       |
| publisherName       | string  | ID de l’éditeur Windows associé à l’application. Cela correspond à la valeur **package/Identity/Publisher** qui apparaît sur la page identité de l' [application](../publish/view-app-identity-details.md) pour l’application dans l’espace partenaires.       |
| firstPublishedDate      | string  | Date de la première publication de l’application, au format ISO 8601.   |
| lastPublishedApplicationSubmission       | object | [Ressource de soumission](#submission_object) qui fournit des informations sur la dernière soumission publiée de l’application.    |
| pendingApplicationSubmission        | object  |  [Ressource de soumission](#submission_object) qui fournit des informations sur la soumission actuellement en attente pour l’application.   |   
| hasAdvancedListingPermission        | boolean  |  Indique si vous pouvez configurer les [gamingOptions](manage-app-submissions.md#gaming-options-object) ou les [codes](manage-app-submissions.md#trailer-object) de fin pour les soumissions de l’application. Cette valeur est true pour les soumissions créées après le 2017 mai. |  |


<span id="add-on-object" />

### <a name="add-on-resource"></a>Ressource d’extension

Cette ressource fournit des informations sur une extension.

```json
{
    "inAppProductId": "9WZDNCRD7DLK"
}
```

Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description         |
|-----------------|---------|----------------------|
| inAppProductId            | string  | ID Windows Store de l’extension. Cette valeur est fournie par le Windows Store. Exemple d’ID Windows Store : 9NBLGGH4TNMP.   |


<span id="flight-object" />

### <a name="flight-resource"></a>Ressource de version d’évaluation du package

Cette ressource fournit des informations sur une version d’évaluation du package pour une application.

```json
{
    "flightId": "7bfc11d5-f710-47c5-8a98-e04bb5aad310",
    "friendlyName": "myflight",
    "lastPublishedFlightSubmission": {
        "id": "1152921504621086517",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621086517"
    },
    "pendingFlightSubmission": {
        "id": "1152921504621215786",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621215786"
    },
    "groupIds": [
        "1152921504606962205"
    ],
    "rankHigherThan": "Non-flighted submission"
}
```

Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description           |
|-----------------|---------|------------------------|
| flightId            | string  | ID de la version d’évaluation du package. Cette valeur est fournie par l’espace partenaires.  |
| friendlyName           | string  | Nom de la version d’évaluation du package, tel que spécifié par le développeur.   |
| lastPublishedFlightSubmission       | object | [Ressource de soumission](#submission_object) qui fournit des informations sur la dernière soumission publiée de la version d’évaluation du package.   |
| pendingFlightSubmission        | object  |  [Ressource de soumission](#submission_object) qui fournit des informations sur la soumission actuellement en attente pour la version d’évaluation du package.  |    
| groupIds           | tableau  | Tableau de chaînes qui contiennent les ID des groupes de versions d’évaluation associés à la version d’évaluation du package. Pour plus d’informations sur les groupes de versions d’évaluation, voir [Versions d’évaluation du package](../publish/package-flights.md).   |
| rankHigherThan           | string  | Nom convivial de la version d’évaluation du package classée juste en dessous de la version d’évaluation du package actuelle. Pour plus d’informations sur le classement des groupes de versions d’évaluation, voir [Versions d’évaluation de package](../publish/package-flights.md).  |


<span id="submission_object" />

### <a name="submission-resource"></a>Ressource de soumission

Cette ressource fournit des informations sur une soumission. L’exemple suivant illustre le format de cette ressource.

```json
{
  "pendingApplicationSubmission": {
    "id": "1152921504621243487",
    "resourceLocation": "applications/9WZDNCRD9MMD/submissions/1152921504621243487"
  }
}
```

Cette ressource a les valeurs suivantes.

| Valeur              | Type   | Description               |
|--------------------|--------|---------------------------|
| id                 | string | ID de la soumission. |
| resourceLocation   | string | Chemin relatif à ajouter à l’URI de requête ```https://manage.devcenter.microsoft.com/v1.0/my/``` de base pour récupérer les données complètes de la soumission. |

 
## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gérer les envois d’application à l’aide de l’API de soumission Microsoft Store](manage-app-submissions.md)
* [Obtenir toutes les applications](get-all-apps.md)
* [Obtenir une application](get-an-app.md)
* [Obtenir des extensions pour une application](get-add-ons-for-an-app.md)
* [Obtenir des versions d’évaluation du package pour une application](get-flights-for-an-app.md)