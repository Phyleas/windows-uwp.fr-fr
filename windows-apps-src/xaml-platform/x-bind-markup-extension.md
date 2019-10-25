---
description: 'L’extension de balisage xBind est une alternative haute performance à la liaison. xBind-nouveauté pour Windows 10 : s’exécute en moins de mémoire que la liaison et prend en charge un meilleur débogage.'
title: extension de balisage xBind
ms.assetid: 529FBEB5-E589-486F-A204-B310ACDC5C06
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: a25797f50ee76542b8f9543cb76453d2916368ac
ms.sourcegitcommit: 82d202478ab4d3011c5ddd2e852958c34336830d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/22/2019
ms.locfileid: "72715856"
---
# <a name="xbind-markup-extension"></a>{x :Bind} (extension de balisage)

**Remarque**  pour obtenir des informations générales sur l’utilisation de la liaison de données dans votre application avec **{x :bind}** (et pour une comparaison complète entre **{x :bind}** et **{Binding}** ), consultez [liaison de données en profondeur](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth).

L’extension de balisage **{x :bind}** (nouveauté pour Windows 10) est une alternative à **{Binding}** . **{x :bind}** s’exécute en moins de temps et moins de mémoire que **{Binding}** et prend en charge un meilleur débogage.

Au moment de la compilation XAML, **{x :bind}** est converti en code qui obtient une valeur d’une propriété sur une source de données et le définit sur la propriété spécifiée dans le balisage. L’objet de liaison peut éventuellement être configuré pour observer les modifications apportées à la valeur de la propriété de source de données et s’actualiser en fonction de ces modifications (`Mode="OneWay"`). Elle peut également être configurée pour envoyer de nouveau les modifications de sa propre valeur à la propriété source (`Mode="TwoWay"`).

