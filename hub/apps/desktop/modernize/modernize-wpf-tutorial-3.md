---
description: Ce tutoriel montre comment ajouter des interfaces utilisateur XAML UWP, créer des packages MSIX et incorporer d’autres composants modernes dans une application WPF.
title: Ajouter un contrôle CalendarView UWP à l’aide d'îles XAML
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, xaml islands, îles xaml
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 830c1cdf2e24e716d51642bc65b5b6783d0d784a
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643372"
---
# <a name="part-3-add-a-uwp-calendarview-control-using-xaml-islands"></a>Partie 3 : Ajouter un contrôle CalendarView UWP à l’aide d'îles XAML

Il s’agit de la troisième partie d’un tutoriel qui montre comment moderniser un exemple d’application de bureau WPF nommé Contoso Expenses. Vous trouverez une vue d’ensemble du tutoriel, les prérequis et les instructions de téléchargement de l’exemple d’application dans [Tutoriel : Modernisation d’une application WPF](modernize-wpf-tutorial.md). Cet article part du principe que vous avez suivi la [partie 2](modernize-wpf-tutorial-2.md).

Dans le scénario fictif de ce tutoriel, l’équipe de développement de Contoso souhaite faciliter la sélection de la date d’une note de frais sur un appareil tactile. Dans cette partie du tutoriel, vous allez ajouter un contrôle UWP [CalendarView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/calendar-view) à l’application. Il s’agit du contrôle utilisé dans la fonctionnalité de date et heure de la barre des tâches de Windows 10.

![Image CalendarViewControl](images/wpf-modernize-tutorial/CalendarViewControl.png)

Contrairement au contrôle **InkCanvas** ajouté dans la [partie 2](modernize-wpf-tutorial-2.md), le kit de ressources de la Communauté Windows ne fournit pas de version encapsulée de **CalendarView** UWP qui peut être utilisé dans les applications WPF. Vous allez plutôt héberger un **InkCanvas** dans le contrôle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) générique. Vous pouvez utiliser ce contrôle pour héberger n’importe quel contrôle UWP interne fourni par le kit SDK Windows ou la bibliothèque WinUI ou n’importe quel contrôle UWP personnalisé créé par un tiers. Le contrôle **WindowsXamlHost** est fourni par le package NuGet `Microsoft.Toolkit.Wpf.UI.XamlHost`. Ce package est inclus dans le package NuGet `Microsoft.Toolkit.Wpf.UI.Controls` que vous avez installé dans la [partie 2](modernize-wpf-tutorial-2.md).

> [!NOTE]
> Ce tutoriel montre uniquement comment utiliser [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) pour héberger le contrôle **CalendarView** interne fourni par le kit SDK Windows. Pour une procédure pas à pas indiquant comment héberger un contrôle personnalisé, consultez [Hébergement d’un contrôle UWP personnalisé dans une application WPF avec XAML Islands](host-custom-control-with-xaml-islands.md).

Pour pouvoir utiliser le contrôle **WindowsXamlHost**, vous devez appeler directement les API WinRT dans le code de l’application WPF. Le package NuGet `Microsoft.Windows.SDK.Contracts` contient les références nécessaires pour cela. Il est également inclus dans le package NuGet `Microsoft.Toolkit.Wpf.UI.Controls` que vous avez installé dans la [partie 2](modernize-wpf-tutorial-2.md).

## <a name="add-the-windowsxamlhost-control"></a>Ajout du contrôle WindowsXamlHost

1. Dans **Explorateur de solutions**, développez le dossier **Affichages** du projet **ContosoExpenses.Core**, puis double-cliquez sur le fichier **AddNewExpense.xaml**. Il s’agit du formulaire permettant d’ajouter une nouvelle dépense à la liste. Il apparaît ainsi dans la version actuelle de l’application.

    ![Ajout d’une nouvelle dépense](images/wpf-modernize-tutorial/AddNewExpense.png)

    Le contrôle sélecteur de dates inclus dans WPF est destiné aux ordinateurs traditionnels avec souris et clavier. Il n’est pas vraiment possible de choisir une date avec un écran tactile, en raison de la petite taille du contrôle et de l’espace limité entre les jours dans le calendrier.

2. En haut du fichier **AddNewExpense.xaml**, ajoutez l’attribut suivant à l’élément **Window**.

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    Une fois cet attribut ajouté, l’élément **Window** devrait se présenter ainsi.

    ```xml
    <Window x:Class="ContosoExpenses.Views.AddNewExpense"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
            DataContext="{Binding Source={StaticResource ViewModelLocator},Path=AddNewExpenseViewModel}"
            xmlns:local="clr-namespace:ContosoExpenses"
            mc:Ignorable="d"
            Title="Add new expense" Height="450" Width="800"
            Background="{StaticResource AddNewExpenseBackground}">
    ```

