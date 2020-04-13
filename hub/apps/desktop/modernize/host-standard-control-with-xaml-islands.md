---
description: Cet article explique comment héberger un contrôle UWP standard dans une application WPF à l’aide d’îlots XAML.
title: Héberger un contrôle UWP standard dans une application WPF à l’aide d’îlots XAML
ms.date: 01/24/2020
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, îlots xaml, contrôles wrappés, contrôles standard, InkCanvas, InkToolbar
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: ed6aa406cd1372819c25bd43b59cd416130b09e0
ms.sourcegitcommit: df0cd9c82d1c0c17ccde424e3c4a6ff680c31a35
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80482513"
---
# <a name="host-a-standard-uwp-control-in-a-wpf-app-using-xaml-islands"></a>Héberger un contrôle UWP standard dans une application WPF à l’aide d’îlots XAML

Cet article illustre deux façons d’héberger un contrôle UWP standard (autrement dit, un contrôle UWP interne fourni par le SDK Windows) dans une application WPF à l’aide d’[îlots XAML](xaml-islands.md) :

* Il montre comment héberger un contrôle [InkCanvas](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) et un contrôle [InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) à l’aide de [contrôles wrappés](xaml-islands.md#wrapped-controls) disponibles dans le Windows Community Toolkit. Ces contrôles wrappent l’interface et les fonctionnalités d’un petit ensemble de contrôles UWP utiles. Vous pouvez les ajouter directement à l’aire de conception de votre projet WPF ou Windows Forms, puis les utiliser comme tout autre contrôle WPF ou Windows Forms dans le concepteur.

* Il montre également comment héberger un contrôle [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) UWP à l’aide du contrôle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) disponible dans le Windows Community Toolkit. Seul un petit ensemble de contrôles UWP étant disponible sous forme de contrôles wrappés, vous pouvez utiliser [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) pour héberger n’importe quel autre contrôle UWP standard.

Cet article montre comment héberger des contrôles UWP dans une application WPF, mais sachez que le processus est similaire pour une application Windows Forms.

## <a name="required-components"></a>Composants requis

Pour héberger un contrôle UWP dans une application WPF (ou Windows Forms), vous avez besoin des composants suivants dans votre solution. Cet article fournit des instructions sur la création de chacun de ces composants.

* **Le projet et le code source de votre application**. L’utilisation du contrôle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) pour héberger des contrôles UWP internes standard est prise en charge dans les applications qui ciblent le .NET Framework ou .NET Core 3.

* **Un projet d’application UWP qui définit une classe Application racine dérivant de XamlApplication**. Votre projet WPF ou Windows Forms doit avoir accès à une instance de la classe [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) fournie par le Windows Community Toolkit afin qu’il puisse découvrir et charger des contrôles XAML UWP personnalisés. À cette fin, la méthode recommandée consiste à définir cet objet dans un projet d’application UWP distinct qui fait partie de la solution pour votre application WPF ou Windows Forms. 

    > [!NOTE]
    > Bien que l’objet `XamlApplication` ne soit pas nécessaire pour héberger un contrôle UWP interne, votre application a besoin de cet objet pour prendre en charge l’ensemble des scénarios d’îlot XAML, y compris l’hébergement de contrôles UWP personnalisés. Ainsi, nous vous recommandons de toujours définir un objet `XamlApplication` dans une solution dans laquelle vous utilisez des îlots XAML.

    > [!NOTE]
    > Votre solution ne peut contenir qu’un seul projet qui définit un objet `XamlApplication`. Tous les contrôles UWP personnalisés de votre application partagent le même objet `XamlApplication`. Le projet qui définit l’objet `XamlApplication` doit inclure des références à tous les autres projets et bibliothèques UWP utilisés pour héberger les contrôles UWP sur l’îlot XAML.

## <a name="create-a-wpf-project"></a>Créer un projet WPF

Avant de commencer, suivez ces instructions pour créer un projet WPF et le configurer pour héberger des îlots XAML. Si vous disposez déjà d’un projet WPF, vous pouvez adapter ces étapes et exemples de code à votre projet.

