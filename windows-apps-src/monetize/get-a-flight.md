---
ms.assetid: 87708690-079A-443D-807E-D2BF9F614DDF
description: Utilisez cette méthode dans l’API de soumission Microsoft Store pour obtenir des données pour un vol de package pour une application inscrite auprès de votre compte espace partenaires.
title: Obtient une version d’évaluation du package
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, API de soumission Microsoft Store, vol, vol de packages
ms.localizationpriority: medium
ms.openlocfilehash: 3916328f2c48642596c6a0b11401f583957bc973
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172903"
---
# <a name="get-a-package-flight"></a>Obtient une version d’évaluation du package

Utilisez cette méthode dans l’API de soumission Microsoft Store pour obtenir des données pour un vol de package pour une application inscrite auprès de votre compte espace partenaires.

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) pour l’API de soumission Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de demande                                                      |
|--------|------------------------------------------------------------------|
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la demande

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Obligatoire. ID Windows Store de l’application qui contient la version d’évaluation du package à obtenir. L’ID de magasin de l’application est disponible dans l’espace partenaires.  |
| flightId | string | Obligatoire. ID de la version d’évaluation du package à obtenir. Cet ID est disponible dans les données de réponse pour les demandes de [création d’un vol de packages](create-a-flight.md) et l' [extraction des vols de packages pour une application](get-flights-for-an-app.md). Pour un vol créé dans l’espace partenaires, cet ID est également disponible dans l’URL de la page de vol de l’espace partenaires.  |


### <a name="request-body"></a>Corps de la demande

Ne fournissez pas de corps de requête pour cette méthode.

### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment récupérer les informations sur un version d’évaluation du dont l’ID est 43e448df-97c9-4a43-a0bc-2a445e736bcd pour une application dont la valeur de l’IS Windows Store est 9WZDNCRD91MD.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>response

L’exemple suivant illustre le corps de réponse JSON d’un appel réussi à cette méthode. Pour plus d’informations sur les valeurs figurant dans le corps de réponse, voir les sections suivantes.

```json
{
  "flightId": "43e448df-97c9-4a43-a0bc-2a445e736bcd",
  "friendlyName": "myflight",
  "lastPublishedFlightSubmission": {
    "id": "1152921504621086517",
    "resourceLocation": "flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621086517"
  },
  "pendingFlightSubmission": {
    "id": "115292150462124364",
    "resourceLocation": "flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243647"
  },
  "groupIds": [
    "0"
  ],
  "rankHigherThan": "671c2857-725e-4faf-9e9e-ea1191ef879c"
}
```

### <a name="response-body"></a>Response body

| Valeur      | Type   | Description                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightId            | string  | ID de la version d’évaluation du package. Cette valeur est fournie par l’espace partenaires.  |
| friendlyName           | string  | Nom de la version d’évaluation du package, tel que spécifié par le développeur.   |  
| lastPublishedFlightSubmission       | object | Objet qui fournit des informations sur la dernière soumission publiée de la version d’évaluation du package. Pour plus d’informations, voir la section [Objet de la soumission](#submission_object) ci-dessous.  |
| pendingFlightSubmission        | object  |  Objet qui fournit des informations sur la soumission actuellement en attente pour la version d’évaluation du package. Pour plus d’informations, voir la section [Objet de la soumission](#submission_object) ci-dessous.  |   
| groupIds           | tableau  | Tableau de chaînes qui contiennent les ID des groupes de versions d’évaluation associés à la version d’évaluation du package. Pour plus d’informations sur les groupes de versions d’évaluation, voir [Versions d’évaluation du package](../publish/package-flights.md).   |
| rankHigherThan           | string  | Nom convivial de la version d’évaluation du package classée juste en dessous de la version d’évaluation du package actuelle. Pour plus d’informations sur le classement des groupes de versions d’évaluation, voir [Versions d’évaluation de package](../publish/package-flights.md).  |


<span id="submission_object" />

### <a name="submission-object"></a>Objet de la soumission

Les valeurs *lastPublishedFlightSubmission* et *pendingFlightSubmission* dans le corps de la réponse contiennent des objets qui fournissent des informations sur les ressources d’une soumission de version d’évaluation du package. Ces objets ont les valeurs suivantes.

| Valeur           | Type    | Description                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | string  | ID de la soumission.    |
| resourceLocation   | string  | Chemin relatif à ajouter à l’URI de requête `https://manage.devcenter.microsoft.com/v1.0/my/` de base pour récupérer les données complètes de la soumission.               |


## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Code d'erreur |  Description     |
|--------|---------------------  |
| 400  | La requête n’est pas valide. |
| 404  | La version d’évaluation du package spécifiée est introuvable.   |   
| 409  | L’application utilise une fonctionnalité d’espace partenaires qui n’est [actuellement pas prise en charge par l’API de soumission Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |                                                                                                 


## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Crée une version d’évaluation du package](create-a-flight.md)
* [Supprimer une version d’évaluation de package](delete-a-flight.md)