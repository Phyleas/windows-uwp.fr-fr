---
title: Transitions de page
description: Découvrez comment utiliser des transitions de page plateforme Windows universelle (UWP) pour fournir aux utilisateurs des commentaires sur la relation entre les pages de votre application.
template: detail.hbs
ms.date: 04/08/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: c77f99e170bdfe6689a9bfd4e8d8075ec2154d28
ms.sourcegitcommit: 45dec3dc0f14934b8ecf1ee276070b553f48074d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89094675"
---
# <a name="page-transitions"></a>Transitions de page

Les transitions de page parcourent les utilisateurs entre les pages d’une application, en fournissant des commentaires sous la forme de la relation entre les pages. Les transitions de page permettent aux utilisateurs de comprendre s’ils se trouvent en haut d’une hiérarchie de navigation, s’ils se déplacent entre les pages frères ou naviguent plus profondément dans la hiérarchie des pages.

Deux animations différentes sont fournies pour la navigation entre les pages d’une application, l' *actualisation* et l' *exploration*des pages, et sont représentées par des sous-classes de [**NavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationtransitioninfo).

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Si l’application de la <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong> est installée, cliquez ici pour <a href="xamlcontrolsgallery:/item/PageTransition">ouvrir l’application et voir les transitions de page en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="page-refresh"></a>Actualisation de la page

L’actualisation de page est une combinaison d’une animation glisser vers le haut et d’une animation de fondu pour le contenu entrant. Utilisez l’actualisation de la page lorsque l’utilisateur se trouve au sommet d’une pile de navigation, par exemple en naviguant entre les onglets ou les éléments de navigation vers la gauche.

Le sentiment souhaité est que l’utilisateur a commencé.

![animation page Refresh](images/page-refresh.gif)

L’animation d’actualisation de la page est représentée par [**EntranceNavigationTransitionInfoClass**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.entrancenavigationtransitioninfo).

```csharp
// Explicitly play the page refresh animation
myFrame.Navigate(typeof(Page2), null, new EntranceNavigationTransitionInfo());

```

**Remarque**: une [**image**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame) utilise automatiquement [**NavigationThemeTransition**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationthemetransition) pour animer la navigation entre deux pages. Par défaut, l’animation est actualiser les pages.

## <a name="drill"></a>Drill

Utilisez l’exploration quand les utilisateurs naviguent plus profondément dans une application, par exemple en affichant plus d’informations après avoir sélectionné un élément.

Le sentiment souhaité est que l’utilisateur est allé plus loin dans l’application.

![animation d’exploration](images/drill.gif)

L’animation Drill est représentée par la classe [**DrillInNavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.drillinnavigationtransitioninfo) .

```csharp
// Play the drill in animation
myFrame.Navigate(typeof(Page2), null, new DrillInNavigationTransitionInfo());
```

## <a name="horizontal-slide"></a>Diapositive horizontale

Utilisez la diapositive horizontale pour montrer que les pages sœurs s’affichent à côté les unes des autres. Le contrôle [NavigationView](../controls-and-patterns/navigationview.md) utilise automatiquement cette animation pour le haut de la navigation, mais si vous créez votre propre expérience de navigation horizontale, vous pouvez implémenter une diapositive horizontale avec SlideNavigationTransitionInfo.

Le sentiment souhaité est que l’utilisateur navigue entre les pages les unes à côté des autres. 

```csharp
// Navigate to the right, ie. from LeftPage to RightPage
myFrame.Navigate(typeof(RightPage), null, new SlideNavigationTransitionInfo() { Effect = SlideNavigationTransitionEffect.FromRight } );

// Navigate to the left, ie. from RightPage to LeftPage
myFrame.Navigate(typeof(LeftPage), null, new SlideNavigationTransitionInfo() { Effect = SlideNavigationTransitionEffect.FromLeft } );
```

## <a name="suppress"></a>Suppress

Pour éviter de lancer une animation pendant la navigation, utilisez [**SuppressNavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) à la place des autres sous-types **NavigationTransitionInfo** .

```csharp
// Suppress the default animation
myFrame.Navigate(typeof(Page2), null, new SuppressNavigationTransitionInfo());
```

La suppression de l’animation est utile si vous générez votre propre transition à l’aide d' [animations connectées](connected-animation.md) ou d’animations d’affichage/de masquage implicites.

## <a name="backwards-navigation"></a>Navigation vers l’arrière

Vous pouvez utiliser `Frame.GoBack(NavigationTransitionInfo)` pour lire une transition spécifique lors de la navigation vers l’arrière.

Cela peut être utile lorsque vous modifiez le comportement de navigation dynamiquement en fonction de la taille de l’écran. par exemple, dans un scénario maître/détail réactif.

## <a name="related-topics"></a>Rubriques connexes

- [Naviguer entre deux pages](../basics/navigate-between-two-pages.md)
- [Motion dans les applications UWindowsWP](index.md)
