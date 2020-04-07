---
Description: Découvrez comment accorder une identité à des applications de bureau non empaquetées afin d'y utiliser les fonctionnalités Windows 10 modernes.
title: Accorder une identité à des applications de bureau non empaquetées
ms.date: 02/28/2020
ms.topic: article
keywords: windows 10, bureau, package, identité, MSIX, Win32
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ae05a00cac19fdd349aa48160b88cde6b84e26b0
ms.sourcegitcommit: 620e4a51e2486ec2cb7190176b3d9bf3d7b5b6af
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/02/2020
ms.locfileid: "78222025"
---
# <a name="grant-identity-to-non-packaged-desktop-apps"></a>Accorder une identité à des applications de bureau non empaquetées

<!--
> [!NOTE]
> The features described in this article require Windows 10 Insider Preview Build 10.0.19000.0 or a later release.
-->

Les applications de bureau non-UWP doivent utiliser une [identité de package](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) pour accéder à bon nombre de fonctionnalités d'extensibilité Windows 10, notamment les tâches en arrière-plan, notifications, vignettes dynamiques et cibles de partage. Pour ces scénarios, le système d’exploitation requiert une identité afin d'identifier l’appelant de l’API correspondante.

Dans les versions du système d’exploitation antérieures à Windows 10 Insider Preview Build 10.0.19000.0, la seule façon d’accorder une identité à une application de bureau consiste à [l'empaqueter dans un package MSIX signé](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root). Pour ces applications, l’identité est spécifiée dans le manifeste du package et l’inscription de l’identité est gérée par le pipeline de déploiement MSIX en fonction des informations contenues dans le manifeste. Tout le contenu référencé dans le manifeste du package est présent dans le package MSIX.

À partir de Windows 10 Insider Preview Build 10.0.19000.0, vous pouvez accorder une identité de package aux applications de bureau non empaquetées dans un package MSIX en générant et en inscrivant un *package partiellement alloué* avec votre application. Cette prise en charge permet aux applications de bureau ne pouvant adopter l'empaquetage MSIX à des fins de déploiement d’utiliser les fonctionnalités d’extensibilité Windows 10 qui requièrent une identité de package. Pour plus d’informations, consultez [ce billet de blog](https://blogs.windows.com/windowsdeveloper/2019/10/29/identity-registration-and-activation-of-non-packaged-win32-apps/#HBMFEM843XORqOWx.97).

Pour générer et inscrire un package partiellement alloué accordant une identité de package à votre application de bureau, suivez les étapes ci-dessous.

1. [Créer un manifeste de package pour le package partiellement alloué](#create-a-package-manifest-for-the-sparse-package)
2. [Générer et signer le package partiellement alloué](#build-and-sign-the-sparse-package)
3. [Ajouter les métadonnées d’identité de package au manifeste de votre application de bureau](#add-the-package-identity-metadata-to-your-desktop-application-manifest)
4. [Inscrire le package partiellement alloué au moment de l’exécution](#register-your-sparse-package-at-run-time)

## <a name="important-concepts"></a>Concepts importants

Les fonctionnalités suivantes permettent aux applications de bureau non empaquetées d'obtenir une identité de package.

### <a name="sparse-packages"></a>Packages partiellement alloués

Un *package partiellement alloué* contient un manifeste, mais aucun autre fichier binaire d’application et contenu. Le manifeste d’un package partiellement alloué peut référencer des fichiers extérieurs au package dans un emplacement externe prédéterminé. Ainsi, les applications qui ne sont pas encore en mesure d’adopter l'empaquetage MSIX peuvent obtenir une identité de package, comme l'exigent certaines fonctionnalités d’extensibilité Windows 10.

> [!NOTE]
> Une application de bureau utilisant un package partiellement alloué ne bénéficie pas tous les avantages d'un déploiement complet via un package MSIX. Parmi ces avantages figurent la protection contre les falsifications, l’installation dans un emplacement verrouillé et la gestion complète par le système d’exploitation du déploiement, de l’exécution et de la désinstallation.

### <a name="package-external-location"></a>Emplacement externe du package

Pour prendre en charge les packages partiellement alloués, le schéma du manifeste de package prend désormais en charge un élément **\<AllowExternalContent\>** facultatif sous l'élément [ **\<Propriétés\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-properties). Le manifeste de votre package peut ainsi référencer du contenu en dehors du package, à un emplacement spécifique sur le disque.

Par exemple, si votre application de bureau non empaquetée installe le fichier exécutable et d’autres contenus dans C:\Program Files\MyDesktopApp\,, vous pouvez créer un package partiellement alloué incluant l'élément **\<AllowExternalContent\>** dans le manifeste. Lors du processus d’installation de votre application, vous pouvez installer le package partiellement alloué et déclarer C:\Program Files\MyDesktopApp\ comme emplacement externe utilisé par votre application.

## <a name="create-a-package-manifest-for-the-sparse-package"></a>Créer un manifeste de package pour le package partiellement alloué

Pour générer un package partiellement alloué, vous devez d’abord créer un [manifeste de package](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest) (fichier nommé AppxManifest.xml) qui déclare les métadonnées d’identité de package de votre application de bureau, entre autres informations requises. Pour créer un manifeste de package pour le package alloué partiellement, le plus simple consiste à utiliser l’exemple ci-dessous et à l'adapter à votre application à l’aide de la [référence de schéma](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root).

Le manifeste de package doit comprendre les éléments suivants :

* Un élément [ **\<Identity\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) décrivant les attributs d’identité de votre application de bureau.
* Un élément **\<AllowExternalContent\>** sous l'élément [ **\<Propriétés\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-properties). La valeur `true` doit être affectée à cet élément pour permettre au manifeste de votre package de référencer du contenu en dehors du package, à un emplacement spécifique sur le disque. Dans une étape ultérieure, vous spécifierez cet emplacement, et plus précisément lors de l'inscription de votre package partiellement alloué à partir du code exécuté dans votre programme d’installation ou votre application. Tout contenu référencé dans le manifeste et absent du package doit être installé à l’emplacement externe.
* L’attribut **MinVersion** de l’élément [ **\<TargetDeviceFamily\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) doit être défini sur `10.0.19000.0` ou version ultérieure.
* Les attributs **TrustLevel=mediumIL** et **RuntimeBehavior = Win32App** de l'élément [ **\<Application\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) déclarent que l’application de bureau associée au package partiellement alloué s’exécutera de la même façon qu’une application de bureau non empaquetée standard, sans virtualisation du registre et du système de fichiers, entre autres modifications d’exécution.

L’exemple suivant montre le contenu complet d’un manifeste de package partiellement alloué (AppxManifest.xml). Ce manifeste comprend une extension `windows.sharetarget`, nécessitant une identité de package.

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

## <a name="build-and-sign-the-sparse-package"></a>Générer et signer le package partiellement alloué

Une fois le manifeste de votre package créé, générez le package partiellement alloué à l’aide de l’[outil MakeAppx.exe](https://docs.microsoft.com/windows/msix/package/create-app-package-with-makeappx-tool) dans le SDK Windows. Le package partiellement alloué ne contenant pas les fichiers référencés dans le manifeste, vous devez spécifier l’option `/nv`, qui ignore la validation sémantique du package.

L’exemple suivant montre comment créer un package partiellement alloué à partir de la ligne de commande.  

```Console
MakeAppx.exe  pack  /d  <path to directory that contains manifest>  /p  <output path>\MyPackage.msix  /nv
```

Pour installer votre package partiellement alloué sur un ordinateur cible, vous devez le signer à l'aide d'un certificat approuvé sur l’ordinateur cible. Vous pouvez créer un certificat auto-signé à des fins de développement et signer votre package partiellement alloué à l’aide de [SignTool](https://docs.microsoft.com/windows/msix/package/sign-app-package-using-signtool), disponible dans le SDK Windows.

L’exemple suivant montre comment signer un package partiellement alloué à partir de la ligne de commande.

```Console
SignTool.exe sign /fd SHA256 /a /f <path to certificate>\MyCertificate.pfx  /p <certificate password>  <path to sparse package>\MyPackage.msix
```

### <a name="add-the-package-identity-metadata-to-your-desktop-application-manifest"></a>Ajouter les métadonnées d’identité du package au manifeste de votre application de bureau

Vous devez également inclure un [manifeste d’application côte à côte](https://docs.microsoft.com/windows/win32/sbscs/application-manifests) avec votre application de bureau, de même qu'un élément [&lt;msix&gt;](https://docs.microsoft.com/windows/win32/sbscs/application-manifests#msix) avec des attributs qui déclarent les attributs d’identité de votre application. Les valeurs de ces attributs sont utilisées par le système d’exploitation pour déterminer l’identité de votre application lors du lancement du fichier exécutable.

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

Les attributs de l'élément **\<msix\>** doivent correspondre à ces valeurs dans le manifeste du package de votre package partiellement alloué :

* Les attributs **packageName** et **publisher** doivent correspondre aux attributs **Name** et **Publisher** de l'élément [ **\<Identity\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) du manifeste de votre package.
* L’attribut **applicationId** doit correspondre à l’attribut **Id** de l'élément [ **\<Application\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) du manifeste de votre application.

Le manifeste d’application côte à côte doit se trouver dans le même répertoire que le fichier exécutable de votre application de bureau et, par convention, porter le même nom que le fichier exécutable de votre application avec l’extension `.manifest`. Par exemple, si le nom du fichier exécutable de votre application est `ContosoPhotoStore`, le nom de fichier du manifeste de l’application doit être `ContosoPhotoStore.exe.manifest`.

## <a name="register-your-sparse-package-at-run-time"></a>Inscrire le package partiellement alloué au moment de l’exécution

Pour accorder une identité de package à votre application de bureau, cette dernière doit inscrire le package partiellement alloué à l’aide de la classe [PackageManager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager). Vous pouvez ajouter du code à votre application pour inscrire le package partiellement alloué lors de la première exécution de votre application. Vous pouvez également exécuter du code afin d'inscrire le package lors de l’installation de votre application de bureau (par exemple, si vous utilisez MSI pour installer votre application de bureau, vous pouvez exécuter ce code à l'aide d'une action personnalisée).

L'exemple suivant montre comment inscrire un package partiellement alloué. Ce code crée un objet **AddPackageOptions** contenant le chemin de l'emplacement externe où le manifeste de votre package peut référencer du contenu en dehors du package. Ensuite, le code transmet cet objet à la méthode **PackageManager.AddPackageByUriAsync** pour inscrire le package partiellement alloué. Cette méthode reçoit également l’emplacement de votre package partiellement alloué signé sous forme d'URI. Pour un exemple plus détaillé, consultez le fichier de code `StartUp.cs` de l'[exemple](#sample) correspondant.

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

Pour un exemple d’application pleinement fonctionnelle montrant comment accorder une identité de package à une application de bureau à l’aide d’un package partiellement alloué, consultez [https://github.com/microsoft/AppModelSamples/tree/master/Samples/SparsePackages](https://github.com/microsoft/AppModelSamples/tree/master/Samples/SparsePackages). Pour plus d’informations sur la création et l’exécution de l’exemple, consultez [ce billet de blog](https://blogs.windows.com/windowsdeveloper/2019/10/29/identity-registration-and-activation-of-non-packaged-win32-apps/#HBMFEM843XORqOWx.97).

Cet exemple inclut ce qui suit :

* Le code source d’une application WPF nommée PhotoStoreDemo. Au démarrage, l’application vérifie si elle s’exécute avec l’identité. Si ce n'est pas le cas, elle inscrit le package partiellement alloué, puis redémarre. Consultez `StartUp.cs` pour obtenir le code qui effectue ces étapes.
* Un manifeste d’application côte à côte nommé `PhotoStoreDemo.exe.manifest`.
* Un manifeste du package nommé `AppxManifest.xml`.
