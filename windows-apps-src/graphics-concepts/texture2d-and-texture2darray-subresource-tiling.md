---
title: Restitution des sous-ressources Texture2D et Texture2DArray sous forme de mosaïque
description: Ces tableaux montrent comment les sous-ressources Texture2D et Texture2DArray sont présentées en mosaïque.
ms.assetid: 2DC14DFC-5299-44D9-895F-5A223D3FD530
keywords:
- Restitution des sous-ressources Texture2D et Texture2DArray sous forme de mosaïque
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 28152f39983f4831a9efa981efcb85fb65fa0204
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156173"
---
# <a name="texture2d-and-texture2darray-subresource-tiling"></a>Restitution des sous-ressources Texture2D et Texture2DArray sous forme de mosaïque


Ces tableaux montrent comment les sous-ressources [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) et [**Texture2DArray**](/windows/desktop/direct3dhlsl/sm5-object-texture2darray) sont présentées en mosaïque. Les valeurs de ces tables ne comptent pas la fin de la compression MIP.

## <a name="span-idsubresources-with-multisample-counts-of-1spanspan-idsubresources-with-multisample-counts-of-1spanspan-idsubresources-with-multisample-counts-of-1spansubresources-with-multisample-counts-of-1"></a><span id="Subresources-with-multisample-counts-of-1"></span><span id="subresources-with-multisample-counts-of-1"></span><span id="SUBRESOURCES-WITH-MULTISAMPLE-COUNTS-OF-1"></span>Sous-ressources avec un nombre d’échantillonnages de 1


Ce tableau montre comment les sous-ressources [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) et [**Texture2DArray**](/windows/desktop/direct3dhlsl/sm5-object-texture2darray) avec un nombre d’échantillons de 1 sont en mosaïque.

| Bits/pixel (1 échantillon/pixel) | Dimensions de la mosaïque (pixels, WxH) |
|-----------------------------|-------------------------------|
| 8                           | 256 x 256                       |
| 16                          | 256 x 128                       |
| 32                          | 128 x 128                       |
| 64                          | 128x64                        |
| 128                         | 64 x 64                         |
| BC1, 4                       | 512x256                       |
| BC2, 3, 5, 6, 7                 | 256 x 256                       |

 

Les nombres de bits de format non pris en charge avec les ressources de streaming sont les formats 96 BPP, les formats vidéo, le \_ format dxgi \_ R1 \_ UNORM, le \_ format dxgi \_ R8G8 B8G8 UNORM et le \_ \_ format dxgi \_ \_ \_ \_ R8R8 G8B8 UNORM.

## <a name="span-idsubresources-with-various-multisample-countsspanspan-idsubresources-with-various-multisample-countsspanspan-idsubresources-with-various-multisample-countsspansubresources-with-various-multisample-counts"></a><span id="Subresources-with-various-multisample-counts"></span><span id="subresources-with-various-multisample-counts"></span><span id="SUBRESOURCES-WITH-VARIOUS-MULTISAMPLE-COUNTS"></span>Sous-ressources avec différents nombres d’échantillonnages


Ce tableau montre comment les sous-ressources [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) et [**Texture2DArray**](/windows/desktop/direct3dhlsl/sm5-object-texture2darray) avec différents nombres d’échantillons sont en mosaïque.

| Bits/pixel (1 échantillon/pixel) | Dimensions de la mosaïque (pixels, WxH) |
|-----------------------------|-------------------------------|
| 1                           | individuelles                           |
| 2                           | 2 x 1                           |
| 4                           | 2x2                           |
| 8                           | 4x2                           |
| 16                          | 4x4                           |

 

Seuls les nombres d’échantillons 1 et 4 sont requis (et autorisés) pour être pris en charge avec les ressources de streaming. Les ressources de streaming ne prennent pas en charge 2, 8 et 16, même si elles sont affichées.

Les implémentations peuvent choisir de prendre en charge les modes d’échantillonnage 2, 8 et/ou 16 pour les ressources sans diffusion en continu, même si la diffusion en continu ne les prend pas en charge.

Les ressources de streaming avec des exemples de nombres supérieurs à 1 ne peuvent pas utiliser les formats 128 BPP.

Les contraintes sur les nombres d’échantillons et les formats pris en charge sont dues à des incohérences matérielles de la spécification.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Restitution de la surface d’une ressource de diffusion en continu sous forme de mosaïque](how-a-streaming-resource-s-area-is-tiled.md)

 

 