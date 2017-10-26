---
author: vladimp
Description: "Les applications de bureau Windows peuvent épingler des vignettes secondaires grâce au Pont du bureau!"
title: "Épingler des vignettes secondaires à partir d’une application de bureau"
label: Pin secondary tiles from desktop application
template: detail.hbs
ms.author: wdg-dev-content
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows10, pont de bureau, vignettes secondaires, épingler, code confidentiel, épinglage, démarrage rapide, exemple de code, exemple, secondarytile, application de bureau, win32, winforms, wpf"
ms.openlocfilehash: dea17368a21983b9ca800aac2efa6410dab06958
ms.sourcegitcommit: 6396a69aab081f5c7a9a59739c83538616d3b1c7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/30/2017
---
# <a name="pin-secondary-tiles-from-desktop-application"></a>Épingler des vignettes secondaires à partir d’une application de bureau
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Grâce au [Pont du bureau](https://developer.microsoft.com/en-us/windows/bridges/desktop), les applications de bureau Windows (telles que Win32, Windows Forms et WPF) peuvent épingler des vignettes secondaires!

> [!IMPORTANT]
> **VERSION PRÉLIMINAIRE | Nécessite Fall Creators Update**: vous devez exécuter [Insider build16199](https://blogs.windows.com/windowsexperience/2017/05/17/announcing-windows-10-insider-preview-build-16199-pc-build-15215-mobile/#bDqf2Ah3Gd7FM66g.97) ou version ultérieure pour utiliser les vignettes secondaires à partir de votre application de bureau.

![Capture d’écran de vignettes secondaires](images/secondarytiles.png)

L’ajout d’une vignette secondaire à partir de votre application WinForms ou WPF est très similaire à une application UWP pure. La seule différence est que vous devez spécifier votre handle de fenêtre principale (HWND). Cela est dû au fait que lors de l’épinglage d’une vignette, Windows affiche une boîte de dialogue modale demandant à l’utilisateur de confirmer s’il souhaite épingler la vignette. Si l’application de bureau ne configure pas l’objet SecondaryTile avec la fenêtre du propriétaire, Windows ne sait pas où dessiner la boîte de dialogue et l’opération échoue.


## <a name="package-your-app-with-desktop-bridge"></a>Créer un package de votre application avec le Pont du bureau

Si vous n’avez pas créé de package de votre application avec le Pont du bureau, [vous devez commencez par le faire](https://docs.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-root) avant de pouvoir utiliser toutes les API UWP.


## <a name="enable-access-to-iinitializewithwindow-interface"></a>Permettre l’accès à l’interface IInitializeWithWindow

Si votre application est écrite dans un langage géré comme C# ou VisualBasic, déclarez l’interface IInitializeWithWindow dans le code de votre application avec l’attribut [ComImport](https://msdn.microsoft.com/library/system.runtime.interopservices.comimportattribute.aspx) et Guid comme indiqué dans l’exemple suivant en C#. Cet exemple suppose que votre fichier de code présente une instruction using pour l’espace de noms System.Runtime.InteropServices.

```csharp
[ComImport]
[Guid("3E68D4BD-7135-4D10-8018-9FB6D9F33FA1")]
[InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface IInitializeWithWindow
{
    void Initialize(IntPtr hwnd);
}
```

Sinon, si vous utilisez C++, ajoutez une référence au fichier d’en-tête **shobjidl.h** dans votre code. Ce fichier d’en-tête contient la déclaration de l’interface *IInitializeWithWindow*.


## <a name="initialize-the-secondary-tile"></a>Initialiser la vignette secondaire

Initialiser un nouvel objet de vignette secondaire exactement comme vous le feriez avec une application UWP normale. Pour en savoir plus sur la création et l’épinglage des vignettes secondaires, voir [Épingler les vignettes secondaires](tiles-and-notifications-secondary-tiles-pinning.md).

```csharp
// Initialize the tile with required arguments
SecondaryTile tile = new SecondaryTile(
    "myTileId5391",
    "Display name",
    "myActivationArgs",
    new Uri("ms-appx:///Assets/Square150x150Logo.png"),
    TileSize.Default);
```


## <a name="assign-the-window-handle"></a>Affecter le handle de fenêtre

Il s’agit d’une étape essentielle pour les applications de bureau. Effectuez un cast de l’objet sur un objet [IInitializeWithWindow](https://msdn.microsoft.com/library/windows/desktop/hh706981.aspx). Ensuite, appelez la méthode [IInitializeWithWindow.Initialize](https://msdn.microsoft.com/library/windows/desktop/hh706982.aspx), puis transmettez le handle de la fenêtre que vous souhaitez configurer comme propriétaire pour la boîte de dialogue modale. L’exemple suivant en C# vous explique comment transmettre le handle de la fenêtre principale de votre application à la méthode.

```csharp
// Assign the window handle
IInitializeWithWindow initWindow = (IInitializeWithWindow)(object)tile;
initWindow.Initialize(System.Diagnostics.Process.GetCurrentProcess().MainWindowHandle);
```


## <a name="pin-the-tile"></a>Épingler la vignette secondaire

Enfin, demandez à épingler la vignette comme vous le feriez pour une application UWP normale.

```csharp
// Pin the tile
bool isPinned = await tile.RequestCreateAsync();

// TODO: Update UI to reflect whether user can now either unpin or pin
```


## <a name="resources"></a>Ressources

* [Exemple de code complet](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [Vue d’ensemble des vignettes secondaires](tiles-and-notifications-secondary-tiles.md)
* [Épingler les vignettes secondaires (UWP)](tiles-and-notifications-secondary-tiles-pinning.md)
* [Pont du bureau](https://developer.microsoft.com/en-us/windows/bridges/desktop)
* [Exemples de code du Pont du bureau](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)