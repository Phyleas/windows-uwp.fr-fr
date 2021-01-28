---
description: Créer un composant Windows Runtime avec C#/WinRT et le consommer à partir d’une application native
title: Créer un composant/WinRT C# et le consommer à partir de C++/WinRT
ms.date: 01/28/2021
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d489e293ca40aa38c27e4c3e19bba6f8a6705e3b
ms.sourcegitcommit: 6f15cc14e0c4c13999c862664fa7a70de8730b74
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98987092"
---
# <a name="walkthrough-create-a-cwinrt-component-and-consume-it-from-cwinrt"></a>Procédure pas à pas : créer un composant/WinRT C# et le consommer à partir de C++/WinRT

> [!NOTE]
> La prise en charge de la création de/WinRT C# décrite dans cet article est actuellement en version préliminaire à partir de C#/WinRT version 1.1.1. À partir de cette version, il est destiné à être utilisé uniquement pour les commentaires et l’évaluation.

C#/WinRT permet aux développeurs .NET 5 de créer leurs propres composants Windows Runtime en C# à l’aide d’un projet de bibliothèque de classes. Les composants créés peuvent être utilisés dans les applications de bureau natives avec une référence de package ou avec une référence de fichier **. winmd** .

Cette procédure pas à pas montre comment vous pouvez utiliser C#/WinRT pour créer vos propres types de Windows Runtime, les empaqueter en tant que composant Windows Runtime et utiliser le composant à partir d’une application console C++/WinRT.

Lors de la création de votre composant d’exécution, suivez les instructions et les restrictions de type décrites dans [cet article](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md) en interne, les types de Windows Runtime dans votre composant peuvent utiliser toutes les fonctionnalités .net autorisées dans une application UWP. Pour plus d’informations, consultez [.net pour les applications UWP](/dotnet/api/index?view=dotnet-uwp-10.0&preserve-view=true). En externe, les membres de votre type peuvent exposer uniquement les types de Windows Runtime pour leurs paramètres et valeurs de retour.

