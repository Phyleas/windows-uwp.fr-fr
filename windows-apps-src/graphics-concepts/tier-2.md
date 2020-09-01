---
title: Niveau 2
description: La prise en charge de niveau 2 pour les ressources de streaming ajoute des fonctionnalités au-delà du niveau 1, telles que la garantie d’un mipmap de texture non compactée lorsque la taille est d’au moins une forme de mosaïque standard. instructions du nuanceur pour la fixation du niveau de détail (LOD) et pour obtenir l’état de l’opération de nuanceur ; en outre, la lecture des vignettes mappées sur NULL traite cette valeur échantillonnée comme étant égale à zéro.
ms.assetid: 111A28EA-661A-4D29-921A-F2E376A46DC5
keywords:
- Niveau 2
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 62fbbf3f21d614739918478b58394e51dbe577f9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175163"
---
# <a name="tier-2"></a>Niveau 2


La prise en charge de niveau 2 pour les ressources de streaming ajoute des fonctionnalités au-delà du niveau 1, telles que la garantie d’un mipmap de texture non compactée lorsque la taille est d’au moins une forme de mosaïque standard. instructions du nuanceur pour la fixation du niveau de détail (LOD) et pour obtenir l’état de l’opération de nuanceur ; en outre, la lecture des vignettes mappées sur NULL traite cette valeur échantillonnée comme étant égale à zéro.

## <a name="span-idtier_2_general_supportspanspan-idtier_2_general_supportspanspan-idtier_2_general_supportspantier-2-general-support"></a><span id="Tier_2_general_support"></span><span id="tier_2_general_support"></span><span id="TIER_2_GENERAL_SUPPORT"></span>Support général de niveau 2


La prise en charge de niveau 2 comprend les éléments suivants.

-   Matériel au niveau de la fonctionnalité 11,1 minimum.
-   Toutes les fonctionnalités du niveau précédent (sans limitations spécifiques au [niveau 1](tier-1.md) ), ainsi que les ajouts apportés aux éléments suivants :
-   Les instructions du nuanceur pour le déverrouillage du LOD et les commentaires d’État mappés sont disponibles. Consultez [exposition des ressources de streaming HLSL](hlsl-streaming-resources-exposure.md).

En plus de celles-ci, il existe des problèmes de prise en charge spécifiques qui suivent.

## <a name="span-idnon-mapped_tilesspanspan-idnon-mapped_tilesspanspan-idnon-mapped_tilesspannon-mapped-tiles"></a><span id="Non-mapped_tiles"></span><span id="non-mapped_tiles"></span><span id="NON-MAPPED_TILES"></span>Vignettes non mappées


Les lectures à partir de vignettes non mappées retournent 0 dans tous les composants non manquants du format, et la valeur par défaut pour les composants manquants.

Les écritures dans les vignettes non mappées sont arrêtées du passage à la mémoire, mais elles peuvent se trouver dans les caches que les lectures ultérieures vers la même adresse peuvent ou non être levées.

## <a name="span-idtexture_filteringspanspan-idtexture_filteringspanspan-idtexture_filteringspantexture-filtering"></a><span id="Texture_filtering"></span><span id="texture_filtering"></span><span id="TEXTURE_FILTERING"></span>Filtrage de texture


Le filtrage de texture avec un encombrement qui chevauche les vignettes **null** et non**null** contribue à 0 (avec des valeurs par défaut pour les composants de format manquants) pour les texels sur les vignettes **null** dans l’opération de filtre globale. Un matériel précoce ne répond pas à cette exigence et retourne 0 (avec des valeurs par défaut pour les composants de format manquants) pour le résultat de filtre complet si des texels (avec une pondération différente de zéro) tombent sur une vignette **null** . Aucun autre matériel ne sera autorisé à manquer l’obligation d’inclure tous les texels (avec une pondération différente de zéro) dans l’opération de filtre.

Les accès texels **null** entraînent l’opération [**CheckAccessFullyMapped**](/windows/desktop/direct3dhlsl/checkaccessfullymapped) sur les commentaires d’État pour qu’une texture lue retourne la valeur false. C’est quelle que soit la façon dont le résultat de l’accès à la texture peut obtenir un masque d’écriture dans le nuanceur et le nombre de composants dans le format de texture (la combinaison peut faire apparaître que la texture n’a pas besoin d’être accessible).

## <a name="span-idalignment_constraintsspanspan-idalignment_constraintsspanspan-idalignment_constraintsspanalignment-constraints"></a><span id="Alignment_constraints"></span><span id="alignment_constraints"></span><span id="ALIGNMENT_CONSTRAINTS"></span>Contraintes d’alignement


Contraintes d’alignement pour les formes de mosaïques standard : les des mipmaps qui remplissent au moins une vignette standard dans toutes les dimensions sont assurés d’utiliser la mosaïque standard, le reste étant considéré comme une **unité** dans n vignettes (n signalées à l’application). L’application peut mapper les N vignettes dans des emplacements arbitrairement disjoints dans un pool de mosaïques, mais doit mapper tout ou aucune des vignettes compressées. La compression MIP est un ensemble unique de vignettes compressées par tranche de tableau.

## <a name="span-idmin_max_reduction_filteringspanspan-idmin_max_reduction_filteringspanspan-idmin_max_reduction_filteringspanminmax-reduction-filtering"></a><span id="Min_Max_reduction_filtering"></span><span id="min_max_reduction_filtering"></span><span id="MIN_MAX_REDUCTION_FILTERING"></span>Filtrage de réduction min/max


Le filtrage de réduction min/max est pris en charge. Consultez [ressources de streaming fonctionnalités d’échantillonnage de texture](streaming-resources-texture-sampling-features.md).

## <a name="span-idlimitationsspanspan-idlimitationsspanspan-idlimitationsspanlimitations"></a><span id="Limitations"></span><span id="limitations"></span><span id="LIMITATIONS"></span>Limitations


Les ressources de streaming avec un des mipmaps inférieur à la taille de mosaïque standard dans une dimension ne sont pas autorisées à avoir une taille de tableau supérieure à 1.

Limitations sur la façon dont les vignettes sont accessibles lorsqu’il existe des mappages en double continuent à s’appliquer. Consultez [limitations d’accès aux vignettes avec des mappages dupliqués](tile-access-limitations-with-duplicate-mappings.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Niveaux de fonctionnalité des ressources de diffusion en continu](streaming-resources-features-tiers.md)

 

 