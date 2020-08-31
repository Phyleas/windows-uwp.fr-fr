---
title: Animations à effet ressort
description: Découvrez comment créer des expériences de ressort dans vos applications à l’aide des API NaturalMotionAnimation.
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP, animation
ms.localizationpriority: medium
ms.openlocfilehash: ecfb6fc001fbf42f70d40ee16abc45aa221c0a75
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89053869"
---
# <a name="spring-animations"></a>Animations à effet ressort

L’article montre comment utiliser Spring NaturalMotionAnimations.

## <a name="prerequisites"></a>Prérequis

Ici, nous partons du principe que vous êtes familiarisé avec les concepts abordés dans les articles suivants :

- [Animations de mouvement naturel](natural-animations.md)

## <a name="why-springs"></a>Pourquoi les ressorts ?

Les ressorts sont une expérience de mouvement courante que nous avons rencontrée à un moment donné de nos vies. allant de Symposium Toys à des expériences de classe physique avec un bloc lié à un ressort. Le mouvement oscillant d’un ressort incite souvent une réponse émotionnelle Playful et léger de ceux qui l’observent. Par conséquent, le mouvement d’un ressort se convertit en interface utilisateur d’application pour ceux qui cherchent à créer une expérience de mouvement livelier qui « dépile » plus d’un utilisateur final qu’une courbe de Bézier cubique traditionnelle. Dans ces cas-là, Spring Motion crée non seulement une expérience de mouvement livelier, mais peut également attirer l’attention sur le contenu nouveau ou animé actuellement. En fonction de la personnalisation de l’application ou du langage motion, l’oscillation est plus prononcée et visible, mais dans d’autres cas, il est plus subtil.

![Mouvement avec animation de ressort ](images/animation/offset-spring.gif)
 ![ avec animation de Bézier cubique](images/animation/offset-cubic-bezier.gif)

## <a name="using-springs-in-your-ui"></a>Utilisation de ressorts dans votre interface utilisateur

Comme mentionné précédemment, les ressorts peuvent être utiles pour s’intégrer à votre application et introduire une expérience d’interface utilisateur très familière et Playful. L’utilisation courante des ressorts dans l’interface utilisateur est la suivante :

| Description de l’utilisation des ressorts | Exemple visuel |
| ------------------------ | -------------- |
| Faites une expérience de mouvement « Pop » et regardez livelier. (Animation de l’échelle) | ![Mettre à l’échelle le mouvement avec l’animation Spring](images/animation/scale-spring.gif) |
| Faire de l’expérience de mouvement une apparence plus énergique (en animant le décalage) | ![Mouvement de décalage avec l’animation de ressort](images/animation/offset-spring.gif) |

Dans chacun de ces cas, le mouvement du ressort peut être déclenché en « ressort » et en oscillant autour d’une nouvelle valeur ou en oscillant autour de sa valeur actuelle avec une rapidité initiale.

![Oscillation de l’animation ressort](images/animation/spring-animation-diagram.png)

## <a name="defining-your-spring-motion"></a>Définition de votre mouvement de ressort

Vous créez une expérience de printemps à l’aide des API NaturalMotionAnimation. Plus précisément, vous créez un SpringNaturalMotionAnimation à l’aide des méthodes Create * du compositeur. Vous pouvez ensuite définir les propriétés suivantes du mouvement :

- DampingRatio : exprime le niveau d’amortissement du mouvement de ressort utilisé dans l’animation.

| Valeur du ratio d’amortissement | Description |
| ------------------- | ----------- |
| DampingRatio = 0 | Désamortie : le ressort oscille pendant une longue période |
| 0 < DampingRatio < 1 | Sous-amortie : le ressort oscille d’un peu à un grand. |
| DampingRatio = 1 | Criticallydamped : le printemps n’effectue aucune oscillation. |
| DampingRation > 1 | Suramortie : le ressort atteint rapidement sa destination avec une décélération brusque et aucune oscillation |

- Période : temps nécessaire à la détente pour effectuer une seule oscillation.
- Valeur finale/de départ : positions de début et de fin définies par le mouvement de ressort (si elles ne sont pas définies, la valeur de départ et/ou la valeur finale est la valeur actuelle).
- Rapidité initiale : rapidité initiale de programmation pour le mouvement.

Vous pouvez également définir un ensemble de propriétés de motion qui sont identiques à KeyFrameAnimations :

- Comportement de DelayTime/Delay
- StopBehavior

Dans les cas courants d’animation du décalage et de la taille/taille, les valeurs suivantes sont recommandées par l’équipe de conception Windows pour DampingRatio et period pour les différents types de ressorts :

| Property | Ressort normal | Ressort amorti | Ressort moins amorti |
| -------- | ------------- | --------------- | -------------------- |
| Offset | Ratio d’amortissement = 0,8 <br/> Période = 50 ms | Ratio d’amortissement = 0,85 <br/> Période = 50 ms | Ratio d’amortissement = 0,65 <br/> Période = 60 ms |
| Échelle/taille | Ratio d’amortissement = 0,7 <br/> Période = 50 ms | Ratio d’amortissement = 0,8 <br/> Période = 50 ms | Ratio d’amortissement = 0,6 <br/> Période = 60 ms |

