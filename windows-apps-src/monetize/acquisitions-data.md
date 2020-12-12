---
description: Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir des données d’acquisition agrégées au format JSON pour les applications UWP et Xbox One qui ont été reçues via le portail des développeurs Xbox (XDP) et disponibles dans le tableau de bord XDP Analytics.
title: Obtenir des données d’acquisitions pour vos applications et vos jeux
ms.date: 03/06/2019
ms.topic: article
keywords: Windows 10, UWP, réseau de publicité, métadonnées d’application
ms.localizationpriority: medium
ms.openlocfilehash: 371f855e644a990191cd8f8b9365553bc2df9693
ms.sourcegitcommit: 368753aea2792984857f6a57a22daed1035f1a33
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2020
ms.locfileid: "97349703"
---
# <a name="get-acquisitions-data-for-your-games-and-apps"></a>Obtenir des données d’acquisitions pour vos applications et vos jeux 
Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir des données d’acquisition agrégées au format JSON pour les applications UWP et Xbox One qui ont été reçues via le portail des développeurs Xbox (XDP) et disponibles dans le tableau de bord XDP Analytics. 

> [!NOTE]
> Cette API ne fournit pas de données agrégées quotidiennement avant le 1er octobre 2016. 

## <a name="prerequisites"></a>Prérequis
Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes : 

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md) pour l’API Microsoft Store Analytics. 
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md) à utiliser dans l’en-tête de requête de cette méthode. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau. 

## <a name="request"></a>Requête
### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de demande |
| --- | --- |
| GET | `https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions` |

### <a name="request-header"></a>En-tête de requête

| En-tête | Type | Description |
| --- | --- | --- |
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** `<token>` . |

### <a name="request-parameters"></a>Paramètres de la demande

| Paramètre | Type | Description | Obligatoire |
| --- | --- | --- | --- |
| applicationId | string | ID de produit du jeu Xbox One pour lequel vous récupérez des données d’acquisition. Pour obtenir l’ID de produit de votre jeu, accédez à votre jeu dans le programme d’analyse XDP et récupérez l’ID de produit à partir de l’URL. Si vous téléchargez vos données acquisitions à partir du rapport de l’analyse de l’espace partenaires, l’ID du produit est également inclus dans le fichier. TSV.  | Oui |
| startDate | Date | Dans la plage de dates, la date de début de la récupération des données d’acquisition. La valeur par défaut est la date actuelle.  | Non |
| endDate | Date | Dans la plage de dates, la date de fin de la récupération des données d’acquisition. La valeur par défaut est la date actuelle.  | Non |
| filter | string | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction contient un nom de champ du corps de la réponse et une valeur qui sont associés aux **opérateurs** **EQ** ou ne, et les instructions peuvent être combinées à l’aide **de and** ou **de ou de**. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre filter. Par exemple, *filtre = l’égalisation du marché « États-Unis » et « EQ de sexe*».  <br/> Vous pouvez spécifier les champs suivants dans le corps de la réponse : <ul><li>**acquisitionType**</li><li>**vieillissement**</li><li>**storeClient**</li><li>**gender**</li><li>**négoci**</li><li>**osVersion**</li><li>**deviceType**</li><li>**sandboxId**</li></ul> | Non |
| aggregationLevel | string | Indique la plage de temps pendant laquelle récupérer les données agrégées. Il peut s’agit des chaînes suivantes : **day**, **week** ou **month**. Par défaut, la valeur est **day**.  | Non |
| orderby | string | Une instruction qui commande les valeurs de données de résultats pour chaque acquisition. La syntaxe est *orderby = Field [Order], champ [Order],...* Le paramètre *Field* peut être l’une des chaînes suivantes : <ul><li>**date**</li><li>**acquisitionType**</li><li>**vieillissement**</li><li>**storeClient**</li><li>**gender**</li><li>**négoci**</li><li>**osVersion**</li><li>**deviceType**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> Le paramètre *order*, facultatif, peut comporter les valeurs **asc** ou **desc** afin de spécifier l’ordre croissant ou décroissant pour chaque champ. La valeur par défaut est **ASC**. Voici un exemple de chaîne *orderby* : *orderby = date, Market*  | Non |
| groupby | string | Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants : <ul><li>**date**</li><li>**applicationName**</li><li>**acquisitionType**</li><li>**ageGroup**</li><li>**storeClient**</li><li>**gender**</li><li>**négoci**</li><li>**osVersion**</li><li>**deviceType**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> Les lignes de données renvoyées comportent les champs spécifiés dans le paramètre *groupby*, ainsi que dans les paramètres suivants : <ul><li>**date**</li><li>**applicationId**</li><li>**acquisitionQuantity**</li></ul> Le paramètre *groupby* peut être utilisé avec le paramètre aggregationLevel. Par exemple : *&GroupBy = ageGroup, market&aggregationLevel = week*  | Non |

### <a name="request-example"></a>Exemple de requête
L’exemple suivant illustre plusieurs demandes d’obtention de données d’acquisition de jeu Xbox One. Remplacez la valeur *ApplicationID* par l’ID de produit de votre jeu.  

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions?applicationId=9WZDNCRFHXHT&startDate=1/1/2017&endDate=2/1/2019&top=10&skip=0 HTTP/1.1 
Authorization: Bearer <your access token> 

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions?applicationId=9WZDNCRFHXHT&startDate=1/1/2017&endDate=2/1/2019&skip=0&filter=market eq 'US' and gender eq 'm' HTTP/1.1 
Authorization: Bearer <your access token> 
```

## <a name="response"></a>response

### <a name="response-body"></a>Response body
| Valeur | Type | Description |
| --- | --- | --- |
| Valeur | tableau | Tableau d’objets qui contiennent les données d’acquisition d’agrégat pour le jeu. Pour plus d’informations sur les données de chaque objet, consultez la section [Valeurs d’acquisition](#acquisition-values) ci-dessous. |
| TotalCount | entier | Nombre total de lignes dans les résultats de la requête. |

### <a name="acquisition-values"></a>Valeurs d’acquisition 
Les éléments du tableau *Value* comportent les valeurs suivantes : 

| Valeur | Type | Description |
| --- | --- | --- |
| Date | string | Première date dans la plage de dates des données d’acquisition. Si la requête spécifiait un jour précis, cette valeur correspond à la date. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| applicationId | string | ID de produit du jeu Xbox One pour lequel vous récupérez des données d’acquisition. |
| applicationName | string | Nom complet du jeu. |
| acquisitionType | string | Une des chaînes suivantes qui indique le type d’acquisition :  <ul><li>**Gratuit**</li><li>**Version d’évaluation**</li><li>**Payant**</li><li>**Code promotionnel**</li><li>**Acquisitionquantity**</li><li>**IAP d’abonnement**</li><li>**Public privé**</li><li>**Ordre préalable**</li><li>**Réussite du jeu Xbox** (ou **Pass** si vous interrogez des données avant le 23 mars 2018)</li><li>**Disque**</li><li>**Code prépayé**</li><li>**Ordre préalable facturé**</li><li>**Ordre préalable annulé**</li><li>**Échec préalable de l’ordre**</li></ul> |
| age | string | L’une des chaînes suivantes qui indique le groupe d’âge de l’utilisateur qui a effectué l’acquisition : <ul><li>**Inférieur à 13**</li><li>**13-17**</li><li>**18-24**</li><li>**25-34**</li><li>**35-44**</li><li>**44-55**</li><li>**Supérieur à 55**</li><li>**Unknown**</li></ul> |
| deviceType | string | L’une des chaînes suivantes qui spécifie le type de périphérique qui a effectué l’acquisition : <ul><li>**ORDINATEURS**</li><li>**Téléphone**</li><li>**Console-Xbox One**</li><li>**Console-série Xbox X**</li><li>**IoT**</li><li>**Serveur**</li><li>**Tablette**</li><li>**Hologrammes**</li><li>**Unknown**</li></ul> |
| gender | string | L’une des chaînes suivantes qui spécifie le sexe de l’utilisateur qui a effectué l’acquisition : <ul><li>**m**</li><li>**f**</li><li>**Unknown**</li></ul> |
| market | string | Le code pays ISO 3166 du marché dans lequel l’acquisition s’est produite. |
| osVersion | string | La version de système d’exploitation sur laquelle l’acquisition s’est produite. Pour cette méthode, cette valeur est toujours **Windows 10**. |
| paymentInstrumentType | string | L’une des chaînes suivantes qui indique l’instruction de paiement utilisée pour l’acquisition : <ul><li>**Carte de crédit**</li><li>**Carte de débit direct**</li><li>**Achat déduit**</li><li>**Solde MS**</li><li>**Opérateur mobile**</li><li>**Virement bancaire en ligne**</li><li>**PayPal**</li><li>**Fractionner la transaction**</li><li>**Échange de jetons**</li><li>**Montant nul payé**</li><li>**eWallet**</li><li>**Unknown**</li></ul> |
| sandboxId | string | ID du bac à sable (sandbox) créé pour le jeu. Il peut s’agir de la valeur **Retail** ou d’un ID sandbox privé. |
| storeClient | string | L’une des chaînes suivantes qui indique la version du magasin dans lequel l’acquisition s’est produite : <ul><li>**Windows Phone Store (client)**</li><li>**Microsoft Store (client)** (ou **Windows Store (client)** si vous interrogez des données avant le 23 mars 2018) </li><li>**Microsoft Store (Web)** (ou **Windows Store (Web)** si vous interrogez des données avant le 23 mars 2018) </li><li>**Volume purchase by organizations**</li><li>**Autres**</li></ul> |
| xboxTitleId | string | L’ID de titre Xbox Live (représenté par la valeur hexadécimale) attribué par le portail des développeurs Xbox (XDP) pour les jeux Xbox Live. |
| acquisitionQuantity | nombre | Le nombre d’acquisitions qui se sont produites durant le niveau d’agrégation spécifié. |
| purchasePriceUSDAmount | nombre | Montant payé par le client pour l’acquisition, converti en USD, à l’aide du taux de change mensuel. |
| purchaseTaxUSDAmount | nombre | Montant de la taxe appliquée à l’acquisition, converti en USD. |
| localCurrencyCode | string | Code devise locale basé sur le pays du compte de l’espace partenaires.  |
| xboxProductId | string | ID de produit Xbox du produit de XDP, le cas échéant.  |
| availabilityId | string | ID de disponibilité du produit de XDP, le cas échéant.  |
| skuId | string | ID de référence du produit de XDP, le cas échéant.  |
| skuDisplayName  | string | Nom complet de la référence du produit de XDP, le cas échéant.  |
| xboxParentProductId | string | ID de produit parent Xbox du produit de XDP, le cas échéant.  |
| parentProductName | string | Nom du produit parent du produit de XDP, le cas échéant.  |
| productTypeName | string | Nom du type de produit du produit de XDP, le cas échéant.  |
| purchaseTaxType | string | Achat du type de taxe du produit à partir de XDP, le cas échéant.  |
| purchasePriceLocalAmount | nombre | Prix d’achat montant local du produit à partir de XDP, le cas échéant.  |
| purchaseTaxLocalAmount | nombre | Achetez le montant local de taxes du produit à partir de XDP, le cas échéant.  |

### <a name="response-example"></a>Exemple de réponse
L’exemple suivant représente un corps de réponse JSON pour cette requête. 

```JSON
{ 
    "Value": [ 
        { 
            "date": "2019-01-15T01:00:00.0000000Z", 
            "applicationId": "9WZDNCRFHXHT", 
            "applicationName": null, 
            "acquisitionType": "Paid", 
            "age": null, 
            "deviceType": "Phone", 
            "gender": null, 
            "market": "US", 
            "osVersion": "Windows 10", 
            "paymentInstrumentType": null, 
            "sandboxId": "RETAIL", 
            "storeClient": "Microsoft Store (client)", 
            "xboxTitleId": null, 
            "localCurrencyCode": "USD", 
            "xboxProductId": null, 
            "availabilityId": "B42LRTSZ2MCJ", 
            "skuId": "0010", 
            "skuDisplayName": null, 
            "xboxParentProductId": null, 
            "parentProductName": null, 
            "productTypeName": "Game", 
            "purchaseTaxType": "TaxesNotIncluded", 
            "acquisitionQuantity": 1, 
            "purchasePriceUSDAmount": 3.08, 
            "purchasePriceLocalAmount": 3.08, 
            "purchaseTaxUSDAmount": 0.09, 
            "purchaseTaxLocalAmount": 0.09 
        } 
    ], 

    "@nextLink": null,
    
    "TotalCount": 12221 
} 
```

 
