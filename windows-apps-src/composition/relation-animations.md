---
title: Animations basées sur les relations
description: Découvrez comment utiliser ExpressionAnimations pour créer des animations basées sur les relations lorsque Motion dépend d’une propriété d’un autre objet.
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP, animation
ms.localizationpriority: medium
ms.openlocfilehash: 91e3ae5b23b7429633053f4d4d876f02127d26e3
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89054419"
---
# <a name="relation-based-animations"></a>Animations basées sur les relations

Cet article fournit une brève présentation de la création d’animations basées sur les relations à l’aide de la composition ExpressionAnimations.

## <a name="dynamic-relation-based-experiences"></a>Expériences dynamiques basées sur les relations

Lors de la création d’expériences de mouvement dans une application, il peut arriver que le mouvement ne soit pas basé sur le temps, mais plutôt dépendant d’une propriété sur un autre objet. Les KeyFrameAnimations ne sont pas en mesure d’exprimer très facilement ces types d’expériences de mouvement. Dans ces instances spécifiques, motion ne doit plus être discret et prédéfini. Au lieu de cela, le mouvement peut s’adapter dynamiquement en fonction de sa relation aux autres propriétés de l’objet. Par exemple, vous pouvez animer l’opacité d’un objet en fonction de sa position horizontale. D’autres exemples incluent des expériences de mouvement comme les en-têtes et le parallaxe.

Ces types d’expériences de mouvement vous permettent de créer une interface utilisateur qui semble plus connectée, au lieu d’en faire un singulier et indépendant. Pour l’utilisateur, cela donne l’impression d’une expérience d’interface utilisateur dynamique.

![Cercle d’orbite](images/animation/orbit.gif)

![Mode liste avec parallaxe](images/animation/parallax.gif)

## <a name="using-expressionanimations"></a>Utilisation de ExpressionAnimations

Pour créer des expériences de mouvement basées sur les relations, vous utilisez le type ExpressionAnimation. ExpressionAnimations (ou expressions pour Short) est un nouveau type d’animation qui vous permet d’exprimer une relation mathématique : une relation que le système utilise pour calculer la valeur d’une propriété d’animation à chaque trame. En d’autres termes, les expressions sont simplement une équation mathématique qui définit la valeur souhaitée d’une propriété d’animation par image. Les expressions sont un composant très polyvalent qui peut être utilisé dans un large éventail de scénarios, notamment :

- Taille relative, animations de décalage.
- En-têtes collants, parallaxe avec ScrollViewer. (Voir [améliorer les expériences ScrollViewer existantes](scroll-input-animations.md).)
- Points d’alignement avec InertiaModifiers et InteractionTracker. (Consultez [créer des points d’ancrage avec des modificateurs d’inertie](inertia-modifiers.md).)

Lorsque vous travaillez avec ExpressionAnimations, il est important de mentionner les points suivants :

- Ne jamais se terminer, contrairement à son frère KeyFrameAnimation, les expressions n’ont pas une durée finie. Étant donné que les expressions sont des relations mathématiques, il s’agit d’animations qui sont constamment « en cours d’exécution ». Vous avez la possibilité d’arrêter ces animations si vous le souhaitez.
- En cours d’exécution, mais pas toujours en cours d’évaluation, les performances sont toujours un problème avec les animations qui s’exécutent en permanence. Vous n’avez pas besoin de vous préoccuper du fait que le système est suffisamment intelligent pour que l’expression ne soit réévaluée que si l’une de ses entrées ou ses paramètres a changé.
- Résolution vers le type d’objet approprié : étant donné que les expressions sont des relations mathématiques, il est important de s’assurer que l’équation qui définit l’expression correspond au même type de la propriété ciblée par l’animation. Par exemple, en cas d’animation du décalage, votre expression doit être résolue en un type Vector3.

### <a name="components-of-an-expression"></a>Composants d’une expression

Lors de la création de la relation mathématique d’une expression, il existe plusieurs composants principaux :

- Parameters : valeurs représentant des valeurs constantes ou des références à d’autres objets composition.
- Opérateurs mathématiques : opérateurs mathématiques classiques plus (+), moins (-), multiplication (*), Division (/) qui associent des paramètres pour former une équation. Les opérateurs conditionnels tels que supérieur à (>), égal (= =), ternaire Operator (condition) sont également inclus. ifTrue : ifFalse), etc.
- Fonctions mathématiques : fonctions mathématiques/raccourcis basés sur System. Numerics. Pour obtenir la liste complète des fonctions prises en charge, consultez [ExpressionAnimation](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.ExpressionAnimation).

Les expressions prennent également en charge un ensemble de mots clés (expressions spéciales qui ont une signification distincte uniquement dans le système ExpressionAnimation). Celles-ci sont répertoriées (ainsi que la liste complète des fonctions mathématiques) dans la documentation [ExpressionAnimation](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.ExpressionAnimation) .

### <a name="creating-expressions-with-expressionbuilder"></a>Création d’expressions avec ExpressionBuilder

Il existe deux options pour créer des expressions dans leur application UWP :

