---
ms.assetid: 2A454057-FF14-40D2-8ED2-CEB5F27E0226
description: Utilisez ces méthodes dans l’API de soumission Microsoft Store pour gérer les soumissions de vol de packages pour les applications qui sont inscrites auprès de votre compte espace partenaires.
title: Gérer les soumissions de versions d’évaluation de package
ms.date: 04/16/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store API de soumission, soumissions de vol
ms.localizationpriority: medium
ms.openlocfilehash: a5c8a8c83420830c5ec20c9586c46d54a02a5238
ms.sourcegitcommit: 7e8dfd83b181fe720b4074cb42adc908e1ba5e44
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98811272"
---
# <a name="manage-package-flight-submissions"></a>Gérer les soumissions de versions d’évaluation de package

L’API de soumission Microsoft Store fournit des méthodes que vous pouvez utiliser pour gérer les soumissions de vol de packages pour vos applications, y compris les déploiements de packages progressifs. Pour obtenir une présentation de l’API de soumission Microsoft Store, y compris les conditions préalables à l’utilisation de l’API, consultez [créer et gérer des soumissions à l’aide des services Microsoft Store](create-and-manage-submissions-using-windows-store-services.md).

> [!IMPORTANT]
> Si vous utilisez l’API de soumission Microsoft Store pour créer une soumission pour un vol de package, assurez-vous d’apporter d’autres modifications à la soumission uniquement à l’aide de l’API, plutôt que de l’espace partenaires. Si vous utilisez le tableau de bord pour modifier une soumission créée à l’origine à l’aide de l’API, vous ne pourrez plus modifier ou valider cette soumission à l’aide de l’API. Dans certains cas, la soumission peut être laissée dans un état d’erreur où elle ne peut pas continuer dans le processus de soumission. Si cela se produit, vous devez supprimer l’envoi et créer une nouvelle soumission.

<span id="methods-for-package-flight-submissions" />

## <a name="methods-for-managing-package-flight-submissions"></a>Méthodes de gestion des soumissions de versions d’évaluation de package

Pour obtenir, créer, mettre à jour, valider ou supprimer une soumission de version d’évaluation du package, utilisez les méthodes ci-dessous. Avant de pouvoir utiliser ces méthodes, le vol de package doit déjà exister dans l’espace partenaires. Vous pouvez créer un vol [de package dans l’espace partenaires](../publish/package-flights.md) ou à l’aide des méthodes de l’API de soumission Microsoft Store décrites dans [gérer les vols de packages](manage-flights.md).

<table>
<colgroup>
<col width="10%" />
<col width="30%" />
<col width="60%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Méthode</th>
<th align="left">URI</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="get-a-flight-submission.md">Obtient une soumission existante de version d’évaluation du package</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status</td>
<td align="left"><a href="get-status-for-a-flight-submission.md">Obtient l’état d’une soumission existante de version d’évaluation du package</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions</td>
<td align="left"><a href="create-a-flight-submission.md">Crée une soumission de version d’évaluation du package</a></td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="update-a-flight-submission.md">Met à jour une soumission existante de version d’évaluation du package</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit</td>
<td align="left"><a href="commit-a-flight-submission.md">Valide une soumission nouvelle ou mise à jour de version d’évaluation du package</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="delete-a-flight-submission.md">Supprime une soumission de version d’évaluation du package</a></td>
</tr>
</tbody>
</table>

<span id="create-a-package-flight-submission">

## <a name="create-a-package-flight-submission"></a>Créer une soumission de version d’évaluation du package

Pour créer une soumission pour une version d’évaluation de package, procédez comme suit.

1. Si vous ne l’avez pas encore fait, complétez les conditions préalables décrites dans [créer et gérer des envois à l’aide de Microsoft Store services](create-and-manage-submissions-using-windows-store-services.md), notamment associer une application Azure ad à votre compte espace partenaires et obtenir votre ID client et votre clé. Vous n’aurez à le faire qu’une seule fois. Une fois à votre disposition, l’ID client et la clé sont réutilisables chaque fois que vous avez besoin de créer un jeton d’accès Azure AD.  

2. [Obtenez un jeton d’accès Azure ad](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Vous devez transmettre ce jeton d’accès aux méthodes de l’API de soumission Microsoft Store. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

3. [Créez une soumission de vol de package](create-a-flight-submission.md) en exécutant la méthode suivante dans le Microsoft Store API de soumission. Cette méthode crée une soumission en cours, qui est une copie de votre dernière soumission publiée.

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions
    ```

    Le corps de la réponse contient une ressource de [soumission de vol](#flight-submission-object) qui inclut l’ID de la nouvelle soumission, l’URI de signature d’accès partagé (SAP) pour le chargement des packages pour l’envoi vers le stockage d’objets BLOB Azure et les données pour la nouvelle soumission (y compris toutes les annonces et les informations de tarification).

    > [!NOTE]
    > Un URI SAP permet d’accéder à une ressource sécurisée dans le stockage Azure sans avoir besoin de clés de compte. Pour obtenir des informations générales sur les URI SAS et leur utilisation avec le stockage d’objets BLOB Azure, consultez [signatures d’accès partagé, partie 1 : présentation du modèle SAS](/azure/storage/common/storage-sas-overview) et [signatures d’accès partagé, partie 2 : créer et utiliser une signature d’accès partagé avec un stockage d’objets BLOB](/azure/storage/common/storage-sas-overview).

4. Si vous ajoutez de nouveaux packages pour la soumission, [préparez-les](../publish/app-package-requirements.md) et ajoutez-les à une archive ZIP.

5. Révisez les données d' [envoi de vol](#flight-submission-object) avec les modifications requises pour la nouvelle soumission, puis exécutez la méthode suivante pour [mettre à jour la soumission de vol de package](update-a-flight-submission.md).

    ```json
    PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}
    ```
      > [!NOTE]
      > Si vous ajoutez de nouveaux packages pour la soumission, veillez à mettre à jour les données d’envoi pour faire référence au nom et au chemin d’accès relatif de ces fichiers dans l’archive ZIP.

4. Si vous ajoutez de nouveaux packages pour la soumission, téléchargez l’archive ZIP dans le [stockage d’objets BLOB Azure](/azure/storage/storage-introduction#blob-storage) à l’aide de l’URI SAS fourni dans le corps de la réponse de la méthode post que vous avez appelée précédemment. Vous pouvez utiliser différentes bibliothèques Azure pour effectuer cette opération sur de nombreuses plateformes, notamment :

    * [Bibliothèque cliente Azure Storage pour .NET](/azure/storage/storage-dotnet-how-to-use-blobs)
    * [Kit de développement logiciel (SDK) Azure Storage pour Java](/azure/storage/storage-java-how-to-use-blob-storage)
    * [Kit de développement logiciel (SDK) Azure Storage pour Python](/azure/storage/storage-python-how-to-use-blob-storage)

    L’exemple de code C# suivant montre comment télécharger une archive ZIP dans le stockage d’objets BLOB Azure à l’aide de la classe [CloudBlockBlob](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob) de la bibliothèque cliente Azure Storage pour .net. Cet exemple repose sur le principe que l’archive ZIP a déjà été écrite dans un objet de flux.

    ```csharp
    string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";
    Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
        new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
    await blockBob.UploadFromStreamAsync(stream);
    ```

5. [Validez la soumission de la version d’évaluation du package](commit-a-flight-submission.md) en exécutant la méthode suivante. Cela indiquera à l’espace partenaires que vous avez terminé avec votre envoi et que vos mises à jour doivent maintenant être appliquées à votre compte.

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit
    ```

6. Vérifiez l’état de la validation en exécutant la méthode suivante pour [récupérer le statut de la soumission de la version d’évaluation du package](get-status-for-a-flight-submission.md).

    ```json
    GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status
    ```

    Pour vérifier l’état de la soumission, examinez la valeur *status* dans le corps de la réponse. Cette valeur doit passer de **CommitStarted** à **PreProcessing** si la requête aboutit ou à **CommitFailed** si elle contient des erreurs. S’il existe des erreurs, le champ *statusDetails* contient d’autres détails s’y rapportant.

7. Une fois la validation correctement terminée, la soumission est envoyée au Windows Store en vue de son intégration. Vous pouvez continuer à surveiller la progression de l’envoi à l’aide de la méthode précédente ou en visitant l’espace partenaires.

<span/>

## <a name="code-examples"></a>Exemples de code

Les articles suivants fournissent des exemples de code détaillés qui montrent comment créer une soumission de version d’évaluation du package dans différents langages de programmation :

* [Exemples de code C#](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Exemples de code Java](java-code-examples-for-the-windows-store-submission-api.md)
* [Exemples de code Python](python-code-examples-for-the-windows-store-submission-api.md)

## <a name="storebroker-powershell-module"></a>Module PowerShell StoreBroker

Au lieu d’appeler directement l’API de soumission Microsoft Store, nous fournissons également un module PowerShell Open source qui implémente une interface de ligne de commande en plus de l’API. Ce module est appelé [StoreBroker](https://github.com/Microsoft/StoreBroker). Vous pouvez utiliser ce module pour gérer vos envois d’application, de vol et de module complémentaire à partir de la ligne de commande au lieu d’appeler directement l’API de soumission Microsoft Store, ou vous pouvez simplement parcourir la source pour voir d’autres exemples sur l’appel de cette API. Le module StoreBroker est utilisé activement au sein de Microsoft comme méthode principale que de nombreuses applications de première partie sont soumises au magasin.

Pour plus d’informations, consultez notre [page StoreBroker sur GitHub](https://github.com/Microsoft/StoreBroker).

<span id="manage-gradual-package-rollout">

## <a name="manage-a-gradual-package-rollout-for-a-package-flight-submission"></a>Gérer un lancement de package progressif pour une soumission de version d’évaluation de package

Vous pouvez publier progressivement les packages mis à jour d’une soumission de version d’évaluation de package pour un pourcentage des clients de votre application sur Windows 10. Cela vous permet de surveiller les commentaires et les données d’analyse des packages spécifiques et de vérifier l’adéquation de votre mise à jour avant de la déployer plus largement. Vous pouvez modifier le pourcentage de lancement (ou arrêter la mise à jour) d’une soumission publiée sans avoir à créer une nouvelle soumission. Pour plus d’informations, notamment des instructions sur l’activation et la gestion d’un déploiement de package graduel dans l’espace partenaires, consultez [cet article](../publish/gradual-package-rollout.md).

Pour activer par programmation un déploiement de package graduel pour une soumission de vol de packages, suivez ce processus à l’aide des méthodes de l’API Microsoft Store soumission :

  1. [Créez une soumission de version d’évaluation du package](create-a-flight-submission.md) ou [récupérez une soumission de version d’évaluation du package](get-a-flight-submission.md).
  2. Dans les données de réponse, localisez la ressource [packageRollout](#package-rollout-object), définissez le champ *isPackageRollout* sur true, puis définissez le champ *packageRolloutPercentage* sur le pourcentage des clients de votre application qui doivent obtenir les packages mis à jour.
  3. Communiquez les données de soumission de la version d’évaluation du package à la méthode [Mettre à jour une soumission de version d’évaluation de package](update-a-flight-submission.md).

Après avoir activé un lancement de packages progressif pour une soumission de version d’évaluation du package, vous pouvez utiliser les méthodes suivantes pour obtenir, mettre à jour, arrêter ou finaliser le lancement progressif par programmation.

<table>
<colgroup>
<col width="10%" />
<col width="30%" />
<col width="60%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Méthode</th>
<th align="left">URI</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/packagerollout</td>
<td align="left"><a href="get-package-rollout-info-for-a-flight-submission.md">Obtient les informations sur le lancement progressif d’une soumission de version d’évaluation du package</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/updatepackagerolloutpercentage</td>
<td align="left"><a href="update-the-package-rollout-percentage-for-a-flight-submission.md">Met à jour le pourcentage de lancement progressif d’une soumission de version d’évaluation du package</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/haltpackagerollout</td>
<td align="left"><a href="halt-the-package-rollout-for-a-flight-submission.md">Arrête le lancement progressif d’une soumission de version d’évaluation du package</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/finalizepackagerollout</td>
<td align="left"><a href="finalize-the-package-rollout-for-a-flight-submission.md">Finalise le lancement progressif d’une soumission de version d’évaluation du package</a></td>
</tr>
</tbody>
</table>

<span/>

## <a name="data-resources"></a>Ressources de données

Les méthodes d’API de soumission Microsoft Store pour la gestion des soumissions de vol de packages utilisent les ressources de données JSON suivantes.

<span id="flight-submission-object" />

### <a name="flight-submission-resource"></a>Ressource de soumission de version d’évaluation

Cette ressource décrit une soumission de version d’évaluation du package.

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

Cette ressource a les valeurs suivantes.

| Valeur      | Type   | Description              |
|------------|--------|------------------------------|
| id            | string  | ID de la soumission.  |
| flightId           | string  |  ID de la version d’évaluation du package auquel la soumission est associée.  |  
| status           | string  | État de la soumission. Il peut s’agir de l’une des valeurs suivantes : <ul><li>Aucun</li><li>Opération annulée</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publication</li><li>Publié</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Libérer</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | objet  |  [Ressource des détails d’état](#status-details-object) qui contient des détails supplémentaires sur l’état de la soumission, notamment des informations sur les éventuelles erreurs.  |
| flightPackages           | tableau  | Contient des [ressources de package de version d’évaluation](#flight-package-object) qui fournissent des détails sur chaque package de la soumission.   |
| packageDeliveryOptions    | objet  | [Ressource des options de remise du package](#package-delivery-options-object) qui contient les paramètres de lancement de packages progressif et de mise à jour obligatoire de la soumission.   |
| fileUploadUrl           | string  | URI de la signature d’accès partagé (SAS) pour le chargement des packages de la soumission. Si vous ajoutez de nouveaux packages à la soumission, chargez l’archive ZIP contenant les packages vers cet URI. Pour plus d’informations, voir [Créer une soumission de version d’évaluation de package](#create-a-package-flight-submission).  |
| targetPublishMode           | string  | Mode de publication pour la soumission. Il peut s’agir de l’une des valeurs suivantes : <ul><li>Immédiat</li><li>Manuel</li><li>SpecificDate</li></ul> |
| targetPublishDate           | string  | Date de publication de la soumission au format ISO 8601, si le paramètre *targetPublishMode* a la valeur SpecificDate.  |
| notesForCertification           | string  |  Fournit des informations supplémentaires aux testeurs de certification, telles que les informations d’identification du compte de test et les étapes permettant d’accéder aux fonctionnalités et de les vérifier. Pour plus d’informations, voir [Notes de certification](../publish/notes-for-certification.md). |

<span id="status-details-object" />

### <a name="status-details-resource"></a>Ressource des détails d’état

Cette ressource contient des détails supplémentaires sur l’état d’une soumission. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description                   |
|-----------------|---------|------|
|  erreurs               |    objet     |   Tableau des [ressources des détails d’état](#status-detail-object) qui contiennent les détails d’erreur de la soumission.   |     
|  warnings               |   objet      | Tableau des [ressources des détails d’état](#status-detail-object) qui contiennent les détails d’avertissement de la soumission.     |
|  certificationReports               |     objet    |   Tableau des [ressources de rapport de certification](#certification-report-object) qui donnent accès aux données du rapport de certification de la soumission. Vous pouvez examiner ces rapports pour obtenir plus d’informations en cas d’échec de la certification.    |  


<span id="status-detail-object" />

### <a name="status-detail-resource"></a>Ressource des détails d’état

Cette ressource contient des informations supplémentaires sur les éventuels erreurs ou avertissements pour une soumission. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description       |
|-----------------|---------|------|
|  code               |    string     |   [Code d’état de soumission](#submission-status-code) qui décrit le type d’erreur ou d’avertissement. |  
|  details               |     string    |  Message contenant des détails sur le problème.     |


<span id="certification-report-object" />

### <a name="certification-report-resource"></a>Ressource de rapport de certification

Cette ressource donne accès aux données du rapport de certification d’une soumission. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description         |
|-----------------|---------|------|
|     Date            |    string     |  Date et heure de génération du rapport, au format ISO 8601.    |
|     reportUrl            |    string     |  URL vous permettant d’accéder au rapport.    |


<span id="flight-package-object" />

### <a name="flight-package-resource"></a>Ressource de package de version d’évaluation

Cette ressource fournit des détails sur un package d’une soumission.

```json
{
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
}
```

Cette ressource a les valeurs suivantes.

> [!NOTE]
> Lors de l’appel de la méthode de [mise à jour d’un vol de package](update-a-flight-submission.md) , seules les valeurs *filename*, *fileStatus*, *minimumDirectXVersion* et *minimumSystemRam* de cet objet sont requises dans le corps de la demande. Les autres valeurs sont remplies par l’espace partenaires.

| Valeur           | Type    | Description              |
|-----------------|---------|------|
| fileName   |   string      |  Nom du package.    |  
| fileStatus    | string    |  État du package. Il peut s’agir de l’une des valeurs suivantes : <ul><li>Aucun</li><li>PendingUpload</li><li>Téléchargé</li><li>PendingDelete</li></ul>    |  
| id    |  string   |  ID qui identifie de manière unique le package. Cette valeur est utilisée par l’espace partenaires.   |     
| version    |  string   |  Version du package d’application. Pour plus d’informations, voir [Numérotation des versions de packages](../publish/package-version-numbering.md).   |   
| architecture    |  string   |  Architecture du package d’application (par exemple, ARM).   |     
| languages    | tableau    |  Tableau des codes des langues prises en charge par l’application. Pour plus d’informations, voir [Langues prises en charge](../publish/supported-languages.md).    |     
| capabilities    |  tableau   |  Tableau des fonctionnalités exigées par le package. Pour plus d’informations sur les fonctionnalités, voir [Déclarations des fonctionnalités d’application](../packaging/app-capability-declarations.md).   |     
| minimumDirectXVersion    |  string   |  Version DirectX minimale prise en charge par le package d’application. Cette valeur peut être définie uniquement pour les applications qui ciblent Windows 8.x. Elle est ignorée pour les applications qui ciblent d’autres versions. Il peut s’agir de l’une des valeurs suivantes : <ul><li>Aucun</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | string    |  Mémoire RAM minimale exigée par le package d’application. Cette valeur peut être définie uniquement pour les applications qui ciblent Windows 8.x. Elle est ignorée pour les applications qui ciblent d’autres versions. Il peut s’agir de l’une des valeurs suivantes : <ul><li>Aucun</li><li>Memory2GB</li></ul>   |    


<span id="package-delivery-options-object" />

### <a name="package-delivery-options-resource"></a>Ressource des options de remise du package

Cette ressource contient les paramètres de déploiement de package progressif et de mise à jour obligatoire de la soumission.

```json
{
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
}
```

Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description        |
|-----------------|---------|------|
| packageRollout   |   objet      |   [Ressource de lancement de packages](#package-rollout-object) qui contient les paramètres de lancement de packages progressif de la soumission.    |  
| isMandatoryUpdate    | boolean    |  Indique si vous souhaitez traiter les packages de cette soumission comme obligatoires pour l’installation automatique des mises à jour de l’application. Pour plus d’informations sur les packages obligatoires pour l’installation automatique des mises à jour de l’application, consultez [Télécharger et installer les mises à jour de package pour votre application](../packaging/self-install-package-updates.md).    |  
| mandatoryUpdateEffectiveDate    |  Date   |  Date et heure auxquelles les packages de cette soumission deviennent obligatoires, au format ISO 8601 dans le fuseau horaire UTC.   |        

<span id="package-rollout-object" />

### <a name="package-rollout-resource"></a>Ressource de lancement de packages

Contient les [paramètres de déploiement de package](#manage-gradual-package-rollout) de la soumission. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description        |
|-----------------|---------|------|
| isPackageRollout   |   boolean      |  Indique si le déploiement de package progressif est activé pour la soumission.    |  
| packageRolloutPercentage    | float    |  Pourcentage d’utilisateurs qui recevront les packages de déploiement progressif.    |  
| packageRolloutStatus    |  string   |  Une des chaînes suivantes qui indique l’état de déploiement de package progressif : <ul><li>PackageRolloutNotStarted</li><li>PackageRolloutInProgress</li><li>PackageRolloutComplete</li><li>PackageRolloutStopped</li></ul>  |  
| fallbackSubmissionId    |  string   |  ID de la soumission qui sera reçue par les clients n’obtenant pas les packages de déploiement progressif.   |          

> [!NOTE]
> Les valeurs *packageRolloutStatus* et *fallbackSubmissionId* sont attribuées par l’espace partenaires et ne sont pas destinées à être définies par le développeur. Si vous incluez ces valeurs dans un corps de demande, ces valeurs seront ignorées.

<span/>

## <a name="enums"></a>Énumérations

Ces méthodes utilisent les énumérations suivantes.

<span id="submission-status-code" />

### <a name="submission-status-code"></a>Code d’état de soumission

Les codes suivants représentent l’état d’une soumission.

| Code           |  Description      |
|-----------------|---------------|
|  None            |     Aucun code n’a été spécifié.         |     
|      InvalidArchive        |     L’archive ZIP contenant le package n’est pas valide ou a un format d’archive non reconnu.  |
| MissingFiles | L’archive ZIP ne dispose pas de tous les fichiers qui ont été répertoriés dans les données de votre soumission, ou ils se trouvent dans un emplacement incorrect dans l’archive. |
| PackageValidationFailed | La validation d’un ou de plusieurs packages de votre soumission a échoué. |
| InvalidParameterValue | Un des paramètres dans le corps de la requête n’est pas valide. |
| InvalidOperation | L’opération que vous avez tentée n’est pas valide. |
| InvalidState | L’opération que vous avez tentée n’est pas valide pour l’état actuel de la version d’évaluation du package. |
| ResourceNotFound | La version d’évaluation du package spécifiée est introuvable. |
| ServiceError | Une erreur de service interne a empêché la requête de réussir. Relancez la requête. |
| ListingOptOutWarning | Le développeur supprimé un listing d’une soumission précédente ou il n’a pas inclus d’informations de listing prises en charge par le package. |
| ListingOptInWarning  | Le développeur a ajouté un listing. |
| UpdateOnlyWarning | Le développeur essaie d’insérer quelque chose qui prend uniquement en charge la mise à jour. |
| Autre  | La soumission est dans un état non reconnu ou non affecté à une catégorie. |
| PackageValidationWarning | Le processus de validation du package a généré un avertissement. |

<span/>

## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gérer les vols de packages à l’aide de l’API de soumission Microsoft Store](manage-flights.md)
* [Obtenir une soumission de version d’évaluation du package](get-a-flight-submission.md)
* [Créer une soumission de version d’évaluation du package](create-a-flight-submission.md)
* [Mettre à jour une soumission de version d’évaluation de package](update-a-flight-submission.md)
* [Valider une soumission de version d’évaluation de package](commit-a-flight-submission.md)
* [Supprime une soumission de version d’évaluation du package](delete-a-flight-submission.md)
* [Obtenir l’état d’une soumission de version d’évaluation de package](get-status-for-a-flight-submission.md)
