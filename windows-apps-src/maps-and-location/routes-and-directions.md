---
title: Afficher des itinéraires et indications sur une carte
description: Découvrez comment récupérer des itinéraires et des directions à l’aide de la classe MapRouteFinder et les afficher sur un collection MapControl dans une application plateforme Windows universelle (UWP).
ms.assetid: BBB4C23A-8F10-41D1-81EA-271BE01AED81
ms.date: 09/20/2017
ms.topic: article
keywords: Windows 10, UWP, itinéraire, carte, emplacement, itinéraires
ms.localizationpriority: medium
ms.openlocfilehash: 49c3d47bb4e1a20f89200950d505d382ac2e8335
ms.sourcegitcommit: 45dec3dc0f14934b8ecf1ee276070b553f48074d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89094726"
---
# <a name="display-routes-and-directions-on-a-map"></a>Afficher des itinéraires et indications sur une carte



Demandez des itinéraires et des directions, puis affichez-les dans votre application.

>[!Note]
>Pour en savoir plus sur l’utilisation de Maps dans votre application, téléchargez l' [exemple de carte plateforme Windows universelle (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl).
>Si le mappage n’est pas une fonctionnalité de base de votre application, envisagez de lancer l’application Windows Maps à la place. Vous pouvez utiliser les schémas d’URI `bingmaps:`, `ms-drive-to:` et `ms-walk-to:` afin de lancer l’application Cartes Windows en accédant à des cartes, itinéraires et indications étape par étape spécifiques. Pour plus d’informations, consultez [Lancer l’application Cartes Windows](https://docs.microsoft.com/windows/uwp/launch-resume/launch-maps-app).

 
## <a name="an-intro-to-maproutefinder-results"></a>Introduction aux résultats de MapRouteFinder


Les classes pour les itinéraires et les indications sont associées comme suit :

* La classe [**MapRouteFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteFinder) a des méthodes qui obtiennent des itinéraires et des directions. Ces méthodes retournent un [**MapRouteFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteFinderResult).

* La classe [**MapRouteFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteFinderResult) contient un objet [**MapRoute**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRoute). Accédez à cet objet via la propriété [**Route**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutefinderresult.route) de la classe **MapRouteFinderResult**.

* La classe [**MapRoute**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRoute) contient une collection d’objets [**MapRouteLeg**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteLeg). Accédez à cette collection via la propriété [**Legs**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproute.legs) de la classe **MapRoute**.

* Chaque classe [**MapRouteLeg**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteLeg) contient une collection d’objets [**MapRouteManeuver**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteManeuver). Accédez à cette collection via la propriété [**Maneuvers**](https://docs.microsoft.com/uwp/api/windows.services.maps.maprouteleg.maneuvers) de la classe **MapRouteLeg**.

Recevez une route ou une direction de conduite en appelant les méthodes de la classe [**MapRouteFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteFinder) . Par exemple, [**GetDrivingRouteAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutefinder.getdrivingrouteasync) ou [**GetWalkingRouteAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutefinder.getwalkingrouteasync).

Lorsque vous demandez un itinéraire :

* Vous pouvez indiquer uniquement un point de départ et un point d’arrivée, ou une série de positions pour calculer l’itinéraire.

    L' *interruption* des points de route ajoute des jambes de route supplémentaires, chacune avec son propre itinéraire. Pour spécifier des points de route d' *arrêt* , utilisez l’une des surcharges [**GetDrivingRouteFromWaypointsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutefinder.getwalkingroutefromwaypointsasync) .

    *Via* un point de route définit des emplacements intermédiaires entre les points de route d' *arrêt* . Ils n’ajoutent pas de jambes de route.  Il s’agit simplement de points de route qu’un itinéraire doit traverser. Pour spécifier *via* des points de route, utilisez l’une des surcharges [**GetDrivingRouteFromEnhancedWaypointsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutefinder.getdrivingroutefromenhancedwaypointsasync) .

* Vous pouvez spécifier des optimisations (par exemple : réduire la distance).

* Vous pouvez spécifier des restrictions (par exemple : éviter les autoroutes).

## <a name="display-directions"></a>Afficher des indications

L’objet [**MapRouteFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteFinderResult) contient un objet [**MapRoute**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRoute) accessible via sa propriété [**Route**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutefinderresult.route).

Le [**MapRoute**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRoute) calculé a des propriétés qui fournissent le temps nécessaire pour parcourir l’itinéraire, la longueur de l’itinéraire et la collection d’objets [**MapRouteLeg**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteLeg) qui contiennent les côtés de l’itinéraire. Chaque objet **MapRouteLeg** contient une collection d’objets [**MapRouteManeuver**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteManeuver). L’objet **MapRouteManeuver** contient des indications accessibles via sa propriété [**InstructionText**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutemaneuver.instructiontext).

>[!IMPORTANT]
>Vous devez spécifier une clé d’authentification Maps pour pouvoir utiliser les services de mappage. Pour plus d’informations, voir [Demander une clé d’authentification pour Cartes](authentication-key.md).

 

```csharp
using System;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.Services.Maps;
using Windows.Devices.Geolocation;
...
private async void button_Click(object sender, RoutedEventArgs e)
{
   // Start at Microsoft in Redmond, Washington.
   BasicGeoposition startLocation = new BasicGeoposition() {Latitude=47.643,Longitude=-122.131};

   // End at the city of Seattle, Washington.
   BasicGeoposition endLocation = new BasicGeoposition() {Latitude = 47.604,Longitude= -122.329};

   // Get the route between the points.
   MapRouteFinderResult routeResult =
         await MapRouteFinder.GetDrivingRouteAsync(
         new Geopoint(startLocation),
         new Geopoint(endLocation),
         MapRouteOptimization.Time,
         MapRouteRestrictions.None);

   if (routeResult.Status == MapRouteFinderStatus.Success)
   {
      System.Text.StringBuilder routeInfo = new System.Text.StringBuilder();

      // Display summary info about the route.
      routeInfo.Append("Total estimated time (minutes) = ");
      routeInfo.Append(routeResult.Route.EstimatedDuration.TotalMinutes.ToString());
      routeInfo.Append("\nTotal length (kilometers) = ");
      routeInfo.Append((routeResult.Route.LengthInMeters / 1000).ToString());

      // Display the directions.
      routeInfo.Append("\n\nDIRECTIONS\n");

      foreach (MapRouteLeg leg in routeResult.Route.Legs)
      {
         foreach (MapRouteManeuver maneuver in leg.Maneuvers)
         {
            routeInfo.AppendLine(maneuver.InstructionText);
         }
      }

      // Load the text box.
      tbOutputText.Text = routeInfo.ToString();
   }
   else
   {
      tbOutputText.Text =
            "A problem occurred: " + routeResult.Status.ToString();
   }
}
```

Cet exemple montre les résultats suivants dans la zone de texte `tbOutputText`.

``` syntax
Total estimated time (minutes) = 18.4833333333333
Total length (kilometers) = 21.847

DIRECTIONS
Head north on 157th Ave NE.
Turn left onto 159th Ave NE.
Turn left onto NE 40th St.
Turn left onto WA-520 W.
Enter the freeway WA-520 from the right.
Keep left onto I-5 S/Portland.
Keep right and leave the freeway at exit 165A towards James St..
Turn right onto James St.
You have reached your destination.
```

## <a name="display-routes"></a>Afficher des itinéraires


Pour afficher un [**MapRoute**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRoute) sur un [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl), créez un [**MapRouteView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapRouteView) avec **MapRoute**. Ajoutez ensuite **MapRouteView** à la collection [**Routes**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.routes) de **MapControl**.

>[!IMPORTANT]
>Vous devez spécifier une clé d’authentification Maps pour pouvoir utiliser les services de mappage ou le contrôle de carte. Pour plus d’informations, voir [Demander une clé d’authentification pour Cartes](authentication-key.md).

 

```csharp
using System;
using Windows.Devices.Geolocation;
using Windows.Services.Maps;
using Windows.UI;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Maps;
...
private async void ShowRouteOnMap()
{
   // Start at Microsoft in Redmond, Washington.
   BasicGeoposition startLocation = new BasicGeoposition() { Latitude = 47.643, Longitude = -122.131 };

   // End at the city of Seattle, Washington.
   BasicGeoposition endLocation = new BasicGeoposition() { Latitude = 47.604, Longitude = -122.329 };


   // Get the route between the points.
   MapRouteFinderResult routeResult =
         await MapRouteFinder.GetDrivingRouteAsync(
         new Geopoint(startLocation),
         new Geopoint(endLocation),
         MapRouteOptimization.Time,
         MapRouteRestrictions.None);

   if (routeResult.Status == MapRouteFinderStatus.Success)
   {
      // Use the route to initialize a MapRouteView.
      MapRouteView viewOfRoute = new MapRouteView(routeResult.Route);
      viewOfRoute.RouteColor = Colors.Yellow;
      viewOfRoute.OutlineColor = Colors.Black;

      // Add the new MapRouteView to the Routes collection
      // of the MapControl.
      MapWithRoute.Routes.Add(viewOfRoute);

      // Fit the MapControl to the route.
      await MapWithRoute.TrySetViewBoundsAsync(
            routeResult.Route.BoundingBox,
            null,
            Windows.UI.Xaml.Controls.Maps.MapAnimationKind.None);
   }
}
```

Cet exemple affiche ce qui suit sur un [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) nommé **MapWithRoute**.

![Contrôle de carte avec l’itinéraire affiché.](images/routeonmap.png)

Voici une version de cet exemple qui utilise un *via* le point de route entre deux points d' *arrêt* :

```csharp
using System;
using Windows.Devices.Geolocation;
using Windows.Services.Maps;
using Windows.UI;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Maps;
...
private async void ShowRouteOnMap()
{
  Geolocator locator = new Geolocator();
  locator.DesiredAccuracyInMeters = 1;
  locator.PositionChanged += Locator_PositionChanged;

  BasicGeoposition point1 = new BasicGeoposition() { Latitude = 47.649693, Longitude = -122.144908 };
  BasicGeoposition point2 = new BasicGeoposition() { Latitude = 47.6205, Longitude = -122.3493 };
  BasicGeoposition point3 = new BasicGeoposition() { Latitude = 48.649693, Longitude = -122.144908 };

  // Get Driving Route from point A  to point B thru point C
  var path = new List<EnhancedWaypoint>();

  path.Add(new EnhancedWaypoint(new Geopoint(point1), WaypointKind.Stop));
  path.Add(new EnhancedWaypoint(new Geopoint(point2), WaypointKind.Via));
  path.Add(new EnhancedWaypoint(new Geopoint(point3), WaypointKind.Stop));

  MapRouteFinderResult routeResult =  await MapRouteFinder.GetDrivingRouteFromEnhancedWaypointsAsync(path);

  if (routeResult.Status == MapRouteFinderStatus.Success)
  {
      MapRouteView viewOfRoute = new MapRouteView(routeResult.Route);
      viewOfRoute.RouteColor = Colors.Yellow;
      viewOfRoute.OutlineColor = Colors.Black;

      myMap.Routes.Add(viewOfRoute);

      await myMap.TrySetViewBoundsAsync(
            routeResult.Route.BoundingBox,
            null,
            Windows.UI.Xaml.Controls.Maps.MapAnimationKind.None);
  }
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Espace partenaires Bing Cartes](https://www.bingmapsportal.com/)
* [Exemple de carte UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [Recommandations de conception pour les cartes](https://docs.microsoft.com/windows/uwp/maps-and-location/controls-map)
* [Vidéos de la build 2015 : utilisation de cartes et de localisation sur un téléphone, une tablette et un PC dans vos applications Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Exemple d’application de trafic UWP](https://github.com/Microsoft/Windows-appsample-trafficapp)
