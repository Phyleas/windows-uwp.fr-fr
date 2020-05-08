---
Description: Les applications Windows qui prennent en charge l’encre Windows peuvent sérialiser et désérialiser des traits d’encre dans un fichier ISF (Ink Serialized Format). Le fichier ISF est une image GIF contenant des métadonnées supplémentaires pour tous les comportements et propriétés de traits d’encre. Les applications qui ne sont pas compatibles avec les entrées manuscrites peuvent afficher l’image GIF statique, y compris la transparence d’arrière-plan de canal alpha.
title: Stocker et récupérer les données de traits Windows Ink
ms.assetid: C96C9D2F-DB69-4883-9809-4A0DF7CEC506
label: Store and retrieve Windows Ink stroke data
template: detail.hbs
keywords: Windows Ink, Windows encrage, DirectInk, InkPresenter, InkCanvas, ISF, Ink Serialized Format, interaction utilisateur, entrée
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 819358fb775444d62cbad414668a779fc5c305ca
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970244"
---
# <a name="store-and-retrieve-windows-ink-stroke-data"></a>Stocker et récupérer les données de traits Windows Ink


Les applications Windows qui prennent en charge l’encre Windows peuvent sérialiser et désérialiser des traits d’encre dans un fichier ISF (Ink Serialized Format). Le fichier ISF est une image GIF contenant des métadonnées supplémentaires pour tous les comportements et propriétés de traits d’encre. Les applications qui ne sont pas compatibles avec les entrées manuscrites peuvent afficher l’image GIF statique, y compris la transparence d’arrière-plan de canal alpha.

> [!NOTE]
> ISF est la représentation persistante la plus compacte de l’entrée manuscrite. Vous pouvez l’intégrer dans un format de document binaire, tel qu’un fichier GIF, ou placer le fichier directement dans le Presse-papiers.

> **API importantes**: [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas), [**Windows. UI. Input. encrage**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking)

## <a name="save-ink-strokes-to-a-file"></a>Enregistrer des traits d’encre dans un fichier

Ici, nous montrons comment enregistrer les traits d’encre dessinés sur un contrôle [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) .

**Téléchargez cet exemple à partir de la [zone Enregistrer et charger des traits d’encre à partir d’un fichier ISF (Ink Serialized Format)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store.zip)**

1.  Tout d’abord, nous configurons l’interface utilisateur.

    L’interface utilisateur comprend les boutons Enregistrer, Charger et Effacer, ainsi que le contrôle [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas).
```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
            <TextBlock x:Name="Header" 
                       Text="Basic ink store sample" 
                       Style="{ThemeResource HeaderTextBlockStyle}" 
                       Margin="10,0,0,0" />
            <Button x:Name="btnSave" 
                    Content="Save" 
                    Margin="50,0,10,0"/>
            <Button x:Name="btnLoad" 
                    Content="Load" 
                    Margin="50,0,10,0"/>
            <Button x:Name="btnClear" 
                    Content="Clear" 
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid Grid.Row="1">
            <InkCanvas x:Name="inkCanvas" />
        </Grid>
    </Grid>
```

2.  Nous définissons ensuite certains comportements d’entrée manuscrite de base.

    Le contrôle [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) est configuré pour interpréter les données d’entrée du stylet et de la souris sous forme de traits d’encre ([**InputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.inputdevicetypes)), et des écouteurs pour les événements Click sur les boutons sont déclarés.
```csharp
public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Listen for button click to initiate save.
        btnSave.Click += btnSave_Click;
        // Listen for button click to initiate load.
        btnLoad.Click += btnLoad_Click;
        // Listen for button click to clear ink canvas.
        btnClear.Click += btnClear_Click;
    }
```

3.  Enfin, nous enregistrons l’entrée manuscrite dans le gestionnaire d’événements Click du bouton **Enregistrer**.

    Une classe [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker) permet à l’utilisateur de sélectionner le fichier et l’emplacement où les données d’entrée manuscrite sont enregistrées.

    Une fois qu’un fichier est sélectionné, nous allons ouvrir un flux [**IRandomAccessStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.IRandomAccessStream) défini sur [**ReadWrite**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileAccessMode).

    Nous appelons ensuite [**SaveAsync**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.iinkstrokecontainer.saveasync) pour sérialiser les traits d’encre gérés par la classe [**InkStrokeContainer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) dans le flux.

