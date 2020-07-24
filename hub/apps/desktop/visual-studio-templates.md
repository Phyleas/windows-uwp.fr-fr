---
description: Cet article présente les modèles de projet et d’élément Visual Studio pour les applications Windows.
title: Modèles de projet et d’élément Visual Studio pour les applications Windows
ms.date: 07/02/2020
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, xaml islands, îles xaml
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.openlocfilehash: 30f190b43b9156a92cdaf2ad533ededfc79c3a74
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493981"
---
# <a name="visual-studio-project-and-item-templates-for-windows-apps"></a>Modèles de projet et d’élément Visual Studio pour les applications Windows

Visual Studio 2019 fournit de nombreux modèles de projet et d’élément qui vous aident à créer des applications pour les appareils Windows 10 avec le langage C\# ou C++. Cette rubrique décrit les modèles et vous aide à en choisir un pour votre scénario.

* Les modèles de projet incluent des fichiers projet, des fichiers de code et d’autres ressources qui sont configurés pour créer des applications ou des composants pouvant être chargés et utilisés par une application.
* Les modèles d’élément sont des fichiers projet qui contiennent du code et du XAML couramment utilisés pouvant être ajoutés à un projet pour réduire le temps de développement. Par exemple, vous pouvez utiliser un modèle d’élément pour ajouter une nouvelle fenêtre, une nouvelle page ou un nouveau contrôle à votre application.

## <a name="winui-templates"></a>Modèles WinUI

La [bibliothèque d’interface utilisateur Windows (WinUI)](../winui/index.md) est la plateforme d’interface utilisateur native moderne des applications Windows parmi les plateformes d’applications de bureau (.NET et Win32 natif) et d’applications UWP. [WinUI 3](../winui/winui3/index.md) (actuellement disponible en préversion pour les développeurs) est la dernière version majeure de WinUI. Celle-ci fait de WinUI un framework d’expérience utilisateur complet pour les applications de bureau Windows.

