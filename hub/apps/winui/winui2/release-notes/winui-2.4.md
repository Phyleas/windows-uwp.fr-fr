---
title: Notes de publication de WinUI 2.4
description: Notes de publication de WinUI 2.4, nouvelles fonctionnalités et corrections de bogues incluses.
ms.date: 07/15/2020
ms.topic: reference
ms.openlocfilehash: 22fd028ba2059a092ee2f2be47a114fb2d618ce1
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86492824"
---
# <a name="windows-ui-library-24"></a>Bibliothèque d’IU Windows 2.4

WinUI 2.4 est la dernière version officielle de la bibliothèque d’IU Windows (WinUI).

WinUI est un projet open source hébergé sur GitHub dans le [dépôt de la bibliothèque d’IU Windows](https://aka.ms/winui). Inscrivez tous les rapports de bogues, demandes de fonctionnalités et contributions de code de la Communauté dans ce dépôt.

Versions de WinUI : [Page des versions sur GitHub](https://github.com/microsoft/microsoft-ui-xaml/releases)

Les packages WinUI peuvent être ajoutés aux projets Visual Studio par le biais du gestionnaire de package NuGet. Pour plus d’informations, consultez [Bien démarrer avec la bibliothèque d’interface utilisateur Windows](../getting-started.md).

Téléchargement du package NuGet : [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)

## <a name="new-features"></a>Nouvelles fonctionnalités

### <a name="radialgradientbrush"></a>RadialGradientBrush

Un RadialGradientBrush est dessiné dans une ellipse qui est définie par les propriétés Center, RadiusX et RadiusY. Les couleurs du début du dégradé se trouvent au centre de l’ellipse et se terminent au niveau du rayon.

![Pinceau dégradé radial](../images/radialgradientbrush.gif)<br>
*Pinceau dégradé radial*

[Indications d’utilisation](/windows/uwp/design/style/brushes#radial-gradient-brushes)

[Informations de référence sur les API](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush)

### <a name="progressring"></a>ProgressRing

Le contrôle ProgressRing est utilisé pour les interactions modales, où l’utilisateur est bloqué jusqu’à la disparition de l’anneau de progression. Utilisez ce contrôle si une opération nécessite que la plupart des interactions avec l’application soit suspendue jusqu’à ce que l’opération soit terminée.

![Contrôle ProgressRing](../images/progressring.gif)<br>
*Contrôle ProgressRing*

[Indications d’utilisation](/windows/uwp/design/controls-and-patterns/progress-controls)

[Informations de référence sur les API](/uwp/api/microsoft.ui.xaml.controls.progressring)

### <a name="tabview-updates"></a>Mises à jour de TabView

Les mises à jour du contrôle TabView vous permettent de contrôler davantage le rendu des onglets.

Vous pouvez définir la largeur des onglets non sélectionnés et montrer juste une icône pour économiser de l’espace à l’écran :

![Tailles des onglets avec le contrôle TabView](..\images\tabview-sizing.gif)<br>
*Tailles des onglets avec le contrôle TabView*

Vous pouvez aussi masquer le bouton Fermer sur les onglets non sélectionnés tant que l’utilisateur ne pointe pas dessus (dans les versions précédentes, il restait affiché) :

![Pointer pour fermer avec le contrôle TabView](..\images\tabview-closebuttononhover.gif)<br>
*Pointer pour fermer avec le contrôle TabView*

[Indications d’utilisation](/windows/uwp/design/controls-and-patterns/tab-view)

[Informations de référence sur les API](/uwp/api/microsoft.ui.xaml.controls.tabview)

### <a name="dark-theme-updates-to-textbox-family-of-controls"></a>Mises à jour du thème sombre pour la famille de contrôles TextBox

Quand le thème sombre est activé, la couleur d’arrière-plan des contrôles de la famille TextBox reste sombre par défaut lors de l’insertion de texte (dans les versions précédentes, la couleur d’arrière-plan devenait blanche pendant l’insertion de texte).

| Avant | Après |
| - | - |
| ![Mises à jour du thème sombre TextBox (avant)](..\images\textbox-darkthemeupdates-before1.gif)<br>*Mises à jour du thème sombre TextBox (avant)* | ![Mises à jour du thème sombre TextBox (après)](..\images\textbox-darkthemeupdates-after1.gif)<br>*Mises à jour du thème sombre TextBox (après)* |
| ![Mises à jour du thème sombre TextBox (avant)](..\images\textbox-darkthemeupdates-before2.gif)<br>*Mises à jour du thème sombre TextBox (avant)* | ![Mises à jour du thème sombre TextBox (après)](..\images\textbox-darkthemeupdates-after2.gif)<br>*Mises à jour du thème sombre TextBox (après)* |

Voici quelques-uns des contrôles inclus dans la famille de contrôles TextBox :

- [TextBox](/uwp/api/windows.ui.xaml.controls.textbox)
- [RichEditBox](/uwp/api/windows.ui.xaml.controls.richtextblock)
- [PasswordBox](/uwp/api/windows.ui.xaml.controls.passwordbox)
- [Editable ComboBox](/uwp/api/windows.ui.xaml.controls.combobox)
- [AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.autosuggestbox)

### <a name="hierarchical-navigation"></a>Navigation hiérarchique

Le contrôle [NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview?view=winui-2.4) prend maintenant en charge la navigation hiérarchique et comprend les modes d’affichage Left, Top et LeftCompact. Un NavigationView hiérarchique est utile pour afficher des catégories de pages, identifier des pages avec des pages enfants associées ou servir dans des applications qui ont des pages de style hub liées à de nombreuses autres pages.

![Contrôle NavigationView hiérarchique](..\images\HierarchicalNavView.gif)<br>*Contrôle NavigationView hiérarchique*

[Indications d’utilisation](/windows/uwp/design/controls-and-patterns/navigationview#hierarchical-navigation)

[Informations de référence sur les API](/uwp/api/microsoft.ui.xaml.controls.navigationview)

## <a name="samples"></a>exemples

Des exemples de chacune des fonctionnalités WinUI 2.4 décrites sont disponibles dans **XAML Controls Gallery**.

Si vous n’avez pas installé l’application XAML Controls Gallery, téléchargez-la à partir du [Microsoft Store](https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt).

Vous pouvez également consulter le code source de XAML Controls Gallery sur [GitHub](https://github.com/Microsoft/Xaml-Controls-Gallery).
