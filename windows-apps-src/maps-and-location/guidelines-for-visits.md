---
Description: Découvrez comment utiliser la puissante fonctionnalité de suivi des visites pour rendre plus pratique le suivi de l’emplacement.
title: Instructions pour l’utilisation du suivi des visites
ms.assetid: 0c101684-48a9-4592-9ed5-6c20f3b830f2
ms.date: 05/18/2017
ms.topic: article
keywords: Windows 10, UWP, carte, emplacement, géoséjour, géoséjours
ms.localizationpriority: medium
ms.openlocfilehash: 3b1766d0f883fa42b005908dcc63102e97ff0d4f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162513"
---
# <a name="guidelines-for-using-visits-tracking"></a>Instructions pour l’utilisation du suivi des visites

La fonctionnalité de visites rationalise le processus de suivi des emplacements afin de la rendre plus efficace pour les besoins pratiques de nombreuses applications. Une visite est définie comme une zone géographique significative que l’utilisateur entre et quitte. Les visites sont similaires aux [limites géographiques](guidelines-for-geofencing.md) dans la mesure où elles permettent à l’application d’être notifiée uniquement lorsque l’utilisateur entre ou quitte certaines zones d’intérêt, éliminant ainsi la nécessité d’un suivi de l’emplacement continu, qui peut constituer un drain sur la durée de vie de la batterie. Toutefois, contrairement aux limites géographiques, les zones de visite sont identifiées de manière dynamique au niveau de la plateforme et n’ont pas besoin d’être définies explicitement par des applications individuelles. En outre, la sélection des visites d’une application est gérée par un seul paramètre de granularité, plutôt que par l’abonnement à des emplacements individuels.

## <a name="preliminary-setup"></a>Installation préliminaire

Avant de poursuivre, assurez-vous que votre application est en charge de l’accès à l’emplacement de l’appareil. Vous devez déclarer la `Location` fonctionnalité dans le manifeste et appeler la méthode **[géolocator. RequestAccessAsync](/uwp/api/Windows.Devices.Geolocation.Geolocator.RequestAccessAsync)** pour vous assurer que les utilisateurs accordent des autorisations d’emplacement à l’application. Pour plus d’informations sur la façon de procéder, consultez [obtenir l’emplacement de l’utilisateur](get-location.md) . 

N’oubliez pas d’ajouter l' `Geolocation` espace de noms à votre classe. Cela sera nécessaire pour que tous les extraits de code de ce guide fonctionnent.

```csharp
using Windows.Devices.Geolocation;
```

## <a name="check-the-latest-visit"></a>Vérifier la dernière visite
La façon la plus simple d’utiliser la fonctionnalité de suivi des visites consiste à récupérer la dernière modification connue de l’état relatif à la visite. Un changement d’État est un événement qui se connecte à la plateforme dans lequel l’utilisateur entre/quitte un emplacement de signification, il y a un déplacement important depuis le dernier rapport ou l’emplacement de l’utilisateur est perdu (Voir l’énumération **[VisitStateChange](/uwp/api/windows.devices.geolocation.visitstatechange)** ). Les modifications d’État sont représentées par les instances de **[géovisite](/uwp/api/windows.devices.geolocation.geovisit)** . Pour récupérer l’instance de **géovisite** pour le dernier changement d’état enregistré, utilisez simplement la méthode désignée dans la classe **[GeovisitMonitor](/uwp/api/windows.devices.geolocation.geovisitmonitor)** .

> [!NOTE]
> La vérification de la dernière visite enregistrée ne garantit pas que les visites sont actuellement suivies par le système. Pour effectuer le suivi des visites à mesure qu’elles se produisent, vous devez soit les surveiller au premier plan, soit vous inscrire pour le suivi en arrière-plan (voir les sections ci-dessous).

```csharp
private async void GetLatestStateChange() {
    // retrieve the Geovisit instance
    Geovisit latestVisit = await GeovisitMonitor.GetLastReportAsync();

    // Using the properties of "latestVisit", parse out the time that the state 
    // change was recorded, the device's location when the change was recorded,
    // and the type of state change.
}
```

### <a name="parse-a-geovisit-instance-optional"></a>Analyser une instance de géovisite (facultatif)
La méthode suivante convertit toutes les informations stockées dans une instance de **géovisite** en une chaîne facilement lisible. Il peut être utilisé dans n’importe quel scénario de ce guide pour vous aider à fournir des commentaires sur les visites signalées.

