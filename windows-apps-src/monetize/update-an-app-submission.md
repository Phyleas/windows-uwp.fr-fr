---
author: mcleanbyron
ms.assetid: E8751EBF-AE0F-4107-80A1-23C186453B1C
description: "Utilisez cette méthode de l’API de soumission du Windows Store pour mettre à jour une soumission d’application existante."
title: "Mettre à jour une soumission d’application à l’aide de l’API de soumission du Windows Store"
translationtype: Human Translation
ms.sourcegitcommit: 178b70db1583790c174d65e060c8bce6e4f69243
ms.openlocfilehash: ad1c565f1ec84127b2ac689cc7cb23d2b39764ef

---

# Mettre à jour une soumission d’application à l’aide de l’API de soumission du Windows Store




Utilisez cette méthode de l’API de soumission du Windows Store pour mettre à jour une soumission d’application existante. Après avoir mis à jour une soumission à l’aide de cette méthode, vous devez [valider la soumission](commit-an-app-submission.md) en vue de son intégration et de sa publication.

Pour plus d’informations sur la façon dont cette méthode s’inscrit dans le processus de création d’une soumission d’application à l’aide de l’API de soumission du Windows Store, voir [Gérer les soumissions d’application](manage-app-submissions.md).

## Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission du Windows Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Créez une soumission pour une application dans votre compte du Centre de développement. Pour cela, vous pouvez utiliser le tableau de bord du Centre de développement ou la méthode [Créer une soumission d’application](create-an-app-submission.md).

>**Remarque**&nbsp;&nbsp;Cette méthode ne peut être utilisée que pour les comptes du Centre de développement Windows qui ont reçu l’autorisation d’utiliser l’API de soumission du Windows Store. Tous les comptes ne bénéficient pas de cette autorisation.

## Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de la requête                                                      |
|--------|------------------------------------------------------------------|
| PUT   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}  ``` |

<span/>
 

### En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |

<span/>

### Paramètres de la requête

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | chaîne | Obligatoire. ID Windows Store de l’application pour laquelle vous voulez mettre à jour une soumission. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| submissionId | chaîne | Obligatoire. ID de la soumission à mettre à jour. Cet ID est disponible dans le tableau de bord du Centre de développement et figure également dans les données de réponse aux requêtes visant à [créer une soumission d’application](create-an-app-submission.md).  |

<span/>

### Corps de la requête

Le corps de la requête contient les paramètres suivants.

| Valeur      | Type   | Description                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| applicationCategory           | chaîne  |   Chaîne qui spécifie la [catégorie et/ou sous-catégorie](https://msdn.microsoft.com/windows/uwp/publish/category-and-subcategory-table) pour votre application. Les catégories et sous-catégories sont combinées en une seule chaîne à l’aide du caractère trait de soulignement«_», par exemple **BooksAndReference_EReader**.      |  
| pricing           |  objet  | Objet qui contient les informations de tarification pour l’application. Pour plus d’informations, voir la section relative à la [ressource de tarification](manage-app-submissions.md#pricing-object).       |   
| visibility           |  chaîne  |  Visibilité de l’application. Les valeurs possibles sont les suivantes: <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>       |   
| targetPublishMode           | chaîne  | Mode de publication pour la soumission. Les valeurs possibles sont les suivantes: <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | chaîne  | Date de publication de la soumission au format ISO8601, si le paramètre *targetPublishMode* a la valeur SpecificDate.  |  
| listings           |   objet  |  Dictionnaire de paires clé/valeur, où chaque clé est un code de pays et chaque valeur est un objet de [ressource de référencement](manage-app-submissions.md#listing-object) qui contient des informations de référencement pour l’application.       |   
| hardwarePreferences           |  tableau  |   Tableau de chaînes qui définissent les [préférences matérielles](https://msdn.microsoft.com/windows/uwp/publish/enter-app-properties#hardware_preferences) pour votre application. Les valeurs possibles sont les suivantes: <ul><li>Touch</li><li>Keyboard</li><li>Mouse</li><li>Camera</li><li>NfcHce</li><li>NFC</li><li>BluetoothLE</li><li>Telephony</li></ul>     |   
| automaticBackupEnabled           |  booléen  |   Indique si Windows peut inclure les données de votre application dans les sauvegardes automatiques sur OneDrive. Pour plus d’informations, voir [Déclarations d’application](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).   |   
| canInstallOnRemovableMedia           |  booléen  |   Indique si les clients peuvent installer votre application sur un stockage amovible. Pour plus d’informations, voir [Déclarations d’application](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).     |   
| isGameDvrEnabled           |  booléen |   Indique si les jeux DVR sont activés pour l’application.    |   
| hasExternalInAppProducts           |     booléen          |   Indique si votre application permet aux utilisateurs d’effectuer des achats hors du système de commerce du Windows Store. Pour plus d’informations, voir [Déclarations d’application](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).     |   
| meetAccessibilityGuidelines           |    booléen           |  Indique si votre application a fait l’objet de tests pour voir si elle est conforme aux recommandations d’accessibilité. Pour plus d’informations, voir [Déclarations d’application](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).      |   
| notesForCertification           |  chaîne  |   Contient des [notes de certification](https://msdn.microsoft.com/windows/uwp/publish/notes-for-certification) pour votre application.    |    
| applicationPackages           |   tableau  | Contient des objets qui fournissent des détails sur chaque package de la soumission. Pour plus d’informations, voir la section [Package d’application](manage-app-submissions.md#application-package-object). Quand vous appelez cette méthode pour mettre à jour une soumission d’application, seules les valeurs *fileName*, *fileStatus*, *minimumDirectXVersion* et *minimumSystemRam* de ces objets sont nécessaires dans le corps de la requête. Les autres valeurs sont renseignées par le Centre de développement.   |    
| enterpriseLicensing           |  chaîne  |  Une des [valeur de gestion des licences d’entreprise](#enterprise-licensing) qui indiquent le comportement de la gestion des licences d’entreprise pour l’application.  |    
| allowMicrosftDecideAppAvailabilityToFutureDeviceFamilies           |  booléen   |  Indique si Microsoft est autorisé à [rendre l’application disponible pour les futures familles d’appareils Windows10](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families).    |    
| allowTargetFutureDeviceFamilies           | booléen   |  Indique si votre application est autorisée à [cibler les futures familles d’appareils Windows10](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families).     |    

<span/>

### Exemple de requête

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
        "keywords": [],
        "licenseTerms": "",
        "privacyPolicy": "",
        "supportContact": "",
        "websiteUrl": "",
        "description": "Description",
        "features": [],
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
        "title": "ApiTestApp For Devbox"
      },
      "platformOverrides": {}
    }
  },
  "hardwarePreferences": [
    "Touch"
  ],
  "automaticBackupEnabled": false,
  "canInstallOnRemovableMedia": true,
  "isGameDvrEnabled": false,
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
  "enterpriseLicensing": "Online",
  "allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies": true,
  "allowTargetFutureDeviceFamilies": {
    "Desktop": false,
    "Mobile": true,
    "Holographic": true,
    "Xbox": false,
    "Team": true
  }
}
```

## Réponse

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
        "keywords": [],
        "licenseTerms": "",
        "privacyPolicy": "",
        "supportContact": "",
        "websiteUrl": "",
        "description": "Description",
        "features": [],
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
        "title": "ApiTestApp For Devbox"
      },
      "platformOverrides": {}
    }
  },
  "hardwarePreferences": [
    "Touch"
  ],
  "automaticBackupEnabled": false,
  "canInstallOnRemovableMedia": true,
  "isGameDvrEnabled": false,
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
  "enterpriseLicensing": "Online",
  "allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies": true,
  "allowTargetFutureDeviceFamilies": {
    "Desktop": false,
    "Mobile": true,
    "Holographic": true,
    "Xbox": false,
    "Team": true
  },
  "friendlyName": "Submission 2"
}
```

## Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Error code |  Description   |
|--------|------------------|
| 400  | Impossible de mettre à jour la soumission, car la requête n’est pas valide. |
| 409  | La soumission n’a pas pu être mise à jour en raison de l’état actuel de l’application, ou celle-ci utilise une fonctionnalité du tableau de bord du Centre de développement qui n’est [actuellement pas prise en charge par l’API de soumission du Windows Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   

<span/>


## Rubriques connexes

* [Créer et gérer des soumissions à l’aide des services du Windows Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obtenir une soumission d’application](get-an-app-submission.md)
* [Créer une soumission d’application](create-an-app-submission.md)
* [Valider une soumission d’application](commit-an-app-submission.md)
* [Supprimer une soumission d’application](delete-an-app-submission.md)
* [Obtenir l’état d’une soumission d’application](get-status-for-an-app-submission.md)



<!--HONumber=Aug16_HO5-->

