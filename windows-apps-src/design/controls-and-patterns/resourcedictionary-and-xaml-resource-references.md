---
description: Explique comment définir un élément ResourceDictionary et des ressources à clé et décrit la relation entre les ressources XAML et les autres ressources que vous définissez dans le cadre de votre application ou de votre package d’application.
MS-HAID: dev\_ctrl\_layout\_txt.resourcedictionary\_and\_xaml\_resource\_references
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Références aux ressources ResourceDictionary et XAML
ms.assetid: E3CBFA3D-6AF5-44E1-B9F9-C3D3EA8A25CE
label: ResourceDictionary and XAML resource references
template: detail.hbs
ms.date: 09/24/2020
ms.topic: conceptual
ms.custom: contperf-fy21q1
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: 93d2c7e3381315d370969b7d5789e92b649012b4
ms.sourcegitcommit: c0da06081d6b9a0386e61facdf68b28f606367b2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2021
ms.locfileid: "98577551"
---
# <a name="resourcedictionary-and-xaml-resource-references"></a>Références aux ressources ResourceDictionary et XAML

 

Vous pouvez définir l’interface utilisateur ou les ressources de votre application avec XAML. Les ressources constituent généralement les définitions d’un objet que vous comptez utiliser plusieurs fois. Pour faire référence à une ressource XAML ultérieurement, vous spécifiez la clé d’une ressource qui agit comme son nom. Vous pouvez faire référence à une ressource dans toute l’application ou à partir d’une page XAML à l’intérieur de celle-ci. Vous pouvez définir vos ressources en utilisant un élément [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary) à partir de XAML Windows Runtime. Vous pouvez alors faire référence à vos ressources en utilisant une [extension de balisage StaticResource](../../xaml-platform/staticresource-markup-extension.md) ou une [extension de balisage ThemeResource](../../xaml-platform/themeresource-markup-extension.md).

Les éléments XAML que vous souhaiterez sans doute déclarer le plus souvent comme ressources XAML sont [Style](/uwp/api/Windows.UI.Xaml.Style), [ControlTemplate](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate), les composants d’animation et les sous-classes [Brush](/uwp/api/Windows.UI.Xaml.Media.Brush). Nous vous expliquons ici comment définir un élément [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary) et des ressources à clé, ainsi que la relation entre les ressources XAML et les autres ressources que vous définissez dans le cadre de votre application ou de votre package d’application. Nous décrivons également des fonctionnalités avancées du dictionnaire de ressources, par exemple [MergedDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.mergeddictionaries) et [ThemeDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries).

**Conditions préalables**

Nous supposons que vous comprenez le balisage XAML et que vous avez lu la rubrique [Vue d’ensemble du langage XAML](../../xaml-platform/xaml-overview.md).

## <a name="define-and-use-xaml-resources"></a>Définir et utiliser des ressources XAML

Les ressources XAML sont des objets référencés plusieurs fois à partir du balisage. Les ressources sont définies dans un composant [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary), généralement dans un fichier séparé ou dans la partie supérieure de la page de balisage, comme ceci.

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <Page.Resources>
        <x:String x:Key="greeting">Hello world</x:String>
        <x:String x:Key="goodbye">Goodbye world</x:String>
    </Page.Resources>

    <TextBlock Text="{StaticResource greeting}" Foreground="Gray" VerticalAlignment="Center"/>
</Page>
```

Dans cet exemple :

-   `<Page.Resources>…</Page.Resources>` - Définit le dictionnaire de ressources.
-   `<x:String>` - Définit la ressource avec la clé « greeting ».
-   `{StaticResource greeting}`- Recherche la ressource avec la clé « greeting », qui est affectée à la propriété [Text](/uwp/api/windows.ui.xaml.controls.textblock.text) du composant [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock).

> **Remarque**&nbsp;&nbsp;Ne confondez pas les concepts liés à [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary) et l’action de build **Resource**, les fichiers de ressources (.resw) ou d’autres « ressources » décrits dans le contexte de la structuration du projet de code qui produit votre package d’application.

Les ressources ne sont pas forcément des chaînes. Il peut s’agir de tout objet partageable, comme des styles, des modèles, des pinceaux et des couleurs. Toutefois, les contrôles, les formes et autres [FrameworkElement](/uwp/api/Windows.UI.Xaml.FrameworkElement), qui ne peuvent pas être partagés, ne peuvent pas être marqués comme ressources réutilisables. Pour plus d’informations sur le partage, voir la section [Les ressources XAML doivent pouvoir être partagées.](#xaml-resources-must-be-shareable), plus bas dans cette rubrique.

Ici, un pinceau et une chaîne sont marqués en tant que ressources et utilisés par les contrôles dans une page.

```XAML
<Page
    x:Class="SpiderMSDN.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <Page.Resources>
        <SolidColorBrush x:Key="myFavoriteColor" Color="green"/>
        <x:String x:Key="greeting">Hello world</x:String>
    </Page.Resources>

    <TextBlock Foreground="{StaticResource myFavoriteColor}" Text="{StaticResource greeting}" VerticalAlignment="Top"/>
    <Button Foreground="{StaticResource myFavoriteColor}" Content="{StaticResource greeting}" VerticalAlignment="Center"/>
