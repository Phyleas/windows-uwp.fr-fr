---
Description: Découvrez comment les touches d’accès rapide peuvent améliorer l’utilisation et l’accessibilité des applications Windows.
title: Raccourcis clavier
label: Keyboard accelerators
template: detail.hbs
keywords: clavier, accélérateur, touche accélérateur, raccourcis clavier, accessibilité, navigation, Focus, texte, entrée, interactions utilisateur, boîtier de commande, à distance
ms.date: 09/24/2020
ms.topic: article
pm-contact: chigy
design-contact: miguelrb
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 8ec5791c212e2fdfbafd40131d96ace6c88fb519
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/06/2020
ms.locfileid: "91749955"
---
# <a name="keyboard-accelerators"></a>Raccourcis clavier

![Clavier de surface](images/accelerators/accelerators_hero2.png)

Les touches accélérateur (ou accélérateurs clavier) sont des raccourcis clavier qui améliorent l’utilisation et l’accessibilité de vos applications Windows en fournissant un moyen intuitif aux utilisateurs d’appeler des actions ou des commandes courantes sans naviguer dans l’interface utilisateur de l’application.

Pour plus d’informations sur la navigation dans l’interface utilisateur d’une application Windows à l’aide de raccourcis clavier, consultez la rubrique [clés d’accès](access-keys.md) .

> [!NOTE]
> Un clavier est indispensable pour les utilisateurs présentant des handicaps (voir [accessibilité du clavier](../accessibility/keyboard-accessibility.md)). il est également un outil important pour les utilisateurs qui préfèrent le faire comme un moyen plus efficace d’interagir avec une application.

## <a name="overview"></a>Vue d’ensemble

Les accélérateurs incluent généralement les touches de fonction F1 à F12 ou une combinaison d’une touche standard associée à une ou plusieurs touches de modification (CTRL, Maj).

> [!NOTE]
> Les contrôles de plateforme UWP intègrent des accélérateurs de clavier. Par exemple, ListView prend en charge Ctrl + A pour sélectionner tous les éléments de la liste, et RichEditBox prend en charge Ctrl + Tab pour insérer un onglet dans la zone de texte. Ces accélérateurs de clavier intégrés sont appelés **accélérateurs de contrôle** et sont exécutés uniquement si le focus se trouve sur l’élément ou sur l’un de ses enfants. Les accélérateurs définis par vous à l’aide des API de l’accélérateur clavier décrits ici sont appelés **accélérateurs d’application**.

Les accélérateurs de clavier ne sont pas disponibles pour chaque action, mais sont souvent associés à des commandes exposées dans des menus (et doivent être spécifiés avec le contenu de l’élément de menu).Les accélérateurs peuvent également être associés à des actions qui n’ont pas d’éléments de menu équivalents. Toutefois, étant donné que les utilisateurs s’appuient sur les menus d’une application pour découvrir et apprendre le jeu de commandes disponible, vous devez essayer de rendre la détection des accélérateurs aussi simple que possible (l’utilisation d’étiquettes ou de modèles établis peut vous aider).

![Raccourcis clavier décrits dans une étiquette d’élément de menu](images/accelerators/accelerators_menuitemlabel.png)  
*Raccourcis clavier décrits dans une étiquette d’élément de menu*

## <a name="when-to-use-keyboard-accelerators"></a>Quand utiliser les accélérateurs de clavier

Nous vous recommandons de spécifier des accélérateurs de clavier à chaque fois qu’ils sont appropriés dans votre interface utilisateur, et de prendre en charge les accélérateurs dans tous les contrôles personnalisés.

- Les accélérateurs de clavier rendent votre application plus accessible aux utilisateurs souffrant de handicaps moteurs, y compris ceux qui ne peuvent appuyer que sur une clé à la fois ou qui ont des difficultés à utiliser une souris. * *

  Une interface utilisateur de clavier bien conçue représente un aspect important de l’accessibilité logicielle. Elle permet aux utilisateurs malvoyants ou souffrant d’un handicap moteur de naviguer dans une application et d’interagir avec ses fonctionnalités. Les utilisateurs qui ne sont pas en mesure d’utiliser une souris peuvent avoir recours à diverses technologies d’assistance, telles que les outils de clavier amélioré, les claviers visuels, les écrans élargis, les lecteurs d’écran et les utilitaires d’entrée vocale. Pour ces utilisateurs, une couverture complète des commandes est cruciale.

- Les accélérateurs de clavier rendent votre application plus utilisable pour les utilisateurs avec pouvoir qui préfèrent interagir via le clavier.

  Les utilisateurs expérimentés disposent souvent d’une préférence pour utiliser le clavier, car les commandes basées sur le clavier peuvent être entrées plus rapidement et ne nécessitent pas la suppression des mains du clavier. Pour ces utilisateurs, l’efficacité et la cohérence sont essentielles. L’exhaustivité n’est importante que pour les commandes les plus fréquemment utilisées.

## <a name="specify-a-keyboard-accelerator"></a>Spécifier une touche d’accès rapide

