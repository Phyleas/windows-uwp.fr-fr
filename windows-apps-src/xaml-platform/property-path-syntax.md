---
description: Vous pouvez utiliser la classe PropertyPath et la syntaxe de chaîne pour instancier une valeur PropertyPath en XAML ou dans le code.
title: Syntaxe de Property-path
ms.assetid: FF3ECF47-D81F-46E3-BE01-C839E0398025
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5a3cac608bbc985bb8c0db0e0cece219b3c566a8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155033"
---
# <a name="property-path-syntax"></a>Syntaxe de Property-path


Vous pouvez utiliser la classe [**PropertyPath**](/uwp/api/Windows.UI.Xaml.PropertyPath) et la syntaxe de chaîne pour instancier une valeur **PropertyPath** en XAML ou dans le code. Les valeurs **PropertyPath** sont utilisées par la liaison de données. Une syntaxe similaire est utilisée pour cibler les animations dans une table de montage séquentiel. Dans les deux scénarios, un chemin de propriété décrit la traversée d’une ou de plusieurs relations de propriétés d’objet qui finissent par devenir une seule propriété.

Vous pouvez affecter une chaîne de chemin de propriété directement à un attribut en XAML. Vous pouvez utiliser la même syntaxe de chaîne pour construire un [**PropertyPath**](/uwp/api/Windows.UI.Xaml.PropertyPath) qui définit une [**liaison**](/uwp/api/Windows.UI.Xaml.Data.Binding) dans le code, ou pour définir une cible d’animation dans le code à l’aide de [**SetTargetProperty**](/uwp/api/windows.ui.xaml.media.animation.storyboard.settargetproperty). Il existe deux zones de fonctionnalités distinctes dans Windows Runtime qui utilisent un chemin de propriété : la liaison de données et le ciblage d’animation. Le ciblage d’animation ne crée pas de valeurs de syntaxe Property-path sous-jacentes dans l’implémentation Windows Runtime ; il conserve l’information sous forme de chaîne. Toutefois, les concepts de traversée de propriété d’objet sont très similaires. La liaison de données et le ciblage d’animation évaluent chacun un chemin de propriété de façon légèrement différente. C’est la raison pour laquelle nous décrivons séparément la syntaxe du chemin de propriété dans chaque cas.

## <a name="property-path-for-objects-in-data-binding"></a>Chemin de propriété pour les objets de la liaison de données

Dans Windows Runtime, vous pouvez lier la valeur cible de n’importe quelle propriété de dépendance. La valeur de la propriété source d’une liaison de données n’a pas besoin d’être une propriété de dépendance ; il peut s’agir d’une propriété d’un objet métier (par exemple une classe écrite dans un langage Microsoft .NET ou en C++). L’objet source de la valeur de liaison peut également être un objet de dépendance existant déjà défini par l’application. La source peut être référencée soit par un simple nom de propriété, soit par une traversée des relations de propriétés d’objet dans le graphique d’objet de l’objet métier.

Vous pouvez lier soit une valeur de propriété individuelle, soit une propriété cible qui contient des listes ou des collections. Si votre source est une collection ou si le chemin d’accès spécifie une propriété de collection, le moteur de liaison de données met en correspondance les éléments de collection de la source à la cible de liaison, ce qui entraîne un comportement tel que le remplissage d’une [**zone**](/uwp/api/Windows.UI.Xaml.Controls.ListBox) de liste avec une liste d’éléments d’une collection de sources de données sans avoir à anticiper les éléments spécifiques dans cette collection.

### <a name="traversing-an-object-graph"></a>Traversée d’un graphique d’objet

L’élément de la syntaxe qui désigne la traversée d’une relation de propriété d’objet dans un graphique d’objet est le point (**.**). Chaque point d’une chaîne de chemin de propriété indique une division entre un objet (côté gauche du point) et une propriété de cet objet (côté droit du point). La chaîne est évaluée de gauche à droite, ce qui permet d’exécuter le code pas à pas dans plusieurs relations de propriétés d’objet. Prenons un exemple :

``` syntax
"{Binding Path=Customer.Address.StreetAddress1}"
```

Voici comment ce chemin d’accès est évalué :

1.  L’objet de contexte de données (ou un [**Source**](/uwp/api/windows.ui.xaml.data.binding.source) spécifié par le même [**Binding**](/uwp/api/Windows.UI.Xaml.Data.Binding)) donne lieu à la recherche de la propriété nommée « Customer ».
2.  L’objet qui a la valeur de la propriété « Customer » donne lieu à la recherche de la propriété nommée « Address ».
3.  L’objet qui a la valeur de la propriété « Address » donne lieu à la recherche de la propriété nommée « StreetAddress1 ».

