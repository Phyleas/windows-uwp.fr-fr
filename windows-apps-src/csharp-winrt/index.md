---
description: C#/WinRT est un ensemble d’outils qui fournit une prise en charge de la projection WinRT pour le code C#.
title: C#/WinRT
ms.date: 05/19/2020
ms.topic: article
keywords: windows 10, uwp, standard, c#, winrt, cswinrt, projection
ms.localizationpriority: medium
ms.openlocfilehash: c3cac3049dbd5d22c23716a2da38a41fb6000a71
ms.sourcegitcommit: 140bbbab0f863a7a1febee85f736b0412bff1ae7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91984495"
---
# <a name="cwinrt"></a>C#/WinRT

> [!IMPORTANT]
> C#/WinRT est en préversion publique et peut être largement modifié avant la version finale. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.

C#/WinRT est un kit de ressources empaqueté dans NuGet qui fournit une prise en charge de la projection Windows Runtime (WinRT) pour le langage C#. Une *projection* est une couche de traduction, comme un assembly d’interopérabilité, qui permet de programmer des API WinRT de manière naturelle et familière pour le langage cible. Par exemple, la projection C#/WinRT masque les détails de l’interopérabilité entre les interfaces C# et WinRT, et fournit des mappages de nombreux types WinRT aux équivalents .NET appropriés, comme des chaînes, des URI, des types valeur courants et des collections génériques.

C#/WinRT fournit actuellement une prise en charge de la consommation des types WinRT, et la préversion actuelle vous permet de [créer](#create-an-interop-assembly) et de [référencer](#reference-an-interop-assembly) les assemblys d’interopérabilité WinRT. Les futures versions de C#/WinRT ajouteront une prise en charge de la création de types WinRT en C#.

## <a name="motivation-for-cwinrt"></a>Motivation pour C#/WinRT

[.NET Core](/dotnet/core/) est le point principal de la plateforme .NET, et .NET 5 est la prochaine version majeure. Il s’agit d’un runtime open source multiplateforme qui peut être utilisé pour générer des applications d’appareil, cloud et IoT.

Les versions précédentes de .NET Framework et .NET Core avaient une connaissance intégrée de WinRT, qui est une technologie spécifique à Windows. Pour prendre en charge les objectifs de portabilité et d’efficacité de .NET 5, nous avons retiré la prise en charge de la projection WinRT du compilateur et du runtime .NET, et nous l’avons déplacée dans le kit de ressources C#/WinRT. L’objectif de C#/WinRT est de fournir la parité avec la prise en charge WinRT intégrée proposée par les versions antérieures du compilateur C# et du runtime .NET. Pour plus d’informations, consultez [Mappages .NET des types Windows Runtime](../winrt-components/net-framework-mappings-of-windows-runtime-types.md).

C#/WinRT prend également en charge WinUI 3.0. Cette version de WinUI retire du système d’exploitation les contrôles et fonctionnalités de l’interface utilisateur Microsoft native. Cela permet aux développeurs d’applications d’utiliser les derniers contrôles et visuels disponibles sur Windows 10, version 1803 et sur les versions ultérieures.

Enfin, C#/WinRT est un kit de ressources général conçu pour prendre en charge d’autres scénarios où la prise en charge intégrée de WinRT n’est pas disponible dans le compilateur C# ou le runtime .NET. C#/WinRT prend en charge les versions du runtime .NET compatibles avec .NET Standard jusqu’à la version 2.0, comme Mono 5.4.

Pour plus d’informations sur C#/WinRT, consultez le [dépôt GitHub C#/WinRT](https://aka.ms/cswinrt/repo).

## <a name="create-an-interop-assembly"></a>Créer un assembly d’interopérabilité

