---
ms.assetid: 02141F86-355E-4046-86EA-2A89D615B7DB
title: Utiliser des pinceaux
description: Les objets Brush permettent de peindre les intérieurs ou les contours de formes, de textes et de parties de contrôles, pour que l’objet peint soit visible dans une interface utilisateur.
ms.date: 04/28/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 350b07e96d95b043116f2eb8029a2352360c50d6
ms.sourcegitcommit: 490b563462853f10f14825f2358e4852ee1011fb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82633002"
---
# <a name="using-brushes-to-paint-backgrounds-foregrounds-and-outlines"></a>Utilisation des pinceaux pour peindre des arrière-plans, des premiers plans et des contours

Utilisez des objets [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) pour peindre les intérieurs et les contours de formes, textes et contrôles XAML afin de les rendre visibles dans l’interface utilisateur de votre application.

> **API importantes** :  [classe Brush](/uwp/api/Windows.UI.Xaml.Media.Brush)

## <a name="introduction-to-brushes"></a>Présentation des pinceaux

Pour peindre un objet [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape), du texte ou des parties d’un [**Control**](/uwp/api/Windows.UI.Xaml.Controls.Control) qui apparaît sur le canevas de l’application, définissez la propriété [**Fill**](/uwp/api/windows.ui.xaml.shapes.shape.fill) de **Shape** ou les propriétés [**Background**](/uwp/api/windows.ui.xaml.controls.control.background) et [**Foreground**](/uwp/api/windows.ui.xaml.controls.control.foreground) d’un **Control** avec une valeur **Brush**.

Les différents types de pinceaux sont les suivants : 
-   [**AcrylicBrush**](/uwp/api/windows.ui.xaml.media.acrylicbrush)
-   [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)
-   [**LinearGradientBrush**](/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush) 
-   [**RadialGradientBrush**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush) 
-   [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush)
-   [**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush)
-   [**XamlCompositionBrushBase**](/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)

## <a name="solid-color-brushes"></a>Pinceaux de couleur unie

Un pinceau [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) peint une zone en une seule couleur [**Color**](/uwp/api/Windows.UI.Color), comme le rouge ou le bleu. Il s’agit du pinceau le plus élémentaire. En XAML, il existe trois façons de définir un **SolidColorBrush** et la couleur associée : avec des noms de couleur prédéfinie, avec des valeurs de couleur hexadécimales ou avec la syntaxe des éléments de propriété.

### <a name="predefined-color-names"></a>Noms de couleur prédéfinie

Vous pouvez utiliser un nom de couleur prédéfinie, comme [**Yellow**](/uwp/api/windows.ui.colors.yellow) ou [**Magenta**](/uwp/api/windows.ui.colors.magenta). 256 couleurs nommées sont disponibles. L’analyseur XAML convertit le nom de la couleur en une structure [**Color**](/uwp/api/Windows.UI.Color) avec les canaux de couleur appropriés. Étant donné que les 256 couleurs nommées s’appuient sur les noms de couleurs *X11* issus de la spécification CSS3 (Cascading Style Sheets, Level 3), vous connaissez peut-être déjà cette liste de couleurs nommées si vous avez de l’expérience en matière de développement ou de conception de sites web.

Voici un exemple qui définit la propriété [**Fill**](/uwp/api/windows.ui.xaml.shapes.shape.fill) d’un objet [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) sur la couleur prédéfinie [**Red**](/uwp/api/windows.ui.colors.red).

```xaml
<Rectangle Width="100" Height="100" Fill="Red" />
```

![SolidColorBrush rendu](images/brushes-solidcolorbrush.jpg)

*SolidColorBrush appliqué à un rectangle*