> [!NOTE]
> Certains types [de Windows Runtime mappés aux types .net](../winrt-components/net-framework-mappings-of-windows-runtime-types.md#uwp-types-that-map-to-net-types-with-a-different-name-andor-namespace). Ces types .NET peuvent être utilisés dans l’interface publique de votre composant Windows Runtime et s’affichent pour les utilisateurs du composant en tant que types de Windows Runtime correspondants.

## <a name="prerequisites"></a>Prérequis

Cette procédure pas à pas nécessite les outils et composants suivants :

- Visual Studio 2019
- SDK .NET 5,0
- [C++/WINRT VSIX](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) pour les modèles de projet/WinRT c++

## <a name="create-a-simple-windows-runtime-component-using-cwinrt"></a>Créer un composant Windows Runtime simple à l’aide de C#/WinRT

Commencez par créer un nouveau projet dans Visual Studio 2019. Sélectionnez le modèle de projet **bibliothèque de classes (.net Core)** et nommez-le **AuthoringDemo**. Vous devrez apporter les modifications et les ajouts suivants au projet :

1. Installez la dernière version du [package NuGet C#/WinRT](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT/).

    a. Dans Explorateur de solutions, cliquez avec le bouton droit sur le nœud du projet et sélectionnez **gérer les packages NuGet**.

    b. Recherchez le package NuGet **Microsoft. Windows. CsWinRT** et installez la version la plus récente. Cette procédure pas à pas utilise C#/WinRT version 1.1.1.

2. Mettez à jour le `TargetFramework` dans le fichier **AuthoringDemo. csproj** et ajoutez les éléments suivants au `PropertyGroup` :

    ```xml
    <PropertyGroup>
        <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
        <Platforms>x64</Platforms>
        <AssemblyVersion>1.0.0.0</AssemblyVersion>
    </PropertyGroup>
    ```

    Pour l' `TargetFramework` élément, vous pouvez utiliser l’un des [monikers du Framework cible](/windows/apps/desktop/modernize/desktop-to-uwp-enhance#net-5-use-the-target-framework-moniker-option)suivants.
    - **.net 5.0-Windows 10.0.17763.0**
    - **.net 5.0-Windows 10.0.18362.0**
    - **.net 5.0-Windows 10.0.19041.0**

    Vous devez également spécifier un `AssemblyVersion` pour votre composant Windows Runtime.

3. Ajoutez un nouvel `PropertyGroup` élément qui définit plusieurs propriétés **CsWinRT** .

    ```xml
    <PropertyGroup>   
        <CsWinRTComponent>true</CsWinRTComponent>
        <CsWinRTWindowsMetadata>10.0.19041.0</CsWinRTWindowsMetadata>
        <CsWinRTEnableLogging>true</CsWinRTEnableLogging>
        <GeneratedFilesDir Condition="'$(GeneratedFilesDir)'==''">$([MSBuild]::NormalizeDirectory('$(MSBuildProjectDirectory)', '$(IntermediateOutputPath)', 'Generated Files'))</GeneratedFilesDir>
    </PropertyGroup>
      ```

      Voici quelques détails sur les propriétés de cet exemple. Pour obtenir la liste complète des propriétés de projet CsWinRT, reportez-vous à la [documentation NuGet CsWinRT.](https://github.com/microsoft/CsWinRT/blob/master/nuget/readme.md)

    - La `CsWinRTComponent` propriété spécifie que votre projet est un composant Windows Runtime, afin qu’un fichier WinMD soit généré pour le composant.
    - La `CsWinRTWindowsMetadata` propriété fournit une source pour les métadonnées Windows. Cela est requis depuis la version 1.1.1.
    - La `CsWinRTEnableLogging` propriété génère un fichier **log.txt** avec une sortie détaillée lors de la génération du composant d’exécution.
    - La `GeneratedFilesDir` propriété est requise pour générer le fichier **. winmd** dans le répertoire de sortie de droite. Cela est requis depuis la version 1.1.1.

4. Vous pouvez créer vos classes d’exécution à l’aide des fichiers **de classe de bibliothèque (. cs)** . Cliquez avec le bouton droit sur le fichier **Class1.cs** et renommez-le **example.cs**. Ajoutez le code suivant à ce fichier, qui ajoute une méthode et une propriété publique à la classe d’exécution. N’oubliez pas de marquer toutes les classes que vous souhaitez exposer dans le composant d’exécution **public**.

    ```csharp
    namespace AuthoringDemo
    {
        public sealed class Example
        {
            public int SampleProperty { get; set; }

            public static string SayHello()
            {
                return "Hello from your C# WinRT component";
            }
        }
    }
    ```

5. Vous pouvez maintenant générer le projet pour générer le fichier de métadonnées de votre composant. Cliquez avec le bouton droit sur le projet dans **Explorateur de solutions** , puis cliquez sur **générer**. Le fichier **AuthoringDemo. winmd** généré se trouve dans le dans le **Explorateur de solutions** sous le dossier **fichiers générés** , ainsi que dans le dossier de sortie de la génération.

## <a name="generate-a-nuget-package-for-the-component"></a>Générer un package NuGet pour le composant

Pour distribuer le composant d’exécution en tant que package NuGet, vous devez apporter les modifications suivantes au projet **AuthoringDemo** . Si vous choisissez de ne pas générer un package NuGet pour votre composant, les applications natives peuvent également utiliser le composant à l’aide d’une référence directe au fichier **. winmd** généré, comme indiqué dans la section suivante.

1. Ajoutez un fichier de cibles afin que les applications natives puissent faire référence au package NuGet généré et utiliser votre composant. Dans le **Explorateur de solutions**, cliquez avec le bouton droit sur le projet **AuthoringDemo** et sélectionnez **Ajouter-> nouvel élément**. Recherchez le modèle de **fichier XML** , puis nommez le fichier **AuthoringDemo. targets**.

    > [!NOTE]
    > Le fichier de cibles **doit** être nommé à l’aide de votre nom de composant, avec le format *YourComponentName. targets*.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
        <Import Project="$(MSBuildThisDirectory)AuthoringDemo.CsWinRT.targets" />
    </Project> 
    ```

   Le fichier **AuthoringDemo. CsWinRT. targets** importé sera ajouté au package NuGet, ce qui configure le package avec les assemblys d’hébergement/WinRT C# pour permettre la consommation à partir d’applications natives.  

2. Ajoutez les éléments suivants au fichier projet **AuthoringDemo. csproj** .

    ```xml
    <PropertyGroup>
        <GeneratePackageOnBuild>true</GeneratePackageOnBuild>
    </PropertyGroup>

    <ItemGroup>
        <Content Include="AuthoringDemo.targets" PackagePath="build;buildTransitive"/>
    </ItemGroup>
    ```

    Ces propriétés génèrent un package NuGet pour votre composant et incluent les cibles CsWinRT dans le package en vue d’une utilisation à partir d’applications natives.

3. Générez à nouveau le projet **AuthoringDemo** . Vous devez maintenant voir dans la sortie de génération que le package NuGet « AuthoringDemo. 1.0.0. nupkg » a été créé avec succès.

## <a name="consume-the-component-in-cwinrt"></a>Utiliser le composant en C++/WinRT

Les composants de Windows Runtime créés en C#/WinRT peuvent être consommés à partir d’applications natives avec quelques modifications. Les étapes suivantes montrent comment appeler le composant créé ci-dessus dans une application console native.

1. Ajoutez un nouveau projet d' **application console C++/WinRT** à votre solution. Notez que ce projet peut également faire partie d’une autre solution si vous le souhaitez.

    a. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de votre solution, puis cliquez sur **Ajouter**  ->  **un nouveau projet**.

    b. Dans la **boîte de dialogue Ajouter un nouveau projet**, recherchez le modèle de projet **application console C++/WinRT** . Sélectionnez le modèle, puis cliquez sur **suivant**.

    c. Nommez le nouveau projet **CppConsoleApp** , puis cliquez sur **créer**.

2. Ajoutez une référence au composant AuthoringDemo. Vous pouvez ajouter une référence de package au package NuGet généré à partir de la section précédente, ou une référence directe à **AuthoringDemo. winmd**.

    - **Option 1 (référence du package)**: cliquez avec le bouton droit sur le projet **CppConsoleApp** , puis sélectionnez **gérer les packages NuGet**. Vous devrez peut-être configurer les sources de votre package pour ajouter une référence au package NuGet AuthoringDemo. Pour ce faire, cliquez sur l’icône Paramètres dans le gestionnaire de package NuGet et ajoutez une source de package au chemin d’accès approprié.

        ![Paramètres NuGet](images/nuget-sources-settings.png)

        Après avoir configuré vos sources de package, recherchez le package **AuthoringDemo** , puis cliquez sur **installer**.

        ![Installer le package NuGet](images/install-authoring-nuget.png)

    - **Option 2 (référence directe)**: cliquez avec le bouton droit sur le projet **CppConsoleApp** , puis cliquez sur **référence de >**. Sélectionnez l’onglet **Parcourir** , puis recherchez et sélectionnez le fichier **AuthoringDemo. winmd** à partir de la sortie de génération du projet **AuthoringDemo** .

3. Pour faciliter l’hébergement du composant, vous devrez ajouter un runtimeconfig.jssur le fichier et un fichier manifeste. Pour plus d’informations sur l’hébergement de composants managés, reportez-vous à [ces documents d’hébergement](https://github.com/microsoft/CsWinRT/blob/master/docs/hosting.md).

    a. Pour ajouter le runtimeconfig.jssur le fichier, cliquez avec le bouton droit sur le projet et choisissez **Ajouter-> nouvel élément**. Recherchez le modèle de **fichier texte** et nommez-le **WinRT.Host.runtimeconfig.js**. Collez le contenu suivant :

    ```json
    {
        "runtimeOptions": {
            "tfm": "net5.0",
            "rollForward": "LatestMinor",
            "framework": {
                "name": "Microsoft.NETCore.App",
                "version": "5.0.0"
            }
        }
    }
    ```

    Remarque pour l' `tfm` entrée, une installation autonome de .net 5 peut être référencée à l’aide de la variable d’environnement DOTNET_ROOT.

    b. Pour ajouter le fichier manifeste, cliquez à nouveau avec le bouton droit sur le projet et choisissez **Ajouter-> nouvel élément**. Recherchez le modèle de **fichier texte** et nommez-le **CppConsoleApp.exe. manifest**. Collez le contenu suivant :

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <assembly manifestVersion="1.0" xmlns="urn:schemas-microsoft-com:asm.v1">
        <assemblyIdentity version="1.0.0.0" name="CppConsoleApp"/>
        <file name="WinRT.Host.dll">
            <activatableClass
                name="AuthoringDemo.Example"
                threadingModel="both"
                xmlns="urn:schemas-microsoft-com:winrt.v1" />
        </file>
    </assembly>
    ```

    Le fichier manifeste est requis pour les applications non empaquetées. Dans ce fichier, spécifiez vos classes d’exécution à l’aide des entrées d’inscriptions de classe activables comme indiqué ci-dessus.

4. Modifiez le fichier projet natif pour inclure les fichiers runtimeconfig et manifest dans le déploiement du projet. Cliquez avec le bouton droit sur le projet, puis cliquez sur **décharger le projet**. Une fois le projet déchargé, cliquez de nouveau avec le bouton droit sur le projet et sélectionnez **modifier le fichier projet**. Recherchez les entrées pour **WinRT.Host.runtimeconfig.jssur** et **CppConsoleApp.exe. manifest**, puis ajoutez la `DeploymentContent` propriété comme indiqué ci-dessous.

    ```xml
    <ItemGroup>
        <None Include="WinRT.Host.runtimeconfig.json">
            <DeploymentContent>true</DeploymentContent>
        </None>

        <Manifest Include="CppConsoleApp.exe.manifest">
            <DeploymentContent>true</DeploymentContent>
        </Manifest>
    </ItemGroup> 
    ```

5. Ouvrez **pch. h** sous les fichiers d’en-tête du projet, puis ajoutez la ligne de code suivante pour inclure votre composant.

    ```cpp
    #include <winrt/AuthoringDemo.h>
    ```

6. Ouvrez **main. cpp** sous les fichiers sources du projet, puis remplacez-le par le contenu suivant.

    ```cpp
    #include "pch.h"
    #include "iostream"

    using namespace winrt;
    using namespace Windows::Foundation;

    int main()
    {
        init_apartment();

        AuthoringDemo::Example ex;
        ex.SampleProperty(42);
        std::wcout << ex.SampleProperty() << std::endl;
        std::wcout << ex.SayHello().c_str() << std::endl;
    }
    ```

7. Générez et exécutez le projet **CppConsoleApp** . Vous devez maintenant voir la sortie ci-dessous.

    ![Sortie de la console C++/WinRT](images/consume-component-output.png)

## <a name="related-topics"></a>Rubriques connexes

- [Création de composants](https://github.com/microsoft/CsWinRT/blob/master/docs/authoring.md)
- [Hébergement de composants managés](https://github.com/microsoft/CsWinRT/blob/master/docs/hosting.md)
