---
title: Composants de Windows Runtime réparties pour une application UWP chargée
description: Ce document traite d’une fonctionnalité ciblée par l’entreprise, prise en charge par Windows 10, qui permet aux applications .NET tactiles d’utiliser le code existant responsable des opérations clés critiques pour l’entreprise.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 81b3930c-6af9-406d-9d1e-8ee6a13ec38a
ms.localizationpriority: medium
ms.openlocfilehash: b28df646bb505889626ced8591c5ef9e6ece3f44
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690349"
---
# <a name="brokered-windows-runtime-components-for-a-side-loaded-uwp-app"></a>Composants de Windows Runtime réparties pour une application UWP chargée

Cet article traite d’une fonctionnalité ciblée par l’entreprise, prise en charge par Windows 10, qui permet aux applications .NET tactiles d’utiliser le code existant responsable des opérations clés critiques pour l’entreprise.

## <a name="introduction"></a>Introduction

>**Notez**  l’exemple de code qui accompagne ce document peut être téléchargé pour [Visual Studio 2015 & 2017](https://aka.ms/brokeredsample). Le modèle Microsoft Visual Studio pour générer des composants de Windows Runtime réparties peut être téléchargé ici : [modèle Visual Studio 2015 ciblant des applications Windows universelles pour Windows 10](https://marketplace.visualstudio.com/items?itemName=vs-publisher-713547.VS2015TemplateBrokeredComponents)

Windows comprend une nouvelle fonctionnalité appelée *répartie des composants de Windows Runtime pour les applications à chargement indépendant*. Nous utilisons le terme IPC (communication entre processus) pour décrire la possibilité d’exécuter des ressources logicielles de bureau existantes dans un processus (composant de bureau) tout en interagissant avec ce code dans une application UWP. Il s’agit d’un modèle familier pour les développeurs d’entreprise, car les applications de base de données et les applications utilisant les services NT dans Windows partagent une architecture multiprocessus similaire.

Le chargement secondaire de l’application est un composant essentiel de cette fonctionnalité.
Les applications spécifiques à l’entreprise n’ont pas de place dans le Microsoft Store grand public et les entreprises ont des exigences très spécifiques en matière de sécurité, de confidentialité, de distribution, d’installation et de maintenance. Par conséquent, le modèle de chargement secondaire est à la fois un impératif de ceux qui utilisent cette fonctionnalité et un détail d’implémentation critique.

Les applications centrées sur les données sont une cible clé pour cette architecture d’application. Les règles d’entreprise existantes ensconced, par exemple, dans SQL Server, sont une partie courante du composant Desktop. Ce n’est certainement pas le seul type de fonctionnalité qui peut être offerts par le composant Desktop, mais une grande partie de la demande pour cette fonctionnalité est liée aux données existantes et à la logique métier.

Enfin, étant donné l’énorme pénétration du Runtime .NET et du langage C\# dans le développement en entreprise, cette fonctionnalité a été développée avec l’accent sur l’utilisation de .NET pour l’application UWP et le composant Desktop. Bien qu’il existe d’autres langages et runtimes possibles pour l’application UWP, l’exemple qui l’accompagne illustre uniquement C\#et se limite exclusivement au Runtime .NET.

## <a name="application-components"></a>Composants de l’application

>**Notez**  cette fonctionnalité est exclusivement réservée à l’utilisation de .net. L’application cliente et le composant Desktop doivent être créés à l’aide de .NET.

**Modèle d'application**

Cette fonctionnalité est basée sur l’architecture d’application générale appelée MVVM (Model View View-Model). Par conséquent, il est supposé que le « modèle » est entièrement hébergé dans le composant du bureau. Par conséquent, il doit être immédiatement évident que le composant de bureau sera « headless » (c’est-à-dire qu’il ne contient aucune interface utilisateur). La vue sera entièrement contenue dans l’application d’entreprise chargée côte à côte. Bien qu’il ne soit pas nécessaire que cette application soit générée avec la construction « View-Model », nous pensons que l’utilisation de ce modèle sera courante.

**Composant du Bureau**

Le composant Desktop de cette fonctionnalité est un nouveau type d’application qui est introduit dans le cadre de cette fonctionnalité. Ce composant de bureau ne peut être écrit qu’en C\# et doit cibler .NET 4,6 ou version ultérieure pour Windows 10. Le type de projet est un hybride entre le CLR ciblant UWP, car le format de communication interprocessus comprend des classes et des types UWP, tandis que le composant Desktop est autorisé à appeler toutes les parties de la bibliothèque de classes du Runtime .NET. L’impact sur le projet Visual Studio sera décrit en détail ultérieurement. Cette configuration hybride permet de marshaler les types UWP entre l’application reposant sur les composants du bureau tout en autorisant l’appel du code CLR du Bureau à l’intérieur de l’implémentation du composant de bureau.

**Façon**

Le contrat entre l’application chargée du côté et le composant Desktop est décrit en termes de système de type UWP. Cela implique de déclarer une ou plusieurs classes C\# qui peuvent représenter un UWP. Consultez la rubrique MSDN [création de composants Windows Runtime en c\# et Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/br230301(v=vs.140)) pour connaître la configuration de Windows Runtime classe à l’aide de c\#.

>**Notez**  les énumérations ne sont pas prises en charge dans le contrat de composants de Windows Runtime entre le composant de bureau et l’application chargée du côté.

**Application chargée côte à côte**

L’application chargée côte à côte est une application UWP normale dans tous les cas, à l’exception d’un : elle est chargée au lieu d’être installée via le Microsoft Store. La plupart des mécanismes d’installation sont identiques : le manifeste et le Packaging de l’application sont similaires (un ajout au manifeste est décrit en détail plus loin). Une fois que le chargement secondaire est activé, un script PowerShell simple peut installer les certificats nécessaires et l’application elle-même. Il s’agit de la bonne pratique courante selon laquelle l’application à charge latérale réussit le test de certification WACK qui est inclus dans le menu Projet/magasin de Visual Studio.

>**Remarque** Le chargement indépendant peut être activé dans les paramètres&gt; mise à jour &&gt; de sécurité pour les développeurs.

Un point important à noter est que le mécanisme App Broker fourni dans le cadre de Windows 10 est uniquement 32-bit. Le composant du Bureau doit être 32 bits.
Les applications à charge latérale peuvent être de 64 bits (à condition qu’un proxy 64 bits et 32 bits soient enregistrés), mais cela est atypique. La génération de l’application à charge latérale en C\# à l’aide de la configuration « neutre » normale et la valeur par défaut « préférer 32 bits » crée des applications à chargement indépendant 32 bits.

**Instanciation de serveur et AppDomains**

Chaque application chargée d’un côté reçoit sa propre instance d’un serveur App Broker (appelée « Multi-instanciation »). Le code serveur s’exécute dans un AppDomain unique. Cela permet l’exécution de plusieurs versions de bibliothèques dans des instances distinctes. Par exemple, l’application A a besoin de la version V 1.1 d’un composant et l’application B a besoin de v2. Ils sont séparés par des composants V 1.1 et v2 dans des répertoires de serveur distincts et pointent l’application vers le serveur qui prend en charge la version appropriée souhaitée.

L’implémentation du code serveur peut être partagée entre plusieurs instances du serveur App Broker en pointant plusieurs applications vers le même répertoire de serveur. Il y aura toujours plusieurs instances du serveur App Broker, mais elles exécuteront du code identique. Tous les composants d’implémentation utilisés dans une application unique doivent être présents dans le même chemin d’accès.

## <a name="defining-the-contract"></a>Définition du contrat

La première étape de la création d’une application à l’aide de cette fonctionnalité consiste à créer le contrat entre l’application chargée côte et le composant Desktop. Cette opération doit être effectuée exclusivement à l’aide des types de Windows Runtime.
Heureusement, ceux-ci sont faciles à déclarer à l’aide des classes C\#. Toutefois, il existe des considérations importantes en matière de performances lors de la définition de ces conversations, qui est décrite dans une section ultérieure.

La séquence permettant de définir le contrat est introduite comme suit :

**Étape 1 :** Créez une nouvelle bibliothèque de classes dans Visual Studio. Veillez à créer le projet à l’aide du modèle **bibliothèque de classes** , et non pas du modèle de **composant Windows Runtime** .

Une implémentation suit évidemment, mais cette section couvre uniquement la définition du contrat inter-processus. L’exemple qui l’accompagne comprend la classe suivante (EnterpriseServer.cs), dont la forme de départ ressemble à ceci :

```csharp
namespace Fabrikam
{
    public sealed class EnterpriseServer
    {

        public ILis<String> TestMethod(String input)
        {
            throw new NotImplementedException();
        }
        
        public IAsyncOperation<int> FindElementAsync(int input)
        {
            throw new NotImplementedException();
        }
        
        public string[] RetrieveData()
        {
            throw new NotImplementedException();
        }
        
        public event EventHandler<string> PeriodicEvent;
    }
}
```

Cela définit une classe « EnterpriseServer » qui peut être instanciée à partir de l’application chargée côte à côte. Cette classe fournit les fonctionnalités promis dans le RuntimeClass. Le RuntimeClass peut être utilisé pour générer la référence winmd qui sera incluse dans l’application chargée côte à côte.

**Étape 2 :** Modifiez le fichier projet manuellement pour modifier le type de sortie du projet pour **Windows Runtime composant**.

Pour effectuer cette opération dans Visual Studio, cliquez avec le bouton droit sur le projet nouvellement créé et sélectionnez « décharger le projet », puis cliquez à nouveau avec le bouton droit et sélectionnez « Modifier EnterpriseServer. csproj » pour ouvrir le fichier projet, un fichier XML, à des fins de modification.

Dans le fichier ouvert, recherchez la balise \<OutputType\> et remplacez sa valeur par « winmdobj ».

**Étape 3 :** Créez une règle de génération qui crée un fichier de métadonnées Windows de « référence » (fichier. winmd). autrement dit, n’a aucune implémentation.

**Étape 4 :** Créez une règle de génération qui crée un fichier de métadonnées Windows de « mise en œuvre », c’est-à-dire qui a les mêmes informations de métadonnées, mais qui inclut également l’implémentation de.

Cette opération est effectuée par les scripts suivants. Ajoutez les scripts à la ligne de commande de l’événement après génération dans **Propriétés** du projet > **événements de build**.

> **Notez** que le script est différent selon la version de Windows que vous ciblez (Windows 10) et la version de Visual Studio utilisée.

**Visual Studio 2015**
```cmd
    call "$(DevEnvDir)..\..\vc\vcvarsall.bat" x86 10.0.14393.0

    md "$(TargetDir)"\impl    md "$(TargetDir)"\reference

    erase "$(TargetDir)\impl\*.winmd"
    erase "$(TargetDir)\impl\*.pdb"
    rem erase "$(TargetDir)\reference\*.winmd"

    xcopy /y "$(TargetPath)" "$(TargetDir)impl"
    xcopy /y "$(TargetDir)*.pdb" "$(TargetDir)impl"

    winmdidl /nosystemdeclares /metadata_dir:C:\Windows\System32\Winmetadata "$(TargetPath)"

    midl /metadata_dir "%WindowsSdkDir%UnionMetadata" /iid "$(SolutionDir)BrokeredProxyStub\$(TargetName)_i.c" /env win32 /x86 /h   "$(SolutionDir)BrokeredProxyStub\$(TargetName).h" /winmd "$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(SolutionDir)BrokeredProxyStub\dlldata.c" /proxy "$(SolutionDir)BrokeredProxyStub\$(TargetName)_p.c"  "$(TargetName).idl"
    mdmerge -n 1 -i "$(ProjectDir)bin\$(ConfigurationName)" -o "$(TargetDir)reference" -metadata_dir "%WindowsSdkDir%UnionMetadata" -partial

    rem erase "$(TargetPath)"

```


**Visual Studio 2017**
```cmd
    call "$(DevEnvDir)..\..\vc\auxiliary\build\vcvarsall.bat" x86 10.0.16299.0

    md "$(TargetDir)"\impl
    md "$(TargetDir)"\reference

    erase "$(TargetDir)\impl\*.winmd"
    erase "$(TargetDir)\impl\*.pdb"
    rem erase "$(TargetDir)\reference\*.winmd"

    xcopy /y "$(TargetPath)" "$(TargetDir)impl"
    xcopy /y "$(TargetDir)*.pdb" "$(TargetDir)impl"

    winmdidl /nosystemdeclares /metadata_dir:C:\Windows\System32\Winmetadata "$(TargetPath)"

    midl /metadata_dir "%WindowsSdkDir%UnionMetadata" /iid "$(SolutionDir)BrokeredProxyStub\$(TargetName)_i.c" /env win32 /x86 /h "$(SolutionDir)BrokeredProxyStub\$(TargetName).h" /winmd "$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(SolutionDir)BrokeredProxyStub\dlldata.c" /proxy "$(SolutionDir)BrokeredProxyStub\$(TargetName)_p.c"  "$(TargetName).idl"
    mdmerge -n 1 -i "$(ProjectDir)bin\$(ConfigurationName)" -o "$(TargetDir)reference" -metadata_dir "%WindowsSdkDir%UnionMetadata" -partial

    rem erase "$(TargetPath)"
```

Une fois que la référence **winmd** est créée (dans le dossier « référence » dans le dossier cible du projet), elle est transférée (copiée) à chaque projet d’application à charge latérale et référencé. Cela sera décrit plus en détail dans la section suivante. La structure de projet incorporée dans les règles de génération ci-dessus garantit que l’implémentation et les **winmd** de référence sont dans des répertoires clairement séparés dans la hiérarchie de génération afin d’éviter toute confusion.

## <a name="side-loaded-applications-in-detail"></a>Applications chargées côte à côte
Comme indiqué précédemment, l’application à chargement indépendant est générée comme n’importe quelle autre application UWP, mais il existe un détail supplémentaire : déclaration de la disponibilité des RuntimeClass dans le manifeste de l’application chargée. Cela permet à l’application d’écrire simplement un nouveau pour accéder aux fonctionnalités du composant Desktop. Une nouvelle entrée de manifeste dans la section <Extension> décrit les RuntimeClass implémentés dans le composant Desktop et les informations sur l’emplacement où elles se trouvent. Le contenu de cette déclaration dans le manifeste de l’application est le même pour les applications ciblant Windows 10. Par exemple :

```XML
<Extension Category="windows.activatableClass.inProcessServer">
    <InProcessServer>
        <Path>clrhost.dll</Path>
        <ActivatableClass ActivatableClassId="Fabrikam.EnterpriseServer" ThreadingModel="both">
            <ActivatableClassAttribute Name="DesktopApplicationPath" Type="string" Value="c:\test" />
        </ActivatableClass>
    </InProcessServer>
</Extension>
```

La catégorie est inProcessServer, car la catégorie outOfProcessServer contient plusieurs entrées qui ne sont pas applicables à cette configuration d’application. Notez que le composant <Path> doit toujours contenir clrhost. dll (Toutefois, cela n’est **pas** appliqué et la spécification d’une autre valeur échouera de manière non définie).

La section <ActivatableClass> est identique à un vrai RuntimeClass in-process préféré par un composant Windows Runtime dans le package de l’application. <ActivatableClassAttribute> est un nouvel élément, et les attributs Name = "DesktopApplicationPath" et type = "String" sont obligatoires et invariants. L’attribut value pointe vers l’emplacement où l’implémentation du composant de bureau winmd réside (plus de détails sur ce point dans la section suivante). Chaque RuntimeClass préféré par le composant Desktop doit avoir sa propre arborescence d’éléments <ActivatableClass>. L’ActivatableClassId doit correspondre au nom complet de l’espace de noms de RuntimeClass.

Comme mentionné dans la section « Définition du contrat », une référence de projet doit être apportée au winmd de référence du composant de bureau. Le système de projet Visual Studio crée normalement une structure de répertoire à deux niveaux portant le même nom. Dans l’exemple, il s’agit de EnterpriseIPCApplication\\EnterpriseIPCApplication. Le **winmd** de référence est copié manuellement vers ce répertoire de deuxième niveau, puis la boîte de dialogue références de projet est utilisée (cliquez sur le **bouton Parcourir.** pour rechercher et référencer ce **winmd**. Après cela, l’espace de noms de niveau supérieur du composant Desktop (par exemple, Fabrikam) doit apparaître en tant que nœud de niveau supérieur dans la partie références du projet.

>**Remarque** Il est très important d’utiliser la **référence winmd** dans l’application chargée côte à côte. Si vous transportez par mégarde le **winmd d’implémentation** vers le répertoire d’application chargé et que vous le référencez, vous recevrez probablement une erreur liée à « impossible de trouver IStringable ». Il s’agit de l’un des signes indiquant que le mauvais **winmd** a été référencé. Les règles de génération après génération de l’application de serveur IPC (détaillées dans la section suivante) séparent soigneusement ces deux **winmd** dans des répertoires distincts.

Variables d’environnement (en particulier% ProgramFiles%) peut être utilisé dans <ActivatableClassAttribute Value="path">. Comme indiqué précédemment, l’app Broker ne prend en charge que 32 bits, de sorte que% ProgramFiles% sera résolu en C :\\Program Files (x86) si l’application est exécutée sur un système d’exploitation 64 bits.

## <a name="desktop-ipc-server-detail"></a>Détails du serveur IPC du Bureau

Les deux sections précédentes décrivent la déclaration de la classe et les mécanismes de transport de la référence **winmd** vers le projet d’application à charge latérale. La majeure partie du travail restant dans le composant Desktop implique une implémentation. Étant donné que l’ensemble du composant Desktop est en mesure d’appeler le code du Bureau (généralement pour réutiliser les ressources de code existantes), le projet doit être configuré de façon spéciale.
Normalement, un projet Visual Studio utilisant .NET utilise l’un des deux « profils ».
La première concerne le bureau (". NETFramework») et l’autre ciblent la partie application UWP du CLR («». Netcore»). Un composant de bureau dans cette fonctionnalité est un hybride entre ces deux. Par conséquent, la section des références est construite avec soin pour mélanger ces deux profils.

Un projet d’application UWP normal ne contient pas de références de projet explicites, car l’intégralité de la surface de l’API Windows Runtime est implicitement incluse.
Normalement, seules les autres références entre projets sont effectuées. Toutefois, un projet de composant de bureau a un ensemble de références très spécial. Il démarre la vie comme un projet « Bibliothèque de classes de\\de bureau classique » et, par conséquent, est un projet de bureau. Par conséquent, les références explicites à l’API Windows Runtime (via des références aux fichiers **winmd** ) doivent être effectuées. Ajoutez les références appropriées comme indiqué ci-dessous.

```XML
<ItemGroup>
    <!-- These reference are added by VS automatically when you create a Class Library project-->
    <Reference Include="System" />
    <Reference Include="System.Core" />
    <Reference Include="System.Xml.Linq" />
    <Reference Include="System.Data.DataSetExtensions" />
    <Reference Include="Microsoft.CSharp" />
    <Reference Include="System.Data" />
    <Reference Include="System.Net.Http" />
<Reference Include="System.Xml" />
    <!-- These reference should be added manually by editing .csproj file-->

    <Reference Include="System.Runtime.WindowsRuntime, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089, processorArchitecture=MSIL">
      <HintPath>$(MSBuildProgramFiles32)\Microsoft SDKs\NETCoreSDK\System.Runtime.WindowsRuntime\4.0.10\lib\netcore50\System.Runtime.WindowsRuntime.dll</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\UnionMetadata\Facade\Windows.WinMD</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Foundation.FoundationContract">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\References\Windows.Foundation.FoundationContract\1.0.0.0\Windows.Foundation.FoundationContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Foundation.UniversalApiContract">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\References\Windows.Foundation.UniversalApiContract\1.0.0.0\Windows.Foundation.UniversalApiContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Networking.Connectivity.WwanContract">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\References\Windows.Networking.Connectivity.WwanContract\1.0.0.0\Windows.Networking.Connectivity.WwanContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.ActivatedEventsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.ActivatedEventsContract\1.0.0.0\Windows.ApplicationModel.Activation.ActivatedEventsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.ActivationCameraSettingsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.ActivationCameraSettingsContract\1.0.0.0\Windows.ApplicationModel.Activation.ActivationCameraSettingsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.ContactActivatedEventsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.ContactActivatedEventsContract\1.0.0.0\Windows.ApplicationModel.Activation.ContactActivatedEventsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.WebUISearchActivatedEventsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.WebUISearchActivatedEventsContract\1.0.0.0\Windows.ApplicationModel.Activation.WebUISearchActivatedEventsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Background.BackgroundAlarmApplicationContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Background.BackgroundAlarmApplicationContract\1.0.0.0\Windows.ApplicationModel.Background.BackgroundAlarmApplicationContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Calls.LockScreenCallContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Calls.LockScreenCallContract\1.0.0.0\Windows.ApplicationModel.Calls.LockScreenCallContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Resources.Management.ResourceIndexerContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Resources.Management.ResourceIndexerContract\1.0.0.0\Windows.ApplicationModel.Resources.Management.ResourceIndexerContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Search.Core.SearchCoreContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Search.Core.SearchCoreContract\1.0.0.0\Windows.ApplicationModel.Search.Core.SearchCoreContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Search.SearchContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Search.SearchContract\1.0.0.0\Windows.ApplicationModel.Search.SearchContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Wallet.WalletContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Wallet.WalletContract\1.0.0.0\Windows.ApplicationModel.Wallet.WalletContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Custom.CustomDeviceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Custom.CustomDeviceContract\1.0.0.0\Windows.Devices.Custom.CustomDeviceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Portable.PortableDeviceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Portable.PortableDeviceContract\1.0.0.0\Windows.Devices.Portable.PortableDeviceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Printers.Extensions.ExtensionsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Printers.Extensions.ExtensionsContract\1.0.0.0\Windows.Devices.Printers.Extensions.ExtensionsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Printers.PrintersContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Printers.PrintersContract\1.0.0.0\Windows.Devices.Printers.PrintersContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Scanners.ScannerDeviceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Scanners.ScannerDeviceContract\1.0.0.0\Windows.Devices.Scanners.ScannerDeviceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Sms.LegacySmsApiContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Sms.LegacySmsApiContract\1.0.0.0\Windows.Devices.Sms.LegacySmsApiContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Gaming.Preview.GamesEnumerationContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Gaming.Preview.GamesEnumerationContract\1.0.0.0\Windows.Gaming.Preview.GamesEnumerationContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Globalization.GlobalizationJapanesePhoneticAnalyzerContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Globalization.GlobalizationJapanesePhoneticAnalyzerContract\1.0.0.0\Windows.Globalization.GlobalizationJapanesePhoneticAnalyzerContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Graphics.Printing3D.Printing3DContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Graphics.Printing3D.Printing3DContract\1.0.0.0\Windows.Graphics.Printing3D.Printing3DContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Management.Deployment.Preview.DeploymentPreviewContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Management.Deployment.Preview.DeploymentPreviewContract\1.0.0.0\Windows.Management.Deployment.Preview.DeploymentPreviewContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Management.Workplace.WorkplaceSettingsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Management.Workplace.WorkplaceSettingsContract\1.0.0.0\Windows.Management.Workplace.WorkplaceSettingsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Capture.AppCaptureContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Capture.AppCaptureContract\1.0.0.0\Windows.Media.Capture.AppCaptureContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Capture.CameraCaptureUIContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Capture.CameraCaptureUIContract\1.0.0.0\Windows.Media.Capture.CameraCaptureUIContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Devices.CallControlContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Devices.CallControlContract\1.0.0.0\Windows.Media.Devices.CallControlContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.MediaControlContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.MediaControlContract\1.0.0.0\Windows.Media.MediaControlContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Playlists.PlaylistsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Playlists.PlaylistsContract\1.0.0.0\Windows.Media.Playlists.PlaylistsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Protection.ProtectionRenewalContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Protection.ProtectionRenewalContract\1.0.0.0\Windows.Media.Protection.ProtectionRenewalContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Networking.NetworkOperators.LegacyNetworkOperatorsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Networking.NetworkOperators.LegacyNetworkOperatorsContract\1.0.0.0\Windows.Networking.NetworkOperators.LegacyNetworkOperatorsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Networking.Sockets.ControlChannelTriggerContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Networking.Sockets.ControlChannelTriggerContract\1.0.0.0\Windows.Networking.Sockets.ControlChannelTriggerContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Security.EnterpriseData.EnterpriseDataContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Security.EnterpriseData.EnterpriseDataContract\1.0.0.0\Windows.Security.EnterpriseData.EnterpriseDataContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Security.ExchangeActiveSyncProvisioning.EasContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Security.ExchangeActiveSyncProvisioning.EasContract\1.0.0.0\Windows.Security.ExchangeActiveSyncProvisioning.EasContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Services.Maps.GuidanceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Services.Maps.GuidanceContract\1.0.0.0\Windows.Services.Maps.GuidanceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Services.Maps.LocalSearchContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Services.Maps.LocalSearchContract\1.0.0.0\Windows.Services.Maps.LocalSearchContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.Profile.SystemManufacturers.SystemManufacturersContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.Profile.SystemManufacturers.SystemManufacturersContract\1.0.0.0\Windows.System.Profile.SystemManufacturers.SystemManufacturersContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.Profile.ProfileHardwareTokenContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.Profile.ProfileHardwareTokenContract\1.0.0.0\Windows.System.Profile.ProfileHardwareTokenContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.Profile.ProfileRetailInfoContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.Profile.ProfileRetailInfoContract\1.0.0.0\Windows.System.Profile.ProfileRetailInfoContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.UserProfile.UserProfileContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.UserProfile.UserProfileContract\1.0.0.0\Windows.System.UserProfile.UserProfileContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.UserProfile.UserProfileLockScreenContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.UserProfile.UserProfileLockScreenContract\1.0.0.0\Windows.System.UserProfile.UserProfileLockScreenContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.ApplicationSettings.ApplicationsSettingsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.ApplicationSettings.ApplicationsSettingsContract\1.0.0.0\Windows.UI.ApplicationSettings.ApplicationsSettingsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.Core.AnimationMetrics.AnimationMetricsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.Core.AnimationMetrics.AnimationMetricsContract\1.0.0.0\Windows.UI.Core.AnimationMetrics.AnimationMetricsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.Core.CoreWindowDialogsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.Core.CoreWindowDialogsContract\1.0.0.0\Windows.UI.Core.CoreWindowDialogsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.Xaml.Hosting.HostingContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.Xaml.Hosting.HostingContract\1.0.0.0\Windows.UI.Xaml.Hosting.HostingContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Web.Http.Diagnostics.HttpDiagnosticsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Web.Http.Diagnostics.HttpDiagnosticsContract\1.0.0.0\Windows.Web.Http.Diagnostics.HttpDiagnosticsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
</ItemGroup>
```

Les références ci-dessus sont un mélange attentif de eferences qui sont essentiels au bon fonctionnement de ce serveur hybride. Le protocole consiste à ouvrir le fichier. csproj (comme décrit dans Comment modifier le projet OutputType) et à ajouter ces références si nécessaire.

Une fois les références correctement configurées, la tâche suivante consiste à implémenter les fonctionnalités du serveur. Consultez la rubrique [meilleures pratiques pour l’interopérabilité avec les composants de Windows Runtime (applications UWP utilisantC++ C\#/vb/et XAML)](https://docs.microsoft.com/previous-versions/windows/apps/hh750311(v=win.10)).
La tâche consiste à créer une Windows Runtime dll de composant qui peut appeler le code du bureau dans le cadre de son implémentation. L’exemple qui l’accompagne comprend les principaux modèles utilisés dans Windows Runtime :

-   Appels de méthode

-   Windows Runtime des sources d’événements par le composant Desktop

-   Opérations asynchrones Windows Runtime

-   Retour de tableaux de types de base

**Installer**

Pour installer l’application, copiez l’implémentation **winmd** dans le répertoire correct spécifié dans le manifeste de l’application chargée de l’exécution latérale : <ActivatableClassAttribute>value = "path". Copiez également les fichiers de support associés et la dll proxy/stub (ce dernier détail est couvert ci-dessous). Si vous ne parvenons pas à copier le **winmd** d’implémentation vers l’emplacement du répertoire du serveur, tous les appels de l’application chargée du côté vers New sur le RuntimeClass lèvent une erreur « classe non inscrite ». Si vous ne parvenez pas à installer le proxy/stub (ou l’échec de l’inscription), tous les appels échouent, sans aucune valeur de retour. Cette dernière erreur n’est souvent **pas** associée à des exceptions visibles.
Si des exceptions sont observées en raison de cette erreur de configuration, elles peuvent faire référence à « cast non valide ».

**Considérations relatives à l’implémentation du serveur**

Le serveur de Windows Runtime de bureau peut être considéré comme un « Worker » ou une « tâche ». Chaque appel au serveur fonctionne sur un thread qui n’est pas une interface utilisateur et tout le code doit être compatible avec plusieurs threads et sécurisé. La partie de l’application chargée du côté qui appelle la fonctionnalité du serveur est également importante. Il est essentiel d’éviter toujours d’appeler du code de longue durée à partir de n’importe quel thread d’interface utilisateur dans l’application chargée. Pour ce faire, deux méthodes principales s’offrent à vous :

1.  Si vous appelez la fonctionnalité serveur à partir d’un thread d’interface utilisateur, utilisez toujours un modèle asynchrone dans la surface d’exposition publique et l’implémentation du serveur.

2.  Appelez les fonctionnalités du serveur à partir d’un thread d’arrière-plan dans l’application chargée.

**Windows Runtime Async sur le serveur**

Étant donné la nature interprocessus du modèle d’application, les appels au serveur ont une charge de traitement plus importante que le code qui s’exécute exclusivement en mode in-process. Il est généralement possible d’appeler une propriété simple qui retourne une valeur en mémoire, car elle s’exécutera suffisamment rapidement pour que le blocage du thread d’interface utilisateur ne soit pas un problème. Toutefois, tout appel qui implique des e/s de n’importe quel type (y compris la gestion des fichiers et les récupérations de base de données) peut potentiellement bloquer le thread d’interface utilisateur appelant et entraîner l’arrêt de l’application en raison d’une absence de réponse. En outre, les appels de propriété sur les objets sont déconseillés dans cette architecture d’application pour des raisons de performances.
Ce sujet est abordé plus en détail dans la section suivante.

Un serveur correctement implémenté implémente normalement les appels effectués directement à partir de threads de l’interface utilisateur via le modèle asynchrone Windows Runtime. Vous pouvez l’implémenter en suivant ce modèle. Tout d’abord, la déclaration (encore une fois, à partir de l’exemple fourni) :

```csharp
public IAsyncOperation<int> FindElementAsync(int input)
```

Cela déclare une opération asynchrone Windows Runtime qui retourne un entier.
L’implémentation de l’opération asynchrone prend normalement la forme suivante :

```csharp
return Task<int>.Run( () =>
{
    int retval = ...
    // execute some potentially long-running code here 
}).AsAsyncOperation<int>();

```

>**Remarque** Il est courant d’attendre d’autres opérations potentiellement longues pendant l’écriture de l’implémentation. Dans ce cas, le code **Task. Run** doit être déclaré :

```csharp
return Task<int>.Run(async () =>
{
    int retval = ...
    // execute some potentially long-running code here 
    await ... // some other WinRT async operation or Task
}).AsAsyncOperation<int>();
```

Les clients de cette méthode Async peuvent attendre cette opération comme toute autre opération de Windows Runtime aysnc.

**Appeler des fonctionnalités serveur à partir d’un thread d’arrière-plan d’application**

Étant donné qu’il est courant que le client et le serveur soient écrits par la même organisation, il est possible d’adopter une pratique de programmation que tous les appels au serveur seront effectués par un thread d’arrière-plan dans l’application chargée. Un appel direct qui collecte un ou plusieurs lots de données à partir du serveur peut être effectué à partir d’un thread d’arrière-plan. Lorsque le ou les résultats sont complètement récupérés, le lot de données qui est en mémoire dans le processus d’application peut généralement être récupéré directement à partir du thread d’interface utilisateur. Les objets C @ no__t_0_ sont naturellement agiles entre les threads d’arrière-plan et les threads d’interface utilisateur, ce qui est particulièrement utile pour ce type de modèle d’appel.\#

## <a name="creating-and-deploying-the-windows-runtime-proxy"></a>Création et déploiement du proxy Windows Runtime

Étant donné que l’approche IPC implique le marshaling des interfaces Windows Runtime entre deux processus, un proxy Windows Runtime et un stub inscrits globalement doivent être utilisés.

**Création du proxy dans Visual Studio**

Le processus de création et d’inscription de proxies et de stubs à utiliser dans un package d’application UWP standard est décrit dans la rubrique [déclenchement d’événements dans des composants de Windows Runtime](https://docs.microsoft.com/previous-versions/windows/apps/dn169426(v=vs.140)).
Les étapes décrites dans cet article sont plus complexes que le processus décrit ci-dessous, car cela implique l’inscription du proxy/stub dans le package d’application (par opposition à son inscription globale).

**Étape 1 :** À l’aide de la solution pour le projet de composant de bureau, créez un projet proxy/stub dans Visual Studio :

**Solution > Ajouter > projet C++ > > l’option Sélectionner une dll de la console Win32.**

Pour les étapes ci-dessous, nous supposons que le composant serveur est appelé **MyWinRTComponent**.

**Étape 3 :** Supprimez tous les fichiers CPP/H du projet.

**Étape 4 :** La section précédente « définition du contrat » contient une commande après génération qui exécute **winmdidl. exe**, **MIDL. exe**, **mdmerge. exe**, et ainsi de suite. L’une des sorties de l’étape MIDL de cette commande après génération génère quatre sorties importantes :

a) dlldata. c

b) un fichier d’en-tête (par exemple, MyWinRTComponent. h)

c) un fichier \*\_i. c (par exemple, MyWinRTComponent\_i. c)

d) un \*\_fichier p. c (par exemple, MyWinRTComponent\_p. c)

**Étape 5 :** Ajoutez ces quatre fichiers générés au projet « MyWinRTProxy ».

**Étape 6 :** Ajoutez un fichier def au projet « MyWinRTProxy » **(projet > ajoutez un nouvel élément > Code > fichier de définition de module**) et mettez à jour le contenu pour qu’il soit :

Bibliothèque MyWinRTComponent. proxies. dll

VENTES

DllCanUnloadNow privé

DllGetClassObject privé

DllRegisterServer privé

DllUnregisterServer privé

**Étape 7 :** Ouvrez les propriétés du projet « MyWinRTProxy » :

**Propriétés Comfiguration > général > nom cible :**

MyWinRTComponent. proxies

**Définitions deC++ préprocesseur C/> > Ajouter**

32\_WINDOWS ; INSCRIRE\_PROXY\_DLL»

**En-C++ tête C/> précompilé : sélectionnez « pas d’utilisation de l’en-tête précompilé »**

**Éditeur de liens > général > ignorer la bibliothèque d’importation : sélectionnez « Oui »**

**> D’entrée de l’éditeur de liens > des dépendances supplémentaires : Ajouter rpcrt4. lib ; runtimeobject. lib**

**Éditeur de liens > les métadonnées Windows > générer les métadonnées Windows : sélectionnez « non »**

**Étape 8 :** Générez le projet « MyWinRTProxy ».

**Déploiement du proxy**

Le proxy doit être inscrit globalement. La méthode la plus simple consiste à faire en sorte que votre processus d’installation appelle DllRegisterServer sur la dll du proxy. Notez que dans la mesure où la fonctionnalité prend uniquement en charge les serveurs conçus pour x86 (c’est-à-dire, aucune prise en charge de 64 bits), la configuration la plus simple consiste à utiliser un serveur 32 bits, un proxy 32 bits et une application de chargement côté 32 bits. Le proxy se situe normalement en regard de l’implémentation **winmd** pour le composant Desktop.

Une étape de configuration supplémentaire doit être effectuée. Pour que le processus chargé de manière autonome puisse charger et exécuter le proxy, le répertoire doit être marqué « lecture/exécution » pour ALL_APPLICATION_PACKAGES. Cette opération s’effectue via l’outil en ligne de commande **icacls. exe** . Cette commande doit s’exécuter dans le répertoire où se trouve le **winmd** d’implémentation et la dll de proxy/stub :

*icacls. /T/Grant \*S-1-15-2-1 : RX*

## <a name="patterns-and-performance"></a>Modèles et performances

Il est très important que les performances du transport inter-processus soient surveillées avec soin. Un appel inter-processus est au moins deux fois plus onéreux qu’un appel in-process. Créer des conversations « bavardes » ou effectuer des transferts répétés d’objets volumineux tels que des images bitmap peut entraîner des performances d’application inattendues et indésirables.

Voici une liste non exhaustive des éléments à prendre en compte :

-   Les appels de méthode synchrones du thread d’interface utilisateur de l’application au serveur doivent toujours être évités. Appelez la méthode à partir d’un thread d’arrière-plan dans l’application, puis utilisez CoreWindowDispatcher pour obtenir les résultats dans le thread d’interface utilisateur, si nécessaire.

-   L’appel d’opérations asynchrones à partir d’un thread d’interface utilisateur d’application est sécurisé, mais tenez compte des problèmes de performances décrits ci-dessous.

-   Le transfert en bloc de résultats réduit les échanges excessifs inter-processus. Cela est normalement effectué à l’aide de la construction de tableau Windows Runtime.

-   Le retour d’une *liste<T>* où *t* est un objet à partir d’une opération asynchrone ou d’une extraction de propriété, provoque un grand nombre de échanges excessifs inter-processus. Par exemple, supposons que vous reveniez à une*liste&lt;personnes&gt;* objets. Chaque passe d’itération est un appel interprocessus. Chaque objet *People* retourné est représenté par un proxy et chaque appel à une méthode ou à une propriété sur cet objet individuel entraîne un appel interprocessus. Par conséquent, une liste « inoffensif » *&lt;personnes&gt;* objet où *nombre* est élevé entraîne un grand nombre d’appels lents. De meilleures performances résultent du transfert en bloc de structs du contenu dans un tableau. Par exemple :

```csharp
struct PersonStruct
{
    String LastName;
    String FirstName;
    int Age;
   // etc.
}
```

Renvoyez ensuite * PersonStruct\[\]* au lieu de *List&lt;PersonObject&gt;* .
Cela obtient toutes les données dans un « tronçon » interprocessus.

Comme pour toutes les considérations relatives aux performances, les mesures et les tests sont critiques. Dans l’idéal, la télémétrie doit être insérée dans les différentes opérations pour déterminer le temps nécessaire. Il est important de mesurer à travers une plage : par exemple, combien de temps faut-il pour utiliser tous les objets de *personnes* pour une requête particulière dans l’application chargée.

Une autre technique est le test de charge de variable. Cela peut être fait en plaçant des hooks de test de performances dans l’application qui introduisent des charges de retard variables dans le traitement du serveur. Cela peut simuler différents types de charge et la réaction de l’application pour faire varier les performances du serveur.
L’exemple montre comment mettre des retards de temps dans le code à l’aide de techniques asynchrones appropriées. Le nombre exact de retards à injecter et la plage de randomisation à placer dans cette charge artificielle varient en fonction de la conception de l’application et de l’environnement prévu dans lequel l’application s’exécutera.

## <a name="development-process"></a>Processus de développement

Lorsque vous apportez des modifications au serveur, vous devez vous assurer que toutes les instances qui étaient en cours d’exécution ne sont plus en cours d’exécution. COM finira par nettoyer le processus, mais le minuteur d’arrêt prend plus de temps que le développement itératif. Par conséquent, la mise à mort d’une instance précédemment exécutée est une étape normale pendant le développement. Cela nécessite que le développeur effectue le suivi de l’instance Dllhost qui héberge le serveur.

Le processus serveur peut être trouvé et supprimé à l’aide du gestionnaire des tâches ou d’autres applications tierces. L’outil en ligne de commande * * TaskList. exe * * est également inclus et a une syntaxe flexible, par exemple :

  
 | **Commande** | **Action** |
 | ------------| ---------- |
 | TaskList | Répertorie tous les processus en cours d’exécution dans l’ordre approximatif de l’heure de création, avec les processus créés le plus récemment près du bas. |
 | TaskList/FI "IMAGENAME EQ Dllhost. exe"/M | Répertorie des informations sur toutes les instances de Dllhost. exe. Le commutateur/M répertorie les modules qu’ils ont chargés. |
 | TaskList/FI "PID eq 12564"/M | Vous pouvez utiliser cette option pour interroger le fichier Dllhost. exe si vous connaissez son PID. |

La liste des modules d’un serveur Service Broker doit répertorier *clrhost. dll* dans sa liste de modules chargés.

## <a name="resources"></a>Ressources

-   [Modèles de projet de composant WinRT réparties pour Windows 10 et VS 2015](https://marketplace.visualstudio.com/items?itemName=vs-publisher-713547.VS2015TemplateBrokeredComponents)

-   [Exemple de composant WinRT répartie NorthwindRT](https://go.microsoft.com/fwlink/p/?LinkID=397349)

-   [Diffusion d’applications de Microsoft Store fiables et fiables](https://go.microsoft.com/fwlink/p/?LinkID=393644)

-   [Contrats et extensions des applications (applications du Windows Store)](https://docs.microsoft.com/previous-versions/windows/apps/hh464906(v=win.10))

-   [Comment chargement des applications sur Windows 10](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)

-   [Déploiement d’applications UWP dans des entreprises](https://go.microsoft.com/fwlink/p/?LinkID=264770)

