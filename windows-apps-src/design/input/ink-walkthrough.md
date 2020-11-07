---
ms.assetid: ''
title: Entrée manuscrite prise en charge dans votre application Windows
description: Découvrez comment prendre en charge l’écriture et le dessin avec Windows Ink dans une application de plateforme Windows universelle de base (UWP) en suivant ce didacticiel pas à pas.
keywords: encre, entrée manuscrite, didacticiel
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fb74b5d15b731a6b08a0adcec20a801b7e133a7f
ms.sourcegitcommit: aaa72ddeb01b074266f4cd51740eec8d1905d62d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2020
ms.locfileid: "94339817"
---
# <a name="tutorial-support-ink-in-your-windows-app"></a>Didacticiel : prendre en charge l’encre dans votre application Windows

![Image de héros du stylet de surface.](images/ink/ink-hero-small.png)  
*Stylet Surface* (disponible à l’achat dans la [Boutique Microsoft](https://www.microsoft.com/p/surface-pen/8zl5c82qmg6b)).

Ce didacticiel décrit comment créer une application Windows de base qui prend en charge l’écriture et le dessin avec Windows Ink. Nous utilisons des extraits de code à partir d’un exemple d’application, que vous pouvez télécharger à partir de GitHub (Voir l' [exemple de code](#sample-code)), pour illustrer les différentes fonctionnalités et les API Windows Ink associées (voir [composants de la plateforme Windows Ink](#components-of-the-windows-ink-platform)) présentées dans chaque étape.

Nous nous concentrons sur les éléments suivants :
* Ajout de la prise en charge de base de l’encre
* Ajout d’une barre d’outils Ink
* Prise en charge de la reconnaissance de l’écriture
* Prise en charge de la reconnaissance des formes de base
* Enregistrement et chargement de l’encre

Pour plus d’informations sur l’implémentation de ces fonctionnalités, consultez interactions avec le [stylet et Windows Ink dans les applications Windows](./pen-and-stylus-interactions.md).

## <a name="introduction"></a>Introduction

Avec Windows Ink, vous pouvez fournir à vos clients l’équivalent numérique de presque n’importe quelle expérience du stylo et du papier, des notes rapides, manuscrites et des annotations aux démonstrations du tableau blanc, et des dessins d’architecture et d’ingénierie aux bons de travail personnels.

## <a name="prerequisites"></a>Prérequis

* Un ordinateur (ou un ordinateur virtuel) qui exécute la version actuelle de Windows 10
* [Visual Studio 2019 et le kit de développement logiciel (SDK) RS2](https://developer.microsoft.com/windows/downloads)
* [SDK Windows 10 (10.0.15063.0)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* Selon votre configuration, vous devrez peut-être installer le package NuGet [Microsoft. Netcore. UniversalWindowsPlatform](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform) et activer le **mode développeur** dans vos paramètres système (paramètres-> Update & Security-> pour les développeurs-> utiliser les fonctionnalités de développement).
* Si vous ne connaissez pas le développement d’applications Windows avec Visual Studio, consultez les rubriques suivantes avant de commencer ce didacticiel :  
    * [Se préparer](/windows/apps/get-started/get-set-up)
    * [Créer une application « Hello World » (XAML)](../../get-started/create-a-hello-world-app-xaml-universal.md)
* **[Facultatif]** Un stylet numérique et un ordinateur avec un affichage qui prend en charge l’entrée de ce stylet numérique.

> [!NOTE] 
> Bien que Windows Ink puisse prendre en charge le dessin avec une souris et une touche tactile (nous montrons comment le faire à l’étape 3 de ce didacticiel) pour une expérience de Windows Ink optimale, nous vous recommandons d’utiliser un stylet numérique et un ordinateur avec un affichage qui prend en charge l’entrée de ce stylet numérique.

## <a name="sample-code"></a>Exemple de code
Tout au long de ce didacticiel, nous utilisons un exemple d’application Ink pour illustrer les concepts et les fonctionnalités abordés.

Téléchargez cet exemple Visual Studio et le code source à partir de [GitHub](https://github.com/) sur [Windows-appsample-to-Started-Ink Sample](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-Ink):

1. Sélectionner le bouton de **clonage ou de téléchargement** vert  
![Clonage du référentiel.](images/ink/ink-clone.png)
2. Si vous avez un compte GitHub, vous pouvez cloner le référentiel sur votre ordinateur local en choisissant **ouvrir dans Visual Studio** . 
3. Si vous n’avez pas de compte GitHub, ou si vous souhaitez simplement une copie locale du projet, choisissez **Télécharger zip** (vous devrez consulter régulièrement les dernières mises à jour)

> [!IMPORTANT]
> La majeure partie du code de l’exemple est commentée. À chaque étape, vous serez invité à supprimer les marques de commentaire de diverses sections du code. Dans Visual Studio, il vous suffit de mettre en surbrillance les lignes de code, puis d’appuyer sur CTRL-K, puis de CTRL + U.

## <a name="components-of-the-windows-ink-platform"></a>Composants de la plateforme Windows Ink

Ces objets fournissent la majeure partie de l’expérience d’entrée manuscrite pour les applications Windows.

| Composant | Description |
| --- | --- |
| [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas) | Un contrôle de plateforme d’interface utilisateur XAML, qui reçoit et affiche par défaut toutes les entrées à partir d’un stylet comme un trait d’encre ou un trait d’effacement. |
| [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) | Objet code-behind, instancié avec un contrôle [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) (exposé via la propriété [**InkCanvas. InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.InkPresenter) ). Cet objet fournit toutes les fonctionnalités d’encrage par défaut exposées par l' [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas), ainsi qu’un ensemble complet d’API pour une personnalisation et une personnalisation supplémentaires. |
| [**InkToolbar**](/uwp/api/Windows.UI.Xaml.Controls.InkToolbar) | Contrôle de plateforme d’interface utilisateur XAML contenant une collection personnalisable et extensible de boutons qui activent les fonctionnalités liées à l’encre dans un [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas)associé. |
| [**IInkD2DRenderer**](/windows/desktop/api/inkrenderer/nn-inkrenderer-iinkd2drenderer)<br/>Nous ne couvrons pas cette fonctionnalité ici. pour plus d’informations, consultez l' [exemple d’encre complexe](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk). | Active le rendu des traits d’encre dans le contexte de périphérique Direct2D désigné d’une application Windows universelle, au lieu du contrôle [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) par défaut. |

## <a name="step-1-run-the-sample"></a>Étape 1 : exécuter l’exemple

Après avoir téléchargé l’exemple d’application RadialController, vérifiez qu’il s’exécute :
1. Ouvrez l’exemple de projet dans Visual Studio.
2. Définissez la liste déroulante **plateformes solution** sur une sélection non-arm.
3. Appuyez sur F5 pour compiler, déployer et exécuter.  

   > [!NOTE]
   > Vous pouvez également sélectionner l’élément de menu **Déboguer**  >  **Démarrer le débogage** ou sélectionner le bouton d’exécution de l' **ordinateur local** indiqué ici.
   > ![Bouton de génération de projet Visual Studio.](images/ink/ink-vsrun-small.png)

La fenêtre d’application s’ouvre, et une fois que l’écran de démarrage s’affiche pendant quelques secondes, vous verrez cet écran initial.

![Capture d’écran de l’application vide.](images/ink/ink-app-step1-empty-small.png)

Nous avons maintenant l’application Windows de base que nous allons utiliser dans le reste de ce didacticiel. Dans les étapes suivantes, nous ajoutons notre fonctionnalité d’encre.

## <a name="step-2-use-inkcanvas-to-support-basic-inking"></a>Étape 2 : utiliser InkCanvas pour prendre en charge l’entrée de base

Vous avez peut-être déjà remarqué que l’application, dans sa forme initiale, ne vous permet pas de dessiner avec le stylet (bien que vous puissiez utiliser le stylet comme périphérique de pointage standard pour interagir avec l’application). 

Nous allons résoudre ce problème.

Pour ajouter la fonctionnalité de base de l’encrage, placez simplement un contrôle [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) sur la page appropriée dans votre application.

> [!NOTE]
> Un InkCanvas a des propriétés de [**hauteur**](/uwp/api/windows.ui.xaml.frameworkelement.Height) et de [**largeur**](/uwp/api/windows.ui.xaml.frameworkelement.Width) par défaut de zéro, sauf s’il s’agit de l’enfant d’un élément qui redimensionne automatiquement ses éléments enfants. 

### <a name="in-the-sample"></a>Dans l'exemple :
1. Ouvrez le fichier MainPage.xaml.cs.
2. Recherchez le code marqué avec le titre de cette étape (« //étape 2 : utiliser InkCanvas pour prendre en charge l’entrée de base »).
3. Supprimez les marques de commentaire des lignes suivantes. (Ces références sont requises pour les fonctionnalités utilisées dans les étapes suivantes).  

``` csharp
    using Windows.UI.Input.Inking;
    using Windows.UI.Input.Inking.Analysis;
    using Windows.UI.Xaml.Shapes;
    using Windows.Storage.Streams;
```

4. Ouvrez le fichier MainPage. Xaml.
5. Recherchez le code marqué avec le titre de cette étape (« \<!-- Step 2: Basic inking with InkCanvas --> »).
6. Supprimez les marques de commentaire de la ligne suivante.  

``` xaml
    <InkCanvas x:Name="inkCanvas" />
```

Et voilà ! 

Vous pouvez maintenant réexécuter l’application. Poursuivez avec Scribble, écrivez votre nom ou (si vous conservez un miroir ou si vous avez une très bonne mémoire) dessinez votre portrait.

![Capture d’écran de l’application d’exemple d’encre de base mise en surbrillance dans cette rubrique.](images/ink/ink-app-step1-name-small.png)

## <a name="step-3-support-inking-with-touch-and-mouse"></a>Étape 3 : prendre en charge l’encrage avec Touch et souris

Vous remarquerez que, par défaut, l’encre est prise en charge uniquement pour l’entrée de stylet. Si vous essayez d’écrire ou de dessiner avec votre doigt, votre souris ou votre pavé tactile, vous serez déçu.

Pour faire tourner ce Smiley, vous devez ajouter une deuxième ligne de code. Cette fois-ci, il se trouve dans le code-behind pour le fichier XAML dans lequel vous avez déclaré votre [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas). 

Dans cette étape, nous allons introduire l’objet [**InkPresenter**](/uwp/api/windows.ui.input.inking.inkpresenter) , qui offre une gestion plus fine de l’entrée, du traitement et du rendu de l’entrée manuscrite (standard et modifiée) sur votre [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas).

> [!NOTE]
> L’entrée d’encre standard (info-bulle ou bouton de gomme) n’est pas modifiée avec une offre de matériel secondaire, comme un bouton de stylet, un bouton droit de la souris ou un mécanisme similaire. 

Pour activer la souris et l’entrée tactile, définissez la propriété [**InputDeviceTypes**](/uwp/api/windows.ui.input.inking.inkpresenter.InputDeviceTypes) de l' [**InkPresenter**](/uwp/api/windows.ui.input.inking.inkpresenter) sur la combinaison des valeurs de [**CoreInputDeviceTypes**](/uwp/api/windows.ui.core.coreinputdevicetypes) que vous souhaitez.

### <a name="in-the-sample"></a>Dans l'exemple :
1. Ouvrez le fichier MainPage.xaml.cs.
2. Recherchez le code marqué avec le titre de cette étape (« //étape 3 : prendre en charge l’entrée tactile avec Touch and Mouse »).
3. Supprimez les marques de commentaire des lignes suivantes.  

``` csharp
    inkCanvas.InkPresenter.InputDeviceTypes =
        Windows.UI.Core.CoreInputDeviceTypes.Mouse | 
        Windows.UI.Core.CoreInputDeviceTypes.Touch | 
        Windows.UI.Core.CoreInputDeviceTypes.Pen;
```

Réexécutez l’application et vous verrez que tous vos rêves de peinture sur un ordinateur ont été vrais !

> [!NOTE]
> Lorsque vous spécifiez des types de périphériques d’entrée, vous devez indiquer la prise en charge de chaque type d’entrée spécifique (y compris Pen), car la définition de cette propriété remplace le paramètre [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas) par défaut.

## <a name="step-4-add-an-ink-toolbar"></a>Étape 4 : ajouter une barre d’outils d’encre

Le [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) est un contrôle de plateforme UWP qui fournit un ensemble personnalisable et extensible de boutons permettant d’activer les fonctionnalités liées à l’encre. 

Par défaut, le [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) comprend un ensemble de boutons de base qui permettent aux utilisateurs de sélectionner rapidement entre un stylo, un crayon, un surligneur ou un gomme, qui peut être utilisé avec un gabarit (règle ou véhicule). Les boutons stylet, crayon et surligneur fournissent chacun un menu volant permettant de sélectionner la couleur et la taille du trait de l’encre.

Pour ajouter un [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) par défaut à une application d’encrage, placez-le simplement sur la même page que votre [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas) et associez les deux contrôles.

### <a name="in-the-sample"></a>Dans l’exemple
1. Ouvrez le fichier MainPage. Xaml.
2. Recherchez le code marqué avec le titre de cette étape (« \<!-- Step 4: Add an ink toolbar --> »).
3. Supprimez les marques de commentaire des lignes suivantes.  

``` xaml
    <InkToolbar x:Name="inkToolbar" 
                        VerticalAlignment="Top" 
                        Margin="10,0,10,0"
                        TargetInkCanvas="{x:Bind inkCanvas}">
    </InkToolbar>
```

> [!NOTE]
> Pour que l’interface utilisateur et le code ne soient pas encombrés et simples, nous utilisons une disposition de grille de base et déclarez le [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) après l' [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas) dans une ligne de grille. Si vous la déclarez avant l' [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas), le [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) est affiché en premier, sous le canevas et inaccessible à l’utilisateur.  

Maintenant, exécutez à nouveau l’application pour voir le [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) et essayez certains des outils.

![Capture d’écran de l’application d’exemple d’encre de base mise en surbrillance dans cette rubrique avec la valeur par défaut InkToolbar.](images/ink/ink-inktoolbar-default-small.png)

### <a name="challenge-add-a-custom-button"></a>Défi : ajouter un bouton personnalisé
<table class="wdg-noborder">
<tr>
<td>

:::image type="icon" source="images/challenge-icon.png":::

</td>
<td>

Voici un exemple de **[InkToolbar](/uwp/api/windows.ui.xaml.controls.inktoolbar)** personnalisé (à partir de Sketchpad dans l’espace de travail Windows Ink).

![Capture d’écran de la barre d’outils Ink de Sketchpad dans l’espace de travail Ink.](images/ink/ink-inktoolbar-sketchpad-small.png)

Pour plus d’informations sur la personnalisation d’un [InkToolbar](/uwp/api/windows.ui.xaml.controls.inktoolbar), consultez [Ajouter un InkToolbar à une application d’écriture manuscrite Windows](ink-toolbar.md).

</td>
</tr>
</table>

## <a name="step-5-support-handwriting-recognition"></a>Étape 5 : prendre en charge la reconnaissance de l’écriture manuscrite

Maintenant que vous pouvez écrire et dessiner dans votre application, essayons d’effectuer une opération utile avec ces griffonnages.

Dans cette étape, nous utilisons les fonctionnalités de reconnaissance de l’écriture manuscrite de Windows Ink pour essayer de déchiffrer ce que vous avez écrit.

> [!NOTE]
> La reconnaissance de l’écriture manuscrite peut être améliorée par le biais du **stylet & paramètres d’encre Windows** :
> 1. Ouvrez le menu Démarrer et sélectionnez **paramètres**.
> 2. Dans l’écran Paramètres, sélectionnez **appareils**  >  **stylet & Windows Ink**.
> ![Capture d’écran de la & page Paramètres du stylet Windows Ink.](images/ink/ink-settings-small.png)
> 3. Sélectionnez **obtenir pour connaître mon écriture manuscrite** pour ouvrir la boîte de dialogue Personnalisation de l' **écriture manuscrite** .
> ![Capture d’écran de la boîte de dialogue Personnalisation de la reconnaissance de l’écriture manuscrite.](images/ink/ink-settings-handwritingpersonalization-small.png)

### <a name="in-the-sample"></a>Dans l'exemple :
1. Ouvrez le fichier MainPage. Xaml.
2. Recherchez le code marqué avec le titre de cette étape (« \<!-- Step 5: Support handwriting recognition --> »).
3. Supprimez les marques de commentaire des lignes suivantes.  

``` xaml
    <Button x:Name="recognizeText" 
            Content="Recognize text"  
            Grid.Row="0" Grid.Column="0"
            Margin="10,10,10,10"
            Click="recognizeText_ClickAsync"/>
    <TextBlock x:Name="recognitionResult" 
                Text="Recognition results: "
                VerticalAlignment="Center" 
                Grid.Row="0" Grid.Column="1"
                Margin="50,0,0,0" />
```

4. Ouvrez le fichier MainPage.xaml.cs.
5. Recherchez le code marqué avec le titre de cette étape (« étape 5 : prendre en charge la reconnaissance de l’écriture manuscrite »).
6. Supprimez les marques de commentaire des lignes suivantes.  

- Il s’agit des variables globales requises pour cette étape.

``` csharp
    InkAnalyzer analyzerText = new InkAnalyzer();
    IReadOnlyList<InkStroke> strokesText = null;
    InkAnalysisResult resultText = null;
    IReadOnlyList<IInkAnalysisNode> words = null;
```

- Il s’agit du gestionnaire du bouton de reconnaissance de **texte** , dans lequel nous procédons au traitement de la reconnaissance.

``` csharp
    private async void recognizeText_ClickAsync(object sender, RoutedEventArgs e)
    {
        strokesText = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
        // Ensure an ink stroke is present.
        if (strokesText.Count > 0)
        {
            analyzerText.AddDataForStrokes(strokesText);

            resultText = await analyzerText.AnalyzeAsync();

            if (resultText.Status == InkAnalysisStatus.Updated)
            {
                words = analyzerText.AnalysisRoot.FindNodes(InkAnalysisNodeKind.InkWord);
                foreach (var word in words)
                {
                    InkAnalysisInkWord concreteWord = (InkAnalysisInkWord)word;
                    foreach (string s in concreteWord.TextAlternates)
                    {
                        recognitionResult.Text += s;
                    }
                }
            }
            analyzerText.ClearDataForAllStrokes();
        }
    }
```

7. Exécutez à nouveau l’application, écrivez un contenu, puis cliquez sur le bouton de **reconnaissance de texte**
8. Les résultats de la reconnaissance s’affichent à côté du bouton

### <a name="challenge-1-international-recognition"></a>Challenge 1 : reconnaissance internationale
<table class="wdg-noborder">
<tr>
<td>

:::image type="icon" source="images/challenge-icon.png":::

</td>
<td>

Windows Ink prend en charge la reconnaissance de texte pour la plupart des langues prises en charge par Windows. Chaque module linguistique comprend un moteur de reconnaissance de l’écriture manuscrite qui peut être installé avec le module linguistique.

Ciblez une langue spécifique en interrogeant les moteurs de reconnaissance de l’écriture manuscrite installés.

Pour plus d’informations sur la reconnaissance de l’écriture manuscrite internationale, consultez [reconnaître les traits d’encre Windows en tant que texte](convert-ink-to-text.md).

</td>
</tr>
</table>

### <a name="challenge-2-dynamic-recognition"></a>Challenge 2 : reconnaissance dynamique
<table class="wdg-noborder">
<tr>
<td>

:::image type="icon" source="images/challenge-icon.png":::

</td>
<td>

Pour ce didacticiel, vous devez appuyer sur un bouton pour lancer la reconnaissance. Vous pouvez également effectuer la reconnaissance dynamique à l’aide d’une fonction de minutage de base.

Pour plus d’informations sur la reconnaissance dynamique, consultez [reconnaître les traits d’encre Windows en tant que texte](convert-ink-to-text.md).

</td>
</tr>
</table>

## <a name="step-6-recognize-shapes"></a>Étape 6 : reconnaître les formes

OK, vous pouvez maintenant convertir vos notes manuscrites en quelque chose d’un peu plus lisible. Mais qu’en est-il de la réunion anonyme de vos diagrammes de Caffeinated Doodles ?

À l’aide de l’analyse de l’encre, votre application peut également reconnaître un ensemble de formes principales, notamment :

- Circle
- Losange
- Dessin
- Ellipse
- EquilateralTriangle
- Hexagone
- IsoscelesTriangle
- Parallélogramme
- Droite
- Quadrangulaires
- Rectangle
- RightTriangle
- Carré
- Trapèze
- Triangle

Dans cette étape, nous utilisons les fonctionnalités de reconnaissance de forme de Windows Ink pour essayer de nettoyer vos Doodles.

Pour cet exemple, nous n’essayons pas de redessiner les traits d’encre (bien que cela soit possible). Au lieu de cela, nous ajoutons un canevas standard sous l’InkCanvas où nous dessinons des objets ellipse ou Polygone équivalents dérivés de l’encre d’origine. Nous supprimons ensuite les traits d’encre correspondants.

### <a name="in-the-sample"></a>Dans l'exemple :
1. Ouvrir le fichier MainPage. Xaml
2. Rechercher le code marqué avec le titre de cette étape (« \<!-- Step 6: Recognize shapes --> »)
3. Supprimez les marques de commentaire de cette ligne.  

``` xaml
   <Canvas x:Name="canvas" />

   And these lines.

    <Button Grid.Row="1" x:Name="recognizeShape" Click="recognizeShape_ClickAsync"
        Content="Recognize shape" 
        Margin="10,10,10,10" />
```

4. Ouvrir le fichier MainPage.xaml.cs
5. Recherchez le code marqué avec le titre de cette étape (« //étape 6 : reconnaître les formes »)
6. Supprimer les marques de commentaire de ces lignes :  

``` csharp
    private async void recognizeShape_ClickAsync(object sender, RoutedEventArgs e)
    {
        ...
    }

    private void DrawEllipse(InkAnalysisInkDrawing shape)
    {
        ...
    }

    private void DrawPolygon(InkAnalysisInkDrawing shape)
    {
        ...
    }
```

7. Exécutez l’application, dessinez des formes, puis cliquez sur le bouton **reconnaître la forme**

Voici un exemple d’organigramme rudimentaire à partir d’une serviette numérique.

![Capture d’écran d’un organigramme rudimentaire à partir d’une serviette numérique.](images/ink/ink-app-step6-shapereco1-small.png)

Voici le même organigramme après reconnaissance de la forme.

![Capture d’écran de l’organigramme après que l’utilisateur a sélectionné reconnaître la forme.](images/ink/ink-app-step6-shapereco2-small.png)


## <a name="step-7-save-and-load-ink"></a>Étape 7 : enregistrer et charger de l’encre

C’est pourquoi vous avez terminé gribouillais et que vous aimez ce que vous voyez, mais pensez peut-être à en modifier quelques-unes plus tard ? Vous pouvez enregistrer vos traits d’encre dans un fichier ISF (Ink Serialized Format) et les charger en vue de les modifier chaque fois que l’inspiration frappe. 

Le fichier ISF est une image GIF de base qui contient des métadonnées supplémentaires qui décrivent les propriétés et les comportements des traits d’encre. Les applications qui ne sont pas activées pour l’encre peuvent ignorer les métadonnées supplémentaires et continuer à charger l’image GIF de base (y compris la transparence d’arrière-plan de canal alpha).

Au cours de cette étape, nous allons raccorder les boutons **Save** et **Load** situés à côté de la barre d’outils Ink.

### <a name="in-the-sample"></a>Dans l'exemple :
1. Ouvrez le fichier MainPage. Xaml.
2. Recherchez le code marqué avec le titre de cette étape (« \<!-- Step 7: Saving and loading ink --> »).
3. Supprimez les marques de commentaire des lignes suivantes. 

``` xaml
    <Button x:Name="buttonSave" 
            Content="Save" 
            Click="buttonSave_ClickAsync"
            Width="100"
            Margin="5,0,0,0"/>
    <Button x:Name="buttonLoad" 
            Content="Load"  
            Click="buttonLoad_ClickAsync"
            Width="100"
            Margin="5,0,0,0"/>
```

4. Ouvrez le fichier MainPage.xaml.cs.
5. Recherchez le code marqué avec le titre de cette étape (« //étape 7 : enregistrer et charger l’encre »).
6. Supprimez les marques de commentaire des lignes suivantes.  

``` csharp
    private async void buttonSave_ClickAsync(object sender, RoutedEventArgs e)
    {
        ...
    }

    private async void buttonLoad_ClickAsync(object sender, RoutedEventArgs e)
    {
        ...
    }
```

7. Exécutez l’application et dessinez un texte.
8. Sélectionnez le bouton **Enregistrer** et enregistrez votre dessin.
9. Effacez l’encre ou redémarrez l’application.
10. Sélectionnez le bouton **charger** et ouvrez le fichier manuscrit que vous venez d’enregistrer.

### <a name="challenge-use-the-clipboard-to-copy-and-paste-ink-strokes"></a>Défi : utiliser le presse-papiers pour copier et coller des traits d’encre 
<table class="wdg-noborder">
<tr>
<td>

:::image type="icon" source="images/challenge-icon.png":::

</td>

<td>

Windows Ink prend également en charge la copie et le collage de traits d’encre vers et depuis le presse-papiers.

Pour plus d’informations sur l’utilisation du presse-papiers avec l’encre, consultez [stocker et récupérer des données de trait d’encre Windows](save-and-load-ink.md).

</td>
</tr>
</table>

## <a name="summary"></a>Résumé

Félicitations, vous avez terminé le didacticiel **d’entrée : prise en charge manuscrite dans votre application Windows** ! Nous vous avons montré le code de base requis pour la prise en charge de l’encre dans vos applications Windows, et comment fournir certaines des expériences utilisateur plus riches prises en charge par la plateforme Windows Ink.

## <a name="related-articles"></a>Articles connexes

* [Interactions avec le stylet et Windows Ink dans les applications Windows](pen-and-stylus-interactions.md)

### <a name="samples"></a>Exemples

* [Exemple d’analyse de l’encre (Basic) (C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-analysis-basic.zip)
* [Exemple de reconnaissance de l’écriture manuscrite (C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-handwriting-reco.zip)
* [Enregistrer et charger des traits d’encre à partir d’un fichier ISF (Ink Serialized Format)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store.zip)
* [Enregistrer et charger des traits manuscrits à partir du presse-papiers](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store-clipboard.zip)
* [Exemple d’emplacement et d’orientation de la barre d’outils encre (de base)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness.zip)
* [Exemple d’emplacement et d’orientation de la barre d’outils Ink (dynamique)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness-dynamic.zip)
* [Exemple d’encre simple (C#/C + +)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)
* [Exemple d’encre complexe (C++)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)
* [Ink, exemple (JavaScript)](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BJavaScript%5D-Windows%208%20app%20samples/JavaScript/Windows%208%20app%20samples/Input%20Ink%20sample%20(Windows%208))
* [Didacticiel de prise en main : écriture manuscrite dans votre application Windows](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-Ink)
* [Exemple de livre de coloriage](https://github.com/Microsoft/Windows-appsample-coloringbook)
* [Exemple de notes de famille](https://github.com/Microsoft/Windows-appsample-familynotes)