À chacune de ces étapes, la valeur est traitée comme un objet. Le type du résultat est vérifié uniquement lorsque la liaison est appliquée à une propriété spécifique. Cet exemple échoue si « Address » est juste une valeur de chaîne qui n’indique pas quelle est la partie de la chaîne correspondant à l’adresse. En règle générale, la liaison pointe vers les valeurs de propriétés imbriquées spécifiques d’un objet métier ayant une structure d’information connue et intentionnelle.

### <a name="rules-for-the-properties-in-a-data-binding-property-path"></a>Règles des propriétés dans un chemin de propriété de liaison de données

-   Toutes les propriétés référencées par un chemin de propriété doivent être publiques dans l’objet métier source.
-   La propriété de fin (dernière propriété nommée dans le chemin d’accès) doit être publique et modifiable. Vous ne pouvez pas lier des valeurs statiques.
-   La propriété end doit être en lecture/écriture si ce chemin d’accès est utilisé comme informations de [**chemin d’accès**](/uwp/api/windows.ui.xaml.data.binding.path) pour une liaison bidirectionnelle.

### <a name="indexers"></a>Indexeurs

Un chemin de propriété pour la liaison de données peut inclure des références à des propriétés indexées. Cela permet de lier des listes/vecteurs ordonnés ou des dictionnaires/mappages. Utilisez des crochets « \[ \] » pour indiquer une propriété indexée. Le contenu de ces crochets peut être soit un entier (pour la liste ordonnée), soit une chaîne sans guillemets (pour les dictionnaires). Vous pouvez également lier un dictionnaire où la clé est un entier. Vous pouvez utiliser différentes propriétés indexées dans le même chemin d’accès en séparant l’objet de la propriété à l’aide d’un point.

Prenons par exemple un objet métier qui contient une liste de « Teams » (liste ordonnée). Chacune des équipes (teams) a un dictionnaire de « Players » où chaque joueur est indexé par le nom de famille. Voici un exemple de chemin de propriété vers un lecteur spécifique sur la seconde équipe : «teams \[ 1 \] . Adversaires \[ Smith \] ». (Vous utilisez 1 pour indiquer le deuxième élément de « Teams », car la liste est indexée à partir de zéro.)

**Remarque**    La prise en charge de l’indexation pour les sources de données C++ est limitée. consultez la rubrique [liaison de données en profondeur](../data-binding/data-binding-in-depth.md).

### <a name="attached-properties"></a>Propriétés jointes

Les chemins de propriété peuvent inclure des références à des propriétés jointes. Comme le nom permettant d’identifier une propriété jointe comprend déjà un point, vous devez mettre entre parenthèses le nom de la propriété jointe afin que le point ne soit pas considéré comme une étape de propriété d’objet. Par exemple, la chaîne qui spécifie que vous souhaitez utiliser [**Canvas. ZIndex**](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc190397(v=vs.95)) comme chemin de liaison est « (Canvas. ZIndex) ». Pour plus d’informations sur les propriétés jointes, voir [Vue d’ensemble des propriétés jointes](attached-properties-overview.md).

### <a name="combining-property-path-syntax"></a>Combinaison d’éléments de la syntaxe du chemin de propriété

Vous pouvez combiner divers éléments de la syntaxe du chemin de propriété en une seule chaîne. Par exemple, vous pouvez définir un chemin de propriété qui fait référence à une propriété jointe indexée, si votre source de données contient une telle propriété.

### <a name="debugging-a-binding-property-path"></a>Débogage d’un chemin de propriété de liaison