</Page>
```

Toutes les ressources doivent présenter une clé. Généralement, cette clé est une chaîne définie avec `x:Key="myString"`. Toutefois, il existe d’autres moyens de spécifier une clé :

-   [Style](/uwp/api/Windows.UI.Xaml.Style) et [ControlTemplate](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) nécessitent un composant **TargetType**, et utilisent le composant **TargetType** en tant que clé si [x:Key](../../xaml-platform/x-key-attribute.md) n’est pas spécifié. Dans ce cas, la clé est l’objet réel Type, pas une chaîne. (Voir les exemples ci-dessous)
-   Les ressources [DataTemplate](/uwp/api/Windows.UI.Xaml.DataTemplate) qui ont un composant **TargetType** utilisent **TargetType** en tant que clé si [x:Key](../../xaml-platform/x-key-attribute.md) n’est pas spécifié. Dans ce cas, la clé est l’objet réel Type, pas une chaîne.
-   [x:Name](../../xaml-platform/x-name-attribute.md) peut être utilisé en lieu et place de [x:Key](../../xaml-platform/x-key-attribute.md). Toutefois, x:Name génère également un champ code-behind pour la ressource. En conséquence, x:Name est moins efficace que x:Key, car ce champ doit être initialisé lors du chargement de la page.

L’ [extension de balisage StaticResource](../../xaml-platform/staticresource-markup-extension.md) peut récupérer des ressources uniquement avec un nom de chaîne ([x:Key](../../xaml-platform/x-key-attribute.md) ou [x:Name](../../xaml-platform/x-name-attribute.md)). Cependant, l’infrastructure XAML recherche également des ressources de style implicites (celles utilisant **TargetType** plutôt que x:Key ou x:Name) quand elle détermine le style et le modèle à utiliser pour un contrôle qui n’a pas défini les propriétés [Style](/uwp/api/windows.ui.xaml.frameworkelement.style) et [ContentTemplate](/uwp/api/windows.ui.xaml.controls.contentcontrol.contenttemplate) ou [ItemTemplate](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate).

Ici, le composant [Style](/uwp/api/Windows.UI.Xaml.Style) a une clé implicite de **typeof(Button)** et, dans la mesure où l’élément [Button](/uwp/api/Windows.UI.Xaml.Controls.Button) situé dans la partie inférieure de la page ne spécifie pas de propriété [Style](/uwp/api/windows.ui.xaml.frameworkelement.style), il recherche un style avec une clé de **typeof(Button)**  :

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <Page.Resources>
        <Style TargetType="Button">
            <Setter Property="Background" Value="Red"/>
        </Style>
    </Page.Resources>
    <Grid>
       <!-- This button will have a red background. -->
       <Button Content="Button" Height="100" VerticalAlignment="Center" Width="100"/>
    </Grid>
</Page>
```

Pour en savoir plus sur les styles implicites et sur leur fonctionnement, voir [Contrôles de style](xaml-styles.md) et [Modèles de contrôle](control-templates.md).

## <a name="look-up-resources-in-code"></a>Rechercher des ressources dans le code

Vous accédez aux membres du dictionnaire de ressources de la manière dont vous accédez aux membres des autres dictionnaires.

> [!WARNING]
> Quand vous effectuez une recherche de ressource dans le code, seules les ressources dans le dictionnaire `Page.Resources` sont examinées. Contrairement à l’[extension de balisage StaticResource](../../xaml-platform/staticresource-markup-extension.md), le code n’est pas reporté sur le dictionnaire `Application.Resources` si les ressources ne sont pas trouvées dans le premier dictionnaire.

 

Cet exemple montre comment récupérer la ressource `redButtonStyle` du dictionnaire de ressources d’une page :

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <Page.Resources>
        <Style TargetType="Button" x:Key="redButtonStyle">
            <Setter Property="Background" Value="red"/>
        </Style>
    </Page.Resources>
</Page>
```

```csharp
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            Style redButtonStyle = (Style)this.Resources["redButtonStyle"];
        }
    }
```
```cppwinrt
    MainPage::MainPage()
    {
        InitializeComponent();
        Windows::UI::Xaml::Style style = Resources().TryLookup(winrt::box_value(L"redButtonStyle")).as<Windows::UI::Xaml::Style>();
    }
```

Pour rechercher les ressources applicatives du code, utilisez **Application.Current.Resources** pour obtenir le dictionnaire de ressources de l’application, comme représenté ici.

```XAML
<Application
    x:Class="MSDNSample.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:SpiderMSDN">
    <Application.Resources>
        <Style TargetType="Button" x:Key="appButtonStyle">
            <Setter Property="Background" Value="red"/>
        </Style>
    </Application.Resources>

</Application>
```

```csharp
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            Style appButtonStyle = (Style)Application.Current.Resources["appButtonStyle"];
        }
    }
```
```cppwinrt
    MainPage::MainPage()
    {
        InitializeComponent();
        Windows::UI::Xaml::Style style = Application::Current().Resources()
                                                               .TryLookup(winrt::box_value(L"appButtonStyle"))
                                                               .as<Windows::UI::Xaml::Style>();
    }
```

Vous pouvez également ajouter une ressource d’application dans le code.

Voici deux éléments à prendre en considération lors de cette opération.

-   Tout d’abord, vous devez ajouter les ressources avant qu’une page n’essaie de les utiliser.
-   Ensuite, vous ne pouvez pas ajouter les ressources dans le constructeur de l’application.

Pour contourner ces deux problèmes, ajoutez la ressource dans la méthode [Application.OnLaunched](/uwp/api/windows.ui.xaml.application.onlaunched), comme suit.

```csharp
// App.xaml.cs
    
