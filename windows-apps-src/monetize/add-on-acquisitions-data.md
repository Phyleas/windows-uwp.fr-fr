---
description: Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir des données d’acquisition d’agrégats d’agrégats au format JSON pour les applications UWP et Xbox One qui ont été reçues par le biais du portail des développeurs Xbox (XDP) et disponibles dans le tableau de bord de l’espace partenaires de XDP Analytics.
title: Obtenir des données d’acquisitions d’extension pour vos applications et vos jeux
ms.date: 03/06/2019
ms.topic: article
keywords: Windows 10, UWP, réseau de publicité, métadonnées d’application
ms.localizationpriority: medium
ms.openlocfilehash: 3e1349582515ce66232ea8266efc588610faae98
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493564"
---
# <a name="get-add-on-acquisitions-data-for-your-games-and-apps"></a>Obtenir des données d’acquisitions d’extension pour vos applications et vos jeux 
Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir des données d’acquisition d’agrégats d’agrégats au format JSON pour les applications UWP et Xbox One qui ont été reçues par le biais du portail des développeurs Xbox (XDP) et disponibles dans le tableau de bord de l’espace partenaires de XDP Analytics. 

## <a name="prerequisites"></a>Prérequis
Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes : 

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md) pour l’API Microsoft Store Analytics. 
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md) à utiliser dans l’en-tête de requête de cette méthode. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau. 

> [!NOTE]
> Cette API ne fournit pas de données agrégées quotidiennement avant le 1er octobre 2016. 

## <a name="request"></a>Requête

### <a name="request-syntax"></a>Syntaxe de la requête
| Méthode | URI de demande |
| --- | --- | 
| GET | `https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions` |

### <a name="request-header"></a>En-tête de requête 
| En-tête | Type | Description | 
| --- | --- | --- |
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** `<token>` . |

### <a name="request-parameters"></a>Paramètres de la demande
Le paramètre *ApplicationID* ou *addonProductId* est obligatoire. Pour récupérer les données d’acquisition de toutes les extensions inscrites dans l’application, spécifiez le paramètre *applicationId*. Pour récupérer les données d’acquisition d’un module complémentaire unique, spécifiez le paramètre *addonProductId* . Si vous spécifiez les deux valeurs, le paramètre *applicationId* est ignoré. 

| Paramètre | Type | Description | Obligatoire | 
| --- | --- | --- | --- |
| applicationId | string | *ProductID* du jeu Xbox One pour lequel vous récupérez des données d’acquisition. Pour obtenir le *ProductID* de votre jeu, accédez à votre jeu dans le programme d’analyse XDP et récupérez le *ProductID* à partir de l’URL. Si vous téléchargez vos données acquisitions à partir du rapport de l’analyse de l’espace partenaires, le *ProductID* est également inclus dans le fichier. TSV. | Oui |
| addonProductId | string | *ProductID* du module complémentaire pour lequel vous souhaitez récupérer les données d’acquisition. | Oui |
| startDate | Date | Dans la plage de dates, date de début de la récupération des données d’acquisition. La valeur par défaut est la date actuelle. | Non |
| endDate | Date | Dans la plage de dates, date de fin de la récupération des données d’acquisition. La valeur par défaut est la date actuelle. | Non |
| top | int | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. | Non |
| skip | int | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite. | Non |
| Filter | string | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction contient un nom de champ du corps de la réponse et une valeur qui sont associés aux opérateurs EQ ou ne, et les instructions peuvent être combinées à l’aide de and ou de ou de. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre filter. Par exemple, filtre = l’égalisation du marché « États-Unis » et « EQ de sexe ». <br/> Vous pouvez spécifier les champs suivants dans le corps de la réponse : <ul><li>**acquisitionType**</li><li>**vieillissement**</li><li>**storeClient**</li><li>**gender**</li><li>**négoci**</li><li>**osVersion**</li><li>**deviceType**</li><li>**sandboxId**</li></ul> | Non |
| aggregationLevel | string | Indique la plage de temps pendant laquelle récupérer les données agrégées. Il peut s’agit des chaînes suivantes : **day**, **week** ou **month**. Par défaut, la valeur est **day**. | Non |
| orderby | string | Instruction qui commande les valeurs de données de résultats pour chaque acquisition d’extension. La syntaxe est *orderby = Field [Order], champ [Order],...* Le paramètre *Field* peut être l’une des chaînes suivantes : <ul><li>**date**</li><li>**acquisitionType**</li><li>**vieillissement**</li><li>**storeClient**</li><li>**gender**</li><li>**négoci**</li><li>**osVersion**</li><li>**deviceType**</li><li>**orderName**</li></ul> Le paramètre order, facultatif, peut comporter les valeurs **asc** ou **desc** afin de spécifier l’ordre croissant ou décroissant pour chaque champ. La valeur par défaut est **ASC**. <br/> Voici un exemple de chaîne *orderby* : *orderby = date, Market* | Non |
| groupby | string | Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants : <ul><li>**date**</li><li>**applicationName**</li><li>**addonProductName**</li> <li>**acquisitionType**</li><li>**vieillissement**</li> <li>**storeClient**</li><li>**gender**</li> <li>**négoci**</li> <li>**osVersion**</li><li>**deviceType**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> Les lignes de données renvoyées comportent les champs spécifiés dans le paramètre *groupby*, ainsi que dans les paramètres suivants : <ul><li>**date**</li><li>**applicationId**</li><li>**addonProductId**</li><li>**acquisitionQuantity**</li></ul> Le paramètre groupby peut être utilisé avec le paramètre *aggregationLevel*. Par exemple : *&GroupBy = Age, market&aggregationLevel = week* | Non |

