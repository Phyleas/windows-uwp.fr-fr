---
description: Découvrez comment utiliser les cases d’option qui permettent aux utilisateurs de sélectionner une option dans une collection constituée d’au moins deux options apparentées, mais qui s’excluent mutuellement.
title: Recommandations en matière de cases d’option
ms.assetid: 41E3F928-AA55-42A2-9281-EC3907C4F898
label: Radio buttons
template: detail.hbs
ms.date: 06/10/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 9d6fe34d9f53142cfe2148f79bf936a473012a49
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219052"
---
# <a name="radio-buttons"></a>Cases d’option

Les cases d’option, ou boutons radio, permettent aux utilisateurs de sélectionner une option dans une collection constituée d’au moins deux options apparentées, mais qui s’excluent mutuellement. Les cases d’option sont toujours utilisées dans des groupes, et chacune d’elles est représentée par une case d’option dans le groupe.

Dans l’état par défaut, aucune case d’option n’est sélectionnée dans un groupe RadioButtons. Autrement dit, toutes les cases d’option sont désactivées. Toutefois, une fois qu’un utilisateur a sélectionné une case d’option, il ne peut plus désélectionner le bouton pour restaurer le groupe à son état vierge initial.

Le comportement singulier d’un groupe RadioButtons le distingue des [cases à cocher](checkbox.md), qui prennent en charge la sélection multiple et la désélection, ou désactivation.

:::image type="content" source="images/controls/radio-button.png" alt-text="Exemple de groupe RadioButtons avec une case d’option sélectionnée":::

**Obtenir la bibliothèque d’interface utilisateur Windows**

| &nbsp; | &nbsp; |
| - | - |
| ![Logo WinUI](images/winui-logo-64x64.png) | Le contrôle RadioButtons est inclus dans la bibliothèque d’IU Windows, package NuGet qui contient les nouveaux contrôles et fonctionnalités d’interface utilisateur pour les applications Windows. Pour plus d’informations, notamment des instructions d’installation, consultez la [bibliothèque d’interface utilisateur Windows](/uwp/toolkits/winui/). |

> **API de la bibliothèque d’interface utilisateur Windows** : [RadioButtons, classe](/uwp/api/microsoft.ui.xaml.controls.radiobuttons), [SelectedItem, propriété](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem), [SelectedIndex, propriété](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex), [SelectionChanged, événement](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged)
>
> **API de plateforme** : [RadioButton, classe](/uwp/api/Windows.UI.Xaml.Controls.RadioButton), [IsChecked, propriété](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked), [Checked, événement](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.Checked)

> [!IMPORTANT]
> **RadioButtons ou RadioButton ?**
>
>Il existe deux façons de créer des groupes de cases d’option.
>
>- À compter de WinUI 2.3, nous vous recommandons d’utiliser le contrôle **[RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons)** . Ce contrôle simplifie la disposition, gère la navigation au clavier et l’accessibilité, et prend en charge la liaison à une source de données.
>- Vous pouvez utiliser des groupes de contrôles **[RadioButton](/uwp/api/Windows.UI.Xaml.Controls.RadioButton)** individuels. Si votre application n’utilise pas WinUI 2.3 ou une version ultérieure, il s’agit de la seule option.

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

Utilisez des cases d’option pour permettre aux utilisateurs d’effectuer une sélection parmi au moins deux options qui s’excluent mutuellement.

:::image type="content" source="images/radiobutton_basic.png" alt-text="Exemple de groupe RadioButtons avec une case d’option sélectionnée":::

Utilisez les cases d’option quand les utilisateurs ont besoin de voir toutes les options avant d’effectuer une sélection. Les cases d’option mettent en évidence toutes les options de la même manière. Ainsi, elles peuvent donner plus d’importance à une option qu’elle n’en a vraiment ou que vous voulez lui en donner.

À moins que les options méritent toutes la même attention, envisagez d’utiliser d’autres contrôles. Par exemple, pour recommander l’option convenant le mieux à la plupart des utilisateurs dans la plupart des cas, utilisez une [zone de liste modifiable](combo-box.md) pour l’afficher comme option par défaut.

:::image type="content" source="images/combo_box_collapsed.png" alt-text="Exemple de groupe RadioButtons avec une case d’option sélectionnée":::

