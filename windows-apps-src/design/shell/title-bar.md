---
description: Personnaliser la barre de titre d’une application de bureau pour qu’elle corresponde à la personnalité de l’application.
title: Personnalisation de la barre de titre
template: detail.hbs
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP, barre de titre
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 6722004efea76ce1a7a2b6eba92d45e8dbf126ba
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174573"
---
# <a name="title-bar-customization"></a>Personnalisation de la barre de titre



Lorsque votre application s’exécute dans une fenêtre de bureau, vous pouvez personnaliser les barres de titre pour qu’elles correspondent à la personnalité de votre application. Les API de personnalisation de la barre de titre vous permettent de spécifier des couleurs pour les éléments de barre de titre, ou d’étendre le contenu de votre application dans la zone de barre de titre et d’en prendre le contrôle total.

> **API importantes**: [propriété ApplicationView. TitleBar](/uwp/api/windows.ui.viewmanagement.applicationview), [classe ApplicationViewTitleBar](/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar), [classe CoreApplicationViewTitleBar](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar)

## <a name="how-much-to-customize-the-title-bar"></a>Quantité de personnalisation de la barre de titre

Il existe deux niveaux de personnalisation que vous pouvez appliquer à la barre de titre.

Pour une personnalisation des couleurs simple, vous pouvez définir des propriétés [ApplicationViewTitleBar](/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar) pour spécifier les couleurs que vous souhaitez utiliser pour les éléments de barre de titre. Dans ce cas, le système conserve la responsabilité pour tous les autres aspects de la barre de titre, par exemple en dessinant le titre de l’application et en définissant des zones glissables.

Votre autre option consiste à masquer la barre de titre par défaut et à la remplacer par votre propre contenu XAML. Par exemple, vous pouvez placer du texte, des boutons ou des menus personnalisés dans la zone de barre de titre. Vous devrez également utiliser cette option pour étendre un arrière-plan [acrylique](../style/acrylic.md) à la zone de barre de titre.

Lorsque vous optez pour une personnalisation complète, vous êtes chargé de placer le contenu dans la zone de barre de titre, et vous pouvez définir votre propre région pouvant faire l’être. Les boutons précédent, fermer, réduire et agrandir du système sont toujours disponibles et gérés par le système, mais les éléments comme le titre de l’application ne le sont pas. Vous devrez créer ces éléments vous-même en fonction des besoins de votre application.

> [!NOTE]
> La personnalisation des couleurs simple est disponible pour les applications Windows à l’aide de XAML, DirectX et HTML. La personnalisation complète est disponible uniquement pour les applications Windows utilisant XAML.

## <a name="simple-color-customization"></a>Personnalisation des couleurs simples

Si vous souhaitez uniquement personnaliser les couleurs de la barre de titre et que vous ne faites rien de trop fantaisie (par exemple, en plaçant des tabulations dans votre barre de titre), vous pouvez définir les propriétés de couleur sur le [ApplicationViewTitleBar](/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar) pour la fenêtre de votre application.

Cet exemple montre comment récupérer une instance de ApplicationViewTitleBar et définir ses propriétés de couleur.

```csharp
// using Windows.UI.ViewManagement;

var titleBar = ApplicationView.GetForCurrentView().TitleBar;

// Set active window colors
titleBar.ForegroundColor = Windows.UI.Colors.White;
titleBar.BackgroundColor = Windows.UI.Colors.Green;
titleBar.ButtonForegroundColor = Windows.UI.Colors.White;
titleBar.ButtonBackgroundColor = Windows.UI.Colors.SeaGreen;
titleBar.ButtonHoverForegroundColor = Windows.UI.Colors.White;
titleBar.ButtonHoverBackgroundColor = Windows.UI.Colors.DarkSeaGreen;
titleBar.ButtonPressedForegroundColor = Windows.UI.Colors.Gray;
titleBar.ButtonPressedBackgroundColor = Windows.UI.Colors.LightGreen;

// Set inactive window colors
titleBar.InactiveForegroundColor = Windows.UI.Colors.Gray;
titleBar.InactiveBackgroundColor = Windows.UI.Colors.SeaGreen;
titleBar.ButtonInactiveForegroundColor = Windows.UI.Colors.Gray;
titleBar.ButtonInactiveBackgroundColor = Windows.UI.Colors.SeaGreen;
```

