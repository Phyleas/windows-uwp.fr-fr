---
title: Rectangles
description: Tout au long de la programmation Direct3D et Windows, les objets à l’écran sont référencés en termes de rectangles englobants.
ms.assetid: 3B78AE66-2C1A-4191-BDCA-D737E33460BA
keywords:
- Rectangles
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a30aa1a2901f109a4f13316024785981023975b8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156253"
---
# <a name="rectangles"></a>Rectangles

Tout au long de la programmation Direct3D et Windows, les objets à l’écran sont référencés en termes de rectangles englobants. Les côtés d’un rectangle englobant sont toujours parallèles aux côtés de l’écran. le rectangle peut donc être décrit par deux points, l’angle supérieur gauche et le coin inférieur droit.

## <a name="span-idbounding_rectanglesspanspan-idbounding_rectanglesspanspan-idbounding_rectanglesspanbounding-rectangles"></a><span id="Bounding_rectangles"></span><span id="bounding_rectangles"></span><span id="BOUNDING_RECTANGLES"></span>Rectangles englobants


La plupart des applications utilisent la structure [**Rect**](/previous-versions/dd162897(v=vs.85)) (ou un alias typedef) pour transporter des informations sur un rectangle englobant à utiliser lorsque blitting à l’écran ou lors de la détection des accès. En C++, la structure **Rect** a la définition suivante.

```cpp
typedef struct tagRECT { 
    LONG    left;    // This is the upper-left corner x-coordinate.
    LONG    top;     // The upper-left corner y-coordinate.
    LONG    right;   // The lower-right corner x-coordinate.
    LONG    bottom;  // The lower-right corner y-coordinate.
} RECT, *PRECT, NEAR *NPRECT, FAR *LPRECT; 
```

Dans l’exemple précédent, les membres gauche et supérieur sont les coordonnées x et y du coin supérieur gauche d’un rectangle englobant. De même, les membres droits et inférieurs composent les coordonnées du coin inférieur droit. L’illustration suivante montre comment vous pouvez visualiser ces valeurs.

![illustration du rectangle délimité par les valeurs gauche, haut, droite et inférieure](images/rect.png)

La colonne de pixels sur le bord droit et la ligne de pixels du bord inférieur ne sont pas incluses dans le RECT. Par exemple, le verrouillage d’un RECT avec les membres {10, 10, 138, 138} produit un objet 128 pixels en largeur et en hauteur.

Pour améliorer l’efficacité, la cohérence et la simplicité d’utilisation, toutes les fonctions de présentation Direct3D fonctionnent avec des rectangles.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Systèmes de coordonnées et géométrie](coordinate-systems-and-geometry.md)

 

 