---
author: anbare
Description: "Découvrez comment épingler une vignette secondaire à partir de votre application UWP."
title: "Épingler les vignettes secondaires"
label: Pin secondary tiles
template: detail.hbs
ms.author: wdg-dev-content
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows10, uwp, vignettes secondaires, épingler, épinglage, démarrage rapide, exemple de code, exemple, secondarytile"
ms.openlocfilehash: 9525dc91ddd40265d7c7fa9ff8e73509ba3029ab
ms.sourcegitcommit: 6396a69aab081f5c7a9a59739c83538616d3b1c7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/30/2017
---
# <a name="pin-secondary-tiles"></a>Épingler les vignettes secondaires
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Cette rubrique vous guide à travers les étapes de création d’une vignette secondaire pour votre application UWP et son épinglage au menu Démarrer.

![Capture d’écran de vignettes secondaires](images/secondarytiles.png)

Pour en savoir plus sur les vignettes secondaires, consultez la [Vue d’ensemble des vignettes secondaires ](tiles-and-notifications-secondary-tiles.md).


## <a name="add-namespace"></a>Ajouter l’espace de noms

Classe Windows.UI.StartScreen namespace includes the SecondaryTile.

```csharp
using Windows.UI.StartScreen;
```


## <a name="initialize-the-secondary-tile"></a>Initialiser la vignette secondaire

Les vignettes secondaires sont composées de plusieurs composants clés...

* **TileId**: un identificateur unique qui vous permet d’identifier la vignette parmi vos autres vignettes secondaires.
* **DisplayName**: nom que vous voulez voir apparaître sur la vignette.
* **Arguments**: arguments que vous voulez transmettre à votre application quand l’utilisateur clique sur votre vignette.
* **Square150x150Logo**: logo requis, affiché sur la vignette de taille moyenne (et vignette redimensionnée en petite taille si aucun petit logo n’est fourni).

Vous **DEVEZ** fournir des valeurs initialisées pour toutes les propriétés mentionnées ci-dessus, faute de quoi vous obtiendrez une exception.

Vous pouvez utiliser plusieurs constructeurs, toutefois, l’utilisation du constructeur prenant en compte tileId, displayName, arguments, square150x150Logo et desiredSize permet de s’assurer que toutes les propriétés requises sont définies.

```csharp
// Construct a unique tile ID, which you will need to use later for updating the tile
string tileId = "City" + zipCode;

// Use a display name you like
string displayName = cityName;

// Provide all the required info in arguments so that when user
// clicks your tile, you can navigate them to the correct content
string arguments = "action=viewCity&zipCode=" + zipCode;

// Initialize the tile with required arguments
SecondaryTile tile = new SecondaryTile(
    tileId,
    displayName,
    arguments,
    new Uri("ms-appx:///Assets/CityTiles/Square150x150Logo.png"),
    TileSize.Default);
```


## <a name="optional-add-support-for-larger-tile-sizes"></a>Facultatif: ajout de la prise en charge de vignettes de plus grande taille

Si vous souhaitez afficher les notifications par vignette enrichies sur votre vignette secondaire, vous souhaiterez probablement permettre à l’utilisateur de redimensionner ses vignettes pour les élargir ou les agrandir, afin qu’ils puissent voir davantage de contenu.

Pour permettre l’élargissement ou l’agrandissement des vignettes de grande taille, vous devez indiquer les éléments *Wide310x150Logo* et *Square310x310Logo *. En outre, vous devez indiquer si possible l’élément *Square71x71Logo* pour la petite taille de vignette (dans le cas contraire nous réduirons la taille de l’élément Square150x150Logo requis pour la petite vignette).

Vous pouvez également indiquer un élément *Square44x44Logo* unique, qui s’affiche si vous le souhaitez dans le coin inférieur droit en présence d’une notification. Si vous n’en n’indiquez pas, l’élément *Square44x44Logo* de votre vignette principale sera utilisé.

```csharp
// Enable wide and large tile sizes
tile.VisualElements.Wide310x150Logo = new Uri("ms-appx:///Assets/CityTiles/Wide310x150Logo.png");
tile.VisualElements.Square310x310Logo = new Uri("ms-appx:///Assets/CityTiles/Square310x310Logo.png");

// Add a small size logo for better looking small tile
tile.VisualElements.Square71x71Logo = new Uri("ms-appx:///Assets/CityTiles/Square71x71Logo.png");

// Add a unique corner logo for the secondary tile
tile.VisualElements.Square44x44Logo = new Uri("ms-appx:///Assets/CityTiles/Square44x44Logo.png");
```


