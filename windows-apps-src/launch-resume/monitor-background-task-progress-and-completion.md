---
title: Surveiller la progression et l’achèvement des tâches en arrière-plan
description: Découvrez comment votre application peut reconnaître la progression et l’achèvement signalés par une tâche en arrière-plan.
ms.assetid: 17544FD7-A336-4254-97DC-2BF8994FF9B2
ms.date: 07/06/2018
ms.topic: article
keywords: Windows 10, UWP, tâche en arrière-plan
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 9c6fb40d64d0826a5cbbfa7c97694e6650df9dbe
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172973"
---
# <a name="monitor-background-task-progress-and-completion"></a>Surveiller la progression et l’achèvement des tâches en arrière-plan

**API importantes**

- [**BackgroundTaskRegistration**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration)
- [**BackgroundTaskProgressEventHandler**](/uwp/api/windows.applicationmodel.background.backgroundtaskprogresseventhandler)
- [**BackgroundTaskCompletedEventHandler**](/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler)

Découvrez comment votre application peut reconnaître la progression et l’achèvement signalés par une tâche en arrière-plan qui s’exécute hors processus. (Pour les tâches en arrière-plan in-process, vous pouvez définir des variables partagées pour signaler la progression et l’achèvement.)

La progression et l’achèvement des tâches en arrière-plan peuvent être surveillés par le code de l’application. Pour ce faire, l’application s’abonne aux événements des tâches en arrière-plan qu’elle a inscrites auprès du système.

- Cette rubrique suppose que vous disposez d’une application qui inscrit les tâches en arrière-plan. Pour commencer rapidement à créer une tâche en arrière-plan, consultez [Créer et inscrire une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md) ou [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-a-background-task.md). Pour des informations plus détaillées sur les conditions et les déclencheurs, voir [Définition de tâches en arrière-plan pour les besoins de votre application](support-your-app-with-background-tasks.md).

## <a name="create-an-event-handler-to-handle-completed-background-tasks"></a>Créer un gestionnaire d’événements pour gérer les tâches en arrière-plan achevées

### <a name="step-1"></a>Étape 1
Créez une fonction de gestionnaire des événements pour gérer les tâches en arrière-plan achevées. Ce code doit suivre un encombrement spécifique, qui prend un objet [**IBackgroundTaskRegistration**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskRegistration) et un objet [**BackgroundTaskCompletedEventArgs**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskCompletedEventArgs) .

Utilisez l’empreinte suivante pour la méthode du gestionnaire d’événements de la tâche d’arrière-plan **OnCompleted** .

```csharp
private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
{
    // TODO: Add code that deals with background task completion.
}
```

```cppwinrt
auto completed{ [this](
        Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
        Windows::ApplicationModel::Background::BackgroundTaskCompletedEventArgs const& /* args */)
{
    // TODO: Add code that deals with background task completion.
} };
```

```cpp
auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
{
    // TODO: Add code that deals with background task completion.
};
```

### <a name="step-2"></a>Étape 2
Ajoutez du code au gestionnaire des événements qui traite l’achèvement des tâches en arrière-plan.

Par exemple, l’[exemple de tâche en arrière-plan](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask) met à jour l’interface utilisateur.

```csharp
private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
{
    UpdateUI();
}
```

```cppwinrt
auto completed{ [this](
        Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
        Windows::ApplicationModel::Background::BackgroundTaskCompletedEventArgs const& /* args */)
{
    UpdateUI();
} };
```

```cpp
auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
{    
    UpdateUI();
};
```

## <a name="create-an-event-handler-function-to-handle-background-task-progress"></a>Créer une fonction de gestionnaire des événements pour gérer la progression des tâches en arrière-plan

### <a name="step-1"></a>Étape 1
Créez une fonction de gestionnaire des événements pour gérer les tâches en arrière-plan achevées. Ce code doit suivre un encombrement spécifique, qui prend un objet [**IBackgroundTaskRegistration**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskRegistration) et un objet [**BackgroundTaskProgressEventArgs**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskProgressEventArgs) :

