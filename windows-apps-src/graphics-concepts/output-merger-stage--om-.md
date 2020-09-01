---
title: Étape Output Merger (OM)
description: L’étape de fusion de sortie (OM) combine différents types de données de sortie (valeurs de nuanceur de pixels, détails et informations de stencil) au contenu de la cible de rendu et des mémoires tampons de profondeur/stencil pour générer le résultat final du pipeline.
ms.assetid: ED2DC4A0-2B92-47AF-884A-BFC2183C78B8
keywords:
- Étape Output Merger (OM)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6478eaa2dadac74c22e5959623f03547f18babfc
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156313"
---
# <a name="output-merger-om-stage"></a>Étape Output Merger (OM)


L’étape de fusion de sortie (OM) combine différents types de données de sortie (valeurs de nuanceur de pixels, détails et informations de stencil) au contenu de la cible de rendu et des mémoires tampons de profondeur/stencil pour générer le résultat final du pipeline.

## <a name="span-idpurpose-and-usesspanspan-idpurpose-and-usesspanspan-idpurpose-and-usesspanpurpose-and-uses"></a><span id="Purpose-and-uses"></span><span id="purpose-and-uses"></span><span id="PURPOSE-AND-USES"></span>Usage et utilisations


L’étape de fusion de sortie (OM) est la dernière étape permettant de déterminer les pixels visibles (avec test des stencils de profondeur) et de fusionner les couleurs finales des pixels.

L’étape OM génère la couleur de pixel rendue finale à l’aide d’une combinaison des éléments suivants :

-   État du pipeline
-   Données de pixels générées par les nuanceurs de pixels
-   Contenu des cibles de rendu
-   Contenu des mémoires tampons de profondeur/stencil.

### <a name="span-idblending-overviewspanspan-idblending-overviewspanspan-idblending-overviewspanblending-overview"></a><span id="Blending-overview"></span><span id="blending-overview"></span><span id="BLENDING-OVERVIEW"></span>Vue d’ensemble de la fusion

La fusion combine une ou plusieurs valeurs de pixel pour créer une couleur de pixel finale. Le diagramme suivant illustre le processus impliqué dans la fusion de données de pixels.

![Diagramme illustrant le fonctionnement de la fusion de données](images/d3d10-blend-state.png)

Conceptuellement, vous pouvez visualiser ce graphique de Flow implémenté deux fois dans l’étape de fusion de sortie : la première fusionne les données RVB, tandis qu’en parallèle, une seconde fusionne les données alpha. Pour savoir comment utiliser l’API pour créer et définir l’état de fusion, consultez Configuration de la [fonctionnalité de fusion](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-blend-state).

La fonction Fixed-Blend peut être activée indépendamment pour chaque cible de rendu. Toutefois, il n’existe qu’un seul jeu de contrôles Blend, de sorte que le même lissage est appliqué à tous les RenderTargets avec la fusion activée. Les valeurs de fusion (y compris BlendFactor) sont toujours ancrées à la plage du format de cible de rendu avant la fusion. Le verrouillage est effectué par cible de rendu, en respectant le type de cible de rendu. La seule exception concerne les formats float16, float11 ou float10 qui ne sont pas ancrées afin que les opérations de fusion sur ces formats puissent être effectuées avec une précision ou une plage au moins égale à celle du format de sortie. Les zéros de NaN et signés sont propagés dans tous les cas (y compris les poids de fusion 0,0).

Lorsque vous utilisez des cibles de rendu sRVB, le Runtime convertit la couleur de la cible de rendu en espace linéaire avant d’effectuer la fusion. Le Runtime convertit la dernière valeur fusionnée en espace sRVB avant de l’enregistrer à nouveau dans la cible de rendu.

### <a name="span-iddual-source-color-blendingspanspan-iddual-source-color-blendingspanspan-iddual-source-color-blendingspandual-source-color-blending"></a><span id="Dual-source-color-blending"></span><span id="dual-source-color-blending"></span><span id="DUAL-SOURCE-COLOR-BLENDING"></span>Fusion de couleurs à double source

Cette fonctionnalité permet à l’étape de fusion de sortie d’utiliser simultanément des sorties de nuanceur de pixels (o0 et O1) comme entrées d’une opération de fusion avec la cible de rendu unique à l’emplacement 0. Les opérations de fusion valides sont les suivantes : Add, Subtract et revsubtract. L’équation de fusion et le masque d’écriture de sortie spécifient les composants que le nuanceur de pixels est en train de sortir. Les composants supplémentaires sont ignorés.

L’écriture dans d’autres sorties de nuanceur de pixels (O2, O3, etc.) n’est pas définie. vous ne pouvez pas écrire dans une cible de rendu si elle n’est pas liée à l’emplacement 0. L’écriture de oDepth est valide pendant la fusion de couleurs à double source.

### <a name="span-iddepth-stencil-testspanspan-iddepth-stencil-testspanspan-iddepth-stencil-testspandepth-stencil-testing-overview"></a><span id="Depth-Stencil-Test"></span><span id="depth-stencil-test"></span><span id="DEPTH-STENCIL-TEST"></span>Présentation-vue d’ensemble des tests stencil

