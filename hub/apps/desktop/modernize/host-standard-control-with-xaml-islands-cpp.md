---
description: Cet article explique comment héberger un contrôle XAML WinRT standard dans une application Win32 C++ à l’aide de l’API d’hébergement XAML.
title: Héberger un contrôle XAML WinRT standard dans une application Win32 C++ à l’aide de XAML Islands
ms.date: 10/02/2020
ms.topic: article
keywords: windows 10, uwp, cpp, win32, îlots xaml, contrôles wrappés, contrôles standard
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: ccd5efd5270ed12d17992f53b3c9ee50feddec4b
ms.sourcegitcommit: 6b64741cba279ac17f23f07baaf4a92a2696e8e1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/15/2020
ms.locfileid: "97502879"
---
# <a name="host-a-standard-winrt-xaml-control-in-a-c-win32-app"></a>Héberger un contrôle XAML WinRT standard dans une application Win32 C++

Cet article explique comment utiliser l’[API d’hébergement XAML UWP](using-the-xaml-hosting-api.md) pour héberger un contrôle XAML WinRT standard (c’est-à-dire un contrôle fourni par le SDK Windows) dans une nouvelle application Win32 C++. Le code est basé sur l’[exemple d’îlot XAML simple](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Standalone_Samples/CppWinRT_Basic_Win32App), et cette section présente certaines des parties les plus importantes du code. Si vous disposez déjà d’un projet d’application Win32 C++, vous pouvez adapter ces étapes et exemples de code à votre projet.

> [!NOTE]
> Le scénario présenté dans cet article ne prend pas en charge l’édition directe du balisage XAML pour les contrôles XAML WinRT hébergés dans votre application. Ce scénario vous limite à modifier l’apparence et le comportement des contrôles hébergés via du code. Pour obtenir des instructions qui vous permettent de modifier directement le balisage XAML lors de l’hébergement de contrôles XAML WinRT, consultez [Héberger un contrôle XAML WinRT personnalisé dans une application Win32 C++](host-custom-control-with-xaml-islands-cpp.md).

## <a name="create-a-desktop-application-project"></a>Créer un projet d’application de bureau

1. Dans Visual Studio 2019 avec Windows SDK 10, version 1903 (version 10.0.18362) ou une version ultérieure installé, créez un projet d’**application de bureau Windows** que vous nommez **MyDesktopWin32App**. Ce type de projet est disponible sous les filtres de projet **C++** , **Windows** et **Bureau**.

2. Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de la solution, cliquez sur **Recibler la solution**, sélectionnez la version **10.0.18362.0** ou une version ultérieure du kit de développement logiciel (SDK), puis cliquez sur **OK**.

