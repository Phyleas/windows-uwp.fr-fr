---
description: Une propriété qui peut être efficacement liée à un contrôle XAML est appelée propriété *observable*. Cette rubrique montre comment implémenter et utiliser une propriété observable, et comment y lier un contrôle XAML.
title: Contrôles XAML ; liaison à une propriété C++/WinRT
ms.date: 09/25/2020
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, XAML, contrôle, liaison, propriété
ms.localizationpriority: medium
ms.openlocfilehash: 77155b92c126f2aae7f798c8ecd67cb255182445
ms.sourcegitcommit: bcf60b6d460dc4855f207ba21da2e42644651ef6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91376237"
---
# <a name="xaml-controls-bind-to-a-cwinrt-property"></a>Contrôles XAML ; liaison à une propriété C++/WinRT

Une propriété qui peut être efficacement liée à un contrôle XAML est appelée propriété *observable*. Ce concept est basé sur le modèle de conception logicielle appelé *modèle observateur*. Cette rubrique montre comment implémenter des propriétés observables en [C++/WinRT](./intro-to-using-cpp-with-winrt.md) et y lier des contrôles XAML (pour des informations de référence, consultez [Liaison de données](../data-binding/index.md).

> [!IMPORTANT]
> Pour obtenir les principaux concepts et termes facilitant votre compréhension pour utiliser et créer des classes runtime avec C++/WinRT, voir [Utiliser des API avec C++/WinRT](consume-apis.md) et [Créer des API avec C++/WinRT](author-apis.md).

## <a name="what-does-observable-mean-for-a-property"></a>Que signifie *observable* pour une propriété ?

Supposons qu’une classe runtime nommée **BookSku** a une propriété nommée **Title**. Si **BookSku** déclenche l’événement [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) chaque fois que la valeur de **Title** change, cela signifie que **Title** est une propriété observable. C’est le comportement de **BookSku** (déclenchement ou non de l’événement) qui détermine lesquelles de ses propriétés, le cas échéant, sont observables.

Un élément de texte XAML, ou contrôle, peut établir une liaison à ces événements, et les gérer. Un tel élément ou contrôle gère l’événement en récupérant la ou les valeurs mises à jour, puis en se mettant lui-même à jour pour afficher la nouvelle valeur.

> [!NOTE]
> Pour plus d’informations sur l’installation et l’utilisation de l’extension VSIX (Visual Studio Extension) C++/WinRT et du package NuGet (qui fournissent ensemble la prise en charge des modèles et des builds de projet), consultez [Prise en charge de Visual Studio pour C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="create-a-blank-app-bookstore"></a>Créer une application vide (Bookstore)

Commencez par créer un nouveau projet dans Microsoft Visual Studio. Créez un projet **Application vide (C++/WinRT)** et nommez-le *Bookstore*. Vérifiez que l’option **Placer la solution et le projet dans le même répertoire** n’est pas cochée. Ciblez la dernière version en disponibilité générale (autrement dit, pas la préversion) du SDK Windows.

Nous allons créer une nouvelle classe pour représenter un livre qui a une propriété de titre observable. La création et l’utilisation de la classe se feront au sein de la même unité de compilation. Mais nous voulons être en mesure d’établir une liaison à cette classe à partir de XAML, c’est pourquoi il s’agira d’une classe runtime. Et nous allons utiliser C++/WinRT à la fois pour la créer et l’utiliser.

La première étape de création d’une nouvelle classe runtime consiste à ajouter un nouvel élément **Fichier Midl (.idl)** au projet. Nommez le nouvel élément `BookSku.idl`. Supprimez le contenu par défaut de `BookSku.idl` et collez-le dans cette déclaration de classe runtime.

```idl
// BookSku.idl
namespace Bookstore
{
    runtimeclass BookSku : Windows.UI.Xaml.Data.INotifyPropertyChanged
    {
        BookSku(String title);
        String Title;
    }
}
```

> [!NOTE]
> Vos classes de modèle de vue &mdash;en fait, toute classe runtime que vous déclarez dans votre application&mdash; ne doit pas dériver d'une classe de base. La classe **BookSku** déclarée ci-dessus en est un exemple. Elle implémente une interface, mais n’est pas dérivée d’une classe de base.
>
> Toute classe runtime que vous déclarez dans l’application et qui *dérive* d’une classe de base est appelée classe *composable*. Et ces classes composables sont soumises à certaines contraintes. Pour qu’une application puisse réussir les tests du [Kit de certification des applications Windows](../debug-test-perf/windows-app-certification-kit.md) utilisés par Visual Studio et par le Microsoft Store afin de valider les soumissions (et par conséquent, pour que l’application puisse être ingérée avec succès dans le Microsoft Store), une classe composable doit finalement dériver d’une classe de base Windows. Ce qui signifie que le type de la classe à la racine de la hiérarchie d’héritage doit provenir d’un espace de noms Windows.*. Si vous devez dériver une classe runtime d’une classe de base&mdash;par exemple, pour implémenter une classe **BindableBase** afin que l’ensemble de vos modèles de vue dérivent de&mdash;, vous pouvez la dériver de [**Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject).
>
> Un modèle de vue est une abstraction d’une vue, et par conséquent, il est directement lié à la vue (le balisage XAML). Un modèle de données est une abstraction de données, et il est consommé uniquement à partir de vos modèles de vue et non directement lié à XAML. Vous pouvez donc déclarer vos modèles de données non pas comme des classes runtime, mais comme des structs ou des classes C++. Elles ne doivent être déclarées dans MIDL, et vous êtes libre d’utiliser la hiérarchie d’héritage de votre choix.

Enregistrez le fichier, puis générez le projet. La génération ne va pas encore totalement réussir, mais elle va fait des choses nécessaires pour nous. Plus particulièrement, pendant le processus de génération, l’outil `midl.exe` est exécuté pour créer un fichier de métadonnées Windows Runtime (`\Bookstore\Debug\Bookstore\Unmerged\BookSku.winmd`) décrivant la classe runtime. Puis, l’outil `cppwinrt.exe` est exécuté pour générer les fichiers de code source et vous aider à créer et utiliser votre classe runtime. Ces fichiers incluent des stubs pour vous aider à implémenter la classe runtime **BookSku** que vous avez déclarée dans votre fichier IDL. Ces stubs sont `\Bookstore\Bookstore\Generated Files\sources\BookSku.h` et `BookSku.cpp`.

Cliquez avec le bouton droit de la souris sur le nœud du projet, puis cliquez sur **Ouvrir le dossier dans l'Explorateur de fichiers**. Le dossier du projet s’ouvre dans l'Explorateur de fichiers. Copiez les fichiers stub `BookSku.h` et `BookSku.cpp` du dossier `\Bookstore\Bookstore\Generated Files\sources\` vers le dossier du projet, à savoir `\Bookstore\Bookstore\`. Dans l’**Explorateur de solutions**, avec le nœud de projet sélectionné, assurez-vous que l’option **Afficher tous les fichiers** est activée. Cliquez avec le bouton droit sur les fichiers stub que vous avez copiés, puis cliquez sur **Inclure dans le projet**.

## <a name="implement-booksku"></a>Implémenter **BookSku**
Maintenant, ouvrons `\Bookstore\Bookstore\BookSku.h` et `BookSku.cpp` et implémentons la classe runtime. Tout d’abord, vous allez voir un `static_assert` en haut de `BookSku.h` et de `BookSku.cpp`, que vous devrez supprimer.

Dans `BookSku.h`, apportez les changements suivants.

- Sur le constructeur par défaut, remplacez `= default` par `= delete`. En effet, nous ne voulons pas de constructeur par défaut.
- Ajoutez un membre privé pour stocker la chaîne de titre. Notez que nous avons un constructeur qui prend la valeur [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring). Cette valeur est la chaîne de titre (« title »).
- Ajoutez un autre membre privé pour l’événement que nous déclencherons quand le titre changera.

Après avoir apporté ces modifications, votre `BookSku.h` se présentera comme suit.

```cppwinrt
// BookSku.h
#pragma once
#include "BookSku.g.h"

namespace winrt::Bookstore::implementation
{
    struct BookSku : BookSkuT<BookSku>
    {
        BookSku() = delete;
        BookSku(winrt::hstring const& title);

        winrt::hstring Title();
        void Title(winrt::hstring const& value);
        winrt::event_token PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& value);
        void PropertyChanged(winrt::event_token const& token);
    
    private:
        winrt::hstring m_title;
        winrt::event<Windows::UI::Xaml::Data::PropertyChangedEventHandler> m_propertyChanged;
    };
}
namespace winrt::Bookstore::factory_implementation
{
    struct BookSku : BookSkuT<BookSku, implementation::BookSku>
    {
    };
}
```

Dans `BookSku.cpp`, implémentez les fonctions comme suit.

```cppwinrt
// BookSku.cpp
#include "pch.h"
#include "BookSku.h"
#include "BookSku.g.cpp"

namespace winrt::Bookstore::implementation
{
    BookSku::BookSku(winrt::hstring const& title) : m_title{ title }
    {
    }

    winrt::hstring BookSku::Title()
    {
        return m_title;
    }

    void BookSku::Title(winrt::hstring const& value)
    {
        if (m_title != value)
        {
            m_title = value;
            m_propertyChanged(*this, Windows::UI::Xaml::Data::PropertyChangedEventArgs{ L"Title" });
        }
    }

    winrt::event_token BookSku::PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& handler)
    {
        return m_propertyChanged.add(handler);
    }

    void BookSku::PropertyChanged(winrt::event_token const& token)
    {
        m_propertyChanged.remove(token);
    }
}
```

Dans la fonction mutateur **Titre**, nous vérifions si une valeur définie est différente de la valeur actuelle. Et, si tel est le cas, nous mettons à jour le titre et déclenchons aussi l’événement [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) avec un argument égal au nom de la propriété qui a changé. De cette façon, l’interface utilisateur saura quelle valeur de propriété elle doit réinterroger.

Si vous voulez le vérifier, le projet est alors à nouveau généré.

## <a name="declare-and-implement-bookstoreviewmodel"></a>Déclarer et implémenter **BookstoreViewModel**
Notre page XAML principale va établir une liaison à un modèle d’affichage principal. Et ce modèle d’affichage va avoir plusieurs propriétés, y compris une de type **BookSku**. Dans cette étape, nous allons déclarer et implémenter la classe runtime de notre modèle d’affichage principal.

Ajoutez un nouvel élément **Fichier Midl (.idl)** nommé `BookstoreViewModel.idl`. Consultez également [Factorisation des classes runtime dans des fichiers Midl (.idl)](./author-apis.md#factoring-runtime-classes-into-midl-files-idl).

```idl
// BookstoreViewModel.idl
import "BookSku.idl";

