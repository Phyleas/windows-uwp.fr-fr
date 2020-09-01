---
title: Étape de l’assembleur d’entrée (IA)
description: L’assembleur d’entrée fournit des données de primitives et d’adjacence au pipeline, telles que des triangles, des lignes et des points, y compris des ID de sémantique pour faciliter l’efficacité des nuanceurs en réduisant le traitement des primitives qui n’ont pas encore été traitées.
ms.assetid: AF1DC611-C872-47F1-BF1A-92C68C8903E6
keywords:
- Étape de l’assembleur d’entrée (IA)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8b1ba0205a837383e1c646664c0550e055227412
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173063"
---
# <a name="input-assembler-ia-stage"></a>Étape de l’assembleur d’entrée (IA)


L’assembleur d’entrée fournit des données de primitives et d’adjacence au pipeline, telles que des triangles, des lignes et des points, y compris des ID de sémantique pour faciliter l’efficacité des nuanceurs en réduisant le traitement des primitives qui n’ont pas encore été traitées.

## <a name="span-idpurpose-and-usesspanspan-idpurpose-and-usesspanspan-idpurpose-and-usesspanpurpose-and-uses"></a><span id="Purpose-and-uses"></span><span id="purpose-and-uses"></span><span id="PURPOSE-AND-USES"></span>Usage et utilisations


L’objectif de l’étape de l’assembleur d’entrée est de lire des données primitives (points, lignes et triangles) à partir de mémoires tampons remplies par l’utilisateur et d’assembler les données dans des primitives qui seront utilisées par les autres étapes de pipeline, et de joindre des [valeurs générées](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics) par le système pour améliorer l’efficacité des nuanceurs. Les valeurs générées par le système sont des chaînes de texte qui sont également appelées sémantiques. Les étapes du nuanceur programmable sont construites à partir d’un noyau de nuanceur commun, qui utilise des valeurs générées par le système (comme un ID primitif, un ID d’instance ou un ID de vertex), afin que l’étape du nuanceur puisse réduire le traitement uniquement aux primitives, instances ou vertex qui n’ont pas encore été traités.

L’étape IA peut assembler des vertex dans plusieurs [types primitifs](primitive-topologies.md) différents (tels que des listes de lignes, des bandes triangulaires ou des primitives avec contiguïté). Les types primitifs tels qu’une liste de triangles avec contiguïté et une liste de lignes avec contiguïté prennent en charge l' [étape Geometry Shader (GS)](geometry-shader-stage--gs-.md).

Les informations d’contiguïté sont visibles pour une application uniquement dans un nuanceur Geometry. Si un nuanceur Geometry a été appelé avec un triangle incluant une contiguïté, par exemple, les données d’entrée contiennent 3 sommets pour chaque triangle et 3 sommets pour les données d’contiguïté par triangle.

Lorsque l’étape IA est demandée pour la sortie des données d’adjacence, les données d’entrée doivent inclure les données d’adjacence. Cela peut nécessiter la fourniture d’un vertex factice (formant un triangle dégenerate), ou peut-être en marquant l’un des attributs de vertex, que le vertex existe ou non. Elle doit également être détectée et gérée par un nuanceur Geometry, bien que l’élimination de la géométrie dégénérée se produise à l’étape de rastérisation.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entrée


L’étape IA lit les données à partir de la mémoire : les données primitives (points, lignes et/ou triangles), à partir de mémoires tampons remplies par l’utilisateur.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Sortie


L’étape IA assemble les données dans des primitives et attache des valeurs générées par le système, et génère des sorties en tant que primitives qui seront utilisées par l' [étape nuanceur de sommets (vs)](vertex-shader-stage--vs-.md) , puis d’autres étapes de pipeline.

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
<td align="left"><p><a href="primitive-topologies.md">Topologies de primitive</a></p></td>
<td align="left"><p>Direct3D prend en charge plusieurs topologies primitives, qui définissent la façon dont les vertex sont interprétés et restitués par le pipeline, tels que les listes de points, les listes de lignes et les bandes triangulaires.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="using-system-generated-values.md">Utilisation de valeurs générées par le système</a></p></td>
<td align="left"><p>Les valeurs générées par le système sont générées par l’étape de l’assembleur d’entrée (en fonction de la <a href="https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics">sémantique</a>d’entrée fournie par l’utilisateur) pour permettre une certaine efficacité dans les opérations de nuanceur. En joignant des données, telles qu’un ID d’instance (visible à l' <a href="vertex-shader-stage--vs-.md">étape de nuanceur de sommets (vs)</a>), un ID de vertex (visible pour vs) ou un ID primitif (visible pour la phase de nuanceur de pixels de <a href="geometry-shader-stage--gs-.md">géométrie (GS</a>) de l’étape de / <a href="pixel-shader-stage--ps-.md">Pixel</a>), une étape de nuanceur ultérieure peut rechercher ces valeurs système afin d’optimiser le traitement.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Pipeline graphique](graphics-pipeline.md)

 

 