3. Installez le package NuGet [Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) pour activer la prise en charge de [C++/WinRT](/windows/uwp/cpp-and-winrt-apis) dans votre projet :

    1. Cliquez avec le bouton droit sur votre projet dans l’**Explorateur de solutions**, puis choisissez **Gérer les packages NuGet**.
    2. Sélectionnez l’onglet **Parcourir**, recherchez le package [Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/), puis installez la version la plus récente de celui-ci.

    > [!NOTE]
    > Pour les nouveaux projets, vous pouvez également installer l’[Extension Visual Studio (VSIX) C++/WinRT](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) et utiliser l’un des modèles de projet C++/WinRT inclus dans cette extension. Pour plus d’informations, consultez [cet article](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

4. Sous l’onglet **Parcourir** du **Gestionnaire de package NuGet**, recherchez le package NuGet [Microsoft.Toolkit.Win32.UI.SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) et installez la dernière version stable de ce package. Ce package fournit plusieurs ressources de génération et d’exécution qui permettent aux îlots XAML Islands de fonctionner dans votre application.

5. Définissez la valeur `maxVersionTested` dans votre [manifeste d’application](/windows/desktop/SbsCs/application-manifests) pour spécifier que votre application est compatible avec Windows 10, version 1903 ou ultérieure.

    1. Si n’avez pas encore de manifeste d’application dans votre projet, ajoutez un nouveau fichier XML à votre projet et nommez-le **app.manifest**.
    2. Dans votre manifeste d’application, incluez l’élément **compatibilité** et les éléments enfants indiqués dans l’exemple suivant. Remplacez l’attribut **Id** de l’élément **maxVersionTested** par le numéro de version de Windows 10 que vous ciblez (il doit s’agir de 10.0.18362 ou d’une version ultérieure).

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

6. Ajoutez une référence aux métadonnées de Windows Runtime :
   1. Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le nœud **Références** de votre projet et sélectionnez **Ajouter une référence**.
   2. Cliquez sur le bouton **Parcourir** en bas de la page et accédez au dossier UnionMetadata dans le chemin d’installation du kit SDK. Par défaut, le kit SDK sera installé sur `C:\Program Files (x86)\Windows Kits\10\UnionMetadata`. 
   3. Ensuite, sélectionnez le dossier nommé d’après la version de Windows que vous ciblez (par exemple, 10.0.18362.0) et à l’intérieur de ce dossier, choisissez le fichier `Windows.winmd`.
   4. Cliquez sur **OK** pour fermer la boîte de dialogue **Ajouter une référence**.

## <a name="use-the-xaml-hosting-api-to-host-a-winrt-xaml-control"></a>Utiliser l’API d’hébergement XAML pour héberger un contrôle XAML WinRT

Le processus de base de l’utilisation de l’API d’hébergement XAML pour héberger un contrôle XAML WinRT suit les grandes étapes suivantes :

1. Initialisez l’infrastructure XAML UWP pour le thread actuel avant que votre application crée l’un des objets [Windows.UI.Xaml.UIElement](/uwp/api/windows.ui.xaml.uielement) qu’elle hébergera. Il existe plusieurs façons de procéder, selon le moment où vous envisagez de créer l’objet [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) qui hébergera les contrôles.

    * Si votre application crée l’objet **DesktopWindowXamlSource** avant de créer l’un des objets **Windows.UI.Xaml.UIElement** qu’il hébergera, cette infrastructure sera initialisée pour vous lorsque vous instancierez l’objet **DesktopWindowXamlSource**. Dans ce scénario, vous n’avez pas besoin d’ajouter de code de votre cru pour initialiser l’infrastructure.

    * Toutefois, si votre application crée les objets **Windows.UI.Xaml.UIElement** avant de créer l’objet **DesktopWindowXamlSource** qui les hébergera, votre application doit appeler la méthode statique [WindowsXamlManager.InitializeForCurrentThread](/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) pour initialiser explicitement l’infrastructure XAML UWP avant l’instanciation des objets **Windows.UI.Xaml.UIElement**. Votre application doit généralement appeler cette méthode lors de l’instanciation de l’élément d’interface utilisateur parent hébergeant l’objet **DesktopWindowXamlSource**.

    > [!NOTE]
    > Cette méthode retourne un objet [WindowsXamlManager](/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) contenant une référence à l’infrastructure XAML UWP. Vous pouvez créer autant d’objets **WindowsXamlManager** que vous le souhaitez sur un thread donné. Toutefois, étant donné que chaque objet contient une référence à l’infrastructure XAML UWP, vous devez agencer les objets pour garantir que les ressources XAML seront finalement libérées.

2. Créez un objet [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) et attachez-le à un élément d’interface utilisateur parent de votre application qui est associé à un identificateur de fenêtre.

    Pour ce faire, vous devez procéder comme ci-dessous :

    1. Créez un objet **DesktopWindowXamlSource** et effectuez un cast de celui-ci vers l’interface COM **IDesktopWindowXamlSourceNative** ou **IDesktopWindowXamlSourceNative2**.
        > [!NOTE]
        > Ces interfaces sont déclarées dans le fichier d’en-tête **Windows.UI.Xaml.Hosting. desktopwindowxamlsource.h** dans le SDK Windows. Par défaut, ce fichier se trouve dans %programfiles(x86)%\Windows Kits\10\Include\\<build number\>\um.

    2. Appelez la méthode **AttachToWindow** de l’interface **IDesktopWindowXamlSourceNative** ou **IDesktopWindowXamlSourceNative2**, puis transmettez l’identificateur de fenêtre de l’élément d’interface utilisateur parent dans votre application.

    3. Définissez la taille initiale de la fenêtre enfant interne contenue dans l’objet **DesktopWindowXamlSource**. Par défaut, cette fenêtre enfant interne a une largeur et une hauteur de 0. Si vous ne définissez pas la taille de la fenêtre, les contrôles XAML WinRT que vous ajoutez à l’objet **DesktopWindowXamlSource** ne sont pas visibles. Pour accéder à la fenêtre enfant interne dans l’objet **DesktopWindowXamlSource**, utilisez la propriété **WindowHandle** de l’interface **IDesktopWindowXamlSourceNative** ou **IDesktopWindowXamlSourceNative2**.

3. Enfin, attribuez l’objet **Windows.UI.Xaml.UIElement** que vous souhaitez héberger à la propriété [Content](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) de votre objet **DesktopWindowXamlSource**.

Les étapes et les exemples de code suivants montrent comment implémenter le processus ci-dessus :

1. Dans le dossier **Fichiers sources** du projet, ouvrez le fichier **MyDesktopWin32App.cpp** par défaut. Supprimez tout le contenu du fichier et ajoutez les instructions `include` et `using` suivantes. En plus des en-têtes et espaces de noms C++ et UWP standard, ces instructions incluent plusieurs éléments spécifiques des îlots XAML.

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

3. Copiez le code suivant après la section précédente. Ce code définit la fonction **WinMain** pour l’application. Cette fonction Initialise une fenêtre de base et utilise l’API d’hébergement XAML pour héberger un simple contrôle **TextBlock** dans la fenêtre.

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
        // to host WinRT XAML controls in any UI element that is associated with a window handle (HWND).
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

4. Copiez le code suivant après la section précédente. Ce code définit la [procédure de fenêtre](/windows/win32/learnwin32/writing-the-window-procedure) pour la fenêtre.

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

5. Enregistrez le fichier de code, puis générez et exécutez l’application. Vérifiez que le contrôle **TextBlock** UWP apparaît dans la fenêtre de l’application.
    > [!NOTE]
    > Vous pouvez voir les quelques avertissements de génération, notamment `warning C4002:  too many arguments for function-like macro invocation 'GetCurrentTime'` et `manifest authoring warning 81010002: Unrecognized Element "maxversiontested" in namespace "urn:schemas-microsoft-com:compatibility.v1"`. Ces avertissements ont trait à des problèmes connus en lien avec les outils actuels et les packages NuGet. Vous pouvez les ignorer.

Pour obtenir des exemples complets qui illustrent l’utilisation de l’API d’hébergement XAML pour héberger un contrôle XAML WinRT, consultez les fichiers de code suivants :

* **C++ Win32 :** Consultez le fichier [XamlBridge.cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Standalone_Samples/Contoso/App/XamlBridge.cpp) dans le [dépôt d’exemples de code XAML Islands](https://github.com/microsoft/Xaml-Islands-Samples).
* **WPF :** Consultez les fichiers [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs) et [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) dans le Kit de ressources Communauté Windows.  
* **Windows Forms :** Consultez les fichiers [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs) et [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) dans le Kit de ressources Communauté Windows.

## <a name="package-the-app"></a>Empaqueter l’application

Vous pouvez empaqueter l’application dans un [package MSIX](/windows/msix) pour la déployer. MSIX est une technologie d’empaquetage moderne pour Windows, basée sur une combinaison des technologies d’installation MSI, .appx, App-V et ClickOnce.

Les instructions suivantes montrent comment empaqueter tous les composants de la solution dans un package MSIX en utilisant le [Projet de création de package d’application Windows](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) dans Visual Studio 2019. Ces étapes sont nécessaires uniquement si vous souhaitez empaqueter l’application dans un package MSIX.

> [!NOTE]
> Si vous choisissez de ne pas empaqueter votre application dans un [package MSIX](/windows/msix) pour la déployer, [Visual C++ Runtime](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) doit être installé sur les ordinateurs qui exécutent votre application.

1. Ajoutez un nouveau [projet d’empaquetage d’application Windows](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) à votre solution. À mesure que vous créez le projet, sélectionnez **Windows 10, version 1903 (10.0; Build 18362)** pour la **Version cible** et la **Version minimale**.

2. Dans le projet d’empaquetage, cliquez avec le bouton droit sur le nœud **Applications**, puis choisissez **Ajouter une référence**. Dans la liste des projets, sélectionnez le projet d’application de bureau C++/Win32 dans votre solution, puis cliquez sur **OK**.

3. Générez et exécutez le projet d’empaquetage. Vérifiez que l’application s’exécute et affiche les contrôles XAML WinRT comme prévu.

## <a name="next-steps"></a>Étapes suivantes

Les exemples de code de cet article montrent comment utiliser le scénario de base pour héberger un contrôle XAML WinRT standard dans une application Win32 C++. Les sections suivantes présentent d’autres scénarios que votre application pourrait devoir prendre en charge.

### <a name="host-a-custom-winrt-xaml-control"></a>Héberger un contrôle XAML WinRT personnalisé

Pour de nombreux scénarios, vous devrez peut-être héberger un contrôle XAML UWP personnalisé contenant plusieurs contrôles fonctionnant ensemble. Le processus d’hébergement d’un contrôle personnalisé (que vous définissez vous-même ou qui est fourni par un tiers) dans une application Win32 C++ est plus complexe que l’hébergement d’un contrôle standard, car il nécessite du code supplémentaire.

Pour obtenir la procédure pas à pas complète, consultez [Héberger un contrôle XAML WinRT personnalisé dans une application C++ Win32 à l’aide de l’API d’hébergement XAML](host-custom-control-with-xaml-islands-cpp.md).

### <a name="advanced-scenarios"></a>Scénarios avancés

De nombreuses applications de bureau qui hébergeant des îlots XAML doivent gérer des scénarios supplémentaires pour offrir une expérience utilisateur fluide. Par exemple, des applications de bureau peuvent devoir gérer la saisie au clavier dans des îlots XAML, la navigation en mode focus entre les îlots XAML et d’autres éléments d’interface utilisateur, et des changements de disposition.

Pour plus d’informations sur la gestion de ces scénarios et des pointeurs vers des exemples de code connexes, consultez [Scénarios avancés pour les îlots XAML dans les applications Win32 C++](advanced-scenarios-xaml-islands-cpp.md).

## <a name="related-topics"></a>Rubriques connexes

* [Héberger des contrôles XAML UWP dans des applications de bureau (îlots XAML)](xaml-islands.md)
* [Utilisation de l’API d’hébergement XAML UWP dans une application Win32 C++](using-the-xaml-hosting-api.md)
* [Héberger un contrôle XAML WinRT personnalisé dans une application Win32 C++](host-custom-control-with-xaml-islands-cpp.md)
* [Scénarios avancés pour îlots XAML dans les applications Win32 C++](advanced-scenarios-xaml-islands-cpp.md)
* [Exemples de code d’îlots XAML](https://github.com/microsoft/Xaml-Islands-Samples)
