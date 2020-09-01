---
title: Étape Stream Output
description: La sortie de flux (SO) envoie en continu des données de vertex (ou flux) à partir de l’étape active précédente vers une ou plusieurs mémoires tampons en mémoire. Les données transmises en mémoire vers la mémoire peuvent être rétablies dans le pipeline en tant que données d’entrée, ou en lecture-retour à partir de l’UC.
ms.assetid: DE89E99F-39BC-4B34-B80F-A7D373AA7C0A
keywords:
- Étape Stream Output
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f56036ecc083d72f552954860d04750c1c83b8b6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156203"
---
# <a name="stream-output-so-stage"></a>Étape Stream Output


La sortie de flux (SO) envoie en continu des données de vertex (ou flux) à partir de l’étape active précédente vers une ou plusieurs mémoires tampons en mémoire. Les données transmises en mémoire vers la mémoire peuvent être rétablies dans le pipeline en tant que données d’entrée, ou en lecture-retour à partir de l’UC.

## <a name="span-idpurpose_and_usesspanspan-idpurpose_and_usesspanspan-idpurpose_and_usesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Usage et utilisations


![diagramme de l’emplacement de l’étape de sortie de flux dans le pipeline](images/d3d10-pipeline-stages-so.png)

L’étape de flux de sortie diffuse en continu des données primitives du pipeline vers la mémoire de façon à ce que le rastériseur. Les données de l’étape précédente peuvent être diffusées en continu vers la mémoire et/ou transmises au rastériseur. Les données transmises en mémoire vers la mémoire peuvent être rétablies dans le pipeline en tant que données d’entrée, ou en lecture-retour à partir de l’UC.

Les données transmises en mémoire vers la mémoire peuvent être lues dans le pipeline dans une passe de rendu ultérieure, ou peuvent être copiées dans une ressource intermédiaire (afin qu’elles puissent être lues par l’UC). La quantité de données transmises en continu peut varier. Direct3D est conçu pour gérer les données sans qu’il soit nécessaire d’interroger (le GPU) sur la quantité de données écrites.--&gt;

Il existe deux façons d’alimenter les données de sortie de flux dans le pipeline :

-   Les données de sortie de flux peuvent être réintégrées à l’étape de l’assembleur d’entrée.
-   Les données de sortie de flux peuvent être lues par des nuanceurs programmables à l’aide de fonctions de [chargement](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-load) .

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entrée


Données de vertex à partir d’une étape de nuanceur précédente.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Sortie


La sortie de flux (SO) envoie en continu des données de vertex (ou flux) à partir de l’étape active précédente, telles que l’étape de nuanceur Geometry (GS), vers une ou plusieurs mémoires tampons en mémoire. Si l’étape de nuanceur Geometry (GS) est inactive, l’étape sortie de flux (SO) génère en continu des données de vertex de l’étape de nuanceur de domaine (DS) vers des mémoires tampons en mémoire (ou si le service d’annuaire est également inactif, à partir de l’étape de nuanceur de sommets (VS)).

Lorsqu’un triangle ou une frange est lié à l’étape de l’assembleur d’entrée (IA), chaque bande est convertie en liste avant d’être diffusée en continu. Les vertex sont toujours écrits comme des primitives complètes (par exemple, 3 sommets à la fois pour les triangles); les primitives incomplètes ne sont jamais transmises en continu. Les types primitifs avec contiguïté ignorent les données d’adjacence avant la diffusion des données en continu.

L’étape flux-sortie prend en charge jusqu’à 4 mémoires tampons simultanément.

-   Si vous diffusez des données dans plusieurs mémoires tampons, chaque mémoire tampon peut uniquement capturer un seul élément (jusqu’à 4 composants) de données par vertex, avec une Stride de données implicite égale à la largeur d’élément dans chaque mémoire tampon (compatible avec la manière dont les mémoires tampons d’élément unique peuvent être liées à l’entrée dans des étapes de nuanceur). En outre, si les mémoires tampons ont des tailles différentes, l’écriture s’arrête dès que l’une des mémoires tampons est pleine.
-   Si vous diffusez des données dans une seule mémoire tampon, la mémoire tampon peut capturer jusqu’à 64 composants scalaires de données par vertex (256 octets ou moins) ou le nombre de vertex peut atteindre jusqu’à 2048 octets.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Pipeline graphique](graphics-pipeline.md)

 

 