---
title: Compression de blocs de texture
description: La prise en charge de la compression par bloc (BC) des textures a été étendue dans Direct3D 11 pour inclure les algorithmes BC6H et BC7.
ms.assetid: 63506C46-BF14-464B-B20C-8B8F359E7AFE
keywords:
- Compression de blocs de texture
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 06c132f23d22dc688f82ff7976bcb93108f2ee1c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158813"
---
# <a name="texture-block-compression"></a>Compression de blocs de texture


La prise en charge de la compression par bloc (BC) des textures a été étendue dans Direct3D 11 pour inclure les algorithmes BC6H et BC7. BC6H prend en charge les données sources de plage haute dynamique, et BC7 fournit une compression de qualité supérieure à la moyenne avec moins d’artefacts pour les données sources RGB standard.

Pour plus d’informations sur la prise en charge des algorithmes de compression de bloc avant Direct3D 11, y compris la prise en charge des BC1 via les formats BC5, consultez [compression par bloc (Direct3D 10)](/windows/desktop/direct3d10/d3d10-graphics-programming-guide-resources-block-compression).

**Remarque à propos des formats de fichiers :  ** Les formats de compression de texture BC6H et BC7 utilisent le format de fichier DDS pour stocker les données de texture compressées. Pour plus d’informations, consultez le [Guide de programmation de DDS](/windows/desktop/direct3ddds/dx-graphics-dds-pguide) pour plus d’informations.

## <a name="span-idblock_compression_formats_supported_in_direct3d_11spanspan-idblock_compression_formats_supported_in_direct3d_11spanspan-idblock_compression_formats_supported_in_direct3d_11spanblock-compression-formats-supported-in-direct3d-11"></a><span id="Block_Compression_Formats_Supported_in_Direct3D_11"></span><span id="block_compression_formats_supported_in_direct3d_11"></span><span id="BLOCK_COMPRESSION_FORMATS_SUPPORTED_IN_DIRECT3D_11"></span>Formats de compression de bloc pris en charge dans Direct3D 11


| Données source                                  | Résolution de compression de données minimale requise                              | Format recommandé | Niveau de fonctionnalité minimal pris en charge |
|----------------------------------------------|---------------------------------------------------------------------------|--------------------|---------------------------------|
| Couleur à trois canaux avec canal alpha       | Trois canaux de couleur (5 bits : 6 bits : 5 bits), avec 0 ou 1 bit (s) d’alpha  | BC1                | Direct3D 9,1                    |
| Couleur à trois canaux avec canal alpha       | Trois canaux de couleur (5 bits : 6 bits : 5 bits), avec 4 bits d’alpha         | BC2                | Direct3D 9,1                    |
| Couleur à trois canaux avec canal alpha       | Trois canaux de couleur (5 bits : 6 bits : 5 bits) avec 8 bits d’alpha          | BC3                | Direct3D 9,1                    |
| Couleur à un canal                            | Un canal de couleur (8 bits)                                                | TEXTURES BC4                | Direct3D 10                     |
| Couleur à deux canaux                            | Deux canaux de couleur (8 bits : 8 bits)                                        | BC5                | Direct3D 10                     |
| Couleur de plage dynamique élevée (HDR) à trois canaux | Trois canaux de couleur (16 bits : 16 bits : 16 bits) dans la « moitié » virgule flottante\* | BC6H               | Direct3D 11                     |
| Couleur à trois canaux, canal alpha facultatif  | Trois canaux de couleur (4 à 7 bits par canal) avec 0 à 8 bits d’alpha  | BC7                | Direct3D 11                     |

 

\*La « moitié » virgule flottante est une valeur de 16 bits qui se compose d’un bit de signe facultatif, d’un exposant biaisé à 5 bits et d’une mantisse 10 ou 11 bits.
## <a name="span-idbc1__bc2__and_b3_formatsspanspan-idbc1__bc2__and_b3_formatsspanspan-idbc1__bc2__and_b3_formatsspanbc1-bc2-and-b3-formats"></a><span id="BC1__BC2__and_B3_Formats"></span><span id="bc1__bc2__and_b3_formats"></span><span id="BC1__BC2__AND_B3_FORMATS"></span>Formats BC1, BC2 et B3


Les formats BC1, BC2 et BC3 sont équivalents aux formats de compression de texture de DXTn Direct3D 9 et sont identiques aux formats Direct3D 10 BC1, BC2 et BC3 correspondants. La prise en charge de ces trois formats est requise par tous les niveaux de fonctionnalité ( \_ niveau de fonctionnalité D3D \_ \_ 9 \_ 1, \_ niveau de fonctionnalité D3D \_ \_ 9 \_ 2, \_ niveau de fonctionnalité D3D \_ \_ 9 \_ 3, niveau de \_ fonctionnalité D3D \_ \_ 10 0, niveau de \_ \_ fonctionnalité D3D \_ \_ 10 \_ 1 et \_ niveau de fonctionnalité D3D \_ \_ 11 \_ 0).

