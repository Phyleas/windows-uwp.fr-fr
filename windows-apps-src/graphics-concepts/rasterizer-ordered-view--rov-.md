---
title: Affichage ordonné du rastériseur (ROV)
description: Les affichages ordonnés du rastériseur permettent de traiter certaines limitations associées aux tampons de profondeur, en particulier le fait d’avoir plusieurs textures contenant de la transparence, toutes s’appliquant aux mêmes pixels.
ms.assetid: BCB1EE0D-4C1D-4E17-BDB7-173F448E0A7B
keywords:
- Affichage ordonné du rastériseur (ROV)
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: bb75d903bee3990b13e261393f3801b4089ec1b0
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2018
ms.locfileid: "1042998"
---
# <a name="rasterizer-ordered-view-rov"></a>Affichage ordonné du rastériseur (ROV)


Les affichages ordonnés du rastériseur permettent de traiter certaines limitations associées aux tampons de profondeur, en particulier le fait d’avoir plusieurs textures contenant de la transparence, toutes s’appliquant aux mêmes pixels.

Les affichages ordonnés du rastériseur permettent d’appliquer les algorithmes de type OIT («Order Independent Transparency») au rendu des pixels. Un tampon de profondeur permet uniquement de dessiner ou masquer un pixel, le concept d’occlusion partielle par transparence n’existe pas. Les algorithmes de type OIT appliquent les textures transparentes dans le bon ordre; ainsi, si, par exemple, un objet en verre transparent doit apparaître derrière une vitre en verre qui se trouve elle-même derrière de la végétation qui utilise des textures transparentes, alors le résultat final est dessiné de façon correcte et prévisible. Sans ROV et sans algorithmes de type OIT, l’ordre dans lequel ces objets transparents seraient dessinés n’était pas prévisible, et la scène rendue pouvait tout simplement prêter à confusion et être erronée.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Affichages](views.md)

 

 



