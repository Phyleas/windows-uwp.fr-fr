---
title: Afficher des centres d’intérêt sur une carte
description: Ajoutez des points d’intérêt à une carte à l’aide des punaises, des images, des formes et des éléments d’interface utilisateur XAML.
ms.assetid: CA00D8EB-6C1B-4536-8921-5EAEB9B04FCA
ms.date: 08/11/2017
ms.topic: article
keywords: Windows 10, UWP, carte, emplacement, clics-infos
ms.localizationpriority: medium
ms.openlocfilehash: c27132c0728c85238b80e710c62d2e733ee1dd5d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155803"
---
# <a name="display-points-of-interest-on-a-map"></a>Afficher les points d’intérêt sur une carte

Ajoutez des points d’intérêt à une carte à l’aide des punaises, des images, des formes et des éléments d’interface utilisateur XAML. Un point d’intérêt est un point spécifique sur la carte, qui représente un élément intéressant. Il peut s’agir, par exemple, de l’emplacement d’une entreprise, d’une localité ou d’un ami.

Pour en savoir plus sur l’affichage de la valeur de votre application, téléchargez l’exemple suivant à partir de l’exemple de mappage [Windows-Universal-Samples référentiel](https://github.com/Microsoft/Windows-universal-samples) sur GitHub : [plateforme Windows universelle (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl).

Affichez les clics-infos, les images et les formes sur la carte [**en ajoutant des**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon)objets madingn, [**MapBillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard),  [**MapPolygon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapPolygon)et [**polyligne**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapPolyline) à une collection **mapelements** d’un objet [**MapElementsLayer**](/uwp/api/windows.ui.xaml.controls.maps.mapelementslayer) . Ajoutez ensuite cet objet de couche à la collection **Layers** d’un contrôle Map.

>[!NOTE]
> Dans les versions précédentes, ce guide vous a montré comment ajouter des éléments cartographiques à la collection [**mapelements**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.MapElements) . Bien que vous puissiez toujours utiliser cette approche, vous vous abviendrez de certains des avantages du nouveau modèle de couche de carte. Pour plus d’informations, consultez la section [utilisation des couches](#layers) de ce guide.

Vous pouvez également afficher des éléments d’interface utilisateur XAML tels qu’un [**bouton**](/uwp/api/Windows.UI.Xaml.Controls.Button), un [**HyperlinkButton**](/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton)ou un [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) sur la carte en les ajoutant au [**MapItemsControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapItemsControl) ou en tant qu' [**enfants**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.children) du [**collection MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl).

Si vous avez un grand nombre d’éléments à placer sur la carte, songez à [superposer des images sous forme de vignettes à la carte](overlay-tiled-images.md). Pour afficher les routes sur la carte, consultez [afficher les itinéraires et les directions](routes-and-directions.md)

## <a name="add-a-pushpin"></a>Ajouter un clic-infos

Affichez une image de type punaise, avec du texte facultatif, sur la carte à l' [**aide de la classe**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon) . Vous pouvez accepter l’image par défaut ou fournir une image personnalisée à l’aide de la propriété [**Image**](/uwp/api/windows.ui.xaml.controls.maps.mapicon.image). L’image suivante affiche l’image par défaut pour un élément **MapIcon**, sans qu’aucune valeur ne soit spécifiée pour la propriété [**Title**](/uwp/api/windows.ui.xaml.controls.maps.mapicon.title), avec un titre court, un autre long et un autre très long.

![exemple de classe MapIcon présentant des titres de différentes longueurs.](images/mapctrl-mapicons.png)

L’exemple suivant montre une carte de la ville de Seattle et ajoute un [**avec l'**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon) image par défaut et un titre facultatif pour indiquer l’emplacement de l’aiguille de l’espace. Il centre également la carte sur l’icône et effectue un zoom avant. Pour plus d’informations générales sur l’utilisation du contrôle de carte, voir [Afficher des cartes avec des vues 2D, 3D et Streetside](display-maps.md).

```csharp
public void AddSpaceNeedleIcon()
{
    var MyLandmarks = new List<MapElement>();

    BasicGeoposition snPosition = new BasicGeoposition { Latitude = 47.620, Longitude = -122.349 };
    Geopoint snPoint = new Geopoint(snPosition);

    var spaceNeedleIcon = new MapIcon
    {
        Location = snPoint,
        NormalizedAnchorPoint = new Point(0.5, 1.0),
        ZIndex = 0,
        Title = "Space Needle"
    };

    MyLandmarks.Add(spaceNeedleIcon);

    var LandmarksLayer = new MapElementsLayer
    {
        ZIndex = 1,
        MapElements = MyLandmarks
    };

    myMap.Layers.Add(LandmarksLayer);

    myMap.Center = snPoint;
    myMap.ZoomLevel = 14;

}
```

Cet exemple affiche le point d’intérêt suivant sur la carte (l’image par défaut au centre).

![carte avec l’élément MapIcon](images/displaypoidefault.png)

La ligne de code suivante affiche la [**avec une**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon) image personnalisée enregistrée dans le dossier composants du projet. La propriété [**Image**](/uwp/api/windows.ui.xaml.controls.maps.mapicon.image) de la classe **MapIcon** attend une valeur de type [**RandomAccessStreamReference**](/uwp/api/Windows.Storage.Streams.RandomAccessStreamReference). Ce type nécessite une instruction **using** pour l’espace de noms [**Windows.Storage.Streams**](/uwp/api/Windows.Storage.Streams).

>[!NOTE]
>Si vous utilisez la même image pour plusieurs icônes de mappage, déclarez le [**RandomAccessStreamReference**](/uwp/api/Windows.Storage.Streams.RandomAccessStreamReference) au niveau de la page ou de l’application pour obtenir des performances optimales.

```csharp
    MapIcon1.Image =
        RandomAccessStreamReference.CreateFromUri(new Uri("ms-appx:///Assets/customicon.png"));
```

Gardez ces considérations à l’esprit lorsque vous utilisez [**la classe**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon) :

-   La propriété [**image**](/uwp/api/windows.ui.xaml.controls.maps.mapicon.image) prend en charge une taille d’image maximale de 2048 × 2048 pixels.
-   Par défaut, il n’est pas garanti que l’image de l’icône de carte s’affiche. Cette classe peut être masquée quand elle cache d’autres éléments ou étiquettes sur la carte. Pour le garder visible, définissez la propriété [**CollisionBehaviorDesired**](/uwp/api/windows.ui.xaml.controls.maps.mapicon.collisionbehaviordesired) de l’icône de carte sur [**MapElementCollisionBehavior. RemainVisible**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapElementCollisionBehavior).
-   Il [**n’est pas**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon) garanti que le [**titre**](/uwp/api/windows.ui.xaml.controls.maps.mapicon.title) facultatif de soit affiché. Si vous ne voyez pas le texte, effectuez un zoom arrière en diminuant la valeur de la propriété [**zoomLevel**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.zoomlevel) de [**collection MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl).
-   Quand vous affichez une image [**MapIcon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon) qui pointe vers un emplacement spécifique sur la carte, par exemple une punaise ou une flèche, envisagez d’affecter à la valeur de la propriété [**NormalizedAnchorPoint**](/uwp/api/windows.ui.xaml.controls.maps.mapicon.normalizedanchorpoint) l’emplacement approximatif du pointeur sur l’image. Si vous conservez la valeur par défaut (0, 0) de **NormalizedAnchorPoint**, qui représente le coin supérieur gauche de l’image, des modifications de la propriété [**ZoomLevel**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.zoomlevel) de la carte peuvent laisser l’image pointer vers un autre emplacement.
-   Si vous ne définissez pas explicitement une [altitude](/uwp/api/windows.devices.geolocation.basicgeoposition) et une [AltitudeReferenceSystem](/uwp/api/windows.devices.geolocation.geopoint.AltitudeReferenceSystem) [**, la**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon) sera placée sur l’aire.

## <a name="add-a-3d-pushpin"></a>Ajouter un clic-infos 3D

Vous pouvez ajouter des objets 3D à une carte. Utilisez la classe [MapModel3D](/uwp/api/windows.ui.xaml.controls.maps.mapmodel3d) pour importer un objet 3D à partir d’un fichier [3MF (3D Manufacturing format)](https://3mf.io/specification/) .

Cette image utilise des tasses de café 3D pour marquer les emplacements des cafés dans un voisinage.

![tasses sur les cartes](images/mugs.png)

Le code suivant ajoute une tasse de café à la carte à l’aide de l’importation d’un fichier ba3MF. Pour simplifier les choses, ce code ajoute l’image au centre de la carte, mais votre code ajoutera probablement l’image à un emplacement spécifique.

```csharp
public async void Add3DMapModel()
{
    var mugStreamReference = RandomAccessStreamReference.CreateFromUri
        (new Uri("ms-appx:///Assets/mug.3mf"));

    var myModel = await MapModel3D.CreateFrom3MFAsync(mugStreamReference,
        MapModel3DShadingOption.Smooth);

    myMap.Layers.Add(new MapElementsLayer
    {
       ZIndex = 1,
       MapElements = new List<MapElement>
       {
          new MapElement3D
          {
              Location = myMap.Center,
              Model = myModel,
          },
       },
    });
}
```

## <a name="add-an-image"></a>Ajouter une image

Affichez des images de grande taille en rapport avec des emplacements de carte tels qu’une image d’un restaurant ou d’un repère. À mesure que les utilisateurs effectuent un zoom arrière, l’image est réduite proportionnellement en taille pour permettre à l’utilisateur d’afficher une plus grande partie de la carte. C’est un peu différent d’un ma- [**chose qui marque**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon) un emplacement spécifique, est généralement petit et conserve la même taille que les utilisateurs qui effectuent un zoom avant et arrière sur une carte.

![Image MapBillboard](images/map-billboard.png)

Le code suivant montre les [**MapBillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) présentées dans l’image ci-dessus.

```csharp
public void AddLandmarkPhoto()
{
    // Create MapBillboard.

    RandomAccessStreamReference mapBillboardStreamReference =
        RandomAccessStreamReference.CreateFromUri(new Uri("ms-appx:///Assets/billboard.jpg"));

    var mapBillboard = new MapBillboard(myMap.ActualCamera)
    {
        Location = myMap.Center,
        NormalizedAnchorPoint = new Point(0.5, 1.0),
        Image = mapBillboardStreamReference
    };

    // Add MapBillboard to a layer on the map control.

    var MyLandmarkPhotos = new List<MapElement>();

    MyLandmarkPhotos.Add(mapBillboard);

    var LandmarksPhotoLayer = new MapElementsLayer
    {
        ZIndex = 1,
        MapElements = MyLandmarkPhotos
    };

    myMap.Layers.Add(LandmarksPhotoLayer);
}
```

Il y a trois parties de ce code qui méritent d’examiner un peu plus près : l’image, l’appareil photo de référence et la propriété [**NormalizedAnchorPoint**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.NormalizedAnchorPoint) .

### <a name="image"></a>Image

Cet exemple montre une image personnalisée enregistrée dans le dossier **ressources** du projet. La propriété [**image**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.Image) de [**MapBillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) attend une valeur de type [**RandomAccessStreamReference**](/uwp/api/Windows.Storage.Streams.RandomAccessStreamReference). Ce type nécessite une instruction **using** pour l’espace de noms [**Windows.Storage.Streams**](/uwp/api/Windows.Storage.Streams).

>[!NOTE]
>Si vous utilisez la même image pour plusieurs icônes de mappage, déclarez le [**RandomAccessStreamReference**](/uwp/api/Windows.Storage.Streams.RandomAccessStreamReference) au niveau de la page ou de l’application pour obtenir des performances optimales.

### <a name="reference-camera"></a>Appareil photo de référence

 Étant donné qu’une image [**MapBillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) se met à l’échelle et que le [**zoomLevel**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.ZoomLevel) de la carte change, il est important de [**définir l’emplacement**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.ZoomLevel) où l’image s’affiche à l’échelle 1x normale. Cette position est définie dans l’appareil photo de référence de l' [**MapBillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard)et, pour la définir, vous devrez passer un objet [**MapCamera**](/uwp/api/windows.ui.xaml.controls.maps.mapcamera) dans le constructeur du [**MapBillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard).

 Vous pouvez définir la position de votre choix dans une [**géopoint**](/uwp/api/windows.devices.geolocation.geopoint), puis utiliser ce [**géopoint**](/uwp/api/windows.devices.geolocation.geopoint) pour créer un objet [**MapCamera**](/uwp/api/windows.ui.xaml.controls.maps.mapcamera) .  Toutefois, dans cet exemple, nous utilisons simplement l’objet [**MapCamera**](/uwp/api/windows.ui.xaml.controls.maps.mapcamera) retourné par la propriété [**ActualCamera**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.ActualCamera) du contrôle Map. Il s’agit de la caméra interne de la carte. La position actuelle de cette caméra devient la position de la caméra de référence ; position d’affichage de l’image [**MapBillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) à l’échelle 1x.

 Si votre application donne aux utilisateurs la possibilité d’effectuer un zoom arrière sur la carte, l’image diminue en taille, car la caméra de cartes interne augmente en altitude, tandis que l’image à l’échelle 1x reste fixe à la position de l’appareil photo de référence.

### <a name="normalizedanchorpoint"></a>NormalizedAnchorPoint

Le [**NormalizedAnchorPoint**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.NormalizedAnchorPoint) est le point de l’image qui est ancré à la propriété [**location**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.Location) de [**MapBillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard). Le point 0.5, 1 est le centre en bas de l’image. Étant donné que nous avons défini la propriété [**location**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.Location) de la [**MapBillboard**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) au centre du contrôle de la carte, le centre inférieur de l’image est ancré au centre du contrôle Maps. Si vous souhaitez que votre image apparaisse centrée directement sur un point, définissez [**NormalizedAnchorPoint**](/uwp/api/windows.ui.xaml.controls.maps.mapbillboard.NormalizedAnchorPoint) sur 0,5, 0.5.  

## <a name="add-a-shape"></a>Ajouter une forme

Utilisez la classe [**MapPolygon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapPolygon) pour afficher une forme multipoint sur la carte. L’exemple suivant, tiré de l’[exemple de carte UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl), affiche une zone rouge bordée de bleu sur la carte.

```csharp
public void HighlightArea()
{
    // Create MapPolygon.

    double centerLatitude = myMap.Center.Position.Latitude;
    double centerLongitude = myMap.Center.Position.Longitude;

    var mapPolygon = new MapPolygon
    {
        Path = new Geopath(new List<BasicGeoposition> {
                    new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude-0.001 },
                    new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude-0.001 },
                    new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude+0.001 },
                    new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude+0.001 },
                }),
        ZIndex = 1,
        FillColor = Colors.Red,
        StrokeColor = Colors.Blue,
        StrokeThickness = 3,
        StrokeDashed = false,
    };

    // Add MapPolygon to a layer on the map control.
    var MyHighlights = new List<MapElement>();

    MyHighlights.Add(mapPolygon);

    var HighlightsLayer = new MapElementsLayer
    {
        ZIndex = 1,
        MapElements = MyHighlights
    };

    myMap.Layers.Add(HighlightsLayer);
}
```

## <a name="add-a-line"></a>Ajouter une ligne


Affichez une ligne sur la carte à l’aide de la classe [**polyligne**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapPolyline) . L’exemple suivant, tiré de l’[exemple de carte UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl), affiche une ligne en pointillé sur la carte.

```csharp
public void DrawLineOnMap()
{
    // Create Polyline.

    double centerLatitude = myMap.Center.Position.Latitude;
    double centerLongitude = myMap.Center.Position.Longitude;
    var mapPolyline = new MapPolyline
    {
        Path = new Geopath(new List<BasicGeoposition> {
                    new BasicGeoposition() {Latitude=centerLatitude-0.0005, Longitude=centerLongitude-0.001 },
                    new BasicGeoposition() {Latitude=centerLatitude+0.0005, Longitude=centerLongitude+0.001 },
                }),
        StrokeColor = Colors.Black,
        StrokeThickness = 3,
        StrokeDashed = true,
    };

   // Add Polyline to a layer on the map control.

   var MyLines = new List<MapElement>();

   MyLines.Add(mapPolyline);

   var LinesLayer = new MapElementsLayer
   {
       ZIndex = 1,
       MapElements = MyLines
   };

   myMap.Layers.Add(LinesLayer);

}
```

## <a name="add-xaml"></a>Ajouter du code XAML

Utilisez du code XAML pour afficher des éléments d’interface utilisateur personnalisés sur la carte. Positionnez le code XAML sur la carte en spécifiant son emplacement et son point d’ancrage normalisé.

-   Appelez [**SetLocation**](/windows/desktop/tablet/icontextnode-setlocation) pour définir l’emplacement du code XAML sur la carte.
-   Définissez l’emplacement relatif sur le XAML qui correspond à l’emplacement spécifié en appelant [**SetNormalizedAnchorPoint**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.setnormalizedanchorpoint).

L’exemple suivant montre une carte de la ville de Seattle et ajoute un contrôle de [**bordure**](/uwp/api/Windows.UI.Xaml.Controls.Border) XAML pour indiquer l’emplacement de l’aiguille de l’espace. Il centre également la carte sur la zone et effectue un zoom avant. Pour plus d’informations générales sur l’utilisation du contrôle de carte, voir [Afficher des cartes avec des vues 2D, 3D et Streetside](display-maps.md).

```csharp
private void displayXAMLButton_Click(object sender, RoutedEventArgs e)
{
   // Specify a known location.
   BasicGeoposition snPosition = new BasicGeoposition { Latitude = 47.620, Longitude = -122.349 };
   Geopoint snPoint = new Geopoint(snPosition);

   // Create a XAML border.
   Border border = new Border
   {
      Height = 100,
      Width = 100,
      BorderBrush = new SolidColorBrush(Windows.UI.Colors.Blue),
      BorderThickness = new Thickness(5),
   };

   // Center the map over the POI.
   MapControl1.Center = snPoint;
   MapControl1.ZoomLevel = 14;

   // Add XAML to the map.
   MapControl1.Children.Add(border);
   MapControl.SetLocation(border, snPoint);
   MapControl.SetNormalizedAnchorPoint(border, new Point(0.5, 0.5));
}
```

Cet exemple affiche une bordure bleue sur la carte.

![Capture d’écran d’un balisage XAML à l’emplacement du point d’intérêt sur la carte](images/displaypoixaml.png)

Les exemples suivants montrent comment ajouter des éléments d’interface utilisateur en code XAML directement dans le balisage XAML de la page à l’aide de la liaison de données. Comme avec d’autres éléments XAML qui affichent du contenu, les [**enfants**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.children) sont la propriété de contenu par défaut de [**collection MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) et ne doivent pas être spécifiés explicitement dans le balisage XAML.

Cet exemple montre comment afficher deux contrôles XAML comme des enfants implicites de [**collection MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl). Ces contrôles apparaissent sur la carte aux emplacements des données liées.

```xml
<maps:MapControl>
    <TextBox Text="Seattle" maps:MapControl.Location="{x:Bind SeattleLocation}"/>
    <TextBox Text="Bellevue" maps:MapControl.Location="{x:Bind BellevueLocation}"/>
</maps:MapControl>
```

Définissez ces emplacements à l’aide des propriétés de votre fichier code-behind.

```csharp
public Geopoint SeattleLocation { get; set; }
public Geopoint BellevueLocation { get; set; }
```

Cet exemple montre comment afficher deux contrôles XAML contenus dans un [**MapItemsControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapItemsControl). Ces contrôles apparaissent sur la carte aux emplacements des données liées.

```xml
<maps:MapControl>
  <maps:MapItemsControl>
    <TextBox Text="Seattle" maps:MapControl.Location="{x:Bind SeattleLocation}"/>
    <TextBox Text="Bellevue" maps:MapControl.Location="{x:Bind BellevueLocation}"/>
  </maps:MapItemsControl>
</maps:MapControl>
```

Cet exemple affiche une collection d’éléments XAML liés à un [**MapItemsControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapItemsControl).

```xml
<maps:MapControl x:Name="MapControl" MapTapped="MapTapped" MapDoubleTapped="MapTapped" MapHolding="MapTapped">
  <maps:MapItemsControl ItemsSource="{x:Bind LandmarkOverlays}">
      <maps:MapItemsControl.ItemTemplate>
          <DataTemplate>
              <StackPanel Background="Black" Tapped ="Overlay_Tapped">
                  <TextBlock maps:MapControl.Location="{Binding Location}" Text="{Binding Title}"
                    maps:MapControl.NormalizedAnchorPoint="0.5,0.5" FontSize="20" Margin="5"/>
              </StackPanel>
          </DataTemplate>
      </maps:MapItemsControl.ItemTemplate>
  </maps:MapItemsControl>
</maps:MapControl>
```

La ``ItemsSource`` propriété de l’exemple ci-dessus est liée à une propriété de type [IList](/dotnet/api/system.collections.ilist) dans le fichier code-behind.

```csharp
public sealed partial class Scenario1 : Page
{
    public IList LandmarkOverlays { get; set; }

    public MyClassConstructor()
    {
         SetLandMarkLocations();
         this.InitializeComponent();   
    }

    private void SetLandMarkLocations()
    {
        LandmarkOverlays = new List<MapElement>();

        var pikePlaceIcon = new MapIcon
        {
            Location = new Geopoint(new BasicGeoposition
            { Latitude = 47.610, Longitude = -122.342 }),
            Title = "Pike Place Market"
        };

        LandmarkOverlays.Add(pikePlaceIcon);

        var SeattleSpaceNeedleIcon = new MapIcon
        {
            Location = new Geopoint(new BasicGeoposition
            { Latitude = 47.6205, Longitude = -122.3493 }),
            Title = "Seattle Space Needle"
        };

        LandmarkOverlays.Add(SeattleSpaceNeedleIcon);
    }
}
```

<a id="layers" />

## <a name="working-with-layers"></a>Utilisation des couches

Les exemples de ce guide ajoutent des éléments à une collection [MapElementLayers](/uwp/api/windows.ui.xaml.controls.maps.mapelementslayer) . Ils montrent ensuite comment ajouter cette collection à la propriété **Layers** du contrôle Map. Dans les versions précédentes, ce guide vous a montré comment ajouter des éléments cartographiques à la collection [**mapelements**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.MapElements) comme suit :

```csharp
var pikePlaceIcon = new MapIcon
{
    Location = new Geopoint(new BasicGeoposition
    { Latitude = 47.610, Longitude = -122.342 }),
    NormalizedAnchorPoint = new Point(0.5, 1.0),
    ZIndex = 0,
    Title = "Pike Place Market"
};

myMap.MapElements.Add(pikePlaceIcon);
```

Bien que vous puissiez toujours utiliser cette approche, vous vous abviendrez de certains des avantages du nouveau modèle de couche de carte. En regroupant vos éléments en couches, vous pouvez manipuler chaque couche indépendamment les unes des autres. Par exemple, chaque couche possède son propre ensemble d’événements, de sorte que vous pouvez répondre à un événement sur une couche particulière et effectuer une action spécifique de cet événement.

En outre, vous pouvez lier XAML directement à un code d' [exécution](/uwp/api/windows.ui.xaml.controls.maps.maplayer). Il s’agit d’une opération que vous ne pouvez pas effectuer à l’aide de la collection [mapelements](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.MapElements)  .

Pour ce faire, vous pouvez utiliser une classe de modèle de vue, un code-behind une page XAML et une page XAML.

### <a name="view-model-class"></a>Classe de modèle d’affichage

```csharp
public class LandmarksViewModel
{
    public ObservableCollection<MapLayer> LandmarkLayer
        { get; } = new ObservableCollection<MapLayer>();

    public LandmarksViewModel()
    {
        var MyElements = new List<MapElement>();

        var pikePlaceIcon = new MapIcon
        {
            Location = new Geopoint(new BasicGeoposition
            { Latitude = 47.610, Longitude = -122.342 }),
            Title = "Pike Place Market"
        };

        MyElements.Add(pikePlaceIcon);

        var LandmarksLayer = new MapElementsLayer
        {
            ZIndex = 1,
            MapElements = MyElements
        };

        LandmarkLayer.Add(LandmarksLayer);         
    }

```

### <a name="code-behind-a-xaml-page"></a>Code-behind d’une page XAML

Connectez la classe de modèle de vue à votre page code-behind.

```csharp
public LandmarksViewModel ViewModel { get; set; }

public myMapPage()
{
    this.InitializeComponent();
    this.ViewModel = new LandmarksViewModel();
}
```

### <a name="xaml-page"></a>Page XAML

Dans votre page XAML, établissez une liaison à la propriété de votre classe de modèle d’affichage qui retourne la couche.

```XML
<maps:MapControl
    x:Name="myMap" TransitFeaturesVisible="False" Loaded="MyMap_Loaded" Grid.Row="2"
    MapServiceToken="Your token" Layers="{x:Bind ViewModel.LandmarkLayer}"/>
```

## <a name="related-topics"></a>Rubriques connexes

* [Espace partenaires Bing Cartes](https://www.bingmapsportal.com/)
* [Exemple de carte UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [Recommandations de conception pour les cartes](./display-maps.md)
* [Vidéos de la build 2015 : utilisation de cartes et de localisation sur un téléphone, une tablette et un PC dans vos applications Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Exemple d’application de trafic UWP](https://github.com/Microsoft/Windows-appsample-trafficapp)
* [**MapIcon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapIcon)
* [**MapPolygon**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapPolygon)
* [**MapPolyline**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapPolyline)