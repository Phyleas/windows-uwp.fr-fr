---
title: Extensibilité-.NET TraceProcessing
description: Dans ce didacticiel, Découvrez comment étendre .NET TraceProcessing.
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: overview
ms.openlocfilehash: f5680bdc6502c4b917667e5a59084286b445063c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166733"
---
# <a name="extend-traceprocessor"></a>Étendre TraceProcessor

De nombreux types de données de trace ont une prise en charge intégrée dans [TraceProcessor](/dotnet/api/microsoft.windows.eventtracing.traceprocessor), mais si vous avez vos autres fournisseurs que vous souhaitez analyser (y compris vos propres fournisseurs personnalisés), ces données sont également disponibles à partir de la trace en direct pendant le traitement.

> [!NOTE]
> Cette partie de l’API est en version préliminaire et en développement actif. Cela peut changer dans les versions ultérieures.

Par exemple, voici un moyen simple d’obtenir la liste des ID des fournisseurs dans une trace.

```csharp
// Open a trace with TraceProcessor.Create() and call Run...

static void Run(ITraceProcessor trace)
{
    HashSet<Guid> providerIds = new HashSet<Guid>();
    trace.Use((e) => providerIds.Add(e.ProviderId));
    trace.Process();

    foreach (Guid providerId in providerIds)
    {
        Console.WriteLine(providerId);
    }
}
```

L’exemple suivant montre une source de données personnalisée simplifiée.

```csharp
// Open a trace with TraceProcessor.Create() and call Run...

static void Run(ITraceProcessor trace)
{
    CustomDataSource customDataSource = new CustomDataSource();
    trace.Use(customDataSource);

    trace.Process();

    Console.WriteLine(customDataSource.Count);
}

class CustomDataSource : IFilteredEventConsumer
{
    public IReadOnlyList<Guid> ProviderIds { get; } = new Guid[] { new Guid("your provider ID") };

    public int Count { get; private set; }

    public void Process(EventContext eventContext)
    {
        ++Count;
    }
}
```

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez appris à étendre TraceProcessor.

L’étape suivante consiste à apprendre à [charger des symboles](symbols.md) pour les traces.