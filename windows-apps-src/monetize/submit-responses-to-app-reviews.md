---
ms.assetid: 038903d6-efab-4da6-96b5-046c7431e6e7
description: Utilisez cette méthode dans l’API Microsoft Store revues pour envoyer des réponses aux révisions de votre application.
title: Soumettre des réponses aux avis
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, services Store, API Microsoft Store revues, acquisitions complémentaires
ms.localizationpriority: medium
ms.openlocfilehash: 9cf62f5f619eba0431f1398a391eef03b47bb13d
ms.sourcegitcommit: 140bbbab0f863a7a1febee85f736b0412bff1ae7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91984635"
---
# <a name="submit-responses-to-reviews"></a>Soumettre des réponses aux avis


Utilisez cette méthode dans l’API de révisions de Microsoft Store pour répondre par programmation aux révisions de votre application. Lorsque vous appelez cette méthode, vous devez spécifier les ID des révisions auxquelles vous souhaitez répondre. Les ID de révision sont disponibles dans les données de réponse de la méthode [obtenir les révisions](get-app-reviews.md) de l’application dans l’API Microsoft Store Analytics et dans le [Téléchargement hors connexion](../publish/download-analytic-reports.md) du [rapport de révisions](../publish/reviews-report.md).

Lorsqu’un client soumet une revue, il peut choisir de ne pas recevoir de réponse à sa révision. Si vous essayez de répondre à une révision pour laquelle le client a choisi de ne pas recevoir de réponses, le corps de la réponse de cette méthode indique que la tentative de réponse a échoué. Avant d’appeler cette méthode, vous pouvez éventuellement déterminer si vous êtes autorisé à répondre à une révision donnée à l’aide de la méthode [obtenir des informations de réponse pour les révisions de l’application](get-response-info-for-app-reviews.md) .

