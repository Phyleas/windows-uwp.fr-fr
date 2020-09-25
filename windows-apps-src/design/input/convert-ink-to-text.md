---
Description: Utilisez la reconnaissance de l’écriture manuscrite et l’analyse de l’encre pour reconnaître les traits d’encre Windows en tant que texte et formes.
title: Reconnaître les traits Windows Ink en tant que texte et formes
ms.assetid: C2F3F3CE-737F-4652-98B7-5278A462F9D3
label: Recognize Windows Ink strokes as text
template: detail.hbs
keywords: Windows Ink, entrée manuscrite Windows, DirectInk, InkPresenter, InkCanvas, reconnaissance de l’écriture manuscrite, interaction utilisateur, entrées
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 66b5303d65e1fefbf3eb8a156ce4ca4c10afda96
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220562"
---
# <a name="recognize-windows-ink-strokes-as-text-and-shapes"></a>Reconnaître les traits Windows Ink en tant que texte et formes

Convertissez les traits d’encre en texte et en formes à l’aide des fonctionnalités de reconnaissance intégrées à l’encre Windows.

> **API importantes**: [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas), [**Windows. UI. Input. encrage**](/uwp/api/Windows.UI.Input.Inking)

## <a name="free-form-recognition-with-ink-analysis"></a>Reconnaissance de forme libre avec analyse de l’encre

Ici, nous montrons comment utiliser le moteur d’analyse d’entrée manuscrite Windows ([Windows. UI. Input. encrage. Analysis](/uwp/api/windows.ui.input.inking.analysis)) pour classifier, analyser et reconnaître un ensemble de traits de forme libre sur un [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) sous forme de texte ou de formes. (Outre la reconnaissance du texte et des formes, l’analyse de l’encre peut également être utilisée pour reconnaître la structure des documents, les listes à puces et les dessins génériques.)

> [!NOTE]
> Pour obtenir des scénarios de base de texte brut simple, tels que l’entrée de formulaire, consultez reconnaissance de l' [écriture manuscrite restreinte](#constrained-handwriting-recognition) plus loin dans cette rubrique.

Dans cet exemple, la reconnaissance est lancée lorsque l’utilisateur clique sur un bouton pour indiquer qu’il s’agit d’un dessin terminé.

**Télécharger cet exemple à partir de l' [exemple analyse de l’encre (Basic)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-analysis-basic.zip)**

1. Tout d’abord, nous allons configurer l’interface utilisateur (MainPage. Xaml). 

   L’interface utilisateur comprend un bouton « Recognize », un [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)et un [**canevas**](/uwp/api/windows.ui.xaml.controls.canvas)standard. Quand vous appuyez sur le bouton « reconnaître », tous les traits d’encre sur la toile de dessin sont analysés et (s’ils sont reconnus) les formes et le texte correspondants sont dessinés sur le canevas standard. Les traits d’encre d’origine sont ensuite supprimés de la toile d’encre.

   ```xaml
   <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel" 
                    Orientation="Horizontal" 
                    Grid.Row="0">
            <TextBlock x:Name="Header" 
                        Text="Basic ink analysis sample" 
                        Style="{ThemeResource HeaderTextBlockStyle}" 
                        Margin="10,0,0,0" />
            <Button x:Name="recognize" 
                    Content="Recognize" 
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid x:Name="drawingCanvas" Grid.Row="1">

            <!-- The canvas where we render the replacement text and shapes. -->
            <Canvas x:Name="recognitionCanvas" />
            <!-- The canvas for ink input. -->
            <InkCanvas x:Name="inkCanvas" />

        </Grid>
   </Grid>
   ```

2. Dans le fichier code-behind de l’interface utilisateur (MainPage.xaml.cs), ajoutez les références de type d’espace de noms requises pour notre fonctionnalité d’analyse de l’encre et de l’encre :
    - [Windows.UI.Input.Inking](/uwp/api/windows.ui.input.inking)
    - [Windows. UI. Input. encrage. Analysis](/uwp/api/windows.ui.input.inking.analysis)
    - [Windows.UI.Xaml.Shapes](/uwp/api/windows.ui.xaml.shapes)

3. Nous spécifions ensuite nos variables globales :

   ```csharp
    InkAnalyzer inkAnalyzer = new InkAnalyzer();
    IReadOnlyList<InkStroke> inkStrokes = null;
    InkAnalysisResult inkAnalysisResults = null;
   ```

4. Ensuite, nous définissons des comportements de base de l’entrée manuscrite :
    - L' [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) est configuré pour interpréter les données d’entrée à partir du stylet, de la souris et du toucher en tant que traits d’encre ([**InputDeviceTypes**](/uwp/api/windows.ui.input.inking.inkpresenter.inputdevicetypes)). 
    - Les traits d’encre sont restitués sur [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) à l’aide de l’élément [**InkDrawingAttributes**](/windows/desktop/tablet/inkdrawingattributes-class) spécifié. 
    - Un écouteur de l’événement Click est également déclaré sur le bouton « Reconnaître ».

    ```csharp
    /// <summary>
    /// Initialize the UI page.
    /// </summary>
    public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen | 
            Windows.UI.Core.CoreInputDeviceTypes.Touch;

        // Set initial ink stroke attributes.
        InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
        drawingAttributes.Color = Windows.UI.Colors.Black;
        drawingAttributes.IgnorePressure = false;
        drawingAttributes.FitToCurve = true;
        inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);

        // Listen for button click to initiate recognition.
        recognize.Click += RecognizeStrokes_Click;
    }
    ```

