---
ms.assetid: 02141F86-355E-4046-86EA-2A89D615B7DB
title: Utiliser des pinceaux
description: Les objets Brush permettent de peindre les intérieurs ou les contours de formes, de textes et de parties de contrôles, pour que l’objet peint soit visible dans une interface utilisateur.
ms.date: 07/13/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 20e38b24195bf3e476fa68284813bfd3a8cd2e6c
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684173"
---
# <a name="using-brushes-to-paint-backgrounds-foregrounds-and-outlines"></a>Utilisation des pinceaux pour peindre des arrière-plans, des premiers plans et des contours

Vous utilisez des objets [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) pour peindre les intérieurs ou les contours de formes, de texte et de parties de contrôles XAML, afin que l’objet peint soit visible dans une interface utilisateur. Examinons les pinceaux disponibles et leur utilisation.

> **API importantes** :  [classe Brush](/uwp/api/Windows.UI.Xaml.Media.Brush)

## <a name="introduction-to-brushes"></a>Présentation des pinceaux

Pour peindre un objet comme un objet [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape) ou les parties d’un objet [**Control**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control) qui apparaît sur le canevas de l’application, vous utilisez un objet [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush). Par exemple, si vous définissez la propriété [**Fill**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.shape.fill) de l’objet **Shape** ou les propriétés [**Background**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.background) et [**Foreground**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.foreground) d’un objet **Control** sur une valeur **Brush**, cette valeur **Brush** détermine la façon dont l’élément d’interface utilisateur est peint ou restitué dans l’interface utilisateur. 

Les différents types de pinceaux sont les suivants : 
-   [**AcrylicBrush**](/uwp/api/windows.ui.xaml.media.acrylicbrush)
-   [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)
-   [**LinearGradientBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush) 
-   [**ImageBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush)
-   [**WebViewBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush)
-   [**XamlCompositionBrushBase**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)

## <a name="solid-color-brushes"></a>Pinceaux de couleur unie

