---
Description: Vous pouvez utiliser la méthode SendRequestAsync pour envoyer des requêtes à la Microsoft Store pour les opérations qui n’ont pas encore d’API disponible dans le SDK Windows.
title: Envoyer des demandes au Microsoft Store
ms.assetid: 070B9CA4-6D70-4116-9B18-FBF246716EF0
ms.date: 03/22/2018
ms.topic: article
keywords: Windows 10, UWP, StoreRequestHelper, SendRequestAsync
ms.localizationpriority: medium
ms.openlocfilehash: a02be93a56d6066ebd4d9547c8cc9ea1a96c9e09
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164493"
---
# <a name="send-requests-to-the-microsoft-store"></a>Envoyer des demandes au Microsoft Store

À compter de Windows 10, la version 1607, le SDK Windows fournit des API pour les opérations liées aux magasins (telles que les achats dans l’application) dans l’espace de noms [Windows. services. Store](/uwp/api/windows.services.store) . Toutefois, bien que les services qui prennent en charge le magasin soient constamment mis à jour, étendus et améliorés entre les versions du système d’exploitation, les nouvelles API sont généralement ajoutées au SDK Windows uniquement pendant les versions majeures du système d’exploitation.

Nous fournissons la méthode [SendRequestAsync](/uwp/api/windows.services.store.storerequesthelper.sendrequestasync) comme un moyen souple de rendre les nouvelles opérations de stockage disponibles pour les applications plateforme Windows universelle (UWP) avant la publication d’une nouvelle version du SDK Windows. Vous pouvez utiliser cette méthode pour envoyer des demandes au magasin pour les nouvelles opérations qui n’ont pas encore d’API correspondante disponible dans la dernière version de la SDK Windows.

> [!NOTE]
> La méthode **SendRequestAsync** est disponible uniquement pour les applications qui ciblent Windows 10, version 1607 ou ultérieure. Certaines des requêtes prises en charge par cette méthode sont prises en charge uniquement dans les versions ultérieures à Windows 10, version 1607.

