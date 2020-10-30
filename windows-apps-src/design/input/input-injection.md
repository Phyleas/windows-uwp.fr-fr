---
description: Simulez et automatisez les entrées à partir d’appareils tels que le clavier, la souris, le toucher, le stylet et le boîtier de votre application Windows.
title: Simuler une entrée utilisateur par une injection d’entrée
label: Input injection
template: detail.hbs
keywords: appareil, digitaliseur, entrée, interaction, injection
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0cd1a56ca46c3e9ea401794ff5b9964545ce0c5d
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030122"
---
# <a name="simulate-user-input-through-input-injection"></a>Simuler une entrée utilisateur par une injection d’entrée

Simuler et automatiser les entrées utilisateur à partir d’appareils tels que le clavier, la souris, la touche, le stylet et le boîtier de manette dans vos applications Windows.

> **API importantes** : [ **Windows. UI. Input. preview. injection**](/uwp/api/windows.ui.input.preview.injection)

## <a name="overview"></a>Vue d’ensemble

L’injection d’entrée permet à votre application Windows de simuler l’entrée à partir de divers périphériques d’entrée et de diriger cette entrée n’importe où, y compris en dehors de la zone cliente de votre application (même pour les applications qui s’exécutent avec des privilèges d’administrateur, telles que l’éditeur du registre).

L’injection d’entrée est utile pour les applications et les outils Windows qui doivent fournir des fonctionnalités qui incluent l’accessibilité, le test (ad hoc, automatisé) et l’accès à distance et les fonctionnalités de prise en charge.

## <a name="setup"></a>Programme d’installation

Pour utiliser les API d’injection d’entrée dans votre application Windows, vous devez ajouter le code suivant au manifeste de l’application :

1. Cliquez avec le bouton droit sur le fichier **Package. appxmanifest** et sélectionnez **afficher le code** .
1. Insérez le code suivant dans le `Package` nœud :
    - `xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"`
    - `IgnorableNamespaces="rescap"`
1. Insérez le code suivant dans le `Capabilities` nœud :
    - `<rescap:Capability Name="inputInjectionBrokered" />`

## <a name="duplicate-user-input"></a>Entrée utilisateur dupliquée

| ![Exemple d’injection de saisie tactile](images/injection/touch-input-injection.gif) | 
|:--:|
| *Exemple d’injection de saisie tactile* |

Dans cet exemple, nous montrons comment utiliser les API d’injection d’entrée ([Windows. UI. Input. preview. injection](/uwp/api/windows.ui.input.preview.injection)) pour écouter les événements d’entrée de souris dans une région d’une application et simuler les événements d’entrée tactile correspondants dans une autre région.

**Télécharger cet exemple à partir de l' [exemple d’injection d’entrée (souris à toucher)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-input-injection-mouse-to-touch.zip)**

1. Tout d’abord, nous allons configurer l’interface utilisateur (MainPage. Xaml).

    Nous avons deux zones de grille (une pour l’entrée de la souris et une pour les entrées tactiles injectées), chacune avec quatre boutons.
      > [!NOTE] 
      > Une valeur doit être assignée à l’arrière-plan de la grille ( `Transparent` dans le cas présent), sinon les événements de pointeur ne sont pas détectés.

    Quand des clics de souris sont détectés dans la zone d’entrée, un événement tactile correspondant est injecté dans la zone d’injection d’entrée. Les clics de bouton à partir de l’entrée Inject sont signalés dans la zone de titre.

    ```xaml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel Grid.Row="0"
                    Margin="10">
            <TextBlock Style="{ThemeResource TitleTextBlockStyle}" 
                       Name="titleText"
                       Text="Touch input injection"
                       HorizontalTextAlignment="Center" />
            <TextBlock Style="{ThemeResource BodyTextBlockStyle}"
                       Name="statusText"
                       HorizontalTextAlignment="Center" />
        </StackPanel>
        <Grid HorizontalAlignment="Center"
                        Grid.Row="1">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition/>
                <ColumnDefinition/>
            </Grid.ColumnDefinitions>
            <TextBlock Grid.Column="0" 
                       Grid.Row="0" 
                       Style="{ThemeResource CaptionTextBlockStyle}"
                       Text="User mouse input area"/>
            <!-- Background must be set to something, otherwise pointer events are not detected. -->
            <Grid Name="ContainerInput" 
                  Grid.Column="0" 
                  Grid.Row="1"
                  HorizontalAlignment="Stretch" 
                  Background="Transparent" 
                  BorderBrush="Green" 
                  BorderThickness="2" 
                  MinHeight="100" MinWidth="300" 
                  Margin="10">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Button Name="B1" 
                        Grid.Column="0" 
                        HorizontalAlignment="Center" 
                        Width="50" Height="50"
                        Content="B1" />
                <Button Name="B2" 
                        Grid.Column="1" 
                        HorizontalAlignment="Center" 
                        Width="50" Height="50"
                        Content="B2" />
                <Button Name="B3" 
                        Grid.Column="2" 
                        HorizontalAlignment="Center" 
                        Width="50" Height="50"
                        Content="B3" />
                <Button Name="B4" 
                        Grid.Column="3" 
                        HorizontalAlignment="Center" 
                        Width="50" Height="50"
                        Content="B4" />
            </Grid>
            <TextBlock Grid.Column="1" 
                       Grid.Row="0"                         
                       Style="{ThemeResource CaptionTextBlockStyle}"
                       Text="Injected touch input area"/>
            <Grid Name="ContainerInject"
                  Grid.Column="1"  
                  Grid.Row="1"
                  HorizontalAlignment="Stretch"
                  BorderBrush="Red" 
                  BorderThickness="2" 
                  MinHeight="100" MinWidth="300" 
                  Margin="10">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Button Name="B1i" Click="Button_Click_Injected"
                        Content="B1i"
                        Grid.Column="0" 
                        HorizontalAlignment="Center" 
                        Width="50" Height="50" />
                <Button Name="B2i" Click="Button_Click_Injected"
                        Content="B2i"
                        Grid.Column="1" 
                        HorizontalAlignment="Center" 
                        Width="50" Height="50" />
                <Button Name="B3i" Click="Button_Click_Injected"
                        Content="B3i"
                        Grid.Column="2" 
                        HorizontalAlignment="Center" 
                        Width="50" Height="50" />
                <Button Name="B4i" Click="Button_Click_Injected"
                        Content="B4i"
                        Grid.Column="3" 
                        HorizontalAlignment="Center" 
                        Width="50" Height="50" />
            </Grid>
        </Grid>
    </Grid>
    ```

