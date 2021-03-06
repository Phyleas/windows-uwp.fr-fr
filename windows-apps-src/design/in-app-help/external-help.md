---
description: Concevez des pages d’aide externe pour obtenir des instructions détaillées et des conseils sur votre application.
title: Recommandations en matière de conception de pages d’aide externe
label: External help
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 56afd553-c520-4a28-b63d-2e1b3c1d3606
ms.localizationpriority: medium
ms.openlocfilehash: 315b384192cb8232560bfa09cdd572fa83a3a549
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030722"
---
# <a name="external-help-pages"></a>Pages d’aide externe



Si votre application nécessite une aide détaillée pour du contenu complexe, envisagez d’héberger ces instructions sur une page web.

## <a name="when-to-use-external-help-pages"></a>Quand utiliser des pages d’aide externe

Les pages d’aide externe sont moins adaptées à un usage général ou une référence rapide. Elles conviennent pour un contenu d’aide trop important pour être intégré à l’application elle-même, ainsi que pour des didacticiels et des instructions relatives aux fonctions avancées d’une application qui ne sont pas utilisés par le grand public.

Si votre contenu d’aide est bref ou suffisamment spécifique pour être affiché dans l’application, procédez ainsi. Ne dirigez pas les utilisateurs en dehors de l’application pour obtenir de l’aide à moins que cela ne soit nécessaire.

## <a name="navigating-external-help-pages"></a>Navigation dans les pages d’aide externe

Lorsqu’un utilisateur est dirigé vers une page d’aide externe, deux scénarios s’offrent à lui :
-   Les utilisateurs sont dirigés directement vers la page correspondant au problème connu. Il s’agit d’une aide contextuelle, qui doit être utilisée dans la mesure du possible.
-   Ils sont dirigés vers une page d’aide générale affichant clairement les catégories et sous-catégories à choisir.

Fournir aux utilisateurs un moyen d’effectuer des recherches dans votre aide, peut être utile, mais n’en faites pas le seul moyen de naviguer dans celle-ci. Il peut parfois être difficile pour les utilisateurs de décrire leur problème, ce qui complique la recherche. Les utilisateurs doivent être en mesure de trouver rapidement les pages correspondant à leurs problèmes sans avoir à effectuer de recherches.

## <a name="tutorials-and-detailed-walkthroughs"></a>Didacticiels et procédures pas à pas détaillées

Les pages d’aide externe sont l’endroit idéal pour proposer des didacticiels et des procédures pas à pas sous forme de texte ou de vidéo.
-   Les didacticiels doivent se limiter aux notions plus complexes et aux fonctions avancées. Les utilisateurs ne doivent pas avoir besoin d’un didacticiel pour utiliser votre application.
-   Veillez à ce que les utilisateurs puissent facilement distinguer les didacticiels des instructions d’aide standard par un affichage différent. Les utilisateurs à la recherche d’instructions avancées sont davantage enclins à effectuer des recherches que les utilisateurs voulant une solution facile à leur problème.
-   Envisagez la création de liens vers des didacticiels à la fois à partir d’un répertoire et de pages d’aide individuelles correspondant à chaque didacticiel.

## <a name="related-articles"></a>Articles connexes

* [Recommandations relatives à l’aide des applications](guidelines-for-app-help.md)