> [!NOTE]
> Ce code peut être placé dans la méthode [OnLaunched](/uwp/api/windows.ui.xaml.application.onlaunched) de votre application (_app.Xaml.cs_), après l’appel à [Window. Activate](/uwp/api/windows.ui.xaml.window.Activate)ou dans la première page de votre application.

> [!TIP]
> Le kit de pratiques de la communauté Windows fournit des extensions qui vous permettent de définir ces propriétés de couleur en XAML. Pour plus d’informations, consultez la documentation du kit de la [communauté Windows](/windows/uwpcommunitytoolkit/extensions/viewextensions).

Voici quelques éléments à prendre en compte lors de la définition des couleurs de la barre de titre :

- La couleur d’arrière-plan du bouton n’est pas appliquée aux États du bouton de fermeture et au pointage enfoncé. Le bouton Fermer utilise toujours la couleur définie par le système pour ces États.
- Les propriétés de couleur du bouton sont appliquées au bouton précédent du système lorsqu’il est utilisé. ([Voir l’historique de navigation et la navigation vers l’arrière](../basics/navigation-history-and-backwards-navigation.md).)
- Si vous affectez à une propriété Color la **valeur null** , elle est rétablie à la couleur système par défaut.
- Vous ne pouvez pas définir de couleurs transparentes. Le canal alpha de la couleur est ignoré.

