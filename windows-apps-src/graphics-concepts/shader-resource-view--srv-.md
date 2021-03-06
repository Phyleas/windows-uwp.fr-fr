---
title: Affichage des ressources de nuanceur (SRV) et des accès sans ordre (UAV)
description: Les vues de ressource de nuanceur habillent en général des textures sous un format accessible aux nuanceurs. Un affichage des accès sans ordre offre une fonctionnalité similaire, mais permet la lecture et l'écriture sur la texture (ou sur d’autres ressources) dans n’importe quel ordre.
ms.assetid: 4505BCD2-0EDA-40F2-887C-EC081FE32E8F
keywords:
- Affichage des ressources de nuanceur (SRV)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: bd6cca08c0845e2da5420fc1f040cc285c87953d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636044"
---
# <a name="shader-resource-view-srv-and-unordered-access-view-uav"></a>Affichage des ressources de nuanceur (SRV) et des accès sans ordre (UAV)


Les vues de ressource de nuanceur habillent en général des textures sous un format accessible aux nuanceurs. Un affichage des accès sans ordre offre une fonctionnalité similaire, mais permet la lecture et l'écriture sur la texture (ou sur d’autres ressources) dans n’importe quel ordre.

L'habillage d'une texture est probablement la forme la plus simple de vue de ressource de nuanceur. Comme exemple plus complexes, on peut évoquer une collection de sous-ressources (tableaux individuels, plans ou couleurs à partir d’une texture mipmappée), de textures 3D, de dégradés de couleur de texture 1D, etc.

Les affichages des accès sans ordre (UAV) sont légèrement plus coûteux en termes de performances, mais permettent par exemple d'écrire une texture en même temps qu'elle est lue. Cela permet à la texture mise à jour d'être réutilisée à d'autres fins par le pipeline graphique. Les vues de ressource de nuanceur sont destinées à une utilisation en lecture seule (qui correspond à l’utilisation la plus fréquente des ressources).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Vues](views.md)

 

 




