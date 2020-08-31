---
ms.assetid: E8751EBF-AE0F-4107-80A1-23C186453B1C
description: Utilisez cette méthode dans l’API de soumission Microsoft Store pour mettre à jour une soumission d’application existante.
title: Mettre à jour une soumission d’application
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store API de soumission, soumission d’une application, mise à jour
ms.localizationpriority: medium
ms.openlocfilehash: 393b7c48723409dfc630c6f85770c4c0f7dcd1a4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158363"
---
# <a name="update-an-app-submission"></a>Mettre à jour une soumission d’application

Utilisez cette méthode dans l’API de soumission Microsoft Store pour mettre à jour une soumission d’application existante. Après avoir mis à jour une soumission à l’aide de cette méthode, vous devez [valider la soumission](commit-an-app-submission.md) en vue de son intégration et de sa publication.

Pour plus d’informations sur la façon dont cette méthode s’intègre au processus de création d’une soumission d’application à l’aide de l’API de soumission Microsoft Store, consultez [gérer les envois d’applications](manage-app-submissions.md).

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) pour l’API de soumission Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Créez une soumission pour l’une de vos applications. Vous pouvez effectuer cette opération dans l’espace partenaires, ou vous pouvez le faire à l’aide de la méthode [créer une soumission d’application](create-an-app-submission.md) .

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de demande                                                      |
|--------|------------------------------------------------------------------|
| PUT   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}  ``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la demande

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Obligatoire. ID Windows Store de l’application pour laquelle vous voulez mettre à jour une soumission. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](../publish/view-app-identity-details.md).  |
| submissionId | string | Obligatoire. ID de la soumission à mettre à jour. Cet ID est disponible dans les données de réponse pour les demandes de [création d’une soumission d’application](create-an-app-submission.md). Pour une soumission qui a été créée dans l’espace partenaires, cet ID est également disponible dans l’URL de la page de soumission dans l’espace partenaires.  |


### <a name="request-body"></a>Corps de la demande

Le corps de la requête contient les paramètres suivants.

