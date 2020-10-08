---
description: Cette rubrique catalogue tous les détails techniques impliqués dans le portage du code source dans un projet [C#](/visualstudio/get-started/csharp) vers son équivalent en [C++/WinRT](./intro-to-using-cpp-with-winrt.md).
title: Passer de C# à C++/WinRT
ms.date: 07/15/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, porter, migrer, C#
ms.localizationpriority: medium
ms.openlocfilehash: 353ca9922bc633efa5f53b2c3a3f4d7a4cad5986
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750615"
---
# <a name="move-to-cwinrt-from-c"></a>Passer de C# à C++/WinRT

Cette rubrique catalogue tous les détails techniques impliqués dans le portage du code source dans un projet [C#](/visualstudio/get-started/csharp) vers son équivalent en [C++/WinRT](./intro-to-using-cpp-with-winrt.md).

Pour une étude de cas concernant le portage de l’un des exemples d’application UWP (plateforme Windows universelle), consultez la rubrique complémentaire [Portage de l’exemple Clipboard vers C++/WinRT à partir de C#](./clipboard-to-winrt-from-csharp.md). Vous pouvez bénéficier d’une pratique et d’une expérience de portage en suivant cette procédure pas à pas et en portant l’exemple pour vous-même au fur et à mesure.

## <a name="how-to-prepare-and-what-to-expect"></a>Comment se préparer et à quoi s’attendre

L’étude de cas [Portage de l’exemple Clipboard vers C++/WinRT à partir de C#](./clipboard-to-winrt-from-csharp.md) présente les types de décisions que vous devrez prendre en matière de conception logicielle lors du portage d’un projet vers C++/WinRT. Il est donc judicieux de se préparer au portage en acquérant de solides connaissances du fonctionnement du code existant. De cette façon, vous aurez une bonne idée de la fonctionnalité de l’application et de la structure du code, et les décisions que vous prendrez vous feront toujours aller de l’avant et dans la bonne voie.

Vous pouvez regrouper les changements de portage à prévoir en quatre catégories.

- [**Porter la projection du langage**](#changes-that-involve-the-language-projection). Le Windows Runtime (WinRT) est *projeté* dans différents langages de programmation. Chacune de ces projections de langage est conçue pour être idiomatique dans le langage de programmation en question. Pour C#, certains types Windows Runtime sont projetés en tant que types .NET. Par exemple, [**System.Collections.Generic.IReadOnlyList\<T\>** ](/dotnet/api/system.collections.generic.ireadonlylist-1) sera retraduit en [**Windows.Foundation.Collections.IVectorView\<T\>** ](/uwp/api/windows.foundation.collections.ivectorview-1). Par ailleurs, en C#, certaines opérations Windows Runtime sont projetées en tant que fonctionnalités du langage C# par souci pratique. Par exemple, en C#, vous utilisez la syntaxe d’opérateur `+=` pour inscrire un délégué de gestion d’événements. Vous allez donc traduire des fonctionnalités de langage comme celle-ci vers l’opération fondamentale effectuée (dans cet exemple, l’inscription d’événements).
- [**Porter la syntaxe du langage**](#changes-that-involve-the-language-syntax). Bon nombre de ces changements sont de simples transformations mécaniques consistant à remplacer un symbole par un autre. Citons par exemple le remplacement d’un point (`.`) par deux signes deux-points (`::`).
- [**Porter la procédure de langage**](#changes-that-involve-procedures-within-the-language). Certains de ces changements peuvent être simples et répétitifs (comme remplacer `myObject.MyProperty` par `myObject.MyProperty()`). D’autres peuvent être plus approfondis (comme porter une procédure impliquant l’utilisation de **System.Text.StringBuilder** vers une autre impliquant l’utilisation de **std::wostringstream**).
- [**Tâches relatives au portage spécifiques à C++/WinRT**](#porting-related-tasks-that-are-specific-to-cwinrt). Certains détails de Windows Runtime sont pris en charge implicitement par C# en arrière-plan. Ces détails sont définis explicitement en C++/WinRT. Par exemple, vous utilisez un fichier `.idl` pour définir vos classes runtime.

Le reste de cette rubrique est structuré selon cette taxonomie.

## <a name="changes-that-involve-the-language-projection"></a>Modifications qui impliquent la projection du langage

| Category | C# | C++/WinRT | Voir aussi |
| -------- | -- | --------- | -------- |
|Objet non typé|`object` ou [**System.Object**](/dotnet/api/system.object)|[**Windows::Foundation::IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable)|[Portage de la méthode **EnableClipboardContentChangedNotifications**](./clipboard-to-winrt-from-csharp.md#enableclipboardcontentchangednotifications)|
|Espaces de noms de projection|`using System;`|`using namespace Windows::Foundation;`||
||`using System.Collections.Generic;`|`using namespace Windows::Foundation::Collections;`||
|Taille d’une collection|`collection.Count`|`collection.Size()`|[Portage de la méthode **BuildClipboardFormatsOutputString**](./clipboard-to-winrt-from-csharp.md#buildclipboardformatsoutputstring)|
|Type de collection classique|[**IList\<T\>** ](/dotnet/api/system.collections.generic.ilist-1)et **Add** pour ajouter un élément.|[**IVector\<T\>** ](/uwp/api/windows.foundation.collections.ivector-1) et **Append** pour ajouter un élément. Si vous utilisez un **std::vector** quelque part, utilisez **push_back** pour ajouter un élément.||
|Type de collection en lecture seule|[**IReadOnlyList\<T\>** ](/dotnet/api/system.collections.generic.ireadonlylist-1)|[**IVectorView\<T\>** ](/uwp/api/windows.foundation.collections.ivectorview-1)|[Portage de la méthode **BuildClipboardFormatsOutputString**](./clipboard-to-winrt-from-csharp.md#buildclipboardformatsoutputstring)|
|Délégué de gestionnaire d’événements en tant que membre de classe|`myObject.EventName += Handler;`|`token = myObject.EventName({ get_weak(), &Class::Handler });`|[Portage de la méthode **EnableClipboardContentChangedNotifications**](./clipboard-to-winrt-from-csharp.md#enableclipboardcontentchangednotifications)|
|Révoquer un délégué de gestionnaire d’événements|`myObject.EventName -= Handler;`|`myObject.EventName(token);`|[Portage de la méthode **EnableClipboardContentChangedNotifications**](./clipboard-to-winrt-from-csharp.md#enableclipboardcontentchangednotifications)|
|Conteneur associatif|[**IDictionary\<K, V\>** ](/dotnet/api/system.collections.generic.idictionary-2)|[**IMap\<K, V\>** ](/uwp/api/windows.foundation.collections.imap-2)||
|Accès aux membres d’un vecteur|`x = v[i];`<br>`v[i] = x;`|`x = v.GetAt(i);`<br>`v.SetAt(i, x);`||

### <a name="registerrevoke-an-event-handler"></a>Inscrire/révoquer un gestionnaire d’événements

En C++/WinRT, vous disposez de plusieurs options syntaxiques pour inscrire/révoquer un délégué de gestionnaire d’événements, comme décrit dans [Gérer des événements en utilisant des délégués en C++/WinRT](./handle-events.md). Consultez également [Portage de la méthode **EnableClipboardContentChangedNotifications**](./clipboard-to-winrt-from-csharp.md#enableclipboardcontentchangednotifications).

Par exemple, quand un destinataire d’événement (objet gérant un événement) est sur le point d’être détruit, vous devrez parfois révoquer un gestionnaire d’événements pour que la source de l’événement (objet déclenchant l’événement) n’effectue pas d’appel dans l’objet détruit. Consultez [Révoquer un délégué inscrit](./handle-events.md#revoke-a-registered-delegate). Dans des cas comme celui-ci, créez une variable membre **event_token** pour vos gestionnaires d’événements. Pour obtenir un exemple, consultez [Portage de la méthode **EnableClipboardContentChangedNotifications**](./clipboard-to-winrt-from-csharp.md#enableclipboardcontentchangednotifications).

Vous pouvez également inscrire un gestionnaire d’événements dans le balisage XAML.

```xaml
<Button x:Name="OpenButton" Click="OpenButton_Click" />
```

Dans C#, même si votre méthode **OpenButton_Click** est privée, XAML sera toujours en mesure de la connecter à l’événement [**ButtonBase.Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) déclenché par *OpenButton*.

En C++/WinRT, votre méthode **OpenButton_Click** doit être publique dans votre [type d’implémentation](./author-apis.md) *si vous souhaitez l’inscrire dans le balisage XAML*. Si vous enregistrez un gestionnaire d’événements uniquement en code impératif, le gestionnaire d’événements n’a pas besoin d’être public.

```cppwinrt
namespace winrt::MyProject::implementation
{
    struct MyPage : MyPageT<MyPage>
    {
        void OpenButton_Click(
            winrt::Windows:Foundation::IInspectable const& sender,
            winrt::Windows::UI::Xaml::RoutedEventArgs const& args);
    }
};
```

Vous pouvez également faire en sorte que la page XAML d’inscription soit l’amie de votre type d’implémentation et définir **OpenButton_Click** en méthode privée.

```cppwinrt
namespace winrt::MyProject::implementation
{
    struct MyPage : MyPageT<MyPage>
    {
    private:
        friend MyPageT;
        void OpenButton_Click(
            winrt::Windows:Foundation::IInspectable const& sender,
            winrt::Windows::UI::Xaml::RoutedEventArgs const& args);
    }
};
```

Dernier scénario, le projet C# que vous portez *est lié* au gestionnaire d’événements à partir du balisage (pour plus de détails sur ce scénario, consultez [Fonctions dans x:Bind](../data-binding/function-bindings.md)).

```xaml
<Button x:Name="OpenButton" Click="{x:Bind OpenButton_Click}" />
```

Vous pouvez juste remplacer ce balisage par le `Click="OpenButton_Click"` plus simple. Ou, si vous préférez, vous pouvez conserver ce balisage tel quel. Pour le prendre en charge, il vous suffit de déclarer le gestionnaire d’événements dans l’IDL.

```idl
void OpenButton_Click(Object sender, Windows.UI.Xaml.RoutedEventArgs e);
```

> [!NOTE]
> Déclarez la fonction comme `void` même si vous l’*implémentez* comme [Déclencher et oublier](./concurrency-2.md#fire-and-forget).

## <a name="changes-that-involve-the-language-syntax"></a>Modifications qui impliquent la syntaxe du langage

| Category | C# | C++/WinRT | Voir aussi |
| -------- | -- | --------- | -------- |
|Modificateurs d’accès|`public \<member\>`|`public:`<br>&nbsp;&nbsp;&nbsp;&nbsp;`\<member\>`|[Portage de la méthode **Button_Click**](./clipboard-to-winrt-from-csharp.md#button_click)|
|Accéder à un membre de données|`this.variable`|`this->variable`||
|Action asynchrone|`async Task ...`|`IAsyncAction ...`||
|Opération asynchrone|`async Task<T> ...`|`IAsyncOperation<T> ...`||
|Méthode « fire-and-forget » ou « déclencher et oublier » (implique async)|`async void ...`|`winrt::fire_and_forget ...`|[Portage de la méthode **CopyButton_Click**](./clipboard-to-winrt-from-csharp.md#copybutton_click)|
|Accéder à une constante énumérée|`E.Value`|`E::Value`|[Portage de la méthode **DisplayChangedFormats**](./clipboard-to-winrt-from-csharp.md#displaychangedformats)|
|Attente coopérative|`await ...`|`co_await ...`|[Portage de la méthode **CopyButton_Click**](./clipboard-to-winrt-from-csharp.md#copybutton_click)|
|Collection de types projetés en tant que champ privé|`private List<MyRuntimeClass> myRuntimeClasses = new List<MyRuntimeClass>();`|`std::vector`<br>`<MyNamespace::MyRuntimeClass>`<br>`m_myRuntimeClasses;`||
|Construction de GUID|`private static readonly Guid myGuid = new Guid("C380465D-2271-428C-9B83-ECEA3B4A85C1");`|`winrt::guid myGuid{ 0xC380465D, 0x2271, 0x428C, { 0x9B, 0x83, 0xEC, 0xEA, 0x3B, 0x4A, 0x85, 0xC1} };`||
|Séparateur d’espace de noms|`A.B.T`|`A::B::T`||
|Null|`null`|`nullptr`|[Portage de la méthode **UpdateStatus**](./clipboard-to-winrt-from-csharp.md#updatestatus)|
|Obtenir un objet de type|`typeof(MyType)`|`winrt::xaml_typename<MyType>()`|[Portage de la propriété **Scenarios**](./clipboard-to-winrt-from-csharp.md#scenarios)|
|Déclaration de paramètre pour une méthode|`MyType`|`MyType const&`|[Passage de paramètres](./concurrency.md#parameter-passing)|
|Déclaration de paramètre pour une méthode async|`MyType`|`MyType`|[Passage de paramètres](./concurrency.md#parameter-passing)|
|Appeler une méthode statique|`T.Method()`|`T::Method()`||
|Chaînes|`string` ou **System.String**|[**winrt::hstring**](/uw/cpp-ref-for-winrt/hstring)|[Gestion des chaînes en C++/WinRT](./strings.md)|
|Littéral de chaîne|`"a string literal"`|`L"a string literal"`|[Portage du constructeur, de **Current** et de **FEATURE_NAME**](./clipboard-to-winrt-from-csharp.md#the-constructor-current-and-feature_name)|
|Type inféré (ou déduit)|`var`|`auto`|[Portage de la méthode **BuildClipboardFormatsOutputString**](./clipboard-to-winrt-from-csharp.md#buildclipboardformatsoutputstring)|
|Directive using|`using A.B.C;`|`using namespace A::B::C;`|[Portage du constructeur, de **Current** et de **FEATURE_NAME**](./clipboard-to-winrt-from-csharp.md#the-constructor-current-and-feature_name)|
|Littéral de chaîne textuelle/brute|`@"verbatim string literal"`|`LR"(raw string literal)"`|[Portage de la méthode **DisplayToast**](./clipboard-to-winrt-from-csharp.md#displaytoast)|

> [!NOTE]
> Si un fichier d’en-tête ne contient pas de directive `using namespace` pour un espace de noms donné, vous devez qualifier complètement tous les noms de types de cet espace de noms ou au moins les qualifier suffisamment pour que le compilateur puisse les trouver. Pour obtenir un exemple, consultez [Portage de la méthode **DisplayToast**](./clipboard-to-winrt-from-csharp.md#displaytoast).

### <a name="porting-classes-and-members"></a>Portage des classes et des membres

Pour chaque type C#, vous devez décider si vous souhaitez le porter vers un type Windows Runtime ou vers une classe, un struct ou une énumération C++ normal. Pour obtenir plus d’informations et des exemples détaillés montrant comment prendre ces décisions, consultez [Portage de l’exemple Clipboard vers C++/WinRT à partir de C#](./clipboard-to-winrt-from-csharp.md).

Une propriété C# devient généralement une fonction d’accesseur, une fonction de mutateur et un membre de données de stockage. Pour obtenir plus d’informations et un exemple, consultez [Portage de la propriété **IsClipboardContentChangedEnabled**](./clipboard-to-winrt-from-csharp.md#isclipboardcontentchangedenabled).

Configurez les champs non statiques en membres de données de votre [type d’implémentation](./author-apis.md).

Un champ statique C# devient une fonction de mutateur et/ou d’accesseur statique C++/WinRT. Pour obtenir plus d’informations et un exemple, consultez [Portage du constructeur, de **Current** et de **FEATURE_NAME**](./clipboard-to-winrt-from-csharp.md#the-constructor-current-and-feature_name).

Là encore, pour chaque fonction membre, vous devez décider si elle appartient ou non à l’IDL ou si elle constitue une fonction membre publique ou privée de votre type d’implémentation. Pour obtenir plus d’informations et des exemples illustrant le processus de décision, consultez [IDL pour le type **MainPage**](./clipboard-to-winrt-from-csharp.md#idl-for-the-mainpage-type).

### <a name="porting-xaml-markup-and-asset-files"></a>Portage du balisage XAML et des fichiers de ressources

Dans le cas du [portage de l’exemple Clipboard vers C++/WinRT à partir de C#](./clipboard-to-winrt-from-csharp.md), nous avons pu utiliser un balisage XAML (ressources comprises) et des fichiers de ressources *identiques* dans l’ensemble du projet C# et C++/WinRT. Dans certains cas, des modifications du balisage sont nécessaires. Consultez [Copier le XAML et les styles nécessaires pour terminer le portage de **MainPage**](./clipboard-to-winrt-from-csharp.md#copy-the-xaml-and-styles-necessary-to-finish-up-porting-mainpage).

## <a name="changes-that-involve-procedures-within-the-language"></a>Modifications qui impliquent des procédures dans le langage

| Category | C# | C++/WinRT | Voir aussi |
| -------- | -- | --------- | -------- |
|Gestion de la durée de vie dans une méthode async|NON APPLICABLE|`auto lifetime{ get_strong() };` ou<br>`auto lifetime = get_strong();`|[Portage de la méthode **CopyButton_Click**](./clipboard-to-winrt-from-csharp.md#copybutton_click)|
|Cession|`using (var t = v)`|`auto t{ v };`<br>`t.Close(); // or let wrapper destructor do the work`|[Portage de la méthode **CopyImage**](./clipboard-to-winrt-from-csharp.md#copyimage)|
|Construire un objet|`new MyType(args)`|`MyType{ args }` ou<br>`MyType(args)`|[Portage de la propriété **Scenarios**](./clipboard-to-winrt-from-csharp.md#scenarios)|
|Créer une référence non initialisée|`MyType myObject;`|`MyType myObject{ nullptr };` ou<br>`MyType myObject = nullptr;`|[Portage du constructeur, de **Current** et de **FEATURE_NAME**](./clipboard-to-winrt-from-csharp.md#the-constructor-current-and-feature_name)|
|Construire un objet dans une variable avec des arguments|`var myObject = new MyType(args);`|`auto myObject{ MyType{ args } };` ou <br>`auto myObject{ MyType(args) };` ou <br>`auto myObject = MyType{ args };` ou <br>`auto myObject = MyType(args);` ou <br>`MyType myObject{ args };` ou <br>`MyType myObject(args);`|[Portage de la méthode **Footer_Click**](./clipboard-to-winrt-from-csharp.md#footer_click)|
|Construire un objet dans une variable sans arguments|`var myObject = new T();`|`MyType myObject;`|[Portage de la méthode **BuildClipboardFormatsOutputString**](./clipboard-to-winrt-from-csharp.md#buildclipboardformatsoutputstring)|
|Raccourci de l’initialisation d’objets|`var p = new FileOpenPicker{`<br>&nbsp;&nbsp;&nbsp;&nbsp;`ViewMode = PickerViewMode.List`<br>`};`|`FileOpenPicker p;`<br>`p.ViewMode(PickerViewMode::List);`||
|Opération de vectorisation en bloc|`var p = new FileOpenPicker{`<br>&nbsp;&nbsp;&nbsp;&nbsp;`FileTypeFilter = { ".png", ".jpg", ".gif" }`<br>`};`|`FileOpenPicker p;`<br>`p.FileTypeFilter().ReplaceAll({ L".png", L".jpg", L".gif" });`|[Portage de la méthode **CopyButton_Click**](./clipboard-to-winrt-from-csharp.md#copybutton_click)|
|Itérer au sein de la collection|`foreach (var v in c)`|`for (auto&& v : c)`|[Portage de la méthode **BuildClipboardFormatsOutputString**](./clipboard-to-winrt-from-csharp.md#buildclipboardformatsoutputstring)|
|Intercepter une exception|`catch (Exception ex)`|`catch (winrt::hresult_error const& ex)`|[Portage de la méthode **PasteButton_Click**](./clipboard-to-winrt-from-csharp.md#pastebutton_click)|
|Détails de l’exception|`ex.Message`|`ex.message()`|[Portage de la méthode **PasteButton_Click**](./clipboard-to-winrt-from-csharp.md#pastebutton_click)|
|Obtenir une valeur de propriété|`myObject.MyProperty`|`myObject.MyProperty()`|[Portage de la méthode **NotifyUser**](./clipboard-to-winrt-from-csharp.md#notifyuser)|
|Définir une valeur de propriété|`myObject.MyProperty = value;`|`myObject.MyProperty(value);`||
|Incrémenter une valeur de propriété|`myObject.MyProperty += v;`|`myObject.MyProperty(thing.Property() + v);`<br>Pour les chaînes, basculer vers un générateur||
|ToString()|`myObject.ToString()`|`winrt::to_hstring(myObject)`|[ToString()](#tostring)|
|Chaîne de langage en chaîne Windows Runtime|NON APPLICABLE|`winrt::hstring{ s }`||
|Génération de chaîne|`StringBuilder builder;`<br>`builder.Append(...);`|`std::wostringstream builder;`<br>`builder << ...;`|[Génération de chaîne](#string-building)|
|Interpolation de chaîne|`$"{i++}) {s.Title}"`|[**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring) et/ou [**winrt::hstring::operator+** ](/uwp/cpp-ref-for-winrt/hstring#operator-concatenation-operator)|[Portage de la méthode **OnNavigatedTo**](./clipboard-to-winrt-from-csharp.md#onnavigatedto)|
|Chaîne vide pour la comparaison|**System.String.Empty**|[**winrt::hstring::empty**](/uwp/cpp-ref-for-winrt/hstring#hstringempty-function)|[Portage de la méthode **UpdateStatus**](./clipboard-to-winrt-from-csharp.md#updatestatus)|
|Créer une chaîne vide|`var myEmptyString = String.Empty;`|`winrt::hstring myEmptyString{ L"" };`||
|Opérations de dictionnaire|`map[k] = v; // replaces any existing`<br>`v = map[k]; // throws if not present`<br>`map.ContainsKey(k)`|`map.Insert(k, v); // replaces any existing`<br>`v = map.Lookup(k); // throws if not present`<br>`map.HasKey(k)`||
|Conversion de type (lever en cas d’échec)|`(MyType)v`|`v.as<MyType>()`|[Portage de la méthode **Footer_Click**](./clipboard-to-winrt-from-csharp.md#footer_click)|
|Conversion de type (null en cas d’échec)|`v as MyType`|`v.try_as<MyType>()`|[Portage de la méthode **PasteButton_Click**](./clipboard-to-winrt-from-csharp.md#pastebutton_click)|
|Les éléments XAML avec x:Name sont des propriétés|`MyNamedElement`|`MyNamedElement()`|[Portage du constructeur, de **Current** et de **FEATURE_NAME**](./clipboard-to-winrt-from-csharp.md#the-constructor-current-and-feature_name)|
|Basculer vers le thread d’interface utilisateur|**CoreDispatcher.RunAsync**|**CoreDispatcher.RunAsync** ou [**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground)|[Portage de la méthode **NotifyUser**](./clipboard-to-winrt-from-csharp.md#notifyuser) et [Portage de la méthode **HistoryAndRoaming**](./clipboard-to-winrt-from-csharp.md#historyandroaming)|
|Construction d’élément d’interface utilisateur dans le code impératif d’une page XAML|Voir [Construction d’élément d’interface utilisateur](#ui-element-construction)|Voir [Construction d’élément d’interface utilisateur](#ui-element-construction)||

Les sections suivantes décrivent plus en détail certains des éléments du tableau.

### <a name="ui-element-construction"></a>Construction d’élément d’interface utilisateur

Ces exemples de code illustrent la construction d’un élément d’interface utilisateur dans le code impératif d’une page XAML.

```csharp
var myTextBlock = new TextBlock()
{
    Text = "Text",
    Style = (Windows.UI.Xaml.Style)this.Resources["MyTextBlockStyle"]
};
```

```cppwinrt
TextBlock myTextBlock;
myTextBlock.Text(L"Text");
myTextBlock.Style(
    winrt::unbox_value<Windows::UI::Xaml::Style>(
        Resources().Lookup(
            winrt::box_value(L"MyTextBlockStyle")
        )
    )
);
```

### <a name="tostring"></a>ToString()

Les types C# fournissent la méthode [Object::ToString](/dotnet/api/system.object.tostring).

```csharp
int i = 2;
var s = i.ToString(); // s is a System.String with value "2".
```

C++/WinRT ne fournit pas directement cette fonctionnalité, mais vous disposez d’alternatives.

```cppwinrt
int i{ 2 };
auto s{ std::to_wstring(i) }; // s is a std::wstring with value L"2".
```

C++/WinRT prend également en charge [**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring) pour certains types. Vous devez ajouter des surcharges pour tous les types supplémentaires que vous souhaitez convertir en chaîne.

| Language | Convertir un entier en chaîne | Convertir une valeur enum en chaîne |
| - | - | - |
| C# | `string result = "hello, " + intValue.ToString();`<br>`string result = $"hello, {intValue}";` | `string result = "status: " + status.ToString();`<br>`string result = $"status: {status}";` |
| C++/WinRT | `hstring result = L"hello, " + to_hstring(intValue);` | `// must define overload (see below)`<br>`hstring result = L"status: " + to_hstring(status);` |

Si vous souhaitez convertir une valeur enum en chaîne, vous devez fournir l’implémentation de **winrt::to_hstring**.

```cppwinrt
namespace winrt
{
    hstring to_hstring(StatusEnum status)
    {
        switch (status)
        {
        case StatusEnum::Success: return L"Success";
        case StatusEnum::AccessDenied: return L"AccessDenied";
        case StatusEnum::DisabledByPolicy: return L"DisabledByPolicy";
        default: return to_hstring(static_cast<int>(status));
        }
    }
}
```

Ces conversions en chaîne sont souvent utilisées implicitement par la liaison de données.

```xaml
<TextBlock>
You have <Run Text="{Binding FlowerCount}"/> flowers.
</TextBlock>
<TextBlock>
Most recent status is <Run Text="{x:Bind LatestOperation.Status}"/>.
</TextBlock>
```

Ces liaisons effectuent la conversion **winrt::to_hstring** de la propriété liée. Pour le deuxième exemple (**StatusEnum**), vous devez fournir votre propre surcharge de **winrt::to_hstring**. Dans le cas contraire, vous recevrez une erreur relative au compilateur.

Consultez également [Portage de la méthode **Footer_Click**](./clipboard-to-winrt-from-csharp.md#footer_click).

### <a name="string-building"></a>Génération de chaîne

Pour la génération de chaînes, C# possède un type [**StringBuilder**](/dotnet/api/system.text.stringbuilder) intégré.

| Category | C# | C++/WinRT |
| -------- | -- | --------- |
| Génération de chaîne | `StringBuilder builder;`<br>`builder.Append(...);` | `std::wostringstream builder;`<br>`builder << ...;` |
| Ajouter une chaîne Windows Runtime en préservant les valeurs null | `builder.Append(s);` | `builder << std::wstring_view{ s };` |
| Ajouter une nouvelle ligne |`builder.Append(Environment.NewLine);` | `builder << std::endl;` |
| Accéder au résultat | `s = builder.ToString();` | `ws = builder.str();` |

Consultez également [Portage de la méthode **BuildClipboardFormatsOutputString**](./clipboard-to-winrt-from-csharp.md#buildclipboardformatsoutputstring) et [Portage de la méthode **DisplayChangedFormats**](./clipboard-to-winrt-from-csharp.md#displaychangedformats).

### <a name="running-code-on-the-main-ui-thread"></a>Exécution de code sur le thread d’interface utilisateur principal 

Cet exemple est tiré de l’[exemple de scanneur de codes-barres](/samples/microsoft/windows-universal-samples/barcodescanner/).

Lorsque vous souhaitez travailler sur le thread d’interface utilisateur principal dans un projet C#, vous utilisez généralement la méthode [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync), comme ceci.

```csharp
private async void Watcher_Added(DeviceWatcher sender, DeviceInformation args)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        // Do work on the main UI thread here.
    });
}
```

Il est bien plus simple d’exprimer cela en C++/WinRT. Notez que nous acceptons les paramètres par valeur en supposant que nous allons vouloir y accéder après le premier point de suspension (`co_await`, dans le cas présent). Pour plus d’informations, consultez [Passage de paramètres](./concurrency.md#parameter-passing).

```cppwinrt
winrt::fire_and_forget Watcher_Added(DeviceWatcher sender, winrt::DeviceInformation args)
{
    co_await Dispatcher();
    // Do work on the main UI thread here.
}
```

Si vous devez effectuer le travail à une priorité différente de celle par défaut, consultez la fonction [**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground), qui a une surcharge qui est prioritaire. Pour obtenir des exemples de code montrant comment attendre un appel à **winrt::resume_foreground**, consultez [Programmation en tenant compte de l’affinité des threads](./concurrency-2.md#programming-with-thread-affinity-in-mind).

## <a name="porting-related-tasks-that-are-specific-to-cwinrt"></a>Tâches relatives au portage spécifiques à C++/WinRT

### <a name="define-your-runtime-classes-in-idl"></a>Définir vos classes runtime dans IDL

Consultez [IDL pour le type **MainPage**](./clipboard-to-winrt-from-csharp.md#idl-for-the-mainpage-type) et [Consolider vos fichiers `.idl`](./clipboard-to-winrt-from-csharp.md#consolidate-your-idl-files).

### <a name="include-the-cwinrt-windows-namespace-header-files-that-you-need"></a>Inclure les fichiers d’en-tête de l’espace de noms Windows C++/WinRT nécessaires

En C++/WinRT, chaque fois que vous voulez utiliser un type à partir d’un espace de noms Windows, vous devez inclure le fichier d’en-tête de l’espace de noms Windows C++/WinRT correspondant. Pour obtenir un exemple, consultez [Portage de la méthode **NotifyUser**](./clipboard-to-winrt-from-csharp.md#notifyuser).

### <a name="boxing-and-unboxing"></a>Boxing et unboxing

C# effectue automatiquement une conversion boxing des scalaires en objets. C++/WinRT vous oblige à appeler la fonction [**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value) de manière explicite. Les deux langages nécessitent un unboxing explicite. Consultez [Conversions boxing et unboxing avec C++/WinRT](./boxing.md).

Dans les tableaux suivants, nous allons utiliser ces définitions.

| C# | C++/WinRT|
|-|-|
| `int i;` | `int i;` |
| `string s;` | `winrt::hstring s;` |
| `object o;` | `IInspectable o;`|

| Opération | C# | C++/WinRT|
|-|-|-|
| Boxing | `o = 1;`<br>`o = "string";` | `o = box_value(1);`<br>`o = box_value(L"string");` |
| Unboxing | `i = (int)o;`<br>`s = (string)o;` | `i = unbox_value<int>(o);`<br>`s = unbox_value<winrt::hstring>(o);` |

C++/CX et C# génèrent des exceptions si vous essayez d’effectuer une conversion unboxing d’un pointeur null en un type de valeur. C++/WinRT considère qu’il s’agit d’une erreur de programmation, ce qui provoque un blocage. Dans C++/WinRT, utilisez la fonction [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) si vous souhaitez prendre en charge le cas avec l’objet qui n’est pas du type que vous pensiez.

| Scénario | C# | C++/WinRT|
|-|-|-|
| Conversion unboxing d’un entier connu |`i = (int)o;` | `i = unbox_value<int>(o);` |
| Si o est null | `System.NullReferenceException` | Se bloquer |
| Si o n’est pas un entier converti par boxing | `System.InvalidCastException` | Se bloquer |
| Conversion unboxing d’un entier, utiliser la valeur de secours si la valeur est null ; se bloquer si autre chose | `i = o != null ? (int)o : fallback;` | `i = o ? unbox_value<int>(o) : fallback;` |
| Conversion unboxing d’un entier si possible ; utiliser la valeur de secours pour toute autre chose | `i = as int? ?? fallback;` | `i = unbox_value_or<int>(o, fallback);` |

Pour obtenir un exemple, consultez [Portage de la méthode **OnNavigatedTo**](./clipboard-to-winrt-from-csharp.md#onnavigatedto) et [Portage de la méthode **Footer_Click**](./clipboard-to-winrt-from-csharp.md#footer_click).

#### <a name="boxing-and-unboxing-a-string"></a>Conversions boxing et unboxing d’une chaîne

Une chaîne est parfois un type de valeur et parfois un type de référence. C# et C++/WinRT traitent les chaînes différemment.

Le type ABI [**HSTRING**](/windows/win32/winrt/hstring) est un pointeur vers une chaîne avec décompte des références. Toutefois, il ne dérive pas à partir de [**IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable). Techniquement, il ne s’agit donc pas d’un *objet*. En outre, un pointeur **HSTRING** null représente la chaîne vide. La conversion boxing d’éléments non dérivés à partir de **IInspectable** est effectuée en les encapsulant dans une interface [**IReference\<T\>** ](/uwp/api/windows.foundation.ireference_t_). De plus, Windows Runtime fournit une implémentation standard sous la forme de l’objet [**PropertyValue**](/uwp/api/windows.foundation.propertyvalue) (les types personnalisés sont signalés sous la forme [**PropertyType::OtherType**](/uwp/api/windows.foundation.propertytype)).

C# représente une chaîne Windows Runtime sous la forme d’un type de référence, tandis que C++/WinRT projette une chaîne sous la forme d’un type de valeur. Cela signifie qu’une chaîne null convertie par boxing peut avoir des représentations différentes selon la façon dont vous l’avez obtenue.

| Comportement | C# | C++/WinRT|
|-|-|-|
| Déclarations | `object o;`<br>`string s;` | `IInspectable o;`<br>`hstring s;` |
| Catégorie de type de chaîne | Type de référence | Type de valeur |
| **HSTRING** null projette sous la forme | `""` | `hstring{}` |
| Est-ce que null et `""` sont identiques ? | Non | Oui |
| Validité de la valeur null | `s = null;`<br>`s.Length` génère NullReferenceException | `s = hstring{};`<br>`s.size() == 0` (valide) |
| Si vous affectez une chaîne null à l’objet | `o = (string)null;`<br>`o == null` | `o = box_value(hstring{});`<br>`o != nullptr` |
| Si vous affectez `""` à l’objet | `o = "";`<br>`o != null` | `o = box_value(hstring{L""});`<br>`o != nullptr` |

Boxing et unboxing de base.

| Opération | C# | C++/WinRT|
|-|-|-|
| Conversion boxing d’une chaîne | `o = s;`<br>La chaîne vide devient un objet non null. | `o = box_value(s);`<br>La chaîne vide devient un objet non null. |
| Conversion unboxing d’une chaîne connue | `s = (string)o;`<br>L’objet null devient une chaîne null.<br>InvalidCastException si ce n’est pas une chaîne. | `s = unbox_value<hstring>(o);`<br>L’objet null plante.<br>Plantage si ce n’est pas une chaîne. |
| Unboxing d’une chaîne possible | `s = o as string;`<br>L’objet null ou la non-chaîne devient une chaîne null.<br><br>OU<br><br>`s = o as string ?? fallback;`<br>Null ou non-chaîne devient la valeur de secours.<br>Chaîne vide conservée. | `s = unbox_value_or<hstring>(o, fallback);`<br>Null ou non-chaîne devient la valeur de secours.<br>Chaîne vide conservée. |

### <a name="making-a-class-available-to-the-binding-markup-extension"></a>Mise à disposition d’une classe pour l’extension de balisage {Binding}

Si vous envisagez d’utiliser l’extension de balisage {Binding} pour lier des données à votre type de données, consultez [Objet de liaison déclaré à l’aide de {Binding}](../data-binding/data-binding-in-depth.md#binding-object-declared-using-binding).

### <a name="consuming-objects-from-xaml-markup"></a>Utilisation d’objets à partir du balisage XAML

Dans un projet C#, vous pouvez utiliser des éléments nommés et des membres privés à partir du balisage XAML. Toutefois, dans C++/WinRT, toutes les entités utilisées via [**l’extension de balisage {x:Bind}** ](../xaml-platform/x-bind-markup-extension.md) XAML doivent être exposées publiquement dans IDL.

En outre, une liaison à un booléen affiche `true` ou `false` dans C#, mais affichera **Windows.Foundation.IReference`1\<Boolean\>** dans C++/WinRT.

Pour obtenir plus d’informations et d’exemples de code, consultez [Utilisation d’objets à partir du balisage](./binding-property.md#consuming-objects-from-xaml-markup).

### <a name="making-a-data-source-available-to-xaml-markup"></a>Mise à disposition d’une source de données pour le balisage XAML

Dans C++/WinRT 2.0.190530.8 et ultérieur, [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) crée un vecteur observable qui prend en charge **[IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)\<T\>** et **IObservableVector\<IInspectable\>** . Pour obtenir un exemple, consultez [Portage de la propriété **Scenarios**](./clipboard-to-winrt-from-csharp.md#scenarios).

Vous pouvez créer votre **fichier Midl (.idl)** de cette façon (consultez [Factorisation des classes runtime dans des fichiers Midl (.idl)](./author-apis.md#factoring-runtime-classes-into-midl-files-idl)).

```idl
namespace Bookstore
{
    runtimeclass BookSku { ... }

    runtimeclass BookstoreViewModel
    {
        Windows.Foundation.Collections.IObservableVector<BookSku> BookSkus{ get; };
    }

    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        BookstoreViewModel MainViewModel{ get; };
    }
}
```

Ensuite, vous l’implémentez comme suit.

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel()
    {
        m_bookSkus = winrt::single_threaded_observable_vector<Bookstore::BookSku>();
        m_bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"To Kill A Mockingbird"));
    }
    
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> BookSkus();
    {
        return m_bookSkus;
    }

private:
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> m_bookSkus;
};
...
```

Pour plus d’informations, consultez [Contrôles d’éléments XAML ; liaison à une collection C++/WinRT](./binding-collection.md) et [Collections avec C++/WinRT](./collections.md).

### <a name="making-a-data-source-available-to-xaml-markup-prior-to-cwinrt-201905308"></a>Mise à disposition d’une source de données pour le balisage XAML (version antérieure à C++/WinRT 2.0.190530.8)

La liaison de données XAML exige qu’une source d’éléments implémente **[IIterable](/uwp/api/windows.foundation.collections.iiterable_t_)\<IInspectable\>** , ainsi que l’une des combinaisons d’interfaces suivantes.

- **IObservableVector\<IInspectable\>**
- **IBindableVector** et **INotifyCollectionChanged**
- **IBindableVector** et **IBindableObservableVector**
- **IBindableVector** uniquement (ne répond pas aux modifications)
- **IVector\<IInspectable\>**
- **IBindableIterable** (effectue une itération et enregistre les éléments dans une collection privée)

Une interface générique comme **IVector\<T\>** ne peut pas être détectée au moment de l’exécution. Chaque **IVector\<T\>** a une fonction **T** qui correspond à un identificateur d’interface (IID) différent. Les développeurs peuvent développer l’ensemble de fonctions **T** de manière arbitraire. Le code de liaison XAML ne peut donc jamais connaître l’ensemble complet à interroger. Cette restriction n’est pas un problème pour C#, car chaque objet CLR implémentant **IEnumerable\<T\>T** implémente automatiquement **IEnumerable**. Au niveau de l’ABI, cela signifie que chaque objet implémentant **IObservableVector\<T\>** implémente automatiquement **IObservableVector\<IInspectable\>** .

C++/WinRT ne propose pas cette garantie. Si une classe runtime C++/WinRT implémente **IObservableVector\<T\>** , nous ne pouvons pas partir du principe qu’une implémentation de **IObservableVector\<IInspectable\>** est également fournie.

Par conséquent, voici à quoi doit ressembler l’exemple précédent.

```idl
...
runtimeclass BookstoreViewModel
{
    // This is really an observable vector of BookSku.
    Windows.Foundation.Collections.IObservableVector<Object> BookSkus{ get; };
}
```

Quant à l’implémentation :

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel()
    {
        m_bookSkus = winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>();
        m_bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"To Kill A Mockingbird"));
    }
    
    // This is really an observable vector of BookSku.
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> BookSkus();
    {
        return m_bookSkus;
    }

private:
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> m_bookSkus;
};
...
```

Si vous avez besoin d’accéder à des objets dans *m_bookSkus*, vous devez appliquer en retour une méthode QI à **Bookstore:: BookSku**.

```cppwinrt
Widget MyPage::BookstoreViewModel(winrt::hstring title)
{
    for (auto&& obj : m_bookSkus)
    {
        auto bookSku = obj.as<Bookstore::BookSku>();
        if (bookSku.Title() == title) return bookSku;
    }
    return nullptr;
}
```

### <a name="derived-classes"></a>Classes dérivées

La classe de base doit être *composable* pour être dérivée à partir d’une classe runtime. C# ne nécessite pas de procédure spéciale pour que vos classes soient composables, contrairement à C++/WinRT. Vous utilisez le [mot clé unsealed](/uwp/midl-3/intro#base-classes) pour indiquer que vous souhaitez que votre classe soit utilisable comme classe de base.

```idl
unsealed runtimeclass BasePage : Windows.UI.Xaml.Controls.Page
{
    ...
}
runtimeclass DerivedPage : BasePage
{
    ...
}
```

Dans le fichier d’en-tête de votre [type d’implémentation](./author-apis.md), vous devez ajouter le fichier d’en-tête de la classe de base avant d’inclure l’en-tête généré automatiquement pour la classe dérivée. Dans le cas contraire, vous obtiendrez des erreurs telles que « Utilisation non conforme de ce type comme expression ».

```cppwinrt
// DerivedPage.h
#include "BasePage.h"       // This comes first.
#include "DerivedPage.g.h"  // Otherwise this header file will produce an error.

namespace winrt::MyNamespace::implementation
{
    struct DerivedPage : DerivedPageT<DerivedPage>
    {
        ...
    }
}
```

## <a name="important-apis"></a>API importantes
* [Espace de noms winrt](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Rubriques connexes
* [Didacticiels C#](/visualstudio/get-started/csharp)
* [C++/WinRT](./index.md)
* [Présentation détaillée de la liaison de données](../data-binding/data-binding-in-depth.md)