namespace Bookstore
{
    runtimeclass BookstoreViewModel
    {
        BookstoreViewModel();
        BookSku BookSku{ get; };
    }
}
```

Enregistrez et générez le projet. (La génération ne va pas encore totalement réussir, mais le but de cette génération est de générer à nouveau les fichiers stub.)

Copiez `BookstoreViewModel.h` et `BookstoreViewModel.cpp` à partir du dossier `Generated Files\sources` dans le dossier de projet, et incluez-les dans le projet. Ouvrez ces fichiers (en supprimant à nouveau `static_assert`), puis implémentez la classe runtime comme indiqué ci-dessous. Notez comment, dans `BookstoreViewModel.h`, nous incluons `BookSku.h`, qui déclare le type d’implémentation pour **BookSku** (c'est-à-dire **winrt::Bookstore::implementation::BookSku**). Et nous supprimons `= default` du constructeur par défaut.

> [!NOTE]
> Dans les listes ci-dessous pour `BookstoreViewModel.h` et `BookstoreViewModel.cpp`, le code illustre la façon par défaut de construire le membre de données *m_bookSku*. C’est la méthode qui date de la première version de C++ /WinRT, et il est conseillé d’être au moins familiarisé avec le modèle. Avec C++ /WinRT version 2.0 et les versions ultérieures, vous disposez d’une forme optimisée de construction, connue sous le nom de *construction uniforme* (consultez [Nouveautés et changements dans C++/WinRT 2.0.](./news.md#news-and-changes-in-cwinrt-20)). Plus loin dans cette rubrique, nous présenterons un exemple de construction uniforme.

```cppwinrt
// BookstoreViewModel.h
#pragma once
#include "BookstoreViewModel.g.h"
#include "BookSku.h"

