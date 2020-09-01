---
title: Étape Hull Shader (HS)
description: L’étape de nuanceur de coque (HS) est l’une des étapes de pavage, qui découpent efficacement une surface unique d’un modèle en plusieurs triangles.
ms.assetid: C62F6F15-CAD7-4C72-9735-00762E346C4C
keywords:
- Étape Hull Shader (HS)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 11c9f0a52c4a320306cf5dfd5ce90fd5bbe2c407
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165013"
---
# <a name="hull-shader-hs-stage"></a>Étape Hull Shader (HS)

L’étape de nuanceur de coque (HS) est l’une des étapes de pavage, qui découpent efficacement une surface unique d’un modèle en plusieurs triangles. L’étape de nuanceur de coque (HS) produit un correctif de géométrie (et des constantes de correction) qui correspondent à chaque correctif d’entrée (quadruple, triangle ou ligne). Un nuanceur de coque est appelé une fois par correctif et il transforme des points de contrôle d’entrée qui définissent une surface de poids faible en points de contrôle qui composent un correctif. Il effectue également des calculs par correctifs pour fournir des données pour l' [étape du paveur (TS)](tessellator-stage--ts-.md) et l' [étape de nuanceur de domaine (DS)](domain-shader-stage--ds-.md).

## <a name="span-idpurpose_and_usesspanspan-idpurpose_and_usesspanspan-idpurpose_and_usesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Usage et utilisations


![diagramme de l’étape de nuanceur de coque](images/d3d11-hull-shader.png)

Les trois étapes de pavage fonctionnent ensemble pour convertir des surfaces d’ordre supérieur (ce qui permet de rendre le modèle simple et efficace) à de nombreux triangles pour un rendu détaillé dans le pipeline graphique. Les étapes de pavage incluent l’étape du nuanceur de coque (HS), l' [étape du paveur (TS)](tessellator-stage--ts-.md)et l' [étape de nuanceur de domaine (DS)](domain-shader-stage--ds-.md).

L’étape de nuanceur de coque (HS) est une étape de nuanceur programmable. Un nuanceur de coque est implémenté avec une fonction HLSL.

Un nuanceur de coque fonctionne en deux phases : une phase de point de contrôle et une phase de constante de patch, qui sont exécutées en parallèle par le matériel. Le compilateur HLSL extrait le parallélisme dans un nuanceur de coque et l’encode en bytecode qui pilote le matériel.

-   La phase du point de contrôle s’exécute une fois pour chaque point de contrôle, la lecture des points de contrôle pour un correctif et la génération d’un point de contrôle de sortie (identifié par un **ControlPointID**).
-   La phase patch-constant s’exécute une fois par correctif pour générer des facteurs de pavage Edge et d’autres constantes par correctif. En interne, de nombreuses phases à constantes de correctifs peuvent s’exécuter en même temps. La phase patch-constant dispose d’un accès en lecture seule à tous les points de contrôle d’entrée et de sortie.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entrée


Entre 1 et 32 points de contrôle d’entrée, qui définissent ensemble une surface de poids faible.

-   Le nuanceur de coque déclare l’État requis par l' [étape du paveur (TS)](tessellator-stage--ts-.md). Cela comprend des informations telles que le nombre de points de contrôle, le type de face de patch et le type de partitionnement à utiliser lors de la le pavage. Ces informations apparaissent en tant que déclarations en général au début du code du nuanceur.
-   Les facteurs de pavage déterminent le degré de subdivision de chaque correctif.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Sortie


Entre 1 et 32 points de contrôle de sortie, qui forment un correctif.

-   La sortie du nuanceur est comprise entre 1 et 32 points de contrôle, quel que soit le nombre de facteurs de pavage. La sortie des points de contrôle d’un nuanceur de coque peut être consommée par l’étape de nuanceur de domaine. Les données de constante de correctif peuvent être consommées par un nuanceur de domaine. Les facteurs de pavage peuvent être consommés par l' [étape du paveur (TS)](tessellator-stage--ts-.md) et l' [étape du nuanceur de domaine (DS)](domain-shader-stage--ds-.md).
-   Si le nuanceur de coque définit un facteur de pavage de bord sur ≤ 0 ou NaN, le correctif est éliminé (omis). Par conséquent, l’étape du paveur peut ou non s’exécuter, le nuanceur de domaine ne s’exécutera pas et aucune sortie visible ne sera générée pour ce correctif.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Tels


```hlsl
[patchsize(12)]
[patchconstantfunc(MyPatchConstantFunc)]
MyOutPoint main(uint Id : SV_ControlPointID,
     InputPatch<MyInPoint, 12> InPts)
{
     MyOutPoint result;
     
     ...
     
     result = TransformControlPoint( InPts[Id] );

     return result;
}
```

Consultez [Comment : créer un nuanceur de coque](/windows/desktop/direct3d11/direct3d-11-advanced-stages-hull-shader-create).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Pipeline graphique](graphics-pipeline.md)

 

 