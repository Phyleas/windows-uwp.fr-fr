---
description: Suivez ces instructions pour préparer les packages de votre application à envoyer au Microsoft Store.
title: Exigences relatives aux packages d’application
ms.assetid: 651B82BA-9D0C-45AC-8997-88CD93DC903C
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP, spécifications du package, packages, format de package, version prise en charge, envoyer
ms.localizationpriority: medium
ms.openlocfilehash: 464714388f9e998bace3af45c580f2a4a7638b27
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034392"
---
# <a name="app-package-requirements"></a>Exigences relatives aux packages d’application

Suivez ces instructions pour préparer les packages de votre application à envoyer au Microsoft Store.

## <a name="before-you-build-your-apps-package-for-the-microsoft-store"></a>Avant de générer le package de votre application pour le Microsoft Store

Veillez à [tester votre application à l’aide du Kit de certification des applications Windows](../debug-test-perf/windows-app-certification-kit.md). Nous vous recommandons également de tester votre application sur différents types de matériels. Notez que tant que nous n’avons pas certifié votre application et que vous l’avez mis à disposition à partir de la Microsoft Store, elle ne peut être installée et exécutée que sur les ordinateurs disposant de licences de développeur.

## <a name="building-the-app-package-using-microsoft-visual-studio"></a>Génération du package d’application à l’aide de Microsoft Visual Studio

Si vous utilisez Visual Studio comme environnement de développement, vous disposez déjà d’outils intégrés pour créer rapidement et facilement un package d’application. Pour plus d’informations, consultez [Empaquetage d’applications](../packaging/index.md).

> [!NOTE]
> Assurez-vous que tous vos noms de fichiers utilisent ANSI. 

Quand vous créez votre package dans Visual Studio, vérifiez que vous êtes connecté au même compte que celui associé à votre compte de développeur. Les informations contenues dans certaines parties du manifeste du package font référence à votre compte. Ces informations sont automatiquement détectées et ajoutées. Si les informations supplémentaires ne sont pas ajoutées au manifeste, vous risquez de rencontrer des échecs de chargement de package. 

Quand vous générez les packages UWP de votre application, Visual Studio peut créer un fichier. msix ou AppX, ou un fichier. msixupload ou. appxupload. Pour les applications UWP, nous vous recommandons de toujours télécharger le fichier. msixupload ou. appxupload dans la page [packages](upload-app-packages.md) . Pour plus d’informations sur l’empaquetage d’applications UWP pour le Windows Store, consultez [empaqueter une application UWP avec Visual Studio](/windows/msix/package/packaging-uwp-apps).

Il n’est pas nécessaire que les packages de votre application soient signés avec un certificat provenant d’une autorité de certification approuvée.


### <a name="app-bundles"></a>Ensembles d’applications

Pour les applications UWP, Visual Studio peut générer un bundle d’applications (. msixbundle ou. appxbundle) pour réduire la taille de l’application que les utilisateurs téléchargent. Cela peut être utile si vous avez défini des ressources propres à une langue, plusieurs ressources de mise à l’échelle d’images ou encore des ressources qui s’appliquent à des versions spécifiques de Microsoft DirectX.

> [!NOTE]
> Une offre groupée d'applications peut contenir vos packages pour toutes les architectures.

Avec un ensemble d’applications, un utilisateur ne télécharge que les fichiers pertinents, et non toutes les ressources disponibles. Pour plus d’informations sur les offres groupées d’applications, consultez [empaquetage d’applications](../packaging/index.md) et [empaqueter une application UWP avec Visual Studio](/windows/msix/package/packaging-uwp-apps).


## <a name="building-the-app-package-manually"></a>Génération manuelle du package d’application

Si vous n’utilisez pas Visual Studio pour créer votre package, vous devez [créer manuellement votre manifeste de package](/uwp/schemas/appxpackage/how-to-create-a-package-manifest-manually).

Pour obtenir des détails complets et les conditions requises concernant le manifeste, consultez la documentation [Manifeste du package d’application](/uwp/schemas/appxpackage/appx-package-manifest). Pour obtenir la certification, votre manifeste doit suivre le schéma du manifeste du package.

Votre manifeste doit inclure des informations spécifiques concernant votre compte et votre application. Pour trouver ces informations, consultez [Visualiser les détails d’identité d’application](view-app-identity-details.md) dans la section **Gestion de l’application** de la page de présentation de votre application, dans le tableau de bord.

> [!NOTE]
> Les valeurs dans le manifeste respectent la casse. Les espaces et autres symboles de ponctuation doivent également correspondre. Saisissez les valeurs correctement et vérifiez-les pour vous assurer qu’elles sont correctes.


Les offres groupées d’applications (. msixbundle ou. appxbundle) utilisent un autre manifeste. Pour obtenir les détails et conditions requises concernant les manifestes d’offres groupées d’applications, consultez la documentation relative au [manifeste d’offre groupée](/uwp/schemas/bundlemanifestschema/bundle-manifest). Notez que dans un fichier. msixbundle ou. appxbundle, le manifeste de chaque package inclus doit utiliser les mêmes éléments et attributs, à l’exception de l’attribut **ProcessorArchitecture** de l’élément [Identity](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) .

> [!TIP]
> Veillez à exécuter le [Kit de certification des applications Windows](../debug-test-perf/windows-app-certification-kit.md) avant de soumettre vos packages. Vous pouvez ainsi déterminer si votre manifeste présente des problèmes susceptibles de faire échouer la certification ou la soumission.


## <a name="package-format-requirements"></a>Exigences relatives au format des packages

Les packages de votre application doivent être conformes aux exigences ci-après.

| Propriété du package de l’application | Condition requise                                                          |
|----------------------|----------------------------------------------------------------------|
| Taille du package         | . msixbundle ou. appxbundle : maximum 25 Go par lot <br>Packages. msix ou. AppX ciblant Windows 10:25 Go maximum par package<br>Packages .appx ciblant Windows 8.1 : 8 Go maximum par package <br> Packages .appx ciblant Windows 8 : 2 Go maximum par package <br> Packages .appx ciblant Windows Phone 8.1 : 4 Go maximum par package <br> Packages .xap : 1 Go maximum par package                                                                           |
| Hachages de mappage de bloc     | Algorithme SHA2-256                                                   |

> [!IMPORTANT]
> Vous ne pouvez plus charger de nouveaux packages XAP générés à l’aide du ou des kits de développement logiciel (SDK) Windows Phone 8. x. Les applications qui sont déjà dans Store avec des packages XAP continuent de fonctionner sur les appareils Windows 10 mobile. Pour plus d’informations, consultez ce billet de [blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

## <a name="supported-versions"></a>Versions prises en charge

Pour les applications UWP, tous les packages doivent cibler une version de Windows 10 prise en charge par le magasin. Les versions prises en charge par votre package doivent être indiquées dans les attributs **MinVersion** et **MaxVersionTested** de l’élément [TargetDeviceFamily](/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) du manifeste de l’application.

Les versions actuellement prises en charge sont les suivantes : 
- Minimum : 10.0.10240.0
- Maximum : 10.0.17763.1


## <a name="storemanifest-xml-file"></a>Fichier XML StoreManifest

Le fichier StoreManifest.xml est un fichier de configuration facultatif qui peut être inclus dans les packages d’application. Son objectif est d’activer des fonctionnalités, telles que la déclaration de votre application en tant qu’application d’appareil Microsoft Store ou la déclaration d’exigences dont dépend un package pour s’appliquer à un appareil, que le manifeste du package ne couvre pas. S’il est utilisé, StoreManifest.xml est soumis avec le package d’application et doit se trouver dans le dossier racine du projet principal de votre application. Pour plus d’informations, voir [Schéma StoreManifest](/uwp/schemas/storemanifest/store-manifest-schema-portal).

 

 
