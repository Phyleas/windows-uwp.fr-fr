---
description: Vous pouvez épingler votre application par programmation à la barre des tâches, BND vous pouvez vérifier si elle est actuellement épinglée.
title: Épingler votre application à la barre des tâches
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP, barre des tâches, gestionnaire de barre des tâches, épingler à la barre des tâches, vignette principale
ms.localizationpriority: medium
ms.openlocfilehash: fa33725447da80b5c3295455f12a3851228a2756
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034162"
---
# <a name="pin-your-app-to-the-taskbar"></a>Épingler votre application à la barre des tâches

Vous pouvez épingler votre application à la barre des tâches par programmation, tout comme vous pouvez [l’épingler à votre application dans le menu Démarrer](tiles-and-notifications/primary-tile-apis.md). Vous pouvez aussi vérifier si votre application est actuellement épinglée et si la barre des tâches autorise l’épinglage. 

![Capture d’écran d’une barre des tâches Windows 10 montrant l’application épinglée.](images/taskbar/taskbar.png)

> [!IMPORTANT]
> **Nécessite la mise à jour des créateurs de automne** : vous devez cibler le SDK 16299 et exécuter la version 16299 ou une version ultérieure pour utiliser les API de la barre des tâches.

> **API importantes** : [classe TaskbarManager](/uwp/api/windows.ui.shell.taskbarmanager) 


## <a name="when-should-you-ask-the-user-to-pin-your-app-to-the-taskbar"></a>Quand devez-vous demander à l’utilisateur d’épingler votre application à la barre des tâches ? 

La [classe TaskbarManager](/uwp/api/windows.ui.shell.taskbarmanager) vous permet de demander à l’utilisateur d’épingler votre application à la barre des tâches ; l’utilisateur doit approuver la demande. Vous avez beaucoup d’efforts dans la création d’une application extraordinaire. vous avez maintenant la possibilité de demander à l’utilisateur de l’épingler à la barre des tâches. Mais avant de plonger dans le code, voici quelques points à garder à l’esprit lorsque vous concevez votre expérience :

* **Élaborez** une expérience utilisateur non perturbatrice et facilement révocables dans votre application avec l’appel « épingler à la barre des tâches » en clair. Évitez d’utiliser des boîtes de dialogue et des lanceurs à cet effet. 
* **Expliquez clairement la** valeur de votre application avant de demander à l’utilisateur de l’épingler.
* **Ne demandez pas** à un utilisateur d’épingler votre application si celle-ci est déjà épinglée ou si l’appareil ne la prend pas en charge. (Cet article explique comment déterminer si l’épinglage est pris en charge.)
* **Ne demandez pas** à l’utilisateur d’épingler votre application à plusieurs reprises (ils seront probablement importunés).
* **N’appelez pas** l’API pin sans interaction explicite de l’utilisateur ou lorsque votre application est réduite/non ouverte.


## <a name="1-check-whether-the-required-apis-exist"></a>1. Vérifiez si les API requises existent.

