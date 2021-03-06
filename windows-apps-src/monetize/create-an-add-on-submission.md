---
ms.assetid: C09F4B7C-6324-4973-980A-A60035792EFC
description: Utilisez cette méthode dans l’API de soumission de Microsoft Store pour créer une nouvelle soumission de module complémentaire pour une application inscrite auprès de partenaires.
title: Créer une soumission d’extension
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, API de soumission au Microsoft Store, créer une soumission d’extension, produit in-app, PIA
ms.localizationpriority: medium
ms.openlocfilehash: cbb093576badf5cd84b132cfb139db9da7d31991
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334927"
---
# <a name="create-an-add-on-submission"></a>Créer une soumission d’extension

Utilisez cette méthode dans l’API de soumission de Microsoft Store pour créer une nouvelle soumission de module complémentaire (également appelés dans l’application produit ou produits) pour une application qui est inscrit pour votre compte espace partenaires. Après avoir créé une soumission à l’aide de cette méthode, [mettez à jour cette soumission](update-an-add-on-submission.md) pour apporter les modifications nécessaires aux données de soumission, puis [validez la soumission](commit-an-add-on-submission.md) pour permettre son intégration et sa publication.

Pour plus d’informations sur la façon dont cette méthode s’inscrit dans le processus de création d’une soumission d’extension à l’aide de l’API de soumission au Microsoft Store, consultez [Gérer les soumissions d’extensions](manage-add-on-submissions.md).

> [!NOTE]
> Cette méthode permet de créer une soumission pour une extension existante. Pour créer une extension, utilisez la méthode [Créer une extension](create-an-add-on.md).

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission au Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Créer un module complémentaire pour l’une de vos applications. Vous pouvez le faire dans le centre de partenaires, ou vous pouvez le faire à l’aide de la [créer un module complémentaire](create-an-add-on.md) (méthode).

## <a name="request"></a>Demande

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de requête                                                      |
|--------|------------------------------------------------------------------|
| PUBLIER    | `https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions` |

### <a name="request-header"></a>En-tête de requête

| Header        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |

### <a name="request-parameters"></a>Paramètres de la requête

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | chaîne | Obligatoire. ID Windows Store de l’extension pour laquelle vous voulez créer une soumission. L’ID de Store est disponible dans le centre de partenaires, et il est inclus dans les données de réponse pour les demandes au [créer un module complémentaire](create-an-add-on.md) ou [obtenir les détails du module complémentaire](get-all-add-ons.md).  |

### <a name="request-body"></a>Corps de la requête

Ne fournissez pas de corps de requête pour cette méthode.

### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment créer une soumission pour une extension.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

L’exemple suivant illustre le corps de réponse JSON d’un appel réussi à cette méthode. Le corps de la réponse contient des informations sur la nouvelle soumission. Pour plus d’informations sur les valeurs figurant dans le corps de réponse, voir [ressource de soumission d’extension](manage-add-on-submissions.md#add-on-submission-object).

```json
{
  "id": "1152921504621243680",
  "contentType": "EMagazine",
  "keywords": [
    "books"
  ],
  "lifetime": "FiveDays",
  "listings": {
    "en": {
      "description": "English add-on description",
      "icon": {
        "fileName": "add-on-en-us-listing2.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (English)"
    },
    "ru": {
      "description": "Russian add-on description",
      "icon": {
        "fileName": "add-on-ru-listing.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (Russian)"
    }
  },
  "pricing": {
    "marketSpecificPricings": {
      "RU": "Tier3",
      "US": "Tier4",
    },
    "sales": [
      {
         "name": "Sale1",
         "basePriceId": "Free",
         "startDate": "2016-05-21T18:40:11.7369008Z",
         "endDate": "2016-05-22T18:40:11.7369008Z",
         "marketSpecificPricings": {
            "RU": "NotAvailable"
         }
      }
    ],
    "priceId": "Free",
    "isAdvancedPricingModel": true
  },
  "targetPublishDate": "2016-03-15T05:10:58.047Z",
  "targetPublishMode": "Immediate",
  "tag": "SampleTag",
  "visibility": "Public",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [
      {
        "code": "None",
        "details": "string"
      }
    ],
    "warnings": [
      {
        "code": "ListingOptOutWarning",
        "details": "You have removed listing language(s): []"
      }
    ],
    "certificationReports": [
      {
      }
    ]
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl",
  "friendlyName": "Submission 2"
}
```

## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Error code |  Description   |
|--------|------------------|
| 400  | Impossible de créer la soumission, car la requête n’est pas valide. |
| 409  | L’envoi n’a pas pu être créé en raison de l’état actuel de l’application, ou l’application utilise une fonctionnalité de partenaires est [actuellement ne pas pris en charge par l’API de soumission de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   

## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gérer les envois de module complémentaire](manage-add-on-submissions.md)
* [Obtenir une présentation du module complémentaire](get-an-add-on-submission.md)
* [Valider une soumission de module complémentaire](commit-an-add-on-submission.md)
* [Mettre à jour une module complémentaire soumission](update-an-add-on-submission.md)
* [Supprimer un dépôt de module complémentaire](delete-an-add-on-submission.md)
* [Obtenir l’état d’une soumission de module complémentaire](get-status-for-an-add-on-submission.md)