```csharp
// Save ink data to a file.
    private async void btnSave_Click(object sender, RoutedEventArgs e)
    {
        // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();

        // Strokes present on ink canvas.
        if (currentStrokes.Count > 0)
        {
            // Let users choose their ink file using a file picker.
            // Initialize the picker.
            Windows.Storage.Pickers.FileSavePicker savePicker = 
                new Windows.Storage.Pickers.FileSavePicker();
            savePicker.SuggestedStartLocation = 
                Windows.Storage.Pickers.PickerLocationId.DocumentsLibrary;
            savePicker.FileTypeChoices.Add(
                "GIF with embedded ISF", 
                new List<string>() { ".gif" });
            savePicker.DefaultFileExtension = ".gif";
            savePicker.SuggestedFileName = "InkSample";

            // Show the file picker.
            Windows.Storage.StorageFile file = 
                await savePicker.PickSaveFileAsync();
            // When chosen, picker returns a reference to the selected file.
            if (file != null)
            {
                // Prevent updates to the file until updates are 
                // finalized with call to CompleteUpdatesAsync.
                Windows.Storage.CachedFileManager.DeferUpdates(file);
                // Open a file stream for writing.
                IRandomAccessStream stream = await file.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);
                // Write the ink strokes to the output stream.
                using (IOutputStream outputStream = stream.GetOutputStreamAt(0))
                {
                    await inkCanvas.InkPresenter.StrokeContainer.SaveAsync(outputStream);
                    await outputStream.FlushAsync();
                }
                stream.Dispose();

                // Finalize write so other apps can update file.
                Windows.Storage.Provider.FileUpdateStatus status =
                    await Windows.Storage.CachedFileManager.CompleteUpdatesAsync(file);

                if (status == Windows.Storage.Provider.FileUpdateStatus.Complete)
                {
                    // File saved.
                }
                else
                {
                    // File couldn't be saved.
                }
            }
            // User selects Cancel and picker returns null.
            else
            {
                // Operation cancelled.
            }
        }
    }
```

> [!NOTE]
> Le format de fichier GIF est le seul pris en charge pour l’enregistrement des données d’entrée manuscrite. Toutefois, la méthode [**LoadAsync**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkmanager.loadasync) (illustrée dans la section suivante) prend en charge des formats supplémentaires pour la compatibilité descendante.

## <a name="load-ink-strokes-from-a-file"></a>Charger des traits d’encre à partir d’un fichier

Ici, nous montrons comment charger des traits d’encre à partir d’un fichier et les restituer sur un contrôle [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas).

**Téléchargez cet exemple à partir de la [zone Enregistrer et charger des traits d’encre à partir d’un fichier ISF (Ink Serialized Format)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store.zip)**

1.  Tout d’abord, nous configurons l’interface utilisateur.

    L’interface utilisateur comprend les boutons Enregistrer, Charger et Effacer, ainsi que le contrôle [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas).
```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
            <TextBlock x:Name="Header" 
                       Text="Basic ink store sample" 
                       Style="{ThemeResource HeaderTextBlockStyle}" 
                       Margin="10,0,0,0" />
            <Button x:Name="btnSave" 
                    Content="Save" 
                    Margin="50,0,10,0"/>
            <Button x:Name="btnLoad" 
                    Content="Load" 
                    Margin="50,0,10,0"/>
            <Button x:Name="btnClear" 
                    Content="Clear" 
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid Grid.Row="1">
            <InkCanvas x:Name="inkCanvas" />
        </Grid>
    </Grid>
```

2.  Nous définissons ensuite certains comportements d’entrée manuscrite de base.

    Le contrôle [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) est configuré pour interpréter les données d’entrée du stylet et de la souris sous forme de traits d’encre ([**InputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.inputdevicetypes)), et des écouteurs pour les événements Click sur les boutons sont déclarés.
```csharp
public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Listen for button click to initiate save.
        btnSave.Click += btnSave_Click;
        // Listen for button click to initiate load.
        btnLoad.Click += btnLoad_Click;
        // Listen for button click to clear ink canvas.
        btnClear.Click += btnClear_Click;
    }
```

3.  Enfin, nous enregistrons l’entrée manuscrite dans le gestionnaire d’événements Click du bouton **Charger**.

    Une classe [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) permet à l’utilisateur de sélectionner le fichier et l’emplacement où récupérer les données d’entrée manuscrite enregistrées.

    Une fois qu’un fichier est sélectionné, nous allons ouvrir un flux [**IRandomAccessStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.IRandomAccessStream) défini sur [**lecture**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileAccessMode).

    Nous appelons ensuite [**LoadAsync**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkmanager.loadasync) pour lire, désérialiser et charger les traits d’encre enregistrés dans le [**InkStrokeContainer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer). Le chargement des traits dans l’élément **InkStrokeContainer** entraîne leur restitution immédiate dans [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) par l’élément [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas).

    > [!NOTE]
    > Tous les traits existants dans InkStrokeContainer sont effacés avant que de nouveaux traits soient chargés.

