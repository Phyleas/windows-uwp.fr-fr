---
title: Créer des liaisons de données
description: Cet article décrit les concepts de base de la liaison de données en XAML.
keywords: XAML, UWP, Bien démarrer
ms.date: 08/20/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ac0603aab5abdc9aef54264c7e8d5bf9ae848889
ms.sourcegitcommit: 8e0e4cac79554e86dc7f035c4b32cb1f229142b0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88943119"
---
# <a name="tutorial-create-data-bindings"></a>Tutoriel : Créer des liaisons de données

Supposons que vous avez conçu et implémenté une belle interface utilisateur remplie d'espaces réservés pour les images, de texte réutilisable « lorem ipsum » et de contrôles qui ne font rien pour le moment. Vous voulez maintenant la connecter à des données réelles et transformer ce prototype de conception en une application vivante.

Dans ce tutoriel, vous allez apprendre à remplacer votre texte réutilisable avec des liaisons de données et à créer d'autres liens directs entre votre interface utilisateur et vos données. Vous découvrirez également comment mettre en forme ou convertir vos données pour l’affichage, et synchroniser votre interface utilisateur et vos données. Après avoir suivi ce tutoriel, vous saurez simplifier et organiser du code XAML et C#, pour le rendre plus facile à maintenir et à étendre.

Vous allez commencer avec une version simplifiée de l’exemple PhotoLab. Cette version de démarrage comprend la couche de données complète ainsi que les mises en page XAML de base et exclut de nombreuses fonctionnalités afin que le code soit plus facile à parcourir. Comme ce tutoriel ne génère pas l’application complète, veillez à examiner la version finale pour voir des fonctionnalités, comme les animations personnalisées et les dispositions adaptatives. Vous la trouverez dans le dossier racine du référentiel [Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab).

L’exemple d’application PhotoLab comporte deux pages. La _page principale_ présente une vue de galerie de photos ainsi que des informations sur chaque fichier image.

![MainPage](../design/basics/images/xaml-basics/mainpage.png)

La *page de détails* affiche une seule photo une fois qu’elle a été sélectionnée. Un menu d’édition volant permet de modifier la photo, de la renommer et de l’enregistrer.

![DetailPage](../design/basics/images/xaml-basics/detailpage.png)

## <a name="prerequisites"></a>Prérequis

+ Visual Studio 2019 : [Téléchargez Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) (l’édition Community est gratuite.)
+ SDK Windows 10 (10.0.17763.0 ou ultérieur) :  [Télécharger le SDK Windows le plus récent (gratuit)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
+ Windows 10, version 1809 ou ultérieure

## <a name="part-0-get-the-starter-code-from-github"></a>Partie 0 : Obtenir le code de démarrage à partir de GitHub

Dans ce tutoriel, vous allez commencer avec une version simplifiée de l’exemple PhotoLab.

1. Accédez à la page GitHub de l’exemple : [https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab).
2. Vous devez ensuite cloner ou télécharger l’exemple. Sélectionnez le bouton **Clone or download**. Un sous-menu s’affiche.
    ![Menu Clone or download dans la page GitHub de l’exemple PhotoLab](../design/basics/images/xaml-basics/clone-repo.png)

    **Si vous n’êtes pas familiarisé avec GitHub :**

    a. Sélectionnez **Télécharger le zip**, puis enregistrez le fichier localement. Cela télécharge un fichier .zip contenant tous les fichiers projet dont vous avez besoin.

    b. Extrayez le fichier. Utilisez l’Explorateur de fichiers pour accéder au fichier .zip que vous venez de télécharger, cliquez dessus avec le bouton droit, puis sélectionnez **Extraire tout**.

    c. Accédez à votre copie locale de l’exemple, puis au répertoire `Windows-appsample-photo-lab-master\xaml-basics-starting-points\data-binding`.

    **Si vous maîtrisez GitHub :**

    a. Clonez localement la branche maîtresse du dépôt.

    b. Accédez au répertoire `Windows-appsample-photo-lab\xaml-basics-starting-points\data-binding`.

3. Double-cliquez sur `Photolab.sln` pour ouvrir la solution dans Visual Studio.

## <a name="part-1-replace-the-placeholders"></a>Partie 1 : Remplacer les espaces réservés

Vous allez créer des liaisons à usage unique dans le modèle de données XAML pour afficher les images réelles et leurs métadonnées à la place du contenu des espaces réservés.

Les liaisons à usage unique sont destinées à des données immuables en lecture seule. Elles sont donc très performantes et faciles à créer, ce qui permet d’afficher de grands jeux de données dans des contrôles `GridView` et `ListView`.

### <a name="replace-the-placeholders-with-one-time-bindings"></a>Remplacer les espaces réservés par des liaisons à usage unique

1. Ouvrez le dossier `xaml-basics-starting-points\data-binding` et lancez le fichier `PhotoLab.sln` dans Visual Studio.

2. Vérifiez que votre **plateforme de solution** est définie sur x86 ou x64 (et non ARM), puis exécutez l’application. Cela indique l’état de l’application avec les espaces réservés de l’interface utilisateur, avant l'ajout des liaisons.

    ![Application en cours d’exécution avec les images et le texte des espaces réservés](../design/basics/images/xaml-basics/gallery-with-placeholder-templates.png)

3. Ouvrez MainPage.xaml et recherchez un `DataTemplate` nommé **ImageGridView_DefaultItemTemplate**. Vous devez mettre à jour ce modèle pour utiliser les liaisons de données.

    **Avant :**

    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate">
    ```

    La valeur `x:Key` est utilisée par `ImageGridView` afin de sélectionner ce modèle pour l'affichage des objets de données.

4. Ajoutez une valeur `x:DataType` au modèle.

    **Après :**

    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate"
                  x:DataType="local:ImageFileInfo">
    ```

    `x:DataType` indique à quel type correspond le modèle. En l’occurrence, il s’agit d’un modèle pour la classe `ImageFileInfo` (où `local:` indique l’espace de noms local, tel que défini dans une déclaration xmlns dans la partie supérieure du fichier).

    `x:DataType` est obligatoire lorsque des expressions `x:Bind` sont utilisées dans un modèle de données, comme indiqué ci-dessous.

5. Dans le `DataTemplate`, recherchez l'élément `Image` nommé `ItemImage` et remplacez sa valeur `Source` comme indiqué.

    **Avant :**

    ```xaml
    <Image x:Name="ItemImage"
           Source="/Assets/StoreLogo.png"
           Stretch="Uniform" />
    ```

    **Après :**

    ```xaml
    <Image x:Name="ItemImage"
           Source="{x:Bind ImageSource}"
           Stretch="Uniform" />
    ```

    `x:Name` identifie un élément XAML pour permettre d’y faire référence ailleurs dans le code XAML et dans le code-behind.

    Les expressions `x:Bind` fournissent une valeur à une propriété de l’interface utilisateur en récupérant la valeur à partir d’une propriété **data-object**. Dans les modèles, la propriété indiquée est une propriété de la valeur définie pour `x:DataType`. Dans ce cas, la source de données est la propriété `ImageFileInfo.ImageSource`.

    > [!NOTE]
    > La valeur `x:Bind` indique également à l’éditeur le type de données ; vous pouvez donc utiliser IntelliSense au lieu de taper le nom de la propriété dans une expression `x:Bind`. Effectuez des tests dans le code que vous venez de coller : placez le curseur juste après `x:Bind` et appuyez sur la barre d’espace pour afficher la liste des propriétés avec lesquelles vous pouvez établir une liaison.

6. Remplacez les valeurs des autres contrôles d’interface utilisateur de la même façon. (Essayez de le faire avec IntelliSense au lieu de copier/coller !)

    **Avant :**

    ```xaml
    <TextBlock Text="Placeholder" ... />
    <StackPanel ... >
        <TextBlock Text="PNG file" ... />
        <TextBlock Text="50 x 50" ... />
    </StackPanel>
    <muxc:RatingControl Value="3" ... />
    ```

    **Après :**

    ```xaml
    <TextBlock Text="{x:Bind ImageTitle}" ... />
    <StackPanel ... >
        <TextBlock Text="{x:Bind ImageFileType}" ... />
        <TextBlock Text="{x:Bind ImageDimensions}" ... />
    </StackPanel>
    <muxc:RatingControl Value="{x:Bind ImageRating}" ... />
    ```

Exécutez l’application pour voir à quoi elle ressemble maintenant. Il n'y a plus aucun espace réservé ! Nous sommes bien partis.

![Application en cours d’exécution avec des images réelles et du texte à la place des espaces réservés](../design/basics/images/xaml-basics/gallery-with-populated-templates.png)

> [!Note]
> Si vous souhaitez aller plus loin, essayez d’ajouter un nouveau TextBlock au modèle de données et utilisez l’astuce IntelliSense x:Bind pour trouver une propriété à afficher.

## <a name="part-2-use-binding-to-connect-the-gallery-ui-to-the-images"></a>Partie 2 : Utiliser une liaison pour connecter l’interface utilisateur de la galerie aux images

Vous allez maintenant créer des liaisons à usage unique dans la page XAML pour connecter l’affichage de la galerie à la collection d’images, en remplaçant le code procédural existant qui s’en charge dans le code-behind. Vous allez également créer un bouton **Supprimer** pour voir comment l’affichage de galerie change lorsque les images sont supprimées de la collection. En même temps, vous allez apprendre à lier des événements aux gestionnaires d’événements pour bénéficier de davantage de souplesse qu’avec les gestionnaires d’événements classiques.

Toutes les liaisons traitées jusqu’à présent se trouvent à l’intérieur de modèles de données et font référence aux propriétés de la classe indiquée par la valeur `x:DataType`. Qu’en est-il du reste du code XAML de la page ?

Les expressions `x:Bind` qui se trouvent en dehors des modèles de données sont toujours liées à la page elle-même. Il est donc possible de faire référence à tout ce qui est placé dans le code-behind ou déclaré dans le code XAML, y compris les propriétés personnalisées et les propriétés des autres contrôles d’interface utilisateur de la page (à condition qu’ils aient une valeur `x:Name`).

Dans l’exemple PhotoLab, une utilisation possible de ce type de liaison consiste à connecter directement le contrôle `GridView` principal à la collection d’images, au lieu de le faire dans le code-behind. Vous verrez par la suite d’autres exemples.

### <a name="bind-the-main-gridview-control-to-the-images-collection"></a>Lier le contrôle GridView principal à la collection Images

1. Dans MainPage.xaml.cs, recherchez la méthode `GetItemsAsync` et supprimez le code qui définit `ItemsSource`.

    **Avant :**

    ```csharp
    ImageGridView.ItemsSource = Images;
    ```

    **Après :**

    ```csharp
    // Replaced with XAML binding:
    // ImageGridView.ItemsSource = Images;
    ```

2. Dans MainPage.xaml, recherchez le `GridView` nommé `ImageGridView` et ajoutez un attribut `ItemsSource`. Pour la valeur, utilisez une expression `x:Bind` qui fait référence à la propriété `Images` implémentée dans le code-behind.

    **Avant :**

    ```xaml
    <GridView x:Name="ImageGridView"
    ```

    **Après :**

    ```xaml
    <GridView x:Name="ImageGridView"
              ItemsSource="{x:Bind Images}"
    ```

    La propriété `Images` étant de type `ObservableCollection<ImageFileInfo>`, les éléments individuels affichés dans le `GridView` sont de type `ImageFileInfo`. ce qui correspond à la valeur `x:DataType` décrite dans la partie 1.

Toutes les liaisons que nous avons examinées jusqu'à présent sont des liaisons à usage unique en lecture seule, soit le comportement par défaut des expressions `x:Bind` simples. Les données ne sont chargées qu’à l’initialisation, ce qui donne des liaisons hautes performances, idéales pour prendre en charge plusieurs vues complexes de jeux de données volumineux.

Même la liaison `ItemsSource` que vous venez d'ajouter est une liaison à usage unique en lecture seule avec une valeur de propriété immuable, à une différence importante près. La valeur immuable de la propriété `Images` est une instance unique et spécifique d’une collection, initialisée une fois, comme on le voit ici.

```csharp
private ObservableCollection<ImageFileInfo> Images { get; }
    = new ObservableCollection<ImageFileInfo>();
```

La valeur de la propriété `Images` ne change jamais mais, comme la propriété est de type `ObservableCollection<T>`, le *contenu* de la collection peut changer, auquel cas la liaison remarque automatiquement les modifications et met à jour l’interface utilisateur.

Pour tester ce comportement, nous allons ajouter temporairement un bouton qui supprime l’image sélectionnée. Ce bouton ne se trouve pas dans la version finale, car le fait de sélectionner une image a pour effet de rediriger vers une page de détails. Toutefois, le comportement d’`ObservableCollection<T>` reste important dans l’exemple PhotoLab final, car le code XAML est initialisé dans le constructeur de la page (par le biais de l’appel de méthode `InitializeComponent`), mais la collection `Images` est remplie ultérieurement dans la méthode `GetItemsAsync`.

### <a name="add-a-delete-button"></a>Ajouter un bouton Supprimer

1. Dans MainPage.xaml, recherchez la `CommandBar` nommée **MainCommandBar** et ajoutez un nouveau bouton avant le bouton de zoom. (Les contrôles de zoom ne fonctionnent pas encore. Vous allez les associer dans la partie suivante de ce tutoriel.)

    ```xaml
    <AppBarButton Icon="Delete"
                  Label="Delete selected image"
                  Click="{x:Bind DeleteSelectedImage}" />
    ```

    Si vous connaissez déjà le langage XAML, cette valeur `Click` peut vous paraître inhabituelle. Dans les versions précédentes de XAML, vous deviez lui affecter une méthode avec une signature de gestionnaire d’événements spécifique, comprenant généralement des paramètres d’expéditeur de l’événement et un objet d'arguments propres à l’événement. Vous pouvez toujours utiliser cette technique si vous avez besoin des arguments de l’événement, mais vous avez également la possibilité de vous connecter à d’autres méthodes avec `x:Bind`. Par exemple, si les données de l’événement ne vous sont pas utiles, vous pouvez vous connecter à des méthodes dépourvues de paramètres, comme ici.

    <!-- TODO add doc links about event binding - and doc links in general? -->

2. Dans MainPage.xaml.cs, ajoutez la méthode `DeleteSelectedImage`.

    ```csharp
    private void DeleteSelectedImage() =>
        Images.Remove(ImageGridView.SelectedItem as ImageFileInfo);
    ```

    Cette méthode supprime simplement l’image sélectionnée de la collection `Images`.

Maintenant, exécutez l’application et utilisez le bouton pour supprimer quelques images. Comme vous pouvez le constater, l’interface utilisateur est automatiquement mise à jour, grâce à la liaison de données et au type `ObservableCollection<T>`.

> [!Note]
> Ce code supprime uniquement l’instance de `ImageFileInfo` de la collection `Images` dans l’application en cours d’exécution. Il ne supprime pas le fichier image de l’ordinateur.

## <a name="part-3-set-up-the-zoom-slider"></a>Partie 3 : Configurer le curseur de zoom

Dans cette partie, vous allez créer des liaisons unidirectionnelles d’un contrôle situé dans le modèle de données au curseur de zoom, qui se trouve en dehors du modèle. Vous découvrirez également que vous pouvez utiliser une liaison de données avec de nombreuses propriétés de contrôle, et pas seulement les plus évidentes comme `TextBlock.Text` et `Image.Source`.

### <a name="bind-the-image-data-template-to-the-zoom-slider"></a>Lier le modèle de données d’image au curseur de zoom

+ Recherchez le `DataTemplate` nommé `ImageGridView_DefaultItemTemplate` et remplacez les valeurs `**Height**` et `Width` du contrôle `Grid` en haut du modèle.

    **Avant**

    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate"
                  x:DataType="local:ImageFileInfo">
        <Grid Height="200"
              Width="200"
              Margin="{StaticResource LargeItemMargin}">
    ```

    **Après**

    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate"
                  x:DataType="local:ImageFileInfo">
        <Grid Height="{Binding Value, ElementName=ZoomSlider}"
              Width="{Binding Value, ElementName=ZoomSlider}"
              Margin="{StaticResource LargeItemMargin}">
    ```

Avez-vous remarqué que ce sont des expressions `Binding` et non des expressions `x:Bind` ? Il s’agit de l’ancienne méthode pour effectuer des liaisons de données, en grande partie obsolète. `x:Bind` fait presque tout ce que `Binding` fait et plus encore. Toutefois, dans un modèle de données, `x:Bind` se lie au type déclaré dans la valeur `x:DataType`. Comment lier un élément du modèle à un élément du code XAML de la page ou du code-behind ? Il faut utiliser une expression `Binding` à l’ancienne.

Les expressions `Binding` ne reconnaissent pas la valeur `x:DataType`, mais elles ont des valeurs `Binding` qui fonctionnent presque de la même manière. Ces valeurs indiquent au moteur de liaison que **Binding Value** est une liaison avec la propriété `Value` de l’élément spécifié sur la page (autrement dit, l’élément possédant cette valeur `x:Name`). Une liaison avec une propriété du code-behind ressemblerait à ```{Binding MyCodeBehindProperty, ElementName=page}```, où `page` fait référence à la valeur `x:Name` définie dans l'élément `Page` dans le code XAML.

> [!NOTE]
> Par défaut, les expressions `Binding` sont *unidirectionnelles*, ce qui signifie qu’elles mettent automatiquement à jour l’interface utilisateur lorsque la valeur de la propriété liée change.
>
> En revanche, `x:Bind` est par défaut *à usage unique*, ce qui signifie que toutes les modifications apportées à la propriété liée sont ignorées. Il s’agit de la valeur par défaut, car c'est l’option qui offre les plus hautes performances et la plupart des liaisons sont établies avec des données statiques en lecture seule.
>
> La leçon qu'il faut en tirer est que, si l’on utilise `x:Bind` avec des propriétés dont la valeur peut changer, il faut ajouter `Mode=OneWay` ou `Mode=TwoWay`. Vous trouverez des exemples dans la section suivante.

Exécutez l’application et utilisez le curseur pour modifier les dimensions du modèle d’image. Comme vous pouvez le constater, l’effet est assez puissant avec peu de code.

![Application en cours d’exécution avec affichage du curseur de zoom](../design/basics/images/xaml-basics/gallery-with-zoom-control.png)

> [!NOTE]
> Petit défi : essayez de lier d’autres propriétés de l’interface utilisateur à la propriété `Value` du curseur de zoom, ou à d’autres curseurs que vous ajoutez après le curseur de zoom. Par exemple, vous pouvez lier la propriété `FontSize` du `TitleTextBlock` à un nouveau curseur avec la valeur par défaut **24**. Veillez à définir des valeurs minimales et maximales raisonnables.

## <a name="part-4-improve-the-zoom-experience"></a>Partie 4 : Améliorer l’expérience de zoom

Dans cette partie, vous allez ajouter une propriété `ItemSize` personnalisée au code-behind et créer des liaisons unidirectionnelles du modèle d'image à la nouvelle propriété. La valeur `ItemSize` sera mise à jour par le curseur de zoom, ainsi que par d’autres facteurs comme le bouton bascule **Ajuster à l’écran** et la taille de la fenêtre, pour offrir une expérience plus précise.

Contrairement aux propriétés de contrôle intégrées, les propriétés personnalisées ne mettent pas automatiquement à jour l’interface utilisateur, même avec des liaisons unidirectionnelles et bidirectionnelles. Elles fonctionnent bien avec des liaisons *à usage unique*, mais certaines opérations sont nécessaires pour que les modifications apportées aux propriétés apparaissent réellement dans l’interface utilisateur.

### <a name="create-the-itemsize-property-so-that-it-updates-the-ui"></a>Créer la propriété ItemSize afin qu’elle mette à jour l’interface utilisateur

1. Dans MainPage.xaml.cs, modifiez la signature de la classe `MainPage` de sorte qu’elle implémente l'interface `INotifyPropertyChanged`.

    **Avant :**

    ```csharp
    public sealed partial class MainPage : Page
    ```

    **Après :**

    ```csharp
    public sealed partial class MainPage : Page, INotifyPropertyChanged
    ```

    Le système de liaison est ainsi informé du fait que `MainPage` a un événement `PropertyChanged` (ajouté ensuite) que les liaisons peuvent détecter pour mettre à jour l’interface utilisateur.

2. Ajoutez un événement `PropertyChanged` à la classe `MainPage`.

    ```csharp
    public event PropertyChangedEventHandler PropertyChanged;
    ```

    Cet événement fournit l’implémentation complète requise par l'interface `INotifyPropertyChanged`. Toutefois, il est nécessaire de déclencher explicitement l’événement dans les propriétés personnalisées pour qu’il fasse effet.

3. Ajoutez une propriété `ItemSize` et déclenchez l'événement `PropertyChanged` dans sa méthode setter.

    ```csharp
    public double ItemSize
    {
        get => _itemSize;
        set
        {
            if (_itemSize != value)
            {
                _itemSize = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(nameof(ItemSize)));
            }
        }
    }
    private double _itemSize;
    ```

    La propriété `ItemSize` expose la valeur d’un champ `_itemSize` privé. Un champ de stockage comme celui-ci permet à la propriété de vérifier si la nouvelle valeur est identique à l’ancienne avant de déclencher un événement `PropertyChanged` potentiellement inutile.

    L’événement lui-même est déclenché par la méthode `Invoke`. Le point d’interrogation vérifie si l'événement `PropertyChanged` est Null, c'est-à-dire si des gestionnaires d’événements ont déjà été ajoutés. Chaque liaison unidirectionnelle ou bidirectionnelle ajoute un gestionnaire d’événements en arrière-plan ; cependant, si aucun n’est à l’écoute, il ne se produit rien de plus. Si en revanche `PropertyChanged` n’est pas Null, la méthode `Invoke` est appelée avec une référence à la source d’événement (la page elle-même, représentée par le mot clé `this`) et un objet **event-args** qui indique le nom de la propriété. Grâce à ces informations, toutes les liaisons unidirectionnelles et bidirectionnelles avec la propriété `ItemSize` sont informées des modifications pour pouvoir mettre à jour l’interface utilisateur liée.

4. Dans MainPage.xaml, recherchez le `DataTemplate` nommé `ImageGridView_DefaultItemTemplate` et remplacez les valeurs `Height` et `Width` du contrôle `Grid` en haut du modèle. (Si vous avez effectué la liaison de contrôle à contrôle dans la partie précédente de ce tutoriel, les seules modifications à apporter consistent à remplacer `Value` par `ItemSize` et `ZoomSlider` par `page`. N’oubliez pas d’effectuer cette opération à la fois pour `Height` et pour `Width` !)

    **Avant**

    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate"
                  x:DataType="local:ImageFileInfo">
        <Grid Height="{Binding Value, ElementName=ZoomSlider}"
            Width="{Binding Value, ElementName=ZoomSlider}"
            Margin="{StaticResource LargeItemMargin}">
    ```

    **Après**

    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate"
                  x:DataType="local:ImageFileInfo">
        <Grid Height="{Binding ItemSize, ElementName=page}"
              Width="{Binding ItemSize, ElementName=page}"
              Margin="{StaticResource LargeItemMargin}">
    ```

Maintenant que l’interface utilisateur peut répondre aux modifications de `ItemSize`, vous devez effectuer quelques changements. Comme nous l’avons vu, la valeur `ItemSize` est calculée à partir de l’état actuel de plusieurs contrôles d’interface utilisateur, mais le calcul doit être effectué à chaque fois que ces contrôles changent d’état. Vous allez donc utiliser une liaison d’événement pour que certaines modifications de l’interface utilisateur appellent une méthode auxiliaire qui mette à jour `ItemSize`.

### <a name="update-the-itemsize-property-value"></a>Mettre à jour la valeur de la propriété ItemSize

1. Ajoutez la méthode `DetermineItemSize` à MainPage.xaml.cs.

    ```csharp
    private void DetermineItemSize()
    {
        if (FitScreenToggle != null
            && FitScreenToggle.IsOn == true
            && ImageGridView != null
            && ZoomSlider != null)
        {
            // The 'margins' value represents the total of the margins around the
            // image in the grid item. 8 from the ItemTemplate root grid + 8 from
            // the ItemContainerStyle * (Right + Left). If those values change,
            // this value needs to be updated to match.
            int margins = (int)this.Resources["LargeItemMarginValue"] * 4;
            double gridWidth = ImageGridView.ActualWidth -
                (int)this.Resources["DefaultWindowSidePaddingValue"];
            double ItemWidth = ZoomSlider.Value + margins;
            // We need at least 1 column.
            int columns = (int)Math.Max(gridWidth / ItemWidth, 1);

            // Adjust the available grid width to account for margins around each item.
            double adjustedGridWidth = gridWidth - (columns * margins);

            ItemSize = (adjustedGridWidth / columns);
        }
        else
        {
            ItemSize = ZoomSlider.Value;
        }
    }
    ```

2. Dans MainPage.xaml, naviguez vers le haut du fichier et ajoutez une liaison d’événement `SizeChanged` à l'élément `Page`.

    **Avant :**

    ```xaml
    <Page x:Name="page"
    ```

    **Après :**

    ```xaml
    <Page x:Name="page"
          SizeChanged="{x:Bind DetermineItemSize}"
    ```

3. Recherchez le `Slider` nommé `ZoomSlider` (dans la section `Page.Resources`) et ajoutez une liaison d’événement `ValueChanged`.

    **Avant :**

    ```xaml
    <Slider x:Name="ZoomSlider"
    ```

    **Après :**

    ```xaml
    <Slider x:Name="ZoomSlider"
            ValueChanged="{x:Bind DetermineItemSize}"
    ```

4. Recherchez le `ToggleSwitch` nommé `FitScreenToggle` et ajoutez une liaison d’événement `Toggled`.

    **Avant :**

    ```xaml
    <ToggleSwitch x:Name="FitScreenToggle"
    ```

    **Après :**

    ```xaml
    <ToggleSwitch x:Name="FitScreenToggle"
                  Toggled="{x:Bind DetermineItemSize}"
    ```

Exécutez l’application et utilisez le curseur de zoom et le bouton bascule **Ajuster à l’écran** pour modifier les dimensions du modèle d’image. Comme vous pouvez le constater, les dernières modifications offrent une expérience de zoom/redimensionnement plus précise sans désorganiser le code.

![Application en cours d’exécution avec la fonction Ajuster à l’écran activée](../design/basics/images/xaml-basics/gallery-with-fit-to-screen.png)

> [!NOTE]
> Petit défi : essayez d’ajouter un `TextBlock` après le `ZoomSlider` et de lier la propriété `Text` à la propriété `ItemSize`. Dans la mesure où il ne se trouve pas dans un modèle de données, vous pouvez utiliser `x:Bind` au lieu de `Binding` comme dans les liaisons `ItemSize` précédentes.

## <a name="part-5-enable-user-edits"></a>Partie 5 : Autoriser les modifications par l’utilisateur

Vous allez maintenant créer des liaisons bidirectionnelles pour permettre aux utilisateurs de mettre à jour les valeurs, notamment le titre de l’image, l'évaluation et divers effets visuels.

Pour cela, vous devez mettre à jour la `DetailPage` existante, qui fournit une visionneuse d’image unique, un contrôle de zoom et une interface utilisateur d'édition.

Toutefois, il faut d’abord lier la `DetailPage` pour que l’application y accède lorsque l’utilisateur clique sur une image dans l'affichage de la galerie.

### <a name="attach-the-detailpage"></a>Lier la DetailPage

1. Dans MainPage.xaml, recherchez le `GridView` nommé `ImageGridView` et ajoutez une valeur `ItemClick`.

    > [!TIP]
    > Si vous tapez la modification ci-dessous au lieu de la copier/coller, vous verrez s’afficher une fenêtre contextuelle IntelliSense indiquant « \<New Event Handler\> ». Si vous appuyez sur la touche Tab, un nom de gestionnaire de méthode par défaut sera choisi comme valeur et la méthode présentée à l’étape suivante sera automatiquement remplacée. Vous pourrez ensuite appuyer sur F12 pour accéder à la méthode dans le code-behind.

    **Avant :**

    ```xaml
    <GridView x:Name="ImageGridView"
    ```

    **Après :**

    ```xaml
    <GridView x:Name="ImageGridView"
              ItemClick="ImageGridView_ItemClick"
    ```

    > [!NOTE]
    > Nous utilisons ici un gestionnaire d’événements classique au lieu d’une expression x:Bind. En effet, nous avons besoin de voir les données d’événement, comme indiqué ci-dessous.

2. Dans MainPage.xaml.cs, ajoutez le gestionnaire d’événements (ou renseignez-le, si vous avez appliqué l'astuce de l’étape précédente).

    ```csharp
    private void ImageGridView_ItemClick(object sender, ItemClickEventArgs e)
    {
        this.Frame.Navigate(typeof(DetailPage), e.ClickedItem);
    }
    ```

    Cette méthode accède simplement à la page de détails, en transmettant l’élément cliqué, qui est un objet `ImageFileInfo` utilisé par **DetailPage.OnNavigatedTo** pour l'initialisation de la page. Vous n’aurez pas à implémenter cette méthode dans ce tutoriel, mais vous pouvez y jeter un œil pour voir ce qu’elle fait.

3. (Facultatif) Supprimez ou mettez en commentaires les contrôles ajoutés dans les précédents points de lecture qui fonctionnent avec l’image sélectionnée. Les conserver ne pose pas de problème, mais il est maintenant beaucoup plus difficile de sélectionner une image sans accéder à la page de détails.

Maintenant que vous avez connecté les deux pages, exécutez l’application et observez le résultat. Tout fonctionne à l’exception des contrôles sur le volet d’édition, qui ne répondent pas lorsque vous essayez de modifier les valeurs.

Comme vous pouvez le constater, la zone de texte du titre affiche le titre et vous permet de taper des modifications. Vous devez déplacer le focus sur un autre contrôle pour valider les modifications, mais le titre qui se trouve en haut à gauche de l’écran ne se met pas encore à jour.

Tous les contrôles sont déjà liés à l’aide des expressions `x:Bind` simples que nous avons présentées dans la partie 1. Pour rappel, cela signifie que ce sont toutes des liaisons à usage unique, ce qui explique pourquoi les modifications apportées aux valeurs ne sont pas enregistrées. Pour résoudre ce problème, il suffit de les transformer en liaisons bidirectionnelles.

### <a name="make-the-editing-controls-interactive"></a>Rendre les contrôles d’édition interactifs

1. Dans DetailPage.xaml, recherchez le `TextBlock` nommé **TitleTextBlock** et le contrôle **RatingControl** qui se trouve après, et mettez à jour leurs expressions `x:Bind` de façon à indiquer **Mode=TwoWay**.

    **Avant :**

    ```xaml
    <TextBlock x:Name="TitleTextBlock"
               Text="{x:Bind item.ImageTitle}"
               ... >
    <muxc:RatingControl Value="{x:Bind item.ImageRating}"
                            ... >
    ```

    **Après :**

    ```xaml
    <TextBlock x:Name="TitleTextBlock"
               Text="{x:Bind item.ImageTitle, Mode=TwoWay}"
               ... >
    <muxc:RatingControl Value="{x:Bind item.ImageRating, Mode=TwoWay}"
                            ... >
    ```

2. Faites de même pour tous les curseurs d'effet qui suivent le contrôle d’évaluation.

    ```xaml
    <Slider Header="Exposure"    ... Value="{x:Bind item.Exposure, Mode=TwoWay}" ...
    <Slider Header="Temperature" ... Value="{x:Bind item.Temperature, Mode=TwoWay}" ...
    <Slider Header="Tint"        ... Value="{x:Bind item.Tint, Mode=TwoWay}" ...
    <Slider Header="Contrast"    ... Value="{x:Bind item.Contrast, Mode=TwoWay}" ...
    <Slider Header="Saturation"  ... Value="{x:Bind item.Saturation, Mode=TwoWay}" ...
    <Slider Header="Blur"        ... Value="{x:Bind item.Blur, Mode=TwoWay}" ...
    ```

Le mode bidirectionnel signifie, comme on pourrait s’y attendre, que les données se déplacent dans les deux sens chaque fois que des modifications sont apportées de part et d'autre.

Comme les liaisons unidirectionnelles abordées précédemment, ces liaisons bidirectionnelles mettent à présent à jour l’interface utilisateur chaque fois que les propriétés liées changent, grâce à l'implémentation de `INotifyPropertyChanged` dans la classe `ImageFileInfo`. Toutefois, les valeurs se déplacent également de l’interface utilisateur vers les propriétés liées à chaque fois que l’utilisateur interagit avec le contrôle. Rien d'autre n'est nécessaire du côté du code XAML.

Exécutez l’application et essayez les contrôles d’édition. Comme vous pouvez le constater, les modifications effectuées affectent à présent les valeurs des images et sont conservées lorsque vous revenez à la page principale.

## <a name="part-6-format-values-through-function-binding"></a>Partie 6 : Mettre en forme les valeurs par le biais d'une liaison de fonction

Il reste un dernier problème à résoudre. Lorsque vous déplacez les curseurs d'effet, les étiquettes qui se trouvent à côté ne changent toujours pas.

![Curseurs d'effet avec les valeurs d'étiquette par défaut](../design/basics/images/xaml-basics/effect-sliders-before-label-fix.png)

La dernière partie de ce tutoriel consiste à ajouter des liaisons qui mettent en forme la valeur des curseurs pour l’affichage.

### <a name="bind-the-effect-slider-labels-and-format-the-values-for-display"></a>Lier les étiquettes des curseurs d’effet et mettre en forme les valeurs pour l'affichage

1. Recherchez le `TextBlock` après le curseur `Exposure` et remplacez la valeur `Text` par l’expression de liaison indiquée ici.

    **Avant :**

    ```xaml
    <Slider Header="Exposure" ... />
    <TextBlock ... Text="0.00" />
    ```

    **Après :**

    ```xaml
    <Slider Header="Exposure" ... />
    <TextBlock ... Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />
    ```

    Ce type de liaison est appelé liaison de fonction, car il consiste à établir une liaison avec la valeur de retour d’une méthode. La méthode doit être accessible par le biais du code-behind de la page ou du type `x:DataType` si vous vous trouvez dans un modèle de données. Dans ce cas, il faut utiliser la méthode .NET `ToString` bien connue, accessible au moyen de la propriété d’élément de la page, puis de la propriété `Exposure` de l’élément. (Cet exemple montre comment établir des liaisons avec des méthodes et des propriétés profondément imbriquées dans une chaîne de connexions.)

    La liaison de fonction est un moyen idéal de mettre en forme des valeurs pour l’affichage, car il est possible de transmettre d’autres sources de liaison comme arguments de méthode ; l’expression de liaison détectera les modifications apportées à ces valeurs comme avec le mode unidirectionnel. Dans cet exemple, l'argument **culture** est une référence à un champ immuable implémenté dans le code-behind, mais il aurait pu tout aussi bien s'agir d'une propriété qui déclenche des événements `PropertyChanged`. Dans ce cas, toute modification apportée à la valeur de propriété conduit l'expression `x:Bind` à appeler `ToString` avec la nouvelle valeur, puis à mettre à jour l’interface utilisateur avec le résultat.

2. Faites de même pour les `TextBlock` correspondant aux étiquettes des autres curseurs d’effet.

    ```xaml
    <Slider Header="Temperature" ... />
    <TextBlock ... Text="{x:Bind item.Temperature.ToString('N', culture), Mode=OneWay}" />

    <Slider Header="Tint" ... />
    <TextBlock ... Text="{x:Bind item.Tint.ToString('N', culture), Mode=OneWay}" />

    <Slider Header="Contrast" ... />
    <TextBlock ... Text="{x:Bind item.Contrast.ToString('N', culture), Mode=OneWay}" />

    <Slider Header="Saturation" ... />
    <TextBlock ... Text="{x:Bind item.Saturation.ToString('N', culture), Mode=OneWay}" />

    <Slider Header="Blur" ... />
    <TextBlock ... Text="{x:Bind item.Blur.ToString('N', culture), Mode=OneWay}" />
    ```

À présent, lorsque vous exécutez l’application, tout fonctionne, y compris les étiquettes de curseur.

![Curseurs d'effet avec des étiquettes fonctionnelles](../design/basics/images/xaml-basics/effect-sliders-after-label-fix.png)

## <a name="conclusion"></a>Conclusion

Ce tutoriel vous a donné un aperçu de la liaison de données et vous a montré quelques-unes des fonctionnalités disponibles. Un mot d’avertissement avant de conclure : toutes les liaisons ne sont pas possibles et il peut arriver que des valeurs soient incompatibles avec certaines propriétés. La liaison offre une grande souplesse, mais elle ne fonctionne pas dans tous les cas.

Un exemple de problème non résolu par une liaison est le cas où un contrôle ne possède aucune propriété adaptée à la liaison, comme avec la fonctionnalité de zoom de la page de détails. Ce curseur de zoom doit interagir avec le `ScrollViewer` qui affiche l’image, mais `ScrollViewer` ne peut être mis à jour que par le biais de sa méthode `ChangeView`. Ici, nous utilisons des gestionnaires d’événements conventionnels pour maintenir la synchronisation entre `ScrollViewer` et le curseur de zoom. Pour plus d’informations, consultez les méthodes `ZoomSlider_ValueChanged` et `MainImageScroll_ViewChanged` dans `DetailPage`.

Néanmoins, la liaison est un moyen puissant et souple de simplifier le code et d'établir une distinction entre la logique de l’interface utilisateur et celle des données. Cela facilite grandement les réglages de part et d’autre de cette division, tout en limitant les risques d’introduire des bogues de l’autre côté.

Un exemple de séparation des données et de l’interface utilisateur est l'utilisation de la propriété `ImageFileInfo.ImageTitle`. Cette propriété (ainsi que la propriété `ImageRating`) est légèrement différente de la propriété `ItemSize` que vous avez créée dans la partie 4, car la valeur est stockée dans les métadonnées du fichier (exposées par le biais du type `ImageProperties`) plutôt que dans un champ. En outre, `ImageTitle` retourne la valeur `ImageName` (définie sur le nom de fichier) s’il n’existe aucun titre dans les métadonnées du fichier.

```csharp
public string ImageTitle
{
    get => String.IsNullOrEmpty(ImageProperties.Title) ? ImageName : ImageProperties.Title;
    set
    {
        if (ImageProperties.Title != value)
        {
            ImageProperties.Title = value;
            var ignoreResult = ImageProperties.SavePropertiesAsync();
            OnPropertyChanged();
        }
    }
}
```

Comme vous pouvez le constater, la méthode setter met à jour la propriété `ImageProperties.Title`, puis appelle `SavePropertiesAsync` pour écrire la nouvelle valeur dans le fichier. (Il s’agit d’une méthode asynchrone, mais il n’est pas possible d’utiliser le mot clé `await` dans une propriété, ce qui n'est de toute façon pas souhaitable puisque les méthodes getter et setter de propriétés doivent se terminer immédiatement. Appelez plutôt la méthode et ignorez l’objet `Task` qu’elle retourne.)

## <a name="going-further"></a>Aller plus loin

Maintenant que vous avez suivi ce labo, vous avez suffisamment de connaissances en liaisons pour résoudre les problèmes par vous-même.

Comme vous l'avez peut-être remarqué, si vous modifiez le niveau de zoom sur la page de détails, il se réinitialise automatiquement lorsque vous revenez en arrière, puis sélectionnez de nouveau la même image. Saurez-vous trouver un moyen de conserver et de restaurer le niveau de zoom de chaque image individuellement ? Bonne chance !

Vous disposez normalement de toutes les informations nécessaires dans ce tutoriel, mais, si vous avez besoin d’une aide supplémentaire, il suffit d'un clic pour accéder à la documentation sur la liaison de données. Commencez ici :

+ [Extension de balisage {x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)
+ [Présentation détaillée de la liaison de données](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)