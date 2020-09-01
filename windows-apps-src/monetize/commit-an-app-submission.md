---
ms.assetid: 934F2DBF-2C7E-4B77-997D-17B9B0535D51
description: Utilisez cette méthode dans l’API Microsoft Store soumission pour valider une soumission d’application nouvelle ou mise à jour dans l’espace partenaires.
title: Valider une soumission d’application
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store l’API de soumission, valider l’application soumise
ms.localizationpriority: medium
ms.openlocfilehash: 83d9527b93e60af144b8c38d98a4f2e4af7852d9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171663"
---
# <a name="commit-an-app-submission"></a>Valider une soumission d’application

Utilisez cette méthode dans l’API Microsoft Store soumission pour valider une soumission d’application nouvelle ou mise à jour dans l’espace partenaires. L’action de validation alerte le Centre des partenaires que les données d’envoi ont été téléchargées (y compris les packages et les images associés). En réponse, l’espace partenaires valide les modifications apportées aux données d’envoi pour l’ingestion et la publication. Une fois l’opération de validation réussie, les modifications apportées à l’envoi sont affichées dans l’espace partenaires.

Pour plus d’informations sur la façon dont l’opération de validation s’intègre au processus d’envoi d’une application à l’aide de l’API de soumission Microsoft Store, consultez [gérer les envois d’applications](manage-app-submissions.md).

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) pour l’API de soumission Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* [Créez une soumission d’application](create-an-app-submission.md), puis [mettez à jour cette soumission](update-an-app-submission.md) avec les éventuelles modifications nécessaires apportées aux données de soumission.

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de demande                                                      |
|--------|------------------------------------------------------------------|
| POST    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la demande

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Obligatoire. L’ID Windows Store de l’application qui contient la soumission à valider. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](../publish/view-app-identity-details.md).  |
| submissionId | string | Obligatoire. ID de la soumission à valider. Cet ID est disponible dans les données de réponse pour les demandes de [création d’une soumission d’application](create-an-app-submission.md). Pour une soumission qui a été créée dans l’espace partenaires, cet ID est également disponible dans l’URL de la page de soumission dans l’espace partenaires.  |

### <a name="request-body"></a>Corps de la demande

Ne fournissez pas de corps de requête pour cette méthode.

### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment valider une soumission de requête.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621243610/commit HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>response

L’exemple suivant illustre le corps de réponse JSON d’un appel réussi à cette méthode. Pour plus d’informations sur les valeurs figurant dans le corps de réponse, voir les sections suivantes.

```json
{
  "status": "CommitStarted"
}
```

### <a name="response-body"></a>Response body

| Valeur      | Type   | Description                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| status           | string  | État de la soumission. Il peut s’agir de l’une des valeurs suivantes : <ul><li>Aucune</li><li>Opération annulée</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publication</li><li>Publié</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Libérer</li><li>ReleaseFailed</li></ul>  |

## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Code d'erreur |  Description   |
|--------|------------------|
| 400  | Les paramètres de la requête ne sont pas valides. |
| 404  | La soumission spécifiée est introuvable. |
| 409  | L’envoi spécifié a été trouvé, mais il n’a pas pu être validé dans son état actuel, ou l’application utilise une fonctionnalité d’espace partenaires qui n’est [actuellement pas prise en charge par l’API de soumission Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |

## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obtenir une soumission d’application](get-an-app-submission.md)
* [Créer une soumission d’applications](create-an-app-submission.md)
* [Mettre à jour une soumission d’application](update-an-app-submission.md)
* [Supprimer une soumission d’application](delete-an-app-submission.md)
* [Obtenir l’état d’une soumission d’application](get-status-for-an-app-submission.md)