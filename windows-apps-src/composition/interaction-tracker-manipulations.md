---
title: Manipulations personnalisées avec InteractionTracker
description: Utilisez les API InteractionTracker pour créer des expériences de manipulation personnalisées.
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP, animation
ms.localizationpriority: medium
ms.openlocfilehash: 8a4b682f009a4ac1350ceee3b8c23fe5e772150d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163593"
---
# <a name="custom-manipulation-experiences-with-interactiontracker"></a>Expériences de manipulation personnalisées avec InteractionTracker

Dans cet article, nous expliquons comment utiliser InteractionTracker pour créer des expériences de manipulation personnalisées.

## <a name="prerequisites"></a>Prérequis

Ici, nous partons du principe que vous êtes familiarisé avec les concepts abordés dans les articles suivants :

- [Animations pilotées par une entrée](input-driven-animations.md)
- [Animations basées sur les relations](relation-animations.md)

## <a name="why-create-custom-manipulation-experiences"></a>Pourquoi créer des expériences de manipulation personnalisées ?

Dans la plupart des cas, l’utilisation des contrôles de manipulation prédéfinis est suffisamment bonne pour créer des expériences d’interface utilisateur. Mais que se passe-t-il si vous souhaitez distinguer les contrôles communs ? Que se passe-t-il si vous souhaitez créer une expérience spécifique basée sur l’entrée ou si vous avez une interface utilisateur où un mouvement de manipulation traditionnel n’est pas suffisant ? C’est là qu’intervient la création d’expériences personnalisées. Ils permettent aux développeurs d’applications et aux concepteurs d’être plus créatifs : apportez à des expériences de mouvement qui mieux illustrer leur personnalisation et leur langage de conception personnalisé. Dès le départ, vous avez accès à l’ensemble approprié de blocs de construction pour personnaliser complètement une expérience de manipulation, de la façon dont le mouvement doit répondre avec le doigt sur l’écran pour aligner les points et le chaînage d’entrée.

Voici quelques exemples courants de la création d’une expérience de manipulation personnalisée :

- Ajout d’un comportement de balayage, de suppression et d’ignorer personnalisé
- Effets pilotés par entrée (panoramique entraînant le flou du contenu)
- Contrôles personnalisés avec mouvements de manipulation personnalisés (ListView personnalisé, ScrollViewer, etc.)

![Exemple de défilement par balayage](images/animation/swipe-scroller.gif)

![Exemple d’extraction vers l’animation](images/animation/pull-to-animate.gif)

## <a name="why-use-interactiontracker"></a>Pourquoi utiliser InteractionTracker ?

InteractionTracker a été introduit dans l’espace de noms Windows. UI. composition. interactions dans la version 10586 du kit de développement logiciel (SDK). InteractionTracker permet :

- **Flexibilité totale** : nous souhaitons pouvoir personnaliser et adapter chaque aspect d’une expérience de manipulation ; plus précisément, les mouvements exacts qui se produisent pendant ou en réponse à l’entrée. Lorsque vous créez une expérience de manipulation personnalisée avec InteractionTracker, tous les boutons dont vous avez besoin sont à votre disposition.
- **Performances fluides** : l’un des défis liés aux manipulations est que leurs performances dépendent du thread d’interface utilisateur. Cela peut avoir un impact négatif sur les manipulations lorsque l’interface utilisateur est occupée. InteractionTracker a été conçu pour utiliser le nouveau moteur d’animation qui fonctionne sur un thread indépendant à 60 FPS, ce qui se traduit par un mouvement en douceur.

## <a name="overview-interactiontracker"></a>Vue d’ensemble : InteractionTracker

Lorsque vous créez des expériences de manipulation personnalisées, il existe deux principaux composants avec lesquels vous interagissez. Nous aborderons les éléments suivants :

- [InteractionTracker](/uwp/api/windows.ui.composition.interactions.interactiontracker) : objet principal conservant un ordinateur d’État dont les propriétés sont pilotées par une entrée d’utilisateur active ou des mises à jour directes et des animations. Elle est ensuite liée à un CompositionAnimation pour créer le mouvement de manipulation personnalisé.
- [VisualInteractionSource](/uwp/api/windows.ui.composition.interactions.visualinteractionsource) : objet de complément qui définit quand et dans quelles conditions l’entrée est envoyée à InteractionTracker. Il définit à la fois le CompositionVisual utilisé pour le test de positionnement, ainsi que d’autres propriétés de configuration d’entrée.

