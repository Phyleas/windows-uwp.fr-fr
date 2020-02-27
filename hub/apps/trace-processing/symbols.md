---
title: Charger les symboles-.NET TraceProcessing
description: Dans ce didacticiel, Découvrez comment charger des symboles lors du traitement des traces.
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: tutorial
ms.openlocfilehash: a6954538159c6fffb3185aa8b3137af26e17b32f
ms.sourcegitcommit: 4fdab7be28aca18cb3879fc205eb49edc4f9a96b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77629090"
---
# <a name="use-symbols-in-net-traceprocessing"></a>Utiliser des symboles dans .NET TraceProcessing

[TraceProcessor](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.traceprocessor) prend en charge le chargement de symboles et l’obtention de piles à partir de plusieurs sources de données. L’application console suivante examine les exemples de processeur et génère la durée estimée d’exécution d’une fonction spécifique (en fonction de l’échantillonnage statistique de la trace de l’utilisation de l’UC).

```csharp
using Microsoft.Windows.EventTracing;
using Microsoft.Windows.EventTracing.Cpu;
using Microsoft.Windows.EventTracing.Symbols;
using System;
using System.Collections.Generic;

class Program
{
    static void Main(string[] args)
    {
        if (args.Length != 3)
        {
            Console.Error.WriteLine("Usage: GetCpuSampleDuration.exe <trace.etl> <imageName> <functionName>");
            return;
        }

        string tracePath = args[0];
        string imageName = args[1];
        string functionName = args[2];
        Dictionary<string, Duration> matchDurationByCommandLine = new Dictionary<string, Duration>();

        using (ITraceProcessor trace = TraceProcessor.Create(tracePath))
        {
            IPendingResult<ISymbolDataSource> pendingSymbolData = trace.UseSymbols();
            IPendingResult<ICpuSampleDataSource> pendingCpuSamplingData = trace.UseCpuSamplingData();

            trace.Process();

            ISymbolDataSource symbolData = pendingSymbolData.Result;
            ICpuSampleDataSource cpuSamplingData = pendingCpuSamplingData.Result;

            symbolData.LoadSymbolsForConsoleAsync(SymCachePath.Automatic, SymbolPath.Automatic).GetAwaiter().GetResult();

            Console.WriteLine();
            IThreadStackPattern pattern = AnalyzerThreadStackPattern.Parse($"{imageName}!{functionName}");

            foreach (ICpuSample sample in cpuSamplingData.Samples)
            {
                if (sample.Stack != null && sample.Stack.Matches(pattern))
                {
                    string commandLine = sample.Process.CommandLine;

                    if (!matchDurationByCommandLine.ContainsKey(commandLine))
                    {
                        matchDurationByCommandLine.Add(commandLine, Duration.Zero);
                    }

                    matchDurationByCommandLine[commandLine] += sample.Weight;
                }
            }

            foreach (string commandLine in matchDurationByCommandLine.Keys)
            {
                Console.WriteLine($"{commandLine}: {matchDurationByCommandLine[commandLine]}");
            }
        }
    }
}
```

L’exécution de ce programme produit une sortie similaire à ce qui suit :

```shell
C:\GetCpuSampleDuration\bin\Debug\> GetCpuSampleDuration.exe C:\boot.etl user32.dll LoadImageInternal
0.0% (0 of 1165; 0 loaded)
<snip>
100.0% (1165 of 1165; 791 loaded)
wininit.exe: 15.99 ms
C:\Windows\Explorer.EXE: 5 ms
winlogon.exe: 20.15 ms
"C:\Users\AdminUAC\AppData\Local\Microsoft\OneDrive\OneDrive.exe" /background: 2.09 ms
```

(Les détails de la sortie varient en fonction de la trace).

## <a name="symbols-format"></a>Format des symboles

En interne, TraceProcessor utilise le format [SymCache](https://docs.microsoft.com/windows-hardware/test/wpt/loading-symbols#symcache-path) , qui est un cache de certaines des données stockées dans un fichier PDB. Lors du chargement de symboles, TraceProcessor requiert la spécification d’un emplacement à utiliser pour ces fichiers SymCache (un chemin SymCache) et prend en charge éventuellement la spécification d’un SymbolPath pour accéder aux fichiers PDB. Quand un SymbolPath est fourni, TraceProcessor crée des fichiers SymCache en dehors des fichiers PDB en fonction des besoins, et le traitement ultérieur des mêmes données peut utiliser les fichiers SymCache directement pour de meilleures performances.

## <a name="next-steps"></a>Étapes suivantes :

Dans ce didacticiel, vous avez appris à charger des symboles lors du traitement des traces.

L’étape suivante consiste à apprendre à [utiliser la diffusion en continu](streaming.md) pour accéder aux données de trace sans mettre en mémoire tampon tous les éléments en mémoire.
