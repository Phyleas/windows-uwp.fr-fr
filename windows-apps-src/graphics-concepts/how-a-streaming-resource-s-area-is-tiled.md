---
title: Restitution de la surface d’une ressource de diffusion en continu sous forme de mosaïque
description: Lorsque vous créez une ressource de streaming, les dimensions, la taille de l’élément de format et le nombre de tranches de des mipmaps et/ou de tableau (le cas échéant) déterminent le nombre de vignettes nécessaires pour sauvegarder la surface d’exposition entière.
ms.assetid: 3485FD8D-2A06-4B0A-8810-ECF37736F94B
keywords:
- Restitution de la surface d’une ressource de diffusion en continu sous forme de mosaïque
- zone de ressources, en mosaïque
- dimensionner les tables, les ressources, les mosaïques
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fae56f673aa3d952b7e85490ec79676c2ac6a48a
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220342"
---
# <a name="how-a-streaming-resources-area-is-tiled"></a>Restitution de la surface d’une ressource de diffusion en continu sous forme de mosaïque


Lorsque vous créez une ressource de streaming, les dimensions, la taille de l’élément de format et le nombre de tranches de des mipmaps et/ou de tableau (le cas échéant) déterminent le nombre de vignettes nécessaires pour sauvegarder la surface d’exposition entière. La disposition pixel/octet dans les mosaïques est déterminée par l’implémentation de. Le nombre de pixels qui tiennent dans une vignette, selon la taille de l’élément de format, est fixe et identique, que vous utilisiez ou non un Swizzle standard.

Le nombre de vignettes qui seront utilisées par une taille de surface donnée et une largeur d’élément de format est bien défini et prévisible en fonction des tables des sections suivantes. Pour les ressources qui contiennent des des mipmaps ou des cas où les dimensions de surface ne remplissent pas de vignette, certaines contraintes existent ; consultez [compression mipmap](mipmap-packing.md).

Différentes ressources de streaming peuvent pointer vers une mémoire identique avec des formats différents tant que les applications ne reposent pas sur les résultats de l’écriture dans la mémoire avec un format et en lisant avec une autre. Toutefois, les applications peuvent reposer sur les résultats de l’écriture dans la mémoire avec un format et la lecture avec un autre si les formats sont dans la même famille de format (autrement dit, ils ont le même format parent sans type). Par exemple, les formats DXGI \_ \_ R8G8B8A8 \_ UNORM et dxgi \_ format \_ R8G8B8A8 \_ uint sont compatibles les uns avec les autres, mais pas avec le \_ format dxgi \_ \_ R16G16 UNORM.

Une exception est l’endroit où les données de saignée d’un alias de format vers un autre sont bien définies : si une vignette contient entièrement 0 pour tous ses bits, cette vignette peut être utilisée avec n’importe quel format qui interprète le contenu de la mémoire comme étant égal à 0 (quelle que soit la disposition de la mémoire). Par conséquent, une vignette peut être déplacée au format 0x00 au format DXGI \_ \_ R8 \_ UNORM, puis utilisée avec un format comme dxgi \_ \_ R32G32 \_ float et le contenu est toujours (0.0 f, 0.0 f).

La disposition des données dans une vignette ne dépend pas de l’endroit où la vignette est mappée dans une ressource globale. Par exemple, une vignette peut être réutilisée à différents emplacements d’une surface à la fois avec un comportement cohérent dans tous les emplacements.

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
<td align="left"><p><a href="texture2d-and-texture2darray-subresource-tiling.md">Restitution des sous-ressources Texture2D et Texture2DArray sous forme de mosaïque</a></p></td>
<td align="left"><p>Ces tableaux montrent comment les sous-ressources <a href="/windows/desktop/direct3dhlsl/sm5-object-texture2d"><strong>Texture2D</strong></a> et <a href="/windows/desktop/direct3dhlsl/sm5-object-texture2darray"><strong>Texture2DArray</strong></a> sont présentées en mosaïque.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture3d-subresource-tiling.md">Restitution de la sous-ressource Texture3D sous forme de mosaïque</a></p></td>
<td align="left"><p>Ce tableau montre comment les sous-ressources <a href="/windows/desktop/direct3dhlsl/sm5-object-texture3d"><strong>Texture3D</strong></a> sont présentées en mosaïque.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="buffer-tiling.md">Mosaïque de mémoires tampons</a></p></td>
<td align="left"><p>Une ressource de <a href="introduction-to-buffers.md">mémoire tampon</a> est divisée en mosaïques de 64 Ko, avec un espace vide dans la dernière vignette si la taille n’est pas un multiple de 64 Ko.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="mipmap-packing.md">Compression de mipmaps</a></p></td>
<td align="left"><p>Un certain nombre de MIPS (par tranche de groupe) peuvent être empaquetés dans un certain nombre de vignettes, selon les dimensions, le format, le nombre de des mipmaps et les tranches de tableau de la ressource de streaming.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Création de ressources de diffusion en continu](creating-streaming-resources.md)

 

 