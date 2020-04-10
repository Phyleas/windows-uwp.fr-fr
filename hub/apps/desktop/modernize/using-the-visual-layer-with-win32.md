---
title: Utilisation de la couche visuelle avec Win32
description: Utilisez la couche visuelle pour améliorer l’interface utilisateur de votre application de bureau Win32.
template: detail.hbs
ms.date: 03/18/2019
ms.topic: article
keywords: UWP, affichage, composition, Win32
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: c9b4ec38b0dd1f6eca3f43cfded74c6292c08100
ms.sourcegitcommit: d1c3e13de3da3f7dce878b3735ee53765d0df240
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66215188"
---
# <a name="using-the-visual-layer-with-win32"></a>Utilisation de la couche visuelle avec Win32

Vous pouvez utiliser des API de composition Windows Runtime (également appelées [couche visuelle](/windows/uwp/composition/visual-layer)) dans vos applications Windows Win32 afin de créer des expériences modernes qui s’activent pour les utilisateurs de Windows 10.

Le code complet de ce tutoriel est disponible sur GitHub : [Exemple Win32 HelloComposition](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition).

Les applications Windows universelles ont accès à l’espace de noms [Windows.UI.Composition](/uwp/api/windows.ui.composition), qui leur permet de contrôler avec précision la composition et l’affichage de leur interface utilisateur. Toutefois, cette API de composition n’est pas limitée aux applications UWP. Les applications de bureau Win32 peuvent tirer parti des systèmes de composition modernes dans UWP et Windows 10.

## <a name="prerequisites"></a>Prérequis

L’API d’hébergement UWP est régie par les conditions préalables suivantes.

- Nous partons du principe que vous êtes familiarisé avec le développement d’applications à l’aide de Win32 et d’UWP. Pour plus d’informations, voir :
  - [Bien démarrer avec Win32 et C++](/windows/desktop/learnwin32/learn-to-program-for-windows)
  - [Bien démarrer avec les applications Windows 10](/windows/uwp/get-started/)
  - [Améliorer votre application de bureau pour Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance)
- Windows 10, version 1803 ou ultérieure
- SDK Windows 10 17134 ou version ultérieure

## <a name="how-to-use-composition-apis-from-a-win32-desktop-application"></a>Guide pratique pour utiliser des API de composition à partir d’une application de bureau Win32

Dans ce tutoriel, vous allez créer une application Win32 C++ et y ajouter des éléments de composition UWP. L’objectif est de configurer correctement le projet, de créer le code d’interopérabilité et de dessiner des éléments simples à l’aide des API de composition Windows. L’application terminée ressemble à ceci.

![Interface utilisateur d’application en cours d’exécution](images/visual-layer-interop/win32-comp-interop-app-ui.png)

## <a name="create-a-c-win32-project-in-visual-studio"></a>Créer un projet C++ Win32 dans Visual Studio

La première étape consiste à créer le projet d’application Win32 dans Visual Studio.

Pour créer un projet d’application Win32 en C++ nommé _HelloComposition_ :

1. Ouvrez Visual Studio, puis sélectionnez **Fichier** > **Nouveau** > **Projet**.

    La boîte de dialogue **Nouveau projet** s’affiche.
1. Dans la catégorie **Installé**, développez le nœud **Visual C++** , puis sélectionnez **Bureau Windows**.
1. Sélectionnez le modèle **Application de bureau Windows**.
1. Entrez le nom _HelloComposition_, puis cliquez sur **OK**.

    Visual Studio crée le projet et ouvre l’éditeur du fichier d’application principal.

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Configurer le projet pour qu’il utilise des API d’exécution Windows

Pour utiliser des API Windows Runtime (WinRT) dans votre application Win32, nous utilisons C++/WinRT. Vous devez configurer votre projet Visual Studio pour ajouter la prise en charge de C++/WinRT.

