---
Description: Utilisez la classe AppWindow pour présenter différentes parties de votre application dans des fenêtres distinctes.
title: Utiliser la classe AppWindow pour afficher des fenêtres secondaires pour une application
ms.date: 07/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9b89d9100157cf40266bb983e258aa187f65dc93
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867465"
---
# <a name="show-multiple-views-with-appwindow"></a>Afficher plusieurs vues avec AppWindow

[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) et les API associées simplifient la création d’applications à plusieurs fenêtres. En effet, elles vous permettent d’afficher le contenu de votre application dans des fenêtres secondaires, tout en continuant à travailler sur le même thread d’interface utilisateur dans chaque fenêtre.

> [!NOTE]
> AppWindow est actuellement disponible en préversion. Vous pouvez donc soumettre des applications utilisant AppWindow sur le Microsoft Store, mais certains composants de plateforme et de framework peuvent ne pas fonctionner avec AppWindow (consultez [Limitations](/uwp/api/windows.ui.windowmanagement.appwindow#limitations)).

Ici, nous abordons certains scénarios impliquant l’affichage de plusieurs fenêtres avec un exemple d’application appelé `HelloAppWindow`. Il illustre les fonctionnalités suivantes :

- Détacher un contrôle de la page principale et l’ouvrir dans une nouvelle fenêtre
- Ouvrir de nouvelles instances d’une page dans de nouvelles fenêtres
- Dimensionner et placer de nouvelles fenêtres dans l’application par programmation
- Associer une ContentDialog à la fenêtre appropriée dans l’application

![Exemple d’application avec une seule fenêtre](images/hello-app-window-single.png)
  
> _Exemple d’application avec une seule fenêtre_

![Exemple d’application avec un sélecteur de couleurs détaché et une fenêtre secondaire](images/hello-app-window-multi.png)

> _Exemple d’application avec un sélecteur de couleurs détaché et une fenêtre secondaire_

> **API importantes** : [espace de noms Windows.UI.WindowManagement](/uwp/api/windows.ui.windowmanagement), [classe AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)

## <a name="api-overview"></a>Vue d’ensemble des API

La classe [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) et les autres API de l’espace de noms [WindowManagement](/uwp/api/windows.ui.windowmanagement) sont disponibles à partir de Windows 10, version 1903 (SDK 18362). Si votre application cible des versions antérieures de Windows 10, vous devez [utiliser ApplicationView pour créer des fenêtres secondaires](application-view.md). Les API WindowManagement sont toujours en cours de développement et présentent des [limitations](/uwp/api/windows.ui.windowmanagement.appwindow#limitations) décrites dans les documents de référence sur les API.

Voici certaines API importantes à utiliser pour afficher du contenu dans une AppWindow.

### <a name="appwindow"></a>AppWindow

La classe [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) peut être utilisée pour afficher une partie d’une application Windows Runtime dans une fenêtre secondaire. Elle est similaire à une classe [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview) d’un point de vue conceptuel, mais pas en ce qui concerne le comportement et la durée de vie. Avec la classe AppWindow, toutes les instances partagent le thread de traitement de l’interface utilisateur (y compris le répartiteur d’événements) à partir duquel elles ont été créées, ce qui simplifie les applications à plusieurs fenêtres. Il s’agit là de l’une de ses principales caractéristiques.

Vous pouvez connecter uniquement du contenu XAML à votre AppWindow. Le contenu holographique ou DirectX natif n’est pas pris en charge. Toutefois, vous pouvez afficher un [SwapChainPanel](/uwp/api/windows.ui.xaml.controls.swapchainpanel) XAML qui héberge du contenu DirectX.

### <a name="windowingenvironment"></a>WindowingEnvironment

L’API [WindowingEnvironment](/uwp/api/windows.ui.windowmanagement.windowingenvironment) vous informe de l’environnement dans lequel votre application est présentée et vous permet ainsi de l’adapter selon les besoins. Elle décrit le type de fenêtre pris en charge par l’environnement (par exemple, `Overlapped` si l’application est exécutée sur un PC ou `Tiled` si elle est exécutée sur une Xbox). Elle fournit également un ensemble d’objets DisplayRegion qui décrivent les zones d’un affichage logique dans lesquelles une application peut être présentée.

### <a name="displayregion"></a>DisplayRegion

L’API [DisplayRegion](/uwp/api/windows.ui.windowmanagement.displayregion) décrit la zone d’un affichage logique dans laquelle une vue peut être présentée à un utilisateur. Par exemple, sur un PC de bureau, il s’agit de l’écran complet moins la zone de la barre des tâches. Il ne s’agit pas nécessairement d’une correspondance exacte avec la zone d’affichage physique du moniteur utilisé. Plusieurs zones d’affichage peuvent être présentes sur un même moniteur, ou une DisplayRegion peut être configurée pour s’étendre sur plusieurs moniteurs si ceux-ci sont parfaitement homogènes.

### <a name="appwindowpresenter"></a>AppWindowPresenter

L’API [AppWindowPresenter](/uwp/api/windows.ui.windowmanagement.appwindowpresenter) permet de changer facilement le mode d’affichage des fenêtres selon des configurations prédéfinies comme `FullScreen` ou `CompactOverlay`. Ces configurations offrent à l’utilisateur une expérience cohérente sur tous les appareils qui prennent en charge la configuration.

### <a name="uicontext"></a>UIContext

L’[UIContext](/uwp/api/windows.ui.uicontext) est un identificateur unique pour une vue ou une fenêtre d’application. Il est créé automatiquement et vous pouvez utiliser la propriété [UIElement.UIContext](/uwp/api/windows.ui.xaml.uielement.uicontext) pour le récupérer. Chaque UIElement de l’arborescence XAML a le même UIContext.

 L’UIContext est important car les API telles que [Window.Current](/uwp/api/Windows.UI.Xaml.Window.Current) et le modèle `GetForCurrentView` s’appuient sur l’utilisation d’une ApplicationView/CoreWindow unique avec une seule arborescence XAML par thread à utiliser. Ce n’est pas le cas quand vous utilisez une AppWindow. Vous utilisez alors l’UIContext pour identifier une fenêtre particulière.

### <a name="xamlroot"></a>XamlRoot

La classe [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot) contient une arborescence d’éléments XAML, la connecte à l’objet hôte de fenêtre (par exemple, [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) ou [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview)) et fournit des informations telles que la taille et la visibilité. Vous ne créez pas d’objet XamlRoot directement. En fait, un objet XamlRoot est créé quand vous attachez un élément XAML à une AppWindow. Vous pouvez ensuite utiliser la propriété [UIElement.XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot) pour récupérer le XamlRoot.

Pour plus d’informations sur l’UIContext et la classe XamlRoot, consultez [Portabilité du code entre les hôtes de fenêtrage](show-multiple-views.md#make-code-portable-across-windowing-hosts).

## <a name="show-a-new-window"></a>Afficher une nouvelle fenêtre

Abordons les étapes permettant d’afficher du contenu dans une nouvelle AppWindow.

**Pour afficher une nouvelle fenêtre**

1. Appelez la méthode statique [AppWindow.TryCreateAsync](/uwp/api/windows.ui.windowmanagement.appwindow.trycreateasync) pour créer une [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow).

    ```csharp
    AppWindow appWindow = await AppWindow.TryCreateAsync();
    ```

1. Créez le contenu de la fenêtre.

    En général, vous créez un [frame](/uwp/api/Windows.UI.Xaml.Controls.Frame) XAML et vous naviguez dans le frame pour accéder à une [page](/uwp/api/Windows.UI.Xaml.Controls.Page) XAML, où vous avez défini le contenu de votre application. Pour plus d’informations sur les frames et les pages, consultez [Navigation pair à pair entre deux pages](../basics/navigate-between-two-pages.md).

    ```csharp
    Frame appWindowContentFrame = new Frame();
    appWindowContentFrame.Navigate(typeof(AppWindowMainPage));
    ```

    Toutefois, vous pouvez afficher tout contenu XAML dans l’AppWindow (pas uniquement un frame et une page). Par exemple, vous pouvez afficher un seul contrôle comme [ColorPicker](/uwp/api/windows.ui.xaml.controls.colorpicker) ou un [SwapChainPanel](/uwp/api/windows.ui.xaml.controls.swapchainpanel) qui héberge du contenu DirectX.

1. Appelez la méthode [ElementCompositionPreview.SetAppWindowContent](/api/windows.ui.xaml.hosting.elementcompositionpreview.setappwindowcontent) pour attacher le contenu XAML à l’AppWindow.

    ```csharp
    ElementCompositionPreview.SetAppWindowContent(appWindow, appWindowContentFrame);
    ```

    L’appel à cette méthode crée un objet [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot) et le définit comme propriété [XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot) de l’UIElement spécifié.

    Vous pouvez appeler cette méthode une fois par instance AppWindow uniquement. Dès lors que le contenu a été défini, les appels suivants à SetAppWindowContent pour cette instance AppWindow échouent. De plus, si vous tentez de déconnecter le contenu AppWindow en passant un objet UIElement null, l’appel échoue.

1. Appelez la méthode [AppWindow.TryShowAsync](/uwp/api/windows.ui.windowmanagement.appwindow.tryshowasync) pour afficher la nouvelle fenêtre.

    ```csharp
    await appWindow.TryShowAsync();
    ```

## <a name="release-resources-when-a-window-is-closed"></a>Libérer des ressources à la fermeture d’une fenêtre

Vous devez toujours gérer l’événement [AppWindow.Closed](/uwp/api/windows.ui.windowmanagement.appwindow.closed) pour libérer les ressources XAML (contenu de l’AppWindow) et les références à l’AppWindow.

```csharp
appWindow.Closed += delegate
{
    appWindowContentFrame.Content = null;
    appWindow = null;
};
```

## <a name="track-instances-of-appwindow"></a>Suivre les instances d’AppWindow

Selon la façon dont vous utilisez plusieurs fenêtres dans votre application, vous serez peut-être amené à suivre les instances d’AppWindow que vous créez. L’exemple `HelloAppWindow` illustre plusieurs modes d’utilisation courants d’une [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow). Ici, nous allons examiner la raison pour laquelle ces fenêtres doivent être suivies et voir comment procéder.

### <a name="simple-tracking"></a>Suivi simple

La fenêtre du sélecteur de couleurs héberge un seul contrôle XAML, et le code permettant d’interagir avec le sélecteur de couleurs se trouve entièrement dans le fichier `MainPage.xaml.cs`. La fenêtre du sélecteur de couleurs n’autorise qu’une seule instance et est essentiellement une extension de `MainWindow`. Pour garantir qu’une seule instance est créée, la fenêtre du sélecteur de couleurs est suivie avec une variable de niveau de page. Avant de créer une fenêtre de sélecteur de couleurs, vous allez vérifier si une instance existe. Si c’est le cas, ignorez les étapes de création de fenêtre et appelez simplement [TryShowAsync](/uwp/api/windows.ui.windowmanagement.appwindow.tryshowasync) sur la fenêtre existante.

```csharp
AppWindow colorPickerAppWindow;

// ...

private async void DetachColorPickerButton_Click(object sender, RoutedEventArgs e)
{
    // Create the color picker window.
    if (colorPickerAppWindow == null)
    {
        // ...
        // Create a new window
        colorPickerAppWindow = await AppWindow.TryCreateAsync();
        // ...
    }
    // Show the window.
    await colorPickerAppWindow.TryShowAsync();
}
```

### <a name="track-an-appwindow-instance-in-its-hosted-content"></a>Suivre une instance AppWindow dans son contenu hébergé

La fenêtre `AppWindowPage` héberge une page XAML complète et le code permettant d’interagir avec la page réside dans `AppWindowPage.xaml.cs`. Elle autorise plusieurs instances, qui fonctionnent de manière indépendante.

La fonctionnalité de la page vous permet de manipuler la fenêtre en la définissant sur `FullScreen` ou `CompactOverlay`. Elle écoute également les événements [AppWindow.Changed](/uwp/api/windows.ui.windowmanagement.appwindow.changed) pour afficher des informations sur la fenêtre. Pour appeler ces API, `AppWindowPage` a besoin d’une référence à l’instance AppWindow qui l’héberge.

Si c’est tout ce dont vous avez besoin, vous pouvez créer une propriété dans `AppWindowPage` et lui affecter l’instance [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) au moment de sa création.

**AppWindowPage.xaml.cs**

Dans `AppWindowPage`, créez une propriété pour contenir la référence AppWindow.

```csharp
public sealed partial class AppWindowPage : Page
{
    public AppWindow MyAppWindow { get; set; }

    // ...
}
```

**MainPage.xaml.cs**

Dans `MainPage`, récupérez une référence à l’instance de page et affectez la nouvelle AppWindow à la propriété dans `AppWindowPage`.

```csharp
private async void ShowNewWindowButton_Click(object sender, RoutedEventArgs e)
{
    // Create a new window.
    AppWindow appWindow = await AppWindow.TryCreateAsync();

    // Create a Frame and navigate to the Page you want to show in the new window.
    Frame appWindowContentFrame = new Frame();
    appWindowContentFrame.Navigate(typeof(AppWindowPage));

    // Get a reference to the page instance and assign the
    // newly created AppWindow to the MyAppWindow property.
    AppWindowPage page = (AppWindowPage)appWindowContentFrame.Content;
    page.MyAppWindow = appWindow;

    // ...
}
```

### <a name="tracking-app-windows-using-uicontext"></a>Suivi des fenêtres d’application avec UIContext

Vous pouvez également avoir besoin d’accéder aux instances [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) à partir d’autres parties de votre application. Par exemple, `MainPage` peut contenir un bouton « Close all » qui ferme toutes les instances d’AppWindow suivies.

Dans ce cas, vous devez utiliser l’identificateur unique [UIContext](/uwp/api/windows.ui.uicontext) pour suivre les instances de fenêtre dans un dictionnaire (classe [Dictionary](/dotnet/api/system.collections.generic.dictionary-2?view=dotnet-uwp-10.0)).

**MainPage.xaml.cs**

Dans `MainPage`, créez le dictionnaire comme propriété statique (Dictionary). Ensuite, ajoutez la page au dictionnaire quand vous la créez, puis retirez-la quand elle est fermée. Vous pouvez récupérer l’UIContext à partir du [frame](/uwp/api/Windows.UI.Xaml.Controls.Frame) de contenu (`appWindowContentFrame.UIContext`) après avoir appelé [ElementCompositionPreview.SetAppWindowContent](/api/windows.ui.xaml.hosting.elementcompositionpreview.setappwindowcontent).

```csharp
public sealed partial class MainPage : Page
{
    // Track open app windows in a Dictionary.
    public static Dictionary<UIContext, AppWindow> AppWindows { get; set; }
        = new Dictionary<UIContext, AppWindow>();

    // ...

    private async void ShowNewWindowButton_Click(object sender, RoutedEventArgs e)
    {
        // Create a new window.
        AppWindow appWindow = await AppWindow.TryCreateAsync();

        // Create a Frame and navigate to the Page you want to show in the new window.
        Frame appWindowContentFrame = new Frame();
        appWindowContentFrame.Navigate(typeof(AppWindowPage));

        // Attach the XAML content to the window.
        ElementCompositionPreview.SetAppWindowContent(appWindow, appWindowContentFrame);

        // Add the new page to the Dictionary using the UIContext as the Key.
        AppWindows.Add(appWindowContentFrame.UIContext, appWindow);
        appWindow.Title = "App Window " + AppWindows.Count.ToString();

        // When the window is closed, be sure to release
        // XAML resources and the reference to the window.
        appWindow.Closed += delegate
        {
            MainPage.AppWindows.Remove(appWindowContentFrame.UIContext);
            appWindowContentFrame.Content = null;
            appWindow = null;
        };

        // Show the window.
        await appWindow.TryShowAsync();
    }

    private async void CloseAllButton_Click(object sender, RoutedEventArgs e)
    {
        while (AppWindows.Count > 0)
        {
            await AppWindows.Values.First().CloseAsync();
        }
    }
    // ...
}
```

**AppWindowPage.xaml.cs**

Pour utiliser l’instance [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) dans votre code `AppWindowPage`, utilisez l’[UIContext](/uwp/api/windows.ui.uicontext) de la page pour la récupérer à partir du dictionnaire statique dans `MainPage`. Vous devez le faire dans le gestionnaire d’événements [Loaded](/uwp/api/windows.ui.xaml.frameworkelement.loaded) de la page plutôt que dans le constructeur pour éviter que l’UIContext soit null. Vous pouvez récupérer l’UIContext à partir de la page : `this.UIContext`.

```csharp
public sealed partial class AppWindowPage : Page
{
    AppWindow window;

    // ...
    public AppWindowPage()
    {
        this.InitializeComponent();

        Loaded += AppWindowPage_Loaded;
    }

    private void AppWindowPage_Loaded(object sender, RoutedEventArgs e)
    {
        // Get the reference to this AppWindow that was stored when it was created.
        window = MainPage.AppWindows[this.UIContext];

        // Set up event handlers for the window.
        window.Changed += Window_Changed;
    }
    // ...
}
```

> [!NOTE]
> L’exemple `HelloAppWindow` illustre les deux façons de suivre la fenêtre dans `AppWindowPage`, mais vous n’en utiliserez généralement qu’une.

## <a name="request-window-size-and-placement"></a>Demander à définir la taille et l’emplacement de la fenêtre

La classe [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) offre plusieurs méthodes permettant de contrôler la taille et l’emplacement de la fenêtre. Comme l’indique le nom des méthodes, le système peut ou non appliquer les modifications demandées selon les facteurs environnementaux.

Appelez [RequestSize](/uwp/api/windows.ui.windowmanagement.appwindow.requestsize) pour spécifier la taille de fenêtre souhaitée comme suit.

```csharp
colorPickerAppWindow.RequestSize(new Size(300, 428));
```

Les méthodes permettant de gérer l’emplacement des fenêtres sont nommées _RequestMove*_  : [RequestMoveAdjacentToCurrentView](/uwp/api/windows.ui.windowmanagement.appwindow.requestmoveadjacenttocurrentview), [RequestMoveAdjacentToWindow](/uwp/api/windows.ui.windowmanagement.appwindow.requestmoveadjacenttowindow), [RequestMoveRelativeToDisplayRegion](/uwp/api/windows.ui.windowmanagement.appwindow.requestmoverelativetodisplayregion), [RequestMoveToDisplayRegion](/uwp/api/windows.ui.windowmanagement.appwindow.requestmovetodisplayregion).

Dans cet exemple, ce code place la fenêtre en regard de la vue principale dont elle est issue.

```csharp
colorPickerAppWindow.RequestMoveAdjacentToCurrentView();
```

Pour obtenir des informations sur la taille et l’emplacement actuels de la fenêtre, appelez [GetPlacement](/uwp/api/windows.ui.windowmanagement.appwindow.getplacement). Ceci retourne un objet [AppWindowPlacement](/uwp/api/windows.ui.windowmanagement.appwindowplacement) qui indique la zone d’affichage ([DisplayRegion](/uwp/api/windows.ui.windowmanagement.appwindowplacement.displayregion)), le décalage ([Offset](/uwp/api/windows.ui.windowmanagement.appwindowplacement.offset)) et la taille ([Size](/uwp/api/windows.ui.windowmanagement.appwindowplacement.size)) de la fenêtre.

Par exemple, vous pouvez appeler ce code pour déplacer la fenêtre vers l’angle supérieur droit de l’écran. Ce code doit être appelé après l’affichage de la fenêtre. Sinon, la taille de la fenêtre retournée par l’appel à GetPlacement sera 0,0 et le décalage sera incorrect.

```csharp
DisplayRegion displayRegion = window.GetPlacement().DisplayRegion;
double displayRegionWidth = displayRegion.WorkAreaSize.Width;
double windowWidth = window.GetPlacement().Size.Width;
int horizontalOffset = (int)(displayRegionWidth - windowWidth);
window.RequestMoveRelativeToDisplayRegion(displayRegion, new Point(horizontalOffset, 0));
```

## <a name="request-a-presentation-configuration"></a>Demander une configuration de présentation

La classe [AppWindowPresenter](/uwp/api/windows.ui.windowmanagement.appwindowpresenter) vous permet d’afficher une [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) selon une configuration prédéfinie adaptée à l’appareil sur lequel elle s’affiche. Vous pouvez utiliser une valeur [AppWindowPresentationConfiguration](/uwp/api/windows.ui.windowmanagement.appwindowpresentationconfiguration) pour placer la fenêtre en mode `FullScreen` ou `CompactOverlay`.

Cet exemple montre comment effectuer les opérations suivantes :

- Utilisez l’événement [AppWindow.Changed](/uwp/api/windows.ui.windowmanagement.appwindow.changed) pour être averti en cas de modification des présentations de fenêtres disponibles.
- Utilisez la propriété [AppWindow.Presenter](/uwp/api/windows.ui.windowmanagement.appwindow.presenter) pour obtenir la valeur actuelle d’[AppWindowPresenter](/uwp/api/windows.ui.windowmanagement.appwindowpresenter).
- Appelez [IsPresentationSupported](/uwp/api/windows.ui.windowmanagement.appwindowpresenter.ispresentationsupported) pour savoir si un [AppWindowPresentationKind](/uwp/api/windows.ui.windowmanagement.appwindowpresentationkind) spécifique est pris en charge.
- Appelez [GetConfiguration](/uwp/api/windows.ui.windowmanagement.appwindowpresenter.getconfiguration) pour vérifier le type de configuration actuellement utilisé.
- Appelez [RequestPresentation](/uwp/api/windows.ui.windowmanagement.appwindowpresenter.requestpresentation) pour modifier la configuration actuelle.

```csharp
private void Window_Changed(AppWindow sender, AppWindowChangedEventArgs args)
{
    if (args.DidAvailableWindowPresentationsChange)
    {
        EnablePresentationButtons(sender);
    }

    if (args.DidWindowPresentationChange)
    {
        ConfigText.Text = window.Presenter.GetConfiguration().Kind.ToString();
    }

    if (args.DidSizeChange)
    {
        SizeText.Text = window.GetPlacement().Size.ToString();
    }
}

private void EnablePresentationButtons(AppWindow window)
{
    // Check whether the current AppWindowPresenter supports CompactOverlay.
    if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.CompactOverlay))
    {
        // Show the CompactOverlay button...
        compactOverlayButton.Visibility = Visibility.Visible;
    }
    else
    {
        // Hide the CompactOverlay button...
        compactOverlayButton.Visibility = Visibility.Collapsed;
    }

    // Check whether the current AppWindowPresenter supports FullScreen?
    if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.FullScreen))
    {
        // Show the FullScreen button...
        fullScreenButton.Visibility = Visibility.Visible;
    }
    else
    {
        // Hide the FullScreen button...
        fullScreenButton.Visibility = Visibility.Collapsed;
    }
}

private void CompactOverlayButton_Click(object sender, RoutedEventArgs e)
{
    if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.CompactOverlay)
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.CompactOverlay);
        fullScreenButton.IsChecked = false;
    }
    else
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
    }
}

private void FullScreenButton_Click(object sender, RoutedEventArgs e)
{
    if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.FullScreen)
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.FullScreen);
        compactOverlayButton.IsChecked = false;
    }
    else
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
    }
}
```

## <a name="reuse-xaml-elements"></a>Réutiliser des éléments XAML

Une [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) vous permet d’avoir plusieurs arborescences XAML avec le même thread d’interface utilisateur. Toutefois, un élément XAML peut être ajouté à une arborescence XAML une fois uniquement. Si vous souhaitez déplacer une partie de votre interface utilisateur d’une fenêtre à une autre, vous devez gérer son emplacement dans l’arborescence XAML.

Cet exemple montre comment réutiliser un contrôle [ColorPicker](/uwp/api/windows.ui.xaml.controls.colorpicker) tout en le déplaçant entre la fenêtre principale et une fenêtre secondaire.

Le sélecteur de couleurs est déclaré dans le code XAML pour `MainPage`, ce qui le place dans l’arborescence XAML `MainPage`.

```xaml
<StackPanel x:Name="colorPickerContainer" Grid.Column="1" Background="WhiteSmoke">
    <Button Click="DetachColorPickerButton_Click" HorizontalAlignment="Right">
        <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE2B4;" />
    </Button>
    <ColorPicker x:Name="colorPicker" Margin="12" Width="288"
                 IsColorChannelTextInputVisible="False"
                 ColorChanged="ColorPicker_ColorChanged"/>
</StackPanel>
```

Quand le sélecteur de couleurs est détaché pour être placé dans une nouvelle AppWindow, vous devez d’abord le supprimer de l’arborescence XAML `MainPage` en le retirant de son conteneur parent. Cet exemple masque également le conteneur parent (ce qui n’est pas obligatoire).

```csharp
colorPickerContainer.Children.Remove(colorPicker);
colorPickerContainer.Visibility = Visibility.Collapsed;
```

Vous pouvez ensuite l’ajouter à la nouvelle arborescence XAML. Ici, vous créez d’abord une grille (classe [Grid](/uwp/api/windows.ui.xaml.controls.grid)) qui sera le conteneur parent du ColorPicker, puis vous ajoutez ce dernier comme enfant de la grille. (Ceci vous permettra de supprimer facilement le ColorPicker de cette arborescence XAML ultérieurement.) Vous définissez ensuite la grille comme racine de l’arborescence XAML dans la nouvelle fenêtre.

```csharp
Grid appWindowRootGrid = new Grid();
appWindowRootGrid.Children.Add(colorPicker);

// Create a new window
colorPickerAppWindow = await AppWindow.TryCreateAsync();

// Attach the XAML content to our window
ElementCompositionPreview.SetAppWindowContent(colorPickerAppWindow, appWindowRootGrid);
```

Quand l’[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) est fermée, vous inversez le processus. Tout d’abord, supprimez le [ColorPicker](/uwp/api/windows.ui.xaml.controls.colorpicker) de la grille ([Grid](/uwp/api/windows.ui.xaml.controls.grid)), puis ajoutez-le comme enfant du [StackPanel](/uwp/api/windows.ui.xaml.controls.stackpanel) dans `MainPage`.

```csharp
// When the window is closed, be sure to release XAML resources
// and the reference to the window.
colorPickerAppWindow.Closed += delegate
{
    appWindowRootGrid.Children.Remove(colorPicker);
    appWindowRootGrid = null;
    colorPickerAppWindow = null;

    colorPickerContainer.Children.Add(colorPicker);
    colorPickerContainer.Visibility = Visibility.Visible;
};
```

```csharp
private async void DetachColorPickerButton_Click(object sender, RoutedEventArgs e)
{
    ColorPickerContainer.Visibility = Visibility.Collapsed;

    // Create the color picker window.
    if (colorPickerAppWindow == null)
    {
        ColorPickerContainer.Children.Remove(colorPicker);

        Grid appWindowRootGrid = new Grid();
        appWindowRootGrid.Children.Add(colorPicker);

        // Create a new window
        colorPickerAppWindow = await AppWindow.TryCreateAsync();
        colorPickerAppWindow.RequestMoveAdjacentToCurrentView();
        colorPickerAppWindow.RequestSize(new Size(300, 428));
        colorPickerAppWindow.Title = "Color picker";

        // Attach the XAML content to our window
        ElementCompositionPreview.SetAppWindowContent(colorPickerAppWindow, appWindowRootGrid);

        // When the window is closed, be sure to release XAML resources
        // and the reference to the window.
        colorPickerAppWindow.Closed += delegate
        {
            appWindowRootGrid.Children.Remove(colorPicker);
            appWindowRootGrid = null;
            colorPickerAppWindow = null;

            ColorPickerContainer.Children.Add(colorPicker);
            ColorPickerContainer.Visibility = Visibility.Visible;
        };
    }
    // Show the window.
    await colorPickerAppWindow.TryShowAsync();
}
```

## <a name="show-a-dialog-box"></a>Afficher une boîte de dialogue

Par défaut, le contenu de boîtes de dialogue s’affiche de manière modale, en fonction de la racine [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview). Quand vous utilisez une [ContentDialog](/uwp/api/windows.ui.xaml.controls.contentdialog) dans une [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow), vous devez définir manuellement l’objet XamlRoot dans la boîte de dialogue à la racine de l’hôte XAML.

Pour ce faire, définissez la propriété [XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot) de la ContentDialog sur le même [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot) qu’un élément déjà présent dans l’AppWindow. Ici, ce code est dans le gestionnaire d’événements [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) d’un bouton. Vous pouvez ainsi utiliser le _sender_ (le bouton sur lequel l’utilisateur a cliqué) pour obtenir le XamlRoot.

```csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 8))
{
    simpleDialog.XamlRoot = ((Button)sender).XamlRoot;
}
```

Si vous avez une ou plusieurs AppWindows ouvertes en plus de la fenêtre principale (ApplicationView), chaque fenêtre peut tenter d’ouvrir une boîte de dialogue, car la boîte de dialogue modale bloque uniquement la fenêtre dont elle est issue. Cependant, une seule [ContentDialog](/uwp/api/windows.ui.xaml.controls.contentdialog) peut être ouverte à la fois sur un même thread. Toute tentative d’ouverture de deux ContentDialog lève une exception, même si la tentative d’ouverture se fait dans des AppWindows distinctes.

Pour gérer cela, vous devez au minimum ouvrir la boîte de dialogue dans un bloc `try/catch` pour intercepter l’exception au cas où une autre boîte de dialogue serait déjà ouverte.

```csharp
try
{
    ContentDialogResult result = await simpleDialog.ShowAsync();
}
catch (Exception)
{
    // The dialog didn't open, probably because another dialog is already open.
}
```

Une autre façon de gérer les boîtes de dialogue consiste à suivre la boîte de dialogue actuellement ouverte et à la fermer avant d’essayer d’en ouvrir une nouvelle. Ici, vous créez pour cela une propriété statique dans `MainPage` appelée `CurrentDialog`.

```csharp
public sealed partial class MainPage : Page
{
    // Track the last opened dialog so you can close it if another dialog tries to open.
    public static ContentDialog CurrentDialog { get; set; } = null;

   // ...
}
```

Ensuite, vous vérifiez si une boîte de dialogue est actuellement ouverte et, si c’est le cas, vous appelez la méthode [Hide](/uwp/api/windows.ui.xaml.controls.contentdialog.hide) pour la fermer. Enfin, vous affectez la nouvelle boîte de dialogue à `CurrentDialog`et essayez de l’afficher.

```csharp
private async void DialogButton_Click(object sender, RoutedEventArgs e)
{
    ContentDialog simpleDialog = new ContentDialog
    {
        Title = "Content dialog",
        Content = "Dialog box for " + window.Title,
        CloseButtonText = "Ok"
    };

    if (MainPage.CurrentDialog != null)
    {
        MainPage.CurrentDialog.Hide();
    }
    MainPage.CurrentDialog = simpleDialog;

    // Use this code to associate the dialog to the appropriate AppWindow by setting
    // the dialog's XamlRoot to the same XamlRoot as an element that is already
    // present in the AppWindow.
    if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 8))
    {
        simpleDialog.XamlRoot = ((Button)sender).XamlRoot;
    }

    try
    {
        ContentDialogResult result = await simpleDialog.ShowAsync();
    }
    catch (Exception)
    {
        // The dialog didn't open, probably because another dialog is already open.
    }
}
```

S’il n’est pas souhaitable de fermer une boîte de dialogue par programmation, ne l’affectez pas comme `CurrentDialog`. Ici, `MainPage` affiche une boîte de dialogue importante qui ne doit être fermée que si l’utilisateur clique sur `Ok`. Étant donné qu’elle n’est pas affectée comme `CurrentDialog`, aucune tentative de fermeture n’est effectuée programmatiquement.

```csharp
public sealed partial class MainPage : Page
{
    // Track the last opened dialog so you can close it if another dialog tries to open.
    public static ContentDialog CurrentDialog { get; set; } = null;

    // ...
    private async void DialogButton_Click(object sender, RoutedEventArgs e)
    {
        ContentDialog importantDialog = new ContentDialog
        {
            Title = "Important dialog",
            Content = "This dialog can only be dismissed by clicking Ok.",
            CloseButtonText = "Ok"
        };

        if (MainPage.CurrentDialog != null)
        {
            MainPage.CurrentDialog.Hide();
        }
        // Do not track this dialog as the MainPage.CurrentDialog.
        // It should only be closed by clicking the Ok button.
        MainPage.CurrentDialog = null;

        try
        {
            ContentDialogResult result = await importantDialog.ShowAsync();
        }
        catch (Exception)
        {
            // The dialog didn't open, probably because another dialog is already open.
        }
    }
    // ...
}
```

## <a name="complete-code"></a>Code complet

### <a name="mainpagexaml"></a>MainPage.xaml

```xaml
<Page
    x:Class="HelloAppWindow.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:HelloAppWindow"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition Width="Auto"/>
        </Grid.ColumnDefinitions>
        <StackPanel VerticalAlignment="Center" HorizontalAlignment="Center">
            <Button x:Name="NewWindowButton" Content="Open new window" 
                    Click="ShowNewWindowButton_Click" Margin="0,12"/>
            <Button Content="Open dialog" Click="DialogButton_Click" 
                    HorizontalAlignment="Stretch"/>
            <Button Content="Close all" Click="CloseAllButton_Click" 
                    Margin="0,12" HorizontalAlignment="Stretch"/>
        </StackPanel>

<StackPanel x:Name="colorPickerContainer" Grid.Column="1" Background="WhiteSmoke">
    <Button Click="DetachColorPickerButton_Click" HorizontalAlignment="Right">
        <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE2B4;" />
    </Button>
            <ColorPicker x:Name="colorPicker" Margin="12" Width="288"
                 IsColorChannelTextInputVisible="False"
                 ColorChanged="ColorPicker_ColorChanged"/>
        </StackPanel>
    </Grid>
</Page>

```

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Windows.Foundation;
using Windows.UI;
using Windows.UI.WindowManagement;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Hosting;
using Windows.UI.Xaml.Media;

// The Blank Page item template is documented at https://go.microsoft.com/fwlink/?LinkId=402352&clcid=0x409

namespace HelloAppWindow
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        AppWindow colorPickerAppWindow;

        // Track open app windows in a Dictionary.
        public static Dictionary<UIContext, AppWindow> AppWindows { get; set; }
            = new Dictionary<UIContext, AppWindow>();

        // Track the last opened dialog so you can close it if another dialog tries to open.
        public static ContentDialog CurrentDialog { get; set; } = null;

        public MainPage()
        {
            this.InitializeComponent();
        }

        private async void ShowNewWindowButton_Click(object sender, RoutedEventArgs e)
        {
            // Create a new window.
            AppWindow appWindow = await AppWindow.TryCreateAsync();

            // Create a Frame and navigate to the Page you want to show in the new window.
            Frame appWindowContentFrame = new Frame();
            appWindowContentFrame.Navigate(typeof(AppWindowPage));

            // Get a reference to the page instance and assign the
            // newly created AppWindow to the MyAppWindow property.
            AppWindowPage page = (AppWindowPage)appWindowContentFrame.Content;
            page.MyAppWindow = appWindow;
            page.TextColorBrush = new SolidColorBrush(colorPicker.Color);

            // Attach the XAML content to the window.
            ElementCompositionPreview.SetAppWindowContent(appWindow, appWindowContentFrame);

            // Add the new page to the Dictionary using the UIContext as the Key.
            AppWindows.Add(appWindowContentFrame.UIContext, appWindow);
            appWindow.Title = "App Window " + AppWindows.Count.ToString();

            // When the window is closed, be sure to release XAML resources
            // and the reference to the window.
            appWindow.Closed += delegate
            {
                MainPage.AppWindows.Remove(appWindowContentFrame.UIContext);
                appWindowContentFrame.Content = null;
                appWindow = null;
            };

            // Show the window.
            await appWindow.TryShowAsync();
        }

        private async void DialogButton_Click(object sender, RoutedEventArgs e)
        {
            ContentDialog importantDialog = new ContentDialog
            {
                Title = "Important dialog",
                Content = "This dialog can only be dismissed by clicking Ok.",
                CloseButtonText = "Ok"
            };

            if (MainPage.CurrentDialog != null)
            {
                MainPage.CurrentDialog.Hide();
            }
            // Do not track this dialog as the MainPage.CurrentDialog.
            // It should only be closed by clicking the Ok button.
            MainPage.CurrentDialog = null;

            try
            {
                ContentDialogResult result = await importantDialog.ShowAsync();
            }
            catch (Exception)
            {
                // The dialog didn't open, probably because another dialog is already open.
            }
        }

        private async void DetachColorPickerButton_Click(object sender, RoutedEventArgs e)
        {
            // Create the color picker window.
            if (colorPickerAppWindow == null)
            {
                colorPickerContainer.Children.Remove(colorPicker);
                colorPickerContainer.Visibility = Visibility.Collapsed;

                Grid appWindowRootGrid = new Grid();
                appWindowRootGrid.Children.Add(colorPicker);

                // Create a new window
                colorPickerAppWindow = await AppWindow.TryCreateAsync();
                colorPickerAppWindow.RequestMoveAdjacentToCurrentView();
                colorPickerAppWindow.RequestSize(new Size(300, 428));
                colorPickerAppWindow.Title = "Color picker";

                // Attach the XAML content to our window
                ElementCompositionPreview.SetAppWindowContent(colorPickerAppWindow, appWindowRootGrid);

                // Make sure to release the reference to this window, 
                // and release XAML resources, when it's closed
                colorPickerAppWindow.Closed += delegate
                {
                    appWindowRootGrid.Children.Remove(colorPicker);
                    appWindowRootGrid = null;
                    colorPickerAppWindow = null;

                    colorPickerContainer.Children.Add(colorPicker);
                    colorPickerContainer.Visibility = Visibility.Visible;
                };
            }
            // Show the window.
            await colorPickerAppWindow.TryShowAsync();
        }

        private void ColorPicker_ColorChanged(ColorPicker sender, ColorChangedEventArgs args)
        {
            NewWindowButton.Background = new SolidColorBrush(args.NewColor);
        }

        private async void CloseAllButton_Click(object sender, RoutedEventArgs e)
        {
            while (AppWindows.Count > 0)
            {
                await AppWindows.Values.First().CloseAsync();
            }
        }
    }
}

```

### <a name="appwindowpagexaml"></a>AppWindowPage.xaml

```xaml
<Page
    x:Class="HelloAppWindow.AppWindowPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:HelloAppWindow"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid>
        <TextBlock x:Name="TitleTextBlock" Text="Hello AppWindow!" FontSize="24" HorizontalAlignment="Center" Margin="24"/>

        <StackPanel VerticalAlignment="Center" HorizontalAlignment="Center">
            <Button Content="Open dialog" Click="DialogButton_Click"
                    Width="200" Margin="0,4"/>
            <Button Content="Move window" Click="MoveWindowButton_Click"
                    Width="200" Margin="0,4"/>
            <ToggleButton Content="Compact Overlay" x:Name="compactOverlayButton" Click="CompactOverlayButton_Click"
                          Width="200" Margin="0,4"/>
            <ToggleButton Content="Full Screen" x:Name="fullScreenButton" Click="FullScreenButton_Click"
                          Width="200" Margin="0,4"/>
            <Grid>
                <TextBlock Text="Size:"/>
                <TextBlock x:Name="SizeText" HorizontalAlignment="Right"/>
            </Grid>
            <Grid>
                <TextBlock Text="Presentation:"/>
                <TextBlock x:Name="ConfigText" HorizontalAlignment="Right"/>
            </Grid>
        </StackPanel>
    </Grid>
</Page>
```

### <a name="appwindowpagexamlcs"></a>AppWindowPage.xaml.cs

```csharp
using System;
using Windows.Foundation;
using Windows.Foundation.Metadata;
using Windows.UI;
using Windows.UI.WindowManagement;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

// The Blank Page item template is documented at https://go.microsoft.com/fwlink/?LinkId=234238

namespace HelloAppWindow
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class AppWindowPage : Page
    {
        AppWindow window;

        public AppWindow MyAppWindow { get; set; }

        public SolidColorBrush TextColorBrush { get; set; } = new SolidColorBrush(Colors.Black);

        public AppWindowPage()
        {
            this.InitializeComponent();

            Loaded += AppWindowPage_Loaded;
        }

        private void AppWindowPage_Loaded(object sender, RoutedEventArgs e)
        {
            // Get the reference to this AppWindow that was stored when it was created.
            window = MainPage.AppWindows[this.UIContext];

            // Set up event handlers for the window.
            window.Changed += Window_Changed;

            TitleTextBlock.Foreground = TextColorBrush;
        }

        private async void DialogButton_Click(object sender, RoutedEventArgs e)
        {
            ContentDialog simpleDialog = new ContentDialog
            {
                Title = "Content dialog",
                Content = "Dialog box for " + window.Title,
                CloseButtonText = "Ok"
            };

            if (MainPage.CurrentDialog != null)
            {
                MainPage.CurrentDialog.Hide();
            }
            MainPage.CurrentDialog = simpleDialog;

            // Use this code to associate the dialog to the appropriate AppWindow by setting
            // the dialog's XamlRoot to the same XamlRoot as an element that is already 
            // present in the AppWindow.
            if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 8))
            {
                simpleDialog.XamlRoot = ((Button)sender).XamlRoot;
            }

            try
            {
                ContentDialogResult result = await simpleDialog.ShowAsync();
            }
            catch (Exception)
            {
                // The dialog didn't open, probably because another dialog is already open.
            }
        }

        private void Window_Changed(AppWindow sender, AppWindowChangedEventArgs args)
        {
            if (args.DidAvailableWindowPresentationsChange)
            {
                EnablePresentationButtons(sender);
            }

            if (args.DidWindowPresentationChange)
            {
                ConfigText.Text = window.Presenter.GetConfiguration().Kind.ToString();
            }

            if (args.DidSizeChange)
            {
                SizeText.Text = window.GetPlacement().Size.ToString();
            }
        }

        private void EnablePresentationButtons(AppWindow window)
        {
            // Check whether the current AppWindowPresenter supports CompactOverlay.
            if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.CompactOverlay))
            {
                // Show the CompactOverlay button...
                compactOverlayButton.Visibility = Visibility.Visible;
            }
            else
            {
                // Hide the CompactOverlay button...
                compactOverlayButton.Visibility = Visibility.Collapsed;
            }

            // Check whether the current AppWindowPresenter supports FullScreen?
            if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.FullScreen))
            {
                // Show the FullScreen button...
                fullScreenButton.Visibility = Visibility.Visible;
            }
            else
            {
                // Hide the FullScreen button...
                fullScreenButton.Visibility = Visibility.Collapsed;
            }
        }

        private void CompactOverlayButton_Click(object sender, RoutedEventArgs e)
        {
            if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.CompactOverlay)
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.CompactOverlay);
                fullScreenButton.IsChecked = false;
            }
            else
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
            }
        }

        private void FullScreenButton_Click(object sender, RoutedEventArgs e)
        {
            if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.FullScreen)
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.FullScreen);
                compactOverlayButton.IsChecked = false;
            }
            else
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
            }
        }

        private void MoveWindowButton_Click(object sender, RoutedEventArgs e)
        {
            DisplayRegion displayRegion = window.GetPlacement().DisplayRegion;
            double displayRegionWidth = displayRegion.WorkAreaSize.Width;
            double windowWidth = window.GetPlacement().Size.Width;
            int horizontalOffset = (int)(displayRegionWidth - windowWidth);
            window.RequestMoveRelativeToDisplayRegion(displayRegion, new Point(horizontalOffset, 0));
        }
    }
}
```

## <a name="related-topics"></a>Rubriques connexes

- [Afficher plusieurs vues](show-multiple-views.md)
- [Afficher plusieurs vues avec ApplicationView](application-view.md)
