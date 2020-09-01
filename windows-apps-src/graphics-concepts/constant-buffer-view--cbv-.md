---
title: Affichage des mémoires tampons de constantes (CBV)
description: Les mémoires tampons constantes contiennent des données de constante de nuanceur. La valeur de ces données est que les données sont conservées et accessibles par n’importe quel nuanceur GPU, jusqu’à ce qu’il soit nécessaire de modifier les données.
ms.assetid: 99AEC6B0-A43B-4B61-8C3A-ECC8DE1B69A7
keywords:
- Affichage des mémoires tampons de constantes (CBV)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 50b590ab931f60b67ecd527629b681c9ffe63f71
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162743"
---
# <a name="constant-buffer-view-cbv"></a>Affichage des mémoires tampons de constantes (CBV)


Les mémoires tampons constantes contiennent des données de constante de nuanceur. La valeur de ces données est que les données sont conservées et accessibles par n’importe quel nuanceur GPU, jusqu’à ce qu’il soit nécessaire de modifier les données.

Les données typiques d’une mémoire tampon constante sont des matrices de monde, de projection et de vue, qui restent constantes tout au long du dessin d’une image.

La disposition de la mémoire tampon constante doit correspondre à la disposition HLSL (reportez-vous aux [règles de compression pour les variables constantes](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-packing-rules)).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Views](views.md)

 

 