namespace winrt::Bookstore::implementation
{
    struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
    {
        BookstoreViewModel();

        Bookstore::BookSku BookSku();

    private:
        Bookstore::BookSku m_bookSku{ nullptr };
    };
}
namespace winrt::Bookstore::factory_implementation
{
    struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel, implementation::BookstoreViewModel>
    {
    };
}
```

```cppwinrt
// BookstoreViewModel.cpp
#include "pch.h"
#include "BookstoreViewModel.h"
#include "BookstoreViewModel.g.cpp"

namespace winrt::Bookstore::implementation
{
    BookstoreViewModel::BookstoreViewModel()
    {
        m_bookSku = winrt::make<Bookstore::implementation::BookSku>(L"Atticus");
    }

    Bookstore::BookSku BookstoreViewModel::BookSku()
    {
        return m_bookSku;
    }
}
```

> [!NOTE]
> Le type de `m_bookSku` est le type projeté (**winrt::Bookstore::BookSku**), et le paramètre de modèle que vous utilisez avec [**winrt::make**](/uwp/cpp-ref-for-winrt/make) est le type d’implémentation (**winrt::Bookstore::implementation::BookSku**). Même dans ce cas, **make** renvoie une instance du type projeté.

Le projet est à présent à nouveau généré.

## <a name="add-a-property-of-type-bookstoreviewmodel-to-mainpage"></a>Ajouter une propriété de type **BookstoreViewModel** à **MainPage**
Ouvrez `MainPage.idl`, qui déclare la classe runtime représentant notre page d’interface utilisateur principale.

- Ajoutez une directive `import` pour importer `BookstoreViewModel.idl`.
- Ajoutez une propriété en lecture seule nommée **MainViewModel**, de type **BookstoreViewModel**.
- Supprimez la propriété **MyProperty**.

```idl
// MainPage.idl
import "BookstoreViewModel.idl";

