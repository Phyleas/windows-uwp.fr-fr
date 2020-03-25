---
description: Cet article explique comment héberger un contrôle UWP standard dans une C++ application Win32 à l’aide de l’API d’hébergement XAML.
title: Héberger un contrôle UWP standard dans C++ une application Win32 à l’aide des ÎLOTs XAML
ms.date: 03/23/2020
ms.topic: article
keywords: Windows 10, UWP, CPP, Win32, îlots XAML, contrôles encapsulés, contrôles standard
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 08308c7bca3cd7f39b08c836e43d791a3fda048f
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226273"
---
# <a name="host-a-standard-uwp-control-in-a-c-win32-app"></a>Héberger un contrôle UWP standard dans C++ une application Win32

Cet article explique comment utiliser l' [API d’hébergement XAML UWP](using-the-xaml-hosting-api.md) pour héberger un contrôle UWP standard (autrement dit, un contrôle fournis par le SDK Windows) dans une C++ nouvelle application Win32. Le code est basé sur l' [exemple d’îlot XAML simple](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Standalone_Samples/CppWinRT_Basic_Win32App), et cette section décrit quelques-unes des parties les plus importantes du code. Si vous disposez déjà C++ d’un projet d’application Win32, vous pouvez adapter ces étapes et exemples de code pour votre projet.

> [!NOTE]
> Le scénario illustré dans cet article ne prend pas en charge la modification directe du balisage XAML pour les contrôles UWP hébergés dans votre application. Ce scénario vous limite à modifier l’apparence et le comportement des contrôles UWP hébergés via du code. Pour obtenir des instructions qui vous permettent de modifier directement le balisage XAML lors de l’hébergement de contrôles UWP, consultez [héberger un contrôle UWP personnalisé dans une C++ application Win32](host-custom-control-with-xaml-islands-cpp.md).

## <a name="create-a-desktop-application-project"></a>Créer un projet d’application de bureau

1. Dans Visual Studio 2019 avec le kit de développement logiciel (SDK) Windows 10, version 1903 (version 10.0.18362) ou une version ultérieure installée, créez un nouveau projet d' **application de bureau Windows** et nommez-le **MyDesktopWin32App**. Ce type de projet est disponible sous **C++** les filtres de projet, **Windows**et de **Bureau** .

2. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de la solution, cliquez sur **recibler la solution**, sélectionnez le **10.0.18362.0** ou une version ultérieure du kit de développement logiciel (SDK), puis cliquez sur **OK**.

3. Installez le package NuGet [Microsoft. Windows. CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) pour activer la prise en charge de [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis) dans votre projet :

    1. Dans **Explorateur de solutions** , cliquez avec le bouton droit sur votre projet et choisissez **gérer les packages NuGet**.
    2. Sélectionnez l’onglet **Parcourir** , recherchez le package [Microsoft. Windows. CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) et installez la dernière version de ce package.

    > [!NOTE]
    > Pour les nouveaux projets, vous pouvez également installer l' [ C++extension Visual Studio/WinRT (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) et utiliser l’un des C++modèles de projet/WinRT inclus dans cette extension. Pour plus d’informations, consultez [cet article](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

4. Installez le package NuGet [Microsoft. Toolkit. Win32. UI. SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) :

    1. Dans la fenêtre **Gestionnaire de package NuGet** , assurez-vous que l’option **inclure la version préliminaire** est sélectionnée.
    2. Sélectionnez l’onglet **Parcourir** , recherchez le package **Microsoft. Toolkit. Win32. UI. SDK** et installez la version v 6.0.0 (ou ultérieure) de ce package. Ce package fournit plusieurs ressources de génération et d’exécution qui permettent aux îlots XAML de fonctionner dans votre application.

