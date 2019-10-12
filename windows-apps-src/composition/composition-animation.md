---
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
title: Animations de composition
description: De nombreuses propriétés d’objet et d’effet de composition peuvent être animées à l’aide d’animations par images clés et expressions, ce qui permet aux propriétés d’un élément d’interface utilisateur de changer dans le temps ou en fonction d’un calcul.
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5991b5f9f3c63a25a7476cc35621488c6cb60b2c
ms.sourcegitcommit: 445320ff0ee7323d823194d4ec9cfa6e710ed85d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72281832"
---
# <a name="composition-animations"></a>Animations de composition

Les API Windows.UI.Composition vous permettent de créer, d’animer, de transformer et de manipuler des objets compositeur dans une couche API unifiée. Les animations de composition offrent un moyen puissant et efficace d’exécuter des animations dans l’interface utilisateur de votre application. Elles ont été entièrement conçues pour garantir l’exécution de vos animations à 60 FPS, indépendamment du thread d’interface utilisateur, et pour vous offrir la possibilité de créer des expériences totalement inédites en usant de nombreuses propriétés et entrées pour produire les animations en question.

## <a name="motion-in-windows"></a>Motion dans Windows

Imaginez la conception du mouvement comme un film. Les transitions transparentes vous laissent concentrés sur l’histoire et donnent vie aux expériences. Nous pouvons inviter ce sentiment dans notre conception, pour guider les utilisateurs d’une tâche à l'autre avec une aisance cinématographique. Motion est souvent le facteur de différenciation entre une interface utilisateur et une expérience utilisateur.

En tant que bloc de construction fondamental de la plate-forme d’interface utilisateur de Windows, CompositionAnimations offre un moyen puissant et efficace de créer des expériences de mouvement dans l’interface utilisateur de votre application. Le moteur d’animation a été conçu dès le départ pour s’assurer que votre mouvement s’exécute à 60 FPS, indépendamment du thread d’interface utilisateur. Ces animations sont conçues pour offrir la flexibilité nécessaire pour créer des expériences de mouvement novatrices basées sur le temps, l’entrée et d’autres propriétés.

### <a name="examples-of-motion"></a>Exemples de mouvement

Voici quelques exemples de mouvement dans une application.

Ici, une application utilise une animation connectée pour animer une image qui « perdure » pour faire partie de l’en-tête de la page suivante. L’effet aide à conserver le contexte de l’utilisateur pendant toute la transition.

![Exemple d’animation connectée](images/animation/connected-animation-example.gif)

Ici, un effet parallaxe visuel déplace les objets à différentes vitesses lorsque vous faites défiler ou que vous développez l'interface utilisateur, afin de créer un effet de profondeur, de perspective et de mouvement.

![Un exemple de parallaxe avec une liste et une image d’arrière-plan](images/animation/parallax-example.gif)

## <a name="using-compositionanimations-to-create-motion"></a>Utilisation de CompositionAnimations pour créer un mouvement

Pour générer un mouvement dans l’interface utilisateur, les développeurs peuvent accéder aux animations en XAML ou dans la couche visuelle. Les animations au niveau de la couche visuelle offrent aux développeurs une série d’avantages :

- Performances : au lieu de l’animation traditionnelle liée aux threads de l’interface utilisateur, les animations sur la plate-forme d’interface utilisateur Windows fonctionnent sur un thread indépendant à 60 FPS, ce qui permet d’effectuer des expériences de mouvement fluide.
- Modèle de création de modèles : les animations dans la couche d’interface utilisateur Windows sont des modèles, ce qui signifie qu’il est possible d’utiliser une seule animation sur plusieurs objets et de modifier les propriétés ou les paramètres sans vous soucier des utilisations précédentes.
- Personnalisation : la couche d’interface utilisateur Windows facilite non seulement la création d’une belle interface utilisateur, mais avec un large éventail de types d’animations, permettant de créer des expériences étonnantes et étonnantes avec un dégradé de personnalisations.

En tant que développeur qui crée des expériences au niveau de la couche d’interface utilisateur Windows, vous avez accès à un large éventail de concepts d’animation pour donner vie à vos conceptions. Vous pouvez utiliser l’un de ces concepts pour animer un composant de propriété ou de sous-chaîne (le cas échéant) de n’importe quel CompositionObject.

> [!NOTE]
> Toutes les propriétés d’un CompositionObject ne peuvent pas être animées. Reportez-vous à la documentation de chaque CompositionObject pour déterminer si une propriété peut être animée.

> [!NOTE]
> Le terme sous- _canal_ fait référence à une forme de composant d’une propriété. Par exemple, le sous-canal X ou XY d’une propriété de décalage Vector3.

| Concept d’animation | Description |
| ----------------- | ----------- |
| [Mouvement basé sur l’heure avec KeyFrameAnimations](time-animations.md)  | Les KeyFrameAnimations sont utilisés pour contrôler directement l’intégralité d’une expérience de mouvement sur une période donnée. Les développeurs décrivant le début, la fin, l’interpolation d’un mouvement entre et la durée d’une image clé traditionnelle. |
| [Mouvement relatif avec ExpressionAnimations](relation-animations.md)  | Les ExpressionAnimations sont utilisés pour décrire la façon dont une trajectoire de la propriété d’un objet doit être pilotée par rapport à la propriété d’un autre objet. Les développeurs définissent une équation mathématique qui définit la relation basée sur la référence. |
| ImplicitAnimations | Ces animations sont basées sur un déclencheur et sont définies séparément de la logique d’application principale. Les ImplicitAnimations sont utilisés pour décrire comment et quand les animations se produisent en réponse à des modifications de propriétés directes. |
| [Mouvement piloté par les entrées avec des animations d’entrée](input-driven-animations.md)  | Les animations d’entrée couvrent un ensemble de scénarios qui permettent aux développeurs de décrire le mouvement basé sur la manipulation via Touch ou d’autres modalités d’entrée. Ces animations sont pilotées en fonction de l’entrée ou des gestes actifs de l’utilisateur. |
| [Mouvement basé sur la physique avec NaturalMotionAnimations](natural-animations.md)  | Les NaturalMotionAnimations sont utilisés pour décrire une expérience de mouvement naturelle et familière basée sur un mouvement de force réel. Plutôt que de définir le temps, les développeurs définissent les caractéristiques du mouvement (par exemple, le ratio d’amortissement des ressorts) |