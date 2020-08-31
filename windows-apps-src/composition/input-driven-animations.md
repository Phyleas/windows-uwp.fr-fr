---
title: Animations pilotées par une entrée
description: En savoir plus sur l’API InputAnimation et sur l’utilisation des animations pilotées par les entrées pour créer des mouvements de réponse dynamique dans l’interface utilisateur de votre application.
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP, animation
ms.localizationpriority: medium
ms.openlocfilehash: 0f9abd902e39b645f27b7a0f5d521097ca4aff8a
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89054359"
---
# <a name="input-driven-animations"></a>Animations pilotées par une entrée

Cet article fournit une introduction à l’API InputAnimation et vous recommande d’utiliser ces types d’animations dans votre interface utilisateur.

## <a name="prerequisites"></a>Prérequis

Ici, nous partons du principe que vous êtes familiarisé avec les concepts abordés dans les articles suivants :

- [Animations basées sur les relations](relation-animations.md)

## <a name="smooth-motion-driven-from-user-interactions"></a>Mouvement lisse piloté par les interactions de l’utilisateur

Dans le langage de conception Fluent, les interactions entre les utilisateurs finaux et les applications sont d’une importance capitale. Les applications doivent non seulement examiner la partie, mais également répondre naturellement et de manière dynamique aux utilisateurs qui interagissent avec eux. Cela signifie que lorsqu’un doigt est placé sur l’écran, l’interface utilisateur doit réagir de manière appropriée à l’évolution des degrés d’entrée ; le défilement doit être lisse et s’embout d’un doigt panoramique sur l’écran.

La création d’une interface utilisateur qui répond de manière dynamique et fluide à l’entrée de l’utilisateur entraîne une plus grande augmentation du mouvement des utilisateurs. en effet, elle semble correcte et naturelle quand les utilisateurs interagissent avec vos différentes expériences d’interface utilisateur. Cela permet aux utilisateurs finaux de se connecter plus facilement à votre application, ce qui rend l’expérience plus mémorable et délicieuse.

## <a name="expanding-past-just-touch"></a>Développement passé juste

Bien que Touch soit l’une des interfaces les plus courantes que les utilisateurs finaux utilisent pour manipuler le contenu de l’interface utilisateur, ils utilisent également diverses autres modalités d’entrée telles que la souris et le stylet. Dans ce cas, il est important que les utilisateurs finaux perçoivent que votre interface utilisateur répond dynamiquement à leur entrée, quelle que soit la modalité d’entrée qu’elles choisissent d’utiliser. Vous devez être Cognizant des différentes modalités d’entrée lors de la conception d’expériences de mouvement pilotées par l’entrée.

## <a name="different-input-driven-motion-experiences"></a>Différentes expériences pilotées par l’entrée

L’espace InputAnimation offre plusieurs expériences différentes qui vous permettent de créer des mouvements de réponse dynamique. Comme le reste du système d’animation de l’interface utilisateur Windows, ces animations pilotées par les entrées fonctionnent sur un thread indépendant, ce qui contribue à contribuer à l’expérience de mouvement dynamique. Toutefois, dans certains cas où l’expérience s’appuie sur des contrôles et des composants XAML existants, les parties de ces expériences sont toujours liées au thread d’interface utilisateur.

Il existe trois expériences principales que vous créez lors de la création d’animations de mouvement dynamiques pilotées par entrée :

1. Amélioration des expériences ScrollView existantes : active la position d’un ScrollViewer XAML pour piloter les expériences d’animation dynamiques.
1. Expériences pilotées par position de pointeur : utilisez la position d’un curseur sur un UIElement testé par positionnement pour conduire des expériences d’animation dynamiques.
1. Expériences de manipulation personnalisée avec InteractionTracker : créez des expériences de manipulation hors thread entièrement personnalisées avec InteractionTracker (par exemple, un canevas de défilement/zoom).

