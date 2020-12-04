---
description: C#/WinRT est un ensemble d’outils qui fournit une prise en charge de la projection WinRT pour le code C#.
title: C#/WinRT
ms.date: 05/19/2020
ms.topic: article
keywords: windows 10, uwp, standard, c#, winrt, cswinrt, projection
ms.localizationpriority: medium
ms.openlocfilehash: 107c85b7e2562edb9995a6bfd76e47904750536b
ms.sourcegitcommit: a15bc17aa0640722d761d0d33f878cb2a822e8ed
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96577091"
---
# <a name="cwinrt"></a>C#/WinRT

C#/WinRT est un kit de ressources empaqueté dans NuGet qui fournit une prise en charge de la projection Windows Runtime (WinRT) pour le langage C#. Une *projection* est une couche de traduction, comme un assembly d’interopérabilité, qui permet de programmer des API WinRT de manière naturelle et familière pour le langage cible. Par exemple, la projection C#/WinRT masque les détails de l’interopérabilité entre les interfaces C# et WinRT, et fournit des mappages de nombreux types WinRT aux équivalents .NET appropriés, comme des chaînes, des URI, des types valeur courants et des collections génériques.

C#/WinRT fournit actuellement une prise en charge de la consommation des types WinRT, et la dernière version vous permet de [créer](#create-an-interop-assembly) et de [référencer](#reference-an-interop-assembly) les assemblys d’interopérabilité WinRT. Les futures versions de C#/WinRT ajouteront une prise en charge de la création de types WinRT en C#.

Pour plus d’informations sur C#/WinRT, consultez le [dépôt GitHub C#/WinRT](https://aka.ms/cswinrt/repo).

## <a name="motivation-for-cwinrt"></a>Motivation pour C#/WinRT

[.NET Core](/dotnet/core/) est le point principal de la plateforme .NET, et .NET 5 est la dernière version majeure. Il s’agit d’un runtime open source multiplateforme qui peut être utilisé pour générer des applications d’appareil, cloud et IoT.

Les versions précédentes de .NET Framework et .NET Core avaient une connaissance intégrée de WinRT, qui est une technologie spécifique à Windows. Pour prendre en charge les objectifs de portabilité et d’efficacité de .NET 5, nous avons retiré la prise en charge de la projection WinRT du compilateur et du runtime .NET, et nous l’avons déplacée dans le kit de ressources C#/WinRT. L’objectif de C#/WinRT est de fournir la parité avec la prise en charge WinRT intégrée proposée par les versions antérieures du compilateur C# et du runtime .NET. Pour plus d’informations, consultez [Mappages .NET des types Windows Runtime](../winrt-components/net-framework-mappings-of-windows-runtime-types.md).

C#/WinRT prend également en charge WinUI 3.0. Cette version de WinUI retire du système d’exploitation les contrôles et fonctionnalités de l’interface utilisateur Microsoft native. Cela permet aux développeurs d’applications d’utiliser les derniers contrôles et visuels disponibles sur Windows 10, version 1803 et sur les versions ultérieures.

Enfin, C#/WinRT est un kit de ressources général conçu pour prendre en charge d’autres scénarios où la prise en charge intégrée de WinRT n’est pas disponible dans le compilateur C# ou le runtime .NET.

## <a name="create-an-interop-assembly"></a>Créer un assembly d’interopérabilité

Les API WinRT sont définies dans les fichiers de métadonnées Windows (*. winmd). Le package NuGet C#/WinRT ([Microsoft.Windows.CsWinRT](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT/)) comprend le compilateur C#/WinRT, **cswinrt.exe**, que vous pouvez utiliser pour traiter les fichiers de métadonnées Windows et générer du code C# .NET 5.0. Vous pouvez compiler ces fichiers sources dans des assemblys d’interopérabilité, de la même façon que [C++/WinRT](../cpp-and-winrt-apis/index.md) génère des en-têtes pour une projection de langage C++. Vous pouvez alors distribuer les assemblys d’interopérabilité C#/WinRT pour les applications à référencer, en même temps que l’assembly de runtime C#/WinRT.

Pour une procédure pas à pas qui montre comment créer et distribuer un assembly d’interopérabilité en tant que package NuGet, consultez [Procédure pas à pas : Générer une projection .NET 5 à partir d’un composant C++/WinRT et mettre à jour le NuGet](net-projection-from-cppwinrt-component.md).

### <a name="invoke-cswinrtexe"></a>Appeler cswinrt.exe

