---
ms.assetid: 1A69A388-B1CC-4D2C-886B-EA07E6E60252
description: Utilisez cette méthode dans l’API de soumission Microsoft Store pour supprimer une soumission de vol de package existante.
title: Supprime une soumission de version d’évaluation du package
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store API de soumission, envoi de vol, suppression, vol de package
ms.localizationpriority: medium
ms.openlocfilehash: 9cfac0958e5f4b4175fafe1d22a42b2b6eae08c2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171653"
---
# <a name="delete-a-package-flight-submission"></a>Supprime une soumission de version d’évaluation du package

Utilisez cette méthode dans l’API de soumission Microsoft Store pour supprimer une soumission de vol de package existante.

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) pour l’API de soumission Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de demande                                                      |
|--------|------------------------------------------------------------------|
| Suppression    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationid}/flights/{flightId}/submissions/{submissionId}` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la demande

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Obligatoire. ID Windows Store de l’application qui contient la soumission de version d’évaluation du package à supprimer. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](../publish/view-app-identity-details.md).  |
| flightId | string | Obligatoire. ID de la version d’évaluation du package qui contient la soumission à supprimer. Cet ID est disponible dans les données de réponse pour les demandes de [création d’un vol de packages](create-a-flight.md) et l' [extraction des vols de packages pour une application](get-flights-for-an-app.md). Pour un vol créé dans l’espace partenaires, cet ID est également disponible dans l’URL de la page de vol de l’espace partenaires.  |
| submissionId | string | Obligatoire. ID de la soumission à supprimer. Cet ID est disponible dans les données de réponse pour les demandes de [création d’une soumission de vol de packages](create-a-flight-submission.md). Pour une soumission qui a été créée dans l’espace partenaires, cet ID est également disponible dans l’URL de la page de soumission dans l’espace partenaires.  |


### <a name="request-body"></a>Corps de la demande

Ne fournissez pas de corps de requête pour cette méthode.


### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment supprimer une soumission de version d’évaluation du package.

```json
DELETE https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>response

En cas de succès, cette méthode retourne un corps de réponse vide.

## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Code d'erreur |  Description   |
|--------|------------------|
| 400  | Les paramètres de la requête ne sont pas valides. |
| 404  | La soumission spécifiée est introuvable. |
| 409  | L’envoi spécifié a été trouvé, mais il n’a pas pu être supprimé dans son état actuel, ou l’application utilise une fonctionnalité de l’espace partenaires qui n’est [actuellement pas prise en charge par l’API de soumission Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |


## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gérer les soumissions de versions d’évaluation de package](manage-flight-submissions.md)
* [Obtenir une soumission de version d’évaluation du package](get-a-flight-submission.md)
* [Créer une soumission de version d’évaluation du package](create-a-flight-submission.md)
* [Valider une soumission de version d’évaluation de package](commit-a-flight-submission.md)
* [Mettre à jour une soumission de version d’évaluation de package](update-a-flight-submission.md)
* [Obtenir l’état d’une soumission de version d’évaluation de package](get-status-for-a-flight-submission.md)