---
ms.assetid: FA25562A-FE62-4DFC-9084-6BD6EAD73636
title: Assurer la réactivité du thread de l’interface utilisateur
description: Les utilisateurs attendent de leurs applications qu’elles restent réactives pendant les opérations de calcul, et ce sur n’importe quel type d’ordinateur.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ba35ae10333481f36f68d666abe5b6bf3b1355fe
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166163"
---
# <a name="keep-the-ui-thread-responsive"></a>Assurer la réactivité du thread de l’interface utilisateur


Les utilisateurs attendent de leurs applications qu’elles restent réactives pendant les opérations de calcul, et ce sur n’importe quel type d’ordinateur. Les implications sont différentes selon les applications. Pour certaines applications, cela implique de fournir des propriétés physiques plus réalistes, d’accélérer le chargement des données à partir d’un disque ou du Web, de représenter des scènes complexes et naviguer entre les pages plus vite, de trouver un itinéraire en un clin d’œil ou encore de traiter rapidement les données. Quel que soit le type de calcul effectué, les utilisateurs veulent que leur application continue de réagir à leurs interactions et qu’elle ne donne pas l’impression de ne plus répondre pendant l’opération de calcul.

Votre application est pilotée par les événements, ce qui signifie que votre code exécute des tâches en réponse à un événement, puis reste inactif jusqu’à l’événement suivant. L’ensemble du code de la plateforme pour l’interface utilisateur (disposition, entrée, déclenchement d’événements, etc.) et du code de votre application pour l’interface utilisateur est exécuté sur le même thread d’interface utilisateur. Une seule instruction peut être exécutée sur ce thread. Par conséquent, si le code de votre application met trop de temps à traiter un événement, l’infrastructure ne peut pas implémenter de disposition ou déclencher de nouveaux événements en réponse à une interaction utilisateur. La réactivité de votre application est liée à la disponibilité du thread d’interface utilisateur pour exécuter les tâches.

Vous devez utiliser le thread d’interface utilisateur pour apporter la quasi-totalité des modifications au thread d’interface utilisateur, notamment la création de types d’interface utilisateur et l’accès à leurs membres. Vous ne pouvez pas mettre à jour l’interface utilisateur à partir d’un thread en arrière-plan, mais vous pouvez y publier un message avec la méthode [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) pour que le code soit exécuté sur ce thread.

> **Remarque**  La seule exception est qu'il existe un thread de rendu distinct capable d'appliquer des modifications d'interface utilisateur qui n'affecteront pas la façon dont l'entrée est gérée ni la disposition de base. Par exemple, de nombreuses animations et transitions qui n’affectent pas la disposition peuvent être exécutées sur ce thread de rendu.

## <a name="delay-element-instantiation"></a>Retarder l’instanciation des éléments

Certaines des étapes les plus lentes dans une application incluent le démarrage et le changement d’affichage. N’obligez pas l’application à faire plus de travail que nécessaire pour afficher l’interface utilisateur que l’utilisateur voit initialement. Par exemple, ne créez pas l’interface utilisateur de manière à révéler progressivement des éléments d’interface et le contenu des fenêtres contextuelles.

-   Utilisez [x:Load attribute](../xaml-platform/x-load-attribute.md) ou [x:DeferLoadStrategy](../xaml-platform/x-deferloadstrategy-attribute.md) pour différer l'instanciation des éléments.
-   Insérez les éléments par programme dans l’arborescence, à la demande.

Les files d'attente [**CoreDispatcher.RunIdleAsync**](/uwp/api/windows.ui.core.coredispatcher.runidleasync) fonctionnent pour le traitement par le thread d'interface utilisateur lorsque ce dernier n'est pas occupé.

## <a name="use-asynchronous-apis"></a>Utiliser des API asynchrones

