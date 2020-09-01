---
title: Niveau 1
description: Cette section décrit la prise en charge de niveau 1.
ms.assetid: 153DA429-0C99-4691-AEB4-124FD9B17BE2
keywords:
- Niveau 1
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7fcd5debb367b73ad23492cf63d3acdd1797c23a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162703"
---
# <a name="tier-1"></a>Niveau 1


Cette section décrit la prise en charge de niveau 1.

## <a name="span-idtier_1_general_limitationsspanspan-idtier_1_general_limitationsspanspan-idtier_1_general_limitationsspantier-1-general-limitations"></a><span id="Tier_1_general_limitations"></span><span id="tier_1_general_limitations"></span><span id="TIER_1_GENERAL_LIMITATIONS"></span>Limites générales du niveau 1


-   Matériel au niveau de la fonctionnalité 11,0 minimum.
-   Aucune prise en charge de la surface.
-   Aucune prise en charge de Texture1D ou Texture3D.
-   Aucun exemple de prise en charge de l’anticrénelage (MSAA) à 2, 8 ou 16. Seul 4x est requis, à l’exception des formats 128 BPP.
-   Aucun modèle de Swizzle standard (la disposition dans les mosaïques de 64 Ko et la compression MIP tail est à la disposition du fabricant du matériel).
-   Limitations sur la façon dont les vignettes sont accessibles lorsqu’il existe des mappages en double. Consultez [limitations d’accès aux vignettes avec des mappages dupliqués](tile-access-limitations-with-duplicate-mappings.md).

## <a name="span-idspecific_limitations_affecting_tier_1_onlyspanspan-idspecific_limitations_affecting_tier_1_onlyspanspan-idspecific_limitations_affecting_tier_1_onlyspanspecific-limitations-affecting-tier-1-only"></a><span id="Specific_limitations_affecting_tier_1_only"></span><span id="specific_limitations_affecting_tier_1_only"></span><span id="SPECIFIC_LIMITATIONS_AFFECTING_TIER_1_ONLY"></span>Limitations spécifiques affectant le niveau 1 uniquement


### <a name="span-idreading_writing_to_streaming_resources_that_have_null_mappingsspanspan-idreading_writing_to_streaming_resources_that_have_null_mappingsspanspan-idreading_writing_to_streaming_resources_that_have_null_mappingsspanreadingwriting-to-streaming-resources-that-have-null-mappings"></a><span id="Reading_writing_to_streaming_resources_that_have_NULL_mappings"></span><span id="reading_writing_to_streaming_resources_that_have_null_mappings"></span><span id="READING_WRITING_TO_STREAMING_RESOURCES_THAT_HAVE_NULL_MAPPINGS"></span>Lecture/écriture de ressources de streaming avec des mappages NULL

Les ressources de streaming peuvent avoir des mappages **null** , mais leur lecture ou leur écriture génère des résultats indéfinis, y compris les appareils supprimés. Les applications peuvent contourner ce risque en mappant une page factice unique à toutes les zones vides. Soyez prudent si vous écrivez et rendez sur une page mappée à plusieurs emplacements de cibles de rendu, car l’ordre des écritures n’est pas défini.

### <a name="span-idno_shader_instructions_for_clamping_lod_and_mapped_status_feedbackspanspan-idno_shader_instructions_for_clamping_lod_and_mapped_status_feedbackspanspan-idno_shader_instructions_for_clamping_lod_and_mapped_status_feedbackspanno-shader-instructions-for-clamping-lod-and-mapped-status-feedback"></a><span id="No_shader_instructions_for_clamping_LOD_and_mapped_status_feedback"></span><span id="no_shader_instructions_for_clamping_lod_and_mapped_status_feedback"></span><span id="NO_SHADER_INSTRUCTIONS_FOR_CLAMPING_LOD_AND_MAPPED_STATUS_FEEDBACK"></span>Aucune instruction de nuanceur pour le blocage des commentaires sur les États et les commentaires

Les instructions du nuanceur pour le déverrouillage du LOD et les commentaires d’État mappés ne sont pas disponibles. Consultez [exposition des ressources de streaming HLSL](hlsl-streaming-resources-exposure.md).

### <a name="span-idalignment_constraints_for_standard_tile_shapesspanspan-idalignment_constraints_for_standard_tile_shapesspanspan-idalignment_constraints_for_standard_tile_shapesspanalignment-constraints-for-standard-tile-shapes"></a><span id="Alignment_constraints_for_standard_tile_shapes"></span><span id="alignment_constraints_for_standard_tile_shapes"></span><span id="ALIGNMENT_CONSTRAINTS_FOR_STANDARD_TILE_SHAPES"></span>Contraintes d’alignement pour les formes de mosaïque standard

