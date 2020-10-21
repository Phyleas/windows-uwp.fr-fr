---
title: Effectuer un géocodage et un géocodage inverse
description: Ce guide vous montre comment convertir des adresses postales en emplacements géographiques (géocodage) et convertir des emplacements géographiques en adresses postales (le géocodage inversé) en appelant les méthodes de la classe MapLocationFinder dans l’espace de noms Windows. services. Maps.
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
ms.date: 10/20/2020
ms.topic: article
keywords: Windows 10, UWP, géocodage, carte, emplacement
ms.localizationpriority: medium
ms.openlocfilehash: 992a9902081f0655885383ef90ea02ed1e79f13a
ms.sourcegitcommit: 7aaf0740a5d3a17ebf9214aa5e5d056924317673
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92297705"
---
# <a name="perform-geocoding-and-reverse-geocoding"></a>Effectuer un géocodage et un géocodage inverse

> [!NOTE]
> [**Collection MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) et les services cartographiques ont une clé d’authentification Maps appelée [**MapServiceToken**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapservicetoken). Pour plus d’informations sur l’obtention et la définition d’une clé d’authentification de cartes, voir [Demander une clé d’authentification de cartes](authentication-key.md).

Ce guide vous montre comment convertir des adresses postales en emplacements géographiques (géocodage) et convertir des emplacements géographiques en adresses postales (le géocodage inversé) en appelant les méthodes de la classe [**MapLocationFinder**](/uwp/api/Windows.Services.Maps.MapLocationFinder) dans l’espace de noms [**Windows. services. Maps**](/uwp/api/Windows.Services.Maps) .

> [!TIP]
> Pour en savoir plus sur l’utilisation de Maps dans votre application, téléchargez l’exemple [collection MapControl](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl) à partir du [référentiel des exemples Windows universels](hhttps://github.com/Microsoft/Windows-universal-samples) sur GitHub.

Les classes impliquées dans le géocodage et le géocodage inversé sont organisées comme suit.

-   La classe [**MapLocationFinder**](/uwp/api/Windows.Services.Maps.MapLocationFinder) contient des méthodes qui gèrent le géocodage ([**FindLocationsAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)) et le géocodage inversé ([**FindLocationsAtAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)).
-   Ces méthodes retournent toutes deux une instance [**MapLocationFinderResult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult) .
-   La propriété [**locations**](/uwp/api/windows.services.maps.maplocationfinderresult.locations) de l' [**MapLocationFinderResult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult) expose une collection d’objets [**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation) . 
-   Les objets [**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation) ont à la fois une propriété [**Address**](/uwp/api/windows.services.maps.maplocation.address) , qui expose un objet [**mapaddress**](/uwp/api/Windows.Services.Maps.MapAddress) représentant une adresse postale, et une propriété [**point**](/uwp/api/windows.services.maps.maplocation.point) , qui expose un objet [**géopoint**](/uwp/api/windows.devices.geolocation.geopoint) représentant un emplacement géographique.

> [!IMPORTANT]
> Vous devez spécifier une clé d’authentification Maps pour pouvoir utiliser les services de mappage. Pour plus d’informations, voir [Demander une clé d’authentification pour Cartes](authentication-key.md).

## <a name="get-a-location-geocode"></a>Obtenir un emplacement (géocode)

Cette section montre comment convertir une adresse postale ou un nom de lieu en emplacement géographique (géocodage).

1.  Appelez l’une des surcharges de la méthode [**FindLocationsAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync) de la classe [**MapLocationFinder**](/uwp/api/Windows.Services.Maps.MapLocationFinder) avec un nom de lieu ou une adresse postale.
2.  La méthode [**FindLocationsAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync) retourne un objet [**MapLocationFinderResult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult) .
3.  Utilisez la propriété [**locations**](/uwp/api/windows.services.maps.maplocationfinderresult.locations) de [**MapLocationFinderResult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult) pour exposer une collection d’objets [**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation) . Il peut y avoir plusieurs objets [**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation) , car le système peut trouver plusieurs emplacements qui correspondent à l’entrée donnée.

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

1.  Appelez la méthode [**FindLocationsAtAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) de la classe [**MapLocationFinder**](/uwp/api/Windows.Services.Maps.MapLocationFinder) .
2.  La méthode [**FindLocationsAtAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) renvoie un objet [**MapLocationFinderResult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult) contenant une collection d’objets [**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation) correspondants.
3.  Utilisez la propriété [**locations**](/uwp/api/windows.services.maps.maplocationfinderresult.locations) de [**MapLocationFinderResult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult) pour exposer une collection d’objets [**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation) . Il peut y avoir plusieurs objets [**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation) , car le système peut trouver plusieurs emplacements qui correspondent à l’entrée donnée.
4.  Accédez aux objets [**mapaddress**](/uwp/api/Windows.Services.Maps.MapAddress) via la propriété [**Address**](/uwp/api/windows.services.maps.maplocation.address) de chaque [**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation).

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
* [Recommandations de conception pour les cartes](./display-maps.md)
* [Vidéo : utilisation des cartes et de l’emplacement sur les téléphones, les tablettes et les PC dans vos applications Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Espace partenaires Bing Cartes](https://www.bingmapsportal.com/)
* [**MapLocationFinder** , classe](/uwp/api/Windows.Services.Maps.MapLocationFinder)
* [Méthode **FindLocationsAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)
* [Méthode **FindLocationsAtAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)
