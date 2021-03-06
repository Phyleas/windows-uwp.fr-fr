---
title: Composants de Windows Runtime réparties pour une application UWP chargée
description: Ce livre blanc présente une fonctionnalité prise en charge par Windows 10, destinée aux entreprises, qui permet aux applications .NET tactiles d’utiliser le code responsable des opérations d’entreprise stratégiques.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 81b3930c-6af9-406d-9d1e-8ee6a13ec38a
ms.localizationpriority: medium
ms.openlocfilehash: de840ca821e573af6522ab1b583f25a6585efb44
ms.sourcegitcommit: aaa72ddeb01b074266f4cd51740eec8d1905d62d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2020
ms.locfileid: "94339657"
---
# <a name="brokered-windows-runtime-components-for-a-side-loaded-uwp-app"></a>Composants de Windows Runtime réparties pour une application UWP chargée

Cet article présente une fonctionnalité prise en charge par Windows 10, destinée aux entreprises, qui permet aux applications .NET tactiles d’utiliser le code responsable des opérations d’entreprise stratégiques.

## <a name="introduction"></a>Introduction

>**Remarque**  L’exemple de code qui accompagne ce document peut être téléchargé pour [Visual Studio 2015 & 2017](https://github.com/Microsoft/Brokered-WinRT-Components). Le modèle Microsoft Visual Studio pour générer des composants de Windows Runtime réparties peut être téléchargé ici : [modèle Visual Studio 2015 ciblant des applications Windows universelles pour Windows 10](https://marketplace.visualstudio.com/items?itemName=vs-publisher-713547.VS2015TemplateBrokeredComponents)

Windows inclut une nouvelle fonctionnalité appelée *composants Windows Runtime du service Broker pour les applications installées hors Windows Store*. Nous utilisons le terme IPC (Inter-Process Communication) pour décrire la capacité à exécuter des composants logiciels de bureau existants dans un processus (composant de bureau) lors de l’interaction avec ce code dans une application UWP. Il s’agit d’un modèle bien connu des développeurs d’entreprise car les applications de base de données et les applications qui utilisent les services NT dans Windows partagent une architecture à plusieurs processus similaire.

L’installation hors Windows Store de l’application est un composant essentiel de cette fonctionnalité.
Les applications spécifiques à l’entreprise n’ont pas de place dans le Microsoft Store grand public et les entreprises ont des exigences très spécifiques en matière de sécurité, de confidentialité, de distribution, d’installation et de maintenance. C’est pourquoi, le modèle d’installation hors Windows Store est un élément requis pour utiliser cette fonctionnalité et également un détail d’implémentation critique.

Les applications centrées sur les données sont une cible clé pour cette architecture d’application. Il est envisagé que les règles d’entreprise existantes utilisées, par exemple, dans SQL Server, fassent partie du composant de bureau. Il ne s’agit pas du seul type de fonctionnalité qui peut être proposé par le composant de bureau, mais cette fonctionnalité est souvent demandée en liaison avec les données existantes et la logique d’entreprise.

Enfin, étant donné la pénétration écrasante du Runtime .NET et du \# langage C dans le développement en entreprise, cette fonctionnalité a été développée avec l’accent sur l’utilisation de .net pour l’application UWP et le composant Desktop. Bien qu’il existe d’autres langages et runtimes possibles pour l’application UWP, l’exemple qui l’accompagne illustre uniquement C \# et est réservé exclusivement au Runtime .net.

## <a name="application-components"></a>Composants d’application

>**Remarque**  Cette fonctionnalité est exclusivement réservée à l’utilisation de .NET. L’application cliente et le composant de bureau doivent être créés à l’aide de .NET.

**Modèle d'application**

Cette fonctionnalité est conçue autour de l’architecture d’application générale connue sous le nom de MVVM (Model View View-Model). C’est pourquoi le « modèle » est considéré comme résidant entièrement dans le composant de bureau. Le composant de bureau sera « headless » (sans périphérique de contrôle), c’est-à-dire qu’il ne contiendra pas d’interface utilisateur. L’affichage sera entièrement contenu dans l’application d’entreprise installée hors Windows Store. Bien que la construction « view-model » ne soit pas une exigence de conception de l’application, il est probable que l’utilisation de ce modèle soit la plus courante.

**Composant de bureau**

Le composant de bureau de cette fonctionnalité est un nouveau type d’application proposé dans le cadre de cette fonctionnalité. Ce composant de bureau peut uniquement être écrit en C \# et doit cibler .net 4,6 ou version ultérieure pour Windows 10. Le type de projet est une solution hybride entre le CLR ciblant UWP, dans la mesure où le format de communication entre processus comprend des types et des classes UWP, alors que le composant de bureau peut appeler toutes les parties de la bibliothèque de classes du runtime .NET. L’impact sur le projet Visual Studio sera décrit en détail ultérieurement. Cette configuration hybride permet le marshaling des types UWP dans l’application conçue sur les composants de bureau tout en permettant l’appel de code CLR de bureau au sein de l’implémentation de composant de bureau.

**Façon**

Le contrat entre l’application installée hors Windows Store et le composant du bureau est décrit en fonction du système de type UWP. Cela implique de déclarer une ou plusieurs \# classes C qui peuvent représenter un UWP. Consultez la rubrique MSDN [création de composants Windows Runtime en c \# et Visual Basic](/previous-versions/windows/apps/br230301(v=vs.140)) pour connaître la configuration requise pour la création d’Windows Runtime classe à l’aide de c \# .

>**Remarque**  Les enums ne sont pas pris en charge dans le contrat de composants Windows Runtime entre le composant de bureau et l’application chargée pour l’instant.

**Application installée hors Windows Store**

L’application chargée côte à côte est une application UWP normale dans tous les cas, à l’exception d’un : elle est chargée au lieu d’être installée via le Microsoft Store. La plupart des mécanismes d’installation sont identiques : le manifeste et le package de l’application sont identiques (un ajout au manifeste est décrit plus en détail ultérieurement). Lorsque l’installation hors Windows Store est activée, un script PowerShell simple peut installer les certificats nécessaires et l’application elle-même. La meilleure pratique standard stipule que l’application installée hors Windows Store doit réussir le test de certification WACK inclus dans le menu Projet/Store de Visual Studio.

>**Remarque** Le chargement indépendant peut être activé dans les paramètres- &gt; mettre à jour & sécurité- &gt; pour les développeurs.

Il est à noter que le mécanisme App Broker n’est fourni dans le cadre de la Mise à jour Windows 10 qu’en version 32 bits. Le composant de bureau doit être 32 bits.
Les applications installées hors Windows Store peuvent être 64 bits (si un proxy 64 bits et un proxy 32 bits sont inscrits), mais cela n’est pas la norme. La création de l’application chargée côte à côte en C \# à l’aide de la configuration « neutre » normale et de la valeur par défaut « préférer 32 bits » crée des applications de chargement côté 32 bits.

**Instanciation serveur et AppDomains**

Toute application installée hors Windows Store reçoit sa propre instance d’un serveur App Broker (« instanciation multiple »). Le code serveur s’exécute dans un unique AppDomain. Cela permet l’exécution de plusieurs versions de bibliothèques dans des instances séparées. Par exemple, l’application A a besoin de V1.1 d’un composant et l’application B a besoin de V2. Ces deux versions sont clairement séparées, les composants V1.1 et V2 se trouvent dans des répertoires serveur distincts et l’application pointe vers le serveur qui prend en charge la version correcte appropriée.

L’implémentation du code serveur peut être partagée entre plusieurs instances de serveur App Broker en faisant pointer plusieurs applications vers le même répertoire serveur. Plusieurs instances du serveur App Broker existent, mais elles exécutent le même code. Tous les composants d’implémentation utilisés dans une application doivent se trouver dans le même chemin.

## <a name="defining-the-contract"></a>Définition du contrat

La première étape pour créer une application en utilisant cette fonctionnalité consiste à créer le contrat entre l’application installée hors Windows Store et le composant de bureau. Pour cela, vous devez utiliser exclusivement des types Windows Runtime.
Heureusement, ceux-ci sont faciles à déclarer à l’aide de \# classes C. Cependant, quand vous définissez ces conversations, des facteurs de performances importants sont à prendre en compte. Ce point est abordé plus en détail plus loin dans cet article.

La séquence pour définir le contrat se présente comme suit :

**Étape 1 :** créez une nouvelle classe dans Visual Studio. Veillez à créer le projet à l’aide du modèle **bibliothèque de classes** , et non pas du modèle de **composant Windows Runtime** .

Une implémentation doit suivre, mais cette section ne traite que de la définition du contrat entre processus. L’exemple fourni comprend la classe suivante (EnterpriseServer.cs), qui ressemble à :

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

Ce code définit une classe « EnterpriseServer » pouvant être instanciée à partir de l’application installée hors Windows Store. Cette classe fournit la fonctionnalité promise par la classe Runtime. La classe Runtime peut servir à générer le fichier winmd de référence qui sera inclus dans l’application installée hors Windows Store.

**Étape 2 :** Modifiez le fichier projet manuellement pour modifier le type de sortie du projet pour **Windows Runtime composant**.

Pour ce faire, dans Visual Studio, cliquez avec le bouton droit de la souris sur le projet nouvellement créé et sélectionnez « Décharger le projet ». Cliquez de nouveau avec le bouton droit de la souris, puis sélectionnez « Modifier EnterpriseServer.csproj » pour ouvrir le fichier de projet, un fichier XML, pour l’éditer.

Dans le fichier ouvert, recherchez la balise \<OutputType\> et changez sa valeur en « winmdobj ».

**Étape 3 :** créez une règle de génération qui crée un fichier de métadonnées Windows de « référence » (fichier .winmd), c'est-à-dire sans implémentation.

**Étape 4 :** créez une règle de génération qui crée un fichier de métadonnées Windows « d’implémentation » (contient les mêmes informations de métadonnées, mais comprend également l’implémentation).

Cette opération s’effectue via les scripts suivants. Ajoutez les scripts à la ligne de commande de l’événement après génération, dans **Propriétés** du projet  >  **événements de build**.

> **Remarque** Le script est différent en fonction de la version de Windows que vous visez (Windows 10) et de la version de Visual Studio utilisée.

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

Une fois le fichier de référence **winmd** créé (dans le sous-dossier « reference » du dossier Target du projet), il est copié sur chaque projet d’application installée hors Windows Store qui le consomme et il est référencé. La section suivante décrira cette procédure. La structure du projet intégrée dans les règles de génération ci-dessus garantit que l’implémentation et la référence **winmd** se trouvent dans des répertoires séparés dans la hiérarchie de génération pour éviter toute confusion.

## <a name="side-loaded-applications-in-detail"></a>Détails sur les applications installées hors Windows Store
Comme indiqué précédemment, l’application installée hors Windows Store est créée comme n’importe quelle application UWP, à un détail près : la déclaration de la disponibilité de la ou des classes Runtime dans le manifeste de l’application installée hors Windows Store. Cela permet à l’application d’écrire un nouvel accès à la fonctionnalité dans le composant de bureau. Une nouvelle entrée de manifeste dans la section <Extension> décrit la classe Runtime implémentée dans le composant de bureau et les informations sur son emplacement. Le contenu de cette déclaration dans le manifeste de l’application est le même pour les applications qui ciblent Windows 10. Par exemple :

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

La catégorie est inProcessServer, car il existe plusieurs entrées dans la catégorie outOfProcessServer qui ne sont pas applicables à cette configuration d’application. Le composant <Path> doit toujours contenir clrhost.dll (cependant cela n’est **pas** mis en œuvre et l’indication d’une autre valeur provoquera un échec).

La section <ActivatableClass> est identique à une classe Runtime véritablement in-process préférée par un composant Windows Runtime dans le package d’application. <ActivatableClassAttribute> est un nouvel élément, et les attributs Name = "DesktopApplicationPath" et type = "String" sont obligatoires et invariants. L’attribut Value pointe vers l’emplacement où réside le fichier winmd d’implémentation du composant de bureau (décrit en détail dans la section suivante). Chaque classe Runtime préférée par le composant de bureau doit avoir son arborescence d’éléments <ActivatableClass>. ActivatableClassId doit correspondre au nom complet d’espace de noms de la classe Runtime.

Comme indiqué dans la section « Définition du contrat », une référence de projet au winmd de référence du composant de bureau doit être créée. Le système de projet Visual Studio crée une structure de répertoires à deux niveaux portant le même nom. Dans l’exemple, il s’agit de EnterpriseIPCApplication \\ EnterpriseIPCApplication. La référence **winmd** est manuellement copiée dans le répertoire de deuxième niveau, puis la boîte de dialogue Références du projet est utilisée (cliquez sur le bouton **Parcourir..** ) pour rechercher et référencer ce **winmd**. Après cela, l’espace de noms de niveau supérieur du composant Desktop (par exemple, Fabrikam) doit apparaître en tant que nœud de niveau supérieur dans la partie références du projet.

>**Remarque** Vous devez absolument utiliser la **reference winmd** dans l’application installée hors Windows Store. Si vous placez accidentellement l’ **implementation winmd** dans le répertoire de l’application installée hors Windows Store et que vous y faites référence, vous recevrez probablement une erreur de type « IStringable introuvable ». Cela indique à coup sûr qu’un **winmd** incorrect a été référencé. Les règles post-build dans l’application serveur IPC (décrites dans la section suivante) placent ces deux **winmd** dans des répertoires distincts.

Variables d’environnement (en particulier% ProgramFiles%) peut être utilisé dans <ActivatableClassAttribute Value="path"> . Comme indiqué précédemment, l’app Broker ne prend en charge que 32 bits, de sorte que% ProgramFiles% sera résolu en C : \\ Program Files (x86) si l’application est exécutée sur un système d’exploitation 64 bits.

## <a name="desktop-ipc-server-detail"></a>Détails du serveur IPC de bureau

Les deux précédentes sections ont décrit la déclaration de la classe et les mécanismes de transport de la référence **winmd** dans le projet d’application installée hors Windows Store. Le travail restant dans le composant de bureau concerne l’implémentation. Comme nous voulons que le composant de bureau puisse appeler le code de bureau (en réutilisant des composants de code existants), le projet doit être configuré d’une certaine façon.
En règle générale, un projet Visual Studio en .NET utilise l’un des deux « profils » existants.
L’un est pour le bureau (« .NetFramework ») et l’autre cible la partie de l’application UWP du CLR (« .NetCore »). Un composant de bureau dans cette fonctionnalité est hybride. Il en résulte que la section Références est construite précisément pour fusionner ces deux profils.

Un projet d’application UWP standard ne contient pas de références de projet explicites, car l’intégralité de la surface d’API Windows Runtime est incluse de façon implicite.
En règle générale, seules des références entre projets sont créées. Cependant, un projet de composant de bureau contient un ensemble de références spécial. Il démarre la vie comme un projet « \\ bibliothèque de classes de bureau classique » et est donc un projet de bureau. Des références explicites à l’API Windows Runtime (via des références aux fichiers **winmd** ) doivent donc être créées. Ajoutez les références adéquates tel qu’indiqué ci-dessous.

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

Ces références sont un mélange précis de références qui sont nécessaires au fonctionnement correct de ce serveur hybride. Le protocole consiste à ouvrir le fichier .csproj (tel que décrit dans « Comment modifier le type de sortie du projet ») et à ajouter ces références si nécessaire.

Une fois ces références configurées correctement, la tâche suivante consiste à implémenter la fonctionnalité du serveur. Consultez la rubrique [meilleures pratiques pour l’interopérabilité avec les composants de Windows Runtime (applications UWP utilisant C \# /VB/C + + et XAML)](/previous-versions/windows/apps/hh750311(v=win.10)).
Cette tâche consiste à créer une DLL de composant Windows Runtime qui est en mesure d’appeler le code de bureau dans le cadre de son implémentation. L’exemple fourni comprend les principaux modèles utilisés dans Windows Runtime :

-   Appels de méthode

-   Sources des événements Windows Runtime par le composant de bureau

-   Opérations asynchrones Windows Runtime

-   Renvoi de tableaux de types de base

**Installer**

Pour installer l’application, copiez le fichier **winmd** d’implémentation dans le répertoire correct indiqué dans le manifeste de l’application installée hors Windows Store associé : Value="path" de <ActivatableClassAttribute>. Copiez également les fichiers de support associés et les DLL proxy/stub (décrites plus bas). Si vous ne copiez pas le fichier **winmd** d’implémentation à l’emplacement du répertoire serveur, tous les appels de l’application installée hors Windows Store à la nouvelle classe Runtime provoqueront une erreur « classe non inscrite ». Si vous n’installez pas le proxy/stub (ou ne l’inscrivez pas) tous les appels échoueront sans valeur de retour. Cette dernière erreur n’est souvent **pas** associée à des exceptions visibles.
Si des exceptions visibles sont provoquées par cette erreur de configuration, elles font référence à un « cast incorrect ».

**Considérations en matière d’implémentation serveur**

Le serveur Windows Runtime de bureau peut être considéré comme étant basé sur un « travail » ou une « tâche ». Chaque appel au serveur se produit sur un thread sans interface utilisateur et tout le code doit être multithread et safe. La partie de l’application installée hors Windows Store qui appelle la fonctionnalité du serveur est très importante. Il ne faut pas appeler du code dont l’exécution dure trop longtemps à partir d’un thread d’interface utilisateur dans l’application installée hors Windows Store. Il existe deux façons de procéder :

1.  Quand vous appelez la fonctionnalité serveur à partir d’un thread d’interface utilisateur, utilisez toujours un modèle asynchrone dans la zone de surface publique et dans l’implémentation du serveur.

2.  Appelez la fonctionnalité du serveur à partir d’un thread en arrière-plan dans l’application installée hors Windows Store.

**Windows Runtime asynchrone sur le serveur**

Étant donné la nature entre processus du modèle d’application, les appels au serveur durent plus longtemps que le code qui s’exécute uniquement in-process. Il est généralement sûr d’appeler une propriété qui retourne une valeur en mémoire, car elle s’exécutera assez rapidement et le thread de l’interface utilisateur ne sera pas bloqué longtemps. Cependant, les appels qui entraînent des E/S (y compris la gestion de fichiers et les récupérations de base de données) peuvent potentiellement bloquer le thread d’interface utilisateur appelant et provoquer l’arrêt de l’application pour non réponse. De plus, les appels de propriétés sur des objets ne sont pas conseillés dans cette architecture d’application pour des raisons de performances.
La section suivante décrit cela en détail.

Un serveur correctement implémenté implémente directement les appels faits aux threads de l’interface utilisateur via le modèle Windows Runtime asynchrone. Cela peut être implémenté en suivant ce modèle. Tout d’abord, la déclaration (là encore, à partir de l’exemple fourni) :

```csharp
public IAsyncOperation<int> FindElementAsync(int input)
```

Une opération Windows Runtime asynchrone qui retourne un entier est déclarée.
L’implémentation de l’opération asynchrone prend normalement la forme suivante :

```csharp
return Task<int>.Run( () =>
{
    int retval = ...
    // execute some potentially long-running code here 
}).AsAsyncOperation<int>();

```

>**Remarque** Il est possible d’attendre des opérations qui peuvent durer longtemps pendant l’écriture de l’implémentation. Si tel est le cas, le code **Task.Run** doit être déclaré :

```csharp
return Task<int>.Run(async () =>
{
    int retval = ...
    // execute some potentially long-running code here 
    await ... // some other WinRT async operation or Task
}).AsAsyncOperation<int>();
```

Les clients de cette méthode asynchrone peuvent attendre cette opération tout comme n’importe quelle opération Windows Runtime asynchrone.

**Fonctionnalité serveur d’appels à partir d’un thread d’application en arrière-plan**

Comme le client et le serveur sont généralement écrits par la même organisation, une pratique de programmation peut décider que tous les appels au serveur seront effectués via un thread en arrière-plan dans l’application installée hors Windows Store. Un appel direct qui recueille un ou plusieurs lots de données du serveur peut être effectué à partir d’un thread d’arrière-plan. Quand tous les résultats sont récupérés, les lots de données en mémoire dans le processus d’application peuvent être récupérés directement du thread d’interface utilisateur. Les \# objets C sont naturellement agiles entre les threads d’arrière-plan et les threads d’interface utilisateur, ce qui est particulièrement utile pour ce type de modèle d’appel.

## <a name="creating-and-deploying-the-windows-runtime-proxy"></a>Création et déploiement du proxy Windows Runtime

Dans la mesure où l’approche IPC comprend le marshaling des interfaces Windows Runtime entre deux processus, un proxy et un stub Windows Runtime inscrits globalement doivent être utilisés.

**Création du proxy dans Visual Studio**

Le processus de création et d’inscription de proxies et de stubs à utiliser dans un package d’application UWP standard est décrit dans la rubrique [déclenchement d’événements dans des composants de Windows Runtime](/previous-versions/windows/apps/dn169426(v=vs.140)).
Les étapes décrites dans cet article sont plus compliquées que la procédure décrite ci-dessous, car elles décrivent l’inscription du proxy/stub dans le package de l’application (plutôt qu’une inscription globale).

**Étape 1 :** en utilisant la solution du projet de composant de bureau, créez un projet Proxy/Stub dans Visual Studio :

**Solution > ajouter > projet > Visual C++ > option Select DLL de la console Win32.**

Suivez les étapes ci-dessous, le composant serveur s’appelle **MyWinRTComponent**.

**Étape 3 :** supprimez tous les fichiers CPP/H du projet.

**Étape 4 :** la section précédente intitulée « Définition du contrat » contient une commande post-build qui exécute **winmdidl.exe** , **midl.exe** , **mdmerge.exe** , et ainsi de suite. L’une des sorties de l’étape midl de cette commande post-build génère quatre sorties importantes :

a) Dlldata.c

b) un fichier d’en-tête (par exemple, MyWinRTComponent. h)

c) un \* \_ fichier i. c (par exemple, MyWinRTComponent \_ i. c)

