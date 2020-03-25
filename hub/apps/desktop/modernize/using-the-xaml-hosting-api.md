---
description: Cet article explique comment héberger une interface utilisateur XAML UWP dans C++ votre application Desktop Win32.
title: Utilisation de l’API d’hébergement XAML UWP dans une application Win32 C++
ms.date: 03/23/2020
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, Win32, îlots XAML
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 36c3aeb7a51c84e92c5bca461aee7efe50740237
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218459"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-c-win32-app"></a>Utilisation de l’API d’hébergement XAML UWP dans une application Win32 C++

À compter de Windows 10, version 1903, les applications de bureau non UWP C++ (y compris les applications Win32, WPF et Windows Forms) peuvent utiliser l' *API d’hébergement XAML UWP* pour héberger des contrôles UWP dans n’importe quel élément d’interface utilisateur associé à un handle de fenêtre (HWND). Cette API permet aux applications de bureau non UWP d’utiliser les dernières fonctionnalités de l’interface utilisateur de Windows 10 qui sont uniquement disponibles via les contrôles UWP. Par exemple, les applications de bureau non UWP peuvent utiliser cette API pour héberger des contrôles UWP qui utilisent le [système de conception Fluent](/windows/uwp/design/fluent-design-system/index) et prennent en charge [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions).

L’API d’hébergement XAML UWP fournit la base d’un ensemble plus large de contrôles que nous fournissons pour permettre aux développeurs d’intégrer l’interface utilisateur Fluent à des applications de bureau non UWP. Cette fonctionnalité est appelée *îlots XAML*. Pour obtenir une vue d’ensemble de cette fonctionnalité, consultez [héberger des contrôles XAML UWP dans les applications de bureau (îlots XAML)](xaml-islands.md).

> [!NOTE]
> Si vous avez des choses à nous dire sur XAML Islands, ouvrez un nouveau problème dans le [dépôt Microsoft.Toolkit.Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) et laissez-y vos commentaires. Si vous préférez que vos commentaires restent privés, envoyez-les à XamlIslandsFeedback@microsoft.com. Vos insights et vos scénarios sont très importants pour nous.

## <a name="is-the-uwp-xaml-hosting-api-the-right-choice-for-your-desktop-app"></a>L’API d’hébergement XAML UWP est-elle le bon choix pour votre application de bureau ?

L’API d’hébergement XAML UWP fournit l’infrastructure de bas niveau pour l’hébergement de contrôles UWP dans les applications de bureau. Certains types d’applications de bureau ont la possibilité d’utiliser des API alternatives et plus pratiques pour atteindre cet objectif.

* Si vous disposez d' C++ une application de bureau Win32 et souhaitez héberger des contrôles UWP dans votre application, vous devez utiliser l’API d’hébergement XAML UWP. Il n’existe aucune solution de remplacement pour ces types d’applications.

