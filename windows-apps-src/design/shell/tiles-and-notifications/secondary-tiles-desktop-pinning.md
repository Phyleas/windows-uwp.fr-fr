---
description: Les applications de bureau peuvent épingler des vignettes secondaires grâce au pont de bureau.
title: Épingler des vignettes secondaires à partir d’applications de bureau
label: Pin secondary tiles from desktop apps
template: detail.hbs
ms.date: 05/25/2017
ms.topic: article
keywords: Windows 10, Desktop Bridge, vignettes secondaires, pin, épinglage, démarrage rapide, exemple de code, exemple, secondarytile, application de bureau, Win32, WinForms, WPF
ms.localizationpriority: medium
ms.openlocfilehash: 1c6451510816e36bed5574f0ae69254c9f4d34f6
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030652"
---
# <a name="pin-secondary-tiles-from-desktop-apps"></a>Épingler des vignettes secondaires à partir d’applications de bureau


Grâce au [pont de bureau](https://developer.microsoft.com/windows/bridges/desktop), les applications de bureau (comme Windows Forms et WPF) peuvent épingler des vignettes secondaires.

![Capture d’écran des vignettes secondaires](images/secondarytiles.png)

> [!IMPORTANT]
> **Nécessite la mise à jour des créateurs** de la place : vous devez cibler le kit de développement logiciel 16299 et exécuter Build 16299 ou version ultérieure pour épingler des vignettes secondaires à partir d’applications Desktop Bridge.

L’ajout d’une vignette secondaire à partir de votre application WPF ou WinForms est très similaire à une application UWP pure. La seule différence est que vous devez spécifier votre handle de fenêtre principale (HWND). En effet, lors de l’épinglage d’une vignette, Windows affiche une boîte de dialogue modale demandant à l’utilisateur de confirmer s’il souhaite épingler la vignette. Si l’application de bureau ne configure pas l’objet SecondaryTile à l’aide de la fenêtre propriétaire, Windows ne sait pas où dessiner la boîte de dialogue et l’opération échoue.


## <a name="package-your-app-with-desktop-bridge"></a>Empaqueter votre application avec Desktop Bridge

Si vous n’avez pas empaqueté votre application avec le pont de bureau, [vous devez d’abord le faire](/windows/msix/desktop/source-code-overview) avant de pouvoir utiliser les api de Windows Runtime.


## <a name="enable-access-to-iinitializewithwindow-interface"></a>Activer l’accès à l’interface IInitializeWithWindow

Si votre application est écrite dans un langage managé tel que C# ou Visual Basic, déclarez l’interface IInitializeWithWindow dans le code de votre application avec l’attribut [ComImport](/dotnet/api/system.runtime.interopservices.comimportattribute) et GUID, comme indiqué dans l’exemple C# suivant. Cet exemple suppose que votre fichier de code présente une instruction using pour l’espace de noms System.Runtime.InteropServices.

```csharp
[ComImport]
[Guid("3E68D4BD-7135-4D10-8018-9FB6D9F33FA1")]
[InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface IInitializeWithWindow
{
    void Initialize(IntPtr hwnd);
}
```

Si vous utilisez C++, vous pouvez également ajouter une référence au fichier d’en-tête **ShObjIdl. h** dans votre code. Ce fichier d’en-tête contient la déclaration de l’interface *IInitializeWithWindow* .


## <a name="initialize-the-secondary-tile"></a>Initialiser la vignette secondaire

Initialisez un nouvel objet de vignette secondaire exactement comme vous le feriez avec une application UWP normale. Pour en savoir plus sur la création et l’épinglage des vignettes secondaires, consultez [Épingler des vignettes secondaires](secondary-tiles-pinning.md).

```csharp
// Initialize the tile with required arguments
SecondaryTile tile = new SecondaryTile(
    "myTileId5391",
    "Display name",
    "myActivationArgs",
    new Uri("ms-appx:///Assets/Square150x150Logo.png"),
    TileSize.Default);
```


## <a name="assign-the-window-handle"></a>Assigner le handle de fenêtre

Il s’agit de l’étape clé pour les applications de bureau. Effectuez un cast de l’objet en objet [IInitializeWithWindow](/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iinitializewithwindow) . Ensuite, appelez la méthode [IInitializeWithWindow.Initialize](/windows/desktop/api/shobjidl_core/nf-shobjidl_core-iinitializewithwindow-initialize) et transmettez le descripteur de la fenêtre qui doit être le propriétaire de la boîte de dialogue modale. L’exemple C# suivant montre comment passer le handle de la fenêtre principale de votre application à la méthode.

```csharp
// Assign the window handle
IInitializeWithWindow initWindow = (IInitializeWithWindow)(object)tile;
initWindow.Initialize(System.Diagnostics.Process.GetCurrentProcess().MainWindowHandle);
```


## <a name="pin-the-tile"></a>Épingler la vignette

Enfin, demandez à épingler la vignette comme vous le feriez pour une application UWP normale.

```csharp
// Pin the tile
bool isPinned = await tile.RequestCreateAsync();

// TODO: Update UI to reflect whether user can now either unpin or pin
```


## <a name="send-tile-notifications"></a>Envoyer des notifications par vignette

> [!IMPORTANT]
> **Nécessite la version 17134,81 du 2018 avril ou une version ultérieure** : vous devez exécuter Build 17134,81 ou version ultérieure pour envoyer des notifications de vignette ou de badge à des vignettes secondaires à partir d’applications Desktop Bridge. Avant cette mise à jour de maintenance. 81, une exception d' *élément 0x80070490 introuvable* s’est produite lors de l’envoi de notifications de vignette ou de badge à des vignettes secondaires à partir d’applications Desktop Bridge.

L’envoi de notifications de vignette ou de badge est identique à celui des applications UWP. Consultez [Envoyer une notification de vignette locale](sending-a-local-tile-notification.md) pour commencer.


## <a name="resources"></a>Ressources

* [Exemple de code complet](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [Vue d’ensemble des vignettes secondaires](secondary-tiles.md)
* [Épingler des vignettes secondaires (UWP)](secondary-tiles-pinning.md)
* [Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop)
* [Exemples de code du pont du Bureau](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)