3. Modifiez l’attribut **Height** de l’élément **Window** en remplaçant 450 par 800. Cette opération est nécessaire, car le contrôle UWP **CalendarView** prend plus d’espace que le sélecteur de dates WPF.

    ```xml
    <Window x:Class="ContosoExpenses.Views.AddNewExpense"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
            DataContext="{Binding Source={StaticResource ViewModelLocator},Path=AddNewExpenseViewModel}"
            xmlns:local="clr-namespace:ContosoExpenses"
            mc:Ignorable="d"
            Title="Add new expense" Height="800" Width="800"
            Background="{StaticResource AddNewExpenseBackground}">
    ```

4. Recherchez l’élément `DatePicker` en bas du fichier et remplacez-le par le code XAML suivant.

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  />
    ```

    Ce code XAML ajoute le contrôle **WindowsXamlHost**. La propriété **InitialTypeName** indique le nom complet du contrôle UWP à héberger (dans ce cas, **Windows.UI.Xaml.Controls.CalendarView**).

5. Appuyez sur la touche F5 pour générer et exécuter l’application dans le débogueur. Choisissez un employé dans la liste, puis appuyez sur le bouton **Ajouter une nouvelle dépense**. Vérifiez que la page suivante héberge le nouveau contrôle UWP **CalendarView**.

    ![Wrapper CalendarView](images/wpf-modernize-tutorial/CalendarViewWrapper.png)

6. Fermez l’application.

## <a name="interact-with-the-windowsxamlhost-control"></a>Interaction avec le contrôle WindowsXamlHost

Vous allez maintenant mettre à jour l’application pour traiter la date sélectionnée, l’afficher à l’écran et remplir l’objet **Expense** pour l’enregistrer dans la base de données.

Le [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) UWP contient deux membres pertinents dans ce scénario :

- La propriété **SelectedDates** contient la date sélectionnée par l’utilisateur.
- L’événement **SelectedDatesChanged** est déclenché lorsque l’utilisateur sélectionne une date.

Cependant, le contrôle **WindowsXamlHost** est un contrôle hôte générique pour *n’importe quel* type de contrôle UWP. Par conséquent, il n’expose pas de propriété nommée **SelectedDates** ou d’événement nommé **SelectedDatesChanged**, car ils sont propres au contrôle **CalendarView**. Pour accéder à ces membres, vous devez écrire du code qui caste **WindowsXamlHost** au type **CalendarView**. La meilleure solution consiste à le faire en réponse à l’événement **ChildChanged** du contrôle **WindowsXamlHost**, qui est déclenché une fois le rendu du contrôle hébergé effectué.

1. Dans le fichier **AddNewExpense.xaml**, ajoutez un gestionnaire d’événements pour l’événement **ChildChanged** du contrôle **WindowsXamlHost** ajouté précédemment. L’élément **WindowsXamlHost** devrait alors se présenter ainsi.

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  ChildChanged="CalendarUwp_ChildChanged" />
    ```

2. Dans le même fichier, recherchez l’élément **Grid.RowDefinitions** du **Grid** principal. Ajoutez un autre élément **RowDefinition** avec **Height** égal à **Auto** à la fin de la liste d’éléments enfants. L’élément **Grid.RowDefinitions** devrait alors se présenter ainsi (avec neuf éléments **RowDefinition**).

    ```xml
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    ```

4. Ajoutez le code XAML suivant après l’élément **WindowsXamlHost** et avant l’élément **Button** à la fin du fichier.

    ```xml
    <TextBlock Text="Selected date:" FontSize="16" FontWeight="Bold" Grid.Row="7" Grid.Column="0" />
    <TextBlock Text="{Binding Path=Date}" FontSize="16" Grid.Row="7" Grid.Column="1" />
    ```

5. Recherchez l’élément **Button** à la fin du fichier, puis modifiez la propriété **Grid.Row** en remplaçant **7** par **8**. Cette modification a pour effet de décaler le bouton d’une ligne vers le bas dans la grille, car une nouvelle ligne a été ajoutée.

    ```xml
    <Button Content="Save" Grid.Row="8" Grid.Column="0" Command="{Binding Path=SaveExpenseCommand}" Margin="5, 12, 0, 0" HorizontalAlignment="Left" Width="180" />
    ```

6. Ouvrez le fichier de code **AddNewExpense.xaml.cs**.

7. Ajoutez l'instruction suivante en haut du fichier.

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

8. Ajoutez le gestionnaire d'événements suivant à la classe `AddNewExpense`. Ce code implémente l’événement **ChildChanged** du contrôle **WindowsXamlHost** déclaré précédemment dans le fichier XAML.

    ```csharp
    private void CalendarUwp_ChildChanged(object sender, System.EventArgs e)
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
                    Messenger.Default.Send<SelectedDateMessage>(new SelectedDateMessage(args.AddedDates[0].DateTime));
                }
            };
        }
    }
    ```

    Ce code utilise la propriété **Child** du contrôle **WindowsXamlHost** pour accéder au contrôle UWP **CalendarView**. Il s’abonne ensuite à l’événement **SelectedDatesChanged**, qui est déclenché lorsque l’utilisateur sélectionne une date dans le calendrier. Ce gestionnaire d’événements transmet la date sélectionnée au ViewModel dans lequel le nouvel objet **Expense** est créé et enregistré dans la base de données. Pour cela, le code utilise l’infrastructure de message fournie par le package NuGet MVVM Light. Il envoie un message nommé **SelectedDateMessage** au ViewModel, qui le reçoit et définit la propriété **Date** avec la valeur sélectionnée. Dans ce scénario, le contrôle **CalendarView** est configuré pour le mode de sélection unique, de sorte que la collection ne contiendra qu’un seul élément.

    À ce stade, le projet ne se compile toujours pas, car il manque l’implémentation de la classe **SelectedDateMessage**. Elle sera effectuée au cours des étapes suivantes.

9. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier **Messages** et choisissez **Ajouter -> Classe**. Nommez la nouvelle classe **SelectedDateMessage**, puis cliquez sur **Ajouter**.

10. Remplacez le contenu du fichier de code **SelectedDateMessage.cs** par le code suivant. Ce code ajoute une instruction using pour l’espace de noms `GalaSoft.MvvmLight.Messaging` (à partir du package NuGet MVVM Light), hérite de la classe **MessageBase** et ajoute la propriété **DateTime** qui est initialisée par le constructeur public. Le fichier devrait alors se présenter ainsi.

    ```csharp
    using GalaSoft.MvvmLight.Messaging;
    using System;
    
    namespace ContosoExpenses.Messages
    {
        public class SelectedDateMessage: MessageBase
        {
            public DateTime SelectedDate { get; set; }
    
            public SelectedDateMessage(DateTime selectedDate)
            {
                this.SelectedDate = selectedDate;
            }
        }
    }
    ```

11. Vous allez maintenant mettre à jour le ViewModel de façon à recevoir ce message et remplir la propriété **Date** du ViewModel. Dans **Explorateur de solutions**, développez le dossier **ViewModels** et ouvrez le fichier **AddNewExpenseViewModel.cs**.

12. Recherchez le constructeur public de la classe `AddNewExpenseViewModel` et ajoutez le code suivant à la fin du constructeur. Ce code s’inscrit pour recevoir **SelectedDateMessage** et en extraire la date sélectionnée par le biais de la propriété **SelectedDate** ; nous l’utilisons ensuite pour définir la propriété **Date** exposée par le ViewModel. Cette propriété étant liée au contrôle **TextBlock** ajouté précédemment, la date apparaît sélectionnée par l’utilisateur.

    ```csharp
    Messenger.Default.Register<SelectedDateMessage>(this, message =>
    {
        Date = message.SelectedDate;
    });
    ```

    Le constructeur `AddNewExpenseViewModel` devrait alors se présenter ainsi. 

    ```csharp
    public AddNewExpenseViewModel(IDatabaseService databaseService, IStorageService storageService)
    {
        this.databaseService = databaseService;
        this.storageService = storageService;

        Date = DateTime.Today;

        Messenger.Default.Register<SelectedDateMessage>(this, message =>
        {
            Date = message.SelectedDate;
        });
    }
    ```

    > [!NOTE]
    > La méthode `SaveExpenseCommand` du même fichier de code se charge d’enregistrer les dépenses dans la base de données. Elle utilise la propriété **Date** pour créer le nouvel objet **Expense**. Cette propriété est maintenant remplie par le contrôle **CalendarView** à travers le message `SelectedDateMessage` que vous venez de créer.

13. Appuyez sur la touche F5 pour générer et exécuter l’application dans le débogueur. Choisissez l’un des employés disponibles, puis cliquez sur le bouton **Ajouter une nouvelle dépense**. Complétez tous les champs du formulaire et choisissez une date dans le nouveau contrôle **CalendarView**. Vérifiez que la date sélectionnée s’affiche sous forme de chaîne sous le contrôle.

14. Appuyez sur le bouton **Enregistrer**. Le formulaire se ferme et la nouvelle dépense est ajoutée à la fin de la liste des dépenses. La première colonne doit comporter la date de la dépense que vous avez sélectionnée dans le contrôle **CalendarView**.

## <a name="next-steps"></a>Étapes suivantes

À ce stade du tutoriel, vous avez remplacé un contrôle de date et d’heure WPF par le contrôle UWP **CalendarView**, qui prend en charge les écrans tactiles et les stylos numériques en plus de l’entrée au clavier et à la souris. Bien que le kit de ressources de la Communauté Windows ne fournisse pas de version encapsulée du contrôle UWP **CalendarView** qui peut être utilisé directement dans une application WPF, vous avez pu héberger le contrôle à l’aide du contrôle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) générique.

Vous pouvez maintenant passer à la [Partie 4 : Ajout d’activités et de notifications utilisateur Windows 10](modernize-wpf-tutorial-4.md).
