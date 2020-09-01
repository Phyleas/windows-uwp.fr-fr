---
title: Ressources de texture
description: En savoir plus sur le rendu avec les ressources de texture Direct3D et sur la prise en charge de la fusion de plusieurs textures à l’aide de étapes de texture.
ms.assetid: 016F6CDA-D361-4E6B-BA99-49E915A19364
keywords:
- Ressources de texture
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d82a5525601c98812d6aab97f5f5d4399ceddc91
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156183"
---
# <a name="texture-resources"></a>Ressources de texture


Les textures sont un type de ressource utilisé pour le rendu.

## <a name="span-idrendering_with_texture_resourcesspanspan-idrendering_with_texture_resourcesspanspan-idrendering_with_texture_resourcesspanrendering-with-texture-resources"></a><span id="Rendering_with_Texture_Resources"></span><span id="rendering_with_texture_resources"></span><span id="RENDERING_WITH_TEXTURE_RESOURCES"></span>Rendu avec des ressources de texture


Direct3D prend en charge la fusion de plusieurs textures par le biais du concept de étapes de texture. Chaque étape de texture contient une texture et des opérations qui peuvent être effectuées sur la texture. Les textures dans les étapes de texture forment le jeu de textures actuelles. Consultez [fusion de texture](texture-blending.md). L’état de chaque texture est encapsulé dans son étape de texture.

Votre application peut également définir la perspective de texture et les États de filtrage de texture. Consultez [filtrage de texture](texture-filtering.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Textures](textures.md)

 

 




