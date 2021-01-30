---
title: Interagir avec une application en arrière-plan dans Cortana-conception et développement UWP Cortana
description: Activez l’interaction utilisateur avec une application en arrière-plan, par le biais de la saisie vocale et texte dans le canevas Cortana, lors de l’exécution d’une commande vocale.
ms.assetid: e42917dc-aece-4880-813f-80b897f9126c
ms.date: 01/28/2021
ms.topic: article
keywords: cortana
ms.openlocfilehash: 111f945c548d34f614305e31c1c79ce9197f0925
ms.sourcegitcommit: d7efd35c1749f695aebbc0db99d8b62b70fb72da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99057758"
---
# <a name="interact-with-a-background-app-in-cortana"></a>Interagir avec une application en arrière-plan dans Cortana

>[!WARNING]
> Cette fonctionnalité n’est plus prise en charge à partir de la mise à jour Windows 10 2020 (version 2004, nom de nom « 20H1 »).

Activez l’interaction utilisateur avec une application en arrière-plan, par le biais de la saisie vocale et texte dans le canevas **Cortana** , lors de l’exécution d’une commande vocale.

> [!NOTE]
> **API importantes**
>
> - [**Windows. ApplicationModel. VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**Éléments et attributs d’un fichier VCD v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

Cortana prend en charge un flux de travail tour à tour complet avec votre application. Ce flux de travail est défini par votre application et peut prendre en charge des fonctionnalités telles que : 

- Opération réussie
- À la main
- Avancement
- Confirmation
- Lever les ambiguïtés
- Erreur

## <a name="composing-feedback-strings"></a>Composition de chaînes de commentaires

> [!TIP]
> **Conditions préalables**
>
> Si vous débutez dans le développement d’applications de plateforme Windows universelle (UWP), consultez les rubriques ci-dessous pour vous familiariser avec les technologies décrites ici.
>
> - [Créer votre première application](/windows/uwp/get-started/your-first-app)
> - Découvrir les événements avec [Vue d’ensemble des événements et des événements routés](/windows/uwp/xaml-platform/events-and-routed-events-overview).
>
> **Instructions relatives à l’expérience utilisateur**
>
> Pour plus d’informations sur l’intégration de votre application avec **Cortana** et les [interactions vocales](speech-interactions.md) , consultez [instructions de conception Cortana](cortana-design-guidelines.md) pour obtenir des conseils utiles sur la conception d’une application vocale utile et attrayante.

Composez les chaînes de commentaires qui sont affichées et parlées par **Cortana**.

Les [instructions de conception de Cortana](cortana-design-guidelines.md) fournissent des recommandations sur la composition de chaînes pour **Cortana**.

Les cartes de contenu peuvent fournir un contexte supplémentaire pour l’utilisateur et vous aider à conserver des chaînes de commentaires concises.

**Cortana** prend en charge les modèles de carte de contenu suivants (un seul modèle peut être utilisé sur l’écran de saisie semi-automatique) :

- Titre uniquement
- Titre avec jusqu’à trois lignes de texte
- Titre avec image
- Titre avec image et jusqu’à trois lignes de texte

L’image peut être :

- 68W x 68h
- 68W x 92h
- 280W x 140h

Vous pouvez également permettre aux utilisateurs de lancer votre application au premier plan en cliquant sur une carte ou le lien de texte vers votre application.

## <a name="completion-screen"></a>Écran de saisie semi-automatique

Un écran de saisie semi-automatique fournit à l’utilisateur des informations sur la tâche de commande vocale terminée.

Les tâches qui prennent moins de 500 millisecondes pour répondre à votre application et qui ne nécessitent aucune information supplémentaire de la part de l’utilisateur sont terminées sans interaction avec  **Cortana**. Cortana affiche simplement l’écran de saisie semi-automatique.

Ici, nous utilisons l’application **Adventure Works** pour afficher l’écran de saisie semi-automatique d’une demande de commande vocale afin d’afficher les prochains voyages à Londres.

:::image type="content" source="images/cortana/cortana-completion-screen-upcomingtrip-small.png" alt-text="Capture d’écran de l’exécution d’une application en arrière-plan Cortana pour un prochain voyage":::

La commande vocale est définie dans AdventureWorksCommands.xml :

```xml
<Command Name="whenIsTripToDestination">
  <Example> When is my trip to Las Vegas?</Example>
  <ListenFor RequireAppName="BeforeOrAfterPhrase"> when is [my] trip to {destination}</ListenFor>
  <ListenFor RequireAppName="ExplicitlySpecified"> when is [my] {builtin:AppName} trip to {destination} </ListenFor>
  <Feedback> Looking for trip to {destination}</Feedback>
  <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
</Command>
```

AdventureWorksVoiceCommandService.cs contient la méthode de message d’achèvement :

```csharp
/// <summary>
/// Show details for a single trip, if the trip can be found. 
/// This demonstrates a simple response flow in Cortana.
/// </summary>
/// <param name="destination">The destination, expected to be in the phrase list.</param>
private async Task SendCompletionMessageForDestination(string destination)
{
    // If this operation is expected to take longer than 0.5 seconds, the task must
    // supply a progress response to Cortana before starting the operation, and
    // updates must be provided at least every 5 seconds.
    string loadingTripToDestination = string.Format(
               cortanaResourceMap.GetValue("LoadingTripToDestination", cortanaContext).ValueAsString,
               destination);
    await ShowProgressScreen(loadingTripToDestination);
    Model.TripStore store = new Model.TripStore();
    await store.LoadTrips();

    // Query for the specified trip. 
    // The destination should be in the phrase list. However, there might be  
    // multiple trips to the destination. We pick the first.
    IEnumerable<Model.Trip> trips = store.Trips.Where(p => p.Destination == destination);

    var userMessage = new VoiceCommandUserMessage();
    var destinationsContentTiles = new List<VoiceCommandContentTile>();
    if (trips.Count() == 0)
    {
        string foundNoTripToDestination = string.Format(
               cortanaResourceMap.GetValue("FoundNoTripToDestination", cortanaContext).ValueAsString,
               destination);
        userMessage.DisplayMessage = foundNoTripToDestination;
        userMessage.SpokenMessage = foundNoTripToDestination;
    }
    else
    {
        // Set plural or singular title.
        string message = "";
        if (trips.Count() > 1)
        {
            message = cortanaResourceMap.GetValue("PluralUpcomingTrips", cortanaContext).ValueAsString;
        }
        else
        {
            message = cortanaResourceMap.GetValue("SingularUpcomingTrip", cortanaContext).ValueAsString;
        }
        userMessage.DisplayMessage = message;
        userMessage.SpokenMessage = message;

        // Define a tile for each destination.
        foreach (Model.Trip trip in trips)
        {
            int i = 1;
            
            var destinationTile = new VoiceCommandContentTile();

            destinationTile.ContentTileType = VoiceCommandContentTileType.TitleWith68x68IconAndText;
            destinationTile.Image = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png"));

            destinationTile.AppLaunchArgument = trip.Destination;
            destinationTile.Title = trip.Destination;
            if (trip.StartDate != null)
            {
                destinationTile.TextLine1 = trip.StartDate.Value.ToString(dateFormatInfo.LongDatePattern);
            }
            else
            {
                destinationTile.TextLine1 = trip.Destination + " " + i;
            }

            destinationsContentTiles.Add(destinationTile);
            i++;
        }
    }

    var response = VoiceCommandResponse.CreateResponse(userMessage, destinationsContentTiles);

    if (trips.Count() > 0)
    {
        response.AppLaunchArgument = destination;
    }

    await voiceServiceConnection.ReportSuccessAsync(response);
}
```

## <a name="hand-off-screen"></a>Écran de la main

Une fois qu’une commande vocale est reconnue, **Cortana** doit appeler ReportSuccessAsync et présenter des commentaires dans environ 500If. l’application App service ne peut pas terminer l’action spécifiée par la commande Voice dans les 500 ms, **Cortana** présente un écran main qui s’affiche jusqu’à ce que votre application appelle ReportSuccessAsync ou jusqu’à 5 secondes.

Si le service d’application n’appelle pas ReportSuccessAsync ou toute autre méthode VoiceCommandServiceConnection, l’utilisateur reçoit un message d’erreur et l’appel App service est annulé.

Voici un exemple d’un écran à part pour l’application **Adventure Works** . Dans cet exemple, un utilisateur a interrogé **Cortana** pour les prochains voyages. L’écran de la main contient un message personnalisé avec le nom de l’app service, une icône et une chaîne de **Commentaires** . 

[!NOTE] Vous pouvez déclarer une chaîne de **Commentaires** dans le fichier VCD. Cette chaîne n’affecte pas le texte de l’interface utilisateur affiché sur le canevas Cortana, elle affecte uniquement le texte parlé par **Cortana**.

:::image type="content" source="images/cortana/cortana-backgroundapp-progress-result.png" alt-text="Capture d’écran de l’écran WebPart de l’application en arrière-plan Cortana":::

## <a name="progress-screen"></a>Écran de progression

Si l’app service prend plus de 500 ms pour appeler ReportSuccessAsync, **Cortana** fournit à l’utilisateur un écran de progression. L’icône de l’application s’affiche, et vous devez fournir les chaînes de progression de l’interface graphique et du TTS pour indiquer que la tâche est activement gérée.

**Cortana** affiche un écran de progression pour un maximum de 5 secondes. Après 5 secondes, **Cortana** présente à l’utilisateur un message d’erreur et l’app service est fermé. Si le service d’application a besoin de plus de 5 secondes pour terminer l’action, il peut continuer à mettre à jour **Cortana** avec les écrans de progression.

Voici un exemple d’écran de progression pour l’application **Adventure Works** . Dans cet exemple, un utilisateur a annulé un voyage à Las Vegas. L’écran de progression comprend un message personnalisé pour l’action, une icône et une vignette de contenu avec des informations sur le trajet en cours d’annulation.

:::image type="content" source="images/cortana/cortana-progress-screen.png" alt-text="Capture d’écran de Cortana avec l’écran de progression des applications en arrière-plan":::

AdventureWorksVoiceCommandService.cs contient la méthode de message de progression suivante, qui appelle [**ReportProgressAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) pour afficher l’écran de progression dans **Cortana**.

```    CSharp
/// <summary>
/// Show a progress screen. These should be posted at least every 5 seconds for a 
/// long-running operation.
/// </summary>
/// <param name="message">The message to display, relating to the task being performed.</param>
/// <returns></returns>
private async Task ShowProgressScreen(string message)
{
    var userProgressMessage = new VoiceCommandUserMessage();
    userProgressMessage.DisplayMessage = userProgressMessage.SpokenMessage = message;

    VoiceCommandResponse response = VoiceCommandResponse.CreateResponse(userProgressMessage);
    await voiceServiceConnection.ReportProgressAsync(response);
}
```

## <a name="confirmation-screen"></a>Écran de confirmation

Lorsqu’une action spécifiée par une commande vocale est irréversible, a un impact significatif ou la confiance de reconnaissance n’est pas élevée, un service d’application peut demander une confirmation.

Voici un exemple d’écran de confirmation pour l’application **Adventure Works** . Dans cet exemple, un utilisateur a demandé à App service d’annuler un voyage à Las Vegas via **Cortana**. App service a fourni à **Cortana** un écran de confirmation qui invite l’utilisateur à entrer une réponse oui ou non avant d’annuler le trajet.

Si l’utilisateur dit autre chose que « oui » ou « non », **Cortana** ne peut pas déterminer la réponse à la question. Dans ce cas, **Cortana** invite l’utilisateur à une question similaire fournie par app service.

À la deuxième invite, si l’utilisateur n’indique toujours pas « oui » ou « non », **Cortana** invite l’utilisateur une troisième fois avec la même question précédée d’un excuses. Si l’utilisateur n’indique toujours pas « oui » ou « non », **Cortana** cesse d’écouter l’entrée vocale et invite l’utilisateur à appuyer sur l’un des boutons à la place.

L’écran de confirmation comprend un message personnalisé pour l’action, une icône et une vignette de contenu avec des informations sur le trajet en cours d’annulation.

:::image type="content" source="images/cortana/cortana-confirmation-screen.png" alt-text="Capture d’écran de Cortana avec l’écran de confirmation de l’application en arrière-plan":::

AdventureWorksVoiceCommandService.cs contient la méthode d’annulation de voyage suivante, qui appelle [**RequestConfirmationAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) pour afficher un écran de confirmation dans **Cortana**.

```    CSharp
/// <summary>
/// Handle the Trip Cancellation task. This task demonstrates how to prompt a user
/// for confirmation of an operation, show users a progress screen while performing
/// a long-running task, and show a completion screen.
/// </summary>
/// <param name="destination">The name of a destination.</param>
/// <returns></returns>
private async Task SendCompletionMessageForCancellation(string destination)
{
    // Begin loading data to search for the target store. 
    // Consider inserting a progress screen here, in order to prevent Cortana from timing out. 
    string progressScreenString = string.Format(
        cortanaResourceMap.GetValue("ProgressLookingForTripToDest", cortanaContext).ValueAsString,
        destination);
    await ShowProgressScreen(progressScreenString);

    Model.TripStore store = new Model.TripStore();
    await store.LoadTrips();

    IEnumerable<Model.Trip> trips = store.Trips.Where(p => p.Destination == destination);
    Model.Trip trip = null;
    if (trips.Count() > 1)
    {
        // If there is more than one trip, provide a disambiguation screen.
        // However, if a significant number of items are returned, you might want to 
        // just display a link to your app and provide a deeper search experience.
        string disambiguationDestinationString = string.Format(
            cortanaResourceMap.GetValue("DisambiguationWhichTripToDest", cortanaContext).ValueAsString,
            destination);
        string disambiguationRepeatString = cortanaResourceMap.GetValue("DisambiguationRepeat", cortanaContext).ValueAsString;
        trip = await DisambiguateTrips(trips, disambiguationDestinationString, disambiguationRepeatString);
    }
    else
    {
        trip = trips.FirstOrDefault();
    }

    var userPrompt = new VoiceCommandUserMessage();
    
    VoiceCommandResponse response;
    if (trip == null)
    {
        var userMessage = new VoiceCommandUserMessage();
        string noSuchTripToDestination = string.Format(
            cortanaResourceMap.GetValue("NoSuchTripToDestination", cortanaContext).ValueAsString,
            destination);
        userMessage.DisplayMessage = userMessage.SpokenMessage = noSuchTripToDestination;

        response = VoiceCommandResponse.CreateResponse(userMessage);
        await voiceServiceConnection.ReportSuccessAsync(response);
    }
    else
    {
        // Prompt the user for confirmation that this is the correct trip to cancel.
        string cancelTripToDestination = string.Format(
            cortanaResourceMap.GetValue("CancelTripToDestination", cortanaContext).ValueAsString,
            destination);
        userPrompt.DisplayMessage = userPrompt.SpokenMessage = cancelTripToDestination;
        var userReprompt = new VoiceCommandUserMessage();
        string confirmCancelTripToDestination = string.Format(
            cortanaResourceMap.GetValue("ConfirmCancelTripToDestination", cortanaContext).ValueAsString,
            destination);
        userReprompt.DisplayMessage = userReprompt.SpokenMessage = confirmCancelTripToDestination;
        
        response = VoiceCommandResponse.CreateResponseForPrompt(userPrompt, userReprompt);

        var voiceCommandConfirmation = await voiceServiceConnection.RequestConfirmationAsync(response);

        // If RequestConfirmationAsync returns null, Cortana has likely been dismissed.
        if (voiceCommandConfirmation != null)
        {
            if (voiceCommandConfirmation.Confirmed == true)
            {
                string cancellingTripToDestination = string.Format(
               cortanaResourceMap.GetValue("CancellingTripToDestination", cortanaContext).ValueAsString,
               destination);
                await ShowProgressScreen(cancellingTripToDestination);

                // Perform the operation to remove the trip from app data. 
                // As the background task runs within the app package of the installed app,
                // we can access local files belonging to the app without issue.
                await store.DeleteTrip(trip);

                // Provide a completion message to the user.
                var userMessage = new VoiceCommandUserMessage();
                string cancelledTripToDestination = string.Format(
                    cortanaResourceMap.GetValue("CancelledTripToDestination", cortanaContext).ValueAsString,
                    destination);
                userMessage.DisplayMessage = userMessage.SpokenMessage = cancelledTripToDestination;
                response = VoiceCommandResponse.CreateResponse(userMessage);
                await voiceServiceConnection.ReportSuccessAsync(response);
            }
            else
            {
                // Confirm no action for the user.
                var userMessage = new VoiceCommandUserMessage();
                string keepingTripToDestination = string.Format(
                    cortanaResourceMap.GetValue("KeepingTripToDestination", cortanaContext).ValueAsString,
                    destination);
                userMessage.DisplayMessage = userMessage.SpokenMessage = keepingTripToDestination;

                response = VoiceCommandResponse.CreateResponse(userMessage);
                await voiceServiceConnection.ReportSuccessAsync(response);
            }
        }
    }
}
```

## <a name="disambiguation-screen"></a>Écran de désambiguïsation

Lorsqu’une action spécifiée par une commande vocale a plusieurs résultats possibles, un app service peut demander plus d’informations à l’utilisateur.

Voici un exemple d’écran de désambiguïsation pour l’application **Adventure Works** . Dans cet exemple, un utilisateur a demandé à App service d’annuler un voyage à Las Vegas via **Cortana**. Toutefois, l’utilisateur a deux voyages à Las Vegas sur différentes dates et l’app service ne peut pas terminer l’action sans que l’utilisateur ne sélectionne le voyage prévu.

App service fournit à **Cortana** un écran de désambiguïsation qui invite l’utilisateur à effectuer une sélection dans une liste de voyages correspondants, avant d’en annuler un.

Dans ce cas, **Cortana** invite l’utilisateur à une question similaire fournie par app service.

À la deuxième invite, si l’utilisateur n’est toujours pas en mesure de l’utiliser pour identifier la sélection, **Cortana** l’invite à l’utilisateur une troisième fois avec la même question précédée d’un excuses. Si l’utilisateur n’indique pas un élément pouvant être utilisé pour identifier la sélection, **Cortana** cesse d’écouter l’entrée vocale et invite l’utilisateur à appuyer plutôt sur l’un des boutons.

L’écran de désambiguïsation comprend un message personnalisé pour l’action, une icône et une vignette de contenu avec des informations sur le trajet en cours d’annulation.

:::image type="content" source="images/cortana/cortana-disambiguation-screen.png" alt-text="Capture d’écran de Cortana avec l’écran de désambiguïsation de l’application en arrière-plan":::

AdventureWorksVoiceCommandService.cs contient la méthode d’annulation de voyage suivante, qui appelle [**RequestDisambiguationAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) pour afficher l’écran de suppression des ambiguïtés dans **Cortana**.

```csharp
/// <summary>
/// Provide the user with a way to identify which trip to cancel. 
/// </summary>
/// <param name="trips">The set of trips</param>
/// <param name="disambiguationMessage">The initial disambiguation message</param>
/// <param name="secondDisambiguationMessage">Repeat prompt retry message</param>
private async Task<Model.Trip> DisambiguateTrips(IEnumerable<Model.Trip> trips, string disambiguationMessage, string secondDisambiguationMessage)
{
    // Create the first prompt message.
    var userPrompt = new VoiceCommandUserMessage();
    userPrompt.DisplayMessage =
        userPrompt.SpokenMessage = disambiguationMessage;

    // Create a re-prompt message if the user responds with an out-of-grammar response.
    var userReprompt = new VoiceCommandUserMessage();
    userReprompt.DisplayMessage =
        userReprompt.SpokenMessage = secondDisambiguationMessage;

    // Create card for each item. 
    var destinationContentTiles = new List<VoiceCommandContentTile>();
    int i = 1;
    foreach (Model.Trip trip in trips)
    {
        var destinationTile = new VoiceCommandContentTile();

        destinationTile.ContentTileType = VoiceCommandContentTileType.TitleWith68x68IconAndText;
        destinationTile.Image = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png"));
        
        // The AppContext can be any arbitrary object.
        destinationTile.AppContext = trip;
        string dateFormat = "";
        if (trip.StartDate != null)
        {
            dateFormat = trip.StartDate.Value.ToString(dateFormatInfo.LongDatePattern);
        }
        else
        {
            // The app allows a trip to have no date.
            // However, the choices must be unique so they can be distinguished.
            // Here, we add a number to identify them.
            dateFormat = string.Format("{0}", i);
        } 

        destinationTile.Title = trip.Destination + " " + dateFormat;
        destinationTile.TextLine1 = trip.Description;

        destinationContentTiles.Add(destinationTile);
        i++;
    }

    // Cortana handles re-prompting if no valid response.
    var response = VoiceCommandResponse.CreateResponseForPrompt(userPrompt, userReprompt, destinationContentTiles);

    // If cortana is dismissed in this operation, null is returned.
    var voiceCommandDisambiguationResult = await
        voiceServiceConnection.RequestDisambiguationAsync(response);
    if (voiceCommandDisambiguationResult != null)
    {
        return (Model.Trip)voiceCommandDisambiguationResult.SelectedItem.AppContext;
    }

    return null;
}
```

## <a name="error-screen"></a>Écran d’erreur

Lorsqu’une action spécifiée par une commande vocale ne peut pas être effectuée, app service peut fournir un écran d’erreur.

Voici un exemple d’écran d’erreur pour l’application **Adventure Works** . Dans cet exemple, un utilisateur a demandé à App service d’annuler un voyage à Las Vegas via **Cortana**. Toutefois, l’utilisateur n’a aucun voyage prévu à Las Vegas.

App service fournit à **Cortana** un écran d’erreur qui inclut un message personnalisé pour l’action, une icône et le message d’erreur spécifique.

Appelez [**ReportFailureAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) pour afficher l’écran d’erreur dans **Cortana**.

```csharp
var userMessage = new VoiceCommandUserMessage();
    userMessage.DisplayMessage = userMessage.SpokenMessage = 
      "Sorry, you don't have any trips to Las Vegas";
                
    var response = VoiceCommandResponse.CreateResponse(userMessage);

    response.AppLaunchArgument = "showUpcomingTrips";
    await voiceServiceConnection.ReportFailureAsync(response);
```

## <a name="related-articles"></a>Articles connexes

- [Interactions Cortana dans les applications Windows](cortana-interactions.md)
- [Éléments et attributs d’un fichier VCD v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Instructions de conception de Cortana](cortana-design-guidelines.md)
- [Exemple de commande vocale Cortana](https://go.microsoft.com/fwlink/p/?LinkID=619899)
