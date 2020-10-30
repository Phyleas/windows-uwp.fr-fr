---
description: Le Microsoft Store applique certaines règles relatives aux numéros de version, qui fonctionnent différemment dans différentes versions de système d’exploitation.
title: Numérotation des versions de packages
ms.assetid: DD7BAE5F-C2EE-44EE-8796-055D4BCB3152
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 18b4d6b8a901e68ea1e8513b7076a951e939f028
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93035032"
---
# <a name="package-version-numbering"></a>Numérotation des versions de packages

Chaque package que vous fournissez doit avoir un numéro de version (fourni sous la forme d’une valeur dans l’attribut **Version** de l’élément [Package/Identity](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) dans le manifeste de l’application). Le Microsoft Store applique certaines règles relatives aux numéros de version, qui fonctionnent différemment dans différentes versions de système d’exploitation.

> [!NOTE]
> Cette rubrique fait référence aux « packages », mais sauf indication contraire, les mêmes règles s’appliquent aux numéros de version des fichiers. msix/. AppX et. msixbundle/. appxbundle.


## <a name="version-numbering-for-windows-10-packages"></a>Numérotation des versions pour les packages Windows 10

> [!IMPORTANT]
> Pour les packages Windows 10 (UWP), la dernière section (quatrième) du numéro de version est réservée à l’utilisation du magasin et doit être laissée sur 0 lorsque vous générez votre package (même si le magasin peut modifier la valeur de cette section). Les autres sections doivent être définies sur un entier compris entre 0 et 65535 (à l’exception de la première section, qui ne peut pas être 0).

Lorsque vous choisissez un package UWP dans votre soumission publiée, le Microsoft Store utilisera toujours le package avec version la plus élevée applicable à l’appareil Windows 10 du client. Cela vous offre une plus grande souplesse et vous permet de contrôler les packages fournis aux clients sur des types spécifiques d’appareils. Il est important de noter que vous pouvez soumettre ces packages dans n’importe quel ordre ; vous n’êtes pas obligé de fournir des packages dont le numéro de version est supérieur avec chaque soumission ultérieure.

Vous pouvez fournir plusieurs packages UWP avec le même numéro de version. Toutefois, les packages qui partagent un même numéro de version ne peuvent pas avoir la même architecture, car l’identité complète que le Windows Store utilise pour chaque package doit être unique. Pour plus d’informations, voir [**Identity**](/uwp/schemas/appxpackage/uapmanifestschema/element-identity).

Lorsque vous fournissez plusieurs packages UWP qui utilisent le même numéro de version, l’architecture (dans l’ordre x64, x86, ARM, neutre) est utilisée pour déterminer celle qui est du rang le plus élevé (lorsque le magasin détermine le package à fournir à l’appareil d’un client). Lors du classement des ensembles d’applications qui utilisent la même version, le niveau d’architecture le plus élevé dans l’ensemble est pris en considération : un ensemble d’applications contenant un package x64 aura un classement plus élevé qu’un ensemble contenant uniquement un package x86.

Cela vous offre une grande souplesse pour faire évoluer votre application au fil du temps. Vous pouvez télécharger et envoyer de nouveaux packages qui utilisent des numéros de version inférieurs pour ajouter la prise en charge des appareils Windows 10 que vous n’avez pas précédemment pris en charge, vous pouvez ajouter des packages avec des versions plus récentes qui ont des dépendances plus strictes pour tirer parti des fonctionnalités matérielles ou du système d’exploitation, ou vous pouvez ajouter des packages avec des versions ultérieures qui servent de

L’exemple suivant montre comment gérer la numérotation des versions pour livrer les packages voulus à vos clients au gré de soumissions multiples.

### <a name="example-moving-to-a-single-package-over-multiple-submissions"></a>Exemple : évolution vers un package unique en plusieurs soumissions

Windows 10 vous permet d’écrire un seul code base qui s’exécute partout. Cela facilite considérablement le démarrage d’un nouveau projet interplateforme. Toutefois, pour diverses raisons, il peut sembler inopportun de fusionner plusieurs codes base existants pour créer immédiatement un seul projet.

Vous pouvez utiliser les règles de contrôle de version du package pour amener progressivement vos clients vers un package unique pour la famille d’appareils universelle, tout en envoyant un certain nombre de mises à jour intermédiaires pour des familles spécifiques d’appareils (y compris ceux qui tirent parti des API Windows 10). L’exemple ci-dessous montre comment les mêmes règles sont appliquées de manière cohérente sur une série d’envois pour la même application.

| Envoi | Contenu                                                  | Expérience client                                                                                                                                                                             |
|------------|-----------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1          | -Version du package : 1.1.10.0 <br> -Famille d’appareils : Windows. Desktop, minVersion 10.0.10240.0 <br> <br> -Version du package : 1.1.0.0 <br> -Famille d’appareils : Windows. mobile, minVersion 10.0.10240.0     | -Les appareils sur Windows 10 Desktop Build 10.0.10240.0 et versions ultérieures obtiendront 1.1.10.0 <br> -Les appareils sur Windows 10 Mobile Build 10.0.10240.0 et versions ultérieures obtiendront 1.1.0.0 <br> -Les autres familles d’appareils ne seront pas en mesure d’acheter et d’installer l’application |
| 2          | -Version du package : 1.1.10.0 <br> -Famille d’appareils : Windows. Desktop, minVersion 10.0.10240.0 <br> <br> -Version du package : 1.1.0.0 <br> -Famille d’appareils : Windows. mobile, minVersion 10.0.10240.0 <br> <br> -Version du package : 1.0.0.0 <br> -Famille d’appareils : Windows. Universal, minVersion 10.0.10240.0    | -Les appareils sur Windows 10 Desktop Build 10.0.10240.0 et versions ultérieures obtiendront 1.1.10.0 <br> -Les appareils sur Windows 10 Mobile Build 10.0.10240.0 et versions ultérieures obtiendront 1.1.0.0 <br> -Les autres familles d’appareils (autres que les ordinateurs de bureau, non mobiles) quand elles sont introduites obtiennent 1.0.0.0 <br> -Les appareils mobiles et de bureau sur lesquels l’application est déjà installée ne voient aucune mise à jour (car ils disposent déjà de la meilleure version disponible, 1.1.10.0 et 1.1.0.0 sont supérieurs à 1.0.0.0) |
| 3          | -Version du package : 1.1.10.0 <br> -Famille d’appareils : Windows. Desktop, minVersion 10.0.10240.0 <br> <br> -Version du package : 1.1.5.0 <br> -Famille d’appareils : Windows. Universal, minVersion 10.0.10250.0 <br> <br> -Version du package : 1.0.0.0 <br> -Famille d’appareils : Windows. Universal, minVersion 10.0.10240.0    | -Les appareils sur Windows 10 Desktop Build 10.0.10240.0 et versions ultérieures obtiendront 1.1.10.0 <br> -Les appareils sur Windows 10 Mobile Build 10.0.10250.0 et versions ultérieures obtiendront 1.1.5.0 <br> -Les appareils sur Windows 10 Mobile Build >= 10.0.10240.0 et < 10.010250.0 obtiendront 1.1.0.0 
| 4          | -Version du package : 2.0.0.0 <br> -Famille d’appareils : Windows. Universal, minVersion 10.0.10240.0   | -Tous les clients sur toutes les familles d’appareils sur Windows 10 Build v 10.0.10240.0 et versions ultérieures obtiendront le package 2.0.0.0 | 

> [!NOTE]
>  Dans tous les cas, les appareils clients recevront le package avec le numéro de version le plus élevé possible. Par exemple, dans la troisième soumission ci-dessus, tous les appareils de bureau recevront le package v1.1.10.0, même si leur système d’exploitation est de la version 10.0.10250.0 ou supérieure, et pourraient donc également accepter le package v1.1.5.0. Étant donné que le package 1.1.10.0 est celui dont le numéro de version est le plus élevé à leur disposition, ils obtiendront ce package.

### <a name="using-version-numbering-to-roll-back-to-a-previously-shipped-package-for-new-acquisitions"></a>Utilisation de la numérotation des versions pour revenir à un package livré précédemment pour de nouvelles acquisitions

Si vous conservez des copies de vos packages, vous avez la possibilité de restaurer le package de votre application dans le Store vers un package Windows 10 antérieur si vous devez détecter des problèmes avec une version. Il s’agit d’un moyen temporaire de limiter l’interruption à vos clients pendant que vous prenez le temps de résoudre le problème.

Pour ce faire, créez une nouvelle [soumission](app-submissions.md). Supprimez le package problématique et chargez l’ancien package que vous souhaitez fournir dans le Windows Store. Les clients qui ont déjà reçu le package que vous restaurez auront toujours le package problématique (car votre ancien package aura un numéro de version antérieur). Cela n’empêchera cependant aucun autre utilisateur d’acquérir le package problématique, tout en permettant à l’application de rester disponible dans le Windows Store.

Pour résoudre le problème pour les clients qui ont déjà reçu le package problématique, vous pouvez envoyer un nouveau package Windows 10 avec un numéro de version plus élevé que le package incorrect dès que possible. Une fois cette soumission certifiée, tous les clients seront mis à jour vers le nouveau package, car celui-ci aura un numéro de version supérieur.


## <a name="version-numbering-for-windows-81-and-earlier-and-windows-phone-81-packages"></a>Numérotation des versions des packages pour Windows 8.1 (et versions antérieures) et Windows Phone 8.1

