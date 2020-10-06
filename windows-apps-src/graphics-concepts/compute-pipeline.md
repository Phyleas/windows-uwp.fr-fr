---
title: Pipeline de calcul
description: Le pipeline de calcul Direct3D est conçu pour traiter les calculs pouvant être effectués principalement en parallèle du pipeline graphique.
ms.assetid: 355B66C6-C0DF-47BA-A9C9-7AFA50B5B614
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 36d1bd524d6e71b0a1aa9477d7a2b7a5f27544aa
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750045"
---
# <a name="compute-pipeline"></a>Pipeline de calcul


\[Certaines informations relatives aux produits précommercialisés peuvent être substantiellement modifiées avant leur commercialisation. Microsoft exclut toute garantie, expresse ou implicite, concernant les informations fournies ici.\]


Le pipeline de calcul Direct3D est conçu pour traiter les calculs pouvant être effectués principalement en parallèle du pipeline graphique. Il n’y a que quelques étapes dans le pipeline de calcul, les données passant de l’entrée à la sortie via l’étape de nuanceur de calcul programmable.

## <a name="purpose"></a>Objectif

Comme les autres nuanceurs programmables, l' [étape de nuanceur de calcul (CS)](compute-shader-stage--cs-.md) est conçue et implémentée en HLSL. Un nuanceur de calcul fournit un calcul universel à grande vitesse et tire parti du grand nombre de processeurs parallèles sur l’unité de traitement graphique (GPU). Le nuanceur de calcul fournit des fonctionnalités de partage de mémoire et de synchronisation de threads pour permettre des méthodes de programmation parallèles plus efficaces. |

## <a name="input"></a>Entrée

Contrairement à d’autres nuanceurs programmables, la définition de l’entrée est abstraite. L’entrée peut être de type un, deux ou trois dimensions, déterminant le nombre d’appels du nuanceur de calcul à exécuter. Il est possible de définir des données partagées pour un ensemble d’appels à lire. |

## <a name="output"></a>Output

Les données de sortie du nuanceur de calcul, qui peuvent être très variées, peuvent être synchronisées avec le pipeline de rendu graphique lorsque les données calculées sont requises.

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">Like other programmable shaders, <a href="#compute-shader-stage--cs-.md">Compute Shader (CS) stage</a> is designed and implemented with HLSL. A compute shader provides high-speed general purpose computing and takes advantage of the large numbers of parallel processors on the graphics processing unit (GPU). The compute shader provides memory sharing and thread synchronization features to allow more effective parallel programming methods.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Unlike other programmable shaders, the definition of input is abstract. The input can be one, two or three-dimensional in nature, determining the number of invocations of the compute shader to execute. It is possible to define shared data for one set of invocations to read.</td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">Output data from the compute shader, which can be highly varied, can be synchronized with the graphics rendering pipeline when the computed data is required.</td>
</tr>
</tbody>
</table>
-->

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Guide d’apprentissage graphique Direct3D](index.md)

 

 
