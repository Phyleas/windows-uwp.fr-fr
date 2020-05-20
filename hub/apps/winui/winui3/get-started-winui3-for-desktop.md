---
description: Ce guide vous montre comment commencer à créer des applications de bureau .NET et C++/Win32 avec une interface utilisateur WinUI 3.
title: Bien démarrer avec WinUI 3 pour les applications de bureau
ms.date: 05/19/2020
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, xaml islands, îles xaml
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: ab67507153e0ff7065baffa92ea6ec35aee5b132
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580766"
---
# <a name="get-started-with-winui-30-for-desktop-apps"></a>Bien démarrer avec WinUI 3.0 pour les applications de bureau

WinUI 3.0 Preview 1 introduit de nouveaux modèles de projet qui vous permettent de créer des applications de bureau C#/.NET managées et C++/Win32 natives avec une interface utilisateur entièrement basée sur WinUI. Quand vous créez des applications à l’aide de ces modèles de projet, la totalité de l’interface utilisateur de votre application est implémentée à l’aide de fenêtres, de contrôles et d’autres types d’interface utilisateur fournis par WinUI 3.0. 

WinUI 3.0 Preview 1 ajoute les modèles de projet **WinUI in Desktop** suivants dans Visual Studio 2019 :

* Applications et bibliothèques C# qui ciblent .NET 5 :
  * Blank App, Packaged (WinUI in Desktop)
  * Class Library (WinUI in Desktop)

* Applications C++/Win32 :
  * Blank App, Packaged (WinUI in Desktop)

