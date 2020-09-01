---
title: Processus de démarrage rapide a trace-.NET TraceProcessing
description: Dans ce guide de démarrage rapide, Découvrez comment accéder aux données dans une trace ETW.
author: maiak
ms.author: maiak
ms.date: 02/20/2020
ms.topic: quickstart
ms.openlocfilehash: 5f8671d8a1490710837908bb4df8f4aa3c99ecdd
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172653"
---
# <a name="quickstart-process-your-first-trace"></a>Démarrage rapide : traiter votre première trace

Essayez TraceProcessor pour accéder aux données d’une trace Suivi d’v nements pour Windows (ETW). TraceProcessor vous permet d’accéder aux données de suivi ETW en tant qu’objets .NET.

Dans ce guide de démarrage rapide, vous allez apprendre à :

1. Installez le package NuGet TraceProcessing.
2. Créez un TraceProcessor.
3. Utilisez TraceProcessor pour accéder aux lignes de commande de processus contenues dans la trace.

## <a name="prerequisites"></a>Prérequis

Visual Studio 2019

## <a name="install-the-traceprocessing-nuget-package"></a>Installer le package NuGet TraceProcessing

.NET TraceProcessing est disponible à partir de [NuGet](https://www.nuget.org/packages/Microsoft.Windows.EventTracing.Processing.All) avec l’ID de package suivant :

Microsoft. Windows. EventTracing. Processing. All

Vous pouvez utiliser ce package dans une application de console pour répertorier les lignes de commande de processus contenues dans une trace ETW (fichier. ETL).

1. Créez une application de console .NET Core. Dans Visual Studio, sélectionnez fichier, nouveau, projet..., puis choisissez le modèle application console (.NET Core) pour C#.

    Entrez un nom de projet, par exemple, TraceProcessorQuickstart, puis choisissez créer.

2. Dans Explorateur de solutions, cliquez avec le bouton droit sur dépendances, puis choisissez gérer les packages NuGet... et basculez vers l’onglet Parcourir.

3. Dans la zone de recherche, entrez Microsoft. Windows. EventTracing. Processing. All, puis recherchez.

    Sélectionnez installer sur le package NuGet portant ce nom, puis fermez la fenêtre NuGet.

## <a name="create-a-traceprocessor"></a>Créer un TraceProcessor

1. Remplacez Program.cs par le contenu suivant :

    ```csharp
    using Microsoft.Windows.EventTracing;
    using Microsoft.Windows.EventTracing.Processes;
    using System;

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
                // TODO: call trace.Use...

                trace.Process();

                Console.WriteLine("TODO: Access data from the trace");
            }
        }
    }
    ```

2. Fournissez un nom de trace à utiliser lors de l’exécution du projet.

    Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet et choisissez Propriétés. Basculez vers l’onglet déboguer et entrez le chemin d’accès à une trace (fichier. ETL) dans arguments de l’application.

    Si vous n’avez pas encore de fichier de trace, vous pouvez utiliser l' [enregistreur de performances Windows](/windows-hardware/test/wpt/start-a-recording) pour en créer un.

3. Exécutez l'application.

    Choisissez déboguer, exécuter sans débogage pour exécuter votre code.

## <a name="use-traceprocessor-to-access-process-command-lines-contained-in-the-trace"></a>Utiliser TraceProcessor pour accéder aux lignes de commande de processus contenues dans la trace

1. Remplacez Program.cs par le contenu suivant :

    ```csharp
    using Microsoft.Windows.EventTracing;
    using Microsoft.Windows.EventTracing.Processes;
    using System;

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
                IPendingResult<IProcessDataSource> pendingProcessData = trace.UseProcesses();

                trace.Process();

                IProcessDataSource processData = pendingProcessData.Result;

                foreach (IProcess process in processData.Processes)
                {
                    Console.WriteLine(process.CommandLine);
                }
            }
        }
    }
    ```

2. Exécutez de nouveau l'application.

    Cette fois, vous devez voir les lignes de commande de liste de tous les processus qui étaient en cours d’exécution pendant l’enregistrement de la trace.

## <a name="next-steps"></a>Étapes suivantes

Dans ce guide de démarrage rapide, vous avez créé une application console, installé TraceProcessor, et vous l’avez utilisée pour accéder aux lignes de commande du processus à partir d’une trace ETW. Vous disposez maintenant d’une application qui accède aux données de trace.

Les informations de processus sont simplement l’un des nombreux types de données stockés dans une trace ETW à laquelle votre application peut accéder.

L’étape suivante consiste à se [rapprocher de TraceProcessor](tutorial.md) et des autres sources de données auxquelles il peut accéder.