---
title: Fonctionnalités d’échantillonnage de texture des ressources de diffusion en continu
description: Ressources de streaming les fonctionnalités d’échantillonnage de texture incluent l’obtention de commentaires sur l’état du nuanceur sur les zones mappées, la vérification de la mise en correspondance de toutes les données qui ont été consultées dans la ressource, le verrouillage pour aider les nuanceurs à éviter les zones dans les ressources de streaming mipmapped qui ne sont pas mappées, et la découverte de ce que le LOD minimal est
ms.assetid: C2B2DD69-8354-417A-894D-6235A8B48B53
keywords:
- Fonctionnalités d’échantillonnage de texture des ressources de diffusion en continu
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 823b7b6ba7835f62277e15fa41fb968c6f4a167f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173043"
---
# <a name="streaming-resources-texture-sampling-features"></a>Fonctionnalités d’échantillonnage de texture des ressources de diffusion en continu


Ressources de streaming les fonctionnalités d’échantillonnage de texture incluent l’obtention de commentaires sur l’état du nuanceur sur les zones mappées, la vérification de la mise en correspondance de toutes les données qui ont été consultées dans la ressource, le verrouillage pour aider les nuanceurs à éviter les zones dans les ressources de streaming mipmapped qui ne sont pas mappées, et la découverte de ce que le LOD minimal est

## <a name="span-idrequirements_of_streaming_resources_texture_sampling_featuresspanspan-idrequirements_of_streaming_resources_texture_sampling_featuresspanspan-idrequirements_of_streaming_resources_texture_sampling_featuresspanrequirements-of-streaming-resources-texture-sampling-features"></a><span id="Requirements_of_streaming_resources_texture_sampling_features"></span><span id="requirements_of_streaming_resources_texture_sampling_features"></span><span id="REQUIREMENTS_OF_STREAMING_RESOURCES_TEXTURE_SAMPLING_FEATURES"></span>Spécifications des ressources de streaming fonctionnalités d’échantillonnage de texture


Les fonctionnalités d’échantillonnage de texture décrites ici nécessitent un niveau de prise en charge de ressources de streaming de niveau [2](tier-2.md) .

## <a name="span-idshader_status_feedback_about_mapped_areasspanspan-idshader_status_feedback_about_mapped_areasspanspan-idshader_status_feedback_about_mapped_areasspanshader-status-feedback-about-mapped-areas"></a><span id="Shader_status_feedback_about_mapped_areas"></span><span id="shader_status_feedback_about_mapped_areas"></span><span id="SHADER_STATUS_FEEDBACK_ABOUT_MAPPED_AREAS"></span>Commentaires sur l’état du nuanceur sur les zones mappées


Toute instruction de nuanceur qui lit et/ou écrit dans une ressource de streaming entraîne l’enregistrement des informations d’État. Cet État est exposé sous la forme d’une valeur de retour supplémentaire facultative sur chaque instruction d’accès aux ressources qui passe dans un registre temporaire 32 bits. Le contenu de la valeur de retour est opaque. Autrement dit, la lecture directe par le programme Shader n’est pas autorisée. Toutefois, vous pouvez utiliser la fonction [**CheckAccessFullyMapped**](/windows/desktop/direct3dhlsl/checkaccessfullymapped) pour extraire les informations d’État.

## <a name="span-idfully_mapped_checkspanspan-idfully_mapped_checkspanspan-idfully_mapped_checkspanfully-mapped-check"></a><span id="Fully_mapped_check"></span><span id="fully_mapped_check"></span><span id="FULLY_MAPPED_CHECK"></span>Vérification entièrement mappée


La fonction [**CheckAccessFullyMapped**](/windows/desktop/direct3dhlsl/checkaccessfullymapped) interprète l’état retourné par un accès à la mémoire et indique si toutes les données en cours d’accès ont été mappées dans la ressource. **CheckAccessFullyMapped** retourne true (0xFFFFFFFF) si les données ont été mappées ou false (0x00000000) si les données n’ont pas été mappées.

Pendant les opérations de filtre, parfois, le poids d’une Texel donnée finit par 0,0. Par exemple, un échantillon linéaire avec des coordonnées de texture qui se trouvent directement sur un centre de Texel : 3 les autres éléments texels (qui peuvent varier en fonction du matériel) contribuent au filtre, mais avec un poids de 0. Ces texels de poids 0 ne contribuent pas du tout au résultat du filtre. par conséquent, s’ils se trouvent sur des vignettes **null** , ils ne sont pas comptabilisés comme un accès non mappé. Notez que la même garantie s’applique aux filtres de texture qui incluent plusieurs niveaux MIP ; Si les texels de l’un des des mipmaps ne sont pas mappés mais que le poids de ces texels est 0, ces texels ne sont pas comptabilisés comme un accès non mappé.

Lors de l’échantillonnage à partir d’un format qui a moins de 4 composants (tels que le \_ format dxgi \_ R8 \_ UNORM), tous les texels qui se trouvent sur des vignettes **null** entraînent le signalement d’un accès mappé **null** , quels que soient les composants que le nuanceur examine réellement dans le résultat. Par exemple, la lecture de R8 \_ UNORM et le masquage du résultat de lecture dans le nuanceur avec. GBA/. YZW ne semblent pas avoir besoin de lire la texture. Toutefois, si l’adresse Texel est une vignette mappée **null** , l’opération est toujours comptabilisée comme un accès de mappage **null** .

Le nuanceur peut vérifier l’État et poursuivre les actions à entreprendre en cas d’échec. Par exemple, un cours d’action peut enregistrer « échecs » (par exemple, via UAV Write) et/ou émettre une autre lecture ancrée à un LOD plus grossiste connu pour être mappé. Une application peut souhaiter effectuer un suivi des accès réussis afin d’obtenir une idée de la partie de l’ensemble mappé de vignettes ayant fait l’objet d’un accès.

