---
title: Portage de l’exemple Clipboard vers C++/WinRT depuis C# (une étude de cas)
description: Cette rubrique présente une étude de cas de portage de l’un des [exemples d’application de plateforme Windows universelle (UWP)](https://github.com/microsoft/Windows-universal-samples) à partir de [C#](/visualstudio/get-started/csharp) vers [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).
ms.date: 03/20/2020
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, porter, migrer, C#, exemple, presse-papiers, cas, étude
ms.localizationpriority: medium
ms.openlocfilehash: 570f3538bf15616a45a17cdbce9a56066c8036bc
ms.sourcegitcommit: 23c5d8dfaeb6edbca780637ffd26fe892db27519
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123647"
---
# <a name="porting-the-clipboard-sample-tocwinrtfromcmdasha-case-study"></a>Portage de l’exemple Clipboard vers C++/WinRT depuis C#&mdash;une étude de cas

Cette rubrique présente une étude de cas de portage de l’un des [exemples d’application de plateforme Windows universelle (UWP)](https://github.com/microsoft/Windows-universal-samples) à partir de [C#](/visualstudio/get-started/csharp) vers [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt). Vous pouvez bénéficier d’une pratique et d’une expérience de portage en suivant la procédure pas à pas et en portant l’exemple pour vous-même au fur et à mesure.

Consultez également [Passer de C# à C++/WinRT](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-csharp), qui présente plusieurs sections traitant de détails techniques spécifiques impliqués dans le portage de C# vers C++/WinRT.

## <a name="download-and-test-the-clipboard-sample"></a>Télécharger et tester l’exemple Presse-papiers

Visitez la page web de l’[exemple Clipboard](https://docs.microsoft.com/samples/microsoft/windows-universal-samples/clipboard/), puis cliquez sur **Download ZIP** (Télécharger le ZIP). Décompressez le fichier téléchargé, puis examinez la structure des dossiers.

- La version C# de l’exemple de code source est contenue dans le dossier nommé `cs`. Les autres fichiers utilisés par la version C# se trouvent dans les dossiers `shared` et `SharedContent`.
- Vous pouvez trouver la version C++/WinRT de l’exemple de code source dans le dossier [cppwinrt](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/Clipboard/cppwinrt), dans le dépôt de l’exemple sur GitHub.

La procédure pas à pas de cette rubrique montre comment vous pouvez recréer la version C++/WinRT en la portant à partir du code source C#. De cette façon, vous pouvez voir comment vous pouvez porter vos propres projets C# vers C++/WinRT.

Pour avoir une idée de ce que fait l’exemple, ouvrez la solution C# (`\Clipboard_sample\cs\Clipboard.sln`), modifiez la configuration de façon appropriée (peut-être en *x64*), générez et exécutez. L’interface utilisateur de l’exemple vous guide à travers ses diverses fonctionnalités, pas à pas.

## <a name="createablankappcwinrt-named-clipboard"></a>Créer une application vide (C++/WinRT), nommée Clipboard

> [!NOTE]
> Pour plus d’informations sur l’installation et l’utilisation de l’extension VSIX (Visual Studio Extension) C++/WinRT et du package NuGet (qui fournissent ensemble la prise en charge des modèles et des builds de projet), consultez  [Prise en charge de Visual Studio pour C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

Commencez le processus de portage en créant un nouveau projet C++/WinRT dans Microsoft Visual Studio. Créez un projet en utilisant le modèle de projet **Application vide (C++/WinRT)** . Définissez son nom sur *Clipboard* et (pour que votre structure de dossiers corresponde à la procédure pas à pas), vérifiez que la case **Placer la solution et le projet dans le même répertoire** est décochée.

Juste pour obtenir une base de référence, vérifiez que ce nouveau projet vide peut être généré et exécuté.

## <a name="packageappxmanifest-and-asset-files"></a>Package.appxmanifest et fichiers de ressources

Si les versions C# et C++/WinRT de l’exemple n’ont pas besoin d’être installées côte à côte sur la même machine, les fichiers sources du manifeste du package d’application des deux projets (`Package.appxmanifest`) peuvent être identiques. Dans le cas présent, vous pouvez simplement copier `Package.appxmanifest` depuis le projet C# vers le projet C++/WinRT.

Pour que les deux versions de l’exemple coexistent, elles ont besoin d’identificateurs différents. Dans ce cas, dans le projet C++/WinRT, ouvrez le fichier `Package.appxmanifest` dans un éditeur XML et prenez note de ces trois valeurs.

- Dans l’élément **/Package/Identity**, notez la valeur de l’attribut **Name**. Il s’agit du *nom du package*. Pour un projet nouvellement créé, le projet lui affecte comme valeur initiale un GUID unique.
- Dans l’élément **/Package/Applications/Application**, notez la valeur de l’attribut **Id**. Il s’agit de l’*ID d’application*.
- Dans l’élément **/Package/mp:PhoneIdentity**, notez la valeur de l’attribut **PhoneProductId**. Là encore, pour un projet nouvellement créé, ceci est défini sur le même GUID que le nom du package.

Copiez ensuite `Package.appxmanifest` depuis le projet C# vers le projet C++/WinRT. Enfin, vous pouvez restaurer les trois valeurs que vous avez notées. Vous pouvez aussi modifier les valeurs copiées pour les rendre uniques et/ou appropriées pour l’application et pour votre organisation (comme vous le feriez habituellement pour un nouveau projet). Par exemple, dans ce cas, au lieu de restaurer la valeur du nom du package, nous pouvons simplement changer la valeur copiée de *Microsoft.SDKSamples.Clipboard.CS* en *Microsoft.SDKSamples.Clipboard.CppWinRT*. Et nous pouvons conserver l’ID d’application défini sur *App*. Tant que le nom du package *ou* l’ID d’application sont différents, les deux applications auront des ID de modèle utilisateur d’application (AUMID, Application User Model ID) différents.

Dans le cadre de cette procédure pas à pas, apporter quelques modifications à `Package.appxmanifest` peut être justifié. Il y a trois occurrences de la chaîne *Clipboard C# Sample*. Changez-les en *Clipboard C++/WinRT Sample*.

Dans le projet C++/WinRT, le fichier `Package.appxmanifest` et le projet ne sont maintenant plus synchronisés quant aux fichiers de ressources qu’ils référencent. Pour remédier à cela, supprimez d’abord les ressources du projet C++/WinRT en sélectionnant tous les fichiers du dossier `Assets` (dans l’Explorateur de solutions de Visual Studio) et en les supprimant (choisissez **Supprimer** dans la boîte de dialogue).

Le projet C# référence des fichiers de ressources d’un dossier partagé. Vous pouvez faire de même dans le projet C++/WinRT ou vous pouvez copier les fichiers comme nous allons le faire dans cette procédure pas à pas.

Accédez au dossier `\Clipboard_sample\SharedContent\media`. Sélectionnez les sept fichiers inclus dans le projet C# (de `microsoft-sdk.png` à `windows-sdk.png`), copiez-les, puis collez-les dans le dossier `\Clipboard\Clipboard\Assets` du nouveau projet.

Cliquez avec le bouton droit sur le dossier `Assets` (dans l’Explorateur de solutions, dans le projet C++/WinRT) > **Ajouter** > **Élément existant...* et accédez à `\Clipboard\Clipboard\Assets`. Dans le sélecteur de fichiers, sélectionnez les sept fichiers, puis cliquez sur **Ajouter**.

`Package.appxmanifest` est maintenant à nouveau synchronisé avec les fichiers de ressources du projet.

## <a name="mainpage-including-the-sample-configuration-functionality"></a>**MainPage**, avec les fonctionnalités de configuration de l’exemple

L’exemple Clipboard, comme tous les [exemples d’applications UWP (Plateforme Windows universelle)](https://github.com/microsoft/Windows-universal-samples), est constitué d’une collection de scénarios que l’utilisateur peut parcourir l’un après l’autre. La collection de scénarios dans un exemple donné est configurée dans le code source de l’exemple. Chaque scénario de la collection est un élément de données qui stocke un titre ainsi que le type de la classe du projet qui implémente le scénario.

Dans la version C# de l’exemple, si vous regardez dans le fichier de code source `SampleConfiguration.cs`, vous voyez deux classes. La plus grande partie de la logique de configuration se trouve dans la classe **MainPage**, qui est une classe partielle (elle forme une classe complète quand elle est combinée avec le balisage dans `MainPage.xaml` et avec le code impératif dans `MainPage.xaml.cs`). L’autre classe de ce fichier de code source est **Scenario**, avec ses propriétés **Title** et **ClassType**.

Dans les sous-sections suivantes, nous allons voir comment porter **MainPage** et **Scenario**.

### <a name="idl-for-the-mainpage-type"></a>IDL pour le type **MainPage**

Les fichiers de code source C# qui implémentent ensemble le type **MainPage** sont : `MainPage.xaml` (que nous allons bientôt porter en le copiant), `MainPage.xaml.cs` et `SampleConfiguration.cs`.

Dans la version C++/WinRT, nous factorisons notre type **MainPage** dans les fichiers de code source d’une façon similaire. Nous allons prendre la logique dans `MainPage.xaml.cs` et la traduire pour la plus grande partie en `MainPage.h` et `MainPage.cpp`. Pour la logique dans `SampleConfiguration.cs`, nous allons traduire cela en `SampleConfiguration.h` et `SampleConfiguration.cpp`.

Les classes d une application UWP C# sont des types Windows Runtime. Cependant, quand vous créez un type dans une application C++/WinRT, vous pouvez choisir si ce type est un type Windows Runtime ou une classe/structure/énumération normale.

Dans le projet C++/WinRT, **MainPage** est déjà un type Windows Runtime : nous ne devons donc rien y changer de ce point de vue. Plus précisément, il s’agit d’une *classe de runtime*.

- Pour plus d’informations sur le choix de créer ou pas une classe de runtime, consultez la rubrique [Créer des API avec C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis).
- Les concepts de *type d’implémentation* et de *type projeté* sont importants en C++/WinRT. Vous pouvez découvrir plus d’informations sur ces concepts dans la rubrique mentionnée ci-dessus et dans [Créer des API avec C++/WinRT](/windows/uwp/cpp-and-winrt-apis/consume-apis).
- Pour plus d’informations sur la connexion entre les classes de runtime et les fichiers `.idl`, lisez et suivez la rubrique [Contrôles XAML ; liaison à une propriété C++/WinRT](/windows/uwp/cpp-and-winrt-apis/binding-property). Cette rubrique vous guide tout au long du processus de création d’une classe de runtime, dont la première étape consiste à ajouter un nouvel élément **Fichier Midl (.idl)** au projet.

Pour **MainPage**, nous avons déjà le fichier `MainPage.idl` nécessaire dans le projet C++/WinRT. Cependant, au cours de cette procédure pas à pas, nous ajouterons de *nouveaux* fichiers `.idl` au projet.

Nous verrons bientôt un descriptif de l’IDL que nous devons ajouter au fichier `MainPage.idl` existant. Avant cela, nous allons réfléchir à ce qui doit aller et ne pas aller dans l’IDL.

Pour déterminer quels membres de **MainPage** nous devons déclarer dans `MainPage.idl` (afin qu’ils fassent partie de la classe de runtime **MainPage**) et ceux qui peuvent simplement être membres du type d’implémentation de **MainPage**, nous allons créer une liste des membres de la classe C# **MainPage**. Nous trouvons ces membres en examinant `MainPage.xaml.cs` et `SampleConfiguration.cs`.

Nous trouvons un total de douze champs et méthodes `protected` et `private`. Nous trouvons aussi les membres `public` suivants.

- Le constructeur par défaut `MainPage()`.
- Les champs statiques **Current** et **FEATURE_NAME**.
- Les propriétés **IsClipboardContentChangedEnabled** et **Scenarios**.
- Les méthodes **BuildClipboardFormatsOutputString**, **DisplayToast**, **EnableClipboardContentChangedNotifications** et **NotifyUser**.

Ce sont ces membres `public` qui sont des candidats à la déclaration dans `MainPage.idl`. Examinons donc chacun d’entre eux et voyons s’ils doivent faire partie de la classe de runtime **MainPage** ou s’ils doivent seulement faire partie de l’implémentation.

- Le constructeur par défaut `MainPage()`. Pour une **Page** XAML, il est normal de déclarer un constructeur par défaut dans son IDL. De cette façon, l’infrastructure d’interface utilisateur XAML peut activer le type.
- Le champ statique **Current** est utilisé dans les pages XAML de scénario individuelles pour accéder à l’instance de l’application de **MainPage**. Comme **Current** n’est pas utilisé pour interagir avec l’infrastructure XAML (et qu’il n’est pas utilisé dans les unités de compilation), nous pourrions en faire seulement un membre du type d’implémentation. Avec vos propres projets, dans des cas de ce type, vous pouvez choisir de procéder ainsi. Néanmoins, comme le champ est une instance du type projeté, il semble logique de le déclarer dans l’IDL. C’est ce que nous allons faire ici (et cela rend également le code un peu plus clair).
- Le cas est similaire pour le champ statique **FEATURE_NAME**, qui est accessible dans le type **MainPage**. Là encore, le choix de le déclarer dans l’IDL rend notre code un peu plus clair.
- La propriété **IsClipboardContentChangedEnabled** est utilisée seulement dans la classe **OtherScenarios**. Ainsi, au cours du portage, nous allons simplifier un peu les choses et en faire un champ privé de la classe de runtime **OtherScenarios**. Celui-ci n’ira donc pas dans l’IDL.
- La propriété **Scenarios** est une collection d’objets de type **Scenario** (un type que nous avons mentionné précédemment). Nous parlerons de **Scenario** dans la sous-section suivante : laissons donc la propriété **Scenarios** telle quelle jusqu’alors.
- Les méthodes **BuildClipboardFormatsOutputString**, **DisplayToast** et **EnableClipboardContentChangedNotifications** sont des fonctions utilitaires qui ont plus à faire avec l’état général de l’exemple qu’avec la page principale. Ainsi, au cours du portage, nous allons refactoriser ces trois méthodes sur un nouveau type d’utilitaire nommé **SampleState** (qui n’a pas besoin d’être un type Windows Runtime). Pour cette raison, ces trois méthodes n’iront pas dans l’IDL.
- La méthode **NotifyUser** est appelée à partir des pages XAML de scénario individuelles sur l’instance de **MainPage** qui est retournée du champ statique *Current*. **Current** étant une instance du type projeté (comme indiqué précédemment), nous devons déclarer **NotifyUser** dans l’IDL. **NotifyUser** prend un paramètre de type **NotifyType**. Nous parlerons de cela dans la sous-section suivante.

Nous progressons : nous développons une liste des membres à ajouter et de ceux à ne pas ajouter au fichier `MainPage.idl`. Nous devons cependant encore aborder la propriété **Scenarios** et le type **NotifyType**. Faisons-le donc maintenant.

### <a name="idl-for-the-scenario-and-notifytype-types"></a>IDL pour les types **Scenario** et **NotifyType**

La classe **Scenario** est définie dans `SampleConfiguration.cs`. Nous devons décider de la façon de porter cette classe en C++/WinRT. Par défaut, nous en ferions probablement un `struct` C++ ordinaire. Cependant, si **Scenario** est utilisé dans des fichiers binaires, ou pour interagir avec l’infrastructure XAML, il doit être déclaré dans l’IDL en tant que type Windows Runtime.

En regardant le code source C#, nous constatons que **Scenario** est utilisé dans ce contexte.

```xaml
<ListBox x:Name="ScenarioControl" ... >
```

```csharp
var itemCollection = new List<Scenario>();
int i = 1;
foreach (Scenario s in scenarios)
{
    itemCollection.Add(new Scenario { Title = $"{i++}) {s.Title}", ClassType = s.ClassType });
}
ScenarioControl.ItemsSource = itemCollection;
```

Une collection d’objets **Scenario** est affectée à la propriété [**ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) d’une **ListBox** (qui est un contrôle d’éléments). Comme **Scenario** *a* besoin d’interagir avec XAML, il doit s’agir d’un type Windows Runtime. Elle doit donc être définie dans l’IDL. Définir le type **Scenario** dans l’IDL fait que le système de build C++/WinRT génère pour vous une définition dans le code source de **Scenario** dans un fichier d’en-tête en arrière-plan (le nom et l’emplacement de celui-ci ne sont pas importants pour cette procédure pas à pas).

Vous vous rappellerez que **MainPage.Scenarios** est une collection d’objets **Scenario**, dont nous venons d’établir qu’il devaient être déclarés dans l’IDL. Pour cette raison, **MainPage.Scenarios** doit également être déclaré dans l’IDL.

**NotifyType** est une `enum` déclarée dans le fichier `MainPage.xaml.cs` de C#. Comme nous passons **NotifyType** à une méthode appartenant à la classe de runtime **MainPage**, **NotifyType** doit être un type Windows Runtime, et il doit être défini dans `MainPage.idl`.

Ajoutons maintenant au fichier `MainPage.idl` les nouveaux types et le nouveau membre de **MainPage** que nous avons décidé de déclarer dans l’IDL. En même temps, nous allons supprimer de l’IDL les membres de l’espace réservé de **MainPage** que le modèle de projet Visual Studio nous a donnés.

Ainsi, dans votre projet C++/WinRT, ouvrez `MainPage.idl` et modifiez-le afin qu’il ressemble à ce qui figure ci-dessous. Notez que l’une des modifications consiste à changer le nom de l’espace de noms de **Clipboard** en **SDKTemplate**. Si vous le souhaitez, vous pouvez simplement supprimer le contenu actuel de votre fichier `MainPage.idl` et y coller ce qui figure ci-dessous. Une autre modification à noter est que nous changeons le nom de **Scenario::ClassType** en **Scenario::ClassName**.

```idl
// MainPage.idl
namespace SDKTemplate
{
    struct Scenario
    {
        String Title;
        Windows.UI.Xaml.Interop.TypeName ClassName;
    };

    enum NotifyType
    {
        StatusMessage,
        ErrorMessage
    };

    [default_interface]
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();

        static MainPage Current{ get; };
        static String FEATURE_NAME{ get; };

        static Windows.Foundation.Collections.IVector<Scenario> scenarios{ get; };

        void NotifyUser(String strMessage, NotifyType type);
    };
}
```

> [!NOTE]
> Pour plus d’informations sur le contenu d’un fichier `.idl` dans un projet C++/WinRT, consultez [Microsoft Interface Definition Language 3.0](/uwp/midl-3/).

Dans votre propre travail de portage, vous ne voulez peut-être pas changer le nom de l’espace de noms comme nous l’avons fait ci-dessus. Nous ne le faisons ici, car l’espace de noms par défaut du projet C# que nous allons porter est **SDKTemplate**, alors que le nom du projet et de l’assembly est **Clipboard**.

Cependant, en effectuant le portage dans cette procédure pas à pas, nous allons changer chaque occurrence dans le code source du nom de l’espace de noms **Clipboard** en **SDKTemplate**. Il y a également un emplacement dans les propriétés du projet C++/WinRT où le nom de l’espace de noms **Clipboard** apparaît : nous allons donc en profiter pour changer cela maintenant.

Dans Visual Studio, pour le projet C++/WinRT, définissez la propriété du projet **Propriétés communes** \> **C++/WinRT** \> **Espace de noms racine** sur la valeur *SDKTemplate*.

### <a name="save-the-idl-and-re-generate-stub-files"></a>Enregistrer l’IDL et regénérer les fichiers stub

Si vous avez lu la rubrique [Contrôles XAML ; liaison à une propriété C++/WinRT](/windows/uwp/cpp-and-winrt-apis/binding-property), vous aurez rencontré la notion de *fichiers stub*. Les fichiers stub sont générés pour vous (par un outil nommé `cppwinrt.exe`, et ils sont basés sur le contenu de vos fichiers `.idl`) quand vous générez votre projet C++/WinRT. Cette rubrique contient plus de détails à leur sujet.

À ce stade de la procédure pas à pas, nous avons fini de modifier le fichier `MainPage.idl` pour le moment : vous devez donc l’enregistrer maintenant. Le projet ne peut pas être généré complètement pour l’instant, mais effectuer une build maintenant est une opération utile, car elle regénère les fichiers stub pour **MainPage**.

Pour ce projet C++/WinRT, ces fichiers stub sont générés dans le dossier `\Clipboard\Clipboard\Generated Files\sources`. Vous les y trouvez une fois la build partielle terminée (à nouveau, comme prévu, la build ne réussit pas entièrement, mais l’étape qui nous intéresse, la génération des stubs, *aura* réussi). Les fichiers qui nous intéressent sont `MainPage.h` et `MainPage.cpp`.

Dans ces deux fichiers stub, vous voyez des implémentations stub des nouveaux membres de **MainPage** que nous avons ajoutés à l’IDL (par exemple **Current** et **FEATURE_NAME**). Nous allons copier ces implémentations stub dans les fichiers `MainPage.h` et `MainPage.cpp` qui se trouvent déjà dans le projet. En même temps, tout comme nous l’avons fait avec l’IDL, nous allons supprimer de ces fichiers existants les membres de l’espace réservé de **MainPage** que le modèle de projet Visual Studio nous a donnés (la propriété factice nommée **MyProperty** et le gestionnaire d’événements nommé **ClickHandler**).

En fait, le seul membre de la version actuelle de **MainPage** que nous voulons conserver est le constructeur.

Une fois que vous avez copié les nouveaux membres à partir des fichiers stub, supprimé les membres dont nous ne voulons pas et mis à jour l’espace de noms, les fichiers `MainPage.h` et `MainPage.cpp` de votre projet doivent se présenter comme ceci. Notez que la seule modification que nous avons apportée au type **factory_implementation** a été de mettre à jour son espace de noms.

```cppwinrt
// MainPage.h
#pragma once
#include "MainPage.g.h"

namespace winrt::SDKTemplate::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        static SDKTemplate::MainPage Current();
        static hstring FEATURE_NAME();
        static Windows::Foundation::Collections::IVector<SDKTemplate::Scenario> scenarios();
        void NotifyUser(hstring const& strMessage, SDKTemplate::NotifyType const& type);
    };
}
namespace winrt::SDKTemplate::factory_implementation
{
    struct MainPage : MainPageT<MainPage, implementation::MainPage>
    {
    };
}
```

```cppwinrt
// MainPage.cpp
#include "pch.h"
#include "MainPage.h"
#include "MainPage.g.cpp"

namespace winrt::SDKTemplate::implementation
{
    MainPage::MainPage()
    {
        InitializeComponent();
    }
    SDKTemplate::MainPage MainPage::Current()
    {
        throw hresult_not_implemented();
    }
    hstring MainPage::FEATURE_NAME()
    {
        throw hresult_not_implemented();
    }
    Windows::Foundation::Collections::IVector<SDKTemplate::Scenario> MainPage::scenarios()
    {
        throw hresult_not_implemented();
    }
    void MainPage::NotifyUser(hstring const& strMessage, SDKTemplate::NotifyType const& type)
    {
        throw hresult_not_implemented();
    }
}
```

Pour les chaînes, C# utilise **System.String**. Pour un exemple, consultez la méthode **MainPage.NotifyUser**. Dans notre IDL, nous déclarons une chaîne avec **String** et, quand l’outil `cppwinrt.exe` génère le code C++/WinRT pour nous, il utilise le type de [**winrt::hstring**](/uw/cpp-ref-for-winrt/hstring). Chaque fois que nous fournissons une chaîne C# dans le code, nous allons porter cela en **winrt::hstring**. Pour plus d’informations, consultez [Gestion des chaînes en C++/WinRT](/windows/uwp/cpp-and-winrt-apis/strings).

Pour obtenir une explication des paramètres de `const&` dans les signatures de méthode, consultez [Passage de paramètres](/windows/uwp/cpp-and-winrt-apis/concurrency#parameter-passing).

### <a name="update-all-remaining-namespace-declarationsreferences-and-build"></a>Mettre à jour toutes les déclarations/références d’espace de noms restantes et générer

Avant de générer le projet C++/WinRT, recherchez les déclarations ou les références à l’espace de noms **Clipboard**, puis remplacez-les par **SDKTemplate**.

- `MainPage.xaml` et `App.xaml`. L’espace de noms apparaît dans les valeurs des attributs `x:Class` et `xmlns:local`.
- `App.idl`.
- `App.h`.
- `App.cpp`. Il existe deux directives `using namespace` (recherchez la sous-chaîne `using namespace Clipboard`) et deux qualifications du type **MainPage** (recherchez `Clipboard::MainPage`). Celles-ci doivent être changées.

Comme nous avons supprimé le gestionnaire d’événements de **MainPage**, accédez également à `MainPage.xaml` et supprimez l’élément **Button** du balisage.

Enregistrez tous les fichiers. Nettoyez la solution (**Générer** > **Nettoyer Clipboard**), puis générez-la. La génération va réussir si les modifications que vous avez apportées jusqu’à présent sont valides.

### <a name="implement-the-mainpage-members-that-we-declared-in-idl"></a>Implémenter les membres de **MainPage** que nous avons déclarés dans l’IDL

#### <a name="the-constructor-current-and-feature_name"></a>Le constructeur, **Current** et **FEATURE_NAME**

Voici le code concerné (du projet C#) que nous avons besoin de porter.

```xaml
<!-- MainPage.xaml -->
...
<TextBlock x:Name="SampleTitle" ... />
...
```

```csharp
// MainPage.xaml.cs
...
public sealed partial class MainPage : Page
{
    public static MainPage Current;

    public MainPage()
    {
        InitializeComponent();
        Current = this;
        SampleTitle.Text = FEATURE_NAME;
    }
...
}
...

// SampleConfiguration.cs
...
public partial class MainPage : Page
{
    public const string FEATURE_NAME = "Clipboard C# sample";
...
}
...
```

Nous allons bientôt réutiliser `MainPage.xaml` dans son intégralité (en le copiant). Pour l’instant, nous allons ajouter temporairement un élément **TextBlock** avec le nom approprié dans le fichier `MainPage.xaml` du projet C++/WinRT.

**FEATURE_NAME** est un champ statique de **MainPage** (un champ `const` C# est essentiellement statique dans son comportement), défini dans `SampleConfiguration.cs`. Pour C++/WinRT, au lieu d’un champ (statique), nous allons en faire l’expression C++/WinRT d’une propriété (statique) en lecture seule. En C++/WinRT, la façon d’exprimer un accesseur Get de propriété est comme une fonction qui retourne la valeur de la propriété et ne prend aucun paramètre (un accesseur). Le champ statique **FEATURE_NAME** C# devient donc la fonction d’accesseur statique **FEATURE_NAME** de C++/WinRT (dans ce cas, qui retourne le littéral de chaîne).

Nous ferions d’ailleurs la même chose pour le portage d’une propriété en lecture seule C#. Pour une propriété C# accessible en écriture, la façon d’exprimer en C++/WinRT un accesseur Set de propriété est une fonction `void` qui prend la valeur de la propriété en tant que paramètre (un mutateur). Dans les deux cas, si le champ ou la propriété C# est statique, l’accesseur et/ou le mutateur C++/WinRT le sont également.

**Current** est un champ statique (pas constant) de **MainPage**. Là encore, nous allons en faire l’expression C++/WinRT d’une propriété en lecture seule, puis la rendre statique. Si **FEATURE_NAME** est constant, **Current** ne l’est pas. Donc, en C++/WinRT, nous aurons besoin d’un champ de stockage, et notre accesseur retournera son contenu. Ainsi, dans le projet C++/WinRT, nous allons déclarer dans `MainPage.h` un champ statique privé nommé **current**, nous allons définir/initialiser **current** dans `MainPage.cpp` (car il a une durée de stockage statique) et nous allons y accéder via une fonction d’accesseur statique publique nommée **Current**.

Le constructeur lui-même exécute quelques affectations, qui sont simples à porter.

Dans le projet C++/WinRT, ajoutez un nouvel élément **Visual C++**  > **Code** > **Fichier C++ (. cpp)** avec le nom `SampleConfiguration.cpp`.

Modifiez `MainPage.xaml`, `MainPage.h`, `MainPage.cpp`et `SampleConfiguration.cpp` pour qu’ils correspondent à ce qui figure ci-dessous.

```xaml
<!-- MainPage.xaml -->
...
<StackPanel ...>
    <TextBlock x:Name="SampleTitle" />
</StackPanel>
...
```

```cppwinrt
// MainPage.h
...
namespace winrt::SDKTemplate::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
...
        static SDKTemplate::MainPage Current() { return current; }
...
    private:
        static SDKTemplate::MainPage current;
...
    };
...
}

// MainPage.cpp
...
namespace winrt::SDKTemplate::implementation
{
    SDKTemplate::MainPage MainPage::current{ nullptr };
...
    MainPage::MainPage()
    {
        InitializeComponent();
        MainPage::current = *this;
        SampleTitle().Text(FEATURE_NAME());
    }
...
}

// SampleConfiguration.cpp
#include "pch.h"
#include "MainPage.h"

using namespace winrt;
using namespace SDKTemplate;

hstring implementation::MainPage::FEATURE_NAME()
{
    return L"Clipboard C++/WinRT Sample";
}
```

Veillez aussi à supprimer les corps de fonction existants de `MainPage.cpp` pour **MainPage::Current()** et **MainPage::FEATURE_NAME()** , car nous définissons maintenant ces méthodes ailleurs.

Comme vous pouvez le voir, **MainPage::Current** est déclarée comme étant de type **SDKTemplate::MainPage**, qui est le type projeté. Elle n’est pas du type **SDKTemplate::Implementation::MainPage**, qui est le type d’implémentation. Le type projeté est celui qui est conçu pour être consommé dans le projet pour l’interopérabilité XAML ou dans les fichiers binaires. Le type d’implémentation est ce que vous utilisez pour implémenter les fonctions que vous avez exposées sur votre type projeté. Étant donné que la déclaration de **MainPage::Current** (dans `MainPage.h`) apparaît dans l’espace de noms de l’implémentation (**winrt::SDKTemplate::implementation**), une **MainPage** non qualifiée aurait fait référence au type d’implémentation. Nous qualifions donc avec **SDKTemplate::** pour indiquer explicitement que **MainPage::Current** doit être une instance du type projeté **winrt::SDKTemplate::MainPage**.

Dans le constructeur, quelques points relatifs à `MainPage::current = *this;` nécessitent une explication.
- Quand vous utilisez le pointeur `this` à l’intérieur d’un membre du type d’implémentation, le pointeur `this` est bien sûr *un pointeur vers le type d’implémentation*.
- Pour convertir le pointeur `this` en type projeté correspondant, déréférencez-le. À condition de générer votre type d’implémentation à partir de l’IDL (comme nous le faisons ici), le type d’implémentation a un opérateur de conversion qui convertit en son type projeté. C’est pourquoi l’affectation fonctionne ici.

Pour plus d’informations sur ces éléments, consultez [Instanciation et retour des types d’implémentation et interfaces](/windows/uwp/cpp-and-winrt-apis/author-apis#instantiating-and-returning-implementation-types-and-interfaces).

Vous voyez aussi `SampleTitle().Text(FEATURE_NAME());` dans le constructeur. La partie `SampleTitle()` est un appel à une fonction d’accesseur simple nommée **SampleTitle**, qui retourne le **TextBlock** que nous avons ajouté au XAML. Chaque fois que vous utilisez `x:Name` pour un élément XAML, le compilateur XAML génère un accesseur pour vous, nommé pour l’élément. La partie `.Text(...)` appelle la fonction de mutateur **Text** sur l’objet **TextBlock** que l’accesseur **SampleTitle** a retourné. `FEATURE_NAME()` appelle notre fonction d’accesseur **MainPage::FEATURE_NAME** statique pour retourner le littéral de chaîne. En même temps, cette ligne de code définit la propriété **Text** du **TextBlock** nommé *SampleTitle*.

Notez que dans la mesure où les chaînes sont de type large (« wide ») dans Windows Runtime, pour porter un littéral de chaîne, nous le préfixons du préfixe d’encodage des caractères larges, `L`. Par exemple, nous changeons "un littéral de chaîne" en L"un littéral de chaîne". Consultez également [Littéraux de chaîne larges](/cpp/cpp/string-and-character-literals-cpp#wide-string-literals).

#### <a name="scenarios"></a>**Scénarios**

Voici le code C# concerné que nous devons porter.

```csharp
// MainPage.xaml.cs
...
public sealed partial class MainPage : Page
{
...
    public List<Scenario> Scenarios
    {
        get { return this.scenarios; }
    }
...
}
...

// SampleConfiguration.cs
...
public partial class MainPage : Page
{
...
    List<Scenario> scenarios = new List<Scenario>
    {
        new Scenario() { Title = "Copy and paste text", ClassType = typeof(CopyText) },
        new Scenario() { Title = "Copy and paste an image", ClassType = typeof(CopyImage) },
        new Scenario() { Title = "Copy and paste files", ClassType = typeof(CopyFiles) },
        new Scenario() { Title = "Other Clipboard operations", ClassType = typeof(OtherScenarios) }
    };
...
}
...
```

De notre investigation précédente, nous savons que cette collection d’objets **Scenario** va être affichée dans une **ListBox**. En C++/WinRT, il y a des limites sur *le type de collection* que nous pouvons affecter à la propriété **ItemsSource** d’un contrôle d’éléments. La collection doit être un vecteur ou un vecteur observable, et ses éléments doivent être un des éléments suivants.

- Des classes de runtime ou
- [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable).

Pour le cas de **IInspectable**, si les éléments ne sont pas eux-mêmes des classes de runtime, ces éléments doivent être d’un type qui peut être « boxed » et « unboxed » vers et depuis [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable). Cela signifie qu’ils doivent être des types Windows Runtime (consultez [Conversions boxing et unboxing de valeurs scalaires vers IInspectable](/windows/uwp/cpp-and-winrt-apis/boxing)).

Pour cette étude de cas, nous n’avons pas fait de **Scenario** une classe de runtime. Cela reste cependant une option raisonnable. Il y aura des cas dans votre propre travail de portage où une classe de runtime sera sans aucun doute la voie à suivre. Par exemple, si vous avez besoin de rendre le type d’élément *observable* (consultez [Contrôles XAML ; liaison à une propriété C++/WinRT](/windows/uwp/cpp-and-winrt-apis/binding-property)) ou si, pour une raison quelconque, l’élément doit avoir des méthodes et qu’il soit davantage que simplement un ensemble de membres de données.

Étant donné que dans cette procédure pas à pas, nous n’allons *pas* utiliser une classe de runtime pour le type **Scenario**, nous devons penser à la conversion boxing. Si nous avions fait de **Scenario** un `struct` C++ normal, nous ne pourrions pas lui appliquer une conversion boxing. Mais nous avons déclaré **Scenario** en tant que `struct` dans l’IDL : nous *pouvons* donc lui appliquer une conversion boxing.

Il nous reste donc à choisir entre effectuer par avance une conversion boxing de **Scenario**, ou attendre jusqu’au moment où nous sommes prêts à faire une affectation à **ItemsSource** et une conversion boxing des éléments juste-à-temps. Voici quelques considérations relatives à ces deux options.

- Conversion boxing par avance. Pour cette option, notre membre de données est une collection d’éléments **IInspectable** prête à être affectée à l’interface utilisateur. Lors de l’initialisation, nous effectuons une conversion boxing des objets **Scenario** vers ce membre de données. Nous n’avons besoin que d’une seule copie de cette collection, mais nous devons effectuer une conversion unboxing d’un élément chaque fois que nous devons lire ses champs.
- Conversion boxing juste-à-temps. Pour cette option, notre membre de données est une collection de **Scenario**. Quand vient le moment de l’affectation à l’interface utilisateur, nous effectuons une conversion boxing des objets **Scenario** du membre de données en une nouvelle collection d’éléments **IInspectable**. Nous pouvons lire les champs des éléments du membre de données sans la conversion unboxing, mais nous avons besoin de deux copies de la collection.

Comme vous pouvez le voir, pour une petite collection comme celle-ci, les avantages et les inconvénients s’équilibrent globalement. Pour cette étude de cas, nous allons donc utiliser l’option juste-à-temps.

Le membre **scenarios** est un champ de **MainPage**, défini et initialisé dans `SampleConfiguration.cs`. **Scenarios** est une propriété en lecture seule de **MainPage**, définie dans `MainPage.xaml.cs` (et implémentée de façon à retourner simplement le champ **scenarios**). Nous allons faire quelque chose de similaire dans projet C++/WinRT ; cependant, nous allons rendre les deux membres statiques (dans la mesure où nous n’avons besoin que d’une seule instance dans l’application et que nous pouvons donc y accéder sans avoir besoin d’une instance de classe). Nous allons les nommer respectivement *scenariosInner* et *scenarios*. Nous allons déclarer *scenariosInner* dans `MainPage.h`. Étant donné qu’il a une durée de stockage statique, nous allons le définir/initialiser dans un fichier `.cpp` (`SampleConfiguration.cpp` dans le cas présent).

Modifiez `MainPage.h` et `SampleConfiguration.cpp` pour qu’ils correspondent à ce qui figure ci-dessous.

```cppwinrt
// MainPage.h
...
struct MainPage : MainPageT<MainPage>
{
...
    static Windows::Foundation::Collections::IVector<Scenario> scenarios() { return scenariosInner; }
...
private:
    static winrt::Windows::Foundation::Collections::IVector<Scenario> scenariosInner;
...
};

// SampleConfiguration.cpp
...
using namespace Windows::Foundation::Collections;
...
IVector<Scenario> implementation::MainPage::scenariosInner = winrt::single_threaded_observable_vector<Scenario>(
{
    Scenario{ L"Copy and paste text", xaml_typename<SDKTemplate::CopyText>() },
    Scenario{ L"Copy and paste an image", xaml_typename<SDKTemplate::CopyImage>() },
    Scenario{ L"Copy and paste files", xaml_typename<SDKTemplate::CopyFiles>() },
    Scenario{ L"History and roaming", xaml_typename<SDKTemplate::HistoryAndRoaming>() },
    Scenario{ L"Other Clipboard operations", xaml_typename<SDKTemplate::OtherScenarios>() },
});
```

Veillez aussi à supprimer le corps de la fonction existante de `MainPage.cpp` pour **MainPage::Scenarios()** , car nous définissons maintenant cette méthode dans le fichier d’en-tête.

Comme vous pouvez le voir, dans `SampleConfiguration.cpp`, nous initialisons le membre de données statique *scenariosInner* en appelant une fonction helper C++/WinRT nommée [winrt::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector). Cette fonction crée un objet de collection Windows Runtime pour nous et la retourne en tant qu’interface [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_). Étant donné que dans cet exemple, la collection n’est pas *observable* (elle n’a pas besoin de l’être, car elle n’ajoute pas et ne supprime pas d’éléments après l’initialisation), nous aurions donc pu choisir à la place d’appeler [winrt::single_threaded_vector](/uwp/cpp-ref-for-winrt/single-threaded-vector). Cette fonction retourne la collection sous la forme d’une interface [**IVector**](/uwp/api/windows.foundation.collections.ivector_t_).

Pour plus d’informations sur les collections et l’établissement d’une liaison à celles-ci, consultez [Contrôles d’éléments XAML ; liaison à une collection C++/WinRT](/windows/uwp/cpp-and-winrt-apis/binding-collection) et [Collections avec C++/WinRT](/windows/uwp/cpp-and-winrt-apis/collections).

Le code d’initialisation que vous venez d’ajouter référence des types qui ne se trouvent pas encore dans le projet (par exemple **winrt::SDKTemplate::CopyText**). Pour remédier à cela, continuons et ajoutons cinq nouvelles pages XAML vierges au projet.

#### <a name="add-five-new-blank-xaml-pages"></a>Ajouter cinq nouvelles pages XAML vides

Ajoutez un nouvel élément **XAML** > **Page vide (C++/WinRT)**   au projet (vérifiez qu’il s’agit bien du modèle d’élément **Page vide (C++/WinRT)** et non pas du modèle **Page vide**). Nommez-le  `CopyText`. La nouvelle page XAML est définie dans l’espace de noms **SDKTemplate**, qui est ce que nous voulons.

Répétez le processus ci-dessus à quatre reprises, puis nommez les pages XAML `CopyImage`, `CopyFiles`, `HistoryAndRoaming` et `OtherScenarios`.

Vous pouvez maintenant regénérer l’application si vous le souhaitez.

#### <a name="notifyuser"></a>**NotifyUser**

Dans le projet C#, vous trouverez l’implémentation de la méthode **MainPage.NotifyUser** dans `MainPage.xaml.cs`. **MainPage.NotifyUser** a une dépendance vis-à-vis de **MainPage.UpdateStatus**, et cette méthode a à son tour des dépendances vis-à-vis d’éléments XAML que nous n’avons pas encore portés. Pour le moment, nous allons simplement utiliser avec définition ultérieure une méthode **UpdateStatus** dans le projet C++/WinRT, et nous en ferons le portage ultérieurement.

Voici le code C# concerné que nous devons porter.

```csharp
// MainPage.xaml.cs
...
public void NotifyUser(string strMessage, NotifyType type)
if (Dispatcher.HasThreadAccess)
{
    UpdateStatus(strMessage, type);
}
else
{
    var task = Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => UpdateStatus(strMessage, type));
}
private void UpdateStatus(string strMessage, NotifyType type) { ... }{
...
```

**NotifyUser** utilise l’énumération [**Windows.UI.Core.CoreDispatcherPriority**](/uwp/api/windows.ui.core.coredispatcherpriority). En C++/WinRT, chaque fois que vous voulez utiliser un type à partir d’un espace de noms Windows, vous devez inclure le fichier d’en-tête de l’espace de noms Windows C++/WinRT correspondant (pour plus d’informations à ce sujet, consultez [Bien démarrer avec C++/WinRT](/windows/uwp/cpp-and-winrt-apis/get-started)). Dans le cas présent, comme vous le verrez dans le code ci-dessous, l’en-tête est `winrt/Windows.UI.Core.h` et nous allons l’inclure dans `pch.h`.

**UpdateStatus** est privé. Nous allons donc en faire une méthode privée sur notre type d’implémentation **MainPage**. **UpdateStatus** n’est pas destiné à être appelé sur la classe de runtime : nous ne le déclarons donc pas dans l’IDL.

Après le portage de **MainPage.NotifyUser** et l’utilisation avec définition ultérieure de **MainPage.UpdateStatus**, voici ce que nous avons dans le projet C++/WinRT. Après ce code, nous allons examiner certains détails.

```cppwinrt
// pch.h
...
#include <winrt/Windows.UI.Core.h>
...

// MainPage.h
...
struct MainPage : MainPageT<MainPage>
{
...
    void NotifyUser(hstring const& strMessage, SDKTemplate::NotifyType const& type);
...
private:
    void UpdateStatus(hstring const& strMessage, SDKTemplate::NotifyType const& type);
...
};

// MainPage.cpp
...
void MainPage::NotifyUser(hstring const& strMessage, SDKTemplate::NotifyType const& type)
{
    if (Dispatcher().HasThreadAccess())
    {
        UpdateStatus(strMessage, type);
    }
    else
    {
        Dispatcher().RunAsync(Windows::UI::Core::CoreDispatcherPriority::Normal, [strMessage, type, this]()
            {
                UpdateStatus(strMessage, type);
            });
    }
}
void MainPage::UpdateStatus(hstring const& strMessage, SDKTemplate::NotifyType const& type)
{
    throw hresult_not_implemented();
}
...
```

En C#, vous *utilisez des points dans* les propriétés imbriquées. Ainsi, le type C# **MainPage** peut accéder à sa propre propriété **Dispatcher** avec la syntaxe `Dispatcher`. C# peut alors *placer des points dans* cette valeur avec une syntaxe comme `Dispatcher.HasThreadAccess`. En C++/WinRT, les propriétés sont implémentées en tant que fonctions d’accesseur : la syntaxe diffère donc seulement en cela que vous ajoutez des parenthèses pour chaque appel de fonction.

|C#|C++/WinRT|
|-|-|
|`Dispatcher.HasThreadAccess`|`Dispatcher().HasThreadAccess()`|

Quand la version C# de **NotifyUser** appelle [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync), elle implémente le délégué du rappel asynchrone en tant que fonction lambda. La version C++/WinRT fait la même chose, mais la syntaxe est un peu différente. En C++/WinRT, nous *capturons* les deux paramètres que nous allons utiliser ainsi que le pointeur `this` (car nous allons appeler une fonction membre). Vous trouverez plus d’informations sur l’implémentation des délégués en tant que lambdas ainsi que des exemples de code dans la rubrique [Gérer des événements en utilisant des délégués en C++/WinRT](/windows/uwp/cpp-and-winrt-apis/handle-events). En outre, nous pouvons ignorer la partie `var task =` dans ce cas particulier. Nous n’attendons pas l’objet asynchrone retourné : il n’est donc pas nécessaire de le stocker. 

### <a name="implement-the-remaining-mainpage-members"></a>Implémenter les membres restants de **MainPage**

Créons une liste complète des membres de **MainPage** (implémentée dans `MainPage.xaml.cs` et `SampleConfiguration.cs`) de façon à voir ceux que nous avons portés jusqu’à présent et ceux qui ne le sont pas encore.

|Membre|Accès|État|
|-|-|-|
|Constructeur de **MainPage**|`public`|Portage effectué|
|Propriété **Current**|`public`|Portage effectué|
|Propriété **FEATURE_NAME**|`public`|Portage effectué|
|Propriété **IsClipboardContentChangedEnabled**|`public`|Portage non commencé|
|Propriété **Scenarios**|`public`|Portage effectué|
|Méthode **BuildClipboardFormatsOutputString**|`public`|Portage non commencé|
|Méthode **DisplayToast**|`public`|Portage non commencé|
|Méthode **EnableClipboardContentChangedNotifications**|`public`|Portage non commencé|
|Méthode **NotifyUser**|`public`|Portage effectué|
|Méthode **OnNavigatedTo**|`protected`|Portage non commencé|
|Champ **isApplicationWindowActive**|`private`|Portage non commencé|
|Champ **needToPrintClipboardFormat**|`private`|Portage non commencé|
|Champ **scenarios**|`private`|Portage effectué|
|Méthode **Button_Click**|`private`|Portage non commencé|
|Méthode **DisplayChangedFormats**|`private`|Portage non commencé|
|Méthode **Footer_Click**|`private`|Portage non commencé|
|Méthode **HandleClipboardChanged**|`private`|Portage non commencé|
|Méthode **OnClipboardChanged**|`private`|Portage non commencé|
|Méthode **OnWindowActivated**|`private`|Portage non commencé|
|Méthode **ScenarioControl_SelectionChanged**|`private`|Portage non commencé|
|Méthode **UpdateStatus**|`private`|Utilisé avec définition ultérieure|

Nous parlerons des membres qui ne sont pas encore portés dans les sous-sections suivantes.

> [!NOTE]
> De temps en temps, nous allons rencontrer dans le code source des références à des éléments d’interface utilisateur dans le balisage XAML (dans `MainPage.xaml`). Quand nous rencontrons ces références, nous allons temporairement les contourner en ajoutant des éléments d’espace réservé simples au code XAML. De cette façon, le projet pourra être généré correctement après chaque sous-section. L’alternative est de résoudre les références en copiant la *totalité* du contenu de `MainPage.xaml` depuis le projet C# dans le projet C++/WinRT. Mais si nous faisons cela, il se passera beaucoup de temps avant de pouvoir faire une pause et une nouvelle génération (ce qui risque de ne pas faire apparaître les fautes de frappe ou d’autres erreurs que nous faisons en cours de route).
>
> Une fois que nous avons terminé le portage du code impératif pour la classe **MainPage**, nous allons *ensuite* copier le contenu du fichier XAML et rester confiant quant au fait que le projet va toujours être généré correctement.

#### <a name="isclipboardcontentchangedenabled"></a>**IsClipboardContentChangedEnabled**

Il s’agit d’une propriété C# get-set qui a par défaut la valeur `false`. Elles est membre de **MainPage** et est définie dans `SampleConfiguration.cs`.

Pour C++/WinRT, nous avons besoin d’une fonction d’accesseur, d’une fonction de mutateur et d’un membre de données stocké sous forme de champ. Comme **IsClipboardContentChangedEnabled** représente l’état de l’un des scénarios de l’exemple, au lieu de l’état de **MainPage**, nous allons créer les nouveaux membres sur un nouveau type d’utilitaire appelé **SampleState**. Nous allons implémenter cela dans notre fichier de code source `SampleConfiguration.cpp` et nous allons rendre les membres `static` (dans la mesure où nous n’avons besoin que d’une seule instance dans l’application et que nous pouvons donc y accéder sans avoir besoin d’une instance de classe).

Pour accompagner notre fichier `SampleConfiguration.cpp` dans le projet C++/WinRT, ajoutez un nouvel élément **Visual C++**  > **Code** > **Fichier d’en-tête (.h)** avec le nom `SampleConfiguration.h`. Modifiez `SampleConfiguration.h` et `SampleConfiguration.cpp` pour qu’ils correspondent à ce qui figure ci-dessous.

```cppwinrt
// SampleConfiguration.h
#pragma once 
#include "pch.h"

namespace winrt::SDKTemplate
{
    struct SampleState
    {
        static bool IsClipboardContentChangedEnabled();
        static void IsClipboardContentChangedEnabled(bool checked);
    private:
        static bool isClipboardContentChangedEnabled;
    };
}

// SampleConfiguration.cpp
...
#include "SampleConfiguration.h"
...
bool SampleState::isClipboardContentChangedEnabled = false;
...
bool SampleState::IsClipboardContentChangedEnabled()
{
    return isClipboardContentChangedEnabled;
}
void SampleState::IsClipboardContentChangedEnabled(bool checked)
{
    if (isClipboardContentChangedEnabled != checked)
    {
        isClipboardContentChangedEnabled = checked;
    }
}
```

Là encore, un champ avec un stockage `static` (comme **SampleState::isClipboardContentChangedEnabled**) doit être défini une seule fois dans l’application, et un fichier `.cpp` est un bon emplacement pour cela (`SampleConfiguration.cpp` dans le cas présent).

#### <a name="buildclipboardformatsoutputstring"></a>**BuildClipboardFormatsOutputString**

Cette méthode est un membre public de **MainPage**, et elle est définie dans `SampleConfiguration.cs`.

```csharp
// SampleConfiguration.cs
...
public string BuildClipboardFormatsOutputString()
{
    DataPackageView clipboardContent = Windows.ApplicationModel.DataTransfer.Clipboard.GetContent();
    StringBuilder output = new StringBuilder();

    if (clipboardContent != null && clipboardContent.AvailableFormats.Count > 0)
    {
        output.Append("Available formats in the clipboard:");
        foreach (var format in clipboardContent.AvailableFormats)
        {
            output.Append(Environment.NewLine + " * " + format);
        }
    }
    else
    {
        output.Append("The clipboard is empty");
    }
    return output.ToString();
}
...
```

En C++/WinRT, nous allons faire de **BuildClipboardFormatsOutputString** une méthode statique publique de **SampleState**. Nous pouvons la rendre `static`, car elle n’accède à aucun membre d’instance.

Pour utiliser les types **Clipboard** and **DataPackageView** en C++/WinRT, nous devons inclure le fichier d’en-tête de l’espace de noms Windows C++/WinRT `winrt/Windows.ApplicationModel.DataTransfer.h`.

En C#, la propriété **DataPackageView.AvailableFormats** est une **IReadOnlyList** . nous pouvons donc accéder à la propriété **Count** de celle-ci. En C++/WinRT, la fonction d’accesseur **DataPackageView::AvailableFormats** retourne une **IVectorView**, qui a une fonction d’accesseur **Size** que nous pouvons appeler.

Pour porter l’utilisation du type C# **System.Text.StringBuilder**, nous allons utiliser le type standard C++ [**std::wostringstream**](/cpp/standard-library/sstream-typedefs#wostringstream). Ce type est un flux de sortie pour les chaînes larges (et pour l’utiliser, nous devons inclure le fichier d’en-tête `sstream`). Au lieu d’utiliser une méthode **Append** comme vous le faites avec un **StringBuilder**, vous utilisez l’[opérateur d’insertion](/cpp/standard-library/using-insertion-operators-and-controlling-format) (`<<`) avec un flux de sortie comme **wostringstream**. Pour plus d’informations, consultez [iostream (programmation)](/cpp/standard-library/iostream-programming) et [Mise en forme des chaînes C++/WinRT](/windows/uwp/cpp-and-winrt-apis/strings#formatting-strings).

Le code C# construit un**StringBuilder** avec le mot clé `new`. En C#, les objets sont des types référence par défaut, déclarés sur le tas avec `new`. Dans le standard C++moderne, les objets sont des types valeur par défaut, déclarés sur la pile (sans utiliser `new`). Nous portons donc `StringBuilder output = new StringBuilder();` en C++/WinRT aussi simplement que `std::wostringstream output;`.

Le mot clé C# `var` demande au compilateur d’inférer un type. Vous portez `var` en `auto` dans C++/WinRT. Cependant, dans C++/WinRT, il existe des cas où (afin d’éviter les copies) vous voulez une *référence* à un type inféré (ou déduit), et vous exprimez cela avec `auto&`. Il existe également des cas où vous voulez un type spécial de référence qui se lie correctement, qu’il soit initialisé avec une *lvalue* ou avec une *rvalue*. Vous exprimez cela avec `auto&&`. Il s’agit de la forme que vous voyez utilisée dans la boucle `for` du code porté ci-dessous. Pour en savoir plus sur les valeurs *lvalue* et *rvalue*, consultez [Catégories de valeurs et références à celles-ci](/windows/uwp/cpp-and-winrt-apis/cpp-value-categories).

Modifiez `pch.h`, `SampleConfiguration.h` et `SampleConfiguration.cpp` pour qu’ils correspondent à ce qui figure ci-dessous.

```cppwinrt
// pch.h
...
#include <sstream>
#include "winrt/Windows.ApplicationModel.DataTransfer.h"
...

// SampleConfiguration.h
...
static hstring BuildClipboardFormatsOutputString();
...

// SampleConfiguration.cpp
...
using namespace Windows::ApplicationModel::DataTransfer;
...
hstring SampleState::BuildClipboardFormatsOutputString()
{
    DataPackageView clipboardContent{ Clipboard::GetContent() };
    std::wostringstream output;

    if (clipboardContent && clipboardContent.AvailableFormats().Size() > 0)
    {
        output << L"Available formats in the clipboard:";
        for (auto&& format : clipboardContent.AvailableFormats())
        {
            output << std::endl << L" * " << std::wstring_view(format);
        }
    }
    else
    {
        output << L"The clipboard is empty";
    }

    return hstring{ output.str() };
}
```

> [!NOTE]
> La syntaxe de la ligne de code `DataPackageView clipboardContent{ Clipboard::GetContent() };` utilise une fonctionnalité du standard C++ moderne appelée *initialisation uniforme*, avec son utilisation caractéristique des accolades au lieu d’un signe `=`. Cette syntaxe indique clairement qu’une initialisation prend place, et non pas une affectation. Si vous préférez la forme de la syntaxe qui *ressemble* à une affectation (mais qui n’en est pas une), vous pouvez remplacer la syntaxe ci-dessus par son équivalent `DataPackageView clipboardContent = Clipboard::GetContent();`. Il est cependant judicieux de bien maîtriser les deux façons d’exprimer l’initialisation, car il est probable que vous verrez les deux fréquemment utilisées dans le code que vous rencontrerez.

#### <a name="displaytoast"></a>**DisplayToast**

**DisplayToast** est une méthode statique publique de la classe C# **MainPage**, que vous trouverez définie dans `SampleConfiguration.cs`. En C++/WinRT, nous allons en faire une méthode statique publique de **SampleState**.

Nous avons déjà vu la plupart des informations détaillées et des techniques pertinentes pour le portage de cette méthode. Un nouvel élément à noter est que vous portez un littéral de chaîne textuelle C# (`@`) en un [littéral de chaîne brute](/cpp/cpp/string-and-character-literals-cpp#raw-string-literals-c11) C++ standard (`LR`).

En outre, quand vous référencez les types [**ToastNotification**](/uwp/api/windows.ui.notifications.toastnotification) et [**XmlDocument**](/uwp/api/windows.data.xml.dom.xmldocument) en C++/WinRT, vous pouvez les qualifier par un nom d’espace de noms ou vous pouvez modifier `SampleConfiguration.cpp` et ajouter des directives `using namespace`, comme dans l’exemple suivant.

```cppwinrt
using namespace Windows::UI::Notifications;
```

Vous avez le même choix quand vous référencez le type [**XmlDocument**](/uwp/api/windows.data.xml.dom.xmldocument) et chaque fois que vous référencez un autre type Windows Runtime.

À part pour ces éléments, suivez simplement les mêmes instructions que celles que vous avez suivies précédemment pour effectuer les étapes suivantes.

- Déclarez la méthode dans `SampleConfiguration.h` et définissez-la dans `SampleConfiguration.cpp`.
- Modifiez `pch.h` pour y inclure tous les fichiers d’en-tête de l’espace de noms Windows C++/WinRT nécessaires.
- Construisez les objets C++/WinRT sur la pile et non pas sur le tas.
- Remplacez les appels aux accesseurs Get de propriété par la syntaxe d’appel de fonction (`()`).

Une cause très courante d’erreurs du compilateur/éditeur de liens est d’avoir oublié d’inclure les fichiers d’en-tête de l’espace de noms Windows C++/WinRT dont vous avez besoin. Pour plus d’informations sur une erreur possible, consultez [Pourquoi l’éditeur de liens retourne-t-il une erreur « LNK2019 : symbole externe non résolu » ?](/windows/uwp/cpp-and-winrt-apis/faq#why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error).

Si vous voulez suivre la procédure pas à pas et porter **DisplayToast** vous-même, vous pouvez comparer vos résultats au code de la version C++/WinRT de l’exemple de code source Clipboard que vous avez téléchargé (il se trouve dans [`Windows-universal-samples/Samples/Clipboard/cppwinrt`](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/Clipboard/cppwinrt)`/Clipboard.sln`).

#### <a name="enableclipboardcontentchangednotifications"></a>**EnableClipboardContentChangedNotifications**

**EnableClipboardContentChangedNotifications** est une méthode statique publique de la classe C# **MainPage**, et elle est définie dans `SampleConfiguration.cs`.

```csharp
// SampleConfiguration.cs
...
public bool EnableClipboardContentChangedNotifications(bool enable)
{
    if (IsClipboardContentChangedEnabled == enable)
    {
        return false;
    }

    IsClipboardContentChangedEnabled = enable;
    if (enable)
    {
        Clipboard.ContentChanged += OnClipboardChanged;
        Window.Current.Activated += OnWindowActivated;
    }
    else
    {
        Clipboard.ContentChanged -= OnClipboardChanged;
        Window.Current.Activated -= OnWindowActivated;
    }
    return true;
}
...
private void OnClipboardChanged(object sender, object e) { ... }
private void OnWindowActivated(object sender, WindowActivatedEventArgs e) { ... }
...
```

En C++/WinRT, nous allons en faire une méthode statique publique de **SampleState**.

En C#, vous utilisez la syntaxe d’opérateur `+=` et `-=` pour inscrire et révoquer des délégués de gestion d’événements. En C++/WinRT, vous disposez de plusieurs options syntaxiques pour inscrire/révoquer un délégué, comme décrit dans [Gérer des événements en utilisant des délégués en C++/WinRT](/windows/uwp/cpp-and-winrt-apis/handle-events). La forme générale consiste cependant à inscrire et à révoquer avec un appel à une paire de fonctions nommées pour l’événement. Pour inscrire, vous passez votre délégué à la fonction d’inscription et vous récupérez en retour un jeton de révocation ([**winrt::event_token**](/uwp/cpp-ref-for-winrt/event-token)). Pour révoquer, vous passez ce jeton à la fonction de révocation. Dans le cas présent, le gestionnaire est statique et (comme vous pouvez le voir dans le code suivant) la syntaxe de l’appel de la fonction est simple.

Le type **object** apparaît dans les signatures du gestionnaire d’événements C#. Dans le langage C#, **object** est un [alias](/dotnet/csharp/language-reference/builtin-types/reference-types) pour le type [**System.Object**](/dotnet/api/system.object) de .NET. L’équivalent en C++/WinRT est [**winrt::Windows::Foundation::IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable). Vous verrez donc **IInspectable** dans les gestionnaires d’événements C++/WinRT.

Modifiez `SampleConfiguration.h` et `SampleConfiguration.cpp` pour qu’ils correspondent à ce qui figure ci-dessous.

```cppwinrt
// SampleConfiguration.h
...
private:
    static event_token clipboardContentChangedToken;
    static event_token activatedToken;
    static void OnClipboardChanged(Windows::Foundation::IInspectable const& sender, Windows::Foundation::IInspectable const& e);
    static void OnWindowActivated(Windows::Foundation::IInspectable const& sender, Windows::UI::Core::WindowActivatedEventArgs const& e);
...

// SampleConfiguration.cpp
...
using namespace Windows::Foundation;
using namespace Windows::UI::Core;
using namespace Windows::UI::Xaml;
...
event_token SampleState::clipboardContentChangedToken;
event_token SampleState::activatedToken;
...
bool SampleState::EnableClipboardContentChangedNotifications(bool enable)
{
    if (isClipboardContentChangedEnabled == enable)
    {
        return false;
    }

    IsClipboardContentChangedEnabled(enable);
    if (enable)
    {
        clipboardContentChangedToken = Clipboard::ContentChanged(OnClipboardChanged);
        activatedToken = Window::Current().Activated(OnWindowActivated);
    }
    else
    {
        Clipboard::ContentChanged(clipboardContentChangedToken);
        Window::Current().Activated(activatedToken);
    }
    return true;
}
void SampleState::OnClipboardChanged(IInspectable const&, IInspectable const&){}
void SampleState::OnWindowActivated(IInspectable const&, WindowActivatedEventArgs const& e){}
```

Laissez les délégués de gestion des événements eux-mêmes (**OnClipboardChanged** et **OnWindowActivated**) en tant que stubs pour l’instant. Ils se trouvent déjà sur la liste des membres à porter : nous y viendrons donc dans les sous-sections ultérieures.

#### <a name="onnavigatedto"></a>**OnNavigatedTo**

**OnNavigatedTo** est une méthode protégée de la classe C# **MainPage** et elle est définie dans `MainPage.xaml.cs`. La voici, avec la **ListBox** XAML qu’elle référence.

```xaml
<!-- MainPage.xaml -->
...
<ListBox x:Name="ScenarioControl" ... />
...
```

```csharp
// MainPage.xaml.cs
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    // Populate the scenario list from the SampleConfiguration.cs file
    var itemCollection = new List<Scenario>();
    int i = 1;
    foreach (Scenario s in scenarios)
    {
        itemCollection.Add(new Scenario { Title = $"{i++}) {s.Title}", ClassType = s.ClassType });
    }
    ScenarioControl.ItemsSource = itemCollection;

    if (Window.Current.Bounds.Width < 640)
    {
        ScenarioControl.SelectedIndex = -1;
    }
    else
    {
        ScenarioControl.SelectedIndex = 0;
    }
}
```

C’est une méthode importante et intéressante, car c’est ici que notre collection d’objets **Scenario** est affectée à l’interface utilisateur. Le code C# génère une [**System.Collections.Generic.List**](/dotnet/api/system.collections.generic.list-1) d’objets **Scenario** et l’affecte à la propriété [**ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) d’une **ListBox** (qui est un contrôle d’éléments). En C#, nous utilisons une [interpolation de chaîne](/dotnet/csharp/language-reference/tokens/interpolated) pour générer le titre de chaque objet **Scenario** (notez l’utilisation du caractère spécial `$`).

En C++/WinRT, nous allons faire de **OnNavigatedTo** une méthode publique de **MainPage**. Nous allons aussi ajouter un élément stub **ListBox** au code XAML pour que les builds réussissent. Après le code, nous allons examiner certains détails.

```xaml
<!-- MainPage.xaml -->
...
<StackPanel ...>
    ...
    <ListBox x:Name="ScenarioControl" />
</StackPanel>
...
```

```cppwinrt
// MainPage.h
...
void OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
...

// MainPage.cpp
...
using namespace winrt::Windows::UI::Xaml;
using namespace winrt::Windows::UI::Xaml::Navigation;
...
void MainPage::OnNavigatedTo(NavigationEventArgs const& /* e */)
{
    auto itemCollection = winrt::single_threaded_observable_vector<IInspectable>();
    int i = 1;
    for (auto s : MainPage::scenarios())
    {
        s.Title = winrt::to_hstring(i++) + L") " + s.Title;
        itemCollection.Append(winrt::box_value(s));
    }
    ScenarioControl().ItemsSource(itemCollection);

    if (Window::Current().Bounds().Width < 640)
    {
        ScenarioControl().SelectedIndex(-1);
    }
    else
    {
        ScenarioControl().SelectedIndex(0);
    }
}
...
```

Là encore, nous appelons la fonction [winrt::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector), mais cette fois-ci pour créer une collection de [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable). Ceci faisait partie de la décision que nous avons prise d’effectuer la conversion boxing de nos objets **Scenario** selon un mode juste-à-temps.

Et, à la place de l’utilisation ici en C# de l’[interpolation de chaîne](/dotnet/csharp/language-reference/tokens/interpolated), nous utilisons une combinaison de la fonction [**to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring) et de l’[opérateur de concaténation ](/uwp/cpp-ref-for-winrt/hstring#operator-concatenation-operator) de **winrt::hstring**.

#### <a name="isapplicationwindowactive"></a>**isApplicationWindowActive**

En C#, **isApplicationWindowActive** est un champ `bool` privé simple appartenant à la classe **MainPage** et il est défini dans `SampleConfiguration.cs`. Sa valeur par défaut est `false`. En C++/WinRT, nous allons en faire un champ statique public de **SampleState** (pour les raisons que nous avons déjà décrites) dans les fichiers `SampleConfiguration.h` et `SampleConfiguration.cpp`, avec la même valeur par défaut.

Nous avons déjà vu comment déclarer, définir et initialiser un champ statique. Pour un actualisateur, revenez à ce que nous avons fait avec le champ **isClipboardContentChangedEnabled**, puis faites de même avec **isApplicationWindowActive**.

#### <a name="needtoprintclipboardformat"></a>**needToPrintClipboardFormat**

Même modèle que **isApplicationWindowActive** (regardez le titre immédiatement avant celui-ci).

#### <a name="button_click"></a>**Button_Click**

**Button_Click** est une méthode (de gestion des événements) privée de la classe C# **MainPage** et elle est définie dans `MainPage.xaml.cs`. La voici, avec la **SplitView** XAML qu’elle référence.

```xaml
<!-- MainPage.xaml -->
...
<SplitView x:Name="Splitter" ... />
...
```

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    Splitter.IsPaneOpen = !Splitter.IsPaneOpen;
}
```

Et voici l’équivalent, porté en C++/WinRT. Notez que dans la version C++/WinRT, le gestionnaire d’événements est `public` (comme vous pouvez le voir, vous le déclarez *avant* les déclarations `private:`).

```xaml
<!-- MainPage.xaml -->
...
<StackPanel ...>
    ...
    <SplitView x:Name="Splitter" />
</StackPanel>
...
```

```cppwinrt
// MainPage.h
...
    void Button_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& e);
private:
...

// MainPage.cpp
void MainPage::Button_Click(Windows::Foundation::IInspectable const& /* sender */, Windows::UI::Xaml::RoutedEventArgs const& /* e */)
{
    Splitter().IsPaneOpen(!Splitter().IsPaneOpen());
}
```

#### <a name="displaychangedformats"></a>**DisplayChangedFormats**

En C#, **DisplayChangedFormats** est une méthode privée appartenant à la classe **MainPage** et elle est définie dans `SampleConfiguration.cs`.

```csharp
private void DisplayChangedFormats()
{
    string output = "Clipboard content has changed!" + Environment.NewLine;
    output += BuildClipboardFormatsOutputString();
    NotifyUser(output, NotifyType.StatusMessage);
}
```

En C++/WinRT, nous allons en faire un champ statique privé de **SampleState** (elle n’accède à aucun membre d’instance), dans les fichiers `SampleConfiguration.h` et `SampleConfiguration.cpp`. Le code C# de cette méthode n’utilise pas **System.Text.StringBuilder**, mais comme elle fait assez de mise en forme de chaîne, pour la version C++/WinRT, il s’agit d’un autre endroit approprié pour utiliser **std::wostringstream**.

Au lieu de la propriété statique [**System.Environment.NewLine**](/dotnet/api/system.environment.newline), qui est utilisée dans le code C#, nous allons insérer `std::endl` du C++ standard (un caractère de saut de ligne) dans le flux de sortie.

```cppwinrt
// SampleConfiguration.h
...
private:
    static void DisplayChangedFormats();
...

// SampleConfiguration.cpp
void SampleState::DisplayChangedFormats()
{
    std::wostringstream output;
    output << L"Clipboard content has changed!" << std::endl;
    output << BuildClipboardFormatsOutputString().c_str();
    MainPage::Current().NotifyUser(output.str(), NotifyType::StatusMessage);
}
```

Il y a une petite chose inefficace dans la conception de la version C++/WinRT ci-dessus. Nous créons d’abord un **std::wostringstream**. Mais nous appelons aussi la méthode **BuildClipboardFormatsOutputString** (que nous avons portée précédemment). Cette méthode crée son propre **std::wostringstream**. Il transforme alors son flux en une **winrt::hstring** et la retourne. Nous appelons la fonction [**hstring::c_str**](/uwp/cpp-ref-for-winrt/hstring#hstringc_str-function) pour reconvertir cette **hstring** retournée en une chaîne de style C, puis nous l’insérons dans notre flux. Il serait plus efficace de créer un seul **std::wostringstream** et de passer cela (par référence) pour que les méthodes puissent y insérer directement des chaînes.

C’est ce que nous faisons dans la version C++/WinRT de l’exemple de [code source](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/Clipboard/cppwinrt) Clipboard. Dans ce code source, il y a une nouvelle méthode statique privée nommée **SampleState::AddClipboardFormatsOutputString**, qui prend et effectue ses opérations sur une référence à un flux de sortie. Ensuite, les méthodes **SampleState::DisplayChangedFormats** et **SampleState::BuildClipboardFormatsOutputString** sont refactorisées pour appeler cette nouvelle méthode. Elle est fonctionnellement équivalente au code de cette rubrique, mais elle est plus efficace.

#### <a name="footer_click"></a>**Footer_Click**

**Footer_Click** est un gestionnaire d’événements asynchrones appartenant à la classe C# **MainPage** et il est défini dans `MainPage.xaml.cs`. Le code ci-dessous est fonctionnellement équivalent à la méthode du code source que vous avez téléchargé. Mais ici, je l’ai développé en le faisant passer d’une ligne à quatre lignes, pour vous permettre de mieux voir ce qu’il fait et donc comment nous devons le porter.

```csharp
async void Footer_Click(object sender, RoutedEventArgs e)
{
    var hyperlinkButton = (HyperlinkButton)sender;
    string tagUrl = hyperlinkButton.Tag.ToString();
    Uri uri = new Uri(tagUrl);
    await Windows.System.Launcher.LaunchUriAsync(uri);
}
```

Si techniquement, la méthode est asynchrone, elle ne fait rien après l’instruction `await` : elle n’a donc pas besoin du `await` (ni du mot clé `async`). Elle les utilise probablement pour éviter le message IntelliSense dans Visual Studio.

La méthode C++/WinRT équivalente est également asynchrone (car elle appelle [**Launcher.LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync)). Mais elle n’a pas besoin de `co_await`, ni de retourner un objet asynchrone. Pour plus d’informations sur `co_await` et sur les objets asynchrones, consultez [Opérations concurrentes et asynchrones avec C++/WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency).

Parlons maintenant de ce que fait la méthode. Comme il s’agit d’un gestionnaire d’événements pour l’événement **Click** d’un **HyperlinkButton**, l’objet nommé *sender* est en fait un **HyperlinkButton**. Le cast est donc sûr (nous pourrions aussi avoir exprimé ce cast sous la forme `sender as HyperlinkButton`). Ensuite, nous récupérons la valeur de la propriété **Tag** (si vous examinez le balisage XAML dans le projet C#, vous verrez que ceci est défini sur une chaîne représentant une URL web). Bien que la propriété **FrameworkElement.Tag** (**HyperlinkButton** est un **FrameworkElement**) soit de type **object**, en C#, nous pouvons convertir cela en chaîne avec [**Object.ToString**](/dotnet/api/system.object.tostring). À partir de la chaîne résultante, nous construisons un objet **Uri**. Enfin (avec l’aide du Shell), nous lançons un navigateur et nous accédons à l’URL.

Voici la méthode portée en C++/WinRT (elle aussi développée pour plus de clarté), après quoi se trouve une description des détails.

```cppwinrt
// pch.h
...
#include "winrt/Windows.System.h"
...

// MainPage.h
...
    void Footer_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& e);
private:
...

// MainPage.cpp
...
using namespace winrt::Windows::Foundation;
using namespace winrt::Windows::UI::Xaml::Controls;
...
void MainPage::Footer_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const&)
{
    auto hyperlinkButton{ sender.as<HyperlinkButton>() };
    hstring tagUrl{ winrt::unbox_value<hstring>(hyperlinkButton.Tag()) };
    Uri uri{ tagUrl };
    Windows::System::Launcher::LaunchUriAsync(uri);
}
```

Comme toujours, nous créons le gestionnaire d’événements `public`. Nous utilisons la fonction [**as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) sur l’objet *sender* pour le caster en **HyperlinkButton**. En C++/WinRT, la propriété **Tag** est un [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (l’équivalent de [**Object**](/dotnet/api/system.object)). Mais il n’y a pas de **Tostring** sur **IInspectable**. Au lieu de cela, nous devons effectuer une conversion unboxing de **IInspectable** en une valeur scalaire (dans le cas présent, une chaîne). Là encore, pour plus d’informations sur les conversions boxing et unboxing, consultez [Conversions boxing et unboxing de valeurs scalaires vers IInspectable](/windows/uwp/cpp-and-winrt-apis/boxing).

Les deux dernières lignes répètent les modèles de portage que nous avons vus précédemment et elles sont assez similaires à la version C#.

#### <a name="handleclipboardchanged"></a>**HandleClipboardChanged**

Rien de nouveau n’est impliqué dans le portage de cette méthode. Vous pouvez comparer les versions C# et C++/WinRT dans l’exemple de code source.

#### <a name="onclipboardchanged-and-onwindowactivated"></a>**OnClipboardChanged** et **OnWindowActivated**

Jusqu’à présent, nous avons seulement des stubs vides pour ces deux gestionnaires d’événements. Leur portage est néanmoins simple et il n’y a rien de nouveau à y expliquer.

#### <a name="scenariocontrol_selectionchanged"></a>**ScenarioControl_SelectionChanged**

Il s’agit d’un autre gestionnaire d’événements privés appartenant à la classe C# **MainPage** et défini dans `MainPage.xaml.cs`. En C++/WinRT, nous allons le rendre public et l’implémenter dans `MainPage.h` et `MainPage.cpp`.

Pour cette méthode, nous avons besoin de **MainPage::Navigating**, qui est un champ booléen privé, initialisé à `false`. Vous aurez aussi besoin d’un **Frame** dans `MainPage.xaml`, nommé *ScenarioFrame*. À part ces détails, le portage de cette méthode ne révèle pas de nouvelles techniques.

#### <a name="updatestatus"></a>**UpdateStatus**

Nous n’avons qu’un stub jusqu’à présent pour **MainPage.UpdateStatus**. Le portage de son implémentation fait également appel à des choses connues. Un nouveau point à noter est que si en C#, nous pouvons comparer une **chaîne** à **String.Empty**, en C++/WinRT, nous appelons au lieu de cela la fonction [**winrt::hstring::empty**](/uwp/cpp-ref-for-winrt/hstring#hstringempty-function). Une autre chose à noter est que `nullptr` est l’équivalent C++ standard de `null` en C#.

Vous pouvez effectuer le reste du portage avec les techniques que nous avons déjà expliquées. Voici une liste de ce que vous devez faire avant la compilation de la version portée de cette méthode.

- Pour `MainPage.xaml`, ajoutez un **Border** nommé *StatusBorder*.
- Pour `MainPage.xaml`, ajoutez un **TextBlock** nommé *StatusBlock*.
- Pour `MainPage.xaml`, ajoutez un **StackPanel** nommé *StatusPanel*.
- Pour `pch.h`, ajoutez `#include "winrt/Windows.UI.Xaml.Media.h"`.
- Pour `pch.h`, ajoutez `#include "winrt/Windows.UI.Xaml.Automation.Peers.h"`.
- Pour `MainPage.cpp` ajoutez `using namespace winrt::Windows::UI::Xaml::Media;`.
- Pour `MainPage.cpp` ajoutez `using namespace winrt::Windows::UI::Xaml::Automation::Peers;`.

### <a name="copy-the-xaml-and-styles-necessary-to-finish-up-porting-mainpage"></a>Copier le XAML et les styles nécessaires pour terminer le portage de **MainPage**

Pour XAML, le cas idéal est celui où vous pouvez utiliser *le même* balisage XAML entre un projet C# et un projet C++/WinRT. L’exemple Clipboard est justement un de ces cas.

Dans son fichier `Styles.xaml`, l’exemple Clipboard contient un **ResourceDictionary** XAML des styles, qui sont appliqués aux boutons, aux menus et à d’autres éléments d’interface utilisateur dans l’interface de l’application. La page `Styles.xaml` est fusionnée dans `App.xaml`. Il y a ensuite le point de départ standard de `MainPage.xaml` pour l’interface utilisateur, que nous avons déjà vu brièvement. Nous pouvons maintenant réutiliser ces trois fichiers `.xaml` inchangés dans la version C++/WinRT du projet.

Comme pour les fichiers de ressources, vous pouvez choisir de référencer les mêmes fichiers XAML partagés depuis plusieurs versions de votre application. Dans cette procédure pas à pas, seulement pour des raisons de simplicité, nous allons copier les fichiers dans le projet C++/WinRT et les ajouter de cette façon.

Accédez au dossier `\Clipboard_sample\SharedContent\xaml`, sélectionnez et copiez `App.xaml` et `MainPage.xaml`, puis collez ces deux fichiers dans le dossier `\Clipboard\Clipboard` de votre projet C++/WinRT, en choisissant de remplacer les fichiers quand vous y êtes invité.

Ajoutez un nouveau dossier au projet C++/WinRT, immédiatement sous le nœud du projet, et nommez-le `Styles`. Accédez au dossier `\Clipboard_sample\SharedContent\xaml`, sélectionnez et copiez `Styles.xaml`, puis collez-le dans le dossier `\Clipboard\Clipboard\Styles` de votre projet C++/WinRT. Cliquez avec le bouton droit sur le dossier `Styles` (dans l’Explorateur de solutions, dans le projet C++/WinRT) > **Ajouter** > **Élément existant...** et accédez à `\Clipboard\Clipboard\Styles`. Dans le sélecteur de fichiers, sélectionnez `Styles`, puis cliquez sur **Ajouter**.

Nous avons maintenant terminé le portage **MainPage** et, si vous avez effectué les étapes, votre projet C++/WinRT peut maintenant être généré et exécuté.

## <a name="consolidate-your-idl-files"></a>Regrouper vos fichiers `.idl`

En plus du point de départ standard `MainPage.xaml` pour l’interface utilisateur, l’exemple Clipboard contient cinq autres pages XAML spécifiques au scénario, ainsi que leurs fichiers code-behind correspondants. Nous allons réutiliser le balisage XAML réel de toutes ces pages, sans modification, dans la version C++/WinRT du projet. Nous allons aussi voir comment porter le code-behind dans les sections majeures suivantes. Mais avant cela, parlons d’IDL.

Il est avantageux de regrouper vos classes de runtime dans un seul fichier IDL (consultez [Factorisation des classes de runtime dans des fichiers Midl (.idl)](/windows/uwp/cpp-and-winrt-apis/author-apis#factoring-runtime-classes-into-midl-files-idl)). Nous allons donc regrouper le contenu de `CopyFiles.idl`, `CopyImage.idl`, `CopyText.idl`, `HistoryAndRoaming.idl` et `OtherScenarios.idl` en déplaçant cet IDL dans un même fichier nommé `Project.idl` (puis en supprimant les fichiers d’origine).

Lors de cette opération, nous allons également supprimer la propriété factice générée automatiquement (`Int32 MyProperty;` et son implémentation) de chacun de ces cinq types de page XAML.

Tout d’abord, ajoutez un nouvel élément **Fichier MIDL (.idl)** au projet C++/WinRT. Nommez-le `Project.idl`. Supprimez le contenu par défaut de `Project.idl` puis collez à sa place la liste ci-dessous.

```idl
// Project.idl
namespace SDKTemplate
{
    [default_interface]
    runtimeclass CopyFiles : Windows.UI.Xaml.Controls.Page
    {
        CopyFiles();
    }

    [default_interface]
    runtimeclass CopyImage : Windows.UI.Xaml.Controls.Page
    {
        CopyImage();
    }

    [default_interface]
    runtimeclass CopyText : Windows.UI.Xaml.Controls.Page
    {
        CopyText();
    }

    [default_interface]
    runtimeclass HistoryAndRoaming : Windows.UI.Xaml.Controls.Page
    {
        HistoryAndRoaming();
    }

    [default_interface]
    runtimeclass OtherScenarios : Windows.UI.Xaml.Controls.Page
    {
        OtherScenarios();
    }
}
```

Comme vous pouvez le voir, il s’agit simplement d’une copie du contenu des fichiers `.idl` individuels, tous dans un même espace de noms, `MyProperty` étant supprimée de chaque classe de runtime.

Dans l’Explorateur de solutions de Visual Studio, sélectionnez en même temps tous les fichiers IDL d’origine (`CopyFiles.idl`, `CopyImage.idl`, `CopyText.idl`, `HistoryAndRoaming.idl` et `OtherScenarios.idl`) puis sélectionnez **Modifier** > **Supprimer** (choisissez **Supprimer** dans la boîte de dialogue).

Enfin, et pour terminer la suppression de `MyProperty` dans les fichiers `.h` et `.cpp` pour chacun des cinq types de page XAML, supprimez les déclarations et les définitions des fonctions d’accesseur `int32_t MyProperty()` et de mutateur `void MyProperty(int32_t)`.

## <a name="copyfiles"></a>**CopyFiles**

Dans le projet C#, le type de page XAML **CopyFiles** est implémenté dans les fichiers de code source `CopyFiles.xaml` et `CopyFiles.xaml.cs`. Jetons un coup d’œil tour à tour à chacun des membres de **CopyFiles**.

### <a name="rootpage"></a>**rootPage**

Il s’agit d’un champ privé.

```csharp
// CopyFiles.xaml.cs
...
public sealed partial class CopyFiles : Page
{
    MainPage rootPage = MainPage.Current;
    ...
}
...
```

En C++/WinRT, nous pouvons le définir et l’initialiser comme ceci.

```cppwinrt
// CopyFiles.h
...
struct CopyFiles : CopyFilesT<CopyFiles>
{
    ...
private:
    SDKTemplate::MainPage rootPage{ MainPage::Current() };
};
...
```

Là encore, (comme avec **MainPage::current**), **CopyFiles::rootPage** est déclaré comme étant de type **SDKTemplate::MainPage**, qui est le type projeté, et non pas le type d’implémentation.

### <a name="copyfiles-the-constructor"></a>**CopyFiles** (le constructeur)

Dans le projet C++/WinRT, le type **CopyFiles** a déjà un constructeur qui contient le code que nous voulons (il appelle simplement **InitializeComponent**).

### <a name="copybutton_click"></a>**CopyButton_Click**

La méthode C# **CopyButton_Click** est un gestionnaire d’événements et, en raison de la présence du mot clé `async` dans sa signature, nous pouvons déterminer que la méthode effectue un travail asynchrone. En C++/WinRT, nous implémentons une méthode asynchrone en tant que *coroutine*. Pour une présentation de la concurrence dans C++/WinRT ainsi qu’une description de ce qu’est une *coroutine*, consultez [Opérations concurrentes et asynchrones avec C++/WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency).

Il est courant de vouloir planifier du travail supplémentaire après la fin d’une coroutine : dans ce cas, la coroutine peut retourner un type d’objet asynchrone qui peut faire l’objet d’une attente et qui peut indiquer une progression. Cependant, ces considérations ne s’appliquent généralement pas à un gestionnaire d’événements. Ainsi, quand vous avez un gestionnaire d’événements qui effectue des opérations asynchrones, vous pouvez l’implémenter en tant que coroutine retournant **winrt::fire_and_forget**. Pour plus d’informations, consultez [Déclencher et oublier](/windows/uwp/cpp-and-winrt-apis/concurrency-2#fire-and-forget).

Même si l’idée d’une coroutine de type « déclencher et oublier » est de ne pas vous soucier du moment où elle se termine, le travail continue néanmoins (ou est suspendu, en attendant la reprise) en arrière-plan. Vous pouvez voir de l’implémentation C# que **CopyButton_Click** dépend du pointeur `this` (il accède au membre de données d’instance `rootPage`). Nous devons donc être sûr que le pointeur `this` (un pointeur vers un objet **CopyFiles**) survit à la coroutine **CopyButton_Click**. Dans une situation comme celle de cet exemple d’application, où l’utilisateur navigue entre les pages de l’interface utilisateur, nous ne pouvons pas contrôler directement la durée de vie de ces pages. Si la page **CopyFiles** venait à être détruite (si l’utilisateur navigue en dehors de celle-ci) alors que **CopyButton_Click** est toujours actif sur un thread d’arrière-plan, il n’est pas prudent d’accéder à `rootPage`. Pour que la coroutine soit correcte, elle doit obtenir une référence forte au pointeur `this` et conserver cette référence pour la durée de la coroutine. Pour plus d’informations, consultez [Références fortes et faibles en C++/WinRT](/windows/uwp/cpp-and-winrt-apis/weak-references).

Si vous regardez dans la version C++/WinRT de l’exemple, pour **CopyFiles::CopyButton_Click**, vous voyez qu’elle est effectuée avec une simple déclaration sur la pile.

```cppwinrt
fire_and_forget CopyFiles::CopyButton_Click(IInspectable const&, RoutedEventArgs const&)
{
    auto lifetime{ get_strong() };
    ...
}
```

Examinons les autres aspects intéressants du code porté.

Dans le code, nous instancions un objet [**FileOpenPicker**](/uwp/api/windows.storage.pickers.fileopenpicker) et, deux lignes plus loin, nous accédons à la propriété [**FileTypeFilter**](/uwp/api/windows.storage.pickers.fileopenpicker.filetypefilter) de cet objet. Le type de retour de cette propriété implémente un **IVector** de chaînes. Et sur cet **IVector**, nous appelons la méthode [IVector<T>.ReplaceAll(T[])](/uwp/api/windows.foundation.collections.ivector-1.replaceall). L’aspect intéressant est la valeur que nous passons à cette méthode, où un tableau est attendu. Voici la ligne de code.

```cppwinrt
filePicker.FileTypeFilter().ReplaceAll({ L"*" });
```

La valeur que nous passons (`{ L"*" }`) est une *liste d’initialiseurs* C++ standard. Elle contient dans le cas présent un seul objet, mais une liste d’initialiseurs peut contenir un nombre quelconque d’objets séparés par des virgules. Les éléments de C++/WinRT qui vous permettent de passer une liste d’initialiseurs à une méthode telle que celle-ci sont expliqués dans [Listes d’initialiseurs standard](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-initializer-lists).

Nous portons le mot clé C# `await` en `co_await` dans C++/WinRT. Voici l’exemple du code.

```cppwinrt
auto storageItems{ co_await filePicker.PickMultipleFilesAsync() };
```

Examinez ensuite cette ligne de code C#.

```csharp
dataPackage.SetStorageItems(storageItems);
```

C# peut convertir implicitement la **IReadOnlyList<StorageFile>** représentée par *storageItems* dans l’élément **IEnumerable<IStorageItem>** attendu par [**DataPackage.SetStorageItems**](/uwp/api/windows.applicationmodel.datatransfer.datapackage.setstorageitems). Cependant, en C++/WinRT, nous devons explicitement caster de **IVectorView<StorageFile>** en **IIterable<IStorageItem>** . Nous avons donc un autre exemple de la fonction [**as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) en action.

```cppwinrt
dataPackage.SetStorageItems(storageItems.as<IVectorView<IStorageItem>>());
```

Là où nous utilisons le mot clé `null` en C# (par exemple `Clipboard.SetContentWithOptions(dataPackage, null)`), nous utilisons `nullptr` en C++/WinRT (par exemple `Clipboard::SetContentWithOptions(dataPackage, nullptr)`).

### <a name="pastebutton_click"></a>**PasteButton_Click**

Il s’agit d’un autre gestionnaire d’événements sous la forme d’une coroutine « déclencher et oublier ». Examinons les aspects intéressants du code porté.

Dans la version C# de l’exemple, nous interceptons les exceptions avec `catch (Exception ex)`. Dans le code C++/WinRT porté, vous verrez l’expression `catch (winrt::hresult_error const& ex)`. Pour plus d’informations sur [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) et sur la façon de l’utiliser, consultez [Gestion des erreurs avec C++/WinRT](/windows/uwp/cpp-and-winrt-apis/error-handling).

Voici un exemple qui teste si un objet C# est `null` ou pas : `if (storageItems != null)`. En C++/WinRT, nous pouvons nous appuyer sur un opérateur de conversion en `bool`, qui effectue le test en interne sur `nullptr`.

Voici une version un peu simplifiée d’un fragment de code provenant de la version C++/WinRT portée de l’exemple.

```cppwinrt
std::wostringstream output;
output << std::wstring_view(ApplicationData::Current().LocalFolder().Path());
```

La construction d’une **std::wstring_view** à partir d’une **winrt::hstring** comme celle-ci montre une alternative à l’appel de la fonction [**hstring::c_str**](/uwp/cpp-ref-for-winrt/hstring#hstringc_str-function) (pour transformer la **winrt::hstring** en chaîne de style C). Cette alternative fonctionne grâce à l’[opérateur de conversion**de **hstring** pour**std::wstring_view](/uwp/cpp-ref-for-winrt/hstring#hstringoperator-stdwstring_view).

Considérez ce fragment C#.

```csharp
var file = storageItem as StorageFile;
if (file != null)
...
```

Pour porter le mot clé C# `as` en C++/WinRT, nous avons vu jusqu’à présent la fonction [**as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) utilisée plusieurs fois. Cette fonction lève une exception si le cast échoue. Mais si nous voulons que le cast retourne `nullptr` s’il échoue (pour pouvoir gérer cette condition dans le code), nous devons à la place utiliser la fonction [**try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function).

```cppwinrt
auto file{ storageItem.try_as<StorageFile>() };
if (file)
...
```

### <a name="copy-the-xaml-necessary-to-finish-up-porting-copyfiles"></a>Copier le XAML pour terminer le portage de **CopyFiles**

Vous pouvez maintenant sélectionner tout le contenu du fichier `CopyFiles.xaml` dans le projet C#, puis le coller dans le fichier `CopyFiles.xaml` du projet C++/WinRT (en remplaçant le contenu existant de ce fichier dans le projet C++/WinRT).

Enfin, éditez `CopyFiles.h` et `.cpp`, et supprimez la fonction **ClickHandler**, car nous avons simplement remplacé le balisage XAML correspondant.

Nous avons maintenant terminé le portage de **CopyFiles** et, si vous avez effectué les différentes étapes, votre projet C++/WinRT peut maintenant être généré et exécuté, et le scénario **CopyFiles** sera fonctionnel.

## <a name="copyimage"></a>**CopyImage**

Pour porter le type de page XAML **CopyImage**, suivez le même processus que pour **CopyFiles**. Lors du portage de **CopyImage**, vous rencontrerez l’utilisation de l’[*instruction using*](/dotnet/csharp/language-reference/keywords/using-statement) de C#, qui garantit que les objets implémentant l’interface [**IDisposable**](/dotnet/api/system.idisposable) sont supprimés correctement.

```csharp
if (imageReceived != null)
{
    using (var imageStream = await imageReceived.OpenReadAsync())
    {
        ... // Pass imageStream to other APIs, and do other work.
    }
}
```

L’interface équivalente e, C++/WinRT est [**IClosable**](/uwp/api/windows.foundation.iclosable), avec sa seule méthode **Close**. Voici l’équivalent C++/WinRT du code C# ci-dessus.

```cppwinrt
if (imageReceived)
{
    auto imageStream{ co_await imageReceived.OpenReadAsync() };
    ... // Pass imageStream to other APIs, and do other work.
    imageStream.Close();
}
```

Les objets C++/WinRT implémentent **IClosable** principalement au bénéfice des langages qui n’ont pas de finalisation déterministe. C++/WinRT a une finalisation déterministe : nous n’avons donc pas souvent besoin d’appeler **IClosable::Close** quand nous écrivons du code C++/WinRT. Il y a cependant des circonstances où c’est une bonne idée d’y faire appel, et celle-ci en fait partie. Ici, l’identificateur *imageStream* est un wrapper avec comptage des références autour d’un objet Windows Runtime sous-jacent (dans le cas présent, un objet qui implémente [**IRandomAccessStreamWithContentType**](/uwp/api/windows.storage.streams.irandomaccessstreamwithcontenttype)). Bien que nous puissions déterminer que le finaliseur de *imageStream* (son destructeur) va s’exécuter à la fin de l’étendue englobante (les accolades), nous ne pouvons pas être certains que le finaliseur va appeler **Close**. La raison en est que nous avons passé *imageStream* à d’autres API, et qu’elles peuvent encore contribuer au comptage des références de l’objet Windows Runtime sous-jacent. C’est pourquoi il est judicieux d’appeler **Close** explicitement. Pour plus d’informations, consultez [Dois-je appeler IClosable::Close sur les classes de runtime que je consomme ?](/windows/uwp/cpp-and-winrt-apis/faq#do-i-need-to-call-iclosableclose-on-runtime-classes-that-i-consume).

Ensuite, examinez l’expression C# `(uint)(imageDecoder.OrientedPixelWidth * 0.5)`, que vous trouvez dans le gestionnaire d’événements **OnDeferredImageRequestedHandler**. Cette expression multiplie un `uint` par un `double`, ce qui produit un `double`. Il caste ensuite ce résultat en `uint`. En C++/WinRT, nous *pourrions* utiliser un cast de style C d’aspect similaire (`(uint32_t)(imageDecoder.OrientedPixelWidth() * 0.5)`), mais il est préférable de préciser exactement le type de cast que nous voulons, et dans le cas présent, nous allons le faire avec `static_cast<uint32_t>(imageDecoder.OrientedPixelWidth() * 0.5)`.

La version C# de **CopyImage.OnDeferredImageRequestedHandler** a une clause `finally`, mais pas de clause `catch`. Nous sommes allés juste un peu plus loin dans la version C++/WinRT et nous avons implémenté une clause `catch` pour pouvoir indiquer si oui ou non le rendu retardé a réussi.

Le portage du reste de cette page XAML ne nécessite pas d’autres explications. Tout comme avec **CopyFiles**, la dernière étape du portage consiste à sélectionner le contenu entier de `CopyImage.xaml` et à le coller dans le même fichier du projet C++/WinRT.

## <a name="copytext"></a>**CopyText**

Vous pouvez porter `CopyText.xaml` et `CopyText.xaml.cs` en utilisant les techniques que nous avons déjà exposées.

## <a name="historyandroaming"></a>**HistoryAndRoaming**

Certains points nous intéressent dans le portage du type de page XAML **HistoryAndRoaming**.

Tout d’abord, examinez le code source C# et suivez le flux de contrôle de **OnNavigatedTo** à travers le gestionnaire d’événements **OnHistoryEnabledChanged**, et enfin jusqu’à la fonction asynchrone **CheckHistoryAndRoaming** (qui ne fait pas l’objet d’une attente : il s’agit donc essentiellement d’une coroutine « déclencher et oublier »). Comme **CheckHistoryAndRoaming** est asynchrone, nous devons faire attention dans C++/WinRT à la durée de vie du pointeur `this`. Vous pouvez voir le résultat si vous examinez l’implémentation dans le fichier de code source `HistoryAndRoaming.cpp`. Premièrement, quand nous attachons des délégués aux événements **Clipboard::HistoryEnabledChanged** et **Clipboard::RoamingEnabledChanged**, nous prenons seulement une référence faible à l’objet de page **HistoryAndRoaming**. Pour cela, nous créons le délégué avec une dépendance sur la valeur retournée depuis [**winrt::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function), au lieu d’une dépendance sur le pointeur `this`. Cela signifie que le délégué lui-même, qui finit par appeler dans le code asynchrone, ne conserve pas active la page **HistoryAndRoaming** dès lors que nous la quittons.

Et deuxièmement, quand nous atteignons finalement notre coroutine **CheckHistoryAndRoaming** « déclencher et oublier », la première chose que nous faisons est de prendre une référence forte à `this` pour garantir que la page **HistoryAndRoaming** reste active au moins jusqu’à ce que la coroutine se termine. Pour plus d’informations sur les deux aspects qui viennent d’être décrits, consultez [Références fortes et faibles en C++/WinRT](/windows/uwp/cpp-and-winrt-apis/weak-references).

Nous trouvons un autre point intéressant dans le portage de **CheckHistoryAndRoaming**. Il contient le code permettant de mettre à jour l’interface utilisateur : nous devons donc être sûr que nous faisons cela sur le thread d’interface utilisateur principal. En règle générale, une méthode asynchrone peut exécuter et/ou reprendre sur n’importe quel thread arbitraire. En C#, la solution consiste à appeler [**CoreDispatcher. RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) et à mettre à jour l’interface utilisateur depuis la fonction lambda. En C++/WinRT, nous pouvons utiliser la fonction [**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground) avec [**répartiteur**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher) du pointeur `this` pour mettre en suspens la coroutine et reprendre immédiatement sur le thread d’interface utilisateur principal.

L’expression appropriée est `co_await winrt::resume_foreground(Dispatcher());`. Bien que ce soit moins clair, nous pouvons aussi exprimer cela simplement sous la forme `co_await Dispatcher();`. La version plus courte est obtenue grâce à un opérateur de conversion fourni par C++/WinRT.

## <a name="otherscenarios"></a>**OtherScenarios**

Vous pouvez porter `OtherScenarios.xaml` et `OtherScenarios.xaml.cs` en utilisant les techniques que nous avons déjà exposées.

## <a name="conclusion"></a>Conclusion

Nous espérons que cette procédure pas à pas vous a armé avec des informations et des techniques de portage suffisantes pour vous permettre maintenant d’aller de l’avant et de porter vos propres applications C# vers C++/WinRT. Via un actualisateur, vous pouvez continuer vous référer aux versions avant (C#) et après (C++/WinRT) du code source de l’exemple Clipboard, et les comparer côte à côte pour voir la correspondance.

## <a name="related-topics"></a>Rubriques connexes
* [Passer de C# à C++/WinRT](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-csharp)