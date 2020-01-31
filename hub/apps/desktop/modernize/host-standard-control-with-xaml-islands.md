---
description: Cet article explique comment héberger un contrôle UWP standard dans une application WPF à l’aide des îlots XAML.
title: Héberger un contrôle UWP standard dans une application WPF à l’aide des îlots XAML
ms.date: 01/24/2020
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, îlots XAML, contrôles encapsulés, contrôles standard, InkCanvas, InkToolbar
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: e7fa4a3d97354efdfe4514ea69230883fffdb3cd
ms.sourcegitcommit: 1455e12a50f98823bfa3730c1d90337b1983b711
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76814019"
---
# <a name="host-a-standard-uwp-control-in-a-wpf-app-using-xaml-islands"></a>Héberger un contrôle UWP standard dans une application WPF à l’aide des îlots XAML

Cet article illustre deux façons d’héberger un contrôle UWP standard (autrement dit, un contrôle UWP de première partie fourni par la bibliothèque SDK Windows ou WinUI) dans une application WPF à l’aide des [îlots XAML](xaml-islands.md):

* Il montre comment héberger un contrôle [UWP et](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) des contrôles [InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) à l’aide de [contrôles encapsulés](xaml-islands.md#wrapped-controls) dans la boîte à outils de la communauté Windows. Ces contrôles encapsulent l’interface et les fonctionnalités d’un petit ensemble de contrôles UWP utiles. Vous pouvez les ajouter directement à l’aire de conception de votre projet WPF ou Windows Forms, puis les utiliser comme n’importe quel autre contrôle WPF ou Windows Forms dans le concepteur.

* Il montre également comment héberger un contrôle de panneau de configuration UWP à l’aide du contrôle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) dans le kit [de commandes de](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) la communauté Windows. Étant donné que seul un petit ensemble de contrôles UWP est disponible sous forme de contrôles encapsulés, vous pouvez utiliser [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) pour héberger n’importe quel autre contrôle UWP standard.

Bien que cet article montre comment héberger des contrôles UWP dans une application WPF, le processus est similaire pour une application Windows Forms.

## <a name="required-components"></a>Composants requis

Pour héberger un contrôle UWP dans une application WPF (ou Windows Forms), vous avez besoin des composants suivants dans votre solution. Cet article fournit des instructions sur la création de chacun de ces composants.

* **Le projet et le code source de votre application**. L’utilisation du contrôle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) pour héberger des contrôles UWP internes standard est prise en charge dans les applications qui ciblent le .NET Framework ou .net Core 3.

* **Projet d’application UWP qui définit une classe d’application racine qui dérive de XamlApplication**. Votre projet WPF ou Windows Forms doit avoir accès à une instance de la classe [Microsoft. Toolkit. Win32. UI. XamlHost. XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) fournie par le kit de connaissances de la communauté Windows. Cet objet joue le rôle de fournisseur de métadonnées racine pour le chargement des métadonnées des types XAML UWP personnalisés dans les assemblys du répertoire actif de votre application.

    Pour ce faire, la méthode recommandée consiste à ajouter un projet d' **application vide (Windows universel)** à la même solution que votre projet WPF ou Windows Forms, à modifier la classe de `App` par défaut de ce projet pour dériver de `XamlApplication`, puis à créer une instance de cet objet dans le code de point d’entrée de votre application.

    > [!NOTE]
    > Bien que ce composant ne soit pas obligatoire pour les scénarios d’îlot XAML trivial tels que l’hébergement d’un contrôle UWP de premier tiers, votre application a besoin de cet objet `XamlApplication` pour prendre en charge l’ensemble des scénarios d’îlot XAML, y compris l’hébergement de contrôles UWP personnalisés. Par conséquent, nous vous recommandons de toujours définir un objet `XamlApplication` dans une solution dans laquelle vous utilisez des îlots XAML.

    > [!NOTE]
    > Votre solution ne peut contenir qu’un seul projet qui définit un objet `XamlApplication`. Tous les contrôles UWP personnalisés de votre application partagent le même `XamlApplication` objet. Le projet qui définit l’objet `XamlApplication` doit inclure des références à toutes les autres bibliothèques et projets UWP utilisés pour héberger les contrôles UWP dans l’îlot XAML.

