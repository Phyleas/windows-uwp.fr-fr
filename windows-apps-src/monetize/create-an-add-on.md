---
author: mcleanbyron
ms.assetid: 5BD650D2-AA26-4DE9-8243-374FDB7D932B
description: "Utilisez cette méthode dans l’API de soumission du Windows Store pour créer une extension pour une app. inscrite dans votre compte du Centre de dév. Windows."
title: "Créer une extension à l’aide de l’API de soumission du Windows Store"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 11cf25fbeacbe3145c9cc3f4a80bdcce3028bf55

---

# Créer une extension à l’aide de l’API de soumission du Windows Store




Utilisez cette méthode dans l’API de soumission du Windows Store pour créer une extension (également connue sous le nom PIA, produit in-app) pour une application inscrite dans votre compte du Centre de développement Windows.

>**Remarque**&nbsp;&nbsp;Cette méthode permet de créer une extension sans soumission. Pour créer une soumission pour une extension, voir les méthodes décrites dans l’article [Gérer les soumissions d’extensions](manage-add-on-submissions.md).

## Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission du Windows Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous disposez de 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

>**Remarque**&nbsp;&nbsp;Cette méthode ne peut être utilisée que pour les comptes du Centre de développement Windows qui ont reçu l’autorisation d’utiliser l’API de soumission du Windows Store. Tous les comptes ne bénéficient pas de cette autorisation.

## Requête

Cette méthode présente la syntaxe suivante. Voir les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de la requête                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts``` |

<span/>
 

### En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |

<span/>

### Corps de la requête

Le corps de la requête contient les paramètres suivants.
 
|  Paramètre  |  Type  |  Description  |  Obligatoire  |
|------|------|------|------|
|  applicationIds  |  tableau  |  Tableau qui contient l’ID Windows Store de l’application à laquelle cette extension est associée. Un seul élément est pris en charge dans ce tableau.   |  Oui  |
|  productId  |  chaîne  |  ID de produit de l’extension. Il s’agit d’un identificateur que vous pouvez utiliser dans le code pour faire référence à l’extension. Pour plus d’informations, consultez [Définir le type et l’ID de votre produit](https://msdn.microsoft.com/windows/uwp/publish/set-your-iap-product-id).  |  Oui  |
|  productType  |  chaîne  |  Type de produit de l’extension. Les valeurs suivantes sont prises en charge: **Durable** et **Consommable**.  |  Oui  |

<span/>

### Exemple de requête

L’exemple suivant montre comment créer une extension consommable pour une application.

```syntax
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1Q...
Content-Type: application/json
{
    "applicationIds": [  "9NBLGGH4R315"  ],
    "productId": "my-new-add-on",
    "productType": "Consumable",
}
```

## Réponse

L’exemple suivant illustre le corps de réponse JSON d’un appel réussi à cette méthode. Pour plus d’informations sur les valeurs figurant dans le corps de réponse, voir [ressource d’extension](manage-add-ons.md#add-on-object).

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

## Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Code d’erreur |  Description                                                                                                                                                                           |
|--------|------------------|
| 400  | La requête n’est pas valide. |
| 409  | L’extension n’a pas pu être créée en raison de son état actuel ou elle utilise une fonctionnalité du tableau de bord du Centre de développement qui n’est [actuellement pas prise en charge par l’API de soumission du Windows Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   

<span/>

## Rubriques connexes

* [Créer et gérer des soumissions à l’aide des services du Windows Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gérer les soumissions d’extensions](manage-add-on-submissions.md)
* [Obtenir toutes les extensions](get-all-add-ons.md)
* [Obtenir une extension](get-an-add-on.md)
* [Supprimer une extension](delete-an-add-on.md)



<!--HONumber=Aug16_HO5-->

