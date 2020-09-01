---
title: Créer et utiliser un service d’application
description: Apprenez à écrire une application de plateforme Windows universelle (UWP) pouvant fournir des services à d’autres applications UWP et à utiliser ces derniers.
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: communication entre l’application et l’application, communication entre processus, IPC, messagerie en arrière-plan, communication en arrière-plan, application vers application, app service
ms.date: 01/16/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: bd67c8a94ae7fc46956f5a6f7c3358e0e28965d2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158823"
---
# <a name="create-and-consume-an-app-service"></a>Créer et utiliser un service d’application

Les services d’application sont des applications UWP qui fournissent des services à d’autres applications UWP. Ils sont analogues aux services Web, sur un appareil. Un app service s’exécute en tant que tâche en arrière-plan dans l’application hôte et peut fournir son service à d’autres applications. Par exemple, un service d’application peut fournir un service d’analyse de code-barres que d’autres applications peuvent utiliser. Ou peut-être une suite d’applications d’entreprise dispose d’un service d’application de vérification orthographique commun qui est disponible pour les autres applications de la suite.  Les services d’application vous permettent de créer des services sans interface utilisateur que les applications peuvent appeler sur le même appareil et à partir de Windows 10, version 1607, sur des appareils distants.

À partir de Windows 10 version 1607, vous pouvez créer des services d’application qui s’exécutent dans le même processus que l’application hôte. Cet article se concentre sur la création et l’utilisation d’un app service qui s’exécute dans un processus d’arrière-plan distinct. Pour plus d’informations sur l’exécution d’un app service dans le même processus que le fournisseur, consultez [convertir un app service pour qu’il s’exécute dans le même processus que son application hôte](convert-app-service-in-process.md) .

