---
title: Partager du code entre une application de bureau et une application UWP
description: Découvrez comment déplacer une application de bureau à partir de .NET Framework (avec des API WPF et Windows Forms) ou C++ Win32 vers plateforme Windows universelle (UWP) et Windows 10.
ms.date: 10/03/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ed23f77936378f2348abf868a67041be84978123
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304681"
---
# <a name="move-from-a-desktop-application-to-uwp"></a>Passer d’une application de bureau à UWP

Si vous disposez d’une application de bureau existante qui a été créée à l’aide de l' .NET Framework (y compris les API WPF et Windows Forms) ou C++ Win32, vous avez plusieurs options pour passer au plateforme Windows universelle (UWP) et à Windows 10.

## <a name="package-your-desktop-application-in-an-msix-package"></a>Empaqueter votre application de bureau dans un package MSIX

Vous pouvez empaqueter votre application de bureau dans un package MSIX pour accéder à de nombreuses autres fonctionnalités Windows 10. MSIX est un format de package d’application Windows moderne qui permet de créer des packages universels pour toutes les applications Windows, notamment les applications UWP, WPF, Windows Forms et Win32. En empaquetant vos applications de bureau Windows dans des packages MSIX, vous avez accès à une expérience d’installation et de mise à jour fiable, à un modèle de sécurité managé avec un système de capacité flexible, à un support pour le Microsoft Store, à la gestion d’entreprise et à de nombreux modèles de distribution personnalisés. Vous pouvez empaqueter votre application si vous avez le code source ou si vous disposez uniquement d’un fichier d’installation existant (par exemple, un programme d’installation MSI ou App-V). Une fois votre application empaquetée, vous pouvez intégrer des fonctionnalités UWP telles que les extensions de package et d’autres composants UWP.

Pour plus d’informations, consultez [package Desktop applications (Desktop Bridge)](/windows/msix/desktop/desktop-to-uwp-root) et les [fonctionnalités qui requièrent l’identité du package](/windows/apps/desktop/modernize/modernize-packaged-apps).

## <a name="use-windows-runtime-apis"></a>Utiliser des API Windows Runtime

Vous pouvez appeler de nombreuses API Windows Runtime directement dans votre application de bureau WPF, Windows Forms ou Win32 C++ afin d’apporter aux utilisateurs de Windows 10 des expériences modernes. Par exemple, vous pouvez appeler des API Windows Runtime pour ajouter des notifications toast à votre application de bureau.

Pour plus d’informations, consultez [Utiliser des API Windows Runtime dans les applications de bureau](/windows/apps/desktop/modernize/desktop-to-uwp-enhance).

## <a name="migrate-a-net-framework-app-to-a-uwp-app"></a>Migrer une application .NET Framework vers une application UWP

Si votre application s’exécute sur le .NET Framework, vous pouvez la migrer vers une application UWP en tirant parti de .NET Standard 2,0. Déplacez autant de code que possible dans .NET Standard bibliothèques de classes 2,0, puis créez une application UWP qui référence vos bibliothèques .NET Standard 2,0. 

### <a name="share-code-in-a-net-standard-20-library"></a>Partager du code dans une bibliothèque .NET Standard 2,0

Si votre application s’exécute sur le .NET Framework, placez autant de code que possible dans .NET Standard bibliothèques de classes 2,0. Tant que votre code utilise des API définies dans le standard, vous pouvez le réutiliser dans une application UWP. Il est plus facile que jamais de partager du code dans une bibliothèque .NET Standard, car de nombreuses API supplémentaires sont incluses dans le .NET Standard 2,0.

Voici une vidéo qui vous en dit davantage.

> [!VIDEO https://www.youtube-nocookie.com/embed/YI4MurjfMn8?list=PLRAdsfhKI4OWx321A_pr-7HhRNk7wOLLY&amp;ecver=1]

#### <a name="add-net-standard-libraries"></a>Ajouter des bibliothèques de .NET Standard

Tout d’abord, ajoutez une ou plusieurs bibliothèques de classes .NET Standard à votre solution.  

![Ajouter un projet standard dotnet](images/desktop-to-uwp/dot-net-standard-project-template.png)

Le nombre de bibliothèques que vous ajoutez à votre solution dépend de la façon dont vous planifiez l’organisation de votre code.

Assurez-vous que chaque bibliothèque de classes cible le **.NET Standard 2,0**.

![.NET Standard cible 2,0](images/desktop-to-uwp/target-standard-20.png)

Ce paramètre se trouve dans les pages de propriétés du projet de bibliothèque de classes.

À partir de votre projet d’application de bureau, ajoutez une référence au projet de bibliothèque de classes.

![Référence de la bibliothèque de classes](images/desktop-to-uwp/class-library-reference.png)

Ensuite, utilisez les outils pour déterminer la quantité de code conforme à la norme. De cette façon, avant de déplacer du code dans la bibliothèque, vous pouvez choisir les parties que vous pouvez réutiliser, les parties qui nécessitent une modification minimale et les parties qui resteront spécifiques à l’application.

#### <a name="check-library-and-code-compatibility"></a>Vérifier la compatibilité de la bibliothèque et du code

Nous allons commencer par des packages NuGet et d’autres fichiers dll obtenus auprès d’un tiers.

Si votre application utilise l’une d’elles, déterminez si elles sont compatibles avec le .NET Standard 2,0. Pour ce faire, vous pouvez utiliser une extension Visual Studio ou un utilitaire de ligne de commande.

Utilisez ces mêmes outils pour analyser votre code. Téléchargez les outils ici ([dotnet-apiport](https://github.com/Microsoft/dotnet-apiport/releases)), puis regardez cette vidéo pour apprendre à les utiliser.
&nbsp;
> [!VIDEO https://www.youtube-nocookie.com/embed/rzs_FGPyAlY?list=PLRAdsfhKI4OWx321A_pr-7HhRNk7wOLLY&amp;ecver=2]

Si votre code n’est pas compatible avec la norme, pensez à d’autres façons d’implémenter ce code. Commencez par ouvrir le [navigateur de l’API .net](/dotnet/api/?view=netstandard-2.0). Vous pouvez utiliser ce navigateur pour passer en revue les API disponibles dans le .NET Standard 2,0. Veillez à étendre la liste à la .NET Standard 2,0.

![point net (option)](images/desktop-to-uwp/dot-net-option.png)

Une partie de votre code sera spécifique à la plateforme et devra rester dans votre projet d’application de bureau.

#### <a name="example-migrating-data-access-code-to-a-net-standard-20-library"></a>Exemple : migration du code d’accès aux données vers une bibliothèque .NET Standard 2,0

Supposons que nous disposons d’une application Windows Forms de base qui montre les clients de notre exemple de base de données Northwind.

![Application Windows Forms](images/desktop-to-uwp/win-forms-app.png)

Le projet contient une bibliothèque de classes .NET Standard 2,0 avec une classe statique nommée **Northwind**. Si nous déplaçons ce code dans la classe **Northwind** , il ne sera pas compilé, car il utilise les ``SQLConnection`` ``SqlCommand`` classes, et ``SqlDataReader`` , ainsi que les classes qui ne sont pas disponibles dans le .NET standard 2,0.

```csharp
public static ArrayList GetCustomerNames()
{
    ArrayList customers = new ArrayList();

    using (SqlConnection conn = new SqlConnection())
    {
        conn.ConnectionString =
            @"Data Source=" +
            @"<Your Server Name>\SQLEXPRESS;Initial Catalog=NORTHWIND;Integrated Security=SSPI";

        conn.Open();

        SqlCommand command = new SqlCommand("select ContactName from customers order by ContactName asc", conn);

        using (SqlDataReader reader = command.ExecuteReader())
        {
            while (reader.Read())
            {
                customers.Add(reader[0]);
            }
        }
    }

    return customers;
}

```
Nous pouvons utiliser le [navigateur de l’API .net](/dotnet/api/?view=netstandard-2.0) pour trouver une alternative. Les ``DbConnection`` ``DbCommand`` classes, et ``DbDataReader`` sont toutes disponibles dans le .NET standard 2,0 afin de pouvoir les utiliser à la place.  

Cette version révisée utilise ces classes pour obtenir une liste de clients, mais pour créer une ``DbConnection`` classe, nous devrons passer un objet de fabrique que nous créons dans l’application cliente.

```csharp
public static ArrayList GetCustomerNames(DbProviderFactory factory)
{
    ArrayList customers = new ArrayList();

    using (DbConnection conn = factory.CreateConnection())
    {
        conn.ConnectionString = @"Data Source=" +
                        @"<Your Server Name>\SQLEXPRESS;Initial Catalog=NORTHWIND;Integrated Security=SSPI";

        conn.Open();

        DbCommand command = factory.CreateCommand();
        command.Connection = conn;
        command.CommandText = "select ContactName from customers order by ContactName asc";

        using (DbDataReader reader = command.ExecuteReader())
        {
            while (reader.Read())
            {
                customers.Add(reader[0]);
            }
        }
    }

    return customers;
}
```  

Dans la page code-behind du Windows Form, nous pouvons simplement créer l’instance Factory et la passer à notre méthode.

```csharp
public partial class Customers : Form
{
    public Customers()
    {
        InitializeComponent();

        dataGridView1.Rows.Clear();

        SqlClientFactory factory = SqlClientFactory.Instance;

        foreach (string customer in Northwind.GetCustomerNames(factory))
        {
            dataGridView1.Rows.Add(customer);
        }
    }
}
```

### <a name="create-a-uwp-app"></a>Créer une application UWP

Vous êtes maintenant prêt à ajouter une application UWP à votre solution.

![image de bureau vers un pont UWP](images/desktop-to-uwp/adaptive-ui.png)

Vous devez toujours concevoir des pages d’interface utilisateur en XAML et écrire du code spécifique à une plateforme ou un appareil, mais lorsque vous avez terminé, vous pourrez atteindre l’intégralité des appareils Windows 10 et vos pages d’application auront une apparence moderne qui s’adapte parfaitement à différentes tailles et résolutions d’écran.

Votre application répondra aux mécanismes d’entrée autres qu’un clavier et une souris, et les fonctionnalités et les paramètres seront intuitifs sur les appareils. Cela signifie que les utilisateurs apprennent à effectuer des tâches une seule fois, puis il fonctionne de façon très familière, quel que soit l’appareil.

Il ne s’agit là que de quelques-unes des bonnes choses qui accompagnent UWP. Pour plus d’informations, consultez [créer des expériences exceptionnelles avec Windows](https://developer.microsoft.com/windows/why-build-for-uwp).

#### <a name="add-a-uwp-project"></a>Ajouter un projet UWP

Tout d’abord, ajoutez un projet UWP à votre solution.

![Projet UWP](images/desktop-to-uwp/new-uwp-app.png)

Ensuite, à partir de votre projet UWP, ajoutez une référence au projet de bibliothèque .NET Standard 2,0.

![Référence de la bibliothèque de classes](images/desktop-to-uwp/class-library-reference2.png)

#### <a name="build-your-pages"></a>Créer vos pages

Ajoutez des pages XAML et appelez le code dans votre bibliothèque .NET Standard 2,0.

![Application UWP](images/desktop-to-uwp/uwp-app.png)

```xml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <StackPanel x:Name="customerStackPanel">
        <ListView x:Name="customerList"/>
    </StackPanel>
</Grid>
```

```csharp
public sealed partial class MainPage : Page
{
    public MainPage()
    {
        this.InitializeComponent();

        SqlClientFactory factory = SqlClientFactory.Instance;

        customerList.ItemsSource = Northwind.GetCustomerNames(factory);
    }
}
```

Pour commencer à utiliser UWP, voir [qu’est-ce qu’une application UWP ?](../get-started/universal-application-platform-guide.md)

### <a name="reach-ios-and-android-devices"></a>Atteindre des appareils iOS et Android

Vous pouvez atteindre des appareils Android et iOS en ajoutant des projets Xamarin.  

![Applications Xamarin](images/desktop-to-uwp/xamarin-apps.png)

Ces projets vous permettent d’utiliser C# pour créer des applications Android et iOS avec un accès complet aux API spécifiques aux plateformes et aux appareils. Ces applications tirent parti de l’accélération matérielle spécifique à la plateforme et sont compilées pour les performances natives.

Ils ont accès à la gamme complète des fonctionnalités exposées par la plateforme et l’appareil sous-jacents, y compris les fonctionnalités spécifiques à la plateforme comme qu’ibeacons et les fragments Android. vous utiliserez des contrôles d’interface utilisateur natif standard pour créer des interfaces utilisateur qui ressemblent à la façon dont les utilisateurs les attendent.

Tout comme UWPs, le coût d’ajout d’une application Android ou iOS est plus faible, car vous pouvez réutiliser la logique métier dans une bibliothèque de classes .NET Standard 2,0. Vous devez concevoir vos pages d’interface utilisateur en XAML et écrire du code spécifique à une plateforme ou un appareil.

#### <a name="add-a-xamarin-project"></a>Ajouter un projet Xamarin

Tout d’abord, ajoutez un projet **Android**, **iOS**ou **multiplateforme** à votre solution.

Ces modèles se trouvent dans la boîte de dialogue **Ajouter un nouveau projet** , sous le groupe **Visual C#** .

![Applications Xamarin](images/desktop-to-uwp/xamarin-projects.png)

>[!NOTE]
>Les projets multiplateformes sont très utiles pour les applications avec peu de fonctionnalités spécifiques à la plateforme. Vous pouvez les utiliser pour créer une interface utilisateur XAML native qui s’exécute sur iOS, Android et Windows. En savoir plus [ici](/xamarin/xamarin-forms/).

Ensuite, à partir de votre projet Android, iOS ou multiplateforme, ajoutez une référence au projet de bibliothèque de classes.

![Référence de la bibliothèque de classes](images/desktop-to-uwp/class-library-reference3.png)

#### <a name="build-your-pages"></a>Créer vos pages

Notre exemple montre une liste de clients dans une application Android.

![Application Android](images/desktop-to-uwp/android-app.png)

```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:padding="10dp" android:textSize="16sp"
    android:id="@android:id/list">
</TextView>
```

```csharp
[Activity(Label = "MyAndroidApp", MainLauncher = true)]
public class MainActivity : ListActivity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);

        SqlClientFactory factory = SqlClientFactory.Instance;

        var customers = (string[])Northwind.GetCustomerNames(factory).ToArray(typeof(string));

        ListAdapter = new ArrayAdapter<string>(this, Resource.Layout.list_item, customers);
    }
}
```

Pour vous familiariser avec les projets Android, iOS et multiplateforme, consultez le [portail des développeurs Xamarin](/xamarin).

## <a name="next-steps"></a>Étapes suivantes

**Trouvez les réponses à vos questions**

Des questions ? Contactez-nous sur Stack Overflow. Notre équipe supervise ces [étiquettes](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Vous pouvez également nous poser vos questions [ici](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Envoyer des commentaires ou apporter des suggestions de fonctionnalités**

Consultez [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).