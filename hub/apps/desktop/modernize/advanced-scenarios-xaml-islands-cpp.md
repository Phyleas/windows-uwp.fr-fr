---
description: Cet article traite des scénarios d’hébergement d’îlot XAML C++ avancés pour les applications Win32.
title: Scénarios avancés pour les îlots C++ XAML dans les applications Win32
ms.date: 03/23/2020
ms.topic: article
keywords: Windows 10, UWP, CPP, Win32, îlots XAML, contrôles encapsulés, contrôles standard
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 50ee005fc0de52a3e0217a71fb3d391445c486db
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226233"
---
# <a name="advanced-scenarios-for-xaml-islands-in-c-win32-apps"></a>Scénarios avancés pour les îlots C++ XAML dans les applications Win32

L' [hôte a un contrôle UWP standard](host-standard-control-with-xaml-islands-cpp.md) et [héberger un contrôle UWP personnalisé](host-custom-control-with-xaml-islands-cpp.md) . fournit des instructions et des exemples pour l' C++ hébergement des îlots XAML dans une application Win32. Toutefois, les exemples de code de ces articles ne traitent pas de nombreux scénarios avancés que les applications de bureau peuvent avoir à gérer pour offrir une expérience utilisateur fluide. Cet article fournit des conseils sur certains de ces scénarios et des pointeurs vers des exemples de code connexes.

## <a name="keyboard-input"></a>Saisie au clavier