``` csharp
// Load ink data from a file.
private async void btnLoad_Click(object sender, RoutedEventArgs e)
{
    // Let users choose their ink file using a file picker.
    // Initialize the picker.
    Windows.Storage.Pickers.FileOpenPicker openPicker =
        new Windows.Storage.Pickers.FileOpenPicker();
    openPicker.SuggestedStartLocation =
        Windows.Storage.Pickers.PickerLocationId.DocumentsLibrary;
    openPicker.FileTypeFilter.Add(".gif");
    // Show the file picker.
    Windows.Storage.StorageFile file = await openPicker.PickSingleFileAsync();
    // User selects a file and picker returns a reference to the selected file.
    if (file != null)
    {
        // Open a file stream for reading.
        IRandomAccessStream stream = await file.OpenAsync(Windows.Storage.FileAccessMode.Read);
        // Read from file.
        using (var inputStream = stream.GetInputStreamAt(0))
        {
            await inkCanvas.InkPresenter.StrokeContainer.LoadAsync(inputStream);
        }
        stream.Dispose();
    }
    // User selects Cancel and picker returns null.
    else
    {
        // Operation cancelled.
    }
}
```

> [!NOTE]
> Le format de fichier GIF est le seul pris en charge pour l’enregistrement des données d’entrée manuscrite. Cependant, la méthode [**LoadAsync**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkmanager.loadasync) prend en charge les formats suivants pour la compatibilité descendante.

| Format                    | Description |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| InkSerializedFormat       | Indique les entrées manuscrites qui persistent avec le format ISF. Le format ISF est la représentation persistante la plus compacte de l’entrée manuscrite. Vous pouvez l’intégrer dans un format de document binaire ou le placer directement dans le Presse-papiers.                                                                                                                                                                                                         |
| Base64InkSerializedFormat | Indique les entrées manuscrites qui persistent avec le codage de l’ISF en tant que flux en base 64. Ce format permet de coder l’entrée manuscrite directement dans un fichier XML ou HTML.                                                                                                                                                                                                                                                |
| GIF                       | Indique les entrées manuscrites qui persistent avec l’utilisation d’un fichier GIF dans lequel la représentation ISF est considérée en tant que métadonnées incorporées au fichier. Ce format permet d’afficher les entrées manuscrites dans des applications qui ne sont pas compatibles avec l’entrée manuscrite, tout en conservant une fidélité totale pour celles qui sont compatibles. Il est idéal pour transporter du contenu d’entrée manuscrite au sein d’un fichier HTML et pour le rendre utilisable par des applications compatibles et non compatibles avec l’entrée manuscrite. |
| Base64Gif                 | Indique les entrées manuscrites qui persistent en utilisant un format GIF renforcé codé en base 64. Ce format permet de coder directement les entrées manuscrites dans un fichier XML ou HTML afin de les convertir ultérieurement en images. Imaginons, par exemple, un format XML créé pour contenir toutes les informations sur les entrées manuscrites et utilisé comme moyen pour générer du HTML via le langage XSLT (Extensible Stylesheet Language Transformations). 

## <a name="copy-and-paste-ink-strokes-with-the-clipboard"></a>Copier et coller des traits d’encre avec le Presse-papiers

Ici, nous montrons comment utiliser le Presse-papiers pour transférer des traits d’encre entre applications.

Pour prendre en charge les fonctionnalités du presse-papiers, les commandes [**InkStrokeContainer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) couper et copier intégrées nécessitent la sélection d’un ou de plusieurs traits d’encre.

Dans cet exemple, nous activons la sélection de traits lorsque l’entrée est modifiée avec un bouton de stylet (ou le bouton droit de la souris). Pour obtenir un exemple complet de sélection de traits d’encre, consultez Entrée directe pour traitement avancé dans [Interactions avec le stylo ou le stylet](pen-and-stylus-interactions.md).

**Téléchargez cet exemple à partir du [presse-papiers pour enregistrer et charger des traits d’encre](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store-clipboard.zip)**

1.  Tout d’abord, nous configurons l’interface utilisateur.

    L’interface utilisateur comprend les boutons Couper, Copier, Coller et Effacer, ainsi que l’élément [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) et un canevas de sélection.
