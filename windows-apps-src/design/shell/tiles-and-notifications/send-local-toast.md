---
Description: Découvrez comment envoyer une notification Toast locale et gérer l’utilisateur en cliquant sur le Toast.
title: Envoyer une notification toast locale
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP, envoyer des notifications Toast, notifications, envoyer des notifications, Toast notifications, guide pratique, démarrage rapide, prise en main, exemple de code, procédure pas à pas
ms.localizationpriority: medium
ms.openlocfilehash: 8e099ae97f67ca2f61a9e771f7ad015305b851d0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174593"
---
# <a name="send-a-local-toast-notification"></a>Envoyer une notification toast locale


Une notification Toast est un message qu’une application peut construire et remettre à l’utilisateur alors qu’elle ne se trouve pas actuellement à l’intérieur de votre application. Ce guide de démarrage rapide vous guide tout au long des étapes de création, de distribution et d’affichage d’une notification Toast Windows 10 avec les nouveaux modèles adaptatifs et les actions interactives. Ces actions sont illustrées par une notification locale, qui est la notification la plus simple à implémenter.

> [!IMPORTANT]
> Les applications de bureau (y compris les applications [MSIX](/windows/msix/desktop/source-code-overview) empaquetées, les applications qui utilisent des [packages éparss](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) pour obtenir l’identité des packages et les applications Win32 non empaquetées classiques) ont des étapes différentes pour envoyer des notifications et gérer l’activation. Pour savoir comment implémenter des toasts, consultez la documentation sur les [applications de bureau](toast-desktop-apps.md) .

Nous allons examiner les éléments suivants :

### <a name="sending-a-toast"></a>Envoi d’un toast

* Construction du composant visuel (texte et image) de la notification
* Ajout d’actions à la notification
* Définition d’un délai d’expiration sur le Toast
* Définir la balise/le groupe afin de pouvoir remplacer/supprimer le Toast ultérieurement
* Envoi de votre toast à l’aide des API locales

### <a name="handling-activation"></a>Gestion de l’activation

* Gestion de l’activation lorsque l’utilisateur clique sur le corps ou sur les boutons
* Gestion de l’activation au premier plan
* Gestion de l’activation en arrière-plan

> **API importantes**: [classe ToastNotification](/uwp/api/Windows.UI.Notifications.ToastNotification), [classe ToastNotificationActivatedEventArgs](/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)


## <a name="prerequisites"></a>Prérequis

Pour bien comprendre cette rubrique, les éléments suivants sont utiles...

* Une connaissance pratique des termes et des concepts de notification Toast. Pour plus d’informations, consultez [Toast and Action Center Overview](/archive/blogs/tiles_and_toasts/toast-notification-and-action-center-overview-for-windows-10).
* Une bonne connaissance du contenu de notification Windows 10 Toast. Pour plus d’informations, consultez [la documentation sur le contenu Toast](adaptive-interactive-toasts.md).
* Un projet d’application UWP Windows 10

> [!NOTE]
> Contrairement à Windows 8/8.1, vous n’avez plus besoin de déclarer dans le manifeste de votre application que votre application est capable d’illustrer les notifications Toast. Toutes les applications sont en charge de l’envoi et de l’affichage des notifications Toast.

> [!NOTE]
> **Applications Windows 8/8.1**: utilisez la [documentation archivée](/previous-versions/windows/apps/hh868254(v=win.10)).


## <a name="install-nuget-packages"></a>Installer les packages NuGet

Nous vous recommandons d’installer les deux packages NuGet suivants dans votre projet. Notre exemple de code utilisera ces packages. À la fin de l’article, nous fournirons les extraits de code « vanille » qui n’utilisent pas de packages NuGet.

