---
title: Éclairage de la composition
description: Les API d’éclairage de la composition peuvent être utilisées pour ajouter l’éclairage 3D dynamique à votre application.
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5531382ef46346a40844a8eb5a5a77c0ad565fbb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154703"
---
# <a name="using-lights-in-windows-ui"></a>Utilisation des lumières dans l’interface utilisateur Windows

Les API Windows. UI. composition vous permettent de créer des animations et des effets en temps réel. L’éclairage de la composition active l’éclairage 3D dans les applications 2D. Dans cette vue d’ensemble, nous allons exécuter les fonctionnalités de configuration des lumières de composition, identifier les éléments visuels pour recevoir chaque lumière et utiliser des effets pour définir des matériaux pour votre contenu.

> [!NOTE]
> Pour savoir comment les objets [XamlLight](/uwp/api/windows.ui.xaml.media.xamllight) s’appliquent [CompositionLights](/uwp/api/Windows.UI.Composition.CompositionLight) pour éclairer les UIElements XAML, consultez la page [éclairage XAML](xaml-lighting.md).

L’éclairage de la composition vous permet de créer une interface utilisateur intéressante en autorisant :

- Transformation d’une lumière indépendante des autres objets de la scène pour permettre des scénarios immersifs comme les scènes de lecture de musique.
- La possibilité de coupler un objet avec une lumière afin qu’il se déplace ensemble indépendamment du reste de la scène pour permettre des scénarios tels que Fluent [révéler](../design/style/reveal.md) la mise en surbrillance.
- Transformation de la lumière et de la scène entière en groupe pour créer des matériaux et une profondeur.

L’éclairage de la composition prend en charge trois concepts clés : **Light**, **Targets**et **SceneLightingEffect**.

## <a name="light"></a>Clair

[CompositionLight](/uwp/api/windows.ui.composition.compositionlight) vous permet de créer différentes lumières et de les placer dans l’espace de coordonnées. Ces lumières ciblent les éléments visuels que vous souhaitez identifier comme allumés par la lumière.

### <a name="light-types"></a>Types de lumière

| Type | Description |
| --- | --- |
| [AmbientLight](/uwp/api/windows.ui.composition.ambientlight) | Source de lumière qui émet un éclairage non directionnel qui apparaît en fonction de tous les éléments de la scène. |
| [DistantLight](/uwp/api/windows.ui.composition.distantlight) | Source de lumière distante de grande taille infinie qui émet de la lumière dans une seule direction. Comme le soleil. |
| [PointLight](/uwp/api/windows.ui.composition.pointlight) | Source de point de lumière qui émet de la lumière dans toutes les directions. Comme une ampoule. |
| [Vedette](/uwp/api/windows.ui.composition.spotlight) | Source de lumière qui émet des cônes internes et externes de la lumière. Comme un torche. |

## <a name="targets"></a>Cibles

Quand les voyants ciblent un visuel (ajouter à la liste des [cibles](/uwp/api/windows.ui.composition.compositionlight.targets) ), le visuel et tous ses descendants prennent connaissance de cette source de lumière et y répondent. Cela peut être aussi simple que la définition d’une source PointLight à la racine d’une arborescence et que tous les éléments visuels ci-dessous réagissent à l’animation de la direction des lumières de point.

**ExclusionsFromTargets** vous donne la possibilité de supprimer l’éclairage d’un visuel ou d’une sous-arborescence d’éléments visuels d’une manière similaire à l’ajout de cibles. Les enfants dans l’arborescence enracinée par le visuel exclu ne sont pas allumés.

### <a name="sample-targets"></a>Exemple (cibles)

Dans l’exemple ci-dessous, nous utilisons un CompositionPointLight pour cibler un TextBlock XAML.

```cs
    _pointLight = _compositor.CreatePointLight();
    _pointLight.Color = Colors.White;
    _pointLight.CoordinateSpace = text; //set up co-ordinate space for offset
    _pointLight.Targets.Add(text); //target XAML TextBlock
```

En ajoutant une animation au décalage de la lumière de point, un effet de scintillement est facilement réalisable.

```cs
_pointLight.Offset = new Vector3(-(float)TextBlock.ActualWidth, (float)TextBlock.ActualHeight / 2, (float)TextBlock.FontSize);
```