Utilisez les API [KeyboardAccelerator](/uwp/api/windows.ui.xaml.input.keyboardaccelerator.-ctor) pour créer des accélérateurs de clavier dans des applications UWP. Avec ces API, vous n’êtes pas obligé de gérer plusieurs événements KeyOut pour détecter la combinaison de touches et vous pouvez localiser des accélérateurs dans les ressources de l’application.

Nous vous recommandons de définir des accélérateurs de clavier pour les actions les plus courantes dans votre application et de les documenter à l’aide de l’étiquette ou de l’info-bulle de l’élément de menu. Dans cet exemple, nous déclarons les accélérateurs de clavier uniquement pour les commandes de renommage et de copie.

``` xaml
<CommandBar Margin="0,200" AccessKey="M">
  <AppBarButton 
    Icon="Share" 
    Label="Share" 
    Click="OnShare" 
    AccessKey="S" />
  <AppBarButton 
    Icon="Copy" 
    Label="Copy" 
    ToolTipService.ToolTip="Copy (Ctrl+C)" 
    Click="OnCopy" 
    AccessKey="C">
    <AppBarButton.KeyboardAccelerators>
      <KeyboardAccelerator 
        Modifiers="Control" 
        Key="C" />
    </AppBarButton.KeyboardAccelerators>
  </AppBarButton>

  <AppBarButton 
    Icon="Delete" 
    Label="Delete" 
    Click="OnDelete" 
    AccessKey="D" />
  <AppBarSeparator/>
  <AppBarButton 
    Icon="Rename" 
    Label="Rename" 
    ToolTipService.ToolTip="Rename (F2)" 
    Click="OnRename" 
    AccessKey="R">
    <AppBarButton.KeyboardAccelerators>
      <KeyboardAccelerator 
        Modifiers="None" Key="F2" />
    </AppBarButton.KeyboardAccelerators>
  </AppBarButton>

  <AppBarButton 
    Icon="SelectAll" 
    Label="Select" 
    Click="OnSelect" 
    AccessKey="A" />
  
  <CommandBar.SecondaryCommands>
    <AppBarButton 
      Icon="OpenWith" 
      Label="Sources" 
      AccessKey="S">
      <AppBarButton.Flyout>
        <MenuFlyout>
          <ToggleMenuFlyoutItem Text="OneDrive" />
          <ToggleMenuFlyoutItem Text="Contacts" />
          <ToggleMenuFlyoutItem Text="Photos"/>
          <ToggleMenuFlyoutItem Text="Videos"/>
        </MenuFlyout>
      </AppBarButton.Flyout>
    </AppBarButton>
    <AppBarToggleButton 
      Icon="Save" 
      Label="Auto Save" 
      IsChecked="True" 
      AccessKey="A"/>
  </CommandBar.SecondaryCommands>

</CommandBar>
```

![Accélérateur clavier décrit dans une info-bulle](images/accelerators/accelerators_tooltip.png)  
***Accélérateur clavier décrit dans une info-bulle***

L’objet [UIElement](/uwp/api/windows.ui.xaml.uielement) a une collection [KeyboardAccelerator](/uwp/api/windows.ui.xaml.input.keyboardaccelerator) , [KeyboardAccelerators](/uwp/api/windows.ui.xaml.uielement.KeyboardAccelerators), où vous spécifiez vos objets KeyboardAccelerator personnalisés et définissez les séquences de touches pour l’accélérateur clavier :

-   **[Key](/uwp/api/windows.ui.xaml.input.keyboardaccelerator.Key)** : [VirtualKey](/uwp/api/windows.system.virtualkey) utilisé pour l’accélérateur clavier.

-   **[Modificateurs](/uwp/api/windows.ui.xaml.input.keyboardaccelerator.Modifiers)** : [VirtualKeyModifiers](/uwp/api/windows.system.virtualkeymodifiers) utilisé pour l’accélérateur clavier. Si le modificateur n’est pas défini, la valeur par défaut est None.

> [!NOTE]
> Les accélérateurs et les accélérateurs à plusieurs clés (Ctrl + Maj + M) sont pris en charge dans une seule clé (A, Delete, F2, espace, ESC, clé multimédia). Toutefois, les clés virtuelles de manette ne sont pas prises en charge.

## <a name="scoped-accelerators"></a>Accélérateurs délimités

Certains accélérateurs fonctionnent uniquement dans des portées spécifiques, tandis que d’autres travaillent à l’échelle de l’application.

Par exemple, Microsoft Outlook comprend les accélérateurs suivants :
-   CTRL + B, Ctrl + I et ESC ne fonctionnent que dans l’étendue du formulaire envoyer un e-mail
-   CTRL + 1 et CTRL + 2 travail à l’ensemble de l’application

### <a name="context-menus"></a>Menu contextuels

Les actions du menu contextuel affectent uniquement des zones ou des éléments spécifiques, tels que les caractères sélectionnés dans un éditeur de texte ou une chanson dans une sélection. Pour cette raison, nous vous recommandons de définir l’étendue des accélérateurs clavier pour les éléments de menu contextuel sur le parent du menu contextuel.

Utilisez la propriété [ScopeOwner](/uwp/api/windows.ui.xaml.input.keyboardaccelerator.ScopeOwner) pour spécifier l’étendue de l’accélérateur clavier. Ce code montre comment implémenter un menu contextuel sur un ListView avec des accélérateurs de clavier délimités :

