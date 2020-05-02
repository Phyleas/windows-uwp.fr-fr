---
title: Rayon d’angle
description: Découvrez-en plus sur les principes des angles arrondis, les approches de conception et les options de personnalisation.
ms.date: 10/08/2019
ms.topic: article
keywords: windows 10, uwp, rayon d’angle, arrondi
ms.openlocfilehash: 134a49ac57678eea0da718e93a14e3d0cf8896d5
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81001476"
---
# <a name="corner-radius"></a>Rayon d’angle

À compter de la version 2.2 de la [bibliothèque d’interface utilisateur Windows](/uwp/toolkits/winui/) (WinUI), de nombreux contrôles utilisent des angles arrondis comme style par défaut. Ces nouveaux styles visent, d’une part, à évoquer la chaleur et la confiance et, d’autre part, à rendre l’interface plus facile à assimiler visuellement pour les utilisateurs.

Voici deux contrôles Button, le premier sans angles arrondis et le second utilisant le nouveau style d’angles arrondis.

![Boutons sans et avec angles arrondis](images/rounded-corner/my-button.png)

Quand vous installez le package NuGet pour WinUI 2.2 ou ultérieur, de nouveaux styles par défaut sont installés pour les contrôles WinUI et les contrôles de plateforme. Ces nouveaux styles sont appliqués automatiquement quand vous utilisez WinUI 2.2 dans votre application ; aucune autre action n’est nécessaire. Toutefois, plus loin dans cet article, nous vous montrerons comment personnaliser les angles arrondis si vous en avez besoin.

> [!IMPORTANT]
> Certains contrôles sont disponibles à la fois dans la plateforme ([Windows.UI.Xaml.Controls](/uwp/api/windows.ui.xaml.controls)) et dans WinUI ([Microsoft.UI.Xaml.Controls](/uwp/api/microsoft.ui.xaml.controls?view=winui-2.2)) ; par exemple, **TreeView** ou **ColorPicker**. Quand vous utilisez WinUI dans votre application, vous devez utiliser la version WinUI du contrôle. L’arrondi des angles peut être appliqué de manière incohérente dans la version plateforme en cas d’utilisation avec WinUI.

> **API importantes** : [Control.CornerRadius, propriété](/uwp/api/windows.ui.xaml.controls.control.cornerradius)

## <a name="default-control-designs"></a>Conceptions de contrôle par défaut

Le style constitué d’angles arrondis est appliqué aux trois endroits suivants d’un contrôle : éléments rectangulaires, éléments d’un menu volant et éléments d’une barre.

### <a name="corners-of-rectangle-ui-elements"></a>Angles des éléments rectangulaires de l’interface utilisateur

- Les contrôles de base comme les boutons visibles à l’écran à tout moment font partie de ces éléments de l’interface utilisateur.
- Pour ceux-ci, nous utilisons un rayon par défaut de **2px**.

![Bouton avec angles arrondis mis en évidence](images/rounded-corner/button.png)

**Contrôles**

- AutoSuggestBox
- Bouton
  - ContentDialog, boutons
- CalendarDatePicker
- CheckBox
  - TreeView, cases à cocher à sélection multiple
- ComboBox
- DatePicker
- DropDownButton
- FlipView
- PasswordBox
- RichEditBox
- SplitButton
- TextBox
- TimePicker
- ToggleButton
- ToggleSplitButton

### <a name="corners-of-flyout-and-overlay-ui-elements"></a>Angles des éléments d’interface utilisateur dans les menus volants et les superpositions

- Il s’agit notamment d’éléments de l’interface utilisateur qui s’affichent à l’écran temporairement, comme MenuFlyout, ou d’éléments qui chevauchent une autre interface utilisateur, comme les onglets TabView.
- Pour ceux-ci, nous utilisons un rayon par défaut de **4px**.

![Exemple de menu volant](images/rounded-corner/flyout.png)

