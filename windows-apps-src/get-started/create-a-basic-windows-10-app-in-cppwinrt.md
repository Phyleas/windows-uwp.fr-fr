---
title: Créer une application « Hello, World! » en utilisant C++/WinRT
description: Cette rubrique vous guide tout au long de la création d’une application Windows 10 UWP « Hello, World! » en utilisant C++/WinRT. L’interface utilisateur de l’application est définie en utilisant le langage XAML (eXtensible Application Markup Language).
ms.date: 07/11/2020
ms.topic: article
keywords: windows 10, uwp, cppwinrt, C++/WinRT
ms.localizationpriority: medium
ms.openlocfilehash: e2f4e6b808d0169f4c9f8f7142f218c00f124ae3
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493713"
---
# <a name="create-a-hello-world-app-using-cwinrt"></a>Créer une application « Hello, World! » en utilisant C++/WinRT

Cette rubrique vous guide tout au long de la création d’une application de plateforme Windows universelle (UWP) Windows 10 « Hello, World! » en utilisant C++/WinRT. L’interface utilisateur de l’application est définie en utilisant le langage XAML (eXtensible Application Markup Language).

C++/WinRT est une projection du langage C++17 moderne entièrement standard pour les API Windows Runtime (WinRT). Pour obtenir des informations, des procédures pas à pas et des exemples de code supplémentaires, consultez la documentation [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/). La rubrique [Bien démarrer avec C++/WinRT](/windows/uwp/cpp-and-winrt-apis/get-started) constitue un bon point de départ.

## <a name="set-up-visual-studio-for-cwinrt"></a>Configurer Visual Studio pour C++/WinRT

