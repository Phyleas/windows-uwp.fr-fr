---
title: Affichage des mémoires tampons de sommets (VBV) et d’index (IBV)
description: En savoir plus sur la vue de mémoire tampon de vertex (VBV) et la vue de mémoire tampon d’index (IBV), qui contiennent des index de données et d’entiers pour les vertex dans le rendu Direct3D.
ms.assetid: 695115D2-9DA0-41F2-9416-33BFAB698129
keywords:
- Vue de la mémoire tampon de vertex (VBV)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a616f2bad8f478b2d20e96b183ba944950fef8a8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156093"
---
# <a name="vertex-buffer-view-vbv-and-index-buffer-view-ibv"></a>Affichage des mémoires tampons de sommets (VBV) et d’index (IBV)


Une mémoire tampon de vertex contient des données pour une liste de vertex. Les données de chaque vertex peuvent inclure la position, la couleur, le vecteur normal, les coordonnées des textures, etc. Une mémoire tampon d’index contient des index d’entiers (offsets) dans une mémoire tampon de vertex et est utilisée pour définir et restituer un objet constitué d’un sous-ensemble de la liste complète des vertex.

La définition d’un seul vertex correspond souvent à l’application à définir, par exemple :

``` syntax
struct CUSTOMVERTEX { 
        FLOAT x, y, z;      // The position
        FLOAT nx, ny, nz;   // The normal
        DWORD color;        // RGBA color
        FLOAT tu, tv;       // The texture coordinates. 
}; 
```

La définition de CUSTOMVERTEX serait ensuite transmise au pilote Graphics lors de la création de mémoires tampons de vertex.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Views](views.md)

 

 




