---
title: Lancer une application pour obtenir des résultats
description: Découvrez comment démarrer une application à partir d’une autre, et échanger des données entre les deux. On parle de démarrage d’une application pour afficher les résultats.
ms.assetid: AFC53D75-B3DD-4FF6-9FC0-9335242EE327
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: fa018920f069c0b4f1d963c6cdfd3213df08fb45
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158764"
---
# <a name="launch-an-app-for-results"></a>Lancer une application pour obtenir des résultats




**API importantes**

-   [**LaunchUriForResultsAsync**](/uwp/api/windows.system.launcher.launchuriforresultsasync)
-   [**ValueSet**](/uwp/api/Windows.Foundation.Collections.ValueSet)

Découvrez comment démarrer une application à partir d’une autre, et échanger des données entre les deux. On parle de *démarrage d’une application pour afficher les résultats*. L’exemple suivant vous montre comment utiliser [**LaunchUriForResultsAsync**](/uwp/api/windows.system.launcher.launchuriforresultsasync) afin de démarrer une application pour afficher les résultats.

Les nouvelles API de communication entre les applications de Windows 10 permettent aux applications Windows (et aux applications web Windows) de démarrer une application et d’échanger des données et des fichiers. Cela vous permet de créer des solutions hybrides à partir de plusieurs applications. Grâce à ces nouvelles API, les tâches complexes qui, auparavant, auraient obligé l’utilisateur à lancer plusieurs applications, peuvent désormais être gérées de manière transparente. Ainsi, votre application peut démarrer une application de réseau social pour choisir un contact, ou une application de validation d’achat pour effectuer un processus de paiement.

L’application que vous démarrez pour afficher les résultats sera désignée sous le nom d’application lancée. L’application qui lance l’application sera désignée sous le nom d’application appelante. Pour cet exemple, vous allez écrire l’application appelante et l’application lancée.

## <a name="step-1-register-the-protocol-to-be-handled-in-the-app-that-youll-launch-for-results"></a>Étape 1 : Inscrire le protocole à gérer dans l’application démarrée pour afficher les résultats


Dans le fichier Package. appxmanifest de l’application lancée, ajoutez une extension de protocole à la section ** &lt; application &gt; ** . L’exemple présent utilise un protocole fictif, appelé **test-app2app**.

L’attribut **ReturnResults** dans l’extension de protocole accepte l’une des valeurs suivantes :

-   **optional** : l’application peut être démarrée pour afficher les résultats via la méthode [**LaunchUriForResultsAsync**](/uwp/api/windows.system.launcher.launchuriforresultsasync), ou à d’autres fins via la méthode [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync). Si vous utilisez l’élément **optional**, l’application lancée doit déterminer si elle a été démarrée pour afficher les résultats. Pour ce faire, elle peut vérifier l’argument d’événement [**OnActivated**](/uwp/api/windows.ui.xaml.application.onactivated). Si la propriété [**IActivatedEventArgs.Kind**](/uwp/api/windows.applicationmodel.activation.iactivatedeventargs.kind) de l’argument renvoie [**ActivationKind.ProtocolForResults**](/uwp/api/Windows.ApplicationModel.Activation.ActivationKind) ou si le type de l’argument d’événement est [**ProtocolActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs), l’application a été lancée via **LaunchUriForResultsAsync**.
-   **always** : l’application peut être démarrée uniquement pour afficher les résultats ; autrement dit, elle ne peut répondre qu’à [**LaunchUriForResultsAsync**](/uwp/api/windows.system.launcher.launchuriforresultsasync).
-   **none** : l’application ne peut pas être démarrée pour afficher les résultats ; elle ne peut répondre qu’à [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync).

Dans cet exemple d’extension de protocole, l’application peut être démarrée uniquement pour afficher les résultats. Cela permet de simplifier la logique figurant dans la méthode **OnActivated** (abordée ci-dessous), car nous avons uniquement besoin de gérer les cas de « démarrage pour afficher les résultats », et non les autres modes d’activation possibles de l’application.

```xml
<Applications>
   <Application ...>

     <Extensions>
       <uap:Extension Category="windows.protocol">
         <uap:Protocol Name="test-app2app" ReturnResults="always">
           <uap:DisplayName>Test app-2-app</uap:DisplayName>
         </uap:Protocol>
       </uap:Extension>
     </Extensions>

   </Application>
</Applications>
```

## <a name="step-2-override-applicationonactivated-in-the-app-that-youll-launch-for-results"></a>Étape 2 : Remplacer l’élément Application.OnActivated dans l’application à démarrer pour afficher les résultats


Si cette méthode n’existe pas déjà dans l’application lancée, créez-la dans la classe `App` définie dans le fichier App.xaml.cs.

