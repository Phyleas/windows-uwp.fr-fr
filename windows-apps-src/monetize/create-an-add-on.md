---
ms.assetid: 5BD650D2-AA26-4DE9-8243-374FDB7D932B
description: Utilisez cette méthode dans l’API de soumission Microsoft Store pour créer un module complémentaire pour une application inscrite auprès de votre compte PartnerCenter.
title: Créer une extension
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store API de soumission, créer un module complémentaire, produit dans l’application, IAP
ms.localizationpriority: medium
ms.openlocfilehash: a54ad4bcdb0c6fd652d56c767aa8362073b07875
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167723"
---
# <a name="create-an-add-on"></a>Créer une extension

Utilisez cette méthode dans l’API de soumission Microsoft Store pour créer un module complémentaire (également appelé produit dans l’application ou IAP) pour une application inscrite auprès de votre compte espace partenaires.

> [!NOTE]
> Cette méthode crée un module complémentaire sans aucune soumission. Pour créer une soumission pour une extension, voir les méthodes décrites dans l’article [Gérer les soumissions d’extensions](manage-add-on-submissions.md).

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) pour l’API de soumission Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de demande                                                      |
|--------|------------------------------------------------------------------|
| POST    | `https://manage.devcenter.microsoft.com/v1.0/my/inappproducts` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-body"></a>Corps de la demande

Le corps de la requête contient les paramètres suivants.

|  Paramètre  |  Type  |  Description  |  Obligatoire  |
|------|------|------|------|
|  applicationIds  |  tableau  |  Tableau qui contient l’ID Windows Store de l’application à laquelle cette extension est associée. Un seul élément est pris en charge dans ce tableau.   |  Oui  |
|  productId  |  string  |  ID de produit de l’extension. Il s’agit d’un identificateur que vous pouvez utiliser dans le code pour faire référence à l’extension. Pour plus d’informations, consultez [Définir le type et l’ID de votre produit](../publish/set-your-add-on-product-id.md).  |  Oui  |
|  productType  |  string  |  Type de produit de l’extension. Les valeurs suivantes sont prises en charge : **Durable** et **Consommable**.  |  Oui  |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment créer une extension consommable pour une application.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1Q...
Content-Type: application/json
{
    "applicationIds": [  "9NBLGGH4R315"  ],
    "productId": "my-new-add-on",
    "productType": "Consumable",
}
```

## <a name="response"></a>response

L’exemple suivant illustre le corps de réponse JSON d’un appel réussi à cette méthode. Pour plus d’informations sur les valeurs figurant dans le corps de réponse, consultez [ressource d’extension](manage-add-ons.md#add-on-object).

```json
{
  "applications": {
    "value": [
      {
        "id": "9NBLGGH4R315",
        "resourceLocation": "applications/9NBLGGH4R315"
      }
    ],
    "totalCount": 1
  },
  "id": "9NBLGGH4TNMP",
  "productId": "my-new-add-on",
  "productType": "Consumable",
}
```

## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Code d'erreur |  Description                                                                                                                                                                           |
|--------|------------------|
| 400  | La requête n’est pas valide. |
| 409  | Le module complémentaire n’a pas pu être créé en raison de son état actuel, ou le module complémentaire utilise une fonctionnalité d’espace partenaires qui n’est [pas prise en charge actuellement par le Microsoft Store API de soumission](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gérer les soumissions d’extensions](manage-add-on-submissions.md)
* [Obtenir toutes les extensions](get-all-add-ons.md)
* [Obtenir une extension](get-an-add-on.md)
* [Supprime une extension](delete-an-add-on.md)