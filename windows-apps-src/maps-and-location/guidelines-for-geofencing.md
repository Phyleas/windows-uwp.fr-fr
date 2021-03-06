---
description: Consultez les instructions et meilleures pratiques pour l’utilisation de géoclôtures pour fournir des expériences contextuelles géographiquement dans votre application.
title: Recommandations concernant la clôture virtuelle des applications
ms.assetid: F817FA55-325F-4302-81BE-37E6C7ADC281
ms.date: 10/20/2020
ms.topic: article
keywords: Windows 10, UWP, carte, emplacement, géoclôture
ms.localizationpriority: medium
ms.openlocfilehash: e4d033673acbb4a8b7fd558d9e6c4f8329d79bf5
ms.sourcegitcommit: 7aaf0740a5d3a17ebf9214aa5e5d056924317673
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92297716"
---
# <a name="guidelines-for-geofencing-apps"></a>Recommandations concernant la clôture virtuelle des applications

> [!NOTE]
> [**Collection MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) et les services cartographiques ont une clé d’authentification Maps appelée [**MapServiceToken**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapservicetoken). Pour plus d’informations sur l’obtention et la définition d’une clé d’authentification de cartes, voir [Demander une clé d’authentification de cartes](authentication-key.md).

**API importantes**

-   [**Geofence class (XAML)**](/uwp/api/Windows.Devices.Geolocation.Geofencing.Geofence)
-   [**Geolocator class (XAML)**](/uwp/api/Windows.Devices.Geolocation.Geolocator)

Suivez ces meilleures pratiques pour définir la [**clôture virtuelle**](/uwp/api/Windows.Devices.Geolocation.Geofencing) dans votre application.

## <a name="recommendations"></a>Recommandations


-   Si votre application a besoin d’un accès à Internet lorsqu’un événement de [**limite géographique**](/uwp/api/Windows.Devices.Geolocation.Geofencing.Geofence) se produit, vérifiez l’accès à Internet avant de créer la limite géographique.
    -   Si l’application ne dispose pas d’un accès à Internet, vous pouvez inviter l’utilisateur à se connecter à Internet avant de configurer la clôture virtuelle.
    -   Si aucun accès à Internet n’est possible, économisez l’énergie nécessaire à la recherche d’emplacements par clôture virtuelle.
