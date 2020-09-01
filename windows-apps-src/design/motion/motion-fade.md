---
Description: Utilisez les animations en fondu pour faire apparaître ou disparaître des éléments. Les deux animations en fondu les plus courantes sont l’apparition en fondu et la disparition en fondu.
title: Afficher et masquer les animations en fondu
ms.assetid: 975E5EE3-EFBE-4159-8D10-3C94143DD07F
label: Motion--fades
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2b33cee13e37f55fd69229c6e41311a41806c987
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172413"
---
# <a name="fade-animations"></a>Afficher et masquer les animations en fondu



Utilisez les animations en fondu pour faire apparaître ou disparaître des éléments. Les deux animations en fondu les plus courantes sont l’apparition en fondu et la disparition en fondu.

> **API importantes**: [**classe FadeInThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation), [**classe FadeOutThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation)


## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées


-   Lorsque votre application effectue la transition entre des éléments non liés ou riches en texte, utilisez une disparition en fondu suivie d’une apparition en fondu. L’objet sortant disparaît ainsi entièrement avant que l’objet entrant ne soit visible.
-   Faites apparaître en fondu le ou les éléments entrants au-dessus des éléments sortants si leur taille reste constante et si vous voulez que l’utilisateur ait l’impression de voir le même élément. Une fois l’apparition en fondu terminée, vous pouvez simplement supprimer l’élément sortant. Cette option n’est viable que lorsque l’élément sortant est entièrement recouvert par l’élément entrant.
-   N’utilisez pas d’animations en fondu pour ajouter ou supprimer des éléments dans une liste. À la place, utilisez les animations de liste créées à cet effet.
-   Évitez d’utiliser des animations en fondu pour modifier l’intégralité du contenu d’une page. Utilisez plutôt les animations de transition entre les pages créées à cet effet.
-   Une disparition en fondu vous permet de supprimer un élément de façon subtile.
## <a name="related-articles"></a>Articles connexes

* [Vue d’ensemble des animations](./xaml-animation.md)
* [Animation de fondus](/previous-versions/windows/apps/jj649429(v=win.10))
* [Démarrage rapide : Animation de votre interface utilisateur avec des animations de la bibliothèque](/previous-versions/windows/apps/hh452703(v=win.10))
* [**Classe FadeInThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeInThemeAnimation)
* [**Classe FadeOutThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.FadeOutThemeAnimation)

 

 