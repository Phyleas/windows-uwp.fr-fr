---
Description: Si vous souhaitez que votre application prenne en charge différentes langues d’affichage et si des opérateurs de chaîne figurent dans votre code, votre balisage XAML ou le manifeste du package d’application, déplacez ces chaînes dans un fichier de ressources (.resw). Vous pouvez ensuite effectuer une copie traduite de ce fichier de ressources pour chaque langue prise en charge par votre application.
title: Localiser les chaînes dans l’interface utilisateur et le manifeste du package d’application
ms.assetid: E420B9BB-C0F6-4EC0-BA3A-BA2875B69722
label: Localize strings in your UI and app package manifest
template: detail.hbs
ms.date: 11/01/2017
ms.topic: article
keywords: windows 10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: 0cf6bc95eef416b481642d84eef8315451916604
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174103"
---
# <a name="localize-strings-in-your-ui-and-app-package-manifest"></a>Localiser les chaînes dans l’interface utilisateur et le manifeste du package d’application

Pour plus d’informations sur la proposition de valeur de la localisation de votre application, consultez [Internationalisation et localisation](../design/globalizing/globalizing-portal.md).

Si vous souhaitez que votre application prenne en charge différentes langues d’affichage et si des opérateurs de chaîne figurent dans votre code, votre balisage XAML ou le manifeste du package d’application, déplacez ces chaînes dans un fichier de ressources (.resw). Vous pouvez ensuite effectuer une copie traduite de ce fichier de ressources pour chaque langue prise en charge par votre application.

Les littéraux de chaîne codés en dur peuvent apparaître dans le code impératif ou dans le balisage XAML, par exemple en tant que propriété **Text** d’un **TextBlock**. Ils peuvent également apparaître dans le fichier source du manifeste de votre package d’application (le `Package.appxmanifest` fichier), par exemple en tant que valeur de nom complet sous l’onglet application du concepteur de manifeste de Visual Studio. Déplacez ces chaînes dans un fichier de ressources (. resw) et remplacez les littéraux de chaîne codés en dur dans votre application et dans votre manifeste par des références aux identificateurs de ressource.

Contrairement aux ressources d’image, où une seule ressource d’image est contenue dans un fichier de ressources d’image, *plusieurs* ressources de chaîne sont contenues dans un fichier de ressources de chaîne. Un fichier de ressources de type chaîne est un fichier de ressources (. resw) et vous créez généralement ce type de fichier de ressources dans un dossier \Strings de votre projet. Pour plus d’informations sur l’utilisation de qualificateurs dans les noms de vos fichiers de ressources (. resw), consultez [adapter vos ressources à la langue, à la mise à l’échelle et à d’autres qualificateurs](tailor-resources-lang-scale-contrast.md).

## <a name="store-strings-in-a-resources-file"></a>Stocker des chaînes dans un fichier de ressources

1. Définissez la langue par défaut de votre application.
    1. Une fois votre solution ouverte dans Visual Studio, ouvrez `Package.appxmanifest` .
    2. Sous l’onglet application, vérifiez que la langue par défaut est définie correctement (par exemple, « fr » ou « en-US »). Les étapes restantes supposent que vous avez défini la langue par défaut sur « en-US ».
    <br>**Remarque**   Au minimum, vous devez fournir des ressources de type chaîne localisées pour cette langue par défaut. Il s’agit des ressources qui seront chargées si aucune meilleure correspondance ne peut être trouvée pour la langue par défaut de l’utilisateur ou les paramètres de langue d’affichage.
2. Créez un fichier de ressources (. resw) pour la langue par défaut.
    1. Sous le nœud de votre projet, créez un nouveau dossier et nommez-le « Strings ».
    2. Sous `Strings` , créez un sous-dossier et nommez-le « en-US ».
    3. Sous `en-US` , créez un fichier de ressources (. resw) et vérifiez qu’il est nommé « Resources. resw ».
    <br>**Remarque**   Si vous avez des fichiers de ressources .NET (. resx) que vous souhaitez porter, consultez [Portage de code XAML et d’interface utilisateur](../porting/wpsl-to-uwp-porting-xaml-and-ui.md#localization-and-globalization).
3. Ouvrez `Resources.resw` et ajoutez ces ressources de type chaîne.

    `Strings/en-US/Resources.resw`

    ![Ajouter une ressource, anglais](images/addresource-en-us.png)

    Dans cet exemple, « Greeting » est un identificateur de ressource de chaîne auquel vous pouvez faire référence à partir de votre balisage, comme nous allons le voir. Pour l’identificateur « salutation », une chaîne est fournie pour une propriété de texte, et une chaîne est fournie pour une propriété de largeur. « Greeting. Text » est un exemple d’identificateur de propriété, car il correspond à une propriété d’un élément d’interface utilisateur. Vous pouvez également, par exemple, ajouter « salutation. Foreground » dans la colonne nom et définir sa valeur sur « rouge ». L’identificateur « adieu » est un identificateur de ressource de chaîne simple ; Il n’a pas de sous-propriétés et il peut être chargé à partir du code impératif, comme nous allons le voir. La colonne commentaire est un bon endroit pour fournir des instructions spéciales aux traducteurs.

    Dans cet exemple, étant donné que nous avons une simple entrée d’identificateur de ressource de chaîne nommée « adieu », nous ne pouvons pas *également* avoir d’identificateurs de propriété basés sur ce même identificateur. Par conséquent, l’ajout de « adieu. Text » provoquerait une erreur d’entrée en double lors de la génération `Resources.resw` .

    Les identificateurs de ressource ne respectent pas la casse et doivent être uniques par fichier de ressources. Veillez à utiliser des identificateurs de ressources significatifs pour fournir un contexte supplémentaire pour les traducteurs. Et ne modifient pas les identificateurs de ressource une fois que les ressources de chaîne sont envoyées pour la traduction. Les équipes de localisation utilisent l’identificateur de ressource pour effectuer le suivi des ajouts, des suppressions et des mises à jour dans les ressources. Les modifications apportées aux identificateurs de ressource &mdash; , également appelées « identificateurs de ressource » &mdash; , exigent que les chaînes soient retraduites, car elles apparaîtront comme si des chaînes étaient supprimées et d’autres ajouts.

## <a name="refer-to-a-string-resource-identifier-from-xaml"></a>Faire référence à un identificateur de ressource de chaîne à partir de XAML

Vous utilisez une [directive x :uid](../xaml-platform/x-uid-directive.md) pour associer un contrôle ou un autre élément dans votre balisage à un identificateur de ressource de chaîne.

```xaml
<TextBlock x:Uid="Greeting"/>
```

Au moment de l’exécution, `\Strings\en-US\Resources.resw` est chargé (car il s’agit du seul fichier de ressources du projet). La directive **x :uid** sur le **TextBlock** provoque une recherche, pour rechercher des identificateurs de propriété dans `Resources.resw` qui contiennent l’identificateur de ressource de chaîne « Greeting ». Les identificateurs de propriété « Greeting. Text » et « salutation. Width » sont trouvés et leurs valeurs sont appliquées au **TextBlock**, remplaçant toutes les valeurs définies localement dans le balisage. La valeur « salutation. Foreground » est également appliquée, si vous l’avez ajoutée. Mais seuls les identificateurs de propriété sont utilisés pour définir des propriétés sur des éléments de balisage XAML. par conséquent, l’affectation de la valeur « adieu » à **x :uid** à ce TextBlock n’a aucun effet. `Resources.resw`*contient l'* identificateur de ressource de chaîne « adieu », mais il ne contient aucun identificateur de propriété.

Quand vous assignez un identificateur de ressource de chaîne à un élément XAML, assurez-vous que *tous* les identificateurs de propriété pour cet identificateur sont appropriés pour l’élément XAML. Par exemple, si vous définissez `x:Uid="Greeting"` sur un **TextBlock** , alors « Greeting. Text » est résolu car le type **TextBlock** a une propriété Text. Toutefois, si vous définissez `x:Uid="Greeting"` sur un **bouton** , « Greeting. Text » génère une erreur d’exécution, car le type de **bouton** n’a pas de propriété Text. Une solution pour ce cas consiste à créer un identificateur de propriété nommé « ButtonGreeting. Content » et à définir `x:Uid="ButtonGreeting"` sur le **bouton**.

Au lieu de définir la **largeur** à partir d’un fichier de ressources, vous souhaiterez probablement autoriser les contrôles à se dimensionner de façon dynamique vers le contenu.

**Remarque**   Pour les [propriétés jointes](../xaml-platform/attached-properties-overview.md), vous avez besoin d’une syntaxe spéciale dans la colonne Name d’un fichier. resw. Par exemple, pour définir une valeur pour la propriété jointe [**AutomationProperties.Name**](/uwp/api/windows.ui.xaml.automation.automationproperties.NameProperty) pour l’identificateur « Greeting », c’est ce que vous entrez dans la colonne Name.

```xml
Greeting.[using:Windows.UI.Xaml.Automation]AutomationProperties.Name
```

## <a name="refer-to-a-string-resource-identifier-from-code"></a>Faire référence à un identificateur de ressource de chaîne à partir du code

Vous pouvez charger explicitement une ressource de type chaîne basée sur un identificateur de ressource de chaîne simple.

> [!NOTE]
> Si vous avez un appel à une méthode **GetForCurrentView** qui *peut* être exécutée sur un thread d’arrière-plan/de travail, protégez cet appel avec un `if (Windows.UI.Core.CoreWindow.GetForCurrentThread() != null)` test. L’appel de **GetForCurrentView** à partir d’un thread d’arrière-plan/de travail entraîne l’exception «* &lt; TypeName &gt; ne peut pas être créé sur des threads qui n’ont pas de CoreWindow.*»

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Farewell");
```

```cppwinrt
auto resourceLoader{ Windows::ApplicationModel::Resources::ResourceLoader::GetForCurrentView() };
myXAMLTextBlockElement().Text(resourceLoader.GetString(L"Farewell"));
```

```cpp
auto resourceLoader = Windows::ApplicationModel::Resources::ResourceLoader::GetForCurrentView();
this->myXAMLTextBlockElement->Text = resourceLoader->GetString("Farewell");
```

Vous pouvez utiliser ce même code dans une bibliothèque de classes (Windows universel) ou dans un projet de [bibliothèque de Windows Runtime (Windows universel)](../winrt-components/index.md) . Au moment de l’exécution, les ressources de l’application qui héberge la bibliothèque sont chargées. Nous vous recommandons d’utiliser une bibliothèque pour charger des ressources à partir de l’application qui l’héberge, puisque l’application est susceptible d’avoir un niveau de localisation plus élevé. Si une bibliothèque a besoin de fournir des ressources, elle doit donner à son application d’hébergement la possibilité de remplacer ces ressources en tant qu’entrée.

Si un nom de ressource est segmenté (il contient des caractères « . »), remplacez les points par des barres obliques (« / ») dans le nom de la ressource. Les identificateurs de propriété, par exemple, contiennent des points ; vous devez donc effectuer cette substition afin de charger l’un d’eux à partir du code.

```csharp
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Fare/Well"); // <data name="Fare.Well" ...> ...
```

En cas de doute, vous pouvez utiliser [MakePri.exe](makepri-exe-command-options.md) pour vider le fichier PRI de votre application. Chaque ressource `uri` est indiquée dans le fichier vidé.

```xml
<ResourceMapSubtree name="Fare"><NamedResource name="Well" uri="ms-resource://<GUID>/Resources/Fare/Well">...
```

## <a name="refer-to-a-string-resource-identifier-from-your-app-package-manifest"></a>Faire référence à un identificateur de ressource de chaîne à partir de votre manifeste de package d’application

1. Ouvrez le fichier source du manifeste du package d’application (le `Package.appxmanifest` fichier), dans lequel la valeur par défaut de votre application `Display name` est exprimée sous la forme d’un littéral de chaîne.

   ![Ajouter une ressource, anglais](images/display-name-before.png)

2. Pour rendre une version localisable de cette chaîne, ouvrez `Resources.resw` et ajoutez une nouvelle ressource de type chaîne portant le nom « AppDisplayName » et la valeur « Adventure Works cycles ».

3. Remplacez le littéral de chaîne de nom complet par une référence à l’identificateur de ressource de chaîne que vous venez de créer (« AppDisplayName »). Pour ce faire, vous utilisez le `ms-resource` schéma URI (Uniform Resource Identifier).

   ![Ajouter une ressource, anglais](images/display-name-after.png)

4. Répétez ce processus pour chaque chaîne du manifeste que vous souhaitez localiser. Par exemple, le nom abrégé de votre application (que vous pouvez configurer pour apparaître sur la vignette de votre application au démarrage). Pour obtenir la liste de tous les éléments du manifeste de package d’application que vous pouvez localiser, consultez [éléments de manifeste localisables](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live).

## <a name="localize-the-string-resources"></a>Localiser les ressources de type chaîne

1. Effectuez une copie de votre fichier de ressources (. resw) pour une autre langue.
    1. Sous « chaînes », créez un sous-dossier et nommez-le « de-DE » pour Deutsch (Deutschland).
   <br>**Remarque**   Pour le nom du dossier, vous pouvez utiliser n’importe quelle [balise de langue BCP-47](https://tools.ietf.org/html/bcp47). Pour plus d’informations sur le qualificateur de langue et pour obtenir la liste des balises de langue communes, consultez [adapter vos ressources à la langue, à la mise à l’échelle et à d’autres qualificateurs](tailor-resources-lang-scale-contrast.md) .
   2. Effectuez une copie de `Strings/en-US/Resources.resw` dans le `Strings/de-DE` dossier.
2. Traduisez les chaînes.
    1. Ouvrez `Strings/de-DE/Resources.resw` et Traduisez les valeurs dans la colonne valeur. Vous n’avez pas besoin de traduire les commentaires.

    `Strings/de-DE/Resources.resw`

    ![Ajouter une ressource (pour l’allemand)](images/addresource-de-de.png)

Si vous le souhaitez, vous pouvez répéter les étapes 1 et 2 pour une autre langue.

`Strings/fr-FR/Resources.resw`

![Ajouter une ressource, français](images/addresource-fr-fr.png)

## <a name="test-your-app"></a>Test de l'application

Testez l’application dans votre langue d’affichage par défaut. Vous pouvez ensuite modifier la langue d’affichage dans **paramètres**  >  **heure & langue**  >  **&**  >  **langues** et tester à nouveau votre application. Examinez les chaînes dans votre interface utilisateur et également dans l’interpréteur de commandes (par exemple, votre barre de titre &mdash; qui est votre nom &mdash; d’affichage et le nom abrégé sur vos vignettes).

**Remarque** Si un nom de dossier correspondant au paramètre de langue d’affichage est trouvé, le fichier de ressources contenu dans ce dossier est chargé. Dans le cas contraire, la solution de secours a lieu et se termine par les ressources de la langue par défaut de votre application.

## <a name="factoring-strings-into-multiple-resources-files"></a>Factorisation de chaînes en plusieurs fichiers de ressources

Vous pouvez conserver toutes vos chaînes dans un fichier de ressources unique (resw), ou vous pouvez les factoriser sur plusieurs fichiers de ressources. Par exemple, vous souhaiterez peut-être conserver vos messages d’erreur dans un fichier de ressources, les chaînes de votre package d’application dans un autre et vos chaînes d’interface utilisateur dans un troisième. Voici à quoi ressemblera votre structure de dossiers dans ce cas.

![Ajouter une ressource, anglais](images/manifest-resources.png)

Pour étendre une référence d’identificateur de ressource de chaîne à un fichier particulier, vous ajoutez simplement `/<resources-file-name>/` avant l’identificateur. L’exemple de balisage ci-dessous suppose que `ErrorMessages.resw` contient une ressource dont le nom est « PasswordTooWeak. Text » et dont la valeur décrit l’erreur.

```xaml
<TextBlock x:Uid="/ErrorMessages/PasswordTooWeak"/>
```

Vous devez uniquement ajouter `/<resources-file-name>/` avant l’identificateur de ressource de chaîne pour les fichiers de ressources *autres que* `Resources.resw` . Cela est dû au fait que « Resources. resw » est le nom de fichier par défaut. c’est donc le cas si vous omettez un nom de fichier (comme nous l’avons fait dans les exemples précédents de cette rubrique).

L’exemple de code ci-dessous suppose que `ErrorMessages.resw` contient une ressource dont le nom est « MismatchedPasswords » et dont la valeur décrit l’erreur.

> [!NOTE]
> Si vous avez un appel à une méthode **GetForCurrentView** qui *peut* être exécutée sur un thread d’arrière-plan/de travail, protégez cet appel avec un `if (Windows.UI.Core.CoreWindow.GetForCurrentThread() != null)` test. L’appel de **GetForCurrentView** à partir d’un thread d’arrière-plan/de travail entraîne l’exception «* &lt; TypeName &gt; ne peut pas être créé sur des threads qui n’ont pas de CoreWindow.*»

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("ErrorMessages");
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("MismatchedPasswords");
```

```cppwinrt
auto resourceLoader{ Windows::ApplicationModel::Resources::ResourceLoader::GetForCurrentView(L"ErrorMessages") };
myXAMLTextBlockElement().Text(resourceLoader.GetString(L"MismatchedPasswords"));
```

```cpp
auto resourceLoader = Windows::ApplicationModel::Resources::ResourceLoader::GetForCurrentView("ErrorMessages");
this->myXAMLTextBlockElement->Text = resourceLoader->GetString("MismatchedPasswords");
```

Si vous deviez déplacer votre ressource « AppDisplayName » de `Resources.resw` et vers `ManifestResources.resw` , puis dans le manifeste de votre package d’application, vous passeriez `ms-resource:AppDisplayName` à `ms-resource:/ManifestResources/AppDisplayName` .

Si un nom de fichier de ressources est segmenté (il contient des caractères « . »), laissez les points dans le nom lorsque vous le référencez. **Ne remplacez pas** les points par des barres obliques (« / »), comme vous le feriez pour un nom de ressource.

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("Err.Msgs");
```

En cas de doute, vous pouvez utiliser [MakePri.exe](makepri-exe-command-options.md) pour vider le fichier PRI de votre application. Chaque ressource `uri` est indiquée dans le fichier vidé.

```xml
<ResourceMapSubtree name="Err.Msgs"><NamedResource name="MismatchedPasswords" uri="ms-resource://<GUID>/Err.Msgs/MismatchedPasswords">...
```

## <a name="load-a-string-for-a-specific-language-or-other-context"></a>Charger une chaîne pour une langue ou un autre contexte spécifique

Le [**ResourceContext**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live) par défaut (obtenu à partir de [**ResourceContext. GetForCurrentView**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.GetForCurrentView)) contient une valeur de qualificateur pour chaque nom de qualificateur, représentant le contexte d’exécution par défaut (en d’autres termes, les paramètres de l’utilisateur et de l’ordinateur actuels). Les fichiers de ressources (. resw) sont mis en correspondance &mdash; en fonction des qualificateurs de leurs noms &mdash; par rapport aux valeurs de qualificateur dans ce contexte d’exécution.

Toutefois, il peut arriver que vous souhaitiez que votre application remplace les paramètres système et soit explicite à propos de la langue, de l’échelle ou d’une autre valeur de qualificateur à utiliser lors de la recherche d’un fichier de ressources correspondant à charger. Par exemple, vous souhaiterez peut-être que vos utilisateurs puissent sélectionner une autre langue pour les info-bulles ou les messages d’erreur.

Pour ce faire, vous pouvez créer un nouveau **ResourceContext** (au lieu d’utiliser celui par défaut), en remplaçant ses valeurs, puis en utilisant cet objet de contexte dans vos recherches de chaîne.

```csharp
var resourceContext = new Windows.ApplicationModel.Resources.Core.ResourceContext(); // not using ResourceContext.GetForCurrentView
resourceContext.QualifierValues["Language"] = "de-DE";
var resourceMap = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap.GetSubtree("Resources");
this.myXAMLTextBlockElement.Text = resourceMap.GetValue("Farewell", resourceContext).ValueAsString;
```

L’utilisation de **QualifierValues** comme dans l’exemple de code ci-dessus fonctionne pour tous les qualificateurs. Pour le cas particulier de la langue, vous pouvez également le faire à la place.

```csharp
resourceContext.Languages = new string[] { "de-DE" };
```

Pour le même effet à un niveau global, vous *pouvez* remplacer les valeurs de qualificateur dans le **ResourceContext**par défaut. Au lieu de cela, nous vous conseillons d’appeler [**ResourceContext. SetGlobalQualifierValue**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_). Vous définissez des valeurs une fois avec un appel à **SetGlobalQualifierValue** , puis ces valeurs sont en vigueur sur le **ResourceContext** par défaut chaque fois que vous l’utilisez pour les recherches.

```csharp
Windows.ApplicationModel.Resources.Core.ResourceContext.SetGlobalQualifierValue("Language", "de-DE");
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Farewell");
```

Certains qualificateurs ont un fournisseur de données système. Ainsi, au lieu d’appeler **SetGlobalQualifierValue** , vous pouvez ajuster le fournisseur par le biais de sa propre API. Par exemple, ce code montre comment définir [**PrimaryLanguageOverride**](/uwp/api/Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride).

```csharp
Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride = "de-DE";
```

## <a name="updating-strings-in-response-to-qualifier-value-change-events"></a>Mise à jour des chaînes en réponse aux événements de modification de valeur de qualificateur

Votre application en cours d’exécution peut répondre aux modifications apportées aux paramètres système qui affectent les valeurs de qualificateur dans le **ResourceContext**par défaut. L’un de ces paramètres système appelle l’événement [**MapChanged**](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live) sur [**ResourceContext. QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues).

En réponse à cet événement, vous pouvez recharger vos chaînes à partir du **ResourceContext**par défaut.

```csharp
public MainPage()
{
    this.InitializeComponent();

    ...

    // Subscribe to the event that's raised when a qualifier value changes.
    var qualifierValues = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues;
    qualifierValues.MapChanged += new Windows.Foundation.Collections.MapChangedEventHandler<string, string>(QualifierValues_MapChanged);
}

private async void QualifierValues_MapChanged(IObservableMap<string, string> sender, IMapChangedEventArgs<string> @event)
{
    var dispatcher = this.myXAMLTextBlockElement.Dispatcher;
    if (dispatcher.HasThreadAccess)
    {
        this.RefreshUIText();
    }
    else
    {
        await dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () => this.RefreshUIText());
    }
}

private void RefreshUIText()
{
    var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();
    this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Farewell");
}
```

## <a name="load-strings-from-a-class-library-or-a-windows-runtime-library"></a>Charger des chaînes à partir d’une bibliothèque de classes ou d’une bibliothèque de Windows Runtime

Les ressources de type chaîne d’une bibliothèque de classes référencée (Windows universelle) ou d’une [bibliothèque de Windows Runtime (Windows universelle)](../winrt-components/index.md) sont généralement ajoutées dans un sous-dossier du package dans lequel elles sont incluses pendant le processus de génération. L’identificateur de ressource d’une telle chaîne prend généralement la forme *NomBibliothèque/ResourcesFileName/ResourceIdentifier*.

Une bibliothèque peut obtenir un ResourceLoader pour ses propres ressources. Par exemple, le code suivant illustre comment une bibliothèque ou une application qui y fait référence peut obtenir un ResourceLoader pour les ressources de type chaîne de la bibliothèque.

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("ContosoControl/Resources");
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("exampleResourceName");
```

Pour une bibliothèque de Windows Runtime (Windows universel), si l’espace de noms par défaut est segmenté (il contient des caractères « . »), utilisez des points dans le nom de la carte de ressources.

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("Contoso.Control/Resources");
```

Vous n’avez pas besoin de le faire pour une bibliothèque de classes (Windows universel). En cas de doute, vous pouvez spécifier des [ options de ligne de commandeMakePri.exe](makepri-exe-command-options.md) pour vider le fichier PRI de votre composant ou de votre bibliothèque. Chaque ressource `uri` est indiquée dans le fichier vidé.

```xml
<NamedResource name="exampleResourceName" uri="ms-resource://Contoso.Control/Contoso.Control/ReswFileName/exampleResourceName">...
```

## <a name="loading-strings-from-other-packages"></a>Chargement de chaînes à partir d’autres packages

Les ressources d’un package d’application sont gérées et accessibles par le biais de l' [**ResourceMap**](/uwp/api/windows.applicationmodel.resources.core.resourcemap?branch=live) de niveau supérieur du package, accessible à partir du [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live)actuel. Dans chaque package, différents composants peuvent avoir leurs propres sous-arbres ResourceMap, auxquels vous pouvez accéder via [**ResourceMap. GetSubtree**](/uwp/api/windows.applicationmodel.resources.core.resourcemap.getsubtree?branch=live).

Un package d’infrastructure peut accéder à ses propres ressources avec un URI d’identificateur de ressource absolu. Consultez également [schémas d’URI](uri-schemes.md).

## <a name="loading-strings-in-non-packaged-applications"></a>Chargement de chaînes dans des applications non empaquetées

À compter de Windows version 1903 (mise à jour de mai 2019), les applications non empaquetées peuvent également tirer parti du système de gestion des ressources.

Créez simplement vos bibliothèques/contrôles utilisateur UWP et [stockez toutes les chaînes dans un fichier de ressources](#store-strings-in-a-resources-file). Vous pouvez ensuite [faire référence à un identificateur de ressource de chaîne à partir de XAML](#refer-to-a-string-resource-identifier-from-xaml), [faire référence à un identificateur de ressource de chaîne à partir du code](#refer-to-a-string-resource-identifier-from-code), ou [charger des chaînes à partir d’une bibliothèque de classes ou d’une bibliothèque de Windows Runtime](#load-strings-from-a-class-library-or-a-windows-runtime-library).

Pour utiliser des ressources dans des applications non empaquetées, vous devez effectuer quelques opérations :

1. Utilisez [GetForViewIndependentUse](/uwp/api/windows.applicationmodel.resources.resourceloader.getforviewindependentuse) au lieu de [GetForCurrentView](/uwp/api/windows.applicationmodel.resources.resourceloader.getforcurrentview) lors de la résolution de ressources à partir du code, car il n’existe pas d' *affichage actuel* dans les scénarios non packagés. L’exception suivante se produit si vous appelez [GetForCurrentView](/uwp/api/windows.applicationmodel.resources.resourceloader.getforcurrentview) dans des scénarios non packagés : les *contextes de ressources ne peuvent pas être créés sur des threads qui n’ont pas de CoreWindow.*
1. Utilisez [MakePri.exe](./compile-resources-manually-with-makepri.md) pour générer manuellement le fichier Resources. pri de votre application.
    - Exécutez `makepri new /pr <PROJECTROOT> /cf <PRICONFIG> /of resources.pri`
    - &lt;Fichier priconfig &gt; doit omettre la section « &lt; Packaging &gt; » pour que toutes les ressources soient regroupées dans un seul fichier Resources. pri. Si vous utilisez le [ fichier de configurationMakePri.exe](./makepri-exe-configuration.md) par défaut créé par [createconfig](./makepri-exe-command-options.md#createconfig-command), vous devez supprimer la &lt; section « Packaging &gt; » manuellement après sa création.
    - Le &lt; fichier priconfig &gt; doit contenir tous les indexeurs appropriés nécessaires pour fusionner toutes les ressources de votre projet dans un seul fichier Resources. pri. Le [ fichier de configurationMakePri.exe](./makepri-exe-configuration.md) par défaut créé par [createconfig](./makepri-exe-command-options.md#createconfig-command) comprend tous les indexeurs.
    - Si vous n’utilisez pas la configuration par défaut, assurez-vous que l’indexeur PRI est activé (consultez la configuration par défaut pour savoir comment procéder) pour fusionner les informations trouvées dans les références de projet UWP, les références NuGet, etc., qui se trouvent dans la racine du projet.
        > [!NOTE]
        > Si vous omettez `/IndexName` et que le projet n’a pas de manifeste d’application, l’espace de noms IndexName/root du fichier PRI est automatiquement défini sur *application*, que le runtime comprend pour les applications non empaquetées (cela supprime la dépendance matérielle précédente sur l’ID de package). Lorsque vous spécifiez des URI de ressource, les références de MS-Resource:///qui omettent l’espace de noms racine déduirent l' *application* en tant qu’espace de noms racine pour les applications non empaquetées (ou vous pouvez spécifier *application* explicitement comme dans MS-Resource://application/).
1. Copiez le fichier PRI dans le répertoire de sortie de génération du fichier. exe.
1. Exécuter le fichier. exe 
    > [!NOTE]
    > Le système de gestion des ressources utilise la langue d’affichage du système plutôt que la liste langue par défaut de l’utilisateur lors de la résolution des ressources en fonction de la langue des applications non empaquetées. La liste langue par défaut de l’utilisateur est utilisée uniquement pour les applications UWP.

> [!Important]
> Vous devez régénérer manuellement les fichiers PRI chaque fois que des ressources sont modifiées. Nous vous recommandons d’utiliser un script postérieur à la génération qui gère la commande [MakePri.exe](./compile-resources-manually-with-makepri.md) et copie la sortie Resources. pri dans le répertoire. exe.

## <a name="important-apis"></a>API importantes
* [ApplicationModel.Resources.ResourceLoader](/uwp/api/Windows.ApplicationModel.Resources.ResourceLoader)
* [ResourceContext. SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)
* [MapChanged](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live)

## <a name="related-topics"></a>Rubriques connexes
* [Portage du balisage XAML et de la couche interface utilisateur](../porting/wpsl-to-uwp-porting-xaml-and-ui.md#localization-and-globalization)
* [Directive x:Uid](../xaml-platform/x-uid-directive.md)
* [Propriétés jointes](../xaml-platform/attached-properties-overview.md)
* [Éléments de manifeste localisables](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live)
* [Balise de langue BCP-47](https://tools.ietf.org/html/bcp47)
* [Adaptez vos ressources à la langue, à la mise à l’échelle et à d’autres qualificateurs](tailor-resources-lang-scale-contrast.md)
* [Comment charger des ressources de type chaîne](/previous-versions/windows/apps/hh965323(v=win.10))