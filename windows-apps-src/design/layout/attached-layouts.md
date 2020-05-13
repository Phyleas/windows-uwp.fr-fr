---
Description: Vous pouvez définir des dispositions attachées pour les utiliser avec des conteneurs comme le contrôle ItemsRepeater.
title: Disposition attachée
label: AttachedLayout
template: detail.hbs
ms.date: 03/13/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9ee88e32ed0ce0fd193fe79e48814a11f494d062
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970214"
---
# <a name="attached-layouts"></a>Dispositions attachées

Un conteneur (par exemple, Panel) qui délègue sa logique de disposition à un autre objet s’appuie sur l’objet de disposition attachée pour fournir le comportement de disposition à ses éléments enfants.  Un modèle de disposition attachée offre à une application la possibilité de modifier la disposition des éléments à l’exécution, ou de partager plus facilement certains aspects de la disposition entre différentes parties de l’interface utilisateur (par exemple, les éléments des lignes d’une table qui apparaissent alignés dans une colonne).

Dans cette rubrique, nous aborderons ce qu’implique la création d’une disposition attachée (avec ou sans virtualisation), les concepts et les classes qu’il faut comprendre et les compromis à prendre en compte pour faire son choix.

| **Obtenir la bibliothèque d’interface utilisateur Windows** |
| - |
| Ce contrôle est inclus dans la bibliothèque d’interface utilisateur Windows, package NuGet qui contient les nouveaux contrôles et fonctionnalités d’interface utilisateur pour les applications Windows. Pour plus d’informations, notamment des instructions d’installation, consultez [Vue d’ensemble de la bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

> **API importantes** :

> * [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
> * [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater)
> * [Disposition](/uwp/api/microsoft.ui.xaml.controls.layout)
>     * [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
>     * [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)
> * [LayoutContext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext)
>     * [NonVirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext)
>     * [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext)
> * [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) (préversion)

## <a name="key-concepts"></a>Concepts clés

Pour effectuer une disposition, il faut répondre à deux questions pour chaque élément :

1. Quelle sera la ***taille*** de cet élément ?

2. Quelle sera la ***position*** de cet élément ?

Le système de disposition XAML, qui répond à ces questions, est brièvement abordé dans le cadre de la discussion sur les [Panneaux personnalisés](/windows/uwp/design/layout/custom-panels-overview).

### <a name="containers-and-context"></a>Conteneurs et contexte

D’un point de vue conceptuel, le [Panel](/uwp/api/windows.ui.xaml.controls.panel) XAML remplit deux rôles importants dans le framework :

1. Il peut contenir des éléments enfants et permet d’utiliser des branches dans l’arborescence d’éléments.
2. Il applique une stratégie de disposition spécifique à ces enfants.

C’est pour cette raison que le Panel en XAML a souvent été synonyme de disposition, alors que, techniquement parlant, il va au-delà.

[ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater) se comporte également comme Panel, à la différence près qu’il n’expose pas de propriété Children permettant d’ajouter ou de supprimer programmatiquement des enfants UIElement.  La durée de vie de ses enfants est en effet gérée automatiquement par le framework pour correspondre à une collection d’éléments de données.  Bien qu’il ne soit pas dérivé de Panel, il se comporte et est traité par le framework comme un Panel.

> [!NOTE]
> [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) est un conteneur, dérivé de Panel, qui délègue sa logique à l’objet [Layout](/uwp/api/microsoft.ui.xaml.controls.layoutpanel.layout) attaché.  LayoutPanel, actuellement en *préversion*, n’est disponible que dans les *préversions* du package WinUI.

#### <a name="containers"></a>Conteneurs

Conceptuellement, un [Panel](/uwp/api/windows.ui.xaml.controls.panel) est un conteneur d’éléments qui a également la capacité de restituer des pixels pour un [Background](/uwp/api/windows.ui.xaml.controls.panel.background).  Il offre un moyen d’encapsuler la logique de disposition courante dans un package facile à utiliser.

Le concept de **disposition attachée** permet de clarifier la distinction entre le rôle du conteneur et celui de la disposition.  Si le conteneur délègue sa logique de disposition à un autre objet, ce dernier est qualifié de disposition attachée, comme on le voit dans l’extrait de code ci-dessous. Les conteneurs qui héritent de [FrameworkElement](/uwp/api/windows.ui.xaml.frameworkelement), comme LayoutPanel, exposent automatiquement les propriétés communes qui fournissent une entrée au processus de disposition XAML (par exemple, Height et Width).

```xaml
<LayoutPanel>
    <LayoutPanel.Layout>
        <UniformGridLayout/>
    </LayoutPanel.Layout>
    <Button Content="1"/>
    <Button Content="2"/>
    <Button Content="3"/>
</LayoutPanel>
```

Pendant le processus de disposition, le conteneur s’appuie sur le *UniformGridLayout* attaché pour mesurer et organiser ses enfants.

#### <a name="per-container-state"></a>État par conteneur

Avec une disposition attachée, une même instance de l’objet de disposition peut être associée à *plusieurs* conteneurs, comme dans l’extrait de code ci-dessous. Par conséquent, elle ne doit pas dépendre du conteneur hôte ou y faire directement référence.  Par exemple :

```xaml
<!-- ... --->
<Page.Resources>
    <ExampleLayout x:Name="exampleLayout"/>
<Page.Resources>

<LayoutPanel x:Name="example1" Layout="{StaticResource exampleLayout}"/>
<LayoutPanel x:Name="example2" Layout="{StaticResource exampleLayout}"/>
<!-- ... --->
```

Dans ce cas, *ExampleLayout* doit soigneusement prendre en compte l’état qu’il utilise dans son calcul de disposition et l’endroit où cet état est stocké pour éviter d’affecter la disposition des éléments d’un panneau avec un autre.  Il serait analogue à un Panel personnalisé dont la logique MeasureOverride et ArrangeOverride dépend des valeurs de ses propriétés *statiques*.

#### <a name="layoutcontext"></a>LayoutContext

L’objectif de [LayoutContext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext) est de résoudre ces problématiques.  Il offre à la disposition attachée la possibilité d’interagir avec le conteneur hôte, par exemple de récupérer les éléments enfants, sans introduire de dépendance directe entre les deux. Le contexte permet également à la disposition de stocker tous les états dont elle a besoin et qui peuvent être liés aux éléments enfants du conteneur.

Les dispositions simples sans virtualisation n’ont généralement pas besoin de conserver d’états, ce qui en fait un faux problème. En revanche, une disposition plus complexe, comme un Grid, peut choisir de conserver l’état entre les appels Measure et Arrange pour éviter d’avoir à recalculer une valeur.

Les dispositions avec virtualisation doivent *souvent* conserver un certain état entre Measure et Arrange, ainsi qu’entre les passes de disposition itératives.

#### <a name="initializing-and-uninitializing-per-container-state"></a>Initialisation et désinitialisation de l’état par conteneur

Quand une disposition est attachée à un conteneur, sa méthode [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore) est appelée et offre la possibilité d’initialiser un objet permettant de stocker l’état.

De même, lors de la suppression de la disposition d’un conteneur, la méthode [UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore) est appelée.  Cela donne à la disposition la possibilité de nettoyer tous les états qu’elle avait associés à ce conteneur.

L’objet d’état de la disposition peut être stocké avec le conteneur et récupéré à partir du conteneur à l’aide de la propriété [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) sur le contexte.

### <a name="ui-virtualization"></a>Virtualisation de l’interface utilisateur

La virtualisation de l’interface utilisateur consiste à retarder la création d’un objet d’interface utilisateur jusqu'au _moment opportun_.  Il s’agit d’une optimisation des performances.  Dans les scénarios sans défilement, la notion de _moment opportun_ peut dépendre de n’importe quels éléments propres à l’application.  Les applications doivent alors envisager d’utiliser [x:Load](../../xaml-platform/x-load-attribute.md). Cela n’implique aucun traitement spécial dans la disposition.

Dans les scénarios avec défilement, par exemple une liste, le _moment opportun_ est souvent déterminé selon que l’élément sera ou non visible pour l’utilisateur, ce qui dépend fortement de son emplacement pendant le processus de disposition et implique des considérations spéciales.  Ce document se concentre sur ce scénario.

> [!NOTE]
> Bien que ce point ne soit pas abordé dans ce document, les fonctionnalités qui permettent la virtualisation de l’interface utilisateur dans les scénarios avec défilement peuvent être appliquées aux scénarios sans défilement.  Exemple : un contrôle ToolBar piloté par les données qui gère la durée de vie des commandes qu’il présente et répond aux modifications de l’espace disponible en recyclant/déplaçant des éléments entre une zone visible et un menu de débordement.

## <a name="getting-started"></a>Prise en main

Tout d’abord, déterminez si la disposition que vous allez créer doit prendre en charge la virtualisation de l’interface utilisateur.

**Quelques points à garder à l’esprit…**

1. Les dispositions sans virtualisation sont plus faciles à créer. Si le nombre d’éléments doit rester toujours faible, il est recommandé de créer une disposition sans virtualisation.
2. La plateforme fournit un jeu de dispositions attachées qui fonctionnent avec [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater#change-the-layout-of-items) et [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) pour couvrir les besoins courants.  Familiarisez-vous avec elles avant de décider de définir une disposition personnalisée.
3. Les dispositions avec virtualisation présentent toujours un coût, une complexité et une charge supplémentaires en matière de processeur et de mémoire par rapport à une disposition sans virtualisation.  En règle générale, si les enfants que devra gérer la disposition ont de bonnes chances de tenir dans une zone faisant trois fois la taille de la fenêtre d’affichage, une disposition avec virtualisation n’apportera vraisemblablement pas grand-chose de plus. Cette taille trois fois supérieure, traitée en détail plus loin dans ce document, est due à la nature asynchrone du défilement sur Windows et à son impact sur la virtualisation.

> [!TIP]
> À titre de référence, les paramètres par défaut de [ListView](/uwp/api/windows.ui.xaml.controls.listview) (et de [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)) impliquent que le recyclage ne commence pas tant que le nombre d’éléments n’est pas suffisant pour remplir trois fois la taille de la fenêtre d’affichage actuelle.

**Choix du type de base**

![Hiérarchie de la disposition attachée](images/xaml-attached-layout-hierarchy.png)

Le type [Layout](/uwp/api/microsoft.ui.xaml.controls.layout) de base présente deux types dérivés servant de point de départ à la création d’une disposition attachée :

1. [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
2. [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)

## <a name="non-virtualizing-layout"></a>Disposition sans virtualisation

L’approche permettant de créer une disposition sans virtualisation devrait sembler familière à tous ceux qui ont créé un [Panel personnalisé](/windows/uwp/design/layout/custom-panels-overview).  Les mêmes concepts s’appliquent.  La principale différence réside dans le fait qu’un [NonVirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext) est utilisé pour accéder à la collection [Children](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext.children) et que la disposition peut choisir de stocker l’état.

1. Dérivez du type de base [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout) (au lieu de Panel).
2. *(Facultatif)* Définissez des propriétés de dépendance qui, en cas de modification, invalideront la disposition.
3. _(**Nouveau**/Facultatif)_ Initialisez tous les objets d’état requis par la disposition dans le cadre de [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore). Remisez-les (stash) avec le conteneur hôte à l’aide du [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) fourni avec le contexte.
4. Remplacez [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.measureoverride) et appelez la méthode [Measure](/uwp/api/windows.ui.xaml.uielement.measure) sur tous les enfants.
5. Remplacez [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.arrangeoverride) et appelez la méthode [Arrange](/uwp/api/windows.ui.xaml.uielement.arrange) sur tous les enfants.
6. *(**Nouveau**/Facultatif)* Nettoyez tous les états enregistrés dans le cadre de [UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore).

### <a name="example-a-simple-stack-layout-varying-sized-items"></a>Exemple : Disposition de pile simple (éléments de taille variable)

![MyStackLayout](images/xaml-attached-layout-mystacklayout.png)

Voici une disposition de pile très simple, sans virtualisation, d’éléments de taille variable. Elle ne comprend pas de propriétés permettant d’ajuster son comportement. L’implémentation ci-dessous montre que la disposition s’appuie sur l’objet de contexte fourni par le conteneur pour :

1. Récupérer le nombre d’enfants.
2. Accéder à chaque élément enfant par son index.

```csharp
public class MyStackLayout : NonVirtualizingLayout
{
    protected override Size MeasureOverride(NonVirtualizingLayoutContext context, Size availableSize)
    {
        double extentHeight = 0.0;
        foreach (var element in context.Children)
        {
            element.Measure(availableSize);
            extentHeight += element.DesiredSize.Height;
        }

        return new Size(availableSize.Width, extentHeight);
    }

    protected override Size ArrangeOverride(NonVirtualizingLayoutContext context, Size finalSize)
    {
        double offset = 0.0;
        foreach (var element in context.Children)
        {
            element.Arrange(
                new Rect(0, offset, finalSize.Width, element.DesiredSize.Height));
            offset += element.DesiredSize.Height;
        }

        return finalSize;
    }
}
```

```xaml
 <LayoutPanel MaxWidth="196">
    <LayoutPanel.Layout>
        <local:MyStackLayout/>
    </LayoutPanel.Layout>

    <Button HorizontalAlignment="Stretch">1</Button>
    <Button HorizontalAlignment="Right">2</Button>
    <Button HorizontalAlignment="Center">3</Button>
    <Button>4</Button>

</LayoutPanel>
```

## <a name="virtualizing-layouts"></a>Dispositions avec virtualisation

Les étapes de haut niveau d’une disposition de virtualisation sont les mêmes que pour une disposition sans virtualisation.  La complexité réside principalement dans l’identification des éléments qui se situeront dans la fenêtre d’affichage et devront être réalisés.

1. Dérivez du type de base [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout).
2. (Facultatif) Définissez des propriétés de dépendance qui, en cas de modification, invalideront la disposition.
3. Initialisez tous les objets d’état requis par la disposition dans le cadre de [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore). Remisez-les (stash) avec le conteneur hôte à l’aide du [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) fourni avec le contexte.
4. Remplacez [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) et appelez la méthode [Measure](/uwp/api/windows.ui.xaml.uielement.measure) pour chacun des enfants qui devront être réalisés.
   1. La méthode [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) est utilisée pour récupérer un UIElement préparé par le framework (par exemple, les liaisons de données appliquées).
5. Remplacez [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride) et appelez la méthode [Arrange](/uwp/api/windows.ui.xaml.uielement.arrange) pour chacun des enfants réalisés.
6. (Facultatif) Nettoyez tous les états enregistrés dans le cadre de [UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore).

> [!TIP]
> La valeur retournée par [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout) est utilisée comme taille du contenu virtualisé.

Il existe deux approches générales à prendre en compte pour créer une disposition avec virtualisation.  Le choix entre l’une et l’autre dépend en grande partie de la méthode de détermination de la taille d’un élément.  S’il suffit de connaître l’index d’un élément dans le jeu de données ou si les données elles-mêmes déterminent sa taille définitive, la disposition est considérée comme **dépendante des données**.  Il s’agit du type le plus facile à créer.  À l’inverse, si la seule façon de déterminer la taille d’un élément consiste à créer et à mesurer l’interface utilisateur, on dira qu’il s’agit d'une disposition **dépendante du contenu**.  Ce type est plus complexe.

### <a name="the-layout-process"></a>Processus de disposition

Pour une disposition dépendante des données comme pour une disposition dépendante du contenu, il est important de comprendre le processus de disposition et l’impact du défilement asynchrone Windows.

Voici une présentation (ultra-)simplifiée des étapes suivies par le framework, du démarrage à l’affichage de l’interface utilisateur à l’écran :

1. Il analyse le balisage.

2. Il génère une arborescence d’éléments.

3. Il effectue une passe de disposition.

4. Il effectue une passe de rendu.

Avec la virtualisation de l’interface utilisateur, la création des éléments, normalement effectuée à l’étape 2, est retardée ou arrêtée tôt, dès qu’il a été déterminé que le contenu créé est suffisant pour remplir la fenêtre d’affichage. Un conteneur avec virtualisation (par exemple, ItemsRepeater) s’en remet à sa disposition attachée pour piloter ce processus. Il lui fournit un [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) qui expose les informations supplémentaires requises par une disposition de virtualisation.

**RealizationRect (p. ex., Viewport)**

Le défilement sur Windows se produit de façon asynchrone sur le thread d’interface utilisateur. Il n’est pas contrôlé par la disposition du framework.  L’interaction et le déplacement se produisent dans le compositeur du système. L’avantage de cette approche est que le défilement du contenu peut toujours être effectué sur 60 FPS.  La difficulté, toutefois, est que la « fenêtre d’affichage », telle qu’elle apparaît à la disposition, risque d’être légèrement obsolète par rapport à ce qui est réellement visible à l’écran. Un utilisateur qui ferait défiler le contenu rapidement pourrait dépasser la vitesse à laquelle le thread d’interface utilisateur génère le nouveau contenu et voir apparaître un écran noir. C’est pourquoi il est souvent nécessaire pour une disposition avec virtualisation de générer une mémoire tampon supplémentaire d’éléments préparés suffisants pour remplir une zone plus grande que la fenêtre d’affichage. Même en cas de charge plus lourde pendant le défilement, le contenu est présenté à l’utilisateur.

![Rectangle de réalisation](images/xaml-attached-layout-realizationrect.png)

La création d’éléments étant coûteuse, les conteneurs avec virtualisation (par exemple, [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater)) fournissent initialement à la disposition attachée un [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) correspondant à la fenêtre d’affichage. Pendant la durée d’inactivité, le conteneur peut augmenter la mémoire tampon de contenu préparé en effectuant des appels répétés à la disposition à l’aide d’un rectangle de réalisation de plus en plus grand. Ce comportement représente une optimisation des performances dont l’objectif est de trouver un juste équilibre entre vitesse de démarrage et qualité de l’expérience de défilement. La taille maximale de la mémoire tampon générée par ItemsRepeater est contrôlée par ses propriétés [VerticalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength) et [HorizontalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength).

**Réutilisation d’éléments (recyclage)**

La disposition est censée dimensionner et positionner les éléments de façon à remplir [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) chaque fois qu’elle est exécutée. Par défaut, [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout) recycle tous les éléments inutilisés à la fin de chaque passe de disposition.

Le [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) transmis à la disposition dans le cadre de [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) et de [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride) fournit les informations supplémentaires requises par une disposition avec virtualisation. Il offre notamment les possibilités suivantes :

1. Interroger le nombre d’éléments dans les données ([ItemCount](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.itemcount)).
2. Récupérer un élément spécifique à l’aide de la méthode [GetItemAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getitemat).
3. Récupérer un [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) représentant la fenêtre d’affichage et la mémoire tampon que la disposition doit remplir avec les éléments réalisés.
4. Demander le UIElement d’un élément spécifique avec la méthode [GetOrCreateElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat).

Un élément demandé pour un index donné sera marqué comme étant « en cours d’utilisation » pour cette passe de la disposition. S’il n’existe pas encore, il sera réalisé et préparé automatiquement pour être utilisé (par exemple, en grossissant l’arborescence de l’interface utilisateur définie dans un DataTemplate, en traitant toutes les liaisons de données, etc.).  Dans le cas contraire, il sera récupéré à partir d’un pool d’instances existantes.

À la fin de chaque passe de mesure, tous les éléments existants non marqués de la mention « en cours d’utilisation » sont automatiquement considérés comme étant réutilisables, sauf si l’option [SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions) a été utilisée lors de leur récupération par la méthode [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat). Le framework les déplace automatiquement vers un pool de recyclage et les rend disponibles. Ils peuvent par la suite être extraits pour être utilisés par un autre conteneur. Le framework tente d’éviter cette solution dans la mesure du possible, car le reparentage présente un coût.

Si la disposition avec virtualisation sait au début de chaque mesure quels éléments ne vont plus se trouver dans le rectangle de réalisation, elle peut optimiser sa réutilisation, plutôt que de compter sur le comportement par défaut du framework. Elle peut déplacer des éléments de manière préemptive dans le pool de recyclage à l’aide de la méthode [RecycleElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recycleelement).  Si cette méthode est appelée avant de demander de nouveaux éléments, ces éléments existants seront disponibles lorsque la disposition émettra par la suite une demande [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) sur un index non encore associé à un élément.

VirtualizingLayoutContext fournit deux propriétés supplémentaires conçues pour les auteurs qui créent une disposition dépendante du contenu. Elles sont présentées en détail plus loin.

1. [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex), qui fournit une _entrée_ facultative à la disposition.
2. [LayoutOrigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin), une _sortie_ facultative de la disposition.

## <a name="data-dependent-virtualizing-layouts"></a>Dispositions avec virtualisation dépendantes des données

Les dispositions avec virtualisation sont plus faciles si l’on connaît la taille de chaque élément sans avoir à mesurer le contenu à afficher.  Dans ce document, nous ferons référence à cette catégorie de dispositions avec virtualisation sous le simple nom de **dispositions des données**, car elles impliquent généralement une inspection des données.  En fonction des données, une application peut choisir une représentation visuelle d’une taille connue, peut-être parce que sa partie des données a été déterminée précédemment par la conception.

L’approche générale consiste pour la disposition à :

1. Calculer la taille et la position de chaque élément.
2. Dans le cadre de [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) :
   1. Utiliser [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) pour déterminer quels éléments doivent apparaître dans la fenêtre d’affichage.
   2. Récupérer le UIElement devant représenter l’élément avec la méthode [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat).
   3. [Mesurer](/uwp/api/windows.ui.xaml.uielement.measure) UIElement avec la taille précalculée.
3. Dans le cadre de [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride) : [organiser](/uwp/api/windows.ui.xaml.uielement.arrange) chaque UIElement réalisé avec la position précalculée.

> [!NOTE]
> L’approche de type disposition des données est souvent incompatible avec la _virtualisation des données_,  en particulier lorsque les seules données chargées en mémoire sont les données requises pour remplir la partie visible par l’utilisateur.  La virtualisation des données ne fait pas référence au chargement différé ou incrémentiel des données lorsqu’un utilisateur fait défiler l’endroit où elles restent résidentes,  mais au moment où les éléments sont libérés de la mémoire à mesure qu’ils défilent hors affichage.  Une disposition des données inspectant chacun de ses éléments de données empêcherait la virtualisation des données de fonctionner comme prévu.  Une disposition comme UniformGridLayout, qui suppose que tout a la même taille, fait exception.

> [!TIP]
> Si vous créez un contrôle personnalisé pour une bibliothèque de contrôles qui sera utilisée par d’autres personnes dans une grande variété de situations, la disposition des données n’est peut-être pas l’option à envisager.

### <a name="example-xbox-activity-feed-layout"></a>Exemple : Disposition du flux d’activités Xbox

L’interface utilisateur du flux d’activités Xbox utilise un modèle répétitif dans lequel chaque ligne comporte une vignette large suivie de deux vignettes étroites, qui sont inversées sur la ligne suivante. Dans cette disposition, la taille de chaque élément est une fonction de la position de l’élément dans le jeu de données et de la taille connue des vignettes (large ou étroite).

![Flux d'activités Xbox](images/xaml-attached-layout-activityfeedscreenshot.png)

Le code ci-dessous présente ce que pourrait être une interface utilisateur de virtualisation personnalisée pour le flux d’activité afin d’illustrer l’approche générale d’une **disposition des données**.

<table>
<td>
    <p>Si vous disposez de l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour ouvrir l’application et voir <a href="xamlcontrolsgallery:/item/ItemsRepeater">ItemsRepeater</a> en action avec cet exemple de disposition.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

#### <a name="implementation"></a>Implémentation

```csharp
/// <summary>
///  This is a custom layout that displays elements in two different sizes
///  wide (w) and narrow (n). There are two types of rows 
///  odd rows - narrow narrow wide
///  even rows - wide narrow narrow
///  This pattern repeats.
/// </summary>

public class ActivityFeedLayout : VirtualizingLayout // STEP #1 Inherit from base attached layout
{
    // STEP #2 - Parameterize the layout
    #region Layout parameters

    // We'll cache copies of the dependency properties to avoid calling GetValue during layout since that
    // can be quite expensive due to the number of times we'd end up calling these.
    private double _rowSpacing;
    private double _colSpacing;
    private Size _minItemSize = Size.Empty;

    /// <summary>
    /// Gets or sets the size of the whitespace gutter to include between rows
    /// </summary>
    public double RowSpacing
    {
        get { return _rowSpacing; }
        set { SetValue(RowSpacingProperty, value); }
    }

    /// <summary>
    /// Gets or sets the size of the whitespace gutter to include between items on the same row
    /// </summary>
    public double ColumnSpacing
    {
        get { return _colSpacing; }
        set { SetValue(ColumnSpacingProperty, value); }
    }

    public Size MinItemSize
    {
        get { return _minItemSize; }
        set { SetValue(MinItemSizeProperty, value); }
    }

    public static readonly DependencyProperty RowSpacingProperty =
        DependencyProperty.Register(
            nameof(RowSpacing),
            typeof(double),
            typeof(ActivityFeedLayout),
            new PropertyMetadata(0, OnPropertyChanged));

    public static readonly DependencyProperty ColumnSpacingProperty =
        DependencyProperty.Register(
            nameof(ColumnSpacing),
            typeof(double),
            typeof(ActivityFeedLayout),
            new PropertyMetadata(0, OnPropertyChanged));

    public static readonly DependencyProperty MinItemSizeProperty =
        DependencyProperty.Register(
            nameof(MinItemSize),
            typeof(Size),
            typeof(ActivityFeedLayout),
            new PropertyMetadata(Size.Empty, OnPropertyChanged));

    private static void OnPropertyChanged(DependencyObject obj, DependencyPropertyChangedEventArgs args)
    {
        var layout = obj as ActivityFeedLayout;
        if (args.Property == RowSpacingProperty)
        {
            layout._rowSpacing = (double)args.NewValue;
        }
        else if (args.Property == ColumnSpacingProperty)
        {
            layout._colSpacing = (double)args.NewValue;
        }
        else if (args.Property == MinItemSizeProperty)
        {
            layout._minItemSize = (Size)args.NewValue;
        }
        else
        {
            throw new InvalidOperationException("Don't know what you are talking about!");
        }

        layout.InvalidateMeasure();
    }

    #endregion

    #region Setup / teardown // STEP #3: Initialize state

    protected override void InitializeForContextCore(VirtualizingLayoutContext context)
    {
        base.InitializeForContextCore(context);

        var state = context.LayoutState as ActivityFeedLayoutState;
        if (state == null)
        {
            // Store any state we might need since (in theory) the layout could be in use by multiple
            // elements simultaneously
            // In reality for the Xbox Activity Feed there's probably only a single instance.
            context.LayoutState = new ActivityFeedLayoutState();
        }
    }

    protected override void UninitializeForContextCore(VirtualizingLayoutContext context)
    {
        base.UninitializeForContextCore(context);

        // clear any state
        context.LayoutState = null;
    }

    #endregion

    #region Layout // STEP #4,5 - Measure and Arrange

    protected override Size MeasureOverride(VirtualizingLayoutContext context, Size availableSize)
    {
        if (this.MinItemSize == Size.Empty)
        {
            var firstElement = context.GetOrCreateElementAt(0);
            firstElement.Measure(new Size(double.PositiveInfinity, double.PositiveInfinity));

            // setting the member value directly to skip invalidating layout
            this._minItemSize = firstElement.DesiredSize;
        }

        // Determine which rows need to be realized.  We know every row will have the same height and
        // only contain 3 items.  Use that to determine the index for the first and last item that
        // will be within that realization rect.
        var firstRowIndex = Math.Max(
            (int)(context.RealizationRect.Y / (this.MinItemSize.Height + this.RowSpacing)) - 1,
            0);
        var lastRowIndex = Math.Min(
            (int)(context.RealizationRect.Bottom / (this.MinItemSize.Height + this.RowSpacing)) + 1,
            (int)(context.ItemCount / 3));

        // Determine which items will appear on those rows and what the rect will be for each item
        var state = context.LayoutState as ActivityFeedLayoutState;
        state.LayoutRects.Clear();

        // Save the index of the first realized item.  We'll use it as a starting point during arrange.
        state.FirstRealizedIndex = firstRowIndex * 3;

        // ideal item width that will expand/shrink to fill available space
        double desiredItemWidth = Math.Max(this.MinItemSize.Width, (availableSize.Width - this.ColumnSpacing * 3) / 4);

        // Foreach item between the first and last index,
        //     Call GetElementOrCreateElementAt which causes an element to either be realized or retrieved
        //       from a recycle pool
        //     Measure the element using an appropriate size
        //
        // Any element that was previously realized which we don't retrieve in this pass (via a call to
        // GetElementOrCreateAt) will be automatically cleared and set aside for later re-use.
        // Note: While this work fine, it does mean that more elements than are required may be
        // created because it isn't until after our MeasureOverride completes that the unused elements
        // will be recycled and available to use.  We could avoid this by choosing to track the first/last
        // index from the previous layout pass.  The diff between the previous range and current range
        // would represent the elements that we can pre-emptively make available for re-use by calling
        // context.RecycleElement(element).
        for (int rowIndex = firstRowIndex; rowIndex < lastRowIndex; rowIndex++)
        {
            int firstItemIndex = rowIndex * 3;
            var boundsForCurrentRow = CalculateLayoutBoundsForRow(rowIndex, desiredItemWidth);

            for (int columnIndex = 0; columnIndex < 3; columnIndex++)
            {
                var index = firstItemIndex + columnIndex;
                var rect = boundsForCurrentRow[index % 3];
                var container = context.GetOrCreateElementAt(index);

                container.Measure(
                    new Size(boundsForCurrentRow[columnIndex].Width, boundsForCurrentRow[columnIndex].Height));

                state.LayoutRects.Add(boundsForCurrentRow[columnIndex]);
            }
        }

        // Calculate and return the size of all the content (realized or not) by figuring out
        // what the bottom/right position of the last item would be.
        var extentHeight = ((int)(context.ItemCount / 3) - 1) * (this.MinItemSize.Height + this.RowSpacing) + this.MinItemSize.Height;

        // Report this as the desired size for the layout
        return new Size(desiredItemWidth * 4 + this.ColumnSpacing * 2, extentHeight);
    }

    protected override Size ArrangeOverride(VirtualizingLayoutContext context, Size finalSize)
    {
        // walk through the cache of containers and arrange
        var state = context.LayoutState as ActivityFeedLayoutState;
        var virtualContext = context as VirtualizingLayoutContext;
        int currentIndex = state.FirstRealizedIndex;

        foreach (var arrangeRect in state.LayoutRects)
        {
            var container = virtualContext.GetOrCreateElementAt(currentIndex);
            container.Arrange(arrangeRect);
            currentIndex++;
        }

        return finalSize;
    }

    #endregion
    #region Helper methods

    private Rect[] CalculateLayoutBoundsForRow(int rowIndex, double desiredItemWidth)
    {
        var boundsForRow = new Rect[3];

        var yoffset = rowIndex * (this.MinItemSize.Height + this.RowSpacing);
        boundsForRow[0].Y = boundsForRow[1].Y = boundsForRow[2].Y = yoffset;
        boundsForRow[0].Height = boundsForRow[1].Height = boundsForRow[2].Height = this.MinItemSize.Height;

        if (rowIndex % 2 == 0)
        {
            // Left tile (narrow)
            boundsForRow[0].X = 0;
            boundsForRow[0].Width = desiredItemWidth;
            // Middle tile (narrow)
            boundsForRow[1].X = boundsForRow[0].Right + this.ColumnSpacing;
            boundsForRow[1].Width = desiredItemWidth;
            // Right tile (wide)
            boundsForRow[2].X = boundsForRow[1].Right + this.ColumnSpacing;
            boundsForRow[2].Width = desiredItemWidth * 2 + this.ColumnSpacing;
        }
        else
        {
            // Left tile (wide)
            boundsForRow[0].X = 0;
            boundsForRow[0].Width = (desiredItemWidth * 2 + this.ColumnSpacing);
            // Middle tile (narrow)
            boundsForRow[1].X = boundsForRow[0].Right + this.ColumnSpacing;
            boundsForRow[1].Width = desiredItemWidth;
            // Right tile (narrow)
            boundsForRow[2].X = boundsForRow[1].Right + this.ColumnSpacing;
            boundsForRow[2].Width = desiredItemWidth;
        }

        return boundsForRow;
    }

    #endregion
}

internal class ActivityFeedLayoutState
{
    public int FirstRealizedIndex { get; set; }

    /// <summary>
    /// List of layout bounds for items starting with the
    /// FirstRealizedIndex.
    /// </summary>
    public List<Rect> LayoutRects
    {
        get
        {
            if (_layoutRects == null)
            {
                _layoutRects = new List<Rect>();
            }

            return _layoutRects;
        }
    }

    private List<Rect> _layoutRects;
}
```

### <a name="optional-managing-the-item-to-uielement-mapping"></a>(Facultatif) Gestion du mappage entre Item et UIElement

Par défaut, [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) conserve un mappage entre les éléments réalisés et l’index dans la source de données qu’ils représentent.  Une disposition peut choisir de gérer ce mappage elle-même en demandant toujours l’option [SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions) lorsqu’elle récupère un élément avec la méthode [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat), ce qui empêche le comportement de recyclage automatique par défaut.  Elle peut par exemple appliquer cette solution si elle n’est utilisée que lorsque le défilement est limité à une direction et que les éléments qu’elle considère sont toujours contigus (c’est-à-dire qu’il suffit de connaître l’index du premier et du dernier élément pour connaître tous les éléments à réaliser).

#### <a name="example-xbox-activity-feed-measure"></a>Exemple : Mesure du flux d’activités Xbox

L’extrait de code ci-dessous montre la logique supplémentaire qui peut être ajoutée à MeasureOverride dans l’exemple précédent pour gérer le mappage.

```csharp
    protected override Size MeasureOverride(VirtualizingLayoutContext context, Size availableSize)
    {
        //...

        // Determine which items will appear on those rows and what the rect will be for each item
        var state = context.LayoutState as ActivityFeedLayoutState;
        state.LayoutRects.Clear();

         // Recycle previously realized elements that we know we won't need so that they can be used to
        // fill in gaps without requiring us to realize additional elements.
        var newFirstRealizedIndex = firstRowIndex * 3;
        var newLastRealizedIndex = lastRowIndex * 3 + 3;
        for (int i = state.FirstRealizedIndex; i < newFirstRealizedIndex; i++)
        {
            context.RecycleElement(state.IndexToElementMap.Get(i));
            state.IndexToElementMap.Clear(i);
        }

        for (int i = state.LastRealizedIndex; i < newLastRealizedIndex; i++)
        {
            context.RecycleElement(context.IndexElementMap.Get(i));
            state.IndexToElementMap.Clear(i);
        }

        // ...

        // Foreach item between the first and last index,
        //     Call GetElementOrCreateElementAt which causes an element to either be realized or retrieved
        //       from a recycle pool
        //     Measure the element using an appropriate size
        //
        for (int rowIndex = firstRowIndex; rowIndex < lastRowIndex; rowIndex++)
        {
            int firstItemIndex = rowIndex * 3;
            var boundsForCurrentRow = CalculateLayoutBoundsForRow(rowIndex, desiredItemWidth);

            for (int columnIndex = 0; columnIndex < 3; columnIndex++)
            {
                var index = firstItemIndex + columnIndex;
                var rect = boundsForCurrentRow[index % 3];
                UIElement container = null;
                if (state.IndexToElementMap.Contains(index))
                {
                    container = state.IndexToElementMap.Get(index);
                }
                else
                {
                    container = context = context.GetOrCreateElementAt(index, ElementRealizationOptions.ForceCreate | ElementRealizationOptions.SuppressAutoRecycle);
                    state.IndexToElementMap.Add(index, container);
                }

                container.Measure(
                    new Size(boundsForCurrentRow[columnIndex].Width, boundsForCurrentRow[columnIndex].Height));

                state.LayoutRects.Add(boundsForCurrentRow[columnIndex]);
            }
        }

        // ...
   }

internal class ActivityFeedLayoutState
{
    // ...
    Dictionary<int, UIElement> IndexToElementMap { get; set; }
    // ...
}
```

## <a name="content-dependent-virtualizing-layouts"></a>Dispositions avec virtualisation dépendantes du contenu

Si vous devez d’abord mesurer le contenu de l’interface utilisateur pour un élément afin de déterminer sa taille exacte, il s’agit d’une **disposition dépendante du contenu**.  Vous pouvez également la considérer comme une disposition dans laquelle chaque élément doit se dimensionner lui-même sans qu’elle lui indique sa taille. Les dispositions avec virtualisation appartenant à cette catégorie sont plus complexes.

> [!NOTE]
> Les dispositions dépendantes du contenu ne doivent pas (de préférence) arrêter la virtualisation des données.

### <a name="estimations"></a>Estimations

Les dispositions dépendantes du contenu s’appuient sur une estimation pour deviner la taille et la position du contenu réalisé. Chaque fois que ces estimations changent, le contenu réalisé se déplace dans la zone réorganisable, ce qui, sans atténuation, peut aboutir à une expérience utilisateur très contrariante et perturbante. Les problèmes potentiels et les atténuations sont abordés ici.

> [!NOTE]
> Les dispositions des données qui considèrent chaque élément et connaissent la taille exacte de tous les éléments, réalisées ou non, ainsi que leur position peuvent éviter totalement ces problèmes.

**Ancrage du défilement**

XAML fournit un mécanisme permettant d’atténuer les décalages soudains de la fenêtre d’affichage en permettant la prise en charge de [l’ancrage du défilement](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider) par les contrôles de défilement, grâce à l’implémentation de l’interface [IScrollAnchorPovider](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider). Lorsque l’utilisateur manipule le contenu, le contrôle de défilement sélectionne continuellement un élément parmi l’ensemble des candidats pour lesquels le suivi a été accepté. Si la position de l’élément d’ancrage change pendant la disposition, le contrôle de défilement décale automatiquement sa fenêtre d’affichage de façon à la maintenir.

La valeur du [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) fourni à la disposition peut refléter l’élément d’ancrage sélectionné choisi par le contrôle de défilement. Dans le cas contraire, si un développeur demande explicitement qu’un élément soit réalisé pour un index avec la méthode [GetOrCreateElement](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.getorcreateelement) sur [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater), cet index est donné en tant que [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) sur la passe suivante de la disposition. Cela permet de préparer la disposition pour le scénario probable selon lequel le développeur réalise un élément et demande ensuite qu’il soit affiché par le biais de la méthode [StartBringIntoView](/uwp/api/windows.ui.xaml.uielement.startbringintoview).

[RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) est l’index de l’élément dans la source de données qu’une disposition dépendante du contenu doit positionner en premier lors de l’estimation de la position de ses éléments. Il doit servir de point de départ au positionnement d’autres éléments réalisés.

**Impact sur les barres de défilement**

Même avec l’ancrage du défilement, si les estimations de la disposition varient beaucoup (peut-être en raison de fluctuations significatives de la taille du contenu), la position du curseur de la barre de défilement peut paraître sauter.  Cela peut être perturbant pour l’utilisateur si le curseur ne semble pas suivre la position du pointeur de la souris lorsqu’il le fait glisser.

Plus la disposition est précise dans ses estimations, moins l’utilisateur risque de voir le curseur de la barre de défilement sauter.

### <a name="layout-corrections"></a>Corrections de la disposition

Une disposition dépendante du contenu doit être préparée à rationaliser son estimation avec la réalité.  Par exemple, lorsque l’utilisateur fait défiler le contenu vers le haut et que la disposition réalise le tout premier élément, elle risque de constater que la position prévue de l’élément, par rapport à celui à partir duquel elle a commencé, se trouve à un emplacement autre que l’origine de (x:0, y:0). Dans ce cas, elle peut utiliser la propriété [LayoutOrigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin) pour définir la position qu’elle a calculée comme sa nouvelle origine.  Le résultat final est semblable à l’ancrage du défilement, dans lequel la fenêtre d’affichage du contrôle de défilement est automatiquement ajustée pour tenir compte de la position du contenu indiquée par la disposition.

![Correction de LayoutOrigin](images/xaml-attached-layout-origincorrection.png)

### <a name="disconnected-viewports"></a>Fenêtres d’affichage déconnectées

La taille retournée par la méthode [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) de la disposition représente la meilleure estimation de la taille du contenu, qui peut changer avec chaque disposition successive.  Lorsque l’utilisateur fait défiler le contenu, la disposition est continuellement réévaluée avec un [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) mis à jour.

Si l’utilisateur fait glisser le curseur très rapidement, il est possible que la fenêtre d’affichage, du point de vue de la disposition, semble faire de grands sauts lorsque la position précédente ne chevauche pas la nouvelle position actuelle.  Cette particularité est due à la nature asynchrone du défilement. Il est également possible pour une application qui utilise la disposition de demander qu’un composant soit affiché pour un élément non réalisé dont la position estimée se trouve en dehors de la plage actuelle suivie par la disposition.

Lorsque la disposition découvre que son estimation est incorrecte ou constate un décalage inattendu de la fenêtre d’affichage, elle doit réorienter sa position de départ.  Les dispositions avec virtualisation fournies dans les contrôles XAML sont développées sous forme de dispositions dépendantes du contenu, car elles placent moins de restrictions sur la nature du contenu qui s’affichera.


### <a name="example-simple-virtualizing-stack-layout-for-variable-sized-items"></a>Exemple : Disposition de pile simple avec virtualisation pour des éléments de taille variable

L’exemple ci-dessous montre une disposition de pile simple pour des éléments de taille variable qui :

* prend en charge la virtualisation de l’interface utilisateur ;
* utilise des estimations pour deviner la taille des éléments non réalisés ;
* est consciente des décalages discontinus potentiels de la fenêtre d’affichage ;
* applique des corrections de la disposition pour tenir compte de ces décalages.

**Utilisation : Balisage**

```xaml
<ScrollViewer>

  <ItemsRepeater x:Name="repeater" >
    <ItemsRepeater.Layout>

      <local:VirtualizingStackLayout />

    </ItemsRepeater.Layout>
    <ItemsRepeater.ItemTemplate>
      <DataTemplate x:Key="item">
        <UserControl IsTabStop="True" UseSystemFocusVisuals="True" Margin="5">
          <StackPanel BorderThickness="1" Background="LightGray" Margin="5">
            <Image x:Name="recipeImage" Source="{Binding ImageUri}"  Width="100" Height="100"/>
              <TextBlock x:Name="recipeDescription"
                         Text="{Binding Description}"
                         TextWrapping="Wrap"
                         Margin="10" />
          </StackPanel>
        </UserControl>
      </DataTemplate>
    </ItemsRepeater.ItemTemplate>
  </ItemsRepeater>

</ScrollViewer>
```

**Code-behind : Main.cs**

```csharp
string _lorem = @"Lorem ipsum dolor sit amet, consectetur adipiscing elit. Etiam laoreet erat vel massa rutrum, eget mollis massa vulputate. Vivamus semper augue leo, eget faucibus nulla mattis nec. Donec scelerisque lacus at dui ultricies, eget auctor ipsum placerat. Integer aliquet libero sed nisi eleifend, nec rutrum arcu lacinia. Sed a sem et ante gravida congue sit amet ut augue. Donec quis pellentesque urna, non finibus metus. Proin sed ornare tellus. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Etiam laoreet erat vel massa rutrum, eget mollis massa vulputate. Vivamus semper augue leo, eget faucibus nulla mattis nec. Donec scelerisque lacus at dui ultricies, eget auctor ipsum placerat. Integer aliquet libero sed nisi eleifend, nec rutrum arcu lacinia. Sed a sem et ante gravida congue sit amet ut augue. Donec quis pellentesque urna, non finibus metus. Proin sed ornare tellus.";

var rnd = new Random();
var data = new ObservableCollection<Recipe>(Enumerable.Range(0, 300).Select(k =>
               new Recipe
               {
                   ImageUri = new Uri(string.Format("ms-appx:///Images/recipe{0}.png", k % 8 + 1)),
                   Description = k + " - " + _lorem.Substring(0, rnd.Next(50, 350))
               }));

repeater.ItemsSource = data;
```

**Code : VirtualizingStackLayout.cs**

```csharp
// This is a sample layout that stacks elements one after
// the other where each item can be of variable height. This is
// also a virtualizing layout - we measure and arrange only elements
// that are in the viewport. Not measuring/arranging all elements means
// that we do not have the complete picture and need to estimate sometimes.
// For example the size of the layout (extent) is an estimation based on the
// average heights we have seen so far. Also, if you drag the mouse thumb
// and yank it quickly, then we estimate what goes in the new viewport.

// The layout caches the bounds of everything that are in the current viewport.
// During measure, we might get a suggested anchor (or start index), we use that
// index to start and layout the rest of the items in the viewport relative to that
// index. Note that since we are estimating, we can end up with negative origin when
// the viewport is somewhere in the middle of the extent. This is achieved by setting the
// LayoutOrigin property on the context. Once this is set, future viewport will account
// for the origin.
public class VirtualizingStackLayout : VirtualizingLayout
{
    // Estimation state
    List<double> m_estimationBuffer = Enumerable.Repeat(0d, 100).ToList();
    int m_numItemsUsedForEstimation = 0;
    double m_totalHeightForEstimation = 0;

    // State to keep track of realized bounds
    int m_firstRealizedDataIndex = 0;
    List<Rect> m_realizedElementBounds = new List<Rect>();

    Rect m_lastExtent = new Rect();

    protected override Size MeasureOverride(VirtualizingLayoutContext context, Size availableSize)
    {
        var viewport = context.RealizationRect;
        DebugTrace("MeasureOverride: Viewport " + viewport);

        // Remove bounds for elements that are now outside the viewport.
        // Proactive recycling elements means we can reuse it during this measure pass again.
        RemoveCachedBoundsOutsideViewport(viewport);

        // Find the index of the element to start laying out from - the anchor
        int startIndex = GetStartIndex(context, availableSize);

        // Measure and layout elements starting from the start index, forward and backward.
        Generate(context, availableSize, startIndex, forward:true);
        Generate(context, availableSize, startIndex, forward:false);

        // Estimate the extent size. Note that this can have a non 0 origin.
        m_lastExtent = EstimateExtent(context, availableSize);
        context.LayoutOrigin = new Point(m_lastExtent.X, m_lastExtent.Y);
        return new Size(m_lastExtent.Width, m_lastExtent.Height);
    }

    protected override Size ArrangeOverride(VirtualizingLayoutContext context, Size finalSize)
    {
        DebugTrace("ArrangeOverride: Viewport" + context.RealizationRect);
        for (int realizationIndex = 0; realizationIndex < m_realizedElementBounds.Count; realizationIndex++)
        {
            int currentDataIndex = m_firstRealizedDataIndex + realizationIndex;
            DebugTrace("Arranging " + currentDataIndex);

            // Arrange the child. If any alignment needs to be done, it
            // can be done here.
            var child = context.GetOrCreateElementAt(currentDataIndex);
            var arrangeBounds = m_realizedElementBounds[realizationIndex];
            arrangeBounds.X -= m_lastExtent.X;
            arrangeBounds.Y -= m_lastExtent.Y;
            child.Arrange(arrangeBounds);
        }

        return finalSize;
    }

    // The data collection has changed, since we are maintaining the bounds of elements
    // in the viewport, we will update the list to account for the collection change.
    protected override void OnItemsChangedCore(VirtualizingLayoutContext context, object source, NotifyCollectionChangedEventArgs args)
    {
        InvalidateMeasure();
        if (m_realizedElementBounds.Count > 0)
        {
            switch (args.Action)
            {
                case NotifyCollectionChangedAction.Add:
                    OnItemsAdded(args.NewStartingIndex, args.NewItems.Count);
                    break;
                case NotifyCollectionChangedAction.Replace:
                    OnItemsRemoved(args.OldStartingIndex, args.OldItems.Count);
                    OnItemsAdded(args.NewStartingIndex, args.NewItems.Count);
                    break;
                case NotifyCollectionChangedAction.Remove:
                    OnItemsRemoved(args.OldStartingIndex, args.OldItems.Count);
                    break;
                case NotifyCollectionChangedAction.Reset:
                    m_realizedElementBounds.Clear();
                    m_firstRealizedDataIndex = 0;
                    break;
                default:
                    throw new NotImplementedException();
            }
        }
    }

    // Figure out which index to use as the anchor and start laying out around it.
    private int GetStartIndex(VirtualizingLayoutContext context, Size availableSize)
    {
        int startDataIndex = -1;
        var recommendedAnchorIndex = context.RecommendedAnchorIndex;
        bool isSuggestedAnchorValid = recommendedAnchorIndex != -1;

        if (isSuggestedAnchorValid)
        {
            if (IsRealized(recommendedAnchorIndex))
            {
                startDataIndex = recommendedAnchorIndex;
            }
            else
            {
                ClearRealizedRange();
                startDataIndex = recommendedAnchorIndex;
            }
        }
        else
        {
            // Find the first realized element that is visible in the viewport.
            startDataIndex = GetFirstRealizedDataIndexInViewport(context.RealizationRect);
            if (startDataIndex < 0)
            {
                startDataIndex = EstimateIndexForViewport(context.RealizationRect, context.ItemCount);
                ClearRealizedRange();
            }
        }

        // We have an anchorIndex, realize and measure it and
        // figure out its bounds.
        if (startDataIndex != -1 & context.ItemCount > 0)
        {
            if (m_realizedElementBounds.Count == 0)
            {
                m_firstRealizedDataIndex = startDataIndex;
            }

            var newAnchor = EnsureRealized(startDataIndex);
            DebugTrace("Measuring start index " + startDataIndex);
            var desiredSize = MeasureElement(context, startDataIndex, availableSize);

            var bounds = new Rect(
                0,
                newAnchor ?
                    (m_totalHeightForEstimation / m_numItemsUsedForEstimation) * startDataIndex : GetCachedBoundsForDataIndex(startDataIndex).Y,
                availableSize.Width,
                desiredSize.Height);
            SetCachedBoundsForDataIndex(startDataIndex, bounds);
        }

        return startDataIndex;
    }


    private void Generate(VirtualizingLayoutContext context, Size availableSize, int anchorDataIndex, bool forward)
    {
        // Generate forward or backward from anchorIndex until we hit the end of the viewport
        int step = forward ? 1 : -1;
        int previousDataIndex = anchorDataIndex;
        int currentDataIndex = previousDataIndex + step;
        var viewport = context.RealizationRect;
        while (IsDataIndexValid(currentDataIndex, context.ItemCount) &&
            ShouldContinueFillingUpSpace(previousDataIndex, forward, viewport))
        {
            EnsureRealized(currentDataIndex);
            DebugTrace("Measuring " + currentDataIndex);
            var desiredSize = MeasureElement(context, currentDataIndex, availableSize);
            var previousBounds = GetCachedBoundsForDataIndex(previousDataIndex);
            Rect currentBounds = new Rect(0,
                                          forward ? previousBounds.Y + previousBounds.Height : previousBounds.Y - desiredSize.Height,
                                          availableSize.Width,
                                          desiredSize.Height);
            SetCachedBoundsForDataIndex(currentDataIndex, currentBounds);
            previousDataIndex = currentDataIndex;
            currentDataIndex += step;
        }
    }

    // Remove bounds that are outside the viewport, leaving one extra since our
    // generate stops after generating one extra to know that we are outside the
    // viewport.
    private void RemoveCachedBoundsOutsideViewport(Rect viewport)
    {
        int firstRealizedIndexInViewport = 0;
        while (firstRealizedIndexInViewport < m_realizedElementBounds.Count &&
               !Intersects(m_realizedElementBounds[firstRealizedIndexInViewport], viewport))
        {
            firstRealizedIndexInViewport++;
        }

        int lastRealizedIndexInViewport = m_realizedElementBounds.Count - 1;
        while (lastRealizedIndexInViewport >= 0 &&
            !Intersects(m_realizedElementBounds[lastRealizedIndexInViewport], viewport))
        {
            lastRealizedIndexInViewport--;
        }

        if (firstRealizedIndexInViewport > 0)
        {
            m_firstRealizedDataIndex += firstRealizedIndexInViewport;
            m_realizedElementBounds.RemoveRange(0, firstRealizedIndexInViewport);
        }

        if (lastRealizedIndexInViewport >= 0 && lastRealizedIndexInViewport < m_realizedElementBounds.Count - 2)
        {
            m_realizedElementBounds.RemoveRange(lastRealizedIndexInViewport + 2, m_realizedElementBounds.Count - lastRealizedIndexInViewport - 3);
        }
    }

    private bool Intersects(Rect bounds, Rect viewport)
    {
        return !(bounds.Bottom < viewport.Top ||
            bounds.Top > viewport.Bottom);
    }

    private bool ShouldContinueFillingUpSpace(int dataIndex, bool forward, Rect viewport)
    {
        var bounds = GetCachedBoundsForDataIndex(dataIndex);
        return forward ?
            bounds.Y < viewport.Bottom :
            bounds.Y > viewport.Top;
    }

    private bool IsDataIndexValid(int currentDataIndex, int itemCount)
    {
        return currentDataIndex >= 0 && currentDataIndex < itemCount;
    }

    private int EstimateIndexForViewport(Rect viewport, int dataCount)
    {
        double averageHeight = m_totalHeightForEstimation / m_numItemsUsedForEstimation;
        int estimatedIndex = (int)(viewport.Top / averageHeight);
        // clamp to an index within the collection
        estimatedIndex = Math.Max(0, Math.Min(estimatedIndex, dataCount));
        return estimatedIndex;
    }

    private int GetFirstRealizedDataIndexInViewport(Rect viewport)
    {
        int index = -1;
        if (m_realizedElementBounds.Count > 0)
        {
            for (int i = 0; i < m_realizedElementBounds.Count; i++)
            {
                if (m_realizedElementBounds[i].Y < viewport.Bottom &&
                   m_realizedElementBounds[i].Bottom > viewport.Top)
                {
                    index = m_firstRealizedDataIndex + i;
                    break;
                }
            }
        }

        return index;
    }

    private Size MeasureElement(VirtualizingLayoutContext context, int index, Size availableSize)
    {
        var child = context.GetOrCreateElementAt(index);
        child.Measure(availableSize);

        int estimationBufferIndex = index % m_estimationBuffer.Count;
        bool alreadyMeasured = m_estimationBuffer[estimationBufferIndex] != 0;
        if (!alreadyMeasured)
        {
            m_numItemsUsedForEstimation++;
        }

        m_totalHeightForEstimation -= m_estimationBuffer[estimationBufferIndex];
        m_totalHeightForEstimation += child.DesiredSize.Height;
        m_estimationBuffer[estimationBufferIndex] = child.DesiredSize.Height;

        return child.DesiredSize;
    }

    private bool EnsureRealized(int dataIndex)
    {
        if (!IsRealized(dataIndex))
        {
            int realizationIndex = RealizationIndex(dataIndex);
            Debug.Assert(dataIndex == m_firstRealizedDataIndex - 1 ||
                dataIndex == m_firstRealizedDataIndex + m_realizedElementBounds.Count ||
                m_realizedElementBounds.Count == 0);

            if (realizationIndex == -1)
            {
                m_realizedElementBounds.Insert(0, new Rect());
            }
            else
            {
                m_realizedElementBounds.Add(new Rect());
            }

            if (m_firstRealizedDataIndex > dataIndex)
            {
                m_firstRealizedDataIndex = dataIndex;
            }

            return true;
        }

        return false;
    }

    // Figure out the extent of the layout by getting the number of items remaining
    // above and below the realized elements and getting an estimation based on
    // average item heights seen so far.
    private Rect EstimateExtent(VirtualizingLayoutContext context, Size availableSize)
    {
        double averageHeight = m_totalHeightForEstimation / m_numItemsUsedForEstimation;

        Rect extent = new Rect(0, 0, availableSize.Width, context.ItemCount * averageHeight);

        if (context.ItemCount > 0 && m_realizedElementBounds.Count > 0)
        {
            extent.Y = m_firstRealizedDataIndex == 0 ?
                            m_realizedElementBounds[0].Y :
                            m_realizedElementBounds[0].Y - (m_firstRealizedDataIndex - 1) * averageHeight;

            int lastRealizedIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count;
            if (lastRealizedIndex == context.ItemCount - 1)
            {
                var lastBounds = m_realizedElementBounds[m_realizedElementBounds.Count - 1];
                extent.Y = lastBounds.Bottom;
            }
            else
            {
                var lastBounds = m_realizedElementBounds[m_realizedElementBounds.Count - 1];
                int lastRealizedDataIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count;
                int numItemsAfterLastRealizedIndex = context.ItemCount - lastRealizedDataIndex;
                extent.Height = lastBounds.Bottom + numItemsAfterLastRealizedIndex * averageHeight - extent.Y;
            }
        }

        DebugTrace("Extent " + extent + " with average height " + averageHeight);
        return extent;
    }

    private bool IsRealized(int dataIndex)
    {
        int realizationIndex = dataIndex - m_firstRealizedDataIndex;
        return realizationIndex >= 0 && realizationIndex < m_realizedElementBounds.Count;
    }

    // Index in the m_realizedElementBounds collection
    private int RealizationIndex(int dataIndex)
    {
        return dataIndex - m_firstRealizedDataIndex;
    }

    private void OnItemsAdded(int index, int count)
    {
        // Using the old indexes here (before it was updated by the collection change)
        // if the insert data index is between the first and last realized data index, we need
        // to insert items.
        int lastRealizedDataIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count - 1;
        int newStartingIndex = index;
        if (newStartingIndex > m_firstRealizedDataIndex &&
            newStartingIndex <= lastRealizedDataIndex)
        {
            // Inserted within the realized range
            int insertRangeStartIndex = newStartingIndex - m_firstRealizedDataIndex;
            for (int i = 0; i < count; i++)
            {
                // Insert null (sentinel) here instead of an element, that way we do not
                // end up creating a lot of elements only to be thrown out in the next layout.
                int insertRangeIndex = insertRangeStartIndex + i;
                int dataIndex = newStartingIndex + i;
                // This is to keep the contiguousness of the mapping
                m_realizedElementBounds.Insert(insertRangeIndex, new Rect());
            }
        }
        else if (index <= m_firstRealizedDataIndex)
        {
            // Items were inserted before the realized range.
            // We need to update m_firstRealizedDataIndex;
            m_firstRealizedDataIndex += count;
        }
    }

    private void OnItemsRemoved(int index, int count)
    {
        int lastRealizedDataIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count - 1;
        int startIndex = Math.Max(m_firstRealizedDataIndex, index);
        int endIndex = Math.Min(lastRealizedDataIndex, index + count - 1);
        bool removeAffectsFirstRealizedDataIndex = (index <= m_firstRealizedDataIndex);

        if (endIndex >= startIndex)
        {
            ClearRealizedRange(RealizationIndex(startIndex), endIndex - startIndex + 1);
        }

        if (removeAffectsFirstRealizedDataIndex &&
            m_firstRealizedDataIndex != -1)
        {
            m_firstRealizedDataIndex -= count;
        }
    }

    private void ClearRealizedRange(int startRealizedIndex, int count)
    {
        m_realizedElementBounds.RemoveRange(startRealizedIndex, count);
        if (startRealizedIndex == 0)
        {
            m_firstRealizedDataIndex = m_realizedElementBounds.Count == 0 ? 0 : m_firstRealizedDataIndex + count;
        }
    }

    private void ClearRealizedRange()
    {
        m_realizedElementBounds.Clear();
        m_firstRealizedDataIndex = 0;
    }

    private Rect GetCachedBoundsForDataIndex(int dataIndex)
    {
        return m_realizedElementBounds[RealizationIndex(dataIndex)];
    }

    private void SetCachedBoundsForDataIndex(int dataIndex, Rect bounds)
    {
        m_realizedElementBounds[RealizationIndex(dataIndex)] = bounds;
    }

    private Rect GetCachedBoundsForRealizationIndex(int relativeIndex)
    {
        return m_realizedElementBounds[relativeIndex];
    }

    void DebugTrace(string message, params object[] args)
    {
        Debug.WriteLine(message, args);
    }
}
```

## <a name="related-articles"></a>Articles connexes

- [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)
- [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
