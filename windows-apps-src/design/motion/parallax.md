---
title: Utiliser l’effet de parallaxe pour ajouter de la profondeur et un déplacement à votre application
description: Découvrez comment utiliser le contrôle ParallaxView dans une application UWP pour créer un effet visuel où les éléments plus proches de la visionneuse se déplacent plus rapidement que les éléments en arrière-plan.
ms.assetid: ''
label: Parallax View
template: detail.hbs
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: abarlow
design-contact: conrwi
dev-contact: stpete
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 5eac1b5d95dff4887258278f9ff700adaf663194
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175543"
---
# <a name="parallax"></a>Parallaxe

La parallaxe est un effet visuel où les éléments plus proches de la visionneuse se déplacent plus rapidement que les éléments en arrière-plan. La parallaxe crée un sentiment de profondeur, de perspective et de mouvement. Dans une application UWP, vous pouvez utiliser le contrôle ParallaxView pour créer un effet parallaxe.  

> **API de la bibliothèque d’interface utilisateur Windows :** [classe ParallaxView](/uwp/api/Microsoft.UI.Xaml.Controls.Parallaxview), [propriété VerticalShift](/uwp/api/Microsoft.UI.Xaml.Controls.Parallaxview.VerticalShift), [propriété HorizontalShift](/uwp/api/Microsoft.UI.Xaml.Controls.Parallaxview.HorizontalShift)
>
> **API de plateforme**: [classe ParallaxView](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview), [propriété VerticalShift](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.VerticalShift), [propriété HorizontalShift](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.HorizontalShift)

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Si l’application de la <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong> est installée, cliquez ici pour <a href="xamlcontrolsgallery:/item/ParallaxView">ouvrir l’application et voir le ParallaxView en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="parallax-and-the-fluent-design-system"></a>Parallaxe et le système de conception Fluent

 Le système Fluent Design vous aide à créer une interface utilisateur moderne et claire qui incorpore de la lumière, de la profondeur, du mouvement, des matières et une mise à l’échelle. Parallaxe est un composant du système de conception Fluent qui ajoute des mouvements, des détails et une mise à l’échelle à votre application. Pour plus d’informations, consultez [Vue d’ensemble de Fluent Design](/windows/apps/fluent-design-system).

## <a name="how-it-works-in-a-user-interface"></a>Fonctionnement dans une interface utilisateur

Dans une interface utilisateur, vous pouvez créer un effet de parallaxe en déplaçant différents objets à des vitesses différentes lorsque l’interface utilisateur fait défiler le curseur ou le panoramique. <!-- Parallax is an important tool in adding depth to applications along with other techniques like transition animations, perspective tilt, and layering. --> Pour illustrer, observons deux couches de contenu, une liste et une image d’arrière-plan.  La liste est placée en haut de l’image d’arrière-plan qui donne à l’illusion que la liste peut être plus proche de la visionneuse.  À présent, pour obtenir l’effet de parallaxe, nous souhaitons que l’objet se rapproche le plus de l’objet plus loin.  Lorsque l’utilisateur fait défiler l’interface, la liste se déplace à un débit plus rapide que l’image d’arrière-plan, ce qui crée l’illusion de profondeur.

 ![Un exemple de parallaxe avec une liste et une image d’arrière-plan](images/_Parallax_v2.gif)

 
## <a name="using-the-parallaxview-control-to-create-a-parallax-effect"></a>Utilisation du contrôle ParallaxView pour créer un effet parallaxe

Pour créer un effet de parallaxe, vous utilisez le contrôle [ParallaxView](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview) . Ce contrôle lie la position de défilement d’un élément de premier plan, tel qu’une liste, à un élément d’arrière-plan, tel qu’une image. Lorsque vous faites défiler l’élément de premier plan, il anime l’élément d’arrière-plan pour créer un effet de parallaxe. 

