---
title: Mappages dans un pool de vignettes
description: Lorsqu’une ressource est créée en tant que ressource de diffusion en continu, les vignettes qui composent la ressource proviennent du pointage sur les emplacements d’un pool de mosaïques. Un pool de mosaïques est un pool de mémoire (sauvegardé par une ou plusieurs allocations en arrière-plan, non visible par l’application).
ms.assetid: 58B8DBD5-62F5-4B94-8DD1-C7D57A812185
keywords:
- Mappages dans un pool de vignettes
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b021fc47eee6063761635422a780bb5f9f341f4d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171913"
---
# <a name="mappings-are-into-a-tile-pool"></a>Mappages dans un pool de vignettes


Lorsqu’une ressource est créée en tant que ressource de diffusion en continu, les vignettes qui composent la ressource proviennent du pointage sur les emplacements d’un pool de mosaïques. Un pool de mosaïques est un pool de mémoire (sauvegardé par une ou plusieurs allocations en arrière-plan, non visible par l’application). Le système d’exploitation et le pilote d’affichage gèrent ce pool de mémoire, et l’encombrement mémoire est facilement compréhensible par une application. Les ressources de streaming mappent des régions de 64 Ko en pointant sur des emplacements dans un pool de mosaïques. Un Fallout de cette installation permet à plusieurs ressources de partager et réutiliser les mêmes vignettes, et également de réutiliser les mêmes vignettes à différents emplacements au sein d’une ressource, si vous le souhaitez.

Le coût de la flexibilité de remplissage des vignettes pour une ressource à partir d’un pool de mosaïques est que la ressource doit effectuer la définition et la maintenance du mappage des vignettes dans le pool de mosaïques pour représenter les vignettes nécessaires pour la ressource. Les mappages de vignette peuvent être modifiés. En outre, toutes les vignettes d’une ressource n’ont pas besoin d’être mappées à la fois ; une ressource peut avoir des mappages **null** . Un mappage **null** définit une vignette comme n’étant pas disponible du point de vue de la ressource qui y accède.

Vous pouvez créer plusieurs pools de mosaïques, et n’importe quel nombre de ressources de streaming peut être mappées à un pool de mosaïques donné en même temps. Les pools de vignettes peuvent également être développés ou réduits. Pour plus d’informations, consultez [redimensionnement de pool de mosaïques](tile-pool-resizing.md). Une contrainte qui existe pour simplifier l’implémentation du pilote d’affichage et du runtime est qu’une ressource de diffusion en continu donnée ne peut avoir que des mappages dans un seul pool de mosaïques à la fois (par opposition à un mappage simultané à plusieurs pools de mosaïques).

La quantité de stockage associée à une ressource de streaming (autrement dit, la mémoire de pool de mosaïques indépendante) est à peu près proportionnelle au nombre de vignettes réellement mappées au pool à un moment donné. Dans le matériel, cela se résume à la mise à l’échelle de l’empreinte mémoire pour le stockage des tables de pages avec la quantité de vignettes mappées (par exemple, en utilisant un modèle de table de pages à plusieurs niveaux, le cas échéant).

Le pool de mosaïques peut être considéré comme une abstraction logicielle complète qui permet aux applications Direct3D de programmer efficacement les tables de pages sur l’unité de traitement graphique (GPU) sans avoir à connaître les détails d’implémentation de bas niveau (ou gérer directement les adresses de pointeur). Les pools de mosaïques n’appliquent pas d’autres niveaux d’indirection dans le matériel. Les optimisations d’une table de page de niveau unique à l’aide de constructions telles que des répertoires de pages sont indépendantes du concept de pool de mosaïques.

Examinons le stockage que la table de page lui-même peut nécessiter dans le pire des cas (Toutefois, dans les implémentations de la pratique, vous n’avez besoin que d’un stockage à peu près proportionnel à ce qui est mappé).

Supposons que chaque entrée de table de pages soit 64 bits.

