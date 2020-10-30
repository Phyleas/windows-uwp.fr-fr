---
description: Vous pouvez épingler par programmation la vignette principale de votre propre application pour démarrer, comme vous pouvez épingler des vignettes secondaires. Et vous pouvez vérifier s’il est actuellement épinglé.
title: API de vignettes principales
label: Primary tile API's
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP, StartScreenManager, épingler la vignette principale, API de vignettes principales, vérifier si la vignette est épinglée, vignette dynamique
ms.localizationpriority: medium
ms.openlocfilehash: 83cf11d80ffcd03148cbe5e784aaad5836357796
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93029692"
---
# <a name="primary-tile-apis"></a>API de vignette principale
 

Les API de vignettes principales vous permettent de vérifier si votre application est actuellement épinglée pour démarrer et de demander l’épinglage de la vignette principale de votre application.

> [!IMPORTANT]
> **Nécessite Creators Update** : vous devez cibler le kit de développement logiciel (SDK) 15063 et exécuter la version 15063 ou une version ultérieure pour utiliser les API de vignettes principales.

> **API importantes** : [**classe StartScreenManager**](/uwp/api/windows.ui.startscreen.startscreenmanager), [ContainsAppListEntryAsync](/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_ContainsAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_), [RequestAddAppListEntryAsync](/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_RequestAddAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_)


## <a name="when-to-use-primary-tile-apis"></a>Quand utiliser les API de vignettes principales

Vous avez beaucoup de travail à concevoir une expérience remarquable pour la vignette principale de votre application, et vous avez maintenant la possibilité de demander à l’utilisateur de l’épingler pour démarrer. Mais avant de plonger dans le code, voici quelques points à garder à l’esprit lorsque vous concevez votre expérience :

* **Élaborez** une expérience utilisateur révocables et sans interruption de l’activité dans votre application avec un appel « épingler une vignette dynamique » en clair.
* **Expliquez clairement la** valeur de la vignette dynamique de votre application avant de demander à l’utilisateur de l’épingler.
* **Ne demandez pas** à un utilisateur d’épingler la vignette de votre application si celle-ci est déjà épinglée ou si l’appareil ne la prend pas en charge (vous trouverez plus d’informations ci-dessous).
* **Ne demandez pas** à l’utilisateur d’épingler la vignette de votre application (elles seront probablement importunées).
* **N’appelez pas** l’API pin sans interaction explicite de l’utilisateur ou lorsque votre application est réduite/non ouverte.


## <a name="checking-whether-the-apis-exist"></a>Vérification de l’existence de l’API

Si votre application prend en charge des versions antérieures de Windows 10, vous devez vérifier si ces API de vignettes principales sont disponibles. Pour ce faire, utilisez ApiInformation. Si les API de vignette principales ne sont pas disponibles, évitez d’exécuter les appels aux API.

```csharp
if (ApiInformation.IsTypePresent("Windows.UI.StartScreen.StartScreenManager"))
{
    // Primary tile API's supported!
}
else
{
    // Older version of Windows, no primary tile API's
}
```


## <a name="check-if-start-supports-your-app"></a>Vérifier si le démarrage prend en charge votre application

Selon le menu Démarrer actuel, et votre type d’application, l’épinglage de votre application à l’écran de démarrage actuel peut ne pas être pris en charge. Seule la prise en charge de Desktop et mobile épingle la vignette principale pour démarrer. Par conséquent, avant d’illustrer une interface utilisateur de code confidentiel ou d’exécuter n’importe quel code confidentiel, vous devez d’abord vérifier si votre application est prise en charge pour l’écran d’accueil actuel. S’il n’est pas pris en charge, ne pas demander à l’utilisateur d’épingler la vignette.

```csharp
// Get your own app list entry
// (which is always the first app list entry assuming you are not a multi-app package)
AppListEntry entry = (await Package.Current.GetAppListEntriesAsync())[0];

// Check if Start supports your app
bool isSupported = StartScreenManager.GetDefault().SupportsAppListEntry(entry);
```


## <a name="check-whether-youre-currently-pinned"></a>Vérifier si vous êtes actuellement épinglé

Pour déterminer si votre vignette principale est actuellement épinglée au démarrage, utilisez la méthode [ContainsAppListEntryAsync](/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_ContainsAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_) .

```csharp
// Get your own app list entry
AppListEntry entry = (await Package.Current.GetAppListEntriesAsync())[0];

// Check if your app is currently pinned
bool isPinned = await StartScreenManager.GetDefault().ContainsAppListEntryAsync(entry);
```


##  <a name="pin-your-primary-tile"></a>Épingler votre vignette principale

Si votre vignette principale n’est pas épinglée actuellement, et si votre vignette est prise en charge par le démarrage, vous souhaiterez peut-être afficher un Conseil aux utilisateurs pour qu’ils puissent épingler votre vignette principale.

> [!NOTE]
> Vous devez appeler cette API à partir d’un thread d’interface utilisateur pendant que votre application est au premier plan, et vous devez appeler cette API uniquement une fois que l’utilisateur a demandé intentionnellement que la vignette principale soit épinglée (par exemple, une fois que l’utilisateur a cliqué sur Oui pour épingler la vignette).

Si l’utilisateur clique sur votre bouton pour épingler la vignette principale, vous devez appeler la méthode [RequestAddAppListEntryAsync](/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_RequestAddAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_) pour demander que votre vignette soit épinglée au démarrage. Cette opération affiche une boîte de dialogue demandant à l’utilisateur de confirmer qu’il souhaite que votre vignette soit épinglée au démarrage.

Cela retourne une valeur booléenne indiquant si votre vignette est maintenant épinglée au démarrage. Si votre vignette était déjà épinglée, cette opération retourne immédiatement la valeur true sans afficher la boîte de dialogue à l’utilisateur. Si l’utilisateur clique sur non dans la boîte de dialogue ou si l’épinglage de votre vignette au démarrage n’est pas pris en charge, la valeur false est retournée. Dans le cas contraire, l’utilisateur a cliqué sur Oui et la vignette a été épinglée, et l’API retourne true.

```csharp
// Get your own app list entry
AppListEntry entry = (await Package.Current.GetAppListEntriesAsync())[0];

// And pin it to Start
bool isPinned = await StartScreenManager.GetDefault().RequestAddAppListEntryAsync(entry);
```


## <a name="resources"></a>Ressources

* [Exemple de code complet sur GitHub](https://github.com/WindowsNotifications/quickstart-pin-primary-tile)
* [Épingler à la barre des tâches](../pin-to-taskbar.md)
* [Vignettes, badges et notifications](index.md)
* [Documentation sur les vignettes adaptatives](create-adaptive-tiles.md)
