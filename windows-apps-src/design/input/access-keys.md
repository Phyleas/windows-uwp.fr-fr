---
Description: Apprenez à améliorer à la fois la convivialité et l’accessibilité de votre application Windows en fournissant aux utilisateurs un moyen intuitif de naviguer et d’interagir rapidement avec l’interface utilisateur visible d’une application par le biais d’un clavier au lieu d’un pointeur (par exemple, une pression tactile ou une souris).
title: Recommandations en matière de conception de touches d’accès rapide
label: Access keys design guidelines
keywords: clavier, touche d’accès, KeyTip, touche accélératrice, accessibilité, navigation, Focus, texte, entrée, interaction utilisateur
template: detail.hbs
ms.date: 06/08/2018
ms.topic: article
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: ca8c21729f27e30e7703291c04a940301a3feb26
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173893"
---
# <a name="access-keys"></a>Clés d'accès

Les touches d’accès rapide sont des raccourcis clavier qui améliorent la convivialité et l’accessibilité de vos applications Windows en fournissant aux utilisateurs un moyen intuitif de naviguer et d’interagir rapidement avec l’interface utilisateur visible d’une application par le biais d’un clavier au lieu d’un pointeur (par exemple, une pression tactile ou une souris).

Pour plus d’informations sur l’appel des actions courantes dans une application Windows à l’aide de raccourcis clavier, consultez la rubrique [touches accélérateur](keyboard-accelerators.md) . 

> [!NOTE]
> Un clavier est indispensable pour les utilisateurs présentant des handicaps (voir [accessibilité du clavier](../accessibility/keyboard-accessibility.md)). il est également un outil important pour les utilisateurs qui préfèrent le faire comme un moyen plus efficace d’interagir avec une application.

L’application Windows fournit une prise en charge intégrée des contrôles de plateforme pour les touches d’accès clavier et les commentaires d’interface utilisateur associés via des signaux visuels appelés touches accélératrices.

## <a name="overview"></a>Vue d’ensemble

Une clé d’accès est une combinaison de la touche Alt et d’une ou plusieurs clés alphanumériques, parfois appelées « *mnémoniques*», généralement appuyées séquentiellement, plutôt que simultanément.

Les touches accélératrices sont des badges qui s’affichent en regard des contrôles qui prennent en charge les touches d’accès quand l’utilisateur appuie sur la touche Alt. Chaque Conseil clé contient les touches alphanumériques qui activent le contrôle associé.

> [!NOTE]
> Les raccourcis clavier sont automatiquement pris en charge pour les touches d’accès en utilisant un seul caractère alphanumérique. Par exemple, en appuyant simultanément sur ALT + F dans Word, vous ouvrez le menu fichier sans afficher les touches accélératrices.

Appuyez sur la touche Alt pour initialiser la fonctionnalité de clé d’accès et afficher toutes les combinaisons de touches actuellement disponibles dans les touches accélératrices. Les séquences de touches suivantes sont gérées par l’infrastructure de clé d’accès, qui rejette les clés non valides jusqu’à ce qu’une clé d’accès valide soit enfoncée, ou que les touches entrée, ÉCHAP, Tab ou flèche soient enfoncées pour désactiver les touches d’accès et retourner la gestion des séquences de touches à l’application.

Les applications Microsoft Office fournissent une prise en charge étendue des clés d’accès. L’illustration suivante montre l’onglet de démarrage de Word avec les touches d’accès activées (Notez la prise en charge des nombres et des séquences de touches multiples).

![Badges de touches accélératrices pour les touches d’accès dans Microsoft Word](images/accesskeys/keytip-badges-word.png)

_Badges de KeyTip pour les touches d’accès dans Microsoft Word_

Pour ajouter une clé d’accès à un contrôle, utilisez la **propriété AccessKey**. La valeur de cette propriété spécifie la séquence de touches d’accès rapide, le raccourci (s’il s’agit d’un caractère alphanumérique unique) et l’info-bulle.

``` xaml
<Button Content="Accept" AccessKey="A" Click="AcceptButtonClick" />
```

## <a name="when-to-use-access-keys"></a>Quand utiliser des clés d’accès

Nous vous recommandons de spécifier des clés d’accès chaque fois que cela est approprié dans votre interface utilisateur, et de prendre en charge les clés d’accès dans tous les contrôles personnalisés.

