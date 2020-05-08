---
Description: Découvrez comment épingler des vignettes secondaires à la barre des tâches.
title: Épingler les vignettes secondaires à la barre des tâches
label: Pin secondary tiles to taskbar
template: detail.hbs
ms.date: 11/28/2018
ms.topic: article
keywords: Windows 10, UWP, épingler à la barre des tâches, vignette secondaire, épingler les vignettes secondaires à la barre des tâches, raccourci
ms.localizationpriority: medium
ms.openlocfilehash: 5d4041faf2fbc729291da902e66be1e0979f9d97
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82971014"
---
# <a name="pin-secondary-tiles-to-taskbar"></a>Épingler les vignettes secondaires à la barre des tâches

Tout comme l’épinglage des vignettes secondaires pour démarrer, vous pouvez épingler des vignettes secondaires à la barre des tâches, ce qui permet à vos utilisateurs d’accéder rapidement au contenu de votre application.

<img alt="Taskbar pinning" src="../images/taskbar/pin-secondary-ui.png" width="972"/>

> [!IMPORTANT]
> **API à accès limité**: cette API est une fonctionnalité à accès limité. Pour utiliser cette API, contactez [taskbarsecondarytile@microsoft.com](mailto:taskbarsecondarytile@microsoft.com?Subject=Limited%20Access%20permission%20to%20use%20secondary%20tiles%20on%20taskbar).

> **Nécessite la mise à jour du 2018 octobre**: vous devez cibler le SDK 17763 et exécuter Build 17763 ou une version ultérieure pour épingler à la barre des tâches.


## <a name="guidance"></a>Instructions

Une vignette secondaire offre un moyen cohérent et efficace pour les utilisateurs d’accéder directement à des zones spécifiques au sein d’une application. Bien qu’un utilisateur décide s’il faut ou non « épingler » une vignette secondaire à la barre des tâches, les zones regroupement dans une application sont déterminées par le développeur. Pour plus d’informations, consultez aide sur les [vignettes secondaires](secondary-tiles-guidance.md).


## <a name="1-determine-if-api-exists-and-unlock-limited-access"></a>1. Déterminez si l’API existe et déverrouillez l’accès limité

Les appareils plus anciens n’ont pas les API d’épinglage de la barre des tâches (si vous ciblez des versions antérieures de Windows 10). Par conséquent, vous ne devriez pas afficher de bouton pin sur ces appareils qui ne peuvent pas être épinglés.

