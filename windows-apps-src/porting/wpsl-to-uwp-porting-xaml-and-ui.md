---
description: La pratique de définition de l’interface utilisateur sous la forme de balisage XAML déclaratif se transpose extrêmement bien entre les applications Silverlight pour Windows Phone et les applications de plateforme Windows universelle (UWP).
title: Portage du balisage XAML et de la couche interface utilisateur de Silverlight pour Windows Phone vers UWP
ms.assetid: 49aade74-5dc6-46a5-89ef-316dbeabbebe
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9a6f78d30b1366078f2094aa17ab15c65a050c43
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164343"
---
#  <a name="porting-windowsphone-silverlight-xaml-and-ui-to-uwp"></a>Portage du balisage XAML et de la couche interface utilisateur de Silverlight pour Windows Phone vers UWP



Rubrique précédente : [Résolution des problèmes](wpsl-to-uwp-troubleshooting.md).

La pratique de définition de l’interface utilisateur sous la forme de balisage XAML déclaratif se transpose extrêmement bien entre les applications Silverlight pour Windows Phone et les applications de plateforme Windows universelle (UWP). Vous allez découvrir que des sections importantes de votre balisage sont compatibles une fois que vous avez mis à jour les références de clés des ressources système, modifié certains noms de type d’élément et remplacé « clr-namespace » par « using ». Une grande partie du code impératif de votre couche présentation (modèles d’affichage et code qui manipule les éléments d’interface utilisateur) peut également être portée directement.

## <a name="a-first-look-at-the-xaml-markup"></a>Découverte du balisage XAML

La rubrique précédente vous a indiqué comment copier vos fichiers XAML et code-behind dans votre nouveau projet Visual Studio Windows 10. L’un des premiers problèmes que vous remarquerez peut-être dans le concepteur XAML de Visual Studio est que l’élément `PhoneApplicationPage` figurant à la racine de votre fichier XAML n’est pas valide pour un projet de plateforme Windows universelle (UWP). Dans la rubrique précédente, vous avez enregistré une copie des fichiers XAML générés par Visual Studio lors de la création du projet Windows 10. Si vous ouvrez cette version de MainPage. xaml, vous verrez que, à la racine, il s’agit de la [**page**](/uwp/api/Windows.UI.Xaml.Controls.Page)de type, qui se trouve dans l’espace de noms [**Windows. UI. Xaml. Controls**](/uwp/api/Windows.UI.Xaml.Controls) . Vous pouvez donc remplacer tous les éléments `<phone:PhoneApplicationPage>` par `<Page>` (n’oubliez pas la syntaxe des éléments de propriété) et supprimer la déclaration `xmlns:phone`.

Pour une approche plus générale destinée à trouver le type UWP correspondant à un type Silverlight pour Windows Phone, vous pouvez vous reporter à la rubrique [Mappages des espaces de noms et des classes](wpsl-to-uwp-namespace-and-class-mappings.md).

## <a name="xaml-namespace-prefix-declarations"></a>Déclarations de préfixe d’espace de noms XAML


Si vous utilisez des instances de types personnalisés dans vos affichages (par exemple, une instance de modèle d’affichage ou un convertisseur de valeurs), vous aurez des déclarations de préfixe d’espace de noms XAML dans votre balisage XAML. Leur syntaxe diffère entre Silverlight pour Windows Phone et UWP. Voici quelques exemples :

```xml
    xmlns:ContosoTradingCore="clr-namespace:ContosoTradingCore;assembly=ContosoTradingCore"
    xmlns:ContosoTradingLocal="clr-namespace:ContosoTradingLocal"
```

Remplacez « clr-namespace » par « using » et supprimez le jeton d’assembly et le point-virgule (l’assembly sera déduit). Le résultat ressemble à ceci :

```xml
    xmlns:ContosoTradingCore="using:ContosoTradingCore"
    xmlns:ContosoTradingLocal="using:ContosoTradingLocal"
```

Vous pouvez avoir une ressource dont le type est défini par le système :

```xml
    xmlns:System="clr-namespace:System;assembly=mscorlib"
    /* ... */
    <System:Double x:Key="FontSizeLarge">40</System:Double>
```

Dans UWP, omettez la déclaration de préfixe « System » et utilisez à la place le préfixe « x » (déjà utilisé) :

```xml
    <x:Double x:Key="FontSizeLarge">40</x:Double>
```

## <a name="imperative-code"></a>Code impératif


Vos modèles d’affichage sont un emplacement où le code impératif référence des types d’interface utilisateur. Un autre emplacement correspond aux fichiers code-behind qui manipulent directement des éléments d’interface utilisateur. Par exemple, vous pouvez constater qu’une ligne de code semblable à celle-ci n’est pas encore compilée :


```csharp
    return new BitmapImage(new Uri(this.CoverImagePath, UriKind.Relative));
```

**BitmapImage** est dans l’espace de noms **System.Windows.Media.Imaging** de Silverlight pour Windows Phone, et une directive using dans le même fichier permet d’utiliser **BitmapImage** sans qualification d’espace de noms, comme dans l’extrait de code ci-dessus. Dans ce cas, vous pouvez cliquer avec le bouton droit sur le nom de type (**BitmapImage**) dans Visual Studio et utiliser la commande **Résoudre** du menu contextuel pour ajouter une nouvelle directive d’espace de noms au fichier. Dans ce cas, l’espace de noms [**Windows. UI. Xaml. Media. Imaging**](/uwp/api/Windows.UI.Xaml.Media.Imaging) est ajouté, où le type se trouve dans la série UWP. Vous pouvez supprimer la directive using **System.Windows.Media.Imaging**, et c’est tout ce que vous aurez à faire pour porter du code semblable à celui de l’extrait ci-dessus. Lorsque vous aurez terminé, vous aurez supprimé tous les espaces de noms de l’application Silverlight pour Windows Phone.

Dans des cas simples comme celui-ci, lorsque vous mappez les types d’un ancien espace de noms sur les mêmes types d’un nouvel espace de noms, vous pouvez utiliser la commande **Rechercher et remplacer** de Visual Studio pour apporter des modifications en bloc à votre code source. La commande **Résoudre** est un excellent moyen de découvrir le nouvel espace de noms d’un type. À titre d’exemple, vous pouvez remplacer toutes les instances « System.Windows » par « Windows.UI.Xaml ». Cela portera essentiellement toutes les directives using et tous les noms de type complet qui font référence à cet espace de noms.

Une fois toutes les anciennes directives using supprimées et les nouvelles ajoutées, vous pouvez utiliser la commande **Organiser les instructions Using** de Visual Studio pour trier vos directives et supprimer celles qui ne sont pas utilisées.

La correction du code impératif est parfois aussi mineure que la modification d’un type de paramètre. Dans d’autres cas, vous devrez utiliser Windows Runtime API au lieu des API .NET pour les applications Windows Runtime 8. x. Pour identifier les API prises en charge, utilisez le reste de ce guide de Portage en association avec [.net pour Windows Runtime vue d’ensemble des applications 8. x](/previous-versions/windows/apps/br230302(v=vs.140)) et la [référence Windows Runtime](/uwp/api/).

Et si vous souhaitez simplement accéder à l’étape de construction de votre projet, vous pouvez commenter ou remplacer tout code non essentiel. Vous pouvez ensuite itérer un problème à la fois et consulter les rubriques suivantes de cette section (ainsi que la rubrique précédente : [Résolution des problèmes](wpsl-to-uwp-troubleshooting.md)), jusqu’à ce que les problèmes de génération et d’exécution soient supprimés et le portage terminé.

## <a name="adaptiveresponsive-ui"></a>Interface utilisateur adaptative/réactive

Étant donné que votre application Windows 10 peut s’exécuter sur une large gamme d’appareils (présentant chacun différentes tailles d’écran et résolutions), vous pouvez compléter la procédure minimale de portage de votre application en adaptant votre interface utilisateur afin d’en optimiser l’aspect sur ces appareils. Vous pouvez utiliser la fonctionnalité adaptative Gestionnaire d’état visuel pour détecter dynamiquement la taille de la fenêtre et modifier la disposition en conséquence. Un exemple de procédure à suivre est décrit à la section [Interface utilisateur adaptative](wpsl-to-uwp-case-study-bookstore2.md) de la rubrique d’étude de cas Bookstore2.

## <a name="alarms-and-reminders"></a>Alarmes et rappels

Le code utilisant les classes **Alarm** ou **Reminder** doit être porté pour utiliser la classe [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) afin de créer et d’inscrire une tâche en arrière-plan et d’afficher un toast au moment approprié. Voir [Traitement en arrière-plan](wpsl-to-uwp-business-and-data.md) et [Toasts](#toasts).

## <a name="animation"></a>Animation

Solution de substitution préférée aux animations d’image clé et aux animations from/to, la bibliothèque d’animations UWP est disponible pour les applications UWP. Ces animations ont été conçues et ajustées pour que leur exécution soit parfaite et leur apparence remarquable, et pour que votre application soit aussi intégrée à Windows que les applications dites intégrées. Voir l’article [Démarrage rapide : animation de votre interface utilisateur avec des animations de la bibliothèque](/previous-versions/windows/apps/hh452703(v=win.10)).

Si vous utilisez des animations d’image clé ou des animations from/to dans vos applications UWP, vous souhaiterez peut-être comprendre la distinction entre les animations dépendantes et indépendantes introduites par la nouvelle plateforme. Voir [Optimiser les animations et le contenu multimédia](../debug-test-perf/optimize-animations-and-media.md). Les animations qui s’exécutent sur le thread d’interface utilisateur (celles qui animent les propriétés de disposition par exemple) sont appelées animations dépendantes. Si elles s’exécutent sur la nouvelle plateforme, elles n’ont aucun effet à moins que vous n’effectuiez l’une des deux opérations suivantes. Vous pouvez les recibler pour animer des propriétés différentes, telles que [**RenderTransform**](/uwp/api/windows.ui.xaml.uielement.rendertransform), de manière à ce qu’elles soient indépendantes. Vous pouvez également définir `EnableDependentAnimation="True"` dans l’élément d’animation afin de confirmer votre intention d’exécuter une animation dont l’exécution parfaite ne peut pas être garantie. Si vous utilisez Blend pour Visual Studio pour créer des animations, cette propriété sera définie pour vous si nécessaire.

## <a name="back-button-handling"></a>Gestion du bouton Précédent

Dans une application Windows 10, vous pouvez adopter une approche unique en matière de gestion du bouton Précédent, et cette approche fonctionnera alors sur tous les appareils. Sur les appareils mobiles, le bouton est fourni à votre intention sous la forme d’un bouton capacitif sur l’appareil ou d’un bouton dans l’interpréteur de commandes. Sur un appareil de bureau, vous ajoutez un bouton au chrome de votre application chaque fois que cette dernière permet la navigation vers l’arrière. Ceci est indiqué dans la barre de titre des applications avec fenêtres ou dans la barre des tâches en mode tablette. L’événement du bouton précédent est un concept universel dans toutes les familles d’appareils, et les boutons implémentés dans le matériel ou dans les logiciels déclenchent le même événement de [**requête**](/uwp/api/windows.ui.core.systemnavigationmanager.backrequested) .

L’exemple ci-après fonctionne pour toutes les familles d’appareils et est adapté aux cas dans lesquels le même traitement s’applique à toutes les pages et où vous n’avez pas besoin de confirmer la navigation (par exemple, pour signaler les modifications non enregistrées).

```csharp
   // app.xaml.cs

    protected override void OnLaunched(LaunchActivatedEventArgs e)
    {
        [...]

        Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested += App_BackRequested;
        rootFrame.Navigated += RootFrame_Navigated;
    }

    private void RootFrame_Navigated(object sender, NavigationEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;

        // Note: On device families that have no title bar, setting AppViewBackButtonVisibility can safely execute 
        // but it will have no effect. Such device families provide a back button UI for you.
        if (rootFrame.CanGoBack)
        {
            Windows.UI.Core.SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
                Windows.UI.Core.AppViewBackButtonVisibility.Visible;
        }
        else
        {
            Windows.UI.Core.SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
                Windows.UI.Core.AppViewBackButtonVisibility.Collapsed;
        }
    }

    private void App_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;

        if (rootFrame.CanGoBack)
        {
            rootFrame.GoBack();
        }
    }
```

Il existe également une approche unique pour toutes les familles d’appareils concernant la fermeture de l’application par programme.

```csharp
   Windows.UI.Xaml.Application.Current.Exit();
```

## <a name="binding-and-compiled-bindings-with-xbind"></a>Liaison et liaisons compilées avec {x: Bind}

La rubrique relative à la liaison inclut les aspects suivants :

-   Liaison d’un élément d’interface utilisateur aux « données » (autrement dit, aux propriétés et aux commandes d’un modèle d’affichage)
-   Liaison d’un élément d’interface utilisateur à un autre élément d’interface utilisateur
-   Écriture d’un modèle d’affichage observable (autrement dit, il déclenche des notifications en cas de modification d’une valeur de propriété et de la disponibilité d’une commande)

Tous ces aspects restent majoritairement pris en charge, mais il existe des différences relatives aux espaces de noms. Par exemple, **System.Windows.Data.Binding** correspond à [**Windows.UI.Xaml.Data.Binding**](/uwp/api/Windows.UI.Xaml.Data.Binding), **System.ComponentModel.INotifyPropertyChanged** correspond à [**Windows.UI.Xaml.Data.INotifyPropertyChanged**](/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged) et **System.Collections.Specialized.INotifyPropertyChanged** correspond à [**Windows.UI.Xaml.Interop.INotifyCollectionChanged**](/uwp/api/Windows.UI.Xaml.Interop.INotifyCollectionChanged).

Les barres de l’application Silverlight pour Windows Phone et les boutons de la barre de l’application ne peuvent pas être liés de la même façon que dans une application UWP. Vous pouvez avoir du code impératif qui construit votre barre de l’application et ses boutons, les lie aux propriétés et aux chaînes localisées et gère leurs événements. Le cas échéant, vous pouvez maintenant porter ce code impératif en le remplaçant par un balisage déclaratif lié aux propriétés et aux commandes, avec des références de ressources statiques, ce qui renforce la sécurité et la maintenabilité de votre application de façon incrémentielle. Vous pouvez utiliser Visual Studio ou Blend pour Visual Studio pour lier et styliser les boutons de la barre de l’application UWP comme tout autre élément XAML. Notez que dans une application UWP, les noms de types que vous utilisez sont [**CommandBar**](/uwp/api/Windows.UI.Xaml.Controls.CommandBar) et [**AppBarButton**](/uwp/api/Windows.UI.Xaml.Controls.AppBarButton).

Les fonctionnalités associées aux liaisons des applications UWP présentent actuellement les limitations suivantes :

-   Il n’existe pas de prise en charge intégrée de la validation des entrées de données et des interfaces [**IDataErrorInfo**](/dotnet/api/system.componentmodel.idataerrorinfo) et [**INotifyDataErrorInfo**](/dotnet/api/system.componentmodel.inotifydataerrorinfo) .
-   La classe [**Binding**](/uwp/api/Windows.UI.Xaml.Data.Binding) n’inclut pas les propriétés de mise en forme étendues disponibles dans Silverlight pour Windows Phone. Toutefois, vous pouvez implémenter [**IValueConverter**](/uwp/api/Windows.UI.Xaml.Data.IValueConverter) pour produire une mise en forme personnalisée.
-   Les méthodes [**IValueConverter**](/uwp/api/Windows.UI.Xaml.Data.IValueConverter) acceptent des chaînes de langage comme paramètres au lieu d’objets [**CultureInfo**](/dotnet/api/system.globalization.cultureinfo).
-   La classe [**CollectionViewSource**](/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) ne fournit pas de prise en charge intégrée pour le tri et le filtrage, et le regroupement fonctionne différemment. Pour plus d’informations, voir [Présentation détaillée de la liaison de données](../data-binding/data-binding-in-depth.md) et [Exemple de liaison de données](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBind).

Bien que les mêmes fonctionnalités de liaison soient toujours majoritairement prises en charge, Windows 10 offre un nouveau mécanisme de liaison plus performant appelé liaisons compilées, qui utilisent l’extension de balisage {x:Bind}. Voir [Liaison de données : accroître les performances de votre application grâce aux nouvelles améliorations de la liaison de données XAML](https://channel9.msdn.com/Events/Build/2015/3-635) et [Exemple x:Bind](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBind) (en anglais).

## <a name="binding-an-image-to-a-view-model"></a>Liaison d’une propriété Image à un modèle d’affichage

Vous pouvez lier la propriété [**image. source**](/uwp/api/windows.ui.xaml.controls.image.source) à toute propriété d’un modèle de vue de type [**ImageSource**](/uwp/api/Windows.UI.Xaml.Media.ImageSource). Voici une implémentation standard d’une telle propriété dans une application Silverlight pour Windows Phone :

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(this.CoverImagePath, UriKind.Relative));
```

Dans une application UWP, vous utilisez le [schéma d’URI](/previous-versions/windows/apps/jj655406(v=win.10)) ms-appx. Pour pouvoir conserver le reste de votre code identique, vous pouvez utiliser une surcharge différente du constructeur **System.Uri** pour placer le schéma d’URI ms-appx dans un URI de base et y ajouter le reste du chemin d’accès. Comme ceci :

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(new Uri("ms-appx://"), this.CoverImagePath));
```

De cette façon, le reste du modèle d’affichage, les valeurs de chemin d’accès dans la propriété de chemin d’accès à l’image et les liaisons dans le balisage XAML peuvent tous rester identiques.

## <a name="controls-and-control-stylestemplates"></a>Contrôles et styles/modèles de contrôle

Les applications Silverlight pour Windows Phone utilisent les contrôles définis dans les espaces de noms **Microsoft.Phone.Controls** et **System.Windows.Controls**. Les applications UWP XAML utilisent des contrôles définis dans l’espace de noms [**Windows. UI. Xaml. Controls**](/uwp/api/Windows.UI.Xaml.Controls) . L’architecture et la conception des contrôles XAML dans UWP sont pratiquement identiques à celles des contrôles Silverlight pour Windows Phone. Toutefois, certaines modifications ont été apportées pour améliorer l’ensemble des contrôles disponibles et les unifier avec les applications Windows. En voici des exemples spécifiques.

| Nom du contrôle | Changement |
|--------------|--------|
| ApplicationBar | Propriété [Page.TopAppBar](/uwp/api/windows.ui.xaml.controls.page.topappbar). |
| ApplicationBarIconButton | L’équivalent UWP est la propriété [Glyph](/uwp/api/windows.ui.xaml.controls.fonticon.glyph). PrimaryCommands est la propriété de contenu du contrôle CommandBar. L’analyseur XAML interprète le code xml interne d’un élément comme la valeur de sa propriété de contenu. |
| ApplicationBarMenuItem | L’équivalent UWP est la propriété [AppBarButton.Label](/uwp/api/windows.ui.xaml.controls.appbarbutton.label) définie sur le texte de l’élément de menu. |
| ContextMenu (dans le kit de ressources pour Windows Phone) | Dans le cas d’un menu volant à sélection unique, utilisez [Flyout](/uwp/api/Windows.UI.Xaml.Controls.Flyout). |
| ControlTiltEffect.TiltEffect class | Des animations de la bibliothèque d’animations UWP sont intégrées aux styles par défaut des contrôles courants. Voir [Animation des actions de pointeur](/previous-versions/windows/apps/jj649432(v=win.10)). |
| LongListSelector avec des données groupées | Le contrôle LongListSelector de Silverlight pour Windows Phone fonctionne de deux façons, qui peuvent être utilisées conjointement. Tout d’abord, il peut afficher les données groupées en fonction d’une clé, par exemple la liste des noms groupés en fonction de leur lettre initiale. Ensuite, il peut « zoomer » entre deux affichages sémantiques : la liste groupée des éléments (par exemple, les noms) et la liste des clés de groupe proprement dites (par exemple, les lettres initiales). Avec la plateforme UWP, vous pouvez afficher les données groupées à l’aide des contrôles ListView et GridView (voir les [Recommandations en matière d’affichage Liste et Grille](../design/controls-and-patterns/lists.md)). |
| LongListSelector avec des données mises à plat | Pour des raisons de performance, dans le cas de très longues listes, nous recommandons le contrôle LongListSelector au lieu d’une zone de liste Silverlight pour Windows Phone, même pour les données non groupées mises à plat. Dans une application UWP, les contrôles [GridView](/uwp/api/Windows.UI.Xaml.Controls.GridView) sont privilégiés pour les longues listes d’éléments, que les données soient ou non prêtes pour le regroupement. |
| Panorama | Le contrôle Panorama de Silverlight Windows Phone correspond aux [recommandations pour les contrôles de Hub dans Windows Runtime applications 8. x](../design/basics/navigation-basics.md) et aux instructions pour le contrôle Hub. <br/> Notez qu’un contrôle Panorama exécute une boucle entre la dernière section et la première, et que son image d’arrière-plan se déplace en parallaxe par rapport aux sections. Les sections de [Hub](/uwp/api/Windows.UI.Xaml.Controls.Hub) n’exécutent aucune boucle, et l’effet parallaxe n’est pas utilisé. |
| Tableau croisé dynamique | L’équivalent UWP du contrôle Pivot de Silverlight pour Windows Phone est [Windows.UI.Xaml.Controls.Pivot](/uwp/api/Windows.UI.Xaml.Controls.Pivot). Il est disponible pour toutes les familles d’appareils. |

**Remarque**    L’état visuel PointerOver est pertinent dans les styles/modèles personnalisés dans les applications Windows 10, mais pas dans Windows Phone les applications Silverlight. Il existe d’autres raisons pour lesquelles vos styles/modèles personnalisés existants peuvent ne pas convenir pour les applications Windows 10, notamment les clés de ressources système que vous utilisez, les modifications apportées aux jeux d’états visuels utilisés et les améliorations de performances appliquées aux styles/modèles par défaut de Windows 10. Nous vous recommandons de modifier une nouvelle copie d’un modèle de contrôle par défaut pour Windows 10, puis de lui réappliquer votre style et votre personnalisation de modèle.

Pour plus d’informations sur les contrôles UWP, voir [Contrôles par fonction](../design/controls-and-patterns/controls-by-function.md), [Liste des contrôles](../design/controls-and-patterns/index.md) et [Recommandations relatives aux contrôles](../design/controls-and-patterns/index.md).

##  <a name="design-language-in-windows10"></a>Langage de conception dans Windows 10

Il existe certaines différences de langage de conception entre les applications Silverlight pour Windows Phone et les applications Windows 10. Pour plus de détails, voir [Conception](https://developer.microsoft.com/windows/apps/design). Malgré les changements en matière de langage, nos principes de conception restent cohérents : être attentif aux détails, mais toujours viser la simplicité en se concentrant sur le contenu sans superflu, en réduisant à tout prix les éléments visuels et en restant authentique en matière de domaine numérique ; utiliser la hiérarchie visuelle, en particulier avec la typographie ; concevoir à l’aide d’une grille et donner vie à vos expériences grâce à des animations fluides.

## <a name="localization-and-globalization"></a>Localisation et globalisation

Pour les chaînes localisées, vous pouvez réutiliser le fichier .resx de votre projet Silverlight pour Windows Phone dans votre projet d’application UWP. Copiez le fichier, ajoutez-le au projet et renommez-le Resources.resw pour que le mécanisme de recherche le trouve par défaut. Définissez **Action de génération** sur **PRIResource** et **Copier dans le répertoire de sortie** sur **Ne pas copier**. Vous pouvez ensuite utiliser les chaînes dans le balisage en spécifiant l’attribut **x:Uid** dans vos éléments XAML. Voir [Démarrage rapide : utilisation de ressources de chaîne](/previous-versions/windows/apps/hh965329(v=win.10)).

Les applications Silverlight pour Windows Phone utilisent la classe **CultureInfo** pour aider à globaliser une application. Les applications UWP utilisent MRT (Modern Resource Technology), qui permet le chargement dynamique des ressources d’application (localisation, échelle et thème) lors de l’exécution et dans l’aire de conception de Visual Studio. Pour plus d’informations, voir [Recommandations relatives aux fichiers, aux données et à la globalisation](../design/usability/index.md).

La rubrique [**ResourceContext. QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.qualifiervalues) explique comment charger des ressources spécifiques à la famille d’appareils en fonction du facteur de sélection des ressources de la famille d’appareils.

## <a name="media-and-graphics"></a>Média et graphismes

Au moment où vous lisez la section relative au média et aux graphismes d’UWP, gardez à l’esprit que les principes de conception de Windows favorisent la réduction drastique des éléments superflus, notamment l’encombrement et la complexité graphiques. La conception de Windows se caractérise par des éléments visuels, une typographie et un mouvement nets et clairs. Si votre application suit ces mêmes principes, elle ressemblera davantage aux applications intégrées.

Silverlight pour Windows Phone dispose d’un type **RadialGradientBrush** qui n’est pas présent dans UWP, contrairement aux autres types [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush). Dans certains cas, vous serez en mesure d’obtenir un effet similaire avec une image bitmap. Notez que vous pouvez [créer un pinceau dégradé radial](/windows/desktop/Direct2D/how-to-create-a-radial-gradient-brush) avec Direct2D dans une application UWP [Microsoft DirectX](/windows/desktop/directx) et XAML en C++.

Silverlight pour Windows Phone comporte la propriété **System.Windows.UIElement.OpacityMask**, mais celle-ci n’est pas membre du type  [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) d’UWP. Dans certains cas, vous serez en mesure d’obtenir un effet similaire avec une image bitmap. Vous pouvez également [créer un masque d’opacité](/windows/desktop/Direct2D/opacity-masks-overview) avec Direct2D dans une application UWP [Microsoft DirectX](/windows/desktop/directx) et XAML en C++. Néanmoins, un cas d’utilisation courante de la propriété **OpacityMask** consiste à utiliser une seule image bitmap qui s’adapte aux thèmes clair et foncé. Pour les graphiques vectoriels, vous pouvez utiliser les pinceaux système thématiques (par exemple, les graphiques en secteurs illustrés ci-dessous). Pour rendre une image bitmap thématique (par exemple, les coches illustrées ci-dessous), vous devez utiliser une approche différente.

![Image bitmap thématique](images/wpsl-to-uwp-case-studies/wpsl-to-uwp-theme-aware-bitmap.png)

Dans une application Silverlight pour Windows Phone, la technique consiste à utiliser un masque alpha (sous la forme d’une image bitmap) en tant qu’élément **OpacityMask** d’un **Rectangle** rempli avec le pinceau de premier plan :

```xml
    <Rectangle Fill="{StaticResource PhoneForegroundBrush}" Width="26" Height="26">
        <Rectangle.OpacityMask>
            <ImageBrush ImageSource="/Assets/wpsl_check.png"/>
        </Rectangle.OpacityMask>
    </Rectangle>
```

La façon la plus simple de porter celui-ci vers une application UWP consiste à utiliser un contrôle [**BitmapIcon**](/uwp/api/Windows.UI.Xaml.Controls.BitmapIcon), comme ceci :

```xml
    <BitmapIcon UriSource="Assets/winrt_check.png" Width="21" Height="21"/>
```

Ici, WinRT \_check.png est un masque Alpha sous la forme d’une bitmap comme wpsl \_check.png est, et il peut être très bien le même fichier. Toutefois, vous souhaiterez peut-être fournir plusieurs tailles différentes de \_check.png WinRT à utiliser pour différents facteurs de mise à l’échelle. Pour plus d’informations sur cela et pour obtenir une explication des modifications apportées aux valeurs de **largeur** et de **hauteur** , consultez [affichage ou pixels effectifs, distance d’affichage et facteurs d’échelle](#view-or-effective-pixels-viewing-distance-and-scale-factors) dans cette rubrique.

Une approche plus générale, qui est appropriée en cas de différences entre les thèmes clair et foncé d’une image bitmap, consiste à utiliser deux composants d’image : l’un avec un premier plan foncé (pour le thème clair) et l’autre avec un premier plan clair (pour le thème foncé). Pour plus d’informations sur la façon de nommer cet ensemble de ressources bitmap, consultez [adapter vos ressources pour la langue, la mise à l’échelle et d’autres qualificateurs](../app-resources/tailor-resources-lang-scale-contrast.md). Une fois qu’un ensemble de fichiers image a été correctement nommé, vous pouvez le désigner dans le résumé, à l’aide de son nom racine, comme ceci :

```xml
    <Image Source="Assets/winrt_check.png" Stretch="None"/>
```

Dans Silverlight pour Windows Phone, la propriété **UIElement.Clip** peut être de toute forme que vous pouvez exprimer avec un contrôle **Geometry** et qui est généralement sérialisée dans le balisage XAML dans le minilangage **StreamGeometry**. Dans la série UWP, le type de la propriété [**clip**](/uwp/api/windows.ui.xaml.uielement.clip) est [**RectangleGeometry**](/uwp/api/Windows.UI.Xaml.Media.RectangleGeometry), de sorte que vous pouvez uniquement découper une région rectangulaire. Permettre à un rectangle d’être défini à l’aide du minilangage serait trop permissif. Ainsi, pour porter une zone de détourage dans le balisage, remplacez la syntaxe de l’attribut **Clip** par la syntaxe d’élément de propriété, comme ceci :

```xml
    <UIElement.Clip>
        <RectangleGeometry Rect="10 10 50 50"/>
    </UIElement.Clip>
```

Notez que vous pouvez [utiliser une géométrie arbitraire sous forme de masque dans une couche](/windows/desktop/Direct2D/direct2d-layers-overview) avec Direct2D dans une application UWP [Microsoft DirectX](/windows/desktop/directx) et XAML en C++.

## <a name="navigation"></a>Navigation

Lorsque vous accédez à une page dans une application Silverlight pour Windows Phone, vous utilisez un schéma d’adressage URI (Uniform Resource Identifier) :

```csharp
    NavigationService.Navigate(new Uri("/AnotherPage.xaml", UriKind.Relative)/*, navigationState*/);
```

Dans une application UWP, vous appelez la méthode [**Frame.Navigate**](/uwp/api/windows.ui.xaml.controls.frame.navigate) et spécifiez le type de la page de destination (tel que défini par l’attribut **x:Class** de définition du balisage XAML de la page) :


```csharp
    // In a page:
    this.Frame.Navigate(typeof(AnotherPage)/*, parameter*/);

    // In a view model, perhaps inside an ICommand implementation:
    var rootFrame = Windows.UI.Xaml.Window.Current.Content as Windows.UI.Xaml.Controls.Frame;
    rootFrame.Navigate(typeof(AnotherPage)/*, parameter*/);
```

Définissez la page de démarrage d’une application Silverlight pour Windows Phone dans WMAppManifest.xml :

```xml
    <DefaultTask Name="_default" NavigationPage="MainPage.xaml" />
```

Dans une application UWP, utilisez du code impératif pour définir la page de démarrage. Voici du code d’App.xaml.cs indiquant comment procéder :

```csharp
    if (!rootFrame.Navigate(typeof(MainPage), e.Arguments))
```

Le mappage d’URI et la navigation par fragment sont des techniques de navigation d’URI. Par conséquent, ils ne sont pas applicables à la navigation UWP, qui ne repose pas sur les URI. Le mappage d’URI existe en réponse à la nature faiblement typée de l’identification d’une page cible avec une chaîne d’URI, ce qui conduit à des problèmes de fragilité et de maintenabilité si la page est déplacée dans un autre dossier et donc dans un autre chemin d’accès relatif. Les applications UWP utilisent la navigation basée sur le type, qui est fortement typée et vérifiée par compilateur, et qui ne présente pas le problème que le mappage d’URI résout. Le cas d’utilisation de la navigation par fragment consiste à transmettre un contexte à la page cible pour que la page puisse entraîner le défilement d’un fragment particulier de son contenu dans l’affichage, ou l’affichage de ce fragment sous une autre forme. Le même objectif peut être atteint en passant un paramètre de navigation lorsque vous appelez la méthode [**Navigate**](/uwp/api/windows.ui.xaml.controls.frame.navigate) .

Pour plus d’informations, voir [Navigation](../design/basics/navigation-basics.md).

## <a name="resource-key-reference"></a>Référence aux clés de ressources

Le langage de conception a évolué pour Windows 10. Suite à cela, certains styles système ont été modifiés, et de nombreuses clés de ressources système ont été supprimées ou renommées. L’éditeur de balisage XAML dans Visual Studio met en surbrillance les références aux clés de ressources qui ne peuvent pas être résolues. Par exemple, il souligne une référence à la clé de style `PhoneTextNormalStyle` d’une ligne ondulée rouge. Si ce n’est pas corrigé, l’application s’arrête immédiatement lorsque vous essayez de la déployer vers l’émulateur ou l’appareil. Il est donc important de veiller à l’exactitude du balisage XAML. Et vous allez découvrir que Visual Studio est un formidable outil pour intercepter ces problèmes.

Voir également la section [Texte](#text) ci-dessous.

## <a name="status-bar-system-tray"></a>Barre d’état (zone de notification)

La barre d’état système (définie dans le balisage XAML avec `shell:SystemTray.IsVisible`) est désormais appelée barre d’état. Elle s’affiche par défaut. Vous pouvez contrôler sa visibilité dans du code impératif en appelant les méthodes [**Windows. UI. ViewManagement. StatusBar. ShowAsync**](/uwp/api/windows.ui.viewmanagement.statusbar.showasync) et [**HideAsync**](/uwp/api/windows.ui.viewmanagement.statusbar.hideasync) .

## <a name="text"></a>Text

Le texte (ou la typographie) constitue un aspect important d’une application UWP et, pendant le portage, il vous sera peut-être utile de revoir les conceptions visuelles de vos vues afin de les harmoniser avec le nouveau langage de conception. Utilisez ces illustrations pour identifier les styles  **TextBlock** système d’UWP disponibles. Recherchez ceux qui correspondent aux styles de Silverlight pour Windows Phone que vous avez utilisés. Vous pouvez également créer vos propres styles universels et y copier les propriétés des styles système de Silverlight pour Windows Phone.

![Styles TextBlock système pour les applications Windows 10](images/label-uwp10stylegallery.png)

Styles TextBlock système pour les applications Windows 10

Dans une application Silverlight pour Windows Phone, la famille de polices par défaut est Segoe WP. Dans une application Windows 10, la famille de polices par défaut est Segoe UI. Par conséquent, les métriques de police dans votre application peuvent être différentes. Si vous souhaitez reproduire l’aspect de votre texte Silverlight pour Windows Phone, vous pouvez définir vos propres métriques à l’aide de propriétés telles que [**LineHeight**](/uwp/api/windows.ui.xaml.controls.textblock.lineheight) et [**LineStackingStrategy**](/uwp/api/windows.ui.xaml.controls.textblock.linestackingstrategy). Pour plus d’informations, voir [Recommandations en matière de polices](https://docs.microsoft.com/windows/uwp/controls-and-patterns/fonts) et [Concevoir des applications UWP](https://developer.microsoft.com/windows/apps/design).

## <a name="theme-changes"></a>Modifications de thème

Dans une application Silverlight pour Windows Phone, le thème par défaut est sombre. Pour les appareils Windows 10, le thème par défaut a changé, mais vous pouvez contrôler le thème utilisé en déclarant un thème demandé dans App.xaml. Par exemple, pour utiliser un thème sombre sur tous les appareils, ajoutez `RequestedTheme="Dark"` à l’élément racine Application.

## <a name="tiles"></a>Vignettes

Les vignettes des applications UWP ont des comportements semblables aux vignettes dynamiques des applications Silverlight pour Windows Phone, à quelques différences près. Par exemple, le code qui appelle la méthode **Microsoft.Phone.Shell.ShellTile.Create** pour créer des vignettes secondaires doit être porté pour appeler [**SecondaryTile.RequestCreateAsync**](/uwp/api/windows.ui.startscreen.secondarytile.requestcreateasync). Voici un exemple de type avant/après. Tout d’abord, la version Silverlight pour Windows Phone :


```csharp
    var tileData = new IconicTileData()
    {
        Title = this.selectedBookSku.Title,
        WideContent1 = this.selectedBookSku.Title,
        WideContent2 = this.selectedBookSku.Author,
        SmallIconImage = this.SmallIconImageAsUri,
        IconImage = this.IconImageAsUri
    };

    ShellTile.Create(this.selectedBookSku.NavigationUri, tileData, true);
```

Et son équivalent UWP :

```csharp
    var tile = new SecondaryTile(
        this.selectedBookSku.Title.Replace(" ", string.Empty),
        this.selectedBookSku.Title,
        this.selectedBookSku.ArgumentString,
        this.IconImageAsUri,
        TileSize.Square150x150);

    await tile.RequestCreateAsync();
```

Le code qui met à jour une vignette avec la méthode **Microsoft.Phone.Shell.ShellTile.Update** ou la classe **Microsoft.Phone.Shell.ShellTileSchedule**, doit être porté pour utiliser les classes [**TileUpdateManager**](/uwp/api/Windows.UI.Notifications.TileUpdateManager), [**TileUpdater**](/uwp/api/Windows.UI.Notifications.TileUpdater), [**TileNotification**](/uwp/api/Windows.UI.Notifications.TileNotification) ou [**ScheduledTileNotification**](/uwp/api/Windows.UI.Notifications.ScheduledTileNotification).

Pour plus d’informations sur les vignettes, les toasts, les badges, les bannières et les notifications, voir [Création de vignettes](/previous-versions/windows/apps/hh868260(v=win.10)) et [Utilisation de vignettes, de badges et de notifications toast](/previous-versions/windows/apps/hh868259(v=win.10)). Pour obtenir des informations spécifiques sur les différentes tailles des composants visuels utilisés pour les vignettes UWP, voir [Composants visuels des vignettes et des toasts](/previous-versions/windows/apps/hh781198(v=win.10)).

## <a name="toasts"></a>Toasts

Le code qui affiche un toast avec la classe **Microsoft.Phone.Shell.ShellToast** doit être porté pour utiliser les classes [**ToastNotificationManager**](/uwp/api/Windows.UI.Notifications.ToastNotificationManager), [**ToastNotifier**](/uwp/api/Windows.UI.Notifications.ToastNotifier), [**ToastNotification**](/uwp/api/Windows.UI.Notifications.ToastNotification) ou [**ScheduledToastNotification**](/uwp/api/Windows.UI.Notifications.ScheduledToastNotification). Notez que sur les appareils mobiles, le terme grand public correspondant à « toast » est « bannière ».

Voir [Utilisation de vignettes, de badges et de notifications toast](/previous-versions/windows/apps/hh868259(v=win.10)).

## <a name="view-or-effective-pixels-viewing-distance-and-scale-factors"></a>Afficher ou compter les pixels, la distance d’affichage et les facteurs d’échelle

Les applications Silverlight pour Windows Phone n’appliquent pas la même méthode que les applications Windows 10 pour abstraire la taille et la disposition des éléments d’interface utilisateur de la taille physique et de la résolution réelles des appareils. Pour ce faire, une application Silverlight pour Windows Phone utilise des pixels d’affichage. Avec Windows 10, le concept de pixels d’affichage a été affiné en pixels effectifs. Voici une explication de ce terme, sa signification et la valeur supplémentaire qu’il offre.

Le terme « résolution » fait référence à la mesure de la densité des pixels et non, comme on le pense souvent, au nombre de pixels. La « résolution effective » est la façon dont les pixels physiques qui composent une image ou un glyphe apparaissent à l’œil, étant donné les différences liées à la distance de visualisation et à la taille des pixels physiques sur l’appareil (la densité de pixels étant l’inverse de la taille des pixels physiques). La résolution effective est une bonne unité de mesure pour créer une expérience, car elle est centrée sur l’utilisateur. La compréhension de tous ces facteurs et le contrôle de la taille des éléments d’interface utilisateur vous permettent de tirer parti de l’expérience utilisateur.

Dans une application Silverlight pour Windows Phone, tous les écrans de téléphone ont une largeur exacte de 480 pixels d’affichage, sans exception, quel que soit le nombre de pixels physiques de l’écran, sa densité de pixels ou sa taille physique. En d’autres termes, un élément **Image** pour lequel `Width="48"` correspond exactement à un dixième de la largeur de l’écran de tout téléphone pouvant exécuter l’application Silverlight pour Windows Phone.

Pour une application Windows 10, les écrans des appareils ne présentent *pas* tous une largeur d’un nombre fixe de pixels effectifs. Ceci est probablement évident, étant donné le large éventail d’appareils sur lesquels une application UWP peut s’exécuter. Les différents appareils utilisés présentent une largeur variable (en pixels effectifs). Celle-ci est de 320 epx sur les plus petits d’entre eux, de 1 024 epx sur les écrans de taille modeste et nettement plus grande sur d’autres. Il vous suffit de continuer à utiliser les éléments à dimensionnement automatique et les panneaux à disposition dynamique que vous utilisez depuis toujours. Dans certains cas, il se peut que vous définissiez une taille fixe pour les propriétés de vos éléments d’interface utilisateur dans le balisage XAML. Un facteur d’échelle est automatiquement affecté à votre application, en fonction de l’appareil sur lequel elle s’exécute et des paramètres d’affichage définis par l’utilisateur. Ce facteur permet aux éléments à taille fixe de l’interface utilisateur de conserver la même taille (approximativement) sur les écrans de différentes tailles de l’utilisateur, pour les opérations tactiles ou pour la lecture. Et avec la disposition dynamique, votre interface utilisateur ne sera pas seulement mise à l’échelle sur différents appareils : elle s’efforcera d’adapter la quantité de contenu appropriée à l’espace disponible.

Étant donné que les écrans de téléphone présentaient auparavant une largeur fixe de 480 pixels d’affichage, et que cette valeur est désormais généralement plus faible en nombre de pixels effectifs, une règle de base consiste à multiplier toute dimension indiquée dans votre balisage d’application Silverlight pour Windows Phone par un facteur de 0,8.

Pour que votre application offre une expérience optimale sur tous les écrans, nous vous recommandons de créer chaque ressource bitmap dans différentes tailles, chacune étant adaptée à un facteur d’échelle spécifique. Fournir des ressources aux échelles 100 %, 200 % et 400 % (dans cet ordre de priorité) produit d’excellents résultats dans la plupart des cas à tous les facteurs d’échelle intermédiaires.

**Remarque**    Si, pour une raison quelconque, vous ne pouvez pas créer de ressources en plusieurs tailles, créez des ressources de 100%. Dans Microsoft Visual Studio, le modèle de projet par défaut pour les applications UWP fournit des ressources de personnalisation (vignettes et logos) dans une seule taille, mais elles ne sont pas à l’échelle 100 %. Lorsque vous créez des ressources pour votre propre application, suivez les recommandations de cette section, fournissez des tailles 100 %, 200 % et 400 %, et utilisez des packs de ressources.

Si vous disposez d’illustrations complexes, vous serez peut-être amené à fournir vos ressources dans un plus grand nombre de tailles. Si vous débutez avec une image vectorielle, il est relativement aisé de générer des ressources de haute qualité à n’importe quel facteur d’échelle.

Nous ne vous recommandons pas d’essayer de prendre en charge tous les facteurs d’échelle, mais la liste complète des facteurs d’échelle pour les applications Windows 10 est la suivante : 100 %, 125 %, 150 %, 200 %, 250 %, 300 % et 400 %. Si vous fournissez ces facteurs, le Windows Store sélectionne les ressources de taille appropriée pour chaque appareil, et seules ces ressources sont téléchargées. Le Windows Store sélectionne les ressources à télécharger en fonction de la résolution de l’appareil.

Pour en savoir plus, voir [Conception réactive 101 pour les applications UWP](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md).

## <a name="window-size"></a>Taille de la fenêtre

Dans votre application UWP, vous pouvez spécifier une taille minimale (largeur et hauteur) avec le code impératif. La taille minimale par défaut est 500 x 320 epx (plus petite taille minimale acceptée). La plus grande taille minimale acceptée est de 500 x 500 epx.

```csharp
   Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetPreferredMinSize
        (new Size { Width = 500, Height = 500 });
```

Rubrique suivante : [Portage pour le modèle d’E/S, d’appareil et d’application](wpsl-to-uwp-input-and-sensors.md).

## <a name="related-topics"></a>Rubriques connexes

* [Mappages des espaces de noms et des classes](wpsl-to-uwp-namespace-and-class-mappings.md)