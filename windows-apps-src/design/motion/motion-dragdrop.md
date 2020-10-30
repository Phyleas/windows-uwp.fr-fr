---
description: Utilisez les animations de glisser-déplacer lors du déplacement d’objets par les utilisateurs, par exemple pour le déplacement d’un élément dans une liste ou le positionnement d’un élément au-dessus d’un autre.
title: Animations de glissement
ms.assetid: 6064755F-6E24-4901-A4FF-263F05F0DFD6
label: Motion--Drag and drop
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: acc193f0851e2f04113e33aa470e9aa784337921
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034282"
---
# <a name="drag-animations"></a>Animations de glissement




Utilisez les animations de glisser-déplacer lors du déplacement d’objets par les utilisateurs, par exemple pour le déplacement d’un élément dans une liste ou le positionnement d’un élément au-dessus d’un autre.

> **API importantes** : [ **classe DragItemThemeAnimation**](/uwp/api/windows.ui.xaml.media.animation.dragitemthemeanimation)


## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées


**Animation de début du glissement**

-   Utilisez l’animation de début du glissement quand l’utilisateur commence à déplacer un objet.
-   Insérez les objets affectés dans l’animation seulement si d’autres objets peuvent être affectés par l’opération de glisser-déplacer.
-   Utilisez l’animation de fin du glissement pour terminer toute séquence d’animation qui a commencé par l’animation de début du glissement. Le changement de taille provoqué par l’animation de début du glissement dans l’objet déplacé est alors inversé.

**Animation de fin du glissement**

-   Utilisez l’animation de fin du glissement quand l’utilisateur dépose un élément déplacé.
-   Utilisez l’animation de fin du glissement en combinaison avec les animations d’ajout et de suppression pour les listes.
-   Incluez les objets affectés dans l’animation de fin du glissement si et seulement si vous avez inclus ces mêmes objets affectés dans l’animation de début du glissement.
-   N’utilisez pas l’animation de fin du glissement si vous n’avez pas utilisé l’animation de début du glissement. Vous devez utiliser les deux animations pour que tous les objets retrouvent leur taille d’origine une fois la séquence de glissement terminée.

**Animation de glissement entre les objets**

-   Utilisez l’animation de glissement entre les objets quand l’utilisateur fait glisser la source du glissement dans une zone de dépôt où elle peut être placée entre deux objets.
-   Choisissez un emplacement où il est possible de déposer l’objet. Cet emplacement ne doit pas être trop petit, sinon l’utilisateur rencontrera des difficultés pour déposer la source du glissement.
-   La direction recommandée pour déplacer les objets affectés afin d’afficher la zone de dépôt est l’étirement direct les uns des autres. Qu’il s’agisse d’un mouvement vertical ou horizontal dépend de l’orientation des objets affectés les uns par rapport aux autres.
-   N’utilisez pas l’animation de glissement entre les objets, s’il n’est pas possible de déposer la source du glissement dans une zone. L’animation de glissement entre les objets indique à l’utilisateur que la source du glissement peut être déposée entre les objets affectés.

**Animation de glissement entre les objets par éloignement**

-   Utilisez l’animation de glissement entre les objets par éloignement quand l’utilisateur éloigne un objet d’une zone où il aurait pu être placé entre deux autres objets.
-   N’utilisez pas l’animation de glissement entre les objets par éloignement si vous n’avez pas d’abord utilisé l’animation de glissement entre les objets.


## <a name="related-articles"></a>Articles connexes

**Pour les développeurs**
* [Vue d’ensemble des animations](./xaml-animation.md)
* [Animation de séquences de glisser-déplacer](/previous-versions/windows/apps/jj649427(v=win.10))
* [Démarrage rapide : Animation de votre interface utilisateur avec des animations de la bibliothèque](/previous-versions/windows/apps/hh452703(v=win.10))
* [**Classe DragItemThemeAnimation**](/uwp/api/windows.ui.xaml.media.animation.dragitemthemeanimation)
* [**Classe DropTargetItemThemeAnimation**](/uwp/api/windows.ui.xaml.media.animation.droptargetitemthemeanimation)
* [**Classe DragOverThemeAnimation**](/uwp/api/windows.ui.xaml.media.animation.dragoverthemeanimation)


 
