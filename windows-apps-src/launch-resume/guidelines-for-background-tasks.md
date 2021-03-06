---
title: Recommandations en matière de tâches en arrière-plan
description: Consultez des conseils détaillés sur le développement et l’exécution de tâches en arrière-plan in-process et out-of-process dans votre application.
ms.assetid: 18FF1104-1F73-47E1-9C7B-E2AA036C18ED
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, tâche en arrière-plan
ms.localizationpriority: medium
ms.openlocfilehash: b73568c5fb4bae6392051fedcd6ca3dea078a98d
ms.sourcegitcommit: 4491da3f509b1126601990a816c6eb301d35ecc6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/23/2020
ms.locfileid: "95416616"
---
# <a name="guidelines-for-background-tasks"></a>Recommandations en matière de tâches en arrière-plan


Assurez-vous que votre application répond aux exigences relatives à l’exécution de tâches en arrière-plan.

## <a name="background-task-guidance"></a>Recommandations en matière de tâches en arrière-plan

Tenez compte des recommandations suivantes au moment de développer une tâche en arrière-plan et avant de publier votre application.

Si vous utilisez une tâche en arrière-plan pour lire du contenu multimédia en arrière-plan, consultez [Lire du contenu multimédia en arrière-plan](../audio-video-camera/background-audio.md). Les informations fournies sur les améliorations apportées dans Windows 10 version 1607 vous simplifieront la vie.

**Tâches en arrière-plan hors processus ou in-process :** Windows 10 version 1607 propose des [tâches en arrière-plan in-process](create-and-register-an-inproc-background-task.md) qui vous permettent d’exécuter du code en arrière-plan dans le même processus que votre application au premier plan. Pour déterminer s’il est préférable de disposer de tâches en arrière-plan in-process ou hors processus, prenez en compte les facteurs suivants :

|Considération | Impact |
|--------------|--------|
|Résilience   | Si votre processus en arrière-plan s’exécute dans un autre processus, un blocage dans votre processus en arrière-plan ne bloque pas votre application au premier plan. De plus, l’activité en arrière-plan peut être arrêtée, même dans votre application, si elle s’exécute au-delà des limites de durée d’exécution. Séparer des tâches en arrière-plan dans une tâche distincte de l’application au premier plan peut être plus judicieux lorsque les processus au premier plan et en arrière-plan n’ont pas à communiquer entre eux (l’un des principaux avantages des tâches en arrière-plan in-process est qu’elles évitent toute communication entre les processus). |
|Simplicité    | Les tâches en arrière-plan in-process ne nécessitent aucune communication entre les processus et sont moins complexes à écrire.  |
|Déclencheurs disponibles | Les tâches en arrière-plan in-process ne prennent pas en charge les déclencheurs suivants : [DeviceUseTrigger](/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](/uwp/api/windows.applicationmodel.background.deviceservicingtrigger) et **IoTStartupTask**. |
|VoIP | Les tâches en arrière-plan in-process ne prennent pas en charge l’activation d’une tâche en arrière-plan VoIP dans votre application. |  

**Limites du nombre d’instances de déclencheur :** Il existe des limites quant au nombre d’instances d’un déclencheur qu’une application peut inscrire. Une application ne peut inscrire   [ApplicationTrigger](/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger), [MediaProcessingTrigger](/uwp/api/windows.applicationmodel.background.mediaprocessingtrigger) et [DeviceUseTrigger](/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396) qu’une seule fois par instance de l’application. Si une application dépasse cette limite, l’inscription lève une exception.

**Quotas de processeur :** les tâches en arrière-plan sont limitées par la quantité de temps d’utilisation de l’horloge en fonction de leur type de déclencheur. La plupart des déclencheurs sont limités à 30 secondes d’utilisation de l’horloge, mais certains ont une capacité d’exécution pouvant atteindre 10 minutes pour exécuter des tâches intensives. Les tâches en arrière-plan doivent être légères pour préserver l’autonomie de la batterie et assurer une meilleure expérience utilisateur pour les applications de premier plan. Pour plus d’informations sur les contraintes de ressource appliquées aux tâches en arrière-plan, consultez [Prendre en charge votre application avec des tâches en arrière-plan](support-your-app-with-background-tasks.md).

**Gérer les tâches en arrière-plan :** votre application doit obtenir la liste des tâches en arrière-plan inscrites, s’inscrire aux gestionnaires de progression et d’achèvement et gérer ces événements de manière appropriée. Vos classes de tâches en arrière-plan doivent signaler la progression, l’annulation et l’achèvement des tâches. Pour plus d’informations, consultez [Gérer une tâche en arrière-plan annulée](handle-a-cancelled-background-task.md) et [Surveiller la progression et l’achèvement des tâches en arrière-plan](monitor-background-task-progress-and-completion.md).

