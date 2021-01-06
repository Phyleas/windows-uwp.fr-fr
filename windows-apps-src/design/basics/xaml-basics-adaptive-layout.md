---
title: Tutoriel Créer des dispositions adaptatives
description: Découvrez comment utiliser les fonctionnalités de disposition adaptative en XAML pour créer des applications qui s’affichent correctement, quelle que soit la taille de la fenêtre.
keywords: XAML, UWP, Bien démarrer
ms.date: 08/20/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e9db25c822d524ec36c6e512e132e49a69a2bd7d
ms.sourcegitcommit: 7aa0e1108fd1a19ebc5632acbc9f66ea9af2b321
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/18/2020
ms.locfileid: "97691521"
---
# <a name="tutorial-create-adaptive-layouts"></a>Tutoriel : Créer des dispositions adaptatives

Cet article décrit les concepts de base des fonctionnalités de disposition adaptative du code XAML. Celles-ci vous permettent de créer des applications attrayantes quelle que soit la taille. Vous allez découvrir comment ajouter des points d’arrêt de fenêtre, créer un DataTemplate et utiliser la classe VisualStateManager afin d’adapter la disposition de votre application. Vous utiliserez ces outils pour optimiser un programme de modification d’image pour les fenêtres de petite taille.

Le programme de retouche d’images comporte deux pages. La _page principale_ présente une vue de galerie de photos ainsi que des informations sur chaque fichier image.

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
2. Vous devez ensuite cloner ou télécharger l’exemple. Cliquez sur le bouton **Cloner ou télécharger**. Un sous-menu s’affiche.
    ![Menu Clone or download dans la page GitHub de l’exemple PhotoLab](images/xaml-basics/clone-repo.png)

    **Si vous n’êtes pas familiarisé avec GitHub :**

    a. Cliquez sur **Télécharger le zip**, puis enregistrez le fichier localement. Cela télécharge un fichier .zip contenant tous les fichiers projet dont vous avez besoin.

    b. Extrayez le fichier. Utilisez l’Explorateur de fichiers pour accéder au fichier .zip que vous venez de télécharger, cliquez dessus avec le bouton droit, puis sélectionnez **Extraire tout**.

    c. Accédez à votre copie locale de l’exemple, puis au répertoire `Windows-appsample-photo-lab-master\xaml-basics-starting-points\adaptive-layout`.

    **Si vous maîtrisez GitHub :**

    a. Clonez localement la branche principale du dépôt.

    b. Accédez au répertoire `Windows-appsample-photo-lab\xaml-basics-starting-points\adaptive-layout`.

3. Double-cliquez sur `Photolab.sln` pour ouvrir la solution dans Visual Studio.

## <a name="part-1-define-window-breakpoints"></a>Partie 1 : Définir des points d’arrêt de fenêtre

Exécutez l'application. Son apparence est bonne en mode plein écran, mais l’interface utilisateur n’est pas idéale lorsque vous réduisez trop la fenêtre. Vous pouvez garantir que l’interface utilisateur présente toujours une apparence correcte en utilisant la classe [VisualStateManager](/uwp/api/windows.ui.xaml.visualstatemanager) pour l’adapter à différentes tailles de fenêtre.

![Petite fenêtre : avant](../basics/images/xaml-basics/adaptive-layout-small-before.png)

Pour plus d’informations sur la disposition des applications, consultez la section [Disposition](../layout/index.md) de la documentation.

### <a name="add-window-breakpoints"></a>Ajouter des points d’arrêt de fenêtre

La première étape consiste à définir les _points d’arrêt_ auxquels différents états visuels sont appliqués. Pour plus d’informations sur les points d’arrêt pour les écrans de taille petite, moyenne et grande, consultez [Tailles d’écran et points d’arrêt](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).

Ouvrez App.xaml à partir de l’Explorateur de solutions, puis ajoutez le code suivant après le `MergedDictionaries`, juste avant la balise `</ResourceDictionary>` de fermeture.

```xaml
    <!--  Window width adaptive breakpoints.  -->
    <x:Double x:Key="MinWindowBreakpoint">0</x:Double>
    <x:Double x:Key="MediumWindowBreakpoint">641</x:Double>
    <x:Double x:Key="LargeWindowBreakpoint">1008</x:Double>
```

Cela crée trois points d’arrêt, ce qui vous permet de créer des états visuels pour trois plages de tailles de fenêtres :

