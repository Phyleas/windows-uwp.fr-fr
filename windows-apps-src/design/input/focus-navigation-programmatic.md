---
Description: Apprenez à gérer par programmation la navigation dans le focus avec le clavier, le boîtier de sélection et les outils d’accessibilité dans une application Windows.
title: Navigation en mode focus programmé avec un clavier, une manette et des outils d’accessibilité
label: Programmatic focus navigation
keywords: clavier, contrôleur de jeu, contrôle à distance, navigation, stratégie de navigation, entrée, interaction utilisateur, accessibilité, convivialité
ms.date: 03/19/2018
ms.topic: article
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 887d8329cc95d735ba33ff8dafc5105874206eaf
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172543"
---
# <a name="programmatic-focus-navigation"></a>Navigation en mode focus programmé

![Clavier, à distance et à D-Pad](images/dpad-remote/dpad-remote-keyboard.png)

Pour déplacer le focus par programme dans votre application Windows, vous pouvez utiliser la méthode [FocusManager. TryMoveFocus](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) ou la méthode [FindNextElement](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) .

[TryMoveFocus](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) tente de modifier le focus de l’élément ayant le focus sur l’élément pouvant être actif suivant dans la direction spécifiée, tandis que [FindNextElement](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) récupère l’élément (en tant que [DependencyObject](/uwp/api/windows.ui.xaml.dependencyobject)) qui recevra le focus en fonction de la direction de navigation spécifiée (navigation directionnelle uniquement, ne peut pas être utilisé pour émuler la navigation par onglets).

> [!NOTE]
> Nous vous recommandons d’utiliser la méthode [FindNextElement](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) au lieu de [FindNextFocusableElement](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextFocusableElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) , car FindNextFocusableElement récupère un UIElement, qui retourne la valeur null si l’élément pouvant être actif suivant n’est pas un UIElement (tel qu’un objet Hyperlink). 

## <a name="find-a-focus-candidate-within-a-scope"></a>Trouver un candidat au focus dans une étendue

Vous pouvez personnaliser le comportement de navigation de focus pour [TryMoveFocus](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) et [FindNextElement](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_), y compris la recherche du candidat au focus suivant dans une arborescence d’interface utilisateur spécifique ou l’exclusion d’éléments spécifiques.

Cet exemple utilise un jeu morpion pour illustrer les méthodes [TryMoveFocus](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) et [FindNextElement](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) .

```xaml
<StackPanel Orientation="Horizontal"
                VerticalAlignment="Center"
                HorizontalAlignment="Center" >
    <Button Content="Start Game" />
    <Button Content="Undo Movement" />
    <Grid x:Name="TicTacToeGrid" KeyDown="OnKeyDown">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="50" />
            <ColumnDefinition Width="50" />
            <ColumnDefinition Width="50" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="50" />
            <RowDefinition Height="50" />
        </Grid.RowDefinitions>
        <myControls:TicTacToeCell 
            Grid.Column="0" Grid.Row="0" 
            x:Name="Cell00" />
        <myControls:TicTacToeCell 
            Grid.Column="1" Grid.Row="0" 
            x:Name="Cell10"/>
        <myControls:TicTacToeCell 
            Grid.Column="2" Grid.Row="0" 
            x:Name="Cell20"/>
        <myControls:TicTacToeCell 
            Grid.Column="0" Grid.Row="1" 
            x:Name="Cell01"/>
        <myControls:TicTacToeCell 
            Grid.Column="1" Grid.Row="1" 
            x:Name="Cell11"/>
        <myControls:TicTacToeCell 
            Grid.Column="2" Grid.Row="1" 
            x:Name="Cell21"/>
        <myControls:TicTacToeCell 
            Grid.Column="0" Grid.Row="2" 
            x:Name="Cell02"/>
        <myControls:TicTacToeCell 
            Grid.Column="1" Grid.Row="2" 
            x:Name="Cell22"/>
        <myControls:TicTacToeCell 
            Grid.Column="2" Grid.Row="2" 
            x:Name="Cell32"/>
    </Grid>
</StackPanel>
```

