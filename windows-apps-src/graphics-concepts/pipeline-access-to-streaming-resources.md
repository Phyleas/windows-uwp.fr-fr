---
title: Accès du pipeline aux ressources de diffusion en continu
description: Les ressources de streaming peuvent être utilisées dans les vues de ressource de nuanceur (SRV), les vues de cible de rendu (RTV), les vues de stencil de profondeur (DSV) et les vues d’accès non ordonnées (UAV), ainsi que certains points de liaison où les vues ne sont pas utilisées, telles que les liaisons de mémoire tampon de vertex.
ms.assetid: 18DA5D61-930D-4466-8EFE-0CED566EA4A6
keywords:
- Accès du pipeline aux ressources de diffusion en continu
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 15b37e471e45a1c2ca604c1a5bf28ace69e35ad3
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220322"
---
# <a name="pipeline-access-to-streaming-resources"></a>Accès du pipeline aux ressources de diffusion en continu


Les ressources de streaming peuvent être utilisées dans les vues de ressource de nuanceur (SRV), les vues de cible de rendu (RTV), les vues de stencil de profondeur (DSV) et les vues d’accès non ordonnées (UAV), ainsi que certains points de liaison où les vues ne sont pas utilisées, telles que les liaisons de mémoire tampon de vertex. Pour obtenir la liste des liaisons prises en charge, consultez [paramètres de création de ressources de streaming](streaming-resource-creation-parameters.md). Les différentes opérations de copie D3D fonctionnent également sur les ressources de streaming.

Si plusieurs coordonnées de mosaïque dans une ou plusieurs vues sont liées au même emplacement de mémoire, les lectures et écritures à partir de différents chemins d’accès à la même mémoire se produisent dans un ordre non déterministe et non reproductible d’accès à la mémoire.

Si toutes les vignettes derrière un encombrement d’accès à la mémoire à partir d’un nuanceur sont mappées à des vignettes uniques, le comportement est identique sur toutes les implémentations à la surface ayant le même contenu de mémoire en mode non en mosaïque.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>Dans cette section


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Rubrique</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="srv-behavior-with-non-mapped-tiles.md">Comportement du SRV avec les vignettes non mappées</a></p></td>
<td align="left"><p>Le comportement des lectures de la vue de ressource de nuanceur (SRV) qui impliquent des vignettes non mappées dépend du niveau de support matériel.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="uav-behavior-with-non-mapped-tiles.md">Comportement de l’UAV avec les vignettes non mappées</a></p></td>
<td align="left"><p>Le comportement des lectures et écritures de la vue d’accès non triée (UAV) dépend du niveau de support matériel.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="rasterizer-behavior-with-non-mapped-tiles.md">Comportement du rastériseur avec les vignettes non mappées</a></p></td>
<td align="left"><p>Cette section décrit le comportement du rastériseur avec les vignettes non mappées.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="tile-access-limitations-with-duplicate-mappings.md">Restrictions d’accès aux vignettes avec des mappages en double</a></p></td>
<td align="left"><p>Il existe des limitations en matière d’accès aux vignettes avec des mappages dupliqués, par exemple lors de la copie de ressources de streaming avec source et destination qui se chevauchent, ou lors du rendu de vignettes partagées dans la zone de rendu.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="streaming-resources-texture-sampling-features.md">Fonctionnalités d’échantillonnage de texture des ressources de diffusion en continu</a></p></td>
<td align="left"><p>Ressources de streaming les fonctionnalités d’échantillonnage de texture incluent l’obtention de commentaires sur l’état du nuanceur sur les zones mappées, la vérification de la mise en correspondance de toutes les données qui ont été consultées dans la ressource, le verrouillage pour aider les nuanceurs à éviter les zones dans les ressources de streaming mipmapped qui ne sont pas mappées, et la découverte de ce que le LOD minimal est</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="hlsl-streaming-resources-exposure.md">Exposition des ressources de diffusion en continu HLSL</a></p></td>
<td align="left"><p>Une syntaxe HLSL (High Level Shader Language) spécifique est requise pour prendre en charge les ressources de streaming dans le <a href="/windows/desktop/direct3dhlsl/d3d11-graphics-reference-sm5">nuancier Model 5</a>.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Ressources de diffusion en continu](streaming-resources.md)

 

 