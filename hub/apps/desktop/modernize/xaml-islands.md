---
description: Ce guide vous aide à créer des interfaces utilisateur UWP Fluent directement dans vos applications WPF et Windows Forms
title: Héberger des contrôles XAML WinRT dans les applications de bureau
ms.date: 10/20/2020
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, xaml islands, îles xaml
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: b8cb2670c09c5a93d2edd67e3f338a33eeb488ad
ms.sourcegitcommit: b4c782b2403da83a6e0b5b7416cc4dc835b068d9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98922755"
---
# <a name="host-winrt-xaml-controls-in-desktop-apps-xaml-islands"></a>Héberger des contrôles XAML WinRT dans des applications de bureau (XAML Islands)

À compter de Windows 10 version 1903, vous pouvez héberger des contrôles XAML WinRT dans des applications de bureau non conçues pour UWP à l’aide d’une fonctionnalité appelée *XAML Islands*. Vous pouvez ainsi améliorer l’apparence, le comportement et les fonctionnalités de vos applications WPF, Windows Forms et Win32 C++ existantes, mais aussi bénéficier des dernières nouveautés de l’interface utilisateur Windows 10 qui sont uniquement disponibles par le biais de contrôles XAML WinRT. Cela signifie que vous pouvez utiliser des fonctionnalités UWP telles que [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions) et des contrôles prenant en charge le [système Fluent Design](/windows/uwp/design/fluent-design-system/index) dans vos applications WPF, Windows Forms et Win32 C++ existantes.

Vous pouvez héberger n’importe quel contrôle XAML WinRT qui dérive de [Windows.UI.Xaml.UIElement](/uwp/api/windows.ui.xaml.uielement), notamment :

* Tout contrôle XAML WinRT de première partie fourni par le SDK Windows ou la bibliothèque WinUI 2.x.
* Tout contrôle XAML WinRT personnalisé (par exemple, un contrôle utilisateur qui se compose de plusieurs contrôles XAML WinRT fonctionnant ensemble). Vous devez disposer du code source du contrôle personnalisé pour pouvoir le compiler avec votre application.

À la base, les îles XAML sont créées à l’aide de l’*API d’hébergement XAML UWP*. Cette API comprend plusieurs classes Windows Runtime et interfaces COM qui ont été introduites dans le SDK Windows 10 version 1903. Nous fournissons également dans le [Windows Community Toolkit](/windows/uwpcommunitytoolkit/) un ensemble de contrôles .NET XAML Island qui utilisent l’API d’hébergement XAML UWP en interne et qui offrent une expérience de développement plus pratique pour les applications WPF et Windows Forms.

La façon dont vous utilisez XAML Islands dépend du type de votre application et des types de contrôles XAML WinRT que vous souhaitez héberger.

> [!NOTE]
> Si vous avez des choses à nous dire sur XAML Islands, ouvrez un nouveau problème dans le [dépôt Microsoft.Toolkit.Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) et laissez-y vos commentaires. Si vous préférez que vos commentaires restent privés, envoyez-les à XamlIslandsFeedback@microsoft.com. Vos insights et vos scénarios sont très importants pour nous.

## <a name="requirements"></a>Conditions requises

Les conditions requises du runtime pour XAML Islands sont les suivantes :

* Windows 10, version 1903 ou ultérieure.
* Si votre application n’est pas empaquetée dans un [package MSIX](/windows/msix) pour la déployer, [Visual C++ Runtime](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) doit être installé sur l’ordinateur.

## <a name="wpf-and-windows-forms-applications"></a>Applications WPF et Windows Forms

> [!NOTE]
> L’utilisation de XAML Islands pour héberger des contrôles XAML WinRT dans des applications WPF et Windows Forms est pris en charge seulement dans les applications ciblant .NET Core 3.x. XAML Islands n’est pas encore pris en charge dans les applications ciblant .NET 5 ou n’importe quelle version du .NET Framework.

Nous recommandons que les applications WPF et Windows Forms utilisent les contrôles .NET XAML Island disponibles dans le Windows Community Toolkit. Ces contrôles fournissent un modèle objet qui imite les propriétés, méthodes et événements des contrôles XAML WinRT correspondants ou qui vous donne accès à ces éléments. Ils gèrent également des comportements tels que la navigation au clavier et les changements de disposition.