5. Définissez la valeur `maxVersionTested` dans votre [manifeste d’application](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) pour spécifier que votre application est compatible avec Windows 10, version 1903 ou ultérieure.

    1. Si vous ne disposez pas déjà d’un manifeste d’application dans votre projet, ajoutez un nouveau fichier XML à votre projet et nommez-le **app. manifest**.
    2. Dans votre manifeste d’application, incluez l’élément **Compatibility** et les éléments enfants indiqués dans l’exemple suivant. Remplacez l’attribut **ID** de l’élément **maxVersionTested** par le numéro de version de Windows 10 que vous ciblez (il doit s’agir de windows 10, version 1903 ou version ultérieure).

        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
            <compatibility xmlns="urn:schemas-microsoft-com:compatibility.v1">
                <application>
                    <!-- Windows 10 -->
                    <maxversiontested Id="10.0.18362.0"/>
                    <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
                </application>
            </compatibility>
        </assembly>
        ```

## <a name="use-the-xaml-hosting-api-to-host-a-uwp-control"></a>Utiliser l’API d’hébergement XAML pour héberger un contrôle UWP

Le processus de base de l’utilisation de l’API d’hébergement XAML pour héberger un contrôle UWP suit ces étapes générales :

1. Initialisez l’infrastructure XAML UWP pour le thread actuel avant que votre application ne crée l’un des objets [Windows. UI. Xaml. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) qu’il hébergera. Il existe plusieurs façons de procéder, selon le moment où vous envisagez de créer l’objet [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) qui hébergera les contrôles.

    * Si votre application crée l’objet **DesktopWindowXamlSource** avant de créer l’un des objets **Windows. UI. Xaml. UIElement** qu’il hébergera, cette infrastructure sera initialisée pour vous lorsque vous instancierez l’objet **DesktopWindowXamlSource** . Dans ce scénario, vous n’avez pas besoin d’ajouter le code de votre choix pour initialiser le Framework.

    * Toutefois, si votre application crée les objets **Windows. UI. Xaml. UIElement** avant de créer l’objet **DesktopWindowXamlSource** qui va les héberger, votre application doit appeler la méthode statique [WindowsXamlManager. InitializeForCurrentThread](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) pour initialiser explicitement l’infrastructure XAML UWP avant l’instanciation des objets **Windows. UI. Xaml. UIElement** . Votre application doit généralement appeler cette méthode lorsque l’élément d’interface utilisateur parent qui héberge le **DesktopWindowXamlSource** est instancié.

    > [!NOTE]
    > Cette méthode retourne un objet [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) qui contient une référence à l’infrastructure XAML UWP. Vous pouvez créer autant d’objets **WindowsXamlManager** que vous le souhaitez sur un thread donné. Toutefois, étant donné que chaque objet contient une référence à l’infrastructure XAML UWP, vous devez supprimer les objets pour vous assurer que les ressources XAML sont libérées.

2. Créez un objet [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) et attachez-le à un élément d’interface utilisateur parent dans votre application associée à un handle de fenêtre.

    Pour ce faire, vous devez suivre les étapes ci-dessous :

    1. Créez un objet **DesktopWindowXamlSource** et effectuez un cast de celui-ci en interface com **IDesktopWindowXamlSourceNative** ou **IDesktopWindowXamlSourceNative2** .
        > [!NOTE]
        > Ces interfaces sont déclarées dans le fichier d’en-tête **Windows. UI. Xaml. Hosting. desktopwindowxamlsource. h** dans le SDK Windows. Par défaut, ce fichier se trouve dans% ProgramFiles (x86)% \ Windows Kits\10\Include\\< numéro de build\>\um.

    2. Appelez la méthode **AttachToWindow** de l’interface **IDesktopWindowXamlSourceNative** ou **IDesktopWindowXamlSourceNative2** , puis transmettez le handle de fenêtre de l’élément d’interface utilisateur parent dans votre application.

    3. Définissez la taille initiale de la fenêtre enfant interne contenue dans le **DesktopWindowXamlSource**. Par défaut, cette fenêtre enfant interne est définie sur une largeur et une hauteur de 0. Si vous ne définissez pas la taille de la fenêtre, les contrôles UWP que vous ajoutez au **DesktopWindowXamlSource** ne seront pas visibles. Pour accéder à la fenêtre enfant interne dans **DesktopWindowXamlSource**, utilisez la propriété **WindowHandle** de l’interface **IDesktopWindowXamlSourceNative** ou **IDesktopWindowXamlSourceNative2** .

3. Enfin, assignez le **Windows. UI. Xaml. UIElement** que vous souhaitez héberger à la propriété de [contenu](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) de votre objet **DesktopWindowXamlSource** .

Les étapes et les exemples de code suivants montrent comment implémenter le processus ci-dessus :

1. Dans le dossier **fichiers sources** du projet, ouvrez le fichier **MyDesktopWin32App. cpp** par défaut. Supprimez tout le contenu du fichier et ajoutez les instructions `include` et `using` suivantes. En plus des en C++ -têtes et des espaces de noms standard et UWP, ces instructions incluent plusieurs éléments spécifiques aux îlots XAML.

    ```cppwinrt
    #include <windows.h>
    #include <stdlib.h>
    #include <string.h>

    #include <winrt/Windows.Foundation.Collections.h>
    #include <winrt/Windows.system.h>
    #include <winrt/windows.ui.xaml.hosting.h>
    #include <windows.ui.xaml.hosting.desktopwindowxamlsource.h>
    #include <winrt/windows.ui.xaml.controls.h>
    #include <winrt/Windows.ui.xaml.media.h>

    using namespace winrt;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Xaml::Hosting;
    using namespace Windows::Foundation::Numerics;
    ```

3. Copiez le code suivant après la section précédente. Ce code définit la fonction **WinMain** pour l’application. Cette fonction Initialise une fenêtre de base et utilise l’API d’hébergement XAML pour héberger un contrôle **TextBlock TextBlock** simple dans la fenêtre.

    ```cppwinrt
    LRESULT CALLBACK WindowProc(HWND, UINT, WPARAM, LPARAM);

    HWND _hWnd;
    HWND _childhWnd;
    HINSTANCE _hInstance;

    int CALLBACK WinMain(_In_ HINSTANCE hInstance, _In_opt_ HINSTANCE hPrevInstance, _In_ LPSTR lpCmdLine, _In_ int nCmdShow)
    {
        _hInstance = hInstance;

        // The main window class name.
        const wchar_t szWindowClass[] = L"Win32DesktopApp";
        WNDCLASSEX windowClass = { };

        windowClass.cbSize = sizeof(WNDCLASSEX);
        windowClass.lpfnWndProc = WindowProc;
        windowClass.hInstance = hInstance;
        windowClass.lpszClassName = szWindowClass;
        windowClass.hbrBackground = (HBRUSH)(COLOR_WINDOW + 1);

        windowClass.hIconSm = LoadIcon(windowClass.hInstance, IDI_APPLICATION);

        if (RegisterClassEx(&windowClass) == NULL)
        {
            MessageBox(NULL, L"Windows registration failed!", L"Error", NULL);
            return 0;
        }

        _hWnd = CreateWindow(
            szWindowClass,
            L"Windows c++ Win32 Desktop App",
            WS_OVERLAPPEDWINDOW | WS_VISIBLE,
            CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT,
            NULL,
            NULL,
            hInstance,
            NULL
        );
        if (_hWnd == NULL)
        {
            MessageBox(NULL, L"Call to CreateWindow failed!", L"Error", NULL);
            return 0;
        }


        // Begin XAML Island section.

        // The call to winrt::init_apartment initializes COM; by default, in a multithreaded apartment.
        winrt::init_apartment(apartment_type::single_threaded);

        // Initialize the XAML framework's core window for the current thread.
        WindowsXamlManager winxamlmanager = WindowsXamlManager::InitializeForCurrentThread();

        // This DesktopWindowXamlSource is the object that enables a non-UWP desktop application 
        // to host UWP controls in any UI element that is associated with a window handle (HWND).
        DesktopWindowXamlSource desktopSource;

        // Get handle to the core window.
        auto interop = desktopSource.as<IDesktopWindowXamlSourceNative>();

        // Parent the DesktopWindowXamlSource object to the current window.
        check_hresult(interop->AttachToWindow(_hWnd));

        // This HWND will be the window handler for the XAML Island: A child window that contains XAML.  
        HWND hWndXamlIsland = nullptr;

        // Get the new child window's HWND. 
        interop->get_WindowHandle(&hWndXamlIsland);

        // Update the XAML Island window size because initially it is 0,0.
        SetWindowPos(hWndXamlIsland, 0, 200, 100, 800, 200, SWP_SHOWWINDOW);

        // Create the XAML content.
        Windows::UI::Xaml::Controls::StackPanel xamlContainer;
        xamlContainer.Background(Windows::UI::Xaml::Media::SolidColorBrush{ Windows::UI::Colors::LightGray() });

        Windows::UI::Xaml::Controls::TextBlock tb;
        tb.Text(L"Hello World from Xaml Islands!");
        tb.VerticalAlignment(Windows::UI::Xaml::VerticalAlignment::Center);
        tb.HorizontalAlignment(Windows::UI::Xaml::HorizontalAlignment::Center);
        tb.FontSize(48);

        xamlContainer.Children().Append(tb);
        xamlContainer.UpdateLayout();
        desktopSource.Content(xamlContainer);

        // End XAML Island section.

        ShowWindow(_hWnd, nCmdShow);
        UpdateWindow(_hWnd);

        //Message loop:
        MSG msg = { };
        while (GetMessage(&msg, NULL, 0, 0))
        {
            TranslateMessage(&msg);
            DispatchMessage(&msg);
        }

        return 0;
    }
    ```

4. Copiez le code suivant après la section précédente. Ce code définit la [procédure de fenêtre](https://docs.microsoft.com/windows/win32/learnwin32/writing-the-window-procedure) pour la fenêtre.

    ```cppwinrt
    LRESULT CALLBACK WindowProc(HWND hWnd, UINT messageCode, WPARAM wParam, LPARAM lParam)
    {
        PAINTSTRUCT ps;
        HDC hdc;
        wchar_t greeting[] = L"Hello World in Win32!";
        RECT rcClient;

        switch (messageCode)
        {
            case WM_PAINT:
                if (hWnd == _hWnd)
                {
                    hdc = BeginPaint(hWnd, &ps);
                    TextOut(hdc, 300, 5, greeting, wcslen(greeting));
                    EndPaint(hWnd, &ps);
                }
                break;
            case WM_DESTROY:
                PostQuitMessage(0);
                break;

            // Create main window
            case WM_CREATE:
                _childhWnd = CreateWindowEx(0, L"ChildWClass", NULL, WS_CHILD | WS_BORDER, 0, 0, 0, 0, hWnd, NULL, _hInstance, NULL);
                return 0;

            // Main window changed size
            case WM_SIZE:
                // Get the dimensions of the main window's client
                // area, and enumerate the child windows. Pass the
                // dimensions to the child windows during enumeration.
                GetClientRect(hWnd, &rcClient);
                MoveWindow(_childhWnd, 200, 200, 400, 500, TRUE);
                ShowWindow(_childhWnd, SW_SHOW);

                return 0;

                // Process other messages.

            default:
                return DefWindowProc(hWnd, messageCode, wParam, lParam);
                break;
        }

        return 0;
    }
    ```

5. Enregistrez le fichier de code, puis générez et exécutez l’application. Vérifiez que vous voyez le contrôle de **TEXTBLOCK** UWP dans la fenêtre de l’application.
    > [!NOTE]
    > Vous pouvez voir les différents avertissements de build, y compris les `warning C4002:  too many arguments for function-like macro invocation 'GetCurrentTime'` et les `manifest authoring warning 81010002: Unrecognized Element "maxversiontested" in namespace "urn:schemas-microsoft-com:compatibility.v1"`. Ces avertissements sont des problèmes connus avec les outils actuels et les packages NuGet, et ils peuvent être ignorés.

Pour obtenir des exemples complets qui illustrent ces tâches, consultez les fichiers de code suivants :

* **C++32**
  * Consultez le fichier [HelloWindowsDesktop. cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Standalone_Samples/CppWinRT_Basic_Win32App/Win32DesktopApp/HelloWindowsDesktop.cpp) .
  * Consultez le fichier [XamlBridge. cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp) .
* **WPF :** Consultez les fichiers [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs) et [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) dans la boîte à outils de la communauté Windows.  
* **Windows Forms :** Consultez les fichiers [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs) et [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) dans la boîte à outils de la communauté Windows.

## <a name="package-the-app"></a>Empaqueter l’application

Vous pouvez éventuellement empaqueter l’application dans un [package MSIX](https://docs.microsoft.com/windows/msix) pour le déploiement. MSIX est la technologie d’empaquetage d’applications moderne pour Windows, qui est basée sur une combinaison de MSI,. AppX, App-V et des technologies d’installation ClickOnce.

Les instructions suivantes vous montrent comment empaqueter tous les composants de la solution dans un package MSIX à l’aide du [projet de packaging des applications Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) dans Visual Studio 2019. Ces étapes sont nécessaires uniquement si vous souhaitez empaqueter l’application dans un package MSIX.

> [!NOTE]
> Si vous choisissez de ne pas empaqueter votre application dans un [package MSIX](https://docs.microsoft.com/windows/msix) pour le déploiement, [Visual C++ Runtime](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) doit être installé sur les ordinateurs qui exécutent votre application.

1. Ajoutez un nouveau [projet d’empaquetage d’applications Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) à votre solution. Au fur et à mesure que vous créez le projet, sélectionnez **Windows 10, version 1903 (10,0 ; Build 18362)** pour la **version cible** et la **version minimale**.

2. Dans le projet d’empaquetage, cliquez avec le bouton droit sur le nœud **applications** et choisissez **Ajouter une référence**. Dans la liste des projets, sélectionnez le C++projet d’application de bureau/Win32 dans votre solution, puis cliquez sur **OK**.

3. Générez et exécutez le projet de Packaging. Vérifiez que l’application s’exécute et affiche les contrôles UWP comme prévu.

## <a name="next-steps"></a>Étapes suivantes :

Les exemples de code de cet article vous montrent comment utiliser le scénario de base pour héberger un contrôle UWP C++ standard dans une application Win32. Les sections suivantes présentent des scénarios supplémentaires que votre application peut avoir besoin de prendre en charge.

### <a name="host-a-custom-uwp-control"></a>Héberger un contrôle UWP personnalisé

Pour de nombreux scénarios, vous devrez peut-être héberger un contrôle XAML UWP personnalisé qui contient plusieurs contrôles qui fonctionnent ensemble. Le processus d’hébergement d’un contrôle UWP personnalisé (soit un contrôle que vous définissez vous-même ou un contrôle fourni par un tiers C++ ) dans une application Win32 est plus complexe que l’hébergement d’un contrôle standard et requiert du code supplémentaire.

Pour obtenir une procédure pas à pas complète, consultez [héberger un C++ contrôle UWP personnalisé dans une application Win32 à l’aide de l’API d’hébergement XAML](host-custom-control-with-xaml-islands-cpp.md).

### <a name="advanced-scenarios"></a>Scénarios avancés

De nombreuses applications de bureau qui hébergent des îlots XAML doivent gérer des scénarios supplémentaires pour offrir une expérience utilisateur fluide. Par exemple, les applications de bureau peuvent avoir besoin de gérer l’entrée au clavier dans les îlots XAML, de se concentrer sur la navigation entre les îlots XAML et d’autres éléments d’interface utilisateur et les modifications de disposition.

Pour plus d’informations sur la gestion de ces scénarios et des pointeurs vers les exemples de code associés, consultez [scénarios avancés pour les îlots XAML dans C++ les applications Win32](advanced-scenarios-xaml-islands-cpp.md).

## <a name="related-topics"></a>Rubriques connexes

* [Héberger des contrôles XAML UWP dans les applications de bureau (îlots XAML)](xaml-islands.md)
* [Utilisation de l’API d’hébergement XAML UWP C++ dans une application Win32](using-the-xaml-hosting-api.md)
* [Héberger un contrôle UWP personnalisé dans C++ une application Win32](host-custom-control-with-xaml-islands-cpp.md)
* [Scénarios avancés pour les îlots C++ XAML dans les applications Win32](advanced-scenarios-xaml-islands-cpp.md)
* [Exemples de code des îlots XAML](https://github.com/microsoft/Xaml-Islands-Samples)
