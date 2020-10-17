---
title: Améliorer les expériences ScrollViewer existantes
description: Découvrez comment utiliser un ScrollViewer et des ExpressionAnimations XAML pour créer des expériences de mouvement dynamiques pilotées par entrée.
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP, animation
ms.localizationpriority: medium
ms.openlocfilehash: 438f108a07349da6515443e64bd4494529b8e6a0
ms.sourcegitcommit: fe21402578a1f434769866dd3c78aac63dbea5ea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92152402"
---
# <a name="enhance-existing-scrollviewer-experiences"></a>Améliorer les expériences ScrollViewer existantes

Cet article explique comment utiliser un ScrollViewer et ExpressionAnimations XAML pour créer des animations dynamiques pilotées par entrée.

## <a name="prerequisites"></a>Prérequis

Ici, nous partons du principe que vous êtes familiarisé avec les concepts abordés dans les articles suivants :

- [Animations pilotées par une entrée](input-driven-animations.md)
- [Animations basées sur les relations](relation-animations.md)

## <a name="why-build-on-top-of-scrollviewer"></a>Pourquoi créer en plus de ScrollViewer ?

En général, vous utilisez le ScrollViewer XAML existant pour créer une surface de défilement et de zoom pour le contenu de votre application. Avec l’introduction du langage de conception Fluent, vous devez maintenant vous concentrer sur la façon d’utiliser le défilement ou le zoom d’une surface pour conduire d’autres expériences de mouvement. Par exemple, en utilisant le défilement pour conduire une animation floue d’un arrière-plan ou un lecteur de la position d’un « en-tête rémanent ».

Dans ces scénarios, vous tirez parti des expériences de comportement ou de manipulation telles que le défilement et le zoom pour rendre plus dynamiques d’autres parties de votre application. Celles-ci permettent à l’application de se sentir plus cohérente, ce qui rend l’expérience plus facile à retenir dans les yeux des utilisateurs finaux. En rendant l’interface utilisateur de l’application plus mémorable, les utilisateurs finaux s’accèdent plus fréquemment à l’application et Pendant des périodes plus longues.

## <a name="what-can-you-build-on-top-of-scrollviewer"></a>Que pouvez-vous créer par-dessus ScrollViewer ?

Vous pouvez tirer parti de la position d’un ScrollViewer pour créer un certain nombre d’expériences dynamiques :

- Parallaxe : utilisez la position d’un ScrollViewer pour déplacer le contenu d’arrière-plan ou de premier plan à une vitesse relative à la position de défilement.
- StickyHeaders : utiliser la position d’un ScrollViewer pour animer et « coller » un en-tête à une position
- Input-Driven Effects : utilisez la position d’un ScrollViewer pour animer un effet de composition tel qu’un flou.

En général, en référençant la position d’un ScrollViewer avec un ExpressionAnimation, vous pouvez créer une animation qui change de façon dynamique la valeur de défilement.

![Mode liste avec parallaxe](images/animation/parallax.gif)

![En-tête discret](images/animation/shy-header.gif)

## <a name="using-scrollviewermanipulationpropertyset"></a>Utilisation de ScrollViewerManipulationPropertySet

Pour créer ces expériences dynamiques à l’aide d’un ScrollViewer XAML, vous devez être en mesure de faire référence à la position de défilement dans une animation. Pour ce faire, vous accédez à un CompositionPropertySet à partir du ScrollViewer XAML appelé ScrollViewerManipulationPropertySet.
ScrollViewerManipulationPropertySet contient une propriété Vector3 unique appelée translation qui donne accès à la position de défilement de l’ScrollViewer. Vous pouvez ensuite le référencer comme n’importe quel autre CompositionPropertySet dans votre ExpressionAnimation.

Étapes générales de la mise en route :

1. Accédez au ScrollViewerManipulationPropertySet via ElementCompositionPreview.
    - `ElementCompositionPreview.GetScrollViewerManipulationPropertySet(ScrollViewer scroller)`
1. Créez un ExpressionAnimation qui fait référence à la propriété translation à partir de PropertySet.
    - N’oubliez pas de définir le paramètre de référence !
1. Ciblez la propriété d’un CompositionObject avec ExpressionAnimation.

> [!NOTE]
> Il est recommandé d’assigner le PropertySet retourné à partir de la méthode GetScrollViewerManipulationPropertySet à une variable de classe. Cela garantit que le jeu de propriétés n’est pas nettoyé par le garbage collection et, par conséquent, n’a aucun effet sur le ExpressionAnimation dans lequel il est référencé. Les ExpressionAnimations ne maintiennent pas une référence forte à l’un des objets utilisés dans l’équation.

## <a name="example"></a> Exemple

Jetons un coup d’œil sur la façon dont l’exemple parallaxe présenté ci-dessus est regroupé. Pour référence, tout le code source de l’application se trouve dans [Windows UI dev Labs référentiel sur GitHub](https://github.com/microsoft/WindowsCompositionSamples).

La première chose à faire est d’obtenir une référence au ScrollViewerManipulationPropertySet.

```csharp
_scrollProperties =
    ElementCompositionPreview.GetScrollViewerManipulationPropertySet(myScrollViewer);
```

L’étape suivante consiste à créer le ExpressionAnimation qui définit une équation qui utilise la position de défilement de l’ScrollViewer.

```csharp
_parallaxExpression = compositor.CreateExpressionAnimation();
_parallaxExpression.SetScalarParameter("StartOffset", 0.0f);
_parallaxExpression.SetScalarParameter("ParallaxValue", 0.5f);
_parallaxExpression.SetScalarParameter("ItemHeight", 0.0f);
_parallaxExpression.SetReferenceParameter("ScrollManipulation", _scrollProperties);
_parallaxExpression.Expression = "(ScrollManipulation.Translation.Y + StartOffset - (0.5 * ItemHeight)) * ParallaxValue - (ScrollManipulation.Translation.Y + StartOffset - (0.5 * ItemHeight))";
```

> [!NOTE]
> Vous pouvez également utiliser les classes d’assistance ExpressionBuilder pour construire cette même expression sans avoir besoin de chaînes :

> ```csharp
> var scrollPropSet = _scrollProperties.GetSpecializedReference<ManipulationPropertySetReferenceNode>();
> var parallaxValue = 0.5f;
> var parallax = (scrollPropSet.Translation.Y + startOffset);
> _parallaxExpression = parallax * parallaxValue - parallax;
> ```

Enfin, vous prenez ce ExpressionAnimation et ciblez le visuel que vous souhaitez faire passer à la parallaxe. Dans ce cas, il s’agit de l’image de chacun des éléments de la liste.

```csharp
Visual visual = ElementCompositionPreview.GetElementVisual(image);
visual.StartAnimation("Offset.Y", _parallaxExpression);
```
