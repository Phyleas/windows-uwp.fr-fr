---
title: Créer et inscrire une tâche en arrière-plan hors processus
description: Créez une classe de tâche en arrière-plan hors processus et inscrivez-la pour permettre son exécution lorsque votre application ne se trouve pas au premier plan.
ms.assetid: 4F98F6A3-0D3D-4EFB-BA8E-30ED37AE098B
ms.date: 02/27/2019
ms.topic: article
keywords: Windows 10, UWP, tâche en arrière-plan
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 638d4e8b4d4bc074566c8b0a6c24d733a5769b32
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164893"
---
# <a name="create-and-register-an-out-of-process-background-task"></a>Créer et inscrire une tâche en arrière-plan hors processus

**API importantes**

-   [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)
-   [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)
-   [**BackgroundTaskCompletedEventHandler**](/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler)

Créez une classe de tâche en arrière-plan et inscrivez-la pour permettre son exécution lorsque votre application ne se trouve pas au premier plan. Cette rubrique explique comment créer et inscrire une tâche en arrière-plan qui s’exécute dans un processus distinct du processus de votre application. Pour exécuter une tâche en arrière-plan directement dans l’application au premier plan, consultez [Créer et inscrire une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md).

> [!NOTE]
> Si vous utilisez une tâche en arrière-plan pour lire du contenu multimédia en arrière-plan, consultez [Lire du contenu multimédia en arrière-plan](../audio-video-camera/background-audio.md). Les informations fournies sur les améliorations apportées dans Windows 10 version 1607 vous simplifieront la vie.

## <a name="create-the-background-task-class"></a>Créer la classe de tâche en arrière-plan

Vous pouvez exécuter du code en arrière-plan en écrivant des classes qui implémentent l’interface [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask). Ce code s’exécute lorsqu’un événement spécifique est déclenché à l’aide de, par exemple, [**événement systemtrigger**](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) ou [**MaintenanceTrigger**](/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger).

Les étapes suivantes vous montrent comment écrire une nouvelle classe qui implémente l’interface [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask).

1.  Créez un projet pour les tâches en arrière-plan et ajoutez-le à votre solution. Pour ce faire, cliquez avec le bouton droit sur le nœud de votre solution dans le **Explorateur de solutions** puis sélectionnez **Ajouter** \> **un nouveau projet**. Sélectionnez ensuite le type de projet **composant Windows Runtime** , nommez le projet, puis cliquez sur OK.
2.  Référencez le projet des tâches en arrière-plan à partir de votre projet d’application de plateforme Windows universelle (UWP). Pour une application C# ou C++, dans votre projet d’application, cliquez avec le bouton droit sur **références** , puis sélectionnez **Ajouter une nouvelle référence**. Sous **Solution**, sélectionnez **Projets** et le nom de votre projet de tâches en arrière-plan, puis cliquez sur **OK**.
3.  Pour le projet tâches en arrière-plan, ajoutez une nouvelle classe qui implémente l’interface [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) . La méthode [**IBackgroundTask. Run**](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) est un point d’entrée obligatoire qui est appelé lorsque l’événement spécifié est déclenché. Cette méthode est requise dans chaque tâche en arrière-plan.

> [!NOTE]
> La classe de tâche en arrière-plan elle-même &mdash; et toutes les autres classes dans le projet de tâche en arrière-plan &mdash; doivent être des classes **publiques** qui sont **sealed** (ou **final**).

L’exemple de code suivant montre un point de départ très basique pour une classe de tâche en arrière-plan.

```csharp
// ExampleBackgroundTask.cs
using Windows.ApplicationModel.Background;

namespace Tasks
{
    public sealed class ExampleBackgroundTask : IBackgroundTask
    {
        public void Run(IBackgroundTaskInstance taskInstance)
        {
            
        }        
    }
}
```

```cppwinrt
// First, add ExampleBackgroundTask.idl, and then build.
// ExampleBackgroundTask.idl
namespace Tasks
{
    [default_interface]
    runtimeclass ExampleBackgroundTask : Windows.ApplicationModel.Background.IBackgroundTask
    {
        ExampleBackgroundTask();
    }
}

// ExampleBackgroundTask.h
#pragma once

#include "ExampleBackgroundTask.g.h"

namespace winrt::Tasks::implementation
{
    struct ExampleBackgroundTask : ExampleBackgroundTaskT<ExampleBackgroundTask>
    {
        ExampleBackgroundTask() = default;

        void Run(Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance);
    };
}

namespace winrt::Tasks::factory_implementation
{
    struct ExampleBackgroundTask : ExampleBackgroundTaskT<ExampleBackgroundTask, implementation::ExampleBackgroundTask>
    {
    };
}

// ExampleBackgroundTask.cpp
#include "pch.h"
#include "ExampleBackgroundTask.h"

namespace winrt::Tasks::implementation
{
    void ExampleBackgroundTask::Run(Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance)
    {
        throw hresult_not_implemented();
    }
}
```