S’il n’existe que deux options possibles qui peuvent être exprimées clairement en tant que choix binaire unique, par exemple activé/désactivé ou oui/non, combinez-les dans un contrôle [case à cocher](checkbox.md) ou [bouton bascule](toggles.md) unique. Par exemple, utilisez une case à cocher unique pour « J’accepte », plutôt que deux cases d’option pour « J’accepte » et « Je n’accepte pas ».

N’utilisez pas deux cases d’option pour un choix binaire unique :

:::image type="content" source="images/radiobutton-vs-checkbox-rb.png" alt-text="Exemple de groupe RadioButtons avec une case d’option sélectionnée":::

Utilisez plutôt une case à cocher :

:::image type="content" source="images/radiobutton-vs-checkbox-cb.png" alt-text="Exemple de groupe RadioButtons avec une case d’option sélectionnée":::

Quand les utilisateurs peuvent sélectionner plusieurs options, utilisez des [cases à cocher](checkbox.md).

:::image type="content" source="images/checkbox2.png" alt-text="Exemple de groupe RadioButtons avec une case d’option sélectionnée":::

Quand les utilisateurs doivent choisir une valeur dans une plage de valeurs (par exemple, *10, 20, 30,... 100*), utilisez un contrôle de [curseur](slider.md).

:::image type="content" source="images/controls/slider.png" alt-text="Exemple de groupe RadioButtons avec une case d’option sélectionnée":::

S’il y a plus de huit options, utilisez une [zone de liste modifiable](combo-box.md).

:::image type="content" source="images/combo_box_scroll.png" alt-text="Exemple de groupe RadioButtons avec une case d’option sélectionnée":::

Si les options disponibles dépendent du contexte actuel d’une application ou si elles peuvent changer de façon dynamique, utilisez un contrôle de liste.

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="The XAML Controls Gallery app icon"></img></td>
<td>
    <p>Si vous disposez de l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, <a href="xamlcontrolsgallery:/item/RadioButton">ouvrez-la pour voir comment fonctionne le contrôle RadioButtons</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="winui-radiobuttons-overview"></a>Vue d’ensemble du contrôle RadioButtons de WinUI

L’accès au clavier et le comportement de navigation ont été optimisés dans le contrôle [RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons). Ces améliorations favorisent l’accessibilité et permettent aux utilisateurs privilégiant l’utilisation du clavier de naviguer dans la liste d’options plus rapidement et plus facilement.

