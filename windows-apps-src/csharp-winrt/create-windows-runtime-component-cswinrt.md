---
description: Créer un composant Windows Runtime avec C#/WinRT et le consommer à partir d’une application native
title: Créer un composant C#/WinRT et le consommer à partir de C++/WinRT
ms.date: 01/28/2021
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9f5157f97163a72ccce1ce9fc3f560fb4e16b1df
ms.sourcegitcommit: 61a874d00991f7ca06466a99a557ef0777bd0f7c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/09/2021
ms.locfileid: "99989649"
---
# <a name="walkthrough-create-a-cwinrt-component-and-consume-it-from-cwinrt"></a>Procédure pas à pas : créer un composant/WinRT C# et le consommer à partir de C++/WinRT

> [!NOTE]
> La prise en charge de la création de/WinRT C# décrite dans cet article est actuellement en version préliminaire à partir de C#/WinRT version 1.1.2-version 1.1.2.210208.6. À partir de cette version, il est destiné à être utilisé uniquement pour les commentaires et l’évaluation.

C#/WinRT permet aux développeurs .NET 5 de créer leurs propres composants Windows Runtime en C# à l’aide d’un projet de bibliothèque de classes. Les composants créés peuvent être utilisés dans les applications de bureau natives en tant que référence de package ou en tant que référence de projet avec quelques modifications.

Cette procédure pas à pas montre comment créer un composant Windows Runtime simple à l’aide de C#/WinRT, distribuer le composant en tant que package NuGet et utiliser le composant à partir d’une application console C++/WinRT. Vous trouverez un exemple de code pour cette procédure pas à pas sur GitHub [ici](https://github.com/microsoft/CsWinRT/tree/master/src/Samples/AuthoringDemo).

Lors de la création de votre composant d’exécution, suivez les instructions et les restrictions de type décrites dans [cet article.](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md) En interne, les types de Windows Runtime dans votre composant peuvent utiliser toutes les fonctionnalités .NET autorisées dans une application UWP. Pour plus d’informations, consultez [.net pour les applications UWP](/dotnet/api/index?view=dotnet-uwp-10.0&preserve-view=true). En externe, les membres de votre type peuvent exposer uniquement les types de Windows Runtime pour leurs paramètres et valeurs de retour.

