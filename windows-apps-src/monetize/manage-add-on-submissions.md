---
ms.assetid: 66400066-24BF-4AF2-B52A-577F5C3CA474
description: Utilisez ces méthodes dans l’API de soumission Microsoft Store pour gérer les envois de compléments pour les applications qui sont inscrites auprès de votre compte espace partenaires.
title: Gérer les soumissions d’extensions
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, API de soumission Microsoft Store, soumissions de module complémentaire, produit dans l’application, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 16c9fa0f4fa4b7b6ac3ec8e0fb005b80ab1b7872
ms.sourcegitcommit: 7e8dfd83b181fe720b4074cb42adc908e1ba5e44
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98811313"
---
# <a name="manage-add-on-submissions"></a>Gérer les soumissions d’extensions

L’API de soumission Microsoft Store fournit des méthodes que vous pouvez utiliser pour gérer les envois de module complémentaire (également appelé produit dans l’application ou IAP) pour vos applications. Pour obtenir une présentation de l’API de soumission Microsoft Store, y compris les conditions préalables à l’utilisation de l’API, consultez [créer et gérer des soumissions à l’aide des services Microsoft Store](create-and-manage-submissions-using-windows-store-services.md).

> [!IMPORTANT]
> Si vous utilisez l’API de soumission Microsoft Store pour créer une soumission pour un module complémentaire, veillez à apporter des modifications supplémentaires à l’envoi uniquement à l’aide de l’API, plutôt que d’apporter des modifications à l’espace partenaires. Si vous utilisez l’espace partenaires pour modifier une soumission créée à l’origine à l’aide de l’API, vous ne pourrez plus modifier ou valider cette soumission à l’aide de l’API. Dans certains cas, la soumission peut être laissée dans un état d’erreur où elle ne peut pas continuer dans le processus de soumission. Si cela se produit, vous devez supprimer l’envoi et créer une nouvelle soumission.

<span id="methods-for-add-on-submissions" />

## <a name="methods-for-managing-add-on-submissions"></a>Méthodes de gestion des soumissions d’un module complémentaire

Pour obtenir, créer, mettre à jour, valider ou supprimer une soumission d’extension, utilisez les méthodes ci-dessous. Avant de pouvoir utiliser ces méthodes, le module complémentaire doit déjà exister dans votre compte espace partenaires. Vous pouvez créer un module complémentaire dans l’espace partenaires en [définissant son type de produit et son ID de produit,](../publish/set-your-add-on-product-id.md) ou en utilisant les méthodes d’API de soumission Microsoft Store décrites dans [gérer les modules complémentaires](manage-add-ons.md).

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
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}</td>
<td align="left"><a href="get-an-add-on-submission.md">Obtient une soumission d’extension existante</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/status</td>
<td align="left"><a href="get-status-for-an-add-on-submission.md">Obtient l’état d’une soumission d’extension existante</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions</td>
<td align="left"><a href="create-an-add-on-submission.md">Créer une soumission d’extension</a></td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}</td>
<td align="left"><a href="update-an-add-on-submission.md">Met à jour une soumission d’extension existante</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/commit</td>
<td align="left"><a href="commit-an-add-on-submission.md">Valide une soumission d’extension nouvelle ou mise à jour</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}</td>
<td align="left"><a href="delete-an-add-on-submission.md">Supprimer une soumission d’extension</a></td>
</tr>
</tbody>
</table>

<span id="create-an-add-on-submission">

## <a name="create-an-add-on-submission"></a>Créer une soumission d’extension

Pour créer une soumission pour une extension, suivez ce processus.

1. Si vous ne l’avez pas encore fait, complétez les conditions préalables décrites dans [créer et gérer des envois à l’aide de Microsoft Store services](create-and-manage-submissions-using-windows-store-services.md), notamment associer une application Azure ad à votre compte espace partenaires et obtenir votre ID client et votre clé. Vous n’aurez à le faire qu’une seule fois. Une fois à votre disposition, l’ID client et la clé sont réutilisables chaque fois que vous avez besoin de créer un jeton d’accès Azure AD.  