Les objets de liaison créés par **{x :bind}** et **{Binding}** sont en grande partie fonctionnellement équivalents. Toutefois, **{x :bind}** exécute un code à usage spécial, qu’il génère au moment de la compilation, et **{Binding}** utilise l’inspection des objets Runtime à usage général. Par conséquent, les liaisons **{x :bind}** (souvent appelées liaisons compilées) offrent de bonnes performances, fournissent la validation de vos expressions de liaison au moment de la compilation et prennent en charge le débogage en vous permettant de définir des points d’arrêt dans les fichiers de code qui sont généré en tant que classe partielle pour votre page. Ces fichiers se trouvent dans votre dossier `obj`, avec des noms tels que ( C#pour)`<view name>.g.cs`.

> [!TIP]
> **{x :bind}** a un mode par défaut de **OneTime**, contrairement à **{Binding}** , qui a le mode par défaut **OneWay**. Cela a été choisi pour des raisons de performances, car l’utilisation de **OneWay** entraîne la génération d’un code supplémentaire pour se déformer et gérer la détection des modifications. Vous pouvez spécifier explicitement un mode pour utiliser une liaison OneWay ou TwoWay. Vous pouvez également utiliser [x :DefaultBindMode](x-defaultbindmode-attribute.md) pour modifier le mode par défaut pour **{x :bind}** pour un segment spécifique de l’arborescence de balises. Le mode spécifié s’applique à toute expression **{x :bind}** sur cet élément et ses enfants, qui ne spécifient pas explicitement un mode dans le cadre de la liaison.

**Exemples d’applications qui illustrent {x :Bind}**

-   [{x :Bind}, exemple](https://go.microsoft.com/fwlink/p/?linkid=619989)
-   [QuizGame](https://github.com/microsoft/Windows-appsample-networkhelper)
-   [Exemple de base de l’interface utilisateur XAML](https://go.microsoft.com/fwlink/p/?linkid=619992)

## <a name="xaml-attribute-usage"></a>Utilisation des attributs XAML

``` syntax
<object property="{x:Bind}" .../>
-or-
<object property="{x:Bind propertyPath}" .../>
-or-
<object property="{x:Bind bindingProperties}" .../>
-or-
<object property="{x:Bind propertyPath, bindingProperties}" .../>
-or-
<object property="{x:Bind pathToFunction.functionName(functionParameter1, functionParameter2, ...), bindingProperties}" .../>
```

| Conditions | Description |
|------|-------------|
| _propertyPath_ | Chaîne qui spécifie le chemin d’accès de la propriété pour la liaison. Pour plus d’informations, reportez-vous à la section [chemin de propriété](#property-path) ci-dessous. |
| _bindingProperties_ |
| _nom_propriété_=_value_\[, _nom_propriété_=_value_\]* | Une ou plusieurs propriétés de liaison spécifiées à l’aide d’une syntaxe de paire nom/valeur. |
| _propName_ | Nom de chaîne de la propriété à définir sur l’objet de liaison. Par exemple, « convertisseur ». |
| _value_ | Valeur à affecter à la propriété. La syntaxe de l’argument dépend de la propriété qui est définie. Voici un exemple d’utilisation de la _valeur_ _nom_propriété_=où la valeur est elle-même une extension de balisage : `Converter={StaticResource myConverterClass}`. Pour plus d’informations, consultez [propriétés que vous pouvez définir avec la section {x :bind}](#properties-that-you-can-set-with-xbind) ci-dessous. |

## <a name="examples"></a>Exemples

```XAML
<Page x:Class="QuizGame.View.HostView" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

Cet exemple de code XAML utilise **{x :bind}** avec une propriété **ListView. ItemTemplate** . Notez la déclaration d’une valeur **x :DataType** .

```XAML
  <DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```

## <a name="property-path"></a>Chemin de la propriété

*PropertyPath* définit le **chemin d’accès** pour une expression **{x :bind}** . **Path** est un chemin de propriété qui spécifie la valeur de la propriété, de la sous-propriété, du champ ou de la méthode à laquelle vous effectuez la liaison (source). Vous pouvez mentionner explicitement le nom de la propriété **path** : `{x:Bind Path=...}`. Ou vous pouvez l’omettre : `{x:Bind ...}`.

### <a name="property-path-resolution"></a>Résolution de chemin de propriété

**{x :bind}** n’utilise pas le **DataContext** comme source par défaut ; à la place, il utilise la page ou le contrôle utilisateur lui-même. Ainsi, le code-behind de votre page ou de votre contrôle utilisateur est recherché pour les propriétés, les champs et les méthodes. Pour exposer votre modèle de vue à **{x :bind}** , vous devez généralement ajouter de nouveaux champs ou propriétés au code-behind pour votre page ou votre contrôle utilisateur. Les étapes d’un chemin de propriété sont délimitées par des points (.), et vous pouvez inclure plusieurs délimiteurs pour traverser les sous-propriétés successives. Utilisez le délimiteur de point, quel que soit le langage de programmation utilisé pour implémenter l’objet lié.

Par exemple : dans une page, **Text = "{X :bind Employee. FirstName}"** recherche un membre **Employee** dans la page, puis un membre **FirstName** sur l’objet retourné par **Employee**. Si vous liez un contrôle d’éléments à une propriété qui contient les dépendants d’un employé, le chemin d’accès de votre propriété peut être « Employee. Dependents », et le modèle d’élément du contrôle des éléments s’occuperait d’afficher les éléments dans les « dépendants ».

Pour C++/CX, **{x :bind}** ne peut pas être lié à des champs et des propriétés privés dans la page ou le modèle de données : vous devez avoir une propriété publique pour pouvoir être liée. La surface d’exposition de la liaison doit être exposée en tant que classes/interfaces CX afin que nous puissions obtenir les métadonnées appropriées. L’attribut **\]pouvant être lié\[** ne doit pas être nécessaire.

Avec **x :bind**, vous n’avez pas besoin d’utiliser **ElementName = xxx** dans le cadre de l’expression de liaison. Au lieu de cela, vous pouvez utiliser le nom de l’élément comme première partie du chemin d’accès pour la liaison, car les éléments nommés deviennent des champs dans la page ou le contrôle utilisateur qui représente la source de liaison racine. 


### <a name="collections"></a>Collections

Si la source de données est une collection, un chemin d’accès de propriété peut spécifier des éléments dans la collection en fonction de leur position ou de leur index. Par exemple, «teams\[0\]. Players», où le littéral «\[\]» encadre le « 0 » qui demande le premier élément d’une collection indexée à zéro.

Pour utiliser un indexeur, le modèle doit implémenter **IList&lt;t&gt;** ou **IVector&lt;t&gt;** sur le type de la propriété qui va être indexée. (Notez que IReadOnlyList&lt;T&gt; et IVectorView&lt;T&gt; ne prennent pas en charge la syntaxe de l’indexeur.) Si le type de la propriété indexée prend en charge **INotifyCollectionChanged** ou **IObservableVector** et que la liaison est unidirectionnelle ou TwoWay, elle s’inscrit et écoute les notifications de modification sur ces interfaces. La logique de détection des modifications est mise à jour en fonction de toutes les modifications de collection, même si cela n’affecte pas la valeur d’index spécifique. Cela est dû au fait que la logique d’écoute est commune à toutes les instances de la collection.

Si la source de données est un dictionnaire ou un mappage, un chemin d’accès de propriété peut spécifier des éléments dans la collection en fonction de leur nom de chaîne. Par exemple **&lt;TextBlock Text = "{X :bind player\['John Smith'\]"/&gt;** recherche un élément dans le dictionnaire nommé « John Smith ». Le nom doit être entouré de guillemets, et les guillemets simples ou doubles peuvent être utilisés. L’outil Hat (^) peut être utilisé pour échapper des guillemets dans des chaînes. Il est généralement plus facile d’utiliser des guillemets autres que ceux utilisés pour l’attribut XAML. (Notez que IReadOnlyDictionary&lt;T&gt; et IMapView&lt;T&gt; ne prennent pas en charge la syntaxe de l’indexeur.)

Pour utiliser un indexeur de chaîne, le modèle doit implémenter **IDictionary&lt;String, t&gt;** ou **IMap&lt;string, t&gt;** sur le type de la propriété qui va être indexée. Si le type de la propriété indexée prend en charge **IObservableMap** et que la liaison est unidirectionnelle ou TwoWay, elle s’inscrit et écoute les notifications de modification sur ces interfaces. La logique de détection des modifications est mise à jour en fonction de toutes les modifications de collection, même si cela n’affecte pas la valeur d’index spécifique. Cela est dû au fait que la logique d’écoute est commune à toutes les instances de la collection.

### <a name="attached-properties"></a>Propriétés jointes

Pour effectuer une liaison aux [propriétés jointes](./attached-properties-overview.md), vous devez placer la classe et le nom de la propriété entre parenthèses après le point. Par exemple **, Text = "{X :bind Button22. ( Grid. Row)}»** . Si la propriété n’est pas déclarée dans un espace de noms XAML, vous devez la faire précéder d’un espace de noms XML, que vous devez mapper à un espace de noms de code au début du document.

### <a name="casting"></a>Employeurs

Les liaisons compilées sont fortement typées et résolvent le type de chaque étape dans un chemin d’accès. Si le type retourné n’a pas le membre, il échouera au moment de la compilation. Vous pouvez spécifier un cast pour indiquer à la liaison le type réel de l’objet. Dans le cas suivant, **obj** est une propriété de type Object, mais contient une zone de texte. nous pouvons donc utiliser **Text = "{X :bind ((TextBox) obj). Text} "** ou **Text =" {x :bind obj. (TextBox. Text)} "** .
Le champ **groups3** dans **Text = "{x :bind ((Data : SampleDataGroup) groups3\[0\]). Title} "** est un dictionnaire d’objets. vous devez donc effectuer un cast de celui-ci en **données : SampleDataGroup**. Notez l’utilisation des données XML **:** préfixe d’espace de noms pour le mappage du type d’objet à un espace de noms de code qui ne fait pas partie de l’espace de noms XAML par défaut.

_Remarque : la C#syntaxe de cast de style est plus flexible que la syntaxe de propriété jointe, et est la syntaxe recommandée à l’avenir._

## <a name="functions-in-binding-paths"></a>Fonctions dans les chemins de liaison

À compter de Windows 10, version 1607, **{x :bind}** prend en charge l’utilisation d’une fonction comme étape feuille du chemin de liaison. Il s’agit d’une fonctionnalité puissante pour la liaison de liaison qui permet plusieurs scénarios dans le balisage. Pour plus d’informations, consultez [liaisons de fonction](../data-binding/function-bindings.md) .

## <a name="event-binding"></a>Liaison d’événements

La liaison d’événements est une fonctionnalité unique pour la liaison compilée. Elle vous permet de spécifier le gestionnaire d’un événement à l’aide d’une liaison, plutôt que d’être une méthode sur le code-behind. Par exemple : **cliquez sur = "{X :bind rootFrame. GoForward}"** .

Pour les événements, la méthode cible ne doit pas être surchargée et doit également :

- Correspond à la signature de l’événement.
- OU n’ont pas de paramètres.
- OU ont le même nombre de paramètres de types qui peuvent être assignés à partir des types des paramètres d’événement.

Dans le code-behind généré, la liaison compilée gère l’événement et l’achemine vers la méthode sur le modèle, en évaluant le chemin d’accès de l’expression de liaison lorsque l’événement se produit. Cela signifie que, contrairement aux liaisons de propriété, il n’effectue pas le suivi des modifications apportées au modèle.

Pour plus d’informations sur la syntaxe de chaîne pour un chemin de propriété, consultez [syntaxe de chemin de propriété](property-path-syntax.md), en gardant à l’esprit les différences décrites ici pour **{x :bind}** .

## <a name="properties-that-you-can-set-with-xbind"></a>Propriétés que vous pouvez définir avec {x :Bind}

**{x :bind}** est illustrée par la syntaxe d’espace réservé *bindingProperties* , car il existe plusieurs propriétés de lecture/écriture qui peuvent être définies dans l’extension de balisage. Les propriétés peuvent être définies dans n’importe quel ordre avec des paires de *valeurs* de=*nom_propriété* séparées par des virgules. Notez que vous ne pouvez pas inclure de sauts de ligne dans l’expression de liaison. Certaines des propriétés requièrent des types qui n’ont pas de conversion de type. ils nécessitent donc des extensions de balisage de leur propre imbrication dans le **{x :bind}** .

Ces propriétés fonctionnent à peu près de la même façon que les propriétés de la classe de [**liaison**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) .

| Propriété | Description |
|----------|-------------|
| **Chemin d’accès** | Consultez la section [chemin d’accès](#property-path) de la propriété ci-dessus. |
| **Onduleur** | Spécifie l’objet convertisseur qui est appelé par le moteur de liaison. Le convertisseur peut être défini en XAML, mais uniquement si vous faites référence à une instance d’objet que vous avez assignée dans une référence d' [extension de balisage {StaticResource}](staticresource-markup-extension.md) à cet objet dans le dictionnaire de ressources. |
| **ConverterLanguage** | Spécifie la culture à utiliser par le convertisseur. (Si vous définissez **ConverterLanguage** , vous devez également définir le **convertisseur**.) La culture est définie en tant qu’identificateur basé sur des normes. Pour plus d’informations, consultez [**ConverterLanguage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage). |
| **ConverterParameter** | Spécifie le paramètre de convertisseur qui peut être utilisé dans la logique de convertisseur. (Si vous définissez **ConverterParameter** , vous devez également définir le **convertisseur**.) La plupart des convertisseurs utilisent une logique simple qui obtient toutes les informations dont ils ont besoin à partir de la valeur passée à convertir, et n’a pas besoin d’une valeur **ConverterParameter** . Le paramètre **ConverterParameter** est utilisé pour les implémentations de convertisseur modérément avancé qui ont plusieurs logiques qui dépassent ce qui est passé dans **ConverterParameter**. Vous pouvez écrire un convertisseur qui utilise des valeurs autres que des chaînes, mais ce n’est pas souvent le cas, consultez la section Notes dans [**ConverterParameter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterparameter) pour plus d’informations. |
| **FallbackValue** | Spécifie une valeur à afficher lorsque la source ou le chemin d’accès ne peut pas être résolu. |
| **Mode** | Spécifie le mode de liaison, comme l’une des chaînes suivantes : « OneTime », « OneWay » ou « TwoWay ». La valeur par défaut est « OneTime ». Notez que cela diffère de la valeur par défaut pour **{Binding}** , qui est « OneWay » dans la plupart des cas. |
| **TargetNullValue** | Spécifie une valeur à afficher lorsque la valeur source est résolue mais qu’elle est explicitement **null**. |
| **BindBack** | Spécifie une fonction à utiliser pour le sens inverse d’une liaison bidirectionnelle. |
| **UpdateSourceTrigger** | Spécifie quand envoyer à nouveau les modifications du contrôle vers le modèle dans les liaisons TwoWay. La valeur par défaut de toutes les propriétés, à l’exception de TextBox. Text, est PropertyChanged ; TextBox. Text est LostFocus.|

> [!NOTE]
> Si vous convertissez le balisage de **{Binding}** en **{x :bind}** , tenez compte des différences dans les valeurs par défaut de la propriété **mode** .
> [**x :DefaultBindMode**](https://docs.microsoft.com/windows/uwp/xaml-platform/x-defaultbindmode-attribute) peut être utilisé pour modifier le mode par défaut de x :bind pour un segment spécifique de l’arborescence de balises. Le mode sélectionné appliquera toutes les expressions x :Bind sur cet élément et ses enfants, qui ne spécifient pas explicitement un mode dans le cadre de la liaison. OneTime est plus performant que OneWay, car l’utilisation de OneWay entraînera la génération d’un plus grand code pour l’accrochage et la gestion de la détection des modifications.

## <a name="remarks"></a>Remarques

Comme **{x :bind}** utilise du code généré pour tirer parti de ses avantages, elle nécessite des informations de type au moment de la compilation. Cela signifie que vous ne pouvez pas lier à des propriétés où vous ne connaissez pas le type à l’avance. Pour cette raison, vous ne pouvez pas utiliser **{x :bind}** avec la propriété **DataContext** , qui est de type **Object**, et qui est également susceptible d’être modifiée au moment de l’exécution.

Lorsque vous utilisez **{x :bind}** avec des modèles de données, vous devez indiquer le type auquel il est lié en définissant une valeur **x :DataType** , comme indiqué dans la section [exemples](#examples) . Vous pouvez également définir le type sur une interface ou un type de classe de base, puis utiliser des casts si nécessaire pour formuler une expression complète.

Les liaisons compilées dépendent de la génération du code. Par conséquent, si vous utilisez **{x :bind}** dans un dictionnaire de ressources, le dictionnaire de ressources doit avoir une classe code-behind. Consultez les [dictionnaires de ressources avec {x :bind}](../data-binding/data-binding-in-depth.md#resource-dictionaries-with-x-bind) pour obtenir un exemple de code.

Les pages et les contrôles utilisateur qui incluent des liaisons compilées auront une propriété « Bindings » dans le code généré. Cela comprend les méthodes suivantes :

- **Update ()** : met à jour les valeurs de toutes les liaisons compilées. Les liaisons à sens unique ou bidirectionnelles auront les écouteurs connectés pour détecter les modifications.
- **Initialize ()** : si les liaisons n’ont pas déjà été initialisées, elle appellera Update () pour initialiser les liaisons
- **StopTracking ()** : déconnectera tous les écouteurs créés pour les liaisons unidirectionnelles et bidirectionnelles. Elles peuvent être réinitialisées à l’aide de la méthode Update ().

> [!NOTE]
> À partir de Windows 10, version 1607, l’infrastructure XAML fournit un convertisseur de valeur booléenne intégré à Visibility. Le convertisseur mappe **true** à la valeur d’énumération **visible** et **false** à **réduire** afin que vous puissiez lier une propriété de visibilité à un booléen sans créer de convertisseur. Notez qu’il ne s’agit pas d’une fonctionnalité de liaison de fonction, mais uniquement d’une liaison de propriété. Pour utiliser le convertisseur intégré, la version minimale du kit de développement logiciel (SDK) cible de votre application doit être 14393 ou une version ultérieure. Vous ne pouvez pas l’utiliser quand votre application cible des versions antérieures de Windows 10. Pour plus d’informations sur les versions cibles, consultez [version adaptative code](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

**Conseil**   si vous devez spécifier une accolade unique pour une valeur, par exemple dans [**chemin d’accès**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) ou [**ConverterParameter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterparameter), faites-le précéder d’une barre oblique inverse : `\{`. Vous pouvez également inclure la chaîne entière qui contient les accolades qui nécessitent un échappement dans un ensemble de devis secondaire, par exemple `ConverterParameter='{Mix}'`.

Le [**convertisseur**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter), [**ConverterLanguage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage) et **ConverterLanguage** sont tous liés au scénario de conversion d’une valeur ou d’un type de la source de liaison en un type ou une valeur qui est compatible avec la propriété de cible de liaison. Pour obtenir plus d’informations et des exemples, consultez la section « conversions de données » de la rubrique [liaison de données en profondeur](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth).

**{x :bind}** est une extension de balisage uniquement, sans possibilité de créer ou de manipuler de telles liaisons par programmation. Pour plus d’informations sur les extensions de balisage, consultez [vue d’ensemble du langage XAML](xaml-overview.md).

