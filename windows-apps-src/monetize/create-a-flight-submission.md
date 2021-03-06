---
ms.assetid: CD866083-EB7F-4389-A907-FC43DC2FCB5E
description: Utilisez cette méthode dans l’API de soumission Microsoft Store pour créer une soumission de vol de packages pour une application inscrite auprès de votre compte espace partenaires.
title: Créer une soumission de version d’évaluation du package
ms.date: 08/03/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store API de soumission, créer un envoi de vol
ms.localizationpriority: medium
ms.openlocfilehash: 5b00439656bfd4bcfaa90d976d28c4c71ab81e27
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175073"
---
# <a name="create-a-package-flight-submission"></a>Créer une soumission de version d’évaluation du package

Utilisez cette méthode dans l’API de soumission Microsoft Store pour créer une soumission pour un vol de package pour une application. Après avoir créé une soumission à l’aide de cette méthode, [mettez à jour cette soumission](update-a-flight-submission.md) pour apporter les modifications nécessaires aux données de soumission, puis [validez la soumission](commit-a-flight-submission.md) pour permettre son intégration et sa publication.

Pour plus d’informations sur la façon dont cette méthode s’intègre au processus de création d’une soumission de vol de packages à l’aide de l’API de soumission Microsoft Store, consultez [gérer les envois de vols de packages](manage-flight-submissions.md).

> [!NOTE]
> Cette méthode crée une soumission pour un vol de package existant. Pour créer une version d’évaluation de package, utilisez la méthode [Créer une version d’évaluation de package](create-a-flight.md).

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) pour l’API de soumission Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Créer un vol de package pour une application. Vous pouvez effectuer cette opération dans l’espace partenaires, ou vous pouvez le faire à l’aide de la méthode [créer un package vol](create-a-flight.md) .

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de demande                                                      |
|--------|------------------------------------------------------------------|
| POST    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la demande

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Obligatoire. ID Windows Store de l’application pour laquelle vous voulez créer une soumission de version d’évaluation de package. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](../publish/view-app-identity-details.md).  |
| flightId | string | Obligatoire. ID de la version d’évaluation de package pour laquelle vous voulez ajouter la soumission. Cet ID est disponible dans les données de réponse pour les demandes de [création d’un vol de packages](create-a-flight.md) et l' [extraction des vols de packages pour une application](get-flights-for-an-app.md).  |


### <a name="request-body"></a>Corps de la demande

Ne fournissez pas de corps de requête pour cette méthode.

### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment créer une soumission de version d’évaluation de package pour une application ayant l’ID Windows Store 9WZDNCRD91MD.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>response

L’exemple suivant illustre le corps de réponse JSON d’un appel réussi à cette méthode. Le corps de la réponse contient des informations sur la nouvelle soumission. Pour plus d’informations sur les valeurs figurant dans le corps de la réponse, voir la [ressource de soumission de version d’évaluation du package](manage-flight-submissions.md#flight-submission-object).

```json
{
  "id": "1152921504621243649",
  "flightId": "cd2e368a-0da5-4026-9f34-0e7934bc6f23",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
      "fileStatus": "PendingUpload",
      "id": "",
      "version": "1.0.0.0",
      "languages": ["en-us"],
      "capabilities": [],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
    }
  ],
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0.0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/8b389577-5d5e-4cbe-a744-1ff2e97a9eb8?sv=2014-02-14&sr=b&sig=wgMCQPjPDkuuxNLkeG35rfHaMToebCxBNMPw7WABdXU%3D&se=2016-06-17T21:29:44Z&sp=rwl",
  "targetPublishMode": "Immediate",
  "targetPublishDate": "",
  "notesForCertification": "No special steps are required for certification of this app."
}
```

## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Code d'erreur |  Description   |
|--------|------------------|
| 400  | Impossible de créer la soumission de version d’évaluation de package, car la requête n’est pas valide. |
| 409  | Impossible de créer l’envoi de vols de packages en raison de l’état actuel de l’application, ou l’application utilise une fonctionnalité d’espace partenaires qui n’est [actuellement pas prise en charge par l’API de soumission Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   

## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gérer les soumissions de versions d’évaluation de package](manage-flight-submissions.md)
* [Obtenir une soumission de version d’évaluation du package](get-a-flight-submission.md)
* [Valider une soumission de version d’évaluation de package](commit-a-flight-submission.md)
* [Mettre à jour une soumission de version d’évaluation de package](update-a-flight-submission.md)
* [Supprime une soumission de version d’évaluation du package](delete-a-flight-submission.md)
* [Obtenir l’état d’une soumission de version d’évaluation de package](get-status-for-a-flight-submission.md)