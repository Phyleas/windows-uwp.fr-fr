---
title: Effectuer un géocodage et un géocodage inverse
description: Ce guide vous montre comment convertir des adresses postales en emplacements géographiques (géocodage) et convertir des emplacements géographiques en adresses postales (le géocodage inversé) en appelant les méthodes de la classe MapLocationFinder dans l’espace de noms Windows. services. Maps.
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
ms.date: 07/02/2018
ms.topic: article
keywords: windows 10, uwp, géocodage, carte, emplacement
ms.localizationpriority: medium
ms.openlocfilehash: 5d7e1dda355cf87a2c8e26c11327cfff32e9d0b5
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259370"
---
# <a name="perform-geocoding-and-reverse-geocoding"></a>Effectuer un géocodage et un géocodage inverse

Ce guide vous montre comment convertir des adresses postales en emplacements géographiques (géocodage) et convertir des emplacements géographiques en adresses postales (le géocodage inversé) en appelant les méthodes de la classe [**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) dans l’espace de noms [**Windows. services. Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps) .

> [!TIP]
> Pour en savoir plus sur l’utilisation de Maps dans votre application, téléchargez l’exemple [collection MapControl](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl) à partir du [référentiel des exemples Windows universels](h https://github.com/Microsoft/Windows-universal-samples) sur GitHub.

Les classes impliquées dans le géocodage et le géocodage inversé sont organisées comme suit.

-   La classe [**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) contient des méthodes qui gèrent le géocodage ([**FindLocationsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)) et le géocodage inversé ([**FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)).
-   Ces méthodes retournent toutes deux une instance [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) .
-   La propriété [**locations**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations) de l' [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) expose une collection d’objets [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) . 
-   Les objets [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) ont à la fois une propriété [**Address**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.address) , qui expose un objet [**mapaddress**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapAddress) représentant une adresse postale, et une propriété [**point**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.point) , qui expose un objet [**géopoint**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint) représentant un emplacement géographique.

> [!IMPORTANT]
> vous devez spécifier une clé d’authentification Maps pour pouvoir utiliser les services de mappage. Pour plus d’informations, voir [Demander une clé d’authentification pour Cartes](authentication-key.md).

## <a name="get-a-location-geocode"></a>Obtenir un emplacement (géocode)

Cette section montre comment convertir une adresse postale ou un nom de lieu en emplacement géographique (géocodage).

1.  Appelez l’une des surcharges de la méthode [**FindLocationsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync) de la classe [**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) avec un nom de lieu ou une adresse postale.
2.  La méthode [**FindLocationsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync) retourne un objet [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) .
3.  Utilisez la propriété [**locations**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations) de [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) pour exposer une collection d’objets [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) . Il peut y avoir plusieurs objets [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) , car le système peut trouver plusieurs emplacements qui correspondent à l’entrée donnée.

```csharp
using Windows.Services.Maps;
using Windows.Devices.Geolocation;
...
private async void geocodeButton_Click(object sender, RoutedEventArgs e)
{
   // The address or business to geocode.
   string addressToGeocode = "Microsoft";

   // The nearby location to use as a query hint.
   BasicGeoposition queryHint = new BasicGeoposition();
   queryHint.Latitude = 47.643;
   queryHint.Longitude = -122.131;
   Geopoint hintPoint = new Geopoint(queryHint);

   // Geocode the specified address, using the specified reference point
   // as a query hint. Return no more than 3 results.
   MapLocationFinderResult result =
         await MapLocationFinder.FindLocationsAsync(
                           addressToGeocode,
                           hintPoint,
                           3);

   // If the query returns results, display the coordinates
   // of the first result.
   if (result.Status == MapLocationFinderStatus.Success)
   {
      tbOutputText.Text = "result = (" +
            result.Locations[0].Point.Position.Latitude.ToString() + "," +
            result.Locations[0].Point.Position.Longitude.ToString() + ")";
   }
}
```

Ce code affiche les résultats suivants dans la zone de texte `tbOutputText`.

``` syntax
result = (47.6406099647284,-122.129339994863)
```

## <a name="get-an-address-reverse-geocode"></a>Obtenir une adresse (géocode inverse)

Cette section montre comment convertir un emplacement géographique en adresse (le géocodage inversé).

1.  Appelez la méthode [**FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) de la classe [**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder).
2.  La méthode [**FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) renvoie un objet [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) contenant une collection d’objets [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) correspondants.
3.  Utilisez la propriété [**locations**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations) de [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) pour exposer une collection d’objets [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) . Il peut y avoir plusieurs objets [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) , car le système peut trouver plusieurs emplacements qui correspondent à l’entrée donnée.
4.  Accédez aux objets [**mapaddress**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapAddress) via la propriété [**Address**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.address) de chaque [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation).

```csharp
using Windows.Services.Maps;
using Windows.Devices.Geolocation;
...
private async void reverseGeocodeButton_Click(object sender, RoutedEventArgs e)
{
   // The location to reverse geocode.
   BasicGeoposition location = new BasicGeoposition();
   location.Latitude = 47.643;
   location.Longitude = -122.131;
   Geopoint pointToReverseGeocode = new Geopoint(location);

   // Reverse geocode the specified geographic location.
   MapLocationFinderResult result =
         await MapLocationFinder.FindLocationsAtAsync(pointToReverseGeocode);

   // If the query returns results, display the name of the town
   // contained in the address of the first result.
   if (result.Status == MapLocationFinderStatus.Success)
   {
      tbOutputText.Text = "town = " +
            result.Locations[0].Address.Town;
   }
}
```

Ce code affiche les résultats suivants dans la zone de texte `tbOutputText`.

``` syntax
town = Redmond
```

## <a name="related-topics"></a>Rubriques connexes

* [Exemple de carte UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [Exemple d’application de trafic UWP](https://github.com/Microsoft/Windows-appsample-trafficapp)
* [Recommandations de conception pour les cartes](https://docs.microsoft.com/windows/uwp/maps-and-location/controls-map)
* [Vidéo : utilisation des cartes et de l’emplacement sur les téléphones, les tablettes et les PC dans vos applications Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Espace partenaires Bing Cartes](https://www.bingmapsportal.com/)
* [**MapLocationFinder** , classe](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder)
* [Méthode **FindLocationsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)
* [Méthode **FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)
