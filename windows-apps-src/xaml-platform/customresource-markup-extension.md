---
description: Fournit une valeur pour tout attribut XAML en évaluant une référence à une ressource qui provient de l’implémentation d’une recherche de ressource personnalisée. La recherche de ressource est effectuée via l’implémentation d’une classe CustomXamlResourceLoader.
title: Extension de balisage CustomResource
ms.assetid: 3A59A8DE-E805-4F04-B9D9-A91E053F3642
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 094a2a957493787acef8e97b49b61778fc1ec42f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155103"
---
# <a name="customresource-markup-extension"></a>Extension de balisage {CustomResource}


Fournit une valeur pour tout attribut XAML en évaluant une référence à une ressource qui provient de l’implémentation d’une recherche de ressource personnalisée. La recherche de ressource est effectuée via l’implémentation d’une classe [**CustomXamlResourceLoader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader).

## <a name="xaml-attribute-usage"></a>Utilisation des attributs XAML

``` syntax
<object property="{CustomResource key}" .../>
```

## <a name="xaml-values"></a>Valeurs XAML

| Terme | Description |
|------|-------------|
| key | Clé pour la ressource demandée. La façon dont la clé est assignée initialement est spécifique à l’implémentation de la classe [**CustomXamlResourceLoader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) actuellement inscrite en vue de son utilisation. |

## <a name="remarks"></a>Remarques

**CustomResource** est une technique permettant d’obtenir des valeurs définies ailleurs dans un référentiel de ressources personnalisées. Cette technique relativement avancée n’est pas utilisée dans la plupart des scénarios des applications Windows Runtime.

Le mode de résolution de **CustomResource** en dictionnaire de ressources n’est pas décrit dans cette rubrique, car il peut varier considérablement selon la façon dont [**CustomXamlResourceLoader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) est implémenté.

La méthode [**GetResource**](/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource) de l’implémentation de [**CustomXamlResourceLoader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) est appelée par l’analyseur XAML Windows Runtime chaque fois qu’il rencontre l’utilisation de `{CustomResource}` dans le balisage. Le *resourceId* passé à **GetResource** vient de l’argument *key*. Par ailleurs, les autres paramètres d’entrée proviennent du contexte, par exemple la propriété à laquelle l’utilisation s’applique.

L’utilisation de `{CustomResource}` ne fonctionne pas par défaut (l’implémentation de base de [**GetResource**](/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource) est incomplète). Pour créer une référence valide à `{CustomResource}`, vous devez effectuer chacune des étapes suivantes :

1.  Dérivez une classe personnalisée de [**CustomXamlResourceLoader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) et remplacez la méthode [**GetResource**](/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource) . N’appelez pas la base dans l’implémentation.
2.  Définissez [**CustomXamlResourceLoader.Current**](/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.current) de manière à référencer votre classe dans une logique d’initialisation. Cette opération doit intervenir avant le chargement de tout code XAML de niveau page comprenant l’utilisation d’une extension `{CustomResource}`. Vous pouvez définir **CustomXamlResourceLoader.Current** dans le constructeur de sous-classe [**Application**](/uwp/api/Windows.UI.Xaml.Application) qui est automatiquement généré dans les modèles code-behind App.xaml.
3.  Vous pouvez à présent utiliser des extensions `{CustomResource}` dans le XAML que votre application charge en tant que pages, ou à partir de dictionnaires de ressources XAML.

**CustomResource** est une extension de balisage. Les extensions de balisage sont généralement implémentées pour éviter que les valeurs d’attribut ne soient autre chose que des valeurs littérales ou des noms de gestionnaire et lorsque l’exigence dépasse le cadre de la définition de convertisseurs de type sur certains types ou propriétés. Toutes les extensions de balisage en XAML utilisent les \{ caractères « » et « » \} dans leur syntaxe d’attribut, qui est la Convention selon laquelle un processeur XAML reconnaît qu’une extension de balisage doit traiter l’attribut.

## <a name="related-topics"></a>Rubriques connexes

* [Références aux ressources ResourceDictionary et XAML](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)
* [**CustomXamlResourceLoader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader)
* [**GetResource**](/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource)