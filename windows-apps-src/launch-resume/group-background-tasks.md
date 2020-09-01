---
title: Grouper l’inscription des tâches en arrière-plan
description: Inscrire/désinscrire des tâches en arrière-plan dans le cadre d’un groupe pour isoler ces inscriptions.
ms.date: 04/05/2017
ms.topic: article
keywords: Windows 10, tâche en arrière-plan
ms.openlocfilehash: 61419ac45acd27758e3c874ac4b03510561ccaf8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155863"
---
# <a name="group-background-task-registration"></a>Grouper l’inscription des tâches en arrière-plan

**API importantes**

[BackgroundTaskRegistrationGroup, classe](/uwp/api/windows.applicationmodel.background.backgroundtaskregistrationgroup)

Les tâches en arrière-plan peuvent maintenant être inscrites dans un groupe, que vous pouvez considérer comme un espace de noms logique. Cette isolation permet de s’assurer que les différents composants d’une application, ou des bibliothèques différentes, n’interfèrent pas avec l’inscription des tâches en arrière-plan de l’autre.

Lorsqu’une application et l’infrastructure (ou bibliothèque) qu’elle utilise inscrit une tâche en arrière-plan portant le même nom, elle peut supprimer par inadvertance les inscriptions de tâches en arrière-plan du Framework. Les créateurs d’applications peuvent également supprimer accidentellement des inscriptions de tâches en arrière-plan de Framework et de bibliothèque, car elles peuvent annuler l’inscription de toutes les tâches d’arrière-plan inscrites à l’aide de [BackgroundTaskRegistration. AllTasks](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.AllTasks).  Avec les groupes, vous pouvez isoler vos inscriptions de tâches en arrière-plan, ce qui ne se produit pas.

## <a name="features-of-groups"></a>Fonctionnalités des groupes

* Les groupes peuvent être identifiés de manière unique par un GUID. Elles peuvent également avoir une chaîne de nom conviviale associée qui est plus facile à lire pendant le débogage.
* Plusieurs tâches en arrière-plan peuvent être enregistrées dans un groupe.
* Les tâches en arrière-plan enregistrées dans un groupe n’apparaissent pas dans [BackgroundTaskRegistration. AllTasks](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.AllTasks). Par conséquent, les applications qui utilisent actuellement **BackgroundTaskRegistration. AllTasks** pour annuler l’inscription de leurs tâches ne désinscrivent pas par inadvertance les tâches d’arrière-plan inscrites dans un groupe. Consultez [Annuler l’inscription des tâches en arrière-plan dans un groupe](#unregister-background-tasks-in-a-group) ci-dessous pour savoir comment annuler l’inscription de tous les déclencheurs d’arrière-plan qui ont été enregistrés dans le cadre d’un groupe.
* Chaque inscription de tâche en arrière-plan aura une propriété de groupe pour déterminer à quel groupe il est associé.
* L’inscription des tâches en arrière-plan in-process avec un groupe entraîne l’activation de l’événement [BackgroundTaskRegistrationGroup. BackgroundActivated](/uwp/api/windows.applicationmodel.background.backgroundtaskregistrationgroup.BackgroundActivated) au lieu de [application. OnBackgroundActivated](/uwp/api/windows.ui.xaml.application.onbackgroundactivated#Windows_UI_Xaml_Application_OnBackgroundActivated_Windows_ApplicationModel_Activation_BackgroundActivatedEventArgs_).

## <a name="register-a-background-task-in-a-group"></a>Inscrire une tâche en arrière-plan dans un groupe

L’exemple suivant montre comment enregistrer une tâche en arrière-plan (déclenchée par une modification de fuseau horaire, dans cet exemple) dans le cadre d’un groupe.

```csharp
private const string groupFriendlyName = "myGroup";
private const string groupId = "3F2504E0-4F89-41D3-9A0C-0305E82C3301";
private const string myTaskName = "My Background Trigger";

public static void RegisterBackgroundTaskInGroup()
{
   BackgroundTaskRegistrationGroup group = BackgroundTaskRegistration.GetTaskGroup(groupId);
   bool isTaskRegistered = false;

   // See if this task already belongs to a group
   if (group != null)
   {
       foreach (var taskKeyValue in group.AllTasks)
       {
           if (taskKeyValue.Value.Name == myTaskName)
           {
               isTaskRegistered = true;
               break;
           }
       }
   }

   // If the background task is not in a group, register it
   if (!isTaskRegistered)
   {
       if (group == null)
       {
           group = new BackgroundTaskRegistrationGroup(groupId, groupFriendlyName);
       }

       var builder = new BackgroundTaskBuilder();
       builder.Name = myTaskName;
       builder.TaskGroup = group; // we specify the group, here
       builder.SetTrigger(new SystemTrigger(SystemTriggerType.TimeZoneChange, false));

       // Because builder.TaskEntryPoint is not specified, OnBackgroundActivated() will be raised when the background task is triggered
       BackgroundTaskRegistration task = builder.Register();
   }
}
```

## <a name="unregister-background-tasks-in-a-group"></a>Annuler l’inscription des tâches en arrière-plan dans un groupe

L’exemple suivant montre comment annuler l’inscription des tâches en arrière-plan qui ont été inscrites dans le cadre d’un groupe.
Étant donné que les tâches en arrière-plan enregistrées dans un groupe n’apparaissent pas dans [BackgroundTaskRegistration. AllTasks](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.AllTasks), vous devez itérer au sein des groupes, rechercher les tâches en arrière-plan inscrites dans chaque groupe et annuler leur inscription.

```csharp
private static void UnRegisterAllTasks()
{
    // Unregister tasks that are part of a group
    foreach (var groupKeyValue in BackgroundTaskRegistration.AllTaskGroups)
    {
        foreach (var groupedTask in groupKeyValue.Value.AllTasks)
        {
            groupedTask.Value.Unregister(true); // passing true to cancel currently running instances of this background task
        }
    }

    // Unregister tasks that aren't part of a group
    foreach(var taskKeyValue in BackgroundTaskRegistration.AllTasks)
    {
        taskKeyValue.Value.Unregister(true); // passing true to cancel currently running instances of this background task
    }
}
```

## <a name="register-persistent-events"></a>Inscrire des événements persistants

Lorsque vous utilisez des groupes d’inscription de tâche en arrière-plan avec des tâches en arrière-plan in-process, les activations en arrière-plan sont dirigées vers l’événement du groupe au lieu de celui de l’objet application ou CoreApplication. Cela permet à plusieurs composants dans votre application de gérer l’activation plutôt que de placer tous les chemins de code d’activation dans l’objet d’application. L’exemple suivant montre comment s’inscrire pour l’événement d’arrière-plan du groupe. Commencez par vérifier [BackgroundTaskRegistration. GetTaskGroup](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.gettaskgroup) pour déterminer si le groupe a déjà été inscrit. Si ce n’est pas le cas, créez un nouveau groupe avec votre ID et votre nom convivial. Ensuite, inscrivez un gestionnaire d’événements à l’événement BackgroundActivated sur le groupe.

```csharp
void RegisterPersistentEvent()
{
    var group = BackgroundTaskRegistration.GetTaskGroup(groupId);
    if (group == null)
    {
        group = new BackgroundTaskRegistrationGroup(groupId, groupFriendlyName);
    }

    group.BackgroundActivated += MyEventHandler;
}
```