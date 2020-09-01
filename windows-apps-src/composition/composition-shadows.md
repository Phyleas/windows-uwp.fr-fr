---
title: Ombres de la composition
description: Les API Shadow vous permettent d’ajouter des ombres personnalisables dynamiques au contenu de l’interface utilisateur.
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 29ca04ac3faf3a2884bcc2346177f49222cf786e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166423"
---
# <a name="shadows-in-windows-ui"></a>Ombres dans l’interface utilisateur Windows

La classe [DropShadow](/uwp/api/Windows.UI.Composition.DropShadow) fournit des moyens de créer une ombre configurable qui peut être appliquée à un [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual) ou [LayerVisual](/uwp/api/windows.ui.composition.layervisual) (sous-arborescence d’éléments visuels). Comme c’est le cas pour les objets de la couche visuelle, toutes les propriétés de DropShadow peuvent être animées à l’aide de CompositionAnimations.

## <a name="basic-drop-shadow"></a>Ombre portée de base

Pour créer une ombre de base, créez simplement un nouveau DropShadow et associez-le à votre visuel. L’ombre est rectangulaire par défaut. Un ensemble standard de propriétés est disponible pour affiner l’apparence de votre ombre.

```cs
var basicRectVisual = _compositor.CreateSpriteVisual();
basicRectVisual.Brush = _compositor.CreateColorBrush(Colors.Blue);
basicRectVisual.Offset = new Vector3(100, 100, 20);
basicRectVisual.Size = new Vector2(300, 300);

var basicShadow = _compositor.CreateDropShadow();
basicShadow.BlurRadius = 25f;
basicShadow.Offset = new Vector3(20, 20, 20);

basicRectVisual.Shadow = basicShadow;
```

![Visuel rectangulaire avec DropShadow de base](images/rectangular-dropshadow.png)

## <a name="shaping-the-shadow"></a>Mise en forme de l’ombre

Il existe plusieurs façons de définir la forme de votre DropShadow :

- **Utiliser la valeur par** défaut : par défaut, la forme DropShadow est définie par le mode « default » sur CompositionDropShadowSourcePolicy. Pour SpriteVisual, la valeur par défaut est rectangulaire, sauf si un masque est fourni. Pour LayerVisual, la valeur par défaut consiste à hériter d’un masque à l’aide de l’alpha du pinceau du visuel.
- **Définir un masque** : vous pouvez définir la propriété [Mask](/uwp/api/windows.ui.composition.dropshadow.mask) pour définir un masque d’opacité pour l’ombre.
- **Spécifiez pour utiliser le masque hérité** – définissez la propriété [SourcePolicy](/uwp/api/windows.ui.composition.dropshadow.sourcepolicy) pour utiliser [CompositionDropShadowSourcePolicy](/uwp/api/windows.ui.composition.compositiondropshadowsourcepolicy). InheritFromVisualContent pour utiliser le masque généré à partir de l’alpha du pinceau du visuel.

## <a name="masking-to-match-your-content"></a>Masquage pour correspondre à votre contenu

Si vous souhaitez que votre ombre corresponde au contenu de l’éléments visuels, vous pouvez soit utiliser le pinceau visuel pour la propriété masque Shadow, soit définir l’ombre de façon à ce qu’elle hérite automatiquement du contenu. Si vous utilisez un LayerVisual, l’ombre héritera du masque par défaut.

```cs
var imageSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/myImage.png"));
var imageBrush = _compositor.CreateSurfaceBrush(imageSurface);

var imageSpriteVisual = _compositor.CreateSpriteVisual();
imageSpriteVisual.Size = new Vector2(400,400);
imageSpriteVisual.Offset = new Vector3(100, 500, 20);
imageSpriteVisual.Brush = imageBrush;

var shadow = _compositor.CreateDropShadow();
shadow.Mask = imageBrush;
// or use shadow.SourcePolicy = CompositionDropShadowSourcePolicy.InheritFromVisualContent;
shadow.BlurRadius = 25f;
shadow.Offset = new Vector3(20, 20, 20);

imageSpriteVisual.Shadow = shadow;
```

![Image Web connectée avec ombre portée masquée](images/ms-brand-web-dropshadow.png)

