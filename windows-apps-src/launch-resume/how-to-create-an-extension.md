---
title: Créer et héberger une extension d’application
description: Écrire et héberger des extensions d’application qui vous permettent d’étendre votre application via des packages que les utilisateurs peuvent installer à partir de la Microsoft Store.
keywords: extension d’application, app service, arrière-plan
ms.date: 01/28/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 122c7c4d206c014d7d76cdab0b1b8fc66c0c9371
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158843"
---
# <a name="create-and-host-an-app-extension"></a>Créer et héberger une extension d’application

Cet article explique comment créer une extension d’application Windows 10 et l’héberger dans une application. Les extensions d’application sont prises en charge dans les applications UWP et les [applications de bureau packagées](/windows/apps/desktop/modernize/#msix-packages).

Pour illustrer la création d’une extension d’application, cet article utilise le XML du manifeste du package et les extraits de code de l' [exemple de code d’extension mathématique](https://github.com/MicrosoftDocs/windows-topic-specific-samples/tree/MathExtensionSample). Cet exemple est une application UWP, mais les fonctionnalités illustrées dans l’exemple s’appliquent également aux applications de bureau empaquetées. Suivez ces instructions pour commencer à utiliser l’exemple :

- Téléchargez et décompressez l' [exemple de code Math extension](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/MathExtensionSample.zip).
- Dans Visual Studio 2019, ouvrez MathExtensionSample. sln. Définissez le type de build sur x86 (**Build**  >  **Configuration Manager**, puis remplacez **Platform** par **x86** pour les deux projets).
- Déployer la solution : **créer**une  >  **solution de déploiement**.

## <a name="introduction-to-app-extensions"></a>Présentation des extensions d’application

Dans Windows 10, les extensions d’application offrent des fonctionnalités similaires à celles des plug-ins, des compléments et des modules complémentaires sur d’autres plateformes. Les extensions d’application ont été introduites dans l’édition anniversaire Windows 10 (version 1607, Build 10.0.14393).

Les extensions d’application sont des applications UWP ou des applications de bureau empaquetées qui ont une déclaration d’extension qui leur permet de partager du contenu et des événements de déploiement avec une application hôte. Une application d’extension peut fournir plusieurs extensions.

Étant donné que les extensions d’application sont uniquement des applications UWP ou des applications de bureau packagées, elles peuvent également être des applications entièrement fonctionnelles, des extensions d’hôte et fournir des extensions à d’autres applications, tout cela sans créer des packages d’application distincts.

Lorsque vous créez un hôte d’extension d’application, vous créez une opportunité de développer un écosystème autour de votre application dans laquelle d’autres développeurs peuvent améliorer votre application de manière à ce que vous n’ayez pas ou non prévu les ressources pour. Envisagez Microsoft Office extensions, les extensions Visual Studio, les extensions de navigateur, etc. Ils créent des expériences plus riches pour les applications qui dépassent les fonctionnalités qu’elles ont fournies avec. Les extensions peuvent ajouter de la valeur et de la longévité à votre application.

À un niveau élevé, pour configurer une relation d’extension d’application, nous devons :

1. Déclarez une application comme hôte d’extension.
2. Déclarez une application en tant qu’extension.
3. Décidez si l’extension doit être implémentée en tant que service d’application, tâche en arrière-plan ou d’une autre façon.
4. Définir le mode de communication des hôtes et de ses extensions.
5. Utilisez l’API [Windows. ApplicationModel. AppExtensions](/uwp/api/Windows.ApplicationModel.AppExtensions) dans l’application hôte pour accéder aux extensions.

Voyons comment cela s’effectue en examinant l' [exemple de code Math extension](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/MathExtensionSample.zip) qui implémente une calculatrice hypothétique à laquelle vous pouvez ajouter de nouvelles fonctions à l’aide d’extensions. Dans Microsoft Visual Studio 2019, chargez **MathExtensionSample. sln** à partir de l’exemple de code.

![Exemple de code d’extension mathématique](images/mathextensionhost-calctab.png)

## <a name="declare-an-app-to-be-an-extension-host"></a>Déclarer une application comme hôte d’extension

Une application s’identifie comme un hôte d’extension d’application en déclarant l' `<AppExtensionHost>` élément dans son fichier Package. appxmanifest. Pour savoir comment procéder, consultez le fichier **Package. appxmanifest** dans le projet **MathExtensionHost** .

_Package. appxmanifest dans le projet MathExtensionHost_
```xml
<Package
  ...
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap uap3 mp">
  ...
    <Applications>
      <Application Id="App" ... >
        ...
        <Extensions>
            <uap3:Extension Category="windows.appExtensionHost">
                <uap3:AppExtensionHost>
                  <uap3:Name>com.microsoft.mathext</uap3:Name>
                </uap3:AppExtensionHost>
          </uap3:Extension>
        </Extensions>
      </Application>
    </Applications>
    ...
</Package>
```

Notez le `xmlns:uap3="http://..."` et la présence de `uap3` dans `IgnorableNamespaces` . Ils sont nécessaires, car nous utilisons l’espace de noms uap3.

`<uap3:Extension Category="windows.appExtensionHost">` identifie cette application en tant qu’hôte d’extension.

L’élément **Name** dans `<uap3:AppExtensionHost>` est le nom de _contrat d’extension_ . Quand une extension spécifie le même nom de contrat d’extension, l’hôte peut la trouver. Par Convention, nous vous recommandons de générer le nom de contrat d’extension à l’aide de votre nom d’application ou de serveur de publication pour éviter les collisions potentielles avec d’autres noms de contrat d’extension.

Vous pouvez définir plusieurs hôtes et plusieurs extensions dans la même application. Dans cet exemple, nous déclarons un hôte. L’extension est définie dans une autre application.

## <a name="declare-an-app-to-be-an-extension"></a>Déclarer une application en tant qu’extension

Une application s’identifie elle-même en tant qu’extension d’application en déclarant l' `<uap3:AppExtension>` élément dans son fichier **Package. appxmanifest** . Ouvrez le fichier **Package. appxmanifest** dans le projet **MathExtension** pour voir comment cela est effectué.

_Package. appxmanifest dans le projet MathExtension :_
```xml
<Package
  ...
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap uap3 mp">
  ...
    <Applications>
      <Application Id="App" ... >
        ...
        <Extensions>
          ...
          <uap3:Extension Category="windows.appExtension">
            <uap3:AppExtension Name="com.microsoft.mathext"
                               Id="power"
                               DisplayName="x^y"
                               Description="Exponent"
                               PublicFolder="Public">
              <uap3:Properties>
                <Service>com.microsoft.powservice</Service>
              </uap3:Properties>
              </uap3:AppExtension>
          </uap3:Extension>
        </Extensions>
      </Application>
    </Applications>
    ...
</Package>
```

Là encore, notez la `xmlns:uap3="http://..."` ligne et la présence de `uap3` dans `IgnorableNamespaces` . Ils sont nécessaires, car nous utilisons l' `uap3` espace de noms.

`<uap3:Extension Category="windows.appExtension">` identifie cette application en tant qu’extension.

La signification des `<uap3:AppExtension>` attributs est la suivante :

|Attribut|Description|Obligatoire|
|---------|-----------|:------:|
|**Nom**|Il s’agit du nom de contrat d’extension. Lorsqu’il correspond au **nom** déclaré dans un hôte, cet hôte est en mesure de trouver cette extension.| :heavy_check_mark: |
|**Identifiant**| Identifie de façon unique cette extension. Étant donné qu’il peut y avoir plusieurs extensions qui utilisent le même nom de contrat d’extension (Imaginez une application de peinture qui prend en charge plusieurs extensions), vous pouvez utiliser l’ID pour les distinguer. Les hôtes d’extension d’application peuvent utiliser l’ID pour déduire une information sur le type d’extension. Par exemple, vous pouvez avoir une extension conçue pour les ordinateurs de bureau et une autre pour les appareils mobiles, l’ID étant le différentiateur. Vous pouvez également utiliser l’élément **Properties** , présenté ci-dessous, pour cela.| :heavy_check_mark: |
|**DisplayName**| Peut être utilisé à partir de votre application hôte pour identifier l’extension de l’utilisateur. Il est possible d’interroger et d’utiliser le [nouveau système de gestion des ressources](../app-resources/using-mrt-for-converted-desktop-apps-and-games.md) ( `ms-resource:TokenName` ) pour la localisation. Le contenu localisé est chargé à partir du package d’extension d’application, et non de l’application hôte. | |
|**Description** | Peut être utilisé à partir de votre application hôte pour décrire l’extension à l’utilisateur. Il est possible d’interroger et d’utiliser le [nouveau système de gestion des ressources](../app-resources/using-mrt-for-converted-desktop-apps-and-games.md) ( `ms-resource:TokenName` ) pour la localisation. Le contenu localisé est chargé à partir du package d’extension d’application, et non de l’application hôte. | |
|**Dossier public**|Nom d’un dossier, relatif à la racine du package, où vous pouvez partager du contenu avec l’hôte d’extension. Par Convention, le nom est « public », mais vous pouvez utiliser n’importe quel nom qui correspond à un dossier dans votre extension.| :heavy_check_mark: |

`<uap3:Properties>` est un élément facultatif qui contient des métadonnées personnalisées que les hôtes peuvent lire au moment de l’exécution. Dans l’exemple de code, l’extension est implémentée en tant qu’App service, de sorte que l’hôte a besoin d’un moyen d’obtenir le nom de cet app service afin qu’il puisse l’appeler. Le nom de l’app service est défini dans l' <Service> élément, que nous avons défini (nous aurions pu l’appeler tout ce que nous voulions). L’hôte de l’exemple de code recherche cette propriété au moment de l’exécution afin d’en savoir plus sur le nom de l’app service.

## <a name="decide-how-you-will-implement-the-extension"></a>Décidez comment vous allez implémenter l’extension.

La [session Build 2016 sur les extensions d’application](https://channel9.msdn.com/Events/Build/2016/B808) montre comment utiliser le dossier public partagé entre l’hôte et les extensions. Dans cet exemple, l’extension est implémentée par un fichier JavaScript qui est stocké dans le dossier public, que l’hôte appelle. Cette approche présente l’avantage d’être légère, ne nécessite pas de compilation et peut prendre en charge la création d’une page d’accueil par défaut qui fournit des instructions pour l’extension et un lien vers la page de Microsoft Store de l’application hôte. Pour plus d’informations, consultez l’exemple de code de l' [extension d’application Build 2016](https://github.com/Microsoft/App-Extensibility-Sample) . Plus précisément, consultez le projet **InvertImageExtension** et `InvokeLoad()` dans ExtensionManager.cs dans le projet **ExtensibilitySample** .

Dans cet exemple, nous allons utiliser un app service pour implémenter l’extension. Les services d’application présentent les avantages suivants :

- Si l’extension se bloque, elle n’arrête pas l’application hôte, car l’application hôte s’exécute dans son propre processus.
- Vous pouvez utiliser le langage de votre choix pour implémenter le service. Il ne doit pas nécessairement correspondre à la langue utilisée pour implémenter l’application hôte.
- App service a accès à son propre conteneur d’applications, qui peut avoir des fonctionnalités différentes de celles de l’hôte.
- Il existe un isolement entre les données du service et l’application hôte.

### <a name="host-app-service-code"></a>Héberger le code App service

Voici le code hôte qui appelle l’app service de l’extension :

_ExtensionManager.cs dans le projet MathExtensionHost_
```cs
public async Task<double> Invoke(ValueSet message)
{
    if (Loaded)
    {
        try
        {
            // make the app service call
            using (var connection = new AppServiceConnection())
            {
                // service name is defined in appxmanifest properties
                connection.AppServiceName = _serviceName;
                // package Family Name is provided by the extension
                connection.PackageFamilyName = AppExtension.Package.Id.FamilyName;

                // open the app service connection
                AppServiceConnectionStatus status = await connection.OpenAsync();
                if (status != AppServiceConnectionStatus.Success)
                {
                    Debug.WriteLine("Failed App Service Connection");
                }
                else
                {
                    // Call the app service
                    AppServiceResponse response = await connection.SendMessageAsync(message);
                    if (response.Status == AppServiceResponseStatus.Success)
                    {
                        ValueSet answer = response.Message as ValueSet;
                        if (answer.ContainsKey("Result")) // When our app service returns "Result", it means it succeeded
                        {
                            return (double)answer["Result"];
                        }
                    }
                }
            }
        }
        catch (Exception)
        {
             Debug.WriteLine("Calling the App Service failed");
        }
    }
    return double.NaN; // indicates an error from the app service
}
```

Il s’agit d’un code classique pour appeler un app service. Pour plus d’informations sur la façon d’implémenter et d’appeler un app service, consultez [comment créer et utiliser un app service](how-to-create-and-consume-an-app-service.md).

Une chose à noter est la façon dont le nom de l’app service à appeler est déterminé. Étant donné que l’hôte n’a pas d’informations sur l’implémentation de l’extension, l’extension doit fournir le nom de son App service. Dans l’exemple de code, l’extension déclare le nom de l’app service dans son fichier dans l' `<uap3:Properties>` élément :

_Package. appxmanifest dans le projet MathExtension_
```xml
    ...
    <uap3:Extension Category="windows.appExtension">
      <uap3:AppExtension ...>
        <uap3:Properties>
          <Service>com.microsoft.powservice</Service>
        </uap3:Properties>
        </uap3:AppExtension>
    </uap3:Extension>
```

Vous pouvez définir votre propre code XML dans l' `<uap3:Properties>` élément. Dans ce cas, nous définissons le nom de l’app service de sorte que l’hôte puisse l’appeler lorsqu’il appelle l’extension.

Lorsque l’hôte charge une extension, du code comme celui-ci extrait le nom du service à partir des propriétés définies dans le package de l’extension. appxmanifest :

_`Update()` dans ExtensionManager.cs, dans le projet MathExtensionHost_
```cs
...
var properties = await ext.GetExtensionPropertiesAsync() as PropertySet;

...
#region Update Properties
// update app service information
_serviceName = null;
if (_properties != null)
{
   if (_properties.ContainsKey("Service"))
   {
       PropertySet serviceProperty = _properties["Service"] as PropertySet;
       this._serviceName = serviceProperty["#text"].ToString();
   }
}
#endregion
```

Avec le nom de l’app service stocké dans `_serviceName` , l’hôte est en mesure de l’utiliser pour appeler le service d’application.

L’appel d’un app service requiert également le nom de famille du package qui contient le service d’application. Heureusement, l’API d’extension d’application fournit ces informations qui sont obtenues sur la ligne : `connection.PackageFamilyName = AppExtension.Package.Id.FamilyName;`

### <a name="define-how-the-host-and-the-extension-will-communicate"></a>Définir la manière dont l’hôte et l’extension communiquent

Les services d’application utilisent une classe [ValueSet](/uwp/api/windows.foundation.collections.valueset) pour échanger des informations. En tant qu’auteur de l’hôte, vous devez trouver un protocole pour communiquer avec les extensions qui sont flexibles. Dans l’exemple de code, cela signifie la prise en compte des extensions qui peuvent prendre 1, 2 ou plus d’arguments à l’avenir.

Pour cet exemple, le protocole des arguments est un **ValueSet** contenant les paires clé/valeur nommées’arg' + le numéro d’argument, par exemple, `Arg1` et `Arg2` . L’hôte passe tous les arguments de **ValueSet**, et l’extension utilise ceux dont il a besoin. Si l’extension est en mesure de calculer un résultat, l’hôte attend que le **ValueSet** retourné par l’extension ait une clé nommée `Result` qui contient la valeur du calcul. Si cette clé n’est pas présente, l’hôte suppose que l’extension n’a pas pu terminer le calcul.

### <a name="extension-app-service-code"></a>Code du service d’application d’extension

Dans l’exemple de code, l’application App service n’est pas implémentée en tant que tâche en arrière-plan. Au lieu de cela, il utilise le modèle App service à processus unique dans lequel App service s’exécute dans le même processus que l’application d’extension qui l’héberge. Il s’agit toujours d’un processus différent de l’application hôte, qui offre les avantages de la séparation des processus, tout en bénéficiant d’avantages en matière de performances en évitant les communications inter-processus entre le processus d’extension et le processus en arrière-plan qui implémente le service d’application. Consultez [convertir un app service pour qu’il s’exécute dans le même processus que son application hôte](convert-app-service-in-process.md) pour voir la différence entre un service d’application qui s’exécute en tant que tâche en arrière-plan et dans le même processus.

Le système effectue l' `OnBackgroundActivate()` activation lorsque l’app service est activé. Ce code configure des gestionnaires d’événements pour gérer l’appel App service réel lorsqu’il est fourni par (), ainsi que pour gérer les `OnAppServiceRequestReceived()` événements de maintenance tels que l’obtention d’un objet Report gérant un événement CANCEL ou Closed.

_App.xaml.cs dans le projet MathExtension._
```cs
protected override void OnBackgroundActivated(BackgroundActivatedEventArgs args)
{
    base.OnBackgroundActivated(args);

    if ( _appServiceInitialized == false ) // Only need to setup the handlers once
    {
        _appServiceInitialized = true;

        IBackgroundTaskInstance taskInstance = args.TaskInstance;
        taskInstance.Canceled += OnAppServicesCanceled;

        AppServiceTriggerDetails appService = taskInstance.TriggerDetails as AppServiceTriggerDetails;
        _appServiceDeferral = taskInstance.GetDeferral();
        _appServiceConnection = appService.AppServiceConnection;
        _appServiceConnection.RequestReceived += OnAppServiceRequestReceived;
        _appServiceConnection.ServiceClosed += AppServiceConnection_ServiceClosed;
    }
}
```

Le code qui effectue le travail de l’extension se trouve dans `OnAppServiceRequestReceived()` . Cette fonction est appelée lorsque l’app service est appelé pour effectuer un calcul. Il extrait les valeurs dont il a besoin à partir du **ValueSet**. S’il peut effectuer le calcul, il place le résultat, sous une clé nommée **result**, dans le **ValueSet** renvoyé à l’hôte. Rappelez-vous que selon le protocole défini pour la communication de cet hôte et de ses extensions, la présence d’une clé de **résultat** indiquera la réussite ; Sinon, échec.

_App.xaml.cs dans le projet MathExtension._
```cs
private async void OnAppServiceRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
{
    // Get a deferral because we use an awaitable API below (SendResponseAsync()) to respond to the message
    // and we don't want this call to get cancelled while we are waiting.
    AppServiceDeferral messageDeferral = args.GetDeferral();
    ValueSet message = args.Request.Message;
    ValueSet returnMessage = new ValueSet();

    double? arg1 = Convert.ToDouble(message["arg1"]);
    double? arg2 = Convert.ToDouble(message["arg2"]);
    if (arg1.HasValue && arg2.HasValue)
    {
        returnMessage.Add("Result", Math.Pow(arg1.Value, arg2.Value)); // For this sample, the presence of a "Result" key will mean the call succeeded
    }

    await args.Request.SendResponseAsync(returnMessage);
    messageDeferral.Complete();
}
```

## <a name="manage-extensions"></a>Gérer les extensions

Maintenant que nous avons vu comment implémenter la relation entre un hôte et ses extensions, voyons comment un hôte trouve les extensions installées sur le système et réagit à l’ajout et à la suppression des packages contenant des extensions.

Le Microsoft Store fournit des extensions sous forme de packages. Le [AppExtensionCatalog](/uwp/api/windows.applicationmodel.appextensions.appextensioncatalog) trouve les packages installés qui contiennent des extensions correspondant au nom de contrat d’extension de l’hôte et fournit des événements qui se déclenchent lorsqu’un package d’extension d’application correspondant à l’hôte est installé ou supprimé.

Dans l’exemple de code, la `ExtensionManager` classe (définie dans **ExtensionManager.cs** dans le projet **MathExtensionHost** ) encapsule la logique de chargement des extensions et de réponse aux installations et désinstallations des packages d’extension.

Le `ExtensionManager` constructeur utilise le `AppExtensionCatalog` pour rechercher les extensions d’application sur le système qui ont le même nom de contrat d’extension que l’hôte :

_ExtensionManager.cs dans le projet MathExtensionHost._
```cs
public ExtensionManager(string extensionContractName)
{
   // catalog & contract
   ExtensionContractName = extensionContractName;
   _catalog = AppExtensionCatalog.Open(ExtensionContractName);
   ...
}
```

Lors de l’installation d’un package d’extension, le `ExtensionManager` rassemble des informations sur les extensions du package qui ont le même nom de contrat d’extension que l’hôte. Une installation peut représenter une mise à jour, auquel cas les informations de l’extension affectée sont mises à jour. Lors de la désinstallation d’un package d’extension, le `ExtensionManager` supprime les informations relatives aux extensions affectées afin que l’utilisateur sache quelles extensions ne sont plus disponibles.

La `Extension` classe (définie dans **ExtensionManager.cs** dans le projet **MathExtensionHost** ) a été créée pour que l’exemple de code accède à l’ID, à la description, au logo et aux informations spécifiques à l’application de l’extension, par exemple si l’utilisateur a activé l’extension.

Pour indiquer que l’extension est chargée (voir `Load()` dans **ExtensionManager.cs**) signifie que l’état du package est correct et que nous avons obtenu son ID, son logo, sa description et son dossier public (que nous n’utilisons pas dans cet exemple). Le package d’extension lui-même n’est pas chargé.

Le concept de déchargement est utilisé pour assurer le suivi des extensions qui ne doivent plus être présentées à l’utilisateur.

Le `ExtensionManager` fournit des instances de collection `Extension` afin que les extensions, leurs noms, leurs descriptions et leurs logos puissent être liés aux données de l’interface utilisateur. La page **ExtensionsTab** est liée à cette collection et fournit une interface utilisateur pour l’activation/la désactivation des extensions, ainsi que la suppression.

![Exemple d’interface utilisateur de l’onglet Extensions](images/mathextensionhost-extensiontab.png)

 Quand une extension est supprimée, le système demande à l’utilisateur de vérifier qu’il souhaite désinstaller le package qui contient l’extension (et éventuellement contient d’autres extensions). Si l’utilisateur accepte, le package est désinstallé et `ExtensionManager` supprime les extensions du package désinstallé de la liste des extensions disponibles pour l’application hôte.

 ![Désinstaller l’interface utilisateur](images/mathextensionhost-uninstall.png)

## <a name="debugging-app-extensions-and-hosts"></a>Débogage d’extensions et d’hôtes d’application

Souvent, l’hôte et l’extension d’extension ne font pas partie de la même solution. Dans ce cas, pour déboguer l’hôte et l’extension :

1. Chargez votre projet hôte dans une instance de Visual Studio.
2. Chargez votre extension dans une autre instance de Visual Studio.
3. Lancez votre application hôte dans le débogueur.
4. Lancez l’application d’extension dans le débogueur. (Si vous souhaitez déployer l’extension, plutôt que de la déboguer, pour tester l’événement d’installation du package de l’hôte, **créez la &gt; solution de déploiement**, à la place).

Vous pourrez maintenant atteindre des points d’arrêt dans l’hôte et l’extension.
Si vous commencez à déboguer l’application d’extension elle-même, vous verrez une fenêtre vide pour l’application. Si vous ne souhaitez pas voir la fenêtre vide, vous pouvez modifier les paramètres de débogage du projet d’extension pour ne pas lancer l’application, mais la déboguer au démarrage (cliquez avec le bouton droit sur le projet d’extension). **Propriétés**  >  de**débogage** > sélectionner **ne pas lancer, mais déboguer mon code au démarrage**) vous devez toujours démarrer le débogage (**F5**) du projet d’extension, mais il attend que l’hôte active l’extension, puis vos points d’arrêt dans l’extension seront atteints.

**Déboguer l’exemple de code**

Dans l’exemple de code, l’hôte et l’extension se trouvent dans la même solution. Procédez comme suit pour déboguer :

1. Assurez-vous que **MathExtensionHost** est le projet de démarrage (cliquez avec le bouton droit sur le projet **MathExtensionHost** , cliquez sur **définir comme projet de démarrage**).
2. Placez un point d’arrêt `Invoke` dans ExtensionManager.cs, dans le projet **MathExtensionHost** .
3. **F5** pour exécuter le projet **MathExtensionHost** .
4. Placez un point d’arrêt dans `OnAppServiceRequestReceived` app.Xaml.cs dans le projet **MathExtension** .
5. Démarrez le débogage du projet **MathExtension** (cliquez avec le bouton droit sur le projet **MathExtension** , **déboguez > démarrer une nouvelle instance**), ce qui le déploiera et déclenchera l’événement d’installation de package dans l’hôte.
6. Dans l’application **MathExtensionHost** , accédez à la page de **calcul** , puis cliquez sur **x ^ y** pour activer l’extension. Le `Invoke()` point d’arrêt est atteint en premier et vous pouvez voir l’appel App service d’extensions effectué. Ensuite `OnAppServiceRequestReceived()` , la méthode dans l’extension est atteinte et vous pouvez voir que app service calcule le résultat et le retourner.

**Dépannage des extensions implémentées en tant que service d’application**

Si votre hôte d’extension a des difficultés à se connecter à App service pour votre extension, assurez-vous que l' `<uap:AppService Name="...">` attribut correspond à ce que vous placez dans votre `<Service>` élément. Si elles ne correspondent pas, le nom du service fourni par votre extension ne correspond pas au nom de l’app service que vous avez implémenté, et l’hôte ne peut pas activer votre extension.

_Package. appxmanifest dans le projet MathExtension :_
```xml
<Extensions>
   <uap:Extension Category="windows.appService">
     <uap:AppService Name="com.microsoft.sqrtservice" />      <!-- This must match the contents of <Service>...</Service> -->
   </uap:Extension>
   <uap3:Extension Category="windows.appExtension">
     <uap3:AppExtension Name="com.microsoft.mathext" Id="sqrt" DisplayName="Sqrt(x)" Description="Square root" PublicFolder="Public">
       <uap3:Properties>
         <Service>com.microsoft.powservice</Service>   <!-- this must match <uap:AppService Name=...> -->
       </uap3:Properties>
     </uap3:AppExtension>
   </uap3:Extension>
</Extensions>   
```

## <a name="a-checklist-of-basic-scenarios-to-test"></a>Liste de vérification des scénarios de base à tester

Quand vous générez un hôte d’extension et que vous êtes prêt à tester la manière dont il prend en charge les extensions, voici quelques scénarios de base à essayer :

- Exécuter l’hôte, puis déployer une application d’extension  
    - L’hôte sélectionne-t-il de nouvelles extensions qui se présentent pendant son exécution ?  
- Déployez l’application d’extension, puis déployez et exécutez l’hôte.
    - L’hôte sélectionne-t-il des extensions précédemment existantes ?  
- Exécutez l’hôte, puis supprimez l’application d’extension.
    - L’hôte détecte-t-il correctement la suppression ?
- Exécutez l’hôte, puis mettez à jour l’application d’extension vers une version plus récente.
    - L’hôte sélectionne-t-il la modification et décharge les anciennes versions de l’extension correctement ?  

**Scénarios avancés à tester :**

- Exécutez l’hôte, déplacez l’application d’extension sur un support amovible, retirez le support.
    - L’hôte détecte-t-il la modification de l’état du package et désactive l’extension ?
- Exécutez l’hôte, puis corrompre l’application d’extension (le rendre non valide, signé différemment, etc.).
    - L’hôte détecte-t-il l’extension falsifiée et la gère correctement ?
- Exécutez l’hôte, puis déployez une application d’extension dont le contenu ou les propriétés ne sont pas valides
    - L’hôte détecte-t-il le contenu non valide et le gère correctement ?

## <a name="design-considerations"></a>Remarques relatives à la conception

- Fournissez une interface utilisateur qui montre à l’utilisateur quelles extensions sont disponibles et leur permet de les activer/désactiver. Vous pouvez également envisager d’ajouter des glyphes pour les extensions qui deviennent indisponibles, car un package est mis hors connexion, etc.
- Diriger l’utilisateur vers l’emplacement où il peut accéder aux extensions. Votre page d’extension peut peut-être fournir une Microsoft Store requête de recherche qui affiche une liste d’extensions pouvant être utilisées avec votre application.
- Examinez comment informer l’utilisateur de l’ajout et de la suppression des extensions. Vous pouvez créer une notification lorsqu’une nouvelle extension est installée et inviter l’utilisateur à l’activer. Les extensions doivent être désactivées par défaut afin que les utilisateurs soient dans le contrôle.

## <a name="how-app-extensions-differ-from-optional-packages"></a>Différences entre les extensions d’application et les packages facultatifs

Le différentiateur clé entre les [packages facultatifs](/windows/msix/package/optional-packages) et les extensions d’application est l’écosystème ouvert par rapport à l’écosystème fermé et le package dépendant et le package indépendant.

Les extensions d’application participent à un écosystème ouvert. Si votre application peut héberger des extensions d’application, tout le monde peut écrire une extension pour votre hôte, à condition qu’elles soient conformes à votre méthode de transmission/réception d’informations à partir de l’extension. Cela diffère des packages facultatifs qui participent à un écosystème fermé dans lequel le serveur de publication décide qui est autorisé à créer un package facultatif qui peut être utilisé avec l’application.

Les extensions d’application sont des packages indépendants et peuvent être des applications autonomes. Ils ne peuvent pas avoir de dépendance de déploiement sur une autre application.Les packages facultatifs nécessitent le package principal et ne peuvent pas s’exécuter sans celui-ci.

Un pack d’extension pour un jeu est un bon candidat pour un package facultatif parce qu’il est étroitement lié au jeu, qu’il ne peut pas s’exécuter indépendamment du jeu et que vous ne souhaitez pas que les packs d’extension soient créés par un développeur quelconque dans l’écosystème.

Si ce même jeu avait des compléments d’interface utilisateur personnalisables ou des thèmes, une extension d’application pourrait être un bon choix, car l’application qui fournit l’extension pourrait s’exécuter de manière autonome, et les tiers pourraient les faire.

## <a name="remarks"></a>Remarques

Cette rubrique fournit une introduction aux extensions d’application. Les points clés à prendre en compte sont la création de l’hôte et son marquage dans son package. fichier appxmanifest, en créant l’extension et en la marquant comme telle dans son fichier Package. appxmanifest, en déterminant comment implémenter l’extension (par exemple, un service d’application, une tâche en arrière-plan ou d’autres moyens), en définissant la manière dont l’hôte communiquera avec les extensions et en utilisant l' [API AppExtensions](/uwp/api/windows.applicationmodel.appextensions) pour accéder aux extensions et les gérer.

## <a name="related-topics"></a>Rubriques connexes

* [Présentation des extensions d’application](/windows/msix/)
* [Générer une session 2016 sur les extensions d’application](https://channel9.msdn.com/Events/Build/2016/B808)
* [Exemple de code d’extension de build 2016](https://github.com/Microsoft/App-Extensibility-Sample)
* [Prendre en charge votre application avec des tâches en arrière-plan](support-your-app-with-background-tasks.md)
* [Comment créer et utiliser un app service](how-to-create-and-consume-an-app-service.md).
* [Espace de noms AppExtensions](/uwp/api/windows.applicationmodel.appextensions)
* [Étendre votre application avec des services, des extensions et des packages](./extend-your-app-with-services-extensions-packages.md)