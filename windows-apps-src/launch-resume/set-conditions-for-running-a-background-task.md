---
title: Définir des conditions pour exécuter une tâche en arrière-plan
description: Découvrez comment définir des conditions qui contrôlent le moment auquel votre tâche en arrière-plan s’exécutera.
ms.assetid: 10ABAC9F-AA8C-41AC-A29D-871CD9AD9471
ms.date: 07/06/2018
ms.topic: article
keywords: Windows 10, UWP, tâche en arrière-plan
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 04f351a2eed5290e31a3f40c5421addf01422154
ms.sourcegitcommit: cc645386b996f6e59f1ee27583dcd4310f8fb2a6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84262780"
---
# <a name="set-conditions-for-running-a-background-task"></a>Définir des conditions pour exécuter une tâche en arrière-plan

**API importantes**

- [**SystemCondition**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemCondition)
- [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType)
- [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)

Découvrez comment définir des conditions qui contrôlent le moment auquel votre tâche en arrière-plan s’exécutera.

Parfois, les tâches en arrière-plan requièrent que certaines conditions soient remplies pour que la tâche en arrière-plan aboutisse. Vous pouvez spécifier une ou plusieurs des conditions spécifiées par [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType) au moment d’inscrire votre tâche en arrière-plan. La condition est vérifiée une fois que le déclencheur a été activé. La tâche en arrière-plan est ensuite mise en file d’attente, mais elle ne s’exécute pas tant que toutes les conditions requises ne sont pas satisfaites.

Le fait de placer des conditions sur des tâches en arrière-plan économise la durée de vie de la batterie et le processeur en empêchant l’exécution inutile Par exemple, si votre tâche en arrière-plan est exécutée sur un minuteur et nécessite une connectivité Internet, ajoutez la condition **InternetAvailable** au [**TaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) avant d’inscrire la tâche. Cela empêchera ainsi la tâche de faire inutilement appel aux ressources système et à l’autonomie de la batterie. Elle s’exécutera uniquement une fois que le minuteur sera arrivé à expiration *et* qu’Internet sera accessible.

Il est également possible de combiner plusieurs conditions en appelant plusieurs fois **AddCondition** sur le même [**TaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder). Veillez à ne pas ajouter de conditions conflictuelles, telles que **UserPresent** et **UserNotPresent**.

## <a name="create-a-systemcondition-object"></a>Créer un objet SystemCondition

Cette rubrique suppose qu’une tâche en arrière-plan est déjà associée à votre application et que cette dernière comporte déjà du code qui crée un objet [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) nommé **taskBuilder**.  Consultez [Créer et inscrire une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md) ou [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-a-background-task.md) si vous devez commencer par créer une tâche en arrière-plan.

Cette rubrique concerne aussi bien les tâches en arrière-plan qui s’exécutent hors processus que celles qui s’exécutent dans le même processus que l’application au premier plan.

Avant d’ajouter la condition, créez un objet [**SystemCondition**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemCondition) pour représenter la condition qui doit être appliquée pour qu’une tâche en arrière-plan s’exécute. Dans le constructeur, spécifiez la condition qui doit être remplie avec une valeur d’énumération [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType) .

Le code suivant crée un objet [**SystemCondition**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemCondition) qui spécifie la condition **InternetAvailable** :

```csharp
SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemCondition internetCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };
```

```cpp
SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
```

## <a name="add-the-systemcondition-object-to-your-background-task"></a>Ajouter l’objet SystemCondition à votre tâche en arrière-plan

Pour ajouter la condition, appelez la méthode [**AddCondition**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.addcondition) sur l’objet [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) et transmettez-lui l’objet [**SystemCondition**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemCondition) .

Le code suivant utilise **taskBuilder** pour ajouter la condition **InternetAvailable** .

```csharp
taskBuilder.AddCondition(internetCondition);
```

```cppwinrt
taskBuilder.AddCondition(internetCondition);
```

```cpp
taskBuilder->AddCondition(internetCondition);
```

## <a name="register-your-background-task"></a>Inscrire votre tâche en arrière-plan

Vous pouvez maintenant inscrire votre tâche en arrière-plan à l’aide de la méthode [**Register**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.register) , et la tâche en arrière-plan ne démarrera pas tant que la condition spécifiée n’est pas remplie.

Le code suivant inscrit la tâche et stocke l’objet BackgroundTaskRegistration obtenu :

```csharp
BackgroundTaskRegistration task = taskBuilder.Register();
```

```cppwinrt
Windows::ApplicationModel::Background::BackgroundTaskRegistration task{ recurringTaskBuilder.Register() };
```

```cpp
BackgroundTaskRegistration ^ task = taskBuilder->Register();
```

> [!NOTE]
> Les paramètres d’inscription de la tâche en arrière-plan sont validés au moment de l’inscription. Si l’un des paramètres d’inscription n’est pas valide, une erreur est renvoyée. Vérifiez que votre application gère de manière fluide les scénarios dans lesquels l’inscription de la tâche en arrière-plan échoue. En revanche, si votre application dépend d’un objet d’inscription valide après la tentative d’inscription d’une tâche, elle peut se bloquer.

