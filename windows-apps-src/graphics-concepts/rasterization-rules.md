---
title: Règles de rastérisation
description: Les règles de pixellisation définissent la façon dont les données vectorielles sont mappées dans les données raster.
ms.assetid: B604725F-96A5-4DB6-B597-9EC57FBBC024
keywords:
- Règles de rastérisation
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7cc139fe9b3ad5324ea6e325f2806d9cf9c5ee1f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168043"
---
# <a name="rasterization-rules"></a>Règles de rastérisation


Les règles de pixellisation définissent la façon dont les données vectorielles sont mappées dans les données raster. Les données raster sont alignées sur les emplacements d’entiers qui sont ensuite éliminés et découpés (pour dessiner le nombre minimal de pixels), et les attributs par pixel sont interpolés (à partir des attributs par vertex) avant d’être passés à un nuanceur de pixels.

Il existe plusieurs types de règles, qui dépendent du type de primitive qui est mappé, ainsi que de la nécessité ou non d’utiliser l’échantillonnage multiple pour réduire les alias. Les illustrations suivantes montrent comment les cas d’angle sont gérés.

## <a name="span-idtrianglespanspan-idtrianglespanspan-idtrianglespantriangle-rasterization-rules-without-multisampling"></a><span id="Triangle"></span><span id="triangle"></span><span id="TRIANGLE"></span>Règles de pixellisation des triangles (sans échantillonnage multiple)


Tout centre de pixels qui se trouve à l’intérieur d’un triangle est dessiné ; un pixel est supposé être à l’intérieur s’il passe la règle en haut à gauche. La règle en haut à gauche est qu’un centre de pixels est défini pour se trouver à l’intérieur d’un triangle s’il se trouve sur le bord supérieur ou sur le bord gauche d’un triangle.

où :

-   Un bord supérieur est un bord qui est exactement horizontal et se trouve au-dessus des autres bords.
-   Un bord gauche est un bord qui n’est pas exactement horizontal et se trouve sur le côté gauche du triangle. un triangle peut avoir un ou deux bords gauches.

La règle en haut à gauche garantit que les triangles adjacents sont dessinés une seule fois.

Cette illustration montre des exemples de pixels qui sont dessinés, car ils se trouvent soit à l’intérieur d’un triangle, soit suivent la règle en haut à gauche.

![exemples de pixellisation des triangles en haut à gauche](images/d3d10-rasterrulestriangle.png)

Les pixels gris clair et foncé des pixels les affichent sous forme de groupes de pixels pour indiquer le triangle dans lequel ils se trouvent.

## <a name="span-idline_1spanspan-idline_1spanspan-idline_1spanline-rasterization-rules-aliased-without-multisampling"></a><span id="Line_1"></span><span id="line_1"></span><span id="LINE_1"></span>Règles de pixellisation de ligne (avec alias, sans échantillonnage multiple)


Les règles de pixellisation de ligne utilisent une zone de test Diamond pour déterminer si une ligne couvre un pixel. Pour les lignes x-major (lignes avec-1 &lt; = pente &lt; = + 1), la zone de test Diamond inclut (illustrée) le bord inférieur gauche, le bord inférieur droit et le coin inférieur ; le losange exclut le bord supérieur gauche, le bord supérieur droit, le plus grand cordon, le coin gauche et le coin droit. Une ligne « y » est une ligne qui n’est pas une ligne x-major ; la zone de test Diamond est la même que celle qui est décrite pour la ligne principale x, sauf que l’angle droit est également inclus.

À partir de la zone losange, une ligne couvre un pixel si la ligne quitte la zone de test de losange du pixel lorsqu’elle se trouve le long de la ligne du début vers la fin. Une bande de lignes se comporte de la même manière, car elle est dessinée comme une séquence de lignes.

L’illustration suivante montre quelques exemples.

![exemples de pixellisation de ligne avec alias](images/d3d10-rasterrulesline.png)

## <a name="span-idline_2spanspan-idline_2spanspan-idline_2spanline-rasterization-rules-antialiased-without-multisampling"></a><span id="Line_2"></span><span id="line_2"></span><span id="LINE_2"></span>Règles de pixellisation de ligne (avec anticrénelage, sans échantillonnage multiple)


Une ligne avec un alias est pixellisée comme s’il s’agissait d’un rectangle (avec la largeur = 1). Le rectangle croise une cible de rendu produisant des valeurs de couverture par pixel, qui sont multipliées par des composants alpha de sortie de nuanceur de pixels. Il n’y a pas d’anticrénelage préformé lors du dessin de lignes sur une cible de rendu à échantillonnage.

Il est considéré qu’il n’existe pas de méthode « optimale » pour effectuer un rendu de ligne avec anticrénelage. Direct3D adopte comme indication la méthode illustrée dans l’illustration suivante. Cette méthode a été dérivée de façon empirique, présentant un certain nombre de propriétés visuelles jugées souhaitables.

