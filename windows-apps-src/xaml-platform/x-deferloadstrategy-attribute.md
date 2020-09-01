---
title: Attribut xDeferLoadStrategy
description: xDeferLoadStrategy retarde la création d’un élément et ses enfants. Cela réduit le temps de démarrage, mais augmente légèrement l’utilisation de la mémoire.Chaque élément affecté ajoute environ 600 octets à l’utilisation de la mémoire.
ms.assetid: E763898E-13FF-4412-B502-B54DBFE2D4E4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f37c04af132e742a54df8e88e5c875a32fa94beb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173773"
---
# <a name="xdeferloadstrategy-attribute"></a>attribut x: DeferLoadStrategy

> [!IMPORTANT]
> À compter de Windows 10, version 1703 (Creators Update), **x :DeferLoadStrategy** est remplacé par l' [**attribut x :Load**](x-load-attribute.md). L’utilisation `x:Load="False"` de est équivalente à `x:DeferLoadStrategy="Lazy"` , mais permet de décharger l’interface utilisateur si nécessaire. Pour plus d’informations, consultez l' [attribut x :Load](x-load-attribute.md) .

Vous pouvez utiliser **x :DeferLoadStrategy = "Lazy"** pour optimiser les performances de démarrage ou de création de l’arborescence de votre application XAML. Quand vous utilisez **x :DeferLoadStrategy = "Lazy"**, la création d’un élément et de ses enfants est différée, ce qui réduit les coûts de temps de démarrage et de mémoire. Cela permet de réduire les coûts des éléments qui s’affichent rarement ou de manière conditionnelle. L’élément est réalisé quand il est référencé à partir du code ou de VisualStateManager.

Toutefois, le suivi des éléments différés par l’infrastructure XAML ajoute environ 600 octets à l’utilisation de la mémoire pour chaque élément affecté. Plus l’arborescence d’éléments que vous reportez est importante, plus vous économisez de temps de démarrage, mais au prix d’une plus grande empreinte mémoire. Par conséquent, il est possible d’abuser de cet attribut dans la mesure où vos performances diminuent.

## <a name="xaml-attribute-usage"></a>Utilisation des attributs XAML

``` syntax
<object x:DeferLoadStrategy="Lazy" .../>
```

## <a name="remarks"></a>Remarques

Les restrictions pour l’utilisation de **x:DeferLoadStrategy** sont les suivantes :

- Vous devez définir un [x :Name](x-name-attribute.md)   pour l’élément, car il doit exister un moyen de Rechercher l’élément ultérieurement.
- Vous pouvez différer uniquement les types qui dérivent de [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) ou [**FlyoutBase**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase).
- Vous ne pouvez pas différer des éléments racine dans une [**page**](/uwp/api/windows.ui.xaml.controls.page), un [**UserControl**](/uwp/api/windows.ui.xaml.controls.usercontrol)ou un [**DataTemplate**](/uwp/api/Windows.UI.Xaml.DataTemplate).
- Vous ne pouvez pas différer des éléments dans un [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary).
- Vous ne pouvez pas différer le XAML libre chargé avec [**XamlReader. Load**](/uwp/api/windows.ui.xaml.markup.xamlreader.load).
- Le déplacement d’un élément parent efface tous les éléments qui n’ont pas été réalisés.

Il existe plusieurs manières de réaliser les éléments différés :

- Appelez [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) avec le nom que vous avez défini sur l’élément.
- Appelez [**GetTemplateChild**](/uwp/api/windows.ui.xaml.controls.control.gettemplatechild) avec le nom que vous avez défini sur l’élément.
- Dans un [**VisualState**](/uwp/api/Windows.UI.Xaml.VisualState), utilisez une animation d' [**accesseur Set**](/uwp/api/Windows.UI.Xaml.Setter) ou de **Storyboard** qui cible l’élément différé.
- Cibler l’élément différé dans un **Storyboard**.
- Utilisez une liaison qui cible l’élément différé.

> NOTE : une fois que l’instanciation d’un élément a démarré, celui-ci est créé sur le thread de l’interface utilisateur, au risque que celle-ci s’interrompe si ce qui est créé en une fois est trop important.

Une fois qu’un élément différé est créé dans l’une des manières mentionnées précédemment, plusieurs choses se produisent :

- L’événement [**Loaded**](/uwp/api/windows.ui.xaml.frameworkelement.loaded) est déclenché sur l’élément.
- Toutes les liaisons sur l’élément sont évaluées.
- Si vous vous êtes inscrit pour recevoir des notifications de modification de propriété sur la propriété contenant les éléments différés, la notification est déclenchée.

Vous pouvez imbriquer des éléments différés, mais ils doivent être réalisés à partir de l’élément le plus éloigné. Si vous essayez de réaliser un élément enfant avant que le parent soit réalisé, une exception est levée.

En règle générale, nous vous recommandons de différer les éléments qui ne sont pas visibles dans le premier frame.Une bonne indication pour la recherche des candidats à retarder consiste à rechercher les éléments qui sont créés avec une [**visibilité**](/uwp/api/windows.ui.xaml.uielement.visibility)réduite. En outre, l’interface utilisateur qui est déclenchée par l’intervention de l’utilisateur est un bon emplacement pour rechercher les éléments que vous pouvez différer.

Méfiez-vous du report des éléments dans un [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView), car cela réduit le temps de démarrage, mais peut également réduire vos performances de panorama en fonction de ce que vous créez. Si vous cherchez à augmenter les performances de panorama, consultez la documentation sur l' [extension de balisage {x :bind}](x-bind-markup-extension.md) et l' [attribut x :phase](x-phase-attribute.md) .

Si l' [attribut x :phase](x-phase-attribute.md) est utilisé conjointement avec **x :DeferLoadStrategy** , quand un élément ou une arborescence d’éléments est réalisé, les liaisons sont appliquées jusqu’à la phase actuelle, y compris. La phase spécifiée pour **x :phase** n’affecte ni ne contrôle le report de l’élément. Lorsqu’un élément de liste est recyclé dans le cadre de la panoramisation, les éléments réalisés se comportent de la même façon que d’autres éléments actifs, et les liaisons compilées (**{x :bind}** Bindings) sont traitées à l’aide des mêmes règles, y compris la mise à l’étape.

Une recommandation générale consiste à mesurer les performances de votre application avant et après pour vous assurer que vous obtenez les performances souhaitées.

## <a name="example"></a>Exemple

```xml
<Grid x:Name="DeferredGrid" x:DeferLoadStrategy="Lazy">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto" />
        <ColumnDefinition Width="Auto" />
    </Grid.ColumnDefinitions>

    <Rectangle Height="100" Width="100" Fill="#F65314" Margin="0,0,4,4" />
    <Rectangle Height="100" Width="100" Fill="#7CBB00" Grid.Column="1" Margin="4,0,0,4" />
    <Rectangle Height="100" Width="100" Fill="#00A1F1" Grid.Row="1" Margin="0,4,4,0" />
    <Rectangle Height="100" Width="100" Fill="#FFBB00" Grid.Row="1" Grid.Column="1" Margin="4,4,0,0" />
</Grid>
<Button x:Name="RealizeElements" Content="Realize Elements" Click="RealizeElements_Click"/>
```

```csharp
private void RealizeElements_Click(object sender, RoutedEventArgs e)
{
    // This will realize the deferred grid.
    this.FindName("DeferredGrid");
}
```