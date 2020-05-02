---
Description: Utilisez les contrôles ListView et GridView pour afficher et manipuler des jeux de données, comme une galerie d’images ou un ensemble d’e-mails.
title: Affichage Liste et affichage Grille
label: List view and grid view
template: detail.hbs
ms.date: 11/04/2019
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f8532ba0-5510-4686-9fcf-87fd7c643e7b
pm-contact: predavid
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: c130505ec79ca83698fd79df26464969afe79c36
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80893469"
---
# <a name="list-view-and-grid-view"></a>Affichage Liste et affichage Grille

La plupart des applications manipulent et affichent des jeux de données, par exemple, une galerie d’image ou un ensemble d’e-mails. L’infrastructure IU XAML fournit les contrôles ListView et GridView qui facilitent l’affichage et la manipulation des données dans votre application.  

> **API importantes** : [classe ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview), [classe GridView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridview), [propriété ItemsSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource), [propriété Items](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items)

> [!NOTE]
> Les contrôles ListView et GridView dérivent de la classe [ListViewBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase) ; ils ont donc les mêmes fonctionnalités mais affichent les données différemment. Dans cet article, quand nous parlons d’affichage de liste, sauf indication contraire, les informations s’appliquent aux contrôles ListView et GridView. Nous pouvons référencer des classes comme ListView ou ListViewItem, mais le préfixe *List* peut être remplacé par *Grid* pour l’équivalent de grille correspondant (GridView ou GridViewItem). 

ListView et GridView offrent de nombreux avantages en rapport avec l’utilisation de collections. Faciles à implémenter et à personnaliser, ils fournissent une interface utilisateur de base ainsi que des fonctionnalités d’interaction et de défilement. Vous pouvez lier ListView et GridView à des sources de données dynamiques existantes ou à des données codées en dur fournies dans le code XAML ou le code-behind. 

Bien que ces deux contrôles soient adaptés à de nombreux cas d’usage, ils conviennent globalement mieux aux collections dans lesquelles tous les éléments doivent avoir une structure de base, une apparence et un comportement d’interaction identiques. En d’autres termes, ces éléments exécutent tous la même action quand l’utilisateur clique dessus : ouvrir un lien, accéder à un emplacement, etc.


## <a name="differences-between-listview-and-gridview"></a>Différences entre ListView et GridView