En outre, cette fonctionnalité est verrouillée dans le cadre d’un accès limité. Pour y accéder, contactez Microsoft. Les appels d’API à **[TaskbarManager. RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync#Windows_UI_Shell_TaskbarManager_RequestPinSecondaryTileAsync_Windows_UI_StartScreen_SecondaryTile_)**, **[TaskbarManager. IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** et **[TaskbarManager. TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)** échouent avec une exception accès refusé. Les applications ne sont pas autorisées à utiliser cette API sans autorisation, et la définition de l’API peut changer à tout moment.

Utilisez la méthode [ApiInformation. IsMethodPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.ismethodpresent#Windows_Foundation_Metadata_ApiInformation_IsMethodPresent_System_String_System_String_) pour déterminer si les API sont présentes. Puis utilisez l’API **[LimitedAccessFeatures](https://docs.microsoft.com/uwp/api/windows.applicationmodel.limitedaccessfeatures)** pour essayer de déverrouiller l’API.

```csharp
if (ApiInformation.IsMethodPresent("Windows.UI.Shell.TaskbarManager", "RequestPinSecondaryTileAsync"))
{
    // API present!
    // Unlock the pin to taskbar feature
    var result = LimitedAccessFeatures.TryUnlockFeature(
        "com.microsoft.windows.secondarytilemanagement",
        "<tokenFromMicrosoft>",
        "<publisher> has registered their use of com.microsoft.windows.secondarytilemanagement with Microsoft and agrees to the terms of use.");

    // If unlock succeeded
    if ((result.Status == LimitedAccessFeatureStatus.Available) ||
        (result.Status == LimitedAccessFeatureStatus.AvailableWithoutToken))
    {
        // Continue
    }
    else
    {
        // Don't show pin to taskbar button or call any of the below APIs
    }
}

else
{
    // Don't show pin to taskbar button or call any of the below APIs
}
```


## <a name="2-get-the-taskbarmanager-instance"></a>2. accéder à l’instance TaskbarManager

Les applications Windows peuvent s’exécuter sur un large éventail d’appareils. tous ne prennent pas en charge la barre des tâches. Pour le moment, seuls les appareils de bureau prennent en charge la barre des tâches. En outre, la présence de la barre des tâches peut se poursuivre. Pour vérifier si la barre des tâches est actuellement présente, appelez la méthode **[TaskbarManager. GetDefault](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.getdefault)** et vérifiez que l’instance retournée n’est pas null. N’affiche pas de bouton pin si la barre des tâches n’est pas présente.

Nous vous recommandons de maintenir sur l’instance pendant la durée d’une opération unique, comme épinglage, puis de saisir une nouvelle instance la prochaine fois que vous devez effectuer une autre opération.

```csharp
TaskbarManager taskbarManager = TaskbarManager.GetDefault();

if (taskbarManager != null)
{
    // Continue
}
else
{
    // Taskbar not present, don't display a pin button
}
```


## <a name="3-check-whether-your-tile-is-currently-pinned-to-the-taskbar"></a>3. vérifier si votre vignette est actuellement épinglée à la barre des tâches

Si votre vignette est déjà épinglée, vous devez afficher un bouton détacher à la place. Vous pouvez utiliser la méthode **[IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** pour vérifier si votre vignette est actuellement épinglée (les utilisateurs peuvent la désépingler à tout moment). Dans cette méthode, vous transmettez le **TileId** de la vignette que vous souhaitez savoir est épinglé.

```csharp
if (await taskbarManager.IsSecondaryTilePinnedAsync("myTileId"))
{
    // The tile is already pinned. Display the unpin button.
}

else 
{
    // The tile is not pinned. Display the pin button.
}
```


## <a name="4-check-whether-pinning-is-allowed"></a>4. vérifier si l’épinglage est autorisé

L’épinglage à la barre des tâches peut être désactivé par stratégie de groupe. La propriété [TaskbarManager. IsPinningAllowed](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.ispinningallowed) vous permet de vérifier si l’épinglage est autorisé.

Quand l’utilisateur clique sur votre bouton pin, vous devez vérifier cette propriété et, si elle est false, vous devez afficher une boîte de dialogue de message informant l’utilisateur que l’épinglage n’est pas autorisé sur cet ordinateur.

```csharp
TaskbarManager taskbarManager = TaskbarManager.GetDefault();
if (taskbarManager == null)
{
    // Display message dialog informing user that taskbar is no longer present, and then hide the button
}

else if (taskbarManager.IsPinningAllowed == false)
{
    // Display message dialog informing user pinning is not allowed on this machine
}

else
{
    // Continue pinning
}
```


## <a name="5-construct-and-pin-your-tile"></a>5. construire et épingler votre vignette

L’utilisateur a cliqué sur votre bouton pin et vous avez déterminé que les API sont présentes, la barre des tâches est présente et l’épinglage est autorisé... heure à épingler !

Tout d’abord, construisez votre vignette secondaire comme vous le feriez lors de l’épinglage pour démarrer. Pour plus d’informations sur les propriétés des vignettes secondaires, consultez [Épingler des vignettes secondaires au début](secondary-tiles-pinning.md). Toutefois, lors de l’épinglage à la barre des tâches, outre les propriétés requises précédemment, Square44x44Logo (il s’agit du logo utilisé par la barre des tâches) est également requis. Sinon, une exception est levée.

Ensuite, transmettez la vignette à la méthode **[RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync)** . Étant donné qu’il s’agit d’un accès limité, cela n’affiche pas de boîte de dialogue de confirmation et ne nécessite pas de thread d’interface utilisateur. Mais à l’avenir, lorsque cela s’ouvre au-delà de l’accès limité, les appelants qui n’utilisent pas l’accès limité reçoivent une boîte de dialogue et doivent utiliser le thread d’interface utilisateur.

```csharp
// Initialize the tile (all properties below are required)
SecondaryTile tile = new SecondaryTile("myTileId");
tile.DisplayName = "PowerPoint 2016 (Remote)";
tile.Arguments = "app=powerpoint";
tile.VisualElements.Square44x44Logo = new Uri("ms-appdata:///AppIcons/PowerPoint_Square44x44Logo.png");
tile.VisualElements.Square150x150Logo = new Uri("ms-appdata:///AppIcons/PowerPoint_Square150x150Logo.png");

// Pin it to the taskbar
bool isPinned = await taskbarManager.RequestPinSecondaryTileAsync(tile);
```

Cette méthode retourne une valeur booléenne qui indique si votre vignette est désormais épinglée à la barre des tâches. Si votre vignette était déjà épinglée, la méthode met à jour la vignette existante et retourne true. Si l’épinglage n’était pas autorisé ou si la barre des tâches n’est pas prise en charge, la méthode retourne false


## <a name="enumerate-tiles"></a>Énumérer les vignettes

Pour afficher toutes les vignettes que vous avez créées et qui sont toujours épinglées à un emplacement (Démarrer, barre des tâches, ou les deux), utilisez **[FindAllAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.findallasync)**. Vous pouvez ensuite vérifier si ces vignettes sont épinglées à la barre des tâches et/ou démarrer. Si la surface n’est pas prise en charge, ces méthodes retournent false.

```csharp
var taskbarManager = TaskbarManager.GetDefault();
var startScreenManager = StartScreenManager.GetDefault();

// Look through all tiles
foreach (SecondaryTile tile in await SecondaryTile.FindAllAsync())
{
    if (taskbarManager != null && await taskbarManager.IsSecondaryTilePinnedAsync(tile.TileId))
    {
        // Tile is pinned to the taskbar
    }

    if (startScreenManager != null && await startScreenManager.ContainsSecondaryTileAsync(tile.TileId))
    {
        // Tile is pinned to Start
    }
}
```


## <a name="update-a-tile"></a>Mettre à jour une vignette

Pour mettre à jour une vignette déjà épinglée, vous pouvez utiliser la méthode [**SecondaryTile. UpdateAsync**](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.updateasync) comme décrit dans [mise à jour d’une vignette secondaire](secondary-tiles-pinning.md#updating-a-secondary-tile).


## <a name="unpin-a-tile"></a>Détacher une vignette

Votre application doit fournir un bouton détacher si la vignette est actuellement épinglée. Pour désépingler la vignette, appelez simplement **[TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)**, en passant le **TileId** de la vignette secondaire que vous souhaitez désépingler.

Cette méthode retourne une valeur booléenne qui indique si votre vignette n’est plus épinglée à la barre des tâches. Si votre vignette n’a pas été épinglée en premier lieu, elle retourne également true. Si le désépinglage n’était pas autorisé, la valeur false est retournée.

Si votre vignette n’était épinglée qu’à la barre des tâches, cette opération supprimera la vignette, car elle ne sera plus épinglée nulle part.

```csharp
var taskbarManager = TaskbarManager.GetDefault();
if (taskbarManager != null)
{
    bool isUnpinned = await taskbarManager.TryUnpinSecondaryTileAsync("myTileId");
}
```


## <a name="delete-a-tile"></a>Supprimer une vignette

Si vous souhaitez désépingler une vignette de partout (Démarrer, barre des tâches), utilisez la méthode **[RequestDeleteAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.requestdeleteasync)** .

Cela est approprié dans les cas où le contenu épinglé par l’utilisateur n’est plus applicable. Par exemple, si votre application vous permet d’épingler un bloc-notes au démarrage et à la barre des tâches, puis que l’utilisateur supprime le bloc-notes, il vous suffit de supprimer la vignette associée au bloc-notes.

```csharp
// Initialize a secondary tile with the same tile ID you want removed.
// Or, locate it with FindAllAsync()
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then delete the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="unpin-only-from-start"></a>Détacher uniquement du début

Si vous souhaitez uniquement désépingler une vignette secondaire du début en la laissant sur la barre des tâches, vous pouvez appeler la méthode **[StartScreenManager. TryRemoveSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager.tryremovesecondarytileasync)** . De la même façon, la vignette est supprimée si elle n’est plus épinglée à d’autres surfaces.

Cette méthode retourne une valeur booléenne qui indique si votre vignette n’est plus épinglée au démarrage. Si votre vignette n’a pas été épinglée en premier lieu, elle retourne également true. Si le désépinglage n’a pas été autorisé ou que Start n’est pas pris en charge, la valeur false.

```csharp
await StartScreenManager.GetDefault().TryRemoveSecondaryTileAsync("myTileId");
```


## <a name="resources"></a>Ressources

* [TaskbarManager, classe](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager)
* [Épingler les vignettes secondaires au début](secondary-tiles-pinning.md)
