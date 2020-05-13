---
title: Moderniser votre application de bureau en utilisant la couche visuelle
description: Utilisez la couche visuelle pour améliorer l’interface utilisateur de votre application de bureau Win32 ou .NET.
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: 33a5f0bc31a8fe1421f7ab0de5f229d2feb77915
ms.sourcegitcommit: ef723e3d6b1b67213c78da696838a920c66d5d30
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/02/2020
ms.locfileid: "82730136"
---
# <a name="using-the-visual-layer-in-desktop-apps"></a>Utilisation de la couche visuelle dans les applications de bureau

Vous pouvez maintenant utiliser des API Windows Runtime dans des applications de bureau non conçues pour UWP. Ces API vous permettent d’améliorer l’apparence, le comportement et les fonctionnalités de vos applications WPF, Windows Forms et Win32 C++, mais aussi de bénéficier des toutes dernières fonctionnalités d’interface utilisateur de Windows 10 qui sont disponibles uniquement par le biais d’UWP.

Dans beaucoup de scénarios, vous pouvez utiliser les [îlots XAML](xaml-islands.md) pour ajouter des contrôles XAML modernes à votre application. Toutefois, si vous devez créer des expériences utilisateur personnalisées plus avancées que les contrôles intégrés, vous pouvez faire appel aux API de la couche visuelle.

La couche visuelle fournit une API en mode retenu très performante pour les graphismes, les effets et les animations. Elle est la base de l’interface utilisateur sur les appareils Windows 10. Les contrôles XAML UWP sont basés sur la couche visuelle, ce qui permet de gérer de nombreux aspects du [système Fluent Design](/windows/uwp/design/fluent-design-system/index), tels que la lumière, la profondeur, le mouvement, la matière et la mise à l’échelle.

![Interface utilisateur créée avec la couche visuelle](images/visual-layer-interop/pull-to-animate.gif)

> _Interface utilisateur créée avec la couche visuelle_

## <a name="create-a-visually-engaging-user-interface-in-any-windows-app"></a>Créer une interface utilisateur attractive et esthétique dans les applications Windows

La couche visuelle vous permet de créer des expériences attractives en utilisant une composition légère de contenu dessiné personnalisé (les visuels) et en ajoutant des animations, des effets et des manipulations percutants à ces objets dans votre application. La couche visuelle ne remplace aucune des infrastructures d’interface utilisateur existantes ; elle est plutôt un complément qui améliore ces infrastructures.

Utilisez la couche visuelle pour personnaliser l’apparence de votre application et conférer à celle-ci une identité forte qui la distingue des autres applications. De plus, la couche visuelle applique les principes du Fluent Design, qui visent à faciliter l’utilisation de vos applications et encourager ainsi l’engagement des utilisateurs. Par exemple, vous pouvez l’utiliser pour créer des signaux visuels et des transitions d’écran animées qui montrent les relations entre les différents éléments à l’écran.

## <a name="visual-layer-features"></a>Fonctionnalités de la couche visuelle

### <a name="brushes"></a>Pinceaux

Avec les [pinceaux de composition](/windows/uwp/composition/composition-brushes), vous peignez des objets d’interface utilisateur auxquels vous pouvez ajouter des couleurs unies, des dégradés, des images, des vidéos, des effets complexes et bien d’autres éléments.

![Œuf créé avec Material Creator](images/visual-layer-interop/egg.gif)

> _Œuf créé avec l’[application de démonstration Material Creator](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/Demos/MaterialCreator)._

### <a name="effects"></a>Effets

Les [effets de composition](/windows/uwp/composition/composition-effects) incluent la lumière, l’ombre et différents effets de filtre. Ils peuvent être animés, personnalisés et chaînés, puis appliqués directement aux visuels. L’effet SceneLightingEffect peut être combiné avec la lumière de composition pour créer une ambiance, une profondeur et une matière.

![Lumière et matière](images/visual-layer-interop/light-interop.gif)

> _Lumière et matière illustrés dans la [Galerie d’exemples de composition d’interface utilisateur Windows](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/SampleGallery)._

### <a name="animations"></a>Animations

Les [animations de composition](/windows/uwp/composition/composition-animation) s’exécutent directement dans le processus du compositeur, indépendamment du thread d’interface utilisateur. Cela garantit la fluidité et la mise à l’échelle des animations quand vous exécutez simultanément un grand nombre d’animations explicites. En plus des animations d’images clés familières pour gérer les modifications de propriétés dans le temps, vous pouvez utiliser des expressions pour établir des relations mathématiques entre les différentes propriétés, y compris les entrées utilisateur. Avec les animations basées sur des entrées, vous créez une interface utilisateur qui répond de manière dynamique et fluide aux entrées utilisateur, ce qui favorise l’engagement de l’utilisateur.

