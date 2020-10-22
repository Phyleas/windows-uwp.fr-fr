---
description: La profondeur z, ou profondeur relative, et l’ombre sont deux façons d’incorporer la profondeur dans votre application pour aider les utilisateurs à se concentrer de manière naturelle et efficace.
title: Profondeur z et ombre pour les applications Windows
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
pm-contact: chigy
ms.localizationpriority: medium
ms.openlocfilehash: 72cffc86d71b34de2c4c9292221889ee4f0bc1d5
ms.sourcegitcommit: 5684340ad29a81939c6a93017b5a39ccbe1f6040
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/19/2020
ms.locfileid: "92184211"
---
# <a name="z-depth-and-shadow"></a>Profondeur Z et ombre

![Image gif montrant quatre rectangles gris empilés les uns sur des autres en diagonale. L’image gif est animée pour faire apparaître et disparaître les ombres.](images/elevation-shadow/shadow.gif)

la création d’une hiérarchie visuelle d’éléments dans votre interface utilisateur facilite l’analyse de celle-ci et comporte ce sur quoi il est important de se concentrer. L’élévation, qui consiste à mettre en avant des éléments spécifiques de votre interface utilisateur, est souvent utilisée pour obtenir une telle hiérarchie dans le logiciel. Cet article explique comment créer un effet d’élévation dans une application Windows à l’aide de la profondeur z et d’ombres.

La profondeur z est un terme utilisé par les créateurs d’applications 3D pour indiquer la distance entre deux surfaces le long de l’axe z. Elle illustre la proximité d’un objet avec le viewer. Le concept est similaire à celui des coordonnées x/y, mais dans la direction de l’axe z.

## <a name="why-use-z-depth"></a>Pourquoi utiliser la profondeur z ?

Dans le monde physique, nous avons tendance à nous concentrer sur les objets qui sont proches de nous. Cet instinct spatial peut être appliqué à l’interface utilisateur numérique. Par exemple, si vous rapprochez un élément de l’utilisateur, ce dernier porte instinctivement son attention sur cet élément. En déplaçant les éléments de l’interface utilisateur sur l’axe z, vous pouvez établir une hiérarchie visuelle entre les objets pour aider les utilisateurs à effectuer des tâches de manière naturelle et efficace dans votre application.

## <a name="what-is-shadow"></a>Qu’est-ce qu’une ombre ?

Une ombre est un moyen pour un utilisateur de percevoir l’élévation. La lumière au-dessus d’un objet élevé crée une ombre sur la surface en dessous de cet objet. Plus l’objet est haut, plus l’ombre est grande et douce. Vous n’êtes pas obligé d’ajouter des ombres à des objets élevés dans votre interface utilisateur, mais elles contribuent à créer une apparence d’élévation.

Dans les applications Windows, les ombres doivent être utilisées à des fins utiles et non esthétiques. L’utilisation d’un trop grand nombre d’ombres diminue voire élimine leur capacité à attirer l’attention de l’utilisateur.

Si vous utilisez des contrôles standard, les ombres ThemeShadow sont incorporées automatiquement à votre interface utilisateur. Toutefois, vous pouvez ajouter manuellement des ombres à votre interface utilisateur à l’aide des API ThemeShadow ou DropShadow. 

## <a name="themeshadow"></a>ThemeShadow

Le type [ThemeShadow](/uwp/api/windows.ui.xaml.media.themeshadow) peut être appliqué à tout élément XAML pour dessiner des ombres appropriées selon les coordonnées x, y, z. Par ailleurs, ThemeShadow s’ajuste automatiquement en fonction d’autres spécifications environnementales :

- S’adapte aux changements liés à l’éclairage, au thème de l’utilisateur, à l’environnement d’application et au shell.
- Applique automatiquement des ombres aux éléments en fonction de leur profondeur z. 
- Assure la synchronisation des éléments à mesure qu’ils se déplacent et changent d’élévation.
- Garantit des ombres cohérentes dans et entre les applications.

Voici comment ThemeShadow a été implémenté sur un MenuFlyout. MenuFlyout propose une expérience intégrée où la surface principale est élevée de 32px et où chaque menu en cascade supplémentaire se trouve 8px au-dessus du menu précédent.

![Capture d’écran de ThemeShadow appliqué à un MenuFlyout avec trois menus imbriqués ouverts. Le premier menu est élevé de 32px et chaque menu ouvert à partir du menu précédent est élevé de 8px, ce qui projette une ombre distincte sur l’arrière-plan.](images/elevation-shadow/themeshadow-menuflyout.png)

