---
description: Pour faciliter la saisie de données par les utilisateurs au moyen du clavier tactile, ou panneau de saisie, définissez l’étendue des entrées du contrôle de texte de sorte qu’elle corresponde au type de données attendu de la part de l’utilisateur.
MS-HAID: dev\_ctrl\_layout\_txt.use\_input\_scope\_to\_change\_the\_touch\_keyboard
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Utiliser l’étendue des entrées pour modifier le clavier tactile
ms.assetid: 6E5F55D7-24D6-47CC-B457-B6231EDE2A71
template: detail.hbs
keywords: clavier, accessibilité, navigation, focus, texte, entrées, interaction utilisateur
ms.date: 02/08/2017
ms.topic: article
ms.openlocfilehash: 9b274d7cafd179157571a51db1122153a33d846e
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93035072"
---
# <a name="use-input-scope-to-change-the-touch-keyboard"></a>Utiliser l’étendue des entrées pour modifier le clavier tactile

Pour faciliter la saisie de données par les utilisateurs au moyen du clavier tactile, ou panneau de saisie, définissez l’étendue des entrées du contrôle de texte de sorte qu’elle corresponde au type de données attendu de la part de l’utilisateur.

### <a name="important-apis"></a>API importantes
- [InputScope](/uwp/api/windows.ui.xaml.controls.textbox.inputscope)
- [InputScopeNameValue](/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue)


Le clavier tactile permet d’entrer du texte quand l’application est exécutée sur un appareil disposant d’un écran tactile. Le clavier tactile est appelé quand l’utilisateur appuie sur un champ d’entrée modifiable, tel qu’un **[contrôle TextBox](/uwp/api/Windows.UI.Xaml.Controls.TextBox)** ou **[RichEditBox](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox)** . Vous pouvez le rendre plus rapide et plus facile pour les utilisateurs d’entrer des données dans votre application en définissant l' *étendue des entrées* du contrôle de texte pour qu’elles correspondent au type de données que l’utilisateur doit entrer. L’étendue des entrées fournit au système une indication sur le type d’entrée de texte attendu par le contrôle, afin que le système puisse fournir une disposition de clavier tactile spécialisée pour le type d’entrée.

Par exemple, si une zone de texte est utilisée uniquement pour entrer un code PIN à 4 chiffres, définissez la propriété [**InputScope**](/uwp/api/windows.ui.xaml.controls.textbox.inputscope) sur **nombre** . Ceci indique au système qu’il doit afficher la disposition du pavé numérique, ce qui permet à l’utilisateur d’entrer plus facilement le code PIN.

> [!IMPORTANT]
> - Ces informations s’appliquent uniquement au clavier virtuel. Elles ne concernent pas les claviers matériels ou visuels figurant dans les Options d’ergonomie de Windows.
> - L’étendue des entrées n’entraîne l’exécution d’aucune validation des entrées et n’empêche pas l’utilisateur de saisir des données par le biais d’un clavier matériel ou d’un autre dispositif du même ordre. Vous restez responsable de la validation d’une entrée dans votre code, si nécessaire.

## <a name="changing-the-input-scope-of-a-text-control"></a>Modification de l’étendue des entrées d’un contrôle de texte

Les étendues d’entrée qui sont disponibles pour votre application sont membres de l’énumération **[InputScopeNameValue](/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue)** . Vous pouvez attribuer l’une de ces valeurs à la propriété **InputScope** d’un élément **[TextBox](/uwp/api/Windows.UI.Xaml.Controls.TextBox)** ou **[RichEditBox](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox)** .

> [!IMPORTANT]
> La propriété **[InputScope](/uwp/api/windows.ui.xaml.controls.passwordbox.inputscope)** de **[PasswordBox](/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)** ne prend en charge que les valeurs **de mot de passe** et **NumericPin** . Toute autre valeur est ignorée.

Ici, vous modifiez l’étendue des entrées de plusieurs zones de texte afin qu’elle corresponde aux données attendues pour chaque zone de texte.

**Pour modifier l’étendue des entrées en XAML**

1.  Dans le fichier XAML de votre page, recherchez la balise correspondant au contrôle de texte que vous voulez modifier.
2.  Ajoutez l’attribut [**InputScope**](/uwp/api/windows.ui.xaml.controls.textbox.inputscope) à la balise et spécifiez la valeur [**InputScopeNameValue**](/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue) qui correspond à l’entrée attendue.

    Voici certaines zones de texte pouvant apparaître dans un formulaire de contact client courant. Lorsque [**InputScope**](/uwp/api/windows.ui.xaml.controls.textbox.inputscope) est défini, un clavier tactile dont la disposition est appropriée aux données s’affiche pour chaque zone de texte.

    ```xaml
    <StackPanel Width="300">
        <TextBox Header="Name" InputScope="Default"/>
        <TextBox Header="Email Address" InputScope="EmailSmtpAddress"/>
        <TextBox Header="Telephone Number" InputScope="TelephoneNumber"/>
        <TextBox Header="Web site" InputScope="Url"/>
    </StackPanel>
    ```

**Pour modifier l’étendue des entrées dans le code**

1.  Dans le fichier XAML de votre page, recherchez la balise correspondant au contrôle de texte que vous voulez modifier. Le cas échéant, définissez l’[attribut x:Name](../../xaml-platform/x-name-attribute.md) de façon à pouvoir référencer le contrôle dans votre code.

    ```csharp
    <TextBox Header="Telephone Number" x:Name="phoneNumberTextBox"/>
    ```

2.  Instanciez un nouvel objet [**InputScope**](/uwp/api/Windows.UI.Xaml.Input.InputScope) .

    ```csharp
    InputScope scope = new InputScope();
    ```

3.  Instanciez un nouvel objet [**InputScopeName**](/uwp/api/Windows.UI.Xaml.Input.InputScopeName).
    
    ```csharp
    InputScopeName scopeName = new InputScopeName();
    ```

4.  Définissez la propriété [**NameValue**](/uwp/api/windows.ui.xaml.input.inputscopename.namevalue) de l’objet [**InputScopeName**](/uwp/api/Windows.UI.Xaml.Input.InputScopeName) sur une valeur de l’énumération [**InputScopeNameValue**](/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue).

    ```csharp
    scopeName.NameValue = InputScopeNameValue.TelephoneNumber;
    ```

5.  Ajoutez l’objet [**InputScopeName**](/uwp/api/Windows.UI.Xaml.Input.InputScopeName) à la collection de [**noms**](/uwp/api/windows.ui.xaml.input.inputscope.names) de l’objet [**InputScope**](/uwp/api/Windows.UI.Xaml.Input.InputScope) .

    ```csharp
    scope.Names.Add(scopeName);
    ```

6.  Définissez l’objet [**InputScope**](/uwp/api/Windows.UI.Xaml.Input.InputScope) comme valeur de la propriété [**InputScope**](/uwp/api/windows.ui.xaml.controls.textbox.inputscope) du contrôle de texte.

    ```csharp
    phoneNumberTextBox.InputScope = scope;
    ```

Voici le code dans son ensemble.

```CSharp
InputScope scope = new InputScope();
InputScopeName scopeName = new InputScopeName();
scopeName.NameValue = InputScopeNameValue.TelephoneNumber;
scope.Names.Add(scopeName);
phoneNumberTextBox.InputScope = scope;
```

Toutes ces étapes peuvent être regroupées dans ce code raccourci.

```CSharp
phoneNumberTextBox.InputScope = new InputScope() 
{
    Names = {new InputScopeName(InputScopeNameValue.TelephoneNumber)}
};
```

## <a name="text-prediction-spell-checking-and-auto-correction"></a>Prédiction de texte, vérification orthographique et correction automatique

Les contrôles [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) et [**RichEditBox**](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) ont plusieurs propriétés qui influencent le comportement du SIP. Pour fournir la meilleure expérience possible à vos utilisateurs, il est important de comprendre de quelle manière ces propriétés affectent la saisie tactile de texte.

-   [**IsSpellCheckEnabled**](/uwp/api/windows.ui.xaml.controls.textbox.isspellcheckenabled) : lorsque la vérification orthographique est activée pour un contrôle de texte, le contrôle interagit avec le moteur de vérification orthographique du système pour signaler les mots qui ne sont pas reconnus. Vous pouvez appuyer sur un mot pour afficher une liste de suggestions de corrections. La vérification orthographique est activée par défaut.

    Pour l’étendue d’entrée **Default** , cette propriété permet également la mise en majuscules automatique du premier mot dans une phrase, ainsi que la correction automatique des mots au fur et à mesure de la saisie. Ces fonctionnalités de correction automatique peuvent être désactivées dans les autres zones d’entrées. Pour en savoir plus, consultez les tableaux présentés plus loin dans cette rubrique.

