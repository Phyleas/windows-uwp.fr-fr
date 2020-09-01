---
title: Exposition des ressources de diffusion en continu HLSL
description: Une syntaxe HLSL (High Level Shader Language) spécifique est requise pour prendre en charge les ressources de streaming dans le nuancier Model 5.
ms.assetid: 00A40D82-0565-43DC-82AB-0675B7E772E3
keywords:
- Exposition des ressources de diffusion en continu HLSL
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: decb0ff2d791417326a70b06c0fdd6a25ea2d119
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173083"
---
# <a name="hlsl-streaming-resources-exposure"></a>Exposition des ressources de diffusion en continu HLSL


Une syntaxe HLSL (High Level Shader Language) spécifique est requise pour prendre en charge les ressources de streaming dans le [nuancier Model 5](/windows/desktop/direct3dhlsl/d3d11-graphics-reference-sm5).

La syntaxe HLSL pour Shader Model 5 est autorisée uniquement sur les appareils avec prise en charge des ressources de streaming. Chaque méthode HLSL pertinente pour les ressources de streaming dans le tableau suivant accepte un (e) (Feedback) ou deux (verrouillage et commentaires dans cet ordre) des paramètres facultatifs supplémentaires. Par exemple, voici un **exemple** de méthode :

**Exemple (échantillonneur, emplacement \[ , décalage \[ , bride \[ , commentaires \] \] \] )**

[**Texture2D. Sample (S, float, int, float, uint)**](/windows/desktop/direct3dhlsl/t2darray-sample-s-float-int-float-uint-)est un exemple de méthode d' **exemple** .

Les paramètres décalage, Clamp et Feedback sont facultatifs. Vous devez spécifier tous les paramètres facultatifs jusqu’à celui dont vous avez besoin, ce qui est cohérent avec les règles C++ pour les arguments de fonction par défaut. Par exemple, si l’état des commentaires est requis, les paramètres de décalage et de fixation doivent être fournis explicitement à **Sample**, même s’ils ne sont pas nécessaires logiquement.

Le paramètre clamp est une valeur flottante scalaire. La valeur littérale de Clamp = 0.0 f indique que l’opération de verrouillage n’est pas effectuée.

Le paramètre feedback est une variable **uint** que vous pouvez fournir à la fonction [**CheckAccessFullyMapped**](/windows/desktop/direct3dhlsl/checkaccessfullymapped) intrinsèque d’interrogation de l’accès mémoire. Vous ne devez pas modifier ni interpréter la valeur du paramètre Feedback ; Toutefois, le compilateur ne fournit aucune analyse et aucun diagnostic avancés pour déterminer si vous avez modifié la valeur.

Voici la syntaxe de [**CheckAccessFullyMapped**](/windows/desktop/direct3dhlsl/checkaccessfullymapped):

**bool CheckAccessFullyMapped (dans uint FeedbackVar);**

[**CheckAccessFullyMapped**](/windows/desktop/direct3dhlsl/checkaccessfullymapped) interprète la valeur de *FeedbackVar* et retourne true si toutes les données en cours d’accès ont été mappées dans la ressource ; Sinon, **CheckAccessFullyMapped** retourne false.

Si le paramètre clamp ou feedback est présent, le compilateur émet une variante de l’instruction de base. Par exemple, l’exemple d’une ressource de streaming génère l' `sample_cl_s` instruction.

Si ni clamp ni Feedback n’est spécifié, le compilateur émet l’instruction de base, afin qu’il n’y ait aucune modification par rapport au comportement actuel.

La valeur Clamp de 0.0 f indique qu’aucune bride n’est exécutée ; ainsi, le compilateur de pilotes peut affiner l’instruction sur le matériel cible. Si les commentaires sont un registre NULL dans une instruction, les commentaires ne sont pas utilisés ; par conséquent, le compilateur de pilotes peut adapter l’instruction à l’architecture cible.

Si le compilateur HLSL déduit que la bride est 0.0 f et que les commentaires sont inutilisés, le compilateur émet l’instruction de base correspondante (par exemple, `sample` plutôt que `sample_cl_s` ).

Si un accès aux ressources en continu se compose de plusieurs instructions de code d’octet constitutives, par exemple, pour des ressources structurées, le compilateur agrège les valeurs de commentaires individuelles via l’opération ou pour produire la valeur de retour finale. Par conséquent, vous voyez une valeur de commentaire unique pour un accès complexe.

Il s’agit de la table de résumé des méthodes HLSL qui sont modifiées pour prendre en charge les commentaires et/ou la fixation. Tout fonctionne sur des ressources en mosaïque et non diffusées de toutes les dimensions. Les ressources qui ne sont pas en streaming semblent toujours être entièrement mappées.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><a href="/windows/desktop/direct3dhlsl/d3d11-graphics-reference-sm5-objects">Objets HLSL</a> </th>
<th align="left">Les méthodes intrinsèques avec l’option Feedback (*)-possèdent également l’option clamp</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>GRAVE Texture2D</p>
<p>GRAVE Texture2DArray</p>
<p>TextureCUBE</p>
<p>TextureCUBEArray</p></td>
<td align="left"><p>Gather</p>
<p>GatherRed</p>
<p>GatherGreen</p>
<p>GatherBlue</p>
<p>GatherAlpha</p>
<p>GatherCmp</p>
<p>GatherCmpRed</p>
<p>GatherCmpGreen</p>
<p>GatherCmpBlue</p>
<p>GatherCmpAlpha</p></td>
</tr>
<tr class="even">
<td align="left"><p>GRAVE Texture1D</p>
<p>GRAVE Texture1DArray</p>
<p>GRAVE Texture2D</p>
<p>GRAVE Texture2DArray</p>
<p>GRAVE Texture3D</p>
<p>TextureCUBE</p>
<p>TextureCUBEArray</p></td>
<td align="left"><p>Exemple</p>
<p>SampleBias*</p>
<p>SampleCmp*</p>
<p>SampleCmpLevelZero</p>
<p>SampleGrad*</p>
<p>SampleLevel</p></td>
</tr>
<tr class="odd">
<td align="left"><p>GRAVE Texture1D</p>
<p>GRAVE Texture1DArray</p>
<p>GRAVE Texture2D</p>
<p>Texture2DMS</p>
<p>GRAVE Texture2DArray</p>
<p>Texture2DArrayMS</p>
<p>GRAVE Texture3D</p>
<p>GRAVE Mémoire tampon</p>
<p>GRAVE ByteAddressBuffer</p>
<p>GRAVE StructuredBuffer</p></td>
<td align="left">Load</td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Accès du pipeline aux ressources de diffusion en continu](pipeline-access-to-streaming-resources.md)

 

 