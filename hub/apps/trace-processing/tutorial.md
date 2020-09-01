---
title: Données de trace d’accès-.NET TraceProcessing
description: Dans ce didacticiel, Découvrez comment accéder aux données de trace à l’aide de TraceProcessor.
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: tutorial
ms.openlocfilehash: ef4d3df6e5a5dd93dbcb2caadc8e3f299aad581c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173693"
---
# <a name="access-trace-data"></a>Accéder aux données de trace

.NET TraceProcessing est disponible à partir de [NuGet](https://www.nuget.org/packages/Microsoft.Windows.EventTracing.Processing.All) avec l’ID de package suivant :

Microsoft. Windows. EventTracing. Processing. All

Ce package vous permet d’accéder aux données d’un fichier de trace. Si vous n’avez pas encore de fichier de trace, vous pouvez utiliser l' [enregistreur de performances Windows](/windows-hardware/test/wpt/start-a-recording) pour en créer un.

L’exemple d’application console suivant montre comment accéder aux lignes de commande de tous les processus contenus dans la trace :

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

## <a name="using-traceprocessor"></a>Utilisation de TraceProcessor

Pour traiter une trace, appelez [TraceProcessor. Create](/dotnet/api/microsoft.windows.eventtracing.traceprocessor.create). L’interface principale est [ITraceProcessor](/dotnet/api/microsoft.windows.eventtracing.itraceprocessor)et l’utilisation de cette interface implique le modèle suivant :

1. Tout d’abord, indiquez au processeur les données que vous souhaitez utiliser à partir d’une trace
2. Ensuite, traitez la trace. les
3. Enfin, accédez aux résultats.

En indiquant au processeur les genres de données que vous voulez, vous n’avez pas besoin de consacrer du temps à traiter de gros volumes de tous les types de données de trace possibles. Au lieu de cela, [TraceProcessor](/dotnet/api/microsoft.windows.eventtracing.traceprocessor) effectue simplement le travail nécessaire pour fournir les types de données spécifiques que vous demandez.

## <a name="recommended-project-settings"></a>Paramètres de projet recommandés

Il existe deux paramètres de projet que nous vous recommandons d’utiliser avec TraceProcessor :

1. Nous vous recommandons d’exécuter les fichiers exe comme 64 bits.

    La valeur par défaut de Visual Studio pour une nouvelle application console de .NET Framework C# est Any CPU avec l’option préférer 32 bits activée. La valeur par défaut pour .NET Core est peut-être déjà la valeur recommandée.

    Le traitement des traces peut nécessiter beaucoup de mémoire, surtout avec des traces plus importantes. nous vous recommandons de changer la plateforme cible en x64 (ou de décocher préférer 32 bits) dans les fichiers exe qui utilisent TraceProcessor. Pour modifier ces paramètres, reportez-vous à l’onglet générer sous propriétés du projet. Pour modifier ces paramètres pour toutes les configurations, assurez-vous que la liste déroulante Configuration est définie sur toutes les configurations, plutôt que sur la configuration par défaut uniquement.

2. Nous vous suggérons d’utiliser NuGet avec le mode PackageReference de style plus récent plutôt que l’ancien mode de packages.config.

    Pour modifier la valeur par défaut pour les nouveaux projets, consultez outils, gestionnaire de package NuGet, paramètres du gestionnaire de package, Package Management, format de gestion des packages par défaut.

## <a name="built-in-data-sources"></a>Sources de données intégrées

Un fichier. etl peut capturer de nombreux types de données dans une trace. Notez que les données qui se trouvent dans un fichier. etl dépendent des fournisseurs qui ont été activés lors de la capture de la trace. La liste suivante répertorie les types de données de trace disponibles à partir de TraceProcessor :

| Code                                      | Description                                                                                                                | Éléments WPA associés                                                    |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------|
| trace. UseClassicEvents()                  | Fournit des événements ETW classiques à partir d’une trace, qui n’incluent pas d’informations de schéma.                                         | Table des événements génériques (quand le type d’événement est Classic ou WPP)             |
| trace. UseConnectedStandbyData()           | Fournit des données à partir d’une trace sur le système entrant et sortant de la veille connectée.                                        | Tableau de synthèse CS                                                     |
| trace. UseCpuIdleStates()                  | Fournit des données à partir d’une trace sur les États C de l’UC.                                                                             | Table des États d’inactivité de l’UC (lorsque le type est actual)                          |
| trace. UseCpuSamplingData()                | Fournit des données à partir d’une trace sur l’utilisation de l’UC en fonction de l’échantillonnage périodique du pointeur d’instruction.                          | Table utilisation de l’UC (échantillonnée)                                            |
| trace. UseCpuSchedulingData()              | Fournit des données à partir d’une trace sur la planification des threads de l’UC, y compris les changements de contexte et les événements de thread Ready.                | Table de l’utilisation de l’UC (précise)                                            |
| trace. UseDevicePowerData()                | Fournit des données à partir d’une trace sur l’appareil D-States.                                                                          | Table DState de l’appareil                                                  |
| trace. UseDirectXData()                    | Fournit des données à partir d’une trace sur l’activité DirectX.                                                                         | Table d’utilisation du GPU                                                |
| traceUseDiskIOData()                      | Fournit des données à partir d’une trace sur l’activité d’e/s disque.                                                                        | Table utilisation du disque                                                     |
| trace. UseEnergyEstimationData()           | Fournit des données à partir d’une trace sur l’utilisation d’énergie estimée par processus du moteur d’estimation d’énergie.                         | Tableau récapitulatif du moteur d’estimation de l’énergie (par processus)                  |
| trace. UseEnergyMeterData()                | Fournit des données à partir d’une trace sur l’utilisation d’énergie mesurée à partir de l’interface de compteur d’énergie (EMI).                                  | Table du moteur d’estimation de l’énergie (par EMI)                              |
| trace. UseFileIOData()                     | Fournit des données à partir d’une trace sur l’activité d’e/s de fichier.                                                                        | Table des e/s de fichier                                                       |
| trace. UseGenericEvents()                  | Fournit des événements manifestes et TraceLogging à partir d’une trace.                                                                  | Table des événements génériques (quand le type d’événement est manifested ou TraceLogging) |
| trace. UseHandles()                        | Fournit des données partielles d’une trace sur les handles de noyau actifs.                                                            | Table Handles                                                        |
| trace. UseHardFaults()                     | Fournit des données à partir d’une trace sur les défauts de page matériels.                                                                         | Table des défauts matériels                                                    |
| trace. UseHeapSnapshots()                  | Fournit des données à partir d’une trace sur l’utilisation du tas de processus.                                                                       | Table d’instantanés du tas                                                  |
| trace. UseHypercalls()                     | Fournit des données sur les hyperappels Hyper-V qui s’est produite pendant une trace.                                                        |                                                                      |
| trace. UseImageSections()                  | Fournit des données à partir d’une trace sur les sections d’une image.                                                                 | Colonne nom de la section de la table utilisation de l’UC (échantillonnée)                 |
| trace. UseInterruptHandlingData()          | Fournit des données à partir d’un suivi à propos de la routine d’interruption (ISR) et de l’activité d’appel de procédure différée (DPC).               | Table DPC/ISR                                                        |
| trace. UseMarks()                          | Fournit les marques (horodateurs étiquetés) à partir d’une trace.                                                                      | Marque la table                                                          |
| trace. UseMemoryUtilizationData()          | Fournit des données à partir d’une trace sur l’utilisation totale de la mémoire système.                                                          | Table d’utilisation de la mémoire                                             |
| trace. UseMetadata()                       | Fournit des métadonnées de suivi disponibles sans traitement supplémentaire.                                                              | Configuration système, traces et général                             |
| trace. UsePlatformIdleStates()             | Fournit des données à partir d’une trace sur les États d’inactivité de la plateforme cible et réelle d’un système.                                   | Table d’état d’inactivité de la plateforme                                            |
| trace. UsePoolAllocations()                | Fournit des données à partir d’une trace sur l’utilisation de la mémoire du pool de noyaux.                                                                 | Tableau récapitulatif du pool                                                   |
| trace. UsePowerConfigurationData()         | Fournit des données à partir d’une trace sur la configuration de l’alimentation du système.                                                               | Configuration du système, paramètres d’alimentation                                 |
| trace. UsePowerDependencyCoordinatorData() | Fournit des données à partir d’une trace sur les phases actives du coordinateur de dépendances d’alimentation.                                               | Tableau de synthèse de la phase de notification                                     |
| trace. UseProcesses()                      | Fournit des données sur les processus actifs pendant une trace, ainsi que leurs images et PDB.                                      | Table des processus ; Table images ; Hub de symboles                           |
| trace. UseProcessorCounters()              | Fournit des données à partir d’une trace sur les valeurs de compteur de performance du processeur à partir de l’analyseur de compteur de processeur (PCM).                |                                                                      |
| trace. UseProcessorFrequencyData()         | Fournit des données à partir d’une trace sur la fréquence à laquelle les processeurs ont été exécutés.                                                    | Table de fréquence du processeur (lorsque le type est réel)                      |
| trace. UseProcessorProfileData()           | Fournit des données à partir d’une trace sur le profil d’alimentation du processeur actif.                                                       | Tableau des profils de processeur                                             |
| trace. UseProcessorParkingData()           | Fournit des données à partir d’une trace sur les processeurs qui ont été immobilisés ou désimmobilisés.                                                 | Table d’état de parking du processeur                                        |
| trace. UseProcessorParkingLimits()         | Fournit des données à partir d’une trace sur le nombre maximal autorisé de processeurs désimmobilisés.                                        | Table d’état de l’embout de parking Core                                         |
| trace. UseProcessorQualityOfServiceData()  | Fournit des données à partir d’une trace sur la qualité de niveau de service pour chaque processeur.                                          | Table de classe de QoS du processeur                                            |
| trace. UseProcessorThrottlingData()        | Fournit des données à partir d’une trace sur la limitation de la fréquence maximale du processeur.                                                   | Tableau des contraintes du processeur                                          |
| trace. UseReadyBootData()                  | Fournit des données à partir d’une trace sur l’activité de prérécupération du démarrage à partir du démarrage prêt.                                                | Table des événements de démarrage prêts                                              |
| trace. UseReferenceSetData()               | Fournit des données à partir d’une trace sur les pages de mémoire virtuelle utilisées par chaque processus.                                             | Table de jeu de références                                                  |
| trace. UseRegionsOfInterest()              | Fournit des régions nommées d’intervalles d’intérêt à partir d’une trace, comme spécifié dans un fichier de configuration XML.                       | Table des régions d’intérêt                                            |
| trace. UseRegistryData()                   | Fournit des données sur l’activité du Registre pendant une trace.                                                                      | Table du Registre                                                       |
| trace. UseResidentSetData()                | Fournit des données à partir d’une trace sur les pages de la mémoire virtuelle pour chaque processus qui résident dans la mémoire physique.       | Table de jeux résidents                                                   |
| trace. UseRundownData()                    | Fournit des données à partir d’une trace sur des intervalles pendant lesquels la collecte des données d’arrêt du suivi s’est produite.                            | Régions ombrées dans la chronologie du graphique                                 |
| trace. UseScheduledTasks()                 | Fournit des données sur les tâches planifiées qui ont été exécutées pendant une trace.                                                               | Table des tâches planifiées                                                |
| trace. UseServices()                       | Fournit des données sur les services qui étaient actifs ou dont l’État a été capturé au cours d’une trace.                                  | Table services ; Configuration système, services                       |
| trace. UseStacks()                         | Fournit des données sur les piles enregistrées pendant une trace.                                                                        |                                                                      |
| trace. UseStackEvents()                    | Fournit des données sur les événements associés aux piles enregistrées pendant une trace.                                                 | Table Stacks                                                         |
| trace. UseStackTags()                      | Fournit un mappeur qui regroupe les piles d’une trace dans des balises de pile, comme spécifié dans un fichier de configuration XML.               | Colonnes telles que la balise Stack et Stack (balises Frame)                     |
| trace. UseSymbols()                        | Offre la possibilité de charger des symboles pour une trace.                                                                          | Configurer les chemins d’accès aux symboles ; Charger les symboles                                 |
| trace. UseSyscalls()                       | Fournit des données sur les syscalls qui se sont produites pendant une trace.                                                                 | Table syscalls                                                       |
| trace. UseSystemMetadata()                 | Fournit des métadonnées générales à l’ensemble du système à partir d’une trace.                                                                       | Configuration du système                                                 |
| trace. UseSystemPowerSourceData()          | Fournit des données à partir d’une trace sur la source d’alimentation du système active (AC ou DC).                                                | Table source d’alimentation du système                                            |
| trace. UseSystemSleepData()                | Fournit des données à partir d’une trace sur l’état d’alimentation global du système.                                                               | Table de transition d’alimentation                                               |
| trace. UseTargetCpuIdleStates()            | Fournit des données à partir d’une trace sur les États C de l’UC cible.                                                                      | Table des États d’inactivité de l’UC (quand le type est target)                          |
| trace. UseTargetProcessorFrequencyData()   | Fournit des données à partir d’une trace sur les fréquences du processeur cible.                                                             | Table de fréquence du processeur (lorsque le type est target)                      |
| trace. UseThreads()                        | Fournit des données sur les threads actifs pendant une trace.                                                                         | Tableau des durées de vie des threads                                               |
| trace. UseTraceStatistics()                | Fournit des statistiques sur les événements d’une trace.                                                                           | Configuration système, statistiques de suivi                               |
| trace. UseUtcData()                        | Fournit des données à partir d’une trace sur l’activité de télémétrie Microsoft à l’aide du client de télémétrie universel (UTC).                      | Table UTC                                                            |
| trace. UseWindowInFocus()                  | Fournit des données à partir d’une trace sur les modifications apportées à la fenêtre active de l’interface utilisateur.                                                 | Fenêtre dans la table Focus                                                |
| trace. UseWindowsTracePreprocessorEvents() | Fournit des événements de préprocesseur de trace du logiciel Windows à partir d’une trace.                                                    | Table de trace WPP ; Table des événements génériques (lorsque le type d’événement est WPP)       |
| trace. UseWinINetData()                    | Fournit des données à partir d’une trace sur l’activité Internet via Windows Internet (WinINet).                                         | Tableau des détails du téléchargement                                               |
| trace. UseWorkingSetData()                 | Fournit des données à partir d’une trace sur les pages de mémoire virtuelle qui se trouvaient dans la plage de travail de chaque processus ou catégorie de noyau. | Table des instantanés de la mémoire virtuelle                                       |

Consultez également les méthodes d’extension sur [ITraceSource](/dotnet/api/microsoft.windows.eventtracing.itracesource) pour toutes les données de trace disponibles, ou examinez la méthode disponible dans « trace ». affiché par IntelliSense.

## <a name="next-steps"></a>Étapes suivantes

Dans cette vue d’ensemble, vous avez appris à accéder aux données de trace à l’aide de TraceProcessor et des sources de données intégrées auxquelles ils peuvent accéder.

L’étape suivante consiste à apprendre à [étendre](extensibility.md) TraceProcessor pour accéder aux données de trace personnalisées.