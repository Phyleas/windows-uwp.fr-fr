---
description: Découvrez comment les applications de bureau C# peuvent envoyer des notifications Toast locales et gérer l’utilisateur qui clique sur le Toast.
title: Envoyer une notification toast locale à partir d’applications de bureau en C#
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from desktop C# apps
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: 'Windows 10, Win32, Desktop, notifications Toast, envoyer un toast, envoyer un toast local, Desktop Bridge, msix, packages éparpillés, C#, C Sharp, Toast notification, WPF, envoyer une notification Toast, WPF, envoyer une notification Toast, envoyer une notification Toast, c#, envoyer une notification, WPF, envoyer une notification Toast, notification Toast WPF, notification Toast C #'
ms.localizationpriority: medium
ms.openlocfilehash: cb91a76db38623b533a925ea1df4728bc0fead78
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034472"
---
# <a name="send-a-local-toast-notification-from-desktop-c-apps"></a>Envoyer une notification toast locale à partir d’applications de bureau en C#

Les applications de bureau (y compris les applications [MSIX](/windows/msix/desktop/source-code-overview) empaquetées, les applications qui utilisent des [packages éparss](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) pour obtenir l’identité des packages et les applications de bureau non packagées classiques) peuvent envoyer des notifications de Toast interactif comme les applications Windows. Toutefois, il existe quelques étapes spéciales pour les applications de bureau en raison des différents schémas d’activation et de l’absence potentielle d’identité de package si vous n’utilisez pas de packages MSIX ou épars.

> [!IMPORTANT]
> Si vous écrivez une application UWP, consultez la [documentation UWP](send-local-toast.md). Pour les autres langages de bureau, veuillez consulter [Win32 C++ WRL](send-local-toast-desktop-cpp-wrl.md).


## <a name="step-1-install-the-notifications-library"></a>Étape 1 : installer la bibliothèque de notifications

Installez le `Microsoft.Toolkit.Uwp.Notifications` [package NuGet](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) dans votre projet.

Cette [bibliothèque de notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) ajoute le code de bibliothèque de compatibilité pour l’utilisation des notifications toast à partir d’applications de bureau. Elle fait également référence aux kits de développement logiciel (SDK) UWP et vous permet de créer des notifications à l’aide de C# au lieu de XML brut. Le reste de ce guide de démarrage rapide dépend de la bibliothèque de notifications.


## <a name="step-2-implement-the-activator"></a>Étape 2 : implémenter l’activateur

Vous devez implémenter un gestionnaire pour l’activation de Toast, de sorte que lorsque l’utilisateur clique sur votre toast, votre application peut effectuer une opération. Cela est nécessaire pour que votre toast soit conservé dans le centre de notifications (dans la mesure où le Toast peut être cliqué plus tard pendant la fermeture de votre application). Cette classe peut être placée n’importe où dans votre projet.

Créez une nouvelle classe **MyNotificationActivator** et étendez la classe **NotificationActivator** . Ajoutez les trois attributs listés ci-dessous, puis créez un CLSID GUID unique pour votre application à l’aide de l’un des nombreux générateurs GUID en ligne. Ce CLSID (identificateur de classe) indique comment le centre de maintenance sait quelle classe activer COM.

**MyNotificationActivator.cs** (créer ce fichier)

```csharp
// The GUID CLSID must be unique to your app. Create a new GUID if copying this code.
[ClassInterface(ClassInterfaceType.None)]
[ComSourceInterfaces(typeof(INotificationActivationCallback))]
[Guid("replaced-with-your-guid-C173E6ADF0C3"), ComVisible(true)]
public class MyNotificationActivator : NotificationActivator
{
    public override void OnActivated(string invokedArgs, NotificationUserInput userInput, string appUserModelId)
    {
        // TODO: Handle activation
    }
}
```


## <a name="step-3-register-with-notification-platform"></a>Étape 3 : s’inscrire auprès de la plateforme de notification

Ensuite, vous devez vous inscrire auprès de la plateforme de notification. Différentes étapes varient selon que vous utilisez des packages MSIX/épars ou un bureau classique. Si vous prenez en charge les deux, vous devez effectuer les deux étapes (Toutefois, vous n’avez pas besoin de répliquer votre code, notre bibliothèque le gère pour vous !).