1.  Les **touches d’accès rendent votre application plus accessible** aux utilisateurs ayant un handicap moteur, y compris les utilisateurs qui ne peuvent appuyer que sur une seule touche à la fois ou qui ont des difficultés à utiliser une souris.

    Une interface utilisateur de clavier bien conçue représente un aspect important de l’accessibilité logicielle. Elle permet aux utilisateurs malvoyants ou souffrant d’un handicap moteur de naviguer dans une application et d’interagir avec ses fonctionnalités. Les utilisateurs qui ne sont pas en mesure d’utiliser une souris peuvent avoir recours à diverses technologies d’assistance, telles que les outils de clavier amélioré, les claviers visuels, les écrans élargis, les lecteurs d’écran et les utilitaires d’entrée vocale. Pour ces utilisateurs, une couverture complète des commandes est cruciale.

2.  Les **clés d’accès rendent votre application plus utilisable** pour les utilisateurs avec pouvoir qui préfèrent interagir via le clavier.

    Les utilisateurs expérimentés disposent souvent d’une préférence pour utiliser le clavier, car les commandes basées sur le clavier peuvent être entrées plus rapidement et ne nécessitent pas la suppression des mains du clavier. Pour ces utilisateurs, l’efficacité et la cohérence sont essentielles. L’exhaustivité n’est importante que pour les commandes les plus fréquemment utilisées.

## <a name="set-access-key-scope"></a>Définir l’étendue de la clé d’accès

Quand un grand nombre d’éléments à l’écran prennent en charge des clés d’accès, nous vous recommandons d’étendre les clés d’accès afin de réduire la **charge cognitive**. Cela réduit le nombre de touches d’accès rapide à l’écran, ce qui les rend plus faciles à localiser et améliore l’efficacité et la productivité.

Par exemple, Microsoft Word fournit deux étendues de clé d’accès : une étendue principale pour les onglets du ruban et une étendue secondaire pour les commandes de l’onglet sélectionné.

Les images suivantes illustrent les deux étendues de clé d’accès dans Word. Le premier affiche les clés d’accès principales qui permettent à un utilisateur de sélectionner un onglet et d’autres commandes de niveau supérieur, tandis que le deuxième affiche les clés d’accès secondaires pour l’onglet dossier de base.

![Clés d’accès principales dans les ](images/accesskeys/primary-access-keys-word.png)
 _clés d’accès principal Microsoft Word dans Microsoft Word_

![Clés d’accès secondaires dans les ](images/accesskeys/secondary-access-keys-word.png)
 _clés d’accès secondaires Microsoft Word dans Microsoft Word_

Les clés d’accès peuvent être dupliquées pour des éléments dans différentes étendues. Dans l’exemple précédent, « 2 » est la clé d’accès pour l’annulation dans l’étendue principale et également « Italic » dans l’étendue secondaire.

Ici, nous montrons comment définir une étendue de clé d’accès.

``` xaml
<CommandBar x:Name="MainCommandBar" AccessKey="M" >
    <AppBarButton AccessKey="G" Icon="Globe" Label="Go"/>
    <AppBarButton AccessKey="S" Icon="Stop" Label="Stop"/>
    <AppBarSeparator/>
    <AppBarButton AccessKey="R" Icon="Refresh" Label="Refresh" IsAccessKeyScope="True">
        <AppBarButton.Flyout>
            <MenuFlyout>
                <MenuFlyoutItem AccessKey="A" Icon="Globe" Text="Refresh A" />
                <MenuFlyoutItem AccessKey="B" Icon="Globe" Text="Refresh B" />
                <MenuFlyoutItem AccessKey="C" Icon="Globe" Text="Refresh C" />
                <MenuFlyoutItem AccessKey="D" Icon="Globe" Text="Refresh D" />
            </MenuFlyout>
        </AppBarButton.Flyout>
    </AppBarButton>
    <AppBarButton AccessKey="B" Icon="Back" Label="Back"/>
    <AppBarButton AccessKey="F" Icon="Forward" Label="Forward"/>
    <AppBarSeparator/>
    <AppBarToggleButton AccessKey="V" Icon="Favorite" Label="Favorite"/>
    <CommandBar.SecondaryCommands>
        <AppBarToggleButton Icon="Like" AccessKey="L" Label="Like"/>
        <AppBarButton Icon="Setting" AccessKey="T" Label="Settings" />
    </CommandBar.SecondaryCommands>
</CommandBar>
```

![Clés d’accès principales pour CommandBar](images/accesskeys/primary-access-keys-commandbar.png)

_Étendue principale CommandBar et clés d’accès prises en charge_

![Clés d’accès secondaires pour CommandBar](images/accesskeys/secondary-access-keys-commandbar.png)

_Portée secondaire CommandBar et clés d’accès prises en charge_

### <a name="windows-10-creators-update-and-older"></a>Windows 10 Creators Update et versions antérieures

Avant la mise à jour des créateurs de Windows 10, certains contrôles, tels que CommandBar, ne prenaient pas en charge les étendues de clé d’accès intégrées.

L’exemple suivant montre comment prendre en charge CommandBar SecondaryCommands avec des clés d’accès, qui sont disponibles une fois qu’une commande parente est appelée (semblable au ruban dans Word).

```xaml
<local:CommandBarHack x:Name="MainCommandBar" AccessKey="M" >
    <AppBarButton AccessKey="G" Icon="Globe" Label="Go"/>
    <AppBarButton AccessKey="S" Icon="Stop" Label="Stop"/>
    <AppBarSeparator/>
    <AppBarButton AccessKey="R" Icon="Refresh" Label="Refresh" IsAccessKeyScope="True">
        <AppBarButton.Flyout>
            <MenuFlyout>
                <MenuFlyoutItem AccessKey="A" Icon="Globe" Text="Refresh A" />
                <MenuFlyoutItem AccessKey="B" Icon="Globe" Text="Refresh B" />
                <MenuFlyoutItem AccessKey="C" Icon="Globe" Text="Refresh C" />
                <MenuFlyoutItem AccessKey="D" Icon="Globe" Text="Refresh D" />
            </MenuFlyout>
        </AppBarButton.Flyout>
    </AppBarButton>
    <AppBarButton AccessKey="B" Icon="Back" Label="Back"/>
    <AppBarButton AccessKey="F" Icon="Forward" Label="Forward"/>
    <AppBarSeparator/>
    <AppBarToggleButton AccessKey="V" Icon="Favorite" Label="Favorite"/>
    <CommandBar.SecondaryCommands>
        <AppBarToggleButton Icon="Like" AccessKey="L" Label="Like"/>
        <AppBarButton Icon="Setting" AccessKey="T" Label="Settings" />
    </CommandBar.SecondaryCommands>
</local:CommandBarHack>
```

```csharp
public class CommandBarHack : CommandBar
{
    CommandBarOverflowPresenter secondaryItemsControl;
    Popup overflowPopup;

    public CommandBarHack()
    {
        this.ExitDisplayModeOnAccessKeyInvoked = false;
        AccessKeyInvoked += OnAccessKeyInvoked;
    }

    protected override void OnApplyTemplate()
    {
        base.OnApplyTemplate();

        Button moreButton = GetTemplateChild("MoreButton") as Button;
        moreButton.SetValue(Control.IsTemplateKeyTipTargetProperty, true);
        moreButton.IsAccessKeyScope = true;

        // SecondaryItemsControl changes
        secondaryItemsControl = GetTemplateChild("SecondaryItemsControl") as CommandBarOverflowPresenter;
        secondaryItemsControl.AccessKeyScopeOwner = moreButton;

        overflowPopup = GetTemplateChild("OverflowPopup") as Popup;
    }

    private void OnAccessKeyInvoked(UIElement sender, AccessKeyInvokedEventArgs args)
    {
        if (overflowPopup != null)
        {
            overflowPopup.Opened += SecondaryMenuOpened;
        }
    }

    private void SecondaryMenuOpened(object sender, object e)
    {
        //This is not neccesay given we are automatically pushing the scope.
        var item = secondaryItemsControl.Items.First();
        if (item != null && item is Control)
        {
            (item as Control).Focus(FocusState.Keyboard);
        }
        overflowPopup.Opened -= SecondaryMenuOpened;
    }
}
```

## <a name="avoid-access-key-collisions"></a>Éviter les collisions de clés d’accès

Les collisions de clés d’accès se produisent lorsque deux ou plusieurs éléments de la même étendue ont des clés d’accès en double ou commencent par les mêmes caractères alphanumériques.

Le système résout les clés d’accès en double en traitant la clé d’accès du premier élément ajouté à l’arborescence d’éléments visuels, en ignorant tous les autres.

Lorsque plusieurs clés d’accès commencent par le même caractère (par exemple, « A », « a1 » et « AB »), le système traite la clé d’accès à un seul caractère et ignore toutes les autres.

Évitez les collisions à l’aide de clés d’accès uniques ou de commandes d’étendue.

## <a name="choose-access-keys"></a>Choisir les clés d’accès

Tenez compte des éléments suivants lorsque vous choisissez des clés d’accès :

-   Utiliser un caractère unique pour réduire les frappes de touches et prendre en charge les touches accélérateur par défaut (Alt + AccessKey)
-   Évitez d’utiliser plus de deux caractères
-   Éviter les collisions de clés d’accès
-   Évitez les caractères difficiles à distinguer d’autres caractères, tels que la lettre « I » et le chiffre « 1 » ou la lettre « O » et le nombre « 0 »
-   Utilisez des antécédents bien connus d’autres applications populaires telles que Word (« F » pour « fichier », « H » pour « démarrage », etc.)
-   Utilisez le premier caractère du nom de la commande, ou un caractère avec une association étroite avec la commande qui vous aide à rappeler
    -   Si la première lettre est déjà assignée, utilisez une lettre aussi proche que possible de la première lettre du nom de commande (« N » pour l’insertion)
    -   Utiliser une consonne distinctive à partir du nom de commande (« W » pour la vue)
    -   Utilisez une voyelle à partir du nom de la commande.

## <a name="localize-access-keys"></a>Localiser les clés d’accès

Si votre application va être localisée dans plusieurs langues, vous devez également envisager de **localiser les clés d’accès**. Par exemple, pour « H » pour « orig » dans en-US et « I » pour « Incio » dans es-ES.

Utilisez l’extension x :Uid dans le balisage pour appliquer les ressources localisées comme indiqué ici :

``` xaml
<Button Content="Home" AccessKey="H" x:Uid="HomeButton" />
```
Les ressources pour chaque langue sont ajoutées aux dossiers de chaînes correspondants dans le projet :

![Dossiers de chaînes de ressources en anglais et espagnol](images/accesskeys/resource-string-folders.png)

_Dossiers de chaînes de ressources en anglais et espagnol_

Les clés d’accès localisées sont spécifiées dans le fichier Resources. resw de vos projets :

![Spécifiez la propriété AccessKey spécifiée dans le fichier Resources. resw](images/accesskeys/resource-resw-file.png)

_Spécifiez la propriété AccessKey spécifiée dans le fichier Resources. resw_

Pour plus d’informations, consultez [traduction des ressources de l’interface utilisateur ](/previous-versions/windows/apps/hh965329(v=win.10))

## <a name="key-tip-positioning"></a>Positionnement des touches accélératrices

Les touches accélératrices sont affichées sous forme de badges flottants par rapport à leur élément d’interface utilisateur correspondant, en tenant compte de la présence d’autres éléments d’interface utilisateur, d’autres conseils clés et du bord de l’écran.

En règle générale, l’emplacement des pourboires par défaut est suffisant et fournit une prise en charge intégrée de l’interface utilisateur adaptative.

![Exemple d’emplacement de l’info-bulle automatique](images/accesskeys/auto-keytip-position.png)

_Exemple d’emplacement de l’info-bulle automatique_

Toutefois, si vous avez besoin de davantage de contrôle sur le positionnement des touches accélératrices, nous vous recommandons ce qui suit :

1.  **Principe d’association évident**: l’utilisateur peut facilement associer le contrôle à l’info-bulle.

    a.  L’info-bulle doit être **proche** de l’élément qui a la clé d’accès (propriétaire).  
    b.  Le Conseil clé doit **éviter de couvrir les éléments activés** qui ont des clés d’accès.   
    c.  Si une info-bulle ne peut pas être placée à proximité de son propriétaire, elle doit chevaucher le propriétaire. 

2.  **Détectabilité**: l’utilisateur peut découvrir rapidement le contrôle avec la touche accélératrice.

    a.  L’astuce clé ne **chevauche** jamais d’autres touches accélératrices.  

3.  **Analyse facile :** L’utilisateur peut facilement parcourir les conseils clés.

    a.  Les touches accélératrices doivent être **alignées** les unes avec les autres et avec l’élément d’interface utilisateur.
    b.  Les conseils clés doivent être **regroupés** autant que possible. 

### <a name="relative-position"></a>Position relative

Utilisez la propriété **KeyTipPlacementMode** pour personnaliser le placement de l’info-bulle clé sur une base par élément ou par groupe.

Les modes de placement sont les suivants : haut, bas, droite, gauche, masqué, Centre et automatique.

![Modes de positionnement des touches accélératrices](images/accesskeys/keytip-postion-modes.png)

_Modes de positionnement des touches accélératrices_

La ligne centrale du contrôle est utilisée pour calculer l’alignement vertical et horizontal de l’info-bulle.

L’exemple suivant montre comment définir le placement de l’info-bulle clé d’un groupe de contrôles à l’aide de la propriété KeyTipPlacementMode d’un conteneur StackPanel.

``` xaml
<StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" KeyTipPlacementMode="Top">
  <Button Content="File" AccessKey="F" />
  <Button Content="Home" AccessKey="H" />
  <Button Content="Insert" AccessKey="N" />
</StackPanel>
```

### <a name="offsets"></a>Décalages

Utilisez les propriétés KeyTipHorizontalOffset et KeyTipVerticalOffset d’un élément pour un contrôle encore plus granulaire de l’emplacement de l’info-bulle de clé.

> [!NOTE]
> Les offsets ne peuvent pas être définis lorsque KeyTipPlacementMode a la valeur auto.

La propriété KeyTipHorizontalOffset indique jusqu’à quel point l’info-bulle doit être déplacée vers la gauche ou la droite. montre comment définir des décalages d’info-bulle pour un bouton.

![Modes de positionnement des touches accélératrices](images/accesskeys/keytip-offsets.png)

_Définir des décalages verticaux et horizontaux pour une touche accélératrice_

``` xaml
<Button
  Content="File"
  AccessKey="F"
  KeyTipPlacementMode="Bottom"
  KeyTipHorizontalOffset="20"
  KeyTipVerticalOffset="-8" />
```

### <a name="screen-edge-alignment-screen-edge-alignment-listparagraph"></a>Alignement du bord de l’écran {#screen-Edge-Alignment. ListParagraph}

L’emplacement d’une info-bulle est ajusté automatiquement en fonction du bord de l’écran pour vérifier que l’info-bulle est entièrement visible. Lorsque cela se produit, la distance entre le contrôle et le point d’alignement de l’extrémité de clé peut différer des valeurs spécifiées pour les décalages horizontal et vertical.

![Modes de positionnement des touches accélératrices](images/accesskeys/keytips-screen-edge.png)

_Le bord de l’écran provoque le repositionnement automatique de l’info-bulle_

## <a name="key-tip-style"></a>Style d’info-bulle

Nous vous recommandons d’utiliser la prise en charge intégrée des touches accélératrices pour les thèmes de plateforme, y compris le contraste élevé.

Si vous devez spécifier vos propres styles d’info-bulle, utilisez des ressources d’application telles que KeyTipFontSize (taille de police), KeyTipFontFamily (famille de polices), KeyTipBackground (arrière-plan), KeyTipForeground (premier plan), KeyTipPadding (remplissage), KeyTipBorderBrush (couleur de bordure) et KeyTipBorderThemeThickness (épaisseur de bordure).

![Modes de positionnement des touches accélératrices](images/accesskeys/keytip-customization.png)

_Options de personnalisation des touches accélératrices_

Cet exemple montre comment modifier les ressources de l’application :

 ```xaml  
<Application.Resources>
  <SolidColorBrush Color="DarkGray" x:Key="MyBackgroundColor" />
  <SolidColorBrush Color="White" x:Key="MyForegroundColor" />
  <SolidColorBrush Color="Black" x:Key="MyBorderColor" />
  <StaticResource x:Key="KeyTipBackground" ResourceKey="MyBackgroundColor" />
  <StaticResource x:Key="KeyTipForeground" ResourceKey="MyForegroundColor" />
  <StaticResource x:Key="KeyTipBorderBrush" ResourceKey="MyBorderColor"/>
  <FontFamily x:Key="KeyTipFontFamily">Consolas</FontFamily>
  <x:Double x:Key="KeyTipContentThemeFontSize">18</x:Double>
  <Thickness x:Key="KeyTipBorderThemeThickness">2</Thickness>
  <Thickness x:Key="KeyTipThemePadding">4,4,4,4</Thickness>
</Application.Resources>
```

## <a name="access-keys-and-narrator"></a>Touches d’accès rapide et Narrateur

L’infrastructure XAML expose des propriétés Automation qui permettent aux clients UI Automation de découvrir des informations sur les éléments de l’interface utilisateur.

Si vous spécifiez la propriété AccessKey sur un contrôle UIElement ou TextElement, vous pouvez utiliser la propriété [AutomationProperties. AccessKey](/dotnet/api/system.windows.automation.automationproperties.accesskey) pour obtenir cette valeur. Les clients d’accessibilité, tels que le narrateur, lisent la valeur de cette propriété chaque fois qu’un élément obtient le focus.

## <a name="related-articles"></a>Articles connexes

* [Interactions avec le clavier](keyboard-interactions.md)
* [Raccourcis clavier](keyboard-accelerators.md)

**Exemples**
* [Galerie de contrôles XAML (alias XamlUiBasics)](https://github.com/Microsoft/Windows-universal-samples/tree/c2aeaa588d9b134466bbd2cc387c8ff4018f151e/Samples/XamlUIBasics)