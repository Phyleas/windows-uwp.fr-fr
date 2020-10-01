---
description: Aidez les utilisateurs à accroître leur productivité en leur permettant de voir les différentes parties de votre application dans des fenêtres distinctes.
title: Afficher plusieurs vues d’une application
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2b25ceaac900868479c9145b9dad5977ca213f03
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91218482"
---
# <a name="show-multiple-views-for-an-app"></a>Afficher plusieurs vues d’une application

![Mode filaire illustrant une application avec plusieurs fenêtres](images/multi-view.gif)

Aidez les utilisateurs à accroître leur productivité en leur permettant d’afficher des parties indépendantes de votre application dans des fenêtres distinctes. Lorsque vous créez plusieurs fenêtres pour une application, la barre des tâches répertorie chaque fenêtre séparément. Les utilisateurs peuvent déplacer, redimensionner, afficher et masquer des fenêtres d’application indépendamment et ils peuvent basculer d’une fenêtre à une autre comme s’il s’agissait d’applications distinctes.

> **API importantes** : [espace de noms Windows.UI.ViewManagement](/uwp/api/windows.ui.viewmanagement), [espace de noms Windows.UI.WindowManagement](/uwp/api/windows.ui.windowmanagement)

## <a name="when-should-an-app-use-multiple-views"></a>Quand une application doit-elle utiliser plusieurs vues ?

Il existe de nombreux scénarios qui peuvent bénéficier de plusieurs vues. Voici quelques exemples :

- Une application de messagerie qui permet aux utilisateurs d’afficher une liste des messages reçus tout en rédigeant un e-mail
- Une application de carnet d’adresses qui permet aux utilisateurs de comparer les informations de contact de plusieurs personnes côte à côte
- Une application de lecteur de musique qui permet aux utilisateurs de voir ce qui est en cours de lecture tout en naviguant dans une liste d’autres morceaux disponibles
- Une application de prise de notes qui permet aux utilisateurs de copier des informations à partir d’une page de notes sur une autre
- Une application de lecture qui permet aux utilisateurs d’ouvrir plusieurs articles à lire plus tard, après avoir pu accéder à tous les gros titres de l'actualité

Chaque disposition d'application est unique, mais nous vous recommandons d'inclure un bouton « nouvelle fenêtre » dans un emplacement prévisible, comme le coin supérieur droit du contenu, qui pourra être ouvert dans une nouvelle fenêtre. Envisagez également d’inclure une option de [menu contextuel](../controls-and-patterns/menus.md) pour « Ouvrir dans une nouvelle fenêtre ».

Pour créer des instances distinctes de votre application (plutôt que des fenêtres distinctes pour la même instance), consultez [Créer une application Windows multi-instance](../../launch-resume/multi-instance-uwp.md).

## <a name="windowing-hosts"></a>Hôtes de fenêtrage

Il existe plusieurs manières d’héberger du contenu Windows dans une application.

- [CoreWindow](/uwp/api/windows.ui.core.corewindow)/[ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview)

     Une vue d’application est l’association de type 1:1 d’un thread et d’une fenêtre que l’application utilise pour afficher le contenu. La première vue créée lors du démarrage de votre application s’appelle la *vue principale*. Chaque objet CoreWindow/ApplicationView fonctionne dans son propre thread. L'utilisation de différents threads d’interface utilisateur peut compliquer les applications à plusieurs fenêtres.

    La vue principale de votre application est toujours hébergée dans un objet ApplicationView. Le contenu d’une fenêtre secondaire peut être hébergé dans un objet ApplicationView ou AppWindow.

    Pour savoir comment utiliser ApplicationView afin d'afficher des fenêtres secondaires dans votre application, consultez [Utiliser ApplicationView](application-view.md).
- [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)

    AppWindow facilite la création d’applications Windows à plusieurs fenêtres, car il fonctionne sur le même thread d’interface utilisateur que celui sur lequel il a été créé.

    La classe AppWindow et les autres API de l’espace de noms [WindowManagement](/uwp/api/windows.ui.windowmanagement) sont disponibles à partir de Windows 10, version 1903 (SDK 18362). Si votre application cible des versions antérieures de Windows 10, vous devez utiliser ApplicationView pour créer des fenêtres secondaires.

    Pour savoir comment utiliser AppWindow afin d'afficher des fenêtres secondaires dans votre application, consultez [Utiliser AppWindow](app-window.md).

    > [!NOTE]
    > AppWindow est actuellement disponible en préversion. Dès lors, vous pouvez soumettre des applications utilisant AppWindow sur le Microsoft Store, mais certains composants de plateforme et d'infrastructure peuvent ne pas fonctionner avec AppWindow (voir [Limitations](/uwp/api/windows.ui.windowmanagement.appwindow#limitations)).
