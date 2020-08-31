---
title: Vecteurs normaux à une face ou un sommet
description: Chaque face d’une maille a un vecteur normal d’unité perpendiculaire. La direction du vecteur est déterminée par l’ordre dans lequel les vertex sont définis et selon que le système de coordonnées est droitier ou gauche.
ms.assetid: 02333579-9749-4612-B121-23F97898A3E0
keywords:
- Vecteurs normaux à une face ou un sommet
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ef0d3ea5a3bc0f5c4ac6b6b660dc543919d297ec
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168163"
---
# <a name="face-and-vertex-normal-vectors"></a>Vecteurs normaux à une face ou un sommet


Chaque face d’une maille a un vecteur normal d’unité perpendiculaire. La direction du vecteur est déterminée par l’ordre dans lequel les vertex sont définis et selon que le système de coordonnées est droitier ou gauche.

## <a name="span-idperpendicular_unit_normal_vector_for_a_front_facespanspan-idperpendicular_unit_normal_vector_for_a_front_facespanspan-idperpendicular_unit_normal_vector_for_a_front_facespanperpendicular-unit-normal-vector-for-a-front-face"></a><span id="Perpendicular_unit_normal_vector_for_a_front_face"></span><span id="perpendicular_unit_normal_vector_for_a_front_face"></span><span id="PERPENDICULAR_UNIT_NORMAL_VECTOR_FOR_A_FRONT_FACE"></span>Vecteur normal d’unité perpendiculaire pour une face avant


Chaque face d’une maille a un vecteur normal d’unité perpendiculaire. La direction du vecteur est déterminée par l’ordre dans lequel les vertex sont définis et selon que le système de coordonnées est droitier ou gauche. La face normale pointe à l’extérieur du visage. Dans Direct3D, seul le recto d’une face est visible. Une face avant est une face dans laquelle les sommets sont définis dans l’ordre des aiguilles d’une montre.

L’illustration suivante montre un vecteur normal pour une face avant :

![vecteur normal pour une face avant](images/nrmlvect.png)

## <a name="span-idculling_back_facesspanspan-idculling_back_facesspanspan-idculling_back_facesspanculling-back-faces"></a><span id="Culling_back_faces"></span><span id="culling_back_faces"></span><span id="CULLING_BACK_FACES"></span>Élimination des faces arrière


Tout visage qui n’est pas une face avant est une face arrière. Direct3D n’affiche pas toujours les faces arrière ; les faces arrière sont dites en suspens. L’élimination des faces arrière signifie l’élimination des faces arrière du rendu. Vous pouvez modifier le mode d’élimination pour restituer les faces arrière si vous le souhaitez. Pour plus d’informations, consultez élimination de l' [État](/windows/desktop/direct3d9/culling-state) .

## <a name="span-idvertex_unit_normalsspanspan-idvertex_unit_normalsspanspan-idvertex_unit_normalsspanvertex-unit-normals"></a><span id="Vertex_unit_normals"></span><span id="vertex_unit_normals"></span><span id="VERTEX_UNIT_NORMALS"></span>Normales des unités de vertex


Direct3D utilise l’unité de vertex Normals pour les effets d’ombrage, d’éclairage et de texture Gouraud.

L’illustration suivante montre les normales des vertex :

![normales aux vertex](images/vertnrml.png)

Lors de l’application de l’ombrage Gouraud à un polygone, Direct3D utilise les normales des sommets pour calculer l’angle entre la source de lumière et l’aire. Il calcule les valeurs de couleur et d’intensité pour les vertex et les interpole pour chaque point sur toutes les surfaces de la primitive. Direct3D calcule la valeur d’intensité de la lumière en utilisant l’angle. Plus l’angle est grand, moins la lumière est éclairée sur l’aire.

## <a name="span-idflat_surfacesspanspan-idflat_surfacesspanspan-idflat_surfacesspanflat-surfaces"></a><span id="Flat_surfaces"></span><span id="flat_surfaces"></span><span id="FLAT_SURFACES"></span>Surfaces plates


Si vous créez un objet qui est plat, définissez les normales des sommets de façon à ce qu’elles pointent perpendiculairement à la surface.

L’illustration suivante montre une surface plate composée de deux triangles avec des normales de vertex :

![surface plate composée de deux triangles avec des normales de vertex](images/flatvert.png)