sealed partial class App : Application
{
    protected override void OnLaunched(LaunchActivatedEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;
        if (rootFrame == null)
        {
            SolidColorBrush brush = new SolidColorBrush(Windows.UI.Color.FromArgb(255, 0, 255, 0)); // green
            this.Resources["brush"] = brush;
            // … Other code that VS generates for you …
        }
    }
}
```
```cppwinrt
// App.cpp

void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    Frame rootFrame{ nullptr };
    auto content = Window::Current().Content();
    if (content)
    {
        rootFrame = content.try_as<Frame>();
    }

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        Windows::UI::Xaml::Media::SolidColorBrush brush{ Windows::UI::ColorHelper::FromArgb(255, 0, 255, 0) };
        Resources().Insert(winrt::box_value(L"brush"), winrt::box_value(brush));
        // … Other code that VS generates for you …
```

## <a name="every-frameworkelement-can-have-a-resourcedictionary"></a>Chaque élément FrameworkElement peut présenter un élément ResourceDictionary associé.

[FrameworkElement](/uwp/api/Windows.UI.Xaml.FrameworkElement) est une classe de base dont sont dérivés les contrôles, et qui présente une propriété [Resources](/uwp/api/windows.ui.xaml.frameworkelement.resources). Ainsi, vous pouvez ajouter un dictionnaire de ressources locales à tout élément **FrameworkElement**.

Ici, les éléments [Page](/uwp/api/Windows.UI.Xaml.Controls.Page) et [Border](/uwp/api/Windows.UI.Xaml.Controls.Border) possèdent des dictionnaires de ressources et présentent une ressource appelée « greeting ». L’élément [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) nommé « textBlock2 » est à l’intérieur de l’élément **Border** : sa recherche de ressources est donc exécutée dans un premier temps dans les ressources de **Border**, puis dans les ressources de **Page** et enfin dans les ressources de [Application](/uwp/api/Windows.UI.Xaml.Application). L’élément **TextBlock** lira « Hola mundo ».

Pour accéder aux ressources de cet élément à partir du code, utilisez la propriété [Resources](/uwp/api/windows.ui.xaml.frameworkelement.resources) de cet élément. Si vous accédez à une ressource de [FrameworkElement](/uwp/api/Windows.UI.Xaml.FrameworkElement) dans le code, plutôt que dans XAML, la recherche sera exécutée dans ce dictionnaire, pas dans les dictionnaires de l’élément parent.

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Page.Resources>
        <x:String x:Key="greeting">Hello world</x:String>
    </Page.Resources>
    
    <StackPanel>
        <!-- Displays "Hello world" -->
        <TextBlock x:Name="textBlock1" Text="{StaticResource greeting}"/>

        <Border x:Name="border">
            <Border.Resources>
                <x:String x:Key="greeting">Hola mundo</x:String>
            </Border.Resources>
            <!-- Displays "Hola mundo" -->
            <TextBlock x:Name="textBlock2" Text="{StaticResource greeting}"/>
        </Border>

        <!-- Displays "Hola mundo", set in code. -->
        <TextBlock x:Name="textBlock3"/>
    </StackPanel>
</Page>

```

```csharp
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            textBlock3.Text = (string)border.Resources["greeting"];
        }
    }
```
```cppwinrt
    MainPage::MainPage()
    {
        InitializeComponent();
        textBlock3().Text(unbox_value<hstring>(border().Resources().TryLookup(winrt::box_value(L"greeting"))));
    }
```

## <a name="merged-resource-dictionaries"></a>Dictionnaires de ressources fusionnés

Un *dictionnaire de ressources fusionné* combine un dictionnaire de ressources avec un autre, généralement dans un autre fichier.

> **Conseil**&nbsp;&nbsp;Pour créer un fichier de dictionnaire de ressources dans Microsoft Visual Studio, utilisez l’option **Ajouter&gt; Nouvel élément… &gt; Dictionnaire de ressources** du menu **Projet**.

Ici, vous définissez un dictionnaire de ressources dans un fichier XAML distinct appelé Dictionary1.xaml.

```XAML
<!-- Dictionary1.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MSDNSample">

    <SolidColorBrush x:Key="brush" Color="Red"/>

</ResourceDictionary>

```

Pour utiliser ce dictionnaire, vous le fusionnez avec le dictionnaire de votre page :

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Page.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="Dictionary1.xaml"/>
            </ResourceDictionary.MergedDictionaries>

            <x:String x:Key="greeting">Hello world</x:String>

        </ResourceDictionary>
    </Page.Resources>

    <TextBlock Foreground="{StaticResource brush}" Text="{StaticResource greeting}" VerticalAlignment="Center"/>
