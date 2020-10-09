---
ms.assetid: dc632a4c-ce48-400b-8e6e-1dddbd13afff
description: Utilisez cette méthode dans l’API Microsoft Store promotions pour gérer les lignes de livraison des campagnes publicitaires promotionnelles.
title: Gérer les lignes de livraison
ms.date: 02/08/2017
ms.topic: article
keywords: API de promotion Windows 10, UWP, Microsoft Store, campagnes ad
ms.localizationpriority: medium
ms.openlocfilehash: e7b370d8eea61033092d833cdf751f1dade86c6d
ms.sourcegitcommit: 5d84d8fe60e83647fa363b710916cf8b92c6e331
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91878562"
---
# <a name="manage-delivery-lines"></a>Gérer les lignes de livraison

Utilisez ces méthodes dans l’API Microsoft Store promotions pour créer une ou plusieurs *lignes de livraison* pour acheter un inventaire et fournir vos annonces pour une campagne publicitaire promotionnelle. Pour chaque ligne de livraison, vous pouvez définir le ciblage, définir le prix de votre offre et déterminer le montant que vous souhaitez dépenser en définissant un budget et en établissant un lien vers les éléments créatifs que vous souhaitez utiliser.

Pour plus d’informations sur la relation entre les lignes de livraison et les campagnes publicitaires, les profils de ciblage et les éléments créatifs, consultez [exécuter des campagnes publicitaires à l’aide des services Microsoft Store](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api).

>**Note** &nbsp; Remarque &nbsp; Avant de pouvoir créer correctement des lignes de livraison pour des campagnes Active Directory à l’aide de cette API, vous devez [d’abord créer une campagne publicitaire payante à l’aide de la page **campagnes publicitaires** dans l’espace partenaires](./index.md). vous devez ajouter au moins un instrument de paiement sur cette page. Après cela, vous serez en mesure de créer avec succès des lignes de livraison facturables pour les campagnes Active Directory à l’aide de cette API. Les campagnes Active Directory que vous créez à l’aide de l’API facturent automatiquement le mode de paiement par défaut choisi dans la page **campagnes publicitaires** de l’espace partenaires.

## <a name="prerequisites"></a>Prérequis

