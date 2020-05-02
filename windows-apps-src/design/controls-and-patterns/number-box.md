---
Description: NumberBox est un contrôle qui permet d’afficher et de modifier des nombres.
title: Zone de nombre
template: detail.hbs
ms.date: 11/27/2019
ms.topic: article
keywords: windows 10, uwp
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: d0e4a290da0d67560715f80bf20fc6ae4d44a8f6
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81266957"
---
# <a name="number-box"></a>Zone de nombre

Représente un contrôle qui permet d’afficher et de modifier des nombres. Il prend en charge la validation, la définition d’un intervalle d’incrémentation et le calcul en ligne d’équations de base (multiplication, division, addition, soustraction, etc.).

**Obtenir la bibliothèque d’interface utilisateur Windows**

|  |  |
| - | - |
| ![Logo WinUI](images/winui-logo-64x64.png) | Le contrôle **NumberBox** nécessite la bibliothèque d’interface utilisateur Windows, package NuGet qui contient les nouveaux contrôles et fonctionnalités d’interface utilisateur destinés aux applications UWP. Pour plus d’informations, notamment des instructions d’installation, consultez [Vue d’ensemble de la bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

**API de la bibliothèque d'interface utilisateur Windows :** [NumberBox, classe](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.NumberBox)

> [!TIP]
> Tout au long de ce document, nous utilisons l’alias **muxc** en XAML pour représenter les API de la bibliothèque d’interface utilisateur Windows que nous avons incluses dans notre projet. Nous l’avons ajouté à notre élément [Page](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page) : `xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`
>
>Dans le code-behind, nous utilisons également l’alias **muxc** en C# pour représenter les API de la bibliothèque d’interface utilisateur Windows que nous avons incluses dans notre projet. Nous avons ajouté cette instruction **using** en haut du fichier : `using muxc = Microsoft.UI.Xaml.Controls;`

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

Vous pouvez utiliser un contrôle NumberBox pour capturer et afficher des entrées mathématiques. Si vous avez besoin d’une zone de texte modifiable qui n’accepte pas que des nombres, utilisez le contrôle [TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox). Si vous avez besoin d’une zone de texte modifiable qui accepte les mots de passe ou d’autres entrées sensibles, consultez [PasswordBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox). Si vous avez besoin d’une zone de texte pour entrer des termes de recherche, consultez [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox). Si vous avez besoin d’entrer ou de modifier du texte mis en forme, consultez [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox).

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous avez installé l’application <strong style="font-weight: semi-bold">XAML Controls Gallery</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/TextBox">l’ouvrir et voir NumberBox en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

### <a name="create-a-simple-numberbox"></a>Créer un NumberBox simple

Le code XAML ci-dessous montre l’apparence par défaut d’un NumberBox de base. Utilisez [x:Bind](/windows/uwp/xaml-platform/x-bind-markup-extension#property-path) pour vérifier que les données présentées à l’utilisateur sont synchronisées avec celles stockées dans votre application.

```xaml
<muxc:NumberBox Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}" />
```

![Champ d’entrée avec le focus indiquant 0.](images/numberbox-basic.png)

### <a name="labeling-numberbox"></a>Étiquetage de NumberBox

Utilisez `Header` ou `PlaceholderText` si l’intention du NumberBox n’est pas claire. `Header` est visible, qu’il y ait ou non une valeur dans le NumberBox.

```xaml
<muxc:NumberBox Header="Enter a number:"
    Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}" />
```

![En-tête indiquant « Enter expression : » au-dessus d’un NumberBox.](images/numberbox-header.png)

`PlaceholderText` s’affiche à l’intérieur du NumberBox uniquement quand `Value` est NaN ou quand l’entrée est effacée par l’utilisateur.

```xaml
<muxc:NumberBox PlaceholderText="1+2^2"
    Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}" />
```

![NumberBox contenant le texte d’espace réservé « A + B ».](images/numberbox-placeholder-text.png)

### <a name="enable-calculation-support"></a>Activer la prise en charge des calculs

L’affectation de true à la propriété `AcceptsExpression` permet à NumberBox d’évaluer des expressions en ligne de base, notamment des multiplications, des divisions, des additions et des soustractions, selon l’ordre standard des opérations. L’évaluation est déclenchée en cas de perte du focus ou quand l’utilisateur appuie sur la touche Entrée. Une fois qu’une expression est évaluée, sa forme d’origine n’est pas conservée.

```xaml
<muxc:NumberBox Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}"
    AcceptsExpression="True" />
```

### <a name="increment-and-decrement-stepping"></a>Intervalle d’incrémentation et de décrémentation

Utilisez la propriété `SmallChange` pour indiquer de combien la valeur dans un NumberBox doit changer quand le NumberBox a le focus et que l’utilisateur :

- fait défiler le contrôle
- appuie sur la flèche Haut
- appuie sur la flèche Bas

Utilisez la propriété `LargeChange` pour indiquer de combien la valeur dans un NumberBox doit changer quand le NumberBox a le focus et que l’utilisateur appuie sur la touche Pg. préc ou Pg. suiv.

Utilisez la propriété `SpinButtonPlacementMode` pour activer les boutons sur lesquels vous pouvez cliquer pour incrémenter ou décrémenter la valeur de NumberBox par la quantité spécifiée dans la propriété `SmallChange`. Ces boutons sont désactivés si une valeur Maximum ou Minimum est dépassée.

Définissez `SpinButtonPlacementMode` avec `Inline` pour faire apparaître les boutons à côté du contrôle.

```xaml
<muxc:NumberBox Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}"
    SmallChange="10"
    LargeChange="100"
    SpinButtonPlacementMode="Inline" />
```

![NumberBox avec, à côté, deux boutons : l’un contenant une flèche vers le bas et l’autre une flèche vers le haut.](images/numberbox-spinbutton-inline.png)

Définissez `SpinButtonPlacementMode` avec `Compact` pour faire apparaître les boutons sous la forme d’un menu volant uniquement quand le NumberBox a le focus.

```xaml
<muxc:NumberBox Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}"
    SmallChange="10"
    LargeChange="100"
    SpinButtonPlacementMode="Compact" />
```

![NumberBox avec, à l’intérieur, une petite icône composée de deux flèches : l’une pointant vers le haut et l’autre vers le bas.](images/numberbox-spinbutton-compact-non-visible.png)

![NumberBox avec, sur le côté, deux boutons flottant à un niveau plus élevé : l’un avec une flèche vers le bas et l’autre avec une flèche vers le haut.](images/numberbox-spinbutton-compact-visible.png)

### <a name="enabling-input-validation"></a>Activation de la validation des entrées

Définissez `ValidationMode` avec `InvalidInputOverwritten` pour permettre à NumberBox de remplacer les entrées non valides, qui ne sont pas numériques ou qui ne sont pas basées sur des formules légales, par la dernière valeur valide quand une évaluation est déclenchée après perte du focus ou activation de la touche Entrée.

```xaml
<muxc:NumberBox Header="Quantity"
    Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}"
    ValidationMode="InvalidInputOverwritten" />
```

Définissez `ValidationMode` avec `Disabled` pour configurer une validation d’entrée personnalisée.

En ce qui concerne les points décimaux et les virgules, la mise en forme utilisée par un utilisateur est remplacée par la celle configurée pour le NumberBox. Une erreur de validation de l’entrée n’est pas déclenchée.

### <a name="formatting-input"></a>Mise en forme de l’entrée

Vous pouvez utiliser la [mise en forme des nombres](/uwp/api/windows.globalization.numberformatting) pour mettre en forme la valeur d’un NumberBox en configurant une instance d’une classe de mise en forme et en l’affectant à la propriété `NumberFormatter`. Les décimales, devises, pourcentages et chiffres significatifs comptent parmi les classes de mise en forme des nombres disponibles. Notez que l’arrondi est également défini par les propriétés de mise en forme des nombres.

Voici un exemple d’utilisation de DecimalFormatter qui met en forme la valeur d’un NumberBox (un chiffre entier, deux chiffres de fraction et arrondi au 0,25 le plus proche) :

```xaml
<muxc:NumberBox  x:Name="FormattedNumberBox"
    Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}" />
```

```csharp
private void SetNumberBoxNumberFormatter()
{
    IncrementNumberRounder rounder = new IncrementNumberRounder();
    rounder.Increment = 0.25;
    rounder.RoundingAlgorithm = RoundingAlgorithm.RoundUp;

    DecimalFormatter formatter = new DecimalFormatter();
    formatter.IntegerDigits = 1;
    formatter.FractionDigits = 2;
    formatter.NumberRounder = rounder;
    FormattedNumberBox.NumberFormatter = formatter;
}
```

![NumberBox indiquant une valeur de 0,00.](images/numberbox-formatted.png)

En ce qui concerne les points décimaux et les virgules, la mise en forme utilisée par un utilisateur est remplacée par la celle configurée pour le NumberBox. Une erreur de validation de l’entrée n’est pas déclenchée.

## <a name="remarks"></a>Remarks

### <a name="input-scope"></a>Étendue d’entrée

`Number` est utilisé pour l’[étendue d’entrée](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue). Cette étendue d’entrée est conçue pour fonctionner avec des chiffres compris entre 0 et 9. Il est possible de la remplacer, mais les autres types InputScope ne sont pas explicitement pris en charge.

### <a name="not-a-number"></a>N’est pas un nombre

Quand l’entrée est effacée d’un NumberBox, `Value` est défini avec `NaN` pour indiquer qu’aucune valeur numérique n’est présente.

### <a name="expression-evaluation"></a>Évaluation d’expression

NumberBox utilise la notation infixe pour évaluer les expressions. Les opérateurs autorisés sont les suivants (par ordre de précédence) :

* ^
* */
* +-

Notez qu’il est possible d’utiliser des parenthèses pour remplacer les règles de précédence.

## <a name="recommendations"></a>Recommandations

* `Text` et `Value` facilitent la capture de la valeur d’un NumberBox au format String ou Double sans qu’il soit nécessaire de convertir la valeur d’un type à un autre. Si vous souhaitez modifier programmatiquement la valeur d’un NumberBox, nous vous recommandons de le faire par le biais de la propriété `Value`. `Value` remplace `Text` dans la configuration initiale. Après la configuration initiale, les modifications apportées à l’un sont propagées à l’autre, mais le fait d’apporter constamment des modifications programmatiquement avec `Value` permet d’éviter tout malentendu conceptuel selon lequel NumberBox accepte des caractères non numériques par le biais de `Text`.
* Utilisez `Header` ou `PlaceholderText` pour informer les utilisateurs que NumberBox accepte uniquement des caractères numériques en entrée. La représentation orthographique des nombres, comme « un », n’est pas résolue en valeur acceptée.
