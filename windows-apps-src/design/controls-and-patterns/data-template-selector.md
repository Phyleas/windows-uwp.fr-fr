---
Description: Utilisez les sélecteurs de modèle de données pour personnaliser les styles de vos éléments en fonction des propriétés des éléments.
title: Sélection du modèle de données
label: Data template selection
template: detail.hbs
ms.date: 10/18/2019
ms.topic: article
keywords: windows 10, uwp
pm-contact: anawish
ms.openlocfilehash: d388e1f4b3f1b1be4e265185934a02b6ccd20064
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "76123851"
---
# <a name="data-template-selection-styling-items-based-on-their-properties"></a>Sélection du modèle de données : Stylisation d’éléments en fonction de leurs propriétés

La conception personnalisée des contrôles de collections est gérée par un [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate). Les modèles de données définissent la disposition et le style de chaque élément, et ce balisage est appliqué à chaque élément de la collection. Cet article explique comment utiliser un [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector) pour appliquer différents modèles de données à une collection et sélectionner le modèle de données à utiliser en fonction de certaines valeurs ou propriétés d’élément de votre choix.

> **API importantes** : [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector), [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate)

[DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector) est une classe qui active une logique personnalisée de sélection de modèle. Elle vous permet de définir des règles qui spécifient le modèle de données à utiliser pour certains éléments d’une collection. Pour implémenter cette logique, vous créez une sous-classe de DataTemplateSelector dans votre code-behind et définissez la logique qui détermine quel modèle de données à utiliser pour quelle catégorie d’éléments (par exemple, les éléments d’un certain type, les éléments ayant une certaine valeur de propriété, etc.). Vous déclarez une instance de cette classe dans la section Resources de votre fichier XAML, ainsi que les définitions des modèles de données que vous allez utiliser. Vous identifiez ces ressources avec une valeur `x:Key`, ce qui vous permet de les référencer dans votre code XAML.

## <a name="prerequisites"></a>Prérequis

