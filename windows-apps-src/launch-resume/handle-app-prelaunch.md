---
title: Gérer le prélancement d’une application
description: Découvrez comment gérer le prélancement d’une application en remplaçant la méthode OnLaunched et en appelant CoreApplication.EnablePrelaunch(true).
ms.assetid: A4838AC2-22D7-46BA-9EB2-F3C248E22F52
ms.date: 07/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 219ca73115d5605f1e1483f2af224a13c28791ff
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164863"
---
# <a name="handle-app-prelaunch"></a>Gérer le prélancement d’une application

Découvrez comment gérer le prélancement d’application en substituant la méthode [**OnLaunched**](/uwp/api/windows.ui.xaml.application.onlaunched) .

## <a name="introduction"></a>Introduction

Lorsque les ressources système disponibles le permettent, les performances de démarrage des applications UWP sur les appareils de la famille des appareils de bureau sont améliorées en lançant de manière proactive les applications les plus fréquemment utilisées de l’utilisateur en arrière-plan. Une application prélancée est placée à l’état suspendu dans les secondes suivant son lancement. Ainsi, lorsque l’utilisateur appelle l’application, celle-ci reprend l’exécution à partir de l’état suspendu, ce qui est plus rapide que le lancement de l’application à froid. L’utilisateur peut donc lancer son application très rapidement.

Avant Windows 10, les applications ne bénéficiaient pas automatiquement du prélancement. Dans Windows 10, version 1511, toutes les applications de plateforme Windows universelle (UWP) étaient candidates au prélancement. Dans Windows 10, version 1607, vous devez autoriser le comportement de prélancement en appelant [CoreApplication.EnablePrelaunch(true)](/uwp/api/windows.applicationmodel.core.coreapplication.enableprelaunch). Placez cet appel dans `OnLaunched()`, à proximité de l’emplacement où la vérification `if (e.PrelaunchActivated == false)` est effectuée.

Le prélancement d’une application dépend des ressources système. Si le système manque de ressources, les applications ne sont pas prélancées.

Le comportement au démarrage de certains types d’applications doit éventuellement être modifié pour prendre en charge le prélancement. Par exemple, une application qui diffuse de la musique au démarrage, un jeu qui part du principe que l’utilisateur est présent et qui affiche des éléments visuels sophistiqués au démarrage, une application de messagerie qui modifie le statut de visibilité en ligne de l’utilisateur au moment du démarrage, peuvent savoir si l’application a été prélancée et modifier ainsi leur comportement au démarrage comme décrit dans les sections ci-dessous.

