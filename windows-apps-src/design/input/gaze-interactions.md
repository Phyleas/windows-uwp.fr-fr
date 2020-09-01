---
title: Interactions avec pointage du regard
Description: Découvrez comment concevoir et optimiser vos applications Windows pour offrir la meilleure expérience possible aux utilisateurs qui reposent sur une entrée de regard de l’œil et des têtes de trace.
label: Gaze interactions
template: detail.hbs
keywords: regard, suivi oculaire, suivi des têtes, point de regard, entrée, interaction utilisateur, accessibilité, convivialité
ms.date: 05/01/2018
ms.topic: article
pm-contact: Jake Cohen
dev-contact: Austin Hodges
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: c91de7eb0200780b04bad1853cb49caf41a22bc0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172503"
---
# <a name="gaze-interactions-and-eye-tracking-in-windows-apps"></a>Interactions de regard et suivi oculaire dans les applications Windows

![Héros du suivi oculaire](images/gaze/eyecontrolbanner1.png)

Fournir un support pour le suivi du regard, de l’attention et de la présence d’un utilisateur en fonction de l’emplacement et du mouvement de leurs yeux.

> [!NOTE]
> Pour les entrées de regard dans [Windows Mixed Reality](/windows/mixed-reality/), consultez le point de [regard](/windows/mixed-reality/gaze).

**API importantes**: [Windows. Devices. Input. Preview](/uwp/api/windows.devices.input.preview), [GazeDevicePreview](/uwp/api/windows.devices.input.preview.gazedevicepreview), [GazePointPreview](/uwp/api/windows.devices.input.preview.gazepointpreview), [GazeInputSourcePreview](/uwp/api/windows.devices.input.preview.gazeinputsourcepreview)

## <a name="overview"></a>Vue d’ensemble

Le point d’entrée en regard est un moyen puissant d’interagir et d’utiliser des applications Windows, particulièrement utiles comme une technologie d’assistance pour les utilisateurs avec des maladies neuro-musculaires (telles que ALS) et d’autres handicaps impliquant des fonctions musculaires ou nerveuses altérées.

En outre, le point d’entrée de regard offre des opportunités aussi attrayantes pour les jeux (y compris l’acquisition et le suivi cibles) et les applications de productivité traditionnelles, les bornes et d’autres scénarios interactifs dans lesquels les périphériques d’entrée traditionnels (clavier, souris, Touch) ne sont pas disponibles