-   [**IsTextPredictionEnabled**](/uwp/api/windows.ui.xaml.controls.textbox.istextpredictionenabled): lorsque la prédiction de texte est activée pour un contrôle de texte, le système affiche une liste de mots que vous pouvez commencer à taper. Vous pouvez sélectionner un élément dans la liste. Ainsi, vous n’avez pas besoin de saisir le mot entier. La prédiction de texte est activée par défaut.

    La prédiction de texte peut être désactivée si l’étendue des entrées est différente de **Default** , même si la propriété [**IsTextPredictionEnabled**](/uwp/api/windows.ui.xaml.controls.textbox.istextpredictionenabled) présente la valeur **true** . Pour en savoir plus, consultez les tableaux présentés plus loin dans cette rubrique.

-   [**PreventKeyboardDisplayOnProgrammaticFocus**](/uwp/api/windows.ui.xaml.controls.textbox.preventkeyboarddisplayonprogrammaticfocus) : définissez cette propriété sur **true** pour empêcher l’affichage du panneau de saisie quand le focus est défini par programme sur un contrôle de texte. Au lieu de cela, le clavier s’affiche uniquement lorsque l’utilisateur interagit avec le contrôle.

## <a name="touch-keyboard-index-for-windows"></a>Index du clavier tactile pour Windows

Ces tableaux affichent les dispositions du panneau de saisie (SIP) Windows pour les valeurs d’étendue d’entrée courantes. L’effet de l’étendue des entrées sur les fonctionnalités activées par les propriétés **IsSpellCheckEnabled** et **IsTextPredictionEnabled** est répertorié pour chaque étendue des entrées. Cette liste des étendues des entrées disponibles n’est pas complète.

> [!Tip] 
> Vous pouvez basculer la plupart des claviers tactiles entre une mise en page alphabétique et une mise en page de chiffres et de symboles en appuyant sur la touche **&123** pour passer à la disposition nombres et symboles, puis appuyer sur la touche **ABCD** pour passer à la mise en page alphabétique.

### <a name="default"></a>Default

`<TextBox InputScope="Default"/>`

Le clavier tactile Windows par défaut.

![Clavier tactile Windows par défaut](images/input-scopes/default.png)
- Vérification orthographique : activée si **IsSpellCheckEnabled**  =  **true** , désactivé si **IsSpellCheckEnabled**  =  **false**
- Correction automatique : activé si **IsSpellCheckEnabled**  =  **true** , désactivé si **IsSpellCheckEnabled**  =  **false**
- Mise en majuscule automatique : activé si **IsSpellCheckEnabled**  =  **true** , désactivé si **IsSpellCheckEnabled**  =  **false**
- Prédiction de texte : activée si **IsTextPredictionEnabled**  =  **true** , Disabled si **IsTextPredictionEnabled**  =  **false**

### <a name="currencyamountandsymbol"></a>CurrencyAmountAndSymbol

`<TextBox InputScope="CurrencyAmountAndSymbol"/>`

Disposition de clavier numérique et symbolique par défaut

![Clavier tactile Windows pour les devises](images/input-scopes/currencyamountandsymbol.png)

- Comprend des touches page gauche/droite pour afficher plus de symboles
- Vérification orthographique : activée par défaut ; peut être désactivée
- Vérification orthographique : activée par défaut ; peut être désactivée
- Mise en majuscules automatique : toujours désactivée
- Prédiction de texte : activée par défaut ; peut être désactivée
 
### <a name="url"></a>Url

`<TextBox InputScope="Url"/>`

![Clavier tactile Windows pour les URL](images/input-scopes/url.png)