## <a name="create-a-wpf-project"></a>Créer un projet WPF

Avant de commencer, suivez ces instructions pour créer un projet WPF et le configurer pour héberger des îlots XAML. Si vous avez un projet WPF existant, vous pouvez adapter ces étapes et des exemples de code pour votre projet.

1. Dans Visual Studio 2019, créez une **application WPF (.NET Framework)** ou un projet d' **application WPF (.net Core)** . Si vous souhaitez créer un projet d' **application WPF (.net Core)** , vous devez d’abord installer la dernière version du [Kit de développement logiciel (SDK) .net Core 3](https://dotnet.microsoft.com/download/dotnet-core/3.0).

2. Vérifiez que les [références de package](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) sont activées :

    1. Dans Visual Studio, cliquez sur **Outils-> gestionnaire de package NuGet-> paramètres du gestionnaire de package**.
    2. Assurez-vous que **PackageReference** est sélectionné pour le **format de gestion des packages par défaut**.

3. Dans **Explorateur de solutions** , cliquez avec le bouton droit sur votre projet WPF, puis choisissez **gérer les packages NuGet**.

4. Dans la fenêtre **Gestionnaire de package NuGet** , assurez-vous que l’option **inclure la version préliminaire** est sélectionnée.

5. Sélectionnez l’onglet **Parcourir** , recherchez le package [Microsoft. Toolkit. WPF. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) (version v 6.0.0 ou ultérieure), puis installez le package. Ce package fournit tout ce dont vous avez besoin pour utiliser les contrôles UWP encapsulés pour WPF (y compris [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) et [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) , ainsi que le contrôle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) .
    > [!NOTE]
    > Windows Forms applications doivent utiliser le package [Microsoft. Toolkit. Forms. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls) (version v 6.0.0 ou ultérieure).

6. Configurez votre solution pour cibler une plateforme spécifique, telle que x86 ou x64. La plupart des scénarios des îlots XAML ne sont pas pris en charge dans les projets qui ciblent **Any CPU**.

    1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de la solution, puis sélectionnez **Propriétés** -> **Propriétés de configuration** -> **Configuration Manager**. 
    2. Sous **plateforme de la solution active**, sélectionnez **nouveau**. 
    3. Dans la boîte de dialogue **nouvelle plateforme de solution** , sélectionnez **x64** ou **x86** , puis cliquez sur **OK**. 
    4. Fermez les boîtes de dialogue ouvertes.

## <a name="define-a-xamlapplication-class-in-a-uwp-app-project"></a>Définir une classe XamlApplication dans un projet d’application UWP

Ensuite, ajoutez un projet d’application UWP à la même solution que votre projet WPF. Vous allez modifier la classe de `App` par défaut de ce projet pour qu’elle dérive de la classe [Microsoft. Toolkit. Win32. UI. XamlHost. XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) fournie par le kit de connaissances de la communauté Windows. Pour plus d’informations sur l’objectif de cette classe, consultez [cette section](#required-components).

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de la solution et sélectionnez **Ajouter** -> **nouveau projet**.
2. Ajoutez un projet **Application vide (Windows universelle)** à votre solution. Assurez-vous que la version cible et la version minimale sont toutes deux définies sur **Windows 10, version 1903** ou ultérieure.
3. Dans le projet d’application UWP, installez le package NuGet [Microsoft. Toolkit. Win32. UI. XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) (version v 6.0.0 ou ultérieure).
4. Ouvrez le fichier **app. Xaml** et remplacez le contenu de ce fichier par le code XAML suivant. Remplacez `MyUWPApp` par l’espace de noms de votre projet d’application UWP.

    ```xml
    <xaml:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:xaml="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns:local="using:MyUWPApp">
    </xaml:XamlApplication>
    ```

5. Ouvrez le fichier **app.Xaml.cs** et remplacez le contenu de ce fichier par le code suivant. Remplacez `MyUWPApp` par l’espace de noms de votre projet d’application UWP.

    ```csharp
    namespace MyUWPApp
    {
        sealed partial class App : Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication
        {
            public App()
            {
                this.Initialize();
            }
        }
    }
    ```

6. Supprimez le fichier **MainPage. Xaml** du projet d’application UWP.
7. Générez le projet d’application UWP.
8. Dans votre projet WPF, cliquez avec le bouton droit sur le nœud **dépendances** et ajoutez une référence à votre projet d’application UWP.

## <a name="instantiate-the-xamlapplication-object-in-the-entry-point-of-your-wpf-app"></a>Instancier l’objet XamlApplication dans le point d’entrée de votre application WPF

Ensuite, ajoutez du code au point d’entrée de votre application WPF pour créer une instance de la classe `App` que vous venez de définir dans le projet UWP (il s’agit de la classe qui dérive maintenant de `XamlApplication`). Pour plus d’informations sur l’objectif de cet objet, consultez [cette section](#required-components).

1. Dans votre projet WPF, cliquez avec le bouton droit sur le nœud du projet, sélectionnez **ajouter** -> **nouvel élément**, puis sélectionnez **classe**. Nommez le **programme** de classe, puis cliquez sur **Ajouter**.

2. Remplacez la classe `Program` générée par le code suivant, puis enregistrez le fichier. Remplacez `MyUWPApp` par l’espace de noms de votre projet d’application UWP, puis remplacez `MyWPFApp` par l’espace de noms de votre projet d’application WPF.

    ```csharp
    public class Program
    {
        [System.STAThreadAttribute()]
        public static void Main()
        {
            using (new MyUWPApp.App())
            {
                MyWPFApp.App app = new MyWPFApp.App();
                app.InitializeComponent();
                app.Run();
            }
        }
    }
    ```

3. Cliquez avec le bouton droit sur le nœud du projet et choisissez **Propriétés**.

4. Sous l’onglet **application** des propriétés, cliquez sur la liste déroulante **objet de démarrage** et choisissez le nom qualifié complet de la classe `Program` que vous avez ajoutée à l’étape précédente. 
    > [!NOTE]
    > Par défaut, les projets WPF définissent une fonction de point d’entrée `Main` dans un fichier de code généré qui n’est pas destiné à être modifié. Cette étape remplace le point d’entrée de votre projet par la méthode `Main` de la nouvelle classe `Program`, qui vous permet d’ajouter le code qui s’exécute aussi tôt que possible dans le processus de démarrage de l’application. 

5. Enregistrez les modifications apportées aux propriétés du projet.

## <a name="host-an-inkcanvas-and-inktoolbar-by-using-wrapped-controls"></a>Héberger un InkCanvas et un InkToolbar à l’aide de contrôles encapsulés

Maintenant que vous avez configuré votre projet pour qu’il utilise des îlots XAML UWP, vous êtes maintenant prêt à ajouter les contrôles [UWP et](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) encapsulés dans l’application.

1. Dans **Explorateur de solutions**, ouvrez le fichier **MainWindow. Xaml** .

2. Dans l’élément de **fenêtre** près du haut du fichier XAML, ajoutez l’attribut suivant. Cela fait référence à l’espace de noms XAML pour le contrôle UWP inclus dans [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) et [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) .

    ```xml
    xmlns:Controls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    Après avoir ajouté cet attribut, l’élément de **fenêtre** doit maintenant ressembler à ce qui suit.

    ```xml
    <Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:local="clr-namespace:WpfApp"
            xmlns:Controls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
            x:Class="WpfApp.MainWindow"
            mc:Ignorable="d"
            Title="MainWindow" Height="800" Width="800">
    ```

3. Dans le fichier **MainWindow. Xaml** , remplacez l’élément `<Grid>` existant par le code XAML suivant. Ce code XAML ajoute un contrôle [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) et [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) (préfixé par le mot clé **Controls** que vous avez défini précédemment en tant qu’espace de noms) au `<Grid>`.

    ```xml
    <Grid Margin="10,50,10,10">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <Controls:InkToolbar x:Name="myInkToolbar" TargetInkCanvas="{x:Reference myInkCanvas}" Grid.Row="0" Width="300"
            Height="50" Margin="10,10,10,10" HorizontalAlignment="Left" VerticalAlignment="Top" />
        <Controls:InkCanvas x:Name="myInkCanvas" Grid.Row="1" HorizontalAlignment="Left" Width="600" Height="400"
            Margin="10,10,10,10" VerticalAlignment="Top" />
    </Grid>
    ```

    > [!NOTE]
    > Vous pouvez également ajouter ces contrôles encapsulés à la fenêtre en les faisant glisser de la section de la **boîte à outils** de la **communauté Windows** de la boîte à outils vers le concepteur.

4. Enregistrez le fichier **MainWindow. Xaml** .

    Si vous avez un appareil qui prend en charge un stylet numérique, comme une surface, et que vous exécutez ce laboratoire sur un ordinateur physique, vous pouvez maintenant générer et exécuter l’application et dessiner de l’encre numérique sur l’écran avec le stylet. Toutefois, si vous n’avez pas de périphérique avec stylet et que vous essayez de vous connecter avec la souris, rien ne se produit. Cela est dû au fait que le contrôle **InkCanvas** est activé uniquement pour les stylets numériques par défaut. Toutefois, vous pouvez modifier ce comportement.

5. Ouvrez le fichier **MainWindow.Xaml.cs** .

6. Ajoutez la déclaration d’espace de noms suivante en haut du fichier :

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

7. Recherchez le constructeur `MainWindow()`. Ajoutez la ligne de code suivante après la méthode `InitializeComponent()` et enregistrez le fichier de code.

    ```csharp
    this.myInkCanvas.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    Vous pouvez utiliser l’objet **InkPresenter** pour personnaliser l’expérience d’encrage par défaut. Ce code utilise la propriété **InputDeviceTypes** pour activer l’entrée de la souris et du stylet.

8. Appuyez de nouveau sur F5 pour régénérer et exécuter l’application dans le débogueur. Si vous utilisez un ordinateur avec une souris, confirmez que vous pouvez dessiner un élément dans l’espace du canevas d’encre avec la souris.

## <a name="host-a-calendarview-by-using-the-host-control"></a>Héberger un CalendarView à l’aide du contrôle hôte

Maintenant que vous avez ajouté les contrôles [UWP et](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) encapsulés à l’application, vous êtes maintenant prêt à utiliser le contrôle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) pour ajouter un [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) à l’application.

> [!NOTE]
> Le contrôle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) est fourni par le package [Microsoft. Toolkit. WPF. UI. XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost) . Ce package est inclus dans le package [Microsoft. Toolkit. WPF. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) que vous avez installé précédemment.

1. Dans **Explorateur de solutions**, ouvrez le fichier **MainWindow. Xaml** .

2. Dans l’élément de **fenêtre** près du haut du fichier XAML, ajoutez l’attribut suivant. Cela fait référence à l’espace de noms XAML pour le contrôle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) .

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    Après avoir ajouté cet attribut, l’élément de **fenêtre** doit maintenant ressembler à ce qui suit.

    ```xml
    <Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:local="clr-namespace:WpfApp"
            xmlns:Controls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
            xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
            x:Class="WpfApp.MainWindow"
            mc:Ignorable="d"
            Title="MainWindow" Height="800" Width="800">
    ```

