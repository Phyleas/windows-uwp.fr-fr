---
ms.assetid: ''
title: Prendre en charge la numérotation en surface (et les autres appareils volants) dans votre application Windows
description: Un didacticiel pas à pas pour l’ajout de la prise en charge de l’accès en surface (et des autres appareils volants) à votre application Windows.
keywords: composer, radial, didacticiel
ms.date: 03/11/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3972e04c59748efabd51b423f6f24fc22291a6d1
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234893"
---
# <a name="tutorial-support-the-surface-dial-and-other-wheel-devices-in-your-windows-app"></a>Didacticiel : prendre en charge la numérotation en surface (et les autres appareils volants) dans votre application Windows

![Image de Surface Dial avec Surface Studio](images/radialcontroller/dial-pen-studio-600px.png)  
*Cadran de surface avec surface Studio et PEN surface* (disponible à l’achat au [Microsoft Store](https://www.microsoft.com/store/d/Surface-Dial/925R551SKTGN?icid=Surface_Accessories_ModB_Surface_Dial_103116)).

Ce didacticiel décrit comment personnaliser les expériences d’interaction utilisateur prises en charge par les appareils volants, tels que le cadran de surface. Nous utilisons des extraits de code à partir d’un exemple d’application, que vous pouvez télécharger à partir de GitHub (Voir l' [exemple de code](#sample-code)), pour illustrer les différentes fonctionnalités et les API [**RadialController**](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller) associées abordées dans chaque étape.

Nous nous concentrons sur les éléments suivants :
* Spécification des outils intégrés à afficher dans le menu [**RadialController**](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller)
* Ajout d’un outil personnalisé au menu
* Contrôle des commentaires haptique
* Personnalisation des interactions de clic
* Personnalisation des interactions de rotation

Pour plus d’informations sur l’implémentation de ces fonctionnalités et d’autres fonctionnalités, consultez [interactions entre les appels en surface dans les applications Windows](windows-wheel-interactions.md).

## <a name="introduction"></a>Introduction

La numérotation en surface est un appareil d’entrée secondaire qui permet aux utilisateurs d’être plus productifs lorsqu’ils sont utilisés avec un périphérique d’entrée principal tel qu’un stylet, une pression tactile ou une souris. En tant qu’appareil d’entrée secondaire, le cadran est généralement utilisé avec la main non dominante pour fournir un accès aux commandes système et à d’autres outils et fonctionnalités plus contextuels. 

La numérotation prend en charge trois gestes de base : 
- Appuyez et maintenez la touche enfoncée pour afficher le menu intégré des commandes.
- Faites pivoter pour mettre en surbrillance un élément de menu (si le menu est actif) ou pour modifier l’action en cours dans l’application (si le menu n’est pas actif).
- Cliquez pour sélectionner l’élément de menu en surbrillance (si le menu est actif) ou pour appeler une commande dans l’application (si le menu n’est pas actif).

## <a name="prerequisites"></a>Prérequis

* Un ordinateur (ou une machine virtuelle) exécutant Windows 10 Creators Update, ou une version plus récente
* [Visual Studio 2019](https://developer.microsoft.com/windows/downloads)
* [SDK Windows 10 (10.0.15063.0)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* Appareil volant (uniquement la [surface de numérotation](https://www.microsoft.com/store/d/Surface-Dial/925R551SKTGN?icid=Surface_Accessories_ModB_Surface_Dial_103116) à l’heure actuelle)
* Si vous ne connaissez pas le développement d’applications Windows avec Visual Studio, consultez les rubriques suivantes avant de commencer ce didacticiel :  
    * [Se préparer](https://docs.microsoft.com/windows/uwp/get-started/get-set-up)
    * [Créer une application « Hello World » (XAML)](https://docs.microsoft.com/windows/uwp/get-started/create-a-hello-world-app-xaml-universal)

## <a name="set-up-your-devices"></a>Configurer vos appareils

1. Vérifiez que votre appareil Windows est sous tension.
2. Accédez à **Démarrer**, sélectionnez **paramètres**  >  **appareils**  >  **Bluetooth & autres appareils**, puis activez la fonction **Bluetooth** .
3. Retirez le bas de la surface de cadran pour ouvrir le compartiment de la batterie, et assurez-vous qu’il y a deux piles AAA dans.
4. Si l’onglet batterie est présent sur le dessous de la composition, retirez-le.
5. Appuyez et maintenez enfoncé le petit bouton incrusté à côté des piles jusqu’à ce que la lumière Bluetooth clignote.
6. Revenez à votre appareil Windows et sélectionnez **Ajouter Bluetooth ou autre périphérique**.
7. Dans la boîte de dialogue **Ajouter un périphérique** , sélectionnez **Bluetooth**  >  **connexion à la surface**Bluetooth. Votre composition de surface doit maintenant se connecter et être ajoutée à la liste des appareils sous **souris, clavier, & stylet** dans la page paramètres **Bluetooth & autres appareils** .
8. Testez la numérotation en appuyant sur la touche et maintenez-la enfoncée pendant quelques secondes pour afficher le menu intégré.
9. Si le menu n’est pas affiché sur votre écran (le cadran doit également vibrer), revenez aux paramètres Bluetooth, retirez l’appareil et réessayez de connecter l’appareil.

> [!NOTE]
> Les appareils roue peuvent être configurés à l’aide des paramètres de la **Roulette** :
> 1. Dans le menu **Démarrer** , sélectionnez **paramètres**.
> 2. Sélectionnez **Devices**  >  **volant**d’appareils.    
> ![Écran Paramètres de la roue](images/radialcontroller/wheel-settings.png)

Vous êtes maintenant prêt à commencer ce didacticiel. 

## <a name="sample-code"></a>Exemple de code
Tout au long de ce didacticiel, nous utilisons un exemple d’application pour illustrer les concepts et les fonctionnalités abordés.

Téléchargez cet exemple Visual Studio et le code source à partir de [GitHub](https://github.com/) à l' [exemple Windows-appsample-to-Started-radialcontroller](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-RadialController):

1. Sélectionnez le bouton de **clonage ou de téléchargement** vert.  
![Clonage du référentiel](images/radialcontroller/wheel-clone.png)
2. Si vous avez un compte GitHub, vous pouvez cloner le référentiel sur votre ordinateur local en choisissant **ouvrir dans Visual Studio**. 
3. Si vous n’avez pas de compte GitHub, ou si vous souhaitez simplement une copie locale du projet, choisissez **Télécharger zip** (vous devrez consulter régulièrement les dernières mises à jour).

> [!IMPORTANT]
> La majeure partie du code de l’exemple est commentée. Tout au long de chaque étape de cette rubrique, vous serez invité à supprimer les marques de commentaire de diverses sections du code. Dans Visual Studio, il vous suffit de mettre en surbrillance les lignes de code, puis d’appuyer sur CTRL-K, puis de CTRL + U.

## <a name="components-that-support-wheel-functionality"></a>Composants qui prennent en charge la fonctionnalité de roulette

Ces objets fournissent la majeure partie de l’expérience de l’appareil roue pour les applications Windows.

| Composant | Description |
| --- | --- |
| [Classe **RadialController** ](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialController) et connexe | Représente un appareil d’entrée de roue ou un accessoire tel que le cadran de surface. |
| [**IRadialControllerConfigurationInterop**](https://docs.microsoft.com/previous-versions/windows/desktop/api/radialcontrollerinterop/nn-radialcontrollerinterop-iradialcontrollerconfigurationinterop)  /  [ **IRadialControllerInterop**](https://docs.microsoft.com/previous-versions/windows/desktop/api/radialcontrollerinterop/nn-radialcontrollerinterop-iradialcontrollerinterop)<br/>Nous ne couvrons pas cette fonctionnalité ici. pour plus d’informations, consultez l' [exemple de bureau classique Windows](https://github.com/Microsoft/Windows-classic-samples/tree/master/Samples/RadialController). | Permet l’interopérabilité avec une application Windows. |

## <a name="step-1-run-the-sample"></a>Étape 1 : exécuter l’exemple

Après avoir téléchargé l’exemple d’application RadialController, vérifiez qu’il s’exécute :
1. Ouvrez l’exemple de projet dans Visual Studio.
2. Définissez la liste déroulante **plateformes solution** sur une sélection non-arm.
3. Appuyez sur F5 pour compiler, déployer et exécuter. 

> [!NOTE]
> Vous pouvez également sélectionner l’élément de menu **Déboguer**  >  **Démarrer le débogage** , ou sélectionner le bouton d’exécution de l' **ordinateur local** indiqué ici : ![ bouton projet de génération Visual Studio](images/radialcontroller/wheel-vsrun.png)

La fenêtre d’application s’ouvre, et une fois que l’écran de démarrage s’affiche pendant quelques secondes, vous verrez cet écran initial.

![Application vide](images/radialcontroller/wheel-app-step1-empty.png)

Nous avons maintenant l’application Windows de base que nous allons utiliser dans le reste de ce didacticiel. Dans les étapes suivantes, nous ajoutons nos fonctionnalités **RadialController** .

## <a name="step-2-basic-radialcontroller-functionality"></a>Étape 2 : fonctionnalités de base de RadialController

Une fois l’application en cours d’exécution et au premier plan, appuyez sur la molette de la surface pour afficher le menu **RadialController** .

Nous n’avons pas encore effectué de personnalisation pour notre application. le menu contient donc un ensemble d’outils contextuels par défaut. 

Ces images affichent deux variantes du menu par défaut. (Il en existe beaucoup d’autres, notamment les outils système de base lorsque le bureau Windows est actif et qu’aucune application n’est au premier plan, des outils d’entrée manuscrite supplémentaires quand un InkToolbar est présent et des outils de mappage lorsque vous utilisez l’application Maps.

| Menu RadialController (par défaut)  | Menu RadialController (par défaut avec la fonction de diffusion multimédia)  |
|---|---|
| ![Menu RadialController par défaut](images/radialcontroller/wheel-app-step2-basic-default.png) | ![Menu RadialController par défaut avec musique](images/radialcontroller/wheel-app-step2-basic-withmusic.png) |

À présent, nous allons commencer par une personnalisation de base.

## <a name="step-3-add-controls-for-wheel-input"></a>Étape 3 : ajouter des contrôles pour l’entrée de roue

Tout d’abord, nous allons ajouter l’interface utilisateur pour notre application :

1. Ouvrez le fichier MainPage_Basic. Xaml.
2. Recherchez le code marqué avec le titre de cette étape (« \< !--étape 3 : ajouter des contrôles pour une entrée de roue--> »).
3. Supprimez les marques de commentaire des lignes suivantes.

    ```xaml
    <Button x:Name="InitializeSampleButton" 
            HorizontalAlignment="Center" 
            Margin="10" 
            Content="Initialize sample" />
    <ToggleButton x:Name="AddRemoveToggleButton"
                    HorizontalAlignment="Center" 
                    Margin="10" 
                    Content="Remove Item"
                    IsChecked="True" 
                    IsEnabled="False"/>
    <Button x:Name="ResetControllerButton" 
            HorizontalAlignment="Center" 
            Margin="10" 
            Content="Reset RadialController menu" 
            IsEnabled="False"/>
    <Slider x:Name="RotationSlider" Minimum="0" Maximum="10"
            Width="300"
            HorizontalAlignment="Center"/>
    <TextBlock Text="{Binding ElementName=RotationSlider, Mode=OneWay, Path=Value}"
                Margin="0,0,0,20"
                HorizontalAlignment="Center"/>
    <ToggleSwitch x:Name="ClickToggle"
                    MinWidth="0" 
                    Margin="0,0,0,20"
                    HorizontalAlignment="center"/>
    ```
À ce stade, seuls le bouton **initialiser l’exemple** , le curseur et le commutateur bascule sont activés. Les autres boutons sont utilisés dans les étapes ultérieures pour ajouter et supprimer des éléments de menu **RadialController** qui fournissent l’accès au curseur et au commutateur bascule.

![Interface utilisateur de l’exemple d’application de base](images/radialcontroller/wheel-app-step3-basicui.png)

## <a name="step-4-customize-the-basic-radialcontroller-menu"></a>Étape 4 : personnaliser le menu RadialController de base

À présent, nous allons ajouter le code requis pour permettre à **RadialController** d’accéder à nos contrôles.

1. Ouvrez le fichier MainPage_Basic. Xaml. cs.
2. Recherchez le code marqué avec le titre de cette étape (« //étape 4 : personnalisation du menu RadialController de base »).
3. Supprimez les marques de commentaire des lignes suivantes :
    - Les références de type [Windows. UI. Input](https://docs.microsoft.com/uwp/api/windows.ui.input) et [Windows. Storage. streams](https://docs.microsoft.com/uwp/api/windows.storage.streams) sont utilisées pour les fonctionnalités des étapes suivantes :  
    
        ```csharp
        // Using directives for RadialController functionality.
        using Windows.UI.Input;
        ```

    - Ces objets globaux ([RadialController](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller), [RadialControllerConfiguration](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollerconfiguration), [RadialControllerMenuItem](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollermenuitem)) sont utilisés dans l’ensemble de l’application.
    
        ```csharp
        private RadialController radialController;
        private RadialControllerConfiguration radialControllerConfig;
        private RadialControllerMenuItem radialControllerMenuItem;
        ```

    - Ici, nous spécifions le gestionnaire de **clics** pour le bouton qui active nos contrôles et initialise notre élément de menu **RadialController** personnalisé.

        ```csharp
        InitializeSampleButton.Click += (sender, args) =>
        { InitializeSample(sender, args); };
        ``` 

    - Nous allons ensuite initialiser notre objet [RadialController](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller) et configurer des gestionnaires pour les événements [RotationChanged](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.RotationChanged) et [ButtonClicked](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ButtonClicked) .

        ```csharp
        // Set up the app UI and RadialController.
        private void InitializeSample(object sender, RoutedEventArgs e)
        {
            ResetControllerButton.IsEnabled = true;
            AddRemoveToggleButton.IsEnabled = true;

            ResetControllerButton.Click += (resetsender, args) =>
            { ResetController(resetsender, args); };
            AddRemoveToggleButton.Click += (togglesender, args) =>
            { AddRemoveItem(togglesender, args); };

            InitializeController(sender, e);
        }
        ```

    - Ici, nous allons initialiser notre élément de menu RadialController personnalisé. Nous utilisons [CreateForCurrentView](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.CreateForCurrentView) pour obtenir une référence à notre objet [RadialController](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller) , nous définissons le critère de rotation sur « 1 » à l’aide de la propriété [RotationResolutionInDegrees](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.RotationResolutionInDegrees) , nous créons ensuite notre [RadialControllerMenuItem](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollermenuitem) à l’aide de [CreateFromFontGlyph](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollermenuitem.CreateFromFontGlyph), nous ajoutons l’élément de menu à la collection d’éléments de menu **RadialController** , puis nous utilisons [SetDefaultMenuItems](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollerconfiguration.setdefaultmenuitems) pour effacer les éléments de menu par défaut et conserver uniquement notre outil personnalisé. 

        ```csharp
        // Configure RadialController menu and custom tool.
        private void InitializeController(object sender, RoutedEventArgs args)
        {
            // Create a reference to the RadialController.
            radialController = RadialController.CreateForCurrentView();
            // Set rotation resolution to 1 degree of sensitivity.
            radialController.RotationResolutionInDegrees = 1;

            // Create the custom menu items.
            // Here, we use a font glyph for our custom tool.
            radialControllerMenuItem =
                RadialControllerMenuItem.CreateFromFontGlyph("SampleTool", "\xE1E3", "Segoe MDL2 Assets");

            // Add the item to the RadialController menu.
            radialController.Menu.Items.Add(radialControllerMenuItem);

            // Remove built-in tools to declutter the menu.
            // NOTE: The Surface Dial menu must have at least one menu item. 
            // If all built-in tools are removed before you add a custom 
            // tool, the default tools are restored and your tool is appended 
            // to the default collection.
            radialControllerConfig =
                RadialControllerConfiguration.GetForCurrentView();
            radialControllerConfig.SetDefaultMenuItems(
                new RadialControllerSystemMenuItemKind[] { });

            // Declare input handlers for the RadialController.
            // NOTE: These events are only fired when a custom tool is active.
            radialController.ButtonClicked += (clicksender, clickargs) =>
            { RadialController_ButtonClicked(clicksender, clickargs); };
            radialController.RotationChanged += (rotationsender, rotationargs) =>
            { RadialController_RotationChanged(rotationsender, rotationargs); };
        }

        // Connect wheel device rotation to slider control.
        private void RadialController_RotationChanged(
            object sender, RadialControllerRotationChangedEventArgs args)
        {
            if (RotationSlider.Value + args.RotationDeltaInDegrees >= RotationSlider.Maximum)
            {
                RotationSlider.Value = RotationSlider.Maximum;
            }
            else if (RotationSlider.Value + args.RotationDeltaInDegrees < RotationSlider.Minimum)
            {
                RotationSlider.Value = RotationSlider.Minimum;
            }
            else
            {
                RotationSlider.Value += args.RotationDeltaInDegrees;
            }
        }

        // Connect wheel device click to toggle switch control.
        private void RadialController_ButtonClicked(
            object sender, RadialControllerButtonClickedEventArgs args)
        {
            ClickToggle.IsOn = !ClickToggle.IsOn;
        }
        ```
4. À présent, réexécutez l’application.
5. Sélectionnez le bouton **initialiser le contrôleur radial** .  
6. Une fois l’application au premier plan, appuyez sur la molette et maintenez-la enfoncée pour afficher le menu. Notez que tous les outils par défaut ont été supprimés (à l’aide de la méthode **RadialControllerConfiguration. SetDefaultMenuItems** ), laissant uniquement l’outil personnalisé. Voici le menu avec notre outil personnalisé. 

| Menu RadialController (personnalisé)  | 
|---|
| ![Menu RadialController personnalisé](images/radialcontroller/wheel-app-step3-custom.png) |

7. Sélectionnez l’outil personnalisé et essayez les interactions prises en charge à partir de la surface de cadran :
    * Une action de rotation déplace le curseur. 
    * Un clic définit le bouton bascule sur activé ou désactivé.

À présent, accrochez-les.

## <a name="step-5-configure-menu-at-runtime"></a>Étape 5 : configurer le menu au moment de l’exécution

Dans cette étape, nous raccordons les boutons de menu **Ajouter/supprimer un élément** et **Réinitialiser les RadialController** pour montrer comment vous pouvez personnaliser le menu de manière dynamique.

1. Ouvrez le fichier MainPage_Basic. Xaml. cs.
2. Recherchez le code marqué avec le titre de cette étape (« //étape 5 : configurer le menu au moment de l’exécution »).
3. Supprimez les marques de commentaire du code dans les méthodes suivantes et réexécutez l’application, mais ne sélectionnez aucun bouton (enregistrez-le pour l’étape suivante).  

    ``` csharp
    // Add or remove the custom tool.
    private void AddRemoveItem(object sender, RoutedEventArgs args)
    {
        if (AddRemoveToggleButton?.IsChecked == true)
        {
            AddRemoveToggleButton.Content = "Remove item";
            if (!radialController.Menu.Items.Contains(radialControllerMenuItem))
            {
                radialController.Menu.Items.Add(radialControllerMenuItem);
            }
        }
        else if (AddRemoveToggleButton?.IsChecked == false)
        {
            AddRemoveToggleButton.Content = "Add item";
            if (radialController.Menu.Items.Contains(radialControllerMenuItem))
            {
                radialController.Menu.Items.Remove(radialControllerMenuItem);
                // Attempts to select and activate the previously selected tool.
                // NOTE: Does not differentiate between built-in and custom tools.
                radialController.Menu.TrySelectPreviouslySelectedMenuItem();
            }
        }
    }

    // Reset the RadialController to initial state.
    private void ResetController(object sender, RoutedEventArgs arg)
    {
        if (!radialController.Menu.Items.Contains(radialControllerMenuItem))
        {
            radialController.Menu.Items.Add(radialControllerMenuItem);
        }
        AddRemoveToggleButton.Content = "Remove item";
        AddRemoveToggleButton.IsChecked = true;
        radialControllerConfig.SetDefaultMenuItems(
            new RadialControllerSystemMenuItemKind[] { });
    }
    ```
4. Sélectionnez le bouton **supprimer un élément** , puis appuyez sur la molette et maintenez-la enfoncée pour afficher à nouveau le menu.

    Notez que le menu contient maintenant la collection d’outils par défaut. Rappelez-vous que, à l’étape 3, lors de la configuration de notre menu personnalisé, nous avons supprimé tous les outils par défaut et ajouté uniquement notre outil personnalisé. Nous avons également noté que, lorsque le menu est défini sur une collection vide, les éléments par défaut du contexte actuel sont rétablis. (Nous avons ajouté notre outil personnalisé avant de supprimer les outils par défaut.)

5. Sélectionnez le bouton **Ajouter un élément** , puis appuyez sur la molette et maintenez-la enfoncée.

    Notez que le menu contient à présent la collection par défaut d’outils et notre outil personnalisé.

6. Sélectionnez le bouton de **menu Réinitialiser RadialController** , puis appuyez sur la molette et maintenez-la enfoncée.

    Notez que le menu revient à son état d’origine.

## <a name="step-6-customize-the-device-haptics"></a>Étape 6 : personnaliser les éléments haptique de l’appareil
La numérotation en surface et les autres appareils roue peuvent fournir aux utilisateurs des commentaires haptique correspondant à l’interaction actuelle (en fonction d’un clic ou d’une rotation).

Dans cette étape, nous montrons comment vous pouvez personnaliser les commentaires haptique en associant nos curseurs et les contrôles de basculement, et en les utilisant pour spécifier dynamiquement le comportement de commentaires haptique. Pour cet exemple, le commutateur Toggle doit avoir la valeur on pour que les commentaires soient activés alors que la valeur de Slider spécifie la fréquence à laquelle le feedback de clic est répété. 

> [!NOTE]
> Les commentaires haptique peuvent être désactivés par l’utilisateur dans la page **paramètres**  >   **Devices**  >  **roulette** appareils.

1. Ouvrez le fichier App.xaml.cs.
2. Recherchez le code marqué avec le titre de cette étape (« Step 6 : Customize the Device haptiques »).
3. Commentez les première et troisième lignes (« MainPage_Basic » et « MainPage ») et supprimez les marques de commentaire de la seconde (« MainPage_Haptics »).  

    ``` csharp
    rootFrame.Navigate(typeof(MainPage_Basic), e.Arguments);
    rootFrame.Navigate(typeof(MainPage_Haptics), e.Arguments);
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
    ```
4. Ouvrez le fichier MainPage_Haptics. Xaml.
5. Recherchez le code marqué avec le titre de cette étape (« \< !--Step 6 : Customize the Device haptiques--> »).
6. Supprimez les marques de commentaire des lignes suivantes. (Ce code d’interface utilisateur indique simplement les fonctionnalités haptique qui sont prises en charge par l’appareil actuel.)    

    ```xaml
    <StackPanel x:Name="HapticsStack" 
                Orientation="Vertical" 
                HorizontalAlignment="Center" 
                BorderBrush="Gray" 
                BorderThickness="1">
        <TextBlock Padding="10" 
                    Text="Supported haptics properties:" />
        <CheckBox x:Name="CBDefault" 
                    Content="Default" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsChecked="True" />
        <CheckBox x:Name="CBIntensity" 
                    Content="Intensity" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBPlayCount" 
                    Content="Play count" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBPlayDuration" 
                    Content="Play duration" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBReplayPauseInterval" 
                    Content="Replay/pause interval" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBBuzzContinuous" 
                    Content="Buzz continuous" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBClick" 
                    Content="Click" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBPress" 
                    Content="Press" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBRelease" 
                    Content="Release" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBRumbleContinuous" 
                    Content="Rumble continuous" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
    </StackPanel>
    ```
7. Ouvrir le fichier MainPage_Haptics. Xaml. cs
8. Recherchez le code marqué avec le titre de cette étape (« étape 6 : personnalisation haptique »)
9. Supprimez les marques de commentaire des lignes suivantes :  

    - La référence de type [Windows. Devices. haptiques](https://docs.microsoft.com/uwp/api/windows.devices.haptics) est utilisée pour les fonctionnalités dans les étapes suivantes.  
    
        ```csharp
        using Windows.Devices.Haptics;
        ```

    - Ici, nous spécifions le gestionnaire pour l’événement [ControlAcquired](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ControlAcquired) qui est déclenché lorsque notre élément de menu personnalisé **RadialController** est sélectionné.

        ```csharp
        radialController.ControlAcquired += (rc_sender, args) =>
        { RadialController_ControlAcquired(rc_sender, args); };
        ``` 

    - Ensuite, nous définissons le gestionnaire [ControlAcquired](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ControlAcquired) , où nous désactivons les commentaires haptique par défaut et initialiserons notre interface utilisateur haptique.

        ```csharp
        private void RadialController_ControlAcquired(
            RadialController rc_sender,
            RadialControllerControlAcquiredEventArgs args)
        {
            // Turn off default haptic feedback.
            radialController.UseAutomaticHapticFeedback = false;

            SimpleHapticsController hapticsController =
                args.SimpleHapticsController;

            // Enumerate haptic support.
            IReadOnlyCollection<SimpleHapticsControllerFeedback> supportedFeedback =
                hapticsController.SupportedFeedback;

            foreach (SimpleHapticsControllerFeedback feedback in supportedFeedback)
            {
                if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.BuzzContinuous)
                {
                    CBBuzzContinuous.IsEnabled = true;
                    CBBuzzContinuous.IsChecked = true;
                }
                else if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.Click)
                {
                    CBClick.IsEnabled = true;
                    CBClick.IsChecked = true;
                }
                else if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.Press)
                {
                    CBPress.IsEnabled = true;
                    CBPress.IsChecked = true;
                }
                else if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.Release)
                {
                    CBRelease.IsEnabled = true;
                    CBRelease.IsChecked = true;
                }
                else if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.RumbleContinuous)
                {
                    CBRumbleContinuous.IsEnabled = true;
                    CBRumbleContinuous.IsChecked = true;
                }
            }

            if (hapticsController?.IsIntensitySupported == true)
            {
                CBIntensity.IsEnabled = true;
                CBIntensity.IsChecked = true;
            }
            if (hapticsController?.IsPlayCountSupported == true)
            {
                CBPlayCount.IsEnabled = true;
                CBPlayCount.IsChecked = true;
            }
            if (hapticsController?.IsPlayDurationSupported == true)
            {
                CBPlayDuration.IsEnabled = true;
                CBPlayDuration.IsChecked = true;
            }
            if (hapticsController?.IsReplayPauseIntervalSupported == true)
            {
                CBReplayPauseInterval.IsEnabled = true;
                CBReplayPauseInterval.IsChecked = true;
            }
        }
        ```

    - Dans nos gestionnaires d’événements [RotationChanged](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.RotationChanged) et [ButtonClicked](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ButtonClicked) , nous connectons les contrôles Slider et bouton bascule correspondants à nos haptique personnalisées. 

        ```csharp
        // Connect wheel device rotation to slider control.
        private void RadialController_RotationChanged(
            object sender, RadialControllerRotationChangedEventArgs args)
        {
            ...
            if (ClickToggle.IsOn && 
                (RotationSlider.Value > RotationSlider.Minimum) && 
                (RotationSlider.Value < RotationSlider.Maximum))
            {
                SimpleHapticsControllerFeedback waveform = 
                    FindWaveform(args.SimpleHapticsController, 
                    KnownSimpleHapticsControllerWaveforms.BuzzContinuous);
                if (waveform != null)
                {
                    args.SimpleHapticsController.SendHapticFeedback(waveform);
                }
            }
        }

        private void RadialController_ButtonClicked(
            object sender, RadialControllerButtonClickedEventArgs args)
        {
            ...

            if (RotationSlider?.Value > 0)
            {
                SimpleHapticsControllerFeedback waveform = 
                    FindWaveform(args.SimpleHapticsController, 
                    KnownSimpleHapticsControllerWaveforms.Click);

                if (waveform != null)
                {
                    args.SimpleHapticsController.SendHapticFeedbackForPlayCount(
                        waveform, 1.0, 
                        (int)RotationSlider.Value, 
                        TimeSpan.Parse("1"));
                }
            }
        }
        ```
    - Enfin, nous obtenons la **[forme d’onde](https://docs.microsoft.com/uwp/api/windows.devices.haptics.simplehapticscontrollerfeedback.Waveform)** demandée (si prise en charge) pour les commentaires haptique. 

        ```csharp
        // Get the requested waveform.
        private SimpleHapticsControllerFeedback FindWaveform(
            SimpleHapticsController hapticsController,
            ushort waveform)
        {
            foreach (var hapticInfo in hapticsController.SupportedFeedback)
            {
                if (hapticInfo.Waveform == waveform)
                {
                    return hapticInfo;
                }
            }
            return null;
        }
        ```

À présent, exécutez à nouveau l’application pour tester les haptique personnalisées en modifiant la valeur du curseur et l’État Toggle-Switch.

## <a name="step-7-define-on-screen-interactions-for-surface-studio-and-similar-devices"></a>Étape 7 : définir des interactions à l’écran pour les appareils surface Studio et similaires
Couplée à surface Studio, la numérotation en surface peut fournir une expérience utilisateur encore plus distinctive. 

Outre l’expérience de menu d’appui prolongé par défaut décrite, Surface Dial peut également être placé directement sur l’écran de Surface Studio. Cela permet d’afficher un menu « à l’écran » spécial. 

En détectant à la fois l’emplacement du contact et les limites du cadran de la surface, le système gère l’occlusion par l’appareil et affiche une version plus grande du menu qui habille l’extérieur de la composition. Ces mêmes informations peuvent également être utilisées par votre application pour adapter l’interface utilisateur à la présence de l’appareil et à son utilisation prévue, notamment au placement de la main et du bras de l’utilisateur. 

L’exemple qui accompagne ce didacticiel comprend un exemple légèrement plus complexe qui illustre certaines de ces fonctionnalités.

Pour voir cela en action (vous aurez besoin d’une surface Studio) :

1. Télécharger l’exemple sur un appareil surface Studio (avec Visual Studio installé)
2. Ouvrir l’exemple dans Visual Studio
3. Ouvrir le fichier App.xaml.cs
4. Recherchez le code marqué avec le titre de cette étape (« étape 7 : définir des interactions à l’écran pour surface Studio et appareils similaires »)
5. Commentez la première et la deuxième ligne (« MainPage_Basic » et « MainPage_Haptics ») et supprimez les marques de commentaire du troisième (« MainPage »)  

    ``` csharp
    rootFrame.Navigate(typeof(MainPage_Basic), e.Arguments);
    rootFrame.Navigate(typeof(MainPage_Haptics), e.Arguments);
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
    ```
6. Exécutez l’application et placez le cadran dans chacune des deux régions de contrôle, en alternant entre elles.    
![RadialController à l’écran](images/radialcontroller/wheel-app-step5-onscreen2.png) 

    Voici une vidéo de cet exemple en action :  

    <iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Programming-the-Microsoft-Surface-Dial/player" width="600" height="400" allowFullScreen frameBorder="0"></iframe>  

## <a name="summary"></a>Résumé

Félicitations, vous avez terminé le *didacticiel de prise en main : prendre en charge la numérotation en surface (et les autres appareils volants) dans votre application Windows*! Nous vous avons montré le code de base requis pour la prise en charge d’un appareil roue dans vos applications Windows, et comment fournir certaines des expériences utilisateur plus riches prises en charge par les API **RadialController** .

## <a name="related-articles"></a>Articles connexes

[Interactions avec Surface Dial](windows-wheel-interactions.md)

### <a name="api-reference"></a>Informations de référence sur les API

- [**RadialController** , classe](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialController)
- [**RadialControllerButtonClickedEventArgs** , classe](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerButtonClickedEventArgs)
- [**RadialControllerConfiguration** , classe](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerConfiguration) 
- [**RadialControllerControlAcquiredEventArgs** , classe](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerControlAcquiredEventArgs) 
- [**RadialControllerMenu** , classe](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerMenu) 
- [**RadialControllerMenuItem** , classe](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerMenuItem) 
- [**RadialControllerRotationChangedEventArgs** , classe](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerRotationChangedEventArgs) 
- [**RadialControllerScreenContact** , classe](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerScreenContact) 
- [**RadialControllerScreenContactContinuedEventArgs** , classe](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerScreenContactContinuedEventArgs) 
- [**RadialControllerScreenContactStartedEventArgs** , classe](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerScreenContactStartedEventArgs)
- [Énumération **RadialControllerMenuKnownIcon**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerMenuKnownIcon) 
- [Énumération **RadialControllerSystemMenuItemKind**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerSystemMenuItemKind) 

### <a name="samples"></a>Exemples

#### <a name="topic-samples"></a>Exemples de la rubrique

[Personnalisation de RadialController](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-radialcontroller-customization.zip)

#### <a name="other-samples"></a>Autres exemples
[Exemple de livre de coloration](https://github.com/Microsoft/Windows-appsample-coloringbook)

[Exemples de la plateforme Windows universelle (C# et C++)](https://github.com/Microsoft/Windows-universal-samples/tree/b78d95134ce2d57c848e0a8dc339fc362748fb9c/Samples/RadialController)

[Exemple du bureau classique Windows](https://github.com/Microsoft/Windows-classic-samples/tree/master/Samples/RadialController)
