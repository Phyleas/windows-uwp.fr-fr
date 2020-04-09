---
description: Ce tutoriel montre comment ajouter des interfaces utilisateur XAML UWP, créer des packages MSIX et incorporer d’autres composants modernes dans votre application WPF.
title: 'Tutoriel : Moderniser une application WPF'
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, xaml islands, îles xaml
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 21049c995d467209b22fe8ea5c40d303911f2c2c
ms.sourcegitcommit: 0a319e2e69ef88b55d472b009b3061a7b82e3ab1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/21/2020
ms.locfileid: "77521281"
---
# <a name="tutorial-modernize-a-wpf-app"></a>Tutoriel : Moderniser une application WPF 

Il y a de nombreuses façons de [moderniser](index.md) les applications de bureau existantes en intégrant les dernières fonctionnalités Windows dans le code source existant au lieu de réécrire les applications à partir de zéro. Dans ce tutoriel, nous allons découvrir plusieurs manières de moderniser une application métier WPF existante à l’aide des fonctionnalités suivantes :

* .NET Core 3
* Contrôles XAML UWP avec XAML Islands
* Cartes adaptatives et notifications Windows 10
* Déploiement de MSIX

Ce tutoriel requiert les compétences de développement suivantes :

* Expérience dans le développement d’applications de bureau Windows avec WPF.
* Connaissances de base de C# et de XAML.
* Connaissances de base d’UWP.

## <a name="overview"></a>Vue d’ensemble

Ce tutoriel fournit le code pour une application métier WPF simple nommée Contoso Expenses. Dans le scénario fictif du tutoriel, Contoso Expenses est une application interne utilisée par les responsables de Contoso Corporation pour suivre les dépenses soumises par leurs rapports. Les responsables sont désormais équipés d’appareils tactiles et souhaitent utiliser l’application Contoso Expenses sans clavier ni souris. Malheureusement, dans la version actuelle les fonctions tactiles ne sont pas optimisées.

Contoso souhaite moderniser cette application avec de nouvelles fonctionnalités Windows pour permettre aux employés de créer plus efficacement des rapports de dépenses. La plupart des fonctionnalités peuvent être facilement implémentées en créant une nouvelle application UWP. Toutefois, l’application existante est complexe et est le résultat de nombreuses années de développement par différentes équipes. La réécriture à partir de zéro avec une nouvelle technologie n’est donc pas une option. L’équipe recherche la meilleure approche pour ajouter de nouvelles fonctionnalités à la base de code existante.

Au début du tutoriel, Contoso Expenses cible .NET Framework 4.7.2 et utilise les bibliothèques tierces suivantes :

* MVVM Light, une implémentation de base pour le modèle MVVM.
* Unity, un conteneur d'injection de dépendances.
* LiteDb, une solution NoSQL incorporée pour stocker les données.
* Bogus, un outil servant à générer des données fictives.

Dans ce tutoriel, vous allez améliorer les Contoso Expenses avec de nouvelles fonctionnalités de Windows :

* Migrer une application WPF existante vers .NET Core 3.0. Ceci ouvrira de nouvelles perspectives importantes à l’avenir.
* Utilisez XAML Islands pour héberger les contrôles inclus dans un wrapper **InkCanvas** et **MapControl** fournis dans le kit de ressources de la communauté Windows.
* Utilisez XAML Islands pour héberger n’importe quel contrôle XAML UWP standard (dans ce cas, **CalendardView**).
* Intégrer des cartes adaptatives et des notifications Windows 10 dans l’application.
* Créez un package de l’application avec MSIX et configurez un pipeline CI/CD sur Azure DevOps afin de pouvoir livrer automatiquement de nouvelles versions de l’application aux testeurs et aux utilisateurs dès qu’elles sont disponibles.

## <a name="prerequisites"></a>Prérequis

Pour suivre ce tutoriel, les composants requis suivants doivent être installés sur votre ordinateur de développement :