4. Dans le fichier **MainWindow. Xaml** , remplacez l’élément `<Grid>` existant par le code XAML suivant. Ce code XAML ajoute une ligne à la grille et ajoute un objet [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) à la dernière ligne. Pour héberger un contrôle de panneau de [configuration UWP,](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) ce code XAML définit la propriété `InitialTypeName` sur le nom qualifié complet du contrôle. Ce code XAML définit également un gestionnaire d’événements pour l’événement `ChildChanged`, qui est déclenché lorsque le contrôle hébergé a été rendu.

    ```xml
    <Grid Margin="10,50,10,10">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <Controls:InkToolbar x:Name="myInkToolbar" TargetInkCanvas="{x:Reference myInkCanvas}" Grid.Row="0" Width="300"
            Height="50" Margin="10,10,10,10" HorizontalAlignment="Left" VerticalAlignment="Top" />
        <Controls:InkCanvas x:Name="myInkCanvas" Grid.Row="1" HorizontalAlignment="Left" Width="600" Height="400" 
            Margin="10,10,10,10" VerticalAlignment="Top" />
        <xamlhost:WindowsXamlHost x:Name="myCalendar" InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Row="2" 
              Margin="10,10,10,10" Width="600" Height="300" ChildChanged="MyCalendar_ChildChanged"  />
    </Grid>
    ```

