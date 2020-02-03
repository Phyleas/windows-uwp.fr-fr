---
description: Ce guide vous aide à créer des interfaces utilisateur UWP Fluent directement dans vos applications WPF et Windows Forms
title: Contrôles UWP dans les applications de bureau
ms.date: 01/24/2020
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, xaml islands, îles xaml
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 96705faff278c4cab31e0ab271bc31d08261401b
ms.sourcegitcommit: 1455e12a50f98823bfa3730c1d90337b1983b711
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76814009"
---
# <a name="host-uwp-xaml-controls-in-desktop-apps-xaml-islands"></a>Héberger des contrôles XAML UWP dans des applications de bureau (XAML Islands)

À compter de Windows 10 version 1903, vous pouvez héberger des contrôles UWP dans des applications de bureau non conçues pour UWP à l’aide d’une fonctionnalité appelée *XAML Islands* (îles XAML). Vous pouvez ainsi améliorer l’apparence, le comportement et les fonctionnalités de vos applications WPF, Windows Forms et Win32 C++ existantes, mais aussi bénéficier des dernières nouveautés de l’interface utilisateur Windows 10 qui sont uniquement disponibles par le biais de contrôles UWP. Cela signifie que vous pouvez utiliser des fonctionnalités UWP telles que [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions) et des contrôles prenant en charge le [système Fluent Design](/windows/uwp/design/fluent-design-system/index) dans vos applications WPF, Windows Forms et Win32 C++ existantes.

Vous pouvez héberger n’importe quel contrôle UWP qui dérive de [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), notamment :

* Tout contrôle UWP de première partie fourni par le SDK Windows ou la bibliothèque WinUI.
* Tout contrôle UWP personnalisé (par exemple, un contrôle utilisateur qui se compose de plusieurs contrôles UWP fonctionnant ensemble). Vous devez disposer du code source du contrôle personnalisé pour pouvoir le compiler avec votre application.

