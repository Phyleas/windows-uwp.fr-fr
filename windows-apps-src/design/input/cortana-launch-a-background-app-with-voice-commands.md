---
title: Activer une application en arrière-plan dans Cortana à l’aide des commandes vocales | Conception et développement UWP Cortana
description: Étendez Cortana avec les fonctionnalités de votre application (en tant que tâche en arrière-plan) à l’aide de commandes vocales.
ms.assetid: e2c7eae3-6beb-4156-92a5-474bba53451e
ms.date: 09/24/2019
ms.topic: article
keywords: cortana
ms.openlocfilehash: f1ed51107f41318cecf2d8fea73484713b4b837c
ms.sourcegitcommit: 8fe992f3a6d8f7975af4911ad88e855bee50083e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99606064"
---
# <a name="activate-a-background-app-in-cortana-using-voice-commands"></a>Activer une application en arrière-plan dans Cortana à l’aide de commandes vocales  

>[!WARNING]
> Cette fonctionnalité n’est plus prise en charge à partir de la mise à jour Windows 10 2020 (version 2004, nom de nom « 20H1 »).
>
> Consultez [Cortana dans Microsoft 365](/microsoft-365/admin/misc/cortana-integration) pour savoir comment Cortana transforme les expériences de productivité modernes.

Outre l’utilisation des commandes vocales dans **Cortana** pour accéder aux fonctionnalités du système, vous pouvez également étendre **Cortana** avec les fonctionnalités et fonctionnalités de votre application (en tant que tâche en arrière-plan) à l’aide de commandes vocales qui spécifient une action ou une commande à exécuter. Quand une application gère une commande vocale en arrière-plan, elle ne prend pas le focus. Au lieu de cela, elle retourne tous les commentaires et résultats via le canevas **Cortana** et la voix **Cortana** .  

