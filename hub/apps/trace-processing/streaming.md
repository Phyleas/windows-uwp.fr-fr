---
title: Utiliser la diffusion en continu-.NET TraceProcessing
description: Dans ce didacticiel, Découvrez comment utiliser la diffusion en continu pour accéder immédiatement aux données de trace et utiliser moins de mémoire.
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: tutorial
ms.openlocfilehash: 6ad0f5977ed4d739ce3133c9e67c0eefc6e0cbd9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168823"
---
# <a name="use-streaming-with-traceprocessor"></a>Utiliser la diffusion en continu avec TraceProcessor

Par défaut, TraceProcessor accède aux données en les chargeant dans la mémoire lors du traitement de la trace. Cette approche de mise en mémoire tampon est facile à utiliser, mais elle peut être coûteuse en termes d’utilisation de la mémoire.

TraceProcessor fournit également une trace. UseStreaming (), qui prend en charge l’accès à plusieurs types de données de trace en continu (traitement des données lors de leur lecture à partir du fichier de trace, plutôt que la mise en mémoire tampon de ces données en mémoire). Par exemple, une trace syscalls peut être très volumineuse et la mise en mémoire tampon de la liste complète des syscalls dans une trace peut être très coûteuse.

## <a name="accessing-buffered-data"></a>Accès aux données mises en mémoire tampon

Le code suivant illustre l’accès aux données syscall dans le sens normal et mis en mémoire tampon par le biais d’une trace. UseSyscalls():

```csharp
using Microsoft.Windows.EventTracing;
using Microsoft.Windows.EventTracing.Processes;
using Microsoft.Windows.EventTracing.Syscalls;
using System;
using System.Collections.Generic;

class Program
{
    static void Main(string[] args)
    {
        if (args.Length != 1)
        {
            Console.Error.WriteLine("Usage: <trace.etl>");
            return;
        }

        using (ITraceProcessor trace = TraceProcessor.Create(args[0]))
        {
            IPendingResult<ISyscallDataSource> pendingSyscallData = trace.UseSyscalls();

            trace.Process();

            ISyscallDataSource syscallData = pendingSyscallData.Result;

            Dictionary<IProcess, int> syscallsPerCommandLine = new Dictionary<IProcess, int>();

            foreach (ISyscall syscall in syscallData.Syscalls)
            {
                IProcess process = syscall.Thread?.Process;

                if (process == null)
                {
                    continue;
                }

                if (!syscallsPerCommandLine.ContainsKey(process))
                {
                    syscallsPerCommandLine.Add(process, 0);
                }

                ++syscallsPerCommandLine[process];
            }

            Console.WriteLine("Process Command Line: Syscalls Count");

            foreach (IProcess process in syscallsPerCommandLine.Keys)
            {
                Console.WriteLine($"{process.CommandLine}: {syscallsPerCommandLine[process]}");
            }
        }
    }
}
```

## <a name="accessing-streaming-data"></a>Accès aux données de streaming

Avec une trace syscalls de grande taille, la tentative de mise en mémoire tampon des données syscall en mémoire peut être très coûteuse, ou il n’est même pas possible. Le code suivant montre comment accéder aux mêmes données syscall en mode de diffusion en continu, en remplaçant la trace. UseSyscalls () avec trace. UseStreaming(). UseSyscalls():

```csharp
using Microsoft.Windows.EventTracing;
using Microsoft.Windows.EventTracing.Processes;
using Microsoft.Windows.EventTracing.Syscalls;
using System;
using System.Collections.Generic;

class Program
{
    static void Main(string[] args)
    {
        if (args.Length != 1)
        {
            Console.Error.WriteLine("Usage: <trace.etl>");
            return;
        }

        using (ITraceProcessor trace = TraceProcessor.Create(args[0]))
        {
            IPendingResult<IThreadDataSource> pendingThreadData = trace.UseThreads();

            Dictionary<IProcess, int> syscallsPerCommandLine = new Dictionary<IProcess, int>();

            trace.UseStreaming().UseSyscalls(ConsumerSchedule.SecondPass, context =>
            {
                Syscall syscall = context.Data;
                IProcess process = syscall.GetThread(pendingThreadData.Result)?.Process;

                if (process == null)
                {
                    return;
                }

                if (!syscallsPerCommandLine.ContainsKey(process))
                {
                    syscallsPerCommandLine.Add(process, 0);
                }

                ++syscallsPerCommandLine[process];
            });

            trace.Process();

            Console.WriteLine("Process Command Line: Syscalls Count");

            foreach (IProcess process in syscallsPerCommandLine.Keys)
            {
                Console.WriteLine($"{process.CommandLine}: {syscallsPerCommandLine[process]}");
            }
        }
    }
}
```