WinUI 3 comprend un package VSIX pour Visual Studio 2019 qui fournit des modèles de projet et d’élément permettant de créer des applications avec une interface basée sur WinUI. Pour plus d’informations sur le package VSIX de WinUI 3 et sur les modèles de projet qu’il fournit, consultez [cette section](../winui/winui3/index.md#install-winui-3-preview-2).

> [!IMPORTANT]
> WinUI 3, ainsi que les modèles Visual Studio qui lui sont associés, sont actuellement disponibles en préversion pour les développeurs, le but étant de réaliser une première évaluation et de rassembler les commentaires de la communauté des développeurs. Pour l’instant, vous ne devez pas l’utiliser avec les applications de production.

## <a name="uwp-templates"></a>Modèles UWP

Visual Studio fournit un large éventail de modèles de projet pour la création d’applications UWP à l’aide du langage C# ou C++. Pour utiliser ces modèles de projet, vous devez inclure la charge de travail **Développement pour la plateforme Windows universelle** lors de l’installation de Visual Studio. Pour les modèles de projet C++, vous devez également inclure le composant facultatif  **C++ (v142) Universal Windows Platforms Tools** pour la charge de travail **Développement pour la plateforme Windows universelle**.

### <a name="project-templates-for-c-and-uwp"></a>Modèles de projet pour C# et UWP

Pour accéder aux modèles de projet C# UWP lorsque vous créez un projet dans Visual Studio, filtrez le langage sur **C#** , la plateforme sur **Windows** et le type de projet sur **UWP**.

![Modèles de projet C# UWP](images/uwp-projects-csharp.png)

Vous pouvez utiliser ces modèles de projet pour créer des applications C# UWP.

| Modèle | Description |
|----------|-------------|
| Application vide (Windows universel) | Créez une application UWP. Le projet généré comprend une page de base qui est dérivée de la classe [Windows.UI.Xaml.Controls.Page](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page) que vous pouvez utiliser pour créer votre interface utilisateur. |
| Application de test unitaire (Windows universel) | Crée un projet de test unitaire en C# pour une application UWP. Pour plus d’informations, consultez [Tests unitaires du code C#](https://docs.microsoft.com/visualstudio/test/unit-testing-visual-csharp-code-in-a-store-app). |

Vous pouvez utiliser ces modèles de projet pour créer certaines parties d’une application UWP C#.

| Modèle | Description |
|----------|-------------|
| Bibliothèque statique (Windows universel) | Crée une bibliothèque de classes managées (DLL) en C# qui peut être utilisée par d’autres applications UWP écrites à l’aide de code managé. |
| Composant Windows Runtime (Windows universel) | Crée un [composant Windows Runtime](https://docs.microsoft.com/windows/uwp/winrt-components/) écrit en C# qui peut être consommé par n’importe quelle application UWP, quel que soit le langage de programmation dans lequel elle a été écrite. |
| Package de code facultatif (Windows universel) | Crée un package facultatif avec du code C# exécutable qui peut être chargé par une application. Pour plus d’informations, consultez [Packages facultatifs avec code exécutable](https://docs.microsoft.com/windows/msix/package/optional-packages-with-executable-code).  |

### <a name="project-templates-for-c-and-uwp"></a>Modèles de projet pour C++ et UWP

Vous pouvez créer des applications UWP C++ à l’aide de deux technologies :

* La technologie recommandée est [C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/). Il s’agit d’une projection du langage C++ qui est implémentée uniquement dans les fichiers d’en-tête, et qui est conçue pour fournir un accès de première classe à l’API WinRT moderne.
* Vous pouvez également utiliser l’ancien ensemble d’extensions [C++/CX](https://docs.microsoft.com/cpp/cppcx/visual-c-language-reference-c-cx). C++/CX est toujours pris en charge, toutefois, nous vous recommandons d’utiliser C++/WinRT.

Pour accéder aux modèles de projet C++ UWP lorsque vous créez un projet dans Visual Studio, filtrez le langage sur **C++** , la plateforme sur **Windows** et le type de projet sur **UWP**. 

> [!NOTE]
> Par défaut, la charge de travail **Développement pour la plateforme Windows universelle** de Visual Studio permet uniquement l’accès aux modèles de projet C++/CX. Pour accéder aux modèles de projet C++/WinRT, vous devez installer le package [VSIX C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

![Modèles de projet C++ UWP](images/uwp-projects-cpp.png)

Vous pouvez utiliser ces modèles de projet pour créer des applications C++ UWP.

| Modèle | Description |
|----------|-------------|
| Application vide (C++/WinRT) | Crée une application UWP C++/WinRT avec une interface utilisateur XAML. Le projet généré comprend une page de base qui est dérivée de la classe [Windows.UI.Xaml.Controls.Page](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page) que vous pouvez utiliser pour créer votre interface utilisateur. |
| Application Core (C++/WinRT) | Crée une application UWP C++/WinRT qui utilise [CoreApplication](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplication) pour s’intégrer à divers frameworks d’interface utilisateur plutôt qu’une interface utilisateur XAML. Pour obtenir une procédure pas à pas sur l’utilisation de ce modèle de projet en vue de créer un jeu simple qui utilise DirectX, consultez [Créer un jeu UWP simple avec DirectX](https://docs.microsoft.com/windows/uwp/gaming/tutorial--create-your-first-uwp-directx-game). |
| Application vide (Windows universel - C++/CX) | Crée une application UWP C++/WinRT avec une interface utilisateur XAML. Le projet généré comprend une page de base qui est dérivée de la classe [Windows.UI.Xaml.Controls.Page](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page) de la bibliothèque WinUI et que vous pouvez utiliser pour créer votre interface utilisateur. |
| Application DirectX 11 et XAML (Windows universel - C++/CX) | Crée une application UWP qui utilise DirectX 11 et un **SwapChainPanel** pour vous permettre d’utiliser des contrôles d’interface utilisateur XAML. Pour plus d’informations, consultez [Modèles de projet de jeu DirectX](https://docs.microsoft.com/windows/uwp/gaming/user-interface). |
| Application DirectX 11 (Windows universel - C++/CX) | Crée une application UWP qui utilise DirectX 11. Pour plus d’informations, consultez [Modèles de projet de jeu DirectX](https://docs.microsoft.com/windows/uwp/gaming/user-interface). |
| Application DirectX 12 (Windows universel - C++/CX) | Crée une application UWP qui utilise DirectX 12. Pour plus d’informations, consultez [Modèles de projet de jeu DirectX](https://docs.microsoft.com/windows/uwp/gaming/user-interface). |
| Application de test unitaire (Windows universel - C++/CX) | Crée un projet de test unitaire en C++/CX pour une application UWP. Pour plus d’informations, consultez [Tester une DLL UWP C++](https://docs.microsoft.com/visualstudio/test/unit-testing-a-visual-cpp-dll-for-store-apps). |

Vous pouvez utiliser ces modèles de projet pour créer certaines parties d’une application UWP C++.

| Modèle | Description |
|----------|-------------|
| Composant Windows Runtime (C++/WinRT) | Crée un [composant Windows Runtime](https://docs.microsoft.com/windows/uwp/winrt-components/) écrit en C++/WinRT qui peut être consommé par n’importe quelle application UWP, quel que soit le langage de programmation dans lequel elle a été écrite. |
| Composant Windows Runtime (Windows universel) | Crée un [composant Windows Runtime](https://docs.microsoft.com/windows/uwp/winrt-components/) écrit en C++/CX qui peut être consommé par n’importe quelle application UWP, quel que soit le langage de programmation dans lequel elle a été écrite. |
| DLL (Windows universel) | Projet de création d’une bibliothèque de liens dynamiques (DLL) en C++/CX pouvant être utilisée dans une application UWP. Pour plus d’informations, consultez [DLL (C++/CX)](https://docs.microsoft.com/cpp/cppcx/dlls-c-cx). |
| Bibliothèque statique (Universal Windows) | Projet de création d’une bibliothèque statique (LIB) en C++/CX pouvant être utilisée dans une application UWP. Pour plus d’informations, consultez [Bibliothèques statiques (C++/CX)](https://docs.microsoft.com/cpp/cppcx/static-libraries-c-cx). |

## <a name="cwin32-templates"></a>Modèles C++/Win32

Visual Studio fournit un large éventail de modèles de projet pour la création d’applications de bureau Windows avec du C++ natif et un accès direct à l’API Win32. Pour utiliser ces modèles de projet, vous devez inclure la charge de travail **Développement Desktop en C++** lors de l’installation de Visual Studio. Cette charge de travail comprend des modèles de projet pour la création d’applications de bureau, d’applications console et de bibliothèques.

### <a name="project-templates-for-desktop-apps"></a>Modèles de projet pour les applications de bureau

Si vous devez accéder aux modèles de projet C++ pour la création d’applications de bureau classiques lorsque vous créez un projet dans Visual Studio, filtrez le langage sur **C++** , la plateforme sur **Windows** et le type de projet sur **Desktop**.

![Modèles de projet pour applications en C++ natif](images/desktop-app-projects-cpp.png)

| Modèle | Description |
|----------|----------|
| Application de bureau Windows | Crée une application de bureau Windows classique à l’aide du langage C++. Pour plus d'informations, consultez [Démonstration : Créer une application de bureau Windows traditionnelle](https://docs.microsoft.com/cpp/windows/walkthrough-creating-windows-desktop-applications-cpp). |
| Assistant Windows Desktop | Fournit un Assistant pas à pas que vous pouvez utiliser pour créer l’un des types de projets suivants : une application de bureau Windows classique, une application console, une bibliothèque de liens dynamiques (DLL) ou une bibliothèque statique. Pour plus d’informations, consultez [Assistant Windows Desktop](https://docs.microsoft.com/cpp/windows/windows-desktop-wizard) et [Démonstration : Créer une application de bureau Windows traditionnelle](https://docs.microsoft.com/cpp/windows/walkthrough-creating-windows-desktop-applications-cpp).         |
| Projet de création de package d’application Windows | Crée un projet que vous pouvez utiliser pour créer une application de bureau dans un [package MSIX](https://docs.microsoft.com/windows/msix/overview). Vous bénéficiez ainsi d’une expérience de déploiement moderne, de la possibilité de l’intégrer aux fonctionnalités de Windows 10 par le biais d’extensions de package et bien plus encore. Pour plus d’informations, consultez [Projet de création de packages d’applications Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net).  |

### <a name="project-templates-for-console-apps"></a>Modèles de projet pour applications console

Si vous devez accéder aux modèles de projet C++ pour les applications console, filtrez le langage sur **C++** , la plateforme sur **Windows** et le type de projet sur **Console**.

![Modèles de projet pour consoles en C++ natif](images/desktop-console-projects-cpp.png)

| Modèle | Description |
|----------|----------|
| Application console Windows (C++/WinRT) | Crée une application console [C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/) sans interface utilisateur. Pour plus d’informations, consultez [Guide de démarrage rapide C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/get-started#a-cwinrt-quick-start). Ce modèle de projet nécessite le package [VSIX C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).  |
| Application console | Crée une application console sans interface utilisateur. Pour plus d'informations, consultez [Démonstration : Création d’un programme C++ standard](https://docs.microsoft.com/cpp/windows/walkthrough-creating-a-standard-cpp-program-cpp). |
| Projet vide | Projet vide pour créer une application, une bibliothèque ou une DLL. Vous devez ajouter le code ou les ressources nécessaires. |

### <a name="project-templates-for-libraries"></a>Modèles de projet pour bibliothèques

Si vous devez accéder aux modèles de projet C++ pour les bibliothèques, filtrez le langage sur **C++** , la plateforme sur **Windows** et le type de projet sur **Bibliothèque**.

![Modèles de projet pour bibliothèques en C++ natif](images/desktop-library-projects-cpp.png)

| Modèle | Description |
|----------|----------|
| Bibliothèque de liens dynamiques (DLL) | Projet pour la création d’une bibliothèque de liens dynamiques (DLL). Pour plus d'informations, consultez [Démonstration : Création et utilisation d’une bibliothèque de liens dynamiques](https://docs.microsoft.com/cpp/build/walkthrough-creating-and-using-a-dynamic-link-library-cpp). |
| Bibliothèque statique | Projet pour la création d’une bibliothèque statique (LIB). Pour plus d'informations, consultez [Démonstration : Créer et utiliser une bibliothèque statique](https://docs.microsoft.com/cpp/build/walkthrough-creating-and-using-a-static-library-cpp). |

### <a name="item-templates-for-native-c-and-win32"></a>Modèles d’élément pour le C++ natif et Win32

Les modèles de projet C++ incluent de nombreux modèles d’élément que vous pouvez utiliser pour effectuer des tâches telles que l’ajout de fichiers et de ressources à votre projet. Pour obtenir une liste complète, consultez [Utilisation de modèles Ajouter un nouvel élément de Visual C++](https://docs.microsoft.com/cpp/build/reference/using-visual-cpp-add-new-item-templates).

## <a name="net-templates"></a>Modèles .NET

Visual Studio fournit un large éventail de modèles de projet pour la création d’applications de bureau Windows qui utilisent .NET et C#. Pour utiliser ces modèles de projet, vous devez inclure la charge de travail **Développement .NET Desktop** lors de l’installation de Visual Studio.

Pour accéder aux modèles de projet C# .NET lorsque vous créez un projet dans Visual Studio, filtrez le langage sur **C#** , la plateforme sur **Windows** et le type de projet sur **Desktop**.

![Modèles de projet C# .NET](images/dotnet-projects-csharp.png)

Vous pouvez utiliser ces modèles de projet pour créer des applications à l’aide de C# et .NET.

| Modèle | Description |
|----------|----------|
| Application WPF (.NET Core) | Crée une application [WPF](https://docs.microsoft.com/dotnet/framework/wpf/) qui cible [.NET Core](https://docs.microsoft.com/dotnet/core/). Pour obtenir une procédure pas à pas de ce modèle de projet, consultez [Créer une application WPF](https://docs.microsoft.com/visualstudio/get-started/csharp/tutorial-wpf). |
| Application WPF (.NET Framework) | Crée une application [WPF](https://docs.microsoft.com/dotnet/framework/wpf/) qui cible [.NET Framework](https://docs.microsoft.com/dotnet/framework/). Pour obtenir une procédure pas à pas de ce modèle de projet, consultez [Tutoriel : Créer votre première application WPF](https://docs.microsoft.com/dotnet/framework/wpf/getting-started/walkthrough-my-first-wpf-desktop-application). |
| Application Windows Forms (.NET Core) | Crée une application [Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/) qui cible [.NET Core](https://docs.microsoft.com/dotnet/core/).  |
| Application Windows Forms (.NET Framework) | Crée une application [Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/) qui cible [.NET Framework](https://docs.microsoft.com/dotnet/framework/). Pour obtenir une procédure pas à pas de ce modèle de projet, consultez [Créer une application Windows Forms dans Visual Studio avec C#](https://docs.microsoft.com/visualstudio/ide/create-csharp-winform-visual-studio). |
| Projet de création de package d’application Windows | Crée un projet que vous pouvez utiliser pour créer une application WPF ou Windows Forms dans un [package MSIX](https://docs.microsoft.com/windows/msix/overview). Vous bénéficiez ainsi d’une expérience de déploiement moderne, de la possibilité de l’intégrer aux fonctionnalités de Windows 10 par le biais d’extensions de package et bien plus encore. Pour plus d’informations, consultez [Projet de création de packages d’applications Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net). |
