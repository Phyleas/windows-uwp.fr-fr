---
Description: Utilisez la classe ApplicationView pour présenter différentes parties de votre application dans des fenêtres distinctes.
title: Utiliser la classe ApplicationView pour montrer les fenêtres secondaires d’une application
ms.date: 07/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2005f254118c44b386879f771cc4e5c0ae2cadc4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165623"
---
# <a name="show-multiple-views-with-applicationview"></a>Afficher plusieurs vues avec ApplicationView

Aidez les utilisateurs à accroître leur productivité en leur permettant de voir des parties indépendantes de votre application dans des fenêtres distinctes. Quand vous créez plusieurs fenêtres pour une application, chacune d’elles se comporte de manière indépendante. La barre des tâches répertorie chaque fenêtre séparément. Les utilisateurs peuvent déplacer, redimensionner, afficher et masquer des fenêtres d’application indépendamment et ils peuvent basculer d’une fenêtre à une autre comme s’il s’agissait d’applications distinctes. Chaque fenêtre opère dans son propre thread.

> **API importantes** : [**ApplicationViewSwitcher**](/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitcher), [**CreateNewView**](/uwp/api/windows.applicationmodel.core.coreapplication.createnewview)

## <a name="what-is-a-view"></a>Qu’est-ce qu’une vue ?

Une vue d’application est l’association de type 1:1 d’un thread et d’une fenêtre que l’application utilise pour afficher le contenu. Elle est représentée par un objet [**Windows.ApplicationModel.Core.CoreApplicationView**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView).

Les vues sont gérées par l’objet [**CoreApplication**](/uwp/api/Windows.ApplicationModel.Core.CoreApplication). Vous devez appeler [**CoreApplication.CreateNewView**](/uwp/api/windows.applicationmodel.core.coreapplication.createnewview) pour créer un objet [**CoreApplicationView**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView). L’objet **CoreApplicationView** réunit [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) et [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) (stockés dans les propriétés [**CoreWindow**](/uwp/api/windows.applicationmodel.core.coreapplicationview.corewindow) et [**Dispatcher**](/uwp/api/windows.applicationmodel.core.coreapplicationview.dispatcher)). Vous pouvez considérer la **CoreApplicationView** comme l’objet qui utilise Windows Runtime pour interagir avec le système Windows principal.

En règle générale, vous ne travaillez pas directement avec la [**CoreApplicationView**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView). À la place, Windows Runtime fournit la classe [**ApplicationView**](/uwp/api/Windows.UI.ViewManagement.ApplicationView) dans l’espace de noms [**Windows.UI.ViewManagement**](/uwp/api/Windows.UI.ViewManagement). Cette classe fournit des propriétés, des méthodes et des événements que vous utilisez quand votre application interagit avec le système de fenêtrage. Pour fonctionner avec une **ApplicationView**, appelez la méthode statique [**ApplicationView.GetForCurrentView**](/uwp/api/windows.ui.viewmanagement.applicationview.getforcurrentview), qui obtient une instance **ApplicationView** liée au thread actuel de la **CoreApplicationView**.

De même, l’infrastructure XAML enveloppe l’objet [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) dans un objet [**Windows.UI.XAML.Window**](/uwp/api/Windows.UI.Xaml.Window). Dans une application XAML, vous interagissez généralement avec l’objet **Window** au lieu de travailler directement avec la **CoreWindow**.

## <a name="show-a-new-view"></a>Afficher une nouvelle vue

Chaque disposition d’application est unique, mais nous vous recommandons d’inclure un bouton « nouvelle fenêtre » à un emplacement prévisible comme le coin supérieur droit du contenu, qui pourra être ouvert dans une nouvelle fenêtre. Envisagez également d’inclure une option de menu contextuel pour « Ouvrir dans une nouvelle fenêtre ».

Examinons les étapes permettant de créer une vue. Ici, la nouvelle vue est lancée en réponse à un clic sur un bouton.