Le matériel n’a pas besoin de correspondre exactement à cet algorithme. les tests sur cette référence ont des tolérances « raisonnables », guidées par certains des principes énumérés ci-dessous, autorisant diverses implémentations matérielles et des tailles de noyau de filtre. Toutefois, aucune de ces possibilités de mise en œuvre matérielle ne peut être communiquée par Direct3D à des applications, au-delà du simple dessin des lignes et de l’observation/mesure de leur apparence.

![exemples de pixellisation de ligne avec anticrénelage](images/d3d10-rasterruleslineaa.png)

Cet algorithme génère des lignes relativement lisses, avec une intensité uniforme, avec des bords dentelés minimaux ou une tresse. Le modèle de moire pour les lignes de fermeture est réduit. Il y a une bonne couverture pour les jonctions entre les segments de ligne placés de bout en bout.

Le noyau de filtre est un compromis raisonnable entre la quantité de flou des bords et les changements d’intensité provoqués par les corrections gamma. La valeur de couverture est multipliée par le nuanceur de pixels o0. a (srcAlpha) selon la formule suivante par l' [étape de fusion de sortie (OM)](output-merger-stage--om-.md):

srcColor \* srcAlpha + destColor \* (1-srcAlpha)

## <a name="span-idpointspanspan-idpointspanspan-idpointspanpoint-rasterization-rules-without-multisampling"></a><span id="Point"></span><span id="point"></span><span id="POINT"></span>Règles de pixellisation des points (sans échantillonnage multiple)


Un point est interprété comme s’il était composé de deux triangles dans un modèle Z, qui utilisent des règles de pixellisation de triangle. La coordonnée identifie le centre d’un carré de largeur d’un pixel. Il n’existe aucun Culling pour les points.

L’illustration suivante montre quelques exemples.

![exemples de pixellisation des points](images/d3d10-rasterrulespoint.png)

## <a name="span-idmultisamplespanspan-idmultisamplespanspan-idmultisamplespanmultisample-anti-aliasing-rasterization-rules"></a><span id="Multisample"></span><span id="multisample"></span><span id="MULTISAMPLE"></span>Exemples de règles de pixellisation d’anticrénelage


L’anticrénelage (multi-sample anticrénelage) réduit les alias géométriques à l’aide de tests de couverture de pixel et de stencil de profondeur dans plusieurs emplacements de sous-échantillons. Pour améliorer les performances, les calculs par pixel sont effectués une fois pour chaque pixel couvert, en partageant les sorties de nuanceur sur les sous-pixels couverts. L’anticrénelage sur un échantillonnage n’réduit pas l’alias des surfaces. Les exemples d’emplacements et les fonctions de reconstruction dépendent de l’implémentation matérielle.

L’illustration suivante montre quelques exemples.

![exemples de rastérisation d’antialias à échantillonnages](images/d3d10-rasterrulesmsaa.png)

Le nombre d’emplacements d’échantillonnage dépend du mode d’échantillonnage. Les attributs de vertex sont interpolés aux centres de pixels, puisque c’est là que le nuanceur de pixels est appelé (cela devient l’extrapolation si le centre n’est pas couvert). Les attributs peuvent être signalés dans le nuanceur de pixels pour être pris en exemple de centre de gravité, ce qui amène les pixels non couverts à interpoler l’attribut à l’intersection de la zone du pixel et de la primitive.

Un nuanceur de pixels s’exécute pour chaque zone de pixels de 2x2 pour prendre en charge les calculs dérivés (qui utilisent des deltas x et y). Cela signifie que les appels de nuanceur se produisent plus que ce qui est indiqué pour remplir le nombre minimal de quanta 2x2 (qui est indépendant de l’échantillonnage multiple). Le résultat du nuanceur est écrit pour chaque exemple couvert qui réussit le test de stencil de profondeur par exemple.

Les règles de pixellisation pour les primitives sont, en général, inchangées par l’anticrénelage multiéchantillon, à l’exception des éléments suivants :

-   Pour un triangle, un test de couverture est effectué pour chaque emplacement d’échantillon (et non pour un centre de pixels). Si plusieurs emplacements de l’échantillon sont couverts, un nuanceur de pixels s’exécute une fois avec des attributs interpolés au centre de pixels. Le résultat est stocké (répliqué) pour chaque emplacement d’échantillon couvert dans le pixel qui réussit le test de profondeur/gabarit.

    Une ligne est traitée comme un rectangle constitué de deux triangles, avec une largeur de ligne de 1,4.

-   Pour un point, un test de couverture est effectué pour chaque exemple d’emplacement (et non pour un centre de pixels).

Les formats d’échantillonnage multiple peuvent être utilisés dans les cibles de rendu qui peuvent être lues dans les nuanceurs à l’aide de [Load](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-load), car aucune résolution n’est requise pour les exemples individuels auxquels le nuanceur accède. Les formats de profondeur ne sont pas pris en charge pour les ressources à échantillonnage unique. par conséquent, les formats de profondeur sont limités aux cibles de rendu uniquement.

Les formats non typés prennent en charge l’échantillonnage multiple pour permettre à un affichage des ressources d’interpréter les données de différentes façons. Par exemple, vous pouvez créer un exemple de ressource à l’aide de R8G8B8A8 \_ sans type, le rendre à l’aide d’une ressource de vue de rendu-cible avec un \_ format R8G8B8A8 uint, puis résoudre le contenu en une autre ressource avec un \_ format de données R8G8B8A8 UNORM.

### <a name="span-idhardware_supportspanspan-idhardware_supportspanspan-idhardware_supportspanhardware-support"></a><span id="Hardware_Support"></span><span id="hardware_support"></span><span id="HARDWARE_SUPPORT"></span>Support matériel

L’API signale la prise en charge du matériel pour l’échantillonnage multiple via le nombre de niveaux de qualité. Par exemple, un niveau de qualité 0 signifie que le matériel ne prend pas en charge l’échantillonnage multiple (à un format et un niveau de qualité particuliers). Un 3 pour les niveaux de qualité signifie que le matériel prend en charge trois exemples de disposition et/ou de résolution des algorithmes. Vous pouvez également supposer les éléments suivants :

-   Tout format qui prend en charge l’échantillonnage multiple prend en charge le même nombre de niveaux de qualité pour chaque format de cette famille.
-   Chaque format qui prend en charge l’échantillonnage multiple et a les \_ formats UNORM, \_ sRVB, \_ ronfler ou \_ float, prend également en charge la résolution.

### <a name="span-idcentroid_samplingspanspan-idcentroid_samplingspanspan-idcentroid_samplingspancentroid-sampling-of-attributes-when-multisample-antialiasing"></a><span id="Centroid_Sampling"></span><span id="centroid_sampling"></span><span id="CENTROID_SAMPLING"></span>Échantillonnage de centre de gravité des attributs lors de l’anticrénelage à échantillonnage multiple

Par défaut, les attributs de vertex sont interpolés sur un centre de pixels pendant l’anticrénelage d’échantillonnage. Si le centre de pixels n’est pas couvert, les attributs sont extrapolés à un centre de pixels. Si une entrée de nuanceur de pixels contenant la sémantique du centre de données (en supposant que le pixel n’est pas entièrement couvert) sera échantillonnée quelque part dans la zone couverte du pixel, éventuellement à l’un des emplacements couverts. Un échantillon de masque (spécifié par l’état du rastériseur) est appliqué avant le calcul de centre de gravité. Par conséquent, un échantillon masqué n’est pas utilisé comme emplacement de centre de gravité.

Le rastériseur de référence choisit un exemple d’emplacement d’échantillonnage de centre de gravité semblable à celui-ci :

-   L’exemple de masque autorise tous les échantillons. Utilisez un centre de pixels si le pixel est couvert ou si aucun des exemples n’est couvert. Dans le cas contraire, le premier échantillon couvert est choisi, en commençant par le centre de pixels et en déplaçant vers l’extérieur.
-   L’exemple de masque désactive tous les exemples, sauf un (scénario courant). Une application peut implémenter l’échantillonnage multipasse en parcourant des valeurs de masque d’échantillon de bit unique et en rerendant la scène pour chaque échantillon à l’aide de l’échantillonnage de centre de gravité. Cela nécessiterait qu’une application ajuste les dérivés pour sélectionner des MIPS de texture plus détaillés de manière appropriée pour la densité d’échantillonnage de texture supérieure.

### <a name="span-idderivative_calculationsspanspan-idderivative_calculationsspanspan-idderivative_calculationsspanderivative-calculations-when-multisampling"></a><span id="Derivative_Calculations"></span><span id="derivative_calculations"></span><span id="DERIVATIVE_CALCULATIONS"></span>Calculs de dérivés lors de l’échantillonnage multiple

Les nuanceurs de pixels s’exécutent toujours à l’aide d’une zone de pixels 2x2 minimale pour prendre en charge les calculs dérivés, qui sont calculés en prenant des deltas entre les données de pixels adjacents (en partant du principe que les données de chaque pixel ont été échantillonnées avec l’espacement horizontal ou vertical). Cela n’est pas affecté par l’échantillonnage multiple.

Si des dérivés sont demandés sur un attribut qui a été échantillonné, le calcul du matériel n’est pas ajusté, ce qui peut provoquer des dérivées inexactes. Un nuanceur attend un vecteur d’unité dans l’espace de rendu-cible, mais peut obtenir un vecteur de non-unité par rapport à un autre espace vectoriel. Par conséquent, il est de la responsabilité de l’application de demander des dérivés des attributs qui sont échantillonnés.

En fait, il est recommandé de ne pas combiner les dérivés et l’échantillonnage de centre de gravité. L’échantillonnage de centre de gravité peut être utile dans les situations où il est essentiel que les attributs interpolés d’une primitive ne soient pas extrapolés, mais cela s’accompagne de compromis tels que des attributs qui semblent sauter là où un bord primitif traverse un pixel (plutôt que changer en continu) ou des dérivés qui ne peuvent pas être utilisés par les opérations d’échantillonnage de texture qui dérivent LOD

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Annexes](appendix.md)

[Étape Rastériseur (RS)](rasterizer-stage--rs-.md)

 

 