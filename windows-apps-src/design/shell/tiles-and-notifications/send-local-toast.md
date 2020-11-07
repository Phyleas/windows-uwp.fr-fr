---
description: Découvrez comment envoyer une notification Toast locale à partir d’applications UWP et gérer l’utilisateur en cliquant sur le Toast.
title: Envoyer une notification Toast locale à partir d’applications UWP
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from UWP apps
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP, envoyer des notifications Toast, notifications, envoyer des notifications, Toast notifications, guide pratique, démarrage rapide, prise en main, exemple de code, procédure pas à pas
ms.localizationpriority: medium
ms.openlocfilehash: 4142fb3d036bb19eb652ca9048a70325eb64b17d
ms.sourcegitcommit: aaa72ddeb01b074266f4cd51740eec8d1905d62d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2020
ms.locfileid: "94339807"
---
# <a name="send-a-local-toast-notification-from-uwp-apps"></a>Envoyer une notification Toast locale à partir d’applications UWP


Une notification Toast est un message qu’une application peut construire et remettre à l’utilisateur alors qu’elle ne se trouve pas actuellement à l’intérieur de votre application. Ce guide de démarrage rapide vous guide tout au long des étapes de création, de distribution et d’affichage d’une notification Toast Windows 10 avec les nouveaux modèles adaptatifs et les actions interactives. Ces actions sont illustrées par une notification locale, qui est la notification la plus simple à implémenter.

> [!IMPORTANT]
> Les applications de bureau (y compris les applications [MSIX](/windows/msix/desktop/source-code-overview) empaquetées, les applications qui utilisent des [packages éparss](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) pour obtenir l’identité des packages et les applications de bureau non packagées classiques) présentent des étapes différentes pour l’envoi de notifications et la gestion de l’activation. Pour savoir comment implémenter des toasts, consultez la documentation sur les [applications de bureau](toast-desktop-apps.md) .

> **API importantes** : [classe ToastNotification](/uwp/api/Windows.UI.Notifications.ToastNotification), [classe ToastNotificationActivatedEventArgs](/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)



## <a name="step-1-install-nuget-package"></a>Étape 1 : installer le package NuGet

Installez le [package NuGet Microsoft. Toolkit. UWP. notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/). Notre exemple de code utilisera ce package. À la fin de l’article, nous fournirons les extraits de code « ordinaires » qui n’utilisent pas de packages NuGet. Ce package vous permet de créer des notifications de Toast sans utiliser XML.


## <a name="step-2-add-namespace-declarations"></a>Étape 2 : ajouter des déclarations d’espace de noms

`Windows.UI.Notifications` comprend les API Toast.

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
```


## <a name="step-3-send-a-toast"></a>Étape 3 : envoyer un toast

Dans Windows 10, le contenu de vos notifications Toast est décrit à l’aide d’un langage adaptable qui offre une grande flexibilité en ce qui concerne l’apparence de votre notification. Pour plus d’informations, consultez la documentation sur le [contenu Toast](adaptive-interactive-toasts.md) .

Nous allons commencer par une simple notification textuelle. Construisez le contenu de la notification (à l’aide de la bibliothèque de notifications) et affichez la notification.

<img alt="Simple text notification" src="images/send-toast-01.png" width="364"/>

```csharp
// Construct the content
var content = new ToastContentBuilder()
    .AddToastActivationInfo("picOfHappyCanyon", ToastActivationType.Foreground)
    .AddText("Andrew sent you a picture")
    .AddText("Check this out, Happy Canyon in Utah!")
    .GetToastContent();

// Create the notification
var notif = new ToastNotification(content.GetXml());

