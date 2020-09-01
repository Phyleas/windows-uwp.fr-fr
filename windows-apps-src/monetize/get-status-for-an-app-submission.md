---
ms.assetid: 039B8810-5C9E-4DB9-A6AF-33E7401311FF
description: Utilisez cette méthode dans l’API de soumission Microsoft Store pour connaître l’état d’une soumission d’application.
title: Obtenir l’état d’une soumission d’application
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store API de soumission, envoi d’applications, état
ms.localizationpriority: medium
ms.openlocfilehash: 4f6adafe9d3ebbc1fcffad830c5214e18e9c515f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175003"
---
# <a name="get-the-status-of-an-app-submission"></a>Obtenir l’état d’une soumission d’application

Utilisez cette méthode dans l’API de soumission Microsoft Store pour connaître l’état d’une soumission d’application. Pour plus d’informations sur le processus de création d’une soumission d’application à l’aide de l’API Microsoft Store soumission, consultez [gérer les envois d’applications](manage-app-submissions.md).

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) pour l’API de soumission Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de demande                                                      |
|--------|------------------------------------------------------------------|
| GET   | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la demande

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Obligatoire. ID Windows Store de l’application qui contient la soumission dont vous voulez obtenir l’état. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](../publish/view-app-identity-details.md).  |
| submissionId | string | Obligatoire. ID de la soumission dont vous voulez obtenir l’état. Cet ID est disponible dans les données de réponse pour les demandes de [création d’une soumission d’application](create-an-app-submission.md). Pour une soumission qui a été créée dans l’espace partenaires, cet ID est également disponible dans l’URL de la page de soumission dans l’espace partenaires.  |


### <a name="request-body"></a>Corps de la demande

Ne fournissez pas de corps de requête pour cette méthode.

### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment obtenir l’état d’une soumission d’application.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621243610/status HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>response

L’exemple suivant illustre le corps de réponse JSON d’un appel réussi à cette méthode. Le corps de la réponse contient des informations sur la soumission spécifiée. Pour plus d’informations sur les valeurs figurant dans le corps de réponse, voir les sections suivantes.

```json
{
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
}
```

### <a name="response-body"></a>Response body

| Valeur      | Type   | Description                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| status           | string  | État de la soumission. Il peut s’agir de l’une des valeurs suivantes : <ul><li>Aucune</li><li>Opération annulée</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publication</li><li>Publié</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Libérer</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | object  |  Contient des détails supplémentaires sur l’état de la soumission, notamment des informations sur les éventuelles erreurs. Pour plus d’informations, voir [Ressource des détails d’état](manage-app-submissions.md#status-details-object). |


## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Code d'erreur |  Description   |
|--------|------------------|
| 404  | La soumission est introuvable. |
| 409  | L’application utilise une fonctionnalité d’espace partenaires qui n’est [actuellement pas prise en charge par l’API de soumission Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |


## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obtenir une soumission d’application](get-an-app-submission.md)
* [Créer une soumission d’applications](create-an-app-submission.md)
* [Valider une soumission d’application](commit-an-app-submission.md)
* [Mettre à jour une soumission d’application](update-an-app-submission.md)
* [Supprimer une soumission d’application](delete-an-app-submission.md)