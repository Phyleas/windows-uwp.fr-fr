---
Description: Créez des applications Windows qui prennent en charge des interactions personnalisées à partir d’appareils Pen et Stylus, y compris l’encre numérique pour les expériences d’écriture et de dessin naturelles.
title: Interactions avec le stylet et Windows Ink dans les applications Windows
ms.assetid: 3DA4F2D2-5405-42A1-9ED9-3A87BCD84C43
label: Pen interactions and Windows Ink in Windows apps
template: detail.hbs
keywords: Windows Ink, entrée manuscrite Windows, DirectInk, InkPresenter, InkCanvas, reconnaissance de l’écriture manuscrite, interaction utilisateur, entrées
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 34e41ff4b6fa402e8a1857a2ea406c9e63e7c868
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91216962"
---
# <a name="pen-interactions-and-windows-ink-in-windows-apps"></a>Interactions avec le stylet et Windows Ink dans les applications Windows

![Stylet Surface](images/ink/hero-small.png)  
*Stylet Surface* (disponible à l’achat dans la [Boutique Microsoft](https://www.microsoft.com/p/surface-pen/8zl5c82qmg6b)).

## <a name="overview"></a>Vue d’ensemble

Optimisez votre application Windows pour l’entrée de stylet afin de fournir des fonctionnalités de [**périphérique à pointeurs**](/uwp/api/Windows.Devices.Input.PointerDevice) standard et la meilleure expérience Windows Ink pour vos utilisateurs.

> [!NOTE]
> Cette rubrique est dédiée à la plateforme Windows Ink. Pour en savoir plus sur la gestion des entrées au pointeur (similaires aux fonctionnalités tactiles, de la souris et du pavé tactile), consultez [Gérer les entrées du pointeur](handle-pointer-input.md).

| Vidéos |   |
| --- | --- |
| <iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Ink-in-Your-UWP-App/player" width="300" height="200" allowFullScreen frameBorder="0"></iframe> | <iframe src="https://channel9.msdn.com/Events/Ignite/2016/BRK2060/player" width="300" height="200" allowFullScreen frameBorder="0"></iframe> |
| *Utilisation de l’encre dans votre application Windows* | *Utiliser le stylet et l’entrée manuscrite Windows pour créer des applications d’entreprise plus conviviales* |

La plateforme Windows Ink, associée à un stylet, permet de créer des notes manuscrites, des dessins et des annotations plus naturellement. La plateforme prend en charge la capture d’entrée du numériseur sous forme de données d’entrée manuscrite, la génération et la gestion de données d’entrée manuscrite, la restitution de ces données sous forme de traits et la conversion de l’encre en texte via la reconnaissance d’écriture manuscrite.

En plus de capturer la position et les mouvements de base du stylet lorsque l’utilisateur écrit ou dessine, votre application peut également effectuer le suivi des niveaux de pression variables utilisés pour un trait et en effectuer le suivi. Ces informations, ainsi que les paramètres relatifs à la forme de la pointe, à sa taille et à sa rotation, à la couleur de l’encre et à l’utilisation (entrée manuscrite normale, effacement, surlignage et sélection), vous permettent de proposer des expériences utilisateur ressemblant étroitement à l’écriture ou au dessin sur papier à l’aide d’un stylo, d’un crayon ou d’un pinceau.

> [!NOTE]
> Votre application prend également en charge l’entrée manuscrite à partir d’autres appareils de pointage, comme les numériseurs tactiles et les souris. 

La plateforme d’entrée manuscrite est très flexible. Elle est conçue pour prendre en charge différents niveaux de fonctionnalité, en fonction de vos besoins.

Pour obtenir des recommandations en matière d’expérience utilisateur avec Windows Ink, consultez [Contrôles pour l’entrée manuscrite](../controls-and-patterns/inking-controls.md).

## <a name="components-of-the-windows-ink-platform"></a>Composants de la plateforme Windows Ink

| Composant | Description |
| --- | --- |
| [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) | Un contrôle de plateforme d’interface utilisateur XAML, qui reçoit et affiche par défaut toutes les entrées à partir d’un stylet comme un trait d’encre ou un trait d’effacement.<br/>Pour plus d’informations sur l’utilisation de l’élément InkCanvas, consultez [Reconnaître les traits d’encre Windows en tant que texte](convert-ink-to-text.md) et [Stocker et récupérer les données de traits Windows Ink](save-and-load-ink.md). |
| [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) | Objet code-behind, instancié avec un contrôle [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) (exposé via la propriété [**InkCanvas. InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) ). Cet objet fournit toutes les fonctionnalités d’entrée manuscrite par défaut exposées par l’élément **InkCanvas**, ainsi qu’un ensemble complet d’API pour plus de personnalisation.<br/>Pour plus d’informations sur l’utilisation de l’élément InkPresenter, consultez [Reconnaître les traits d’encre Windows en tant que texte](convert-ink-to-text.md) et [Stocker et récupérer les données de traits Windows Ink](save-and-load-ink.md). |
| [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) | Contrôle de plateforme d’interface utilisateur XAML contenant une collection personnalisable et extensible de boutons qui activent les fonctionnalités liées à l’encre dans un [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas)associé.<br/>Pour plus d’informations sur l’utilisation de InkToolbar, consultez [Ajouter un InkToolbar à une application d’écriture manuscrite Windows](ink-toolbar.md). |
| [**IInkD2DRenderer**](/windows/desktop/api/inkrenderer/nn-inkrenderer-iinkd2drenderer) | Active le rendu des traits d’encre dans le contexte de périphérique Direct2D désigné d’une application Windows universelle, au lieu du contrôle [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) par défaut. Cela offre une personnalisation totale de l’expérience d’entrée manuscrite.<br/>Pour plus d’informations, consultez [l’exemple d’entrée manuscrite complexe](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk). |

## <a name="basic-inking-with-inkcanvas"></a>Entrée manuscrite de base avec InkCanvas

Pour ajouter la fonctionnalité d’encrage de base, il vous suffit de placer un contrôle de plateforme UWP [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) sur la page appropriée de votre application.

Par défaut, l’élément [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) prend en charge l’entrée manuscrite uniquement à partir d’un stylet. L’entrée est restituée sous la forme d’un trait d’encre à l’aide des paramètres par défaut pour la couleur et l’épaisseur (un stylo à bille noir avec une épaisseur de 2 pixels), ou traitée sous forme d’un effaceur de trait (lorsque l’entrée manuscrite est effectuée à partir d’une pointe d’effaceur ou que la pointe du stylet est modifiée à l’aide d’un bouton d’effacement).

> [!NOTE]
> Si une pointe d’effaceur ou le bouton n’est pas présent, l’élément InkCanvas peut être configuré pour traiter les entrées à partir de la pointe du stylet comme un trait d’effacement.

Dans cet exemple, un [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) recouvre une image d’arrière-plan.

> [!NOTE]
> Un InkCanvas a des propriétés de [**hauteur**](/uwp/api/windows.ui.xaml.frameworkelement.Height) et de [**largeur**](/uwp/api/windows.ui.xaml.frameworkelement.Width) par défaut de zéro, sauf s’il s’agit de l’enfant d’un élément qui redimensionne automatiquement ses éléments enfants, tels que les contrôles [StackPanel](/uwp/api/windows.ui.xaml.controls.stackpanel) ou de [grille](/uwp/api/windows.ui.xaml.controls.grid) .

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header"
                   Text="Basic ink sample"
                   Style="{ThemeResource HeaderTextBlockStyle}"
                   Margin="10,0,0,0" />            
    </StackPanel>
    <Grid Grid.Row="1">
        <Image Source="Assets\StoreLogo.png" />
        <InkCanvas x:Name="inkCanvas" />
    </Grid>
</Grid>
```

Cette série d’images montre comment une entrée manuscrite de stylet est restituée par ce contrôle [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas).

| ![InkCanvas vide avec une image d’arrière-plan](images/ink_basic_1_small.png) | ![InkCanvas avec des traits d’encre](images/ink_basic_2_small.png) | ![InkCanvas avec un trait effacé](images/ink_basic_3_small.png) |
| --- | --- | ---|
| [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) vide avec une image d’arrière-plan. | [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) avec des traits d’encre. | [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) avec un trait effacé (notez comment fonctionne l’effacement sur un trait complet et non seulement sur une partie). |

La fonctionnalité d’entrée manuscrite prise en charge par le contrôle [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) est proposée par un objet code-behind appelé [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter).

Pour une entrée manuscrite de base, vous n’avez pas à vous soucier de l' [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter). Toutefois, pour personnaliser et configurer le comportement de l’entrée manuscrite sur l’élément [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas), vous devez accéder à son objet **InkPresenter** correspondant.

## <a name="basic-customization-with-inkpresenter"></a>Personnalisation de base avec InkPresenter

Un objet [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) est instancié avec chaque contrôle [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas).

> [!NOTE]
> L’élément [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) ne peut pas être instancié directement. Au lieu de cela, elle est accessible via la propriété [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) de l' [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas). 

En plus de fournir tous les comportements d’encrage par défaut de son contrôle [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) correspondant, l' [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) fournit un ensemble complet d’API pour une personnalisation de trait supplémentaire et une gestion plus fine de l’entrée de stylet (standard et modifié). Cela comprend les propriétés de trait, les types de périphériques d’entrée pris en charge et si l’entrée est traitée par l’objet ou passée à l’application pour traitement.

> [!NOTE]
> L’entrée d’encre standard (à partir de l’info-bulle ou du bouton de l’info-bulle du stylet) n’est pas modifiée avec une offre matérielle secondaire, comme un bouton du stylet, un bouton droit de la souris ou un mécanisme similaire. 

Par défaut, l’encre est prise en charge uniquement pour l’entrée de stylet. Ici, nous configurons l' [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) pour interpréter les données d’entrée du stylet et de la souris comme des traits d’encre. Nous définissons également des attributs de trait d’encre initiaux utilisés pour restituer les traits dans l’élément [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas).

Pour activer la souris et l’entrée tactile, définissez la propriété [**InputDeviceTypes**](/uwp/api/windows.ui.input.inking.inkpresenter.InputDeviceTypes) de l' [**InkPresenter**](/uwp/api/windows.ui.input.inking.inkpresenter) sur la combinaison des valeurs de [**CoreInputDeviceTypes**](/uwp/api/windows.ui.core.coreinputdevicetypes) que vous souhaitez.

```csharp
public MainPage()
{
    this.InitializeComponent();

    // Set supported inking device types.
    inkCanvas.InkPresenter.InputDeviceTypes =
        Windows.UI.Core.CoreInputDeviceTypes.Mouse |
        Windows.UI.Core.CoreInputDeviceTypes.Pen;

    // Set initial ink stroke attributes.
    InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
    drawingAttributes.Color = Windows.UI.Colors.Black;
    drawingAttributes.IgnorePressure = false;
    drawingAttributes.FitToCurve = true;
    inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);
}
```

Les attributs de traits d’encre peuvent être définis de manière dynamique pour s’adapter aux préférences de l’utilisateur ou aux exigences de l’application.

Ici, nous laissons un utilisateur choisir parmi une liste de couleurs d’encre.

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header"
                   Text="Basic ink customization sample"
                   VerticalAlignment="Center"
                   Style="{ThemeResource HeaderTextBlockStyle}"
                   Margin="10,0,0,0" />
        <TextBlock Text="Color:"
                   Style="{StaticResource SubheaderTextBlockStyle}"
                   VerticalAlignment="Center"
                   Margin="50,0,10,0"/>
        <ComboBox x:Name="PenColor"
                  VerticalAlignment="Center"
                  SelectedIndex="0"
                  SelectionChanged="OnPenColorChanged">
            <ComboBoxItem Content="Black"/>
            <ComboBoxItem Content="Red"/>
        </ComboBox>
    </StackPanel>
    <Grid Grid.Row="1">
        <Image Source="Assets\StoreLogo.png" />
        <InkCanvas x:Name="inkCanvas" />
    </Grid>
</Grid>
```

Nous traitons ensuite les modifications de manière à tenir compte de la couleur sélectionnée et actualisons les attributs de traits d’encre en conséquence.

```csharp
// Update ink stroke color for new strokes.
private void OnPenColorChanged(object sender, SelectionChangedEventArgs e)
{
    if (inkCanvas != null)
    {
        InkDrawingAttributes drawingAttributes =
            inkCanvas.InkPresenter.CopyDefaultDrawingAttributes();

        string value = ((ComboBoxItem)PenColor.SelectedItem).Content.ToString();

        switch (value)
        {
            case "Black":
                drawingAttributes.Color = Windows.UI.Colors.Black;
                break;
            case "Red":
                drawingAttributes.Color = Windows.UI.Colors.Red;
                break;
            default:
                drawingAttributes.Color = Windows.UI.Colors.Black;
                break;
        };

        inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);
    }
}
```

Ces images montrent comment l’élément [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) traite et personnalise l’entrée de stylet.

| ![InkCanvas avec des traits d’encre noire par défaut](images/ink-basic-custom-1-small.png) | ![InkCanvas avec des traits d’encre rouge sélectionnés par l’utilisateur.](images/ink-basic-custom-2-small.png) |
| --- | --- |
| [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) avec traits d’encre noire par défaut. | [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) avec les traits d’encre rouge sélectionnés par l’utilisateur. | 

Pour exploiter des fonctionnalités dépassant l’entrée manuscrite et l’effacement, telles que la sélection de trait, votre application doit identifier une entrée spécifique afin que l’élément [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) la transmette directement non traitée afin d’être gérée par votre application.

## <a name="pass-through-input-for-advanced-processing"></a>Entrée directe pour traitement avancé

Par défaut, [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) traite toutes les entrées comme un trait d’encre ou un trait d’effacement, y compris l’entrée modifiée par une offre matérielle secondaire, telle qu’un bouton de stylet, un bouton droit de la souris ou similaire. Toutefois, les utilisateurs attendent généralement des fonctionnalités supplémentaires ou un comportement modifié avec ces intuitivité secondaires.

Dans certains cas, vous devrez peut-être également exposer des fonctionnalités supplémentaires pour les stylets sans intuitivité secondaire (fonctionnalités généralement associées à la pointe du stylet), d’autres types de périphériques d’entrée ou un type de comportement modifié en fonction d’une sélection de l’utilisateur dans l’interface utilisateur de votre application.

Pour prendre cela en charge, [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) peut être configuré pour ne pas traiter une entrée spécifique. Cette entrée non traitée est ensuite directement transmise à votre application pour traitement.

### <a name="example---use-unprocessed-input-to-implement-stroke-selection"></a>Exemple-utiliser une entrée non traité pour implémenter la sélection de traits 

La plateforme Windows Ink ne fournit pas de prise en charge intégrée pour les actions qui requièrent des entrées modifiées, telles que la sélection de traits. Pour prendre en charge des fonctionnalités de ce type, vous devez fournir une solution personnalisée dans vos applications. 

L’exemple de code suivant (tout le code se trouve dans les fichiers MainPage. xaml et MainPage.xaml.cs) explique comment activer la sélection de traits lorsque l’entrée est modifiée à l’aide d’un bouton de stylet (ou du bouton droit de la souris).

1.  Tout d’abord, nous configurons l’interface utilisateur dans MainPage.xaml.

    Ici, nous ajoutons une zone de dessin (sous l' [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)) pour dessiner le trait de sélection. Une couche distincte permet de dessiner le trait de sélection sans modifier l’élément **InkCanvas** ni son contenu.

    ![InkCanvas vide avec un canevas de sélection sous-jacent](images/ink-unprocessed-1-small.png)

      ```xaml
        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
          <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
          </Grid.RowDefinitions>
          <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
            <TextBlock x:Name="Header"
              Text="Advanced ink customization sample"
              VerticalAlignment="Center"
              Style="{ThemeResource HeaderTextBlockStyle}"
              Margin="10,0,0,0" />
          </StackPanel>
          <Grid Grid.Row="1">
            <!-- Canvas for displaying selection UI. -->
            <Canvas x:Name="selectionCanvas"/>
            <!-- Inking area -->
            <InkCanvas x:Name="inkCanvas"/>
          </Grid>
        </Grid>
      ```

2.  Dans MainPage.xaml.cs, nous déclarons quelques variables globales pour conserver des références à des aspects de l’interface utilisateur de sélection. Plus précisément, le trait du lasso de sélection et le rectangle englobant qui met en surbrillance les traits sélectionnés.

      ```csharp
        // Stroke selection tool.
        private Polyline lasso;
        // Stroke selection area.
        private Rect boundingRect;
      ```

3.  Ensuite, nous configurerons l' [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) pour interpréter les données d’entrée à la fois à partir du stylet et de la souris comme traits d’encre, puis vous définirez des attributs de trait d’encre initiale utilisés pour le rendu des traits dans le [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas).

    Plus important encore, nous utilisons la propriété [**InputProcessingConfiguration**](/uwp/api/windows.ui.input.inking.inkpresenter.inputprocessingconfiguration) de l’élément [InkPresenter](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) pour indiquer que toute entrée modifiée doit être traitée par l’application. La modification d’une entrée est spécifiée en attribuant à **InputProcessingConfiguration.RightDragAction** la valeur [**InkInputRightDragAction.LeaveUnprocessed**](/uwp/api/Windows.UI.Input.Inking.InkInputRightDragAction). Lorsque cette valeur est définie, l' [InkPresenter](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) passe à la classe [InkUnprocessedInput](/uwp/api/windows.ui.input.inking.inkunprocessedinput) , un ensemble d’événements de pointeur que vous pouvez gérer.

    Nous attribuons des écouteurs pour les événements [**PointerPressed**](/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointerpressed), [**PointerMoved**](/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointermoved)et [**PointerReleased**](/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointerreleased) non traités passés par l' [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter). Toutes les fonctionnalités de sélection sont implémentées dans les gestionnaires de ces événements.

    Enfin, nous attribuons des écouteurs pour les événements [**StrokeStarted**](/uwp/api/windows.ui.input.inking.inkstrokeinput.strokestarted) et [**StrokesErased**](/uwp/api/windows.ui.input.inking.inkpresenter.strokeserased) de l' [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter). Nous utilisons les gestionnaires de ces événements pour nettoyer l’interface utilisateur de sélection si un nouveau trait est commencé ou un trait existant effacé.

    ![InkCanvas avec des traits d’encre noire par défaut](images/ink-unprocessed-2-small.png)

      ```csharp
        public MainPage()
        {
          this.InitializeComponent();

          // Set supported inking device types.
          inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

          // Set initial ink stroke attributes.
          InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
          drawingAttributes.Color = Windows.UI.Colors.Black;
          drawingAttributes.IgnorePressure = false;
          drawingAttributes.FitToCurve = true;
          inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);

          // By default, the InkPresenter processes input modified by
          // a secondary affordance (pen barrel button, right mouse
          // button, or similar) as ink.
          // To pass through modified input to the app for custom processing
          // on the app UI thread instead of the background ink thread, set
          // InputProcessingConfiguration.RightDragAction to LeaveUnprocessed.
          inkCanvas.InkPresenter.InputProcessingConfiguration.RightDragAction =
              InkInputRightDragAction.LeaveUnprocessed;

          // Listen for unprocessed pointer events from modified input.
          // The input is used to provide selection functionality.
          inkCanvas.InkPresenter.UnprocessedInput.PointerPressed +=
              UnprocessedInput_PointerPressed;
          inkCanvas.InkPresenter.UnprocessedInput.PointerMoved +=
              UnprocessedInput_PointerMoved;
          inkCanvas.InkPresenter.UnprocessedInput.PointerReleased +=
              UnprocessedInput_PointerReleased;

          // Listen for new ink or erase strokes to clean up selection UI.
          inkCanvas.InkPresenter.StrokeInput.StrokeStarted +=
              StrokeInput_StrokeStarted;
          inkCanvas.InkPresenter.StrokesErased +=
              InkPresenter_StrokesErased;
        }
      ```

4.  Nous définissons ensuite des gestionnaires pour les événements [**PointerPressed**](/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointerpressed), [**PointerMoved**](/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointermoved), et [**PointerReleased**](/uwp/api/windows.ui.input.inking.inkunprocessedinput.pointerreleased) non traités transmis directement par [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter).

    Toutes les fonctionnalités de sélection sont implémentées dans ces gestionnaires, y compris le trait de lasso et le rectangle englobant.

    ![Lasso de sélection](images/ink-unprocessed-3-small.png)

      ```csharp
        // Handle unprocessed pointer events from modified input.
        // The input is used to provide selection functionality.
        // Selection UI is drawn on a canvas under the InkCanvas.
        private void UnprocessedInput_PointerPressed(
          InkUnprocessedInput sender, PointerEventArgs args)
        {
          // Initialize a selection lasso.
          lasso = new Polyline()
          {
              Stroke = new SolidColorBrush(Windows.UI.Colors.Blue),
              StrokeThickness = 1,
              StrokeDashArray = new DoubleCollection() { 5, 2 },
              };

              lasso.Points.Add(args.CurrentPoint.RawPosition);

              selectionCanvas.Children.Add(lasso);
          }

          private void UnprocessedInput_PointerMoved(
            InkUnprocessedInput sender, PointerEventArgs args)
          {
            // Add a point to the lasso Polyline object.
            lasso.Points.Add(args.CurrentPoint.RawPosition);
          }

          private void UnprocessedInput_PointerReleased(
            InkUnprocessedInput sender, PointerEventArgs args)
          {
            // Add the final point to the Polyline object and
            // select strokes within the lasso area.
            // Draw a bounding box on the selection canvas
            // around the selected ink strokes.
            lasso.Points.Add(args.CurrentPoint.RawPosition);

            boundingRect =
              inkCanvas.InkPresenter.StrokeContainer.SelectWithPolyLine(
                lasso.Points);

            DrawBoundingRect();
          }
      ```

5.  Pour conclure le gestionnaire d’événements PointerReleased, nous effaçons tout le contenu de la couche de sélection (le trait du lasso), puis dessinons un rectangle englobant unique autour des traits d’encre englobés par la zone du lasso.

    ![Rectangle englobant de sélection](images/ink-unprocessed-4-small.png)

      ```csharp
        // Draw a bounding rectangle, on the selection canvas, encompassing
        // all ink strokes within the lasso area.
        private void DrawBoundingRect()
        {
          // Clear all existing content from the selection canvas.
          selectionCanvas.Children.Clear();

          // Draw a bounding rectangle only if there are ink strokes
          // within the lasso area.
          if (!((boundingRect.Width == 0) ||
            (boundingRect.Height == 0) ||
            boundingRect.IsEmpty))
            {
              var rectangle = new Rectangle()
              {
                Stroke = new SolidColorBrush(Windows.UI.Colors.Blue),
                  StrokeThickness = 1,
                  StrokeDashArray = new DoubleCollection() { 5, 2 },
                  Width = boundingRect.Width,
                  Height = boundingRect.Height
              };

              Canvas.SetLeft(rectangle, boundingRect.X);
              Canvas.SetTop(rectangle, boundingRect.Y);

              selectionCanvas.Children.Add(rectangle);
            }
          }
      ```

6.  Enfin, nous définissons des gestionnaires pour les événements InkPresenter [**StrokeStarted**](/uwp/api/windows.ui.input.inking.inkstrokeinput.strokestarted) et [**StrokesErased**](/uwp/api/windows.ui.input.inking.inkpresenter.strokeserased).

    Ces deux événements appellent simplement la même fonction de nettoyage pour effacer la sélection actuelle chaque fois qu’un nouveau trait est détecté.

      ```csharp
        // Handle new ink or erase strokes to clean up selection UI.
        private void StrokeInput_StrokeStarted(
          InkStrokeInput sender, Windows.UI.Core.PointerEventArgs args)
        {
          ClearSelection();
        }

        private void InkPresenter_StrokesErased(
          InkPresenter sender, InkStrokesErasedEventArgs args)
        {
          ClearSelection();
        }
      ```

7.  Voici la fonction permettant de supprimer l’ensemble de l’interface utilisateur de sélection du canevas de sélection au commencement d’un nouveau trait ou à l’effacement d’un trait existant.

      ```csharp
        // Clean up selection UI.
        private void ClearSelection()
        {
          var strokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
          foreach (var stroke in strokes)
          {
            stroke.Selected = false;
          }
          ClearDrawnBoundingRect();
         }

        private void ClearDrawnBoundingRect()
        {
          if (selectionCanvas.Children.Any())
          {
            selectionCanvas.Children.Clear();
            boundingRect = Rect.Empty;
          }
        }
      ```

## <a name="custom-ink-rendering"></a>Restitution d’une entrée manuscrite personnalisée

Par défaut, l’entrée d’encre est traitée sur un thread d’arrière-plan à faible latence et rendue en cours, ou « humide », car elle est dessinée. Lorsque le trait est terminé (stylet ou doigt levé, ou bouton de la souris relâché), le trait est traité sur le thread d’interface utilisateur et rendu « secs » à la couche [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) (au-dessus du contenu de l’application et en remplaçant l’encre humide).

Vous pouvez remplacer ce comportement par défaut et contrôler complètement l’expérience d’entrée manuscrite en « séchage personnalisé » des traits d’encre humide. Si le comportement par défaut est généralement suffisant pour la plupart des applications, il existe quelques cas où un séchage personnalisé peut être nécessaire, notamment :
- Gestion plus efficace des collections volumineuses ou complexes de traits d’encre
- Prise en charge des panoramiques et des zooms plus efficaces sur les grands canevas d’encre
- Entrelacement de l’encre et d’autres objets, tels que des formes ou du texte, tout en conservant l’ordre de plan 
- Séchage et conversion de l’encre de manière synchrone en une forme DirectX (par exemple, une ligne droite ou une forme pixellisée et intégrée au contenu de l’application plutôt que sous forme de couche **InkCanvas** distincte).

Le séchage personnalisé requiert un objet [**IInkD2DRenderer**](/windows/desktop/api/inkrenderer/nn-inkrenderer-iinkd2drenderer) pour gérer l’entrée d’encre et la rendre dans le contexte de périphérique Direct2D de votre application Windows universelle, au lieu du contrôle [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) par défaut.

En appelant [**ActivateCustomDrying**](/uwp/api/windows.ui.input.inking.inkpresenter.activatecustomdrying) (avant le chargement de [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) ), une application crée un objet [**InkSynchronizer**](/uwp/api/Windows.UI.Input.Inking.InkSynchronizer) pour personnaliser la façon dont un trait d’encre est rendu sec à un [**SurfaceImageSource**](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) ou [**VirtualSurfaceImageSource**](/uwp/api/windows.ui.xaml.media.imaging.virtualsurfaceimagesource). 

[**SurfaceImageSource**](/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource) et [**VirtualSurfaceImageSource**](/uwp/api/windows.ui.xaml.media.imaging.virtualsurfaceimagesource) fournissent une surface partagée DirectX pour que votre application les dessine et les compose dans le contenu de votre application, bien que outre fournisse une surface virtuelle plus grande que l’écran pour des opérations de panoramique et de zoom performantes. Étant donné que les mises à jour visuelles de ces surfaces sont synchronisées avec le thread d’interface utilisateur XAML, lorsque l’encre est restituée à l’un ou l’autre, l’encre humide peut être supprimée de l’InkCanvas simultanément. 

Vous pouvez également personnaliser l’encre sèche dans un [SwapChainPanel](/uwp/api/windows.ui.xaml.controls.swapchainpanel), mais la synchronisation avec le thread d’interface utilisateur n’est pas garantie et il peut y avoir un délai entre le moment où l’encre est rendue dans votre SwapChainPanel et le moment où l’encre est supprimée de l’InkCanvas.

Pour obtenir un exemple complet de cette fonctionnalité, consultez [l’exemple d’entrée manuscrite complexe](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk).

> [!NOTE]
> Séchage personnalisé et [ **InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar)  
> Si votre application remplace le comportement par défaut du rendu d’entrée manuscrite de l’élément [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) par une implémentation de séchage personnalisé, les traits d’encre restitués ne sont plus disponibles pour l’élément InkToolbar et les commandes d’effacement intégrées de l’élément InkToolbar ne fonctionneront pas comme prévu. Pour fournir des fonctionnalités d’effacement, vous devez gérer tous les événements de pointeur, effectuer le test de positionnement sur chaque trait et remplacer la commande intégrée « Effacer toutes les entrées manuscrites ».

## <a name="other-articles-in-this-section"></a>Autres articles de cette section

| Rubrique | Description |
| --- | --- |
| [Reconnaître les traits d’encre](convert-ink-to-text.md) | Convertissez des traits d’encre en texte à l’aide de la reconnaissance de l’écriture manuscrite ou en formes à l’aide de la reconnaissance personnalisée. |
| [Stocker et récupérer des traits d’encre](save-and-load-ink.md) | Stockez des données de traits d’encre dans un fichier GIF (Graphics Interchange Format) à l’aide des métadonnées intégrées ISF (Ink Serialized Format). |
| [Ajouter un InkToolbar à une application Windows inking](ink-toolbar.md) | Ajoutez un InkToolbar par défaut à une application d’écriture manuscrite Windows, ajoutez un bouton stylet personnalisé au InkToolbar et liez le bouton stylet personnalisé à une définition de stylet personnalisée. |

## <a name="related-articles"></a>Articles connexes

- [Prise en main : écriture manuscrite dans votre application Windows](./ink-walkthrough.md)
- [Gestion des entrées du pointeur](handle-pointer-input.md)
- [Identification des périphériques d’entrée](identify-input-devices.md)

### <a name="apis"></a>API

- [**Windows.Devices.Input**](/uwp/api/Windows.Devices.Input)
- [**Windows.UI.Input.Inking**](/uwp/api/Windows.UI.Input.Inking)
- [**Windows. UI. Input. encrage. Core**](/uwp/api/Windows.UI.Input.Inking.Core)

### <a name="samples"></a>Exemples

- [Didacticiel de prise en main : écriture manuscrite dans votre application Windows](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-Ink)
- [Exemple d’encre simple (C#/C + +)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)
- [Exemple d’encre complexe (C++)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)
- [Ink, exemple (JavaScript)](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BJavaScript%5D-Windows%208%20app%20samples/JavaScript/Windows%208%20app%20samples/Input%20Ink%20sample%20(Windows%208))
- [Exemple de livre de coloriage](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [Exemple de notes de famille](https://github.com/Microsoft/Windows-appsample-familynotes)
- [Exemple d’entrée de base](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Exemple d’entrée à faible latence](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Exemple de mode d’interaction utilisateur](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Exemples de visuels de focus](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>Exemples d’archive

- [Entrée : exemple de fonctionnalités de périphériques](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [Entrée : exemple d’événements d’entrée utilisateur XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [Exemple de défilement XAML, panoramique et zoom](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [Entrée : Mouvements et manipulations avec GestureRecognizer](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
