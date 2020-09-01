---
title: Déclencher une tâche en arrière-plan à partir de votre application
description: Découvrez comment utiliser un déclencheur d’application pour exécuter une tâche en arrière-plan que vous souhaitez activer à partir de votre application.
ms.date: 07/06/2018
ms.topic: article
keywords: déclencheur de tâche en arrière-plan, tâche en arrière-plan
ms.localizationpriority: medium
ms.openlocfilehash: 446adc9921d2d124fb6e1304a06b70fa72da3d96
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155833"
---
# <a name="trigger-a-background-task-from-within-your-app"></a>Déclencher une tâche en arrière-plan à partir de votre application

Découvrez comment utiliser [ApplicationTrigger](/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger) pour activer une tâche en arrière-plan à partir de votre application.

Pour obtenir un exemple de création d’un déclencheur d’application, consultez cet [exemple](https://github.com/Microsoft/Windows-universal-samples/blob/v2.0.0/Samples/BackgroundTask/cs/BackgroundTask/Scenario5_ApplicationTriggerTask.xaml.cs).

Cette rubrique suppose que vous avez une tâche en arrière-plan que vous souhaitez activer à partir de votre application. Si vous n’avez pas encore de tâche en arrière-plan, il existe un exemple de tâche en arrière-plan sur [BackgroundActivity.cs](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/BackgroundActivation/cs/BackgroundActivity.cs). Ou bien, suivez les étapes décrites dans [créer et inscrire une tâche en arrière-plan out-of-process](create-and-register-a-background-task.md) pour en créer une.

## <a name="why-use-an-application-trigger"></a>Pourquoi utiliser un déclencheur d’application ?

Utilisez un **ApplicationTrigger** pour exécuter du code dans un processus séparé à partir de l’application de premier plan. Un **ApplicationTrigger** est approprié si votre application a un travail qui doit être effectué en arrière-plan, même si l’utilisateur ferme l’application de premier plan. Si le travail en arrière-plan doit s’arrêter lorsque l’application est fermée ou être lié à l’état du processus de premier plan, [l’exécution étendue](run-minimized-with-extended-execution.md) doit être utilisée à la place.

## <a name="create-an-application-trigger"></a>Créer un déclencheur d’application

Créez un nouveau [ApplicationTrigger](/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger). Vous pouvez la stocker dans un champ comme dans l’extrait de code ci-dessous. C’est pour des raisons pratiques que nous n’avons pas besoin de créer une nouvelle instance ultérieurement lorsque nous souhaitons signaler le déclencheur. Toutefois, vous pouvez utiliser n’importe quelle instance **ApplicationTrigger** pour signaler le déclencheur.

```csharp
// _AppTrigger is an ApplicationTrigger field defined at a scope that will keep it alive
// as long as you need to trigger the background task.
// Or, you could create a new ApplicationTrigger instance and use that when you want to
// trigger the background task.
_AppTrigger = new ApplicationTrigger();
```

```cppwinrt
// _AppTrigger is an ApplicationTrigger field defined at a scope that will keep it alive
// as long as you need to trigger the background task.
// Or, you could create a new ApplicationTrigger instance and use that when you want to
// trigger the background task.
Windows::ApplicationModel::Background::ApplicationTrigger _AppTrigger;
```

```cpp
// _AppTrigger is an ApplicationTrigger field defined at a scope that will keep it alive
// as long as you need to trigger the background task.
// Or, you could create a new ApplicationTrigger instance and use that when you want to
// trigger the background task.
ApplicationTrigger ^ _AppTrigger = ref new ApplicationTrigger();
```

## <a name="optional-add-a-condition"></a>(Facultatif) Ajouter une condition

Vous pouvez créer une condition de tâche en arrière-plan pour contrôler le moment d’exécution de la tâche. Une condition empêche la tâche en arrière-plan de s’exécuter jusqu’à ce que la condition soit remplie. Pour plus d’informations, consultez [définir les conditions d’exécution d’une tâche en arrière-plan](set-conditions-for-running-a-background-task.md).

Dans cet exemple, la condition est définie sur **InternetAvailable** afin que, une fois déclenchée, la tâche s’exécute uniquement une fois que l’accès à Internet est disponible. Pour obtenir la liste des conditions possibles, consultez [**SystemConditionType**](/uwp/api/Windows.ApplicationModel.Background.SystemConditionType).

```csharp
SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemCondition internetCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };
```

```cpp
SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable)
```

Pour obtenir des informations plus détaillées sur les conditions et types de déclencheurs d’arrière-plan, consultez [prise en charge de votre application avec des tâches en arrière-plan](support-your-app-with-background-tasks.md).

##  <a name="call-requestaccessasync"></a>Appeler RequestAccessAsync()

Avant d’inscrire la tâche d’arrière-plan **ApplicationTrigger** , appelez [**RequestAccessAsync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) pour déterminer le niveau d’activité d’arrière-plan que l’utilisateur autorise, car l’utilisateur a peut-être désactivé l’activité en arrière-plan pour votre application. Pour plus d’informations sur la façon dont les utilisateurs peuvent contrôler les paramètres de l’activité d’arrière-plan, consultez [optimiser l’activité en arrière-plan](../debug-test-perf/optimize-background-activity.md) .

```csharp
var requestStatus = await Windows.ApplicationModel.Background.BackgroundExecutionManager.RequestAccessAsync();
if (requestStatus != BackgroundAccessStatus.AlwaysAllowed)
{
   // Depending on the value of requestStatus, provide an appropriate response
   // such as notifying the user which functionality won't work as expected
}
```

## <a name="register-the-background-task"></a>Inscrire la tâche en arrière-plan

Inscrivez la tâche en arrière-plan en appelant la fonction qui vous permet de le faire. Pour plus d’informations sur l’inscription des tâches en arrière-plan et pour voir la définition de la méthode **RegisterBackgroundTask ()** dans l’exemple de code ci-dessous, consultez [inscrire une tâche en arrière-plan](register-a-background-task.md).

Si vous envisagez d’utiliser un déclencheur d’application pour étendre la durée de vie de votre processus de premier plan, envisagez d’utiliser à la place une [exécution étendue](run-minimized-with-extended-execution.md) . Le déclencheur d’application est conçu pour créer un processus hébergé séparément pour effectuer des tâches dans. L’extrait de code suivant inscrit un déclencheur d’arrière-plan out-of-process.

```csharp
string entryPoint = "Tasks.ExampleBackgroundTaskClass";
string taskName   = "Example application trigger";

BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, appTrigger, internetCondition);
```

```cppwinrt
std::wstring entryPoint{ L"Tasks.ExampleBackgroundTaskClass" };
std::wstring taskName{ L"Example application trigger" };

Windows::ApplicationModel::Background::BackgroundTaskRegistration task{
    RegisterBackgroundTask(entryPoint, taskName, appTrigger, internetCondition) };
```

```cpp
String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
String ^ taskName   = "Example application trigger";

BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, appTrigger, internetCondition);
```

Les paramètres d’inscription de la tâche en arrière-plan sont validés au moment de l’inscription. Si l’un des paramètres d’inscription n’est pas valide, une erreur est renvoyée. Vérifiez que votre application gère de manière fluide les scénarios dans lesquels l’inscription de la tâche en arrière-plan échoue. En revanche, si votre application dépend d’un objet d’inscription valide après la tentative d’inscription d’une tâche, elle peut se bloquer.

## <a name="trigger-the-background-task"></a>Déclencher la tâche en arrière-plan

Avant de déclencher la tâche en arrière-plan, utilisez [BackgroundTaskRegistration](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration) pour vérifier que la tâche en arrière-plan est inscrite. Il est judicieux de vérifier que toutes les tâches en arrière-plan sont inscrites lors du lancement de l’application.

Déclenchez la tâche en arrière-plan en appelant [ApplicationTrigger. RequestAsync](/uwp/api/windows.applicationmodel.background.applicationtrigger). Toute instance **ApplicationTrigger** fera.

Notez que **ApplicationTrigger. RequestAsync** ne peut pas être appelé à partir de la tâche en arrière-plan elle-même, ou quand l’application est en cours d’exécution (voir [cycle de vie](app-lifecycle.md) des applications pour plus d’informations sur les États de l’application).
Elle peut retourner [DisabledByPolicy](/uwp/api/windows.applicationmodel.background.applicationtriggerresult) si l’utilisateur a défini des stratégies d’énergie ou de confidentialité qui empêchent l’application d’effectuer une activité en arrière-plan.
En outre, un seul AppTrigger peut être exécuté à la fois. Si vous tentez d’exécuter un AppTrigger alors qu’un autre est déjà en cours d’exécution, la fonction renverra [CurrentlyRunning](/uwp/api/windows.applicationmodel.background.applicationtriggerresult).

```csharp
var result = await _AppTrigger.RequestAsync();
```

## <a name="manage-resources-for-your-background-task"></a>Gérer les ressources pour votre tâche en arrière-plan

Utilisez la méthode [BackgroundExecutionManager.RequestAccessAsync](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager) pour déterminer si l’utilisateur a opté pour une activité limitée de votre application en arrière-plan. Tenez compte du taux d’utilisation de la batterie ; exécutez l’application en arrière-plan uniquement lorsqu’elle est nécessaire dans le cadre d’une action souhaitée par l’utilisateur. Pour plus d’informations sur la façon dont les utilisateurs peuvent contrôler les paramètres de l’activité d’arrière-plan, consultez [optimiser l’activité en arrière-plan](../debug-test-perf/optimize-background-activity.md) .  

- Mémoire : le réglage de la mémoire et de l’utilisation de l’énergie de votre application est essentiel pour garantir que le système d’exploitation permettra l’exécution de votre tâche en arrière-plan. Utilisez les [API de gestion](/uwp/api/windows.system.memorymanager) de la mémoire pour voir la quantité de mémoire utilisée par votre tâche en arrière-plan. Plus la tâche en arrière-plan utilise de la mémoire, plus il est difficile pour le système d’exploitation de le maintenir en cours d’exécution lorsqu’une autre application est au premier plan. L’utilisateur dispose d’un contrôle étroit sur l’ensemble des activités en arrière-plan que votre application peut exécuter, et bénéficie d’une visibilité étendue sur l’impact de cette dernière sur le taux d’utilisation de la batterie.  
- Temps processeur : les tâches en arrière-plan sont limitées par la durée d’utilisation de l’horloge sur le type de déclencheur. Les tâches en arrière-plan déclenchées par le déclencheur d’application sont limitées à environ 10 minutes.

Pour plus d’informations sur les contraintes de ressource appliquées aux tâches en arrière-plan, consultez [Prendre en charge votre application avec des tâches en arrière-plan](support-your-app-with-background-tasks.md).

## <a name="remarks"></a>Remarques

À compter de Windows 10, il n’est plus nécessaire pour l’utilisateur d’ajouter votre application à l’écran de verrouillage afin d’utiliser des tâches en arrière-plan.

Une tâche en arrière-plan s’exécute uniquement à l’aide d’un **ApplicationTrigger** si vous avez appelé [**RequestAccessAsync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) en premier.

## <a name="related-topics"></a>Rubriques connexes

* [Recommandations relatives aux tâches en arrière-plan](guidelines-for-background-tasks.md)
* [Exemple de code de tâche en arrière-plan](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)
* [Créer et inscrire une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md).
* [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-a-background-task.md)
* [Déboguer une tâche en arrière-plan](debug-a-background-task.md)
* [Déclarer des tâches en arrière-plan dans le manifeste de l’application](declare-background-tasks-in-the-application-manifest.md)
* [Libérer de la mémoire quand l’application bascule en arrière-plan](reduce-memory-usage.md)
* [Gérer une tâche en arrière-plan annulée](handle-a-cancelled-background-task.md)
* [Comment déclencher des événements de suspension, de reprise et d’arrière-plan dans des applications UWP (lors du débogage)](/previous-versions/hh974425(v=vs.110))
* [Surveiller la progression et l’achèvement des tâches en arrière-plan](monitor-background-task-progress-and-completion.md)
* [Reporter la suspension d’une application avec l’exécution étendue](run-minimized-with-extended-execution.md)
* [Inscrire une tâche en arrière-plan](register-a-background-task.md)
* [Répondre aux événements système avec des tâches en arrière-plan](respond-to-system-events-with-background-tasks.md)
* [Définir des conditions pour l’exécution d’une tâche en arrière-plan](set-conditions-for-running-a-background-task.md)
* [Mettre à jour une vignette dynamique à partir d’une tâche en arrière-plan](update-a-live-tile-from-a-background-task.md)
* [Utiliser un déclencheur de maintenance](use-a-maintenance-trigger.md)