``` xaml
<ListView x:Name="MyList">
  <ListView.ContextFlyout>
    <MenuFlyout>
      <MenuFlyoutItem Text="Share" Icon="Share"/>
      <MenuFlyoutItem Text="Copy" Icon="Copy">
        <MenuFlyoutItem.KeyboardAccelerators>
          <KeyboardAccelerator 
            Modifiers="Control" 
            Key="C" 
            ScopeOwner="{x:Bind MyList }" />
        </MenuFlyoutItem.KeyboardAccelerators>
      </MenuFlyoutItem>
      
      <MenuFlyoutItem Text="Delete" Icon="Delete" />
      <MenuFlyoutSeparator />
      
      <MenuFlyoutItem Text="Rename">
        <MenuFlyoutItem.KeyboardAccelerators>
          <KeyboardAccelerator 
            Modifiers="None" 
            Key="F2" 
            ScopeOwner="{x:Bind MyList}" />
        </MenuFlyoutItem.KeyboardAccelerators>
      </MenuFlyoutItem>
      
      <MenuFlyoutItem Text="Select" />
    </MenuFlyout>
    
  </ListView.ContextFlyout>
    
  <ListViewItem>Track 1</ListViewItem>
  <ListViewItem>Alternative Track 1</ListViewItem>

</ListView>
```

L’attribut ScopeOwner de l’élément MenuFlyoutItem. KeyboardAccelerators marque l’accélérateur comme étendu au lieu de global (la valeur par défaut est null ou global). Pour plus d’informations, consultez la section **résolution des accélérateurs** plus loin dans cette rubrique.

## <a name="invoke-a-keyboard-accelerator"></a>Appeler une touche d’accès rapide 

L’objet [KeyboardAccelerator](/uwp/api/windows.ui.xaml.input.keyboardaccelerator) utilise le [modèle de contrôle UI Automation (UIA)](/windows/desktop/WinAuto/uiauto-controlpatternsoverview) pour agir lorsqu’un accélérateur est appelé.

UIA [modèles de contrôle] exposent les fonctionnalités de contrôle courantes. Par exemple, le contrôle Button implémente le modèle de contrôle [Invoke](/windows/desktop/WinAuto/uiauto-implementinginvoke) pour prendre en charge l’événement Click (en général, un contrôle est appelé en cliquant, en double-cliquant, ou en appuyant sur entrée, sur un raccourci clavier prédéfini ou sur une autre combinaison de touches). Quand un accélérateur clavier est utilisé pour appeler un contrôle, l’infrastructure XAML recherche si le contrôle implémente le modèle de contrôle Invoke et, si tel est le cas, l’active (il n’est pas nécessaire d’écouter l’événement KeyboardAcceleratorInvoked).

Dans l’exemple suivant, CTRL + S déclenche l’événement Click, car le bouton implémente le modèle Invoke.

``` xaml 
<Button Content="Save" Click="OnSave">
  <Button.KeyboardAccelerators>
    <KeyboardAccelerator Key="S" Modifiers="Control" />
  </Button.KeyboardAccelerators>
</Button>
```

Si un élément implémente plusieurs modèles de contrôle, un seul peut être activé par le biais d’un accélérateur. Les modèles de contrôle sont classés par ordre de priorité comme suit :
1.  Invoke (bouton)
2.  Toggle (case à cocher)
3.  Sélection (ListView)
4.  Développer/réduire (ComboBox) 

Si aucune correspondance n’est identifiée, l’accélérateur n’est pas valide et un message de débogage est fourni («aucun modèle d’automatisation pour ce composant n’a été trouvé. Implémentez tout le comportement souhaité dans l’événement appelé. La définition de la propriété Handled sur true dans votre gestionnaire d’événements supprime ce message.»)

## <a name="custom-keyboard-accelerator-behavior"></a>Comportement de l’accélérateur de clavier personnalisé

L’événement appelé de l’objet [KeyboardAccelerator](/uwp/api/windows.ui.xaml.input.keyboardaccelerator) est déclenché lors de l’exécution de l’accélérateur. L’objet d’événement [KeyboardAcceleratorInvokedEventArgs](/uwp/api/windows.ui.xaml.input.keyboardacceleratorinvokedeventargs) comprend les propriétés suivantes :

- [**Géré**](/uwp/api/windows.ui.xaml.input.keyboardacceleratorinvokedeventargs.handled) (booléen) : l’affectation de la valeur true empêche l’événement qui déclenche le modèle de contrôle et arrête la propagation des événements d’accélérateur. La valeur par défaut est false.
- [**Element**](/uwp/api/windows.ui.xaml.input.keyboardacceleratorinvokedeventargs.element) (DependencyObject) : objet associé à l’accélérateur.
- [**KeyboardAccelerator**](/uwp/api/windows.ui.xaml.input.keyboardacceleratorinvokedeventargs.keyboardaccelerator): raccourci clavier utilisé pour déclencher l’événement appelé.

Ici, nous montrons comment définir une collection d’accélérateurs de clavier pour les éléments d’un ListView et comment gérer l’événement appelé pour chaque accélérateur.