Pour utiliser le contrôle ParallaxView, vous fournissez un élément source, un élément d’arrière-plan, puis définissez les propriétés [VerticalShift](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.VerticalShift) (pour le défilement vertical) et/ou [HorizontalShift](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.HorizontalShift) (pour le défilement horizontal) sur une valeur supérieure à zéro. 
* La propriété source prend une référence à l’élément Foreground. Pour que l’effet parallaxe se produise, le premier plan doit être un [ScrollViewer](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) ou un élément qui contient un ScrollViewer, tel qu’un [ListView](/uwp/api/windows.ui.xaml.controls.listview) ou un [RichTextBox](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox). 

* Pour définir l’élément d’arrière-plan, ajoutez cet élément en tant qu’enfant du contrôle ParallaxView. L’élément d’arrière-plan peut être n’importe quel [UIElement](/uwp/api/windows.ui.xaml.uielement), tel qu’une [image](/uwp/api/Windows.UI.Xaml.Controls.Image) ou un panneau qui contient des éléments d’interface utilisateur supplémentaires. 

Pour créer un effet de parallaxe, ParallaxView doit se trouver derrière l’élément de premier plan. Les panneaux [grille](/uwp/api/windows.ui.xaml.controls.grid) et [canevas](/uwp/api/windows.ui.xaml.controls.canvas) vous permettent de superposer des éléments les uns aux autres, afin qu’ils fonctionnent bien avec le contrôle ParallaxView.  

Cet exemple crée un effet de parallaxe pour une liste :
 
```xaml
<Grid>
    <ParallaxView Source="{x:Bind ForegroundElement}" VerticalShift="50"> 
    
        <!-- Background element --> 
        <Image x:Name="BackgroundImage" Source="Assets/turntable.png"
               Stretch="UniformToFill"/>
    </ParallaxView>
    
    <!-- Foreground element -->
    <ListView x:Name="ForegroundElement">
       <x:String>Item 1</x:String> 
       <x:String>Item 2</x:String> 
       <x:String>Item 3</x:String> 
       <x:String>Item 4</x:String> 
       <x:String>Item 5</x:String>     
       <x:String>Item 6</x:String> 
       <x:String>Item 7</x:String> 
       <x:String>Item 8</x:String> 
       <x:String>Item 9</x:String> 
       <x:String>Item 10</x:String>     
       <x:String>Item 11</x:String> 
       <x:String>Item 13</x:String> 
       <x:String>Item 14</x:String> 
       <x:String>Item 15</x:String> 
       <x:String>Item 16</x:String>     
       <x:String>Item 17</x:String> 
       <x:String>Item 18</x:String> 
       <x:String>Item 19</x:String> 
       <x:String>Item 20</x:String> 
       <x:String>Item 21</x:String>        
    </ListView>
</Grid>
```    

Le ParallaxView ajuste automatiquement la taille de l’image pour qu’elle fonctionne pour l’opération de parallaxe. vous n’avez donc pas à vous soucier de la déconnexion de l’image.

## <a name="customizing-the-parallax-effect"></a>Personnalisation de l’effet parallaxe 

Les propriétés VerticalShift et HorizontalShift vous permettent de contrôler le degré de l’effet de parallaxe.

* La propriété VerticalShift spécifie le décalage vertical de l’arrière-plan pendant toute l’opération de parallaxe. La valeur 0 signifie que l’arrière-plan ne se déplace pas du tout.
* La propriété HorizontalShift spécifie le décalage horizontal de l’arrière-plan pendant toute l’opération de parallaxe. La valeur 0 signifie que l’arrière-plan ne se déplace pas du tout.

Des valeurs plus élevées créent un effet plus spectaculaire. 

Pour obtenir la liste complète des méthodes de personnalisation de la parallaxe, consultez la classe ParallaxView. 

## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées

- Utiliser la fonction parallaxe dans les listes avec une image d’arrière-plan
- Envisagez d’utiliser la parallaxe dans ListViewItems quand les ListViewItems contiennent une image
- Ne l’utilisez pas partout, une utilisation abusive peut réduire son impact

## <a name="related-articles"></a>Articles connexes

- [ParallaxView, classe](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview) 
- [Fluent Design pour UWP](/windows/apps/fluent-design-system)
- [Science in the System: Fluent Design and Depth](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)