</Page>
```

Voici ce qui se passe dans cet exemple. Dans `<Page.Resources>`, vous déclarez `<ResourceDictionary>`. L’infrastructure XAML crée de manière implicite un dictionnaire de ressources lorsque vous ajoutez des ressources dans `<Page.Resources>`. Néanmoins, vous n’avez pas besoin ici d’un dictionnaire quelconque, il vous faut absolument un composant qui contient des dictionnaires fusionnés.

Aussi, vous déclarez `<ResourceDictionary>`, puis ajoutez des éléments à sa collection `<ResourceDictionary.MergedDictionaries>`. Chacune de ces entrées prend la forme `<ResourceDictionary Source="Dictionary1.xaml"/>`. Pour ajouter plusieurs dictionnaires, ajoutez simplement une entrée `<ResourceDictionary Source="Dictionary2.xaml"/>` après la première entrée.

Après `<ResourceDictionary.MergedDictionaries>…</ResourceDictionary.MergedDictionaries>`, vous pouvez éventuellement placer des ressources supplémentaires dans votre dictionnaire principal. Les ressources d’un dictionnaire fusionné s’utilisent comme celles d’un dictionnaire standard. Dans l’exemple ci-dessus, `{StaticResource brush}` trouve la ressource dans le dictionnaire enfant/fusionné (Dictionary1.xaml), tandis que `{StaticResource greeting}` trouve sa ressource dans le dictionnaire de la page principale.

Dans la séquence de recherche de ressource, un dictionnaire [MergedDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.mergeddictionaries) est consulté uniquement après que toutes les autres ressources indexées de ce [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary) ont été consultées. Après avoir effectué une recherche dans ce niveau, les dictionnaires fusionnés sont atteints et chaque élément inclus dans le **MergedDictionaries** est consulté. S’il existe plusieurs dictionnaires fusionnés, ils sont consultés dans l’ordre inverse de celui dans lequel ils sont déclarés dans la propriété **MergedDictionaries**. Dans l’exemple suivant, si Dictionary2.xaml et Dictionary1.xaml ont déclaré une clé identique, la clé de Dictionary2.xaml est utilisée en premier, car elle termine l’ensemble **MergedDictionaries**.

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Page.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="Dictionary1.xaml"/>
                <ResourceDictionary Source="Dictionary2.xaml"/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Page.Resources>

    <TextBlock Foreground="{StaticResource brush}" Text="greetings!" VerticalAlignment="Center"/>
</Page>
```

Dans l’étendue d’un [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary), le dictionnaire est consulté afin de vérifier le caractère unique de la clé. Toutefois, cette étendue ne s’étend pas à différents éléments dans des fichiers [MergedDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.mergeddictionaries) différents.

Vous pouvez utiliser la combinaison de séquence de recherche et de manque d’exigence de caractère unique de clé parmi les étendues de dictionnaire fusionné pour créer une séquence de valeurs de secours des ressources [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary). Par exemple, vous pouvez stocker les préférences utilisateur d’une couleur de pinceau particulière dans le dernier dictionnaire de ressources fusionné dans la séquence, à l’aide d’un dictionnaire de ressources qui se synchronise avec l’état de votre application et les données des préférences utilisateur. Cependant, s’il n’existe encore aucune préférence utilisateur, vous pouvez définir cette même chaîne de clé pour une ressource **ResourceDictionary** dans le fichier [MergedDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.mergeddictionaries) initial et elle peut servir de valeur de secours. N’oubliez pas que toute valeur que vous fournissez dans un dictionnaire de ressources principal est toujours consultée avant les dictionnaires fusionnés. Par conséquent, si vous voulez appliquer la technique ayant recours à la valeur de secours, ne définissez pas cette ressource dans un dictionnaire de ressources principal.

## <a name="theme-resources-and-theme-dictionaries"></a>Ressources de thème et dictionnaires de thèmes

Un élément [ThemeResource](../../xaml-platform/themeresource-markup-extension.md) est similaire à un élément [StaticResource](../../xaml-platform/staticresource-markup-extension.md), mais la recherche de ressources est réévaluée lors de toute modification du thème.

Dans cet exemple, vous définissez le premier plan d’un élément [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) sur une valeur du thème actuel.

```XAML
<TextBlock Text="hello world" Foreground="{ThemeResource FocusVisualWhiteStrokeThemeBrush}" VerticalAlignment="Center"/>
```

Un dictionnaire de thème est un type spécifique de dictionnaire fusionné qui conserve les ressources variant en fonction du thème exécuté par l’utilisateur sur son appareil. Par exemple, le thème « clair » peut utiliser un pinceau de couleur blanche alors que le thème par défaut (« sombre ») peut utiliser un pinceau de couleur sombre. Le pinceau change la ressource dans laquelle il est résolu, mais autrement la composition d’un contrôle qui utilise le pinceau comme ressource pourrait être identique. Pour reproduire le comportement de basculement de thème dans vos propres modèles et styles, plutôt que d’utiliser [MergedDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.mergeddictionaries) en tant que propriété pour fusionner des éléments dans les principaux dictionnaires, utilisez la propriété [ThemeDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries).

Chaque élément [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary) dans [ThemeDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries) doit avoir une valeur [x:Key](../../xaml-platform/x-key-attribute.md). La valeur est une chaîne qui nomme le thème pertinent : par exemple, « Défaut », « Sombre », « Clair » ou « ContrasteÉlevé ». Généralement, `Dictionary1` et `Dictionary2` définissent les ressources qui ont des noms identiques, mais des valeurs différentes.

Ici, vous utilisez du texte rouge pour le thème clair et du texte bleu pour le thème sombre.

```XAML
<!-- Dictionary1.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MSDNSample">

    <SolidColorBrush x:Key="brush" Color="Red"/>

</ResourceDictionary>

<!-- Dictionary2.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MSDNSample">

    <SolidColorBrush x:Key="brush" Color="blue"/>

</ResourceDictionary>
```

Dans cet exemple, vous définissez le premier plan d’un élément [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) sur une valeur du thème actuel.

