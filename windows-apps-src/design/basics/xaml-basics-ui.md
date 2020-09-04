---
title: 'Tutoriel : Créer une interface utilisateur'
description: Suivez ce tutoriel afin d’apprendre à créer une interface utilisateur de base pour un programme de modification d’image à l’aide des outils XAML de Visual Studio.
keywords: XAML, UWP, Bien démarrer
ms.date: 08/20/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e4c2c8d52069bf074897ec09fa44f550066b28b5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89160753"
---
# <a name="tutorial-create-a-user-interface"></a>Tutoriel : Créer une interface utilisateur

Dans ce tutoriel, vous allez apprendre à créer une interface utilisateur de base pour un programme de retouche d’images. Pour ce faire, vous allez effectuer plusieurs opérations :

+ Utilisation des outils XAML de Visual Studio, par exemple le concepteur XAML, la Boîte à outils, l’éditeur XAML, le panneau Propriétés et la Structure du document pour ajouter des contrôles et du contenu à votre interface utilisateur.
+ Utilisation de certains des panneaux de disposition XAML les plus courants, tels que `RelativePanel`, `Grid` et `StackPanel`.

Le programme de retouche d’images comporte deux pages. La _page principale_ présente une vue de galerie de photos ainsi que des informations sur chaque fichier image.

![Page principale](images/xaml-basics/mainpage.png)

La *page de détails* affiche une seule photo une fois qu’elle a été sélectionnée. Un menu d’édition volant permet de modifier la photo, de la renommer et de l’enregistrer.

![Page de détails](images/xaml-basics/detailpage.png)

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

    c. Accédez à votre copie locale de l’exemple, puis au répertoire `Windows-appsample-photo-lab-master\xaml-basics-starting-points\user-interface`.

    **Si vous maîtrisez GitHub :**

    a. Clonez localement la branche maîtresse du dépôt.

    b. Accédez au répertoire `Windows-appsample-photo-lab\xaml-basics-starting-points\user-interface`.

3. Double-cliquez sur `Photolab.sln` pour ouvrir la solution dans Visual Studio.

## <a name="part-1-add-a-textblock-control-by-using-xaml-designer"></a>Partie 1 : Ajouter un contrôle TextBlock à l’aide du concepteur XAML

Visual Studio fournit plusieurs outils pour faciliter la création de votre interface utilisateur XAML.

+ Faites glisser des contrôles de la _Boîte à outils_ vers l’aire de conception du _Concepteur XAML_ et observez à quoi ils ressemblent avant d’exécuter l’application.
+ Le panneau _Propriétés_ vous permet de voir et de définir toutes les propriétés du contrôle qui est actif dans le concepteur.
+ La _Structure du document_ montre la structure parent/enfant de l’arborescence visuelle XAML de votre interface utilisateur.
+ L’_Éditeur XAML_ vous permet d’entrer et de modifier directement le balisage XAML.

Voici l’interface utilisateur de Visual Studio avec les outils étiquetés.

![Disposition Visual Studio](images/xaml-basics/visual-studio-tools.png)

Chacun de ces outils facilitant la création de votre interface utilisateur, nous allons tous les utiliser dans ce tutoriel. Vous allez commencer par utiliser le concepteur XAML pour ajouter un contrôle.

Pour ajouter un contrôle à l’aide du concepteur XAML

1. Dans l’Explorateur de solutions, double-cliquez sur **MainPage.xaml** pour l’ouvrir. Cette étape affiche la page principale de l’application sans aucun élément d’interface utilisateur ajouté.

2. Avant d’aller plus loin, vous devez apporter quelques ajustements à Visual Studio :

    + Vérifiez que la plateforme de solution est définie sur x86 ou x64 (et non ARM).
    + Définissez le concepteur XAML de la page principale pour afficher l’aperçu du Bureau 13,3".

    Vous devez voir les deux paramètres dans la partie supérieure de la fenêtre, comme illustré ici.

    ![Paramètres Visual Studio](images/xaml-basics/layout-vs-settings.png)

    Vous pouvez exécuter l’application maintenant, mais vous ne verrez pas grand-chose. Nous allons ajouter quelques éléments d’interface utilisateur pour rendre les choses plus intéressantes.

3. Dans la Boîte à outils, développez **Contrôles XAML communs** et recherchez le contrôle [TextBlock](/uwp/api/windows.ui.xaml.controls.textblock). Faites glisser un contrôle `TextBlock` sur l’aire de conception et placez-le près du coin supérieur gauche de la page.

    Le contrôle `TextBlock` est ajouté à la page et le concepteur définit des propriétés en fonction de ce qu’il estime le mieux adapté à la disposition souhaitée. Une surbrillance bleue apparaît autour du contrôle `TextBlock` pour indiquer qu’il est désormais l’objet actif. Remarquez les marges et les autres paramètres ajoutés par le concepteur.

    Votre code XAML doit ressembler à ce qui suit. Ne vous inquiétez pas si sa mise en forme n’est pas exactement la même. Nous l’avons abrégé ici afin d’en faciliter la lecture.

    ```xaml
    <TextBlock HorizontalAlignment="Left"
               Margin="351,44,0,0"
               TextWrapping="Wrap"
               Text="TextBlock"
               VerticalAlignment="Top"/>
    ```

    Dans les étapes suivantes, vous allez mettre à jour ces valeurs.

4. Dans le panneau **Propriétés**, remplacez la valeur **Nom** du contrôle `TextBlock` par **TitleTextBlock**. (Vérifiez que le contrôle `TextBlock` est toujours l’objet actif.)

5. Sous **Common**, remplacez la valeur **Text** par **Collection**.

    ![Propriétés de TextBlock](images/xaml-basics/text-block-properties.png)

    Dans l’éditeur XAML, votre code XAML se présente désormais comme suit.

    ```xaml
    <TextBlock x:Name="TitleTextBlock"
               HorizontalAlignment="Left"
               Margin="351,44,0,0"
               TextWrapping="Wrap"
               Text="Collection"
               VerticalAlignment="Top"/>
    ```

6. Pour positionner le contrôle `TextBlock`, vous devez tout d’abord supprimer les valeurs de propriété ajoutées par Visual Studio. Dans Structure du document, cliquez avec le bouton droit sur **TitleTextBlock**, puis sélectionnez **Disposition** > **Tout réinitialiser**.

    ![Structure du document](images/xaml-basics/doc-outline-reset.png)

7. Dans le panneau **Propriétés**, entrez `Margin` dans la zone de recherche pour trouver facilement la propriété `Margin`. Définissez les marges à gauche et en bas à 24.

    ![Marges de TextBlock](images/xaml-basics/margins.png)

    Les marges sont le moyen le plus simple pour positionner un élément sur la page. Elles sont utiles pour affiner votre disposition, mais vous devez éviter d’utiliser de grandes valeurs de marge comme celles ajoutées par Visual Studio. Elles compliquent l’adaptation de votre interface utilisateur à différentes tailles d’écran.

    Pour plus d’informations, consultez [Alignement, marges et espacement](../layout/alignment-margin-padding.md).

8. Dans Structure du document, cliquez avec le bouton droit sur **TitleTextBlock**, puis sélectionnez **Modifier le style** > **Appliquer la ressource** > **TitleTextBlockStyle**. Cette étape applique un style défini par le système au texte du titre.

    ```xaml
    <TextBlock x:Name="TitleTextBlock"
               TextWrapping="Wrap"
               Text="Collection"
               Margin="24,0,0,24"
               Style="{StaticResource TitleTextBlockStyle}"/>
    ```

9. Dans le panneau **Propriétés**, entrez **textwrapping** dans la zone de recherche pour trouver facilement la propriété `TextWrapping`. Sélectionnez le _marqueur de propriété_ de la propriété `TextWrapping` pour ouvrir son menu. (Le marqueur de propriété est le symbole de petite case situé à droite de chaque valeur de propriété. Il est noir pour indiquer que la propriété est définie sur une valeur autre que la valeur par défaut.) Dans le menu **Propriété**, sélectionnez `TextWrapping` pour réinitialiser la propriété **TextWrapping**.

    Visual Studio ajoute cette propriété, mais comme elle est déjà définie dans le style que vous avez appliqué, vous n’en avez pas besoin ici.

