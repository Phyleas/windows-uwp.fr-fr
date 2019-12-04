---
description: Utilisez cet URI REST pour obtenir des données de détails de bloc pour une application de bureau pendant une plage de dates donnée et d’autres filtres facultatifs.
title: Obtenir les détails du bloc de mise à niveau pour votre application
ms.date: 07/11/2018
ms.topic: article
keywords: Windows 10, blocs d’applications de bureau, programme d’application de bureau Windows
localizationpriority: medium
ms.openlocfilehash: b30594ff3943d1ed9183190a0b5a43824816c080
ms.sourcegitcommit: ae9c1646398bb5a4a888437628eca09ae06e6076
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74735114"
---
# <a name="get-upgrade-block-details-for-your-desktop-application"></a>Obtenir des informations concernant les blocs de mise à niveau pour votre application de bureau

Utilisez cet URI REST pour obtenir des détails sur les appareils Windows 10 sur lesquels un fichier exécutable spécifique dans votre application de bureau bloque l’exécution d’une mise à niveau vers Windows 10. Vous pouvez utiliser cet URI uniquement pour les applications de bureau que vous avez ajoutées au [programme d’application de bureau Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program). Ces informations sont également disponibles dans le [rapport des blocs d’application](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#application-blocks-report) pour les applications de bureau dans l’espace partenaires.

Cet URI est similaire à [obtenir les blocs de mise à niveau pour votre application de bureau](get-desktop-block-data.md), mais il retourne les informations de bloc de périphérique pour un exécutable spécifique dans votre application de bureau.

## <a name="prerequisites"></a>Conditions préalables


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de requête       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockdetails```|


### <a name="request-header"></a>En-tête de requête

| En-tête        | Tapez   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Azure AD jeton d’accès au format **porteur** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Tapez   |  Description      |  Obligatoire  
|---------------|--------|---------------|------|
| applicationId | chaîne | ID de produit de l’application de bureau pour laquelle vous souhaitez récupérer des données de bloc. Pour obtenir l’ID de produit d’une application de bureau, ouvrez un [rapport analytique pour votre application de bureau dans l’espace partenaires](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program) (par exemple, le rapport sur les **blocs**) et récupérez l’ID de produit de l’URL. |  Oui  |
| fileName | chaîne | Nom de l’exécutable bloqué |
| startDate | date | Date de début dans la plage de dates des données de bloc à récupérer. La valeur par défaut est de 90 jours avant la date actuelle. |  non  |
| endDate | date | Date de fin dans la plage de dates des données de bloc à récupérer. La valeur par défaut est la date du jour. |  non  |
| top | entier | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  non  |
| skip | entier | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite. |  non  |
| filter | chaîne  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ Nom dans le corps de la réponse et une valeur, qui sont associés aux opérateurs **eq** ou **ne**, et les instructions peuvent être combinées à l’aide des opérateurs **and** ou **or**. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*. Vous pouvez spécifier les champs suivants dans le corps de réponse :<p/><ul><li><strong>applicationVersion</strong></li><li><strong>SOA</strong></li><li><strong>blockType</strong></li><li><strong>deviceType</strong></li><li><strong>négoci</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>productName</strong></li><li><strong>targetOs</strong></li></ul> | non   |
| orderby | chaîne | Instruction qui classe les valeurs des données de résultat pour chaque bloc. La syntaxe est la suivante <em>orderby=field [order],field [order],...</em>. Le paramètre <em>champ</em> peut être l'une des champs suivants du corps de réponse :<p/><ul><li><strong>applicationVersion</strong></li><li><strong>SOA</strong></li><li><strong>blockType</strong></li><li><strong>date</strong><li><strong>deviceType</strong></li><li><strong>négoci</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>productName</strong></li><li><strong>targetOs</strong></li><li><strong>deviceCount</strong></li></ul><p>Le paramètre facultatif <em>order</em> peut avoir la valeur <strong>asc</strong> ou <strong>desc</strong> pour spécifier l’ordre croissant ou décroissant de chaque champ. La valeur par défaut est <strong>asc</strong>.</p><p>Voici un exemple de chaîne <em>orderby</em> : <em>orderby=date,market</em></p> |  non  |
| groupby | chaîne | Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants dans le corps de réponse :<p/><ul><li><strong>applicationVersion</strong></li><li><strong>SOA</strong></li><li><strong>blockType</strong></li><li><strong>deviceType</strong></li><li><strong>négoci</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>targetOs</strong></li></ul><p>Les lignes de données renvoyées comportent les champs spécifiés dans le paramètre <em>groupby</em>, ainsi que dans les paramètres suivants :</p><ul><li><strong>applicationId</strong></li><li><strong>date</strong></li><li><strong>productName</strong></li><li><strong>deviceCount</strong></li></ul></p> |  non  |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre plusieurs demandes d’obtention de données de bloc d’application de bureau. Remplacez la valeur *ApplicationID* par l’ID de produit de votre application de bureau.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockdetails?applicationId=10238467886765136388&fileName=contoso.exe&startDate=2018-05-01&endDate=2018-06-07&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockdetails?applicationId=10238467886765136388&fileName=contoso.exe&startDate=2018-05-01&endDate=2018-06-07&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse


### <a name="response-body"></a>Corps de la réponse

| Valeur      | Tapez   | Description                  |
|------------|--------|-------------------------------------------------------|
| Valeur      | tableau  | Tableau d’objets qui contiennent des données de bloc d’agrégation. Pour plus d’informations sur les données incluses dans chaque objet, voir le tableau suivant. |
| @nextLink  | chaîne | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour demander la page suivante. Par exemple, cette valeur est retournée si le paramètre **supérieur** de la demande est défini sur 10000, mais qu’il y a plus de 10000 lignes de données de bloc pour la requête. |
| TotalCount | entier    | Nombre total de lignes dans les résultats de données de la requête. |


Les éléments du tableau *Value* comportent les valeurs suivantes :

| Valeur               | Tapez   | Description                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | chaîne | ID de produit de l’application de bureau pour laquelle vous avez récupéré des données de bloc. |
| date                | chaîne | Date associée à la valeur des occurrences de bloc. |
| productName         | chaîne | Le nom complet de l’application de bureau tel que dérivé des métadonnées de ses fichiers exécutables associés. |
| fileName            | chaîne | Exécutable qui a été bloqué. |
| applicationVersion  | chaîne | Version de l’exécutable de l’application qui a été bloquée. |
| osVersion           | chaîne | L’une des chaînes suivantes qui spécifie la version du système d’exploitation sur laquelle l’application de bureau est en cours d’exécution :<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Windows Server 2016</strong></li><li><strong>Windows Server 1709</strong></li><li><strong>Connue</strong></li></ul>   |
| osRelease           | chaîne | L’une des chaînes suivantes qui spécifient la version du système d’exploitation ou la sonnerie de vol (sous la forme d’un sous-remplissage dans la version du système d’exploitation) sur laquelle l’application de bureau est en cours d’exécution.<p/><p>Pour Windows 10 :</p><ul><li><strong>Version 1507</strong></li><li><strong>Version 1511</strong></li><li><strong>Version 1607</strong></li><li><strong>Version 1703</strong></li><li><strong>Version 1709</strong></li><li><strong>Version préliminaire</strong></li><li><strong>Insider rapidement</strong></li><li><strong>Insider lent</strong></li></ul><p/><p>Pour Windows Server 1709 :</p><ul><li><strong>COMMERCIALE</strong></li></ul><p>Pour Windows Server 2016 :</p><ul><li><strong>Version 1607</strong></li></ul><p>Pour Windows 8.1 :</p><ul><li><strong>Mise à jour 1</strong></li></ul><p>Pour Windows 7 :</p><ul><li><strong>Service Pack 1</strong></li></ul><p>Si la version du système d’exploitation ou l'anneau de distribution de version d’évaluation est inconnu(e), ce champ comporte la valeur <strong>Inconnu</strong>.</p> |
| marché              | chaîne | Code du pays ISO 3166 du marché dans lequel l’application de bureau est bloquée. |
| deviceType          | chaîne | L’une des chaînes suivantes qui spécifie le type de périphérique sur lequel l’application de bureau est bloquée :<p/><ul><li><strong>ORDINATEURS</strong></li><li><strong>Serveurs</strong></li><li><strong>Tablette</strong></li><li><strong>Connue</strong></li></ul> |
| blockType            | chaîne | L’une des chaînes suivantes qui spécifie le type de bloc trouvé sur l’appareil :<p/><ul><li><strong>Sédiment potentiel</strong></li><li><strong>Sédiment temporaire</strong></li><li><strong>Notification d’exécution</strong></li></ul><p/><p/>Pour plus d’informations sur ces types de bloc et sur ce qu’ils signifient pour les développeurs et les utilisateurs, consultez la description du [rapport des blocs d’application](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#application-blocks-report). |
| architecture        | chaîne | Architecture de l’appareil sur lequel se trouve le bloc : <p/><ul><li><strong>ARM64</strong></li><li><strong>Systèmes</strong></li></ul> |
| targetOs            | chaîne | L’une des chaînes suivantes qui spécifie la version du système d’exploitation Windows 10 sur laquelle l’application de bureau est bloquée : <p/><ul><li><strong>Version 1709</strong></li><li><strong>Version 1803</strong></li></ul> |
| deviceCount         | nombre | Nombre d’appareils distincts qui ont des blocs au niveau d’agrégation spécifié. |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
     "applicationId": "10238467886765136388",
     "date": "2018-06-03",
     "productName": "Contoso Demo",
     "fileName": "contosodemo.exe",
     "applicationVersion": "2.2.2.0",
     "osVersion": "Windows 8.1",
     "osRelease": "Update 1",
     "market": "ZA",
     "deviceType": "All",
     "blockType": "Runtime Notification",
     "architecture": "X86",
     "targetOs": "RS4",
     "deviceCount": 120
    }
  ],
  "@nextLink": "desktop/blockdetails?applicationId=123456789&startDate=2018-01-01&endDate=2018-02-01&top=10000&skip=10000&groupby=applicationVersion,deviceType,osVersion,osRelease",
  "TotalCount": 23012
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Programme d’application de bureau Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)
* [Obtenir des blocs de mise à niveau pour votre application de bureau](get-desktop-block-data.md)
