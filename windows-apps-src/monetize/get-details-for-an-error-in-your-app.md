---
ms.assetid: f0c0325e-ad61-4238-a096-c37802db3d3b
description: Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir des données détaillées pour une erreur spécifique pour votre application.
title: Obtenir les informations sur une erreur de votre application
ms.date: 06/05/2018
ms.topic: article
keywords: Windows 10, UWP, services Store, API Microsoft Store Analytics, erreurs, détails
ms.localizationpriority: medium
ms.openlocfilehash: 52e440fc7e1d174567b9b10c92e38acb3e2edc18
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86492804"
---
# <a name="get-details-for-an-error-in-your-app"></a>Obtenir les informations sur une erreur de votre application

Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir des données détaillées pour une erreur spécifique pour votre application au format JSON. Cette méthode ne récupère que les informations concernant les erreurs survenues dans les 30 derniers jours. Des données d’erreur détaillées sont également disponibles dans la section **défaillances** du [rapport d’intégrité](../publish/health-report.md) dans l’espace partenaires.

Pour utiliser cette méthode, vous devez d’abord utiliser la méthode [Obtenir les données de rapport d’erreurs](get-error-reporting-data.md) afin de récupérer l’ID de l’erreur sur laquelle vous souhaitez des informations détaillées.

## <a name="prerequisites"></a>Prérequis


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) pour l’API Microsoft Store Analytics.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Récupérez l’ID de l’erreur sur laquelle vous souhaitez des informations détaillées. Pour ce faire, utilisez la méthode [Obtenir les données de rapport d’erreurs](get-error-reporting-data.md) et utilisez la valeur **failureHash** dans le corps de la réponse de cette méthode.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de demande                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/failuredetails``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la demande

| Paramètre        | Type   |  Description      |  Obligatoire  
|---------------|--------|---------------|------|
| applicationId | string | ID Windows Store de l’application dont vous souhaitez récupérer des données d’erreur. L’ID du magasin est disponible sur la [page identité](../publish/view-app-identity-details.md) de l’application dans l’espace partenaires. Exemple d’ID Windows Store : 9WZDNCRFJ3Q8. |  Oui  |
| failureHash | string | ID de l’erreur sur laquelle vous souhaitez des informations détaillées. Pour obtenir la valeur correspondant à l’erreur qui vous intéresse, utilisez la méthode [Obtenir les données de rapport d’erreurs](get-error-reporting-data.md) et utilisez la valeur **failureHash** dans le corps de la réponse de cette méthode. |  Oui  |
| startDate | Date | Date de début des données à récupérer concernant l’erreur. La valeur par défaut est de 30 jours avant la date actuelle.<p/><p/>**Remarque :** &nbsp; &nbsp; Cette méthode peut uniquement récupérer les détails des erreurs qui se sont produites au cours des 30 derniers jours. |  Non  |
| endDate | Date | Date de fin des données à récupérer concernant l’erreur. La valeur par défaut est la date actuelle. |  Non  |
| top | int | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | int | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10 et skip=0 pour obtenir les 10 premières lignes de données, top=10 et skip=10 pour obtenir les 10 lignes suivantes, et ainsi de suite. |  Non  |
| Filter |string  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction contient un nom de champ du corps de la réponse et une valeur qui sont associés aux **opérateurs** **EQ** ou ne, et les instructions peuvent être combinées à l’aide **de and** ou **de ou de**. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*. Vous pouvez spécifier les champs suivants dans le corps de la réponse :<p/><ul><li><strong>négoci</strong></li><li><strong>date</strong></li><li><strong>cabId</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>DeviceModel au</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>packageVersion</strong></li><li><strong>osBuild</strong></li></ul> | Non   |
| orderby | string | Instruction commandant les valeurs des données de résultats. La syntaxe est <em>orderby = Field [Order], champ [Order],...</em>. Le paramètre <em>Field</em> peut être l’une des chaînes suivantes :<ul><li><strong>négoci</strong></li><li><strong>date</strong></li><li><strong>cabId</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>DeviceModel au</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>packageVersion</strong></li><li><strong>osBuild</strong></li></ul><p>Le paramètre <em>order</em>, facultatif, peut comporter les valeurs <strong>asc</strong> ou <strong>desc</strong> afin de spécifier l’ordre croissant ou décroissant pour chaque champ. La valeur par défaut est <strong>ASC</strong>.</p><p>Voici un exemple de chaîne <em>orderby</em> : <em>orderby = date, Market</em></p> |  Non  |


### <a name="request-example"></a>Exemple de requête

Les exemples suivants fournissent plusieurs requêtes permettant de récupérer des données d’erreur. Remplacez la valeur *applicationId* par l’ID Windows Store de votre application.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failuredetails?applicationId=9NBLGGGZ5QDR&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failuredetails?applicationId=9NBLGGGZ5QDR&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0&filter=market eq 'US' and deviceType eq 'Windows.Desktop' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>response


### <a name="response-body"></a>Response body

