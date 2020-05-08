---
Description: Découvrez comment épingler une vignette secondaire pour démarrer à partir de votre application Windows.
title: Épingler les vignettes secondaires au début
label: Pin secondary tiles to Start
template: detail.hbs
ms.date: 05/25/2017
ms.topic: article
keywords: Windows 10, UWP, vignettes secondaires, code confidentiel, épinglage, démarrage rapide, exemple de code, exemple, secondarytile
ms.localizationpriority: medium
ms.openlocfilehash: 8c535e7e2abaf68212cb0a2f6daac8741b6548a5
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82971044"
---
# <a name="pin-secondary-tiles-to-start"></a>Épingler les vignettes secondaires au début


Cette rubrique vous guide tout au long des étapes de création d’une vignette secondaire pour votre application Windows et de l’épingler au menu Démarrer.

![Capture d’écran des vignettes secondaires](images/secondarytiles.png)

Pour en savoir plus sur les vignettes secondaires, consultez la [vue d’ensemble des vignettes secondaires](secondary-tiles.md).


## <a name="add-namespace"></a>Ajouter un espace de noms

L’espace de noms Windows. UI. écran comprend la classe SecondaryTile.

```csharp
using Windows.UI.StartScreen;
```


## <a name="initialize-the-secondary-tile"></a>Initialiser la vignette secondaire

Les vignettes secondaires sont composées de quelques composants clés...

* **TileId**: identificateur unique qui vous permet d’identifier la vignette parmi les autres vignettes secondaires.
* **DisplayName**: nom que vous souhaitez afficher sur la vignette.
* **Arguments**: les arguments que vous souhaitez repasser à votre application lorsque l’utilisateur clique sur votre vignette.
* **Square150x150Logo**: le logo requis, affiché sur la vignette taille moyenne (et redimensionné en mosaïque de petite taille si aucun petit logo n’est fourni).

Vous **devez** fournir des valeurs initialisées pour toutes les propriétés ci-dessus, sinon vous obtiendrez une exception.

Vous pouvez utiliser un certain nombre de constructeurs, mais l’utilisation du constructeur qui prend tileId, displayName, arguments, square150x150Logo et desiredSize permet de garantir la définition de toutes les propriétés requises.

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


## <a name="optional-add-support-for-larger-tile-sizes"></a>Facultatif : ajouter la prise en charge des tailles de vignettes plus grandes

Si vous envisagez d’afficher des notifications de vignette enrichies sur votre vignette secondaire, vous souhaiterez probablement autoriser l’utilisateur à redimensionner sa vignette pour qu’elle soit large ou grande, afin de pouvoir voir encore plus de contenu.

Pour activer des tailles de vignette larges et volumineuses, vous devez fournir les *Wide310x150Logo* et *Square310x310Logo*. En outre, si possible, vous devez fournir le *Square71x71Logo* pour la petite taille de vignette (sinon, nous allons réduire le Square150x150Logo requis pour la petite vignette).

Vous pouvez également fournir un *Square44x44Logo*unique, qui est éventuellement affiché dans le coin inférieur droit lorsqu’une notification est présente. Si vous n’en fournissez pas, les *Square44x44Logo* de votre vignette principale seront utilisés à la place.

```csharp
// Enable wide and large tile sizes
tile.VisualElements.Wide310x150Logo = new Uri("ms-appx:///Assets/CityTiles/Wide310x150Logo.png");
tile.VisualElements.Square310x310Logo = new Uri("ms-appx:///Assets/CityTiles/Square310x310Logo.png");

// Add a small size logo for better looking small tile
tile.VisualElements.Square71x71Logo = new Uri("ms-appx:///Assets/CityTiles/Square71x71Logo.png");

// Add a unique corner logo for the secondary tile
tile.VisualElements.Square44x44Logo = new Uri("ms-appx:///Assets/CityTiles/Square44x44Logo.png");
```


## <a name="optional-enable-showing-the-display-name"></a>Facultatif : activer l’affichage du nom complet

Par défaut, le nom complet ne s’affiche pas. Pour afficher le nom complet sur moyenne/large/grande, ajoutez le code suivant.

```csharp
// Show the display name on all sizes
tile.VisualElements.ShowNameOnSquare150x150Logo = true;
tile.VisualElements.ShowNameOnWide310x150Logo = true;
tile.VisualElements.ShowNameOnSquare310x310Logo = true;
```