``` xaml
<ListView x:Name="MyListView">
  <ListView.KeyboardAccelerators>
    <KeyboardAccelerator Key="A" Modifiers="Control,Shift" Invoked="SelectAllInvoked" />
    <KeyboardAccelerator Key="F5" Invoked="RefreshInvoked"  />
  </ListView.KeyboardAccelerators>
</ListView>
```

``` csharp
void SelectAllInvoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
  MyListView.SelectAll();
  args.Handled = true;
}

void RefreshInvoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
  MyListView.SelectionMode = ListViewSelectionMode.None;
  MyListView.SelectionMode = ListViewSelectionMode.Multiple;
  args.Handled = true;
}
```

## <a name="override-default-keyboard-behavior"></a>Remplacer le comportement du clavier par défaut

Certains contrôles, lorsqu’ils ont le focus, prennent en charge les accélérateurs de clavier intégrés qui remplacent tout accélérateur défini par l’application. Par exemple, lorsqu’une [zone](/uwp/api/windows.ui.xaml.controls.textbox) de texte a le focus, l’accélérateur Ctrl + C copie uniquement le texte actuellement sélectionné (les accélérateurs définis par l’application sont ignorés et aucune autre fonctionnalité n’est exécutée).

Bien que nous ne recommandons pas de substituer les comportements de contrôle par défaut en raison de la familiarité et des attentes des utilisateurs, vous pouvez remplacer l’accélérateur clavier intégré d’un contrôle. L’exemple suivant montre comment substituer l’accélérateur clavier Ctrl + C pour une [zone de texte](/uwp/api/windows.ui.xaml.controls.textbox) via le gestionnaire d’événements [PreviewKeyDown](/uwp/api/windows.ui.xaml.uielement.previewkeydown) : 

``` csharp
 private void TextBlock_PreviewKeyDown(object sender, KeyRoutedEventArgs e)
 {
    var ctrlState = CoreWindow.GetForCurrentThread().GetKeyState(Windows.System.VirtualKey.Control);
    var isCtrlDown = ctrlState == CoreVirtualKeyStates.Down || ctrlState 
        ==  (CoreVirtualKeyStates.Down | CoreVirtualKeyStates.Locked);
    if (isCtrlDown && e.Key == Windows.System.VirtualKey.C)
    {
        // Your custom keyboard accelerator behavior.
        
        e.Handled = true;
    }
 }
```  

## <a name="disable-a-keyboard-accelerator"></a>Désactiver une touche d’accès rapide 

Si un contrôle est désactivé, l’accélérateur associé est également désactivé. Dans l’exemple suivant, étant donné que la propriété IsEnabled de ListView est définie sur false, le contrôle associé + un accélérateur ne peut pas être appelé.

``` xaml
<ListView >
  <ListView.KeyboardAccelerators>
    <KeyboardAccelerator Key="A"
      Modifiers="Control"
      Invoked="CustomListViewSelecAllInvoked" />
  </ListView.KeyboardAccelerators>
  
  <TextBox>
    <TextBox.KeyboardAccelerators>
      <KeyboardAccelerator 
        Key="A" 
        Modifiers="Control" 
        Invoked="CustomTextSelecAllInvoked" 
        IsEnabled="False" />
    </TextBox.KeyboardAccelerators>
  </TextBox>

<ListView>
```

Les contrôles parents et enfants peuvent partager le même accélérateur. Dans ce cas, le contrôle parent peut être appelé même si l’enfant a le focus et que son accélérateur est désactivé.

## <a name="screen-readers-and-keyboard-accelerators"></a>Lecteurs d’écran et accélérateurs de clavier 

Les lecteurs d’écran tels que le narrateur peuvent annoncer la combinaison de touches d’accélérateur clavier aux utilisateurs. Par défaut, il s’agit de chaque modificateur (dans l’ordre d’énumération VirtualModifiers) suivi de la clé (et séparées par des signes « + »). Vous pouvez personnaliser ce à l’aide de la propriété jointe [AcceleratorKey](/uwp/api/windows.ui.xaml.automation.automationproperties.AcceleratorKeyProperty) AutomationProperties. Si plusieurs accélérateurs sont spécifiés, seul le premier est annoncé.

Dans cet exemple, AutomationProperty. AcceleratorKey retourne la chaîne « Control + Shift + A » :

``` xaml
<ListView x:Name="MyListView">
  <ListView.KeyboardAccelerators>

    <KeyboardAccelerator 
      Key="A" 
      Modifiers="Control,Shift" 
      Invoked="CustomSelectAllInvoked" />
      
    <KeyboardAccelerator 
      Key="F5" 
      Modifiers="None" 
      Invoked="RefreshInvoked" />

  </ListView.KeyboardAccelerators>

</ListView>   
```

> [!NOTE] 
> La définition de AutomationProperties. AcceleratorKey n’active pas les fonctionnalités du clavier, mais indique uniquement à l’infrastructure UIA les clés utilisées.

## <a name="common-keyboard-accelerators"></a>Accélérateurs clavier courants

Nous vous recommandons de rendre les accélérateurs de clavier cohérents entre les applications Windows. Les utilisateurs doivent mémoriser les accélérateurs de clavier et s’attendre à des résultats identiques (ou similaires).

