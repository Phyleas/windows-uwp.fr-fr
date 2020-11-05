---
description: TabView offre un moyen flexible d’organiser plusieurs documents dans des onglets dynamiques
title: Vue d’onglets
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f34e2a882746ac833d2b78373a96496c1f079864
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034772"
---
# <a name="tabview"></a>TabView

Le contrôle TabView permet d’afficher un ensemble d’onglets et leur contenu respectif. Les TabViews sont utiles pour afficher plusieurs pages (ou documents) de contenu tout en donnant à un utilisateur la possibilité de réorganiser, d’ouvrir ou de fermer de nouveaux onglets.

![Exemple de TabView](images/tabview/tab-introduction.png)

**Obtenir la bibliothèque d’interface utilisateur Windows**

:::row:::
   :::column:::
      ![Logo WinUI](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      Le contrôle **TabView** nécessite la bibliothèque d’interface utilisateur Windows, package NuGet qui contient les nouveaux contrôles et fonctionnalités d’interface utilisateur destinés aux applications Windows. Pour plus d’informations, notamment des instructions d’installation, consultez la [bibliothèque d’interface utilisateur Windows](/uwp/toolkits/winui/).
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **API de la bibliothèque d’interface utilisateur Windows**  : [Classe TabView](/uwp/api/microsoft.ui.xaml.controls.tabview), [Classe TabViewItem](/uwp/api/microsoft.ui.xaml.controls.tabviewitem)

> [!TIP]
> Tout au long de ce document, nous utilisons l’alias **muxc** en XAML pour représenter les API de la bibliothèque d’interface utilisateur Windows que nous avons incluses dans notre projet. Nous l’avons ajouté à notre élément [Page](/uwp/api/windows.ui.xaml.controls.page) : `xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`
>
>Dans le code-behind, nous utilisons également l’alias **muxc** en C# pour représenter les API de la bibliothèque d’interface utilisateur Windows que nous avons incluses dans notre projet. Nous avons ajouté cette instruction **using** en haut du fichier : `using muxc = Microsoft.UI.Xaml.Controls;`

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

En général, les interfaces utilisateur avec onglets existent dans deux styles distincts qui diffèrent en termes de fonction et d’apparence : Les **onglets statiques** sont le genre d’onglets souvent présents dans les fenêtres de paramètres. Ils contiennent un nombre défini de pages dans un ordre fixe, qui contiennent généralement du contenu prédéfini.
Les **onglets de document** sont le type d’onglets présents dans un navigateur, par exemple Microsoft Edge. Les utilisateurs peuvent créer, supprimer et réorganiser les onglets, déplacer les onglets entre les fenêtres et modifier le contenu des onglets.

[TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) propose des onglets de document pour les applications UWP. Utilisez un TabView quand :

- Les utilisateurs pourront ouvrir, fermer ou réorganiser dynamiquement les onglets.
- Les utilisateurs pourront ouvrir des documents ou des pages web directement dans des onglets.
- Les utilisateurs pourront glisser-déplacer des onglets entre les fenêtres.

Si un TabView n’est pas approprié pour votre application, utilisez des contrôles tels que [Pivot](./pivot.md) ou [NavigationView](./navigationview.md).

## <a name="anatomy"></a>Anatomie

L’image ci-dessous montre les parties du contrôle [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview). Le TabStrip a un en-tête et un pied de page, mais contrairement à un document, l’en-tête et le pied de page de TabStrip sont situés respectivement à l’extrême gauche et à l’extrême droite de la bande.

![Anatomie du contrôle TabView](images/tabview/tab-view-anatomy.png)

L’image suivante montre les parties du contrôle [TabViewItem](/uwp/api/microsoft.ui.xaml.controls.tabviewitem). Notez que bien que le contenu soit affiché dans le contrôle TabView, en réalité il fait partie du TabViewItem.

![Anatomie du contrôle TabViewItem](images/tabview/tab-control-anatomy.png)

### <a name="create-a-tab-view"></a>Créer une vue d’onglets

Cet exemple crée un [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) simple avec des gestionnaires d’événements pour prendre en charge l’ouverture et la fermeture des onglets.

```xaml
 <muxc:TabView AddTabButtonClick="TabView_AddTabButtonClick"
               TabCloseRequested="TabView_TabCloseRequested"/>
```

```csharp
// Add a new Tab to the TabView
private void TabView_AddTabButtonClick(muxc.TabView sender, object args)
{
    var newTab = new muxc.TabViewItem();
    newTab.IconSource = new muxc.SymbolIconSource() { Symbol = Symbol.Document };
    newTab.Header = "New Document";

    // The Content of a TabViewItem is often a frame which hosts a page.
    Frame frame = new Frame();
    newTab.Content = frame;
    frame.Navigate(typeof(Page1));

    sender.TabItems.Add(newTab);
}

// Remove the requested tab from the TabView
private void TabView_TabCloseRequested(muxc.TabView sender, muxc.TabViewTabCloseRequestedEventArgs args)
{
    sender.TabItems.Remove(args.Tab);
}
```

## <a name="behavior"></a>Comportement

Il existe plusieurs façons de tirer parti ou d’étendre la fonctionnalité d’un [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview).

### <a name="bind-tabitemssource-to-a-tabviewitemcollection"></a>Lier TabItemsSource à un TabViewItemCollection

```xaml
<muxc:TabView TabItemsSource="{x:Bind TabViewItemCollection}" />
```

### <a name="display-tabview-tabs-in-a-windows-titlebar"></a>Afficher des onglets TabView dans la barre de titre d’une fenêtre

Au lieu de faire en sorte que les onglets occupent leur propre ligne sous la barre de titre d’une fenêtre, vous pouvez fusionner ces deux éléments dans la même zone. Cela permet d’économiser de l’espace vertical pour votre contenu, et donne à votre application un aspect moderne.

Étant donné qu’un utilisateur peut faire glisser une fenêtre par sa barre de titre pour la repositionner, il est important que la barre de titre ne soit pas complètement remplie avec des onglets. Ainsi, lors de l’affichage d’onglets dans une barre de titre, vous devez spécifier une partie de la barre de titre à réserver comme zone qui peut être glissée. Si vous ne spécifiez pas de zone pouvant être glissée, l’intégralité de la barre de titre pourra être glissée, ce qui empêchera vos onglets de recevoir des événements d’entrée. Si votre TabView s’affiche dans la barre de titre d’une fenêtre, vous devez toujours inclure un [TabStripFooter](/uwp/api/microsoft.ui.xaml.controls.tabview.tabstripfooter) dans votre [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) et le marquer en tant que zone pouvant être glissée.

Pour plus d’informations, consultez [Personnalisation de la barre de titre](../shell/title-bar.md).

![Onglets dans la barre de titre](images/tabview/tab-extend-to-title.png)

```xaml
<muxc:TabView HorizontalAlignment="Stretch" VerticalAlignment="Stretch">
    <muxc:TabViewItem Header="Home" IsClosable="False">
        <muxc:TabViewItem.IconSource>
            <muxc:SymbolIconSource Symbol="Home" />
        </muxc:TabViewItem.IconSource>
    </muxc:TabViewItem>
    <muxc:TabViewItem Header="Document 1">
        <muxc:TabViewItem.IconSource>
            <muxc:SymbolIconSource Symbol="Document" />
        </muxc:TabViewItem.IconSource>
    </muxc:TabViewItem>
    <muxc:TabViewItem Header="Document 2">
        <muxc:TabViewItem.IconSource>
            <muxc:SymbolIconSource Symbol="Document" />
        </muxc:TabViewItem.IconSource>
    </muxc:TabViewItem>
    <muxc:TabViewItem Header="Document 3">
        <muxc:TabViewItem.IconSource>
            <muxc:SymbolIconSource Symbol="Document" />
        </muxc:TabViewItem.IconSource>
    </muxc:TabViewItem>

    <muxc:TabView.TabStripHeader>
        <Grid x:Name="ShellTitlebarInset" Background="Transparent" />
    </muxc:TabView.TabStripHeader>
    <muxc:TabView.TabStripFooter>
        <Grid x:Name="CustomDragRegion" Background="Transparent" />
    </muxc:TabView.TabStripFooter>
</muxc:TabView>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
    coreTitleBar.ExtendViewIntoTitleBar = true;
    coreTitleBar.LayoutMetricsChanged += CoreTitleBar_LayoutMetricsChanged;

    Window.Current.SetTitleBar(CustomDragRegion);
}

private void CoreTitleBar_LayoutMetricsChanged(CoreApplicationViewTitleBar sender, object args)
{
    if (FlowDirection == FlowDirection.LeftToRight)
    {
        CustomDragRegion.MinWidth = sender.SystemOverlayRightInset;
        ShellTitlebarInset.MinWidth = sender.SystemOverlayLeftInset;
    }
    else
    {
        CustomDragRegion.MinWidth = sender.SystemOverlayLeftInset;
        ShellTitlebarInset.MinWidth = sender.SystemOverlayRightInset;
    }

    CustomDragRegion.Height = ShellTitlebarInset.Height = sender.Height;
}
```

>[!NOTE]
> Pour être sûr que les onglets de la barre de titre ne sont pas masqués par le contenu de l’interpréteur de commandes, vous devez tenir compte des superpositions gauche et droite. Dans les dispositions LTR, l’incrustation droite comprend les boutons de légende et la zone de glissement. Dans les dispositions RTL, c’est l’inverse. Les valeurs SystemOverlayLeftInset et SystemOverlayRightInset sont des valeurs gauche et droite physiques. Par conséquent, vous devez aussi les inverser en cas de disposition RTL.

### <a name="control-overflow-behavior"></a>Comportement de dépassement de capacité de contrôle

Quand la barre d’onglets contient de nombreux onglets, vous pouvez contrôler leur affichage en définissant [TabView.TabWidthMode](/uwp/api/microsoft.ui.xaml.controls.tabview.tabwidthmode).

| Valeur TabWidthMode | Comportement                                                                                                                                                    |
|--------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Égal              | À mesure que de nouveaux onglets sont ajoutés, tous les onglets sont réduits horizontalement jusqu’à ce qu’ils atteignent une très petite largeur minimale.                                                       |
| SizeToContent      | Les onglets ont toujours leur « taille naturelle », la taille minimale nécessaire pour afficher leur icône et leur en-tête. Ils ne sont pas développés ou réduits à mesure que des onglets sont ajoutés ou fermés. |

Quelle que soit la valeur que vous choisissez, il se peut qu’il y ait trop d’onglets à afficher dans votre bande d’onglets. Dans ce cas, des gâchettes de défilement sont affichées. Elles permettent à l’utilisateur de faire défiler le TabStrip vers la gauche et vers la droite.

### <a name="guidance-for-tab-selection"></a>Aide pour la sélection d’onglets

La plupart des utilisateurs sont habitués à utiliser des onglets de document, ne serait-ce que dans les navigateurs web. Quand ils utilisent des onglets de document dans votre application, leur expérience détermine leurs attentes quant à la manière dont vos onglets doivent se comporter.

Quelle que soit la façon dont l’utilisateur interagit avec un ensemble d’onglets de document, il doit toujours y avoir un onglet actif. Si l’utilisateur ferme l’onglet sélectionné ou le fait basculer dans une autre fenêtre, un autre onglet doit devenir l’onglet actif. [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) tente de le faire automatiquement en sélectionnant l’onglet suivant. Si vous avez une bonne raison pour que votre application autorise un TabView avec un onglet non sélectionné, la zone de contenu du TabView sera simplement vide.

## <a name="keyboard-navigation"></a>Navigation au clavier

Par défaut, [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) prend en charge de nombreux scénarios courants de navigation au clavier. Cette section décrit les fonctionnalités intégrées et fournit des recommandations sur les fonctionnalités supplémentaires qui peuvent être utiles pour certaines applications.

### <a name="tab-and-cursor-key-behavior"></a>Comportement des touches de tabulation et de curseur

Quand le focus se déplace dans la zone de _TabStrip_ , le [TabViewItem](/uwp/api/microsoft.ui.xaml.controls.tabviewitem) sélectionné obtient le focus. L’utilisateur peut ensuite utiliser les flèches gauche et droite pour déplacer le focus (et non la sélection) vers d’autres onglets dans le TabStrip. Le focus de la flèche est bloqué à l’intérieur de la bande d’onglet et du bouton d’ajout d’onglet (+), s’il est présent. Pour déplacer le focus hors de la zone de TabStrip, l’utilisateur peut appuyer sur la touche Tab afin de déplacer le focus sur l’élément pouvant être actif suivant.

Déplacer le focus à l’aide de la touche Tab

![Déplacer le focus à l’aide de la touche Tab](images/tabview/tab-keyboard-behavior-1.png)

Les touches de direction ne cyclent pas le focus

![Les touches de direction ne cyclent pas le focus](images/tabview/tab-keyboard-behavior-3.png)

### <a name="selecting-a-tab"></a>Sélection d’un onglet

Quand un TabViewItem a le focus, un appui sur la touche Espace ou Entrée sélectionne ce TabViewItem.

Utilisez les touches de direction pour déplacer le focus, puis appuyez sur Espace pour sélectionner l’onglet.

![Espace pour sélectionner l’onglet](images/tabview/tab-keyboard-behavior-2.png)

### <a name="shortcuts-for-selecting-adjacent-tabs"></a>Raccourcis pour la sélection d’onglets adjacents

Ctrl+Tab sélectionne le [TabViewItem](/uwp/api/microsoft.ui.xaml.controls.tabviewitem) suivant. Ctrl+Maj+Tab sélectionne le TabViewItem précédent. Dans ce cas, la liste d’onglets est « en boucle ». Autrement dit, si vous sélectionnez l’onglet suivant alors que le dernier onglet est sélectionné, le premier onglet devient alors sélectionné.

### <a name="closing-a-tab"></a>Fermeture d’un onglet

Un appui sur Ctrl+F4 déclenche l’événement [TabCloseRequested](/uwp/api/microsoft.ui.xaml.controls.tabview.tabcloserequested). Gérez l’événement et fermez l’onglet, le cas échéant.

### <a name="keyboard-guidance-for-app-developers"></a>Aide relative au clavier pour les développeurs d’applications

Certaines applications peuvent nécessiter un contrôle de clavier plus avancé. Implémentez les raccourcis suivants s’ils sont appropriés pour votre application.

> [!WARNING]
> Si vous ajoutez un [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview) à une application existante, vous avez peut-être déjà créé des raccourcis clavier qui correspondent aux combinaisons de touches des raccourcis clavier TabView recommandés. Dans ce cas, vous devrez décider si vous souhaitez conserver vos raccourcis existants ou offrir une expérience de navigation d’onglets intuitive à l’utilisateur.

- Ctrl+T doit ouvrir un nouvel onglet. En général, cet onglet contient un document prédéfini ou est créé vide avec un moyen simple de choisir son contenu. Si l’utilisateur doit choisir du contenu pour un nouvel onglet, donnez le focus d’entrée au contrôle de sélection du contenu.
- Ctrl+W doit fermer l’onglet sélectionné. Rappelez-vous que TabView sélectionne automatiquement l’onglet suivant.
- Ctrl+Maj+T doit ouvrir les onglets récemment fermés (ou, plus précisément, ouvrir de nouveaux onglets avec le même contenu que les onglets récemment fermés). Commencez par le dernier onglet fermé, puis revenez un cran en arrière à chaque fois que le raccourci est appelé. Notez que cela nécessite de tenir à jour une liste des onglets récemment fermés.
- Ctrl+1 doit sélectionner le premier onglet de la liste d’onglets. De même, Ctrl+2 doit sélectionner le deuxième onglet, Ctrl+3 doit sélectionner le troisième, et ainsi de suite jusqu’à Ctrl+8.
- Ctrl+9 doit sélectionner le dernier onglet de la liste d’onglets, quel que soit le nombre d’onglets figurant dans la liste.
- Si les onglets offrent plus que la simple commande Fermer (par exemple la duplication ou l’épinglage d’un onglet), utilisez un menu contextuel pour afficher toutes les actions disponibles qui peuvent être effectuées sur un onglet.

### <a name="implement-browser-style-keyboarding-behavior"></a>Implémenter le comportement du clavier de style navigateur

Cet exemple implémente une partie des recommandations ci-dessus sur un [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview). Plus précisément, il implémente Ctrl+T, Ctrl+W, Ctrl+1-8 et Ctrl+9.

```xaml
<muxc:TabView x:Name="TabRoot">
    <muxc:TabView.KeyboardAccelerators>
        <KeyboardAccelerator Key="T" Modifiers="Control" Invoked="NewTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="W" Modifiers="Control" Invoked="CloseSelectedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number1" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number2" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number3" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number4" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number5" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number6" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number7" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number8" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
        <KeyboardAccelerator Key="Number9" Modifiers="Control" Invoked="NavigateToNumberedTabKeyboardAccelerator_Invoked" />
    </muxc:TabView.KeyboardAccelerators>
    <!-- ... some tabs ... -->
</muxc:TabView>
```

```csharp
private void NewTabKeyboardAccelerator_Invoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    // Create new tab.
    var newTab = new muxc.TabViewItem();
    newTab.IconSource = new muxc.SymbolIconSource() { Symbol = Symbol.Document };
    newTab.Header = "New Document";

    // The Content of a TabViewItem is often a frame which hosts a page.
    Frame frame = new Frame();
    newTab.Content = frame;
    frame.Navigate(typeof(Page1));

    TabRoot.TabItems.Add(newTab);
}

private void CloseSelectedTabKeyboardAccelerator_Invoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    // Only remove the selected tab if it can be closed.
    if (((muxc.TabViewItem)TabRoot.SelectedItem).IsClosable)
    {
        TabRoot.TabItems.Remove(TabRoot.SelectedItem);
    }
}

private void NavigateToNumberedTabKeyboardAccelerator_Invoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    int tabToSelect = 0;

    switch (sender.Key)
    {
        case Windows.System.VirtualKey.Number1:
            tabToSelect = 0;
            break;
        case Windows.System.VirtualKey.Number2:
            tabToSelect = 1;
            break;
        case Windows.System.VirtualKey.Number3:
            tabToSelect = 2;
            break;
        case Windows.System.VirtualKey.Number4:
            tabToSelect = 3;
            break;
        case Windows.System.VirtualKey.Number5:
            tabToSelect = 4;
            break;
        case Windows.System.VirtualKey.Number6:
            tabToSelect = 5;
            break;
        case Windows.System.VirtualKey.Number7:
            tabToSelect = 6;
            break;
        case Windows.System.VirtualKey.Number8:
            tabToSelect = 7;
            break;
        case Windows.System.VirtualKey.Number9:
            // Select the last tab
            tabToSelect = TabRoot.TabItems.Count - 1;
            break;
    }

    // Only select the tab if it is in the list
    if (tabToSelect < TabRoot.TabItems.Count)
    {
        TabRoot.SelectedIndex = tabToSelect;
    }
}
```

## <a name="related-articles"></a>Articles connexes

- [MasterDetails](./master-details.md)
- [NavigationView](./navigationview.md)
- [Pivot](./pivot.md)
