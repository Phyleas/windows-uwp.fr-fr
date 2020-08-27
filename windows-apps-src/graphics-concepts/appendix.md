---
title: Annexes
description: Affichez une table de liens vers des rubriques contenant des détails techniques sur les règles à virgule flottante, la conversion de type de données, les règles de pixellisation et la compression de bloc de texture.
ms.assetid: 461CCE6F-BAF0-4965-90A5-FD36B511F72C
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 41762bf24202d155ed7af288ff43ea898b43e46d
ms.sourcegitcommit: 8e0e4cac79554e86dc7f035c4b32cb1f229142b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88942769"
---
# <a name="appendices"></a>Annexes

Les sections suivantes fournissent des informations techniques détaillées.

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
<td align="left"><p><a href="floating-point-rules.md">Règles en matière de virgule flottante</a></p></td>
<td align="left"><p>Direct3D prend en charge plusieurs représentations à virgule flottante. Tous les calculs à virgule flottante fonctionnent sous un sous-ensemble défini des règles à virgule flottante simple précision IEEE 754 32 bits.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="data-type-conversion.md">Conversion de types de données</a></p></td>
<td align="left"><p>Les sections suivantes décrivent comment Direct3D gère les conversions entre les types de données.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="rasterization-rules.md">Règles de rastérisation</a></p></td>
<td align="left"><p>Les règles de pixellisation définissent la façon dont les données vectorielles sont mappées dans les données raster. Les données raster sont alignées sur les emplacements d’entiers qui sont ensuite éliminés et découpés (pour dessiner le nombre minimal de pixels), et les attributs par pixel sont interpolés (à partir des attributs par vertex) avant d’être passés à un nuanceur de pixels.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture-block-compression.md">Compression de blocs de texture</a></p></td>
<td align="left"><p>La prise en charge de la compression par bloc (BC) des textures a été étendue dans Direct3D 11 pour inclure les algorithmes BC6H et BC7. BC6H prend en charge les données sources de plage haute dynamique, et BC7 fournit une compression de qualité supérieure à la moyenne avec moins d’artefacts pour les données sources RGB standard.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Guide d’apprentissage graphique Direct3D](index.md)

 

 




