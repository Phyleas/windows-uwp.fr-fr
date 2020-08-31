---
title: Étape Tessellator (TS)
description: L’étape du paveur (TS) crée un modèle d’échantillonnage du domaine qui représente le correctif Geometry et génère un ensemble d’objets plus petits (triangles, points ou lignes) qui connectent ces exemples.
ms.assetid: 2F006F3D-5A04-4B3F-8BC7-55567EFCFA6C
keywords:
- Étape Tessellator (TS)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 90dfb8d28be786cb542e72fde5a24bed4de68f78
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167973"
---
# <a name="tessellator-ts-stage"></a>Étape Tessellator (TS)


L’étape du paveur (TS) crée un modèle d’échantillonnage du domaine qui représente le correctif Geometry et génère un ensemble d’objets plus petits (triangles, points ou lignes) qui connectent ces exemples.

## <a name="span-idpurpose_and_usesspanspan-idpurpose_and_usesspanspan-idpurpose_and_usesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Usage et utilisations


Le diagramme suivant met en évidence les étapes du pipeline de graphiques Direct3D.

![diagramme du pipeline Direct3D 11 qui met en évidence les étapes du nuanceur de coque, du du paveur et du nuanceur de domaine](images/d3d11-pipeline-stages-tessellation.png)

Le diagramme suivant illustre la progression à travers les étapes de pavage.

![diagramme de la progression de la polygonalisation](images/tess-prog.png)

La progression commence par la surface de sous-division de faible détail. La progression met ensuite en évidence le correctif d’entrée avec le correctif de géométrie, les exemples de domaine et les triangles correspondants qui connectent ces exemples. La progression met enfin en évidence les vertex qui correspondent à ces exemples.

Le runtime Direct3D prend en charge trois étapes qui implémentent le pavage, qui convertit les surfaces de sous-division de faible détail en primitives de détail supérieures sur le GPU. Polygonalisation des vignettes (ou fractionnement) des surfaces de poids fort dans des structures appropriées pour le rendu.

Les étapes de pavage fonctionnent ensemble pour convertir des surfaces d’ordre supérieur (ce qui permet de rendre le modèle simple et efficace) à de nombreux triangles pour un rendu détaillé dans le pipeline de graphiques Direct3D.

Pavage utilise le GPU pour calculer une surface plus détaillée à partir d’une surface construite à partir de quatre carreaux, de carreaux de triangle ou d’isolignes. Pour rapprocher la surface de poids fort, chaque correctif est subdivisé en triangles, points ou lignes à l’aide de facteurs de pavage. Le pipeline de graphiques Direct3D implémente le pavage à l’aide de trois étapes de pipeline :

-   [Phase de nuanceur de coque (HS)](hull-shader-stage--hs-.md) : étape de nuanceur programmable qui produit un correctif de géométrie (et des constantes de correction) qui correspondent à chaque correctif d’entrée (quadruple, triangle ou ligne).
-   Étape du paveur (TS) : étape de pipeline à fonction fixe qui crée un modèle d’échantillonnage du domaine qui représente le correctif de géométrie et génère un ensemble d’objets plus petits (triangles, points ou lignes) qui connectent ces exemples.
-   [Étape de nuanceur de domaine (DS)](domain-shader-stage--ds-.md) : étape de nuanceur programmable qui calcule la position du vertex qui correspond à chaque exemple de domaine.

En implémentant la polygonalisation dans le matériel, un pipeline graphique peut évaluer des modèles de détail inférieurs (nombre inférieur de polygones) et un rendu plus détaillé. Bien que le pavage de logiciels puisse être effectué, le pavage implémenté par le matériel peut générer une quantité incroyable de détails visuels (y compris la prise en charge du mappage de déplacement) sans ajouter le détail visuel aux tailles de modèle et aux taux d’actualisation ensuivre.

Avantages de la polygonalisation :

-   Le pavage économise beaucoup de mémoire et de bande passante, ce qui permet à une application d’afficher des surfaces plus détaillées à partir de modèles de faible résolution. La technique de pavage implémentée dans le pipeline de graphiques Direct3D prend également en charge le mappage de déplacement, qui peut produire des quantités impressionnantes de détails de surface.
-   Le pavage prend en charge les techniques de rendu évolutives, telles que les niveaux de détail continus ou d’affichage, qui peuvent être calculés à la volée.
-   La facettisation améliore les performances en effectuant des calculs coûteux à une fréquence inférieure (en effectuant des calculs sur un modèle moins détaillé). Cela peut inclure la fusion de calculs à l’aide de formes Blend ou de cibles morphes pour des calculs réalistes ou physiques pour la détection de collision ou la dynamique de corps souple.

Le pipeline de graphiques Direct3D implémente le pavage dans le matériel, ce qui permet de décharger le travail du processeur vers le GPU. Cela peut entraîner d’importantes améliorations des performances si une application implémente un grand nombre de cibles morphes et/ou des modèles de déformation/déformation plus sophistiqués.

