---
description: Grâce aux API d’indexation de ressources de package (IRP), vous pouvez développer un système de génération personnalisé pour les ressources de votre application UWP. Le système de génération pourra créer, versionner, et vider les fichiers d’index de ressource de package (IRP) au niveau de complexité dont votre application UWP a besoin.
title: Indexation des ressources de package et systèmes de génération personnalisés
template: detail.hbs
ms.date: 05/07/2018
ms.topic: article
keywords: windows 10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: 84b6a05927e2106df136741a262c3af5ef08a575
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031662"
---
# <a name="package-resource-indexing-pri-apis-and-custom-build-systems"></a>API d’indexation de ressources de package (IRP) et systèmes de génération personnalisés
Grâce aux [API d’indexation de ressources de package (IRP)](/windows/desktop/menurc/pri-indexing-reference), vous pouvez développer un système de génération personnalisé pour les ressources de votre application UWP. Le système de génération pourra créer, versionner et vider (en tant que XML) les fichiers d’index de ressources de package (IRP) au niveau de complexité dont votre application UWP a besoin. Si vous disposez d’un système de génération personnalisé qui utilise actuellement l’outil en ligne de commande MakePri.exe (consultez [compiler des ressources manuellement avec MakePri.exe](makepri-exe-command-options.md)), pour des performances et un contrôle accrus, nous vous recommandons de passer à l’appel des API PRI au lieu d’appeler MakePri.exe.

Les API PRI ont été introduites dans le SDK Windows pour Windows 10, version 1803. Les API prennent la forme d’API Windows Win32, ce qui signifie que vous disposez de plusieurs options pour les appeler. Vous pouvez les appeler directement à partir d’une application Win32, ou vous pouvez les appeler via l’appel de code non [managé](/dotnet/framework/interop/consuming-unmanaged-dll-functions?branch=live) à partir d’une application .net ou même à partir d’une application UWP.

Les scénarios de cette rubrique illustrent l’appel d’API PRI à partir d’un projet d’application console Windows Visual C++ Win32. Pour obtenir des informations générales, consultez [système de gestion des ressources](resource-management-system.md).

> [!NOTE]
> Il est peu probable qu’il y ait un problème, car vous ne souhaiterez probablement pas soumettre votre application de système de génération personnalisée au Microsoft Store. Toutefois, si vous choisissez l’option de développement de votre système de génération personnalisé sous la forme d’une application UWP, il s’agit d’une application UWP inhabituelle dans la mesure où vous ne pouvez pas la soumettre au Microsoft Store. Cela est dû au fait qu’une application UWP qui utilise l’appel de code non managé échoue Microsoft Store certification. Notez que, dans ce cas, les appels de code non managé *existent uniquement dans votre système de génération personnalisé* ; *pas* dans votre application UWP d’expédition (celle pour laquelle vous créez des fichiers PRI).

## <a name="scenario-walkthroughs"></a>Procédures pas à pas de scénario
|Rubrique|Description|
|-|-|
|[Scénario 1 : générer un fichier PRI à partir de ressources de type chaîne et de fichiers d’éléments multimédias](pri-apis-scenario-1.md)|Dans ce scénario, nous allons créer une nouvelle application pour représenter notre système de génération personnalisé. Nous allons créer un indexeur de ressource et y ajouter des chaînes et d’autres types de ressources. Ensuite, nous allons générer et vider un fichier PRI.|

## <a name="important-apis"></a>API importantes
* [Référence PRI (package Resource indexer)](/windows/desktop/menurc/pri-indexing-reference)

## <a name="related-topics"></a>Rubriques connexes
* [Compiler des ressources manuellement avec MakePri.exe](makepri-exe-command-options.md)
* [Consommation de fonctions DLL non managées](/dotnet/framework/interop/consuming-unmanaged-dll-functions?branch=live)
* [Système de gestion des ressources](resource-management-system.md)
