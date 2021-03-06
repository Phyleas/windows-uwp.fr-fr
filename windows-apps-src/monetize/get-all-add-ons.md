---
ms.assetid: 7B6A99C6-AC86-41A1-85D0-3EB39A7211B6
description: Utilisez cette méthode dans l’API de soumission de Microsoft Store pour récupérer toutes les données d’un module complémentaire pour toutes les applications qui sont inscrits dans votre compte espace partenaires.
title: Obtenir toutes les extensions
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, API de soumission au Microsoft Store, extensions, produit in-app, PIA
ms.localizationpriority: medium
ms.openlocfilehash: 84b55ea8bc62955e3556fcff4e8d608738eb59ce
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334427"
---
# <a name="get-all-add-ons"></a>Obtenir toutes les extensions

Utilisez cette méthode dans l’API de soumission de Microsoft Store pour récupérer des données pour tous les modules complémentaires pour toutes les applications qui sont inscrits dans votre compte espace partenaires.

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission au Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Demande

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de requête                                                      |
|--------|------------------------------------------------------------------|
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/inappproducts` |


### <a name="request-header"></a>En-tête de requête

| Header        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

Tous les paramètres de la requête sont facultatifs pour cette méthode. Si vous appelez cette méthode sans paramètre, la réponse contient les données de toutes les extensions pour toutes les applications inscrites dans votre compte.

|  Paramètre  |  Type  |  Description  |  Obligatoire  |
|------|------|------|------|
|  top  |  entier  |  Nombre d’éléments à retourner dans la requête (autrement dit, nombre d’extensions à retourner). Si votre compte comporte plus d’extensions que la valeur que vous spécifiez dans la requête, le corps de la réponse comprend un chemin d’URI relatif que vous pouvez ajouter à l’URI de la méthode pour solliciter la page suivante de données.  |  Non  |
|  skip  |  entier  |  Nombre d’éléments à ignorer dans la requête avant de retourner les éléments restants. Utilisez ce paramètre pour parcourir des ensembles de données. Par exemple, top=10 et skip=0 permettent de récupérer les éléments 1 à 10, top=10 et skip=10 permettent de récupérer les éléments 11 à 20, et ainsi de suite.  |  Non  |


### <a name="request-body"></a>Corps de la requête

Ne fournissez pas de corps de requête pour cette méthode.

### <a name="request-examples"></a>Exemples de demande

L’exemple suivant montre comment récupérer toutes les données d’extension de toutes les applications inscrites dans votre compte.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts HTTP/1.1
Authorization: Bearer <your access token>
```

L’exemple suivant montre comment récupérer les 10 premières extensions uniquement.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts?top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

L’exemple suivant montre le corps de réponse JSON renvoyé par une requête réussie sur les 5 premières extensions inscrites dans un compte de développeur qui en possède 1072 au total. Pour des raisons de concision, cet exemple affiche uniquement les données des deux premières extensions retournées par la requête. Pour plus d’informations sur les valeurs figurant dans le corps de réponse, voir la section suivante.

```json
{
  "@nextLink": "inappproducts/?skip=5&top=5",
  "value": [
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
      "productId": "a8b8310b-fa8d-4da0-aca0-577ef6dce4dd",
      "productType": "Consumable",
      "pendingInAppProductSubmission": {
        "id": "1152921504621243619",
        "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243619"
      },
      "lastPublishedInAppProductSubmission": {
        "id": "1152921504621243705",
        "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243705"
      }
    },
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
      "id": "9NBLGGH4TNMN",
      "productId": "6a3c9788-a350-448a-bd32-16160a13018a",
      "productType": "Consumable",
      "pendingInAppProductSubmission": {
        "id": "1152921504621243538",
        "resourceLocation": "inappproducts/9NBLGGH4TNMN/submissions/1152921504621243538"
      },
      "lastPublishedInAppProductSubmission": {
        "id": "1152921504621243106",
        "resourceLocation": "inappproducts/9NBLGGH4TNMN/submissions/1152921504621243106"
      }
    },

  // Other add-ons omitted for brevity...
  ],
  "totalCount": 1072
}
```

### <a name="response-body"></a>Corps de la réponse

| Value      | Type   | Description                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| @nextLink  | chaîne | S’il existe des pages supplémentaires de données, cette chaîne contient un chemin relatif que vous pouvez ajouter à l’URI de requête `https://manage.devcenter.microsoft.com/v1.0/my/` de base pour solliciter la page suivante de données. Par exemple, si le paramètre *top* du corps de requête initial a la valeur 10, mais qu’il existe 100 extensions inscrites dans votre compte, le corps de réponse comprendra une valeur  @nextLink de `inappproducts?skip=10&top=10`, ce qui indique que vous pouvez appeler `https://manage.devcenter.microsoft.com/v1.0/my/inappproducts?skip=10&top=10` pour solliciter les 10 extensions suivantes. |
| valeur            | tableau  |  Tableau contenant des objets qui fournissent des informations sur chaque extension. Pour plus d’informations, voir [ressource d’extension](manage-add-ons.md#add-on-object).   |
| totalCount   | entier  | Nombre d’objets d’application dans le tableau *value* du corps de réponse.     |


## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Error code |  Description   |
|--------|------------------|
| 404  | Aucune extension n’a été trouvée. |
| 409  | Les applications ou les modules complémentaires utilisent les fonctionnalités de partenaires qui sont [actuellement ne pas pris en charge par l’API de soumission de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |


## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gérer les envois de module complémentaire](manage-add-on-submissions.md)
* [Obtenir un module complémentaire](get-an-add-on.md)
* [Créer un module complémentaire](create-an-add-on.md)
* [Supprimer un module complémentaire](delete-an-add-on.md)
