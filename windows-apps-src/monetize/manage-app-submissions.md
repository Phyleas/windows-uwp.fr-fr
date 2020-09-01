---
ms.assetid: C7428551-4B31-4259-93CD-EE229007C4B8
description: Utilisez ces méthodes dans l’API de soumission Microsoft Store pour gérer les envois des applications inscrites auprès de votre compte espace partenaires.
title: Gérer les soumissions d’applications
ms.date: 04/30/2018
ms.topic: article
keywords: Windows 10, UWP, API de soumission Microsoft Store, soumissions d’applications
ms.localizationpriority: medium
ms.openlocfilehash: de612607da2192af3358c94874e0896557ca6d08
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171363"
---
# <a name="manage-app-submissions"></a>Gérer les soumissions d’applications

L’API de soumission Microsoft Store fournit des méthodes que vous pouvez utiliser pour gérer les envois de vos applications, y compris les déploiements de packages progressifs. Pour obtenir une présentation de l’API de soumission Microsoft Store, y compris les conditions préalables à l’utilisation de l’API, consultez [créer et gérer des soumissions à l’aide des services Microsoft Store](create-and-manage-submissions-using-windows-store-services.md).

> [!IMPORTANT]
> Si vous utilisez l’API de soumission Microsoft Store pour créer une soumission pour une application, assurez-vous d’apporter d’autres modifications à l’envoi uniquement à l’aide de l’API, plutôt que de l’espace partenaires. Si vous utilisez l’espace partenaires pour modifier une soumission créée à l’origine à l’aide de l’API, vous ne pourrez plus modifier ou valider cette soumission à l’aide de l’API. Dans certains cas, la soumission peut être laissée dans un état d’erreur où elle ne peut pas continuer dans le processus de soumission. Si cela se produit, vous devez supprimer l’envoi et créer une nouvelle soumission.

> [!IMPORTANT]
> Vous ne pouvez pas utiliser cette API pour publier des soumissions pour des [achats en volume via le Microsoft Store pour l’entreprise et Microsoft Store pour l’éducation](../publish/organizational-licensing.md) ou pour publier des soumissions pour les [applications métier](../publish/distribute-lob-apps-to-enterprises.md) directement aux entreprises. Pour ces deux scénarios, vous devez utiliser l’espace partenaires pour publier l’envoi.


<span id="methods-for-app-submissions" />

## <a name="methods-for-managing-app-submissions"></a>Méthodes de gestion des soumissions d’applications

Pour obtenir, créer, mettre à jour, valider ou supprimer une soumission d’applications, procédez comme suit. Avant de pouvoir utiliser ces méthodes, l’application doit déjà exister dans votre compte espace partenaires et vous devez d’abord créer une soumission pour l’application dans l’espace partenaires. Pour plus d’informations, voir les [Conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites).

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
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}</td>
<td align="left"><a href="get-an-app-submission.md">Obtient une soumission d’applications existante</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status</td>
<td align="left"><a href="get-status-for-an-app-submission.md">Obtient l’état d’une soumission d’applications existante</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions</td>
<td align="left"><a href="create-an-app-submission.md">Crée une soumission d’applications</a></td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}</td>
<td align="left"><a href="update-an-app-submission.md">Met à jour une soumission d’applications existante</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit</td>
<td align="left"><a href="commit-an-app-submission.md">Valide une soumission d’applications nouvelle ou mise à jour</a></td>
</tr>
<tr>
<td align="left">Suppression</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}</td>
<td align="left"><a href="delete-an-app-submission.md">Supprimer une soumission d’application</a></td>
</tr>
</tbody>
</table>

<span id="create-an-app-submission">

### <a name="create-an-app-submission"></a>Créer une soumission d’applications

Pour créer une soumission pour une application, suivez ce processus.

1. Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) pour l’API de soumission Microsoft Store.
    > [!NOTE]
    > Assurez-vous que l’application a déjà au moins une soumission terminée avec les informations d’évaluation de l' [âge](../publish/age-ratings.md) terminées.

2. [Obtenez un jeton d’accès Azure ad](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Vous devez transmettre ce jeton d’accès aux méthodes de l’API de soumission Microsoft Store. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

3. [Créez une soumission d’application](create-an-app-submission.md) en exécutant la méthode suivante dans le Microsoft Store API de soumission. Cette méthode crée une soumission en cours, qui est une copie de votre dernière soumission publiée.

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions
    ```

    Le corps de la réponse contient une ressource de [soumission d’application](#app-submission-object) qui inclut l’ID de la nouvelle soumission, l’URI de signature d’accès partagé (SAS) pour le téléchargement de tous les fichiers associés pour la soumission vers le stockage d’objets BLOB Azure (tels que les packages d’application, l’affichage d’images et les fichiers de code de fin) et toutes les données pour la nouvelle soumission

    > [!NOTE]
    > Un URI SAP permet d’accéder à une ressource sécurisée dans le stockage Azure sans avoir besoin de clés de compte. Pour obtenir des informations générales sur les URI SAS et leur utilisation avec le stockage d’objets blob Azure, consultez [Signatures d’accès partagé, partie 1 : présentation du modèle SAS](/azure/storage/common/storage-sas-overview) et [Signatures d’accès partagé, partie 2 : créer et utiliser une SAS avec le stockage d’objets blob](/azure/storage/common/storage-sas-overview).

4. Si vous ajoutez de nouveaux packages, en répertoriant des images ou des fichiers de code de fin pour la soumission, [Préparez les packages d’application](../publish/app-package-requirements.md) et [Préparez les captures d’écran, les images et les codes](../publish/app-screenshots-and-images.md)de fin de l’application. Ajoutez tous ces fichiers à une archive ZIP.

5. Révisez les données d’envoi de l' [application](#app-submission-object) avec les modifications requises pour la nouvelle soumission, puis exécutez la méthode suivante pour [mettre à jour la soumission de l’application](update-an-app-submission.md).

    ```json
    PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}
    ```
      > [!NOTE]
      > Si vous ajoutez de nouveaux fichiers pour la soumission, veillez à mettre à jour les données d’envoi pour faire référence au nom et au chemin d’accès relatif de ces fichiers dans l’archive ZIP.

4. Si vous ajoutez de nouveaux packages, en répertoriant des images ou des fichiers de code de fin pour la soumission, chargez l’archive ZIP dans le [stockage d’objets BLOB Azure](/azure/storage/storage-introduction#blob-storage) à l’aide de l’URI SAS fourni dans le corps de la réponse de la méthode post que vous avez appelée précédemment. Vous pouvez utiliser différentes bibliothèques Azure pour effectuer cette opération sur de nombreuses plateformes, notamment :

    * [Bibliothèque cliente Azure Storage pour .NET](/azure/storage/storage-dotnet-how-to-use-blobs)
    * [Kit de développement logiciel (SDK) Azure Storage pour Java](/azure/storage/storage-java-how-to-use-blob-storage)
    * [Kit de développement logiciel (SDK) Azure Storage pour Python](/azure/storage/storage-python-how-to-use-blob-storage)

    L’exemple de code suivant en C# montre comment charger une archive ZIP vers le stockage d’objets blob Azure à l’aide de la classe [CloudBlockBlob](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob) incluse dans la bibliothèque cliente de stockage Azure pour .NET. Cet exemple repose sur le principe que l’archive ZIP a déjà été écrite dans un objet de flux.

    ```csharp
    string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";
    Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
        new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
    await blockBob.UploadFromStreamAsync(stream);
    ```

5. [Validez la soumission d’applications](commit-an-app-submission.md) en exécutant la méthode suivante. Cela indiquera à l’espace partenaires que vous avez terminé avec votre envoi et que vos mises à jour doivent maintenant être appliquées à votre compte.

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit
    ```