Les modèles de projet d’application génèrent un projet d’application WinUI et un [Projet de création de packages d’applications Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) configuré pour générer l’application dans un package [MSIX](https://docs.microsoft.com/windows/msix/overview) pour le déploiement.

## <a name="prerequisites"></a>Prérequis

Pour utiliser les modèles de projet WinUI 3 pour applications de bureau décrits dans cet article, configurez votre ordinateur de développement en suivant les instructions ci-dessous :

1. Vérifiez que votre ordinateur de développement dispose de Windows 10, version 1803 (build 17134) ou d’une version plus récente. WinUI 3 pour les applications de bureau nécessite la version 1803 ou une version ultérieure du système d’exploitation.

2. Installez Visual Studio 2019, version 16.7 Preview 1. Pour plus d’informations, consultez [ces instructions](index.md#configure-your-dev-environment).

3. Installez les versions x64 et x86 de .NET 5 Preview 4 :
    * x64 : [https://aka.ms/dotnet/net5/preview4/Sdk/dotnet-sdk-win-x64.exe](https://aka.ms/dotnet/net5/preview4/Sdk/dotnet-sdk-win-x64.exe)
    * x86 : [https://aka.ms/dotnet/net5/preview4/Sdk/dotnet-sdk-win-x86.exe](https://aka.ms/dotnet/net5/preview4/Sdk/dotnet-sdk-win-x86.exe)

4. Installez l’extension VSIX qui comprend les modèles de projet WinUI 3.0 Preview 1 pour Visual Studio 2019. Pour plus d’informations, consultez [ces instructions](index.md#visual-studio-project-templates).

## <a name="create-a-winui-3-desktop-app-for-c-and-net-5"></a>Créer une application de bureau WinUI 3 pour C# et .NET 5

1. Dans Visual Studio 2019, sélectionnez **Fichier** -> **Nouveau** -> **Projet**.

2. Dans les filtres déroulants de projet, sélectionnez respectivement **C#** , **Windows** et **WinUI**.

3. Sélectionnez le type de projet **Blank App, Packaged (WinUI in Desktop)** , puis cliquez sur **Suivant**.

    ![Modèle de projet d’application vide](images/WinUI-csharp-newproject.png)

4. Entrez un nom de projet, choisissez d’autres options si vous le souhaitez, puis cliquez sur **Créer**.

5. Dans la boîte de dialogue suivante, définissez Windows 10, version 1903 (build 18362) comme **Version cible** et Windows 10, version 1803 (build 17134) comme **Version minimale**, puis cliquez sur **OK**.

    ![Version cible et minimale](images/WinUI-min-target-version.png)

6. À ce stade, Visual Studio génère deux projets :

    * ***Nom du projet* (Desktop)**  : ce projet contient le code de votre application. Le fichier de code **App.xaml.cs** définit une classe `Application` qui représente l’instance de votre application, et le fichier de code **MainWindow.xaml.cs** définit une classe `MainWindow` qui représente la fenêtre principale affichée par votre application. Ces classes dérivent de types dans l’espace de noms **Microsoft.UI.Xaml** fourni par WinUI.

        ![Ajouter un projet](images/WinUI-csharp-appproject.png)

    * ***Nom du projet* (Package)**  : il s’agit d’un [Projet de création de packages d’applications Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) configuré pour générer l’application dans un package MSIX pour le déploiement. Ce projet contient le [manifeste du package](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root) pour votre application, et il s’agit du projet de démarrage de votre solution par défaut.

        ![Ajouter un projet](images/WinUI-csharp-packageproject.png)

7. Pour ajouter un nouvel élément à votre projet d’application, cliquez avec le bouton droit sur le nœud du projet ***Nom du projet* (Desktop)** dans l’**Explorateur de solutions** et sélectionnez **Ajouter** -> **Nouvel élément**. Dans la boîte de dialogue **Ajouter un nouvel élément**, sélectionnez l’onglet **WinUI**, choisissez l’élément que vous souhaitez ajouter, puis cliquez sur **Ajouter**. Vous pouvez choisir parmi les types d’éléments suivants :

    * **Page vierge**
    * **Fenêtre vide**
    * **Contrôle personnalisé**
    * **Dictionnaire de ressources**
    * **Fichier de ressources**
    * **Contrôle utilisateur**

    ![Nouvel élément](images/WinUI-csharp-newitem.png)

8. Générez et exécutez votre solution pour vérifier que l’application s’exécute sans erreur.

## <a name="create-a-winui-3-desktop-app-for-cwin32"></a>Créer une application de bureau WinUI 3 pour C++/Win32

1. Dans Visual Studio 2019, sélectionnez **Fichier** -> **Nouveau** -> **Projet**.

2. Dans les filtres déroulants de projet, sélectionnez **C++** , **Windows** et **WinUI**.

3. Sélectionnez le type de projet **Blank App, Packaged (WinUI in Desktop)** , puis cliquez sur **Suivant**.

    ![Modèle de projet d’application vide](images/WinUI-cpp-newproject.png)

4. Entrez un nom de projet, choisissez d’autres options si vous le souhaitez, puis cliquez sur **Créer**.

5. Dans la boîte de dialogue suivante, définissez Windows 10, version 1903 (build 18362) comme **Version cible** et Windows 10, version 1803 (build 17134) comme **Version minimale**, puis cliquez sur **OK**.

    ![Version cible et minimale](images/WinUI-min-target-version.png)

6. À ce stade, Visual Studio génère deux projets :

    * ***Nom du projet* (Desktop)**  : ce projet contient le code de votre application. Le fichier de code **App.xaml.cs** et différents fichiers de code **App** définissent une classe `Application` qui représente l’instance de votre application, et le fichier de code **MainWindow.xaml** et différents fichiers de code **MainWindow** définissent une classe `MainWindow` qui représente la fenêtre principale affichée par votre application. Ces classes dérivent de types dans l’espace de noms **Microsoft.UI.Xaml** fourni par WinUI.

        ![Ajouter un projet](images/WinUI-cpp-appproject.png)

    * ***Nom du projet* (Package)**  : il s’agit d’un [Projet de création de packages d’applications Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) configuré pour générer l’application dans un package MSIX pour le déploiement. Ce projet contient le [manifeste du package](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root) pour votre application, et il s’agit du projet de démarrage de votre solution par défaut.

        ![Projet du package](images/WinUI-cpp-packageproject.png)

7. Pour ajouter un nouvel élément à votre projet d’application, cliquez avec le bouton droit sur le nœud du projet ***Nom du projet* (Desktop)** dans l’**Explorateur de solutions** et sélectionnez **Ajouter** -> **Nouvel élément**. Dans la boîte de dialogue **Ajouter un nouvel élément**, sélectionnez l’onglet **WinUI**, choisissez l’élément que vous souhaitez ajouter, puis cliquez sur **Ajouter**. Vous pouvez choisir parmi les types d’éléments suivants :

    * **Page vierge**
    * **Fenêtre vide**
    * **Contrôle personnalisé**
    * **Dictionnaire de ressources**
    * **Fichier de ressources**
    * **Contrôle utilisateur**

    ![Nouvel élément](images/WinUI-cpp-newitem.png)

8. Générez et exécutez votre solution pour vérifier que l’application s’exécute sans erreur.

## <a name="known-issues-and-limitations"></a>Limitations et problèmes connus

Pour obtenir la liste des problèmes connus et des limitations de Preview 1, consultez [cette section](index.md#preview-1-limitations-and-known-issues).

## <a name="related-topics"></a>Rubriques connexes

* [WinUI 3.0](index.md)