Une fois que vous avez défini les propriétés, vous pouvez passer votre NaturalMotionAnimation Spring dans la méthode StartAnimation d’un CompositionObject ou la propriété motion d’un InertiaModifier InteractionTracker.

## <a name="example"></a>Exemple

Dans cet exemple, vous créez une expérience d’interface utilisateur de navigation et de canevas dans laquelle, quand l’utilisateur clique sur un bouton de développement, un volet de navigation est animé avec un mouvement élastique et oscillant.

![Animation Spring sur clic](images/animation/spring-animation-on-click.gif)

Commencez par définir l’animation Spring dans l’événement de clic pour lorsque le volet de navigation s’affiche. Vous définissez ensuite les propriétés de l’animation, à l’aide de la fonctionnalité InitialValueExpression pour utiliser une expression pour définir le FinalValue. Vous gardez également à l’esprit si le volet est ouvert ou non et, lorsque vous êtes prêt, démarrez l’animation.

```csharp
private void Button_Clicked(object sender, RoutedEventArgs e)
{
 _springAnimation = _compositor.CreateSpringScalarAnimation();
 _springAnimation.DampingRatio = 0.75f;
 _springAnimation.Period = TimeSpan.FromSeconds(0.5);

 if (!_expanded)
 {
 _expanded = true;
 _propSet.InsertBoolean("expanded", true);
 _springAnimation.InitialValueExpression["FinalValue"] = "this.StartingValue + 250";
 } else
 {
 _expanded = false;
 _propSet.InsertBoolean("expanded", false);
_springAnimation.InitialValueExpression["FinalValue"] = "this.StartingValue - 250";
 }
 _naviPane.StartAnimation("Offset.X", _springAnimation);
}
```

Que se passe-t-il si vous souhaitez lier ce mouvement à l’entrée ? Par conséquent, si l’utilisateur final fait défiler les volets, les volets s’accompagnent d’un mouvement de ressort ? Plus important encore, si l’utilisateur effectue un balayage plus complexe ou plus rapide, l’adaptateur de mouvement est basé sur la vélocité de l’utilisateur final.

![Animation de ressort lors du balayage](images/animation/spring-animation-on-swipe.gif)

Pour ce faire, vous pouvez utiliser la même animation Spring et la transmettre à un InertiaModifier avec InteractionTracker. Pour plus d’informations sur InputAnimations et InteractionTracker, consultez [expériences de manipulation personnalisées avec InteractionTracker](interaction-tracker-manipulations.md). Nous partons du principe que vous avez déjà configuré vos InteractionTracker et VisualInteractionSource pour cet exemple de code. Nous allons nous concentrer sur la création des InertiaModifiers qui prendront en compte un NaturalMotionAnimation, dans ce cas un ressort.

```csharp
// InteractionTracker and the VisualInteractionSource previously setup
// The open and close ScalarSpringAnimations defined earlier
private void SetupInput()
{
 // Define the InertiaModifier to manage the open motion
 var openMotionModifer = InteractionTrackerInertiaNaturalMotion.Create(compositor);

 // Condition defines to use open animation if panes in non-expanded view
 // Property set value to track if open or closed is managed in other part of code
 openMotionModifer.Condition = _compositor.CreateExpressionAnimation(
"propset.expanded == false");
 openMotionModifer.Condition.SetReferenceParameter("propSet", _propSet);
 openMotionModifer.NaturalMotion = _openSpringAnimation;

 // Define the InertiaModifer to manage the close motion
 var closeMotionModifier = InteractionTrackerInertiaNaturalMotion.Create(_compositor);

 // Condition defines to use close animation if panes in expanded view
 // Property set value to track if open or closed is managed in other part of code
 closeMotionModifier.Condition = 
_compositor.CreateExpressionAnimation("propSet.expanded == true");
 closeMotionModifier.Condition.SetReferenceParameter("propSet", _propSet);
 closeMotionModifier.NaturalMotion = _closeSpringAnimation;

 _tracker.ConfigurePositionXInertiaModifiers(new 
InteractionTrackerInertiaNaturalMotion[] { openMotionModifer, closeMotionModifier});

 // Take output of InteractionTracker and assign to the pane
 var exp = _compositor.CreateExpressionAnimation("-tracker.Position.X");
 exp.SetReferenceParameter("tracker", _tracker);
 ElementCompositionPreview.GetElementVisual(pageNavigation).
StartAnimation("Translation.X", exp);
}
```

Vous disposez à présent d’une animation de programmation et de ressort pilotée par l’entrée dans votre interface utilisateur !

En résumé, les étapes à suivre pour utiliser une animation Spring dans votre application :

1. Créez vos SpringAnimation à partir de votre compositeur.
1. Définissez les propriétés du SpringAnimation si vous souhaitez des valeurs autres que celles par défaut :
    - DampingRatio
    - Période
    - Valeur finale
    - Valeur initiale
    - Rapidité initiale
1. Assigner à la cible.
    - Si vous animez une propriété CompositionObject, transmettez SpringAnimation en tant que paramètre à StartAnimation.
    - Si vous souhaitez utiliser with Input, définissez la propriété NaturalMotion d’un InertiaModifier sur SpringAnimation.