Utilisez l’empreinte suivante pour la méthode de gestionnaire des événements de tâche en arrière-plan OnProgress :

```csharp
private void OnProgress(IBackgroundTaskRegistration task, BackgroundTaskProgressEventArgs args)
{
    // TODO: Add code that deals with background task progress.
}
```

```cppwinrt
auto progress{ [this](
    Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
    Windows::ApplicationModel::Background::BackgroundTaskProgressEventArgs const& /* args */)
{
    // TODO: Add code that deals with background task progress.
} };
```

```cpp
auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
{
    // TODO: Add code that deals with background task progress.
};
```

### <a name="step-2"></a>Étape 2
Ajoutez du code au gestionnaire des événements qui traite l’achèvement des tâches en arrière-plan.

Ainsi, l’[exemple de tâche en arrière-plan](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask) met à jour l’interface utilisateur conformément à l’état de progression transmis avec le paramètre *args* :

```csharp
private void OnProgress(IBackgroundTaskRegistration task, BackgroundTaskProgressEventArgs args)
{
    var progress = "Progress: " + args.Progress + "%";
    BackgroundTaskSample.SampleBackgroundTaskProgress = progress;
    UpdateUI();
}
```

```cppwinrt
auto progress{ [this](
    Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
    Windows::ApplicationModel::Background::BackgroundTaskProgressEventArgs const& args)
{
    winrt::hstring progress{ L"Progress: " + winrt::to_hstring(args.Progress()) + L"%" };
    BackgroundTaskSample::SampleBackgroundTaskProgress = progress;
    UpdateUI();
} };
```

```cpp
auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
{
    auto progress = "Progress: " + args->Progress + "%";
    BackgroundTaskSample::SampleBackgroundTaskProgress = progress;
    UpdateUI();
};
```

## <a name="register-the-event-handler-functions-with-new-and-existing-background-tasks"></a>Inscrire les fonctions de gestionnaire d’événements auprès des tâches en arrière-plan nouvelles et existantes

### <a name="step-1"></a>Étape 1
Lorsque l’application inscrit une tâche en arrière-plan pour la première fois, elle doit s’inscrire pour recevoir les mises à jour de progression et d’achèvement de la tâche au cas où celle-ci s’exécuterait pendant que l’application est toujours active au premier plan.

Ainsi, l’[exemple de tâche en arrière-plan](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask) appelle la fonction suivante pour chaque tâche en arrière-plan qu’il inscrit :

```csharp
private void AttachProgressAndCompletedHandlers(IBackgroundTaskRegistration task)
{
    task.Progress += new BackgroundTaskProgressEventHandler(OnProgress);
    task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
}
```

```cppwinrt
void SampleBackgroundTask::AttachProgressAndCompletedHandlers(Windows::ApplicationModel::Background::IBackgroundTaskRegistration const& task)
{
    auto progress{ [this](
        Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
        Windows::ApplicationModel::Background::BackgroundTaskProgressEventArgs const& args)
    {
        winrt::hstring progress{ L"Progress: " + winrt::to_hstring(args.Progress()) + L"%" };
        BackgroundTaskSample::SampleBackgroundTaskProgress = progress;
        UpdateUI();
    } };

    task.Progress(progress);

    auto completed{ [this](
        Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
        Windows::ApplicationModel::Background::BackgroundTaskCompletedEventArgs const& /* args */)
    {
        UpdateUI();
    } };

    task.Completed(completed);
}
```

```cpp
void SampleBackgroundTask::AttachProgressAndCompletedHandlers(IBackgroundTaskRegistration^ task)
{
    auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
    {
        auto progress = "Progress: " + args->Progress + "%";
        BackgroundTaskSample::SampleBackgroundTaskProgress = progress;
        UpdateUI();
    };

    task->Progress += ref new BackgroundTaskProgressEventHandler(progress);

    auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
    {
        UpdateUI();
    };

    task->Completed += ref new BackgroundTaskCompletedEventHandler(completed);
}
```

