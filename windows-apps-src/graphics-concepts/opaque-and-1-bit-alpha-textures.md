---
title: Textures opaques et alpha 1 bit
description: Le format de texture BC1 est pour les textures opaques ou qui ont une seule couleur transparente.
ms.assetid: 8C53ACDD-72ED-4307-B4F3-2FCF9A9F53EC
keywords:
- Textures opaques et alpha 1 bit
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c08489055bfd4e867310c5bdec6fea655ddc6d41
ms.sourcegitcommit: 53c00939b20d4b0a294936df3d395adb0c13e231
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91933010"
---
# <a name="span-iddirect3dconceptsopaque_and_1-bit_alpha_texturesspanopaque-and-1-bit-alpha-textures"></a><span id="direct3dconcepts.opaque_and_1-bit_alpha_textures"></span>Textures opaques et alpha 1 bit

Le format de texture BC1 est pour les textures opaques ou qui ont une seule couleur transparente.

Pour chaque bloc alpha opaque ou 1 bit, les valeurs 2 16 bits (format RVB 5:6:5) et une image bitmap 4x4 avec 2 bits par pixel sont stockées. Cela totalise 64 bits pour 16 texels, ou quatre bits par Texel. Dans la bitmap de bloc, il y a 2 bits par Texel pour choisir entre les quatre couleurs, deux d’entre elles étant stockées dans les données encodées. Les deux autres couleurs sont dérivées de ces couleurs stockées par interpolation linéaire. Cette disposition est présentée dans le diagramme suivant.

![diagramme de la disposition bitmap](images/colors1.png)

Le format alpha de 1 bit est distingué du format opaque en comparant les valeurs de couleur 2 16 bits stockées dans le bloc. Ils sont traités comme des entiers non signés. Si la première couleur est supérieure à la seconde, cela implique que seules les éléments de texture opaques sont définis. Cela signifie que quatre couleurs sont utilisées pour représenter les texels. Dans un encodage en quatre couleurs, il existe deux couleurs dérivées et les quatre couleurs sont également réparties dans l’espace de couleurs RVB. Ce format est similaire au format RVB 5:6:5. Sinon, pour la transparence alpha de 1 bit, trois couleurs sont utilisées et le quatrième est réservé pour représenter des texels transparents.

Dans un encodage en trois couleurs, il existe une couleur dérivée et le quatrième code 2 bits est réservé pour indiquer une Texel transparente (informations alpha). Ce format est analogue à RVBA 5:5:5:1, où le bit final est utilisé pour l’encodage du masque Alpha.

L’exemple de code suivant illustre l’algorithme permettant de déterminer si l’encodage à trois ou quatre couleurs est sélectionné :

```cpp
if (color_0 > color_1) 
{
    // Four-color block: derive the other two colors. 
    
    // 00 = color_0, 01 = color_1, 10 = color_2, 11 = color_3
    // These 2-bit codes correspond to the 2-bit fields 
    // stored in the 64-bit block.
    color_2 = (2 * color_0 + color_1 + 1) / 3;
    color_3 = (color_0 + 2 * color_1 + 1) / 3;
}    
else
{ 
    // Three-color block: derive the other color.
    // 00 = color_0,  01 = color_1,  10 = color_2,  
    // 11 = transparent.
    // These 2-bit codes correspond to the 2-bit fields 
    // stored in the 64-bit block. 
    color_2 = (color_0 + color_1) / 2;    
    color_3 = transparent;    

}
```

Il est recommandé de définir les composants RVBA du pixel de transparence sur zéro avant la fusion.

Les tableaux suivants indiquent la disposition de la mémoire pour le bloc de 8 octets. Il est supposé que le premier index correspond à la coordonnée y et le second correspond à la coordonnée x. Par exemple, Texel \[ 1 \] \[ 2 \] fait référence au pixel de la carte de texture à (x, y) = (2, 1).

Voici la disposition de la mémoire pour le bloc de 8 octets (64 bits) :

| Adresse du mot | mot 16 bits    |
|--------------|----------------|
| 0            | Couleur \_ 0       |
| 1            | Couleur \_ 1       |
| 2            | Mot bitmap \_ 0 |
| 3            | Mot bitmap \_ 1 |

 

\_La couleur 0 et \_ la couleur 1, les couleurs aux deux extrêmes sont présentées comme suit :

| Bits        | Couleur                 |
|-------------|-----------------------|
| 4:0 (LSB \* ) | Composant de couleur bleu  |
| 10:5        | Composant de couleur verte |
| 15:11       | Composant de couleur rouge   |

 

