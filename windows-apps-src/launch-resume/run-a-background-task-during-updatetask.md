---
title: Exécuter une tâche en arrière-plan lorsque votre application UWP est mise à jour
description: Découvrez comment créer une tâche en arrière-plan qui s’exécute lorsque votre application de magasin plateforme Windows universelle (UWP) est mise à jour.
ms.date: 04/21/2017
ms.topic: article
keywords: Windows 10, UWP, mettre à jour, tâche en arrière-plan, UpdateTask, tâche en arrière-plan
ms.localizationpriority: medium
ms.openlocfilehash: ff4dd5487357728b6a79f4c4d31a437075fcd006
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164803"
---
# <a name="run-a-background-task-when-your-uwp-app-is-updated"></a>Exécuter une tâche en arrière-plan lorsque votre application UWP est mise à jour

Découvrez comment écrire une tâche en arrière-plan qui s’exécute après que votre application de Store plateforme Windows universelle (UWP) a été mise à jour.

La tâche d’arrière-plan de la tâche de mise à jour est appelée par le système d’exploitation après que l’utilisateur a installé une mise à jour d’une application installée sur l’appareil. Cela permet à votre application d’effectuer des tâches d’initialisation, telles que l’initialisation d’un nouveau canal de notification push, la mise à jour du schéma de base de données, etc., avant que l’utilisateur lance votre application mise à jour.

La tâche de mise à jour diffère du lancement d’une tâche en arrière-plan à l’aide du déclencheur [ServicingComplete](/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) , car dans ce cas, votre application doit s’exécuter au moins une fois avant d’être mise à jour afin d’inscrire la tâche en arrière-plan qui sera activée par le déclencheur **ServicingComplete** .  La tâche de mise à jour n’est pas inscrite. par conséquent, une application qui n’a jamais été exécutée, mais qui est mise à niveau, aura toujours sa tâche de mise à jour déclenchée.

## <a name="step-1-create-the-background-task-class"></a>Étape 1 : créer la classe de tâche en arrière-plan

Comme pour les autres types de tâches en arrière-plan, vous implémentez la tâche d’arrière-plan de tâche de mise à jour en tant que composant Windows Runtime. Pour créer ce composant, suivez les étapes de la section **créer la classe de tâche en arrière-plan** de la rubrique [créer et inscrire une tâche en arrière-plan out-of-process](./create-and-register-a-background-task.md). Procédez comme suit :

- Ajout d’un projet de composant Windows Runtime à votre solution.
- Création d’une référence à partir de votre application dans le composant.
- Création d’une classe publique et sealed dans le composant qui implémente [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask).
- Implémentation de la méthode [**Run**](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) , qui est le point d’entrée requis qui est appelé lors de l’exécution de la tâche de mise à jour. Si vous envisagez d’effectuer des appels asynchrones à partir de votre tâche en arrière-plan, [créer et inscrire une tâche en arrière-plan out-of-process](./create-and-register-a-background-task.md) explique comment utiliser un report dans votre méthode d' **exécution** .

Vous n’avez pas besoin d’inscrire cette tâche en arrière-plan (la section « enregistrer la tâche en arrière-plan à exécuter » dans la rubrique **créer et inscrire une tâche en arrière-plan out-of-process** ) pour utiliser la tâche de mise à jour. Il s’agit de la principale raison d’utiliser une tâche de mise à jour, car vous n’avez pas besoin d’ajouter du code à votre application pour inscrire la tâche et l’application n’a pas besoin d’être exécutée une seule fois avant d’être mise à jour pour enregistrer la tâche en arrière-plan.

L’exemple de code suivant montre un point de départ de base pour une classe de tâche d’arrière-plan de tâche de mise à jour en C#. La classe de tâche d’arrière-plan elle-même-et toutes les autres classes dans le projet de tâche en arrière-plan doivent être **publiques** et **sealed**. Votre classe de tâche en arrière-plan doit dériver de **IBackgroundTask** et avoir une méthode **Run ()** publique avec la signature indiquée ci-dessous :

```cs
using Windows.ApplicationModel.Background;

namespace BackgroundTasks
{
    public sealed class UpdateTask : IBackgroundTask
    {
        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // your app migration/update code here
        }
    }
}
```

## <a name="step-2-declare-your-background-task-in-the-package-manifest"></a>Étape 2 : déclarer votre tâche en arrière-plan dans le manifeste du package

Dans le Explorateur de solutions Visual Studio, cliquez avec le bouton droit sur **Package. appxmanifest** , puis cliquez sur **afficher le code** pour afficher le manifeste du package. Ajoutez le `<Extensions>` code XML suivant pour déclarer votre tâche de mise à jour :

```XML
<Package ...>
    ...
  <Applications>  
    <Application ...>  
        ...
      <Extensions>  
        <Extension Category="windows.updateTask"  EntryPoint="BackgroundTasks.UpdateTask">  
        </Extension>  
      </Extensions>

    </Application>  
  </Applications>  
</Package>
```

Dans le XML ci-dessus, assurez-vous que l' `EntryPoint` attribut est défini sur le nom de l’espace de noms. Class de votre classe de tâche de mise à jour. Le nom respecte la casse.

## <a name="step-3-debugtest-your-update-task"></a>Étape 3 : déboguer/tester votre tâche de mise à jour

Assurez-vous que vous avez déployé votre application sur votre ordinateur pour qu’il y ait une mise à jour.

Définissez un point d’arrêt dans la méthode Run () de votre tâche en arrière-plan.

![définir un point d’arrêt](images/run-func-breakpoint.png)

Ensuite, dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet de votre application (pas le projet de tâche en arrière-plan), puis cliquez sur **Propriétés**. Dans le Fenêtre Propriétés de l’application, cliquez sur **Déboguer** sur la gauche, puis sélectionnez **ne pas lancer, mais déboguer mon code au démarrage**:

![définir les paramètres de débogage](images/do-not-launch-but-debug.png)

Ensuite, pour vous assurer que le UpdateTask est déclenché, augmentez le numéro de version du package. Dans la Explorateur de solutions, double-cliquez sur le fichier **Package. appxmanifest** de votre application pour ouvrir le concepteur de packages, puis mettez à jour le numéro de **Build** :

![mettre à jour la version](images/bump-version.png)

Maintenant, dans Visual Studio 2019, lorsque vous appuyez sur F5, votre application est mise à jour et le système active votre composant UpdateTask en arrière-plan. Le débogueur est automatiquement attaché au processus en arrière-plan. Votre point d’arrêt est alors atteint et vous pouvez parcourir votre logique de code de mise à jour.

Une fois la tâche en arrière-plan terminée, vous pouvez lancer l’application de premier plan à partir du menu Démarrer de Windows au sein de la même session de débogage. Le débogueur s’attache à nouveau automatiquement, cette fois à votre processus de premier plan, et vous pouvez effectuer un pas à pas détaillé dans la logique de votre application.

> [!NOTE]
> Utilisateurs de Visual Studio 2015 : les étapes ci-dessus s’appliquent à Visual Studio 2017 ou Visual Studio 2019. Si vous utilisez Visual Studio 2015, vous pouvez utiliser les mêmes techniques pour déclencher et tester le UpdateTask, sauf si Visual Studio ne s’y attache pas. Une autre procédure dans VS 2015 consiste à configurer un [ApplicationTrigger](./trigger-background-task-from-app.md) qui définit UpdateTask comme point d’entrée et à déclencher l’exécution directement à partir de l’application de premier plan.

## <a name="see-also"></a>Voir aussi

[Créer et inscrire une tâche en arrière-plan hors processus](./create-and-register-a-background-task.md)