Une mémoire tampon de stencil de profondeur, créée en tant que ressource de texture, peut contenir à la fois des données de profondeur et des données de stencil. Les données de profondeur sont utilisées pour déterminer les pixels les plus proches de l’appareil photo, et les données du stencil sont utilisées pour masquer les pixels qui peuvent être mis à jour. Au final, les données de la profondeur et de la valeur du stencil sont utilisées par l’étape de fusion de sortie pour déterminer si un pixel doit être dessiné ou non. Le diagramme suivant illustre le fonctionnement conceptuel du test des stencils de profondeur.

![Diagramme illustrant le fonctionnement du test des stencils de profondeur](images/d3d10-depth-stencil-test.png)

Pour configurer le test des stencils de profondeur, consultez [configuration de la fonctionnalité de stencil de profondeur](configuring-depth-stencil-functionality.md). Un objet de stencil de profondeur encapsule l’état de gabarit de profondeur. Une application peut spécifier l’état du stencil de profondeur, ou l’étape OM utilisera les valeurs par défaut. Les opérations de fusion sont effectuées par pixel si l’échantillonnage multiple est désactivé. Si l’échantillonnage multiple est activé, la fusion se produit sur une base par échantillonnage multiple.

Le processus d’utilisation de la mémoire tampon de profondeur pour déterminer le pixel à dessiner est appelé mise en mémoire tampon de profondeur, également parfois appelé mise en mémoire tampon z.

Une fois les valeurs de profondeur atteintes à l’étape de fusion de sortie (qu’elles proviennent d’une interpolation ou d’un nuanceur de pixels), elles sont toujours ancrées : z = min (Viewport. MaxDepth, Max (Viewport. MinDepth, z)) en fonction du format/précision de la mémoire tampon de profondeur, à l’aide de règles à virgule flottante. Après le verrouillage, la valeur de profondeur est comparée (à l’aide de DepthFunc) à la valeur de mémoire tampon de profondeur existante. Si aucun tampon de profondeur n’est lié, le test de profondeur passe toujours.

S’il n’existe aucun composant de stencil dans le format de mémoire tampon de profondeur, ou si aucune limite de mémoire tampon de profondeur n’est liée, le test de stencil passe toujours.

Une seule mémoire tampon de gabarit ou de profondeur peut être active à la fois ; toute vue de ressource liée doit correspondre (taille et dimensions identiques) à la vue de la profondeur/du gabarit. Cela ne signifie pas que la taille de la ressource doit être la même que la taille de la vue doit correspondre.

### <a name="span-idsample-maskspanspan-idsample-maskspanspan-idsample-maskspansample-mask-overview"></a><span id="Sample-Mask"></span><span id="sample-mask"></span><span id="SAMPLE-MASK"></span>Présentation de l’exemple de masque

Un exemple de masque est un masque de couverture à exemple de 32 bits qui détermine les exemples qui sont mis à jour dans les cibles de rendu actives. Un seul exemple de masque est autorisé. Le mappage des bits dans un exemple de masque aux exemples d’une ressource est défini par un utilisateur. Pour le rendu n-Sample, les n premiers bits (à partir du LSB) de l’échantillon de masque sont utilisés (32 bits le nombre maximal de bits).

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entrée


L’étape de fusion de sortie (OM) génère la couleur de pixel rendue finale à l’aide d’une combinaison des éléments suivants :

-   État du pipeline
-   Données de pixels générées par les nuanceurs de pixels
-   Contenu des cibles de rendu
-   Contenu des mémoires tampons de profondeur/stencil.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Sortie


### <a name="span-idoutput-write-mask-overviewspanspan-idoutput-write-mask-overviewspanspan-idoutput-write-mask-overviewspanoutput-write-mask-overview"></a><span id="Output-write-mask-overview"></span><span id="output-write-mask-overview"></span><span id="OUTPUT-WRITE-MASK-OVERVIEW"></span>Vue d’ensemble du masque d’écriture de sortie

Utilisez un masque d’écriture de sortie pour contrôler (par composant) les données qui peuvent être écrites dans une cible de rendu.

### <a name="span-idmultiple-render-targets-overviewspanspan-idmultiple-render-targets-overviewspanspan-idmultiple-render-targets-overviewspanmultiple-render-targets-overview"></a><span id="Multiple-render-targets-overview"></span><span id="multiple-render-targets-overview"></span><span id="MULTIPLE-RENDER-TARGETS-OVERVIEW"></span>Vue d’ensemble des cibles de rendu multiples

Un nuanceur de pixels peut être utilisé pour effectuer le rendu sur au moins 8 cibles de rendu distinctes, qui doivent toutes être du même type (buffer, Texture1D, Texture1DArray, etc.). En outre, toutes les cibles de rendu doivent avoir la même taille dans toutes les dimensions (largeur, hauteur, profondeur, taille de tableau, nombre d’échantillons). Chaque cible de rendu peut avoir un format de données différent.

Vous pouvez utiliser n’importe quelle combinaison d’emplacements de cibles de rendu (jusqu’à 8). Toutefois, un affichage des ressources ne peut pas être lié à plusieurs emplacements de rendu-cible simultanément. Une vue peut être réutilisée tant que les ressources ne sont pas utilisées simultanément.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>Dans cette section


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Rubrique</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="configuring-depth-stencil-functionality.md">Configuration de la fonctionnalité de profondeur-gabarit</a></p></td>
<td align="left"><p>Cette section décrit les étapes de configuration de la mémoire tampon du stencil de profondeur et l’état du gabarit de profondeur pour l’étape de fusion de sortie.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Pipeline graphique](graphics-pipeline.md)

 

 