**SendRequestAsync** est une méthode statique de la classe [StoreRequestHelper](/uwp/api/windows.services.store.storerequesthelper) . Pour appeler cette méthode, vous devez passer les informations suivantes à la méthode :
* Objet [StoreContext](/uwp/api/windows.services.store.storecontext) qui fournit des informations sur l’utilisateur pour lequel vous souhaitez effectuer l’opération. Pour plus d’informations sur cet objet, consultez [prise en main de la classe StoreContext](in-app-purchases-and-trials.md#get-started-with-the-storecontext-class).
* Entier qui identifie la demande que vous souhaitez envoyer au magasin.
* Si la requête prend en charge des arguments, vous pouvez également passer une chaîne au format JSON qui contient les arguments à passer avec la requête.

L'exemple suivant montre comment appeler cette méthode. Cet exemple requiert l’utilisation d’instructions pour les espaces de noms **Windows. services. Store** et **System. Threading. Tasks** .

```csharp
public async Task<bool> AddUserToFlightGroup()
{
    StoreSendRequestResult result = await StoreRequestHelper.SendRequestAsync(
        StoreContext.GetDefault(), 8,
        "{ \"type\": \"AddToFlightGroup\", \"parameters\": { \"flightGroupId\": \"your group ID\" } }");

    if (result.ExtendedError == null)
    {
        return true;
    }

    return false;
}
```

Consultez les sections suivantes pour plus d’informations sur les demandes actuellement disponibles via la méthode **SendRequestAsync** . Nous mettrons à jour cet article lorsque la prise en charge de nouvelles demandes est ajoutée.

## <a name="request-for-in-app-ratings-and-reviews"></a>Demande d’évaluations et de révisions dans l’application

Vous pouvez lancer par programmation une boîte de dialogue à partir de votre application qui demande à votre client d’évaluer votre application et d’envoyer une revue en transmettant l’entier de requête 16 à la méthode **SendRequestAsync** . Pour plus d’informations, consultez [afficher une boîte de dialogue d’évaluation et de révision dans votre application](request-ratings-and-reviews.md#show-a-rating-and-review-dialog-in-your-app).

## <a name="requests-for-flight-group-scenarios"></a>Demandes pour les scénarios de groupe de vols

> [!IMPORTANT]
> Toutes les demandes de groupe de vol décrites dans cette section ne sont actuellement pas disponibles pour la plupart des comptes de développeur. Ces requêtes échouent, sauf si votre compte de développeur est spécialement approvisionné par Microsoft.

La méthode **SendRequestAsync** prend en charge un ensemble de demandes pour les scénarios de groupe de vol, telles que l’ajout d’un utilisateur ou d’un appareil à un groupe de vol. Pour envoyer ces demandes, transmettez la valeur 7 ou 8 au paramètre *requestKind* avec une chaîne au format JSON au paramètre *parametersAsJson* qui indique la demande que vous souhaitez envoyer, ainsi que tous les arguments associés. Ces valeurs *requestKind* diffèrent des manières suivantes.

|  Valeur du type de demande  |  Description  |
|----------------------|---------------|
|  7                   |  Les requêtes sont exécutées dans le contexte de l’appareil actuel. Cette valeur ne peut être utilisée que sur Windows 10, version 1703 ou ultérieure.  |
|  8                   |  Les requêtes sont exécutées dans le contexte de l’utilisateur actuellement connecté au magasin. Cette valeur peut être utilisée sur Windows 10, version 1607 ou ultérieure.  |

Les demandes de groupe de vol suivantes sont actuellement implémentées.

### <a name="retrieve-remote-variables-for-the-highest-ranked-flight-group"></a>Récupérer les variables distantes pour le groupe de vol le plus classé

> [!IMPORTANT]
> Cette demande n’est actuellement pas disponible pour la plupart des comptes de développeur. Cette demande échouera, sauf si votre compte de développeur est spécialement approvisionné par Microsoft.

Cette requête récupère les variables distantes du groupe de vol le plus élevé pour l’utilisateur ou l’appareil actuel. Pour envoyer cette demande, transmettez les informations suivantes aux paramètres *requestKind* et *parametersAsJson* de la méthode **SendRequestAsync** .

|  Paramètre  |  Description  |
|----------------------|---------------|
|  *requestKind*                   |  Spécifiez 7 pour renvoyer le groupe de vol le plus élevé de l’appareil, ou spécifiez 8 pour retourner le groupe de vol le plus élevé pour l’utilisateur actuel et l’appareil. Nous vous recommandons d’utiliser la valeur 8 pour le paramètre *requestKind* , car cette valeur renverra le groupe de vols le plus élevé dans l’appartenance pour l’utilisateur actuel et l’appareil.  |
|  *parametersAsJson*                   |  Transmettez une chaîne au format JSON qui contient les données affichées dans l’exemple ci-dessous.  |

L’exemple suivant montre le format des données JSON à passer à *parametersAsJson*. Le champ de *type* doit être affecté à la chaîne *GetRemoteVariables*. Affectez le champ *ProjectId* à l’ID du projet dans lequel vous avez défini les variables distantes dans l’espace partenaires.

```json
{ 
    "type": "GetRemoteVariables", 
    "parameters": "{ \"projectId\": \"your project ID\" }" 
}
```

Une fois cette demande envoyée, la propriété [Response](/uwp/api/windows.services.store.storesendrequestresult.Response) de la valeur de retour [StoreSendRequestResult](/uwp/api/windows.services.store.storesendrequestresult) contient une chaîne au format JSON avec les champs suivants.

|  Champ  |  Description  |
|----------------------|---------------|
|  *façon*                   |  Valeur booléenne, où **true** indique que l’identité de l’utilisateur ou de l’appareil n’était pas présente dans la demande, et **false** indique que l’identité de l’utilisateur ou de l’appareil était présente dans la demande.  |
|  *name*                   |  Chaîne qui contient le nom du groupe de vols dont le classement est le plus élevé auquel appartient l’appareil ou l’utilisateur.  |
|  *settings*                   |  Dictionnaire de paires clé/valeur qui contiennent le nom et la valeur des variables distantes que le développeur a configurées pour le groupe de vol.  |

L’exemple suivant illustre une valeur de retour pour cette requête.

```json
{ 
  "anonymous": false, 
  "name": "Insider Slow",
  "settings":
  {
      "Audience": "Slow"
      ...
  }
}
```

### <a name="add-the-current-device-or-user-to-a-flight-group"></a>Ajouter l’appareil ou l’utilisateur actuel à un groupe de vol

> [!IMPORTANT]
> Cette demande n’est actuellement pas disponible pour la plupart des comptes de développeur. Cette demande échouera, sauf si votre compte de développeur est spécialement approvisionné par Microsoft.

Pour envoyer cette demande, transmettez les informations suivantes aux paramètres *requestKind* et *parametersAsJson* de la méthode **SendRequestAsync** .

|  Paramètre  |  Description  |
|----------------------|---------------|
|  *requestKind*                   |  Spécifiez 7 pour ajouter l’appareil à un groupe de vol, ou spécifiez 8 pour ajouter l’utilisateur actuellement connecté au magasin à un groupe de vol.  |
|  *parametersAsJson*                   |  Transmettez une chaîne au format JSON qui contient les données affichées dans l’exemple ci-dessous.  |

L’exemple suivant montre le format des données JSON à passer à *parametersAsJson*. Le champ de *type* doit être affecté à la chaîne *AddToFlightGroup*. Affectez le champ *flightGroupId* à l’ID du groupe de vols auquel vous souhaitez ajouter l’appareil ou l’utilisateur.

```json
{ 
    "type": "AddToFlightGroup", 
    "parameters": "{ \"flightGroupId\": \"your group ID\" }" 
}
```

En cas d’erreur avec la demande, la propriété [HttpStatusCode](/uwp/api/windows.services.store.storesendrequestresult.HttpStatusCode) de la valeur de retour [StoreSendRequestResult](/uwp/api/windows.services.store.storesendrequestresult) contient le code de réponse.

### <a name="remove-the-current-device-or-user-from-a-flight-group"></a>Supprimer l’appareil ou l’utilisateur actuel d’un groupe de vol

> [!IMPORTANT]
> Cette demande n’est actuellement pas disponible pour la plupart des comptes de développeur. Cette demande échouera, sauf si votre compte de développeur est spécialement approvisionné par Microsoft.

Pour envoyer cette demande, transmettez les informations suivantes aux paramètres *requestKind* et *parametersAsJson* de la méthode **SendRequestAsync** .

|  Paramètre  |  Description  |
|----------------------|---------------|
|  *requestKind*                   |  Spécifiez 7 pour supprimer l’appareil d’un groupe de vol, ou spécifiez 8 pour supprimer l’utilisateur actuellement connecté au magasin à partir d’un groupe de vol.  |
|  *parametersAsJson*                   |  Transmettez une chaîne au format JSON qui contient les données affichées dans l’exemple ci-dessous.  |

L’exemple suivant montre le format des données JSON à passer à *parametersAsJson*. Le champ de *type* doit être affecté à la chaîne *RemoveFromFlightGroup*. Affectez le champ *flightGroupId* à l’ID du groupe de vols duquel vous souhaitez supprimer l’appareil ou l’utilisateur.

```json
{ 
    "type": "RemoveFromFlightGroup", 
    "parameters": "{ \"flightGroupId\": \"your group ID\" }" 
}
```

En cas d’erreur avec la demande, la propriété [HttpStatusCode](/uwp/api/windows.services.store.storesendrequestresult.HttpStatusCode) de la valeur de retour [StoreSendRequestResult](/uwp/api/windows.services.store.storesendrequestresult) contient le code de réponse.

## <a name="related-topics"></a>Rubriques connexes

* [Afficher une boîte de dialogue d’évaluation et de vérification dans votre application](request-ratings-and-reviews.md#show-a-rating-and-review-dialog-in-your-app)
* [SendRequestAsync](/uwp/api/windows.services.store.storerequesthelper.sendrequestasync)