namespace Bookstore
{
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        BookstoreViewModel MainViewModel{ get; };
    }
}
```

Enregistrez le fichier. La génération ne va pas encore totalement réussir, mais cette étape est utile car elle regénère les fichiers de code source dans lesquels la classe runtime **MainPage** est implémentée (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` et `MainPage.cpp`). Continuons et générons le projet maintenant. L’erreur de build à laquelle vous pouvez vous attendre à ce stade est **'MainViewModel': n’est pas membre de 'winrt::Bookstore::implementation::MainPage'** .

Si vous omettez l’inclure `BookstoreViewModel.idl` (voir le listing`MainPage.idl` ci-dessus), vous verrez l’erreur **attendu \< près de « MainViewModel »** . Une autre astuce consiste à vérifier que vous conservez tous les types dans le même espace de noms : l’espace de noms indiqué dans les listings de code.

Pour résoudre l’erreur que nous devrions voir, vous devez maintenant copier les stubs accesseur de la propriété **MainViewModel** à partir des fichiers générés (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` et `MainPage.cpp`) et dans `\Bookstore\Bookstore\MainPage.h` et `MainPage.cpp`. Les étapes à suivre sont décrites ci-après.

Dans `\Bookstore\Bookstore\MainPage.h`, effectuez les étapes suivantes.

- Incluez `BookstoreViewModel.h`, qui déclare le type d’implémentation pour **BookstoreViewModel** (c’est-à-dire **winrt::Bookstore::implementation::BookstoreViewModel**).
- Ajoutez un membre privé pour stocker le modèle d’affichage. Notez que la fonction d’accesseur de propriété (et le *membre m_mainViewModel*) sont implémentés en termes de type projeté pour **BookstoreViewModel** (c’est-à-dire **Bookstore::BookstoreViewModel**).
- Le type d’implémentation se trouvant dans le même projet (unité de compilation) que l’application, nous construisons *m_mainViewModel* par le biais de la surcharge du constructeur qui accepte **std::nullptr_t**.
- Supprimez la propriété **MyProperty**.

> [!NOTE]
> Dans les deux listings ci-dessous pour `MainPage.h` et `MainPage.cpp`, le code illustre la façon par défaut de construire le membre de données *m_mainViewModel*. Dans la section qui suit, nous allons montrer une version qui utilise la construction uniforme à la place.

```cppwinrt
// MainPage.h
...
#include "BookstoreViewModel.h"
...
namespace winrt::Bookstore::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        Bookstore::BookstoreViewModel MainViewModel();

        void ClickHandler(Windows::Foundation::IInspectable const&, Windows::UI::Xaml::RoutedEventArgs const&);

    private:
        Bookstore::BookstoreViewModel m_mainViewModel{ nullptr };
    };
}
...
```

Apportez les modifications suivantes à `\Bookstore\Bookstore\MainPage.cpp` en vous reportant au listing plus bas.

- Appelez [**winrt::make**](/uwp/cpp-ref-for-winrt/make) (avec le type d’implémentation **BookstoreViewModel**) pour attribuer une nouvelle instance du type projeté **BookstoreViewModel** à *m_mainViewModel*. Comme nous l’avons vu plus haut, le constructeur **BookstoreViewModel** crée un objet **BookSku** comme membre de données privé en lui attribuant initialement le titre `L"Atticus"`.
- Dans le gestionnaire d’événements du bouton (**ClickHandler**), mettez à jour le titre du livre en spécifiant le titre publié.
- Implémentez l’accesseur pour la propriété **MainViewModel**.
- Supprimez la propriété **MyProperty**.

```cppwinrt
// MainPage.cpp
#include "pch.h"
#include "MainPage.h"
#include "MainPage.g.cpp"