Pour utiliser ces méthodes, vous devez d’abord effectuer les opérations suivantes :

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](run-ad-campaigns-using-windows-store-services.md#prerequisites) pour l’API Microsoft Store promotions.

  > [!NOTE]
  > Dans le cadre des conditions préalables, assurez-vous de [créer au moins une campagne publicitaire payante dans l’espace partenaires](./index.md) et d’ajouter au moins un instrument de paiement pour la campagne publicitaire dans l’espace partenaires. Les lignes de livraison que vous créez à l’aide de cette API facturent automatiquement le mode de paiement par défaut choisi dans la page **campagnes publicitaires** de l’espace partenaires.

* [Obtenez un jeton d’accès Azure ad](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de demande pour ces méthodes. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête

Ces méthodes ont les URI suivants.

| Type de méthode | URI de requête         |  Description  |
|--------|---------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line``` |  Crée une nouvelle ligne de remise.  |
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/{lineId}``` |  Modifie la ligne de remise spécifiée par *lineId*.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/{lineId}``` |  Obtient la ligne de remise spécifiée par *lineId*.  |


### <a name="header"></a>En-tête

| En-tête        | Type   | Description         |
|---------------|--------|---------------------|
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |
| ID de suivi   | GUID   | facultatif. ID qui effectue le suivi du workflow d’appel.                                  |


### <a name="request-body"></a>Corps de la demande

Les méthodes de publication et de placement requièrent un corps de requête JSON avec les champs requis d’un objet de [ligne de remise](#line) et tous les champs supplémentaires que vous souhaitez définir ou modifier.


### <a name="request-examples"></a>Exemples de demande

L’exemple suivant montre comment appeler la méthode de publication pour créer une ligne de remise.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/line HTTP/1.1
Authorization: Bearer <your access token>

{
    "name": "Contoso App Campaign - Paid Line",
    "configuredStatus": "Active",
    "startDateTime": "2017-01-19T12:09:34Z",
    "endDateTime": "2017-01-31T23:59:59Z",
    "bidAmount": 0.4,
    "dailyBudget": 20,
    "targetProfileId": {
        "id": 310021746
    },
    "creatives": [
        {
            "id": 106851
        }
    ],
    "campaignId": 31043481,
    "minMinutesPerImp ": 1
}
```

L’exemple suivant montre comment appeler la méthode d’obtention pour récupérer une ligne de remise.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/31019990  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>response

Ces méthodes retournent un corps de réponse JSON avec un objet de [ligne de remise](#line) qui contient des informations sur la ligne de remise qui a été créée, mise à jour ou récupérée. L’exemple suivant illustre un corps de réponse pour ces méthodes.

```json
{
    "Data": {
        "id": 31043476,
        "name": "Contoso App Campaign - Paid Line",
        "configuredStatus": "Active",
        "effectiveStatus": "Active",
        "effectiveStatusReasons": [
            "{\"ValidationStatusReasons\":null}"
        ],
        "startDateTime": "2017-01-19T12:09:34Z",
        "endDateTime": "2017-01-31T23:59:59Z",
        "createdDateTime": "2017-01-17T10:28:34Z",
        "bidType": "CPM",
        "bidAmount": 0.4,
        "dailyBudget": 20,
        "targetProfileId": {
            "id": 310021746
        },
        "creatives": [
            {
                "id": 106126
            }
        ],
        "campaignId": 31043481,
        "minMinutesPerImp ": 1,
        "pacingType ": "SpendEvenly",
        "currencyId ": 732
    }
}

```


<span id="line"/>

## <a name="delivery-line-object"></a>Objet de ligne de remise

Les corps de demande et de réponse pour ces méthodes contiennent les champs suivants. Ce tableau indique les champs qui sont en lecture seule (ce qui signifie qu’ils ne peuvent pas être modifiés dans la méthode PUT) et les champs requis dans le corps de la demande pour les méthodes POSTÉRIEURes ou PUT.

| Champ        | Type   |  Description      |  Lecture seule  | Default  | Requis pour la publication/la mise en place |   
|--------------|--------|---------------|------|-------------|------------|
|  id   |  entier   |  ID de la ligne de remise.     |   Oui    |      |  Non      |    
|  name   |  string   |   Nom de la ligne de remise.    |    Non   |      |  POST     |     
|  configuredStatus   |  string   |  L’une des valeurs suivantes qui spécifie l’état de la ligne de remise spécifiée par le développeur : <ul><li>**Actif**</li><li>**Inactif**</li></ul>     |  Non     |      |   POST    |       
|  effectiveStatus   |  string   |   L’une des valeurs suivantes qui spécifie l’État effectif de la ligne de livraison en fonction de la validation du système : <ul><li>**Actif**</li><li>**Inactif**</li><li>**Traitement en cours**</li><li>**Échec**</li></ul>    |    Oui   |      |  Non      |      
|  effectiveStatusReasons   |  tableau   |  Une ou plusieurs des valeurs suivantes qui spécifient la raison de l’État effectif de la ligne de livraison : <ul><li>**AdCreativesInactive**</li><li>**ValidationFailed**</li></ul>      |  Oui     |     |    Non    |           
|  startDatetime   |  string   |  Date et heure de début de la ligne de remise, au format ISO 8601. Cette valeur ne peut pas être modifiée si elle est déjà passée.     |    Non   |      |    APRÈS, PUT     |
|  endDatetime   |  string   |  Date et heure de fin de la ligne de remise, au format ISO 8601. Cette valeur ne peut pas être modifiée si elle est déjà passée.     |   Non    |      |  APRÈS, PUT     |
|  createdDatetime   |  string   |  Date et heure de création de la ligne de remise, au format ISO 8601.      |    Oui   |      |  Non      |
|  bidType   |  string   |  Valeur qui spécifie le type d’enchère de la ligne de remise. Actuellement, la seule valeur prise en charge est **CPM**.      |    Non   |  CPM    |  Non     |
|  bidAmount   |  Décimal   |  Montant de l’enchère à utiliser pour faire l’enchère d’une demande de publicité.      |    Non   |  Valeur CPM moyenne basée sur les marchés cibles (cette valeur est révisée régulièrement).    |    Non    |  
|  dailyBudget   |  Décimal   |  Budget journalier pour la ligne de livraison. *DailyBudget* ou *lifetimeBudget* doit être défini.      |    Non   |      |   APRÈS, PUT (si *lifetimeBudget* n’est pas défini)       |
|  lifetimeBudget   |  Décimal   |   Budget à long terme de la ligne de livraison. LifetimeBudget * ou *dailyBudget* doit être défini.      |    Non   |      |   APRÈS, PUT (si *dailyBudget* n’est pas défini)    |
|  targetingProfileId   |  object   |  Sur l’objet qui identifie le [profil de ciblage](manage-targeting-profiles-for-ad-campaigns.md#targeting-profile) qui décrit les utilisateurs, les zones géographiques et les types d’inventaire que vous souhaitez cibler pour cette ligne de livraison. Cet objet est constitué d’un champ d' *ID* unique qui spécifie l’ID du profil de ciblage.     |    Non   |      |  Non      |  
|  éléments créatifs   |  tableau   |  Un ou plusieurs objets qui représentent les [éléments créatifs](manage-creatives-for-ad-campaigns.md#creative) associés à la ligne de livraison. Chaque objet de ce champ est constitué d’un champ d' *ID* unique qui spécifie l’ID d’un élément créatif.      |    Non   |      |   Non     |  
|  campaignId   |  entier   |  ID de la campagne parent Active Directory.      |    Non   |      |   Non     |  
|  minMinutesPerImp   |  entier   |  Spécifie l’intervalle de temps minimal (en minutes) entre deux impressions présentées au même utilisateur à partir de cette ligne de livraison.      |    Non   |  4000    |  Non      |  
|  pacingType   |  string   |  L’une des valeurs suivantes qui spécifient le type de rythme : <ul><li>**SpendEvenly**</li><li>**SpendAsFastAsPossible**</li></ul>      |    Non   |  SpendEvenly    |  Non      |
|  currencyId   |  entier   |  ID de la devise de la campagne.      |    Oui   |  La devise du compte de développeur (vous n’avez pas besoin de spécifier ce champ dans les appels de publication ou de placement)    |   Non     |      |


## <a name="related-topics"></a>Rubriques connexes

* [Exécuter des campagnes Active Directory à l’aide des services Microsoft Store](run-ad-campaigns-using-windows-store-services.md)
* [Gérer les campagnes publicitaires](manage-ad-campaigns.md)
* [Gérer les profils de ciblage pour les campagnes ad](manage-targeting-profiles-for-ad-campaigns.md)
* [Gérer les éléments créatifs pour les campagnes ad](manage-creatives-for-ad-campaigns.md)
* [Obtenir les données relatives aux performances de la campagne publicitaire](get-ad-campaign-performance-data.md)