2. Nous allons ensuite initialiser notre application.
    
    Dans cet extrait de code, nous déclarons nos objets globaux et déclarons les écouteurs pour les événements de pointeur ([AddHandler](/uwp/api/windows.ui.xaml.uielement.addhandler)) dans la zone d’entrée de la souris qui peuvent être marqués comme gérés dans les événements de clic sur le bouton.

    L’objet [InputInjector](/uwp/api/windows.ui.input.preview.injection.inputinjector) représente l’appareil d’entrée virtuel pour l’envoi des données d’entrée.

    Dans le `ContainerInput_PointerPressed` Gestionnaire, nous appelons la fonction d’injection tactile.

    Dans le `ContainerInput_PointerReleased` Gestionnaire, nous appelons UninitializeTouchInjection pour arrêter l’objet [InputInjector](/uwp/api/windows.ui.input.preview.injection.inputinjector) .

    ```csharp
    public sealed partial class MainPage : Page
    {
        /// <summary>
        /// The virtual input device.
        /// </summary>
        InputInjector _inputInjector;

        /// <summary>
        /// Initialize the app, set the window size, 
        /// and add pointer input handlers for the container.
        /// </summary>
        public MainPage()
        {
            this.InitializeComponent();

            ApplicationView.PreferredLaunchViewSize =
                new Size(600, 200);
            ApplicationView.PreferredLaunchWindowingMode =
                ApplicationViewWindowingMode.PreferredLaunchViewSize;

            // Button handles PointerPressed/PointerReleased in 
            // the Tapped routed event, but we need the container Grid 
            // to handle them also. Add a handler for both 
            // PointerPressedEvent and PointerReleasedEvent on the input Grid 
            // and set handledEventsToo to true.
            ContainerInput.AddHandler(PointerPressedEvent,
                new PointerEventHandler(ContainerInput_PointerPressed), true);
            ContainerInput.AddHandler(PointerReleasedEvent,
                new PointerEventHandler(ContainerInput_PointerReleased), true);
        }

        /// <summary>
        /// PointerReleased handler for all pointer conclusion events.
        /// PointerPressed and PointerReleased events do not always occur 
        /// in pairs, so your app should listen for and handle any event that 
        /// might conclude a pointer down (such as PointerExited, PointerCanceled, 
        /// and PointerCaptureLost).  
        /// </summary>
        /// <param name="sender">Source of the click event</param>
        /// <param name="e">Event args for the button click routed event</param>
        private void ContainerInput_PointerReleased(
            object sender, PointerRoutedEventArgs e)
        {
            // Prevent most handlers along the event route from handling event again.
            e.Handled = true;

            // Shut down the virtual input device.
            _inputInjector.UninitializeTouchInjection();
        }

        /// <summary>
        /// PointerPressed handler.
        /// PointerPressed and PointerReleased events do not always occur 
        /// in pairs. Your app should listen for and handle any event that 
        /// might conclude a pointer down (such as PointerExited, 
        /// PointerCanceled, and PointerCaptureLost).  
        /// </summary>
        /// <param name="sender">Source of the click event</param>
        /// <param name="e">Event args for the button click routed event</param>
        private void ContainerInput_PointerPressed(
            object sender, PointerRoutedEventArgs e)
        {
            // Prevent most handlers along the event route from 
            // handling the same event again.
            e.Handled = true;

            InjectTouchForMouse(e.GetCurrentPoint(ContainerInput));

        }
        ...
    }
    ```
