---
title: Animations de mouvement naturel
description: Découvrez les animations de mouvement naturel et comment les utiliser dans l’interface utilisateur de votre application.
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP, animation
ms.localizationpriority: medium
ms.openlocfilehash: 02c76991a60205042642f57fed475755db8c8071
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174083"
---
# <a name="natural-motion-animations"></a>Animations de mouvement naturel

Cet article fournit une brève vue d’ensemble de l’espace NaturalMotionAnimation et explique de manière conceptuelle comment utiliser ces types d’animations dans votre interface utilisateur.

## <a name="making-motion-feel-familiar-and-natural"></a>Rendre le mouvement familier et naturel

Les applications intéressantes créent des expériences qui capturent et conservent l’attention de l’utilisateur et aident les utilisateurs à effectuer des tâches. Motion est le facteur de différenciation clé qui sépare une interface utilisateur de l’expérience utilisateur, ce qui permet d’établir une connexion entre les utilisateurs et l’application avec laquelle ils interagissent. Plus la connexion est efficace, plus l’engagement et la satisfaction des utilisateurs finaux sont élevés.

Pour créer cette connexion, vous pouvez créer des expériences qui semblent familières aux utilisateurs. Les utilisateurs ont des attentes inconscientes quant à la façon dont ils perçoivent motion en se basant sur les expériences réelles. Nous voyons comment les objets s’impriment à l’étage, à tomber dans le tableau, à rebondir les uns sur les autres et à osciller avec un ressort. Le mouvement qui tire parti de cette attente en se basant sur la physique réelle ressemble à la réalité et semble plus naturel dans nos yeux. Le mouvement devient plus naturel et plus interactif, mais plus important encore, l’expérience complète devient plus mémorable et délicieuse.

![Mettre à l’échelle le mouvement sans mouvement de mise à l’échelle avec un mouvement de mise à l’échelle ](images/animation/scale-no-animation.gif)
 ![ cubique ](images/animation/scale-cubic-bezier.gif)
 ![ avec une animation Spring](images/animation/scale-spring.gif)

Le résultat net est un engagement utilisateur et une rétention plus élevés avec l’application.

## <a name="balancing-control-and-dynamism"></a>Contrôle d’équilibrage et dynamisme

Dans l’interface utilisateur traditionnelle, [KeyFrameAnimation](/uwp/api/windows.ui.composition.keyframeanimation)est le moyen le plus dominant pour décrire motion. Les images clés offraient le plus grand contrôle aux concepteurs et aux développeurs pour définir le début, la fin et l’interpolation. Bien que cela soit très utile dans de nombreux cas, les animations d’images clés ne sont pas très dynamiques. le mouvement n’est pas adaptable et se présente sous n’importe quelle condition.

À l’autre extrémité du spectre, des simulations sont souvent observées dans les moteurs de jeux et physiques. Ces expériences sont souvent les plus réalistes et dynamiques avec lesquelles les utilisateurs interagissent, créant ainsi un sens de la ambiance et de la randomisation que les utilisateurs voient quotidiennement. Bien que cela rende la sensation de mouvement plus active et dynamique, les concepteurs et les développeurs ont moins de contrôle, ce qui rend plus difficile l’intégration de l’interface utilisateur traditionnelle.

![Diagramme du spectre de contrôle](images/animation/natural-motion-diagram.png)

[NaturalMotionAnimation](/uwp/api/windows.ui.composition.naturalmotionanimation)existe pour aider à combler cette division, ce qui permet d’obtenir un équilibre entre les éléments importants d’une animation, tels que le début et la fin, tout en conservant le mouvement qui ressemble et semble naturel et dynamique.

> [!NOTE]
> Les NaturalMotionAnimations ne sont pas destinés à remplacer les animations d’images clés. dans ce cas, les images clés sont toujours placées dans le langage de conception Fluent. Les NaturalMotionAnimations sont conçus pour être utilisés dans des endroits où le mouvement est nécessaire, mais les animations d’images clés ne sont pas suffisamment dynamiques.

## <a name="using-naturalmotionanimations"></a>Utilisation de NaturalMotionAnimations

À partir de la mise à jour des créateurs de automne, vous avez accès à une nouvelle expérience de mouvement : les **animations de ressort**. Pour plus d’informations sur les ressorts, consultez [animations Springs](spring-animations.md) .

Ce type de mouvement est obtenu à l’aide du nouveau NaturalMotionAnimation – un nouveau type d’animation centré sur l’activation des développeurs pour créer des mouvements de sensation plus familiers et plus naturels dans leur interface utilisateur, avec un équilibre entre contrôle et dynamisme. Ils exposent les fonctionnalités suivantes :

- Définissez les valeurs de début et de fin.
- Définissez InitialVelocity et liez à Input avec InteractionTracker.
- Définir des propriétés spécifiques aux mouvements (telles que DampingRatio pour les ressorts.)

Formule générale pour commencer :

1. Créez le NaturalMotionAnimation à partir du compositeur à l’aide de l’une des méthodes **Create** .
1. Définissez les propriétés de l’animation.
1. Transmettez le NaturalMotionAnimation en tant que paramètre à l’appel StartAnimation d’un CompositionObject.
    - Ou défini sur la propriété motion d’un InertiaModifier InteractionTracker.

Exemple de base utilisant un Spring NaturalMotionAnimation pour créer un « ressort » visuel sur un nouvel emplacement de décalage X :

```csharp
_springAnimation = _compositor.CreateSpringScalarAnimation();
_springAnimation.Period = TimeSpan.FromSeconds(0.07);
_springAnimation.DelayTime = TimeSpan.FromSeconds(1);
_springAnimation.EndPoint = 500f;
sectionNav.StartAnimation("Offset.X", _springAnimation);
```