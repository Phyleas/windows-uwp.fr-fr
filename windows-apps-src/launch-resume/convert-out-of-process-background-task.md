---
title: Porter une tâche en arrière-plan hors processus vers une tâche en arrière-plan in-process
description: Portage d’une tâche en arrière-plan out-of-process vers une tâche en arrière-plan in-process qui s’exécute au sein de votre processus d’application de premier plan.
ms.date: 09/19/2018
ms.topic: article
keywords: Windows 10, UWP, tâche en arrière-plan, app service
ms.assetid: 5327e966-b78d-4859-9b97-5a61c362573e
ms.localizationpriority: medium
ms.openlocfilehash: f500be50c8b57415f5ad2fe62c28cc21f4b57b68
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162653"
---
# <a name="port-an-out-of-process-background-task-to-an-in-process-background-task"></a>Porter une tâche en arrière-plan hors processus vers une tâche en arrière-plan in-process

La façon la plus simple de porter votre activité d’arrière-plan de la programmation en dehors des processus (OOP) vers l’activité en cours consiste à placer votre code de méthode [IBackgroundTask. Run](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run?f=255&MSPPError=-2147217396) dans votre application et à le lancer à partir de [OnBackgroundActivated](/uwp/api/windows.ui.xaml.application.onbackgroundactivated). La technique décrite ici ne consiste pas à créer un shim à partir d’une tâche en arrière-plan OOP vers une tâche en arrière-plan in-process. Il s’agit de réécrire (ou de déplacer) une version OOP vers une version in-process.

Si votre application comporte plusieurs tâches en arrière-plan, [l’exemple d’activation en arrière-plan](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation) montre comment vous pouvez utiliser `BackgroundActivatedEventArgs.TaskInstance.Task.Name` pour identifier la tâche qui est lancée.

Si vous êtes en train de communiquer entre les processus en arrière-plan et au premier plan, vous pouvez supprimer ce code de communication et de gestion de l’état.

## <a name="background-tasks-and-trigger-types-that-cannot-be-converted"></a>Types de déclencheur et tâches en arrière-plan qui ne peuvent pas être convertis

* Les tâches en arrière-plan intégrées au processus ne prennent pas en charge l’activation d’une tâche VoIP en arrière-plan.
* Les tâches en arrière-plan in-process ne prennent pas en charge les déclencheurs suivants :  [DeviceUseTrigger](/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](/uwp/api/windows.applicationmodel.background.deviceservicingtrigger) et **IoTStartupTask**