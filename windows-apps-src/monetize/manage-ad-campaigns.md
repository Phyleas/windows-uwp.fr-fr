---
ms.assetid: 7b07a6ca-4be1-497c-a901-0a2da3762555
description: Utilisez cette méthode dans l’API Microsoft Store promotions pour créer, modifier et recevoir des campagnes publicitaires promotionnelles.
title: Gérer les campagnes publicitaires
ms.date: 02/08/2017
ms.topic: article
keywords: API de promotion Windows 10, UWP, Microsoft Store, campagnes ad
ms.localizationpriority: medium
ms.openlocfilehash: bf5945ca68a4ea060943cb2cc89ba907f597e4f5
ms.sourcegitcommit: 5d84d8fe60e83647fa363b710916cf8b92c6e331
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91878402"
---
# <a name="manage-ad-campaigns"></a>Gérer les campagnes publicitaires

Utilisez ces méthodes dans l' [API Microsoft Store promotions](run-ad-campaigns-using-windows-store-services.md) pour créer, modifier et obtenir des campagnes publicitaires promotionnelles pour votre application. Chaque campagne que vous créez à l’aide de cette méthode peut être associée à une seule application.

>**Note** &nbsp; Remarque &nbsp; Vous pouvez également créer et gérer des campagnes Active Directory à l’aide de l’espace partenaires, et les campagnes que vous créez par programme sont accessibles dans l’espace partenaires. Pour plus d’informations sur la gestion des campagnes Active Directory dans l’espace partenaires, consultez [créer une campagne publicitaire pour votre application](./index.md).

Lorsque vous utilisez ces méthodes pour créer ou mettre à jour une campagne, vous appelez généralement une ou plusieurs des méthodes suivantes pour gérer les *lignes de livraison*, les *profils de ciblage*et les *éléments créatifs* associés à la campagne. Pour plus d’informations sur la relation entre les campagnes, les lignes de livraison, les profils de ciblage et les éléments créatifs, consultez [exécuter des campagnes publicitaires à l’aide des services Microsoft Store](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api).

* [Gérer les lignes de livraison pour les campagnes de publicité](manage-delivery-lines-for-ad-campaigns.md)
* [Gérer les profils de ciblage pour les campagnes ad](manage-targeting-profiles-for-ad-campaigns.md)
* [Gérer les éléments créatifs pour les campagnes ad](manage-creatives-for-ad-campaigns.md)

## <a name="prerequisites"></a>Prérequis

