---
title: Niveaux de fonctionnalité des ressources de diffusion en continu
description: Accédez à des articles sur les trois niveaux de fonctionnalités de fonctionnalités pour les ressources de streaming Direct3D, précédemment appelées ressources en mosaïque.
ms.assetid: 6AE7EA72-3929-4BB4-8780-F0CF26192D87
keywords:
- Niveaux de fonctionnalité des ressources de diffusion en continu
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ee27244c4d4c2797b71c9d5c8c2c5185a99596b5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173053"
---
# <a name="streaming-resources-features-tiers"></a>Niveaux de fonctionnalité des ressources de diffusion en continu


Direct3D prend en charge les ressources de streaming dans trois niveaux de fonctionnalités.

Le niveau 1 fournit des fonctionnalités de base pour les ressources de streaming.

Le niveau 2 ajoute des fonctionnalités au-delà du niveau 1, telles que la garantie d’un mipmap de texture non condensé lorsque la taille correspond à au moins une forme de mosaïque standard ; instructions du nuanceur pour la fixation du niveau de détail (LOD) et pour obtenir l’état de l’opération de nuanceur ; en outre, la lecture des vignettes mappées sur NULL traite cette valeur échantillonnée comme étant égale à zéro.

Le niveau 3 ajoute des fonctionnalités Texture3D, au-delà de la couche 2.

Les fonctions de requête sont disponibles dans les versions de Direct3D, pour valider la prise en charge du matériel et des pilotes pour les ressources de streaming, et au niveau du niveau.

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
<td align="left"><p><a href="tier-1.md">Niveau 1</a></p></td>
<td align="left"><p>Cette section décrit la prise en charge de niveau 1.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="tier-2.md">Niveau 2</a></p></td>
<td align="left"><p>La prise en charge de niveau 2 pour les ressources de streaming ajoute des fonctionnalités au-delà du niveau 1, telles que la garantie d’un mipmap de texture non compactée lorsque la taille est d’au moins une forme de mosaïque standard. instructions du nuanceur pour la fixation du niveau de détail (LOD) et pour obtenir l’état de l’opération de nuanceur ; en outre, la lecture des vignettes mappées sur NULL traite cette valeur échantillonnée comme étant égale à zéro.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="tier-3.md">Niveau 3</a></p></td>
<td align="left"><p>Le niveau 3 ajoute la prise en charge de Texture3D pour les ressources de streaming, en plus des capacités de <a href="tier-2.md">niveau 2</a> .</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Ressources de diffusion en continu](streaming-resources.md)

 

 




