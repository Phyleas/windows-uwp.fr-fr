---
title: Étendre votre application avec des services, des extensions et des packages
description: Décrit comment créer une tâche en arrière-plan qui s’exécute lorsque votre application de magasin plateforme Windows universelle (UWP) est mise à jour.
ms.date: 05/07/2018
ms.topic: article
keywords: Windows 10, UWP, étendre, composant, app service, package, extension
ms.localizationpriority: medium
ms.openlocfilehash: 9239178701082012f2878e996ede2f3f56a9a94b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155873"
---
# <a name="extend-your-app-with-services-extensions-and-packages"></a>Étendre votre application avec des services, des extensions et des packages

Windows 10 propose de nombreuses technologies permettant d’étendre et de composant votre application. Ce tableau doit vous aider à déterminer la technologie à utiliser en fonction de vos besoins. Elle est suivie d’une brève description des scénarios et des technologies.

| Scénario                           | Package de ressources   | Package de ressources      | Package facultatif   | Bundle plat        | Extension d’application      | App Service        | Installation en continu  |
|------------------------------------|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|
| Plug-ins de code tiers            |                    |                    |                    |                    | :heavy_check_mark: |                    |                    |
| Plug-ins de code in-proc              |                    |                    | :heavy_check_mark: |                    |                    |                    |                    |
| Ressources UX (chaînes/images)         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| Contenu à la demande <br/> (par exemple, niveaux supplémentaires) |      |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| Licence et acquisition distinctes |                    |                    | :heavy_check_mark: |                    | :heavy_check_mark: | :heavy_check_mark: |                    |
| Acquisition dans l’application                 |                    |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    |                    |
| Optimiser l’heure d’installation              | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| Réduire l’encombrement du disque              | :heavy_check_mark: |                    | :heavy_check_mark: |                    |                    |                    |                    |
| Optimiser l’empaquetage                 |                    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |
| Réduire la durée de la publication             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |

## <a name="scenario-descriptions-the-rows-in-the-table-above"></a>Descriptions du scénario (lignes du tableau ci-dessus)

**Plug-ins tiers**  

Code que vous pouvez télécharger à partir du Store et exécuter à partir de votre application. Par exemple, extensions pour le navigateur Microsoft Edge.

**Plug-ins de code in-proc**  

Code qui s’exécute in-process avec votre application. Peut également inclure du contenu. Étant donné que le code s’exécute dans un processus, un niveau de confiance plus élevé est pris en compte. Vous pouvez choisir de ne pas exposer ce type d’extensibilité à une tierce partie.

**Ressources UX (chaînes/images)**  

Ressources de l’interface utilisateur, telles que les chaînes localisées, les images et tout autre contenu de l’interface utilisateur que vous souhaitez factoriser en fonction des paramètres régionaux ou de toute autre raison.

**Contenu à la demande**  

Contenu que vous souhaitez télécharger ultérieurement. Par exemple, les achats dans l’application qui vous permettent de télécharger de nouveaux niveaux, des nouvelles apparences ou des fonctionnalités.

**Licence et acquisition distinctes**  

Possibilité de disposer d’une licence et d’acquérir le contenu indépendamment de l’application.

**Acquisition dans l’application**  

Indique s’il existe une prise en charge par programme pour acquérir le contenu à partir de l’application.

**Optimiser l’heure d’installation**

Fournit des fonctionnalités pour réduire le temps nécessaire à l’acquisition de l’application à partir du magasin et au démarrage de l’exécution.

**Réduire l’encombrement du disque** Réduit la taille d’une application en incluant uniquement les applications ou les ressources nécessaires.

**Optimiser l’empaquetage** Optimise le processus d’empaquetage d’application pour les applications à grande échelle ou complexes.

**Réduire la durée** de la publication Réduisez la durée nécessaire à la publication de votre application dans le magasin, le partage local ou le serveur Web.

## <a name="technology-descriptions-the-columns-in-the-table-above"></a>Descriptions des technologies (les colonnes du tableau ci-dessus)

**Package de ressources**

Les packages de ressources sont des packages d’actifs uniquement qui permettent à votre application de s’adapter à plusieurs tailles d’affichage et langues système. Le package de ressources cible la langue de l’utilisateur, l’échelle du système et les fonctionnalités DirectX, permettant à l’application d’être adaptée à un large éventail de scénarios utilisateur. Bien qu’un package d’application puisse contenir plusieurs ressources, le système d’exploitation télécharge uniquement les ressources pertinentes par appareil utilisateur, ce qui économise de la bande passante et de l’espace disque.

**Package de ressources** Les packages d’actifs sont une source commune et centralisée de fichiers exécutables ou non exécutables pour une utilisation par votre application. Il s’agit généralement de fichiers non-processeur ou spécifiques à une langue. Par exemple, il peut s’agir d’une collection d’images dans un package de ressources et de vidéos dans un autre package de ressources, qui sont utilisées par l’application. Si votre application prend en charge plusieurs architectures et plusieurs langues, ces ressources peuvent être incluses dans le package d’architecture ou le package de ressources, mais cela signifie également que les ressources sont dupliquées plusieurs fois dans les différents packages d’architecture, en prenant de l’espace disque. Si vous utilisez des packages de ressources, ils ne doivent être inclus qu’une seule fois dans le package d’application global. Pour en savoir plus, consultez [Présentation des packages d’actifs](/windows/msix/package/asset-packages) .