Cela peut ne pas être toujours possible en raison de différences de fonctionnalités entre les applications.

| **Modification** | **Accélérateur clavier courant** |
| ------------- | ----------------------------------- |
| Démarrer le mode d’édition | Ctrl+E |
| Sélectionner tous les éléments d’un contrôle ou d’une fenêtre ayant le focus | Ctrl + A |
| Rechercher et remplacer | Ctrl + H |
| Annuler | Ctrl + Z |
| Rétablir | CTRL + Y |
| Supprimer la sélection et la copier dans le presse-papiers | Ctrl + X |
| Copier la sélection dans le presse-papiers | Ctrl + C, Ctrl + Inser |
| Coller le contenu du presse-papiers | Ctrl + V, Maj + Inser |
| Coller le contenu du presse-papiers (avec les options) | Ctrl + Alt + V |
| Renommer un élément | F2 |
| Ajouter un nouvel élément | Ctrl+N |
| Ajouter un nouvel élément secondaire | Ctrl + Maj + N |
| Supprimer l’élément sélectionné (avec annuler) | Suppr, Ctrl + D |
| Supprimer l’élément sélectionné (sans annuler) | Maj + Suppr |
| Gras | Ctrl + B |
| Souligner | Ctrl + U |
| Italique | Ctrl + I |
| **Navigation** | |
| Rechercher du contenu dans un contrôle ou une fenêtre ayant le focus | Ctrl+F |
| Atteindre le résultat de la recherche suivant | F3 |
| **Autres actions** | |
| Ajouter des favoris | Ctrl + D | 
| Actualiser | F5 ou Ctrl + R | 
| Zoom avant | Ctrl + + | 
| Faire un zoom arrière | Ctrl +- | 
| Zoomer sur la vue par défaut | Ctrl + 0 | 
| Enregistrer | Ctrl+S | 
| Fermer | Ctrl+W | 
| Imprimer | Ctrl+P | 

Notez que certaines combinaisons ne sont pas valides pour les versions localisées de Windows. Par exemple, dans la version espagnole de Windows, CTRL + N est utilisé pour le gras au lieu de CTRL + B. Nous vous recommandons de fournir des accélérateurs de clavier localisés si l’application est localisée.

## <a name="usability-affordances-for-keyboard-accelerators"></a>Intuitivité de convivialité pour les accélérateurs de clavier

### <a name="tooltips"></a>Info-bulles

Comme les accélérateurs de clavier ne sont généralement pas décrits directement dans l’interface utilisateur de votre application Windows, vous pouvez améliorer la détectabilité par le biais des [info-bulles](../controls-and-patterns/tooltips.md), qui s’affichent automatiquement lorsque l’utilisateur déplace le focus vers, appuie dessus ou pointe le pointeur de la souris sur un contrôle. L’info-bulle peut déterminer si un contrôle est associé à un accélérateur clavier et, le cas échéant, la combinaison de touches accélérateur.

**Windows 10, version 1803 (mise à jour d’avril 2018) et versions ultérieures**

Par défaut, lorsque des accélérateurs de clavier sont déclarés, tous les contrôles (à l’exception de [MenuFlyoutItem](/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem) et [ToggleMenuFlyoutItem](/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem)) présentent les combinaisons de touches correspondantes dans une info-bulle.

> [!NOTE] 
> Si plusieurs accélérateurs sont définis pour un contrôle, seul le premier est présenté.

![Info-bulle de la touche accélérateur](images/accelerators/accelerators_tooltip_savebutton_small.png)

*Combinaison de touches d’accès rapide dans l’info-bulle*

Pour les objets [Button](/uwp/api/windows.ui.xaml.controls.button), [AppBarButton](/uwp/api/windows.ui.xaml.controls.appbarbutton)et [AppBarToggleButton](/uwp/api/windows.ui.xaml.controls.appbartogglebutton) , l’accélérateur clavier est ajouté à l’info-bulle par défaut du contrôle. Pour les objets [MenuFlyoutItem](/uwp/api/windows.ui.xaml.controls.appbarbutton) et [ToggleMenuFlyoutItem](/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem) , l’accélérateur clavier est affiché avec le texte du menu volant.

> [!NOTE]
> La spécification d’une info-bulle (voir Button1 dans l’exemple suivant) remplace ce comportement.

```xaml
<StackPanel x:Name="Container" Grid.Row="0" Background="AliceBlue">
    <Button Content="Button1" Margin="20"
            Click="OnSave" 
            KeyboardAcceleratorPlacementMode="Auto" 
            ToolTipService.ToolTip="Tooltip">
        <Button.KeyboardAccelerators>
            <KeyboardAccelerator  Key="A" Modifiers="Windows"/>
        </Button.KeyboardAccelerators>
    </Button>
    <Button Content="Button2"  Margin="20"
            Click="OnSave" 
            KeyboardAcceleratorPlacementMode="Auto">
        <Button.KeyboardAccelerators>
            <KeyboardAccelerator  Key="B" Modifiers="Windows"/>
        </Button.KeyboardAccelerators>
    </Button>
    <Button Content="Button3"  Margin="20"
            Click="OnSave" 
            KeyboardAcceleratorPlacementMode="Auto">
        <Button.KeyboardAccelerators>
            <KeyboardAccelerator  Key="C" Modifiers="Windows"/>
        </Button.KeyboardAccelerators>
    </Button>
</StackPanel>
```

