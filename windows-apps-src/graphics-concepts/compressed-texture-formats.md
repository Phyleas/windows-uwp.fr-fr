---
title: "Formats de texture compressés"
description: "Cette section contient des informations sur l’organisation interne des formats de texture compressés."
ms.assetid: 24D17B9F-8CA7-4006-9E0F-178C6B3CAEC9
keywords: "Formats de texture compressés"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 1bf94307093913c3b89b1d2a80e1e77d8dec81eb
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="compressed-texture-formats"></a>Formats de texture compressés


Cette section contient des informations sur l’organisation interne des formats de texture compressés. Vous n'avez pas besoin de ces informations pour utiliser des textures compressées, car vous pouvez utiliser les fonctions Direct3D pour effectuer la conversion entre les formats compressés. Ces informations vous seront cependant utiles si vous souhaitez utiliser directement des données de surface compressées.

Direct3D utilise un format de compression qui divise les mappages de texture en blocs de 4x4texels. Si la texture ne contient pas de transparence (si elle est opaque) ou si la transparence est spécifiée par un alpha 1bit, un bloc de 8octets représente le bloc de la carte de texture. Si la carte de texture contient des texels transparents, elle est représentée par un bloc de 16octets utilisant un canal alpha.

Toute texture unique doit spécifier que ses données sont stockées sous la forme de 64 ou 128bits par groupe de 16texels. Si des blocs de 64bits (format BC1) sont utilisés pour la texture, il est possible de combiner les formats opaque et alpha 1bit pour chaque bloc au sein de la même texture. En d’autres termes, la comparaison de la magnitude de l’entier non signé de color\_0 et color\_1 est effectuée de manière unique pour chaque bloc de 16texels.

Lorsque des blocs de 128bits sont utilisés, le canal alpha doit être spécifié au format explicite (BC2) ou en mode interpolé (format BC3) pour l'ensemble de la texture. Comme pour la couleur, lorsque le mode interpolé est sélectionné, il est possible d'appliquer, bloc par bloc, le mode huit valeurs alpha interpolées ou six valeurs alpha interpolées. Là encore, la comparaison de la magnitude d’alpha\_0 et alpha\_1 est effectuée de manière unique pour chaque bloc.

Le pas pour les formats BCn est mesuré en octets (et non en blocs). Par exemple, si vous avez une largeur de 16, vous obtiendrez un pas de quatre blocs (4\*8 pour BC1, 4\*16 pour BC2 ou BC3).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Ressources de texture compressées](compressed-texture-resources.md)

 

 



