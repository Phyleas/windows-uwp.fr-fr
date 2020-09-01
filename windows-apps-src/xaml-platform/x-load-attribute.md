---
title: attribut xLoad
description: xLoad permet la création et la destruction dynamiques d’un élément et de ses enfants, réduisant ainsi le temps de démarrage et l’utilisation de la mémoire.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1b110091c1a1f208ad06ea52f5c84dc7daad56c0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161683"
---
# <a name="xload-attribute"></a>Attribut x:Load

Vous pouvez utiliser **x :Load** pour optimiser le démarrage, la création de l’arborescence d’éléments visuels et l’utilisation de la mémoire de votre application XAML. L’utilisation de **x :Load** a un effet visuel similaire à la **visibilité**, sauf que lorsque l’élément n’est pas chargé, sa mémoire est libérée et, en interne, un petit espace réservé est utilisé pour marquer son emplacement dans l’arborescence d’éléments visuels.

L’élément d’interface utilisateur attribué avec x :Load peut être chargé et déchargé par le biais du code, ou à l’aide d’une expression [x :bind](x-bind-markup-extension.md) . Cela permet de réduire les coûts des éléments qui s’affichent rarement ou de manière conditionnelle. Lorsque vous utilisez x :Load sur un conteneur tel que Grid ou StackPanel, le conteneur et tous ses enfants sont chargés ou déchargés en tant que groupe.

Le suivi des éléments différés par l’infrastructure XAML ajoute environ 600 octets à l’utilisation de la mémoire pour chaque élément attribué avec x :Load, pour tenir compte de l’espace réservé. Par conséquent, il est possible d’abuser de cet attribut dans la mesure où vos performances diminuent réellement. Nous vous recommandons de l’utiliser uniquement sur les éléments qui doivent être masqués. Si vous utilisez x :Load sur un conteneur, la charge est payée uniquement pour l’élément avec l’attribut x :Load.

> [!IMPORTANT]
> L’attribut x :Load est disponible à partir de Windows 10, version 1703 (Creators Update). La version minimale ciblée par votre projet Visual Studio doit être *Windows 10 Creators Update (10,0, Build 15063)* pour pouvoir utiliser x :Load.

## <a name="xaml-attribute-usage"></a>Utilisation des attributs XAML

``` syntax
<object x:Load="True" .../>
<object x:Load="False" .../>
<object x:Load="{x:Bind Path.to.a.boolean, Mode=OneWay}" .../>
```

## <a name="loading-elements"></a>Charger des éléments

Il existe plusieurs façons de charger les éléments :

- Utilisez une expression [x :bind](x-bind-markup-extension.md) pour spécifier l’état de chargement. L’expression doit retourner **true** pour charger et **false** pour décharger l’élément.
- Appelez [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) avec le nom que vous avez défini sur l’élément.
- Appelez [**GetTemplateChild**](/uwp/api/windows.ui.xaml.controls.control.gettemplatechild) avec le nom que vous avez défini sur l’élément.
- Dans un [**VisualState**](/uwp/api/Windows.UI.Xaml.VisualState), utilisez une animation [**Setter**](/uwp/api/Windows.UI.Xaml.Setter) ou **Storyboard** qui cible l’élément x :Load.
- Ciblez l’élément déchargé dans une **table de montage séquentiel**.

> NOTE : une fois que l’instanciation d’un élément a démarré, celui-ci est créé sur le thread de l’interface utilisateur, au risque que celle-ci s’interrompe si ce qui est créé en une fois est trop important.

Une fois qu’un élément différé est créé dans l’une des manières mentionnées précédemment, plusieurs choses se produisent :

- L’événement [**Loaded**](/uwp/api/windows.ui.xaml.frameworkelement.loaded) est déclenché sur l’élément.
- Le champ de x :Name est défini.
- Toutes les liaisons x :Bind sur l’élément sont évaluées.
- Si vous vous êtes inscrit pour recevoir des notifications de modification de propriété sur la propriété contenant les éléments différés, la notification est déclenchée.

## <a name="unloading-elements"></a>Déchargement des éléments

Pour décharger un élément :

- Utilisez une expression x :Bind pour spécifier l’état de chargement. L’expression doit retourner **true** pour charger et **false** pour décharger l’élément.
- Dans une page ou un UserControl, appelez **UnloadObject** et transmettez la référence d’objet
- Appeler **Windows. UI. Xaml. Markup. XamlMarkupHelper. UnloadObject** et passer la référence d’objet

Lorsqu’un objet est déchargé, il est remplacé dans l’arborescence par un espace réservé. L’instance d’objet reste en mémoire jusqu’à ce que toutes les références aient été libérées. L’API UnloadObject sur un contrôle page/UserControl est conçue pour libérer les références détenues par CodeGen pour x :Name et x :Bind. Si vous conservez des références supplémentaires dans le code d’application, elles doivent également être publiées.

