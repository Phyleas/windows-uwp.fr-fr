---
Description: Découvrez comment accorder une identité à des applications de bureau non packagées pour pouvoir utiliser les fonctionnalités Windows 10 modernes dans ces applications.
title: Accorder l’identité à des applications de bureau non packagées
ms.date: 10/25/2019
ms.topic: article
keywords: Windows 10, Desktop, package, Identity, MSIX, Win32
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f355bba3087f58ed20800052371804048bc0006c
ms.sourcegitcommit: d7eccdb27c22bccac65bd014e62b6572a6b44602
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73145614"
---
# <a name="grant-identity-to-non-packaged-desktop-apps"></a>Accorder l’identité à des applications de bureau non packagées

<!--
> [!NOTE]
> The features described in this article require Windows 10 Insider Preview Build 10.0.19000.0 or a later release.
-->

De nombreuses fonctionnalités d’extensibilité de Windows 10 requièrent l’utilisation d’une [identité de package](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) à partir d’applications de bureau non UWP, y compris des tâches en arrière-plan, des notifications, des vignettes dynamiques et des cibles de partage. Pour ces scénarios, le système d’exploitation nécessite une identité pour pouvoir identifier l’appelant de l’API correspondante.

Dans les versions de système d’exploitation antérieures à Windows 10 Insider Preview Build 10.0.19000.0, la seule façon d’accorder une identité à une application de bureau consiste à l' [empaqueter dans un package MSIX signé](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root). Pour ces applications, l’identité est spécifiée dans le manifeste du package et l’inscription de l’identité est gérée par le pipeline de déploiement MSIX en fonction des informations contenues dans le manifeste. Tout le contenu référencé dans le manifeste du package est présent dans le package MSIX.

