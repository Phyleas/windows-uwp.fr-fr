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
ms.openlocfilehash: 6705c314d9a70f8b6282841a7f8b1df76c6ef880
ms.sourcegitcommit: 6dd6d61c912daab2cc4defe5ba0cf717339f7765
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84978401"
---
# <a name="radio-buttons"></a>Cases d’option

Les cases d’option permettent aux utilisateurs de sélectionner une option dans une collection constituée d’au moins deux options apparentées, mais qui s’excluent mutuellement. Chaque option est représentée par une case d’option.

Dans l’état par défaut, aucune case d’option n’est sélectionnée dans un groupe. Cependant, du moment qu’une case d’option est sélectionnée par un utilisateur, l’état désélectionné du groupe ne peut pas être rétabli par l’utilisateur.

Le comportement singulier d’un groupe de cases d’option le distingue des [cases à cocher](checkbox.md), qui prennent en charge la sélection multiple et la désélection.

![Cases d’option](images/controls/radio-button.png)

**Obtenir la bibliothèque d’interface utilisateur Windows**

|  |  |
| - | - |
| ![Logo WinUI](images/winui-logo-64x64.png) | Le contrôle **RadioButtons** est inclus dans la bibliothèque d’IU Windows, package NuGet qui contient les nouveaux contrôles et fonctionnalités d’interface utilisateur pour les applications Windows. Pour plus d’informations, notamment des instructions d’installation, consultez la [bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

> **API de la bibliothèque d’interface utilisateur Windows :** [RadioButtons, classe](/uwp/api/microsoft.ui.xaml.controls.radiobuttons), [SelectionChanged, événement](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged), [SelectedItem, propriété](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem), [SelectedIndex, propriété](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex)
>
> **API de plateforme :** [classe RadioButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RadioButton), [événement Checked](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.Checked), [propriété IsChecked](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked)

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

Utilisez les cases d’option pour permettre aux utilisateurs d’effectuer une sélection parmi au moins deux options qui s’excluent mutuellement.

![Groupe de cases d’option](images/radiobutton_basic.png)

Utilisez les cases d’option lorsque les utilisateurs ont besoin de voir toutes les options pour effectuer une sélection. Sachant que les cases d’option mettent en évidence toutes les options de la même manière, elles peuvent donner plus d’importance à une option qu’elle n’en a vraiment ou que vous voulez lui en donner. À moins que les options méritent toutes la même attention de la part de l’utilisateur, envisagez d’utiliser d’autres contrôles. Par exemple, si l’option par défaut est recommandée pour la plupart des utilisateurs et dans la plupart des situations, utilisez plutôt une [zone de liste modifiable](combo-box.md).

![Liste déroulante utilisée pour mettre en évidence une option par défaut](images/combo_box_collapsed.png)

S’il n’y a que deux options qui s’excluent mutuellement, combinez-les dans une [case à cocher](checkbox.md) ou un [bouton bascule](toggles.md) unique. Par exemple, utilisez une case à cocher pour « J’accepte », plutôt que deux cases d’option pour « J’accepte » et « Je n’accepte pas ».

![Une case à cocher est une bonne alternative pour présenter un choix binaire](images/radiobutton_vs_checkbox.png)

Si l’utilisateur peut sélectionner plusieurs options, utilisez une [case à cocher](checkbox.md).

![Les cases à cocher prennent en charge la sélection multiple](images/checkbox2.png)

Lorsque les options sont des numéros incrémentés de manière fixe (10, 20, 30), utilisez un contrôle [curseur](slider.md).

![Curseur utilisé pour sélectionner des valeurs par paliers](images/controls/slider.png)

S’il y a plus de huit options, utilisez une [zone de liste modifiable ou une zone de liste](combo-box.md).

![Zone de liste utilisée pour présenter plusieurs options](images/combo_box_scroll.png)

> [!NOTE]
> Si les options disponibles dépendent du contexte actuel de l’application ou si elles peuvent changer de façon dynamique, utilisez une [zone de liste](combo-box.md#list-boxes) à sélection unique.

## <a name="radiobuttons-behavior"></a>Comportement de RadioButtons

L’accès clavier et le comportement de navigation ont été optimisés dans les groupes [RadioButton](/uwp/api/windows.ui.xaml.controls.radiobutton?view=winrt-19041) pour favoriser l’accessibilité et permettre aux utilisateurs avec pouvoir de naviguer dans la liste d’options avec plus de facilité et de rapidité à l’aide du clavier.

En plus des raccourcis clavier et des améliorations sur le plan de l’accessibilité, la disposition visuelle par défaut des cases d’option individuelles au sein d’un groupe RadioButton a aussi été optimisée via les paramètres automatisés d’orientation d’espacement et de marges. Il n’est donc plus nécessaire de spécifier ces propriétés, comme pouvait l’exiger un contrôle de regroupement plus primitif comme [StackPanel](../layout/layout-panels.md#stackpanel) ou [Grid](../layout/layout-panels.md#grid).

### <a name="navigating-a-radiobuttons-group"></a>Navigation dans un groupe RadioButtons

Le contrôle RadioButtons prend en charge deux états :

- Liste de contrôles RadioButton dans laquelle aucun contrôle n’est sélectionné/activé
- Liste de contrôles RadioButton dans laquelle un contrôle est déjà sélectionné/activé

Les deux sections suivantes se penchent sur les deux comportements de focus pour les cases d’option.

#### <a name="item-already-selected"></a>Élément déjà sélectionné

Quand une case d’option est sélectionnée et que l’utilisateur accède à la liste à l’aide de la touche de tabulation, la case d’option sélectionnée obtient le focus.

|Liste sans le focus de tabulation | Liste avec le focus de tabulation initial |
|:--:|:--:|
| ![Liste sans le focus de tabulation](images/radiobutton-selected-item-no-tab-focus.png) | ![Liste avec le focus de tabulation initial](images/radiobutton-selected-item-tab-focus.png)|

#### <a name="no-item-selected"></a>Aucun élément sélectionné

Quand aucune case d’option n’est sélectionnée, la première case d’option de la liste obtient le focus.

> [!NOTE]
> L’élément qui reçoit le focus de tabulation à partir de la navigation initiale avec la touche de tabulation n’est pas sélectionné/activé.

|Liste sans le focus de tabulation | Liste avec le focus de tabulation initial|
|:--:|:--:|
| ![Liste sans le focus de tabulation](images/radiobutton-no-selected-item-no-tab-focus.png) | ![Liste avec le focus de tabulation initial](images/radiobutton-no-selected-item-tab-focus.png)|

### <a name="keyboard-navigation"></a>Navigation au clavier

Quand vous avez une seule ligne ou colonne de cases d’option et qu’un élément a déjà reçu le focus de tabulation, les touches de direction permettent une « navigation interne » entre les éléments du contrôle RadioButtons. Pour plus d’informations sur les comportements de navigation avec le clavier, consultez [Interactions avec le clavier – Navigation](../input/keyboard-interactions.md#navigation).

Pour un contrôle RadioButtons, quand la liste d’options est organisée verticalement (exclusivement), les touches de direction haut/bas permettent de naviguer entre les éléments, tandis que les touches de direction gauche/droite sont inopérantes. En revanche, dans une liste organisée horizontalement (exclusivement), les touches de direction gauche/droite et haut/bas permettent de naviguer entre les éléments de la même façon.

![Exemple de navigation au clavier dans un groupe RadioButton à une seule colonne/ligne](images/radiobutton-keyboard-navigation-single-column-row.png)<br/>
*Exemple de navigation au clavier dans un groupe RadioButton à une seule colonne/ligne*

#### <a name="navigating-within-multi-columnrow-layouts"></a>Navigation dans les dispositions à plusieurs colonnes/lignes

Dans l’ordre « column-major » (où les éléments sont renseignés de haut en bas et de gauche à droite), quand le focus est sur le dernier élément d’une colonne et que la touche de direction bas est enfoncée, le focus se déplace sur le premier élément de la colonne suivante. Le même comportement est observé dans le cas inverse : quand le focus est défini sur le premier élément d’une colonne et que la touche de direction haut est enfoncée, le focus se déplace sur le dernier élément de la colonne précédente.

![Exemple de navigation au clavier dans un groupe RadioButton à plusieurs colonnes/lignes](images/radiobutton-keyboard-navigation-multi-column-row.png)

Dans un ordre « row-major » (où les éléments sont renseignés de gauche à droite et de haut en bas), quand le focus est sur le dernier élément d’une ligne et que la touche de direction droite est enfoncée, le focus se déplace sur le premier élément de la ligne suivante. Le même comportement est observé dans le cas inverse : quand le focus est défini sur le premier élément d’une ligne et que la touche de direction gauche est enfoncée, le focus se déplace sur le dernier élément de la ligne précédente.

##### <a name="wrapping"></a>Renvoi à la ligne

Le groupe RadioButtons n’est pas renvoyé à la ligne. En effet, dans le cas où un lecteur d’écran est utilisé, il n’y a plus ce sentiment de limite et d’indication claire de début et de fin, ce qui rend difficile la navigation dans la liste pour les utilisateurs malvoyants. Le contrôle RadioButtons ne prend pas non plus en charge l’énumération, car elle est censée contenir un nombre raisonnable d’éléments (voir [Est-ce le bon contrôle ?](#is-this-the-right-control)).

## <a name="selection-follows-focus"></a>La sélection suit le focus

Quand vous vous servez du clavier pour naviguer entre les éléments d’une liste RadioButtons (où un élément est déjà sélectionné), à mesure que le focus passe d’un élément à l’autre, l’élément qui prend le focus est sélectionné/activé et l’élément qui le perd est désélectionné/désactivé.

|Avant la navigation au clavier | Après la navigation au clavier|
|:--|:--|
| ![Exemple de focus et de sélection avant la navigation au clavier](images/radiobutton-two-selected-before-keyboard-navigation.png)</br>*Exemple de focus et de sélection avant la navigation au clavier* | ![Exemple de focus et de sélection après la navigation au clavier](images/radiobutton-three-selected-after-keyboard-navigation.png)<br/>*Exemple de focus et de sélection après la navigation au clavier où la touche de direction bas ou droite déplace le focus sur le RadioButton « 3 », sélectionne « 3 » et désélectionne « 2 ».*

### <a name="navigating-with-xbox-gamepad-and-remote-control"></a>Navigation avec une manette de jeu ou une télécommande Xbox

Si vous utilisez une manette de jeu ou une télécommande Xbox pour naviguer dans un contrôle RadioButtons, le comportement « la sélection suit le focus » est désactivé et le bouton « A » doit être enfoncé pour sélectionner la case d’option qui a le focus.

## <a name="accessibility-behavior"></a>Comportement en matière d’accessibilité

Le tableau suivant détaille la façon dont le Narrateur gère un groupe de cases d’option et ce qui est annoncé (en fonction des préférences du Narrateur qu sont définies par l’utilisateur).

| Focus initial | Le focus se déplace sur l’élément sélectionné |
|:--|:--|
| Collection RadioButton « Nom de groupe », x sur N sélectionné | « Nom » de RadioButton sélectionné, x sur N |
|Collection RadioButton « nom de groupe », aucun sélectionné| « Nom » de RadioButton non sélectionné, x sur N <br> *(Dans le cas d’une navigation avec les touches Maj-Flèche, ce qui indique que la sélection ne suit pas le focus)* |

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/RadioButton">ouvrir l’application et voir l'objet RadioButton en action</a>.</p>
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

Ici, nous déclarons un contrôle RadioButtons de base avec trois options.

```xaml
<RadioButtons Header="App Mode" SelectedIndex="2">
    <RadioButton>Item 1</RadioButton>
    <RadioButton>Item 2</RadioButton>
    <RadioButton>Item 3</RadioButton>
</RadioButtons>
```

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

![Cases d’option dans des groupes à deux colonnes](images/radiobutton-multi-columns.png)

### <a name="data-binding"></a>Liaison de données

Le contrôle RadioButtons prend en charge la liaison de données en utilisant sa propriété [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.itemssource), comme illustré dans l’extrait de code suivant.

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

## <a name="create-your-own-radio-button-group"></a>Créer votre propre groupe de cases d’option

> [!Important]
> Nous vous recommandons d’utiliser le contrôle RadioButtons de WinUI pour regrouper des éléments RadioButton (sauf si vous utilisez une ancienne version de WinUI).

Les cases d’option s’utilisent en groupes. Les 2 méthodes permettant de grouper des contrôles de cases d’option sont les suivantes :

- Placez-les dans le même conteneur parent.
- Attribuez la même valeur à la propriété [GroupName](/uwp/api/Windows.UI.Xaml.Controls.RadioButton.GroupName) de chaque case d’option.

Dans cet exemple, le premier groupe de cases d’option est implicitement formé en raison de son appartenance au même panneau d’empilement. Le second groupe est divisé entre 2 panneaux d’empilement, si bien que les cases d’option sont explicitement regroupées par GroupName.

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

Voici comment se présente ce groupe de cases d’option :

![Cases d’option en deux groupes](images/radio-button-groups.png)

## <a name="radio-button-states"></a>États des cases d’option

Les cases d’option peuvent avoir l’un des deux états suivants : activé ou désactivé. Lorsqu’une case d’option est sélectionnée, sa propriété [IsChecked](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked) vaut **true**. Quand une case d’option est désactivée, sa propriété **IsChecked** a la valeur **false**. Une case d’option peut être désactivée en cliquant sur une autre case d’option du même groupe, mais elle ne peut pas l’être en cliquant à nouveau dessus. Toutefois, vous pouvez désactiver une case d’option par programmation en définissant sa propriété IsChecked sur **false**.

## <a name="recommendations"></a>Recommandations

- Assurez-vous que le but et l’état actuel d’un ensemble de cases d’option sont clairs.
- Limitez le contenu du texte de la case d’option à une seule ligne.
- Si le contenu du texte est dynamique, songez au redimensionnement du bouton et à ses conséquences sur les effets visuels environnant.
- Utilisez la police par défaut à moins que votre organisation ait passé d’autres consignes.
- Ne placez pas deux groupes de cases d’option côte à côte. Lorsque deux groupes de cases d’option sont adjacents, il est difficile de déterminer quelles cases appartiennent à quel groupe.

### <a name="visuals-to-consider"></a>Visuels à prendre en considération

Les images suivantes montrent la meilleure disposition possible des cases d’option dans un groupe RadioButton.

![Ensemble de cases d’option](images/radiobutton-layout.png)

![recommandations d'espacement en matière de cases d’option](images/radiobutton-redline.png)

> [!NOTE]
> Si vous utilisez un contrôle RadioButtons de WinUI, l’espacement, les marges et l’orientation sont déjà optimisés.

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemple de Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) : tous les contrôles XAML dans un format interactif.

## <a name="related-topics"></a>Rubriques connexes

### <a name="for-designers"></a>Pour les concepteurs

- [Boutons](buttons.md)
- [Boutons bascule](toggles.md)
- [Cases à cocher](checkbox.md)
- [Listes et zones de liste modifiable](lists.md)
- [Curseurs](slider.md)

### <a name="for-developers-xaml"></a>Pour les développeurs (XAML)

- [Classe RadioButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.radiobutton)