![Info-bulle de la touche accélérateur](images/accelerators/accelerators-button-small.png)

*Combinaison de touches d’accélérateur ajoutée à l’info-bulle par défaut du bouton*

```xaml
<AppBarButton Icon="Save" Label="Save">
    <AppBarButton.KeyboardAccelerators>
        <KeyboardAccelerator Key="S" Modifiers="Control"/>
    </AppBarButton.KeyboardAccelerators>
</AppBarButton>
```

![Info-bulle de la touche accélérateur](images/accelerators/accelerators-appbarbutton-small.png)

*Combinaison de touches d’accès rapide ajoutée à l’info-bulle par défaut de AppBarButton*

```xaml
<AppBarButton AccessKey="R" Icon="Refresh" Label="Refresh" IsAccessKeyScope="True">
    <AppBarButton.Flyout>
        <MenuFlyout>
            <MenuFlyoutItem AccessKey="A" Icon="Refresh" Text="Refresh A">
                <MenuFlyoutItem.KeyboardAccelerators>
                    <KeyboardAccelerator Key="R" Modifiers="Control"/>
                </MenuFlyoutItem.KeyboardAccelerators>
            </MenuFlyoutItem>
            <MenuFlyoutItem AccessKey="B" Icon="Globe" Text="Refresh B" />
            <MenuFlyoutItem AccessKey="C" Icon="Globe" Text="Refresh C" />
            <MenuFlyoutItem AccessKey="D" Icon="Globe" Text="Refresh D" />
            <ToggleMenuFlyoutItem AccessKey="E" Icon="Globe" Text="ToggleMe">
                <MenuFlyoutItem.KeyboardAccelerators>
                    <KeyboardAccelerator Key="Q" Modifiers="Control"/>
                </MenuFlyoutItem.KeyboardAccelerators>
            </ToggleMenuFlyoutItem>
        </MenuFlyout>
    </AppBarButton.Flyout>
</AppBarButton>
```

![Info-bulle de la touche accélérateur](images/accelerators/accelerators-appbar-menuflyoutitem-small.png)

*Combinaison de touches d’accès rapide ajoutée au texte de MenuFlyoutItem*

Contrôlez le comportement de présentation à l’aide de la propriété [KeyboardAcceleratorPlacementMode](/uwp/api/windows.ui.xaml.uielement.KeyboardAcceleratorPlacementMode) , qui accepte deux valeurs : [auto](/uwp/api/windows.ui.xaml.input.keyboardacceleratorplacementmode) ou [Hidden](/uwp/api/windows.ui.xaml.input.keyboardacceleratorplacementmode).    

```xaml
<Button Content="Save" Click="OnSave" KeyboardAcceleratorPlacementMode="Auto">
    <Button.KeyboardAccelerators>
        <KeyboardAccelerator Key="S" Modifiers="Control" />
    </Button.KeyboardAccelerators>
</Button>
```

Dans certains cas, vous devrez peut-être présenter une info-bulle relative à un autre élément (généralement un objet conteneur). Par exemple, un contrôle de tableau croisé dynamique qui affiche l’info-bulle pour un PivotItem avec l’en-tête de tableau croisé dynamique. 

Ici, nous expliquons comment utiliser la propriété KeyboardAcceleratorPlacementTarget pour afficher la combinaison de touches d’accès rapide du bouton enregistrer avec le conteneur de grille au lieu du bouton.

```xaml
<Grid x:Name="Container" Padding="30">
  <Button Content="Save"
    Click="OnSave"
    KeyboardAcceleratorPlacementMode="Auto"
    KeyboardAcceleratorPlacementTarget="{x:Bind Container}">
    <Button.KeyboardAccelerators>
      <KeyboardAccelerator  Key="S" Modifiers="Control" />
    </Button.KeyboardAccelerators>
  </Button>
</Grid>
```

### <a name="labels"></a>Étiquettes

Dans certains cas, nous vous recommandons d’utiliser l’étiquette d’un contrôle pour déterminer si le contrôle est associé à un accélérateur clavier et, le cas échéant, la combinaison de touches accélérateur. 

Certains contrôles de plateforme effectuent cette opération par défaut, en particulier les objets [MenuFlyoutItem](/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem) et [ToggleMenuFlyoutItem](/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem) , tandis que le [AppBarButton](/uwp/api/windows.ui.xaml.controls.appbarbutton) et le [AppBarToggleButton](/uwp/api/windows.ui.xaml.controls.appbartogglebutton) le font lorsqu’ils apparaissent dans le menu de dépassement de capacité de la [barre de commandes](/uwp/api/windows.ui.xaml.controls.commandbar).

![Raccourcis clavier décrits dans une étiquette d’élément de menu](images/accelerators/accelerators_menuitemlabel.png)  
*Raccourcis clavier décrits dans une étiquette d’élément de menu*

