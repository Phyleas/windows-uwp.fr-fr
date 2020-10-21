---
title: Superposer des images sous forme de vignettes sur une carte
description: Superposez des images vignette tierces ou personnalisées sur une carte à l’aide de sources vignette. Utilisez des sources vignette pour superposer des informations spécifiques (informations météorologiques, démographiques, sismiques...) ou pour remplacer entièrement la carte par défaut.
ms.assetid: 066BD6E2-C22B-4F5B-AA94-5D6C86A09BDF
ms.date: 10/20/2020
ms.topic: article
keywords: Windows 10, UWP, carte, emplacement, images, superposition
ms.localizationpriority: medium
ms.openlocfilehash: 8d4a8e3f8a8566fbaae64d44fe876808f094bdd5
ms.sourcegitcommit: 7aaf0740a5d3a17ebf9214aa5e5d056924317673
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92297635"
---
# <a name="overlay-tiled-images-on-a-map"></a>Superposer des images sous forme de vignettes sur une carte

> [!NOTE]
> [**Collection MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) et les services cartographiques ont une clé d’authentification Maps appelée [**MapServiceToken**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapservicetoken). Pour plus d’informations sur l’obtention et la définition d’une clé d’authentification de cartes, voir [Demander une clé d’authentification de cartes](authentication-key.md).

Superposez des images vignette tierces ou personnalisées sur une carte à l’aide de sources vignette. Utilisez des sources vignette pour superposer des informations spécifiques (informations météorologiques, démographiques, sismiques...) ou pour remplacer entièrement la carte par défaut.

**Conseil** Pour en savoir plus sur l’utilisation de Maps dans votre application, téléchargez l' [exemple de mappage plateforme Windows universelle (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl) sur GitHub.

<a id="tileintro" />

## <a name="tiled-image-overview"></a>Vue d’ensemble des images sous forme de vignettes

Les différents services de carte (cartes Nokia et cartes Bing, par exemple) découpent les cartes en vignettes de forme carrée, afin de permettre une récupération et un affichage rapides. Ces vignettes sont au format 256 x 256 pixels et font l’objet d’un rendu préalable, selon différents niveaux de détail. De nombreux services tiers fournissent également des données cartographiques divisées en vignettes. Utilisez les sources de vignette pour récupérer des vignettes tierces, ou pour créer vos propres vignettes personnalisées et les superposer sur la carte affichée dans le [**collection MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl).

**Important**    Lorsque vous utilisez des sources de vignettes, vous n’êtes pas obligé d’écrire du code pour demander ou positionner des vignettes individuelles. Le [**collection MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) demande des vignettes lorsqu’il en a besoin. Chaque demande spécifie les coordonnées X et Y et le niveau de zoom de la vignette individuelle. Il vous suffit de spécifier le format de l’URI ou le nom de fichier à utiliser pour récupérer les vignettes dans la propriété **UriFormatString**. Ainsi, vous insérez des paramètres remplaçables dans l’URI ou le nom de fichier de base afin d’indiquer où transmettre les coordonnées X et Y et le niveau de zoom de chaque vignette.

Voici un exemple de la propriété [**UriFormatString**](/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring) pour une classe [**HttpMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource) qui affiche les paramètres remplaçables des coordonnées X et Y et le niveau de zoom.

```syntax
http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}
```

(Les coordonnées X et Y représentent l’emplacement de la vignette individuelle au sein de la carte du monde, au niveau de détail spécifié. Le système de numérotation des vignettes démarre à partir de la valeur {0, 0}, dans le coin supérieur gauche de la carte. Par exemple, la vignette située à {1, 2} se trouve dans la deuxième colonne de la troisième ligne de la grille de vignettes.)

Pour plus d’informations sur le système de vignettes utilisé par les services cartographiques, voir [Système de vignette Cartes Bing](/bingmaps/articles/bing-maps-tile-system).

### <a name="overlay-tiles-from-a-tile-source"></a>Superposer des vignettes à partir d’une source de vignette

Superposer les images en mosaïque d’une source de vignette sur une carte à l’aide de [**MapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileDataSource).

1.  Instanciez l’une des trois classes de source de données de mosaïque qui héritent de [**MapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileDataSource).

    -   [**HttpMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource)
    -   [**LocalMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.LocalMapTileDataSource)
    -   [**CustomMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.CustomMapTileDataSource)

    Configurez le paramètre **UriFormatString** à utiliser pour demander des vignettes en insérant des paramètres remplaçables dans l’URI ou le nom de fichier de base.

    L’exemple suivant instancie une classe [**HttpMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource). Cet exemple spécifie la valeur de la propriété [**UriFormatString**](/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring) dans le constructeur de la classe **HttpMapTileDataSource**.

    ```csharp
        HttpMapTileDataSource dataSource = new HttpMapTileDataSource(
          "http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}");
    ```