## <a name="span-idsmooth_shading_on_a_non-flat_objectspanspan-idsmooth_shading_on_a_non-flat_objectspanspan-idsmooth_shading_on_a_non-flat_objectspansmooth-shading-on-a-non-flat-object"></a><span id="Smooth_shading_on_a_non-flat_object"></span><span id="smooth_shading_on_a_non-flat_object"></span><span id="SMOOTH_SHADING_ON_A_NON-FLAT_OBJECT"></span>Ombrage lissé sur un objet non plat


Au lieu d’un objet plat, il est plus probable que votre objet soit constitué de bandes triangulaires et que les triangles ne soient pas coplanés. Un moyen simple d’obtenir un ombrage lissé sur tous les triangles de la bande consiste à calculer d’abord le vecteur normal de surface pour chaque facette polygonale à laquelle le vertex est associé. La normale du vertex peut être définie pour créer un angle égal avec chaque surface normale. Toutefois, cette méthode peut ne pas être suffisamment efficace pour les primitives complexes.

Cette méthode est illustrée par le diagramme suivant, qui illustre deux surfaces, S1 et S2 vu le bord ci-dessus. Les vecteurs normaux pour S1 et S2 sont affichés en bleu. Le vecteur de la normale du vertex est affiché en rouge. L’angle que le vecteur de la normale du vertex effectue avec la normale de surface de S1 est le même que l’angle entre la normale du vertex et la surface normale de S2. Lorsque ces deux surfaces sont éclairées et grisées à l’aide de l’ombrage Gouraud, le résultat est un bord lisse et correctement arrondi entre eux.

L’illustration suivante montre deux surfaces (S1 et S2) et leurs vecteurs normaux et vecteur normal de vertex :

![deux surfaces (S1 et S2) et leurs vecteurs normaux et vecteur normal de vertex](images/gvert.png)

Si la normale du vertex se rapproche de l’une des visages avec laquelle elle est associée, elle provoque l’augmentation ou la diminution de l’intensité de la lumière pour les points de cette surface, en fonction de l’angle qu’elle effectue avec la source de lumière. Le diagramme suivant montre un exemple. Ces surfaces sont vues latéralement. La normale au vertex se rapproche de S1, provoquant ainsi un angle plus petit avec la source de lumière que si la normale du vertex avait des angles égaux avec les normales de surface.

L’illustration suivante montre deux surfaces (S1 et S2) avec un vecteur de la normale de vertex qui se rapproche d’une face :

![deux surfaces (S1 et S2) avec un vecteur de la normale au sommet qui se rapproche d’une face](images/gvert2.png)

## <a name="span-idsharp_edgesspanspan-idsharp_edgesspanspan-idsharp_edgesspansharp-edges"></a><span id="Sharp_edges"></span><span id="sharp_edges"></span><span id="SHARP_EDGES"></span>Bords nets


Vous pouvez utiliser l’ombrage Gouraud pour afficher des objets dans une scène 3D avec des bords tranchants. Pour ce faire, dupliquez les vecteurs normaux de vertex à n’importe quelle intersection de faces où un bord aigu est requis.

L’illustration suivante montre les vecteurs normaux de vertex dupliqués à des arêtes aiguës :

![vecteurs normaux de vertex dupliqués à bords aigus](images/shade1.png)

Si vous utilisez les méthodes DrawPrimitive pour restituer votre scène, définissez l’objet avec des bords nets comme une liste de triangles, plutôt qu’une bande triangulaire. Lorsque vous définissez un objet en tant que bande triangulaire, Direct3D le traite comme un seul Polygone composé de plusieurs visages triangulaires. L’ombrage Gouraud est appliqué à la fois sur chaque face du polygone et entre les faces adjacentes.

Le résultat est un objet qui est progressivement ombré d’un visage à l’autres. Étant donné qu’une liste de triangles est un polygone composé d’une série de faces triangulaires disjointes, Direct3D applique l’ombrage Gouraud sur chaque face du polygone. Toutefois, il n’est pas appliqué d’un visage à un visage. Si deux ou plusieurs triangles d’une liste triangulaire sont adjacents, ils semblent avoir un bord aigu entre eux.

Une autre solution consiste à passer à l’ombrage plat lors du rendu d’objets avec des bords nets. Il s’agit de la méthode la plus efficace, mais cela peut entraîner la non-affichage des objets de la scène comme les objets qui sont grisés à l’Gouraud.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Systèmes de coordonnées et géométrie](coordinate-systems-and-geometry.md)

 

 