```cpp
// ExampleBackgroundTask.h
#pragma once

using namespace Windows::ApplicationModel::Background;

namespace Tasks
{
    public ref class ExampleBackgroundTask sealed : public IBackgroundTask
    {

    public:
        ExampleBackgroundTask();

        virtual void Run(IBackgroundTaskInstance^ taskInstance);
        void OnCompleted(
            BackgroundTaskRegistration^ task,
            BackgroundTaskCompletedEventArgs^ args
        );
    };
}

// ExampleBackgroundTask.cpp
#include "ExampleBackgroundTask.h"

using namespace Tasks;

void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
{
}
```

4.  Si vous exécutez du code asynchrone dans votre tâche en arrière-plan, celle-ci doit alors utiliser un report, Si vous n’utilisez pas de report, le processus de tâche en arrière-plan peut se terminer de façon inattendue si la méthode **Run** retourne une valeur avant l’exécution de tout travail asynchrone.

Demandez le report dans la méthode **Run** avant d’appeler la méthode asynchrone. Enregistrez le report dans un membre de données de classe afin qu’il soit accessible à partir de la méthode asynchrone. Déclarez le report terminé après que l’exécution du code asynchrone a abouti.

L’exemple de code suivant permet d’obtenir le report, de l’enregistrer et de le libérer lorsque le code asynchrone est terminé.

```csharp
BackgroundTaskDeferral _deferral; // Note: defined at class scope so that we can mark it complete inside the OnCancel() callback if we choose to support cancellation
public async void Run(IBackgroundTaskInstance taskInstance)
{
    _deferral = taskInstance.GetDeferral();
    //
    // TODO: Insert code to start one or more asynchronous methods using the
    //       await keyword, for example:
    //
    // await ExampleMethodAsync();
    //

    _deferral.Complete();
}
```

```cppwinrt
// ExampleBackgroundTask.h
...
private:
    Windows::ApplicationModel::Background::BackgroundTaskDeferral m_deferral{ nullptr };

// ExampleBackgroundTask.cpp
...
Windows::Foundation::IAsyncAction ExampleBackgroundTask::Run(
    Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance)
{
    m_deferral = taskInstance.GetDeferral(); // Note: defined at class scope so that we can mark it complete inside the OnCancel() callback if we choose to support cancellation.
    // TODO: Modify the following line of code to call a real async function.
    co_await ExampleCoroutineAsync(); // Run returns at this point, and resumes when ExampleCoroutineAsync completes.
    m_deferral.Complete();
}
```

```cpp
void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
{
    m_deferral = taskInstance->GetDeferral(); // Note: defined at class scope so that we can mark it complete inside the OnCancel() callback if we choose to support cancellation.

    //
    // TODO: Modify the following line of code to call a real async function.
    //       Note that the task<void> return type applies only to async
    //       actions. If you need to call an async operation instead, replace
    //       task<void> with the correct return type.
    //
    task<void> myTask(ExampleFunctionAsync());

    myTask.then([=]() {
        m_deferral->Complete();
    });
}
```

> [!NOTE]
> En C#, les méthodes asynchrones de vos tâches en arrière-plan peuvent être appelées à l’aide des mots clés **async/await**. En C++/CX, un résultat similaire peut être obtenu à l’aide d’une chaîne de tâches.

Pour plus d’informations sur les modèles asynchrones, voir [Programmation asynchrone](../threading-async/asynchronous-programming-universal-windows-platform-apps.md). Pour obtenir des exemples supplémentaires sur l’utilisation de reports en vue d’empêcher l’arrêt prématuré d’une tâche en arrière-plan, voir l’[exemple de tâche en arrière-plan](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask).

Les étapes qui suivent sont à effectuer dans l’une de vos classes d’application (par exemple, MainPage.xaml.cs).

> [!NOTE]
> Vous pouvez également créer une fonction dédiée à l’inscription des tâches en arrière-plan &mdash; , consultez [inscrire une tâche en arrière-plan](register-a-background-task.md). Dans ce cas, au lieu d’utiliser les trois étapes suivantes, vous pouvez simplement construire le déclencheur et le fournir à la fonction d’inscription avec le nom de la tâche, le point d’entrée de la tâche et (éventuellement) une condition.