6. Vérifiez l’état de validation en exécutant la méthode suivante pour [obtenir l’état de la soumission d’application](get-status-for-an-app-submission.md).

    ```json
    GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status
    ```

    Pour vérifier l’état de la soumission, examinez la valeur *status* dans le corps de la réponse. Cette valeur doit passer de **CommitStarted** à **PreProcessing** si la requête aboutit ou à **CommitFailed** si elle contient des erreurs. S’il existe des erreurs, le champ *statusDetails* contient d’autres détails s’y rapportant.

7. Une fois la validation correctement terminée, la soumission est envoyée au Windows Store en vue de son intégration. Vous pouvez continuer à surveiller la progression de l’envoi à l’aide de la méthode précédente ou en visitant l’espace partenaires.

<span id="manage-gradual-package-rollout">

## <a name="methods-for-managing-a-gradual-package-rollout"></a>Méthodes de gestion d’un lancement de packages progressif

Vous pouvez publier progressivement les packages mis à jour d’une soumission d’application pour un pourcentage des clients de votre application sur Windows 10. Cela vous permet de surveiller les commentaires et les données d’analyse des packages spécifiques et de vérifier l’adéquation de votre mise à jour avant de la déployer plus largement. Vous pouvez modifier le pourcentage de lancement (ou arrêter la mise à jour) d’une soumission publiée sans avoir à créer une nouvelle soumission. Pour plus d’informations, notamment des instructions sur l’activation et la gestion d’un déploiement de package graduel dans l’espace partenaires, consultez [cet article](../publish/gradual-package-rollout.md).