d) un \* \_ fichier p. c (par exemple, MyWinRTComponent \_ p. c)

**Étape 5 :** ajoutez ces quatre fichiers générés au projet « MyWinRTProxy ».

**Étape 6 :** ajoutez un fichier de définition au projet « MyWinRTProxy » **(Projet &gt; Ajouter un nouvel élément &gt; Code &gt; Fichier de définition de module** ) et mettez à jour le contenu ainsi :

LIBRARY MyWinRTComponent.Proxies.dll

EXPORTS

DllCanUnloadNow PRIVATE

DllGetClassObject PRIVATE

DllRegisterServer PRIVATE

DllUnregisterServer PRIVATE

**Étape 7 :** ouvrez les propriétés pour le projet « MyWinRTProxy » :

**Propriétés Comfiguration > général > nom cible :**

MyWinRTComponent.Proxies

**Définitions de préprocesseur > C/C++ > ajouter**

«WIN32 ; \_ Windows INSCRIRE \_ la \_ dll du proxy»

**C/C++ > en-tête précompilé : sélectionnez « pas d’utilisation de l’en-tête précompilé »**

**Éditeur de liens > général > ignorer la bibliothèque d’importation : sélectionnez « Oui »**

**> d’entrée de l’éditeur de liens > des dépendances supplémentaires : Ajouter rpcrt4. lib ; runtimeobject. lib**

