---
ms.assetid: a2751e22-6842-073a-daec-425fb981bafe
title: Couche visuelle
description: L’API Windows.UI.Composition vous donne accès à la couche de composition comprise entre la couche d’infrastructure (XAML) et la couche graphique (DirectX).
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3cdd7c17bc0d419a7449b366b620a4d5654cccc4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89160723"
---
# <a name="visual-layer"></a>Couche visuelle

La couche visuelle fournit une API haute performance, en mode mémorisé pour les graphiques, les effets et les animations, et constitue la base de toute l’interface utilisateur sur les appareils Windows.Vous définissez votre interface utilisateur de façon déclarative et la couche visuelle s’appuie sur l’accélération matérielle graphique pour s’assurer que le contenu, les effets et les animations sont rendus de manière lisse et sans problème, indépendamment du thread d’interface utilisateur de l’application.

Principales mises en évidence :

* API WinRT familières
* Conçu pour une interface utilisateur et des interactions plus dynamiques
* Concepts alignés avec les outils de conception
* Évolutivité linéaire sans falaise soudaine sur les performances

Vos applications UWP Windows utilisent déjà la couche visuelle via l’un des frameworks d’interface utilisateur. Vous pouvez également tirer parti de la couche visuelle directement pour le rendu personnalisé, les effets et les animations avec très peu d’efforts.

![Superposition d’infrastructure d’interface utilisateur : la couche d’infrastructure (Windows. UI. XAML) repose sur la couche visuelle (Windows. UI. composition) qui repose sur la couche Graphics (DirectX).](images/layers-win-ui-composition.png)

## <a name="whats-in-the-visual-layer"></a>Contenu de la couche visuelle

Les fonctions principales de la couche visuelle sont les suivantes :

1. **Contenu**: composition légère de contenu dessiné personnalisé
1. **Effets**: système d’effets d’interface utilisateur en temps réel dont les effets peuvent être animés, chaînés et personnalisés
1. **Animations**: animations expressifs et non indépendantes de l’infrastructure s’exécutant indépendamment du thread d’interface utilisateur

### <a name="content"></a>Contenu

Le contenu est hébergé, transformé et rendu disponible pour être utilisé par le système d’animation et d’effets à l’aide d’éléments visuels. À la base de la hiérarchie de classes se trouve la classe [**Visual**](/uwp/api/Windows.UI.Composition.Visual) , un proxy léger thread-agile dans le processus d’application pour l’état visuel dans le compositeur. Les sous-classes de Visual incluent  [**ContainerVisual**](/uwp/api/Windows.UI.Composition.ContainerVisual) pour permettre aux enfants de créer des arborescences d’éléments visuels et [**SpriteVisual**](/uwp/api/Windows.UI.Composition.SpriteVisual) qui contiennent du contenu et qui peuvent être peints avec des couleurs unies, du contenu dessiné personnalisé ou des effets visuels. Ensemble, ces types d’éléments visuels composent la structure de l’arborescence visuelle pour l’interface utilisateur 2D et les FrameworkElement XAML les plus visibles.

Pour plus d’informations, consultez vue d’ensemble visuelle de la [composition](composition-visual-tree.md) .

### <a name="effects"></a>Effects (Effets)

Le système Effects de la couche visuelle vous permet d’appliquer une chaîne d’effets de filtre et de transparence à un visuel ou à une arborescence d’éléments visuels. Il s’agit d’un système d’effets d’interface utilisateur, à ne pas confondre avec les effets d’image et de support. Les effets fonctionnent conjointement avec le système d’animation, permettant aux utilisateurs d’obtenir des animations lisses et dynamiques des propriétés d’effet, rendues indépendantes du thread d’interface utilisateur. Les effets de la couche visuelle fournissent les blocs de construction créatifs qui peuvent être combinés et animés pour construire des expériences personnalisées et interactives.

En plus des chaînes d’effet pouvant être animées, la couche visuelle prend également en charge un modèle d’éclairage qui permet aux visuels de simuler des propriétés de matériau en répondant aux lumières pouvant être animées. Les visuels peuvent également converser des ombres. L’éclairage et les ombres peuvent être combinés pour créer une perception de la profondeur et du réalisme.

Pour plus d’informations, consultez la vue d’ensemble [Effets de composition](composition-effects.md).

### <a name="animations"></a>Animations

Le système d’animation de la couche visuelle vous permet de déplacer des éléments visuels, d’animer des effets et de générer des transformations, des clips et d’autres propriétés.Il s’agit d’un système agnostique du cadre qui a été conçu dès le départ, avec des performances à l’esprit.Il s’exécute indépendamment du thread d’interface utilisateur pour assurer la fluidité et l’évolutivité.Bien qu’il vous permette d’utiliser des animations d’image clé familières pour piloter les modifications de propriétés dans le temps, il vous permet également de définir des relations mathématiques entre différentes propriétés, y compris les entrées utilisateur, vous permettant de créer directement des expériences chorégraphiés transparents.

Pour plus d’informations, consultez la vue d’ensemble [Animations de composition](composition-animation.md).

### <a name="working-with-your-xaml-uwp-app"></a>Utilisation de votre application UWP XAML

Vous pouvez accéder à un visuel créé par l’infrastructure XAML, et en sauvegardant un FrameworkElement visible, à l’aide de la classe [**ElementCompositionPreview**](/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview) dans [**Windows. UI. Xaml. Hosting**](/uwp/api/Windows.UI.Xaml.Hosting). Notez que les visuels créés pour vous par l’infrastructure sont fournis avec des limites de personnalisation. Cela est dû au fait que le Framework gère les décalages, les transformations et les durées de vie. Vous pouvez toutefois créer vos propres visuels et les attacher à un élément XAML existant via ElementCompositionPreview, ou en les ajoutant à un ContainerVisual existant dans l’arborescence d’éléments visuels.

Pour plus d’informations, consultez la vue d’ensemble de l' [utilisation de la couche visuelle avec XAML](using-the-visual-layer-with-xaml.md) .

### <a name="working-with-your-desktop-app"></a>Utilisation de votre application de bureau

Vous pouvez utiliser la couche visuelle pour améliorer l’apparence, la convivialité et les fonctionnalités de vos applications de bureau WPF, Windows Forms et C++ Win32. Vous pouvez migrer des îlots de contenu pour utiliser la couche visuelle et conserver le reste de l’interface utilisateur dans son infrastructure existante. Autrement dit, vous pouvez apporter d’importantes mises à jour et améliorations à l’interface utilisateur de votre application sans avoir à modifier de grandes parties de votre codebase actuel.

Pour plus d’informations, consultez [Moderniser votre application de bureau à l’aide de la couche Visuel](/windows/apps/desktop/modernize/visual-layer-in-desktop-apps).

## <a name="additional-resources"></a>Ressources supplémentaires

* [**Documentation de référence complète sur l’API**](/uwp/api/Windows.UI.Composition)
* Exemples d’interface utilisateur et de composition avancés dans le [GitHub WindowsUIDevLabs](https://github.com/microsoft/WindowsCompositionSamples).
* [Galerie d’exemples Windows. UI. composition](https://www.microsoft.com/store/apps/9pp1sb5wgnww)
* [@windowsui Flux Twitter ](https://twitter.com/windowsui)
* Lisez l’article MSDN de Kenny Kerr sur cette API : [Graphismes et animation : l’API Composition Windows passe à Windows 10](/archive/msdn-magazine/2015/windows-10-special-issue/graphics-and-animation-windows-composition-turns-10)