## <a name="optional-3d-secondary-tiles"></a>Facultatif : vignettes secondaires 3D
Vous pouvez améliorer votre vignette secondaire pour Windows Mixed Reality en ajoutant des ressources 3D. Les utilisateurs peuvent placer les vignettes 3D directement dans leur page d’accueil Windows Mixed Reality au lieu du menu Démarrer lors de l’utilisation de votre application dans un environnement de réalité mixte. Par exemple, vous pouvez créer des photosphères de 360 ° qui se lient directement à une application de visionneuse de photos 360 °, ou permettre aux utilisateurs de placer un modèle 3D d’une chaise à partir d’un catalogue de mobilier qui ouvre une page de détails sur les options de tarification et de couleur de cet objet lorsqu’il est sélectionné. Pour commencer, reportez-vous à la documentation du développeur de la [réalité mixte](https://developer.microsoft.com/windows/mixed-reality/implementing_3d_deep_links_for_your_app_in_the_windows_mixed_reality_home).



## <a name="pin-the-secondary-tile"></a>Épingler la vignette secondaire

Enfin, demandez à épingler la vignette. Notez que ce doit être appelé à partir d’un thread d’interface utilisateur. Sur le bureau, une boîte de dialogue s’affiche pour demander à l’utilisateur de confirmer s’il souhaite épingler la vignette.

> [!IMPORTANT]
> Si vous êtes une application de bureau Windows à l’aide du pont Desktop, vous devez d’abord effectuer une étape supplémentaire comme décrit dans [Épingler à partir de l’application de bureau](secondary-tiles-desktop-pinning.md) .

```csharp
// Pin the tile
bool isPinned = await tile.RequestCreateAsync();

// TODO: Update UI to reflect whether user can now either unpin or pin
```


## <a name="check-if-a-secondary-tile-exists"></a>Vérifier l’existence d’une vignette secondaire

Si votre utilisateur visite une page de votre application qu’il a déjà épinglée au démarrage, vous devez afficher à la place un bouton « désépingler ».

Par conséquent, lorsque vous choisissez le bouton à afficher, vous devez d’abord vérifier si la vignette secondaire est actuellement épinglée.

```csharp
// Check if the secondary tile is pinned
bool isPinned = SecondaryTile.Exists(tileId);

// TODO: Update UI to reflect whether user can either unpin or pin
```


## <a name="unpinning-a-secondary-tile"></a>Désépinglage d’une vignette secondaire

Si la vignette est actuellement épinglée et que l’utilisateur clique sur votre bouton de désépingler, vous pouvez désépingler (supprimer) la vignette.

```csharp
// Initialize a secondary tile with the same tile ID you want removed
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then unpin the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="updating-a-secondary-tile"></a>Mise à jour d’une vignette secondaire

Si vous devez mettre à jour les logos, le nom complet ou tout autre élément de la vignette secondaire, vous pouvez utiliser *RequestUpdateAsync*.

```csharp
// Initialize a secondary tile with the same tile ID you want to update
SecondaryTile tile = new SecondaryTile(tileId);

// Assign ALL properties, including ones you aren't changing

// And then update it
await tile.UpdateAsync();
```


## <a name="enumerating-all-pinned-secondary-tiles"></a>Énumération de toutes les vignettes secondaires épinglées

Si vous devez découvrir toutes les vignettes épinglées par l’utilisateur, au lieu d’utiliser *SecondaryTile. Exists*, vous pouvez également utiliser *SecondaryTile. FindAllAsync ()*.

```csharp
// Get all secondary tiles
var tiles = await SecondaryTile.FindAllAsync();
```


## <a name="send-a-tile-notification"></a>Envoyer une notification par vignette

Pour savoir comment afficher du contenu riche sur votre vignette via des notifications de vignette, consultez [Envoyer une notification de vignette locale](sending-a-local-tile-notification.md).


## <a name="related"></a>Rubriques connexes

* [Vue d’ensemble des vignettes secondaires](secondary-tiles.md)
* [Aide sur les vignettes secondaires](secondary-tiles-guidance.md)
* [Ressources de vignette](app-assets.md)
* [Documentation sur le contenu de la vignette](create-adaptive-tiles.md)
* [Envoyer une notification par vignette locale](sending-a-local-tile-notification.md)