**Utilisez [BackgroundTaskDeferral](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskDeferral) :** si votre classe de tâches en arrière-plan exécute du code asynchrone, veillez à utiliser des reports. Sinon, votre tâche en arrière-plan peut se terminer prématurément lorsque la méthode [Run](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) retourne (ou la méthode [OnBackgroundActivated](/uwp/api/windows.ui.xaml.application.onbackgroundactivated) dans le cas des tâches en arrière-plan in-process). Pour plus d’informations, consultez [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-a-background-task.md).

L’autre solution consiste à demander un report et à utiliser **async/await** pour exécuter des appels de méthode asynchrone. Fermez le report après les appels de la méthode **await**.

**Mettez à jour le manifeste d’application :**  Pour les tâches en arrière-plan qui s’exécutent hors processus, déclarez chaque tâche en arrière-plan dans le manifeste de l’application, ainsi que le type de déclencheur avec lequel elle est utilisée. Sinon, votre application ne pourra pas inscrire la tâche en arrière-plan lors de l’exécution.

Si vous avez plusieurs tâches en arrière-plan, déterminez si elles doivent s’exécuter dans le même processus hôte ou être séparées dans différents processus hôtes. Placez-les dans des processus hôtes séparés si vous craignez qu’une défaillance dans une tâche en arrière-plan puisse réduire les autres tâches en arrière-plan.  Utilisez l’entrée de **groupe de ressources** dans le concepteur de manifeste pour regrouper les tâches en arrière-plan dans différents processus hôtes. 

Pour définir le **groupe de ressources**, ouvrez le concepteur package. appxmanifest, choisissez **déclarations** et ajoutez une déclaration de **app service** :

![Paramètre de groupe de ressources](images/resourcegroup.png)

Pour plus d’informations sur le paramètre de groupe de ressources, consultez [Référence du schéma d’application](/uwp/schemas/appxpackage/uapmanifestschema/element-application) .

Les tâches en arrière-plan qui s’exécutent dans le même processus que l’application au premier plan n’ont pas à se déclarer dans le manifeste de l’application. Pour plus d’informations sur la déclaration hors processus dans le manifeste, consultez [Déclarer des tâches en arrière-plan dans le manifeste de l’application](declare-background-tasks-in-the-application-manifest.md).

**Préparez les mises à jour de l’application :** si votre application doit subir des mises à jour, créez et inscrivez une tâche en arrière-plan **ServicingComplete** (consultez [SystemTriggerType](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType)) pour annuler l’inscription des tâches en arrière-plan de la version précédente de l’application et inscrivez les tâches en arrière-plan à la nouvelle version. Le moment est également idéal pour effectuer des mises à jour d’application qui peuvent se révéler nécessaires hors du contexte d’exécution au premier plan.

**Demander l’exécution des tâches en arrière-plan :**

> **Important**  À compter de Windows 10, les applications ne sont plus obligées de se trouver sur l’écran de verrouillage comme condition préalable à l’exécution des tâches en arrière-plan.

Les applications de plateforme Windows universelle (UWP) peuvent exécuter tous les types de tâches prises en charge, sans être épinglées à l’écran de verrouillage. Toutefois, les applications doivent appeler [**GetAccessState**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.getaccessstatus) et vérifier que l’exécution de l’application en arrière-plan n’est pas refusée. Vérifiez que **GetAccessStatus** ne retourne pas l’une des énumérations [**BackgroundAccessStatus**](/uwp/api/windows.applicationmodel.background.backgroundaccessstatus) refusées. Par exemple, cette méthode retourne **BackgroundAccessStatus. DeniedByUser** si l’utilisateur a refusé explicitement des autorisations de tâche en arrière-plan pour votre application dans les paramètres de l’appareil.

Si l’exécution de votre application est refusée en arrière-plan, votre application doit appeler [**RequestAccessAsync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) et garantir que la réponse n’est pas refusée avant l’inscription des tâches en arrière-plan.

Pour plus d’informations sur le choix des utilisateurs concernant l’activité en arrière-plan et l’économiseur de batterie, consultez [optimiser l’activité en arrière-plan](../debug-test-perf/optimize-background-activity.md) 
## <a name="background-task-checklist"></a>Liste de vérifications relatives aux tâches en arrière-plan

*S’applique aux tâches en arrière-plan in-process et hors processus*

