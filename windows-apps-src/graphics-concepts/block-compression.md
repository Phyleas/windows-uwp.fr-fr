---
title: Compression de blocs
description: La compression de bloc est une technique de compression de texture de perte pour réduire la taille de la texture et l’encombrement mémoire, ce qui augmente les performances. Une texture compressée par bloc peut être plus petite qu’une texture de 32 bits par couleur.
ms.assetid: 2FAD6BE8-C6E4-4112-AF97-419CD27F7C73
keywords:
- Compression de blocs
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 02b65357b52740e9b56e0e2dc11ff22723825f20
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156453"
---
# <a name="block-compression"></a>Compression de blocs

La compression de bloc est une technique de compression de texture de perte pour réduire la taille de la texture et l’encombrement mémoire, ce qui augmente les performances. Une texture compressée par bloc peut être plus petite qu’une texture de 32 bits par couleur.

La compression de bloc est une technique de compression de texture qui réduit la taille de la texture. En comparaison avec une texture de 32 bits par couleur, une texture compressée par bloc peut atteindre 75 pour cent de moins. Les applications voient généralement une augmentation des performances lors de l’utilisation de la compression de bloc en raison de l’encombrement mémoire plus faible.

En cas de perte, la compression de bloc fonctionne bien et est recommandée pour toutes les textures qui sont transformées et filtrées par le pipeline. Les textures directement mappées à l’écran (éléments d’interface utilisateur tels que les icônes et le texte) ne sont pas de bons choix pour la compression, car les artefacts sont plus perceptibles.

Une texture compressée par bloc doit être créée en tant que multiple de taille 4 dans toutes les dimensions et ne peut pas être utilisée comme sortie du pipeline.

## <a name="span-idbasicsspanspan-idbasicsspanspan-idbasicsspanhow-block-compression-works"></a><span id="Basics"></span><span id="basics"></span><span id="BASICS"></span>Fonctionnement de la compression de bloc

La compression par bloc est une technique qui permet de réduire la quantité de mémoire requise pour stocker des données de couleur. En stockant des couleurs de leur taille d’origine et d’autres couleurs à l’aide d’un schéma d’encodage, vous pouvez réduire considérablement la quantité de mémoire requise pour stocker l’image. Étant donné que le matériel décode automatiquement les données compressées, il n’y a aucune altération des performances pour l’utilisation des textures compressées.

Pour voir le fonctionnement de la compression, observez les deux exemples suivants. Le premier exemple décrit la quantité de mémoire utilisée pour stocker des données non compressées ; le deuxième exemple décrit la quantité de mémoire utilisée pour stocker des données compressées.

