---
title: Appareils perdus
description: Un appareil Direct3D peut être dans un état opérationnel ou perdu.
ms.assetid: 1639CC02-8000-4208-AA95-91C1F0A3B08D
keywords:
- Appareils perdus
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d7f2c06564e0477fcec67418cae739e2fae123d7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158853"
---
# <a name="lost-devices"></a>Appareils perdus


Un appareil Direct3D peut être dans un état opérationnel ou perdu. L’état *opérationnel* est l’état normal de l’appareil dans lequel l’appareil s’exécute et affiche tout le rendu comme prévu. L’appareil effectue une transition vers l’état *perdu* lorsqu’un événement, tel que la perte du focus clavier dans une application en plein écran, rend le rendu impossible. L’état perdu est caractérisé par l’échec silencieux de toutes les opérations de rendu, ce qui signifie que les méthodes de rendu peuvent retourner des codes de réussite même si les opérations de rendu échouent.

Par défaut, le jeu complet de scénarios pouvant entraîner la perte d’un appareil n’est pas spécifié. Parmi les exemples classiques, citons la perte de focus, par exemple quand l’utilisateur appuie sur ALT + TAB ou lorsqu’une boîte de dialogue système est initialisée. Les appareils peuvent également être perdus en raison d’un événement de gestion de l’alimentation ou lorsqu’une autre application suppose une opération en plein écran. En outre, tout échec de la réinitialisation d’un appareil met l’appareil dans un état perdu.

Toutes les méthodes dérivées de [**IUnknown**](/windows/desktop/api/unknwn/nn-unknwn-iunknown) sont assurées de fonctionner après la perte d’un appareil. Après la perte de l’appareil, chaque fonction a généralement les trois options suivantes :

-   Échec avec une erreur « appareil perdu »-cela signifie que l’application doit reconnaître que l’appareil a été perdu, de sorte que l’application identifie qu’un événement ne se produit pas comme prévu.
-   Échec en mode silencieux, en renvoyant \_ un ou plusieurs codes de retour : si une fonction échoue en mode silencieux, l’application ne peut généralement pas faire la distinction entre le résultat de « succès » et une « défaillance silencieuse ».
-   Retourne un code de retour.

## <a name="span-idresponding_to_a_lost_devicespanspan-idresponding_to_a_lost_devicespanspan-idresponding_to_a_lost_devicespanresponding-to-a-lost-device"></a><span id="Responding_to_a_Lost_Device"></span><span id="responding_to_a_lost_device"></span><span id="RESPONDING_TO_A_LOST_DEVICE"></span>Réponse à un appareil perdu


Un appareil perdu doit recréer les ressources (y compris les ressources mémoire vidéo) après sa réinitialisation. Si un appareil est perdu, l’application interroge l’appareil pour voir s’il peut être restauré à l’état opérationnel. Si ce n’est pas le cas, l’application attend que l’appareil puisse être restauré.

Si l’appareil peut être restauré, l’application prépare l’appareil en détruisant toutes les ressources de la mémoire vidéo et toutes les chaînes de permutation. La réinitialisation est la seule procédure qui a un effet lorsqu’un appareil est perdu, et est le seul moyen par lequel une application peut remplacer l’appareil par un état opérationnel. La réinitialisation échouera à moins que l’application libère toutes les ressources allouées, y compris les cibles de rendu et les surfaces du stencil de profondeur.

Pour l’essentiel, les appels haute fréquence de Direct3D ne retournent aucune information indiquant si l’appareil a été perdu. L’application peut continuer à appeler des méthodes de rendu, sans recevoir de notification d’un appareil perdu. En interne, ces opérations sont ignorées jusqu’à ce que l’appareil soit réinitialisé à l’état opérationnel.

## <a name="span-idlocking_operationsspanspan-idlocking_operationsspanspan-idlocking_operationsspanlocking-operations"></a><span id="Locking_Operations"></span><span id="locking_operations"></span><span id="LOCKING_OPERATIONS"></span>Opérations de verrouillage


En interne, Direct3D effectue suffisamment de travail pour s’assurer qu’une opération de verrouillage va être effectuée après la perte d’un appareil. Toutefois, il n’est pas garanti que les données de la ressource de mémoire vidéo seront exactes au cours de l’opération de verrouillage. Il est garanti qu’aucun code d’erreur ne sera renvoyé. Cela permet d’écrire des applications sans se préoccuper de la perte d’appareil pendant une opération de verrouillage.

## <a name="span-idresourcesspanspan-idresourcesspanspan-idresourcesspanresources"></a><span id="Resources"></span><span id="resources"></span><span id="RESOURCES"></span>Situées


Les ressources peuvent consommer de la mémoire vidéo. Étant donné qu’un appareil perdu est déconnecté de la mémoire vidéo appartenant à la carte, il n’est pas possible de garantir l’allocation de mémoire vidéo lorsque l’appareil est perdu. Par conséquent, toutes les méthodes de création de ressources sont implémentées pour aboutir, mais elles allouent en fait uniquement de la mémoire système factice. Étant donné que toutes les ressources de mémoire vidéo doivent être détruites avant le redimensionnement de l’appareil, il n’y a aucun problème de dépassement de la mémoire vidéo. Ces surfaces factices permettent aux opérations de verrouillage et de copie de fonctionner normalement jusqu’à ce que l’application découvre que l’appareil a été perdu.

Toute la mémoire vidéo doit être libérée pour qu’un appareil puisse être réinitialisé à un état opérationnel. Les autres données d’État sont automatiquement détruites par la transition vers un état opérationnel.

Vous êtes encouragé à développer des applications avec un seul chemin de code pour répondre à la perte de l’appareil. Ce chemin de code est susceptible d’être similaire, s’il n’est pas identique, au chemin de code utilisé pour initialiser l’appareil au démarrage.

## <a name="span-idretrieved_dataspanspan-idretrieved_dataspanspan-idretrieved_dataspanretrieved-data"></a><span id="Retrieved_Data"></span><span id="retrieved_data"></span><span id="RETRIEVED_DATA"></span>Données récupérées


Direct3D permet aux applications de valider les États de texture et de rendu par rapport au rendu sur un seul passage par le matériel.

Direct3D permet également aux applications de copier des images générées ou écrites précédemment à partir de ressources de mémoire vidéo vers des ressources de mémoire système non volatiles. Étant donné que les images sources de ces transferts peuvent être perdues à tout moment, Direct3D autorise l’échec de ces opérations de copie lorsque l’appareil est perdu.

Les opérations de copie peuvent échouer, car il n’y a aucune surface primaire lorsque l’appareil est perdu. La création de chaînes de permutation peut également échouer, car une mémoire tampon d’arrière-plan ne peut pas être créée lorsque l’appareil est perdu.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Appareils](devices.md)

 

 