En tant que machine d’État, les propriétés de InteractionTracker peuvent être pilotées par l’un des éléments suivants :

- Interaction directe de l’utilisateur : l’utilisateur final manipule directement dans la région de test de positionnement VisualInteractionSource
- Inertie : à partir d’une vélocité par programme ou d’un mouvement utilisateur, propriétés de InteractionTracker animer sous une courbe d’inertie
- CustomAnimation : animation personnalisée ciblant directement une propriété de InteractionTracker

### <a name="interactiontracker-state-machine"></a>Machine à États InteractionTracker

Comme mentionné précédemment, InteractionTracker est une machine à États avec 4 États, chacun d’entre eux pouvant passer à l’un des quatre autres États. (Pour plus d’informations sur la façon dont InteractionTracker passe d’un État à l’autre, consultez la documentation relative à la classe [InteractionTracker](/uwp/api/windows.ui.composition.interactions.interactiontracker) .)

| State | Description |
|-------|-------------|
| Idle | Pas d’entrée active, de conduite ou d’animations |
| Interaction | Entrée utilisateur active détectée |
| Inertie | Mouvement actif résultant d’une vitesse d’entrée ou de programmation active |
| CustomAnimation | Mouvement actif résultant d’une animation personnalisée |

Dans chacun des cas où l’état de InteractionTracker change, un événement (ou un rappel) est généré que vous pouvez écouter. Pour que vous puissiez écouter ces événements, ils doivent implémenter l’interface [IInteractionTrackerOwner](/uwp/api/windows.ui.composition.interactions.iinteractiontrackerowner) et créer leur objet InteractionTracker avec la méthode CreateWithOwner. Le diagramme suivant présente également le moment où les différents événements sont déclenchés.

![Machine à États InteractionTracker](images/animation/interaction-tracker-diagram.png)

## <a name="using-the-visualinteractionsource"></a>Utilisation de VisualInteractionSource

Pour que InteractionTracker soit piloté par l’entrée, vous devez connecter un VisualInteractionSource (VIS) à celui-ci. L’VIS est créée en tant qu’objet de complément à l’aide d’un CompositionVisual pour définir :

1. La région de test d’atteinte dans laquelle l’entrée sera suivie et les mouvements de l’espace de coordonnées sont détectés dans
1. Les configurations d’entrée qui vont être détectées et routées sont les suivantes :
    - Mouvements détectables : position X et Y (panoramique horizontal et vertical), mise à l’échelle (pincement)
    - Inertie
    - Rail & chaînage
    - Modes de redirection : données d’entrée redirigées automatiquement vers InteractionTracker

> [!NOTE]
> Étant donné que le VisualInteractionSource est créé en fonction de la position du test d’atteinte et de l’espace de coordonnées d’un visuel, il est recommandé de ne pas utiliser un visuel qui sera en mouvement ou en changeant de position.

> [!NOTE]
> Vous pouvez utiliser plusieurs instances VisualInteractionSource avec le même InteractionTracker s’il existe plusieurs régions de test de positionnement. Toutefois, le cas le plus courant consiste à n’utiliser qu’un seul VIS.

Le VisualInteractionSource est également chargé de gérer le moment où les données d’entrée issues de différentes modalités (tactiles, PTP, Pen) sont routées vers InteractionTracker. Ce comportement est défini par la propriété ManipulationRedirectionMode. Par défaut, toutes les entrées de pointeur sont envoyées au thread d’interface utilisateur et l’entrée du pavé tactile de précision passe à VisualInteractionSource et InteractionTracker.

Par conséquent, si vous souhaitez que le toucher et le stylet (Creators Update) effectuent une manipulation via un VisualInteractionSource et InteractionTracker, vous devez appeler la méthode VisualInteractionSource. TryRedirectForManipulation. Dans l’extrait de code abrégé ci-dessous d’une application XAML, la méthode est appelée lorsqu’un événement tactile appuyé se produit dans la grille du plus haut UIElement :