+ Petite (de 0 à 640 pixels de large)
+ Moyenne (de 641 à 1007 pixels de large)
+ Grande (plus de 1007 pixels de large)

Dans cet exemple, vous créez une nouvelle apparence uniquement pour la petite taille de fenêtre. Les tailles moyenne et grande utilisent la même apparence.

## <a name="part-2-add-a-data-template-for-small-window-sizes"></a>Partie 2 : Ajouter un modèle de données pour les petites tailles de fenêtre

Pour que cette application ait une apparence correcte même quand elle est affichée dans une petite fenêtre, vous pouvez créer un nouveau modèle de données qui optimise la manière dont les images de la vue Galerie d’images sont affichées lorsque l’utilisateur réduit la fenêtre.

### <a name="create-a-new-datatemplate"></a>Créer un DataTemplate

 Ouvrez MainPage.xaml à partir de l’Explorateur de solutions, puis ajoutez le code suivant à l’intérieur des balises `Page.Resources`.

```xaml
<DataTemplate x:Key="ImageGridView_SmallItemTemplate"
              x:DataType="local:ImageFileInfo">

    <!-- Create image grid -->
    <Grid Height="{Binding ItemSize, ElementName=page}"
          Width="{Binding ItemSize, ElementName=page}">

        <!-- Place image in grid, stretching it to fill the pane-->
        <Image x:Name="ItemImage"
               Source="{x:Bind ImageSource, Mode=OneWay}"
               Stretch="UniformToFill">
        </Image>

    </Grid>
</DataTemplate>
```

Ce modèle de galerie économise l’espace de l’écran en supprimant la bordure autour des images et en éliminant les métadonnées d’image (nom de fichier, évaluations, etc.) situées sous chaque vignette. À la place, chaque vignette s’affiche sous la forme d’un simple carré.

### <a name="add-metadata-to-a-tooltip"></a>Ajouter des métadonnées à une info-bulle

Vous souhaitez que l’utilisateur puisse encore accéder aux métadonnées de chaque image. Vous allez donc ajouter une info-bulle à chaque élément d’image. Ajoutez le code suivant à l’intérieur des balises `Image` de l’objet DataTemplate que vous venez de créer.

```xaml
    <!-- Add a tooltip to the image that displays metadata -->
    <ToolTipService.ToolTip>
        <ToolTip x:Name="tooltip">

            <!-- Arrange tooltip elements vertically -->
            <StackPanel Orientation="Vertical"
                        Grid.Row="1">

                <!-- Image title -->
                <TextBlock Text="{x:Bind ImageTitle, Mode=OneWay}"
                           HorizontalAlignment="Center"
                           Style="{StaticResource SubtitleTextBlockStyle}" />

                <!-- Arrange elements horizontally -->
                <StackPanel Orientation="Horizontal"
                            HorizontalAlignment="Center">

                    <!-- Image file type -->
                    <TextBlock Text="{x:Bind ImageFileType}"
                               HorizontalAlignment="Center"
                               Style="{StaticResource CaptionTextBlockStyle}" />

                    <!-- Image dimensions -->
                    <TextBlock Text="{x:Bind ImageDimensions}"
                               HorizontalAlignment="Center"
                               Style="{StaticResource CaptionTextBlockStyle}"
                               Margin="8,0,0,0" />
                </StackPanel>
            </StackPanel>
        </ToolTip>
    </ToolTipService.ToolTip>
```

Quand vous pointerez la souris sur une vignette (ou que vous effectuez un appui prolongé dessus dans le cas d’un écran tactile), le titre, le type de fichier et les dimensions de l’image s’afficheront.

## <a name="part-3-define-visual-states"></a>Partie 3 : Définir les états visuels

Vous avez maintenant créé une disposition pour vos données, mais l’application n’a actuellement aucun moyen de savoir quand utiliser cette disposition plutôt que les styles par défaut. Pour résoudre ce problème, vous devez ajouter un [VisualStateManager](/uwp/api/windows.ui.xaml.visualstatemanager) et des définitions [VisualState](/uwp/api/windows.ui.xaml.visualstate).

### <a name="add-a-visualstatemanager"></a>Ajouter un VisualStateManager

Ajoutez le code suivant à l’élément racine de la page, `RelativePanel`.

