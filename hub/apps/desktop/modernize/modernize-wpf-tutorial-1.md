---
description: Ce tutoriel montre comment ajouter des interfaces utilisateur XAML UWP, créer des packages MSIX et incorporer d’autres composants modernes dans votre application WPF.
title: Effectuer une migration de l'application Contoso Expenses vers .NET Core 3
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, xaml islands, îles xaml
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: c11f1cab37e79fc320f1fb38f5b909d2cecd1ad4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161583"
---
# <a name="part-1-migrate-the-contoso-expenses-app-to-net-core-3"></a>Partie 1 : Effectuer une migration de l'application Contoso Expenses vers .NET Core 3

Il s’agit de la première partie d’un tutoriel qui montre comment moderniser un exemple d’application de bureau WPF nommée Contoso Expenses. Pour obtenir une vue d’ensemble du tutoriel, des prérequis et des instructions pour le téléchargement de l’exemple d’application, consultez [Tutoriel : Moderniser une application WPF](modernize-wpf-tutorial.md).
  
Dans cette partie du tutoriel, vous allez migrer l’intégralité de l’application Contoso Expenses de .NET Framework 4.7.2 vers [.NET Core 3](modernize-wpf-tutorial.md#net-core-3). Avant de commencer cette partie du tutoriel, veillez à [ouvrir et générer l’exemple ContosoExpenses](modernize-wpf-tutorial.md#get-the-contoso-expenses-sample-app) dans Visual Studio 2019.

> [!NOTE]
> Pour plus d’informations sur la migration d’une application WPF du .NET Framework vers .NET Core 3, consultez [cette série de blogs](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/).

## <a name="migrate-the-contosoexpenses-project-to-net-core-3"></a>Migrer le projet ContosoExpenses vers .NET Core 3

Dans cette section, vous allez migrer le projet ContosoExpenses de l’application Contoso Expenses vers .NET Core 3. Pour ce faire, vous devez créer un fichier projet qui contient les mêmes fichiers que le projet ContosoExpenses existant, mais qui cible .NET Core 3 au lieu de .NET Framework 4.7.2. Cela vous permet de gérer une solution unique avec les versions .NET Framework et .NET Core de l’application.

1. Vérifiez que le projet ContosoExpenses cible actuellement .NET Framework 4.7.2. Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet **ContosoExpenses**, sélectionnez **Propriétés**, puis vérifiez que la propriété **Framework cible** de l’onglet **Application** a pour valeur .NET Framework 4.7.2.

    ![.NET Framework version 4.7.2 pour le projet](images/wpf-modernize-tutorial/NETFramework472.png)

3. Dans l’Explorateur Windows, accédez au dossier **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses** et créez un fichier texte nommé **ContosoExpenses.Core.csproj**.

4. Cliquez avec le bouton droit sur le fichier, choisissez **Ouvrir avec**, puis ouvrez-le dans un éditeur de texte de votre choix, tel que le Bloc-notes, Visual Studio Code ou Visual Studio.

5. Copiez le texte suivant dans le fichier et enregistrez-le.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">

      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFramework>netcoreapp3.0</TargetFramework>
        <UseWPF>true</UseWPF>
     </PropertyGroup>

    </Project>
    ```

6. Fermez le fichier et revenez à la solution **ContosoExpenses** dans Visual Studio.

7. Cliquez avec le bouton droit sur la solution **ContosoExpenses**, puis choisissez **Ajouter -> Projet existant**. Sélectionnez le fichier **ContosoExpenses.Core.csproj** que vous venez de créer dans le dossier `C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses` pour l’ajouter à la solution.

**ContosoExpenses.Core.csproj** comprend les éléments suivants :

* L’élément **Project** spécifie une version du SDK de **Microsoft.NET.Sdk.WindowsDesktop**. Cela fait référence aux applications .NET pour Windows Desktop et comprend des composants pour les applications WPF et Windows Forms.
* L’élément **PropertyGroup** contient des éléments enfants qui indiquent que la sortie du projet est un fichier exécutable (et non une DLL), qui cible .NET Core 3 et utilise WPF. Pour une application Windows Forms, vous devez utiliser un élément **UseWinForms** au lieu de l’élément **UseWPF**.

> [!NOTE]
> Lorsque vous utilisez le format. csproj introduit avec .NET Core 3.0, tous les fichiers dans le même dossier que le fichier. csproj sont considérés comme faisant partie du projet. Par conséquent, vous n’avez pas à spécifier chaque fichier inclus dans le projet. Vous devez spécifier uniquement les fichiers pour lesquels vous voulez définir une action de génération personnalisée ou que vous souhaitez exclure.

## <a name="migrate-the-contosoexpensesdata-project-to-net-standard"></a>Migrer le projet ContosoExpenses.Data vers .NET Standard

La solution **ContosoExpenses** comprend une bibliothèque de classes **ContosoExpenses.Data** qui contient des modèles et des interfaces pour les services et qui cible .NET 4.7.2. Les applications .NET Core 3.0 peuvent utiliser des bibliothèques .NET Framework, à condition qu’elles n’utilisent pas les API qui ne sont pas disponibles dans .NET Core. Toutefois, le meilleur moyen de se moderniser consiste à déplacer vos bibliothèques vers .NET Standard. Vous vous assurez ainsi que votre bibliothèque est entièrement prise en charge par votre application .NET Core 3.0. De plus, vous pouvez également réutiliser la bibliothèque avec d’autres plateformes, telles que web (via ASP.NET Core) et mobile (via Xamarin).

Pour migrer le projet **ContosoExpenses.Data** vers .NET Standard :

1. Dans Visual Studio, cliquez avec le bouton droit sur le projet **ContosoExpenses.Data** et choisissez **Décharger le projet**. Cliquez à nouveau avec le bouton droit sur le projet, puis choisissez **Modifier ContosoExpenses.Data.csproj**.

2. Supprimez tout le contenu du fichier projet.

3. Copiez et collez le code XML suivant, et enregistrez le fichier.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">

      <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
      </PropertyGroup>

    </Project>
    ```

4. Cliquez avec le bouton droit sur le projet **ContosoExpenses.Data** et choisissez **Recharger le projet**.

## <a name="configure-nuget-packages-and-dependencies"></a>Configurer les packages NuGet et les dépendances

Quand vous avez migré les projets **ContosoExpenses.Core** et **ContosoExpenses.Data** dans les sections précédentes, vous avez supprimé les références des packages NuGet des projets. Dans cette section, vous allez rajouter ces références.

Pour configurer les packages NuGet pour le projet **ContosoExpenses.Data** :

1. Dans le projet **ContosoExpenses.Data**, développez le nœud **Dépendances**. Notez que la section **NuGet** est manquante.

    ![Packages NuGet](images/wpf-modernize-tutorial/NuGetPackages.png)

    Si vous ouvrez **Packages.config** dans l’**Explorateur de solutions**, vous trouverez les anciennes références des packages NuGet qui utilisaient le projet lorsqu’il se servait du .NET Framework complet.

    ![Dépendances et packages](images/wpf-modernize-tutorial/Packages.png)

    Voici le contenu du fichier **Packages.config**. Vous remarquerez que tous les packages NuGet ciblent .NET Framework 4.7.2 dans son intégralité :

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="Bogus" version="26.0.2" targetFramework="net472" />
      <package id="LiteDB" version="4.1.4" targetFramework="net472" />
    </packages>
    ```

2. Dans le projet **ContosoExpenses.Data**, supprimez le fichier **Packages.config**.

4. Dans le projet **ContosoExpenses.Data**, cliquez avec le bouton droit sur le nœud **Dépendances**, puis choisissez **Gérer les packages NuGet**.

  ![Gérer les packages NuGet...](images/wpf-modernize-tutorial/ManageNugetNETCORE3.png)

5. Dans la fenêtre **Gestionnaire de package NuGet**, cliquez sur **Parcourir**. Recherchez le package `Bogus` et installez la dernière version stable.

    ![Package NuGet Bogus](images/wpf-modernize-tutorial/Bogus.png)

6. Recherchez le package `LiteDB` et installez la dernière version stable.

    ![Package NuGet LiteDB](images/wpf-modernize-tutorial/LiteDB.png)

    Vous vous demandez peut-être où cette liste de packages NuGet est stockée, car le projet n’a plus de fichier packages.config. Les packages NuGet référencés sont stockés directement dans le fichier .csproj. Vous pouvez le vérifier en affichant le contenu du fichier projet **ContosoExpenses.Data.csproj** dans un éditeur de texte. Les lignes suivantes sont ajoutées à la fin du fichier :

    ```xml
    <ItemGroup>
       <PackageReference Include="Bogus" Version="26.0.2" />
       <PackageReference Include="LiteDB" Version="4.1.4" />
    </ItemGroup>
    ```

    > [!NOTE]
    > Vous pouvez également remarquer que vous installez les mêmes packages pour ce projet .NET Core 3 que ceux utilisés par les projets NET Framework 4.7.2. Les packages NuGet prennent en charge le multi-ciblage. Les auteurs de bibliothèques peuvent inclure différentes versions d’une bibliothèque dans le même package, compilées pour diverses architectures et plateformes. Ces packages prennent en charge le .NET Framework complet ainsi que .NET Standard 2.0, qui est compatible avec les projets .NET Core 3. Pour plus d’informations sur les différences entre .NET Framework, .NET Core et .NET Standard, consultez [.NET Standard](/dotnet/standard/net-standard).

Pour configurer les packages NuGet pour le projet **ContosoExpenses.Core** :

1. Dans le projet **ContosoExpenses.Core**, ouvrez le fichier **packages.config**. Notez qu’il contient actuellement les références suivantes qui ciblent .NET Framework 4.7.2.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="CommonServiceLocator" version="2.0.2" targetFramework="net472" />
      <package id="MvvmLightLibs" version="5.4.1.1" targetFramework="net472" />
      <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.2" targetFramework="net472" />
      <package id="Unity" version="5.10.2" targetFramework="net472" />
    </packages>
    ```

    Dans les étapes suivantes, vous allez utiliser les versions .NET Standard des packages `MvvmLightLibs` et `Unity`. Les deux autres sont des dépendances automatiquement téléchargées par NuGet lors de l’installation de ces deux bibliothèques.

2. Dans le projet **ContosoExpenses.Core**, supprimez le fichier **Packages.config**.

3. Cliquez avec le bouton droit sur le projet **ContosoExpenses.Core**, puis choisissez **Gérer les packages NuGet**.

4. Dans la fenêtre **Gestionnaire de package NuGet**, cliquez sur **Parcourir**. Recherchez le package `Unity` et installez la dernière version stable.

    ![Package Unity](images/wpf-modernize-tutorial/UnityPackage.png)

5. Recherchez le package `MvvmLightLibsStd10` et installez la dernière version stable. Il s’agit de la version .NET Standard du package `MvvmLightLibs`. Pour ce package, l’auteur a choisi d’empaqueter la version .NET Standard de la bibliothèque dans un package distinct de celui de la version .NET Framework.

    ![Package MvvmLightsLibs](images/wpf-modernize-tutorial/MvvmLightsLibsPackage.png)

6. Dans le projet **ContosoExpenses.Core**, cliquez avec le bouton droit sur le nœud **Dépendances**, puis choisissez **Ajouter une référence**.

7. Dans la catégorie **Projets > Solution**, sélectionnez **ContosoExpenses.Data**, puis cliquez sur **OK**.

    ![Ajouter une référence](images/wpf-modernize-tutorial/AddReference.png)

## <a name="disable-auto-generated-assembly-attributes"></a>Désactiver les attributs d’assembly générés automatiquement

À ce stade du processus de migration, si vous essayez de générer le projet **ContosoExpenses.Core**, vous verrez des erreurs.

![Nouvelles erreurs de génération .NET Core 3](images/wpf-modernize-tutorial/NETCORE3BuildNewErrors.png)

Ce problème se pose parce que le nouveau format .csproj introduit avec NET Core 3.0 stocke les informations de l’assembly dans le fichier projet plutôt que dans le fichier **AssemblyInfo.cs**. Pour corriger ces erreurs, désactivez ce comportement et laissez le projet continuer à utiliser le fichier **AssemblyInfo.cs**.

1. Dans Visual Studio, cliquez avec le bouton droit sur le projet **ContosoExpenses.Core** et choisissez **Décharger le projet**. Cliquez à nouveau avec le bouton droit sur le projet, puis choisissez **Modifier ContosoExpenses.Core.csproj**.

1. Ajoutez l’élément suivant dans la section **PropertyGroup** et enregistrez le fichier.

    ```XML
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    Après avoir ajouté cet élément, la section **PropertyGroup** doit à présent ressembler à ceci :

    ```XML
    <PropertyGroup>
      <OutputType>WinExe</OutputType>
      <TargetFramework>netcoreapp3.0</TargetFramework>
      <UseWPF>true</UseWPF>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

3. Cliquez avec le bouton droit sur le projet **ContosoExpenses.Core** et choisissez **Recharger le projet**.

4. Cliquez avec le bouton droit sur le projet **ContosoExpenses.Data** et choisissez **Décharger le projet**. Cliquez à nouveau avec le bouton droit sur le projet, puis choisissez **Modifier ContosoExpenses.Data.csproj**.

5. Ajoutez la même entrée dans la section **PropertyGroup** et enregistrez le fichier.

    ```xml
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    Après avoir ajouté cet élément, la section **PropertyGroup** doit à présent ressembler à ceci :

    ```xml
    <PropertyGroup>
      <TargetFramework>netstandard2.0</TargetFramework>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

6. Cliquez avec le bouton droit sur le projet **ContosoExpenses.Data** et choisissez **Recharger le projet**.

## <a name="add-the-windows-compatibility-pack"></a>Ajouter le Pack de compatibilité Windows

Si vous essayez maintenant de compiler les projets **ContosoExpenses.Core** et **ContosoExpenses.Data**, vous verrez que les erreurs précédentes sont résolues, mais qu’il existe toujours des erreurs semblables dans la bibliothèque **ContosoExpenses.Data**.

`Services\RegistryService.cs(9,26,9,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,26,12,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,97,12,123): error CS0103: The name 'RegistryKeyPermissionCheck' does not exist in the current context`

Ces erreurs sont le résultat de la conversion du projet **ContosoExpenses.Data** à partir d’une bibliothèque .NET Framework (qui est spécifique pour Windows) vers une bibliothèque .NET Standard, qui peut s’exécuter sur plusieurs plateformes, notamment Linux, Android, iOS, etc. Le projet **ContosoExpenses.Data** contient une classe appelée **RegistryService**, qui interagit avec le Registre, un concept propre à Windows.

Pour résoudre ces erreurs, installez le package NuGet [Compatibilité Windows](https://www.nuget.org/packages/Microsoft.Windows.Compatibility). Ce package assure la prise en charge de nombreuses API spécifiques à Windows à utiliser dans une bibliothèque .NET Standard. La bibliothèque ne sera plus multiplateforme après l’utilisation de ce package, mais elle ciblera toujours .NET Standard. 

1. Cliquez avec le bouton droit sur le projet **ContosoExpenses.Data**.
2. Choisissez **Gérer les packages NuGet**.
3. Dans la fenêtre **Gestionnaire de package NuGet**, cliquez sur **Parcourir**. Recherchez le package `Microsoft.Windows.Compatibility` et installez la dernière version stable.

    ![Installer le package NuGet](images/wpf-modernize-tutorial/WindowsCompatibilityPack.png)

4. Maintenant, réessayez de compiler le projet, en cliquant avec le bouton droit sur le projet **ContosoExpenses.Data** et en choisissant **Générer**.

Cette fois, le processus de génération se termine sans erreurs.

## <a name="test-and-debug-the-migration"></a>Tester et déboguer la migration

Maintenant que les projets sont correctement générés, vous êtes prêt à exécuter et à tester l’application pour voir s’il existe des erreurs d’exécution.

1. Cliquez avec le bouton droit sur le projet **ContosoExpenses.Core**, puis choisissez **Définir comme projet de démarrage**.

2. Appuyez sur F5 pour démarrer le projet **ContosoExpenses.Core** dans le débogueur. Une exception semblable à la suivante s’affiche.

    ![Exception affichée dans Visual Studio](images/wpf-modernize-tutorial/ExceptionNETCore3.png)

    Cette exception est levée car, lorsque vous avez supprimé le contenu du fichier .csproj au début de la migration, vous avez supprimé les informations relatives à l’**action de génération** pour les fichiers image. Les étapes suivantes résolvent ce problème.

3. Arrêtez le débogueur.

4. Cliquez avec le bouton droit sur le projet **ContosoExpenses.Core** et choisissez **Décharger le projet**. Cliquez à nouveau avec le bouton droit sur le projet, puis choisissez **Modifier ContosoExpenses.Core.csproj**.

5. Avant l’élément **Project** de fermeture, ajoutez l’entrée suivante :

    ```xml
    <ItemGroup>
      <Content Include="Images/*">
        <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
      </Content>
    </ItemGroup>
    ```

6. Cliquez avec le bouton droit sur le projet **ContosoExpenses.Core** et choisissez **Recharger le projet**.

7. Pour affecter Contoso.ico à l’application, cliquez avec le bouton droit sur le projet **ContosoExpenses.Core** et choisissez **Propriétés**. Dans la page ouverte, cliquez sur la liste déroulante sous **Icône** et sélectionnez `Images\contoso.ico`.

    ![Icône Contoso dans les propriétés du projet](images/wpf-modernize-tutorial/ContosoIco.png)

8. Cliquez sur **Enregistrer**.

9. Appuyez sur F5 pour démarrer le projet **ContosoExpenses.Core** dans le débogueur. Vérifiez que l’application s’exécute maintenant.

## <a name="next-steps"></a>Étapes suivantes

À ce stade du tutoriel, vous avez réussi à migrer l’application Contoso Expenses vers .NET Core 3. Vous êtes maintenant prêt à passer à la [Partie 2 : Ajouter un contrôle InkCanvas UWP avec XAML Islands](modernize-wpf-tutorial-2.md).

> [!NOTE]
> Si vous avez un écran haute résolution, vous remarquerez peut-être que l’application semble très petite. Vous allez résoudre ce problème à l’étape suivante du tutoriel.