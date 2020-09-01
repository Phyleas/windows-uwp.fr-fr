---
title: Cycle de vie d’une application UWP Windows 10
description: Cette rubrique décrit le cycle de vie d’une application de plateforme Windows universelle (UWP) Windows 10, depuis son activation jusqu’à sa fermeture.
keywords: cycle de vie d’application suspendue reprise lancement activer
ms.assetid: 6C469E77-F1E3-4859-A27B-C326F9616D10
ms.date: 01/23/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 519ff7ee7bd581ce0f19c0e4297ad1ba34380068
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173033"
---
# <a name="windows-10-universal-windows-platform-uwp-app-lifecycle"></a>Cycle de vie d’une application de plateforme Windows universelle (UWP) Windows 10


Cette rubrique décrit le cycle de vie d’une application de plateforme Windows universelle (UWP), depuis son activation jusqu’à sa fermeture.

## <a name="a-little-history"></a>Un peu d’histoire

Avant Windows 8, les applications avaient un cycle de vie simple. Les applications Win32 et .NET sont en cours d’exécution ou pas. Lorsqu’un utilisateur les réduit ou les ferme, elles continuent de s’exécuter. Cela ne posait aucun problème jusqu’à ce que les appareils mobiles et la gestion de l’alimentation prennent une importance croissante.

Windows 8 a introduit un nouveau modèle d’application avec des applications UWP. Globalement, un état suspendu a été ajouté. Une application UWP est suspendue peu après que l’utilisateur l’a réduite ou bascule vers une autre application. Autrement dit, les threads de l’application sont arrêtés et l’application reste en mémoire, sauf si le système d’exploitation a besoin de récupérer des ressources. Lorsque l’utilisateur revient à l’application, celle-ci peut rapidement reprendre un état d’exécution.

Plusieurs modes permettent aux applications de continuer de s’exécuter en arrière-plan, comme les [tâches en arrière-plan](support-your-app-with-background-tasks.md), l’[exécution étendue](/uwp/api/windows.applicationmodel.extendedexecution) et l’exécution commanditée par une activité (par exemple, la fonctionnalité **BackgroundMediaEnabled** qui permet à une application de continuer à [lire du contenu multimédia en arrière-plan](../audio-video-camera/background-audio.md)). De plus, les opérations de transfert en arrière-plan se poursuivent, même si votre application est suspendue ou arrêtée. Pour plus d’informations, consultez [Comment télécharger un fichier](/previous-versions/windows/apps/jj152726(v=win.10)).

Par défaut, les applications qui ne sont pas au premier plan sont suspendues afin d’économiser l’énergie et de libérer des ressources pour l’application au premier plan.

Cet état suspendu vous rajoute des contraintes aux développeurs, car le système d’exploitation peut décider d’arrêter une application suspendue afin de libérer des ressources. L’application arrêtée reste visible dans la barre des tâches. Lorsque l’utilisateur clique dessus, l’application doit restaurer l’état qui était le sien avant d’être arrêtée, car l’utilisateur ne sait pas que le système l’a fermée. Il pense qu’elle est en attente en arrière-plan pendant qu’il effectue d’autres opérations et qu’elle va reprendre le même état qu’auparavant. Dans cette rubrique, nous allons examiner comment procéder.

Windows 10 version 1607 utilise deux états supplémentaires d’application : **Exécution au premier plan** et **Exécution en arrière-plan**. Nous allons également examiner ces nouveaux états dans les sections suivantes.

## <a name="app-execution-state"></a>État d’exécution de l’application

L’illustration suivante représente les états possibles d’une application dans Windows 10 version 1607. Passons en revue le cycle de vie classique d’une application UWP.

![Diagramme d’état indiquant les transitions entre les états d’exécution d’une application](images/updated-lifecycle.png)

Les applications adoptent l’état d’exécution en arrière-plan lorsque l’utilisateur les démarre ou les active. Si l’application doit se déplacer au premier plan en raison d’un lancement d’application de premier plan, l’application obtient l’événement [**LeavingBackground**](/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground) .

