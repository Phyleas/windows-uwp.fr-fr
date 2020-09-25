---
description: Découvrez l’importance du minutage et de l’accélération pour rendre le mouvement naturel pour les objets entrants, sortants ou déplacés dans l’interface utilisateur.
title: Minutage et accélération
label: Timing and easing
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: fe776361276341e368db1fbdf8e332a1e5dc70b5
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220192"
---
# <a name="timing-and-easing"></a>Minutage et accélération

Alors que motion est basé dans le monde réel, nous sommes également un support numérique, qui est fourni avec une attente de vitesse et de performances.

## <a name="examples"></a>Exemples

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Si l’application de la <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong> est installée, cliquez ici pour <a href="xamlcontrolsgallery:/item/EasingFunction">ouvrir l’application et voir fonctions d’accélération en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-fluent-motion-uses-time"></a>Comment le mouvement Fluent utilise l’heure

Le minutage est un élément important pour rendre le mouvement naturel pour des objets entrants, sortants ou déplacés dans l’interface utilisateur.

1. Les objets ou les scènes qui entrent dans la vue sont rapides, mais célébré. La durée de ces animations est généralement plus longue que celle des sorties pour permettre la création hiérarchique d’une scène.
1. Les objets ou scènes qui quittent la vue sont très rapides. L’utilisateur doit être en mesure de comprendre où l’interface utilisateur s’est déroulée. Toutefois, une fois que l’interface utilisateur est fermée, elle ne doit pas être utilisée.
1. Les objets qui se traduisent à travers une scène doivent avoir une durée correspondant à la distance qu’ils voyagent.

## <a name="timing-in-fluent-motion"></a>Minutage dans le mouvement Fluent

Le minutage du mouvement dans Fluent utilise 500 ms (ou une demi-seconde) comme base de référence, car il s’agit de la durée maximale que l’utilisateur perçoit comme instantanée.

![Image Hero](images/time.gif)

### <a name="150ms-exit"></a>**150 m** (sortie)

:::row:::
    :::column:::
À utiliser pour des objets ou des pages qui quittent la scène ou se ferment.
Permet d’obtenir rapidement des commentaires directionnels sur la sortie de l’interface utilisateur, où le minutage n’entrave pas la fréquence d’images pour obtenir une animation fluide.
    :::column-end:::
    :::column:::
        ![150 m de mouvement](images/150msAlt.gif)
    :::column-end:::
:::row-end:::

### <a name="300ms-enter"></a>**300 m** (entrée)

:::row:::
    :::column:::
Utilisez pour les objets ou les pages qui entrent dans la scène ou qui s’ouvrent.
Permet de consacrer un peu de temps à la fête du contenu au fur et à mesure de son entrée dans la scène.
    :::column-end:::
    :::column:::
        ![mouvement de 300 m](images/300ms.gif)
    :::column-end:::
:::row-end:::

### <a name="500ms-move"></a>**≤ 500 ms** (déplacement)

:::row:::
    :::column:::
À utiliser pour les objets qui se traduisent sur une seule scène ou dans plusieurs scènes. 
    :::column-end:::
    :::column:::
        ![mouvement de 500 ms](images/500ms.gif)
    :::column-end:::
:::row-end:::

## <a name="easing-in-fluent-motion"></a>Accélération du mouvement Fluent

L’accélération est un moyen de manipuler la vélocité d’un objet lors de son déplacement. C’est la colle qui associe toutes les expériences de mouvement Fluent. Bien qu’extrême, l’accélération utilisée dans le système permet d’unifier la sensation physique des objets qui se déplacent dans le système. Il s’agit d’une façon d’imiter le monde réel et de faire en sorte que les objets se sentent dans leur environnement.

![Image Hero](images/easing.gif)

## <a name="apply-easing-to-motion"></a>Appliquer une accélération à motion

Ces accélérations vous aideront à obtenir une sensation plus naturelle et sont la ligne de base que nous utilisons pour le mouvement Fluent.

