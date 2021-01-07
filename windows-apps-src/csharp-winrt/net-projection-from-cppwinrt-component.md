---
description: Cette procédure pas à pas montre comment utiliser C#/WinRT pour générer une projection .NET 5 pour un composant C++/WinRT.
title: Procédure pas à pas pour générer une projection .NET 5 à partir d’un composant C++/WinRT et distribuer le NuGet
ms.date: 11/12/2020
ms.topic: article
keywords: Windows 10, c#, WinRT, cswinrt, projection
ms.localizationpriority: medium
ms.openlocfilehash: 57bc5c49d47dacee910cd3d80964f797633ef587
ms.sourcegitcommit: 6da85cc75c02a5a7417966abddc8824ac87fb619
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/07/2021
ms.locfileid: "97964732"
---
# <a name="walkthrough-generate-a-net-5-projection-from-a-cwinrt-component-and-distribute-the-nuget"></a>Procédure pas à pas : Générer une projection .NET 5 à partir d’un composant C++/WinRT et distribuer le NuGet

Cette procédure pas à pas montre comment utiliser [C#/WinRT](index.md) pour générer une projection .net 5 pour un composant C++/WinRT, créer le package NuGet associé et référencer le package NuGet à partir d’une application console C# .net 5.

Vous pouvez télécharger l’exemple complet pour cette procédure pas à pas à partir de GitHub [ici](https://github.com/microsoft/CsWinRT/tree/master/src/Samples/Net5ProjectionSample).

## <a name="prerequisites"></a>Prérequis

Cette procédure pas à pas et l’exemple correspondant requièrent les outils et composants suivants :

- [Visual Studio 16,8](https://visualstudio.microsoft.com/downloads/) (ou version ultérieure) avec la charge de travail de développement plateforme Windows universelle installée. Dans **Détails de l’installation**  >  **plateforme Windows universelle développement**, activez l’option **outils de plateforme Windows universelle C++ (v14x)** .
- [.Net 5,0 SDK](https://dotnet.microsoft.com/download/dotnet/5.0).
- [Extension VSIX c++/WinRT](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) pour les modèles de projet/WinRT c++.

## <a name="create-a-simple-cwinrt-runtime-component"></a>Créer un composant d’exécution C++/WinRT simple

Pour suivre cette procédure pas à pas, vous devez d’abord disposer d’un composant C++/WinRT pour lequel créer une projection .NET 5. Cette procédure pas à pas utilise le projet **SimpleMathComponent** dans l’exemple associé à partir de GitHub [ici](https://github.com/microsoft/CsWinRT/tree/master/src/Samples/Net5ProjectionSample/SimpleMathComponent). Il s’agit d’un projet **Windows Runtime Component (C++/WinRT)** qui a été créé à l’aide de l' [extension/WinRT VSIX c++](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package). Une fois que vous avez copié le projet sur votre ordinateur de développement, ouvrez la solution dans Visual Studio 2019 preview.

Le code de ce projet fournit les fonctionnalités pour les opérations mathématiques de base affichées dans le fichier d’en-tête ci-dessous. 

```cpp
// SimpleMath.h
...
namespace winrt::SimpleMathComponent::implementation
{
    struct SimpleMath: SimpleMathT<SimpleMath>
    {
        SimpleMath() = default;
        double add(double firstNumber, double secondNumber);
        double subtract(double firstNumber, double secondNumber);
        double multiply(double firstNumber, double secondNumber);
        double divide(double firstNumber, double secondNumber);
    };
}
```

Pour obtenir des instructions plus détaillées sur la création d’un composant C++/WinRT et la génération d’un fichier. winmd, consultez [Windows Runtime Components with c++/WinRT](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md).

> [!NOTE]
> Si vous implémentez [IInspectable :: getruntimeclassname,](/windows/win32/api/inspectable/nf-inspectable-iinspectable-getruntimeclassname) dans votre composant, celui-ci **doit** retourner un nom de classe WinRT valide. Comme C#/WinRT utilise la chaîne de nom de classe pour l’interopérabilité, un nom de classe d’exécution incorrect lève une **exception InvalidCastException**.

## <a name="add-a-projection-project-to-the-component-solution"></a>Ajouter un projet de projection à la solution de composant

Si vous avez cloné l’exemple à partir de référentiel, commencez par supprimer le projet **SimpleMathProjection** pour suivre la procédure pas à pas.

1. Ajoutez un nouveau projet de **bibliothèque de classes (.net Core)** à votre solution.

    1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de votre solution, puis cliquez sur **Ajouter**  ->  **un nouveau projet**.
    2. Dans la **boîte de dialogue Ajouter un nouveau projet**, recherchez le modèle de projet **bibliothèque de classes (.net Core)** . Sélectionnez le modèle, puis cliquez sur **suivant**.
    3. Nommez le nouveau projet **SimpleMathProjection** , puis cliquez sur **créer**.

2. Supprimez le fichier **Class1.cs** vide du projet.

3. Installez le [package NuGet C#/WinRT](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT).

    1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur votre projet **SimpleMathProjection** , puis sélectionnez **gérer les packages NuGet**. 
    2. Recherchez le package NuGet **Microsoft. Windows. CsWinRT** et installez la version la plus récente.

4. Ajoutez une référence de projet au projet **SimpleMathComponent** . Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le nœud **dépendances** sous le projet **SimpleMathProjection** , sélectionnez **Ajouter une référence de projet**, puis sélectionnez le projet **SimpleMathComponent** .

Une fois ces étapes effectuées, votre **Explorateur de solutions** devrait ressembler à ceci.

![Explorateur de solutions présentant les dépendances du projet de projection](images/projection-dependencies.png)

## <a name="edit-the-project-file-to-execute-cwinrt"></a>Modifier le fichier projet pour exécuter C#/WinRT

Avant de pouvoir appeler **cswinrt.exe** et générer l’assembly de projection, vous devez modifier le fichier projet pour le projet de projection.

1. Dans **Explorateur de solutions**, double-cliquez sur le nœud **SimpleMathProjection** pour ouvrir le fichier projet dans l’éditeur.

2. Mettez à jour l' `TargetFramework` élément pour référencer le SDK Windows. Cela ajoute les dépendances d’assembly nécessaires à la prise en charge de l’interopérabilité et de la projection. Notre exemple cible la version la plus récente de Windows 10 lors de cette procédure pas à pas, **net 5.0-Windows 10.0.19041.0** (également appelé Kit de développement logiciel (SDK) version 2004).

    ```xml
    <PropertyGroup>
      <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
      <Platforms>x64</Platforms>
    </PropertyGroup>
    ```

3. Ajoutez un nouvel `PropertyGroup` élément qui définit plusieurs propriétés **cswinrt** .

    ```xml
    <PropertyGroup>
      <CsWinRTIncludes>SimpleMathComponent</CsWinRTIncludes>
      <CsWinRTGeneratedFilesDir>$(OutDir)</CsWinRTGeneratedFilesDir>
    </PropertyGroup>
    ```

    Voici quelques détails sur les paramètres de cet exemple :

    - La `CsWinRTIncludes` propriété spécifie les espaces de noms à projeter.
    - La `CsWinRTGeneratedFilesDir` propriété définit le répertoire de sortie dans lequel les fichiers de la projection sont générés, que nous avons défini dans la section suivante sur la génération à partir de la source.

4. La dernière version de C#/WinRT, telle que celle de cette procédure pas à pas, peut nécessiter la spécification de métadonnées Windows. Il peut être fourni avec les deux éléments suivants :

    - Une référence de package NuGet, par exemple [Microsoft. Windows. Sdk. Contracts]( https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts/):

      ```xml
      <ItemGroup>
        <PackageReference Include="Microsoft.Windows.SDK.Contracts" Version="10.0.19041.1" />
      </ItemGroup>
      ```

    - Une autre option consiste à ajouter la `CsWinRTWindowsMetadata` propriété suivante au à `PropertyGroup` partir de l’étape 3 :

      ```xml
      <CsWinRTWindowsMetadata>10.0.19041.0</CsWinRTWindowsMetadata>
      ```

5. Enregistrez et fermez le fichier **SimpleMathProjection. csproj** .

## <a name="build-projects-out-of-source"></a>Générer des projets à partir de la source

Dans l' [exemple associé](https://github.com/microsoft/CsWinRT/tree/master/src/Samples/Net5ProjectionSample), la build est configurée avec le fichier **Directory. Build. props** . Les fichiers générés de la génération des projets **SimpleMathComponent** et **SimpleMathProjection** s’affichent dans le dossier *_build* au niveau de la solution. Pour configurer vos projets pour qu’ils soient générés hors de la source, copiez le fichier **Directory. Build. props** ci-dessous dans le répertoire contenant votre fichier solution.

```xml
<Project>
  <PropertyGroup>
    <BuildOutDir>$([MSBuild]::NormalizeDirectory('$(SolutionDir)_build', '$(Platform)', '$(Configuration)'))</BuildOutDir>
    <OutDir>$([MSBuild]::NormalizeDirectory('$(BuildOutDir)', '$(MSBuildProjectName)', 'bin'))</OutDir>
    <IntDir>$([MSBuild]::NormalizeDirectory('$(BuildOutDir)', '$(MSBuildProjectName)', 'obj'))</IntDir>
  </PropertyGroup>
</Project>
```

Même si cette étape n’est pas requise pour générer une projection, elle offre une simplicité en générant des fichiers de build à partir des deux projets dans le même répertoire et en facilitant le nettoyage de la génération. Notez que si vous ne générez pas de source, **SimpleMathComponent. winmd** et l’assembly d’interopérabilité **SimpleMathComponent.dll** seront générés dans des répertoires différents dans leurs dossiers de projet respectifs. Ces fichiers sont référencés dans **SimpleMathProjection. NuSpec** ci-dessous, de sorte que les chemins d’accès doivent être modifiés en conséquence.

## <a name="create-a-nuget-package-from-the-projection"></a>Créer un package NuGet à partir de la projection

Pour distribuer et utiliser l’assembly d’interopérabilité, vous pouvez créer automatiquement un package NuGet lors de la génération de la solution en ajoutant des propriétés de projet supplémentaires. Pour les cibles .NET 5,0, le package doit inclure l’assembly d’interopérabilité, l’assembly d’implémentation et une dépendance sur le package NuGet C#/WinRT pour l’assembly de Runtime C#/WinRT requis, **WinRT.Runtime.dll**.

1. Ajoutez un fichier de spécifications NuGet (. NuSpec) au projet **SimpleMathProjection** .

    1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le nœud **SimpleMathProjection** , choisissez **Ajouter**  ->  **un nouveau dossier**, puis nommez le dossier **NuGet**. 
    2. Cliquez avec le bouton droit sur le dossier **NuGet** , choisissez **Ajouter**  ->  **un nouvel élément**, choisissez le fichier XML, puis nommez-le **SimpleMathProjection. NuSpec**. 

2. Ajoutez le code suivant à **SimpleMathProjection. csproj** pour générer automatiquement le package. Ces propriétés spécifient le `NuspecFile` et le répertoire pour générer le package NuGet.

    ```xml
    <PropertyGroup>
      <GeneratedNugetDir>.\nuget\</GeneratedNugetDir>
      <NuspecFile>$(GeneratedNugetDir)SimpleMathProjection.nuspec</NuspecFile>
      <OutputPath>$(GeneratedNugetDir)</OutputPath>
      <GeneratePackageOnBuild>true</GeneratePackageOnBuild>
    </PropertyGroup>

3. Open the **SimpleMathProjection.nuspec** file to edit the package creation properties. Below is an example NuGet spec for distributing the interop assembly from the C++/WinRT component. Note that for .NET 5.0 targets, under the `dependencies` node there is a dependency on CsWinRT, and under the `files` node **SimpleMathProjection.dll** is specified instead of **SimpleMathComponent.winmd** for the target `lib\net5.0\SimpleMathProjection.dll`. This behavior is new in .NET 5.0 and enabled by C#/WinRT. The implementation assembly, **SimpleMathComponent.dll**, must also be deployed for .NET 5.0 targets. 

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <package xmlns="http://schemas.microsoft.com/packaging/2012/06/nuspec.xsd">
      <metadata>
        <id>SimpleMathComponent</id>
        <version>0.1.0-prerelease</version>
        <authors>Contoso Math Inc.</authors>
        <description>A simple component with basic math operations</description>
        <dependencies>
          <group targetFramework=".NETCoreApp3.0" />
          <group targetFramework="UAP10.0" />
          <group targetFramework=".NETFramework4.6" />
          <group targetFramework="net5.0">
            <dependency id="Microsoft.Windows.CsWinRT" version="1.0.1" exclude="Build,Analyzers" />
          </group>
        </dependencies>
      </metadata>
      <files>
        <!--Support netcore3, uap, net46+, net5, c++ -->
        <file src="..\..\_build\x64\Debug\SimpleMathComponent\bin\SimpleMathComponent\SimpleMathComponent.winmd" target="lib\netcoreapp3.0\SimpleMathComponent.winmd" />
        <file src="..\..\_build\x64\Debug\SimpleMathComponent\bin\SimpleMathComponent\SimpleMathComponent.winmd" target="lib\uap10.0\SimpleMathComponent.winmd" />
        <file src="..\..\_build\x64\Debug\SimpleMathComponent\bin\SimpleMathComponent\SimpleMathComponent.winmd" target="lib\net46\SimpleMathComponent.winmd" />
        <file src="..\..\_build\x64\Debug\SimpleMathProjection\bin\SimpleMathProjection.dll" target="lib\net5.0\SimpleMathProjection.dll" />
        <file src="..\..\_build\x64\Debug\SimpleMathComponent\bin\SimpleMathComponent\SimpleMathComponent.dll" target="runtimes\win10-x64\native\SimpleMathComponent.dll" />
      </files>
    </package>
    ```

## <a name="build-the-solution-to-generate-the-projection-and-nuget-package"></a>Générer la solution pour générer la projection et le package NuGet

À ce stade, vous pouvez maintenant générer la solution : cliquez avec le bouton droit sur le nœud de votre solution et sélectionnez **générer la solution**. Cela génère d’abord le projet de composant, puis le projet de projection. Les fichiers et l’assembly Interop **. cs** sont générés dans le répertoire de sortie, en plus des fichiers de métadonnées du projet de composant. Vous serez également en mesure de voir le package NuGet généré **simplemathcomponent 0.1.0-prerelease. nupkg** dans le dossier **NuGet** .

![Explorateur de solutions présentant la génération de projection](images/projection-generated-files.png)

## <a name="reference-the-nuget-package-in-a-c-net-50-console-application"></a>Référencer le package NuGet dans une application console C# .NET 5,0

Pour utiliser le **SimpleMathComponent** projeté, vous pouvez simplement ajouter une référence au package NuGet nouvellement créé dans votre application. Les étapes suivantes montrent comment effectuer cette opération en créant une application console simple dans une solution distincte.

1. Créez une solution avec un projet **application console (.net Core)** .

    1. Dans Visual Studio, sélectionnez **Fichier** -> **Nouveau** -> **Projet**.
    2. Dans la **boîte de dialogue Ajouter un nouveau projet**, recherchez le modèle de projet **application console (.net Core)** . Sélectionnez le modèle, puis cliquez sur **suivant**.
    3. Nommez le nouveau projet **SampleConsoleApp** , puis cliquez sur **créer**. La création de ce projet dans une nouvelle solution vous permet de restaurer le package NuGet **SimpleMathComponent** séparément.

2. Dans **Explorateur de solutions**, double-cliquez sur le nœud **SampleConsoleApp** pour ouvrir le fichier projet **SampleConsoleApp. csproj** et mettez à jour le moniker du Framework cible et la configuration de la plateforme, comme indiqué dans l’exemple suivant.

    ```xml
    <PropertyGroup>
      <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
      <Platforms>x64</Platforms>
    </PropertyGroup>
    ```

3. Ajoutez le package NuGet **SimpleMathComponent** au projet **SampleConsoleApp** . Vous aurez également besoin du package NuGet [Microsoft. VCRTForwarders. 140](https://www.nuget.org/packages/Microsoft.VCRTForwarders.140/) , qui est requis dans les applications qui ne sont pas empaquetées dans un package MSIX. Pour restaurer le NuGet **SimpleMathComponent** lors de la génération du projet, vous pouvez utiliser la `RestoreSources` propriété avec le chemin d’accès au dossier **NuGet** dans votre solution de composant.

    ```xml
    <PropertyGroup>
      <RestoreSources>
        https://api.nuget.org/v3/index.json;
        ../../CppWinRTProjectionSample/SimpleMathProjection/nuget
      </RestoreSources>
    </PropertyGroup>

    <ItemGroup>
      <PackageReference Include="Microsoft.VCRTForwarders.140" Version="1.0.6" />
      <PackageReference Include="SimpleMathComponent" Version="0.1.0-prerelease" />
    </ItemGroup>
    ```

    Notez que pour cette procédure pas à pas, le chemin d’accès de restauration NuGet pour le **SimpleMathComponent** suppose que les deux fichiers de solution se trouvent dans le même répertoire. Vous pouvez également [Ajouter un flux de package NuGet local](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-visual-studio#package-sources) à votre solution.

4. Modifiez le fichier **Program.cs** pour utiliser la fonctionnalité fournie par **SimpleMathComponent**.

    ```csharp
    static void Main(string[] args)
    {
        var x = new SimpleMathComponent.SimpleMath();
        Console.WriteLine("Adding 5.5 + 6.5 ...");
        Console.WriteLine(x.add(5.5, 6.5).ToString());
    }
    ```

5. Générez et exécutez l’application console. Vous devez voir la sortie ci-dessous.

    ![Sortie de la console NET5](images/console-output.png)

## <a name="resources"></a>Ressources

- [Exemple de code complet pour cette procédure pas à pas](https://github.com/microsoft/CsWinRT/tree/master/src/Samples/Net5ProjectionSample)
