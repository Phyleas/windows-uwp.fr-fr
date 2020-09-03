---
title: Notes de publication de WinUI 2.2
description: Notes de publication de WinUI 2.2, nouvelles fonctionnalités et corrections de bogues incluses.
ms.date: 07/15/2020
ms.topic: article
ms.openlocfilehash: 4c200701e0d845d6c9b9f8797899d88cc72d8c1c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154903"
---
# <a name="windows-ui-library-22"></a>Bibliothèque d’IU Windows 2.2

WinUI 2.2 est la dernière version officielle de la bibliothèque d’IU Windows.

Vous pouvez ajouter des packages WinUI à votre application à l’aide du gestionnaire de package NuGet. Pour plus d’informations, consultez [Bien démarrer avec la bibliothèque d’interface utilisateur Windows](../getting-started.md).

WinUI est un projet open source hébergé sur GitHub. Vous pouvez nous faire part de vos rapports de bogues, demandes de fonctionnalités et contributions de code de la Communauté dans le [dépôt de la bibliothèque d’interface utilisateur Windows](https://aka.ms/winui).

## <a name="microsoftuixaml-22-version-history"></a>Historique des versions de Microsoft.UI.Xaml 2.2

### <a name="windows-ui-library-22-official-release"></a>Version officielle de la bibliothèque d’IU Windows 2.2

AOÛT 2019

[Page des versions sur GitHub](https://github.com/microsoft/microsoft-ui-xaml/releases)

[Téléchargement du package NuGet](https://www.nuget.org/packages/Microsoft.UI.Xaml)

### <a name="new-features"></a>Nouvelles fonctionnalités

#### <a name="tabview"></a>TabView

![Exemple](../images/tabview-gif.gif)

#### <a name="description"></a>Description

Le contrôle TabView est une collection d’onglets qui représentent chacun une nouvelle page ou un nouveau document dans votre application. TabView est utile quand votre application contient plusieurs pages de contenu et que l’utilisateur s’attend à pouvoir ajouter, fermer et réorganiser les onglets. Le nouveau [Terminal Windows](https://github.com/Microsoft/Terminal) utilise TabView pour afficher plusieurs interfaces de ligne de commande.

#### <a name="documentation"></a>Documentation

https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.tabview?view=winui-2.2

#### <a name="navigationview-updates"></a>Mises à jour de NavigationView

##### <a name="a-navigationviews-back-button-update"></a>a) Mise à jour du bouton Précédent de NavigationView

![Exemple](../images/navigationview-back-button.gif)

##### <a name="description"></a>Description

En mode minimal de NavigationView, le bouton Précédent ne disparaît plus. Lors de l’ouverture et de la fermeture du volet, les utilisateurs n’ont plus besoin de déplacer leur curseur pour cliquer sur le bouton hamburger. Cette fonctionnalité est opérationnelle par défaut. Vous n’avez pas besoin d’apporter des modifications au code.

##### <a name="b-navigationview---no-auto-padding"></a>b) NavigationView - Aucun remplissage automatique

![Exemple](../images/navigationview-no-auto-padding.png)

##### <a name="description"></a>Description

Les développeurs d’applications peuvent désormais récupérer tous les pixels dans leur fenêtre d’application quand ils utilisent le contrôle NavigationView et s’étendent dans la zone de barre de titre.

##### <a name="documentation"></a>Documentation

https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview#top-whitespace

#### <a name="visual-style-updates"></a>Mises à jour des styles visuels

##### <a name="a-corner-radius-update"></a>a) Mise à jour du rayon d’angle

![Exemple](../images/corner-radius.png)

##### <a name="description"></a>Description

L’attribut CornerRadius a été ajouté. Les contrôles par défaut ont été mis à jour pour utiliser des angles légèrement arrondis. Les développeurs peuvent facilement personnaliser le rayon d’angle pour donner à votre application un aspect unique si vous le souhaitez.

##### <a name="github-spec-link"></a>Lien vers des spécifications sur GitHub

https://github.com/microsoft/microsoft-ui-xaml/issues/524

##### <a name="b-border-thickness-update"></a>b) Mise à jour de l’épaisseur de la bordure

![Exemple](../images/border-thickness.png)

##### <a name="description"></a>Description

La personnalisation de la propriété BorderThickness a été simplifiée. Les contrôles par défaut ont été mis à jour pour réduire les contours afin d’obtenir un aspect plus clair et familier.

##### <a name="github-spec-link"></a>Lien vers des spécifications sur GitHub

https://github.com/microsoft/microsoft-ui-xaml/issues/835

##### <a name="c-button-visual-update"></a>c) Mise à jour du visuel de Button

![Exemple](../images/button-hover-visual-update.png)

##### <a name="description"></a>Description : 
Le visuel de Button par défaut a été mis à jour pour supprimer le contour qui apparaissait pendant le survol afin de lui donner un aspect plus clair.

##### <a name="github-spec-link"></a>Lien vers des spécifications sur GitHub :  
https://github.com/microsoft/microsoft-ui-xaml/issues/953

##### <a name="d-splitbutton-visual-update"></a>c) Mise à jour du visuel de SplitButton

![Exemple](../images/splitbutton-visual-update.png)

##### <a name="description"></a>Description : 
Le visuel de SplitButton par défaut a été mis à jour pour le différencier davantage de DropDownButton.

##### <a name="github-spec-link"></a>Lien vers des spécifications sur GitHub : 
https://github.com/microsoft/microsoft-ui-xaml/issues/986

##### <a name="e-toggleswitch-visual-update"></a>e) Mise à jour du visuel de ToggleSwitch

![Exemple](../images/toggleswitch-update.png)

##### <a name="description"></a>Description : 
La largeur par défaut de ToggleSwitch a été réduite de 44 px à 40 px, afin qu’elle soit visuellement équilibrée tout en conservant sa facilité d’utilisation.

##### <a name="github-spec-link"></a>Lien vers des spécifications sur GitHub : 
https://github.com/microsoft/microsoft-ui-xaml/issues/836

##### <a name="f-checkbox-and-radiobutton-visual-update"></a>f) Mise à jour des visuels de CheckBox et RadioButton

![Exemple](../images/checkbox-radiobutton.png)

##### <a name="description"></a>Description : 
Les visuels de CheckBox et RadioButton ont été mis à jour pour être cohérents avec le reste du changement apporté aux styles visuels.

##### <a name="github-spec-link"></a>Lien vers des spécifications sur GitHub : 
https://github.com/microsoft/microsoft-ui-xaml/issues/839

## <a name="examples"></a>Exemples

L’exemple d’application de galerie de contrôles XAML contient des démonstrations interactives et des exemples de code pour l’utilisation des contrôles WinUI.

* Installez l’application XAML Controls Gallery à partir du [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt).

* XAML Controls Gallery est également [open source sur GitHub](
https://github.com/Microsoft/Xaml-Controls-Gallery).

## <a name="documentation"></a>Documentation

Des articles de procédures pour les contrôles de la bibliothèque d’interface utilisateur Windows sont inclus dans la [documentation sur les contrôles de la plateforme Windows universelle](/windows/uwp/design/controls-and-patterns/).

Les documents de référence sur les API se trouvent ici : [API de la bibliothèque d’interface utilisateur Windows](/uwp/api/overview/winui/).

## <a name="microsoftuixaml-22-version-history"></a>Historique des versions de Microsoft.UI.Xaml 2.2

### <a name="microsoftuixaml-22190702001-prerelease"></a>Microsoft.UI.Xaml 2.2.190702001-prerelease

Juillet 2019

[Page des versions sur GitHub](https://github.com/microsoft/microsoft-ui-xaml/releases/tag/v2.2.190702001-prerelease)

[Téléchargement du package NuGet](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.2.190702001-prerelease)

### <a name="experimental-feature"></a>Fonctionnalité expérimentale

* [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview?view=winui-2.2)

### <a name="microsoftuixaml-2220190416001-prerelease"></a>Microsoft.UI.Xaml 2.2.20190416001-prerelease

Avril 2019

[Page des versions sur GitHub](https://github.com/Microsoft/microsoft-ui-xaml/releases/tag/v2.2.190416008-prerelease)

[Téléchargement du package NuGet](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.2.190416008-prerelease)

#### <a name="experimental-features"></a>Fonctionnalités expérimentales

* [FlowLayout](/uwp/api/microsoft.ui.xaml.controls.flowlayout)

* [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel)

* [RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons)

* [ScrollViewer](/uwp/api/microsoft.ui.xaml.controls.scrollviewer)