---
title: Étape Pixel Shader (PS)
description: L’étape de nuanceur de pixels (PS) reçoit des données interpolées pour une primitive et génère des données par pixel telles que la couleur.
ms.assetid: 0AEBFDFB-0AD8-4633-AE4E-A44004B57745
keywords:
- Étape Pixel Shader (PS)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1d35fa06ac415b1d4197c1b1bcbf101f1d4cf9b7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156283"
---
# <a name="pixel-shader-ps-stage"></a>Étape Pixel Shader (PS)


L’étape de nuanceur de pixels (PS) reçoit des données interpolées pour une primitive et génère des données par pixel telles que la couleur.

Il s’agit d’une étape de nuanceur programmable ; Il est affiché sous la forme d’un bloc arrondi dans le diagramme de [pipeline graphique](graphics-pipeline.md) . Cette étape de nuanceur présente ses propres fonctionnalités uniques, basées sur le modèle de nuanceur 4,0 [Common-Shader Core](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-common-core).

L’étape de nuanceur de pixels (PS) permet d’effectuer des techniques d’ombrage riches, telles que l’éclairage par pixel et le traitement des messages. Un nuanceur de pixels est un programme qui combine des variables constantes, des données de texture, des valeurs interpolées par vertex et d’autres données pour produire des sorties par pixel. L' [étape de rastérisation (RS)](rasterizer-stage--rs-.md) appelle un nuanceur de pixels une fois pour chaque pixel couvert par une primitive. Toutefois, il est possible de spécifier un nuanceur **null** pour éviter d’exécuter un nuanceur.

Lors de l’échantillonnage multiple d’une texture, un nuanceur de pixels est appelé une fois par pixel couvert lorsqu’un test de profondeur/gabarit se produit pour chaque exemple multiple couvert. Les exemples qui réussissent le test de profondeur/stencil sont mis à jour avec la couleur de sortie du nuanceur de pixels.

Les fonctions intrinsèques de nuanceur de pixels produisent ou utilisent des dérivées de quantités par rapport à l’espace d’écran x et y. L’utilisation la plus courante pour les dérivés consiste à calculer les calculs de niveau de détail pour l’échantillonnage de texture et, dans le cas du filtrage anisotrope, à sélectionner des échantillons le long de l’axe de l’anisotropie. En règle générale, une implémentation matérielle exécute simultanément un nuanceur de pixels sur plusieurs pixels (par exemple une grille 2x2), de sorte que les dérivés des quantités calculées dans le nuanceur de pixels peuvent être raisonnablement approchées comme des deltas des valeurs au même point d’exécution en pixels adjacents.

## <a name="span-idinputsspanspan-idinputsspanspan-idinputsspaninputs"></a><span id="Inputs"></span><span id="inputs"></span><span id="INPUTS"></span>Port


Lorsque le pipeline est configuré sans nuanceur Geometry, un nuanceur de pixels est limité à 16, 32 bits, 4 composants. Dans le cas contraire, un nuanceur de pixels peut prendre jusqu’à 32, 32 bits, 4 composants.

Les données d’entrée de nuanceur de pixels incluent des attributs de vertex (qui peuvent être interpolés avec ou sans correction de perspective) ou peuvent être traités comme des constantes par Primitives. Les entrées de nuanceur de pixels sont interpolées à partir des attributs de vertex de la primitive en cours de pixellisation, en fonction du mode d’interpolation déclaré. Si une primitive est découpée avant pixellisation, le mode d’interpolation est également respecté au cours du processus de découpage.

Les attributs de vertex sont interpolés (ou évalués) aux emplacements du centre du nuanceur de pixels. Les modes d’interpolation d’attribut de nuanceur de pixels sont déclarés dans une déclaration de registre d’entrée, en fonction de chaque élément, dans un [argument](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-function-parameters) ou une [structure d’entrée](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-struct). Les attributs peuvent être interpolés de manière linéaire ou avec un échantillonnage de centre de gravité. Consultez la section « échantillonnage de centre de gravité des attributs lors de l’anticrénelage à l’échantillonnage multiple » dans les [règles de pixellisation](rasterization-rules.md). L’évaluation de l’un des centres de gravité s’applique uniquement au cours de l’échantillonnage multiple pour couvrir les cas où un pixel est couvert par une primitive mais un centre de pixels ne peut pas être. l’évaluation du centre de gravité se produit aussi près que possible du centre de pixels (non couvert).

Les entrées peuvent également être déclarées avec une [sémantique de valeur système](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics), qui marque un paramètre consommé par d’autres étapes de pipeline. Par exemple, une position de pixel doit être marquée avec la \_ sémantique de position SV. L' [étape de l’assembleur d’entrée](input-assembler-stage--ia-.md) peut produire une scalaire pour un nuanceur de pixels (à l’aide de SV \_ PrimitiveID); l' [étape de rastérisation](rasterizer-stage--rs-.md) peut également générer une scalaire pour un nuanceur de pixels (à l’aide de SV \_ IsFrontFace).

## <a name="span-idoutputsspanspan-idoutputsspanspan-idoutputsspanoutputs"></a><span id="Outputs"></span><span id="outputs"></span><span id="OUTPUTS"></span>Enverra


Un nuanceur de pixels peut sortir jusqu’à 8, 32 bits, 4 couleurs de composant ou aucune couleur si le pixel est ignoré. Les composants du registre de sortie du nuanceur de pixels doivent être déclarés avant de pouvoir être utilisés. un masque d’écriture de sortie distinct est autorisé pour chaque registre.

Utilisez l’État profondeur-écriture-activer (dans l' [étape de fusion de sortie (OM)](output-merger-stage--om-.md)) pour contrôler si les données de profondeur sont écrites dans une mémoire tampon de profondeur (ou utilisez l’instruction ignore pour ignorer les données pour ce pixel). Un nuanceur de pixels peut également générer une valeur facultative 32 bits, 1-composant, à virgule flottante, pour le test de profondeur (à l’aide de la \_ sémantique de profondeur SV). La valeur de profondeur est sortie dans le registre oDepth et remplace la valeur de profondeur interpolée pour le test de profondeur (en supposant que le test de profondeur est activé). Il n’existe aucun moyen de modifier dynamiquement l’utilisation de la profondeur de fonction fixe ou du nuanceur oDepth.

Un nuanceur de pixels ne peut pas générer une valeur de stencil.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Pipeline graphique](graphics-pipeline.md)

 

 