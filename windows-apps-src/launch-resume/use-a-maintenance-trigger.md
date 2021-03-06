---
title: Utiliser un déclencheur de maintenance
description: Découvrez comment utiliser la classe MaintenanceTrigger pour exécuter du code léger en arrière-plan tandis que l’appareil est branché.
ms.assetid: 727D9D84-6C1D-4DF3-B3B0-2204EA4D76DD
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: c390280e084cd8557633f591b1686a0550f6a656
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155763"
---
# <a name="use-a-maintenance-trigger"></a>Utiliser un déclencheur de maintenance

**API importantes**

- [**MaintenanceTrigger**](/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger)
- [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)
- [**SystemCondition**](/uwp/api/Windows.ApplicationModel.Background.SystemCondition)

Découvrez comment utiliser la classe [**MaintenanceTrigger**](/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger) pour exécuter du code léger en arrière-plan pendant que l’appareil est branché.

## <a name="create-a-maintenance-trigger-object"></a>Créer un objet déclencheur de maintenance

Cet exemple suppose que vous disposez d’un code léger à exécuter en arrière-plan qui vous permettra d’améliorer votre application pendant que l’appareil est branché. Cette rubrique porte sur le déclencheur [**MaintenanceTrigger**](/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger), qui est semblable au déclencheur [**SystemTrigger**](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType).

Des informations supplémentaires sur l’écriture d’une classe de tâche en arrière-plan sont disponibles dans les rubriques [Créer et inscrire une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md) ou [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-a-background-task.md).

Créez un nouvel objet [**MaintenanceTrigger**](/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger) . Le deuxième paramètre, *OneShot*, indique si la tâche de maintenance s’exécute une seule fois ou régulièrement. Si *OneShot* est défini sur true, le premier paramètre (*FreshnessTime*) indique le nombre de minutes à attendre avant de planifier la tâche en arrière-plan. Si *OneShot* est défini sur false, *FreshnessTime* indique la fréquence d’exécution de la tâche en arrière-plan.

> [!NOTE]
> Si *FreshnessTime* a une valeur inférieure à 15 minutes, une exception est levée lors de la tentative d’inscription de la tâche en arrière-plan.

Cet exemple de code crée un déclencheur qui s’exécute une fois par heure.

```csharp
uint waitIntervalMinutes = 60;
MaintenanceTrigger taskTrigger = new MaintenanceTrigger(waitIntervalMinutes, false);
```

```cppwinrt
uint32_t waitIntervalMinutes{ 60 };
Windows::ApplicationModel::Background::MaintenanceTrigger taskTrigger{ waitIntervalMinutes, false };
```

```cpp
unsigned int waitIntervalMinutes = 60;
MaintenanceTrigger ^ taskTrigger = ref new MaintenanceTrigger(waitIntervalMinutes, false);
```

## <a name="optional-add-a-condition"></a>(Facultatif) Ajouter une condition

- Si besoin est, créez une condition de tâche en arrière-plan afin de contrôler le moment où la tâche est exécutée. Une condition empêche votre tâche en arrière-plan de s’exécuter tant que la condition n’est pas satisfaite. Pour plus d’informations, voir [Définir des conditions pour exécuter une tâche en arrière-plan](set-conditions-for-running-a-background-task.md).

Dans cet exemple, la condition est définie sur **InternetAvailable**, de sorte que la maintenance s’exécute quand Internet est accessible (ou le devient). Pour obtenir la liste des conditions de tâche d’arrière-plan possibles, consultez [**SystemConditionType**](/uwp/api/Windows.ApplicationModel.Background.SystemConditionType).

Le code suivant ajoute une condition au générateur de tâches de maintenance :

```csharp
SystemCondition exampleCondition = new SystemCondition(SystemConditionType.InternetAvailable);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemCondition exampleCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };
```

```cpp
SystemCondition ^ exampleCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
```

## <a name="register-the-background-task"></a>Inscrire la tâche en arrière-plan

