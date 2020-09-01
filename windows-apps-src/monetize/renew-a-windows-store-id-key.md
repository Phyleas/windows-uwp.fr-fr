---
ms.assetid: 3569C505-8D8C-4D85-B383-4839F13B2466
description: Découvrez comment renouveler une clé d’ID de Microsoft Store expirée à l’aide de la méthode Renew dans les API de regroupement et d’achat Microsoft Store.
title: Renouveler une clé d’ID du Microsoft Store
ms.date: 03/19/2018
ms.topic: article
keywords: API de collection Windows 10, UWP, Microsoft Store, Microsoft Store achat d’API, Microsoft Store clé d’ID, renouvellement
ms.localizationpriority: medium
ms.openlocfilehash: fe19b446f88e16b87ff40288f5d4480f9230469e
ms.sourcegitcommit: e273e5901bfa6596dfef4cc741bb1c42614c25ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89238254"
---
# <a name="renew-a-microsoft-store-id-key"></a>Renouveler une clé d’ID du Microsoft Store


Utilisez cette méthode pour renouveler une clé de Microsoft Store. Lorsque vous [générez une clé d’ID de Microsoft Store](view-and-grant-products-from-a-service.md#step-4), la clé est valide pendant 90 jours. Après l’expiration de la clé, vous pouvez utiliser la clé arrivée à expiration pour en renégocier une nouvelle à l’aide de cette méthode.

## <a name="prerequisites"></a>Prérequis


Pour utiliser cette méthode, vous devez disposer des éléments suivants :

* Jeton d’accès Azure AD qui a la valeur d’URI de l’audience `https://onestore.microsoft.com` .
* Une clé d’ID de Microsoft Store expirée qui a été [générée à partir du code côté client dans votre application](view-and-grant-products-from-a-service.md#step-4).

Pour plus d’informations, consultez [gérer les habilitations de produit à partir d’un service](view-and-grant-products-from-a-service.md).

## <a name="request"></a>Requête

### <a name="request-syntax"></a>Syntaxe de la requête

| Type de clé    | Méthode | URI de demande                                              |
|-------------|--------|----------------------------------------------------------|
| Collections | POST   | ```https://collections.mp.microsoft.com/v6.0/b2b/keys/renew``` |
| Purchase    | POST   | ```https://purchase.mp.microsoft.com/v6.0/b2b/keys/renew```    |


### <a name="request-header"></a>En-tête de requête

| En-tête         | Type   | Description                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Host           | string | Doit être défini sur la valeur **collections.mp.microsoft.com** ou **purchase.mp.microsoft.com**.           |
| Content-Length | nombre | Longueur du corps de la demande.                                                                       |
| Content-Type   | string | Spécifie le type de requête et de réponse. Actuellement, la seule valeur prise en charge est **application/json**. |


### <a name="request-body"></a>Corps de la demande

| Paramètre     | Type   | Description                       | Obligatoire |
|---------------|--------|-----------------------------------|----------|
| serviceTicket | string | Jeton d’accès Azure AD.        | Oui      |
| key           | string | Clé d’ID de Microsoft Store expirée. | Oui       |


### <a name="request-example"></a>Exemple de requête

```syntax
POST https://collections.mp.microsoft.com/v6.0/b2b/keys/renew HTTP/1.1
Content-Length: 2774
Content-Type: application/json
Host: collections.mp.microsoft.com

{
    "serviceTicket": "eyJ0eXAiOiJKV1QiLCJhb….",
    "Key": "eyJ0eXAiOiJKV1QiLCJhbG…."
}
```

## <a name="response"></a>response


### <a name="response-body"></a>Response body

| Paramètre | Type   | Description                                                                                                            |
|-----------|--------|------------------------------------------------------------------------------------------------------------------------|
| key       | string | Clé d’Microsoft Store actualisée qui peut être utilisée lors des appels ultérieurs à l’API de regroupements Microsoft Store ou à l’API d’achat. |


### <a name="response-example"></a>Exemple de réponse

```syntax
HTTP/1.1 200 OK
Content-Length: 1646
Content-Type: application/json
MS-CorrelationId: bfebe80c-ff89-4c4b-8897-67b45b916e47
MS-RequestId: 1b5fa630-d672-4971-b2c0-3713f4ea6c85
MS-CV: xu2HW6SrSkyfHyFh.0.0
MS-ServerId: 030011428
Date: Tue, 13 Sep 2015 07:31:12 GMT

{
    "key":"eyJ0eXAi….."
}
```

## <a name="error-codes"></a>Codes d’erreur


| Code | Error        | Code d’erreur interne           | Description   |
|------|--------------|----------------------------|---------------|
| 401  | Non autorisé | AuthenticationTokenInvalid | Le jeton d’accès Azure AD n’est pas valide. Dans certains cas, les détails de l’erreur ServiceError contiennent plus d’informations, par exemple lorsque le jeton est arrivé à expiration ou que la revendication *appid* est manquante. |
| 401  | Non autorisé | InconsistentClientId       | La revendication *ClientID* dans la clé d’ID de Microsoft Store et la revendication *AppID* dans le jeton d’accès Azure ad ne correspondent pas.                                                                     |


## <a name="related-topics"></a>Rubriques connexes


* [Gérer les droits sur les produits à partir d’un service](view-and-grant-products-from-a-service.md)
* [Demander des produits](query-for-products.md)
* [Signaler le traitement de la commande d’un produit consommable](report-consumable-products-as-fulfilled.md)
* [Octroyer des produits gratuits](grant-free-products.md)
