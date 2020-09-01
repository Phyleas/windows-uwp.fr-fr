---
title: Animations de propriété XAML
description: Découvrez comment animer des propriétés sur un UIElement XAML directement à l’aide d’animations de composition plateforme Windows universelle (UWP).
ms.date: 09/13/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 0a7ab119df407b45fb391aebf50df5d85f89ec0f
ms.sourcegitcommit: e273e5901bfa6596dfef4cc741bb1c42614c25ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89238294"
---
# <a name="animating-xaml-elements-with-composition-animations"></a>Animer des éléments XAML avec des animations de composition

Cet article présente de nouvelles propriétés qui vous permettent d’animer un UIElement XAML avec les performances des animations de composition et la facilité de définition des propriétés XAML.

Avant Windows 10, version 1809, vous aviez deux possibilités pour créer des animations dans vos applications UWP :

- Utilisez des constructions XAML comme [les animations de Storyboard](storyboarded-animations.md), ou les classes _* ThemeTransition_ et _* ThemeAnimation_ dans l’espace de noms [Windows. UI. Xaml. Media. Animation](/uwp/api/windows.ui.xaml.media.animation) .
- Utilisez les animations de composition comme décrit dans [utilisation de la couche visuelle avec XAML](../../composition/using-the-visual-layer-with-xaml.md).

L’utilisation de la couche visuelle offre de meilleures performances que l’utilisation des constructions XAML. Toutefois, l’utilisation de [ElementCompositionPreview](/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview) pour obtenir l’objet [visuel](/uwp/api/windows.ui.composition.visual) de la composition sous-jacente de l’élément, puis l’animation de l’élément visuel avec des animations de composition, est plus complexe à utiliser.

À partir de Windows 10, version 1809, vous pouvez animer des propriétés sur un UIElement directement à l’aide d’animations de composition sans avoir à obtenir le visuel de la composition sous-jacente.

> [!NOTE]
> Pour utiliser ces propriétés sur UIElement, la version cible de votre projet UWP doit être 1809 ou une version ultérieure. Pour plus d’informations sur la configuration de la version de votre projet, consultez [version adaptative Apps](../../debug-test-perf/version-adaptive-apps.md).

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Si l’application de la <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong> est installée, cliquez ici pour <a href="xamlcontrolsgallery:/item/XamlCompInterop">ouvrir l’application et voir interopérabilité de l’animation en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="new-rendering-properties-replace-old-rendering-properties"></a>Les nouvelles propriétés de rendu remplacent les anciennes propriétés de rendu

Ce tableau répertorie les propriétés que vous pouvez utiliser pour modifier le rendu d’un UIElement, qui peut également être animé avec un [CompositionAnimation](/uwp/api/windows.ui.composition.compositionanimation).

