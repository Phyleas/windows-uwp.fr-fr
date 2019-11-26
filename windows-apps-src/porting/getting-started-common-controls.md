---
ms.assetid: E2B73380-D673-48C6-9026-96976D745017
description: 'Prise en main : Contrôles courants'
title: 'Prise en main : Contrôles courants'
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bc45f21acf5b9a485cf6bd5ead18482f1920bf99
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260142"
---
# <a name="getting-started-common-controls"></a>Prise en main : Contrôles courants


## <a name="common-controls-list"></a>Liste des contrôles courants

Dans la section qui précède, vous utilisiez seulement deux contrôles : les boutons et les blocs de texte. Il existe bien sûr beaucoup plus de contrôles à votre disposition. Voici quelques contrôles communs que vous utiliserez dans vos applications et leurs équivalents dans iOS. Les contrôles iOS apparaissent dans l’ordre alphabétique, en regard des contrôles les plus similaires pour la plateforme Windows universelle (UWP).

L’intelligence des contrôles UWP est qu’ils sont capables de détecter le type d’appareil sur lequel ils s’exécutent, et de modifier leur apparence et leurs fonctionnalités en conséquence. Par exemple, si votre projet utilise le contrôle [**DatePicker**](https://docs.microsoft.com/previous-versions/windows/apps/br211681(v=win.10)), il est capable de s’optimiser lui-même pour adapter son apparence et son comportement à l’appareil sur lequel il opère, qu’il s’agisse d’un ordinateur de bureau ou d’un téléphone. Vous n’avez rien à faire : les contrôles s’ajustent automatiquement au moment de l’exécution.

| Contrôle iOS (classe/protocole) | Contrôle UWP équivalent |
|------------------------------|--------------------------------------|
| Indicateur d’activité (**UIActivityIndicatorView**) | [**ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) <br/> Voir aussi [Démarrage rapide : ajout de contrôles de progression](https://docs.microsoft.com/previous-versions/windows/apps/hh780651(v=win.10)) |
| Vue bannière publicitaire (**ADBannerView**) et vue déléguée bannière publicitaire (**ADBannerViewDelegate**) | [Classe AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) <br/> Voir aussi [Afficher des publicités dans votre application](../monetize/display-ads-in-your-app.md) |
| Bouton (UIButton) | [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) <br/> Voir aussi [Démarrage rapide : ajout de contrôles de bouton](https://docs.microsoft.com/previous-versions/windows/apps/jj153346(v=win.10)) |
| Sélecteur de dates (UIDatePicker) | [DatePicker](https://docs.microsoft.com/previous-versions/windows/apps/br211681(v=win.10)) |
| Vue image (UIImageView) | [Image](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) <br/> Voir également [Image et ImageBrush](https://docs.microsoft.com/windows/uwp/controls-and-patterns/images-imagebrushes) |
| Libellé (UILabel) | [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) <br/> Voir aussi [Démarrage rapide : affichage de texte](https://docs.microsoft.com/previous-versions/windows/apps/hh700392(v=win.10)) |
| Vue carte (MKMapView) et vue déléguée carte (MKMapViewDelegate) | Consultez [Bing Maps pour les applications UWP](https://msdn.microsoft.com/library/hh846481) |
| Contrôleur de navigation (UINavigationController) et contrôleur de navigation délégué (UINavigationControllerDelegate) | [Frame](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) <br/> Voir aussi [Navigation](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) |
| Contrôle de page (UIPageControl) | [Page](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) <br/> Voir aussi [Navigation](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) |
| Vue sélecteur (UIPickerView) et vue déléguée sélecteur (UIPickerViewDelegate) | [ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) <br/> Voir aussi [Ajout de zones de liste déroulante et de zones de liste](https://docs.microsoft.com/previous-versions/windows/apps/hh780616(v=win.10)) |
| Barre de progression (UIProgressView) | [ProgressBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressBar) <br/> Voir aussi [Démarrage rapide : ajout de contrôles de progression](https://docs.microsoft.com/previous-versions/windows/apps/hh780651(v=win.10)) |
| Vue de défilement (UIScrollView) et vue déléguée de défilement (UIScrollViewDelegate) | [ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) <br/>  Voir aussi [Exemple de zoom, panoramique et défilement XAML (Extensible Application Markup Language)](https://code.msdn.microsoft.com/windowsapps/xaml-scrollviewer-pan-and-949d29e9) |
| Barre de recherche (UISearchBar) et barre de recherche déléguée (UISearchBarDelegate) | Voir [Ajout d’une fonctionnalité de recherche à une application](https://docs.microsoft.com/previous-versions/windows/apps/jj130767(v=win.10)) <br/>  Voir aussi [Démarrage rapide : ajout d’une fonctionnalité de recherche à une application](https://docs.microsoft.com/previous-versions/windows/apps/hh868180(v=win.10)) |
| Contrôle segmenté (UISegmentedControl) | Aucun |
| Curseur (UISlider) | [Curseur](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider) <br/>  Voir aussi [Comment ajouter un curseur](https://docs.microsoft.com/previous-versions/windows/apps/hh868197(v=win.10)) |
| Contrôleur de vue fractionnée (UISplitViewController) et contrôleur de vue fractionnée délégué (UISplitViewControllerDelegate) | Aucun |
| Switch (UISwitch) | [ToggleSwitch](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToggleSwitch) <br/>  Voir aussi [Comment ajouter un bouton bascule](https://docs.microsoft.com/previous-versions/windows/apps/hh868198(v=win.10)) |
| Contrôleur de barre d’onglets (UITabBarController) et contrôleur de barre d’onglets délégué (UITabBarControllerDelegate) | Aucun |
| Contrôleur de vue de table (UITableViewController), vue de table (UITableView), vue de table déléguée (UITableViewDelegate) et cellule de table (UITableViewCell) | [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) <br/>  Voir aussi [Démarrage rapide : ajout des contrôles ListView et GridView](https://docs.microsoft.com/previous-versions/windows/apps/hh780650(v=win.10)) |
| Champ de texte (UITextField) et champ de texte délégué (UITextFieldDelegate) | [TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) <br/>  Voir aussi [Afficher et modifier du texte](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/text-controls) |
| Vue de texte (UITextView) et vue de texte déléguée (UITextViewDelegate) | [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) <br/>  Voir aussi [Démarrage rapide : affichage de texte](https://docs.microsoft.com/previous-versions/windows/apps/hh700392(v=win.10)) |
| Vue (UIView) et contrôleur de vue (UIViewController) | [Page](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) <br/>  Voir aussi [Navigation](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) |
| Vue Web (UIWebView) et vue Web déléguée (UIWebViewDelegate) | [WebView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) <br/>  Voir aussi [Exemple de contrôle d’affichage Web XAML](https://code.msdn.microsoft.com/windowsapps/XAML-WebView-control-sample-58ad63f7) |
| Fenêtre (UIWindow) | [Frame](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) <br/>  Voir aussi [Navigation](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) |

Pour obtenir d’autres contrôles, voir [Liste des contrôles](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/).

**Remarque**  pour obtenir la liste des contrôles des applications UWP utilisant JavaScript et HTML, consultez [liste des contrôles](https://docs.microsoft.com/previous-versions/windows/apps/hh465453(v=win.10)).

### <a name="next-step"></a>Étape suivante

[Prise en main : navigation](getting-started-navigation.md)

## <a name="related-topics"></a>Rubriques connexes

* [Build 2014 : qu’en est-il des contrôles et de l’interface utilisateur XAML ?](https://channel9.msdn.com/Events/Build/2014/2-516)
* [Build 2014 : développement d’applications à l’aide de l’infrastructure d’interface utilisateur XAML commune](https://channel9.msdn.com/Events/Build/2014/2-507)
* [Build 2014 : utilisation de Visual Studio pour générer des applications convergées en XAML](https://channel9.msdn.com/Events/Build/2014/3-591)
