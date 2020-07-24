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
ms.openlocfilehash: 7393d4d1bae227bf3b586a54fba5d43ca2dcb53e
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493404"
---
# <a name="get-started-with-winui-3-for-desktop-apps"></a>Bien démarrer avec WinUI 3 pour les applications de bureau

WinUI 3 Preview 2 introduit de nouveaux modèles de projet qui vous permettent de créer des applications de bureau C#/.NET Core gérées et C++/Win32 natives avec une interface utilisateur entièrement basée sur WinUI. Quand vous créez des applications en utilisant ces modèles de projet, la totalité de l’interface utilisateur de votre application est implémentée à l’aide de fenêtres, de contrôles et d’autres types d’interface utilisateur fournis par WinUI 3. Pour obtenir la liste complète des modèles de projet, consultez [cette section](index.md#project-templates-for-winui-3).

## <a name="prerequisites"></a>Prérequis

Pour utiliser les modèles de projet WinUI 3 pour applications de bureau décrits dans cet article, configurez votre ordinateur de développement et installez WinUI 3 Preview 2 en suivant les instructions mentionnées [ici](index.md#install-winui-3-preview-2).

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

    * ***Nom du projet* (Package)**  : il s’agit d’un [Projet de création de packages d’applications Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) configuré pour générer l’application dans un [package MSIX](https://docs.microsoft.com/windows/msix/overview). Vous bénéficiez ainsi d’une expérience de déploiement moderne, de la possibilité de l’intégrer aux fonctionnalités de Windows 10 par le biais d’extensions de package et bien plus encore. Ce projet contient le [manifeste du package](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root) pour votre application, et il s’agit du projet de démarrage de votre solution par défaut.

        ![Ajouter un projet](images/WinUI-csharp-packageproject.png)

7. Pour ajouter un nouvel élément à votre projet d’application, cliquez avec le bouton droit sur le nœud du projet ***Nom du projet* (Desktop)** dans l’**Explorateur de solutions** et sélectionnez **Ajouter** -> **Nouvel élément**. Dans la boîte de dialogue **Ajouter un nouvel élément**, sélectionnez l’onglet **WinUI**, choisissez l’élément que vous souhaitez ajouter, puis cliquez sur **Ajouter**. Pour plus d’informations sur les éléments disponibles, consultez [cette section](index.md#item-templates-for-winui-3).

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

    * ***Nom du projet* (Package)**  : il s’agit d’un [Projet de création de packages d’applications Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) configuré pour générer l’application dans un [package MSIX](https://docs.microsoft.com/windows/msix/overview). Vous bénéficiez ainsi d’une expérience de déploiement moderne, de la possibilité de l’intégrer aux fonctionnalités de Windows 10 par le biais d’extensions de package et bien plus encore. Ce projet contient le [manifeste du package](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root) pour votre application, et il s’agit du projet de démarrage de votre solution par défaut.

        ![Projet du package](images/WinUI-cpp-packageproject.png)

7. Pour ajouter un nouvel élément à votre projet d’application, cliquez avec le bouton droit sur le nœud du projet ***Nom du projet* (Desktop)** dans l’**Explorateur de solutions** et sélectionnez **Ajouter** -> **Nouvel élément**. Dans la boîte de dialogue **Ajouter un nouvel élément**, sélectionnez l’onglet **WinUI**, choisissez l’élément que vous souhaitez ajouter, puis cliquez sur **Ajouter**. Pour plus d’informations sur les éléments disponibles, consultez [cette section](index.md#item-templates-for-winui-3).

    ![Nouvel élément](images/WinUI-cpp-newitem.png)

8. Générez et exécutez votre solution pour vérifier que l’application s’exécute sans erreur.

## <a name="known-issues-and-limitations"></a>Limitations et problèmes connus

Pour obtenir la liste des problèmes connus et des limitations, consultez [cette section](index.md#preview-2-limitations-and-known-issues).

## <a name="related-topics"></a>Rubriques connexes

* [Bibliothèque d’interface utilisateur Windows 3](index.md)
