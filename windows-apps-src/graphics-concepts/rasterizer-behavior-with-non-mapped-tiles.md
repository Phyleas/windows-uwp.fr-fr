---
title: Comportement du rastériseur avec les vignettes non mappées
description: En savoir plus sur les comportements de rastériseur DepthStencilView (DSV) et RenderTargetView (RTV) avec des vignettes non mappées.
ms.assetid: AC7B818D-E52B-4727-AEA2-39CFDC279CE7
keywords:
- Comportement du rastériseur avec les vignettes non mappées
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f8085d8d29a86c0c5da82f6cb98c57c037b81beb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171883"
---
# <a name="span-iddirect3dconceptsrasterizer_behavior_with_non-mapped_tilesspanrasterizer-behavior-with-non-mapped-tiles"></a><span id="direct3dconcepts.rasterizer_behavior_with_non-mapped_tiles"></span>Comportement du rastériseur avec les vignettes non mappées


Cette section décrit le comportement du rastériseur avec les vignettes non mappées.

## <a name="span-iddepthstencilviewspanspan-iddepthstencilviewspanspan-iddepthstencilviewspandepthstencilview"></a><span id="DepthStencilView"></span><span id="depthstencilview"></span><span id="DEPTHSTENCILVIEW"></span>DepthStencilView


Le comportement des lectures et écritures de vue stencil (DSV) dépend du niveau de support matériel. Pour une répartition des spécifications, consultez comportement global en lecture et en écriture des [niveaux de fonctionnalités des ressources de streaming](streaming-resources-features-tiers.md).

Voici le comportement idéal :

Si une vignette n’est pas mappée dans DepthStencilView, la valeur de retour de la profondeur de lecture est 0, qui est ensuite alimentée dans toutes les opérations configurées pour la valeur de lecture de profondeur. Les écritures dans la vignette de profondeur manquante sont supprimées. Cette définition idéale pour la gestion des écritures n’est pas requise par le [niveau 2](tier-2.md). les écritures dans des vignettes non mappées peuvent se trouver dans un cache que les lectures suivantes pourraient récupérer.

## <a name="span-idrendertargetviewspanspan-idrendertargetviewspanspan-idrendertargetviewspanrendertargetview"></a><span id="RenderTargetView"></span><span id="rendertargetview"></span><span id="RENDERTARGETVIEW"></span>RenderTargetView


Le comportement des lectures et des écritures de la vue de la cible de rendu dépend du niveau de support matériel. Pour une répartition des spécifications, consultez comportement global en lecture et en écriture des [niveaux de fonctionnalités des ressources de streaming](streaming-resources-features-tiers.md).

Sur toutes les implémentations, différentes limites de RTVs (et de DSV) peuvent avoir des zones mappées et non mappées différentes et peuvent avoir des formats de surface de taille différents (ce qui signifie des formes de mosaïques différentes).

Voici le comportement idéal :

Les lectures à partir de RTVs retournent 0 dans les vignettes et les écritures manquantes sont supprimées. Cette définition idéale pour la gestion des écritures n’est pas requise par le [niveau 2](tier-2.md). les écritures dans des vignettes non mappées peuvent se trouver dans un cache que les lectures suivantes pourraient récupérer.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Accès du pipeline aux ressources de diffusion en continu](pipeline-access-to-streaming-resources.md)

 

 




