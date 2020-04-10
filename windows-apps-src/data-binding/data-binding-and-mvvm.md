---
ms.assetid: F46306EC-DFF3-4FF0-91A8-826C1F8C4A52
title: Liaison de données et MVVM
description: La liaison de données est au cœur du modèle de conception architecturale de l’interface utilisateur MVVM (Model-View-ViewModel) et permet un couplage faible entre l’interface utilisateur et le code non associé à l’interface utilisateur.
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 931f2fcbcdbf58b9dc2ca40403d7466b620a8991
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63798104"
---
# <a name="data-binding-and-mvvm"></a>Liaison de données et MVVM

Le modèle MVVM (Model-View-ViewModel) est un modèle de conception architecturale d’interface utilisateur permettant de découpler l’interface utilisateur et le code qui ne lui est pas associé. Avec MVVM, vous définissez votre interface utilisateur de façon déclarative en XAML et vous utilisez le balisage de liaison de données pour la lier à d’autres couches contenant des données et des commandes. L’infrastructure de liaison de données fournit un couplage faible qui maintient l’interface utilisateur et les données liées synchronisées, et route l’entrée utilisateur vers les commandes appropriées. 

Étant donné qu’elle fournit un couplage faible, l’utilisation de la liaison de données réduit les dépendances dures entre les différents types de code. Cela facilite le changement d’unités de code individuelles (méthodes, classes, contrôles, etc.) sans entraîner d’effets secondaires inattendus dans d’autres unités. Ce découplage est un exemple de la *séparation des problèmes*, qui est un concept important dans de nombreux modèles de conception. 

## <a name="benefits-of-mvvm"></a>Avantages du modèle MVVM

Le découplage de votre code présente de nombreux avantages, notamment :

* Activation d’un style de codage itératif et exploratoire. Un changement isolée est moins risqué et plus facile à expérimenter.
* Simplification des tests unitaires. Les unités de code qui sont isolées les unes des autres peuvent être testées individuellement et hors des environnements de production.
* Prise en charge de la collaboration en équipe. Le code découplé qui est conforme à des interfaces bien conçues peut être développé par des personnes ou des équipes distinctes, puis intégré ultérieurement.
* Amélioration de la maintenabilité. La résolution de bogues dans du code découplé est moins susceptible d’entraîner des régressions dans un autre code.

Contrairement à MVVM, une application avec une structure « code-behind » plus conventionnelle utilise généralement la liaison de données pour les données en affichage seul et répond à l’entrée utilisateur en gérant directement les événements exposés par des contrôles. Les gestionnaires d’événements sont implémentés dans des fichiers code-behind (comme MainPage.xaml.cs) et sont souvent fortement couplés avec les contrôles, contenant généralement du code qui manipule directement l’interface utilisateur. Il est donc difficile, voire impossible, de remplacer un contrôle sans avoir à mettre à jour le code de gestion des événements. Avec cette architecture, les fichiers code-behind accumulent souvent du code qui n’est pas directement lié à l’interface utilisateur, comme le code d’accès aux bases de données, et qui finit par être dupliqué et modifié pour être utilisé avec d’autres pages.

## <a name="app-layers"></a>Couches application

Quand vous utilisez le modèle MVVM, une application comprend les différentes couches suivantes :

* La couche de **modèle** définit les types qui représentent vos données métier. Cela inclut tout ce qui est nécessaire pour modéliser le domaine d’application principal, et souvent la logique d’application principale. Cette couche est totalement indépendante des couches de vue et de modèle de vue, et réside souvent partiellement dans le cloud. Avec une couche de modèle totalement implémentée, vous pouvez créer plusieurs applications clientes différentes si vous le souhaitez, par exemple des applications UWP et web qui fonctionnent avec les mêmes données sous-jacentes.
* La couche de **vue** définit l’interface utilisateur à l’aide du balisage XAML. Le balisage comprend des expressions de liaison de données (comme [x :Bind](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)) qui définissent la connexion entre des composants d’interface utilisateur spécifiques et divers membres de modèle et de modèle de vue. Les fichiers code-behind sont parfois utilisés dans le cadre de la couche de vue pour contenir du code supplémentaire nécessaire à la personnalisation ou à la manipulation de l’interface utilisateur, ou pour extraire des données d’arguments du gestionnaire d’événements avant d’appeler une méthode de modèle de vue qui effectue le travail. 
* La couche de **modèle de vue** fournit des cibles de liaison de données pour la vue. Dans de nombreux cas, le modèle de vue expose directement le modèle ou fournit des membres qui wrappent des membres de modèle spécifiques. Le modèle de vue peut également définir des membres pour effectuer le suivi de données relatives à l’interface utilisateur, mais pas au modèle, comme l’ordre d’affichage d’une liste d’éléments. Le modèle de vue sert également de point d’intégration à d’autres services comme le code d’accès aux bases de données. Pour les projets simples, vous n’aurez peut-être pas besoin d’une couche de modèle distincte, mais uniquement d’un modèle de vue qui encapsule toutes les données dont vous avez besoin. 

## <a name="basic-and-advanced-mvvm"></a>Modèle MMVM de base et avancé

Comme avec n’importe quel modèle de conception, il existe plusieurs façons d’implémenter MVVM, et de nombreuses techniques différentes sont considérées comme faisant partie de MVVM. C’est pourquoi il existe plusieurs frameworks MVVM tiers différents qui prennent en charge les diverses plateformes XAML, dont UWP. Toutefois, ces frameworks incluent généralement plusieurs services pour l’implémentation d’une architecture découplée, ce qui rend l’exacte définition de MVVM un peu ambiguë. 

Même si les frameworks MVVM sophistiqués peuvent être très utiles, en particulier pour les projets à l’échelle de l’entreprise, il existe généralement un coût associé à l’adoption d’un modèle ou d’une technique spécifique, et les avantages ne sont pas toujours clairs, en fonction de l’échelle et de la taille de votre projet. Heureusement, vous pouvez adopter uniquement les techniques qui offrent un avantage clair et tangible, et ignorer les autres tant que vous n’en avez pas besoin. 

En particulier, vous pouvez obtenir de nombreux avantages en comprenant et en appliquant toute la puissance de la liaison de données et en divisant la logique de votre application selon les couches décrites précédemment. Pour ce faire, vous pouvez utiliser uniquement les fonctionnalités fournies par le SDK Windows et sans utiliser de frameworks externes. En particulier, l’[extension de balisage {x :Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) rend la liaison de données plus facile et plus performante que dans les plateformes XAML précédentes, ce qui supprime le besoin de disposer d’une grande quantité du code réutilisable qui était auparavant nécessaire.

Pour obtenir des instructions supplémentaires sur l’utilisation du modèle MVVM de base prêt à l’emploi, consultez l’[exemple de base de données Customers Orders](https://github.com/Microsoft/Windows-appsample-customers-orders-database) sur GitHub. La plupart des autres [exemples d’application UWP](https://github.com/Microsoft?q=windows-appsample
) utilisent également une architecture MVVM de base, et l’[exemple d’application de trafic ](https://github.com/Microsoft/Windows-appsample-trafficapp) inclut à la fois les versions code-behind et MVVM, avec des notes décrivant la [conversion MVVM](https://github.com/Microsoft/Windows-appsample-trafficapp/blob/MVVM/MVVM.md). 

## <a name="see-also"></a>Voir aussi

### <a name="topics"></a>Rubriques

[Présentation détaillée de la liaison de données](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)  
[Extension de balisage {x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)  

### <a name="samples"></a>exemples

[Exemple de base de données Customers Orders](https://github.com/Microsoft/Windows-appsample-customers-orders-database)  
[Exemple de l’inventaire VanArsdel](https://github.com/Microsoft/InventorySample)  
[Exemple d’application de trafic](https://github.com/Microsoft/Windows-appsample-trafficapp)  