```csharp
private void root_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Touch)
    {
        _source.TryRedirectForManipulation(e.GetCurrentPoint(root));
    }
}
```

## <a name="tie-in-with-expressionanimations"></a>Associer à ExpressionAnimations

Lors de l’utilisation de InteractionTracker pour piloter une expérience de manipulation, vous interagissez principalement avec les propriétés de mise à l’échelle et de position. Comme les autres propriétés CompositionObject, ces propriétés peuvent être à la fois la cible et référencées dans un CompositionAnimation, le plus souvent ExpressionAnimations.

Pour utiliser InteractionTracker dans une expression, référencez la propriété position (ou mise à l’échelle) du dispositif de suivi comme dans l’exemple ci-dessous. Comme la propriété de InteractionTracker est modifiée en raison de l’une des conditions décrites précédemment, la sortie de l’expression change également.

```csharp
// With Strings
var opacityExp = _compositor.CreateExpressionAnimation("-tracker.Position");
opacityExp.SetReferenceParameter("tracker", _tracker);

// With ExpressionBuilder
var opacityExp = -_tracker.GetReference().Position;
```

> [!NOTE]
> Lorsque vous faites référence à la position de InteractionTracker dans une expression, vous devez nier la valeur de l’expression résultante pour se déplacer dans le sens approprié. En effet, la progression de InteractionTracker de l’origine sur un graphique et vous permet de réfléchir à la progression de InteractionTracker dans les coordonnées réelles, telles que la distance par rapport à son origine.

## <a name="get-started"></a>Démarrer

Pour commencer à utiliser InteractionTracker pour créer des expériences de manipulation personnalisées :

1. Créez votre objet InteractionTracker à l’aide de InteractionTracker. Create ou InteractionTracker. CreateWithOwner.
    - (Si vous utilisez CreateWithOwner, veillez à implémenter l’interface IInteractionTrackerOwner.)
1. Définissez la position minimale et maximale de votre InteractionTracker nouvellement créée.
1. Créez votre VisualInteractionSource avec un CompositionVisual.
    - Assurez-vous que le visuel que vous transmettez a une taille différente de zéro. Dans le cas contraire, il n’obtiendra pas de test de positionnement correct.
1. Définissez les propriétés de VisualInteractionSource.
    - VisualInteractionSourceRedirectionMode
    - PositionXSourceMode, PositionYSourceMode, ScaleSourceMode
    - Rail & chaînage
1. Ajoutez VisualInteractionSource à InteractionTracker à l’aide de InteractionTracker. InteractionSources. Add.
1. Configurer TryRedirectForManipulation pour la détection de l’entrée tactile et du stylet.
    - Pour XAML, cette opération est généralement effectuée sur l’événement PointerPressed de l’UIElement.
1. Créez un ExpressionAnimation qui fait référence à la position de InteractionTracker et ciblez la propriété d’un CompositionObject.

Voici un extrait de code succinct qui indique #1-5 en action :

```csharp
private void InteractionTrackerSetup(Compositor compositor, Visual hitTestRoot)
{ 
    // #1 Create InteractionTracker object
    var tracker = InteractionTracker.Create(compositor);

    // #2 Set Min and Max positions
    tracker.MinPosition = new Vector3(-1000f);
    tracker.MaxPosition = new Vector3(1000f);

    // #3 Setup the VisualInteractionSource
    var source = VisualInteractionSource.Create(hitTestRoot);

    // #4 Set the properties for the VisualInteractionSource
    source.ManipulationRedirectionMode =
        VisualInteractionSourceRedirectionMode.CapableTouchpadOnly;
    source.PositionXSourceMode = InteractionSourceMode.EnabledWithInertia;
    source.PositionYSourceMode = InteractionSourceMode.EnabledWithInertia;

    // #5 Add the VisualInteractionSource to InteractionTracker
    tracker.InteractionSources.Add(source);
}
```

Pour obtenir des utilisations plus avancées de InteractionTracker, consultez les articles suivants :

- [Créer des points d’alignement avec InertiaModifiers](inertia-modifiers.md)
- [Extraction à actualiser avec SourceModifiers](source-modifiers.md)