Si votre application prend en charge des versions antérieures de Windows 10, vous devez vérifier si la classe TaskbarManager est disponible. Vous pouvez utiliser la  [méthode ApiInformation. IsTypePresent](/uwp/api/windows.foundation.metadata.apiinformation#Windows_Foundation_Metadata_ApiInformation_IsTypePresent_System_String_) pour effectuer cette vérification. Si la classe TaskbarManager n’est pas disponible, évitez d’exécuter les appels aux API.

```csharp
if (ApiInformation.IsTypePresent("Windows.UI.Shell.TaskbarManager"))
{
    // Taskbar APIs exist!
}

else
{
    // Older version of Windows, no taskbar APIs
}
```


## <a name="2-check-whether-taskbar-is-present-and-allows-pinning"></a>2. Vérifiez si la barre des tâches est présente et autorise l’épinglage

Les applications Windows peuvent s’exécuter sur un large éventail d’appareils. tous ne prennent pas en charge la barre des tâches. Pour le moment, seuls les appareils de bureau prennent en charge la barre des tâches. 

Même si la barre des tâches est disponible, une stratégie de groupe sur l’ordinateur de l’utilisateur peut désactiver l’épinglage de la barre des tâches. Ainsi, avant d’essayer d’épingler votre application, vous devez vérifier si l’épinglage à la barre des tâches est pris en charge. La [propriété TaskbarManager. IsPinningAllowed](/uwp/api/windows.ui.shell.taskbarmanager.IsPinningAllowed) retourne la valeur true si la barre des tâches est présente et autorise l’épinglage. 

```csharp
// Check if taskbar allows pinning (Group Policy can disable it, or some device families don't have taskbar)
bool isPinningAllowed = TaskbarManager.GetDefault().IsPinningAllowed;
```

> [!NOTE]
> Si vous ne souhaitez pas épingler votre application à la barre des tâches et que vous souhaitez simplement savoir si la barre des tâches est disponible, utilisez la [propriété TaskbarManager. IsSupported](/uwp/api/windows.ui.shell.taskbarmanager.IsSupported).


## <a name="3-check-whether-your-app-is-currently-pinned-to-the-taskbar"></a>3. Vérifiez si votre application est actuellement épinglée à la barre des tâches

Évidemment, il est inutile de demander à l’utilisateur de vous permettre d’épingler l’application à la barre des tâches si elle est déjà épinglée. Vous pouvez utiliser la [méthode TaskbarManager. IsCurrentAppPinnedAsync](/uwp/api/windows.ui.shell.taskbarmanager.IsCurrentAppPinnedAsync) pour vérifier si l’application est déjà épinglée avant de demander à l’utilisateur.

```csharp
// Check whether your app is currently pinned
bool isPinned = await TaskbarManager.GetDefault().IsCurrentAppPinnedAsync();

if (isPinned)
{
    // The app is already pinned--no point in asking to pin it again!
}
else 
{
    //The app is not pinned. 
}
```


##  <a name="4-pin-your-app"></a>4. Épinglez votre application

Si la barre des tâches est présente et que l’épinglage est autorisé et que votre application n’est pas épinglée actuellement, vous pouvez afficher un Conseil discret pour permettre aux utilisateurs de savoir qu’ils peuvent épingler votre application. Par exemple, vous pouvez afficher une icône d’épingle dans votre interface utilisateur sur laquelle l’utilisateur peut cliquer. 

Si l’utilisateur clique sur l’interface utilisateur de votre suggestion de code confidentiel, vous appelez la [méthode TaskbarManager. RequestPinCurrentAppAsync](/uwp/api/windows.ui.shell.taskbarmanager.RequestPinCurrentAppAsync). Cette méthode affiche une boîte de dialogue qui demande à l’utilisateur de confirmer qu’il souhaite que votre application soit épinglée à la barre des tâches.

> [!IMPORTANT]
> Cela doit être appelé à partir d’un thread d’interface utilisateur de premier plan, sinon une exception est levée.

```csharp
// Request to be pinned to the taskbar
bool isPinned = await TaskbarManager.GetDefault().RequestPinCurrentAppAsync();
```

![Boîte de dialogue épingler](images/taskbar/pin-dialog.png)

Cette méthode retourne une valeur booléenne qui indique si votre application est désormais épinglée à la barre des tâches. Si votre application était déjà épinglée, la méthode retourne immédiatement la valeur true sans montrer la boîte de dialogue à l’utilisateur. Si l’utilisateur clique sur « non » dans la boîte de dialogue, ou si l’épinglage de votre application à la barre des tâches n’est pas autorisé, la méthode retourne la valeur false. Dans le cas contraire, l’utilisateur a cliqué sur Oui et l’application a été épinglée et l’API retourne true.


## <a name="resources"></a>Ressources

* [Exemple de code complet sur GitHub](https://github.com/WindowsNotifications/quickstart-pin-to-taskbar)
* [TaskbarManager, classe](/uwp/api/windows.ui.shell.taskbarmanager)
* [Épingler une application au menu Démarrer](tiles-and-notifications/primary-tile-apis.md)
