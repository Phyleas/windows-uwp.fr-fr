---
title: Créer et inscrire une tâche en arrière-plan in-process
description: Créez et inscrivez une tâche in-process qui s’exécute dans le même processus que votre application au premier plan.
ms.date: 11/03/2017
ms.topic: article
keywords: Windows 10, UWP, tâche en arrière-plan
ms.assetid: d99de93b-e33b-45a9-b19f-31417f1e9354
ms.localizationpriority: medium
ms.openlocfilehash: 489de52a3c592bac9d715b679470b84c2af7e621
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155993"
---
# <a name="create-and-register-an-in-process-background-task"></a>Créer et inscrire une tâche en arrière-plan in-process

**API importantes**

-   [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)
-   [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)
-   [**BackgroundTaskCompletedEventHandler**](/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler)

Cette rubrique explique comment créer et inscrire une tâche en arrière-plan qui s’exécute dans le même processus que votre application.

Les tâches en arrière-plan in-process sont plus faciles à implémenter que les tâches en arrière-plan hors processus. Toutefois, elles sont moins résilientes. Un blocage du code en cours d’exécution dans une tâche en arrière-plan in-process entraînera le blocage de votre application. Notez également que les déclencheurs [DeviceUseTrigger](/uwp/api/windows.applicationmodel.background.deviceusetrigger), [DeviceServicingTrigger](/uwp/api/windows.applicationmodel.background.deviceservicingtrigger) et **IoTStartupTask** ne peuvent pas être utilisés avec le modèle in-process. Vous ne pourrez pas non plus activer une tâche VoIP en arrière-plan au sein de votre application. Ces tâches et ces déclencheurs peuvent néanmoins être utilisés avec le modèle de tâche en arrière-plan hors processus.

N’oubliez pas que l’activité en arrière-plan peut être arrêtée, même en cas d’exécution au sein du processus au premier plan de l’application, si elle s’exécute au-delà des limites de durée d’exécution. La résilience obtenue grâce à la répartition des tâches dans une tâche en arrière-plan qui s’exécute dans un processus distinct reste utile pour plusieurs raisons. Dissocier la tâche en arrière-plan de l’application au premier plan peut être la meilleure option pour les tâches pour lesquelles aucune communication avec l’application au premier plan n’est requise.

## <a name="fundamentals"></a>Notions de base

Le modèle in-process améliore le cycle de vie de l’application grâce à des notifications avancées qui vous indiquent quand votre application est au premier plan ou en arrière-plan. Deux nouveaux événements sont disponibles à partir de l’objet application pour ces transitions : [**EnteredBackground**](/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground) et [**LeavingBackground**](/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground). Ces événements s’intègrent au cycle de vie de l’application en fonction de l’état de visibilité de votre application. Pour en savoir plus sur ces événements et comment ils affectent le cycle de vie de l’application, consultez la rubrique [Cycle de vie de l’application](app-lifecycle.md).

En règle générale, vous gérerez l’événement **EnteredBackground** pour exécuter le code lorsque votre application s’exécute en arrière-plan, et l’événement **LeavingBackground** pour savoir quand votre application est passée au premier plan.

## <a name="register-your-background-task-trigger"></a>Inscrire votre déclencheur de tâche en arrière-plan

L’inscription d’une activité en arrière-plan in-process est similaire à celle d’une activité en arrière-plan hors processus. Pour tous les déclencheurs en arrière-plan, l’inscription fait appel à l’élément [BackgroundTaskBuilder](/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder?f=255&MSPPError=-2147217396). Le générateur permet d’inscrire facilement une tâche en arrière-plan en définissant toutes les valeurs requises au même endroit :

> [!div class="tabbedCodeSnippets"]
> ```cs
> var builder = new BackgroundTaskBuilder();
> builder.Name = "My Background Trigger";
> builder.SetTrigger(new TimeTrigger(15, true));
> // Do not set builder.TaskEntryPoint for in-process background tasks
> // Here we register the task and work will start based on the time trigger.
> BackgroundTaskRegistration task = builder.Register();
> ```

> [!NOTE]
> Les applications Windows universelles doivent appeler [**RequestAccessAsync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) avant d’inscrire l’un des types de déclencheurs d’arrière-plan.
> Pour vous assurer que votre application Windows universelle continue de s’exécuter correctement après la publication d’une mise à jour, vous devez appeler [**RemoveAccess**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.removeaccess), puis [**RequestAccessAsync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) lorsque votre application est lancée après avoir été mise à jour. Pour plus d’informations, voir [Recommandations en matière de tâches en arrière-plan](guidelines-for-background-tasks.md).

Pour les activités en arrière-plan in-process, vous ne définissez pas `TaskEntryPoint.`. En laissant ce champ vide, vous activez le point d’entrée par défaut, une nouvelle méthode protégée sur l’objet Application appelé [OnBackgroundActivated()](/uwp/api/windows.ui.xaml.application.onbackgroundactivated).

Une fois le déclencheur inscrit, il se déclenchera en fonction du type de déclencheur défini dans la méthode [SetTrigger](/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.settrigger). L’exemple ci-dessus utilise un déclencheur [TimeTrigger](/uwp/api/windows.applicationmodel.background.timetrigger) qui se déclenchera quinze minutes après son inscription.

## <a name="add-a-condition-to-control-when-your-task-will-run-optional"></a>Ajouter une condition pour définir à quel moment votre tâche sera exécutée (facultatif)