À compter de Windows 10 Insider Preview Build 10.0.19000.0, vous pouvez accorder l’identité du package aux applications de bureau qui ne sont pas empaquetées dans un package MSIX en générant et en inscrivant un *package épars* avec votre application. Cette prise en charge permet aux applications de bureau qui n’ont pas encore pu adopter le Packaging MSIX pour le déploiement d’utiliser les fonctionnalités d’extensibilité de Windows 10 qui requièrent l’identité du package. Pour plus d’informations générales, consultez ce billet de [blog](https://blogs.windows.com/windowsdeveloper/2019/10/29/identity-registration-and-activation-of-non-packaged-win32-apps/#HBMFEM843XORqOWx.97).

Pour créer et inscrire un package fragmenté qui accorde l’identité du package à votre application de bureau, procédez comme suit.

1. [Créer un manifeste de package pour le package épars](#create-a-package-manifest-for-the-sparse-package)
2. [Créer et signer le package épars](#build-and-sign-the-sparse-package)
3. [Ajouter les métadonnées d’identité du package à votre manifeste d’application de bureau](#add-the-package-identity-metadata-to-your-desktop-application-manifest)
4. [Inscrire votre package fragmenté au moment de l’exécution](#register-your-sparse-package-at-run-time)

## <a name="important-concepts"></a>Concepts importants

Les fonctionnalités suivantes permettent aux applications de bureau non packagées d’acquérir l’identité des packages.

### <a name="sparse-packages"></a>Packages épars

Un *package fragmenté* contient un manifeste de package, mais aucun autre fichier binaire d’application et contenu. Le manifeste d’un package fragmenté peut référencer des fichiers en dehors du package dans un emplacement externe prédéterminé. Cela permet aux applications qui ne sont pas encore en mesure d’adopter le Packaging MSIX pour l’ensemble de leur application d’obtenir l’identité du package, comme requis par certaines fonctionnalités d’extensibilité de Windows 10.

> [!NOTE]
> Une application de bureau qui utilise un package fragmenté ne bénéficie pas de certains avantages du déploiement complet via un package MSIX. Ces avantages incluent la protection contre les falsifications, l’installation dans un emplacement verrouillé et la gestion complète par le système d’exploitation au moment du déploiement, de l’exécution et de la désinstallation.

### <a name="package-external-location"></a>Emplacement externe du package

Pour prendre en charge les packages épars, le schéma de manifeste du package prend désormais en charge un élément facultatif **\<AllowExternalContent\>** sous le [ **\<propriétés\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-properties) élément. Cela permet à votre manifeste de package de faire référence à du contenu en dehors du package, à un emplacement spécifique sur le disque.

Par exemple, si vous disposez d’une application de bureau non packagée existante qui installe le fichier exécutable de l’application et un autre contenu dans C:\Program Files\MyDesktopApp\, vous pouvez créer un package épars qui comprend le **\<AllowExternalContent\>** élément dans le manifeste. Pendant le processus d’installation de votre application ou la première fois que vos applications sont installées, vous pouvez installer le package Sparse et déclarer C:\Program Files\MyDesktopApp\ comme emplacement externe utilisé par votre application.

## <a name="create-a-package-manifest-for-the-sparse-package"></a>Créer un manifeste de package pour le package épars

Avant de pouvoir générer un package fragmenté, vous devez d’abord créer un [manifeste de package](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest) (un fichier nommé AppxManifest. Xml) qui déclare les métadonnées d’identité du package pour votre application de bureau et d’autres informations requises. Le moyen le plus simple de créer un manifeste de package pour le package fragmenté consiste à utiliser l’exemple ci-dessous et à le personnaliser pour votre application à l’aide de la [référence de schéma](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root).

Assurez-vous que le manifeste du package comprend les éléments suivants :

* [ **\<d’identité\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) élément qui décrit les attributs d’identité pour votre application de bureau.
* **\<élément AllowExternalContent\>** sous l’élément [ **\<propriétés\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-properties) . Cet élément doit être affecté à la valeur `true`, ce qui permet à votre manifeste de package de référencer du contenu en dehors du package, à un emplacement spécifique sur le disque. Dans une étape ultérieure, vous devez spécifier le chemin d’accès de l’emplacement externe lorsque vous inscrivez votre package fragmenté à partir du code qui s’exécute dans votre programme d’installation ou votre application. Tout contenu que vous référencez dans le manifeste qui ne se trouve pas dans le package lui-même doit être installé à l’emplacement externe.
* L’attribut **MinVersion** de l’élément [ **\<TargetDeviceFamily\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) doit avoir la valeur `10.0.19000.0` ou une version ultérieure.
* Les attributs **trustLevel = mediumIL** et **RuntimeBehavior = Win32App** de l’élément\>de l' [**application\<** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) déclarent que l’application de bureau associée au package Sparse s’exécutera de la même façon qu’un bureau non emballé standard. application, sans la virtualisation du Registre et du système de fichiers, ainsi que d’autres modifications au moment de l’exécution.

L’exemple suivant montre le contenu complet d’un manifeste de package fragmenté (AppxManifest. Xml). Ce manifeste comprend une extension de `windows.sharetarget`, qui requiert l’identité du package.

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package 
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  xmlns:uap10="http://schemas.microsoft.com/appx/manifest/uap/windows10/10"
  IgnorableNamespaces="uap uap2 uap3 rescap desktop uap10">
  <Identity Name="ContosoPhotoStore" ProcessorArchitecture="x64" Publisher="CN=Contoso" Version="1.0.0.0" />
  <Properties>
    <DisplayName>ContosoPhotoStore</DisplayName>
    <PublisherDisplayName>Contoso</PublisherDisplayName>
    <Logo>Assets\storelogo.png</Logo>
    <uap10:AllowExternalContent>true</uap10:AllowExternalContent>
  </Properties>
  <Resources>
    <Resource Language="en-us" />
  </Resources>
  <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.19000.0" MaxVersionTested="10.0.19000.0" />
  </Dependencies>
  <Capabilities>
    <rescap:Capability Name="runFullTrust" />
    <rescap:Capability Name="unvirtualizedResources"/>
  </Capabilities>
  <Applications>
    <Application Id="ContosoPhotoStore" Executable="ContosoPhotoStore.exe" uap10:TrustLevel="mediumIL" uap10:RuntimeBehavior="win32App"> 
      <uap:VisualElements AppListEntry="none" DisplayName="Contoso PhotoStore" Description="Demonstrate photo app" BackgroundColor="transparent" Square150x150Logo="Assets\Square150x150Logo.png" Square44x44Logo="Assets\Square44x44Logo.png">
        <uap:DefaultTile Wide310x150Logo="Assets\Wide310x150Logo.png" Square310x310Logo="Assets\LargeTile.png" Square71x71Logo="Assets\SmallTile.png"></uap:DefaultTile>
        <uap:SplashScreen Image="Assets\SplashScreen.png" />
      </uap:VisualElements>
      <Extensions>
        <uap:Extension Category="windows.shareTarget">
          <uap:ShareTarget Description="Send to ContosoPhotoStore">
            <uap:SupportedFileTypes>
              <uap:FileType>.jpg</uap:FileType>
              <uap:FileType>.png</uap:FileType>
              <uap:FileType>.gif</uap:FileType>
            </uap:SupportedFileTypes>
            <uap:DataFormat>StorageItems</uap:DataFormat>
            <uap:DataFormat>Bitmap</uap:DataFormat>
          </uap:ShareTarget>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

## <a name="build-and-sign-the-sparse-package"></a>Créer et signer le package épars

Après avoir créé votre manifeste de package, générez le package Sparse à l’aide de l' [outil MakeAppx. exe](https://docs.microsoft.com/windows/msix/package/create-app-package-with-makeappx-tool) dans le SDK Windows. Étant donné que le package épars ne contient pas les fichiers référencés dans le manifeste, vous devez spécifier l’option `/nv`, qui ignore la validation sémantique pour le package.

L’exemple suivant montre comment créer un package fragmenté à partir de la ligne de commande.  

```Console
MakeAppx.exe  pack  /d  <path to directory that contains manifest>  /p  <output path>\MyPackage.msix  /nv
```

Avant de pouvoir installer correctement le package Sparse sur un ordinateur cible, vous devez le signer avec un certificat approuvé sur l’ordinateur cible. Vous pouvez créer un nouveau certificat auto-signé à des fins de développement et signer votre package Sparse à l’aide de [SignTool](https://docs.microsoft.com/windows/msix/package/sign-app-package-using-signtool), disponible dans le SDK Windows.

L’exemple suivant montre comment signer un package fragmenté à partir de la ligne de commande.

```Console
SignTool.exe sign /fd SHA256 /a /f <path to certificate>\MyCertificate.pfx  /p <certificate password>  <path to sparse package>\MyPackage.msix
```

### <a name="add-the-package-identity-metadata-to-your-desktop-application-manifest"></a>Ajouter les métadonnées d’identité du package à votre manifeste d’application de bureau

Vous devez également inclure un [manifeste d’application côte à côte](https://docs.microsoft.com/windows/win32/sbscs/application-manifests) avec votre application de bureau et inclure un élément **\<msix\>** avec des attributs qui déclarent les attributs d’identité de votre application. Les valeurs de ces attributs sont utilisées par le système d’exploitation pour déterminer l’identité de votre application lors du lancement de l’exécutable.

L’exemple suivant montre un manifeste d’application côte à côte avec un élément **\<msix\>** .

```xml
<?xml version="1.0" encoding="utf-8"?>
<assembly manifestVersion="1.0" xmlns="urn:schemas-microsoft-com:asm.v1">
  <assemblyIdentity version="1.0.0.0" name="Contoso.PhotoStoreApp"/>
  <msix xmlns="urn:schemas-microsoft-com:msix.v1"
          publisher="CN=Contoso"
          packageName="ContosoPhotoStore"
          applicationId="ContosoPhotoStore"
        />
</assembly>
```

Les attributs de l’élément **\<msix\>** doivent correspondre à ces valeurs dans le manifeste du package pour votre package Sparse :

* Les attributs **PackageName** et **Publisher** doivent correspondre respectivement aux attributs **Name** et **publisher** de l’élément [**Identity\>\<** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) dans votre manifeste de package.
* L’attribut **ApplicationID** doit correspondre à l’attribut **ID** de l’élément [ **\<\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) de l’application dans votre manifeste de package.

Le manifeste d’application côte à côte doit se trouver dans le même répertoire que le fichier exécutable de votre application de bureau et, par Convention, il doit porter le même nom que le fichier exécutable de votre application avec l’extension `.manifest` ajoutée. Par exemple, si le nom de l’exécutable de votre application est `ContosoPhotoStore`, le nom de fichier du manifeste de l’application doit être `ContosoPhotoStore.exe.manifest`.

## <a name="register-your-sparse-package-at-run-time"></a>Inscrire votre package fragmenté au moment de l’exécution

Pour accorder l’identité de package à votre application de bureau, votre application doit inscrire le package Sparse à l’aide de la classe [packagemanager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager) . Vous pouvez ajouter du code à votre application pour inscrire le package épars quand votre application est exécutée pour la première fois, ou vous pouvez exécuter du code pour inscrire le package pendant l’installation de votre application de bureau (par exemple, si vous utilisez MSI pour installer votre application de bureau). , vous pouvez exécuter ce code à partir d’une action personnalisée.

L’exemple suivant montre comment inscrire un package fragmenté. Ce code crée un objet **AddPackageOptions** qui contient le chemin d’accès à l’emplacement externe où votre manifeste de package peut référencer du contenu en dehors du package. Ensuite, le code passe cet objet à la méthode **packagemanager. AddPackageByUriAsync** pour inscrire le package fragmenté. Cette méthode reçoit également l’emplacement de votre package fragmenté signé en tant qu’URI. Pour obtenir un exemple plus complet, consultez le fichier de code `StartUp.cs` dans l' [exemple](#sample)associé.

```csharp
private static bool registerSparsePackage(string externalLocation, string sparsePkgPath)
{
    bool registration = false;
    try
    {
        Uri externalUri = new Uri(externalLocation);
        Uri packageUri = new Uri(sparsePkgPath);

        Console.WriteLine("exe Location {0}", externalLocation);
        Console.WriteLine("msix Address {0}", sparsePkgPath);

        Console.WriteLine("  exe Uri {0}", externalUri);
        Console.WriteLine("  msix Uri {0}", packageUri);

        PackageManager packageManager = new PackageManager();

        // Declare use of an external location
        var options = new AddPackageOptions();
        options.ExternalLocationUri = externalUri;

        Windows.Foundation.IAsyncOperationWithProgress<DeploymentResult, DeploymentProgress> deploymentOperation = packageManager.AddPackageByUriAsync(packageUri, options);

        // Other progress and error-handling code omitted for brevity...
    }
}
```

## <a name="sample"></a>Exemple

Pour obtenir un exemple d’application entièrement fonctionnel qui montre comment accorder l’identité d’un package à une application de bureau à l’aide d’un package fragmenté, consultez [https://aka.ms/sparsepkgsample](https://aka.ms/sparsepkgsample). Vous trouverez plus d’informations sur la création et l’exécution de l’exemple dans ce billet de [blog](https://blogs.windows.com/windowsdeveloper/2019/10/29/identity-registration-and-activation-of-non-packaged-win32-apps/#HBMFEM843XORqOWx.97).

Cet exemple comprend les éléments suivants :

* Le code source d’une application WPF nommée PhotoStoreDemo. Au démarrage, l’application vérifie si elle s’exécute avec l’identité. S’il ne s’exécute pas avec l’identité, il inscrit le package fragmenté, puis redémarre l’application. Consultez `StartUp.cs` pour obtenir le code qui effectue ces étapes.
* Un manifeste d’application côte à côte nommé `PhotoStoreDemo.exe.manifest`.
* Un manifeste de package nommé `AppxManifest.xml`.