2. [Obtenez un jeton d’accès Azure ad](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Vous devez transmettre ce jeton d’accès aux méthodes de l’API de soumission Microsoft Store. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

3. Exécutez la méthode suivante dans l’API de soumission Microsoft Store. Cette méthode crée une soumission en cours, qui est une copie de votre dernière soumission publiée. Pour plus d’informations, voir [Créer une soumission d’extension](create-an-add-on-submission.md).

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions
    ```

    Le corps de la réponse contient une ressource de [soumission de module complémentaire](#add-on-submission-object) qui inclut l’ID de la nouvelle soumission, l’URI de signature d’accès partagé (SAS) pour le chargement de toutes les icônes de module complémentaire pour l’envoi vers le stockage d’objets BLOB Azure et toutes les données pour la nouvelle soumission (telles que les listes et les informations de tarification).

    > [!NOTE]
    > Un URI SAP permet d’accéder à une ressource sécurisée dans le stockage Azure sans avoir besoin de clés de compte. Pour obtenir des informations générales sur les URI SAS et leur utilisation avec le stockage d’objets BLOB Azure, consultez [signatures d’accès partagé, partie 1 : présentation du modèle SAS](/azure/storage/common/storage-sas-overview) et [signatures d’accès partagé, partie 2 : créer et utiliser une signature d’accès partagé avec un stockage d’objets BLOB](/azure/storage/common/storage-sas-overview).

4. Si vous ajoutez de nouvelles icônes pour la soumission, [préparez-les](../publish/create-add-on-store-listings.md) et ajoutez-les à une archive ZIP.

5. Mettez à jour les données d' [envoi du module complémentaire](#add-on-submission-object) avec les modifications requises pour la nouvelle soumission, puis exécutez la méthode suivante pour mettre à jour la soumission. Pour plus d’informations, voir [Mettre à jour une soumission d’extension](update-an-add-on-submission.md).

    ```json
    PUT https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}
    ```
      > [!NOTE]
      > Si vous ajoutez de nouvelles icônes pour la soumission, veillez à mettre à jour les données d’envoi pour faire référence au nom et au chemin d’accès relatif de ces fichiers dans l’archive ZIP.

4. Si vous ajoutez de nouvelles icônes pour l’envoi, téléchargez l’archive ZIP dans le [stockage d’objets BLOB Azure](/azure/storage/storage-introduction#blob-storage) à l’aide de l’URI SAS fourni dans le corps de la réponse de la méthode post que vous avez appelée précédemment. Vous pouvez utiliser différentes bibliothèques Azure pour effectuer cette opération sur de nombreuses plateformes, notamment :

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

5. Validez la soumission en exécutant la méthode suivante. Cela indiquera à l’espace partenaires que vous avez terminé avec votre envoi et que vos mises à jour doivent maintenant être appliquées à votre compte. Pour plus d’informations, voir [Valider une soumission d’extension](commit-an-add-on-submission.md).

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/commit
    ```

6. Vérifiez l’état de validation en exécutant la méthode suivante. Pour plus d’informations, voir [Obtenir l’état d’une soumission d’extension](get-status-for-an-add-on-submission.md).

    ```json
    GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/status
    ```

    Pour vérifier l’état de la soumission, examinez la valeur *status* dans le corps de la réponse. Cette valeur doit passer de **CommitStarted** à **PreProcessing** si la requête aboutit ou à **CommitFailed** si elle contient des erreurs. S’il existe des erreurs, le champ *statusDetails* contient d’autres détails s’y rapportant.

7. Une fois la validation correctement terminée, la soumission est envoyée au Windows Store en vue de son intégration. Vous pouvez continuer à surveiller la progression de l’envoi à l’aide de la méthode précédente ou en visitant l’espace partenaires.

<span/>

## <a name="code-examples"></a>Exemples de code

Les articles suivants fournissent des exemples de code détaillés qui montrent comment créer une soumission d’extension dans différents langages de programmation :

