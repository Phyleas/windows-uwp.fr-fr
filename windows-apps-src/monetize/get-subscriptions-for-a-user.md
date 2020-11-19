---
ms.assetid: 94B5B2E9-BAEE-4B7F-BAF1-DA4D491427D7
description: Utilisez cette méthode dans l’API d’achat Microsoft Store pour obtenir les abonnements qu’un utilisateur donné a des droits d’utiliser.
title: Obtenir les abonnements d’un utilisateur
ms.date: 07/10/2018
ms.topic: article
keywords: API d’achat Windows 10, UWP Microsoft Store, abonnements
ms.localizationpriority: medium
ms.openlocfilehash: c77218cc7157e3d9a5508146395b284b57397d0f
ms.sourcegitcommit: 2a23972e9a0807256954d6da5cf21d0bbe7afb0a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/19/2020
ms.locfileid: "94941795"
---
# <a name="get-subscriptions-for-a-user"></a>Obtenir les abonnements d’un utilisateur

Utilisez cette méthode dans l’API d’achat Microsoft Store pour obtenir les modules complémentaires d’abonnement qu’un utilisateur donné a des droits d’utiliser.

> [!NOTE]
> Cette méthode peut être utilisée uniquement par les comptes de développeurs qui ont été approvisionnés par Microsoft pour être en mesure de créer des modules complémentaires d’abonnement pour les applications plateforme Windows universelle (UWP). Les modules complémentaires d’abonnement ne sont actuellement pas disponibles pour la plupart des comptes de développeur.

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez disposer des éléments suivants :

* Jeton d’accès Azure AD qui a la valeur d’URI de l’audience `https://onestore.microsoft.com` .
* Clé d’ID de Microsoft Store qui représente l’identité de l’utilisateur dont vous souhaitez obtenir les abonnements.

Pour plus d’informations, consultez [gérer les habilitations de produit à partir d’un service](view-and-grant-products-from-a-service.md).

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de demande                                            |
|--------|--------------------------------------------------------|
| POST   | ```https://purchase.mp.microsoft.com/v8.0/b2b/recurrences/query``` |


### <a name="request-header"></a>En-tête de requête

| En-tête         | Type   | Description      |
|----------------|--------|-------------------|
| Autorisation  | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;.                           |
| Host           | string | Doit être défini sur la valeur **Purchase.MP.Microsoft.com**.                                            |
| Content-Length | nombre | Longueur du corps de la demande.                                                                       |
| Content-Type   | string | Spécifie le type de requête et de réponse. Actuellement, la seule valeur prise en charge est **application/json**. |


### <a name="request-body"></a>Corps de la demande