2.  Instanciez et configurez une classe [**MapTileSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource). Spécifiez la classe [**MapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileDataSource) que vous avez configurée à l’étape précédente en tant que propriété [**DataSource**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.datasource) de la classe **MapTileSource**.

    L’exemple suivant spécifie la [**source de source**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.datasource) de [**MapTileSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource)dans le constructeur.

    ```csharp
        MapTileSource tileSource = new MapTileSource(dataSource);
    ```

    Vous pouvez limiter les conditions dans lesquelles les vignettes sont affichées à l’aide des propriétés de [**MapTileSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource).

    -   Affichez les vignettes uniquement dans une zone géographique spécifique en affectant une valeur à la propriété [**Bounds**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.bounds).
    -   Affichez les vignettes selon certains niveaux de détail spécifiques en affectant une valeur à la propriété [**ZoomLevelRange**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.zoomlevelrange).

    Éventuellement, configurez d’autres propriétés du [**MapTileSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource) qui affectent le chargement ou l’affichage des vignettes, telles [**que Layer**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.layer), [**AllowOverstretch**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.allowoverstretch), [**IsRetryEnabled**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.isretryenabled)et [**IsTransparencyEnabled**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.istransparencyenabled).

3.  Ajoutez [**MapTileSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource) à la collection [**TileSources**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.tilesources) du [**collection MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl).

    ```csharp
         MapControl1.TileSources.Add(tileSource);
    ```

## <a name="overlay-tiles-from-a-web-service"></a>Superposer des vignettes provenant d’un service web


Superposer les images en mosaïque récupérées à partir d’un service Web à l’aide de [**HttpMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource).