Pour l’accès à la taille de la table de pages le plus défavorable pour une seule surface, étant donné les limites de ressources dans Direct3D 11, supposons qu’une ressource de streaming est créée avec un format de bit par élément de 128 (par exemple, un nombre de pixels en virgule flottante), de sorte qu’une vignette de 64 Ko ne contient que 4096 pixels. La taille de [**Texture2DArray**](/windows/desktop/direct3dhlsl/sm5-object-texture2darray) maximale prise en charge de 16384 \* 16384 \* 2048 (mais avec un seul mipmap) nécessite environ 1 Go de stockage dans la table de page en cas de remplissage complet (à l’exception de des mipmaps) à l’aide des entrées de table de bits 64. L’ajout de des mipmaps augmenterait le stockage de table de pages entièrement mappé (cas le plus défavorable) d’une troisième, à environ 1,3 Go.

Ce cas donne accès à environ 10,6 téraoctets de mémoire adressable. Toutefois, il peut y avoir une limite sur la quantité de mémoire adressable, ce qui réduirait éventuellement les limites de la plage de téraoctets.

Un autre cas à prendre en compte est une ressource de streaming [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) unique de 16384 \* 16384 avec un format binaire par élément de 32, y compris des mipmaps. L’espace nécessaire dans une table de pages entièrement remplie serait 170KB à peu près avec des entrées de table de bits 64.

Enfin, prenons un exemple utilisant un format BC, par exemple BC7 avec 128 bits par mosaïque de 4 x 4. Il s’agit d’un octet par pixel. Un [**Texture2DArray**](/windows/desktop/direct3dhlsl/sm5-object-texture2darray) de 16384 \* 16384 \* 2048, y compris des MIPMAPS, nécessite environ 85MB pour remplir entièrement cette mémoire dans une table de pages. Ce n’est pas une mauvaise considération. cela permet à une ressource de streaming de s’étendre sur 550 gigapixels (512 Go de mémoire dans ce cas).

Dans la pratique, rien près de ces mappages complets serait défini, étant donné que la quantité de mémoire physique disponible ne permettrait pas à quelque point près qu’il soit possible de le mapper et de le référencer à la fois. Toutefois, avec un pool de mosaïques, les applications peuvent choisir de réutiliser les vignettes (en guise d’exemple simple, en réutilisant efficacement une vignette de couleur noire pour les grandes régions noires dans une image), en utilisant efficacement le pool de mosaïques (autrement dit, les mappages de tables de pages) comme un outil pour la compression de mémoire.

Le contenu initial de la table de page a la **valeur null** pour toutes les entrées. Les applications ne peuvent pas non plus transmettre des données initiales pour le contenu de la mémoire de l’aire, car elles démarrent sans stockage de mémoire.

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
<td align="left"><p><a href="tile-pool-creation.md">Création d’un pool de vignettes</a></p></td>
<td align="left"><p>Les applications peuvent créer un ou plusieurs pools de mosaïques par appareil Direct3D. La taille totale de chaque pool de mosaïques est limitée à la limite de taille des ressources de Direct3D 11, soit environ 1/4 de RAM GPU.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="tile-pool-resizing.md">Redimensionnement d’un pool de vignettes</a></p></td>
<td align="left"><p>Redimensionner un pool de mosaïques pour augmenter un pool de mosaïques si l’application a besoin d’une plus grande plage de travail pour le mappage des ressources de streaming dans celle-ci, ou pour réduire l’espace nécessaire.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="hazard-tracking-versus-tile-pool-resources.md">Suivi des risques ou ressources d’un pool de vignettes</a></p></td>
<td align="left"><p>Pour les ressources qui ne sont pas diffusées en continu, Direct3D peut empêcher certaines conditions de danger pendant le rendu, mais comme le suivi des risques se trouve au niveau de la mosaïque pour les ressources de streaming, le suivi des conditions de danger lors du rendu des ressources de streaming peut être trop onéreux.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Création de ressources de diffusion en continu](creating-streaming-resources.md)

 

 