using namespace winrt;
using namespace Windows::UI::Xaml;

namespace winrt::Bookstore::implementation
{
    MainPage::MainPage()
    {
        m_mainViewModel = winrt::make<Bookstore::implementation::BookstoreViewModel>();
        InitializeComponent();
    }

    void MainPage::ClickHandler(Windows::Foundation::IInspectable const& /* sender */, Windows::UI::Xaml::RoutedEventArgs const& /* args */)
    {
        MainViewModel().BookSku().Title(L"To Kill a Mockingbird");
    }

    Bookstore::BookstoreViewModel MainPage::MainViewModel()
    {
        return m_mainViewModel;
    }
}
```

### <a name="uniform-construction"></a>Construction uniforme
Pour utiliser la construction uniforme au lieu de [**winrt::make**](/uwp/cpp-ref-for-winrt/make), dans `MainPage.h`, déclarez et initialisez *m_mainViewModel* en une seule étape, comme indiqué ci-dessous.

```cppwinrt
// MainPage.h
...
#include "BookstoreViewModel.h"
...
struct MainPage : MainPageT<MainPage>
{
    ...
private:
    Bookstore::BookstoreViewModel m_mainViewModel;
};
...
```

Ensuite, dans le constructeur **MainPage** utilisé dans `MainPage.cpp`, le code `m_mainViewModel = winrt::make<Bookstore::implementation::BookstoreViewModel>();` n’est pas nécessaire.

Pour obtenir plus d’informations sur la construction uniforme et des exemples de code, consultez [Accepter la construction uniforme et l’accès direct à l’implémentation](./author-apis.md#opt-in-to-uniform-construction-and-direct-implementation-access).

## <a name="bind-the-button-to-the-title-property"></a>Lier le bouton à la propriété **Title**
Ouvrez `MainPage.xaml`, qui contient le balisage XAML pour notre page d’interface utilisateur principale. Comme indiqué dans le listing ci-dessous, supprimez le nom du bouton et modifiez la valeur de sa propriété **Content** d’une chaîne littérale à une expression de liaison. Notez la propriété `Mode=OneWay` sur l’expression de liaison (à sens unique, du modèle d’affichage vers l’interface utilisateur). Sans cette propriété, l’interface utilisateur ne répondra pas aux événements de modification de propriété.

```xaml
<Button Click="ClickHandler" Content="{x:Bind MainViewModel.BookSku.Title, Mode=OneWay}"/>
```

Lancez à présent le processus de génération et exécutez le projet. Cliquez sur le bouton pour exécuter le gestionnaire d’événements **Click**. Ce gestionnaire appelle la fonction mutateur du titre du livre ; ce mutateur déclenche un événement pour informer l’interface utilisateur que la propriété **Title** a été modifiée ; et le bouton réinterroge la valeur de cette propriété pour mettre à jour sa propre valeur **Content**.

## <a name="using-the-binding-markup-extension-with-cwinrt"></a>Utilisation de l’extension de balisage {Binding} avec C++/WinRT
Pour la version actuellement publiée de C++/WinRT, pour pouvoir utiliser l’extension de balisage {Binding}, vous devrez implémenter les interfaces [ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) et [ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty).

## <a name="element-to-element-binding"></a>Liaison d’élément à élément

Vous pouvez lier la propriété d’un élément XAML à celle d’un autre élément XAML. Voici un exemple qui illustre cette opération dans le balisage.

```xaml
<TextBox x:Name="myTextBox" />
<TextBlock Text="{x:Bind myTextBox.Text, Mode=OneWay}" />
```

Vous devez déclarer l’entité XAML nommée `myTextBox` comme propriété en lecture seule dans votre fichier Midl (.idl).

```idl
// MainPage.idl
runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
{
    MainPage();
    Windows.UI.Xaml.Controls.TextBox myTextBox{ get; };
}
```

La raison est la suivante : tous les types que le compilateur XAML doit valider (notamment ceux utilisés dans [{x:Bind}](../xaml-platform/x-bind-markup-extension.md)) sont lus à partir de métadonnées Windows (WinMD). Il vous suffit donc d’ajouter la propriété en lecture seule à votre fichier Midl. Ne l’implémentez pas, car le code-behind XAML généré automatiquement fournit l’implémentation pour vous.

## <a name="consuming-objects-from-xaml-markup"></a>Utilisation d’objets à partir du balisage XAML

Toutes les entités utilisées via [**l’extension de balisage {x:Bind}** ](../xaml-platform/x-bind-markup-extension.md) XAML doivent être exposées publiquement dans IDL. De plus, si le balisage XAML contient une référence à un autre élément qui se trouve également dans un balisage, la méthode getter de ce balisage doit être présente dans IDL.

```xaml
<Page x:Name="MyPage">
    <StackPanel>
        <CheckBox x:Name="UseCustomColorCheckBox" Content="Use custom color"
             Click="UseCustomColorCheckBox_Click" />
        <Button x:Name="ChangeColorButton" Content="Change color"
            Click="{x:Bind ChangeColorButton_OnClick}"
            IsEnabled="{x:Bind UseCustomColorCheckBox.IsChecked.Value, Mode=OneWay}"/>
    </StackPanel>