* Pour les applications WPF et Windows Forms, nous vous recommandons vivement d’utiliser les [contrôles .net Island](xaml-islands.md#wpf-and-windows-forms-applications) dans le kit de pratiques de la communauté Windows au lieu d’utiliser directement l’API d’hébergement XAML UWP. Ces contrôles utilisent l’API d’hébergement XAML UWP en interne et implémentent tout le comportement que vous devrez sinon gérer vous-même si vous utilisiez l’API d’hébergement XAML UWP directement, y compris la navigation au clavier et les modifications de disposition.

Étant donné que nous recommandons C++ que seules les applications Win32 utilisent l’API d’hébergement XAML UWP, cet article fournit principalement C++ des instructions et des exemples pour les applications Win32. Toutefois, vous pouvez utiliser l’API d’hébergement XAML UWP dans WPF et les applications de Windows Forms si vous le souhaitez. Cet article pointe vers le code source pertinent pour les [contrôles hôtes](xaml-islands.md#host-controls) pour WPF et Windows Forms dans la boîte à outils de la communauté Windows afin que vous puissiez voir comment l’API d’hébergement XAML UWP est utilisée par ces contrôles.

## <a name="learn-how-to-use-the-xaml-hosting-api"></a>Découvrez comment utiliser l’API d’hébergement XAML

Pour suivre des instructions pas à pas avec des exemples de code pour l’utilisation de l’API C++ d’hébergement XAML dans les applications Win32, consultez les articles suivants :

* [Héberger un contrôle UWP standard](host-standard-control-with-xaml-islands-cpp.md)
* [Héberger un contrôle UWP personnalisé](host-custom-control-with-xaml-islands-cpp.md)
* [Scénarios avancés](advanced-scenarios-xaml-islands-cpp.md)

## <a name="samples"></a>Exemples

La façon dont vous utilisez l’API d’hébergement XAML UWP dans votre code dépend de votre type d’application, de la conception de votre application et d’autres facteurs. Pour illustrer l’utilisation de cette API dans le contexte d’une application complète, cet article fait référence au code des exemples suivants.

### <a name="c-win32"></a>C++32

Les exemples suivants montrent comment utiliser l’API d’hébergement XAML UWP dans une C++ application Win32 :

* [Exemple d’îlot XAML simple](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Standalone_Samples/CppWinRT_Basic_Win32App). Cet exemple illustre une implémentation de base de l’hébergement d’un contrôle UWP dans une C++ application Win32 non empaquetée (autrement dit, une application qui n’est pas intégrée à un package MSIX).

* [Îlot XAML avec exemple de contrôle personnalisé](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32). Cet exemple illustre une implémentation complète de l’hébergement d’un contrôle UWP personnalisé dans C++ une application Win32 empaquetée, ainsi que la gestion d’autres comportements tels que l’entrée au clavier et la navigation dans le focus.

### <a name="wpf-and-windows-forms"></a>WPF et Windows Forms

Le contrôle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) de la boîte à outils de la communauté Windows sert d’exemple de référence pour l’utilisation de l’API d’hébergement UWP dans WPF et les applications de Windows Forms. Le code source est disponible aux emplacements suivants :

* Pour la version WPF du contrôle, [cliquez ici](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost). La version WPF dérive de [System. Windows. Interop. HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost).

* Pour obtenir la version Windows Forms du contrôle, [cliquez ici](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost). La version Windows Forms dérive de [System. Windows. Forms. Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control).

> [!NOTE]
> Nous vous recommandons vivement d’utiliser les [contrôles .net Island](xaml-islands.md#wpf-and-windows-forms-applications) dans le kit de pratiques de la communauté Windows au lieu d’utiliser directement l’API d’hébergement XAML UWP dans WPF et les applications de Windows Forms. Les exemples de liens WPF et Windows Forms dans cet article sont fournis à titre d’illustration uniquement.

## <a name="architecture-of-the-api"></a>Architecture de l’API

L’API d’hébergement XAML UWP comprend ces principaux types de Windows Runtime et interfaces COM.

|  Type ou interface | Description |
|--------------------|-------------|
| [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) | Cette classe représente l’infrastructure XAML UWP. Cette classe fournit une méthode [InitializeForCurrentThread](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) statique unique qui initialise l’infrastructure XAML UWP sur le thread actuel de l’application de bureau. |
| [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) | Cette classe représente une instance du contenu XAML UWP que vous hébergez dans votre application de bureau. Le membre le plus important de cette classe est la propriété de [contenu](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) . Vous assignez cette propriété à un [Windows. UI. Xaml. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) que vous souhaitez héberger. Cette classe a également d’autres membres pour le routage du focus clavier vers et à partir des îlots XAML. |
| IDesktopWindowXamlSourceNative | Cette interface COM fournit la méthode **AttachToWindow** , que vous utilisez pour attacher un îlot XAML dans votre application à un élément d’interface utilisateur parent. Chaque objet **DesktopWindowXamlSource** implémente cette interface. Cette interface est déclarée dans Windows. UI. Xaml. Hosting. desktopwindowxamlsource. h. |
| IDesktopWindowXamlSourceNative2 | Cette interface COM fournit la méthode **PreTranslateMessage** , qui permet à l’infrastructure XAML UWP de traiter correctement certains messages Windows. Chaque objet **DesktopWindowXamlSource** implémente cette interface. Cette interface est déclarée dans Windows. UI. Xaml. Hosting. desktopwindowxamlsource. h. |

Le diagramme suivant illustre la hiérarchie d’objets dans un îlot XAML hébergé dans une application de bureau.

* Au niveau de la base se trouve l’élément d’interface utilisateur dans votre application où vous souhaitez héberger l’îlot XAML. Cet élément d’interface utilisateur doit avoir un handle de fenêtre (HWND). Voici des exemples d’éléments d’interface utilisateur dans lesquels vous pouvez héberger [window](https://docs.microsoft.com/windows/desktop/winmsg/about-windows) un îlot C++ XAML : fenêtre pour les applications Win32, [System. Windows. Interop. HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) pour les applications WPF et [System. Windows. forms. Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) pour les applications Windows Forms.

* Au niveau suivant, il s’agit d’un objet **DesktopWindowXamlSource** . Cet objet fournit l’infrastructure pour l’hébergement de l’îlot XAML. Votre code est chargé de créer cet objet et de l’attacher à l’élément d’interface utilisateur parent.

* Lorsque vous créez un **DesktopWindowXamlSource**, cet objet crée automatiquement une fenêtre enfant native pour héberger votre contrôle UWP. Cette fenêtre enfant native est principalement abstraite de votre code, mais vous pouvez accéder à son handle (HWND) si nécessaire.

* Enfin, au niveau supérieur, il s’agit du contrôle UWP que vous souhaitez héberger dans votre application de bureau. Il peut s’agir de n’importe quel objet UWP dérivé de [Windows. UI. Xaml. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), y compris tout contrôle UWP fourni par le SDK Windows, ainsi que des contrôles utilisateur personnalisés.

![Architecture DesktopWindowXamlSource](images/xaml-islands/xaml-hosting-api-rev2.png)

> [!NOTE]
> Quand vous hébergez XAML Islands dans une application de bureau, plusieurs arborescences de contenu XAML peuvent s’exécuter simultanément sur le même thread. Pour accéder à l’élément racine d’une arborescence de contenu XAML dans un XAML Islands et obtenir des informations connexes sur le contexte de son hébergement, utilisez la classe [XamlRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.xamlroot). Les API [CoreWindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow), [ApplicationView](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview)et [Window](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window) ne fournissent pas les informations correctes pour les îlots XAML. Pour plus d'informations, consultez [cette section](xaml-islands.md#window-host-context-for-xaml-islands).

## <a name="troubleshooting"></a>Résolution des problèmes

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>Erreur lors de l’utilisation de l’API d’hébergement XAML UWP dans une application UWP

| Problème | Résolution |
|-------|------------|
| Votre application reçoit une **exception COMException** avec le message suivant : «impossible d’activer DesktopWindowXamlSource. Ce type ne peut pas être utilisé dans une application UWP.» ou «impossible d’activer WindowsXamlManager. Ce type ne peut pas être utilisé dans une application UWP.» | Cette erreur indique que vous essayez d’utiliser l’API d’hébergement XAML UWP (en particulier, vous essayez d’instancier les types [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) ou [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) ) dans une application UWP. L’API d’hébergement XAML UWP est uniquement destinée à être utilisée dans les applications de bureau non UWP, telles que WPF, les C++ Windows Forms et les applications Win32. |

### <a name="error-trying-to-use-the-windowsxamlmanager-or-desktopwindowxamlsource-types"></a>Erreur lors de la tentative d’utilisation des types WindowsXamlManager ou DesktopWindowXamlSource

| Problème | Résolution |
|-------|------------|
| Votre application reçoit une exception avec le message suivant : «WindowsXamlManager et DesktopWindowXamlSource sont pris en charge pour les applications ciblant Windows version 10.0.18226.0 et ultérieures. Vérifiez le manifeste de l’application ou le manifeste du package et assurez-vous que la propriété MaxTestedVersion est mise à jour.» | Cette erreur indique que votre application a essayé d’utiliser les types **WindowsXamlManager** ou **DESKTOPWINDOWXAMLSOURCE** dans l’API d’hébergement XAML UWP, mais que le système d’exploitation ne peut pas déterminer si l’application a été créée pour cibler Windows 10, version 1903 ou ultérieure. L’API d’hébergement XAML UWP a été introduite pour la première fois comme version préliminaire dans une version antérieure de Windows 10, mais elle n’est prise en charge qu’à partir de Windows 10, version 1903.</p></p>Pour résoudre ce problème, créez un package MSIX pour l’application et exécutez-le à partir du package, ou installez le package NuGet [Microsoft. Toolkit. Win32. UI. SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) dans votre projet.  |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>Erreur lors de l’attachement à une fenêtre sur un thread différent

| Problème | Résolution |
|-------|------------|
| Votre application reçoit une **exception COMException** avec le message suivant : « échec de la méthode AttachToWindow, car le HWND spécifié a été créé sur un thread différent ». | Cette erreur indique que votre application a appelé la méthode **IDesktopWindowXamlSourceNative :: AttachToWindow** et lui a passé le HWND d’une fenêtre qui a été créée sur un thread différent. Vous devez passer cette méthode au HWND d’une fenêtre qui a été créée sur le même thread que le code à partir duquel vous appelez la méthode. |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>Erreur lors de l’attachement à une fenêtre dans une fenêtre de niveau supérieur différente

| Problème | Résolution |
|-------|------------|
| Votre application reçoit une **COMException** avec le message suivant : « échec de la méthode AttachToWindow, car le HWND spécifié descend d’une fenêtre de niveau supérieur à celle du HWND précédemment passé à AttachToWindow sur le même thread. » | Cette erreur indique que votre application a appelé la méthode **IDesktopWindowXamlSourceNative :: AttachToWindow** et lui a passé le HWND d’une fenêtre qui descend d’une fenêtre de niveau supérieur différente d’une fenêtre que vous avez spécifiée dans un appel précédent à cette méthode sur le même thread.</p></p>Une fois que votre application a appelé **AttachToWindow** sur un thread particulier, tous les autres objets [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) sur le même thread peuvent uniquement être attachés à des fenêtres qui sont des descendants de la même fenêtre de niveau supérieur qui a été passée dans le premier appel à **AttachToWindow**. Lorsque tous les objets **DesktopWindowXamlSource** sont fermés pour un thread particulier, le **DesktopWindowXamlSource** suivant est ensuite libre de s’attacher à une nouvelle fenêtre.</p></p>Pour résoudre ce problème, fermez tous les objets **DesktopWindowXamlSource** liés à d’autres fenêtres de niveau supérieur sur ce thread, ou créez un nouveau thread pour ce **DesktopWindowXamlSource**. |

## <a name="related-topics"></a>Rubriques connexes

* [Héberger des contrôles XAML UWP dans les applications de bureau (îlots XAML)](xaml-islands.md)
* [Héberger un contrôle UWP standard dans C++ une application Win32](host-standard-control-with-xaml-islands-cpp.md)
* [Héberger un contrôle UWP personnalisé dans C++ une application Win32](host-custom-control-with-xaml-islands-cpp.md)
* [Scénarios avancés pour les îlots C++ XAML dans les applications Win32](advanced-scenarios-xaml-islands-cpp.md)
* [Exemples de code des îlots XAML](https://github.com/microsoft/Xaml-Islands-Samples)
* [C++Exemple d’îlots XAML Win32](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)