- [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) (XAML Islands)

     Le contenu XAML UWP d'une application Win32 (utilisant HWND), également appelé XAML Islands, est hébergé dans un objet DesktopWindowXamlSource.

    Pour plus d’informations sur XAML Islands, consultez [Utiliser l'API d’hébergement XAML UWP dans une application de bureau](/windows/apps/desktop/modernize/using-the-xaml-hosting-api).

### <a name="make-code-portable-across-windowing-hosts"></a>Portabilité du code entre les hôtes de fenêtrage

Lorsque le contenu XAML s'affiche dans un objet [CoreWindow](/uwp/api/windows.ui.core.corewindow), il est toujours associé à un objet [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview) et XAML [Window](/uwp/api/windows.ui.xaml.window). Vous pouvez utiliser les API de ces classes pour récupérer des informations telles que les limites de fenêtre. Pour récupérer une instance de ces classes, utilisez la méthode statique [CoreWindow.GetForCurrentThread](/uwp/api/windows.ui.core.corewindow.getforcurrentthread), la [méthode ApplicationView.GetForCurrentView](/uwp/api/windows.ui.viewmanagement.applicationview.getforcurrentview) ou la propriété [Window.Current](/uwp/api/windows.ui.xaml.window.current). En outre, de nombreuses classes utilisent le modèle `GetForCurrentView` pour récupérer une instance de classe, par exemple [DisplayInformation.GetForCurrentView](/uwp/api/windows.graphics.display.displayinformation.getforcurrentview).

Ces API fonctionnent en présence d'une même arborescence de contenu XAML pour un objet CoreWindow/ApplicationView, afin d'informer XAML que le contexte dans lequel il est hébergé est celui de CoreWindow/ApplicationView.

Lorsque le contenu XAML s’exécute à l’intérieur d’un objet AppWindow ou DesktopWindowXamlSource, plusieurs arborescences de contenu XAML peuvent s’exécuter simultanément sur le même thread. Dans ce cas, ces API ne fournissent pas les informations qui conviennent, car le contenu n’est plus exécuté dans l'objet CoreWindow/ApplicationView (et XAML Window).

Pour vous assurer du bon fonctionnement de votre code dans tous les hôtes de fenêtrage, vous devez remplacer les API reposant sur les objets [CoreWindow](/uwp/api/windows.ui.core.corewindow), [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview), et [Window](/uwp/api/windows.ui.xaml.window) par les nouvelles API obtenant leur contexte à partir de la classe [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot).
La classe XamlRoot représente une arborescence de contenu XAML et les informations de contexte dans lequel elle est hébergée, qu’il s’agisse d’un objet CoreWindow, AppWindow ou DesktopWindowXamlSource. Cette couche d’abstraction vous permet d’écrire le même code, quel que soit l’hôte de fenêtrage dans lequel le XAML s’exécute.

Ce tableau montre du code qui ne fonctionne pas correctement dans les hôtes de fenêtrage et le code portable de remplacement, ainsi que plusieurs API que vous n'êtes pas tenu de modifier.

| Si vous avez utilisé... | Remplacez par... |
| - | - |
| CoreWindow.GetForCurrentThread().[Bounds](/uwp/api/windows.ui.core.corewindow.bounds) | _uiElement_.XamlRoot.[Size](/uwp/api/windows.ui.xaml.xamlroot.size) |
| CoreWindow.GetForCurrentThread().[SizeChanged](/uwp/api/windows.ui.core.corewindow.sizechanged) | _uiElement_.XamlRoot.[Changed](/uwp/api/windows.ui.xaml.xamlroot.changed) |
| CoreWindow.[Visible](/uwp/api/windows.ui.core.corewindow.visible) | _uiElement_.XamlRoot.[IsHostVisible](/uwp/api/windows.ui.xaml.xamlroot.ishostvisible) |
| CoreWindow.[VisibilityChanged](/uwp/api/windows.ui.core.corewindow.visibilitychanged) | _uiElement_.XamlRoot.[Changed](/uwp/api/windows.ui.xaml.xamlroot.changed) |
| CoreWindow.GetForCurrentThread().[GetKeyState](/uwp/api/windows.ui.core.corewindow.getkeystate) | Inchangé. Pris en charge dans AppWindow et DesktopWindowXamlSource. |
| CoreWindow.GetForCurrentThread().[GetAsyncKeyState](/uwp/api/windows.ui.core.corewindow.getasynckeystate) | Inchangé. Pris en charge dans AppWindow et DesktopWindowXamlSource. |
| Window.[Current](/uwp/api/windows.ui.xaml.window.current) | Retourne l’objet XAML Window principal qui est étroitement lié à l'objet CoreWindow actuel. Consultez la remarque en fin de tableau. |
| Window.Current.[Bounds](/uwp/api/windows.ui.xaml.window.bounds) | _uiElement_.XamlRoot.[Size](/uwp/api/windows.ui.xaml.xamlroot.size) |
| Window.Current.[Content](/uwp/api/windows.ui.xaml.window.content) | UIElement root =  _uiElement_.XamlRoot.[Content](/uwp/api/windows.ui.xaml.xamlroot.content) |
| Window.Current.[Compositor](/uwp/api/windows.ui.xaml.window.compositor) | Inchangé. Pris en charge dans AppWindow et DesktopWindowXamlSource. |
| VisualTreeHelper.[FindElementsInHostCoordinates](/uwp/api/windows.ui.xaml.media.visualtreehelper.findelementsinhostcoordinates)<br>Même si le paramètre UIElement est facultatif, la méthode lève une exception si aucun UIElement n’est fourni quand il est hébergé sur un îlot. | Spécifiez _uiElement_.XamlRoot comme un UIElement au lieu de le laisser vide. |
| VisualTreeHelper.[GetOpenPopups](/uwp/api/windows.ui.xaml.media.visualtreehelper.getopenpopups)<br/>Dans les applications XAML Islands, cela génère une erreur. Dans les applications AppWindow, cela renvoie les fenêtres contextuelles ouvertes dans la fenêtre principale. | VisualTreeHelper.[GetOpenPopupsForXamlRoot](/uwp/api/windows.ui.xaml.media.visualtreehelper.getopenpopupsforxamlroot)(_uiElement_.XamlRoot) |
| FocusManager.[GetFocusedElement](/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement) | FocusManager.[GetFocusedElement](/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement#Windows_UI_Xaml_Input_FocusManager_GetFocusedElement_Windows_UI_Xaml_XamlRoot_)(_uiElement_.XamlRoot) |
| contentDialog.ShowAsync() | contentDialog.[XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot) = _uiElement_.XamlRoot;<br/>contentDialog.ShowAsync(); |
| menuFlyout.ShowAt(null, new Point(10, 10)); | menuFlyout.[XamlRoot](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.xamlroot) = _uiElement_.XamlRoot;<br/>menuFlyout.ShowAt(null, new Point(10, 10)); |

> [!NOTE]
> Le contenu XAML d'un objet DesktopWindowXamlSource présente un objet CoreWindow/Window sur le thread, mais il est toujours invisible et a une taille de 1x1. L'application peut y accéder, mais il ne renvoie pas de limites ou de visibilité significatives.
>
>Le contenu XAML d'un objet AppWindow présente toujours un objet CoreWindow sur le même thread. Si vous appelez une API `GetForCurrentView` ou `GetForCurrentThread`, cette API renvoie un objet qui reflète l’état de CoreWindow sur le thread, mais pas les objets AppWindows susceptibles de s’exécuter sur ce thread.


## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées

- Fournissez un point d’entrée clair à la vue secondaire en utilisant le glyphe « ouvrir une nouvelle fenêtre ».
- Communiquez l’objectif de la vue secondaire aux utilisateurs.
- Assurez-vous que votre application est totalement fonctionnelle dans une vue unique et que les utilisateurs ouvriront une vue secondaire uniquement pour des raisons pratiques.
- N’utilisez pas la vue secondaire pour fournir des notifications ou d'autres éléments visuels temporaires.

## <a name="related-topics"></a>Rubriques connexes

- [Utiliser AppWindow](app-window.md)
- [Utiliser ApplicationView](application-view.md)
- [ApplicationViewSwitcher](/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitcher)
- [CreateNewView](/uwp/api/windows.applicationmodel.core.coreapplication.createnewview)