### <a name="themeshadow-in-common-controls"></a>ThemeShadow dans des contrôles courants

Les contrôles communs suivants utilisent automatiquement ThemeShadow pour projeter des ombres à partir d’une profondeur de 32px, sauf indication contraire :

- [Menu contextuel](../controls-and-patterns/menus.md), [barre de commandes](../controls-and-patterns/app-bars.md), [menu volant de barre de commandes](../controls-and-patterns/command-bar-flyout.md), [MenuBar](../controls-and-patterns/menus.md#create-a-menu-bar)
- [Boîtes de dialogue et menus volants](../controls-and-patterns/dialogs-and-flyouts/index.md) (boîte de dialogue à 64px)
- [NavigationView](../controls-and-patterns/navigationview.md)
- [ComboBox](../controls-and-patterns/combo-box.md), [DropDownButton, SplitButton, ToggleSplitButton](../controls-and-patterns/buttons.md)
- [TeachingTip](../controls-and-patterns/dialogs-and-flyouts/teaching-tip.md)
- [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) 
- [Sélecteurs de calendrier, de date et d’heure](../controls-and-patterns/date-and-time.md)
- [Info-bulle](../controls-and-patterns/tooltips.md) (16px)
- [Contrôle de transport multimédia](../controls-and-patterns/media-playback.md#media-transport-controls), [InkToolbar](../controls-and-patterns/inking-controls.md)
- [Animation connectée](../motion/connected-animation.md)

Remarque : Les menus volants appliquent uniquement ThemeShadow s’ils sont compilés sur Windows 10 version 1903 ou un SDK plus récent.

### <a name="themeshadow-in-popups"></a>ThemeShadow dans les fenêtres contextuelles

L’interface utilisateur de votre application utilise sans doute des fenêtres contextuelles pour attirer l’attention de l’utilisateur et exiger de lui une action rapide. Dans ce scénario, il est recommandé d’utiliser des ombres pour créer une hiérarchie dans l’interface utilisateur de votre application.

ThemeShadow projette automatiquement des ombres lorsqu’il est appliqué à un élément XAML dans un [Popup](/uwp/api/windows.ui.xaml.controls.primitives.popup). Il projette des ombres sur le contenu en arrière-plan de l’application (derrière l’élément) et sur toute autre fenêtre contextuelle ouverte en dessous de l’élément.

Pour utiliser ThemeShadow avec des fenêtres contextuelles, utilisez la propriété `Shadow` pour appliquer ThemeShadow à un élément XAML. Élevez ensuite l’élément par rapport à d’autres éléments situés derrière, par exemple en utilisant le composant z de la propriété `Translation`.
Pour la plupart des interfaces utilisateur avec des fenêtres contextuelles, l’élévation par défaut recommandée par rapport au contenu en arrière-plan de l’application est de 32 pixels effectifs.

Cet exemple montre un Rectangle dans un Popup qui projette une ombre sur le contenu en arrière-plan de l’application et toute autre fenêtre contextuelle derrière lui :

```xaml
<Popup>
    <Rectangle x:Name="PopupRectangle" Fill="Lavender" Height="48" Width="96">
        <Rectangle.Shadow>
            <ThemeShadow />
        </Rectangle.Shadow>
    </Rectangle>
</Popup>
```

```csharp
// Elevate the rectangle by 32px
PopupRectangle.Translation += new Vector3(0, 0, 32);
```

![Fenêtre contextuelle rectangulaire unique avec une ombre.](images/elevation-shadow/PopupRectangle.png)

### <a name="disabling-default-themeshadow-on-custom-flyout-controls"></a>Désactivation de ThemeShadow par défaut sur des contrôles Flyout personnalisés

Les contrôles basés sur [Flyout](/uwp/api/Windows.UI.Xaml.Controls.flyout), [DatePickerFlyout](/uwp/api/windows.ui.xaml.controls.datepickerflyout), [MenuFlyout](/uwp/api/Windows.UI.Xaml.Controls.menuflyout) ou [TimePickerFlyout](/uwp/api/windows.ui.xaml.controls.timepickerflyout) utilisent automatiquement ThemeShadow pour projeter une ombre.

Si l’ombre par défaut ne semble pas correcte sur le contenu de votre contrôle, vous pouvez la désactiver en affectant à la propriété [IsDefaultShadowEnabled](/uwp/api/windows.ui.xaml.controls.flyoutpresenter.isdefaultshadowenabled) la valeur `false` sur le FlyoutPresenter associé :

```xaml
<Flyout>
    <Flyout.FlyoutPresenterStyle>
        <Style TargetType="FlyoutPresenter">
            <Setter Property="IsDefaultShadowEnabled" Value="False" />
        </Style>
    </Flyout.FlyoutPresenterStyle>
</Flyout>
```

### <a name="themeshadow-in-other-elements"></a>ThemeShadow dans d’autres éléments

En général, nous vous encourageons à bien réfléchir avant d’utiliser des ombres et à limiter leur utilisation aux cas où elles introduisent une hiérarchie visuelle utile. Cependant, nous fournissons un moyen de projeter une ombre à partir de n’importe quel élément de l’interface utilisateur si cela est nécessaire dans des scénarios avancés.

Pour projeter une ombre à partir d’un élément XAML qui n’est pas dans un Popup, vous devez spécifier explicitement les autres éléments susceptibles de recevoir l’ombre dans la collection `ThemeShadow.Receivers`. Un receveur ne peut pas être un ancêtre du projeteur d’ombre dans l’arborescence d’éléments visuels.

Cet exemple montre deux rectangles projetant des ombres sur un Grid derrière eux :

```xaml
<Grid>
    <Grid.Resources>
        <ThemeShadow x:Name="SharedShadow" />
    </Grid.Resources>

    <Grid x:Name="BackgroundGrid" Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" />

    <Rectangle x:Name="Rectangle1" Height="100" Width="100" Fill="Turquoise" Shadow="{StaticResource SharedShadow}" />

    <Rectangle x:Name="Rectangle2" Height="100" Width="100" Fill="Turquoise" Shadow="{StaticResource SharedShadow}" />
</Grid>
```

```csharp
/// Add BackgroundGrid as a shadow receiver and elevate the casting buttons above it
SharedShadow.Receivers.Add(BackgroundGrid);

Rectangle1.Translation += new Vector3(0, 0, 16);
Rectangle2.Translation += new Vector3(120, 0, 32);
```

![Deux rectangles turquoise côte à côte avec des ombres.](images/elevation-shadow/SharedShadow.png)

### <a name="performance-best-practices-for-themeshadow"></a>Bonnes pratiques liées aux performances de ThemeShadow

1. Le système fixe une limite de 5 paires projeteur-récepteur et désactive l’ombre si cette limite est dépassée. Respectez la limite imposée par le système de 5 paires projeteur-récepteur.

2. Limitez le nombre d’éléments récepteurs personnalisés au minimum nécessaire.

3. Si plusieurs éléments récepteurs se trouvent à la même élévation, essayez de les combiner en ciblant un seul élément parent à la place.

4. Si plusieurs éléments projettent le même type d’ombre sur les mêmes éléments récepteurs, ajoutez l’ombre en tant que ressource partagée et réutilisez-la.

## <a name="drop-shadow"></a>Ombre portée

DropShadow ne répond pas automatiquement à son environnement et n’utilise aucune source de lumière. Pour obtenir des exemples d’implémentation, consultez la [classe DropShadow](/uwp/api/windows.ui.composition.dropshadow).

## <a name="which-shadow-should-i-use"></a>Quelle ombre dois-je utiliser ?

| Propriété | ThemeShadow | DropShadow |
| - | - | - |
| **SDK min.** | Windows 10 version 1903 | 14393 |
| **Adaptabilité** | Oui | Non |
| **Personnalisation** | Non | Oui |
| **Source de lumière** | Automatique (globale par défaut, mais peut être remplacée par l’application) | Aucune |
| **Prise en charge dans les environnements 3D** | Oui | Non |

- Gardez à l’esprit que l’objectif d’une ombre est de fournir une hiérarchie utile, pas un simple traitement visuel.
- Nous recommandons généralement d’utiliser ThemeShadow, car il s’adapte automatiquement à son environnement.
- Si les performances vous préoccupent, limitez le nombre d’ombres, utilisez un autre traitement visuel ou utilisez DropShadow.
- Si vous souhaitez établir une hiérarchie visuelle dans des scénarios plus avancés, envisagez d’utiliser un autre traitement visuel (par exemple, des couleurs). Si des ombres sont nécessaires, utilisez DropShadow.