L’une des complications pour la journalisation est qu’il n’existe aucun mécanisme pour signaler le jeu exact de vignettes qui aurait été accédé. L’application peut effectuer des suppositions prudentes en se basant sur les coordonnées utilisées pour l’accès, ainsi que sur l’utilisation de l’instruction LOD. par exemple, [**tex2Dlod**](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-tex2dlod)) retourne le calcul du LOD matériel.

Une autre compliquation est que beaucoup d’accès seront sur les mêmes vignettes, si bien qu’un grand nombre d’enregistrements redondants se produira et éventuellement de contention de mémoire. Cela peut être pratique si le matériel peut avoir la possibilité de ne pas prendre en compte les accès de vignette s’ils ont été signalés ailleurs auparavant. L’état de ce suivi peut peut-être être réinitialisé à partir de l’API (probablement au niveau des limites de cadre).

## <a name="span-idper-sample_minlod_clampspanspan-idper-sample_minlod_clampspanspan-idper-sample_minlod_clampspanper-sample-minlod-clamp"></a><span id="Per-sample_MinLOD_clamp"></span><span id="per-sample_minlod_clamp"></span><span id="PER-SAMPLE_MINLOD_CLAMP"></span>Clamp MinLOD par exemple


Pour aider les nuanceurs à éviter les zones dans les ressources de diffusion en continu mipmapped qui ne sont pas mappées, la plupart des instructions de nuanceur qui impliquent l’utilisation d’un échantillonneur (filtrage) ont un mode qui permet au nuanceur de passer un paramètre de pincement float32 MinLOD supplémentaire à l’échantillon de texture. Cette valeur se trouve dans l’espace de nombre mipmap de la vue, par opposition à la ressource sous-jacente.

Le matériel s’exécute au ` max(fShaderMinLODClamp,fComputedLOD) ` même endroit dans le calcul LOD où la bride MinLOD par ressource se produit, ce qui est également un [**nombre max**](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-max)().

Si le résultat de l’application de la bride LOD par échantillon et de tout autre clamp LOD défini dans l’échantillonneur est un jeu vide, le résultat est le même que le résultat de l’accès hors limites en tant que bride minLOD par ressource : 0 pour les composants dans le format de surface et les valeurs par défaut pour les composants manquants.

L’instruction LOD (par exemple, [**tex2Dlod**](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-tex2dlod)), qui met à jour la clamp minLOD par exemple décrite ici, retourne à la fois un LOD ancré et non ancré. Le LOD ancré renvoyé à partir de cette instruction LOD reflète tout le verrouillage, y compris la bride par ressource, mais pas une bride par échantillon. Par exemple, la bride est contrôlée et connue du nuanceur, de sorte que l’auteur du nuanceur peut appliquer manuellement cette bride à la valeur de retour de l’instruction LOD, si vous le souhaitez.

## <a name="span-idmin_max_reduction_filteringspanspan-idmin_max_reduction_filteringspanspan-idmin_max_reduction_filteringspanminmax-reduction-filtering"></a><span id="Min_Max_reduction_filtering"></span><span id="min_max_reduction_filtering"></span><span id="MIN_MAX_REDUCTION_FILTERING"></span>Filtrage de réduction min/max


Les applications peuvent choisir de gérer leurs propres structures de données qui les informent de ce à quoi ressemblent les mappages pour une ressource de streaming. Il peut s’agir, par exemple, d’une surface qui contient un Texel pour contenir des informations sur chaque vignette dans une ressource de streaming. Il est possible de stocker le premier LOD qui est mappé à un emplacement de mosaïque donné. En procédant à un échantillonnage minutieux de cette structure de données de la même façon que la ressource de streaming est censée être échantillonnée, il est possible de découvrir ce que le LOD minimal qui est entièrement mappé pour un encombrement de filtre de texture entier est. Pour faciliter ce processus, Direct3D 11,2 introduit un nouveau mode d’échantillonnage à usage général, le filtrage min/max.

L’utilitaire de filtrage min/max pour le suivi LOD peut être utile à d’autres fins, telles que, peut-être le filtrage des surfaces de profondeur.

Le filtrage de réduction min/max est un mode sur les échantillonneurs qui extrait le même jeu de texels qu’un filtre de texture normal peut extraire. Mais au lieu de fusionner les valeurs pour produire une réponse, elle retourne la valeur min () ou Max () des texels extraits, en fonction du composant (par exemple, le minimum de toutes les valeurs R, séparément du nombre minimal de toutes les valeurs G, etc.).

Les opérations min/max suivent les règles de précision arithmétiques Direct3D. L’ordre des comparaisons n’a pas d’importance.

Pendant les opérations de filtre qui ne sont pas min/max, parfois, le poids d’une Texel donnée finit par 0,0. Par exemple, un échantillon linéaire avec des coordonnées de texture qui se trouvent directement sur un centre de texels ; dans ce cas, 3 autres texels (qui peuvent varier en fonction du matériel) contribuent au filtre, mais avec un poids de 0. Pour l’un de ces texels, qui serait 0 poids sur un filtre non-min/max, si le filtre est min/max, ces texels ne contribuent toujours pas au résultat (et les poids n’affectent pas les opérations de filtre min/max).

La prise en charge de cette fonctionnalité dépend de la prise en charge de [niveau 2](tier-2.md) pour les ressources de streaming.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Accès du pipeline aux ressources de diffusion en continu](pipeline-access-to-streaming-resources.md)

 

 