#### <a name="msixsparse-packages"></a>[Packages MSIX/Sparse](#tab/msix-sparse)

Si vous utilisez un package [MSIX](/windows/msix/desktop/source-code-overview) ou [Sparse](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) (ou si vous prenez en charge les deux), dans votre **Package. appxmanifest** , ajoutez :

1. Déclaration pour **xmlns : com**
2. Déclaration pour **xmlns : Desktop**
3. Dans l’attribut **ignorablenamespaces en spécifiant** , **com** et **Desktop**
4. **com : extension** pour l’activateur com à l’aide du GUID de l’étape #2. Veillez à inclure le `Arguments="-ToastActivated"` pour que vous sachiez que votre lancement s’est bien fait d’un toast
5. **Desktop : extension** pour **Windows. toastNotificationActivation** pour déclarer le CLSID de votre activateur Toast (le GUID de l’étape #2).

**Package.appxmanifest**

```xml
<!--Add these namespaces-->
<Package
  ...
  xmlns:com="http://schemas.microsoft.com/appx/manifest/com/windows10"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="... com desktop">
  ...
  <Applications>
    <Application>
      ...
      <Extensions>

        <!--Register COM CLSID LocalServer32 registry key-->
        <com:Extension Category="windows.comServer">
          <com:ComServer>
            <com:ExeServer Executable="YourProject\YourProject.exe" Arguments="-ToastActivated" DisplayName="Toast activator">
              <com:Class Id="replaced-with-your-guid-C173E6ADF0C3" DisplayName="Toast activator"/>
            </com:ExeServer>
          </com:ComServer>
        </com:Extension>

        <!--Specify which CLSID to activate when toast clicked-->
        <desktop:Extension Category="windows.toastNotificationActivation">
          <desktop:ToastNotificationActivation ToastActivatorCLSID="replaced-with-your-guid-C173E6ADF0C3" /> 
        </desktop:Extension>

      </Extensions>
    </Application>
  </Applications>
 </Package>
```


#### <a name="unpackaged"></a>[Désassemblées](#tab/classic)

Si vous n’utilisez pas MSIX/Sparse (ou si vous prenez en charge les deux), vous devez déclarer l’ID de modèle utilisateur de l’application (identifiant AUMID) et le CLSID de l’activateur Toast (le GUID à partir de l’étape #2) sur le raccourci de votre application dans Démarrer.

Choisissez un identifiant AUMID unique qui identifie votre application de bureau. Il se présente généralement sous la forme [CompanyName]. [AppName], mais vous voulez vous assurer qu’il est unique pour toutes les applications (n’hésitez pas à ajouter des chiffres à la fin).

### <a name="step-31-wix-installer"></a>Étape 3,1 : programme d’installation de WiX

Si vous utilisez WiX pour votre programme d’installation, modifiez le fichier **Product. wxs** pour ajouter les deux propriétés de raccourci au raccourci du menu Démarrer, comme indiqué ci-dessous. Assurez-vous que le GUID de l’étape #2 est inséré `{}` comme indiqué ci-dessous.

**Product. wxs**

```xml
<Shortcut Id="ApplicationStartMenuShortcut" Name="Wix Sample" Description="Wix Sample" Target="[INSTALLFOLDER]WixSample.exe" WorkingDirectory="INSTALLFOLDER">
                    
    <!--AUMID-->
    <ShortcutProperty Key="System.AppUserModel.ID" Value="YourCompany.YourApp"/>
    
    <!--COM CLSID-->
    <ShortcutProperty Key="System.AppUserModel.ToastActivatorCLSID" Value="{replaced-with-your-guid-C173E6ADF0C3}"/>
    
</Shortcut>
```

> [!IMPORTANT]
> Pour utiliser réellement les notifications, vous devez installer votre application par le biais du programme d’installation avant de procéder au débogage normalement, afin que le raccourci de démarrage avec votre identifiant AUMID et CLSID soit présent. Une fois le raccourci de démarrage présent, vous pouvez déboguer à l’aide de la touche F5 à partir de Visual Studio.


### <a name="step-32-register-aumid-and-com-server"></a>Étape 3,2 : inscrire identifiant AUMID et le serveur COM

Ensuite, quel que soit votre programme d’installation, dans le code de démarrage de votre application (avant d’appeler les API de notification), appelez la méthode **RegisterAumidAndComServer** , en spécifiant votre classe d’activateur de notification à partir de l’étape #2 et votre identifiant aumid utilisé ci-dessus.

```csharp
// Register AUMID and COM server (for MSIX/sparse package apps, this no-ops)
DesktopNotificationManagerCompat.RegisterAumidAndComServer<MyNotificationActivator>("YourCompany.YourApp");
```

Si vous prenez en charge à la fois le package MSIX/Sparse et le bureau classique, n’hésitez pas à appeler cette méthode, quelle que soit la valeur. Si vous exécutez dans un package MSIX/Sparse, cette méthode sera simplement retournée immédiatement. Vous n’avez pas besoin de dupliquer votre code.

Cette méthode vous permet d’appeler les API de compatibilité pour envoyer et gérer des notifications sans avoir à fournir constamment vos identifiant AUMID. Elle insère la clé de registre LocalServer32 pour le serveur COM.

---


## <a name="step-4-register-com-activator"></a>Étape 4 : inscrire l’activateur COM

Pour le package MSIX/Sparse et les applications de bureau classiques, vous devez inscrire votre type d’activateur de notification, afin de pouvoir gérer les activations Toast.

Dans le code de démarrage de votre application, appelez la méthode **RegisterActivator** suivante, en transmettant votre implémentation de la classe **NotificationActivator** que vous avez créée à l’étape #2. Cette méthode doit être appelée pour que vous puissiez recevoir des activations Toast.

```csharp
// Register COM server and activator type
DesktopNotificationManagerCompat.RegisterActivator<MyNotificationActivator>();
```


## <a name="step-5-send-a-notification"></a>Étape 5 : envoyer une notification

L’envoi d’une notification est identique aux applications UWP, sauf que vous allez utiliser la classe **DesktopNotificationManagerCompat** pour créer un **ToastNotifier** . La bibliothèque de compatibilité gère automatiquement la différence entre le package MSIX/Sparse et le bureau classique, ce qui vous permet de ne pas avoir à dupliquer votre code. Pour le bureau classique, la bibliothèque de compatibilité met en cache votre identifiant AUMID que vous avez fournie lors de l’appel de **RegisterAumidAndComServer** afin que vous n’ayez pas à vous soucier du moment auquel fournir ou ne pas fournir le identifiant aumid.

> [!NOTE]
> Installez la [bibliothèque de notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) afin de pouvoir créer des notifications à l’aide de C#, comme indiqué ci-dessous, au lieu d’utiliser des données XML brutes.

Veillez à utiliser le **ToastContent** présenté ci-dessous (ou le modèle ToastGeneric si vous créez un fichier XML), car les modèles de notification Windows 8.1 Toast hérités n’activeront pas votre activateur de notification com que vous avez créé à l’étape #2.

> [!IMPORTANT]
> Les images http sont uniquement prises en charge dans les applications de package MSIX/Sparse qui disposent de la fonctionnalité Internet dans leur manifeste. Les applications de bureau classiques ne prennent pas en charge les images http ; vous devez télécharger l’image dans vos données d’application locales et la référencer localement.

```csharp
// Construct the visuals of the toast (using Notifications library)
ToastContent toastContent = new ToastContentBuilder()
    .AddToastActivationInfo("action=viewConversation&conversationId=5", ToastActivationType.Foreground)
    .AddText("Hello world!")
    .GetToastContent();

// And create the toast notification
var toast = new ToastNotification(toastContent.GetXml());

// And then show it
DesktopNotificationManagerCompat.CreateToastNotifier().Show(toast);
```

> [!IMPORTANT]
> Les applications de bureau classiques ne peuvent pas utiliser les modèles Toast hérités (comme ToastText02). L’activation des modèles hérités échouera lorsque le CLSID COM sera spécifié. Vous devez utiliser les modèles ToastGeneric Windows 10 comme indiqué ci-dessus.


## <a name="step-6-handling-activation"></a>Étape 6 : gestion de l’activation

Quand l’utilisateur clique sur votre toast, la méthode **OnActivated** de votre classe **NotificationActivator** est appelée.

À l’intérieur de la méthode OnActivated, vous pouvez analyser les arguments que vous avez spécifiés dans le Toast et obtenir l’entrée utilisateur tapée ou sélectionnée par l’utilisateur, puis activer votre application en conséquence.

> [!NOTE]
> La méthode **OnActivated** n’est pas appelée sur le thread d’interface utilisateur. Si vous souhaitez exécuter des opérations de thread d’interface utilisateur, vous devez appeler `Application.Current.Dispatcher.Invoke(callback)` .

```csharp
// The GUID must be unique to your app. Create a new GUID if copying this code.
[ClassInterface(ClassInterfaceType.None)]
[ComSourceInterfaces(typeof(INotificationActivationCallback))]
[Guid("replaced-with-your-guid-C173E6ADF0C3"), ComVisible(true)]
public class MyNotificationActivator : NotificationActivator
{
    public override void OnActivated(string invokedArgs, NotificationUserInput userInput, string appUserModelId)
    {
        Application.Current.Dispatcher.Invoke(delegate
        {
            // Tapping on the top-level header launches with empty args
            if (arguments.Length == 0)
            {
                // Perform a normal launch
                OpenWindowIfNeeded();
                return;
            }

            // Parse the query string (using NuGet package QueryString.NET)
            QueryString args = QueryString.Parse(invokedArgs);

            // See what action is being requested 
            switch (args["action"])
            {
                // Open the image
                case "viewImage":

                    // The URL retrieved from the toast args
                    string imageUrl = args["imageUrl"];

                    // Make sure we have a window open and in foreground
                    OpenWindowIfNeeded();

                    // And then show the image
                    (App.Current.Windows[0] as MainWindow).ShowImage(imageUrl);

                    break;

                // Background: Quick reply to the conversation
                case "reply":

                    // Get the response the user typed
                    string msg = userInput["tbReply"];

                    // And send this message
                    SendMessage(msg);

                    // If there's no windows open, exit the app
                    if (App.Current.Windows.Count == 0)
                    {
                        Application.Current.Shutdown();
                    }

                    break;
            }
        });
    }

    private void OpenWindowIfNeeded()
    {
        // Make sure we have a window open (in case user clicked toast while app closed)
        if (App.Current.Windows.Count == 0)
        {
            new MainWindow().Show();
        }

        // Activate the window, bringing it to focus
        App.Current.Windows[0].Activate();

        // And make sure to maximize the window too, in case it was currently minimized
        App.Current.Windows[0].WindowState = WindowState.Normal;
    }
}
```

Pour une prise en charge correcte du lancement de votre application, dans votre `App.xaml.cs` fichier, vous souhaiterez peut-être substituer la méthode **OnStartup** (pour les applications WPF) pour déterminer si vous êtes lancé à partir d’un toast ou non. S’il est lancé à partir d’un toast, il y aura un ARG de lancement de « -ToastActivated ». Quand vous voyez cela, vous devez arrêter d’exécuter tout code d’activation de lancement normal et autoriser le lancement de votre handle de code **OnActivated** .

```csharp
protected override async void OnStartup(StartupEventArgs e)
{
    // Register AUMID, COM server, and activator
    DesktopNotificationManagerCompat.RegisterAumidAndComServer<MyNotificationActivator>("YourCompany.YourApp");
    DesktopNotificationManagerCompat.RegisterActivator<MyNotificationActivator>();

    // If launched from a toast
    if (e.Args.Contains("-ToastActivated"))
    {
        // Our NotificationActivator code will run after this completes,
        // and will show a window if necessary.
    }

    else
    {
        // Show the window
        // In App.xaml, be sure to remove the StartupUri so that a window doesn't
        // get created by default, since we're creating windows ourselves (and sometimes we
        // don't want to create a window if handling a background activation).
        new MainWindow().Show();
    }

    base.OnStartup(e);
}
```


### <a name="activation-sequence-of-events"></a>Séquence d’activation des événements

Pour WPF, la séquence d’activation est la suivante...

Si votre application est déjà en cours d’exécution :

1. **OnActivated** dans votre **NotificationActivator** est appelé

Si votre application n’est pas en cours d’exécution :

1. **OnStartup** dans `App.xaml.cs` est appelé avec les **arguments** « -ToastActivated »
2. **OnActivated** dans votre **NotificationActivator** est appelé


### <a name="foreground-vs-background-activation"></a>Activation au premier plan et en arrière-plan
Pour les applications de bureau, le premier plan et l’activation en arrière-plan sont gérés de la même façon : votre activateur COM est appelé. Il s’agit du code de votre application pour décider s’il faut afficher une fenêtre ou simplement effectuer un travail, puis quitter. Par conséquent, la spécification d’un **ActivationType** de l' **arrière-plan** dans votre contenu Toast ne change pas le comportement.


## <a name="step-7-remove-and-manage-notifications"></a>Étape 7 : supprimer et gérer les notifications

La suppression et la gestion des notifications sont identiques aux applications UWP. Toutefois, nous vous recommandons d’utiliser notre bibliothèque de compatibilité pour obtenir un **DesktopNotificationHistoryCompat** . vous n’avez donc pas à vous soucier de la fourniture de identifiant aumid si vous utilisez le bureau classique.

```csharp
// Remove the toast with tag "Message2"
DesktopNotificationManagerCompat.History.Remove("Message2");

// Clear all toasts
DesktopNotificationManagerCompat.History.Clear();
```


## <a name="step-8-deploying-and-debugging"></a>Étape 8 : déploiement et débogage

Pour déployer et déboguer votre application MSIX, consultez [exécuter, déboguer et tester une application de bureau empaquetée](/windows/msix/desktop/desktop-to-uwp-debug).

Pour déployer et déboguer votre application de bureau classique, vous devez installer votre application par le biais du programme d’installation avant de déboguer normalement, afin que le raccourci de démarrage avec vos identifiant AUMID et CLSID soit présent. Une fois le raccourci de démarrage présent, vous pouvez déboguer à l’aide de la touche F5 à partir de Visual Studio.

Si vos notifications ne s’affichent pas dans votre application de bureau classique (et qu’aucune exception n’est levée), cela signifie probablement que le raccourci de démarrage n’est pas présent (installer votre application via le programme d’installation), ou que le identifiant AUMID que vous avez utilisé dans le code ne correspond pas à identifiant AUMID dans votre raccourci de démarrage.

Si vos notifications s’affichent mais ne sont pas conservées dans le centre de maintenance (disparition après la fermeture de la fenêtre contextuelle), cela signifie que vous n’avez pas implémenté correctement l’activateur COM.

Si vous avez installé à la fois votre package MSIX/Sparse et l’application de bureau Classic, Notez que l’application de package MSIX/Sparse remplace l’application de bureau classique lors du traitement des activations de Toast. Cela signifie que les toasts de l’application de bureau classique lanceront toujours l’application de package MSIX/Sparse quand vous cliquez dessus. La désinstallation de l’application de package MSIX/Sparse repasse l’activation à l’application de bureau classique.


## <a name="known-issues"></a>Problèmes connus

Problème **résolu : l’application ne devient pas focalisée après avoir cliqué sur Toast** : dans les builds 15063 et antérieures, les droits de premier plan n’ont pas été transférés à votre application lorsque nous avons activé le serveur com. Par conséquent, votre application clignoterait simplement quand vous avez essayé de la déplacer au premier plan. Il n’existe aucune solution de contournement pour ce problème. Nous avons résolu ce problème dans les versions 16299 et supérieures.


## <a name="resources"></a>Ressources

* [Exemple de code complet sur GitHub](https://github.com/WindowsNotifications/desktop-toasts)
* [Notifications toast à partir d’applications de bureau](toast-desktop-apps.md)
* [Documentation sur le contenu Toast](adaptive-interactive-toasts.md)
