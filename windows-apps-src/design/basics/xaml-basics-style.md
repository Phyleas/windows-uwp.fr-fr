---
title: Créer des styles personnalisés
description: Suivez ce tutoriel pour apprendre à créer des styles personnalisés et des contrôles de curseur afin de personnaliser l’interface utilisateur de votre application XAML.
keywords: XAML, UWP, Bien démarrer
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 04357cc43ed227b109d4aab20c509077261360ee
ms.sourcegitcommit: 6cb20dca1cb60b4f6b894b95dcc2cc3a166165ad
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/01/2020
ms.locfileid: "91636599"
---
# <a name="tutorial-create-custom-styles"></a>Tutoriel : Créer des styles personnalisés

Ce tutoriel vous montre comment personnaliser l’interface utilisateur de notre application XAML. Avertissement : ce tutoriel peut impliquer ou non une licorne. (Ce n'est pas une blague !)

L’exemple d’application PhotoLab comporte deux pages. La _page principale_ présente une vue de galerie de photos ainsi que des informations sur chaque fichier image.

![Capture d’écran de la page principale du Laboratoire photo.](../basics/images/xaml-basics/mainpage.png)

La *page de détails* affiche une seule photo une fois qu’elle a été sélectionnée. Un menu d’édition volant permet de modifier la photo, de la renommer et de l’enregistrer.

![Capture d’écran de la page de détails du Laboratoire photo.](../basics/images/xaml-basics/detailpage.png)

## <a name="prerequisites"></a>Prérequis

+ Visual Studio 2019 : [Téléchargez Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) (l’édition Community est gratuite.)
+ SDK Windows 10 (10.0.17763.0 ou ultérieur) :  [Télécharger le SDK Windows le plus récent (gratuit)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
+ Windows 10, version 1809 ou ultérieure

## <a name="part-0-get-the-starter-code-from-github"></a>Partie 0 : Obtenir le code de démarrage à partir de GitHub

Dans ce tutoriel, vous allez commencer avec une version simplifiée de l’exemple PhotoLab.

1. Accédez à la page GitHub de l’exemple : [https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab).
2. Vous devez ensuite cloner ou télécharger l’exemple. Sélectionnez le bouton **Clone or download**. Un sous-menu s’affiche.
    ![Menu Clone or download dans la page GitHub de l’exemple PhotoLab](images/xaml-basics/clone-repo.png)

    **Si vous n’êtes pas familiarisé avec GitHub :**

    a. Sélectionnez **Télécharger le zip**, puis enregistrez le fichier localement. Cela télécharge un fichier .zip contenant tous les fichiers projet dont vous avez besoin.

    b. Extrayez le fichier. Utilisez l’Explorateur de fichiers pour accéder au fichier .zip que vous venez de télécharger, cliquez dessus avec le bouton droit, puis sélectionnez **Extraire tout**.

    c. Accédez à votre copie locale de l’exemple, puis au répertoire `Windows-appsample-photo-lab-master\xaml-basics-starting-points\style`.

    **Si vous maîtrisez GitHub :**

    a. Clonez localement la branche maîtresse du dépôt.

    b. Accédez au répertoire `Windows-appsample-photo-lab\xaml-basics-starting-points\style`.

3. Double-cliquez sur `Photolab.sln` pour ouvrir la solution dans Visual Studio.

## <a name="part-1-create-a-fancy-slider-control"></a>Partie 1 : Créer un contrôle de curseur fantaisie

L’application Windows fournit un certain nombre de méthodes pour personnaliser l’apparence de votre application. Des paramètres de polices et de typographie aux effets de flou, en passant par les couleurs et les dégradés, vous disposez d'un grand nombre d’options.

Pour la première partie de ce tutoriel, nous allons égayer quelques uns de nos contrôles d’édition de photos.

![Curseur modeste avec le style par défaut.](../basics/images/xaml-basics/slider-start.png)

Ces curseurs sont très bien, ils font tout ce qu'un curseur doit faire, mais ils ne sont pas très fantaisistes. Nous allons arranger ça.

Le curseur d’exposition ajuste l’exposition de l’image : faites-le glisser vers la gauche et l’image devient plus sombre ; faites-le glisser vers la droite et elle devient plus claire. Nous allons enjoliver notre curseur en lui attribuant un arrière-plan qui passe du noir au blanc. Cela rendra le curseur plus esthétique, ce qui est très bien, mais donnera également un indice visuel sur la fonctionnalité qu'il fournit.

Appuyez sur la touche F5 pour compiler et exécuter l’application. Le premier écran montre une galerie d’images. Cliquez sur une image pour accéder à la page de détails de l’image. Une fois que vous y êtes, cliquez sur le bouton Modifier pour voir les contrôles d’édition sur lesquels nous allons travailler. Quittez l’application et revenez à Visual Studio.

### <a name="customize-a-slider-control"></a>Personnaliser un contrôle de curseur

1. Dans le panneau de l’Explorateur de solutions, double-cliquez sur DetailPage.xaml pour l'ouvrir.

    ![Fichier DetailPage.xaml dans l’Explorateur de solutions de Visual Studio.](../basics/images/xaml-basics/style-detail-page-explorer.png)

1. Utilisez un élément `Polygon` pour créer une forme d’arrière-plan pour le curseur d’exposition.

    [L’espace de noms Windows.UI.Xaml.Shapes](/uwp/api/Windows.UI.Xaml.Shapes) fournit sept formes au choix. Vous pouvez choisir une ellipse, un rectangle, et un objet appelé Path, qui permet de réaliser toutes sortes de formes, et oui, même une licorne !

    ![Une licorne](../basics/images/xaml-basics/unicorn.png)

    > **En savoir plus :** L'article [Dessiner des formes](../controls-and-patterns/shapes.md) vous explique tout ce que vous devez savoir sur les formes XAML.

    Nous voulons créer un widget d'aspect triangulaire, qui ressemble à la forme dessinée sur le contrôle de volume d’une chaîne stéréo.

    ![Curseur de volume](../basics/images/xaml-basics/style-volume-slider.png)

    La forme `Polygon` devrait faire l’affaire ! Pour définir un polygone, vous spécifiez un ensemble de points et lui donnez un remplissage. Nous allons créer un polygone d'environ 200 pixels de largeur et de 20 pixels de hauteur, avec un remplissage dégradé.

    Dans DetailPage.xaml, recherchez le code du curseur d’exposition, puis créez un élément Polygon juste avant lui :

    * Définissez **Grid.Row** sur « 2 » pour placer le polygone sur la même ligne que le curseur d’exposition.
    * Attribuez à la propriété **Points** la valeur « 0,20 200,20 200,0 » pour définir la forme du triangle.
    * Attribuez à la propriété **Stretch** la valeur « Fill » et à la propriété **HorizontalAlignment** la valeur « Stretch ».
    * Attribuez à **Hauteur** la valeur « 20 » et à **VerticalAlignment** la valeur « Center ».
    * Donnez au **Polygon** un remplissage de dégradé linéaire.
    * Sur le curseur d'exposition, attribuez à la propriété **Foreground** la valeur « Transparent » afin de pouvoir voir le polygone.

    **Avant**

    ```xaml
    <Slider Header="Exposure"
        Grid.Row="2"
        Value="{x:Bind item.Exposure, Mode=TwoWay}"
        Minimum="-2"
        Maximum="2" />
    ```

    **Après**

    ```xaml
    <Polygon Grid.Row="2" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Black" />
                    <GradientStop Offset="1" Color="White" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Exposure"
        Grid.Row="2"
        Foreground="Transparent"
        Value="{x:Bind item.Exposure, Mode=TwoWay}"
        Minimum="-2"
        Maximum="2" />
    ```

    Remarques :
    * Si vous examinez le code XAML environnant, vous verrez que ces éléments se trouvent dans un objet `Grid`. Nous plaçons le polygone sur la même ligne que le curseur d’exposition (`Grid.Row="2"`) pour qu’ils apparaissent au même endroit. Nous plaçons le polygone avant le curseur pour que celui-ci s'affiche en haut de la forme.
    * Nous avons défini `Stretch="Fill"` et `HorizontalAlignment="Stretch"` sur le polygone afin que le triangle s’adapte pour remplir l’espace disponible. Si la largeur du curseur diminue ou augmente, le polygone rétrécit ou s'élargit en conséquence.

1. Compilez et exécutez l’application. Votre curseur doit désormais avoir un aspect remarquable :

    ![Curseur d'exposition fantaisie](../basics/images/xaml-basics/style-exposure-slider-done.png)

1. Nous allons améliorer le curseur suivant, le curseur de température. Le curseur de température change la température de couleur de l’image ; si vous le faites glisser vers la gauche, l’image est plus bleue et si vous le faites glisser vers la droite, l’image est plus jaune.

    Nous allons utiliser une autre polygone pour cette forme en arrière-plan avec les mêmes dimensions que le précédent, mais cette fois nous allons utiliser un remplissage dégradé bleu-jaune au lieu de noir et blanc.

    **Avant**

    ```xaml
    <TextBlock Grid.Row="2"
                Grid.Column="1"
                 Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />

    <Slider Header="Temperature"
            Grid.Row="3" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Temperature, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1" />
    ```

    **Après**

    ```xaml
    <TextBlock Grid.Row="2"
                Grid.Column="1"
                Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />

    <Polygon Grid.Row="3" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Blue" />
                    <GradientStop Offset="1" Color="Yellow" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Temperature"
            Grid.Row="3" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Temperature, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1" />
    ```

1. Compilez et exécutez l’application. Vous devez maintenant avoir deux curseurs fantaisie.

    ![Deux curseurs fantaisies](../basics/images/xaml-basics/style-2sliders-done.png)

1. **Exercice supplémentaire**

    Ajoutez une forme d’arrière-plan au curseur de teinte qui présente un dégradé du vert au rouge.

    ![Trois curseurs fantaisie](../basics/images/xaml-basics/style-3sliders-done.png)

Félicitations, vous avez terminé la partie 1 ! Si vous êtes bloqué ou que vous souhaitez consulter la solution définitive, vous trouverez le code terminé sur [https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab).

## <a name="part-2-create-basic-styles"></a>Partie 2 : Créer des styles de base

Un des avantages des styles XAML est qu’ils permettent de réduire considérablement la quantité de code que vous devez écrire et d'actualiser beaucoup plus facilement l’apparence de votre application.

Pour définir un style, vous ajoutez un élément [Style](/uwp/api/Windows.UI.Xaml.Style) à la propriété [Ressources](/uwp/api/windows.ui.xaml.frameworkelement.Resources) d’un élément qui contient le contrôle que vous souhaitez pour le style.  Si vous ajoutez votre style à la propriété `Page.Resources`, vos styles seront accessibles à la page entière. Si vous ajoutez votre style à la propriété `Application.Resources` dans votre fichier App.xaml, le style sera accessible à l’ensemble de l’application.

Vous pouvez créer des styles nommés et des styles généraux. Un style nommé doit être explicitement appliqué à des contrôles spécifiques ; un style général est appliqué à tout contrôle qui correspond au `TargetType` spécifié.

Dans cet exemple, `x:Key` est associé au premier style et le type cible de ce dernier est `Button`. La propriété `Style` du premier bouton est définie sur cette clé : le style est donc un style nommé et doit être appliqué explicitement. Le type cible du deuxième style est `Button` et aucun attribut `x:Key` n’est associé à ce dernier : le style est donc appliqué automatiquement au deuxième bouton.

```xaml
<Page.Resources>
    <Style x:Key="PurpleStyle" TargetType="Button">
        <Setter Property="FontFamily" Value="Lucida Sans Unicode"/>
        <Setter Property="FontStyle" Value="Italic"/>
        <Setter Property="FontSize" Value="14"/>
        <Setter Property="Foreground" Value="MediumOrchid"/>
    </Style>

    <Style TargetType="Button">
        <Setter Property="Foreground" Value="Orange"/>
    </Style>
</Page.Resources>

<Grid x:Name="LayoutRoot">
    <Button Content="Button" Style="{StaticResource PurpleStyle}"/>
    <Button Content="Button" />
</Grid>
```

Ajoutons un style à notre application. Dans DetailsPage.xaml, jetez un coup de œil aux blocs de texte qui se trouvent en regard de nos curseurs d'exposition, de température et de teinte. Chacun de ces blocs de texte affiche la valeur d’un curseur. Voici le bloc de texte du curseur d’exposition. Notez que les propriétés `Margin`, `VerticalAlignment` et `Padding` sont définies.

```xaml
<TextBlock Grid.Row="2"
            Grid.Column="1"
            Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
            Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />
```

Examinez les autres blocs de texte, notez que ces mêmes propriétés sont définies sur les mêmes valeurs. Cela paraît être un bon candidat pour un style...

### <a name="create-a-value-text-block-style"></a>Créer un style de bloc de texte de valeur

1. Ouvrez DetailsPage.xaml.

2. Recherchez le contrôle `Grid` nommé **EditControlsGrid**. Il contient nos curseurs et nos zones de texte. Notez que la grille définit déjà un style pour nos curseurs.

    ```xaml
    <Grid x:Name="EditControlsGrid"
            HorizontalAlignment="Stretch"
            Margin="24,48,24,24">
        <Grid.Resources>
            <Style TargetType="Slider">
                <Setter Property="Margin"
                        Value="0,0,0,0" />
                <Setter Property="Padding"
                        Value="0" />
                <Setter Property="MinWidth"
                        Value="100" />
                <Setter Property="StepFrequency"
                        Value="0.1" />
                <Setter Property="TickFrequency"
                        Value="0.1" />
            </Style>
        </Grid.Resources>
    ```

3. Créez un style pour un **TextBlock** qui définit **Margin** sur « 10,8,0,0 », **VerticalAlignment** sur « Center » et **Padding** sur « 0 ».

    **Avant**

    ```xaml
        <Grid.Resources>
            <Style TargetType="Slider">
                <Setter Property="Margin"
                        Value="0,0,0,0" />
                <Setter Property="Padding"
                        Value="0" />
                <Setter Property="MinWidth"
                        Value="100" />
                <Setter Property="StepFrequency"
                        Value="0.1" />
                <Setter Property="TickFrequency"
                        Value="0.1" />
            </Style>
        </Grid.Resources>
    ```

    **Après**

    ```xaml
        <Grid.Resources>
            <Style TargetType="Slider">
                <Setter Property="Margin"
                        Value="0,0,0,0" />
                <Setter Property="Padding"
                        Value="0" />
                <Setter Property="MinWidth"
                        Value="100" />
                <Setter Property="StepFrequency"
                        Value="0.1" />
                <Setter Property="TickFrequency"
                        Value="0.1" />
            </Style>
            <Style TargetType="TextBlock">
                <Setter Property="Margin"
                        Value="10,8,0,0" />
                <Setter Property="VerticalAlignment"
                        Value="Center" />
                <Setter Property="Padding"
                        Value="0" />
            </Style>
        </Grid.Resources>
    ```

4. Transformons-le en style nommé afin de pouvoir spécifier les contrôles `TextBlock` auxquels il s’applique. Définissez la propriété `x:Key` du style sur « ValueTextBlock ».

    **Avant**

    ```xaml
            <Style TargetType="TextBlock">
                <Setter Property="Margin"
                        Value="10,8,0,0" />
                <Setter Property="VerticalAlignment"
                        Value="Center" />
                <Setter Property="Padding"
                        Value="0" />
            </Style>
    ```

    **Après**

    ```xaml
            <Style TargetType="TextBlock"
                   x:Key="ValueTextBlock">
                <Setter Property="Margin"
                        Value="10,8,0,0" />
                <Setter Property="VerticalAlignment"
                        Value="Center" />
                <Setter Property="Padding"
                        Value="0" />
            </Style>
    ```

5. Pour chaque `TextBlock`, supprimez ses propriétés `Margin`, `VerticalAlignment` et `Padding`, puis définissez sa propriété `Style` sur « {StaticResource ValueTextBlock} ».

    **Avant**

    ```xaml
     <TextBlock Grid.Row="2"
                Grid.Column="1"
                Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />
    ```

    **Après**

    ```xaml
     <TextBlock Grid.Row="2"
                Grid.Column="1"
                Style="{StaticResource ValueTextBlock}"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />
    ```

    Effectuer cette modification sur les 6 contrôles TextBlock associés aux curseurs.

6. Compilez et exécutez l’application. Elle doit être... identique. Mais le fait d'avoir écrit un code efficace et facile à gérer doit vous procurer un sentiment d'intense satisfaction et de réussite.

Félicitations, vous avez terminé la partie 2 !

## <a name="part-3-use-a-control-template-to-make-a-fancy-slider"></a>Partie 3 : Utiliser un modèle de contrôle pour réaliser un curseur fantaisie

Vous souvenez-vous comment, dans la partie 1, nous avons ajouté une forme derrière le curseur pour lui donner de l'allure ?

Nous avons bien travaillé, mais il existe un meilleur moyen d’obtenir le même effet : créer un modèle de contrôle.

1. Dans le panneau de l’Explorateur de solutions, double-cliquez sur DetailPage.xaml.

1. Ensuite, nous allons utiliser le modèle de contrôle par défaut pour le curseur comme point de départ. Ajoutez ce code XAML à l'élément `Page.Resources`. (L'élément `Page.Resources` se trouve vers le début de la page.)

    Cette application utilise la bibliothèque d’interface utilisateur Windows (WinUI). Vous pouvez donc copier le modèle par défaut à partir du dépôt GitHub WinUI : [Slider_themeresources.xaml](https://github.com/microsoft/microsoft-ui-xaml/blob/master/dev/Slider/Slider_themeresources.xaml).


    ```xaml
    <ControlTemplate x:Key="FancySliderControlTemplate" TargetType="Slider"
      xmlns:contract6Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,6)"
      xmlns:contract6NotPresent="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractNotPresent(Windows.Foundation.UniversalApiContract,6)"
      xmlns:contract7Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,7)"
      xmlns:contract7NotPresent="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractNotPresent(Windows.Foundation.UniversalApiContract,7)">
      <Grid Margin="{TemplateBinding Padding}">
        <Grid.Resources>
            <Style TargetType="Thumb" x:Key="SliderThumbStyle">
                <Setter Property="BorderThickness" Value="0" />
                <Setter Property="Background" Value="{ThemeResource SliderThumbBackground}" />
                <Setter Property="Template">
                    <Setter.Value>
                        <ControlTemplate TargetType="Thumb">
                            <Border
                                Background="{TemplateBinding Background}"
                                BorderBrush="{TemplateBinding BorderBrush}"
                                BorderThickness="{TemplateBinding BorderThickness}"
                                CornerRadius="{ThemeResource SliderThumbCornerRadius}" />
                        </ControlTemplate>
                    </Setter.Value>
                </Setter>
            </Style>
        </Grid.Resources>

        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup x:Name="CommonStates">
                <VisualState x:Name="Normal" />

                <VisualState x:Name="Pressed">

                    <Storyboard>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalTrackRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalTrackRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalThumb" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalThumb" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderContainerBackgroundPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalDecreaseRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalDecreaseRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillPressed}" />
                        </ObjectAnimationUsingKeyFrames>
                    </Storyboard>
                </VisualState>

                <VisualState x:Name="Disabled">

                    <Storyboard>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HeaderContentPresenter" Storyboard.TargetProperty="Foreground">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderHeaderForegroundDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalDecreaseRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalTrackRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalDecreaseRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalTrackRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalThumb" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalThumb" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="TopTickBar" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTickBarFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="BottomTickBar" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTickBarFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="LeftTickBar" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTickBarFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="RightTickBar" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTickBarFillDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderContainerBackgroundDisabled}" />
                        </ObjectAnimationUsingKeyFrames>
                    </Storyboard>
                </VisualState>

                <VisualState x:Name="PointerOver">

                    <Storyboard>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalTrackRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalTrackRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalThumb" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalThumb" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="Background">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderContainerBackgroundPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalDecreaseRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalDecreaseRect" Storyboard.TargetProperty="Fill">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillPointerOver}" />
                        </ObjectAnimationUsingKeyFrames>
                    </Storyboard>
                </VisualState>

            </VisualStateGroup>
            <VisualStateGroup x:Name="FocusEngagementStates">
                <VisualState x:Name="FocusDisengaged" />
                <VisualState x:Name="FocusEngagedHorizontal">

                    <Storyboard>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="(Control.IsTemplateFocusTarget)">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="False" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalThumb" Storyboard.TargetProperty="(Control.IsTemplateFocusTarget)">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="True" />
                        </ObjectAnimationUsingKeyFrames>
                    </Storyboard>
                </VisualState>
                <VisualState x:Name="FocusEngagedVertical">

                    <Storyboard>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="(Control.IsTemplateFocusTarget)">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="False" />
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalThumb" Storyboard.TargetProperty="(Control.IsTemplateFocusTarget)">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="True" />
                        </ObjectAnimationUsingKeyFrames>
                    </Storyboard>
                </VisualState>

            </VisualStateGroup>

        </VisualStateManager.VisualStateGroups>

        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <ContentPresenter x:Name="HeaderContentPresenter"
            Grid.Row="0"
            Content="{TemplateBinding Header}"
            ContentTemplate="{TemplateBinding HeaderTemplate}"
            FontWeight="{ThemeResource SliderHeaderThemeFontWeight}"
            Foreground="{ThemeResource SliderHeaderForeground}"
            Margin="{ThemeResource SliderTopHeaderMargin}"
            TextWrapping="Wrap"
            Visibility="Collapsed"
            x:DeferLoadStrategy="Lazy"/>
        <Grid x:Name="SliderContainer"
            Grid.Row="1"
            Background="{ThemeResource SliderContainerBackground}"
            Control.IsTemplateFocusTarget="True">
            <Grid x:Name="HorizontalTemplate" MinHeight="{ThemeResource SliderHorizontalHeight}">

                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>

                <Grid.RowDefinitions>
                    <RowDefinition Height="{ThemeResource SliderPreContentMargin}" />
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="{ThemeResource SliderPostContentMargin}" />
                </Grid.RowDefinitions>

                <Rectangle x:Name="HorizontalTrackRect"
                    Fill="{TemplateBinding Background}"
                    Height="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Row="1"
                    Grid.ColumnSpan="3"
                    contract7Present:RadiusX="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7Present:RadiusY="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusX="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusY="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}" />
                <Rectangle x:Name="HorizontalDecreaseRect" Fill="{TemplateBinding Foreground}" Grid.Row="1"
                    contract7Present:RadiusX="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7Present:RadiusY="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusX="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusY="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}" />
                <TickBar x:Name="TopTickBar"
                    Visibility="Collapsed"
                    Fill="{ThemeResource SliderTickBarFill}"
                    Height="{ThemeResource SliderOutsideTickBarThemeHeight}"
                    VerticalAlignment="Bottom"
                    Margin="0,0,0,4"
                    Grid.ColumnSpan="3" />
                <TickBar x:Name="HorizontalInlineTickBar"
                    Visibility="Collapsed"
                    Fill="{ThemeResource SliderInlineTickBarFill}"
                    Height="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Row="1"
                    Grid.ColumnSpan="3" />
                <TickBar x:Name="BottomTickBar"
                    Visibility="Collapsed"
                    Fill="{ThemeResource SliderTickBarFill}"
                    Height="{ThemeResource SliderOutsideTickBarThemeHeight}"
                    VerticalAlignment="Top"
                    Margin="0,4,0,0"
                    Grid.Row="2"
                    Grid.ColumnSpan="3" />
                <Thumb x:Name="HorizontalThumb"
                    Style="{StaticResource SliderThumbStyle}"
                    DataContext="{TemplateBinding Value}"
                    Height="{ThemeResource SliderHorizontalThumbHeight}"
                    Width="{ThemeResource SliderHorizontalThumbWidth}"
                    Grid.Row="0"
                    Grid.RowSpan="3"
                    Grid.Column="1"
                    FocusVisualMargin="-14,-6,-14,-6"
                    AutomationProperties.AccessibilityView="Raw" />
            </Grid>
            <Grid x:Name="VerticalTemplate" MinWidth="{ThemeResource SliderVerticalWidth}" Visibility="Collapsed">

                <Grid.RowDefinitions>
                    <RowDefinition Height="*" />
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>

                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="{ThemeResource SliderPreContentMargin}" />
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="{ThemeResource SliderPostContentMargin}" />
                </Grid.ColumnDefinitions>

                <Rectangle x:Name="VerticalTrackRect"
                    Fill="{TemplateBinding Background}"
                    Width="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Column="1"
                    Grid.RowSpan="3"
                    contract7Present:RadiusX="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7Present:RadiusY="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusX="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusY="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}" />
                <Rectangle x:Name="VerticalDecreaseRect"
                    Fill="{TemplateBinding Foreground}"
                    Grid.Column="1"
                    Grid.Row="2"
                    contract7Present:RadiusX="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7Present:RadiusY="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusX="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusY="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}" />
                <TickBar x:Name="LeftTickBar"
                    Visibility="Collapsed"
                    Fill="{ThemeResource SliderTickBarFill}"
                    Width="{ThemeResource SliderOutsideTickBarThemeHeight}"
                    HorizontalAlignment="Right"
                    Margin="0,0,4,0"
                    Grid.RowSpan="3" />
                <TickBar x:Name="VerticalInlineTickBar"
                    Visibility="Collapsed"
                    Fill="{ThemeResource SliderInlineTickBarFill}"
                    Width="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Column="1"
                    Grid.RowSpan="3" />
                <TickBar x:Name="RightTickBar"
                    Visibility="Collapsed"
                    Fill="{ThemeResource SliderTickBarFill}"
                    Width="{ThemeResource SliderOutsideTickBarThemeHeight}"
                    HorizontalAlignment="Left"
                    Margin="4,0,0,0"
                    Grid.Column="2"
                    Grid.RowSpan="3" />
                <Thumb x:Name="VerticalThumb"
                    Style="{StaticResource SliderThumbStyle}"
                    DataContext="{TemplateBinding Value}"
                    Width="{ThemeResource SliderVerticalThumbWidth}"
                    Height="{ThemeResource SliderVerticalThumbHeight}"
                    Grid.Row="1"
                    Grid.Column="0"
                    Grid.ColumnSpan="3"
                    FocusVisualMargin="-6,-14,-6,-14"
                    AutomationProperties.AccessibilityView="Raw" />
            </Grid>
        </Grid>
      </Grid>
    </ControlTemplate>
    ```

    Incroyable, tout ce code XAML ! Les modèles de contrôle sont une fonctionnalité puissante, mais ils peuvent être très complexes. C’est pourquoi il est généralement conseillé de démarrer à partir du modèle par défaut.

1. Dans le `ControlTemplate` que vous venez d’ajouter, recherchez le contrôle de grille nommé `HorizontalTemplate`. Cette grille définit la partie du modèle que vous souhaitez modifier.

    ```xaml
    <Grid x:Name="HorizontalTemplate" MinHeight="{ThemeResource SliderHorizontalHeight}">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="{ThemeResource SliderPreContentMargin}" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="{ThemeResource SliderPostContentMargin}" />
        </Grid.RowDefinitions>
    ```

1.  Créez un polygone identique à celui que vous avez créé pour le curseur d'exposition dans la partie 1. Ajoutez le polygone après la balise fermante `Grid.RowDefinitions`. Définissez `Grid.Row` sur « 0 », `Grid.RowSpan` sur « 3 » et `Grid.ColumnSpan` sur « 3 ».

    **Avant**

    ```xaml
    <Grid x:Name="HorizontalTemplate" MinHeight="44">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="18" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="18" />
        </Grid.RowDefinitions>
    ```

    **Après**

    ```xaml
    <Grid x:Name="HorizontalTemplate" MinHeight="44">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="18" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="18" />
        </Grid.RowDefinitions>
        <Polygon Grid.Row="0" Grid.RowSpan="3"  Grid.ColumnSpan="3" Stretch="Fill"
                    Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                    VerticalAlignment="Center" Height="20" >
            <Polygon.Fill>
                <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                    <LinearGradientBrush.GradientStops>
                        <GradientStop Offset="0" Color="Black" />
                        <GradientStop Offset="1" Color="White" />
                    </LinearGradientBrush.GradientStops>
                </LinearGradientBrush>
            </Polygon.Fill>
        </Polygon>
    ```

1. Supprimez le paramètre `Polygon.Fill`. Définissez `Fill` sur `"{TemplateBinding Background}"`. Ainsi, la définition de la propriété `Background` du curseur définit également la propriété `Fill` du polygone.

    **Avant**

    ```xaml
        <Polygon Grid.Row="0" Grid.RowSpan="3"  Grid.ColumnSpan="3" Stretch="Fill"
                    Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                    VerticalAlignment="Center" Height="20" >
            <Polygon.Fill>
                <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                    <LinearGradientBrush.GradientStops>
                        <GradientStop Offset="0" Color="Black" />
                        <GradientStop Offset="1" Color="White" />
                    </LinearGradientBrush.GradientStops>
                </LinearGradientBrush>
            </Polygon.Fill>
        </Polygon>
    ```

    **Après**

    ```xaml
        <Polygon Grid.Row="0" Grid.RowSpan="3"  Grid.ColumnSpan="3" Stretch="Fill"
                    Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                    VerticalAlignment="Center" Height="20"
                    Fill="{TemplateBinding Background}">
        </Polygon>
    ```

1. Juste après le polygone que vous avez ajouté, se trouve un rectangle nommé `HorizontalTrackRect`. Supprimez le paramètre `Fill` du Rectangle afin que celui-ci ne soit pas visible et ne bloque pas notre forme de polygone. (Nous ne voulons pas supprimer complètement le rectangle car le modèle de contrôle l'utilise également pour des objets visuels d'interaction, par exemple le pointage.)

    **Avant**

    ```xaml
        <Rectangle x:Name="HorizontalTrackRect"
                    Fill="{TemplateBinding Background}"
                    Height="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Row="1"
                    Grid.ColumnSpan="3" />
    ```

    **Après**

    ```xaml
        <Rectangle x:Name="HorizontalTrackRect"
                    Height="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Row="1"
                    Grid.ColumnSpan="3" />
    ```

    Vous en avez terminé avec le modèle ! Nous devons maintenant l'appliquer à nos curseurs.

1. Nous allons mettre à jour notre curseur d'exposition.

    * Affectez `"{StaticResource FancySliderControlTemplate}"` comme valeur de la propriété `Template` du curseur.
    * Supprimez le paramètre `Background="Transparent"` du curseur.
    * Définissez le `Background` du curseur en dégradé linéaire passant du noir au blanc.
    * Supprimez le polygone en arrière-plan que nous avons créé dans la partie 1.

    **Avant**

    ```xaml
    <Polygon Grid.Row="2" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Black" />
                    <GradientStop Offset="1" Color="White" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Exposure"
            Grid.Row="2" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Exposure, Mode=TwoWay}"
            Minimum="-2"
            Maximum="2" />
    ```

    **Après**

    ```xaml
    <Slider Header="Exposure"
            Grid.Row="2"  Foreground="Transparent"
            Value="{x:Bind item.Exposure, Mode=TwoWay}"
            Minimum="-2"
            Maximum="2"
            Template="{StaticResource FancySliderControlTemplate}">
        <Slider.Background>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Black" />
                    <GradientStop Offset="1" Color="White" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Slider.Background>
    </Slider>
    ```

1. Effectuez les mêmes mises à jour pour le curseur de température.

    **Avant**

    ```xaml
    <Polygon Grid.Row="3" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Blue" />
                    <GradientStop Offset="1" Color="Yellow" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Temperature"
            Grid.Row="3" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Temperature, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1" />
    ```

    **Après**

    ```xaml
    <Slider Header="Temperature"
            Grid.Row="3" Foreground="Transparent"
            Value="{x:Bind item.Temperature, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1"
            Template="{StaticResource FancySliderControlTemplate}">
        <Slider.Background>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Blue" />
                    <GradientStop Offset="1" Color="Yellow" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Slider.Background>
    </Slider>
    ```

1. Effectuez les mêmes mises à jour pour le curseur de teinte.

    **Avant**

    ```xaml
    <Polygon Grid.Row="4" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Red" />
                    <GradientStop Offset="1" Color="Green" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Tint"
            Grid.Row="4" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Tint, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1" />
    ```

    **Après**

    ```xaml
    <Slider Header="Tint"
            Grid.Row="4" Foreground="Transparent"
            Value="{x:Bind item.Tint, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1"
            Template="{StaticResource FancySliderControlTemplate}">
        <Slider.Background>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Red" />
                    <GradientStop Offset="1" Color="Green" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Slider.Background>
    </Slider>
    ```

1. Compilez et exécutez l’application.

    ![Les meilleurs curseurs du monde](../basics/images/xaml-basics/style-sliders-templates.png)

    Comme vous pouvez le constater, nos mises à jour ont amélioré le positionnement du polygone. Désormais, le bas du polygone est aligné sur le bas du curseur de défilement.

Félicitations, vous avez terminé le tutoriel ! Si vous êtes bloqué et que vous souhaitez consulter la solution définitive, vous trouverez l’exemple complet sur [https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab).