### <a name="listview"></a>ListView
Le contrôle ListView affiche les données en les empilant dans une seule colonne. ListView fonctionne mieux avec les éléments qui ont du texte comme point focal et avec les collections censées être lues de haut en bas (classées par ordre alphabétique). ListView est souvent utilisé dans le cadre de listes de messages ou de résultats de recherche. Les collections qui doivent être affichées dans plusieurs colonnes ou dans un format de type table ne doivent _pas_ utiliser ListView, mais un [DataGrid](https://docs.microsoft.com/windows/communitytoolkit/controls/datagrid) à la place.

![Un affichage Liste avec des données regroupées](images/listview-grouped-example-resized-final.png)

### <a name="gridview"></a>GridView
GridView présente une collection d’éléments en lignes et en colonnes qui peut défiler sur le plan vertical. Les données sont empilées horizontalement jusqu’à ce qu’elles remplissent les colonnes, puis se poursuivent avec la ligne suivante. GridView fonctionne mieux avec les éléments qui ont des images comme point focal et avec les collections qui peuvent être lues d’un côté à l’autre ou qui ne sont pas triées dans un ordre spécifique. GridView est couramment utilisé dans les galeries de photos ou de produits.

![Exemple de bibliothèque de contenu](images/gridview-simple-example-final.png)

## <a name="which-collection-control-should-you-use-a-comparison-with-itemsrepeater"></a>Quel contrôle de collection devez-vous utiliser ? Comparaison avec ItemsRepeater

ListView et GridView sont des contrôles prêts à l’emploi qui permettent d’afficher n’importe quelle collection avec leurs propres interface utilisateur et expérience utilisateur intégrées. Le contrôle [ItemsRepeater](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/items-repeater) permet également d’afficher des collections, mais il a été conçu en tant que module pour créer un contrôle personnalisé qui répond à vos besoins exacts en matière d’interface utilisateur. Les différences les plus importantes qui doivent influer sur votre décision d’utiliser tel ou tel contrôle sont les suivantes :

-   ListView et GridView sont des contrôles riches en fonctionnalités qui nécessitent peu de personnalisations, mais qui offrent de nombreuses possibilités. ItemsRepeater est un module qui vous permet de créer votre propre contrôle de disposition, mais qui n’offre pas les mêmes fonctionnalités intégrées. Vous devez donc implémenter toutes les fonctionnalités ou interactions nécessaires.
-   Utilisez ItemsRepeater si vous avez une interface utilisateur très personnalisée que vous ne pouvez pas créer à l’aide de ListView ou GridView, ou si vous avez une source de données qui nécessite un comportement très différent pour chaque élément.


Pour en savoir plus sur ItemsRepeater, consultez ces [instructions](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/items-repeater) et la [documentation sur l’API](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.itemsrepeater?view=winui-2.2).

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour ouvrir l’application et voir l’objet <a href="xamlcontrolsgallery:/item/ListView">ListView</a> ou <a href="xamlcontrolsgallery:/item/GridView">GridView</a> en contexte.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-listview-or-gridview"></a>Créer un ListView ou GridView

ListView et GridView étant tous les deux des types [ItemsControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol), ils peuvent contenir une collection d’éléments de n’importe quel type. Des éléments doivent figurer dans leur collection [Items](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items) avant de pouvoir afficher quoi que ce soit à l’écran. Pour remplir l’affichage, vous pouvez ajouter des éléments directement à la collection [Items](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items) ou définir la propriété [ItemsSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) sur une source de données. 

> [!IMPORTANT]
> Vous pouvez utiliser Items ou ItemsSource pour remplir la liste, mais vous ne pouvez pas utiliser les deux en même temps. Si vous définissez la propriété ItemsSource et que vous ajoutez un élément en XAML, l’élément ajouté est ignoré. Si vous définissez la propriété ItemsSource et que vous ajoutez un élément à la collection Items dans le code, une exception est levée.

Par souci de simplicité, de nombreux exemples de cet article remplissent directement la collection `Items`. Toutefois, il est plus courant que les éléments d’une liste proviennent d’une source dynamique, par exemple, une liste de livres d’une base de données en ligne. Vous utilisez la propriété `ItemsSource` dans ce but. 

### <a name="add-items-to-a-listview-or-gridview"></a>Ajouter des éléments à un ListView ou GridView

Vous pouvez ajouter des éléments à la collection [Items](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items) de ListView ou de GridView en utilisant XAML ou du code pour obtenir le même résultat. Vous ajoutez généralement des éléments en XAML si vous avez un petit nombre d’éléments qui ne sont pas modifiés et sont facilement définis, ou si vous générez les éléments dans le code au moment du runtime. 

<u>Méthode 1 : Ajouter des éléments à la collection Items</u>
#### <a name="option-1-add-items-through-xaml"></a>Option 1 : Ajouter des éléments en XAML
```xml
<!-- No corresponding C# code is needed for this example. -->

<ListView x:Name="Fruits"> 
   <x:String>Apricot</x:String> 
   <x:String>Banana</x:String> 
   <x:String>Cherry</x:String> 
   <x:String>Orange</x:String> 
   <x:String>Strawberry</x:String> 
</ListView>  
```


#### <a name="option-2-add-items-through-c"></a>Option 2 : Ajouter des éléments en C#

##### <a name="c-code"></a>Code C# :
```csharp
// Create a new ListView and add content. 
ListView Fruits = new ListView(); 
Fruits.Items.Add("Apricot"); 
Fruits.Items.Add("Banana"); 
Fruits.Items.Add("Cherry"); 
Fruits.Items.Add("Orange"); 
Fruits.Items.Add("Strawberry");
 
// Add the ListView to a parent container in the visual tree (that you created in the corresponding XAML file).
FruitsPanel.Children.Add(Fruits); 
```

##### <a name="corresponding-xaml-code"></a>Code XAML correspondant :
```xml
<StackPanel Name="FruitsPanel"></StackPanel>
```
Les deux options ci-dessus génèrent le même ListView, comme indiqué ci-dessous :

![Affichage de liste simple](images/listview-basic-code-example2.png)
<br/>
<u>Méthode 2 : Ajouter des éléments en définissant ItemsSource</u>

En général, vous utilisez un contrôle ListView ou GridView pour afficher des données à partir d’une source telle qu’une base de données ou Internet. Pour renseigner un contrôle ListView/GridView à partir d’une source de données, affectez à sa propriété [ItemsSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) une collection d’éléments de données. Cette méthode fonctionne mieux si votre contrôle ListView ou GridView doit contenir des objets de classe personnalisés, comme indiqué dans les exemples ci-dessous.

#### <a name="option-1-set-itemssource-in-c"></a>Option 1 : Définir ItemsSource en C#
Ici, la propriété ItemsSource de l’affichage Liste prend la valeur de l’instance d’une collection directement dans le code. 

##### <a name="c-code"></a>Code C# :
```csharp 
// Class defintion should be provided within the namespace being used, outside of any other classes.

this.InitializeComponent();

// Instead of adding hard coded items to an ObservableCollection as shown below, 
//the data could be pulled asynchronously from a database or the internet.
ObservableCollection<Contact> Contacts = new ObservableCollection<Contact>();

// Contact objects are created by providing a first name, last name, and company for the Contact constructor.
// They are then added to the ObservableCollection Contacts.
Contacts.Add(new Contact("John", "Doe", "ABC Printers"));
Contacts.Add(new Contact("Jane", "Doe", "XYZ Refridgerators"));
Contacts.Add(new Contact("Santa", "Claus", "North Pole Toy Factory Inc."));

// Create a new ListView (or GridView) for the UI, add content by setting ItemsSource
ListView ContactsLV = new ListView();
ContactsLV.ItemsSource = Contacts;

// Add the ListView to a parent container in the visual tree (that you created in the corresponding XAML file)
ContactPanel.Children.Add(ContactsLV);
```

##### <a name="xaml-code"></a>Code XAML :
```xml
<StackPanel x:Name="ContactPanel"></StackPanel>
```

#### <a name="option-2-set-itemssource-in-xaml"></a>Option 2 : Définir ItemsSource en XAML
Vous pouvez également lier la propriété ItemsSource à une collection en XAML. Ici, ItemsSource est lié à une propriété publique nommée `Contacts` qui expose la collection de données privées de la page, `_contacts`.

**XAML**
```xml
<ListView x:Name="ContactsLV" ItemsSource="{x:Bind Contacts}"/>
```

**C#**
```csharp
// Class defintion should be provided within the namespace being used, outside of any other classes.
// These two declarations belong outside of the main page class.
private ObservableCollection<Contact> _contacts = new ObservableCollection<Contact>();

public ObservableCollection<Contact> Contacts
{
    get { return this._contacts; }
}

// This method should be defined within your main page class.
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    // Instead of hard coded items, the data could be pulled 
    // asynchronously from a database or the internet.
    Contacts.Add(new Contact("John", "Doe", "ABC Printers"));
    Contacts.Add(new Contact("Jane", "Doe", "XYZ Refridgerators"));
    Contacts.Add(new Contact("Santa", "Claus", "North Pole Toy Factory Inc."));
}
```

Les deux options ci-dessus génèrent le même ListView, comme indiqué ci-dessous. ListView affiche uniquement la représentation sous forme de chaîne de chaque élément, car nous n’avons pas fourni de modèle de données.

![Affichage de liste simple avec ItemsSource défini](images/listview-basic-code-example-final.png)

> [!IMPORTANT]
> Si aucun modèle de données n’est défini, les objets de classe personnalisés apparaissent uniquement dans ListView avec leur valeur de chaîne si une méthode [ToString()](https://docs.microsoft.com/uwp/api/windows.foundation.istringable.tostring) est définie.

 La section suivante décrit en détail comment représenter visuellement des éléments de classe simples et personnalisés dans un ListView ou GridView.

Pour plus d’informations sur la liaison de données, consultez [Vue d’ensemble de la liaison de données](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-quickstart).

> [!NOTE]
> Si vous avez besoin d’afficher des données groupées dans votre ListView, vous devez effectuer une liaison à un [CollectionViewSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource). CollectionViewSource agit en tant que proxy pour la classe de collection dans XAML et active la prise en charge du regroupement. Pour plus d’informations, consultez [CollectionViewSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource).

## <a name="customizing-the-look-of-items-with-a-datatemplate"></a>Personnalisation de l’apparence d’éléments avec un DataTemplate

Un modèle de données dans un ListView ou GridView définit la manière dont les éléments/données sont visualisés. Par défaut, un élément de données est affiché dans le ListView en tant que représentation sous forme de chaîne de l’objet de données auquel il est lié. Vous pouvez afficher la représentation chaîne d’une propriété spécifique de l’élément de données en définissant [DisplayMemberPath](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.displaymemberpath) sur cette propriété.

Toutefois, en général, on souhaite afficher une représentation enrichie des données. Pour définir précisément la façon dont les éléments sont affichés dans le ListView/GridView, créez un [DataTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate). Le code XAML dans l’objet DataTemplate définit la disposition et l’apparence des contrôles qui permettent d’afficher un élément individuel. Les contrôles dans la disposition peuvent être liés aux propriétés d’un objet de données ou avoir du contenu statique défini inline. 

> [!NOTE]
> Si vous utilisez l’[extension de balisage x:Bind](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) dans un DataTemplate, vous devez spécifier le DataType (`x:DataType`) sur le DataTemplate.

#### <a name="simple-listview-data-template"></a>Modèle de données ListView simple
Dans cet exemple, l’élément de données est une chaîne simple. Un DataTemplate est défini inline dans la définition ListView pour ajouter une image à gauche de la chaîne et afficher la chaîne en bleu vert. Il s’agit du même ListView que celui créé à l’aide de la méthode 1 et de l’option 1 présentées ci-dessus.

**XAML**
```XML
<!--No corresponding C# code is needed for this example.-->
<ListView x:Name="FruitsList">
                <ListView.ItemTemplate>
                    <DataTemplate x:DataType="x:String">
                        <Grid>
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="47"/>
                                <ColumnDefinition/>
                            </Grid.ColumnDefinitions>
                            <Image Source="Assets/placeholder.png" Width="32" Height="32"
                                HorizontalAlignment="Left" VerticalAlignment="Center"/>
                            <TextBlock Text="{x:Bind}" Foreground="Teal" FontSize="14" 
                                Grid.Column="1" VerticalAlignment="Center"/>
                        </Grid>
                    </DataTemplate>
                </ListView.ItemTemplate>
                <x:String>Apricot</x:String>
                <x:String>Banana</x:String>
                <x:String>Cherry</x:String>
                <x:String>Orange</x:String>
                <x:String>Strawberry</x:String>
            </ListView>

```

Voici à quoi ressemblent les éléments de données affichés avec ce modèle de données dans un ListView :

![Éléments ListView avec un modèle de données](images/listview-w-datatemplate1-final.png)

#### <a name="listview-data-template-for-custom-class-objects"></a>Modèle de données ListView pour des objets de classe personnalisés
Dans cet exemple, l’élément de données est un objet Contact. Un DataTemplate est défini inline dans la définition ListView pour ajouter une image du contact à gauche du nom et de la société du contact. Ce ListView a été créé à l’aide de la méthode 2 et de l’option 2 mentionnées ci-dessus.
```xml
<ListView x:Name="ContactsLV" ItemsSource="{x:Bind Contacts}">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:Contact">
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="*"/>
                    <RowDefinition Height="*"/>
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto"/>
                    <ColumnDefinition Width="*"/>
                </Grid.ColumnDefinitions>
                <Image Grid.Column="0" Grid.RowSpan="2" Source="Assets/grey-placeholder.png" Width="32"
                    Height="32" HorizontalAlignment="Center" VerticalAlignment="Center"></Image>
                <TextBlock Grid.Column="1" Text="{x:Bind Name}" Margin="12,6,0,0" 
                    Style="{ThemeResource BaseTextBlockStyle}"/>
                <TextBlock  Grid.Column="1" Grid.Row="1" Text="{x:Bind Company}" Margin="12,0,0,6" 
                    Style="{ThemeResource BodyTextBlockStyle}"/>
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Voici à quoi ressemblent les éléments de données affichés avec ce modèle de données dans un ListView :

![Éléments de classe personnalisés ListView avec un modèle de données](images/listview-customclass-datatemplate-final.png)

Les modèles de données sont le principal moyen de définir l’aspect de votre ListView. Ils peuvent également avoir un impact significatif sur les performances si votre liste contient un grand nombre d’éléments.  

Votre modèle de données peut être défini inline dans la définition ListView/GridView (illustrée ci-dessus) ou séparément dans une section de ressources. S’il est défini en dehors de ListView/GridView, le DataTemplate doit avoir un attribut [x:Key](https://docs.microsoft.com/windows/uwp/xaml-platform/x-key-attribute) et être affecté à la propriété [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) de ListView ou GridView à l’aide de cette clé.

Pour plus d’informations et pour obtenir des exemples d’utilisation de modèles de données et de conteneurs d’éléments afin de définir l’apparence des éléments dans votre liste ou grille, consultez [Modèles et conteneurs d’éléments](item-containers-templates.md). 

## <a name="change-the-layout-of-items"></a>Modifier la disposition des éléments

Quand vous ajoutez des éléments à un ListView ou GridView, le contrôle encapsule automatiquement chaque élément dans un conteneur d’éléments, puis dispose tous les conteneurs d’éléments. La manière dont ces conteneurs d’éléments sont disposés dépend de l’élément [ItemsPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel) du contrôle.  
- Par défaut, **ListView** utilise un élément [ItemsStackPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemsstackpanel), ce qui donne une liste verticale, comme ceci.

![Un affichage Liste simple](images/listview-simple.png)

- **GridView** utilise un élément [ItemsWrapGrid](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemswrapgrid), ce qui ajoute les éléments horizontalement, enveloppe et défile verticalement, comme ceci.

![Un affichage Grille simple](images/gridview-simple.png)

Vous pouvez modifier la disposition des éléments en ajustant les propriétés sur le panneau d’éléments, ou remplacer le panneau par défaut par un autre panneau.

> [!NOTE]
> Veillez à ne pas désactiver la virtualisation si vous changez ItemsPanel. **ItemsStackPanel** et **ItemsWrapGrid** prennent en charge la virtualisation, leur utilisation est donc sécurisée. Si vous utilisez tout autre panneau, vous pourriez désactiver la virtualisation et ralentir les performances de l’affichage Liste. Pour plus d’informations, voir les articles relatifs à l’affichage Liste sous [Performances](https://docs.microsoft.com/windows/uwp/debug-test-perf/performance-and-xaml-ui). 

Cet exemple montre comment créer une disposition **ListView** avec des conteneurs d’éléments dans une liste horizontale en modifiant la propriété [Orientation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemsstackpanel.orientation) de **ItemsStackPanel**.
Étant donné que l’affichage Liste défile verticalement par défaut, vous devez également ajuster certaines propriétés sur l’élément interne [ScrollViewer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer) de l’affichage Liste pour le faire défiler horizontalement.
- [ScrollViewer.HorizontalScrollMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode) sur **Activé** ou **Auto**
- [ScrollViewer.HorizontalScrollBarVisibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollbarvisibility) sur **Auto** 
- [ScrollViewer.VerticalScrollMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollmode) sur **Désactivé** 
- [ScrollViewer.VerticalScrollBarVisibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibility) sur **Masqué** 

> [!IMPORTANT]
> Ces exemples étant affichés avec une largeur non limitée de l’affichage Liste, les barres de défilement horizontales ne sont pas visibles. Si vous exécutez ce code, vous pouvez définir `Width="180"` sur ListView pour afficher les barres de défilement.

**XAML**
```xml
<ListView Height="60" 
          ScrollViewer.HorizontalScrollMode="Enabled" 
          ScrollViewer.HorizontalScrollBarVisibility="Auto"
          ScrollViewer.VerticalScrollMode="Disabled"
          ScrollViewer.VerticalScrollBarVisibility="Hidden">
    <ListView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsStackPanel Orientation="Horizontal"/>
        </ItemsPanelTemplate>
    </ListView.ItemsPanel>
    <x:String>Apricot</x:String>
    <x:String>Banana</x:String>
    <x:String>Cherry</x:String>
    <x:String>Orange</x:String>
    <x:String>Strawberry</x:String>
</ListView>
```

La liste résultante ressemble à ce qui suit.

![Un affichage Liste horizontal](images/listview-horizontal2-final.png)

 Dans l’exemple suivant, **ListView** dispose les éléments dans une liste d’habillage verticale en utilisant **ItemsWrapGrid** au lieu d’**ItemsStackPanel**. 
 
> [!IMPORTANT]
> La hauteur de l’affichage Liste doit être limitée pour forcer le contrôle à encapsuler les conteneurs.

**XAML**
```xml
<ListView Height="100"
          ScrollViewer.HorizontalScrollMode="Enabled" 
          ScrollViewer.HorizontalScrollBarVisibility="Auto"
          ScrollViewer.VerticalScrollMode="Disabled"
          ScrollViewer.VerticalScrollBarVisibility="Hidden">
    <ListView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid/>
        </ItemsPanelTemplate>
    </ListView.ItemsPanel>
    <x:String>Apricot</x:String>
    <x:String>Banana</x:String>
    <x:String>Cherry</x:String>
    <x:String>Orange</x:String>
    <x:String>Strawberry</x:String>
</ListView>
```

La liste résultante ressemble à ce qui suit.

![Un affichage Liste avec disposition de grille](images/listview-itemswrapgrid2-final.png)

Si vous affichez des données groupées dans votre affichage Liste, ItemsPanel détermine la manière dont les groupes d’éléments (et non les éléments individuels) sont disposés. Par exemple, si l’élément ItemsStackPanel horizontal présenté précédemment est utilisé pour afficher les données groupées, les groupes sont organisés horizontalement, mais les éléments dans chaque groupe sont toujours empilés verticalement, comme illustré ici.

![Un affichage Liste horizontal groupé](images/listview-horizontal-groups.png)

## <a name="item-selection-and-interaction"></a>Sélection d’éléments et interaction

Vous pouvez choisir différentes façons de permettre à un utilisateur d’interagir avec un affichage Liste. Par défaut, un utilisateur peut sélectionner un seul élément. Vous pouvez modifier la propriété [SelectionMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectionmode) afin d’autoriser la sélection multiple ou de désactiver la sélection. Vous pouvez définir la propriété [IsItemClickEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled) pour qu’un utilisateur clique sur un élément pour appeler une action (par exemple, un bouton) au lieu de sélectionner l’élément.

> **Remarque**&nbsp;&nbsp;ListView et GridView utilisent l’énumération [ListViewSelectionMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewselectionmode) pour leur propriété SelectionMode. IsItemClickEnabled est défini sur **False** par défaut, vous devez le définir uniquement pour activer le mode clic.

Le tableau suivant montre les moyens dont un utilisateur dispose pour interagir avec un affichage Liste, et comment vous pouvez répondre à l’interaction.

Pour activer cette interaction : | Utilisez ces paramètres : | Gérez cet événement : | Utilisez cette propriété pour obtenir l’élément sélectionné :
----------------------------|---------------------|--------------------|--------------------------------------------
Aucune interaction | [SelectionMode ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectionmode) = **Aucun**, [IsItemClickEnabledFalse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled) = **False** | NON APPLICABLE | NON APPLICABLE 
Sélection unique | SelectionMode = **Simple**, IsItemClickEnabled = **False** | [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) | [SelectedItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem), [SelectedIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex)  
Sélection multiple | SelectionMode = **Multiple**, IsItemClickEnabled = **False** | [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) | [SelectedItems](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selecteditems)  
Sélection étendue | SelectionMode = **Étendu**, IsItemClickEnabled = **False** | [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) | [SelectedItems](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selecteditems)  
Cliquez sur | SelectionMode = **Aucun**, IsItemClickEnabled = **True** | [ItemClick](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.itemclick) | NON APPLICABLE 

> **Remarque**&nbsp;&nbsp;À compter de Windows 10, vous pouvez activer IsItemClickEnabled pour déclencher un événement ItemClick pendant que SelectionMode est également défini sur Simple, Multiple ou Étendu. De cette façon, l’événement ItemClick est déclenché en premier suivi de l’événement SelectionChanged. Dans certains cas, par exemple, si vous accédez à une autre page dans le gestionnaire d’événements ItemClick, l’événement SelectionChanged n’est pas déclenché et l’élément n’est pas sélectionné.

Vous pouvez définir ces propriétés dans XAML ou dans le code, comme illustré ici.

**XAML**
```xaml
<ListView x:Name="myListView" SelectionMode="Multiple"/>

<GridView x:Name="myGridView" SelectionMode="None" IsItemClickEnabled="True"/> 
```

**C#**
```csharp
myListView.SelectionMode = ListViewSelectionMode.Multiple; 

myGridView.SelectionMode = ListViewSelectionMode.None;
myGridView.IsItemClickEnabled = true;
```

### <a name="read-only"></a>Lecture seule

Vous pouvez définir la propriété SelectionMode sur **ListViewSelectionMode.None** pour désactiver la sélection d’éléments. Cela place le contrôle en mode lecture seule : vous pouvez l’utiliser pour afficher des données, mais pas pour interagir avec celles-ci. Le contrôle lui-même n’est pas désactivé, seule la sélection d’éléments est désactivée.

### <a name="single-selection"></a>Sélection unique

Le tableau suivant décrit le clavier, la souris et les interactions tactiles, lorsque SelectionMode est défini sur **Simple**.

Touche de modification | Interaction
-------------|------------
Aucune | <li>Un utilisateur peut sélectionner un seul élément à l’aide de la barre d’espace, du clic de souris ou de l’appui tactile.</li>
Ctrl | <li>Un utilisateur peut désélectionner un seul élément à l’aide de la barre d’espace, du clic de souris ou de l’appui tactile.</li><li>À l’aide des touches de direction, un utilisateur peut déplacer le focus indépendamment de la sélection.</li>

Lorsque SelectionMode est défini sur **Simple**, vous pouvez obtenir l’élément de données sélectionné à partir de la propriété [SelectedItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem). Vous pouvez obtenir l’index dans la collection de l’élément sélectionné en utilisant la propriété [SelectedIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex). Si aucun élément n’est sélectionné, SelectedItem est défini sur **null**, et SelectedIndex est -1. 
 
Si vous essayez de définir un élément qui n’est pas dans la collection **Items** en tant que **SelectedItem**, l’opération est ignorée et SelectedItem est **null**. Toutefois, si vous tentez de définir **SelectedIndex** sur un index en dehors de la plage **Items** dans la liste, une exception **System.ArgumentException** se produit. 

### <a name="multiple-selection"></a>Sélection multiple

Le tableau suivant décrit le clavier, la souris et les interactions tactiles, lorsque SelectionMode est défini sur **Multiple**.

Touche de modification | Interaction
-------------|------------
Aucune | <li>Un utilisateur peut sélectionner plusieurs éléments à l’aide de la barre d’espace, du clic de souris ou de l’appui tactile pour activer/désactiver la sélection sur l’élément sélectionné.</li><li>À l’aide des touches de direction, un utilisateur peut déplacer le focus indépendamment de la sélection.</li>
Maj | <li>Un utilisateur peut sélectionner plusieurs éléments contigus en cliquant ou en appuyant sur le premier élément de la sélection, puis sur le dernier.</li><li>À l’aide des touches de direction, un utilisateur peut créer une sélection contiguë à partir de l’élément sélectionné lorsque la touche MAJ est enfoncée.</li>

### <a name="extended-selection"></a>Sélection étendue

Le tableau suivant décrit le clavier, la souris et les interactions tactiles, lorsque SelectionMode est défini sur **Étendu**.

Touche de modification | Interaction
-------------|------------
Aucune | <li>Le comportement est identique à celui de la sélection **Simple**.</li>
Ctrl | <li>Un utilisateur peut sélectionner plusieurs éléments à l’aide de la barre d’espace, du clic de souris ou de l’appui tactile pour activer/désactiver la sélection sur l’élément sélectionné.</li><li>À l’aide des touches de direction, un utilisateur peut déplacer le focus indépendamment de la sélection.</li>
Maj | <li>Un utilisateur peut sélectionner plusieurs éléments contigus en cliquant ou en appuyant sur le premier élément de la sélection, puis sur le dernier.</li><li>À l’aide des touches de direction, un utilisateur peut créer une sélection contiguë à partir de l’élément sélectionné lorsque la touche MAJ est enfoncée.</li>

Lorsque SelectionMode est défini sur **Multiple** ou **Étendu**, vous pouvez obtenir les éléments de données sélectionnés à partir de la propriété [SelectedItems](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selecteditems). 

Les propriétés **SelectedIndex**, **SelectedItem** et **SelectedItems** sont synchronisées. Par exemple, si vous définissez SelectedIndex sur -1, SelectedItem est défini sur **null** et SelectedItems est vide ; si vous définissez SelectedItem sur **null**, SelectedIndex est défini sur -1 et SelectedItems est vide.

En mode de sélection multiple, **SelectedItem** contient l’élément qui a été sélectionné en premier, et **Selectedindex** contient l’index de l’élément qui a été sélectionné en premier. 

### <a name="respond-to-selection-changes"></a>Répondre aux modifications de sélection

En réaction aux modifications de sélection dans un affichage Liste, gérez l’événement [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged). Dans le code du gestionnaire d’événements, vous pouvez obtenir la liste des éléments sélectionnés auprès de la propriété [SelectionChangedEventArgs.AddedItems](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems). Vous pouvez obtenir tous les éléments qui ont été désélectionnés à partir de la propriété [SelectionChangedEventArgs.RemovedItems](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.removeditems). Les collections AddedItems et RemovedItems contiennent au maximum 1 élément, sauf si l’utilisateur sélectionne une plage d’éléments en maintenant la touche MAJ enfoncée.

Cet exemple montre comment gérer l’événement **SelectionChanged** et accéder à des collections d’éléments différents.

**XAML**
```xml
<StackPanel HorizontalAlignment="Right">
    <ListView x:Name="listView1" SelectionMode="Multiple" 
              SelectionChanged="ListView1_SelectionChanged">
        <x:String>Apricot</x:String>
        <x:String>Banana</x:String>
        <x:String>Cherry</x:String>
        <x:String>Orange</x:String>
        <x:String>Strawberry</x:String>
    </ListView>
    <TextBlock x:Name="selectedItem"/>
    <TextBlock x:Name="selectedIndex"/>
    <TextBlock x:Name="selectedItemCount"/>
    <TextBlock x:Name="addedItems"/>
    <TextBlock x:Name="removedItems"/>
</StackPanel> 
```

**C#**
```csharp
private void ListView1_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (listView1.SelectedItem != null)
    {
        selectedItem.Text = 
            "Selected item: " + listView1.SelectedItem.ToString();
    }
    else
    {
        selectedItem.Text = 
            "Selected item: null";
    }
    selectedIndex.Text = 
        "Selected index: " + listView1.SelectedIndex.ToString();
    selectedItemCount.Text = 
        "Items selected: " + listView1.SelectedItems.Count.ToString();
    addedItems.Text = 
        "Added: " + e.AddedItems.Count.ToString();
    removedItems.Text = 
        "Removed: " + e.RemovedItems.Count.ToString();
}
```

### <a name="click-mode"></a>Mode clic

Vous pouvez modifier un affichage Liste afin qu’un utilisateur clique sur des éléments tels que des boutons au lieu de les sélectionner. Cela est par exemple utile lorsque l’utilisateur de votre application accède à une nouvelle page en cliquant sur un élément dans une liste ou dans une grille. Pour activer ce comportement :
- Définissez **SelectionMode** sur **Aucun**.
- Définissez **IsItemClickEnabled** sur **true**.
- Gérez l’événement **ItemClick** pour effectuer une action lorsque l’utilisateur clique sur un élément.

Voici un affichage Liste constitué d’éléments pouvant être activés par un clic. Le code dans le gestionnaire d’événement ItemClick accède à une nouvelle page.

**XAML**
```xml
<ListView SelectionMode="None"
          IsItemClickEnabled="True" 
          ItemClick="ListView1_ItemClick">
    <x:String>Page 1</x:String>
    <x:String>Page 2</x:String>
    <x:String>Page 3</x:String>
    <x:String>Page 4</x:String>
    <x:String>Page 5</x:String>
</ListView>
```

**C#**
```csharp
private void ListView1_ItemClick(object sender, ItemClickEventArgs e)
{
    switch (e.ClickedItem.ToString())
    {
        case "Page 1":
            this.Frame.Navigate(typeof(Page1));
            break;

        case "Page 2":
            this.Frame.Navigate(typeof(Page2));
            break;

        case "Page 3":
            this.Frame.Navigate(typeof(Page3));
            break;

        case "Page 4":
            this.Frame.Navigate(typeof(Page4));
            break;

        case "Page 5":
            this.Frame.Navigate(typeof(Page5));
            break;

        default:
            break;
    }
}
```

### <a name="select-a-range-of-items-programmatically"></a>Sélectionner une plage d’éléments par programme

Parfois, vous avez besoin de manipuler la sélection d’éléments d’un affichage Liste par programme. Par exemple, vous pourriez avoir un bouton **Sélectionner tout** pour permettre à un utilisateur de sélectionner tous les éléments dans une liste. Dans ce cas, il n’est généralement pas très efficace d’ajouter et de supprimer des éléments de la collection SelectedItems un par un. Chaque modification d’élément déclenche un événement SelectionChanged. Lorsque vous travaillez avec des éléments directement au lieu de fonctionner avec des valeurs d’index, les éléments sont dévirtualisés.

Les méthodes [SelectAll](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectall), [SelectRange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectrange), et [DeselectRange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.deselectrange) fournissent un moyen plus efficace de modifier la sélection que l’utilisation de la propriété SelectedItems. Ces méthodes sélectionnent ou désélectionnent à l’aide de plages d’index d’élément. Les éléments qui sont virtualisés le restent, car seul l’index est utilisé. Tous les éléments dans la plage spécifiée sont sélectionnés (ou désélectionnés), quel que soit leur état de sélection d’origine. L’événement SelectionChanged ne se produit qu’une seule fois pour chaque appel à ces méthodes.

> [!IMPORTANT]
> Vous devez appeler ces méthodes uniquement quand la propriété SelectionMode a la valeur Multiple ou Extended. Si vous appelez SelectRange quand SelectionMode est défini sur Simple, ou Aucun, une exception est levée.

Lorsque vous sélectionnez des éléments utilisant des plages d’index, utilisez la propriété [SelectedRanges](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectedranges) pour obtenir toutes les plages sélectionnées dans la liste.

Si ItemsSource implémente [IItemsRangeInfo](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.iitemsrangeinfo), et que vous utilisez ces méthodes pour modifier la sélection, les propriétés **AddedItems** et **RemovedItems** ne sont pas définies dans SelectionChangedEventArgs. La définition de ces propriétés nécessite la dévirtualisation de l’objet d’élément. Utilisez la propriété **SelectedRanges** pour obtenir les éléments à la place.

Vous pouvez sélectionner tous les éléments dans une collection en appelant la méthode SelectAll. Toutefois, il n’existe aucune méthode correspondante pour désélectionner tous les éléments. Vous pouvez désélectionner tous les éléments en appelant DeselectRange et en transmettant [ItemIndexRange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.itemindexrange) avec une valeur [FirstIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.itemindexrange.firstindex) de 0 et une valeur [Longueur](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.itemindexrange.length) égale au nombre d’éléments dans la collection. L’exemple ci-dessous illustre ceci, ainsi qu’une option permettant de sélectionner tous les éléments.

**XAML**
```xml
<StackPanel Width="160">
    <Button Content="Select all" Click="SelectAllButton_Click"/>
    <Button Content="Deselect all" Click="DeselectAllButton_Click"/>
    <ListView x:Name="listView1" SelectionMode="Multiple">
        <x:String>Apricot</x:String>
        <x:String>Banana</x:String>
        <x:String>Cherry</x:String>
        <x:String>Orange</x:String>
        <x:String>Strawberry</x:String>
    </ListView>
</StackPanel>
```

**C#**
```csharp
private void SelectAllButton_Click(object sender, RoutedEventArgs e)
{
    if (listView1.SelectionMode == ListViewSelectionMode.Multiple ||
        listView1.SelectionMode == ListViewSelectionMode.Extended)
    {
        listView1.SelectAll();
    }
}

private void DeselectAllButton_Click(object sender, RoutedEventArgs e)
{
    if (listView1.SelectionMode == ListViewSelectionMode.Multiple ||
        listView1.SelectionMode == ListViewSelectionMode.Extended)
    {
        listView1.DeselectRange(new ItemIndexRange(0, (uint)listView1.Items.Count));
    }
}
```

Pour plus d’informations sur la modification de l’aspect des éléments sélectionnés, consultez [Modèles et conteneurs d’éléments](item-containers-templates.md).

### <a name="drag-and-drop"></a>Glisser-déplacer

Les contrôles ListView et GridView prennent en charge le glisser-déplacer des éléments à l’intérieur d’eux-mêmes et entre eux-mêmes et d’autres contrôles ListView et GridView. Pour plus d’informations sur l’implémentation du modèle glisser-déplacer, voir [Glisser-déplacer](../input/drag-and-drop.md).

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemple de ListView et GridView XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView) - Illustre les contrôles ListView et GridView.
- [Exemple de fonctionnalité glisser-déposer XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlDragAndDrop) - Illustre la fonctionnalité glisser-déposer avec le contrôle ListView.
- [Exemple de Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) : tous les contrôles XAML dans un format interactif.

## <a name="related-articles"></a>Articles connexes

- [Listes](lists.md)
- [Modèles et conteneurs d’éléments](item-containers-templates.md)
- [Glisser-déplacer](../input/drag-and-drop.md)
