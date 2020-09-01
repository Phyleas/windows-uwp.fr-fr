---
title: Besoin en ressources de diffusion en continu
description: Les ressources de diffusion en continu sont nécessaires pour que la mémoire GPU ne stocke pas les régions des surfaces auxquelles il n’est pas possible d’accéder, et pour indiquer au matériel comment filtrer les vignettes adjacentes.
ms.assetid: A88BE65B-104F-4176-9809-C12580A3684C
keywords:
- Besoin en ressources de diffusion en continu
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b2e95427be7cdae450e60317d13765bb9372e84b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173023"
---
# <a name="the-need-for-streaming-resources"></a>Besoin en ressources de diffusion en continu


Les ressources de diffusion en continu sont nécessaires pour que la mémoire GPU ne stocke pas les régions des surfaces auxquelles il n’est pas possible d’accéder, et pour indiquer au matériel comment filtrer les vignettes adjacentes.

## <a name="span-idstreaming_resources_or_sparse_texturesspanspan-idstreaming_resources_or_sparse_texturesspanspan-idstreaming_resources_or_sparse_texturesspanstreaming-resources-or-sparse-textures"></a><span id="Streaming_resources_or_sparse_textures"></span><span id="streaming_resources_or_sparse_textures"></span><span id="STREAMING_RESOURCES_OR_SPARSE_TEXTURES"></span>Ressources de streaming ou textures éparses


Les ressources de *streaming* (appelées *ressources en mosaïque* dans Direct3D 11) sont des ressources logiques volumineuses qui utilisent de petites quantités de mémoire physique.

Les ressources de *streaming ont un*autre nom. « Épars » véhicule à la fois la nature en mosaïque des ressources, et peut-être la raison principale de les faire apparaître en mosaïque, ce qui n’est pas censé tous être mappés à la fois. En fait, une application peut créer de manière concevable une ressource de streaming dans laquelle aucune donnée n’est créée pour toutes les régions et les mips de la ressource, intentionnellement. Ainsi, le contenu proprement dit peut être fragmenté, et le mappage du contenu dans la mémoire de l’unité de traitement graphique (GPU) à un moment donné est un sous-ensemble de celui-ci (encore plus fragmenté).

## <a name="span-idwithout_tiling__memory_allocations_are_managed_at_subresource_granularityspanspan-idwithout_tiling__memory_allocations_are_managed_at_subresource_granularityspanspan-idwithout_tiling__memory_allocations_are_managed_at_subresource_granularityspanwithout-tiling-memory-allocations-are-managed-at-subresource-granularity"></a><span id="Without_tiling__memory_allocations_are_managed_at_subresource_granularity"></span><span id="without_tiling__memory_allocations_are_managed_at_subresource_granularity"></span><span id="WITHOUT_TILING__MEMORY_ALLOCATIONS_ARE_MANAGED_AT_SUBRESOURCE_GRANULARITY"></span>Sans mosaïque, les allocations de mémoire sont gérées au niveau de granularité des sous-ressources


Dans un système graphique (autrement dit, le système d’exploitation, le pilote d’affichage et le matériel graphique) sans prise en charge des ressources de streaming, le système graphique gère toutes les allocations de mémoire Direct3D au niveau de granularité des sous-ressources.

Pour une [mémoire tampon](introduction-to-buffers.md), la mémoire tampon entière est la sous-ressource.

Pour une [texture](textures.md), (par exemple, [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d)), chaque niveau MIP est une sous-ressource ; pour un tableau de textures, (par exemple, [**Texture2DArray**](/windows/desktop/direct3dhlsl/sm5-object-texture2darray)), chaque niveau MIP d’une tranche de tableau donnée est une sous-ressource. Le système graphique expose uniquement la possibilité de gérer le mappage des allocations à cette granularité des sous-ressources. Dans le contexte des ressources de streaming, le terme « mappage » fait référence à la façon dont les données sont visibles par le GPU.

## <a name="span-idwithout_tiling__can_t_access_only_a_small_portion_of_mipmap_chainspanspan-idwithout_tiling__can_t_access_only_a_small_portion_of_mipmap_chainspanspan-idwithout_tiling__can_t_access_only_a_small_portion_of_mipmap_chainspanwithout-tiling-cant-access-only-a-small-portion-of-mipmap-chain"></a><span id="Without_tiling__can_t_access_only_a_small_portion_of_mipmap_chain"></span><span id="without_tiling__can_t_access_only_a_small_portion_of_mipmap_chain"></span><span id="WITHOUT_TILING__CAN_T_ACCESS_ONLY_A_SMALL_PORTION_OF_MIPMAP_CHAIN"></span>Sans mosaïque, ne peut pas accéder à une petite partie de la chaîne mipmap


Supposons qu’une application sait qu’une opération de rendu particulière a uniquement besoin d’accéder à une petite partie d’une chaîne mipmap d’image (peut-être pas même la zone complète d’un mipmap donné). Dans l’idéal, l’application peut informer le système graphique de ce besoin. Le système graphique se détournerait alors uniquement pour s’assurer que la mémoire nécessaire est mappée sur le GPU sans pagination dans une mémoire trop importante.

En réalité, sans prise en charge des ressources de streaming, le système graphique ne peut être informé que de la mémoire qui doit être mappée sur le GPU à la granularité des sous-ressources (par exemple, une plage de niveaux de mipmap complets accessibles). Il n’existe aucune erreur de demande dans le système graphique. par conséquent, il est possible d’utiliser un grand nombre de mémoire GPU excédentaire pour mettre en correspondance des sous-ressources complètes avant qu’une commande de rendu faisant référence à une partie de la mémoire ne soit exécutée. Il ne s’agit là que d’un problème qui rend l’utilisation des allocations de mémoire volumineuses difficile dans Direct3D sans la prise en charge de la diffusion en continu des ressources.

## <a name="span-idsoftware_paging_to_break_the_surface_into_smaller_tilesspanspan-idsoftware_paging_to_break_the_surface_into_smaller_tilesspanspan-idsoftware_paging_to_break_the_surface_into_smaller_tilesspansoftware-paging-to-break-the-surface-into-smaller-tiles"></a><span id="Software_paging_to_break_the_surface_into_smaller_tiles"></span><span id="software_paging_to_break_the_surface_into_smaller_tiles"></span><span id="SOFTWARE_PAGING_TO_BREAK_THE_SURFACE_INTO_SMALLER_TILES"></span>Pagination logicielle pour scinder la surface en vignettes plus petites


La pagination de logiciel peut être utilisée pour scinder la surface en mosaïques suffisamment petites pour le matériel à gérer.

Direct3D prend en charge les surfaces [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) avec jusqu’à 16384 pixels sur un côté donné. Une image de 16384 de largeur de 16384 de haut et de 4 octets par pixel consomme 1 Go de mémoire vidéo (et l’ajout de des mipmaps aurait doublé cette quantité). Dans la pratique, il est rare que tous les Go soient référencés dans une seule opération de rendu.

Certains développeurs de jeux modélisent des surfaces de terrain aussi grandes que 128K de 128K. La façon dont ils peuvent travailler sur les GPU existants consiste à scinder la surface en mosaïques suffisamment petites pour gérer le matériel. L’application doit déterminer les vignettes qui peuvent être nécessaires et les charger dans un cache de textures sur le GPU-a Software Paging System.

L’un des inconvénients majeurs de cette approche vient du fait que le matériel ne connaît rien sur la pagination qui se passe : quand une partie d’une image doit être affichée sur l’écran qui chevauche les vignettes, le matériel ne sait pas comment effectuer un filtrage de fonction fixe (c’est-à-dire efficace) sur les vignettes. Cela signifie que l’application gérant sa propre mosaïque logicielle doit recourir à un filtrage manuel de la texture dans le code du nuanceur (ce qui devient très onéreux si un filtre anisotrope de bonne qualité est souhaité) et/ou gaspiller de la mémoire des marges autour des vignettes qui contiennent des données provenant de vignettes voisines afin que le filtrage matériel des fonctions fixes puisse continuer à fournir de l'

## <a name="span-idmaking_tiled_representation_of_surface_allocations_a_first-class_featurespanspan-idmaking_tiled_representation_of_surface_allocations_a_first-class_featurespanspan-idmaking_tiled_representation_of_surface_allocations_a_first-class_featurespanmaking-tiled-representation-of-surface-allocations-a-first-class-feature"></a><span id="Making_tiled_representation_of_surface_allocations_a_first-class_feature"></span><span id="making_tiled_representation_of_surface_allocations_a_first-class_feature"></span><span id="MAKING_TILED_REPRESENTATION_OF_SURFACE_ALLOCATIONS_A_FIRST-CLASS_FEATURE"></span>Création d’une représentation en mosaïque des allocations de surface d’une fonctionnalité de première classe


