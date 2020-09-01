---
title: Utilisation d’objets Windows Runtime dans un environnement multithread | Microsoft Docs
description: Cet article décrit la façon dont le .NET Framework gère les appels de code C# et Visual Basic à des objets fournis par le Windows Runtime ou par Windows Runtime composants.
ms.date: 01/14/2017
ms.topic: article
ms.assetid: 43ffd28c-c4df-405c-bf5c-29c94e0d142b
keywords: Windows 10, UWP, minuterie, threads
ms.localizationpriority: medium
ms.openlocfilehash: 4287ed5fd5659b7d39d52ada9e3958c592771d59
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164133"
---
# <a name="using-windows-runtime-objects-in-a-multithreaded-environment"></a>Utilisation des objets Windows Runtime dans un environnement multithread
Cet article décrit la façon dont le .NET Framework gère les appels de code C# et Visual Basic à des objets fournis par le Windows Runtime ou par Windows Runtime composants.

Dans le .NET Framework, vous pouvez accéder à n’importe quel objet à partir de plusieurs threads par défaut, sans traitement spécial. Vous avez simplement besoin d’une référence à l’objet. Dans le Windows Runtime, ces objets sont appelés *agile*. La plupart des Windows Runtime classes sont agiles, mais certaines classes ne le sont pas, et même les classes agile peuvent nécessiter un traitement spécial.

Dans la mesure du possible, le common language runtime (CLR) traite les objets d’autres sources, tels que le Windows Runtime, comme s’ils étaient .NET Framework objets :

- Si l’objet implémente l’interface [IAgileObject](/windows/desktop/api/objidl/nn-objidl-iagileobject) , ou a l’attribut [MarshalingBehaviorAttribute](/uwp/api/Windows.Foundation.Metadata.MarshalingBehaviorAttribute) avec [MARSHALINGTYPE. agile](/uwp/api/Windows.Foundation.Metadata.MarshalingType), le CLR le traite comme agile.

- Si le CLR peut marshaler un appel à partir du thread sur lequel il a été effectué dans le contexte de thread de l’objet cible, il le fait en toute transparence.

- Si l’objet a l’attribut [MarshalingBehaviorAttribute](/uwp/api/Windows.Foundation.Metadata.MarshalingBehaviorAttribute) avec [MarshalingType. None](/uwp/api/Windows.Foundation.Metadata.MarshalingType), la classe ne fournit pas d’informations de marshaling. Le CLR ne peut pas marshaler l’appel. il lève donc une exception [InvalidCastException](/dotnet/api/system.invalidcastexception) avec un message indiquant que l’objet peut être utilisé uniquement dans le contexte de thread dans lequel il a été créé.

Les sections suivantes décrivent les effets de ce comportement sur les objets de diverses sources.

## <a name="objects-from-a-windows-runtime-component-that-is-written-in-c-or-visual-basic"></a>Objets d’un Windows Runtime composant écrit en C# ou Visual Basic
Tous les types du composant qui peuvent être activés sont agiles par défaut.

> [!NOTE]
>  L’agilité n’implique pas la sécurité des threads. Dans l’Windows Runtime et le .NET Framework, la plupart des classes ne sont pas thread-safe, car la sécurité des threads a un coût de performance et la plupart des objets ne sont jamais accessibles par plusieurs threads. Il est plus efficace de synchroniser l’accès à des objets individuels (ou d’utiliser des classes thread-safe) uniquement en cas de besoin.

Lorsque vous créez un composant Windows Runtime, vous pouvez remplacer la valeur par défaut. Consultez l’interface [ICustomQueryInterface](/dotnet/api/system.runtime.interopservices.icustomqueryinterface) et l’interface [IAgileObject](/windows/desktop/api/objidl/nn-objidl-iagileobject) .

## <a name="objects-from-the-windows-runtime"></a>Objets de la Windows Runtime
La plupart des classes dans le Windows Runtime sont agiles et le CLR les traite comme agile. La documentation pour ces classes répertorie « MarshalingBehaviorAttribute(Agile) » parmi les attributs de classe. Toutefois, les membres de certaines de ces classes agiles, tels que les contrôles XAML, lèvent des exceptions s’ils ne sont pas appelés sur le thread d’interface utilisateur. Par exemple, le code suivant tente d’utiliser un thread d’arrière-plan pour définir une propriété du bouton sur lequel l’utilisateur a cliqué. La propriété de [contenu](/uwp/api/Windows.UI.Xaml.Controls.ContentControl) du bouton lève une exception.

```csharp
private async void Button_Click_2(object sender, RoutedEventArgs e)
{
    Button b = (Button) sender;
    await Task.Run(() => {
        b.Content += ".";
    });
}
```

```vb
Private Async Sub Button_Click_2(sender As Object, e As RoutedEventArgs)
    Dim b As Button = CType(sender, Button)
    Await Task.Run(Sub()
                       b.Content &= "."
                   End Sub)
End Sub
```

Vous pouvez accéder au bouton en toute sécurité à l’aide de sa propriété [Dispatcher](/uwp/api/Windows.UI.Xaml.DependencyObject) ou `Dispatcher` de n’importe quel objet qui existe dans le contexte du thread d’interface utilisateur (tel que la page sur laquelle se trouve le bouton). Le code suivant utilise la méthode [RunAsync](/uwp/api/Windows.UI.Core.CoreDispatcher) de l’objet [CoreDispatcher](/uwp/api/Windows.UI.Core.CoreDispatcher) pour distribuer l’appel sur le thread d’interface utilisateur.