* [Exemples de code C#](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Exemples de code Java](java-code-examples-for-the-windows-store-submission-api.md)
* [Exemples de code Python](python-code-examples-for-the-windows-store-submission-api.md)

## <a name="storebroker-powershell-module"></a>Module PowerShell StoreBroker

Au lieu d’appeler directement l’API de soumission Microsoft Store, nous fournissons également un module PowerShell Open source qui implémente une interface de ligne de commande en plus de l’API. Ce module est appelé [StoreBroker](https://github.com/Microsoft/StoreBroker). Vous pouvez utiliser ce module pour gérer vos envois d’application, de vol et de module complémentaire à partir de la ligne de commande au lieu d’appeler directement l’API de soumission Microsoft Store, ou vous pouvez simplement parcourir la source pour voir d’autres exemples sur l’appel de cette API. Le module StoreBroker est utilisé activement au sein de Microsoft comme méthode principale que de nombreuses applications de première partie sont soumises au magasin.

Pour plus d’informations, consultez notre [page StoreBroker sur GitHub](https://github.com/Microsoft/StoreBroker).

<span/>

## <a name="data-resources"></a>Ressources de données

Les méthodes d’API de soumission Microsoft Store pour la gestion des envois de compléments utilisent les ressources de données JSON suivantes.

<span id="add-on-submission-object" />

### <a name="add-on-submission-resource"></a>Ressource de soumission d’extension

Cette ressource décrit une soumission d’extension.

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
    "sales": [],
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

Cette ressource a les valeurs suivantes.

| Valeur      | Type   | Description        |
|------------|--------|----------------------|
| id            | string  | ID de la soumission. Cet ID est disponible dans les données de réponse pour les demandes de [création d’une soumission de module complémentaire](create-an-add-on-submission.md), d' [obtenir tous les modules](get-all-add-ons.md)complémentaires et [d’obtenir un module complémentaire](get-an-add-on.md). Pour une soumission qui a été créée dans l’espace partenaires, cet ID est également disponible dans l’URL de la page de soumission dans l’espace partenaires.  |
| contentType           | string  |  [Type de contenu](../publish/enter-add-on-properties.md#content-type) qui est fourni dans l’extension. Il peut s’agir de l’une des valeurs suivantes : <ul><li>NotSet</li><li>BookDownload</li><li>EMagazine</li><li>ENewspaper</li><li>MusicDownload</li><li>MusicStream</li><li>OnlineDataStorage</li><li>VideoDownload</li><li>VideoStream</li><li>Asp</li><li>OnlineDownload</li></ul> |  
| mots clés           | tableau  | Tableau de chaînes qui contiennent jusqu’à 10 [mots clés](../publish/enter-add-on-properties.md#keywords) pour l’extension. Votre application peut rechercher des extensions à l’aide de ces mots clés.   |
| lifetime           | string  |  Durée de vie de l’extension. Il peut s’agir de l’une des valeurs suivantes : <ul><li>Toujours</li><li>OneDay</li><li>ThreeDays</li><li>FiveDays</li><li>OneWeek</li><li>TwoWeeks</li><li>OneMonth</li><li>TwoMonths</li><li>ThreeMonths</li><li>SixMonths</li><li>OneYear</li></ul> |
| listings           | objet  |  Dictionnaire de paires clé/valeur, où chaque clé est un code de pays à deux lettres ISO 3166-1 alpha-2, et chaque valeur est un objet de [ressource de référencement](#listing-object) qui contient les informations de référencement de l’extension.  |
| Prix           | objet  | [Ressource de tarification](#pricing-object) qui contient les informations de tarification de l’extension.   |
| targetPublishMode           | string  | Mode de publication pour la soumission. Il peut s’agir de l’une des valeurs suivantes : <ul><li>Immédiat</li><li>Manuel</li><li>SpecificDate</li></ul> |
| targetPublishDate           | string  | Date de publication de la soumission au format ISO 8601, si le paramètre *targetPublishMode* a la valeur SpecificDate.  |
| tag           | string  |  [Données développeur personnalisées](../publish/enter-add-on-properties.md#custom-developer-data) de l’extension (ces informations étaient précédemment appelées *tag*).   |
| visibility  | string  |  Visibilité de l’extension. Il peut s’agir de l’une des valeurs suivantes : <ul><li>Hidden</li><li>Public</li><li>Privé</li><li>NotSet</li></ul>  |
| status  | string  |  État de la soumission. Il peut s’agir de l’une des valeurs suivantes : <ul><li>Aucun</li><li>Opération annulée</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publication</li><li>Publié</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Libérer</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | objet  |  [Ressource des détails d’état](#status-details-object) qui contient des détails supplémentaires sur l’état de la soumission, notamment des informations sur les éventuelles erreurs. |
| fileUploadUrl           | string  | URI de la signature d’accès partagé (SAS) pour le chargement des packages de la soumission. Si vous ajoutez de nouveaux packages à la soumission, chargez l’archive ZIP contenant les packages vers cet URI. Pour plus d’informations, voir [Créer une soumission d’extension](#create-an-add-on-submission).  |
| friendlyName  | string  |  Nom convivial de la soumission, comme indiqué dans l’espace partenaires. Cette valeur est générée lorsque vous créez l’envoi.  |

<span id="listing-object" />

### <a name="listing-resource"></a>Ressource de référencement

Cette ressource contient [des informations de liste pour un module complémentaire](../publish/create-add-on-store-listings.md). Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description       |
|-----------------|---------|------|
|  description               |    string     |   Description du listing d’extensions.   |     
|  icon               |   objet      |[Ressource d’icône](#icon-object) qui contient les données de l’icône du listing d’extensions.    |
|  title               |     string    |   Titre du listing d’extensions.   |  

<span id="icon-object" />

### <a name="icon-resource"></a>Ressource d’icône

Cette ressource contient les données d’icône du listing d’extensions. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description     |
|-----------------|---------|------|
|  fileName               |    string     |   Nom du fichier d’icône dans l’archive ZIP que vous avez chargé pour la soumission. L’icône doit être un fichier. png qui mesure exactement 300 x 300 pixels.   |     
|  fileStatus               |   string      |  État du fichier d’icône. Il peut s’agir de l’une des valeurs suivantes : <ul><li>Aucun</li><li>PendingUpload</li><li>Téléchargé</li><li>PendingDelete</li></ul>   |

<span id="pricing-object" />

### <a name="pricing-resource"></a>Ressource de tarification

Cette ressource contient des informations de tarification pour l’extension. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description    |
|-----------------|---------|------|
|  marketSpecificPricings               |    objet     |  Dictionnaire de paires clé/valeur, où chaque clé est un code de pays à deux lettres ISO 3166-1 alpha-2 et chaque valeur est un [niveau de prix](#price-tiers). Ces éléments représentent les [prix personnalisés de votre extension sur des marchés spécifiques](../publish/set-add-on-pricing-and-availability.md). Tous les éléments de ce dictionnaire remplacent le prix de base spécifié par la valeur *priceId* du marché spécifié.     |     
|  sales               |   tableau      |  **Déconseillé**. Tableau des [ressources de ventes](#sale-object) qui contiennent des informations commerciales pour l’extension.     |     
|  priceId               |   string      |  [Niveau de prix](#price-tiers) qui spécifie le [prix de base](../publish/set-add-on-pricing-and-availability.md) de l’extension.    |    
|  isAdvancedPricingModel               |   boolean      |  Si la **valeur est true**, votre compte de développeur a accès à l’ensemble développé de niveaux tarifaires de. 99 usd à 1999,99 USD. Si la **valeur est false**, votre compte de développeur a accès à l’ensemble d’origine des niveaux de tarification de. 99 usd à 999,99 USD. Pour plus d’informations sur les différents niveaux, consultez [niveaux tarifaires](#price-tiers).<br/><br/> &nbsp; Remarque &nbsp; Ce champ est en lecture seule.   |


<span id="sale-object" />

### <a name="sale-resource"></a>Ressource Sale

Cette ressource contient des informations commerciales sur une extension.

> [!IMPORTANT]
> La ressource de **vente** n’est plus prise en charge. actuellement, vous ne pouvez pas obtenir ou modifier les données de vente pour une soumission de module complémentaire à l’aide de l’API Microsoft Store soumission. À l’avenir, nous mettrons à jour l’API de soumission Microsoft Store pour introduire une nouvelle façon d’accéder par programmation aux informations relatives aux ventes pour les envois de compléments.
>    * Après avoir appelé la [méthode GET pour soumettre un module complémentaire](get-an-add-on-submission.md), la ressource *Sales* est vide. Vous pouvez continuer à utiliser l’espace partenaires pour obtenir les données de vente pour votre envoi de module complémentaire.
>    * Lors de l’appel de la [méthode PUT pour mettre à jour la soumission d’un module complémentaire](update-an-add-on-submission.md), les informations de la valeur *Sales* sont ignorées. Vous pouvez continuer à utiliser l’espace partenaires pour modifier les données de vente de votre soumission de module complémentaire.

Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description           |
|-----------------|---------|------|
|  name               |    string     |   Nom de la vente.    |     
|  basePriceId               |   string      |  [Niveau de prix](#price-tiers) à utiliser pour le prix de base de la vente.    |     
|  startDate               |   string      |   Date de début de la vente au format ISO 8601.  |     
|  endDate               |   string      |  Date de fin de la vente au format ISO 8601.      |     
|  marketSpecificPricings               |   objet      |   Dictionnaire de paires clé/valeur, où chaque clé est un code de pays à deux lettres ISO 3166-1 alpha-2 et chaque valeur est un [niveau de prix](#price-tiers). Ces éléments représentent les [prix personnalisés de votre extension sur des marchés spécifiques](../publish/set-add-on-pricing-and-availability.md). Tous les éléments de ce dictionnaire remplacent le prix de base spécifié par la valeur *basePriceId* du marché spécifié.    |

<span id="status-details-object" />

### <a name="status-details-resource"></a>Ressource des détails d’état

Cette ressource contient des détails supplémentaires sur l’état d’une soumission. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description       |
|-----------------|---------|------|
|  erreurs               |    objet     |   Tableau des [ressources des détails d’état](#status-detail-object) qui contiennent les détails d’erreur de la soumission.   |     
|  warnings               |   objet      | Tableau des [ressources des détails d’état](#status-detail-object) qui contiennent les détails d’avertissement de la soumission.     |
|  certificationReports               |     objet    |   Tableau des [ressources de rapport de certification](#certification-report-object) qui donnent accès aux données du rapport de certification de la soumission. Vous pouvez examiner ces rapports pour obtenir plus d’informations en cas d’échec de la certification.    |  

<span id="status-detail-object" />

### <a name="status-detail-resource"></a>Ressource des détails d’état

Cette ressource contient des informations supplémentaires sur les éventuels erreurs ou avertissements pour une soumission. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description    |
|-----------------|---------|------|
|  code               |    string     |   [Code d’état de soumission](#submission-status-code) qui décrit le type d’erreur ou d’avertissement.   |     
|  details               |     string    |  Message contenant des détails sur le problème.     |

<span id="certification-report-object" />

### <a name="certification-report-resource"></a>Ressource de rapport de certification

Cette ressource donne accès aux données du rapport de certification d’une soumission. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description               |
|-----------------|---------|------|
|     Date            |    string     |  Date et heure de génération du rapport, au format ISO 8601.    |
|     reportUrl            |    string     |  URL vous permettant d’accéder au rapport.    |

## <a name="enums"></a>Énumérations

Ces méthodes utilisent les énumérations suivantes.

<span id="price-tiers" />

### <a name="price-tiers"></a>Niveaux de prix

Les valeurs suivantes représentent les niveaux de tarification disponibles dans la ressource de [ressource de tarification](#pricing-object) pour une soumission de module complémentaire.

| Valeur           | Description       |
|-----------------|------|
|  Base               |   Le niveau de prix n’est pas défini ; utilisez le prix de base de l’extension.      |     
|  NotAvailable              |   L’extension n’est pas disponible dans la région spécifiée.    |     
|  Gratuit              |   L’extension est gratuite.    |    
|  Niveau *xxxx*               |   Chaîne qui spécifie le niveau de prix du module complémentaire, au format de **niveau <em>xxxx</em>**. Actuellement, les plages de prix suivantes sont prises en charge :<br/><br/><ul><li>Si la valeur *isAdvancedPricingModel* de la [ressource de tarification](#pricing-object) est **true**, les valeurs de niveau de tarification disponibles pour votre compte sont **Tier1012**  -  **Tier1424**.</li><li>Si la valeur *isAdvancedPricingModel* de la [ressource de tarification](#pricing-object) est **false**, les valeurs de niveau de tarification disponibles pour votre compte sont **niveau2**  -  **Tier96**.</li></ul>Pour afficher le tableau complet des niveaux de prix disponibles pour votre compte de développeur, y compris les prix spécifiques au marché qui sont associés à chaque niveau, accédez à la page **tarification et disponibilité** pour l’une de vos soumissions d’application dans l’espace partenaires, puis cliquez sur le lien afficher **la** **table** dans la section **marchés et prix personnalisés** .     |

<span id="submission-status-code" />

### <a name="submission-status-code"></a>Code d’état de soumission

Les valeurs suivantes représentent le code d’état d’une soumission.

| Valeur           |  Description      |
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
* [Gérer les modules complémentaires à l’aide de l’API de soumission Microsoft Store](manage-add-ons.md)
* [Envois de module complémentaire dans l’espace partenaires](../publish/add-on-submissions.md)
