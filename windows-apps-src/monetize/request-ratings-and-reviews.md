---
Description: Découvrez les différentes façons dont vous pouvez permettre aux clients d’évaluer et de passer en revue votre application par programmation.
title: Demander des évaluations et des révisions pour votre application
ms.date: 01/22/2019
ms.topic: article
keywords: Windows 10, UWP, évaluations, revues
ms.localizationpriority: medium
ms.openlocfilehash: c0a668ac66f48e386a6299a64e5bcc18cec4fccc
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158373"
---
# <a name="request-ratings-and-reviews-for-your-app"></a>Demander des évaluations et des révisions pour votre application

Vous pouvez ajouter du code à votre application plateforme Windows universelle (UWP) pour inviter vos clients à évaluer ou à passer en revue votre application. Vous pouvez procéder de plusieurs façons :
* Vous pouvez afficher une boîte de dialogue d’évaluation et de révision directement dans le contexte de votre application.
* Vous pouvez ouvrir par programmation la page évaluation et vérification de votre application dans la Microsoft Store.

Lorsque vous êtes prêt à analyser vos évaluations et à consulter les données, vous pouvez afficher les données dans l’espace partenaires ou utiliser l’API Microsoft Store Analytics pour récupérer ces données par programme.

> [!IMPORTANT]
> Lorsque vous ajoutez une fonction d’évaluation au sein de votre application, toutes les révisions doivent envoyer l’utilisateur aux mécanismes d’évaluation de la boutique, quelle que soit l’évaluation de l’étoile choisie. Si vous collectez des commentaires ou des commentaires d’utilisateurs, il doit être clair qu’il n’est pas lié à l’évaluation des applications ou aux révisions dans le magasin, mais qu’il est envoyé directement au développeur de l’application. Pour plus d’informations sur les [activités frauduleuses ou malhonnêtes](/legal/windows/agreements/store-developer-code-of-conduct#3-fraudulent-or-dishonest-activities), consultez Code de réalisation du développeur.

## <a name="show-a-rating-and-review-dialog-in-your-app"></a>Afficher une boîte de dialogue d’évaluation et de vérification dans votre application

Pour afficher par programme une boîte de dialogue de votre application qui demande à votre client d’évaluer votre application et de soumettre une revue, appelez la méthode [RequestRateAndReviewAppAsync](/uwp/api/windows.services.store.storecontext.requestrateandreviewappasync) dans l’espace de noms [Windows. services. Store](/uwp/api/windows.services.store) . 

> [!IMPORTANT]
> La demande d’affichage de la boîte de dialogue évaluation et examen doit être appelée sur le thread d’interface utilisateur dans votre application.

```csharp
using Windows.ApplicationModel.Store;

private StoreContext _storeContext;

public async Task Initialize()
{
    if (App.IsMultiUserApp) // pseudo-code
    {
        IReadOnlyList<User> users = await User.FindAllAsync();
        User firstUser = users[0];
        _storeContext = StoreContext.GetForUser(firstUser);
    }
    else
    {
        _storeContext = StoreContext.GetDefault();
    }
}

private async Task PromptUserToRateApp()
{
    // Check if we’ve recently prompted user to review, we don’t want to bother user too often and only between version changes
    if (HaveWePromptedUserInPastThreeMonths())  // pseudo-code
    {
        return;
    }

    StoreRateAndReviewResult result = await 
        _storeContext.RequestRateAndReviewAppAsync();

    // Check status
    switch (result.Status)
    { 
        case StoreRateAndReviewStatus.Succeeded:
            // Was this an updated review or a new review, if Updated is false it means it was a users first time reviewing
            if (result.UpdatedExistingRatingOrReview)
            {
                // This was an updated review thank user
                ThankUserForReview(); // pseudo-code
            }
            else
            {
                // This was a new review, thank user for reviewing and give some free in app tokens
                ThankUserForReviewAndGrantTokens(); // pseudo-code
            }
            // Keep track that we prompted user and don’t do it again for a while
            SetUserHasBeenPrompted(); // pseudo-code
            break;

        case StoreRateAndReviewStatus.CanceledByUser:
            // Keep track that we prompted user and don’t prompt again for a while
            SetUserHasBeenPrompted(); // pseudo-code

            break;

        case StoreRateAndReviewStatus.NetworkError:
            // User is probably not connected, so we’ll try again, but keep track so we don’t try too often
            SetUserHasBeenPromptedButHadNetworkError(); // pseudo-code

            break;

        // Something else went wrong
        case StoreRateAndReviewStatus.OtherError:
        default:
            // Log error, passing in ExtendedJsonData however it will be empty for now
            LogError(result.ExtendedError, result.ExtendedJsonData); // pseudo-code
            break;
    }
}
```

La méthode **RequestRateAndReviewAppAsync** a été introduite dans Windows 10, version 1809, et elle ne peut être utilisée que dans les projets qui ciblent la **mise à jour 2018 d’octobre de windows 10 (10,0 ; Build 17763)** ou une version ultérieure dans Visual Studio.

### <a name="response-data-for-the-rating-and-review-request"></a>Données de réponse pour la demande d’évaluation et de vérification

Une fois que vous avez envoyé la demande pour afficher la boîte de dialogue évaluation et examen, la propriété [ExtendedJsonData](/uwp/api/windows.services.store.storerateandreviewresult.extendedjsondata) de la classe [StoreRateAndReviewResult](/uwp/api/windows.services.store.storerateandreviewresult) contient une chaîne au format JSON qui indique si la demande a réussi.

L’exemple suivant illustre la valeur de retour de cette demande après que le client a envoyé une évaluation ou une évaluation.

```json
{ 
  "status": "success", 
  "data": {
    "updated": false
  },
  "errorDetails": "Success"
}
```

L’exemple suivant illustre la valeur de retour de cette demande après que le client a choisi de ne pas soumettre une évaluation ou une révision.

```json
{ 
  "status": "aborted", 
  "errorDetails": "Navigation was unsuccessful"
}
```

Le tableau suivant décrit les champs de la chaîne de données au format JSON.

| Champ          | Description                                                                                                                                   |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| *statut*       | Chaîne qui indique si le client a envoyé une évaluation ou une révision. Les valeurs prises en charge sont **réussite** et **abandon**. |
| *data*         | Objet qui contient une valeur booléenne unique nommée *updated*. Cette valeur indique si le client a mis à jour une évaluation ou une révision existante. L’objet de *données* est inclus uniquement dans les réponses de réussite. |
| *errorDetails* | Chaîne qui contient les détails de l’erreur de la demande.                                                                                     |

## <a name="launch-the-rating-and-review-page-for-your-app-in-the-store"></a>Lancer la page évaluation et vérification de votre application dans le Store

Si vous souhaitez ouvrir par programmation la page évaluation et vérification de votre application dans le magasin, vous pouvez utiliser la méthode [LaunchUriAsync](/uwp/api/windows.system.launcher.launchuriasync) avec le schéma d' ```ms-windows-store://review``` URI, comme illustré dans cet exemple de code.

```csharp
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-windows-store://review/?ProductId=9WZDNCRFHVJL"));
```

Pour plus d’informations, consultez [lancer l’application Microsoft Store](../launch-resume/launch-store-app.md).

## <a name="analyze-your-ratings-and-reviews-data"></a>Analysez vos évaluations et vos données de révision

Pour analyser les évaluations et examiner les données de vos clients, vous avez le choix entre plusieurs options :
* Vous pouvez utiliser le rapport [évaluations](../publish/reviews-report.md) dans l’espace partenaires pour afficher les évaluations et les évaluations de vos clients. Vous pouvez également télécharger ce rapport pour l’afficher hors connexion.
* Vous pouvez utiliser les méthodes [obtenir les évaluations des applications](get-app-ratings.md) et obtenir des revues d' [applications](get-app-reviews.md) dans l’API Windows Store Analytics pour récupérer par programmation les évaluations et les révisions de vos clients au format JSON.

## <a name="related-topics"></a>Rubriques connexes

* [Envoyer des requêtes au Microsoft Store](send-requests-to-the-store.md)
* [Lancer l’application Microsoft Store](../launch-resume/launch-store-app.md)
* [Rapport sur les révisions](../publish/reviews-report.md)