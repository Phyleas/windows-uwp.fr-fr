---
description: Ce guide vous montre comment commencer à créer des applications de bureau .NET et C++/Win32 avec une interface utilisateur WinUI 3.
title: Bien démarrer avec WinUI 3 pour les applications de bureau
ms.date: 11/17/2020
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, xaml islands, îles xaml
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 067e6a6798fbfc2633c3e356be64ecae0403cc6d
ms.sourcegitcommit: 4df27104a9e346d6b9fb43184812441fe5ea3437
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96025155"
---
# <a name="get-started-with-winui-3-for-desktop-apps"></a>Bien démarrer avec WinUI 3 pour les applications de bureau

WinUI 3 Preview 3 contient de nouveaux modèles de projet qui vous permettent de créer des applications de bureau C#/.NET Core gérées et C++/Win32 natives avec une interface utilisateur entièrement basée sur WinUI. Quand vous créez des applications en utilisant ces modèles de projet, la totalité de l’interface utilisateur de votre application est implémentée à l’aide de fenêtres, de contrôles et d’autres types d’interface utilisateur fournis par WinUI 3. Pour obtenir la liste complète des modèles de projet, consultez [cette section](index.md#project-templates-for-winui-3).

## <a name="prerequisites"></a>Prérequis

Pour utiliser les modèles de projet WinUI 3 pour applications de bureau décrits dans cet article, configurez votre ordinateur de développement et installez WinUI 3 Preview 3 en suivant les instructions mentionnées [ici](index.md#install-winui-3-preview-3).

## <a name="create-a-winui-3-desktop-app-for-c-and-net-5"></a>Créer une application de bureau WinUI 3 pour C# et .NET 5

1. Dans Visual Studio 2019, sélectionnez **Fichier** -> **Nouveau** -> **Projet**.

2. Dans les filtres déroulants de projet, sélectionnez respectivement **C#** , **Windows** et **WinUI**.

3. Sélectionnez le type de projet **Blank App, Packaged (WinUI in Desktop)** , puis cliquez sur **Suivant**.

    ![Capture d’écran de l’Assistant Créer un projet avec l’option Blank App, Packaged (WinUI in Desktop) mise en surbrillance.](images/WinUI-csharp-newproject.png)

4. Entrez un nom de projet, choisissez d’autres options si vous le souhaitez, puis cliquez sur **Créer**.

5. Dans la boîte de dialogue suivante, définissez Windows 10, version 1903 (build 18362) comme **Version cible** et Windows 10, version 1803 (build 17134) comme **Version minimale**, puis cliquez sur **OK**.

    ![Version cible et minimale](images/WinUI-min-target-version.png)

6. À ce stade, Visual Studio génère deux projets :

    * **_Nom du projet_ (Desktop)**  : ce projet contient le code de votre application. Le fichier **App.xaml** et le fichier code-behind **App.xaml.cs** définissent une classe `Application` qui représente votre instance d’application. Le fichier **MainWindow.xaml** et le fichier code-behind **MainWindow.xaml.cs** définissent une classe `MainWindow` qui représente la fenêtre principale affichée par votre application. Ces classes dérivent de types dans l’espace de noms **Microsoft.UI.Xaml** fourni par WinUI.

        ![Capture d’écran de Visual Studio montrant le volet Explorateur de solutions et le contenu du fichier MainPage.xaml.cs.](images/WinUI-csharp-appproject.png)

    * **_Nom du projet_ (Package)**  : il s’agit d’un [Projet de création de packages d’applications Windows](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) configuré pour générer l’application dans un [package MSIX](/windows/msix/overview). Vous bénéficiez ainsi d’une expérience de déploiement moderne, de la possibilité de l’intégrer aux fonctionnalités de Windows 10 par le biais d’extensions de package et bien plus encore. Ce projet contient le [manifeste du package](/uwp/schemas/appxpackage/uapmanifestschema/schema-root) pour votre application, et il s’agit du projet de démarrage de votre solution par défaut.

        ![Capture d’écran de Visual Studio montrant le volet Explorateur de solutions et le contenu du fichier Package.appxmanifest.](images/WinUI-csharp-packageproject.png)

7. Pour ajouter un nouvel élément à votre projet d’application, cliquez avec le bouton droit sur le nœud du projet **_Nom du projet_ (Desktop)** dans l’**Explorateur de solutions** et sélectionnez **Ajouter** -> **Nouvel élément**. Dans la boîte de dialogue **Ajouter un nouvel élément**, sélectionnez l’onglet **WinUI**, choisissez l’élément que vous souhaitez ajouter, puis cliquez sur **Ajouter**. Pour plus d’informations sur les éléments disponibles, consultez [cette section](index.md#item-templates-for-winui-3).

    ![Capture d’écran de la boîte de dialogue Ajouter un nouvel élément avec l’option Installé > Éléments Visual C# > WinUI sélectionnée et l’option Page vierge mise en surbrillance.](images/WinUI-csharp-newitem.png)

8. Générez et exécutez votre solution pour vérifier que l’application s’exécute sans erreur.

## <a name="create-a-winui-3-desktop-app-for-cwin32"></a>Créer une application de bureau WinUI 3 pour C++/Win32

1. Dans Visual Studio 2019, sélectionnez **Fichier** -> **Nouveau** -> **Projet**.

2. Dans les filtres déroulants de projet, sélectionnez **C++** , **Windows** et **WinUI**.

3. Sélectionnez le type de projet **Blank App, Packaged (WinUI in Desktop)** , puis cliquez sur **Suivant**.

    ![Autre capture d’écran de l’Assistant Créer un projet avec l’option Blank App, Packaged (WinUI in Desktop) mise en surbrillance.](images/WinUI-cpp-newproject.png)

4. Entrez un nom de projet, choisissez d’autres options si vous le souhaitez, puis cliquez sur **Créer**.

5. Dans la boîte de dialogue suivante, définissez Windows 10, version 1903 (build 18362) comme **Version cible** et Windows 10, version 1803 (build 17134) comme **Version minimale**, puis cliquez sur **OK**.

    ![Version cible et minimale](images/WinUI-min-target-version.png)

6. À ce stade, Visual Studio génère deux projets :

    * **_Nom du projet_ (Desktop)**  : ce projet contient le code de votre application. Le fichier de code **App.xaml.cs** et différents fichiers de code **App** définissent une classe `Application` qui représente l’instance de votre application, et le fichier de code **MainWindow.xaml** et différents fichiers de code **MainWindow** définissent une classe `MainWindow` qui représente la fenêtre principale affichée par votre application. Ces classes dérivent de types dans l’espace de noms **Microsoft.UI.Xaml** fourni par WinUI.

        ![Capture d’écran de Visual Studio montrant le volet Explorateur de solutions et le contenu du fichier MainPage.xaml.](images/WinUI-cpp-appproject.png)

    * **_Nom du projet_ (Package)**  : il s’agit d’un [Projet de création de packages d’applications Windows](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) configuré pour générer l’application dans un [package MSIX](/windows/msix/overview). Vous bénéficiez ainsi d’une expérience de déploiement moderne, de la possibilité de l’intégrer aux fonctionnalités de Windows 10 par le biais d’extensions de package et bien plus encore. Ce projet contient le [manifeste du package](/uwp/schemas/appxpackage/uapmanifestschema/schema-root) pour votre application, et il s’agit du projet de démarrage de votre solution par défaut.

        ![Autre capture d’écran de Visual Studio montrant le volet Explorateur de solutions et le contenu du fichier Package.appxmanifest.](images/WinUI-cpp-packageproject.png)

7. Pour ajouter un nouvel élément à votre projet d’application, cliquez avec le bouton droit sur le nœud du projet **_Nom du projet_ (Desktop)** dans l’**Explorateur de solutions** et sélectionnez **Ajouter** -> **Nouvel élément**. Dans la boîte de dialogue **Ajouter un nouvel élément**, sélectionnez l’onglet **WinUI**, choisissez l’élément que vous souhaitez ajouter, puis cliquez sur **Ajouter**. Pour plus d’informations sur les éléments disponibles, consultez [cette section](index.md#item-templates-for-winui-3).

    ![Nouvel élément](images/WinUI-cpp-newitem.png)

8. Générez et exécutez votre solution pour vérifier que l’application s’exécute sans erreur.

   > [!NOTE]
   > Seul le projet empaqueté est lancé, donc assurez-vous qu’il est défini comme projet de démarrage.

## <a name="localizing-your-winui-desktop-app"></a>Traduction de votre application de bureau WinUI

Pour prendre en charge plusieurs langues dans une application de bureau WinUI et garantir une bonne traduction de votre projet empaqueté, ajoutez les ressources appropriées au projet (voir [Ressources d’application et système de gestion des ressources](/windows/uwp/app-resources/)) et déclarez chaque langue prise en charge dans le fichier `package.appxmanifest` de votre projet. Lorsque vous générez le projet, les langues spécifiées sont ajoutées au manifeste d’application généré (`AppxManifest.xml`) et les ressources correspondantes sont utilisées.

1. Ouvrez le `package.appxmanifest` du .wapproj dans un éditeur de texte et recherchez la section suivante :

    ```xml
    <Resources>
        <Resource Language="x-generate"/>
    </Resources>
    ```

2. Remplacez `<Resource Language="x-generate">` par les éléments `<Resource />` pour chacune de vos langues prises en charge. Par exemple, le balisage suivant spécifie que les ressources traduites « en-US » et « es-ES » sont disponibles :

    ```xml
    <Resources>
        <Resource Language="en-US"/>
        <Resource Language="es-ES"/>
    </Resources>
    ```

## <a name="known-issues-and-limitations"></a>Limitations et problèmes connus

Pour obtenir la liste des problèmes connus et des limitations, consultez [cette section](index.md#preview-3-limitations-and-known-issues).

## <a name="related-topics"></a>Rubriques connexes

* [Bibliothèque d’interface utilisateur Windows 3](index.md)