Il existe deux ensembles de contrôles XAML Island pour les applications WPF et Windows Forms : les *contrôles wrappés* et les *contrôles hôtes*. 

### <a name="wrapped-controls"></a>Contrôles wrappés

Les applications WPF et Windows Forms peuvent utiliser une sélection de contrôles XAML Islands qui wrappent l’interface et les fonctionnalités d’un contrôle XAML WinRT spécifique. Vous pouvez ajouter ces contrôles directement à l’aire de conception de votre projet WPF ou Windows Forms, puis les utiliser comme tout autre contrôle WPF ou Windows Forms dans le concepteur.

Les contrôles XAML WinRT wrappés suivants sont actuellement disponibles dans le Windows Community Toolkit. 

| Contrôler | Système d’exploitation minimal pris en charge | Description |
|-----------------|-------------------------------|-------------|
| [InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10 version 1903 | Fournissent une surface et les barres d’outils associées pour l’interaction utilisateur basée sur Windows Ink dans votre application de bureau Windows Forms ou WPF. |
| [MediaPlayerElement](/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10 version 1903 | Incorpore une vue qui diffuse en streaming du contenu multimédia tel que des vidéos dans votre application de bureau Windows Forms ou WPF. |
| [MapControl](/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10 version 1903 | Permet d’afficher une carte symbolique ou photoréaliste dans votre application de bureau Windows Forms ou WPF. |

Pour découvrir pas à pas comment utiliser les contrôles XAML WinRT wrappés, consultez [Héberger un contrôle XAML WinRT standard dans une application WPF](host-standard-control-with-xaml-islands.md).

### <a name="host-controls"></a>Contrôles hôtes

Pour les contrôles personnalisés et des scénarios autres que ceux couverts par les contrôles wrappés disponibles, les applications WPF et Windows Forms peuvent également utiliser le contrôle [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) proposé dans le Windows Community Toolkit.

| Contrôler | Système d’exploitation minimal pris en charge | Description |
|-----------------|-------------------------------|-------------|
| [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) | Windows 10 version 1903 | Peut héberger n’importe quel contrôle XAML WinRT dérivé de [Windows.UI.Xaml.UIElement](/uwp/api/windows.ui.xaml.uielement), notamment tout contrôle XAML WinRT interne fourni par le SDK Windows, ainsi que des contrôles personnalisés. |

Pour découvrir pas à pas comment utiliser le contrôle **WindowsXamlHost**, consultez [Héberger un contrôle XAML WinRT standard dans une application WPF](host-standard-control-with-xaml-islands.md) et [Héberger un contrôle XAML WinRT personnalisé dans une application WPF avec XAML Islands](host-custom-control-with-xaml-islands.md).

<span id="requirements" />

### <a name="configure-your-project-to-use-the-xaml-island-net-controls"></a>Configurer votre projet pour utiliser les contrôles .NET XAML Island

Les contrôles .NET XAML Island nécessitent Windows 10 version 1903 ou ultérieure. Pour utiliser ces contrôles, installez l’un des packages NuGet listés ci-dessous. Ces packages fournissent tout ce dont vous avez besoin pour utiliser les contrôles hôtes et les contrôles wrappés XAML Island, ainsi que d’autres packages NuGet connexes obligatoires.

| Type de contrôle | Package NuGet  | Articles connexes |
|-----------------|----------------|---------------------|
| [Contrôles wrappés](#wrapped-controls) | Version 6.0.0 ou ultérieure de ces packages : <ul><li>WPF : [Microsoft.Toolkit.Wpf.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)</li><li>Windows Forms : [Microsoft.Toolkit.Forms.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls)</li></ul>  | [Héberger un contrôle XAML WinRT standard dans une application WPF](host-standard-control-with-xaml-islands.md)  |
| [Contrôle hôte](#host-controls) | Version 6.0.0 ou ultérieure de ces packages : <ul><li>WPF : [Microsoft.Toolkit.Wpf.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost)</li><li>Windows Forms : [Microsoft.Toolkit.Forms.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost)</li></ul>  | [Héberger un contrôle XAML WinRT standard dans une application WPF](host-standard-control-with-xaml-islands.md)<br/>[Héberger un contrôle XAML WinRT personnalisé dans une application WPF](host-custom-control-with-xaml-islands.md)  |

Tenez compte des points suivants :

* Les packages de contrôles hôtes sont également inclus dans les packages de contrôles wrappés. Vous pouvez installer les packages de contrôles wrappés si vous souhaitez utiliser les deux ensembles de contrôles.

* Si vous hébergez un contrôle XAML WinRT personnalisé, vous devrez également effectuer quelques étapes supplémentaires pour référencer le contrôle personnalisé. Pour plus d’informations, consultez [Héberger un contrôle XAML WinRT personnalisé dans une application WPF avec XAML Islands](host-custom-control-with-xaml-islands.md).

### <a name="web-view-controls"></a>Contrôles de vue web

Le Windows Community Toolkit fournit également les contrôles .NET suivants pour héberger du contenu web dans les applications WPF et Windows Forms. Ces contrôles sont souvent utilisés dans des scénarios de modernisation d’applications de bureau similaires à ceux des contrôles XAML Island. Ils sont conservés dans le même dépôt [Microsoft.Toolkit.Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32) que les contrôles XAML Island.

| Contrôler | Système d’exploitation minimal pris en charge | Description |
|-----------------|-------------------------------|-------------|
| [WebView](/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10 version 1803 | Utilise le moteur de rendu Microsoft Edge pour afficher le contenu web. |
| [WebViewCompatible](/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | Fournit une version de **WebView** compatible avec d’autres versions de système d’exploitation. Ce contrôle utilise le moteur de rendu Microsoft Edge pour afficher le contenu web sur Windows 10 versions 1803 et ultérieures, et le moteur de rendu Internet Explorer pour afficher le contenu web sur les versions antérieures de Windows 10, Windows 8.x et Windows 7. |

Pour utiliser ces contrôles, installez l’un des packages NuGet suivants :

* WPF : [Microsoft.Toolkit.Wpf.UI.Controls.WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls.WebView)
* Windows Forms : [Microsoft.Toolkit.Forms.UI.Controls.WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls.WebView)

## <a name="c-win32-applications"></a>Applications Win32 C++

Les contrôles .NET XAML Island ne sont pas pris en charge dans les applications Win32 C++. Ces applications doivent utiliser à la place l’*API d’hébergement XAML UWP* fournie par le SDK Windows 10 (versions 1903 et ultérieures).

L’API d’hébergement XAML UWP se compose de plusieurs classes Windows Runtime et d’interfaces COM que votre application Win32 C++ peut utiliser pour héberger n’importe quel contrôle XAML WinRT dérivé de [Windows.UI.Xaml.UIElement](/uwp/api/windows.ui.xaml.uielement). Vous pouvez héberger des contrôles XAML WinRT dans n’importe quel élément d’interface utilisateur de votre application avec un handle de fenêtre (HWND) associé. Pour plus d’informations sur cette API, consultez les articles suivants.

* [Utilisation de l’API d’hébergement XAML UWP dans une application Win32 C++](using-the-xaml-hosting-api.md)
* [Héberger un contrôle XAML WinRT standard dans une application Win32 C++](host-standard-control-with-xaml-islands-cpp.md)
* [Héberger un contrôle XAML WinRT personnalisé dans une application Win32 C++](host-custom-control-with-xaml-islands-cpp.md)

> [!NOTE]
> Les contrôles wrappés et hôtes dans le Windows Community Toolkit utilisent l’API d’hébergement XAML UWP en interne et implémentent tous les comportements que vous devriez gérer vous-même si vous utilisiez cette API directement, comme la navigation au clavier et les changements de disposition. Pour les applications WPF et Windows Forms, nous vous recommandons vivement d’utiliser ces contrôles au lieu de l’API d’hébergement XAML UWP directement, car ils éliminent un grand nombre des détails d’implémentation associés à l’utilisation de l’API.

## <a name="architecture-of-xaml-islands"></a>Architecture des contrôles XAML Islands

Voici un bref aperçu de l’organisation architecturale des différents types de contrôles XAML Island au-dessus de l’API d’hébergement XAML UWP.

![Architecture des contrôles hôtes](images/xaml-islands/host-controls.png)

Les API présentes en bas de ce diagramme sont fournies avec le SDK Windows. Les contrôles wrappés et hôtes sont disponibles par le biais de packages NuGet dans le Windows Community Toolkit.

## <a name="limitations-and-workarounds"></a>Limitations et solutions de contournement

Les sections suivantes présentent les limitations et solutions de contournement pour certains scénarios de développement UWP dans les applications de bureau qui utilisent XAML Islands. 

### <a name="supported-only-with-workarounds"></a>Prise en charge uniquement avec des solutions de contournement

:heavy_check_mark: L’hébergement de contrôles de la [bibliothèque WinUI 2.x](../../winui/index.md) dans un îlot XAML est pris en charge de manière conditionnelle dans la version actuelle de XAML Islands. Si votre application de bureau utilise un [package MSIX](/windows/msix) pour le déploiement, vous pouvez héberger les contrôles WinUI des versions prépubliées et publiées du package NugGet [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml). Si votre application de bureau n’est pas empaquetée avec MSIX, vous pouvez héberger les contrôles WinUI uniquement si vous installez une version prépubliée du package NuGet [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml). La prise en charge de l’hébergement des contrôles de la [bibliothèque WinUI 3.0](../../winui/winui3/index.md) sera disponible dans une version ultérieure.

:heavy_check_mark: Pour accéder à l’élément racine d’une arborescence de contenu XAML dans un îlot XAML et obtenir des informations connexes sur le contexte de son hébergement, n’utilisez pas les classes [CoreWindow](/uwp/api/windows.ui.core.corewindow), [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview), et [Window](/uwp/api/windows.ui.xaml.window). Utilisez plutôt la classe [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot). Pour plus d’informations, consultez [cette section](#window-host-context-for-xaml-islands).

:heavy_check_mark: Pour prendre en charge le [contrat de partage](/windows/uwp/app-to-app/share-data) d’une application WPF, Windows Forms ou Win32 C++, votre application doit utiliser l’interface [IDataTransferManagerInterop](/windows/win32/api/shobjidl_core/nn-shobjidl_core-idatatransfermanagerinterop) pour obtenir l’objet [DataTransferManager](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager) afin de lancer l’opération de partage pour une fenêtre spécifique. Pour obtenir un exemple qui illustre l’utilisation de cette interface dans une application WPF, consultez [l’exemple ShareSource](https://github.com/microsoft/Windows-classic-samples/tree/master/Samples/ShareSource).

:heavy_check_mark: L’utilisation de `x:Bind` avec des contrôles hébergés dans XAML Islands n’est pas prise en charge. Vous devrez déclarer le modèle de données dans une bibliothèque .NET Standard.

### <a name="not-supported"></a>Non prise en charge

:no_entry_sign : Utilisation de XAML Islands dans les applications WPF et Windows Forms qui ciblent le .NET Framework. XAML Islands est pris en charge uniquement dans les applications qui ciblent .NET Core 3.x.

:no_entry_sign : Le contenu XAML UWP dans XAML Islands ne répond pas aux modifications de thème Windows du mode sombre au mode clair, ou vice versa, au moment de l’exécution. Le contenu répond aux modifications de contraste élevé au moment de l’exécution.

:no_entry_sign : ajout d'un contrôle **WebView** à un contrôle utilisateur personnalisé (sur thread, hors thread ou hors processus).

:no_entry_sign : Le contrôle [MediaPlayer](/uwp/api/Windows.Media.Playback.MediaPlayer) et le contrôle hôte [MediaPlayerElement](/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) ne sont pas pris en charge en mode plein écran.

:no_entry_sign : Entrée de texte avec la vue de l’écriture manuscrite. Pour en savoir plus sur cette fonctionnalité, consultez [cet article](/windows/uwp/design/controls-and-patterns/text-handwriting-view).

:no_entry_sign : Contrôles de texte qui utilisent des liens de contenu `@Places` et `@People`. Pour en savoir plus sur cette fonctionnalité, consultez [cet article](/windows/uwp/design/controls-and-patterns/content-links).

:no_entry_sign : Les îles XAML ne prennent pas en charge l’hébergement d’un [ContentDialog](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog) contenant un contrôle qui accepte une entrée de texte, comme [TextBox](/uwp/api/windows.ui.xaml.controls.textbox), [RichEditBox](/uwp/api/windows.ui.xaml.controls.richeditbox) ou [AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.autosuggestbox). Si vous procédez ainsi, le contrôle d’entrée ne répond pas correctement aux pressions sur les touches. Pour obtenir des fonctionnalités similaires à l’aide d’une île XAML, nous vous recommandons d’héberger un [Popup](/uwp/api/Windows.UI.Xaml.Controls.Primitives.Popup) contenant le contrôle d’entrée.

:no_entry_sign : Actuellement, les îles XAML ne prennent pas en charge l’affichage des fichiers SVG dans un contrôle [Windows.UI.Xaml.Controls.Image](/uwp/api/Windows.UI.Xaml.Controls.Image) hébergé ou en utilisant un objet [Windows.UI.Xaml.Media.Imaging.SvgImageSource](/uwp/api/windows.ui.xaml.media.imaging.svgimagesource). Pour contourner ce problème, convertissez les fichiers image que vous souhaitez afficher au format raster, comme JPG ou PNG.

### <a name="window-host-context-for-xaml-islands"></a>Contexte de l’hôte de fenêtrage pour XAML Islands

Quand vous hébergez XAML Islands dans une application de bureau, plusieurs arborescences de contenu XAML peuvent s’exécuter simultanément sur le même thread. Pour accéder à l’élément racine d’une arborescence de contenu XAML dans un XAML Islands et obtenir des informations connexes sur le contexte de son hébergement, utilisez la classe [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot). Les classes [CoreWindow](/uwp/api/windows.ui.core.corewindow), [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview) et [Window](/uwp/api/windows.ui.xaml.window) ne fournissent pas les informations correctes pour XAML Islands. Les objets [CoreWindow](/uwp/api/windows.ui.core.corewindow) et [Window](/uwp/api/windows.ui.xaml.window) existent sur le thread et sont accessibles à votre application, mais ils ne retournent pas de limites ou de visibilité significatives (ils restent toujours invisibles et ont une taille de 1x1). Pour plus d’informations, consultez [Hôtes de fenêtrage](/windows/uwp/design/layout/show-multiple-views#windowing-hosts).

Par exemple, pour obtenir le rectangle englobant de la fenêtre qui contient un contrôle XAML WinRT hébergé dans XAML Islands, utilisez la propriété [XamlRoot.Size](/uwp/api/windows.ui.xaml.xamlroot.size) du contrôle. Étant donné que chaque contrôle XAML WinRT qui peut être hébergé dans XAML Islands est dérivé de [Windows.UI.Xaml.UIElement](/uwp/api/windows.ui.xaml.uielement), vous pouvez utiliser la propriété [XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot) du contrôle pour accéder à l’objet **XamlRoot**.

```csharp
Size windowSize = myUWPControl.XamlRoot.Size;
```

N’utilisez pas la propriété [CoreWindows.Bounds](/uwp/api/windows.ui.core.corewindow.bounds) pour obtenir le rectangle englobant.

```csharp
// This will return incorrect information for a WinRT XAML control that is hosted in a XAML Island.
Rect windowSize = CoreWindow.GetForCurrentThread().Bounds;
```

Pour avoir la liste des API de fenêtrage courantes à proscrire dans le contexte de XAML Islands et connaître le code de remplacement [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot) recommandé, consultez le tableau dans [cette section](/windows/uwp/design/layout/show-multiple-views#make-code-portable-across-windowing-hosts).

Pour obtenir un exemple qui illustre l’utilisation de cette interface dans une application WPF, consultez [l’exemple ShareSource](https://github.com/microsoft/Windows-classic-samples/tree/master/Samples/ShareSource).

## <a name="additional-resources"></a>Ressources supplémentaires

Pour obtenir plus d’informations générales et des tutoriels sur l’utilisation de XAML Islands, consultez les articles et ressources suivants :

* [Moderniser une application WPF](modernize-wpf-tutorial.md) : Ce tutoriel fournit des instructions pas à pas pour utiliser des contrôles wrappés et des contrôles hôtes dans le Windows Community Toolkit afin d’ajouter des contrôles XAML WinRT à une application métier WPF existante. Ce tutoriel comprend le code complet de l’application WPF ainsi que des instructions détaillées pour chaque étape du processus.
* [Exemples de code XAML Islands](https://github.com/microsoft/Xaml-Islands-Samples) : Ce dépôt contient des exemples Windows Forms, WPF et C++/Win32 qui montrent comment utiliser XAML Islands.
* [XAML Islands v1 - Updates and Roadmap](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap) : ce billet de blog répond à de nombreuses questions courantes sur XAML Islands et propose une feuille de route détaillée pour le développement.