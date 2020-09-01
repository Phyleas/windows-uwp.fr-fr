---
title: Introduction aux règles de rastérisation
description: Souvent, les points spécifiés pour les vertex ne correspondent pas exactement aux pixels de l’écran. Dans ce cas, Direct3D applique les règles de pixellisation des triangles pour décider quels pixels s’appliquent à un triangle donné.
ms.assetid: 4232CDBA-F669-4417-9378-F9013E83462C
keywords:
- Introduction aux règles de rastérisation
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 38522be28280c0a08f6cb065e5dfb5c2f26642a8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162793"
---
# <a name="introduction-to-rasterization-rules"></a>Introduction aux règles de rastérisation


Souvent, les points spécifiés pour les vertex ne correspondent pas exactement aux pixels de l’écran. Dans ce cas, Direct3D applique les règles de pixellisation des triangles pour décider quels pixels s’appliquent à un triangle donné.

Il s’agit d’une présentation simplifiée des règles de pixellisation. Pour plus d’informations, consultez [règles de pixellisation](rasterization-rules.md). Voir aussi l' [étape rastériseur (RS)](rasterizer-stage--rs-.md).

## <a name="span-idtriangle_rasterization_rulesspanspan-idtriangle_rasterization_rulesspanspan-idtriangle_rasterization_rulesspantriangle-rasterization-rules"></a><span id="Triangle_Rasterization_Rules"></span><span id="triangle_rasterization_rules"></span><span id="TRIANGLE_RASTERIZATION_RULES"></span>Règles de pixellisation des triangles


Direct3D utilise une convention de remplissage en haut à gauche pour remplir la géométrie. Il s’agit de la même convention que celle utilisée pour les rectangles dans GDI et OpenGL. Dans Direct3D, le centre du pixel est le point déterminant. Si le centre est à l’intérieur d’un triangle, le pixel fait partie du triangle. Les centres de pixels sont des coordonnées entières.

Cette description des règles de pixellisation des triangles utilisées par Direct3D ne s’applique pas nécessairement à tout le matériel disponible. Vos tests peuvent découvrir des variations mineures dans l’implémentation de ces règles.

L’illustration suivante montre un rectangle dont l’angle supérieur gauche se trouve à (0,0) et dont l’angle inférieur droit est (5, 5). Ce rectangle remplit 25 pixels, comme prévu. La largeur du rectangle est définie à droite, moins gauche. La hauteur est définie en tant que bas moins haut.

![un carré numéroté divisé en six lignes et colonnes](images/pixmap.png)

Dans la Convention de remplissage en haut à gauche, *Top* fait référence à l’emplacement vertical des étendues horizontales, tandis que *Left* fait référence à l’emplacement horizontal des pixels dans une étendue. Un bord ne peut pas être un bord supérieur, sauf s’il est horizontal. En général, la plupart des triangles ont uniquement des bords gauche et droit. L’illustration suivante montre un bord supérieur et un bord droit.

![carré numéroté qui contient deux triangles](images/triedge.png)

La Convention de remplissage en haut à gauche détermine l’action effectuée par Direct3D lorsqu’un triangle passe au centre d’un pixel. L’illustration suivante montre deux triangles, un à (0, 0), (5, 0) et (5, 5) et l’autre à (0, 5), (0, 0) et (5, 5). Dans ce cas, le premier triangle obtient 15 pixels (en noir), tandis que le second obtient uniquement 10 pixels (indiqué en gris), car la périphérie partagée est le bord gauche du premier triangle.

![carré numéroté qui affiche deux triangles](images/twotris.png)

Si vous définissez un rectangle avec son coin supérieur gauche à (0,5, 0,5) et son coin inférieur droit à (2,5, 4,5), le point central de ce rectangle est (1,5, 2,5). Lorsque le rastériseur Direct3D tessellates ce rectangle, le centre de chaque pixel est sans ambiguïté à l’intérieur de chacun des quatre triangles, et la Convention de remplissage supérieure gauche n’est pas nécessaire. L’illustration suivante montre cela. Les pixels du rectangle sont étiquetés en fonction du triangle dans lequel Direct3D les intègre.

![carré numéroté qui contient un rectangle divisé en quatre triangles](images/noambig.png)

Si vous déplacez le rectangle de l’illustration précédente afin que son coin supérieur gauche se trouve à l’emplacement (1,0, 1,0), à l’angle inférieur droit à (3,0, 5,0) et à son point central à (2,0, 3,0), Direct3D applique la Convention de remplissage en haut à gauche. La plupart des pixels de ce rectangle chevauchent la bordure entre deux ou plusieurs triangles, comme le montre l’illustration suivante.

![carré numéroté qui contient un rectangle divisé en quatre triangles](images/fillrule.png)

Pour les deux rectangles, les mêmes pixels sont affectés, comme indiqué dans l’illustration suivante.

![pixels affectés par les deux carrés numérotés précédents](images/samepix.png)

## <a name="span-idpoint_and_line_rulesspanspan-idpoint_and_line_rulesspanspan-idpoint_and_line_rulesspanpoint-and-line-rules"></a><span id="Point_and_Line_Rules"></span><span id="point_and_line_rules"></span><span id="POINT_AND_LINE_RULES"></span>Règles de point et de ligne


Les points sont restitués de la même façon que les points sprites, qui sont rendus sous forme de quadrilatères alignés sur l’écran et sont donc conformes aux mêmes règles que le rendu de polygones.

Les règles de rendu de ligne sans anticrénelage sont exactement les mêmes que celles pour les [lignes GDI](/windows/desktop/gdi/lines).

## <a name="span-idpoint_sprite_rulesspanspan-idpoint_sprite_rulesspanspan-idpoint_sprite_rulesspanpoint-sprite-rules"></a><span id="Point_Sprite_Rules"></span><span id="point_sprite_rules"></span><span id="POINT_SPRITE_RULES"></span>Règles de point Sprite


Les points de soulignement et les primitives de correctif sont pixellisés comme si les primitives étaient d’abord fractionnées en triangles et les triangles obtenus pixellisés.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Appareils](devices.md)

[Étape Rastériseur (RS)](rasterizer-stage--rs-.md)

[Règles de rastérisation](rasterization-rules.md)

 

 