### <a name="step-2"></a>Étape 2
Lorsque l’application se lance ou accède à une nouvelle page dans laquelle l’état des tâches en arrière-plan est important, elle doit obtenir la liste des tâches en arrière-plan actuellement inscrites et les associer aux fonctions de gestionnaire des événements de progression et d’achèvement. Cette liste est conservée dans la propriété [**BackgroundTaskRegistration**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration).[**AllTasks**](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.alltasks).

Ainsi, l’[exemple de tâche en arrière-plan](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask) utilise le code suivant pour joindre les gestionnaires d’événements lorsque vous accédez à la page SampleBackgroundTask :

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    foreach (var task in BackgroundTaskRegistration.AllTasks)
    {
        if (task.Value.Name == BackgroundTaskSample.SampleBackgroundTaskName)
        {
            AttachProgressAndCompletedHandlers(task.Value);
            BackgroundTaskSample.UpdateBackgroundTaskStatus(BackgroundTaskSample.SampleBackgroundTaskName, true);
        }
    }

    UpdateUI();
}
```

```cppwinrt
void SampleBackgroundTask::OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& /* e */)
{
    // A pointer back to the main page. This is needed if you want to call methods in MainPage such
    // as NotifyUser().
    m_rootPage = MainPage::Current;

    // Attach progress and completed handlers to any existing tasks.
    auto allTasks{ Windows::ApplicationModel::Background::BackgroundTaskRegistration::AllTasks() };

    for (auto const& task : allTasks)
    {
        if (task.Value().Name() == SampleBackgroundTaskName)
        {
            AttachProgressAndCompletedHandlers(task.Value());
            break;
        }
    }

    UpdateUI();
}
```

```cpp
void SampleBackgroundTask::OnNavigatedTo(NavigationEventArgs^ e)
{
    // A pointer back to the main page.  This is needed if you want to call methods in MainPage such
    // as NotifyUser().
    rootPage = MainPage::Current;

    // Attach progress and completed handlers to any existing tasks.
    auto iter = BackgroundTaskRegistration::AllTasks->First();
    auto hascur = iter->HasCurrent;
    while (hascur)
    {
        auto cur = iter->Current->Value;

        if (cur->Name == SampleBackgroundTaskName)
        {
            AttachProgressAndCompletedHandlers(cur);
            break;
        }

        hascur = iter->MoveNext();
    }

    UpdateUI();
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Créer et inscrire une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md).
* [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-a-background-task.md)
* [Déclarer des tâches en arrière-plan dans le manifeste de l’application](declare-background-tasks-in-the-application-manifest.md)
* [Gérer une tâche en arrière-plan annulée](handle-a-cancelled-background-task.md)
* [Inscrire une tâche en arrière-plan](register-a-background-task.md)
* [Répondre aux événements système avec des tâches en arrière-plan](respond-to-system-events-with-background-tasks.md)
* [Définir des conditions pour l’exécution d’une tâche en arrière-plan](set-conditions-for-running-a-background-task.md)
* [Mettre à jour une vignette dynamique à partir d’une tâche en arrière-plan](update-a-live-tile-from-a-background-task.md)
* [Utiliser un déclencheur de maintenance](use-a-maintenance-trigger.md)
* [Exécuter une tâche en arrière-plan en fonction d’un minuteur](run-a-background-task-on-a-timer-.md)
* [Recommandations relatives aux tâches en arrière-plan](guidelines-for-background-tasks.md)
* [Déboguer une tâche en arrière-plan](debug-a-background-task.md)
* [Comment déclencher des événements de suspension, de reprise et d’arrière-plan dans des applications UWP (lors du débogage)](/previous-versions/hh974425(v=vs.110))