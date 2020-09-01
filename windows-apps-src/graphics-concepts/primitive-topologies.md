---
title: Topologies de primitive
description: Direct3D prend en charge plusieurs topologies primitives, qui définissent la façon dont les vertex sont interprétés et restitués par le pipeline, tels que les listes de points, les listes de lignes et les bandes triangulaires.
ms.assetid: 7AA5A4A2-0B7C-431D-B597-684D58C02BA5
keywords:
- Topologies de primitive
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 45abb0c356b4ee6923bf6edd0b462f568749de5e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156333"
---
# <a name="primitive-topologies"></a>Topologies de primitive


Direct3D prend en charge plusieurs topologies primitives, qui définissent la façon dont les vertex sont interprétés et restitués par le pipeline, tels que les listes de points, les listes de lignes et les bandes triangulaires.

## <a name="span-idprimitive_typesspanspan-idprimitive_typesspanspan-idprimitive_typesspanbasic-primitive-topologies"></a><span id="Primitive_Types"></span><span id="primitive_types"></span><span id="PRIMITIVE_TYPES"></span>Topologies de base Primitives


Les topologies de base primitives (ou types primitifs) suivantes sont prises en charge :

-   [Listes de points](point-lists.md)
-   [Listes de lignes](line-lists.md)
-   [Bandes de lignes](line-strips.md)
-   [Listes de triangles](triangle-lists.md)
-   [Bandes de triangles](triangle-strips.md)

Pour obtenir une visualisation de chaque type primitif, consultez le diagramme plus loin dans cette rubrique, dans la direction de l' [enroulement et les positions des sommets de début](#winding-direction-and-leading-vertex-positions).

L' [étape de l’assembleur d’entrée](input-assembler-stage--ia-.md) lit les données à partir des tampons de vertex et d’index, assemble les données dans ces primitives, puis envoie les données aux étapes de pipeline restantes.

## <a name="span-idprimitive_adjacencyspanspan-idprimitive_adjacencyspanspan-idprimitive_adjacencyspanprimitive-adjacency"></a><span id="Primitive_Adjacency"></span><span id="primitive_adjacency"></span><span id="PRIMITIVE_ADJACENCY"></span>Contiguïté primitive


Tous les types primitifs Direct3D (à l’exception de la liste de points) sont disponibles dans deux versions : un type primitif avec contiguïté et un type primitif sans contiguïté. Les primitives avec contiguïté contiennent certains des vertex environnants, tandis que les primitives sans contiguïté contiennent uniquement les vertex de la primitive cible. Par exemple, la primitive de liste de lignes a une primitive de liste de lignes correspondante qui inclut l’adjacence.

Les primitives adjacentes sont destinées à fournir des informations supplémentaires sur votre géométrie et sont visibles uniquement par le biais d’un nuanceur Geometry. L’adjacence est utile pour les nuanceurs de géométrie qui utilisent la détection silhouette, l’extrusion du volume de clichés instantanés, etc.

Supposons, par exemple, que vous souhaitiez dessiner une liste de triangles avec contiguïté. Une liste de triangles contenant 36 sommets (avec contiguïté) produira 6 primitives terminées. Les primitives avec contiguïté (à l’exception des bandes de lignes) contiennent exactement deux fois plus de vertex que la primitive équivalente sans contiguïté, où chaque vertex supplémentaire est un vertex adjacent.

## <a name="span-idwinding_direction_and_leading_vertex_positionsspanspan-idwinding_direction_and_leading_vertex_positionsspanspan-idwinding_direction_and_leading_vertex_positionsspanspan-idwinding-direction-and-leading-vertex-positionsspanwinding-direction-and-leading-vertex-positions"></a><span id="Winding_Direction_and_Leading_Vertex_Positions"></span><span id="winding_direction_and_leading_vertex_positions"></span><span id="WINDING_DIRECTION_AND_LEADING_VERTEX_POSITIONS"></span><span id="winding-direction-and-leading-vertex-positions"></span>Direction du vent et positions des sommets de début


Comme indiqué dans l’illustration suivante, un sommet principal est le premier vertex non adjacent dans une primitive. Un type primitif peut avoir plusieurs sommets de début définis, tant que chacun d’eux est utilisé pour une primitive différente.

-   Pour une bande triangulaire avec contiguïté, les sommets de début sont 0, 2, 4, 6, et ainsi de suite.
-   Pour une ligne avec contiguïté, les sommets de début sont 1, 2, 3, et ainsi de suite.
-   En revanche, une primitive adjacente n’a pas de sommet principal.

L’illustration suivante montre l’ordonnancement des vertex pour tous les types primitifs que l’assembleur d’entrée peut produire.

![diagramme de l’ordonnancement des vertex pour les types primitifs](images/d3d10-primitive-topologies.png)

Les symboles de l’illustration précédente sont décrits dans le tableau suivant.

| Symbole                                                                                   | Nom              | Description                                                                         |
|------------------------------------------------------------------------------------------|-------------------|-------------------------------------------------------------------------------------|
| ![symbole pour un vertex](images/d3d10-primitive-topologies-vertex.png)                     | Sommet            | Point dans l’espace 3D.                                                                |
| ![symbole de direction de l’enroulement](images/d3d10-primitive-topologies-winding-direction.png) | Direction de l’enroulement | Ordre des vertex lors de l’assemblage d’une primitive. Peut être dans le sens inverse des aiguilles d’une montre. |
| ![symbole du sommet de début](images/d3d10-primitive-topologies-leading-vertex.png)       | Sommet principal    | Premier vertex non adjacent dans une primitive qui contient des données à constante.       |

 

## <a name="span-idgenerating_multiple_stripsspanspan-idgenerating_multiple_stripsspanspan-idgenerating_multiple_stripsspangenerating-multiple-strips"></a><span id="Generating_Multiple_Strips"></span><span id="generating_multiple_strips"></span><span id="GENERATING_MULTIPLE_STRIPS"></span>Génération de plusieurs bandes


Vous pouvez générer plusieurs bandes à l’aide de la coupe des bandes. Vous pouvez effectuer une coupure de bande en appelant explicitement la fonction [RestartStrip](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-so-restartstrip) HLSL ou en insérant une valeur d’index spéciale dans le tampon d’index. Cette valeur est-1, qui est 0xFFFFFFFF pour les index 32 bits ou 0xFFFF pour les index 16 bits.

Un index de-1 indique un « Cut » ou un « restart » explicite de la bande actuelle. L’index précédent termine la primitive ou la bande précédente, et l’index suivant démarre une nouvelle primitive ou une nouvelle bande.

Pour plus d’informations sur la génération de plusieurs bandes, consultez la section [Geometry Shader (GS) stage](geometry-shader-stage--gs-.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Étape de l’assembleur d’entrée (IA)](input-assembler-stage--ia-.md)

[Pipeline graphique](graphics-pipeline.md)

 

 