## <a name="how-streaming-works"></a>Fonctionnement de la diffusion en continu

Par défaut, toutes les données de streaming sont fournies lors de la première passe dans la trace, et les données mises en mémoire tampon provenant d’autres sources ne sont pas disponibles. L’exemple ci-dessus montre comment combiner la diffusion en continu avec la mise en mémoire tampon : les données de thread sont mises en mémoire tampon avant que les données syscall ne soient diffusées en continu. Par conséquent, la trace doit être lue deux fois : une fois pour obtenir les données de thread mises en mémoire tampon, et une deuxième fois pour accéder aux données de diffusion en continu syscall avec les données de thread mises en mémoire tampon maintenant disponibles. Pour combiner la diffusion en continu et la mise en mémoire tampon de cette manière, l’exemple passe ConsumerSchedule. SecondPass à la trace. UseStreaming(). UseSyscalls (), ce qui entraîne le traitement de syscall dans une seconde étape de la trace. En s’exécutant dans une deuxième passe, le rappel syscall peut accéder au résultat en attente à partir de trace. UseThreads () lors du traitement de chaque syscall. Sans cet argument facultatif, la diffusion en continu de syscall aurait été exécutée lors de la première passe dans la trace (il n’y aura qu’un seul passage) et le résultat en attente de trace. UseThreads () n’est pas encore disponible. Dans ce cas, le rappel a toujours accès au ThreadId à partir de syscall, mais il n’a pas accès au processus du thread (car le thread de traitement des données de liaison est fourni via d’autres événements qui n’ont peut-être pas encore été traités).

Les principales différences d’utilisation entre la mise en mémoire tampon et la diffusion en continu :

1. La mise en mémoire tampon retourne un [IPendingResult &lt; T &gt; ](/dotnet/api/microsoft.windows.eventtracing.ipendingresult-1)et le résultat qu’elle contient est uniquement disponible avant le traitement de la trace. Une fois que la trace a été traitée, les résultats peuvent être énumérés à l’aide de techniques telles que foreach et LINQ.
2. La diffusion en continu retourne void et accepte à la place un argument de rappel. Elle appelle le rappel une fois que chaque élément est disponible. Étant donné que les données ne sont pas mises en mémoire tampon, il n’y a jamais de liste de résultats à énumérer avec foreach ou LINQ : le rappel de streaming doit mettre en mémoire tampon la partie des données qu’il souhaite enregistrer pour une utilisation une fois le traitement terminé.
3. Le code pour le traitement des données mises en mémoire tampon apparaît après l’appel à trace. Process (), lorsque les résultats en attente sont disponibles.
4. Le code pour le traitement des données de streaming apparaît avant l’appel à trace. Process (), en tant que rappel de la trace. UseStreaming. use... méthode ().
5. Un consommateur de diffusion en continu peut choisir de traiter uniquement une partie du flux et d’annuler les rappels futurs en appelant le contexte. Cancel (). Un consommateur de mise en mémoire tampon est toujours fourni avec une liste de mises en mémoire tampon complète.

## <a name="correlated-streaming-data"></a>Données de diffusion en continu corrélées

Parfois, les données de trace sont fournies dans une séquence d’événements, par exemple, les syscalls sont journalisés par le biais d’événements Enter et Exit distincts, mais les données combinées des deux événements peuvent être plus utiles. Trace de la méthode. UseStreaming(). UseSyscalls () met en corrélation les données de ces deux événements et les fournit lorsque la paire devient disponible. Certains types de données corrélées sont disponibles par le biais d’une trace. UseStreaming():