Pour en savoir plus, consultez l’exemple de [scintillement du texte](https://github.com/microsoft/WindowsCompositionSamples/tree/master/SampleGallery/Samples/SDK 14393/TextShimmer) complet à l’exemple de cuisine WindowUIDevLabs.

## <a name="restrictions"></a>Restrictions

Plusieurs facteurs sont à prendre en compte pour déterminer le contenu qui sera allumé par CompositionLight.

Concept | Détails
--- | ---
**Lumière ambiante** | Si vous ajoutez une lumière non ambiante à votre scène, vous désactivez toutes les lumières existantes.  Les éléments qui ne sont pas ciblés par une lumière non ambiante apparaissent en noir.  Pour éclairer de manière naturelle les visuels environnants non ciblés par la lumière, utilisez une lumière ambiante conjointement avec d’autres lumières.
**Nombre d’éclairages** | Vous pouvez utiliser deux lumières de composition non ambiantes dans n’importe quelle combinaison pour cibler votre interface utilisateur. Les éclairages ambiants ne sont pas restreints ; les lumières des points, des points et des points éloignés sont.
**Durée de vie** | CompositionLight peut rencontrer des conditions de durée de vie (par exemple : le garbage collector peut recycler l’objet Light avant son utilisation).  Nous vous recommandons de conserver une référence à vos lumières en ajoutant des lumières en tant que membre pour aider l’application à gérer la durée de vie.
**Transformations** | Les indicateurs doivent être placés dans un nœud au-dessus de l’interface utilisateur qui utilise des effets tels que les [transformations de perspective](../design/layout/3-d-perspective-effects.md) dans votre structure visuelle pour être correctement dessinés.
**Cibles et espace de coordonnées** | CoordinateSpace est l’espace visuel dans lequel toutes les propriétés des lumières doivent être définies. CompositionLight. targets doit se trouver dans l’arborescence CoordinateSpace.

## <a name="lighting-properties"></a>Propriétés d’éclairage

Selon le type de lumière utilisé, une lumière peut avoir des propriétés pour l’atténuation et l’espace. Tous les types d’éclairage n’utilisent pas toutes les propriétés.

Propriété | Description
--- | ---
**Couleur** | [Couleur](/uwp/api/windows.ui.color) de la lumière. Les valeurs de couleur d’éclairage sont définies par la diffusion [D3D](../graphics-concepts/light-properties.md) , l’ambiance et l’spéculaire qui définit la couleur émise. L’éclairage utilise des valeurs RVBA pour les lumières ; le composant de couleur alpha n’est pas utilisé.
**Sens** | Direction de la lumière. La direction dans laquelle pointe la lumière est spécifiée par rapport à son visuel [CoordinateSpace](/uwp/api/windows.ui.composition.distantlight.coordinatespace) .
**Espace de coordonnées** | Chaque visuel a un espace de coordonnées 3D implicite. L’axe X est de gauche à droite. La direction Y est de haut en bas. La direction Z est un point hors du plan. Le point d’origine de cette coordonnée est l’angle supérieur gauche du visuel, tandis que l’unité est DIP (Device Independent Pixel). Offset de lumière défini dans cette coordonnée.
**Cônes internes et externes** | Les lumières émettent un cône de lumière qui comporte deux parties : un cône interne vif et un cône externe. La composition vous permet de contrôler les angles et la couleur des cônes internes et externes.
**Offset** | Décalage de la source de lumière par rapport à son visuel d’espace de coordonnées.

> [!NOTE]
> Lorsque plusieurs lumières atteignent le même visuel, ou chaque fois que la valeur de couleur d’une lumière est suffisamment grande pour dépasser 1,0, la couleur de la lumière peut changer en raison de la fixation d’un canal de couleur d’éclairage.

### <a name="advanced-lighting-properties"></a>Propriétés d’éclairage avancées

Propriété | Description
--- | ---
**Intensité** | Contrôle la luminosité de la lumière.
**Atténuation** | L’atténuation contrôle la manière dont l’intensité d’une lumière diminue vers la distance maximale spécifiée par la propriété de la plage.  Les propriétés d’atténuation constante, Quadradic et linéaire peuvent être utilisées.

## <a name="getting-started-with-lighting"></a>Prise en main avec éclairage

Pour ajouter des lumières, suivez ces étapes générales :

- Créez et placez les lumières : créez des lumières et placez-les dans un espace de coordonnées spécifié.
- Identifiez les objets à la lumière : ciblez la lumière aux visuels pertinents.
- Facultatif Définir la manière dont des objets individuels réagissent aux lumières : utilisez SceneLightingEffect avec un EffectBrush pour personnaliser la réflexion légère pour l’affichage du SpriteVisual. Les valeurs par défaut de réflexion prennent en charge l’éclairage des enfants du CoordinateSpace d’une source de lumière.  Un visuel peint avec un SceneLightingEffect remplace l’éclairage par défaut de cet visuel.

## <a name="scenelightingeffect"></a>SceneLightingEffect

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) est utilisé pour modifier l’éclairage par défaut appliqué au contenu d’un [SpriteVisual](/uwp/api/Windows.UI.Composition.SpriteVisual) ciblé par un [CompositionLight](/uwp/api/windows.ui.composition.compositionlight).

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) est fréquemment utilisé pour la création de matériaux. Un SceneLightingEffect est un effet utilisé lorsque vous souhaitez obtenir un résultat plus complexe, comme l’activation de propriétés réfléchissantes sur une image et/ou la fourniture d’une illusion de profondeur avec une carte normale. Un SceneLightingEffect offre la possibilité de personnaliser votre interface utilisateur à l’aide des propriétés d’éclairage comme les montants spéculaires et spéculaires. Vous pouvez personnaliser davantage les effets d’éclairage avec le reste du pipeline d’effets, ce qui vous permet de mélanger et de composer différentes réactions d’éclairage avec votre contenu.