À la base, les îles XAML sont créées à l’aide de l’*API d’hébergement XAML UWP*. Cette API comprend plusieurs classes Windows Runtime et interfaces COM qui ont été introduites dans le SDK Windows 10 version 1903. Nous fournissons également dans le [Windows Community Toolkit](https://docs.microsoft.com/windows/uwpcommunitytoolkit/) un ensemble de contrôles .NET XAML Island qui utilisent l’API d’hébergement XAML UWP en interne et qui offrent une expérience de développement plus pratique pour les applications WPF et Windows Forms.

La façon dont vous utilisez XAML Islands dépend du type de votre application et des types de contrôles UWP que vous souhaitez héberger.

> [!NOTE]
> Si vous avez des choses à nous dire sur XAML Islands, ouvrez un nouveau problème dans le [dépôt Microsoft.Toolkit.Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) et laissez-y vos commentaires. Si vous préférez que vos commentaires restent privés, envoyez-les à XamlIslandsFeedback@microsoft.com. Vos insights et vos scénarios sont très importants pour nous.

## <a name="wpf-and-windows-forms-applications"></a>Applications WPF et Windows Forms

Nous recommandons que les applications WPF et Windows Forms utilisent les contrôles .NET XAML Island disponibles dans le Windows Community Toolkit. Ces contrôles fournissent un modèle objet qui imite les propriétés, méthodes et événements des contrôles UWP correspondants ou qui vous donne accès à ces éléments. Ils gèrent également des comportements tels que la navigation au clavier et les changements de disposition.

Il existe deux ensembles de contrôles XAML Island pour les applications WPF et Windows Forms : les *contrôles wrappés* et les *contrôles hôtes*. 

### <a name="wrapped-controls"></a>Contrôles wrappés

Les applications WPF et Windows Forms peuvent utiliser une sélection de contrôles XAML Island qui wrappent l’interface et les fonctionnalités d’un contrôle UWP spécifique. Vous pouvez ajouter ces contrôles directement à l’aire de conception de votre projet WPF ou Windows Forms, puis les utiliser comme tout autre contrôle WPF ou Windows Forms dans le concepteur.

Les contrôles UWP wrappés suivants sont actuellement disponibles dans le Windows Community Toolkit. 

| Contrôler | Système d’exploitation minimal pris en charge | Description |
|-----------------|-------------------------------|-------------|
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10 version 1903 | Fournissent une surface et les barres d’outils associées pour l’interaction utilisateur basée sur Windows Ink dans votre application de bureau Windows Forms ou WPF. |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10 version 1903 | Incorpore une vue qui diffuse en streaming du contenu multimédia tel que des vidéos dans votre application de bureau Windows Forms ou WPF. |
| [MapControl](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10 version 1903 | Permet d’afficher une carte symbolique ou photoréaliste dans votre application de bureau Windows Forms ou WPF. |

Pour découvrir pas à pas comment utiliser les contrôles UWP wrappés, consultez [Héberger un contrôle UWP standard dans une application WPF](host-standard-control-with-xaml-islands.md).

### <a name="host-controls"></a>Contrôles hôtes

Pour les scénarios autres que ceux couverts par les contrôles wrappés disponibles, les applications WPF et Windows Forms peuvent également utiliser le contrôle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) proposé dans le Windows Community Toolkit.

| Contrôler | Système d’exploitation minimal pris en charge | Description |
|-----------------|-------------------------------|-------------|
| [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) | Windows 10 version 1903 | Peut héberger n’importe quel contrôle UWP dérivé de [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), notamment tout contrôle UWP de première partie fourni par le SDK Windows ainsi que des contrôles personnalisés. |

Pour découvrir pas à pas comment utiliser le contrôle **WindowsXamlHost**, consultez [Héberger un contrôle UWP standard dans une application WPF](host-standard-control-with-xaml-islands.md) et [Héberger un contrôle UWP personnalisé dans une application WPF avec XAML Islands](host-custom-control-with-xaml-islands.md).

> [!NOTE]
> L’utilisation du contrôle **WindowsXamlHost** pour héberger des contrôles UWP personnalisés est uniquement prise en charge dans les applications WPF et Windows Forms ciblant .NET Core 3. L’hébergement de contrôles UWP de première partie fournis par le SDK Windows est pris en charge dans les applications qui ciblent le .NET Framework ou .NET Core 3.

<span id="requirements" />

### <a name="configure-your-project-to-use-the-xaml-island-net-controls"></a>Configurer votre projet pour utiliser les contrôles .NET XAML Island

Les contrôles .NET XAML Island nécessitent Windows 10 version 1903 ou ultérieure. Pour utiliser ces contrôles, installez l’un des packages NuGet listés ci-dessous. Ces packages fournissent tout ce dont vous avez besoin pour utiliser les contrôles hôtes et les contrôles wrappés XAML Island, ainsi que d’autres packages NuGet connexes obligatoires.

| Type de contrôle | Package NuGet  | Articles connexes |
|-----------------|----------------|---------------------|
| [Contrôles wrappés](#wrapped-controls) | Version 6.0.0 ou ultérieure de ces packages : <ul><li>WPF : [Microsoft.Toolkit.Wpf.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)</li><li>Windows Forms : [Microsoft.Toolkit.Forms.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls)</li></ul>  | [Héberger un contrôle UWP standard dans une application WPF](host-standard-control-with-xaml-islands.md)  |
| [Contrôle hôte](#host-controls) | Version 6.0.0 ou ultérieure de ces packages : <ul><li>WPF : [Microsoft.Toolkit.Wpf.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost)</li><li>Windows Forms : [Microsoft.Toolkit.Forms.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost)</li></ul>  | [Héberger un contrôle UWP standard dans une application WPF](host-standard-control-with-xaml-islands.md)<br/>[Héberger un contrôle UWP personnalisé dans une application WPF](host-custom-control-with-xaml-islands.md)  |

Tenez compte des points suivants :

* Les packages de contrôles hôtes sont également inclus dans les packages de contrôles wrappés. Vous pouvez installer les packages de contrôles wrappés si vous souhaitez utiliser les deux ensembles de contrôles.

* Si vous hébergez un contrôle UWP personnalisé, votre projet WPF ou Windows Forms doit cibler .NET Core 3. L’hébergement de contrôles UWP personnalisés n’est pas pris en charge dans les applications qui ciblent le .NET Framework. Vous devrez également effectuer quelques étapes supplémentaires pour référencer le contrôle personnalisé. Pour plus d’informations, consultez [Héberger un contrôle UWP personnalisé dans une application WPF avec XAML Islands](host-custom-control-with-xaml-islands.md).

* Dans les versions antérieures de ces instructions, vous deviez ajouter l’élément `maxversiontested` à un manifeste d’application dans votre projet WPF ou Windows Forms. Désormais, tant que vous utilisez les dernières versions listées ci-dessus des packages NuGet, il est inutile d’ajouter cet élément à votre manifeste.

### <a name="architecture-of-xaml-island-net-controls"></a>Architecture des contrôles .NET XAML Island

Voici un bref aperçu de l’organisation architecturale des différents types de contrôles XAML Island au-dessus de l’API d’hébergement XAML UWP.

![Architecture des contrôles hôtes](images/xaml-islands/host-controls.png)

Les API présentes en bas de ce diagramme sont fournies avec le SDK Windows. Les contrôles wrappés et hôtes sont disponibles par le biais de packages NuGet dans le Windows Community Toolkit.

### <a name="web-view-controls"></a>Contrôles de vue web

Le Windows Community Toolkit fournit également les contrôles .NET suivants pour héberger du contenu web dans les applications WPF et Windows Forms. Ces contrôles sont souvent utilisés dans des scénarios de modernisation d’applications de bureau similaires à ceux des contrôles XAML Island. Ils sont conservés dans le même dépôt [Microsoft.Toolkit.Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32) que les contrôles XAML Island.

| Contrôler | Système d’exploitation minimal pris en charge | Description |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10 version 1803 | Utilise le moteur de rendu Microsoft Edge pour afficher le contenu web. |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | Fournit une version de **WebView** compatible avec d’autres versions de système d’exploitation. Ce contrôle utilise le moteur de rendu Microsoft Edge pour afficher le contenu web sur Windows 10 versions 1803 et ultérieures, et le moteur de rendu Internet Explorer pour afficher le contenu web sur les versions antérieures de Windows 10, Windows 8.x et Windows 7. |

Pour utiliser ces contrôles, installez l’un des packages NuGet suivants :

* WPF : [Microsoft.Toolkit.Wpf.UI.Controls.WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls.WebView)
* Windows Forms : [Microsoft.Toolkit.Forms.UI.Controls.WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls.WebView)

## <a name="c-win32-applications"></a>Applications Win32 C++

Les contrôles .NET XAML Island ne sont pas pris en charge dans les applications Win32 C++. Ces applications doivent utiliser à la place l’*API d’hébergement XAML UWP* fournie par le SDK Windows 10 (versions 1903 et ultérieures).

L’API d’hébergement XAML UWP se compose de plusieurs classes Windows Runtime et d’interfaces COM que votre application Win32 C++ peut utiliser pour héberger n’importe quel contrôle UWP dérivé de [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement). Vous pouvez héberger des contrôles UWP dans n’importe quel élément d’interface utilisateur de votre application avec un handle de fenêtre (HWND) associé. Pour plus d’informations sur cette API, notamment les prérequis, consultez [Utilisation de l’API d’hébergement XAML UWP dans une application Win32 C++](using-the-xaml-hosting-api.md).

> [!NOTE]
> Les contrôles wrappés et hôtes dans le Windows Community Toolkit utilisent l’API d’hébergement XAML UWP en interne et implémentent tous les comportements que vous devriez gérer vous-même si vous utilisiez cette API directement, comme la navigation au clavier et les changements de disposition. Pour les applications WPF et Windows Forms, nous vous recommandons vivement d’utiliser ces contrôles au lieu de l’API d’hébergement XAML UWP directement, car ils éliminent un grand nombre des détails d’implémentation associés à l’utilisation de l’API.

## <a name="feature-roadmap"></a>Feuille de route des fonctionnalités

Voici l’état actuel des fonctionnalités liées à XAML Islands à compter de Windows 10 version 1903 et de la version 6.0 du Windows Community Toolkit :

* **Applications Win32 C++ :** l’API d’hébergement XAML UWP associée à Windows 10 version 1903 est considérée comme la version 1.0.
* **Applications managées qui ciblent le .NET Framework 4.6.2 et ultérieur :** les contrôles XAML Island disponibles dans les [packages NuGet version 6.0.0](#configure-your-project-to-use-the-xaml-island-net-controls) sont considérés comme la version 1.0 pour les applications qui ciblent le .NET Framework 4.6.2 et ultérieur.
* **Applications managées qui ciblent .NET Core 3.0 et ultérieur :** les contrôles disponibles dans les [packages NuGet version 6.0.0](#configure-your-project-to-use-the-xaml-island-net-controls) sont toujours en Developer Preview pour les applications qui ciblent .NET Core 3.0 et ultérieur. La version 1.0 de ces contrôles pour .NET Core 3.0 et ultérieur est prévue pour une future version.

## <a name="additional-resources"></a>Ressources supplémentaires

Pour obtenir plus d’informations générales et des tutoriels sur l’utilisation de XAML Islands, consultez les articles et ressources suivants :

* [Moderniser une application WPF](modernize-wpf-tutorial.md) : ce tutoriel fournit des instructions pas à pas pour utiliser des contrôles wrappés et hôtes dans le Windows Community Toolkit afin d’ajouter des contrôles UWP à une application métier WPF existante. Ce tutoriel comprend le code complet de l’application WPF ainsi que des instructions détaillées pour chaque étape du processus.
* [Exemples de code XAML Islands](https://github.com/microsoft/Xaml-Islands-Samples) : Ce dépôt contient des exemples Windows Forms, WPF et C++/Win32 qui montrent comment utiliser XAML Islands.
* [XAML Islands v1 - Updates and Roadmap](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap) : ce billet de blog répond à de nombreuses questions courantes sur XAML Islands et propose une feuille de route détaillée pour le développement.