-   Assurez-vous que les notifications de clôture virtuelle sont appropriées en vérifiant l’horodatage et l’emplacement actuel lorsqu’un événement de géorepérage indique un changement apporté à un état [**Entered**](/uwp/api/Windows.Devices.Geolocation.Geofencing.GeofenceState) ou **Exited**. Pour plus d’informations, voir la section ci-dessous **Vérification de l’horodatage et de l’emplacement actuel**.
(#timestamp) ci-dessous pour plus d’informations.
-   Créez des exceptions qui permettent de gérer les cas où un périphérique n’a pas accès aux informations sur l’emplacement et d’en notifier l’utilisateur si nécessaire. La non-disponibilité des informations sur l’emplacement peut avoir différentes causes : les autorisations sont désactivées, le périphérique n’est pas pourvu d’une radio GPS, le signal GPS est bloqué ou le signal Wi-Fi n’est pas assez fort.
-   En règle générale, votre application n’a pas besoin de détecter les événements de clôture virtuelle au premier plan et en arrière-plan simultanément. Dans le cas contraire, suivez les recommandations ci-après :

    -   Appelez la méthode [**ReadReports**](/uwp/api/windows.devices.geolocation.geofencing.geofencemonitor.readreports) pour savoir si un événement s’est produit.
    -   Désinscrivez votre détecteur d’événements au premier plan lorsque votre application n’est pas visible pour l’utilisateur et réinscrivez-le quand elle redevient visible.

    Pour plus d’informations et pour obtenir des exemples de code, voir [Détecteurs en arrière-plan et au premier plan](#background-and-foreground-listeners).

-   N’utilisez pas plus de 1000 clôtures virtuelles par application. Le système prend en charge plusieurs milliers de clôtures virtuelles par application, mais en limitant leur nombre à 1000, vous optimiserez les performances de votre application et contribuerez à réduire l’utilisation de la mémoire par l’application.
-   Ne créez pas de clôtures virtuelles avec un rayon de moins de 50 mètres. Si votre application doit utiliser une clôture virtuelle avec un petit rayon, conseillez aux utilisateurs d’utiliser votre application sur un appareil équipé d’une radio GPS pour garantir des performances optimales.

## <a name="additional-usage-guidance"></a>Indications d’utilisation supplémentaires

### <a name="checking-the-time-stamp-and-current-location"></a>Vérification de l’horodatage et de l’emplacement actuel

Lorsqu’un événement indique un changement apporté à un état [**Entered**](/uwp/api/Windows.Devices.Geolocation.Geofencing.GeofenceState) ou **Exited**, vérifiez à la fois l’horodatage de l’événement et votre emplacement actuel. Divers facteurs peuvent avoir une incidence sur le moment où l’événement est réellement traité par l’utilisateur : par exemple, le système ne dispose pas de ressources suffisantes pour lancer une tâche en arrière-plan, l’utilisateur ne remarque pas la notification ou le périphérique est en état de veille (sur Windows). Par exemple, il peut se produire la séquence suivante :

-   Votre application crée une clôture virtuelle et surveille la présence d’événements Enter et Exit pour cette dernière.
-   L’utilisateur déplace l’appareil à l’intérieur de la clôture virtuelle, ce qui provoque le déclenchement d’un événement Enter.
-   Votre application envoie une notification à l’utilisateur et l’informe qu’il se trouve à présent à l’intérieur de la clôture virtuelle.
-   Occupé, l’utilisateur ne remarque la notification que dix minutes plus tard.
-   Pendant ce laps de temps, l’utilisateur est repassé à l’extérieur de la clôture virtuelle.

À partir de l’horodatage, vous pouvez constater que l’action est survenue dans le passé. À partir de l’emplacement actuel, vous pouvez voir que l’utilisateur se trouve de nouveau en dehors de la clôture virtuelle. Selon les fonctionnalités de votre application, vous pouvez filtrer cet événement.

### <a name="background-and-foreground-listeners"></a>Détecteurs en arrière-plan et au premier plan

En règle générale, votre application n’a pas besoin de détecter les événements [**Geofence**](/uwp/api/Windows.Devices.Geolocation.Geofencing.Geofence) au premier plan et en arrière-plan simultanément. Toutefois, si cela est nécessaire, la méthode la plus sûre est de confier la gestion des notifications à la tâche en arrière-plan. Si vous configurez les écouteurs de la limite de premier plan et d’arrière-plan, il n’y a aucune garantie qui sera déclenchée en premier, et vous devez donc toujours appeler la méthode [**ReadReports**](/uwp/api/windows.devices.geolocation.geofencing.geofencemonitor.readreports) pour déterminer si un événement s’est produit.

Si vous avez configuré des détecteurs de clôtures virtuelles au premier plan et en arrière-plan, vous devez désinscrire votre détecteur d’événements au premier plan lorsque votre application n’est pas visible pour l’utilisateur et le réinscrire quand elle redevient visible. L’exemple de code présenté ci-dessous permet d’inscrire l’événement de visibilité.

```csharp
    Windows.UI.Core.CoreWindow coreWindow;    

    // This needs to be set before InitializeComponent sets up event registration for app visibility
    coreWindow = CoreWindow.GetForCurrentThread();
    coreWindow.VisibilityChanged += OnVisibilityChanged;
```

```javascript
 document.addEventListener("visibilitychange", onVisibilityChanged, false);
```

Lorsque la visibilité change, vous pouvez activer ou désactiver les gestionnaires d’événements au premier plan comme le montre cet exemple.

```csharp
private void OnVisibilityChanged(CoreWindow sender, VisibilityChangedEventArgs args)
{
    // NOTE: After the app is no longer visible on the screen and before the app is suspended
    // you might want your app to use toast notification for any geofence activity.
    // By registering for VisibiltyChanged the app is notified when the app is no longer visible in the foreground.

    if (args.Visible)
    {
        // register for foreground events
        GeofenceMonitor.Current.GeofenceStateChanged += OnGeofenceStateChanged;
        GeofenceMonitor.Current.StatusChanged += OnGeofenceStatusChanged;
    }
    else
    {
        // unregister foreground events (let background capture events)
        GeofenceMonitor.Current.GeofenceStateChanged -= OnGeofenceStateChanged;
        GeofenceMonitor.Current.StatusChanged -= OnGeofenceStatusChanged;
    }
}
```

```javascript
function onVisibilityChanged() {
    // NOTE: After the app is no longer visible on the screen and before the app is suspended
    // you might want your app to use toast notification for any geofence activity.
    // By registering for VisibiltyChanged the app is notified when the app is no longer visible in the foreground.

    if (document.msVisibilityState === "visible") {
        // register for foreground events
        Windows.Devices.Geolocation.Geofencing.GeofenceMonitor.current.addEventListener("geofencestatechanged", onGeofenceStateChanged);
        Windows.Devices.Geolocation.Geofencing.GeofenceMonitor.current.addEventListener("statuschanged", onGeofenceStatusChanged);
    } else {
        // unregister foreground events (let background capture events)
        Windows.Devices.Geolocation.Geofencing.GeofenceMonitor.current.removeEventListener("geofencestatechanged", onGeofenceStateChanged);
        Windows.Devices.Geolocation.Geofencing.GeofenceMonitor.current.removeEventListener("statuschanged", onGeofenceStatusChanged);
    }
}
```

### <a name="sizing-your-geofences"></a>Dimensionnement de vos clôtures virtuelles

Si le GPS est capable de fournir les informations d’emplacement les plus précises, le géorepérage peut également se servir du Wi-Fi ou d’autres capteurs d’emplacement pour déterminer la position actuelle de l’utilisateur. En revanche, l’emploi de ces autres méthodes peut affecter la taille des clôtures virtuelles qu’il vous est possible de créer. Si le niveau de précision est faible, la création de clôtures virtuelles de petite taille n’est pas utile. En général, nous vous recommandons de ne pas créer de clôture virtuelle avec un rayon inférieur à 50 mètres. Par ailleurs, notez que les tâches de clôture virtuelle en arrière-plan sont exécutées seulement de manière régulière sur Windows, ce qui peut vous faire manquer un événement [**Enter**](/uwp/api/Windows.Devices.Geolocation.Geofencing.GeofenceState) ou **Exit** entier si vous utilisez une petite clôture virtuelle.

Si votre application doit utiliser une clôture virtuelle avec un petit rayon, conseillez aux utilisateurs d’utiliser votre application sur un appareil équipé d’une radio GPS pour garantir des performances optimales.

## <a name="related-topics"></a>Rubriques connexes


* [Configurer une limite géographique](./set-up-a-geofence.md)
* [Obtenir l’emplacement actuel](./get-location.md)
* [Exemple de géolocalisation UWP (géolocalisation)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)
 

 