**Éditeur de liens > les métadonnées Windows > générer les métadonnées Windows : sélectionnez « non »**

**Étape 8 :** générez le projet « MyWinRTProxy ».

**Déploiement du proxy**

Le proxy doit être inscrit globalement. Pour ce faire, le processus d’installation doit appeler DllRegisterServer dans la DLL proxy. Dans la mesure où la fonctionnalité ne prend en charge que des serveurs x86 (pas de prise en charge 64 bits), la configuration la plus simple utilise un serveur 32 bits, un proxy 32 bits et une application installée hors Windows Store 32 bits. Le proxy se trouve généralement avec l’implémentation **winmd** pour le composant de bureau.

Une étape de configuration supplémentaire est nécessaire. Pour que le processus d’installation hors Windows Store charge et exécute le proxy, le répertoire doit être marqué « lecture / exécution » pour ALL_APPLICATION_PACKAGES. Utilisez pour cela l’outil en ligne de commande **icacls.exe**. Cette commande doit être exécutée sur le répertoire dans lequel se trouve l’implémentation **winmd** et les DLL proxy/stub :

*icacls. /T/Grant \* S-1-15-2-1 : RX*

## <a name="patterns-and-performance"></a>Modèles et performances

Il est très important que les performances du transport interprocessus soient étroitement surveillées. Un appel interprocessus est deux fois plus coûteux qu’un appel in-process. Si vous créez des conversations « bavardes » entre les processus ou effectuez des transferts répétés d’objets de grande taille, comme des images bitmap, les performances de l’application peuvent s’en ressentir.

