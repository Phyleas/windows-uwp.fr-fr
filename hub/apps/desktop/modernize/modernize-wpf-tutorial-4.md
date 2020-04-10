---
description: Ce tutoriel montre comment ajouter des interfaces utilisateur XAML UWP, créer des packages MSIX et incorporer d’autres composants modernes dans votre application WPF.
title: Ajouter des activités et des notifications utilisateur Windows 10
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, xaml islands, îles xaml
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 8443ac25ba678986046b967a90a8899eaffb76aa
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420124"
---
# <a name="part-4-add-windows-10-user-activities-and-notifications"></a>Partie 4 : Ajouter des activités et des notifications utilisateur Windows 10

Il s’agit de la quatrième partie d’un tutoriel qui explique comment moderniser un exemple d’application de bureau WPF nommé Contoso Expenses. Pour obtenir une vue d’ensemble du tutoriel, les prérequis et des instructions pour le téléchargement de l’exemple d’application, consultez [Tutoriel : Moderniser une application WPF](modernize-wpf-tutorial.md). Cet article part du principe que vous avez suivi la [troisième partie](modernize-wpf-tutorial-3.md) de ce tutoriel.

Dans les parties précédentes de ce tutoriel, vous avez ajouté des contrôles XAML UWP à l’application à l’aide d’îlots XAML. Dans le cadre de cet ajout, vous avez également autorisé l’application à appeler n’importe quelle API WinRT. Cela permet à l’application d’utiliser de nombreuses autres fonctionnalités proposées par Windows 10, et pas seulement les contrôles XAML UWP.

Dans le scénario fictif de ce tutoriel, l’équipe de développement Contoso a décidé d’ajouter deux nouvelles fonctionnalités à l’application : les activités et les notifications. Cette partie du tutoriel explique comment implémenter ces fonctionnalités.

## <a name="add-a-user-activity"></a>Ajouter une activité utilisateur

Dans Windows 10, les applications peuvent effectuer le suivi des activités réalisées par l’utilisateur, telles que l’ouverture d’un fichier ou l’affichage d’une page. Ces activités sont ensuite rendues disponibles par le biais de la chronologie, fonctionnalité datant de Windows 10 version 1803, qui permet à l’utilisateur de remonter rapidement dans le temps et de reprendre une activité qu’il a déjà commencée.

![Image de la Chronologie Windows](images/wpf-modernize-tutorial/WindowsTimeline.png)

