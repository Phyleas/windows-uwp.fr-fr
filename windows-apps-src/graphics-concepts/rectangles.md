---
title: Rectangles
description: "Dans la programmationDirect3D et Windows, les objets à l’écran sont appelés «rectangles englobants»."
ms.assetid: 3B78AE66-2C1A-4191-BDCA-D737E33460BA
keywords: Rectangles
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 2128dd8fa6ff22e20cd8a25dea0fd44431c1fae2
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="rectangles"></a>Rectangles


Dans la programmationDirect3D et Windows, les objets à l’écran sont appelés «rectangles englobants». Les côtés d’un rectangle englobant sont toujours parallèles aux côtés de l’écran, ce qui signifie que le rectangle peut être décrit par deux points: l’angle supérieur gauche et l’angle inférieur droit.

## <a name="span-idboundingrectanglesspanspan-idboundingrectanglesspanspan-idboundingrectanglesspanbounding-rectangles"></a><span id="Bounding_rectangles"></span><span id="bounding_rectangles"></span><span id="BOUNDING_RECTANGLES"></span>Rectangles englobants


La plupart des applications utilisent la structure [**RECT**](https://msdn.microsoft.com/library/windows/desktop/dd162897) (ou un alias typedef’d) pour acheminer les informations relatives à un rectangle englobant à utiliser lors du rendu à l’écran par blitting ou lors de la détection des correspondances. En langage C++, la structure **RECT** a la définition suivante.

```
typedef struct tagRECT { 
    LONG    left;    // This is the upper-left corner x-coordinate.
    LONG    top;     // The upper-left corner y-coordinate.
    LONG    right;   // The lower-right corner x-coordinate.
    LONG    bottom;  // The lower-right corner y-coordinate.
} RECT, *PRECT, NEAR *NPRECT, FAR *LPRECT; 
```

Dans l’exemple précédent, les membres left et top sont les coordonnées x et y de l’angle supérieur gauche d’un rectangle englobant. De la même façon, les membres right et bottom forment les coordonnées de l’angle inférieur droit. L’illustration suivante montre comment vous pouvez visualiser ces valeurs.

![illustration du rectangle délimité par les valeurs left, top, right et bottom](images/rect.png)

La colonne de pixels du bord droit et la ligne de pixels du bord inférieur ne sont pas incluses dans la structure RECT. Par exemple, le résultat du verrouillage d’une structure RECT avec des membres {10, 10, 138, 138} est un objet de 128pixels de largeur et hauteur.

Dans un souci d’efficacité, de cohérence et de simplicité d’utilisation, toutes les fonctions de présentation Direct3D fonctionnent avec les rectangles.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Systèmes de coordonnées et géométrie](coordinate-systems-and-geometry.md)

 

 