Pour plus d’informations sur la configuration du développement Visual Studio pour C++/WinRT&mdash;notamment l’installation et l’utilisation de l’extension VSIX (Visual Studio Extension) C++/WinRT et du package NuGet (qui fournissent ensemble la prise en charge des modèles et des builds de projet)&mdash;, consultez [Prise en charge de Visual Studio pour C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

Pour télécharger Visual Studio, consultez [Téléchargements](https://visualstudio.microsoft.com/downloads/).

Pour obtenir une présentation de XAML, consultez [Vue d’ensemble du langage XAML](/windows/uwp/xaml-platform/xaml-overview).

## <a name="create-a-blank-app-helloworldcppwinrt"></a>Créer une application vide (HelloWorldCppWinRT)

Notre première application, intitulée « Hello, World! », illustre certaines fonctionnalités de base en matière d’interactivité, de présentation et de styles.

Commencez par créer un nouveau projet dans Microsoft Visual Studio. Créez un projet **Application vide (C++/WinRT)** , puis nommez-le *HelloWorldCppWinRT*. Vérifiez que l’option **Placer la solution et le projet dans le même répertoire** n’est pas cochée. Ciblez la dernière version en disponibilité générale (autrement dit, pas la préversion) du SDK Windows.

Dans une section ultérieure de cette rubrique, il vous sera demandé de générer votre projet (ne le générez pas avant cela).

### <a name="about-the-project-files"></a>À propos des fichiers de projet

En général, dans le dossier du projet, chaque fichier `.xaml` (balisage XAML) a un fichier `.idl`, `.h` et `.cpp` correspondant. Réunis, ces fichiers sont compilés en un type de page XAML.

Vous pouvez modifier un fichier de balisage XAML pour créer des éléments d’interface utilisateur, et vous pouvez lier ces éléments à des sources de données (une tâche appelée [liaison de données](/windows/uwp/data-binding/)). Vous modifiez les fichiers `.h` et `.cpp` (et parfois le fichier `.idl`) pour ajouter une logique personnalisée pour votre page XAML (des gestionnaires d’événements, par exemple).

Examinons certains des fichiers de projet.

- `App.idl`, `App.xaml`, `App.h` et `App.cpp`. Ces fichiers représentent la spécialisation de votre application de la classe de [**Windows::UI::Xaml::Application**](/uwp/api/windows.ui.xaml.application), qui comporte le point d’entrée de votre application. `App.xaml` ne contient aucun balisage spécifique à chaque page, mais vous pouvez y ajouter des styles d’éléments d’interface utilisateur, ainsi que tous les autres éléments que vous voulez rendre accessibles à partir de toutes les pages. Les fichiers `.h` et `.cpp` contiennent des gestionnaires pour divers événements de cycle de vie de l’application. En général, c’est ici que vous ajoutez du code personnalisé pour initialiser votre application quand elle démarre et pour effectuer un nettoyage quand elle est suspendue ou arrêtée.
- `MainPage.idl`, `MainPage.xaml`, `MainPage.h` et `MainPage.cpp`. Ces fichiers contiennent le balisage XAML, ainsi que l’implémentation, pour le type de page principal (de démarrage) par défaut dans une application, qui est la classe runtime **MainPage**. **MainPage** ne prend pas en charge de la navigation, mais elle fournit une interface utilisateur par défaut et un gestionnaire d’événements pour vous aider à démarrer.
- `pch.h` et `pch.cpp`. Ces fichiers représentent le fichier d’en-tête précompilé de votre projet. Dans `pch.h`, incluez tous les fichiers d’en-tête qui ne changent pas souvent, puis incluez `pch.h` dans d’autres fichiers du projet.

## <a name="a-first-look-at-the-code"></a>Découverte du code

### <a name="runtime-classes"></a>Classes runtime

Comme vous le savez peut-être, toutes les classes d’une application de plateforme Windows universelle (UWP) écrites en C# sont des types Windows Runtime. Cependant, quand vous créez un type dans une application C++/WinRT, vous pouvez choisir si ce type est un type Windows Runtime ou une classe/structure/énumération C++ normale.

Toutes les pages XAML de votre projet doivent être de type Windows Runtime. Par conséquent, **MainPage** est un type Windows Runtime. Plus précisément, il s’agit d’une *classe de runtime*. Tout type qui est consommé par une page XAML doit également être un type Windows Runtime. Quand vous écrivez un [composant Windows Runtime](/windows/uwp/winrt-components/create-a-windows-runtime-component-in-cppwinrt) et que vous voulez créer un type qui peut être consommé à partir d’une autre application, vous créez alors un type Windows Runtime. Dans d’autres cas, votre type peut être un type C++ normal. En général, un type Windows Runtime peut être consommé avec n’importe quel langage Windows Runtime.

Le fait qu’un type soit défini dans [MIDL (Microsoft Interface Definition Language)](/uwp/midl-3/) à l’intérieur d’un fichier `.idl` (Interface Definition Language) constitue une bonne indication qu’il est de type Windows Runtime. Prenons **MainPage** comme exemple.

```idl
// MainPage.idl
namespace HelloWorldCppWinRT
{
    [default_interface]
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        Int32 MyProperty;
    }
}
```

Et voici la structure de base de l’implémentation de la classe runtime **MainPage** et sa fabrique d’activation, comme indiqué dans `MainPage.h`.

```cppwinrt
// MainPage.h
...
namespace winrt::HelloWorldCppWinRT::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        int32_t MyProperty();
        void MyProperty(int32_t value);
        ...
    };
}

namespace winrt::HelloWorldCppWinRT::factory_implementation
{
    struct MainPage : MainPageT<MainPage, implementation::MainPage>
    {
    };
}
```    

Pour plus d’informations sur le choix de créer ou pas une classe de runtime pour un type donné, consultez la rubrique [Créer des API avec C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis). Pour plus d’informations sur la connexion entre les classes runtime et IDL (fichiers `.idl`), vous pouvez lire et suivre la rubrique [Contrôles XAML - Liaison à une propriété C++/WinRT](/windows/uwp/cpp-and-winrt-apis/binding-property). Cette rubrique vous guide tout au long du processus de création d’une classe de runtime, dont la première étape consiste à ajouter un nouvel élément **Fichier Midl (.idl)** au projet.

Ajoutons maintenant des fonctionnalités au projet **HelloWorldCppWinRT**.

## <a name="step-1-modify-your-startup-page"></a>Étape 1. Modifier votre page de démarrage

Dans l’**Explorateur de solutions**, ouvrez `MainPage.xaml` afin de pouvoir créer les contrôles qui constituent l’interface utilisateur.

Supprimez le **StackPanel** qui y figure déjà, ainsi que son contenu. À sa place, collez le code XAML suivant.

```xaml
<StackPanel x:Name="contentPanel" Margin="120,30,0,0">
    <TextBlock HorizontalAlignment="Left" Text="Hello, World!" FontSize="36"/>
    <TextBlock Text="What's your name?"/>
    <StackPanel x:Name="inputPanel" Orientation="Horizontal" Margin="0,20,0,20">
        <TextBox x:Name="nameInput" Width="300" HorizontalAlignment="Left"/>
        <Button x:Name="inputButton" Content="Say &quot;Hello&quot;"/>
    </StackPanel>
    <TextBlock x:Name="greetingOutput"/>
</StackPanel>
```

Ce nouveau [**StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) comporte un [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) qui demande le nom de l’utilisateur, un [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) qui accepte le nom de l’utilisateur, un [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button), ainsi qu’un autre élément **TextBlock**.

Étant donné que nous avons supprimé l’élément **Button** nommé *myButton*, nous allons devoir supprimer la référence à celui-ci dans le code. Par conséquent, dans `MainPage.cpp`, supprimez la ligne de code se trouvant dans la fonction **MainPage::ClickHandler**.

À ce stade, vous avez créé une application Windows universelle très basique. Pour voir à quoi ressemble l’application UWP, générez et exécutez l’application.

![Écran de l'application UWP, avec des contrôles](images/xaml-hw-app2.png)

Dans l’application, vous pouvez taper du contenu dans la zone de texte. Mais le fait de cliquer sur le bouton n’a pour le moment aucun effet.

## <a name="step-2-add-an-event-handler"></a>Étape 2. Ajouter un gestionnaire d’événements

Dans `MainPage.xaml`, recherchez l’élément **Button** nommé *inputButton*, puis déclarez un gestionnaire d’événements pour son événement [**ButtonBase ::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click). Le balisage de l’élément **Button** doit maintenant ressembler à ceci.

```xaml
<Button x:Name="inputButton" Content="Say &quot;Hello&quot;" Click="inputButton_Click"/>
```

Implémentez le gestionnaire d’événements comme suit.

```cppwinrt
// MainPage.h
struct MainPage : MainPageT<MainPage>
{
    ...
    void inputButton_Click(
        winrt::Windows::Foundation::IInspectable const& sender,
        winrt::Windows::UI::Xaml::RoutedEventArgs const& e);
};

// MainPage.cpp
namespace winrt::HelloWorldCppWinRT::implementation
{
    ...
    void MainPage::inputButton_Click(
        winrt::Windows::Foundation::IInspectable const& sender,
        winrt::Windows::UI::Xaml::RoutedEventArgs const& e)
    {
        greetingOutput().Text(L"Hello, " + nameInput().Text() + L"!");
    }
}
```

Pour plus d’informations, consultez [Gérer des événements en utilisant des délégués](/windows/uwp/cpp-and-winrt-apis/handle-events).

L’implémentation récupère le nom de l’utilisateur à partir de la zone de texte, puis il utilise pour créer un message d’accueil qu’il l’affiche dans le bloc de texte *greetingOutput*.

Générez et exécutez l’application. Tapez votre nom dans la zone de texte, puis cliquez sur le bouton. L’application affiche un message d’accueil personnalisé.

![Écran de l’application avec un message affiché](images/xaml-hw-app3.png)

## <a name="step-3-style-the-startup-page"></a>Étape 3. Appliquer un style à la page de démarrage

### <a name="choose-a-theme"></a>Choisir un thème

Il est très simple de personnaliser l’apparence d’une application. Par défaut, votre application utilise des ressources dont le style est de couleur claire. Les ressources système comprennent également un thème sombre.

Pour tester le thème sombre, modifiez `App.xaml`, puis ajoutez une valeur pour [**Application::RequestedTheme**](/uwp/api/windows.ui.xaml.application.requestedtheme).

```xaml
<Application
    ...
    RequestedTheme="Dark">

</Application>
```

Nous recommandons le thème sombre pour les applications qui affichent essentiellement des images ou du contenu vidéo, et le thème clair pour les applications qui contiennent beaucoup de texte. Si vous utilisez un modèle de couleurs personnalisé, utilisez le thème qui s’accorde le mieux à l’apparence de votre application.

> [!NOTE]
> Un thème est appliqué au démarrage de votre application. Il ne peut pas être changé pendant que l’application est en cours d’exécution.

### <a name="use-system-styles"></a>Utiliser les styles système

Dans cette section, nous allons modifier l’apparence du texte (par exemple, en augmentant la taille de la police).

Dans `MainPage.xaml`, recherchez « What is your name ? » (Quel est votre nom ?) comme valeur de **TextBlock**. Définissez sa propriété [**Style**](/uwp/api/windows.ui.xaml.style) sur une référence à la clé de ressource système *BaseTextBlockStyle*.

```xaml
<TextBlock Text="What's your name?" Style="{ThemeResource BaseTextBlockStyle}"/>
```

*BaseTextBlockStyle* est la clé d’une ressource qui est définie dans l’élément [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) de `\Program Files (x86)\Windows Kits\10\DesignTime\CommonConfiguration\Neutral\UAP\<version>\Generic\generic.xaml`. Voici les valeurs de propriété qui sont définies par ce style.

```xaml
<Style x:Key="BaseTextBlockStyle" TargetType="TextBlock">
    <Setter Property="FontFamily" Value="XamlAutoFontFamily" />
    <Setter Property="FontWeight" Value="SemiBold" />
    <Setter Property="FontSize" Value="14" />
    <Setter Property="TextTrimming" Value="None" />
    <Setter Property="TextWrapping" Value="Wrap" />
    <Setter Property="LineStackingStrategy" Value="MaxHeight" />
    <Setter Property="TextLineBounds" Value="Full" />
</Style>
```

Également dans `MainPage.xaml`, recherchez l’élément **TextBlock** nommé `greetingOutput`. Définissez aussi sa propriété **Style** sur *BaseTextBlockStyle*. Si vous générez et exécutez l’application maintenant, vous voyez que l’apparence des deux blocs de texte a changé (par exemple, la taille de police est maintenant plus grande).

## <a name="step-4-have-the-ui-adapt-to-different-window-sizes"></a>Étape 4. Faire en sorte que l’interface utilisateur s’adapte à différentes tailles de fenêtre

À présent, nous allons faire en sorte que l’interface utilisateur s’adapte de façon dynamique à un changement de taille de la fenêtre et qu’elle s’affiche correctement sur les appareils dotés de petits écrans. Pour ce faire, vous allez ajouter une section [**VisualStateManager**](/uwp/api/Windows.UI.Xaml.VisualStateManager) dans `MainPage.xaml`. Vous allez définir différents états visuels pour différentes tailles de fenêtres, puis définir les propriétés à appliquer pour chacun de ces états visuels.

### <a name="adjust-the-ui-layout"></a>Ajuster la disposition de l’interface utilisateur

Ajoutez ce bloc de code XAML en tant que premier élément enfant de l’élément racine **StackPanel**.

```xaml
<StackPanel ...>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState x:Name="wideState">
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="641" />
                </VisualState.StateTriggers>
            </VisualState>
            <VisualState x:Name="narrowState">
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="0" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="contentPanel.Margin" Value="20,30,0,0"/>
                    <Setter Target="inputPanel.Orientation" Value="Vertical"/>
                    <Setter Target="inputButton.Margin" Value="0,4,0,0"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    ...
</StackPanel>
```

Générez et exécutez l’application. Notez que la présentation de l’interface utilisateur est la même qu’auparavant jusqu’à ce que la fenêtre soit redimensionnée à une taille inférieure à 641 DIP (Device-Independent Pixels, pixels indépendants des appareils). À ce stade, l’état visuel *narrowState* est appliqué et, avec lui, toutes les méthodes setter de propriété définis pour cet état.

L’élément [**VisualState**](/uwp/api/Windows.UI.Xaml.VisualState) nommé `wideState` comporte un élément [**AdaptiveTrigger**](/uwp/api/Windows.UI.Xaml.AdaptiveTrigger) dont la propriété [**MinWindowWidth**](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth) est définie sur 641. Cela signifie que l’état ne doit s’appliquer que si la largeur de la fenêtre n’est pas inférieure à la valeur minimale de 641 DIP. Vous ne définissez aucun objet [**Setter**](/uwp/api/Windows.UI.Xaml.Setter) pour cet état, de sorte qu’il utilise les propriétés de disposition que vous avez définies dans le code XAML pour le contenu de la page.

Le second élément [**VisualState**](/uwp/api/Windows.UI.Xaml.VisualState), `narrowState`, comporte un élément [**AdaptiveTrigger**](/uwp/api/Windows.UI.Xaml.AdaptiveTrigger) dont la propriété [**MinWindowWidth**](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth) est définie sur 0. Cet état est appliqué lorsque la largeur de la fenêtre est supérieure à 0, mais inférieure à 641 DIP. À exactement 641 DIP, `wideState` est actif. Dans `narrowState`, vous définissez des objets [**Setter**](/uwp/api/Windows.UI.Xaml.Setter) de façon à changer les propriétés de disposition des contrôles dans l’interface utilisateur.

- Vous réduisez la marge gauche de l’élément *contentPanel* de 120 à 20.
- Vous changez l’[**Orientation**](/uwp/api/windows.ui.xaml.controls.stackpanel.orientation) de l’élément *inputPanel* en remplaçant **Horizontal** par **Vertical**.
- Vous ajoutez une marge supérieure de 4 DIP à l’élément *inputButton*.

## <a name="summary"></a>Résumé

Cette procédure pas à pas vous a montré comment ajouter du contenu dans une application Windows universelle, comment ajouter de l’interactivité et comment changer l’apparence de l’interface utilisateur.