Les activités utilisateur sont suivies à l’aide de [Microsoft Graph](https://developer.microsoft.com/graph/). Toutefois, lorsque vous créez une application Windows 10, vous n’avez pas besoin d’interagir directement avec les points de terminaison REST fournis par Microsoft Graph. En effet, vous pouvez utiliser un ensemble pratique d’API WinRT. Nous allons utiliser ces API WinRT dans l’application Contoso Expenses afin de détecter chaque ouverture de dépense dans l’application, puis nous allons utiliser des cartes adaptatives pour permettre aux utilisateurs de créer l’activité.

### <a name="introduction-to-adaptive-cards"></a>Présentation des cartes adaptatives

Cette section fournit une brève présentation des [cartes adaptatives](https://docs.microsoft.com/adaptive-cards/). Si vous n’avez pas besoin de ces informations, vous pouvez accéder directement aux instructions fournies dans [Ajouter une carte adaptative](#add-an-adaptive-card).

Les cartes adaptatives permettent aux développeurs d’échanger du contenu de carte de manière commune et cohérente. Une carte adaptative est décrite par une charge utile JSON qui définit son contenu (par exemple, du texte, des images, des actions, etc.).

Une carte adaptative définit uniquement le contenu et non l’apparence visuelle de celui-ci. La plateforme qui reçoit la carte adaptative peut restituer le contenu à l’aide du style le plus approprié. Les cartes adaptatives sont conçues à l’aide d’un [convertisseur](https://docs.microsoft.com/adaptive-cards/rendering-cards/getting-started) (renderer), qui permet de convertir la charge utile JSON en interface utilisateur native. Par exemple, l’interface utilisateur peut être au format XAML pour une application WPF ou UWP, au format AXML pour une application Android, ou au format HTML pour un site web ou une conversation de chatbot.

Voici un exemple simple de charge utile de carte adaptative.

```json
{
    "type": "AdaptiveCard",
    "body": [
        {
            "type": "Container",
            "items": [
                {
                    "type": "TextBlock",
                    "size": "Medium",
                    "weight": "Bolder",
                    "text": "Publish Adaptive Card schema"
                },
                {
                    "type": "ColumnSet",
                    "columns": [
                        {
                            "type": "Column",
                            "items": [
                                {
                                    "type": "Image",
                                    "style": "Person",
                                    "url": "https://pbs.twimg.com/profile_images/3647943215/d7f12830b3c17a5a9e4afcc370e3a37e_400x400.jpeg",
                                    "size": "Small"
                                }
                            ],
                            "width": "auto"
                        },
                        {
                            "type": "Column",
                            "items": [
                                {
                                    "type": "TextBlock",
                                    "weight": "Bolder",
                                    "text": "Matt Hidinger",
                                    "wrap": true
                                },
                                {
                                    "type": "TextBlock",
                                    "spacing": "None",
                                    "text": "Created {{DATE(2017-02-14T06:08:39Z,SHORT)}}",
                                    "isSubtle": true,
                                    "wrap": true
                                }
                            ],
                            "width": "stretch"
                        }
                    ]
                }
            ]
        }
    ],
    "actions": [
        {
            "type": "Action.ShowCard",
            "title": "Set due date",
            "card": {
                "type": "AdaptiveCard",
                "style": "emphasis",
                "body": [
                    {
                        "type": "Input.Date",
                        "id": "dueDate"
                    },
                    {
                        "type": "Input.Text",
                        "id": "comment",
                        "placeholder": "Add a comment",
                        "isMultiline": true
                    }
                ],
                "actions": [
                    {
                        "type": "Action.OpenUrl",
                        "title": "OK",
                        "url": "http://adaptivecards.io"
                    }
                ],
                "$schema": "http://adaptivecards.io/schemas/adaptive-card.json"
            }
        },
        {
            "type": "Action.OpenUrl",
            "title": "View",
            "url": "http://adaptivecards.io"
        }
    ],
    "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
    "version": "1.0"
}
```

L’image ci-dessous montre comment ce JSON est restitué de différentes façons par le canal Teams, par Cortana et par une notification Windows.

![Image du rendu de la carte adaptative](images/wpf-modernize-tutorial/AdaptiveCards.png)

Les cartes adaptatives jouent un rôle important dans la fonctionnalité Chronologie, car ce sont elles que Windows utilise pour restituer les activités. Chaque miniature affichée dans la chronologie est en fait une carte adaptative. Par conséquent, quand vous créez une activité utilisateur dans votre application, vous êtes invité à fournir une carte adaptative pour son affichage.

> [!NOTE]
> Pour réfléchir à la conception d’une carte adaptative, l’utilisation d’un [concepteur en ligne](https://adaptivecards.io/designer/) est idéale. Vous pourrez concevoir la carte avec des composants (images, textes, colonnes, etc.) et obtenir le JSON correspondant. Une fois que vous avez une idée de la conception finale, vous pouvez utiliser la bibliothèque [AdaptiveCards](https://www.nuget.org/packages/AdaptiveCards/) pour faciliter la génération de votre carte adaptative à l’aide de classes C# au lieu d’un JSON simple, qui peut être difficile à déboguer et à générer.

### <a name="add-an-adaptive-card"></a>Ajouter une carte adaptative

1. Cliquez avec le bouton droit sur le projet **ContosoExpenses.Core** dans l’Explorateur de solutions, puis choisissez **Gérer les packages NuGet**.

2. Dans la fenêtre **Gestionnaire de package NuGet**, cliquez sur **Parcourir**. Recherchez le package `Newtonsoft.Json` et installez la dernière version disponible. Il s’agit d’une bibliothèque de manipulation JSON populaire qui permet de manipuler plus facilement les chaînes JSON dont ont besoin les cartes adaptatives.

    ![Package NuGet NewtonSoft.Json](images/wpf-modernize-tutorial/JsonNetNuGet.png)

    > [!NOTE]
    > Si vous n’installez pas le package `Newtonsoft.Json` séparément, la bibliothèque AdaptiveCards référencera une version antérieure du package `Newtonsoft.Json` qui ne prend pas en charge .NET Core 3.0.

2. Dans la fenêtre **Gestionnaire de package NuGet**, cliquez sur **Parcourir**. Recherchez le package `AdaptiveCards` et installez la dernière version disponible.

    ![Package NuGet des cartes adaptatives](images/wpf-modernize-tutorial/AdaptiveCardsNuGet.png)

3. Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet **ContosoExpenses.Core**, puis choisissez **Ajouter > Classe**. Nommez la classe **TimelineService.cs**, puis cliquez sur **OK**.

4. Dans le fichier **TimelineService.cs**, ajoutez les instructions suivantes en haut du fichier.

    ```csharp
    using AdaptiveCards;
    using ContosoExpenses.Data.Models;
    ```

5. Remplacez l’espace de noms déclaré dans le fichier `ContosoExpenses.Core` par `ContosoExpenses`.

5. Ajoutez la méthode suivante à la classe `TimelineService`.

   ```csharp
    private string BuildAdaptiveCard(Expense expense)
    {
        AdaptiveCard card = new AdaptiveCard("1.0");

        AdaptiveTextBlock title = new AdaptiveTextBlock
        {
            Text = expense.Description,
            Size = AdaptiveTextSize.Medium,
            Wrap = true
        };

        AdaptiveColumnSet columnSet = new AdaptiveColumnSet();
        AdaptiveColumn photoColumn = new AdaptiveColumn
        {
            Width = "auto"
        };

        AdaptiveImage image = new AdaptiveImage
        {
            Url = new Uri("https://appmodernizationworkshop.blob.core.windows.net/contosoexpenses/Contoso192x192.png"),
            Size = AdaptiveImageSize.Small,
            Style = AdaptiveImageStyle.Default
        };
        photoColumn.Items.Add(image);

        AdaptiveTextBlock amount = new AdaptiveTextBlock
        {
            Text = expense.Cost.ToString(),
            Weight = AdaptiveTextWeight.Bolder,
            Wrap = true
        };

        AdaptiveTextBlock date = new AdaptiveTextBlock
        {
            Text = expense.Date.Date.ToShortDateString(),
            IsSubtle = true,
            Spacing = AdaptiveSpacing.None,
            Wrap = true
        };

        AdaptiveColumn expenseColumn = new AdaptiveColumn
        {
            Width = "stretch"
        };
        expenseColumn.Items.Add(amount);
        expenseColumn.Items.Add(date);

        columnSet.Columns.Add(photoColumn);
        columnSet.Columns.Add(expenseColumn);

        card.Body.Add(title);
        card.Body.Add(columnSet);

        string json = card.ToJson();
        return json;
    }
    ```

#### <a name="about-the-code"></a>À propos du code

Cette méthode reçoit un objet **Expense** avec toutes les informations sur les dépenses qui doivent être affichées, puis elle génère un nouvel objet **AdaptiveCard**. La méthode ajoute le code suivant à la carte :

- Un titre, qui utilise la description de la dépense
- Une image, qui correspond au logo Contoso
- Le montant de la dépense
- La date de la dépense

Les trois derniers éléments sont répartis en deux colonnes, de sorte que le logo Contoso et le détail des dépenses peuvent être placés côte à côte. Après la génération de l’objet, la méthode retourne la chaîne JSON correspondante à l’aide de la méthode **ToJson**.

### <a name="define-the-user-activity"></a>Définir l’activité utilisateur

Maintenant que vous avez défini la carte adaptative, vous pouvez créer une activité utilisateur basée sur celle-ci.

1. Ajoutez les instructions suivantes au début du fichier **TimelineService.cs** :

    ```csharp
    using Windows.ApplicationModel.UserActivities;
    using System.Threading.Tasks;
    using Windows.UI.Shell;
    ```

    > [!NOTE]
    > Il s’agit d’espaces de noms UWP. Ceux-ci sont résolus, car le package NuGet `Microsoft.Toolkit.Wpf.UI.Controls` que vous avez installé à l’étape 2 comprend une référence au package `Microsoft.Windows.SDK.Contracts`, qui permet au projet **ContosoExpenses.Core** de référencer des API WinRT, même s’il s’agit d’un projet .NET Core 3.

2. Ajoutez les déclarations de champ suivantes à la classe `TimelineService`.

    ```csharp
    private UserActivityChannel _userActivityChannel;
    private UserActivity _userActivity;
    private UserActivitySession _userActivitySession;
    ```

3. Ajoutez la méthode suivante à la classe `TimelineService`.

    ```csharp
    public async Task AddToTimeline(Expense expense)
    {
        _userActivityChannel = UserActivityChannel.GetDefault();
        _userActivity = await _userActivityChannel.GetOrCreateUserActivityAsync($"Expense-{expense.ExpenseId}");

        _userActivity.ActivationUri = new Uri($"contosoexpenses://expense/{expense.ExpenseId}");
        _userActivity.VisualElements.DisplayText = "Contoso Expenses";

        string json = BuildAdaptiveCard(expense);

        _userActivity.VisualElements.Content = AdaptiveCardBuilder.CreateAdaptiveCardFromJson(json);

        await _userActivity.SaveAsync();
        _userActivitySession?.Dispose();
        _userActivitySession = _userActivity.CreateSession();
    }
    ```

4. Enregistrez les modifications apportées à **TimelineService.cs**.

#### <a name="about-the-code"></a>À propos du code

La méthode `AddToTimeline` obtient d’abord un objet **UserActivityChannel** qui est nécessaire au stockage des activités utilisateur. Ensuite, elle crée une activité utilisateur à l’aide de la méthode **GetOrCreateUserActivityAsync**, qui exige un identificateur unique. De cette façon, si une activité existe déjà, l’application peut la mettre à jour. Dans le cas contraire, elle en crée une nouvelle. L’identificateur à passer dépend du type d’application que vous créez :

* Si vous souhaitez mettre à jour toujours la même activité pour que la chronologie n’affiche que la plus récente, vous pouvez utiliser un identificateur fixe (par exemple, **Expenses**).
* Si vous souhaitez effectuer le suivi de chaque activité séparément pour que la chronologie affiche toutes les activités, vous pouvez utiliser un identificateur dynamique.

Dans ce scénario, l’application considère chaque ouverture de dépense comme une activité utilisateur différente, de sorte que le code crée chaque identificateur à l’aide du mot clé **Expense-** suivi de l’ID de dépense unique.

Une fois que la méthode a créé un objet **UserActivity**, elle fournit à l’objet les informations suivantes :

* Un **ActivationUri** qui est appelé lorsque l’utilisateur clique sur l’activité dans la chronologie. Le code utilise un protocole personnalisé appelé **contosoexpenses** dont l’application s’occupera plus tard.
* L’objet **VisualElements**, qui contient un ensemble de propriétés qui définissent l’apparence visuelle de l’activité. Ce code définit **DisplayText** (qui correspond au titre qui s’affiche au-dessus de l’entrée dans la chronologie), ainsi que **Content** (le contenu). 

C’est là qu’entre en jeu la carte adaptative que vous avez définie précédemment. L’application passe à la méthode la carte adaptative que vous avez créée précédemment en tant que contenu. Toutefois, pour représenter une carte, Windows 10 utilise un objet différent de celui utilisé par le package NuGet `AdaptiveCards`. Par conséquent, la méthode recrée la carte à l’aide de la méthode **CreateAdaptiveCardFromJson** exposée par la classe **AdaptiveCardBuilder**. Une fois que la méthode a créé l’activité utilisateur, elle l’enregistre puis crée une nouvelle session.

Lorsqu’un utilisateur clique sur une activité dans la chronologie, le protocole **contosoexpenses://** est activé et l’URL inclut les informations dont l’application a besoin pour récupérer les dépenses sélectionnées. Si vous le souhaitez, vous pouvez implémenter l’activation du protocole pour que l’application réagisse correctement lorsque l’utilisateur utilise la chronologie.

### <a name="integrate-the-application-with-timeline"></a>Intégrer l’application à la chronologie

Maintenant que vous avez créé une classe qui interagit avec la chronologie, vous pouvez commencer à l’utiliser pour améliorer l’expérience de l’application. Le meilleur moment auquel utiliser la méthode **AddToTimeline** exposée par la classe **TimelineService** est lorsque l’utilisateur ouvre la page de détails d’une dépense.

1. Dans le projet **ContosoExpenses.Core**, développez le dossier **ViewModels** et ouvrez le fichier **ExpenseDetailViewModel.cs**. Il s’agit du ViewModel qui prend en charge la fenêtre des détails des dépenses.

2. Recherchez le constructeur public de la classe **ExpenseDetailViewModel**, puis ajoutez le code suivant à la fin du constructeur. Chaque fois que la fenêtre de dépenses est ouverte, la méthode appelle la méthode **AddToTimeline** et passe la dépense actuelle. La classe **TimelineService** utilise ces informations pour créer une activité utilisateur à l’aide des informations sur les dépenses.

    ```csharp
    TimelineService timeline = new TimelineService();
    timeline.AddToTimeline(expense);
    ```

    Lorsque vous avez terminé, le constructeur doit ressembler à ceci.

    ```csharp
    public ExpensesDetailViewModel(IDatabaseService databaseService, IStorageService storageService)
    {
        var expense = databaseService.GetExpense(storageService.SelectedExpense);

        ExpenseType = expense.Type;
        Description = expense.Description;
        Location = expense.Address;
        Amount = expense.Cost;

        TimelineService timeline = new TimelineService();
        timeline.AddToTimeline(expense);
    }
    ```

3. Appuyez sur la touche F5 pour générer et exécuter l’application dans le débogueur. Choisissez un employé dans la liste, puis choisissez une dépense. Dans la page de détails, notez la description, la date et le montant de la dépense.

4. Appuyez sur **Démarrer + Espace** pour ouvrir la chronologie.

5. Faites défiler la liste des applications actuellement ouvertes jusqu’à la section intitulée **Plus tôt aujourd’hui**. Cette section présente certaines des activités utilisateur les plus récentes. Cliquez sur le lien **Voir toutes les activités** en regard de l’en-tête **Plus tôt aujourd’hui**.

6. Vérifiez que vous voyez une nouvelle carte contenant les informations sur la dépense que vous avez sélectionnée dans l’application.

    ![Chronologie des dépenses Contoso](images/wpf-modernize-tutorial/ContosoExpensesTimeline.png)

7. Si vous ouvrez d’autres dépenses, de nouvelles cartes seront ajoutées en tant qu’activités utilisateur. N’oubliez pas que le code utilise un identificateur différent pour chaque activité. Il crée donc une carte pour chaque dépense que vous ouvrez dans l’application.

8. Fermez l’application.

## <a name="add-a-notification"></a>Ajouter une notification

La deuxième fonctionnalité que l’équipe de développement Contoso souhaite ajouter est une notification qui est présentée à l’utilisateur chaque fois qu’une nouvelle dépense est enregistrée dans la base de données. Pour ce faire, vous pouvez tirer parti du système de notification intégré dans Windows 10, qui est accessible aux développeurs via les API WinRT. Ce système de notification présente de nombreux avantages :

- Les notifications sont cohérentes avec le reste du système d’exploitation.
- Elles sont actionnables.
- Elles sont stockées dans le centre de notifications afin de pouvoir être consultées ultérieurement.

Pour ajouter une notification à l’application :

1. Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet **ContosoExpenses.Core**, puis choisissez **Ajouter > Classe**. Nommez la classe **NotificationService.cs**, puis cliquez sur **OK**.

2. Dans le fichier **NotificationService.cs**, ajoutez les instructions suivantes en haut du fichier.

    ```csharp
    using Windows.Data.Xml.Dom;
    using Windows.UI.Notifications;
    ```

3. Remplacez l’espace de noms déclaré dans le fichier `ContosoExpenses.Core` par `ContosoExpenses`.

4. Ajoutez la méthode suivante à la classe `NotificationService`.

    ```csharp
    public void ShowNotification(string description, double amount)
    {
        string xml = $@"<toast>
                          <visual>
                            <binding template='ToastGeneric'>
                              <text>Expense added</text>
                              <text>Description: {description} - Amount: {amount} </text>
                            </binding>
                          </visual>
                        </toast>";

        XmlDocument doc = new XmlDocument();
        doc.LoadXml(xml);

        ToastNotification toast = new ToastNotification(doc);
        ToastNotificationManager.CreateToastNotifier().Show(toast);
    }
    ```

    Les notifications toast sont représentées par une charge utile XML, qui peut inclure du texte, des images, des actions, etc. Vous trouverez tous les éléments pris en charge [ici](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/toast-schema). Ce code utilise un schéma très simple comprenant deux lignes de texte : le titre et le corps. Une fois que le code a défini la charge utile XML et l’a chargée dans un objet **XmlDocument**, il wrappe le code XML dans un objet **ToastNotification** et l’affiche à l’aide d’une classe **ToastNotificationManager**.

5. Dans le projet **ContosoExpenses.Core**, développez le dossier **ViewModels** et ouvrez le fichier **AddNewExpenseViewModel.cs**. 

6. Recherchez la méthode `SaveExpenseCommand`, qui est déclenchée lorsque l’utilisateur appuie sur le bouton pour enregistrer une nouvelle dépense. Ajoutez le code suivant à cette méthode, juste après l’appel à la méthode `SaveExpense`.

    ```csharp
    NotificationService notificationService = new NotificationService();
    notificationService.ShowNotification(expense.Description, expense.Cost);
    ```

    Lorsque vous avez terminé, la méthode `SaveExpenseCommand` doit ressembler à ceci.

    ```csharp
    private RelayCommand _saveExpenseCommand;
    public RelayCommand SaveExpenseCommand
    {
        get
        {
            if (_saveExpenseCommand == null)
            {
                _saveExpenseCommand = new RelayCommand(() =>
                {
                    Expense expense = new Expense
                    {
                        Address = Address,
                        City = City,
                        Cost = Cost,
                        Date = Date,
                        Description = Description,
                        EmployeeId = storageService.SelectedEmployeeId,
                        Type = ExpenseType
                    };

                    databaseService.SaveExpense(expense);

                    NotificationService notificationService = new NotificationService();
                    notificationService.ShowNotification(expense.Description, expense.Cost);

                    Messenger.Default.Send<UpdateExpensesListMessage>(new UpdateExpensesListMessage());
                    Messenger.Default.Send<CloseWindowMessage>(new CloseWindowMessage());
                }, () => IsFormFilled
                );
            }

            return _saveExpenseCommand;
        }
    }
    ```

7. Appuyez sur la touche F5 pour générer et exécuter l’application dans le débogueur. Choisissez un employé dans la liste, puis cliquez sur le bouton **Ajouter une nouvelle dépense**. Complétez tous les champs du formulaire, puis appuyez sur **Enregistrer**.

8. L’exception suivante s’affiche.

    ![Erreur de notification toast](images/wpf-modernize-tutorial/ToastNotificationError.png)

Cette exception est due au fait que l’application Contoso Expenses ne dispose pas encore d’une identité de package. Certaines API WinRT, dont l’API des notifications, exigent une identité de package avant de pouvoir être utilisées dans une application. Par défaut, les applications UWP reçoivent l’identité du package, car elles peuvent uniquement être distribuées via des packages MSIX. D’autres types d’applications Windows, notamment les applications WPF, peuvent également être déployés via des packages MSIX pour obtenir l’identité du package. La partie suivante de ce tutoriel explique la procédure à suivre.

## <a name="next-steps"></a>Étapes suivantes

À ce stade du tutoriel, vous avez ajouté à l’application une activité utilisateur qui s’intègre à la chronologie Windows, puis vous avez ajouté à l’application une notification qui est déclenchée lorsque les utilisateurs créent une nouvelle dépense. Toutefois, la notification ne fonctionne pas encore, car l’application exige l’identité du package pour utiliser l’API des notifications. Si vous voulez savoir comment créer un package MSIX pour l’application afin d’obtenir l’identité du package et obtenir d’autres avantages concernant le déploiement, consultez [Partie 5 : Créer un package et le déployer avec MSIX](modernize-wpf-tutorial-5.md).
