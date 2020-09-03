---
description: Cet article explique comment héberger une interface utilisateur XAML UWP dans votre application de bureau Win32 C++.
title: Utilisation de l’API d’hébergement XAML UWP dans une application Win32 C++
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, win32, îlots xaml
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 8427519dd010553eb1f4f00f951dcc747a94b0c0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174183"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-c-win32-app"></a>Utilisation de l’API d’hébergement XAML UWP dans une application Win32 C++

Depuis Windows 10 version 1903, les applications de bureau non UWP (à savoir, les applications WPF, Windows Forms et Win32 C++) peuvent utiliser l’*API d’hébergement XAML UWP* pour héberger des contrôles UWP dans tout élément d’interface utilisateur associé à un identificateur de fenêtre (HWND). Cette API permet aux applications de bureau non UWP d’utiliser les dernières fonctionnalités de l’interface utilisateur Windows 10 qui sont disponibles uniquement via des contrôles UWP. Par exemple, des applications de bureau non UWP peuvent utiliser cette API pour héberger des contrôles UWP qui utilisent le [Système de conception Fluent](/windows/uwp/design/fluent-design-system/index) et prennent en charge [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions).

L’API d’hébergement XAML UWP constitue le fondement d’un ensemble plus large de contrôles que nous fournissons pour permettre aux développeurs d’intégrer l’interface utilisateur Fluent dans des applications de bureau non UWP. Cette fonctionnalité est nommée *îlots XAML*. Pour en obtenir une vue d’ensemble, consultez [Héberger des contrôles XAML UWP dans des applications de bureau (îlots XAML)](xaml-islands.md).

> [!NOTE]
> Si vous avez des choses à nous dire sur XAML Islands, ouvrez un nouveau problème dans le [dépôt Microsoft.Toolkit.Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) et laissez-y vos commentaires. Si vous préférez que vos commentaires restent privés, envoyez-les à XamlIslandsFeedback@microsoft.com. Vos insights et vos scénarios sont très importants pour nous.

## <a name="is-the-uwp-xaml-hosting-api-the-right-choice-for-your-desktop-app"></a>L’API d’hébergement XAML UWP est-elle le bon choix pour votre application de bureau ?

L’API d’hébergement XAML UWP fournit l’infrastructure de bas niveau pour l’hébergement de contrôles UWP dans des applications de bureau. Certains types d’applications de bureau ont la possibilité d’utiliser d’autres API plus pratiques pour atteindre cet objectif.

* Si vous avez une application de bureau Win32 C++ dans laquelle vous souhaitez héberger des contrôles UWP, vous devez utiliser l’API d’hébergement XAML UWP. Il n’existe aucune alternative pour ces types d’applications.

