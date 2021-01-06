---
title: Pipeline graphique
description: Le pipeline graphique Direct3D est conçu pour la génération de graphismes destinés aux applications de jeu en temps réel. Les données circulent de l’entrée vers la sortie en transitant par chacune des phases configurables ou programmables de ce pipeline.
ms.assetid: C9519AD0-5425-48BD-9FF4-AED8959CA4AD
keywords:
- Pipeline graphique
- Étapes de canalisation
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2032a57b598f08b24c1d52cecfa4f92b90591ec0
ms.sourcegitcommit: 48702934676ae366fd46b7d952396c5e2fb2cbbe
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/06/2021
ms.locfileid: "97927792"
---
# <a name="graphics-pipeline"></a>Pipeline graphique

Le pipeline graphique Direct3D est conçu pour la génération de graphismes destinés aux applications de jeu en temps réel. Les données circulent de l’entrée vers la sortie en transitant par chacune des phases configurables ou programmables de ce pipeline.

Toutes les étapes peuvent être configurées à l’aide de l’API Direct3D. Les étapes qui composent les noyaux de nuanceur courants (blocs rectangulaires arrondis) sont programmables à l’aide du langage de programmation [HLSL](/windows/desktop/direct3dhlsl/dx-graphics-hlsl) . Cela rend le pipeline extrêmement flexible et adaptable.

Les plus couramment utilisés sont l’étape du nuanceur de sommets (VS) et l’étape de nuanceur de pixels (PS). Si vous ne fournissez pas ces étapes de nuanceur, un vertex de non-opération, un vertex direct et un nuanceur de pixels par défaut sont utilisés.

![diagramme du flot de données dans le pipeline programmable Direct3D 11](images/d3d11-pipeline-stages.jpg)

## <a name="input-assembler-stage"></a>Étape assembleur d’entrée

L' [assembleur d’entrée](input-assembler-stage--ia-.md) fournit des données de primitives et d’adjacence au pipeline, telles que des triangles, des lignes et des points, y compris des ID de sémantique pour faciliter l’efficacité des nuanceurs en réduisant le traitement des primitives qui n’ont pas encore été traitées.

- Entrée

   Données primitives (triangles, lignes et/ou points), à partir de mémoires tampons remplies par l’utilisateur en mémoire. Et éventuellement des données d’adjacence. Un triangle est 3 sommets pour chaque triangle et éventuellement 3 sommets pour les données d’contiguïté par triangle.

- Sortie

   Primitives avec des valeurs générées par le système attachées (par exemple, un ID primitif, un ID d’instance ou un ID de vertex).

## <a name="vertex-shader-stage"></a>Étape nuanceur de sommets

L' [étape vertex shader (vs)](vertex-shader-stage--vs-.md) traite les vertex, en effectuant généralement des opérations telles que les transformations, les pelures et l’éclairage. Un nuanceur de sommets prend un vertex d’entrée unique et produit un seul vertex de sortie. Opérations individuelles par vertex, telles que les transformations, les pelures, la transformation et l’éclairage par vertex.

- Entrée

   Un vertex unique, avec des valeurs générées par le système VertexID et InstanceID. Chaque vertex d’entrée de nuanceur de sommets peut être constitué de vecteurs jusqu’à 16 32 bits (jusqu’à 4 composants chacun).

- Sortie

   Un seul vertex. Chaque vertex de sortie peut être constitué de plusieurs vecteurs à 4 16 32 bits.

## <a name="hull-shader-stage"></a>Étape nuanceur de coque

L' [étape de nuanceur de coque (HS)](hull-shader-stage--hs-.md) est l’une des étapes de pavage, qui découpent efficacement une surface unique d’un modèle en plusieurs triangles. Un nuanceur de coque est appelé une fois par correctif et il transforme des points de contrôle d’entrée qui définissent une surface de poids faible en points de contrôle qui composent un correctif. Il effectue également des calculs par correctifs pour fournir des données pour l’étape du paveur (TS) et l’étape de nuanceur de domaine (DS).

- Entrée

   Entre 1 et 32 points de contrôle d’entrée, qui définissent ensemble une surface de poids faible.

- Sortie

   Entre 1 et 32 points de contrôle de sortie, qui forment un correctif. Le nuanceur de coque déclare l’état de l’étape du paveur (TS), y compris le nombre de points de contrôle, le type de face de correctif et le type de partitionnement à utiliser lors de la le pavage.

## <a name="tessellator-stage"></a>Étape du paveur

L' [étape du paveur (TS)](tessellator-stage--ts-.md) crée un modèle d’échantillonnage du domaine qui représente le correctif Geometry et génère un ensemble d’objets plus petits (triangles, points ou lignes) qui connectent ces exemples.

- Entrée

   Le du paveur fonctionne une fois par correctif à l’aide des facteurs de pavage (qui spécifient le degré de précision du domaine à fractionner) et le type de partitionnement (qui spécifie l’algorithme utilisé pour découper un correctif) qui sont transmis à partir de l’étape de nuanceur de coque.

- Sortie

   Du paveur génère des coordonnées UV (et éventuellement w) et la topologie de surface à l’étape de nuanceur de domaine.

## <a name="domain-shader-stage"></a>Étape du nuanceur de domaine

L' [étape du nuanceur de domaine (DS)](domain-shader-stage--ds-.md) calcule la position du vertex d’un point subdivisé dans le correctif de sortie ; il calcule la position du vertex qui correspond à chaque exemple de domaine. Un nuanceur de domaine est exécuté une fois par point de sortie d’étape du paveur et dispose d’un accès en lecture seule aux constantes de correctif de sortie de nuanceur de coque et de sortie, et les coordonnées UV de sortie de l’étape du paveur.

- Entrée

   Un nuanceur de domaine consomme des points de contrôle de sortie à partir de l' [étape de nuanceur de coque (HS)](hull-shader-stage--hs-.md). Les sorties du nuanceur de coque incluent : les points de contrôle, les données de constantes de correctifs et les facteurs de pavage (les facteurs de pavage peuvent inclure les valeurs utilisées par le du paveur de la fonction fixe, ainsi que les valeurs brutes, avant l’arrondissement par pavage d’entiers, par exemple). Un nuanceur de domaine est appelé une fois par coordonnée de sortie de l' [étape du paveur (TS)](tessellator-stage--ts-.md).

- Sortie

   L’étape du nuanceur de domaine (DS) affiche la position du vertex d’un point subdivisé dans le correctif de sortie.

## <a name="geometry-shader-stage"></a>Étape de nuanceur Geometry

L' [étape de nuanceur Geometry (GS)](geometry-shader-stage--gs-.md) traite les primitives entières : les triangles, les lignes et les points, ainsi que leurs vertex adjacents. Il prend en charge l’amplification et la désamplification Geometry. Elle est utile pour les algorithmes, y compris l’expansion point Sprite, les systèmes de particule dynamiques, la génération de fourrure et de fin, la génération de volume de cliché instantané, le rendu carte cubique à passage unique, l’échange de matériel Per-Primitive et la configuration de Per-Primitive matérielle, y compris la génération de coordonnées Barycentric en tant que données primitives afin qu’un nuanceur de pixels puisse effectuer une

- Entrée

   Contrairement aux nuanceurs de vertex, qui opèrent sur un seul vertex, les entrées du nuanceur Geometry sont les vertex pour une primitive complète (trois vertex pour triangles, deux sommets pour les lignes ou un vertex unique pour le point).

- Sortie

   L’étape de nuanceur Geometry (GS) est en charge de la génération de plusieurs vertex formant une seule topologie sélectionnée. Les topologies de sortie de nuanceur Geometry disponibles sont **tristrip**, **linestrip** et **PointList**. Le nombre de primitives émises peut varier librement au sein de n’importe quel appel du nuanceur Geometry, bien que le nombre maximal de vertex pouvant être émis doive être déclaré statiquement. Les longueurs de bande émises à partir d’un appel de nuanceur Geometry peuvent être arbitraires et de nouvelles bandes peuvent être créées via la fonction HLSL [RestartStrip](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-so-restartstrip) .

