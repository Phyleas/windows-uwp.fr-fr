---
description: Découvrez comment implémenter la navigation vers l’arrière afin de parcourir l’historique de navigation de l’utilisateur dans une application Windows.
title: Navigation dans l’historique et navigation vers l’arrière
template: detail.hbs
op-migration-status: ready
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: 738efe43afaf5c278425d5636bddc72cecadf47a
ms.sourcegitcommit: d51c3dd64d58c7fa9513ba20e736905f12df2a9a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98988761"
---
# <a name="navigation-history-and-backwards-navigation-for-windows-apps"></a>Historique de navigation et navigation vers l’arrière pour les applications Windows

> **API importantes** : [événement BackRequested](/uwp/api/Windows.UI.Core.SystemNavigationManager.BackRequested), [classe SystemNavigationManager](/uwp/api/Windows.UI.Core.SystemNavigationManager), [OnNavigatedTo](/uwp/api/windows.ui.xaml.controls.page.onnavigatedto)

L’application Windows fournit un système de navigation vers l’arrière cohérent afin de parcourir l’historique de navigation de l’utilisateur dans une application et, en fonction de l’appareil, d’une application à l’autre.

Pour implémenter la navigation vers l’arrière dans votre application, placez un bouton Précédent dans l’angle supérieur gauche de l’interface utilisateur de votre application. Lorsqu’il appuie sur le bouton Précédent, l’utilisateur s’attend à accéder à l’emplacement précédent dans l’historique de navigation de l’application. Sachez qu’il vous incombe de décider des actions de navigation à ajouter à l’historique de navigation et de la réponse à un appui sur le bouton Précédent.