5. Pour cet exemple, nous procédons à l’analyse de l’encre dans le gestionnaire d’événements Click du bouton « Recognize » (reconnaître).
    - Tout d’abord, appelez [**GetStrokes**](/uwp/api/windows.ui.input.inking.inkstrokecontainer.GetStrokes) sur le [**StrokeContainer**](/uwp/api/windows.ui.input.inking.inkpresenter.StrokeContainer) de [**InkCanvas. InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.InkPresenter) pour récupérer la collection de tous les traits d’encre actuels.
    - Si des traits d’encre sont présents, transmettez-les dans un appel à [**AddDataForStrokes**](/uwp/api/windows.ui.input.inking.analysis.inkanalyzer#Windows_UI_Input_Inking_Analysis_InkAnalyzer_AddDataForStrokes_Windows_Foundation_Collections_IIterable_Windows_UI_Input_Inking_InkStroke__) du InkAnalyzer.
    - Nous essayons de reconnaître les dessins et le texte, mais vous pouvez utiliser la méthode [**SetStrokeDataKind**](/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.setstrokedatakind) pour spécifier si vous êtes intéressé uniquement par du texte (y compris la structure de document et les listes de puces) ou uniquement dans les dessins (y compris la reconnaissance de forme).
    - Appelez [**AnalyzeAsync**](/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.AnalyzeAsync) pour initier l’analyse de l’encre et récupérez [**InkAnalysisResult**](/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult).
    - Si l' [**État**](/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult.Status) retourne l’état **mis à jour**, appelez [**FindNodes**](/uwp/api/windows.ui.input.inking.analysis.inkanalysisroot.findnodes) pour [**InkAnalysisNodeKind. InkWord**](/uwp/api/windows.ui.input.inking.analysis.inkanalysisnodekind) et [**InkAnalysisNodeKind. InkDrawing**](/uwp/api/windows.ui.input.inking.analysis.inkanalysisnodekind).
    - Itérez au sein des jeux de types de nœuds et dessinez le texte ou la forme respectifs sur la zone de travail reconnaissance (sous le canevas d’encre).
    - Enfin, supprimez les nœuds reconnus de la InkAnalyzer et les traits d’encre correspondants de la toile d’encre.

    ```csharp
    /// <summary>
    /// The "Analyze" button click handler.
    /// Ink recognition is performed here.
    /// </summary>
    /// <param name="sender">Source of the click event</param>
    /// <param name="e">Event args for the button click routed event</param>
    private async void RecognizeStrokes_Click(object sender, RoutedEventArgs e)
    {
        inkStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
        // Ensure an ink stroke is present.
        if (inkStrokes.Count > 0)
        {
            inkAnalyzer.AddDataForStrokes(inkStrokes);

            // In this example, we try to recognizing both 
            // writing and drawing, so the platform default 
            // of "InkAnalysisStrokeKind.Auto" is used.
            // If you're only interested in a specific type of recognition,
            // such as writing or drawing, you can constrain recognition 
            // using the SetStrokDataKind method as follows:
            // foreach (var stroke in strokesText)
            // {
            //     analyzerText.SetStrokeDataKind(
            //      stroke.Id, InkAnalysisStrokeKind.Writing);
            // }
            // This can improve both efficiency and recognition results.
            inkAnalysisResults = await inkAnalyzer.AnalyzeAsync();

            // Have ink strokes on the canvas changed?
            if (inkAnalysisResults.Status == InkAnalysisStatus.Updated)
            {
                // Find all strokes that are recognized as handwriting and 
                // create a corresponding ink analysis InkWord node.
                var inkwordNodes = 
                    inkAnalyzer.AnalysisRoot.FindNodes(
                        InkAnalysisNodeKind.InkWord);

                // Iterate through each InkWord node.
                // Draw primary recognized text on recognitionCanvas 
                // (for this example, we ignore alternatives), and delete 
                // ink analysis data and recognized strokes.
                foreach (InkAnalysisInkWord node in inkwordNodes)
                {
                    // Draw a TextBlock object on the recognitionCanvas.
                    DrawText(node.RecognizedText, node.BoundingRect);

                    foreach (var strokeId in node.GetStrokeIds())
                    {
                        var stroke = 
                            inkCanvas.InkPresenter.StrokeContainer.GetStrokeById(strokeId);
                        stroke.Selected = true;
                    }
                    inkAnalyzer.RemoveDataForStrokes(node.GetStrokeIds());
                }
                inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();

                // Find all strokes that are recognized as a drawing and 
                // create a corresponding ink analysis InkDrawing node.
                var inkdrawingNodes =
                    inkAnalyzer.AnalysisRoot.FindNodes(
                        InkAnalysisNodeKind.InkDrawing);
                // Iterate through each InkDrawing node.
                // Draw recognized shapes on recognitionCanvas and
                // delete ink analysis data and recognized strokes.
                foreach (InkAnalysisInkDrawing node in inkdrawingNodes)
                {
                    if (node.DrawingKind == InkAnalysisDrawingKind.Drawing)
                    {
                        // Catch and process unsupported shapes (lines and so on) here.
                    }
                    // Process generalized shapes here (ellipses and polygons).
                    else
                    {
                        // Draw an Ellipse object on the recognitionCanvas (circle is a specialized ellipse).
                        if (node.DrawingKind == InkAnalysisDrawingKind.Circle || node.DrawingKind == InkAnalysisDrawingKind.Ellipse)
                        {
                            DrawEllipse(node);
                        }
                        // Draw a Polygon object on the recognitionCanvas.
                        else
                        {
                            DrawPolygon(node);
                        }
                        foreach (var strokeId in node.GetStrokeIds())
                        {
                            var stroke = inkCanvas.InkPresenter.StrokeContainer.GetStrokeById(strokeId);
                            stroke.Selected = true;
                        }
                    }
                    inkAnalyzer.RemoveDataForStrokes(node.GetStrokeIds());
                }
                inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
            }
        }
    }
    ```

6. Voici la fonction permettant de dessiner un TextBlock sur notre canevas de reconnaissance. Nous utilisons le rectangle englobant du trait d’encre associé sur le canevas d’encre pour définir la position et la taille de police du TextBlock.

   ```csharp
    /// <summary>
    /// Draw ink recognition text string on the recognitionCanvas.
    /// </summary>
    /// <param name="recognizedText">The string returned by text recognition.</param>
    /// <param name="boundingRect">The bounding rect of the original ink writing.</param>
    private void DrawText(string recognizedText, Rect boundingRect)
    {
        TextBlock text = new TextBlock();
        Canvas.SetTop(text, boundingRect.Top);
        Canvas.SetLeft(text, boundingRect.Left);
    
        text.Text = recognizedText;
        text.FontSize = boundingRect.Height;
    
        recognitionCanvas.Children.Add(text);
    }
   ```

7. Voici les fonctions permettant de dessiner des ellipses et des polygones sur notre canevas de reconnaissance. Nous utilisons le rectangle englobant du trait d’encre associé sur le canevas d’encre pour définir la position et la taille de police des formes.

   ```csharp
    // Draw an ellipse on the recognitionCanvas.
    private void DrawEllipse(InkAnalysisInkDrawing shape)
    {
        var points = shape.Points;
        Ellipse ellipse = new Ellipse();

        ellipse.Width = shape.BoundingRect.Width;
        ellipse.Height = shape.BoundingRect.Height;

        Canvas.SetTop(ellipse, shape.BoundingRect.Top);
        Canvas.SetLeft(ellipse, shape.BoundingRect.Left);

        var brush = new SolidColorBrush(Windows.UI.ColorHelper.FromArgb(255, 0, 0, 255));
        ellipse.Stroke = brush;
        ellipse.StrokeThickness = 2;
        recognitionCanvas.Children.Add(ellipse);
    }

    // Draw a polygon on the recognitionCanvas.
    private void DrawPolygon(InkAnalysisInkDrawing shape)
    {
        List<Point> points = new List<Point>(shape.Points);
        Polygon polygon = new Polygon();

        foreach (Point point in points)
        {
            polygon.Points.Add(point);
        }

        var brush = new SolidColorBrush(Windows.UI.ColorHelper.FromArgb(255, 0, 0, 255));
        polygon.Stroke = brush;
        polygon.StrokeThickness = 2;
        recognitionCanvas.Children.Add(polygon);
    }
   ```

Voici cet exemple en action :

| Avant l’analyse | Après l’analyse |
| --- | --- |
| ![Avant l’analyse](images/ink/ink-analysis-raw2-small.png) | ![Après l’analyse](images/ink/ink-analysis-analyzed2-small.png) |

## <a name="constrained-handwriting-recognition"></a>Reconnaissance de l’écriture manuscrite restreinte

Dans la section précédente ([reconnaissance de forme libre avec l’analyse de l’encre](#free-form-recognition-with-ink-analysis)), nous avons démontré comment utiliser les [API d’analyse d’encre](/uwp/api/windows.ui.input.inking.analysis) pour analyser et reconnaître des traits d’encre arbitraires au sein d’une zone InkCanvas.

Dans cette section, nous montrons comment utiliser le moteur de reconnaissance de l’écriture manuscrite Windows Ink (pas l’analyse de l’encre) pour convertir un ensemble de traits sur un [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) en texte (en fonction du module linguistique par défaut installé).

> [!NOTE]
> La reconnaissance de base de l’écriture manuscrite présentée dans cette section est idéale pour les scénarios de saisie de texte sur une seule ligne, tels que l’entrée de formulaire. Pour obtenir des scénarios de reconnaissance plus riches qui incluent l’analyse et l’interprétation de la structure de document, des éléments de liste, des formes et des dessins (en plus de la reconnaissance de texte), consultez la section précédente : [reconnaissance de forme libre avec l’analyse d’encre](#free-form-recognition-with-ink-analysis).

Dans cet exemple, la reconnaissance est lancée lorsque l’utilisateur clique sur un bouton pour indiquer que l’écriture est terminée.

**Télécharger cet exemple à partir de l' [exemple de reconnaissance](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-handwriting-reco.zip) de l’écriture manuscrite**

1. Tout d’abord, nous configurons l’interface utilisateur.

   L’interface utilisateur comprend un bouton « Recognize », un [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)et une zone pour afficher les résultats de la reconnaissance.    

   ```xaml
   <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel"
                    Orientation="Horizontal"
                    Grid.Row="0">
                <TextBlock x:Name="Header"
                    Text="Basic ink recognition sample"
                    Style="{ThemeResource HeaderTextBlockStyle}"
                    Margin="10,0,0,0" />
                <Button x:Name="recognize"
                    Content="Recognize"
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid Grid.Row="1">
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="Auto"/>
            </Grid.RowDefinitions>
            <InkCanvas x:Name="inkCanvas"
                Grid.Row="0"/>
            <TextBlock x:Name="recognitionResult"
                Grid.Row="1"
                Margin="50,0,10,0"/>
        </Grid>
    </Grid>
    ```

2. Pour cet exemple, vous devez d’abord ajouter les références de type d’espace de noms requises pour notre fonctionnalité d’encre :
    - [Windows.UI.Input](/uwp/api/windows.ui.input)
    - [Windows.UI.Input.Inking](/uwp/api/windows.ui.input.inking)

3. Nous définissons ensuite certains comportements d’entrée manuscrite de base.

    L’élément [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) est configuré pour interpréter les données d’entrée de stylet et de souris sous forme de traits d’encre ([**InputDeviceTypes**](/uwp/api/windows.ui.input.inking.inkpresenter.inputdevicetypes)). Les traits d’encre sont restitués sur [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) à l’aide de l’élément [**InkDrawingAttributes**](/windows/desktop/tablet/inkdrawingattributes-class) spécifié. Un écouteur de l’événement Click est également déclaré sur le bouton « Reconnaître ».

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

            // Listen for button click to initiate recognition.
            recognize.Click += Recognize_Click;
        }
    ```

4. Enfin, nous procédons à la reconnaissance de l’écriture manuscrite de base. Pour cet exemple, nous utilisons le gestionnaire d’événements Click du bouton « Reconnaître » pour procéder à la reconnaissance de l’écriture manuscrite.

   - Un [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) stocke tous les traits d’encre dans un objet [**InkStrokeContainer**](/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) . Les traits sont exposés par le biais de la propriété [**StrokeContainer**](/uwp/api/windows.ui.input.inking.inkpresenter.strokecontainer) de l’élément **InkPresenter** et récupérés à l’aide de la méthode [**GetStrokes**](/uwp/api/windows.ui.input.inking.inkstrokecontainer.getstrokes). 

    ```csharp
    // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
    ```

    - Un [**InkRecognizerContainer**](/uwp/api/Windows.UI.Input.Inking.InkRecognizerContainer) est créé pour gérer le processus de reconnaissance de l’écriture manuscrite.

    ```csharp
    // Create a manager for the InkRecognizer object
        // used in handwriting recognition.
        InkRecognizerContainer inkRecognizerContainer =
            new InkRecognizerContainer();
    ```

    - [**RecognizeAsync**](/uwp/api/windows.ui.input.inking.inkmanager.recognizeasync) est appelé pour récupérer un ensemble d’objets [**InkRecognitionResult**](/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) . Les résultats de la reconnaissance sont générés pour chaque mot détecté par un [**InkRecognizer**](/uwp/api/Windows.UI.Input.Inking.InkRecognizer).

    ```csharp
    // Recognize all ink strokes on the ink canvas.
        IReadOnlyList<InkRecognitionResult> recognitionResults =
            await inkRecognizerContainer.RecognizeAsync(
                inkCanvas.InkPresenter.StrokeContainer,
                InkRecognitionTarget.All);
    ```

    - Chaque objet [**InkRecognitionResult**](/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) contient un ensemble de candidats de texte. L’élément le plus haut de cette liste est considéré par le moteur de reconnaissance comme étant la meilleure correspondance, suivi des candidats restants, afin de réduire la confiance.

       Nous allons itérer au sein de chaque [**InkRecognitionResult**](/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) et compiler la liste des candidats. Les candidats sont ensuite affichés et le [**InkStrokeContainer**](/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) est effacé (ce qui efface également l' [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)).

    ```csharp
    string str = "Recognition result\n";
        // Iterate through the recognition results.
        foreach (var result in recognitionResults)
        {
            // Get all recognition candidates from each recognition result.
            IReadOnlyList<string> candidates = result.GetTextCandidates();
            str += "Candidates: " + candidates.Count.ToString() + "\n";
            foreach (string candidate in candidates)
            {
                str += candidate + " ";
            }
        }
        // Display the recognition candidates.
        recognitionResult.Text = str;
        // Clear the ink canvas once recognition is complete.
        inkCanvas.InkPresenter.StrokeContainer.Clear();
    ```

    - Voici un exemple de gestionnaire de clics, complet.

    ```csharp
    // Handle button click to initiate recognition.
        private async void Recognize_Click(object sender, RoutedEventArgs e)
        {
            // Get all strokes on the InkCanvas.
            IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();

            // Ensure an ink stroke is present.
            if (currentStrokes.Count > 0)
            {
                // Create a manager for the InkRecognizer object
                // used in handwriting recognition.
                InkRecognizerContainer inkRecognizerContainer =
                    new InkRecognizerContainer();

                // inkRecognizerContainer is null if a recognition engine is not available.
                if (!(inkRecognizerContainer == null))
                {
                    // Recognize all ink strokes on the ink canvas.
                    IReadOnlyList<InkRecognitionResult> recognitionResults =
                        await inkRecognizerContainer.RecognizeAsync(
                            inkCanvas.InkPresenter.StrokeContainer,
                            InkRecognitionTarget.All);
                    // Process and display the recognition results.
                    if (recognitionResults.Count > 0)
                    {
                        string str = "Recognition result\n";
                        // Iterate through the recognition results.
                        foreach (var result in recognitionResults)
                        {
                            // Get all recognition candidates from each recognition result.
                            IReadOnlyList<string> candidates = result.GetTextCandidates();
                            str += "Candidates: " + candidates.Count.ToString() + "\n";
                            foreach (string candidate in candidates)
                            {
                                str += candidate + " ";
                            }
                        }
                        // Display the recognition candidates.
                        recognitionResult.Text = str;
                        // Clear the ink canvas once recognition is complete.
                        inkCanvas.InkPresenter.StrokeContainer.Clear();
                    }
                    else
                    {
                        recognitionResult.Text = "No recognition results.";
                    }
                }
                else
                {
                    Windows.UI.Popups.MessageDialog messageDialog = new Windows.UI.Popups.MessageDialog("You must install handwriting recognition engine.");
                    await messageDialog.ShowAsync();
                }
            }
            else
            {
                recognitionResult.Text = "No ink strokes to recognize.";
            }
        }
    ```

## <a name="international-recognition"></a>Reconnaissance internationale

La reconnaissance de l’écriture manuscrite intégrée à la plate-forme Windows Ink comprend un sous-ensemble complet de paramètres régionaux et de langues pris en charge par Windows.

Pour obtenir la liste des langues prises en charge par le [**InkRecognizer**](/uwp/api/Windows.UI.Input.Inking.InkRecognizer) , consultez la rubrique relative à la propriété [**InkRecognizer.Name**](/uwp/api/windows.ui.input.inking.inkrecognizer.name) .

Votre application peut interroger l’ensemble des moteurs de reconnaissance de l’écriture manuscrite installés et utiliser l’un d’eux, ou laisser un utilisateur sélectionner sa langue par défaut.

**Remarque**    Les utilisateurs peuvent consulter la liste des langues installées en accédant à **paramètres- &gt; heure & langage**. Les langues installées figurent sous **Langues**.

Pour installer de nouveaux modules linguistiques et activer la reconnaissance de l’écriture manuscrite pour cette langue, procédez comme suit :

1. Accédez à **Paramètres &gt; Heure et langue &gt; Région et langue**.
2. Cliquez sur **Ajouter une langue**.
3. Sélectionnez une langue de la liste, puis choisissez la version de la région. La langue est désormais répertoriée dans la page **Région et langue**.
4. Cliquez sur la langue et sélectionnez **Options**.
5. Dans la page **Options de langue**, téléchargez le **moteur de reconnaissance de l’écriture manuscrite** (vous pouvez également télécharger le module linguistique dans son intégralité, le moteur de reconnaissance vocale et la disposition du clavier ici).

Nous montrons ici comment utiliser le moteur de reconnaissance de l’écriture manuscrite pour interpréter un ensemble de traits sur un élément [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) basé sur le module de reconnaissance sélectionné.

L’utilisateur lance la reconnaissance en cliquant sur un bouton une fois qu’il a terminé d’écrire.

1. Tout d’abord, nous configurons l’interface utilisateur.

   L’interface utilisateur comprend un bouton « Recognize » (reconnaître), une zone de liste modifiable qui répertorie tous les reconnaissances d’écriture manuscrite, l' [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)et une zone pour afficher les résultats de la reconnaissance.

    ```xaml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <StackPanel x:Name="HeaderPanel"
                        Orientation="Horizontal"
                        Grid.Row="0">
                <TextBlock x:Name="Header"
                           Text="Advanced international ink recognition sample"
                           Style="{ThemeResource HeaderTextBlockStyle}"
                           Margin="10,0,0,0" />
                <ComboBox x:Name="comboInstalledRecognizers"
                         Margin="50,0,10,0">
                    <ComboBox.ItemTemplate>
                        <DataTemplate>
                            <StackPanel Orientation="Horizontal">
                                <TextBlock Text="{Binding Name}" />
                            </StackPanel>
                        </DataTemplate>
                    </ComboBox.ItemTemplate>
                </ComboBox>
                <Button x:Name="buttonRecognize"
                        Content="Recognize"
                        IsEnabled="False"
                        Margin="50,0,10,0"/>
            </StackPanel>
            <Grid Grid.Row="1">
                <Grid.RowDefinitions>
                    <RowDefinition Height="*"/>
                    <RowDefinition Height="Auto"/>
                </Grid.RowDefinitions>
                <InkCanvas x:Name="inkCanvas"
                           Grid.Row="0"/>
                <TextBlock x:Name="recognitionResult"
                           Grid.Row="1"
                           Margin="50,0,10,0"/>
            </Grid>
        </Grid>
    ```

2. Nous définissons ensuite certains comportements d’entrée manuscrite de base.

   L’élément [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) est configuré pour interpréter les données d’entrée de stylet et de souris sous forme de traits d’encre ([**InputDeviceTypes**](/uwp/api/windows.ui.input.inking.inkpresenter.inputdevicetypes)). Les traits d’encre sont restitués sur [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) à l’aide de l’élément [**InkDrawingAttributes**](/windows/desktop/tablet/inkdrawingattributes-class) spécifié.

   Nous appelons une fonction `InitializeRecognizerList` pour remplir la zone de liste modifiable du module de reconnaissance avec une liste des modules de reconnaissance de l’écriture manuscrite installés.

   Nous déclarons également des écouteurs pour l’événement Click sur le bouton Reconnaître et l’événement modifié de la sélection sur la zone de liste modifiable du module de reconnaissance.

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

        // Populate the recognizer combo box with installed recognizers.
        InitializeRecognizerList();

        // Listen for combo box selection.
        comboInstalledRecognizers.SelectionChanged +=
            comboInstalledRecognizers_SelectionChanged;

        // Listen for button click to initiate recognition.
        buttonRecognize.Click += Recognize_Click;
    }
    ```

3. Nous remplissons la zone de liste modifiable du module de reconnaissance avec une liste des modules de reconnaissance de l’écriture manuscrite installés.

   Un [**InkRecognizerContainer**](/uwp/api/Windows.UI.Input.Inking.InkRecognizerContainer) est créé pour gérer le processus de reconnaissance de l’écriture manuscrite. Utilisez cet objet pour appeler [**GetRecognizers**](/uwp/api/windows.ui.input.inking.inkrecognizercontainer.getrecognizers) et récupérer la liste des module de reconnaissance installés pour remplir la zone de liste déroulante du module de reconnaissance.

    ```csharp
    // Populate the recognizer combo box with installed recognizers.
    private void InitializeRecognizerList()
    {
        // Create a manager for the handwriting recognition process.
        inkRecognizerContainer = new InkRecognizerContainer();
        // Retrieve the collection of installed handwriting recognizers.
        IReadOnlyList<InkRecognizer> installedRecognizers =
            inkRecognizerContainer.GetRecognizers();
        // inkRecognizerContainer is null if a recognition engine is not available.
        if (!(inkRecognizerContainer == null))
        {
            comboInstalledRecognizers.ItemsSource = installedRecognizers;
            buttonRecognize.IsEnabled = true;
        }
    }
    ```
    
4. Mettez à jour le module de reconnaissance de l’écriture manuscrite en cas de modification de la sélection de la zone de liste modifiable.

   Utilisez [**InkRecognizerContainer**](/uwp/api/Windows.UI.Input.Inking.InkRecognizerContainer) pour appeler [**SetDefaultRecognizer**](/uwp/api/windows.ui.input.inking.inkrecognizercontainer.setdefaultrecognizer) en fonction du module de reconnaissance sélectionné à partir de la zone de liste déroulante du module de reconnaissance.

    ```csharp
    // Handle recognizer change.
        private void comboInstalledRecognizers_SelectionChanged(
            object sender, SelectionChangedEventArgs e)
        {
            inkRecognizerContainer.SetDefaultRecognizer(
                (InkRecognizer)comboInstalledRecognizers.SelectedItem);
        }
    ```

5. Enfin, nous effectuons la reconnaissance de l’écriture manuscrite en fonction du module de reconnaissance sélectionné. Pour cet exemple, nous utilisons le gestionnaire d’événements Click du bouton « Reconnaître » pour procéder à la reconnaissance de l’écriture manuscrite.

   - Un [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) stocke tous les traits d’encre dans un objet [**InkStrokeContainer**](/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) . Les traits sont exposés par le biais de la propriété [**StrokeContainer**](/uwp/api/windows.ui.input.inking.inkpresenter.strokecontainer) de l’élément **InkPresenter** et récupérés à l’aide de la méthode [**GetStrokes**](/uwp/api/windows.ui.input.inking.inkstrokecontainer.getstrokes).

    ```csharp
    // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes =
            inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
    ```

    - [**RecognizeAsync**](/uwp/api/windows.ui.input.inking.inkmanager.recognizeasync) est appelé pour récupérer un ensemble d’objets [**InkRecognitionResult**](/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) .

      Les résultats de la reconnaissance sont générés pour chaque mot détecté par un [**InkRecognizer**](/uwp/api/Windows.UI.Input.Inking.InkRecognizer).

    ```csharp
    // Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults =
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer,
            InkRecognitionTarget.All);
    ```

    - Chaque objet [**InkRecognitionResult**](/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) contient un ensemble de candidats de texte. L’élément le plus haut de cette liste est considéré par le moteur de reconnaissance comme étant la meilleure correspondance, suivi des candidats restants, afin de réduire la confiance.

       Nous allons itérer au sein de chaque [**InkRecognitionResult**](/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) et compiler la liste des candidats. Les candidats sont ensuite affichés et le [**InkStrokeContainer**](/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) est effacé (ce qui efface également l' [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)).

    ```csharp
    string str = "Recognition result\n";
    // Iterate through the recognition results.
    foreach (InkRecognitionResult result in recognitionResults)
    {
            // Get all recognition candidates from each recognition result.
            IReadOnlyList<string> candidates =
                result.GetTextCandidates();
            str += "Candidates: " + candidates.Count.ToString() + "\n";
            foreach (string candidate in candidates)
            {
                str += candidate + " ";
            }
    }
    // Display the recognition candidates.
    recognitionResult.Text = str;
    // Clear the ink canvas once recognition is complete.
    inkCanvas.InkPresenter.StrokeContainer.Clear();
    ```

    - Voici un exemple de gestionnaire de clics, complet.

    ```csharp
    // Handle button click to initiate recognition.
    private async void Recognize_Click(object sender, RoutedEventArgs e)
    {
        // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes =
            inkCanvas.InkPresenter.StrokeContainer.GetStrokes();

        // Ensure an ink stroke is present.
        if (currentStrokes.Count > 0)
        {
            // inkRecognizerContainer is null if a recognition engine is not available.
            if (!(inkRecognizerContainer == null))
            {
                // Recognize all ink strokes on the ink canvas.
                IReadOnlyList<InkRecognitionResult> recognitionResults =
                    await inkRecognizerContainer.RecognizeAsync(
                        inkCanvas.InkPresenter.StrokeContainer,
                        InkRecognitionTarget.All);
                // Process and display the recognition results.
                if (recognitionResults.Count > 0)
                {
                    string str = "Recognition result\n";
                    // Iterate through the recognition results.
                    foreach (InkRecognitionResult result in recognitionResults)
                    {
                        // Get all recognition candidates from each recognition result.
                        IReadOnlyList<string> candidates =
                            result.GetTextCandidates();
                        str += "Candidates: " + candidates.Count.ToString() + "\n";
                        foreach (string candidate in candidates)
                        {
                            str += candidate + " ";
                        }
                    }
                    // Display the recognition candidates.
                    recognitionResult.Text = str;
                    // Clear the ink canvas once recognition is complete.
                    inkCanvas.InkPresenter.StrokeContainer.Clear();
                }
                else
                {
                    recognitionResult.Text = "No recognition results.";
                }
            }
            else
            {
                Windows.UI.Popups.MessageDialog messageDialog =
                    new Windows.UI.Popups.MessageDialog(
                        "You must install handwriting recognition engine.");
                await messageDialog.ShowAsync();
            }
        }
        else
        {
            recognitionResult.Text = "No ink strokes to recognize.";
        }
    }
    ```

## <a name="dynamic-recognition"></a>Reconnaissance dynamique

Bien que les deux exemples précédents requièrent que l’utilisateur appuie sur un bouton pour démarrer la reconnaissance, vous pouvez également effectuer la reconnaissance dynamique à l’aide de l’entrée de trait associée à une fonction de minutage de base.

Pour cet exemple, nous allons utiliser les mêmes paramètres de trait et d’interface utilisateur que l’exemple de reconnaissance internationale précédent.

1. Ces objets globaux ([InkAnalyzer](/uwp/api/windows.ui.input.inking.analysis.inkanalyzer), [InkStroke](/uwp/api/windows.ui.input.inking.inkstroke), [InkAnalysisResult](/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult), [DispatcherTimer](/uwp/api/windows.ui.xaml.dispatchertimer)) sont utilisés dans l’ensemble de l’application.

    ```csharp
    // Stroke recognition globals.
    InkAnalyzer inkAnalyzer;
    DispatcherTimer recoTimer;
    ```

2. Au lieu d’un bouton pour lancer la reconnaissance, nous ajoutons des écouteurs pour deux événements de trait [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) ([**StrokesCollected**](/uwp/api/windows.ui.input.inking.inkpresenter.strokescollected) et [**StrokeStarted**](/uwp/api/windows.ui.input.inking.inkstrokeinput.strokestarted)) et configurerons un minuteur de base ([**DispatcherTimer**](/uwp/api/Windows.UI.Xaml.DispatcherTimer)) avec un intervalle de [**graduation**](/uwp/api/windows.ui.xaml.dispatchertimer.tick) d’un seconde.

    ```csharp
    public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Listen for stroke events on the InkPresenter to 
        // enable dynamic recognition.
        // StrokesCollected is fired when the user stops inking by 
        // lifting their pen or finger, or releasing the mouse button.
        inkCanvas.InkPresenter.StrokesCollected += inkCanvas_StrokesCollected;
        // StrokeStarted is fired when ink input is first detected.
        inkCanvas.InkPresenter.StrokeInput.StrokeStarted +=
            inkCanvas_StrokeStarted;

        inkAnalyzer = new InkAnalyzer();

        // Timer to manage dynamic recognition.
        recoTimer = new DispatcherTimer();
        recoTimer.Interval = TimeSpan.FromSeconds(1);
        recoTimer.Tick += recoTimer_TickAsync;
    }
    ```

3. Nous définissons ensuite les gestionnaires pour les événements InkPresenter que nous avons déclarés dans la première étape (nous remplaçons également l’événement de la page [**OnNavigatingFrom**](/uwp/api/windows.ui.xaml.controls.page.onnavigatingfrom) pour gérer notre minuterie).

    - [**StrokesCollected**](/uwp/api/windows.ui.input.inking.inkpresenter.strokescollected)  
    Ajouter des traits d’encre ([**AddDataForStrokes**](/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.adddataforstrokes)) au InkAnalyzer et démarrer le minuteur de reconnaissance lorsque l’utilisateur arrête l’entrée manuscrite (en tirant son stylet ou son doigt, ou en relâchant le bouton de la souris). La reconnaissance est lancée dès que l’utilisateur arrête d’écrire pendant plus d’une seconde.  

        Utilisez la méthode [**SetStrokeDataKind**](/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.setstrokedatakind) pour spécifier si vous êtes intéressé uniquement par du texte (notamment la structure de document AMD Bullet List) ou uniquement dans les dessins (reconnaissance de la forme y compris).

    - [**StrokeStarted**](/uwp/api/windows.ui.input.inking.inkstrokeinput.strokestarted)  
    Arrête la minuterie sachant que le nouveau trait est probablement la suite d’une entrée manuscrite unique, si un nouveau trait commence avant le prochain battement d’horloge.

    ```csharp
    // Handler for the InkPresenter StrokeStarted event.
    // Don't perform analysis while a stroke is in progress.
    // If a new stroke starts before the next timer tick event,
    // stop the timer as the new stroke is likely the continuation
    // of a single handwriting entry.
    private void inkCanvas_StrokeStarted(InkStrokeInput sender, PointerEventArgs args)
    {
        recoTimer.Stop();
    }
    // Handler for the InkPresenter StrokesCollected event.
    // Stop the timer and add the collected strokes to the InkAnalyzer.
    // Start the recognition timer when the user stops inking (by 
    // lifting their pen or finger, or releasing the mouse button).
    // If ink input is not detected after one second, initiate recognition.
    private void inkCanvas_StrokesCollected(InkPresenter sender, InkStrokesCollectedEventArgs args)
    {
        recoTimer.Stop();
        // If you're only interested in a specific type of recognition,
        // such as writing or drawing, you can constrain recognition 
        // using the SetStrokDataKind method, which can improve both 
        // efficiency and recognition results.
        // In this example, "InkAnalysisStrokeKind.Writing" is used.
        foreach (var stroke in args.Strokes)
        {
            inkAnalyzer.AddDataForStroke(stroke);
            inkAnalyzer.SetStrokeDataKind(stroke.Id, InkAnalysisStrokeKind.Writing);
        }
        recoTimer.Start();
    }
    // Override the Page OnNavigatingFrom event handler to 
    // stop our timer if user leaves page.
    protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
    {
        recoTimer.Stop();
    }
    ```

4. Enfin, nous exécutons la reconnaissance de l’écriture manuscrite. Pour cet exemple, nous utilisons le gestionnaire d’événements [**Tick**](/uwp/api/windows.ui.xaml.dispatchertimer.tick) d’un élément [**DispatcherTimer**](/uwp/api/Windows.UI.Xaml.DispatcherTimer) pour lancer la reconnaissance de l’écriture manuscrite.
    - Appelez [**AnalyzeAsync**](/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.AnalyzeAsync) pour initier l’analyse de l’encre et récupérez [**InkAnalysisResult**](/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult).
    - Si l' [**État**](/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult.Status) retourne l’état **mis à jour**, appelez [**FindNodes**](/uwp/api/windows.ui.input.inking.analysis.inkanalysisroot.findnodes) pour les types de nœuds  [**InkAnalysisNodeKind. InkWord**](/uwp/api/windows.ui.input.inking.analysis.inkanalysisnodekind).
    - Itérer au sein des nœuds et afficher le texte reconnu.
    - Enfin, supprimez les nœuds reconnus de la InkAnalyzer et les traits d’encre correspondants de la toile d’encre.

    ```csharp
    private async void recoTimer_TickAsync(object sender, object e)
    {
        recoTimer.Stop();
        if (!inkAnalyzer.IsAnalyzing)
        {
            InkAnalysisResult result = await inkAnalyzer.AnalyzeAsync();

            // Have ink strokes on the canvas changed?
            if (result.Status == InkAnalysisStatus.Updated)
            {
                // Find all strokes that are recognized as handwriting and 
                // create a corresponding ink analysis InkWord node.
                var inkwordNodes =
                    inkAnalyzer.AnalysisRoot.FindNodes(
                        InkAnalysisNodeKind.InkWord);

                // Iterate through each InkWord node.
                // Display the primary recognized text (for this example, 
                // we ignore alternatives), and then delete the 
                // ink analysis data and recognized strokes.
                foreach (InkAnalysisInkWord node in inkwordNodes)
                {
                    string recognizedText = node.RecognizedText;
                    // Display the recognition candidates.
                    recognitionResult.Text = recognizedText;

                    foreach (var strokeId in node.GetStrokeIds())
                    {
                        var stroke =
                            inkCanvas.InkPresenter.StrokeContainer.GetStrokeById(strokeId);
                        stroke.Selected = true;
                    }
                    inkAnalyzer.RemoveDataForStrokes(node.GetStrokeIds());
                }
                inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
            }
        }
        else
        {
            // Ink analyzer is busy. Wait a while and try again.
            recoTimer.Start();
        }
    }
    ```

## <a name="related-articles"></a>Articles connexes

- [Interactions avec le stylo et le stylet](pen-and-stylus-interactions.md)

### <a name="topic-samples"></a>Exemples de la rubrique

- [Exemple d’analyse de l’encre (Basic) (C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-analysis-basic.zip)
- [Exemple de reconnaissance de l’écriture manuscrite (C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-handwriting-reco.zip)

### <a name="other-samples"></a>Autres exemples

- [Exemple d’encre simple (C#/C + +)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)
- [Exemple d’encre complexe (C++)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)
- [Ink, exemple (JavaScript)](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BJavaScript%5D-Windows%208%20app%20samples/JavaScript/Windows%208%20app%20samples/Input%20Ink%20sample%20(Windows%208))
- [Didacticiel de prise en main : écriture manuscrite dans votre application Windows](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-Ink)
- [Exemple de livre de coloriage](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [Exemple de notes de famille](https://github.com/Microsoft/Windows-appsample-familynotes)
