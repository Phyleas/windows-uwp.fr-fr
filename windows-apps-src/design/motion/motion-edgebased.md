---
Description: Les animations latérales affichent ou masquent l’interface utilisateur qui part du bord de l’écran.
title: Animations latérales de l’interface utilisateur
ms.assetid: 5A8F73B1-F4F6-424b-9EDF-A9766C5DEAE8
label: Motion--edge-based UI
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b36f609308d559f5f0cbb56c90420f2381fb8e53
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220172"
---
# <a name="edge-based-ui-animations"></a>Animations latérales de l’interface utilisateur





Les animations latérales affichent ou masquent l’interface utilisateur qui part du bord de l’écran. L’utilisateur ou l’application peut initier les actions d’affichage ou de masquage. L’interface utilisateur peut se superposer à l’application ou intégrer sa surface principale. Si l’élément d’interface utilisateur fait partie de la surface principale, un redimensionnement du reste de l’application peut être nécessaire pour l’accueillir.

> **API importantes**: [ **classe EdgeUIThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.EdgeUIThemeTransition)


## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées


-   Utilisez les animations latérales de l’interface utilisateur pour afficher ou masquer une barre de messages ou une barre d’erreur qui n’occupe pas beaucoup d’espace à l’écran.
-   Faites appel aux animations de volets pour afficher un élément d’interface utilisateur qui glisse et occupe un espace suffisant sur l’écran, comme c’est le cas pour le volet de tâches ou un clavier personnalisé.
-   Faites apparaître l’élément d’interface en le faisant glisser à partir du bord auquel il doit venir s’attacher.
-   Faites-le disparaître en le faisant glisser vers le bord dont il provient.
-   Si le contenu de l’application doit être redimensionné à la suite du glissement de l’élément d’interface utilisateur, utilisez des animations en fondu pour le redimensionnement.
    -   Si l’élément d’interface utilisateur apparaît, utilisez l’animation en fondu après l’animation latérale de l’interface utilisateur ou des volets.
    -   Si l’élément d’interface utilisateur disparaît, utilisez l’animation en fondu en même temps que l’animation latérale de l’interface utilisateur ou des volets.
-   Ne mettez pas en application ces animations pour les notifications. Les notifications ne doivent pas être hébergées au sein d’une interface utilisateur latérale.
-   N’appliquez pas les animations latérales de l’interface utilisateur ou des volets à un conteneur ou contrôle d’interface utilisateur qui ne se trouve pas au bord de l’écran. Ces animations ne servent que pour l’affichage, le redimensionnement et la fermeture d’éléments d’interface utilisateur aux bords de l’écran. Pour déplacer d’autres types d’interface utilisateur, utilisez les animations de repositionnement.

    ![Indique à quel moment utiliser les animations latérales de l’interface utilisateur ou des volets et quand faire appel au repositionnement.](images/edgevsreposition.png)

## <a name="related-articles"></a>Articles connexes


**Pour les développeurs**
* [Vue d’ensemble des animations](./xaml-animation.md)
* [Animation latérale de l’interface utilisateur](/previous-versions/windows/apps/jj649428(v=win.10))
* [Démarrage rapide : Animation de votre interface utilisateur avec des animations de la bibliothèque](/previous-versions/windows/apps/hh452703(v=win.10))
* [**Classe EdgeUIThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.EdgeUIThemeTransition)
* [**Classe PaneThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.PaneThemeTransition)
* [Animation de fondus](/previous-versions/windows/apps/jj649429(v=win.10))
* [Animation de repositionnements](/previous-versions/windows/apps/jj649434(v=win.10))

 

 
