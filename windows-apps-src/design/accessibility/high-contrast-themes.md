---
description: Décrit les étapes nécessaires pour garantir que votre application d’application Windows est utilisable quand un thème à contraste élevé est actif.
ms.assetid: FD7CA6F6-A8F1-47D8-AA6C-3F2EC3168C45
title: Thèmes à contraste élevé
template: detail.hbs
ms.date: 09/28/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 118f604b8c8c95a863773270825ff4db5c5a1b3a
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82969454"
---
# <a name="high-contrast-themes"></a>Thèmes à contraste élevé  

Windows prend en charge les thèmes à contraste élevé pour les systèmes d’exploitation et les applications que les utilisateurs peuvent être amenés à activer. Les thèmes à contraste élevé utilisent une petite palette de couleurs contrastées qui facilitent l’affichage de l’interface.

![Calculatrice représentée en thème clair et en thème noir à contraste élevé](images/high-contrast-calculators.png)

*Calculatrice représentée en thème clair et en thème noir à contraste élevé*

Pour basculer sur un thème à contraste élevé, accédez à *Paramètres &gt; Options d’ergonomie &gt; Contraste élevé*.

> [!NOTE]
> Ne confondez pas les thèmes à contraste élevé avec les thèmes Ombre et lumière, qui prennent en charge une palette de couleurs plus large, sans contraste élevé. Pour plus d’informations sur les thèmes Ombre et lumière, consultez l’article sur la [couleur](../style/color.md).

Les contrôles communs sont pourvus d’une prise en charge gratuite complète du contraste élevé, mais nous vous recommandons de faire preuve de prudence lors de la personnalisation de votre interface utilisateur. Le bogue le plus courant lié au contraste élevé est provoqué par le codage en dur d’une couleur sur un contrôle inclus.

```xaml
<!-- Don't do this! -->
<Grid Background="#E6E6E6">

<!-- Instead, create BrandedPageBackgroundBrush and do this. -->
<Grid Background="{ThemeResource BrandedPageBackgroundBrush}">
```

Lorsque la couleur `#E6E6E6` est incluse dans le premier exemple, la Grille conserve la couleur d’arrière-plan dans l’ensemble des thèmes. Si l’utilisateur bascule sur le thème noir à contraste élevé, il souhaitera disposer d’un arrière-plan noir sur votre application. Dans la mesure où `#E6E6E6` est presque blanc, certains utilisateurs peuvent ne pas pouvoir interagir avec votre application.

Dans le second exemple, l’[**extension de balisage {ThemeResource}**](../../xaml-platform/themeresource-markup-extension.md) est utilisée pour référencer une couleur dans la collection [**ThemeDictionaries**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries), une propriété dédiée d’un élément [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary). **ThemeDictionaries** permet à XAML de permuter automatiquement les couleurs en fonction du thème actuel de l’utilisateur.

## <a name="theme-dictionaries"></a>Dictionnaires de thème

Lorsque vous devez modifier la couleur par défaut du système, créez une collection ThemeDictionaries pour votre application.

1. Commencez par créer le système approprié, le cas échéant. Dans App. xaml, créez une collection **ThemeDictionaries** , y compris la **valeur par défaut** et le **contraste** minimal.
2. Dans **default**, créez le type de [pinceau](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Brush) dont vous avez besoin, généralement un **SolidColorBrush**. Donnez-lui un nom de *x :Key* spécifique à ce qui est utilisé pour.
3. Affectez la **couleur** de votre choix.
4. Copiez ce **pinceau** dans le **contraste HighContrast**.

``` xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called
            out below -->
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>

            <!-- Optional, Light is used in light theme.
            If included, Default will be used for Dark theme -->
            <ResourceDictionary x:Key="Light">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>

            <!-- HighContrast is used in all high contrast themes -->
            <ResourceDictionary x:Key="HighContrast">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

La dernière étape, décrite dans la section suivante, consiste à identifier la couleur à utiliser dans le contraste élevé.

> [!NOTE]
> **HighContrast** n’est pas le seul nom de clé disponible. Il y a également **HighContrastBlack**, **HighContrastWhite**et **HighContrastCustom**. Dans la plupart des cas, le **contraste** élevé est tout ce dont vous avez besoin.

## <a name="high-contrast-colors"></a>Couleurs à contraste élevé

La page *Paramètres &gt; Options d’ergonomie &gt; Contraste élevé* comporte 4 thèmes à contraste élevé par défaut. 


![Paramètres de contraste élevé](images/high-contrast-settings.png)  

*Lorsque l’utilisateur sélectionne une option, la page affiche un aperçu.*  

![Ressources de contraste élevé](images/high-contrast-resources.png)  

*Vous pouvez cliquer sur chaque échantillon de couleur de l’Aperçu pour modifier sa valeur. Chaque nuance est également directement mappée à une ressource de couleur XAML.*  

Chaque ressource de **couleur SystemColor *** est une variable qui met à jour automatiquement la couleur lorsque l’utilisateur bascule les thèmes à contraste élevé. Vous trouverez ci-après des recommandations vous indiquant quand et où utiliser chaque ressource.

Ressource | Utilisation |
|--------|-------|
**SystemColorWindowTextColor** | Copie de texte de corps, titres, listes ; tout texte ne pouvant faire l’objet d’aucune interaction |
| **SystemColorHotlightColor** | Liens hypertexte |
| **SystemColorGrayTextColor** | Interface utilisateur désactivée |
| **SystemColorHighlightTextColor** | Couleur de premier plan pour le texte ou l’interface utilisateur activés, sélectionnés ou faisant l’objet d’une interaction |
| **SystemColorHighlightColor** | Couleur d’arrière-plan pour le texte ou l’interface utilisateur activés, sélectionnés ou faisant l’objet d’une interaction |
| **SystemColorButtonTextColor** | Couleur de premier plan des boutons ; toute interface utilisateur ne pouvant faire l’objet d’aucune interaction |
| **SystemColorButtonFaceColor** | Couleur d’arrière-plan des boutons ; toute interface utilisateur ne pouvant faire l’objet d’aucune interaction |
| **SystemColorWindowColor** | Arrière-plan des pages, des volets, des fenêtres contextuelles et des barres |

Il est souvent utile d’examiner les applications, menus Démarrer ou contrôles communs existants afin de savoir comment d’autres utilisateurs ont résolu des problèmes de conception de contraste élevé similaire au vôtre.

**À faire**

* Respectez les paires premier plan/arrière-plan autant que possible.
* Testez l’ensemble des 4 thèmes à contraste élevé pendant l’exécution de votre application. Lorsqu’il change de thème, l’utilisateur ne devrait pas avoir à redémarrer votre application.
* Soyez cohérent.

**À ne pas faire**

* Coder en dur une couleur dans le thème **HighContrast** ; Utilisez les ressources de **couleur SystemColor *** .
* Choisissez une ressource de couleur à des fins esthétiques. N’oubliez pas que la couleur change avec le thème !
* N’utilisez pas **SystemColorGrayTextColor** pour la copie de corps qui est secondaire ou qui agit comme un indicateur.


Pour continuer l’exemple précédent, vous devez choisir une ressource pour **BrandedPageBackgroundBrush**. Étant donné que le nom indique qu’il sera utilisé pour un arrière-plan, **SystemColorWindowColor** est un bon choix.

``` xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called
            out below -->
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>

            <!-- Optional, Light is used in light theme.
            If included, Default will be used for Dark theme -->
            <ResourceDictionary x:Key="Light">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>

            <!-- HighContrast is used in all high contrast themes -->
            <ResourceDictionary x:Key="HighContrast">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="{ThemeResource SystemColorWindowColor}" />
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

Plus loin dans votre application,vous pouvez désormais définir l’arrière-plan.

```xaml
<Grid Background="{ThemeResource BrandedPageBackgroundBrush}">
```

Notez la manière dont ** \{\} ThemeResource** est utilisé deux fois, une fois pour référencer **SystemColorWindowColor** et à nouveau pour référencer **BrandedPageBackgroundBrush**. Les deux instances sont nécessaires pour la bonne définition du thème lors de l’exécution. Il s’agit d’un bon moment pour tester la fonctionnalité dans votre application. L’arrière-plan de la grille est automatiquement mis à jour lorsque vous définissez un thème à contraste élevé. Il est également mis à jour lorsque vous basculez entre deux thèmes à contraste élevé différents.

## <a name="when-to-use-borders"></a>Quand utiliser les bordures

Les pages, les volets, les menus contextuels et les barres doivent tous utiliser **SystemColorWindowColor** pour l’arrière-plan du contraste élevé. Si nécessaire, ajoutez une bordure à contraste élevé uniquement afin de préserver les bordures importantes au sein de votre interface utilisateur.

![Volet de navigation séparé du reste de la page](images/high-contrast-actions-content.png)  

*Le volet de navigation et la page partagent la même couleur d’arrière-plan dans un contraste élevé. Une bordure à contraste élevé uniquement pour les diviser est essentielle.*


## <a name="list-items"></a>Éléments de liste

Dans un contraste élevé, les éléments d’un [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) ont leur arrière-plan défini sur **SystemColorHighlightColor** lorsqu’ils sont survolés, appuyés ou sélectionnés. Les éléments de liste complexes présentent régulièrement un bogue, durant lequel la couleur du contenu de l’élément de liste ne peut pas être inversée lorsque ce dernier est survolé, activé ou sélectionné. Cela rend impossible sa lecture.

![Liste simple en thème clair et en thème noir à contraste élevé](images/high-contrast-list1.png)

*Liste simple dans le thème clair (à gauche) et contraste élevé thème noir (à droite). Le deuxième élément est sélectionné. Notez comment la couleur de texte est inversée en contraste élevé.*


### <a name="list-items-with-colored-text"></a>Éléments de liste avec du texte coloré

Un coupable définit TextBlock.Foreground dans l’instance [DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate).de ListView. Cette procédure est généralement effectuée dans le cadre de l’établissement de la hiérarchie visuelle. La propriété Foreground est définie sur [ListViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewitem), tandis que TextBlocks dans DataTemplate hérite de la couleur appropriée d’arrière-plan lorsque l’élément est survolé, activé ou sélectionné. Toutefois, la définition de Foreground rompt l’héritage.

![Liste complexe en thème clair et en thème noir à contraste élevé](images/high-contrast-list2.png)

*Liste complexe dans le thème clair (à gauche) et contraste élevé thème noir (à droite). Dans un contraste élevé, la deuxième ligne de l’élément sélectionné n’a pas pu être inversée.*  

Vous pouvez contourner ce problème en définissant le premier plan de façon conditionnelle via un style qui se trouve dans une collection **ThemeDictionaries** . Étant donné que le **premier plan** n’est pas défini par **SecondaryBodyTextBlockStyle** dans **HighContrast**, sa couleur est correctement inversée.

```xaml
<!-- In App.xaml... -->
<ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Default">
        <Style
            x:Key="SecondaryBodyTextBlockStyle"
            TargetType="TextBlock"
            BasedOn="{StaticResource BodyTextBlockStyle}">
            <Setter Property="Foreground" Value="{StaticResource SystemControlForegroundBaseMediumBrush}" />
        </Style>
    </ResourceDictionary>

    <ResourceDictionary x:Key="Light">
        <Style
            x:Key="SecondaryBodyTextBlockStyle"
            TargetType="TextBlock"
            BasedOn="{StaticResource BodyTextBlockStyle}">
            <Setter Property="Foreground" Value="{StaticResource SystemControlForegroundBaseMediumBrush}" />
        </Style>
    </ResourceDictionary>

    <ResourceDictionary x:Key="HighContrast">
        <!-- The Foreground Setter is omitted in HighContrast -->
        <Style
            x:Key="SecondaryBodyTextBlockStyle"
            TargetType="TextBlock"
            BasedOn="{StaticResource BodyTextBlockStyle}" />
    </ResourceDictionary>
</ResourceDictionary.ThemeDictionaries>

<!-- Usage in your DataTemplate... -->
<DataTemplate>
    <StackPanel>
        <TextBlock Style="{StaticResource BodyTextBlockStyle}" Text="Double line list item" />

        <!-- Note how ThemeResource is used to reference the Style -->
        <TextBlock Style="{ThemeResource SecondaryBodyTextBlockStyle}" Text="Second line of text" />
    </StackPanel>
</DataTemplate>
```


## <a name="detecting-high-contrast"></a>Détection du contraste élevé

Vous pouvez vérifier par programmation si le thème actuel est un thème à contraste élevé à l’aide des membres de la classe [**AccessibilitySettings**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.AccessibilitySettings) .

> [!NOTE]
> Prenez soin d’appeler le constructeur **AccessibilitySettings** à partir d’une étendue dans laquelle l’application est initialisée et affiche déjà du contenu.

## <a name="related-topics"></a>Rubriques connexes  
* [Accessibilité](accessibility.md)
* [Exemple de paramètres et de contraste d’interface utilisateur](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20high%20contrast%20style%20sample%20(Windows%208))
* [Exemple d’accessibilité XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample)
* [Exemple de contraste élevé XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20high%20contrast%20style%20sample%20(Windows%208))
* [**AccessibilitySettings**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.AccessibilitySettings)