Pour obtenir un exemple de code App service, consultez la page [exemples d’applications plateforme Windows universelle (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices).

## <a name="create-a-new-app-service-provider-project"></a>Créer un projet de fournisseur de service d’application

Dans la procédure décrite ici, nous allons créer tous les éléments dans une seule solution par souci de simplicité.

1. Dans Visual Studio 2015 ou version ultérieure, créez un projet d’application UWP et nommez-le **AppServiceProvider**.
    1. Sélectionnez **un fichier > nouveau > projet...** 
    2. Dans la boîte de dialogue **créer un nouveau projet** , sélectionnez **application vide (Windows universel) C#**. Il s’agit de l’application qui rend l’app service disponible pour d’autres applications UWP.
    3. Cliquez sur **suivant**, puis nommez le projet **AppServiceProvider**, choisissez un emplacement pour celui-ci, puis cliquez sur **créer**.

2. Lorsque vous êtes invité à sélectionner une **cible** et une **version minimale** pour le projet, sélectionnez au moins **10.0.14393**. Si vous souhaitez utiliser le nouvel attribut **SupportsMultipleInstances** , vous devez utiliser visual studio 2017 ou visual studio 2019, et cibler **10.0.15063** (**Windows 10 Creators Update**) ou version ultérieure.

<span id="appxmanifest"/>

## <a name="add-an-app-service-extension-to-packageappxmanifest"></a>Ajouter une extension App service à Package. appxmanifest

Dans le projet **AppServiceProvider** , ouvrez le fichier **Package. appxmanifest** dans un éditeur de texte : 

1. Cliquez avec le bouton droit sur le **Explorateur de solutions**. 
2. Sélectionnez **Ouvrir avec**. 
3. Sélectionnez **éditeur XML (texte)**. 

Ajoutez l' `AppService` extension suivante à l’intérieur de l' `<Application>` élément. Cet exemple publie le service `com.microsoft.inventory`, qui identifie cette application en tant que fournisseur de service d’application. Le service proprement dit est implémenté sous forme de tâche en arrière-plan. Le projet App service expose le service à d’autres applications. Nous vous recommandons d’utiliser un style de nom de domaine inverse pour le nom du service.

Notez que le `xmlns:uap4` préfixe d’espace de noms et l' `uap4:SupportsMultipleInstances` attribut sont uniquement valides si vous ciblez SDK Windows version 10.0.15063 ou ultérieure. Vous pouvez les supprimer en toute sécurité si vous ciblez des versions antérieures du SDK.

``` xml
<Package
    ...
    xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
    xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
    ...
    <Applications>
        <Application Id="AppServiceProvider.App"
          Executable="$targetnametoken$.exe"
          EntryPoint="AppServiceProvider.App">
          ...
          <Extensions>
            <uap:Extension Category="windows.appService" EntryPoint="MyAppService.Inventory">
              <uap3:AppService Name="com.microsoft.inventory" uap4:SupportsMultipleInstances="true"/>
            </uap:Extension>
          </Extensions>
          ...
        </Application>
    </Applications>
```

L' `Category` attribut identifie cette application en tant que fournisseur App service.

L' `EntryPoint` attribut identifie la classe qualifiée d’espace de noms qui implémente le service, que nous allons implémenter ensuite.

L' `SupportsMultipleInstances` attribut indique que chaque fois que l’app service est appelé, il doit s’exécuter dans un nouveau processus. Cette fonctionnalité n’est pas obligatoire, mais elle est disponible si vous avez besoin de cette fonctionnalité et que vous ciblez le kit de développement logiciel (SDK) 10.0.15063 (**Windows 10 Creators Update**) ou une version ultérieure. Elle doit également être précédée de l' `uap4` espace de noms.

## <a name="create-the-app-service"></a>Créer le service d’application

1.  Un app service peut être implémenté en tant que tâche en arrière-plan. Cela permet à une application de premier plan d’appeler un app service dans une autre application. Pour créer un app service en tant que tâche en arrière-plan, ajoutez un nouveau Windows Runtime projet de composant à la solution (**fichier &gt; Ajouter un &gt; nouveau projet**) nommé **MyAppService**. Dans la boîte de dialogue **Ajouter un nouveau projet** , sélectionnez **installé > Visual C# > composant Windows Runtime (Windows universel)**.
2.  Dans le projet **AppServiceProvider** , ajoutez une référence de projet à projet au nouveau projet **MyAppService** (dans la **Explorateur de solutions**, cliquez avec le bouton droit sur le projet **AppServiceProvider** > **solution ajouter**des  >  projets de**référence**  >  **Projects**  >  **Solution**, puis sélectionnez **MyAppService**  >  **OK**). Cette étape est essentielle, car si vous n’ajoutez pas la référence, app service ne se connecte pas au moment de l’exécution.
3.  Dans le projet **MyAppService** , ajoutez les instructions **using** suivantes au début de **Class1.cs**:
    ```cs
    using Windows.ApplicationModel.AppService;
    using Windows.ApplicationModel.Background;
    using Windows.Foundation.Collections;
    ```

4.  Renommez **Class1.cs** en **Inventory.cs**et remplacez le code stub de **Class1** par une nouvelle classe de tâche en arrière-plan nommée **Inventory**:

    ```cs
    public sealed class Inventory : IBackgroundTask
    {
        private BackgroundTaskDeferral backgroundTaskDeferral;
        private AppServiceConnection appServiceconnection;
        private String[] inventoryItems = new string[] { "Robot vacuum", "Chair" };
        private double[] inventoryPrices = new double[] { 129.99, 88.99 };

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // Get a deferral so that the service isn't terminated.
            this.backgroundTaskDeferral = taskInstance.GetDeferral();

            // Associate a cancellation handler with the background task.
            taskInstance.Canceled += OnTaskCanceled;

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            // This function is called when the app service receives a request.
        }

        private void OnTaskCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            if (this.backgroundTaskDeferral != null)
            {
                // Complete the service deferral.
                this.backgroundTaskDeferral.Complete();
            }
        }
    }
    ```

    C’est dans cette classe que le service d’application effectuera son travail.

    La méthode **Run** est appelée lors de la création de la tâche en arrière-plan. Étant donné que les tâches en arrière-plan sont terminées après la fin de l' **exécution** , le code extrait un report afin que la tâche en arrière-plan reste en attente de demandes. Un service d’application qui est implémenté en tant que tâche en arrière-plan reste actif pendant environ 30 secondes après réception d’un appel, sauf s’il est à nouveau appelé dans cette fenêtre de temps ou si un report est pris en charge. Si app service est implémenté dans le même processus que l’appelant, la durée de vie de l’app service est liée à la durée de vie de l’appelant.

    La durée de vie d’App service dépend de l’appelant :

    * Si l’appelant est au premier plan, la durée de vie d’App service est identique à celle de l’appelant.
    * Si l’appelant est en arrière-plan, l’exécution de l’app service est de 30 secondes. La mise en attente d’un report offre une durée supplémentaire de 5 secondes.

    **OnTaskCanceled** est appelé quand la tâche est annulée. La tâche est annulée lorsque l’application cliente supprime le [AppServiceConnection](/uwp/api/Windows.ApplicationModel.AppService.AppServiceConnection), que l’application cliente est interrompue, que le système d’exploitation est arrêté ou en veille ou que le système d’exploitation ne dispose pas des ressources nécessaires pour exécuter la tâche.

## <a name="write-the-code-for-the-app-service"></a>Écrire le code du service d’application

**OnRequestReceived** est l’emplacement du code pour App service. Remplacez le **OnRequestReceived** stub dans le **Inventory.cs** de **MyAppService**par le code de cet exemple. Ce code obtient un index pour un article en stock et le transmet au service avec une chaîne de commande pour récupérer le nom et le prix de l’article en stock spécifié. Pour vos propres projets, ajoutez le code de gestion des erreurs.

```cs
private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
{
    // Get a deferral because we use an awaitable API below to respond to the message
    // and we don't want this call to get canceled while we are waiting.
    var messageDeferral = args.GetDeferral();

    ValueSet message = args.Request.Message;
    ValueSet returnData = new ValueSet();

    string command = message["Command"] as string;
    int? inventoryIndex = message["ID"] as int?;

    if (inventoryIndex.HasValue &&
        inventoryIndex.Value >= 0 &&
        inventoryIndex.Value < inventoryItems.GetLength(0))
    {
        switch (command)
        {
            case "Price":
            {
                returnData.Add("Result", inventoryPrices[inventoryIndex.Value]);
                returnData.Add("Status", "OK");
                break;
            }

            case "Item":
            {
                returnData.Add("Result", inventoryItems[inventoryIndex.Value]);
                returnData.Add("Status", "OK");
                break;
            }

            default:
            {
                returnData.Add("Status", "Fail: unknown command");
                break;
            }
        }
    }
    else
    {
        returnData.Add("Status", "Fail: Index out of range");
    }

    try
    {
        // Return the data to the caller.
        await args.Request.SendResponseAsync(returnData);
    }
    catch (Exception e)
    {
        // Your exception handling code here.
    }
    finally
    {
        // Complete the deferral so that the platform knows that we're done responding to the app service call.
        // Note for error handling: this must be called even if SendResponseAsync() throws an exception.
        messageDeferral.Complete();
    }
}
```

Notez que **OnRequestReceived** est **asynchrone** , car nous effectuons un appel de méthode pouvant être attendu à [SendResponseAsync](/uwp/api/windows.applicationmodel.appservice.appservicerequest.sendresponseasync) dans cet exemple.

Un report est effectué afin que le service puisse utiliser des méthodes **Async** dans le gestionnaire **OnRequestReceived** . Elle garantit que l’appel à **OnRequestReceived** ne se termine pas tant que le traitement du message n’est pas terminé.  [SendResponseAsync](/uwp/api/windows.applicationmodel.appservice.appservicerequest.sendresponseasync) envoie le résultat à l’appelant. **SendResponseAsync** n’indique pas l’achèvement de l’appel. Il s’agit de l’achèvement du report qui signale à [SendMessageAsync](/uwp/api/windows.applicationmodel.appservice.appserviceconnection.sendmessageasync) que **OnRequestReceived** est terminé. L’appel à **SendResponseAsync** est encapsulé dans un bloc try/finally, car vous devez effectuer le report même si **SendResponseAsync** lève une exception.

Les services d’application utilisent des objets [ValueSet](/uwp/api/Windows.Foundation.Collections.ValueSet) pour échanger des informations. La taille des données que vous pouvez transmettre est limitée uniquement par les ressources système. Il n’y a aucune clé prédéfinie utilisable dans votre classe **ValueSet**. Vous devez déterminer quelles valeurs de clé vous allez utiliser pour définir le protocole de votre service d’application. L’appelant doit être écrit avec ce protocole à l’esprit. Dans cet exemple, nous avons choisi une clé nommée `Command` qui a une valeur qui indique si vous souhaitez que l’app service fournisse le nom de l’article d’inventaire ou son prix. L’index du nom de l’inventaire est stocké sous la `ID` clé. La valeur de retour est stockée sous la `Result` clé.

Une énumération [AppServiceClosedStatus](/uwp/api/Windows.ApplicationModel.AppService.AppServiceClosedStatus) est renvoyée à l’appelant pour indiquer si l’appel au service d’application a réussi ou échoué. Un exemple de la façon dont l’appel à App service peut échouer est si le système d’exploitation abandonne le point de terminaison de service, car ses ressources ont été dépassées. Vous pouvez renvoyer des informations d’erreur supplémentaires via la classe [ValueSet](/uwp/api/Windows.Foundation.Collections.ValueSet). Dans cet exemple, nous utilisons une clé nommée `Status` pour retourner des informations d’erreur plus détaillées à l’appelant.

L’appel à [SendResponseAsync](/uwp/api/windows.applicationmodel.appservice.appservicerequest.sendresponseasync) renvoie la classe [ValueSet](/uwp/api/Windows.Foundation.Collections.ValueSet) à l’appelant.

## <a name="deploy-the-service-app-and-get-the-package-family-name"></a>Déployer l’application de service et obtenir le nom de la famille de packages

Le fournisseur App service doit être déployé avant de pouvoir l’appeler à partir d’un client. Vous pouvez le déployer en sélectionnant **générer > déployer la solution** dans Visual Studio.

Vous aurez également besoin du nom de la famille de packages du fournisseur App service pour pouvoir l’appeler. Vous pouvez l’obtenir en ouvrant le fichier **Package. appxmanifest** du projet **AppServiceProvider** dans le mode concepteur (double-cliquez dessus dans le **Explorateur de solutions**). Sélectionnez l’onglet **Packaging** , copiez la valeur en regard de nom de la **famille de packages**, puis collez-la à un emplacement comme le bloc-notes pour l’instant.

## <a name="write-a-client-to-call-the-app-service"></a>Écrire un client pour appeler le service d’application

1.  Ajoutez un nouveau projet d’application universelle Windows vide à la solution avec **fichier &gt; Ajouter un &gt; nouveau projet**. Dans la boîte de dialogue **Ajouter un nouveau projet** , sélectionnez **installé > Visual C# > application vide (Windows universel)** et nommez-le **ClientApp**.

2.  Dans le projet **ClientApp** , ajoutez l’instruction **using** suivante en haut de **MainPage.Xaml.cs**:
    ```cs
    using Windows.ApplicationModel.AppService;
    ```

3.  Ajoutez une zone de texte appelée **TextBox** et un bouton à **MainPage. Xaml**.

4.  Ajoutez un gestionnaire de clic de bouton pour le bouton appelé **button_Click**, puis ajoutez le mot clé **Async** à la signature du gestionnaire de bouton.

5. Remplacez le stub de votre gestionnaire d’événement de clic pour le bouton par le code suivant. Veillez à inclure la déclaration de champ `inventoryService`.
    ```cs
   private AppServiceConnection inventoryService;

   private async void button_Click(object sender, RoutedEventArgs e)
   {
       // Add the connection.
       if (this.inventoryService == null)
       {
           this.inventoryService = new AppServiceConnection();

           // Here, we use the app service name defined in the app service 
           // provider's Package.appxmanifest file in the <Extension> section.
           this.inventoryService.AppServiceName = "com.microsoft.inventory";

           // Use Windows.ApplicationModel.Package.Current.Id.FamilyName 
           // within the app service provider to get this value.
           this.inventoryService.PackageFamilyName = "Replace with the package family name";

           var status = await this.inventoryService.OpenAsync();

           if (status != AppServiceConnectionStatus.Success)
           {
               textBox.Text= "Failed to connect";
               this.inventoryService = null;
               return;
           }
       }

       // Call the service.
       int idx = int.Parse(textBox.Text);
       var message = new ValueSet();
       message.Add("Command", "Item");
       message.Add("ID", idx);
       AppServiceResponse response = await this.inventoryService.SendMessageAsync(message);
       string result = "";

       if (response.Status == AppServiceResponseStatus.Success)
       {
           // Get the data  that the service sent to us.
           if (response.Message["Status"] as string == "OK")
           {
               result = response.Message["Result"] as string;
           }
       }

       message.Clear();
       message.Add("Command", "Price");
       message.Add("ID", idx);
       response = await this.inventoryService.SendMessageAsync(message);

       if (response.Status == AppServiceResponseStatus.Success)
       {
           // Get the data that the service sent to us.
           if (response.Message["Status"] as string == "OK")
           {
               result += " : Price = " + response.Message["Result"] as string;
           }
       }

       textBox.Text = result;
   }
   ```
    
    Remplacez le nom de la famille de packages dans la ligne `this.inventoryService.PackageFamilyName = "Replace with the package family name";` par le nom de la famille de packages du projet **AppServiceProvider** que vous avez obtenu dans [déployer l’application de service et obtenez le nom](#deploy-the-service-app-and-get-the-package-family-name)de la famille de packages.

    > [!NOTE]
    > Veillez à coller le littéral de chaîne au lieu de le placer dans une variable. Elle ne fonctionnera pas si vous utilisez une variable.

    Le code établit tout d’abord une connexion avec le service d’application. La connexion reste ouverte jusqu’à ce que vous supprimiez `this.inventoryService` . Le nom du service d’application doit correspondre à l' `AppService` attribut de l’élément `Name` que vous avez ajouté au fichier **Package. Appxmanifest** du projet **AppServiceProvider** . Dans cet exemple, il s’agit de `<uap3:AppService Name="com.microsoft.inventory"/>`.

    Un [ValueSet](/uwp/api/Windows.Foundation.Collections.ValueSet) nommé `message` est créé pour spécifier la commande que nous voulons envoyer à App service. L’exemple de service d’application attend une commande pour indiquer laquelle des deux actions effectuer. Nous obtenons l’index à partir de la zone de texte dans l’application cliente, puis appelons le service avec la `Item` commande pour récupérer la description de l’élément. Ensuite, nous effectuons l’appel avec la `Price` commande pour récupérer le prix de l’élément. Le texte du bouton est défini en fonction du résultat.

    Étant donné que [AppServiceResponseStatus](/uwp/api/Windows.ApplicationModel.AppService.AppServiceResponseStatus) indique uniquement si le système d’exploitation a réussi à connecter l’appel à App service, nous vérifions la `Status` clé dans le [ValueSet](/uwp/api/Windows.Foundation.Collections.ValueSet) que nous recevons de l’app service pour vous assurer qu’il a pu traiter la demande.

6. Définissez le projet **ClientApp** comme projet de démarrage (cliquez dessus avec le bouton droit dans le **Explorateur de solutions**  >  **définir comme projet de démarrage**) et exécutez la solution. Entrez le chiffre 1 dans la zone de texte et cliquez sur le bouton. Le service doit renvoyer « Chair : Price = 88.99 ».

    ![exemple d’application affichant chair price=88.99](images/appserviceclientapp.png)

Si l’appel App service échoue, vérifiez les éléments suivants dans le projet **ClientApp** :

1.  Vérifiez que le nom de la famille de packages affecté à la connexion du service d’inventaire correspond au nom de la famille de packages de l’application **AppServiceProvider** . Consultez la ligne dans le **bouton de \_ clic** avec `this.inventoryService.PackageFamilyName = "...";` .
2.  Dans ** \_ cliquer sur un bouton**, vérifiez que le nom App service affecté à la connexion au service d’inventaire correspond au nom de l’app service dans le fichier **Package. appxmanifest** de **AppServiceProvider**. Voir `this.inventoryService.AppServiceName = "com.microsoft.inventory";`.
3.  Assurez-vous que l’application **AppServiceProvider** a été déployée. (Dans le **Explorateur de solutions**, cliquez avec le bouton droit sur la solution et choisissez **déployer la solution**).

## <a name="debug-the-app-service"></a>Déboguer le service d’application

1.  Assurez-vous que la solution est déployée avant le débogage, car l’application du fournisseur App service doit être déployée pour que le service puisse être appelé. (Dans Visual Studio, **Générer &gt; Déployer la solution**).
2.  Dans le **Explorateur de solutions**, cliquez avec le bouton droit sur le projet **AppServiceProvider** et choisissez **Propriétés**. Dans l’onglet **Déboguer**, définissez **Action de démarrage** sur **Ne pas lancer, mais déboguer mon code au démarrage**. (Remarque : Si vous utilisez C++ pour implémenter votre fournisseur App service, dans l’onglet **débogage** , vous pouvez modifier l' **application de lancement** en **non**).
3.  Dans le projet **MyAppService** , dans le fichier **Inventory.cs** , définissez un point d’arrêt dans **OnRequestReceived**.
4.  Définissez le projet **AppServiceProvider** comme projet de démarrage et appuyez sur **F5**.
5.  Démarrez **ClientApp** à partir du menu Démarrer (pas à partir de Visual Studio).
6.  Entrez le chiffre 1 dans la zone de texte et appuyez sur le bouton. Le débogueur s’arrête dans l’appel au service d’application sur le point d’arrêt défini dans votre service d’application.

## <a name="debug-the-client"></a>Déboguer le client

1.  Suivez les instructions de l’étape précédente pour déboguer le client qui appelle App service.
2.  Lancez **ClientApp** à partir du menu Démarrer.
3.  Attachez le débogueur au processus de **ClientApp.exe** (pas au processus **ApplicationFrameHost.exe** ). (Dans Visual Studio, choisissez **Déboguer &gt; Attacher au processus…**.)
4.  Dans le projet **ClientApp** , définissez un point d’arrêt dans le ** \_ clic sur un bouton**.
5.  Les points d’arrêt à la fois dans le client et dans App service sont alors atteints lorsque vous entrez le numéro 1 dans la zone de texte de **ClientApp** et cliquent sur le bouton.

## <a name="general-app-service-troubleshooting"></a>Résolution des problèmes généraux d’App service

Si vous rencontrez un état **AppUnavailable** après avoir essayé de vous connecter à un app service, vérifiez les éléments suivants :

- Assurez-vous que le projet App Service Provider et le projet App service sont déployés. Les deux doivent être déployés avant l’exécution du client, car sinon, le client n’aura rien à connecter à. Vous pouvez déployer à partir de Visual Studio à l’aide de la solution de déploiement de **Build**  >  **Deploy Solution**.
- Dans le **Explorateur de solutions**, assurez-vous que votre projet de fournisseur App service a une référence entre projets au projet qui implémente le service d’application.
- Vérifiez que l' `<Extensions>` entrée et ses éléments enfants ont été ajoutés au fichier **Package. appxmanifest** appartenant au projet App Service Provider comme indiqué ci-dessus dans [Ajouter une extension App service à Package. appxmanifest](#appxmanifest).
- Vérifiez que la chaîne [AppServiceConnection. AppServiceName](/uwp/api/windows.applicationmodel.appservice.appserviceconnection.appservicename) de votre client qui appelle le fournisseur App service correspond à la valeur `<uap3:AppService Name="..." />` spécifiée dans le fichier **Package. appxmanifest** du projet du fournisseur App service.
- Vérifiez que [AppServiceConnection. PackageFamilyName](/uwp/api/windows.applicationmodel.appservice.appserviceconnection.packagefamilyname) correspond au nom de la famille de packages du composant fournisseur App service comme indiqué ci-dessus dans [Ajouter une extension App service au package. appxmanifest](#appxmanifest)
- Pour les services d’application hors processus tels que celui utilisé dans cet exemple, vérifiez que le `EntryPoint` spécifié dans l' `<uap:Extension ...>` élément du fichier **Package. appxmanifest** de votre projet App Service Provider correspond à l’espace de noms et au nom de classe de la classe publique qui implémente [IBackgroundTask](/uwp/api/windows.applicationmodel.background.ibackgroundtask) dans votre projet App service.

### <a name="troubleshoot-debugging"></a>Résoudre les problèmes de débogage

Si le débogueur ne s’arrête pas aux points d’arrêt dans vos projets App Service Provider ou App service, vérifiez les éléments suivants :

- Assurez-vous que le projet App Service Provider et le projet App service sont déployés. Les deux doivent être déployés avant l’exécution du client. Vous pouvez les déployer à partir de Visual Studio à l’aide de la solution de déploiement de **Build**  >  **Deploy Solution**.
- Assurez-vous que le projet que vous souhaitez déboguer est défini comme projet de démarrage et que les propriétés de débogage de ce projet sont définies de manière à ne pas exécuter le projet lorsque la touche **F5** est enfoncée. Cliquez avec le bouton droit sur le projet, puis cliquez sur **Propriétés**et **Déboguer** (ou **débogage** en C++). En C#, remplacez l' **action de démarrage** par **ne pas lancer, mais déboguez mon code au démarrage**. Dans C++, définissez **lancer l’application** sur **non**.

## <a name="remarks"></a>Remarques

Cet exemple fournit une introduction à la création d’un app service qui s’exécute en tant que tâche en arrière-plan et l’appelle à partir d’une autre application. Voici les points clés à noter :

* Créez une tâche en arrière-plan pour héberger le service d’application.
* Ajoutez l' `windows.appService` extension au fichier **Package. appxmanifest** du fournisseur App service.
* Obtenez le nom de la famille de packages du fournisseur App service afin de pouvoir vous y connecter à partir de l’application cliente.
* Ajoutez une référence entre projets du projet App Service Provider au projet App service.
* Utilisez [Windows. ApplicationModel. AppService. AppServiceConnection](/uwp/api/Windows.ApplicationModel.AppService.AppServiceConnection) pour appeler le service.

## <a name="full-code-for-myappservice"></a>Code complet pour MyAppService

```cs
using System;
using Windows.ApplicationModel.AppService;
using Windows.ApplicationModel.Background;
using Windows.Foundation.Collections;

namespace MyAppService
{
    public sealed class Inventory : IBackgroundTask
    {
        private BackgroundTaskDeferral backgroundTaskDeferral;
        private AppServiceConnection appServiceconnection;
        private String[] inventoryItems = new string[] { "Robot vacuum", "Chair" };
        private double[] inventoryPrices = new double[] { 129.99, 88.99 };

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // Get a deferral so that the service isn't terminated.
            this.backgroundTaskDeferral = taskInstance.GetDeferral();

            // Associate a cancellation handler with the background task.
            taskInstance.Canceled += OnTaskCanceled;

            // Retrieve the app service connection and set up a listener for incoming app service requests.
            var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
            appServiceconnection = details.AppServiceConnection;
            appServiceconnection.RequestReceived += OnRequestReceived;
        }

        private async void OnRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            // Get a deferral because we use an awaitable API below to respond to the message
            // and we don't want this call to get canceled while we are waiting.
            var messageDeferral = args.GetDeferral();

            ValueSet message = args.Request.Message;
            ValueSet returnData = new ValueSet();

            string command = message["Command"] as string;
            int? inventoryIndex = message["ID"] as int?;

            if (inventoryIndex.HasValue &&
                 inventoryIndex.Value >= 0 &&
                 inventoryIndex.Value < inventoryItems.GetLength(0))
            {
                switch (command)
                {
                    case "Price":
                        {
                            returnData.Add("Result", inventoryPrices[inventoryIndex.Value]);
                            returnData.Add("Status", "OK");
                            break;
                        }

                    case "Item":
                        {
                            returnData.Add("Result", inventoryItems[inventoryIndex.Value]);
                            returnData.Add("Status", "OK");
                            break;
                        }

                    default:
                        {
                            returnData.Add("Status", "Fail: unknown command");
                            break;
                        }
                }
            }
            else
            {
                returnData.Add("Status", "Fail: Index out of range");
            }

            // Return the data to the caller.
            await args.Request.SendResponseAsync(returnData);

            // Complete the deferral so that the platform knows that we're done responding to the app service call.
            // Note for error handling: this must be called even if SendResponseAsync() throws an exception.
            messageDeferral.Complete();
        }


        private void OnTaskCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            if (this.backgroundTaskDeferral != null)
            {
                // Complete the service deferral.
                this.backgroundTaskDeferral.Complete();
            }
        }
    }
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Convertir un service d’application pour qu’il s’exécute dans le même processus que son application hôte](convert-app-service-in-process.md)
* [Prendre en charge votre application avec des tâches en arrière-plan](support-your-app-with-background-tasks.md)
* [App service code, exemple (C#, C++ et VB)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices)