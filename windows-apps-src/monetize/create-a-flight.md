---
ms.assetid: 8C1E9E36-13AF-4386-9D0F-F9CB320F02F5
description: Utilisez cette méthode dans l’API de soumission Microsoft Store pour créer un vol de package pour une application inscrite auprès de votre compte espace partenaires.
title: Crée une version d’évaluation du package
ms.date: 04/16/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store API de soumission, créer un vol
ms.localizationpriority: medium
ms.openlocfilehash: ec087d590c0286eb99c3ad2524036d9fd4230508
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172943"
---
# <a name="create-a-package-flight"></a>Crée une version d’évaluation du package

Utilisez cette méthode dans l’API de soumission Microsoft Store pour créer un vol de package pour une application inscrite auprès de votre compte espace partenaires.

> [!NOTE]
> Cette méthode crée un vol de package sans aucune soumission. Pour créer une soumission pour une version d’évaluation de package, voir les méthodes présentées dans l’article [Gérer les soumissions de version d’évaluation de package](manage-flight-submissions.md).

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) pour l’API de soumission Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de demande                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la demande

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Obligatoire. ID Windows Store de l’application pour laquelle vous voulez créer une version d’évaluation de package. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](../publish/view-app-identity-details.md).  |


### <a name="request-body"></a>Corps de la demande

Le corps de la requête contient les paramètres suivants.

|  Paramètre  |  Type  |  Description  |  Obligatoire  |
|------|------|------|------|
|  friendlyName  |  string  |  Nom de la version d’évaluation du package, tel que spécifié par le développeur.  |  Non  |
|  groupIds  |  tableau  |  Tableau de chaînes qui contiennent les ID des groupes de versions d’évaluation associés à la version d’évaluation du package. Pour plus d’informations sur les groupes de versions d’évaluation, voir [Versions d’évaluation du package](../publish/package-flights.md).  |  Non  |
|  rankHigherThan  |  string  |  Nom convivial de la version d’évaluation du package classée juste en dessous de la version d’évaluation du package actuelle. Si vous ne définissez pas ce paramètre, la nouvelle version d’évaluation de package est classée au-dessus de toutes les autres. Pour plus d’informations sur le classement des groupes de versions d’évaluation, voir [Versions d’évaluation de package](../publish/package-flights.md).    |  Non  |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment créer une nouvelle version d’évaluation de package pour une application ayant l’ID Windows Store 9WZDNCRD911W.

```syntax
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1Q...
Content-Type: application/json
{
  "friendlyName": "myflight",
  "groupIds": [
    0
  ],
  "rankHigherThan": null
}

```

## <a name="response"></a>response

L’exemple suivant illustre le corps de réponse JSON d’un appel réussi à cette méthode. Pour plus d’informations sur les valeurs figurant dans le corps de réponse, voir les sections suivantes.

```json
{
  "flightId": "43e448df-97c9-4a43-a0bc-2a445e736bcd",
  "friendlyName": "myflight",
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
| friendlyName           | string  | Nom de la version d’évaluation du package, tel que spécifié dans la requête.   |  
| groupIds           | tableau  | Tableau de chaînes qui contiennent les ID des groupes de versions d’évaluation associés à la version d’évaluation de package, comme spécifié dans la requête. Pour plus d’informations sur les groupes de versions d’évaluation, voir [Versions d’évaluation du package](../publish/package-flights.md).   |
| rankHigherThan           | string  | Nom convivial de la version d’évaluation de package qui est classée juste en dessous de la version d’évaluation de package actuelle, comme spécifié dans la requête. Pour plus d’informations sur le classement des groupes de versions d’évaluation, voir [Versions d’évaluation de package](../publish/package-flights.md).  |


## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Code d'erreur |  Description   |
|--------|------------------|
| 400  | La requête n’est pas valide. |
| 409  | Le vol de package n’a pas pu être créé en raison de son état actuel, ou l’application utilise une fonctionnalité d’espace partenaires qui n’est [actuellement pas prise en charge par l’API de soumission Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obtient une version d’évaluation du package](get-a-flight.md)
* [Supprimer une version d’évaluation de package](delete-a-flight.md)