Pour gérer correctement l’entrée au clavier pour chaque îlot XAML, votre application doit passer tous les messages Windows à l’infrastructure XAML UWP afin que certains messages puissent être traités correctement. Pour ce faire, dans l’application qui peut accéder à la boucle de message, effectuez un cast de l’objet **DesktopWindowXamlSource** pour chaque îlot XAML en une interface com **IDesktopWindowXamlSourceNative2** . Ensuite, appelez la méthode **PreTranslateMessage** de cette interface et transmettez le message actuel.

  * Win32 :: l’application peut appeler **PreTranslateMessage** directement dans sa boucle de message principale. **C++** Pour obtenir un exemple, consultez le fichier [XamlBridge. cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp#L16) .

  * **WPF :** L’application peut appeler **PreTranslateMessage** à partir du gestionnaire d’événements pour l’événement [ComponentDispatcher. ThreadFilterMessage](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage) . Pour obtenir un exemple, consultez le fichier [WindowsXamlHostBase.focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177) dans le kit de pratiques de la communauté Windows.

  * **Windows Forms :** L’application peut appeler **PreTranslateMessage** à partir d’une substitution pour la méthode [Control. PreprocessMessage](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.preprocessmessage) . Pour obtenir un exemple, consultez le fichier [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100) dans le kit de pratiques de la communauté Windows.

## <a name="keyboard-focus-navigation"></a>Navigation au focus clavier

Lorsque l’utilisateur parcourt les éléments d’interface utilisateur de votre application à l’aide du clavier (par exemple, en appuyant sur la touche **Tab** ou la touche direction/flèche), vous devez déplacer le focus par programmation vers et à partir de l’objet **DesktopWindowXamlSource** . Lorsque la navigation au clavier de l’utilisateur atteint le **DesktopWindowXamlSource**, déplacez le focus dans le premier objet [Windows. UI. Xaml. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) dans l’ordre de navigation de votre interface utilisateur, continuez à déplacer le focus sur les objets **Windows. UI. Xaml. UIElement** suivants lorsque l’utilisateur parcourt les éléments, puis replacez le focus sur le **DesktopWindowXamlSource** et dans l’élément d’interface utilisateur parent.  

L’API d’hébergement XAML UWP fournit plusieurs types et membres pour vous aider à accomplir ces tâches.

* Lorsque la navigation au clavier entre dans votre **DesktopWindowXamlSource**, l’événement [GotFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) est déclenché. Gérer cet événement et déplacer par programmation le focus vers le premier **Windows. UI. Xaml. UIElement** hébergé à l’aide de la méthode [NavigateFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus) .

* Lorsque l’utilisateur se trouve sur le dernier élément pouvant être actif dans votre **DesktopWindowXamlSource** et appuie sur la touche **Tab** ou sur une touche de direction, l’événement [TakeFocusRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) est déclenché. Gère cet événement et déplace par programmation le focus vers l’élément pouvant être actif suivant dans l’application hôte. Par exemple, dans une application WPF où le **DesktopWindowXamlSource** est hébergé dans un [System. Windows. Interop. HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost), vous pouvez utiliser la méthode [MoveFocus](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) pour transférer le focus vers l’élément pouvant être actif suivant dans l’application hôte.

Pour obtenir des exemples qui montrent comment effectuer cette opération dans le contexte d’un exemple d’application fonctionnel, consultez les fichiers de code suivants :

  * /Win32 : consultez le fichier [XamlBridge. cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp) .  **C++**

  * **WPF :** Consultez le fichier [WindowsXamlHostBase.focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) dans la boîte à outils de la communauté Windows.  

  * **Windows Forms :** Consultez le fichier [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) dans la boîte à outils de la communauté Windows.

## <a name="handle-layout-changes"></a>Gérer les modifications de disposition

Lorsque l’utilisateur modifie la taille de l’élément d’interface utilisateur parent, vous devez gérer les modifications de disposition nécessaires pour vous assurer que vos contrôles UWP s’affichent comme prévu. Voici quelques scénarios importants à prendre en compte.

* Dans une C++ application Win32, lorsque votre application gère le WM_SIZE message, elle peut repositionner l’îlot XAML hébergé à l’aide de la fonction [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) . Pour obtenir un exemple, consultez le fichier de code [SampleApp. cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/SampleApp.cpp#L170) .

* Lorsque l’élément d’interface utilisateur parent doit connaître la taille de la zone rectangulaire nécessaire pour ajuster le **Windows. UI. Xaml. UIElement** que vous hébergez sur le **DesktopWindowXamlSource**, appelez la méthode [measure](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) de **Windows. UI. Xaml. UIElement**. Par exemple :

    * Dans une application WPF, vous pouvez effectuer cette opération à partir de la méthode [MeasureOverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) de la [HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) qui héberge **DesktopWindowXamlSource**. Pour obtenir un exemple, consultez le fichier [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) dans le kit de pratiques de la communauté Windows.

    * Dans une application Windows Forms, vous pouvez effectuer cette opération à partir de la méthode [GetPreferredSize](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) du [contrôle](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) qui héberge **DesktopWindowXamlSource**. Pour obtenir un exemple, consultez le fichier [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) dans le kit de pratiques de la communauté Windows.

* Lorsque la taille de l’élément d’interface utilisateur parent change, appelez la méthode [arrange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) de la racine **Windows. UI. Xaml. UIElement** que vous hébergez sur le **DesktopWindowXamlSource**. Par exemple :

    * Dans une application WPF, vous pouvez effectuer cette opération à partir de la méthode [ArrangeOverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) de l’objet [HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) qui héberge **DesktopWindowXamlSource**. Pour obtenir un exemple, consultez le fichier [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) dans le kit de pratiques de la communauté Windows.

    * Dans une application Windows Forms, vous pouvez effectuer cette opération à partir du gestionnaire de l’événement [SizeChanged](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) du [contrôle](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) qui héberge **DesktopWindowXamlSource**. Pour obtenir un exemple, consultez le fichier [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) dans le kit de pratiques de la communauté Windows.

## <a name="handle-dpi-changes"></a>Gérer les modifications PPP

L’infrastructure XAML UWP gère automatiquement les modifications DPI pour les contrôles UWP hébergés (par exemple, lorsque l’utilisateur fait glisser la fenêtre entre les moniteurs avec différentes résolutions d’écran). Pour une expérience optimale, nous recommandons que votre application Windows Forms, WPF ou C++ Win32 soit configurée pour prendre en charge la résolution par moniteur.

Pour configurer votre application pour qu’elle prenne en charge la résolution par moniteur, ajoutez un [manifeste d’assembly côte à côte](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) à votre projet et définissez l’élément **\<DpiAwareness\>** sur **PerMonitorV2**. Pour plus d’informations sur cette valeur, consultez la description de [DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context).

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
        <windowsSettings>
            <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2</dpiAwareness>
        </windowsSettings>
    </application>
</assembly>
```

## <a name="related-topics"></a>Rubriques connexes

* [Héberger des contrôles XAML UWP dans les applications de bureau (îlots XAML)](xaml-islands.md)
* [Utilisation de l’API d’hébergement XAML UWP C++ dans une application Win32](using-the-xaml-hosting-api.md)
* [Héberger un contrôle UWP standard dans C++ une application Win32](host-standard-control-with-xaml-islands-cpp.md)
* [Héberger un contrôle UWP personnalisé dans C++ une application Win32](host-custom-control-with-xaml-islands-cpp.md)
* [Exemples de code des îlots XAML](https://github.com/microsoft/Xaml-Islands-Samples)
* [C++Exemple d’îlots XAML Win32](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)