```XAML
<Page
    x:Class="MSDNSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Page.Resources>
        <ResourceDictionary>
            <ResourceDictionary.ThemeDictionaries>
                <ResourceDictionary Source="Dictionary1.xaml" x:Key="Light"/>
                <ResourceDictionary Source="Dictionary2.xaml" x:Key="Dark"/>
            </ResourceDictionary.ThemeDictionaries>
        </ResourceDictionary>
    </Page.Resources>
    <TextBlock Foreground="{StaticResource brush}" Text="hello world" VerticalAlignment="Center"/>
</Page>
```

Pour les dictionnaires de thèmes, le dictionnaire actif à utiliser pour la recherche de ressource change dynamiquement, chaque fois que l’[extension de balisage ThemeResource](../../xaml-platform/themeresource-markup-extension.md) est utilisée pour l’établissement de la référence et que le système détecte un changement de thème. Le comportement de recherche effectué par le système est généralement basé sur le mappage du thème actif sur l’élément [x:Key](../../xaml-platform/x-key-attribute.md) d’un dictionnaire de thèmes spécifique.

Il peut s’avérer utile d’examiner la manière dont les dictionnaires de thèmes sont structurés dans les ressources de conception XAML par défaut, équivalant aux modèles que Windows Runtime utilise par défaut pour ses contrôles. Ouvrez les fichiers XAML dans \\(Program Files)\\Windows Kits\\10\\DesignTime\\CommonConfiguration\\Neutral\\UAP\\&lt;Version du SDK&gt;\\Generic en utilisant un éditeur de texte ou votre IDE. Notez la manière dont les dictionnaires de thèmes sont définis tout d’abord dans generic.xaml et la manière dont chaque dictionnaire de thèmes définit les mêmes clés. Chacune de ces clés est ensuite référencée par des éléments de composition dans les différents éléments indexés qui se trouvent en dehors des dictionnaires de thèmes et définie ultérieurement dans le code XAML. Il existe aussi un fichier themeresources.xaml distinct, utilisé à des fins de conception, qui contient uniquement les ressources de thème et des modèles supplémentaires (il ne contient pas les modèles de contrôle par défaut). Les zones de thème sont des copies de ce que vous pourriez voir dans generic.xaml.

Quand vous utilisez des outils de conception XAML pour modifier des copies de styles et de modèles, ces outils de conception extraient des sections des dictionnaires de ressource de conception XAML et les placent sous forme de copies locales d’éléments de dictionnaire XAML faisant partie de votre application et projet.

Pour plus d’informations et pour consulter une liste des ressources système et propres aux thèmes accessibles à une application, consultez la rubrique [Ressources de thème XAML](xaml-theme-resources.md).

## <a name="lookup-behavior-for-xaml-resource-references"></a>Comportement de recherche pour les références aux ressources XAML

*Comportement de recherche* est le terme qui sert à décrire la façon dont le système de ressources XAML essaie de trouver une ressource XAML. La recherche se produit lorsqu’une clé est référencée en tant que référence de ressource XAML à partir d’un endroit quelconque du code XAML de l’application. Le comportement du système de ressources est tout d’abord prévisible en ce qui concerne l’emplacement où il va vérifier l’existence d’une ressource selon l’étendue. Si une ressource est introuvable dans l’étendue initiale, celle-ci est développée. Le comportement de recherche se poursuit dans l’ensemble des emplacements et étendues où une ressource XAML peut éventuellement être définie par une application ou le système. Si toutes les tentatives de recherche de ressources possibles échouent, il en résulte souvent une erreur. Il est généralement possible d’éliminer ces erreurs pendant le processus de développement.

Le comportement de recherche pour les références aux ressources XAML commence avec l’objet où l’utilisation réelle est appliquée et sa propre propriété [Resources](/uwp/api/windows.ui.xaml.frameworkelement.resources). S’il existe un [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary) à cet endroit, ce **ResourceDictionary** est consulté afin de vérifier s’il contient un élément avec la clé demandée. Le premier niveau de recherche est rarement pertinent, car en règle générale on ne définit pas et ne fait pas référence à une ressource sur le même objet. En fait, il est rare qu’il existe une propriété **Resources** à cet endroit. Vous pouvez effectuer des références aux ressources XAML à partir de presque n’importe où en XAML ; vous n’êtes pas limité aux propriétés des sous-classes [FrameworkElement](/uwp/api/Windows.UI.Xaml.FrameworkElement).

La séquence de recherche vérifie ensuite l’objet parent suivant dans l’arborescence d’objets d’exécution de l’application. S’il existe un [FrameworkElement.Resources](/uwp/api/windows.ui.xaml.frameworkelement.resources) et qu’il contient un [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary), l’élément dictionnaire avec la chaîne de clé spécifiée est demandé. Si la ressource est trouvée, la séquence de recherche s’arrête et l’objet est fourni à l’emplacement où la référence a été effectuée. Sinon, le comportement de recherche passe au niveau parent suivant vers la racine de l’arborescence d’objets. La recherche continue de manière récursive vers le haut jusqu’à atteindre l’élément racine du XAML, tous les emplacements de ressources immédiates possibles étant consultés au passage.

> **Remarque**&nbsp;&nbsp;Il est courant de définir toutes les ressources immédiates au niveau racine d’une page, à la fois pour tirer parti de ce comportement de recherche de ressource et par convention du style de balisage XAML.

 

