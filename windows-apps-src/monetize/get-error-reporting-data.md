---
ms.assetid: 252C44DF-A2B8-4F4F-9D47-33E423F48584
description: Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir des données agrégées des rapports d’erreurs pour une plage de dates donnée et d’autres filtres facultatifs.
title: Obtenir les données de rapport d’erreurs pour votre application
ms.date: 09/04/2018
ms.topic: article
keywords: Windows 10, UWP, services Store, API Microsoft Store Analytics, Erreurs
ms.localizationpriority: medium
ms.openlocfilehash: b1ecfad64598bb74ead0a912fa67b9e1c5c5d0c2
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86492794"
---
# <a name="get-error-reporting-data-for-your-app"></a>Obtenir les données de rapport d’erreurs pour votre application

Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir des données agrégées des rapports d’erreurs pour votre application au format JSON pour une plage de dates donnée et d’autres filtres facultatifs. Cette méthode peut uniquement récupérer les erreurs qui se sont produites au cours des 30 derniers jours. Ces informations sont également disponibles dans la section **défaillances** du [rapport d’intégrité](../publish/health-report.md) dans l’espace partenaires.

Vous pouvez récupérer des informations supplémentaires sur l’erreur en utilisant les méthodes [obtenir les détails](get-details-for-an-error-in-your-app.md)de l’erreur, obtenir la trace de la [pile](get-the-stack-trace-for-an-error-in-your-app.md)et [Télécharger le fichier CAB](download-the-cab-file-for-an-error-in-your-app.md) .

## <a name="prerequisites"></a>Prérequis


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) pour l’API Microsoft Store Analytics.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de demande                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la demande

| Paramètre        | Type   |  Description      |  Obligatoire  
|---------------|--------|---------------|------|
| applicationId | string | L’ID Windows Store de l’application pour laquelle vous souhaitez récupérer les données de rapport d’erreur. L’ID du magasin est disponible sur la [page identité](../publish/view-app-identity-details.md) de l’application dans l’espace partenaires. Exemple d’ID Windows Store : 9WZDNCRFJ3Q8. |  Oui  |
| startDate | Date | Dans la plage de dates, la date de début de la récupération des données de rapport d’erreurs. La valeur par défaut est la date actuelle. Si *aggregationLevel* est un **jour**, une **semaine**ou un **mois**, ce paramètre doit spécifier une date au format ```mm/dd/yyyy``` . Si *aggregationLevel* est l' **heure**, ce paramètre peut spécifier une date au format ```mm/dd/yyyy``` ou une date et une heure au format ```yyyy-mm-dd hh:mm:ss``` .<p/><p/>**Remarque :** &nbsp; &nbsp; Cette méthode peut uniquement récupérer les erreurs qui se sont produites au cours des 30 derniers jours.  |  Non  |
| endDate | Date | Dans la plage de dates, la date de fin de la récupération des données de rapports d’erreurs. La valeur par défaut est la date actuelle. Si *aggregationLevel* est un **jour**, une **semaine**ou un **mois**, ce paramètre doit spécifier une date au format ```mm/dd/yyyy``` . Si *aggregationLevel* est l' **heure**, ce paramètre peut spécifier une date au format ```mm/dd/yyyy``` ou une date et une heure au format ```yyyy-mm-dd hh:mm:ss``` . |  Non  |
| top | int | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | int | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite. |  Non  |
| Filter |string  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction contient un nom de champ du corps de la réponse et une valeur qui sont associés aux **opérateurs** **EQ** ou ne, et les instructions peuvent être combinées à l’aide **de and** ou **de ou de**. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*. Vous pouvez spécifier les champs suivants dans le corps de la réponse :<p/><ul><li>**applicationName**</li><li>**failureName**</li><li>**failureHash**</li><li>**symbole**</li><li>**osVersion**</li><li>**osRelease**</li><li>**eventType**</li><li>**négoci**</li><li>**deviceType**</li><li>**packageName**</li><li>**packageVersion**</li><li>**date**</li></ul> | Non   |
| aggregationLevel | string | Indique la plage de temps pendant laquelle récupérer les données agrégées. Il peut s’agir de l’une des chaînes suivantes : **heure**, **jour**, **semaine**ou **mois**. Par défaut, la valeur est **day**. Si vous spécifiez **week** ou **month**, les valeurs *failureName* et *failureHash* sont limitées à 1 000 compartiments.<p/><p/>**Remarque :** &nbsp; &nbsp; Si vous spécifiez l' **heure**, vous pouvez récupérer les données d’erreur uniquement à partir des 72 heures précédentes. Pour récupérer les données d’erreur datant de plus de 72 heures, spécifiez le **jour** ou l’un des autres niveaux d’agrégation.  | Non |
| orderby | string | Instruction commandant les valeurs des données de résultats. La syntaxe est *orderby = Field [Order], champ [Order],...*. Le paramètre *Field* peut être l’une des chaînes suivantes :<ul><li>**applicationName**</li><li>**failureName**</li><li>**failureHash**</li><li>**symbole**</li><li>**osVersion**</li><li>**osRelease**</li><li>**eventType**</li><li>**négoci**</li><li>**deviceType**</li><li>**packageName**</li><li>**packageVersion**</li><li>**date**</li></ul><p>Le paramètre *order*, facultatif, peut comporter les valeurs **asc** ou **desc** afin de spécifier l’ordre croissant ou décroissant pour chaque champ. La valeur par défaut est **ASC**.</p><p>Voici un exemple de chaîne *orderby* : *orderby = date, Market*</p> |  Non  |
| groupby | string | Une instruction qui applique l’agrégation des données uniquement sur les champs spécifiés. Vous pouvez spécifier les champs suivants :<ul><li>**failureName**</li><li>**failureHash**</li><li>**symbole**</li><li>**osVersion**</li><li>**eventType**</li><li>**négoci**</li><li>**deviceType**</li><li>**packageName**</li><li>**packageVersion**</li></ul><p>Les lignes de données renvoyées comportent les champs spécifiés dans le paramètre *groupby*, ainsi que dans les paramètres suivants :</p><ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**deviceCount**</li><li>**eventCount**</li></ul><p>Le paramètre *groupby* peut être utilisé avec le paramètre *aggregationLevel*. Par exemple : * &amp; GroupBy = failureName, Market &amp; aggregationLevel = week*</p></p> |  Non  |


