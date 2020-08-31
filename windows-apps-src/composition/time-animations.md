---
title: Animations basées sur le temps
description: Découvrez comment utiliser les classes KeyFrameAnimations pour créer des animations basées sur le temps qui guident les utilisateurs à travers les modifications de l’interface utilisateur.
ms.date: 12/12/2018
ms.topic: article
keywords: Windows 10, UWP, animation
ms.localizationpriority: medium
ms.openlocfilehash: c63f59e7bcf282dc829d0fb8fa5971113f7638ad
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89053589"
---
# <a name="time-based-animations"></a>Animations basées sur l’heure

Quand un composant dans ou une expérience utilisateur complète change, les utilisateurs finaux l’observent souvent de deux manières : au fil du temps ou instantanément. Sur la plate-forme Windows, le premier est préféré par rapport aux expériences utilisateur qui changent instantanément et qui ne sont pas en mesure de suivre les utilisateurs finaux. L’utilisateur final perçoit alors l’expérience comme transférerez et peu naturelle.

Au lieu de cela, vous pouvez modifier votre interface utilisateur au fil du temps pour guider l’utilisateur final, ou les informer des modifications apportées à l’expérience. Sur la plate-forme Windows, cette opération s’effectue à l’aide d’animations basées sur le temps, également appelées KeyFrameAnimations. KeyFrameAnimations vous permet de modifier une interface utilisateur au fil du temps et de contrôler chaque aspect de l’animation, notamment comment et quand elle démarre et comment elle atteint son état final. Par exemple, l’animation d’un objet vers une nouvelle position sur 300 millisecondes est plus agréable que le « téléportage ». Lorsque vous utilisez des animations au lieu de modifications instantanées, le résultat net est une expérience plus agréable et attrayante.

## <a name="types-of-time-based-animations"></a>Types d’animations basées sur l’heure

Il existe deux catégories d’animations basées sur le temps que vous pouvez utiliser pour créer de superbes expériences utilisateur sur Windows :

**Animations explicites** : comme le nom signifie, vous démarrez explicitement l’animation pour effectuer des mises à jour.
**Animations implicites** : ces animations sont lancées par le système en votre nom lorsqu’une condition est remplie.

Pour cet article, nous allons aborder la création et l’utilisation d’animations _explicites_ basées sur le temps avec KeyFrameAnimations.

Pour les animations de temps explicites et implicites, il existe différents types, qui correspondent aux différents types de propriétés de CompositionObjects que vous pouvez animer.

- ColorKeyFrameAnimation
- QuaternionKeyFrameAnimation
- ScalarKeyFrameAnimation
- Vector2KeyFrameAnimation
- Vector3KeyFrameAnimation
- Vector4KeyFrameAnimation

## <a name="create-time-based-animations-with-keyframeanimations"></a>Créer des animations basées sur l’heure avec KeyFrameAnimations

Avant de décrire comment créer des animations explicites basées sur le temps avec KeyFrameAnimations, passons en revue quelques concepts.

- Images clés : il s’agit des « instantanés » individuels qu’une animation va animer.
  - Défini en tant que paires clé & valeur. La clé représente la progression comprise entre 0 et 1, où, dans la durée de vie de l’animation, cette « capture instantanée » a lieu. L’autre paramètre représente la valeur de la propriété pour l’instant.
- Propriétés KeyFrameAnimation : options de personnalisation que vous pouvez appliquer pour répondre aux besoins de l’interface utilisateur.
  - DelayTime : temps avant le démarrage d’une animation après l’appel de StartAnimation.
  - Duration : durée de l’animation.
  - IterationBehavior : nombre ou comportement de répétition infini pour une animation.
  - IterationCount : nombre de fois finis qu’une animation d’image clé se répète.
  - Nombre d’images clés : lit le nombre d’images clés dans une animation d’image clé particulière.
  - StopBehavior : spécifie le comportement d’une valeur de propriété d’animation lorsque StopAnimation est appelé.
  - Direction : spécifie la direction de l’animation pour la lecture.
- Groupe d’animation : démarrage de plusieurs animations en même temps.
  - Souvent utilisé lorsque vous souhaitez animer plusieurs propriétés en même temps.

Pour plus d’informations, consultez [CompositionAnimationGroup](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionanimationgroup).

Avec ces concepts à l’esprit, nous allons nous pencher sur la formule générale permettant de construire un KeyFrameAnimation :

1. Identifiez le CompositionObject et sa propriété respective que vous devez animer.
1. Créez un modèle de type KeyFrameAnimation du compositeur qui correspond au type de propriété que vous souhaitez animer.
1. À l’aide du modèle d’animation, commencez à ajouter des images clés et à définir les propriétés de l’animation.
    - Au moins une image clé est requise (l’image clé 100% ou 1F).
    - Il est également recommandé de définir une durée.
1. Une fois que vous êtes prêt à exécuter cette animation, appelez StartAnimation (...) sur le CompositionObject, en ciblant la propriété que vous souhaitez animer. Plus précisément :
    - `visual.StartAnimation("targetProperty", CompositionAnimation animation);`
    - `visual.StartAnimationGroup(AnimationGroup animationGroup);`
1. Si vous avez une animation en cours d’exécution et que vous souhaitez arrêter l’animation ou le groupe d’animation, vous pouvez utiliser les API suivantes :
    - `visual.StopAnimation("targetProperty");`
    - `visual.StopAnimationGroup(AnimationGroup AnimationGroup);`

Prenons un exemple pour voir cette formule en action.

## <a name="example"></a>Exemple

Dans cet exemple, vous souhaitez animer le décalage d’un visuel de <0, 0, 0> à <200, 0,0> sur 1 seconde. En outre, vous souhaitez voir l’animation visuelle entre ces positions 10 fois.

![Animation d’image clé](images/animation/animated-rectangle.gif)

Commencez par identifier le CompositionObject et la propriété que vous souhaitez animer. Dans ce cas, le carré rouge est représenté par un visuel de composition nommé `redVisual` . Vous démarrez votre animation à partir de cet objet.

Ensuite, étant donné que vous souhaitez animer la propriété offset, vous devez créer un Vector3KeyFrameAnimation (offset est de type Vector3). Vous définissez également les images clés correspondantes pour le KeyFrameAnimation.

```csharp
    Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
    animation.InsertKeyFrame(1f, new Vector3(200f, 0f, 0f));
```

Vous définissez ensuite les propriétés de KeyFrameAnimation pour décrire sa durée, ainsi que le comportement d’animation entre les deux positions (Current et <200, 0, 0>) 10 fois.

```csharp
    animation.Duration = TimeSpan.FromSeconds(2);
    animation.Direction = Windows.UI.Composition.AnimationDirection.Alternate;
    // Run animation for 10 times
    animation.IterationCount = 10;
```

Enfin, pour exécuter une animation, vous devez la démarrer sur une propriété d’un CompositionObject.

```csharp
redVisual.StartAnimation("Offset", animation);
```

Voici le code complet.

```csharp
private void AnimateSquare(Compositor compositor, SpriteVisual redVisual)
{ 
    Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
    animation.InsertKeyFrame(1f, new Vector3(200f, 0f, 0f));
    animation.Duration = TimeSpan.FromSeconds(2);
    animation.Direction = Windows.UI.Composition.AnimationDirection.Alternate;
    // Run animation for 10 times
    animation.IterationCount = 10;
    redVisual.StartAnimation("Offset", animation);
} 
```
