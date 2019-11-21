---
ms.assetid: AAE467F9-B3C7-4366-99A2-8A880E5692BE
title: Utiliser un minuteur pour envoyer un élément de travail
description: Découvrez comment créer un élément de travail qui s’exécute une fois le délai du minuteur écoulé.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, minuteur, threads
ms.localizationpriority: medium
ms.openlocfilehash: 7bd870858bbccffa07b082384ae6ddea987b67f2
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258926"
---
# <a name="use-a-timer-to-submit-a-work-item"></a>Utiliser un minuteur pour envoyer un élément de travail


<b>API importantes</b>

-   [**Windows.UI.Core namespace**](https://docs.microsoft.com/uwp/api/Windows.UI.Core)
-   [**Windows.System.Threading namespace**](https://docs.microsoft.com/uwp/api/Windows.System.Threading)

Découvrez comment créer un élément de travail qui s’exécute une fois le délai du minuteur écoulé.

## <a name="create-a-single-shot-timer"></a>Créer un minuteur à déclenchement unique

Utilisez la méthode [**CreateTimer**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.createtimer) pour créer un minuteur pour l’élément de travail. Fournissez une expression lambda qui effectue la tâche, puis utilisez le paramètre *delay* pour spécifier la durée pendant laquelle le pool de threads attend avant de pouvoir attribuer l’élément de travail à un thread disponible. Le délai est spécifié à l’aide d’une structure [**TimeSpan**](https://docs.microsoft.com/uwp/api/Windows.Foundation.TimeSpan).

> **Note**  You can use [**CoreDispatcher.RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) to access the UI and show progress from the work item.

L’exemple suivant crée un élément de travail qui s’exécute dans trois minutes :

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> TimeSpan delay = TimeSpan.FromMinutes(3);
>             
> ThreadPoolTimer DelayTimer = ThreadPoolTimer.CreateTimer(
>     (source) =>
>     {
>         //
>         // TODO: Work
>         //
>         
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(
>             CoreDispatcherPriority.High,
>             () =>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //
>
>             });
>
>     }, delay);
> ```
> ``` cpp
> TimeSpan delay;
> delay.Duration = 3 * 60 * 10000000; // 10,000,000 ticks per second
>
> ThreadPoolTimer ^ DelayTimer = ThreadPoolTimer::CreateTimer(
>         ref new TimerElapsedHandler([this](ThreadPoolTimer^ source)
>         {
>             //
>             // TODO: Work
>             //
>             
>             //
>             // Update the UI thread by using the UI core dispatcher.
>             //
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([this]()
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>
>                     ExampleUIUpdateMethod("Timer completed.");
>
>                 }));
>
>         }), delay);
> ```

## <a name="provide-a-completion-handler"></a>Fournir un gestionnaire d’achèvement

Si nécessaire, gérez l’annulation et l’achèvement de l’élément de travail avec un objet [**TimerDestroyedHandler**](https://docs.microsoft.com/uwp/api/windows.system.threading.timerdestroyedhandler). Utilisez la surcharge [**CreateTimer**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.createtimer) pour fournir une expression lambda supplémentaire. Celle-ci s’exécute lorsque le minuteur est annulé ou que l’élément de travail se termine.

L’exemple suivant crée un minuteur qui envoie l’élément de travail, puis appelle une méthode lorsque l’élément de travail se termine ou que le minuteur est annulé :

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> TimeSpan delay = TimeSpan.FromMinutes(3);
>             
> bool completed = false;
>
> ThreadPoolTimer DelayTimer = ThreadPoolTimer.CreateTimer(
>     (source) =>
>     {
>         //
>         // TODO: Work
>         //
>
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(
>                 CoreDispatcherPriority.High,
>                 () =>
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>
>                 });
>
>         completed = true;
>     },
>     delay,
>     (source) =>
>     {
>         //
>         // TODO: Handle work cancellation/completion.
>         //
>
>
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(
>             CoreDispatcherPriority.High,
>             () =>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //
>
>                 if (completed)
>                 {
>                     // Timer completed.
>                 }
>                 else
>                 {
>                     // Timer cancelled.
>                 }
>
>             });
>     });
> ```
> ``` cpp
> TimeSpan delay;
> delay.Duration = 3 * 60 * 10000000; // 10,000,000 ticks per second
>
> completed = false;
>
> ThreadPoolTimer ^ DelayTimer = ThreadPoolTimer::CreateTimer(
>         ref new TimerElapsedHandler([&](ThreadPoolTimer ^ source)
>         {
>             //
>             // TODO: Work
>             //
>
>             //
>             // Update the UI thread by using the UI core dispatcher.
>             //
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([&]()
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>
>                 }));
>
>             completed = true;
>
>         }),
>         delay,
>         ref new TimerDestroyedHandler([&](ThreadPoolTimer ^ source)
>         {
>             //
>             // TODO: Handle work cancellation/completion.
>             //
>
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([&]()
>                 {
>                     //
>                     // Update the UI thread by using the UI core dispatcher.
>                     //
>
>                     if (completed)
>                     {
>                         // Timer completed.
>                     }
>                     else
>                     {
>                         // Timer cancelled.
>                     }
>
>                 }));
>         }));
> ```

## <a name="cancel-the-timer"></a>Annuler le minuteur

Si le compte à rebours du minuteur se poursuit alors que l’élément de travail n’est plus nécessaire, appelez [**Cancel**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.cancel). Le minuteur est annulé et l’élément de travail n’est pas envoyé au pool de threads.

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> DelayTimer.Cancel();
> ```
> ``` cpp
> DelayTimer->Cancel();
> ```

## <a name="remarks"></a>Notes

Les applications de plateforme Windows universelle (UWP) ne peuvent pas utiliser **Thread.Sleep**, car cela peut bloquer le thread d’interface utilisateur. Vous pouvez utiliser un objet [**ThreadPoolTimer**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPoolTimer) pour créer un élément de travail à la place. Cela retarde la tâche accomplie par l’élément de travail sans bloquer le thread d’interface utilisateur.

Pour obtenir un exemple de code complet illustrant les éléments de travail, les éléments de travail de minuteur et les éléments de travail périodiques, voir l’[exemple de pool de threads](https://code.msdn.microsoft.com/windowsapps/Pool-Sample-5aa60454). The code sample was originally written for Windows 8.1 but the code can be re-used in Windows 10.

Pour plus d’informations sur la répétition de minuteurs, voir [Créer un élément de travail périodique](create-a-periodic-work-item.md).

## <a name="related-topics"></a>Rubriques connexes

* [Envoyer un élément de travail au pool de threads](submit-a-work-item-to-the-thread-pool.md)
* [Bonnes pratiques pour l’utilisation du pool de threads](best-practices-for-using-the-thread-pool.md)
* [Utiliser un minuteur pour envoyer un élément de travail](use-a-timer-to-submit-a-work-item.md)
 

 