```csharp
private async void Button_Click_2(object sender, RoutedEventArgs e)
{
    Button b = (Button) sender;
    await b.Dispatcher.RunAsync(
        Windows.UI.Core.CoreDispatcherPriority.Normal,
        () => {
            b.Content += ".";
    });
}

```

```vb
Private Async Sub Button_Click_2(sender As Object, e As RoutedEventArgs)
    Dim b As Button = CType(sender, Button)
    Await b.Dispatcher.RunAsync(
        Windows.UI.Core.CoreDispatcherPriority.Normal,
        Sub()
            b.Content &= "."
        End Sub)
End Sub
```

> [!NOTE]
>  La `Dispatcher` propriété ne lève pas d’exception lorsqu’elle est appelée à partir d’un autre thread.

La durée de vie d’un objet Windows Runtime créé sur le thread d’interface utilisateur est limitée par la durée de vie du thread. N’essayez pas d’accéder aux objets sur un thread d’interface utilisateur après la fermeture de la fenêtre.

Si vous créez votre propre contrôle en héritant un contrôle XAML ou en composant un ensemble de contrôles XAML, votre contrôle est agile, car il s’agit d’un objet .NET Framework. Toutefois, s’il appelle des membres de sa classe de base ou des classes qui le composent, ou si vous appelez des membres hérités, ces membres lèvent des exceptions quant ils sont appelés à partir de tout thread autre que le thread d’interface utilisateur.

### <a name="classes-that-cant-be-marshaled"></a>Classes qui ne peuvent pas être marshalées
Les classes Windows Runtime qui ne fournissent pas d’informations de marshaling ont l’attribut [MarshalingBehaviorAttribute](/uwp/api/Windows.Foundation.Metadata.MarshalingBehaviorAttribute) avec [MarshalingType. None](/uwp/api/Windows.Foundation.Metadata.MarshalingType). La documentation pour cette classe répertorie « MarshalingBehaviorAttribute(None) » parmi ses attributs.

Le code suivant crée un objet [CameraCaptureUI](/uwp/api/Windows.Media.Capture.CameraCaptureUI) sur le thread d’interface utilisateur, puis tente de définir une propriété de l’objet à partir d’un thread de pool de threads. Le CLR ne peut pas marshaler l’appel et lève une exception [System. InvalidCastException](/dotnet/api/system.invalidcastexception) avec un message indiquant que l’objet peut être utilisé uniquement dans le contexte de thread dans lequel il a été créé.

```csharp
Windows.Media.Capture.CameraCaptureUI ccui;

private async void Button_Click_1(object sender, RoutedEventArgs e)
{
    ccui = new Windows.Media.Capture.CameraCaptureUI();

    await Task.Run(() => {
        ccui.PhotoSettings.AllowCropping = true;
    });
}

```

```vb
Private ccui As Windows.Media.Capture.CameraCaptureUI

Private Async Sub Button_Click_1(sender As Object, e As RoutedEventArgs)
    ccui = New Windows.Media.Capture.CameraCaptureUI()

    Await Task.Run(Sub()
                       ccui.PhotoSettings.AllowCropping = True
                   End Sub)
End Sub
```

La documentation de [CameraCaptureUI](/uwp/api/Windows.Media.Capture.CameraCaptureUI) répertorie également « THREADINGATTRIBUTE (STA) » parmi les attributs de la classe, car elle doit être créée dans un contexte à thread unique, comme le thread d’interface utilisateur.

Si vous souhaitez accéder à l’objet [CameraCaptureUI](/uwp/api/Windows.Media.Capture.CameraCaptureUI) à partir d’un autre thread, vous pouvez mettre en cache l’objet [CoreDispatcher](/uwp/api/Windows.UI.Core.CoreDispatcher) pour le thread d’interface utilisateur et l’utiliser ultérieurement pour distribuer l’appel sur ce thread. Vous pouvez également obtenir le répartiteur à partir d’un objet XAML tel que la page, comme illustré dans le code suivant.

```csharp
Windows.Media.Capture.CameraCaptureUI ccui;

private async void Button_Click_3(object sender, RoutedEventArgs e)
{
    ccui = new Windows.Media.Capture.CameraCaptureUI();

    await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
        () => {
            ccui.PhotoSettings.AllowCropping = true;
        });
}

```

```vb
Dim ccui As Windows.Media.Capture.CameraCaptureUI

Private Async Sub Button_Click_3(sender As Object, e As RoutedEventArgs)

    ccui = New Windows.Media.Capture.CameraCaptureUI()

    Await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                                Sub()
                                    ccui.PhotoSettings.AllowCropping = True
                                End Sub)
End Sub
```

## <a name="objects-from-a-windows-runtime-component-that-is-written-in-c"></a>Objets d’un Windows Runtime composant écrit en C++
Par défaut, les classes du composant qui peuvent être activées sont agiles. Toutefois, C++ autorise un contrôle significatif sur les modèles de thread et le comportement de marshaling. Comme décrit plus haut dans cet article, le CLR reconnaît des classes agiles, essaie de marshaler les appels lorsque les classes ne sont pas agiles et lève une exception [System. InvalidCastException](/dotnet/api/system.invalidcastexception) lorsqu’une classe n’a pas d’informations de marshaling.

Pour les objets qui s’exécutent sur le thread d’interface utilisateur et lèvent des exceptions lorsqu’ils sont appelés à partir d’un thread autre que le thread d’interface utilisateur, vous pouvez utiliser l’objet [CoreDispatcher](/uwp/api/Windows.UI.Core.CoreDispatcher) du thread d’interface utilisateur pour distribuer l’appel.

## <a name="see-also"></a>Voir aussi
[Guide C#](/dotnet/csharp/)

[Guide de Visual Basic](/dotnet/visual-basic/)