```csharp
private async void Button_Click(object sender, RoutedEventArgs e)
{
    CoreApplicationView newView = CoreApplication.CreateNewView();
    int newViewId = 0;
    await newView.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        Frame frame = new Frame();
        frame.Navigate(typeof(SecondaryPage), null);   
        Window.Current.Content = frame;
        // You have to activate the window in order to show it later.
        Window.Current.Activate();

        newViewId = ApplicationView.GetForCurrentView().Id;
    });
    bool viewShown = await ApplicationViewSwitcher.TryShowAsStandaloneAsync(newViewId);
}
```

**Pour montrer une nouvelle vue**

1.  Appelez [**CoreApplication.CreateNewView**](/uwp/api/windows.applicationmodel.core.coreapplication.createnewview) pour créer une fenêtre et un thread pour le contenu de la vue.

    ```csharp
    CoreApplicationView newView = CoreApplication.CreateNewView();
    ```

2.  Suivez l’élément [**Id**](/uwp/api/windows.ui.viewmanagement.applicationview.id) de la nouvelle vue. Vous pouvez utiliser cette fonction pour afficher la vue plus tard.

    Vous souhaiterez peut-être prendre en compte la création d’une infrastructure dans votre application pour faciliter le suivi des vues que vous créez. Voir la classe `ViewLifetimeControl` dans l’[exemple MultipleViews](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MultipleViews) pour plus d’informations.

    ```csharp
    int newViewId = 0;
    ```

3.  Sur le nouveau thread, remplissez la fenêtre.

    Vous devez utiliser la méthode [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) pour planifier le travail sur le thread d’interface utilisateur de la nouvelle vue. Vous utilisez une [expression lambda](/dotnet/csharp/language-reference/operators/lambda-expressions) pour transmettre une fonction en tant qu’argument à la méthode **RunAsync**. Le travail que vous effectuez dans la fonction lambda se répercute sur le thread de la nouvelle vue.

    En XAML, vous ajoutez généralement [**Frame**](/uwp/api/Windows.UI.Xaml.Controls.Frame) à la propriété [**Content**](/uwp/api/windows.ui.xaml.window.content) de [**Window**](/uwp/api/Windows.UI.Xaml.Window), puis parcourez **Frame** vers une [**Page**](/uwp/api/Windows.UI.Xaml.Controls.Page) XAML où vous avez défini le contenu de votre application. Pour plus d’informations sur les frames et les pages, consultez [Navigation pair à pair entre deux pages](../basics/navigate-between-two-pages.md).

    Une fois le nouvel élément [**Window**](/uwp/api/Windows.UI.Xaml.Window) rempli, vous devez appeler la méthode **[Activate](/uwp/api/windows.ui.xaml.window.activate)** de **Window** pour afficher l’élément **Window** ultérieurement. Cela se produit sur le thread de la nouvelle vue ; ainsi la nouvelle **Window** est activée.

    Enfin, récupérez l’élément [**Id**](/uwp/api/windows.ui.viewmanagement.applicationview.id) de la nouvelle vue que vous utilisez pour afficher la vue plus tard. Une fois encore, cela se produit sur le thread de la nouvelle vue ; ainsi [**ApplicationView.GetForCurrentView**](/uwp/api/windows.ui.viewmanagement.applicationview.getforcurrentview) obtient l’élément **Id** de la nouvelle vue.

    ```csharp
    await newView.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        Frame frame = new Frame();
        frame.Navigate(typeof(SecondaryPage), null);   
        Window.Current.Content = frame;
        // You have to activate the window in order to show it later.
        Window.Current.Activate();

        newViewId = ApplicationView.GetForCurrentView().Id;
    });
    ```

4.  Affichez la nouvelle vue en appelant [**ApplicationViewSwitcher.TryShowAsStandaloneAsync**](/uwp/api/windows.ui.viewmanagement.applicationviewswitcher.tryshowasstandaloneasync).

    Après avoir créé une vue, vous pouvez l’afficher dans une nouvelle fenêtre en appelant la méthode [**ApplicationViewSwitcher.TryShowAsStandaloneAsync**](/uwp/api/windows.ui.viewmanagement.applicationviewswitcher.tryshowasstandaloneasync). Le paramètre *viewId* de cette méthode est un entier qui identifie de manière unique chacune des vues dans votre application. Vous récupérez l’élément [**Id**](/uwp/api/windows.ui.viewmanagement.applicationview.id) de la vue à l’aide de la propriété **ApplicationView.Id** ou de la méthode [**ApplicationView.GetApplicationViewIdForWindow**](/uwp/api/windows.ui.viewmanagement.applicationview.getapplicationviewidforwindow).

    ```csharp
    bool viewShown = await ApplicationViewSwitcher.TryShowAsStandaloneAsync(newViewId);
    ```