```csharp
private void OnKeyDown(object sender, KeyRoutedEventArgs e)
{
    DependencyObject candidate = null;

    var options = new FindNextElementOptions ()
    {
        SearchRoot = TicTacToeGrid,
        XYFocusNavigationStrategyOverride = XYFocusNavigationStrategyOverride.Projection
    };

    switch (e.Key)
    {
        case Windows.System.VirtualKey.Up:
            candidate = 
                FocusManager.FindNextElement(
                    FocusNavigationDirection.Up, options);
            break;
        case Windows.System.VirtualKey.Down:
            candidate = 
                FocusManager.FindNextElement(
                    FocusNavigationDirection.Down, options);
            break;
        case Windows.System.VirtualKey.Left:
            candidate = FocusManager.FindNextElement(
                FocusNavigationDirection.Left, options);
            break;
        case Windows.System.VirtualKey.Right:
            candidate = 
                FocusManager.FindNextElement(
                    FocusNavigationDirection.Right, options);
            break;
    }
    // Also consider whether candidate is a Hyperlink, WebView, or TextBlock.
    if (candidate != null && candidate is Control)
    {
        (candidate as Control).Focus(FocusState.Keyboard);
    }
}
```

Utilisez [FindNextElementOptions](/uwp/api/windows.ui.xaml.input.findnextelementoptions) pour personnaliser davantage la façon dont les candidats au focus sont identifiés. Cet objet fournit les propriétés suivantes :

- [SearchRoot](/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_SearchRoot) -étendue recherche des candidats de navigation de focus aux enfants de ce DependencyObject. Null indique de démarrer la recherche à partir de la racine de l’arborescence d’éléments visuels.

> [!Important] 
> Si une ou plusieurs transformations sont appliquées aux descendants de **SearchRoot** qui les placent en dehors de la zone directionnelle, ces éléments sont toujours considérés comme des candidats.

- [ExclusionRect](/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_ExclusionRect) : les candidats à la navigation du focus sont identifiés à l’aide d’un rectangle englobant « fictif » dans lequel tous les objets qui se chevauchent sont exclus du focus de navigation. Ce rectangle est utilisé uniquement pour les calculs et n’est jamais ajouté à l’arborescence d’éléments visuels.
- [HintRect](/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_HintRect) : les candidats à la navigation du focus sont identifiés à l’aide d’un rectangle englobant « fictif » qui identifie les éléments les plus susceptibles de recevoir le focus. Ce rectangle est utilisé uniquement pour les calculs et n’est jamais ajouté à l’arborescence d’éléments visuels.
- [XYFocusNavigationStrategyOverride](/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_XYFocusNavigationStrategyOverride) : stratégie de navigation de focus utilisée pour identifier le meilleur élément candidat pour recevoir le focus.

L’image suivante illustre certains de ces concepts. 

Lorsque l’élément B a le focus, FindNextElement identifie I comme candidat au focus lors de la navigation vers la droite. Les raisons sont les suivantes :
- En raison du [HintRect](/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_HintRect) sur un, la référence de départ est, et non B
- C n’est pas un candidat, car MyPanel a été spécifié en tant que [SearchRoot](/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_SearchRoot)
- F n’est pas candidat, car [ExclusionRect](/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_ExclusionRect) le chevauche

![Comportement de navigation de focus personnalisé à l’aide d’indicateurs de navigation](images/keyboard/navigation-hints.png)

*Comportement de navigation de focus personnalisé à l’aide d’indicateurs de navigation*

## <a name="navigation-focus-events"></a>Événements de focus de navigation

### <a name="nofocuscandidatefound-event"></a>Événement NoFocusCandidateFound

L’événement [UIElement. NoFocusCandidateFound](/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_NoFocusCandidateFound) est déclenché quand l’utilisateur appuie sur les touches de tabulation ou de direction et qu’il n’y a pas de candidat au focus dans la direction spécifiée. Cet événement n’est pas déclenché pour [TryMoveFocus](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_).

Étant donné qu’il s’agit d’un événement routé, il se propage de l’élément ayant le focus vers le haut par le biais d’objets parents successifs jusqu’à la racine de l’arborescence d’objets. Cela vous permet de gérer l’événement là où cela est nécessaire.

<a name="split-view-code-sample"></a>