Voici une liste d’éléments à prendre en compte :

-   Il n’est pas conseillé d’utiliser des appels de méthode synchrones à partir du thread d’interface utilisateur de l’application. Appelez la méthode à partir d’un thread d’arrière-plan dans l’application, puis utilisez CoreWindowDispatcher pour obtenir des résultats dans le thread d’interface utilisateur si nécessaire.

-   L’appel d’opérations async à partir du thread d’interface utilisateur est safe, mais pensez aux problèmes de performances présentées dans ce document.

-   Le transfert en bloc des résultats limite le trafic interprocessus. La construction Windows Runtime Array est utilisée pour cela.

-   Si *List<T>* , où *T* est un objet, est retourné d’une opération async ou d’une extraction de propriété, de nombreux échanges interprocessus seront nécessaires. Par exemple, si vous retournez un objet *List&gt;People&lt;*. Chaque itération correspondra à un appel interprocessus. Chaque objet *People* retourné est représenté par un proxy et chaque appel à une méthode ou propriété sur cet objet individuel donnera lieu à un appel interprocessus. Un « simple » objet *List&lt;People&gt;* avec une valeur *Count* élevée produira un grand nombre d’appels lents. Le transfert en bloc de structures de contenu dans un tableau donne de meilleures performances. Par exemple :