Si une représentation en mosaïque des allocations de surface est une fonctionnalité de première classe dans le système graphique, l’application peut indiquer au matériel les vignettes à rendre disponibles. De cette façon, moins de mémoire GPU est gaspillée le stockage des régions de surfaces que l’application sait n’est pas accessible, et le matériel peut comprendre comment filtrer les vignettes adjacentes, en éliminant une partie des difficultés rencontrées par les développeurs qui effectuent des mosaïques de logiciels de manière autonome.

Mais pour fournir une solution complète, il est nécessaire de faire ce qu’il faut faire pour traiter le fait que, indépendamment de la prise en charge de l’affichage en mosaïque dans une surface, la dimension surface maximale est actuellement de 16384-nulle près du 128 Ko + que les applications souhaitent déjà. Il suffit d’une approche qui nécessite le matériel pour prendre en charge des tailles de texture plus volumineuses. Toutefois, il existe des coûts et/ou des compromis significatifs pour passer à cet itinéraire.

Le chemin d’accès du filtre de texture Direct3D’s et le chemin de rendu sont déjà saturés en termes de précision dans la prise en charge de 16 000 textures avec les autres exigences, telles que la prise en charge des étendues de Viewport en dehors de la surface pendant le rendu, ou la prise en charge de la texture de la surface pendant le filtrage. Une possibilité consiste à définir un compromis de telle sorte que la taille de la texture augmente au-delà de 16 Ko, la fonctionnalité/précision est indiquée d’une certaine manière. Toutefois, même avec cette concession, des coûts matériels supplémentaires peuvent être nécessaires en termes d’adressage dans l’ensemble du système matériel pour atteindre des tailles de texture plus volumineuses.

## <a name="span-idissue_with_large_textures__precision_for_locations_on_surfacespanspan-idissue_with_large_textures__precision_for_locations_on_surfacespanspan-idissue_with_large_textures__precision_for_locations_on_surfacespanissue-with-large-textures-precision-for-locations-on-surface"></a><span id="Issue_with_large_textures__precision_for_locations_on_surface"></span><span id="issue_with_large_textures__precision_for_locations_on_surface"></span><span id="ISSUE_WITH_LARGE_TEXTURES__PRECISION_FOR_LOCATIONS_ON_SURFACE"></span>Problème avec les textures volumineuses : précision pour les emplacements sur la surface


L’un des problèmes qui entrent dans la lecture des textures est très élevé, c’est que les coordonnées de la texture à virgule flottante simple précision (et les interpolateurs associés pour prendre en charge la pixellisation) sont à court de précision pour spécifier des emplacements sur la surface avec précision. Le filtrage de texture instable s’ensuit. Une option coûteuse serait d’exiger la prise en charge de l’interpolateur double précision, bien que cela puisse être excessif en raison d’une alternative raisonnable.

## <a name="span-idenabling_multiple_resources_of_different_dimensions_to_share_memoryspanspan-idenabling_multiple_resources_of_different_dimensions_to_share_memoryspanspan-idenabling_multiple_resources_of_different_dimensions_to_share_memoryspanenabling-multiple-resources-of-different-dimensions-to-share-memory"></a><span id="Enabling_multiple_resources_of_different_dimensions_to_share_memory"></span><span id="enabling_multiple_resources_of_different_dimensions_to_share_memory"></span><span id="ENABLING_MULTIPLE_RESOURCES_OF_DIFFERENT_DIMENSIONS_TO_SHARE_MEMORY"></span>Activation de plusieurs ressources de dimensions différentes pour partager la mémoire


Un autre scénario pouvant être utilisé par les ressources de streaming consiste à permettre à plusieurs ressources de différents formats/Dimensions de partager la même mémoire. Parfois, les applications ont des ensembles exclusifs de ressources qui ne doivent pas être utilisés en même temps, ou des ressources qui sont créées uniquement pour une utilisation très courte, puis détruites, suivies de la création d’autres ressources. Une forme de général qui peut être en dehors des « ressources de streaming » est qu’il est possible d’autoriser l’utilisateur à pointer plusieurs ressources différentes au même (chevauchement) de la mémoire. En d’autres termes, la création et la destruction des « ressources » (qui définissent une dimension/format, etc.) peuvent être découplées à partir de la gestion de la mémoire sous-jacente des ressources du point de vue de l’application.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Ressources de diffusion en continu](streaming-resources.md)

 

 