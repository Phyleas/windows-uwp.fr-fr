---
description: Ce guide vous montre comment commencer à créer des applications UWP avec une interface utilisateur WinUI 3.
title: Bien démarrer avec WinUI 3 pour les applications UWP
ms.date: 11/17/2020
ms.topic: article
keywords: windows 10, uwp, winui
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 5da4944b38fc764eb11bdc2f6daed0cab54ea445
ms.sourcegitcommit: 75e1f49be211e8b4b3e825978d67625776f992f5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94691617"
---
# <a name="get-started-with-winui-3-for-uwp-apps"></a>Bien démarrer avec WinUI 3 pour les applications UWP

WinUI 3 Preview 3 contient de nouveaux modèles de projet qui vous permettent de créer une application de plateforme Windows universelle (UWP) avec une interface utilisateur reposant entièrement sur WinUI. Quand vous créez des applications en utilisant ces modèles de projet, la totalité de l’interface utilisateur de votre application est implémentée à l’aide de fenêtres, de contrôles et de styles fournis par WinUI 3. Pour obtenir la liste complète des modèles de projet WinUI 3 pris en charge, consultez [Modèles de projet pour WinUI 3](index.md#project-templates-for-winui-3).

## <a name="prerequisites"></a>Prérequis

Pour utiliser les modèles de projet WinUI 3 pour les applications UWP décrits dans cet article, configurez votre ordinateur de développement et [installez WinUI 3 Preview 3](index.md#install-winui-3-preview-3).

## <a name="create-a-winui-3-app-in-uwp-for-c"></a>Créer une « application WinUI 3 dans UWP » pour C#

1. Utilisez Visual Studio 2019 pour créer un projet.
   - Si Visual Studio est déjà en cours d’exécution, sélectionnez **Fichier** -> **Nouveau** -> **Projet**.

   :::image type="content" source="images/WinUI-and-UWP/vs2019-menu-file-new-project.png" alt-text="Visual Studio 2019 - Fichier > Nouveau -> Menu projet":::

   - Dans le cas contraire, lancez Visual Studio, puis sélectionnez **Créer un projet**.

   :::image type="content" source="images/WinUI-and-UWP/vs2019-splash-new-project.png" alt-text="Visual Studio 2019 - Créer un projet":::

2. Dans la boîte de dialogue **Créer un projet**, sélectionnez respectivement **C#** , **Windows** et **WinUI** dans les filtres déroulants de projet.

3. Sélectionnez le type de projet **Application vide (WinUI dans UWP)** , puis cliquez sur **Suivant**.

:::image type="content" source="images/WinUI-and-UWP/vs2019-create-new-project-dialog.png" alt-text="Visual Studio 2019 - Boîte de dialogue Créer un projet":::

4. Entrez un nom de projet, choisissez d’autres options si vous le souhaitez, puis cliquez sur **Créer**.

:::image type="content" source="images/WinUI-and-UWP/vs2019-configure-new-project-dialog.png" alt-text="Capture d’écran de la boîte de dialogue Configurer votre nouveau projet, avec la zone de texte Emplacement et l’option Créer mise en évidence.":::

5. Dans la boîte de dialogue suivante, définissez Windows 10, version 1903 (build 18362) comme **Version cible** et Windows 10, version 1803 (build 17134) comme **Version minimale**, puis cliquez sur **OK**.

:::image type="content" source="images/WinUI-min-target-version.png" alt-text="Boîte de dialogue Version cible et minimale":::

6. Visual Studio génère le projet **WinUI dans UWP** avec les objets suivants :

    - **_Nom du projet_ (Windows universel)**  : contient le code de votre application. Il s’agit du projet de démarrage par défaut pour votre solution de projet.

    :::image type="content" source="images/WinUI-and-UWP/vs2019-project.png" alt-text="Capture d’écran du panneau Explorateur de solutions, avec la solution Windows universelle mise en évidence.":::

    - **Package.appxmanifest** : contient les informations dont le système a besoin pour déployer, afficher ou mettre à jour votre application. Pour plus d’informations, consultez [Manifeste du package d’application](/uwp/schemas/appxpackage/appx-package-manifest).

    :::image type="content" source="images/WinUI-and-UWP/vs2019-file-package-manifest.png" alt-text="Visual Studio 2019 - Manifeste du package d’application":::

    - **App.xaml/App.xaml.cs** : fichiers de code définissant la classe `Application`, qui représente l’instance de votre application.

    :::image type="content" source="images/WinUI-and-UWP/vs2019-file-app-xaml.png" alt-text="Visual Studio 2019 - Fichier App.xaml":::

    :::image type="content" source="images/WinUI-and-UWP/vs2019-file-app-xaml-cs.png" alt-text="Visual Studio 2019 - Fichier App.xaml.cs":::

    - **MainPage.xaml/MainPage.xaml.cs** : fichiers de code qui représentent les fenêtres principales affichées par votre application. Ces classes dérivent de types dans l’espace de noms **Microsoft.UI.Xaml** fourni par WinUI.

    :::image type="content" source="images/WinUI-and-UWP/vs2019-file-mainpage-xaml.png" alt-text="Visual Studio 2019 - Fichier MainPage.xaml":::

    :::image type="content" source="images/WinUI-and-UWP/vs2019-file-mainpage-xaml-cs.png" alt-text="Visual Studio 2019 - Fichier MainPage.xaml.cs":::

7. Pour ajouter un nouvel élément à votre projet d’application, cliquez avec le bouton droit sur le nœud de projet **_Nom du projet_ (Windows universel)** dans l’**Explorateur de solutions**, puis sélectionnez **Ajouter** -> **Nouvel élément**. Dans la boîte de dialogue **Ajouter un nouvel élément**, sélectionnez l’onglet **WinUI**, choisissez l’élément que vous souhaitez ajouter, puis cliquez sur **Ajouter**. Pour plus d’informations sur les éléments disponibles, consultez [Modèles d’élément pour WinUI 3](index.md#item-templates-for-winui-3).

    :::image type="content" source="images/WinUI-and-UWP/vs2019-add-new-item-dialog.png" alt-text="Visual Studio 2019 - Boîte de dialogue Ajouter un nouvel élément":::

8. Générez, déployez et lancez votre application pour voir à quoi elle ressemble.

    1. Vous pouvez déboguer votre application sur l’ordinateur local, dans un simulateur ou un émulateur, ou sur un appareil distant. Sélectionnez votre appareil cible dans la liste déroulante.

        :::image type="content" source="images/WinUI-and-UWP/vs2019-menu-target-device.png" alt-text="Capture d’écran de la liste déroulante Ordinateur local.":::

    1. Appuyez sur F5, cliquez sur le bouton **Générer**, ou sélectionnez **Déboguer -> Démarrer le débogage** pour générer et exécuter votre solution, et vérifier que l’application s’exécute sans erreur.

        :::image type="content" source="images/WinUI-and-UWP/vs2019-project-running.png" alt-text="Capture d’écran de l’application en cours d’exécution, avec le bouton Cliquer ici.":::

## <a name="known-issues-and-limitations"></a>Limitations et problèmes connus

Pour obtenir la liste des problèmes connus et des limitations, consultez [cette section](index.md#preview-3-limitations-and-known-issues).

## <a name="related-topics"></a>Rubriques connexes

- [WinUI 3](index.md)
- [Créer votre première application](/windows/uwp/get-started/your-first-app)