1. Dans Visual Studio 2019, créez une **Application WPF (.NET Framework)** ou un projet **Application WPF (.NET Core)** . Si vous souhaitez créer un projet **Application WPF (.NET Core)** , vous devez d’abord installer la dernière version du [SDK .NET Core 3](https://dotnet.microsoft.com/download/dotnet-core/3.0).

2. Assurez-vous que les [références de package](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) sont activées :

    1. Dans Visual Studio, cliquez sur **Outils -> Gestionnaire de package NuGet-> Paramètres du Gestionnaire de package**.
    2. Assurez-vous que **PackageReference** est sélectionné pour **Format de gestion de package par défaut**.

3. Cliquez avec le bouton droit sur votre projet WPF dans l’**Explorateur de solutions**, puis choisissez **Gérer les packages NuGet**.

4. Dans la fenêtre **Gestionnaire de package NuGet**, assurez-vous que l’option **Inclure la version préliminaire** est activée.

5. Sélectionnez l’onglet **Parcourir**, recherchez le package [Microsoft.Toolkit.Wpf.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) (version v6.0.0 ou ultérieure), puis installez le package. Ce package fournit tout ce dont vous avez besoin pour utiliser les contrôles UWP wrappés pour WPF (y compris [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) et [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) et le contrôle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)).
    > [!NOTE]
    > Les applications Windows Forms doivent utiliser le package [Microsoft.Toolkit.Forms.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls) (version v 6.0.0 ou ultérieure).

6. Configurez votre solution afin qu’elle cible une plateforme spécifique, telle que x86 ou x64. La plupart des scénarios d’îlots XAML ne sont pas pris en charge dans les projets qui ciblent **N’importe quelle UC**.

    1. Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de la solution, puis sélectionnez **Propriétés** -> **Propriétés de la configuration** -> **Gestionnaire de configurations**. 
    2. Sous **Plateforme de la solution active**, sélectionnez **Nouveau**. 
    3. Dans la boîte de dialogue **Nouvelle plateforme de solution**, sélectionnez **x64** ou **x86**, puis appuyez sur **OK**. 
    4. Fermez les boîtes de dialogue ouvertes.

## <a name="define-a-xamlapplication-class-in-a-uwp-app-project"></a>Définir une classe XamlApplication dans un projet d’application UWP

Ensuite, ajoutez un projet d’application UWP à votre solution et modifiez la classe `App` par défaut de ce projet afin de la dériver de la classe [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) fournie par le Windows Community Toolkit. Cette classe prend en charge l’interface [IXamlMetadaraProvider](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Markup.IXamlMetadataProvider), qui permet à votre application de découvrir et de charger des métadonnées pour les contrôles XAML UWP personnalisés dans les assemblys du répertoire actif de votre application au moment de l’exécution. Cette classe initialise également le framework XAML UWP pour le thread actuel.

> [!NOTE]
> Bien que cette étape ne soit pas nécessaire pour héberger un contrôle UWP interne, votre application a besoin de l’objet `XamlApplication` pour prendre en charge l’ensemble des scénarios d’îlot XAML, y compris l’hébergement de contrôles UWP personnalisés. Ainsi, nous vous recommandons de toujours définir un objet `XamlApplication` dans une solution dans laquelle vous utilisez des îlots XAML.

1. Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de la solution, puis sélectionnez **Ajouter** -> **Nouveau projet**.
2. Ajoutez un projet **Application vide (Windows universelle)** à votre solution. Assurez-vous que la version cible et la version minimale sont toutes les deux définies sur **Windows 10, version 1903** ou ultérieure.
3. Dans le projet d’application UWP, installez le package NuGet [Microsoft.Toolkit.Win32.UI.XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) (version v6.0.0 ou ultérieure).
4. Ouvrez le fichier **App.xaml** et remplacez le contenu de ce fichier par le code XAML suivant. Remplacez `MyUWPApp` par l’espace de noms de votre projet d’application UWP.

    ```xml
    <xaml:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:xaml="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns:local="using:MyUWPApp">
    </xaml:XamlApplication>
    ```

5. Ouvrez le fichier **App.xaml.cs** et remplacez le contenu de ce fichier par le code suivant. Remplacez `MyUWPApp` par l’espace de noms de votre projet d’application UWP.

    ```csharp
    namespace MyUWPApp
    {
        public sealed partial class App : Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication
        {
            public App()
            {
                this.Initialize();
            }
        }
    }
    ```

6. Supprimez le fichier **MainPage.xaml** du projet d’application UWP.
7. Générez le projet d’application UWP.
8. Dans votre projet WPF, cliquez avec le bouton droit sur le nœud **Dépendances** et ajoutez une référence à votre projet d’application UWP.

## <a name="instantiate-the-xamlapplication-object-in-the-entry-point-of-your-wpf-app"></a>Instancier l’objet XamlApplication dans le point d’entrée de votre application WPF