| Format de compression de bloc | Format DXGI                                                                           | Format équivalent Direct3D 9                               | Octets par bloc de pixels 4x4 |
|--------------------------|---------------------------------------------------------------------------------------|------------------------------------------------------------|---------------------------|
| BC1                      | DXGI \_ format \_ BC1 \_ UNORM, DXGI \_ format \_ BC1 \_ UNORM \_ sRGB, DXGI \_ format \_ BC1 non \_ typé | D3DFMT \_ DXT1, FourCC = "DXT1"                                | 8                         |
| BC2                      | DXGI \_ format \_ BC2 \_ UNORM, DXGI \_ format \_ BC2 \_ UNORM \_ sRGB, DXGI \_ format \_ BC2 non \_ typé | D3DFMT \_ DXT2 \* , FourCC = "DXT2", D3DFMT \_ DXT3, FourCC = "DXT3" | 16                        |
| BC3                      | DXGI \_ format \_ BC3 \_ UNORM, DXGI \_ format \_ BC3 \_ UNORM \_ sRGB, DXGI \_ format \_ BC3 non \_ typé | D3DFMT \_ DXT4 \* , FourCC = "DXT4", D3DFMT \_ DXT5, FourCC = "DXT5" | 16                        |

 

\*Ces schémas de compression (DXT2 et DXT4) ne font aucune distinction entre les formats alpha prémultipliés Direct3D 9 et les formats alpha standard. Cette distinction doit être gérée par les nuanceurs programmables au moment du rendu.

## <a name="span-idbc4_and_bc5_formatsspanspan-idbc4_and_bc5_formatsspanspan-idbc4_and_bc5_formatsspanbc4-and-bc5-formats"></a><span id="BC4_and_BC5_Formats"></span><span id="bc4_and_bc5_formats"></span><span id="BC4_AND_BC5_FORMATS"></span>Formats textures BC4 et BC5


| Format de compression de bloc | Format DXGI                                                                     | Format équivalent Direct3D 9 | Octets par bloc de pixels 4x4 |
|--------------------------|---------------------------------------------------------------------------------|------------------------------|---------------------------|
| TEXTURES BC4                      | DXGI \_ format \_ textures BC4 \_ UNORM, DXGI \_ format \_ textures BC4 \_ ronfler, DXGI \_ format \_ textures BC4 non \_ typé | FourCC = "ATI1"                | 8                         |
| BC5                      | DXGI \_ format \_ BC5 \_ UNORM, DXGI \_ format \_ BC5 \_ ronfler, DXGI \_ format \_ BC5 non \_ typé | FourCC = "ATI2"                | 16                        |

 

## <a name="span-idbc6h_formatspanspan-idbc6h_formatspanspan-idbc6h_formatspanbc6h-format"></a><span id="BC6H_Format"></span><span id="bc6h_format"></span><span id="BC6H_FORMAT"></span>Format BC6H


Pour obtenir des informations plus détaillées sur ce format, consultez la documentation de [format BC6H](/windows/desktop/direct3d11/bc6h-format) .

| Format de compression de bloc | Format DXGI                                                                      | Format équivalent Direct3D 9 | Octets par bloc de pixels 4x4 |
|--------------------------|----------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC6H                     | DXGI \_ format \_ BC6H \_ UF16, DXGI \_ format \_ BC6H \_ SF16, DXGI \_ format \_ BC6H non \_ typé | N/A                          | 16                        |

 

Le format BC6H peut sélectionner différents modes d’encodage pour chaque bloc de pixels 4x4. Un total de 14 modes d’encodage différents sont disponibles, chacun avec des compromis légèrement différents dans la qualité visuelle résultante de la texture affichée. Le choix des modes permet de décoder rapidement le matériel avec le niveau de qualité sélectionné ou adapté en fonction du contenu source, mais il augmente également considérablement la complexité de l’espace de recherche.

## <a name="span-idbc7_formatspanspan-idbc7_formatspanspan-idbc7_formatspanbc7-format"></a><span id="BC7_Format"></span><span id="bc7_format"></span><span id="BC7_FORMAT"></span>Format BC7


Pour obtenir des informations plus détaillées sur ce format, consultez la documentation de [format Bc7](/windows/desktop/direct3d11/bc7-format) .

| Format de compression de bloc | Format DXGI                                                                           | Format équivalent Direct3D 9 | Octets par bloc de pixels 4x4 |
|--------------------------|---------------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC7                      | DXGI \_ format \_ Bc7 \_ UNORM, DXGI \_ format \_ Bc7 \_ UNORM \_ sRGB, DXGI \_ format \_ Bc7 non \_ typé | N/A                          | 16                        |

 

Le format BC7 peut sélectionner différents modes d’encodage pour chaque bloc de pixels 4x4. Un total de 8 modes d’encodage différents sont disponibles, chacun avec des compromis légèrement différents dans la qualité visuelle résultante de la texture affichée. Le choix des modes permet de décoder rapidement le matériel avec le niveau de qualité sélectionné ou adapté en fonction du contenu source, mais il augmente également considérablement la complexité de l’espace de recherche.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Annexes](appendix.md)

[Textures](/windows/desktop/direct3d11/overviews-direct3d-11-resources-textures)

 

 