## <a name="enhancing-existing-scrollviewer-experiences"></a>Amélioration des expériences ScrollViewer existantes

L’une des méthodes courantes pour créer des expériences plus dynamiques consiste à générer sur un contrôle ScrollViewer XAML existant. Dans ces situations, vous tirez parti de la position de défilement d’un ScrollViewer pour créer des composants d’interface utilisateur supplémentaires qui rendent une expérience de défilement simple plus dynamique. Voici quelques exemples : en-têtes rémanents/discrets et parallaxe.

![Mode liste avec parallaxe](images/animation/parallax.gif)

![En-tête discret](images/animation/shy-header.gif)

Lors de la création de ces types d’expériences, il existe une formule générale à suivre :

1. Accédez à ScrollManipulationPropertySet à partir du ScrollViewer XAML pour lequel vous souhaitez piloter une animation.
    - Effectuée via l’API ElementCompositionPreview. GetScrollViewerManipulationPropertySet (élément UIElement)
    - Retourne un CompositionPropertySet contenant une propriété appelée **translation**
1. Créez un ExpressionAnimation de composition avec une équation qui fait référence à la propriété translation.
1. Démarrez l’animation sur la propriété d’un CompositionObject.

Pour plus d’informations sur la création de ces expériences, consultez [améliorer les expériences ScrollViewer existantes](scroll-input-animations.md).

## <a name="pointer-position-driven-experiences"></a>Expériences pilotées par position de pointeur

Une autre expérience dynamique courante impliquant l’entrée consiste à conduire une animation en fonction de la position d’un pointeur comme un curseur de la souris. Dans ces situations, les développeurs tirent parti de l’emplacement d’un curseur lorsqu’ils effectuent des tests d’atteinte au sein d’un UIElement XAML qui rend possible la création d’expériences comme Spotlight.

![Exemple de Spotlight de pointeur](images/animation/spotlight-reveal.gif)

![Exemple de rotation du pointeur](images/animation/pointer-rotate.gif)

Lors de la création de ces types d’expériences, il existe une formule générale à suivre :

1. Accédez à PointerPositionPropertySet à partir d’un UIElement XAML qui vous permet de connaître la position du curseur lors d’un test d’atteinte.
    - Effectuée via l’API ElementCompositionPreview. GetPointerPositionPropertySet (élément UIElement)
    - Retourne un CompositionPropertySet contenant une propriété appelée **position**
1. Créez un CompositionExpressionAnimation avec une équation qui fait référence à la propriété position.
1. Démarrez l’animation sur la propriété d’un CompositionObject.

## <a name="custom-manipulation-experiences-with-interactiontracker"></a>Expériences de manipulation personnalisées avec InteractionTracker

L’un des défis liés à l’utilisation d’un ScrollViewer XAML est qu’il est lié au thread d’interface utilisateur. Par conséquent, l’expérience de défilement et de zoom peut souvent être décalée et instable si le thread d’interface utilisateur est occupé et entraîne une expérience inversée. En outre, il n’est pas possible de personnaliser de nombreux aspects de l’expérience ScrollViewer. InteractionTracker a été créé pour résoudre les deux problèmes en fournissant un ensemble de blocs de construction pour créer des expériences de manipulation personnalisées qui sont exécutées sur un thread indépendant.

![exemple d’interactions 3D](images/animation/interactions-3d.gif)

![Exemple d’extraction vers l’animation](images/animation/pull-to-animate.gif)

Lors de la création d’expériences avec InteractionTracker, il existe une formule générale à suivre :

1. Créez votre objet InteractionTracker et définissez ses propriétés.
1. Créez VisualInteractionSources pour tous les CompositionVisual qui doivent capturer l’entrée pour InteractionTracker à consommer.
1. Créez un ExpressionAnimation de composition avec une équation qui fait référence à la propriété position de InteractionTracker.
1. Démarrez l’animation sur la propriété d’un visuel de composition que vous souhaitez pilotée par InteractionTracker.