## <a name="place-multiple-conditions-on-your-background-task"></a>Placer plusieurs conditions dans la tâche en arrière-plan

Pour ajouter plusieurs conditions, votre application effectue plusieurs appels à la méthode [**AddCondition**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.addcondition). Pour être effectifs, ces appels doivent intervenir avant l’inscription de la tâche.

> [!NOTE]
> Veillez à ne pas ajouter de conditions conflictuelles à une tâche en arrière-plan.

L’extrait de code suivant montre plusieurs conditions dans le contexte de la création et de l’inscription d’une tâche en arrière-plan.

```csharp
// Set up the background task.
TimeTrigger hourlyTrigger = new TimeTrigger(60, false);

var recurringTaskBuilder = new BackgroundTaskBuilder();

recurringTaskBuilder.Name           = "Hourly background task";
recurringTaskBuilder.TaskEntryPoint = "Tasks.ExampleBackgroundTaskClass";
recurringTaskBuilder.SetTrigger(hourlyTrigger);

// Begin adding conditions.
SystemCondition userCondition     = new SystemCondition(SystemConditionType.UserPresent);
SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);

recurringTaskBuilder.AddCondition(userCondition);
recurringTaskBuilder.AddCondition(internetCondition);

// Done adding conditions, now register the background task.

BackgroundTaskRegistration task = recurringTaskBuilder.Register();
```

```cppwinrt
// Set up the background task.
Windows::ApplicationModel::Background::TimeTrigger hourlyTrigger{ 60, false };

Windows::ApplicationModel::Background::BackgroundTaskBuilder recurringTaskBuilder;

recurringTaskBuilder.Name(L"Hourly background task");
recurringTaskBuilder.TaskEntryPoint(L"Tasks.ExampleBackgroundTaskClass");
recurringTaskBuilder.SetTrigger(hourlyTrigger);

// Begin adding conditions.
Windows::ApplicationModel::Background::SystemCondition userCondition{
    Windows::ApplicationModel::Background::SystemConditionType::UserPresent };
Windows::ApplicationModel::Background::SystemCondition internetCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };

recurringTaskBuilder.AddCondition(userCondition);
recurringTaskBuilder.AddCondition(internetCondition);

// Done adding conditions, now register the background task.
Windows::ApplicationModel::Background::BackgroundTaskRegistration task{ recurringTaskBuilder.Register() };
```

```cpp
// Set up the background task.
TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);

auto recurringTaskBuilder = ref new BackgroundTaskBuilder();

recurringTaskBuilder->Name           = "Hourly background task";
recurringTaskBuilder->TaskEntryPoint = "Tasks.ExampleBackgroundTaskClass";
recurringTaskBuilder->SetTrigger(hourlyTrigger);

// Begin adding conditions.
SystemCondition ^ userCondition     = ref new SystemCondition(SystemConditionType::UserPresent);
SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);

recurringTaskBuilder->AddCondition(userCondition);
recurringTaskBuilder->AddCondition(internetCondition);

// Done adding conditions, now register the background task.
BackgroundTaskRegistration ^ task = recurringTaskBuilder->Register();
```

## <a name="remarks"></a>Remarques

> [!NOTE]
> Choisissez des conditions pour votre tâche en arrière-plan afin qu’elle s’exécute uniquement lorsqu’elle est nécessaire, et ne s’exécute pas dans le cas contraire. Pour obtenir une description des différentes conditions de tâche en arrière-plan, consultez [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType) .

## <a name="related-topics"></a>Rubriques connexes

* [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-a-background-task.md)
* [Créer et inscrire une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md)
* [Déclarer des tâches en arrière-plan dans le manifeste de l’application](declare-background-tasks-in-the-application-manifest.md)
* [Gérer une tâche en arrière-plan annulée](handle-a-cancelled-background-task.md)
* [Surveiller la progression et l’achèvement des tâches en arrière-plan](monitor-background-task-progress-and-completion.md)
* [Inscrire une tâche en arrière-plan](register-a-background-task.md)
* [Répondre aux événements système avec des tâches en arrière-plan](respond-to-system-events-with-background-tasks.md)
* [Mettre à jour une vignette dynamique à partir d’une tâche en arrière-plan](update-a-live-tile-from-a-background-task.md)
* [Utiliser un déclencheur de maintenance](use-a-maintenance-trigger.md)
* [Exécuter une tâche en arrière-plan en fonction d’un minuteur](run-a-background-task-on-a-timer-.md)
* [Recommandations en matière de tâches en arrière-plan](guidelines-for-background-tasks.md)
* [Déboguer une tâche en arrière-plan](debug-a-background-task.md)
* [Comment déclencher des événements de suspension, de reprise et d’arrière-plan dans des applications UWP (lors du débogage)](https://msdn.microsoft.com/library/windows/apps/hh974425(v=vs.110).aspx)