\*bit le moins significatif

Le mot bitmap \_ 0 est disposé comme suit :

| Bits          | Texel           |
|---------------|-----------------|
| 1:0 (LSB)     | Texel \[ 0 \] \[ 0\] |
| 3:2           | Texel \[ 0 \] \[ 1\] |
| 5:4           | Texel \[ 0 \] \[ 2\] |
| 7:6           | Texel \[ 0 \] \[ 3\] |
| 9:8           | Texel \[ 1 \] \[ 0\] |
| 11:10         | Texel \[ 1 \] \[ 1\] |
| 13:12         | Texel \[ 1 \] \[ 2\] |
| 15:14 (MSB \* ) | Texel \[ 1 \] \[ 3\] |

 

\*bit le plus significatif (MSB)

Le mot bitmap \_ 1 est disposé comme suit :

| Bits        | Texel           |
|-------------|-----------------|
| 1:0 (LSB)   | Texel \[ 2 \] \[ 0\] |
| 3:2         | Texel \[ 2 \] \[ 1\] |
| 5:4         | Texel \[ 2 \] \[ 2\] |
| 7:6         | Texel \[ 2 \] \[ 3\] |
| 9:8         | Texel \[ 3 \] \[ 0\] |
| 11:10       | Texel \[ 3 \] \[ 1\] |
| 13:12       | Texel \[ 3 \] \[ 2\] |
| 15:14 (MSB) | Texel \[ 3 \] \[ 3\] |

 

## <a name="span-idexample_of_opaque_color_encodingspanspan-idexample_of_opaque_color_encodingspanspan-idexample_of_opaque_color_encodingspanexample-of-opaque-color-encoding"></a><span id="Example_of_Opaque_Color_Encoding"></span><span id="example_of_opaque_color_encoding"></span><span id="EXAMPLE_OF_OPAQUE_COLOR_ENCODING"></span>Exemple d’encodage de couleur opaque


En guise d’exemple d’encodage opaque, supposons que les couleurs rouge et noir sont à l’extrême. Le rouge est la couleur \_ 0 et le noir est la couleur \_ 1. Quatre couleurs interpolées forment le dégradé uniformément réparti entre eux. Pour déterminer les valeurs de la bitmap 4x4, les calculs suivants sont utilisés :

```cpp
00 ? color_0
01 ? color_1
10 ? 2/3 color_0 + 1/3 color_1
11 ? 1/3 color_0 + 2/3 color_1
```

Le bitmap ressemble alors au diagramme suivant.

![Diagramme de la mise en page bitmap développée pour le rouge et le noir.](images/colors2.png)

Cela ressemble à la série de couleurs illustrée ci-dessous.

**Remarque**    Dans une image, pixel (0,0) apparaît en haut à gauche.

 

![illustration d’un dégradé encodé opaque](images/redsquares.png)

## <a name="span-idexample_of_1_bit_alpha_encodingspanspan-idexample_of_1_bit_alpha_encodingspanspan-idexample_of_1_bit_alpha_encodingspanexample-of-1-bit-alpha-encoding"></a><span id="Example_of_1_Bit_Alpha_Encoding"></span><span id="example_of_1_bit_alpha_encoding"></span><span id="EXAMPLE_OF_1_BIT_ALPHA_ENCODING"></span>Exemple d’encodage alpha 1 bits


Ce format est sélectionné lorsque l’entier non signé 16 bits, Color \_ 0, est inférieur à l’entier 16 bits non signé, Color \_ 1. Un exemple de ce format peut être utilisé dans une arborescence, par rapport à un ciel bleu. Certains texels peuvent être marqués comme étant transparents alors que trois nuances de vert sont toujours disponibles pour les feuilles. Deux couleurs corrigent les extrêmes, tandis que la troisième est une couleur interpolée.

L’illustration suivante est un exemple d’une telle image.

![illustration de l’encodage alpha 1 bits](images/greenthing.png)

Lorsque l’image est affichée en blanc, le Texel est encodé comme transparent. Les composants RVBA des texels transparents doivent être définis sur zéro avant la fusion.

L’encodage bitmap des couleurs et de la transparence est déterminé à l’aide des calculs suivants.

```cpp
00 ? color_0
01 ? color_1
10 ? 1/2 color_0 + 1/2 color_1
11   ?   Transparent
```

Le bitmap ressemble alors au diagramme suivant.

![Diagramme de la disposition bitmap développée pour le vert plus clair et le vert plus sombre.](images/colors3.png)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Ressources de texture compressées](compressed-texture-resources.md)

 

 