```csharp
private string ParseGeovisit(Geovisit visit){
    string visitString = null;

    // Use a DateTimeFormatter object to process the timestamp. The following
    // format configuration will give an intuitive representation of the date/time
    Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatterLongTime;
    
    formatterLongTime = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter(
        "{hour.integer}:{minute.integer(2)}:{second.integer(2)}", new[] { "en-US" }, "US", 
        Windows.Globalization.CalendarIdentifiers.Gregorian, 
        Windows.Globalization.ClockIdentifiers.TwentyFourHour);
    
    // use this formatter to convert the timestamp to a string, and add to "visitString"
    visitString = formatterLongTime.Format(visit.Timestamp);

    // Next, add on the state change type value
    visitString += " " + visit.StateChange.ToString();

    // Next, add the position information (if any is provided; this will be null if 
    // the reported event was "TrackingLost")
    if (visit.Position != null) {
        visitString += " (" +
        visit.Position.Coordinate.Point.Position.Latitude.ToString() + "," +
        visit.Position.Coordinate.Point.Position.Longitude.ToString() + 
        ")";
    }

    return visitString;
}
```

## <a name="monitor-visits-in-the-foreground"></a>Surveiller les visites au premier plan

La classe **GeovisitMonitor** utilisée dans la section précédente gère également le scénario d’écoute des modifications d’État sur une période donnée. Pour ce faire, vous pouvez instancier cette classe, inscrire une méthode de gestionnaire pour son événement et appeler la `Start` méthode.

```csharp
// this GeovisitMonitor instance will belong to the class scope
GeovisitMonitor monitor;

public void RegisterForVisits() {

    // Create and initialize a new monitor instance.
    monitor = new GeovisitMonitor();
    
    // Attach a handler to receive state change notifications.
    monitor.VisitStateChanged += OnVisitStateChanged;
    
    // Calling the start method will start Visits tracking for a specified scope:
    // For higher granularity such as venue/building level changes, choose Venue.
    // For lower granularity in the range of zipcode level changes, choose City.
    monitor.Start(VisitMonitoringScope.Venue);
}
```

Dans cet exemple, la `OnVisitStateChanged` méthode gère les rapports de visite entrants. L’instance de **géovisite** correspondante est transmise par le biais du paramètre d’événement.

```csharp
private void OnVisitStateChanged(GeoVisitWatcher sender, GeoVisitStateChangedEventArgs args) {
    Geovisit visit = args.Visit;
    
    // Using the properties of "visit", parse out the time that the state 
    // change was recorded, the device's location when the change was recorded,
    // and the type of state change.
}
```
Lorsque l’application a terminé la surveillance des modifications d’État liées à la visite, elle doit arrêter le moniteur et annuler l’inscription du ou des gestionnaires d’événements. Cela doit également être fait chaque fois que l’application est suspendue ou fermée.

```csharp
public void UnregisterFromVisits() {
    
    // Stop the monitor to stop tracking Visits. Otherwise, tracking will
    // continue until the monitor instance is destroyed.
    monitor.Stop();
    
    // Remove the handler to stop receiving state change events.
    monitor.VisitStateChanged -= OnVisitStateChanged;
}
```

## <a name="monitor-visits-in-the-background"></a>Surveiller les visites en arrière-plan

Vous pouvez également implémenter la surveillance de visite dans une tâche en arrière-plan, afin que l’activité relative à la visite puisse être gérée sur l’appareil, même lorsque votre application n’est pas ouverte. Il s’agit de la méthode recommandée, car elle est plus polyvalente et économe en énergie. 

Ce guide utilise le modèle dans [créer et inscrire une tâche en arrière-plan out-of-process](../launch-resume/create-and-register-a-background-task.md), dans laquelle les fichiers de l’application principale résident dans un projet et le fichier de tâche en arrière-plan se trouvent dans un projet distinct de la même solution. Si vous débutez avec l’implémentation de tâches en arrière-plan, il est recommandé de suivre les instructions ci-dessous, en effectuant les substitutions nécessaires ci-dessous pour créer une tâche en arrière-plan de gestion des visites.

> [!NOTE]
> Dans les extraits de code suivants, certaines fonctionnalités importantes telles que la gestion des erreurs et le stockage local sont absentes pour des raisons de simplicité. Pour une implémentation fiable du traitement des visites en arrière-plan, consultez l' [exemple d’application](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation).


Tout d’abord, assurez-vous que votre application a déclaré des autorisations de tâche en arrière-plan. Dans l' `Application/Extensions` élément de votre fichier *Package. appxmanifest* , ajoutez l’extension suivante (ajoutez un `Extensions` élément s’il n’en existe pas déjà un).

```xml
<Extension Category="windows.backgroundTasks" EntryPoint="Tasks.VisitBackgroundTask">
    <BackgroundTasks>
        <Task Type="location" />
    </BackgroundTasks>
</Extension>
```