Si vous définissez un objet [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) en utilisant un code plutôt que du XAML, chaque couleur nommée est disponible en tant que valeur de propriété statique de la classe [**Colors**](https://docs.microsoft.com/uwp/api/windows.ui.colors). Par exemple, pour déclarer une valeur [**Color**](/uwp/api/windows.ui.xaml.media.solidcolorbrush.color) d’un objet **SolidColorBrush** pour représenter la couleur nommée « Orchid », définissez la valeur **Color** sur la valeur statique [**Colors.Orchid**](/uwp/api/windows.ui.colors.orchid).

### <a name="hexadecimal-color-values"></a>Valeurs de couleur hexadécimales

Vous pouvez utiliser une chaîne au format hexadécimal pour déclarer des valeurs de couleurs précises sur 24 bits avec un canal alpha sur 8 bits pour un objet [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush). Deux caractères dans la plage 0 à F définissent chaque valeur du composant, et l’ordre des valeurs de composants de la chaîne hexadécimale est le suivant : canal alpha (opacité), canal rouge, canal vert et canal bleu (**ARVB**). Par exemple, la valeur hexadécimale « \#FFFF0000 » définit un rouge entièrement opaque (alpha=« FF », rouge=« FF », vert=« 00 » et bleu=« 00 »).

L’exemple XAML suivant définit la propriété [**Fill**](/uwp/api/windows.ui.xaml.shapes.shape.fill) d’un objet [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) sur la valeur hexadécimale « \#FFFF0000 » et aboutit au même résultat que l’utilisation de la couleur nommée [**Colors.Red**](/uwp/api/windows.ui.colors.red).

```xml
<StackPanel>
  <Rectangle Width="100" Height="100" Fill="#FFFF0000" />
</StackPanel>
```

### <a name="property-element-syntax"></a>Syntaxe des éléments de la propriété

Vous pouvez utiliser la syntaxe des éléments de propriété pour définir un objet [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush). Cette syntaxe est plus longue que les méthodes précédentes, mais elle vous permet de spécifier des valeurs de propriétés supplémentaires sur un élément, comme la propriété [**Opacity**](/uwp/api/windows.ui.xaml.media.brush.opacity). Pour plus d’informations sur la syntaxe XAML, notamment la syntaxe des éléments de propriété, consultez la [vue d’ensemble du langage XAML](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-overview) et le [guide de la syntaxe XAML](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-syntax-guide).

Dans les exemples précédents, le pinceau en cours de création est généré implicitement et automatiquement, dans le cadre d’un raccourci délibéré du langage XAML qui assure la simplicité de la définition de l’interface utilisateur dans les cas les plus courants. L’exemple suivant crée un objet [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) et crée explicitement l’objet [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) en tant que valeur d’élément pour une propriété [**Rectangle.Fill**](/uwp/api/windows.ui.xaml.shapes.shape.fill). La propriété [**Color**](/uwp/api/windows.ui.xaml.media.solidcolorbrush.color) de l’objet **SolidColorBrush** est définie sur [**Blue**](/uwp/api/windows.ui.colors.blue), tandis que la propriété [**Opacity**](/uwp/api/windows.ui.xaml.media.brush.opacity) est définie sur 0,5.

```xml
<Rectangle Width="200" Height="150">
    <Rectangle.Fill>
        <SolidColorBrush Color="Blue" Opacity="0.5" />
    </Rectangle.Fill>
</Rectangle>
```

## <a name="linear-gradient-brushes"></a>Pinceaux de dégradé linéaire

Un objet [**LinearGradientBrush**](/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush) peint une zone avec un dégradé défini le long d’une ligne. Cette ligne est appelée *axe de dégradé*. Vous spécifiez les couleurs du dégradé et leur emplacement le long de l’axe avec des objets [**GradientStop**](/uwp/api/Windows.UI.Xaml.Media.GradientStop). Par défaut, l’axe de dégradé va du coin supérieur gauche vers le coin inférieur droit de la zone peinte par le pinceau, ce qui produit un ombrage en diagonale.

Le [**GradientStop**](/uwp/api/Windows.UI.Xaml.Media.GradientStop) est le composant de base d’un pinceau de dégradé. Un point de dégradé spécifie la propriété [**Color**](/uwp/api/windows.ui.xaml.media.gradientstop.color) du pinceau à un [**Offset**](/uwp/api/windows.ui.xaml.media.gradientstop.offset) le long de l’axe de dégradé, quand le pinceau est appliqué à la zone à peindre.

La propriété [**Color**](/uwp/api/windows.ui.xaml.media.gradientstop.color) du point de dégradé spécifie sa couleur. Vous pouvez définir la couleur en utilisant un nom de couleur prédéfini ou en spécifiant des valeurs **ARVB** hexadécimales.

La propriété [**Offset**](/uwp/api/windows.ui.xaml.media.gradientstop.offset) d’un objet [**GradientStop**](/uwp/api/Windows.UI.Xaml.Media.GradientStop) spécifie la position de chaque objet **GradientStop** le long de l’axe de dégradé. **Offset** est une valeur **double** de 0 à 1. Si la propriété **Offset** a pour valeur 0, l’objet **GradientStop** est placé au début de l’axe de dégradé, c’est-à-dire près de son [**StartPoint**](/uwp/api/windows.ui.xaml.media.lineargradientbrush.startpoint). Si la propriété **Offset** a la valeur 1, l’objet **GradientStop** est placé au point [**EndPoint**](/uwp/api/windows.ui.xaml.media.lineargradientbrush.endpoint). Au minimum, un objet [**LinearGradientBrush**](/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush) utile doit comporter deux valeurs **GradientStop**, chaque valeur **GradientStop** devant spécifier une propriété [**Color**](/uwp/api/windows.ui.xaml.media.gradientstop.color) différente et avoir une propriété **Offset** différente dont la valeur est comprise entre 0 et 1.

Cet exemple crée un dégradé linéaire avec quatre couleurs et l’utilise pour peindre un objet [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle).

```xml
<!-- This rectangle is painted with a diagonal linear gradient. -->
<Rectangle Width="200" Height="100">
    <Rectangle.Fill>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="Yellow" Offset="0.0" x:Name="GradientStop1"/>
            <GradientStop Color="Red" Offset="0.25" x:Name="GradientStop2"/>
            <GradientStop Color="Blue" Offset="0.75" x:Name="GradientStop3"/>
            <GradientStop Color="LimeGreen" Offset="1.0" x:Name="GradientStop4"/>
        </LinearGradientBrush>
    </Rectangle.Fill>
</Rectangle>
```

La couleur de chaque point entre les extrémités est interpolée de façon linéaire sous la forme d’une combinaison de la couleur spécifiée par les deux points de dégradé. L’image suivante met en évidence les points de dégradés de l’exemple précédent. Les cercles indiquent la position des points de dégradé, tandis que la ligne en pointillés représente l’axe de dégradé.

![Points de dégradé](images/linear-gradients-stops.png)

*Combinaison de couleurs spécifiée par les deux points de dégradé englobants*

Vous pouvez changer la ligne de positionnement des points de dégradé en affectant aux propriétés [**StartPoint**](/uwp/api/windows.ui.xaml.media.lineargradientbrush.startpoint) et [**EndPoint**](/uwp/api/windows.ui.xaml.media.lineargradientbrush.endpoint) des valeurs différentes des valeurs par défaut initiales `(0,0)` et `(1,1)`. En modifiant les valeurs des coordonnées de **StartPoint** et de **EndPoint**, vous pouvez créer des dégradés horizontaux ou verticaux, inverser le sens du dégradé ou condenser son étalement afin de l’appliquer à une plage plus petite que la zone peinte totale. Pour condenser le dégradé, vous définissez **StartPoint** et/ou **EndPoint** sur des valeurs comprises entre 0 et 1. Par exemple, pour obtenir un dégradé horizontal dont l’effet se produit en totalité sur la moitié gauche du pinceau, tandis que le côté droit conserve la dernière couleur unie définie par l’objet [**GradientStop**](/uwp/api/Windows.UI.Xaml.Media.GradientStop), vous spécifiez une propriété **StartPoint** sur la valeur `(0,0)` et une propriété **EndPoint** sur la valeur `(0.5,0)`.

### <a name="use-tools-to-make-gradients"></a>Utiliser les outils pour rendre les dégradés

Maintenant que vous savez comment fonctionnent les dégradés linéaires, vous pouvez utiliser Visual Studio ou Blend pour les créer plus facilement. Pour créer un dégradé, sélectionnez l’objet auquel vous voulez l’appliquer dans l’aire de conception ou dans une vue XAML. Développez **Pinceau**, puis sélectionnez l’onglet **Dégradé linéaire**.

![Créer un dégradé linéaire dans Visual Studio](images/tool-gradient-brush-1.png)

*Création d’un dégradé linéaire dans Visual Studio*

Vous pouvez à présent modifier les couleurs des points de dégradé et modifier leurs positions à l’aide de la barre au bas de la fenêtre. Vous pouvez également ajouter de nouveaux points de dégradé en cliquant sur la barre, ou en supprimer en les faisant glisser à l’extérieur de la barre (voir la capture d’écran suivante).

![Barre au bas de la fenêtre de propriétés qui contrôle les points de dégradé](images/tool-gradient-brush-2.png)

*Curseur du paramètre de dégradé*

## <a name="radial-gradient-brushes"></a>Pinceaux de dégradés radiaux

Un [**RadialGradientBrush**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush) est dessiné dans une ellipse qui est définie par les propriétés [**Center**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.center), [**RadiusX**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusx) et [**RadiusY**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusy). Les couleurs du début du dégradé se trouvent au centre de l’ellipse et se terminent au niveau du rayon.

Les couleurs du dégradé radial sont définies par des points de couleur ajoutés à la propriété de collection [**GradientStops**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.gradientstops). Chaque point de dégradé spécifie une couleur et un décalage le long du dégradé.

L’origine du dégradé est définie par défaut au centre et peut être décalée à l’aide de la propriété [**GradientOrigin**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.gradientorigin).

[MappingMode](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.mappingmode) définit si [**Center**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.center), [**RadiusX**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusx), [**RadiusY**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusy) et [**GradientOrigin**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.gradientorigin) représentent des coordonnées relatives ou absolues.

Quand [**MappingMode**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.mappingmode) a la valeur `RelativeToBoundingBox`, les valeurs X et Y des trois propriétés sont traitées par rapport aux limites d’élément, où `(0,0)` représente le coin supérieur gauche et `(1,1)` représente le coin inférieur droit des limites d’élément pour les propriétés [**Center**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.center), [**RadiusX**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusx) et [**RadiusY**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusy) et `(0,0)` représente le centre de la propriété [**GradientOrigin**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.gradientorigin).

Quand [**MappingMode**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.mappingmode) a la valeur `Absolute`, les valeurs X et Y des trois propriétés sont traitées comme des coordonnées absolues dans les limites d’élément.

Cet exemple crée un dégradé linéaire avec quatre couleurs et l’utilise pour peindre un objet [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle).

```xml
<!-- This rectangle is painted with a radial gradient. -->
<Rectangle Width="200" Height="200">
    <Rectangle.Fill>
        <media:RadialGradientBrush>
            <GradientStop Color="Blue" Offset="0.0" />
            <GradientStop Color="Yellow" Offset="0.2" />
            <GradientStop Color="LimeGreen" Offset="0.4" />
            <GradientStop Color="LightBlue" Offset="0.6" />
            <GradientStop Color="Blue" Offset="0.8" />
            <GradientStop Color="LightGray" Offset="1" />
        </media:RadialGradientBrush>
    </Rectangle.Fill>
</Rectangle>
```

La couleur de chaque point entre les extrémités est interpolée de façon radiale sous la forme d’une combinaison de la couleur spécifiée par les deux points de dégradé englobants. L’image suivante met en évidence les points de dégradés de l’exemple précédent. 

![Points de dégradé](images/radial-gradient.png)

*Points de dégradé*

## <a name="image-brushes"></a>Pinceaux image

Un objet [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) peint une zone avec une image qui provient d’une source de fichier image. Vous définissez la propriété [**ImageSource**](/uwp/api/Windows.UI.Xaml.Media.ImageSource) sur le chemin de l’image à charger. En règle générale, la source d’image provient d’un élément **Content** qui fait partie des ressources de votre application.

Par défaut, quand vous utilisez un pinceau [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush), l’image associée est étirée de façon à remplir entièrement la zone peinte. Elle peut ainsi apparaître déformée si les proportions de cette zone sont différentes de celles de l’image. Vous pouvez changer ce comportement en modifiant la propriété [**Stretch**](/uwp/api/windows.ui.xaml.media.tilebrush.stretch). Pour cela, remplacez sa valeur par défaut **Fill** par la valeur **None**, **Uniform** ou **UniformToFill**.

L’exemple suivant crée un pinceau [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) avec la propriété [**ImageSource**](/uwp/api/Windows.UI.Xaml.Media.ImageSource) définie sur une image nommée licorice.jpg, que vous devez inclure comme ressource dans l’application. L’objet **ImageBrush** peint ensuite la zone définie par une forme [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse).

```xml
<Ellipse Height="200" Width="300">
   <Ellipse.Fill>
     <ImageBrush ImageSource="licorice.jpg" />
   </Ellipse.Fill>
</Ellipse>
```

![Rendu d’ImageBrush.](images/brushes-imagebrush.jpg)

*ImageBrush rendu*

[**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) et [**Image**](/uwp/api/Windows.UI.Xaml.Controls.Image) référencent un fichier source d’image par un URI (Uniform Resource Identifier), où ce fichier utilise plusieurs formats d’image possibles. Ces fichiers sources d’image sont spécifiés sous forme d’URI. Pour plus d’informations sur la spécification de sources d’images, sur les formats d’images utilisables et sur leur empaquetage dans une application, consultez [Image et ImageBrush](https://docs.microsoft.com/windows/uwp/controls-and-patterns/images-imagebrushes).

## <a name="brushes-and-text"></a>Pinceaux et texte

Vous pouvez également utiliser des pinceaux pour appliquer des caractéristiques de rendu à des éléments de texte. Par exemple, la propriété [**Foreground**](/uwp/api/windows.ui.xaml.controls.textblock.foreground) de [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) prend un pinceau [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush). Vous pouvez appliquer n’importe quel pinceau décrit ici à du texte. Toutefois, soyez vigilant avec les pinceaux appliqués au texte, car tout arrière-plan peut rendre le texte illisible si vous utilisez des pinceaux qui se fondent dans l’arrière-plan. Utilisez le pinceau [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) pour pouvoir lire l’élément de texte dans la plupart des cas, sauf si vous voulez que l’élément de texte soit principalement décoratif.

Même quand vous utilisez une couleur unie, veillez à ce que le contraste entre la couleur du texte que vous choisissez et la couleur d’arrière-plan du conteneur de disposition du texte soit suffisant. Le niveau de contraste entre le premier plan du texte et l’arrière-plan du conteneur du texte a de l’importance en termes d’accessibilité.

## <a name="webviewbrush"></a>WebViewBrush

Un objet [**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush) est un type particulier de pinceau qui peut accéder au contenu normalement affiché dans un contrôle [**WebView**](/uwp/api/Windows.UI.Xaml.Controls.WebView). Au lieu de restituer le contenu dans la zone de contrôle **WebView** rectangulaire, l’objet **WebViewBrush** peint ce contenu sur un autre élément qui a une propriété de type [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) pour une surface de rendu. L’objet **WebViewBrush** n’est pas approprié pour chaque scénario de pinceau, mais il est utile pour les transitions d’un objet **WebView**. Pour plus d’informations, consultez [**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush).

## <a name="xamlcompositionbrushbase"></a>XamlCompositionBrushBase

[**XamlCompositionBrushBase**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase) est une classe de base utilisée pour créer des pinceaux personnalisés utilisant [**CompositionBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush) pour peindre des éléments d’interface utilisateur XAML.

Ceci permet une interopérabilité « liste déroulante »entre les couches Windows.UI.Xaml et Windows.UI.Composition, comme décrit dans [**Vue d’ensemble de la couche visuelle**](/windows/uwp/composition/visual-layer). 

Pour créer un pinceau personnalisé, créez une classe qui hérite de XamlCompositionBrushBase et qui implémente les méthodes nécessaires.

Par exemple, ceci peut être utilisé pour appliquer des [**effets**](/uwp/composition/composition-effects) aux éléments d’interface utilisateur XAML à l’aide d’un [**CompositionEffectBrush**](/uwp/api/Windows.UI.Composition.CompositionEffectBrush), comme un **GaussianBlurEffect** ou un [**SceneLightingEffect**](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) qui contrôle les propriétés réfléchissantes d’un élément UIElement XAML quand celui-ci est éclairé par une [**XamlLight**](/uwp/api/windows.ui.xaml.media.xamllight).

Pour obtenir des exemples de code, consultez [**XamlCompositionBrushBase**](/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase).

## <a name="brushes-as-xaml-resources"></a>Pinceaux en tant que ressources XAML

Vous pouvez déclarer n’importe quel pinceau en tant que ressource XAML à clé dans un dictionnaire de ressources XAML. Ceci permet de répliquer facilement les mêmes valeurs de pinceau pour plusieurs éléments d’une interface utilisateur. Les valeurs de pinceau sont ensuite partagées et appliquées dans tous les cas où vous référencez la ressource de pinceau en tant qu’utilisation de [{StaticResource}](/uwp/xaml-platform/staticresource-markup-extension) dans votre XAML. Ceci inclut les cas où un modèle de contrôle XAML référence le pinceau partagé et où le modèle de contrôle est lui-même une ressource XAML à clé.

## <a name="brushes-in-code"></a>Pinceaux dans le code

Il est beaucoup plus courant de spécifier des pinceaux en XAML que d’utiliser du code pour les définir. Cela est dû au fait que les pinceaux sont généralement définis en tant que ressources XAML, et que les valeurs des pinceaux sont souvent le résultat d’outils de conception ou bien qu’elles font partie d’une définition d’interface utilisateur XAML. Néanmoins, pour les quelques cas où vous voulez définir un pinceau avec du code, tous les types [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) sont disponibles pour l’instanciation du code.

Pour créer un objet [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) dans du code, utilisez le constructeur qui prend un paramètre [**Color**](/uwp/api/Windows.UI.Color). Passez une valeur qui est une propriété statique de la classe [**Colors**](/uwp/api/windows.ui.colors), comme celle-ci :

```cs
SolidColorBrush blueBrush = new SolidColorBrush(Windows.UI.Colors.Blue);
```

```vb
Dim blueBrush as SolidColorBrush = New SolidColorBrush(Windows.UI.Colors.Blue)
```

```cppwinrt
Windows::UI::Xaml::Media::SolidColorBrush blueBrush{ Windows::UI::Colors::Blue() };
```

```cpp
blueBrush = ref new SolidColorBrush(Windows::UI::Colors::Blue);
```

Pour [**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush) et [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush), utilisez le constructeur par défaut, puis appelez d’autres API avant d’essayer d’utiliser ce pinceau pour une propriété d’interface utilisateur.

-   [**ImageSource**](/uwp/api/windows.ui.xaml.media.imagebrush.imagesourceproperty) nécessite une classe [**BitmapImage**](/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage) (et non pas un URI) quand vous définissez un [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) en utilisant du code. Si votre source est un flux, utilisez la méthode [**SetSourceAsync**](/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync) pour initialiser la valeur. Si votre source est un URI incluant du contenu de votre application qui utilise les schémas **ms-appx** ou **ms-resource**, utilisez le constructeur [**BitmapImage**](/uwp/api/windows.ui.xaml.media.imaging.bitmapimage) qui prend un URI. Vous pouvez également envisager de gérer l’événement [**ImageOpened**](/uwp/api/windows.ui.xaml.media.imagebrush.imageopened) s’il y a des problèmes de délai liés à la récupération ou au décodage de la source de l’image, auquel cas il peut être nécessaire d’afficher un contenu alternatif tant que la source de l’image n’est pas disponible.
-   Pour la classe [**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush), vous pouvez avoir besoin d’appeler la méthode [**Redraw**](/uwp/api/windows.ui.xaml.controls.webviewbrush.redraw) si vous avez récemment réinitialisé la propriété [**SourceName**](/uwp/api/windows.ui.xaml.controls.webviewbrush.sourcename) ou que le contenu de la classe [**WebView**](/uwp/api/Windows.UI.Xaml.Controls.WebView) est également changé avec du code.

Pour obtenir des exemples de code, consultez [**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush),  [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) et [**XamlCompositionBrushBase**](/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase).
 

 