Vous pouvez ajouter une condition pour définir à quel moment votre tâche sera exécutée après que l’événement de déclencheur est survenu. Par exemple, si vous ne souhaitez pas que la tâche s’exécute tant que l’utilisateur n’est pas présent, appliquez la condition **UserPresent**. Pour obtenir la liste des conditions possibles, consultez [**SystemConditionType**](/uwp/api/Windows.ApplicationModel.Background.SystemConditionType).

L’exemple de code suivant affecte une condition qui exige la présence de l’utilisateur :

> [!div class="tabbedCodeSnippets"]
> ```cs
> builder.AddCondition(new SystemCondition(SystemConditionType.UserPresent));
> ```

## <a name="place-your-background-activity-code-in-onbackgroundactivated"></a>Placer votre code d’activité en arrière-plan dans OnBackgroundActivated()

Placez votre code d’activité en arrière-plan dans [OnBackgroundActivated](/uwp/api/windows.ui.xaml.application.onbackgroundactivated) pour répondre à votre déclencheur d’arrière-plan lorsqu’il est déclenché. **OnBackgroundActivated** peut être traité comme [IBackgroundTask. Run](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run?f=255&MSPPError=-2147217396). La méthode a un paramètre [BackgroundActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.backgroundactivatedeventargs) , qui contient toutes les caractéristiques de la méthode **Run** . Par exemple, dans App.xaml.cs :

``` cs
using Windows.ApplicationModel.Background;

...

sealed partial class App : Application
{
  ...

  protected override void OnBackgroundActivated(BackgroundActivatedEventArgs args)
  {
      base.OnBackgroundActivated(args);
      IBackgroundTaskInstance taskInstance = args.TaskInstance;
      DoYourBackgroundWork(taskInstance);  
  }
}
```

Pour obtenir un exemple **OnBackgroundActivated** plus riche, consultez [convertir un app service pour qu’il s’exécute dans le même processus que son application hôte](convert-app-service-in-process.md).

## <a name="handle-background-task-progress-and-completion"></a>Gérer la progression et l’achèvement des tâches en arrière-plan

La progression et l’achèvement des tâches peuvent être surveillés de la même manière que pour les tâches en arrière-plan à plusieurs processus (voir [Surveiller la progression et l’achèvement des tâches en arrière-plan](monitor-background-task-progress-and-completion.md)), mais vous découvrirez sans doute un moyen encore plus facile de le faire en utilisant des variables pour suivre l’état d’avancement ou d’achèvement dans votre application. C’est l’un des avantages d’exécuter votre code d’activité en arrière-plan dans le même processus que votre application.

## <a name="handle-background-task-cancellation"></a>Gérer l’annulation des tâches en arrière-plan

Le processus d’annulation des tâches en arrière-plan in-process est le même que pour les tâches en arrière-plan hors processus (voir [Gérer une tâche en arrière-plan annulée](handle-a-cancelled-background-task.md)). Notez que votre gestionnaire d’événements **BackgroundActivated** doit être fermé avant l’annulation pour éviter l’arrêt de tout le processus. Si votre application au premier plan se ferme de façon inattendue lorsque vous annulez la tâche en arrière-plan, assurez-vous que votre gestionnaire s’est fermé avant l’annulation.

## <a name="the-manifest"></a>Le manifeste

Contrairement aux tâches en arrière-plan hors processus, vous n’avez pas besoin d’ajouter des informations sur la tâche en arrière-plan dans le manifeste du package pour exécuter les tâches en arrière-plan in-process.

## <a name="summary-and-next-steps"></a>Résumé et étapes suivantes

Vous devriez maintenant connaître les principes de l’écriture d’une tâche en arrière-plan in-process.

Consultez les rubriques connexes suivantes pour obtenir des informations de référence sur les API, des recommandations conceptuelles pour les tâches en arrière-plan, ainsi que des instructions plus détaillées pour écrire des applications qui utilisent des tâches en arrière-plan.

## <a name="related-topics"></a>Rubriques connexes

**Rubriques d’instructions détaillées sur les tâches en arrière-plan**

* [Convertir une tâche en arrière-plan hors processus en une tâche en arrière-plan in-process](convert-out-of-process-background-task.md)
* [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-a-background-task.md)
* [Lire du contenu multimédia en arrière-plan](../audio-video-camera/background-audio.md)
* [Répondre aux événements système avec des tâches en arrière-plan](respond-to-system-events-with-background-tasks.md)
* [Inscrire une tâche en arrière-plan](register-a-background-task.md)
* [Définir des conditions pour l’exécution d’une tâche en arrière-plan](set-conditions-for-running-a-background-task.md)
* [Utiliser un déclencheur de maintenance](use-a-maintenance-trigger.md)
* [Gérer une tâche en arrière-plan annulée](handle-a-cancelled-background-task.md)
* [Surveiller la progression et l’achèvement des tâches en arrière-plan](monitor-background-task-progress-and-completion.md)
* [Exécuter une tâche en arrière-plan en fonction d’un minuteur](run-a-background-task-on-a-timer-.md)

**Recommandations en matière de tâches en arrière-plan**

* [Recommandations relatives aux tâches en arrière-plan](guidelines-for-background-tasks.md)
* [Déboguer une tâche en arrière-plan](debug-a-background-task.md)
* [Comment déclencher des événements de suspension, de reprise et d’arrière-plan dans des applications UWP (lors du débogage)](/previous-versions/hh974425(v=vs.110))

**Informations de référence d’API de tâche en arrière-plan**

* [**Windows.ApplicationModel.Background**](/uwp/api/Windows.ApplicationModel.Background)