---
description: Vue d’ensemble des composants MRT Core et de leur fonctionnement pour le chargement des ressources d’application (réunion de projet)
title: Introduction à MRT Core (réunion de projet)
ms.topic: article
ms.date: 12/11/2020
keywords: MRT, MRTCore, PRI, makepri, ressources, chargement des ressources
ms.author: hickeys
author: hickeys
ms.localizationpriority: medium
ms.openlocfilehash: 0039d2a585f850bd7a15afc619d3500c092de50b
ms.sourcegitcommit: cddc595969c658ce30fbc94ded92db4a8ad1bf66
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2020
ms.locfileid: "97349377"
---
# <a name="introduction-to-mrt-core-project-reunion"></a>Introduction à MRT Core (réunion de projet)

MRT Core est une version simplifiée du [système de gestion des ressources](/windows/uwp/app-resources/resource-management-system) Windows moderne qui est distribué dans le cadre de la réunion de [projet](../index.md).

MRT Core possède des fonctionnalités au moment de la génération et au moment de l’exécution. Au moment de la génération, le système crée un index de toutes les différentes variantes des ressources regroupées avec votre application. Cet index est l’index de ressource du package, ou PRI, et il est également inclus dans le package de votre application. Au moment de l’exécution, le système détecte les paramètres de l’utilisateur et de l’ordinateur qui sont en vigueur, consulte les informations dans le PRI et charge automatiquement les ressources qui correspondent le mieux à ces paramètres.

## <a name="package-resource-index-pri-file"></a>Fichier d’index de ressources de package (PRI)

Chaque package d’application doit contenir un index binaire des ressources de l’application. Cet index est créé au moment de la génération et est contenu dans un ou plusieurs fichiers de ressources (. resw).

Un fichier resw contient des ressources de type chaîne réelles et un ensemble indexé de chemins d’accès de fichiers qui font référence à différents fichiers dans le package.
Un package contient généralement un fichier resw unique par langue, nommé Resources. resw. Le fichier Resources. resw à la racine de chaque package est chargé automatiquement lors de l’instanciation du ResourceManager.

Chaque fichier resw contient une collection nommée de ressources, appelée mappage des ressources. Lorsqu’un fichier resw d’un package est chargé, le nom du mappage de ressources est vérifié pour correspondre au nom de l’identité du package.

Les fichiers Resw contiennent uniquement des données, de sorte qu’ils n’utilisent pas le format PE (Portable Executable). Ils sont spécifiquement conçus pour être uniquement des données.

## <a name="using-mrt-core-to-access-app-resources"></a>Utilisation du MRT Core pour accéder aux ressources de l’application

### <a name="resource-loader-basic-functionality"></a>Chargeur de ressources (fonctionnalité de base)

La façon la plus simple d’accéder à vos ressources d’application par programmation consiste à utiliser l’espace de noms [Microsoft. ApplicationModel. Resources](/windows/winui/api/microsoft.applicationmodel.resources) et la classe ResourceLoader. ResourceLoader fournit un accès de base aux ressources de type chaîne à partir de l’ensemble de fichiers de ressources, de bibliothèques référencées ou d’autres packages.

### <a name="resource-manager-advanced-functionality"></a>Gestionnaire des ressources (fonctionnalités avancées)

La classe [ResourceManager](/windows/winui/api/microsoft.applicationmodel.resources.resourcemanager) fournit des informations supplémentaires sur les ressources, telles que l’énumération et l’inspection. Cela va au-delà de ce que fournit la classe **ResourceLoader** .

Un objet [ResourceCandidate](/windows/winui/api/microsoft.applicationmodel.resources.resourcecandidate) représente une seule valeur de ressource concrète et ses qualificateurs, tels que la chaîne « Hello World » pour l’anglais, ou « logo.scale-100.jpg » comme ressource d’image qualifiée spécifique à la résolution scale-100.

Les ressources disponibles pour une application sont stockées dans des collections hiérarchiques, auxquelles vous pouvez accéder à l’aide d’un objet [ResourceMap](/windows/winui/api/microsoft.applicationmodel.resources.resourcemap) . La classe **ResourceManager** donne accès aux différentes instances **ResourceMap** de niveau supérieur utilisées par l’application, qui correspondent aux différents packages de l’application. La valeur [ResourceManager. MainResourceMap](/windows/winui/api/microsoft.applicationmodel.resources.resourcemanager.mainresourcemap) correspond au mappage des ressources pour le package d’application actuel et exclut tous les packages d’infrastructure référencés. Chaque **ResourceMap** est nommé pour le nom du package qui est spécifié dans le manifeste du package. Dans un **ResourceMap** se trouvent des sous-arbres (voir [ResourceMap. GetSubtree]/Windows/WinUI/API/Microsoft.ApplicationModel.resources.ResourceMap.getsubtree)), qui contiennent davantage d’objets **NamedResource** . Les sous-arborescences correspondent généralement aux fichiers de ressources qui contiennent la ressource.

Notez que l’identificateur de ressource est traité comme un fragment de Uniform Resource Identifier (URI), en fonction de la sémantique de l’URI. Par exemple, `GetValue("Caption%20")` est traité comme `GetValue("Caption ")` . N’utilisez pas «  ? » ou « # » dans les identificateurs de ressource, car ils terminent l’évaluation du chemin d’accès à la ressource. Par exemple, « MyResource ? 3 » est considéré comme « MyResource ».

Le **ResourceManager** prend non seulement en charge l’accès aux ressources de type chaîne d’une application, mais également la possibilité d’énumérer et d’inspecter les diverses ressources de fichiers. Afin d’éviter les collisions entre les fichiers et d’autres ressources provenant d’un fichier, les chemins d’accès des fichiers indexés se trouvent tous dans un sous-arbre **ResourceMap** réservé « files ». Par exemple, le fichier « \Images\logo.png » correspond au nom de ressource « Files/images/logo.png ».

Les API [StorageFile](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) gèrent en toute transparence les références aux fichiers en tant que ressources, et sont appropriées pour les scénarios d’utilisation classiques. **ResourceManager** ne doit être utilisé que pour les scénarios avancés, par exemple lorsque vous souhaitez substituer le contexte actuel.

### <a name="resourcecontext"></a>ResourceContext

Les candidats aux ressources sont choisis en fonction d’un [ResourceContext](/windows/winui/api/microsoft.applicationmodel.resources.resourcecontext)particulier, qui est une collection de valeurs de qualificateurs de ressources (langue, échelle, contraste, etc.). Un contexte par défaut utilise la configuration actuelle de l’application pour chaque valeur de qualificateur, sauf si elle est remplacée. Par exemple, les ressources telles que les images peuvent être qualifiées pour la mise à l’échelle, ce qui varie d’un moniteur à l’autre et, par conséquent, d’une vue d’application à une autre. Pour cette raison, chaque vue d’application possède un contexte par défaut distinct. Le contexte par défaut pour une vue donnée peut être obtenu à l’aide de [ResourceContext. GetForCurrentView](/windows/winui/api/microsoft.applicationmodel.resources.resourcecontext). Chaque fois que vous récupérez un candidat à la ressource, vous devez transmettre une instance **ResourceContext** pour obtenir la valeur la plus appropriée pour une vue donnée.

### <a name="important-apis"></a>API importantes

- [ResourceLoader](/windows/winui/api/microsoft.applicationmodel.resources.resourceloader)
- [ResourceManager](/windows/winui/api/microsoft.applicationmodel.resources.resourcemanager)
- [ResourceContext](/windows/winui/api/microsoft.applicationmodel.resources.resourcecontext)

## <a name="sample"></a>Exemple

Pour obtenir un exemple qui montre comment utiliser l’API MRT Core, consultez l' [exemple MRT Core](https://github.com/microsoft/Project-Reunion-Samples/tree/main/MrtCore).