Ensuite, ajoutez du code au point d’entrée de votre application WPF pour créer une instance de la classe `App` que vous venez de définir dans le projet UWP (il s’agit de la classe qui dérive maintenant de `XamlApplication`).

1. Dans votre projet WPF, cliquez avec le bouton droit sur le nœud du projet, sélectionnez **Ajouter** -> **Nouvel élément**, puis sélectionnez **Classe**. Nommez la classe **Program**, puis cliquez sur **Ajouter**.

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

3. Cliquez avec le bouton droit sur le nœud du projet, puis choisissez **Propriétés**.

4. Sous l’onglet **Application** des propriétés, cliquez sur la liste déroulante **Objet de démarrage** et choisissez le nom qualifié complet de la classe `Program` que vous avez ajoutée à l’étape précédente. 
    > [!NOTE]
    > Par défaut, les projets WPF définissent une fonction de point d’entrée `Main` dans un fichier de code généré qui n’est pas destiné à être modifié. Cette étape remplace le point d’entrée de votre projet par la méthode `Main` de la nouvelle classe `Program`, ce qui vous permet d’ajouter du code qui s’exécute aussi tôt que possible dans le processus de démarrage de l’application. 

5. Enregistrez les modifications apportées aux propriétés du projet.

## <a name="host-an-inkcanvas-and-inktoolbar-by-using-wrapped-controls"></a>Héberger un InkCanvas et un InkToolbar à l’aide de contrôles wrappés

Votre projet étant configuré pour utiliser des îlots XAML UWP, vous pouvez ajouter à l’application les contrôles UWP wrappés [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) et [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar).

1. Dans l’**Explorateur de solutions**, ouvrez le fichier **MainWindow.xaml**.

2. Dans l’élément **Window** vers le début du fichier XAML, ajoutez l’attribut suivant. Ce dernier référence l’espace de noms XAML pour les contrôles UWP wrappés [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) et [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar).

    ```xml
    xmlns:Controls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    Une fois cet attribut ajouté, l’élément **Window** doit ressembler à ce qui suit.

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

3. Dans le fichier **MainWindow.xaml**, remplacez l’élément `<Grid>` existant par le code XAML suivant. Ce code XAML ajoute à l’élément `<Grid>` un contrôle [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) et un contrôle [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) (préfixés par le mot clé **Controls** que vous avez défini en tant qu’espace de noms).

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
    > Vous pouvez également ajouter ces contrôles wrappés à la fenêtre en les faisant glisser depuis la section **Windows Community Toolkit** de la **Boîte à outils** vers le concepteur.

4. Enregistrez le fichier **MainWindow.xaml**.

    Si vous avez un appareil qui prend en charge un stylet numérique, tel qu’une Surface, et que vous suivez ce labo sur un ordinateur physique, vous pouvez maintenant générer et exécuter l’application et dessiner avec de l’encre numérique sur l’écran au moyen du stylet. Toutefois, si vous n’avez pas d’appareil avec stylet et que vous essayez de signer avec la souris, rien ne se produit. En effet, le contrôle **InkCanvas** est activé uniquement pour les stylets numériques par défaut. Cependant, vous pouvez changer ce comportement.

5. Ouvrez le fichier **MainWindow.xaml.cs**.

6. Ajoutez la déclaration d’espace de noms suivante au début du fichier :

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

7. Recherchez le constructeur `MainWindow()`. Ajoutez la ligne de code suivante après la méthode `InitializeComponent()` et enregistrez le fichier de code.

    ```csharp
    this.myInkCanvas.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    Vous pouvez utiliser l’objet **InkPresenter** pour personnaliser l’expérience d’entrée manuscrite par défaut. Ce code utilise la propriété **InputDeviceTypes** pour permettre l’entrée avec la souris comme avec le stylet.

8. Appuyez sur F5 pour regénérer et exécuter l’application dans le débogueur. Si vous utilisez un ordinateur avec une souris, vérifiez que vous pouvez utiliser cette dernière pour dessiner un élément dans l’espace du canevas d’encre.

## <a name="host-a-calendarview-by-using-the-host-control"></a>Héberger un CalendarView à l’aide du contrôle hôte

Les contrôles UWP wrappés [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) et [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) étant ajoutés à l’application, vous pouvez utiliser le contrôle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) pour ajouter un [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) à l’application.

> [!NOTE]
> Le contrôle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) est fourni par le package [Microsoft.Toolkit.Wpf.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost). Ce package est inclus dans le package [Microsoft.Toolkit.Wpf.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) que vous avez installé.

