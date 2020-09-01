---
title: Étape Vertex Shader (VS)
description: L’étape vertex shader (VS) traite les vertex, en effectuant généralement des opérations telles que les transformations, les pelures et l’éclairage. Un nuanceur de sommets prend un vertex d’entrée unique et produit un seul vertex de sortie.
ms.assetid: 5133C4BB-B4E6-4697-9276-F718AD44869C
keywords:
- Étape Vertex Shader (VS)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 88a990470d0ff8aea5c6415584bd63b2b4b65981
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156053"
---
# <a name="vertex-shader-vs-stage"></a>Étape Vertex Shader (VS)


L’étape vertex shader (VS) traite les vertex, en effectuant généralement des opérations telles que les transformations, les pelures et l’éclairage. Un nuanceur de sommets prend un vertex d’entrée unique et produit un seul vertex de sortie.

## <a name="span-idpurpose_and_usesspanspan-idpurpose_and_usesspanspan-idpurpose_and_usesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Usage et utilisations


L’étape nuanceur de sommets (VS) est utilisée pour le traitement individuel par vertex, par exemple :

-   Transformations
-   Apparence
-   Métamorphose
-   Éclairage par vertex

L’étape du nuanceur de sommets est une étape de nuanceur programmable ; Il est affiché sous la forme d’un bloc arrondi dans le diagramme de [pipeline graphique](graphics-pipeline.md) . Cette étape de nuanceur utilise le modèle de nuanceur 4,0 [Common-Shader Core](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-common-core).

L’étape vertex-shader (VS) traite les vertex de l’assembleur d’entrée. Les nuanceurs vertex fonctionnent toujours sur un vertex d’entrée unique et produisent un seul vertex de sortie. L’étape du nuanceur de sommets doit toujours être active pour que le pipeline s’exécute. Si aucune modification ou transformation de vertex n’est requise, un nuanceur de sommets directs doit être créé et défini sur le pipeline.

Chaque vertex d’entrée de nuanceur de sommets peut être constitué de vecteurs jusqu’à 16 32 bits (jusqu’à 4 composants chacun). Chaque vertex de sortie peut être constitué de plusieurs vecteurs à 4 16 32 bits. Tous les nuanceurs de vertex doivent avoir au minimum une entrée et une sortie, ce qui peut être aussi limité qu’une seule valeur scalaire.

L’étape vertex-shader peut consommer deux valeurs générées par le système à partir de l’assembleur d’entrée : VertexID et InstanceID (consultez valeurs système et sémantiques). Étant donné que VertexID et InstanceID sont tous deux significatifs au niveau d’un vertex, et que les ID générés par le matériel peuvent uniquement être introduits dans la première étape qui les comprend, ces valeurs d’ID peuvent uniquement être chargées dans l’étape de nuanceur de sommets.

Les nuanceurs vertex sont toujours exécutés sur tous les vertex, y compris les vertex adjacents dans les topologies de la primitive d’entrée avec contiguïté. Le nombre de fois que le nuanceur vertex a été exécuté peut être interrogé à partir du processeur à l’aide des statistiques de pipeline VSInvocations.

Un nuanceur de sommets peut effectuer des opérations d’échantillonnage de charge et de texture lorsque les dérivés de l’espace écran ne sont pas requis (à l’aide des fonctions intrinsèques HLSL : [Sample (DirectX HLSL texture Object)](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-sample), [SAMPLECMPLEVELZERO (DirectX HLSL texture Object)](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-samplecmplevelzero)et [SampleGrad (DirectX HLSL texture Object)](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-samplegrad)).

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entrée


Un vertex unique, avec des valeurs générées par le système VertexID et InstanceID. Chaque vertex d’entrée de nuanceur de sommets peut être constitué de vecteurs jusqu’à 16 32 bits (jusqu’à 4 composants chacun).

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Sortie


Un seul vertex. Chaque vertex de sortie peut être constitué de plusieurs vecteurs à 4 16 32 bits.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Pipeline graphique](graphics-pipeline.md)

 

 