Bien que les termes « lancé » et « activé » puissent paraître similaires, ils font référence aux différentes façons dont le système d’exploitation peut démarrer votre application. Examinons tout d’abord le lancement d’une application.

## <a name="app-launch"></a>Lancement d’une application

Lorsqu’une application est lancée, la méthode [**OnLaunched**](/uwp/api/windows.ui.xaml.application.onlaunched) est appelée. Elle reçoit un paramètre [**LaunchActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs) qui indique, entre autres, les arguments transmis à l’application, l’identificateur de la vignette qui a lancé l’application et l’état antérieur de l’application.

Obtenez l’état antérieur de votre application grâce à [LaunchActivatedEventArgs.PreviousExecutionState](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.previousexecutionstate) qui renvoie un état [ApplicationExecutionState](/uwp/api/windows.applicationmodel.activation.applicationexecutionstate). Ses valeurs et l’action appropriée à effectuer selon cet état sont les suivantes :

| ApplicationExecutionState | Explication | Action à effectuer |
|-------|-------------|----------------|
| **Pas en cours d'exécution** | Une application peut être dans cet état, si elle n’a pas été lancée depuis le dernier redémarrage de l’ordinateur ou la dernière ouverture de session. Elle peut également être dans cet état si une erreur d’exécution l’a bloquée ou si l’utilisateur l’a fermée auparavant.| Initialisez l’application comme si elle s’exécutait pour la première fois dans la session utilisateur active. |
|**Suspendu** | L’utilisateur a réduit l’application ou activé une autre application et n’est pas revenu à la première après quelques secondes. | Lorsque l’application est suspendue, son état est conservé en mémoire. Il vous suffit de vous réapproprier les descripteurs de fichiers ou d’autres ressources qui ont été libérés lorsque l’application a été suspendue. |
| **Terminé** | L’application a été suspendue puis arrêtée, car le système a dû libérer de la mémoire. | Restaurez l’application dans l’état qui était le sien lorsque l’utilisateur a basculé vers une autre application.|
|**ClosedByUser** | L’utilisateur a fermé l’application en effectuant le mouvement de fermeture en mode tablette ou en appuyant sur Alt + F4. Lorsque l’utilisateur ferme l’application, celle-ci est suspendue puis arrêtée. | Comme l’application a suivi les mêmes étapes qui aboutissent à l’état Terminated, gérez cette situation comme l’état Terminated.|
|**Exécution** | L’application était déjà ouverte lorsque l’utilisateur a essayé de la relancer. | Rien. Notez qu’aucune autre instance de votre application n’est lancée. L’instance en cours d’exécution est simplement activée. |

**Remarque** La   *session utilisateur active* est basée sur l’ouverture de session Windows. Tant que l’utilisateur actuel ne s’est pas déconnecté ou n’a pas arrêté ou redémarré Windows, la session utilisateur reste active entre des événements, tels que l’authentification de l’écran de verrouillage, le changement d’utilisateur, etc. 

Gardez à l’esprit que si l’appareil dispose de ressources suffisantes, le système d’exploitation prélance fréquemment les applications qui ont autorisé ce comportement, afin d’optimiser la réactivité. Les applications prélancées démarrent en arrière-plan puis sont rapidement suspendues, permettant ainsi à l’utilisateur de les réactiver, une opération plus rapide qu’un démarrage.

Du fait du prélancement, la méthode **OnLaunched()** de l’application peut être initiée par le système plutôt que par l’utilisateur. Comme l’application est prélancée en arrière-plan, il est possible que vous deviez effectuer une autre action dans **OnLaunched()**. Par exemple, si votre application lit de la musique en cas de lancement, l’utilisateur ne peut pas savoir d’où le son provient, car l’application est prélancée en arrière-plan. Le prélancement de votre application en arrière-plan est suivi d’un appel à **Application.Suspending**. Ensuite, lorsque l’utilisateur démarre l’application, l’événement de reprise est appelé ainsi que la méthode **OnLaunched()**. Pour plus d’informations sur la gestion du prélancement, consultez [Gérer le prélancement d’une application](handle-app-prelaunch.md). Seules les applications qui autorisent cette opération sont prélancées.