1.  Instanciez une [**HttpMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource).
2.  Spécifiez le format de l’URI que le service web attend, en tant que valeur de la propriété [**UriFormatString**](/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring). Pour créer cette valeur, insérez les paramètres remplaçables dans l’URI de base. Par exemple, dans l’exemple de code suivant, la valeur de l’élément **UriFormatString** est la suivante :

    ``` syntax
    http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}
    ```

    Le service web doit prendre en charge un URI qui contient les paramètres remplaçables {zoomlevel}, {x} et {y}. La plupart des services web (par exemple ceux de Nokia, Bing et Google) prennent en charge les URI dans ce format. Si le service web requiert des arguments supplémentaires qui ne sont pas disponibles avec la propriété [**UriFormatString**](/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring), vous devez créer un URI personnalisé. Créez et renvoyez un URI personnalisé en gérant l’événement [**UriRequested**](/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.urirequested). Pour plus d’informations, voir la section [Fournir un URI personnalisé](#customuri) plus loin dans cette rubrique.

3.  Suivez ensuite les étapes restantes décrites précédemment dans [Vue d’ensemble des images sous forme de vignettes](#tileintro).

L’exemple suivant illustre la superposition de vignettes provenant d’un service web fictif sur une carte représentant l’Amérique du Nord. La valeur de [**UriFormatString**](/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring) est spécifiée dans le constructeur du [**HttpMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource). Dans cet exemple, les vignettes sont uniquement affichées dans les limites géographiques spécifiées par la propriété facultative [**Bounds**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.bounds).

```csharp
private void AddHttpMapTileSource()
{
    // Create the bounding box in which the tiles are displayed.
    // This example represents North America.
    BasicGeoposition northWestCorner =
        new BasicGeoposition() { Latitude = 48.38544, Longitude = -124.667360 };
    BasicGeoposition southEastCorner =
        new BasicGeoposition() { Latitude = 25.26954, Longitude = -80.30182 };
    GeoboundingBox boundingBox = new GeoboundingBox(northWestCorner, southEastCorner);

    // Create an HTTP data source.
    // This example retrieves tiles from a fictitious web service.
    HttpMapTileDataSource dataSource = new HttpMapTileDataSource(
        "http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}");

    // Optionally, add custom HTTP headers if the web service requires them.
    dataSource.AdditionalRequestHeaders.Add("header name", "header value");

    // Create a tile source and add it to the Map control.
    MapTileSource tileSource = new MapTileSource(dataSource);
    tileSource.Bounds = boundingBox;
    MapControl1.TileSources.Add(tileSource);
}
```

```cppwinrt
...
#include <winrt/Windows.Devices.Geolocation.h>
#include <winrt/Windows.UI.Xaml.Controls.Maps.h>
...
void MainPage::AddHttpMapTileSource()
{
    Windows::Devices::Geolocation::BasicGeoposition northWest{ 48.38544, -124.667360 };
    Windows::Devices::Geolocation::BasicGeoposition southEast{ 25.26954, -80.30182 };
    Windows::Devices::Geolocation::GeoboundingBox boundingBox{ northWest, southEast };

    Windows::UI::Xaml::Controls::Maps::HttpMapTileDataSource dataSource{
        L"http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}" };

    dataSource.AdditionalRequestHeaders().Insert(L"header name", L"header value");

    Windows::UI::Xaml::Controls::Maps::MapTileSource tileSource{ dataSource };
    tileSource.Bounds(boundingBox);

    MapControl1().TileSources().Append(tileSource);
}
...
```

```cpp
void MainPage::AddHttpMapTileSource()
{
    BasicGeoposition northWest = { 48.38544, -124.667360 };
    BasicGeoposition southEast = { 25.26954, -80.30182 };
    GeoboundingBox^ boundingBox = ref new GeoboundingBox(northWest, southEast);

    auto dataSource = ref new Windows::UI::Xaml::Controls::Maps::HttpMapTileDataSource(
        "http://www.<web service name>.com/z={zoomlevel}&x={x}&y={y}");

    dataSource->AdditionalRequestHeaders->Insert("header name", "header value");

    auto tileSource = ref new Windows::UI::Xaml::Controls::Maps::MapTileSource(dataSource);
    tileSource->Bounds = boundingBox;

    this->MapControl1->TileSources->Append(tileSource);
}
```

## <a name="overlay-tiles-from-local-storage"></a>Superposer des vignettes provenant d’un stockage local


Superposez des images sous forme de vignettes stockées en tant que fichiers dans le stockage local, à l’aide de la [**LocalMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.LocalMapTileDataSource). En règle générale, vous placez ces fichiers dans un package, puis vous les distribuez avec votre application.

1.  Instanciez un [**LocalMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.LocalMapTileDataSource).
2.  Spécifiez le format des noms de fichier en tant que valeur de la propriété [**UriFormatString**](/uwp/api/windows.ui.xaml.controls.maps.localmaptiledatasource.uriformatstring). Pour créer cette valeur, insérez les paramètres remplaçables dans le nom de fichier de base. Par exemple, dans l’exemple de code suivant, la valeur de l’élément [**UriFormatString**](/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring) est la suivante :

    ``` syntax
        Tile_{zoomlevel}_{x}_{y}.png
    ```

    Si le format de nom de fichiers nécessite des arguments supplémentaires qui ne sont pas fournis avec la propriété [**UriFormatString**](/uwp/api/windows.ui.xaml.controls.maps.localmaptiledatasource.uriformatstring), vous devez créer un URI personnalisé. Créez et renvoyez un URI personnalisé en gérant l’événement [**UriRequested**](/uwp/api/windows.ui.xaml.controls.maps.localmaptiledatasource.urirequested). Pour plus d’informations, voir la section [Fournir un URI personnalisé](#customuri) plus loin dans cette rubrique.

3.  Suivez ensuite les étapes restantes décrites précédemment dans [Vue d’ensemble des images sous forme de vignettes](#tileintro).

Vous pouvez utiliser les protocoles et les emplacements suivants pour charger les vignettes à partir du stockage local :

| Uri | En savoir plus |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| ms-appx:/// | Pointe vers la racine du dossier d’installation de l’application. |
|  | Il s’agit de l’emplacement référencé par la propriété [Package.InstalledLocation](/uwp/api/windows.applicationmodel.package.installedlocation). |
| ms-appdata:///local | Pointe vers la racine du stockage local de l’application. |
|  | Il s’agit de l’emplacement référencé par la propriété [ApplicationData.LocalFolder](/uwp/api/windows.storage.applicationdata.localfolder). |
| ms-appdata:///temp | Pointe vers le dossier temporaire de l’application. |
|  | Il s’agit de l’emplacement référencé par la propriété [ApplicationData.TemporaryFolder](/uwp/api/windows.storage.applicationdata.temporaryfolder). |

 

L’exemple suivant illustre le chargement de vignettes stockées en tant que fichiers dans le dossier d’installation de l’application, via le protocole `ms-appx:///`. La valeur de la propriété [**UriFormatString**](/uwp/api/windows.ui.xaml.controls.maps.localmaptiledatasource.uriformatstring) est spécifiée dans le constructeur de la classe [**LocalMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.LocalMapTileDataSource). Dans cet exemple, les vignettes s’affichent uniquement lorsque le niveau de zoom de la carte est compris dans la plage spécifiée par la propriété facultative [**ZoomLevelRange**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.zoomlevelrange) .

```csharp
        void AddLocalMapTileSource()
        {
            // Specify the range of zoom levels
            // at which the overlaid tiles are displayed.
            MapZoomLevelRange range;
            range.Min = 11;
            range.Max = 20;

            // Create a local data source.
            LocalMapTileDataSource dataSource = new LocalMapTileDataSource(
                "ms-appx:///TileSourceAssets/Tile_{zoomlevel}_{x}_{y}.png");

            // Create a tile source and add it to the Map control.
            MapTileSource tileSource = new MapTileSource(dataSource);
            tileSource.ZoomLevelRange = range;
            MapControl1.TileSources.Add(tileSource);
        }
```

<a id="customuri" />

## <a name="provide-a-custom-uri"></a>Fournir un URI personnalisé

Si les paramètres remplaçables disponibles avec la propriété [**UriFormatString**](/uwp/api/windows.ui.xaml.controls.maps.httpmaptiledatasource.uriformatstring) de [**HttpMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.HttpMapTileDataSource) ou la propriété [**UriFormatString**](/uwp/api/windows.ui.xaml.controls.maps.localmaptiledatasource.uriformatstring) de l' [**LocalMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.LocalMapTileDataSource) ne sont pas suffisants pour récupérer vos vignettes, vous devez créer un URI personnalisé. Créez et renvoyez un URI personnalisé en proposant un gestionnaire personnalisé pour l’événement **UriRequested**. L’événement **UriRequested** est déclenché pour chaque vignette individuelle.

1.  Dans le gestionnaire personnalisé de l’événement **UriRequested**, combinez les arguments personnalisés requis avec les propriétés [**X**](/uwp/api/windows.ui.xaml.controls.maps.maptileurirequestedeventargs.x), [**Y**](/uwp/api/windows.ui.xaml.controls.maps.maptileurirequestedeventargs.y)et [**ZoomLevel**](/uwp/api/windows.ui.xaml.controls.maps.maptileurirequestedeventargs.zoomlevel) de la classe [**MapTileUriRequestedEventArgs**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileUriRequestedEventArgs) pour créer l’URI personnalisé.
2.  Renvoyez l’URI personnalisé dans la propriété [**Uri**](/uwp/api/windows.ui.xaml.controls.maps.maptileurirequest.uri) de la classe [**MapTileUriRequest**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileUriRequest), qui est contenue dans la propriété [**Request**](/uwp/api/windows.ui.xaml.controls.maps.maptileurirequestedeventargs.request) de la classe [**MapTileUriRequestedEventArgs**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileUriRequestedEventArgs).

L’exemple suivant explique comment fournir un URI personnalisé en créant un gestionnaire personnalisé pour l’événement **UriRequested**. Il indique également comment implémenter le modèle de report si vous devez exécuter une action asynchrone pour créer l’URI personnalisé.

```csharp
using Windows.UI.Xaml.Controls.Maps;
using System.Threading.Tasks;
...
            var httpTileDataSource = new HttpMapTileDataSource();
            // Attach a handler for the UriRequested event.
            httpTileDataSource.UriRequested += HandleUriRequestAsync;
            MapTileSource httpTileSource = new MapTileSource(httpTileDataSource);
            MapControl1.TileSources.Add(httpTileSource);
...
        // Handle the UriRequested event.
        private async void HandleUriRequestAsync(HttpMapTileDataSource sender,
            MapTileUriRequestedEventArgs args)
        {
            // Get a deferral to do something asynchronously.
            // Omit this line if you don't have to do something asynchronously.
            var deferral = args.Request.GetDeferral();

            // Get the custom Uri.
            var uri = await GetCustomUriAsync(args.X, args.Y, args.ZoomLevel);

            // Specify the Uri in the Uri property of the MapTileUriRequest.
            args.Request.Uri = uri;

            // Notify the app that the custom Uri is ready.
            // Omit this line also if you don't have to do something asynchronously.
            deferral.Complete();
        }

        // Create the custom Uri.
        private async Task<Uri> GetCustomUriAsync(int x, int y, int zoomLevel)
        {
            // Do something asynchronously to create and return the custom Uri.        }
        }
```

## <a name="overlay-tiles-from-a-custom-source"></a>Superposer des vignettes provenant d’une source personnalisée

Superposer des vignettes personnalisées à l’aide de [**CustomMapTileDataSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.CustomMapTileDataSource). Créez des vignettes par programme en mémoire, à la volée, ou écrivez votre propre code pour charger les vignettes existantes à partir d’une autre source.

Pour créer ou charger des vignettes personnalisées, fournissez un gestionnaire personnalisé pour l’événement [**BitmapRequested**](/uwp/api/windows.ui.xaml.controls.maps.custommaptiledatasource.bitmaprequested) . L’événement **BitmapRequested** est déclenché pour chaque vignette individuelle.

1.  Dans le gestionnaire personnalisé de l’événement [**BitmapRequested**](/uwp/api/windows.ui.xaml.controls.maps.custommaptiledatasource.bitmaprequested), combinez les arguments personnalisés requis avec les propriétés [**X**](/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.x), [**Y**](/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.y) et [**ZoomLevel**](/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.zoomlevel) de la classe [**MapTileBitmapRequestedEventArgs**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileBitmapRequestedEventArgs) pour créer ou récupérer une vignette personnalisée.
2.  Retournez la vignette personnalisée dans la propriété [**PixelData**](/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequest.pixeldata) de [**MapTileBitmapRequest**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileBitmapRequest), qui est contenue dans la propriété [**Request**](/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.request) de [**MapTileBitmapRequestedEventArgs**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileBitmapRequestedEventArgs). La propriété **PixelData** présente le type [**IRandomAccessStreamReference**](/uwp/api/Windows.Storage.Streams.IRandomAccessStreamReference).

L’exemple suivant indique comment fournir des vignettes personnalisées en créant un gestionnaire personnalisé pour l’événement **BitmapRequested**. Cet exemple permet de créer des vignettes rouges identiques, partiellement opaques. L’exemple ignore les propriétés [**X**](/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.x), [**Y**](/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.y)et [**zoomLevel**](/uwp/api/windows.ui.xaml.controls.maps.maptilebitmaprequestedeventargs.zoomlevel) du [**MapTileBitmapRequestedEventArgs**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileBitmapRequestedEventArgs). Cet exemple n’est pas concret, mais il indique comment créer des vignettes personnalisées en mémoire, à la volée. Il indique également comment implémenter le modèle de report si vous devez exécuter une action asynchrone pour créer les vignettes personnalisées.

```csharp
using Windows.UI.Xaml.Controls.Maps;
using Windows.Storage.Streams;
using System.Threading.Tasks;
...
        CustomMapTileDataSource customDataSource = new CustomMapTileDataSource();
        // Attach a handler for the BitmapRequested event.
        customDataSource.BitmapRequested += customDataSource_BitmapRequestedAsync;
        customTileSource = new MapTileSource(customDataSource);
        MapControl1.TileSources.Add(customTileSource);
...
        // Handle the BitmapRequested event.
        private async void customDataSource_BitmapRequestedAsync(
            CustomMapTileDataSource sender,
            MapTileBitmapRequestedEventArgs args)
        {
            var deferral = args.Request.GetDeferral();
            args.Request.PixelData = await CreateBitmapAsStreamAsync();
            deferral.Complete();
        }

        // Create the custom tiles.
        // This example creates red tiles that are partially opaque.
        private async Task<RandomAccessStreamReference> CreateBitmapAsStreamAsync()
        {
            int pixelHeight = 256;
            int pixelWidth = 256;
            int bpp = 4;

            byte[] bytes = new byte[pixelHeight * pixelWidth * bpp];

            for (int y = 0; y < pixelHeight; y++)
            {
                for (int x = 0; x < pixelWidth; x++)
                {
                    int pixelIndex = y * pixelWidth + x;
                    int byteIndex = pixelIndex * bpp;

                    // Set the current pixel bytes.
                    bytes[byteIndex] = 0xff;        // Red
                    bytes[byteIndex + 1] = 0x00;    // Green
                    bytes[byteIndex + 2] = 0x00;    // Blue
                    bytes[byteIndex + 3] = 0x80;    // Alpha (0xff = fully opaque)
                }
            }

            // Create RandomAccessStream from byte array.
            InMemoryRandomAccessStream randomAccessStream =
                new InMemoryRandomAccessStream();
            IOutputStream outputStream = randomAccessStream.GetOutputStreamAt(0);
            DataWriter writer = new DataWriter(outputStream);
            writer.WriteBytes(bytes);
            await writer.StoreAsync();
            await writer.FlushAsync();
            return RandomAccessStreamReference.CreateFromStream(randomAccessStream);
        }
```

```cppwinrt
...
#include <winrt/Windows.Storage.Streams.h>
...
Windows::Foundation::IAsyncOperation<Windows::Storage::Streams::InMemoryRandomAccessStream> MainPage::CustomRandomAccessStream()
{
    constexpr int pixelHeight{ 256 };
    constexpr int pixelWidth{ 256 };
    constexpr int bpp{ 4 };

    std::array<uint8_t, pixelHeight * pixelWidth * bpp> bytes;

    for (int y = 0; y < pixelHeight; y++)
    {
        for (int x = 0; x < pixelWidth; x++)
        {
            int pixelIndex{ y * pixelWidth + x };
            int byteIndex{ pixelIndex * bpp };

            // Set the current pixel bytes.
            bytes[byteIndex] = (byte)(std::rand() % 256);        // Red
            bytes[byteIndex + 1] = (byte)(std::rand() % 256);    // Green
            bytes[byteIndex + 2] = (byte)(std::rand() % 256);    // Blue
            bytes[byteIndex + 3] = (byte)((std::rand() % 56) + 200);    // Alpha (0xff = fully opaque)
        }
    }

    // Create RandomAccessStream from byte array.
    Windows::Storage::Streams::InMemoryRandomAccessStream randomAccessStream;
    Windows::Storage::Streams::IOutputStream outputStream{ randomAccessStream.GetOutputStreamAt(0) };
    Windows::Storage::Streams::DataWriter writer{ outputStream };
    writer.WriteBytes(bytes);

    co_await writer.StoreAsync();
    co_await writer.FlushAsync();

    co_return randomAccessStream;
}
...
```

```cpp
InMemoryRandomAccessStream^ TileSources::CustomRandomAccessStream::get()
{
    int pixelHeight = 256;
    int pixelWidth = 256;
    int bpp = 4;

    Array<byte>^ bytes = ref new Array<byte>(pixelHeight * pixelWidth * bpp);

    for (int y = 0; y < pixelHeight; y++)
    {
        for (int x = 0; x < pixelWidth; x++)
        {
            int pixelIndex = y * pixelWidth + x;
            int byteIndex = pixelIndex * bpp;

            // Set the current pixel bytes.
            bytes[byteIndex] = (byte)(std::rand() % 256);        // Red
            bytes[byteIndex + 1] = (byte)(std::rand() % 256);    // Green
            bytes[byteIndex + 2] = (byte)(std::rand() % 256);    // Blue
            bytes[byteIndex + 3] = (byte)((std::rand() % 56) + 200);    // Alpha (0xff = fully opaque)
        }
    }

    // Create RandomAccessStream from byte array.
    InMemoryRandomAccessStream^ randomAccessStream = ref new InMemoryRandomAccessStream();
    IOutputStream^ outputStream = randomAccessStream->GetOutputStreamAt(0);
    DataWriter^ writer = ref new DataWriter(outputStream);
    writer->WriteBytes(bytes);

    create_task(writer->StoreAsync()).then([writer](unsigned int)
    {
        create_task(writer->FlushAsync());
    });

    return randomAccessStream;
}
```

## <a name="replace-the-default-map"></a>Remplacer la carte par défaut

Pour remplacer entièrement la carte par défaut par des vignettes personnalisées ou tierces, procédez comme suit :

-   Spécifiez [**MapTileLayer**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileLayer).**BackgroundReplacement** en tant que valeur de la propriété [**Layer**](/uwp/api/windows.ui.xaml.controls.maps.maptilesource.layer) de la classe [**MapTileSource**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapTileSource).
-   Spécifiez [**MapStyle**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapStyle).**None** en tant que valeur de la propriété [**Style**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.style) de la classe [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl).

## <a name="related-topics"></a>Rubriques connexes

* [Espace partenaires Bing Cartes](https://www.bingmapsportal.com/)
* [Exemple de carte UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [Recommandations de conception pour les cartes](./display-maps.md)
* [Vidéos de la build 2015 : utilisation de cartes et de localisation sur un téléphone, une tablette et un PC dans vos applications Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Exemple d’application de trafic UWP](https://github.com/Microsoft/Windows-appsample-trafficapp)