Si la ressource demandée est introuvable dans les ressources immédiates, l’étape de recherche suivante consiste à consulter la propriété [Application.Resources](/uwp/api/windows.ui.xaml.application.resources). **Application.Resources** est l’endroit idéal où placer des ressources spécifiques à une application qui sont référencées par plusieurs pages dans la structure de navigation de votre application.

Les modèles de contrôle ont un autre emplacement possible dans la recherche de référence : les dictionnaires de thèmes. Un dictionnaire de thème est un fichier XAML qui possède un élément [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary) en guise de racine. Le dictionnaire de thème peut être un dictionnaire fusionné à partir de [Application.Resources](/uwp/api/windows.ui.xaml.application.resources). Il peut également s’agir du dictionnaire de thème spécifique à un contrôle pour un contrôle personnalisé basé sur un modèle.

Pour finir, il existe une recherche de ressource ayant pour objet des ressources de plateforme. Parmi les ressources de plateforme figurent les modèles de contrôles définis pour chacun des thèmes d’interface utilisateur du système et qui définissent l’apparence par défaut de tous les contrôles que vous utilisez pour l’interface utilisateur dans une application Windows Runtime. Les ressources de plateforme comprennent également un ensemble de ressources nommées liées à l’apparence et aux thèmes utilisés dans tout le système. Ces ressources constituent techniquement un élément [MergedDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.mergeddictionaries) et sont donc disponibles pour la recherche à partir du XAML ou du code une fois l’application chargée. Par exemple, les ressources de thème système incluent une ressource nommée « SystemColorWindowTextColor » qui offre une définition [Color](/uwp/api/Windows.UI.Color) pour faire correspondre la couleur du texte de l’application à la couleur du texte d’une fenêtre système provenant du système d’exploitation et des préférences utilisateur. D’autres styles XAML destinés à votre application peuvent faire référence à ce style, ou votre code peut se procurer une valeur de recherche de ressource (et la transtyper en valeur **Color** dans le cas de l’exemple).

Pour obtenir plus d’informations et une liste des ressources système et propres aux thèmes accessibles à une application Windows en XAML, consultez [Ressources de thème XAML](xaml-theme-resources.md).

Si la clé demandée reste introuvable dans ces emplacements, une exception/erreur d’analyse XAML se produit. Dans certains cas, il peut s’agir d’une exception d’exécution non détectée par une action de compilation de balisage XAML, ni par un environnement de conception XAML.

En raison du comportement de recherche à plusieurs niveaux applicable aux dictionnaires de ressources, vous pouvez délibérément définir plusieurs éléments de ressources ayant chacun la même valeur de chaîne comme clé, dès l’instant où chaque ressource est définie à un niveau différent. Autrement dit, même si les clés doivent être uniques dans une classe [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary) donnée, cette exigence ne s’étend pas à la séquence de comportement de recherche dans son ensemble. Pendant la recherche, seul le premier objet correctement récupéré est utilisé pour la référence de ressource XAML, puis la recherche s’arrête. Vous pourriez utiliser ce comportement pour demander la même ressource XAML par une clé à différents niveaux dans le code XAML de votre application et obtenir en retour différentes ressources, en fonction de l’étendue à partir de laquelle la référence de ressource XAML a été effectuée et du comportement spécifique de cette recherche.

##  <a name="forward-references-within-a-resourcedictionary"></a>Références anticipées dans un ResourceDictionary


Les références aux ressources XAML dans un dictionnaire de ressources particulier doivent faire référence à une ressource ayant déjà été définie avec une clé, et cette ressource doit, d’un point de vue lexical, apparaître avant la référence à la ressource. Les références anticipées ne peuvent pas être résolues par une référence de ressource XAML. Pour cette raison, si vous utilisez des références de ressources XAML à partir d’une autre ressource, vous devez concevoir votre structure de dictionnaire de ressources de telle sorte que les ressources utilisées par d’autres ressources soient d’abord définies dans un dictionnaire de ressources.

Les ressources définies au niveau de l’application ne peuvent pas faire référence à des ressources immédiates. Ceci équivaut à essayer d’effectuer une référence anticipée, car les ressources d’application sont en fait traitées en premier (au premier démarrage de l’application et avant le chargement du contenu de navigation de page). Néanmoins, toute ressource immédiate pouvant faire référence à une ressource d’application, cette technique peut s’avérer utile pour éviter les situations de référence anticipée.

## <a name="xaml-resources-must-be-shareable"></a>Les ressources XAML doivent pouvoir être partagées.


Pour qu’un objet existe dans un [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary), il doit être *partageable*.

Ceci est obligatoire car, lorsque l’arborescence d’objets d’une application est construite et utilisée au moment de l’exécution, les objets ne peuvent pas exister à plusieurs emplacements dans l’arborescence. En interne, le système de ressources crée des copies des valeurs de ressources à utiliser dans le graphique d’objet de votre application quand chaque ressource XAML est demandée.

Une classe [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary) et Windows Runtime XAML en général prennent en charge ces objets pour une utilisation partageable :

-   Styles et modèles ([Style](/uwp/api/Windows.UI.Xaml.Style) et classes dérivées de [FrameworkTemplate](/uwp/api/Windows.UI.Xaml.FrameworkTemplate))
-   Pinceaux et couleurs (classes dérivées de [Brush](/uwp/api/Windows.UI.Xaml.Media.Brush) et valeurs [Color](/uwp/api/Windows.UI.Color))
-   Types d’animation, y compris [Storyboard](/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard)
-   Transformations (classes dérivées de [GeneralTransform](/uwp/api/Windows.UI.Xaml.Media.GeneralTransform))
-   [Matrix](/uwp/api/Windows.UI.Xaml.Media.Matrix) et [Matrix3D](/uwp/api/Windows.UI.Xaml.Media.Media3D.Matrix3D)
-   Valeurs [Point](/uwp/api/Windows.Foundation.Point)
-   Certaines autres structures en rapport avec l’interface utilisateur, telles que [Thickness](/uwp/api/Windows.UI.Xaml.Thickness) et [CornerRadius](/uwp/api/Windows.UI.Xaml.CornerRadius)
-   [Types de données intrinsèques XAML](../../xaml-platform/xaml-intrinsic-data-types.md)

Vous pouvez également utiliser des types personnalisés en tant que ressource partageable si vous suivez les modèles d’implémentation nécessaires. Vous définissez de telles classes dans votre code de stockage (ou dans des composants d’exécution que vous incluez), puis vous instanciez ces classes en XAML en tant que ressource. Les sources de données objet et les implémentations [IValueConverter](/uwp/api/Windows.UI.Xaml.Data.IValueConverter) constituent des exemples.

Les types personnalisés doivent posséder un constructeur par défaut, car l’analyseur XAML l’utilise pour instancier une classe. Les types personnalisés utilisés en tant que ressources ne peuvent pas avoir la classe [UIElement](/uwp/api/Windows.UI.Xaml.UIElement) dans leur héritage, car une classe **UIElement** ne peut jamais être partagée (elle a toujours vocation à représenter exactement un seul élément d’interface utilisateur qui existe à une seule position dans le graphe d’objets de votre application d’exécution).

## <a name="usercontrol-usage-scope"></a>Étendue de l’utilisation de UserControl


Un élément [UserControl](/uwp/api/Windows.UI.Xaml.Controls.UserControl) présente un comportement de recherche de ressource spécial car il offre les concepts inhérents d’étendue de définition et d’étendue d’utilisation. Un **UserControl** qui effectue une référence de ressource XAML à partir de son étendue de définition doit pouvoir prendre en charge la recherche de cette ressource dans sa propre séquence de recherche d’étendue de définition. Autrement dit, il ne peut pas accéder aux ressources d’application. À partir d’une étendue d’utilisation **UserControl**, une référence de ressource est traitée comme étant dans la séquence de recherche vers sa racine de page d’utilisation (comme toute autre référence de ressource effectuée à partir d’un objet dans une arborescence d’objets chargée) et peut accéder aux ressources d’application.

## <a name="resourcedictionary-and-xamlreaderload"></a>ResourceDictionary et XamlReader.Load

Vous pouvez utiliser un [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary) comme racine ou comme partie de l’entrée XAML pour la méthode [XamlReader.Load](/uwp/api/windows.ui.xaml.markup.xamlreader.load). Vous pouvez aussi inclure des références aux ressources XAML dans ce code XAML si elles sont toutes entièrement autonomes dans le code XAML envoyé à des fins de chargement. **XamlReader.Load** analyse le code XAML dans un contexte qui ne tient compte d’aucun autre objet **ResourceDictionary**, pas même de [Application.Resources](/uwp/api/windows.ui.xaml.application.resources). De même, n’utilisez pas `{ThemeResource}` qui est contenu dans le code XAML envoyé à la méthode **XamlReader.Load**.

## <a name="using-a-resourcedictionary-from-code"></a>Utilisation d’un ResourceDictionary à partir de code

La plupart des scénarios relatifs à un [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary) sont exclusivement gérés en XAML. Vous déclarez le conteneur **ResourceDictionary** et les ressources au sein d’un fichier XAML ou d’un ensemble de nœuds XAML dans un fichier de définition de l’interface utilisateur. Vous utilisez ensuite les références aux ressources XAML pour demander ces ressources à d’autres parties du code XAML. Toutefois, dans certains scénarios, votre application peut souhaiter ajuster le contenu d’un **ResourceDictionary** en utilisant du code qui est exécuté lorsque l’application est en fonctionnement, ou tout du moins pour demander le contenu d’un **ResourceDictionary** afin de voir si une ressource a déjà été définie. Ces appels de code étant effectués sur une instance de **ResourceDictionary**, vous devez d’abord extraire soit un **ResourceDictionary** immédiat quelque part dans l’arborescence d’objets en obtenant [FrameworkElement.Resources](/uwp/api/windows.ui.xaml.frameworkelement.resources), soit `Application.Current.Resources`.

Dans du code C\# ou Microsoft Visual Basic, vous pouvez référencer une ressource dans un [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary) donné au moyen de l’indexeur ([Item](/dotnet/api/system.windows.resourcedictionary.item)). Un **ResourceDictionary** étant un dictionnaire indexé par des chaînes, il utilise la clé de chaîne au lieu d’un index d’entiers. Dans les extensions de composant Visual C++ (C++/CX), utilisez [Lookup](/uwp/api/windows.ui.xaml.resourcedictionary.lookup).

Lorsque vous utilisez du code pour examiner ou modifier un [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary), le comportement pour des API comme [Lookup](/uwp/api/windows.ui.xaml.resourcedictionary.lookup) ou [Item](/dotnet/api/system.windows.resourcedictionary.item) ne passe pas directement des ressources immédiates aux ressources d’application. Il s’agit d’un comportement d’analyseur XAML qui se produit uniquement lorsque des pages XAML sont chargées. Au moment de l’exécution, l’étendue pour les clés est autonome et propre à l’instance **ResourceDictionary** que vous utilisez à ce moment-là. Toutefois, cette étendue s’étend aux [MergedDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.mergeddictionaries).

Par ailleurs, si vous demandez une clé qui n’existe pas dans le [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary), il se peut qu’aucune erreur ne soit générée et que la valeur fournie comme valeur de retour soit simplement **null**. En revanche, vous obtiendrez peut-être quand même une erreur si vous essayez d’utiliser la valeur **null** retournée en tant que valeur. L’erreur provient alors de la méthode setter de la propriété, et non de votre appel à **ResourceDictionary**. Vous pouvez éviter une erreur si la propriété a accepté **null** en tant que valeur valide. Notez le contraste entre ce comportement et un comportement de recherche XAML au moment de l’analyse XAML. Tout échec de résolution de la clé fournie à partir du code XAML au moment de l’analyse génère une erreur d’analyse XAML, même dans les cas où la propriété aurait pu accepter la valeur **null**.

Les dictionnaires de ressources fusionnés sont inclus dans l’étendue d’index du dictionnaire de ressources principal qui fait référence au dictionnaire fusionné au moment de l’exécution. Autrement dit, vous pouvez utiliser **Item** ou [Lookup](/uwp/api/windows.ui.xaml.resourcedictionary.lookup) du dictionnaire principal pour rechercher des objets qui ont été définis dans le dictionnaire fusionné. Dans ce cas, le comportement de recherche ressemble au comportement de recherche XAML au moment de l’analyse : s’il existe dans des dictionnaires fusionnés plusieurs objets qui ont la même clé, l’objet du dernier dictionnaire ajouté est retourné.

Vous êtes autorisé à ajouter des éléments à un [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary) existant en appelant **Add** (C\# ou Visual Basic) ou [Insert](/uwp/api/windows.ui.xaml.resourcedictionary.insert) (C++/CX). Vous pouvez ajouter les éléments à des ressources immédiates ou à des ressources d’application. Chacun de ces appels d’API requiert une clé, ce qui satisfait à l’exigence selon laquelle chaque élément d’un **ResourceDictionary** doit avoir une clé. Toutefois, les éléments que vous ajoutez à un **ResourceDictionary** au moment de l’exécution ne sont pas pertinents du point de vue des références aux ressources XAML. La recherche nécessaire pour les références aux ressources XAML se produit quand ce code XAML est analysé au chargement de l’application (ou une modification de thème est détectée). Les ressources ajoutées aux collections au moment de l’exécution n’étaient alors pas disponibles, et la modification du **ResourceDictionary** n’annule pas une ressource déjà récupérée de ce dernier même en modifiant la valeur de cette ressource.

Vous pouvez aussi supprimer des éléments d’un [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary) au moment de l’exécution, effectuer des copies de tous les éléments ou de certains d’entre eux, ou exécuter d’autres opérations. La liste des membres pour **ResourceDictionary** indique les API disponibles. Dans la mesure où **ResourceDictionary** a une API projetée pour prendre en charge ses interfaces de collection sous-jacentes, vos options en matière d’API diffèrent selon que vous utilisez C\# ou Visual Basic, ou bien C++/CX.

## <a name="resourcedictionary-and-localization"></a>ResourceDictionary et localisation


Un [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary) XAML peut contenir initialement des chaînes devant être localisées. Dans ce cas, vous devez stocker ces chaînes en tant que ressources de projet plutôt que dans un **ResourceDictionary**. Extrayez les chaînes du XAML et attribuez une valeur [x:Uid directive](../../xaml-platform/x-uid-directive.md) à l’élément propriétaire. Ensuite, définissez une ressource dans un fichier de ressources. Spécifiez un nom de ressource au format *XUIDValue*.*PropertyName* et une valeur de ressource de la chaîne qui doit être localisée.

## <a name="custom-resource-lookup"></a>Recherche de ressources personnalisées

Pour les scénarios avancés, vous pouvez implémenter une classe ayant un autre comportement que le comportement de recherche de référence aux ressources XAML décrit dans cette rubrique. Pour ce faire, implémentez la classe [CustomXamlResourceLoader](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader). Vous pouvez ensuite accéder à ce comportement à l’aide de l’[extension de balisage CustomResource](../../xaml-platform/customresource-markup-extension.md) pour les références de ressources au lieu d’utiliser [StaticResource](../../xaml-platform/staticresource-markup-extension.md) ou [ThemeResource](../../xaml-platform/themeresource-markup-extension.md). La plupart des applications ne présenteront pas de scénario qui exige cela. Pour plus d’informations, voir [CustomXamlResourceLoader](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader).

 
## <a name="related-topics"></a>Rubriques connexes

* [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary)
* [Vue d’ensemble du langage XAML](../../xaml-platform/xaml-overview.md)
* [Extension de balisage StaticResource](../../xaml-platform/staticresource-markup-extension.md)
* [Extension de balisage ThemeResource](../../xaml-platform/themeresource-markup-extension.md)
* [Ressources de thème XAML](xaml-theme-resources.md)
* [Application de styles aux contrôles](xaml-styles.md)
* [Attribut x:Key](../../xaml-platform/x-key-attribute.md)

 

 