- Inclut les touches **.com** et ![touche OK](images/input-scopes/kbdgokey.png) (OK). Appuyez sur la touche **. com** et maintenez-la enfoncée pour afficher des options supplémentaires ( **. org** , **.net** et des suffixes spécifiques à une région)
- Comprend les **clés :** , **-** et **/**
- Vérification orthographique : désactivée par défaut ; peut être activée
- Correction automatique : désactivée par défaut ; peut être activée
- Mise en majuscules automatique : désactivée par défaut ; peut être activée
- Prédiction de texte : désactivée par défaut ; peut être activée


### <a name="emailsmtpaddress"></a>EmailSmtpAddress

`<TextBox InputScope="EmailSmtpAddress"/>`

![Clavier tactile Windows pour les adresses de messagerie](images/input-scopes/emailsmtpaddress.png)
- Comprend les **@** clés et **. com** . Appuyez sur la touche **. com** et maintenez-la enfoncée pour afficher des options supplémentaires ( **. org** , **.net** et des suffixes spécifiques à une région)
- Comprend les **touches _** et **-**
- Vérification orthographique : désactivée par défaut ; peut être activée
- Correction automatique : désactivée par défaut ; peut être activée
- Mise en majuscules automatique : désactivée par défaut ; peut être activée
- Prédiction de texte : désactivée par défaut ; peut être activée


### <a name="number"></a>Number

`<TextBox InputScope="Number"/>`

![Clavier tactile Windows pour les nombres](images/input-scopes/number.png)
- Vérification orthographique : activée par défaut ; peut être désactivée
- Vérification orthographique : activée par défaut ; peut être désactivée
- Mise en majuscules automatique : toujours désactivée
- Prédiction de texte : activée par défaut ; peut être désactivée

### <a name="telephonenumber"></a>TelephoneNumber

`<TextBox InputScope="TelephoneNumber"/>`

![Clavier tactile Windows pour les numéros de téléphone](images/input-scopes/telephonenumber.png)
- Vérification orthographique : activée par défaut ; peut être désactivée
- Vérification orthographique : activée par défaut ; peut être désactivée
- Mise en majuscules automatique : toujours désactivée
- Prédiction de texte : activée par défaut ; peut être désactivée

### <a name="search"></a>Rechercher

`<TextBox InputScope="Search"/>`

![Clavier tactile Windows pour la recherche](images/input-scopes/search.png)
- Contient la clé de **recherche** au lieu de la touche **entrée**
- Vérification orthographique : activée par défaut ; peut être désactivée
- Vérification orthographique : activée par défaut ; peut être désactivée
- Mise en majuscules automatique : toujours désactivée
- Prédiction de texte : activée par défaut ; peut être désactivée

### <a name="searchincremental"></a>SearchIncremental

`<TextBox InputScope="SearchIncremental"/>`

![Clavier tactile Windows pour la recherche incrémentielle](images/input-scopes/searchincremental.png)
- Même disposition que la **valeur par défaut**
- Vérification orthographique : désactivée par défaut ; peut être activée
- Correction automatique : toujours désactivée
- Mise en majuscules automatique : toujours désactivée
- Prédiction de texte : toujours désactivée

### <a name="formula"></a>Formule

`<TextBox InputScope="Formula"/>`

![Clavier tactile Windows pour la formule](images/input-scopes/formula.png)
- Comprend la **=** clé
- Comprend également les **%** **$** clés, et **+**
- Vérification orthographique : activée par défaut ; peut être désactivée
- Vérification orthographique : activée par défaut ; peut être désactivée
- Mise en majuscules automatique : toujours désactivée
- Prédiction de texte : activée par défaut ; peut être désactivée

### <a name="chat"></a>Conversation

`<TextBox InputScope="Chat"/>`

![Clavier tactile Windows par défaut](images/input-scopes/default.png)
- Même disposition que la **valeur par défaut**
- Vérification orthographique : activée par défaut ; peut être désactivée
- Vérification orthographique : activée par défaut ; peut être désactivée
- Mise en majuscules automatique : activée par défaut ; peut être désactivée
- Prédiction de texte : activée par défaut ; peut être désactivée

### <a name="nameorphonenumber"></a>NameOrPhoneNumber

`<TextBox InputScope="NameOrPhoneNumber"/>`

![Clavier tactile Windows par défaut](images/input-scopes/default.png)
- Même disposition que la **valeur par défaut**
- Vérification orthographique : désactivée par défaut ; peut être activée
- Correction automatique : désactivée par défaut ; peut être activée
- Mise en majuscules automatique : désactivée par défaut, peut être activée (la première lettre de chaque mot est en majuscule)
- Prédiction de texte : désactivée par défaut ; peut être activée