**Contrôles**

- CommandBarFlyout
- ContentDialog
- Menu volant
- et MenuFlyout
- TabView, onglets
- TeachingTip
- Info-bulle
- Partie de menu volant (à l’ouverture)
  - AutoSuggestBox
  - CalendarDatePicker
  - ComboBox
  - DatePicker
  - DropDownButton
  - MenuBar
  - SplitButton
  - TimePicker
  - ToggleSplitButton

### <a name="bar-elements"></a>Éléments de barre

- Ces éléments d’interface utilisateur sont en forme de barres ou de lignes ; par exemple, ProgressBar.
- Nous utilisons ici un rayon par défaut de **2px**.

![Exemple de barre de progression](images/rounded-corner/bars.png)

**Contrôles**

- NavigationView, indicateur de sélection
- Pivot, indicateur de sélection
- ProgressBar
- ScrollBar (quand `IndicatorMode=TouchIndicator`)
- Curseur
  - ColorPicker, curseur de couleur
  - MediaTransportControls, curseur de barre de recherche

## <a name="customization-options"></a>Options de personnalisation

Les valeurs des rayons d’angle par défaut que nous fournissons n’étant pas fixes, vous pouvez facilement modifier la quantité d’arrondi sur les angles de plusieurs manières. Vous pouvez par exemple utiliser deux ressources globales ou la propriété [CornerRadius](/uwp/api/windows.ui.xaml.controls.control.cornerradius) directement sur le contrôle en fonction du niveau de précision souhaité.

### <a name="when-not-to-round"></a>Quand ne pas arrondir

Dans certains cas, les angles d’un contrôle ne doivent pas être arrondis (ils ne le sont pas par défaut).

- Quand plusieurs éléments d’interface utilisateur hébergés à l’intérieur d’un conteneur se touchent l’un l’autre, comme les deux parties d’un SplitButton. Il ne doit pas y avoir d’espace entre les deux.

![SplitButton](images/rounded-corner/split-button-2.png)

- Quand un contrôle est hébergé dans un autre conteneur, comme la barre d’un ScrollBar et les boutons qui font partie du conteneur ScrollBar (qui fait également partie d’un ScrollViewer).

![ScrollBar](images/rounded-corner/scrollbar.png)

- Quand un élément d’interface utilisateur de menu volant est connecté à une interface utilisateur qui appelle le menu volant d’un côté.

![AutoSuggest](images/rounded-corner/autosuggest.png)

### <a name="keyboard-focus-rectangle-and-shadow"></a>Rectangle et ombre du focus clavier

Notre conception par défaut n’effectue aucune tâche spéciale pour arrondir les angles du rectangle de focus clavier ou de l’ombre du contrôle. L’utilisation d’une valeur de rayon d’angle plus élevée n’entraîne aucune rupture fonctionnelle. Sachez toutefois qu’une valeur plus élevée peut entraîner des anomalies visuelles.

Voici un exemple qui montre comment un rayon d’angle plus important peut rendre une ombre indésirable :

![ContentDialogShadow](images/rounded-corner/larger-corner-radius.png)

### <a name="rounded-corners-and-performance"></a>Angles arrondis et performances

Le rendu d’angles arrondis demande naturellement plus de puissance que le rendu d’angles carrés. Au moment de sélectionner les valeurs de rayon d’angle par défaut, nous avons pris en compte les principes de conception, mais nous avons également veillé à ce que les contrôles par défaut fonctionnent bien quand vous les utilisez dans vos applications.

Quand vous songez aux performances d’une application dans ce contexte, vous devez principalement tenir compte du temps de chargement des pages et du temps de lancement de l’application. Sachez que les coins arrondis sur une grande surface d’interface utilisateur ont un impact plus important sur les performances. Évitez de dessiner des angles arrondis dans l’interface utilisateur d’une application en plein écran. Ce problème se pose moins si l’interface utilisateur est affichée brièvement après le chargement d’une page, comme avec un ContentDialog.