Vous pouvez remplacer le texte d’accélérateur par défaut de l’étiquette par le biais de la propriété [KeyboardAcceleratorTextOverride](/uwp/api/windows.ui.xaml.controls.appbarbutton.KeyboardAcceleratorTextOverride) des contrôles [MenuFlyoutItem](/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem), [ToggleMenuFlyoutItem](/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem), [AppBarButton](/uwp/api/windows.ui.xaml.controls.appbarbutton)et [AppBarToggleButton](/uwp/api/windows.ui.xaml.controls.appbartogglebutton) (n’utilisez qu’un seul espace pour aucun texte). 

> [!NOTE] 
> Le texte de remplacement n’est pas présenté si le système ne parvient pas à détecter un clavier attaché (vous pouvez le vérifier vous-même à l’aide de la propriété [KeyboardPresent](/uwp/api/windows.devices.input.keyboardcapabilities.KeyboardPresent) ).

## <a name="advanced-concepts"></a>Concepts avancés

Ici, nous allons examiner certains aspects de bas niveau des accélérateurs clavier.

### <a name="when-an-accelerator-is-invoked"></a>Quand un accélérateur est appelé

Les accélérateurs sont composés de deux types de clés : les modificateurs et les non-modificateurs. Les touches de modification incluent Maj, menu, contrôle et la touche Windows, qui sont exposées par le biais de [VirtualKeyModifiers](/uwp/api/Windows.System.VirtualKeyModifiers). Les non-modificateurs sont des clés virtuelles, telles que Delete, F3, la barre d’espace, ÉCHAP et toutes les touches de ponctuation et alphanumériques. Une touche d’accès rapide est appelée quand l’utilisateur appuie sur une touche non modificatrice tout en maintenant enfoncée une ou plusieurs touches de modification. Par exemple, si l’utilisateur appuie sur Ctrl + Maj + M, quand l’utilisateur appuie sur M, le Framework vérifie les modificateurs (Ctrl et Maj) et déclenche l’accélérateur, s’il existe.

> [!NOTE]
> Par défaut, l’accélérateur se répète (par exemple, lorsque l’utilisateur appuie sur Ctrl + Maj, puis maintient la touche M enfoncée, l’accélérateur est appelé à plusieurs reprises jusqu’à ce que M soit relâché). Ce comportement ne peut pas être modifié.

### <a name="input-event-priority"></a>Priorité d’événement d’entrée
Les événements d’entrée se produisent dans une séquence spécifique que vous pouvez intercepter et gérer en fonction des spécifications de votre application. 

#### <a name="the-keydownkeyup-bubbling-event"></a>Événement de propagation keyverse/KeyUp 

En XAML, une séquence de touches est traitée comme s’il s’agissait d’un seul pipeline de propagation d’entrée. Ce pipeline d’entrée est utilisé par les événements KEYpoint/KeyUp et l’entrée de caractère. Par exemple, si un élément a le focus et que l’utilisateur appuie sur une touche enfoncée, un événement keyverse est déclenché sur l’élément, suivi du parent de l’élément, et ainsi de suite jusqu’à l’arborescence, jusqu’aux arguments. La propriété gérée a la valeur true.

L’événement KeyOut est également utilisé par certains contrôles pour implémenter les accélérateurs de contrôle intégrés. Lorsqu’un contrôle a une touche d’accès rapide, il gère l’événement KeyOut, ce qui signifie qu’il n’y aura pas de propagation d’événements KeyOut. Par exemple, le RichEditBox prend en charge la copie avec Ctrl + C. Quand la touche CTRL est enfoncée, l’événement KeyOut est déclenché et se propage, mais lorsque l’utilisateur appuie sur C en même temps, l’événement keyverse est marqué comme géré et n’est pas déclenché (sauf si le paramètre handledEventsToo de [UIElement. AddHandler](/uwp/api/windows.ui.xaml.uielement.addhandler) a la valeur true).

#### <a name="the-characterreceived-event"></a>Événement CharacterReceived

Lorsque l’événement [CharacterReceived](/uwp/api/windows.ui.core.corewindow.CharacterReceived) est déclenché après l’événement KeyOut [pour les contrôles](/uwp/api/windows.ui.core.corewindow.KeyDown) de texte tels que TextBox, vous pouvez annuler l’entrée de caractères dans le gestionnaire d’événements KeyOut.

#### <a name="the-previewkeydown-and-previewkeyup-events"></a>Événements PreviewKeyDown et PreviewKeyUp

Les événements d’entrée en préversion sont déclenchés avant tout autre événement. Si vous ne gérez pas ces événements, l’accélérateur de l’élément qui a le focus est activé, suivi de l’événement KeyOut. Les deux événements sont propagés jusqu’à ce qu’ils soient gérés.


![](images/accelerators/accelerators_keyevents.png)
***Séquence d’événements*** clés de séquence d’événements clés

Ordre des événements :

Afficher un aperçu des événements KeyOut...
App Accelerator OnKeyDown, méthode KeyOut, événements d’application accélérateurs sur la méthode OnKeyDown parente sur l’événement keyversion parent sur le parent (se propage à la racine)...
Événements CharacterReceived PreviewKeyUp événements KeyUpEvents

Lorsque l’événement d’accélérateur est géré, l’événement KeyOut est également marqué comme géré. L’événement KeyUp reste non géré.

### <a name="resolving-accelerators"></a>Résolution des accélérateurs