Il est garanti uniquement que les MIPS (à partir du plus) dont les dimensions sont tous des multiples de la taille de mosaïque standard prennent en charge les formes de mosaïque standard et peuvent avoir des vignettes individuelles mappées/non mappées. Le premier mipmap dans une ressource de streaming qui a une dimension qui n’est pas un multiple de la taille de mosaïque standard, ainsi que toutes les des mipmaps plus grossières, peut avoir une forme de mosaïque non standard, qui se raccorde en N 64 à 64 mosaïques pour cet ensemble de mips à la fois (N signalé à l’application). Ces vignettes N sont considérées comme une seule unité, qui doit être entièrement mappée ou entièrement démappée par l’application à un moment donné, bien que les mappages de chacune des vignettes puissent se trouver dans des emplacements de jointure arbitraire dans un pool de mosaïques.

### <a name="span-idarray_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_sizespanspan-idarray_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_sizespanspan-idarray_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_sizespanarray-of-mipmaps-that-arent-a-multiple-of-standard-tile-size"></a><span id="Array_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_size"></span><span id="array_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_size"></span><span id="ARRAY_OF_MIPMAPS_THAT_AREN_T_A_MULTIPLE_OF_STANDARD_TILE_SIZE"></span>Tableau de des mipmaps qui ne sont pas un multiple de taille de mosaïque standard

Les ressources de streaming avec n’importe quel des mipmaps n’est pas autorisée pour une taille de tableau supérieure à 1.

### <a name="span-idswitching_between_referencing_tiles_in_a_tile_pool_via_a_buffer_and_texture_resourcespanspan-idswitching_between_referencing_tiles_in_a_tile_pool_via_a_buffer_and_texture_resourcespanspan-idswitching_between_referencing_tiles_in_a_tile_pool_via_a_buffer_and_texture_resourcespanswitching-between-referencing-tiles-in-a-tile-pool-via-a-buffer-and-texture-resource"></a><span id="Switching_between_referencing_tiles_in_a_tile_pool_via_a_Buffer_and_Texture_resource"></span><span id="switching_between_referencing_tiles_in_a_tile_pool_via_a_buffer_and_texture_resource"></span><span id="SWITCHING_BETWEEN_REFERENCING_TILES_IN_A_TILE_POOL_VIA_A_BUFFER_AND_TEXTURE_RESOURCE"></span>Basculement entre les vignettes de référencement dans un pool de mosaïques via une ressource de mémoire tampon et de texture

Pour basculer entre les vignettes de référencement dans un pool de mosaïques via une ressource de [mémoire tampon](introduction-to-buffers.md) pour référencer les mêmes vignettes via une ressource de [texture](introduction-to-textures.md) , ou vice versa, la mise à jour la plus récente des mappages de vignettes ou la copie de mappages de vignette qui définit des mappages à ces vignettes de pool de mosaïques doit être pour la même dimension de ressource (tampon et texture \* ) que la dimension de ressource qui sera utilisée pour accéder aux vignettes. Dans le cas contraire, le comportement n’est pas défini, y compris le risque de réinitialisation de l’appareil.

Ainsi, par exemple, il n’est pas valide de mettre à jour les mappages de vignette pour définir des mappages de vignettes pour une mémoire tampon, puis de mettre à jour les mappages de vignette sur les mêmes vignettes dans le pool de mosaïques via une ressource [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) , puis d’accéder aux vignettes via la mémoire tampon. Les opérations de contournement sont la redéfinition des mappages de vignettes pour une ressource lors du basculement entre la mémoire tampon et la texture (ou vice versa) partageant des vignettes ou simplement jamais de partager des vignettes dans un pool de mosaïques entre les ressources de mémoire tampon et les ressources de texture.

### <a name="span-idmin_max_reduction_filteringspanspan-idmin_max_reduction_filteringspanspan-idmin_max_reduction_filteringspanminmax-reduction-filtering"></a><span id="Min_Max_reduction_filtering"></span><span id="min_max_reduction_filtering"></span><span id="MIN_MAX_REDUCTION_FILTERING"></span>Filtrage de réduction min/max

Le filtrage de réduction min/max n’est pas pris en charge. Consultez [ressources de streaming fonctionnalités d’échantillonnage de texture](streaming-resources-texture-sampling-features.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Niveaux de fonctionnalité des ressources de diffusion en continu](streaming-resources-features-tiers.md)

 

 