Windows affiche un écran de démarrage pour l’application lancée. Pour configurer cet écran de démarrage, consultez [Ajout d’un écran de démarrage](/previous-versions/windows/apps/hh465331(v=win.10)).

Lorsque l’écran de démarrage s’affiche, votre application doit enregistrer les gestionnaires d’événements et configurer l’interface utilisateur personnalisée dont elle a besoin pour la page initiale. Vérifiez que ces tâches s’exécutent dans le constructeur de l’application et dans la méthode **OnLaunched()** en quelques secondes. Sinon, le système peut penser que votre application ne répond pas et l’arrêter. Si une application doit demander des données au réseau ou récupérer de grandes quantités de données sur le disque, ces activités doivent être effectuées hors du lancement. Une application peut utiliser son interface utilisateur de chargement personnalisée ou un écran de démarrage étendu, pendant l’exécution de ces longues opérations. Pour plus d’informations, consultez [Afficher un écran de démarrage plus longtemps](create-a-customized-splash-screen.md) et cet [exemple d’écran de démarrage](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/SplashScreen).

Une fois lancée, l’application adopte l’état **Running** et l’écran de démarrage disparaît (ses ressources et objets sont effacés).

## <a name="app-activation"></a>Activation d’une application

Une application peut être lancée par l’utilisateur ou activée par le système. Une application peut être activée par un contrat, comme un contrat de partage. Elle peut aussi être activée pour gérer un protocole d’URI personnalisé ou un fichier avec une extension que votre application est configurée pour gérer. Pour obtenir la liste des modes d’activation possibles de votre application, consultez [**ActivationKind**](/uwp/api/Windows.ApplicationModel.Activation.ActivationKind).

La classe [**Windows.UI.Xaml.Application**](/uwp/api/Windows.UI.Xaml.Application) définit les méthodes que vous pouvez utiliser pour gérer les différents modes d’activation de votre application.
[**OnActivated**](/uwp/api/windows.ui.xaml.application.onactivated) peut gérer tous les types d’activation possibles. Toutefois, il est plus courant d’employer certaines méthodes pour gérer les modes d’activation les plus courants et d’utiliser **OnActivated** pour les modes d’activation plus rares. Les méthodes autorisant des activations spécifiques sont les suivantes :

[**OnCachedFileUpdaterActivated**](/uwp/api/windows.ui.xaml.application.oncachedfileupdateractivated)  
[**OnFileActivated**](/uwp/api/windows.ui.xaml.application.onfileactivated)  
[**OnFileOpenPickerActivated**](/uwp/api/windows.ui.xaml.application.onfileopenpickeractivated)  [**OnFileSavePickerActivated**](/uwp/api/windows.ui.xaml.application.onfilesavepickeractivated)  
[**OnSearchActivated**](/uwp/api/windows.ui.xaml.application.onsearchactivated)  
[**OnShareTargetActivated**](/uwp/api/windows.ui.xaml.application.onsharetargetactivated)