Le du paveur est une étape de fonction fixe initialisée par la liaison d’un [nuanceur de coque](hull-shader-stage--hs-.md) au pipeline. (voir [How to : Initialize the du paveur stage](/windows/desktop/direct3d11/direct3d-11-advanced-stages-tessellator-initialize)). L’objectif de l’étape du paveur consiste à subdiviser un domaine (quadruple, tri ou ligne) en plusieurs objets plus petits (triangles, points ou lignes). Le du paveur mosaïque un domaine canonique dans un système de coordonnées normalisé (zéro à un). Par exemple, un domaine à quatre cœurs est fractionné sur un carré d’unité.

### <a name="span-idphases_in_the_tessellator__ts__stagespanspan-idphases_in_the_tessellator__ts__stagespanspan-idphases_in_the_tessellator__ts__stagespanphases-in-the-tessellator-ts-stage"></a><span id="Phases_in_the_Tessellator__TS__stage"></span><span id="phases_in_the_tessellator__ts__stage"></span><span id="PHASES_IN_THE_TESSELLATOR__TS__STAGE"></span>Phases de l’étape du paveur (TS)

L’étape du paveur (TS) fonctionne en deux phases :

-   La première phase traite les facteurs de pavage, en corrigeant les problèmes d’arrondi, en gérant de très petits facteurs, en réduisant et en combinant des facteurs, à l’aide d’opérations arithmétiques à virgule flottante 32 bits.
-   La deuxième phase génère des listes de points ou de topologies en fonction du type de partitionnement sélectionné. Il s’agit de la tâche principale de l’étape du paveur et utilise des fractions de 16 bits avec l’arithmétique à virgule fixe. L’arithmétique à virgule fixe autorise l’accélération matérielle tout en conservant une précision acceptable. Par exemple, étant donné un correctif à l’échelle du compteur 64, cette précision peut placer des points à une résolution de 2 mm.

    | Type de partitionnement | Plage                       |
    |----------------------|-----------------------------|
    | Fractionnaire \_ impair      | \[1... 63\]                  |
    | Fractionnaire \_ pair     | TessFactor plage : \[ 2.. 64\] |
    | Integer              | TessFactor plage : \[ 1.. 64\] |
    | Pow2                 | TessFactor plage : \[ 1.. 64\] |

     

La facettisation est implémentée avec deux étapes de nuanceur programmables : un [nuanceur de coque](hull-shader-stage--hs-.md) et un [nuanceur de domaine](domain-shader-stage--ds-.md). Ces étapes de nuanceur sont programmées avec du code HLSL qui est défini dans le nuancier Model 5. Les cibles du nuanceur sont : HS \_ 5 \_ 0 et DS \_ 5 \_ 0. Le titre crée le nuanceur, puis le code du matériel est extrait des nuanceurs compilés passés dans le runtime lorsque les nuanceurs sont liés au pipeline.

### <a name="span-idenabling_disabling_tessellationspanspan-idenabling_disabling_tessellationspanspan-idenabling_disabling_tessellationspanenablingdisabling-tessellation"></a><span id="Enabling_disabling_tessellation"></span><span id="enabling_disabling_tessellation"></span><span id="ENABLING_DISABLING_TESSELLATION"></span>Activation/désactivation du pavage

Activez le pavage en créant un nuanceur de coque et en le liant à l’étape de nuanceur de coque (cela configure automatiquement l’étape du paveur). Pour générer les positions finales des vertex à partir des correctifs fractionnés, vous devez également créer un [nuanceur de domaine](domain-shader-stage--ds-.md) et le lier à l’étape de nuanceur de domaine. Une fois le pavage activé, l’entrée de données de l’étape de l’assembleur d’entrée doit être une donnée de correctif. La topologie de l’assembleur d’entrée doit être une topologie à constantes de correctifs.

Pour désactiver le pavage, affectez la valeur **null**au nuanceur de coque et au nuanceur de domaine. Ni l’étape de [nuanceur Geometry (GS),](geometry-shader-stage--gs-.md) ni la [sortie de flux de données (SO)](stream-output-stage--so-.md) ne peuvent lire les points de contrôle de sortie de nuanceur-nuanceur ou les données de correctifs.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entrée


Le du paveur fonctionne une fois par correctif à l’aide des facteurs de pavage (qui spécifient le degré de précision du domaine à fractionner) et le type de partitionnement (qui spécifie l’algorithme utilisé pour découper un correctif) qui sont transmis à partir de l’étape de nuanceur de coque.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Sortie


Du paveur génère des coordonnées UV (et éventuellement w) et la topologie de surface à l’étape de nuanceur de domaine.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Pipeline graphique](graphics-pipeline.md)

 

 