Ici, nous montrons comment une grille ouvre un [SPLITVIEW](/uwp/api/windows.ui.xaml.controls.splitview) lorsque l’utilisateur tente de déplacer le focus vers la gauche du contrôle le plus actif à gauche (voir [conception pour Xbox et TV](../devices/designing-for-tv.md#navigation-pane)).

```xaml
<Grid NoFocusCandidateFound="OnNoFocusCandidateFound">
...
</Grid>
```

```csharp
private void OnNoFocusCandidateFound (
    UIElement sender, NoFocusCandidateFoundEventArgs args)
{
    if(args.NavigationDirection == FocusNavigationDirection.Left)
    {
        if(args.InputDevice == FocusInputDeviceKind.Keyboard ||
        args.InputDevice == FocusInputDeviceKind.GameController )
            {
                OpenSplitPaneView();
            }
        args.Handled = true;
    }
}
```

### <a name="gotfocus-and-lostfocus-events"></a>Événements GotFocus et LostFocus
Les événements [UIElement. GotFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) et [UIElement. LostFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus) sont déclenchés lorsqu’un élément est activé ou perd le focus, respectivement. Cet événement n’est pas déclenché pour [TryMoveFocus](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_).

Étant donné qu’il s’agit d’événements routés, ils se propagent de l’élément ayant le focus vers le haut par le biais d’objets parents successifs jusqu’à la racine de l’arborescence d’objets. Cela vous permet de gérer l’événement là où cela est nécessaire.

### <a name="gettingfocus-and-losingfocus-events"></a>Événements GettingFocus et LosingFocus

Les événements [UIElement. GettingFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) et [UIElement. LosingFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) se déclenchent avant les événements [UIElement. GotFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) et [UIElement. LostFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus) respectifs. 

Étant donné qu’il s’agit d’événements routés, ils se propagent de l’élément ayant le focus vers le haut par le biais d’objets parents successifs jusqu’à la racine de l’arborescence d’objets. Comme cela se produit avant qu’une modification de focus ait lieu, vous pouvez rediriger ou annuler le changement de focus.

[GettingFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) et [LosingFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) sont des événements synchrones. le focus ne sera donc pas déplacé pendant la propagation de ces événements. Toutefois, [GotFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) et [LostFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus) sont des événements asynchrones, ce qui signifie qu’il n’y a aucune garantie que le focus ne se déplacera pas avant l’exécution du gestionnaire.

Si le focus se déplace dans un appel à [Control. Focus](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_Focus_Windows_UI_Xaml_FocusState_), [GettingFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) est déclenché pendant l’appel, tandis que [GotFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) est déclenché après l’appel.

La cible de navigation de Focus peut être modifiée pendant les événements [GettingFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) et [LosingFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) (avant le déplacement du focus) via la propriété [GettingFocusEventArgs. NewFocusedElement](/uwp/api/windows.ui.xaml.input.gettingfocuseventargs#Windows_UI_Xaml_Input_GettingFocusEventArgs_NewFocusedElement) . Même si la cible est modifiée, l’événement se propage toujours et la cible peut être de nouveau modifiée.

Pour éviter les problèmes de réentrance, une exception est levée si vous essayez de déplacer le focus (à l’aide de [TryMoveFocus](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) ou de [Control. Focus](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_Focus_Windows_UI_Xaml_FocusState_)) pendant que ces événements sont propagés.

Ces événements sont déclenchés, quelle que soit la raison pour laquelle le focus se déplace (y compris la navigation par onglets, la navigation directionnelle et la navigation par programmation).

Voici l’ordre d’exécution des événements de Focus :

1.  [LosingFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) Si le focus est réinitialisé à l’élément perdant le focus ou [TryCancel](/uwp/api/windows.ui.xaml.input.losingfocuseventargs#Windows_UI_Xaml_Input_LosingFocusEventArgs_TryCancel) réussit, aucun autre événement n’est déclenché.
2.  [GettingFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) Si le focus est réinitialisé à l’élément perdant le focus ou [TryCancel](/uwp/api/windows.ui.xaml.input.gettingfocuseventargs#Windows_UI_Xaml_Input_GettingFocusEventArgs_TryCancel) réussit, aucun autre événement n’est déclenché.
3.  [LostFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus)
4.  [GotFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus)

L’illustration suivante montre comment, lors du déplacement vers la droite d’un, le XYFocus choisit B4 comme candidat. B4 déclenche ensuite l’événement GettingFocus où ListView a la possibilité de réassigner le focus à B3.

![Modification de la cible de navigation de focus sur l’événement GettingFocus](images/keyboard/focus-events.png)

*Modification de la cible de navigation de focus sur l’événement GettingFocus*

Ici, nous montrons comment gérer l’événement [GettingFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) et rediriger le focus.

```XAML
<StackPanel Orientation="Horizontal">
    <Button Content="A" />
    <ListView x:Name="MyListView" SelectedIndex="2" GettingFocus="OnGettingFocus">
        <ListViewItem>LV1</ListViewItem>
        <ListViewItem>LV2</ListViewItem>
        <ListViewItem>LV3</ListViewItem>
        <ListViewItem>LV4</ListViewItem>
        <ListViewItem>LV5</ListViewItem>
    </ListView>
</StackPanel>
```

```csharp
private void OnGettingFocus(UIElement sender, GettingFocusEventArgs args)
{
    //Redirect the focus only when the focus comes from outside of the ListView.
    // move the focus to the selected item
    if (MyListView.SelectedIndex != -1 && 
        IsNotAChildOf(MyListView, args.OldFocusedElement))
    {
        var selectedContainer = 
            MyListView.ContainerFromItem(MyListView.SelectedItem);
        if (args.FocusState == 
            FocusState.Keyboard && 
            args.NewFocusedElement != selectedContainer)
        {
            args.TryRedirect(
                MyListView.ContainerFromItem(MyListView.SelectedItem));
            args.Handled = true;
        }
    }
}
```

Ici, nous montrons comment gérer l’événement [LosingFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) pour un [CommandBar](/uwp/api/windows.ui.xaml.controls.commandbar) et définir le focus lorsque le menu est fermé.

```XAML
<CommandBar x:Name="MyCommandBar" LosingFocus="OnLosingFocus">
     <AppBarButton Icon="Back" Label="Back" />
     <AppBarButton Icon="Stop" Label="Stop" />
     <AppBarButton Icon="Play" Label="Play" />
     <AppBarButton Icon="Forward" Label="Forward" />

     <CommandBar.SecondaryCommands>
         <AppBarButton Icon="Like" Label="Like" />
         <AppBarButton Icon="Share" Label="Share" />
     </CommandBar.SecondaryCommands>
 </CommandBar>
```

```csharp
private void OnLosingFocus(UIElement sender, LosingFocusEventArgs args)
{
    if (MyCommandBar.IsOpen == true && 
        IsNotAChildOf(MyCommandBar, args.NewFocusedElement))
    {
        if (args.TryCancel())
        {
            args.Handled = true;
        }
    }
}
```

## <a name="find-the-first-and-last-focusable-element"></a>Rechercher le premier et le dernier élément pouvant être actif

Les méthodes [FocusManager. FindFirstFocusableElement](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindFirstFocusableElement_Windows_UI_Xaml_DependencyObject_) et [FocusManager. FindLastFocusableElement](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindLastFocusableElement_Windows_UI_Xaml_DependencyObject_) déplacent le focus sur le premier ou le dernier élément pouvant être actif dans l’étendue d’un objet (l’arborescence d’éléments d’un [UIElement](/uwp/api/windows.ui.xaml.uielement) ou l’arborescence de texte d' [un TextElement).](/uwp/api/windows.ui.xaml.documents.textelement) L’étendue est spécifiée dans l’appel (si l’argument est null, l’étendue est la fenêtre active).

Si aucun candidat au focus ne peut être identifié dans la portée, la valeur null est retournée.

Ici, nous expliquons comment spécifier que les boutons d’un CommandBar ont un comportement directionnel de renvoi à la ligne (voir [interactions au clavier](keyboard-interactions.md#popup-ui)).

```XAML
<CommandBar x:Name="MyCommandBar" LosingFocus="OnLosingFocus">
    <AppBarButton Icon="Back" Label="Back" />
    <AppBarButton Icon="Stop" Label="Stop" />
    <AppBarButton Icon="Play" Label="Play" />
    <AppBarButton Icon="Forward" Label="Forward" />

    <CommandBar.SecondaryCommands>
        <AppBarButton Icon="Like" Label="Like" />
        <AppBarButton Icon="ReShare" Label="Share" />
    </CommandBar.SecondaryCommands>
</CommandBar>
```

```csharp
private void OnLosingFocus(UIElement sender, LosingFocusEventArgs args)
{
    if (IsNotAChildOf(MyCommandBar, args.NewFocussedElement))
    {
        DependencyObject candidate = null;
        if (args.Direction == FocusNavigationDirection.Left)
        {
            candidate = FocusManager.FindLastFocusableElement(MyCommandBar);
        }
        else if (args.Direction == FocusNavigationDirection.Right)
        {
            candidate = FocusManager.FindFirstFocusableElement(MyCommandBar);
        }
        if (candidate != null)
        {
            args.NewFocusedElement = candidate;
            args.Handled = true;
        }
    }
}
```

## <a name="related-articles"></a>Articles connexes

- [Navigation centrée sur le clavier, le boîtier, le contrôle à distance et les outils d’accessibilité](focus-navigation.md)
- [Interactions avec le clavier](keyboard-interactions.md)
- [Accessibilité du clavier](../accessibility/keyboard-accessibility.md)