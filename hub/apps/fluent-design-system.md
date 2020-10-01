---
description: Découvrez le système Fluent Design dans la plateforme Windows universelle (UWP) et comment l’intégrer à vos applications.
title: Système Fluent Design pour Windows
keywords: disposition d’application uwp, plateforme windows universelle, conception d’application, interface, système fluent design
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.author: mcleans
author: mcleanbyron
ms.openlocfilehash: 8a05a0a9aeb3a31e76c0510eef70b5ee3036d2f7
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91216831"
---
# <a name="the-fluent-design-system-for-windows-app-creators"></a>Système Fluent Design pour créateurs d’applications Windows

![En-tête Fluent Design](images/fluent/fluentdesign-app-header.png)

## <a name="introduction"></a>Introduction

Fluent Design est un système de création d’interfaces utilisateur adaptatives, intuitives et esthétiques.

## <a name="principles"></a>Principes

**Adaptive : les expériences Fluent paraissent naturelles sur tous les appareils**

Les expériences Fluent s’adaptent à l’environnement. Une expérience Fluent est agréable aussi bien sur une tablette que sur un PC ou une Xbox. Elle fonctionne même parfaitement sur un casque de réalité mixte. Lorsque vous ajoutez du matériel, comme un nouveau moniteur pour votre PC, l’expérience Fluent en tire parti.

**Intuitive : les expériences Fluent sont intuitives et puissantes**

Les expériences Fluent s’ajustent au comportement et aux intentions. Elles comprennent et anticipent ce dont l’utilisateur a besoin. Elles unissent les personnes et les idées, qu’elles soient géographiquement proches ou éloignées.

**Esthétique : les expériences Fluent sont attractives et immersives**

En intégrant des éléments du monde physique, une expérience Fluent exploite quelque chose de fondamental. Elle utilise la lumière, l’ombre, le mouvement, la profondeur et la texture pour organiser les informations d’une façon qui semble intuitive et instinctive.


## <a name="applying-fluent-design-to-your-app-with-uwp"></a>Utilisation de Fluent Design dans votre application avec UWP

![Logo Fluent Design](images/fluent/fluentdesign_header.png)

Nos instructions de conception expliquent comment appliquer les principes de Fluent Design à vos applications. Pour quel type d’applications ? Même si la plupart de nos instructions peuvent s’appliquer à n’importe quelle plateforme, nous avons créé la plateforme Windows universelle (UWP) pour prendre en charge Fluent Design.

Les fonctionnalités Fluent Design sont intégrées à UWP. Certaines de ces fonctionnalités (comme les pixels effectifs et le système d’entrée universel) sont automatiques. Il n’est pas nécessaire d’écrire du code supplémentaire pour en bénéficier. D’autres fonctionnalités, telles que l’acrylique, sont facultatives : vous pouvez les ajouter à votre application en écrivant du code.

> Nous mettons à disposition les contrôles UWP pour les postes de travail afin que vous puissiez améliorer l’apparence et les fonctionnalités de vos applications WPF ou Windows existantes avec des fonctionnalités Fluent Design. Pour plus d’informations, consultez [Héberger des contrôles UWP dans les applications WPF et Windows Forms](/windows/uwp/xaml-platform/xaml-host-controls).

<!-- To apply Fluent Design to your app, follow our guidelines and use UWP (Universal Windows Platform) you can use UWP UI features combined with best practices for creating apps that perform beautifully on all types of Windows-powered devices. -->

En plus de fournir des conseils de conception, nos articles Fluent Design vous expliquent comment écrire le code de vos conceptions. UWP utilise le langage de balisage XAML, qui facilite la création d’interfaces utilisateur. Voici un exemple :

```xaml
<Grid BorderBrush="Blue" BorderThickness="4">
    <TextBox Text="Design with XAML" Margin="20" Padding="16,24"/>
</Grid>
```

![Exemple XAML](images/fluent/xaml-example.png)


> Si vous ne connaissez pas bien le développement UWP, consultez la page [Bien démarrer avec UWP](/windows/uwp/get-started).

## <a name="find-a-natural-fit"></a>Trouvez une solution naturelle

Comment faire pour qu’une application semble naturelle sur un grand nombre d’appareils ? En donnant l’impression qu’elle a été conçue pour chacun de ces appareils. Une disposition qui s’adapte à différentes tailles d’écran (pour éviter aussi bien les pertes d’espace que l’encombrement de l’écran) rend l’expérience naturelle, comme si elle avait été spécialement conçue pour l’appareil.

:::row:::
    :::column:::
        ![image fpo](images/fluent/thumbnail-size-classes.jpg)
    :::column-end:::
    :::column span="2":::
**Concevez pour les points d’arrêt appropriés**

