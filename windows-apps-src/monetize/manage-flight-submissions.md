---
author: mcleanbyron
ms.assetid: 2A454057-FF14-40D2-8ED2-CEB5F27E0226
description: "Utilisez ces méthodes dans l’API de soumission du Windows Store pour gérer les soumissions de versions d’évaluation de package pour les applications qui sont inscrites dans votre compte du Centre de développement Windows."
title: "Gérer les soumissions de versions d’évaluation de package à l’aide de l’API de soumission du Windows Store"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 18d28495b80101cf5cfe53869b0f5cd3d61b50c9

---

# Gérer les soumissions de versions d’évaluation de package à l’aide de l’API de soumission du Windows Store




Utilisez les méthodes suivantes dans l’API de soumission du Windows Store pour gérer les soumissions de versions d’évaluation de package pour les applications qui sont inscrites dans votre compte du Centre de développement Windows. Pour obtenir une présentation de l’API de soumission du Windows Store, notamment les conditions préalables à l’utilisation de l’API, voir [Créer et gérer des soumissions à l’aide des services du Windows Store](create-and-manage-submissions-using-windows-store-services.md).

>**Remarque**&nbsp;&nbsp;Ces méthodes ne peuvent être utilisées que pour les comptes du Centre de développement Windows qui ont reçu l’autorisation d’utiliser l’API de soumission du Windows Store. Tous les comptes ne bénéficient pas de cette autorisation. Avant de pouvoir utiliser ces méthodes pour créer ou gérer les soumissions de version d’évaluation de package, la version d’évaluation de package doit déjà exister dans votre compte du Centre de développement. Vous pouvez créer une version d’évaluation de package en [utilisant le tableau de bord du Centre de développement](https://msdn.microsoft.com/windows/uwp/publish/package-flights) ou en utilisant les méthodes de l’API de soumission du Windows Store décrites dans [Gérer les versions d’évaluation de package](manage-flights.md).

| Méthode        | URI    | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}``` | Obtient les données d’une soumission de version d’évaluation de package existante. Pour plus d’informations, voir [Obtenir une soumission de version d’évaluation de package](get-a-flight-submission.md). |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status``` | Obtient l’état d’une soumission de version d’évaluation de package existante. Pour plus d’informations, voir [Obtenir l’état d’une soumission de version d’évaluation de package](get-status-for-a-flight-submission.md). |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/applications/{applicationId}/flights/{flightId}/submissions``` | Crée une soumission de version d’évaluation de package pour une application inscrite dans votre compte du Centre de développement Windows. Pour plus d’informations, voir [Créer une soumission de version d’évaluation de package](create-a-flight-submission.md). |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit``` | Valide une soumission de version d’évaluation de package nouvelle ou mise à jour dans le Centre de développement Windows. Pour plus d’informations, voir [Valider une soumission de version d’évaluation de package](commit-a-flight-submission.md). |
| PUT | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}``` | Met à jour une soumission de version d’évaluation de package existante. Pour plus d’informations, voir [Mettre à jour une soumission de version d’évaluation de package](update-a-flight-submission.md). |
| DELETE | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}``` | Supprime une soumission de version d’évaluation de package. Pour plus d’informations, voir [Supprimer une soumission de version d’évaluation de package](delete-a-flight-submission.md). |

<span id="create-a-package-flight-submission">
## Créer une soumission de version d’évaluation du package

Pour créer une soumission pour une version d’évaluation de package, procédez comme suit.

1. Si vous ne l’avez pas encore fait, remplissez les conditions préalables décrites dans [Créer et gérer des soumissions à l’aide des services du Windows Store](create-and-manage-submissions-using-windows-store-services.md), notamment l’association d’une application AzureAD à votre compte du Centre de développement Windows et l’obtention de votre ID client et de votre clé. Vous n’aurez à le faire qu’une seule fois. Une fois à votre disposition, l’ID client et la clé sont réutilisables chaque fois que vous avez besoin de créer un jeton d’accès AzureAD.  