Pour utiliser ces méthodes, vous devez d’abord effectuer les opérations suivantes :

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](run-ad-campaigns-using-windows-store-services.md#prerequisites) pour l’API Microsoft Store promotions.

  >**Note** &nbsp; Remarque &nbsp; Dans le cadre des conditions préalables, assurez-vous de [créer au moins une campagne publicitaire payante dans l’espace partenaires](./index.md) et d’ajouter au moins un instrument de paiement pour la campagne publicitaire dans l’espace partenaires. Les lignes de livraison des campagnes de publicité que vous créez à l’aide de cette API facturent automatiquement l’instrument de paiement par défaut choisi dans la page **campagnes publicitaires** de l’espace partenaires.

* [Obtenez un jeton d’accès Azure ad](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de demande pour ces méthodes. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.


## <a name="request"></a>Requête

Ces méthodes ont les URI suivants.

| Type de méthode | URI de requête                                                      |  Description  |
|--------|------------------------------------------------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign``` |  Crée une nouvelle campagne publicitaire.  |
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/{campaignId}``` |  Modifie la campagne Active Directory spécifiée par *campaignId*.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/{campaignId}``` |  Obtient la campagne publicitaire spécifiée par *campaignId*.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign``` |  Requêtes pour les campagnes Active Directory. Consultez la section [paramètres](#parameters) pour connaître les paramètres de requête pris en charge.  |


### <a name="header"></a>En-tête

| En-tête        | Type   | Description         |
|---------------|--------|---------------------|
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |
| ID de suivi   | GUID   | facultatif. ID qui effectue le suivi du workflow d’appel.                                  |


<span id="parameters"/> 

### <a name="parameters"></a>Paramètres

La méthode obtenir pour interroger des campagnes Active Directory prend en charge les paramètres de requête facultatifs suivants.

| Nom        | Type   |  Description      |    
|-------------|--------|---------------|------|
| skip  |  int   | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir des ensembles de données. Par exemple, Fetch = 10 et Skip = 0 récupère les 10 premières lignes de données, Top = 10 et Skip = 10 récupère les 10 lignes de données suivantes, et ainsi de suite.    |       
| fetch  |  int   | Le nombre de lignes de données à renvoyer dans la requête.    |       
| campaignSetSortColumn  |  string   | Classe les objets de [campagne](#campaign) dans le corps de la réponse par le champ spécifié. La syntaxe est <em>CampaignSetSortColumn = Field</em>, où le paramètre <em>Field</em> peut être l’une des chaînes suivantes :</p><ul><li><strong>id</strong></li><li><strong>createdDateTime</strong></li></ul><p>La valeur par défaut est **createdDateTime**.     |     
| isDescending  |  Boolean   | Trie les objets de [campagne](#campaign) dans le corps de la réponse dans l’ordre décroissant ou croissant.   |         
| storeProductId  |  string   | Utilisez cette valeur pour retourner uniquement les campagnes ad associées à l’application avec l' [ID de magasin](in-app-purchases-and-trials.md#store-ids)spécifié. Un exemple d’ID de magasin pour un produit est 9nblggh42cfd.   |         
| label  |  string   | Utilisez cette valeur pour retourner uniquement les campagnes ad qui incluent l' *étiquette* spécifiée dans l’objet [campagne](#campaign) .    |       |    


### <a name="request-body"></a>Corps de la demande

Les méthodes de publication et de placement requièrent un corps de requête JSON avec les champs requis d’un objet [campagne](#campaign) et les champs supplémentaires que vous souhaitez définir ou modifier.


### <a name="request-examples"></a>Exemples de demande

L’exemple suivant montre comment appeler la méthode de publication pour créer une campagne publicitaire.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign HTTP/1.1
Authorization: Bearer <your access token>

{
    "name": "Contoso App Campaign",
    "storeProductId": "9nblggh42cfd",
    "configuredStatus": "Active",
    "objective": "DriveInstalls",
    "type": "Community"
}
```

L’exemple suivant montre comment appeler la méthode d’obtention pour récupérer une campagne publicitaire spécifique.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/31043481  HTTP/1.1
Authorization: Bearer <your access token>
```

L’exemple suivant montre comment appeler la méthode obtenir pour interroger un ensemble de campagnes de publicité, triées par date de création.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign?storeProductId=9nblggh42cfd&fetch=100&skip=0&campaignSetSortColumn=createdDateTime HTTP/1.1
Authorization: Bearer <your access token>
```


## <a name="response"></a>response

Ces méthodes retournent un corps de réponse JSON avec un ou plusieurs objets [Campaign](#campaign) , en fonction de la méthode que vous avez appelée. L’exemple suivant illustre un corps de réponse pour la méthode d’extraction pour une campagne spécifique.

```json
{
    "Data": {
        "id": 31043481,
        "name": "Contoso App Campaign",
        "createdDate": "2017-01-17T10:12:15Z",
        "storeProductId": "9nblggh42cfd",
        "configuredStatus": "Active",
        "effectiveStatus": "Active",
        "effectiveStatusReasons": [
            "{\"ValidationStatusReasons\":null}"
        ],
        "labels": [],
        "objective": "DriveInstalls",
        "type": "Paid",
        "lines": [
            {
                "id": 31043476,
                "name": "Contoso App Campaign - Paid Line"
            }
        ]
    }
}
```


<span id="campaign"/>

## <a name="campaign-object"></a>Objet de campagne

Les corps de demande et de réponse pour ces méthodes contiennent les champs suivants. Ce tableau indique les champs qui sont en lecture seule (ce qui signifie qu’ils ne peuvent pas être modifiés dans la méthode PUT) et les champs requis dans le corps de la demande de la méthode de publication.

| Champ        | Type   |  Description      |  Lecture seule  | Default  | Requis pour la publication |  
|--------------|--------|---------------|------|-------------|------------|
|  id   |  entier   |  L’ID de la campagne publicitaire.     |   Oui    |      |  Non     |       
|  name   |  string   |   Nom de la campagne publicitaire.    |    Non   |      |  Oui     |       
|  configuredStatus   |  string   |  L’une des valeurs suivantes qui spécifie l’état de la campagne publicitaire spécifiée par le développeur : <ul><li>**Actif**</li><li>**Inactif**</li></ul>     |  Non     |  Actif    |   Oui    |       
|  effectiveStatus   |  string   |   L’une des valeurs suivantes qui spécifie l’État effectif de la campagne publicitaire en fonction de la validation du système : <ul><li>**Actif**</li><li>**Inactif**</li><li>**Traitement en cours**</li></ul>    |    Oui   |      |   Non      |       
|  effectiveStatusReasons   |  tableau   |  Une ou plusieurs des valeurs suivantes qui spécifient la raison de l’État effectif de la campagne publicitaire : <ul><li>**AdCreativesInactive**</li><li>**BillingFailed**</li><li>**AdLinesInactive**</li><li>**ValidationFailed**</li><li>**Échec**</li></ul>      |  Oui     |     |    Non     |       
|  storeProductId   |  string   |  [ID de stockage](in-app-purchases-and-trials.md#store-ids) de l’application à laquelle cette campagne publicitaire est associée. Un exemple d’ID de magasin pour un produit est 9nblggh42cfd.     |   Oui    |      |  Oui     |       
|  étiquettes   |  tableau   |   Une ou plusieurs chaînes qui représentent des légendes personnalisées pour la campagne. Ces étiquettes sont utilisées pour la recherche et le marquage des campagnes.    |   Non    |  null    |    Non     |       
|  type   | string    |  L’une des valeurs suivantes qui spécifient le type de campagne : <ul><li>**Payé**</li><li>**Placés**</li><li>**Communauté**</li></ul>      |   Oui    |      |   Oui    |       
|  objective   |  string   |  L’une des valeurs suivantes qui spécifient l’objectif de la campagne : <ul><li>**DriveInstall**</li><li>**DriveReengagement**</li><li>**DriveInAppPurchase**</li></ul>     |   Non    |  DriveInstall    |   Oui    |       
|  lignes   |  tableau   |   Un ou plusieurs objets qui identifient les [lignes de livraison](manage-delivery-lines-for-ad-campaigns.md#line) associées à la campagne publicitaire. Chaque objet de ce champ se compose d’un champ *ID* et d’un champ *nom* qui spécifie l’ID et le nom de la ligne de remise.     |   Non    |      |    Non     |       
|  createdDate   |  string   |  Date et heure de création de la campagne publicitaire, au format ISO 8601.     |  Oui     |      |     Non    |       |


## <a name="related-topics"></a>Rubriques connexes

* [Exécuter des campagnes Active Directory à l’aide des services Microsoft Store](run-ad-campaigns-using-windows-store-services.md)
* [Gérer les lignes de livraison pour les campagnes de publicité](manage-delivery-lines-for-ad-campaigns.md)
* [Gérer les profils de ciblage pour les campagnes ad](manage-targeting-profiles-for-ad-campaigns.md)
* [Gérer les éléments créatifs pour les campagnes ad](manage-creatives-for-ad-campaigns.md)
* [Obtenir les données relatives aux performances de la campagne publicitaire](get-ad-campaign-performance-data.md)