Pour appeler cswinrt.exe à partir d’un projet, installez le dernier [package NuGet C#/WinRT](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT/). Vous pouvez ensuite définir les propriétés d’un projet C#/WinRT dans un projet de **bibliothèque C#** pour générer un assembly d’interopérabilité. Le fragment de projet suivant montre un appel simple de **cswinrt** pour générer des sources de projection pour les types dans l’espace de noms Contoso. Ces sources sont ensuite incluses dans la build du projet.

```xml
<PropertyGroup>
  <CsWinRTIncludes>Contoso</CsWinRTIncludes>
</PropertyGroup>
```

Dans ce projet, vous devez également référencer le package NuGet CsWinRT et les fichiers .winmd spécifiques au projet que vous souhaitez projeter, qu’il s’agisse d’un package NuGet, d’une référence de projet ou d’une référence directe. Par défaut, les espaces de noms **Windows** et **Microsoft** ne sont pas projetés. Pour obtenir la liste complète des propriétés de projet CsWinRT, reportez-vous à la [documentation CsWinRT NuGet](https://github.com/microsoft/CsWinRT/blob/master/nuget/readme.md).

### <a name="distribute-the-interop-assembly"></a>Distribuer l’assembly d’interopérabilité

Un assembly d’interopérabilité est généralement distribué sous la forme d’un package NuGet, avec une dépendance sur le package NuGet C#/WinRT pour l’assembly de runtime C#/WinRT nécessaire, **WinRT.Runtime.dll**.

Pour vous assurer que la version correcte du runtime C#/WinRT est déployée pour les applications .Net 5.0, ajoutez une condition `targetFramework` dans le fichier .nuspec avec une dépendance avec le package NuGet C#/WinRT.

```xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2013/05/nuspec.xsd">
  <metadata>
    <dependencies>
      <group targetFramework="net5.0">
        <dependency id="Microsoft.Windows.CsWinRT" version="1.0.1" />
      </group>
    </dependencies>
  </metadata>
</package>
```

> [!NOTE]
> Le moniker de framework cible pour .NET 5.0 passe de « NETCoreApp5.0 » à « net5.0 ».

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

## <a name="common-errors-with-net-5"></a>Erreurs courantes avec .NET 5+

Vous pouvez rencontrer les erreurs ou avertissements suivants dans un projet généré avec une version du kit SDK .NET antérieure à celle de l’une de ses dépendances.

| Message d'erreur ou d’avertissement | Motif |
|--------------------------|--------|
| System.IO.FileLoadException | Cette erreur d’exécution se produit lors de l’appel d’API dans une bibliothèque qui n’expose pas de types SDK Windows. |
| Avertissement MSB3277 : Détection de conflits entre les différentes versions de Microsoft.Windows.SDK.NET qui n’ont pas pu être résolus. | Cette erreur de build se produit lors du référencement d’une bibliothèque qui expose des types de SDK Windows sur sa surface d’API. |
| [CS1705](/dotnet/csharp/language-reference/compiler-messages/cs1705) : L’assembly 'AssemblyName1' utilise 'TypeName' dont la version est supérieure à celle de l’assembly référencé 'AssemblyName2' | Cette erreur du compilateur de build se produit lors du référencement et de la consommation des types de SDK Windows exposés dans une bibliothèque. |

Pour corriger ces erreurs, mettez à jour votre kit SDK .NET avec la dernière version. Cette mise à jour permet de vous assurer que les versions d’assembly de SDK Windows et de runtime utilisées par votre application sont compatibles avec toutes les dépendances. Ces erreurs peuvent se produire avec des mises à jour de fonctionnalités ou de maintenance anticipées du kit SDK .NET 5, car les correctifs de runtime peuvent demander des mises à jour de vos versions d’assembly.

## <a name="known-issues"></a>Problèmes connus

Les problèmes connus et les changements cassants sont notés dans le [dépôt GitHub C#/WinRT](https://aka.ms/cswinrt/repo).

Si vous rencontrez des problèmes fonctionnels avec le package NuGet C#/WinRT, le compilateur cswinrt.exe ou les sources de projection générées, signalez-les-nous par le biais de la [page relative aux problèmes liées à C#/WinRT](https://github.com/microsoft/CsWinRT/issues).

## <a name="additional-resources"></a>Ressources supplémentaires

* [Dépôt GitHub C#/WinRT](https://aka.ms/cswinrt/repo)
