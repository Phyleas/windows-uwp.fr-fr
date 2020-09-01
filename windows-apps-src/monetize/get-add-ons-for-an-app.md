---
ms.assetid: E59FB6FE-5318-46DF-B050-73F599C3972A
description: Utilisez cette méthode dans l’API de soumission Microsoft Store pour récupérer des informations sur les achats dans l’application pour une application inscrite auprès de votre espace partenaires.
title: Obtenir des extensions pour une application
ms.date: 02/08/2017
ms.topic: article
keywords: API de soumission Windows 10, UWP, Microsoft Store, modules complémentaires, produits dans l’application, IAPs
ms.localizationpriority: medium
ms.openlocfilehash: 77d2bf238d74ca1576e45898afa752b78f05db0e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167673"
---
# <a name="get-add-ons-for-an-app"></a>Obtenir des extensions pour une application

Utilisez cette méthode dans l’API de soumission Microsoft Store pour répertorier les modules complémentaires pour une application inscrite auprès de votre compte espace partenaires.

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) pour l’API de soumission Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de demande                                                      |
|--------|------------------------------------------------------------------|
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listinappproducts` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la demande


|  Nom  |  Type  |  Description  |  Obligatoire  |
|------|------|------|------|
|  applicationId  |  string  |  ID Windows Store de l’application pour laquelle vous voulez récupérer les extensions. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](../publish/view-app-identity-details.md).  |  Oui  |
|  top  |  int  |  Nombre d’éléments à retourner dans la requête (autrement dit, nombre d’extensions à retourner). Si l’application comporte plus d’extensions que la valeur que vous spécifiez dans la requête, le corps de la réponse comprend un chemin d’URI relatif que vous pouvez ajouter à l’URI de la méthode pour solliciter la page suivante de données.  |  Non  |
|  skip |  int  | Nombre d’éléments à ignorer dans la requête avant de retourner les éléments restants. Utilisez ce paramètre pour parcourir des ensembles de données. Par exemple, top=10 et skip=0 permettent de récupérer les éléments 1 à 10, top=10 et skip=10 permettent de récupérer les éléments 11 à 20, et ainsi de suite.   |  Non  |


### <a name="request-body"></a>Corps de la demande

Ne fournissez pas de corps de requête pour cette méthode.

### <a name="request-examples"></a>Exemples de demande

L’exemple suivant montre comment répertorier toutes les extensions pour une application.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listinappproducts HTTP/1.1
Authorization: Bearer <your access token>
```

L’exemple suivant montre comment répertorier les 10 premières extensions pour une application.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listinappproducts?top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>response

L’exemple suivant montre le corps de réponse JSON renvoyé par une requête réussie sur les 10 premières extensions d’une application qui en possède 53 au total. Pour des raisons de concision, cet exemple affiche uniquement les données des trois premières extensions retournées par la requête. Pour plus d’informations sur les valeurs figurant dans le corps de réponse, voir la section suivante.

```json
{
  "@nextLink": "applications/9NBLGGH4R315/listinappproducts/?skip=10&top=10",
  "value": [
    {
      "inAppProductId": "9NBLGGH4TNMP"
    },
    {
      "inAppProductId": "9NBLGGH4TNMN"
    },
    {
      "inAppProductId": "9NBLGGH4TNNR"
    },
    // Next 7 add-ons  are omitted for brevity ...
  ],
  "totalCount": 53
}
```

### <a name="response-body"></a>Response body

| Valeur      | Type   | Description                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| @nextLink  | string | S’il existe des pages supplémentaires de données, cette chaîne contient un chemin relatif que vous pouvez ajouter à l’URI de requête `https://manage.devcenter.microsoft.com/v1.0/my/` de base pour solliciter la page suivante de données. Par exemple, si le paramètre *supérieur* du corps de la demande initiale est défini sur 10, mais qu’il existe 50 modules complémentaires pour l’application, le corps de la réponse inclut la @nextLink valeur `applications/{applicationid}/listinappproducts/?skip=10&top=10` , qui indique que vous pouvez appeler `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationid}/listinappproducts/?skip=10&top=10` pour demander les 10 modules complémentaires suivants. |
| value      | tableau  | Tableau d’objets qui répertorie l’ID Windows Store de chaque extension pour l’application spécifiée. Pour plus d’informations sur les données incluses dans chaque objet, voir [ressource d’extension](get-app-data.md#add-on-object).                                                                                                                           |
| totalCount | int    | Nombre total de lignes dans les résultats de données pour la requête (autrement dit, nombre total d’extensions pour l’application spécifiée).    |


## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Code d'erreur |  Description   |
|--------|------------------|
| 404  | Aucune extension n’a été trouvée. |
| 409  | Les modules complémentaires utilisent des fonctionnalités de l’espace partenaires qui [ne sont actuellement pas prises en charge par l’API de soumission Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |


## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obtenir toutes les applications](get-all-apps.md)
* [Obtenir une application](get-an-app.md)
* [Obtenir des versions d’évaluation du package pour une application](get-flights-for-an-app.md)