Les modèles par défaut pour les projets XAML (C#, VB, C++) et WinJS prennent en charge le prélancement dans Visual Studio 2015 Update 3.

## <a name="prelaunch-and-the-app-lifecycle"></a>Prélancement et cycle de vie de l’application

Une fois qu’une application est prélancée, elle passe à l’état suspendu. (Voir [Gérer la suspension d’une application](suspend-an-app.md).)

## <a name="detect-and-handle-prelaunch"></a>Détecter et gérer le prélancement

Les applications reçoivent l’indicateur [**LaunchActivatedEventArgs. PrelaunchActivated**](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.prelaunchactivated) lors de l’activation. Utilisez cet indicateur pour exécuter du code qui ne doit s’exécuter que lorsque l’utilisateur lance explicitement l’application, comme illustré dans la modification suivante apportée à [**application. OnLaunched**](/uwp/api/windows.ui.xaml.application.onlaunched).

```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    // CoreApplication.EnablePrelaunch was introduced in Windows 10 version 1607
    bool canEnablePrelaunch = Windows.Foundation.Metadata.ApiInformation.IsMethodPresent("Windows.ApplicationModel.Core.CoreApplication", "EnablePrelaunch");

    // NOTE: Only enable this code if you are targeting a version of Windows 10 prior to version 1607
    // and you want to opt-out of prelaunch.
    // In Windows 10 version 1511, all UWP apps were candidates for prelaunch.
    // Starting in Windows 10 version 1607, the app must opt-in to be prelaunched.
    //if ( !canEnablePrelaunch && e.PrelaunchActivated == true)
    //{
    //    return;
    //}

    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        rootFrame = new Frame();

        rootFrame.NavigationFailed += OnNavigationFailed;

        if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
        {
            //TODO: Load state from previously suspended application
        }

        // Place the frame in the current Window
        Window.Current.Content = rootFrame;
    }

    if (e.PrelaunchActivated == false)
    {
        // On Windows 10 version 1607 or later, this code signals that this app wants to participate in prelaunch
        if (canEnablePrelaunch)
        {
            TryEnablePrelaunch();
        }

        // TODO: This is not a prelaunch activation. Perform operations which
        // assume that the user explicitly launched the app such as updating
        // the online presence of the user on a social network, updating a
        // what's new feed, etc.

        if (rootFrame.Content == null)
        {
            // When the navigation stack isn't restored navigate to the first page,
            // configuring the new page by passing required information as a navigation
            // parameter
            rootFrame.Navigate(typeof(MainPage), e.Arguments);
        }
        // Ensure the current window is active
        Window.Current.Activate();
    }
}

/// <summary>
/// Encapsulates the call to CoreApplication.EnablePrelaunch() so that the JIT
/// won't encounter that call (and prevent the app from running when it doesn't
/// find it), unless this method gets called. This method should only
/// be called when the caller determines that we are running on a system that
/// supports CoreApplication.EnablePrelaunch().
/// </summary>
private void TryEnablePrelaunch()
{
    Windows.ApplicationModel.Core.CoreApplication.EnablePrelaunch(true);
}
```

Notez la `TryEnablePrelaunch()` fonction, ci-dessus. La raison pour laquelle l’appel à `CoreApplication.EnablePrelaunch()` est pris en compte dans cette fonction est que lorsqu’une méthode est appelée, le JIT (compilation juste-à-temps) tente de compiler la totalité de la méthode. Si votre application s’exécute sur une version de Windows 10 qui ne prend pas en charge `CoreApplication.EnablePrelaunch()` , le JIT échoue. En factorisant l’appel dans une méthode qui est appelée uniquement lorsque l’application détermine que la plateforme prend en charge `CoreApplication.EnablePrelaunch()` , nous évitons ce problème.

Dans l’exemple ci-dessus, il existe également un code qui vous permet de supprimer les marques de commentaire si votre application doit refuser le prélancement lors de l’exécution sur Windows 10, version 1511. Dans la version 1511, toutes les applications UWP ont été automatiquement choisies dans le prélancement, ce qui peut ne pas convenir à votre application.

## <a name="use-the-visibilitychanged-event"></a>Utiliser l’événement VisibilityChanged

Les applications activées par prélancement ne sont pas visibles pour l’utilisateur. Elles le deviennent lorsque l’utilisateur bascule vers celles-ci. Vous souhaitez peut-être retarder certaines opérations jusqu’à ce que la fenêtre principale de votre application soit visible. Par exemple, si votre application affiche une liste des nouveaux éléments d’un flux, vous pouvez mettre à jour cette liste pendant l’événement [**VisibilityChanged**](/uwp/api/windows.ui.xaml.window.visibilitychanged) plutôt que d’utiliser une liste qui a été générée lors du prélancement de l’application et qui peut être obsolète au moment où l’utilisateur active l’application. Le code suivant gère l’événement **VisibilityChanged** pour **MainPage** :

```csharp
public sealed partial class MainPage : Page
{
    public MainPage()
    {
        this.InitializeComponent();

        Window.Current.VisibilityChanged += WindowVisibilityChangedEventHandler;
    }

    void WindowVisibilityChangedEventHandler(System.Object sender, Windows.UI.Core.VisibilityChangedEventArgs e)
    {
        // Perform operations that should take place when the application becomes visible rather than
        // when it is prelaunched, such as building a what's new feed
    }
}
```

## <a name="directx-games-guidance"></a>Recommandations en matière de jeux DirectX

Il n’est généralement pas conseillé d’activer le prélancement pour les jeux DirectX car de nombreux jeux DirectX s’initialisent avant que le prélancement puisse être détecté. À partir de Windows version 1607, édition anniversaire, le prélancement de votre jeu sera désactivé par défaut.  Si vous souhaitez activer le prélancement pour votre jeu, appelez l’élément [CoreApplication.EnablePrelaunch(true)](/uwp/api/windows.applicationmodel.core.coreapplication.enableprelaunch).

Si votre jeu utilise une version antérieure de Windows 10, vous pouvez gérer la condition de prélancement pour quitter l’application :

```cppwinrt
void ViewProvider::OnActivated(CoreApplicationView const& /* appView */, Windows::ApplicationModel::Activation::IActivatedEventArgs const& args)
{
    if (args.Kind() == Windows::ApplicationModel::Activation::ActivationKind::Launch)
    {
        auto launchArgs{ args.as<Windows::ApplicationModel::Activation::LaunchActivatedEventArgs>()};
        if (launchArgs.PrelaunchActivated())
        {
            // Opt-out of Prelaunch.
            CoreApplication::Exit();
        }
    }
}

void ViewProvider::Initialize(CoreApplicationView const & appView)
{
    appView.Activated({ this, &App::OnActivated });
}
```

```cpp
void ViewProvider::OnActivated(CoreApplicationView^ appView,IActivatedEventArgs^ args)
{
    if (args->Kind == ActivationKind::Launch)
    {
        auto launchArgs = static_cast<LaunchActivatedEventArgs^>(args);
        if (launchArgs->PrelaunchActivated)
        {
            // Opt-out of Prelaunch
            CoreApplication::Exit();
            return;
        }
    }
}
```

## <a name="winjs-app-guidance"></a>Recommandations en matière d’applications WinJS

Si votre application WinJS utilise une version antérieure de Windows 10, vous pouvez gérer la condition de prélancement dans votre gestionnaire [onactivated](/previous-versions/windows/apps/br212679(v=win.10)) :

```javascript
    app.onactivated = function (args) {
        if (!args.detail.prelaunchActivated) {
            // TODO: This is not a prelaunch activation. Perform operations which
            // assume that the user explicitly launched the app such as updating
            // the online presence of the user on a social network, updating a
            // what's new feed, etc.
        }
    }
```

## <a name="general-guidance"></a>Conseils d’ordre général

-   Les applications ne doivent pas effectuer d’opérations de longue durée pendant le prélancement, puisque l’application s’arrêtera si elle ne peut pas être suspendue rapidement.
-   Les applications ne doivent pas lancer la lecture audio à partir d' [**application. OnLaunched**](/uwp/api/windows.ui.xaml.application.onlaunched) quand l’application est prélancée, car l’application n’est pas visible et il n’est pas évident que la lecture audio est possible.
-   Les applications ne doivent pas effectuer d’opérations lors du lancement, qui suppose que l’application est visible par l’utilisateur ou qu’elle a été lancée explicitement par l’utilisateur. Dans la mesure où une application peut maintenant être lancée en arrière-plan sans action explicite de l’utilisateur, les développeurs doivent prendre en compte la confidentialité, l’incidence sur les performances et l’expérience utilisateur.
    -   Le moment où une application sociale doit définir l’état de l’utilisateur sur « en ligne » est un exemple de considération relative à la confidentialité. Elle doit attendre que l’utilisateur bascule vers l’application au lieu de modifier l’état de celui-ci lors du prélancement de l’application.
    -   Exemple de considération relative à l’expérience utilisateur : vous possédez une application, telle qu’un jeu, qui affiche une séquence d’introduction lors de son lancement, et aimeriez avoir la possibilité de retarder la séquence d’introduction jusqu’à ce que l’utilisateur bascule vers l’application.
    -   En ce qui concerne l’incidence sur les performances, vous devrez peut-être attendre que l’utilisateur bascule vers l’application pour récupérer les informations météorologiques actuelles au lieu de les charger lors du prélancement de l’application, puis les charger une nouvelle fois lorsque l’application devient visible pour vous assurer que les informations sont à jour.
-   Si votre application efface sa vignette dynamique lors de son lancement, reportez cette opération pour qu’elle ait lieu après l’événement VisibilityChanged.
-   La télémétrie de votre application doit faire la distinction entre les activations de vignette standard et les activations de prélancement, afin que vous puissiez identifier plus facilement le scénario correspondant en cas de problème.
-   Si vous possédez Microsoft Visual Studio 2015 Update 1 et Windows 10 version 1511, vous pouvez simuler le prélancement de votre application dans Visual Studio 2015 en choisissant **Déboguer** &gt; **Autres cibles de débogage** &gt; **Déboguer le prélancement de l’application Windows universelle**.

## <a name="related-topics"></a>Rubriques connexes

* [Cycle de vie de l’application](app-lifecycle.md)
* [CoreApplication.EnablePrelaunch](/uwp/api/windows.applicationmodel.core.coreapplication.enableprelaunch)