Pour que l’application reste réactive, la plateforme fournit des versions asynchrones de plusieurs de ses API. Une API asynchrone permet de garantir que votre thread d’exécution actif n’est jamais bloqué pendant un laps de temps significatif. Si vous appelez une API à partir du thread d’interface utilisateur, utilisez la version asynchrone si elle est disponible. Pour plus d’informations sur la programmation avec des modèles **async**, voir [Programmation asynchrone](../threading-async/asynchronous-programming-universal-windows-platform-apps.md) ou [Appel d’API asynchrones en C# ou Visual Basic](../threading-async/call-asynchronous-apis-in-csharp-or-visual-basic.md).

## <a name="offload-work-to-background-threads"></a>Décharger les tâches sur les threads en arrière-plan

Codez les gestionnaires d’événements de sorte qu’ils effectuent rapidement leur retour. Dans les cas où une quantité considérable de tâches doivent être effectuées, planifiez-les sur un thread en arrière-plan et revenez.

Vous pouvez planifier les tâches de manière asynchrone en utilisant l’opérateur **await** dans C#, l’opérateur **Await** dans Visual Basic ou des délégués dans C++. Cependant, cela ne garantit pas que les tâches ainsi planifiées seront toujours exécutées sur un thread en arrière-plan. La plupart des API de plateforme Windows universelle (UWP) planifient automatiquement les tâches sur le thread en arrière-plan, mais si vous appelez le code de votre application seulement avec l’opérateur **await** ou un délégué, ce délégué ou cette méthode s’exécute sur le thread de l’interface utilisateur. Vous devez indiquer explicitement que vous voulez exécuter le code de votre application sur un thread en arrière-plan. En C# et en Visual Basic, vous avez la possibilité de le faire en transmettant le code à [**Task.Run**](/dotnet/api/system.threading.tasks.task.run).

Gardez à l’esprit que les éléments d’interface utilisateur sont accessibles uniquement à partir du thread d’interface utilisateur. Utilisez le thread d’interface utilisateur pour accéder aux éléments d’interface utilisateur avant de lancer le travail en arrière-plan et/ou utilisez [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) ou [**CoreDispatcher.RunIdleAsync**](/uwp/api/windows.ui.core.coredispatcher.runidleasync) sur le thread d’arrière-plan.

Une opération de calcul d’intelligence artificielle dans un jeu constitue un exemple de travail pouvant être effectué sur un thread d’arrière-plan. L’exécution du code qui calcule l’action suivante de l’ordinateur peut prendre un temps considérable.

```csharp
public class AsyncExample
{
    private async void NextMove_Click(object sender, RoutedEventArgs e)
    {
        // The await causes the handler to return immediately.
        await System.Threading.Tasks.Task.Run(() => ComputeNextMove());
        // Now update the UI with the results.
        // ...
    }

    private async System.Threading.Tasks.Task ComputeNextMove()
    {
        // Perform background work here.
        // Don't directly access UI elements from this method.
    }
}
```

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public class Example
> {
>     // ...
>     private async void NextMove_Click(object sender, RoutedEventArgs e)
>     {
>         await Task.Run(() => ComputeNextMove());
>         // Update the UI with results
>     }
> 
>     private async Task ComputeNextMove()
>     {
>         // ...
>     }
>     // ...
> }
> ```
> ```vb
> Public Class Example
>     ' ...
>     Private Async Sub NextMove_Click(ByVal sender As Object, ByVal e As RoutedEventArgs)
>         Await Task.Run(Function() ComputeNextMove())
>         ' update the UI with results
>     End Sub
> 
>     Private Async Function ComputeNextMove() As Task
>         ' ...
>     End Function
>     ' ...
> End Class
> ```

Dans cet exemple, le gestionnaire `NextMove_Click` effectue son retour lorsqu’il reçoit l’instruction **await** afin d’assurer la réactivité du thread d’interface utilisateur. Toutefois, l’exécution reprend dans ce gestionnaire à la fin de l’opération `ComputeNextMove` (qui est exécutée sur un thread en arrière-plan). Le reste du code du gestionnaire permet de mettre à jour l’interface utilisateur avec les résultats.

> **Remarque**  Il existe également pour UWP des API [**ThreadPool**](/uwp/api/Windows.System.Threading.ThreadPool) et [**ThreadPoolTimer**](/uwp/api/windows.system.threading.threadpooltimer) qui peuvent être utilisées dans des scénarios similaires. Pour plus d’informations, voir [Threads et programmation asynchrone](../threading-async/index.md).

## <a name="related-topics"></a>Rubriques connexes

* [Interactions utilisateur personnalisées](../design/layout/index.md)