Pour activer par programmation le déploiement graduel d’un package pour une soumission d’application, suivez ce processus à l’aide des méthodes de l’API Microsoft Store soumission :

  1. [Créez une soumission d’applications](create-an-app-submission.md) ou [obtenez une soumission d’applications existante](get-an-app-submission.md).
  2. Dans les données de réponse, localisez la ressource [packageRollout](#package-rollout-object) , affectez la valeur **true**au champ *isPackageRollout* , puis définissez le champ *packageRolloutPercentage* sur le pourcentage des clients de votre application qui doivent obtenir les packages mis à jour.
  3. Transmettez les données de soumission d’applications mises à jour à la méthode [Mettre à jour une soumission d’applications](update-an-app-submission.md).

Après avoir activé un lancement de packages progressif pour une soumission d’applications, vous pouvez utiliser les méthodes suivantes pour obtenir, mettre à jour, arrêter ou finaliser le lancement progressif par programmation.

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
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/packagerollout</td>
<td align="left"><a href="get-package-rollout-info-for-an-app-submission.md">Obtient des informations sur le lancement progressif d’une soumission d’applications</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/updatepackagerolloutpercentage</td>
<td align="left"><a href="update-the-package-rollout-percentage-for-an-app-submission.md">Met à jour le pourcentage de lancement progressif d’une soumission d’applications</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/haltpackagerollout</td>
<td align="left"><a href="halt-the-package-rollout-for-an-app-submission.md">Arrête le lancement progressif d’une soumission d’applications</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/finalizepackagerollout</td>
<td align="left"><a href="finalize-the-package-rollout-for-an-app-submission.md">Finalise le lancement progressif d’une soumission d’applications</a></td>
</tr>
</tbody>
</table>


## <a name="code-examples-for-managing-app-submissions"></a>Exemples de code pour gérer des soumissions d’applications

Les articles suivants fournissent des exemples de code détaillés qui montrent comment créer une soumission d’applications dans différents langages de programmation :

* [Exemple de code C# : soumissions d’applications, d’extensions et de versions d’évaluation](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Exemple de code C# : soumission d’applications avec options de jeu et bandes-annonces](csharp-code-examples-for-submissions-game-options-and-trailers.md)
* [Exemple de code Java : soumissions d’applications, d’extensions et de versions d’évaluation](java-code-examples-for-the-windows-store-submission-api.md)
* [Exemple de code Java : soumission d’applications avec options de jeu et bandes-annonces](java-code-examples-for-submissions-game-options-and-trailers.md)
* [Exemple de code Python : soumissions d’applications, d’extensions et de versions d’évaluation](python-code-examples-for-the-windows-store-submission-api.md)
* [Exemple de code Python : soumission d’applications avec options de jeu et bandes-annonces](python-code-examples-for-submissions-game-options-and-trailers.md)

## <a name="storebroker-powershell-module"></a>Module PowerShell StoreBroker

Au lieu d’appeler directement l’API de soumission Microsoft Store, nous fournissons également un module PowerShell Open source qui implémente une interface de ligne de commande en plus de l’API. Ce module est appelé [StoreBroker](https://github.com/Microsoft/StoreBroker). Vous pouvez utiliser ce module pour gérer vos envois d’application, de vol et de module complémentaire à partir de la ligne de commande au lieu d’appeler directement l’API de soumission Microsoft Store, ou vous pouvez simplement parcourir la source pour voir d’autres exemples sur l’appel de cette API. Le module StoreBroker est utilisé activement au sein de Microsoft comme méthode principale que de nombreuses applications de première partie sont soumises au magasin.

Pour plus d’informations, consultez notre [page StoreBroker sur GitHub](https://github.com/Microsoft/StoreBroker).

<span/>

## <a name="data-resources"></a>Ressources de données
Les méthodes d’API de soumission Microsoft Store pour gérer les envois d’application utilisent les ressources de données JSON suivantes.

<span id="app-submission-object" />

### <a name="app-submission-resource"></a>Ressource de soumission d’applications

Cette ressource décrit une soumission d’applications.

```json
{
  "id": "1152921504621243540",
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2",
    "isAdvancedPricingModel": true
  },
  "visibility": "Public",
  "targetPublishMode": "Manual",
  "targetPublishDate": "1601-01-01T00:00:00Z",
  "listings": {
    "en-us": {
      "baseListing": {
        "copyrightAndTrademarkInfo": "",
        "keywords": [
          "epub"
        ],
        "licenseTerms": "",
        "privacyPolicy": "",
        "supportContact": "",
        "websiteUrl": "",
        "description": "Description",
        "features": [
          "Free ebook reader"
        ],
        "releaseNotes": "",
        "images": [
          {
            "fileName": "contoso.png",
            "fileStatus": "Uploaded",
            "id": "1152921504672272757",
            "description": "Main page",
            "imageType": "Screenshot"
          }
        ],
        "recommendedHardware": [],
        "title": "Contoso ebook reader"
      },
      "platformOverrides": {
        "Windows81": {
          "description": "Ebook reader for Windows 8.1"
        }
      }
    }
  },
  "hardwarePreferences": [
    "Touch"
  ],
  "automaticBackupEnabled": false,
  "canInstallOnRemovableMedia": true,
  "isGameDvrEnabled": false,
  "gamingOptions": [],
  "hasExternalInAppProducts": false,
  "meetAccessibilityGuidelines": true,
  "notesForCertification": "",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/387a9ea8-a412-43a9-8fb3-a38d03eb483d?sv=2014-02-14&sr=b&sig=sdd12JmoaT6BhvC%2BZUrwRweA%2Fkvj%2BEBCY09C2SZZowg%3D&se=2016-06-17T18:32:26Z&sp=rwl",
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "Uploaded",
      "id": "1152921504620138797",
      "version": "1.0.0.0",
      "architecture": "ARM",
      "languages": [
        "en-US"
      ],
      "capabilities": [
        "ID_RESOLUTION_HD720P",
        "ID_RESOLUTION_WVGA",
        "ID_RESOLUTION_WXGA"
      ],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None",
      "targetDeviceFamilies": [
        "Windows.Mobile min version 10.0.10240.0"
      ]
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
  "enterpriseLicensing": "Online",
  "allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies": true,
  "allowTargetFutureDeviceFamilies": {
    "Desktop": false,
    "Mobile": true,
    "Holographic": true,
    "Xbox": false,
    "Team": true
  },
  "friendlyName": "Submission 2",
  "trailers": []
}
```

Cette ressource a les valeurs suivantes.

| Valeur      | Type   | Description      |
|------------|--------|-------------------|
| id            | string  | ID de la soumission. Cet ID est disponible dans les données de réponse pour les demandes de [création d’une soumission d’application](create-an-app-submission.md), d' [extraction de toutes les applications](get-all-apps.md)et [d’extraction d’une application](get-an-app.md). Pour une soumission qui a été créée dans l’espace partenaires, cet ID est également disponible dans l’URL de la page de soumission dans l’espace partenaires.  |
| applicationCategory           | string  |   Chaîne qui spécifie la [catégorie et/ou sous-catégorie](../publish/category-and-subcategory-table.md) pour votre application. Les catégories et sous-catégories sont combinées en une seule chaîne à l’aide du caractère trait de soulignement « _ », par exemple **BooksAndReference_EReader**.      |  
| Prix           |  object  | [Ressource de tarification](#pricing-object) qui contient les informations de tarification de l’application.        |   
| visibility           |  string  |  Visibilité de l’application. Il peut s’agir de l’une des valeurs suivantes : <ul><li>Hidden</li><li>Public</li><li>Privées</li><li>NotSet</li></ul>       |   
| targetPublishMode           | string  | Mode de publication pour la soumission. Il peut s’agir de l’une des valeurs suivantes : <ul><li>Immédiat</li><li>Manuel</li><li>SpecificDate</li></ul> |
| targetPublishDate           | string  | Date de publication de la soumission au format ISO 8601, si le paramètre *targetPublishMode* a la valeur SpecificDate.  |  
| listings           |   object  |  Dictionnaire de paires clé/valeur, où chaque clé est un code de pays et chaque valeur est une [ressource de référencement](#listing-object) qui contient des informations de référencement de l’application.       |   
| hardwarePreferences           |  tableau  |   Tableau de chaînes qui définissent les [préférences matérielles](../publish/enter-app-properties.md) pour votre application. Il peut s’agir de l’une des valeurs suivantes : <ul><li>Toucher</li><li>Clavier</li><li>Souris</li><li>Appareil photo</li><li>NfcHce</li><li>NFC</li><li>BluetoothLE</li><li>Telephony</li></ul>     |   
| automaticBackupEnabled           |  boolean  |   Indique si Windows peut inclure les données de votre application dans les sauvegardes automatiques sur OneDrive. Pour plus d’informations, voir [Déclarations d’application](../publish/product-declarations.md).   |   
| canInstallOnRemovableMedia           |  boolean  |   Indique si les clients peuvent installer votre application sur un stockage amovible. Pour plus d’informations, voir [Déclarations d’application](../publish/product-declarations.md).     |   
| isGameDvrEnabled           |  boolean |   Indique si les jeux DVR sont activés pour l’application.    |   
| gamingOptions           |  tableau |   Tableau qui contient une [ressource d’options de jeu](#gaming-options-object) qui définit des paramètres liés aux jeux pour l’application.     |   
| hasExternalInAppProducts           |     boolean          |   Indique si votre application permet aux utilisateurs d’effectuer des achats en dehors du système de commerce Microsoft Store. Pour plus d’informations, voir [Déclarations d’application](../publish/product-declarations.md).     |   
| meetAccessibilityGuidelines           |    boolean           |  Indique si votre application a fait l’objet de tests pour voir si elle est conforme aux recommandations d’accessibilité. Pour plus d’informations, voir [Déclarations d’application](../publish/product-declarations.md).      |   
| notesForCertification           |  string  |   Contient des [notes de certification](../publish/notes-for-certification.md) pour votre application.    |    
| status           |   string  |  État de la soumission. Il peut s’agir de l’une des valeurs suivantes : <ul><li>Aucune</li><li>Opération annulée</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publication</li><li>Publié</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Libérer</li><li>ReleaseFailed</li></ul>      |    
| statusDetails           |   object  | [Ressource des détails d’état](#status-details-object) qui contient des détails supplémentaires sur l’état de la soumission, notamment des informations sur les éventuelles erreurs.       |    
| fileUploadUrl           |   string  | URI de la signature d’accès partagé (SAS) pour le chargement des packages de la soumission. Si vous ajoutez de nouveaux packages, en répertoriant des images ou des fichiers de code de fin pour la soumission, téléchargez l’archive ZIP qui contient les packages et les images sur cet URI. Pour plus d’informations, voir [Créer une soumission d’application](#create-an-app-submission).       |    
| applicationPackages           |   tableau  | Tableau des [ressources de package d’application](#application-package-object) qui fournissent des détails sur chaque package de la soumission. |    
| packageDeliveryOptions    | object  | [Ressource des options de remise du package](#package-delivery-options-object) qui contient les paramètres de lancement de packages progressif et de mise à jour obligatoire de la soumission.  |
| enterpriseLicensing           |  string  |  Une des [valeur de gestion des licences d’entreprise](#enterprise-licensing) qui indiquent le comportement de la gestion des licences d’entreprise pour l’application.  |    
| allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies           |  boolean   |  Indique si Microsoft est autorisé à [rendre l’application disponible pour les futures familles d’appareils Windows 10](../publish/set-app-pricing-and-availability.md).    |    
| allowTargetFutureDeviceFamilies           | object   |  Dictionnaire de paires clé/valeur, où chaque clé est une [famille d’appareils Windows 10](../publish/set-app-pricing-and-availability.md) et chaque valeur est une valeur booléenne qui indique si votre application est autorisée à cibler la famille d’appareils spécifiée.     |    
| friendlyName           |   string  |  Nom convivial de la soumission, comme indiqué dans l’espace partenaires. Cette valeur est générée lorsque vous créez l’envoi.       |  
| codes           |  tableau |   Tableau qui contient jusqu’à 15 [ressources de code de fin](#trailer-object) représentant des codes de fin vidéo pour la liste des applications.<br/><br/>   |  


<span id="pricing-object" />

### <a name="pricing-resource"></a>Ressource de tarification

Cette ressource contient des informations de tarification pour l’application. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description        |
|-----------------|---------|------|
|  trialPeriod               |    string     |  Chaîne qui spécifie la période d’évaluation de l’application. Il peut s’agir de l’une des valeurs suivantes : <ul><li>NoFreeTrial</li><li>OneDay</li><li>TrialNeverExpires</li><li>SevenDays</li><li>FifteenDays</li><li>ThirtyDays</li></ul>    |
|  marketSpecificPricings               |    object     |  Dictionnaire de paires clé/valeur, où chaque clé est un code de pays à deux lettres ISO 3166-1 alpha-2 et chaque valeur est un [niveau de prix](#price-tiers). Ces éléments représentent les [prix personnalisés de votre application sur des marchés spécifiques](../publish/define-market-selection.md). Tous les éléments de ce dictionnaire remplacent le prix de base spécifié par la valeur *priceId* du marché spécifié.      |     
|  sales               |   tableau      |  **Déconseillé**. Tableau des [ressources de ventes](#sale-object) qui contiennent des informations commerciales pour l’application.   |     
|  priceId               |   string      |  [Niveau de prix](#price-tiers) qui spécifie le [prix de base](../publish/define-market-selection.md) de l’application.   |     
|  isAdvancedPricingModel               |   boolean      |  Si la **valeur est true**, votre compte de développeur a accès à l’ensemble développé de niveaux tarifaires de. 99 usd à 1999,99 USD. Si la **valeur est false**, votre compte de développeur a accès à l’ensemble d’origine des niveaux de tarification de. 99 usd à 999,99 USD. Pour plus d’informations sur les différents niveaux, consultez [niveaux tarifaires](#price-tiers).<br/><br/>**Note** &nbsp; Remarque &nbsp; Ce champ est en lecture seule.   |


<span id="sale-object" />

### <a name="sale-resource"></a>Ressource Sale

Cette ressource contient des informations commerciales pour une application.

> [!IMPORTANT]
> La ressource de **vente** n’est plus prise en charge. actuellement, vous ne pouvez pas obtenir ou modifier les données de vente d’une soumission d’application à l’aide de l’API de soumission Microsoft Store. À l’avenir, nous mettrons à jour l’API de soumission Microsoft Store pour introduire une nouvelle façon d’accéder par programmation aux informations de vente pour les soumissions d’applications.
>    * Après avoir appelé la [méthode GET pour soumettre une application](get-an-app-submission.md), la ressource *Sales* est vide. Vous pouvez continuer à utiliser l’espace partenaires pour obtenir les données de vente pour la soumission de votre application.
>    * Lors de l’appel de la [méthode PUT pour mettre à jour la soumission d’une application](update-an-app-submission.md), les informations de la valeur *Sales* sont ignorées. Vous pouvez continuer à utiliser l’espace partenaires pour modifier les données de vente de la soumission de votre application.

Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description    |
|-----------------|---------|------|
|  name               |    string     |   Nom de la vente.    |     
|  basePriceId               |   string      |  [Niveau de prix](#price-tiers) à utiliser pour le prix de base de la vente.    |     
|  startDate               |   string      |   Date de début de la vente au format ISO 8601.  |     
|  endDate               |   string      |  Date de fin de la vente au format ISO 8601.      |     
|  marketSpecificPricings               |   object      |   Dictionnaire de paires clé/valeur, où chaque clé est un code de pays à deux lettres ISO 3166-1 alpha-2 et chaque valeur est un [niveau de prix](#price-tiers). Ces éléments représentent les [prix personnalisés de votre application sur des marchés spécifiques](../publish/define-market-selection.md). Tous les éléments de ce dictionnaire remplacent le prix de base spécifié par la valeur *basePriceId* du marché spécifié.    |


<span id="listing-object" />

### <a name="listing-resource"></a>Ressource de référencement

Cette ressource contient des informations de listing pour une application. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description                  |
|-----------------|---------|------|
|  baseListing               |   object      |  Informations de [listing de base](#base-listing-object) pour l’application, qui définissent les informations de listing par défaut pour toutes les plateformes.   |     
|  platformOverrides               | object |   Dictionnaire de paires clé/valeur, où chaque clé est une chaîne qui identifie une plateforme pour laquelle remplacer les informations de référencement et chaque valeur est une ressource de [référencement de base](#base-listing-object) (contenant uniquement les valeurs de la description au titre) qui spécifie les informations de référencement à remplacer pour la plateforme spécifiée. Les clés peuvent avoir les valeurs suivantes : <ul><li>Unknown</li><li>Windows80</li><li>Windows81</li><li>WindowsPhone71</li><li>WindowsPhone80</li><li>WindowsPhone81</li></ul>     |      |     

<span id="base-listing-object" />

### <a name="base-listing-resource"></a>Ressource de référencement de base

Cette ressource contient des informations de référencement de base pour une application. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description       |
|-----------------|---------|------|
|  copyrightAndTrademarkInfo                |   string      |  [Informations de copyright et/ou de marque](../publish/create-app-store-listings.md) facultatives.  |
|  mots clés                |  tableau       |  Tableau de [mots clés](../publish/create-app-store-listings.md) facilitant l’apparition de l’application dans les résultats de recherche.    |
|  licenseTerms                |    string     | [Termes du contrat de licence](../publish/create-app-store-listings.md) facultatifs de votre application.     |
|  privacyPolicy                |   string      |   Cette valeur est obsolète. Pour définir ou modifier l’URL de la politique de confidentialité de votre application, vous devez le faire dans la page [Propriétés](../publish/enter-app-properties.md#privacy-policy-url) de l’espace partenaires. Vous pouvez omettre cette valeur de vos appels à l’API de soumission. Si vous définissez cette valeur, elle sera ignorée.       |
|  supportContact                |   string      |  Cette valeur est obsolète. Pour définir ou modifier l’URL ou l’adresse de messagerie du contact de support pour votre application, vous devez le faire dans la page  [Propriétés](../publish/enter-app-properties.md#support-contact-info) de l’espace partenaires. Vous pouvez omettre cette valeur de vos appels à l’API de soumission. Si vous définissez cette valeur, elle sera ignorée.        |
|  websiteUrl                |   string      |  Cette valeur est obsolète. Pour définir ou modifier l’URL de la page Web de votre application, vous devez le faire dans la page  [Propriétés](../publish/enter-app-properties.md#website) de l’espace partenaires. Vous pouvez omettre cette valeur de vos appels à l’API de soumission. Si vous définissez cette valeur, elle sera ignorée.      |    
|  description               |    string     |   [Description](../publish/create-app-store-listings.md) du listing de l’application.   |     
|  features               |    tableau     |  Tableau contenant 20 chaînes au maximum qui répertorient les [fonctionnalités](../publish/create-app-store-listings.md) de votre application.     |
|  releaseNotes               |  string       |  [Notes de publication](../publish/create-app-store-listings.md) de votre application.    |
|  images               |   tableau      |  Tableau des ressources d’[image et d’icône](#image-object) de la description de l’application.  |
|  recommendedHardware               |   tableau      |  Tableau contenant jusqu’à 11 chaînes qui répertorient les [configurations matérielles recommandées](../publish/create-app-store-listings.md#additional-information) pour votre application.     |
|  minimumHardware               |     string    |  Tableau contenant jusqu’à 11 chaînes qui répertorient les [configurations matérielles minimales](../publish/create-app-store-listings.md#additional-information) pour votre application.    |  
|  title               |     string    |   Titre du listing de l’application.   |  
|  shortDescription               |     string    |  Utilisé uniquement pour les jeux. Cette description apparaît dans la section **informations** du Hub de jeu sur Xbox One et aide les clients à en savoir plus sur votre jeu.   |  
|  shortTitle               |     string    |  Version plus petite du nom de votre produit. S’il est fourni, ce nom plus petit peut apparaître à différents emplacements sur Xbox 1 (lors de l’installation, dans les réalisations, etc.) à la place du titre complet de votre produit.    |  
|  sortTitle               |     string    |   Si votre produit peut être alphabétique de différentes manières, vous pouvez entrer une autre version ici. Cela peut aider les clients à trouver plus rapidement le produit lors de la recherche.    |  
|  voiceTitle               |     string    |   Un autre nom pour votre produit qui, s’il est fourni, peut être utilisé dans l’expérience audio sur Xbox 1 lors de l’utilisation de Kinect ou d’un casque.    |  
|  devStudio               |     string    |   Spécifiez cette valeur si vous souhaitez inclure un champ **développé par** dans la liste. (Le champ **publié par** répertorie le nom complet de l’éditeur associé à votre compte, que vous fournissiez ou non une valeur *devStudio* .)    |  

<span id="image-object" />

### <a name="image-resource"></a>Ressource d’image

Cette ressource contient les données d’image et d’icône d’une description d’application. Pour plus d’informations sur les images et les icônes d’une liste d’applications, consultez [captures d’écran et images d’application](../publish/app-screenshots-and-images.md). Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description           |
|-----------------|---------|------|
|  fileName               |    string     |   Nom du fichier image dans l’archive ZIP que vous avez chargé pour la soumission.    |     
|  fileStatus               |   string      |  État du fichier image. Il peut s’agir de l’une des valeurs suivantes : <ul><li>Aucune</li><li>PendingUpload</li><li>Téléchargé</li><li>PendingDelete</li></ul>   |
|  id  |  string  | ID de l’image. Cette valeur est fournie par l’espace partenaires.  |
|  description  |  string  | Description de l’image.  |
|  imageType  |  string  | Indique le type de l’image. Les chaînes suivantes sont actuellement prises en charge. <p/>[Images de capture d’écran](../publish/app-screenshots-and-images.md#screenshots): <ul><li>Capture d’écran (utiliser cette valeur pour la capture d’écran du bureau)</li><li>MobileScreenshot</li><li>XboxScreenshot</li><li>SurfaceHubScreenshot</li><li>HoloLensScreenshot</li></ul><p/>[Stocker les logos](../publish/app-screenshots-and-images.md#store-logos):<ul><li>StoreLogo9x16 </li><li>StoreLogoSquare</li><li>Icône (utilisez cette valeur pour le logo 1:1 300 x 300 pixels)</li></ul><p/>[Images promotionnelles](../publish/app-screenshots-and-images.md#promotional-images): <ul><li>PromotionalArt16x9</li><li>PromotionalArtwork2400X1200</li></ul><p/>[Images Xbox](../publish/app-screenshots-and-images.md#xbox-images): <ul><li>XboxBrandedKeyArt</li><li>XboxTitledHeroArt</li><li>XboxFeaturedPromotionalArt</li></ul><p/>[Images promotionnelles facultatives](../publish/app-screenshots-and-images.md#optional-promotional-images): <ul><li>SquareIcon358X358</li><li>BackgroundImage1000X800</li><li>PromotionalArtwork414X180</li></ul><p/> <!-- The following strings are also recognized for this field, but they correspond to image types that are no longer for listings in the Store.<ul><li>PromotionalArtwork846X468</li><li>PromotionalArtwork558X756</li><li>PromotionalArtwork414X468</li><li>PromotionalArtwork558X558</li><li>WideIcon358X173</li><li>Unknown</li></ul> -->   |


<span id="gaming-options-object" />

### <a name="gaming-options-resource"></a>Ressource des options de jeu

Cette ressource contient des paramètres liés aux jeux pour l’application. Les valeurs de cette ressource correspondent aux [paramètres de jeu](../publish/enter-app-properties.md#game-settings) pour les envois dans l’espace partenaires.

```json
{
  "gamingOptions": [
    {
      "genres": [
        "Games_ActionAndAdventure",
        "Games_Casino"
      ],
      "isLocalMultiplayer": true,
      "isLocalCooperative": true,
      "isOnlineMultiplayer": false,
      "isOnlineCooperative": false,
      "localMultiplayerMinPlayers": 2,
      "localMultiplayerMaxPlayers": 12,
      "localCooperativeMinPlayers": 2,
      "localCooperativeMaxPlayers": 12,
      "isBroadcastingPrivilegeGranted": true,
      "isCrossPlayEnabled": false,
      "kinectDataForExternal": "Enabled"
    }
  ],
}
```

Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description        |
|-----------------|---------|------|
|  genres               |    tableau     |  Tableau d’une ou plusieurs des chaînes suivantes qui décrivent les genres du jeu : <ul><li>Games_ActionAndAdventure</li><li>Games_CardAndBoard</li><li>Games_Casino</li><li>Games_Educational</li><li>Games_FamilyAndKids</li><li>Games_Fighting</li><li>Games_Music</li><li>Games_Platformer</li><li>Games_PuzzleAndTrivia</li><li>Games_RacingAndFlying</li><li>Games_RolePlaying</li><li>Games_Shooter</li><li>Games_Simulation</li><li>Games_Sports</li><li>Games_Strategy</li><li>Games_Word</li></ul>    |
|  isLocalMultiplayer               |    boolean     |  Indique si le jeu prend en charge le mode multijoueur local.      |     
|  isLocalCooperative               |   boolean      |  Indique si le jeu prend en charge les co-op locaux.    |     
|  isOnlineMultiplayer               |   boolean      |  Indique si le jeu prend en charge le mode multijoueur en ligne.    |     
|  isOnlineCooperative               |   boolean      |  Indique si le jeu prend en charge les co-op en ligne.    |     
|  localMultiplayerMinPlayers               |   int      |   Spécifie le nombre minimal de joueurs pris en charge par le jeu pour la version multijoueur locale.   |     
|  localMultiplayerMaxPlayers               |   int      |   Spécifie le nombre maximal de joueurs pris en charge par le jeu pour le mode multijoueur local.  |     
|  localCooperativeMinPlayers               |   int      |   Spécifie le nombre minimal de joueurs pris en charge par le jeu pour les co-op locaux.  |     
|  localCooperativeMaxPlayers               |   int      |   Spécifie le nombre maximal de joueurs pris en charge par le jeu pour les co-op locaux.  |     
|  isBroadcastingPrivilegeGranted               |   boolean      |  Indique si le jeu prend en charge la diffusion.   |     
|  isCrossPlayEnabled               |   boolean      |   Indique si le jeu prend en charge les sessions multijoueur entre les joueurs sur les PC Windows 10 et Xbox.  |     
|  kinectDataForExternal               |   string      |  L’une des valeurs de chaîne suivantes qui indique si le jeu peut collecter des données Kinect et les envoyer à des services externes : <ul><li>NotSet</li><li>Unknown</li><li>activé</li><li>Désactivé</li></ul>   |

> [!NOTE]
> La ressource *gamingOptions* a été ajoutée à mai 2017, après la première publication de l’API de soumission Microsoft Store pour les développeurs. Si vous avez créé une soumission pour une application via l’API de soumission avant l’introduction de cette ressource et que cette soumission est toujours en cours, cette ressource sera null pour les soumissions de l’application jusqu’à ce que vous validiez la soumission ou que vous la supprimiez. Si la ressource *gamingOptions* n’est pas disponible pour les envois d’une application, le champ *hasAdvancedListingPermission* de la [ressource d’application](get-app-data.md#application_object) retournée par la méthode [obtenir une application](get-an-app.md) a la valeur false.

<span id="status-details-object" />

### <a name="status-details-resource"></a>Ressource des détails d’état

Cette ressource contient des détails supplémentaires sur l’état d’une soumission. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description         |
|-----------------|---------|------|
|  erreurs               |    object     |   Tableau des [ressources des détails d’état](#status-detail-object) qui contiennent les détails d’erreur de la soumission.    |     
|  warnings               |   object      | Tableau des [ressources des détails d’état](#status-detail-object) qui contiennent les détails d’avertissement de la soumission.      |
|  certificationReports               |     object    |   Tableau des [ressources de rapport de certification](#certification-report-object) qui donnent accès aux données du rapport de certification de la soumission. Vous pouvez examiner ces rapports pour obtenir plus d’informations en cas d’échec de la certification.   |  


<span id="status-detail-object" />

### <a name="status-detail-resource"></a>Ressource des détails d’état

Cette ressource contient des informations supplémentaires sur les éventuels erreurs ou avertissements pour une soumission. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description        |
|-----------------|---------|------|
|  code               |    string     |   [Code d’état de soumission](#submission-status-code) qui décrit le type d’erreur ou d’avertissement.   |     
|  details               |     string    |  Message contenant des détails sur le problème.     |


<span id="application-package-object" />

### <a name="application-package-resource"></a>Ressource de package d’application

Cette ressource contient des détails sur un package d’application pour la soumission.

```json
{
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "Uploaded",
      "id": "1152921504620138797",
      "version": "1.0.0.0",
      "architecture": "ARM",
      "languages": [
        "en-US"
      ],
      "capabilities": [
        "ID_RESOLUTION_HD720P",
        "ID_RESOLUTION_WVGA",
        "ID_RESOLUTION_WXGA"
      ],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None",
      "targetDeviceFamilies": [
        "Windows.Mobile min version 10.0.10240.0"
      ]
    }
  ],
}
```

Cette ressource a les valeurs suivantes.  

> [!NOTE]
> Lors de l’appel de la méthode [mettre à jour une soumission d’application](update-an-app-submission.md) , seules les valeurs *filename*, *fileStatus*, *minimumDirectXVersion*et *minimumSystemRam* de cet objet sont requises dans le corps de la demande. Les autres valeurs sont remplies par l’espace partenaires.

| Valeur           | Type    | Description                   |
|-----------------|---------|------|
| fileName   |   string      |  Nom du package.    |  
| fileStatus    | string    |  État du package. Il peut s’agir de l’une des valeurs suivantes : <ul><li>Aucune</li><li>PendingUpload</li><li>Téléchargé</li><li>PendingDelete</li></ul>    |  
| id    |  string   |  ID qui identifie de manière unique le package. Cette valeur est fournie par l’espace partenaires.   |     
| version    |  string   |  Version du package d’application. Pour plus d’informations, voir [Numérotation des versions de packages](../publish/package-version-numbering.md).   |   
| architecture    |  string   |  Architecture du package (par exemple, ARM).   |     
| languages    | tableau    |  Tableau des codes des langues prises en charge par l’application. Pour en savoir plus, consultez [Langages pris en charge](../publish/supported-languages.md).    |     
| capabilities    |  tableau   |  Tableau des fonctionnalités exigées par le package. Pour plus d’informations sur les fonctionnalités, voir [Déclarations des fonctionnalités d’application](../packaging/app-capability-declarations.md).   |     
| minimumDirectXVersion    |  string   |  Version DirectX minimale prise en charge par le package d’application. Cela peut être défini uniquement pour les applications qui ciblent Windows 8. x. Pour les applications qui ciblent d’autres versions de système d’exploitation, cette valeur doit être présente lors de l’appel de la méthode de [mise à jour d’une application de soumission](update-an-app-submission.md) , mais la valeur que vous spécifiez est ignorée. Il peut s’agir de l’une des valeurs suivantes : <ul><li>Aucune</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | string    |  Mémoire RAM minimale exigée par le package d’application. Cela peut être défini uniquement pour les applications qui ciblent Windows 8. x. Pour les applications qui ciblent d’autres versions de système d’exploitation, cette valeur doit être présente lors de l’appel de la méthode de [mise à jour d’une application de soumission](update-an-app-submission.md) , mais la valeur que vous spécifiez est ignorée. Il peut s’agir de l’une des valeurs suivantes : <ul><li>Aucune</li><li>Memory2GB</li></ul>   |       
| targetDeviceFamilies    | tableau    |  Tableau de chaînes qui représentent les familles d’appareils que le package cible. Cette valeur est utilisée uniquement pour les packages qui ciblent Windows 10. Pour les packages qui ciblent des versions antérieures, cette valeur est **None**. Les chaînes de famille d’appareils suivantes sont actuellement prises en charge pour les packages Windows 10, où *{0}* est une chaîne de version Windows 10 telle que 10.0.10240.0, 10.0.10586.0 ou 10.0.14393.0 : <ul><li>Version minimale de Windows. Universal *{0}*</li><li>Version minimale de Windows. Desktop *{0}*</li><li>Version minimale de Windows. mobile *{0}*</li><li>Windows. Xbox-version minimale *{0}*</li><li>Version minimale de Windows. holographique *{0}*</li></ul>   |    

<span/>

<span id="certification-report-object" />

### <a name="certification-report-resource"></a>Ressource de rapport de certification

Cette ressource donne accès aux données du rapport de certification d’une soumission. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description             |
|-----------------|---------|------|
|     Date            |    string     |  Date et heure de génération du rapport, au format ISO 8601.    |
|     reportUrl            |    string     |  URL vous permettant d’accéder au rapport.    |


<span id="package-delivery-options-object" />

### <a name="package-delivery-options-resource"></a>Ressource des options de remise du package

Cette ressource contient les paramètres de déploiement de package progressif et de mise à jour obligatoire de la soumission.

```json
{
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0,
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
| packageRollout   |   object      |  [Ressource de lancement de packages](#package-rollout-object) qui contient les paramètres de lancement de packages progressif de la soumission.   |  
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

<span id="trailer-object" />

### <a name="trailers-resource"></a>Ressource des codes de fin

Cette ressource représente un code de fin vidéo pour la liste des applications. Les valeurs de cette ressource correspondent aux options des [codes](../publish/app-screenshots-and-images.md#trailers) de fin des soumissions dans l’espace partenaires.

Vous pouvez ajouter jusqu’à 15 ressources de code de fin au tableau des *codes* de fin dans une [ressource de soumission d’application](#app-submission-object). Pour télécharger des fichiers vidéo de code de fin et des images miniatures pour une soumission, ajoutez ces fichiers à la même archive ZIP qui contient les packages et répertoriant les images pour la soumission, puis chargez cette archive ZIP sur l’URI de signature d’accès partagé (SAS) pour la soumission. Pour plus d’informations sur le téléchargement de l’archive ZIP vers l’URI SAS, consultez [créer une soumission d’application](#create-an-app-submission).

```json
{
  "trailers": [
    {
      "id": "1158943556954955699",
      "videoFileName": "Trailers\\ContosoGameTrailer.mp4",
      "videoFileId": "1159761554639123258",
      "trailerAssets": {
        "en-us": {
          "title": "Contoso Game",
          "imageList": [
            {
              "fileName": "Images\\ContosoGame-Thumbnail.png",
              "id": "1155546904097346923",
              "description": "This is a still image from the video."
            }
          ]
        }
      }
    }
  ]
}
```

Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description        |
|-----------------|---------|------|
|  id               |    string     |   ID du code de fin. Cette valeur est fournie par l’espace partenaires.   |
|  videoFileName               |    string     |    Nom du fichier vidéo de code de fin dans l’archive ZIP qui contient les fichiers pour l’envoi.    |     
|  videoFileId               |   string      |  ID du fichier vidéo de code de fin. Cette valeur est fournie par l’espace partenaires.   |     
|  trailerAssets               |   object      |  Dictionnaire de paires clé/valeur, où chaque clé est un code de langue et chaque valeur est une [ressource de source](#trailer-assets-object) de code de fin qui contient des ressources spécifiques aux paramètres régionaux supplémentaires pour le code de fin. Pour plus d’informations sur les codes de langue pris en charge, consultez [langues prises en charge](../publish/supported-languages.md).    |     

> [!NOTE]
> La ressource des *codes* de fin a été ajoutée à mai 2017, après la première publication de l’API de soumission Microsoft Store pour les développeurs. Si vous avez créé une soumission pour une application via l’API de soumission avant l’introduction de cette ressource et que cette soumission est toujours en cours, cette ressource sera null pour les soumissions de l’application jusqu’à ce que vous validiez la soumission ou que vous la supprimiez. Si la ressource des *codes* de fin n’est pas disponible pour les envois d’une application, le champ *hasAdvancedListingPermission* de la [ressource d’application](get-app-data.md#application_object) retournée par la méthode [obtenir une application](get-an-app.md) est false.

<span id="trailer-assets-object" />

### <a name="trailer-assets-resource"></a>Ressource des ressources de code de fin

Cette ressource contient des ressources spécifiques aux paramètres régionaux supplémentaires pour un code de fin défini dans une [ressource de code de fin](#trailer-object). Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description        |
|-----------------|---------|------|
| title   |   string      |  Titre localisé du code de fin. Le titre s’affiche lorsque l’utilisateur lit le code de fin en mode plein écran.     |  
| imageList    | tableau    |   Tableau qui contient une ressource d' [image](#image-for-trailer-object) qui fournit l’image miniature du code de fin. Vous ne pouvez inclure qu’une seule ressource [image](#image-for-trailer-object) dans ce tableau.  |   


<span id="image-for-trailer-object" />

### <a name="image-resource-for-a-trailer"></a>Ressource image (pour un code de fin)

Cette ressource décrit l’image miniature d’un code de fin. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description           |
|-----------------|---------|------|
|  fileName               |    string     |   Nom du fichier image miniature dans l’archive ZIP que vous avez téléchargée pour la soumission.    |     
|  id  |  string  | ID de l’image miniature. Cette valeur est fournie par l’espace partenaires.  |
|  description  |  string  | Description de l’image miniature. Cette valeur est uniquement des métadonnées et n’est pas affichée aux utilisateurs.   |

<span/>

## <a name="enums"></a>Énumérations

Ces méthodes utilisent les énumérations suivantes.

<span id="price-tiers" />

### <a name="price-tiers"></a>Niveaux de prix

Les valeurs suivantes représentent les niveaux de tarification disponibles dans la ressource de [ressource de tarification](#pricing-object) pour une soumission d’application.

| Valeur           | Description        |
|-----------------|------|
|  Base               |   Le niveau de prix n’est pas défini ; utilisez le prix de base de l’application.      |     
|  NotAvailable              |   L’application n’est pas disponible dans la région spécifiée.    |     
|  Gratuit              |   Cette application est gratuite.    |    
|  Niveau*xxx*               |   Chaîne qui spécifie le niveau de prix de l’application, au format de **niveau<em>xxxx</em>**. Actuellement, les plages de prix suivantes sont prises en charge :<br/><br/><ul><li>Si la valeur *isAdvancedPricingModel* de la [ressource de tarification](#pricing-object) est **true**, les valeurs de niveau de tarification disponibles pour votre compte sont **Tier1012**  -  **Tier1424**.</li><li>Si la valeur *isAdvancedPricingModel* de la [ressource de tarification](#pricing-object) est **false**, les valeurs de niveau de tarification disponibles pour votre compte sont **niveau2**  -  **Tier96**.</li></ul>Pour afficher le tableau complet des niveaux de prix disponibles pour votre compte de développeur, y compris les prix spécifiques au marché qui sont associés à chaque niveau, accédez à la page **tarification et disponibilité** pour l’une de vos soumissions d’application dans l’espace partenaires, puis cliquez sur le lien afficher **la** **table** dans la section **marchés et prix personnalisés** .    |


<span id="enterprise-licensing" />

### <a name="enterprise-licensing-values"></a>Valeurs de gestion des licences d’entreprise

Les valeurs suivantes représentent le comportement de la licence organisationnelle pour l’application. Pour plus d’informations sur ces options, voir [Options de gestion des licences organisationnelles](../publish/organizational-licensing.md).

> [!NOTE]
> Même si vous pouvez configurer les options de licence d’organisation pour une soumission d’application via l’API de soumission, vous ne pouvez pas utiliser cette API pour publier des soumissions pour les [achats en volume via le Microsoft Store pour l’entreprise et Microsoft Store pour l’enseignement](../publish/organizational-licensing.md). Pour publier des envois aux Microsoft Store pour les entreprises et les Microsoft Store pour l’éducation, vous devez utiliser l’espace partenaires.


| Valeur           |  Description      |
|-----------------|---------------|
| Aucune            |     Ne mets pas votre application à la disposition des entreprises dont les licences en volume sont gérées par le Windows Store (en ligne).         |     
| En ligne        |     Mets votre application à la disposition des entreprises dont les licences en volume sont gérées par le Windows Store (en ligne).  |
| OnlineAndOffline | Mets votre application à la disposition des entreprises dont les licences en volume sont gérées par le Windows Store (en ligne) et à la disposition des entreprises par le biais de l’achat de licences en mode hors connexion. |


<span id="submission-status-code" />

### <a name="submission-status-code"></a>Code d’état de soumission

Les valeurs suivantes représentent le code d’état d’une soumission.

| Valeur           |  Description      |
|-----------------|---------------|
| Aucune            |     Aucun code n’a été spécifié.         |     
| InvalidArchive        |     L’archive ZIP contenant le package n’est pas valide ou a un format d’archive non reconnu.  |
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
| Autres  | La soumission est dans un état non reconnu ou non affecté à une catégorie. |
| PackageValidationWarning | Le processus de validation du package a généré un avertissement. |

<span/>

## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Récupération de données d’application à l’aide de l’API de soumission Microsoft Store](get-app-data.md)
* [Envois d’applications dans l’espace partenaires](../publish/app-submissions.md)