---
title: Déclarer des tâches en arrière-plan dans le manifeste de l’application
description: Activez l’utilisation des tâches en arrière-plan en les déclarant comme extensions dans le manifeste de l’application.
ms.assetid: 6B4DD3F8-3C24-4692-9084-40999A37A200
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, tâche en arrière-plan
ms.localizationpriority: medium
ms.openlocfilehash: e1580bdc62585cb777334c217419b4de6a691add
ms.sourcegitcommit: 894decaf374f22bf39d4aecc1ab50d34ac011e31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88162564"
---
# <a name="declare-background-tasks-in-the-application-manifest"></a>Déclarer des tâches en arrière-plan dans le manifeste de l’application




**API importantes**

-   [**Schéma BackgroundTasks**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)
-   [**Windows.ApplicationModel.Background**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background)

Activez l’utilisation des tâches en arrière-plan en les déclarant comme extensions dans le manifeste de l’application.

> [!Important]
>  Cet article est spécifique aux tâches en arrière-plan hors processus. Les tâches en arrière-plan in-process ne sont pas déclarées dans le manifeste.

Les tâches d’arrière-plan out-of-process doivent être déclarées dans le manifeste de l’application, sinon votre application ne sera pas en mesure de les inscrire (une exception sera levée). De plus, les tâches en arrière-plan hors processus doivent être déclarées dans le manifeste de l’application pour réussir la certification.

Cette rubrique suppose que vous avez créé une ou plusieurs classes de tâche en arrière-plan et que votre application inscrit chaque tâche en arrière-plan à exécuter en réponse à un déclencheur au minimum.

## <a name="add-extensions-manually"></a>Ajouter manuellement les extensions


Ouvrez le manifeste de l’application (Package.appxmanifest) et accédez à l’élément Application. Créez un élément Extensions (s’il n’en existe pas).

L’extrait de code suivant provient de l’[exemple de tâche en arrière-plan](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask) :

```xml
<Application Id="App"
   ...
   <Extensions>
     <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.SampleBackgroundTask">
       <BackgroundTasks>
         <Task Type="systemEvent" />
         <Task Type="timer" />
       </BackgroundTasks>
     </Extension>
     <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ServicingComplete">
       <BackgroundTasks>
         <Task Type="systemEvent"/>
       </BackgroundTasks>
     </Extension>
   </Extensions>
 </Application>
```

## <a name="add-a-background-task-extension"></a>Ajouter une extension de tâche en arrière-plan  

Déclarez votre première tâche en arrière-plan.

Copiez ce code dans l’élément Extensions (vous ajouterez des attributs aux étapes suivantes).

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="">
      <BackgroundTasks>
        <Task Type="" />
      </BackgroundTasks>
    </Extension>
</Extensions>
```

1.  Modifiez l’attribut EntryPoint afin que votre code utilise la même chaîne comme point d’entrée lors de l’inscription de votre tâche en arrière-plan (**namespace.classname**).

    Dans cet exemple, le point d’entrée est ExampleBackgroundTaskNameSpace.ExampleBackgroundTaskClassName :

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ExampleBackgroundTaskClassName">
       <BackgroundTasks>
         <Task Type="" />
       </BackgroundTasks>
    </Extension>
</Extensions>
```

2.  Modifiez la liste de l’attribut Task Type pour indiquer le type d’inscription de tâche utilisé avec cette tâche en arrière-plan. Si la tâche en arrière-plan est inscrite avec plusieurs types de déclencheur, ajoutez des éléments Task et des attributs Type supplémentaires pour chacun d’eux.

    **Remarque**    Veillez à répertorier chacun des types de déclencheurs que vous utilisez, ou la tâche en arrière-plan ne s’inscrira pas avec les types de déclencheurs non déclarés (la méthode [**Register**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.register) échouera et lèvera une exception).

    Cet extrait de code montre que des déclencheurs d’événements système et des notifications Push sont utilisés :

```xml
<Extension Category="windows.backgroundTasks" EntryPoint="Tasks.BackgroundTaskClass">
    <BackgroundTasks>
        <Task Type="systemEvent" />
        <Task Type="pushNotification" />
    </BackgroundTasks>
</Extension>
```

### <a name="add-multiple-background-task-extensions"></a>Ajouter plusieurs extensions de tâche en arrière-plan

Répétez l’étape 2 pour chaque classe de tâche en arrière-plan supplémentaire inscrite par votre application.

L’exemple suivant représente l’élément Application complet de l’[exemple de tâche en arrière-plan](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask). Il illustre l’utilisation de deux classes de tâche en arrière-plan avec au total trois types de déclencheur. Copiez la section Extensions de cet exemple et modifiez-la si nécessaire pour déclarer des tâches en arrière-plan dans le manifeste de l’application.

```xml
<Applications>
    <Application Id="App"
      Executable="$targetnametoken$.exe"
      EntryPoint="BackgroundTask.App">
        <uap:VisualElements
          DisplayName="BackgroundTask"
          Square150x150Logo="Assets\StoreLogo-sdk.png"
          Square44x44Logo="Assets\SmallTile-sdk.png"
          Description="BackgroundTask"

          BackgroundColor="#00b2f0">
          <uap:LockScreen Notification="badgeAndTileText" BadgeLogo="Assets\smalltile-Windows-sdk.png" />
            <uap:SplashScreen Image="Assets\Splash-sdk.png" />
            <uap:DefaultTile DefaultSize="square150x150Logo" Wide310x150Logo="Assets\tile-sdk.png" >
                <uap:ShowNameOnTiles>
                    <uap:ShowOn Tile="square150x150Logo" />
                    <uap:ShowOn Tile="wide310x150Logo" />
                </uap:ShowNameOnTiles>
            </uap:DefaultTile>
        </uap:VisualElements>

      <Extensions>
        <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.SampleBackgroundTask">
          <BackgroundTasks>
            <Task Type="systemEvent" />
            <Task Type="timer" />
          </BackgroundTasks>
        </Extension>
        <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ServicingComplete">
          <BackgroundTasks>
            <Task Type="systemEvent"/>
          </BackgroundTasks>
        </Extension>
      </Extensions>
    </Application>
</Applications>
```

## <a name="declare-where-your-background-task-will-run"></a>Déclarer l’emplacement d’exécution de la tâche en arrière-plan

Vous pouvez spécifier l’emplacement d’exécution de vos tâches en arrière-plan :

* Par défaut, ils s’exécutent dans le processus de BackgroundTaskHost.exe.
* Dans le même processus que votre application de premier plan.
* Utilisez `ResourceGroup` pour placer plusieurs tâches en arrière-plan dans le même processus d’hébergement ou pour les séparer en différents processus.
* Utilisez `SupportsMultipleInstances` pour exécuter le processus en arrière-plan dans un nouveau processus qui obtient ses propres limites de ressources (mémoire, processeur) chaque fois qu’un nouveau déclencheur est déclenché.

### <a name="run-in-the-same-process-as-your-foreground-application"></a>Exécuter dans le même processus que votre application de premier plan

Voici un exemple de code XML déclarant une tâche en arrière-plan qui s’exécute dans le même processus que l’application au premier plan.

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="ExecModelTestBackgroundTasks.ApplicationTriggerTask">
        <BackgroundTasks>
            <Task Type="systemEvent" />
        </BackgroundTasks>
    </Extension>
</Extensions>
```

Lorsque vous spécifiez **entryPoint**, votre application reçoit un rappel de la méthode spécifiée lorsque le déclencheur est activé. Si vous ne spécifiez pas de **point d’entrée**, votre application reçoit le rappel via [OnBackgroundActivated ()](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onbackgroundactivated).  Pour plus d’informations, consultez [créer et inscrire une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md) .

### <a name="specify-where-your-background-task-runs-with-the-resourcegroup-attribute"></a>Spécifiez l’emplacement d’exécution de votre tâche en arrière-plan avec l’attribut groupe de ressources.

Voici un exemple de code XML déclarant une tâche en arrière-plan qui s’exécute dans un processus BackgroundTaskHost.exe distinct des autres instances de tâches en arrière-plan de la même application. Notez l’attribut `ResourceGroup`, qui définit quelles tâches en arrière-plan vont s’exécuter en même temps.

```xml
<Extensions>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.SessionConnectedTriggerTask" ResourceGroup="foo">
      <BackgroundTasks>
        <Task Type="systemEvent" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.TimeZoneTriggerTask" ResourceGroup="foo">
      <BackgroundTasks>
        <Task Type="systemEvent" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.TimerTriggerTask" ResourceGroup="bar">
      <BackgroundTasks>
        <Task Type="timer" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.ApplicationTriggerTask" ResourceGroup="bar">
      <BackgroundTasks>
        <Task Type="general" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.MaintenanceTriggerTask" ResourceGroup="foobar">
      <BackgroundTasks>
        <Task Type="general" />
      </BackgroundTasks>
    </Extension>
</Extensions>
```

### <a name="run-in-a-new-process-each-time-a-trigger-fires-with-the-supportsmultipleinstances-attribute"></a>Exécuter dans un nouveau processus chaque fois qu’un déclencheur se déclenche avec l’attribut SupportsMultipleInstances

Cet exemple déclare une tâche en arrière-plan qui s’exécute dans un nouveau processus qui obtient ses propres limites de ressources (mémoire et UC) chaque fois qu’un nouveau déclencheur est déclenché. Notez l’utilisation de `SupportsMultipleInstances` qui active ce comportement. Pour pouvoir utiliser cet attribut, vous devez cibler la version du kit de développement logiciel (SDK) « 10.0.15063 » (Windows 10 Creators Update) ou une version ultérieure.

```xml
<Package
    xmlns:uap4="https://schemas.microsoft.com/appx/manifest/uap/windows10/4"
    ...
    <Applications>
        <Application ...>
            ...
            <Extensions>
                <Extension Category="windows.backgroundTasks" EntryPoint="BackgroundTasks.TimerTriggerTask">
                    <BackgroundTasks uap4:SupportsMultipleInstances="True">
                        <Task Type="timer" />
                    </BackgroundTasks>
                </Extension>
            </Extensions>
        </Application>
    </Applications>
```

> [!NOTE]
> Vous ne pouvez pas spécifier `ResourceGroup` ou `ServerName` conjointement avec `SupportsMultipleInstances` .

## <a name="related-topics"></a>Rubriques connexes

* [Déboguer une tâche en arrière-plan](debug-a-background-task.md)
* [Inscrire une tâche en arrière-plan](register-a-background-task.md)
* [Recommandations relatives aux tâches en arrière-plan](guidelines-for-background-tasks.md)
