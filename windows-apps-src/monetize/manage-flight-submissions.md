---
ms.assetid: 2A454057-FF14-40D2-8ED2-CEB5F27E0226
description: Utilisez ces méthodes dans l’API de soumission Microsoft Store pour gérer les soumissions de vol de packages pour les applications qui sont inscrites auprès de votre compte espace partenaires.
title: Gérer les envois de vols de packages
ms.date: 04/16/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store API de soumission, soumissions de vol
ms.localizationpriority: medium
ms.openlocfilehash: 50596fdadae2ac4a0625687e7c8acaf985ccfaa7
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690375"
---
# <a name="manage-package-flight-submissions"></a>Gérer les envois de vols de packages

L’API de soumission Microsoft Store fournit des méthodes que vous pouvez utiliser pour gérer les soumissions de vol de packages pour vos applications, y compris les déploiements de packages progressifs. Pour obtenir une présentation de l’API de soumission Microsoft Store, y compris les conditions préalables à l’utilisation de l’API, consultez [créer et gérer des soumissions à l’aide des services Microsoft Store](create-and-manage-submissions-using-windows-store-services.md).

> [!IMPORTANT]
> Si vous utilisez l’API de soumission Microsoft Store pour créer une soumission pour un vol de package, assurez-vous d’apporter d’autres modifications à la soumission uniquement à l’aide de l’API, plutôt que de l’espace partenaires. Si vous utilisez le tableau de bord pour modifier une soumission créée à l’origine à l’aide de l’API, vous ne pourrez plus modifier ou valider cette soumission à l’aide de l’API. Dans certains cas, la soumission peut être laissée dans un état d’erreur où elle ne peut pas continuer dans le processus de soumission. Si cela se produit, vous devez supprimer l’envoi et créer une nouvelle soumission.

<span id="methods-for-package-flight-submissions" />

## <a name="methods-for-managing-package-flight-submissions"></a>Méthodes de gestion des soumissions de vol de packages