## <a name="register-the-background-task-to-run"></a>Inscrire la tâche en arrière-plan à des fins d’exécution

1.  Déterminez si la tâche en arrière-plan est déjà inscrite en effectuant une itération au sein de la propriété [**BackgroundTaskRegistration. AllTasks**](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.alltasks) . Cette étape est primordiale ; si votre application ne vérifie pas la présence d’inscriptions de tâches en arrière-plan existantes, elle peut aisément procéder plusieurs fois à l’inscription de la tâche, ce qui risque de poser des problèmes de performance et d’épuiser le temps processeur disponible pour la tâche avant que le travail ne soit effectué.

L’exemple suivant itère sur la propriété **AllTasks** et affecte à une variable d’indicateur la valeur true si la tâche est déjà inscrite.

```csharp
var taskRegistered = false;
var exampleTaskName = "ExampleBackgroundTask";

foreach (var task in BackgroundTaskRegistration.AllTasks)
{
    if (task.Value.Name == exampleTaskName)
    {
        taskRegistered = true;
        break;
    }
}
```

```cppwinrt
std::wstring exampleTaskName{ L"ExampleBackgroundTask" };

auto allTasks{ Windows::ApplicationModel::Background::BackgroundTaskRegistration::AllTasks() };

bool taskRegistered{ false };
for (auto const& task : allTasks)
{
    if (task.Value().Name() == exampleTaskName)
    {
        taskRegistered = true;
        break;
    }
}

// The code in the next step goes here.
```

```cpp
boolean taskRegistered = false;
Platform::String^ exampleTaskName = "ExampleBackgroundTask";

auto iter = BackgroundTaskRegistration::AllTasks->First();
auto hascur = iter->HasCurrent;

while (hascur)
{
    auto cur = iter->Current->Value;

    if(cur->Name == exampleTaskName)
    {
        taskRegistered = true;
        break;
    }

    hascur = iter->MoveNext();
}
```

2.  Si la tâche en arrière-plan n’est pas déjà inscrite, utilisez [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) pour créer une instance de votre tâche en arrière-plan. Le point d’entrée de la tâche doit correspondre au nom de votre classe de tâche en arrière-plan précédé de l’espace de noms.

Le déclencheur de tâche en arrière-plan contrôle à quel moment la tâche en arrière-plan. Pour obtenir la liste des déclencheurs possibles, consultez [**événement systemtrigger**](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType).

Par exemple, ce code crée une tâche en arrière-plan et la définit pour s’exécuter lorsque le déclencheur **TimeZoneChanged** se produit :

```csharp
var builder = new BackgroundTaskBuilder();

builder.Name = exampleTaskName;
builder.TaskEntryPoint = "Tasks.ExampleBackgroundTask";
builder.SetTrigger(new SystemTrigger(SystemTriggerType.TimeZoneChange, false));
```

```cppwinrt
if (!taskRegistered)
{
    Windows::ApplicationModel::Background::BackgroundTaskBuilder builder;
    builder.Name(exampleTaskName);
    builder.TaskEntryPoint(L"Tasks.ExampleBackgroundTask");
    builder.SetTrigger(Windows::ApplicationModel::Background::SystemTrigger{
        Windows::ApplicationModel::Background::SystemTriggerType::TimeZoneChange, false });
    // The code in the next step goes here.
}
```

```cpp
auto builder = ref new BackgroundTaskBuilder();

builder->Name = exampleTaskName;
builder->TaskEntryPoint = "Tasks.ExampleBackgroundTask";
builder->SetTrigger(ref new SystemTrigger(SystemTriggerType::TimeZoneChange, false));
```

3.  Vous pouvez ajouter une condition afin de contrôler à quel moment votre tâche sera exécutée après que l’événement de déclencheur est survenu (facultatif). Par exemple, si vous ne souhaitez pas que la tâche s’exécute tant que l’utilisateur n’est pas présent, appliquez la condition **UserPresent**. Pour obtenir la liste des conditions possibles, consultez [**SystemConditionType**](/uwp/api/Windows.ApplicationModel.Background.SystemConditionType).

L’exemple de code suivant affecte une condition qui exige la présence de l’utilisateur :

```csharp
builder.AddCondition(new SystemCondition(SystemConditionType.UserPresent));
```

```cppwinrt
builder.AddCondition(Windows::ApplicationModel::Background::SystemCondition{ Windows::ApplicationModel::Background::SystemConditionType::UserPresent });
// The code in the next step goes here.
```

