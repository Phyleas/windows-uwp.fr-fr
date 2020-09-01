---
title: Ressources système pour les applications UWP et les jeux sur Xbox One
description: Découvrez comment les applications UWP s’exécutant sur Xbox partagent des ressources avec le système et d’autres applications, et sur les besoins en ressources pour les applications ou les jeux UWP.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 12e87019-4315-424e-b73c-426d565deef9
ms.localizationpriority: medium
ms.openlocfilehash: dbc6af56867508e3178ddd8b731af49270faf3d6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174683"
---
# <a name="system-resources-for-uwp-apps-and-games-on-xbox-one"></a>Ressources système pour les applications UWP et les jeux sur Xbox One

Les applications UWP s’exécutant sur Xbox One partagent des ressources avec le système et d’autres applications. Les ressources disponibles pour une application UWP sur Xbox varient selon que vous soumettez comme application ou en tant que jeu de programme de créateurs Xbox Live.

* Mémoire maximale disponible lors de l’exécution au premier plan :
    * Applications : 1 Go
    * Jeux : 5 Go

La mémoire maximale disponible pour une application en cours d’exécution en arrière-plan est de 128 Mo. Le mode arrière-plan s’applique uniquement aux applications simultanées, comme les lecteurs musicaux en arrière-plan.  Les jeux seront suspendus et arrêtés en arrière-plan.


Le dépassement de ces limitations entraîne des échecs d’allocation de mémoire. Pour en savoir plus sur la surveillance de la mémoire, consultez la documentation de référence sur la [classe MemoryManager](/uwp/api/windows.system.memorymanager).

> [!NOTE]
> Lors de l’exécution de votre application ou de votre jeu à partir du débogueur Visual Studio, ces contraintes de mémoire ne s’appliquent pas. Cette limite s’applique uniquement quand l’exécution n’a pas lieu en mode débogage.

* UC
    * Applications : partage de cœurs de processeur 2-4 en fonction du nombre d’applications et de jeux exécutés sur le système.
    * Jeux : 4 cœurs d’UC exclusifs et 2 cœurs d’UC partagés.

* GPU
    * Applications : partage de 45% du GPU en fonction du nombre d’applications et de jeux exécutés sur le système.
    * Jeux : accès complet aux cycles GPU disponibles.

* Prise en charge de DirectX
    * Applications : niveau de fonctionnalité DirectX 11 10.
    * Jeux : DirectX 12 et DirectX 11 Feature Level 10.

* Toutes les applications et tous les jeux doivent cibler l’architecture x64 afin d’être développés ou envoyés au Store pour Xbox.  

Pour le **développement d’applications**, les ressources disponibles peuvent être limitées par rapport à un PC standard et peuvent varier en fonction du nombre d’applications et de jeux exécutés sur le système.

Pour le **développement de jeux**, Xbox One, comme les autres consoles de jeux, est un élément matériel spécialisé qui nécessite un kit de développement matériel spécifique pour accéder à son potentiel complet. Si vous travaillez sur un jeu qui nécessite l’accès au maximum du potentiel de la Xbox, envisagez de vous inscrire auprès du [ID@Xbox](https://www.xbox.com/Developers/id) programme pour avoir accès à un kit de développement Xbox One.

## <a name="see-also"></a>Voir aussi
- [UWP sur Xbox One](index.md)
- [Prise en main du programme de créateurs Xbox Live](/gaming/xbox-live/get-started-with-creators/creators-program)
- [DirectX et UWP sur Xbox One](https://walbourn.github.io/)