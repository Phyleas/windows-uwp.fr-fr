---
title: Choix d’une ressource
description: Découvrez comment identifier et sélectionner les meilleures ressources à lier à différentes étapes du pipeline graphique 3D dans votre application.
ms.assetid: 6BAD6287-2930-42F8-BF51-69A379D1D2C3
keywords:
- Choix d’une ressource
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 69527915988136854e201b82332c17f3e0134290
ms.sourcegitcommit: 45dec3dc0f14934b8ecf1ee276070b553f48074d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89094467"
---
# <a name="choosing-a-resource"></a>Choix d’une ressource


Une ressource est une collection de données qui est utilisée par le pipeline 3D. La création de ressources et la définition de leur comportement constituent la première étape de la programmation de votre application. Ce guide couvre les rubriques de base pour le choix des ressources requises par votre application.

## <a name="span-ididentify_bindingspanspan-ididentify_bindingspanspan-ididentify_bindingspanidentify-pipeline-stages-that-need-resources"></a><span id="Identify_Binding"></span><span id="identify_binding"></span><span id="IDENTIFY_BINDING"></span>Identifier les étapes de pipeline nécessitant des ressources


La première étape consiste à choisir l’étape (ou les étapes) du [pipeline graphique](graphics-pipeline.md) qui utilisera une ressource. Autrement dit, Identifiez chaque étape qui lira des données à partir d’une ressource, ainsi que les étapes permettant d’écrire des données dans une ressource. Connaître les étapes de pipeline dans lesquelles les ressources seront utilisées détermine les API qui seront appelées pour lier la ressource à l’étape.

Ce tableau répertorie les types de ressources qui peuvent être liées à chaque étape du pipeline. Elle indique si la ressource peut être liée en tant qu’entrée ou en sortie.

| Étape de pipeline  | Entrée/Sortie | Ressource               | Type de ressource                           |
|-----------------|--------|------------------------|-----------------------------------------|
| Assembleur d'entrée | Dans     | Mémoire tampon de vertex          | Buffer                                  |
| Assembleur d'entrée | Dans     | Mémoire tampon d’index           | Buffer                                  |
| Étapes du nuanceur   | Dans     | Nuanceur-ResourceView    | Buffer, Texture1D, Texture2D, Texture3D |
| Étapes du nuanceur   | Dans     | Tampon de nuanceur-constante | Buffer                                  |
| Sortie de flux   | Sortie    | Buffer                 | Buffer                                  |
| Fusion de sortie   | Sortie    | Affichage de la cible de rendu     | Buffer, Texture1D, Texture2D, Texture3D |
| Fusion de sortie   | Sortie    | Affichage de la profondeur/du gabarit     | Texture1D, Texture2D                    |

 

## <a name="span-ididentify_usagespanspan-ididentify_usagespanspan-ididentify_usagespanidentify-how-each-resource-will-be-used"></a><span id="Identify_Usage"></span><span id="identify_usage"></span><span id="IDENTIFY_USAGE"></span>Identifier la manière dont chaque ressource sera utilisée


Une fois que vous avez choisi les étapes de pipeline que votre application utilisera (et par conséquent les ressources requises par chaque étape), l’étape suivante consiste à déterminer la manière dont chaque ressource sera utilisée, c’est-à-dire, si une ressource est accessible par le processeur ou le GPU.

Le matériel sur lequel votre application s’exécute aura au minimum au moins un processeur et un GPU. Pour choisir une valeur d’utilisation, déterminez le type de processeur qui doit lire ou écrire dans la ressource à partir des options suivantes.

| Utilisation des ressources | Peut être mis à jour par                    | Fréquence des mises à jour |
|----------------|--------------------------------------|---------------------|
| Default        | GPU                                  | rarement        |
| Dynamique        | UC                                  | fréquemment          |
| Staging        | GPU                                  | n/a                 |
| Non modifiable      | UC (uniquement au moment de la création de la ressource) | n/a                 |

 

