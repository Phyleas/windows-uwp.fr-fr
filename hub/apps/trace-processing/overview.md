---
title: Qu’est-ce que .NET TraceProcessing-.NET TraceProcessing
description: Dans cette vue d’ensemble, Découvrez le .NET TraceProcessing.
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: overview
ms.openlocfilehash: 03f88d6bd1ac150d94f73f9f9177249c3b5ae78f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170553"
---
# <a name="process-etw-traces-in-net"></a>Traiter les traces ETW dans .NET

[Suivi d’v nements pour Windows (ETW)](/windows/win32/etw/event-tracing-portal) est un système de collecte de traces puissant intégré au système d’exploitation Windows. Windows a une intégration profonde avec ETW, y compris des données sur le comportement du système jusqu’au noyau pour les événements tels que les changements de contexte, l’allocation de mémoire, le processus de création et de sortie, et bien plus encore. Les données à l’échelle du système disponibles à partir d’ETW en font un bon choix pour l’analyse des performances de bout en bout ou d’autres questions qui requièrent la recherche de l’interaction entre un grand nombre de composants dans le système.

Contrairement à la journalisation du texte, ETW fournit des événements structurés conçus pour le traitement automatisé des données. Microsoft a créé des outils puissants en plus de ces événements structurés, notamment [Windows Performance Analyzer (WPA)](/windows-hardware/test/wpt/windows-performance-analyzer), qui fournit une interface graphique pour la visualisation et l’exploration des données de trace capturées dans un fichier de suivi ETW (. ETL).

Au sein de Microsoft, nous utilisons énormément des traces ETW pour mesurer les performances des nouvelles builds de Windows. Compte tenu du volume de données à l’origine du système d’ingénierie de Windows, l’analyse automatisée est essentielle. Pour notre analyse de trace automatisée, nous utilisons beaucoup C# et .NET, nous avons donc créé l' [API .net TraceProcessing](https://www.nuget.org/packages/Microsoft.Windows.EventTracing.Processing.All) pour accéder à de nombreux types de données de trace ETW. Cette technologie est également utilisée dans Windows Performance Analyzer pour alimenter plusieurs de ses tables.

Les packages NuGet .NET TraceProcessing vous permettent d’analyser vos propres applications et systèmes avec les mêmes outils que ceux utilisés par Microsoft pour analyser Windows.

## <a name="next-steps"></a>Étapes suivantes

Dans cette vue d’ensemble, vous avez appris ce que .NET TraceProcessing est.

L’étape suivante consiste à [traiter votre première trace](quickstart.md).

## <a name="related-topics"></a>Rubriques connexes

* [Accéder aux données de trace](tutorial.md)
* [Étendre TraceProcessor](extensibility.md)
* [Charger les symboles](symbols.md)
* [Utiliser la diffusion en continu](streaming.md)
* [Exemples](https://github.com/microsoft/eventtracing-processing-samples)
* [Référence sur l’API](reference.md)