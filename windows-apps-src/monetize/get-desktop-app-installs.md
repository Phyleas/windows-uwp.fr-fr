---
description: Utilisez cet URI REST pour obtenir des données d’installation agrégées pour une application de bureau pendant une plage de dates donnée et d’autres filtres facultatifs.
title: Obtenir les installations d’applications de bureau
ms.date: 03/01/2018
ms.topic: article
keywords: Windows 10, installations d’applications de bureau, programme d’application de bureau Windows
localizationpriority: medium
ms.openlocfilehash: 1c4f7aaffc9e623d4863bee4de44daa75ae05852
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175023"
---
# <a name="get-desktop-application-installs"></a>Obtenir les installations d’applications de bureau


Utilisez cet URI REST pour obtenir des données d’installation agrégées au format JSON pour une application de bureau que vous avez ajoutée au [programme d’application de bureau Windows](/windows/desktop/appxpkg/windows-desktop-application-program). Cet URI vous permet d’obtenir des données d’installation pendant une plage de dates donnée et d’autres filtres facultatifs. Ces informations sont également disponibles dans le [rapport installations](/windows/desktop/appxpkg/windows-desktop-application-program) pour les applications de bureau dans l’espace partenaires.

## <a name="prerequisites"></a>Prérequis


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) pour l’API Microsoft Store Analytics.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de demande       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/installbasedaily```|


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la demande

| Paramètre        | Type   |  Description      |  Obligatoire  
|---------------|--------|---------------|------|
| applicationId | string | ID de produit de l’application de bureau pour laquelle vous souhaitez récupérer des données d’installation. Pour obtenir l’ID de produit d’une application de bureau, ouvrez un [rapport analytique pour votre application de bureau dans l’espace partenaires](/windows/desktop/appxpkg/windows-desktop-application-program) (par exemple, le **rapport installations**) et récupérez l’ID de produit à partir de l’URL. |  Oui  |
| startDate | Date | Date de début dans la plage de dates des données d’installation à récupérer. La valeur par défaut est de 90 jours avant la date actuelle. |  Non  |
| endDate | Date | Date de fin dans la plage de dates des données d’installation à récupérer. La valeur par défaut est la date actuelle. |  Non  |
| top | int | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | int | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite. |  Non  |
| Filter | string  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction contient un nom de champ du corps de la réponse et une valeur qui sont associés aux **opérateurs** **EQ** ou ne, et les instructions peuvent être combinées à l’aide **de and** ou **de ou de**. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*. Vous pouvez spécifier les champs suivants dans le corps de la réponse :<p/><ul><li><strong>applicationVersion</strong></li><li><strong>deviceType</strong></li><li><strong>négoci</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li></ul> | Non   |
| orderby | string | Instruction qui classe les valeurs des données de résultat pour chaque installation. La syntaxe est <em>orderby = Field [Order], champ [Order],...</em>. Le paramètre <em>Field</em> peut être l’un des champs suivants du corps de la réponse :<p/><ul><li><strong>productName</strong></li><li><strong>date</strong><li><strong>applicationVersion</strong></li><li><strong>deviceType</strong></li><li><strong>négoci</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>installBase</strong></li></ul><p>Le paramètre <em>order</em>, facultatif, peut comporter les valeurs <strong>asc</strong> ou <strong>desc</strong> afin de spécifier l’ordre croissant ou décroissant pour chaque champ. La valeur par défaut est <strong>ASC</strong>.</p><p>Voici un exemple de chaîne <em>orderby</em> : <em>orderby = date, Market</em></p> |  Non  |
| groupby | string | Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants dans le corps de la réponse :<p/><ul><li><strong>applicationVersion</strong></li><li><strong>deviceType</strong></li><li><strong>négoci</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li></ul><p>Les lignes de données renvoyées comportent les champs spécifiés dans le paramètre <em>groupby</em>, ainsi que dans les paramètres suivants :</p><ul><li><strong>applicationId</strong></li><li><strong>date</strong></li><li><strong>productName</strong></li><li><strong>installBase</strong></li></ul></p> |  Non  |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre plusieurs demandes d’obtention des données d’installation de l’application de bureau. Remplacez la valeur *ApplicationID* par l’ID de produit de votre application de bureau.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/installbasedaily?applicationId=1234567890&startDate=2018-01-01&endDate=2018-02-01&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/installbasedaily?applicationId=1234567890&startDate=2018-01-01&endDate=2018-02-01&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>response


### <a name="response-body"></a>Response body

| Valeur      | Type   | Description                  |
|------------|--------|-------------------------------------------------------|
| Valeur      | tableau  | Tableau d’objets qui contiennent des données d’installation de regroupement. Pour plus d’informations sur les données de chaque objet, consultez le tableau suivant. |
| @nextLink  | string | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est retournée si le paramètre **supérieur** de la demande est défini sur 10000, mais qu’il y a plus de 10000 lignes de données d’installation pour la requête. |
| TotalCount | int    | Nombre total de lignes dans les résultats de la requête. |


Les éléments du tableau *Value* comportent les valeurs suivantes :

| Valeur               | Type   | Description                           |
|---------------------|--------|-------------------------------------------|
| Date                | string | Date associée à la valeur de base de l’installation. |
| applicationId       | string | ID de produit de l’application de bureau pour laquelle vous avez récupéré les données d’installation. |
| ProductName         | string | Nom complet de l’application de bureau déduite des métadonnées de ses exécutables associés. |
| applicationVersion  | string | Version de l’exécutable de l’application qui a été installée. |
| deviceType          | string | L’une des chaînes suivantes qui spécifient le type d’appareil sur lequel l’application de bureau est installée :<p/><ul><li><strong>ORDINATEURS</strong></li><li><strong>Serveur</strong></li><li><strong>Tablet</strong></li><li><strong>Unknown</strong></li></ul> |
| market              | string | Code du pays ISO 3166 du marché sur lequel l’application de bureau est installée. |
| osVersion           | string | L’une des chaînes suivantes qui spécifient la version du système d’exploitation sur laquelle l’application de bureau est installée :<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Windows Server 2016</strong></li><li><strong>Windows Server 1709</strong></li><li><strong>Unknown</strong></li></ul>   |
| osRelease           | string | L’une des chaînes suivantes qui spécifient la version du système d’exploitation ou la sonnerie de vol (sous la forme d’un sous-remplissage dans la version du système d’exploitation) sur laquelle l’application de bureau est installée.<p/><p>Pour Windows 10 :</p><ul><li><strong>Version 1507</strong></li><li><strong>Version 1511</strong></li><li><strong>Version 1607</strong></li><li><strong>Version 1703</strong></li><li><strong>Version 1709</strong></li><li><strong>Version préliminaire</strong></li><li><strong>Insider rapidement</strong></li><li><strong>Insider lent</strong></li></ul><p/><p>Pour Windows Server 1709 :</p><ul><li><strong>COMMERCIALE</strong></li></ul><p>Pour Windows Server 2016 :</p><ul><li><strong>Version 1607</strong></li></ul><p>Pour Windows 8.1 :</p><ul><li><strong>Update 1</strong></li></ul><p>Pour Windows 7 :</p><ul><li><strong>Service Pack 1</strong></li></ul><p>Si la version du système d’exploitation ou la sonnerie de vol est inconnue, ce champ a la valeur <strong>Unknown</strong>.</p> |
| installBase         | nombre | Nombre d’appareils distincts sur lesquels le produit est installé au niveau de l’agrégation spécifiée. |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "date": "2018-01-24",
      "applicationId": "123456789",
      "productName": "Contoso Demo",
      "applicationVersion": "1.0.0.0",
      "deviceType": "PC",
      "market": "All",
      "osVersion": "Windows 10",
      "osRelease": "Version 1709",
      "installBase": 348218.0
    }
  ],
  "@nextLink": "desktop/installbasedaily?applicationId=123456789&startDate=2018-01-01&endDate=2018-02-01&top=10000&skip=10000&groupby=applicationVersion,deviceType,osVersion,osRelease",
  "TotalCount": 23012
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Programme d’application de bureau Windows](/windows/desktop/appxpkg/windows-desktop-application-program)