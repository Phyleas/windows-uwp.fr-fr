---
description: Cet article présente des scénarios d’hébergement XAML Islands avancés pour les applications Win32 C++.
title: Scénarios avancés pour XAML Islands dans les applications Win32 C++
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10, uwp, cpp, win32, xaml islands, contrôles wrappés, contrôles standard
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 50ee005fc0de52a3e0217a71fb3d391445c486db
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80226233"
---
# <a name="advanced-scenarios-for-xaml-islands-in-c-win32-apps"></a>Scénarios avancés pour XAML Islands dans les applications Win32 C++

Les articles [Héberger un contrôle UWP standard](host-standard-control-with-xaml-islands-cpp.md) et [Héberger un contrôle UWP personnalisé](host-custom-control-with-xaml-islands-cpp.md) fournissent des instructions et des exemples d’hébergement XAML Islands dans une application Win32 C++. Toutefois, les exemples de codes de ces articles n’abordent pas de nombreux scénarios avancés que les applications de bureau peuvent avoir à gérer pour offrir une expérience utilisateur fluide. Cet article fournit des conseils sur certains de ces scénarios ainsi que des pointeurs vers des exemples de code connexes.

## <a name="keyboard-input"></a>Saisie au clavier

Pour gérer correctement la saisie au clavier pour chaque élément XAML Island, votre application doit transmettre tous les messages Windows à l’infrastructure XAML UWP afin que certains messages puissent être traités correctement. Pour cela, à certains endroits de votre application qui peuvent accéder à la boucle de message, convertissez l’objet **DesktopWindowXamlSource** pour chaque élément XAML Island en une interface COM **IDesktopWindowXamlSourceNative2**. Appelez ensuite la méthode **PreTranslateMessage** de cette interface et transmettez le message actuel.

  * **C++ Win32:**  : L’application peut appeler **PreTranslateMessage** directement dans sa boucle de message principale. Pour obtenir un exemple, consultez le fichier [XamlBridge.cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp#L16).

  * **WPF :** L’application peut appeler **PreTranslateMessage** à partir du gestionnaire d’événements pour l’événement [ComponentDispatcher.ThreadFilterMessage](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage). Pour obtenir un exemple, consultez le fichier [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177) dans le Kit de ressources Communauté Windows.

  * **Windows Forms :** L’application peut appeler **PreTranslateMessage** à partir d’un remplacement de la méthode [Control.PreprocessMessage](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.preprocessmessage). Pour obtenir un exemple, consultez le fichier [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100) dans le Kit de ressources Communauté Windows.

## <a name="keyboard-focus-navigation"></a>Navigation au clavier en mode focus

Lorsque l’utilisateur parcourt les éléments d’interface utilisateur de votre application à l’aide du clavier (par exemple, en appuyant sur la touche **Tab** ou la touche direction/flèche), vous devez placer par programmation le focus dans et hors de l’objet **DesktopWindowXamlSource**. Lorsque la navigation au clavier de l’utilisateur atteint l’objet **DesktopWindowXamlSource**, placez le focus sur le premier objet [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) dans l’ordre de navigation de votre interface utilisateur, continuez à placer le focus sur les objets **Windows.UI.Xaml.UIElement** suivants lorsque l’utilisateur parcourt les éléments, puis placez le focus hors de l’objet **DesktopWindowXamlSource** et dans l’élément d’interface utilisateur parent.  

L’API d’hébergement XAML UWP fournit plusieurs types et membres pour vous aider à accomplir ces tâches.

* Quand la navigation au clavier atteint votre objet **DesktopWindowXamlSource**, l’événement [GotFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) est déclenché. Gérez cet événement et placez par programmation le focus sur le premier objet **Windows.UI.Xaml.UIElement** hébergé à l’aide de la méthode [NavigateFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus).

* Lorsque l’utilisateur se trouve sur le dernier élément pouvant être actif dans votre objet **DesktopWindowXamlSource** et appuie sur la touche **Tab** ou sur une touche de direction, l’événement [TakeFocusRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) est déclenché. Gérez cet événement et placez par programmation le focus sur le prochain élément pouvant être actif dans l’application hôte. Par exemple, dans une application WPF où **DesktopWindowXamlSource** est hébergé dans un objet [System.Windows.Interop.HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost), vous pouvez utiliser la méthode [MoveFocus](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) pour déplacer le focus vers le prochain élément pouvant être actif dans l’application hôte.

Pour obtenir des exemples montrant comment effectuer cette opération dans le contexte d’un exemple d’application fonctionnel, consultez les fichiers de code suivants :

  * **C++/Win32** : Consultez le fichier [XamlBridge.cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp).

  * **WPF :** Consultez le fichier [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) dans le Kit de ressources Communauté Windows.  

  * **Windows Forms :** Consultez le fichier [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) dans le Kit de ressources Communauté Windows.

## <a name="handle-layout-changes"></a>Gérer les modifications de disposition

Lorsque l’utilisateur modifie la taille de l’élément d’interface utilisateur parent, vous devez gérer les modifications de disposition nécessaires pour vous assurer que vos contrôles UWP s’affichent comme prévu. Voici quelques scénarios importants à prendre en compte.

* Dans une application Win32 C++, lorsque votre application gère le message WM_SIZE, elle peut repositionner l’élément XAML Island hébergé à l’aide de la fonction [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos). Pour obtenir un exemple, consultez le fichier de code [SampleApp.cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/SampleApp.cpp#L170).

* Lorsque l’élément d’interface utilisateur parent doit obtenir la taille de la zone rectangulaire nécessaire pour ajuster l’objet **Windows.UI.Xaml.UIElement** que vous hébergez sur **DesktopWindowXamlSource**, appelez la méthode [Measure](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) de **Windows.UI.Xaml.UIElement**. Par exemple :

    * Dans une application WPF, vous pouvez effectuer cette opération à partir de la méthode [MeasureOverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) de l’objet [HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) qui héberge **DesktopWindowXamlSource**. Pour obtenir un exemple, consultez le fichier [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) dans le Kit de ressources Communauté Windows.

    * Dans une application Windows Forms, vous pouvez effectuer cette opération à partir de la méthode [GetPreferredSize](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) de l’objet [Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) qui héberge **DesktopWindowXamlSource**. Pour obtenir un exemple, consultez le fichier [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) dans le Kit de ressources Communauté Windows.

* Lorsque la taille de l’élément d’interface utilisateur parent change, appelez la méthode [Arrange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) de l’objet **Windows.UI.Xaml.UIElement** que vous hébergez sur **DesktopWindowXamlSource**. Par exemple :

    * Dans une application WPF, vous pouvez effectuer cette opération à partir de la méthode [ArrangeOverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) de l’objet [HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) qui héberge **DesktopWindowXamlSource**. Pour obtenir un exemple, consultez le fichier [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) dans le Kit de ressources Communauté Windows.

    * Dans une application Windows Forms, vous pouvez effectuer cette opération à partir du gestionnaire pour l’événement [SizeChanged](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) de l’objet [Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) qui héberge **DesktopWindowXamlSource**. Pour obtenir un exemple, consultez le fichier [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) dans le Kit de ressources Communauté Windows.

## <a name="handle-dpi-changes"></a>Gérer les changements de résolutions (DPI)

L’infrastructure XAML UWP gère automatiquement les changements de DPI pour les contrôles UWP hébergés (par exemple, lorsque l’utilisateur fait glisser la fenêtre entre des moniteurs affichant des résolutions DPI différentes). Pour une expérience optimale, nous recommandons de configurer votre application Windows Forms, WPF ou Win32 C++ pour prendre en charge la résolution par moniteur.

Pour configurer votre application afin de prendre charge la résolution par moniteur, ajoutez un [manifeste d’assembly côte à côte](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) à votre projet, puis définissez l’élément **\<dpiAwareness\>** sur **PerMonitorV2**. Pour plus d’informations sur cette valeur, consultez la description de [DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context).

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

* [Héberger des contrôles XAML UWP dans des applications de bureau (îlots XAML)](xaml-islands.md)
* [Utilisation de l’API d’hébergement XAML UWP dans une application Win32 C++](using-the-xaml-hosting-api.md)
* [Héberger un contrôle UWP standard dans une application Win32 C++](host-standard-control-with-xaml-islands-cpp.md)
* [Héberger un contrôle UWP personnalisé dans une application Win32 C++](host-custom-control-with-xaml-islands-cpp.md)
* [Exemples de code XAML Islands](https://github.com/microsoft/Xaml-Islands-Samples)
* [Exemple d’îlots XAML Win32 C++](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)