Les API WinRT sont définies dans les fichiers de métadonnées Windows (*. winmd). Le package NuGet C#/WinRT ([Microsoft.Windows.CsWinRT](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT/)) comprend le compilateur C#/WinRT, **cswinrt**, que vous pouvez utiliser pour traiter les fichiers de métadonnées Windows et générer du code C# .NET 5.0. Vous pouvez compiler ces fichiers sources dans des assemblys d’interopérabilité, de la même façon que [C++/WinRT](../cpp-and-winrt-apis/index.md) génère des en-têtes pour une projection de langage C++. Vous pouvez alors distribuer les assemblys d’interopérabilité C#/WinRT pour les applications à référencer, en même temps que l’assembly de runtime C#/WinRT.

Pour une procédure pas à pas qui montre comment créer un assembly d’interopérabilité, consultez [Procédure pas à pas : Générer une projection .NET 5 à partir d’un composant C++/WinRT et mettre à jour le NuGet](net-projection-from-cppwinrt-component.md).

### <a name="invoke-cswinrtexe"></a>Appeler cswinrt.exe

Pour afficher des options de ligne de commande, exécutez `cswinrt -?`. Pour appeler cswinrt.exe à partir d’un projet, nous vous recommandons d’utiliser un fichier Directory.Build.Targets. Le fragment de projet suivant montre un appel simple de **cswinrt** pour générer des sources de projection pour les types dans l’espace de noms Contoso. Ces sources sont ensuite incluses dans la build du projet.

```xml
  <Target Name="GenerateProjection" BeforeTargets="Build">
    <PropertyGroup>
      <CsWinRTParams>
# This sample demonstrates using a response file for cswinrt execution.
# Run "cswinrt -h" to see all command line options.
-verbose
# Include Windows SDK metadata to satisfy references to 
# Windows types from project-specific metadata.
-in 10.0.18362.0
# Don't project referenced Windows types, as these are 
# provided by the Windows interop assembly.
-exclude Windows 
# Reference project-specific winmd files, defined elsewhere,
# such as from a NuGet package.
-in @(ContosoWinMDs->'"%(FullPath)"', ' ')
# Include project-specific namespaces/types in the projection
-include Contoso 
# Write projection sources to the "Generated Files" folder,
# which should be excluded from checkin (e.g., .gitignored).
-out "$(ProjectDir)Generated Files"
      </CsWinRTParams>
    </PropertyGroup>
    <WriteLinesToFile
        File="$(CsWinRTResponseFile)" Lines="$(CsWinRTParams)"
        Overwrite="true" WriteOnlyWhenDifferent="true" />
    <Message Text="$(CsWinRTCommand)" Importance="$(CsWinRTVerbosity)" />
    <Exec Command="$(CsWinRTCommand)" />
  </Target>

  <Target Name="IncludeProjection" BeforeTargets="CoreCompile" AfterTargets="GenerateProjection">
    <ItemGroup>
      <Compile Include="$(ProjectDir)Generated Files/*.cs" Exclude="@(Compile)" />
    </ItemGroup>
  </Target>
```

### <a name="distribute-the-interop-assembly"></a>Distribuer l’assembly d’interopérabilité

Un assembly d’interopérabilité est généralement distribué sous la forme d’un package NuGet, avec une dépendance sur le package NuGet C#/WinRT pour l’assembly de runtime C#/WinRT nécessaire, **winrt.runtime.dll**. Il existe deux versions de l’assembly de runtime C#/WinRT : une ciblant .NET Standard 2.0 et une ciblant .NET 5.0. Une seule de ces versions est déployée, en fonction du framework cible d’une application. 

* Le ciblage de .NET Standard 2.0 est approprié pour les applications multiplateformes de bas niveau qui souhaitent apporter des fonctionnalités d’éclairage sur Windows.
* Cibler .NET 5.0 est recommandé pour les applications Windows modernes nécessitant un garbage collection correct sur des références d’objets natives, comme les applications XAML.

Un assembly d’interopérabilité peut garantir que la version correcte du runtime C#/WinRT est déployée pour une application en incluant une condition `targetFramework` dans le fichier nuspec.

```xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2013/05/nuspec.xsd">
  <metadata>
    <dependencies>
      <group targetFramework=".NETStandard2.0">
        <dependency id="Microsoft.Windows.CsWinRT" version="0.1.0" />
      </group>
      <group targetFramework=".NET5.0">
        <dependency id="Microsoft.Windows.CsWinRT" version="0.1.0" />
      </group>
    </dependencies>
  </metadata>
</package>
```

> [!NOTE]
> Le moniker de framework cible pour .NET 5.0 passe de « NETCoreApp5.0 » à « .NET5.0 ». Les préversions C#/WinRT utilisent l’un ou l’autre.

## <a name="reference-an-interop-assembly"></a>Référencer un assembly d’interopérabilité

En général, les assemblys d’interopérabilité C#/WinRT sont référencés par les projets d’application. Mais ils peuvent également être, à leur tour, référencés par des assemblys d’interopérabilité intermédiaires. Par exemple, l’assembly d’interopérabilité WinUI fait référence à l’assembly d’interopérabilité du SDK Windows.

Pour consommer des types C#/WinRT projetés, ajoutez une référence au package NuGet C#/WinRT d’interopérabilité approprié à votre projet. Cela entraîne l’ajout de l’assembly d’interopérabilité et de l’assembly du runtime C#/WinRT au projet.

Si vous distribuez un composant WinRT tiers sans assembly d’interopérabilité officiel, un projet d’application peut suivre la procédure de [création d’un assembly d’interopérabilité](#create-an-interop-assembly) pour générer ses propres sources de projection privées. Nous ne recommandons pas cette approche, car elle peut produire des projections en conflit du même type dans un processus. La création de package NuGet, qui suit le schéma de [gestion sémantique de version](https://semver.org), est conçue pour éviter cela. Utilisez de préférence un assembly d’interopérabilité tiers officiel.

### <a name="winrt-type-activation"></a>Activation du type WinRT

C#/WinRT prend en charge l’activation de types WinRT hébergés par le système d’exploitation, ainsi que des composants tiers comme [Win2D](https://www.nuget.org/packages/Win2D.uwp/). La prise en charge de l’activation de composants tiers dans une application de bureau est rendue possible avec l’[activation WinRT sans inscription](https://blogs.windows.com/windowsdeveloper/2019/04/30/enhancing-non-packaged-desktop-apps-using-windows-runtime-components/), disponible dans Windows 10 version 1903 et les ultérieures. Cela peut également nécessiter l’utilisation du package des [redirecteurs VCRT](https://www.nuget.org/packages/Microsoft.VCRTForwarders.140/) si le composant a été conçu pour cibler les applications UWP.

C#/WinRT fournit également un chemin d’activation de secours si Windows ne parvient pas à activer le type comme décrit ci-dessus. Dans ce cas, C#/WinRT tente de localiser une DLL d’implémentation native basée sur le nom complet du type, en supprimant progressivement des éléments. Par exemple, la logique de secours tentera d’activer le type Contoso.Controls.Widget à partir des modules suivants, dans l’ordre :

1. Contoso.Controls.Widget.dll
2. Contoso.Controls.dll
3. Contoso.dll

C#/WinRT utilise l’[autre ordre de recherche de LoadLibrary](/windows/win32/dlls/dynamic-link-library-search-order#alternate-search-order-for-desktop-applications) pour localiser une DLL d’implémentation. Une application basée sur ce comportement de secours doit empaqueter la DLL d’implémentation en même temps que le module d’application.

## <a name="known-issues"></a>Problèmes connus

Certains problèmes de performances liés à l’interopérabilité ont été identifiés dans la préversion actuelle de C#/WinRT. Ils seront traités avant la version finale qui sera publiée fin 2020.

Si vous rencontrez des problèmes fonctionnels avec le package NuGet C#/WinRT, le compilateur cswinrt.exe ou les sources de projection générées, signalez-les-nous par le biais de la [page relative aux problèmes liées à C#/WinRT](https://github.com/microsoft/CsWinRT/issues).

## <a name="additional-resources"></a>Ressources supplémentaires

* [Dépôt GitHub C#/WinRT](https://aka.ms/cswinrt/repo)