---
title: Configurer une limite géographique
description: Configurez une Geofence dans votre application et découvrez comment gérer les notifications au premier plan et en arrière-plan.
ms.assetid: A3A46E03-0751-4DBD-A2A1-2323DB09BDBA
ms.date: 10/20/2020
ms.topic: article
keywords: Windows 10, UWP, carte, emplacement, limite géographique, notifications
ms.localizationpriority: medium
ms.openlocfilehash: ca6dad1a96f37e3a308ad10c84293a8d49fb0329
ms.sourcegitcommit: 7aaf0740a5d3a17ebf9214aa5e5d056924317673
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92297589"
---
# <a name="set-up-a-geofence"></a>Configurer une limite géographique

> [!NOTE]
> [**Collection MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) et les services cartographiques ont une clé d’authentification Maps appelée [**MapServiceToken**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapservicetoken). Pour plus d’informations sur l’obtention et la définition d’une clé d’authentification de cartes, voir [Demander une clé d’authentification de cartes](authentication-key.md).

Configurez une [**limite géographique**](/uwp/api/Windows.Devices.Geolocation.Geofencing.Geofence) dans votre application et apprenez à gérer les notifications au premier plan et en arrière-plan.

**Conseil** Pour en savoir plus sur l’accès à l’emplacement dans votre application, téléchargez l’exemple suivant à partir du [référentiel Windows-universal-samples](https://github.com/Microsoft/Windows-universal-samples) sur GitHub.

-   [Exemple de carte pour UWP (plateforme Windows universelle)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)

## <a name="enable-the-location-capability"></a>Activer la fonctionnalité de localisation


1.  Dans l’**Explorateur de solutions**, double-cliquez sur **package.appxmanifest**, puis sélectionnez l’onglet **Capacités**.
2.  Dans la liste **Capacités**, cochez **Localisation**. Cette opération ajoute la fonctionnalité `Location` de l’appareil au fichier manifeste du package.

```xml
  <Capabilities>
    <!-- DeviceCapability elements must follow Capability elements (if present) -->
    <DeviceCapability Name="location"/>
  </Capabilities>
```

## <a name="set-up-a-geofence"></a>Configurer une limite géographique


### <a name="step-1-request-access-to-the-users-location"></a>Étape 1 : Demander l’accès à l’emplacement de l’utilisateur

**Important** Vous devez demander l’accès à l’emplacement de l’utilisateur à l’aide de la méthode [**RequestAccessAsync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) avant d’essayer d’y accéder. Vous devez appeler la méthode **RequestAccessAsync** à partir du thread de l’interface utilisateur et votre application doit être au premier plan. Votre application sera en mesure d’accéder aux informations d’emplacement de l’utilisateur une fois que l’utilisateur aura accordé l’autorisation à votre application.

```csharp
using Windows.Devices.Geolocation;
...
var accessStatus = await Geolocator.RequestAccessAsync();
```

La méthode [**RequestAccessAsync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) demande à l’utilisateur l’autorisation d’accéder à son emplacement. L’utilisateur est invité une fois seulement (par application). Une fois la première autorisation accordée ou refusée, cette méthode ne demande plus d’autorisation. Pour aider l’utilisateur à modifier les autorisations d’emplacement une fois qu’il a été invité, nous vous recommandons de fournir un lien vers les paramètres d’emplacement, comme illustré plus loin dans cette rubrique.

### <a name="step-2-register-for-changes-in-geofence-state-and-location-permissions"></a>Étape 2 : Inscrire les modifications d’autorisations d’emplacement et d’état de limite géographique

Dans cet exemple, une instruction **switch** est utilisée avec l’élément **accessStatus** (de l’exemple précédent) afin d’agir uniquement lorsque l’accès à l’emplacement de l’utilisateur est autorisé. Si l’accès à l’emplacement de l’utilisateur est autorisé, le code accède aux limites géographiques actuelles et s’inscrit aux modifications de l’état des limites géographiques, ainsi qu’aux modifications des autorisations d’emplacement.

**Conseil** Lorsque vous utilisez une limite géographique, surveillez les modifications des autorisations d’emplacement avec l’événement [**StatusChanged**](/uwp/api/windows.devices.geolocation.geofencing.geofencemonitor.statuschanged) de la classe GeofenceMonitor plutôt qu’avec l’événement StatusChanged de la classe Geolocator. Un [**GeofenceMonitorStatus**](/uwp/api/Windows.Devices.Geolocation.Geofencing.GeofenceMonitorStatus) de **Disabled** équivaut à un [**PositionStatus**](/uwp/api/Windows.Devices.Geolocation.PositionStatus) désactivé : les deux indiquent que l’application n’est pas autorisée à accéder à l’emplacement de l’utilisateur.

```csharp
switch (accessStatus)
{
    case GeolocationAccessStatus.Allowed:
        geofences = GeofenceMonitor.Current.Geofences;

        FillRegisteredGeofenceListBoxWithExistingGeofences();
        FillEventListBoxWithExistingEvents();

        // Register for state change events.
        GeofenceMonitor.Current.GeofenceStateChanged += OnGeofenceStateChanged;
        GeofenceMonitor.Current.StatusChanged += OnGeofenceStatusChanged;
        break;


    case GeolocationAccessStatus.Denied:
        _rootPage.NotifyUser("Access denied.", NotifyType.ErrorMessage);
        break;

    case GeolocationAccessStatus.Unspecified:
        _rootPage.NotifyUser("Unspecified error.", NotifyType.ErrorMessage);
        break;
}
```

Ensuite, lorsque vous quittez l’application de premier plan, désinscrivez les écouteurs d’événements.

```csharp
protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
{
    GeofenceMonitor.Current.GeofenceStateChanged -= OnGeofenceStateChanged;
    GeofenceMonitor.Current.StatusChanged -= OnGeofenceStatusChanged;

    base.OnNavigatingFrom(e);
}
```

### <a name="step-3-create-the-geofence"></a>Étape 3 : créer la limite géographique

Vous voici prêt à définir et configurer un objet [**Geofence**](/uwp/api/Windows.Devices.Geolocation.Geofencing.Geofence). Vous pouvez choisir parmi différentes surcharges de constructeur, selon vos besoins. Dans le constructeur de clôture virtuelle de base, spécifiez uniquement les éléments [**Id**](/uwp/api/windows.devices.geolocation.geofencing.geofence.id) et [**Geoshape**](/uwp/api/windows.devices.geolocation.geofencing.geofence.geoshape), comme illustré ici.

```csharp
// Set the fence ID.
string fenceId = "fence1";

// Define the fence location and radius.
BasicGeoposition position;
position.Latitude = 47.6510;
position.Longitude = -122.3473;
position.Altitude = 0.0;
double radius = 10; // in meters

// Set a circular region for the geofence.
Geocircle geocircle = new Geocircle(position, radius);

// Create the geofence.
Geofence geofence = new Geofence(fenceId, geocircle);

```

Vous pouvez ajuster davantage votre clôture virtuelle en utilisant l’un des autres constructeurs. Dans l’exemple suivant, le constructeur de limite géographique indique les paramètres supplémentaires ci-après :

-   [**MonitoredStates**](/uwp/api/windows.devices.geolocation.geofencing.geofence.monitoredstates) : indique les événements de la limite géographique pour lesquels vous souhaitez recevoir des notifications pour la saisie de la région définie, en laissant la région définie ou en supprimant la clôture.
-   [**SingleUse**](/uwp/api/windows.devices.geolocation.geofencing.geofence.singleuse) -supprime la clôture géographique une fois que tous les États dont la clôture géographique est surveillée sont satisfaits.
-   [**DwellTime**](/uwp/api/windows.devices.geolocation.geofencing.geofence.dwelltime) : indique la durée pendant laquelle l’utilisateur doit être à l’extérieur ou à l’extérieur de la zone définie avant le déclenchement des événements d’entrée/sortie.
-   [**StartTime**](/uwp/api/windows.devices.geolocation.geofencing.geofence.starttime) : indique quand démarrer l’analyse de la clôture géographique.
-   [**Duration**](/uwp/api/windows.devices.geolocation.geofencing.geofence.duration) : indique la période pour laquelle surveiller la clôture géographique.

```csharp
// Set the fence ID.
string fenceId = "fence2";

// Define the fence location and radius.
BasicGeoposition position;
position.Latitude = 47.6510;
position.Longitude = -122.3473;
position.Altitude = 0.0;
double radius = 10; // in meters

// Set the circular region for geofence.
Geocircle geocircle = new Geocircle(position, radius);

// Remove the geofence after the first trigger.
bool singleUse = true;

// Set the monitored states.
MonitoredGeofenceStates monitoredStates =
                MonitoredGeofenceStates.Entered |
                MonitoredGeofenceStates.Exited |
                MonitoredGeofenceStates.Removed;

// Set how long you need to be in geofence for the enter event to fire.
TimeSpan dwellTime = TimeSpan.FromMinutes(5);

// Set how long the geofence should be active.
TimeSpan duration = TimeSpan.FromDays(1);

// Set up the start time of the geofence.
DateTimeOffset startTime = DateTime.Now;

// Create the geofence.
Geofence geofence = new Geofence(fenceId, geocircle, monitoredStates, singleUse, dwellTime, startTime, duration);
```

Après la création, n’oubliez pas d’inscrire votre nouvelle [**limite géographique**](/uwp/api/Windows.Devices.Geolocation.Geofencing.Geofence) auprès du moniteur.

```csharp
// Register the geofence
try {
   GeofenceMonitor.Current.Geofences.Add(geofence);
} catch {
   // Handle failure to add geofence
}
```

### <a name="step-4-handle-changes-in-location-permissions"></a>Étape 4 : gérer les modifications apportées aux autorisations d’emplacement

L’objet [**GeofenceMonitor**](/uwp/api/Windows.Devices.Geolocation.Geofencing.GeofenceMonitor) déclenche l’événement [**StatusChanged**](/uwp/api/windows.devices.geolocation.geofencing.geofencemonitor.statuschanged) afin d’indiquer que les paramètres d’emplacement de l’utilisateur ont changé. Cet événement transmet l’état correspondant par le biais de la propriété **sender.Status** de l’argument (de type [**GeofenceMonitorStatus**](/uwp/api/Windows.Devices.Geolocation.Geofencing.GeofenceMonitorStatus)). Notez que cette méthode n’est pas appelée à partir du thread d’interface utilisateur et que l’objet [**Dispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) invoque les modifications de l’interface utilisateur.

```csharp
using Windows.UI.Core;
...
public async void OnGeofenceStatusChanged(GeofenceMonitor sender, object e)
{
   await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
   {
    // Show the location setting message only if the status is disabled.
    LocationDisabledMessage.Visibility = Visibility.Collapsed;

    switch (sender.Status)
    {
     case GeofenceMonitorStatus.Ready:
      _rootPage.NotifyUser("The monitor is ready and active.", NotifyType.StatusMessage);
      break;

     case GeofenceMonitorStatus.Initializing:
      _rootPage.NotifyUser("The monitor is in the process of initializing.", NotifyType.StatusMessage);
      break;

     case GeofenceMonitorStatus.NoData:
      _rootPage.NotifyUser("There is no data on the status of the monitor.", NotifyType.ErrorMessage);
      break;

     case GeofenceMonitorStatus.Disabled:
      _rootPage.NotifyUser("Access to location is denied.", NotifyType.ErrorMessage);

      // Show the message to the user to go to the location settings.
      LocationDisabledMessage.Visibility = Visibility.Visible;
      break;

     case GeofenceMonitorStatus.NotInitialized:
      _rootPage.NotifyUser("The geofence monitor has not been initialized.", NotifyType.StatusMessage);
      break;

     case GeofenceMonitorStatus.NotAvailable:
      _rootPage.NotifyUser("The geofence monitor is not available.", NotifyType.ErrorMessage);
      break;

     default:
      ScenarioOutput_Status.Text = "Unknown";
      _rootPage.NotifyUser(string.Empty, NotifyType.StatusMessage);
      break;
    }
   });
}
```

## <a name="set-up-foreground-notifications"></a>Configurer les notifications au premier plan


Après avoir créé vos limites géographiques, vous devez ajouter la logique pour gérer ce qui se passe en cas d’événement de limite géographique. En fonction de l’élément [**MonitoredStates**](/uwp/api/windows.devices.geolocation.geofencing.geofence.monitoredstates) que vous avez configuré, il est possible que vous receviez un événement quand :

-   L’utilisateur est entré dans une région d’intérêt.
-   L’utilisateur quitte une région d’intérêt.
-   La limite géographique a expiré ou a été supprimée. Notez qu’aucune application en arrière-plan n’est activée pour un événement de suppression.

Vous pouvez écouter des événements directement à partir de votre application en cours d’exécution, ou bien vous inscrire à une tâche en arrière-plan afin de recevoir une notification en arrière-plan dès qu’un événement survient.

### <a name="step-1-register-for-geofence-state-change-events"></a>Étape 1 : inscrire les événements de changement d’état de limite géographique

Pour que votre application reçoive une notification au premier plan d’un changement d’état de limite géographique, vous devez inscrire un gestionnaire d’événements. Cela est généralement configuré lors de la création de la limite géographique.

```csharp
private void Initialize()
{
    // Other initialization logic

    GeofenceMonitor.Current.GeofenceStateChanged += OnGeofenceStateChanged;
}

```

### <a name="step-2-implement-the-geofence-event-handler"></a>Étape 2 : implémenter le gestionnaire d’événements de limite géographique

L’étape suivante consiste à implémenter les gestionnaires d’événements. L’action à entreprendre ici dépend de la raison pour laquelle votre application utilise la limite géographique.

```csharp
public async void OnGeofenceStateChanged(GeofenceMonitor sender, object e)
{
    var reports = sender.ReadReports();

    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        foreach (GeofenceStateChangeReport report in reports)
        {
            GeofenceState state = report.NewState;

            Geofence geofence = report.Geofence;

            if (state == GeofenceState.Removed)
            {
                // Remove the geofence from the geofences collection.
                GeofenceMonitor.Current.Geofences.Remove(geofence);
            }
            else if (state == GeofenceState.Entered)
            {
                // Your app takes action based on the entered event.

                // NOTE: You might want to write your app to take a particular
                // action based on whether the app has internet connectivity.

            }
            else if (state == GeofenceState.Exited)
            {
                // Your app takes action based on the exited event.

                // NOTE: You might want to write your app to take a particular
                // action based on whether the app has internet connectivity.

            }
        }
    });
}



```

## <a name="set-up-background-notifications"></a>Configurer les notifications en arrière-plan


Après avoir créé vos limites géographiques, vous devez ajouter la logique pour gérer ce qui se passe en cas d’événement de limite géographique. En fonction de l’élément [**MonitoredStates**](/uwp/api/windows.devices.geolocation.geofencing.geofence.monitoredstates) que vous avez configuré, il est possible que vous receviez un événement quand :

-   L’utilisateur est entré dans une région d’intérêt.
-   L’utilisateur quitte une région d’intérêt.
-   La limite géographique a expiré ou a été supprimée. Notez qu’aucune application en arrière-plan n’est activée pour un événement de suppression.

Pour écouter un événement de limite géographique en arrière-plan

-   Déclarez la tâche en arrière-plan dans le manifeste de votre application.
-   Inscrivez la tâche en arrière-plan dans votre application. Si votre application nécessite un accès à Internet (par exemple, pour accéder à un service cloud), vous pouvez définir un indicateur à cet effet lorsque l’événement est déclenché. Vous pouvez aussi définir un indicateur pour vous assurer que l’utilisateur est présent au déclenchement de l’événement et être certain que l’utilisateur en est informé.
-   Tandis que votre application est exécutée au premier plan, invitez l’utilisateur à lui accorder des autorisations d’emplacement.

### <a name="step-1-register-for-geofence-state-change-events"></a>Étape 1 : inscrire les événements de changement d’état de limite géographique

Dans le manifeste de votre application, dans l’onglet **Déclarations**, ajoutez une déclaration pour une tâche en arrière-plan d’emplacement. Pour ce faire :

-   Ajoutez une déclaration de type **Tâches en arrière-plan**.
-   Définissez une tâche de propriété de type **Emplacement**.
-   Dans votre application, définissez un point d’entrée à entrer au moment où l’événement est déclenché.

### <a name="step-2-register-the-background-task"></a>Étape 2 : inscrire la tâche en arrière-plan

Le code utilisé dans cette étape inscrit la tâche de limite géographique en arrière-plan. Rappelez-vous que lors de la création de la clôture géographique, nous avons vérifié les autorisations relatives à l’emplacement.

```csharp
async private void RegisterBackgroundTask(object sender, RoutedEventArgs e)
{
    // Get permission for a background task from the user. If the user has already answered once,
    // this does nothing and the user must manually update their preference via PC Settings.
    BackgroundAccessStatus backgroundAccessStatus = await BackgroundExecutionManager.RequestAccessAsync();

    // Regardless of the answer, register the background task. Note that the user can use
    // the Settings app to prevent your app from running background tasks.
    // Create a new background task builder.
    BackgroundTaskBuilder geofenceTaskBuilder = new BackgroundTaskBuilder();

    geofenceTaskBuilder.Name = SampleBackgroundTaskName;
    geofenceTaskBuilder.TaskEntryPoint = SampleBackgroundTaskEntryPoint;

    // Create a new location trigger.
    var trigger = new LocationTrigger(LocationTriggerType.Geofence);

    // Associate the location trigger with the background task builder.
    geofenceTaskBuilder.SetTrigger(trigger);

    // If it is important that there is user presence and/or
    // internet connection when OnCompleted is called
    // the following could be called before calling Register().
    // SystemCondition condition = new SystemCondition(SystemConditionType.UserPresent | SystemConditionType.InternetAvailable);
    // geofenceTaskBuilder.AddCondition(condition);

    // Register the background task.
    geofenceTask = geofenceTaskBuilder.Register();

    // Associate an event handler with the new background task.
    geofenceTask.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);

    BackgroundTaskState.RegisterBackgroundTask(BackgroundTaskState.LocationTriggerBackgroundTaskName);

    switch (backgroundAccessStatus)
    {
    case BackgroundAccessStatus.Unspecified:
    case BackgroundAccessStatus.Denied:
        rootPage.NotifyUser("This app is not allowed to run in the background.", NotifyType.ErrorMessage);
        break;

    }
}


```

### <a name="step-3-handling-the-background-notification"></a>Étape 3 : gérer la notification en arrière-plan

Les actions que vous entreprenez pour notifier l’utilisateur dépendent de ce qu’accomplit votre application, mais vous pouvez afficher une notification toast, diffuser un son ou mettre à jour une vignette dynamique. Le code utilisé dans cette étape gère la notification.

```csharp
async private void OnCompleted(IBackgroundTaskRegistration sender, BackgroundTaskCompletedEventArgs e)
{
    if (sender != null)
    {
        // Update the UI with progress reported by the background task.
        await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            try
            {
                // If the background task threw an exception, display the exception in
                // the error text box.
                e.CheckResult();

                // Update the UI with the completion status of the background task.
                // The Run method of the background task sets the LocalSettings.
                var settings = ApplicationData.Current.LocalSettings;

                // Get the status.
                if (settings.Values.ContainsKey("Status"))
                {
                    rootPage.NotifyUser(settings.Values["Status"].ToString(), NotifyType.StatusMessage);
                }

                // Do your app work here.

            }
            catch (Exception ex)
            {
                // The background task had an error.
                rootPage.NotifyUser(ex.ToString(), NotifyType.ErrorMessage);
            }
        });
    }
}


```

## <a name="change-the-privacy-settings"></a>Modifier les paramètres de confidentialité


Si les paramètres de confidentialité d’emplacement n’autorisent pas votre application à accéder à l’emplacement de l’utilisateur, nous vous recommandons de fournir un lien pratique vers les **paramètres de confidentialité d’emplacement** dans l’application **Paramètres**. Dans cet exemple, un contrôle de lien hypertexte est utilisé pour accéder à l’URI `ms-settings:privacy-location`.

```xml
<!--Set Visibility to Visible when access to the user's location is denied. -->  
<TextBlock x:Name="LocationDisabledMessage" FontStyle="Italic"
                 Visibility="Collapsed" Margin="0,15,0,0" TextWrapping="Wrap" >
          <Run Text="This app is not able to access Location. Go to " />
              <Hyperlink NavigateUri="ms-settings:privacy-location">
                  <Run Text="Settings" />
              </Hyperlink>
          <Run Text=" to check the location privacy settings."/>
</TextBlock>
```

Par ailleurs, votre application peut appeler la méthode[**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync) pour lancer l’application **Paramètres** à partir du code. Pour plus d’informations, voir [Lancer l’application Paramètres Windows](../launch-resume/launch-settings-app.md).

```csharp
using Windows.System;
...
bool result = await Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-location"));
```

## <a name="test-and-debug-your-app"></a>Tester et déboguer votre application


Il est parfois difficile de procéder au test et au débogage d’applications de limite géographique, car ces dernières dépendent de l’emplacement d’un appareil. Nous présentons ici plusieurs méthodes qui permettent de tester les limites géographiques au premier plan et en arrière-plan.

**Pour déboguer une application de limite géographique, vous pouvez :**

1.  Déplacer physiquement l’appareil
2.  tester l’entrée dans une zone de clôture virtuelle en créant une zone qui inclut votre emplacement physique actuel, afin que vous soyez déjà dans la zone de clôture virtuelle et que l’événement d’« entrée dans la zone de clôture virtuelle » se déclenche immédiatement ;
3.  Utiliser l’émulateur de Microsoft Visual Studio pour simuler des emplacements pour l’appareil

### <a name="test-and-debug-a-geofencing-app-that-is-running-in-the-foreground"></a>Tester et déboguer une application de limite géographique s’exécutant au premier plan

**Pour tester votre application de limite géographique s’exécutant au premier plan**

1.  Générez votre application dans Visual Studio.
2.  Lancez votre application dans l’émulateur Visual Studio.
3.  Utilisez ces outils pour simuler différents emplacements internes et externes à votre zone de clôture virtuelle. Attendez suffisamment longtemps après l’heure spécifiée par la propriété [**DwellTime**](/uwp/api/windows.devices.geolocation.geofencing.geofence.dwelltime) pour déclencher l’événement. Notez que vous devez autoriser la demande de localisation géographique pour l’application. Pour plus d’informations sur la simulation d’emplacements, voir [Définir la localisation géographique simulée du périphérique](/previous-versions/hh441475(v=vs.120)#BKMK_Set_the_simulated_geo_location_of_the_device).
4.  Vous pouvez également utiliser l’émulateur pour estimer la taille des limites et le temps approximatif nécessaire à leur détection à différents débits.

### <a name="test-and-debug-a-geofencing-app-that-is-running-in-the-background"></a>Tester et déboguer une application de limite géographique en arrière-plan

**Pour tester votre application de limite géographique s’exécutant en arrière-plan**

1.  Générez votre application dans Visual Studio. Notez que votre application doit définir le type de tâche en arrière-plan appelé **Emplacement**.
2.  Déployez d’abord l’application en local.
3.  Fermez votre application en cours d’exécution localement.
4.  Lancez votre application dans l’émulateur Visual Studio. Notez que la simulation de géorepérage en arrière-plan n’est prise en charge que sur une seule application à la fois dans l’émulateur. Ne lancez pas plusieurs applications de géorepérage dans l’émulateur.
5.  À l’aide de l’émulateur, simulez différents emplacements internes et externes à votre zone de clôture virtuelle. Attendez suffisamment longtemps après l’élément [**DwellTime**](/uwp/api/windows.devices.geolocation.geofencing.geofence.dwelltime) pour déclencher l’événement. Notez que vous devez autoriser la demande de localisation géographique pour l’application.
6.  Utilisez Visual Studio pour déclencher la tâche d’arrière-plan relative à l’emplacement. Pour plus d’informations sur le déclenchement de tâches en arrière-plan dans Visual Studio, voir [Comment déclencher les tâches en arrière-plan](/previous-versions/hh974425(v=vs.120)#BKMK_Trigger_background_tasks).

## <a name="troubleshoot-your-app"></a>Résoudre les problèmes de votre application


Pour que votre application puisse accéder à l’emplacement, l’option **Localisation** doit être activée sur l’appareil. Dans l’application **Paramètres**, vérifiez que les **paramètres de confidentialité relatifs à la géolocalisation** suivants sont bien activés :

-   Le paramètre **Emplacement de cet appareil...** est **activé** (non applicable dans Windows 10 Mobile).
-   Le paramètre des services de localisation **Emplacement** est **activé**.
-   Sous **Choisir les applications qui peuvent utiliser votre emplacement**, votre application est **activée**.

## <a name="related-topics"></a>Rubriques connexes

* [Exemple de géolocalisation UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)
* [Recommandations en matière de conception pour le géorepérage](./guidelines-for-geofencing.md)
* [Recommandations de conception pour les applications prenant en charge la géolocalisation](./guidelines-and-checklist-for-detecting-location.md)