</Page>
```

L’élément *ChangeColorButton* fait référence à l’élément *UseCustomColorCheckBox* via une liaison. L’IDL de cette page doit donc déclarer une propriété en lecture seule intitulée *UseCustomColorCheckBox* pour qu’elle soit accessible à la liaison.

Le délégué du gestionnaire d’événements Click pour *UseCustomColorCheckBox* utilise la syntaxe de délégué XAML classique afin de ne pas avoir besoin d’une entrée dans l’IDL. Il doit simplement être public dans votre classe d’implémentation. En revanche, *ChangeColorButton* possède également un gestionnaire d’événements Click `{x:Bind}` qui doit également être placé dans l’IDL.

```idl
runtimeclass MyPage : Windows.UI.Xaml.Controls.Page
{
    MyPage();

    // These members are consumed by binding.
    void ChangeColorButton_OnClick();
    Windows.UI.Xaml.Controls.CheckBox UseCustomColorCheckBox{ get; };
}
```

Vous n’avez pas besoin de fournir une implémentation pour la propriété **UseCustomColorCheckBox**. Le générateur de code XAML le fait pour vous.

### <a name="binding-to-boolean"></a>Liaison à un booléen

Vous pouvez effectuer cette opération en mode Diagnostic.

<syntaxhighlight lang="xml">
<TextBlock Text="{Binding CanPair}"/>
</syntaxhighlight>

`true` ou `false` s’affiche dans C++/CX, tandis que **Windows.Foundation.IReference`1<Boolean>** s’affiche dans C++/WinRT.

Utilisez `x:Bind` lors de la liaison à un booléen.

```xaml
<TextBlock Text="{x:Bind CanPair}"/>
```

## <a name="important-apis"></a>API importantes
* [INotifyPropertyChanged::PropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)
* [Modèle de fonction winrt::make](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>Rubriques connexes
* [Utiliser des API avec C++/WinRT](consume-apis.md)
* [Créer des API avec C++/WinRT](author-apis.md)