> [!NOTE]
> **API importantes**
>
> - [**Windows. ApplicationModel. VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**Éléments et attributs d’un fichier VCD v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

Les applications peuvent être activées au premier plan (l’application prend le focus) ou activées en arrière-plan (**Cortana** conserve le focus), en fonction de la complexité de l’interaction. Par exemple, les commandes vocales qui requièrent un contexte supplémentaire ou une entrée utilisateur (par exemple, l’envoi d’un message à un contact spécifique) sont mieux gérées dans une application de premier plan, tandis que les commandes de base (telles que le listage des prochains voyages) peuvent être gérées dans **Cortana** via une application en arrière-plan.  

Si vous souhaitez activer une application au premier plan à l’aide de commandes vocales, consultez [activer une application de premier plan avec des commandes vocales via Cortana](cortana-launch-a-foreground-app-with-voice-commands.md).  

> [!NOTE]
> Une commande vocale est un énoncé unique avec un but spécifique, défini dans un fichier de définition de commande vocale (VCD), dirigé vers une application installée via **Cortana**.
>
> Un fichier VCD définit une ou plusieurs commandes vocales, chacune avec un objectif unique.
>
> Les définitions de commandes vocales peuvent varier en complexité. Ils peuvent prendre en charge n’importe quel caractère à partir d’une seule et même énoncée à une collection d’énoncés de langage naturel plus flexibles, qui dénotent le même objectif.

Nous utilisons une application de planification et de gestion de voyage nommée **Adventure Works** intégrée dans l’interface utilisateur de **Cortana** , illustrée ici, pour illustrer la plupart des concepts et fonctionnalités que nous abordons. Pour plus d’informations, consultez l' [exemple de commande vocale Cortana](https://go.microsoft.com/fwlink/p/?LinkID=619899).

:::image type="content" source="images/cortana/cortana-overview.png" alt-text="Capture d’écran de Cortana lancement de l’application de premier plan":::

Pour afficher un voyage **Adventure Works** sans **Cortana**, un utilisateur peut lancer l’application et accéder à la page des **voyages à venir** .  

En utilisant des commandes vocales via **Cortana** pour lancer votre application en arrière-plan, l’utilisateur peut simplement indiquer `Adventure Works, when is my trip to Las Vegas?` . Votre application gère la commande et **Cortana** affiche les résultats ainsi que l’icône de votre application et d’autres informations sur l’application, si elles sont fournies.  

:::image type="content" source="images/cortana/cortana-backgroundapp-result.png" alt-text="Capture d’écran de Cortana avec une requête de base et un écran de résultats à l’aide de l’application AdventureWorks en arrière-plan":::

Les étapes de base suivantes ajoutent la fonctionnalité de commande vocale et étendent **Cortana** avec les fonctionnalités d’arrière-plan de votre application à l’aide de la voix ou de l’entrée au clavier.

1. Créez un app service (voir [**Windows. ApplicationModel. AppService**](/uwp/api/Windows.ApplicationModel.AppService)) que **Cortana** appelle en arrière-plan.  
2. Créez un fichier VCD. Le fichier VCD est un document XML qui définit toutes les commandes parlées que l’utilisateur peut indiquer pour initier des actions ou appeler des commandes lors de l’activation de votre application. Consultez [**éléments et attributs VCD v 1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2).  
3. Inscrivez les jeux de commandes dans le fichier VCD lorsque l’application est lancée.  
4. Gérez l’activation en arrière-plan d’App service et l’exécution de la commande vocale.  
5. Affichez et parlez les commentaires appropriés à la commande vocale dans **Cortana**.  

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

## <a name="create-a-new-solution-with-a-primary-project-in-visual-studio"></a>Créer une solution avec un projet principal dans Visual Studio  

1. Lancez Microsoft Visual Studio 2015.  
    La page de démarrage de Visual Studio 2015 s’affiche.  

2. Dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.  
    La boîte **de dialogue Nouveau projet** s’affiche. Le volet gauche de la boîte de dialogue vous permet de sélectionner le type de modèle à afficher.  

3. Dans le volet gauche, développez **> modèles installés > Visual C \# > Windows**, puis choisissez le groupe **universel** de modèles. Le volet central de la boîte de dialogue affiche une liste de modèles de projet pour les applications plateforme Windows universelle (UWP).  
4. Dans le volet central, sélectionnez le modèle **application vide (Windows universel)** .  
    Le modèle d' **application vide** crée une application UWP minimale qui est compilée et exécutée. Le modèle d' **application vide** n’intègre pas de données ni de contrôles d’interface utilisateur. Vous ajoutez des contrôles à l’application à l’aide de cette page comme guide.  

5. Dans la zone de texte **nom** , tapez le nom de votre projet. Exemple : utilisez `AdventureWorks` .  
6. Cliquez sur le bouton **OK** pour créer le projet.  
    Microsoft Visual Studio crée votre projet et l’affiche dans la **Explorateur de solutions**.  

## <a name="add-image-assets-to-primary-project-and-specify-them-in-the-app-manifest"></a>Ajouter des ressources d’image au projet principal et les spécifier dans le manifeste de l’application  

Les applications UWP doivent sélectionner automatiquement les images les plus appropriées. La sélection est basée sur des paramètres et des fonctionnalités d’appareil (contraste élevé, pixels effectifs, paramètres régionaux, etc.). Vous devez fournir les images et vous assurer que vous utilisez la Convention d’affectation de noms et l’organisation de dossiers appropriées dans votre projet d’application pour les différentes versions de ressource.  
Si vous ne fournissez pas les versions de ressources recommandées, l’expérience utilisateur peut être affectée des manières suivantes.

- Accessibilité
- Localisation  
- Qualité de l’image  
Les versions des ressources sont utilisées pour adapter les modifications suivantes dans l’expérience utilisateur.  
- Préférences utilisateur  
- Infirmité  
- Type d'appareil  
- Emplacement  

Pour plus d’informations sur les ressources d’image pour les facteurs de contraste et d’échelle élevés, consultez la page instructions pour les éléments multimédias en mosaïque et en icônes située dans [msdn.Microsoft.com/Windows/UWP/Controls-and-Patterns/Tiles-and-notifications-App-Assets](/windows/uwp/app-resources/images-tailored-for-scale-theme-contrast).  

Vous devez nommer les ressources à l’aide de qualificateurs. Les qualificateurs de ressources sont des modificateurs de dossiers et de noms de fichiers qui identifient le contexte dans lequel une version particulière d’une ressource doit être utilisée.  

La Convention d’affectation de noms standard est `foldername/qualifiername-value[_qualifiername-value]/filename.qualifiername-value[_qualifiername-value].ext` .  
Exemple : `images/logo.scale-100_contrast-white.png` , qui peut faire référence au code en utilisant uniquement le dossier racine et le nom de fichier : `images/logo.png` .  
Pour plus d’informations, consultez la page Comment nommer des ressources à l’aide de qualificateurs situé dans [msdn.Microsoft.com/library/Windows/Apps/XAML/hh965324.aspx](/previous-versions/windows/apps/hh965324(v=win.10)).  

Microsoft vous recommande de marquer la langue par défaut des fichiers de ressources de chaîne (tels que `en-US\resources.resw` ) et le facteur d’échelle par défaut sur les images (comme `logo.scale-100.png` ), même si vous ne prévoyez pas de fournir actuellement des ressources localisées ou à plusieurs résolutions. Toutefois, Microsoft vous recommande, au minimum, de fournir des ressources pour les facteurs de mise à l’échelle 100, 200 et 400.  

>[!IMPORTANT]
> L’icône d’application utilisée dans la zone de titre de la zone de dessin **Cortana** est l’icône Square44x44Logo spécifiée dans le `Package.appxmanifest` fichier.  
> Vous pouvez également spécifier une icône pour chaque entrée dans la zone de contenu de la zone de dessin **Cortana** . Les tailles d’image valides pour les icônes de résultats sont :
>
> - 68W x 68h
> - 68W x 92h  
> - 280W x 140h  

La vignette de contenu n’est pas validée tant qu’un objet [**VoiceCommandResponse**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) n’est pas passé à la classe [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) . Si vous transmettez un objet [**VoiceCommandResponse**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) à **Cortana** qui comprend une vignette de contenu avec une image qui n’adhère pas à ces ratios de taille, une exception peut se produire.  

Exemple : l’application **Adventure Works** ( `VoiceCommandService\\AdventureWorksVoiceCommandService.cs` ) spécifie un carré simple, gris ( `GreyTile.png` ) sur la classe [**VoiceCommandContentTile**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTile) à l’aide du modèle de vignette [**TitleWith68x68IconAndText**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTileType) . Les variantes de logo se trouvent dans `VoiceCommandService\\Images` et sont extraites à l’aide de la méthode [**GetFileFromApplicationUriAsync**](/uwp/api/Windows.Storage.StorageFile) .

```csharp
var destinationTile = new VoiceCommandContentTile();  

destinationTile.ContentTileType = VoiceCommandContentTileType.TitleWith68x68IconAndText;
destinationTile.Image = await StorageFile.GetFileFromApplicationUriAsync(
    new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png")
);  
```

## <a name="create-an-app-service-project"></a>Créer un projet App Service

1. Cliquez avec le bouton droit sur le nom de votre solution, sélectionnez **nouveau > projet**.  
2. Sous **installé > modèles > Visual C \# > Windows > universel**, sélectionnez le **composant Windows Runtime**. Le **composant Windows Runtime** est le composant qui implémente l’app service ([**Windows. ApplicationModel. AppService**](/uwp/api/Windows.ApplicationModel.AppService)).  
3. Tapez un nom pour le projet et cliquez sur le bouton **OK** .  
    Exemple : `VoiceCommandService`.  
4. Dans **Explorateur de solutions**, sélectionnez le `VoiceCommandService` projet et renommez le `Class1.cs` fichier généré par Visual Studio.
    Exemple : **Adventure Works** utilise `AdventureWorksVoiceCommandService.cs` .  
5. Cliquez sur le bouton **Oui** . Lorsque vous y êtes invité, si vous souhaitez renommer toutes les occurrences de `Class1.cs` .  
6. Dans le fichier `AdventureWorksVoiceCommandService.cs` :
    1. Ajoutez la directive using suivante.  
        `using Windows.ApplicationModel.Background;`  
    2. Lorsque vous créez un nouveau projet, le nom du projet est utilisé comme espace de noms racine par défaut dans tous les fichiers. Renommez l’espace de noms pour imbriquer le code App service sous le projet principal.
        Exemple : `namespace AdventureWorks.VoiceCommands`.  
    3. Dans Explorateur de solutions, cliquez avec le bouton droit sur le nom du projet App service, puis sélectionnez **Propriétés**.  
    4. Sous l’onglet **bibliothèque** , mettez à jour le champ **espace de noms par défaut** avec cette même valeur.  
        Exemple : `AdventureWorks.VoiceCommands`.  
    5. Créez une nouvelle classe qui implémente l’interface [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) . Cette classe requiert une méthode [**Run**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) , qui est le point d’entrée lorsque Cortana reconnaît la commande vocale.  

    Exemple : une classe de tâche en arrière-plan de base de l’application **Adventure Works** .  

    >[!NOTE]
    > La classe de tâche d’arrière-plan elle-même, ainsi que toutes les classes dans le projet de tâche en arrière-plan, doivent être des classes publiques sealed.  

    ```csharp
    namespace AdventureWorks.VoiceCommands
    {
        ...
        
        /// <summary>
        /// The VoiceCommandService implements the entry point for all voice commands.
        /// The individual commands supported are described in the VCD xml file. 
        /// The service entry point is defined in the appxmanifest.
        /// </summary>
        public sealed class AdventureWorksVoiceCommandService : IBackgroundTask
        {
            ...

            /// <summary>
            /// The background task entrypoint. 
            /// 
            /// Background tasks must respond to activation by Cortana within 0.5 second, and must 
            /// report progress to Cortana every 5 seconds (unless Cortana is waiting for user
            /// input). There is no running time limit on the background task managed by Cortana,
            /// but developers should use plmdebug (https://msdn.microsoft.com/library/windows/hardware/jj680085%28v=vs.85%29.aspx)
            /// on the Cortana app package in order to prevent Cortana timing out the task during
            /// debugging.
            /// 
            /// The Cortana UI is dismissed if Cortana loses focus. 
            /// The background task is also dismissed even if being debugged. 
            /// Use of Remote Debugging is recommended in order to debug background task behaviors. 
            /// Open the project properties for the app package (not the background task project), 
            /// and enable Debug -> "Do not launch, but debug my code when it starts". 
            /// Alternatively, add a long initial progress screen, and attach to the background task process while it runs.
            /// </summary>
            /// <param name="taskInstance">Connection to the hosting background service process.</param>
            public void Run(IBackgroundTaskInstance taskInstance)
            {
              //
              // TODO: Insert code 
              //
              //
        }
      }
    }
    ```  

7. Déclarez votre tâche en arrière-plan en tant que **AppService** dans le manifeste de l’application.  
    1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le `Package.appxmanifest` fichier, puis sélectionnez **afficher le code**.  
    2. Recherchez l' [`Application`](/uwp/schemas/appxpackage/uapmanifestschema/element-application) élément.  
    3. Ajoutez un [`Extensions`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extensions) élément à l' [`Application`](/uwp/schemas/appxpackage/uapmanifestschema/element-application) élément.  
    4. Ajoutez un [`uap:Extension`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) élément à l' [`Extensions`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extensions) élément.  
    5. Ajoutez un `Category` attribut à l' `uap:Extension` élément et affectez `Category` à l’attribut la valeur `windows.appService` .  
    6. Ajoutez un `EntryPoint` attribut à l' `uap: Extension` élément et définissez la valeur de l' `EntryPoint` attribut sur le nom de la classe qui implémente [`IBackgroundTask`](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) .  
        Exemple : `AdventureWorks.VoiceCommands.AdventureWorksVoiceCommandService`.  
    7. Ajoutez un [`uap:AppService`](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-appservice) élément à l' `uap:Extension` élément.  
    8. Ajoutez un `Name` attribut à l' [`uap:AppService`](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-appservice) élément et affectez à la valeur de l' `Name` attribut un nom pour le service d’application, dans ce cas `AdventureWorksVoiceCommandService` .  
    9. Ajoutez un deuxième [`uap:Extension`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) élément à l' [`Extensions`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extensions) élément.  
    10. Ajoutez un `Category` attribut à cet [`uap:Extension`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) élément et affectez `Category` à l’attribut la valeur `windows.personalAssistantLaunch` .  

    Exemple : un manifeste de l’application Adventure Works.

    ```xml
    <Package>
        <Applications>
            <Application>
            
                <Extensions>
                    <uap:Extension Category="windows.appService" EntryPoint="CortanaBack1.VoiceCommands.AdventureWorksVoiceCommandService">
                        <uap:AppService Name="AdventureWorksVoiceCommandService"/>
                    </uap:Extension>
                    <uap:Extension Category="windows.personalAssistantLaunch"/>
                </Extensions>
                
            <Application>
        <Applications>
    </Package>
    ```  

8. Ajoutez ce projet App service comme référence dans le projet principal.  
    1. Cliquez avec le bouton droit sur les **références**.  
    2. Sélectionnez **Ajouter une référence...**.  
    3. Dans la boîte de dialogue **Gestionnaire de références** , développez **projets** et sélectionnez le projet App service.  
    4. Cliquez sur le bouton **OK** .  

## <a name="create-a-vcd-file"></a>Créer un fichier VCD

1. Dans Visual Studio, cliquez avec le bouton droit sur le nom de votre projet principal, puis sélectionnez **ajouter > nouvel élément**. Ajoutez un **fichier XML**.  
2. Tapez un nom pour le fichier [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) .  
    Exemple : `AdventureWorksCommands.xml`.
3. Cliquez sur le bouton **Ajouter** .  
4. Dans **Explorateur de solutions**, sélectionnez le fichier [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) .  
5. Dans la fenêtre **Propriétés** , affectez à **action de génération** la valeur **contenu**, puis définissez **copier dans le répertoire de sortie** sur **copier si plus récent**.  

## <a name="edit-the-vcd-file"></a>Modifier le fichier VCD  

1. Ajoutez un `VoiceCommands` élément avec un `xmlns` attribut pointant vers `https://schemas.microsoft.com/voicecommands/1.2` .  
2. Pour chaque langue prise en charge par votre application, créez un [`CommandSet`](/previous-versions/windows/dn722331(v=win.10)) élément qui comprend les commandes vocales prises en charge par votre application.  
    Vous pouvez déclarer plusieurs [`CommandSet`](/previous-versions/windows/dn722331(v=win.10)) éléments, chacun avec un [`xml:lang`](/previous-versions/windows/dn722331(v=win.10)) attribut différent afin que votre application soit utilisée sur différents marchés. Par exemple, une application pour le États-Unis peut avoir un [`CommandSet`](/previous-versions/windows/dn722331(v=win.10)) pour l’anglais et un pour l' [`CommandSet`](/previous-versions/windows/dn722331(v=win.10)) espagnol.  

    >[!IMPORTANT]
    > Pour activer une application et initier une action à l’aide d’une commande vocale, l’application doit inscrire un fichier VCD qui comprend un [`CommandSet`](/previous-versions/windows/dn722331(v=win.10)) élément avec une langue qui correspond à la langue vocale indiquée dans le périphérique de l’utilisateur. Le langage de reconnaissance vocale se trouve dans **paramètres > > langage de reconnaissance vocale >** vocale.  

3. Ajoutez un `Command` élément pour chaque commande que vous souhaitez prendre en charge.  
    Chaque `Command` déclaré dans un fichier [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) doit inclure les informations suivantes :  
    - `Name`Attribut utilisé par votre application pour identifier la commande vocale au moment de l’exécution.  
    - `Example`Élément qui contient une expression décrivant la manière dont un utilisateur appelle la commande. **Cortana** présente l’exemple lorsque l’utilisateur dit `What can I say?` , `Help` ou que des clics en **voient plus**.  
    - `ListenFor`Élément qui comprend les mots ou expressions que votre application reconnaît comme une commande. Chaque `ListenFor` élément peut contenir des références à un ou plusieurs `PhraseList` éléments qui contiennent des mots spécifiques relatifs à la commande.  

       >[!NOTE]
       > `ListenFor` les éléments ne doivent pas être modifiés par programmation. Toutefois, `PhraseList` les éléments associés à des `ListenFor` éléments peuvent être modifiés par programmation. Les applications doivent modifier le contenu de l' `PhraseList` élément au moment de l’exécution en fonction du jeu de données généré lorsque l’utilisateur utilise l’application.
       >
       > Pour plus d’informations, consultez [modifier dynamiquement les listes d’expressions VCD de Cortana](cortana-dynamically-modify-voice-command-definition-vcd-phrase-lists.md).  

    - `Feedback`Élément qui comprend le texte pour **Cortana** à afficher et à parler lorsque l’application est lancée.  

Un `Navigate` élément indique que la commande vocale active l’application au premier plan. Dans cet exemple, la ```showTripToDestination``` commande est une tâche de premier plan.  

Un `VoiceCommandService` élément indique que la commande vocale active l’application en arrière-plan. La valeur de l' `Target` attribut de cet élément doit correspondre à la valeur de l' `Name` attribut de l' [`uap:AppService`](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-appservice) élément dans le fichier Package. appxmanifest. Dans cet exemple, les `whenIsTripToDestination` `cancelTripToDestination` commandes et sont des tâches en arrière-plan qui spécifient le nom de l’app service sous la forme `AdventureWorksVoiceCommandService` .  

Pour plus d’informations, consultez les informations de référence sur les [**éléments et les attributs VCD v 1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) .  

Exemple : partie du fichier [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) qui définit les `en-us` commandes vocales pour l’application **Adventure Works** .  

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

>[!NOTE]
> Les données de commande vocale ne sont pas conservées entre les installations d’applications. Pour vous assurer que les données de commande vocale de votre application restent intactes, envisagez d’initialiser votre fichier VCD chaque fois que votre application est lancée ou activée, ou conservez un paramètre qui indique si le VCD est actuellement installé.  

Dans le fichier `app.xaml.cs` :  

1. Ajoutez la directive using suivante :

   ```csharp
   using Windows.Storage;
   ```

2. Marquez la `OnLaunched` méthode avec le modificateur Async.  

   ```csharp
   protected async override void OnLaunched(LaunchActivatedEventArgs e)
   ```  

3. Appelez la [`InstallCommandDefinitionsFromStorageFileAsync`](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager) méthode dans le [`OnLaunched`](/uwp/api/Windows.UI.Xaml.Application) Gestionnaire pour enregistrer les commandes vocales qui doivent être reconnues.  
    Exemple : l’application Adventure Works définit un [`StorageFile`](/uwp/api/Windows.Storage.StorageFile) objet.  
    Exemple : appelez la [`GetFileAsync`](/uwp/api/Windows.Storage.StorageFolder) méthode pour initialiser l' [`StorageFile`](/uwp/api/Windows.Storage.StorageFile) objet avec le `AdventureWorksCommands.xml` fichier.  
    L' [`StorageFile`](/uwp/api/Windows.Storage.StorageFile) objet est ensuite passé à la [`InstallCommandDefinitionsFromStorageFileAsync`](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager) méthode.  

   ```csharp
   try {
      // Install the main VCD. 
      StorageFile vcdStorageFile = await Package.Current.InstalledLocation.GetFileAsync(
            @"AdventureWorksCommands.xml"
      );
       
      await Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager.InstallCommandDefinitionsFromStorageFileAsync(vcdStorageFile);
     
      // Update phrase list.
      ViewModel.ViewModelLocator locator = App.Current.Resources["ViewModelLocator"] as ViewModel.ViewModelLocator;
      if(locator != null) {
            await locator.TripViewModel.UpdateDestinationPhraseList();
        }
    }
    catch (Exception ex) {
        System.Diagnostics.Debug.WriteLine("Installing Voice Commands Failed: " + ex.ToString());
    }
    ```  

## <a name="handle-activation"></a>Gérer l’activation  

Spécifiez le mode de réponse de votre application aux activations de commandes vocales suivantes.

>[!NOTE]
> Vous devez lancer votre application au moins une fois après avoir installé les jeux de commandes vocales.  

1. Vérifiez que votre application a été activée par une commande vocale.  

    Remplacez l' [`Application.OnActivated`](/uwp/api/Windows.UI.Xaml.Application) événement et vérifiez si [**IActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs).[**Le genre**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs) est [**VoiceCommand**](/uwp/api/Windows.ApplicationModel.Activation.ActivationKind).  

2. Déterminez le nom de la commande et ce qui a été parlé.  

    Obtenir une référence à un [`VoiceCommandActivatedEventArgs`](/uwp/api/Windows.ApplicationModel.Activation.VoiceCommandActivatedEventArgs) objet à partir du [**IActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs) et interroger la [`Result`](/uwp/api/Windows.ApplicationModel.Activation.VoiceCommandActivatedEventArgs) propriété d’un [`SpeechRecognitionResult`](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) objet.  

    Pour déterminer ce que l’utilisateur a dit, vérifiez la valeur du [**texte**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) ou les propriétés sémantiques de l’expression reconnue dans le [`SpeechRecognitionSemanticInterpretation`](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation) dictionnaire.  

3. Prenez les mesures appropriées dans votre application, comme naviguer jusqu’à la page souhaitée.  

    >[!NOTE]
    > Si vous devez faire référence à votre VCD, consultez la section [modifier le fichier VCD](#edit-the-vcd-file) .

    Après avoir reçu le résultat de reconnaissance vocale pour la commande vocale, vous obtenez le nom de la commande à partir de la première valeur dans le [`RulePath`](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) tableau. Étant donné que le fichier VCD définit plusieurs commandes vocales possibles, vous devez vérifier que la valeur correspond aux noms des commandes du VCD et prendre les mesures appropriées.  

    L’action la plus courante pour une application consiste à accéder à une page dont le contenu est pertinent dans le contexte de la commande vocale.  
    Exemple : Ouvrez la page de **déclenchement** et transmettez la valeur de la commande Voice, la façon dont la commande a été entrée et la phrase de destination reconnue (le cas échéant). L’application peut également envoyer un paramètre de navigation au [**SpeechRecognitionResult**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) lors de la navigation jusqu’à la page de **déclenchement** .  

    Vous êtes en mesure de déterminer si la commande vocale qui a lancé votre application a fait son discours ou si elle a été tapée sous forme de texte à partir du [`SpeechRecognitionSemanticInterpretation.Properties`](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation) dictionnaire à l’aide de la clé **commandMode** . La valeur de cette clé sera `voice` ou `text` . Si la valeur de la clé est `voice` , envisagez d’utiliser la synthèse vocale ([**Windows. Media. SpeechSynthesis**](/uwp/api/Windows.Media.SpeechSynthesis)) dans votre application pour fournir à l’utilisateur des commentaires prononcés.  

    Utilisez [**SpeechRecognitionSemanticInterpretation. Properties**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation) pour rechercher le contenu parlé dans les `PhraseList` `PhraseTopic` contraintes ou d’un `ListenFor` élément. La clé de dictionnaire est la valeur de l' `Label` attribut de `PhraseList` l' `PhraseTopic` élément ou.
    Exemple : code suivant pour accéder à la valeur de l’expression **{destination}** .  

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
    protected override void OnActivated(IActivatedEventArgs args) {
        base.OnActivated(args);
        
        Type navigationToPageType;
        ViewModel.TripVoiceCommand? navigationCommand = null;
        
        // Voice command activation.
        if (args.Kind == ActivationKind.VoiceCommand) {
            // Event args may represent many different activation types. 
            // Cast the args so that you only get useful parameters out.
            var commandArgs = args as VoiceCommandActivatedEventArgs;
            
            Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = commandArgs.Result;
            
            // Get the name of the voice command and the text spoken.
            // See VoiceCommands.xml for supported voice commands.
            string voiceCommandName = speechRecognitionResult.RulePath[0];
            string textSpoken = speechRecognitionResult.Text;
            
            // commandMode indicates whether the command was entered using speech or text.
            // Apps should respect text mode by providing silent (text) feedback.
            string commandMode = this.SemanticInterpretation("commandMode", speechRecognitionResult);
            
            switch (voiceCommandName) {
                case "showTripToDestination":
                    // Access the value of {destination} in the voice command.
                    string destination = this.SemanticInterpretation("destination", speechRecognitionResult);
                    
                    // Create a navigation command object to pass to the page.
                    navigationCommand = new ViewModel.TripVoiceCommand(
                        voiceCommandName,
                        commandMode,
                        textSpoken,
                        destination
                    );
              
                    // Set the page to navigate to for this voice command.
                    navigationToPageType = typeof(View.TripDetails);
                    break;
                default:
                    // If not able to determine what page to launch, then go to the default entry point.
                    navigationToPageType = typeof(View.TripListView);
                    break;
            }
        }
        // Protocol activation occurs when a card is selected within Cortana (using a background task).
        else if (args.Kind == ActivationKind.Protocol) {
            // Extract the launch context. In this case, use the destination from the phrase set (passed
            // along in the background task inside Cortana), which makes no attempt to be unique. A unique id or 
            // identifier is ideal for more complex scenarios. The destination page is left to check if the 
            // destination trip still exists, and navigate back to the trip list if it does not.
            var commandArgs = args as ProtocolActivatedEventArgs;
            Windows.Foundation.WwwFormUrlDecoder decoder = new Windows.Foundation.WwwFormUrlDecoder(commandArgs.Uri.Query);
            var destination = decoder.GetFirstValueByName("LaunchContext");
            
            navigationCommand = new ViewModel.TripVoiceCommand(
                "protocolLaunch",
                "text",
                "destination",
                destination
            );
            
            navigationToPageType = typeof(View.TripDetails);
        }
        else {
            // If launched using any other mechanism, fall back to the main page view.
            // Otherwise, the app will freeze at a splash screen.
            navigationToPageType = typeof(View.TripListView);
        }
        
        // Repeat the same basic initialization as OnLaunched() above, taking into account whether
        // or not the app is already active.
        Frame rootFrame = Window.Current.Content as Frame;
        
        // Do not repeat app initialization when the Window already has content,
        // just ensure that the window is active.
        if (rootFrame == null) {
            // Create a frame to act as the navigation context and navigate to the first page.
            rootFrame = new Frame();
            App.NavigationService = new NavigationService(rootFrame);
            
            rootFrame.NavigationFailed += OnNavigationFailed;
            
            // Place the frame in the current window.
            Window.Current.Content = rootFrame;
        }
        
        // Since the expectation is to always show a details page, navigate even if 
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
    private string SemanticInterpretation(string interpretationKey, SpeechRecognitionResult speechRecognitionResult) {
        return speechRecognitionResult.SemanticInterpretation.Properties[interpretationKey].FirstOrDefault();
    }
    ```  

## <a name="handle-the-voice-command-in-the-app-service"></a>Gérer la commande vocale dans le App Service  

Traitez la commande vocale dans App service.  

1. Ajoutez les directives using suivantes à votre fichier de service de commandes vocales.  
    Exemple : `AdventureWorksVoiceCommandService.cs`.  

    ```csharp
        using Windows.ApplicationModel.VoiceCommands;
        using Windows.ApplicationModel.Resources.Core;
        using Windows.ApplicationModel.AppService;
    ```  

2. Effectuez un report de service pour que votre application App service ne soit pas terminée lors du traitement de la commande vocale.  
3. Vérifiez que votre tâche en arrière-plan s’exécute en tant que service d’application activé par une commande vocale.  
    1. Effectuez un cast de [**IBackgroundTaskInstance. TriggerDetails**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance) en [**Windows. ApplicationModel. AppService. AppServiceTriggerDetails**](/uwp/api/Windows.ApplicationModel.AppService.AppServiceTriggerDetails).  
    2. Vérifiez que [**IBackgroundTaskInstance.TriggerDetails.Name**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskRegistration) est le nom de l’app service dans le `Package.appxmanifest` fichier.  
4. Utilisez [**IBackgroundTaskInstance. TriggerDetails**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance) pour créer un [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) pour que **Cortana** récupère la commande vocale.
5. Inscrivez un gestionnaire d’événements pour [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection).  [**VoiceCommandCompleted**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) pour recevoir une notification lorsque l’app service est fermé en raison d’une annulation de l’utilisateur.  
6. Inscrivez un gestionnaire d’événements pour [**IBackgroundTaskInstance. Canceled**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance) pour recevoir une notification lorsque l’app service est fermé en raison d’une défaillance inattendue.  
7. Déterminez le nom de la commande et ce qui a été parlé.  
    1. Utilisez [**VoiceCommand**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommand). Propriété [**CommandName**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommand) pour déterminer le nom de la commande vocale.  
    2. Pour déterminer ce que l’utilisateur a dit, vérifiez la valeur du [**texte**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) ou les propriétés sémantiques de l’expression reconnue dans le [`SpeechRecognitionSemanticInterpretation`](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation) dictionnaire.  
8. Prenez les mesures appropriées dans votre App service.  
9. Affichez et parlez les commentaires à la commande vocale à l’aide de **Cortana**.  
    1. Déterminez les chaînes que **Cortana** doit afficher et parlez à l’utilisateur en réponse à la commande vocale et créez un [`VoiceCommandResponse`](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) objet. Pour obtenir des conseils sur la façon de sélectionner les chaînes de commentaires que **Cortana** affiche et parle, consultez [instructions de conception de Cortana](cortana-design-guidelines.md).  
    2. Utilisez l’instance [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) pour signaler la progression ou l’achèvement à **Cortana** en appelant [**ReportProgressAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) ou [**ReportSuccessAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) avec l' `VoiceCommandServiceConnection` objet.  

    >[!NOTE]
    > Si vous devez faire référence à votre VCD, consultez la section [modifier le fichier VCD](#edit-the-vcd-file) .  

    ```csharp
    public sealed class VoiceCommandService : IBackgroundTask {
        private BackgroundTaskDeferral serviceDeferral;
        VoiceCommandServiceConnection voiceServiceConnection;
        
        public async void Run(IBackgroundTaskInstance taskInstance) {
            //Take a service deferral so the service isn&#39;t terminated.
            this.serviceDeferral = taskInstance.GetDeferral();
            
            taskInstance.Canceled += OnTaskCanceled;
            
            var triggerDetails = taskInstance.TriggerDetails as AppServiceTriggerDetails;
    
            if (triggerDetails != null &amp;&amp; 
                triggerDetails.Name == "AdventureWorksVoiceServiceEndpoint") {
                try {
                    voiceServiceConnection = 
                    VoiceCommandServiceConnection.FromAppServiceTriggerDetails(
                        triggerDetails);
                    voiceServiceConnection.VoiceCommandCompleted += 
                    VoiceCommandCompleted;
                    
                    VoiceCommand voiceCommand = await 
                    voiceServiceConnection.GetVoiceCommandAsync();
                    
                    switch (voiceCommand.CommandName) {
                        case "whenIsTripToDestination":
                            {
                                var destination = 
                                voiceCommand.Properties["destination"][0];
                                SendCompletionMessageForDestination(destination);
                                break;
                            }
                            
                            // As a last resort, launch the app in the foreground.
                        default:
                            LaunchAppInForeground();
                            break;
                    }
                }
                finally {
                    if (this.serviceDeferral != null) {
                        // Complete the service deferral.
                        this.serviceDeferral.Complete();
                    }
                }
            }
        }
        
        private void VoiceCommandCompleted(VoiceCommandServiceConnection sender,
            VoiceCommandCompletedEventArgs args) {
            if (this.serviceDeferral != null) {
                // Insert your code here.
                // Complete the service deferral.
                this.serviceDeferral.Complete();
            }
        }
        
        private async void SendCompletionMessageForDestination(
            string destination) {
            // Take action and determine when the next trip to destination
            // Insert code here.
            
            // Replace the hardcoded strings used here with strings 
            // appropriate for your application.
            
            // First, create the VoiceCommandUserMessage with the strings 
            // that Cortana will show and speak.
            var userMessage = new VoiceCommandUserMessage();
            userMessage.DisplayMessage = "Here's your trip.";
            userMessage.SpokenMessage = "Your trip to Vegas is on August 3rd.";
            
            // Optionally, present visual information about the answer.
            // For this example, create a VoiceCommandContentTile with an 
            // icon and a string.
            var destinationsContentTiles = new List<VoiceCommandContentTile>();
            
            var destinationTile = new VoiceCommandContentTile();
            destinationTile.ContentTileType = 
                VoiceCommandContentTileType.TitleWith68x68IconAndText;
            // The user taps on the visual content to launch the app. 
            // Pass in a launch argument to enable the app to deep link to a 
            // page relevant to the item displayed on the content tile.
            destinationTile.AppLaunchArgument = 
                string.Format("destination={0}", "Las Vegas");
            destinationTile.Title = "Las Vegas";
            destinationTile.TextLine1 = "August 3rd 2015";
            destinationsContentTiles.Add(destinationTile);
            
            // Create the VoiceCommandResponse from the userMessage and list    
            // of content tiles.
            var response = VoiceCommandResponse.CreateResponse(
                userMessage, destinationsContentTiles);
            
            // Cortana displays a "Go to app_name" link that the user 
            // taps to launch the app. 
            // Pass in a launch to enable the app to deep link to a page 
            // relevant to the voice command.
            response.AppLaunchArgument = string.Format(
                "destination={0}", "Las Vegas");
            
            // Ask Cortana to display the user message and content tile and 
            // also speak the user message.
            await voiceServiceConnection.ReportSuccessAsync(response);
        }
        
        private async void LaunchAppInForeground() {
            var userMessage = new VoiceCommandUserMessage();
            userMessage.SpokenMessage = "Launching Adventure Works";
            
            var response = VoiceCommandResponse.CreateResponse(userMessage);
            
            // When launching the app in the foreground, pass an app 
            // specific launch parameter to indicate what page to show.
            response.AppLaunchArgument = "showAllTrips=true";
            
            await voiceServiceConnection.RequestAppLaunchAsync(response);
        }
    }
    ```  

Une fois activé, app service dispose de 0,5 seconde pour appeler [**ReportSuccessAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection). **Cortana** affiche et indique une chaîne de commentaires.  

>[!NOTE]
> Vous pouvez déclarer une chaîne de **Commentaires** dans le fichier VCD. La chaîne n’affecte pas le texte de l’interface utilisateur affiché sur le canevas Cortana, elle affecte uniquement le texte parlé par **Cortana**.  

Si l’application prend plus de 0,5 seconde pour effectuer l’appel, **Cortana** insère un écran main, comme illustré ici. **Cortana** affiche l’écran de mise hors connexion jusqu’à ce que l’application appelle **ReportSuccessAsync** ou jusqu’à 5 secondes. Si le service d’application n’appelle pas **ReportSuccessAsync**, ou l’une des [`VoiceCommandServiceConnection`](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) méthodes qui fournissent **Cortana** avec les informations, l’utilisateur reçoit un message d’erreur et l’app service est annulé.  

:::image type="content" source="images/cortana/cortana-backgroundapp-progress-result.png" alt-text="Capture d’écran de Cortana et d’une requête de base avec des écrans de progression et de résultats à l’aide de l’application AdventureWorks en arrière-plan":::

## <a name="related-articles"></a>Articles connexes

- [Interactions Cortana dans les applications Windows](cortana-interactions.md)
- [Activer une application au premier plan avec les commandes vocales via Cortana](cortana-launch-a-foreground-app-with-voice-commands.md)
- [Éléments et attributs d’un fichier VCD v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Instructions de conception de Cortana](cortana-design-guidelines.md)
- [Exemple de commande vocale Cortana](https://go.microsoft.com/fwlink/p/?LinkID=619899)