| Propriété | Type | Description |
| -- | -- | -- |
| [Opacity](/uwp/api/windows.ui.xaml.uielement.opacity) | Double | Degré d’opacité de l’objet |
| [Traduction](/uwp/api/windows.ui.xaml.uielement.translation) | Vector3 | Déplacer la position X/Y/Z de l’élément |
| [TransformMatrix](/uwp/api/windows.ui.xaml.uielement.transformmatrix) | Matrix4x4 | Matrice de transformation à appliquer à l’élément |
| [Mettre à l'échelle](/uwp/api/windows.ui.xaml.uielement.scale) | Vector3 | Mettre à l’échelle l’élément, centré sur le CenterPoint |
| [Rotation](/uwp/api/windows.ui.xaml.uielement.rotation) | Float | Pivoter l’élément autour des RotationAxis et CenterPoint |
| [RotationAxis](/uwp/api/windows.ui.xaml.uielement.rotationaxis) | Vector3 | Axe de rotation |
| [CenterPoint](/uwp/api/windows.ui.xaml.uielement.centerpoint) | Vector3 | Point central de l’échelle et de la rotation |

La valeur de la propriété TransformMatrix est associée aux propriétés Scale, rotation et translation dans l’ordre suivant : TransformMatrix, Scale, rotation, translation.

Ces propriétés n’affectent pas la disposition de l’élément ; par conséquent, la modification de ces propriétés [Measure](/uwp/api/windows.ui.xaml.uielement.measure)n’entraîne pas de nouvelle passe de / [réorganisation](/uwp/api/windows.ui.xaml.uielement.arrange) de mesure.

Ces propriétés ont le même objectif et le même comportement que les propriétés de type like sur la classe d’éléments [visuels](/uwp/api/windows.ui.composition.visual) composition (à l’exception de la traduction, qui n’est pas en mode visuel).

### <a name="example-setting-the-scale-property"></a>Exemple : définition de la propriété Scale

Cet exemple montre comment définir la propriété Scale sur un bouton.

```xaml
<Button Scale="2,2,1" Content="I am a large button" />
```

```csharp
var button = new Button();
button.Content = "I am a large button";
button.Scale = new Vector3(2.0f,2.0f,1.0f);
```

### <a name="mutual-exclusivity-between-new-and-old-properties"></a>Exclusivité mutuelle entre les propriétés nouvelles et anciennes

> [!NOTE]
> La propriété **Opacity** n’applique pas l’exclusivité mutuelle décrite dans cette section. Vous utilisez la même propriété Opacity si vous utilisez des animations XAML ou de composition.

Les propriétés qui peuvent être animées avec un CompositionAnimation sont des remplacements pour plusieurs propriétés UIElement existantes :

- [RenderTransform](/uwp/api/windows.ui.xaml.uielement.rendertransform)
- [RenderTransformOrigin](/uwp/api/windows.ui.xaml.uielement.rendertransformorigin)
- [Projection](/uwp/api/windows.ui.xaml.uielement.projection)
- [Transform3D](/uwp/api/windows.ui.xaml.uielement.transform3d)

Lorsque vous définissez (ou animez) les nouvelles propriétés, vous ne pouvez pas utiliser les anciennes propriétés. À l’inverse, si vous définissez (ou animez) l’une des anciennes propriétés, vous ne pouvez pas utiliser les nouvelles propriétés.

Vous ne pouvez pas non plus utiliser les nouvelles propriétés si vous utilisez ElementCompositionPreview pour récupérer et gérer vous-même le visuel à l’aide des méthodes suivantes :

- [ElementCompositionPreview.GetElementVisual](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual)
- [ElementCompositionPreview.SetIsTranslationEnabled](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.setistranslationenabled)

> [!IMPORTANT]
> Si vous tentez de combiner l’utilisation des deux jeux de propriétés, l’appel de l’API échoue et un message d’erreur est généré.

Il est possible de basculer d’un jeu de propriétés à un autre en les effaçant, bien que pour des raisons de simplicité, il n’est pas recommandé. Si la propriété est stockée par un DependencyProperty (par exemple, UIElement. projection est associé à UIElement. ProjectionProperty), appelez ClearValue pour le restaurer à son état « inutilisé ». Sinon (par exemple, la propriété Scale), affectez à la propriété sa valeur par défaut.

## <a name="animating-uielement-properties-with-compositionanimation"></a>Animation des propriétés UIElement avec CompositionAnimation

Vous pouvez animer les propriétés de rendu listées dans le tableau avec un CompositionAnimation. Ces propriétés peuvent également être référencées par un [ExpressionAnimation](/uwp/api/windows.ui.composition.expressionanimation).

Utilisez les méthodes [StartAnimation](/uwp/api/windows.ui.xaml.uielement.startanimation) et [StopAnimation](/uwp/api/windows.ui.xaml.uielement.stopanimation) sur UIElement pour animer les propriétés UIElement.

### <a name="example-animating-the-scale-property-with-a-vector3keyframeanimation"></a>Exemple : animation de la propriété Scale avec un Vector3KeyFrameAnimation

Cet exemple montre comment animer l’échelle d’un bouton.

```csharp
var compositor = Window.Current.Compositor;
var animation = compositor.CreateVector3KeyFrameAnimation();

animation.InsertKeyFrame(1.0f, new Vector3(2.0f,2.0f,1.0f));
animation.Duration = TimeSpan.FromSeconds(1);
animation.Target = "Scale";

button.StartAnimation(animation);
```

### <a name="example-animating-the-scale-property-with-an-expressionanimation"></a>Exemple : animation de la propriété Scale avec un ExpressionAnimation

Une page comporte deux boutons. Le deuxième bouton s’anime sur deux fois plus grand (via la mise à l’échelle) que le premier bouton.

```xaml
<Button x:Name="sourceButton" Content="Source"/>
<Button x:Name="destinationButton" Content="Destination"/>
```

```csharp
var compositor = Window.Current.Compositor;
var animation = compositor.CreateExpressionAnimation("sourceButton.Scale*2");
animation.SetExpressionReferenceParameter("sourceButton", sourceButton);
animation.Target = "Scale";
destinationButton.StartAnimation(animation);
```

## <a name="related-topics"></a>Rubriques connexes

- [Animations dans une table de montage séquentiel](storyboarded-animations.md)
- [Utilisation de la couche visuelle avec le contenu XAML](../../composition/using-the-visual-layer-with-xaml.md)
- [Vue d’ensemble des transformations](../layout/transforms.md)
