---
description: Identifie de manière unique les éléments objet pour l’accès à l’objet instancié depuis le code-behind ou le code général.
title: Attribut xName
ms.assetid: 4FF1F3ED-903A-4305-B2BD-DCD29E0C9E6D
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b5fd9c2b482eb79fcdace2c068afa21bad673de2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161673"
---
# <a name="xname-attribute"></a>Attribut x:Name


Identifie de manière unique les éléments objet pour l’accès à l’objet instancié depuis le code-behind ou le code général. Une fois appliqué à un modèle de programmation de stockage, **x:Name** peut être considéré comme équivalent à la variable qui contient une référence d’objet, telle que renvoyée par un constructeur.

## <a name="xaml-attribute-usage"></a>Utilisation des attributs XAML

``` syntax
<object x:Name="XAMLNameValue".../>
```

## <a name="xaml-values"></a>Valeurs XAML

| Terme | Description |
|------|-------------|
| XAMLNameValue | Chaîne qui se conforme aux restrictions de la grammaire XamlName. |

##  <a name="xamlname-grammar"></a>Grammaire XamlName

Les points suivants représentent la grammaire normative qui régit une chaîne servant de clé dans cette implémentation XAML :

``` syntax
XamlName ::= NameStartChar (NameChar)*
NameStartChar ::= LetterCharacter | '_'
NameChar ::= NameStartChar | DecimalDigit
LetterCharacter ::= ('a'-'z') | ('A'-'Z')
DecimalDigit ::= '0'-'9'
CombiningCharacter::= none
```

-   Les caractères sont limités à la plage ASCII inférieure, et plus particulièrement aux lettres majuscules et minuscules de l’alphabet romain, chiffres et le caractère de soulignement ( \_ ).
-   La plage de caractères Unicode n’est pas prise en charge.
-   Un nom ne peut pas commencer par un chiffre. Certaines implémentations d’outils ajoutent un trait de soulignement ( \_ ) à une chaîne si l’utilisateur fournit un chiffre comme caractère initial, ou l’outil génère automatiquement des valeurs **x :Name** en fonction d’autres valeurs qui contiennent des chiffres.

## <a name="remarks"></a>Remarques

Le **x:Name** spécifié devient le nom d’un champ créé dans le code sous-jacent lors du traitement du code XAML, et ce champ contient une référence à l’objet. Le processus de création de ce champ est effectué par les étapes cible MSBuild, lesquelles ont également la responsabilité de joindre les classes partielles pour un fichier XAML et son code-behind. Ce comportement n’est pas nécessairement propre au langage XAML ; il s’agit de l’implémentation particulière que la programmation de plateforme Windows universelle (UWP) pour XAML applique afin d’utiliser **x:Name** dans ses modèles de programmation et d’application.

Chaque **x:Name** défini doit être unique au sein d’un namescope XAML. En général, un namescope XAML est défini au niveau de l’élément racine d’une page chargée et contient tous les éléments sous cet élément dans une seule page XAML. D’autres namescopes XAML sont définis par n’importe quel modèle de contrôle ou modèle de données défini sur cette page. Au moment de l’exécution, un autre namescope XAML est créé pour la racine de l’arborescence d’objets créée à partir d’un modèle de contrôle appliqué, ainsi que par les arborescences d’objets créées à partir d’un appel à la méthode [**XamlReader.Load**](/uwp/api/windows.ui.xaml.markup.xamlreader.load). Pour plus d’informations, voir [Namescopes XAML](xaml-namescopes.md).

Les outils de conception génèrent souvent automatiquement des valeurs **x:Name** pour les éléments lorsqu’ils sont introduits dans l’aire de conception. Le schéma de génération automatique varie selon le concepteur que vous utilisez, mais le schéma habituel consiste à générer une chaîne qui commence par le nom de la classe qui stocke l’élément, suivi d’un entier progressif. Par exemple, si vous introduisez le premier élément [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) dans le concepteur, vous pouvez constater que dans le code XAML, la valeur d’attribut **x:Name** de cet élément est « Button1 ».

Il n’est pas possible de définir **x:Name** dans la syntaxe de l’élément de propriété XAML, ou dans le code à l’aide de [**SetValue**](/uwp/api/windows.ui.xaml.dependencyobject.setvalue). **x:Name** peut uniquement être défini à l’aide de la syntaxe de l’attribut XAML sur les éléments.

**Remarque**    En particulier pour les applications C++/CX, un champ de stockage pour une référence **x :Name** n’est pas créé pour l’élément racine d’un fichier ou d’une page XAML. Si vous devez référencer l’objet racine à partir d’un fichier code-behind C++, utilisez d’autres API ou une traversée d’arborescence. Par exemple, vous pouvez appeler [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) pour un élément enfant nommé connu avant d’appeler [**Parent**](/uwp/api/windows.ui.xaml.frameworkelement.parent).

### <a name="xname-and-other-name-properties"></a>x:Name et autres propriétés Name

Certains types utilisés en XAML UWP ont également une propriété nommée **Name**. Par exemple, [**FrameworkElement.Name**](/uwp/api/windows.ui.xaml.frameworkelement.name) et [**TextElement.Name**](/uwp/api/windows.ui.xaml.documents.textelement.name).

Si **Name** est disponible en tant que propriété définissable sur un élément, **Name** et **x:Name** peuvent être utilisés de façon interchangeable en XAML, mais une erreur est générée si les deux attributs sont spécifiés sur le même élément. Il existe également certains cas où il y a une propriété **Name** mais en lecture seule (comme [**VisualState.Name**](/uwp/api/windows.ui.xaml.visualstate.name)). Si tel est le cas, vous devez toujours utiliser **x:Name** pour nommer cet élément dans le code XAML, et l’objet **Name** en lecture seule existe pour certains scénarios de code moins courant.

**Remarque** Il est préférable ne de pas utiliser   [**FrameworkElement.Name**](/uwp/api/windows.ui.xaml.frameworkelement.name) comme un moyen de modifier des valeurs initialement définies par **x:Name**, bien qu’il existe certains scénarios faisant exception à cette règle générale. Dans les scénarios standard, la création et la définition de namescopes XAML sont une opération du processeur XAML. La modification de l’objet **FrameworkElement.Name** au moment de l’exécution peut engendrer un mauvais alignement de l’attribution des noms du namescope XAML/champ privé, ce dont il est difficile d’effectuer le suivi dans votre code-behind.

### <a name="xname-and-xkey"></a>x:Name et x:Key

**x:Name** peut être appliqué en tant qu’attribut à des éléments dans un [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) pour agir en tant que substitut de l’[attribut x:Key](x-key-attribute.md). (C’est une règle que tous les éléments d’un **ResourceDictionary** doivent avoir un attribut x:Key.) Cela est courant dans le cas des [Animations dans une table de montage séquentiel](../design/motion/storyboarded-animations.md). Pour plus d’informations, voir la section correspondante de [Références aux ressources ResourceDictionary et XAML](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md).