Un événement d’accélérateur de clavier est propagé à partir de l’élément qui a le focus jusqu’à la racine. Si l’événement n’est pas géré, l’infrastructure XAML recherche d’autres accélérateurs d’application non délimités en dehors du chemin de propagation.

Lorsque deux accélérateurs de clavier sont définis avec la même combinaison de touches, la première touche de l’ensemble de l’arborescence d’éléments visuels est appelée.

Les accélérateurs de clavier délimités sont appelés uniquement lorsque le focus se trouve dans une étendue spécifique. Par exemple, dans une grille qui contient des dizaines de contrôles, une touche d’accès rapide peut être appelée pour un contrôle uniquement lorsque le focus est dans la grille (le propriétaire de l’étendue).

### <a name="scoping-accelerators-programmatically"></a>Étendue des accélérateurs par programmation

La méthode [UIElement. TryInvokeKeyboardAccelerator](/uwp/api/windows.ui.xaml.uielement.tryinvokekeyboardaccelerator) appelle tous les accélérateurs correspondants dans la sous-arborescence de l’élément.

La méthode [UIElement. OnProcessKeyboardAccelerators](/uwp/api/windows.ui.xaml.uielement.onprocesskeyboardaccelerators) est exécutée avant l’accélérateur clavier. Cette méthode passe un objet [ProcessKeyboardAcceleratorArgs](/uwp/api/windows.ui.xaml.input.processkeyboardacceleratoreventargs) qui contient la clé, le modificateur et une valeur booléenne indiquant si l’accélérateur clavier est géré. S’il est marqué comme étant géré, l’accélérateur de clavier se propage (l’accélérateur de clavier externe n’est donc jamais appelé).

> [!NOTE]
> OnProcessKeyboardAccelerators se déclenche toujours, qu’il soit géré ou non (semblable à l’événement OnKeyDown). Vous devez vérifier si l’événement a été marqué comme géré.

Dans cet exemple, nous utilisons OnProcessKeyboardAccelerators et TryInvokeKeyboardAccelerator pour étendre les accélérateurs de clavier à l’objet page :

``` csharp
protected override void OnProcessKeyboardAccelerators(
  ProcessKeyboardAcceleratorArgs args)
{
  if(args.Handled != true)
  {
    this.TryInvokeKeyboardAccelerator(args);
    args.Handled = true;
  }
}
```

### <a name="localize-the-accelerators"></a>Localiser les accélérateurs

Nous vous recommandons de localiser tous les accélérateurs de clavier. Vous pouvez le faire avec le fichier de ressources UWP standard (. resw) et l’attribut x :Uid dans vos déclarations XAML. Dans cet exemple, le Windows Runtime charge automatiquement les ressources.

![Localisation de l’accélérateur clavier avec les ressources UWP fichier de ](images/accelerators/accelerators_localization.png)
 ***localisation avec les ressources UWP***

``` xaml
<Button x:Uid="myButton" Click="OnSave">
  <Button.KeyboardAccelerators>
    <KeyboardAccelerator x:Uid="myKeyAccelerator" Modifiers="Control"/>
  </Button.KeyboardAccelerators>
</Button>
```

### <a name="setup-an-accelerator-programmatically"></a>Configurer un accélérateur par programmation

Voici un exemple de définition par programmation d’un accélérateur :

``` csharp
void AddAccelerator(
  VirtualKeyModifiers keyModifiers, 
  VirtualKey key, 
  TypedEventHandler<KeyboardAccelerator, KeyboardAcceleratorInvokedEventArgs> handler )
  {
    var accelerator = 
      new KeyboardAccelerator() 
      { 
        Modifiers = keyModifiers, Key = key
      };
    accelerator.Invoked += handler;
    this.KeyboardAccelerators.Add(accelerator);
  }
```

> [!NOTE]
> KeyboardAccelerator n’est pas partageable, le même KeyboardAccelerator ne peut pas être ajouté à plusieurs éléments.

### <a name="override-keyboard-accelerator-behavior"></a>Remplacer le comportement de l’accélérateur du clavier

Vous pouvez gérer l’événement [KeyboardAccelerator. invoked](/uwp/api/windows.ui.xaml.input.keyboardaccelerator.Invoked) pour remplacer le comportement de KeyboardAccelerator par défaut.

Cet exemple montre comment remplacer la commande « Sélectionner tout » (raccourci clavier Ctrl + A) dans un contrôle ListView personnalisé. Nous définissons également la propriété Handled sur true pour arrêter davantage la propagation des événements.

```csharp
public class MyListView : ListView
{
  …
  protected override void OnKeyboardAcceleratorInvoked(KeyboardAcceleratorInvokedEventArgs args) 
  {
    if(args.Accelerator.Key == VirtualKey.A 
      && args.Accelerator.Modifiers == KeyboardModifiers.Control)
    {
      CustomSelectAll(TypeOfSelection.OnlyNumbers); 
      args.Handled = true;
    }
  }
  …
}
```

## <a name="related-articles"></a>Articles connexes

- [Interactions avec le clavier](keyboard-interactions.md)
- [Clés d’accès](access-keys.md)

### <a name="samples"></a>Exemples

- [Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery)
