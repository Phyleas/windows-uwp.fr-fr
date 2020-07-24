---
Description: Les cases d’option permettent aux utilisateurs de faire un choix parmi au moins deux possibilités.
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
ms.openlocfilehash: dc6f5eb32cdedf442b6866e1e53be85edfb98dcb
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493434"
---
# <a name="radio-buttons"></a>Cases d’option

Les cases d’option, ou boutons radio, permettent aux utilisateurs de sélectionner une option dans une collection constituée d’au moins deux options apparentées, mais qui s’excluent mutuellement. Chaque option est représentée par une case d’option.

Dans l’état par défaut, aucune case d’option n’est sélectionnée dans un groupe RadioButtons. Autrement dit, toutes les cases d’option sont désactivées. Toutefois, quand une case d’option est sélectionnée, l’état désactivé du groupe ne peut pas être restauré.

Le comportement singulier d’un groupe RadioButtons le distingue des [cases à cocher](checkbox.md), qui prennent en charge la sélection multiple et la désélection ou la désactivation.

![Exemple de groupe RadioButtons avec une case d’option sélectionnée](images/controls/radio-button.png)

## <a name="get-the-windows-ui-library"></a>Obtenir la bibliothèque d’interface utilisateur Windows

| &nbsp; | &nbsp; |
| - | - |
| ![Logo WinUI](images/winui-logo-64x64.png) | Le contrôle RadioButtons est inclus dans la bibliothèque d’IU Windows, package NuGet qui contient les nouveaux contrôles et fonctionnalités d’interface utilisateur pour les applications Windows. Pour plus d’informations et pour obtenir des instructions d’installation, consultez la [bibliothèque d’IU Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

**API de la bibliothèque d’interface utilisateur Windows** : 
* [Classe RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons)
* [Événement SelectionChanged](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged)
* [Propriété SelectedItem](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem)
* [Propriété SelectedIndex](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex)

**API de plateforme** : 
* [Classe RadioButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RadioButton)
* [Événement Checked](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.Checked)
* [Propriété IsChecked](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked)

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

Utilisez les cases d’option pour permettre aux utilisateurs d’effectuer une sélection parmi au moins deux options qui s’excluent mutuellement.

![Groupe RadioButtons avec une case d’option sélectionnée](images/radiobutton_basic.png)

Utilisez les cases d’option quand les utilisateurs ont besoin de voir toutes les options avant d’effectuer une sélection. Les cases d’option mettent en évidence toutes les options de la même manière. Ainsi, elles peuvent donner plus d’importance à une option qu’elle n’en a vraiment ou que vous voulez lui en donner. 

À moins que les options méritent toutes la même attention, envisagez d’utiliser d’autres contrôles. Par exemple, pour recommander l’option convenant le mieux à la plupart des utilisateurs dans la plupart des cas, utilisez une [zone de liste modifiable](combo-box.md) pour l’afficher comme option par défaut.

![Zone de liste modifiable affichant une option par défaut](images/combo_box_collapsed.png)

S’il n’y a que deux options qui s’excluent mutuellement, combinez-les dans un contrôle de [case à cocher](checkbox.md) ou de [bouton bascule](toggles.md) unique. Par exemple, utilisez une case à cocher unique pour « J’accepte », plutôt que deux cases d’option pour « J’accepte » et « Je n’accepte pas ».

![Une case à cocher est une bonne alternative pour présenter un choix binaire.](images/radiobutton_vs_checkbox.png)

Quand les utilisateurs peuvent sélectionner plusieurs options, utilisez des [cases à cocher](checkbox.md).

![Les cases à cocher prennent en charge la sélection multiple.](images/checkbox2.png)

Quand les utilisateurs doivent choisir une valeur dans une plage de valeurs (par exemple, *10, 20, 30,... 100*), utilisez un contrôle de [curseur](slider.md).

![Contrôle de curseur affichant une valeur dans une plage de valeurs](images/controls/slider.png)

Si au moins huit options sont proposées, utilisez une [zone de liste modifiable](combo-box.md).

![Zone de liste affichant plusieurs options](images/combo_box_scroll.png)

> [!NOTE]
> Si les options disponibles dépendent du contexte actuel d’une application ou si elles peuvent changer de façon dynamique, utilisez un contrôle de liste.

## <a name="radiobuttons-behavior"></a>Comportement de RadioButtons

L’accès clavier et le comportement de navigation ont été optimisés dans la [classe RadioButton](/uwp/api/windows.ui.xaml.controls.radiobutton?view=winrt-19041). Ces améliorations favorisent l’accessibilité et permettent aux utilisateurs privilégiant l’utilisation du clavier de naviguer dans la liste d’options plus rapidement et plus facilement.

En plus de ces améliorations, la disposition visuelle par défaut des cases d’option individuelles au sein d’un groupe RadioButtons a également été optimisée avec des paramètres automatisés d’orientation, d’espacement et de marges. Ces améliorations vous dispensent de spécifier ces propriétés, comme pouvait l’exiger un contrôle de regroupement plus primitif comme [StackPanel](../layout/layout-panels.md#stackpanel) ou [Grid](../layout/layout-panels.md#grid).

### <a name="navigating-a-radiobuttons-group"></a>Navigation dans un groupe RadioButtons

Le contrôle RadioButtons prend en charge deux états :

- Aucune case d’option n’est sélectionnée.
- Une case d’option est sélectionnée.

Les deux sections suivantes se penchent sur les deux comportements de focus pour les cases d’option.

#### <a name="no-radio-button-is-selected"></a>Aucune case d’option n’est sélectionnée.

Quand aucune case d’option n’est sélectionnée, la première case d’option de la liste obtient le focus.

> [!NOTE]
> L’élément qui reçoit le focus au début de la navigation par touche de tabulation n’est pas sélectionné.

|Liste sans le focus de tabulation | Liste avec le focus de tabulation initial|
|:--:|:--:|
| ![Liste sans le focus de tabulation](images/radiobutton-no-selected-item-no-tab-focus.png) | ![Liste avec le focus de tabulation initial](images/radiobutton-no-selected-item-tab-focus.png)|

#### <a name="one-radio-button-is-selected"></a>Une case d’option est sélectionnée.

Quand une case d’option est sélectionnée et que l’utilisateur accède à la liste à l’aide de la touche de tabulation, la case d’option sélectionnée obtient le focus.

|Liste sans le focus de tabulation | Liste avec le focus de tabulation initial |
|:--:|:--:|
| ![Liste sans le focus de tabulation](images/radiobutton-selected-item-no-tab-focus.png) | ![Liste avec le focus de tabulation initial](images/radiobutton-selected-item-tab-focus.png)|


### <a name="keyboard-navigation"></a>Navigation au clavier

Quand les utilisateurs ont une seule ligne ou colonne de cases d’option et qu’un élément a déjà reçu le focus de tabulation, ils peuvent utiliser les touches de direction pour effectuer une « navigation interne » entre les éléments du contrôle RadioButtons. Pour plus d’informations sur les comportements de navigation avec le clavier, consultez [Interactions avec le clavier – Navigation](../input/keyboard-interactions.md#navigation).

Pour un contrôle RadioButtons, quand la liste d’options est organisée verticalement uniquement, les touches de direction haut et bas permettent de naviguer entre les éléments, tandis que les touches de direction gauche et droite sont inopérantes. En revanche, dans une liste organisée horizontalement uniquement, les touches de direction gauche et droite et haut et bas permettent de naviguer entre les éléments de la même façon.

![Exemple de navigation au clavier dans un groupe RadioButtons à une seule colonne ou une seule ligne](images/radiobutton-keyboard-navigation-single-column-row.png)<br/>
*Exemple de navigation au clavier dans un groupe RadioButtons à une seule colonne ou une seule ligne*

#### <a name="navigating-within-multi-column-or-multi-row-layouts"></a>Navigation dans les dispositions à plusieurs colonnes ou plusieurs lignes

Dans l’ordre column-major, le focus se déplace de haut en bas et de gauche à droite. Quand le focus est sur le dernier élément d’une colonne et que la touche de direction bas est enfoncée, le focus se place sur le premier élément de la colonne suivante. Ce comportement concerne également l’ordre inverse : quand le focus est placé sur le premier élément d’une colonne et que la touche de direction haut est enfoncée, le focus se place sur le dernier élément de la colonne précédente.

![Exemple de navigation au clavier dans un groupe RadioButtons à plusieurs colonnes/lignes](images/radiobutton-keyboard-navigation-multi-column-row.png)

Dans un ordre « row-major » (où les éléments sont renseignés de gauche à droite et de haut en bas), quand le focus est sur le dernier élément d’une ligne et que la touche de direction droite est enfoncée, le focus se déplace sur le premier élément de la ligne suivante. Ce comportement concerne également l’ordre inverse : quand le focus est placé sur le premier élément d’une ligne et que la touche de direction gauche est enfoncée, le focus se place sur le dernier élément de la ligne précédente.

Pour plus d’informations, consultez [Interactions avec le clavier](https://docs.microsoft.com/windows/uwp/design/input/keyboard-interactions#wrapping-homogeneous-list-and-grid-view-items).

##### <a name="wrapping"></a>Renvoi à la ligne

Le groupe RadioButtons n’est pas renvoyé à la ligne. En effet, les utilisateurs qui ont recours à un lecteur d’écran perdent ce sentiment de limite et n’ont plus d’indication claire de début et de fin. La navigation dans la liste devient alors particulièrement difficile pour les utilisateurs malvoyants. Le contrôle RadioButtons ne prend pas non plus en charge l’énumération, car il est censé contenir un nombre raisonnable d’éléments (consultez [Est-ce le bon contrôle ?](#is-this-the-right-control)).

## <a name="selection-follows-focus"></a>La sélection suit le focus.

Quand les utilisateurs ont recours au clavier pour naviguer entre les éléments d’une liste RadioButtons (dans laquelle un élément est déjà sélectionné), quand le focus passe d’un élément à l’autre, l’élément qui prend le focus est sélectionné et l’élément qui le perd est désactivé.

|Avant la navigation au clavier | Après la navigation au clavier|
|:--|:--|
| ![Exemple de focus et de sélection avant la navigation au clavier](images/radiobutton-two-selected-before-keyboard-navigation.png)</br>*Exemple de focus et de sélection avant la navigation au clavier* | ![Exemple de focus et de sélection après la navigation au clavier](images/radiobutton-three-selected-after-keyboard-navigation.png)<br/>*Exemple de focus et de sélection après la navigation au clavier où la touche de direction bas ou droite déplace le focus sur la case d’option 3, la sélectionne et désactive la case d’option 2* |

### <a name="navigating-with-xbox-gamepad-and-remote-control"></a>Navigation avec une manette de jeu ou une télécommande Xbox

Si un utilisateur navigue entre des cases d’option à l’aide d’une télécommande ou d’une manette de jeu Xbox, le comportement « la sélection suit le focus » est désactivé et il doit appuyer sur le bouton « A » pour sélectionner la case d’option qui a le focus.

## <a name="accessibility-behavior"></a>Comportement en matière d’accessibilité

Le tableau suivant décrit la façon dont le Narrateur gère un groupe RadioButtons et ce qui est annoncé. Ce comportement dépend de la manière dont l’utilisateur a défini les préférences du Narrateur.

| Focus initial | Le focus se déplace sur l’élément sélectionné |
|:--|:--|
| La collection RadioButton « Nom de groupe » a le focus, et l’élément x (sur N éléments) est sélectionné. | Si le RadioButton « nom » est sélectionné, l’élément x a le focus. |
| La collection RadioButton « Nom de groupe » a le focus, et aucun élément n’est sélectionné.| Si le RadioButton « nom » n’est pas sélectionné, l’élément x a le focus. <br> Si l’utilisateur utilise les touches Maj-Flèche, aucune sélection ne suit le focus. |

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

## <a name="using-the-winui-radiobuttons-control"></a>Utilisation du contrôle RadioButtons de WinUI

Si vous utilisez [WinUI](https://github.com/microsoft/microsoft-ui-xaml), nous vous recommandons d’utiliser le contrôle [RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons).

Le contrôle RadioButtons est facile à configurer et à utiliser, et il garantit un comportement approprié et prévisible du clavier et du Narrateur.

Dans le code suivant, vous déclarez un contrôle RadioButtons de base avec trois options :

```xaml
<RadioButtons Header="App Mode" SelectedIndex="2">
    <RadioButton>Item 1</RadioButton>
    <RadioButton>Item 2</RadioButton>
    <RadioButton>Item 3</RadioButton>
</RadioButtons>
```
Le résultat est présenté dans l’image suivante :

![Cases d’option en deux groupes](images/default-radiobutton-group.png)

### <a name="defining-multiple-columns"></a>Définition de plusieurs colonnes

Vous pouvez déclarer un contrôle RadioButtons multicolonne en spécifiant la [propriété MaxColumns](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.maxcolumns).

```xaml
<muxc:RadioButtons Header="App Mode" MaxColumns="3">
    <x:String>Column 1</x:String>
    <x:String>Column 2</x:String>
    <x:String>Column 3</x:String>
    <x:String>Column 1</x:String>
    <x:String>Column 2</x:String>
    <x:String>Column 3</x:String>
</muxc:RadioButtons>
```

![Cases d’option dans deux groupes à trois colonnes](images/radiobutton-multi-columns.png)

### <a name="data-binding"></a>Liaison de données

Le contrôle RadioButtons prend en charge la liaison de données, qui utilise sa propriété [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.itemssource) comme illustré dans l’extrait de code suivant.

```xaml
<RadioButtons Header="App Mode" ItemsSource="{x:Bind radioButtonItems}" />
```

```c#
public sealed partial class MainPage : Page
{
    public class OptionDataModel
    {
        public string Label;
        public override string ToString()
        {
            return Label;
        }
    }

    List<OptionDataModel> radioButtonItems;

    public MainPage()
    {
        this.InitializeComponent();

        radioButtonItems = new List<OptionDataModel>();
        radioButtonItems.Add(new OptionDataModel() { label = "Item 1" });
        radioButtonItems.Add(new OptionDataModel() { label = "Item 2" });
        radioButtonItems.Add(new OptionDataModel() { label = "Item 3" });
    }
}
```

## <a name="create-your-own-radiobuttons-group"></a>Créer votre propre groupe RadioButtons

> [!Important]
> Nous vous recommandons d’utiliser le contrôle RadioButtons de WinUI pour regrouper des éléments RadioButton, sauf si vous utilisez une ancienne version de WinUI.

Les cases d’option s’utilisent en groupes. Vous pouvez regrouper les cases d’option de l’une des deux façons suivantes :

- Placez-les dans le même conteneur parent.
- Définissez la propriété [GroupName](/uwp/api/Windows.UI.Xaml.Controls.RadioButton.GroupName) de chaque case d’option sur la même valeur.

Dans cet exemple, le premier groupe de cases d’option est implicitement formé en raison de son appartenance au même panneau d’empilement. Le second groupe est divisé entre deux panneaux d’empilement. Ainsi, les cases d’option sont explicitement regroupées par GroupName.

```xaml
<StackPanel>
    <StackPanel>
        <TextBlock Text="Background" Style="{ThemeResource BaseTextBlockStyle}"/>
        <StackPanel Orientation="Horizontal">
            <RadioButton Content="Green" Tag="Green" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Yellow" Tag="Yellow" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Blue" Tag="Blue" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="White" Tag="White" Checked="BGRadioButton_Checked" IsChecked="True"/>
        </StackPanel>
    </StackPanel>
    <StackPanel>
        <TextBlock Text="BorderBrush" Style="{ThemeResource BaseTextBlockStyle}"/>
        <StackPanel Orientation="Horizontal">
            <StackPanel>
                <RadioButton Content="Green" GroupName="BorderBrush" Tag="Green" Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="Yellow" GroupName="BorderBrush" Tag="Yellow" Checked="BorderRadioButton_Checked" IsChecked="True"/>
            </StackPanel>
            <StackPanel>
                <RadioButton Content="Blue" GroupName="BorderBrush" Tag="Blue" Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="White" GroupName="BorderBrush" Tag="White"  Checked="BorderRadioButton_Checked"/>
            </StackPanel>
        </StackPanel>
    </StackPanel>
    <Border x:Name="BorderExample1" BorderThickness="10" BorderBrush="#FFFFD700" Background="#FFFFFFFF" Height="50" Margin="0,10,0,10"/>
</StackPanel>
```

```csharp
private void BGRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && BorderExample1 != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "Yellow":
                BorderExample1.Background = new SolidColorBrush(Colors.Yellow);
                break;
            case "Green":
                BorderExample1.Background = new SolidColorBrush(Colors.Green);
                break;
            case "Blue":
                BorderExample1.Background = new SolidColorBrush(Colors.Blue);
                break;
            case "White":
                BorderExample1.Background = new SolidColorBrush(Colors.White);
                break;
        }
    }
}

private void BorderRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && BorderExample1 != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "Yellow":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.Gold);
                break;
            case "Green":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.DarkGreen);
                break;
            case "Blue":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.DarkBlue);
                break;
            case "White":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.White);
                break;
        }
    }
}
```

Voici comment se présente ce groupe RadioButtons :

![Cases d’option en deux groupes](images/radio-button-groups.png)

## <a name="radio-button-states"></a>États des cases d’option

Une case d’option a deux états : sélectionnée ou désactivée. Quand une case d’option est sélectionnée, sa propriété [IsChecked](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked) a la valeur `true`. Quand une case d’option est désactivée, sa propriété IsChecked a la valeur `false`. Une case d’option peut être désactivée si l’utilisateur sélectionne une autre case d’option dans le même groupe, mais pas s’il la sélectionne de nouveau. Toutefois, vous pouvez désactiver une case d’option par programmation en définissant sa propriété IsChecked sur `false`.

## <a name="recommendations"></a>Recommandations

- Assurez-vous que le but et l’état actuel d’un ensemble de cases d’option sont explicites.
- Limitez l’étiquette de texte de la case d’option à une seule ligne.
- Si l’étiquette de texte est dynamique, songez au redimensionnement automatique du bouton et à ses conséquences sur les effets visuels environnants.
- Utilisez la police par défaut à moins que votre organisation ait passé d’autres consignes.
- Ne placez pas deux groupes RadioButtons côte à côte. Quand deux groupes RadioButtons sont adjacents, il peut être difficile pour les utilisateurs de déterminer quelles cases appartiennent à quel groupe.

### <a name="visuals-to-consider"></a>Visuels à prendre en considération

Les images suivantes montrent la meilleure organisation possible des cases d’option dans un groupe RadioButtons.

![Image montrant un ensemble de cases d’option organisées verticalement](images/radiobutton-layout.png)

![Image montrant les recommandations d’espacement des cases d’option](images/radiobutton-redline.png)

> [!NOTE]
> Si vous utilisez un contrôle RadioButtons de WinUI, l’espacement, les marges et l’orientation sont déjà optimisés.

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- Pour obtenir tous les contrôles XAML dans un format interactif, consultez [Exemple de Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery). 

## <a name="related-topics"></a>Rubriques connexes

### <a name="for-designers"></a>Pour les concepteurs

- [Boutons](buttons.md)
- [Boutons bascule](toggles.md)
- [Cases à cocher](checkbox.md)
- [Listes et zones de liste modifiable](lists.md)
- [Curseurs](slider.md)

### <a name="for-developers-xaml"></a>Pour les développeurs (XAML)

- [Classe RadioButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.radiobutton)