Au lieu de concevoir une application pour chaque taille d’écran, le fait de se concentrer sur plusieurs largeurs principales (également appelées « points d’arrêt ») peut considérablement simplifier vos conceptions et votre code, tout en donnant à votre application une apparence remarquable sur les petits comme sur les grands écrans.

[En savoir plus sur les tailles d’écran et les points d’arrêt](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![image fpo](images/fluent/rspd-resize.gif)
    :::column-end:::
    :::column span="2":::
**Créez une disposition dynamique**

Pour qu’une application paraisse naturelle, elle doit adapter sa disposition à différents appareils et tailles d’écran. Vous pouvez utiliser le dimensionnement automatique, des panneaux de disposition, des états visuels et même des définitions d’interface utilisateur séparées dans XAML pour créer une interface utilisateur réactive.

[En savoir plus sur la conception dynamique](/windows/uwp/design/layout/responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![image fpo](images/fluent/devices.jpg)
    :::column-end:::
    :::column span="2":::
**Concevez pour un large éventail d’appareils**

Les applications UWP peuvent s’exécuter sur un large éventail d’appareils fonctionnant sous Windows. Il est utile de comprendre quels appareils sont disponibles, à quoi ils servent et comment les utilisateurs interagissent avec.

[En savoir plus sur les appareils UWP](/windows/uwp/design/devices/)
:::row-end:::

:::row:::
    :::column:::
        ![image fpo](images/fluent/keyboard-shortcuts.jpg)
    :::column-end:::
    :::column span="2":::
**Optimisez pour l’entrée appropriée**

Les applications UWP prennent automatiquement en charge les interactions souris, clavier, stylet et tactile courantes. Vous n’avez rien de spécial à faire. Toutefois, si vous le souhaitez, vous pouvez améliorer votre application avec une prise en charge optimisée pour des entrées spécifiques, comme le stylet et Surface Dial.

[En savoir plus sur les entrées et les interactions](/windows/uwp/design/input/input-primer)
:::row-end:::

## <a name="make-it-intuitive"></a>Rendez l’utilisation intuitive

Une expérience semble intuitive quand elle se comporte comme l’utilisateur s’y attend. En tirant parti des contrôles et des modèles établis, ainsi que des plateformes d’accessibilité et de globalisation, vous pouvez créer une expérience intuitive qui permet aux utilisateurs d’être plus productifs.

L’expérience intuitive est obtenue lorsque la bonne action est exécutée au bon moment.

Les expériences Fluent utilisent des contrôles et des modèles de façon cohérente, de sorte qu’elles se comportent comme l’utilisateur s’y attend. Les expériences Fluent sont accessibles aux personnes avec un large éventail de capacités physiques et incorporent des fonctionnalités de globalisation de sorte que les personnes du monde entier peuvent les utiliser.

:::row:::
    :::column:::
        ![image fpo](images/fluent/thumbnail-navview.png)
    :::column-end:::
    :::column span="2":::
**Fournissez la navigation appropriée**

Créez une expérience intuitive en utilisant la structure d’application et les composants de navigation appropriés.

[En savoir plus sur la navigation](/windows/uwp/design/basics/navigation-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![image fpo](images/fluent/thumbnail-commanding.png)
    :::column-end:::
    :::column span="2":::
**Soyez interactif**

Les boutons, les barres de commandes, les raccourcis clavier et les menus contextuels permettent aux utilisateurs d’interagir avec votre application. Ces outils transforment une expérience statique en une expérience dynamique.

[En savoir plus sur les commandes](/windows/uwp/design/basics/commanding-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![image fpo](images/fluent/thumbnail-controls-2.jpg)
    :::column-end:::
    :::column span="2":::
**Utilisez les contrôles appropriés pour la tâche**

Les contrôles sont les blocs de construction de l’interface utilisateur ; l’utilisation du contrôle approprié vous permet de créer une interface utilisateur qui se comporte comme les utilisateurs s’y attendent. UWP fournit plus de 45 contrôles, des simples boutons aux contrôles de données puissants.

[En savoir plus sur les contrôles UWP](/windows/uwp/design/controls-and-patterns/)
:::row-end:::

:::row:::
    :::column:::
        ![image inclusive](images/fluent/thumbnail-inclusive.png)
    :::column-end:::
    :::column span="2":::
**Soyez inclusif** Une application bien conçue est accessible aux personnes souffrant de handicaps ou d’invalidités. Avec du codage supplémentaire, vous pouvez partager votre application avec des personnes dans le monde entier.

[En savoir plus sur la facilité d’utilisation](/windows/uwp/design/usability/)
:::row-end:::

## <a name="be-engaging-and-immersive"></a>Soyez attrayant et immersif

Fluent Design ne consiste pas à créer des effets tape-à-l’œil. Il intègre des effets physiques qui améliorent véritablement l’expérience utilisateur, car ils émulent des expériences que nos cerveaux sont programmés pour traiter de manière efficace.

## <a name="use-light"></a>Utilisez la lumière

La lumière a le don d’attirer notre attention. Elle crée une ambiance et un sentiment d’appartenance. C’est aussi un outil pratique pour éclairer des informations.

Ajoutez de la lumière à votre application UWP :

:::row:::
    :::column:::
        ![image fpo](images/fluent/Nav_Reveal_Animation.gif)
    :::column-end:::
    :::column span="2":::
**Effet Révéler**

L’effet [Révéler](/windows/uwp/design/style/reveal) utilise la lumière pour faire ressortir des éléments interactifs. La lumière illumine les éléments avec lesquels l’utilisateur peut interagir, en révélant des bordures masquées. Révéler est automatiquement activé sur certains contrôles, tels que l’affichage Liste et l’affichage Grille. Vous pouvez l’activer sur d’autres contrôles en appliquant nos styles Révéler prédéfinis.
:::row-end:::

:::row:::
    :::column:::
        ![image fpo](images/fluent/traveling-focus-fullscreen-light-rf.gif)
    :::column-end:::
    :::column span="2":::
**Révéler focus**

L’effet [Révéler focus](/windows/uwp/design/style/reveal-focus) utilise la lumière pour attirer l’attention sur l’élément sur lequel le focus est positionné.
:::row-end:::

## <a name="create-a-sense-of-depth"></a>Créez une profondeur

Nous vivons dans un monde en trois dimensions. En intégrant délibérément de la profondeur dans l’interface utilisateur, nous transformons une interface 2D plate en quelque chose de mieux, quelque chose qui présente efficacement des informations et des concepts en créant une hiérarchie visuelle. Cela réinvente la relation entre les éléments dans un environnement physique stratifié.

Ajoutez de la profondeur à votre application UWP :

:::row:::
    :::column:::
        ![image fpo](images/fluent/_parallax_v2.gif)
    :::column-end:::
    :::column span="2":::
**Parallaxe**

[Parallaxe](/windows/uwp/design/motion/parallax) crée une illusion de profondeur en donnant l’impression que les éléments situés au premier plan se déplacent plus rapidement que ceux de l’arrière-plan.
:::row-end:::

## <a name="incorporate-motion"></a>Incorporez du mouvement

Imaginez la conception du mouvement comme un film. La fluidité des transitions vous permet de vous concentrer sur l’histoire et donne vie aux expériences. Nous pouvons inviter ces sentiments dans nos conceptions, en guidant les utilisateurs d’une tâche à l’autre avec une aisance cinématographique.

Ajoutez du mouvement à votre application UWP :

:::row:::
    :::column:::
        ![gif de continuité](images/fluent/continuityXbox.gif)
    :::column-end:::
    :::column span="2":::
**Animations connectées**

Les [animations connectées](/windows/uwp/design/motion/connected-animation) aident l’utilisateur à préserver le contexte en créant une transition transparente entre les scènes.
:::row-end:::

## <a name="build-it-with-the-right-material"></a>Générez-la avec la matière appropriée

Les éléments qui nous entourent dans le monde réel sont sensoriels et stimulants. Ils plient, s’étirent, rebondissent, éclatent et glissent. Ces caractéristiques matérielles se traduisent en environnements numériques et donnent envie aux utilisateurs d’interagir avec nos conceptions.

Ajoutez de la matière à votre application UWP :

:::row:::
    :::column:::
        ![image fpo](images/fluent/acrylic_lighttheme_base.png)
    :::column-end:::
    :::column span="2":::
**Acrylique**

[Acrylique](/windows/uwp/design/style/acrylic) est un matériau translucide qui permet à l’utilisateur de voir des couches de contenu, en établissant une hiérarchie d’éléments d’interface utilisateur.
:::row-end:::

## <a name="design-toolkits-and-code-samples"></a>Kits de ressources et exemples de code

Vous voulez commencer à créer vos propres applications avec Fluent Design ? Nos kits de ressources pour Adobe XD, Adobe Illustrator, Adobe Photoshop, Framer et Sketch vous aideront à accélérer vos conceptions, et nos exemples vous aideront à coder plus rapidement.

:::row:::
    :::column:::
        ![image fpo](images/fluent/thumbnail-toolkits.jpg)
    :::column-end:::
    :::column span="2":::
**Page Kits de ressources et exemples de conception**

Consultez notre page [Kits de ressources et exemples de conception](/windows/uwp/design/downloads/)
:::row-end:::









