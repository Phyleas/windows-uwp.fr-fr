---
description: Vous pouvez créer une arborescence extensible en liant l’ItemsSource à une source de données hiérarchique, ou vous pouvez créer et gérer des objets TreeViewNode vous-même.
title: Arborescence
label: Tree view
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
pm-contact: predavid
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
dev_langs:
- csharp
- vb
ms.custom: RS5, 19H1
ms.openlocfilehash: e033c98aa9336c544219786a254cebaedfcd9f29
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750145"
---
# <a name="treeview"></a>TreeView

Le contrôle XAML [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) active une liste hiérarchique comportant des nœuds de développement et de réduction qui contiennent des éléments imbriqués. Vous pouvez l’utiliser pour illustrer une structure de dossiers ou des relations imbriquées dans votre IU.

Les API **TreeView** prennent en charge les fonctionnalités suivantes :

- Imbrication de niveau N
- Sélection d’un ou plusieurs nœuds
- Liaison de données à la propriété **ItemsSource** sur **TreeView** et [TreeViewItem](/uwp/api/windows.ui.xaml.controls.treeviewitem)
- **TreeViewItem** comme racine du modèle d’élément **TreeView**
- Types arbitraires de contenu dans un **TreeViewItem**
- Glisser-déplacer entre des arborescences

**Obtenir la bibliothèque d’interface utilisateur Windows**

:::row:::
   :::column:::
      ![Logo WinUI](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      Le contrôle **TreeView** est inclus dans la bibliothèque d’interface utilisateur Windows, package NuGet contenant de nouveaux contrôles et fonctionnalités d’interface utilisateur destinés aux applications Windows. Pour plus d’informations, notamment des instructions d’installation, consultez la [bibliothèque d’interface utilisateur Windows](/uwp/toolkits/winui/).
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **API de la bibliothèque d’interface utilisateur Windows :** [Classe TreeView](/uwp/api/microsoft.ui.xaml.controls.treeview), [classe TreeViewNode](/uwp/api/microsoft.ui.xaml.controls.treeviewnode), [propriété TreeView.ItemsSource](/uwp/api/microsoft.ui.xaml.controls.treeview.itemssource)
>
> **API de plateforme :** [Classe TreeView](/uwp/api/windows.ui.xaml.controls.treeview), [classe TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode), [propriété TreeView.ItemsSource](/uwp/api/windows.ui.xaml.controls.treeview.itemssource)

> [!TIP]
> Tout au long de ce document, nous utilisons l’alias **muxc** en XAML pour représenter les API de la bibliothèque d’interface utilisateur Windows que nous avons incluses dans notre projet. Nous l’avons ajouté à notre élément [Page](/uwp/api/windows.ui.xaml.controls.page) : `xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`
>
>Dans le code-behind, nous utilisons également l’alias **muxc** en C# pour représenter les API de la bibliothèque d’interface utilisateur Windows que nous avons incluses dans notre projet. Nous avons ajouté cette instruction **using** en haut du fichier : `using muxc = Microsoft.UI.Xaml.Controls;`

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

- Utilisez un contrôle **TreeView** lorsque les éléments comportent des éléments de liste imbriqués et qu’il est important d’illustrer la relation hiérarchique des éléments par rapport à leurs homologues et leurs nœuds.

- Évitez d’utiliser **TreeView** si la mise en évidence de la relation imbriquée d’un élément n’est pas une priorité. Pour la plupart des scénarios d’exploration, un affichage sous forme de liste normal est approprié.

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/TreeView">ouvrir l’application et voir l’objet TreeView en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="treeview-ui"></a>Interface utilisateur TreeView

L’arborescence utilise une combinaison de retraits et d’icônes pour représenter la relation imbriquée existant entre les nœuds parents et les nœuds enfants. Les nœuds réduits utilisent un chevron pointant vers la droite, et les nœuds développés un chevron pointant vers le bas.

![Icône de chevron dans TreeView](images/treeview-simple.png)

Vous pouvez inclure une icône dans le modèle de données d’élément de l’arborescence pour représenter les nœuds. Par exemple, si vous affichez une hiérarchie de système de fichiers, vous pouvez utiliser des icônes de dossier pour les nœuds parents et des icônes de fichier pour les nœuds terminaux.

![Icônes de chevron et dossier dans un TreeView](images/treeview-icons.png)

## <a name="create-a-tree-view"></a>Créer une arborescence

Vous pouvez créer une arborescence en liant l’[ItemsSource](/uwp/api/windows.ui.xaml.controls.treeview.itemssource) à une source de données hiérarchique, ou vous pouvez créer et gérer des objets **TreeViewNode** vous-même.

Pour créer une arborescence, vous utilisez un contrôle [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) et une hiérarchie d’objets [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode). Vous créez la hiérarchie des nœuds en ajoutant un ou plusieurs nœuds racines à la collection [RootNodes](/uwp/api/windows.ui.xaml.controls.treeview.rootnodes) du contrôle **TreeView**. D’autres nœuds peuvent ensuite être ajoutés à la collection [Children](/uwp/api/windows.ui.xaml.controls.treeviewnode.children) de chaque **TreeViewNode**. Vous pouvez imbriquer des nœuds de l’arborescence à la profondeur que vous souhaitez.

Vous pouvez lier une source de données hiérarchique à la propriété [ItemsSource](/uwp/api/windows.ui.xaml.controls.treeview.itemssource) pour fournir le contenu de l’arborescence, comme vous le feriez avec l’**ItemsSource** de [ListView](/uwp/api/windows.ui.xaml.controls.listview). De même, utilisez [ItemTemplate](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate) (et l’[ItemTemplateSelector](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate) facultatif) pour fournir un [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) qui affiche l’élément.

> [!IMPORTANT]
> **ItemsSource** et ses API associées nécessitent Windows 10, version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk) ou ultérieure), ou la [bibliothèque d’interface utilisateur Windows](/uwp/toolkits/winui/).
>
> **ItemsSource** est un autre mécanisme pour**TreeView.RootNodes** permettant de placer du contenu dans le contrôle **TreeView**. Vous ne pouvez pas définir **ItemsSource** et **RootNodes** en même temps. Quand vous utilisez **ItemsSource**, les nœuds sont créés pour vous, et vous pouvez y accéder à partir de la propriété **TreeView.RootNodes**.

Voici un exemple d’arborescence simple déclarée en XAML. Vous ajoutez généralement les nœuds dans le code, mais nous affichons ici la hiérarchie XAML, parce qu’il peut être utile de visualiser la façon dont la hiérarchie de nœuds est créée.

```xaml
<muxc:TreeView>
    <muxc:TreeView.RootNodes>
        <muxc:TreeViewNode Content="Flavors"
                           IsExpanded="True">
            <muxc:TreeViewNode.Children>
                <muxc:TreeViewNode Content="Vanilla"/>
                <muxc:TreeViewNode Content="Strawberry"/>
                <muxc:TreeViewNode Content="Chocolate"/>
            </muxc:TreeViewNode.Children>
        </muxc:TreeViewNode>
    </muxc:TreeView.RootNodes>
</muxc:TreeView>
```

Dans la plupart des cas, votre arborescence présente les données à partir d’une source de données. Donc, en général, vous déclarez le contrôle racine **TreeView** en XAML, mais ajoutez les objets **TreeViewNode** dans le code ou à l’aide de la liaison de données.

### <a name="bind-to-a-hierarchical-data-source"></a>Lier à une source de données hiérarchique

Pour créer une arborescence à l’aide de la liaison de données, définissez une collection hiérarchique sur la propriété **TreeView.ItemsSource**. Ensuite, dans le modèle **ItemTemplate**, définissez la collection d’éléments enfants avec la propriété **TreeViewItem.ItemsSource**.

