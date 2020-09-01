---
title: Utilisation de valeurs générées par le système
description: Les valeurs générées par le système sont générées par l’étape de l’assembleur d’entrée (en fonction de la sémantique d’entrée fournie par l’utilisateur) pour permettre une certaine efficacité dans les opérations de nuanceur.
ms.assetid: C7CBA81D-68CA-4E9A-95E3-8185C280C843
keywords:
- Utilisation de valeurs générées par le système
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7217b52c6e9f9882997649c5f843eb119d741e0b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156153"
---
# <a name="span-iddirect3dconceptsusing_system-generated_valuesspanusing-system-generated-values"></a><span id="direct3dconcepts.using_system-generated_values"></span>Utilisation de valeurs générées par le système


Les valeurs générées par le système sont générées par l’étape de l' [assembleur d’entrée](input-assembler-stage--ia-.md) (en fonction de la [sémantique](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics)d’entrée fournie par l’utilisateur) pour permettre une certaine efficacité dans les opérations de nuanceur. En joignant des données, telles qu’un ID d’instance (visible à l' [étape de nuanceur de sommets (vs)](vertex-shader-stage--vs-.md)), un ID de vertex (visible pour vs) ou un ID primitif (visible pour la phase de nuanceur de pixels de [géométrie (GS](geometry-shader-stage--gs-.md)) de l’étape de / [Pixel](pixel-shader-stage--ps-.md)), une étape de nuanceur ultérieure peut rechercher ces valeurs système afin d’optimiser le traitement.

Par exemple, l’étape VS peut Rechercher l’ID d’instance pour récupérer des données par vertex supplémentaires pour le nuanceur ou pour effectuer d’autres opérations. les étapes GS et PS peuvent utiliser l’ID primitif pour extraire les données par primitive de la même façon.

## <a name="span-idvertexidspanspan-idvertexidspanspan-idvertexidspanvertexid"></a><span id="VertexID"></span><span id="vertexid"></span><span id="VERTEXID"></span>VertexID


Un ID de vertex est utilisé par chaque étape du nuanceur pour identifier chaque vertex. Il s’agit d’un entier non signé 32 bits dont la valeur par défaut est 0. Elle est assignée à un vertex lorsque la primitive est traitée par l’étape de l' [assembleur d’entrée (IA)](input-assembler-stage--ia-.md). Attachez la sémantique de l’ID de vertex à la déclaration d’entrée du nuanceur pour indiquer à l’étape IA de générer un ID par vertex.

L’IA ajoutera un ID de vertex à chaque vertex pour une utilisation par les étapes du nuanceur. Pour chaque appel de dessin, l’ID de vertex est incrémenté de 1. Dans les appels de dessin indexés, le nombre rétablit la valeur de départ. Si l’ID de vertex déborde (dépasse 2 ³ ² – 1), il est renvoyé à la ligne à 0.

Pour tous les types primitifs, les vertex ont un ID de vertex qui leur est associé (indépendamment de l’adjacence).

## <a name="span-idprimitiveidspanspan-idprimitiveidspanspan-idprimitiveidspanprimitiveid"></a><span id="PrimitiveID"></span><span id="primitiveid"></span><span id="PRIMITIVEID"></span>PrimitiveID


Un ID primitif est utilisé par chaque étape du nuanceur pour identifier chaque primitive. Il s’agit d’un entier non signé 32 bits dont la valeur par défaut est 0. Il est assigné à une primitive lorsque la primitive est traitée par l' [étape de l’assembleur d’entrée (IA)](input-assembler-stage--ia-.md). Pour indiquer à l’étape IA de générer un ID primitif, attachez la sémantique de l’ID primitif à la déclaration d’entrée du nuanceur.

L’étape IA ajoutera un ID primitif à chaque primitive en vue de son utilisation par l' [étape de nuanceur Geometry (GS)](geometry-shader-stage--gs-.md) ou par l' [étape de nuanceur de sommets](vertex-shader-stage--vs-.md) (par rapport à la première étape active après l’étape IA). Pour chaque appel de dessin indexé, l’ID primitif est incrémenté de 1. Toutefois, l’ID primitif réinitialise à 0 chaque fois qu’une nouvelle instance commence. Tous les autres appels de dessin ne modifient pas la valeur de l’ID d’instance. Si l’ID d’instance déborde (dépasse 2 ³ ² – 1), il est renvoyé à la ligne à 0.