Vous avez ajouté la première partie de l’interface utilisateur à votre application. Exécutez l’application maintenant pour voir à quoi elle ressemble.

> [!NOTE]
> Dans cette partie du tutoriel, vous avez ajouté un contrôle en le faisant glisser. Vous pouvez aussi ajouter un contrôle en double-cliquant dessus dans la Boîte à outils. Essayez et regardez les différences dans le code XAML généré par Visual Studio.

## <a name="part-2-add-a-gridview-control-by-using-the-xaml-editor"></a>Partie 2 : Ajouter un contrôle GridView à l’aide de l’éditeur XAML

Dans la partie 1, vous avez eu une idée de l’utilisation du concepteur XAML et de certains des autres outils fournis par Visual Studio. Ici, vous allez utiliser l’éditeur XAML pour travailler directement avec le balisage XAML. À mesure que vous vous familiarisez avec XAML, vous trouverez sans doute que c’est un moyen plus efficace de travailler.

Tout d’abord, vous remplacerez la disposition racine [Grid](/uwp/api/windows.ui.xaml.controls.grid) par un [RelativePanel](/uwp/api/windows.ui.xaml.controls.relativepanel). `RelativePanel` facilite la réorganisation des blocs de l’interface utilisateur par rapport au panneau ou à d’autres éléments de l’interface utilisateur. Vous constaterez son utilité dans le tutoriel [Disposition adaptative en XAML](xaml-basics-adaptive-layout.md).

Ensuite, vous allez ajouter un contrôle [GridView](/uwp/api/windows.ui.xaml.controls.gridview) pour afficher vos données.

Pour ajouter un contrôle à l’aide de l’éditeur XAML

1. Dans l’éditeur XAML, remplacez la racine `Grid` par `RelativePanel`.

    **Avant**

    ```xaml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
          <TextBlock x:Name="TitleTextBlock"
                     Text="Collection"
                     Margin="24,0,0,24"
                     Style="{StaticResource TitleTextBlockStyle}"/>
    </Grid>
    ```

    **Après**

    ```xaml
    <RelativePanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock x:Name="TitleTextBlock"
                   Text="Collection"
                   Margin="24,0,0,24"
                   Style="{StaticResource TitleTextBlockStyle}"/>
    </RelativePanel>
    ```

    Pour plus d’informations sur les dispositions utilisant `RelativePanel`, consultez [Panneaux de disposition](../layout/layout-panels.md#relativepanel).

2. Sous l’élément `TextBlock`, ajoutez un contrôle `GridView` nommé **ImageGridView**. Définissez les _propriétés jointes_ `RelativePanel` pour placer le contrôle sous le texte du titre et l’étirer sur toute la largeur de l’écran.

    **Ajoutez ce code XAML**

    ```xaml
    <GridView x:Name="ImageGridView"
              Margin="0,0,0,8"
              RelativePanel.AlignLeftWithPanel="True"
              RelativePanel.AlignRightWithPanel="True"
              RelativePanel.Below="TitleTextBlock"/>
    ```

    **Après TextBlock**

    ```xaml
    <RelativePanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock x:Name="TitleTextBlock"
                   Text="Collection"
                   Margin="24,0,0,24"
                   Style="{StaticResource TitleTextBlockStyle}"/>

        <!-- Add the GridView control here. -->

    </RelativePanel>
    ```

    Pour plus d’informations sur les propriétés jointes du panneau, consultez [Panneaux de disposition](../layout/layout-panels.md).

3. Pour que le contrôle `GridView` affiche quelque chose, vous devez lui donner une collection de données à afficher. Ouvrez **MainPage.xaml.cs** et recherchez la méthode `GetItemsAsync`. Cette méthode remplit une collection appelée **Images**, qui est une propriété que nous avons ajoutée à **MainPage**.

    À la fin de la méthode `GetItemsAsync`, ajoutez cette ligne de code.

    ```csharp
    ImageGridView.ItemsSource = Images;
    ```

    Cela affecte la collection **Images** de l’application comme valeur de la propriété [ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) du contrôle `GridView`. Elle donne également au contrôle `GridView` quelque chose à afficher.

C’est le moment idéal pour exécuter l’application et vérifier que tout fonctionne. Voici à quoi cela doit ressembler.

![Point de contrôle de l’interface utilisateur de l’application 1](images/xaml-basics/layout-0.png)

Vous remarquerez que l’application ne montre pas encore d’images. Par défaut, elle affiche la valeur `ToString` du type de données qui se trouvent dans la collection. Ensuite, vous allez créer un modèle de données pour définir la façon dont les données s’affichent.

> [!NOTE]
> Vous pouvez en savoir plus sur les dispositions utilisant `RelativePanel` dans l’article [Panneaux de disposition](../layout/layout-panels.md#relativepanel). Jetez un coup d’œil, puis essayez quelques dispositions différentes en définissant les propriétés jointes `RelativePanel` sur `TextBlock` et `GridView`.

## <a name="part-3-add-a-datatemplate-object-to-display-your-data"></a>Partie 3 : Ajouter un objet DataTemplate pour afficher vos données

Maintenant, vous allez créer un [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) qui indique au contrôle `GridView` comment afficher vos données. Pour obtenir une explication exhaustive des modèles de données, consultez [Conteneurs et modèles d’éléments](../controls-and-patterns/item-containers-templates.md).

Pour l’instant, vous allez seulement ajouter des espaces réservés pour vous aider à créer la disposition souhaitée. Dans le tutoriel [Liaison de données XAML](../../data-binding/xaml-basics-data-binding.md), vous remplacerez ces espaces réservés par des données réelles provenant de la classe `ImageFileInfo`. Vous pouvez ouvrir le fichier **ImageFileInfo.cs** maintenant si vous souhaitez voir à quoi ressemble l’objet de données.

Pour ajouter un modèle de données à une vue Grille

1. Ouvrez **MainPage.xaml**.

2. Pour afficher l’évaluation, utilisez le [RatingControl](/uwp/api/microsoft.ui.xaml.controls.ratingcontrol) du package NuGet de [bibliothèque d’interface utilisateur Windows](/windows/apps/winui/) (WinUI). Ajoutez une référence d’espace de noms XAML qui spécifie l’espace de noms pour les contrôles WinUI. Placez-la dans la balise d’ouverture `Page`, juste après les autres entrées `xmlns:`.

    **Ajoutez ce code XAML**

    ```xaml
    xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
    ```

    **Après la dernière entrée `xmlns:`**

    ```xaml
    <Page x:Name="page"
      x:Class="PhotoLab.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:PhotoLab"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
      mc:Ignorable="d"
      NavigationCacheMode="Enabled">
    ```

    Pour plus d’informations sur les espaces de noms XAML, consultez [Espaces de noms XAML et mappage d’espaces de noms](../../xaml-platform/xaml-namespaces-and-namespace-mapping.md).

3. Dans Structure du document, cliquez avec le bouton droit sur **ImageGridView**. Dans le menu contextuel, sélectionnez **Modifier les modèles supplémentaires** > **Modifier les éléments générés (ItemTemplate)**  > **Créer vide**. La boîte de dialogue **Créer une ressource** s’ouvre.

4. Dans la boîte de dialogue, remplacez la valeur de **Nom (clé)** par **ImageGridView_DefaultItemTemplate**, puis sélectionnez **OK**.

    Plusieurs choses se produisent quand vous sélectionnez **OK** :

    + Un objet `DataTemplate` est ajouté à la section `Page.Resources` de **MainPage.xaml**.

        ```xaml
        <Page.Resources>
            <DataTemplate x:Key="ImageGridView_DefaultItemTemplate">
                <Grid/>
            </DataTemplate>
        </Page.Resources>
        ```

    + La propriété `GridView` du contrôle [GridView](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) est définie sur la ressource `DataTemplate`.

       ```xaml
           <GridView x:Name="ImageGridView"
                     Margin="0,0,0,8"
                     RelativePanel.AlignLeftWithPanel="True"
                     RelativePanel.AlignRightWithPanel="True"
                     RelativePanel.Below="TitleTextBlock"
                     ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"/>
       ```

5. Dans la ressource **ImageGridView_DefaultItemTemplate**, donnez à la racine `Grid` une hauteur et une largeur de **300**, et une marge de **8**. Ajoutez deux lignes, puis définissez la hauteur de la deuxième ligne sur **Automatique**.

    **Avant**

    ```xaml
    <Grid/>
    ```

    **Après**

    ```xaml
    <Grid Height="300"
          Width="300"
          Margin="8">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
    </Grid>
    ```

    Pour plus d’informations sur les dispositions de `Grid`, consultez [Panneaux de disposition](../layout/layout-panels.md#grid).

6. Ajoutez des contrôles à la disposition de `Grid`.

    a. Ajoutez un contrôle [Image](/uwp/api/windows.ui.xaml.controls.image) à la grille ; par défaut, il est placé sur la première ligne de la grille (ligne 0). C’est ici que l’image sera affichée, mais pour le moment vous allez utiliser le logo Store de l’application comme espace réservé.

    b. Ajoutez des contrôles `TextBlock` pour afficher le nom, le type de fichier et les dimensions de l’image. Pour ce faire, vous utilisez des contrôles `StackPanel` pour organiser les blocs de texte. Utilisez la propriété jointe `Grid.Row` pour placer le `StackPanel` le plus à l’extérieur sur la deuxième ligne (ligne 1).

    Pour plus d’informations sur la disposition `StackPanel`, consultez [Panneaux de disposition](../layout/layout-panels.md#stackpanel).

    c. Ajoutez le `RatingControl` au contrôle `StackPanel` externe (vertical). Placez-le après le contrôle `StackPanel` interne (horizontal).

    **Modèle définitif**

    ```xaml
    <Grid Height="300"
          Width="300"
          Margin="8">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Image x:Name="ItemImage"
               Source="Assets/StoreLogo.png"
               Stretch="Uniform" />

        <StackPanel Orientation="Vertical"
                    Grid.Row="1">
            <TextBlock Text="ImageTitle"
                       HorizontalAlignment="Center"
                       Style="{StaticResource SubtitleTextBlockStyle}" />
            <StackPanel Orientation="Horizontal"
                        HorizontalAlignment="Center">
                <TextBlock Text="ImageFileType"
                           HorizontalAlignment="Center"
                           Style="{StaticResource CaptionTextBlockStyle}" />
                <TextBlock Text="ImageDimensions"
                           HorizontalAlignment="Center"
                           Style="{StaticResource CaptionTextBlockStyle}"
                           Margin="8,0,0,0" />
            </StackPanel>

            <muxc:RatingControl Value="3" IsReadOnly="True"/>
        </StackPanel>
    </Grid>
    ```

Exécutez maintenant l’application pour voir le contrôle `GridView` avec le modèle d’élément que vous venez de créer. Ensuite, vous allez changer la couleur d’arrière-plan et ajouter un espace entre les éléments de la grille.

![Point de contrôle de l’interface utilisateur de l’application 3](images/xaml-basics/layout-1.png)

## <a name="part-4-modify-the-item-container-style"></a>Partie 4 : Modifier le style du conteneur d’éléments

Le modèle de contrôle d’un élément contient les visuels qui affichent l’état, comme la sélection, le pointage et le focus. Ces visuels sont rendus au-dessus ou en dessous du modèle de données. Ici, vous allez modifier les propriétés `Background` et `Margin` du modèle de contrôle afin de donner aux éléments de `GridView` un arrière-plan grisé.

Pour modifier le conteneur d’éléments

1. Dans Structure du document, cliquez avec le bouton droit sur **ImageGridView**. Dans le menu contextuel, sélectionnez **Modifier les modèles supplémentaires** > **Modifier le conteneur d’éléments généré (ItemContainerStyle)**  > **Modifier une copie**. La boîte de dialogue **Créer une ressource** s’ouvre.

2. Dans la boîte de dialogue, remplacez la valeur de **Nom (clé)** par **ImageGridView_DefaultItemContainerStyle**, puis sélectionnez **OK**.

    Une copie du style par défaut est ajoutée à la section **Page.Resources** de votre code XAML.

    ```xaml
    <Style x:Key="ImageGridView_DefaultItemContainerStyle" TargetType="GridViewItem">
        <Setter Property="FontFamily" Value="{ThemeResource ContentControlThemeFontFamily}"/>
        <Setter Property="FontSize" Value="{ThemeResource ControlContentThemeFontSize}"/>
        <Setter Property="Background" Value="{ThemeResource GridViewItemBackground}"/>
        <Setter Property="Foreground" Value="{ThemeResource GridViewItemForeground}"/>
        <Setter Property="TabNavigation" Value="Local"/>
        <Setter Property="IsHoldingEnabled" Value="True"/>
        <Setter Property="HorizontalContentAlignment" Value="Center"/>
        <Setter Property="VerticalContentAlignment" Value="Center"/>
        <Setter Property="Margin" Value="0,0,4,4"/>
        <Setter Property="MinWidth" Value="{ThemeResource GridViewItemMinWidth}"/>
        <Setter Property="MinHeight" Value="{ThemeResource GridViewItemMinHeight}"/>
        <Setter Property="AllowDrop" Value="False"/>
        <Setter Property="UseSystemFocusVisuals" Value="{StaticResource UseSystemFocusVisuals}"/>
        <Setter Property="FocusVisualMargin" Value="-2"/>
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="GridViewItem">
                <!-- XAML removed for clarity
                    <ListViewItemPresenter ... />
                -->   
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
    ```

    Le style par défaut `GridViewItem` définit un grand nombre de propriétés. Toutefois, dans votre copie du modèle, il vous suffit de conserver les propriétés que vous souhaitez modifier.

    Comme à l’étape précédente, la propriété **ItemContainerStyle** du contrôle **GridView** est définie sur la nouvelle ressource **Style**.

    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"
                  ItemContainerStyle="{StaticResource ImageGridView_DefaultItemContainerStyle}"/>
    ```

3. Supprimez tous les éléments `Setter` sauf `Background` et `Margin`.

4. Remplacez la valeur de la propriété `Background` par `Gray`.

    **Avant**

    ```xaml
        <Setter Property="Background" Value="{ThemeResource GridViewItemBackground}"/>
    ```

    **Après**

    ```xaml
        <Setter Property="Background" Value="Gray"/>
    ```

5. Remplacez la valeur de la propriété `Margin` par `8`.

    **Avant**

    ```xaml
        <Setter Property="Margin" Value="0,0,4,4"/>
    ```

    **Après**

    ```xaml
        <Setter Property="Margin" Value="8"/>
    ```

Exécutez l’application et regardez à quoi elle ressemble désormais. Redimensionnez la fenêtre de l’application. Le contrôle `GridView` s’occupe de réorganiser les images pour vous mais, pour certaines largeurs, il reste beaucoup d’espace sur le côté droit de la fenêtre d’application. Il serait préférable de centrer les images. Vous allez vous en occuper après.

![Point de contrôle de l’interface utilisateur de l’application 3](images/xaml-basics/layout-2.png)

> [!Note]
> Si vous souhaitez faire des essais, essayez d’affecter aux propriétés `Background` et `Margin` des valeurs différentes pour en voir l’effet.

## <a name="part-5-apply-some-final-adjustments-to-the-layout"></a>Partie 5 : Appliquer quelques derniers ajustements à la disposition

Pour centrer les images dans la page, vous devez régler l’alignement du contrôle `Grid` dans la page. Avez-vous besoin de régler l’alignement des images dans le `GridView` ? Est-ce important ? Voyons voir.

Pour plus d’informations sur l’alignement, consultez [Alignement, marges et espacement](../layout/alignment-margin-padding.md).

(Vous pouvez essayer de paramétrer la propriété `Background` de `GridView` sur votre couleur préférée lors de cette étape. Cela vous permet de voir plus clairement ce qui se passe dans la disposition.)

Pour modifier l’alignement des images

1. Dans `GridView`, affectez la valeur `Center` à la propriété [HorizontalAlignment](/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment).

    **Avant**

    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"/>
    ```

    **Après**

    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"
                  HorizontalAlignment="Center"/>
    ```

2. Exécutez l’application et redimensionnez la fenêtre. Faites défiler vers le bas pour afficher plus d’images.

    Les images sont centrées, ce qui améliore l’aspect. Toutefois, la barre de défilement est alignée sur le bord du contrôle `GridView` au lieu du bord de la fenêtre. Pour résoudre ce problème, vous devez centrer les images dans `GridView` plutôt que centrer `GridView` dans la page. Cela représente un peu plus de travail, mais le résultat sera meilleur à la fin.

3. Supprimez le paramètre `HorizontalAlignment` de l’étape précédente.

4. Dans Structure du document, cliquez avec le bouton droit sur **ImageGridView**. Dans le menu contextuel, sélectionnez **Modifier les modèles supplémentaires** > **Modifier la disposition des éléments (ItemsPanel)**  > **Modifier une copie**. La boîte de dialogue **Créer une ressource** s’ouvre.

5. Dans la boîte de dialogue, remplacez la valeur de **Nom (clé)** par **ImageGridView_ItemsPanelTemplate**, puis sélectionnez **OK**.

    Une copie du modèle **ItemsPanelTemplate** par défaut est ajoutée à la section **Page.Resources** de votre code XAML. (Et comme auparavant, `GridView` est mis à jour pour référencer cette ressource.)

    ```xaml
    <ItemsPanelTemplate x:Key="ImageGridView_ItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal" />
    </ItemsPanelTemplate>
    ```

    De la même manière que vous avez utilisé plusieurs panneaux pour disposer les contrôles dans votre application, `GridView` a un panneau interne qui gère la disposition de ses éléments. Maintenant que vous avez accès à ce panneau ([ItemsWrapGrid](/uwp/api/windows.ui.xaml.controls.itemswrapgrid)), vous pouvez modifier ses propriétés pour changer la disposition des éléments à l’intérieur du contrôle `GridView`.

6. Dans le `ItemsWrapGrid`, affectez la valeur `Center` à la propriété `HorizontalAlignment`.

    **Avant**

    ```xaml
    <ItemsPanelTemplate x:Key="ImageGridView_ItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal" />
    </ItemsPanelTemplate>
    ```

    **Après**

    ```xaml
    <ItemsPanelTemplate x:Key="ImageGridView_ItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal"
                       HorizontalAlignment="Center"/>
    </ItemsPanelTemplate>
    ```

7. Exécutez l’application et redimensionnez à nouveau la fenêtre. Faites défiler vers le bas pour afficher plus d’images.

![Point de contrôle de l’interface utilisateur de l’application 4](images/xaml-basics/layout-3.png)

Désormais, la barre de défilement est alignée sur le bord de la fenêtre. Bravo ! Vous avez créé l’interface utilisateur de base de votre application.

## <a name="go-further"></a>Aller plus loin

Maintenant que vous avez créé l’interface utilisateur de base, consultez ces autres tutoriels. Ils sont également basés sur l’exemple PhotoLab.

* Ajoutez des images réelles et des données dans le [tutoriel de liaison de données XAML](../../data-binding/xaml-basics-data-binding.md).
* Rendez l’interface utilisateur adaptable en fonction des tailles d’écran dans le [tutoriel de disposition adaptative en XAML](xaml-basics-adaptive-layout.md).


## <a name="get-the-final-version-of-the-photolab-sample"></a>Obtenir la version finale de l’exemple PhotoLab

Ce tutoriel ne génère pas l’application de retouche photo complète. Veillez donc à examiner la [version finale](https://github.com/Microsoft/Windows-appsample-photo-lab) pour voir d’autres fonctionnalités, comme les animations personnalisées et les dispositions adaptatives.