```xaml
<muxc:TreeView ItemsSource="{x:Bind DataSource}">
    <muxc:TreeView.ItemTemplate>
        <DataTemplate x:DataType="local:Item">
            <muxc:TreeViewItem ItemsSource="{x:Bind Children}"
                               Content="{x:Bind Name}"/>
        </DataTemplate>
    </muxc:TreeView.ItemTemplate>
</muxc:TreeView>
```

Pour obtenir le code complet, consultez [Arborescence utilisant la liaison de données](#tree-view-using-data-binding).

#### <a name="items-and-item-containers"></a>Éléments et conteneurs d’éléments

Si vous utilisez **TreeView.ItemsSource**, ces API permettent d’obtenir l’élément de données ou le nœud à partir du conteneur et vice versa.

| [TreeViewItem](/uwp/api/windows.ui.xaml.controls.treeviewitem) | Description |
| ------------------------------------------------------------------ | ----------- |
| [TreeView.ItemFromContainer](/uwp/api/windows.ui.xaml.controls.treeview.itemfromcontainer) | Obtient l’élément de données pour le conteneur **TreeViewItem** spécifié. |
| [TreeView.ContainerFromItem](/uwp/api/windows.ui.xaml.controls.treeview.containerfromitem) | Obtient le conteneur **TreeViewItem** pour l’élément de données spécifié. |

| [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode) | Description |
| -------------------------------------------------------------- | ----------- |
| [TreeView.NodeFromContainer](/uwp/api/windows.ui.xaml.controls.treeview.nodefromcontainer) | Obtient le **TreeViewItem** pour le conteneur **TreeViewItem** spécifié. |
| [TreeView.ContainerFromNode](/uwp/api/windows.ui.xaml.controls.treeview.containerfromnode) | Obtient le conteneur **TreeViewItem** pour le **TreeViewNode** spécifié. |

### <a name="manage-tree-view-nodes"></a>Gérer les nœuds de l’arborescence

Cette arborescence est identique à celle créée en XAML, mais les nœuds sont créés dans le code à la place.

```xaml
<muxc:TreeView x:Name="sampleTreeView"/>
```

```csharp
private void InitializeTreeView()
{
    muxc.TreeViewNode rootNode = new muxc.TreeViewNode() { Content = "Flavors" };
    rootNode.IsExpanded = true;
    rootNode.Children.Add(new muxc.TreeViewNode() { Content = "Vanilla" });
    rootNode.Children.Add(new muxc.TreeViewNode() { Content = "Strawberry" });
    rootNode.Children.Add(new muxc.TreeViewNode() { Content = "Chocolate" });

    sampleTreeView.RootNodes.Add(rootNode);
}
```

```vb
Private Sub InitializeTreeView()
    Dim rootNode As New muxc.TreeViewNode With {.Content = "Flavors", .IsExpanded = True}
    With rootNode.Children
        .Add(New muxc.TreeViewNode With {.Content = "Vanilla"})
        .Add(New muxc.TreeViewNode With {.Content = "Strawberry"})
        .Add(New muxc.TreeViewNode With {.Content = "Chocolate"})
    End With
    sampleTreeView.RootNodes.Add(rootNode)
End Sub
```

Ces API sont disponibles pour la gestion de la hiérarchie de données de votre arborescence.

| [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) | Description |
| ------------------------------------------------------ | ----------- |
| [RootNodes](/uwp/api/windows.ui.xaml.controls.treeview.rootnodes) | Une arborescence peut avoir un ou plusieurs nœuds racine. Ajoutez un objet **TreeViewNode** à la collection **RootNodes** pour créer un nœud racine. Le **Parent** d’un nœud racine est toujours **null**. La **Profondeur** d’un nœud racine est 0. |

| [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode) | Description |
| -------------------------------------------------------------- | ----------- |
| [Children](/uwp/api/windows.ui.xaml.controls.treeviewnode.children) | Ajoutez des objets **TreeViewNode** à la collection **Children** d’un nœud parent pour créer votre hiérarchie de nœuds. Un nœud est le **Parent** de tous les nœuds de sa collection **Children**. |
| [HasChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.haschildren) | **true** si le nœud a réalisé des enfants. **false** indique un dossier ou un élément vide. |
| [HasUnrealizedChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.hasunrealizedchildren) | Utilisez cette propriété si vous remplissez des nœuds quand ils sont développés. Consultez [Remplir un nœud quand il est développé](#fill-a-node-when-its-expanding) plus loin dans cet article. |
| [Depth](/uwp/api/windows.ui.xaml.controls.treeviewnode.depth) | Indique la distance entre le nœud racine et un nœud enfant. |
| [Parent](/uwp/api/windows.ui.xaml.controls.treeviewnode.parent) | Obtient le **TreeViewNode** qui possède la collection **Children** dont ce nœud fait partie. |

L’arborescence utilise les propriétés **HasChildren** et **HasUnrealizedChildren** pour déterminer si l’icône développer/réduire s’affiche. Si l’une de ces propriétés est **true**, l’icône s’affiche ; autrement, elle ne s’affiche pas.

## <a name="tree-view-node-content"></a>Contenu d’un nœud d’arborescence

Vous pouvez stocker l’élément de données qu’un nœud d’arborescence représente dans sa propriété [Content](/uwp/api/windows.ui.xaml.controls.treeviewnode.content).

Dans les exemples précédents, le contenu a une valeur de chaîne simple. Ici, un nœud d’arborescence représente le dossier **Images** de l’utilisateur, de sorte que la bibliothèque d’images [StorageFolder](/uwp/api/windows.storage.storagefolder) est affectée à la propriété **Content** du nœud.

```csharp
StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
muxc.TreeViewNode pictureNode = new muxc.TreeViewNode();
pictureNode.Content = picturesFolder;
```

```vb
Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
Dim pictureNode As New muxc.TreeViewNode With {.Content = picturesFolder}
```

> [!NOTE]
> Pour accéder au dossier **Images**, vous devez spécifier la fonctionnalité **Bibliothèque d’images** dans le manifeste d’application. Pour plus d’informations, voir [Déclarations des fonctionnalités d’application](../../packaging/app-capability-declarations.md).

Vous pouvez fournir un [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) pour spécifier comment l’élément de données s’affiche dans l’arborescence.

> [!NOTE]
> Dans Windows 10, version 1803, vous devez redéfinir le modèle du contrôle **TreeView** et spécifier un **ItemTemplate** personnalisé si votre contenu n’est pas une chaîne. Dans les versions ultérieures, définissez la propriété **TreeView.ItemTemplate**. Pour plus d’informations, consultez [TreeView.ItemTemplate](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate).

### <a name="item-container-style"></a>Style de conteneur d’éléments

Que vous utilisiez **ItemsSource** ou **RootNodes**, l’élément effectivement utilisé pour afficher chaque nœud, appelé « conteneur », est un objet [TreeViewItem](/uwp/api/windows.ui.xaml.controls.treeviewitem). Vous pouvez modifier les propriétés **TreeViewItem** pour appliquer un style de conteneur à l’aide des propriétés **ItemContainerStyle** et [ItemContainerStyleSelector](/uwp/api/windows.ui.xaml.controls.treeview.itemcontainerstyle) de [TreeView](/uwp/api/windows.ui.xaml.controls.treeview.itemcontainerstyleselector).

Cet exemple montre comment remplacer les glyphes développés/réduits par des signes +/- orange. Dans le modèle par défaut **TreeViewItem**, les glyphes sont définis pour utiliser la police `Segoe MDL2 Assets`. Vous pouvez définir la propriété **Setter.Value** en fournissant la valeur de caractère Unicode dans le format utilisé par XAML, comme suit : `Value="&#xE948;"`.

```xaml
<muxc:TreeView>
    <muxc:TreeView.ItemContainerStyle>
        <Style TargetType="muxc:TreeViewItem">
            <Setter Property="CollapsedGlyph" Value="&#xE948;"/>
            <Setter Property="ExpandedGlyph" Value="&#xE949;"/>
            <Setter Property="GlyphBrush" Value="DarkOrange"/>
        </Style>
    </muxc:TreeView.ItemContainerStyle>
    <muxc:TreeView.RootNodes>
        <muxc:TreeViewNode Content="Flavors"
               IsExpanded="True">
            <muxc:TreeViewNode.Children>
                <muxc:TreeViewNode Content="Vanilla"/>
                <muxc:TreeViewNode Content="Strawberry"/>
                <muxc:TreeViewNode Content="Chocolate"/>
            </muxc:TreeViewNode.Children>
        </muxc:TreeViewNode>
    </muxc:TreeView.RootNodes>
</muxc:TreeView>
```

### <a name="item-template-selectors"></a>Sélecteurs de modèles d’éléments

Par défaut, [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) affiche la représentation de chaîne de l'élément de données pour chaque nœud. Vous pouvez définir la propriété [ItemTemplate](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate) afin de modifier ce qui s'affiche pour tous les nœuds. Vous pouvez également utiliser un [ItemTemplateSelector](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplateselector) pour choisir un [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) différent pour les éléments d’arborescence en fonction du type d’élément ou d'autres critères que vous spécifiez.

Par exemple, dans une application d’explorateur de fichiers, vous pouvez utiliser un modèle de données pour les dossiers et un autre pour les fichiers.

![Dossiers et fichiers utilisant différents modèles de données](images/treeview-icons.png)

Voici un exemple montrant comment créer et utiliser un sélecteur de modèle d’élément.  Pour plus d’informations, consultez la classe [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector).

> [!NOTE]
> Ce code fait partie d’un exemple complet et ne fonctionne pas de manière autonome. Pour voir l’exemple complet, y compris le code qui définit `ExplorerItem`, accédez au [référentiel Xaml-Controls-Gallery](https://github.com/microsoft/Xaml-Controls-Gallery) sur GitHub. [TreeViewPage.xaml](https://github.com/microsoft/Xaml-Controls-Gallery/blob/1ecd85c908a8a1cb9a8201e548f58db379801e69/XamlControlsGallery/ControlPages/TreeViewPage.xaml) et [TreeViewPage.xaml.cs](https://github.com/Microsoft/Xaml-Controls-Gallery/blob/1ecd85c908a8a1cb9a8201e548f58db379801e69/XamlControlsGallery/ControlPages/TreeViewPage.xaml.cs) contiennent le code approprié.

```xaml
<Page.Resources>
    <DataTemplate x:Key="FolderTemplate" x:DataType="local:ExplorerItem">
        <muxc:TreeViewItem ItemsSource="{x:Bind Children}">
            <StackPanel Orientation="Horizontal">
                <Image Width="20" Source="Assets/folder.png"/>
                <TextBlock Text="{x:Bind Name}" />
            </StackPanel>
        </muxc:TreeViewItem>
    </DataTemplate>

    <DataTemplate x:Key="FileTemplate" x:DataType="local:ExplorerItem">
        <muxc:TreeViewItem>
            <StackPanel Orientation="Horizontal">
                <Image Width="20" Source="Assets/file.png"/>
                <TextBlock Text="{x:Bind Name}"/>
            </StackPanel>
        </muxc:TreeViewItem>
    </DataTemplate>

    <local:ExplorerItemTemplateSelector
            x:Key="ExplorerItemTemplateSelector"
            FolderTemplate="{StaticResource FolderTemplate}"
            FileTemplate="{StaticResource FileTemplate}" />
</Page.Resources>

<Grid>
    <muxc:TreeView
        ItemsSource="{x:Bind DataSource}"
        ItemTemplateSelector="{StaticResource ExplorerItemTemplateSelector}"/>
</Grid>
```

```csharp
public class ExplorerItemTemplateSelector : DataTemplateSelector
{
    public DataTemplate FolderTemplate { get; set; }
    public DataTemplate FileTemplate { get; set; }

    protected override DataTemplate SelectTemplateCore(object item)
    {
        var explorerItem = (ExplorerItem)item;
        if (explorerItem.Type == ExplorerItem.ExplorerItemType.Folder) return FolderTemplate;

        return FileTemplate;
    }
}
```

Le type d’objet transmis à la méthode [SelectTemplateCore](/uwp/api/windows.ui.xaml.controls.datatemplateselector.selecttemplatecore) varie selon que vous créez l’arborescence en définissant la propriété **ItemsSource**, ou en créant et en gérant des objets **TreeViewNode**  vous-même.

- Si **ItemsSource** est défini, le type de l'objet correspondra au type de l'élément de données. Dans l’exemple précédent, l’objet était un `ExplorerItem`, et pourrait donc être utilisé après une simple conversion en `ExplorerItem` : `var explorerItem = (ExplorerItem)item;`.
- Si **ItemsSource** n’est pas défini et que vous gérez vous-même les nœuds d’arborescence, l’objet transmis à **SelectTemplateCore** est un **TreeViewNode**. Dans ce cas, vous pouvez obtenir l’élément de données à partir de la propriété [TreeViewNode.Content](/uwp/api/windows.ui.xaml.controls.treeviewnode.content).

Voici un sélecteur de modèle de données extrait de l'exemple [Arborescence de bibliothèque d’images et de musique](#pictures-and-music-library-tree-view) illustré plus loin. La méthode **SelectTemplateCore** reçoit un **TreeViewNode**, qui peut avoir contenir un [StorageFolder](/uwp/api/windows.storage.storagefolder) ou un [StorageFile](/uwp/api/windows.storage.storagefile). En fonction de ce contenu, vous pouvez retourner un modèle par défaut ou un modèle spécifique pour le dossier de musique, le dossier d'images, les fichiers de musique ou les fichiers d'image.

```csharp
protected override DataTemplate SelectTemplateCore(object item)
{
    var node = (TreeViewNode)item;
    if (node.Content is StorageFolder)
    {
        var content = node.Content as StorageFolder;
        if (content.DisplayName.StartsWith("Pictures")) return PictureFolderTemplate;
        if (content.DisplayName.StartsWith("Music")) return MusicFolderTemplate;
    }
    else if (node.Content is StorageFile)
    {
        var content = node.Content as StorageFile;
        if (content.ContentType.StartsWith("image")) return PictureItemTemplate;
        if (content.ContentType.StartsWith("audio")) return MusicItemTemplate;
    }
    return DefaultTemplate;
}
```

```vb
Protected Overrides Function SelectTemplateCore(ByVal item As Object) As DataTemplate
    Dim node = CType(item, muxc.TreeViewNode)

    If TypeOf node.Content Is StorageFolder Then
        Dim content = TryCast(node.Content, StorageFolder)
        If content.DisplayName.StartsWith("Pictures") Then Return PictureFolderTemplate
        If content.DisplayName.StartsWith("Music") Then Return MusicFolderTemplate
    ElseIf TypeOf node.Content Is StorageFile Then
        Dim content = TryCast(node.Content, StorageFile)
        If content.ContentType.StartsWith("image") Then Return PictureItemTemplate
        If content.ContentType.StartsWith("audio") Then Return MusicItemTemplate
    End If

    Return DefaultTemplate
End Function
```

## <a name="interacting-with-a-tree-view"></a>Interaction avec une arborescence

Vous pouvez configurer une arborescence pour permettre à un utilisateur d’interagir avec celle-ci de différentes manières :

- Développer ou réduire des nœuds
- Éléments de sélection multiple ou unique
- Cliquer pour appeler un élément

### <a name="expandcollapse"></a>Développer/réduire

Vous pouvez développer ou réduire tout nœud d’arborescence ayant des enfants en cliquant sur le glyphe développer/réduire. Vous pouvez également développer ou réduire un nœud par programmation et répondre quand un nœud change d’état.

#### <a name="expandcollapse-a-node-programmatically"></a>Développer/réduire un nœud par programmation

Il existe 2 façons de développer ou de réduire un nœud d’arborescence dans votre code.

- La classe [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) possède les méthodes [Collapse](/uwp/api/windows.ui.xaml.controls.treeview.collapse) et [Expand](/uwp/api/windows.ui.xaml.controls.treeview.expand). Quand vous appelez ces méthodes, vous passez le **TreeViewNode** que vous voulez développer ou réduire.

- Chaque [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode) a la propriété [IsExpanded](/uwp/api/windows.ui.xaml.controls.treeviewnode.isexpanded). Vous pouvez utiliser cette propriété pour vérifier l’état d’un nœud ou le configurer pour changer l’état. Vous pouvez également définir cette propriété dans le code XAML pour définir l’état initial d’un nœud.

### <a name="fill-a-node-when-its-expanding"></a>Remplir un nœud quand il est développé

Vous devrez peut-être afficher un grand nombre de nœuds dans votre arborescence, ou vous ne saurez peut-être pas à l’avance combien de nœuds elle contiendra. Le contrôle **TreeView** n’étant pas virtualisé, vous pouvez gérer les ressources en remplissant chaque nœud quand il est développé et en supprimant les nœuds enfants quand il est réduit.

Gérez l’événement [Expanding](/uwp/api/windows.ui.xaml.controls.treeview.expand) et utilisez la propriété [HasUnrealizedChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.hasunrealizedchildren) pour ajouter des enfants à un nœud quand il est développé. La propriété **HasUnrealizedChildren** indique si le nœud doit être rempli ou si sa collection **Children** a déjà été remplie. Il est important de garder à l’esprit que le **TreeViewNode** ne définit pas cette valeur et que vous devez le gérer dans le code de votre application.

Voici un exemple d’utilisation de ces API. Consultez l’exemple de code complet à la fin de cet article pour connaître le contexte, notamment l’implémentation de **FillTreeNode**.

```csharp
private void SampleTreeView_Expanding(muxc.TreeView sender, muxc.TreeViewExpandingEventArgs args)
{
    if (args.Node.HasUnrealizedChildren)
    {
        FillTreeNode(args.Node);
    }
}
```

```vb
Private Sub SampleTreeView_Expanding(sender As muxc.TreeView, args As muxc.TreeViewExpandingEventArgs)
    If args.Node.HasUnrealizedChildren Then
        FillTreeNode(args.Node)
    End If
End Sub
```

Ce n’est pas obligatoire, mais vous pouvez également gérer l’événement [Collapsed](/uwp/api/windows.ui.xaml.controls.treeview.collapsed) et supprimer les nœuds enfants à la fermeture du nœud parent. Cela peut être important si votre arborescence a un grand nombre de nœuds, ou si les données de nœud utilisent beaucoup de ressources. Vous devez prendre en compte l’impact qu’a le remplissage d’un nœud à chaque ouverture sur les performances par rapport au fait de laisser les enfants sur un nœud fermé. La meilleure option dépend de votre application.

Voici un exemple de gestionnaire de l’événement **Collapsed**.

```csharp
private void SampleTreeView_Collapsed(muxc.TreeView sender, muxc.TreeViewCollapsedEventArgs args)
{
    args.Node.Children.Clear();
    args.Node.HasUnrealizedChildren = true;
}
```

```vb
Private Sub SampleTreeView_Collapsed(sender As muxc.TreeView, args As muxc.TreeViewCollapsedEventArgs)
    args.Node.Children.Clear()
    args.Node.HasUnrealizedChildren = True
End Sub
```

### <a name="invoking-an-item"></a>Appel d’un élément

Un utilisateur peut appeler une action (en traitant l’élément comme un bouton) au lieu de sélectionner l’élément. Vous gérez l’événement [ItemInvoked](/uwp/api/windows.ui.xaml.controls.treeview.iteminvoked) pour répondre à cette interaction utilisateur.

> [!NOTE]
> Contrairement à **ListView**, qui a la propriété [IsItemClickEnabled](/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled), l’appel d’un élément est toujours activé sur l’arborescence. Vous pouvez toujours choisir si vous souhaitez gérer l’événement ou non.

**Classe [TreeViewItemInvokedEventArgs](/uwp/api/windows.ui.xaml.controls.treeviewiteminvokedeventargs)**

Les arguments d’événement **ItemInvoked** vous permettent d’accéder à l’élément appelé. La propriété [InvokedItem](/uwp/api/windows.ui.xaml.controls.treeviewiteminvokedeventargs.invokeditem) a le nœud qui a été appelé. Vous pouvez le caster en **TreeViewNode** et obtenir l’élément de données à partir de la propriété **TreeViewNode.Content**.

Voici un exemple de gestionnaire d’événements **ItemInvoked**. L’élément de données est un [IStorageItem](/uwp/api/windows.storage.istorageitem), et cet exemple affiche uniquement des informations sur le fichier et l’arborescence. En outre, si le nœud est un nœud de dossier, il développe ou réduit le nœud en même temps. Autrement, le nœud se développe ou se réduit uniquement quand vous cliquez sur le chevron.

```csharp
private void SampleTreeView_ItemInvoked(muxc.TreeView sender, muxc.TreeViewItemInvokedEventArgs args)
{
    var node = args.InvokedItem as muxc.TreeViewNode;
    if (node.Content is IStorageItem item)
    {
        FileNameTextBlock.Text = item.Name;
        FilePathTextBlock.Text = item.Path;
        TreeDepthTextBlock.Text = node.Depth.ToString();

        if (node.Content is StorageFolder)
        {
            node.IsExpanded = !node.IsExpanded;
        }
    }
}
```

```vb
Private Sub SampleTreeView_ItemInvoked(sender As muxc.TreeView, args As muxc.TreeViewItemInvokedEventArgs)
    Dim node = TryCast(args.InvokedItem, muxc.TreeViewNode)
    Dim item = TryCast(node.Content, IStorageItem)
    If item IsNot Nothing Then
        FileNameTextBlock.Text = item.Name
        FilePathTextBlock.Text = item.Path
        TreeDepthTextBlock.Text = node.Depth.ToString()
        If TypeOf node.Content Is StorageFolder Then
            node.IsExpanded = Not node.IsExpanded
        End If
    End If
End Sub
```

### <a name="item-selection"></a>Sélection d’élément

Le contrôle **TreeView** prend en charge la sélection unique et la sélection multiple. Par défaut, la sélection de nœuds est désactivée, mais vous pouvez définir la propriété [TreeView.SelectionMode](/uwp/api/windows.ui.xaml.controls.treeview.selectionmode) pour permettre la sélection de nœuds. Les valeurs [TreeViewSelectionMode](/uwp/api/windows.ui.xaml.controls.treeviewselectionmode) sont **None**, **Single** et **Multiple**.

#### <a name="multiple-selection"></a>Sélection multiple

Quand la sélection multiple est activée, une case à cocher s’affiche en regard de chaque nœud d’arborescence, et les éléments sélectionnés sont mis en surbrillance. Un utilisateur peut sélectionner ou désélectionner un élément à l’aide de la case à cocher. Le fait de cliquer sur l’élément permet toujours de l’appeler.

Le fait de sélectionner ou de désélectionner un nœud parent sélectionne ou désélectionne tous les enfants de ce nœud. Si seulement une partie des enfants sous un nœud parent est sélectionnée, la case à cocher du nœud parent est affichée dans un état indéterminé.

![Sélection multiple dans une arborescence](images/treeview-selection.png)

Les nœuds sélectionnés sont ajoutés à la collection [SelectedNodes](/uwp/api/windows.ui.xaml.controls.treeview.selectednodes) de l’arborescence. Vous pouvez appeler la méthode [SelectAll](/uwp/api/windows.ui.xaml.controls.treeview.selectall) pour sélectionner tous les nœuds d’une arborescence.

> [!NOTE]
> Si vous appelez **SelectAll**, tous les nœuds réalisés sont sélectionnés, quel que soit **SelectionMode**. Pour fournir une expérience utilisateur cohérente, vous ne devez appeler **SelectAll** que si **SelectionMode** est **Multiple**.

#### <a name="selection-and-realizedunrealized-nodes"></a>Sélection et nœuds réalisés/non réalisés

Si votre arborescence a des nœuds non réalisés, ils ne sont pas pris en compte pour la sélection. Voici quelques aspects à prendre en compte concernant la sélection avec des nœuds non réalisés.

- Si un utilisateur sélectionne un nœud parent, tous les enfants réalisés sous ce parent sont également sélectionnés. De même, si tous les nœuds enfants sont sélectionnés, le nœud parent est également sélectionné.
- La méthode **SelectAll** ajoute uniquement des nœuds réalisés à la collection **SelectedNodes**.
- Si un nœud parent avec des enfants non réalisés est sélectionné, les enfants sont sélectionnés quand ils sont réalisés.

#### <a name="selecteditemselecteditems"></a>SelectedItem/SelectedItems

Depuis WinUI 2.2, TreeView possède les propriétés [SelectedItem](/uwp/api/microsoft.ui.xaml.controls.treeview.selecteditem) et [SelectedItems](/uwp/api/microsoft.ui.xaml.controls.treeview.selecteditems). Vous pouvez utiliser ces propriétés pour récupérer directement le contenu des nœuds sélectionnés. Si la sélection multiple est activée, SelectedItem contient le premier élément de la collection SelectedItems.

## <a name="code-examples"></a>Exemples de code

Les exemples de code suivants illustrent diverses fonctionnalités du contrôle d’arborescence.

### <a name="tree-view-using-xaml"></a>Arborescence utilisant XAML

Cet exemple montre comment créer une structure d’arborescence simple en XAML. L’arborescence affiche des parfums de glace et des garnitures que l’utilisateur peut choisir, organisés par catégories. La sélection multiple est activée et quand l’utilisateur clique sur un bouton, les éléments sélectionnés s’affichent dans l’interface utilisateur de l’application principale.

```xaml
<Page
    x:Class="TreeViewTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
          Padding="100">
        <SplitView IsPaneOpen="True"
               DisplayMode="Inline"
               OpenPaneLength="296">
            <SplitView.Pane>
                <muxc:TreeView x:Name="DessertTree" SelectionMode="Multiple">
                    <muxc:TreeView.RootNodes>
                        <muxc:TreeViewNode Content="Flavors" IsExpanded="True">
                            <muxc:TreeViewNode.Children>
                                <muxc:TreeViewNode Content="Vanilla"/>
                                <muxc:TreeViewNode Content="Strawberry"/>
                                <muxc:TreeViewNode Content="Chocolate"/>
                            </muxc:TreeViewNode.Children>
                        </muxc:TreeViewNode>

                        <muxc:TreeViewNode Content="Toppings">
                            <muxc:TreeViewNode.Children>
                                <muxc:TreeViewNode Content="Candy">
                                    <muxc:TreeViewNode.Children>
                                        <muxc:TreeViewNode Content="Chocolate"/>
                                        <muxc:TreeViewNode Content="Mint"/>
                                        <muxc:TreeViewNode Content="Sprinkles"/>
                                    </muxc:TreeViewNode.Children>
                                </muxc:TreeViewNode>
                                <muxc:TreeViewNode Content="Fruits">
                                    <muxc:TreeViewNode.Children>
                                        <muxc:TreeViewNode Content="Mango"/>
                                        <muxc:TreeViewNode Content="Peach"/>
                                        <muxc:TreeViewNode Content="Kiwi"/>
                                    </muxc:TreeViewNode.Children>
                                </muxc:TreeViewNode>
                                <muxc:TreeViewNode Content="Berries">
                                    <muxc:TreeViewNode.Children>
                                        <muxc:TreeViewNode Content="Strawberry"/>
                                        <muxc:TreeViewNode Content="Blueberry"/>
                                        <muxc:TreeViewNode Content="Blackberry"/>
                                    </muxc:TreeViewNode.Children>
                                </muxc:TreeViewNode>
                            </muxc:TreeViewNode.Children>
                        </muxc:TreeViewNode>
                    </muxc:TreeView.RootNodes>
                </muxc:TreeView>
            </SplitView.Pane>

            <StackPanel Grid.Column="1" Margin="12,0">
                <Button Content="Select all" Click="SelectAllButton_Click"/>
                <Button Content="Create order" Click="OrderButton_Click" Margin="0,12"/>
                <TextBlock Text="Your flavor selections:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FlavorList" Margin="0,0,0,12"/>
                <TextBlock Text="Your topping selections:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="ToppingList"/>
            </StackPanel>
        </SplitView>
    </Grid>
</Page>
```

```csharp
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using muxc = Microsoft.UI.Xaml.Controls;

namespace TreeViewTest
{
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
        }

        private void OrderButton_Click(object sender, RoutedEventArgs e)
        {
            FlavorList.Text = string.Empty;
            ToppingList.Text = string.Empty;

            foreach (muxc.TreeViewNode node in DessertTree.SelectedNodes)
            {
                if (node.Parent.Content?.ToString() == "Flavors")
                {
                    FlavorList.Text += node.Content + "; ";
                }
                else if (node.HasChildren == false)
                {
                    ToppingList.Text += node.Content + "; ";
                }
            }
        }

        private void SelectAllButton_Click(object sender, RoutedEventArgs e)
        {
            if (DessertTree.SelectionMode == muxc.TreeViewSelectionMode.Multiple)
            {
                DessertTree.SelectAll();
            }
        }
    }
}
```

```vb
Private Sub OrderButton_Click(sender As Object, e As RoutedEventArgs)
    FlavorList.Text = String.Empty
    ToppingList.Text = String.Empty
    For Each node As muxc.TreeViewNode In DessertTree.SelectedNodes
        If node.Parent.Content?.ToString() = "Flavors" Then
            FlavorList.Text += node.Content & "; "
        ElseIf node.HasChildren = False Then
            ToppingList.Text += node.Content & "; "
        End If
    Next
End Sub

Private Sub SelectAllButton_Click(sender As Object, e As RoutedEventArgs)
    If DessertTree.SelectionMode = muxc.TreeViewSelectionMode.Multiple Then
        DessertTree.SelectAll()
    End If
End Sub
```

### <a name="tree-view-using-data-binding"></a>Arborescence utilisant la liaison de données

Cet exemple montre comment créer la même arborescence que l’exemple précédent. Toutefois, au lieu de créer la hiérarchie de données en XAML, les données sont créées dans le code et liées à la propriété **ItemsSource** de l’arborescence. (Les gestionnaires d’événements de bouton illustrés dans l’exemple précédent s’appliquent à cet exemple également.)

```xaml
<Page
    x:Class="TreeViewTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
    xmlns:local="using:TreeViewTest"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
          Padding="100">
        <SplitView IsPaneOpen="True"
                   DisplayMode="Inline"
                   OpenPaneLength="296">
            <SplitView.Pane>
                <muxc:TreeView Name="DessertTree"
                                      SelectionMode="Multiple"
                                      ItemsSource="{x:Bind DataSource}">
                    <muxc:TreeView.ItemTemplate>
                        <DataTemplate x:DataType="local:Item">
                            <muxc:TreeViewItem
                                ItemsSource="{x:Bind Children}"
                                Content="{x:Bind Name}"/>
                        </DataTemplate>
                    </muxc:TreeView.ItemTemplate>
                </muxc:TreeView>
            </SplitView.Pane>

            <StackPanel Grid.Column="1" Margin="12,0">
                <Button Content="Select all"
                        Click="SelectAllButton_Click"/>
                <Button Content="Create order"
                        Click="OrderButton_Click"
                        Margin="0,12"/>
                <TextBlock Text="Your flavor selections:"
                           Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FlavorList" Margin="0,0,0,12"/>
                <TextBlock Text="Your topping selections:"
                           Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="ToppingList"/>
            </StackPanel>
        </SplitView>
    </Grid>

</Page>
```

```csharp
using System.Collections.ObjectModel;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using muxc = Microsoft.UI.Xaml.Controls;

namespace TreeViewTest
{
    public sealed partial class MainPage : Page
    {
        private ObservableCollection<Item> DataSource = new ObservableCollection<Item>();

        public MainPage()
        {
            this.InitializeComponent();
            DataSource = GetDessertData();
        }

        private ObservableCollection<Item> GetDessertData()
        {
            var list = new ObservableCollection<Item>();

            Item flavorsCategory = new Item()
            {
                Name = "Flavors",
                Children =
                {
                    new Item() { Name = "Vanilla" },
                    new Item() { Name = "Strawberry" },
                    new Item() { Name = "Chocolate" }
                }
            };

            Item toppingsCategory = new Item()
            {
                Name = "Toppings",
                Children =
                {
                    new Item()
                    {
                        Name = "Candy",
                        Children =
                        {
                            new Item() { Name = "Chocolate" },
                            new Item() { Name = "Mint" },
                            new Item() { Name = "Sprinkles" }
                        }
                    },
                    new Item()
                    {
                        Name = "Fruits",
                        Children =
                        {
                            new Item() { Name = "Mango" },
                            new Item() { Name = "Peach" },
                            new Item() { Name = "Kiwi" }
                        }
                    },
                    new Item()
                    {
                        Name = "Berries",
                        Children =
                        {
                            new Item() { Name = "Strawberry" },
                            new Item() { Name = "Blueberry" },
                            new Item() { Name = "Blackberry" }
                        }
                    }
                }
            };

            list.Add(flavorsCategory);
            list.Add(toppingsCategory);
            return list;
        }

        private void OrderButton_Click(object sender, RoutedEventArgs e)
        {
            FlavorList.Text = string.Empty;
            ToppingList.Text = string.Empty;

            foreach (muxc.TreeViewNode node in DessertTree.SelectedNodes)
            {
                if (node.Parent.Content?.ToString() == "Flavors")
                {
                    FlavorList.Text += node.Content + "; ";
                }
                else if (node.HasChildren == false)
                {
                    ToppingList.Text += node.Content + "; ";
                }
            }
        }

        private void SelectAllButton_Click(object sender, RoutedEventArgs e)
        {
            if (DessertTree.SelectionMode == muxc.TreeViewSelectionMode.Multiple)
            {
                DessertTree.SelectAll();
            }
        }
    }

    public class Item
    {
        public string Name { get; set; }
        public ObservableCollection<Item> Children { get; set; } = new ObservableCollection<Item>();

        public override string ToString()
        {
            return Name;
        }
    }
}

```

### <a name="pictures-and-music-library-tree-view"></a>Arborescence de bibliothèque d’images et de musique

Cet exemple montre comment créer une arborescence montrant le contenu et la structure des bibliothèques **Images** et **Musique** de l’utilisateur. Le nombre d’éléments ne peut pas être connu à l’avance, donc chaque nœud est rempli quand il est développé et vidé quand il est réduit.

Un modèle d’élément personnalisé est utilisé pour afficher les éléments de données, qui sont de type [IStorageItem](/uwp/api/windows.storage.istorageitem).

> [!IMPORTANT]
> Le code dans cet exemple nécessite les fonctionnalités **picturesLibrary** et **musicLibrary**. Pour plus d’informations sur l’accès aux fichiers, consultez [Autorisations d’accès aux fichiers](../../files/file-access-permissions.md), [Énumérer et interroger des fichiers et dossiers](../../files/quickstart-listing-files-and-folders.md) et [Fichiers et dossiers dans les bibliothèques de musique, d’images et de vidéos](../../files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md).

```xaml
<Page
    x:Class="TreeViewTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:TreeViewTest"
    xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
    xmlns:storage="using:Windows.Storage"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">
    <Page.Resources>
        <DataTemplate x:Key="TreeViewItemDataTemplate" x:DataType="muxc:TreeViewNode">
            <Grid Height="44">
                <TextBlock Text="{x:Bind ((storage:IStorageItem)Content).Name}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </Grid>
        </DataTemplate>

        <DataTemplate x:Key="MusicItemDataTemplate" x:DataType="muxc:TreeViewNode">
            <StackPanel Height="44" Orientation="Horizontal">
                <SymbolIcon Symbol="Audio" Margin="0,0,4,0"/>
                <TextBlock Text="{x:Bind ((storage:StorageFile)Content).DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </StackPanel>
        </DataTemplate>

        <DataTemplate x:Key="PictureItemDataTemplate" x:DataType="muxc:TreeViewNode">
            <StackPanel Height="44" Orientation="Horizontal">
                <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xEB9F;"
                          Margin="0,0,4,0"/>
                <TextBlock Text="{x:Bind ((storage:StorageFile)Content).DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </StackPanel>
        </DataTemplate>

        <DataTemplate x:Key="MusicFolderDataTemplate" x:DataType="muxc:TreeViewNode">
            <StackPanel Height="44" Orientation="Horizontal">
                <SymbolIcon Symbol="MusicInfo" Margin="0,0,4,0"/>
                <TextBlock Text="{x:Bind ((storage:StorageFolder)Content).DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </StackPanel>
        </DataTemplate>

        <DataTemplate x:Key="PictureFolderDataTemplate" x:DataType="muxc:TreeViewNode">
            <StackPanel Height="44" Orientation="Horizontal">
                <SymbolIcon Symbol="Pictures" Margin="0,0,4,0"/>
                <TextBlock Text="{x:Bind ((storage:StorageFolder)Content).DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </StackPanel>
        </DataTemplate>

        <local:ExplorerItemTemplateSelector
            x:Key="ExplorerItemTemplateSelector"
            DefaultTemplate="{StaticResource TreeViewItemDataTemplate}"
            MusicItemTemplate="{StaticResource MusicItemDataTemplate}"
            MusicFolderTemplate="{StaticResource MusicFolderDataTemplate}"
            PictureItemTemplate="{StaticResource PictureItemDataTemplate}"
            PictureFolderTemplate="{StaticResource PictureFolderDataTemplate}"/>
    </Page.Resources>

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <SplitView IsPaneOpen="True"
                   DisplayMode="Inline"
                   OpenPaneLength="296">
            <SplitView.Pane>
                <Grid>
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition/>
                    </Grid.RowDefinitions>
                    <Button Content="Refresh tree" Click="RefreshButton_Click" Margin="24,12"/>
                    <muxc:TreeView x:Name="sampleTreeView" Grid.Row="1" SelectionMode="Single"
                              ItemTemplateSelector="{StaticResource ExplorerItemTemplateSelector}"
                              Expanding="SampleTreeView_Expanding"
                              Collapsed="SampleTreeView_Collapsed"
                              ItemInvoked="SampleTreeView_ItemInvoked"/>
                </Grid>
            </SplitView.Pane>

            <StackPanel Grid.Column="1" Margin="12,72">
                <TextBlock Text="File name:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FileNameTextBlock" Margin="0,0,0,12"/>

                <TextBlock Text="File path:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FilePathTextBlock" Margin="0,0,0,12"/>

                <TextBlock Text="Tree depth:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="TreeDepthTextBlock" Margin="0,0,0,12"/>
            </StackPanel>
        </SplitView>
    </Grid>
</Page>
```

```csharp
using System;
using System.Collections.Generic;
using Windows.Storage;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using muxc = Microsoft.UI.Xaml.Controls;

namespace TreeViewTest
{
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            InitializeTreeView();
        }

        private void InitializeTreeView()
        {
            // A TreeView can have more than 1 root node. The Pictures library
            // and the Music library will each be a root node in the tree.
            // Get Pictures library.
            StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
            muxc.TreeViewNode pictureNode = new muxc.TreeViewNode();
            pictureNode.Content = picturesFolder;
            pictureNode.IsExpanded = true;
            pictureNode.HasUnrealizedChildren = true;
            sampleTreeView.RootNodes.Add(pictureNode);
            FillTreeNode(pictureNode);

            // Get Music library.
            StorageFolder musicFolder = KnownFolders.MusicLibrary;
            muxc.TreeViewNode musicNode = new muxc.TreeViewNode();
            musicNode.Content = musicFolder;
            musicNode.IsExpanded = true;
            musicNode.HasUnrealizedChildren = true;
            sampleTreeView.RootNodes.Add(musicNode);
            FillTreeNode(musicNode);
        }

        private async void FillTreeNode(muxc.TreeViewNode node)
        {
            // Get the contents of the folder represented by the current tree node.
            // Add each item as a new child node of the node that's being expanded.

            // Only process the node if it's a folder and has unrealized children.
            StorageFolder folder = null;

            if (node.Content is StorageFolder && node.HasUnrealizedChildren == true)
            {
                folder = node.Content as StorageFolder;
            }
            else
            {
                // The node isn't a folder, or it's already been filled.
                return;
            }

            IReadOnlyList<IStorageItem> itemsList = await folder.GetItemsAsync();

            if (itemsList.Count == 0)
            {
                // The item is a folder, but it's empty. Leave HasUnrealizedChildren = true so
                // that the chevron appears, but don't try to process children that aren't there.
                return;
            }

            foreach (var item in itemsList)
            {
                var newNode = new muxc.TreeViewNode();
                newNode.Content = item;

                if (item is StorageFolder)
                {
                    // If the item is a folder, set HasUnrealizedChildren to true.
                    // This makes the collapsed chevron show up.
                    newNode.HasUnrealizedChildren = true;
                }
                else
                {
                    // Item is StorageFile. No processing needed for this scenario.
                }

                node.Children.Add(newNode);
            }

            // Children were just added to this node, so set HasUnrealizedChildren to false.
            node.HasUnrealizedChildren = false;
        }

        private void SampleTreeView_Expanding(muxc.TreeView sender, muxc.TreeViewExpandingEventArgs args)
        {
            if (args.Node.HasUnrealizedChildren)
            {
                FillTreeNode(args.Node);
            }
        }

        private void SampleTreeView_Collapsed(muxc.TreeView sender, muxc.TreeViewCollapsedEventArgs args)
        {
            args.Node.Children.Clear();
            args.Node.HasUnrealizedChildren = true;
        }

        private void SampleTreeView_ItemInvoked(muxc.TreeView sender, muxc.TreeViewItemInvokedEventArgs args)
        {
            var node = args.InvokedItem as muxc.TreeViewNode;

            if (node.Content is IStorageItem item)
            {
                FileNameTextBlock.Text = item.Name;
                FilePathTextBlock.Text = item.Path;
                TreeDepthTextBlock.Text = node.Depth.ToString();

                if (node.Content is StorageFolder)
                {
                    node.IsExpanded = !node.IsExpanded;
                }
            }
        }

        private void RefreshButton_Click(object sender, RoutedEventArgs e)
        {
            sampleTreeView.RootNodes.Clear();
            InitializeTreeView();
        }
    }

    public class ExplorerItemTemplateSelector : DataTemplateSelector
    {
        public DataTemplate DefaultTemplate { get; set; }
        public DataTemplate MusicItemTemplate { get; set; }
        public DataTemplate PictureItemTemplate { get; set; }
        public DataTemplate MusicFolderTemplate { get; set; }
        public DataTemplate PictureFolderTemplate { get; set; }

        protected override DataTemplate SelectTemplateCore(object item)
        {
            var node = (muxc.TreeViewNode)item;

            if (node.Content is StorageFolder)
            {
                var content = node.Content as StorageFolder;
                if (content.DisplayName.StartsWith("Pictures")) return PictureFolderTemplate;
                if (content.DisplayName.StartsWith("Music")) return MusicFolderTemplate;
            }
            else if (node.Content is StorageFile)
            {
                var content = node.Content as StorageFile;
                if (content.ContentType.StartsWith("image")) return PictureItemTemplate;
                if (content.ContentType.StartsWith("audio")) return MusicItemTemplate;

            }
            return DefaultTemplate;
        }
    }
}
```

```vb
Public NotInheritable Class MainPage
    Inherits Page

    Public Sub New()
        InitializeComponent()
        InitializeTreeView()
    End Sub

    Private Sub InitializeTreeView()
        ' A TreeView can have more than 1 root node. The Pictures library
        ' and the Music library will each be a root node in the tree.
        ' Get Pictures library.
        Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
        Dim pictureNode As New muxc.TreeViewNode With {
        .Content = picturesFolder,
        .IsExpanded = True,
        .HasUnrealizedChildren = True
    }
        sampleTreeView.RootNodes.Add(pictureNode)
        FillTreeNode(pictureNode)

        ' Get Music library.
        Dim musicFolder As StorageFolder = KnownFolders.MusicLibrary
        Dim musicNode As New muxc.TreeViewNode With {
        .Content = musicFolder,
        .IsExpanded = True,
        .HasUnrealizedChildren = True
    }
        sampleTreeView.RootNodes.Add(musicNode)
        FillTreeNode(musicNode)
    End Sub

    Private Async Sub FillTreeNode(node As muxc.TreeViewNode)
        ' Get the contents of the folder represented by the current tree node.
        ' Add each item as a new child node of the node that's being expanded.

        ' Only process the node if it's a folder and has unrealized children.
        Dim folder As StorageFolder = Nothing
        If TypeOf node.Content Is StorageFolder AndAlso node.HasUnrealizedChildren Then
            folder = TryCast(node.Content, StorageFolder)
        Else
            ' The node isn't a folder, or it's already been filled.
            Return
        End If

        Dim itemsList As IReadOnlyList(Of IStorageItem) = Await folder.GetItemsAsync()
        If itemsList.Count = 0 Then
            ' The item is a folder, but it's empty. Leave HasUnrealizedChildren = true so
            ' that the chevron appears, but don't try to process children that aren't there.
            Return
        End If

        For Each item In itemsList
            Dim newNode As New muxc.TreeViewNode With {
            .Content = item
        }
            If TypeOf item Is StorageFolder Then
                ' If the item is a folder, set HasUnrealizedChildren to True.
                ' This makes the collapsed chevron show up.
                newNode.HasUnrealizedChildren = True
            Else
                ' Item is StorageFile. No processing needed for this scenario.
            End If
            node.Children.Add(newNode)
        Next

        ' Children were just added to this node, so set HasUnrealizedChildren to False.
        node.HasUnrealizedChildren = False
    End Sub

    Private Sub SampleTreeView_Expanding(sender As muxc.TreeView, args As muxc.TreeViewExpandingEventArgs)
        If args.Node.HasUnrealizedChildren Then
            FillTreeNode(args.Node)
        End If
    End Sub

    Private Sub SampleTreeView_Collapsed(sender As muxc.TreeView, args As muxc.TreeViewCollapsedEventArgs)
        args.Node.Children.Clear()
        args.Node.HasUnrealizedChildren = True
    End Sub

    Private Sub SampleTreeView_ItemInvoked(sender As muxc.TreeView, args As muxc.TreeViewItemInvokedEventArgs)
        Dim node = TryCast(args.InvokedItem, muxc.TreeViewNode)
        Dim item = TryCast(node.Content, IStorageItem)
        If item IsNot Nothing Then
            FileNameTextBlock.Text = item.Name
            FilePathTextBlock.Text = item.Path
            TreeDepthTextBlock.Text = node.Depth.ToString()
            If TypeOf node.Content Is StorageFolder Then
                node.IsExpanded = Not node.IsExpanded
            End If
        End If
    End Sub

    Private Sub RefreshButton_Click(sender As Object, e As RoutedEventArgs)
        sampleTreeView.RootNodes.Clear()
        InitializeTreeView()
    End Sub

End Class

Public Class ExplorerItemTemplateSelector
    Inherits DataTemplateSelector

    Public Property DefaultTemplate As DataTemplate
    Public Property MusicItemTemplate As DataTemplate
    Public Property PictureItemTemplate As DataTemplate
    Public Property MusicFolderTemplate As DataTemplate
    Public Property PictureFolderTemplate As DataTemplate

    Protected Overrides Function SelectTemplateCore(ByVal item As Object) As DataTemplate
        Dim node = CType(item, muxc.TreeViewNode)

        If TypeOf node.Content Is StorageFolder Then
            Dim content = TryCast(node.Content, StorageFolder)
            If content.DisplayName.StartsWith("Pictures") Then Return PictureFolderTemplate
            If content.DisplayName.StartsWith("Music") Then Return MusicFolderTemplate
        ElseIf TypeOf node.Content Is StorageFile Then
            Dim content = TryCast(node.Content, StorageFile)
            If content.ContentType.StartsWith("image") Then Return PictureItemTemplate
            If content.ContentType.StartsWith("audio") Then Return MusicItemTemplate
        End If

        Return DefaultTemplate
    End Function
End Class
```

### <a name="drag-and-drop-items-between-tree-views"></a>Glisser-déplacer des éléments entre des arborescences

L’exemple suivant montre comment créer deux arborescences autorisant le glissement-déplacement d’éléments entre elles. Quand vous faites glisser un élément d’une arborescence vers l’autre, il est ajouté à la fin de la liste. Vous pouvez toutefois retrier les éléments dans une arborescence. Notez que cet exemple ne prend en compte que les arborescences avec un seul nœud racine.

```xaml
<Page
    x:Class="TreeViewTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>

        <TreeView x:Name="treeView1"
                  AllowDrop="True"
                  CanDragItems="True"
                  CanReorderItems="True"
                  DragOver="TreeView_DragOver"
                  Drop="TreeView_Drop"
                  DragItemsStarting="TreeView_DragItemsStarting"
                  DragItemsCompleted="TreeView_DragItemsCompleted"/>
        <TreeView x:Name="treeView2"
                  AllowDrop="True"
                  Grid.Column="1"
                  CanDragItems="True"
                  CanReorderItems="True"
                  DragOver="TreeView_DragOver"
                  Drop="TreeView_Drop"
                  DragItemsStarting="TreeView_DragItemsStarting"
                  DragItemsCompleted="TreeView_DragItemsCompleted"/>

    </Grid>

</Page>
```

```csharp
using System;
using Windows.ApplicationModel.DataTransfer;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;

namespace TreeViewTest
{
    public sealed partial class MainPage : Page
    {
        private TreeViewNode deletedItem;
        private TreeView sourceTreeView;

        public MainPage()
        {
            this.InitializeComponent();
            InitializeTreeView();
        }

        private void InitializeTreeView()
        {
            TreeViewNode parentNode1 = new TreeViewNode() { Content = "tv1" };
            TreeViewNode parentNode2 = new TreeViewNode() { Content = "tv2" };

            parentNode1.Children.Add(new TreeViewNode() { Content = "tv1FirstChild" });
            parentNode1.Children.Add(new TreeViewNode() { Content = "tv1SecondChild" });
            parentNode1.Children.Add(new TreeViewNode() { Content = "tv1ThirdChild" });
            parentNode1.Children.Add(new TreeViewNode() { Content = "tv1FourthChild" });
            parentNode1.IsExpanded = true;
            treeView1.RootNodes.Add(parentNode1);

            parentNode2.Children.Add(new TreeViewNode() { Content = "tv2FirstChild" });
            parentNode2.Children.Add(new TreeViewNode() { Content = "tv2SecondChild" });
            parentNode2.IsExpanded = true;
            treeView2.RootNodes.Add(parentNode2);
        }

        private void TreeView_DragOver(object sender, DragEventArgs e)
        {
            if (e.DataView.Contains(StandardDataFormats.Text))
            {
                e.AcceptedOperation = DataPackageOperation.Move;
            }
        }

        private async void TreeView_Drop(object sender, DragEventArgs e)
        {
            if (e.DataView.Contains(StandardDataFormats.Text))
            {
                string text = await e.DataView.GetTextAsync();
                TreeView destinationTreeView = sender as TreeView;

                if (destinationTreeView.RootNodes != null)
                {
                    TreeViewNode newNode = new TreeViewNode() { Content = text };
                    destinationTreeView.RootNodes[0].Children.Add(newNode);
                    deletedItem = newNode;
                }
            }
        }

        private void TreeView_DragItemsStarting(TreeView sender, TreeViewDragItemsStartingEventArgs args)
        {
            if (args.Items.Count == 1)
            {
                args.Data.RequestedOperation = DataPackageOperation.Move;
                sourceTreeView = sender;

                foreach (var item in args.Items)
                {
                    args.Data.SetText(item.ToString());
                }
            }
        }

        private void TreeView_DragItemsCompleted(TreeView sender, TreeViewDragItemsCompletedEventArgs args)
        {
            var children = sourceTreeView.RootNodes[0].Children;

            if (deletedItem != null)
            {
                for (int i = 0; i < children.Count; i++)
                {
                    if (children[i].Content.ToString() == deletedItem.Content.ToString())
                    {
                        children.RemoveAt(i);
                        break;
                    }
                }
            }

            sourceTreeView = null;
            deletedItem = null;
        }
    }
}
```

## <a name="related-articles"></a>Articles connexes

- [Classe TreeView](/uwp/api/windows.ui.xaml.controls.treeview)
- [ListView, classe](/uwp/api/windows.ui.xaml.controls.listview)
- [ListView et GridView](listview-and-gridview.md)
