---
Description: Utilisez les animations contextuelles pour afficher ou masquer l’interface utilisateur contextuelle de menus volants ou des éléments d’interface utilisateur contextuels personnalisés. Les éléments contextuels sont des conteneurs qui se superposent au contenu de l’application et qui disparaissent si l’utilisateur appuie ou clique en dehors de ces éléments contextuels.
title: Animations d’interface utilisateur contextuelle
ms.assetid: 4E9025CE-FC90-4d4c-9DE6-EC6B6F2AD9DF
label: Motion--Pop-up animations
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3d605378e802f28015734da4c35a22f41adfc185
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91217733"
---
# <a name="pop-up-ui-animations"></a>Animations d’interface utilisateur contextuelle



Utilisez les animations contextuelles pour afficher ou masquer l’interface utilisateur contextuelle de menus volants ou des éléments d’interface utilisateur contextuels personnalisés. Les éléments contextuels sont des conteneurs qui se superposent au contenu de l’application et qui disparaissent si l’utilisateur appuie ou clique en dehors de ces éléments contextuels.

> **API importantes**: [**classe PopInThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation), [**classe PopupThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition)


## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées


-   Utilisez les animations contextuelles pour afficher ou masquer des éléments d’interface utilisateur contextuels personnalisés qui ne font pas partie de la page de l’application proprement dite. Les contrôles courants fournis par Windows intègrent déjà ces animations.
-   N’utilisez pas d’animation contextuelle pour les info-bulles ou les boîtes de dialogue.
-   Ne recourez pas non plus aux animations contextuelles pour afficher ou masquer un élément d’interface utilisateur au sein du contenu principal de votre application ; n’utilisez des animations contextuelles que pour afficher ou masquer un conteneur contextuel apparaissant au-dessus du contenu principal de l’application.

## <a name="related-articles"></a>Articles connexes

* [Vue d’ensemble des animations](./xaml-animation.md)
* [Animation de l’interface utilisateur contextuelle](/previous-versions/windows/apps/jj649433(v=win.10))
* [Démarrage rapide : Animation de votre interface utilisateur avec des animations de la bibliothèque](/previous-versions/windows/apps/hh452703(v=win.10))
* [**Classe PopInThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation)
* [**Classe PopOutThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopOutThemeAnimation)
* [**Classe PopupThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition)

 

 