L' [étape de nuanceur de pixels (PS)](pixel-shader-stage--ps-.md) n’a pas d’entrée distincte pour un ID primitif ; Toutefois, toute entrée de nuanceur de pixels qui spécifie un ID primitif utilise un mode d’interpolation constante.

Il n’existe aucune prise en charge pour la génération automatique d’un ID primitif pour les primitives adjacentes. Pour les types primitifs avec contiguïté, tel qu’une bande triangulaire avec contiguïté, un ID primitif est conservé uniquement pour les primitives intérieures (les primitives non adjacentes), tout comme l’ensemble de primitives dans une bande de triangle sans contiguïté.

## <a name="span-idinstanceidspanspan-idinstanceidspanspan-idinstanceidspaninstanceid"></a><span id="InstanceID"></span><span id="instanceid"></span><span id="INSTANCEID"></span>InstanceID


Un ID d’instance est utilisé par chaque étape du nuanceur pour identifier l’instance de la géométrie en cours de traitement. Il s’agit d’un entier non signé 32 bits dont la valeur par défaut est 0.

L' [étape de l’assembleur d’entrée](input-assembler-stage--ia-.md) ajoute un ID d’instance à chaque vertex si la déclaration d’entrée du nuanceur de sommets comprend la sémantique de l’ID d’instance. Pour chaque appel de dessin indexé, l’ID d’instance est incrémenté de 1. Tous les autres appels de dessin ne modifient pas la valeur de l’ID d’instance. Si l’ID d’instance déborde (dépasse 2 ³ ² – 1), il est renvoyé à la ligne à 0.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Tels


L’illustration suivante montre comment les valeurs système sont attachées à une bande triangulaire d’instance dans l' [étape d’assembleur d’entrée (IA)](input-assembler-stage--ia-.md).

![illustration des valeurs système pour une bande triangulaire instanciée](images/d3d10-ia-example.png)

Ces tableaux affichent les valeurs système générées pour les deux instances de la même bande triangulaire. La première instance (U d’instance) est affichée en bleu, la deuxième instance (instance V) est affichée en vert. Les lignes pleines connectent les vertex dans les primitives, les lignes en pointillés connectent les vertex adjacents.

Les tableaux suivants indiquent les valeurs générées par le système pour l’instance U.

| Données de vertex    | C, U | D, U | E, U | F, U | G, U | H, U | I, U | J, U | K, U | L, U |
|----------------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| **VertexID**   | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   |
| **InstanceID** | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |

 

L’instance de la bande triangulaire U a 3 primitives triangulaires, avec les valeurs générées par le système suivantes :

|                 |     |     |     |
|-----------------|-----|-----|-----|
| **PrimitiveID** | 0   | 1   | 2   |
| **InstanceID**  | 0   | 0   | 0   |

 

Les tableaux suivants indiquent les valeurs générées par le système pour l’instance V.

| Données de vertex    | C, V | D, V | E, V | F, V | G, V | H, V | I, V | J, V | K, V | L, V |
|----------------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| **VertexID**   | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   |
| **InstanceID** | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   |

 

L’instance de la bande triangulaire V a 3 primitives triangulaires, avec les valeurs générées par le système suivantes :

|                 |     |     |     |
|-----------------|-----|-----|-----|
| **PrimitiveID** | 0   | 1   | 2   |
| **InstanceID**  | 1   | 1   | 1   |

 

L' [étape de l’assembleur d’entrée](input-assembler-stage--ia-.md) génère les ID (vertex, primitif et instance); Notez également que chaque instance reçoit un ID d’instance unique. Les données se terminent par la bande coupée, qui sépare chaque instance de la bande de triangle.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Étape de l’assembleur d’entrée (IA)](input-assembler-stage--ia-.md)

 

 