## <a name="the-main-view"></a>Vue principale


La première vue créée lors du démarrage de votre application s’appelle la *vue principale*. Cette vue est stockée dans la propriété [**CoreApplication.MainView**](/uwp/api/windows.applicationmodel.core.coreapplication.mainview) et sa propriété [**IsMain**](/uwp/api/windows.applicationmodel.core.coreapplicationview.ismain) a la valeur true. Vous ne créez pas cette vue. Elle est créée par l’application. Le thread de la vue principale agit en tant que gestionnaire de l’application, et tous les évènements d’activation de l’application sont fournis sur ce thread.

Si des vues secondaires sont ouvertes, la fenêtre de la vue principale peut être masquée (par exemple, en cliquant sur le bouton de fermeture (x) dans la barre de titre de la fenêtre), mais son thread reste actif. Appeler [**Close**](/uwp/api/windows.ui.xaml.window.close) sur la [**Window**](/uwp/api/Windows.UI.Xaml.Window) de la vue principale entraîne une **InvalidOperationException**. (Utilisez [**Application.Exit**](/uwp/api/windows.ui.xaml.application.exit) pour fermer votre application.) Si le thread de la vue principale est terminé, l’application se ferme.

## <a name="secondary-views"></a>Vues secondaires


Les autres vues, notamment les vues que vous avez créées en appelant [**CreateNewView**](/uwp/api/windows.applicationmodel.core.coreapplication.createnewview) dans le code de votre application, sont des vues secondaires. La vue principale et les vues secondaires sont stockées dans la collection [**CoreApplication.Views**](/uwp/api/windows.applicationmodel.core.coreapplication.views). En règle générale, vous créez des vues secondaires en réponse à une action de l’utilisateur. Dans certains cas, le système crée des vues secondaires pour votre application.

> [!NOTE]
> Vous pouvez utiliser la fonctionnalité d’*accès affecté* de Windows pour exécuter une application en [mode plein écran](/windows/manage/set-up-a-device-for-anyone-to-use). Dans ce cas, le système crée une vue secondaire pour présenter l’interface utilisateur de votre application au-dessus de l’écran de verrouillage. Les vues secondaires créées par l’application ne sont pas autorisées. Ainsi, si vous essayez d’afficher votre propre vue secondaire en mode plein écran, une exception est levée.

## <a name="switch-from-one-view-to-another"></a>Basculer d’une vue à une autre

Pensez à un moyen de permettre à l’utilisateur de revenir à la fenêtre parente à partir d’une fenêtre secondaire. Pour ce faire, utilisez la méthode [**ApplicationViewSwitcher.SwitchAsync**](/uwp/api/windows.ui.viewmanagement.applicationviewswitcher.switchasync). Vous appelez cette méthode à partir du thread de la fenêtre que vous quittez et vous transmettez l’ID de vue de la fenêtre vers laquelle vous basculez.

```csharp
await ApplicationViewSwitcher.SwitchAsync(viewIdToShow);
```

Lorsque vous utilisez [**SwitchAsync**](/uwp/api/windows.ui.viewmanagement.applicationviewswitcher.switchasync), vous pouvez choisir si vous voulez fermer la fenêtre initiale et la supprimer de la barre des tâches en spécifiant la valeur de [**ApplicationViewSwitchingOptions**](/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitchingOptions).

## <a name="related-topics"></a>Rubriques connexes

- [Afficher plusieurs vues](show-multiple-views.md)
- [Afficher plusieurs vues avec AppWindow](app-window.md)
- [ApplicationViewSwitcher](/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitcher)
- [CreateNewView](/uwp/api/windows.applicationmodel.core.coreapplication.createnewview)