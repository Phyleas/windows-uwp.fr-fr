---
author: kbridge
ms.assetid: 
title: "Entrée manuscrite de prise en charge UWP Academy - Suivi des saisies dans votre application UWP"
description: "UWP Academy. Suivi des saisies. Un didacticiel détaillé pour ajouter la prise en charge de l’entrée manuscrite à votre application UWP."
keywords: UWP Academy
ms.author: kbridge
ms.date: 04/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: a70ac88773907edb7e5d02708be19fb9feec01a9
ms.sourcegitcommit: 7540962003b38811e6336451bb03d46538b35671
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/26/2017
---
# <a name="support-ink-in-your-uwp-app"></a>Entrée manuscrite prise en charge dans votre application UWP

![Stylet Surface](images/ink/ink-hero-small.png)  
*Stylet Surface* (disponible à l’achat dans la [Microsoft Store](https://aka.ms/purchasesurfacepen)).

Ce didacticiel explique comment créer une application de plateforme Windows universelle (UWP) basique, qui prend en charge l'écriture et le dessin avec Windows Ink. Nous utilisons des extraits de code à partir d’un exemple d’application, que vous pouvez télécharger à partir de GitHub (voir [Exemple de code](#sample-code)), pour illustrer les diverses fonctionnalités et les API Windows Ink associées (voir [Composants de la plateforme Windows Ink](#components-of-the-windows-ink-platform)) décrites dans chaque étape.

Nous nous concentrons sur les éléments suivants:
* Ajout de la prise en charge de l’entrée manuscrite de base
* Ajout d’une barre d’outils d'entrée manuscrite
* Prise en charge de la reconnaissance de l’écriture manuscrite
* Prise en charge de la reconnaissance des formes de base
* Enregistrement et chargement de l’entrée manuscrite

Pour plus d’informations sur l’implémentation de ces fonctionnalités, voir [Interactions du stylet et Windows Ink dans les applications UWP](https://docs.microsoft.com/windows/uwp/input-and-devices/pen-and-stylus-interactions).

## <a name="introduction"></a>Introduction

Avec Windows Ink, vous pouvez fournir à vos clients l’équivalent numérique de quasiment n’importe quelle expérience manuscrite imaginable, depuis des notes manuscrites rapides et des annotations dans des démonstrations sur tableau blanc, en passant par des croquis techniques et architecturaux ou encore pour vos travaux personnels.

## <a name="prerequisites"></a>Prérequis

* Un ordinateur (ou un appareil virtuel) exécutant la version actuelle de Windows 10
* [Visual Studio2017 et RS2 SDK](https://developer.microsoft.com/windows/downloads)
* [SDK Windows 10 (10.0.15063.0)](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk)
* Si vous débutez dans le développement d’applications de plateforme Windows universelle (UWP) avec Visual Studio, consultez les rubriques suivantes avant de démarrer ce didacticiel:  
    * [Préparation](https://docs.microsoft.com/en-us/windows/uwp/get-started/get-set-up)
    * [Créer une application «Hello World» (XAML)](https://docs.microsoft.com/en-us/windows/uwp/get-started/create-a-hello-world-app-xaml-universal)
* **[FACULTATIF] **Un stylet numérique et un ordinateur avec un écran prenant en charge les entrées de ce stylet numérique.  
   > [!NOTE] 
   > Windows Ink peut prendre en charge le dessin avec une souris et des fonctions tactiles (nous expliquons la marche à suivre pour ce faire à l’étape3 de ce didacticiel), toutefois, pour une expérience Windows Ink optimale, nous vous recommandons de disposer d'un stylet numérique et d'un ordinateur avec un écran prenant en charge ce stylet numérique.

## <a name="sample-code"></a>Exemple de code
Tout au long de ce didacticiel, nous utilisons un exemple d’application d’entrée manuscrite pour illustrer les concepts et les fonctionnalités décrites.

Téléchargez cet exemple Visual Studio et ce code source à partir de [GitHub](https://github.com/) à [windows-appsample-get-démarré-exemple d’entrée manuscrite](https://aka.ms/appsample-ink):

1. Sélectionnez le bouton vert **Clonage ou téléchargement**  
![Clonage du référentiel](images/ink/ink-clone.png)
2. Si vous avez un compte GitHub, vous pouvez cloner le référentiel sur votre ordinateur local en sélectionnant **Ouvrir dans Visual Studio** 
3. Si vous n’avez pas de compte GitHub, ou si vous souhaitez simplement obtenir une copie locale du projet, sélectionnez **Télécharger le fichier ZIP** (vous devrez régulièrement vous assurer de télécharger les dernières mises à jour)

> [!IMPORTANT]
> La plupart du code de l’exemple est commenté. À chaque étape, vous serez invité à annuler les commentaires des différentes sections du code. Dans Visual Studio, mettez simplement en surbrillance les lignes de code et appuyez sur CTRL-K, puis sur CTRL + U.

## <a name="components-of-the-windows-ink-platform"></a>Composants de la plateforme Windows Ink

Ces objets fournissent la majeure partie de l’expérience d’entrée manuscrite pour les applications UWP.

| Composant | Description |
| --- | --- |
| [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) | Un contrôle de plateforme d’interface utilisateur XAML, qui reçoit et affiche par défaut toutes les entrées à partir d’un stylet comme un trait d’encre ou un trait d’effacement. |
| [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) | Un objet code-behind, instancié avec un contrôle [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) (exposé par le biais de la propriété [**InkCanvas.InkPresenter**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.inkcanvas#Windows_UI_Xaml_Controls_InkCanvas_InkPresenter)). Cet objet fournit toutes les fonctionnalités d’entrée manuscrite par défaut exposées par l’élément [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas), ainsi qu’un ensemble complet d’API pour plus de personnalisation. |
| [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx) | Une plateforme d'interface utilisateur XAML contenant une collection extensible et personnalisable de boutons activant des fonctionnalités d’entrée manuscrite dans un [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) associé. |
| [**IInkD2DRenderer**](https://msdn.microsoft.com/library/mt147263)<br/>Nous n'abordons pas cette fonctionnalité ici; pour plus d’informations, consultez [Exemple d’entrée manuscrite complexe](http://go.microsoft.com/fwlink/p/?LinkID=620314). | Permet le rendu des traits d’encre sur le contexte d’appareil Direct2D désigné d’une application Windows universelle, au lieu du contrôle [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) par défaut. |

## <a name="step-1-run-the-sample"></a>Étape1: exécution de l’exemple

Une fois que vous avez téléchargé l’exemple d’application RadialController, vérifiez qu’elle s’exécute:
1. Ouvrez l’exemple de projet dans Visual Studio.
2. Définissez la liste déroulante **Plateformes de solution** sur une sélection non ARM.
3. Appuyez sur F5 pour compiler, déployer et exécuter.  

   > [!NOTE]
   > Vous pouvez également sélectionner l'élément de menu **Déboguer** > **Démarrer le débogage** ou sélectionnez le bouton **Ordinateur local** illustré ici.
   > ![Bouton de projet Visual Studio Build](images/ink/ink-vsrun.png)

Ouvre la fenêtre d’application, et une fois qu'un écran de démarrage se sera affiché pendant quelques secondes, vous verrez cet écran initial.

![Application vide](images/ink/ink-app-step1-empty.png)

OK, nous vous proposons désormais l’application UWP de base que nous allons utiliser tout au long de ce didacticiel. Dans les étapes suivantes, nous ajouterons nos fonctionnalités d’entrée manuscrite.

## <a name="step-2-use-inkcanvas-to-support-basic-inking"></a>Étape2: Utilisez InkCanvas pour prendre en charge l’entrée manuscrite de base

Vous avez probablement déjà remarqué que l’application, dans sa forme initiale, ne vous permet pas de dessiner avec le stylet (bien que vous puissiez utiliser le stylet comme périphérique de pointage standard pour interagir avec l’application). 

Nous allons remédier à cette lacune au cours de cette étape.

Pour ajouter des fonctionnalités d’écriture manuscrite de base, il suffit de placer une commande de plateforme UWP [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) sur la page appropriée dans votre application.

> [!NOTE]
> Un InkCanvas a des propriétés par défaut [**Hauteur**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_Height) et [**Largeur**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_Width) égales à zéro, sauf s’il s'agit de l’enfant d’un élément qui dimensionne automatiquement ses éléments enfants. 

### <a name="in-the-sample"></a>Dans l’exemple:
1. Ouvrez le fichier MainPage.xaml.cs.
2. Recherchez le code marqué avec le titre de cette étape («/ / Étape2: Utilisation d'InkCanvas pour prendre en charge l’entrée manuscrite de base»).
3. Supprimez les commentaires des lignes suivantes. (Ces références sont requises pour la fonctionnalité utilisée dans les étapes suivantes).  

``` csharp
    using Windows.UI.Input.Inking;
    using Windows.UI.Input.Inking.Analysis;
    using Windows.UI.Xaml.Shapes;
    using Windows.Storage.Streams;
```

4. Ouvrez le fichier MainPage.xaml.
5. Recherchez le code marqué avec le titre de cette étape («\<!--Étape2: Entrée manuscrite de base avec InkCanvas-->»).
6. Supprimez les commentaires de la ligne suivante.  

``` xaml
    <InkCanvas x:Name="inkCanvas" />
```

Et voilà! 

Exécutez de nouveau l’application. Lancez-vous et dessinez, écrivez votre nom, ou (si vous êtes face à un miroir ou si vous avez une très bonne mémoire) esquissez votre auto-portrait.

![Entrée manuscrite de base](images/ink/ink-app-step1-name.png)

## <a name="step-3-support-inking-with-touch-and-mouse"></a>Étape3: Prise en charge de l’entrée manuscrite avec une interaction tactile et la souris

Vous remarquerez que, par défaut, l'entrée manuscrite est prise en charge uniquement pour la saisie effectuée à l'aide du stylet. Si vous tentez d’écrire ou de dessiner avec votre doigt, votre souris ou votre pavé tactile, vous serez déçu.

Pour remédier à cela, vous devez ajouter une deuxième ligne de code. Cette fois, celle-ci sera dans le code-behind pour le fichier XAML dans lequel vous avez déclaré votre [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas). 

Dans cette étape, nous allons présenter l'objet [**InkPresenter**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.inkpresenter), qui assure une gestion plus fine de l’entrée, du traitement et du rendu des entrées manuscrites (standard ou modifiées) sur votre [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas).

> [!NOTE]
> L’entrée manuscrite standard (mine du stylet ou mine/bouton de la gomme) n’est pas modifiée avec une affordance matérielle secondaire, tel qu’un bouton gomme du stylet, le bouton droit de la souris ou un mécanisme similaire. 

Pour activer la souris et l'entrée manuscrite tactile, définissez la propriété [**InputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter#Windows_UI_Input_Inking_InkPresenter_InputDeviceTypes) de [**InkPresenter**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.inkpresenter) sur la combinaison de valeurs [**CoreInputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.core.coreinputdevicetypes) de votre choix.

### <a name="in-the-sample"></a>Dans l’exemple:
1. Ouvrez le fichier MainPage.xaml.cs.
2. Recherchez le code marqué avec le titre de cette étape («//Étape3: Prise en charge de l’entrée manuscrite avec une interaction tactile et la souris»).
3. Supprimez les commentaires des lignes suivantes.  

``` csharp
    inkCanvas.InkPresenter.InputDeviceTypes =
        Windows.UI.Core.CoreInputDeviceTypes.Mouse | 
        Windows.UI.Core.CoreInputDeviceTypes.Touch | 
        Windows.UI.Core.CoreInputDeviceTypes.Pen;
```

Exécutez de nouveau l’application et vous constaterez que tous vos rêves d'esquisses réalisées avec vos doigts sur l'écran de votre ordinateur sont devenus réalité!

> [!NOTE]
> Lorsque vous spécifiez les types de périphérique d’entrée, vous devez indiquer la prise en charge pour chaque type d'entrée spécifique (y compris le stylet), car cette propriété remplace le paramètre par défaut [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas).

## <a name="step-4-add-an-ink-toolbar"></a>Étape4: Ajouter une barre d’outils d'entrée manuscrite

L'élément [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) est un contrôle de plateforme UWP qui fournit un ensemble extensible et personnalisable de boutons en vue de l’activation des fonctionnalités relatives à l’entrée manuscrite. 

Par défaut, [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) inclut un ensemble de boutons de base, qui permet aux utilisateurs de faire rapidement leur choix entre un stylet, un crayon, un surligneur ou une gomme, lesquels peuvent tous être utilisés avec un gabarit (règle ou rapporteur). Les boutons de stylet, crayon et surligneur fournissent également un menu volant pour sélectionner la couleur et le trait d’encre.

Pour ajouter une valeur par défaut [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) à une application d’entrée manuscrite, placez-la simplement sur la même page que votre [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) et associez les deux contrôles.

### <a name="in-the-sample"></a>Dans l’exemple
1. Ouvrez le fichier MainPage.xaml.
2. Recherchez le code marqué avec le titre de cette étape («\<!-- Étape4: ajouter une barre d’outils encre-->»).
3. Supprimez les commentaires des lignes suivantes.  

``` xaml
    <InkToolbar x:Name="inkToolbar" 
                        VerticalAlignment="Top" 
                        Margin="10,0,10,0"
                        TargetInkCanvas="{x:Bind inkCanvas}">
    </InkToolbar>
```

> [!NOTE]
> Pour faire en sorte que l’interface utilisateur et le code restent les plus aérés et simples possible, nous utilisons une disposition de grille de base et déclarons [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) après [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) dans une ligne de grille. Si vous la déclarez avant [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas), [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) s'affiche en premier, sous la zone de dessin et est inaccessible à l’utilisateur.  

Exécutez de nouveau l’application pour voir [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) et essayer certains des outils.

![InkToolbar à partir du bloc-croquis dans l’espace de travail Windows Ink](images/ink/ink-inktoolbar-default.png)

### <a name="challenge-add-a-custom-button"></a>Défi: Ajouter un bouton personnalisé
<table class="wdg-noborder">
<tr>
 <td width=125><img src="images/challenge-icon.png"></td>
    <td width=500>Voici un exemple d'<strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar">InkToolbar</a></strong> personnalisée (à partir du bloc-croquis dans l’espace de travail Windows Ink).

![InkToolbar à partir du bloc-croquis dans l’espace de travail Windows Ink](images/ink/ink-inktoolbar-sketchpad.png)

Pour en savoir plus sur la personnalisation d'une [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar), consultez [Ajouter un élément InkToolbar à une application d’entrée manuscrite de plateforme Windows universelle (UWP)](https://docs.microsoft.com/en-us/windows/uwp/input-and-devices/ink-toolbar).

</tr>
</table>

## <a name="step-5-support-handwriting-recognition"></a>Étape5: Prise en charge de la reconnaissance de l’écriture manuscrite

Maintenant que vous pouvez écrire et dessiner dans votre application, nous allons essayer de rendre ces dessins utiles.

Dans cette étape, nous utilisons les fonctionnalités de reconnaissance de l’écriture manuscrite de Windows Ink pour tenter de déchiffrer ce que vous avez écrit.

> [!NOTE]
> La reconnaissance de l’écriture manuscrite peut être améliorée via les paramètres **Stylet et Windows Ink**:
> 1. Ouvrez le menu Démarrer et sélectionnez **Paramètres**.
> 2. À partir de l’écran Paramètres, sélectionnez **Périphériques** > **Stylet et Windows Ink**.
> ![InkToolbar à partir du bloc-croquis dans l’espace de travail Windows Ink](images/ink/ink-settings-large.png)
> 3. Sélectionnez **Apprendre mon écriture manuscrite** pour ouvrir la boîte de dialogue **Personnalisation de l’écriture manuscrite**.
> ![InkToolbar à partir du bloc-croquis dans l’espace de travail Windows Ink](images/ink/ink-settings-handwritingpersonalization-large.png)

### <a name="in-the-sample"></a>Dans l’exemple:
1. Ouvrez le fichier MainPage.xaml.
2. Recherchez le code marqué avec le titre de cette étape («\<!--Étape5: Prise en charge de la reconnaissance de l'écriture manuscrite -->»).
3. Supprimez les commentaires des lignes suivantes.  

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
5. Recherchez le code marqué avec le titre de cette étape («Étape5: Prise en charge de la reconnaissance de l'écriture manuscrite»).
6. Supprimez les commentaires des lignes suivantes.  

- Voici les variables globales requises pour cette étape.

``` csharp
    InkAnalyzer analyzerText = new InkAnalyzer();
    IReadOnlyList<InkStroke> strokesText = null;
    InkAnalysisResult resultText = null;
    IReadOnlyList<IInkAnalysisNode> words = null;
```

- Il s'agit du gestionnaire pour le bouton **Reconnaître le texte** qui permet de procéder au traitement de la reconnaissance.

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

7. Exécutez de nouveau l’application, écrivez quelque chose, puis cliquez sur le bouton **Reconnaître le texte**
8. Les résultats de la reconnaissance s'affichent en regard du bouton

### <a name="challenge-1-international-recognition"></a>Défi 1: Reconnaissance internationale
<table class="wdg-noborder">
<tr>
 <td width=125><img src="images/challenge-icon.png"></td>
    <td width=500><p>Windows Ink prend en charge la reconnaissance de texte pour la plupart des langues prises en charge par Windows. Chaque module linguistique inclut un moteur de reconnaissance de l’écriture manuscrite qui peut être installé avec le module linguistique.</p>
    <p>Ciblez une langue spécifique en interrogeant les moteurs de reconnaissance de l’écriture manuscrite installés.</p>
    <p>Pour plus d’informations sur la reconnaissance de l'écriture manuscrite internationale, voir <a href="https://docs.microsoft.com/windows/uwp/input-and-devices/convert-ink-to-text">Reconnaître les traits Windows Ink en tant que texte</a>.</p>
</tr>
</table>

### <a name="challenge-2-dynamic-recognition"></a>Challenge 2: Reconnaissance dynamique
<table class="wdg-noborder">
<tr>
 <td width=125><img src="images/challenge-icon.png"></td>
    <td width=500><p>Pour ce didacticiel, nous avons besoin d'appuyer sur un bouton pour lancer la reconnaissance. Vous pouvez également effectuer la reconnaissance dynamique à l’aide d’une fonction de chronométrage de base.</p>

Pour plus d'informations sur la reconnaissance dynamique, voir [Reconnaître les traits Windows Ink en tant que texte](https://docs.microsoft.com/en-us/windows/uwp/input-and-devices/convert-ink-to-text).
</tr>
</table>

## <a name="step-6-recognize-shapes"></a>Étape6: Reconnaître les formes

OK, maintenant vous pouvez transformer vos notes manuscrites en quelque chose d’un peu plus lisible. Mais qu’en est-il de ces gribouillis tremblotants et alimentés à la caféine dans vos notes de la réunion de ce matin?

À l’aide de l’analyse de l’entrée manuscrite, votre application peut également reconnaître un ensemble de formes, notamment:
- Cercle
- Losange
- Dessin
- Ellipse
- Triangle équilatéral
- Hexagone
- Triangle isocèle
- Parallélogramme
- Pentagone
- Quadrilatère
- Rectangle
- Triangle à angle droit
- Carré
- Trapèze
- Triangle

Dans cette étape, nous utilisons les fonctionnalités de reconnaissance des formes de Windows Ink pour épurer vos griffonnages.

Pour cet exemple, nous ne tentons pas de redessiner des traits d’encre (bien que cela soit possible). Au lieu de cela, nous ajoutons un canevas standard sous l’élément InkCanvas, où nous dessinons des objets Ellipse ou Polygone équivalents, dérivés de l’entrée manuscrite d’origine. Nous supprimons ensuite les traits d’encre correspondant.

### <a name="in-the-sample"></a>Dans l’exemple:
1. Ouvrez le fichier MainPage.xaml
2. Recherchez le code marqué avec le titre de cette étape («\<!--Étape6: Reconnaître les formes-->»)
3. Supprimez le commentaire cette ligne.  

``` xaml
   <Canvas x:Name="canvas" />

   And these lines.

    <Button Grid.Row="1" x:Name="recognizeShape" Click="recognizeShape_ClickAsync"
        Content="Recognize shape" 
        Margin="10,10,10,10" />
```

4. Ouvrez le fichier MainPage.xaml.cs
5. Recherchez le code marqué avec le titre de cette étape («//Étape6: Reconnaître les formes»)
6. Supprimez les commentaires de ces lignes:  

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

7. Exécutez l’application, dessinez des formes, puis cliquez sur le bouton **Reconnaître la forme**

Voici un exemple d'organigramme rudimentaire provenant d'une serviette numérique.

![Organigramme d’entrée manuscrite d’origine](images/ink/ink-app-step6-shapereco1.png)

Voici le même organigramme après la reconnaissance de forme.

![Organigramme d’entrée manuscrite d’origine](images/ink/ink-app-step6-shapereco2.png)


## <a name="step-7-save-and-load-ink"></a>Étape7: Enregistrer et charger l’entrée manuscrite

Bien, vous avez terminé de griffonner et vous êtes content de vous, toutefois vous pensez modifier quelques petites choses ultérieurement? Vous pouvez enregistrer vos traits d’encre dans un fichier au format ISF (Ink Serialized Format) et les charger afin de les modifier à chaque fois que vous vous sentez inspiré. 

Le fichierISF est une imageGIF basique contenant des métadonnées supplémentaires décrivant les comportements et les propriétés de traits d’encre. Les applications ne prenant pas en charge les entrées manuscrites peuvent ignorer les métadonnées supplémentaires et charger l’image GIF base (y compris la transparence d’arrière-plan canal alpha).

Dans cette étape, nous connectons les boutons **Enregistrer** et **Charger**, situés en regard de la barre d’outils des entrées manuscrites.

### <a name="in-the-sample"></a>Dans l’exemple:
1. Ouvrez le fichier MainPage.xaml.
2. Recherchez le code marqué avec le titre de cette étape («\<!--Étape7: enregistrement et chargement d’encre-->»).
3. Supprimez les commentaires des lignes suivantes. 

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
5. Recherchez le code marqué avec le titre de cette étape («// --Étape7: Enregistrement et chargement d’encre»).
6. Supprimez les commentaires des lignes suivantes.  

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

7. Exécutez l’application et dessinez quelque chose.
8. Sélectionnez le bouton **Enregistrer** et enregistrez votre dessin.
9. Effacez l’entrée manuscrite ou redémarrez l’application.
10. Sélectionnez le bouton**Charger** et ouvrez le fichier d’entrée manuscrite que vous venez d’enregistrer.

### <a name="challenge-use-the-clipboard-to-copy-and-paste-ink-strokes"></a>Test: Utiliser le presse-papiers pour copier et coller des traits d’encre 
<table class="wdg-noborder">
<tr>
 <td width=125><img src="images/challenge-icon.png"></td>
    <td width=500><p>Windows Ink prend également en charge les fonctions copier et coller des traits d’encre vers et depuis le presse-papiers.

Pour plus d’informations sur l’utilisation du Presse-papiers avec l'entrée manuscrite, consultez [Stocker et récupérer les données de traits WindowsInk](https://docs.microsoft.com/en-us/windows/uwp/input-and-devices/save-and-load-ink).
</tr>
</table>

## <a name="summary"></a>Résumé

Félicitations, vous venez de terminer le didacticiel **Entrée: Prise en charge des entrées manuscrites dans votre application UWP**! Nous vous avons indiqué le code de base requis pour prendre en charge les entrées manuscrites dans vos applications UWP, et comment fournir certaines expériences utilisateur enrichies, prises en charge par la plateforme Windows Ink.

## <a name="related-articles"></a>Articles connexes

* [Interactions avec le stylet et WindowsInk dans les applications UWP](https://docs.microsoft.com/windows/uwp/input-and-devices/pen-and-stylus-interactions)

**Exemples**
* [Exemple d’entrée manuscrite simple (C#/C++)](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [Exemple d’entrée manuscrite complexe (C++)](http://go.microsoft.com/fwlink/p/?LinkID=620314)
* [Exemple d’entrée manuscrite (JavaScript)](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [Prise en main du didacticiel: prendre en charge l’entrée manuscrite dans votre application UWP](https://aka.ms/appsample-ink)
* [Exemple de livre de coloriage](https://aka.ms/cpubsample-coloringbook)
* [Exemple de notes de famille](https://aka.ms/cpubsample-familynotessample)