| Code                                        | Description                                                                                                                                     |
|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| trace. UseStreaming(). UseContextSwitchData() | Diffuse des données de changement de contexte corrélées (à partir d’événements compacts et non compacts, avec des SwitchInThreadIds plus précises que les événements bruts non compacts). |
| trace. UseStreaming(). UseScheduledTasks()    | Diffuse en corrélation les données de tâche planifiée.                                                                                                         |
| trace. UseStreaming(). UseSyscalls()          | Diffuse des données d’appels système corrélés.                                                                                                            |
| trace. UseStreaming(). UseWindowInFocus()     | Diffuse en continu les données de fenêtre dans le focus.                                                                                                        |

## <a name="standalone-streaming-events"></a>Événements de streaming autonomes

En outre, trace. UseStreaming () fournit des événements analysés pour plusieurs types d’événements autonomes :

| Code                                               | Description                                     |
|----------------------------------------------------|-------------------------------------------------|
| trace. UseStreaming(). UseLastBranchRecordEvents()   | Flux analysés par les derniers événements d’enregistrement de branche (LBR). |
| trace. UseStreaming(). UseReadyThreadEvents()        | Flux d’événements de threads prêts analysés.             |
| trace. UseStreaming(). UseThreadCreateEvents()       | Streams a analysé les événements de création de thread.            |
| trace. UseStreaming(). UseThreadExitEvents()         | Événements de sortie de thread analysés par flux.              |
| trace. UseStreaming(). UseThreadRundownStartEvents() | Événements de démarrage de l’arrêt des threads analysés par flux.     |
| trace. UseStreaming(). UseThreadRundownStopEvents()  | Événements d’arrêt d’arrêt de thread analysés par flux.      |
| trace. UseStreaming(). UseThreadSetNameEvents()      | Événements de nom d’ensemble de threads analysés par flux.          |

## <a name="underlying-streaming-events-for-correlated-data"></a>Événements de diffusion en continu sous-jacents pour les données corrélées

Enfin, trace. UseStreaming () fournit également les événements sous-jacents utilisés pour mettre en corrélation des données dans la liste ci-dessus. Ces événements sous-jacents sont les suivants :

| Code                                                        | Description                                                                                | Inclus dans                                 |
|-------------------------------------------------------------|--------------------------------------------------------------------------------------------|---------------------------------------------|
| trace. UseStreaming(). UseCompactContextSwitchEvents()        | Streams a analysé les événements de changement de contexte compact.                                              | trace. UseStreaming(). UseContextSwitchData() |
| trace. UseStreaming(). UseContextSwitchEvents()               | Événements de changement de contexte analysés par flux. SwitchInThreadIds peut ne pas être précis dans certains cas. | trace. UseStreaming(). UseContextSwitchData() |
| trace. UseStreaming(). UseFocusChangeEvents()                 | Flux analysés événements de modification de focus de la fenêtre.                                                 | trace. UseStreaming(). UseWindowInFocus()     |
| trace. UseStreaming(). UseScheduledTaskStartEvents()          | Événements de démarrage de tâche planifiée analysés par flux.                                                | trace. UseStreaming(). UseScheduledTasks()    |
| trace. UseStreaming(). UseScheduledTaskStopEvents()           | Flux analysés événements d’arrêt de tâche planifiée.                                                 | trace. UseStreaming(). UseScheduledTasks()    |
| trace. UseStreaming(). UseScheduledTaskTriggerEvents()        | Événements de déclencheur de tâche planifiée analysés par flux.                                              | trace. UseStreaming(). UseScheduledTasks()    |
| trace. UseStreaming(). UseSessionLayerSetActiveWindowEvents() | Streams a analysé les événements de la fenêtre active de la couche de session.                                     | trace. UseStreaming(). UseWindowInFocus()     |
| trace. UseStreaming(). UseSyscallEnterEvents()                | Streams analysés syscall entrez des événements.                                                       | trace. UseStreaming(). UseSyscalls()          |
| trace. UseStreaming(). UseSyscallExitEvents()                 | Événements de sortie syscall analysés par flux.                                                        | trace. UseStreaming(). UseSyscalls()          |

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez appris à utiliser la diffusion en continu pour accéder immédiatement aux données de trace et à utiliser moins de mémoire.

L’étape suivante consiste à Rechercher l’accès aux données souhaitées à partir de vos traces. Examinez les [exemples](https://github.com/microsoft/eventtracing-processing-samples) pour obtenir quelques idées. Notez que toutes les traces n’incluent pas tous les types de données pris en charge.