1. Dans l’**Explorateur de solutions**, ouvrez le fichier **MainWindow.xaml**.

2. Dans l’élément **Window** vers le début du fichier XAML, ajoutez l’attribut suivant. Ce dernier référence l’espace de noms XAML pour le contrôle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost).

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    Une fois cet attribut ajouté, l’élément **Window** doit ressembler à ce qui suit.

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

4. Dans le fichier **MainWindow.xaml**, remplacez l’élément `<Grid>` existant par le code XAML suivant. Ce code XAML ajoute une ligne à la grille et ajoute un objet [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) à la dernière ligne. Pour héberger un contrôle [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) UWP, ce code XAML définit la propriété `InitialTypeName` sur le nom qualifié complet du contrôle. Ce code XAML définit également un gestionnaire d’événements pour l’événement `ChildChanged`, qui est déclenché une fois le contrôle hébergé affiché.

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

5. Enregistrez le fichier **MainWindow.xaml** et ouvrez le fichier **MainWindow.xaml.cs**.

7. Ajoutez la déclaration d’espace de noms suivante au début du fichier :

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

10. Ajoutez la méthode de gestionnaire d’événements `ChildChanged` suivante à la classe `MainWindow`, puis enregistrez le fichier de code. Une fois le contrôle hôte affiché, ce gestionnaire d’événements s’exécute et crée un gestionnaire d’événements simple pour l’événement `SelectedDatesChanged` du contrôle calendrier.

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

11. Appuyez sur F5 pour regénérer et exécuter l’application dans le débogueur. Vérifiez que le contrôle calendrier s’affiche maintenant en bas de la fenêtre.

## <a name="package-the-app"></a>Empaqueter l’application

Vous pouvez empaqueter l’application WPF dans un [package MSIX](https://docs.microsoft.com/windows/msix) pour la déployer. MSIX est une technologie d’empaquetage moderne pour Windows, basée sur une combinaison des technologies d’installation MSI, .appx, App-V et ClickOnce.

Les instructions suivantes montrent comment empaqueter tous les composants de la solution dans un package MSIX en utilisant le [Projet de création de package d’application Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) dans Visual Studio 2019. Ces étapes sont nécessaires uniquement si vous souhaitez empaqueter l’application WPF dans un package MSIX.

> [!NOTE]
> Si vous choisissez de ne pas empaqueter votre application dans un [package MSIX](https://docs.microsoft.com/windows/msix) pour la déployer, [Visual C++ Runtime](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) doit être installé sur les ordinateurs qui exécutent votre application.

1. Ajoutez un nouveau [projet d’empaquetage d’application Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) à votre solution. À mesure que vous créez le projet, sélectionnez **Windows 10, version 1903 (10.0; Build 18362)** pour la **Version cible** et la **Version minimale**.

2. Dans le projet d’empaquetage, cliquez avec le bouton droit sur le nœud **Applications**, puis choisissez **Ajouter une référence**. Dans la liste des projets, sélectionnez le projet WPF dans votre solution, puis cliquez sur **OK**.

3. Configurez votre solution afin qu’elle cible une plateforme spécifique, telle que x86 ou x64. Cette opération est nécessaire pour générer l’application WPF dans un package MSIX à l’aide du projet de création de packages d’applications Windows.

    1. Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de la solution, puis sélectionnez **Propriétés** -> **Propriétés de la configuration** -> **Gestionnaire de configurations**.
    2. Sous **Plateforme de la solution active**, sélectionnez **x64** ou **x86**.
    3. Dans la ligne de votre projet WPF, dans la colonne **Plateforme**, sélectionnez **Nouveau**.
    4. Dans la boîte de dialogue **Nouvelle plateforme de solution**, sélectionnez **x64** ou **x86** (la même plateforme que celle que vous avez sélectionnée pour **Plateforme de la solution active**), puis cliquez sur **OK**.
    5. Fermez les boîtes de dialogue ouvertes.

5. Générez et exécutez le projet d’empaquetage. Vérifiez que l’application WPF s’exécute et que le contrôle personnalisé UWP s’affiche comme prévu.

## <a name="related-topics"></a>Rubriques connexes

* [Héberger des contrôles XAML UWP dans des applications de bureau (îlots XAML)](xaml-islands.md)
* [Exemples de code d’îlots XAML](https://github.com/microsoft/Xaml-Islands-Samples)
* [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)
* [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar)
* [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)