```cpp
builder->AddCondition(ref new SystemCondition(SystemConditionType::UserPresent));
```

4.  Inscrivez la tâche en arrière-plan en appelant la méthode Register sur l’objet [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) . Stockez le résultat de [**BackgroundTaskRegistration**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration) afin qu’il puisse être utilisé à l’étape suivante.

Le code qui suit inscrit la tâche en arrière-plan et stocke le résultat :

```csharp
BackgroundTaskRegistration task = builder.Register();
```

```cppwinrt
Windows::ApplicationModel::Background::BackgroundTaskRegistration task{ builder.Register() };
```

```cpp
BackgroundTaskRegistration^ task = builder->Register();
```

> [!NOTE]
> Les applications Windows universelles doivent appeler [**RequestAccessAsync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) avant d’inscrire l’un des types de déclencheurs d’arrière-plan.

Pour vous assurer que votre application Windows universelle continue de s’exécuter correctement après la publication d’une mise à jour, utilisez le déclencheur **ServicingComplete** (voir [SystemTriggerType](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType)) pour effectuer les modifications de configuration requises après la mise à jour, telles que la migration de la base de données de l’application et l’inscription des tâches en arrière-plan. Pour l’instant, il est recommandé d’annuler l’inscription des tâches en arrière-plan associées à la version précédente de l’application (voir [**RemoveAccess**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.removeaccess)) et d’inscrire des tâches en arrière-plan pour la nouvelle version de l’application (voir [**RequestAccessAsync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync)).

Pour plus d’informations, voir [Recommandations en matière de tâches en arrière-plan](guidelines-for-background-tasks.md).

## <a name="handle-background-task-completion-using-event-handlers"></a>Gérer l’achèvement des tâches en arrière-plan à l’aide de gestionnaires d’événements

Vous devez enregistrer une méthode avec [**BackgroundTaskCompletedEventHandler**](/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler), afin que votre application puisse obtenir les résultats de la tâche en arrière-plan. Lorsque l’application est lancée ou reprise, la méthode marquée est appelée si la tâche en arrière-plan est terminée depuis la dernière fois que l’application est au premier plan. (La méthode OnCompleted est appelée immédiatement si la tâche en arrière-plan se termine pendant que votre application est au premier plan.)

1.  Écrivez une méthode OnCompleted pour gérer l’achèvement des tâches en arrière-plan. Par exemple, le résultat des tâches en arrière-plan peut entraîner une mise à jour de l’interface utilisateur. L’empreinte de la méthode présentée ici est requise pour la méthode de gestionnaire d’événements OnCompleted, même si cet exemple n’utilise pas le paramètre *args*.

L’exemple de code suivant reconnaît l’achèvement des tâches en arrière-plan et appelle un exemple de méthode de mise à jour de l’interface utilisateur qui prend une chaîne de message.

```csharp
private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
{
    var settings = Windows.Storage.ApplicationData.Current.LocalSettings;
    var key = task.TaskId.ToString();
    var message = settings.Values[key].ToString();
    UpdateUI(message);
}
```

```cppwinrt
void UpdateUI(winrt::hstring const& message)
{
    MyTextBlock().Dispatcher().RunAsync(Windows::UI::Core::CoreDispatcherPriority::Normal, [=]()
    {
        MyTextBlock().Text(message);
    });
}

void OnCompleted(
    Windows::ApplicationModel::Background::BackgroundTaskRegistration const& sender,
    Windows::ApplicationModel::Background::BackgroundTaskCompletedEventArgs const& /* args */)
{
    // You'll previously have inserted this key into local settings.
    auto settings{ Windows::Storage::ApplicationData::Current().LocalSettings().Values() };
    auto key{ winrt::to_hstring(sender.TaskId()) };
    auto message{ winrt::unbox_value<winrt::hstring>(settings.Lookup(key)) };

    UpdateUI(message);
}
```

```cpp
void MainPage::OnCompleted(BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
{
    auto settings = ApplicationData::Current->LocalSettings->Values;
    auto key = task->TaskId.ToString();
    auto message = dynamic_cast<String^>(settings->Lookup(key));
    UpdateUI(message);
}
```

> [!NOTE]
> Les mises à jour de l’interface utilisateur doivent être effectuées de manière asynchrone pour éviter de retarder le thread d’interface utilisateur. Pour obtenir un exemple, voir la méthode UpdateUI dans l’[exemple de tâche en arrière-plan](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask).

2.  Revenez à l’endroit où vous avez inscrit la tâche en arrière-plan. Après cette ligne de code, ajoutez un nouvel objet [**BackgroundTaskCompletedEventHandler**](/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler) . Fournissez votre méthode OnCompleted comme paramètre du constructeur **BackgroundTaskCompletedEventHandler**.

