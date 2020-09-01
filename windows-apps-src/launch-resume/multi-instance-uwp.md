---
title: Créer une application Windows universelle multi-instance
description: Cette rubrique explique comment écrire des applications UWP qui prennent en charge l’instanciation multiple.
keywords: UWP multi-instance
ms.date: 09/21/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8e19425222459bb3bd56a5fe406ec76c0b7fb6b9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164773"
---
# <a name="create-a-multi-instance-universal-windows-app"></a>Créer une application Windows universelle multi-instance

Cette rubrique explique comment créer des applications de plateforme Windows universelle multi-instance (UWP).

À partir de Windows 10, version 1803 (10,0 ; Build 17134), votre application UWP peut s’abonner pour prendre en charge plusieurs instances. Si une instance d’une application UWP à instances multiples est en cours d’exécution et si une demande d’activation consécutive est reçue, la plateforme n’active pas l’instance existante. Au lieu de cela, elle crée une instance qui s’exécute dans un processus distinct.

> [!IMPORTANT]
> L’instanciation multiple est prise en charge pour les applications JavaScript, contrairement à la redirection d’instanciation multiple. Étant donné que la redirection d’instanciation multiple n’est pas prise en charge pour les applications JavaScript, la classe [**AppInstance**](/uwp/api/windows.applicationmodel.appinstance) n’est pas utile pour ces applications.

## <a name="opt-in-to-multi-instance-behavior"></a>Accepter le comportement multi-instance

Si vous créez une application multi-instance, vous pouvez installer le projet d' [application multi-instance Templates. vsix](https://marketplace.visualstudio.com/items?itemName=AndrewWhitechapelMSFT.MultiInstanceApps), disponible à partir du [Visual Studio Marketplace](https://marketplace.visualstudio.com/). Une fois les modèles installés, ils sont disponibles dans la boîte de dialogue **nouveau projet** sous **Visual C# > Windows universel** (ou d' **autres langages > Visual C++ > Windows universel**).

Deux modèles sont installés : l' **application UWP multi-instance**, qui fournit le modèle pour créer une application multi-instance et une **application UWP pour la redirection multi-instance**, qui fournit une logique supplémentaire sur laquelle vous pouvez créer pour lancer une nouvelle instance ou activer de manière sélective une instance qui a déjà été lancée. Par exemple, si vous souhaitez qu’une seule instance à la fois modifie le même document, vous devez mettre l’instance qui a ouvert ce fichier au premier plan plutôt que de lancer une nouvelle instance.

Les deux modèles sont ajoutés `SupportsMultipleInstances` au `package.appxmanifest` fichier. Notez le préfixe d’espace `desktop4` de noms et `iot2` : seuls les projets qui ciblent les projets de bureau ou d’Internet des objets (IOT) prennent en charge l’instanciation multiple.

```xml
<Package
  ...
  xmlns:desktop4="http://schemas.microsoft.com/appx/manifest/desktop/windows10/4"
  xmlns:iot2="http://schemas.microsoft.com/appx/manifest/iot/windows10/2"  
  IgnorableNamespaces="uap mp desktop4 iot2">
  ...
  <Applications>
    <Application Id="App"
      ...
      desktop4:SupportsMultipleInstances="true"
      iot2:SupportsMultipleInstances="true">
      ...
    </Application>
  </Applications>
   ...
</Package>
```

## <a name="multi-instance-activation-redirection"></a>Redirection d’activation de plusieurs instances

 La prise en charge de l’instanciation multiple pour les applications UWP va au-delà du simple fait qu’il est possible de lancer plusieurs instances de l’application. Elle permet la personnalisation dans les cas où vous souhaitez choisir si une nouvelle instance de votre application est lancée ou si une instance déjà en cours d’exécution est activée. Par exemple, si l’application est lancée pour modifier un fichier qui est déjà en cours de modification dans une autre instance, vous souhaiterez peut-être rediriger l’activation vers cette instance au lieu d’ouvrir une autre instance qui est déjà en train de modifier le fichier.

