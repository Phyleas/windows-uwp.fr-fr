---
title: Restitution de la sous-ressource Texture3D sous forme de tuiles
description: Ce tableau montre comment des sous-ressources Texture3D sont restituées sous forme de tuiles.
ms.assetid: 210D03E4-CF12-47E0-BA2F-C8D059B17D3E
keywords:
- Restitution de la sous-ressource Texture3D sous forme de tuiles
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: d51d20ddaeca5aa0689104b3dd71e36b1a5d4132
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2018
ms.locfileid: "1043988"
---
# <a name="texture3d-subresource-tiling"></a>Restitution de la sous-ressource Texture3D sous forme de tuiles


Ce tableau montre comment des sous-ressources [**Texture3D**](https://msdn.microsoft.com/library/windows/desktop/ff471562) sont restituées sous forme de tuiles. Les valeurs figurant dans ce tableau ne prennent pas en compte le package de fin de mip.

Ce tableau prend les tuiles [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) et divise les dimensions x/y par 4 et ajoute 16couches de profondeur. Toutes les tuiles du premier plan (plan 2D de tuiles définissant les 16premières couches de profondeur) apparaissent avant les plans ultérieurs.

**Remarque**  La prise en charge de [**Texture3D**](https://msdn.microsoft.com/library/windows/desktop/ff471562) dans les ressources de diffusion en continu n’est pas disponible dans l’implémentation initiale des ressources de diffusion en continu, mais les formes de tuiles souhaitées sont répertoriées ici pour la prise en charge possible avec une version ultérieure.

 

| Bits/pixel (1exemple/pixel) | Dimensions de la tuile (Pixels, lxhxp) |
|-----------------------------|---------------------------------|
| 8                           | 64x32x32                        |
| 16                          | 32x32x32                        |
| 32                          | 32x32x16                        |
| 64                          | 32x16x16                        |
| 128                         | 16x16x16                        |
| BC1,4                       | 128x64x16                       |
| BC2,3,5,6,7                 | 64x64x16                        |

 

Les bits de format non pris en charge avec les ressources de diffusion en continu sont les formats de 96bpp, les formats vidéo, DXGI\_FORMAT\_R1\_UNORM, DXGI\_FORMAT\_R8G8\_B8G8\_UNORM et DXGI\_FORMAT\_R8R8\_G8B8\_UNORM.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques associées


[Restitution de la surface d’une ressource de diffusion en continu sous forme de vignettes](how-a-streaming-resource-s-area-is-tiled.md)

 

 