## <a name="optional-enable-showing-the-display-name"></a>Facultatif: permettre l’affichage du nom d’affichage

Par défaut le nom d’affichage NE s’affiche PAS. Pour afficher le nom d’affichage en moyen, large ou grand, ajoutez le code suivant.

```csharp
// Show the display name on all sizes
tile.VisualElements.ShowNameOnSquare150x150Logo = true;
tile.VisualElements.ShowNameOnWide310x150Logo = true;
tile.VisualElements.ShowNameOnSquare310x310Logo = true;
```


## <a name="pin-the-secondary-tile"></a>Épingler la vignette secondaire

Enfin, demandez l’épinglage de la vignette. Notez que cela doit faire l’objet d’un appel à partir d’un thread d’interface utilisateur. Sur le bureau, une boîte de dialogue s’affiche demandant à l’utilisateur à confirmer s’il souhaite épingler la vignette.

> [!IMPORTANT]
> Si vous êtes une application de bureau Windows utilisant le Pont du bureau, vous devez d’abord effectuer une étape supplémentaire comme décrit dans [Épingler à partir de l’application de bureau](tiles-and-notifications-secondary-tiles-desktop-pinning.md)

```csharp
// Pin the tile
bool isPinned = await tile.RequestCreateAsync();

// TODO: Update UI to reflect whether user can now either unpin or pin
```


## <a name="check-if-a-secondary-tile-exists"></a>Vérifier l’existence d’une vignette secondaire

Si l’utilisateur visite une page dans votre application qu’il a déjà épinglée au menu Démarrer, vous pouvez afficher à la place un bouton «Désépingler».

Par conséquent, lorsque vous choisissez le bouton à afficher, vous devez d’abord vérifier si la vignette secondaire est déjà épinglée.

```csharp
// Check if the secondary tile is pinned
bool isPinned = SecondaryTile.Exists(tileId);

// TODO: Update UI to reflect whether user can either unpin or pin
```


## <a name="unpinning-a-secondary-tile"></a>Désépingler une vignette secondaire

Si la vignette est actuellement épinglée, et que l’utilisateur clique sur le bouton désépingler, vous devrez désépingler (supprimer) la vignette.

```csharp
// Initialize a secondary tile with the same tile ID you want removed
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then unpin the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="updating-a-secondary-tile"></a>Mise à jour d’une vignette secondaire

Si vous avez besoin de mettre à jour les logos, le nom d’affichage ou tout autre élément de la vignette secondaire, vous pouvez utiliser *RequestUpdateAsync *.

```csharp
// Initialize a secondary tile with the same tile ID you want to update
SecondaryTile tile = new SecondaryTile(tileId);

// Assign ALL properties, including ones you aren't changing

// And then update it
await tile.UpdateAsync();
```


## <a name="enumerating-all-pinned-secondary-tiles"></a>Énumération de toutes les vignettes secondaires épinglées

Si vous avez besoin de détecter toutes les vignettes épinglées par votre utilisateur, au lieu d’utiliser *SecondaryTile.Exists*, vous pouvez également utiliser *SecondaryTile.FindAllAsync()*.

```csharp
// Get all secondary tiles
var tiles = await SecondaryTile.FindAllAsync();
```


## <a name="send-a-tile-notification"></a>Envoyer une notification par vignette

Pour savoir comment afficher le contenu enrichi sur votre vignette via les notifications par vignette, consultez [Envoyer une notification par vignette locale ](tiles-and-notifications-sending-a-local-tile-notification.md).


## <a name="related"></a>Similaire

* [Vue d’ensemble des vignettes secondaires](tiles-and-notifications-secondary-tiles.md)
* [Instructions relatives aux vignettes secondaires](tiles-and-notifications-secondary-tiles-guidance.md)
* [Ressources de vignette](tiles-and-notifications-app-assets.md)
* [Documentation sur le contenu des vignettes](tiles-and-notifications-create-adaptive-tiles.md)
* [Envoyer une notification par vignette locale](tiles-and-notifications-sending-a-local-tile-notification.md)