### <a name="request-example"></a>Exemple de requête
L’exemple suivant illustre quelques requêtes d’obtention de données d’acquisition d’extensions. Remplacez les valeurs *addonProductId* et *APPLICATIONID* par l’ID de magasin approprié pour votre module complémentaire ou votre application. 

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions?applicationId=9WZDNCRFJ314&startDate=1/1/2015&endDate=2/1/2015&skip=0 HTTP/1.1 

Authorization: Bearer <your access token> 

 

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions?applicationId=9WZDNCRFJ314&startDate=1/1/2015&endDate=2/1/2015&skip=0&filter=market eq 'GB' and gender eq 'm' HTTP/1.1 

Authorization: Bearer <your access token>
```

## <a name="response"></a>response

### <a name="response-body"></a>Response body

| Valeur | Type | Description |
| --- | --- | --- |
| Valeur | tableau | Tableau d’objets contenant des données d’acquisition agrégées d’extensions. Pour plus d’informations sur les données incluses dans chaque objet, voir [Valeurs d’acquisition d’extensions](#add-on-acquisition-values) ci-dessous. |
| @nextLink | string | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête a la valeur 10000, mais qu’il existe plus de 10 000 lignes de données d’acquisition d’extensions pour la demande. |
| TotalCount | int | Nombre total de lignes dans les résultats de la requête. |

### <a name="add-on-acquisition-values"></a>Valeurs d’acquisition d’extensions
Les éléments du tableau Value comportent les valeurs suivantes :

| Valeur | Type | Description | 
| --- | --- | --- |
| Date | string | Première date dans la plage de dates des données d’acquisition. Si la requête spécifiait un jour précis, cette valeur correspond à la date. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| addonProductId | string | *ProductID* du module complémentaire pour lequel vous extrayez des données d’acquisition. |
| addonProductName | string | Nom d’affichage de l’extension. Cette valeur s’affiche uniquement dans les données de la réponse si le paramètre *aggregationLevel* est défini sur **Day**, sauf si vous spécifiez le champ **addonProductName** dans le paramètre *GroupBy* . |
| applicationId | string | *ProductID* de l’application pour laquelle vous souhaitez récupérer les données d’acquisition de module complémentaire. |
| applicationName | string | Nom complet du jeu. |
| deviceType | string | L’une des chaînes suivantes qui spécifie le type de périphérique qui a effectué l’acquisition : <ul><li>ORDINATEURS</li><li>Numéros</li><li>« Console-Xbox One »</li><li>« Console-Xbox X »</li><li>IOT</li><li>Serveurs</li><li>Tablette</li><li>Holographique</li><li>Connue</li></ul> |
| storeClient | string | L’une des chaînes suivantes qui indique la version du magasin dans lequel l’acquisition s’est produite : <ul><li>« Magasin de Windows Phone (client) »</li><li>« Microsoft Store (client) » (ou « Windows Store (client) » si vous interrogez des données avant le 23 mars 2018)</li><li>« Microsoft Store (Web) » (ou « Windows Store (Web) » si vous interrogez des données avant le 23 mars 2018)</li><li>« Achat en volume par les organisations »</li><li>Autres</li></ul> |
| osVersion | string | La version de système d’exploitation sur laquelle l’acquisition s’est produite. Pour cette méthode, cette valeur est toujours « Windows 10 ». |
| market | string | Le code pays ISO 3166 du marché dans lequel l’acquisition s’est produite. |
| gender | string | L’une des chaînes suivantes qui spécifie le sexe de l’utilisateur qui a effectué l’acquisition : <ul><li>"m"</li><li>"f"</li><li>Connue</li></ul> |
| age | string | L’une des chaînes suivantes qui indique le groupe d’âge de l’utilisateur qui a effectué l’acquisition : <ul><li>« inférieur à 13 »</li><li>« 13-17 »</li><li>« 18-24 »</li><li>« 25-34 »</li><li>« 35-44 »</li><li>« 44-55 »</li><li>« supérieur à 55 »</li><li>Connue</li></ul> |
| acquisitionType | string | Une des chaînes suivantes qui indique le type d’acquisition : <ul><li>Gratuit </li><li>Evaluation </li><li>Litige</li><li>« Code promotionnel » </li><li>Acquisitionquantity</li><li>"IAP d’abonnement"</li><li>« Public privé »</li><li>« Pré-ordre »</li><li>« Réussite du jeu Xbox » (ou « passe de jeu » si vous interrogez des données avant le 23 mars 2018)</li><li>Libérer</li><li>« Code prépayé »</li><li>« Pré-ordre facturé »</li><li>« Pré-ordre annulé »</li><li>« Préordre en échec »</li></ul> |
| acquisitionQuantity | entier | Le nombre d’acquisitions qui se sont produites. |
| inAppProductId | string | ID de produit du produit sur lequel ce module complémentaire est utilisé.  |
| inAppProductName | string | Nom de produit du produit sur lequel ce module complémentaire est utilisé.  |
| paymentInstrumentType | string | Type d’instrument de paiement utilisé pour l’acquisition.  |
| sandboxId | string | ID du bac à sable (sandbox) créé pour le jeu. Il peut s’agir de la valeur **Retail** ou d’un ID sandbox privé.  |
| xboxTitleId | string | ID du titre Xbox du produit de XDP, le cas échéant.  |
| localCurrencyCode | string | Code devise locale basé sur le pays du compte de l’espace partenaires.  |
| xboxProductId | string | ID de produit Xbox du produit de XDP, le cas échéant.  |
| availabilityId | string | ID de disponibilité du produit de XDP, le cas échéant.  |
| skuId | string | ID de référence du produit de XDP, le cas échéant.  |
| skuDisplayName | string | Nom complet de la référence du produit de XDP, le cas échéant.  |
| xboxParentProductId | string | ID de produit parent Xbox du produit de XDP, le cas échéant.  |
| parentProductName | string | Nom du produit parent du produit de XDP, le cas échéant.  |
| productTypeName | string | Nom du type de produit du produit de XDP, le cas échéant.  |
| purchaseTaxType | string | Achat du type de taxe du produit à partir de XDP, le cas échéant.  |
| purchasePriceUSDAmount | nombre | Montant payé par le client pour le module complémentaire, converti en USD.  |
| purchasePriceLocalAmount | nombre | Montant des taxes appliquées au module complémentaire.  |
| purchaseTaxUSDAmount | nombre | Montant des taxes appliquées au module complémentaire, converti en USD.  |
| purchaseTaxLocalAmount | nombre | Achetez le montant local de taxes du produit à partir de XDP, le cas échéant.  |

### <a name="response-example"></a>Exemple de réponse
L’exemple suivant représente un corps de réponse JSON pour cette requête. 

```JSON
{ 
  "Value": [ 
    { 
            "inAppProductId": "9NBLGGH1864K", 
            "inAppProductName": "866879", 
            "addonProductId": "9NBLGGH1864K", 
            "addonProductName": "866879", 
            "date": "2017-11-05", 
            "applicationId": "9WZDNCRFJ314", 
            "applicationName": "Tetris Blitz", 
            "acquisitionType": "Iap", 
            "age": "35-49", 
            "deviceType": "Phone", 
            "gender": "m", 
            "market": "US", 
            "osVersion": "Windows Phone 8.1", 
            "paymentInstrumentType": "Credit Card", 
            "sandboxId": "RETAIL", 
            "storeClient": "Windows Phone Store (client)", 
            "xboxTitleId": "", 
            "localCurrencyCode": "USD", 
            "xboxProductId": "00000000-0000-0000-0000-000000000000", 
            "availabilityId": "", 
            "skuId": "", 
            "skuDisplayName": "Full", 
            "xboxParentProductId": "", 
            "parentProductName": "Tetris Blitz", 
            "productTypeName": "Add-On", 
            "purchaseTaxType": "", 
            "acquisitionQuantity": 1, 
            "purchasePriceUSDAmount": 1.08, 
            "purchasePriceLocalAmount": 0.09, 
            "purchaseTaxUSDAmount": 1.08, 
            "purchaseTaxLocalAmount": 0.09 
        } 
    ], 

    "@nextLink": null, 
    
    "TotalCount": 7601 
} 
```