1. Génération de l’équation sous forme de chaîne via l’API publique officielle.
1. Génération de l’équation dans un modèle objet de type sécurisé via l’outil ExpressionBuilder Open source. Consultez la [source et la documentation GitHub](https://github.com/microsoft/WindowsCompositionSamples/tree/master/ExpressionBuilder).

Pour les besoins de ce document, nous allons définir nos expressions à l’aide de ExpressionBuilder.

### <a name="parameters"></a>Paramètres

Les paramètres constituent le cœur d’une expression. Il existe deux types de paramètres :

- Constantes : il s’agit de paramètres représentant des variables typées System. Numeric. Ces paramètres voient leurs valeurs affectées une fois que l’animation est démarrée.
- Références : ces paramètres représentent des références à CompositionObjects : ces paramètres obtiennent en continu leurs valeurs mises à jour après le démarrage d’une animation.

En général, les références sont l’aspect principal de la façon dont la sortie d’une expression peut changer dynamiquement. À mesure que ces références changent, la sortie de l’expression change en conséquence. Si vous créez votre expression avec des chaînes ou si vous les utilisez dans un scénario de création de modèles (à l’aide de votre expression pour cibler plusieurs CompositionObjects), vous devez nommer et définir les valeurs de vos paramètres. Voir la section Exemple pour plus d'informations.

### <a name="working-with-keyframeanimations"></a>Utilisation de KeyFrameAnimations

Les expressions peuvent également être utilisées avec KeyFrameAnimations. Dans ce cas, vous souhaitez utiliser une expression pour définir la valeur d’une image clé à un moment donné. ces types de trames sont appelés ExpressionKeyFrames.

```csharp
KeyFrameAnimation.InsertExpressionKeyFrame(Single, String)
KeyFrameAnimation.InsertExpressionKeyFrame(Single, ExpressionNode)
```

Toutefois, contrairement à ExpressionAnimations, les ExpressionKeyFrames ne sont évalués qu’une seule fois lorsque le KeyFrameAnimation est démarré. N’oubliez pas que vous ne transmettez pas un ExpressionAnimation en tant que valeur de l’image clé, plutôt qu’une chaîne (ou un ExpressionNode, si vous utilisez ExpressionBuilder).

## <a name="example"></a>Exemple

Passons maintenant en revue un exemple d’utilisation d’expressions, en particulier l’exemple PropertySet de la Galerie d’exemples d’interfaces utilisateur Windows. Nous allons examiner l’expression qui gère le comportement de mouvement orbite de la boule bleue.

![Cercle d’orbite](images/animation/orbit.gif)

Il existe trois composants en lecture pour l’expérience totale :

1. Un KeyFrameAnimation, en animant le décalage Y de la boule rouge.
1. PropertySet avec une propriété de **rotation** qui permet de piloter l’orbite, animée par un autre KeyFrameAnimation.
1. Un ExpressionAnimation qui pilote le décalage de la boule bleue référençant le décalage de la bille rouge et la propriété de rotation pour maintenir un orbite parfait.

Nous nous concentrerons sur le ExpressionAnimation défini dans #3. Nous utiliserons également les classes ExpressionBuilder pour construire cette expression. Une copie du code utilisé pour générer cette expérience via des chaînes est indiquée à la fin.

Dans cette équation, il existe deux propriétés que vous devez référencer à partir de PropertySet ; l’un est un décalage Centerpoint et l’autre est la rotation.

```
var propSetCenterPoint =
_propertySet.GetReference().GetVector3Property("CenterPointOffset");

// This rotation value will animate via KFA from 0 -> 360 degrees
var propSetRotation = _propertySet.GetReference().GetScalarProperty("Rotation");
```

Ensuite, vous devez définir le composant Vector3 qui tient compte de la rotation d’orbite réelle.

```
var orbitRotation = EF.Vector3(
    EF.Cos(EF.ToRadians(propSetRotation)) * 150,
    EF.Sin(EF.ToRadians(propSetRotation)) * 75, 0);
```

> [!NOTE]
> `EF` est une notation « using » abrégée pour définir ExpressionBuilder. ExpressionFunctions.

Enfin, Combinez ces composants et référencez la position de la boule rouge pour définir la relation mathématique.

```
var orbitExpression = redSprite.GetReference().Offset + propSetCenterPoint + orbitRotation;
blueSprite.StartAnimation("Offset", orbitExpression);
```

Dans une situation hypothétique, que se passerait-il si vous souhaitiez utiliser cette même expression mais avec deux autres visuels, ce qui signifie 2 jeux de cercles d’orbite. Avec CompositionAnimations, vous pouvez réutiliser l’animation et cibler plusieurs CompositionObjects. La seule chose que vous devez modifier lorsque vous utilisez cette expression pour le cas Orbit supplémentaire est la référence à l’élément visuel. Nous appelons ce modèles.

Dans ce cas, vous modifiez l’expression que vous avez créée précédemment. Au lieu d’obtenir une référence à CompositionObject, vous créez une référence avec un nom, puis vous affectez des valeurs différentes :

```
var orbitExpression = ExpressionValues.Reference.CreateVisualReference("orbitRoundVisual");
orbitExpression.SetReferenceParameter("orbitRoundVisual", redSprite);
blueSprite.StartAnimation("Offset", orbitExpression);
// Later on … use same Expression to assign to another orbiting Visual
orbitExpression.SetReferenceParameter("orbitRoundVisual", yellowSprite);
greenSprite.StartAnimation("Offset", orbitExpression);
```

Voici le code si vous avez défini votre expression avec des chaînes via l’API publique.

```
ExpressionAnimation expressionAnimation =
compositor.CreateExpressionAnimation("visual.Offset + " +
"propertySet.CenterPointOffset + " +
"Vector3(cos(ToRadians(propertySet.Rotation)) * 150," + "sin(ToRadians(propertySet.Rotation)) * 75, 0)");
 var propSetCenterPoint = _propertySet.GetReference().GetVector3Property("CenterPointOffset");
 var propSetRotation = _propertySet.GetReference().GetScalarProperty("Rotation");
expressionAnimation.SetReferenceParameter("propertySet", _propertySet);
expressionAnimation.SetReferenceParameter("visual", redSprite);
```