Un pinceau [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) peint une zone en une seule couleur [**Color**](https://docs.microsoft.com/uwp/api/Windows.UI.Color), comme le rouge ou le bleu. Il s’agit du pinceau le plus élémentaire. Il existe trois façons de définir un pinceau **SolidColorBrush** et la couleur associée en XAML : via des noms de couleur prédéfinie, des valeurs de couleur hexadécimales ou la syntaxe des éléments de propriété.

### <a name="predefined-color-names"></a>Noms de couleur prédéfinie

Vous pouvez utiliser un nom de couleur prédéfinie, comme [**Yellow**](https://docs.microsoft.com/uwp/api/windows.ui.colors.yellow) ou [**Magenta**](https://docs.microsoft.com/uwp/api/windows.ui.colors.magenta). 256 couleurs nommées sont disponibles. L’analyseur XAML convertit le nom de la couleur en une structure [**Color**](https://docs.microsoft.com/uwp/api/Windows.UI.Color) avec les canaux de couleur appropriés. Étant donné que les 256 couleurs nommées s’appuient sur les noms de couleurs *X11* issus de la spécification CSS3 (Cascading Style Sheets, Level 3), vous connaissez peut-être déjà cette liste de couleurs nommées si vous avez de l’expérience en matière de développement ou de conception de sites web.

Voici un exemple qui définit la propriété [**Fill**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.shape.fill) d’un objet [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) sur la couleur prédéfinie [**Red**](https://docs.microsoft.com/uwp/api/windows.ui.colors.red).

```xml
<Rectangle Width="100" Height="100" Fill="Red" />
```

Cette image montre l’objet [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) appliqué à l’objet [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle).

![Un objet SolidColorBrush rendu.](images/brushes-solidcolorbrush.jpg)

Si vous définissez un objet [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) en utilisant un code plutôt que du XAML, chaque couleur nommée est disponible en tant que valeur de propriété statique de la classe [**Colors**](https://docs.microsoft.com/uwp/api/windows.ui.colors). Par exemple, pour déclarer une valeur [**Color**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.solidcolorbrush.color) d’un objet **SolidColorBrush** pour représenter la couleur nommée « Orchid », définissez la valeur **Color** sur la valeur statique [**Colors.Orchid**](https://docs.microsoft.com/uwp/api/windows.ui.colors.orchid).

### <a name="hexadecimal-color-values"></a>Valeurs de couleur hexadécimales

Vous pouvez utiliser une chaîne au format hexadécimal pour déclarer des valeurs de couleurs précises sur 24 bits avec un canal alpha sur 8 bits pour un objet [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush). Deux caractères dans la plage 0 à F définissent chaque valeur du composant, et l’ordre des valeurs de composants de la chaîne hexadécimale est le suivant : canal alpha (opacité), canal rouge, canal vert et canal bleu (**ARVB**). Par exemple, la valeur hexadécimale « \#FFFF0000 » définit un rouge entièrement opaque (alpha=« FF », rouge=« FF », vert=« 00 » et bleu=« 00 »).

L’exemple XAML suivant définit la propriété [**Fill**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.shape.fill) d’un objet [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) sur la valeur hexadécimale « \#FFFF0000 » et aboutit au même résultat que l’utilisation de la couleur nommée [**Colors.Red**](https://docs.microsoft.com/uwp/api/windows.ui.colors.red).

```xml
<StackPanel>
  <Rectangle Width="100" Height="100" Fill="#FFFF0000" />
</StackPanel>
```

### <a name="span-idproperty_element_syntax__spanspan-idproperty_element_syntax__spanspan-idproperty_element_syntax__spanproperty-element-syntax"></a><span id="Property_element_syntax__"></span><span id="property_element_syntax__"></span><span id="PROPERTY_ELEMENT_SYNTAX__"></span>Syntaxe des éléments de la propriété

Vous pouvez utiliser la syntaxe des éléments de propriété pour définir un objet [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush). Cette syntaxe est plus longue que les méthodes précédentes, mais elle vous permet de spécifier des valeurs de propriétés supplémentaires sur un élément, comme la propriété [**Opacity**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.brush.opacity). Pour plus d’informations sur la syntaxe XAML, notamment la syntaxe des éléments de propriété, consultez [Vue d’ensemble du langage XAML](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-overview) et [Guide la syntaxe XAML](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-syntax-guide).

Dans les exemples précédents, la chaîne « SolidColorBrush » n’apparaît nulle part dans la syntaxe. Le pinceau en cours de création est généré implicitement et automatiquement, dans le cadre d’un raccourci délibéré du langage XAML qui assure la simplicité de la définition de l’interface utilisateur dans les cas les plus courants. L’exemple suivant crée un objet [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) et crée explicitement l’objet [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) en tant que valeur d’élément pour une propriété [**Rectangle.Fill**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.shape.fill). La propriété [**Color**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.solidcolorbrush.color) de l’objet **SolidColorBrush** est définie sur [**Blue**](https://docs.microsoft.com/uwp/api/windows.ui.colors.blue), tandis que la propriété [**Opacity**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.brush.opacity) est définie sur 0,5.

```xml
<Rectangle Width="200" Height="150">
    <Rectangle.Fill>
        <SolidColorBrush Color="Blue" Opacity="0.5" />
    </Rectangle.Fill>
</Rectangle>
```

## <a name="span-idlinear_gradient_brushes_spanspan-idlinear_gradient_brushes_spanspan-idlinear_gradient_brushes_spanlinear-gradient-brushes"></a><span id="Linear_gradient_brushes_"></span><span id="linear_gradient_brushes_"></span><span id="LINEAR_GRADIENT_BRUSHES_"></span>Pinceaux de dégradé linéaire

Un objet [**LinearGradientBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush) peint une zone avec un dégradé défini le long d’une ligne. Cette ligne est appelée *axe de dégradé*. Vous spécifiez les couleurs du dégradé et leur emplacement le long de l’axe avec des objets [**GradientStop**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.GradientStop). Par défaut, l’axe de dégradé va du coin supérieur gauche vers le coin inférieur droit de la zone peinte par le pinceau, ce qui produit un ombrage en diagonale.

Le [**GradientStop**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.GradientStop) est le composant de base d’un pinceau de dégradé. Un point de dégradé spécifie la propriété [**Color**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.gradientstop.color) du pinceau à un [**Offset**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.gradientstop.offset) le long de l’axe de dégradé, quand le pinceau est appliqué à la zone à peindre.

La propriété [**Color**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.gradientstop.color) du point de dégradé spécifie sa couleur. Vous pouvez définir la couleur en utilisant un nom de couleur prédéfini ou en spécifiant des valeurs **ARVB** hexadécimales.

La propriété [**Offset**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.gradientstop.offset) d’un objet [**GradientStop**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.GradientStop) spécifie la position de chaque objet **GradientStop** le long de l’axe de dégradé. **Offset** est une valeur **double** de 0 à 1. Si la propriété **Offset** a pour valeur 0, l’objet **GradientStop** est placé au début de l’axe de dégradé, c’est-à-dire près de son [**StartPoint**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.lineargradientbrush.startpoint). Si la propriété **Offset** a la valeur 1, l’objet **GradientStop** est placé au point [**EndPoint**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.lineargradientbrush.endpoint). Au minimum, un objet [**LinearGradientBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush) utile doit comporter deux valeurs **GradientStop**, chaque valeur **GradientStop** devant spécifier une propriété [**Color**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.gradientstop.color) différente et avoir une propriété **Offset** différente dont la valeur est comprise entre 0 et 1.

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

La couleur de chaque point entre les extrémités est interpolée de façon linéaire sous la forme d’une combinaison de la couleur spécifiée par les deux points de dégradé. L’illustration met en évidence les points de dégradés de l’exemple précédent. Les cercles indiquent la position des points de dégradé, tandis que la ligne en pointillés représente l’axe de dégradé.

![Points de dégradé](images/linear-gradients-stops.png) Vous pouvez changer la ligne de positionnement des points de dégradé en affectant aux propriétés [**StartPoint**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.lineargradientbrush.startpoint) et [**EndPoint**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.lineargradientbrush.endpoint) des valeurs différentes des valeurs par défaut initiales `(0,0)` et `(1,1)`. En modifiant les valeurs des coordonnées de **StartPoint** et de **EndPoint**, vous pouvez créer des dégradés horizontaux ou verticaux, inverser le sens du dégradé ou condenser son étalement afin de l’appliquer à une plage plus petite que la zone peinte totale. Pour condenser le dégradé, vous définissez **StartPoint** et/ou **EndPoint** sur des valeurs comprises entre 0 et 1. Par exemple, pour obtenir un dégradé horizontal dont l’effet se produit en totalité sur la moitié gauche du pinceau, tandis que le côté droit conserve la dernière couleur unie définie par l’objet [**GradientStop**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.GradientStop), vous spécifiez une propriété **StartPoint** sur la valeur `(0,0)` et une propriété **EndPoint** sur la valeur `(0.5,0)`.

### <a name="span-iduse_tools_to_make_gradientsspanspan-iduse_tools_to_make_gradientsspanspan-iduse_tools_to_make_gradientsspanuse-tools-to-make-gradients"></a><span id="Use_tools_to_make_gradients"></span><span id="use_tools_to_make_gradients"></span><span id="USE_TOOLS_TO_MAKE_GRADIENTS"></span>Utiliser des outils pour rendre les dégradés

Maintenant que vous savez comment fonctionnent les dégradés linéaires, vous pouvez utiliser Visual Studio ou Blend pour les créer plus facilement. Pour créer un dégradé, sélectionnez l’objet auquel vous voulez l’appliquer dans l’aire de conception ou dans une vue XAML. Développez **Pinceau**, puis sélectionnez l’onglet **Dégradé linéaire** (voir la capture d’écran suivante).

![Créer un dégradé linéaire avec Visual Studio](images/tool-gradient-brush-1.png)

Vous pouvez à présent modifier les couleurs des points de dégradé et modifier leurs positions à l’aide de la barre au bas de la fenêtre. Vous pouvez également ajouter de nouveaux points de dégradé en cliquant sur la barre, ou en supprimer en les faisant glisser à l’extérieur de la barre (voir la capture d’écran suivante).

![Barre au bas de la fenêtre de propriétés qui contrôle les points de dégradé.](images/tool-gradient-brush-2.png)

## <a name="span-idimage_brushesspanspan-idimage_brushesspanspan-idimage_brushesspanimage-brushes"></a><span id="Image_brushes"></span><span id="image_brushes"></span><span id="IMAGE_BRUSHES"></span>Pinceaux d’image

Un objet [**ImageBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush) peint une zone avec une image qui provient d’une source de fichier image. Vous définissez la propriété [**ImageSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageSource) sur le chemin de l’image à charger. En règle générale, la source d’image provient d’un élément **Content** qui fait partie des ressources de votre application.

Par défaut, quand vous utilisez un pinceau [**ImageBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush), l’image associée est étirée de façon à remplir entièrement la zone peinte. Elle peut ainsi apparaître déformée si les proportions de cette zone sont différentes de celles de l’image. Vous pouvez changer ce comportement en modifiant la propriété [**Stretch**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.tilebrush.stretch). Pour cela, remplacez sa valeur par défaut **Fill** par la valeur **None**, **Uniform** ou **UniformToFill**.

L’exemple suivant crée un pinceau [**ImageBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush) avec la propriété [**ImageSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageSource) définie sur une image nommée licorice.jpg, que vous devez inclure comme ressource dans l’application. L’objet **ImageBrush** peint ensuite la zone définie par une forme [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse).

```xml
<Ellipse Height="200" Width="300">
   <Ellipse.Fill>
     <ImageBrush ImageSource="licorice.jpg" />
   </Ellipse.Fill>
</Ellipse>
```

Voici le rendu de l’objet [**ImageBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush).

![Rendu d’ImageBrush.](images/brushes-imagebrush.jpg)

[**ImageBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush) et [**Image**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) référencent un fichier source d’image par un URI (Uniform Resource Identifier), où ce fichier utilise plusieurs formats d’image possibles. Ces fichiers sources d’image sont spécifiés sous forme d’URI. Pour plus d’informations sur la spécification de sources d’images, sur les formats d’images utilisables et sur leur empaquetage dans une application, consultez [Image et ImageBrush](https://docs.microsoft.com/windows/uwp/controls-and-patterns/images-imagebrushes).

## <a name="brushes-and-text"></a>Pinceaux et texte

Vous pouvez également utiliser des pinceaux pour appliquer des caractéristiques de rendu à des éléments de texte. Par exemple, la propriété [**Foreground**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.foreground) de [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) prend un pinceau [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush). Vous pouvez appliquer n’importe quel pinceau décrit ici à du texte. Soyez cependant prudent quand vous appliquez un pinceau à du texte, car vous risquez de rendre le texte illisible si vous utilisez des pinceaux qui se fondent dans l’arrière-plan du texte ou qui affectent la netteté du contour des caractères du texte. Utilisez le pinceau [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) pour pouvoir lire l’élément de texte dans la plupart des cas, sauf si vous voulez que l’élément de texte soit principalement décoratif.

Même quand vous utilisez une couleur unie, veillez à ce que le contraste entre la couleur du texte que vous choisissez et la couleur d’arrière-plan du conteneur de disposition du texte soit suffisant. Le niveau de contraste entre le premier plan du texte et l’arrière-plan du conteneur du texte a de l’importance en termes d’accessibilité.

## <a name="webviewbrush"></a>WebViewBrush

Un objet [**WebViewBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush) est un type particulier de pinceau qui peut accéder au contenu normalement affiché dans un contrôle [**WebView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView). Au lieu de restituer le contenu dans la zone de contrôle **WebView** rectangulaire, l’objet **WebViewBrush** peint ce contenu sur un autre élément qui a une propriété de type [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) pour une surface de rendu. L’objet **WebViewBrush** n’est pas approprié pour chaque scénario de pinceau, mais il est utile pour les transitions d’un objet **WebView**. Pour plus d’informations, consultez [**WebViewBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush).

## <a name="xamlcompositionbrushbase"></a>XamlCompositionBrushBase

[**XamlCompositionBrushBase**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase) est une classe de base utilisée pour créer des pinceaux personnalisés utilisant [**CompositionBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush) pour peindre des éléments d’interface utilisateur XAML.

Ceci permet une interopérabilité « liste déroulante »entre les couches Windows.UI.Xaml et Windows.UI.Composition, comme décrit dans [**Vue d’ensemble de la couche visuelle**](/windows/uwp/composition/visual-layer). 

Pour créer un pinceau personnalisé, créez une classe qui hérite de XamlCompositionBrushBase et qui implémente les méthodes nécessaires.

Par exemple, ceci peut être utilisé pour appliquer des [**effets**](/windows/uwp/composition/composition-effects) aux éléments d’interface utilisateur XAML à l’aide d’un [**CompositionEffectBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush), comme un **GaussianBlurEffect** ou un [**SceneLightingEffect**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) qui contrôle les propriétés réfléchissantes d’un élément UIElement XAML quand celui-ci est éclairé par une [**XamlLight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamllight).

Pour obtenir des exemples de code, consultez la page de référence de la classe [**XamlCompositionBrushBase**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase).

## <a name="brushes-as-xaml-resources"></a>Pinceaux en tant que ressources XAML

Vous pouvez déclarer n’importe quel pinceau en tant que ressource XAML à clé dans un dictionnaire de ressources XAML. Ceci permet de répliquer facilement les mêmes valeurs de pinceau pour plusieurs éléments d’une interface utilisateur. Les valeurs de pinceau sont ensuite partagées et appliquées dans tous les cas où vous référencez la ressource de pinceau en tant qu’utilisation de [{StaticResource}](https://docs.microsoft.com/windows/uwp/xaml-platform/staticresource-markup-extension) dans votre XAML. Ceci inclut les cas où un modèle de contrôle XAML référence le pinceau partagé et où le modèle de contrôle est lui-même une ressource XAML à clé.

## <a name="brushes-in-code"></a>Pinceaux dans le code

Il est beaucoup plus courant de spécifier des pinceaux en XAML que d’utiliser du code pour les définir. Cela est dû au fait que les pinceaux sont généralement définis en tant que ressources XAML, et que les valeurs des pinceaux sont souvent le résultat d’outils de conception ou bien qu’elles font partie d’une définition d’interface utilisateur XAML. Néanmoins, pour les quelques cas où vous voulez définir un pinceau avec du code, tous les types [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) sont disponibles pour l’instanciation du code.

Pour créer un objet [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) dans du code, utilisez le constructeur qui prend un paramètre [**Color**](https://docs.microsoft.com/uwp/api/Windows.UI.Color). Passez une valeur qui est une propriété statique de la classe [**Colors**](https://docs.microsoft.com/uwp/api/windows.ui.colors), comme celle-ci :

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

Pour [**WebViewBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush) et [**ImageBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush), utilisez le constructeur par défaut, puis appelez d’autres API avant d’essayer d’utiliser ce pinceau pour une propriété d’interface utilisateur.

-   [**ImageSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imagebrush.imagesourceproperty) nécessite une classe [**BitmapImage**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage) (et non pas un URI) quand vous définissez un [**ImageBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush) en utilisant du code. Si votre source est un flux, utilisez la méthode [**SetSourceAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync) pour initialiser la valeur. Si votre source est un URI incluant du contenu de votre application qui utilise les schémas **ms-appx** ou **ms-resource**, utilisez le constructeur [**BitmapImage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage) qui prend un URI. Vous pouvez également envisager de gérer l’événement [**ImageOpened**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imagebrush.imageopened) s’il y a des problèmes de délai liés à la récupération ou au décodage de la source de l’image, auquel cas il peut être nécessaire d’afficher un contenu alternatif tant que la source de l’image n’est pas disponible.
-   Pour la classe [**WebViewBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush), vous pouvez avoir besoin d’appeler la méthode [**Redraw**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webviewbrush.redraw) si vous avez récemment réinitialisé la propriété [**SourceName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webviewbrush.sourcename) ou que le contenu de la classe [**WebView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) est également changé avec du code.

Pour obtenir des exemples de code, consultez les pages de référence pour [**WebViewBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush), [**ImageBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush) et [**XamlCompositionBrushBase**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase).
 

 