Lorsqu’un élément est déchargé, tout l’état associé à l’élément est ignoré. par conséquent, si vous utilisez x :Load en tant que version optimisée de la visibilité, vous vérifiez que tout l’État est appliqué via des liaisons ou qu’il est réappliqué par le code lorsque l’événement chargé est déclenché.

## <a name="restrictions"></a>Restrictions

Les restrictions liées à l’utilisation de **x :Load** sont les suivantes :

- Vous devez définir un [x :Name](x-name-attribute.md)   pour l’élément, car il doit exister un moyen de Rechercher l’élément ultérieurement.
- Vous pouvez uniquement utiliser x :Load sur les types qui dérivent de [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) ou [**FlyoutBase**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase).
- Vous ne pouvez pas utiliser x :Load sur les éléments racine dans une [**page**](/uwp/api/windows.ui.xaml.controls.page), un [**UserControl**](/uwp/api/windows.ui.xaml.controls.usercontrol)ou un [**DataTemplate**](/uwp/api/Windows.UI.Xaml.DataTemplate).
- Vous ne pouvez pas utiliser x :Load sur les éléments d’un [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary).
- Vous ne pouvez pas utiliser x :Load sur du code XAML libre chargé avec [**XamlReader. Load**](/uwp/api/windows.ui.xaml.markup.xamlreader.load).
- Le déplacement d’un élément parent efface tous les éléments qui n’ont pas été chargés.

## <a name="remarks"></a>Remarques

Vous pouvez utiliser x :Load sur les éléments imbriqués, mais ils doivent être obtenus à partir de l’élément le plus externe dans. Si vous essayez de réaliser un élément enfant avant que le parent soit réalisé, une exception est levée.

En règle générale, nous vous recommandons de différer les éléments qui ne sont pas visibles dans le premier frame.Une bonne indication pour la recherche des candidats à retarder consiste à rechercher les éléments qui sont créés avec une [**visibilité**](/uwp/api/windows.ui.xaml.uielement.visibility)réduite. En outre, l’interface utilisateur qui est déclenchée par l’intervention de l’utilisateur est un bon emplacement pour rechercher les éléments que vous pouvez différer.

Méfiez-vous du report des éléments dans un [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView), car cela réduit le temps de démarrage, mais peut également réduire vos performances de panorama en fonction de ce que vous créez. Si vous cherchez à augmenter les performances de panorama, consultez la documentation sur l' [extension de balisage {x :bind}](x-bind-markup-extension.md) et l' [attribut x :phase](x-phase-attribute.md) .

Si l' [attribut x :phase](x-phase-attribute.md) est utilisé conjointement avec **x :Load** , quand un élément ou une arborescence d’éléments est réalisé, les liaisons sont appliquées jusqu’à la phase actuelle, y compris. La phase spécifiée pour **x :phase** affecte ou contrôle l’état de chargement de l’élément. Lorsqu’un élément de liste est recyclé dans le cadre de la panoramisation, les éléments réalisés se comportent de la même façon que d’autres éléments actifs, et les liaisons compilées (**{x :bind}** Bindings) sont traitées à l’aide des mêmes règles, y compris la mise à l’étape.

Une recommandation générale consiste à mesurer les performances de votre application avant et après pour vous assurer que vous obtenez les performances souhaitées.

## <a name="example"></a>Exemple

```xml
<StackPanel>
    <Grid x:Name="DeferredGrid" x:Load="False">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>

        <Rectangle Height="100" Width="100" Fill="Orange" Margin="0,0,4,4"/>
        <Rectangle Height="100" Width="100" Fill="Green" Grid.Column="1" Margin="4,0,0,4"/>
        <Rectangle Height="100" Width="100" Fill="Blue" Grid.Row="1" Margin="0,4,4,0"/>
        <Rectangle Height="100" Width="100" Fill="Gold" Grid.Row="1" Grid.Column="1" Margin="4,4,0,0"
                   x:Name="one" x:Load="{x:Bind (x:Boolean)CheckBox1.IsChecked, Mode=OneWay}"/>
        <Rectangle Height="100" Width="100" Fill="Silver" Grid.Row="1" Grid.Column="1" Margin="4,4,0,0"
                   x:Name="two" x:Load="{x:Bind Not(CheckBox1.IsChecked), Mode=OneWay}"/>
    </Grid>

    <Button Content="Load elements" Click="LoadElements_Click"/>
    <Button Content="Unload elements" Click="UnloadElements_Click"/>
    <CheckBox x:Name="CheckBox1" Content="Swap Elements" />
</StackPanel>
```

```csharp
// This is used by the bindings between the rectangles and check box.
private bool Not(bool? value) { return !(value==true); }

private void LoadElements_Click(object sender, RoutedEventArgs e)
{
    // This will load the deferred grid, but not the nested
    // rectangles that have x:Load attributes.
    this.FindName("DeferredGrid"); 
}

private void UnloadElements_Click(object sender, RoutedEventArgs e)
{
     // This will unload the grid and all its child elements.
     this.UnloadObject(DeferredGrid);
}
```