## <a name="stream-output-stage"></a>Étape de sortie de flux

La [sortie de flux (SO)](stream-output-stage--so-.md) envoie en continu des données de vertex (ou flux) à partir de l’étape active précédente vers une ou plusieurs mémoires tampons en mémoire. Les données transmises en mémoire vers la mémoire peuvent être redistribuées dans le pipeline en tant que données d’entrée, ou en lecture-retour à partir de l’UC.

- Entrée

   Données de vertex à partir d’une étape de pipeline précédente.

- Sortie

   La sortie de flux (SO) envoie en continu des données de vertex (ou flux) à partir de l’étape active précédente, telles que l’étape de nuanceur Geometry (GS), vers une ou plusieurs mémoires tampons en mémoire. Si l’étape de nuanceur Geometry (GS) est inactive et que l’étape sortie de flux (SO) est active, elle génère en continu des données de vertex de l’étape de nuanceur de domaine (DS) vers des mémoires tampons en mémoire (ou si le DS est également inactif, à partir de l’étape de nuanceur de sommets (VS)).

## <a name="rasterizer-stage"></a>Étape de rastérisation

L' [étape de rastérisation (RS)](rasterizer-stage--rs-.md) découpe les primitives qui ne sont pas en vue, prépare les primitives pour l’étape de nuanceur de pixels (PS) et détermine comment appeler les nuanceurs de pixels. Convertit les informations de vecteur (composées de formes ou de Primitives) en une image raster (composée de pixels) pour l’affichage de graphiques 3D en temps réel.

- Entrée

   Les vertex (x, y, z, w) arrivant à l’étape du rastériseur sont supposés être dans l’espace de clip homogène. Dans cet espace de coordonnées, les points de l’axe X sont à droite, Y pointe vers le haut et Z points loin de l’appareil photo.

- Sortie

   Pixels réels qui doivent être rendus. Comprend des attributs de vertex à utiliser dans l’interpolation par le nuanceur de pixels.

## <a name="pixel-shader-stage"></a>Étape nuanceur de pixels

L' [étape de nuanceur de pixels (PS)](pixel-shader-stage--ps-.md) reçoit des données interpolées pour une primitive et génère des données par pixel telles que la couleur.

- Entrée

   Lorsque le pipeline est configuré sans nuanceur Geometry, un nuanceur de pixels est limité à 16, 32 bits, 4 composants. Dans le cas contraire, un nuanceur de pixels peut prendre jusqu’à 32, 32 bits, 4 composants. Les données d’entrée de nuanceur de pixels incluent des attributs de vertex (qui peuvent être interpolés avec ou sans correction de perspective) ou peuvent être traités comme des constantes par Primitives. Les entrées de nuanceur de pixels sont interpolées à partir des attributs de vertex de la primitive en cours de pixellisation, en fonction du mode d’interpolation déclaré. Si une primitive est découpée avant pixellisation, le mode d’interpolation est également respecté au cours du processus de découpage.

- Sortie

   Un nuanceur de pixels peut sortir jusqu’à 8, 32 bits, 4 couleurs de composant ou aucune couleur si le pixel est ignoré. Les composants du registre de sortie du nuanceur de pixels doivent être déclarés avant de pouvoir être utilisés. un masque d’écriture de sortie distinct est autorisé pour chaque registre.

## <a name="output-merger-stage"></a>Étape de fusion de sortie

L' [étape de fusion de sortie (OM)](output-merger-stage--om-.md) combine différents types de données de sortie (valeurs de nuanceur de pixels, détails et informations de stencil) au contenu de la cible de rendu et des mémoires tampons de profondeur/stencil pour générer le résultat final du pipeline.

- Entrée

   Les entrées de fusion de sortie sont l’état du pipeline, les données de pixels générées par les nuanceurs de pixels, le contenu des cibles de rendu et le contenu des mémoires tampons de profondeur/stencil.

- Sortie

   Couleur du pixel rendu final.

## <a name="related-topics"></a>Rubriques connexes

- [Guide d’apprentissage graphique Direct3D](index.md)

- [Pipeline de calcul](compute-pipeline.md)