Dans une application permettant de sélectionner vos amis dans un réseau social, cette fonction peut être utilisée pour ouvrir la page du sélecteur de contacts. Dans l’exemple suivant, une page appelée **LaunchedForResultsPage** s’affiche lorsque l’application est activée pour afficher les résultats. Assurez-vous que l’instruction **using** est incluse en haut du fichier.

```cs
using Windows.ApplicationModel.Activation;
...
protected override void OnActivated(IActivatedEventArgs args)
{
    // Window management
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame == null)
    {
        rootFrame = new Frame();
        Window.Current.Content = rootFrame;
    }

    // Code specific to launch for results
    var protocolForResultsArgs = (ProtocolForResultsActivatedEventArgs)args;
    // Open the page that we created to handle activation for results.
    rootFrame.Navigate(typeof(LaunchedForResultsPage), protocolForResultsArgs);

    // Ensure the current window is active.
    Window.Current.Activate();
}
```

Étant donné que l’extension de protocole dans le fichier Package.appxmanifest spécifie l’élément **ReturnResults** en tant que **always**, le code que nous venons d’illustrer peut effectuer un transtypage de l’élément `args` directement à [**ProtocolForResultsActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.ProtocolForResultsActivatedEventArgs). Cela permet de s’assurer que seul **ProtocolForResultsActivatedEventArgs** sera transmis à **OnActivated** pour cette application. Si votre application peut être activée de manière différente du lancement des résultats, vous pouvez vérifier si la propriété [**IActivatedEventArgs. Kind**](/uwp/api/windows.applicationmodel.activation.iactivatedeventargs.kind) retourne [**ActivationKind. ProtocolForResults**](/uwp/api/Windows.ApplicationModel.Activation.ActivationKind) pour savoir si l’application a été lancée pour les résultats.

## <a name="step-3-add-a-protocolforresultsoperation-field-to-the-app-you-launch-for-results"></a>Étape 3 : Ajouter un champ ProtocolForResultsOperation à l’application que vous démarrez pour afficher les résultats


```cs
private Windows.System.ProtocolForResultsOperation _operation = null;
```

Vous utiliserez le champ [**ProtocolForResultsOperation**](/uwp/api/windows.applicationmodel.activation.protocolforresultsactivatedeventargs.protocolforresultsoperation) pour signaler que l’application lancée est prête à renvoyer le résultat à l’application à l’origine de l’appel. Dans cet exemple, le champ est ajouté à la classe **LaunchedForResultsPage**, car vous effectuez le démarrage pour afficher les résultats à partir de cette page et devez être à même d’y accéder.

## <a name="step-4-override-onnavigatedto-in-the-app-you-launch-for-results"></a>Étape 4 : Remplacer OnNavigatedTo() dans l’application que vous démarrez pour afficher les résultats


Remplacez la méthode [**OnNavigatedTo**](/uwp/api/windows.ui.xaml.controls.page.onnavigatedto) dans la page que vous allez afficher lorsque votre application est démarrée pour afficher les résultats. Si cette méthode n’existe pas encore, créez-la dans la classe de la page définie dans le fichier &lt;pagename&gt;.xaml.cs. Assurez-vous que l’instruction **using** suivante est incluse en haut du fichier :

```cs
using Windows.ApplicationModel.Activation
```

L’objet [**NavigationEventArgs**](/uwp/api/Windows.UI.Xaml.Navigation.NavigationEventArgs) de la méthode [**OnNavigatedTo**](/uwp/api/windows.ui.xaml.controls.page.onnavigatedto) contient les données transmises par l’application à l’origine de l’appel. Ces données, d’une taille de 100 Ko au maximum, sont stockées dans un objet [**ValueSet**](/uwp/api/Windows.Foundation.Collections.ValueSet).

Dans cet exemple de code, l’application lancée s’attend à ce que les données envoyées par l’application à l’origine de l’appel soient incluses dans un élément [**ValueSet**](/uwp/api/Windows.Foundation.Collections.ValueSet), sous une clé appelée **TestData**, car il s’agit de l’élément dont l’envoi a été prévu par le code de l’exemple d’application à l’origine de l’appel.

```cs
using Windows.ApplicationModel.Activation;
...
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    var protocolForResultsArgs = e.Parameter as ProtocolForResultsActivatedEventArgs;
    // Set the ProtocolForResultsOperation field.
    _operation = protocolForResultsArgs.ProtocolForResultsOperation;

    if (protocolForResultsArgs.Data.ContainsKey("TestData"))
    {
        string dataFromCaller = protocolForResultsArgs.Data["TestData"] as string;
    }
}
...
private Windows.System.ProtocolForResultsOperation _operation = null;
```

