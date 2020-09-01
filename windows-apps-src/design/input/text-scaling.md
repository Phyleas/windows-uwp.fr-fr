---
Description: Générez des applications Windows et des contrôles personnalisés/basés sur des modèles qui prennent en charge la mise à l’échelle du texte de la plateforme.
title: Mise à l’échelle du texte
label: Text scaling
template: detail.hbs
keywords: UWP, texte, mise à l’échelle, accessibilité, « facilité d’accès », affichage, « rendre le texte plus grand », interaction utilisateur, entrée
ms.date: 08/02/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 386920532f4598ee2d1519d292454b47c285555b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165833"
---
# <a name="text-scaling"></a>Mise à l’échelle du texte

![Exemple de mise à l’échelle du texte 100% à 225%](images/coretext/text-scaling-news-hero-small.png)  
*Exemple de mise à l’échelle du texte dans Windows 10 (100% à 225%)*

## <a name="overview"></a>Vue d’ensemble

La lecture d’un texte sur un écran d’ordinateur (de l’appareil mobile à l’écran du moniteur de bureau à l’écran géant d’un Surface Hub) peut être difficile pour de nombreuses personnes. Inversement, certains utilisateurs recherchent les tailles de police utilisées dans les applications et les sites Web pour être plus volumineuses que nécessaire.

Pour garantir que le texte est le plus lisible possible pour le plus grand nombre d’utilisateurs, Windows offre aux utilisateurs la possibilité de modifier la taille de police relative sur le système d’exploitation et les applications individuelles. Au lieu d'utiliser une loupe (qui se contente généralement d'agrandir tout ce qui se trouve dans une zone de l'écran et introduit ses propres problèmes d'utilisation), de modifier la résolution d'affichage ou de se fier à la mise à l'échelle PPP (qui redimensionne tout en fonction de l'affichage et de la distance de visualisation standard), un utilisateur peut rapidement accéder à un paramètre permettant de redimensionner uniquement le texte, dans une fourchette comprise entre 100 % (taille par défaut) et 225 %.

## <a name="support"></a>Support

Les applications Windows universelles (à la fois standard et PWA) prennent en charge par défaut la mise à l’échelle du texte.

Si votre application Windows comprend des contrôles personnalisés, des surfaces de texte personnalisées, des hauteurs de contrôle codés en dur, des frameworks plus anciens ou des frameworks tiers, vous devrez probablement effectuer des mises à jour pour garantir une expérience cohérente et utile à vos utilisateurs.  

DirectWrite, GDI et XAML SwapChainPanels ne prennent pas en charge la mise à l’échelle du texte en mode natif, tandis que la prise en charge de Win32 est limitée aux menus, aux icônes et aux barres d’outils.  

<!-- If you want to support text scaling in your application with these frameworks, you’ll need to support the text scaling change event outlined below and provide alternative sizes for your UI and content.   -->

## <a name="user-experience"></a>Expérience utilisateur

Les utilisateurs peuvent ajuster la mise à l’échelle du texte à l’aide du curseur augmenter la taille du texte sur les paramètres-> faciliter l’accès-> écran visuel/affichage.

![Exemple de mise à l’échelle du texte 100% à 225%](images/coretext/text-scaling-settings-100-small.png)  
*Paramètre de mise à l’échelle du texte à partir des paramètres-> facilité d’accès-> écran vision/affichage*

## <a name="ux-guidance"></a>Recommandations en matière d’expérience utilisateur

Au fur et à mesure que du texte est redimensionné, les contrôles et les conteneurs doivent également être redimensionnés et redistribués pour s’adapter au texte et à la nouvelle disposition. Comme mentionné précédemment, en fonction de l’application, de l’infrastructure et de la plateforme, une grande partie de ce travail est effectuée pour vous. L’aide de l’expérience utilisateur suivante couvre les cas où elle ne l’est pas.

### <a name="use-the-platform-controls"></a>Utiliser les contrôles de plateforme

Avons-nous déjà dit cela ? Il convient de répéter : dans la mesure du possible, utilisez toujours les contrôles intégrés fournis avec les différentes infrastructures d’application Windows pour obtenir l’expérience utilisateur la plus complète possible pour un minimum d’effort.

Par exemple, tous les contrôles de texte UWP prennent en charge l’expérience de mise à l’échelle du texte complète sans personnalisation ni création de modèles.

Voici un extrait de code d’une application UWP de base qui comprend deux contrôles de texte standard :

``` xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    <TextBlock Grid.Row="0" 
                Style="{ThemeResource TitleTextBlockStyle}"
                Text="Text scaling test" 
                HorizontalTextAlignment="Center" />
    <Grid Grid.Row="1">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto"/>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="Auto"/>
        </Grid.ColumnDefinitions>
        <Image Grid.Column="0" 
                Source="Assets/StoreLogo.png" 
                Width="450" Height="450"/>
        <StackPanel Grid.Column="1" 
                    HorizontalAlignment="Center">
            <TextBlock TextWrapping="WrapWholeWords">
                The quick brown fox jumped over the lazy dog.
            </TextBlock>
            <TextBox PlaceholderText="Type something here" />
        </StackPanel>
        <Image Grid.Column="2" 
                Source="Assets/StoreLogo.png" 
                Width="450" Height="450"/>
    </Grid>
    <TextBlock Grid.Row="2" 
                Style="{ThemeResource TitleTextBlockStyle}"
                Text="Text scaling test footer" 
                HorizontalTextAlignment="Center" />
</Grid>
```

![Mise à l’échelle du texte animé 100% à 225%](images/coretext/text-scaling.gif)  
*Mise à l’échelle du texte animé*

### <a name="use-auto-sizing"></a>Utiliser le dimensionnement automatique

Ne spécifiez pas de tailles absolues pour vos contrôles. Dans la mesure du possible, laissez la plateforme redimensionner automatiquement vos contrôles en fonction des paramètres de l’utilisateur et de l’appareil.  

Dans cet extrait de code de l’exemple précédent, nous utilisons les `Auto` `*` valeurs et Width d’un ensemble de colonnes de grille et laissons la plateforme ajuster la disposition de l’application en fonction de la taille des éléments contenus dans la grille.

``` xaml
<Grid.ColumnDefinitions>
    <ColumnDefinition Width="Auto"/>
    <ColumnDefinition Width="*"/>
    <ColumnDefinition Width="Auto"/>
</Grid.ColumnDefinitions>
```

### <a name="use-text-wrapping"></a>Utiliser l’habillage du texte

Pour garantir que la disposition de votre application est aussi flexible et adaptable que possible, activez l’habillage du texte dans n’importe quel contrôle qui contient du texte (de nombreux contrôles ne prennent pas en charge l’habillage du texte par défaut).

Si vous ne spécifiez pas l’habillage du texte, la plateforme utilise d’autres méthodes pour ajuster la disposition, y compris le découpage (Voir l’exemple précédent).

Ici, nous utilisons les `AcceptsReturn` `TextWrapping` Propriétés de zone de texte et pour nous assurer que notre disposition est la plus flexible possible.

``` xaml
<TextBox PlaceholderText="Type something here" 
          AcceptsReturn="True" TextWrapping="Wrap" />
```

![Mise à l’échelle du texte animé 100% à 225% avec habillage du texte](images/coretext/text-scaling-textwrap.gif)  
*Mise à l’échelle du texte animé avec habillage du texte*

### <a name="specify-text-trimming-behavior"></a>Spécifier le comportement de suppression de texte

Si l’habillage du texte n’est pas le comportement par défaut, la plupart des contrôles de texte vous permettent d’ajuster votre texte ou de spécifier des ellipses pour le comportement de suppression de texte. Le découpage est préférable aux ellipses, car les ellipses occupent de l’espace.

> [!NOTE]
> Si vous devez découper votre texte, découpez la fin de la chaîne, pas le début.

Dans cet exemple, nous montrons comment découper du texte dans un TextBlock à l’aide de la propriété [TextTrimming](/uwp/api/windows.ui.xaml.controls.textblock.texttrimming) .

``` xaml
<TextBlock TextTrimming="Clip">
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

![Mise à l’échelle du texte de 100% à 225% avec découpage du texte](images/coretext/text-scaling-clipping-small.png)  
*Mise à l’échelle du texte avec le découpage de texte*

### <a name="use-a-tooltip"></a>Utiliser une info-bulle

Si vous découpez du texte, utilisez une info-bulle pour fournir le texte complet à vos utilisateurs.

Ici, nous ajoutons une info-bulle à un TextBlock qui ne prend pas en charge l’habillage du texte :

``` xaml
<TextBlock TextTrimming="Clip">
    <ToolTipService.ToolTip>
        <ToolTip Content="The quick brown fox jumped over the lazy dog."/>
    </ToolTipService.ToolTip>
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

### <a name="dont-scale-font-based-icons-or-symbols"></a>Ne pas mettre à l’échelle les symboles ou les icônes basés sur des polices

Lorsque vous utilisez des icônes de police pour l’accentuation ou la décoration, désactivez la mise à l’échelle sur ces caractères.

Affectez à la propriété [IsTextScaleFactorEnabled](/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled) la valeur `false` pour la plupart des contrôles XAML.

### <a name="support-text-scaling-natively"></a>Prendre en charge la mise à l’échelle du texte en mode natif

Gérez l’événement système [TextScaleFactorChanged](/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged) uisettings dans votre infrastructure et vos contrôles personnalisés. Cet événement est déclenché chaque fois que l’utilisateur définit le facteur d’échelle du texte sur son système.

## <a name="summary"></a>Résumé

Cette rubrique fournit une vue d’ensemble de la prise en charge de la mise à l’échelle du texte dans Windows et inclut l’expérience utilisateur et l’aide pour les développeurs sur la personnalisation de l’expérience utilisateur.

## <a name="related-articles"></a>Articles connexes

### <a name="api-reference"></a>Informations de référence sur les API

- [IsTextScaleFactorEnabled](/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled)
- [TextScaleFactorChanged](/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged)