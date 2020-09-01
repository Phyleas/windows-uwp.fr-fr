---
title: Types de ressources
description: Les différents types de ressources ont une disposition distincte (ou un encombrement mémoire).
ms.assetid: BCDDF227-1837-44DA-ABD4-E39BCFF2B8EF
keywords:
- Types de ressources
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b300747027f0c9466e7ca04b4c4b571882f57a6e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156433"
---
# <a name="resource-types"></a>Types de ressources


Les différents types de ressources ont une disposition distincte (ou un encombrement mémoire). Toutes les ressources utilisées par le pipeline Direct3D dérivent de deux types de ressources de base : les [mémoires tampons](#buffer-resources) et les [textures](#texture-resources). Une mémoire tampon est une collection de données brutes (éléments); une texture est une collection de texels (éléments de texture).

Il existe deux façons de spécifier entièrement la disposition (ou l’encombrement mémoire) d’une ressource :

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Élément</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><span id="Typed"></span><span id="typed"></span><span id="TYPED"></span>Tapé</p></td>
<td align="left"><p>Spécifiez complètement le type lorsque la ressource est créée.</p></td>
</tr>
<tr class="even">
<td align="left"><p><span id="Typeless"></span><span id="typeless"></span><span id="TYPELESS"></span>Sans type</p></td>
<td align="left"><p>Spécifiez complètement le type lorsque la ressource est liée au pipeline.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idbuffer_resourcesspanspan-idbuffer_resourcesspanspan-idbuffer_resourcesspanspan-idbuffer-resourcesspanbuffer-resources"></a><span id="Buffer_Resources"></span><span id="buffer_resources"></span><span id="BUFFER_RESOURCES"></span><span id="buffer-resources"></span>Ressources de mémoire tampon


Une ressource de mémoire tampon est une collection de données entièrement typées ; en interne, une mémoire tampon contient des éléments. Un élément est composé de 1 à 4 composants. Voici des exemples de types de données d’élément : une valeur de données compressée (comme R8G8B8A8), un entier 8 bits unique, des valeurs float 4 32 bits. Ces types de données sont utilisés pour stocker des données, telles qu’un vecteur de position, un vecteur normal, une coordonnée de texture dans une mémoire tampon de vertex, un index dans une mémoire tampon d’index ou l’état de l’appareil.

Une mémoire tampon est créée en tant que ressource non structurée. Étant donné qu’elle n’est pas structurée, une mémoire tampon ne peut pas contenir de niveaux de mipmap, n’est pas filtrée lors de la lecture et ne peut pas être multiéchantillonnée.

### <a name="span-idbuffer_typesspanspan-idbuffer_typesspanspan-idbuffer_typesspanbuffer-types"></a><span id="Buffer_Types"></span><span id="buffer_types"></span><span id="BUFFER_TYPES"></span>Types de mémoires tampons

-   [Mémoire tampon de vertex](#vertex-buffer)
-   [Mémoire tampon d’index](#index-buffer)
-   [Mémoire tampon constante](#shader-constant-buffer)

### <a name="span-idvertex_bufferspanspan-idvertex_bufferspanspan-idvertex_bufferspanspan-idvertex-bufferspanvertex-buffer"></a><span id="Vertex_Buffer"></span><span id="vertex_buffer"></span><span id="VERTEX_BUFFER"></span><span id="vertex-buffer"></span>Mémoire tampon de vertex

Une mémoire tampon est une collection d’éléments ; une mémoire tampon de vertex contient des données par vertex. L’exemple le plus simple est une mémoire tampon de vertex qui contient un type de données, par exemple des données de position. Il peut être visualisé comme dans l’illustration suivante.

![illustration d’une mémoire tampon de vertex qui contient des données de position](images/d3d10-resources-single-element-vb2.png)

Plus souvent, une mémoire tampon de vertex contient toutes les données nécessaires pour spécifier entièrement les sommets 3D. Par exemple, il peut s’agir d’une mémoire tampon de vertex qui contient la position par vertex, les coordonnées normales et de texture. Ces données sont généralement organisées sous la forme d’ensembles d’éléments par vertex, comme indiqué dans l’illustration suivante.

![illustration d’une mémoire tampon de vertex qui contient la position, la normale et les données de texture](images/d3d10-vertex-buffer-element.png)

Cette mémoire tampon de vertex contient des données par vertex pour huit vertex ; chaque vertex stocke trois éléments (coordonnées de position, normale et de texture). Les positions et normales sont généralement spécifiées à l’aide de valeurs float 3 32 bits et de coordonnées de texture à l’aide de valeurs float 2 32 bits.

Pour accéder aux données à partir d’une mémoire tampon de vertex, vous devez savoir à quel vertex accéder et ces autres paramètres de mémoire tampon :

-   *Offset* : nombre d’octets à partir du début de la mémoire tampon vers les données du premier vertex.
-   *BaseVertexLocation* -nombre d’octets entre le décalage et le premier vertex utilisé par l’appel de dessin approprié.

Avant de créer une mémoire tampon de vertex, vous devez définir sa disposition en créant un objet de disposition d’entrée. Une fois l’objet de disposition d’entrée créé, liez-le à l’étape de l’assembleur d’entrée (IA).

### <a name="span-idindex_bufferspanspan-idindex_bufferspanspan-idindex_bufferspanspan-idindex-bufferspanindex-buffer"></a><span id="Index_Buffer"></span><span id="index_buffer"></span><span id="INDEX_BUFFER"></span><span id="index-buffer"></span>Mémoire tampon d’index

Une mémoire tampon d’index contient un ensemble séquentiel d’index 16 bits ou 32 bits ; chaque index est utilisé pour identifier un vertex dans une mémoire tampon de vertex. L’utilisation d’un tampon d’index avec une ou plusieurs mémoires tampons de vertex pour fournir des données à l’étape IA est appelée indexation. Un tampon d’index peut être visualisé comme dans l’illustration suivante.

![illustration d’un tampon d’index](images/d3d10-index-buffer.png)

Les index séquentiels stockés dans une mémoire tampon d’index se trouvent avec les paramètres suivants :

-   *Offset* : nombre d’octets entre le début de la mémoire tampon et le premier index.
-   *StartIndexLocation* -nombre d’octets entre le décalage et le premier vertex utilisé par l’appel de dessin approprié.
-   *IndexCount* -nombre d’index à afficher.

Une mémoire tampon d’index peut combiner plusieurs bandes de lignes ou de triangle ([topologies de primitives](primitive-topologies.md)) en les séparant par un index de coupe. Un index de coupe-bande permet de dessiner plusieurs bandes de lignes ou de triangles à l’aide d’un seul appel de dessin. Un index de coupe est la valeur maximale possible pour l’index (0xFFFF pour un index 16 bits, 0xFFFFFFFF pour un index 32 bits). L’index de coupe rétablit l’ordre d’enroulement dans les primitives indexées et peut être utilisé pour supprimer les triangles dégénérées qui peuvent autrement être nécessaires pour conserver l’ordre d’enroulement approprié dans une bande de triangle. L’illustration suivante montre un exemple d’index de coupe-bande.

![illustration d’un index de coupe-bande](images/d3d10-ia-strips-cut-value.png)

### <a name="span-idshader_constant_bufferspanspan-idshader_constant_bufferspanspan-idshader_constant_bufferspanspan-idshader-constant-bufferspanconstant-buffer"></a><span id="Shader_Constant_Buffer"></span><span id="shader_constant_buffer"></span><span id="SHADER_CONSTANT_BUFFER"></span><span id="shader-constant-buffer"></span>Mémoire tampon constante

Direct3D dispose d’une mémoire tampon pour fournir des constantes de nuanceur appelées une mémoire tampon de nuanceur constante ou simplement une mémoire tampon constante. Conceptuellement, elle ressemble à une mémoire tampon de vertex à un seul élément, comme indiqué dans l’illustration suivante.

![illustration d’une mémoire tampon de nuanceur constante](images/d3d10-shader-resource-buffer.png)

Chaque élément stocke une constante composant de 1 à 4, déterminée par le format des données stockées.

Les mémoires tampons constantes réduisent la bande passante requise pour mettre à jour les constantes de nuanceur en permettant aux constantes de nuanceur d’être regroupées et validées en même temps plutôt que d’effectuer des appels individuels pour valider chaque constante séparément.

Un nuanceur continue à lire des variables dans une mémoire tampon constante directement par nom de variable de la même façon que les variables qui ne font pas partie d’une mémoire tampon constante sont lues.

Chaque étape de nuanceur autorise jusqu’à 15 mémoires tampons constantes de nuanceur ; chaque mémoire tampon peut contenir jusqu’à 4096 constantes.

Utilisez une mémoire tampon constante pour stocker les résultats de l’étape de flux de sortie.

Pour obtenir un exemple de déclaration d’une mémoire tampon constante dans un nuanceur, consultez [constantes de nuanceur (DirectX HLSL)](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-constants) .

## <a name="span-idtexture_resourcesspanspan-idtexture_resourcesspanspan-idtexture_resourcesspanspan-idtexture-resourcesspantexture-resources"></a><span id="Texture_Resources"></span><span id="texture_resources"></span><span id="TEXTURE_RESOURCES"></span><span id="texture-resources"></span>Ressources de texture


Une ressource de texture est une collection structurée de données conçue pour stocker des texels. Contrairement aux mémoires tampons, les textures peuvent être filtrées par des échantillonneurs de texture, car elles sont lues par des unités de nuanceur. Le type de texture affecte la manière dont la texture est filtrée. Un Texel représente la plus petite unité d’une texture qui peut être lue ou écrite par le pipeline. Chaque Texel contient 1 à 4 composants, organisés dans l’un des formats DXGI (voir [** \_ format dxgi**](/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format)).

Les textures sont créées en tant que ressource structurée afin que leur taille soit connue. Toutefois, chaque texture peut être typée ou sans type au moment de la création de ressources, à condition que le type soit entièrement spécifié à l’aide d’une vue lorsque la texture est liée au pipeline.

-   [Types de texture](#texture-types)
-   [Sous-ressources](#subresources)
-   [Typage fort et typage faible](#typed)

### <a name="span-idtexture_typesspanspan-idtexture_typesspanspan-idtexture_typesspanspan-idtexture-typesspantexture-types"></a><span id="Texture_Types"></span><span id="texture_types"></span><span id="TEXTURE_TYPES"></span><span id="texture-types"></span>Types de texture

Il existe plusieurs types de textures : 1D, 2D, 3D, qui peuvent être créées avec ou sans des mipmaps. Direct3D prend également en charge les tableaux de texture et les textures à échantillonnages.

-   [Texture 1D](#texture1d-resource)
-   [Tableau de textures 1D](#texture1d-array-resource)
-   [Texture 2D et tableau de texture 2D](#texture2d-resource)
-   [Texture 3D](#texture3d-resource)

### <a name="span-idtexture1d_resourcespanspan-idtexture1d_resourcespanspan-idtexture1d_resourcespanspan-idtexture1d-resourcespan1d-texture"></a><span id="Texture1D_Resource"></span><span id="texture1d_resource"></span><span id="TEXTURE1D_RESOURCE"></span><span id="texture1d-resource"></span>Texture 1D

Une texture 1D dans sa forme la plus simple contient des données de texture qui peuvent être traitées par une coordonnée de texture unique. Il peut être visualisé sous la forme d’un tableau de texels, comme indiqué dans l’illustration suivante.

![illustration d’une texture 1D](images/d3d10-1d-texture.png)

Chaque Texel contient un certain nombre de composants de couleur en fonction du format des données stockées. Pour augmenter la complexité, vous pouvez créer une texture 1D avec des niveaux de mipmap, comme indiqué dans l’illustration suivante.

![illustration d’une texture 1D avec des niveaux de mipmap](images/d3d10-resource-texture1d.png)

Un niveau de mipmap est une texture qui est une puissance de deux plus petite que le niveau supérieur. Le niveau le plus élevé contient le plus de détails, chaque niveau suivant étant plus petit ; pour un mipmap 1D, le plus petit niveau contient un Texel. Les différents niveaux sont identifiés par un index appelé LOD (niveau de détail); vous pouvez utiliser le LOD pour accéder à une texture plus petite lors du rendu de la géométrie qui n’est pas aussi proche de l’appareil photo.

### <a name="span-idtexture1d_array_resourcespanspan-idtexture1d_array_resourcespanspan-idtexture1d_array_resourcespanspan-idtexture1d-array-resourcespan1d-texture-array"></a><span id="Texture1D_Array_Resource"></span><span id="texture1d_array_resource"></span><span id="TEXTURE1D_ARRAY_RESOURCE"></span><span id="texture1d-array-resource"></span>Tableau de textures 1D

Direct3D 10 possède également une nouvelle structure de données pour un tableau de textures. Un tableau de textures 1D se présente conceptuellement comme dans l’illustration suivante.

![illustration d’un tableau de textures 1D](images/d3d10-resource-texture1darray.png)

Ce tableau de texture contient trois textures. Chacune des trois textures a une largeur de texture de 5 (qui est le nombre d’éléments dans la première couche). Chaque texture contient également un mipmap à 3 couches.

Tous les tableaux de texture dans Direct3D sont un tableau homogène de textures. Cela signifie que chaque texture d’un tableau de texture doit avoir le même format de données et la même taille (y compris la largeur de texture et le nombre de niveaux de mipmap). Vous pouvez créer des tableaux de texture de différentes tailles, à condition que toutes les textures dans chaque tableau correspondent à la taille.

### <a name="span-idtexture2d_resourcespanspan-idtexture2d_resourcespanspan-idtexture2d_resourcespanspan-idtexture2d-resourcespan2d-texture-and-2d-texture-array"></a><span id="Texture2D_Resource"></span><span id="texture2d_resource"></span><span id="TEXTURE2D_RESOURCE"></span><span id="texture2d-resource"></span>Texture 2D et tableau de texture 2D

Une ressource Texture2D contient une grille 2D de texels. Chaque Texel est adressable par un vecteur u, v. Comme il s’agit d’une ressource de texture, elle peut contenir des niveaux de mipmap et des sous-ressources. Une ressource de texture 2D entièrement remplie ressemble à l’illustration suivante.

![illustration d’une ressource de texture 2D](images/d3d10-resource-texture2d.png)

Cette ressource de texture contient une seule texture 3x5 avec trois niveaux de mipmap.

Une ressource Texture2DArray est un tableau homogène de textures 2D ; autrement dit, chaque texture a le même format de données et les mêmes dimensions (y compris les niveaux de mipmap). Elle présente une disposition similaire à celle du tableau de texture 1D, à ceci près que les textures contiennent désormais des données 2D et qu’elles ressemblent donc à l’illustration suivante.

![illustration d’un tableau de ressources de texture 2D](images/d3d10-resource-texture2darray.png)

Ce tableau de texture contient trois textures ; chaque texture est 3x5 avec deux niveaux de mipmap.

### <a name="span-idtexture2darray_resource_as_a_texture_cubespanspan-idtexture2darray_resource_as_a_texture_cubespanspan-idtexture2darray_resource_as_a_texture_cubespanusing-a-texture2darray-as-a-texture-cube"></a><span id="Texture2DArray_Resource_as_a_Texture_Cube"></span><span id="texture2darray_resource_as_a_texture_cube"></span><span id="TEXTURE2DARRAY_RESOURCE_AS_A_TEXTURE_CUBE"></span>Utilisation d’un Texture2DArray en tant que cube de texture

Un cube de texture est un tableau de textures 2D qui contient 6 textures, une pour chaque visage du cube. Un cube de texture entièrement rempli ressemble à l’illustration suivante.

![illustration d’un tableau de ressources de texture 2D qui représentent un cube de texture](images/d3d10-resource-texturecube.png)

Un tableau de textures 2D qui contient 6 textures peut être lu dans les nuanceurs avec les fonctions intrinsèques de mappage de cube, une fois qu’elles sont liées au pipeline avec une vue de texture de cube. Les cubes de texture sont adressés à partir du nuanceur avec un vecteur 3D qui pointe vers le centre du cube de texture.

### <a name="span-idtexture3d_resourcespanspan-idtexture3d_resourcespanspan-idtexture3d_resourcespanspan-idtexture3d-resourcespan3d-texture"></a><span id="Texture3D_Resource"></span><span id="texture3d_resource"></span><span id="TEXTURE3D_RESOURCE"></span><span id="texture3d-resource"></span>Texture 3D

Une ressource Texture3D (également appelée texture de volume) contient un volume 3D de texels. Étant donné qu’il s’agit d’une ressource de texture, elle peut contenir des niveaux de mipmap. Une texture 3D entièrement remplie ressemble à l’illustration suivante.

![illustration d’une ressource de texture 3D](images/d3d10-resource-texture3d.png)

Quand une tranche mipmap 3D est liée en tant que sortie de la cible de rendu (avec une vue de cible de rendu), la texture 3D se comporte de la même manière qu’un tableau de texture 2D avec n tranches. La tranche de rendu particulière est choisie à partir de l’étape Geometry-Shader.

Il n’existe aucun concept de tableau de texture 3D ; par conséquent, une sous-ressource de texture 3D est un niveau mipmap unique.

### <a name="span-idsubresourcesspanspan-idsubresourcesspanspan-idsubresourcesspansubresources"></a><span id="Subresources"></span><span id="subresources"></span><span id="SUBRESOURCES"></span>Sous-ressources

L’API Direct3D référence des ressources entières ou des sous-ensembles de ressources. Pour spécifier la partie des ressources, Direct3D a inventé le terme sous- *ressources*, ce qui signifie qu’il s’agit d’un sous-ensemble d’une ressource.

Une mémoire tampon est définie en tant que sous-ressource unique. Les textures sont un peu plus compliquées, car il existe plusieurs types de textures (1D, 2D, etc.) qui prennent en charge des niveaux de mipmap et/ou des tableaux de texture. En commençant par le cas le plus simple, une texture 1D est définie en tant que sous-ressource unique, comme le montre l’illustration suivante.

![illustration d’une texture 1D](images/d3d10-1d-texture.png)

Cela signifie que le tableau de texels qui composent une texture 1D est contenu dans une seule sous-ressource.

Si vous développez une texture 1D avec trois niveaux de mipmap, elle peut être visualisée comme suit.

![illustration d’une texture 1D avec des niveaux de mipmap](images/d3d10-resource-texture1d.png)

Imaginez qu’il s’agit d’une texture unique constituée de trois sous-textures. Chaque sous-texture étant considérée comme une sous-ressource, cette texture 1D contient 3 sous-ressources. Une sous-texture (ou sous-ressource) peut être indexée à l’aide du niveau de détail (LOD) pour une seule texture. Lors de l’utilisation d’un tableau de textures, l’accès à une sous-texture particulière nécessite à la fois le LOD et la texture particulière. L’API combine ces deux informations dans un seul index de sous-ressources de base zéro, comme indiqué ici.

![illustration d’un index de sous-ressources de base zéro](images/d3d10-resource-texture1darray-sub-indexing.png)

### <a name="span-idselecting_subresourcesspanspan-idselecting_subresourcesspanspan-idselecting_subresourcesspanselecting-subresources"></a><span id="Selecting_Subresources"></span><span id="selecting_subresources"></span><span id="SELECTING_SUBRESOURCES"></span>Sélection de sous-ressources

Certaines API accèdent à une ressource entière, d’autres accèdent à une partie d’une ressource. Les API qui accèdent à une partie d’une ressource utilisent généralement une description de la vue pour spécifier les sous-ressources auxquelles accéder.

Ces figures illustrent les termes utilisés par une description de la vue lors de l’accès à un tableau de textures.

### <a name="span-idarray_slicespanspan-idarray_slicespanspan-idarray_slicespanarray-slice"></a><span id="Array_Slice"></span><span id="array_slice"></span><span id="ARRAY_SLICE"></span>Tranche de tableau

À partir d’un tableau de textures, chaque texture avec des mipmaps, une tranche de tableau (représentée par le rectangle blanc) comprend une texture et toutes ses sous-textures, comme le montre l’illustration suivante.

![illustration d’une épissure de tableau](images/d3d10-resource-array-slice.png)

### <a name="span-idmip_slicespanspan-idmip_slicespanspan-idmip_slicespanmip-slice"></a><span id="Mip_Slice"></span><span id="mip_slice"></span><span id="MIP_SLICE"></span>Tranche MIP

Une tranche MIP (représentée par le rectangle blanc) comprend un niveau de mipmap pour chaque texture d’un tableau, comme indiqué dans l’illustration suivante.

![illustration d’une épissure MIP](images/d3d10-resource-mip-slice.png)

### <a name="span-idselecting_a_single_subresourcespanspan-idselecting_a_single_subresourcespanspan-idselecting_a_single_subresourcespanselecting-a-single-subresource"></a><span id="Selecting_a_Single_Subresource"></span><span id="selecting_a_single_subresource"></span><span id="SELECTING_A_SINGLE_SUBRESOURCE"></span>Sélection d’une seule sous-ressource

Vous pouvez utiliser ces deux types de tranches pour choisir une seule sous-ressource, comme indiqué dans l’illustration suivante.

![illustration du choix d’une sous-ressource à l’aide d’un segment de tableau et d’une épissure MIP](images/d3d10-resource-subresources-1.png)

### <a name="span-idselecting_multiple_subresourcesspanspan-idselecting_multiple_subresourcesspanspan-idselecting_multiple_subresourcesspanselecting-multiple-subresources"></a><span id="Selecting_Multiple_Subresources"></span><span id="selecting_multiple_subresources"></span><span id="SELECTING_MULTIPLE_SUBRESOURCES"></span>Sélection de plusieurs sous-ressources

Vous pouvez utiliser ces deux types de tranches avec le nombre de niveaux de mipmap et/ou le nombre de textures pour choisir plusieurs sous-ressources.

![illustration du choix de plusieurs sous-ressources](images/d3d10-resource-subresources-2.png)

Quel que soit le type de texture que vous utilisez, avec ou sans des mipmaps, avec ou sans tableau de texture, il existe souvent des fonctions d’assistance fournies pour calculer l’index d’une sous-ressource particulière.

### <a name="span-idtypedspanspan-idtypedspanspan-idtypedspanstrong-vs-weak-typing"></a><span id="Typed"></span><span id="typed"></span><span id="TYPED"></span>Typage fort et typage faible

La création d’une ressource entièrement typée restreint la ressource au format avec lequel elle a été créée. Cela permet au runtime d’optimiser l’accès, surtout si la ressource est créée avec des indicateurs indiquant qu’elle ne peut pas être mappée par l’application. Les ressources créées avec un type spécifique ne peuvent pas être réinterprétées à l’aide du mécanisme de vue.

Dans une ressource non typée, le type de données est inconnu lorsque la ressource est créée pour la première fois. L’application doit choisir parmi les formats de type non disponibles. Vous devez spécifier la taille de la mémoire à allouer et déterminer si le runtime doit générer les sous-textures dans un mipmap.

Toutefois, le format de données exact (si la mémoire est interprétée comme des entiers, des valeurs à virgule flottante, des entiers non signés, etc.) n’est pas déterminé tant que la ressource n’est pas liée au pipeline avec une vue. Étant donné que le format de texture reste flexible jusqu’à ce que la texture soit liée au pipeline, la ressource est appelée stockage faiblement typé. Le stockage faiblement typé présente l’avantage de pouvoir être réutilisé ou réinterprété (dans un autre format) tant que le bit de composant du nouveau format correspond au nombre de bits de l’ancien format.

Une seule ressource peut être liée à plusieurs étapes de pipeline, à condition que chacune ait une vue unique, qui qualifie entièrement les formats à chaque emplacement. Par exemple, une ressource créée avec un format sans type peut être utilisée comme format FLOAT et un format UINT à différents emplacements dans le pipeline simultanément.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Ressources](resources.md)