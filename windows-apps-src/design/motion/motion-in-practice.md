---
Description: Découvrez comment les notions de base du mouvement Fluent sont rassemblées dans votre application.
title: Mouvement en pratique-animation dans les applications Windows
label: Motion in practice
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 45ab6c593b9e20f778e4b352a8b284cefe57c9a8
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970324"
---
# <a name="bringing-it-together"></a>Regroupement

Le minutage, l’accélération, la direction et la gravité fonctionnent ensemble pour former la Fondation du mouvement Fluent. Chaque doit être pris en compte dans le contexte des autres et appliqué de manière appropriée dans le contexte de votre application.

Voici trois façons d’appliquer les principes de base du mouvement Fluent dans votre application.

:::row:::
    :::column:::
**Animation implicite** L’interpolation et le minutage automatiques entre les valeurs d’un paramètre changent pour obtenir un mouvement Fluent très simple à l’aide des valeurs standardisées.
    :::column-end:::
    :::column:::
**Animation intégrée** Les composants système, tels que les contrôles communs et les mouvements partagés, sont « Fluent par défaut ». Les principes de base ont été appliqués de manière cohérente avec leur utilisation implicite.
    :::column-end:::
    :::column:::
Personnaliser l' **animation en suivant les recommandations d’aide** Il peut arriver que le système ne fournisse pas encore une solution de mouvement exacte pour votre scénario. Dans ce cas, utilisez les recommandations fondamentales de base comme point de départ pour vos expériences.
    :::column-end:::
:::row-end:::

**Exemple de transition**

![animation fonctionnelle](images/pageRefresh.gif)

:::row:::
    :::column:::
<b>Transférer le sens :</b><br>
Disparition en fondu : 150 m ; Accélération : le <b>sens de l'</b> accélération par défaut est le suivant :<br>
Glissez vers le haut 150 px : 300M ; Accélération : décélération par défaut
    :::column-end:::
    :::column:::
<b>Direction vers l’arrière :</b><br>
Glissez vers le 150 px : 150 m ; Accélération : direction de l’accélération par défaut vers <b>l’arrière dans :</b><br>
Fondu : 300 m ; Accélération : décélération par défaut
    :::column-end:::
:::row-end:::

**Exemple d’objet**

 ![mouvement de 300 m](images/control.gif)

:::row:::
    :::column:::
<b>Développer le sens :</b><br>
Croissance : 300 m ; Accélération : standard
    :::column-end:::
    :::column:::
<b>Contrat de direction :</b><br>
Croissance : 150 m ; Accélération : accélération par défaut
    :::column-end:::
:::row-end:::

## <a name="examples"></a>Exemples

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Si l’application de la <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong> est installée, cliquez ici pour <a href="xamlcontrolsgallery:/item/ImplicitTransition">ouvrir l’application et voir les transitions implicites en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="implicit-animations"></a>Animations implicites

> Les animations implicites nécessitent Windows 10, version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou version ultérieure.

Les animations implicites sont un moyen simple d’obtenir un mouvement Fluent en interpolant automatiquement entre les anciennes et les nouvelles valeurs pendant la modification d’un paramètre.

Vous pouvez animer de manière implicite les modifications apportées aux propriétés suivantes :

- [UIElement](/uwp/api/windows.ui.xaml.uielement)
  - **Opacity**
  - **Rotation**
  - **Mise à l’échelle**
  - **Traduction**

- [Border](/uwp/api/windows.ui.xaml.controls.border), [ContentPresenter](/uwp/api/windows.ui.xaml.controls.contentpresenter)ou [Panel](/uwp/api/windows.ui.xaml.controls.panel)
  - **Contexte**

Chaque propriété qui peut avoir des modifications qui peuvent être animées de manière implicite a une propriété de _transition_ correspondante. Pour animer la propriété, vous assignez un type de transition à la propriété de _transition_ correspondante. Ce tableau présente les propriétés de _transition_ et le type de transition à utiliser pour chacun d’eux.

| Propriété animée | Propriété transition | Type de transition implicite |
| -- | -- | -- |
| [UIElement. Opacity](/uwp/api/windows.ui.xaml.uielement.opacity) | [OpacityTransition](/uwp/api/windows.ui.xaml.uielement.opacitytransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Rotation](/uwp/api/windows.ui.xaml.uielement.rotation) | [RotationTransition](/uwp/api/windows.ui.xaml.uielement.rotationtransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Scale](/uwp/api/windows.ui.xaml.uielement.scale) | [ScaleTransition](/uwp/api/windows.ui.xaml.uielement.scaletransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.vector3transition) |
| [UIElement.Translation](/uwp/api/windows.ui.xaml.uielement.translation) | [TranslationTransition](/uwp/api/windows.ui.xaml.uielement.translationtransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.vector3transition) |
| [Border. Background](/uwp/api/windows.ui.xaml.controls.border.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.border.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [ContentPresenter. Background](/uwp/api/windows.ui.xaml.controls.contentpresenter.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.contentpresenter.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [Panel. Background](/uwp/api/windows.ui.xaml.controls.panel.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.panel.backgroundtransition)  | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |

Cet exemple montre comment utiliser la propriété Opacity et la transition pour faire apparaître un bouton en fondu lorsque le contrôle est activé et disparaît lorsqu’il est désactivé.

```xaml
<Button x:Name="SubmitButton"
        Content="Submit"
        Opacity="{x:Bind OpaqueIfEnabled(SubmitButton.IsEnabled), Mode=OneWay}">
    <Button.OpacityTransition>
        <ScalarTransition />
    </Button.OpacityTransition>
</Button>
```

```csharp
public double OpaqueIfEnabled(bool IsEnabled)
{
    return IsEnabled ? 1.0 : 0.2;
}
```

## <a name="related-articles"></a>Articles connexes

- [Vue d’ensemble du mouvement](index.md)
- [Minutage et accélération](timing-and-easing.md)
- [Direction et gravité](directionality-and-gravity.md)
