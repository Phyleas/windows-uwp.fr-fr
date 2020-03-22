---
author: knicholasa
description: La profondeur Z, ou la profondeur relative, et l’ombre sont deux manières d’incorporer la profondeur dans votre application pour aider les utilisateurs à se concentrer naturellement et efficacement.
title: Profondeur Z et ombre pour les applications UWP
template: detail.hbs
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
pm-contact: chigy
ms.localizationpriority: medium
ms.openlocfilehash: 216974ba564a192f94473469f3a7a49191ef2192
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2020
ms.locfileid: "80081388"
---
# <a name="z-depth-and-shadow"></a>Profondeur Z et ombre

![Image gif montrant quatre rectangles gris empilés en diagonale, l’un au-dessus de l’autre. Le GIF est animé afin que les ombres apparaissent et disparaissent.](images/elevation-shadow/shadow.gif)

La création d’une hiérarchie visuelle d’éléments dans votre interface utilisateur facilite l’analyse de l’interface utilisateur et transmet ce qui est important pour se concentrer sur. L’élévation, qui consiste à mettre en avant les éléments sélectionnés de votre interface utilisateur, est souvent utilisée pour obtenir une telle hiérarchie dans le logiciel. Cet article explique comment créer une élévation dans une application UWP à l’aide de la profondeur z et de l’ombre.

Z-Depth est un terme utilisé parmi les créateurs d’applications en 3D pour indiquer la distance entre deux surfaces le long de l’axe z. Il montre comment fermer un objet dans la visionneuse. Considérez-le comme un concept similaire aux coordonnées x/y, mais dans la direction z.

## <a name="why-use-z-depth"></a>Pourquoi utiliser z-Depth ?

Dans le monde physique, nous avons tendance à nous concentrer sur les objets qui sont plus proches de nous. Nous pouvons également appliquer cet instinct spatial à l’interface utilisateur numérique. Par exemple, si vous rapprochez un élément de l’utilisateur, l’utilisateur instinctivement le focus sur l’élément. En déplaçant les éléments d’interface utilisateur plus près de l’axe z, vous pouvez établir une hiérarchie visuelle entre les objets, ce qui aide les utilisateurs à effectuer des tâches naturellement et efficacement dans votre application.

## <a name="what-is-shadow"></a>Qu’est-ce que le cliché instantané ?

Shadow est un moyen pour un utilisateur de percevoir l’élévation. Clair au-dessus d’un objet élevé crée une ombre sur l’aire ci-dessous. Plus l’objet est élevé, plus l’ombre est grande et lisse. Les objets élevés dans votre interface utilisateur n’ont pas besoin d’avoir des ombres, mais ils permettent de créer l’apparence de l’élévation.

Dans les applications UWP, les ombres doivent être utilisées dans un sens volontaire plutôt que esthétique. L’utilisation d’un trop grand nombre d’ombres diminue ou élimine la capacité de l’ombre à concentrer l’utilisateur.

Si vous utilisez des contrôles standard, les ombres ThemeShadow seront incorporées automatiquement dans votre interface utilisateur. Toutefois, vous pouvez inclure manuellement les ombres dans votre interface utilisateur à l’aide des API ThemeShadow ou DropShadow. 

## <a name="themeshadow"></a>ThemeShadow

Le type [ThemeShadow](/uwp/api/windows.ui.xaml.media.themeshadow) peut être appliqué à n’importe quel élément XAML pour dessiner des Shadows de manière appropriée en fonction des coordonnées x, y, z. ThemeShadow s’ajuste également automatiquement pour d’autres spécifications environnementales :

- S’adapte aux modifications de l’éclairage, du thème de l’utilisateur, de l’environnement de l’application et de l’interpréteur de commandes.
- Applique automatiquement des ombres aux éléments en fonction de leur profondeur z. 
- Maintient la synchronisation des éléments à mesure qu’ils se déplacent et modifient l’élévation.
- Garantit la cohérence des Shadows dans et entre les applications.

Voici comment ThemeShadow a été implémenté sur un MenuFlyout. MenuFlyout a une expérience intégrée dans laquelle l’aire principale est élevée à 32px et chaque menu en cascade supplémentaire est ouvert + 8px au-dessus du menu à partir duquel il s’ouvre.

![Capture d’écran de ThemeShadow appliquée à un MenuFlyout avec trois menus imbriqués ouverts. Le premier menu est élevé 32px et chaque menu suivant qui s’ouvre à partir du menu précédent est élevé 8px de manière à ce qu’il quitte une ombre distincte sur l’arrière-plan.](images/elevation-shadow/themeshadow-menuflyout.png)

### <a name="themeshadow-in-common-controls"></a>ThemeShadow dans les contrôles communs

Les contrôles communs suivants utilisent automatiquement ThemeShadow pour effectuer un cast des Shadows de 32px Depth, sauf indication contraire :

- [Menu contextuel](../controls-and-patterns/menus.md), [barre de commandes](../controls-and-patterns/app-bars.md), menu [volant de barre de commandes](../controls-and-patterns/command-bar-flyout.md), [BarreMenus](../controls-and-patterns/menus.md#create-a-menu-bar)
- [Boîtes de dialogue et lanceurs](../controls-and-patterns/dialogs.md) (boîte de dialogue sur 64px)
- [NavigationView](../controls-and-patterns/navigationview.md)
- [ComboBox](../controls-and-patterns/combo-box.md), [DropDownButton, SplitButton, ToggleSplitButton](../controls-and-patterns/buttons.md)
- [TeachingTip](../controls-and-patterns/dialogs-and-flyouts/teaching-tip.md)
- [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) 
- [Sélecteurs de calendrier/date/heure](../controls-and-patterns/date-and-time.md)
- [Info-bulle](../controls-and-patterns/tooltips.md) (16px)
- [Contrôle du transport multimédia](../controls-and-patterns/media-playback.md#media-transport-controls), [InkToolbar](../controls-and-patterns/inking-controls.md)
- [Animation connectée](../motion/connected-animation.md)

Remarque : les lanceurs appliquent uniquement ThemeShadow lorsqu’elles sont compilées par rapport à Windows 10 version 1903 ou à un SDK plus récent.

### <a name="themeshadow-in-popups"></a>ThemeShadow dans les fenêtres contextuelles

C’est souvent le cas lorsque l’interface utilisateur de votre application utilise une fenêtre contextuelle pour les scénarios où vous avez besoin d’attirer l’attention de l’utilisateur et de l’action rapide. Il s’agit de bons exemples qui illustrent l’utilisation de Shadow pour créer une hiérarchie dans l’interface utilisateur de votre application.

ThemeShadow convertit automatiquement les ombres quand il est appliqué à un élément XAML dans une [fenêtre contextuelle](/uwp/api/windows.ui.xaml.controls.primitives.popup). Il effectue un cast des ombres sur le contenu en arrière-plan de l’application derrière celui-ci, ainsi que d’autres menus contextuels ouverts en dessous.

Pour utiliser ThemeShadow avec des fenêtres contextuelles, utilisez la propriété `Shadow` pour appliquer un ThemeShadow à un élément XAML. Ensuite, élevez l’élément à partir d’autres éléments en arrière-plan, par exemple en utilisant le composant z de la propriété `Translation`.
Pour la plupart de l’interface utilisateur, l’élévation recommandée par défaut par rapport au contenu en arrière-plan de l’application est de 32 pixels effectifs.

Cet exemple montre un rectangle dans une fenêtre contextuelle qui convertit une ombre sur le contenu en arrière-plan de l’application et tous les autres menus contextuels qu’il contient :

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

![Popup rectangulaire unique avec une ombre.](images/elevation-shadow/PopupRectangle.png)

### <a name="disabling-default-themeshadow-on-custom-flyout-controls"></a>Désactivation de ThemeShadow par défaut sur les contrôles de menu volant personnalisés

Les contrôles basés sur le [menu volant](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.flyout), [DatePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.datepickerflyout), [MenuFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.menuflyout) ou [TimePickerFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.timepickerflyout) utilisent automatiquement ThemeShadow pour effectuer un cast d’une ombre.

Si l’ombre par défaut ne semble pas correcte sur le contenu de votre contrôle, vous pouvez la désactiver en définissant la propriété [IsDefaultShadowEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.flyoutpresenter.isdefaultshadowenabled) sur `false` sur le FlyoutPresenter associé :

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

En général, nous vous encourageons à réfléchir soigneusement à votre utilisation de l’ombre et à limiter son utilisation aux cas où elle introduit une hiérarchie visuelle significative. Toutefois, nous fournissons un moyen d’effectuer un cast d’une ombre à partir de n’importe quel élément d’interface utilisateur si vous avez des scénarios avancés qui l’exigent.

Pour effectuer un cast d’une ombre à partir d’un élément XAML qui n’est pas dans un popup, vous devez spécifier explicitement les autres éléments qui peuvent recevoir l’ombre dans la collection `ThemeShadow.Receivers`. Les destinataires ne peuvent pas être un ancêtre du casteur dans l’arborescence d’éléments visuels.

Cet exemple montre deux rectangles qui effectuent un cast des ombres dans une grille derrière :

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

![Deux rectangles turquoise les uns à côté des autres, avec des ombres.](images/elevation-shadow/SharedShadow.png)

### <a name="performance-best-practices-for-themeshadow"></a>Meilleures pratiques en matière de performances pour ThemeShadow

1. Le système définit une limite de 5 paires de transtypageur-récepteur et désactive l’ombre si cette opération est dépassée. Respectez la limite imposée par le système de 5 paires caster-Receiver.

2. Limitez le nombre d’éléments de récepteur personnalisés au minimum nécessaire.

3. Si plusieurs éléments Receiver sont à la même altitude, essayez de les combiner en ciblant un seul élément parent à la place.

4. Si plusieurs éléments effectuent un cast du même type d’ombre sur les mêmes éléments de récepteur, ajoutez l’ombre en tant que ressource partagée et réutilisez-la.

## <a name="drop-shadow"></a>Ombre portée

DropShadow ne répond pas automatiquement à son environnement et n’utilise pas de sources lumineuses. Pour obtenir des exemples d’implémentation, consultez la [classe DropShadow](https://docs.microsoft.com/uwp/api/windows.ui.composition.dropshadow).

## <a name="which-shadow-should-i-use"></a>Quelle ombre dois-je utiliser ?

| Propriété | ThemeShadow | DropShadow |
| - | - | - |
| **SDK min.** | Windows 10 version 1903 | 14393 |
| **Adaptabilité** | Oui | Non |
| **Personnalisation** | Non | Oui |
| **Source de lumière** | Automatique (global par défaut, mais peut être remplacé par l’application) | Aucune |
| **Pris en charge dans les environnements 3D** | Oui | Non |

- Gardez à l’esprit que l’objectif de l’ombre est de fournir une hiérarchie significative, et non un simple traitement visuel.
- En général, nous vous recommandons d’utiliser ThemeShadow, qui s’adapte automatiquement à son environnement.
- Pour les problèmes de performances, limitez le nombre d’ombres, utilisez un autre traitement visuel ou utilisez DropShadow.
- Si vous avez des scénarios plus avancés pour obtenir une hiérarchie visuelle, envisagez d’utiliser un autre traitement visuel (par exemple, la couleur). Si une ombre est nécessaire, utilisez DropShadow.