L’exemple de code suivant ajoute un [**BackgroundTaskCompletedEventHandler**](/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler) à [**BackgroundTaskRegistration**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration):

```csharp
task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
```

```cppwinrt
task.Completed({ this, &MainPage::OnCompleted });
```

```cpp
task->Completed += ref new BackgroundTaskCompletedEventHandler(this, &MainPage::OnCompleted);
```

## <a name="declare-in-the-app-manifest-that-your-app-uses-background-tasks"></a>Déclarer dans le manifeste de l’application que votre application utilise des tâches en arrière-plan

Pour que votre application puisse exécuter des tâches en arrière-plan, vous devez déclarer chaque tâche en arrière-plan dans le manifeste de l’application. Si votre application tente d’enregistrer une tâche en arrière-plan avec un déclencheur qui n’est pas répertorié dans le manifeste, l’inscription de la tâche en arrière-plan échoue avec une erreur « classe d’exécution non inscrite ».

1.  Ouvrez le concepteur de manifeste du package en accédant au fichier nommé Package.appxmanifest.
2.  Ouvrez l’onglet **Déclarations**.
3.  Dans la liste déroulante **Déclarations disponibles**, sélectionnez **Tâches en arrière-plan**, puis cliquez sur **Ajouter**.
4.  Cochez la case **Événement système**.
5.  Dans la zone de texte **point d’entrée :** , entrez l’espace de noms et le nom de votre classe d’arrière-plan, pour cet exemple, Tasks. ExampleBackgroundTask.
6.  Fermez le concepteur de manifeste.

L’élément Extensions suivant est ajouté à votre fichier Package.appxmanifest pour inscrire la tâche en arrière-plan :

```xml
<Extensions>
  <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ExampleBackgroundTask">
    <BackgroundTasks>
      <Task Type="systemEvent" />
    </BackgroundTasks>
  </Extension>
</Extensions>
```

## <a name="summary-and-next-steps"></a>Résumé et étapes suivantes

Vous devez à présent être en mesure d’écrire une classe de tâche en arrière-plan, d’inscrire la tâche en arrière-plan dans votre application et de permettre à votre application de reconnaître à quel moment la tâche en arrière-plan est achevée. Vous devez également savoir comment mettre à jour le manifeste de l’application de sorte que votre application peut inscrire correctement la tâche en arrière-plan.

> [!NOTE]
> Téléchargez l’[exemple de tâche en arrière-plan](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask) pour examiner des exemples de code similaires dans le contexte d’une application UWP aboutie et robuste ayant recours à des tâches en arrière-plan.

Consultez les rubriques connexes suivantes pour obtenir des informations de référence sur les API, des recommandations conceptuelles pour les tâches en arrière-plan, ainsi que des instructions plus détaillées pour écrire des applications qui utilisent des tâches en arrière-plan.

## <a name="related-topics"></a>Rubriques connexes

**Rubriques d’instructions détaillées sur les tâches en arrière-plan**

* [Répondre aux événements système avec des tâches en arrière-plan](respond-to-system-events-with-background-tasks.md)
* [Inscrire une tâche en arrière-plan](register-a-background-task.md)
* [Définir des conditions pour l’exécution d’une tâche en arrière-plan](set-conditions-for-running-a-background-task.md)
* [Utiliser un déclencheur de maintenance](use-a-maintenance-trigger.md)
* [Gérer une tâche en arrière-plan annulée](handle-a-cancelled-background-task.md)
* [Surveiller la progression et l’achèvement des tâches en arrière-plan](monitor-background-task-progress-and-completion.md)
* [Exécuter une tâche en arrière-plan en fonction d’un minuteur](run-a-background-task-on-a-timer-.md)
* [Créer et inscrire une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md).
* [Convertir une tâche en arrière-plan hors processus en une tâche en arrière-plan in-process](convert-out-of-process-background-task.md)  

**Recommandations en matière de tâches en arrière-plan**

* [Recommandations relatives aux tâches en arrière-plan](guidelines-for-background-tasks.md)
* [Déboguer une tâche en arrière-plan](debug-a-background-task.md)
* [Comment déclencher des événements de suspension, de reprise et d’arrière-plan dans des applications UWP (lors du débogage)](/previous-versions/hh974425(v=vs.110))

**Informations de référence d’API de tâche en arrière-plan**

* [**Windows.ApplicationModel.Background**](/uwp/api/Windows.ApplicationModel.Background)