-   Associez votre tâche en arrière-plan au déclencheur approprié.
-   Ajoutez des conditions pour assurer une exécution aboutie de votre tâche en arrière-plan.
-   Gérez la progression, l’achèvement et l’annulation de la tâche en arrière-plan.
-   Réinscrivez vos tâches en arrière-plan pendant le lancement de l’application. De cette façon, elles seront inscrites au premier lancement de l’application. Cela permet également de détecter si l’utilisateur a désactivé les fonctionnalités d’exécution en arrière-plan de votre application (en cas d’échec de l’inscription).
-   Recherchez les erreurs d’inscription de la tâche en arrière-plan. Le cas échéant, tentez d’inscrire la tâche en arrière-plan une nouvelle fois avec d’autres valeurs de paramètres.
-   Pour toutes les familles d’appareils, à l’exception des ordinateurs de bureau, les tâches en arrière-plan peuvent être arrêtées en cas de mémoire insuffisante de l’appareil. Si aucune exception de mémoire insuffisante n’est exposée ou si l’application ne la gère pas, la tâche en arrière-plan est alors arrêtée sans avertissement ni déclenchement de l’événement OnCanceled. Cela permet de garantir l’expérience utilisateur de l’application au premier plan. Votre tâche en arrière-plan doit être conçue de manière à gérer ce scénario.

*S’applique uniquement aux tâches en arrière-plan hors processus*

-   Créez votre tâche en arrière-plan dans un composant Windows Runtime.
-   N’affichez pas l’interface utilisateur hormis les toasts, vignettes et mises à jour de badge de la tâche en arrière-plan.
-   Dans la méthode [Run](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run), demandez des reports pour chaque appel de méthode asynchrone, puis fermez-les lorsque la méthode est terminée. Vous pouvez également utiliser un report avec **async/await**.
-   Utilisez un dispositif de stockage persistant pour partager des données entre la tâche en arrière-plan et l’application.
-   Déclarez chaque tâche en arrière-plan dans le manifeste de l’application, de même que le type de déclencheur avec lequel elle est utilisée. Vérifiez que le point d’entrée et les types de déclencheur sont corrects.
-   Ne spécifiez aucun élément Executable dans le manifeste sauf si vous utilisez un déclencheur à exécuter dans le même contexte que l’application (comme le déclencheur [**ControlChannelTrigger**](/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger)).

*S’applique uniquement aux tâches en arrière-plan in-process*

- Lors de l’annulation d’une tâche, vérifiez que le gestionnaire d’événements `BackgroundActivated` se ferme avant l’annulation ou la fin du processus.
-   Écrivez des tâches en arrière-plan de courte durée. La plupart des tâches en arrière-plan sont limitées à 30 secondes d’utilisation de l’horloge du mur.


*Pratiques à éviter*
- Réduisez l’utilisation de la communication entre processus via COM ou RPC.
-   Le processus avec lequel vous tentez de communiquer peut ne pas être à l’État en cours d’exécution, ce qui peut entraîner un blocage.
-   Un laps de temps considérable peut être consacré à la communication entre processus, et est compté par rapport au temps alloué à l’exécution de votre tâche en arrière-plan.
- Ne comptez pas sur l’interaction utilisateur dans les tâches en arrière-plan.


## <a name="related-topics"></a>Rubriques connexes

* [Créer et inscrire une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md)
* [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-a-background-task.md)
* [Créer et inscrire une tâche en arrière-plan COM winmain](create-and-register-a-winmain-background-task.md)
* [Déclarer des tâches en arrière-plan dans le manifeste de l’application](declare-background-tasks-in-the-application-manifest.md)
* [Lire le média en arrière-plan](../audio-video-camera/background-audio.md)
* [Gérer une tâche en arrière-plan annulée](handle-a-cancelled-background-task.md)
* [Surveiller la progression et l’achèvement des tâches en arrière-plan](monitor-background-task-progress-and-completion.md)
* [Inscrire une tâche en arrière-plan](register-a-background-task.md)
* [Répondre aux événements système avec des tâches en arrière-plan](respond-to-system-events-with-background-tasks.md)
* [Définir des conditions pour l’exécution d’une tâche en arrière-plan](set-conditions-for-running-a-background-task.md)
* [Mettre à jour une vignette dynamique à partir d’une tâche en arrière-plan](update-a-live-tile-from-a-background-task.md)
* [Utiliser un déclencheur de maintenance](use-a-maintenance-trigger.md)
* [Exécuter une tâche en arrière-plan en fonction d’un minuteur](run-a-background-task-on-a-timer-.md)
* [Déboguer une tâche en arrière-plan](debug-a-background-task.md)
* [Comment déclencher des événements de suspension, de reprise et d’arrière-plan dans des applications UWP (lors du débogage)](/previous-versions/hh974425(v=vs.110))

 

 