| Valeur      | Type   | Description                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| applicationCategory           | string  |   Chaîne qui spécifie la [catégorie et/ou sous-catégorie](../publish/category-and-subcategory-table.md) pour votre application. Les catégories et sous-catégories sont combinées en une seule chaîne à l’aide du caractère trait de soulignement « _ », par exemple **BooksAndReference_EReader**.      |  
| Prix           |  object  | Objet qui contient les informations de tarification pour l’application. Pour plus d’informations, voir la section relative à la [ressource de tarification](manage-app-submissions.md#pricing-object).       |   
| visibility           |  string  |  Visibilité de l’application. Il peut s’agir de l’une des valeurs suivantes : <ul><li>Hidden</li><li>Public</li><li>Privées</li><li>NotSet</li></ul>       |   
| targetPublishMode           | string  | Mode de publication pour la soumission. Il peut s’agir de l’une des valeurs suivantes : <ul><li>Immédiat</li><li>Manuel</li><li>SpecificDate</li></ul> |
| targetPublishDate           | string  | Date de publication de la soumission au format ISO 8601, si le paramètre *targetPublishMode* a la valeur SpecificDate.  |  
| listings           |   object  |  Dictionnaire de paires clé/valeur, où chaque clé est un code de pays et chaque valeur est un objet de [ressource de référencement](manage-app-submissions.md#listing-object) qui contient des informations de référencement pour l’application.       |   
| hardwarePreferences           |  tableau  |   Tableau de chaînes qui définissent les [préférences matérielles](../publish/enter-app-properties.md) pour votre application. Il peut s’agir de l’une des valeurs suivantes : <ul><li>Toucher</li><li>Clavier</li><li>Souris</li><li>Appareil photo</li><li>NfcHce</li><li>NFC</li><li>BluetoothLE</li><li>Telephony</li></ul>     |   
| automaticBackupEnabled           |  boolean  |   Indique si Windows peut inclure les données de votre application dans les sauvegardes automatiques sur OneDrive. Pour plus d’informations, voir [Déclarations d’application](../publish/product-declarations.md).   |   
| canInstallOnRemovableMedia           |  boolean  |   Indique si les clients peuvent installer votre application sur un stockage amovible. Pour plus d’informations, voir [Déclarations d’application](../publish/product-declarations.md).     |   
| isGameDvrEnabled           |  boolean |   Indique si les jeux DVR sont activés pour l’application.    |   
| gamingOptions           |  object |   Tableau qui contient une [ressource d’options de jeu](manage-app-submissions.md#gaming-options-object) qui définit des paramètres liés aux jeux pour l’application.     |   
| hasExternalInAppProducts           |     boolean          |   Indique si votre application permet aux utilisateurs d’effectuer des achats en dehors du système de commerce Microsoft Store. Pour plus d’informations, voir [Déclarations d’application](../publish/product-declarations.md).     |   
| meetAccessibilityGuidelines           |    boolean           |  Indique si votre application a fait l’objet de tests pour voir si elle est conforme aux recommandations d’accessibilité. Pour plus d’informations, voir [Déclarations d’application](../publish/product-declarations.md).      |   
| notesForCertification           |  string  |   Contient des [notes de certification](../publish/notes-for-certification.md) pour votre application.    |    
| applicationPackages           |   tableau  | Contient des objets qui fournissent des détails sur chaque package de la soumission. Pour plus d’informations, voir la section [Package d’application](manage-app-submissions.md#application-package-object). Quand vous appelez cette méthode pour mettre à jour une soumission d’application, seules les valeurs *fileName*, *fileStatus*, *minimumDirectXVersion* et *minimumSystemRam* de ces objets sont nécessaires dans le corps de la requête. Les autres valeurs sont remplies par l’espace partenaires.   |    
| packageDeliveryOptions    | object  | Contient les paramètres de déploiement de package progressif et de mise à jour obligatoire de la soumission. Pour plus d’informations, consultez [Objet options de remise du package](manage-app-submissions.md#package-delivery-options-object).  |
| enterpriseLicensing           |  string  |  Une des [valeur de gestion des licences d’entreprise](manage-app-submissions.md#enterprise-licensing) qui indiquent le comportement de la gestion des licences d’entreprise pour l’application.  |    
| allowMicrosftDecideAppAvailabilityToFutureDeviceFamilies           |  boolean   |  Indique si Microsoft est autorisé à [rendre l’application disponible pour les futures familles d’appareils Windows 10](../publish/set-app-pricing-and-availability.md).    |    
| allowTargetFutureDeviceFamilies           | boolean   |  Indique si votre application est autorisée à [cibler les futures familles d’appareils Windows 10](../publish/set-app-pricing-and-availability.md).     |   
| codes           |  tableau |   Tableau qui contient jusqu’à [ressources de terminaison](manage-app-submissions.md#trailer-object) qui représentent des codes de fin vidéo pour la liste des applications.   |   


### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment mettre à jour une soumission d’application.

```json
PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621230023 HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
{
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2"
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
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "PendingUpload",
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
  "enterpriseLicensing": "Online",
  "allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies": true,
  "allowTargetFutureDeviceFamilies": {
    "Desktop": false,
    "Mobile": true,
    "Holographic": true,
    "Xbox": false,
    "Team": true
  },
  "trailers": []
}
```

## <a name="response"></a>response

L’exemple suivant illustre le corps de réponse JSON d’un appel réussi à cette méthode. Le corps de la réponse contient des informations sur la soumission mise à jour. Pour plus d’informations sur les valeurs figurant dans le corps de la réponse, voir la [ressource de soumission d’application](manage-app-submissions.md#app-submission-object).

```json
{
  "id": "1152921504621243540",
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2"
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
            "imageType": "Screenshot"
          }
        ],
        "recommendedHardware": [],
        "title": "Contoso ebook reader"
      },
      "platformOverrides": {
        "Windows81": {
          "description": "Ebook reader for Windows 8.1",
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
      "fileStatus": "PendingUpload",
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

## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Code d'erreur |  Description   |
|--------|------------------|
| 400  | Impossible de mettre à jour la soumission, car la requête n’est pas valide. |
| 409  | La soumission n’a pas pu être mise à jour en raison de l’état actuel de l’application, ou l’application utilise une fonctionnalité de l’espace partenaires qui n’est [actuellement pas prise en charge par l’API de soumission Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obtenir une soumission d’application](get-an-app-submission.md)
* [Créer une soumission d’applications](create-an-app-submission.md)
* [Valider une soumission d’application](commit-an-app-submission.md)
* [Supprimer une soumission d’application](delete-an-app-submission.md)
* [Obtenir l’état d’une soumission d’application](get-status-for-an-app-submission.md)