---
title: Étape Geometry Shader (GS)
description: L’étape de nuanceur Geometry (GS) traite les triangles, les lignes et les points de la totalité des primitives, ainsi que leurs vertex adjacents.
ms.assetid: 8A1350DD-B006-488F-9DAF-14CD2483BA4E
keywords:
- Étape Geometry Shader (GS)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 61d794e93718bc9450c1a0f3dce2e921da7e513d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168073"
---
# <a name="geometry-shader-gs-stage"></a>Étape Geometry Shader (GS)


L’étape de nuanceur Geometry (GS) traite les primitives entières : les triangles, les lignes et les points, ainsi que leurs vertex adjacents. Elle est utile pour les algorithmes, y compris l’expansion point Sprite, les systèmes de particule dynamiques et la génération du volume de cliché instantané. Il prend en charge l’amplification et la désamplification Geometry.

## <a name="span-idpurpose_and_usesspanspan-idpurpose_and_usesspanspan-idpurpose_and_usesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Usage et utilisations


L’étape de nuanceur Geometry traite les primitives entières : les triangles (3 sommets avec jusqu’à 3 sommets adjacents), les lignes (2 sommets avec 2 vertex adjacents) et les points (1 vertex).

![illustration d’un triangle et d’une ligne avec des vertex adjacents](images/d3d10-gs.png)

Le nuanceur Geometry prend également en charge l’amplification et la désamplification Geometry limitées. En fonction d’une primitive d’entrée, le nuanceur Geometry peut ignorer la primitive ou émettre une ou plusieurs nouvelles primitives.

L’étape de nuanceur Geometry (GS) est une étape de nuanceur programmable ; Il est affiché sous la forme d’un bloc arrondi dans le diagramme de [pipeline graphique](graphics-pipeline.md) . Cette étape de nuanceur expose ses propres fonctionnalités uniques, basées sur les modèles de nuanceur (voir [Common-Shader Core](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-common-core)).

L’étape de nuanceur Geometry convient parfaitement aux algorithmes, notamment :

-   Expansion point Sprite
-   Systèmes de particule dynamiques
-   Génération de fourrure/fin
-   Génération du volume de cliché instantané
-   Rendu à passage unique-à-carte cubique
-   Échange de matériel par primitive
-   Configuration des matériaux par primitive : cette fonctionnalité comprend la génération de coordonnées Barycentric en tant que données primitives afin qu’un nuanceur de pixels puisse effectuer une interpolation d’attributs personnalisés.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entrée


L’étape de nuanceur Geometry exécute un code de nuanceur spécifié par l’application avec des primitives entières comme entrée et la possibilité de générer des vertex sur la sortie. Contrairement aux nuanceurs de vertex, qui opèrent sur un seul vertex, les entrées du nuanceur Geometry sont les vertex pour une primitive complète (trois vertex pour triangles, deux sommets pour les lignes ou un vertex unique pour le point). Les nuanceurs de géométrie peuvent également intégrer les données de vertex pour les primitives adjacentes au bord comme entrée (trois autres sommets pour un triangle, deux autres sommets pour une ligne).

L’étape de nuanceur Geometry peut utiliser la valeur générée par le système **SV \_ PrimitiveID** qui est générée automatiquement par l’étape de l' [assembleur d’entrée (IA)](input-assembler-stage--ia-.md). Cela permet de récupérer ou de calculer des données par primitives si vous le souhaitez.

Lorsqu’un nuanceur Geometry est actif, il est appelé une fois pour chaque primitive passée ou générée précédemment dans le pipeline. Chaque appel du nuanceur Geometry considère comme entrée les données pour l’appel de la primitive, qu’il s’agisse d’un point unique, d’une ligne unique ou d’un triangle unique. Une bande de triangles de plus tôt dans le pipeline se traduirait par un appel du nuanceur Geometry pour chaque triangle de la bande (comme si la bande était développée dans une liste de triangles). Toutes les données d’entrée pour chaque vertex de la primitive individuelle sont disponibles (autrement dit, 3 sommets pour un triangle), ainsi que les données de vertex adjacentes, le cas échéant et disponibles.

Abréviations courantes des vertex :

|     |                 |
|-----|-----------------|
| TV  | Sommet de triangle |
| LV  | Sommet de ligne     |
| ALTERNATIF  | Sommet adjacent |

 

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Sortie


L’étape de nuanceur Geometry (GS) est en charge de la génération de plusieurs vertex formant une seule topologie sélectionnée. Les topologies de sortie de nuanceur Geometry disponibles sont **tristrip**, **linestrip**et **PointList**. Le nombre de primitives émises peut varier librement au sein de n’importe quel appel du nuanceur Geometry, bien que le nombre maximal de vertex pouvant être émis doive être déclaré statiquement. Les longueurs de bande émises à partir d’un appel de nuanceur Geometry peuvent être arbitraires et de nouvelles bandes peuvent être créées via la fonction HLSL [RestartStrip](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-so-restartstrip) .

L’exécution d’une instance geometry Shader est atomique d’autres appels, à ceci près que les données ajoutées aux flux sont en série. Les sorties d’un appel donné d’un nuanceur Geometry sont indépendantes des autres appels (bien que le classement soit respecté). Un nuanceur de géométrie qui génère des bandes de triangle démarre une nouvelle bande à chaque appel.

La sortie du nuanceur Geometry peut être alimentée à l’étape de rastérisation et/ou à une mémoire tampon de vertex en mémoire via l’étape de sortie de flux. La sortie alimentée en mémoire est étendue à des listes point/ligne/triangle individuelles (exactement telles qu’elles sont transmises au rastériseur).

Un nuanceur Geometry génère des données d’un vertex à la fois en ajoutant des sommets à un objet de flux de sortie. La topologie des flux est déterminée par une déclaration fixe, en choisissant un **TriangleStream**, **LineStream** et **PointStream** comme sortie de l’étape GS.

Trois types d’objets de flux sont disponibles : **TriangleStream**, **LineStream** et **PointStream**, qui sont tous des objets basés sur un modèle. La topologie de la sortie est déterminée par le type d’objet respectif, tandis que le format des vertex ajoutés au flux est déterminé par le type de modèle.

Quand une sortie de nuanceur Geometry est identifiée comme une valeur interprétée par le système (par exemple, **SV \_ RenderTargetArrayIndex** ou **SV \_ position**), le matériel examine ces données et effectue un comportement dépendant de la valeur, en plus de pouvoir passer les données elles-mêmes à l’étape de nuanceur suivante pour l’entrée. Lorsque la sortie de ce type de données du nuanceur Geometry a un sens pour le matériel au niveau de la primitive (par exemple, **SV \_ RenderTargetArrayIndex** ou **SV \_ ViewportArrayIndex** **), \_ **les données par primitives sont extraites du sommet de début émis pour la primitive, et non ** \_ \[ \] de la même façon** .

Les primitives partiellement terminées peuvent être générées par le nuanceur Geometry si le nuanceur Geometry se termine et que la primitive est incomplète. Les primitives incomplètes sont ignorées silencieusement. Cela est similaire à la façon dont l’IA traite les primitives partiellement terminées.

Le nuanceur Geometry peut effectuer des opérations d’échantillonnage de charge et de texture où les dérivés de l’espace écran ne sont pas nécessaires (**samplelevel**, **samplecmplevelzero**, **samplegrad**).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Pipeline graphique](graphics-pipeline.md)

 

 