```csharp
struct PersonStruct
{
    String LastName;
    String FirstName;
    int Age;
   // etc.
}
```

Retournez ensuite *PersonStruct \[ \]* au lieu *de &lt; List &gt; PersonObject*.
Toutes les données sont transmises en un saut (hop) interprocessus

Comme pour tous les éléments à prendre en compte pour les performances, les mesures et le test sont critiques. La télémétrie doit être utilisée dans différentes opérations pour connaître la durée de chacune d’elles. Il est important de réaliser ces mesures sur une plage : par exemple, quel est le temps nécessaire pour consommer tous les objets *People* pour une requête spécifique dans l’application installée hors Windows Store ?

Le test de charge variable peut également être utilisé. Pour cela, ajoutez des crochets de test de performance dans l’application qui introduisent des chargements différés variables pendant le traitement du serveur. Cela simule différents types de charge et la réaction de l’application à différentes performances du serveur.
L’exemple montre comment introduire des délais dans le code en utilisant des techniques async appropriées. La durée exacte du délai et la plage de randomisation à ajouter à la charge artificielle dépendent de la conception de l’application et de l’environnement dans lequel l’application s’exécute.

## <a name="development-process"></a>Processus de développement

Lorsque vous souhaitez apporter des modifications au serveur, assurez-vous au préalable qu’aucune instance démarrée précédemment n’est encore en cours d’exécution. Même si le code COM nettoie le processus, le minuteur d’arrêt prend plus de temps et nuit à l’efficacité du développement itératif. La suppression d’instances précédentes en cours d’exécution est donc une étape normale du développement. Cela nécessite que le développeur sache quelle instance dllhost héberge le serveur.

