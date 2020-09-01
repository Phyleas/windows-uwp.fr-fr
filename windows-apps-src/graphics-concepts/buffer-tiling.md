---
title: Mosaïque de mémoires tampons
description: Une ressource de mémoire tampon est divisée en mosaïques de 64 Ko, avec un espace vide dans la dernière vignette si la taille n’est pas un multiple de 64 Ko.
ms.assetid: 577DC6B0-F373-4748-AD80-2784C597C366
keywords:
- Mosaïque de mémoires tampons
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fdb78dc2cff6ccedec58acbc946068e14511fdc2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156353"
---
# <a name="buffer-tiling"></a>Mosaïque de mémoires tampons


Une ressource de [mémoire tampon](introduction-to-buffers.md) est divisée en mosaïques de 64 Ko, avec un espace vide dans la dernière vignette si la taille n’est pas un multiple de 64 Ko.

Les mémoires tampons structurées ne doivent pas avoir de contrainte sur le STRIDE à présenter en mosaïque. Toutefois, les optimisations de performances possibles dans le matériel pour l’utilisation de [**StructuredBuffers**](/windows/desktop/direct3dhlsl/sm5-object-structuredbuffer) peuvent être sacrifiées en les mettant en mosaïque en premier lieu.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Restitution de la surface d’une ressource de diffusion en continu sous forme de mosaïque](how-a-streaming-resource-s-area-is-tiled.md)

 

 