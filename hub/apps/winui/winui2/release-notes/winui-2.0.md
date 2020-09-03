---
title: Notes de publication de WinUI 2.0
description: Notes de publication de WinUI 2.0.
ms.date: 07/15/2020
ms.topic: article
ms.openlocfilehash: eb1cde864427768ab1adb3b982e4acc5b58906db
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154813"
---
# <a name="windows-ui-library-20"></a>Bibliothèque d’IU Windows 2.0

WinUI 2.0 est la première version publique de la bibliothèque d’IU Windows.

WinUI est le moyen le plus simple de créer des expériences Fluent Design intéressantes pour Windows.

Elle comprend deux packages NuGet :

* [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml) : contrôles et Fluent Design pour les applications UWP. Il s’agit du package WinUI principal.

* [Microsoft.UI.Xaml.Core.Direct](https://www.nuget.org/packages/Microsoft.UI.Xaml.Core.Direct) : API de bas niveau à utiliser dans les composants d’intergiciel (middleware).

Vous pouvez télécharger et utiliser des packages WinUI dans votre application à l’aide du gestionnaire de package NuGet. Pour plus d’informations, consultez [Bien démarrer avec la bibliothèque d’interface utilisateur Windows](/uwp/toolkits/winui/getting-started).

WinUI est un projet open source hébergé sur GitHub. Vous pouvez nous faire part de vos rapports de bogues, demandes de fonctionnalités et contributions de code de la Communauté dans le [dépôt de la bibliothèque d’interface utilisateur Windows](https://aka.ms/winui).

## <a name="microsoftuixaml-20181011001"></a>Microsoft.UI.Xaml 2.0.181011001

Octobre 2018

Il s’agit de la première version du [package NuGet Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml). Elle comprend des fonctionnalités et des contrôles Fluent natifs officiels pour les applications UWP Windows.

### <a name="new-features"></a>Nouvelles fonctionnalités

Les contrôles et les modèles de cette version sont les suivants :

| Fonctionnalité | Description |
| --- | --- |
|[AcrylicBrush]( /uwp/api/microsoft.ui.xaml.media.acrylicbrush)| Peint une zone avec un matériau semi-transparent qui utilise plusieurs effets, notamment le flou et une texture du bruit.|
|[BitmapIconSource]( /uwp/api/microsoft.ui.xaml.controls.bitmapiconsource)| Représente une source d’icône qui utilise une image bitmap en guise de contenu.|
|[ColorPicker]( /uwp/api/microsoft.ui.xaml.controls.colorpicker)| Représente un contrôle qui permet à l’utilisateur de choisir une couleur à l’aide d’un spectre de couleurs, de curseurs et d’une entrée de texte.|
|[CommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.commandbarflyout)|Représente un menu volant spécialisé qui fournit la disposition pour AppBarButton et les éléments de commande associés.|
|[DropDownButton](/uwp/api/microsoft.ui.xaml.controls.dropdownbutton)|Représente un bouton avec un chevron destiné à ouvrir un menu.|
|[FontIconSource ](/uwp/api/microsoft.ui.xaml.controls.fonticonsource)|Représente une source d’icône qui utilise un glyphe de la police spécifiée.|
|[MenuBar](/uwp/api/microsoft.ui.xaml.controls.menubar)|Représente un conteneur spécialisé qui présente un ensemble de menus sur une ligne horizontale, généralement en haut d’une fenêtre d’application.|
|[MenuBarItem](/uwp/api/microsoft.ui.xaml.controls.menubaritem)|Représente un menu de niveau supérieur dans un contrôle MenuBar.|
|[NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview)|Représente un conteneur qui active la navigation dans le contenu de l’application. Il possède un en-tête, une vue pour le contenu principal et un volet de menu pour les commandes de navigation.|
|[ParallaxView](/uwp/api/microsoft.ui.xaml.controls.parallaxview)|Représente un conteneur qui relie la position de défilement d’un élément au premier plan, par exemple une liste, à un élément en arrière-plan, comme une image. Tandis que vous faites défiler l’élément au premier plan, celui-ci anime l’élément en arrière-plan pour créer un effet parallaxe.|
|[PersonPicture](/uwp/api/microsoft.ui.xaml.controls.personpicture)|Représente un contrôle qui affiche l’image d’avatar d’une personne, si celle-ci est disponible. Dans le cas contraire, il affiche les initiales de la personne ou un glyphe générique.|
|[RatingControl](/uwp/api/microsoft.ui.xaml.controls.ratingcontrol)|Représente un contrôle qui permet à l’utilisateur d’entrer une évaluation.|
|[RefreshContainer](/uwp/api/microsoft.ui.xaml.controls.refreshcontainer)|Représente un contrôle conteneur qui fournit un RefreshVisualizer et une fonctionnalité Tirer pour actualiser pour le contenu avec défilement.|
|[RefreshVisualizer](/uwp/api/microsoft.ui.xaml.controls.refreshvisualizer)|Représente un contrôle qui fournit des indicateurs d’état animés pour l’actualisation du contenu.|
|[RevealBackgroundBrush](/uwp/api/microsoft.ui.xaml.media.revealbackgroundbrush)|Peint un arrière-plan de contrôle avec un effet de révélation à l’aide du pinceau de composition et d’effets de lumière.|
|[RevealBorderBrush](/uwp/api/microsoft.ui.xaml.media.revealborderbrush)|Peint une bordure de contrôle avec un effet de révélation à l’aide du pinceau de composition et d’effets de lumière.|
|[RevealBrush](/uwp/api/microsoft.ui.xaml.media.revealbrush)|Classe de base pour les pinceaux qui utilisent des effets de composition et un éclairage pour implémenter le traitement de la conception visuelle de la révélation.|
|[SplitButton](/uwp/api/microsoft.ui.xaml.controls.splitbutton)|Représente un bouton à deux composants, qui peuvent être appelés séparément. Un composant se comporte comme un bouton standard, tandis que l’autre appelle un menu volant.|
|[SwipeControl](/uwp/api/microsoft.ui.xaml.controls.swipecontrol)|Représente un conteneur qui fournit l’accès à des commandes contextuelles par le biais d’interactions tactiles.|
|[SymbolIconSource](/uwp/api/microsoft.ui.xaml.controls.symboliconsource)|Représente une source d’icône qui utilise un glyphe de la police Segoe MDL2 Assets en guise de contenu.|
|[TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout)|Représente un menu volant de barre de commandes spécialisé qui contient des commandes de modification de texte.|
|[ToggleSplitButton](/uwp/api/microsoft.ui.xaml.controls.togglesplitbutton)|Représente un bouton à deux composants, qui peuvent être appelés séparément. Un composant se comporte comme un bouton bascule, tandis que l’autre appelle un menu volant.|
|[TreeView](/uwp/api/microsoft.ui.xaml.controls.treeview)|Représente une liste hiérarchique comportant des nœuds que vous pouvez développer et réduire, et qui contiennent des éléments imbriqués.|

## <a name="examples"></a>Exemples

L’exemple d’application de galerie de contrôles XAML contient des démonstrations interactives et des exemples de code pour l’utilisation des contrôles WinUI.

* Installez l’application XAML Controls Gallery à partir du [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt).

* XAML Controls Gallery est également [open source sur GitHub](
https://github.com/Microsoft/Xaml-Controls-Gallery).

## <a name="documentation"></a>Documentation

Des articles de procédures pour les contrôles de la bibliothèque d’interface utilisateur Windows sont inclus dans la [documentation sur les contrôles de la plateforme Windows universelle](/windows/uwp/design/controls-and-patterns/).

Les documents de référence sur les API se trouvent ici : [API de la bibliothèque d’interface utilisateur Windows](/uwp/api/overview/winui/).