> [!NOTE]
> Certains types [de Windows Runtime mappés aux types .net](../winrt-components/net-framework-mappings-of-windows-runtime-types.md#uwp-types-that-map-to-net-types-with-a-different-name-andor-namespace). Ces types .NET peuvent être utilisés dans l’interface publique de votre composant Windows Runtime et s’affichent pour les utilisateurs du composant en tant que types de Windows Runtime correspondants.

## <a name="prerequisites"></a>Prérequis

Cette procédure pas à pas nécessite les outils et composants suivants :

- Visual Studio 2019
- SDK .NET 5,0
- [C++/WINRT VSIX](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) pour les modèles de projet/WinRT c++

## <a name="create-a-simple-windows-runtime-component-using-cwinrt"></a>Créer un composant Windows Runtime simple à l’aide de C#/WinRT

Commencez par créer un nouveau projet dans Visual Studio 2019. Sélectionnez le modèle de projet **bibliothèque de classes (.net Core)** et nommez-le **AuthoringDemo**. Vous devrez apporter les modifications et les ajouts suivants au projet :

1. Mettez à jour le `TargetFramework` dans le fichier **AuthoringDemo. csproj** et ajoutez les éléments suivants au `PropertyGroup` :

    ```xml
    <PropertyGroup>
        <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
        <Platforms>x64</Platforms>
    </PropertyGroup>
    ```

    Pour l' `TargetFramework` élément, vous pouvez utiliser l’un des [monikers du Framework cible](/windows/apps/desktop/modernize/desktop-to-uwp-enhance#net-5-use-the-target-framework-moniker-option)suivants.
    - **.net 5.0-Windows 10.0.17763.0**
    - **.net 5.0-Windows 10.0.18362.0**
    - **.net 5.0-Windows 10.0.19041.0**

2. Installez la dernière version du [package NuGet C#/WinRT](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT/1.1.2-prerelease.210208.6).

    a. Dans Explorateur de solutions, cliquez avec le bouton droit sur le nœud du projet et sélectionnez **gérer les packages NuGet**.

    b. Recherchez le package NuGet **Microsoft. Windows. CsWinRT** et installez la version la plus récente. Cette procédure pas à pas utilise C#/WinRT version 1.1.2-version 1.1.2.210208.6.

3. Ajoutez un nouvel `PropertyGroup` élément qui définit plusieurs propriétés C#/WinRT.

    ```xml
    <PropertyGroup>   
        <CsWinRTComponent>true</CsWinRTComponent>
        <CsWinRTWindowsMetadata>10.0.19041.0</CsWinRTWindowsMetadata>
    </PropertyGroup>
      ```

      Voici quelques détails sur les propriétés de cet exemple. Pour obtenir la liste complète des propriétés de projet C#/WinRT, reportez-vous à la [documentation c#/WinRT NuGet.](https://github.com/microsoft/CsWinRT/blob/master/nuget/readme.md)

    - La `CsWinRTComponent` propriété spécifie que votre projet est un composant Windows Runtime, afin qu’un fichier WinMD soit généré pour le composant.
    - La `CsWinRTWindowsMetadata` propriété fournit une source pour les métadonnées Windows. Cela est requis depuis la version 1.1.1.

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

Ensuite, générez un package NuGet pour le composant. Lorsque vous générez le package, C#/WinRT configure le composant et les assemblys d’hébergement dans le package en vue d’une utilisation à partir d’applications natives.

Il existe plusieurs façons de générer le package NuGet :

* Si vous souhaitez générer un package NuGet chaque fois que vous générez le projet, ajoutez la propriété suivante au fichier projet **AuthoringDemo** , puis régénérez le projet.

    ```xml
    <PropertyGroup>
        <GeneratePackageOnBuild>true</GeneratePackageOnBuild>
    </PropertyGroup>
    ```

* Vous pouvez également générer un package NuGet en cliquant avec le bouton droit sur le projet **AuthoringDemo** dans **Explorateur de solutions** et en sélectionnant **Pack**.

Lorsque vous générez le package, la fenêtre de **génération** doit indiquer que le package NuGet `AuthoringDemo.1.0.0.nupkg` a été créé avec succès.

## <a name="consume-the-component-in-cwinrt"></a>Utiliser le composant en C++/WinRT

Les composants de Windows Runtime créés en C#/WinRT peuvent être consommés à partir d’applications natives avec quelques modifications. Les étapes suivantes montrent comment appeler le composant créé ci-dessus dans une application console native. 

1. Ajoutez un nouveau projet d' **application console C++/WinRT** à votre solution. Notez que ce projet peut également faire partie d’une autre solution si vous le souhaitez.

    a. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de votre solution, puis cliquez sur **Ajouter**  ->  **un nouveau projet**.

    b. Dans la **boîte de dialogue Ajouter un nouveau projet**, recherchez le modèle de projet **application console C++/WinRT** . Sélectionnez le modèle, puis cliquez sur **suivant**.

    c. Nommez le nouveau projet **CppConsoleApp** , puis cliquez sur **créer**.

2. Ajoutez une référence au composant AuthoringDemo, sous la forme d’un package NuGet ou d’une référence de projet.

    - **Option 1 (référence du package)**:  

        a. Cliquez avec le bouton droit sur le projet **CppConsoleApp** , puis sélectionnez **gérer les packages NuGet**. Vous devrez peut-être configurer les sources de votre package pour ajouter une référence au package NuGet AuthoringDemo. Pour ce faire, cliquez sur l’icône **paramètres** dans le gestionnaire de package NuGet et ajoutez une source de package au chemin d’accès approprié.

        ![Paramètres NuGet](images/nuget-sources-settings.png)

        b. Après avoir configuré vos sources de package, recherchez le package **AuthoringDemo** , puis cliquez sur **installer**.

        ![Installer le package NuGet](images/install-authoring-nuget.png)

    - **Option 2 (référence du projet)**:
        
        a. Cliquez avec le bouton droit sur le projet **CppConsoleApp** et sélectionnez **Ajouter** une  ->  **référence**. Sous le nœud **projets** , ajoutez une référence au projet **AuthoringDemo** . À partir de cette version préliminaire, vous devrez également ajouter une référence de fichier à **AuthoringDemo. winmd** à partir du nœud **Parcourir** . Le fichier winmd généré se trouve dans le répertoire de sortie du projet **AuthoringDemo** .

        b. Pour cette version préliminaire, vous devrez également ajouter le groupe de propriétés suivant à **CppConsoleApp. vcxproj**. Pour modifier le fichier projet d’application native, commencez par cliquer avec le bouton droit sur le nœud de projet **CppConsoleApp** et sélectionnez **décharger le projet**.

        ```xml
        <PropertyGroup>
            <TargetFrameworkVersion>net5.0</TargetFrameworkVersion>
            <TargetFramework>native</TargetFramework>
            <TargetRuntime>Native</TargetRuntime>
        </PropertyGroup>
        ```

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

4. Modifiez le projet de façon à inclure les runtimeconfig.jssur et les fichiers manifeste dans la sortie lors du déploiement du projet. Pour les fichiers **WinRT.Host.runtimeconfig.js** et **CppConsoleApp.exe. manifest** , cliquez sur le fichier dans **Explorateur de solutions** et affectez à la propriété **content** la **valeur true**. Voici un exemple de ce à quoi cela ressemble.

    ![Déployer du contenu](images/deploy-content.png)

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

- [Exemple de code](https://github.com/microsoft/CsWinRT/tree/master/src/Samples/AuthoringDemo)
- [Création de composants](https://github.com/microsoft/CsWinRT/blob/master/docs/authoring.md)
- [Hébergement de composants managés](https://github.com/microsoft/CsWinRT/blob/master/docs/hosting.md)