Comme un chemin de propriété est interprété par un moteur de liaison et qu’il s’appuie sur des informations présentes uniquement au moment de l’exécution, vous devez souvent déboguer un chemin de propriété de liaison sans pouvoir compter sur la prise en charge classique au moment du design ou au moment de la compilation dans les outils de développement. Bien souvent, l’échec de la résolution d’un chemin de propriété se traduit au moment de l’exécution par une valeur vide et l’absence d’erreurs, car il s’agit du comportement de secours défini volontairement pour la résolution de la liaison. Heureusement, Microsoft Visual Studio fournit un mode de sortie du débogage qui peut isoler la partie d’un chemin de propriété spécifiant une source de liaison dont la résolution a échoué. Pour plus d’informations sur l’utilisation de cette fonctionnalité d’outil de développement, voir la section « Débogage » de la rubrique [Présentation détaillée de la liaison de données](../data-binding/data-binding-in-depth.md#debugging).

## <a name="property-path-for-animation-targeting"></a>Chemin de propriété pour le ciblage d’animation

Une animation s’appuie sur le ciblage d’une propriété de dépendance où des valeurs de table de montage séquentiel sont appliquées durant l’animation. Pour identifier l’objet dans lequel une propriété d’animation existe, l’animation cible un élément par son nom ([attribut x:Name](x-name-attribute.md)). Il est souvent nécessaire de définir un chemin de propriété qui commence par l’objet identifié en tant que [**Storyboard.TargetName**](/dotnet/api/system.windows.media.animation.storyboard.targetname) et se termine par la valeur de propriété de dépendance particulière à laquelle l’animation doit s’appliquer. Ce chemin de propriété est utilisé comme valeur pour [**Storyboard. TargetProperty**](/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms616983(v=vs.95)).

Pour plus d’informations sur la définition d’animations en XAML, voir [Animations dans une table de montage séquentiel](../design/motion/storyboarded-animations.md).

## <a name="simple-targeting"></a>Ciblage simple

Si vous animez une propriété qui existe sur l’objet ciblé lui-même et si une animation peut être appliquée directement au type de cette propriété (au lieu d’une sous-propriété de la valeur d’une propriété), il vous suffit de nommer la propriété animée sans qualification supplémentaire. Par exemple, si vous ciblez une sous-classe de [**forme**](/uwp/api/Windows.UI.Xaml.Shapes.Shape) telle que [**rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle), et que vous appliquez une [**couleur**](/uwp/api/Windows.UI.Color) animée à la propriété [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill) , le chemin de la propriété peut être « Fill ».

## <a name="indirect-property-targeting"></a>Ciblage de propriété indirect

Vous pouvez animer une propriété qui représente une sous-propriété de l’objet cible. En d’autres termes, s’il existe une propriété de l’objet cible qui est un objet lui-même et si cet objet a des propriétés, vous devez définir un chemin de propriété qui explique comment exécuter le code pas à pas de cette relation de propriété d’objet. Chaque fois que vous spécifiez un objet pour lequel vous souhaitez animer une sous-propriété, mettez entre parenthèses le nom de la propriété et spécifiez cette dernière en respectant le format *typename*.*propertyname*. Par exemple, pour spécifier que vous souhaitez la valeur de l’objet de la propriété [**RenderTransform**](/uwp/api/windows.ui.xaml.uielement.rendertransform) d’un objet cible, vous spécifiez « (UIElement. RenderTransform) » comme première étape du chemin d’accès de la propriété. Il ne s’agit pas d’un chemin d’accès complet, car aucune animation ne peut s’appliquer directement à une valeur [**Transform**](/uwp/api/Windows.UI.Xaml.Media.Transform). Par conséquent, pour cet exemple, vous complétez à présent le chemin de propriété afin que la propriété de fin soit une propriété de la sous-classe **Transform** pouvant être animée par une valeur **Double** : « (UIElement.RenderTransform).(CompositeTransform.TranslateX) »

## <a name="specifying-a-particular-child-in-a-collection"></a>Spécification d’un enfant particulier dans une collection

Pour spécifier un élément enfant dans une propriété de collection, vous pouvez utiliser un indexeur numérique. Utilisez des crochets « \[ \] » autour de la valeur d’index d’entier. Vous ne pouvez référencer que des listes ordonnées, pas des dictionnaires. Dans la mesure où une collection n’est pas une valeur qui peut être animée, l’utilisation d’un indexeur ne peut jamais représenter la propriété de fin d’un chemin de propriété.

Par exemple, pour spécifier que vous souhaitez animer la première couleur d’arrêt de couleur dans un [**LinearGradientBrush**](/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush) qui est appliqué à la propriété d' [**arrière-plan**](/uwp/api/windows.ui.xaml.controls.control.background) d’un contrôle, il s’agit du chemin d’accès de la propriété : « (Control. Background) ». (GradientBrush. GradientStops) \[ 0 \] . ( GradientStop. Color)». Notez que l’indexeur n’est pas la dernière étape du chemin d’accès et que la dernière étape, en particulier, doit faire référence à la propriété [**GradientStop.Color**](/uwp/api/windows.ui.xaml.media.gradientstop.color) de l’élément 0 de la collection pour lui appliquer une valeur animée [**Color**](/uwp/api/Windows.UI.Color).

## <a name="animating-an-attached-property"></a>Animation d’une propriété jointe

Il ne s’agit pas d’un scénario répandu. Toutefois, il est possible d’animer une propriété jointe, du moment que cette dernière possède une valeur de propriété qui correspond à un type d’animation. Comme le nom permettant d’identifier une propriété jointe comprend déjà un point, vous devez mettre entre parenthèses le nom de la propriété jointe afin que le point ne soit pas considéré comme une étape de propriété d’objet. Par exemple, la chaîne pour spécifier que vous souhaitez animer la propriété de la [**grille.**](/dotnet/api/system.windows.controls.grid.row) jointe de ligne sur un objet, utilisez le chemin d’accès de la propriété « (Grid. Row) ».

**Remarque**    Pour cet exemple, la valeur de [**Grid. Row**](/dotnet/api/system.windows.controls.grid.row) est un type de propriété **Int32** . Par conséquent, vous ne pouvez pas l’animer avec une animation **Double**. À la place, définissez un [**ObjectAnimationUsingKeyFrames**](/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames) qui a des composants [**DiscreteObjectKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.DiscreteObjectKeyFrame), où [**ObjectKeyFrame.Value**](/uwp/api/windows.ui.xaml.media.animation.objectkeyframe.value) a la valeur d’un entier tel que « 0 » ou « 1 ».

## <a name="rules-for-the-properties-in-an-animation-targeting-property-path"></a>Règles des propriétés dans un chemin de propriété de ciblage d’animation

-   Le point de départ supposé du chemin de la propriété est l’objet identifié par un [**Storyboard. TargetName**](/dotnet/api/system.windows.media.animation.storyboard.targetname).
-   Tous les objets et propriétés référencés dans le chemin de propriété doivent être publics.
-   La propriété de fin (dernière propriété nommée dans le chemin d’accès) doit être publique, être accessible en lecture/écriture et représenter une propriété de dépendance.
-   La propriété de fin doit avoir un type de propriété pouvant être animé par l’une des classes générales des types d’animation (animations [**Color**](/uwp/api/Windows.UI.Color), animations **Double**, animations [**Point**](/uwp/api/Windows.Foundation.Point), [**ObjectAnimationUsingKeyFrames**](/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames)).

## <a name="the-propertypath-class"></a>Classe PropertyPath

La classe [**PropertyPath**](/uwp/api/Windows.UI.Xaml.PropertyPath) est le type de propriété sous-jacent [**Binding. Path**](/uwp/api/windows.ui.xaml.data.binding.path) pour le scénario de liaison.

La plupart du temps, vous pouvez appliquer [**PropertyPath**](/uwp/api/Windows.UI.Xaml.PropertyPath) en XAML, sans recourir à la moindre ligne de code. Mais dans certains cas, vous pouvez être amené à définir un objet **PropertyPath** avec du code et l’affecter à une propriété au moment de l’exécution.

[**PropertyPath**](/uwp/api/Windows.UI.Xaml.PropertyPath) a un constructeur [**PropertyPath (String)**](/uwp/api/windows.ui.xaml.propertypath.-ctor) et n’a pas de constructeur par défaut. La chaîne que vous passez à ce constructeur est une chaîne définie à l’aide de la syntaxe du chemin de propriété, comme nous l’avons expliqué précédemment. Il s’agit également de la même chaîne que celle que vous utiliseriez pour assigner le [**chemin d’accès**](/uwp/api/windows.ui.xaml.data.binding.path) en tant qu’attribut XAML. La seule autre API de la classe **PropertyPath** est la propriété [**Path**](/uwp/api/windows.ui.xaml.propertypath.path), qui est accessible en lecture seule. Vous pouvez utiliser cette propriété comme chaîne de construction pour une autre instance de **PropertyPath**.

## <a name="related-topics"></a>Rubriques connexes

* [Présentation détaillée de la liaison de données](../data-binding/data-binding-in-depth.md)
* [Animations dans une table de montage séquentiel](../design/motion/storyboarded-animations.md)
* [Extension de balisage {Binding}](binding-markup-extension.md)
* [**PropertyPath**](/uwp/api/Windows.UI.Xaml.PropertyPath)
* [**Liaison**](/uwp/api/Windows.UI.Xaml.Data.Binding)
* [**Constructeur de liaison**](/uwp/api/windows.ui.xaml.data.binding.-ctor)
* [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext)