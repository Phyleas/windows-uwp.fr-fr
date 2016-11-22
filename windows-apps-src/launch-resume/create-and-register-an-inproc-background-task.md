---
author: TylerMSFT
title: "Créer et inscrire une tâche en arrière-plan in-process"
description: "Créez et inscrivez une tâche in-process qui s’exécute dans le même processus que votre application au premier plan."
translationtype: Human Translation
ms.sourcegitcommit: d64527e36d995187936d4a0dbb94d973976d40ea
ms.openlocfilehash: f4bf682c27f856402ae8d1b5fd85bc998921efac

---

# Créer et inscrire une tâche en arrière-plan in-process

**API importantes**

-   [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781)

Cette rubrique explique comment créer et inscrire une tâche en arrière-plan qui s’exécute dans le même processus que votre application.

Les tâches en arrière-plan in-process sont plus faciles à implémenter que les tâches en arrière-plan hors processus. Toutefois, elles sont moins résilientes. Un blocage du code en cours d’exécution dans une tâche en arrière-plan in-process entraînera le blocage de votre application. Notez également que les déclencheurs [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) et **IoTStartupTask** ne peuvent pas être utilisés avec le modèle in-process. Vous ne pourrez pas non plus activer une tâche VoIP en arrière-plan au sein de votre application. Ces tâches et ces déclencheurs peuvent néanmoins être utilisés avec le modèle de tâche en arrière-plan hors processus.

N’oubliez pas que l’activité en arrière-plan peut être arrêtée, même en cas d’exécution au sein du processus au premier plan de l’application, si elle s’exécute au-delà des limites de durée d’exécution. La résilience obtenue grâce à la répartition des tâches dans une tâche en arrière-plan qui s’exécute dans un processus distinct reste utile pour plusieurs raisons. Dissocier la tâche en arrière-plan de l’application au premier plan peut être la meilleure option pour les tâches pour lesquelles aucune communication avec l’application au premier plan n’est requise.

## Principes de base

Le modèle in-process améliore le cycle de vie de l’application grâce à des notifications avancées qui vous indiquent quand votre application est au premier plan ou en arrière-plan. Deux nouveaux événements sont disponibles à partir de l’objet Application pour ces transitions: [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) et [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground). Ces événements s’intègrent au cycle de vie de l’application en fonction de l’état de visibilité de votre application. Pour en savoir plus sur ces événements et comment ils affectent le cycle de vie de l’application, consultez la rubrique [Cycle de vie de l’application](app-lifecycle.md).

En règle générale, vous gérerez l’événement **EnteredBackground** pour exécuter le code lorsque votre application s’exécute en arrière-plan, et l’événement **LeavingBackground** pour savoir quand votre application est passée au premier plan.

## Inscrire votre déclencheur de tâche en arrière-plan

L’inscription d’une activité en arrière-plan in-process est similaire à celle d’une activité en arrière-plan hors processus. Pour tous les déclencheurs en arrière-plan, l’inscription fait appel à l’élément [BackgroundTaskBuilder](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundtaskbuilder.aspx?f=255&MSPPError=-2147217396). Le générateur permet d’inscrire facilement une tâche en arrière-plan en définissant toutes les valeurs requises au même endroit:

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
> Les applications Windows universelles doivent appeler l’élément [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) avant d’inscrire n’importe quel type de déclencheur en arrière-plan.
> Pour vous assurer que votre application Windows universelle continue de s’exécuter correctement après la publication d’une mise à jour, vous devez appeler [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471), puis [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) lorsque votre application est lancée après avoir été mise à jour. Pour plus d’informations, voir [Recommandations en matière de tâches en arrière-plan](guidelines-for-background-tasks.md).

Pour les activités en arrière-plan in-process, vous ne définissez pas `TaskEntryPoint.`. En laissant ce champ vide, vous activez le point d’entrée par défaut, une nouvelle méthode protégée sur l’objet Application appelé [OnBackgroundActivated()](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx).

Une fois le déclencheur inscrit, il se déclenchera en fonction du type de déclencheur défini dans la méthode [SetTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundtaskbuilder.settrigger.aspx). L’exemple ci-dessus utilise un déclencheur [TimeTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.timetrigger.aspx) qui se déclenchera quinze minutes après son inscription.

## Ajouter une condition pour définir à quel moment votre tâche sera exécutée (facultatif)

Vous pouvez ajouter une condition pour définir à quel moment votre tâche sera exécutée après que l’événement de déclencheur est survenu. Par exemple, si vous ne souhaitez pas que la tâche s’exécute tant que l’utilisateur n’est pas présent, appliquez la condition **UserPresent**. Pour obtenir la liste des conditions possibles, voir [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835).

    The following sample code assigns a condition requiring the user to be present:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     builder.AddCondition(new SystemCondition(SystemConditionType.UserPresent));
    > ```

## Placer votre code d’activité en arrière-plan dans OnBackgroundActivated()

Placez votre code d’activité en arrière-plan dans [OnBackgroundActivated](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx)** pour répondre à votre déclencheur en arrière-plan au moment où il se déclenche. L’événement **OnBackgroundActivated** peut être traité de la même manière que [IBackgroundTask.Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396). La méthode a un paramètre [BackgroundActivatedEventArgs](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.activation.backgroundactivatedeventargs.aspx) qui regroupe tous les éléments fournis par la méthode Run.

## Gérer la progression et l’achèvement des tâches en arrière-plan

La progression et l’achèvement des tâches peuvent être surveillés de la même manière que pour les tâches en arrière-plan à plusieurs processus (voir [Surveiller la progression et l’achèvement des tâches en arrière-plan](monitor-background-task-progress-and-completion.md)), mais vous découvrirez sans doute un moyen encore plus facile de le faire en utilisant des variables pour suivre l’état d’avancement ou d’achèvement dans votre application. C’est l’un des avantages d’exécuter votre code d’activité en arrière-plan dans le même processus que votre application.

## Gérer l’annulation des tâches en arrière-plan

Le processus d’annulation des tâches en arrière-plan in-process est le même que pour les tâches en arrière-plan hors processus (voir [Gérer une tâche en arrière-plan annulée](handle-a-cancelled-background-task.md)). Notez que votre gestionnaire d’événements **BackgroundActivated** doit être fermé avant l’annulation pour éviter l’arrêt de tout le processus. Si votre application au premier plan se ferme de façon inattendue lorsque vous annulez la tâche en arrière-plan, assurez-vous que votre gestionnaire s’est fermé avant l’annulation.

## Le manifeste

Contrairement aux tâches en arrière-plan hors processus, vous n’avez pas besoin d’ajouter des informations sur la tâche en arrière-plan dans le manifeste du package pour exécuter les tâches en arrière-plan in-process.

## Récapitulatif et étapes suivantes

Vous devriez maintenant connaître les principes de l’écriture d’une tâche en arrière-plan in-process.

Consultez les rubriques connexes suivantes pour obtenir des informations de référence sur les API, des recommandations conceptuelles pour les tâches en arrière-plan, ainsi que des instructions plus détaillées pour écrire des applications qui utilisent des tâches en arrière-plan.

## Rubriques connexes

**Rubriques d’instructions détaillées sur les tâches en arrière-plan**

* [Convertir une tâche en arrière-plan hors processus en une tâche en arrière-plan in-process](convert-out-of-process-background-task.md)
* [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-an-outofproc-background-task.md)
* [Lire du contenu multimédia en arrière-plan](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio)
* [Répondre aux événements système avec des tâches en arrière-plan](respond-to-system-events-with-background-tasks.md)
* [Inscrire une tâche en arrière-plan](register-a-background-task.md)
* [Définir des conditions pour exécuter une tâche en arrière-plan](set-conditions-for-running-a-background-task.md)
* [Utiliser un déclencheur de maintenance](use-a-maintenance-trigger.md)
* [Gérer une tâche en arrière-plan annulée](handle-a-cancelled-background-task.md)
* [Surveiller la progression et l’achèvement des tâches en arrière-plan](monitor-background-task-progress-and-completion.md)
* [Exécuter une tâche en arrière-plan en fonction d’un minuteur](run-a-background-task-on-a-timer-.md)

**Recommandations en matière de tâches en arrière-plan**

* [Recommandations pour les tâches en arrière-plan](guidelines-for-background-tasks.md)
* [Déboguer une tâche en arrière-plan](debug-a-background-task.md)
* [Comment déclencher des événements de suspension, des événements de reprise et des événements en arrière-plan dans des applications du Windows Store (lors du débogage)](http://go.microsoft.com/fwlink/p/?linkid=254345)

**Informations de référence d’API de tâche en arrière-plan**

* [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847)



<!--HONumber=Nov16_HO1-->