```xaml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
    ...

        <!-- Large window VisualState -->
        <VisualState>

        </VisualState>

        <!-- Medium window VisualState -->
        <VisualState>

        </VisualState>

        <!-- Small window VisualState -->
        <VisualState>

        </VisualState>

    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

### <a name="create-statetriggers-to-apply-the-visual-state"></a>Créer des StateTriggers pour appliquer l’état visuel

Ensuite, créez les `StateTriggers` qui correspondent à chaque point d’ancrage. Dans MainPage.xaml, ajoutez le code suivant à chaque `VisualState`.

```xaml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
    ...

        <!-- Large window VisualState -->
        <VisualState>

            <!-- Large window trigger -->
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="{StaticResource LargeWindowBreakpoint}"/>
            </VisualState.StateTriggers>

        </VisualState>

        <!-- Medium window VisualState -->
        <VisualState>

            <!-- Medium window trigger -->
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="{StaticResource MediumWindowBreakpoint}"/>
            </VisualState.StateTriggers>

        </VisualState>

        <!-- Small window VisualState -->
        <VisualState>

            <!-- Small window trigger -->
            <VisualState.StateTriggers >
                <AdaptiveTrigger MinWindowWidth="{StaticResource MinWindowBreakpoint}"/>
            </VisualState.StateTriggers>

        </VisualState>

    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

Lorsque chaque état visuel est déclenché, l’application utilise les attributs de disposition affectés au `VisualState` actif.

### <a name="set-properties-for-each-visual-state"></a>Définir les propriétés de chaque état visuel

Pour finir, définissez les propriétés de chaque état visuel afin que le `VisualStateManager` sache quels attributs appliquer lorsque l’état est déclenché. Chaque méthode setter cible une propriété d’un élément XAML particulier et lui affecte la valeur donnée. Ajoutez ce code à l’état visuel `SmallWindow` que vous venez de créer, après le `StateTriggers`.

```xaml
    <!-- Small window setters -->
    <VisualState.Setters>

        <!-- Apply small template and styles -->
        <Setter Target="ImageGridView.ItemTemplate"
                Value="{StaticResource ImageGridView_SmallItemTemplate}" />
        <Setter Target="ImageGridView.ItemContainerStyle"
                Value="{StaticResource ImageGridView_SmallItemContainerStyle}" />

        <!-- Adjust the zoom slider to fit small windows-->
        <Setter Target="ZoomSlider.Minimum"
                Value="80" />
        <Setter Target="ZoomSlider.Maximum"
                Value="180" />
        <Setter Target="ZoomSlider.TickFrequency"
                Value="20" />
        <Setter Target="ZoomSlider.Value"
                Value="100" />
    </VisualState.Setters>
```

Ces setters affectent le nouveau `DataTemplate` que vous avez créé dans la section précédente en tant qu’`ItemTemplate` de la Galerie d’images. Elles optimisent également le curseur de zoom pour l’adapter au petit écran.

### <a name="run-the-app"></a>Exécuter l’application

Exécutez l'application. Lors du chargement de l’application, essayez de modifier la taille de la fenêtre. Quand vous réduisez la fenêtre à une petite taille, vous devez voir l’application basculer vers la petite disposition que vous avez créée dans la Partie 2.

![Petite fenêtre : après](../basics/images/xaml-basics/adaptive-layout-small-after.png)

## <a name="going-further"></a>Aller plus loin

Maintenant que vous avez terminé ce laboratoire, vous avez suffisamment de connaissances en disposition adaptative pour étendre l’expérience par vous-même. Pour aller plus loin, essayez d’optimiser la disposition pour des tailles d’écran plus grandes (par exemple Surface Hub). Si vous souhaitez tester une disposition Surface Hub, consultez [Tester les applications de Surface Hub à l’aide de Visual Studio](../../debug-test-perf/test-surface-hub-apps-using-visual-studio.md).

Si vous êtes bloqué, vous trouverez des instructions supplémentaires dans [Dispositions réactives avec XAML](../layout/layouts-with-xaml.md).

Si vous souhaitez en savoir plus sur la manière dont l’application de retouche photo initiale a été générée, vous pouvez également consulter les tutoriels sur les [interfaces utilisateur](../basics/xaml-basics-ui.md) et la [liaison de données](../../data-binding/xaml-basics-data-binding.md) XAML.

## <a name="get-the-final-version-of-the-photolab-sample"></a>Obtenir la version finale de l’exemple PhotoLab

Comme ce tutoriel ne génère pas l’application de retouche photo complète, veillez à examiner la [version finale](https://github.com/Microsoft/Windows-appsample-photo-lab) pour voir d’autres fonctionnalités, comme les animations personnalisées et les styles.
