---
description: Lors de la génération, le système de gestion des ressources crée un index de toutes les variantes de ressources qui sont incluses dans le package avec votre application. Lors de l’exécution, le système détecte les paramètres en vigueur pour l’utilisateur et l’ordinateur, et charge les ressources les plus appropriées pour ces paramètres.
title: Système de gestion des ressources
template: detail.hbs
ms.date: 10/20/2017
ms.topic: article
keywords: windows 10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: 083f49dd8a269bd3a0277084cadc175271d5e501
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031622"
---
# <a name="resource-management-system"></a>Système de gestion des ressources
Le système de gestion des ressources présente des fonctionnalités au moment de la génération et au moment de l’exécution. Au moment de la génération, le système crée un index de toutes les différentes variantes des ressources regroupées avec votre application. Cet index est l’index de ressource du package, ou PRI, et il est également inclus dans le package de votre application. Au moment de l’exécution, le système détecte les paramètres de l’utilisateur et de l’ordinateur qui sont en vigueur, consulte les informations dans le PRI et charge automatiquement les ressources qui correspondent le mieux à ces paramètres.

## <a name="package-resource-index-pri-file"></a>Fichier d’index de ressources de package (PRI)
Chaque package d’application doit contenir un index binaire des ressources de l’application. Cet index est créé au moment de la génération et il est contenu dans un ou plusieurs fichiers d’index de ressources de package (PRI).

- Un fichier PRI contient des ressources de chaîne réelles et un ensemble indexé de chemins d’accès de fichiers qui font référence à différents fichiers dans le package.
- Un package contient généralement un fichier PRI unique par langue, nommé Resources. pri.
- Le fichier Resources. PRI à la racine de chaque package est chargé automatiquement lors de l’instanciation du [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) .
- Les fichiers PRI peuvent être créés et vidés avec l’outil [MakePRI.exe](compile-resources-manually-with-makepri.md).
- Pour le développement d’applications standard, vous n’avez pas besoin de MakePRI.exe, car il est déjà intégré au flux de travail de compilation de Visual Studio. Et Visual Studio prennent en charge la modification des fichiers PRI dans une interface utilisateur dédiée. Toutefois, vos localisateurs et les outils qu’ils utilisent peuvent se baser sur MakePRI.exe.
- Chaque fichier PRI contient un ensemble nommé de ressources, que l'on qualifie de mappage des ressources. Lorsqu’un fichier PRI d’un package est chargé, le nom du mappage de ressources est vérifié pour correspondre au nom de l’identité du package.
- Les fichiers PRI contiennent uniquement des données, donc ils n’utilisent pas le format PE (Portable Executable). Ils sont spécialement conçus pour être des données uniquement comme format de ressource pour Windows. Elles remplacent les ressources contenues dans des dll dans le modèle d’application Win32.

## <a name="uwp-api-access-to-app-resources"></a>Accès de l’API UWP aux ressources de l’application

### <a name="basic-functionality-resourceloader"></a>Fonctionnalités de base (ResourceLoader)
La façon la plus simple d’accéder à vos ressources d’application par programmation consiste à utiliser l’espace de noms [**Windows. ApplicationModel. Resources**](/uwp/api/windows.applicationmodel.resources?branch=live) et la classe [**ResourceLoader**](/uwp/api/windows.applicationmodel.resources.resourceloader?branch=live) . **ResourceLoader** fournit un accès de base aux ressources de type chaîne à partir de l’ensemble de fichiers de ressources, de bibliothèques référencées ou d’autres packages.

### <a name="advanced-functionality-resourcemanager"></a>Fonctionnalités avancées (ResourceManager)
La classe [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) (dans l’espace de noms [**Windows. ApplicationModel. resources. Core**](/uwp/api/windows.applicationmodel.resources.core?branch=live) ) fournit des informations supplémentaires sur les ressources, telles que l’énumération et l’inspection. Cela va au-delà de ce que fournit la classe **ResourceLoader** .

Un objet [**NamedResource**](/uwp/api/windows.applicationmodel.resources.core.namedresource?branch=live) représente une ressource logique individuelle avec plusieurs langages ou d’autres variantes qualifiées. Il décrit la vue logique de l’élément multimédia ou de la ressource, avec un identificateur de ressource de chaîne, tel que `Header1` , ou un nom de fichier de ressources tel que `logo.jpg` .

Un objet [**ResourceCandidate**](/uwp/api/windows.applicationmodel.resources.core.resourcecandidate?branch=live) représente une seule valeur de ressource concrète et ses qualificateurs, tels que la chaîne « Hello World » pour l’anglais, ou « logo.scale-100.jpg » comme ressource d’image qualifiée spécifique à la résolution **Scale-100** .

Les ressources disponibles pour une application sont stockées dans des collections hiérarchiques, auxquelles vous pouvez accéder à l’aide d’un objet [**ResourceMap**](/uwp/api/windows.applicationmodel.resources.core.resourcemap?branch=live) . La classe **ResourceManager** donne accès aux différentes instances **ResourceMap** de niveau supérieur utilisées par l’application, qui correspondent aux différents packages de l’application. La valeur [**MainResourceMap**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager.MainResourceMap) correspond au mappage des ressources pour le package d’application actuel et exclut tous les packages d’infrastructure référencés. Chaque **ResourceMap** est nommé pour le nom du package qui est spécifié dans le manifeste du package. Dans un **ResourceMap** se trouvent des sous-arbres (voir [**ResourceMap. GetSubtree**](/uwp/api/windows.applicationmodel.resources.core.resourcemap.getsubtree?branch=live)), qui contiennent davantage d’objets **NamedResource** . Les sous-arborescences correspondent généralement aux fichiers de ressources qui contiennent la ressource. Pour plus d’informations, consultez [localiser des chaînes dans votre interface utilisateur et le manifeste du package d’application](localize-strings-ui-manifest.md) et charger des [images et des ressources adaptées à l’échelle, au thème, au contraste élevé et à d’autres](images-tailored-for-scale-theme-contrast.md).

Voici un exemple.

```csharp
// using Windows.ApplicationModel.Resources.Core;
ResourceMap resourceMap =  ResourceManager.Current.MainResourceMap.GetSubtree("Resources");
ResourceContext resourceContext = ResourceContext.GetForCurrentView()
var str = resourceMap.GetValue("String1", resourceContext).ValueAsString;
```

**Remarque** L’identificateur de ressource est traité comme un fragment de Uniform Resource Identifier (URI), en fonction de la sémantique de l’URI. Par exemple, `GetValue("Caption%20")` est traité comme `GetValue("Caption ")` . N’utilisez pas «  ? » ou « # » dans les identificateurs de ressource, car ils terminent l’évaluation du chemin d’accès à la ressource. Par exemple, « MyResource ? 3 » est considéré comme « MyResource ».

Le **ResourceManager** prend non seulement en charge l’accès aux ressources de type chaîne d’une application, mais également la possibilité d’énumérer et d’inspecter les diverses ressources de fichiers. Afin d’éviter les collisions entre les fichiers et d’autres ressources provenant d’un fichier, les chemins d’accès des fichiers indexés se trouvent tous dans un sous-arbre **ResourceMap** réservé « files ». Par exemple, le fichier `\Images\logo.png` correspond au nom de la ressource `Files/images/logo.png` .

Les API [**StorageFile**](/uwp/api/Windows.Storage.StorageFile?branch=live) gèrent en toute transparence les références aux fichiers en tant que ressources, et sont appropriées pour les scénarios d’utilisation classiques. **ResourceManager** ne doit être utilisé que pour les scénarios avancés, par exemple lorsque vous souhaitez substituer le contexte actuel.

### <a name="resourcecontext"></a>ResourceContext
Les candidats aux ressources sont choisis en fonction d’un [**ResourceContext**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext?branch=live)particulier, qui est une collection de valeurs de qualificateurs de ressources (langue, échelle, contraste, etc.). Un contexte par défaut utilise la configuration actuelle de l’application pour chaque valeur de qualificateur, sauf si elle est remplacée. Par exemple, les ressources telles que les images peuvent être qualifiées pour la mise à l’échelle, ce qui varie d’un moniteur à l’autre et, par conséquent, d’une vue d’application à une autre. Pour cette raison, chaque vue d’application possède un contexte par défaut distinct. Le contexte par défaut pour une vue donnée peut être obtenu à l’aide de [**ResourceContext. GetForCurrentView**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.GetForCurrentView). Chaque fois que vous récupérez un candidat à la ressource, vous devez transmettre une instance **ResourceContext** pour obtenir la valeur la plus appropriée pour une vue donnée.

## <a name="important-apis"></a>API importantes
* [ResourceLoader](/uwp/api/windows.applicationmodel.resources.resourceloader?branch=live)
* [ResourceManager](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live)
* [ResourceContext](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live)

## <a name="related-topics"></a>Rubriques connexes
* [Localiser les chaînes dans l’interface utilisateur et le manifeste du package d’application](localize-strings-ui-manifest.md)
* [Charger des images et des ressources adaptées pour la mise à l’échelle, le thème, le contraste élevé et autres](images-tailored-for-scale-theme-contrast.md)