Ensuite, dans la définition de la classe de tâche en arrière-plan, collez le code suivant. La `Run` méthode de cette tâche d’arrière-plan passera simplement les détails du déclencheur (qui contiennent les informations de visite) dans une méthode distincte.

```csharp
using Windows.ApplicationModel.Background;

namespace Tasks {
    
    public sealed class VisitBackgroundTask : IBackgroundTask {
        
        public void Run(IBackgroundTaskInstance taskInstance) {
            
            // get a deferral
            BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
            
            // this task's trigger will be a Geovisit trigger
            GeovisitTriggerDetails triggerDetails = taskInstance.TriggerDetails as GeovisitTriggerDetails;

            // Handle Visit reports
            GetVisitReports(triggerDetails);         

            finally {
                deferral.Complete();
            }
        }        
    }
}
```

Définissez la `GetVisitReports` méthode quelque part dans cette même classe.

```csharp
private void GetVisitReports(GeovisitTriggerDetails triggerDetails) {

    // Read reports from the triggerDetails. This populates the "reports" variable 
    // with all of the Geovisit instances that have been logged since the previous
    // report reading.
    IReadOnlyList<Geovisit> reports = triggerDetails.ReadReports();

    foreach (Geovisit report in reports) {
        // Using the properties of "visit", parse out the time that the state 
        // change was recorded, the device's location when the change was recorded,
        // and the type of state change.
    }

    // Note: depending on the intent of the app, you many wish to store the
    // reports in the app's local storage so they can be retrieved the next time 
    // the app is opened in the foreground.
}
```

Ensuite, dans le projet principal de votre application, vous devez effectuer l’inscription de cette tâche en arrière-plan. Créez une méthode d’inscription qui peut être appelée par une action de l’utilisateur ou qui est appelée chaque fois que la classe est activée.

```csharp
// a reference to this registration should be declared at the class level
private IBackgroundTaskRegistration visitTask = null;

// The app must call this method at some point to allow future use of 
// the background task. 
private async void RegisterBackgroundTask(object sender, RoutedEventArgs e) {
    
    string taskName = "MyVisitTask";
    string taskEntryPoint = "Tasks.VisitBackgroundTask";

    // First check whether the task in question is already registered
    foreach (var task in BackgroundTaskRegistration.AllTasks) {
        if (task.Value.Name == taskName) {
            // if a task is found with the name of this app's background task, then
            // return and do not attempt to register this task
            return;
        }
    }
    
    // Attempt to register the background task.
    try {
        // Get permission for a background task from the user. If the user has 
        // already responded once, this does nothing and the user must manually 
        // update their preference via Settings.
        BackgroundAccessStatus backgroundAccessStatus = await BackgroundExecutionManager.RequestAccessAsync();

        switch (backgroundAccessStatus) {
            case BackgroundAccessStatus.AlwaysAllowed:
            case BackgroundAccessStatus.AllowedSubjectToSystemPolicy:
                // BackgroundTask is allowed
                break;

            default:
                // notify user that background tasks are disabled for this app
                //...
                break;
        }

        // Create a new background task builder
        BackgroundTaskBuilder visitTaskBuilder = new BackgroundTaskBuilder();

        visitTaskBuilder.Name = exampleTaskName;
        visitTaskBuilder.TaskEntryPoint = taskEntryPoint;

        // Create a new Visit trigger
        var trigger = new GeovisitTrigger();

        // Set the desired monitoring scope.
        // For higher granularity such as venue/building level changes, choose Venue.
        // For lower granularity in the range of zipcode level changes, choose City. 
        trigger.MonitoringScope = VisitMonitoringScope.Venue; 

        // Associate the trigger with the background task builder
        visitTaskBuilder.SetTrigger(trigger);

        // Register the background task
        visitTask = visitTaskBuilder.Register();      
    }
    catch (Exception ex) {
        // notify user that the task failed to register, using ex.ToString()
    }
}
```

Cela établit qu’une classe de tâche en arrière-plan appelée `VisitBackgroundTask` dans l’espace de noms fera une action `Tasks` avec le `location` type de déclencheur. 

Votre application doit maintenant être en mesure d’inscrire la tâche en arrière-plan de gestion des visites. cette tâche doit être activée chaque fois que l’appareil enregistre une modification d’État liée à la visite. Vous devrez renseigner la logique de votre classe de tâche en arrière-plan pour déterminer ce que vous devez faire avec ces informations de changement d’État.

## <a name="related-topics"></a>Rubriques connexes
* [Créer et inscrire une tâche en arrière-plan hors processus](../launch-resume/create-and-register-a-background-task.md)
* [Obtenir la localisation de l’utilisateur](get-location.md)
* [Espace de noms Windows. Devices. géolocalisation](/uwp/api/windows.devices.geolocation)