2. [Obtenir un jeton d’accès AzureAD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Vous devez transmettre ce jeton d’accès aux méthodes de l’API de soumission du Windows Store. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

3. Exécutez la méthode suivante de l’API de soumission du Windows Store. Cette méthode crée une soumission en cours, qui est une copie de votre dernière soumission publiée. Pour plus d’informations, voir [Créer une soumission de version d’évaluation de package](create-a-flight-submission.md).

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications{applicationId}/flights/{flightId}/submissions
  ```

  Le corps de la réponse contient trois éléments: l’ID de la nouvelle soumission, ses données (notamment toutes les listes et informations tarifaires), ainsi que l’URI de signatures d’accès partagé (SAS) pour le chargement de tous les packages de la soumission. Pour plus d’informations sur SAS, voir [Signatures d’accès partagé, partie1: présentation du modèle SAS](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/).

4. Si vous ajoutez de nouveaux packages pour la soumission, [préparez-les](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements) et ajoutez-les à une archive ZIP.

5. Mettez à jour les données de la soumission avec toutes les modifications nécessaires pour la nouvelle soumission, puis exécutez la méthode suivante pour mettre à jour la soumission. Pour plus d’informations, voir [Mettre à jour une soumission de version d’évaluation de package](update-a-flight-submission.md).

  ```
  PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}
  ```

  >**Remarque**&nbsp;&nbsp;Si vous ajoutez de nouveaux packages pour la soumission, veillez à mettre à jour les données de la soumission pour faire référence au nom et au chemin relatif de ces fichiers dans l’archiveZIP.

4. Si vous ajoutez de nouveaux packages pour la soumission, chargez l’archiveZIP sur l’URI SAS fourni dans le corps de la réponse de la méthode POST appelée à l’étape2. Pour plus d’informations, voir [Signatures d’accès partagé, partie 2: créer et utiliser une SAS avec le stockage d’objets blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/).

  L’extrait de code suivant montre comment charger l’archive à l’aide de la classe [CloudBlockBlob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.aspx) incluse dans la bibliothèque cliente de stockage Azure pour.NET.

  ```csharp
  string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";

  Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
      new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
  await blockBob.UploadFromStreamAsync(stream);
  ```

5. Validez la soumission en exécutant la méthode suivante. Le Centre de développement est ainsi informé que vous avez terminé votre soumission et que vos mises à jour doivent être appliqués à votre compte. Pour plus d’informations, voir [Valider une soumission de version d’évaluation de package](commit-a-flight-submission.md).

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit
  ```

6. Vérifiez l’état de validation en exécutant la méthode suivante. Pour plus d’informations, voir [Obtenir l’état d’une soumission de version d’évaluation de package](get-status-for-a-flight-submission.md).

  ```
  GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status
  ```

  Pour vérifier l’état de la soumission, examinez la valeur *status* dans le corps de la réponse. Cette valeur doit passer de **CommitStarted** à **PreProcessing** si la demande aboutit ou à **CommitFailed** la demande contient des erreurs. S’il existe des erreurs, le champ *statusDetails* contient d’autres détails s’y rapportant.

7. Une fois la validation correctement terminée, la soumission est envoyée au Windows Store en vue de son intégration. Vous pouvez continuer à surveiller la progression de la soumission à l’aide de la méthode précédente ou en consultant le tableau de bord du Centre de développement.

## Ressources

Ces méthodes utilisent les ressources de données suivantes.

<span id="flight-submission-object" />
### Soumission de version d’évaluation de package