En plus de ces améliorations, la disposition visuelle par défaut des cases d’option individuelles au sein d’un groupe RadioButtons a également été optimisée avec des paramètres automatisés d’orientation, d’espacement et de marges. Ces améliorations vous dispensent de spécifier ces propriétés, comme pouvait l’exiger un contrôle de regroupement plus primitif comme [StackPanel](../layout/layout-panels.md#stackpanel) ou [Grid](../layout/layout-panels.md#grid).

### <a name="navigating-a-radiobuttons-group"></a>Navigation dans un groupe RadioButtons

Le contrôle `RadioButtons` a un comportement de navigation spécial qui aide les utilisateurs du clavier à parcourir la liste plus rapidement et plus facilement.

#### <a name="keyboard-focus"></a>Focus clavier

Le contrôle `RadioButtons` prend en charge deux états :

- Aucune case d’option n’est sélectionnée.
- Une case d’option est sélectionnée.

Les sections suivantes décrivent le comportement de focus du contrôle dans chaque état.

##### <a name="no-radio-button-is-selected"></a>Aucune case d’option n’est sélectionnée.

Quand aucune case d’option n’est sélectionnée, la première case d’option de la liste obtient le focus.

> [!NOTE]
> L’élément qui reçoit le focus au début de la navigation par touche de tabulation n’est pas sélectionné.

:::row:::
   :::column span="":::
     **_Liste sans le focus de tabulation, aucune sélection_**

     ![Liste sans le focus de tabulation et aucun élément sélectionné](images/radiobutton-no-selected-item-no-tab-focus.png)
   :::column-end:::
   :::column span="":::
      **_Liste avec le focus de tabulation initial, aucune sélection_**

      ![Liste avec le focus de tabulation initial et aucun élément sélectionné](images/radiobutton-no-selected-item-tab-focus.png)
   :::column-end:::
:::row-end:::

##### <a name="one-radio-button-is-selected"></a>Une case d’option est sélectionnée.

Quand un utilisateur accède à l’aide de la touche de tabulation à une liste où une case d’option est déjà sélectionnée, la case d’option sélectionnée obtient le focus.

:::row:::
   :::column span="":::
     **_Liste sans le focus de tabulation_**

     ![Liste sans le focus de tabulation et un élément sélectionné](images/radiobutton-selected-item-no-tab-focus.png)
   :::column-end:::
   :::column span="":::
      **_Liste avec le focus de tabulation initial_**

      ![Liste avec le focus de tabulation initial et un élément sélectionné](images/radiobutton-selected-item-tab-focus.png)
   :::column-end:::
:::row-end:::

#### <a name="keyboard-navigation"></a>Navigation au clavier

Pour plus d’informations sur les comportements de navigation généraux avec le clavier, consultez [Interactions avec le clavier – Navigation](../input/keyboard-interactions.md#navigation).

Lorsqu’un élément d’un groupe `RadioButtons` a déjà le focus, l’utilisateur peut utiliser les touches de direction pour la « navigation interne » entre les éléments du groupe. Les touches de direction Haut et Bas permettent d’accéder à l’élément logique « suivant » ou « précédent », comme défini dans votre balisage XAML. Les touches de direction Gauche et Droite permettent de se déplacer de manière spatiale.

##### <a name="navigation-within-single-column-or-single-row-layouts"></a>Navigation dans les dispositions à une seule colonne ou à une seule ligne

Dans une disposition à une seule colonne ou à une seule ligne, la navigation au clavier entraîne le comportement suivant :

:::row:::
   :::column span="":::
     **_Une seule colonne_**

     ![Exemple de navigation au clavier dans un groupe RadioButtons à une seule colonne](images/radiobutton-keyboard-navigation-single-column.png)

     Les touches de direction Haut et Bas permettent de se déplacer entre les éléments.</br>Les touches de direction Gauche et Droite ne font rien.
   :::column-end:::
   :::column span="":::
      **_Ligne unique_**

      ![Exemple de navigation au clavier dans un groupe RadioButtons à une seule ligne](images/radiobutton-keyboard-navigation-single-row.png)

      Les touches de direction Gauche et Haut permettent d’accéder à l’élément précédent, tandis que les touches de direction Droite et Bas permettent d’accéder à l’élément suivant.
   :::column-end:::
:::row-end:::

##### <a name="navigation-within-multi-column-multi-row-layouts"></a>Navigation dans les dispositions à plusieurs colonnes ou plusieurs lignes

Dans une disposition en grille à plusieurs colonnes et à plusieurs lignes, la navigation au clavier entraîne ce comportement :

**_Touches de direction Gauche/Droite_**

:::row:::
   :::column span="":::
      ![Exemple de navigation horizontale au clavier dans un groupe RadioButtons à plusieurs colonnes/lignes](images/radiobutton-keyboard-navigation-multi-column-row-1.png)

      

      Les touches de direction Gauche et Droite déplacent le focus horizontalement entre les éléments d’une ligne.
   :::column-end:::
   :::column span="":::
     ![Exemple de navigation horizontale au clavier avec focus sur le dernier élément d’une colonne](images/radiobutton-keyboard-navigation-multi-column-row-3.png)

      Lorsque le focus se trouve sur le dernier élément d’une colonne et que la touche de direction Droite ou Gauche est enfoncée, le focus se déplace vers le dernier élément de la colonne suivante ou précédente (s’il y en a une).
   :::column-end:::
:::row-end:::

**_Touches de direction Haut/Bas_**

:::row:::
   :::column span="":::
      ![Exemple de navigation verticale au clavier dans un groupe RadioButtons à plusieurs colonnes/lignes](images/radiobutton-keyboard-navigation-multi-column-row-2.png)

      Les touches de direction Haut et Bas permettent de déplacer le focus verticalement entre les éléments d’une colonne.
   :::column-end:::
   :::column span="":::
     ![Exemple de navigation verticale au clavier avec focus sur le dernier élément d’une colonne](images/radiobutton-keyboard-navigation-multi-column-row-4.png)

      Lorsque le focus se trouve sur le dernier élément d’une colonne et que la touche de direction Bas est enfoncée, le focus se déplace vers le premier élément de la colonne suivante (s’il y en a une). Lorsque le focus se trouve sur le premier élément d’une colonne et que la touche de direction Haut est enfoncée, le focus se déplace vers le dernier élément de la colonne précédente (s’il y en a une).
   :::column-end:::
:::row-end:::

Pour plus d’informations, consultez [Interactions avec le clavier](../input/keyboard-interactions.md#wrapping-homogeneous-list-and-grid-view-items).

###### <a name="wrapping"></a>Renvoi à la ligne

Le groupe RadioButtons ne renvoie pas le focus de la première ligne ou colonne jusqu’à la dernière, ni de la dernière ligne ou colonne jusqu’à la première. En effet, les utilisateurs qui ont recours à un lecteur d’écran perdent ce sentiment de limite et n’ont plus d’indication claire de début et de fin. La navigation dans la liste devient alors particulièrement difficile pour les utilisateurs malvoyants.

Le contrôle `RadioButtons` ne prend pas non plus en charge l’énumération, car il est censé contenir un nombre raisonnable d’éléments (consultez [Est-ce le bon contrôle ?](#is-this-the-right-control)).

### <a name="selection-follows-focus"></a>La sélection suit le focus.

Lorsque vous utilisez le clavier pour naviguer entre les éléments d’un groupe `RadioButtons`, quand le focus passe d’un élément au suivant, l’élément qui prend le focus est sélectionné et l’élément qui le perd est désactivé.

:::row:::
   :::column span="":::
      **_Avant la navigation au clavier_**

      ![Exemple de focus et de sélection avant la navigation au clavier](images/radiobutton-two-selected-before-keyboard-navigation.png)

      Focus et sélection avant la navigation au clavier.
   :::column-end:::
   :::column span="":::
     **_Après la navigation au clavier_**

      ![Exemple de focus et de sélection après la navigation au clavier](images/radiobutton-three-selected-after-keyboard-navigation.png)

      Focus et sélection après la navigation au clavier, où la touche de direction Bas déplace le focus sur la case d’option 3, la sélectionne et désactive la case d’option 2.
   :::column-end:::
:::row-end:::

Vous pouvez déplacer le focus sans changer la sélection en naviguant à l’aide de Ctrl+touches de direction. Une fois le focus déplacé, vous pouvez utiliser la barre d’espace pour sélectionner l’élément qui a actuellement le focus.

#### <a name="navigating-with-xbox-gamepad-and-remote-control"></a>Navigation avec une manette de jeu ou une télécommande Xbox

Si vous naviguez entre des cases d’option avec une télécommande ou une manette de jeu Xbox, le comportement « la sélection suit le focus » est désactivé. Vous devez appuyer sur le bouton « A » pour sélectionner la case d’option qui a le focus.

## <a name="accessibility-behavior"></a>Comportement en matière d’accessibilité

Le tableau suivant décrit la façon dont le Narrateur gère un groupe `RadioButtons` et ce qui est annoncé. Ce comportement dépend de la manière dont l’utilisateur a défini les préférences du Narrateur.

| Action | Annonce du Narrateur |
|:--|:--|
| Le focus se déplace sur l’élément sélectionné | « _nom_, RadioButton, sélectionné, _x_ sur _N_ » |
|Le focus se déplace vers un élément non sélectionné<br> *(Si vous naviguez avec Ctrl+touches de direction ou la manette de jeu Xbox,<br>ce qui indique que la sélection ne suit pas le focus.)* | « _nom_, RadioButton, non sélectionné, _x_ sur _N_ »  |

> [!NOTE]
> Le _**nom**_ que le narrateur annonce pour chaque élément est la valeur de la propriété jointe [AutomationProperties.Name](/uwp/api/windows.ui.xaml.automation.automationproperties.nameproperty) si elle est disponible pour l’élément. Sinon, il s’agit de la valeur retournée par la méthode [ToString](/dotnet/api/system.object.tostring?view=dotnet-uwp-10.0) de l’élément.
>
> _**x**_ est le numéro de l’élément actuel. _**N**_ est le nombre total d’éléments dans le groupe.

## <a name="create-a-winui-radiobuttons-group"></a>Créer un groupe RadioButtons WinUI

Le contrôle `RadioButtons` utilise un modèle de contenu similaire à un [ItemsControl](/uwp/api/windows.ui.xaml.controls.itemscontrol). Cela signifie que vous pouvez :

- Le remplir en ajoutant des éléments directement à la collection [Items](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.items) ou en liant des données à sa propriété [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.itemssource).
- Utiliser les propriétés [SelectedIndex](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex) ou [SelectedItem](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem) pour récupérer et définir l’option sélectionnée.
- Gérer l’événement [SelectionChanged](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged) pour agir lorsqu’une option est choisie.

> [!TIP]
> Tout au long de ce document, nous utilisons l’alias **muxc** en XAML pour représenter les API de la bibliothèque d’interface utilisateur Windows que nous avons incluses dans notre projet. Nous l’avons ajouté à notre élément [Page](/uwp/api/windows.ui.xaml.controls.page) : `xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`
>
>Dans le code-behind, nous utilisons également l’alias **muxc** en C# pour représenter les API de la bibliothèque d’interface utilisateur Windows que nous avons incluses dans notre projet. Nous avons ajouté cette instruction **using** en haut du fichier : `using muxc = Microsoft.UI.Xaml.Controls;`

Ici, vous déclarez un contrôle `RadioButtons` simple avec trois options. La propriété [Header](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.header) est définie de façon à donner une étiquette au groupe, et la propriété `SelectedIndex` est définie de façon à fournir une option par défaut.

```xaml
<muxc:RadioButtons Header="Background color"
                   SelectedIndex="0"
                   SelectionChanged="BackgroundColor_SelectionChanged">
    <x:String>Red</x:String>
    <x:String>Green</x:String>
    <x:String>Blue</x:String>
</muxc:RadioButtons>
```

Le résultat ressemble à ceci :

:::image type="content" source="images/radiobuttons-default-group.png" alt-text="Exemple de groupe RadioButtons avec une case d’option sélectionnée":::

Pour effectuer une action lorsque l’utilisateur sélectionne une option, gérez l’événement [SelectionChanged](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged). Ici, vous changez la couleur d’arrière-plan d’un élément [Border](/uwp/api/windows.ui.xaml.controls.border) nommé « ExampleBorder » (`<Border x:Name="ExampleBorder" Width="100" Height="100"/>`).

```csharp
private void BackgroundColor_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (ExampleBorder != null && sender is muxc.RadioButtons rb)
    {
        string colorName = rb.SelectedItem as string;
        switch (colorName)
        {
            case "Red":
                ExampleBorder.Background = new SolidColorBrush(Colors.Red);
                break;
            case "Green":
                ExampleBorder.Background = new SolidColorBrush(Colors.Green);
                break;
            case "Blue":
                ExampleBorder.Background = new SolidColorBrush(Colors.Blue);
                break;
        }
    }
}
```

> [!TIP]
> Vous pouvez également obtenir l’élément sélectionné à partir de la propriété [SelectionChangedEventArgs.AddedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems). Il n’y aura qu’un seul élément sélectionné, à l’index 0. Vous pourriez donc récupérer l’élément sélectionné comme suit : `string colorName = e.AddedItems[0] as string;`.

### <a name="selection-states"></a>États de sélection

Une case d’option a deux états : sélectionnée ou désactivée. Lorsqu’une option est sélectionnée dans un groupe `RadioButtons`, vous pouvez obtenir sa valeur à partir de la propriété [SelectedItem](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem) et son emplacement dans la collection à partir de la propriété [SelectedIndex](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex). Une case d’option peut être désactivée si l’utilisateur sélectionne une autre case d’option dans le même groupe, mais pas s’il la sélectionne de nouveau. Toutefois, vous pouvez désactiver un groupe de cases d’option par programmation en définissant `SelectedItem = null` ou `SelectedIndex = -1`. (Affecter à `SelectedIndex` n’importe quelle valeur en dehors de la plage de la collection `Items` entraîne l’absence de sélection.)

### <a name="radiobuttons-content"></a>Contenu de RadioButtons

Dans l’exemple précédent, vous avez rempli le contrôle `RadioButtons` avec des chaînes simples. Le contrôle a fourni les cases d’option et utilisé les chaînes comme étiquette pour chacune d’elles.

Toutefois, vous pouvez remplir le contrôle `RadioButtons` avec n’importe quel objet. En règle générale, l’objet doit fournir une représentation sous forme de chaîne utilisable en tant qu’étiquette de texte. Dans certains cas, une image peut convenir à la place du texte.

Ici, les éléments [SymbolIcon](/uwp/api/windows.ui.xaml.controls.symbolicon) sont utilisés pour remplir le contrôle.

```xaml
<muxc:RadioButtons Header="Choose the icon without an arrow">
    <SymbolIcon Symbol="Back"/>
    <SymbolIcon Symbol="Attach"/>
    <SymbolIcon Symbol="HangUp"/>
    <SymbolIcon Symbol="FullScreen"/>
</muxc:RadioButtons>
```

:::image type="content" source="images/radiobuttons-symbolicon.png" alt-text="Exemple de groupe RadioButtons avec une case d’option sélectionnée":::

Vous pouvez également utiliser des contrôles [RadioButton](/uwp/api/Windows.UI.Xaml.Controls.RadioButton) individuels pour remplir les éléments `RadioButtons`. Il s’agit d’un cas particulier que nous aborderons plus loin. Consultez [Contrôles RadioButton dans un groupe RadioButtons]().

L’avantage de pouvoir utiliser n’importe quel objet est que vous pouvez lier le contrôle `RadioButtons` à un type personnalisé dans votre modèle de données. Vous en verrez une illustration dans la section suivante.

### <a name="data-binding"></a>Liaison de données

Le contrôle `RadioButtons` prend en charge la liaison de données à sa propriété [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.itemssource). Cet exemple montre comment lier le contrôle à une source de données personnalisée. L’apparence et les fonctionnalités de cet exemple sont identiques à celles de l’exemple de couleur d’arrière-plan précédent, mais ici les pinceaux de couleur sont stockés dans le modèle de données au lieu d’être créés dans le gestionnaire d’événements `SelectionChanged`.

```xaml
 <muxc:RadioButtons Header="Background color"
                    SelectedIndex="0"
                    SelectionChanged="BackgroundColor_SelectionChanged"
                    ItemsSource="{x:Bind colorOptionItems}"/>
```

```c#
public sealed partial class MainPage : Page
{
    // Custom data item.
    public class ColorOptionDataModel
    {
        public string Label { get; set; }
        public SolidColorBrush ColorBrush { get; set; }

        public override string ToString()
        {
            return Label;
        }
    }

    List<ColorOptionDataModel> colorOptionItems;

    public MainPage1()
    {
        this.InitializeComponent();

        colorOptionItems = new List<ColorOptionDataModel>();
        colorOptionItems.Add(new ColorOptionDataModel()
            { Label = "Red", ColorBrush = new SolidColorBrush(Colors.Red) });
        colorOptionItems.Add(new ColorOptionDataModel()
            { Label = "Green", ColorBrush = new SolidColorBrush(Colors.Green) });
        colorOptionItems.Add(new ColorOptionDataModel()
            { Label = "Blue", ColorBrush = new SolidColorBrush(Colors.Blue) });
    }

    private void BackgroundColor_SelectionChanged(object sender, SelectionChangedEventArgs e)
    {
        var option = e.AddedItems[0] as ColorOptionDataModel;
        ExampleBorder.Background = option?.ColorBrush;
    }
}
```

### <a name="radiobutton-controls-in-a-radiobuttons-group"></a>Contrôles RadioButton dans un groupe RadioButtons

Vous pouvez utiliser des contrôles [RadioButton](/uwp/api/Windows.UI.Xaml.Controls.RadioButton) individuels pour remplir les éléments `RadioButtons`. Vous pourriez par exemple souhaiter accéder à certaines propriétés, comme `AutomationProperties.Name`, ou peut-être avez-vous du code `RadioButton` existant mais souhaitez tirer parti de la disposition et de la navigation de `RadioButtons`.

```xaml
<muxc:RadioButtons Header="Background color">
    <RadioButton Content="Red" Tag="red" AutomationProperties.Name="red"/>
    <RadioButton Content="Green" Tag="green" AutomationProperties.Name="green"/>
    <RadioButton Content="Blue" Tag="blue" AutomationProperties.Name="blue"/>
</muxc:RadioButtons>
```

Lorsque vous utilisez des contrôles `RadioButton` dans un groupe `RadioButtons`, le contrôle `RadioButtons` sait comment présenter le `RadioButton` ; ainsi, vous ne vous retrouverez pas avec deux cercles de sélection.

Toutefois, vous devez avoir connaissance de certains comportements. Nous vous recommandons de gérer l’état et les événements sur les contrôles individuels ou sur `RadioButtons`, mais pas les deux, afin d’éviter les conflits.

Le tableau ci-dessous montre les événements et les propriétés connexes sur les deux contrôles.

|RadioButton  |RadioButtons  |
|---------|---------|
|[Checked](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.checked), [Unchecked](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.unchecked), [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) |    [SelectionChanged](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged) |
|[IsChecked](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.ischecked)  | [SelectedItem](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem), [SelectedIndex](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex) |

Si vous gérez des événements sur un `RadioButton` individuel, tels que `Checked` ou `Unchecked`, et que vous gérez également l’événement `RadioButtons.SelectionChanged`, les deux événements se déclenchent. L’événement `RadioButton` se produit en premier, suivi de l’événement `RadioButtons.SelectionChanged`, ce qui peut entraîner des conflits.

Les propriétés `IsChecked`, `SelectedItem` et `SelectedIndex` restent synchronisées. Une modification apportée à une propriété met à jour les deux autres.

La propriété [RadioButton.GroupName](/uwp/api/windows.ui.xaml.controls.radiobutton.groupname) est ignorée. Le groupe est créé par le contrôle `RadioButtons`.

### <a name="defining-multiple-columns"></a>Définition de plusieurs colonnes

Par défaut, le contrôle `RadioButtons` dispose ses cases d’option verticalement dans une seule colonne. Vous pouvez définir la propriété [MaxColumns](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.maxcolumns) afin que le contrôle dispose les cases d’option dans plusieurs colonnes. (Dans ce cas, elles sont présentées dans l’ordre de stockage en colonne [column-major], où les éléments sont renseignés de haut en bas, puis de gauche à droite.)

```xaml
<muxc:RadioButtons Header="Options" MaxColumns="3">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
    <x:String>Item 6</x:String>
</muxc:RadioButtons>
```

:::image type="content" source="images/radiobuttons-multi-column.png" alt-text="Exemple de groupe RadioButtons avec une case d’option sélectionnée":::

> [!TIP]
> Pour que les éléments soient disposés sur une seule ligne horizontale, affectez à `MaxColumns` une valeur égale au nombre d’éléments dans le groupe.

## <a name="create-your-own-radiobutton-group"></a>Créer votre propre groupe RadioButton

> [!Important]
> Nous vous recommandons d’utiliser le contrôle `RadioButtons` de WinUI pour regrouper des éléments `RadioButton`, sauf si vous utilisez une ancienne version de WinUI.

Les cases d’option s’utilisent en groupes. Vous pouvez regrouper des contrôles [RadioButton](/uwp/api/Windows.UI.Xaml.Controls.RadioButton) de deux manières :

- Placez-les dans le même conteneur parent.
- Définissez la propriété [GroupName](/uwp/api/Windows.UI.Xaml.Controls.RadioButton.GroupName) de chaque case d’option sur la même valeur.

Dans cet exemple, le premier groupe de cases d’option est implicitement formé en raison de son appartenance au même panneau d’empilement. Le deuxième groupe étant divisé entre deux panneaux d’empilement, `GroupName` est utilisé pour les regrouper explicitement dans un groupe unique.

```xaml
<StackPanel>
    <StackPanel>
        <TextBlock Text="Background" Style="{ThemeResource BaseTextBlockStyle}"/>
        <!-- Group 1 - implicit grouping -->
        <StackPanel Orientation="Horizontal">
            <RadioButton Content="Green" Tag="green" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Yellow" Tag="yellow" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Blue" Tag="blue" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="White" Tag="white" Checked="BGRadioButton_Checked"
                         IsChecked="True"/>
        </StackPanel>
    </StackPanel>

    <StackPanel>
        <TextBlock Text="BorderBrush" Style="{ThemeResource BaseTextBlockStyle}"/>
        <!-- Group 2 - grouped by GroupName -->
        <StackPanel Orientation="Horizontal">
            <StackPanel>
                <RadioButton Content="Green" Tag="green" GroupName="BorderBrush"
                             Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="Yellow" Tag="yellow" GroupName="BorderBrush"
                             Checked="BorderRadioButton_Checked" IsChecked="True"/>
            </StackPanel>
            <StackPanel>
                <RadioButton Content="Blue" Tag="blue" GroupName="BorderBrush"
                             Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="White" Tag="white"  GroupName="BorderBrush"
                             Checked="BorderRadioButton_Checked"/>
            </StackPanel>
        </StackPanel>
    </StackPanel>
    <Border x:Name="ExampleBorder"
            BorderBrush="#FFFFD700" Background="#FFFFFFFF"
            BorderThickness="10" Height="50" Margin="0,10"/>
</StackPanel>
```

```csharp
private void BGRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && ExampleBorder != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "yellow":
                ExampleBorder.Background = new SolidColorBrush(Colors.Yellow);
                break;
            case "green":
                ExampleBorder.Background = new SolidColorBrush(Colors.Green);
                break;
            case "blue":
                ExampleBorder.Background = new SolidColorBrush(Colors.Blue);
                break;
            case "white":
                ExampleBorder.Background = new SolidColorBrush(Colors.White);
                break;
        }
    }
}

private void BorderRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && ExampleBorder != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "yellow":
                ExampleBorder.BorderBrush = new SolidColorBrush(Colors.Gold);
                break;
            case "green":
                ExampleBorder.BorderBrush = new SolidColorBrush(Colors.DarkGreen);
                break;
            case "blue":
                ExampleBorder.BorderBrush = new SolidColorBrush(Colors.DarkBlue);
                break;
            case "white":
                ExampleBorder.BorderBrush = new SolidColorBrush(Colors.White);
                break;
        }
    }
}
```

Ces deux groupes de contrôles `RadioButton` ressemblent à ceci :

:::image type="content" source="images/radio-button-groups.png" alt-text="Exemple de groupe RadioButtons avec une case d’option sélectionnée":::

### <a name="radio-button-states"></a>États des cases d’option

Une case d’option a deux états : sélectionnée ou désactivée. Quand une case d’option est sélectionnée, sa propriété [IsChecked](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked) a la valeur `true`. Quand une case d’option est désactivée, sa propriété `IsChecked` a la valeur `false`. Une case d’option peut être désactivée si l’utilisateur sélectionne une autre case d’option dans le même groupe, mais pas s’il la sélectionne de nouveau. Toutefois, vous pouvez désactiver une case d’option par programmation en affectant la valeur `false` à sa propriété `IsChecked`.

### <a name="visuals-to-consider"></a>Visuels à prendre en considération

L’espacement par défaut des contrôles `RadioButton` individuels est différent de l’espacement fourni par un groupe `RadioButtons`. Pour appliquer l’espacement `RadioButtons` à des contrôles `RadioButton` individuels, utilisez une valeur `Margin` égale à `0,0,7,3`, comme indiqué ici.

```xaml
<StackPanel>
    <StackPanel.Resources>
        <Style TargetType="RadioButton">
            <Setter Property="Margin" Value="0,0,7,3"/>
        </Style>
    </StackPanel.Resources>
    <TextBlock Text="Background"/>
    <RadioButton Content="Item 1"/>
    <RadioButton Content="Item 2"/>
    <RadioButton Content="Item 3"/>
</StackPanel>
```

Les images suivantes montrent l’espacement préféré des cases d’option dans un groupe.

:::image type="content" source="images/radiobutton-layout.png" alt-text="Exemple de groupe RadioButtons avec une case d’option sélectionnée":::

:::image type="content" source="images/radiobutton-redline.png" alt-text="Exemple de groupe RadioButtons avec une case d’option sélectionnée":::

> [!NOTE]
> Si vous utilisez un contrôle RadioButtons de WinUI, l’espacement, les marges et l’orientation sont déjà optimisés.

## <a name="recommendations"></a>Recommandations

- Assurez-vous que le but et l’état actuel d’un ensemble de cases d’option sont explicites.
- Limitez l’étiquette de texte de la case d’option à une seule ligne.
- Si l’étiquette de texte est dynamique, songez au redimensionnement automatique du bouton et à ses conséquences sur les effets visuels environnants.
- Utilisez la police par défaut à moins que votre organisation ait passé d’autres consignes.
- Ne placez pas deux groupes RadioButtons côte à côte. Quand deux groupes RadioButtons sont adjacents, il peut être difficile pour les utilisateurs de déterminer quelles cases appartiennent à quel groupe.

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- Pour obtenir tous les contrôles XAML dans un format interactif, consultez [Exemple de Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery).

## <a name="related-topics"></a>Rubriques connexes

- [Boutons](buttons.md)
- [Boutons bascule](toggles.md)
- [Cases à cocher](checkbox.md)
- [Listes et zones de liste modifiable](lists.md)
- [Curseurs](slider.md)
- [Classe RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons)
- [Classe RadioButton](/uwp/api/windows.ui.xaml.controls.radiobutton)