5. Enregistrez le fichier **MainWindow. Xaml** et ouvrez le fichier **MainWindow.Xaml.cs** .

7. Ajoutez la déclaration d’espace de noms suivante en haut du fichier :

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

10. Ajoutez la méthode de gestionnaire d’événements `ChildChanged` suivante à la classe `MainWindow` et enregistrez le fichier de code. Lorsque le contrôle hôte a été rendu, ce gestionnaire d’événements s’exécute et crée un gestionnaire d’événements simple pour l’événement `SelectedDatesChanged` du contrôle calendrier.

    ```csharp
    private void MyCalendar_ChildChanged(object sender, EventArgs e)
    {
        WindowsXamlHost windowsXamlHost = (WindowsXamlHost)sender;

        Windows.UI.Xaml.Controls.CalendarView calendarView =
            (Windows.UI.Xaml.Controls.CalendarView)windowsXamlHost.Child;

        if (calendarView != null)
        {
            calendarView.SelectedDatesChanged += (obj, args) =>
            {
                if (args.AddedDates.Count > 0)
                {
                    MessageBox.Show("The user selected a new date: " + 
                        args.AddedDates[0].DateTime.ToString());
                }
            };
        }
    }
    ```

11. Appuyez de nouveau sur F5 pour régénérer et exécuter l’application dans le débogueur. Vérifiez que le contrôle calendrier s’affiche désormais en bas de la fenêtre.

## <a name="package-the-app"></a>Empaqueter l’application

Vous pouvez éventuellement empaqueter l’application WPF dans un [package MSIX](https://docs.microsoft.com/windows/msix) pour le déploiement. MSIX est la technologie d’empaquetage d’applications moderne pour Windows et est basée sur une combinaison de technologies d’installation MSI, APPX, App-V et ClickOnce.

Les instructions suivantes vous montrent comment empaqueter tous les composants de la solution dans un package MSIX à l’aide du [projet de packaging des applications Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) dans Visual Studio 2019. Ces étapes sont nécessaires uniquement si vous souhaitez empaqueter l’application WPF dans un package MSIX.

1. Ajoutez un nouveau [projet d’empaquetage d’applications Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) à votre solution. Au fur et à mesure que vous créez le projet, sélectionnez **Windows 10, version 1903 (10,0 ; Build 18362)** pour la **version cible** et la **version minimale**.

2. Dans le projet d’empaquetage, cliquez avec le bouton droit sur le nœud **applications** et choisissez **Ajouter une référence**. Dans la liste des projets, sélectionnez le projet WPF dans votre solution, puis cliquez sur **OK**.

3. Configurez votre solution pour cibler une plateforme spécifique, telle que x86 ou x64. Cela est requis pour générer l’application WPF dans un package MSIX à l’aide du projet de création de packages d’applications Windows.

    1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de la solution, puis sélectionnez **Propriétés** -> **Propriétés de configuration** -> **Configuration Manager**.
    2. Sous **plateforme de la solution active**, sélectionnez **x64** ou **x86**.
    3. Dans la ligne de votre projet WPF, dans la colonne **plateforme** , sélectionnez **nouveau**.
    4. Dans la boîte de dialogue **nouvelle plateforme de solution** , sélectionnez **x64** ou **x86** (la même plateforme que celle que vous avez sélectionnée pour **plateforme de solution active**), puis cliquez sur **OK**.
    5. Fermez les boîtes de dialogue ouvertes.

5. Générez et exécutez le projet de Packaging. Vérifiez que WPF s’exécute et que le contrôle personnalisé UWP s’affiche comme prévu.

## <a name="related-topics"></a>Rubriques connexes

* [Héberger des contrôles XAML UWP dans les applications de bureau (îlots XAML)](xaml-islands.md)
* [Exemples de code des îlots XAML](https://github.com/microsoft/Xaml-Islands-Samples)
* [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)
* [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar)
* [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)