3. Voici la fonction d’injection d’entrée tactile.

    Tout d’abord, nous appelons [TryCreate](/uwp/api/windows.ui.input.preview.injection.inputinjector.trycreate) pour instancier l’objet [InputInjector](/uwp/api/windows.ui.input.preview.injection.inputinjector) .

    Ensuite, nous appelons [InitializeTouchInjection](/uwp/api/windows.ui.input.preview.injection.inputinjector.initializetouchinjection) avec un [InjectedInputVisualizationMode](/uwp/api/windows.ui.input.preview.injection.injectedinputvisualizationmode) de `Default` .

    Après avoir calculé le point d’injection, nous appelons [InjectedInputTouchInfo](/uwp/api/windows.ui.input.preview.injection.injectedinputtouchinfo) pour initialiser la liste de points tactiles à injecter (pour cet exemple, nous créons un point tactile correspondant au pointeur d’entrée de la souris).

    Enfin, nous appelons [InjectTouchInput](/uwp/api/windows.ui.input.preview.injection.inputinjector.injecttouchinput) deux fois, le premier pour un pointeur vers le haut et le second pour un pointeur vers le haut.

    ```csharp
    /// <summary>
    /// Inject touch input on injection target corresponding 
    /// to mouse click on input target.
    /// </summary>
    /// <param name="pointerPoint">The mouse click pointer.</param>
    private void InjectTouchForMouse(PointerPoint pointerPoint)
    {
        // Create the touch injection object.
        _inputInjector = InputInjector.TryCreate();

        if (_inputInjector != null)
        {
            _inputInjector.InitializeTouchInjection(
                InjectedInputVisualizationMode.Default);

            // Create a unique pointer ID for the injected touch pointer.
            // Multiple input pointers would require more robust handling.
            uint pointerId = pointerPoint.PointerId + 1;

            // Get the bounding rectangle of the app window.
            Rect appBounds =
                Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().VisibleBounds;

            // Get the top left screen coordinates of the app window rect.
            Point appBoundsTopLeft = new Point(appBounds.Left, appBounds.Top);

            // Get a reference to the input injection area.
            GeneralTransform injectArea =
                ContainerInject.TransformToVisual(Window.Current.Content);

            // Get the top left screen coordinates of the input injection area.
            Point injectAreaTopLeft = injectArea.TransformPoint(new Point(0, 0));

            // Get the screen coordinates (relative to the input area) 
            // of the input pointer.
            int pointerPointX = (int)pointerPoint.Position.X;
            int pointerPointY = (int)pointerPoint.Position.Y;

            // Create the point for input injection and calculate its screen location.
            Point injectionPoint =
                new Point(
                    appBoundsTopLeft.X + injectAreaTopLeft.X + pointerPointX,
                    appBoundsTopLeft.Y + injectAreaTopLeft.Y + pointerPointY);

            // Create a touch data point for pointer down.
            // Each element in the touch data list represents a single touch contact. 
            // For this example, we're mirroring a single mouse pointer.
            List<InjectedInputTouchInfo> touchData =
                new List<InjectedInputTouchInfo>
                {
                    new InjectedInputTouchInfo
                    {
                        Contact = new InjectedInputRectangle
                        {
                            Left = 30, Top = 30, Bottom = 30, Right = 30
                        },
                        PointerInfo = new InjectedInputPointerInfo
                        {
                            PointerId = pointerId,
                            PointerOptions =
                            InjectedInputPointerOptions.PointerDown |
                            InjectedInputPointerOptions.InContact |
                            InjectedInputPointerOptions.New,
                            TimeOffsetInMilliseconds = 0,
                            PixelLocation = new InjectedInputPoint
                            {
                                PositionX = (int)injectionPoint.X ,
                                PositionY = (int)injectionPoint.Y
                            }
                    },
                    Pressure = 1.0,
                    TouchParameters =
                        InjectedInputTouchParameters.Pressure |
                        InjectedInputTouchParameters.Contact
                }
            };

            // Inject the touch input. 
            _inputInjector.InjectTouchInput(touchData);

            // Create a touch data point for pointer up.
            touchData = new List<InjectedInputTouchInfo>
            {
                new InjectedInputTouchInfo
                {
                    PointerInfo = new InjectedInputPointerInfo
                    {
                        PointerId = pointerId,
                        PointerOptions = InjectedInputPointerOptions.PointerUp
                    }
                }
            };

            // Inject the touch input. 
            _inputInjector.InjectTouchInput(touchData);
        }
    }
    ```

4. Enfin, nous prenons en charge tous les événements routés de [clic](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase) de bouton dans la zone d’injection d’entrée et mettez à jour l’interface utilisateur avec le nom du bouton sur lequel vous avez cliqué.

## <a name="see-also"></a>Voir aussi

### <a name="topic-samples"></a>Exemples de la rubrique

- [Exemple d’injection d’entrée (souris à toucher)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-input-injection-mouse-to-touch.zip)