Les données d’événement de ces méthodes incluent la même propriété  [**PreviousExecutionState**](/uwp/api/windows.applicationmodel.activation.iactivatedeventargs.previousexecutionstate) que nous avons illustrée ci-dessus, qui vous indique l’état de votre application avant qu’elle ait été activée. Interprétez cet état et ce que vous devez faire comme indiqué dans la section [Lancement d’une application](#app-launch).

**Remarque**   Si vous vous connectez à l’aide du compte d’administrateur de l’ordinateur, vous ne pouvez pas activer les applications UWP.

## <a name="running-in-the-background"></a>Exécution en arrière-plan ##

À partir de Windows 10, version 1607, les applications peuvent exécuter des tâches en arrière-plan dans le même processus que l’application elle-même. Pour en savoir plus, consultez [Activité en arrière-plan avec le modèle à processus unique](https://blogs.windows.com/buildingapps/2016/06/07/background-activity-with-the-single-process-model/#tMmI7wUuYu5CEeRm.99). Nous n’étudierons pas le traitement en arrière-plan intégré au processus dans cet article, mais nous allons examiner son impact sur le cycle de vie, avec les deux nouveaux événements qui se rapportent à votre application lorsqu’elle est en arrière-plan. Il s’agit de : [**EnteredBackground**](/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground) et [**LeavingBackground**](/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground).

Ces événements indiquent également si l’utilisateur peut voir l’interface utilisateur de votre application.

L’état Exécution en arrière-plan est l’état par défaut d’une application lancée, activée ou réactivée. Dans cet état, l’interface utilisateur de votre application n’est pas visible.

## <a name="running-in-the-foreground"></a>Exécution au premier plan ##

L’état Exécution au premier plan signifie que l’interface utilisateur de votre application est visible.

L’événement **LeavingBackground** est déclenché juste avant que l’interface utilisateur de votre application soit visible et avant d’adopter l’état Exécution au premier plan. Il se déclenche également lorsque l’utilisateur revient dans votre application.

Auparavant, le meilleur emplacement pour charger les éléments de l’interface utilisateur était le gestionnaire d’événements **Activated** ou **Resuming**. À présent, **LeavingBackground** est le meilleur endroit pour vérifier que votre interface utilisateur est prête.

Il est important de vérifier que les ressources visuelles sont prêtes à ce moment-là, car c’est la dernière occasion de faire le travail avant que votre application ne s’affiche à l’utilisateur. Le travail de l’interface utilisateur dans ce gestionnaire d’événements doit se terminer rapidement, car il conditionne le temps de lancement et de reprise constaté par l’utilisateur. Le temps de vérifier que la première image de l’interface utilisateur est prête correspond au paramètre **LeavingBackground**. Ensuite, les longs appels de réseau ou de stockage doivent être gérés de manière asynchrone afin que le gestionnaire d’événements puisse renvoyer des valeurs.

Lorsque l’utilisateur bascule hors de votre application, celle-ci reprend l’état Exécution en arrière-plan.

## <a name="reentering-the-background-state"></a>Retour à l’état en arrière-plan

L’événement **EnteredBackground** indique que votre application n’est plus visible au premier plan. Sur le bureau, **EnteredBackground** se déclenche lorsque votre application est réduite ; sur Windows Phone, c’est lorsque vous basculez vers l’écran de démarrage ou une autre application.

### <a name="reduce-your-apps-memory-usage"></a>Réduction de l’utilisation de la mémoire par votre application

Comme votre application n’est plus visible à l’utilisateur, le moment est idéal pour arrêter le travail de rendu de l’interface utilisateur et des animations. Vous pouvez utiliser **LeavingBackground** pour relancer ces opérations.

Si vous avez l’intention d’effectuer un travail en arrière-plan, c’est le moment de s’y préparer. Il est recommandé de vérifier [MemoryManager.AppMemoryUsageLevel](/uwp/api/windows.system.memorymanager.appmemoryusagelevel) et, si nécessaire, de réduire la quantité de mémoire utilisée par votre application en arrière-plan, afin que le système ne risque pas de l’arrêter pour libérer des ressources.

Pour plus d’informations, consultez [Libérer de la mémoire lorsque votre application passe à l’arrière-plan](reduce-memory-usage.md).

### <a name="save-your-state"></a>Enregistrement de votre état

Le gestionnaire d’événements de suspension est le meilleur emplacement pour enregistrer l’état de votre application. Toutefois, si vous travaillez en arrière-plan (par exemple, lecture audio, à l’aide d’une session d’exécution étendue ou d’une tâche en arrière-plan in-proc), il est également recommandé d’enregistrer vos données de façon asynchrone à partir de votre gestionnaire d’événements **EnteredBackground** . Cela est dû au fait qu’il est possible que votre application se termine alors qu’elle se trouve à une priorité plus faible en arrière-plan. Et comme elle ne sera pas passée par l’état Suspended en l’occurrence, vous perdrez vos données.

L’enregistrement de vos données dans votre gestionnaire d’événements **EnteredBackground** , avant le début de l’activité en arrière-plan, garantit une expérience utilisateur optimale lorsque l’utilisateur ramène votre application au premier plan. Vous pouvez utiliser les API de données d’application pour enregistrer les données et paramètres. Pour plus d’informations, consultez [Stocker et récupérer des paramètres et autres données d’application](../design/app-settings/store-and-retrieve-app-data.md).

Si, après avoir enregistré vos données, vous dépassez votre limite d’utilisation de mémoire, vous pouvez libérer vos données de la mémoire car vous pourrez les recharger ultérieurement. Ce faisant, vous libérez de la mémoire qui peut être utilisée par les ressources nécessaires à l’activité en arrière-plan.

Sachez que, si elle application exécute une activité en arrière-plan, votre application peut passer de l’état Exécution en arrière-plan à l’état Exécution au premier plan, sans passer par l’état Suspendu.

### <a name="asynchronous-work-and-deferrals"></a>Tâches asynchrones et reports

Si vous effectuez un appel asynchrone depuis votre gestionnaire, le contrôle renvoie immédiatement un retour de cet appel. Cela signifie que l’exécution peut ensuite revenir de votre gestionnaire d’événements et votre application prend l’état suivant, même si l’appel asynchrone n’est pas encore terminé. Utilisez la méthode [**GetDeferral**](/uwp/api/windows.applicationmodel.suspendingoperation.getdeferral) sur l’objet [**EnteredBackgroundEventArgs**](/uwp/api/Windows.ApplicationModel) qui est passé à votre gestionnaire d’événements pour retarder l’interruption jusqu’à ce que vous appeliez la méthode [**Complete**](/uwp/api/windows.foundation.deferral.complete) sur l’objet [**Windows. Foundation. Report**](/uwp/api/windows.foundation.deferral) retourné.

Un report n’augmente pas le temps d’exécution nécessaire de votre code avant l’arrêt de votre application. Cela ne retarde que l’arrêt jusqu'à ce que la méthode *Complete* soit appelée ou que la date d’échéance ne soit passée, *la première de ces deux éventualités prévalant*.

S’il vous faut davantage de temps pour enregistrer votre état, examinez les différentes solutions pour enregistrer votre état progressivement avant que l’application passe à l’état en arrière-plan afin que votre gestionnaire d’événements **EnteredBackground** ait moins de données à enregistrer. Vous pouvez également demander une session [ExtendedExecutionSession](/archive/msdn-magazine/2015/windows-10-special-issue/app-lifecycle-keep-apps-alive-with-background-tasks-and-extended-execution) pour obtenir plus de temps. Il n’y a aucune garantie que la demande soit accordée, mais il est préférable de trouver des solutions pour réduire le temps nécessaire à l’enregistrement de votre état.

## <a name="app-suspend"></a>Suspension d’une application

Lorsque l’utilisateur réduit une application, Windows patiente quelques secondes pour voir si l’utilisateur va rebasculer vers celle-ci. S’il n’y revient pas dans le délai imparti, et qu’aucune exécution étendue, tâche en arrière-plan ou exécution commanditée par l’activité n’est active, Windows suspend l’application. Une application est également suspendue lorsque l’écran de verrouillage reste affiché tant qu’aucune session d’exécution étendue, etc. n’est active dans cette application.

Quand une application est suspendue, elle appelle l’événement [**application. suspending**](/uwp/api/windows.ui.xaml.application.suspending) . Les modèles de projet UWP de Visual Studio fournissent un gestionnaire pour cet événement appelé **OnSuspending** dans **App.xaml.cs**. Avant Windows 10 version 1607, vous auriez placé le code pour enregistrer votre état ici. Aujourd’hui, il est recommandé d’enregistrer votre état lorsque l’application prend l’état d’arrière-plan, comme décrit ci-dessus.

Vous devez également libérer les ressources exclusives et les descripteurs de fichiers pour permettre aux autres applications d’y accéder lorsque votre application est suspendue. Appareils photo, périphériques d’E/S, appareils externes et ressources réseau sont autant d’exemples de ressources exclusives. En libérant explicitement les ressources exclusives et les descripteurs de fichiers, vous permettez aux autres applications d’y accéder lorsque votre application est suspendue. Lorsque l’application reprend, elle doit réacquérir ses ressources exclusives et ses descripteurs de fichiers.

### <a name="be-aware-of-the-deadline"></a>Gardez à l’esprit la date d’échéance

Pour que l’appareil soit rapide et réactif, vous devez exécuter votre code dans votre gestionnaire d’événements de suspension pendant une durée limitée. Cette durée varie selon l’appareil et vous constatez que ce dernier utilise une propriété de l’objet [**SuspendingOperation**](/uwp/api/Windows.ApplicationModel.SuspendingOperation) appelé date d’échéance.

Comme pour le gestionnaire d’événements **EnteredBackground**, si vous effectuez un appel asynchrone depuis votre gestionnaire, le contrôle renvoie immédiatement un retour de cet appel. Cela signifie que l’exécution peut ensuite renvoyer un retour de votre gestionnaire d’événements et votre application prend l’état Suspended, même si l’appel asynchrone n’est pas encore terminé. Utilisez la méthode [**GetDeferral**](/uwp/api/windows.applicationmodel.suspendingoperation.getdeferral) sur l’objet [**SuspendingOperation**](/uwp/api/Windows.ApplicationModel.SuspendingOperation) (disponible via les arguments de l’événement) pour retarder la suspension jusqu’à ce que vous appeliez la méthode [**Complete**](/uwp/api/windows.applicationmodel.suspendingdeferral.complete) sur l’objet [**SuspendingDeferral**](/uwp/api/Windows.ApplicationModel.SuspendingDeferral) renvoyé.

Pour obtenir plus de temps, vous pouvez demander une session [ExtendedExecutionSession](/archive/msdn-magazine/2015/windows-10-special-issue/app-lifecycle-keep-apps-alive-with-background-tasks-and-extended-execution). Il n’y a aucune garantie que la demande soit accordée, mais il est préférable de trouver des solutions pour réduire le temps nécessaire dans votre gestionnaire d’événements **Suspended**.

### <a name="app-terminate"></a>Arrêt d’une application

Le système tente de conserver votre application et ses données en mémoire pendant sa suspension. Toutefois, si le système ne dispose pas des ressources pour conserver votre application en mémoire, il arrête votre application. Les applications ne sont pas notifiées de leur arrêt. De ce fait, vous ne pouvez enregistrer les données de votre application que dans votre gestionnaire d’événements **OnSuspension** ou de manière asynchrone à partir de votre gestionnaire **EnteredBackground**.

Lorsque votre application détermine qu’elle a été activée après avoir été arrêtée, elle doit charger les données qu’elle avait enregistrées, afin qu’elle reprenne l’état qui était le sien avant son arrêt. Quand l’utilisateur revient à une application interrompue qui s’est arrêtée, l’application doit restaurer ses données d’application dans sa méthode [**OnLaunched**](/uwp/api/windows.ui.xaml.application.onlaunched) . Le système ne vous notifie pas de l’arrêt d’une application. Celle-ci doit donc enregistrer ses données d’application et libérer les ressources exclusives et descripteurs de fichiers avant d’être suspendue, pour ensuite les restaurer en cas de reprise après un arrêt.

**Remarque concernant le débogage à l’aide de Visual Studio :** Visual Studio empêche Windows de suspendre une application qui est jointe au débogueur. afin que l’utilisateur puisse voir l’interface de débogage de Visual Studio pendant l’exécution de l’application. Lorsque vous déboguez une application, vous pouvez lui envoyer un événement de suspension à l’aide de Visual Studio. Vérifiez que la barre d’outils **Emplacement de débogage** est visible et cliquez sur l’icône **Suspendre**.

## <a name="app-resume"></a>Reprise d’une application

Une application suspendue reprend son exécution quand l’utilisateur bascule vers celle-ci ou quand elle est active au moment où l’appareil quitte un état de faible consommation d’énergie.

Lorsqu’une application quitte l’état **Suspended**, elle prend l’état **Exécution en arrière-plan** et le système restaure l’application dans le dernier état qui était le sien, afin qu’elle s’affiche à l’utilisateur comme si elle avait continué de s’exécuter en continu. Aucune donnée d’application stockée en mémoire n’est perdue. Par conséquent, la plupart des applications n’ont pas besoin de restaurer leur état en cas de reprise. Mais elles doivent se réapproprier les descripteurs de fichier ou d’appareil libérés lors de leur suspension, et restaurer un état qui a été libéré explicitement lors de leur suspension.

Votre application peut être suspendue pendant plusieurs heures ou jours. Si votre application présente du contenu ou des connexions réseau caduques, il convient de les actualiser lors de la reprise. Si une application a inscrit le gestionnaire de l’événement [**Application.Resuming**](/uwp/api/windows.ui.xaml.application.resuming), ce gestionnaire est appelé lorsque l’application à l’état **Suspended** est réactivée. Vous pouvez actualiser le contenu et les données de votre application à l’aide de ce gestionnaire d’événements.

Si une application suspendue est activée pour participer à un contrat ou une extension d’application, elle reçoit d’abord l’événement **Resuming** puis l’événement **Activated**.

Si l’application suspendue a été arrêtée, il n’y a aucun événement **Resuming** et la méthode **OnLaunched()** est appelée avec un paramètre **ApplicationExecutionState** ayant pour valeur **Terminated**. Comme vous avez enregistré votre état lorsque l’application a été suspendue, vous pouvez le restaurer pendant la méthode **OnLaunched()** afin que votre application s’affiche à l’utilisateur comme elle était lorsque ce dernier a basculé vers une autre application.

Lorsqu’elle est suspendue, une application ne reçoit aucun événement réseau qu’elle est configurée pour recevoir. Ces événements réseau ne sont pas mis en file d’attente, mais simplement manqués. Par conséquent, votre application doit tester l’état du réseau lors de sa reprise.

**Remarque**    Étant donné que l’événement de [**reprise**](/uwp/api/windows.ui.xaml.application.resuming) n’est pas déclenché à partir du thread d’interface utilisateur, un répartiteur doit être utilisé si le code de votre gestionnaire de reprise communique avec votre interface utilisateur. Pour un exemple de code illustrant la marche à suivre, consultez [Mettre à jour le thread d’interface utilisateur à partir d’un thread d’arrière-plan](https://github.com/Microsoft/Windows-task-snippets/blob/master/tasks/UI-thread-access-from-background-thread.md).

Pour des consignes générales, consultez [Lancement, reprise et tâches en arrière-plan](./index.md).

## <a name="app-close"></a>Fermeture d’une application

En général, les utilisateurs n’ont pas besoin de fermer les applications et peuvent laisser Windows les gérer. Toutefois, ils peuvent décider de fermer une application en effectuant un mouvement de fermeture, en appuyant sur Alt+F4 ou en utilisant le sélecteur de tâche sur Windows Phone.

Aucun événement n’indique que l’utilisateur a fermé l’application. Lorsqu’elle est fermée par l’utilisateur, une application est d’abord suspendue pour lui donner l’occasion d’enregistrer son état. Sous Windows 8.1 et versions ultérieures, une fois fermée par l’utilisateur, une application est supprimée de l’écran et de la liste de répartition sans être arrêtée de manière explicite.

**Comportement fermé par l’utilisateur :**    Si votre application doit faire une autre chose lorsqu’elle est fermée par l’utilisateur que lorsqu’elle est fermée par Windows, vous pouvez utiliser le gestionnaire d’événements d’activation pour déterminer si l’application a été arrêtée par l’utilisateur ou par Windows. Voir les descriptions des états **ClosedByUser** et **Terminated** dans la documentation relative à l’énumération [**ApplicationExecutionState**](/uwp/api/Windows.ApplicationModel.Activation.ApplicationExecutionState).

Nous recommandons que les applications ne puissent se fermer par programme qu’en cas d’absolue nécessité. Par exemple, si une application détecte une fuite de mémoire, elle peut se fermer pour sécuriser les données personnelles de l’utilisateur.

## <a name="app-crash"></a>Blocage d’une application

La procédure en cas de blocage du système est conçue pour permettre aux utilisateurs de revenir à ce qu’ils étaient en train de faire, aussi rapidement que possible. Vous ne devez pas fournir de boîte de dialogue d’avertissement ou d’autres notifications, car celles-ci retarderont l’utilisateur.

Si votre application se bloque, cesse de répondre ou génère une exception, un rapport de problèmes est envoyé à Microsoft via les [paramètres de commentaires et diagnostics](https://support.microsoft.com/help/4468236/diagnostics-feedback-and-privacy-in-windows-10-microsoft-privacy) de l’utilisateur. Microsoft vous fournit un sous-ensemble des données d’erreur dans le rapport de problèmes pour que vous puissiez les utiliser afin d’améliorer votre application. Vous pouvez consulter ces données dans la page Qualité du tableau de bord.

Lorsque l’utilisateur active une application après une panne, son gestionnaire d’événements d’activation reçoit une valeur [**ApplicationExecutionState**](/uwp/api/Windows.ApplicationModel.Activation.ApplicationExecutionState) de **NotRunning** et doit afficher son interface utilisateur et ses données d’origine. Après une panne, n’utilisez pas de manière automatique les données d’application utilisées pour **Resuming** avec **Suspended**, car ces données peuvent être endommagées. Consultez [Recommandations en matière d’interruption et de reprise d’une application](./index.md).

## <a name="app-removal"></a>Suppression d’applications

Lorsqu’un utilisateur supprime votre application, elle est supprimée avec toutes ses données locales. La suppression d’une application n’affecte pas les données de l’utilisateur stockées dans des emplacements communs, telles les bibliothèques de documents ou d’images.

## <a name="app-lifecycle-and-the-visual-studio-project-templates"></a>Cycle de vie de l’application et modèles de projet Visual Studio

Le code de base approprié au cycle de vie de l’application est fourni dans les modèles de projet Visual Studio. L’application de base gère l’activation au lancement, fournit un emplacement pour restaurer vos données d’application et affiche l’interface utilisateur principale avant même que vous ayez ajouté votre propre code. Pour plus d’informations, consultez [Modèles de projet en C#, VB et C++ pour les applications du Windows Store](/previous-versions/windows/apps/hh768232(v=win.10)).

## <a name="key-application-lifecycle-apis"></a>Principales API du cycle de vie d’une application

-   Espace de noms [**Windows.ApplicationModel**](/uwp/api/Windows.ApplicationModel)
-   Espace de noms [**Windows. ApplicationModel. activation**](/uwp/api/Windows.ApplicationModel.Activation)
-   Espace de noms [**Windows. ApplicationModel. Core**](/uwp/api/Windows.ApplicationModel.Core)
-   [**Windows. UI. Xaml. application**](/uwp/api/Windows.UI.Xaml.Application) , classe (XAML)
-   Classe [**Windows.UI.Xaml.Window**](/uwp/api/Windows.UI.Xaml.Window) (XAML)

## <a name="related-topics"></a>Rubriques connexes

* [**ApplicationExecutionState**](/uwp/api/Windows.ApplicationModel.Activation.ApplicationExecutionState)
* [Recommandations pour la suspension et la reprise d’une application](./index.md)
* [Gérer le prélancement d’une application](handle-app-prelaunch.md)
* [Gérer l’activation d’une application](activate-an-app.md)
* [Gérer la suspension d’une application](suspend-an-app.md)
* [Gérer la reprise d’une application](resume-an-app.md)
* [Activité en arrière-plan avec le modèle à processus unique](https://blogs.windows.com/buildingapps/2016/06/07/background-activity-with-the-single-process-model/#tMmI7wUuYu5CEeRm.99)
* [Lire le média en arrière-plan](../audio-video-camera/background-audio.md)

 

 