Utilisez les méthodes suivantes pour obtenir, créer, mettre à jour, valider ou supprimer une soumission de vol de package. Avant de pouvoir utiliser ces méthodes, le vol de package doit déjà exister dans l’espace partenaires. Vous pouvez créer un vol [de package dans l’espace partenaires](https://docs.microsoft.com/windows/uwp/publish/package-flights) ou à l’aide des méthodes de l’API de soumission Microsoft Store décrites dans [gérer les vols de packages](manage-flights.md).

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
<td align="left"><a href="get-a-flight-submission.md">Recevoir une soumission de vol de packages existante</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status</td>
<td align="left"><a href="get-status-for-a-flight-submission.md">Recevoir l’état d’une soumission de vol de packages existante</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions</td>
<td align="left"><a href="create-a-flight-submission.md">Créer une soumission de vol de packages</a></td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="update-a-flight-submission.md">Mettre à jour une soumission de vol de packages existante</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit</td>
<td align="left"><a href="commit-a-flight-submission.md">Valider une soumission de vol de packages nouvelle ou mise à jour</a></td>
</tr>
<tr>
<td align="left">SUPPRIMER</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="delete-a-flight-submission.md">Supprimer une soumission de vol de packages</a></td>
</tr>
</tbody>
</table>

<span id="create-a-package-flight-submission">

## <a name="create-a-package-flight-submission"></a>Créer une soumission de vol de packages

Pour créer une soumission pour un vol de package, suivez ce processus.

1. Si vous ne l’avez pas encore fait, complétez les conditions préalables décrites dans [créer et gérer des envois à l’aide de Microsoft Store services](create-and-manage-submissions-using-windows-store-services.md), notamment associer une application Azure ad à votre compte espace partenaires et obtenir votre ID client et votre clé. Vous ne devez effectuer cette opération qu’une seule fois. une fois que vous avez l’ID client et la clé, vous pouvez les réutiliser chaque fois que vous avez besoin de créer un nouveau jeton d’accès Azure AD.  

2. [Obtenez un jeton d’accès Azure ad](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Vous devez transmettre ce jeton d’accès aux méthodes de l’API de soumission Microsoft Store. Une fois que vous avez obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant son expiration. Une fois le jeton expiré, vous pouvez en obtenir un nouveau.

3. [Créez une soumission de vol de package](create-a-flight-submission.md) en exécutant la méthode suivante dans le Microsoft Store API de soumission. Cette méthode crée une nouvelle soumission en cours, qui est une copie de votre dernière soumission publiée.

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions
    ```

    Le corps de la réponse contient une ressource de [soumission de vol](#flight-submission-object) qui inclut l’ID de la nouvelle soumission, l’URI de signature d’accès partagé (SAP) pour le chargement des packages pour l’envoi vers le stockage d’objets BLOB Azure, et les données pour la nouvelle soumission (y compris l’ensemble annonces et informations de tarification).

    > [!NOTE]
    > Un URI SAP permet d’accéder à une ressource sécurisée dans le stockage Azure sans avoir besoin de clés de compte. Pour obtenir des informations générales sur les URI SAS et leur utilisation avec le stockage d’objets BLOB Azure, consultez [signatures d’accès partagé, partie 1 : présentation du modèle SAS](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/) et [signatures d’accès partagé, partie 2 : créer et utiliser une signature d’accès partagé avec un stockage d’objets BLOB](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/).

4. Si vous ajoutez de nouveaux packages pour la soumission, [Préparez les packages](https://docs.microsoft.com/windows/uwp/publish/app-package-requirements) et ajoutez-les à une archive zip.

5. Révisez les données d' [envoi de vol](#flight-submission-object) avec les modifications requises pour la nouvelle soumission, puis exécutez la méthode suivante pour [mettre à jour la soumission de vol de package](update-a-flight-submission.md).

    ```json
    PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}
    ```
      > [!NOTE]
      > Si vous ajoutez de nouveaux packages pour la soumission, veillez à mettre à jour les données d’envoi pour faire référence au nom et au chemin d’accès relatif de ces fichiers dans l’archive ZIP.

4. Si vous ajoutez de nouveaux packages pour la soumission, téléchargez l’archive ZIP dans le [stockage d’objets BLOB Azure](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage) à l’aide de l’URI SAS fourni dans le corps de la réponse de la méthode post que vous avez appelée précédemment. Il existe différentes bibliothèques Azure que vous pouvez utiliser pour effectuer cette opération sur diverses plateformes, notamment :

    * [Bibliothèque cliente Azure Storage pour .NET](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-blobs)
    * [Kit de développement logiciel (SDK) Azure Storage pour Java](https://docs.microsoft.com/azure/storage/storage-java-how-to-use-blob-storage)
    * [Kit de développement logiciel (SDK) Stockage Azure pour Python](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-blob-storage)

    L’exemple C# de code suivant montre comment télécharger une archive zip dans le stockage d’objets BLOB Azure à l’aide de la classe [CloudBlockBlob](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob) dans la bibliothèque cliente Azure Storage pour .net. Cet exemple suppose que l’archive ZIP a déjà été écrite dans un objet de flux.

    ```csharp
    string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";
    Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
        new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
    await blockBob.UploadFromStreamAsync(stream);
    ```

5. [Validez la soumission de vol de package](commit-a-flight-submission.md) en exécutant la méthode suivante. Cela indiquera à l’espace partenaires que vous avez terminé avec votre envoi et que vos mises à jour doivent maintenant être appliquées à votre compte.

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit
    ```

6. Vérifiez l’état de validation en exécutant la méthode suivante pour [connaître l’état de l’envoi du vol de package](get-status-for-a-flight-submission.md).

    ```json
    GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status
    ```

    Pour confirmer l’état de l’envoi, passez en revue la valeur d' *État* dans le corps de la réponse. Cette valeur doit passer de **CommitStarted** à **prétraitement** si la demande est réussie ou à **CommitFailed** en cas d’erreurs dans la demande. En cas d’erreur, le champ *statusDetails* contient des détails supplémentaires sur l’erreur.

7. Une fois la validation terminée, l’envoi est envoyé au magasin pour réception. Vous pouvez continuer à surveiller la progression de l’envoi à l’aide de la méthode précédente ou en visitant l’espace partenaires.

<span/>

## <a name="code-examples"></a>Exemples de code

Les articles suivants fournissent des exemples de code détaillés qui montrent comment créer une soumission de vol de package dans différents langages de programmation :

* [C#exemples de code](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Exemples de code Java](java-code-examples-for-the-windows-store-submission-api.md)
* [Exemples de code python](python-code-examples-for-the-windows-store-submission-api.md)

## <a name="storebroker-powershell-module"></a>Module PowerShell StoreBroker

Au lieu d’appeler directement l’API de soumission Microsoft Store, nous fournissons également un module PowerShell Open source qui implémente une interface de ligne de commande en plus de l’API. Ce module est appelé [StoreBroker](https://aka.ms/storebroker). Vous pouvez utiliser ce module pour gérer vos envois d’application, de vol et de module complémentaire à partir de la ligne de commande au lieu d’appeler directement l’API de soumission Microsoft Store, ou vous pouvez simplement parcourir la source pour voir d’autres exemples sur l’appel de cette API. Le module StoreBroker est utilisé activement au sein de Microsoft comme méthode principale que de nombreuses applications de première partie sont soumises au magasin.

Pour plus d’informations, consultez notre [page StoreBroker sur GitHub](https://aka.ms/storebroker).

<span id="manage-gradual-package-rollout">

## <a name="manage-a-gradual-package-rollout-for-a-package-flight-submission"></a>Gérer un déploiement de package graduel pour une soumission de vol de packages

Vous pouvez déployer progressivement les packages mis à jour dans un envoi de vol de packages vers un pourcentage des clients de votre application sur Windows 10. Cela vous permet de surveiller les commentaires et les données analytiques pour les packages spécifiques afin de vous assurer que vous êtes sûr de la mise à jour avant de la déployer plus largement. Vous pouvez modifier le pourcentage de déploiement (ou arrêter la mise à jour) pour une soumission publiée sans avoir à créer une nouvelle soumission. Pour plus d’informations, notamment des instructions sur l’activation et la gestion d’un déploiement de package graduel dans l’espace partenaires, consultez [cet article](../publish/gradual-package-rollout.md).

Pour activer par programmation un déploiement de package graduel pour une soumission de vol de packages, suivez ce processus à l’aide des méthodes de l’API Microsoft Store soumission :

  1. [Créez une soumission de vol de package](create-a-flight-submission.md) ou [recevez une soumission de vol de package](get-a-flight-submission.md).
  2. Dans les données de réponse, localisez la ressource [packageRollout](#package-rollout-object) , affectez la valeur true au champ *isPackageRollout* , puis définissez le champ *packageRolloutPercentage* sur le pourcentage des clients de votre application qui doivent obtenir les packages mis à jour.
  3. Transmettez les données d’envoi du vol de package mises à jour à la méthode de [mise à jour d’un vol de package](update-a-flight-submission.md) .

Une fois le déploiement de package graduel activé pour une soumission de vol de packages, vous pouvez utiliser les méthodes suivantes pour obtenir, mettre à jour, arrêter ou finaliser le déploiement graduel.

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
<td align="left"><a href="get-package-rollout-info-for-a-flight-submission.md">Obtenir les informations de déploiement progressif pour une soumission de vol de packages</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/updatepackagerolloutpercentage</td>
<td align="left"><a href="update-the-package-rollout-percentage-for-a-flight-submission.md">Mettre à jour le pourcentage de déploiement graduel pour une soumission de vol de packages</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/haltpackagerollout</td>
<td align="left"><a href="halt-the-package-rollout-for-a-flight-submission.md">Arrêter le déploiement graduel pour une soumission de vol de packages</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/finalizepackagerollout</td>
<td align="left"><a href="finalize-the-package-rollout-for-a-flight-submission.md">Finaliser le déploiement graduel pour l’envoi d’un vol de packages</a></td>
</tr>
</tbody>
</table>

<span/>

## <a name="data-resources"></a>Ressources de données

Les méthodes d’API de soumission Microsoft Store pour la gestion des soumissions de vol de packages utilisent les ressources de données JSON suivantes.

<span id="flight-submission-object" />

### <a name="flight-submission-resource"></a>Ressource de soumission de vol

Cette ressource décrit une soumission de vol de packages.

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
| flightId           | string  |  ID du vol de package auquel la soumission est associée.  |  
| status           | string  | État de la soumission. Il peut s’agir de l’une des valeurs suivantes : <ul><li>Aucune</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publication</li><li>Publié</li><li>PublishFailed</li><li>Prétraitement</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Libérer</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | objet  |  [Ressource de détails d’État](#status-details-object) qui contient des détails supplémentaires sur l’état de la soumission, y compris des informations sur les erreurs.  |
| flightPackages           | array  | Contient des [ressources de package de vol](#flight-package-object) qui fournissent des détails sur chaque package de la soumission.   |
| packageDeliveryOptions    | objet  | [Ressource d’options de remise de package](#package-delivery-options-object) qui contient des paramètres de déploiement de package progressif et de mise à jour obligatoire pour la soumission.   |
| fileUploadUrl           | string  | URI de signature d’accès partagé (SAS) pour le chargement de tous les packages pour l’envoi. Si vous ajoutez de nouveaux packages pour la soumission, téléchargez l’archive ZIP qui contient les packages sur cet URI. Pour plus d’informations, consultez [créer une soumission de vol de packages](#create-a-package-flight-submission).  |
| targetPublishMode           | string  | Mode de publication pour la soumission. Il peut s’agir de l’une des valeurs suivantes : <ul><li>Immédiat</li><li>Manuel</li><li>SpecificDate</li></ul> |
| targetPublishDate           | string  | Date de publication pour l’envoi au format ISO 8601, si *targetPublishMode* est défini sur SpecificDate.  |
| notesForCertification           | string  |  Fournit des informations supplémentaires pour les testeurs de certification, tels que les informations d’identification du compte de test et les étapes d’accès et de vérification des fonctionnalités. Pour plus d’informations, consultez [Notes pour la certification](https://docs.microsoft.com/windows/uwp/publish/notes-for-certification). |

<span id="status-details-object" />

### <a name="status-details-resource"></a>Ressource des détails de l’État

Cette ressource contient des détails supplémentaires sur l’état d’une soumission. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description                   |
|-----------------|---------|------|
|  errors               |    objet     |   Tableau de [ressources de détail d’État](#status-detail-object) qui contiennent les détails de l’erreur pour la soumission.   |     
|  Ceux               |   objet      | Tableau de [ressources de détail d’État](#status-detail-object) qui contiennent des détails d’avertissement pour la soumission.     |
|  certificationReports               |     objet    |   Tableau de [ressources de rapport de certification](#certification-report-object) qui fournissent l’accès aux données du rapport de certification pour la soumission. Vous pouvez consulter ces rapports pour plus d’informations en cas d’échec de la certification.    |  


<span id="status-detail-object" />

### <a name="status-detail-resource"></a>Ressource de détail d’État

Cette ressource contient des informations supplémentaires sur les erreurs ou avertissements associés à une soumission. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description       |
|-----------------|---------|------|
|  code               |    string     |   [Code d’état d’envoi](#submission-status-code) qui décrit le type d’erreur ou d’avertissement. |  
|  détails               |     string    |  Un message contenant plus de détails sur le problème.     |


<span id="certification-report-object" />

### <a name="certification-report-resource"></a>Ressource du rapport de certification

Cette ressource permet d’accéder aux données du rapport de certification pour une soumission. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description         |
|-----------------|---------|------|
|     date            |    string     |  Date et heure de génération du rapport, au format ISO 8601.    |
|     reportUrl            |    string     |  URL à laquelle vous pouvez accéder au rapport.    |


<span id="flight-package-object" />

### <a name="flight-package-resource"></a>Ressource de package de vol

Cette ressource fournit des détails sur un package dans une soumission.

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
> Lors de l’appel de la méthode de [mise à jour d’un vol de package](update-a-flight-submission.md) , seules les valeurs *filename*, *fileStatus*, *minimumDirectXVersion*et *minimumSystemRam* de cet objet sont requises dans le corps de la demande. Les autres valeurs sont remplies par l’espace partenaires.

| Valeur           | Type    | Description              |
|-----------------|---------|------|
| fileName   |   string      |  Nom du package.    |  
| fileStatus    | string    |  État du package. Il peut s’agir de l’une des valeurs suivantes : <ul><li>Aucune</li><li>PendingUpload</li><li>Téléchargé</li><li>PendingDelete</li></ul>    |  
| id    |  string   |  ID qui identifie de façon unique le package. Cette valeur est utilisée par l’espace partenaires.   |     
| version    |  string   |  Version du package d’application. Pour plus d’informations, consultez la page [numérotation des versions de package](https://docs.microsoft.com/windows/uwp/publish/package-version-numbering).   |   
| architecture    |  string   |  Architecture du package d’application (par exemple, ARM).   |     
| languages    | array    |  Tableau de codes de langue pour les langues prises en charge par l’application. Pour plus d’informations, consultez pour plus d’informations, consultez [langues prises en charge](https://docs.microsoft.com/windows/uwp/publish/supported-languages).    |     
| capabilities    |  array   |  Tableau des fonctionnalités requises par le package. Pour plus d’informations sur les fonctionnalités, consultez [déclarations de fonctionnalités d’application](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations).   |     
| minimumDirectXVersion    |  string   |  Version de DirectX minimale prise en charge par le package d’application. Cela peut être défini uniquement pour les applications qui ciblent Windows 8. x ; elle est ignorée pour les applications qui ciblent d’autres versions. Il peut s’agir de l’une des valeurs suivantes : <ul><li>Aucune</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | string    |  RAM minimale requise par le package d’application. Cela peut être défini uniquement pour les applications qui ciblent Windows 8. x ; elle est ignorée pour les applications qui ciblent d’autres versions. Il peut s’agir de l’une des valeurs suivantes : <ul><li>Aucune</li><li>Memory2GB</li></ul>   |    


<span id="package-delivery-options-object" />

### <a name="package-delivery-options-resource"></a>Ressource options de remise de package

Cette ressource contient des paramètres de déploiement de package progressifs et des paramètres de mise à jour obligatoire pour la soumission.

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
| packageRollout   |   objet      |   [Ressource de déploiement de package](#package-rollout-object) qui contient des paramètres de déploiement de package progressifs pour la soumission.    |  
| isMandatoryUpdate    | booléenne    |  Indique si vous souhaitez traiter les packages dans cette soumission comme obligatoires pour l’installation automatique des mises à jour d’application. Pour plus d’informations sur les packages obligatoires pour l’installation automatique des mises à jour d’application, consultez [Télécharger et installer des mises à jour de package pour votre application](../packaging/self-install-package-updates.md).    |  
| mandatoryUpdateEffectiveDate    |  date   |  Date et heure auxquelles les packages de cette soumission deviennent obligatoires, au format ISO 8601 et au fuseau horaire UTC.   |        

<span id="package-rollout-object" />

### <a name="package-rollout-resource"></a>Ressource de déploiement de package

Cette ressource contient des [paramètres de déploiement de package](#manage-gradual-package-rollout) progressifs pour la soumission. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description        |
|-----------------|---------|------|
| isPackageRollout   |   booléenne      |  Indique si le déploiement graduel de packages est activé pour la soumission.    |  
| packageRolloutPercentage    | float    |  Pourcentage des utilisateurs qui recevront les packages dans le déploiement graduel.    |  
| packageRolloutStatus    |  string   |  L’une des chaînes suivantes qui indique l’état du déploiement de package graduel : <ul><li>PackageRolloutNotStarted</li><li>PackageRolloutInProgress</li><li>PackageRolloutComplete</li><li>PackageRolloutStopped</li></ul>  |  
| fallbackSubmissionId    |  string   |  ID de la soumission qui sera reçue par les clients qui n’obtiennent pas les packages de déploiement progressif.   |          

> [!NOTE]
> Les valeurs *packageRolloutStatus* et *fallbackSubmissionId* sont attribuées par l’espace partenaires et ne sont pas destinées à être définies par le développeur. Si vous incluez ces valeurs dans un corps de demande, ces valeurs seront ignorées.

<span/>

## <a name="enums"></a>Enums

Ces méthodes utilisent les énumérations suivantes.

<span id="submission-status-code" />

### <a name="submission-status-code"></a>Code d’état de l’envoi

Les codes suivants représentent l’état d’une soumission.

| Code           |  Description      |
|-----------------|---------------|
|  Aucune            |     Aucun code n’a été spécifié.         |     
|      InvalidArchive        |     L’archive ZIP contenant le package n’est pas valide ou son format d’archive n’est pas reconnu.  |
| MissingFiles | L’archive ZIP ne contient pas tous les fichiers qui étaient répertoriés dans vos données d’envoi, ou ils se trouvent à un emplacement incorrect dans l’archive. |
| PackageValidationFailed | Échec de la validation d’un ou plusieurs packages dans votre soumission. |
| InvalidParameterValue | L’un des paramètres dans le corps de la demande n’est pas valide. |
| InvalidOperation | L’opération que vous avez tentée n’est pas valide. |
| InvalidState | L’opération que vous avez tentée n’est pas valide pour l’état actuel du vol de package. |
| ResourceNotFound | Le vol de package spécifié est introuvable. |
| ServiceError | Une erreur de service interne a empêché la requête de s’effectuer correctement. Recommencez la demande. |
| ListingOptOutWarning | Le développeur a supprimé une liste d’une soumission précédente ou n’incluait pas d’informations de liste prises en charge par le package. |
| ListingOptInWarning  | Le développeur a ajouté une liste. |
| UpdateOnlyWarning | Le développeur tente d’insérer un nom qui ne prend en charge que les mises à jour. |
| Autres  | L’envoi est dans un État non reconnu ou non classé. |
| PackageValidationWarning | Le processus de validation du package a généré un avertissement. |

<span/>

## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gérer les vols de packages à l’aide de l’API de soumission Microsoft Store](manage-flights.md)
* [Obtenir une soumission de vol de packages](get-a-flight-submission.md)
* [Créer une soumission de vol de packages](create-a-flight-submission.md)
* [Mettre à jour une soumission de vol de packages](update-a-flight-submission.md)
* [Valider une soumission de vol de packages](commit-a-flight-submission.md)
* [Supprimer une soumission de vol de packages](delete-a-flight-submission.md)
* [Obtenir l’état d’une soumission de vol de packages](get-status-for-a-flight-submission.md)
