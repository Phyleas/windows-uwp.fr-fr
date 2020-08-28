---
title: Échantillonnage des points les plus proches
description: En savoir plus sur l’utilisation de l’échantillonnage par point le plus proche dans Direct3D comme alternative au filtrage de texture pour le traitement des textures dans une application.
ms.assetid: D7F88320-2C61-47E9-9B92-EC31D48DB079
keywords:
- Échantillonnage des points les plus proches
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 090c148e05e664ffe0b027fe9af7eb69c11b22a9
ms.sourcegitcommit: cb5af00af05e838621c270173e7fde1c5d2168ef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89043491"
---
# <a name="span-iddirect3dconceptsnearest-point_samplingspannearest-point-sampling"></a><span id="direct3dconcepts.nearest-point_sampling"></span>Échantillonnage des points les plus proches


Les applications ne sont pas tenues d’utiliser le filtrage de texture. Direct3D peut être défini de manière à ce qu’il calcule l’adresse Texel, qui ne correspond souvent pas à des entiers, et copie la couleur de l’Texel avec l’adresse entière la plus proche. Ce processus est appelé *échantillonnage à point le plus proche*. L’échantillonnage à point le plus proche peut être tel que si la taille de la texture est similaire à la taille de l’image de la primitive sur l’écran. Si ce n’est pas le cas, la texture doit être agrandie ou minimisés. Le résultat de la non-correspondance des tailles de texture avec la taille de l’image primitive peut être une image composée, avec un alias ou flou.

Utilisez l’échantillonnage à point le plus proche avec précaution, car il peut parfois provoquer des artefacts graphiques lorsque la texture est échantillonnée à la limite entre deux texels. Cette limite est la position le long de la texture (u ou v) à partir de laquelle le Texel échantillonné passe d’un Texel au suivant. Lorsque l’échantillonnage de point est utilisé, le système choisit un exemple de Texel ou l’autre, et le résultat peut changer brusquement d’une Texel à la Texel suivante lorsque la limite est franchie. Cet effet peut apparaître sous la forme d’artefacts graphiques indésirables dans la texture affichée. Lorsque le filtrage linéaire est utilisé, le Texel qui en résulte est calculé à partir des texels contigus et est lissé en douceur entre eux lorsque l’index de texture se déplace dans la limite.

Cet effet peut être observé lors du mappage d’une très petite texture sur un polygone de très grande taille : une opération souvent appelée grossissement. Par exemple, lorsque vous utilisez une texture qui ressemble à un damier, l’échantillonnage à point le plus proche se traduit par un damier plus large qui affiche des bords distincts. En revanche, le filtrage de texture linéaire génère une image où les couleurs des damiers varient harmonieusement sur le polygone.

Dans la plupart des cas, les applications reçoivent les meilleurs résultats en évitant les échantillons les plus proches chaque fois que cela est possible. La majorité du matériel aujourd’hui étant optimisé pour le filtrage linéaire, votre application ne doit pas avoir de dégradation des performances. Si l’effet souhaité requiert l’utilisation de l’échantillonnage à point le plus proche (par exemple, lors de l’utilisation de textures pour afficher des caractères de texte lisibles), votre application doit être extrêmement prudente pour éviter l’échantillonnage aux limites de Texel, ce qui peut entraîner des effets indésirables. L’illustration suivante montre à quoi ces artefacts peuvent ressembler.

![illustration d’une zone à six sections avec lignes horizontales non continues dans les deux carrés en haut à droite](images/ptrtfct.png)

Les deux carrés situés en haut à droite du groupe apparaissent différemment de leurs voisins, avec des décalages en diagonale. Pour éviter les artefacts graphiques comme ceux-ci, vous devez vous familiariser avec les règles d’échantillonnage de texture Direct3D pour le filtrage à point le plus proche. Direct3D mappe une coordonnée de texture à virgule flottante allant de \[ 0,0 1,0 \] (0,0 à 1,0, inclusivement) à une valeur d’espace de Texel d’entier comprise entre \[ -0,5, n-0,5 \] , où n est le nombre de texels dans une dimension donnée sur la texture. L’index de texture résultant est arrondi à l’entier le plus proche. Ce mappage peut introduire des imprécisions d’échantillonnage au niveau des limites de Texel.

Pour obtenir un exemple simple, Imaginez une application qui restitue des polygones avec le mode d’adressage de texture de retour à la ligne. À l’aide du mappage utilisé par Direct3D, l’index de texture u est mappé comme indiqué dans le diagramme suivant pour une texture avec une largeur de 4 texels.

![diagramme des coordonnées de texture 0,0 et 1,0 à la limite entre les texels](images/ptsmpprb.png)

Les coordonnées de texture, 0,0 et 1,0 pour cette illustration, sont exactement à la limite entre les texels. À l’aide de la méthode par laquelle Direct3D mappe les valeurs, les coordonnées de texture sont comprises entre \[ -0,5, 4-0,5 \] , où 4 représente la largeur de la texture. Dans ce cas, le Texel échantillonné est le Texel 0 pour un index de texture de 1,0. Toutefois, si la coordonnée de texture était légèrement inférieure à 1,0, le Texel échantillonné est le Texel n plutôt que le Texel 0.

En effet, l’agrandissement d’une petite texture à l’aide des coordonnées de texture de 0,0 et 1,0 exactement avec le filtrage à point le plus proche sur un triangle aligné sur l’espace à l’écran se traduit par des pixels pour lesquels la carte de texture est échantillonnée à la limite entre les texels. Toutes les inexactitudes dans le calcul des coordonnées de texture, mais peu volumineuses, entraînent des artefacts le long des zones de l’image rendue qui correspondent aux bords de Texel de la carte de texture.

L’exécution de ce mappage de coordonnées de texture à virgule flottante à des texels d’entiers avec une précision parfaite est difficile, fastidieuse en termes de temps de calcul et n’est généralement pas nécessaire. La plupart des implémentations matérielles utilisent une approche itérative pour le calcul des coordonnées de texture à chaque emplacement de pixel au sein d’un triangle. Les approches itératives ont tendance à masquer ces imprécisions, car les erreurs sont accumulées uniformément pendant l’itération.

Le rastériseur de référence Direct3D utilise une approche d’évaluation directe pour calculer les index de texture à chaque emplacement de pixel. L’évaluation directe diffère de l’approche itérative dans le cas où toute inexactitude dans l’opération présente une distribution d’erreurs plus aléatoire. Cela est dû au fait que les erreurs d’échantillonnage qui se produisent au niveau des limites peuvent être plus perceptibles, car le rastériseur de référence n’effectue pas cette opération avec une précision parfaite.

La meilleure approche consiste à utiliser le filtrage de point le plus proche uniquement lorsque cela est nécessaire. Quand vous devez l’utiliser, il est recommandé de décaler légèrement les coordonnées de texture à partir des positions de limite pour éviter les artefacts.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Filtrage de textures](texture-filtering.md)

 

 




