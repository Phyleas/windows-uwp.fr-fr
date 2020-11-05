---
description: Filtrez les éléments de votre collection par entrée utilisateur.
title: Filtrage des collections
label: Filtering collections
template: detail.hbs
ms.date: 12/3/2019
ms.topic: article
keywords: windows 10, uwp
pm-contact: anawish
ms.openlocfilehash: a62ec52fe2b8f6caac2ac27cfc4d002ec44a5b32
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034532"
---
# <a name="filtering-collections-and-lists-through-user-input"></a>Filtrage de collections et de listes par entrée utilisateur
Si votre collection affiche de nombreux éléments ou qu’elle est fortement liée à l’interaction utilisateur, le filtrage est une fonctionnalité utile à implémenter. Vous pouvez implémenter le filtrage selon la méthode décrite dans cet article pour la plupart des contrôles de collection, notamment [ListView](/uwp/api/Windows.UI.Xaml.Controls.ListView), [GridView](/uwp/api/windows.ui.xaml.controls.gridview) et [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater?view=winui-2.2). De nombreux types d’entrée utilisateur peuvent être utilisés pour filtrer une collection : cases à cocher, cases d’option, curseurs, etc. Cet article décrit comment utiliser une recherche d’utilisateur entrée au format texte pour mettre à jour un ListView en temps réel. 

> [!NOTE]
> Cet article est centré sur le filtrage avec un ListView. Sachez que la méthode de filtrage peut également être appliquée à d’autres contrôles de collection tels que GridView, ItemsRepeater ou TreeView.

## <a name="setting-up-the-ui-and-xaml-for-filtering"></a>Configuration de l’interface utilisateur et du XAML pour le filtrage
Pour implémenter le filtrage, votre application doit avoir un ListView situé à côté d’un TextBox ou d’un autre contrôle permettant à l’utilisateur d’entrer du texte. Le texte que l’utilisateur tape dans le TextBox sert de filtre. Ainsi, seuls les résultats contenant l’entrée de texte ou la requête de recherche s’affichent. À mesure que l’utilisateur tape du texte dans le TextBox, le ListView est mis à jour avec les résultats filtrés. En particulier, chaque fois que le texte dans le TextBox change, ne serait-ce que d’une seule lettre, les éléments du ListView sont filtrés selon ce terme.

Le code ci-dessous montre une interface utilisateur avec un ListView simple et son DataTemplate, ainsi qu’un TextBox associé. Dans cet exemple, le ListView affiche une collection d’objets Person. Person est une classe définie dans le code-behind (elle ne figure donc pas dans l’exemple de code ci-dessous), et chaque objet Person a les propriétés suivantes : FirstName, LastName et Company.

Les utilisateurs peuvent taper un terme de recherche/filtrage dans le TextBox pour filtrer la liste des objets Person par nom de famille. Notez que le TextBox est lié à un nom spécifique (`FilterByLName`) et qu’il a son propre événement TextChanged (`FilteredLV_LNameChanged`). Le nom lié nous permet d’accéder au contenu/texte du TextBox dans le code-behind. L’événement TextChanged se déclenche chaque fois que l’utilisateur tape du texte dans le TextBox, ce qui nous permet d’effectuer une opération de filtrage après réception de l’entrée utilisateur. 

Pour que le filtrage fonctionne, le ListView doit avoir une source de données pouvant être manipulée dans le code-behind, par exemple un `ObservableCollection<>`. Dans ce cas, la propriété ItemsSource de ListView est affectée à un `ObservableCollection<Person>` dans le code-behind. 

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="1*"></ColumnDefinition>
        <ColumnDefinition Width="1*"></ColumnDefinition>
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
            <RowDefinition Height="400"></RowDefinition>
            <RowDefinition Height="400"></RowDefinition>
    </Grid.RowDefinitions>

    <ListView x:Name="FilteredListView"
                Grid.Column="0"
                Margin="0,0,20,0">

        <ListView.ItemTemplate>
            <DataTemplate x:DataType="local:Person">
                <StackPanel>
                    <TextBlock Style="{ThemeResource BaseTextBlockStyle}" Margin="0,5,0,5">
                        <Run Text="{x:Bind FirstName}"></Run>
                        <Run Text="{x:Bind LastName}"></Run>
                    </TextBlock>
                    <TextBlock Style="{ThemeResource BodyTextBlockStyle}" Margin="0,5,0,5" Text="{x:Bind Company}"/>
                </StackPanel>
            </DataTemplate>
        </ListView.ItemTemplate>

    </ListView>

    <TextBox x:Name="FilterByLName" Grid.Column="1" Header="Last Name" Width="200"
             HorizontalAlignment="Left" VerticalAlignment="Top" Margin="0,0,0,20"
             TextChanged="FilteredLV_LNameChanged"/>