* Pour des applications WPF et Windows Forms, nous vous recommandons vivement d’utiliser les [contrôles .NET d’îlots XAML](xaml-islands.md#wpf-and-windows-forms-applications) disponibles dans le Kit de ressources Communauté Windows au lieu d’utiliser directement l’API d’hébergement XAML UWP. Ces contrôles utilisent l’API d’hébergement XAML UWP en interne et implémentent tous les comportements que vous devriez gérer vous-même si vous utilisiez cette API directement, comme la navigation au clavier et les changements de disposition.

Étant donné que nous recommandons que seules les applications Win32 C++ utilisent l’API d’hébergement XAML UWP, cet article fournit principalement des instructions et des exemples pour les applications Win32 C++. Vous pouvez cependant utiliser l’API d’hébergement XAML UWP dans des applications WPF et Windows Forms si vous le souhaitez. Cet article pointe vers du code source pertinent pour les [contrôles hôtes](xaml-islands.md#host-controls) pour WPF et Windows Forms figurant dans le Kit de ressources Communauté Windows. Vous pouvez ainsi voir comment ces contrôles utilisent l’API d’hébergement XAML UWP.

## <a name="learn-how-to-use-the-xaml-hosting-api"></a>Découvrez comment utiliser l’API d’hébergement XAML

Pour suivre des instructions pas à pas avec des exemples de code pour l’utilisation de l’API d’hébergement XAML dans des applications Win32 C++, consultez les articles suivants :

* [Héberger un contrôle UWP standard](host-standard-control-with-xaml-islands-cpp.md)
* [Héberger un contrôle UWP personnalisé](host-custom-control-with-xaml-islands-cpp.md)
* [Scénarios avancés](advanced-scenarios-xaml-islands-cpp.md)

## <a name="samples"></a>exemples

La façon dont vous utilisez l’API d’hébergement XAML UWP dans votre code dépend de votre type d’application, de la conception de votre application et d’autres facteurs. Pour illustrer l’utilisation de cette API dans le contexte d’une application complète, cet article fait référence au code des exemples suivants.

### <a name="c-win32"></a>Win32 C++

Les exemples suivants montrent comment utiliser l’API d’hébergement XAML UWP dans une application Win32 C++ :

* [Exemple d’îlot XAML simple](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Standalone_Samples/CppWinRT_Basic_Win32App). Cet exemple illustre une implémentation de base de l’hébergement d’un contrôle UWP dans une application Win32 C++ non empaquetée (non intégrée dans un package MSIX).

* [Exemples d’îlot XAML avec contrôle personnalisé](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32). Cet exemple illustre une implémentation complète de l’hébergement d’un contrôle UWP personnalisé dans une application Win32 C++ empaquetée, ainsi que la gestion d’autres comportements tels que la saisie au clavier et la navigation dans le focus.

### <a name="wpf-and-windows-forms"></a>WPF et Windows Forms

Le contrôle [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) dans le Kit de ressources Communauté Windows fait référence pour l’utilisation de l’API d’hébergement UWP dans des applications WPF et Windows Forms. Le code source est disponible aux emplacements suivants :

* Pour la version WPF du contrôle, [accédez ici](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost). La version WPF dérive de [System.Windows.Interop.HwndHost](/dotnet/api/system.windows.interop.hwndhost).

* Pour la version Windows Forms du contrôle, [cliquez ici](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost). La version Windows Forms dérive de [System.Windows.Forms.Control](/dotnet/api/system.windows.forms.control).

> [!NOTE]
> Nous vous recommandons vivement d’utiliser les [contrôles .NET d’îlots XAML](xaml-islands.md#wpf-and-windows-forms-applications) disponibles dans le Kit de ressources Communauté Windows au lieu d’utiliser l’API d’hébergement XAML UWP directement dans des applications WPF et Windows Forms. Les exemples de liens WPF et Windows Forms dans cet article sont fournis uniquement à titre d’illustration.

## <a name="architecture-of-the-api"></a>Architecture de l’API

L’API d’hébergement XAML UWP comprend les principaux types de Windows Runtime et interfaces COM suivants.

|  Type ou interface | Description |
|--------------------|-------------|
| [WindowsXamlManager](/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) | Cette classe représente l’infrastructure XAML UWP. Cette classe fournit une méthode statique [InitializeForCurrentThread](/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) unique qui initialise l’infrastructure XAML UWP sur le thread actuel de l’application de bureau. |
| [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) | Cette classe représente une instance du contenu XAML UWP que vous hébergez dans votre application de bureau. Le membre le plus important de cette classe est la propriété [Content](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content). Vous attribuez cette propriété à un objet [Windows.UI.Xaml.UIElement](/uwp/api/windows.ui.xaml.uielement) que vous souhaitez héberger. Cette classe comprend également d’autres membres pour le routage de la navigation en mode focus au clavier à destination et à partir des îlots XAML. |
| IDesktopWindowXamlSourceNative | Cette interface COM fournit la méthode **AttachToWindow** que vous utilisez pour attacher un îlot XAML dans votre application à un élément d’interface utilisateur parent. Chaque objet **DesktopWindowXamlSource** implémente cette interface. Cette interface est déclarée dans windows.ui.xaml.hosting.desktopwindowxamlsource.h. |
| IDesktopWindowXamlSourceNative2 | Cette interface COM fournit la méthode **PreTranslateMessage** qui permet à l’infrastructure XAML UWP de traiter correctement certains messages Windows. Chaque objet **DesktopWindowXamlSource** implémente cette interface. Cette interface est déclarée dans windows.ui.xaml.hosting.desktopwindowxamlsource.h. |

Le diagramme suivant illustre la hiérarchie d’objets dans un îlot XAML hébergé dans une application de bureau.

* Au niveau de base se trouve l’élément d’interface utilisateur dans votre application où vous souhaitez héberger l’îlot XAML. Cet élément d’interface utilisateur doit avoir un identificateur de fenêtre (HWND). Les exemples d’éléments d’interface utilisateur dans lesquels vous pouvez héberger un îlot XAML incluent une [fenêtre](/windows/desktop/winmsg/about-windows) pour les applications Win32 C++, un [System.Windows.Interop.HwndHost](/dotnet/api/system.windows.interop.hwndhost) pour les applications WPF, et un [System.Windows.Interop.HwndHost](/dotnet/api/system.windows.forms.control) pour les applications Windows Forms.

* Au niveau suivant se trouve un objet **DesktopWindowXamlSource**. Cet objet fournit l’infrastructure pour l’hébergement de l’îlot XAML. Votre code est chargé de créer cet objet et de l’attacher à l’élément d’interface utilisateur parent.

* Lorsque vous créez un objet **DesktopWindowXamlSource**, celui-ci crée automatiquement une fenêtre enfant native pour héberger votre contrôle UWP. Cette fenêtre enfant native est principalement abstraite de votre code, mais vous pouvez accéder à son identificateur (HWND) si nécessaire.

* Enfin, au niveau supérieur figure le contrôle UWP que vous souhaitez héberger dans votre application de bureau. Il peut s’agir de n’importe quel objet UWP dérivant de [Windows.UI.Xaml.UIElement](/uwp/api/windows.ui.xaml.uielement), notamment de tout contrôle UWP fourni par le SDK Windows, ainsi que de contrôles utilisateur personnalisés.

![Architecture de DesktopWindowXamlSource](images/xaml-islands/xaml-hosting-api-rev2.png)

> [!NOTE]
> Quand vous hébergez XAML Islands dans une application de bureau, plusieurs arborescences de contenu XAML peuvent s’exécuter simultanément sur le même thread. Pour accéder à l’élément racine d’une arborescence de contenu XAML dans un XAML Islands et obtenir des informations connexes sur le contexte de son hébergement, utilisez la classe [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot). Les classes [CoreWindow](/uwp/api/windows.ui.core.corewindow), [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview) et [Window](/uwp/api/windows.ui.xaml.window) ne fournissent pas les informations correctes pour les îlots XAML. Pour plus d’informations, consultez [cette section](xaml-islands.md#window-host-context-for-xaml-islands).

## <a name="troubleshooting"></a>Résolution des problèmes

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>Erreur lors de l’utilisation de l’API d’hébergement XAML UWP dans une application UWP

| Problème | Solution |
|-------|------------|
| Votre application reçoit une **COMException** avec le message suivant : « Impossible d’activer DesktopWindowXamlSource. Ce type ne peut pas être utilisé dans une application UWP. » Ou « impossible d’activer WindowsXamlManager. Ce type ne peut pas être utilisé dans une application UWP. » | Cette erreur indique que vous essayez d’utiliser l’API d’hébergement XAML UWP (en particulier, vous essayez d’instancier les types [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) ou [WindowsXamlManager](/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)) dans une application UWP. L’API d’hébergement XAML UWP est destinée à être utilisée uniquement dans les applications de bureau non UWP, telles que les applications WPF, Windows Forms et Win32 C++. |

### <a name="error-trying-to-use-the-windowsxamlmanager-or-desktopwindowxamlsource-types"></a>Erreur lors de la tentative d’utilisation des types WindowsXamlManager ou DesktopWindowXamlSource

| Problème | Solution |
|-------|------------|
| Votre application reçoit une exception avec le message suivant : «WindowsXamlManager et DesktopWindowXamlSource sont pris en charge pour les applications ciblant Windows, versions 10.0.18226.0 et ultérieures. Vérifiez le manifeste de l’application ou le manifeste du package et assurez-vous que la propriété MaxTestedVersion est à jour. » | Cette erreur indique que votre application a tenté d’utiliser les types **WindowsXamlManager** ou **DesktopWindowXamlSource** dans l’API d’hébergement XAML UWP, mais que le système d’exploitation ne peut pas déterminer si l’application a été créée pour cibler Windows 10, version 1903 ou ultérieure. L’API d’hébergement XAML UWP a été introduite pour la première fois en préversion dans une version antérieure de Windows 10, mais elle n’est prise en charge qu’à partir Windows 10, version 1903.</p></p>Pour résoudre ce problème, créez un package MSIX pour l’application et exécutez-la à partir du package, ou installez le package NuGet [Microsoft.Toolkit.Win32.UI.SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) dans votre projet.  |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>Erreur lors de l’attachement à une fenêtre sur un autre thread

| Problème | Solution |
|-------|------------|
| Votre application reçoit une **COMException** avec le message suivant : « La méthode AttachToWindow a échoué parce que le HWND spécifié a été créé sur un autre thread. » | Cette erreur indique que votre application a appelé la méthode **IDesktopWindowXamlSourceNative::AttachToWindow** et lui a passé le HWND d’une fenêtre qui a été créée sur un autre thread. Vous devez passer à cette méthode le HWND d’une fenêtre créée sur le même thread que le code à partir duquel vous appelez la méthode. |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>Erreur lors de l’attachement à une fenêtre dans une fenêtre de niveau supérieur différente

| Problème | Solution |
|-------|------------|
| Votre application reçoit une **COMException** avec le message suivant : « La méthode AttachToWindow a échoué parce que le HWND spécifié descend d’une fenêtre de niveau supérieur différente du HWND qui était précédemment passé à la méthode AttachToWindow sur le même thread. » | Cette erreur indique que votre application a appelé la méthode **IDesktopWindowXamlSourceNative::AttachToWindow** et lui a passé le HWND d’une fenêtre qui descend d’une fenêtre de niveau supérieur différente d’une fenêtre que vous avez spécifiée dans un appel précédent à cette méthode sur le même thread.</p></p>Après que votre application appelle la méthode **AttachToWindow** sur un thread particulier, tous les autres objets [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) sur le même thread peuvent être attachés uniquement à des fenêtres qui descendent de la fenêtre de niveau supérieur qui a été passée dans le premier appel à la méthode **AttachToWindow**. Lorsque tous les objets **DesktopWindowXamlSource** sont fermés pour un thread particulier, l’objet **DesktopWindowXamlSource** suivant est libre de s’attacher à nouveau à toute fenêtre.</p></p>Pour résoudre ce problème, fermez tous les objets **DesktopWindowXamlSource** liés à d’autres fenêtres de niveau supérieur sur ce thread, ou créez un thread pour cet objet **DesktopWindowXamlSource**. |

## <a name="related-topics"></a>Rubriques connexes

* [Héberger des contrôles XAML UWP dans des applications de bureau (îlots XAML)](xaml-islands.md)
* [Héberger un contrôle UWP standard dans une application Win32 C++](host-standard-control-with-xaml-islands-cpp.md)
* [Héberger un contrôle UWP personnalisé dans une application Win32 C++](host-custom-control-with-xaml-islands-cpp.md)
* [Scénarios avancés pour îlots XAML dans les applications Win32 C++](advanced-scenarios-xaml-islands-cpp.md)
* [Exemples de code d’îlots XAML](https://github.com/microsoft/Xaml-Islands-Samples)
* [Exemple d’îlots XAML Win32 C++](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)