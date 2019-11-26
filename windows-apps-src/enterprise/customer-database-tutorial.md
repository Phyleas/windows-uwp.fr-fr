---
title: Créer une application de base de données de clients
description: Créez une application de base de données client et apprenez à implémenter les fonctions de base des applications d’entreprise.
keywords: entreprise, didacticiel, client, données, créer une mise à jour de lecture de suppression, REST, authentification
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: med
ms.openlocfilehash: b8cf0554464b56337e3d57b58db543092682ffa3
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258615"
---
# <a name="tutorial-create-a-customer-database-application"></a>Didacticiel : Créer une application de base de données de clients

Ce didacticiel crée une application simple pour la gestion d’une liste de clients. Dans ce cas, il introduit une sélection de concepts de base pour les applications d’entreprise dans UWP. Vous allez apprendre à effectuer les opérations suivantes :

* Implémentez des opérations de création, lecture, mise à jour et suppression sur une base de données SQL locale.
* Ajoutez une grille de données pour afficher et modifier les données client dans votre interface utilisateur.
* Réorganiser les éléments d’interface utilisateur dans une disposition de formulaire de base.

Le point de départ de ce didacticiel est une application à page unique avec une interface utilisateur et des fonctionnalités minimales, sur la base d’une version simplifiée de l' [exemple d’application Customer Orders Database](https://github.com/Microsoft/Windows-appsample-customers-orders-database). Il est écrit en C# XAML et nous supposons que vous disposez d’une connaissance de base de ces deux langages.

![Page principale de l’application active](images/customer-database-tutorial/customer-list.png)

### <a name="prerequisites"></a>Conditions préalables requises

