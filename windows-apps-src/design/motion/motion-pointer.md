---
Description: Utilisez les animations de pointeur pour fournir un retour visuel lorsqu’un utilisateur appuie sur un élément.
title: Animations de clic avec le pointeur
ms.assetid: EEB10A2C-629A-4705-8468-4D019D74DDFF
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d429f55b6c8004ea0b5e16f842f5c7ee492d754d
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91218846"
---
# <a name="pointer-click-animations"></a>Animations de clic avec le pointeur



Utilisez les animations de pointeur pour fournir un retour visuel lorsqu’un utilisateur appuie sur un élément. L’animation de pointeur vers le bas réduit et incline légèrement l’élément sélectionné. Elle est lue lorsque l’utilisateur appuie pour la première fois sur un élément. L’animation de pointeur vers le haut, qui restaure l’élément à sa position d’origine, est lue quand l’utilisateur relâche le pointeur.


> **API importantes**: [**classe PointerUpThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PointerUpThemeAnimation), [**classe PointerDownThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation)


## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées

-   Lorsque vous utilisez une animation de pointeur vers le haut, déclenchez immédiatement l’animation dès que l’utilisateur relâche le pointeur. Ceci permet de fournir un retour instantané à l’utilisateur, qui sait ainsi que son action a été reconnue, même si l’action déclenchée par l’appui (telle que la navigation vers une nouvelle page) est plus lente à se produire.

## <a name="related-articles"></a>Articles connexes

* [Vue d’ensemble des animations](./xaml-animation.md)
* [Animation de clics de pointeur](/previous-versions/windows/apps/jj649432(v=win.10))
* [Démarrage rapide : Animation de votre interface utilisateur avec des animations de la bibliothèque](/previous-versions/windows/apps/hh452703(v=win.10))
* [**Classe PointerUpThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PointerUpThemeAnimation)
* [**PointerDownThemeAnimation, classe**](/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation)

 

 
