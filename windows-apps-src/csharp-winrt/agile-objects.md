---
description: Un objet agile est un objet qui est accessible à partir de n’importe quel thread. C#/WinRT fournit la prise en charge des références agile si vous devez marshaler un objet non agile sur plusieurs cloisonnements de façon sécurisée.
title: Objets agiles avec C#/WinRT
ms.date: 11/17/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a963f1a9918e6732028ad74903b096a38a14ec8f
ms.sourcegitcommit: 3b10880007fe9e29ea2b9305fe62ced239d974be
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2020
ms.locfileid: "96355230"
---
# <a name="agile-objects-in-cwinrt"></a>Objets agile en C#/WinRT

La plupart des Windows Runtime classes sont *agiles*, ce qui signifie qu’elles sont accessibles à partir de n’importe quel thread sur différents cloisonnements. Les types/WinRT C# que vous créez sont agiles par défaut et il n’est pas possible de désactiver ce comportement pour ces types.

Toutefois, les types de/WinRT C# projetés (qui incluent des types de Windows Runtime fournis par le SDK Windows et la bibliothèque WinUI) peuvent ou ne pas être agiles. Par exemple, de nombreux types qui représentent des objets d’interface utilisateur ne sont pas agiles. Lorsque vous consommez des types non agiles, vous devez prendre en considération leur modèle de thread et le comportement de marshaling. C#/WinRT fournit la prise en charge des références agile si vous devez marshaler un objet non agile sur plusieurs cloisonnements de façon sécurisée.

> [!NOTE]
> Le Windows Runtime est basé sur COM. En termes de COM, une classe agile est inscrite avec `ThreadingModel` = *Both*. Pour plus d’informations sur les modèles de thread COM, voir [Présentation et utilisation des modèles de thread COM](/previous-versions/ms809971(v=msdn.10)).

## <a name="check-for-agile-support"></a>Vérifier la prise en charge agile

Pour vérifier si un objet Windows Runtime est agile, utilisez le code suivant pour déterminer si l’objet prend en charge l’interface [IAgileObject](/windows/desktop/api/objidl/nn-objidl-iagileobject) .

```csharp
var queryAgileObject = testObject.As<IAgileObject>();

if (queryAgileObject != null) {
    // testObject is agile.
}
```

## <a name="create-an-agile-reference"></a>Créer une référence agile

Pour créer une référence agile pour un objet non agile, vous pouvez utiliser la `AsAgile` méthode d’extension. `AsAgile` est une méthode d’extension générique qui peut être appliquée à n’importe quel type/WinRT C# projeté. Si le type n’est pas un type projeté, une exception est levée. Voici un exemple d’utilisation d’un objet [PopupMenu](/uwp/api/Windows.UI.Popups.PopupMenu) , qui est un type non agile issu de l’SDK Windows.

```csharp
var nonAgileObj = new Windows.UI.Popups.PopupMenu();
AgileReference<Windows.UI.Popups.PopupMenu> agileReference = nonAgileObj.AsAgile();
```

Vous pouvez maintenant passer `agileReference` à un thread dans un cloisonnement différent et l’utiliser ici.

```csharp
await Task.Run(() => {
        Windows.UI.Popups.PopupMenu nonAgileObjAgain = agileReference.Get()
    });
```