> [!NOTE]
> L’éclairage de la scène ne produit pas d’ombres. C’est un effet axé sur le rendu 2D.  Il ne prend pas en compte les scénarios d’éclairage 3D qui incluent des modèles d’éclairage réels, y compris les ombres.


Propriété | Description
--- | ---
**Carte normale** | NormalMaps créer un effet d’une texture dans laquelle un point normal vers la lumière sera plus clair et un pointage normal sera plus faible. Pour ajouter un NormalMap à votre visuel ciblé, utilisez un [CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) à l’aide de LoadedImageSurface pour charger un élément multimédia NormalMap.
**Ambiant** | Les propriétés ambiantes sont principalement utilisées pour contrôler la réflexion de couleur globale.
**Spéculaire** | La réflexion spéculaire crée des surbrillances sur les objets, ce qui les rend brillants. Vous pouvez contrôler le niveau de réflexion spéculaire ainsi que le niveau de brillance.  Ces propriétés sont manipulées pour créer des effets significatifs, tels que des métaux shinny ou du papier glacé.
**Diffus** | La réflexion diffuse diffuse la lumière dans toutes les directions.
**Modèle de réflexion** | Le [modèle de réflexion](/uwp/api/windows.ui.composition.effects.scenelightingeffectreflectancemodel) vous permet de choisir entre [Blinn Phong](/visualstudio/designers/how-to-create-a-basic-phong-shader) et Blinn Phong physiquement.  Vous devez choisir un Phong Blinn en fonction des points lorsque vous souhaitez obtenir des surbrillances spéculaires.

### <a name="sample-scenelightingeffect"></a>Exemple (SceneLightingEffect)

L’exemple ci-dessous montre comment ajouter une carte normale à un SceneLightingEffect.

```cs
CompositionBrush CreateNormalMapBrush(ICompositionSurface normalMapImage)
{
    var colorSourceEffect = new ColorSourceEffect()
    {
        Color = Colors.White
    };
    var sceneLightingEffect = new SceneLightingEffect()
    {
        NormalMapSource = new CompositionEffectSourceParameter("NormalMap")
    };

    var compositeEffect = new ArithmeticCompositeEffect()
    {
        Source1 = colorSourceEffect,
        Source2 = sceneLightingEffect,
    };

    var factory = _compositor.CreateEffectFactory(sceneLightingEffect);

    var normalMapBrush = _compositor.CreateSurfaceBrush();
    normalMapBrush.Surface = normalMapImage;
    normalMapBrush.Stretch = CompositionStretch.Fill;

    var brush = factory.CreateBrush();
    brush.SetSourceParameter("NormalMap", normalMapBrush);

    return brush;
}
```

## <a name="related-articles"></a>Articles connexes

- [Création de matériaux et d’éclairages dans la couche visuelle](https://blogs.windows.com/buildingapps/2017/08/04/creating-materials-lights-visual-layer/)
- [Vue d’ensemble de l’éclairage](../graphics-concepts/lighting-overview.md)
- [API CompositionCapabilities](/uwp/api/windows.ui.composition.compositioncapabilities)
- [Mathématiques d’éclairage](../graphics-concepts/mathematics-of-lighting.md)
- [SceneLightingEffect](/uwp/api/windows.ui.composition.effects.scenelightingeffect)
- [WindowsUIDevLabs GitHub référentiel](https://github.com/microsoft/WindowsCompositionSamples)