### <a name="request-example"></a>Exemple de requête

Les exemples suivants fournissent font figurer plusieurs requêtes de récupération des données de rapport d’erreurs. Remplacez la valeur *applicationId* par l’ID Windows Store de votre application.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'phone' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>response


### <a name="response-body"></a>Response body

| Valeur      | Type    | Description     |
|------------|---------|--------------|
| Valeur      | tableau   | Tableau d’objets comportant les données agrégées de rapport d’erreurs. Pour plus d’informations sur les données de chaque objet, consultez la section [Valeurs des erreurs](#error-values) ci-dessous.     |
| @nextLink  | string  | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est renvoyée si le paramètre **top** de la demande est défini sur 10000, mais que plus de 10 000 lignes d’erreurs sont associées à la requête. |
| TotalCount | entier | Nombre total de lignes dans les résultats de la requête.     |


### <a name="error-values"></a>Valeurs d’erreur

Les éléments du tableau *Value* comportent les valeurs suivantes :

| Valeur           | Type    | Description        |
|-----------------|---------|---------------------|
| Date            | string  | Première date dans la plage de dates pour les données d’erreur, au format ```yyyy-mm-dd``` . Si la demande spécifie un seul jour, cette valeur est cette date. Si la demande spécifie une plage de dates plus longue, cette valeur correspond à la première date de cette plage de dates. Pour les demandes qui spécifient une valeur *aggregationLevel* de l' **heure**, cette valeur comprend également une valeur d’heure au format ```hh:mm:ss``` .  |
| applicationId   | string  | ID Windows Store de l’application dont vous souhaitez récupérer les données d’erreur.   |
| applicationName | string  | Nom d’affichage de l’application.   |
| failureName     | string  | Nom de l’échec, qui est constitué de quatre parties : une ou plusieurs classes de problème, un code de vérification des exceptions/bogues, le nom de l’image où l’échec s’est produit et le nom de la fonction associée.  |
| failureHash     | string  | Identificateur unique de l’erreur.   |
| symbole          | string  | Le symbole affecté à cette erreur. |
| osVersion       | string  | L’une des chaînes suivantes qui spécifie la version du système d’exploitation sur laquelle l’erreur s’est produite :<ul><li>**Windows Phone 7.5**</li><li>**Windows Phone 8**</li><li>**Windows Phone 8.1**</li><li>**Windows Phone 10**</li><li>**Windows 8**</li><li>**Windows 8.1**</li><li>**Windows 10**</li><li>**Unknown**</li></ul>  |
| osRelease       | string  |  L’une des chaînes suivantes qui spécifient la version du système d’exploitation ou la sonnerie de vol (sous la forme d’un sous-remplissage dans la version du système d’exploitation) sur laquelle l’erreur s’est produite.<p/><p>Pour Windows 10 :</p><ul><li><strong>Version 1507</strong></li><li><strong>Version 1511</strong></li><li><strong>Version 1607</strong></li><li><strong>Version 1703</strong></li><li><strong>Version 1709</strong></li><li><strong>Version 1803</strong></li><li><strong>Version préliminaire</strong></li><li><strong>Insider rapidement</strong></li><li><strong>Insider lent</strong></li></ul><p/><p>Pour Windows Server 1709 :</p><ul><li><strong>COMMERCIALE</strong></li></ul><p>Pour Windows Server 2016 :</p><ul><li><strong>Version 1607</strong></li></ul><p>Pour Windows 8.1 :</p><ul><li><strong>Update 1</strong></li></ul><p>Pour Windows 7 :</p><ul><li><strong>Service Pack 1</strong></li></ul><p>Si la version du système d’exploitation ou la sonnerie de vol est inconnue, ce champ a la valeur <strong>Unknown</strong>.</p>    |
| eventType       | string  | Une des chaînes suivantes :<ul><li>**incident**</li><li>**panne**</li><li>**capacité**</li><li>**JSE**</li></ul>      |
| market          | string  | Code pays ISO 3166 du marché des appareils.   |
| deviceType      | string  | L’une des chaînes suivantes qui indique le type d’appareil sur lequel l’erreur s’est produite :<ul><li>**PC**</li><li>**Numéros**</li><li>**Console-Xbox One**</li><li>**Console-série Xbox X**</li><li>**IoT**</li><li>**Hologrammes**</li><li>**Unknown**</li></ul>    |
| packageName     | string  | Nom unique du package applicatif associé à cette erreur.      |
| packageVersion  | string  | Version du package applicatif associé à cette erreur.   |
| deviceCount     | nombre | Le nombre d’appareils uniques correspondant à cette erreur pour le niveau d’agrégation spécifié.  |
| eventCount      | nombre | Le nombre d’événements affectés à cette erreur pour le niveau d’agrégation spécifié.      |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "date": "2017-03-09",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Demo",
      "failureName": "APPLICATION_FAULT_8013150a_StoreWrapper.ni.DLL!70475e55",
      "failureHash": "5a6b2170-1661-ed47-24d7-230fed0077af",
      "symbol": "storewrapper_ni!70475e55",
      "osVersion": "Windows 10",
      "osRelease": "Version 1507",
      "eventType": "crash",
      "market": "US",
      "deviceType": "PC",
      "packageName": "",
      "packageVersion": "0.0.0.0",
      "deviceCount": 0.0,
      "eventCount": 1.0
    }
  ],
  "@nextLink": "failurehits?applicationId=9NBLGGGZ5QDR&aggregationLevel=week&startDate=2017/03/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 21
}

```

## <a name="related-topics"></a>Rubriques connexes

* [Rapport sur l’intégrité](../publish/health-report.md)
* [Obtenir les informations sur une erreur de votre application](get-details-for-an-error-in-your-app.md)
* [Obtenir la trace de pile concernant une erreur dans votre application](get-the-stack-trace-for-an-error-in-your-app.md)
* [Télécharger le fichier CAB concernant une erreur dans votre application](download-the-cab-file-for-an-error-in-your-app.md)
* [Accéder aux données d’analyse à l’aide des services Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir des acquisitions d’applications](get-app-acquisitions.md)
* [Obtenir des acquisitions d’extensions](get-in-app-acquisitions.md)
* [Obtenir les classifications des applications](get-app-ratings.md)
* [Obtenir les avis sur les applications](get-app-reviews.md)
