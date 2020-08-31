---
title: Restitution de la sous-ressource Texture3D sous forme de mosaïque
description: Consultez un tableau qui montre comment les sous-ressources Texture3D sont présentées en mosaïque, en fonction des bits par pixel de la mosaïque Texture2D.
ms.assetid: 210D03E4-CF12-47E0-BA2F-C8D059B17D3E
keywords:
- Restitution de la sous-ressource Texture3D sous forme de mosaïque
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9ee4ff5c87022f9fd303b1331665a2551704cb93
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167943"
---
# <a name="texture3d-subresource-tiling"></a>Restitution de la sous-ressource Texture3D sous forme de mosaïque


Ce tableau montre comment les sous-ressources [**Texture3D**](/windows/desktop/direct3dhlsl/sm5-object-texture3d) sont présentées en mosaïque. Les valeurs de cette table ne comptent pas la fin de la compression MIP.

Cette table prend la mosaïque [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) et divise les dimensions x/y par 4 et ajoute 16 couches de profondeur. Toutes les vignettes du premier plan (plan 2D des vignettes définissant les 16 premières couches de profondeur) apparaissent avant les plans suivants.

**Remarque :** la prise en charge de  [**Texture3D**](/windows/desktop/direct3dhlsl/sm5-object-texture3d) dans les ressources de streaming n’est pas exposée dans l’implémentation initiale des ressources de streaming, mais les formes de vignette souhaitées sont répertoriées ici pour une prise en charge possible dans une version ultérieure.

 

| Bits/pixel (1 échantillon/pixel) | Dimensions de la mosaïque (pixels, WxHxD) |
|-----------------------------|---------------------------------|
| 8                           | 64x32x32                        |
| 16                          | 32x32x32                        |
| 32                          | 32x32x16                        |
| 64                          | 32x16x16                        |
| 128                         | 16x16x16                        |
| BC1, 4                       | 128x64x16                       |
| BC2, 3, 5, 6, 7                 | 64x64x16                        |

 

Les nombres de bits de format non pris en charge avec les ressources de streaming sont les formats 96 BPP, les formats vidéo, le \_ format dxgi \_ R1 \_ UNORM, le \_ format dxgi \_ R8G8 B8G8 UNORM et le \_ \_ format dxgi \_ \_ \_ \_ R8R8 G8B8 UNORM.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Restitution de la surface d’une ressource de diffusion en continu sous forme de mosaïque](how-a-streaming-resource-s-area-is-tiled.md)

 

 