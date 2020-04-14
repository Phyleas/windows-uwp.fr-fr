---
Description: Découvrez comment créer une application hébergée qui hérite de l’exécutable, du point d’entrée et des attributs d’exécution d’une application hôte.
title: Créer des applications hébergées
ms.date: 01/28/2020
ms.topic: article
keywords: windows 10, bureau, package, identité, MSIX, Win32
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: a3017073b15ea18214e9c78263fb212bb192132b
ms.sourcegitcommit: 8b7b677c7da24d4f39e14465beec9c4a3779927d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81266927"
---
# <a name="create-hosted-apps"></a>Créer des applications hébergées

À compter de Windows 10, version 2004, vous pouvez créer des *applications hébergées*. Une application hébergée partage le même exécutable et la même définition qu’une application *hôte* parent, mais elle se présente comme une application distincte sur le système.

Les applications hébergées sont utiles dans les scénarios où vous souhaitez qu’un composant (tel qu’un fichier exécutable ou un fichier de script) se comporte comme une application Windows 10 autonome, mais le composant requiert un processus hôte pour pouvoir s’exécuter. Par exemple, un script PowerShell ou python peut être fourni sous la forme d’une application hébergée nécessitant l’installation d’un hôte pour pouvoir s’exécuter. Une application hébergée peut avoir sa propre vignette de départ, son identité et son intégration profonde avec les fonctionnalités Windows 10, telles que les tâches en arrière-plan, les notifications, les vignettes et les cibles de partage.

La fonctionnalité applications hébergées est prise en charge par plusieurs éléments et attributs du manifeste de package qui permettent à une application hébergée d’utiliser un exécutable et une définition dans un package d’application hôte. Quand un utilisateur exécute l’application hébergée, le système d’exploitation lance automatiquement l’exécutable hôte sous l’identité de l’application hébergée. L’hôte peut ensuite charger les ressources visuelles, le contenu ou les API d’appel comme application hébergée. L’application hébergée obtient l’intersection des fonctionnalités déclarées entre l’hôte et l’application hébergée. Cela signifie qu’une application hébergée ne peut pas demander plus de fonctionnalités que celles fournies par l’hôte.

## <a name="define-a-host"></a>Définir un hôte

L' *hôte* est le principal processus exécutable ou d’exécution pour l’application hébergée. Actuellement, les seuls hôtes pris en charge sont les applications de C++Bureau (.net ou/Win32) qui ont une *identité de package*. Pour le moment, les applications UWP ne sont pas prises en charge en tant qu’hôtes. Il existe plusieurs façons pour une application de bureau d’avoir une identité de package :

* La méthode la plus courante pour accorder l’identité d’un package à une application de bureau consiste à l' [empaqueter dans un package MSIX](https://docs.microsoft.com/windows/msix).
* Dans certains cas, vous pouvez également choisir d’accorder l’identité du package en créant un [package fragmenté](https://docs.microsoft.com/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps). Cette option est utile si vous ne pouvez pas adopter l’empaquetage MSIX pour le déploiement de votre application de bureau.

L’hôte est déclaré dans son manifeste de package par l’extension **uap10 : HostRuntime** . Cette extension a un attribut d' **ID** auquel une valeur qui est également référencée par le manifeste de package pour l’application hébergée doit être affectée. Lorsque l’application hébergée est activée, l’ordinateur hôte est lancé sous l’identité de l’application hébergée et peut charger du contenu ou des fichiers binaires à partir du package d’application hébergé.

L’exemple suivant montre comment définir un hôte dans un manifeste de package. L’extension **uap10 : HostRuntime** est à l’ensemble du package et est donc déclarée en tant qu’enfant de l’élément de [package](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-package) .

``` xml
<Package xmlns:uap10="http://schemas.microsoft.com/appx/manifest/uap/windows10/10">

  <Extensions>
    <uap10:Extension Category="windows.hostRuntime"  
        Executable="PyScriptEngine\PyScriptEngine.exe"  
        uap10:RuntimeBehavior="packagedClassicApp"  
        uap10:TrustLevel="mediumIL">
      <uap10:HostRuntime Id="PythonHost" />
    </uap10:Extension>
  </Extensions>

</Package>
```

Prenez note de ces informations importantes sur les éléments suivants.

| Élément              | Détails |
|----------------------|-------|
| **uap10 : extension** | La catégorie `windows.hostRuntime` déclare une extension au niveau du package qui définit les informations d’exécution à utiliser lors de l’activation d’une application hébergée. Une application hébergée s’exécute avec les définitions déclarées dans l’extension. Quand vous utilisez l’application hôte déclarée dans l’exemple précédent, une application hébergée s’exécute en tant que fichier exécutable **PyScriptEngine. exe** au niveau de confiance **mediumIL** .<br/><br/>Les attributs **executable**, **uap10 : RuntimeBehavior**et **uap10 : TrustLevel** spécifient le nom du fichier binaire du processus hôte dans le package et comment les applications hébergées s’exécutent. Par exemple, une application hébergée utilisant les attributs dans l’exemple précédent s’exécutera en tant que fichier exécutable PyScriptEngine. exe au niveau de confiance mediumIL. |
| **uap10:HostRuntime** | L’attribut **ID** déclare l’identificateur unique de cette application hôte spécifique dans le package. Un package peut avoir plusieurs applications hôtes, et chacune doit avoir un élément **uap10 : HostRuntime** avec un **ID**unique.

## <a name="declare-a-hosted-app"></a>Déclarer une application hébergée

Une *application hébergée* déclare une dépendance de package sur un *hôte*. L’application hébergée utilise l’ID de l’hôte (autrement dit, l’attribut **ID** de l’extension **uap10 : HostRuntime** dans le package hôte) pour l’activation au lieu de spécifier un exécutable de point d’entrée dans son propre package. L’application hébergée contient généralement du contenu, des ressources visuelles, des scripts ou des fichiers binaires qui sont accessibles par l’hôte. La valeur [TargetDeviceFamily](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) dans le package d’application hébergé doit cibler la même valeur que l’hôte.

Les packages d’application hébergée peuvent être signés ou non signés :

* Les packages signés peuvent contenir des fichiers exécutables. Cela est utile dans les scénarios qui ont un mécanisme d’extension binaire, qui permet à l’hôte de charger une DLL ou un composant inscrit dans le package d’application hébergé.
* Les packages non signés ne peuvent contenir que des fichiers non exécutables. Cela est utile dans les scénarios où l’hôte a uniquement besoin de charger des images, des ressources et des fichiers de contenu ou de script. Les packages non signés doivent inclure une valeur de `OID` spéciale dans leur élément [Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) ou ils ne sont pas autorisés à s’inscrire. Cela empêche les packages non signés d’entrer en conflit ou d’usurper l’identité d’un package signé.

Pour définir une application hébergée, déclarez les éléments suivants dans le manifeste du package :

* Élément **uap10 : HostRuntimeDependency** . Il s’agit d’un enfant de l’élément [Dependencies](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-dependencies) .
* L’attribut **uap10 : HostID** de l’élément [application](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) (pour une application) ou un élément [extension](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) (pour une extension activable).

L’exemple suivant illustre les sections correspondantes d’un manifeste de package pour une application hébergée non signée.

``` xml
<Package xmlns:uap10="http://schemas.microsoft.com/appx/manifest/uap/windows10/10">

  <Identity Name="NumberGuesserManifest"
    Publisher="CN=AppModelSamples, OID.2.25.311729368913984317654407730594956997722=1"
    Version="1.0.0.0" />

  <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.19041.0" MaxVersionTested="10.0.19041.0" />
    <uap10:HostRuntimeDependency Name="PyScriptEnginePackage" Publisher="CN=AppModelSamples" MinVersion="1.0.0.0"/>
  </Dependencies>

  <Applications>
    <Application Id="NumberGuesserApp"  
      uap10:HostId="PythonHost"  
      uap10:Parameters="-Script &quot;NumberGuesser.py&quot;">
    </Application>
  </Applications>

</Package>
```

Prenez note de ces informations importantes sur les éléments suivants.

| Élément              | Détails |
|----------------------|-------|
| [**Personnelles**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) | Étant donné que le package d’application hébergé dans cet exemple n’est pas signé, l’attribut **Publisher** doit inclure la chaîne `OID.2.25.311729368913984317654407730594956997722=1`. Cela permet de s’assurer que le package non signé ne peut pas usurper l’identité d’un package signé. |
| [**TargetDeviceFamily**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) | L’attribut **MinVersion** doit spécifier 10.0.19041.0 ou une version ultérieure du système d’exploitation. |
| **uap10:HostRuntimeDependency** | Cet élément d’élément déclare une dépendance sur le package d’application hôte. Cela se compose du **nom** et de l' **éditeur** du package hôte, ainsi que de la **MinVersion** dont il dépend. Ces valeurs se trouvent sous l’élément [Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) dans le package hôte. |
| **Application** | L’attribut **uap10 : HostID** exprime la dépendance sur l’hôte. Le package d’application hébergé doit déclarer cet attribut à la place des attributs d' **exécutable** et de **point d’entrée** habituels pour une [application](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) ou un élément d' [extension](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) . Par conséquent, l’application hébergée hérite des attributs **executable**, **entryPoint** et Runtime de l’hôte avec la valeur **HostID** correspondante.<br/><br/>L’attribut **uap10 : Parameters** spécifie les paramètres qui sont passés à la fonction de point d’entrée de l’exécutable hôte. Étant donné que l’hôte doit savoir quoi faire avec ces paramètres, il existe un contrat implicite entre l’hôte et l’application hébergée. |

## <a name="register-an-unsigned-hosted-app-package-at-run-time"></a>Inscrire un package d’application hébergé non signé au moment de l’exécution

L’un des avantages de l’extension **uap10 : HostRuntime** est qu’elle permet à un hôte de générer de façon dynamique un package d’application hébergé au moment de l’exécution et de l’inscrire à l’aide de l’API [packagemanager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager) , sans avoir à le signer. Cela permet à un hôte de générer de façon dynamique le contenu et le manifeste du package d’application hébergé, puis de l’inscrire.

Utilisez les méthodes suivantes de la classe [packagemanager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager) pour inscrire un package d’application hébergé non signé. Ces méthodes sont disponibles à partir de Windows 10, version 2004.

* **AddPackageByUriAsync**: enregistre un package MSIX non signé à l’aide de la propriété **AllowUnsigned** du paramètre *options* .
* **RegisterPackageByUriAsync**: effectue une inscription de fichier manifeste de package libre. Si le package est signé, le dossier contenant le manifeste doit inclure un [fichier. p7x](https://docs.microsoft.com/windows/msix/overview#inside-an-msix-package) et un catalogue. Si la valeur n’est pas signée, la propriété **AllowUnsigned** du paramètre *options* doit être définie.

### <a name="requirements-for-unsigned-hosted-apps"></a>Configuration requise pour les applications hébergées non signées

* Les éléments d' [application](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) ou d' [extension](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) du manifeste du package ne peuvent pas contenir de données d’activation telles que les attributs **executable**, **entryPoint**ou **trustLevel** . Au lieu de cela, ces éléments peuvent uniquement contenir un attribut **uap10 : HostID** qui exprime la dépendance sur l’hôte et un attribut **Uap10 : Parameters** .
* Le package doit être un package principal. Il ne peut pas s’agir d’un bundle, d’un package d’infrastructure, d’une ressource ou d’un package facultatif.

### <a name="requirements-for-a-host-that-installs-and-registers-an-unsigned-hosted-app-package"></a>Configuration requise pour un ordinateur hôte qui installe et inscrit un package d’application hébergé non signé

* L’hôte doit avoir une [identité de package](#define-a-host).
* L’ordinateur hôte doit disposer de la [fonctionnalité de restriction](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations#restricted-capabilities) **packageManagement** .
    ```xml
    <rescap:Capability Name="packageManagement" />
    ```

<!--
* If the host runs in app container (for example, it is a UWP app), it must also have the unsigned package management [custom capability](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations#custom-capabilities) and a [Signed Custom Capability Descriptor (SCCD) file](https://docs.microsoft.com/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#preparing-the-signed-custom-capability-descriptor-sccd-file).
    ```xml
    <uap4:CustomCapability Name="Microsoft.unsignedPackageManagement_cw5n1h2txyewy" />
    ```
-->

## <a name="sample"></a>Exemple

Pour obtenir un exemple d’application entièrement fonctionnel qui déclare lui-même en tant qu’hôte, puis inscrit dynamiquement un package d’application hébergé au moment de l’exécution, consultez l' [exemple d’application hébergée](https://aka.ms/hostedappsample).

### <a name="the-host"></a>L’hôte

L’hôte est nommé **PyScriptEngine**. Il s’agit d’un wrapper C# écrit dans qui exécute des scripts Python. Lorsqu’il est exécuté avec le paramètre `-Register`, le moteur de script installe une application hébergée contenant un script Python. Lorsqu’un utilisateur tente de lancer l’application hébergée récemment installée, l’ordinateur hôte est lancé et exécute le script Python **NumberGuesser** .

Le manifeste du package pour l’application hôte (le fichier Package. appxmanifest dans le dossier PyScriptEnginePackage) contient une extension **uap10 : HostRuntime** qui déclare l’application en tant qu’hôte avec l’ID **PythonHost** et l’exécutable **PyScriptEngine. exe**.  

> [!NOTE]
> Dans cet exemple, le manifeste du package est nommé Package. appxmanifest et il fait partie d’un [projet de packaging des applications Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net). Lorsque ce projet est généré, il [génère un manifeste nommé AppxManifest. xml](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/generate-package-manifest) et génère le package MSIX pour l’application hôte.

### <a name="the-hosted-app"></a>L’application hébergée

L’application hébergée se compose d’un script Python et d’artefacts de package tels que le manifeste du package. Il ne contient aucun fichier PE.

Le manifeste du package pour l’application hébergée (le fichier NumberGuesser/AppxManifest. Xml) contient les éléments suivants :

* L’attribut **Publisher** de l’élément [Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) contient l’identificateur `OID.2.25.311729368913984317654407730594956997722=1`, qui est requis pour un package non signé.
* L’attribut **uap10 : HostID** de l’élément [application](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) identifie **PythonHost** en tant qu’hôte.

### <a name="run-the-sample"></a>Exécuter l'exemple

L’exemple requiert la version 10.0.19041.0 ou ultérieure de Windows 10 et le SDK Windows.

1. Téléchargez l' [exemple](https://aka.ms/hostedappsample) dans un dossier sur votre ordinateur de développement.
2. Ouvrez la solution PyScriptEngine. sln dans Visual Studio et définissez le projet **PyScriptEnginePackage** comme projet de démarrage.
3. Générez le projet **PyScriptEnginePackage** .
4. Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet **PyScriptEnginePackage** et choisissez **déployer**.
5. Ouvrez une fenêtre d’invite de commandes dans le répertoire où vous avez copié les exemples de fichiers et exécutez la commande suivante pour inscrire l’exemple d’application **NumberGuesser** (l’application hébergée). Remplacez `D:\repos\HostedApps` par le chemin d’accès où vous avez copié les exemples de fichiers.

    ```CMD
    D:\repos\HostedApps>pyscriptengine -Register D:\repos\HostedApps\NumberGuesser\AppxManifest.xml
    ```

    > [!NOTE]
    > Vous pouvez exécuter `pyscriptengine` sur la ligne de commande, car l’hôte de l’exemple déclare un [AppExecutionAlias](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias).

6. Ouvrez le menu **Démarrer** et cliquez sur **NumberGuesser** pour exécuter l’application hébergée.