```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
            <TextBlock x:Name="tbHeader" 
                       Text="Basic ink store sample" 
                       Style="{ThemeResource HeaderTextBlockStyle}" 
                       Margin="10,0,0,0" />
            <Button x:Name="btnCut" 
                    Content="Cut" 
                    Margin="20,0,10,0"/>
            <Button x:Name="btnCopy" 
                    Content="Copy" 
                    Margin="20,0,10,0"/>
            <Button x:Name="btnPaste" 
                    Content="Paste" 
                    Margin="20,0,10,0"/>
            <Button x:Name="btnClear" 
                    Content="Clear" 
                    Margin="20,0,10,0"/>
        </StackPanel>
        <Grid x:Name="gridCanvas" Grid.Row="1">
            <!-- Canvas for displaying selection UI. -->
            <Canvas x:Name="selectionCanvas"/>
            <!-- Inking area -->
            <InkCanvas x:Name="inkCanvas"/>
        </Grid>
    </Grid>
```

2.  Nous définissons ensuite certains comportements d’entrée manuscrite de base.

    L’élément [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) est configuré pour interpréter les données d’entrée de stylet et de souris sous forme de traits d’encre ([**InputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.inputdevicetypes)). Des écouteurs d’événements Click sur les boutons ainsi que des événements relatifs au pointeur et aux traits pour la fonctionnalité de sélection sont également déclarés ici.

    Pour obtenir un exemple complet de sélection de traits d’encre, consultez Entrée directe pour traitement avancé dans [Interactions avec le stylo ou le stylet](pen-and-stylus-interactions.md).
```csharp
public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Listen for button click to cut ink strokes.
        btnCut.Click += btnCut_Click;
        // Listen for button click to copy ink strokes.
        btnCopy.Click += btnCopy_Click;
        // Listen for button click to paste ink strokes.
        btnPaste.Click += btnPaste_Click;
        // Listen for button click to clear ink canvas.
        btnClear.Click += btnClear_Click;

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

3.  Enfin, après avoir ajouté la prise en charge de la sélection de traits, nous implémentons la fonctionnalité de Presse-papiers dans les gestionnaires d’événements Click sur les boutons **Couper**, **Copier** et **Coller**.

    Pour Cut, nous appelons d’abord [**CopySelectedToClipboard**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstrokecontainer.copyselectedtoclipboard) sur le [**InkStrokeContainer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) de l' [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter).

    Nous appelons ensuite [**DeleteSelected**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstrokecontainer.deleteselected) pour supprimer les traits de la toile d’encre.

    Enfin, nous supprimons tous les traits de sélection du canevas de sélection.
    
```csharp
private void btnCut_Click(object sender, RoutedEventArgs e)
    {
        inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
        inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
        ClearSelection();
    }
```
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

Pour la copie, nous appelons simplement [**CopySelectedToClipboard**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstrokecontainer.copyselectedtoclipboard) sur le [**InkStrokeContainer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) de l' [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter).


```csharp
private void btnCopy_Click(object sender, RoutedEventArgs e)
    {
        inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
    }
```

Pour le collage, nous appelons [**CanPasteFromClipboard**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstrokecontainer.canpastefromclipboard) pour vous assurer que le contenu du presse-papiers peut être collé dans le canevas d’encre.

Si c’est le cas, nous appelons [**PasteFromClipboard**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstrokecontainer.pastefromclipboard) pour insérer les traits d’encre du presse-papiers dans le [**InkStrokeContainer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) de l' [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter), qui restitue ensuite les traits dans le canevas d’encre.

```csharp
private void btnPaste_Click(object sender, RoutedEventArgs e)
    {
        if (inkCanvas.InkPresenter.StrokeContainer.CanPasteFromClipboard())
        {
            inkCanvas.InkPresenter.StrokeContainer.PasteFromClipboard(
                new Point(0, 0));
        }
        else
        {
            // Cannot paste from clipboard.
        }
    }
```

## <a name="related-articles"></a>Articles connexes

* [Interactions avec le stylo et le stylet](pen-and-stylus-interactions.md)

**Exemples de la rubrique**
* [Enregistrer et charger des traits d’encre à partir d’un fichier ISF (Ink Serialized Format)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store.zip)
* [Enregistrer et charger des traits manuscrits à partir du presse-papiers](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store-clipboard.zip)

**Autres exemples**
* [Exemple d’encre simple (C#/C + +)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)
* [Exemple d’encre complexe (C++)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)
* [Ink, exemple (JavaScript)](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BJavaScript%5D-Windows%208%20app%20samples/JavaScript/Windows%208%20app%20samples/Input%20Ink%20sample%20(Windows%208))
* [Didacticiel de prise en main : écriture manuscrite dans votre application Windows](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-Ink)
* [Exemple de livre de coloriage](https://github.com/Microsoft/Windows-appsample-coloringbook)
* [Exemple de notes de famille](https://github.com/Microsoft/Windows-appsample-familynotes)