(Pour plus d’informations, consultez [Bien démarrer avec C++/WinRT - Modifier un projet d’application de bureau Windows pour ajouter la prise en charge de C++/WinRT](/windows/uwp/cpp-and-winrt-apis/get-started.md#modify-a-windows-desktop-application-project-to-add-cwinrt-support)).

1. Dans le menu **Projet**, ouvrez les propriétés du projet (_Propriétés de HelloComposition_) et assurez-vous que les paramètres suivants sont définis sur les valeurs spécifiées :

    - Pour **Configuration**, sélectionnez _Toutes les configurations_. Pour **Plateforme**, sélectionnez _Toutes les plateformes_.
    - **Propriétés de la configuration** > **Général** > **Version du SDK Windows** = _10.0.17763.0_ ou supérieure

    ![Définir la version du SDK](images/visual-layer-interop/sdk-version.png)

    - **C/C++**  > **Langage** > **Norme du langage C++**  = _ISO C++ 17 Standard (/stf:c++17)_

    ![Définir la norme du langage](images/visual-layer-interop/language-standard.png)

    - **Éditeur de liens** > **Entrée** > **Dépendances supplémentaires** doit inclure « _windowsapp.lib_ ». Si cet élément n’est pas inclus dans la liste, ajoutez-le.

    ![Ajouter une dépendance dans l’éditeur de liens](images/visual-layer-interop/linker-dependencies.png)

1. Mettez à jour l’en-tête précompilé.

    - Renommez `stdafx.h` et `stdafx.cpp` en `pch.h` et `pch.cpp`, respectivement.
    - Affectez à la propriété de projet **C/C++**  > **En-têtes précompilés** > **Fichier d’en-tête précompilé** la valeur *pch.h*.
    - Recherchez `#include "stdafx.h"` et remplacez-le par `#include "pch.h"` dans tous les fichiers.

        (**Modifier** > **Rechercher et remplacer** > **Chercher dans les fichiers**)

        ![Rechercher et remplacer stdafx.h](images/visual-layer-interop/replace-stdafx.png)

    - Dans `pch.h`, incluez `winrt/base.h` et `unknwn.h`.

        ```cppwinrt
        // reference additional headers your program requires here
        #include <unknwn.h>
        #include <winrt/base.h>
        ```

Il est judicieux de générer le projet à ce stade pour vérifier qu’il n’y a pas d’erreurs avant de poursuivre.

## <a name="create-a-class-to-host-composition-elements"></a>Créer une classe pour héberger les éléments de la composition

Pour héberger le contenu que vous créez avec la couche visuelle, créez une classe (_CompositionHost_) afin de gérer l’interopérabilité et créer des éléments de composition. C’est là que vous effectuez la majeure partie de la configuration pour l’hébergement des API de composition, notamment :

- Obtention d’un [compositeur](/uwp/api/windows.ui.composition.compositor), qui crée et gère des objets dans l’espace de noms [Windows.UI.Composition](/uwp/api/windows.ui.composition)
- Création d’un [DispatcherQueueController](/uwp/api/windows.system.dispatcherqueuecontroller)/[DispatcherQueue](/uwp/api/windows.system.dispatcherqueue) pour gérer les tâches des API WinRT
- Création d’un [DesktopWindowTarget](/uwp/api/windows.ui.composition.desktop.desktopwindowtarget) et d’un conteneur de composition pour afficher les objets de composition

Nous convertissons cette classe en singleton afin d’éviter les problèmes de thread. Par exemple, vous ne pouvez créer qu’une seule file d’attente de répartiteur par thread. Ainsi, l’instanciation d’une seconde instance de CompositionHost sur le même thread entraînerait une erreur.

> [!TIP]
> Si nécessaire, consultez le code complet à la fin du tutoriel pour vous assurer que tout le code se trouve à la bonne place à mesure que vous parcourez le tutoriel.

1. Ajoutez un nouveau fichier de classe à votre projet.
    - Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet _HelloComposition_.
    - Dans le menu contextuel, sélectionnez **Ajouter** > **Classe...** .
    - Dans la boîte de dialogue **Ajouter une classe**, nommez la classe _CompositionHost.cs_, puis cliquez sur **Ajouter**.

1. Incluez les en-têtes et les instructions _using_ nécessaires pour l’interopérabilité de la composition.
    - Dans CompositionHost.h, ajoutez ces instructions _include_ au début du fichier.

    ```cppwinrt
    #pragma once
    #include <winrt/Windows.UI.Composition.Desktop.h>
    #include <windows.ui.composition.interop.h>
    #include <DispatcherQueue.h>
    ```

    - Dans CompositionHost.cpp, ajoutez ces instructions _using_ au début du fichier, après toutes les instructions _include_ éventuelles.

    ```cppwinrt
    using namespace winrt;
    using namespace Windows::System;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Composition::Desktop;
    using namespace Windows::Foundation::Numerics;
    ```

1. Modifiez la classe pour utiliser le modèle singleton.
    - Dans CompositionHost.h, rendez le constructeur privé.
    - Déclarez une méthode _GetInstance_ statique publique.

    ```cppwinrt
    class CompositionHost
    {
    public:
        ~CompositionHost();
        static CompositionHost* GetInstance();

    private:
        CompositionHost();
    };
    ```

    - Dans CompositionHost.cpp, ajoutez la définition de la méthode _GetInstance_.

    ```cppwinrt
    CompositionHost* CompositionHost::GetInstance()
    {
        static CompositionHost instance;
        return &instance;
    }
    ```

1. Dans CompositionHost.h, déclarez des variables de membre privé pour le compositeur, DispatcherQueueController et DesktopWindowTarget.

    ```cppwinrt
    winrt::Windows::UI::Composition::Compositor m_compositor{ nullptr };
    winrt::Windows::System::DispatcherQueueController m_dispatcherQueueController{ nullptr };
    winrt::Windows::UI::Composition::Desktop::DesktopWindowTarget m_target{ nullptr };
    ```

1. Ajoutez une méthode publique pour initialiser les objets d’interopérabilité de composition.
    > [!NOTE]
    > Dans _Initialize_, vous appelez les méthodes _EnsureDispatcherQueue_, _CreateDesktopWindowTarget_ et _CreateCompositionRoot_. Vous allez créer ces méthodes aux étapes suivantes.

    - Dans CompositionHost.h, déclarez une méthode publique nommée _Initialize_ qui prend un HWND comme argument.

    ```cppwinrt
    void Initialize(HWND hwnd);
    ```

    - Dans CompositionHost.cpp, ajoutez la définition de la méthode _Initialize_.

    ```cppwinrt
    void CompositionHost::Initialize(HWND hwnd)
    {
        EnsureDispatcherQueue();
        if (m_dispatcherQueueController) m_compositor = Compositor();

        CreateDesktopWindowTarget(hwnd);
        CreateCompositionRoot();
    }
    ```

1. Créez une file d’attente de répartiteur sur le thread qui utilisera Windows Composition.

    Un compositeur doit être créé sur un thread qui a une file d’attente de répartiteur. Cette méthode est donc appelée en premier pendant l’initialisation.

    - Dans CompositionHost.h, déclarez une méthode privée nommée _EnsureDispatcherQueue_.

    ```cppwinrt
    void EnsureDispatcherQueue();
    ```

    - Dans CompositionHost.cpp, ajoutez la définition de la méthode _EnsureDispatcherQueue_.

    ```cppwinrt
    void CompositionHost::EnsureDispatcherQueue()
    {
        namespace abi = ABI::Windows::System;

        if (m_dispatcherQueueController == nullptr)
        {
            DispatcherQueueOptions options
            {
                sizeof(DispatcherQueueOptions), /* dwSize */
                DQTYPE_THREAD_CURRENT,          /* threadType */
                DQTAT_COM_ASTA                  /* apartmentType */
            };

            Windows::System::DispatcherQueueController controller{ nullptr };
            check_hresult(CreateDispatcherQueueController(options, reinterpret_cast<abi::IDispatcherQueueController**>(put_abi(controller))));
            m_dispatcherQueueController = controller;
        }
    }
    ```

1. Inscrivez la fenêtre de votre application en tant que cible de composition.
    - Dans CompositionHost.h, déclarez une méthode privée nommée _CreateDesktopWindowTarget_ qui prend un HWND comme argument.

    ```cppwinrt
    void CreateDesktopWindowTarget(HWND window);
    ```

    - Dans CompositionHost.cpp, ajoutez la définition de la méthode _CreateDesktopWindowTarget_.

    ```cppwinrt
    void CompositionHost::CreateDesktopWindowTarget(HWND window)
    {
        namespace abi = ABI::Windows::UI::Composition::Desktop;

        auto interop = m_compositor.as<abi::ICompositorDesktopInterop>();
        DesktopWindowTarget target{ nullptr };
        check_hresult(interop->CreateDesktopWindowTarget(window, false, reinterpret_cast<abi::IDesktopWindowTarget**>(put_abi(target))));
        m_target = target;
    }
    ```

1. Créez un conteneur visuel racine destiné à contenir des objets visuels.
    - Dans CompositionHost.h, déclarez une méthode privée nommée _CreateCompositionRoot_.

    ```cppwinrt
    void CreateCompositionRoot();
    ```

    - Dans CompositionHost.cpp, ajoutez la définition de la méthode _CreateCompositionRoot_.

    ```cppwinrt
    void CompositionHost::CreateCompositionRoot()
    {
        auto root = m_compositor.CreateContainerVisual();
        root.RelativeSizeAdjustment({ 1.0f, 1.0f });
        root.Offset({ 124, 12, 0 });
        m_target.Root(root);
    }
    ```

Générez le projet maintenant pour vous assurer qu’il n’y a pas d’erreur.

Ces méthodes configurent les composants nécessaires à l’interopérabilité entre la couche visuelle UWP et les API Win32. Vous pouvez maintenant ajouter du contenu à votre application.

### <a name="add-composition-elements"></a>Ajouter des éléments de composition

Une fois l’infrastructure en place, vous pouvez générer le contenu de composition que vous souhaitez montrer.

Pour cet exemple, vous ajoutez du code qui crée un [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual) carré de couleur aléatoire avec une animation qui entraîne sa suppression après un bref délai.

1. Ajoutez un élément de composition.
    - Dans CompositionHost.h, déclarez une méthode publique nommée _AddElement_ qui prend 3 valeurs **float** comme arguments.

    ```cppwinrt
    void AddElement(float size, float x, float y);
    ```

    - Dans CompositionHost.cpp, ajoutez la définition de la méthode _AddElement_.

    ```cppwinrt
    void CompositionHost::AddElement(float size, float x, float y)
    {
        if (m_target.Root())
        {
            auto visuals = m_target.Root().as<ContainerVisual>().Children();
            auto visual = m_compositor.CreateSpriteVisual();

            auto element = m_compositor.CreateSpriteVisual();
            uint8_t r = (double)(double)(rand() % 255);;
            uint8_t g = (double)(double)(rand() % 255);;
            uint8_t b = (double)(double)(rand() % 255);;

            element.Brush(m_compositor.CreateColorBrush({ 255, r, g, b }));
            element.Size({ size, size });
            element.Offset({ x, y, 0.0f, });

            auto animation = m_compositor.CreateVector3KeyFrameAnimation();
            auto bottom = (float)600 - element.Size().y;
            animation.InsertKeyFrame(1, { element.Offset().x, bottom, 0 });

            using timeSpan = std::chrono::duration<int, std::ratio<1, 1>>;

            std::chrono::seconds duration(2);
            std::chrono::seconds delay(3);

            animation.Duration(timeSpan(duration));
            animation.DelayTime(timeSpan(delay));
            element.StartAnimation(L"Offset", animation);
            visuals.InsertAtTop(element);

            visuals.InsertAtTop(visual);
        }
    }
    ```

## <a name="create-and-show-the-window"></a>Créer et afficher la fenêtre

À présent, vous pouvez ajouter un bouton et le contenu de composition UWP à votre interface utilisateur Win32.

1. Dans HelloComposition.cpp, au début du fichier, incluez _CompositionHost.h_, définissez BTN_ADD et récupérez une instance de CompositionHost.

    ```cppwinrt
    #include "CompositionHost.h"

    // #define MAX_LOADSTRING 100 // This is already in the file.
    #define BTN_ADD 1000

    CompositionHost* compHost = CompositionHost::GetInstance();
    ```

1. Dans la méthode `InitInstance`, changez la taille de la fenêtre créée. (Dans cette ligne, remplacez `CW_USEDEFAULT, 0` par `900, 672`.)

    ```cppwinrt
    HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
        CW_USEDEFAULT, 0, 900, 672, nullptr, nullptr, hInstance, nullptr);
    ```

1. Dans la fonction WndProc, ajoutez `case WM_CREATE` au bloc switch _message_. Dans ce cas, vous initialisez le CompositionHost et créez le bouton.

    ```cppwinrt
    case WM_CREATE:
    {
        compHost->Initialize(hWnd);
        srand(time(nullptr));

        CreateWindow(TEXT("button"), TEXT("Add element"),
            WS_VISIBLE | WS_CHILD | BS_PUSHBUTTON,
            12, 12, 100, 50,
            hWnd, (HMENU)BTN_ADD, nullptr, nullptr);
    }
    break;
    ```

1. De même, dans la fonction WndProc, gérez le clic sur le bouton pour ajouter un élément de composition à l’interface utilisateur. 

    Ajoutez `case BTN_ADD` au bloc switch _wmId_ à l’intérieur du bloc WM_COMMAND.

    ```cppwinrt
    case BTN_ADD: // addButton click
    {
        double size = (double)(rand() % 150 + 50);
        double x = (double)(rand() % 600);
        double y = (double)(rand() % 200);
        compHost->AddElement(size, x, y);
        break;
    }
    ```

À présent, vous pouvez générer et exécuter votre application. Si nécessaire, consultez le code complet à la fin du tutoriel pour vous assurer que tout le code se trouve à la bonne place.

Quand vous exécutez l’application et cliquez sur le bouton, vous devez voir les carrés animés ajoutés à l’interface utilisateur.

## <a name="additional-resources"></a>Ressources supplémentaires

- [Exemple Win32 HelloComposition (GitHub)](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)
- [Bien démarrer avec Win32 et C++](/windows/desktop/learnwin32/learn-to-program-for-windows)
- [Bien démarrer avec les applications Windows 10](/windows/uwp/get-started/) (UWP)
- [Améliorer votre application de bureau pour Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Windows.UI.Composition namespace](/uwp/api/windows.ui.composition) (UWP)

## <a name="complete-code"></a>Code complet

Voici le code complet de la classe CompositionHost et de la méthode InitInstance.

### <a name="compositionhosth"></a>CompositionHost.h

```cppwinrt
#pragma once
#include <winrt/Windows.UI.Composition.Desktop.h>
#include <windows.ui.composition.interop.h>
#include <DispatcherQueue.h>

class CompositionHost
{
public:
    ~CompositionHost();
    static CompositionHost* GetInstance();

    void Initialize(HWND hwnd);
    void AddElement(float size, float x, float y);

private:
    CompositionHost();

    void CreateDesktopWindowTarget(HWND window);
    void EnsureDispatcherQueue();
    void CreateCompositionRoot();

    winrt::Windows::UI::Composition::Compositor m_compositor{ nullptr };
    winrt::Windows::UI::Composition::Desktop::DesktopWindowTarget m_target{ nullptr };
    winrt::Windows::System::DispatcherQueueController m_dispatcherQueueController{ nullptr };
};
```

### <a name="compositionhostcpp"></a>CompositionHost.cpp

```cppwinrt
#include "pch.h"
#include "CompositionHost.h"

using namespace winrt;
using namespace Windows::System;
using namespace Windows::UI;
using namespace Windows::UI::Composition;
using namespace Windows::UI::Composition::Desktop;
using namespace Windows::Foundation::Numerics;

CompositionHost::CompositionHost()
{
}

CompositionHost* CompositionHost::GetInstance()
{
    static CompositionHost instance;
    return &instance;
}

CompositionHost::~CompositionHost()
{
}

void CompositionHost::Initialize(HWND hwnd)
{
    EnsureDispatcherQueue();
    if (m_dispatcherQueueController) m_compositor = Compositor();

    if (m_compositor)
    {
        CreateDesktopWindowTarget(hwnd);
        CreateCompositionRoot();
    }
}

void CompositionHost::EnsureDispatcherQueue()
{
    namespace abi = ABI::Windows::System;

    if (m_dispatcherQueueController == nullptr)
    {
        DispatcherQueueOptions options
        {
            sizeof(DispatcherQueueOptions), /* dwSize */
            DQTYPE_THREAD_CURRENT,          /* threadType */
            DQTAT_COM_ASTA                  /* apartmentType */
        };

        Windows::System::DispatcherQueueController controller{ nullptr };
        check_hresult(CreateDispatcherQueueController(options, reinterpret_cast<abi::IDispatcherQueueController**>(put_abi(controller))));
        m_dispatcherQueueController = controller;
    }
}

void CompositionHost::CreateDesktopWindowTarget(HWND window)
{
    namespace abi = ABI::Windows::UI::Composition::Desktop;

    auto interop = m_compositor.as<abi::ICompositorDesktopInterop>();
    DesktopWindowTarget target{ nullptr };
    check_hresult(interop->CreateDesktopWindowTarget(window, false, reinterpret_cast<abi::IDesktopWindowTarget**>(put_abi(target))));
    m_target = target;
}

void CompositionHost::CreateCompositionRoot()
{
    auto root = m_compositor.CreateContainerVisual();
    root.RelativeSizeAdjustment({ 1.0f, 1.0f });
    root.Offset({ 124, 12, 0 });
    m_target.Root(root);
}

void CompositionHost::AddElement(float size, float x, float y)
{
    if (m_target.Root())
    {
        auto visuals = m_target.Root().as<ContainerVisual>().Children();
        auto visual = m_compositor.CreateSpriteVisual();

        auto element = m_compositor.CreateSpriteVisual();
        uint8_t r = (double)(double)(rand() % 255);;
        uint8_t g = (double)(double)(rand() % 255);;
        uint8_t b = (double)(double)(rand() % 255);;

        element.Brush(m_compositor.CreateColorBrush({ 255, r, g, b }));
        element.Size({ size, size });
        element.Offset({ x, y, 0.0f, });

        auto animation = m_compositor.CreateVector3KeyFrameAnimation();
        auto bottom = (float)600 - element.Size().y;
        animation.InsertKeyFrame(1, { element.Offset().x, bottom, 0 });

        using timeSpan = std::chrono::duration<int, std::ratio<1, 1>>;

        std::chrono::seconds duration(2);
        std::chrono::seconds delay(3);

        animation.Duration(timeSpan(duration));
        animation.DelayTime(timeSpan(delay));
        element.StartAnimation(L"Offset", animation);
        visuals.InsertAtTop(element);

        visuals.InsertAtTop(visual);
    }
}
```

### <a name="hellocompositioncpp-partial"></a>HelloComposition.cpp (partiel)

```cppwinrt
#include "pch.h"
#include "HelloComposition.h"
#include "CompositionHost.h"

#define MAX_LOADSTRING 100
#define BTN_ADD 1000

CompositionHost* compHost = CompositionHost::GetInstance();

// Global Variables:

// ...
// ... code not shown ...
// ...

BOOL InitInstance(HINSTANCE hInstance, int nCmdShow)
{
   hInst = hInstance; // Store instance handle in our global variable

   HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
      CW_USEDEFAULT, 0, 900, 672, nullptr, nullptr, hInstance, nullptr);

// ...
// ... code not shown ...
// ...
}

// ...

LRESULT CALLBACK WndProc(HWND hWnd, UINT message, WPARAM wParam, LPARAM lParam)
{
    switch (message)
    {
// Add this...
    case WM_CREATE:
    {
        compHost->Initialize(hWnd);
        srand(time(nullptr));

        CreateWindow(TEXT("button"), TEXT("Add element"),
            WS_VISIBLE | WS_CHILD | BS_PUSHBUTTON,
            12, 12, 100, 50,
            hWnd, (HMENU)BTN_ADD, nullptr, nullptr);
    }
    break;
// ...
    case WM_COMMAND:
    {
        int wmId = LOWORD(wParam);
        // Parse the menu selections:
        switch (wmId)
        {
        case IDM_ABOUT:
            DialogBox(hInst, MAKEINTRESOURCE(IDD_ABOUTBOX), hWnd, About);
            break;
        case IDM_EXIT:
            DestroyWindow(hWnd);
            break;
// Add this...
        case BTN_ADD: // addButton click
        {
            double size = (double)(rand() % 150 + 50);
            double x = (double)(rand() % 600);
            double y = (double)(rand() % 200);
            compHost->AddElement(size, x, y);
            break;
        }
// ...
        default:
            return DefWindowProc(hWnd, message, wParam, lParam);
        }
    }
    break;
    case WM_PAINT:
    {
        PAINTSTRUCT ps;
        HDC hdc = BeginPaint(hWnd, &ps);
        // TODO: Add any drawing code that uses hdc here...
        EndPaint(hWnd, &ps);
    }
    break;
    case WM_DESTROY:
        PostQuitMessage(0);
        break;
    default:
        return DefWindowProc(hWnd, message, wParam, lParam);
    }
    return 0;
}

// ...
// ... code not shown ...
// ...
```