Il est possible de trouver et d’arrêter le processus serveur via le Gestionnaire des tâches ou d’une autre application tierce. L’outil de ligne de commande **TaskList.exe** est également inclus et a une syntaxe flexible, par exemple :

  
 | **Commande** | **Action** |
 | ------------| ---------- |
 | tasklist | Affiche une liste de tous les processus en cours d’exécution dans l’ordre approximatif de leur date de création. Les derniers processus créés sont affichés vers le bas de la liste. |
 | tasklist /FI "IMAGENAME eq dllhost.exe" /M | Affiche des informations sur toutes les instances dllhost.exe. Le commutateur /M répertorie les modules ayant été chargés. |
 | tasklist /FI "PID eq 12564" /M | Cette option vous permet de rechercher une instance dllhost.exe en fonction de son PID (si vous le connaissez). |

Pour un serveur du service Broker, *clrhost.dll* doit figurer dans la liste des modules chargés.

## <a name="resources"></a>Ressources

-   [Modèles de projets pour les composants WinRT du service Broker pour Windows 10 et VS 2015](https://marketplace.visualstudio.com/items?itemName=vs-publisher-713547.VS2015TemplateBrokeredComponents)

-   [Diffusion d’applications de Microsoft Store fiables et fiables](https://blogs.msdn.com/b/b8/archive/2012/05/17/delivering-reliable-and-trustworthy-metro-style-apps.aspx)

-   [Contrats et extensions des applications (applications du Windows Store)](/previous-versions/windows/apps/hh464906(v=win.10))

-   [Comment installer des applications hors Windows Store sur Windows 10](/windows/apps/get-started/enable-your-device-for-development)

-   [Déploiement d’applications UWP dans des entreprises](https://blogs.msdn.com/b/windowsstore/archive/2012/04/25/deploying-metro-style-apps-to-businesses.aspx)