Cette ressource représente une soumission pour une version d’évaluation de package. L’exemple suivant illustre le format de cette ressource.

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
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/8b389577-5d5e-4cbe-a744-1ff2e97a9eb8?sv=2014-02-14&sr=b&sig=wgMCQPjPDkuuxNLkeG35rfHaMToebCxBNMPw7WABdXU%3D&se=2016-06-17T21:29:44Z&sp=rwl",
  "targetPublishMode": "Immediate",
  "targetPublishDate": "",
  "notesForCertification": "No special steps are required for certification of this app."
}
```

Cette ressource a les valeurs suivantes.

| Valeur      | Type   | Description                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | chaîne  | ID de la soumission.  |
| flightId           | chaîne  |  ID de la version d’évaluation du package auquel la soumission est associée.  |  
| status           | chaîne  | État de la soumission. Les valeurs possibles sont les suivantes: <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | objet  |  Contient des détails supplémentaires sur l’état de la soumission, notamment des informations sur les éventuelles erreurs. Pour plus d’informations, voir la section [Détails d’état](#status-details-object) ci-après. |
| flightPackages           | tableau  | Contient des objets qui fournissent des détails sur chaque package de la soumission. Pour plus d’informations, voir la section [Package de version d’évaluation](#flight-package-object) ci-après.  |
| fileUploadUrl           | chaîne  | URI de la signature d’accès partagé (SAS) pour le chargement des packages de la soumission. Si vous ajoutez de nouveaux packages à la soumission, chargez l’archive ZIP contenant les packages vers cet URI. Pour plus d’informations, voir [Créer une soumission de version d’évaluation de package](#create-a-package-flight-submission).  |
| targetPublishMode           | chaîne  | Mode de publication pour la soumission. Les valeurs possibles sont les suivantes: <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | chaîne  | Date de publication de la soumission au format ISO8601, si le paramètre *targetPublishMode* a la valeur SpecificDate.  |
| notesForCertification           | chaîne  |  Fournit des informations supplémentaires aux testeurs de certification, telles que les informations d’identification du compte de test et les étapes permettant d’accéder aux fonctionnalités et de les vérifier. Pour plus d’informations, voir [Notes de certification](https://msdn.microsoft.com/windows/uwp/publish/notes-for-certification). |

<span id="status-details-object" />
### Détails d’état

Cette ressource contient des détails supplémentaires sur l’état d’une soumission. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  errors               |    objet     |   Tableau d’objets qui contiennent des détails d’erreur pour la soumission. Pour plus d’informations, voir la section [Détail d’état](#status-detail-object) ci-après.   |     
|  warnings               |   objet      | Tableau d’objets qui contiennent des détails d’avertissement pour la soumission. Pour plus d’informations, voir la section [Détail d’état](#status-detail-object) ci-après.     |
|  certificationReports               |     objet    |   Tableau d’objets qui donnent accès aux données du rapport de certification pour la soumission. Vous pouvez examiner ces rapports pour obtenir plus d’informations en cas d’échec de la certification. Pour plus d’informations, voir la section [Rapport de certification](#certification-report-object) ci-dessous.   |  


<span id="status-detail-object" />
### Détail d’état

Cette ressource contient des informations supplémentaires sur les éventuels erreurs ou avertissements pour une soumission. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  code               |    chaîne     |   Chaîne qui décrit le type d’erreur ou d’avertissement. Pour plus d’informations, voir la section [Code d’état de soumission](#submission-status-code) ci-dessous.   |     
|  details               |     chaîne    |  Message contenant des détails sur le problème.     |


<span id="certification-report-object" />
### Rapport de certification

Cette ressource donne accès aux données du rapport de certification pour une soumission. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description                                                                                                                                                                                                                          |
|-----------------|---------|------|
|     date            |    chaîne     |  Date et heure de génération du rapport, au format ISO8601.    |
|     reportUrl            |    chaîne     |  URL vous permettant d’accéder au rapport.    |


<span id="flight-package-object" />
### Package de version d’évaluation

Cette ressource fournit des détails sur un package d’une soumission. L’exemple suivant illustre le format de cette ressource.

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

>**Remarque**&nbsp;&nbsp;Quand vous appelez la méthode de [mise à jour d’une soumission de version d’évaluation](update-a-flight-submission.md), seules les valeurs *fileName*, *fileStatus*, *minimumDirectXVersion* et *minimumSystemRam* de cet objet sont nécessaires dans le corps de la requête. Les autres valeurs sont renseignées par le Centre de développement.

| Valeur           | Type    | Description                                                                                                                                                                                                                          |
|-----------------|---------|------|
| fileName   |   chaîne      |  Nom du package.    |  
| fileStatus    | chaîne    |  État du package. Les valeurs possibles sont les suivantes: <ul><li>None</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>    |  
| id    |  chaîne   |  ID qui identifie de manière unique le package. Cette valeur est utilisée par le Centre de développement.   |     
| version    |  chaîne   |  Version du package d’application. Pour plus d’informations, voir [Numérotation des versions de packages](https://msdn.microsoft.com/windows/uwp/publish/package-version-numbering).   |   
| architecture    |  chaîne   |  Architecture du package d’application (par exemple, ARM).   |     
| languages    | tableau    |  Tableau de codes de langue pour les langues prises en charge par l’application. Pour plus d’informations, voir [Langues prises en charge](https://msdn.microsoft.com/windows/uwp/publish/supported-languages).    |     
| capabilities    |  tableau   |  Tableau des fonctionnalités exigées par le package. Pour plus d’informations sur les fonctionnalités, voir [Déclarations des fonctionnalités d’application](https://msdn.microsoft.com/windows/uwp/packaging/app-capability-declarations).   |     
| minimumDirectXVersion    |  chaîne   |  Version DirectX minimale prise en charge par le package d’application. Cette valeur peut être définie uniquement pour les applications qui ciblent Windows8.x. Elle est ignorée pour les applications qui ciblent d’autres versions. Les valeurs possibles sont les suivantes: <ul><li>None</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | chaîne    |  Mémoire RAM minimale exigée par le package d’application. Cette valeur peut être définie uniquement pour les applications qui ciblent Windows8.x. Elle est ignorée pour les applications qui ciblent d’autres versions. Les valeurs possibles sont les suivantes: <ul><li>None</li><li>Memory2GB</li></ul>   |    

<span/>

## Enums

Ces méthodes utilisent les énumérations suivantes.

<span id="submission-status-code" />
### Code d’état de la soumission

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
| ServiceError | Une erreur de service interne a empêché la demande de réussir. Relancez la demande. |
| ListingOptOutWarning | Le développeur a supprimé une liste d’une soumission précédente ou il n’a pas inclus d’informations de référencement prises en charge par le package. |
| ListingOptInWarning  | Le développeur a ajouté une description. |
| UpdateOnlyWarning | Le développeur essaie d’insérer une opération qui prend uniquement en charge la mise à jour. |
| Other  | La soumission est dans un état non reconnu ou n’appartenant aucune catégorie. |
| PackageValidationWarning | Le processus de validation du package a généré un avertissement. |

<span/>

## Rubriques connexes

* [Créer et gérer des soumissions à l’aide des services du Windows Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gérer les versions d’évaluation du package à l’aide de l’API de soumission du Windows Store](manage-flights.md)
* [Obtenir une soumission de version d’évaluation du package](get-a-flight-submission.md)
* [Créer une soumission de version d’évaluation du package](create-a-flight-submission.md)
* [Mettre à jour une soumission de version d’évaluation de package](update-a-flight-submission.md)
* [Valider une soumission de version d’évaluation du package](commit-a-flight-submission.md)
* [Supprimer une soumission de version d’évaluation du package](delete-a-flight-submission.md)
* [Obtenir l’état d’une soumission de version d’évaluation du package](get-status-for-a-flight-submission.md)



<!--HONumber=Aug16_HO5-->