![Interface utilisateur créée avec la couche visuelle](images/visual-layer-interop/swipe-scroller.gif)

> _Mouvement illustré dans la [Galerie d’exemples de composition d’interface utilisateur Windows](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/SampleGallery)._

## <a name="keep-your-existing-codebase-and-adopt-incrementally"></a>Conservation de votre codebase existant et adoption incrémentielle

Le code de vos applications existantes est le fruit d’un gros investissement que vous ne voudrez sans doute pas perdre. Vous pouvez migrer des _îlots_ de contenu pour utiliser la couche visuelle et conserver le reste de l’interface utilisateur dans son infrastructure existante. Autrement dit, vous pouvez apporter d’importantes mises à jour et améliorations à l’interface utilisateur de votre application sans avoir à modifier de grandes parties de votre codebase actuel.

## <a name="samples-and-tutorials"></a>Exemples et tutoriels

Découvrez comment utiliser la couche visuelle dans vos applications en expérimentant nos exemples. Les exemples et tutoriels mis à votre disposition vous familiariseront avec la couche visuelle et vous montreront comment utiliser les différentes fonctionnalités.

### <a name="win32"></a>Win32

- Tutoriel [Utilisation de la couche visuelle avec Win32](using-the-visual-layer-with-win32.md)
  - [Exemple HelloComposition](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)
- [Exemple HelloVectors](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloVectors)
- [Exemple VirtualSurfaces](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/VirtualSurfaces)
- [Exemple ScreenCapture](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/ScreenCaptureforHWND)

### <a name="windows-forms"></a>Windows Forms

- Tutoriel [Utilisation de la couche visuelle avec Windows Forms](using-the-visual-layer-with-windows-forms.md)
  - [Exemple HelloComposition](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition)
- [Exemple d’intégration de couche visuelle](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration)

### <a name="wpf"></a>WPF

- Tutoriel [Utilisation de la couche visuelle avec WPF](using-the-visual-layer-with-wpf.md)
  - [Exemple HelloComposition](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/HelloComposition)
- [Exemple VisualLayerIntegration](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/VisualLayerIntegration)
- [Exemple ScreenCapture](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/ScreenCapture)

## <a name="limitations"></a>Limitations

La plupart des fonctionnalités de la couche visuelle ont le même comportement quand elles sont hébergées dans une application de bureau ou dans une application UWP, mais certaines fonctionnalités présentent des limitations. Voici quelques-unes des limitations à connaître :

- Les chaînes d’effets s’appuient sur [Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm) pour les descriptions des effets. Le [package NuGet Win2D](https://www.nuget.org/packages/Win2D.uwp) n’est pas pris en charge dans les applications de bureau. Vous devez donc le recompiler à partir du [code source](https://github.com/Microsoft/Win2D).
- Pour effectuer un test de positionnement, vous devez effectuer des calculs de limites en parcourant manuellement l’arborescence des visuels. C’est la même chose que la couche visuelle dans UWP, sauf qu’ici, il n’y a pas d’élément XAML que vous pouvez lier facilement pour le test de positionnement.
- La couche visuelle n’a pas de primitive de rendu du texte.
- Quand deux technologies d’interface utilisateur différentes sont utilisées ensemble, telles que WPF et la couche visuelle, elles gèrent chacune le dessin de leurs propres pixels à l’écran, mais elles ne peuvent pas partager de pixels. Par conséquent, le contenu de la couche visuelle s’affiche toujours au-dessus des autres contenus d’interface utilisateur. (C’est le problème d’_espace de rendu_.) Vous devrez peut-être ajouter du code ou faire des tests supplémentaires pour vous assurer que le contenu de la couche visuelle se redimensionne correctement avec l’interface utilisateur hôte et qu’il n’occulte aucun autre contenu.
- Le contenu hébergé dans une application de bureau n’est pas automatiquement redimensionné ou mis à l’échelle pour la résolution DPI. Vous devrez peut-être effectuer des étapes en plus pour vérifier que votre contenu gère les modifications de DPI. (Pour plus d’informations, consultez les tutoriels propres à la plateforme.)

## <a name="additional-resources"></a>Ressources supplémentaires

- [Couche visuelle](/windows/uwp/composition/visual-layer)
- [Visuel de composition](/windows/uwp/composition/composition-visual-tree)
- [Pinceaux de composition](/windows/uwp/composition/composition-brushes)
- [Effets de composition](/windows/uwp/composition/composition-effects)
- [Animations de composition](/windows/uwp/composition/composition-animation)

Informations de référence sur les API

- [Windows.UI.Composition](/uwp/api/Windows.UI.Composition)