> [!NOTE]
> La prise en charge du matériel de suivi oculaire a été introduite dans **Windows 10 automne Creators Update** avec le [contrôle oculaire](https://support.microsoft.com/help/4043921/windows-10-get-started-eye-control), une fonctionnalité intégrée qui vous permet d’utiliser vos yeux pour contrôler le pointeur sur l’écran, taper avec le clavier visuel et communiquer avec des personnes utilisant la conversion de texte par synthèse vocale. Un ensemble d’API Windows Runtime ([Windows. Devices. Input. Preview](/uwp/api/windows.devices.input.preview)) pour la création d’applications pouvant interagir avec le matériel de suivi visuel est disponible avec la **mise à jour 2018 de Windows 10 avril (version 1803, Build 17134)** et les versions ultérieures.

## <a name="privacy"></a>Confidentialité

En raison des données personnelles potentiellement sensibles collectées par les appareils de suivi oculaire, vous devez déclarer la `gazeInput` fonctionnalité dans le manifeste de l’application de votre application (voir la section **d’installation** suivante). Quand elle est déclarée, Windows invite automatiquement les utilisateurs avec une boîte de dialogue de consentement (lorsque l’application est exécutée pour la première fois), où l’utilisateur doit accorder l’autorisation pour que l’application communique avec le périphérique de suivi oculaire et accède à ces données.

En outre, si votre application collecte, stocke ou transfère des données de suivi oculaire, vous devez la décrire dans la déclaration de confidentialité de votre application et respecter toutes les autres exigences relatives aux **informations personnelles** dans le [contrat de développement d’application](/legal/windows/agreements/app-developer-agreement) et les stratégies de [Microsoft Store](/legal/windows/agreements/store-policies).

## <a name="setup"></a>Programme d’installation

Pour utiliser les API de saisie en regard de votre application Windows, vous devez : 

- Spécifiez la `gazeInput` fonctionnalité dans le manifeste de l’application.

    Ouvrez le fichier **Package. appxmanifest** avec le concepteur de manifeste Visual Studio, ou ajoutez la fonctionnalité manuellement en sélectionnant **afficher le code**, puis en insérant le code suivant `DeviceCapability` dans le `Capabilities` nœud :

    ```xaml
    <Capabilities>
       <DeviceCapability Name="gazeInput" />
    </Capabilities>
    ```

- Un appareil de suivi oculaire compatible Windows connecté à votre système (intégré ou périphérique) et sous tension.

    Pour obtenir la liste des appareils de suivi oculaire pris en charge, consultez prise en [main du contrôle visuel dans Windows 10](https://support.microsoft.com/help/4043921/windows-10-get-started-eye-control#supported-devices ) .

## <a name="basic-eye-tracking"></a>Suivi des yeux de base

Dans cet exemple, nous montrons comment suivre le regard de l’utilisateur dans une application Windows et utiliser une fonction de minutage avec un test de positionnement de base pour indiquer la manière dont il peut maintenir son focus pointant sur un élément spécifique.

Une petite ellipse est utilisée pour indiquer où se trouve le point de regard dans la fenêtre d’affichage de l’application, tandis qu’un [RadialProgressBar](/windows/communitytoolkit/controls/radialprogressbar) de la boîte à outils de la [communauté Windows](/windows/communitytoolkit/) est placé de manière aléatoire sur le canevas. Quand le focus du regard est détecté sur la barre de progression, un minuteur est démarré et la barre de progression est déplacée de façon aléatoire sur le canevas lorsque la barre de progression atteint 100%.

![Suivi du regard avec l’exemple de minuterie](images/gaze/gaze-input-timed2.gif)

*Suivi du regard avec l’exemple de minuterie*

**Télécharger cet exemple à partir de l' [exemple de texte en regard (Basic)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-gazeinput-basic.zip)**

1. Tout d’abord, nous allons configurer l’interface utilisateur (MainPage. Xaml).

    ```xaml
    <Page
        x:Class="gazeinput.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:gazeinput"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:controls="using:Microsoft.Toolkit.Uwp.UI.Controls"    
        mc:Ignorable="d">
    
        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
            <Grid x:Name="containerGrid">
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="*"/>
                </Grid.RowDefinitions>
                <StackPanel x:Name="HeaderPanel" 
                        Orientation="Horizontal" 
                        Grid.Row="0">
                    <StackPanel.Transitions>
                        <TransitionCollection>
                            <AddDeleteThemeTransition/>
                        </TransitionCollection>
                    </StackPanel.Transitions>
                    <TextBlock x:Name="Header" 
                           Text="Gaze tracking sample" 
                           Style="{ThemeResource HeaderTextBlockStyle}" 
                           Margin="10,0,0,0" />
                    <TextBlock x:Name="TrackerCounterLabel"
                           VerticalAlignment="Center"                 
                           Style="{ThemeResource BodyTextBlockStyle}"
                           Text="Number of trackers: " 
                           Margin="50,0,0,0"/>
                    <TextBlock x:Name="TrackerCounter"
                           VerticalAlignment="Center"                 
                           Style="{ThemeResource BodyTextBlockStyle}"
                           Text="0" 
                           Margin="10,0,0,0"/>
                    <TextBlock x:Name="TrackerStateLabel"
                           VerticalAlignment="Center"                 
                           Style="{ThemeResource BodyTextBlockStyle}"
                           Text="State: " 
                           Margin="50,0,0,0"/>
                    <TextBlock x:Name="TrackerState"
                           VerticalAlignment="Center"                 
                           Style="{ThemeResource BodyTextBlockStyle}"
                           Text="n/a" 
                           Margin="10,0,0,0"/>
                </StackPanel>
                <Canvas x:Name="gazePositionCanvas" Grid.Row="1">
                    <controls:RadialProgressBar
                        x:Name="GazeRadialProgressBar" 
                        Value="0"
                        Foreground="Blue" 
                        Background="White"
                        Thickness="4"
                        Minimum="0"
                        Maximum="100"
                        Width="100"
                        Height="100"
                        Outline="Gray"
                        Visibility="Collapsed"/>
                    <Ellipse 
                        x:Name="eyeGazePositionEllipse"
                        Width="20" Height="20"
                        Fill="Blue" 
                        Opacity="0.5" 
                        Visibility="Collapsed">
                    </Ellipse>
                </Canvas>
            </Grid>
        </Grid>
    </Page>
    ```

2. Nous allons ensuite initialiser notre application.

    Dans cet extrait de code, nous déclarons nos objets globaux et remplaçons l’événement de la page [OnNavigatedTo](/uwp/api/windows.ui.xaml.controls.page.onnavigatedto) pour démarrer notre [Observateur d’appareils de regard](/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview) et l’événement de page [OnNavigatedFrom](/uwp/api/windows.ui.xaml.controls.page.onnavigatedfrom) pour arrêter notre Observateur de [périphériques en regard](/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview).

    ```csharp
    using System;
    using Windows.Devices.Input.Preview;
    using Windows.UI.Xaml.Controls;
    using Windows.UI.Xaml;
    using Windows.Foundation;
    using System.Collections.Generic;
    using Windows.UI.Xaml.Media;
    using Windows.UI.Xaml.Navigation;
    
    namespace gazeinput
    {
        public sealed partial class MainPage : Page
        {
            /// <summary>
            /// Reference to the user's eyes and head as detected
            /// by the eye-tracking device.
            /// </summary>
            private GazeInputSourcePreview gazeInputSource;
    
            /// <summary>
            /// Dynamic store of eye-tracking devices.
            /// </summary>
            /// <remarks>
            /// Receives event notifications when a device is added, removed, 
            /// or updated after the initial enumeration.
            /// </remarks>
            private GazeDeviceWatcherPreview gazeDeviceWatcher;
    
            /// <summary>
            /// Eye-tracking device counter.
            /// </summary>
            private int deviceCounter = 0;
    
            /// <summary>
            /// Timer for gaze focus on RadialProgressBar.
            /// </summary>
            DispatcherTimer timerGaze = new DispatcherTimer();
    
            /// <summary>
            /// Tracker used to prevent gaze timer restarts.
            /// </summary>
            bool timerStarted = false;
    
            /// <summary>
            /// Initialize the app.
            /// </summary>
            public MainPage()
            {
                InitializeComponent();
            }

            /// <summary>
            /// Override of OnNavigatedTo page event starts GazeDeviceWatcher.
            /// </summary>
            /// <param name="e">Event args for the NavigatedTo event</param>
            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
                // Start listening for device events on navigation to eye-tracking page.
                StartGazeDeviceWatcher();
            }
    
            /// <summary>
            /// Override of OnNavigatedFrom page event stops GazeDeviceWatcher.
            /// </summary>
            /// <param name="e">Event args for the NavigatedFrom event</param>
            protected override void OnNavigatedFrom(NavigationEventArgs e)
            {
                // Stop listening for device events on navigation from eye-tracking page.
                StopGazeDeviceWatcher();
            }
        }
    }
    ```

3. Ensuite, nous ajoutons les méthodes de l’observateur de l’appareil. 
    
    Dans `StartGazeDeviceWatcher` , nous appelons [CreateWatcher](/uwp/api/windows.devices.input.preview.gazeinputsourcepreview.createwatcher) et déclarons les écouteurs d’événements d’appareil de suivi oculaire ([DeviceAdded](/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview.added), [DeviceUpdated](/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview.updated)et [DeviceRemoved](/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview.removed)).

    Dans `DeviceAdded` , nous vérifions l’état de l’appareil de suivi oculaire. Si vous êtes un appareil viable, nous incrémentons notre nombre d’appareils et activons le suivi du regard. Pour plus d’informations, consultez l’étape suivante.

    Dans `DeviceUpdated` , nous activons également le suivi du regard, car cet événement est déclenché si un appareil est recalibré.

    Dans `DeviceRemoved` , nous décrémentant notre compteur d’appareils et supprimons les gestionnaires d’événements de l’appareil.

    Dans `StopGazeDeviceWatcher` , nous arrêtons l’observateur de l’appareil de regard. 

```csharp
    /// <summary>
    /// Start gaze watcher and declare watcher event handlers.
    /// </summary>
    private void StartGazeDeviceWatcher()
    {
        if (gazeDeviceWatcher == null)
        {
            gazeDeviceWatcher = GazeInputSourcePreview.CreateWatcher();
            gazeDeviceWatcher.Added += this.DeviceAdded;
            gazeDeviceWatcher.Updated += this.DeviceUpdated;
            gazeDeviceWatcher.Removed += this.DeviceRemoved;
            gazeDeviceWatcher.Start();
        }
    }

    /// <summary>
    /// Shut down gaze watcher and stop listening for events.
    /// </summary>
    private void StopGazeDeviceWatcher()
    {
        if (gazeDeviceWatcher != null)
        {
            gazeDeviceWatcher.Stop();
            gazeDeviceWatcher.Added -= this.DeviceAdded;
            gazeDeviceWatcher.Updated -= this.DeviceUpdated;
            gazeDeviceWatcher.Removed -= this.DeviceRemoved;
            gazeDeviceWatcher = null;
        }
    }

    /// <summary>
    /// Eye-tracking device connected (added, or available when watcher is initialized).
    /// </summary>
    /// <param name="sender">Source of the device added event</param>
    /// <param name="e">Event args for the device added event</param>
    private void DeviceAdded(GazeDeviceWatcherPreview source, 
        GazeDeviceWatcherAddedPreviewEventArgs args)
    {
        if (IsSupportedDevice(args.Device))
        {
            deviceCounter++;
            TrackerCounter.Text = deviceCounter.ToString();
        }
        // Set up gaze tracking.
        TryEnableGazeTrackingAsync(args.Device);
    }

    /// <summary>
    /// Initial device state might be uncalibrated, 
    /// but device was subsequently calibrated.
    /// </summary>
    /// <param name="sender">Source of the device updated event</param>
    /// <param name="e">Event args for the device updated event</param>
    private void DeviceUpdated(GazeDeviceWatcherPreview source,
        GazeDeviceWatcherUpdatedPreviewEventArgs args)
    {
        // Set up gaze tracking.
        TryEnableGazeTrackingAsync(args.Device);
    }

    /// <summary>
    /// Handles disconnection of eye-tracking devices.
    /// </summary>
    /// <param name="sender">Source of the device removed event</param>
    /// <param name="e">Event args for the device removed event</param>
    private void DeviceRemoved(GazeDeviceWatcherPreview source,
        GazeDeviceWatcherRemovedPreviewEventArgs args)
    {
        // Decrement gaze device counter and remove event handlers.
        if (IsSupportedDevice(args.Device))
        {
            deviceCounter--;
            TrackerCounter.Text = deviceCounter.ToString();

            if (deviceCounter == 0)
            {
                gazeInputSource.GazeEntered -= this.GazeEntered;
                gazeInputSource.GazeMoved -= this.GazeMoved;
                gazeInputSource.GazeExited -= this.GazeExited;
            }
        }
    }
```

4. Ici, nous vérifions si l’appareil est viable dans `IsSupportedDevice` et, le cas échéant, vous tentez d’activer le suivi du regard dans `TryEnableGazeTrackingAsync` .

    Dans `TryEnableGazeTrackingAsync` , nous déclarons les gestionnaires d’événements de pointage et nous appelons [GazeInputSourcePreview. GetForCurrentView ()](/uwp/api/windows.devices.input.preview.gazeinputsourcepreview.getforcurrentview) pour obtenir une référence à la source d’entrée (ce qui doit être appelé sur le thread d’interface utilisateur, consultez [conserver le thread d’interface utilisateur réactif](../../debug-test-perf/keep-the-ui-thread-responsive.md)).

    > [!NOTE]
    > Vous devez appeler [GazeInputSourcePreview. GetForCurrentView ()](/uwp/api/windows.devices.input.preview.gazeinputsourcepreview.getforcurrentview) uniquement quand un appareil compatible de suivi oculaire est connecté et requis par votre application. Dans le cas contraire, la boîte de dialogue de consentement n’est pas nécessaire.

```csharp
    /// <summary>
    /// Initialize gaze tracking.
    /// </summary>
    /// <param name="gazeDevice"></param>
    private async void TryEnableGazeTrackingAsync(GazeDevicePreview gazeDevice)
    {
        // If eye-tracking device is ready, declare event handlers and start tracking.
        if (IsSupportedDevice(gazeDevice))
        {
            timerGaze.Interval = new TimeSpan(0, 0, 0, 0, 20);
            timerGaze.Tick += TimerGaze_Tick;

            SetGazeTargetLocation();

            // This must be called from the UI thread.
            gazeInputSource = GazeInputSourcePreview.GetForCurrentView();

            gazeInputSource.GazeEntered += GazeEntered;
            gazeInputSource.GazeMoved += GazeMoved;
            gazeInputSource.GazeExited += GazeExited;
        }
        // Notify if device calibration required.
        else if (gazeDevice.ConfigurationState ==
                    GazeDeviceConfigurationStatePreview.UserCalibrationNeeded ||
                    gazeDevice.ConfigurationState ==
                    GazeDeviceConfigurationStatePreview.ScreenSetupNeeded)
        {
            // Device isn't calibrated, so invoke the calibration handler.
            System.Diagnostics.Debug.WriteLine(
                "Your device needs to calibrate. Please wait for it to finish.");
            await gazeDevice.RequestCalibrationAsync();
        }
        // Notify if device calibration underway.
        else if (gazeDevice.ConfigurationState == 
            GazeDeviceConfigurationStatePreview.Configuring)
        {
            // Device is currently undergoing calibration.  
            // A device update is sent when calibration complete.
            System.Diagnostics.Debug.WriteLine(
                "Your device is being configured. Please wait for it to finish"); 
        }
        // Device is not viable.
        else if (gazeDevice.ConfigurationState == GazeDeviceConfigurationStatePreview.Unknown)
        {
            // Notify if device is in unknown state.  
            // Reconfigure/recalbirate the device.  
            System.Diagnostics.Debug.WriteLine(
                "Your device is not ready. Please set up your device or reconfigure it."); 
        }
    }

    /// <summary>
    /// Check if eye-tracking device is viable.
    /// </summary>
    /// <param name="gazeDevice">Reference to eye-tracking device.</param>
    /// <returns>True, if device is viable; otherwise, false.</returns>
    private bool IsSupportedDevice(GazeDevicePreview gazeDevice)
    {
        TrackerState.Text = gazeDevice.ConfigurationState.ToString();
        return (gazeDevice.CanTrackEyes &&
                    gazeDevice.ConfigurationState == 
                    GazeDeviceConfigurationStatePreview.Ready);
    }
```

5. Nous allons ensuite configurer nos gestionnaires d’événements de point de regard.

    Nous affichons et masquerons l’ellipse de suivi du regard dans `GazeEntered` et `GazeExited` , respectivement.

    Dans `GazeMoved` , nous allons déplacer notre ellipse de suivi du point d’interlettrage en fonction du [EyeGazePosition](/uwp/api/windows.devices.input.preview.gazepointpreview.eyegazeposition) fourni par le [CurrentPoint](/uwp/api/windows.devices.input.preview.gazeenteredprevieweventargs.currentpoint) du [GazeEnteredPreviewEventArgs](/uwp/api/windows.devices.input.preview.gazeenteredprevieweventargs). Nous allons également gérer le minuteur de focus du point de regard sur le [RadialProgressBar](/windows/communitytoolkit/controls/radialprogressbar), qui déclenche le repositionnement de la barre de progression. Pour plus d’informations, consultez l’étape suivante.

    ```csharp
    /// <summary>
    /// GazeEntered handler.
    /// </summary>
    /// <param name="sender">Source of the gaze entered event</param>
    /// <param name="e">Event args for the gaze entered event</param>
    private void GazeEntered(
        GazeInputSourcePreview sender, 
        GazeEnteredPreviewEventArgs args)
    {
        // Show ellipse representing gaze point.
        eyeGazePositionEllipse.Visibility = Visibility.Visible;

        // Mark the event handled.
        args.Handled = true;
    }

    /// <summary>
    /// GazeExited handler.
    /// Call DisplayRequest.RequestRelease to conclude the 
    /// RequestActive called in GazeEntered.
    /// </summary>
    /// <param name="sender">Source of the gaze exited event</param>
    /// <param name="e">Event args for the gaze exited event</param>
    private void GazeExited(
        GazeInputSourcePreview sender, 
        GazeExitedPreviewEventArgs args)
    {
        // Hide gaze tracking ellipse.
        eyeGazePositionEllipse.Visibility = Visibility.Collapsed;

        // Mark the event handled.
        args.Handled = true;
    }

    /// <summary>
    /// GazeMoved handler translates the ellipse on the canvas to reflect gaze point.
    /// </summary>
    /// <param name="sender">Source of the gaze moved event</param>
    /// <param name="e">Event args for the gaze moved event</param>
    private void GazeMoved(GazeInputSourcePreview sender, GazeMovedPreviewEventArgs args)
    {
        // Update the position of the ellipse corresponding to gaze point.
        if (args.CurrentPoint.EyeGazePosition != null)
        {
            double gazePointX = args.CurrentPoint.EyeGazePosition.Value.X;
            double gazePointY = args.CurrentPoint.EyeGazePosition.Value.Y;

            double ellipseLeft = 
                gazePointX - 
                (eyeGazePositionEllipse.Width / 2.0f);
            double ellipseTop = 
                gazePointY - 
                (eyeGazePositionEllipse.Height / 2.0f) - 
                (int)Header.ActualHeight;

            // Translate transform for moving gaze ellipse.
            TranslateTransform translateEllipse = new TranslateTransform
            {
                X = ellipseLeft,
                Y = ellipseTop
            };

            eyeGazePositionEllipse.RenderTransform = translateEllipse;

            // The gaze point screen location.
            Point gazePoint = new Point(gazePointX, gazePointY);

            // Basic hit test to determine if gaze point is on progress bar.
            bool hitRadialProgressBar = 
                DoesElementContainPoint(
                    gazePoint, 
                    GazeRadialProgressBar.Name, 
                    GazeRadialProgressBar); 

            // Use progress bar thickness for visual feedback.
            if (hitRadialProgressBar)
            {
                GazeRadialProgressBar.Thickness = 10;
            }
            else
            {
                GazeRadialProgressBar.Thickness = 4;
            }

            // Mark the event handled.
            args.Handled = true;
        }
    }
    ```
6. Enfin, Voici les méthodes utilisées pour gérer le temporisateur de focalisation pour cette application.

    `DoesElementContainPoint` vérifie si le pointeur en forme de regard se trouve sur la barre de progression. Si c’est le cas, il démarre le minuteur de pointage et incrémente la barre de progression sur chaque graduation du minuteur de point de regard.

    `SetGazeTargetLocation` définit l’emplacement initial de la barre de progression et, si la barre de progression se termine (selon le chronomètre de focus), déplace la barre de progression vers un emplacement aléatoire.

    ```csharp
    /// <summary>
    /// Return whether the gaze point is over the progress bar.
    /// </summary>
    /// <param name="gazePoint">The gaze point screen location</param>
    /// <param name="elementName">The progress bar name</param>
    /// <param name="uiElement">The progress bar UI element</param>
    /// <returns></returns>
    private bool DoesElementContainPoint(
        Point gazePoint, string elementName, UIElement uiElement)
    {
        // Use entire visual tree of progress bar.
        IEnumerable<UIElement> elementStack = 
            VisualTreeHelper.FindElementsInHostCoordinates(gazePoint, uiElement, true);
        foreach (UIElement item in elementStack)
        {
            //Cast to FrameworkElement and get element name.
            if (item is FrameworkElement feItem)
            {
                if (feItem.Name.Equals(elementName))
                {
                    if (!timerStarted)
                    {
                        // Start gaze timer if gaze over element.
                        timerGaze.Start();
                        timerStarted = true;
                    }
                    return true;
                }
            }
        }

        // Stop gaze timer and reset progress bar if gaze leaves element.
        timerGaze.Stop();
        GazeRadialProgressBar.Value = 0;
        timerStarted = false;
        return false;
    }

    /// <summary>
    /// Tick handler for gaze focus timer.
    /// </summary>
    /// <param name="sender">Source of the gaze entered event</param>
    /// <param name="e">Event args for the gaze entered event</param>
    private void TimerGaze_Tick(object sender, object e)
    {
        // Increment progress bar.
        GazeRadialProgressBar.Value += 1;

        // If progress bar reaches maximum value, reset and relocate.
        if (GazeRadialProgressBar.Value == 100)
        {
            SetGazeTargetLocation();
        }
    }

    /// <summary>
    /// Set/reset the screen location of the progress bar.
    /// </summary>
    private void SetGazeTargetLocation()
    {
        // Ensure the gaze timer restarts on new progress bar location.
        timerGaze.Stop();
        timerStarted = false;

        // Get the bounding rectangle of the app window.
        Rect appBounds = Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().VisibleBounds;

        // Translate transform for moving progress bar.
        TranslateTransform translateTarget = new TranslateTransform();

        // Calculate random location within gaze canvas.
            Random random = new Random();
            int randomX = 
                random.Next(
                    0, 
                    (int)appBounds.Width - (int)GazeRadialProgressBar.Width);
            int randomY = 
                random.Next(
                    0, 
                    (int)appBounds.Height - (int)GazeRadialProgressBar.Height - (int)Header.ActualHeight);

        translateTarget.X = randomX;
        translateTarget.Y = randomY;

        GazeRadialProgressBar.RenderTransform = translateTarget;

        // Show progress bar.
        GazeRadialProgressBar.Visibility = Visibility.Visible;
        GazeRadialProgressBar.Value = 0;
    }
    ```

## <a name="see-also"></a>Voir aussi

### <a name="resources"></a>Ressources

- [Bibliothèque de regards Windows Community Toolkit](/windows/communitytoolkit/gaze/gazeinteractionlibrary)

### <a name="topic-samples"></a>Exemples de la rubrique

- [Point de regard (Basic) (C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-gazeinput-basic.zip)