Les exemples de code montrent comment appliquer des valeurs d’accélération recommandées aux animations d’animation (XAML) ou aux animations de composition (C#).

### <a name="accelerate-exit"></a>**Accélérer** (quitter)

:::row:::
    :::column:::
Utilisez pour l’interface utilisateur ou les objets qui quittent la scène.

Les objets sont alimentés et gagnent en puissance jusqu’à ce qu’ils atteignent la vélocité d’échappement.
L’idée est que l’objet tente son plus difficile à sortir de la méthode de l’utilisateur et de faire de la place pour que de nouveaux contenus soient disponibles.
    :::column-end:::
    :::column:::
        ![accélérer l’accélération](images/accelEase.gif)
    :::column-end:::
:::row-end:::

```
cubic-bezier(0.7 , 0 , 1 , 0.5)
```

```xaml
<!-- Use for XAML Storyboard animations. -->
<Storyboard x:Name="Storyboard">
    <DoubleAnimation Storyboard.TargetName="Translation" Storyboard.TargetProperty="X" From="0" To="200" Duration="0:0:0.15">
        <DoubleAnimation.EasingFunction>
            <ExponentialEase Exponent="4.5" EasingMode="EaseIn" />
        </DoubleAnimation.EasingFunction>
    </DoubleAnimation>
</Storyboard>
```

```csharp
// Use for Composition animations.
CubicBezierEasingFunction accelerate =
    _compositor.CreateCubicBezierEasingFunction(new Vector2(0.7f, 0.0f), new Vector2(1.0f, 0.5f));
_exitAnimation = _compositor.CreateScalarKeyFrameAnimation();
_exitAnimation.InsertKeyFrame(0.0f, _startValue);
_exitAnimation.InsertKeyFrame(1.0f, _endValue, accelerate);
_exitAnimation.Duration = TimeSpan.FromMilliseconds(150);
```

### <a name="decelerate-enter"></a>**Décélération** (entrée)

:::row:::
    :::column:::
À utiliser pour les objets ou l’interface utilisateur qui entrent dans la scène, en parcourant ou en générant.

Une fois sur scène, l’objet est rempli avec un frottement extrême, ce qui ralentit l’objet au repos.
L’idée est que l’objet est passé à partir d’une longue distance et qu’il est entré à une vélocité extrême, ou qu’il retourne rapidement à un État Rest.

Même si elle est précédée d’un moment de l’absence de réponse, la rapidité de l’objet entrant a un impact sur la rapidité et la réactivité.
    :::column-end:::
    :::column:::
        ![accélération de la décélération](images/decelEase.gif)
    :::column-end:::
:::row-end:::

```
cubic-bezier(0.1 , 0.9 , 0.2 , 1)
```

```xaml
<!-- Use for XAML Storyboard animations. -->
<Storyboard x:Name="Storyboard">
    <DoubleAnimation Storyboard.TargetName="Translation" Storyboard.TargetProperty="X" From="0" To="200" Duration="0:0:0.3">
        <DoubleAnimation.EasingFunction>
            <ExponentialEase Exponent="7" EasingMode="EaseOut" />
        </DoubleAnimation.EasingFunction>
    </DoubleAnimation>
</Storyboard>
```

```csharp
// Use for Composition animations.
CubicBezierEasingFunction decelerate =
    _compositor.CreateCubicBezierEasingFunction(new Vector2(0.1f, 0.9f), new Vector2(0.2f, 1.0f));
_enterAnimation = _compositor.CreateScalarKeyFrameAnimation();
_enterAnimation.InsertKeyFrame(0.0f, _startValue);
_enterAnimation.InsertKeyFrame(1.0f, _endValue, decelerate);
_enterAnimation.Duration = TimeSpan.FromMilliseconds(300);
```

### <a name="standard-easing-move"></a>**Accélération standard** (déplacement)

:::row:::
    :::column:::
Il s’agit de la base de référence pour les modifications de paramètres animés dans le système.
Utilisez une accélération standard pour les objets qui changent d’État à l’État à l’écran, par exemple une modification de position simple. En outre, utilisez-le pour transformer des objets en séquence, comme un objet qui croît.

L’idée est que les objets qui changent d’état de A à B sont survenus et pris en charge par, des forces naturelles.
    :::column-end:::
    :::column:::
        ![accélération standard](images/standardEase.gif)
    :::column-end:::
:::row-end:::

```
cubic-bezier(0.8 , 0 , 0.2 , 1)
```

```xaml
<!-- Use for XAML Storyboard animations. -->
<Storyboard x:Name="Storyboard">
    <DoubleAnimation Storyboard.TargetName="Translation" Storyboard.TargetProperty="X" From="0" To="200" Duration="0:0:0.5">
        <DoubleAnimation.EasingFunction>
            <CircleEase EasingMode="EaseInOut" />
        </DoubleAnimation.EasingFunction>
    </DoubleAnimation>
</Storyboard>
```

```csharp
// Use for Composition animations.
CubicBezierEasingFunction standard =
    _compositor.CreateCubicBezierEasingFunction(new Vector2(0.8f, 0.0f), new Vector2(0.2f, 1.0f));
 _moveAnimation = _compositor.CreateScalarKeyFrameAnimation();
 _moveAnimation.InsertKeyFrame(0.0f, _startValue);
 _moveAnimation.InsertKeyFrame(1.0f, _endValue, standard);
 _moveAnimation.Duration = TimeSpan.FromMilliseconds(500);
```

## <a name="related-articles"></a>Articles connexes

- [Vue d’ensemble du mouvement](index.md)
- [Direction et gravité](directionality-and-gravity.md)