Pour la plupart des applications qui ont plusieurs pages, nous vous recommandons d’utiliser le contrôle [NavigationView](../controls-and-patterns/navigationview.md) pour fournir l’infrastructure de navigation de votre application. Il s’adapte à un large éventail de tailles d’écran et prend en charge les styles de navigation _supérieure_ et _gauche_. Si votre application utilise le contrôle `NavigationView`, vous pouvez ensuite utiliser le [bouton Précédent intégré de NavigationView](../controls-and-patterns/navigationview.md#backwards-navigation).

> [!NOTE]
> Les instructions et les exemples de cet article doivent être utilisés quand vous implémentez la navigation sans utiliser le contrôle `NavigationView`. Si vous utilisez `NavigationView`, ces informations vous donnent des connaissances générales, mais il est préférable d’utiliser les instructions et les exemples spécifiques de l’article [NavigationView](../controls-and-patterns/navigationview.md)

## <a name="back-button"></a>Bouton Précédent

Pour créer un bouton Précédent, utilisez le contrôle [Button](../controls-and-patterns/buttons.md) et le style `NavigationBackButtonNormalStyle`, puis placez le bouton dans l’angle supérieur gauche de l’interface utilisateur de votre application (pour plus d’informations, consultez les exemples de code XAML ci-dessous).

![Bouton Précédent dans l’angle supérieur gauche de l’interface utilisateur de l’application](images/back-nav/BackEnabled.png)

```xaml
<Page>
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>

        <Button x:Name="BackButton"
                Style="{StaticResource NavigationBackButtonNormalStyle}"
                IsEnabled="{x:Bind Frame.CanGoBack, Mode=OneWay}" 
                ToolTipService.ToolTip="Back"/>

    </Grid>
</Page>
```

Si votre application possède un objet [CommandBar](../controls-and-patterns/app-bars.md) supérieur, le contrôle Button de 44 pixels de haut ne s’aligne pas correctement sur les contrôles AppBarButtons de 48 pixels. Pour éviter ce problème, alignez le haut du contrôle Button dans la limite de 48 pixels.

![Bouton Précédent dans la barre de commandes supérieure](images/back-nav/CommandBar.png)

```xaml
<Page>
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        
        <CommandBar>
            <CommandBar.Content>
                <Button x:Name="BackButton"
                        Style="{StaticResource NavigationBackButtonNormalStyle}"
                        IsEnabled="{x:Bind Frame.CanGoBack, Mode=OneWay}" 
                        ToolTipService.ToolTip="Back" 
                        VerticalAlignment="Top"/>
            </CommandBar.Content>
        
            <AppBarButton Icon="Delete" Label="Delete"/>
            <AppBarButton Icon="Save" Label="Save"/>
        </CommandBar>
    </Grid>
</Page>
```

Pour réduire les déplacements entre les éléments d’interface utilisateur de votre application, affichez un bouton Précédent désactivé quand il n’y a plus rien dans la pile de retour arrière (`IsEnabled="{x:Bind Frame.CanGoBack, Mode=OneWay}"`). Cependant, si vous pensez que votre application n’aura jamais de pile de retour arrière, vous n’avez pas besoin d’afficher le bouton Précédent.

![États du bouton Précédent](images/back-nav/BackDisabled.png)

## <a name="optimize-for-different-devices-and-inputs"></a>Optimiser pour différents appareils et différentes entrées

Ce guide de conception de la navigation vers l’arrière s’applique à tous les appareils, mais vos utilisateurs vont en tirer davantage parti si vous faites une optimisation pour les différents facteurs de forme et les différentes méthodes d’entrée.

Pour optimiser votre interface utilisateur :

- **Bureau/hub** : représentez le bouton Précédent dans l’application dans l’angle supérieur gauche de l’interface utilisateur de votre application.
- **[Mode Tablette](https://support.microsoft.com/windows/use-your-pc-like-a-tablet-4fbfcca5-f058-814a-4f80-a12e703d7c34)**  : Un bouton Précédent matériel ou logiciel peut être présent sur un appareil sur les tablettes mais, pour des raisons de clarté, nous vous recommandons de représenter un bouton Précédent dans l’application.
- **Xbox/TV** : Ne représentez pas de bouton Précédent pour éviter d’alourdir inutilement l’interface utilisateur. Au lieu de cela, utilisez le bouton B du boîtier de commande pour naviguer vers l’arrière.

Si votre application va s’exécuter sur Xbox, [créez un déclencheur visuel personnalisé pour Xbox](../devices/designing-for-tv.md#custom-visual-state-trigger-for-xbox) pour activer/désactiver la visibilité du bouton. Si vous utilisez un contrôle [NavigationView](../controls-and-patterns/navigationview.md), il va activer/désactiver automatiquement la visibilité du bouton Précédent quand votre application s’exécute sur Xbox.

Nous vous recommandons de gérer les événements suivants (en plus du clic sur le bouton Précédent) pour prendre en charge les entrées les plus courantes pour la navigation vers l’arrière.

| Événement | Entrée |
| --- | --- |
| [CoreDispatcher.AcceleratorKeyActivated](/uwp/api/windows.ui.core.coredispatcher.acceleratorkeyactivated) | Alt+Flèche gauche,<br/>VirtualKey.GoBack |
| [SystemNavigationManager.BackRequested](/api/windows.ui.core.systemnavigationmanager.backrequested) | Windows + Retour arrière,<br/>Bouton B de la manette de jeu,<br/>Bouton Précédent du mode tablette,<br/>Bouton Précédent matériel |
| [CoreWindow.PointerPressed](/uwp/api/windows.ui.core.corewindow.pointerpressed) | VirtualKey.XButton1<br/>(Comme le bouton Précédent qui se trouve sur certaines souris.) |

## <a name="code-examples"></a>Exemples de code

Cette section montre comment gérer la navigation vers l’arrière en utilisant différentes entrées.

### <a name="back-button-and-back-navigation"></a>Bouton Précédent et navigation vers l’arrière

Au minimum, vous devez gérer l’événement `Click` du bouton Précédent et fournir le code pour effectuer la navigation vers l’arrière. Vous devez aussi désactiver le bouton Précédent quand la pile de retour arrière est vide.

Cet exemple de code montre comment implémenter le comportement de navigation vers l’arrière avec un bouton Précédent. Le code répond à l’événement [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.Click) du bouton pour naviguer. Le bouton Précédent est activé ou désactivé dans la méthode [OnNavigatedTo](/uwp/api/windows.ui.xaml.controls.page.onnavigatedto), qui est appelée lors de la navigation vers une nouvelle page.

Le code est montré pour `MainPage`, mais vous devez ajouter ce code à chaque page qui prend en charge la navigation vers l’arrière. Pour éviter de le dupliquer, vous pouvez placer le code lié à la navigation dans la classe `App` de la page code-behind de `App.xaml.*`.

```xaml
<!-- MainPage.xaml -->
<Page x:Class="AppName.MainPage">
...
        <Button x:Name="BackButton" Click="BackButton_Click"
                Style="{StaticResource NavigationBackButtonNormalStyle}"
                IsEnabled="{x:Bind Frame.CanGoBack, Mode=OneWay}" 
                ToolTipService.ToolTip="Back"/>
...
<Page/>
```

Code-behind :

```csharp
// MainPage.xaml.cs
private void BackButton_Click(object sender, RoutedEventArgs e)
{
    App.TryGoBack();
}

// App.xaml.cs
//
// Add this method to the App class.
public static bool TryGoBack()
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoBack)
    {
        rootFrame.GoBack();
        return true;
    }
    return false;
}
```

```cppwinrt
// MainPage.h
namespace winrt::AppName::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();
 
        void MainPage::BackButton_Click(IInspectable const&, RoutedEventArgs const&)
        {
            App::TryGoBack();
        }
    };
}

// App.h
#include "winrt/Windows.UI.Core.h"
#include "winrt/Windows.System.h"
#include "winrt/Windows.UI.Input.h"
#include "winrt/Windows.UI.Xaml.Input.h"
 
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::UI::Core;
using namespace Windows::UI::Input;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;

struct App : AppT<App>
{
    App();

    // ...

    // Perform back navigation if possible.
    static bool TryGoBack()
    {
        Frame rootFrame{ nullptr };
        auto content = Window::Current().Content();
        if (content)
        {
            rootFrame = content.try_as<Frame>();
            if (rootFrame.CanGoBack())
            {
                rootFrame.GoBack();
                return true;
            }
        }
        return false;
    }
};
```

### <a name="support-access-keys"></a>Prendre en charge les touches d’accès rapide

La prise en charge intégrale du clavier permet de garantir que vos applications fonctionnent parfaitement pour les utilisateurs aux compétences, capacités et attentes différentes. Nous vous recommandons de prendre en charge les touches accélérateur pour la navigation vers l’avant et vers l’arrière, car les utilisateurs qui s’en servent attendent les deux. Pour plus d’informations, consultez [Interactions avec le clavier](..\input\keyboard-interactions.md) et [Raccourcis claviers](..\input\keyboard-accelerators.md).

Les touches accélérateur pour la navigation vers l’avant et vers l’arrière sont Alt+Flèche droite (vers l’avant) et Alt+ Flèche gauche (vers l’arrière). Pour prendre en charge ces touches pour la navigation, gérez l’événement [CoreDispatcher.AcceleratorKeyActivated](/uwp/api/windows.ui.core.coredispatcher.acceleratorkeyactivated). Vous gérez un événement qui est directement sur la fenêtre (et non pas un élément dans la page) pour que l’application réponde aux touches accélérateur, quel que soit l’élément qui a le focus.

Ajoutez le code à la classe `App` pour prendre en charge les touches accélérateur et la navigation vers l’avant, comme illustré ici. (Ceci suppose que le code précédent pour prendre en charge le bouton Précédent a déjà été ajouté.) Vous pouvez voir l’ensemble du code de `App` à la fin de la section Exemples de code.

```csharp
// App.xaml.cs
// Add event handler in OnLaunced.
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // ...
        // rootFrame.NavigationFailed += OnNavigationFailed;

        // Add support for accelerator keys. 
        // Listen to the window directly so the app responds
        // to accelerator keys regardless of which element has focus.
        Window.Current.CoreWindow.Dispatcher.AcceleratorKeyActivated +=
            CoreDispatcher_AcceleratorKeyActivated;

        // ...

    }
}

// ...

// Add this code after the TryGoBack method added previously.
// Perform forward navigation if possible.
private bool TryGoForward()
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoForward)
    {
        rootFrame.GoForward();
        return true;
    }
    return false;
}

// Invoked on every keystroke, including system keys such as Alt key combinations.
// Used to detect keyboard navigation between pages even when the page itself
// doesn't have focus.
private void CoreDispatcher_AcceleratorKeyActivated(CoreDispatcher sender, AcceleratorKeyEventArgs e)
{
    // When Alt+Left are pressed navigate back.
    // When Alt+Right are pressed navigate forward.
    if (e.EventType == CoreAcceleratorKeyEventType.SystemKeyDown
        && (e.VirtualKey == VirtualKey.Left || e.VirtualKey == VirtualKey.Right)
        && e.KeyStatus.IsMenuKeyDown == true
        && !e.Handled)
    {
        if (e.VirtualKey == VirtualKey.Left)
        {
            e.Handled = TryGoBack();
        }
        else if (e.VirtualKey == VirtualKey.Right)
        {
            e.Handled = TryGoForward();
        }
    }
}
```

```cppwinrt
// App.cpp
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // ...
        // rootFrame.NavigationFailed({ this, &App::OnNavigationFailed });

        // Add support for accelerator keys. 
        // Listen to the window directly so the app responds
        // to accelerator keys regardless of which element has focus.
        Window::Current().CoreWindow().Dispatcher().
            AcceleratorKeyActivated({ this, &App::CoreDispatcher_AcceleratorKeyActivated });

        // ...
    }
}

// App.h
struct App : AppT<App>
{
    App();

    // ...
    // Add this code after the TryGoBack method added previously.

private:
    // Perform forward navigation if possible.
    bool TryGoForward()
    {
        Frame rootFrame{ nullptr };
        auto content = Window::Current().Content();
        if (content)
        {
            rootFrame = content.try_as<Frame>();
            if (rootFrame.CanGoForward())
            {
                rootFrame.GoForward();
                return true;
            }
        }
        return false;
    }
 
 
    // Invoked on every keystroke, including system keys such as Alt key combinations.
    // Used to detect keyboard navigation between pages even when the page itself
    // doesn't have focus.
    void CoreDispatcher_AcceleratorKeyActivated(CoreDispatcher const& /* sender */, AcceleratorKeyEventArgs const& e)
    {
        // When Alt+Left are pressed navigate back.
        // When Alt+Right are pressed navigate forward.
        if (e.EventType() == CoreAcceleratorKeyEventType::SystemKeyDown
            && (e.VirtualKey() == Windows::System::VirtualKey::Left || e.VirtualKey() == Windows::System::VirtualKey::Right)
            && e.KeyStatus().IsMenuKeyDown
            && !e.Handled())
        {
            if (e.VirtualKey() == Windows::System::VirtualKey::Left)
            {
                e.Handled(TryGoBack());
            }
            else if (e.VirtualKey() == Windows::System::VirtualKey::Right)
            {
                e.Handled(TryGoForward());
            }
        }
    }
};
```

### <a name="handle-system-back-requests"></a>Gérer les demandes de retour arrière du système

Les appareils Windows offrent différents moyens au système pour passer une demande de navigation vers l’arrière à votre application. Parmi les moyens courants figurent le bouton B sur une manette de jeu, la touche Windows + la touche Retour Arrière ou le bouton Précédent du système en mode tablette : les options réellement disponibles varient selon l’appareil.

Vous pouvez prendre en charge les demandes de retour arrière fournies par le système à partir des touches matérielles et logicielles du système en inscrivant un écouteur pour l’événement [SystemNavigationManager.BackRequested](/api/windows.ui.core.systemnavigationmanager.backrequested).

Voici le code ajouté à la classe `App` pour prendre en charge les demandes de retour arrière fournies par le système. (Ceci suppose que le code précédent pour prendre en charge le bouton Précédent a déjà été ajouté.) Vous pouvez voir l’ensemble du code de `App` à la fin de la section Exemples de code.

```csharp
// App.xaml.cs
// Add event handler in OnLaunced.
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // ...
        // Add support for accelerator keys. 
        // ... (Previously added code.)

        // Add support for system back requests. 
        SystemNavigationManager.GetForCurrentView().BackRequested 
            += System_BackRequested;

        // ...

    }
}

// ...
// Handle system back requests.
private void System_BackRequested(object sender, BackRequestedEventArgs e)
{
    if (!e.Handled)
    {
        e.Handled = TryGoBack();
    }
}
```

```cppwinrt
// App.cpp
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // ...
        // Add support for accelerator keys. 
        // ... (Previously added code.)

        // Add support for system back requests. 
        SystemNavigationManager::GetForCurrentView().
            BackRequested({ this, &App::System_BackRequested });

        // ...
    }
}

// App.h
struct App : AppT<App>
{
    App();

    // ...

private:
    // ...

    // Handle system back requests.
    void System_BackRequested(IInspectable const& /* sender */, BackRequestedEventArgs const& e)
    {
        if (!e.Handled())
        {
            e.Handled(TryGoBack());
        }
    }
};
```

#### <a name="system-back-behavior-for-backward-compatibility"></a>Comportement du retour arrière du système pour la compatibilité descendante

Auparavant, les applications UWP utilisaient [SystemNavigationManager.AppViewBackButtonVisibility](/uwp/api/windows.ui.core.systemnavigationmanager.appviewbackbuttonvisibility) pour afficher ou masquer un bouton Précédent du système pour la navigation vers l’arrière. (Ce bouton déclenche un événement [SystemNavigationManager.BackRequested](/api/windows.ui.core.systemnavigationmanager.backrequested).) Cette API continuera d’être prise en charge pour assurer la compatibilité descendante, mais nous vous recommandons de ne plus utiliser le bouton Précédent exposé par `AppViewBackButtonVisibility`. Au lieu de cela, vous devez fournir votre propre bouton Précédent dans l’application, comme décrit dans cet article.

Si vous continuez d’utiliser [AppViewBackButtonVisibility](/uwp/api/windows.ui.core.appviewbackbuttonvisibility), l’interface utilisateur du système affiche le bouton Précédent du système à l’intérieur de la barre de titre. (Les interactions utilisateur et l’apparence du bouton Précédent sont identiques à celles des builds précédentes.)

![Bouton Précédent de la barre de titre](images/nav-back-pc.png)

### <a name="handle-mouse-navigation-buttons"></a>Gérer les boutons de navigation de la souris

Certaines souris fournissent des boutons de navigation matériels pour la navigation vers l’avant et vers l’arrière. Vous pouvez prendre en charge ces boutons de la souris en gérant l’événement [CoreWindow.PointerPressed](/uwp/api/windows.ui.core.corewindow.pointerpressed), et en recherchant [IsXButton1Pressed](/uwp/api/windows.ui.input.pointerpointproperties.isxbutton1pressed) (vers l’arrière) ou [IsXButton2Pressed](/uwp/api/windows.ui.input.pointerpointproperties.isxbutton2pressed) (vers l’avant).

Voici le code ajouté à la classe `App` pour prendre en charge la navigation avec les boutons de la souris. (Ceci suppose que le code précédent pour prendre en charge le bouton Précédent a déjà été ajouté.) Vous pouvez voir l’ensemble du code de `App` à la fin de la section Exemples de code.

```csharp
// App.xaml.cs
// Add event handler in OnLaunced.
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // ...
        // Add support for system back requests. 
        // ... (Previously added code.)

        // Add support for mouse navigation buttons. 
        Window.Current.CoreWindow.PointerPressed += CoreWindow_PointerPressed;

        // ...

    }
}

// ...

// Handle mouse back button.
private void CoreWindow_PointerPressed(CoreWindow sender, PointerEventArgs e)
{
    // For this event, e.Handled arrives as 'true', so invert the value.
    if (e.CurrentPoint.Properties.IsXButton1Pressed
        && e.Handled)
    {
        e.Handled = !TryGoBack();
    }
    else if (e.CurrentPoint.Properties.IsXButton2Pressed
            && e.Handled)
    {
        e.Handled = !TryGoForward();
    }
}
```

```cppwinrt
// App.cpp
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // ...
        // Add support for system back requests. 
        // ... (Previously added code.)

        // Add support for mouse navigation buttons. 
        Window::Current().CoreWindow().
            PointerPressed({ this, &App::CoreWindow_PointerPressed });

        // ...
    }
}

// App.h
struct App : AppT<App>
{
    App();

    // ...

private:
    // ...

    // Handle mouse forward and back buttons.
    void CoreWindow_PointerPressed(CoreWindow const& /* sender */, PointerEventArgs const& e)
    {
        // For this event, e.Handled arrives as 'true', so invert the value. 
        if (e.CurrentPoint().Properties().IsXButton1Pressed()
            && e.Handled())
        {
            e.Handled(!TryGoBack());
        }
        else if (e.CurrentPoint().Properties().IsXButton2Pressed()
            && e.Handled())
        {
            e.Handled(!TryGoForward());
        }
    }
};
```

### <a name="all-code-added-to-app-class"></a>Tout le code ajouté à la classe App
 
```csharp
// App.xaml.cs
//
// (Add event handlers in OnLaunched override.)
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // ...
        // rootFrame.NavigationFailed += OnNavigationFailed;

        // Add support for accelerator keys. 
        // Listen to the window directly so the app responds
        // to accelerator keys regardless of which element has focus.
        Window.Current.CoreWindow.Dispatcher.AcceleratorKeyActivated +=
            CoreDispatcher_AcceleratorKeyActivated;

        // Add support for system back requests. 
        SystemNavigationManager.GetForCurrentView().BackRequested 
            += System_BackRequested;

        // Add support for mouse navigation buttons. 
        Window.Current.CoreWindow.PointerPressed += CoreWindow_PointerPressed;

        // ...

    }
}

// ...

// (Add these methods to the App class.)
public static bool TryGoBack()
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoBack)
    {
        rootFrame.GoBack();
        return true;
    }
    return false;
}

// Perform forward navigation if possible.
private bool TryGoForward()
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoForward)
    {
        rootFrame.GoForward();
        return true;
    }
    return false;
}

// Invoked on every keystroke, including system keys such as Alt key combinations.
// Used to detect keyboard navigation between pages even when the page itself
// doesn't have focus.
private void CoreDispatcher_AcceleratorKeyActivated(CoreDispatcher sender, AcceleratorKeyEventArgs e)
{
    // When Alt+Left are pressed navigate back.
    // When Alt+Right are pressed navigate forward.
    if (e.EventType == CoreAcceleratorKeyEventType.SystemKeyDown
        && (e.VirtualKey == VirtualKey.Left || e.VirtualKey == VirtualKey.Right)
        && e.KeyStatus.IsMenuKeyDown == true
        && !e.Handled)
    {
        if (e.VirtualKey == VirtualKey.Left)
        {
            e.Handled = TryGoBack();
        }
        else if (e.VirtualKey == VirtualKey.Right)
        {
            e.Handled = TryGoForward();
        }
    }
}

// Handle system back requests.
private void System_BackRequested(object sender, BackRequestedEventArgs e)
{
    if (!e.Handled)
    {
        e.Handled = TryGoBack();
    }
}

// Handle mouse back button.
private void CoreWindow_PointerPressed(CoreWindow sender, PointerEventArgs e)
{
    // For this event, e.Handled arrives as 'true', so invert the value.
    if (e.CurrentPoint.Properties.IsXButton1Pressed
        && e.Handled)
    {
        e.Handled = !TryGoBack();
    }
    else if (e.CurrentPoint.Properties.IsXButton2Pressed
            && e.Handled)
    {
        e.Handled = !TryGoForward();
    }
}


```

```cppwinrt
// App.cpp
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // ...
        // rootFrame.NavigationFailed({ this, &App::OnNavigationFailed });

        // Add support for accelerator keys. 
        // Listen to the window directly so the app responds
        // to accelerator keys regardless of which element has focus.
        Window::Current().CoreWindow().Dispatcher().
            AcceleratorKeyActivated({ this, &App::CoreDispatcher_AcceleratorKeyActivated });

        // Add support for system back requests. 
        SystemNavigationManager::GetForCurrentView().
            BackRequested({ this, &App::System_BackRequested });

        // Add support for mouse navigation buttons. 
        Window::Current().CoreWindow().
            PointerPressed({ this, &App::CoreWindow_PointerPressed });

        // ...
    }
}

// App.h
#include "winrt/Windows.UI.Core.h"
#include "winrt/Windows.System.h"
#include "winrt/Windows.UI.Input.h"
#include "winrt/Windows.UI.Xaml.Input.h"
 
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::UI::Core;
using namespace Windows::UI::Input;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;

struct App : AppT<App>
{
    App();

    // ...

    // Perform back navigation if possible.
    static bool TryGoBack()
    {
        Frame rootFrame{ nullptr };
        auto content = Window::Current().Content();
        if (content)
        {
            rootFrame = content.try_as<Frame>();
            if (rootFrame.CanGoBack())
            {
                rootFrame.GoBack();
                return true;
            }
        }
        return false;
    }
private:
    // Perform forward navigation if possible.
    bool TryGoForward()
    {
        Frame rootFrame{ nullptr };
        auto content = Window::Current().Content();
        if (content)
        {
            rootFrame = content.try_as<Frame>();
            if (rootFrame.CanGoForward())
            {
                rootFrame.GoForward();
                return true;
            }
        }
        return false;
    }
  
    // Invoked on every keystroke, including system keys such as Alt key combinations.
    // Used to detect keyboard navigation between pages even when the page itself
    // doesn't have focus.
    void CoreDispatcher_AcceleratorKeyActivated(CoreDispatcher const& /* sender */, AcceleratorKeyEventArgs const& e)
    {
        // When Alt+Left are pressed navigate back.
        // When Alt+Right are pressed navigate forward.
        if (e.EventType() == CoreAcceleratorKeyEventType::SystemKeyDown
            && (e.VirtualKey() == Windows::System::VirtualKey::Left || e.VirtualKey() == Windows::System::VirtualKey::Right)
            && e.KeyStatus().IsMenuKeyDown
            && !e.Handled())
        {
            if (e.VirtualKey() == Windows::System::VirtualKey::Left)
            {
                e.Handled(TryGoBack());
            }
            else if (e.VirtualKey() == Windows::System::VirtualKey::Right)
            {
                e.Handled(TryGoForward());
            }
        }
    }

    // Handle system back requests.
    void System_BackRequested(IInspectable const& /* sender */, BackRequestedEventArgs const& e)
    {
        if (!e.Handled())
        {
            e.Handled(TryGoBack());
        }
    }

    // Handle mouse forward and back buttons.
    void CoreWindow_PointerPressed(CoreWindow const& /* sender */, PointerEventArgs const& e)
    {
        // For this event, e.Handled arrives as 'true', so invert the value. 
        if (e.CurrentPoint().Properties().IsXButton1Pressed()
            && e.Handled())
        {
            e.Handled(!TryGoBack());
        }
        else if (e.CurrentPoint().Properties().IsXButton2Pressed()
            && e.Handled())
        {
            e.Handled(!TryGoForward());
        }
    }
};
```

## <a name="guidelines-for-custom-back-navigation-behavior"></a>Recommandations sur le comportement personnalisé de navigation vers l’arrière

Si vous choisissez de fournir votre propre navigation de pile Back, l’expérience doit être cohérente avec les autres applications. Nous vous recommandons de suivre les modèles d’actions de navigation suivants :

<div class="mx-responsive-img">
<table>
<thead>
<tr class="header">
<th align="left">Action de navigation</th>
<th align="left">Ajouter à l’historique de navigation ?</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="vertical-align:top;"><strong>Page à page, différents groupes d’homologues</strong></td>
<td style="vertical-align:top;"><strong>Oui</strong>
<p>Dans cette illustration, l’utilisateur navigue du niveau 1 de l’application vers le niveau 2, en traversant des groupes d’homologues. La navigation est donc ajoutée à l’historique de navigation.</p>
<p><img src="images/back-nav/nav-pagetopage-diffpeers-imageonly1.png" alt="Diagram of navigation across peer groups showing the user navigating from group one to group two and the back to group one." /></p>
<p>Dans l’illustration suivante, l’utilisateur navigue entre deux groupes d’homologues du même niveau. La navigation est donc ajoutée à l’historique de navigation.</p>
<p><img src="images/back-nav/nav-pagetopage-diffpeers-imageonly2.png" alt="Diagram of navigation across peer groups showing the user navigating from group one to group two then on to group three and back to group two." /></p></td>
</tr>
<tr class="even">
<td style="vertical-align:top;"><strong>Page à page, même groupe d’homologues, pas d’élément de navigation à l’écran</strong>
<p>L’utilisateur navigue d’une page à une autre dans le même groupe d’homologues. Aucun élément de navigation à l’écran (tel que <a href="/windows/uwp/design/controls-and-patterns/navigationview">NavigationView</a>) ne fournit une navigation directe vers les deux pages.</p></td>
<td style="vertical-align:top;"><strong>Oui</strong>
<p>Dans l’illustration suivante, l’utilisateur navigue entre deux pages dans le même groupe d’homologues et la navigation doit être ajoutée à l’historique de navigation.</p>
<p><img src="images/back-nav/nav-pagetopage-samepeer-noosnavelement.png" alt="Navigation within a peer group" /></p></td>
</tr>
<tr class="odd">
<td style="vertical-align:top;"><strong>Page à page, même groupe d’homologues, avec un élément de navigation à l’écran</strong>
<p>L’utilisateur navigue d’une page à une autre dans le même groupe d’homologues. Les deux pages sont affichées dans le même élément de navigation, tel que <a href="/windows/uwp/design/controls-and-patterns/navigationview">NavigationView</a>.</p></td>
<td style="vertical-align:top;"><strong>Cela dépend</strong>
<p>Oui, ajouter à l’historique de navigation, avec deux exceptions notables. Si vous pensez que des utilisateurs de votre application passeront fréquemment d’une page à l’autre dans le groupe d’homologues, ou si vous souhaitez conserver la hiérarchie de navigation, n’effectuez aucun ajout à l’historique de navigation. Dans ce cas, lorsque l’utilisateur appuie sur le bouton Précédent, il retourne à la dernière page avant d’avoir accédé au groupe d’homologues actuel. </p>
<p><img src="images/back-nav/nav-pagetopage-samepeer-yesosnavelement.png" alt="Navigation across peer groups when a navigation element is present" /></p></td>
</tr>
<tr class="even">
<td style="vertical-align:top;"><strong>Afficher une interface utilisateur temporaire</strong>
<p>L’application affiche une fenêtre indépendante ou une fenêtre enfant, comme une boîte de dialogue, un écran de démarrage ou un clavier visuel. Elle passe également en mode spécial, comme le mode de sélection multiple.</p></td>
<td style="vertical-align:top;"><strong>Non</strong>
<p>Lorsque l’utilisateur appuie sur le bouton Précédent, il ignore l’interface utilisateur temporaire (masque le clavier visuel, annule la boîte de dialogue, etc.) et retourne dans la page qui a généré l’interface utilisateur temporaire.</p>
<p><img src="images/back-nav/back-transui.png" alt="Showing a transient UI" /></p></td>
</tr>
<tr class="odd">
<td style="vertical-align:top;"><strong>Énumérer les éléments</strong>
<p>L’application affiche le contenu d’un élément visuel, tel que les détails de l’élément sélectionné dans la liste des éléments maîtres/détails.</p></td>
<td style="vertical-align:top;"><strong>Non</strong>
<p>L’énumération des éléments est semblable à la navigation au sein d’un groupe d’homologues. Lorsque l’utilisateur appuie sur le bouton Précédent, il navigue vers la page qui a précédé la page actuelle qui comporte l’énumération d’éléments.</p>
<p><img src="images/back-nav/nav-enumerate.png" alt="Iterm enumeration" /></p></td>
</tr>
</tbody>
</table>
</div>

### <a name="resuming"></a>Resuming

Quand l’utilisateur bascule vers une autre application et retourne dans votre application, nous recommandons un retour à la dernière page de l’historique de navigation.

## <a name="related-articles"></a>Articles connexes

- [Notions de base sur la navigation](navigation-basics.md)