| Paramètre      | Type   | Description     | Obligatoire |
|----------------|--------|-----------------|----------|
| b2bKey         | string | [Clé d’ID de Microsoft Store](view-and-grant-products-from-a-service.md#step-4) qui représente l’identité de l’utilisateur dont vous souhaitez obtenir les abonnements.  | Oui      |
| continuationToken |  string     |  Si l’utilisateur a des droits à plusieurs abonnements, le corps de la réponse retourne un jeton de continuation lorsque la limite de page est atteinte. Indiquez ce jeton de continuation ici dans les appels ultérieurs pour récupérer les produits restants.    | Non      |
| pageSize       | string | Nombre maximal d’abonnements à retourner dans une réponse. La valeur par défaut est 25.     |  Non      |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment utiliser cette méthode pour obtenir les modules complémentaires d’abonnement qu’un utilisateur donné a des droits d’utiliser. Remplacez la valeur *b2bKey* par la [clé d’ID de Microsoft Store](view-and-grant-products-from-a-service.md#step-4) qui représente l’identité de l’utilisateur dont vous souhaitez obtenir les abonnements.

```json
POST https://purchase.mp.microsoft.com/v8.0/b2b/recurrences/query HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
Host: purchase.mp.microsoft.com

{
  "b2bKey":  "eyJ0eXAiOiJ..."
}
```


## <a name="response"></a>response

Cette méthode retourne un corps de réponse JSON qui contient une collection d’objets de données qui décrivent les modules complémentaires d’abonnement que l’utilisateur a des droits d’utiliser. L’exemple suivant illustre le corps de la réponse pour un utilisateur qui a le droit d’un abonnement.

```json
{
  "items": [
    {
      "autoRenew":true,
      "beneficiary":"pub:gFVuEBiZHPXonkYvtdOi+tLE2h4g2Ss0ZId0RQOwzDg=",
      "expirationTime":"2017-06-11T03:07:49.2552941+00:00",
      "id":"mdr:0:bc0cb6960acd4515a0e1d638192d77b7:77d5ebee-0310-4d23-b204-83e8613baaac",
      "lastModified":"2017-01-08T21:07:51.1459644+00:00",
      "market":"US",
      "productId":"9NBLGGH52Q8X",
      "skuId":"0024",
      "startTime":"2017-01-10T21:07:49.2552941+00:00",
      "recurrenceState":"Active"
    }
  ]
}
```


### <a name="response-body"></a>Response body

Le corps de la réponse contient les données suivantes.

| Valeur        | Type   | Description            |
|---------------|--------|---------------------|
| items | tableau | Tableau d’objets qui contiennent les données relatives à chaque module complémentaire d’abonnement que l’utilisateur spécifié a le droit d’utiliser. Pour plus d’informations sur les données de chaque objet, consultez le tableau suivant.  |


Chaque objet dans le tableau d' *éléments* contient les valeurs suivantes.

| Valeur        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------|
| Renouvellement autorenouvelé | Boolean |  Indique si l’abonnement est configuré pour renouveler automatiquement à la fin de la période d’abonnement en cours.   |
| beneficiary | string |  ID du bénéficiaire du droit associé à cet abonnement.   |
| expirationTime | string | Date et heure d’expiration de l’abonnement, au format ISO 8601. Ce champ est disponible uniquement lorsque l’abonnement est dans certains États. Le délai d’expiration indique généralement le moment où l’état actuel arrive à expiration. Par exemple, pour un abonnement actif, la date d’expiration indique à quel moment le prochain renouvellement automatique aura lieu.    |
| expirationTimeWithGrace | string | Date et heure d’expiration de l’abonnement, y compris la période de grâce au format ISO 8601. Cette valeur indique quand l’utilisateur perd l’accès à l’abonnement après l’échec du renouvellement automatique de l’abonnement.    |
| id | string |  ID de l'abonnement. Utilisez cette valeur pour indiquer l’abonnement que vous souhaitez modifier lorsque vous appelez la méthode [modifier l’état de facturation d’un abonnement pour une méthode utilisateur](change-the-billing-state-of-a-subscription-for-a-user.md) .    |
| isTrial | Boolean |  Indique si l’abonnement est un essai.     |
| lastModified | string |  Date et heure de la dernière modification de l’abonnement, au format ISO 8601.      |
| market | string | Code du pays (au format ISO 3166-1 alpha-2 à deux lettres) dans lequel l’utilisateur a acquis l’abonnement.      |
| productId | string |  [ID de magasin](in-app-purchases-and-trials.md#store-ids) du [produit](in-app-purchases-and-trials.md#products-skus-and-availabilities) qui représente le module complémentaire d’abonnement dans le catalogue de Microsoft Store. Un exemple d’ID de magasin pour un produit est 9NBLGGH42CFD.     |
| skuId | string |  [ID de magasin](in-app-purchases-and-trials.md#store-ids) de la [référence SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities) qui représente le module complémentaire d’abonnement du catalogue Microsoft Store. Un exemple d’ID de magasin pour une référence (SKU) est 0,010.    |
| startTime | string |  Date et heure de début de l’abonnement, au format ISO 8601.     |
| recurrenceState | string  |  Une des valeurs suivantes :<ul><li>**Aucun**: &nbsp; &nbsp; cela indique un abonnement perpétuel.</li><li>**Actif**: &nbsp; &nbsp; l’abonnement est actif et l’utilisateur est autorisé à utiliser les services.</li><li>**Inactif**: &nbsp; &nbsp; l’abonnement est passé à la date d’expiration et l’utilisateur a désactivé l’option de renouvellement automatique pour l’abonnement.</li><li>**Annulé**: &nbsp; &nbsp; l’abonnement a été volontairement arrêté avant la date d’expiration, avec ou sans remboursement.</li><li>**InDunning** Inversion : &nbsp; &nbsp; l’abonnement se trouve dans *rappels* (autrement dit, l’abonnement est proche de l’expiration et Microsoft tente d’acquérir des fonds pour renouveler automatiquement l’abonnement).</li><li>**Échec**: &nbsp; &nbsp; la période rappels est terminée et l’abonnement n’a pas pu être renouvelé après plusieurs tentatives.</li></ul><p>**Remarque :**</p><ul><li>**Inactif** / **Annulé** / **Failed** est un état terminal. Lorsqu’un abonnement passe à l’un de ces États, l’utilisateur doit reacheter l’abonnement pour l’activer à nouveau. L’utilisateur n’est pas autorisé à utiliser les services dans ces États.</li><li>Lorsqu’un abonnement est **annulé**, le ExpirationTime est mis à jour avec la date et l’heure de l’annulation.</li><li>L’ID de l’abonnement restera le même pendant toute sa durée de vie. Elle ne change pas si l’option de renouvellement automatique est activée ou désactivée. Si un utilisateur achète un abonnement après avoir atteint un état terminal, un nouvel ID d’abonnement est créé.</li><li>L’ID d’un abonnement doit être utilisé pour exécuter une opération sur un abonnement individuel.</li><li>Lorsqu’un utilisateur achète un abonnement après l’avoir annulé ou interrompu, si vous interrogez les résultats de l’utilisateur, vous obtiendrez deux entrées : l’une avec l’ancien ID d’abonnement dans un état terminal et l’autre avec le nouvel ID d’abonnement dans un état actif.</li><li>Il est toujours recommandé de vérifier à la fois recurrenceState et expirationTime, car les mises à jour de recurrenceState peuvent potentiellement être retardées de quelques minutes (ou parfois des heures).       |
| cancellationDate | string   |  Date et heure de l’annulation de l’abonnement de l’utilisateur, au format ISO 8601.     |


## <a name="related-topics"></a>Rubriques connexes

* [Gérer les droits sur les produits à partir d’un service](view-and-grant-products-from-a-service.md)
* [Modifier l’état de facturation de l’abonnement d’un utilisateur](change-the-billing-state-of-a-subscription-for-a-user.md)
* [Demander des produits](query-for-products.md)
* [Signaler le traitement de la commande d’un produit consommable](report-consumable-products-as-fulfilled.md)
* [Renouveler une clé d’ID du Microsoft Store](renew-a-windows-store-id-key.md)