* Windows 10, version 1903 (build 18362) ou version ultérieure.
* [Visual Studio 2019](https://www.visualstudio.com).
* [.NET Core 3 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) (installez la dernière version).

Veillez à installer les charges de travail et les fonctionnalités facultatives suivantes avec Visual Studio 2019 :

* Développement .NET Desktop
* Développement pour la plateforme Windows universelle
* Kit de développement logiciel (SDK) Windows 10 (10.0.18362.0 ou version ultérieure)

## <a name="get-the-contoso-expenses-sample-app"></a>Télécharger l’exemple d’application Contoso Expenses

Avant de commencer le tutoriel, téléchargez le code source de l’application Contoso Expenses et assurez-vous que vous pouvez créer le code dans Visual Studio.

1. Téléchargez le code source de l’application à partir de l’onglet **Mises en production** du [référentiel d’atelier AppConsult WinAppsModernization](https://github.com/Microsoft/AppConsult-WinAppsModernizationWorkshop). Le lien direct est [https://github.com/microsoft/AppConsult-WinAppsModernizationWorkshop/releases](https://github.com/microsoft/AppConsult-WinAppsModernizationWorkshop/releases).
2. Ouvrez le fichier zip et extrayez tout le contenu vers la racine de votre lecteur **C :\\** . Un dossier nommé **C:\WinAppsModernizationWorkshop** est alors créé.
3. Ouvrez Visual Studio 2019 et double-cliquez sur le fichier **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses\ContosoExpenses.sln** pour ouvrir la solution.
4. Vérifiez que vous pouvez créer, exécuter et déboguer le projet WPF Contoso Expenses en appuyant sur le bouton **Démarrer** ou sur CTRL + F5.

## <a name="get-started"></a>Prendre en main

Une fois que vous disposez du code source de l’exemple d’application Contoso Expenses et que vous pouvez le générer dans Visual Studio, vous êtes prêt à commencer le tutoriel :

* [Partie 1 : Effectuer une migration de l'application Contoso Expenses vers .NET Core 3](modernize-wpf-tutorial-1.md)
* [Partie 2 : Ajouter un contrôle InkCanvas UWP avec XAML Islands](modernize-wpf-tutorial-2.md)
* [Partie 3 : Ajouter un contrôle CalendarView UWP avec XAML Islands](modernize-wpf-tutorial-3.md)
* [Partie 4 : Ajouter des activités et des notifications utilisateur Windows 10](modernize-wpf-tutorial-4.md)
* [Partie 5 : Créer un package et déployer avec MSIX](modernize-wpf-tutorial-5.md)

## <a name="important-concepts"></a>Concepts importants

Les sections suivantes fournissent des informations générales sur certains des principaux concepts abordés dans ce tutoriel. Si vous êtes déjà familiarisé avec ces concepts, vous pouvez ignorer cette section.

### <a name="universal-windows-platform-uwp"></a>Plateforme Windows universelle (UWP)

Dans Windows 8, Microsoft a introduit un nouveau framework appelé Windows Runtime (WinRT). Contrairement à .NET Framework, WinRT est une couche native d’API qui sont exposées directement aux applications. WinRT a également introduit des projections de langage, qui sont des couches ajoutées au runtime pour permettre aux développeurs d’interagir avec lui à l’aide de langages tels que C# et JavaScript en plus de C++. Les projections permettent aux développeurs de créer des applications qui s’ajoutent à WinRT et tirent parti des mêmes connaissances C# et XAML acquises lors de la création d’applications avec .NET Framework. 

Dans Windows 10, Microsoft a introduit la [plateforme Windows universelle (UWP)](/windows/uwp/get-started/universal-application-platform-guide), qui repose sur WinRT. La fonctionnalité la plus importante d’UWP est qu’elle offre un ensemble commun d’API sur toutes les plateformes d’appareils : que l’application s’exécute sur un ordinateur de bureau, sur une Xbox ou sur un HoloLens, vous pouvez utiliser les mêmes API.

À l’avenir, la plupart des nouvelles fonctionnalités de Windows 10 seront exposées via des API WinRT, notamment les fonctionnalités telles que Timeline, Project Rome et Windows Hello.

### <a name="msix-packaging"></a>Création de packages MSIX

[MSIX](/windows/msix/) est le modèle de création de packages pour les applications Windows. MSIX prend en charge les applications UWP ainsi que la création d’applications de bureau à l’aide de technologies telles que Win32, WPF, Windows Forms, Java et Electron, entre autres. Quand vous créez un package MSIX d’une application de bureau, vous pouvez publier votre application sur le Microsoft Store. Votre application de bureau obtient également l’identité du package lors de son installation, ce qui lui permet d’utiliser un ensemble plus large d’API WinRT.

Pour plus d’informations, consultez ces articles :

* [Créer des packages d’applications de bureau](/windows/uwp/porting/desktop-to-uwp-root)
* [Dans les coulisses de votre application de bureau packagée](/windows/uwp/porting/desktop-to-uwp-behind-the-scenes)

### <a name="xaml-islands"></a>XAML Islands

À compter de Windows 10 version 1903, vous pouvez héberger des contrôles UWP dans des applications de bureau non-UWP à l’aide d’une fonctionnalité appelée *XAML Islands*. Vous pouvez ainsi améliorer l’apparence, le comportement et les fonctionnalités de vos applications de bureau existantes avec les dernières nouveautés de l’interface utilisateur Windows 10, qui sont uniquement disponibles par le biais de contrôles UWP. Cela signifie que vous pouvez utiliser des fonctionnalités UWP telles que Windows Ink et des contrôles prenant en charge le système Fluent Design dans vos applications WPF, Windows Forms et Win32 C++ existantes.

Pour plus d’informations, consultez [Contrôles UWP dans les applications de bureau (XAML Islands)](/windows/uwp/xaml-platform/xaml-host-controls). Ce tutoriel vous guide tout au long du processus d’utilisation de deux types différents de contrôles XAML Island :

* Contrôles [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) et [MapControl](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) dans le kit de ressources de la communauté Windows. Ces contrôles WPF incluent l’interface et les fonctionnalités des contrôles UWP correspondants dans un wrapper et peuvent être utilisés comme n’importe quel autre contrôle WPF dans le concepteur Visual Studio.

* Contrôle [Affichage du calendrier](/windows/uwp/design/controls-and-patterns/calendar-view). Il s’agit d’un contrôle UWP standard que vous allez héberger à l’aide du contrôle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) dans le kit de ressources de la communauté Windows.

### <a name="net-core-3"></a>.NET Core 3

[.NET Core](https://docs.microsoft.com/dotnet/core/) est un framework open source qui implémente une version multiplateforme, légère et facilement extensible de l’intégralité de .NET Framework. Par rapport à l’intégralité de .NET Framework, le temps de démarrage de .NET Core est beaucoup plus rapide et la plupart des API ont été optimisées.

Dès les premières versions, l’objectif de .NET Core a été de prendre en charge les applications web ou principales. Avec .NET Core, vous pouvez facilement créer des applications web évolutives ou des API qui peuvent être hébergées sur Windows, Linux, ou dans des architectures de microservices telles que des conteneurs Docker.

.NET Core 3 est la dernière version de .NET Core. La grande nouveauté de cette version est la prise en charge des applications de bureau Windows, notamment les applications Windows Forms et WPF. Vous pouvez exécuter vos applications de bureau Windows, nouvelles et existantes, sur .NET Core 3 et tirer pleinement parti de cette nouvelle version de .NET Core. De plus, vous pourrez utiliser les contrôles UWP hébergés dans [XAML Islands](xaml-islands.md) dans toutes les applications Windows Forms et WPF qui ciblent .NET Core 3.

> [!NOTE]
> WPF et Windows Forms ne deviennent pas des multiplateformes, et vous ne pouvez pas exécuter WPF ou Windows Forms sur Linux et MacOS. Les composants de l’interface utilisateur de WPF et de Windows Forms ont toujours une dépendance sur le système de rendu Windows.

Pour plus d’informations, consultez [Nouveautés de .NET Core 3.0](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0).