## <a name="using-an-alternative-mask"></a>Utilisation d’un autre masque

Dans certains cas, vous souhaiterez peut-être mettre l’ombre en forme de façon à ce qu’elle ne corresponde pas au contenu de votre visuel. Pour obtenir cet effet, vous devez définir explicitement la propriété Mask à l’aide d’un pinceau avec alpha.

Dans l’exemple ci-dessous, nous chargeons deux surfaces : une pour le contenu visuel et une pour le masque d’ombre :

```cs
var imageSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/myImage.png"));
var imageBrush = _compositor.CreateSurfaceBrush(imageSurface);

var circleSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/myCircleImage.png"));
var customMask = _compositor.CreateSurfaceBrush(circleSurface);

var imageSpriteVisual = _compositor.CreateSpriteVisual();
imageSpriteVisual.Size = new Vector2(400,400);
imageSpriteVisual.Offset = new Vector3(100, 500, 20);
imageSpriteVisual.Brush = imageBrush;

var shadow = _compositor.CreateDropShadow();
shadow.Mask = customMask;
shadow.BlurRadius = 25f;
shadow.Offset = new Vector3(20, 20, 20);

imageSpriteVisual.Shadow = shadow;
```

![Image Web connectée avec ombre portée masquée par un cercle](images/ms-brand-web-masked-dropshadow.png)

## <a name="animating"></a>Animer

Comme c’est le cas dans la couche visuelle, les propriétés DropShadow peuvent être animées à l’aide d’animations de composition. Ci-dessous, nous modifions le code de l’exemple de gicleurs ci-dessus pour animer le rayon de flou de l’ombre.

```cs
ScalarKeyFrameAnimation blurAnimation = _compositor.CreateScalarKeyFrameAnimation();
blurAnimation.InsertKeyFrame(0.0f, 25.0f);
blurAnimation.InsertKeyFrame(0.7f, 50.0f);
blurAnimation.InsertKeyFrame(1.0f, 25.0f);
blurAnimation.Duration = TimeSpan.FromSeconds(4);
blurAnimation.IterationBehavior = AnimationIterationBehavior.Forever;
shadow.StartAnimation("BlurRadius", blurAnimation);
```

## <a name="shadows-in-xaml"></a>Shadows en XAML

Si vous souhaitez ajouter une ombre à des éléments d’infrastructure plus complexes, il existe deux façons d’interopérabilité avec les ombres entre XAML et la composition :

1. Utilisez le [DropShadowPanel](https://github.com/windows-toolkit/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Uwp.UI.Controls/DropShadowPanel/DropShadowPanel.Properties.cs) disponible dans la boîte à outils de la communauté Windows. Pour plus d’informations sur son utilisation, consultez la [documentation de DropShadowPanel](/windows/uwpcommunitytoolkit/controls/DropShadowPanel) .
1. Créez un visuel à utiliser comme hôte d’ombre & le lier au visuel de document XAML.
1. Utilisez le contrôle CompositionShadow personnalisé [SamplesCommon](https://github.com/microsoft/WindowsCompositionSamples/tree/master/SamplesCommon/SamplesCommon) de la Galerie d’exemples de composition. Consultez l’exemple ici pour plus d’utilisation.

## <a name="performance"></a>Performances

Bien que la couche visuelle ait de nombreuses optimisations en place pour rendre les effets efficaces et utilisables, la génération d’ombres peut être une opération relativement coûteuse en fonction des options que vous définissez. Voici les « coûts » de haut niveau pour les différents types d’ombres. Notez que même si certaines ombres peuvent être onéreuses, elles peuvent toujours être utiles dans certains scénarios.

Caractéristiques de l’ombre| Coût
------------- | -------------
Rectangulaire    | Faible
Masque Shadow      | Élevé
CompositionDropShadowSourcePolicy.InheritFromVisualContent | Élevé
Rayon de flou statique | Faible
Animer le rayon de flou | Élevé

## <a name="additional-resources"></a>Ressources supplémentaires

- [API DropShadow de la composition](/uwp/api/Windows.UI.Composition.DropShadow)
- [WindowsUIDevLabs GitHub référentiel](https://github.com/microsoft/WindowsCompositionSamples)