- [Stockage des données non compressées](#storing-uncompressed-data)
- [Stockage des données compressées](#storing-compressed-data)

### <a name="span-idstoring_uncompressed_dataspanspan-idstoring_uncompressed_dataspanspan-idstoring_uncompressed_dataspanspan-idstoring-uncompressed-dataspanstoring-uncompressed-data"></a><span id="Storing_Uncompressed_Data"></span><span id="storing_uncompressed_data"></span><span id="STORING_UNCOMPRESSED_DATA"></span><span id="storing-uncompressed-data"></span>Stockage des données non compressées

L’illustration suivante représente une texture 4 × 4 non compressée. Supposons que chaque couleur contient un composant de couleur unique (rouge par exemple) et qu’elle est stockée en un octet de mémoire.

![texture 4x4 non compressée](images/d3d10-block-compress-1.png)

Les données non compressées sont disposées en mémoire de façon séquentielle et nécessitent 16 octets, comme indiqué dans l’illustration suivante.

![données non compressées en mémoire séquentielle](images/d3d10-block-compress-2.png)

### <a name="span-idstoring_compressed_dataspanspan-idstoring_compressed_dataspanspan-idstoring_compressed_dataspanspan-idstoring-compressed-dataspanstoring-compressed-data"></a><span id="Storing_Compressed_Data"></span><span id="storing_compressed_data"></span><span id="STORING_COMPRESSED_DATA"></span><span id="storing-compressed-data"></span>Stockage des données compressées

Maintenant que vous avez vu la quantité de mémoire utilisée par une image non compressée, consultez la quantité de mémoire enregistrée par une image compressée. Le format de compression [BC1](#bc1) stocke 2 couleurs (1 octet chacune) et des index 16 3 bits (48 bits, ou 6 octets) qui sont utilisés pour interpoler les couleurs d’origine dans la texture, comme indiqué dans l’illustration suivante.

![format de compression BC1](images/d3d10-block-compress-3.png)

L’espace total requis pour stocker les données compressées est de 8 octets, ce qui représente une économie de mémoire de 50% par rapport à l’exemple non compressé. Les économies sont encore plus importantes lorsque plusieurs composants de couleur sont utilisés.

Les économies importantes de mémoire fournies par la compression de bloc peuvent entraîner une augmentation des performances. Cette performance est due au coût de la qualité de l’image (en raison de l’interpolation de couleur). Toutefois, la qualité inférieure n’est souvent pas perceptible.

La section suivante montre comment Direct3D permet d’utiliser la compression de bloc dans une application.

## <a name="span-idusing_block_compressionspanspan-idusing_block_compressionspanspan-idusing_block_compressionspanusing-block-compression"></a><span id="Using_Block_Compression"></span><span id="using_block_compression"></span><span id="USING_BLOCK_COMPRESSION"></span>Utilisation de la compression de bloc

Créez une texture compressée par bloc comme une texture non compressée, sauf que vous spécifiez un format compressé par bloc.

Ensuite, créez une vue pour lier la texture au pipeline, car une texture compressée par bloc ne peut être utilisée que comme entrée d’une étape de nuanceur, vous souhaitez créer un affichage des ressources de nuanceur.

Utilisez une texture de bloc compressée de la même façon que vous utilisez une texture non compressée. Si votre application obtient un pointeur de mémoire vers des données compressées par bloc, vous devez prendre en compte le remplissage de la mémoire dans un mipmap qui entraîne une différence entre la taille déclarée et la taille réelle.

- [Taille virtuelle et taille physique](#virtual-size-versus-physical-size)

### <a name="span-idvirtual_sizespanspan-idvirtual_sizespanspan-idvirtual_sizespanspan-idvirtual-size-versus-physical-sizespanvirtual-size-versus-physical-size"></a><span id="Virtual_Size"></span><span id="virtual_size"></span><span id="VIRTUAL_SIZE"></span><span id="virtual-size-versus-physical-size"></span>Taille virtuelle et taille physique

Si vous avez un code d’application qui utilise un pointeur de mémoire pour parcourir la mémoire d’une texture de bloc compressée, il y a une considération importante qui peut nécessiter une modification dans le code de votre application. Une texture compressée par bloc doit être un multiple de 4 dans toutes les dimensions, car les algorithmes de compression de bloc opèrent sur des blocs texels 4x4. Il s’agit d’un problème pour un mipmap dont les dimensions initiales sont divisible par 4, mais pas les niveaux subdivisés. Le diagramme suivant montre la différence de zone entre la taille virtuelle (déclarée) et la taille physique (réelle) de chaque niveau de mipmap.

![niveaux de mipmap non compressés et compressés](images/d3d10-block-compress-pad.png)

Le côté gauche du diagramme montre les tailles de niveaux de mipmap générées pour une texture 60 × 40 non compressée. La taille de niveau supérieur est tirée de l’appel d’API qui génère la texture. chaque niveau suivant correspond à la moitié de la taille du niveau précédent. Pour une texture non compressée, il n’existe aucune différence entre la taille virtuelle (déclarée) et la taille physique (réelle).

La partie droite du diagramme montre les tailles de niveaux mipmap générées pour la même texture 60 × 40 avec compression. Notez que les deuxième et troisième niveaux ont une marge intérieure de la mémoire pour que les facteurs de taille de 4 soient à chaque niveau. Cela est nécessaire pour que les algorithmes puissent fonctionner sur des blocs Texel 4 × 4. Cela est particulièrement évident si vous considérez des niveaux de mipmap inférieurs à 4 × 4 ; la taille de ces très petits niveaux de mipmap est arrondie au facteur le plus proche de 4 lorsque la mémoire de texture est allouée.

Le matériel d’échantillonnage utilise la taille virtuelle ; Lorsque la texture est échantillonnée, le remplissage de la mémoire est ignoré. Pour les niveaux de mipmap inférieurs à 4 × 4, seules les quatre premières texels seront utilisées pour une carte 2 × 2, et seul le premier Texel sera utilisé par un bloc 1 × 1. Toutefois, il n’existe aucune structure d’API qui expose la taille physique (y compris le remplissage de la mémoire).

En résumé, veillez à utiliser des blocs de mémoire alignés lors de la copie des régions qui contiennent des données compressées par blocs. Pour effectuer cette opération dans une application qui obtient un pointeur de mémoire, assurez-vous que le pointeur utilise le pas de la surface pour prendre en compte la taille de la mémoire physique.

## <a name="span-idcompression_algorithmsspanspan-idcompression_algorithmsspanspan-idcompression_algorithmsspancompression-algorithms"></a><span id="Compression_Algorithms"></span><span id="compression_algorithms"></span><span id="COMPRESSION_ALGORITHMS"></span>Algorithmes de compression

Les techniques de compression de bloc dans Direct3D fractionnent les données de texture non compressées en blocs de 4 × 4, compressent chaque bloc, puis stockent les données. Pour cette raison, les textures censées être compressées doivent avoir des dimensions de texture multiples de 4.

![compression de bloc](images/d3d10-compression-1.png)

Le diagramme précédent montre une texture partitionnée en blocs Texel. Le premier bloc affiche la disposition des 16 texels étiquetés a-p, mais chaque bloc a la même organisation de données.

Direct3D implémente plusieurs schémas de compression, chacun d’entre eux implémentant un compromis différent entre le nombre de composants stockés, le nombre de bits par composant et la quantité de mémoire consommée. Utilisez ce tableau pour vous aider à choisir le format le mieux adapté au type de données et à la résolution de données qui convient le mieux à votre application.

| Données sources                     | Résolution de la compression des données (en bits) | Choisir ce format de compression |
|---------------------------------|---------------------------------------|--------------------------------|
| Couleur à trois composants et alpha | Couleur (5:6:5), alpha (1) ou non alpha  | [BC1](#bc1)                    |
| Couleur à trois composants et alpha | Couleur (5:6:5), alpha (4)              | [BC2](#bc2)                    |
| Couleur à trois composants et alpha | Couleur (5:6:5), alpha (8)              | [BC3](#bc3)                    |
| Couleur d’un composant             | Un composant (8)                     | [TEXTURES BC4](#bc4)                    |
| Couleur à deux composants             | Deux composants (8:8)                  | [BC5](#bc5)                    |

- [BC1](#bc1)
- [BC2](#bc2)
- [BC3](#bc3)
- [TEXTURES BC4](#bc4)
- [BC5](#bc5)

### <a name="span-idbc1spanspan-idbc1spanbc1"></a><span id="BC1"></span><span id="bc1"></span>BC1

Utilisez le premier format de compression de bloc (BC1) (DXGI \_ format \_ BC1 \_ untype, DXGI \_ format BC1 UNORM ou dxgi BC1 UNORM \_ \_ \_ \_ \_ sRGB) pour stocker les données de couleur à trois composants à l’aide d’une couleur 5:6:5 (5 bits rouge, 6 bits vert, 5 bits bleus). Cela est vrai même si les données contiennent également une valeur alpha de 1 bit. En supposant une texture 4 × 4 utilisant le format de données le plus grand possible, le format BC1 réduit la mémoire requise de 48 octets (16 couleurs × 3 composants/couleur × 1 octet/composant) à 8 octets de mémoire.

L’algorithme fonctionne sur 4 × 4 blocs de texels. Au lieu de stocker 16 couleurs, l’algorithme enregistre 2 couleurs de référence (couleur \_ 0 et couleur \_ 1) et des index de couleurs 16 2 bits (bloque a – p), comme illustré dans le diagramme suivant.

![disposition de la compression BC1](images/d3d10-compression-bc1.png)

Les index de couleurs (a – p) sont utilisés pour rechercher les couleurs d’origine d’une table des couleurs. La table des couleurs contient 4 couleurs. Les deux premières couleurs (couleur \_ 0 et couleur \_ 1) représentent les couleurs minimales et maximales. Les deux autres couleurs, Color \_ 2 et Color \_ 3, sont des couleurs intermédiaires calculées à l’aide d’une interpolation linéaire.

```cpp
color_2 = 2/3*color_0 + 1/3*color_1
color_3 = 1/3*color_0 + 2/3*color_1
```

Les quatre couleurs reçoivent des valeurs d’index de 2 bits qui seront enregistrées dans les blocs a – p.

```cpp
color_0 = 00
color_1 = 01
color_2 = 10
color_3 = 11
```

Enfin, chacune des couleurs des blocs a à p est comparée aux quatre couleurs de la table des couleurs, et l’index de la couleur la plus proche est stocké dans les blocs de 2 bits.

Cet algorithme se prête aux données qui contiennent également une valeur alpha de 1 bit. La seule différence est que la couleur \_ 3 est définie sur 0 (qui représente une couleur transparente) et que la couleur \_ 2 est un mélange linéaire de la couleur \_ 0 et de la couleur \_ 1.

```cpp
color_2 = 1/2*color_0 + 1/2*color_1;
color_3 = 0;
```

### <a name="span-idbc2spanspan-idbc2spanbc2"></a><span id="BC2"></span><span id="bc2"></span>BC2

Utilisez le format BC2 (DXGI \_ format \_ BC2 \_ sans type, DXGI format BC2 UNORM ou dxgi BC2 UNORM BC3 \_ \_ \_ \_ \_ \_ ) pour stocker les données qui contiennent des données de couleur et alpha avec une faible cohérence (utilisez [BC3](#bc3) pour les données alpha hautement cohérentes). Le format BC2 stocke les données RVB sous la forme d’une couleur 5:6:5 (5 bits rouge, 6 bits vert, 5 bits bleus) et alpha comme valeur 4 bits distincte. En supposant une texture 4 × 4 utilisant le plus grand format de données possible, cette technique de compression réduit la mémoire requise de 64 octets (16 couleurs × 4 composants/couleur × 1 octet/composant) à 16 octets de mémoire.

Le format BC2 stocke les couleurs avec le même nombre de bits et la même disposition de données que le format [BC1](#bc1) ; Toutefois, BC2 requiert une mémoire supplémentaire de 64 bits pour stocker les données alpha, comme indiqué dans le diagramme suivant.

![disposition de la compression BC2](images/d3d10-compression-bc2.png)

### <a name="span-idbc3spanspan-idbc3spanbc3"></a><span id="BC3"></span><span id="bc3"></span>BC3

Utilisez le format BC3 (DXGI \_ format \_ BC3 \_ sans type, DXGI format BC3 UNORM ou dxgi BC3 UNORM BC2 \_ \_ \_ \_ \_ \_ ) pour stocker des données de couleur hautement cohérentes (utilisez [BC2](#bc2) avec des données alpha moins cohérentes). Le format BC3 stocke les données de couleur à l’aide de la couleur 5:6:5 (5 bits rouge, 6 bits vert, 5 bits bleus) et des données alpha à l’aide d’un octet. En supposant une texture 4 × 4 utilisant le plus grand format de données possible, cette technique de compression réduit la mémoire requise de 64 octets (16 couleurs × 4 composants/couleur × 1 octet/composant) à 16 octets de mémoire.

Le format BC3 stocke les couleurs avec le même nombre de bits et la même disposition de données que le format [BC1](#bc1) ; Toutefois, BC3 requiert une mémoire supplémentaire de 64 bits pour stocker les données alpha. Le format BC3 gère l’alpha en stockant deux valeurs de référence et en les interpolant entre elles (de la même façon que BC1 stocke la couleur RVB).

L’algorithme fonctionne sur 4 × 4 blocs de texels. Au lieu de stocker 16 valeurs alpha, l’algorithme stocke 2 index alpha de référence (alpha \_ 0 et alpha \_ 1) et des index de couleur 16 3 bits (alpha a à p), comme indiqué dans le diagramme suivant.

![disposition de la compression BC3](images/d3d10-compression-bc3.png)

Le format BC3 utilise les indices alpha (a – p) pour rechercher les couleurs d’origine d’une table de recherche qui contient 8 valeurs. Les deux premières valeurs (alpha \_ 0 et alpha \_ 1) sont les valeurs minimale et maximale ; les six autres valeurs intermédiaires sont calculées à l’aide d’une interpolation linéaire.

L’algorithme détermine le nombre de valeurs alpha interpolées en examinant les deux valeurs alpha de référence. Si alpha \_ 0 est supérieur à alpha \_ 1, BC3 interpole 6 valeurs alpha ; sinon, il interpole 4. Quand BC3 n’interpole que 4 valeurs alpha, il définit deux valeurs alpha supplémentaires (0 pour la transparence intégrale et 255 pour le total opaque). BC3 compresse les valeurs alpha dans la zone de Texel 4 × 4 en stockant le code binaire correspondant aux valeurs alpha interpolées qui correspond le mieux à l’alpha d’origine pour un Texel donné.

```cpp
if( alpha_0 > alpha_1 )
{
  // 6 interpolated alpha values.
  alpha_2 = 6/7*alpha_0 + 1/7*alpha_1; // bit code 010
  alpha_3 = 5/7*alpha_0 + 2/7*alpha_1; // bit code 011
  alpha_4 = 4/7*alpha_0 + 3/7*alpha_1; // bit code 100
  alpha_5 = 3/7*alpha_0 + 4/7*alpha_1; // bit code 101
  alpha_6 = 2/7*alpha_0 + 5/7*alpha_1; // bit code 110
  alpha_7 = 1/7*alpha_0 + 6/7*alpha_1; // bit code 111
}
else
{
  // 4 interpolated alpha values.
  alpha_2 = 4/5*alpha_0 + 1/5*alpha_1; // bit code 010
  alpha_3 = 3/5*alpha_0 + 2/5*alpha_1; // bit code 011
  alpha_4 = 2/5*alpha_0 + 3/5*alpha_1; // bit code 100
  alpha_5 = 1/5*alpha_0 + 4/5*alpha_1; // bit code 101
  alpha_6 = 0;                         // bit code 110
  alpha_7 = 255;                       // bit code 111
}
```

### <a name="span-idbc4spanspan-idbc4spanbc4"></a><span id="BC4"></span><span id="bc4"></span>TEXTURES BC4

Utilisez le format textures BC4 pour stocker les données de couleur d’un composant à l’aide de 8 bits pour chaque couleur. En raison de l’exactitude accrue (par rapport à [BC1](#bc1)), textures BC4 est idéal pour le stockage des données à virgule flottante dans la plage de \[ 0 à 1 à l' \] aide du format dxgi \_ \_ textures BC4 \_ UNORM et de \[ -1 à + 1 à l' \] aide du format dxgi \_ \_ textures BC4 \_ ronfler. En supposant une texture 4 × 4 utilisant le plus grand format de données possible, cette technique de compression réduit la mémoire requise de 16 octets (16 couleurs × 1 composant/couleur × 1 octet/composant) à 8 octets.

L’algorithme fonctionne sur 4 × 4 blocs de texels. Au lieu de stocker 16 couleurs, l’algorithme stocke 2 couleurs de référence (rouge \_ 0 et rouge \_ 1) et les index de couleurs 16 3 bits (rouge a à rouge p), comme indiqué dans le diagramme suivant.

![disposition de la compression textures BC4](images/d3d10-compression-bc4.png)

L’algorithme utilise les index de 3 bits pour rechercher des couleurs dans une table de couleurs qui contient 8 couleurs. Les deux premières couleurs (rouge \_ 0 et rouge \_ 1) représentent les couleurs minimales et maximales. L’algorithme calcule les couleurs restantes à l’aide d’une interpolation linéaire.

L’algorithme détermine le nombre de valeurs de couleur interpolées en examinant les deux valeurs de référence. Si \_ le 0 rouge est supérieur à \_ 1 rouge, textures BC4 interpole 6 valeurs de couleur ; sinon, il interpole 4. Quand textures BC4 n’interpole que 4 valeurs de couleur, il définit deux valeurs de couleur supplémentaires (0.0 f pour entièrement transparent et 1,0 f pour entièrement opaque). TEXTURES BC4 compresse les valeurs alpha dans la zone de Texel 4 × 4 en stockant le code binaire correspondant aux valeurs alpha interpolées qui correspond le mieux à l’alpha d’origine pour un Texel donné.

- [TEXTURES BC4 \_ UNORM](#bc4-unorm)
- [TEXTURES BC4 \_ ronfler](#bc4-snorm)

### <a name="span-idbc4_unormspanspan-idbc4_unormspanspan-idbc4-unormspanbc4_unorm"></a><span id="BC4_UNORM"></span><span id="bc4_unorm"></span><span id="bc4-unorm"></span>TEXTURES BC4 \_ UNORM

L’interpolation des données à composant unique s’effectue comme dans l’exemple de code suivant.

```cpp
unsigned word red_0, red_1;

if( red_0 > red_1 )
{
  // 6 interpolated color values
  red_2 = (6*red_0 + 1*red_1)/7.0f; // bit code 010
  red_3 = (5*red_0 + 2*red_1)/7.0f; // bit code 011
  red_4 = (4*red_0 + 3*red_1)/7.0f; // bit code 100
  red_5 = (3*red_0 + 4*red_1)/7.0f; // bit code 101
  red_6 = (2*red_0 + 5*red_1)/7.0f; // bit code 110
  red_7 = (1*red_0 + 6*red_1)/7.0f; // bit code 111
}
else
{
  // 4 interpolated color values
  red_2 = (4*red_0 + 1*red_1)/5.0f; // bit code 010
  red_3 = (3*red_0 + 2*red_1)/5.0f; // bit code 011
  red_4 = (2*red_0 + 3*red_1)/5.0f; // bit code 100
  red_5 = (1*red_0 + 4*red_1)/5.0f; // bit code 101
  red_6 = 0.0f;                     // bit code 110
  red_7 = 1.0f;                     // bit code 111
}
```

Les index de 3 bits sont affectés aux couleurs de référence (000 – 111, car il y a 8 valeurs), qui seront enregistrées dans des blocs de a à rouge p au cours de la compression.

### <a name="span-idbc4_snormspanspan-idbc4_snormspanspan-idbc4-snormspanbc4_snorm"></a><span id="BC4_SNORM"></span><span id="bc4_snorm"></span><span id="bc4-snorm"></span>TEXTURES BC4 \_ ronfler

Le \_ format dxgi \_ textures BC4 \_ ronfler est exactement le même, sauf que les données sont encodées dans une plage de ronflements et lorsque 4 valeurs de couleur sont interpolées. L’interpolation des données à composant unique s’effectue comme dans l’exemple de code suivant.

```cpp
signed word red_0, red_1;

if( red_0 > red_1 )
{
  // 6 interpolated color values
  red_2 = (6*red_0 + 1*red_1)/7.0f; // bit code 010
  red_3 = (5*red_0 + 2*red_1)/7.0f; // bit code 011
  red_4 = (4*red_0 + 3*red_1)/7.0f; // bit code 100
  red_5 = (3*red_0 + 4*red_1)/7.0f; // bit code 101
  red_6 = (2*red_0 + 5*red_1)/7.0f; // bit code 110
  red_7 = (1*red_0 + 6*red_1)/7.0f; // bit code 111
}
else
{
  // 4 interpolated color values
  red_2 = (4*red_0 + 1*red_1)/5.0f; // bit code 010
  red_3 = (3*red_0 + 2*red_1)/5.0f; // bit code 011
  red_4 = (2*red_0 + 3*red_1)/5.0f; // bit code 100
  red_5 = (1*red_0 + 4*red_1)/5.0f; // bit code 101
  red_6 = -1.0f;                    // bit code 110
  red_7 =  1.0f;                    // bit code 111
}
```

Les index de 3 bits sont affectés aux couleurs de référence (000 – 111, car il y a 8 valeurs), qui seront enregistrées dans des blocs de a à rouge p au cours de la compression.

### <a name="span-idbc5spanspan-idbc5spanbc5"></a><span id="BC5"></span><span id="bc5"></span>BC5

Utilisez le format BC5 pour stocker les données de couleur à deux composants à l’aide de 8 bits pour chaque couleur. En raison de l’exactitude accrue (par rapport à [BC1](#bc1)), BC5 est idéal pour le stockage des données à virgule flottante dans la plage de \[ 0 à 1 à l' \] aide du format dxgi \_ \_ BC5 \_ UNORM et de \[ -1 à + 1 à l' \] aide du format dxgi \_ \_ BC5 \_ ronfler. En supposant une texture 4 × 4 utilisant le plus grand format de données possible, cette technique de compression réduit la mémoire requise de 32 octets (16 couleurs × 2 composants/couleur × 1 octet/composant) à 16 octets.

- [BC5 \_ UNORM](#bc5-unorm)
- [BC5 \_ ronfler](#bc5-snorm)

L’algorithme fonctionne sur 4 × 4 blocs de texels. Au lieu de stocker 16 couleurs pour les deux composants, l’algorithme stocke 2 couleurs de référence pour chaque composant (rouge \_ 0, rouge \_ 1, vert \_ 0 et vert \_ 1) et les index de couleurs 16 3 bits pour chaque composant (rouge a à p rouge et vert a à vert p), comme illustré dans le diagramme suivant.

![disposition de la compression BC5](images/d3d10-compression-bc5.png)

L’algorithme utilise les index de 3 bits pour rechercher des couleurs dans une table de couleurs qui contient 8 couleurs. Les deux premières couleurs (rouge \_ 0 et rouge \_ 1 (ou vert \_ 0 et vert \_ 1) sont les couleurs minimales et maximales. L’algorithme calcule les couleurs restantes à l’aide d’une interpolation linéaire.

L’algorithme détermine le nombre de valeurs de couleur interpolées en examinant les deux valeurs de référence. Si \_ le 0 rouge est supérieur à \_ 1 rouge, BC5 interpole 6 valeurs de couleur ; sinon, il interpole 4. Quand BC5 interpole uniquement 4 valeurs de couleur, il définit les deux autres valeurs de couleur à 0.0 f et 1,0 f.

### <a name="span-idbc5_unormspanspan-idbc5_unormspanspan-idbc5-unormspanbc5_unorm"></a><span id="BC5_UNORM"></span><span id="bc5_unorm"></span><span id="bc5-unorm"></span>BC5 \_ UNORM

L’interpolation des données à composant unique s’effectue comme dans l’exemple de code suivant. Les calculs pour les composants verts sont similaires.

```cpp
unsigned word red_0, red_1;

if( red_0 > red_1 )
{
  // 6 interpolated color values
  red_2 = (6*red_0 + 1*red_1)/7.0f; // bit code 010
  red_3 = (5*red_0 + 2*red_1)/7.0f; // bit code 011
  red_4 = (4*red_0 + 3*red_1)/7.0f; // bit code 100
  red_5 = (3*red_0 + 4*red_1)/7.0f; // bit code 101
  red_6 = (2*red_0 + 5*red_1)/7.0f; // bit code 110
  red_7 = (1*red_0 + 6*red_1)/7.0f; // bit code 111
}
else
{
  // 4 interpolated color values
  red_2 = (4*red_0 + 1*red_1)/5.0f; // bit code 010
  red_3 = (3*red_0 + 2*red_1)/5.0f; // bit code 011
  red_4 = (2*red_0 + 3*red_1)/5.0f; // bit code 100
  red_5 = (1*red_0 + 4*red_1)/5.0f; // bit code 101
  red_6 = 0.0f;                     // bit code 110
  red_7 = 1.0f;                     // bit code 111
}
```

Les index de 3 bits sont affectés aux couleurs de référence (000 – 111, car il y a 8 valeurs), qui seront enregistrées dans des blocs de a à rouge p au cours de la compression.

### <a name="span-idbc5_snormspanspan-idbc5_snormspanspan-idbc5-snormspanbc5_snorm"></a><span id="BC5_SNORM"></span><span id="bc5_snorm"></span><span id="bc5-snorm"></span>BC5 \_ ronfler

Le \_ format dxgi \_ BC5 \_ ronfler est exactement le même, sauf que les données sont encodées dans une plage de ronflements et lorsque 4 valeurs de données sont interpolées, les deux valeurs supplémentaires sont-1.0 f et 1.0 f. L’interpolation des données à composant unique s’effectue comme dans l’exemple de code suivant. Les calculs pour les composants verts sont similaires.

```cpp
signed word red_0, red_1;

if( red_0 > red_1 )
{
  // 6 interpolated color values
  red_2 = (6*red_0 + 1*red_1)/7.0f; // bit code 010
  red_3 = (5*red_0 + 2*red_1)/7.0f; // bit code 011
  red_4 = (4*red_0 + 3*red_1)/7.0f; // bit code 100
  red_5 = (3*red_0 + 4*red_1)/7.0f; // bit code 101
  red_6 = (2*red_0 + 5*red_1)/7.0f; // bit code 110
  red_7 = (1*red_0 + 6*red_1)/7.0f; // bit code 111
}
else
{
  // 4 interpolated color values
  red_2 = (4*red_0 + 1*red_1)/5.0f; // bit code 010
  red_3 = (3*red_0 + 2*red_1)/5.0f; // bit code 011
  red_4 = (2*red_0 + 3*red_1)/5.0f; // bit code 100
  red_5 = (1*red_0 + 4*red_1)/5.0f; // bit code 101
  red_6 = -1.0f;                    // bit code 110
  red_7 =  1.0f;                    // bit code 111
}
```

Les index de 3 bits sont affectés aux couleurs de référence (000 – 111, car il y a 8 valeurs), qui seront enregistrées dans des blocs de a à rouge p au cours de la compression.

## <a name="span-iddifferencesspanspan-iddifferencesspanspan-iddifferencesspanformat-conversion"></a><span id="Differences"></span><span id="differences"></span><span id="DIFFERENCES"></span>Conversion de format

Direct3D permet de copier les textures de type préstructurées et les textures compressées par bloc des mêmes largeurs de bits.

Vous pouvez copier des ressources entre quelques types de format. Ce type d’opération de copie effectue un type de conversion de format qui réinterprète les données de ressources comme un type de format différent. Prenons l’exemple suivant qui illustre la différence entre la réinterprétation des données et le comportement d’un type de conversion plus classique :

```cpp
FLOAT32 f = 1.0f;
UINT32 u;
```

Pour réinterpréter « f » comme type de « u », utilisez [memcpy](/cpp/c-runtime-library/reference/memcpy-wmemcpy):

```cpp
memcpy( &u, &f, sizeof( f ) ); // 'u' becomes equal to 0x3F800000.
```

Dans la réinterprétation précédente, la valeur sous-jacente des données ne change pas. [memcpy](/cpp/c-runtime-library/reference/memcpy-wmemcpy) réinterprète le float comme un entier non signé.

Pour effectuer le type de conversion le plus courant, utilisez l’assignation :

```cpp
u = f; // 'u' becomes 1.
```

Dans la conversion précédente, la valeur sous-jacente des données change.

Le tableau suivant répertorie les formats autorisés de source et de destination que vous pouvez utiliser dans ce type de réinterprétation de conversion de format. Vous devez encoder les valeurs correctement pour que la réinterprétation fonctionne comme prévu.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Largeur de bit</th>
<th align="left">Ressource non compressée</th>
<th align="left">Ressource compressée par bloc</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">32</td>
<td align="left"><p>DXGI_FORMAT_R32_UINT</p>
<p>DXGI_FORMAT_R32_SINT</p></td>
<td align="left">DXGI_FORMAT_R9G9B9E5_SHAREDEXP</td>
</tr>
<tr class="even">
<td align="left">64</td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_UINT</p>
<p>DXGI_FORMAT_R16G16B16A16_SINT</p>
<p>DXGI_FORMAT_R32G32_UINT</p>
<p>DXGI_FORMAT_R32G32_SINT</p></td>
<td align="left"><p>DXGI_FORMAT_BC1_UNORM [_SRGB]</p>
<p>DXGI_FORMAT_BC4_UNORM</p>
<p>DXGI_FORMAT_BC4_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left">128</td>
<td align="left"><p>DXGI_FORMAT_R32G32B32A32_UINT</p>
<p>DXGI_FORMAT_R32G32B32A32_SINT</p></td>
<td align="left"><p>DXGI_FORMAT_BC2_UNORM [_SRGB]</p>
<p>DXGI_FORMAT_BC3_UNORM [_SRGB]</p>
<p>DXGI_FORMAT_BC5_UNORM</p>
<p>DXGI_FORMAT_BC5_SNORM</p></td>
</tr>
</tbody>
</table>

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes

[Ressources de texture compressées](compressed-texture-resources.md)