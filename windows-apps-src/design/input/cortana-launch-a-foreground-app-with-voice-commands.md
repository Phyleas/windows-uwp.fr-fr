---
title: Activer une application de premier plan avec des commandes vocales par le biais de Cortana-Cortana UWP conception et développement
description: Utilisez les commandes vocales pour activer votre application au premier plan et exécuter une action ou une commande au sein de l’application.
ms.assetid: e4bf3714-6f62-466f-9e7c-3b03ee86a117
ms.date: 01/28/2021
ms.topic: article
keywords: cortana
ms.openlocfilehash: a9df2d107482234fa8783d92a1f4fbb075e7d505
ms.sourcegitcommit: d7efd35c1749f695aebbc0db99d8b62b70fb72da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99057754"
---
# <a name="activate-a-foreground-app-with-voice-commands-through-cortana"></a>Activer une application au premier plan avec les commandes vocales via Cortana

>[!WARNING]
> Cette fonctionnalité n’est plus prise en charge à partir de la mise à jour Windows 10 2020 (version 2004, nom de nom « 20H1 »).

En plus d’utiliser des commandes vocales dans **Cortana** pour accéder aux fonctionnalités du système, vous pouvez également étendre **Cortana** avec les fonctionnalités et fonctionnalités de votre application. À l’aide de commandes vocales, votre application peut être activée au premier plan et une action ou une commande exécutée dans l’application.

> [!NOTE]
> **API importantes**
>
> - [**Windows. ApplicationModel. VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**Éléments et attributs d’un fichier VCD v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

Quand une application gère une commande vocale au premier plan, elle prend le focus et Cortana est rejetée. Si vous préférez, vous pouvez activer votre application et exécuter une commande en tant que tâche en arrière-plan. Dans ce cas, Cortana conserve le focus et votre application retourne tous les commentaires et résultats via le canevas **Cortana** et la voix **Cortana** .

Les commandes vocales qui requièrent un contexte supplémentaire ou une entrée utilisateur (par exemple, l’envoi d’un message à un contact spécifique) sont mieux gérées dans une application de premier plan, tandis que les commandes de base (comme la liste des voyages à venir) peuvent être gérées dans **Cortana** par le biais d’une application en arrière-plan.

Si vous souhaitez activer une application en arrière-plan à l’aide de commandes vocales, consultez [activer une application en arrière-plan dans Cortana à l’aide des commandes vocales](cortana-launch-a-background-app-with-voice-commands.md).

> [!NOTE]
> Une commande vocale est un énoncé unique avec un but spécifique, défini dans un fichier de définition de commande vocale (VCD), dirigé vers une application installée via **Cortana**.
>
> Un fichier VCD définit une ou plusieurs commandes vocales, chacune avec un objectif unique.
>
> Les définitions de commandes vocales peuvent varier en complexité. Ils peuvent prendre en charge n’importe quel caractère à partir d’une seule et même énoncée à une collection d’énoncés de langage naturel plus flexibles, qui dénotent le même objectif.

Pour illustrer les fonctionnalités d’application de premier plan, nous allons utiliser une application de planification et de gestion de voyage nommée **Adventure Works** à partir de l' [exemple de commande vocale Cortana](https://go.microsoft.com/fwlink/p/?LinkID=619899).

Pour créer un nouveau voyage **Adventure Works** sans **Cortana**, un utilisateur lance l’application et accède à la page **nouveau voyage** . Pour afficher un voyage existant, un utilisateur lance l’application, accède à la page des **voyages à venir** et sélectionne le trajet.

À l’aide de commandes vocales via **Cortana**, l’utilisateur peut simplement indiquer « Adventure Works ajouter un voyage » ou « ajouter un voyage à Adventure Works » pour lancer l’application et accéder à la nouvelle page de **voyage** . À son tour, le message « Adventure Works, afficher mon voyage à Londres » lance l’application et accède à la page de détails du **trajet** , comme indiqué ici.

:::image type="content" source="images/cortana/cortana-foreground-with-adventureworks.png" alt-text="Capture d’écran de Cortana lancement de l’application de premier plan":::

Voici les étapes de base pour ajouter une fonctionnalité de commande vocale et intégrer Cortana à votre application à l’aide de la voix ou de l’entrée au clavier :

1. Créez un fichier VCD. Il s’agit d’un document XML qui définit toutes les commandes vocales que l’utilisateur peut indiquer pour initier des actions ou appeler des commandes lors de l’activation de votre application. Consultez [**éléments et attributs VCD v 1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2).
2. Inscrivez les jeux de commandes dans le fichier VCD lorsque l’application est lancée.
3. Gérez la commande d’activation par voix, la navigation dans l’application et l’exécution de la commande.

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

## <a name="create-a-new-solution-with-project-in-visual-studio"></a>Créer une solution avec un projet dans Visual Studio

1. Lancez Microsoft Visual Studio 2015.

    La page de démarrage de Visual Studio 2015 s’affiche.

2. Dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.

    La boîte **de dialogue Nouveau projet** s’affiche. Le volet gauche de la boîte de dialogue vous permet de sélectionner le type de modèle à afficher.

3. Dans le volet gauche, développez **> modèles installés > Visual C \# > Windows**, puis choisissez le groupe **universel** de modèles. Le volet central de la boîte de dialogue affiche une liste de modèles de projet pour les applications plateforme Windows universelle (UWP).
4. Dans le volet central, sélectionnez le modèle **application vide (Windows universel)** .

    Le modèle d' **application vide** crée une application UWP minimale qui se compile et s’exécute, mais ne contient pas de données ni de contrôles d’interface utilisateur. Vous ajoutez des contrôles à l’application au cours de ce didacticiel.

5. Dans la zone de texte **nom** , tapez le nom de votre projet. Pour cet exemple, nous utilisons « AdventureWorks ».
6. Cliquez sur **OK** pour créer le projet.

    Microsoft Visual Studio crée votre projet et l’affiche dans la **Explorateur de solutions**.

## <a name="add-image-assets-to-project-and-specify-them-in-the-app-manifest"></a>Ajouter des ressources d’image au projet et les spécifier dans le manifeste de l’application

Les applications UWP peuvent sélectionner automatiquement les images les plus appropriées en fonction de paramètres spécifiques et de fonctionnalités d’appareil (contraste élevé, pixels effectifs, paramètres régionaux, etc.). Il vous suffit de fournir les images et de vous assurer que vous utilisez la Convention d’affectation de noms et l’organisation de dossiers appropriées dans le projet d’application pour les différentes versions de ressource. Si vous ne fournissez pas les versions de ressources recommandées, l’accessibilité, la localisation et la qualité de l’image peuvent être affectées, en fonction des préférences, des capacités, du type d’appareil et de l’emplacement de l’utilisateur.

Pour plus d’informations sur les ressources d’image pour les facteurs de contraste et d’échelle élevés, consultez Instructions pour les éléments multimédias en [mosaïque et en icône](/windows/uwp/app-resources/images-tailored-for-scale-theme-contrast).

Vous nommez des ressources à l’aide de qualificateurs. Les qualificateurs de ressources sont des modificateurs de dossiers et de noms de fichiers qui identifient le contexte dans lequel une version particulière d’une ressource doit être utilisée.

La Convention d’affectation de noms standard est `foldername/qualifiername-value[_qualifiername-value]/filename.qualifiername-value[_qualifiername-value].ext` . Par exemple, `images/logo.scale-100_contrast-white.png` , qui peut être référencé dans le code en utilisant uniquement le dossier racine et le nom de fichier : `images/logo.png` . Consultez [Comment nommer des ressources à l’aide de qualificateurs](/previous-versions/windows/apps/hh965324(v=win.10)).

Nous vous recommandons de marquer la langue par défaut des fichiers de ressources de type chaîne (par exemple, `en-US\resources.resw` ) et le facteur d’échelle par défaut sur les images (comme `logo.scale-100.png` ), même si vous ne prévoyez pas de fournir actuellement des ressources localisées ou à plusieurs résolutions. Toutefois, nous vous recommandons, au minimum, de fournir des ressources pour les facteurs de mise à l’échelle 100, 200 et 400.

> [!IMPORTANT]
> L’icône d’application utilisée dans la zone de titre de la zone de dessin **Cortana** est l’icône Square44x44Logo spécifiée dans le fichier « Package. appxmanifest ».

## <a name="create-a-vcd-file"></a>Créer un fichier VCD

1. Dans Visual Studio, cliquez avec le bouton droit sur le nom de votre projet principal, puis sélectionnez **ajouter > nouvel élément**. Ajoutez un **fichier XML**.
2. Tapez un nom pour le fichier [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) (pour cet exemple, « AdventureWorksCommands.xml »), puis cliquez sur Ajouter.
3. Dans **Explorateur de solutions**, sélectionnez le fichier [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) .
4. Dans la fenêtre **Propriétés** , affectez à **action de génération** la valeur **contenu**, puis définissez **copier dans le répertoire de sortie** sur **copier si plus récent**.

## <a name="edit-the-vcd-file"></a>Modifier le fichier VCD

Ajoutez un élément **VoiceCommands** avec un attribut **xmlns** pointant vers `https://schemas.microsoft.com/voicecommands/1.2` .

1. Pour chaque langue prise en charge par votre application, créez un élément [**commandSet**](/previous-versions/windows/dn722331(v=win.10)) qui contient les commandes vocales prises en charge par votre application.

   Vous pouvez déclarer plusieurs éléments [**commandSet**](/previous-versions/windows/dn722331(v=win.10)) , chacun avec un attribut [**XML : lang**](/previous-versions/windows/dn722331(v=win.10)) différent, afin que votre application soit utilisée sur différents marchés. Par exemple, une application pour le États-Unis peut avoir un [**commandSet**](/previous-versions/windows/dn722331(v=win.10)) pour l’anglais et un [**commandSet**](/previous-versions/windows/dn722331(v=win.10)) pour l’espagnol.

   > [!CAUTION]
   > Pour activer une application et initier une action à l’aide d’une commande vocale, l’application doit inscrire un fichier VCD contenant un [**commandSet**](/previous-versions/windows/dn722331(v=win.10)) avec une langue qui correspond à la langue sélectionnée par l’utilisateur pour son appareil. Le langage de reconnaissance vocale se trouve dans **paramètres > > langage de reconnaissance vocale >** vocale.

2. Ajoutez un élément de **commande** pour chaque commande que vous souhaitez prendre en charge. Chaque **commande** déclarée dans un fichier [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) doit inclure les informations suivantes :

   - Attribut **appname** utilisé par votre application pour identifier la commande vocale au moment de l’exécution.
   - Un **exemple** d’élément qui contient une expression décrivant comment un utilisateur peut appeler la commande. **Cortana** affiche cet exemple lorsque l’utilisateur dit « que puis-je prononcer ? », « aide » ou qu’il clique sur **plus**.
   - Élément **ListenFor** qui contient les mots ou expressions que votre application reconnaît comme une commande. Chaque élément **ListenFor** peut contenir des références à un ou plusieurs éléments **PhraseList** qui contiennent des mots spécifiques relatifs à la commande.
  
> [!NOTE]
> Les éléments **ListenFor** ne peuvent pas être modifiés par programmation. Toutefois, les éléments **PhraseList** associés aux éléments **ListenFor** peuvent être modifiés par programmation. Les applications doivent modifier le contenu du **PhraseList** au moment de l’exécution en fonction du jeu de données généré lorsque l’utilisateur utilise l’application. Consultez [modifier dynamiquement les listes d’expressions VCD du CD-vidéo Cortana](cortana-dynamically-modify-voice-command-definition-vcd-phrase-lists.md).

Élément de **Commentaires** qui contient le texte pour **Cortana** à afficher et à parler lorsque l’application est lancée.

Un élément **Navigate** indique que la commande vocale active l’application au premier plan. Dans cet exemple, la `showTripToDestination` commande est une tâche de premier plan.

Un élément **VoiceCommandService** indique que la commande vocale active l’application en arrière-plan. La valeur de l’attribut **target** de cet élément doit correspondre à la valeur de l’attribut **Name** de l’élément [**UAP : AppService**](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-appservice) dans le fichier Package. appxmanifest. Dans cet exemple, les `whenIsTripToDestination` `cancelTripToDestination` commandes et sont des tâches en arrière-plan qui spécifient le nom de App service sous la forme « AdventureWorksVoiceCommandService ».

Pour plus d’informations, consultez les informations de référence sur les [**éléments et les attributs VCD v 1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) .

Voici une partie du fichier [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) qui définit les commandes vocales en-US pour l’application **Adventure Works** .

```xml
<?xml version="1.0" encoding="utf-8" ?>
<VoiceCommands xmlns="https://schemas.microsoft.com/voicecommands/1.2">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <AppName> Adventure Works </AppName>
    <Example> Show trip to London </Example>

    <Command Name="showTripToDestination">
      <Example> Show trip to London </Example>
      <ListenFor RequireAppName="BeforeOrAfterPhrase"> show [my] trip to {destination} </ListenFor>
      <ListenFor RequireAppName="ExplicitlySpecified"> show [my] {builtin:AppName} trip to {destination} </ListenFor>
      <Feedback> Showing trip to {destination} </Feedback>
      <Navigate />
    </Command>

    <Command Name="whenIsTripToDestination">
      <Example> When is my trip to Las Vegas?</Example>
      <ListenFor RequireAppName="BeforeOrAfterPhrase"> when is [my] trip to {destination}</ListenFor>
      <ListenFor RequireAppName="ExplicitlySpecified"> when is [my] {builtin:AppName} trip to {destination} </ListenFor>
      <Feedback> Looking for trip to {destination}</Feedback>
      <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
    </Command>
    
    <Command Name="cancelTripToDestination">
      <Example> Cancel my trip to Las Vegas </Example>
      <ListenFor RequireAppName="BeforeOrAfterPhrase"> cancel [my] trip to {destination}</ListenFor>
      <ListenFor RequireAppName="ExplicitlySpecified"> cancel [my] {builtin:AppName} trip to {destination} </ListenFor>
      <Feedback> Cancelling trip to {destination}</Feedback>
      <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
    </Command>

    <PhraseList Label="destination">
      <Item>London</Item>
      <Item>Las Vegas</Item>
      <Item>Melbourne</Item>
      <Item>Yosemite National Park</Item>
    </PhraseList>
  </CommandSet>
```

## <a name="install-the-vcd-commands"></a>Installer les commandes VCD

Votre application doit s’exécuter une fois pour installer le VCD.

> [!NOTE]
> Les données de commande vocale ne sont pas conservées entre les installations d’applications. Pour vous assurer que les données de commande vocale de votre application restent intactes, envisagez d’initialiser votre fichier VCD chaque fois que votre application est lancée ou activée, ou conservez un paramètre qui indique si le VCD est actuellement installé.

Dans le fichier « app.xaml.cs » :

1. Ajoutez la directive using suivante :

    ```csharp
    using Windows.Storage;
    ```

1. Marquez la méthode « OnLaunched » avec le modificateur Async.  

    ```csharp
    protected async override void OnLaunched(LaunchActivatedEventArgs e)
    ```

1. Appelez [**InstallCommandDefinitionsFromStorageFileAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager) dans le gestionnaire [**OnLaunched**](/uwp/api/Windows.UI.Xaml.Application) pour enregistrer les commandes vocales que le système doit reconnaître.

  Dans l’exemple Adventure Works, nous commençons par définir un objet [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) .

  Nous appelons ensuite [**GetFileAsync**](/uwp/api/Windows.Storage.StorageFolder) pour l’initialiser avec notre fichier « AdventureWorksCommands.xml ».

  Cet objet [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) est ensuite passé à [**InstallCommandDefinitionsFromStorageFileAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager).

```csharp
try
{
  // Install the main VCD. 
  StorageFile vcdStorageFile = 
  await Package.Current.InstalledLocation.GetFileAsync(
  @"AdventureWorksCommands.xml");

  await Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager.
InstallCommandDefinitionsFromStorageFileAsync(vcdStorageFile);

  // Update phrase list.
  ViewModel.ViewModelLocator locator = App.Current.Resources["ViewModelLocator"] as ViewModel.ViewModelLocator;
  if(locator != null)
  {
     await locator.TripViewModel.UpdateDestinationPhraseList();
  }
}
catch (Exception ex)
{
  System.Diagnostics.Debug.WriteLine("Installing Voice Commands Failed: " + ex.ToString());
}
```

## <a name="handle-activation-and-execute-voice-commands"></a>Gérer l’activation et exécuter les commandes vocales

Spécifiez le mode de réponse de votre application aux activations de commandes vocales suivantes (une fois qu’elle a été lancée au moins une fois et que les jeux de commandes vocales ont été installés).

1. Vérifiez que votre application a été activée par une commande vocale.

    Remplacez l’événement [**application. OnActivated**](/uwp/api/Windows.UI.Xaml.Application) et vérifiez si [**IActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs). Le [**genre**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs) est [**VoiceCommand**](/uwp/api/Windows.ApplicationModel.Activation.ActivationKind).

2. Déterminez le nom de la commande et ce qui a été parlé.

    Obtenez une référence à un objet [**VoiceCommandActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.VoiceCommandActivatedEventArgs) à partir du [**IActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs) et interrogez la propriété [**results**](/uwp/api/Windows.ApplicationModel.Activation.VoiceCommandActivatedEventArgs) pour un objet [**SpeechRecognitionResult**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) .

    Pour déterminer ce que l’utilisateur a dit, vérifiez la valeur du [**texte**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) ou les propriétés sémantiques de l’expression reconnue dans le dictionnaire [**SpeechRecognitionSemanticInterpretation**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation) .

3. Prenez les mesures appropriées dans votre application, comme naviguer jusqu’à la page souhaitée.

Pour cet exemple, nous revenons au VCD à l’étape 3 : modifier le fichier VCD.

Une fois que nous obtenons le résultat de reconnaissance vocale pour la commande vocale, nous obtenons le nom de la commande à partir de la première valeur dans le tableau [**rulepath**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) . Étant donné que le fichier VCD a défini plusieurs commandes vocales possibles, nous devons comparer la valeur par rapport aux noms de commande du VCD et prendre les mesures appropriées.

L’action la plus courante qu’une application peut effectuer consiste à accéder à une page dont le contenu est pertinent dans le contexte de la commande vocale. Pour cet exemple, nous allons accéder à une page de **déclenchement** et transmettre la valeur de la commande vocale, la façon dont la commande a été entrée et la phrase « destination » reconnue (le cas échéant). L’application peut également envoyer un paramètre de navigation au [**SpeechRecognitionResult**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) lors de la navigation vers la page.

Vous pouvez déterminer si la commande vocale qui a lancé votre application a fait son discours ou si elle a été tapée en tant que texte, à partir du dictionnaire [**SpeechRecognitionSemanticInterpretation. Properties**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation) à l’aide de la clé **commandMode** . La valeur de cette clé est soit « Voice », soit « Text ». Si la valeur de la clé est « voix », envisagez d’utiliser la synthèse vocale ([**Windows. Media. SpeechSynthesis**](/uwp/api/Windows.Media.SpeechSynthesis)) dans votre application pour fournir à l’utilisateur des commentaires prononcés.

Utilisez [**SpeechRecognitionSemanticInterpretation. Properties**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation) pour rechercher le contenu parlé dans les contraintes **PhraseList** ou **PhraseTopic** d’un élément **ListenFor** . La clé de dictionnaire est la valeur de l’attribut **label** de l’élément **PhraseList** ou **PhraseTopic** . Ici, nous montrons comment accéder à la valeur de l’expression **{destination}** .

```csharp
/// <summary>
/// Entry point for an application activated by some means other than normal launching. 
/// This includes voice commands, URI, share target from another app, and so on. 
/// 
/// NOTE:
/// A previous version of the VCD file might remain in place 
/// if you modify it and update the app through the store. 
/// Activations might include commands from older versions of your VCD. 
/// Try to handle these commands gracefully.
/// </summary>
/// <param name="args">Details about the activation method.</param>
protected override void OnActivated(IActivatedEventArgs args)
{
    base.OnActivated(args);

    Type navigationToPageType;
    ViewModel.TripVoiceCommand? navigationCommand = null;

    // Voice command activation.
    if (args.Kind == ActivationKind.VoiceCommand)
    {
        // Event args can represent many different activation types. 
        // Cast it so we can get the parameters we care about out.
        var commandArgs = args as VoiceCommandActivatedEventArgs;

        Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = commandArgs.Result;

        // Get the name of the voice command and the text spoken. 
        // See VoiceCommands.xml for supported voice commands.
        string voiceCommandName = speechRecognitionResult.RulePath[0];
        string textSpoken = speechRecognitionResult.Text;

        // commandMode indicates whether the command was entered using speech or text.
        // Apps should respect text mode by providing silent (text) feedback.
        string commandMode = this.SemanticInterpretation("commandMode", speechRecognitionResult);
        
        switch (voiceCommandName)
        {
            case "showTripToDestination":
                // Access the value of {destination} in the voice command.
                string destination = this.SemanticInterpretation("destination", speechRecognitionResult);

                // Create a navigation command object to pass to the page. 
                navigationCommand = new ViewModel.TripVoiceCommand(
                    voiceCommandName,
                    commandMode,
                    textSpoken,
                    destination);

                // Set the page to navigate to for this voice command.
                navigationToPageType = typeof(View.TripDetails);
                break;
            default:
                // If we can't determine what page to launch, go to the default entry point.
                navigationToPageType = typeof(View.TripListView);
                break;
        }
    }
    // Protocol activation occurs when a card is clicked within Cortana (using a background task).
    else if (args.Kind == ActivationKind.Protocol)
    {
        // Extract the launch context. In this case, we're just using the destination from the phrase set (passed
        // along in the background task inside Cortana), which makes no attempt to be unique. A unique id or 
        // identifier is ideal for more complex scenarios. We let the destination page check if the 
        // destination trip still exists, and navigate back to the trip list if it doesn't.
        var commandArgs = args as ProtocolActivatedEventArgs;
        Windows.Foundation.WwwFormUrlDecoder decoder = new Windows.Foundation.WwwFormUrlDecoder(commandArgs.Uri.Query);
        var destination = decoder.GetFirstValueByName("LaunchContext");

        navigationCommand = new ViewModel.TripVoiceCommand(
                                "protocolLaunch",
                                "text",
                                "destination",
                                destination);

        navigationToPageType = typeof(View.TripDetails);
    }
    else
    {
        // If we were launched via any other mechanism, fall back to the main page view.
        // Otherwise, we'll hang at a splash screen.
        navigationToPageType = typeof(View.TripListView);
    }

    // Repeat the same basic initialization as OnLaunched() above, taking into account whether
    // or not the app is already active.
    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active.
    if (rootFrame == null)
    {
        // Create a frame to act as the navigation context and navigate to the first page.
        rootFrame = new Frame();
        App.NavigationService = new NavigationService(rootFrame);

        rootFrame.NavigationFailed += OnNavigationFailed;

        // Place the frame in the current window.
        Window.Current.Content = rootFrame;
    }

    // Since we're expecting to always show a details page, navigate even if 
    // a content frame is in place (unlike OnLaunched).
    // Navigate to either the main trip list page, or if a valid voice command
    // was provided, to the details page for that trip.
    rootFrame.Navigate(navigationToPageType, navigationCommand);

    // Ensure the current window is active
    Window.Current.Activate();
}

/// <summary>
/// Returns the semantic interpretation of a speech result. 
/// Returns null if there is no interpretation for that key.
/// </summary>
/// <param name="interpretationKey">The interpretation key.</param>
/// <param name="speechRecognitionResult">The speech recognition result to get the semantic interpretation from.</param>
/// <returns></returns>
private string SemanticInterpretation(string interpretationKey, SpeechRecognitionResult speechRecognitionResult)
{
  return speechRecognitionResult.SemanticInterpretation.Properties[interpretationKey].FirstOrDefault();
}
```

## <a name="related-articles"></a>Articles connexes

- [Interactions Cortana dans les applications Windows](cortana-interactions.md)
- [Activer une application en arrière-plan dans Cortana à l’aide de commandes vocales](cortana-launch-a-background-app-with-voice-commands.md)
- [Éléments et attributs d’un fichier VCD v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Instructions de conception de Cortana](cortana-design-guidelines.md)
- [Exemple de commande vocale Cortana](https://go.microsoft.com/fwlink/p/?LinkID=619899)
