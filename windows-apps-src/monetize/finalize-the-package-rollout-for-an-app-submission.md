---
description: Utilisez cette méthode dans l’API de soumission Microsoft Store pour finaliser le déploiement de package pour une soumission d’application.
title: Finaliser le lancement d’une soumission d’application
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store API de soumission, déploiement de packages, envoi d’applications, finaliser
ms.assetid: c7dd39e6-5162-455a-b03b-1ed76bffcf6e
ms.localizationpriority: medium
ms.openlocfilehash: 65c3d976607d260cca38e8ee09520d6a60e758a4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171513"
---
# <a name="finalize-the-rollout-for-an-app-submission"></a>Finaliser le lancement d’une soumission d’application


Utilisez cette méthode dans l’API de soumission Microsoft Store pour [finaliser le déploiement de package](../publish/gradual-package-rollout.md#completing-the-rollout) pour une soumission d’application. Pour plus d’informations sur le processus de création d’une soumission d’application à l’aide de l’API Microsoft Store soumission, consultez [gérer les envois d’applications](manage-app-submissions.md).

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) pour l’API de soumission Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Créez une soumission pour une application dans votre compte espace partenaires. Vous pouvez effectuer cette opération dans l’espace partenaires, ou vous pouvez le faire à l’aide de la méthode [créer une soumission d’application](create-an-app-submission.md) .
* Autorisez un lancement de package progressif pour la soumission. Vous pouvez effectuer cette opération [dans l’espace partenaires](../publish/gradual-package-rollout.md), ou vous pouvez le faire à l' [aide de l’API Microsoft Store soumission](manage-app-submissions.md#manage-gradual-package-rollout).

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et des paramètres de la requête.

| Méthode | URI de demande                                                      |
|--------|------------------------------------------------------------------|
| POST   | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/finalizepackagerollout` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la demande

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Obligatoire. ID Windows Store de l’application qui contient la soumission avec le lancementdu package que vous voulez finaliser. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](../publish/view-app-identity-details.md).  |
| submissionId | string | Obligatoire. ID de la soumission avec le lancement du package que vous voulez finaliser. Cet ID est disponible dans les données de réponse pour les demandes de [création d’une soumission d’application](create-an-app-submission.md). Pour une soumission qui a été créée dans l’espace partenaires, cet ID est également disponible dans l’URL de la page de soumission dans l’espace partenaires.  |


### <a name="request-body"></a>Corps de la demande

Ne fournissez pas de corps de requête pour cette méthode.

### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment finaliser le lancement du package d’une soumission de version d’évaluation de package.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621243680/finalizepackagerollout HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>response

L’exemple suivant illustre le corps de réponse JSON d’un appel réussi à cette méthode. Pour plus d’informations sur les valeurs figurant dans le corps de la réponse, voir la [ressource de lancement du package](manage-app-submissions.md#package-rollout-object).

```json
{
    "isPackageRollout": true,
    "packageRolloutPercentage": 100.0,
    "packageRolloutStatus": "PackageRolloutComplete",
    "fallbackSubmissionId": "1212922684621243058"
}
```


## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Code d'erreur |  Description   |
|--------|------------------|
| 404  | La soumission est introuvable. |
| 409  | Ce code indique l’une des erreurs suivantes :<br/><br/><ul><li>La soumission n’est pas dans un état valide pour l’opération de lancement progressif (avant d’appeler cette méthode, la soumission doit être publiée et la valeur [packageRolloutStatus](manage-app-submissions.md#package-rollout-object) doit être définie sur **PackageRolloutInProgress**).</li><li>La soumission n’appartient pas à l’application spécifiée.</li><li>L’application utilise une fonctionnalité du centre DevPartner qui n’est [actuellement pas prise en charge par l’API de soumission Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).</li></ul> |   


## <a name="related-topics"></a>Rubriques connexes

* [Lancement de package progressif](../publish/gradual-package-rollout.md)
* [Gérer les envois d’application à l’aide de l’API de soumission Microsoft Store](manage-app-submissions.md)
* [Créer et gérer des envois à l’aide des services Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)