- Comment [utiliser et créer un contrôle de collection, comme ListView ou GridView](listview-and-gridview.md)
- Comment [personnaliser l’apparence de vos éléments à l’aide d’un DataTemplate](item-containers-templates.md#data-template)

## <a name="when-not-to-use-a-datatemplateselector"></a>Quand ne pas utiliser un DataTemplateSelector

En règle générale, vous ne devez pas donner à chaque élément d’un ListView ou GridView une disposition/un style complètement différent. Il s’agit là d’une mauvaise utilisation d’un DataTemplateSelector qui peut avoir un impact négatif sur les performances.

Vous pouvez contrôler certains éléments du visuel d’un élément de liste à l’aide d’un seul modèle de données en liant certaines propriétés. Par exemple, pour que des éléments aient une icône différente, créez une liaison avec une propriété de source d’icône dans le modèle de données et attribuez à chaque élément une valeur différente pour cette propriété de source d’icône. Vous obtiendriez de meilleures performances qu’avec un DataTemplateSelector.

## <a name="when-to-use-a-datatemplateselector"></a>Quand utiliser un DataTemplateSelector

Vous devez créer un [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector) quand vous souhaitez utiliser plusieurs modèles de données dans un contrôle de collection. Un `DataTemplateSelector` vous donne la possibilité de faire ressortir certains éléments tout en conservant une disposition d’éléments similaire. Il existe de nombreux cas d’utilisation dans lesquels un DataTemplateSelector est utile, et quelques scénarios dans lesquels il est préférable de repenser le contrôle et la stratégie que vous utilisez.

Les contrôles de collection sont généralement liés à une collection d’éléments qui sont tous du même type. Toutefois, même si les éléments peuvent être du même type, ils peuvent avoir des valeurs différentes pour certaines propriétés ou représenter des significations différentes. Certains éléments peuvent également être plus importants que d’autres, ou un élément peut être particulièrement important ou différent et doit se démarquer visuellement. Dans ces situations, un DataTemplateSelector est très utile.

Vous pouvez également créer une liaison à une collection qui contient différents types d’éléments. La collection liée peut être une combinaison de chaînes, d’entiers, d’objets de classe personnalisés, etc. Cela rend DataTemplateSelector particulièrement utile, car il peut affecter des modèles de données différents en fonction du type d’objet d’un élément.

Voici quelques exemples de cas dans lesquels vous pouvez utiliser un sélecteur de modèle de données  :

- Représentation de différents niveaux d’employés au sein d’un ListView - Vous souhaitez que les types/niveaux d’employé aient un arrière-plan de couleur différente pour les reconnaître plus facilement.
- Représentation d’articles en promotion dans une galerie de produits utilisant un GridView - Vous souhaitez que les articles en promotion aient un arrière-plan rouge ou une police de couleur différente pour les différencier des articles proposés au prix normal.
- Représentation des meilleures photos dans une galerie de photos avec FlipView.
- Représentation des nombres négatifs/positifs dans un ListView de manière différente, ou des chaînes courtes/longues.

## <a name="create-a-datatemplateselector"></a>Créer un DataTemplateSelector

Quand vous créez un sélecteur de modèle de données, vous définissez la logique de sélection du modèle dans votre code et les modèles de données dans votre code XAML.

### <a name="code-behind-component"></a>Composant code-behind

Pour utiliser un sélecteur de modèle de données, vous commencez par créer une sous-classe de [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector) (c’est-à-dire une classe qui en dérive) dans votre code-behind. Dans votre classe, vous déclarez chaque modèle en tant que propriété de la classe. Ensuite, vous remplacez la méthode [SelectTemplateCore](/uwp/api/windows.ui.xaml.controls.datatemplateselector.selecttemplatecore) pour inclure votre propre logique de sélection de modèle.

Voici un exemple d’une sous-classe `DataTemplateSelector` simple appelée `MyDataTemplateSelector`.

```csharp
public class MyDataTemplateSelector : DataTemplateSelector
{
    public DataTemplate Normal { get; set; }
    public DataTemplate Accent { get; set; }

    protected override DataTemplate SelectTemplateCore(object item)
    {
        if ((int)item % 2 == 0)
        {
            return Normal;
        }
        else
        {
            return Accent;
        }
    }
}
```

La classe `MyDataTemplateSelector` dérive de la classe `DataTemplateSelector` et définit d’abord deux objets [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) : `Normal` et `Accent`. Ces déclarations sont vides pour l’instant, mais elles seront « renseignées » avec le balisage dans le fichier XAML.

La méthode `SelectTemplateCore` reçoit un objet élément (c’est-à-dire chaque élément de la collection), et est remplacée par des règles stipulant quel `DataTemplate` retourner dans quelles circonstances. Dans ce cas, si l’élément est un nombre impair, il reçoit le modèle de données `Accent` ; s’il s’agit d’un nombre pair, il reçoit le modèle de données `Normal`.

### <a name="xaml-component"></a>Composant XAML

Ensuite, vous devez créer une instance de cette nouvelle classe `MyDataTemplateSelector` dans la section Resources de votre fichier XAML. Toutes les ressources nécessitent une `x:Key` que vous utilisez pour créer une liaison à la propriété `ItemTemplateSelector` de votre contrôle de collection (dans une étape ultérieure). Vous créez également deux instances des objets `DataTemplate` et vous définissez leur disposition dans la section Resources. Vous affectez ces modèles de données aux propriétés `Accent` et `Normal` que vous avez déclarées dans la classe `MyDataTemplateSelector`.

Voici un exemple des ressources XAML et du balisage nécessaire :

```xaml
<Page.Resources>

<DataTemplate x:Key="NormalItemTemplate" x:DataType="x:Int32">
    <Button HorizontalAlignment="Stretch" VerticalAlignment="Stretch" Background="{ThemeResource SystemChromeLowColor}">
        <TextBlock Text="{x:Bind}" />
    </Button>
</DataTemplate>

<DataTemplate x:Key="AccentItemTemplate" x:DataType="x:Int32">
    <Button HorizontalAlignment="Stretch" VerticalAlignment="Stretch" Background="{ThemeResource SystemAccentColor}">
        <TextBlock Text="{x:Bind}" />
    </Button>
</DataTemplate>

<l:MyDataTemplateSelector x:Key="MyDataTemplateSelector"
    Normal="{StaticResource NormalItemTemplate}"
    Accent="{StaticResource AccentItemTemplate}"/>

</Page.Resources>
```

Comme vous pouvez le voir ci-dessus, les deux modèles de données `Normal` et `Accent` sont définis : ils affichent tous les deux des éléments en tant que boutons. Toutefois, le modèle de données `Accent` utilise un pinceau de couleur d’accentuation pour l’arrière-plan, tandis que le modèle de données `Normal` utilise un pinceau de couleur gris (`SystemChromeLowColor`). Ces deux modèles de données sont ensuite affectés aux objets DataTemplate `Normal` et `Accent` qui sont des attributs de la classe MyDataTemplateSelector, créée dans le code-behind C#.

La dernière étape consiste à lier votre `DataTemplateSelector` à la propriété `ItemTemplateSelector` de votre contrôle de collection (dans ce cas, un ListView). Cela vous évite d’affecter une valeur à la propriété `ItemTemplate`. 

```xaml
<ListView x:Name = "TestListView"
          ItemsSource = "{x:Bind NumbersList}"
          ItemTemplateSelector = "{StaticResource MyDataTemplateSelector}">
</ListView>
```

Une fois votre code compilé, chaque élément de la collection passe la méthode `SelectTemplateCore` substituée dans `MyDataTemplateSelector` et est affiché avec le DataTemplate approprié.

> [!IMPORTANT]
> Quand vous utilisez `DataTemplateSelector` avec un [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater?view=winui-2.2), vous liez le `DataTemplateSelector` à la propriété `ItemTemplate`. `ItemsRepeater` n’a pas de propriété `ItemTemplateSelector`.

## <a name="datatemplateselector-performance-considerations"></a>Considérations relatives aux performances de DataTemplateSelector

Quand vous utilisez un ListView ou un GridView avec une grande collection de données, les performances associées aux défilements et aux panoramiques peuvent être un sujet de préoccupation. Pour que les grandes collections continuent de bien fonctionner, vous pouvez effectuer certaines étapes pour améliorer les performances de vos modèles de données. Celles-ci sont décrites plus en détail dans [Optimisation des options d’interface ListView et GridView](/windows/uwp/debug-test-perf/optimize-gridview-and-listview).

- _Réduction des éléments par élément_ : maintenez le nombre d’éléments d’interface utilisateur dans un modèle de données à un niveau minimum raisonnable.
- Recyclage de conteneurs avec des collections hétérogènes
  - Utilisez l’_événement ChoosingItemContainer_ : cet événement est un moyen très performant d’utiliser différents modèles de données pour différents éléments. Pour obtenir des performances optimales, vous devez optimiser la mise en cache et la sélection des modèles de données pour vos données spécifiques.
  - Utilisez un _sélecteur de modèle d’élément_ : évitez d’utiliser un sélecteur de modèle d’élément (`DataTemplateSelector`) dans certains cas en raison de son impact sur les performances.