## <a name="step-5-write-code-to-return-data-to-the-calling-app"></a>Étape 5 : Écrire du code pour renvoyer des données à l’application à l’origine de l’appel


Dans l’application lancée, utilisez [**ProtocolForResultsOperation**](/uwp/api/windows.applicationmodel.activation.protocolforresultsactivatedeventargs.protocolforresultsoperation) pour retourner des données à l’application appelante. Dans cet exemple de code, un objet [**ValueSet**](/uwp/api/Windows.Foundation.Collections.ValueSet) est créé. Il contient la valeur à renvoyer à l’application à l’origine de l’appel. Le champ **ProtocolForResultsOperation** est ensuite utilisé pour envoyer la valeur à l’application à l’origine de l’appel.

```cs
    ValueSet result = new ValueSet();
    result["ReturnedData"] = "The returned result";
    _operation.ReportCompleted(result);
```

## <a name="step-6-write-code-to-launch-the-app-for-results-and-get-the-returned-data"></a>Étape 6 : Écrire du code pour démarrer l’application pour afficher les résultats et obtenir les données renvoyées


Démarrez l’application à partir d’une méthode asynchrone dans l’application à l’origine de l’appel, comme illustré dans cet exemple de code. Notez les instructions **using**, qui sont nécessaires pour que le code soit compilé :

```cs
using System.Threading.Tasks;
using Windows.System;
...

async Task<string> LaunchAppForResults()
{
    var testAppUri = new Uri("test-app2app:"); // The protocol handled by the launched app
    var options = new LauncherOptions();
    options.TargetApplicationPackageFamilyName = "67d987e1-e842-4229-9f7c-98cf13b5da45_yd7nk54bq29ra";

    var inputData = new ValueSet();
    inputData["TestData"] = "Test data";

    string theResult = "";
    LaunchUriResult result = await Windows.System.Launcher.LaunchUriForResultsAsync(testAppUri, options, inputData);
    if (result.Status == LaunchUriStatus.Success &&
        result.Result != null &&
        result.Result.ContainsKey("ReturnedData"))
    {
        ValueSet theValues = result.Result;
        theResult = theValues["ReturnedData"] as string;
    }
    return theResult;
}
```

Dans cet exemple, un élément [**ValueSet**](/uwp/api/Windows.Foundation.Collections.ValueSet) contenant la clé **TestData** est transmis à l’application lancée. L’application lancée crée un élément **ValueSet** avec une clé appelée **ReturnedData**, qui contient le résultat renvoyé à l’application à l’origine de l’appel.

Vous devez générer et déployer l’application à démarrer pour afficher les résultats avant d’exécuter l’application à l’origine de l’appel. Sinon, [**LaunchUriResult.Status**](/uwp/api/Windows.System.LaunchUriStatus) indiquera **LaunchUriStatus.AppUnavailable**.

Vous aurez besoin du nom de famille de l’application lancée lorsque vous définissez le [**TargetApplicationPackageFamilyName**](/uwp/api/windows.system.launcheroptions.targetapplicationpackagefamilyname). Une façon d’obtenir le nom de famille consiste à effectuer l’appel suivant à partir de l’application lancée :

```cs
string familyName = Windows.ApplicationModel.Package.Current.Id.FamilyName;
```

## <a name="remarks"></a>Remarques


L’exemple de cette procédure inclut une introduction de type « hello world » pour le démarrage d’une application afin d’afficher les résultats. La principale chose à noter est que la nouvelle API [**LaunchUriForResultsAsync**](/uwp/api/windows.system.launcher.launchuriforresultsasync) vous permet de lancer une application de manière asynchrone et de communiquer via la classe [**ValueSet**](/uwp/api/Windows.Foundation.Collections.ValueSet) . Les données à transmettre via un élément **ValueSet** doivent présenter une taille de 100 Ko au maximum. Si vous avez besoin de transmettre des volumes de données plus importants, vous pouvez partager des fichiers à l’aide de la classe [**SharedStorageAccessManager**](/uwp/api/Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager) pour créer des jetons de fichier que vous pouvez passer entre les applications. Par exemple, pour un élément **ValueSet** appelé `inputData`, vous pouvez stocker le jeton dans un fichier que vous souhaitez partager avec l’application lancée :

```cs
inputData["ImageFileToken"] = SharedStorageAccessManager.AddFile(myFile);
```

transférez-le ensuite à l’application lancée via **LaunchUriForResultsAsync**.

## <a name="related-topics"></a>Rubriques connexes


* [**LaunchUri**](/uwp/api/windows.system.launcher.launchuriasync)
* [**LaunchUriForResultsAsync**](/uwp/api/windows.system.launcher.launchuriforresultsasync)
* [**ValueSet**](/uwp/api/Windows.Foundation.Collections.ValueSet)

 

 