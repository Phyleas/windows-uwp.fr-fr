---
ms.assetid: 67a46812-881c-404b-9f3b-c6786f39e72b
title: Personnaliser le workflow d’impression
description: Créez des expériences de flux de travail d’impression personnalisées pour répondre aux besoins de votre organisation.
ms.date: 07/03/2020
ms.topic: article
keywords: Windows 10, UWP, impression
ms.localizationpriority: medium
ms.openlocfilehash: 2bcfc5a24ff9202840b59166de625ac619c05670
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493394"
---
# <a name="customize-the-print-workflow"></a>Personnaliser le workflow d’impression

## <a name="overview"></a>Vue d’ensemble

Les développeurs peuvent personnaliser l’expérience de flux de travail d’impression via l’utilisation d’une application de flux de travail d’impression. Les applications de flux de travail d’impression sont des applications UWP qui étendent les fonctionnalités des [applications de périphériques Microsoft Store (WSDAs)](https://docs.microsoft.com/windows-hardware/drivers/devapps/). il est donc utile de vous familiariser avec WSDAs avant de continuer.

Tout comme dans le cas de WSDAs, lorsque l’utilisateur d’une application source choisit d’imprimer un texte et parcourt la boîte de dialogue d’impression, le système vérifie si une application de workflow est associée à cette imprimante. Si c’est le cas, l’application de flux de travail d’impression démarre (principalement en tant que tâche en arrière-plan ; en savoir plus sur ce qui suit). Une application de flux de travail est en mesure de modifier à la fois le ticket d’impression (le document XML qui configure les paramètres de l’imprimante pour la tâche d’impression en cours) et le contenu XPS réel à imprimer. Il peut éventuellement exposer cette fonctionnalité à l’utilisateur en lançant une interface utilisateur au milieu du processus. Après avoir effectué son travail, il transmet le contenu d’impression et le ticket d’impression au pilote.

Comme il implique des composants d’arrière-plan et de premier plan, et parce qu’il est associé de façon fonctionnelle à d’autres applications, une application de flux de travail d’impression peut être plus compliquée à implémenter que d’autres catégories d’applications UWP. Nous vous recommandons d’examiner l' [exemple d’application de workflow](https://github.com/Microsoft/print-oem-samples) tout en lisant ce guide pour mieux comprendre comment les différentes fonctionnalités peuvent être implémentées. Certaines fonctionnalités, telles que les vérifications des erreurs et la gestion de l’interface utilisateur, sont absentes de ce guide pour des raisons de simplicité.

## <a name="getting-started"></a>Prise en main

L’application de flux de travail doit indiquer son point d’entrée au système d’impression afin qu’elle puisse être lancée à l’heure appropriée. Pour ce faire, insérez la déclaration suivante dans l' `Application/Extensions` élément du fichier *Package. appxmanifest* du projet UWP.

```xml
<uap:Extension Category="windows.printWorkflowBackgroundTask"  
    EntryPoint="WFBackgroundTasks.WfBackgroundTask" />
```

> [!IMPORTANT]
> Il existe de nombreux scénarios dans lesquels la personnalisation de l’impression ne nécessite pas d’entrée utilisateur. Pour cette raison, les applications de flux de travail d’impression s’exécutent en tant que tâches en arrière-plan par défaut.

Si une application de workflow est associée à l’application source qui a démarré le travail d’impression (voir la section suivante pour obtenir des instructions à ce propos), le système d’impression examine ses fichiers manifeste pour un point d’entrée de tâche en arrière-plan.

## <a name="do-background-work-on-the-print-ticket"></a>Effectuer un travail en arrière-plan sur le ticket d’impression

La première chose que fait le système d’impression avec l’application de flux de travail est d’activer sa tâche en arrière-plan (dans ce cas, la `WfBackgroundTask` classe dans l' `WFBackgroundTasks` espace de noms). Dans la méthode de la tâche en arrière-plan `Run` , vous devez effectuer un cast des détails du déclencheur de la tâche en instance **[PrintWorkflowTriggerDetails](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowtriggerdetails)** . Cela fournira les fonctionnalités spéciales pour une tâche d’arrière-plan du flux de travail d’impression. Il expose la propriété **[PrintWorkflowSession](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowtriggerdetails.PrintWorkflowSession)** , qui est une instance de **[PrintWorkFlowBackgroundSession](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession)**. Imprimer les classes de session de workflow-à la fois les variétés d’arrière-plan et de premier plan : contrôle les étapes séquentielles de l’application de flux de travail d’impression.

Enregistrez ensuite les méthodes de gestionnaire pour les deux événements déclenchés par cette classe de session. Vous allez définir ces méthodes ultérieurement.

```csharp
public void Run(IBackgroundTaskInstance taskInstance) {
    // Take out a deferral here and complete once all the callbacks are done
    runDeferral = taskInstance.GetDeferral();

    // Associate a cancellation handler with the background task.
    taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);

    // cast the task's trigger details as PrintWorkflowTriggerDetails
    PrintWorkflowTriggerDetails workflowTriggerDetails = taskInstance.TriggerDetails as PrintWorkflowTriggerDetails;

    // Get the session manager, which is unique to this print job
    PrintWorkflowBackgroundSession sessionManager = workflowTriggerDetails.PrintWorkflowSession;

    // add the event handler callback routines
    sessionManager.SetupRequested += OnSetupRequested;
    sessionManager.Submitted += OnXpsOMPrintSubmitted;

    // Allow the event source to start
    // This call blocks until all of the workflow callbacks complete
    sessionManager.Start();
}
```

Lorsque la `Start` méthode est appelée, le gestionnaire de sessions déclenche d’abord l’événement **[SetupRequested](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession.SetupRequested)** . Cet événement expose des informations générales sur la tâche d’impression, ainsi que le ticket d’impression. À ce niveau, le ticket d’impression peut être modifié en arrière-plan.

```csharp
private void OnSetupRequested(PrintWorkflowBackgroundSession sessionManager, PrintWorkflowBackgroundSetupRequestedEventArgs printTaskSetupArgs) {
    // Take out a deferral here and complete once all the callbacks are done
    Deferral setupRequestedDeferral = printTaskSetupArgs.GetDeferral();

    // Get general information about the source application, print job title, and session ID
    string sourceApplicationName = printTaskSetupArgs.Configuration.SourceAppDisplayName;
    string jobTitle = printTaskSetupArgs.Configuration.JobTitle;
    string sessionId = printTaskSetupArgs.Configuration.SessionId;

    // edit the print ticket
    WorkflowPrintTicket printTicket = printTaskSetupArgs.GetUserPrintTicketAsync();

    // ...
```

Il est important, dans la gestion du **SetupRequested** , que l’application détermine si un composant de premier plan doit être lancé. Cela peut dépendre d’un paramètre précédemment enregistré dans le stockage local ou d’un événement qui s’est produit lors de la modification du ticket d’impression, ou il peut s’agir d’un paramètre statique de votre application spécifique.

```csharp
// ...

if (UIrequested) {
    printTaskSetupArgs.SetRequiresUI();

    // Any data that is to be passed to the foreground task must be stored the app's local storage.
    // It should be prefixed with the sourceApplicationName string and the SessionId string, so that
    // it can be identified as pertaining to this workflow app session.
}

// Complete the deferral taken out at the start of OnSetupRequested
setupRequestedDeferral.Complete();
```

## <a name="do-foreground-work-on-the-print-job-optional"></a>Effectuer un travail de premier plan sur le travail d’impression (facultatif)

Si la méthode **[SetRequiresUI](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsetuprequestedeventargs.SetRequiresUI)** a été appelée, le système d’impression examinera le fichier manifeste pour le point d’entrée vers l’application de premier plan. L' `Application/Extensions` élément de votre fichier *Package. appxmanifest* doit contenir les lignes suivantes. Remplacez la valeur de `EntryPoint` par le nom de l’application de premier plan.

```xml
<uap:Extension Category="windows.printWorkflowForegroundTask"  
    EntryPoint="MyWorkFlowForegroundApp.App" />
```

Ensuite, le système d’impression appelle la méthode **OnActivated** pour le point d’entrée de l’application donné. Dans la méthode **OnActivated** de son fichier _app.Xaml.cs_ , l’application de flux de travail doit vérifier le type d’activation pour vérifier qu’il s’agit d’une activation de flux de travail. Si c’est le cas, l’application de flux de travail peut effectuer un cast des arguments d’activation vers un objet **[PrintWorkflowUIActivatedEventArgs](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowuiactivatedeventargs)** , qui expose un objet **[PrintWorkflowForegroundSession](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession)** en tant que propriété. Cet objet, comme son équivalent d’arrière-plan dans la section précédente, contient des événements déclenchés par le système d’impression et vous pouvez assigner des gestionnaires à ces objets. Dans ce cas, la fonctionnalité de gestion des événements sera implémentée dans une classe distincte appelée `WorkflowPage` .

Tout d’abord, dans le fichier _app.Xaml.cs_ :

```csharp
protected override void OnActivated(IActivatedEventArgs args){

    if (args.Kind == ActivationKind.PrintWorkflowForegroundTask) {

        // the app should instantiate a new UI view so that it can properly handle the case when
        // several print jobs are active at the same time.
        Frame rootFrame = new Frame();
        if (null == Window.Current.Content)
        {
            rootFrame.Navigate(typeof(WorkflowPage));
            Window.Current.Content = rootFrame;
        }

        // Get the main page
        WorkflowPage workflowPage = (WorkflowPage)rootFrame.Content;

        // Make sure the page knows it's handling a foreground task activation
        workflowPage.LaunchType = WorkflowPage.WorkflowPageLaunchType.ForegroundTask;

        // Get the activation arguments
        PrintWorkflowUIActivatedEventArgs printTaskUIEventArgs = args as PrintWorkflowUIActivatedEventArgs;

        // Get the session manager
        PrintWorkflowForegroundSession taskSessionManager = printTaskUIEventArgs.PrintWorkflowSession;

        // Add the callback handlers - these methods are in the workflowPage class
        taskSessionManager.SetupRequested += workflowPage.OnSetupRequested;
        taskSessionManager.XpsDataAvailable += workflowPage.OnXpsDataAvailable;

        // start raising the print workflow events
        taskSessionManager.Start();
    }
}
```

Une fois que l’interface utilisateur a attaché des gestionnaires d’événements et que la méthode **OnActivated** s’est arrêtée, le système d’impression déclenche l’événement **[SetupRequested](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession.SetupRequested)** à gérer par l’interface utilisateur. Cet événement fournit les mêmes données que l’événement de configuration de tâche en arrière-plan, y compris les informations sur le travail d’impression et le document de ticket d’impression, mais sans la possibilité de demander le lancement d’une interface utilisateur supplémentaire. Dans le fichier _WorkflowPage.Xaml.cs_ :

```csharp
internal void OnSetupRequested(PrintWorkflowForegroundSession sessionManager, PrintWorkflowForegroundSetupRequestedEventArgs printTaskSetupArgs) {
    // If anything asynchronous is going to be done, you need to take out a deferral here,
    // since otherwise the next callback happens once this one exits, which may be premature
    Deferral setupRequestedDeferral = printTaskSetupArgs.GetDeferral();

    // Get information about the source application, print job title, and session ID
    string sourceApplicationName = printTaskSetupArgs.Configuration.SourceAppDisplayName;
    string jobTitle = printTaskSetupArgs.Configuration.JobTitle;
    string sessionId = printTaskSetupArgs.Configuration.SessionId;
    // the following string should be used when storing data that pertains to this workflow session
    // (such as user input data that is meant to change the print content later on)
    string localStorageVariablePrefix = string.Format("{0}::{1}::", sourceApplicationName, sessionID);

    try
    {
        // receive and store user input
        // ...
    }
    catch (Exception ex)
    {
        string errorMessage = ex.Message;
        Debug.WriteLine(errorMessage);
    }
    finally
    {
        // Complete the deferral taken out at the start of OnSetupRequested
        setupRequestedDeferral.Complete();
    }
}
```

Ensuite, le système d’impression déclenche l’événement **[XpsDataAvailable](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession.XpsDataAvailable)** pour l’interface utilisateur. Dans le gestionnaire de cet événement, l’application de workflow peut accéder à toutes les données disponibles pour l’événement d’installation et peut également lire directement les données XPS, soit en tant que flux d’octets bruts, soit en tant que modèle objet. L’accès aux données XPS permet à l’interface utilisateur de fournir des services d’aperçu avant impression et de fournir à l’utilisateur des informations supplémentaires sur les opérations que l’application de flux de travail exécutera sur les données.

Dans le cadre de ce gestionnaire d’événements, l’application de flux de travail doit acquérir un objet report s’il continue à interagir avec l’utilisateur. Sans Report, le système d’impression considère que la tâche d’interface utilisateur se termine lorsque le gestionnaire d’événements **XpsDataAvailable** s’arrête ou lorsqu’il appelle une méthode Async. Lorsque l’application a rassemblé toutes les informations requises de l’interaction de l’utilisateur avec l’interface utilisateur, elle doit effectuer le report pour que le système d’impression puisse avancer.

```csharp
internal async void OnXpsDataAvailable(PrintWorkflowForegroundSession sessionManager, PrintWorkflowXpsDataAvailableEventArgs printTaskXpsAvailableEventArgs)
{
    // Take out a deferral
    Deferral xpsDataAvailableDeferral = printTaskXpsAvailableEventArgs.GetDeferral();

    SpoolStreamContent xpsStream = printTaskXpsAvailableEventArgs.Operation.XpsContent.GetSourceSpoolDataAsStreamContent();

    IInputStream inputStream = xpsStream.GetInputSpoolStream();

    using (var inputReader = new Windows.Storage.Streams.DataReader(inputStream))
    {
        // Read the XPS data from input stream
        byte[] xpsData = new byte[inputReader.UnconsumedBufferLength];
        while (inputReader.UnconsumedBufferLength > 0)
        {
            inputReader.ReadBytes(xpsData);
            // Do something with the XPS data, e.g. preview
            // ...
        }
    }

    // Complete the deferral taken out at the start of this method
    xpsDataAvailableDeferral.Complete();
}
```

En outre, l’instance **[PrintWorkflowSubmittedOperation](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedoperation)** exposée par les arguments d’événement donne la possibilité d’annuler le travail d’impression ou d’indiquer que le travail a réussi, mais qu’aucun travail d’impression de sortie n’est nécessaire. Pour ce faire, appelez la méthode **[Complete](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedoperation.Complete)** avec une valeur **[PrintWorkflowSubmittedStatus](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedstatus)** .

> [!NOTE]
> Si l’application de workflow annule le travail d’impression, il est vivement recommandé de fournir une notification Toast indiquant la raison pour laquelle le travail a été annulé.

## <a name="do-final-background-work-on-the-print-content"></a>Effectuer le travail d’arrière-plan final sur le contenu d’impression

Une fois que l’interface utilisateur a terminé le report dans l’événement **PrintTaskXpsDataAvailable** (ou si l’étape de l’interface utilisateur a été contournée), le système d’impression déclenche l’événement **[soumis](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession.Submitted)** pour la tâche en arrière-plan. Dans le gestionnaire de cet événement, l’application de workflow peut accéder à toutes les données fournies par l’événement **XpsDataAvailable** . Toutefois, contrairement à l’un des événements précédents, l' **envoi** fournit également un accès en *écriture* au contenu du travail d’impression final via une instance **[PrintWorkflowTarget](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowtarget)** .

L’objet qui est utilisé pour spouler les données pour l’impression finale varie selon que les données sources sont accessibles en tant que flux d’octets bruts ou en tant que modèle objet XPS. Lorsque l’application de workflow accède aux données sources via un flux d’octets, un flux d’octets de sortie est fourni pour écrire les données finales du travail dans. Lorsque l’application de workflow accède aux données sources via le modèle objet, un enregistreur de documents est fourni pour écrire des objets dans le travail de sortie. Dans les deux cas, l’application de flux de travail doit lire toutes les données sources, modifier les données requises et écrire les données modifiées dans la cible de sortie.

Lorsque la tâche en arrière-plan termine l’écriture des données, elle doit appeler **Complete** sur l’objet **PrintWorkflowSubmittedOperation** correspondant. Une fois que l’application de workflow a terminé cette étape et que le gestionnaire d’événements **soumis** se termine, la session de workflow est fermée et l’utilisateur peut surveiller l’état du travail d’impression final à l’aide des boîtes de dialogue d’impression standard.

## <a name="final-steps"></a>Dernières étapes

### <a name="register-the-print-workflow-app-to-the-printer"></a>Inscrire l’application de flux de travail d’impression sur l’imprimante

Votre application de flux de travail est associée à une imprimante utilisant le même type d’envoi de fichier de métadonnées que pour WSDAs. En fait, une seule application UWP peut agir à la fois comme une application de flux de travail et un WSDA qui fournit des fonctionnalités d’impression des paramètres de tâche. Suivez les [étapes WSDA correspondantes pour créer l’Association de métadonnées](https://docs.microsoft.com/windows-hardware/drivers/devapps/step-2--create-device-metadata).

La différence réside dans le fait que lorsque les WSDAs sont automatiquement activés pour l’utilisateur (l’application est toujours lancée lorsque cet utilisateur imprime sur l’appareil associé), les applications de flux de travail ne le sont pas. Elles ont une stratégie distincte qui doit être définie.

### <a name="set-the-workflow-apps-policy"></a>Définir la stratégie de l’application de workflow

La stratégie d’application de workflow est définie par les commandes PowerShell sur l’appareil qui doit exécuter l’application de Workflow. Les commandes Set-Printer, Add-Printer (port existant) et Add-Printer (nouveau port WSD) sont modifiées pour permettre la définition des stratégies de Workflow.

* `Disabled`: Les applications de flux de travail ne sont pas activées.
* `Uninitialized`: Les applications de workflow sont activées si le workflow DCA est installé dans le système. Si l’application n’est pas installée, l’impression continuera.
* `Enabled`: Le contrat de workflow sera activé si le processus DCA de workflow est installé dans le système. Si l’application n’est pas installée, l’impression échoue.

La commande suivante rend l’application de flux de travail obligatoire sur l’imprimante spécifiée.

```Powershell
Set-Printer –Name "Microsoft XPS Document Writer" -WorkflowPolicy Enabled
```

Un utilisateur local peut exécuter cette stratégie sur une imprimante locale, ou, pour l’implémentation d’entreprise, l’administrateur de l’imprimante peut exécuter cette stratégie sur le serveur d’impression. La stratégie est alors synchronisée avec toutes les connexions clientes. L’administrateur de l’imprimante peut utiliser cette stratégie chaque fois qu’une nouvelle imprimante est ajoutée.

## <a name="see-also"></a>Voir aussi

[Exemple d’application de workflow](https://github.com/Microsoft/print-oem-samples)

[Espace de noms Windows. Graphics. Printing. Workflow](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow)