### <a name="page-or-app-wide-cornerradius-changes"></a>Modifications de CornerRadius à l’échelle de la page ou de l’application

Deux ressources d’application contrôlent les rayons d’angle de tous les contrôles :

- `ControlCornerRadius` : la valeur par défaut est 2px.
- `OverlayCornerRadius` : la valeur par défaut est 4px.

Si vous remplacez la valeur de ces ressources dans une étendue quelconque, tous les contrôles de cette étendue seront affectés.

Cela signifie que si vous souhaitez modifier l’arrondi de tous les contrôles auxquels l’arrondi peut être appliqué, vous pouvez définir les deux ressources au niveau de l’application avec les nouvelles valeurs CornerRadius comme ceci :

```xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
            <ResourceDictionary>
                <CornerRadius x:Key="OverlayCornerRadius">0</CornerRadius>
                <CornerRadius x:Key="ControlCornerRadius">0</CornerRadius>
            </ResourceDictionary>
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

Sinon, si vous souhaitez modifier l’arrondi de tous les contrôles dans une étendue particulière, comme au niveau d’une page ou d’un conteneur, vous pouvez suivre une approche similaire :

```xaml
<Grid>
    <Grid.Resources>
        <CornerRadius x:Key="ControlCornerRadius">8</CornerRadius>
    </Grid.Resources>
    <Button Content="Button"/>
</Grid>
```

> [!NOTE]
> La ressource `OverlayCornerRadius` doit être définie au niveau de l’application pour prendre effet.
>
>En effet, les fenêtres contextuelles et les menus volants sont dynamiques et créés au niveau de l’élément racine de l’arborescence visuelle. Toutes les ressources utilisées doivent donc également y être définies. Dans le cas contraire, elles sont hors de l’étendue.

### <a name="per-control-cornerradius-changes"></a>Modifications de CornerRadius par contrôle

Vous pouvez modifier la propriété [CornerRadius](/uwp/api/windows.ui.xaml.controls.control.cornerradius) directement sur les contrôles si vous souhaitez changer l’arrondi d’un nombre spécifique de contrôles.

|Par défaut | Propriété modifiée |
|:-- |:-- |
|![DefaultCheckBox](images/rounded-corner/default-checkbox.png)| ![CustomCheckBox](images/rounded-corner/custom-checkbox.png)|
|`<CheckBox Content="Checkbox"/>` | `<CheckBox Content="Checkbox" CornerRadius="5"/> ` |

Tous les angles des contrôles ne répondent pas à la modification de leur propriété `CornerRadius`. Pour faire en sorte que le contrôle dont vous souhaitez arrondir les angles répond comme prévu à la propriété `CornerRadius`, commencez par vérifier que les ressources globales `ControlCornerRadius` ou `OverlayCornerRadius` affectent le contrôle en question. Si ce n’est pas le cas, vérifiez que le contrôle que vous souhaitez arrondir a bien des angles. La plupart de nos contrôles n’affichent pas les bords réels et ne peuvent donc pas utiliser correctement la propriété `CornerRadius`.

### <a name="basing-custom-styles-on-winui"></a>Baser des styles personnalisés sur WinUI

Vous pouvez baser vos styles personnalisés sur les styles d’angles arrondis de WinUI en spécifiant l’attribut `BasedOn` approprié dans votre style. Par exemple, pour créer un style de bouton personnalisé basé sur le style de bouton WinUI, procédez comme suit :

```xaml
<Style x:Key="MyCustomButtonStyle" BasedOn="{StaticResource DefaultButtonStyle}">
   ...
</Style>
```

En général, les styles de contrôle WinUI suivent une convention d’affectation de noms cohérente : « DefaultXYZStyle », où « XYZ » est le nom du contrôle. Pour une référence complète, vous pouvez parcourir les fichiers XAML dans le référentiel WinUI.