Pour le voir en action, regardez cette vidéo sur la création d’applications UWP multi-instances.

> [!VIDEO https://www.youtube.com/embed/clnnf4cigd0]

Le modèle d' **application UWP redirection multi-instance** est ajouté `SupportsMultipleInstances` au fichier Package. appxmanifest comme indiqué ci-dessus, et ajoute également un **Program.cs** (ou **Program. cpp**, si vous utilisez la version C++ du modèle) à votre projet qui contient une `Main()` fonction. La logique de redirection de l’activation est insérée dans la `Main` fonction. Le modèle de **Program.cs** est illustré ci-dessous.

La propriété [**AppInstance. RecommendedInstance**](/uwp/api/windows.applicationmodel.appinstance.recommendedinstance) représente l’instance préférée fournie par l’interpréteur de commandes pour cette demande d’activation, le cas échéant (ou `null` s’il n’y en a pas). Si l’interpréteur de commandes fournit une préférence, vous pouvez rediriger l’activation vers cette instance, ou vous pouvez l’ignorer si vous le souhaitez.

``` csharp
public static class Program
{
    // This example code shows how you could implement the required Main method to
    // support multi-instance redirection. The minimum requirement is to call
    // Application.Start with a new App object. Beyond that, you may delete the
    // rest of the example code and replace it with your custom code if you wish.

    static void Main(string[] args)
    {
        // First, we'll get our activation event args, which are typically richer
        // than the incoming command-line args. We can use these in our app-defined
        // logic for generating the key for this instance.
        IActivatedEventArgs activatedArgs = AppInstance.GetActivatedEventArgs();

        // If the Windows shell indicates a recommended instance, then
        // the app can choose to redirect this activation to that instance instead.
        if (AppInstance.RecommendedInstance != null)
        {
            AppInstance.RecommendedInstance.RedirectActivationTo();
        }
        else
        {
            // Define a key for this instance, based on some app-specific logic.
            // If the key is always unique, then the app will never redirect.
            // If the key is always non-unique, then the app will always redirect
            // to the first instance. In practice, the app should produce a key
            // that is sometimes unique and sometimes not, depending on its own needs.
            string key = Guid.NewGuid().ToString(); // always unique.
                                                    //string key = "Some-App-Defined-Key"; // never unique.
            var instance = AppInstance.FindOrRegisterInstanceForKey(key);
            if (instance.IsCurrentInstance)
            {
                // If we successfully registered this instance, we can now just
                // go ahead and do normal XAML initialization.
                global::Windows.UI.Xaml.Application.Start((p) => new App());
            }
            else
            {
                // Some other instance has registered for this key, so we'll 
                // redirect this activation to that instance instead.
                instance.RedirectActivationTo();
            }
        }
    }
}
```

`Main()` est la première chose qui s’exécute. Il s’exécute avant [**OnLaunched**](/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_) et [**OnActivated**](/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnActivated_Windows_ApplicationModel_Activation_IActivatedEventArgs_). Cela vous permet de déterminer s’il faut activer cette, ou une autre instance, avant l’exécution de tout autre code d’initialisation dans votre application.

Le code ci-dessus détermine si une instance existante ou nouvelle de votre application est activée. Une clé est utilisée pour déterminer s’il existe une instance existante que vous souhaitez activer. Par exemple, si votre application peut être lancée pour [gérer l’activation de fichier](./handle-file-activation.md), vous pouvez utiliser le nom de fichier comme clé. Vous pouvez ensuite vérifier si une instance de votre application est déjà inscrite auprès de cette clé et l’activer au lieu d’ouvrir une nouvelle instance. Voici l’idée derrière le code : `var instance = AppInstance.FindOrRegisterInstanceForKey(key);`

Si une instance inscrite avec la clé est trouvée, cette instance est activée. Si la clé est introuvable, l’instance actuelle (l’instance en cours d’exécution `Main` ) crée son objet application et commence à s’exécuter.

## <a name="background-tasks-and-multi-instancing"></a>Tâches en arrière-plan et instanciation multiple

- Les tâches en arrière-plan out-of-proc prennent en charge l’instanciation multiple. En règle générale, chaque nouveau déclencheur génère une nouvelle instance de la tâche en arrière-plan (même si techniquement parlant, plusieurs tâches en arrière-plan peuvent s’exécuter dans le même processus hôte). Toutefois, une autre instance de la tâche en arrière-plan est créée.
- Les tâches en arrière-plan in-proc ne prennent pas en charge l’instanciation multiple.
- Les tâches audio en arrière-plan ne prennent pas en charge l’instanciation multiple.
- Quand une application enregistre une tâche en arrière-plan, elle commence par vérifier si la tâche est déjà inscrite, puis la supprime et la réinscrit, ou ne fait rien pour conserver l’inscription existante. Il s’agit toujours du comportement classique des applications multi-instances. Toutefois, une application à instanciation multiple peut choisir d’inscrire un nom de tâche en arrière-plan différent pour chaque instance. Cela aboutit à plusieurs inscriptions pour le même déclencheur, et plusieurs instances de tâche en arrière-plan sont activées lorsque le déclencheur est activé.
- App-services lance une instance distincte de la tâche d’arrière-plan App-service pour chaque connexion. Cela reste inchangé pour les applications multi-instances, chaque instance d’une application multi-instance obtiendra sa propre instance de la tâche d’arrière-plan App-service. 

## <a name="additional-considerations"></a>Considérations supplémentaires

- L’instanciation multiple est prise en charge par les applications UWP qui ciblent des projets de bureau et de Internet des objets (IoT).
- Pour éviter les problèmes de concurrence et de conflit, les applications multi-instances doivent prendre des mesures pour partitionner/synchroniser l’accès aux paramètres, au stockage local des applications et à toute autre ressource (par exemple, les fichiers utilisateur, un magasin de données, etc.) qui peuvent être partagées entre plusieurs instances. Les mécanismes de synchronisation standard, tels que les mutex, les sémaphores, les événements, etc., sont disponibles.
- Si l’application a `SupportsMultipleInstances` dans son fichier Package. appxmanifest, ses extensions n’ont pas besoin d’être déclarées `SupportsMultipleInstances` . 
- Si vous ajoutez `SupportsMultipleInstances` à une autre extension, à l’exception des tâches en arrière-plan ou des services d’application, et que l’application qui héberge l’extension ne déclare pas également `SupportsMultipleInstances` dans son fichier Package. appxmanifest, une erreur de schéma est générée.
- Les applications peuvent utiliser [**la déclaration de**](./declare-background-tasks-in-the-application-manifest.md) groupe de ressources dans leur manifeste pour regrouper plusieurs tâches en arrière-plan dans le même hôte. Cela est en conflit avec le Multi-instanciation, où chaque activation entre dans un hôte distinct. Par conséquent, une application ne peut pas déclarer `SupportsMultipleInstances` et `ResourceGroup` dans son manifeste.

## <a name="sample"></a>Exemple

Consultez [exemple multi-instance](https://github.com/Microsoft/AppModelSamples/tree/master/Samples/BananaEdit) pour obtenir un exemple de redirection d’activation de plusieurs instances.

## <a name="see-also"></a>Voir aussi

[AppInstance. FindOrRegisterInstanceForKey](/uwp/api/windows.applicationmodel.appinstance#Windows_ApplicationModel_AppInstance_FindOrRegisterInstanceForKey_System_String_) 
 [AppInstance. GetActivatedEventArgs](/uwp/api/windows.applicationmodel.appinstance#Windows_ApplicationModel_AppInstance_GetActivatedEventArgs) 
 [AppInstance. RedirectActivationTo](/uwp/api/windows.applicationmodel.appinstance#Windows_ApplicationModel_AppInstance_RedirectActivationTo) 
 [Gérer l’activation d’application](./activate-an-app.md)