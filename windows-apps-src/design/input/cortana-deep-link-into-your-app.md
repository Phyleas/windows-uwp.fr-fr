---
title: Lien profond à partir d’une application en arrière-plan dans Cortana vers une application de premier plan-conception et développement UWP de Cortana
description: Fournissez des liens détaillés à partir d’une application en arrière-plan dans **Cortana** qui lance l’application au premier plan dans un État ou un contexte spécifique.
ms.assetid: 6fe5fcc5-9ee4-4c04-92f4-7b1bf7ef5651
ms.date: 01/28/2021
ms.topic: article
keywords: cortana
ms.openlocfilehash: d96e54604c5def61802a77625a6c18c556db909d
ms.sourcegitcommit: 8fe992f3a6d8f7975af4911ad88e855bee50083e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99606054"
---
# <a name="deep-link-from-a-background-app-in-cortana-to-a-foreground-app"></a>Lien profond à partir d’une application en arrière-plan dans Cortana vers une application de premier plan

>[!WARNING]
> Cette fonctionnalité n’est plus prise en charge à partir de la mise à jour Windows 10 2020 (version 2004, nom de nom « 20H1 »).
>
> Consultez [Cortana dans Microsoft 365](/microsoft-365/admin/misc/cortana-integration) pour savoir comment Cortana transforme les expériences de productivité modernes.

Fournissez des liens détaillés à partir d’une application en arrière-plan dans **Cortana** qui lance l’application au premier plan dans un État ou un contexte spécifique.

> [!NOTE]
> **Cortana** et le service d’application en arrière-plan sont terminés lorsque l’application de premier plan est lancée.

Un lien profond est affiché par défaut sur l’écran de saisie semi-automatique de **Cortana** , comme indiqué ici (« Go to AdventureWorks »), mais vous pouvez afficher des liens détaillés sur d’autres écrans.

:::image type="content" source="images/cortana/cortana-completion-screen-upcomingtrip-small.png" alt-text="Capture d’écran de l’exécution d’une application en arrière-plan Cortana pour un prochain voyage":::

> [!NOTE]
> **API importantes**
>
> - [**Windows. ApplicationModel. VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**Éléments et attributs d’un fichier VCD v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

## <a name="overview"></a>Vue d’ensemble

Les utilisateurs peuvent accéder à votre application via **Cortana** en :

- Activation d’une application en premier plan (voir [activation d’une application de premier plan avec des commandes vocales via Cortana](cortana-launch-a-foreground-app-with-voice-commands.md)).
- Exposer des fonctionnalités spécifiques en tant que service d’application en arrière-plan (consultez [activer une application en arrière-plan dans Cortana à l’aide de commandes vocales](cortana-launch-a-background-app-with-voice-commands.md)).
- Liaison profonde à des pages, des contenus et un État ou un contexte spécifiques.

Nous aborderons la liaison profonde ici.

La liaison profonde est utile lorsque Cortana et votre App service jouent le rôle de passerelle vers votre application complète (au lieu de demander à l’utilisateur de lancer l’application via le menu Démarrer) ou de fournir un accès à des détails et des fonctionnalités plus riches au sein de votre application, ce qui n’est pas possible via Cortana. La liaison profonde est une autre façon d’augmenter la convivialité et de promouvoir votre application.

Il existe trois façons de fournir des liens détaillés :

- Lien « accéder à l' &lt; application &gt; » sur différents écrans **Cortana** .
- Lien incorporé dans une vignette de contenu sur différents écrans **Cortana** .
- Lancement de l’application de premier plan par programmation à partir du service d’application en arrière-plan.

## <a name="go-to-ltappgt-deep-link"></a>Lien ciblé « accéder à l' &lt; application &gt; »

**Cortana** affiche &lt; le lien détaillé « aller à &gt; l’application » sous la carte de contenu sur la plupart des écrans.

:::image type="content" source="images/cortana/cortana-completion-screen.png" alt-text="Capture d’écran du lien détaillé « aller à l’application » de Cortana sur l’écran de fin d’une application en arrière-plan.":::

Vous pouvez fournir un argument de lancement pour ce lien qui ouvre votre application dans un contexte similaire à celui de l’app service. Si vous ne fournissez pas d’argument de lancement, l’application est lancée sur l’écran principal.

Dans cet exemple de AdventureWorksVoiceCommandService.cs de l’exemple **AdventureWorks** , nous transmettons la chaîne de destination spécifiée ( `destination` ) à la méthode SendCompletionMessageForDestination, qui récupère tous les voyages correspondants et fournit un lien profond vers l’application.

Tout d’abord, nous créons un [**VoiceCommandUserMessage**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandUserMessage) ( ```userMessage``` ) parlé par **Cortana** et affiché sur le canevas **Cortana** . Un objet de liste [**VoiceCommandContentTile**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTile) est ensuite créé pour afficher la collection de cartes de résultats sur le canevas.

Ces deux objets sont ensuite transmis à la méthode [CreateResponse](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) de l’objet [**VoiceCommandResponse**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) ( `response` ). Nous définissons ensuite la valeur de la propriété [**AppLaunchArgument**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) de l’objet Response sur la valeur de `destination` passed à cette fonction. Lorsqu’un utilisateur appuie sur une vignette de contenu sur la zone de dessin Cortana, les valeurs de paramètre sont transmises à l’application via l’objet de réponse.

Enfin, nous appelons la méthode [**ReportSuccessAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) de [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection).

```csharp
/// <summary>
/// Show details for a single trip, if the trip can be found. 
/// This demonstrates a simple response flow in Cortana.
/// </summary>
/// <param name="destination">The destination specified in the voice command.</param>
private async Task SendCompletionMessageForDestination(string destination)
{
...
    IEnumerable<Model.Trip> trips = store.Trips.Where(p => p.Destination == destination);

    var userMessage = new VoiceCommandUserMessage();
    var destinationsContentTiles = new List<VoiceCommandContentTile>();
...
    var response = VoiceCommandResponse.CreateResponse(userMessage, destinationsContentTiles);

    if (trips.Count() > 0)
    {
        response.AppLaunchArgument = destination;
    }

    await voiceServiceConnection.ReportSuccessAsync(response);
}
```

## <a name="content-tile-deep-link"></a>Lien profond de la vignette de contenu

Vous pouvez ajouter des liens détaillés à des cartes de contenu sur différents écrans **Cortana** .

:::image type="content" source="images/cortana/cortana-backgroundapp-progress-result.png" alt-text="Capture d’écran de la zone de travail Cortana pour le workflow de l’application en arrière-plan Cortana de bout en bout à l’aide d’AdventureWorks voyage à venir avec le transfert":::*AdventureWorks « prochain voyage » avec écran de remise*

À l’instar des liens « atteindre l' &lt; application &gt; », vous pouvez fournir un argument de lancement pour ouvrir votre application avec le même contexte que l’app service. Si vous ne fournissez pas d’argument de lancement, la vignette de contenu n’est pas liée à votre application.

Dans cet exemple de AdventureWorksVoiceCommandService.cs de l’exemple **AdventureWorks** , nous passons la destination spécifiée à la méthode SendCompletionMessageForDestination, qui récupère tous les voyages correspondants et fournit des cartes de contenu avec des liens détaillés vers l’application.

Tout d’abord, nous créons un  [**VoiceCommandUserMessage**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandUserMessage) ( ```userMessage``` ) parlé par **Cortana** et affiché sur le canevas **Cortana** . Un objet de liste [**VoiceCommandContentTile**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTile) est ensuite créé pour afficher la collection de cartes de résultats sur le canevas. 

Ces deux objets sont ensuite transmis à la méthode [CreateResponse](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) de l’objet [**VoiceCommandResponse**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) ( ```response``` ). Nous définissons ensuite la valeur de la propriété [**AppLaunchArgument**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) sur la valeur de destination dans la commande Voice.

Enfin, nous appelons la méthode [**ReportSuccessAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) de [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection).
Ici, nous ajoutons deux vignettes de contenu avec des valeurs de paramètre [**AppLaunchArgument**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) différentes à une liste [**VoiceCommandContentTile**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTile) utilisée dans l’appel [**ReportSuccessAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) de l’objet [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) .

```csharp
/// <summary>
/// Show details for a single trip, if the trip can be found. 
/// This demonstrates a simple response flow in Cortana.
/// </summary>
/// <param name="destination">The destination specified in the voice command.</param>
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

## <a name="programmatic-deep-link"></a>Lien profond par programmation

Vous pouvez également lancer par programmation votre application à l’aide d’un argument Launch pour ouvrir votre application avec le même contexte que le service APP service. Si vous ne fournissez pas d’argument de lancement, l’application est lancée sur l’écran principal.

Ici, nous ajoutons un paramètre [**AppLaunchArgument**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) avec une valeur de « Las Vegas » à un objet [**VoiceCommandResponse**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) utilisé dans l’appel [**RequestAppLaunchAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) de l’objet [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) .

```CSharp
var userMessage = new VoiceCommandUserMessage();
userMessage.DisplayMessage = "Here are your trips.";
userMessage.SpokenMessage = 
  "You have one trip to Vegas coming up.";

response = VoiceCommandResponse.CreateResponse(userMessage);
response.AppLaunchArgument = “Las Vegas”;
await  VoiceCommandServiceConnection.RequestAppLaunchAsync(response);
```

## <a name="app-manifest"></a>Manifeste de l’application

Pour activer la liaison profonde à votre application, vous devez déclarer l' `windows.personalAssistantLaunch` extension dans le fichier Package. appxmanifest de votre projet d’application.

Ici, nous déclarons l' `windows.personalAssistantLaunch` extension pour l’application **Adventure Works** .

```XML
<Extensions>
  <uap:Extension Category="windows.appService" 
    EntryPoint="AdventureWorks.VoiceCommands.AdventureWorksVoiceCommandService">
    <uap:AppService Name="AdventureWorksVoiceCommandService"/>
  </uap:Extension>
  <uap:Extension Category="windows.personalAssistantLaunch"/> 
</Extensions>
```

## <a name="protocol-contract"></a>Contrat de protocole

Votre application est lancée au premier plan par le biais de l’activation Uniform Resource Identifier (URI) à l’aide d’un contrat de [**protocole**](/uwp/api/Windows.ApplicationModel.Activation.ActivationKind) . Votre application doit remplacer l’événement [**OnActivated**](/uwp/api/Windows.UI.Xaml.Application) de votre application et Rechercher un **ActivationKind** de **protocole**. Pour plus d’informations, consultez [gérer l’activation d’URI](/windows/uwp/launch-resume/handle-uri-activation).

Ici, nous décoderons l’URI fourni par [**ProtocolActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs) pour accéder à l’argument de lancement. Pour cet exemple, l' [**URI**](/uwp/api/Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs) est défini sur «Windows. personalassistantlaunch :? LaunchContext = Las Vegas».

```CSharp
if (args.Kind == ActivationKind.Protocol)
  {
    var commandArgs = args as ProtocolActivatedEventArgs;
    Windows.Foundation.WwwFormUrlDecoder decoder = 
      new Windows.Foundation.WwwFormUrlDecoder(commandArgs.Uri.Query);
    var destination = decoder.GetFirstValueByName("LaunchContext");

    navigationCommand = new ViewModel.TripVoiceCommand(
      "protocolLaunch",
      "text",
      "destination",
      destination);

    navigationToPageType = typeof(View.TripDetails);

    rootFrame.Navigate(navigationToPageType, navigationCommand);

    // Ensure the current window is active.
    Window.Current.Activate();
  }
```

## <a name="related-articles"></a>Articles connexes

- [Interactions Cortana dans les applications Windows](cortana-interactions.md)
- [Instructions de conception de Cortana](cortana-design-guidelines.md)
- [Éléments et attributs d’un fichier VCD v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Exemple de commande vocale Cortana](https://go.microsoft.com/fwlink/p/?LinkID=619899)