L’utilisation par défaut doit être utilisée pour une ressource qui est censée être mise à jour par l’UC rarement (moins d’une fois par frame). Dans l’idéal, l’UC n’écrira jamais directement dans une ressource avec une utilisation par défaut afin d’éviter des pénalités de performances potentielles.

L’utilisation dynamique doit être utilisée pour une ressource que l’UC met à jour relativement souvent (une ou plusieurs fois par trame). Un scénario classique pour une ressource dynamique consiste à créer des tampons de vertex et d’index dynamiques qui seraient remplis au moment de l’exécution avec des données relatives à la géométrie visible du point de vue de l’utilisateur pour chaque image. Ces mémoires tampons sont utilisées pour afficher uniquement la géométrie visible par l’utilisateur pour ce frame.

L’utilisation intermédiaire doit être utilisée pour copier des données vers et à partir d’autres ressources. Un scénario classique consiste à copier des données dans une ressource avec l’utilisation par défaut (à laquelle l’UC ne peut pas accéder) à une ressource avec une utilisation intermédiaire (à laquelle l’UC peut accéder).

Les ressources immuables doivent être utilisées lorsque les données de la ressource ne seront jamais modifiées.

Une autre façon de regarder la même idée est de réfléchir à ce que fait une application avec une ressource.

| Comment l’application utilise la ressource     | Utilisation des ressources       |
|---------------------------------------|----------------------|
| Charger une seule fois et ne jamais la mettre à jour            | Immuable ou par défaut |
| L’application remplit les ressources à plusieurs reprises | Dynamique              |
| Restituer à la texture                     | Default              |
| Accès de l’UC des données GPU                | Staging              |

 

Si vous n’êtes pas sûr de l’utilisation à choisir, commencez par utiliser l’utilisation par défaut, car il est supposé être le cas le plus courant. Une mémoire tampon de nuanceur constante est le type de ressource qui doit toujours avoir une utilisation par défaut.

## <a name="span-idresource_types_and_pipeline_stagesspanspan-idresource_types_and_pipeline_stagesspanspan-idresource_types_and_pipeline_stagesspanbinding-resources-to-pipeline-stages"></a><span id="Resource_Types_and_Pipeline_stages"></span><span id="resource_types_and_pipeline_stages"></span><span id="RESOURCE_TYPES_AND_PIPELINE_STAGES"></span>Liaison de ressources à des étapes de pipeline


Une ressource peut être liée à plusieurs étapes de pipeline en même temps, à condition que les restrictions spécifiées lors de la création de la ressource soient remplies. Ces restrictions sont spécifiées comme indicateurs d’utilisation, indicateurs de liaison ou indicateurs d’accès au processeur. Plus précisément, une ressource peut être liée en tant qu’entrée et en sortie simultanément, à condition que la lecture et l’écriture d’une partie d’une ressource ne puissent pas se produire en même temps.

Lors de la liaison d’une ressource, réfléchissez à la façon dont le GPU et l’UC accèdent à la ressource. Les ressources qui sont conçues pour un seul but (n’utilisez pas plusieurs indicateurs d’accès d’utilisation, de liaison et d’UC) peuvent avoir une meilleure performance.

Par exemple, considérez le cas d’une cible de rendu utilisée comme texture plusieurs fois. Il peut être plus rapide d’avoir deux ressources : une cible de rendu et une texture utilisées comme ressource de nuanceur. Chaque ressource n’utilise qu’un seul indicateur de liaison, indiquant « cible de rendu » ou « ressource de nuanceur ». Les données sont copiées de la texture de cible de rendu vers la texture du nuanceur.

Dans cet exemple, cette technique peut améliorer les performances en isolant l’écriture de la cible de rendu à partir de la lecture de la texture du nuanceur. La seule façon d’être sûr est d’implémenter les deux approches et de mesurer la différence de performances dans votre application.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Ressources](resources.md)

 

 