// And show it!
ToastNotificationManager.CreateToastNotifier().Show();
```

## <a name="step-4-handling-activation"></a>Étape 4 : gestion de l’activation

Quand l’utilisateur clique sur votre notification (ou sur un bouton de la notification avec activation de premier plan), le **OnActivated** **app.Xaml.cs** de votre application est appelé.

**App.xaml.cs**

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Handle notification activation
    if (e is ToastNotificationActivatedEventArgs toastActivationArgs)
    {
        // Obtain the arguments from the notification
        string args = toastActivationArgs.Argument;

        // Obtain any user input (text boxes, menu selections) from the notification
        ValueSet userInput = toastActivationArgs.UserInput;
 
        // TODO: Show the corresponding content
    }
}
```

> [!IMPORTANT]
> Vous devez initialiser votre Frame et activer votre fenêtre comme votre code **OnLaunched** . **OnLaunched n’est pas appelé si l’utilisateur clique sur votre toast** , même si votre application a été fermée et est lancée pour la première fois. Nous vous recommandons souvent de combiner **OnLaunched** et **OnActivated** dans votre propre `OnLaunchedOrActivated` méthode, car la même initialisation doit se produire dans les deux.


## <a name="activation-in-depth"></a>Activation en profondeur

La première étape de l’action de vos notifications consiste à ajouter des arguments de lancement à votre notification, afin que votre application puisse savoir ce qui doit être lancé lorsque l’utilisateur clique sur la notification (dans ce cas, nous incluons des informations qui nous indiquent par la suite que nous devons ouvrir une conversation, et que nous savons la conversation spécifique à ouvrir).

Nous vous recommandons d’installer le package NuGet [QueryString.net](https://www.nuget.org/packages/QueryString.NET/) pour faciliter la construction et l’analyse des chaînes de requête pour vos arguments de notification, comme indiqué ci-dessous.

```csharp
using Microsoft.QueryStringDotNET; // QueryString.NET

int conversationId = 384928;

// Construct the content
var content = new ToastContentBuilder()

    // Arguments returned when user taps body of notification
    .AddToastActivationInfo(new QueryString() // Using QueryString.NET
    {
        { "action", "viewConversation" },
        { "conversationId", conversationId.ToString() }
    }.ToString(), ToastActivationType.Foreground)

    .AddText("Andrew sent you a picture")
    ...
```


Voici un exemple plus complexe de gestion de l’activation...

**App.xaml.cs**

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Get the root frame
    Frame rootFrame = Window.Current.Content as Frame;
 
    // TODO: Initialize root frame just like in OnLaunched
 
    // Handle toast activation
    if (e is ToastNotificationActivatedEventArgs toastActivationArgs)
    {            
        // Parse the query string (using QueryString.NET)
        QueryString args = QueryString.Parse(toastActivationArgs.Argument);
 
        // See what action is being requested 
        switch (args["action"])
        {
            // Open the image
            case "viewImage":
 
                // The URL retrieved from the toast args
                string imageUrl = args["imageUrl"];
 
                // If we're already viewing that image, do nothing
                if (rootFrame.Content is ImagePage && (rootFrame.Content as ImagePage).ImageUrl.Equals(imageUrl))
                    break;
 
                // Otherwise navigate to view it
                rootFrame.Navigate(typeof(ImagePage), imageUrl);
                break;
                             
 
            // Open the conversation
            case "viewConversation":
 
                // The conversation ID retrieved from the toast args
                int conversationId = int.Parse(args["conversationId"]);
 
                // If we're already viewing that conversation, do nothing
                if (rootFrame.Content is ConversationPage && (rootFrame.Content as ConversationPage).ConversationId == conversationId)
                    break;
 
                // Otherwise navigate to view it
                rootFrame.Navigate(typeof(ConversationPage), conversationId);
                break;
        }
 
        // If we're loading the app for the first time, place the main page on
        // the back stack so that user can go back after they've been
        // navigated to the specific page
        if (rootFrame.BackStack.Count == 0)
            rootFrame.BackStack.Add(new PageStackEntry(typeof(MainPage), null, null));
    }
 
    // TODO: Handle other types of activation
 
    // Ensure the current window is active
    Window.Current.Activate();
}
```


## <a name="adding-images"></a>Ajouter des images

Vous pouvez ajouter du contenu riche aux notifications. Nous allons ajouter une image en ligne et un profil (remplacement du logo de l’application).

> [!NOTE]
> Les images peuvent être utilisées à partir du package de l’application, du stockage local de l’application ou du Web. À compter de la mise à jour des créateurs de automne, les images Web peuvent atteindre jusqu’à 3 Mo sur des connexions normales et 1 Mo sur les connexions limitées. Sur les appareils qui n’exécutent pas encore la mise à jour des créateurs de automne, les images Web ne doivent pas dépasser 200 Ko.

> [!IMPORTANT]
> Les images http sont uniquement prises en charge dans les applications UWP/MSIX/épars qui disposent de la fonctionnalité Internet dans leur manifeste. Les applications de bureau non-MSIX/épars ne prennent pas en charge les images http ; vous devez télécharger l’image dans vos données d’application locales et la référencer localement.

<img alt="Toast with images" src="images/send-toast-02.png" width="364"/>

```csharp
// Construct the content
var content = new ToastContentBuilder()
    ...

    // Inline image
    .AddInlineImage(new Uri("https://picsum.photos/360/202?image=883"))

    // Profile (app logo override) image
    .AddAppLogoOverride(new Uri("ms-appdata:///local/Andrew.jpg"), ToastGenericAppLogoCrop.Circle)
    
    .GetToastContent();
    
...
```



## <a name="adding-buttons-and-inputs"></a>Ajout de boutons et d’entrées

Vous pouvez ajouter des boutons et des entrées pour rendre vos notifications interactives. Les boutons peuvent lancer votre application de premier plan, un protocole ou votre tâche en arrière-plan. Nous allons ajouter une zone de texte de réponse, un bouton « like » et un bouton « View » qui ouvre l’image.

<img alt="Toast with images and buttons" src="images/send-toast-03.png" width="364"/>

```csharp
int conversationId = 384928;

// Construct the content
var content = new ToastContentBuilder()
    ...

    // Text box for replying
    .AddInputTextBox("tbReply", placeHolderContent: "Type a response")

    // Reference the text box's ID in order to place this button next to the text box
    .AddButton("tbReply", "Reply", ToastActivationType.Background, new QueryString()
    {
        { "action", "reply" },
        { "conversationId", conversationId.ToString() }
    }.ToString(), imageUri: new Uri("Assets/Reply.png", UriKind.Relative))

    .AddButton("Like", ToastActivationType.Background, new QueryString()
    {
        { "action", "like" },
        { "conversationId", conversationId.ToString() }
    }.ToString())

    .AddButton("View", ToastActivationType.Foreground, new QueryString()
    {
        { "action", "viewImage" },
        { "imageUrl", image.ToString() }
    }.ToString())
    
    .GetToastContent();
    
...
```

L’activation des boutons de premier plan est gérée de la même façon que le corps Toast principal (votre App.xaml.cs OnActivated est appelé).



## <a name="handling-background-activation"></a>Gestion de l’activation en arrière-plan

Lorsque vous spécifiez l’activation en arrière-plan sur votre toast (ou sur un bouton à l’intérieur du Toast), votre tâche en arrière-plan est exécutée au lieu d’activer votre application de premier plan.

Pour plus d’informations sur les tâches en arrière-plan, consultez [prendre en charge votre application avec des tâches en arrière-plan](../../../launch-resume/support-your-app-with-background-tasks.md).

Si vous ciblez la version 14393 ou une version ultérieure, vous pouvez utiliser des tâches en arrière-plan in-process, ce qui simplifie beaucoup les choses. Notez que les tâches en arrière-plan in-process ne peuvent pas s’exécuter sur des versions antérieures de Windows. Nous allons utiliser une tâche en arrière-plan in-process dans cet exemple de code.

```csharp
const string taskName = "ToastBackgroundTask";

// If background task is already registered, do nothing
if (BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name.Equals(taskName)))
    return;

// Otherwise request access
BackgroundAccessStatus status = await BackgroundExecutionManager.RequestAccessAsync();

// Create the background task
BackgroundTaskBuilder builder = new BackgroundTaskBuilder()
{
    Name = taskName
};

// Assign the toast action trigger
builder.SetTrigger(new ToastNotificationActionTrigger());

// And register the task
BackgroundTaskRegistration registration = builder.Register();
```


Ensuite, dans votre App.xaml.cs, remplacez la méthode OnBackgroundActivated. Vous pouvez ensuite récupérer les arguments prédéfinis et l’entrée utilisateur, comme pour l’activation au premier plan.

**App.xaml.cs**

```csharp
protected override async void OnBackgroundActivated(BackgroundActivatedEventArgs args)
{
    var deferral = args.TaskInstance.GetDeferral();
 
    switch (args.TaskInstance.Task.Name)
    {
        case "ToastBackgroundTask":
            var details = args.TaskInstance.TriggerDetails as ToastNotificationActionTriggerDetail;
            if (details != null)
            {
                string arguments = details.Argument;
                var userInput = details.UserInput;

                // Perform tasks
            }
            break;
    }
 
    deferral.Complete();
}
```


## <a name="set-an-expiration-time"></a>Définir un délai d’expiration

Dans Windows 10, toutes les notifications Toast sont placées dans le centre de maintenance après avoir été rejetées ou ignorées par l’utilisateur, afin que les utilisateurs puissent consulter votre notification une fois que la fenêtre contextuelle a disparu.

Toutefois, si le message de votre notification n’est pertinent que pendant une période donnée, vous devez définir un délai d’expiration sur la notification Toast afin que les utilisateurs ne voient pas les informations obsolètes de votre application. Par exemple, si une promotion est uniquement valide pendant 12 heures, définissez le délai d’expiration sur 12 heures. Dans le code ci-dessous, nous définissons l’heure d’expiration sur 2 jours.

> [!NOTE]
> La valeur par défaut et le délai d’expiration maximal pour les notifications Toast locales sont de 3 jours.

```csharp
// Create toast content
var content = new ToastContentBuilder()
    .AddText("Expires in 2 days...")
    .GetToastContent();

// Set expiration time
var notif = new ToastNotification(content.GetXml())
{
    ExpirationTime = DateTime.Now.AddDays(2)
};

// And show it!
ToastNotificationManager.CreateToastNotifier().Show(notif);
```


## <a name="provide-a-primary-key-for-your-toast"></a>Fournir une clé primaire pour votre toast

Si vous souhaitez supprimer ou remplacer par programmation la notification que vous envoyez, vous devez utiliser la propriété Tag (et éventuellement la propriété Group) pour fournir une clé primaire pour votre notification. Ensuite, vous pouvez utiliser cette clé primaire à l’avenir pour supprimer ou remplacer la notification.

Pour plus d’informations sur le remplacement ou la suppression des notifications Toast déjà fournies, consultez [démarrage rapide : gestion des notifications Toast dans le centre de maintenance (XAML)](/previous-versions/windows/apps/dn631260(v=win.10)).

Les balises et les groupes fonctionnent comme une clé primaire composite. Group est l’identificateur plus générique, dans lequel vous pouvez assigner des groupes tels que « wallPosts », « messages », « friendRequests », etc. La balise doit ensuite identifier de manière unique la notification proprement dite à partir du groupe. À l’aide d’un groupe générique, vous pouvez supprimer toutes les notifications de ce groupe à l’aide de l' [API RemoveGroup](/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_).

```csharp
// Create toast content
var content = new ToastContentBuilder()
    .AddText("New post on your wall!")
    .GetToastContent();

// Set tag/group
new ToastNotification(content.GetXml())
{
    Tag = "18365",
    Group = "wallPosts"
};

// And show it!
ToastNotificationManager.CreateToastNotifier().Show(notif);
```



## <a name="clear-your-notifications"></a>Effacer vos notifications

Les applications UWP sont responsables de la suppression et de l’effacement de leurs propres notifications. Lorsque votre application est lancée, les notifications ne sont pas automatiquement effacées.

Windows supprimera automatiquement une notification uniquement si l’utilisateur clique explicitement sur la notification.

Voici un exemple de ce qu’une application de messagerie doit faire...

1. L’utilisateur reçoit plusieurs toasts sur les nouveaux messages d’une conversation
2. L’utilisateur appuie sur l’un de ces toasts pour ouvrir la conversation
3. L’application ouvre la conversation, puis efface tous les toasts pour cette conversation (en utilisant [RemoveGroup](/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_) sur le groupe d’applications pour cette conversation).
4. Le centre de maintenance de l’utilisateur reflète désormais correctement l’état de notification, car aucune notification périmée pour cette conversation n’est laissée dans le centre de maintenance.

Pour en savoir plus sur l’effacement de toutes les notifications ou la suppression de notifications spécifiques, consultez [démarrage rapide : gestion des notifications Toast dans le centre de maintenance (XAML)](/previous-versions/windows/apps/dn631260(v=win.10)).

```csharp
ToastNotificationManager.History.Clear();
```


## <a name="plain-code-snippets"></a>Extraits de code bruts

Si vous n’utilisez pas la bibliothèque de notifications de NuGet, vous pouvez construire manuellement votre code XML comme indiqué ci-dessous pour créer un [ToastNotification](/uwp/api/Windows.UI.Notifications.ToastNotification).

```csharp
using Windows.UI.Notifications;
using Windows.Data.Xml.Dom;

// In a real app, these would be initialized with actual data
string title = "Andrew sent you a picture";
string content = "Check this out, Happy Canyon in Utah!";
string image = "http://blogs.msdn.com/cfs-filesystemfile.ashx/__key/communityserver-blogs-components-weblogfiles/00-00-01-71-81-permanent/2727.happycanyon1_5B00_1_5D00_.jpg";
string logo = "ms-appdata:///local/Andrew.jpg";
 
// TODO: all values need to be XML escaped
 
// Construct the visuals of the toast
string toastVisual =
$@"<visual>
  <binding template='ToastGeneric'>
    <text>{title}</text>
    <text>{content}</text>
    <image src='{image}'/>
    <image src='{logo}' placement='appLogoOverride' hint-crop='circle'/>
  </binding>
</visual>";

// In a real app, these would be initialized with actual data
int conversationId = 384928;
 
// Generate the arguments we'll be passing in the toast
string argsReply = $"action=reply&conversationId={conversationId}";
string argsLike = $"action=like&conversationId={conversationId}";
string argsView = $"action=viewImage&imageUrl={Uri.EscapeDataString(image)}";
 
// TODO: all args need to be XML escaped
 
string toastActions =
$@"<actions>
 
  <input
      type='text'
      id='tbReply'
      placeHolderContent='Type a response'/>
 
  <action
      content='Reply'
      arguments='{argsReply}'
      activationType='background'
      imageUri='Assets/Reply.png'
      hint-inputId='tbReply'/>
 
  <action
      content='Like'
      arguments='{argsLike}'
      activationType='background'/>
 
  <action
      content='View'
      arguments='{argsView}'/>
 
</actions>";

// Now we can construct the final toast content
string argsLaunch = $"action=viewConversation&conversationId={conversationId}";
 
// TODO: all args need to be XML escaped
 
string toastXmlString =
$@"<toast launch='{argsLaunch}'>
    {toastVisual}
    {toastActions}
</toast>";
 
// Parse to XML
XmlDocument toastXml = new XmlDocument();
toastXml.LoadXml(toastXmlString);
 
// Generate toast
var toast = new ToastNotification(toastXml);
```


## <a name="resources"></a>Ressources

* [Exemple de code complet sur GitHub](https://github.com/WindowsNotifications/quickstart-sending-local-toast-win10)
* [Documentation sur le contenu Toast](adaptive-interactive-toasts.md)
* [ToastNotification, classe](/uwp/api/Windows.UI.Notifications.ToastNotification)
* [ToastNotificationActivatedEventArgs, classe](/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)