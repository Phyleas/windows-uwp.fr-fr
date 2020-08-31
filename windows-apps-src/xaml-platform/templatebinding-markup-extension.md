---
description: Lie la valeur d’une propriété dans un modèle de contrôle à la valeur d’une autre propriété exposée sur le contrôle basé sur un modèle. TemplateBinding peut uniquement être utilisé dans une définition ControlTemplate en XAML.
title: Extension de balisage TemplateBinding
ms.assetid: FDE71086-9D42-4287-89ED-8FBFCDF169DC
ms.date: 10/29/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3b368ca2f429d52674ba1cb3493012d54dc0848a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154923"
---
# <a name="templatebinding-markup-extension"></a>Extension de balisage {TemplateBinding}

Lie la valeur d’une propriété dans un modèle de contrôle à la valeur d’une autre propriété exposée sur le contrôle basé sur un modèle. **TemplateBinding** ne peut être utilisé que dans une définition [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) en XAML.

## <a name="xaml-attribute-usage"></a>Utilisation des attributs XAML

``` syntax
<object propertyName="{TemplateBinding sourceProperty}" .../>
```

## <a name="xaml-attribute-usage-for-setter-property-in-template-or-style"></a>Utilisation des attributs XAML (pour la propriété Setter dans un modèle ou style)

``` syntax
<Setter Property="propertyName" Value="{TemplateBinding sourceProperty}" .../>
```

## <a name="xaml-values"></a>Valeurs XAML

| Terme | Description |
|------|-------------|
| propertyName | Nom de la propriété définie dans la syntaxe de la méthode setter. Il doit s’agir d’une propriété de dépendance. |
| sourceProperty | Nom d’une autre propriété de dépendance qui existe sur le type qui est basé sur un modèle. |

## <a name="remarks"></a>Remarques

L’utilisation de **TemplateBinding** est un élément fondamental de la définition d’un modèle de contrôle, si vous êtes l’auteur d’un contrôle personnalisé ou si vous remplacez un modèle de contrôle pour des contrôles existants. Pour plus d’informations, voir [Démarrage rapide : modèles de contrôles](/previous-versions/windows/apps/hh465374(v=win.10)).

Il est relativement courant pour *propertyName* et *targetProperty* d’utiliser le même nom de propriété. Dans ce cas, un contrôle peut définir une propriété sur elle-même et la transmettre à une propriété existante nommée de manière intuitive de l’une de ses parties de composants. Par exemple, un contrôle qui incorpore un [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) dans sa composition, qui est utilisée pour afficher la propriété de **texte** du contrôle, peut inclure ce XAML en tant que composant dans le modèle de contrôle : `<TextBlock Text="{TemplateBinding Text}" .... />`

Les types utilisés comme valeur de la propriété de source et valeur de la propriété cible doivent correspondre. Il est impossible d’introduire un convertisseur lorsque vous utilisez **TemplateBinding**. Si les valeurs ne correspondent pas, une erreur se produit lors de l’analyse du code XAML. Si vous avez besoin d’un convertisseur, vous pouvez utiliser la syntaxe détaillée pour une liaison de modèle, par exemple : `{Binding RelativeSource={RelativeSource TemplatedParent}, Converter="..." ...}`

Toute tentative d’utilisation d’un **TemplateBinding** en dehors d’une définition [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) en XAML engendre une erreur d’analyse.

Vous pouvez utiliser **TemplateBinding** lorsque la valeur du parent basé sur un modèle est également différée comme autre liaison. L’évaluation de **TemplateBinding** peut démarrer lorsque des valeurs sont affectées à des liaisons runtime requises.

**TemplateBinding** est toujours une liaison à sens unique. Les deux propriétés doivent toutes être des propriétés de dépendance.

**TemplateBinding** est une extension de balisage. Les extensions de balisage sont généralement implémentées pour éviter que les valeurs d’attribut ne soient autre chose que des valeurs littérales ou des noms de gestionnaire et lorsque l’exigence dépasse le cadre de la définition de convertisseurs de type sur certains types ou propriétés. Toutes les extensions de balisage XAML utilisent les caractères « { » et « } » dans leur syntaxe d’attribut, ce qui correspond à la convention qui permet au processeur XAML de reconnaître qu’une extension de balisage doit traiter l’attribut.

**Remarque**    Dans l’implémentation de processeur XAML Windows Runtime, il n’existe aucune représentation de classe de stockage pour **TemplateBinding**. **TemplateBinding** est à utiliser exclusivement dans le balisage XAML. Il n’y a pas de moyen simple de reproduire le comportement dans du code.

### <a name="xbind-in-controltemplate"></a>x :Bind dans ControlTemplate

> [!NOTE]
> L’utilisation de x :Bind dans un ControlTemplate requiert Windows 10, version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou version ultérieure. Pour plus d’informations sur les versions cibles, voir [Code adaptatif de version](../debug-test-perf/version-adaptive-code.md).

À compter de Windows 10, version 1809, vous pouvez utiliser l’extension de balisage **x :bind** partout où vous utilisez **TemplateBinding** dans un [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate). 

La propriété [TargetType](/uwp/api/windows.ui.xaml.controls.controltemplate.targettype) est obligatoire (pas facultative) sur [ControlTemplate](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) lors de l’utilisation de **x :bind**.

Avec la prise en charge de **x :bind** , vous pouvez utiliser des [liaisons de fonction](../data-binding/function-bindings.md) ainsi que des liaisons bidirectionnelles dans un [ControlTemplate](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate).

Dans cet exemple, la propriété **TextBlock. Text** prend la valeur **Button. Content. ToString**. Le TargetType sur le ControlTemplate fait office de source de données et atteint le même résultat qu’un TemplateBinding du parent.

```xaml
<ControlTemplate TargetType="Button">
    <Grid>
        <TextBlock Text="{x:Bind Content, Mode=OneWay}"/>
    </Grid>
</ControlTemplate>
```

## <a name="related-topics"></a>Rubriques connexes

* [Démarrage rapide : modèles de contrôle](/previous-versions/windows/apps/hh465374(v=win.10))
* [Présentation détaillée de la liaison de données](../data-binding/data-binding-in-depth.md)
* [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)
* [Vue d’ensemble du langage XAML](xaml-overview.md)
* [Vue d’ensemble des propriétés de dépendance](dependency-properties-overview.md)
 