* [Microsoft. Toolkit. UWP. notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/): générer des charges utiles Toast par le biais d’objets au lieu de données XML brutes.
* [QueryString.net](https://www.nuget.org/packages/QueryString.NET/): générer et analyser les chaînes de requête avec C #


## <a name="add-namespace-declarations"></a>Ajout de déclarations d'espaces de noms

`Windows.UI.Notifications` comprend les API Toast.

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
using Microsoft.QueryStringDotNET; // QueryString.NET
```


## <a name="send-a-toast"></a>Envoyer un toast

Dans Windows 10, le contenu de vos notifications Toast est décrit à l’aide d’un langage adaptable qui offre une grande flexibilité en ce qui concerne l’apparence de votre notification. Pour plus d’informations, consultez la documentation sur le [contenu Toast](adaptive-interactive-toasts.md) .

### <a name="constructing-the-visual-part-of-the-content"></a>Construction de la partie visuelle du contenu

Commençons par construire la partie visuelle du contenu, qui comprend le texte et les images que l’utilisateur doit voir.

Grâce à la bibliothèque de notifications, la génération du contenu XML est simple. Si vous n’installez pas la bibliothèque de notifications à partir de NuGet, vous devez construire le XML manuellement, ce qui laisse de l’espace pour les erreurs.

> [!NOTE]
> Les images peuvent être utilisées à partir du package de l’application, du stockage local de l’application ou du Web. À compter de la mise à jour des créateurs de automne, les images Web peuvent atteindre jusqu’à 3 Mo sur des connexions normales et 1 Mo sur les connexions limitées. Sur les appareils qui n’exécutent pas encore la mise à jour des créateurs de automne, les images Web ne doivent pas dépasser 200 Ko.

```csharp
// In a real app, these would be initialized with actual data
string title = "Andrew sent you a picture";
string content = "Check this out, Happy Canyon in Utah!";
string image = "https://picsum.photos/360/202?image=883";
string logo = "ms-appdata:///local/Andrew.jpg";
 
// Construct the visuals of the toast
ToastVisual visual = new ToastVisual()
{
    BindingGeneric = new ToastBindingGeneric()
    {
        Children =
        {
            new AdaptiveText()
            {
                Text = title
            },
 
            new AdaptiveText()
            {
                Text = content
            },
 
            new AdaptiveImage()
            {
                Source = image
            }
        },
 
        AppLogoOverride = new ToastGenericAppLogo()
        {
            Source = logo,
            HintCrop = ToastGenericAppLogoCrop.Circle
        }
    }
};
```


### <a name="constructing-actions-part-of-the-content"></a>Construction d’actions dans le contenu

À présent, nous allons ajouter des actions au contenu.

Dans l’exemple ci-dessous, nous avons inclus un élément input qui permet à l’utilisateur d’entrer du texte, qui est retourné à l’application lorsque l’utilisateur clique sur l’un des boutons ou sur le Toast lui-même.

Nous avons ensuite ajouté deux boutons, chacun avec son propre type d’activation, son propre contenu et ses arguments.
* **ActivationType** est utilisé pour spécifier la façon dont votre application souhaite être activée lorsque cette action est effectuée par l’utilisateur. Vous pouvez choisir de lancer votre application au premier plan, de lancer une tâche en arrière-plan ou de lancer une autre application. Que votre application choisisse le premier plan ou l’arrière-plan, vous recevrez toujours l’entrée utilisateur et les arguments que vous avez spécifiés, afin que votre application puisse exécuter l’action correcte, par exemple l’envoi du message ou l’ouverture d’une conversation.

```csharp
// In a real app, these would be initialized with actual data
int conversationId = 384928;
 
// Construct the actions for the toast (inputs and buttons)
ToastActionsCustom actions = new ToastActionsCustom()
{
    Inputs =
    {
        new ToastTextBox("tbReply")
        {
            PlaceholderContent = "Type a response"
        }
    },
 
    Buttons =
    {
        new ToastButton("Reply", new QueryString()
        {
            { "action", "reply" },
            { "conversationId", conversationId.ToString() }
 
        }.ToString())
        {
            ActivationType = ToastActivationType.Background,
            ImageUri = "Assets/Reply.png",
 
            // Reference the text box's ID in order to
            // place this button next to the text box
            TextBoxId = "tbReply"
        },
 
        new ToastButton("Like", new QueryString()
        {
            { "action", "like" },
            { "conversationId", conversationId.ToString() }
 
        }.ToString())
        {
            ActivationType = ToastActivationType.Background
        },
 
        new ToastButton("View", new QueryString()
        {
            { "action", "viewImage" },
            { "imageUrl", image }
 
        }.ToString())
    }
};
```


### <a name="combining-the-above-to-construct-the-full-content"></a>Combinaison de la version ci-dessus pour construire le contenu complet

La construction du contenu est maintenant terminée et nous pouvons l’utiliser pour instancier votre objet [**ToastNotification**](/uwp/api/Windows.UI.Notifications.ToastNotification) .

**Remarque**: vous pouvez également fournir un type d’activation à l’intérieur de l’élément racine, pour spécifier le type d’activation qui doit se produire lorsque l’utilisateur appuie sur le corps de la notification Toast. Normalement, le fait de cliquer sur le corps du Toast doit lancer votre application au premier plan pour créer une expérience utilisateur cohérente, mais vous pouvez utiliser d’autres types d’activation pour s’adapter à votre scénario spécifique où il est plus logique pour l’utilisateur.

Vous devez toujours définir la propriété **Launch** . par conséquent, quand l’utilisateur appuie sur le corps du Toast et que votre application est lancée, votre application connaît le contenu à afficher.

```csharp
// Now we can construct the final toast content
ToastContent toastContent = new ToastContent()
{
    Visual = visual,
    Actions = actions,
 
    // Arguments when the user taps body of toast
    Launch = new QueryString()
    {
        { "action", "viewConversation" },
        { "conversationId", conversationId.ToString() }
 
    }.ToString()
};
 
// And create the toast notification
var toast = new ToastNotification(toastContent.GetXml());
```


## <a name="set-an-expiration-time"></a>Définir un délai d’expiration

Dans Windows 10, toutes les notifications Toast sont placées dans le centre de maintenance après avoir été rejetées ou ignorées par l’utilisateur, afin que les utilisateurs puissent consulter votre notification une fois que la fenêtre contextuelle a disparu.

Toutefois, si le message de votre notification n’est pertinent que pendant une période donnée, vous devez définir un délai d’expiration sur la notification Toast afin que les utilisateurs ne voient pas les informations obsolètes de votre application. Par exemple, si une promotion est uniquement valide pendant 12 heures, définissez le délai d’expiration sur 12 heures. Dans le code ci-dessous, nous définissons l’heure d’expiration sur 2 jours.

> [!NOTE]
> La valeur par défaut et le délai d’expiration maximal pour les notifications Toast locales sont de 3 jours.

```csharp
toast.ExpirationTime = DateTime.Now.AddDays(2);
```


## <a name="provide-a-primary-key-for-your-toast"></a>Fournir une clé primaire pour votre toast

Si vous souhaitez supprimer ou remplacer par programmation la notification que vous envoyez, vous devez utiliser la propriété Tag (et éventuellement la propriété Group) pour fournir une clé primaire pour votre notification. Ensuite, vous pouvez utiliser cette clé primaire à l’avenir pour supprimer ou remplacer la notification.

Pour plus d’informations sur le remplacement ou la suppression des notifications Toast déjà fournies, consultez [démarrage rapide : gestion des notifications Toast dans le centre de maintenance (XAML)](/previous-versions/windows/apps/dn631260(v=win.10)).

Les balises et les groupes fonctionnent comme une clé primaire composite. Group est l’identificateur plus générique, dans lequel vous pouvez assigner des groupes tels que « wallPosts », « messages », « friendRequests », etc. La balise doit ensuite identifier de manière unique la notification proprement dite à partir du groupe. À l’aide d’un groupe générique, vous pouvez supprimer toutes les notifications de ce groupe à l’aide de l' [API RemoveGroup](/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_).

```csharp
toast.Tag = "18365";
toast.Group = "wallPosts";
```


## <a name="send-the-notification"></a>Envoyer la notification

Une fois que vous avez initialisé votre toast, créez simplement un [ToastNotifier](/uwp/api/windows.ui.notifications.toastnotifier) et appelez Show (), en transmettant votre notification Toast.

```csharp
ToastNotificationManager.CreateToastNotifier().Show(toast);
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


## <a name="activation-handling"></a>Gestion de l’activation

Dans Windows 10, lorsque l’utilisateur clique sur votre toast, vous pouvez faire en sorte que le Toast active votre application de deux manières différentes...

* Activation au premier plan
* Activation en arrière-plan

> [!NOTE]
> Si vous utilisez les modèles Toast hérités de Windows 8.1, **OnLaunched** est appelé à la place de **OnActivated**. La documentation suivante s’applique uniquement aux notifications Windows 10 modernes qui utilisent la bibliothèque de notifications (ou le modèle ToastGeneric si vous utilisez du code XML brut).


### <a name="handling-foreground-activation"></a>Gestion de l’activation au premier plan

Dans Windows 10, lorsqu’un utilisateur clique sur un toast moderne (ou un bouton sur le Toast), **OnActivated** est appelé à la place de **OnLaunched**, avec un nouveau type d’activation – **ToastNotification**. Ainsi, le développeur est en mesure de distinguer facilement l’activation d’un toast et d’effectuer des tâches en conséquence.

Dans l’exemple ci-dessous, vous pouvez récupérer la chaîne d’arguments que vous avez initialement fournie dans le contenu Toast. Vous pouvez également récupérer l’entrée fournie par l’utilisateur dans vos zones de texte et zones de sélection.

> [!IMPORTANT]
> Vous devez initialiser votre Frame et activer votre fenêtre comme votre code **OnLaunched** . **OnLaunched n’est pas appelé si l’utilisateur clique sur votre toast**, même si votre application a été fermée et est lancée pour la première fois. Nous vous recommandons souvent de combiner **OnLaunched** et **OnActivated** dans votre propre `OnLaunchedOrActivated` méthode, car la même initialisation doit se produire dans les deux.

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Get the root frame
    Frame rootFrame = Window.Current.Content as Frame;
 
    // TODO: Initialize root frame just like in OnLaunched
 
    // Handle toast activation
    if (e is ToastNotificationActivatedEventArgs)
    {
        var toastActivationArgs = e as ToastNotificationActivatedEventArgs;
                 
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


Ensuite, dans votre App.xaml.cs, remplacez la méthode OnBackgroundActivated, vous pouvez récupérer les arguments prédéfinis et l’entrée utilisateur, de la même façon que l’activation de premier plan.

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



## <a name="plain-vanilla-code-snippets"></a>Extraits de code « vanille » en clair

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