</Grid>
```
## <a name="filtering-the-data"></a>Filtrage des données
Les requêtes [LINQ](/dotnet/csharp/programming-guide/concepts/linq/introduction-to-linq-queries) vous permettent de regrouper, d’organiser et de sélectionner certains éléments dans une collection. Pour filtrer une liste, nous créons une requête LINQ qui sélectionne uniquement les termes correspondant à la requête de recherche ou au terme de filtrage entré par l’utilisateur dans le TextBox `FilterByLName`. Le résultat de la requête peut être affecté à un objet de collection [IEnumerable<T>](/dotnet/api/system.collections.generic.ienumerable-1). Nous pouvons ensuite utiliser cette collection pour la comparer à la liste d’origine : les éléments sans correspondance sont supprimés et ceux pour lesquels une correspondance existe sont rajoutés (en cas de retour arrière).

> [!NOTE]
> Pour que le ListView s’anime de la manière la plus intuitive qui soit lors de l’ajout et de la soustraction d’éléments, il est important d’effectuer ces opérations sur la collection ItemsSource de ListView elle-même (plutôt que de créer une collection d’objets filtrés et de l’affecter à la propriété ItemsSource de ListView).

Pour commencer, nous devons initialiser notre source de données d’origine dans une collection distincte telle que `List<T>` ou `ObservableCollection<T>`. Dans cet exemple, nous avons un `List<Person>` appelé `People` qui contient tous les objets Person affichés dans le ListView (le remplissage et l’initialisation de cette liste ne figurent pas dans l’extrait de code ci-dessous). Nous devons également stocker les données filtrées dans une liste qui change chaque fois qu’un filtre est appliqué. Nous utilisons un `ObservableCollection<Person>` appelé `PeopleFiltered` qui, au moment de l’initialisation, a le même contenu que `People`.
 
Le code ci-dessous effectue les étapes suivantes pour réaliser l’opération de filtrage :
 - Affecte `PeopledFiltered` à la propriété ItemsSource de ListView. 
 - Définit l’événement TextChanged, `FilteredLV_LNameChanged()`, pour le TextBox `FilterByLName`. Filtre les données à l’intérieur de cette fonction.
 - Accède à la requête de recherche ou au terme de filtrage entré par l’utilisateur par le biais de `FilterByLName.Text` pour filtrer les données. Utilise une requête LINQ pour sélectionner les éléments dans `People` dont le nom de famille contient le terme `FilterByLName.Text` et ajoute ces éléments correspondants à une collection nommée `TempFiltered`.
 - Compare la collection `PeopleFiltered` actuelle aux éléments récemment filtrés dans `TempFiltered`, puis supprime et ajoute des éléments de `PeopleFiltered` au besoin.
 - À mesure que des éléments sont supprimés et ajoutés de `PeopleFiltered`, le ListView est mis à jour et s’anime en conséquence.

 ```csharp
using System.Linq;

public MainPage()
{
    // Define People collection to hold all Person objects. 
    // Populate collection - i.e. add Person objects (not shown)
    IList<Person> People = new List<Person>();

    // Create PeopleFiltered collection and copy data from original People collection
    ObservableCollection<Person> PeopleFiltered = new ObservableCollection<Person>(People);

    // Set the ListView's ItemsSource property to the PeopleFiltered collection
    FilteredListView.ItemsSource = PeopleFiltered;

    // ... 
}

private void FilteredLV_LNameChanged(object sender, TextChangedEventArgs e)
{
    /* Perform a Linq query to find all Person objects (from the original People collection)
    that fit the criteria of the filter, save them in a new List called TempFiltered. */
    List<Person> TempFiltered;
    
    /* Make sure all text is case-insensitive when comparing, and make sure 
    the filtered items are in a List object */
    TempFiltered = people.Where(contact => contact.LastName.Contains(FilterByLName.Text, StringComparison.InvariantCultureIgnoreCase)).ToList();
    
    /* Go through TempFiltered and compare it with the current PeopleFiltered collection,
    adding and subtracting items as necessary: */

    // First, remove any Person objects in PeopleFiltered that are not in TempFiltered
    for (int i = PeopleFiltered.Count - 1; i >= 0; i--)
    {
        var item = PeopleFiltered[i];
        if (!TempFiltered.Contains(item))
        {
            PeopleFiltered.Remove(item);
        }
    }

    /* Next, add back any Person objects that are included in TempFiltered and may 
    not currently be in PeopleFiltered (in case of a backspace) */

    foreach (var item in TempFiltered)
    {
        if (!PeopleFiltered.Contains(item))
        {
            PeopleFiltered.Add(item);
        }
    }
}
 ```

Maintenant, quand l’utilisateur tape ses termes de filtrage dans le TextBox `FilterByLName`, le ListView est immédiatement mis à jour et montre uniquement les personnes dont le nom de famille contient le terme de filtrage.

## <a name="next-steps"></a>Étapes suivantes

### <a name="get-the-sample-code"></a>Obtenir l’exemple de code
- Si vous avez installé l’application XAML Controls Gallery</strong>, cliquez [ici](xamlcontrolsgallery:/item/ListView) pour l’ouvrir et voir un exemple de filtrage de liste plus robuste et plus détaillé sur la page ListView.
- Obtenir l’[application XAML Controls Gallery (Microsoft Store)](https://www.microsoft.com/store/productId/9MSVH128X2ZT).

### <a name="related-articles"></a>Articles connexes
- [Listes](lists.md)
- [Mode Liste et affichage Grille](listview-and-gridview.md)
- [Commandes de collection](collection-commanding.md)