Windows donne à un utilisateur la possibilité d’appliquer la [couleur d’accentuation](../style/color.md#accent-color) sélectionnée à la barre de titre. Si vous définissez une couleur de barre de titre, nous vous recommandons de définir explicitement toutes les couleurs. Cela garantit qu’il n’existe aucune combinaison de couleurs non intentionnelle qui se produit en raison des paramètres de couleur définis par l’utilisateur.

## <a name="full-customization"></a>Personnalisation complète

Lorsque vous optez pour la personnalisation de la barre de titre complète, la zone cliente de votre application est étendue pour couvrir la totalité de la fenêtre, y compris la zone de barre de titre. Vous êtes responsable du dessin et de la gestion des entrées pour la totalité de la fenêtre, à l’exception des boutons de légende, qui sont superposés sur la zone de dessin de l’application.

Pour masquer la barre de titre par défaut et étendre votre contenu dans la zone de barre de titre, affectez la valeur **true**à la propriété [CoreApplicationViewTitleBar. ExtendViewIntoTitleBar](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar) .

Cet exemple montre comment récupérer le CoreApplicationViewTitleBar et définir la propriété ExtendViewIntoTitleBar sur **true**. Vous pouvez le faire dans la méthode [OnLaunched](/uwp/api/windows.ui.xaml.application.onlaunched) de votre application (_app.Xaml.cs_) ou dans la première page de votre application.

```csharp
// using Windows.ApplicationModel.Core;

// Hide default title bar.
var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
coreTitleBar.ExtendViewIntoTitleBar = true;
```

> [!TIP]
> Ce paramètre persiste lorsque votre application est fermée et redémarrée. Dans Visual Studio, si vous affectez à ExtendViewIntoTitleBar la valeur **true**et que vous souhaitez ensuite rétablir la valeur par défaut, vous devez définir explicitement la valeur **false** et exécuter votre application pour remplacer le paramètre PERSISTED.

### <a name="draggable-regions"></a>Régions glissables

La zone Glissable de la barre de titre définit l’emplacement où l’utilisateur peut cliquer et faire glisser pour déplacer la fenêtre (par opposition au simple glissement du contenu dans la zone de dessin de l’application). Vous spécifiez la région pouvant être glissée en appelant la méthode [Window. SetTitleBar](/uwp/api/windows.ui.xaml.window.settitlebar) et en passant dans un UIElement qui définit la région Glissable. (L’UIElement est souvent un panneau qui contient d’autres éléments.)

Voici comment définir une grille de contenu comme zone de barre de titre Glissable. Ce code est inséré dans le code XAML et le code-behind de la première page de votre application. Consultez la section [exemple complet de personnalisation](./title-bar.md#full-customization-example) pour obtenir le code complet.


> [!IMPORTANT]
> Par défaut, certains éléments d’interface utilisateur tels que Grid ne participent pas au test de positionnement lorsqu’ils n’ont pas de jeu d’arrière-plan.
> Pour que la grille `AppTitleBar` dans l’exemple ci-dessous autorise le glissement, nous devons définir l’arrière-plan sur `Transparent` .

```xaml
<Grid x:Name="AppTitleBar" Background="Transparent">
    <!-- Width of the padding columns is set in LayoutMetricsChanged handler. -->
    <!-- Using padding columns instead of Margin ensures that the background
         paints the area under the caption control buttons (for transparent buttons). -->
    <Grid.ColumnDefinitions>
        <ColumnDefinition x:Name="LeftPaddingColumn" Width="0"/>
        <ColumnDefinition/>
        <ColumnDefinition x:Name="RightPaddingColumn" Width="0"/>
    </Grid.ColumnDefinitions>
    <Image Source="Assets/Square44x44Logo.png" 
           Grid.Column="1" HorizontalAlignment="Left" 
           Width="20" Height="20" Margin="12,0"/>
    <TextBlock Text="Custom Title Bar" 
               Grid.Column="1" 
               Style="{StaticResource CaptionTextBlockStyle}" 
               Margin="44,8,0,0"/>
</Grid>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
    coreTitleBar.ExtendViewIntoTitleBar = true;
    coreTitleBar.LayoutMetricsChanged += CoreTitleBar_LayoutMetricsChanged;
    // Set XAML element as a draggable region.
    Window.Current.SetTitleBar(AppTitleBar);
}

private void CoreTitleBar_LayoutMetricsChanged(CoreApplicationViewTitleBar sender, object args)
{
    AppTitleBar.Height = sender.Height;
}
```

UIElement ( `AppTitleBar` ) fait partie du code XAML de votre application. Vous pouvez déclarer et définir la barre de titre dans une page racine qui ne change pas, ou bien déclarer et définir une zone de barre de titre dans chaque page à laquelle votre application peut accéder. Si vous le configurez dans chaque page, vous devez vous assurer que la région Glissable reste cohérente lorsqu’un utilisateur navigue autour de votre application.

Vous pouvez appeler SetTitleBar pour basculer vers un nouvel élément de barre de titre pendant que votre application est en cours d’exécution. Vous pouvez également passer **null** comme paramètre à SetTitleBar pour rétablir le comportement de glissement par défaut. (Pour plus d’informations, consultez « région Glissable par défaut ».)

> [!IMPORTANT]
> La région Glissable que vous spécifiez doit être testée par accès, ce qui signifie que pour certains éléments, vous devrez peut-être définir un pinceau d’arrière-plan transparent. Pour plus d’informations, consultez les notes sur [VisualTreeHelper. FindElementsInHostCoordinates](/uwp/api/windows.ui.xaml.media.visualtreehelper.findelementsinhostcoordinates) .
>
>Par exemple, si vous définissez une grille comme région Glissable, définissez `Background="Transparent"` pour la rendre déplacée.
>
>Cette grille ne peut pas être glissée (mais des éléments visibles dans celle-ci sont) : `<Grid x:Name="AppTitleBar">` .
>
>Cette grille est similaire, mais l’ensemble de la grille peut être glissé : `<Grid x:Name="AppTitleBar" Background="Transparent">` .

#### <a name="default-draggable-region"></a>Région Glissable par défaut

Si vous ne spécifiez pas une région pouvant faire l’être, un rectangle représentant la largeur de la fenêtre, la hauteur des boutons de légende et positionnée le long du bord supérieur de la fenêtre est défini comme zone de glissement par défaut.

Si vous définissez une région Glissable, le système réduit la zone de glissement par défaut vers le niveau d’une petite zone de la taille d’un bouton de légende, positionné à gauche des boutons de légende (ou à droite si les boutons de légende se trouvent sur le côté gauche de la fenêtre). Cela garantit qu’il y a toujours une zone cohérente que l’utilisateur peut faire glisser.

### <a name="system-caption-buttons"></a>Boutons de légende système

Le système réserve l’angle supérieur gauche ou supérieur droit de la fenêtre d’application pour les boutons de légende système (précédent, réduire, agrandir, fermer). Le système conserve le contrôle de la zone de contrôle de légende pour garantir que la fonctionnalité minimale est fournie pour faire glisser, réduire, agrandir et fermer la fenêtre. Le système dessine le bouton Fermer dans l’angle supérieur droit pour les langues de gauche à droite et l’angle supérieur gauche pour les langues de droite à gauche.

Les dimensions et la position de la zone de contrôle de légende sont communiquées par la classe CoreApplicationViewTitleBar pour vous permettre de la tenir compte de la disposition de l’interface utilisateur de votre barre de titre. La largeur de la région réservée de chaque côté est donnée par les propriétés [SystemOverlayLeftInset](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.SystemOverlayLeftInset) ou [SystemOverlayRightInset](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.SystemOverlayRightInset) , et sa hauteur est donnée par la propriété [Height](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.Height) .

Vous pouvez dessiner du contenu sous la zone de contrôle de légende définie par ces propriétés, telles que l’arrière-plan de votre application, mais vous ne devez pas placer l’interface utilisateur avec laquelle l’utilisateur peut interagir. Elle ne reçoit aucune entrée, car l’entrée pour les contrôles de légende est gérée par le système.

Vous pouvez gérer l’événement [LayoutMetricsChanged](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.LayoutMetricsChanged) pour répondre aux modifications apportées à la taille des boutons de légende. Par exemple, cela peut se produire lorsque le bouton précédent du système est affiché ou masqué. Gérez cet événement pour vérifier et mettre à jour le positionnement des éléments d’interface utilisateur qui dépendent de la taille de la barre de titre.

Cet exemple montre comment ajuster la disposition de votre barre de titre pour tenir compte des modifications telles que le bouton précédent du système affiché ou masqué. `AppTitleBar`, `LeftPaddingColumn` et `RightPaddingColumn` sont déclarés dans le XAML présenté précédemment.

```csharp
private void CoreTitleBar_LayoutMetricsChanged(CoreApplicationViewTitleBar sender, object args)
{
    UpdateTitleBarLayout(sender);
}

private void UpdateTitleBarLayout(CoreApplicationViewTitleBar coreTitleBar)
{
    // Get the size of the caption controls area and back button 
    // (returned in logical pixels), and move your content around as necessary.
    LeftPaddingColumn.Width = new GridLength(coreTitleBar.SystemOverlayLeftInset);
    RightPaddingColumn.Width = new GridLength(coreTitleBar.SystemOverlayRightInset);
    TitleBarButton.Margin = new Thickness(0,0,coreTitleBar.SystemOverlayRightInset,0);

    // Update title bar control size as needed to account for system size changes.
    AppTitleBar.Height = coreTitleBar.Height;
}
```

### <a name="interactive-content"></a>Contenu interactif

Vous pouvez placer des contrôles interactifs, tels que des boutons, des menus ou une zone de recherche, dans la partie supérieure de l’application afin qu’ils apparaissent dans la barre de titre. Toutefois, il existe quelques règles que vous devez suivre pour vous assurer que vos éléments interactifs reçoivent une entrée d’utilisateur.
- Vous devez appeler SetTitleBar pour définir une zone comme zone de barre de titre qui peut être Glissable. Si vous ne le faites pas, le système définit la région Glissable par défaut en haut de la page. Le système gère alors toutes les entrées d’utilisateur dans cette zone et empêche les entrées d’atteindre vos contrôles.
- Placez vos contrôles interactifs sur la partie supérieure de la région Glissable définie par l’appel à SetTitleBar (avec un ordre de plan plus élevé). Ne rendez pas vos enfants de contrôles interactifs de l’UIElement transmis à SetTitleBar. Une fois que vous avez transmis un élément à SetTitleBar, le système le traite comme la barre de titre du système et gère toutes les entrées de pointeur sur cet élément.

Ici, l' `TitleBarButton` élément a un ordre de plan supérieur à `AppTitleBar` , donc il reçoit une entrée d’utilisateur.

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition />
    </Grid.RowDefinitions>

    <Grid x:Name="AppTitleBar" Background="Transparent">
        <!-- Width of the padding columns is set in LayoutMetricsChanged handler. -->
        <!-- Using padding columns instead of Margin ensures that the background
             paints the area under the caption control buttons (for transparent buttons). -->
        <Grid.ColumnDefinitions>
            <ColumnDefinition x:Name="LeftPaddingColumn" Width="0"/>
            <ColumnDefinition/>
            <ColumnDefinition x:Name="RightPaddingColumn" Width="0"/>
        </Grid.ColumnDefinitions>
        <Image Source="Assets/Square44x44Logo.png"
               Grid.Column="1" HorizontalAlignment="Left"
               Width="20" Height="20" Margin="12,0"/>
        <TextBlock Text="Custom Title Bar"
                   Grid.Column="1"
                   Style="{StaticResource CaptionTextBlockStyle}"
                   Margin="44,8,0,0"/>
    </Grid>

    <!-- This Button has a higher z-order than AppTitleBar, 
         so it receives user input. -->
    <Button x:Name="TitleBarButton" Content="Button in the title bar"
        HorizontalAlignment="Right"/>
</Grid>
```

### <a name="transparency-in-caption-buttons"></a>Transparence dans les boutons de légende

Lorsque vous affectez à ExtendViewIntoTitleBar la **valeur true**, vous pouvez rendre l’arrière-plan des boutons de légende transparent pour permettre l’affichage de l’arrière-plan de votre application. En général, vous définissez l’arrière-plan sur [couleurs. transparent](/uwp/api/windows.ui.colors.Transparent) pour une transparence totale. Pour la transparence partielle, définissez le canal alpha de la [couleur](/uwp/api/windows.ui.color) sur laquelle vous avez défini la propriété.

Ces propriétés ApplicationViewTitleBar peuvent être transparentes :

- ButtonBackgroundColor
- ButtonHoverBackgroundColor
- ButtonPressedBackgroundColor
- ButtonInactiveBackgroundColor

La couleur d’arrière-plan du bouton n’est pas appliquée aux États du bouton de fermeture et au pointage enfoncé. Le bouton Fermer utilise toujours la couleur définie par le système pour ces États.

Toutes les autres propriétés de couleur continuent d’ignorer le canal alpha. Si ExtendViewIntoTitleBar a la valeur **false**, le canal alpha est toujours ignoré pour toutes les propriétés de couleur ApplicationViewTitleBar.

### <a name="full-screen-and-tablet-mode"></a>Mode plein écran et Tablet PC

Lorsque votre application s’exécute en mode _plein écran_ ou _en mode tablette_, le système masque les boutons de contrôle de barre de titre et de légende. Toutefois, l’utilisateur peut appeler la barre de titre pour qu’il apparaisse sous la forme d’une superposition en haut de l’interface utilisateur de l’application.
Vous pouvez gérer l’événement [CoreApplicationViewTitleBar. IsVisibleChanged](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.IsVisibleChanged) pour être averti lorsque la barre de titre est masquée ou appelée, et afficher ou masquer le contenu de la barre de titre personnalisée si nécessaire.

Cet exemple montre comment gérer IsVisibleChanged pour afficher et masquer l' `AppTitleBar` élément présenté précédemment.

```csharp
public MainPage()
{
    this.InitializeComponent();

    var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;

    // Register a handler for when the title bar visibility changes.
    // For example, when the title bar is invoked in full screen mode.
    coreTitleBar.IsVisibleChanged += CoreTitleBar_IsVisibleChanged;
}

private void CoreTitleBar_IsVisibleChanged(CoreApplicationViewTitleBar sender, object args)
{
    if (sender.IsVisible)
    {
        AppTitleBar.Visibility = Visibility.Visible;
    }
    else
    {
        AppTitleBar.Visibility = Visibility.Collapsed;
    }
}
```

>[!NOTE]
>Le mode _plein écran_ ne peut être entré que s’il est pris en charge par votre application. Pour plus d’informations, consultez [ApplicationView. IsFullScreenMode](/uwp/api/windows.ui.viewmanagement.applicationview.IsFullScreenMode) . Le [_mode tablette_](https://support.microsoft.com/help/17210/windows-10-use-your-pc-like-a-tablet) est une option utilisateur sur le matériel pris en charge, de sorte qu’un utilisateur peut choisir d’exécuter n’importe quelle application en mode tablette.

## <a name="full-customization-example"></a>Exemple de personnalisation complète

```xaml
<Page
    x:Class="CustomTitleBar.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:CustomTitleBar"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition />
        </Grid.RowDefinitions>

        <Grid x:Name="AppTitleBar" Background="Transparent">
            <!-- Width of the padding columns is set in LayoutMetricsChanged handler. -->
            <!-- Using padding columns instead of Margin ensures that the background
                 paints the area under the caption control buttons (for transparent buttons). -->
            <Grid.ColumnDefinitions>
                <ColumnDefinition x:Name="LeftPaddingColumn" Width="0"/>
                <ColumnDefinition/>
                <ColumnDefinition x:Name="RightPaddingColumn" Width="0"/>
            </Grid.ColumnDefinitions>
            <Image Source="Assets/Square44x44Logo.png" 
                   Grid.Column="1" HorizontalAlignment="Left" 
                   Width="20" Height="20" Margin="12,0"/>
            <TextBlock Text="Custom Title Bar" 
                       Grid.Column="1" 
                       Style="{StaticResource CaptionTextBlockStyle}" 
                       Margin="44,8,0,0"/>
        </Grid>

        <!-- This Button has a higher z-order than MyTitleBar, 
             so it receives user input. -->
        <Button x:Name="TitleBarButton" Content="Button in the title bar"
                HorizontalAlignment="Right"/>
    </Grid>
</Page>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    // Hide default title bar.
    var coreTitleBar = CoreApplication.GetCurrentView().TitleBar;
    coreTitleBar.ExtendViewIntoTitleBar = true;
    UpdateTitleBarLayout(coreTitleBar);

    // Set XAML element as a draggable region.
    Window.Current.SetTitleBar(AppTitleBar);

    // Register a handler for when the size of the overlaid caption control changes.
    // For example, when the app moves to a screen with a different DPI.
    coreTitleBar.LayoutMetricsChanged += CoreTitleBar_LayoutMetricsChanged;

    // Register a handler for when the title bar visibility changes.
    // For example, when the title bar is invoked in full screen mode.
    coreTitleBar.IsVisibleChanged += CoreTitleBar_IsVisibleChanged;
}

private void CoreTitleBar_LayoutMetricsChanged(CoreApplicationViewTitleBar sender, object args)
{
    UpdateTitleBarLayout(sender);
}

private void UpdateTitleBarLayout(CoreApplicationViewTitleBar coreTitleBar)
{
    // Get the size of the caption controls area and back button 
    // (returned in logical pixels), and move your content around as necessary.
    LeftPaddingColumn.Width = new GridLength(coreTitleBar.SystemOverlayLeftInset);
    RightPaddingColumn.Width = new GridLength(coreTitleBar.SystemOverlayRightInset);
    TitleBarButton.Margin = new Thickness(0,0,coreTitleBar.SystemOverlayRightInset,0);

    // Update title bar control size as needed to account for system size changes.
    AppTitleBar.Height = coreTitleBar.Height;
}

private void CoreTitleBar_IsVisibleChanged(CoreApplicationViewTitleBar sender, object args)
{
    if (sender.IsVisible)
    {
        AppTitleBar.Visibility = Visibility.Visible;
    }
    else
    {
        AppTitleBar.Visibility = Visibility.Collapsed;
    }
}
```

## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées

- Rendez-vous évident lorsque votre fenêtre est active ou inactive. Au minimum, modifiez la couleur du texte, des icônes et des boutons de votre barre de titre.
- Définissez une région Glissable le long du bord supérieur de la zone de dessin de l’application. La correspondance entre le positionnement des barres de titre du système facilite la recherche des utilisateurs.
- Définissez une région pouvant faire l’élément de glissement qui correspond à la barre de titre visuelle (le cas échéant) sur la zone de dessin de l’application.

## <a name="related-articles"></a>Articles connexes

- [Acrylique](../style/acrylic.md)
- [Couleur](../style/color.md)