| Valeur      | Type    | Description    |
|------------|---------|------------|
| Valeur      | tableau   | Tableau d’objets comportant des données d’erreur détaillées. Pour plus d’informations sur les données de chaque objet, consultez la section [Valeurs des informations d’erreur](#error-detail-values) ci-dessous.          |
| @nextLink  | string  | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête est défini sur 10, mais que plus de 10 lignes d’erreur sont associées à la requête. |
| TotalCount | entier | Nombre total de lignes dans les résultats de la requête.        |


<span id="error-detail-values"/>

### <a name="error-detail-values"></a>Valeurs des informations d’erreur

Les éléments du tableau *Value* comportent les valeurs suivantes :

| Valeur           | Type    | Description     |
|-----------------|---------|----------------------------|
| applicationId   | string  | ID Windows Store de l’application dont vous souhaitez récupérer des données d’erreur.      |
| failureHash     | string  | Identificateur unique de l’erreur.     |
| failureName     | string  | Nom de l’échec, qui est constitué de quatre parties : une ou plusieurs classes de problème, un code de vérification des exceptions/bogues, le nom de l’image où l’échec s’est produit et le nom de la fonction associée.           |
| Date            | string  | Date de début des données d’erreur. Si la requête spécifiait un jour précis, cette valeur correspond à la date. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| cabId           | string  | ID unique du fichier CAB associé à cette erreur.   |
| cabExpirationTime  | string  | Date et heure auxquelles le fichier CAB est arrivé à expiration et n’est plus téléchargeable au format ISO 8601.   |
| market          | string  | Code pays ISO 3166 du marché des appareils.     |
| osBuild         | string  | Numéro de version du système d’exploitation sur lequel l’erreur s’est produite.       |
| packageVersion  | string  | Version du package applicatif associé à cette erreur.    |
| deviceModel           | string  | Chaîne identifiant le modèle d’appareil sur lequel l’application s’exécutait lorsque l’erreur s’est produite.   |
| osVersion       | string  | L’une des chaînes suivantes qui indique la version du système d’exploitation sur laquelle l’erreur s’est produite :<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows 8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Unknown</strong></li></ul>    |
| osRelease       | string  |  L’une des chaînes suivantes qui spécifient la version du système d’exploitation ou la sonnerie de vol (sous la forme d’un sous-remplissage dans la version du système d’exploitation) sur laquelle l’erreur s’est produite.<p/><p>Pour Windows 10 :</p><ul><li><strong>Version 1507</strong></li><li><strong>Version 1511</strong></li><li><strong>Version 1607</strong></li><li><strong>Version 1703</strong></li><li><strong>Version 1709</strong></li><li><strong>Version 1803</strong></li><li><strong>Version préliminaire</strong></li><li><strong>Insider rapidement</strong></li><li><strong>Insider lent</strong></li></ul><p/><p>Pour Windows Server 1709 :</p><ul><li><strong>COMMERCIALE</strong></li></ul><p>Pour Windows Server 2016 :</p><ul><li><strong>Version 1607</strong></li></ul><p>Pour Windows 8.1 :</p><ul><li><strong>Update 1</strong></li></ul><p>Pour Windows 7 :</p><ul><li><strong>Service Pack 1</strong></li></ul><p>Si la version du système d’exploitation ou la sonnerie de vol est inconnue, ce champ a la valeur <strong>Unknown</strong>.</p>    |
| deviceType      | string  | Une des chaînes suivantes qui spécifie le type de l’appareil sur lequel l’application s’exécutait lorsque l’erreur s’est produite :<ul><li><strong>PC</strong></li><li><strong>Numéros</strong></li><li><strong>Console-Xbox One</strong></li><li><strong>Console-série Xbox X</strong></li><li><strong>IoT</strong></li><li><strong>Hologrammes</strong></li><li><strong>Unknown</strong></li></ul>     |
| cabDownloadable           | Booléen  | Indique si le fichier CAB est téléchargeable par cet utilisateur.   |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR ",
      "failureHash": "012345-5dbc9-b12f-c124-9d9810f05d8b",
      "failureName": "STOWED_EXCEPTION_System.UriFormatException_exe!ContosoGame.GroupedItems+_ItemView_ItemClick_d__9.MoveNext",
      "date": "2018-02-05 09:11:25",
      "cabId": "133637331323",
      "cabExpirationTime": "2016-12-05 09:11:25",
      "market": "US",
      "osBuild": "10.0.10240",
      "packageVersion": "1.0.2.6",
      "deviceModel": "Contoso Computer",
      "osVersion": "Windows 10",
      "osRelease": "Version 1507",
      "deviceType": "PC",
      "cabDownloadable": false
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Rapport sur l’intégrité](../publish/health-report.md)
* [Accéder aux données d’analyse à l’aide des services Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir les données de rapport d’erreurs](get-error-reporting-data.md)
* [Obtenir la trace de pile concernant une erreur dans votre application](get-the-stack-trace-for-an-error-in-your-app.md)
* [Télécharger le fichier CAB concernant une erreur dans votre application](download-the-cab-file-for-an-error-in-your-app.md)