* [Vérifiez que vous disposez de la dernière version de Visual Studio et du kit de développement logiciel (SDK) Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* [Cloner ou télécharger l’exemple de didacticiel de base de données client](https://github.com/microsoft/windows-tutorials-customer-database)

Une fois que vous avez cloné/téléchargé le référentiel, vous pouvez modifier le projet en ouvrant **CustomerDatabaseTutorial. sln** avec Visual Studio.

> [!NOTE]
> Consultez l' [exemple de base de données Custom Orders complet](https://github.com/Microsoft/Windows-appsample-customers-orders-database) pour voir l’application sur laquelle ce didacticiel était basé.

## <a name="part-1-code-of-interest"></a>Partie 1 : code d’intérêt

Si vous exécutez votre application immédiatement après l’avoir ouverte, vous verrez quelques boutons en haut d’un écran vide. Bien qu’il ne soit pas visible, l’application contient déjà une base de données SQLite locale approvisionnée avec quelques clients de test. À partir de là, vous allez commencer par implémenter un contrôle d’interface utilisateur pour afficher ces clients, puis passer à l’ajout d’opérations dans la base de données. Avant de commencer, c’est là que vous allez travailler.

### <a name="views"></a>Affichages

**CustomerListPage. Xaml** est la vue de l’application, qui définit l’interface utilisateur de la page unique dans ce didacticiel. Chaque fois que vous devez ajouter ou modifier un élément visuel dans l’interface utilisateur, vous le ferez dans ce fichier. Ce didacticiel vous aidera à ajouter ces éléments :

* Un **RadDataGrid** pour l’affichage et la modification de vos clients. 
* **StackPanel** pour définir les valeurs initiales d’un nouveau client.

### <a name="viewmodels"></a>ViewModels

**ViewModels\CustomerListPageViewModel.cs** est l’endroit où se trouve la logique fondamentale de l’application. Chaque action de l’utilisateur effectuée dans la vue est transmise dans ce fichier à des fins de traitement. Dans ce didacticiel, vous allez ajouter un nouveau code et implémenter les méthodes suivantes :

* **CreateNewCustomerAsync**, qui initialise un nouvel objet CustomerViewModel.
* **DeleteNewCustomerAsync**, qui supprime un nouveau client avant qu’il ne s’affiche dans l’interface utilisateur.
* **DeleteAndUpdateAsync**, qui gère la logique du bouton supprimer.
* **GetCustomerListAsync**, qui récupère une liste de clients à partir de la base de données.
* **SaveInitialChangesAsync**, qui ajoute les informations d’un nouveau client à la base de données.
* **UpdateCustomersAsync**, qui actualise l’interface utilisateur pour refléter les clients ajoutés ou supprimés.

**CustomerViewModel** est un wrapper pour les informations d’un client qui indique s’il a été récemment modifié. Vous n’avez rien à ajouter à cette classe, mais une partie du code que vous ajouterez à un autre emplacement le référencera.

Pour plus d’informations sur la façon dont l’exemple est construit, consultez la [vue d’ensemble de la structure d’application](../enterprise/customer-database-app-structure.md).

## <a name="part-2-add-the-datagrid"></a>Partie 2 : ajouter le contrôle DataGrid

Avant de commencer à utiliser les données client, vous devez ajouter un contrôle d’interface utilisateur pour afficher ces clients. Pour ce faire, nous allons utiliser un contrôle **RadDataGrid** tiers prédéfini. Le package NuGet **Telerik. UI. for. UniversalWindowsPlatform** a déjà été inclus dans ce projet. Nous allons ajouter la grille à notre projet.

1. Ouvrez **Views\CustomerListPage.Xaml** à partir de la Explorateur de solutions. Ajoutez la ligne de code suivante dans la balise **page** pour déclarer un mappage à l’espace de noms Telerik contenant la grille de données.

    ```xaml
        xmlns:telerikGrid="using:Telerik.UI.Xaml.Controls.Grid"
    ```

2. Sous la **barre de commandes** dans le **RelativePanel** principal de la vue, ajoutez un contrôle **RadDataGrid** , avec certaines options de configuration de base :

    ```xaml
    <Grid
        x:Name="CustomerListRoot"
        Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <RelativePanel>
            <CommandBar
                x:Name="mainCommandBar"
                HorizontalAlignment="Stretch"
                Background="AliceBlue">
                <!--CommandBar content-->
            </CommandBar>
            <telerikGrid:RadDataGrid
                x:Name="DataGrid"
                BorderThickness="0"
                ColumnDataOperationsMode="Flyout"
                GridLinesVisibility="None"
                GroupPanelPosition="Left"
                RelativePanel.AlignLeftWithPanel="True"
                RelativePanel.AlignRightWithPanel="True"
                RelativePanel.Below="mainCommandBar" />
        </RelativePanel>
    </Grid>
    ```

3. Vous avez ajouté la grille de données, mais vous avez besoin de données à afficher. Ajoutez-y les lignes de code suivantes :

    ```xaml
    ItemsSource="{x:Bind ViewModel.Customers}"
    UserEditMode="Inline"
    ```
    Maintenant que vous avez défini une source de données à afficher, **RadDataGrid** gère la majeure partie de la logique d’interface utilisateur pour vous. Toutefois, si vous exécutez votre projet, vous ne verrez toujours aucune donnée à l’écran. Cela est dû au fait que le ViewModel ne le charge pas encore.

![Application vide, sans client](images/customer-database-tutorial/blank-customer-list.png)

## <a name="part-3-read-customers"></a>Partie 3 : lire les clients

Lorsqu’il est initialisé, **ViewModels\CustomerListPageViewModel.cs** appelle la méthode **GetCustomerListAsync** . Cette méthode doit récupérer les données client de test à partir de la base de données SQLite qui est incluse dans le didacticiel.

1. Dans **ViewModels\CustomerListPageViewModel.cs**, mettez à jour votre méthode **GetCustomerListAsync** avec ce code :

    ```csharp
    public async Task GetCustomerListAsync()
    {
        var customers = await App.Repository.Customers.GetAsync(); 
        if (customers == null)
        {
            return;
        }
        await DispatcherHelper.ExecuteOnUIThreadAsync(() =>
        {
            Customers.Clear();
            foreach (var c in customers)
            {
                Customers.Add(new CustomerViewModel(c));
            }
        });
    }
    ```
    La méthode **GetCustomerListAsync** est appelée lors du chargement du ViewModel, mais avant cette étape, elle n’a rien fait. Ici, nous avons ajouté un appel à la méthode **GetAsync** dans le **référentiel/SqlCustomerRepository**. Cela lui permet de contacter le référentiel pour récupérer une collection énumérable d’objets Customer. Il les analyse ensuite dans des objets individuels, avant de les ajouter à son **ObservableCollection** interne, afin qu’ils puissent être affichés et modifiés.

2. Exécutez votre application. vous verrez maintenant la grille de données affichant la liste des clients.

![Liste initiale des clients](images/customer-database-tutorial/initial-customers.png)

## <a name="part-4-edit-customers"></a>Partie 4 : modifier les clients

Vous pouvez modifier les entrées de la grille de données en double-cliquant dessus, mais vous devez vous assurer que toutes les modifications que vous apportez dans l’interface utilisateur sont également apportées à votre collection de clients dans le code-behind. Cela signifie que vous devez implémenter la liaison de données bidirectionnelle. Si vous souhaitez plus d’informations à ce sujet, consultez notre [Introduction à la liaison de données](../get-started/display-customers-in-list-learning-track.md).

1. Tout d’abord, déclarez que **ViewModels\CustomerListPageViewModel.cs** implémente l’interface **INotifyPropertyChanged** :

    ```csharp
    public class CustomerListPageViewModel : INotifyPropertyChanged
    ```

2. Ensuite, dans le corps principal de la classe, ajoutez l’événement et la méthode suivants :

    ```csharp
    public event PropertyChangedEventHandler PropertyChanged;

    public void OnPropertyChanged([CallerMemberName] string propertyName = null) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    ```

    La méthode **OnPropertyChanged** permet à vos accesseurs set de déclencher facilement l’événement **propertyChanged** , ce qui est nécessaire pour la liaison de données bidirectionnelle.

3. Mettez à jour la méthode setter pour **SelectedCustomer** avec cet appel de fonction :

    ```csharp
    public CustomerViewModel SelectedCustomer
    {
        get => _selectedCustomer;
        set
        {
            if (_selectedCustomer != value)
            {
                _selectedCustomer = value;
                OnPropertyChanged();
            }
        }
    }
    ```

4. Dans **Views\CustomerListPage.Xaml**, ajoutez la propriété **SelectedCustomer** à votre grille de données.

    ```xaml
    SelectedItem="{x:Bind ViewModel.SelectedCustomer, Mode=TwoWay}"
    ```

    Cela associe la sélection de l’utilisateur dans la grille de données à l’objet client correspondant dans le code-behind. Le mode de liaison TwoWay permet de répercuter les modifications apportées à l’interface utilisateur sur cet objet.

5. Exécutez votre application. Vous pouvez maintenant voir les clients affichés dans la grille et apporter des modifications aux données sous-jacentes par le biais de votre interface utilisateur.

![Modification d’un client dans la grille de données](images/customer-database-tutorial/edit-customer-inline.png)

## <a name="part-5-update-customers"></a>Partie 5 : mettre à jour les clients

Maintenant que vous pouvez voir et modifier vos clients, vous devez être en mesure de transmettre vos modifications à la base de données et d’extraire les mises à jour apportées par d’autres utilisateurs.

1. Revenez à **ViewModels\CustomerListPageViewModel.cs**et accédez à la méthode **UpdateCustomersAsync** . Mettez-le à jour avec ce code pour transmettre les modifications à la base de données et récupérer les nouvelles informations :

    ```csharp
    public async Task UpdateCustomersAsync()
    {
        foreach (var modifiedCustomer in Customers
            .Where(x => x.IsModified).Select(x => x.Model))
        {
            await App.Repository.Customers.UpsertAsync(modifiedCustomer);
        }
        await GetCustomerListAsync();
    }
    ```
    Ce code utilise la propriété **IsModified** de **ViewModels\CustomerViewModel.cs**, qui est mise à jour automatiquement chaque fois que le client est modifié. Cela vous permet d’éviter les appels inutiles et de pousser uniquement les modifications des clients mis à jour vers la base de données.

## <a name="part-6-create-a-new-customer"></a>Partie 6 : créer un nouveau client

L’ajout d’un nouveau client présente un défi, car le client apparaîtra sous la forme d’une ligne vide si vous l’ajoutez à l’interface utilisateur avant de fournir des valeurs pour ses propriétés. Ce n’est pas un problème, mais nous allons ici faciliter la définition des valeurs initiales d’un client. Dans ce didacticiel, nous allons ajouter un panneau réductible simple, mais si vous avez plus d’informations à ajouter, vous pouvez créer une page distincte à cet effet.

### <a name="update-the-code-behind"></a>Mettre à jour le code-behind

1. Ajoutez un nouveau champ privé et une propriété publique à **ViewModels\CustomerListPageViewModel.cs**. Ce sera utilisé pour contrôler si le panneau est visible.

    ```csharp
    private bool _addingNewCustomer = false;

    public bool AddingNewCustomer
    {
        get => _addingNewCustomer;
        set
        {
            if (_addingNewCustomer != value)
            {
                _addingNewCustomer = value;
                OnPropertyChanged();
            }
        }
    }
    ```

2. Ajoutez une nouvelle propriété publique au ViewModel, à l’inverse de la valeur de **AddingNewCustomer**. Ce sera utilisé pour désactiver les boutons de barre de commandes normaux lorsque le panneau est visible.

    ```csharp
    public bool EnableCommandBar => !AddingNewCustomer;
    ```
    Vous avez maintenant besoin d’un moyen d’afficher le Panneau réductible et de créer un client à modifier dans celui-ci. 

3. Ajoutez un nouveau Fiend privé et une propriété publique au ViewModel, pour contenir le client qui vient d’être créé.

    ```csharp
    private CustomerViewModel _newCustomer;

    public CustomerViewModel NewCustomer
    {
        get => _newCustomer;
        set
        {
            if (_newCustomer != value)
            {
                _newCustomer = value;
                OnPropertyChanged();
            }
        }
    }
    ```

2.  Mettez à jour votre méthode **CreateNewCustomerAsync** pour créer un nouveau client, ajoutez-le au référentiel et définissez-le en tant que client sélectionné :

    ```csharp
    public async Task CreateNewCustomerAsync()
    {
        CustomerViewModel newCustomer = new CustomerViewModel(new Models.Customer());
        NewCustomer = newCustomer;
        await App.Repository.Customers.UpsertAsync(NewCustomer.Model);
        AddingNewCustomer = true;
    }
    ```

3. Mettez à jour la méthode **SaveInitialChangesAsync** pour ajouter un client nouvellement créé au référentiel, mettez à jour l’interface utilisateur, puis fermez le panneau.

    ```csharp
    public async Task SaveInitialChangesAsync()
    {
        await App.Repository.Customers.UpsertAsync(NewCustomer.Model);
        await UpdateCustomersAsync();
        AddingNewCustomer = false;
    }
    ```
4. Ajoutez la ligne de code suivante comme dernière ligne dans la méthode setter pour **AddingNewCustomer**:

    ```csharp
    OnPropertyChanged(nameof(EnableCommandBar));
    ```

    Cela permet de s’assurer que **EnableCommandBar** est automatiquement mis à jour chaque fois que **AddingNewCustomer** est modifié.

### <a name="update-the-ui"></a>Mettre à jour l’interface utilisateur

1. Revenez à **Views\CustomerListPage.Xaml**et ajoutez un **StackPanel** avec les propriétés suivantes entre votre **CommandBar** et votre grille de données :

    ```xaml
    <StackPanel
        x:Name="newCustomerPanel"
        Orientation="Horizontal"
        x:Load="{x:Bind ViewModel.AddingNewCustomer, Mode=OneWay}"
        RelativePanel.Below="mainCommandBar">
    </StackPanel>
    ```
    L’attribut **x :Load** garantit que ce panneau s’affiche uniquement lorsque vous ajoutez un nouveau client.

2. Apportez la modification suivante à la position de votre grille de données pour vous assurer qu’elle descend lorsque le nouveau panneau s’affiche :

    ```xaml
    RelativePanel.Below="newCustomerPanel"
    ```

3. Mettez à jour votre panneau de pile avec quatre contrôles **TextBox** . Ils sont liés aux propriétés individuelles du nouveau client et vous permettent de modifier ses valeurs avant de l’ajouter à la grille de données.

    ```xaml
    <StackPanel
        x:Name="newCustomerPanel"
        Orientation="Horizontal"
        x:Load="{x:Bind ViewModel.AddingNewCustomer, Mode=OneWay}"
        RelativePanel.Below="mainCommandBar">
        <TextBox
            Header="First name"
            PlaceholderText="First"
            Margin="8,8,16,8"
            MinWidth="120"
            Text="{x:Bind ViewModel.NewCustomer.FirstName, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
        <TextBox
            Header="Last name"
            PlaceholderText="Last"
            Margin="0,8,16,8"
            MinWidth="120"
            Text="{x:Bind ViewModel.NewCustomer.LastName, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
        <TextBox
            Header="Address"
            PlaceholderText="1234 Address St, Redmond WA 00000"
            Margin="0,8,16,8"
            MinWidth="280"
            Text="{x:Bind ViewModel.NewCustomer.Address, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
        <TextBox
            Header="Company"
            PlaceholderText="Company"
            Margin="0,8,16,8"
            MinWidth="120"
            Text="{x:Bind ViewModel.NewCustomer.Company, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
    </StackPanel>
    ```

4. Ajoutez un bouton simple au nouveau panneau de la pile pour enregistrer le client qui vient d’être créé :

    ```xaml
    <StackPanel>
        <!--Text boxes from step 3-->
        <AppBarButton
            x:Name="SaveNewCustomer"
            Click="{x:Bind ViewModel.SaveInitialChangesAsync}"
            Icon="Save"/>
    </StackPanel>
    ```

5. Mettez à jour la **barre de commandes**pour que les boutons standard créer, supprimer et mettre à jour soient désactivés lorsque le panneau pile est visible :

    ```xaml
    <CommandBar
        x:Name="mainCommandBar"
        HorizontalAlignment="Stretch"
        IsEnabled="{x:Bind ViewModel.EnableCommandBar, Mode=OneWay}"
        Background="AliceBlue">
        <!--App bar buttons-->
    </CommandBar>
    ```

6. Exécutez votre application. Vous pouvez maintenant créer un client et entrer ses données dans le panneau pile.

![Création d’un nouveau client](images/customer-database-tutorial/add-new-customer.png)

## <a name="part-7-delete-a-customer"></a>Partie 7 : supprimer un client

La suppression d’un client est la dernière opération de base que vous devez implémenter. Lorsque vous supprimez un client que vous avez sélectionné dans la grille de données, vous devez immédiatement appeler **UpdateCustomersAsync** afin de mettre à jour l’interface utilisateur. Toutefois, vous n’avez pas besoin d’appeler cette méthode si vous supprimez un client que vous venez de créer.

1. Accédez à **ViewModels\CustomerListPageViewModel.cs**et mettez à jour la méthode **DeleteAndUpdateAsync** :

    ```csharp
    public async void DeleteAndUpdateAsync()
    {
        if (SelectedCustomer != null)
        {
            await App.Repository.Customers.DeleteAsync(_selectedCustomer.Model.Id);
        }
        await UpdateCustomersAsync();
    }
    ```

2. Dans **Views\CustomerListPage.Xaml**, mettez à jour le panneau de pile pour ajouter un nouveau client pour qu’il contienne un deuxième bouton :

    ```xaml
    <StackPanel>
        <!--Text boxes for adding a new customer-->
        <AppBarButton
            x:Name="DeleteNewCustomer"
            Click="{x:Bind ViewModel.DeleteNewCustomerAsync}"
            Icon="Cancel"/>
        <AppBarButton
            x:Name="SaveNewCustomer"
            Click="{x:Bind ViewModel.SaveInitialChangesAsync}"
            Icon="Save"/>
    </StackPanel>
    ```

3. Dans **ViewModels\CustomerListPageViewModel.cs**, mettez à jour la méthode **DeleteNewCustomerAsync** pour supprimer le nouveau client :

    ```csharp
    public async Task DeleteNewCustomerAsync()
    {
        if (NewCustomer != null)
        {
            await App.Repository.Customers.DeleteAsync(_newCustomer.Model.Id);
            AddingNewCustomer = false;
        }
    }
    ```

4. Exécutez votre application. Vous pouvez maintenant supprimer des clients, soit au sein de la grille de données, soit dans le panneau pile.

![Supprimer un nouveau client](images/customer-database-tutorial/delete-new-customer.png)

## <a name="conclusion"></a>Conclusion

Félicitations ! Une fois cette opération effectuée, votre application dispose maintenant d’une gamme complète d’opérations de base de données locales. Vous pouvez créer, lire, mettre à jour et supprimer des clients dans votre interface utilisateur, et ces modifications sont enregistrées dans votre base de données et sont conservées dans différents lancements de votre application.

Maintenant que vous avez terminé, prenez en compte les éléments suivants :
* Si vous ne l’avez pas déjà fait, consultez la [vue d’ensemble](../enterprise/customer-database-app-structure.md) de la structure d’application pour plus d’informations sur la raison pour laquelle l’application est créée.
* Explorez l' [exemple de base de données Custom Orders complet](https://github.com/Microsoft/Windows-appsample-customers-orders-database) pour voir l’application sur laquelle est basé ce didacticiel.

Si vous êtes un défi, vous pouvez continuer...

## <a name="going-further-connect-to-a-remote-database"></a>Aller plus loin : se connecter à une base de données distante

Nous vous avons fourni une procédure pas à pas pour implémenter ces appels sur une base de données SQLite locale. Mais que se passe-t-il si vous souhaitez utiliser une base de données distante ?

Si vous souhaitez faire un essai, vous aurez besoin de votre propre compte Azure Active Directory (AAD) et de la possibilité d’héberger votre propre source de données.

Vous devez ajouter l’authentification, les fonctions pour gérer les appels REST, puis créer une base de données distante avec laquelle interagir. Il y a du code dans l' [exemple de base de données Custom Order Orders](https://github.com/Microsoft/Windows-appsample-customers-orders-database) que vous pouvez référencer pour chaque opération nécessaire.

### <a name="settings-and-configuration"></a>Paramètres et configuration

Les étapes nécessaires pour se connecter à votre base de données distante sont indiquées dans le [fichier Lisez-moi de l’exemple](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/README.md). Vous devez effectuer les opérations suivantes :

* Fournissez votre ID client de compte Azure à [constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs).
* Indiquez l’URL de la base de données distante à [constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs).
* Fournissez la chaîne de connexion de la base de données à [constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs).
* Associez votre application au Microsoft Store.
* Copiez le [projet de service](https://github.com/Microsoft/Windows-appsample-customers-orders-database/tree/master/ContosoService) dans votre application et déployez-le sur Azure.

### <a name="authentication"></a>Authentification

Vous devez créer un bouton pour démarrer une séquence d’authentification et une fenêtre contextuelle ou une page distincte pour recueillir les informations d’un utilisateur. Une fois que vous avez créé cela, vous devez fournir du code qui demande les informations d’un utilisateur et l’utilise pour acquérir un jeton d’accès. L’exemple de base de données Customers Orders encapsule Microsoft Graph appels avec la bibliothèque **Gestionnaire** pour acquérir un jeton et gérer l’authentification auprès d’un compte AAD.

* La logique d’authentification est implémentée dans [**AuthenticationViewModel.cs**](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/ViewModels/AuthenticationViewModel.cs).
* Le processus d’authentification est affiché dans le contrôle [**AuthenticationControl. Xaml**](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/UserControls/AuthenticationControl.xaml) personnalisé.

### <a name="rest-calls"></a>Appels REST

Vous n’avez pas besoin de modifier le code que nous avons ajouté dans ce didacticiel afin d’implémenter les appels REST. Au lieu de cela, vous devez effectuer les opérations suivantes :

* Créez de nouvelles implémentations des interfaces **ICustomerRepository** et **ITutorialRepository** , en implémentant le même jeu de fonctions par le biais de Rest au lieu de sqlite. Vous devez sérialiser et désérialiser JSON, et vous pouvez encapsuler vos appels REST dans une classe **HttpHelper** distincte, si nécessaire. Reportez-vous à [l’exemple complet](https://github.com/Microsoft/Windows-appsample-customers-orders-database/tree/master/ContosoRepository/Rest) pour plus de détails.
* Dans **app.Xaml.cs**, créez une fonction pour initialiser le référentiel REST et appelez-le au lieu de **SqliteDatabase** lorsque l’application est initialisée. Là encore, reportez-vous à [l’exemple complet](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/App.xaml.cs).

Une fois les trois étapes terminées, vous devez être en mesure de vous authentifier auprès de votre compte AAD par le biais de votre application. Les appels REST à la base de données distante remplacent les appels SQLite locaux, mais l’expérience utilisateur doit être la même. Si vous vous sentez encore plus ambitieux, vous pouvez ajouter une page de paramètres pour permettre à l’utilisateur de basculer dynamiquement entre les deux.