> [!IMPORTANT]
> Vous ne pouvez plus charger de nouveaux packages XAP générés à l’aide du ou des kits de développement logiciel (SDK) Windows Phone 8. x. Les applications qui sont déjà dans Store avec des packages XAP continuent de fonctionner sur les appareils Windows 10 mobile. Pour plus d’informations, consultez ce billet de [blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

Pour les packages .appx qui ciblent Windows Phone 8.1, le numéro de version du package dans une nouvelle soumission doit toujours être supérieur à celui du package inclus dans la dernière soumission (ou toute soumission précédente).

Pour les packages. AppX qui ciblent Windows 8 et Windows 8.1, la même règle s’applique par architecture : le numéro de version du package dans une nouvelle soumission doit toujours être supérieur à celui du dernier package publié dans le magasin pour la même architecture.

De plus, le numéro de version des packages Windows 8.1 doit toujours être supérieur aux numéros de version de vos packages Windows 8 pour la même application. Autrement dit, le numéro de version d’un package Windows 8 que vous soumettez doit être inférieur au numéro de version de tout package Windows 8.1 que vous avez soumis pour la même application.

> [!NOTE]
> Si votre application a également des packages Windows 10, le numéro de version des packages Windows 10 doit être supérieur à ceux de vos packages Windows 8, Windows 8.1 et/ou Windows Phone 8,1. Pour plus d’informations, consultez [Ajout de packages pour Windows 10 à une application précédemment publiée](guidance-for-app-package-management.md#adding-packages-for-windows-10-to-a-previously-published-app).

Voici quelques exemples de ce qui se passe dans les différents scénarios de mise à jour des numéros de version pour les packages ciblant Windows 8 et Windows 8.1.

| Version de votre application dans le Store  | Version transférée | Une fois la nouvelle version dans le magasin, elle est installée dans une nouvelle acquisition | Une fois la nouvelle version dans le magasin, elle est mise à jour si un client a déjà l’application |
|---------------------------------------------|-----------------------------|--------------------------------------------------------------------------------------------|----------|
| Rien                                     | x86, v1.0.0.0               | x86, v1.0.0.0 sur les ordinateurs x86 et x64                                                | Nothing. |
| x86, v1.0.0.0                               | x64, v1.0.0.0               | v1.0.0.0 pour l’architecture de l’ordinateur du client                                                   | Nothing. Les numéros de version sont les mêmes. |
| x86, v1.0.0.0 <br> x64, v1.0.0.0            | x64, v1.0.0.1               | v1.0.0.0 pour les clients possédant un ordinateur x86 <br> v1.0.0.1 pour les clients possédant un ordinateur x64                 | Rien pour les clients exécutant l’application sur un ordinateur x86. <br> Mise à jour de v1.0.0.0 vers v1.0.0.1 pour les clients exécutant l’application sur un ordinateur x64. <br> **Remarque**  Si la version x86 de l’application s’exécute sur un ordinateur x64, l’application n’est pas mise à jour vers la version x64, à moins que le client ne soit désinstallé et réinstallé. |
| Rien                                     | neutre, v1.0.0.1           | neutre, v1.0.0.1 sur tous les ordinateurs                                                         | Nothing. |
| neutre, v1.0.0.1                           | x86, v1.0.0.0 <br> x64, v1.0.0.0 <br> ARM, v1.0.0.0 | v1.0.0.0 pour l’architecture de l’ordinateur du client.          | Nothing. Les clients qui possèdent la version neutre v1.0.0.1 de l’application continuent de l’utiliser. |
| neutre, v1.0.0.1 <br> x86, v1.0.0.0 <br> x64, v1.0.0.0 <br> ARM, v1.0.0.0 | x86, v1.0.0.1 <br> x64, v1.0.0.1 <br> ARM, v1.0.0.1 | v1.0.0.1 pour l’architecture de l’ordinateur du client. | Rien pour les clients qui exécutent la version neutre v1.0.0.1 de l’application. <br> Mise à jour de v1.0.0.0 vers v1.0.0.1 pour les clients exécutant la version v1.0.0.0 de l’application générée pour l’architecture de leur ordinateur. |
| x86, v1.0.0.1 <br> x64, v1.0.0.1 <br> ARM, v1.0.0.1 | x86, v1.0.0.2 <br> x64, v1.0.0.2 <br> ARM, v1.0.0.2 | v1.0.0.2 pour l’architecture de l’ordinateur du client.  | Mise à jour de v1.0.0.1 vers v1.0.0.2 pour les clients exécutant la version v1.0.0.1 de l’application générée pour l’architecture de leur ordinateur. |

> [!NOTE]
> Contrairement aux packages. AppX, les numéros de version des packages. xap ne sont pas pris en compte lors de la détermination du package à fournir à un client donné. Pour mettre à jour un client d’un package .xap vers une version plus récente, veillez à supprimer l’ancien .xap dans la nouvelle soumission.