- Inscrivez la tâche en arrière-plan en appelant la fonction qui vous permet de le faire. Pour plus d’informations sur l’inscription des tâches en arrière-plan, voir [Inscrire une tâche en arrière-plan](register-a-background-task.md).

Le code suivant inscrit la tâche de maintenance. Notez qu’il part du principe que votre tâche en arrière-plan s’exécute dans un processus distinct de celui de votre application, car il spécifie `entryPoint`. Si votre tâche en arrière-plan s’exécute dans le même processus que votre application, vous ne devez pas spécifier `entryPoint`.

```csharp
string entryPoint = "Tasks.ExampleBackgroundTaskClass";
string taskName   = "Maintenance background task example";

BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition);
```

```cppwinrt
std::wstring entryPoint{ L"Tasks.ExampleBackgroundTaskClass" };
std::wstring taskName{ L"Maintenance background task example" };

Windows::ApplicationModel::Background::BackgroundTaskRegistration task{
    RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition) };
```

```cpp
String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
String ^ taskName   = "Maintenance background task example";

BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition);
```

> [!NOTE]
> Pour toutes les familles d’appareils, à l’exception des ordinateurs de bureau, les tâches en arrière-plan peuvent être arrêtées en cas de mémoire insuffisante de l’appareil. Si aucune exception de mémoire insuffisante n’est exposée ou si l’application ne la gère pas, la tâche en arrière-plan est alors arrêtée sans avertissement ni déclenchement de l’événement OnCanceled. Cela permet de garantir l’expérience utilisateur de l’application au premier plan. Votre tâche en arrière-plan doit être conçue de manière à gérer ce scénario.

> [!NOTE]
> Plateforme Windows universelle applications doivent appeler [**RequestAccessAsync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) avant d’inscrire l’un des types de déclencheurs d’arrière-plan.

Pour vous assurer que votre application Windows universelle continue à s’exécuter correctement après la publication d’une mise à jour de votre application, vous devez appeler [**RemoveAccess**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.removeaccess) , puis appeler [**RequestAccessAsync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) lorsque votre application est lancée après sa mise à jour. Pour plus d’informations, voir [Recommandations en matière de tâches en arrière-plan](guidelines-for-background-tasks.md).

> [!NOTE]
> Les paramètres d’inscription de la tâche en arrière-plan sont validés au moment de l’inscription. Si l’un des paramètres d’inscription n’est pas valide, une erreur est renvoyée. Vérifiez que votre application gère de manière fluide les scénarios dans lesquels l’inscription de la tâche en arrière-plan échoue. En revanche, si votre application dépend d’un objet d’inscription valide après la tentative d’inscription d’une tâche, elle peut se bloquer.

## <a name="related-topics"></a>Rubriques connexes

* [Créer et inscrire une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md).
* [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-a-background-task.md)
* [Déclarer des tâches en arrière-plan dans le manifeste de l’application](declare-background-tasks-in-the-application-manifest.md)
* [Gérer une tâche en arrière-plan annulée](handle-a-cancelled-background-task.md)
* [Surveiller la progression et l’achèvement des tâches en arrière-plan](monitor-background-task-progress-and-completion.md)
* [Inscrire une tâche en arrière-plan](register-a-background-task.md)
* [Répondre aux événements système avec des tâches en arrière-plan](respond-to-system-events-with-background-tasks.md)
* [Définir des conditions pour l’exécution d’une tâche en arrière-plan](set-conditions-for-running-a-background-task.md)
* [Mettre à jour une vignette dynamique à partir d’une tâche en arrière-plan](update-a-live-tile-from-a-background-task.md)
* [Exécuter une tâche en arrière-plan en fonction d’un minuteur](run-a-background-task-on-a-timer-.md)
* [Recommandations relatives aux tâches en arrière-plan](guidelines-for-background-tasks.md)
* [Déboguer une tâche en arrière-plan](debug-a-background-task.md)
* [Comment déclencher des événements de suspension, de reprise et d’arrière-plan dans des applications UWP (lors du débogage)](/previous-versions/hh974425(v=vs.110))