**Package facultatif**

Les packages facultatifs sont utilisés pour compléter ou étendre les fonctionnalités d’origine d’un package d’application. Il est possible de publier une application, puis de publier ultérieurement des packages facultatifs, ou de publier l’application et les packages facultatifs simultanément. En étendant votre application via un package facultatif, vous bénéficiez des avantages de la distribution et de la monétisation du contenu sous la forme d’un package d’application distinct. Les packages facultatifs sont généralement destinés à être développés par le développeur de l’application d’origine, car ils s’exécutent avec l’identité de l’application principale (contrairement aux extensions d’application). Selon la façon dont vous définissez votre package facultatif, vous pouvez charger du code, des ressources ou du code et des ressources à partir de votre package facultatif dans votre application principale. Si vous avez besoin d’améliorer votre application avec du contenu qui peut être monétisé, concédé sous licence et distribué séparément, les packages facultatifs peuvent être le bon choix pour vous. Pour plus d’informations sur l’implémentation, consultez [packages facultatifs et création de jeux associés](/windows/msix/package/optional-packages).

**Bundle plat** 
 Les [packages d’application de groupement plats](/windows/msix/package/flat-bundles) sont similaires aux lots d’applications standard, mais au lieu d’inclure tous les packages d’application dans le dossier, le bundle plat contient uniquement des *références* à ces packages d’application. En contenant des références à des packages d’application au lieu des fichiers eux-mêmes, un bundle plat réduit la durée nécessaire pour empaqueter et télécharger une application.

**Extension d’application**

Les [extensions d’application](/uwp/api/windows.applicationmodel.appextensions) permettent à votre application UWP d’héberger du contenu fourni par d’autres applications UWP. Découvrez, énumérez et accédez à du contenu en lecture seule à partir de ces applications.

Si une application prend en charge les extensions, n’importe quel développeur peut soumettre une extension pour l’application. Ainsi, l’application hôte doit être robuste lors du chargement d’une extension avec laquelle elle n’a pas été testée. Les extensions doivent être considérées comme non fiables.

Les applications ne peuvent pas charger du code à partir d’extensions. Si vous avez besoin d’exécuter du code, envisagez App Services.

**App Service**

Les services d’application Windows activent la communication entre applications en permettant à votre application UWP de fournir des services à une autre application Windows universelle. Les services d’application vous permettent de créer des services sans interface utilisateur que les applications peuvent appeler sur le même appareil et à partir de Windows 10, version 1607, sur des appareils distants. Pour plus d’informations [, consultez créer et consommer un service d’application](./how-to-create-and-consume-an-app-service.md) .

Les services d’application sont des applications UWP qui fournissent des services à d’autres applications UWP. Ils sont analogues aux services Web sur un appareil. Un app service s’exécute en tant que tâche en arrière-plan dans l’application hôte et peut fournir son service à d’autres applications. Par exemple, un service d’application peut fournir un service d’analyse de code-barres que d’autres applications peuvent utiliser. Ou peut-être une suite d’applications d’entreprise dispose d’un service d’application de vérification orthographique commun qui est disponible pour les autres applications de la suite.

**Installation de la diffusion en continu d’applications UWP**

L’installation en continu est un moyen d’optimiser la façon dont votre application est livrée aux utilisateurs. Au lieu d’attendre que l’application entière soit téléchargée avant de pouvoir l’utiliser, les utilisateurs peuvent s’associer à l’application dès qu’une partie requise a été téléchargée. C’est à vous, en tant que développeur, de segmenter votre application dans une section requise pour l’activation et le lancement de base et le contenu supplémentaire pour le reste de l’application. Pour plus d’informations et pour plus d’informations sur l’implémentation, consultez [installation de streaming d’applications UWP](/windows/msix/package/streaming-install) .

## <a name="see-also"></a>Voir aussi

[Créer et consommer un service d’application](./how-to-create-and-consume-an-app-service.md)  
[Introduction aux packages d’actifs](/windows/msix/package/asset-packages)  
[Création de package à l’aide de la disposition de mise en package](/windows/msix/package/packaging-layout)  
[Création de packages facultatifs et d’ensembles associés](/windows/msix/package/optional-packages)  
[Développer avec des packages d’actifs et la mise en dossier des packages](/windows/msix/package/package-folding)  
[Installation en continu d’une application UWP](/windows/msix/package/streaming-install)  
[Packages de bundles d’applications plats](/windows/msix/package/flat-bundles)  
[Espace de noms Windows. ApplicationModel. AppService](/uwp/api/Windows.ApplicationModel.AppService)  
[Espace de noms Windows. ApplicationModel. extensions](/uwp/api/windows.applicationmodel.appextensions)