> [!NOTE]
> Outre l’utilisation de cette méthode pour répondre par programmation aux révisions, vous pouvez également répondre aux révisions à [l’aide de l’espace partenaires](../publish/respond-to-customer-reviews.md).

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](respond-to-reviews-using-windows-store-services.md#prerequisites) pour l’API de révisions de Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](respond-to-reviews-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Obtenir les ID des révisions auxquelles vous souhaitez répondre. Les ID de révision sont disponibles dans les données de réponse de la méthode [obtenir les révisions](get-app-reviews.md) de l’application dans l’API Microsoft Store Analytics et dans le [Téléchargement hors connexion](../publish/download-analytic-reports.md) du [rapport de révisions](../publish/reviews-report.md).

## <a name="request"></a>Requête

### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de requête                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/reviews/responses``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la demande

Cette méthode n’a aucun paramètre de requête.


### <a name="request-body"></a>Corps de la demande

Le corps de la demande a les valeurs suivantes.

| Valeur        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------|
| Réponses | tableau | Tableau d’objets qui contiennent les données de réponse que vous souhaitez envoyer. Pour plus d’informations sur les données de chaque objet, consultez le tableau suivant. |


Chaque objet dans le tableau de *réponses* contient les valeurs suivantes.

| Valeur        | Type   | Description           |  Obligatoire  |
|---------------|--------|-----------------------------|-----|
| ApplicationId | string |  ID de magasin de l’application avec la révision à laquelle vous souhaitez répondre. L’ID du magasin est disponible sur la [page identité](../publish/view-app-identity-details.md) de l’application de l’espace partenaires. Exemple d’ID Windows Store : 9WZDNCRFJ3Q8.   |  Oui  |
| ReviewId | string |  ID de la révision à laquelle vous souhaitez répondre (il s’agit d’un GUID). Les ID de révision sont disponibles dans les données de réponse de la méthode [obtenir les révisions](get-app-reviews.md) de l’application dans l’API Microsoft Store Analytics et dans le [Téléchargement hors connexion](../publish/download-analytic-reports.md) du [rapport de révisions](../publish/reviews-report.md).   |  Oui  |
| ResponseText | string | La réponse que vous souhaitez envoyer. Votre réponse doit suivre [ces instructions](../publish/respond-to-customer-reviews.md#guidelines-for-responses).   |  Oui  |
| SupportEmail | string | L’adresse de messagerie du support technique de votre application, que le client peut utiliser pour vous contacter directement. Il doit s’agir d’une adresse de messagerie valide.     |  Oui  |
| IsPublic | Boolean |  Si vous spécifiez la **valeur true**, votre réponse sera affichée dans la liste des boutiques de votre application, directement sous la revue du client, et sera visible pour tous les clients. Si vous spécifiez **false** et que l’utilisateur n’a pas refusé de recevoir des réponses par courrier électronique, votre réponse sera envoyée au client par courrier électronique et ne sera pas visible pour les autres clients figurant dans la liste des boutiques de votre application. Si vous spécifiez **false** et que l’utilisateur a refusé de recevoir des réponses par courrier électronique, une erreur est retournée.   |  Oui  |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment utiliser cette méthode pour envoyer des réponses à plusieurs révisions.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/reviews/responses HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
{
  "Responses": [
    {
      "ApplicationId": "9WZDNCRFJ3Q8",
      "ReviewId": "6be543ff-1c9c-4534-aced-af8b4fbe0316",
      "ResponseText": "Thank you for pointing out this bug. I fixed it and published an update, you should have the fix soon",
      "SupportEmail": "support@contoso.com",
      "IsPublic": true
    },
    {
      "ApplicationId": "9NBLGGH1RP08",
      "ReviewId": "80c9671a-96c2-4278-bcbc-be0ce5a32a7c",
      "ResponseText": "Thank you for submitting your review. Can you tell more about what you were doing in the app when it froze? Thanks very much for your help.",
      "SupportEmail": "support@contoso.com",
      "IsPublic": false
    }
  ]
}
```

## <a name="response"></a>response

### <a name="response-body"></a>Response body

| Valeur        | Type   | Description            |
|---------------|--------|---------------------|
| Résultats | tableau | Tableau d’objets qui contiennent les données relatives à chaque réponse que vous avez envoyée. Pour plus d’informations sur les données de chaque objet, consultez le tableau suivant.  |


Chaque objet du tableau de *résultats* contient les valeurs suivantes.

| Valeur        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------|
| ApplicationId | string |  ID de magasin de l’application avec la révision à laquelle vous avez répondu. Exemple d’ID Windows Store : 9WZDNCRFJ3Q8.   |
| ReviewId | string |  ID de la révision à laquelle vous avez répondu. Il s’agit d’un GUID.   |
| Réussite | string | La valeur **true** indique que votre réponse a été envoyée avec succès. La valeur **false** indique que votre réponse a échoué.    |
| FailureReason | string | **En cas**de **réussite** , cette valeur contient la raison de l’échec. En cas de **réussite** , cette **valeur est vide**.      |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Result": [
    {
      "ApplicationId": "9WZDNCRFJ3Q8",
      "ReviewId": "6be543ff-1c9c-4534-aced-af8b4fbe0316",
      "Successful": "true",
      "FailureReason": ""
    },
    {
      "ApplicationId": "9NBLGGH1RP08",
      "ReviewId": "80c9671a-96c2-4278-bcbc-be0ce5a32a7c",
      "Successful": "false",
      "FailureReason": "No Permission"
    }
  ]
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Répondre aux évaluations des clients à l’aide de l’espace partenaires](../publish/respond-to-customer-reviews.md)
* [Répondre aux révisions à l’aide des services Microsoft Store](respond-to-reviews-using-windows-store-services.md)
* [Obtenir des informations de réponse pour les révisions d’application](get-response-info-for-app-reviews.md)
* [Obtenir les avis sur les applications](get-app-reviews.md)
