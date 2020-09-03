---
Description: NavigationView est un contrôle adaptatif qui implémente des modèles de navigation de niveau supérieur pour votre application.
title: Affichage de navigation
template: detail.hbs
ms.date: 05/02/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: ''
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 7f71a11c76bc6318c9000a9468c7bd9574e0c5d0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170033"
---
# <a name="navigation-view"></a>Affichage de navigation

Le contrôle NavigationView garantit une navigation de niveau supérieur à votre application. Il s’adapte à un large éventail de tailles d’écran et prend en charge les styles de navigation _supérieure_ et _gauche_.

![navigation supérieure](images/nav-view-header.png)<br/>
_Affichage de navigation prend en charge à la fois le panneau ou menu de navigation supérieure et gauche_

**Obtenir la bibliothèque d’interface utilisateur Windows**

|  |  |
| - | - |
| ![Logo WinUI](images/winui-logo-64x64.png) | Le contrôle **NavigationView** est inclus dans la bibliothèque d’interface utilisateur Windows, package NuGet qui contient les nouveaux contrôles et fonctionnalités d’interface utilisateur pour les applications Windows. Pour plus d’informations, notamment des instructions d’installation, consultez [Vue d’ensemble de la bibliothèque d’interface utilisateur Windows](/uwp/toolkits/winui/). |

> **API de plateforme** : [Classe Windows.UI.Xaml.Controls.NavigationView](/uwp/api/windows.ui.xaml.controls.navigationview)
>
> **API de la bibliothèque d’interface utilisateur Windows** : [Classe Microsoft.UI.Xaml.Controls.NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview)
>
> Certaines fonctionnalités de NavigationView, par exemple la navigation _supérieure_ et _hiérarchique_, nécessitent Windows 10, version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou version ultérieure, ou la [bibliothèque d’interface utilisateur Windows](/uwp/toolkits/winui/).

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

NavigationView est un contrôle de navigation adaptatif très efficace pour les tâches suivantes :

- La fourniture d’une expérience de navigation cohérente dans toute votre application.
- La conservation de l’espace de l’écran des fenêtres de plus petite taille.
- La gestion de l’accès à de nombreuses catégories de navigation.

Pour visualiser d’autres modèles de navigation, voir [Informations de base relatives à la conception de la navigation](../basics/navigation-basics.md).

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/XAML-controls-gallery-app-icon-sm.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/NavigationView">ouvrir l’application et voir l'objet NavigationView en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="display-modes"></a>Modes d’affichage

> La propriété PaneDisplayMode nécessite Windows 10 version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou ultérieure, ou la [bibliothèque d’interface utilisateur Windows](/uwp/toolkits/winui/).

Vous pouvez utiliser la propriété PaneDisplayMode pour configurer différents styles de navigation, ou modes d’affichage, pour NavigationView.

:::row:::
    :::column:::
    ### <a name="top"></a>Haut
    Le volet est positionné au-dessus du contenu.</br>
    `PaneDisplayMode="Top"`
    :::column-end:::
    :::column span="2":::
    ![Exemple de navigation supérieure](images/displaymode-top.png)
    :::column-end:::
:::row-end:::

Nous vous recommandons la navigation _supérieure_ dans les cas suivants :

- Vous avez au moins 5 catégories de navigation de niveau supérieur d’égale importance, et toute autre catégorie de navigation de niveau supérieur qui se retrouve dans le menu déroulant de dépassement est considérée comme moins importante.
- Vous devez afficher toutes les options de navigation à l’écran.
- Vous souhaitez davantage d’espace pour le contenu de votre application.
- Les icônes ne peuvent pas décrire clairement les catégories de navigation de votre application.

:::row:::
    :::column:::
    ### <a name="left"></a>Gauche
    Le volet est développé et positionné à gauche du contenu.</br>
    `PaneDisplayMode="Left"`
    :::column-end:::
    :::column span="2":::
    ![Exemple de volet de navigation à gauche développé](images/displaymode-left.png)
    :::column-end:::
:::row-end:::

Nous vous recommandons la navigation _à gauche_ dans les cas suivants :

- Vous avez 5 à 10 catégories de navigation de niveau supérieur de même importance.
- Vous voulez faire ressortir les catégories de navigation, en laissant moins d'espace aux autres contenus de l'application.

:::row:::
    :::column:::
    ### <a name="leftcompact"></a>LeftCompact
    Le volet affiche les icônes jusqu'à ce qu'il soit ouvert et positionné à gauche du contenu.</br>
    `PaneDisplayMode="LeftCompact"`
    :::column-end:::
    :::column span="2":::
    ![Exemple de volet de navigation à gauche réduit](images/displaymode-leftcompact.png)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
    ### <a name="leftminimal"></a>LeftMinimal
    Seul le bouton de menu est affiché jusqu'à ce que le volet s’ouvre. Lorsqu’il est ouvert, il apparaît à gauche du contenu.</br>
    `PaneDisplayMode="LeftMinimal"`
    :::column-end:::
    :::column span="2":::
    ![Exemple de volet de navigation à gauche minimal](images/displaymode-leftminimal.png)
    :::column-end:::
:::row-end:::

### <a name="auto"></a>Auto

Par défaut, la propriété PaneDisplayMode est définie sur Auto. En mode Auto, la vue de navigation passe du mode LeftMinimal (gauche minimal) lorsque la fenêtre est étroite, au mode LeftCompact (gauche compact), puis Left (gauche) lorsque la fenêtre s'élargit. Pour en savoir plus, voir la section [Comportement adaptatif](#adaptive-behavior).

![Comportement adaptatif par défaut de la navigation à gauche](images/displaymode-auto.png)<br/>
_Comportement adaptatif par défaut de la vue de navigation_

## <a name="anatomy"></a>Anatomie

Ces images montrent la disposition du volet, de l’en-tête et des zones de contenu du contrôle avec une configuration de navigation _supérieure_ ou _à gauche_.

![Disposition de la vue de navigation supérieure](images/topnav-anatomy.png)<br/>
_Disposition de la navigation supérieure_

![Disposition de la vue de navigation à gauche](images/leftnav-anatomy.png)<br/>
_Disposition de la navigation à gauche_

### <a name="pane"></a>Volet

Vous pouvez utiliser la propriété PaneDisplayMode pour positionner le volet au-dessus ou à gauche du contenu.

Le volet NavigationView peut contenir :

- Objets [NavigationViewItem](/uwp/api/windows.ui.xaml.controls.navigationviewitem). Éléments de navigation pour accéder à des pages spécifiques.
- Objets [NavigationViewItemSeparator](/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator). Séparateurs pour regrouper des éléments de navigation. Définissez la propriété [Opacité](/uwp/api/windows.ui.xaml.uielement.opacity) sur 0 pour afficher le séparateur comme un espace.
- Objets [NavigationViewItemHeader](/uwp/api/windows.ui.xaml.controls.navigationviewitemheader). En-têtes pour l’étiquetage de groupes d’éléments.
- Un contrôle [AutoSuggestBox](auto-suggest-box.md) facultatif permettant d'effectuer des recherches au niveau de l’application. Attribuez le contrôle à la propriété [NavigationView.AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.navigationview.autosuggestbox).
- Un point d’entrée facultatif pour les [paramètres d’application](../app-settings/guidelines-for-app-settings.md). Pour masquer l’élément relatif aux paramètres, définissez la propriété [IsSettingsVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsSettingsVisible) sur **false**.

Le volet gauche contient également les éléments suivants :

- Un bouton de menu pour activer/désactiver le volet ouvert et fermé. Sur les grandes fenêtres d’application lorsque le volet est ouvert, vous pouvez choisir de masquer ce bouton à l’aide de la propriété [IsPaneToggleButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsPaneToggleButtonVisible).

La vue de navigation affiche un bouton Précédent placé dans le coin supérieur gauche du volet. Mais elle ne gère pas automatiquement la navigation vers l’arrière et n'ajoute aucun contenu à la pile arrière. Pour activer la navigation vers l’arrière, consultez la section [Navigation vers l’arrière](#backwards-navigation).

Voici l’anatomie du volet détaillé pour les positions du volet de navigation supérieure et à gauche.

#### <a name="top-navigation-pane"></a>Volet de navigation supérieure

![Anatomie de volet de navigation supérieure](images/navview-pane-anatomy-horizontal.png)

1. En-têtes
1. Éléments de navigation
1. Séparateurs
1. AutoSuggestBox (facultatif)
1. Bouton Paramètres (facultatif)

#### <a name="left-navigation-pane"></a>Volet de navigation à gauche

![Anatomie de volet de navigation à gauche](images/navview-pane-anatomy-vertical.png)

1. Bouton Menu
1. Éléments de navigation
1. Séparateurs
1. En-têtes
1. AutoSuggestBox (facultatif)
1. Bouton Paramètres (facultatif)

#### <a name="pane-footer"></a>Pied de page du volet

Vous pouvez placer un contenu de forme libre dans le pied de page du volet en l’ajoutant à la propriété [PaneFooter](/uwp/api/windows.ui.xaml.controls.navigationview.PaneFooter).

:::row:::
    :::column:::
    ![Navigation supérieure du pied de page du volet](images/navview-freeform-footer-top.png)<br>
     _Pied de page du volet supérieur_<br>
    :::column-end:::
    :::column:::
    ![Pied de page du volet de navigation à gauche](images/navview-freeform-footer-left.png)<br>
    _Pied de page du volet gauche_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-title-and-header"></a>Titre et en-tête du volet

Vous pouvez placer le contenu textuel dans la zone d'en-tête du volet en définissant la propriété [PaneTitle](/uwp/api/windows.ui.xaml.controls.navigationview.PaneTitle). Cette propriété prend une chaîne et affiche le texte en regard du bouton de menu.

Pour ajouter du contenu non textuel, comme une image ou un logo, vous pouvez placer n'importe quel élément dans l'en-tête du volet en l'ajoutant à la propriété [PaneHeader](/uwp/api/windows.ui.xaml.controls.navigationview.PaneHeader).

Si les propriétés PaneTitle et PaneHeader sont définies, le contenu est empilé horizontalement à côté du bouton de menu, la propriété PaneTitle étant la plus proche du bouton de menu.

:::row:::
    :::column:::
    ![En-tête du volet de navigation supérieure](images/navview-freeform-header-top.png)<br>
     _En-tête du volet supérieur_<br>
    :::column-end:::
    :::column:::
    ![En-tête du volet de navigation à gauche](images/navview-freeform-header-left.png)<br>
    _En-tête du volet gauche_<br>
    :::column-end:::
:::row-end:::

#### <a name="pane-content"></a>Contenu du volet

Vous pouvez placer un contenu de forme libre dans le volet en l’ajoutant à la propriété [PaneCustomContent](/uwp/api/windows.ui.xaml.controls.navigationview.PaneCustomContent).

:::row:::
    :::column:::
    ![Contenu personnalisé du volet de navigation supérieure](images/navview-freeform-pane-top.png)<br>
     _Contenu personnalisé du volet supérieur_<br>
    :::column-end:::
    :::column:::
    ![Contenu personnalisé du volet de navigation à gauche](images/navview-freeform-pane-left.png)<br>
    _Contenu personnalisé du volet gauche_<br>
    :::column-end:::
:::row-end:::

### <a name="header"></a>Header

Vous pouvez ajouter un titre de page en définissant la propriété [Header](/uwp/api/windows.ui.xaml.controls.navigationview.header).

![Exemple de zone d’en-tête de l’affichage de navigation](images/nav-header.png)<br/>
_En-tête de l’affichage de navigation_

La zone d'en-tête est alignée verticalement avec le bouton de navigation dans la position du volet gauche et se trouve sous le volet dans la position du volet supérieur. Elle affiche une hauteur fixe de 52 px. Elle contient le titre de la page de la catégorie de navigation sélectionnée. L’en-tête est ancré sur le haut de la page et se comporte comme un point de découpage de défilement pour la zone de contenu.

L'en-tête est visible chaque fois que NavigationView est en mode minimal. Vous pouvez choisir de masquer l’en-tête dans les autres modes, utilisés pour des fenêtres de largeur supérieure. Pour masquer l’en-tête, définissez la propriété [AlwaysShowHeader](/uwp/api/windows.ui.xaml.controls.navigationview.AlwaysShowHeader) sur **false**.

### <a name="content"></a>Content

![Exemple de zone de contenu de l’affichage de navigation](images/nav-content.png)<br/>
_Contenu d’affichage de navigation_

La zone de contenu présente la plupart des informations relatives à la catégorie de navigation sélectionnée.

Nous recommandons des marges de 12 px pour votre zone de contenu lorsque NavigationView est en mode **Minimal**, et des marges de 24 px dans les autres cas.

## <a name="adaptive-behavior"></a>Comportement adaptatif

Par défaut, le mode d’affichage de navigation change automatiquement selon la quantité d’espace d’écran à sa disposition. Les propriétés [CompactModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth) et [ExpandedModeThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.expandedmodethresholdwidth) indiquent les points d’arrêt où le mode d'affichage change. Vous pouvez modifier ces valeurs pour personnaliser le comportement du mode d’affichage adaptatif.

### <a name="default"></a>Par défaut

Lorsque la propriété PaneDisplayMode est définie sur sa valeur par défaut **Auto**, le comportement adaptatif consiste à afficher :

- Un volet gauche développé sur les fenêtres de grande largeur (1008 px ou supérieur).
- Un volet de navigation gauche (LeftCompact) comportant uniquement des icônes sur les fenêtres de largeur moyenne (641 px à 1007 px).
- Uniquement un bouton de menu (LeftMinimal) sur les fenêtres de petite largeur (640 px ou moins).

Pour plus d'informations sur les tailles de fenêtre pour un comportement adaptatif, consultez [Tailles d’écran et points d’arrêt](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).

![Comportement adaptatif par défaut de la navigation à gauche](images/displaymode-auto.png)<br/>
_Comportement adaptatif par défaut de la vue de navigation_

### <a name="minimal"></a>Minimal

Un deuxième modèle adaptatif courant consiste à utiliser un volet gauche étendu sur les fenêtres de grande largeur, et uniquement un bouton de menu sur les fenêtres de moyenne et petite largeurs.

Ce modèle est recommandé dans les cas suivants :

- Vous voulez plus d'espace pour le contenu de l'application sur les fenêtres de plus petite largeur.
- Vos catégories de navigation ne peuvent pas être clairement représentées par des icônes.

![Comportement adaptatif minimal de la navigation à gauche](images/adaptive-behavior-minimal.png)<br/>
_Comportement adaptatif « minimal » de l’affichage de navigation_

Pour configurer ce comportement, définissez CompactModeThresholdWidth sur la largeur à laquelle vous voulez que le volet se réduise. Ici, la taille passe de la valeur par défaut 640 à 1007. Vous devez également définir ExpandedModeThresholdWidth pour vous assurer que les valeurs n’entrent pas en conflit.

```xaml
<NavigationView CompactModeThresholdWidth="1007" ExpandedModeThresholdWidth="1007"/>
```

### <a name="compact"></a>Compact

Un troisième modèle adaptatif courant consiste à utiliser un volet gauche étendu sur les fenêtres de grande largeur, et un volet de navigation à icônes uniquement (LeftCompact) sur les fenêtres de moyenne et petite largeurs.

Ce modèle est recommandé dans les cas suivants :

- Il est important de toujours afficher toutes les options de navigation à l’écran.
- Vos catégories de navigation peuvent être clairement représentées par des icônes.

![Comportement adaptatif compact de la navigation à gauche](images/adaptive-behavior-compact.png)<br/>
_Comportement adaptatif compact de l’affichage de navigation_

Pour configurer ce comportement, définissez CompactModeThresholdWidth sur 0.

```xaml
<NavigationView CompactModeThresholdWidth="0"/>
```

### <a name="no-adaptive-behavior"></a>Aucun comportement adaptatif

Pour désactiver le comportement adaptatif automatique, définissez PaneDisplayMode sur une valeur autre que Auto. Ici, la valeur est définie sur LeftMinimal afin d’afficher uniquement le bouton de menu, quelle que soit la largeur de la fenêtre.

![Comportement non adaptatif de la navigation à gauche](images/adaptive-behavior-none.png)<br/>
_Mode de navigation avec PaneDisplayMode définie sur LeftMinimal_

```xaml
<NavigationView PaneDisplayMode="LeftMinimal" />
```

Comme décrit précédemment dans la section _Modes d’affichage_, vous pouvez définir le volet afin qu’il soit toujours visible, toujours développé, toujours compact ou toujours minimal. Vous pouvez également gérer les modes d’affichage vous-même dans le code de votre application. Vous trouverez un exemple dans la prochaine section.

### <a name="top-to-left-navigation"></a>Navigation du haut vers la gauche

Lorsque vous utilisez la navigation supérieure dans votre application, les éléments de navigation se réduisent en un menu de dépassement à mesure que la largeur de la fenêtre diminue. Lorsque la fenêtre de votre application est étroite, il est possible d'améliorer l'expérience de l'utilisateur en faisant passer le mode PaneDisplayMode de la navigation supérieure (Top) à LeftMinimal, plutôt que de laisser tous les éléments se réduire dans le menu de dépassement.

Nous recommandons d'utiliser la navigation supérieure pour les grandes fenêtres, et la navigation par la gauche pour les petites fenêtres dans les cas suivants :

- Vous utilisez un groupe de catégories de navigation de niveau supérieur de même importance à afficher ensemble, de sorte que si une catégorie de ce groupe ne rentre pas dans l'écran, vous la réduisez avec la navigation à gauche pour lui donner la même importance.
- Vous souhaitez conserver autant d'espace de contenu que possible dans les fenêtres de petite taille.

Cet exemple montre comment utiliser des propriétés [VisualStateManager](/uwp/api/Windows.UI.Xaml.VisualStateManager) et [AdaptiveTrigger.MinWindowWidth](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth) pour basculer entre la navigation supérieure (Top) et LeftMinimal.

![Exemple 1 de comportement adaptatif supérieur ou à gauche](images/navigation-top-to-left.png)

```xaml
<Grid>
    <NavigationView x:Name="NavigationViewControl" >
        <NavigationView.MenuItems>
            <NavigationViewItem Content="A" x:Name="A" />
            <NavigationViewItem Content="B" x:Name="B" />
            <NavigationViewItem Content="C" x:Name="C" />
        </NavigationView.MenuItems>
    </NavigationView>

    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState>
                <VisualState.StateTriggers>
                    <AdaptiveTrigger
                        MinWindowWidth="{x:Bind NavigationViewControl.CompactModeThresholdWidth}" />
                </VisualState.StateTriggers>

                <VisualState.Setters>
                    <Setter Target="NavigationViewControl.PaneDisplayMode" Value="Top"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>

```

> [!TIP]
> Lorsque vous utilisez AdaptiveTrigger.MinWindowWidth, l'état visuel est déclenché lorsque la fenêtre est plus large que la largeur minimale spécifiée. Cela signifie que la valeur XAML par défaut définit la fenêtre étroite, et que la valeur VisualState définit les modifications appliquées lorsque la fenêtre s'élargit. La propriété PaneDisplayMode par défaut pour l’affichage de navigation est Auto. Par conséquent, lorsque la largeur de la fenêtre est inférieure ou égale à la valeur CompactModeThresholdWidth, la navigation LeftMinimal est utilisée. Lorsque la fenêtre s'élargit, VisualState prend le pas sur la valeur par défaut et la navigation supérieure est utilisée.

## <a name="navigation"></a>Navigation

L’affichage de navigation n’effectue aucune tâche de navigation automatiquement. Lorsque l'utilisateur appuie sur un élément de navigation, l’affichage de navigation montre cet élément comme étant sélectionné et affiche un événement [ItemInvoked](/uwp/api/windows.ui.xaml.controls.navigationview.ItemInvoked). Si l’appui entraîne la sélection d’un nouvel élément, l’événement [SelectionChanged](/uwp/api/windows.ui.xaml.controls.navigationview.SelectionChanged) est également déclenché.

Vous pouvez gérer l'un ou l'autre événement pour effectuer des tâches liées à la navigation demandée. Celui que vous devez gérer dépend du comportement que vous voulez appliquer à votre application. En règle générale, vous naviguez jusqu'à la page demandée puis mettez à jour l'en-tête de l’affichage de navigation en réponse à ces événements.

**ItemInvoked** est déclenché chaque fois que l’utilisateur appuie sur un élément de navigation, même s’il est déjà sélectionné. (L'élément peut également être invoqué avec une action équivalente à l'aide de la souris, du clavier ou d'une autre entrée. Pour plus d’informations, voir [Entrée et interactions](../input/index.md).) Si vous naviguez dans le gestionnaire ItemInvoked, par défaut, la page sera rechargée et une entrée en double sera ajoutée à la pile de navigation. Si vous naviguez lorsqu'un élément est invoqué, vous devez empêcher le rechargement de la page ou vous assurer qu'une entrée en double n'est pas créée dans le backstack de navigation lorsque la page est rechargée. (Voir les exemples de code.)

**SelectionChanged** peut être déclenché par un utilisateur invoquant un élément qui n'est pas actuellement sélectionné, ou en modifiant par programmation l'élément sélectionné. Si le changement de sélection se produit parce qu'un utilisateur a invoqué un élément, l'événement ItemInvoked se produit en premier. Si la modification de la sélection s’effectue par programmation, ItemInvoked n’est pas déclenché.

### <a name="backwards-navigation"></a>Navigation vers l’arrière

NavigationView intègre un bouton précédent ; mais, comme pour la navigation vers l'avant, il n'effectue pas automatiquement la navigation vers l'arrière. Lorsque l’utilisateur actionne le bouton précédent, l’événement [BackRequested](/uwp/api/windows.ui.xaml.controls.navigationview.BackRequested) est déclenché. Vous gérez cet événement pour effectuer la navigation vers l’arrière. Pour obtenir plus d’informations et des exemples de code, consultez l’article [Historique de navigation et navigation vers l’arrière](../basics/navigation-history-and-backwards-navigation.md).

En mode Minimal ou Compact, le volet Affichage de navigation est ouvert en tant que menu volant. Dans ce cas, vous pouvez cliquer sur le bouton précédent pour fermer le volet et afficher l'événement **PaneClosing** à la place.

Vous pouvez masquer ou désactiver le bouton précédent en définissant ces propriétés :

- [IsBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackButtonVisible): pour afficher et masquer le bouton précédent. Cette propriété prend une valeur de l'énumération [NavigationViewBackButtonVisible](/uwp/api/windows.ui.xaml.controls.navigationviewbackbuttonvisible), qui est définie sur **Auto** par défaut. Lorsque le bouton est réduit, aucun espace n’est réservé pour celui-ci dans la disposition.
- [IsBackEnabled](/uwp/api/windows.ui.xaml.controls.navigationview.IsBackEnabled): activer ou désactiver le bouton précédent. Vous pouvez lier les données de cette propriété à la propriété [CanGoBack](/uwp/api/windows.ui.xaml.controls.frame.cangoback) de votre cadre de navigation. **BackRequested** n’est pas déclenché si **IsBackEnabled** est défini sur **false**.

:::row:::
    :::column:::
        ![Bouton précédent en mode navigation dans le volet de navigation gauche](images/leftnav-back.png)<br/>
        _Bouton précédent dans le volet de navigation gauche_
    :::column-end:::
    :::column:::
        ![Bouton précédent en mode navigation dans le volet de navigation supérieur](images/topnav-back.png)<br/>
        _Bouton précédent dans le volet de navigation supérieur_
    :::column-end:::
:::row-end:::

## <a name="code-example"></a>Exemple de code

> [!IMPORTANT]
> Pour tout projet qui utilise la boîte à outils de la bibliothèque d’interface utilisateur Windows (WinUI), suivez les mêmes étapes de configuration préliminaires. Pour plus d’informations sur l’arrière-plan, la configuration et la prise en charge, consultez [Prise en main de la bibliothèque d’IU Windows](/uwp/toolkits/winui/getting-started).

Cet exemple montre comment utiliser **NavigationView** avec un volet de navigation supérieur sur de grandes fenêtres et un volet de navigation à gauche sur les petites fenêtres. Il peut être adapté à la navigation à gauche uniquement en supprimant les paramètres de navigation *supérieure* dans le  **VisualStateManager**.

L'exemple montre une façon recommandée de configurer des données de navigation qui s’adaptent à de nombreux scénarios courants. Il explique également comment implémenter la navigation vers l’arrière avec le bouton précédent de **NavigationView** et la navigation au clavier.

Ce code suppose que votre application contient des pages avec les noms suivants pour accéder à ces éléments : *HomePage*, *AppsPage*, *GamesPage*, *MusicPage*, *MyContentPage* et *SettingsPage*. Le code de ces pages n’est pas affiché.

> [!IMPORTANT]
> Vous trouverez des informations sur les pages de l’application dans un [ValueTuple](/dotnet/api/system.valuetuple). Cette structure nécessite une version minimale SDK 17763 ou supérieure pour votre projet d’application. Si vous utilisez la version WinUI de NavigationView pour cibler des versions antérieures de Windows 10, vous pouvez utiliser le [paquet NuGet System.ValueTuple](https://www.nuget.org/packages/System.ValueTuple/).

> [!IMPORTANT]
> Ce code montre comment utiliser la version [Bibliothèque d’interface utilisateur Windows](/uwp/toolkits/winui/) de NavigationView. Si vous utilisez la version plateforme de NavigationView à la place, la version minimale pour votre projet d'application doit être SDK 17763 ou supérieure. Pour utiliser la version plateforme, supprimez toutes les références à `muxc:`.

```xaml
<Page ... xmlns:muxc="using:Microsoft.UI.Xaml.Controls" ... >
<Grid>
    <muxc:NavigationView x:Name="NavView"
                         Loaded="NavView_Loaded"
                         ItemInvoked="NavView_ItemInvoked"
                         BackRequested="NavView_BackRequested">
        <muxc:NavigationView.MenuItems>
            <muxc:NavigationViewItem Tag="home" Icon="Home" Content="Home"/>
            <muxc:NavigationViewItemSeparator/>
            <muxc:NavigationViewItemHeader x:Name="MainPagesHeader"
                                           Content="Main pages"/>
            <muxc:NavigationViewItem Tag="apps" Content="Apps">
                <muxc:NavigationViewItem.Icon>
                    <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xEB3C;"/>
                </muxc:NavigationViewItem.Icon>
            </muxc:NavigationViewItem>
            <muxc:NavigationViewItem Tag="games" Content="Games">
                <muxc:NavigationViewItem.Icon>
                    <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE7FC;"/>
                </muxc:NavigationViewItem.Icon>
            </muxc:NavigationViewItem>
            <muxc:NavigationViewItem Tag="music" Icon="Audio" Content="Music"/>
        </muxc:NavigationView.MenuItems>

        <muxc:NavigationView.AutoSuggestBox>
            <!-- See AutoSuggestBox documentation for
                 more info about how to implement search. -->
            <AutoSuggestBox x:Name="NavViewSearchBox" QueryIcon="Find"/>
        </muxc:NavigationView.AutoSuggestBox>

        <ScrollViewer>
            <Frame x:Name="ContentFrame" Padding="12,0,12,24" IsTabStop="True"
                   NavigationFailed="ContentFrame_NavigationFailed"/>
        </ScrollViewer>
    </muxc:NavigationView>

    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState>
                <VisualState.StateTriggers>
                    <AdaptiveTrigger
                        MinWindowWidth="{x:Bind NavViewCompactModeThresholdWidth}"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <!-- Remove the next 3 lines for left-only navigation. -->
                    <Setter Target="NavView.PaneDisplayMode" Value="Top"/>
                    <Setter Target="NavViewSearchBox.Width" Value="200"/>
                    <Setter Target="MainPagesHeader.Visibility" Value="Collapsed"/>
                    <!-- Leave the next line for left-only navigation. -->
                    <Setter Target="ContentFrame.Padding" Value="24,0,24,24"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>
</Page>
```

> [!IMPORTANT]
> Ce code montre comment utiliser la version [Bibliothèque d’interface utilisateur Windows](/uwp/toolkits/winui/) de NavigationView. Si vous utilisez la version plateforme de NavigationView à la place, la version minimale pour votre projet d'application doit être SDK 17763 ou supérieure. Pour utiliser la version plateforme, supprimez toutes les références à `muxc`.

```csharp
// Add "using" for WinUI controls.
// using muxc = Microsoft.UI.Xaml.Controls;

private double NavViewCompactModeThresholdWidth { get { return NavView.CompactModeThresholdWidth; } }

private void ContentFrame_NavigationFailed(object sender, NavigationFailedEventArgs e)
{
    throw new Exception("Failed to load Page " + e.SourcePageType.FullName);
}

// List of ValueTuple holding the Navigation Tag and the relative Navigation Page
private readonly List<(string Tag, Type Page)> _pages = new List<(string Tag, Type Page)>
{
    ("home", typeof(HomePage)),
    ("apps", typeof(AppsPage)),
    ("games", typeof(GamesPage)),
    ("music", typeof(MusicPage)),
};

private void NavView_Loaded(object sender, RoutedEventArgs e)
{
    // You can also add items in code.
    NavView.MenuItems.Add(new muxc.NavigationViewItemSeparator());
    NavView.MenuItems.Add(new muxc.NavigationViewItem
    {
        Content = "My content",
        Icon = new SymbolIcon((Symbol)0xF1AD),
        Tag = "content"
    });
    _pages.Add(("content", typeof(MyContentPage)));

    // Add handler for ContentFrame navigation.
    ContentFrame.Navigated += On_Navigated;

    // NavView doesn't load any page by default, so load home page.
    NavView.SelectedItem = NavView.MenuItems[0];
    // If navigation occurs on SelectionChanged, this isn't needed.
    // Because we use ItemInvoked to navigate, we need to call Navigate
    // here to load the home page.
    NavView_Navigate("home", new Windows.UI.Xaml.Media.Animation.EntranceNavigationTransitionInfo());

    // Add keyboard accelerators for backwards navigation.
    var goBack = new KeyboardAccelerator { Key = Windows.System.VirtualKey.GoBack };
    goBack.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(goBack);

    // ALT routes here
    var altLeft = new KeyboardAccelerator
    {
        Key = Windows.System.VirtualKey.Left,
        Modifiers = Windows.System.VirtualKeyModifiers.Menu
    };
    altLeft.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(altLeft);
}

private void NavView_ItemInvoked(muxc.NavigationView sender,
                                 muxc.NavigationViewItemInvokedEventArgs args)
{
    if (args.IsSettingsInvoked == true)
    {
        NavView_Navigate("settings", args.RecommendedNavigationTransitionInfo);
    }
    else if (args.InvokedItemContainer != null)
    {
        var navItemTag = args.InvokedItemContainer.Tag.ToString();
        NavView_Navigate(navItemTag, args.RecommendedNavigationTransitionInfo);
    }
}

// NavView_SelectionChanged is not used in this example, but is shown for completeness.
// You will typically handle either ItemInvoked or SelectionChanged to perform navigation,
// but not both.
private void NavView_SelectionChanged(muxc.NavigationView sender,
                                      muxc.NavigationViewSelectionChangedEventArgs args)
{
    if (args.IsSettingsSelected == true)
    {
        NavView_Navigate("settings", args.RecommendedNavigationTransitionInfo);
    }
    else if (args.SelectedItemContainer != null)
    {
        var navItemTag = args.SelectedItemContainer.Tag.ToString();
        NavView_Navigate(navItemTag, args.RecommendedNavigationTransitionInfo);
    }
}

private void NavView_Navigate(
    string navItemTag,
    Windows.UI.Xaml.Media.Animation.NavigationTransitionInfo transitionInfo)
{
    Type _page = null;
    if (navItemTag == "settings")
    {
        _page = typeof(SettingsPage);
    }
    else
    {
        var item = _pages.FirstOrDefault(p => p.Tag.Equals(navItemTag));
        _page = item.Page;
    }
    // Get the page type before navigation so you can prevent duplicate
    // entries in the backstack.
    var preNavPageType = ContentFrame.CurrentSourcePageType;

    // Only navigate if the selected page isn't currently loaded.
    if (!(_page is null) && !Type.Equals(preNavPageType, _page))
    {
        ContentFrame.Navigate(_page, null, transitionInfo);
    }
}

private void NavView_BackRequested(muxc.NavigationView sender,
                                   muxc.NavigationViewBackRequestedEventArgs args)
{
    On_BackRequested();
}

private void BackInvoked(KeyboardAccelerator sender,
                         KeyboardAcceleratorInvokedEventArgs args)
{
    On_BackRequested();
    args.Handled = true;
}

private bool On_BackRequested()
{
    if (!ContentFrame.CanGoBack)
        return false;

    // Don't go back if the nav pane is overlayed.
    if (NavView.IsPaneOpen &&
        (NavView.DisplayMode == muxc.NavigationViewDisplayMode.Compact ||
         NavView.DisplayMode == muxc.NavigationViewDisplayMode.Minimal))
        return false;

    ContentFrame.GoBack();
    return true;
}

private void On_Navigated(object sender, NavigationEventArgs e)
{
    NavView.IsBackEnabled = ContentFrame.CanGoBack;

    if (ContentFrame.SourcePageType == typeof(SettingsPage))
    {
        // SettingsItem is not part of NavView.MenuItems, and doesn't have a Tag.
        NavView.SelectedItem = (muxc.NavigationViewItem)NavView.SettingsItem;
        NavView.Header = "Settings";
    }
    else if (ContentFrame.SourcePageType != null)
    {
        var item = _pages.FirstOrDefault(p => p.Page == e.SourcePageType);

        NavView.SelectedItem = NavView.MenuItems
            .OfType<muxc.NavigationViewItem>()
            .First(n => n.Tag.Equals(item.Tag));

        NavView.Header =
            ((muxc.NavigationViewItem)NavView.SelectedItem)?.Content?.ToString();
    }
}
```

> [!NOTE]
> Pour la version [C++/WinRT](../../cpp-and-winrt-apis/index.md) de cet exemple de code, commencez par créer un projet basé sur le modèle de projet **Application vide ( C++/WinRT)** , puis ajoutez le code de la liste dans les fichiers de code source indiqués. Pour utiliser le code source exactement comme indiqué dans la liste, nommez votre nouveau projet *NavigationViewCppWinRT*

```cppwinrt
// MainPage.idl
runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
{
    ...
    Double NavViewCompactModeThresholdWidth{ get; };
}

// pch.h
...
#include "winrt/Windows.UI.Xaml.Input.h"
#include "winrt/Windows.UI.Xaml.Media.Animation.h"
#include "winrt/Microsoft.UI.Xaml.Controls.h"
#include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"

// MainPage.h
#pragma once

#include "MainPage.g.h"

namespace muxc
{
    using namespace winrt::Microsoft::UI::Xaml::Controls;
};

namespace wuxc
{
    using namespace winrt::Windows::UI::Xaml::Controls;
};

namespace winrt::NavigationViewCppWinRT::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        double NavViewCompactModeThresholdWidth();
        void ContentFrame_NavigationFailed(
            Windows::Foundation::IInspectable const& /* sender */,
            Windows::UI::Xaml::Navigation::NavigationFailedEventArgs const& args);
        void NavView_Loaded(
            Windows::Foundation::IInspectable const& /* sender */,
            Windows::UI::Xaml::RoutedEventArgs const& /* args */);
        void NavView_ItemInvoked(
            Windows::Foundation::IInspectable const& /* sender */,
            muxc::NavigationViewItemInvokedEventArgs const& args);

        // NavView_SelectionChanged is not used in this example, but is shown for completeness.
        // You'll typically handle either ItemInvoked or SelectionChanged to perform navigation,
        // but not both.
        void NavView_SelectionChanged(
            muxc::NavigationView const& /* sender */,
            muxc::NavigationViewSelectionChangedEventArgs const& args);
        void NavView_Navigate(
            std::wstring navItemTag,
            Windows::UI::Xaml::Media::Animation::NavigationTransitionInfo const& transitionInfo);
        void NavView_BackRequested(
            muxc::NavigationView const& /* sender */,
            muxc::NavigationViewBackRequestedEventArgs const& /* args */);
        void BackInvoked(
            Windows::UI::Xaml::Input::KeyboardAccelerator const& /* sender */,
            Windows::UI::Xaml::Input::KeyboardAcceleratorInvokedEventArgs const& args);
        bool On_BackRequested();
        void On_Navigated(
            Windows::Foundation::IInspectable const& /* sender */,
            Windows::UI::Xaml::Navigation::NavigationEventArgs const& args);

    private:
        // Vector of std::pair holding the Navigation Tag and the relative Navigation Page.
        std::vector<std::pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>> m_pages;
    };
}

namespace winrt::NavigationViewCppWinRT::factory_implementation
{
    struct MainPage : MainPageT<MainPage, implementation::MainPage>
    {
    };
}

// MainPage.cpp
#include "pch.h"
#include "MainPage.h"
#include "MainPage.g.cpp"

namespace winrt::NavigationViewCppWinRT::implementation
{
    MainPage::MainPage()
    {
        InitializeComponent();
        m_pages.push_back(std::make_pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>
            (L"home", winrt::xaml_typename<NavigationViewCppWinRT::HomePage>()));
        m_pages.push_back(std::make_pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>
            (L"apps", winrt::xaml_typename<NavigationViewCppWinRT::AppsPage>()));
        m_pages.push_back(std::make_pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>
            (L"games", winrt::xaml_typename<NavigationViewCppWinRT::GamesPage>()));
        m_pages.push_back(std::make_pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>
            (L"music", winrt::xaml_typename<NavigationViewCppWinRT::MusicPage>()));
    }

    double MainPage::NavViewCompactModeThresholdWidth()
    {
        return NavView().CompactModeThresholdWidth();
    }

    void MainPage::ContentFrame_NavigationFailed(
        Windows::Foundation::IInspectable const& /* sender */,
        Windows::UI::Xaml::Navigation::NavigationFailedEventArgs const& args)
    {
        throw winrt::hresult_error(
            E_FAIL, winrt::hstring(L"Failed to load Page ") + args.SourcePageType().Name);
    }

    // List of ValueTuple holding the Navigation Tag and the relative Navigation Page
    std::vector<std::pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>> m_pages;

    void MainPage::NavView_Loaded(
        Windows::Foundation::IInspectable const& /* sender */,
        Windows::UI::Xaml::RoutedEventArgs const& /* args */)
    {
        // You can also add items in code.
        NavView().MenuItems().Append(muxc::NavigationViewItemSeparator());
        muxc::NavigationViewItem navigationViewItem;
        navigationViewItem.Content(winrt::box_value(L"My content"));
        navigationViewItem.Icon(wuxc::SymbolIcon(static_cast<wuxc::Symbol>(0xF1AD)));
        navigationViewItem.Tag(winrt::box_value(L"content"));
        NavView().MenuItems().Append(navigationViewItem);
        m_pages.push_back(
            std::make_pair<std::wstring, Windows::UI::Xaml::Interop::TypeName>(
                L"content", winrt::xaml_typename<NavigationViewCppWinRT::MyContentPage>()));

        // Add handler for ContentFrame navigation.
        ContentFrame().Navigated({ this, &MainPage::On_Navigated });

        // NavView doesn't load any page by default, so load home page.
        NavView().SelectedItem(NavView().MenuItems().GetAt(0));
        // If navigation occurs on SelectionChanged, then this isn't needed.
        // Because we use ItemInvoked to navigate, we need to call Navigate
        // here to load the home page.
        NavView_Navigate(L"home",
            Windows::UI::Xaml::Media::Animation::EntranceNavigationTransitionInfo());

        // Add keyboard accelerators for backwards navigation.
        Windows::UI::Xaml::Input::KeyboardAccelerator goBack;
        goBack.Key(Windows::System::VirtualKey::GoBack);
        goBack.Invoked({ this, &MainPage::BackInvoked });
        KeyboardAccelerators().Append(goBack);

        // ALT routes here
        Windows::UI::Xaml::Input::KeyboardAccelerator altLeft;
        goBack.Key(Windows::System::VirtualKey::Left);
        goBack.Modifiers(Windows::System::VirtualKeyModifiers::Menu);
        goBack.Invoked({ this, &MainPage::BackInvoked });
        KeyboardAccelerators().Append(altLeft);
    }

    void MainPage::NavView_ItemInvoked(
        Windows::Foundation::IInspectable const& /* sender */,
        muxc::NavigationViewItemInvokedEventArgs const& args)
    {
        if (args.IsSettingsInvoked())
        {
            NavView_Navigate(L"settings", args.RecommendedNavigationTransitionInfo());
        }
        else if (args.InvokedItemContainer())
        {
            NavView_Navigate(
                winrt::unbox_value_or<winrt::hstring>(
                    args.InvokedItemContainer().Tag(), L"").c_str(),
                args.RecommendedNavigationTransitionInfo());
        }
    }

    // NavView_SelectionChanged is not used in this example, but is shown for completeness.
    // You will typically handle either ItemInvoked or SelectionChanged to perform navigation,
    // but not both.
    void MainPage::NavView_SelectionChanged(
        muxc::NavigationView const& /* sender */,
        muxc::NavigationViewSelectionChangedEventArgs const& args)
    {
        if (args.IsSettingsSelected())
        {
            NavView_Navigate(L"settings", args.RecommendedNavigationTransitionInfo());
        }
        else if (args.SelectedItemContainer())
        {
            NavView_Navigate(
                winrt::unbox_value_or<winrt::hstring>(
                    args.SelectedItemContainer().Tag(), L"").c_str(),
                args.RecommendedNavigationTransitionInfo());
        }
    }

    void MainPage::NavView_Navigate(
        std::wstring navItemTag,
        Windows::UI::Xaml::Media::Animation::NavigationTransitionInfo const& transitionInfo)
    {
        Windows::UI::Xaml::Interop::TypeName pageTypeName;
        if (navItemTag == L"settings")
        {
            pageTypeName = winrt::xaml_typename<NavigationViewCppWinRT::SettingsPage>();
        }
        else
        {
            for (auto&& eachPage : m_pages)
            {
                if (eachPage.first == navItemTag)
                {
                    pageTypeName = eachPage.second;
                    break;
                }
            }
        }
        // Get the page type before navigation so you can prevent duplicate
        // entries in the backstack.
        Windows::UI::Xaml::Interop::TypeName preNavPageType =
            ContentFrame().CurrentSourcePageType();

        // Navigate only if the selected page isn't currently loaded.
        if (pageTypeName.Name != L"" && preNavPageType.Name != pageTypeName.Name)
        {
            ContentFrame().Navigate(pageTypeName, nullptr, transitionInfo);
        }
    }

    void MainPage::NavView_BackRequested(
        muxc::NavigationView const& /* sender */,
        muxc::NavigationViewBackRequestedEventArgs const& /* args */)
    {
        On_BackRequested();
    }

    void MainPage::BackInvoked(
        Windows::UI::Xaml::Input::KeyboardAccelerator const& /* sender */,
        Windows::UI::Xaml::Input::KeyboardAcceleratorInvokedEventArgs const& args)
    {
        On_BackRequested();
        args.Handled(true);
    }

    bool MainPage::On_BackRequested()
    {
        if (!ContentFrame().CanGoBack())
            return false;

        // Don't go back if the nav pane is overlaid.
        if (NavView().IsPaneOpen() &&
            (NavView().DisplayMode() == muxc::NavigationViewDisplayMode::Compact ||
                NavView().DisplayMode() == muxc::NavigationViewDisplayMode::Minimal))
            return false;

        ContentFrame().GoBack();
        return true;
    }

    void MainPage::On_Navigated(
        Windows::Foundation::IInspectable const& /* sender */,
        Windows::UI::Xaml::Navigation::NavigationEventArgs const& args)
    {
        NavView().IsBackEnabled(ContentFrame().CanGoBack());

        if (ContentFrame().SourcePageType().Name ==
            winrt::xaml_typename<NavigationViewCppWinRT::SettingsPage>().Name)
        {
            // SettingsItem is not part of NavView.MenuItems, and doesn't have a Tag.
            NavView().SelectedItem(NavView().SettingsItem().as<muxc::NavigationViewItem>());
            NavView().Header(winrt::box_value(L"Settings"));
        }
        else if (ContentFrame().SourcePageType().Name != L"")
        {
            for (auto&& eachPage : m_pages)
            {
                if (eachPage.second.Name == args.SourcePageType().Name)
                {
                    for (auto&& eachMenuItem : NavView().MenuItems())
                    {
                        auto navigationViewItem =
                            eachMenuItem.try_as<muxc::NavigationViewItem>();
                        {
                            if (navigationViewItem)
                            {
                                winrt::hstring hstringValue =
                                    winrt::unbox_value_or<winrt::hstring>(
                                        navigationViewItem.Tag(), L"");
                                if (hstringValue == eachPage.first)
                                {
                                    NavView().SelectedItem(navigationViewItem);
                                    NavView().Header(navigationViewItem.Content());
                                }
                            }
                        }
                    }
                    break;
                }
            }
        }
    }
}
```

### <a name="alternative-cwinrt-implementation"></a>Autre implémentation de C++/WinRT

Le code C# et C++/WinRT montré ci-dessus est conçu pour vous permettre d’utiliser le même balisage XAML pour les deux versions. Toutefois, il existe une autre façon d’implémenter la version C++/WinRT décrite dans cette section, que vous préférerez peut-être.

Vous trouverez ci-dessous une autre version du gestionnaire **NavView_ItemInvoked**. La technique de cette version du gestionnaire consiste d'abord à stocker (dans la balise de [**NavigationViewItem**](/uwp/api/windows.ui.xaml.controls.navigationviewitem)) le nom de type complet de la page vers laquelle vous voulez naviguer. Dans le gestionnaire, vous effectuez une conversion unboxing de cette valeur, la convertissez en un objet [**Windows::UI::Xaml::Interop::TypeName**](/uwp/api/windows.ui.xaml.interop.typename) et l’utilisez pour accéder à la page de destination. Il n’est pas nécessaire de mapper la variable nommée `_pages` que vous voyez dans l’exemple ci-dessus, et vous pourrez créer des tests unitaires confirmant que les valeurs à l’intérieur de vos balises sont d’un type valide. Voir aussi [Conversions boxing et unboxing de valeurs scalaires vers IInspectable avec C++/WinRT](../../cpp-and-winrt-apis/boxing.md).

```cppwinrt
void MainPage::NavView_ItemInvoked(
    Windows::Foundation::IInspectable const & /* sender */,
    Windows::UI::Xaml::Controls::NavigationViewItemInvokedEventArgs const & args)
{
    if (args.IsSettingsInvoked())
    {
        // Navigate to Settings.
    }
    else if (args.InvokedItemContainer())
    {
        Windows::UI::Xaml::Interop::TypeName pageTypeName;
        pageTypeName.Name = unbox_value<hstring>(args.InvokedItemContainer().Tag());
        pageTypeName.Kind = Windows::UI::Xaml::Interop::TypeKind::Primitive;
        ContentFrame().Navigate(pageTypeName, nullptr);
    }
}
```

## <a name="hierarchical-navigation"></a>Navigation hiérarchique
Certaines applications peuvent avoir une structure hiérarchique plus complexe qui nécessite plus qu’une simple liste plate d’éléments de navigation. Vous souhaiterez peut-être utiliser des éléments de navigation de niveau supérieur pour afficher des catégories de pages, avec des éléments enfants affichant des pages spécifiques. Cette approche est également utile si vous avez des pages de style hub uniquement liées à d’autres pages. Dans ces types de cas, vous devez créer un NavigationView hiérarchique.

Pour montrer une liste hiérarchique d’éléments de navigation imbriqués dans le volet, utilisez la propriété [MenuItems](/uwp/api/microsoft.ui.xaml.controls.navigationviewitem.menuitems?view=winui-2.4) ou la propriété [MenuItemsSource](/uwp/api/microsoft.ui.xaml.controls.navigationviewitem.menuitemssource?view=winui-2.4) de **NavigationViewItem**.
Chaque NavigationViewItem peut contenir d’autres NavigationViewItems et éléments d’organisation tels que des séparateurs et des en-têtes d’élément. Pour afficher une liste hiérarchique quand vous utilisez `MenuItemsSource`, définissez `ItemTemplate` en tant que NavigationViewItem et liez sa propriété `MenuItemsSource` au niveau suivant de la hiérarchie.

Bien que NavigationViewItem puisse contenir un nombre quelconque de niveaux imbriqués, nous vous recommandons de limiter la profondeur de la hiérarchie de navigation de votre application. Nous pensons que deux niveaux sont idéaux pour faciliter l’utilisation et la compréhension.

NavigationView affiche la hiérarchie dans les modes d’affichage du volet Top, Left et LeftCompact. Voici à quoi ressemble une sous-arborescence développée dans chacun des modes d’affichage du volet :

![NavigationView avec hiérarchie](images/navigation-view-hierarchy-labeled.png)

### <a name="adding-a-hierarchy-of-items-in-markup"></a>Ajout d’une hiérarchie d’éléments dans le balisage
Déclarez la hiérarchie de navigation de l’application dans le balisage.

```Xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<muxc:NavigationView>
    <muxc:NavigationView.MenuItems>
        <muxc:NavigationViewItem Content="Home" Icon="Home" ToolTipService.ToolTip="Home"/>
        <muxc:NavigationViewItem Content="Collections" Icon="Keyboard" ToolTipService.ToolTip="Collections">
            <muxc:NavigationViewItem.MenuItems>
                <muxc:NavigationViewItem Content="Notes" Icon="Page" ToolTipService.ToolTip="Notes"/>
                <muxc:NavigationViewItem Content="Mail" Icon="Mail" ToolTipService.ToolTip="Mail"/>
            </muxc:NavigationViewItem.MenuItems>
        </muxc:NavigationViewItem>
    </muxc:NavigationView.MenuItems>
</muxc:NavigationView>
```

### <a name="adding-a-hierarchy-of-items-using-data-binding"></a>Ajout d’une hiérarchie d’éléments à l’aide de la liaison de données

Ajoutez une hiérarchie d’éléments de menu à NavigationView en 
* liant la propriété MenuItemsSource aux données hiérarchiques ;
* définissant le modèle d’élément en tant que NavigationViewMenuItem, avec son contenu (Content) défini sur l’étiquette de l’élément de menu et sa propriété MenuItemsSource liée au niveau suivant de la hiérarchie.

Cet exemple montre également les événements de développement ([Expanding](/uwp/api/microsoft.ui.xaml.controls.navigationview.expanding?view=winui-2.4)) et de réduction ([Collapsed](/uwp/api/microsoft.ui.xaml.controls.navigationview.collapsed?view=winui-2.4)). Ces événements sont déclenchés pour un élément de menu avec enfants.

```xaml
<Page ... xmlns:muxc="using:Microsoft.UI.Xaml.Controls" ... >
    <Page.Resources>
        <DataTemplate x:Key="NavigationViewMenuItem" x:DataType="local:Category">
            <muxc:NavigationViewItem Content="{x:Bind Name}" MenuItemsSource="{x:Bind Children}"/>
        </DataTemplate>
    </Page.Resources>

    <Grid>
        <muxc:NavigationView x:Name="navview" 
    MenuItemsSource="{x:Bind Categories, Mode=OneWay}" 
    MenuItemTemplate="{StaticResource NavigationViewMenuItem}" 
    ItemInvoked="{x:Bind OnItemInvoked}" 
    Expanding="OnItemExpanding" 
    Collapsed="OnItemCollapsed" 
    PaneDisplayMode="Left">
            <StackPanel Margin="10,10,0,0">
                <TextBlock Margin="0,10,0,0" x:Name="ExpandingItemLabel" Text="Last Expanding: N/A"/>
                <TextBlock x:Name="CollapsedItemLabel" Text="Last Collapsed: N/A"/>
            </StackPanel>
        </muxc:NavigationView>
    </Grid>
</Page>
```

```csharp
public class Category
{
    public String Name { get; set; }
    public String CategoryIcon { get; set; }
    public ObservableCollection<Category> Children { get; set; }
}

public sealed partial class HierarchicalNavigationViewDataBinding : Page
{
    public HierarchicalNavigationViewDataBinding()
    {
        this.InitializeComponent();
    }  

    public ObservableCollection<Category> Categories = new ObservableCollection<Category>()
    {
        new Category(){
            Name = "Menu item 1",
            CategoryIcon = "Icon",
            Children = new ObservableCollection<Category>() {
                new Category(){
                    Name = "Menu item 2",
                    CategoryIcon = "Icon",
                    Children = new ObservableCollection<Category>() {
                        new Category() {
                            Name  = "Menu item 3",
                            CategoryIcon = "Icon",
                            Children = new ObservableCollection<Category>() {
                                new Category() { Name  = "Menu item 4", CategoryIcon = "Icon" },
                                new Category() { Name  = "Menu item 5", CategoryIcon = "Icon" }
                            }
                        }
                    }
                }
            }
        },
        new Category(){
            Name = "Menu item 6",
            CategoryIcon = "Icon",
            Children = new ObservableCollection<Category>() {
                new Category(){
                    Name = "Menu item 7",
                    CategoryIcon = "Icon",
                    Children = new ObservableCollection<Category>() {
                        new Category() { Name  = "Menu item 8", CategoryIcon = "Icon" },
                        new Category() { Name  = "Menu item 9", CategoryIcon = "Icon" }
                    }
                }
            }
        },
        new Category(){ Name = "Menu item 10", CategoryIcon = "Icon" }
    };

    private void OnItemInvoked(object sender, NavigationViewItemInvokedEventArgs e)
    {
        var clickedItem = e.InvokedItem;
        var clickedItemContainer = e.InvokedItemContainer;
    }
    private void OnItemExpanding(object sender, NavigationViewItemExpandingEventArgs e)
    {
        var nvib = e.ExpandingItemContainer;
        var name = "Last expanding: " + nvib.Content.ToString();
        ExpandingItemLabel.Text = name;
    }
    private void OnItemCollapsed(object sender, NavigationViewItemCollapsedEventArgs e)
    {
        var nvib = e.CollapsedItemContainer;
        var name = "Last collapsed: " + nvib.Content;
        CollapsedItemLabel.Text = name;
    }
}
```

```cppwinrt
// Category.idl
namespace HierarchicalNavigationViewDataBinding
{
    runtimeclass Category
    {
        String Name;
        String CategoryIcon;
        Windows.Foundation.Collections.IObservableVector<Category> Children;
    }
}

// Category.h
#pragma once
#include "Category.g.h"

namespace winrt::HierarchicalNavigationViewDataBinding::implementation
{
    struct Category : CategoryT<Category>
    {
        Category();
        Category(winrt::hstring name,
            winrt::hstring categoryIcon,
            Windows::Foundation::Collections::
                IObservableVector<HierarchicalNavigationViewDataBinding::Category> children);

        winrt::hstring Name();
        void Name(winrt::hstring const& value);
        winrt::hstring CategoryIcon();
        void CategoryIcon(winrt::hstring const& value);
        Windows::Foundation::Collections::
            IObservableVector<HierarchicalNavigationViewDataBinding::Category> Children();
        void Children(Windows::Foundation::Collections:
            IObservableVector<HierarchicalNavigationViewDataBinding::Category> const& value);

    private:
        winrt::hstring m_name;
        winrt::hstring m_categoryIcon;
        Windows::Foundation::Collections::
            IObservableVector<HierarchicalNavigationViewDataBinding::Category> m_children;
    };
}

// Category.cpp
#include "pch.h"
#include "Category.h"
#include "Category.g.cpp"

namespace winrt::HierarchicalNavigationViewDataBinding::implementation
{
    Category::Category()
    {
        m_children = winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
    }

    Category::Category(
        winrt::hstring name,
        winrt::hstring categoryIcon,
        Windows::Foundation::Collections::
            IObservableVector<HierarchicalNavigationViewDataBinding::Category> children)
    {
        m_name = name;
        m_categoryIcon = categoryIcon;
        m_children = children;
    }

    hstring Category::Name()
    {
        return m_name;
    }

    void Category::Name(hstring const& value)
    {
        m_name = value;
    }

    hstring Category::CategoryIcon()
    {
        return m_categoryIcon;
    }

    void Category::CategoryIcon(hstring const& value)
    {
        m_categoryIcon = value;
    }

    Windows::Foundation::Collections::IObservableVector<HierarchicalNavigationViewDataBinding::Category>
        Category::Children()
    {
        return m_children;
    }

    void Category::Children(
        Windows::Foundation::Collections::IObservableVector<HierarchicalNavigationViewDataBinding::Category>
            const& value)
    {
        m_children = value;
    }
}

// MainPage.idl
import "Category.idl";

namespace HierarchicalNavigationViewDataBinding
{
    [default_interface]
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        Windows.Foundation.Collections.IObservableVector<Category> Categories{ get; };
    }
}

// MainPage.h
#pragma once

#include "MainPage.g.h"

namespace muxc
{
    using namespace winrt::Microsoft::UI::Xaml::Controls;
};

namespace winrt::HierarchicalNavigationViewDataBinding::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        Windows::Foundation::Collections::IObservableVector<HierarchicalNavigationViewDataBinding::Category>
            Categories();

        void OnItemInvoked(muxc::NavigationView const& sender, muxc::NavigationViewItemInvokedEventArgs const& args);
        void OnItemExpanding(
            muxc::NavigationView const& sender,
            muxc::NavigationViewItemExpandingEventArgs const& args);
        void OnItemCollapsed(
            muxc::NavigationView const& sender,
            muxc::NavigationViewItemCollapsedEventArgs const& args);

    private:
        Windows::Foundation::Collections::
            IObservableVector<HierarchicalNavigationViewDataBinding::Category> m_categories;
    };
}

namespace winrt::HierarchicalNavigationViewDataBinding::factory_implementation
{
    struct MainPage : MainPageT<MainPage, implementation::MainPage>
    {
    };
}

// MainPage.cpp
#include "pch.h"
#include "MainPage.h"
#include "MainPage.g.cpp"

#include "Category.h"

namespace winrt::HierarchicalNavigationViewDataBinding::implementation
{
    MainPage::MainPage()
    {
        InitializeComponent();

        m_categories = 
            winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();

        auto menuItem10 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 10", L"Icon", nullptr);

        auto menuItem9 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 9", L"Icon", nullptr);
        auto menuItem8 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 8", L"Icon", nullptr);
        auto menuItem7Children = 
            winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
        menuItem7Children.Append(*menuItem9);
        menuItem7Children.Append(*menuItem8);

        auto menuItem7 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 7", L"Icon", menuItem7Children);
        auto menuItem6Children = 
            winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
        menuItem6Children.Append(*menuItem7);

        auto menuItem6 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 6", L"Icon", menuItem6Children);

        auto menuItem5 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 5", L"Icon", nullptr);
        auto menuItem4 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 4", L"Icon", nullptr);
        auto menuItem3Children =
            winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
        menuItem3Children.Append(*menuItem5);
        menuItem3Children.Append(*menuItem4);

        auto menuItem3 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 3", L"Icon", menuItem3Children);
        auto menuItem2Children = 
            winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
        menuItem2Children.Append(*menuItem3);

        auto menuItem2 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 2", L"Icon", menuItem2Children);
        auto menuItem1Children = 
            winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
        menuItem1Children.Append(*menuItem2);

        auto menuItem1 = winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
            (L"Menu item 1", L"Icon", menuItem1Children);

        m_categories.Append(*menuItem1);
        m_categories.Append(*menuItem6);
        m_categories.Append(*menuItem10);
    }

    Windows::Foundation::Collections::IObservableVector<HierarchicalNavigationViewDataBinding::Category>
        MainPage::Categories()
    {
        return m_categories;
    }

    void MainPage::OnItemInvoked(
        muxc::NavigationView const& /* sender */,
        muxc::NavigationViewItemInvokedEventArgs const& args)
    {
        auto clickedItem = args.InvokedItem();
        auto clickedItemContainer = args.InvokedItemContainer();
    }

    void MainPage::OnItemExpanding(
        muxc::NavigationView const& /* sender */,
        muxc::NavigationViewItemExpandingEventArgs const& args)
    {
        auto nvib = args.ExpandingItemContainer();
        auto name = L"Last expanding: " + winrt::unbox_value<winrt::hstring>(nvib.Content());
        ExpandingItemLabel().Text(name);
    }

    void MainPage::OnItemCollapsed(
        muxc::NavigationView const& /* sender */,
        muxc::NavigationViewItemCollapsedEventArgs const& args)
    {
        auto nvib = args.CollapsedItemContainer();
        auto name = L"Last collapsed: " + winrt::unbox_value<winrt::hstring>(nvib.Content());
        CollapsedItemLabel().Text(name);
    }
}
```

### <a name="selection"></a>d’un certificat SSTP

Par défaut, tous les éléments peuvent contenir des enfants, être appelés ou être sélectionnés.

Quand vous proposez aux utilisateurs une arborescence d’options de navigation, vous pouvez choisir de rendre les éléments parents non sélectionnables, par exemple quand votre application n’a pas de page de destination associée à l’élément parent concerné. Si vos éléments parents _sont_ sélectionnables, nous vous recommandons d’utiliser les modes d’affichage Left-Expanded ou Top du volet. Avec le mode LeftCompact, l’utilisateur accède à l’élément parent pour ouvrir la sous-arborescence enfant chaque fois qu’il est appelé.

Les éléments sélectionnés affichent leurs indicateurs de sélection le long de leur bord gauche quand ils sont disposés en mode Left ou de leur bord inférieur quand ils sont disposés en mode Top. Vous trouverez ci-dessous des NavigationViews en mode Left et Top dans lesquels un élément parent est sélectionné.

![NavigationView en mode Left avec parent sélectionné](images/navigation-view-selection.png)

![NavigationView en mode Top avec parent sélectionné](images/navigation-view-selection-top.png)

Il est possible que l’élément sélectionné ne soit pas toujours visible. Si un enfant d’une sous-arborescence réduite/non développée est sélectionné, son premier ancêtre visible s’affiche comme étant sélectionné. L’indicateur de sélection revient à l’élément sélectionné si/quand la sous-arborescence est développée.

Par exemple, dans l’image ci-dessus, l’élément Calendar peut être sélectionné par l’utilisateur, qui peut ensuite réduire la sous-arborescence. Dans ce cas, l’indicateur de sélection s’affiche sous l’élément Account, car ce dernier est le premier ancêtre visible de l’élément Calendar. L’indicateur de sélection revient à l’élément Calendar quand l’utilisateur redéveloppe la sous-arborescence. 

Le NavigationView entier n’affiche pas plus d’un indicateur de sélection.

Dans les deux modes Top et Left, un clic sur les flèches sur les NavigationViewItems permet de développer ou de réduire la sous-arborescence. En cliquant ou en appuyant _ailleurs_ sur le NavigationViewItem, vous déclenchez l’événement `ItemInvoked`, qui entraîne également la réduction ou le développement de la sous-arborescence.

Pour empêcher un élément d’afficher l’indicateur de sélection quand il est appelé, affectez la valeur False à sa propriété [SelectsOnInvoked](/uwp/api/microsoft.ui.xaml.controls.navigationviewitem.selectsoninvoked?view=winui-2.3), comme indiqué ci-dessous :

```xaml
<Page ... xmlns:muxc="using:Microsoft.UI.Xaml.Controls" ... >
    <Page.Resources>
        <DataTemplate x:Key="NavigationViewMenuItem" x:DataType="local:Category">
            <muxc:NavigationViewItem Content="{x:Bind Name}"
            MenuItemsSource="{x:Bind Children}"
            SelectsOnInvoked="{x:Bind IsLeaf}"/>
        </DataTemplate>
    </Page.Resources>

    <Grid>
        <muxc:NavigationView x:Name="navview" 
    MenuItemsSource="{x:Bind Categories, Mode=OneWay}" 
    MenuItemTemplate="{StaticResource NavigationViewMenuItem}">
        </muxc:NavigationView>
    </Grid>
</Page>
```

```csharp
public class Category
{
    public String Name { get; set; }
    public String CategoryIcon { get; set; }
    public ObservableCollection<Category> Children { get; set; }
    public bool IsLeaf { get; set; }
}

public sealed partial class HierarchicalNavigationViewDataBinding : Page
{
    public HierarchicalNavigationViewDataBinding()
    {
        this.InitializeComponent();
    }  

    public ObservableCollection<Category> Categories = new ObservableCollection<Category>()
    {
        new Category(){
            Name = "Menu item 1",
            CategoryIcon = "Icon",
            Children = new ObservableCollection<Category>() {
                new Category(){
                    Name = "Menu item 2",
                    CategoryIcon = "Icon",
                    Children = new ObservableCollection<Category>() {
                        new Category() {
                            Name  = "Menu item 3",
                            CategoryIcon = "Icon",
                            Children = new ObservableCollection<Category>() {
                                new Category() { Name  = "Menu item 4", CategoryIcon = "Icon", IsLeaf = true },
                                new Category() { Name  = "Menu item 5", CategoryIcon = "Icon", IsLeaf = true }
                            }
                        }
                    }
                }
            }
        },
        new Category(){
            Name = "Menu item 6",
            CategoryIcon = "Icon",
            Children = new ObservableCollection<Category>() {
                new Category(){
                    Name = "Menu item 7",
                    CategoryIcon = "Icon",
                    Children = new ObservableCollection<Category>() {
                        new Category() { Name  = "Menu item 8", CategoryIcon = "Icon", IsLeaf = true },
                        new Category() { Name  = "Menu item 9", CategoryIcon = "Icon", IsLeaf = true }
                    }
                }
            }
        },
        new Category(){ Name = "Menu item 10", CategoryIcon = "Icon", IsLeaf = true }
    };
}
```

```cppwinrt
// Category.idl
namespace HierarchicalNavigationViewDataBinding
{
    runtimeclass Category
    {
        ...
        Boolean IsLeaf;
    }
}

// Category.h
...
struct Category : CategoryT<Category>
{
    ...
    Category(winrt::hstring name,
        winrt::hstring categoryIcon,
        Windows::Foundation::Collections::IObservableVector<HierarchicalNavigationViewDataBinding::Category> children,
        bool isleaf = false);
    ...
    bool IsLeaf();
    void IsLeaf(bool value);

private:
    ...
    bool m_isleaf;
};

// Category.cpp
...
Category::Category(winrt::hstring name,
    winrt::hstring categoryIcon,
    Windows::Foundation::Collections::IObservableVector<HierarchicalNavigationViewDataBinding::Category> children,
    bool isleaf) : m_name(name), m_categoryIcon(categoryIcon), m_children(children), m_isleaf(isleaf) {}
...
bool Category::IsLeaf()
{
    return m_isleaf;
}

void Category::IsLeaf(bool value)
{
    m_isleaf = value;
}

// MainPage.h and MainPage.cpp
// Delete OnItemInvoked, OnItemExpanding, and OnItemCollapsed.

// MainPage.cpp
...
MainPage::MainPage()
{
    InitializeComponent();

    m_categories = winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();

    auto menuItem10 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 10", L"Icon", nullptr, true);

    auto menuItem9 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 9", L"Icon", nullptr, true);
    auto menuItem8 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 8", L"Icon", nullptr, true);
    auto menuItem7Children = 
        winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
    menuItem7Children.Append(*menuItem9);
    menuItem7Children.Append(*menuItem8);

    auto menuItem7 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 7", L"Icon", menuItem7Children);
    auto menuItem6Children = 
        winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
    menuItem6Children.Append(*menuItem7);

    auto menuItem6 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 6", L"Icon", menuItem6Children);

    auto menuItem5 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 5", L"Icon", nullptr, true);
    auto menuItem4 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 4", L"Icon", nullptr, true);
    auto menuItem3Children = 
        winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
    menuItem3Children.Append(*menuItem5);
    menuItem3Children.Append(*menuItem4);

    auto menuItem3 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 3", L"Icon", menuItem3Children);
    auto menuItem2Children = 
        winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
    menuItem2Children.Append(*menuItem3);

    auto menuItem2 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 2", L"Icon", menuItem2Children);
    auto menuItem1Children = 
        winrt::single_threaded_observable_vector<HierarchicalNavigationViewDataBinding::Category>();
    menuItem1Children.Append(*menuItem2);

    auto menuItem1 = 
        winrt::make_self<HierarchicalNavigationViewDataBinding::implementation::Category>
        (L"Menu item 1", L"Icon", menuItem1Children);

    m_categories.Append(*menuItem1);
    m_categories.Append(*menuItem6);
    m_categories.Append(*menuItem10);
}
...
```

### <a name="keyboarding-within-hierarchical-navigationview"></a>Utilisation du clavier avec un NavigationView hiérarchique

Les utilisateurs peuvent déplacer le focus dans la vue de navigation à l’aide du [clavier](../input/keyboard-interactions.md). Les touches de direction exposent la « navigation interne » dans le volet et suivent les interactions fournies dans l’[arborescence](./tree-view.md). Les actions des touches changent quand vous naviguez dans le NavigationView ou dans son menu volant, qui s’affiche dans les modes Top and LeftCompact de HierarchicalNavigationView. Voici les actions spécifiques que chaque touche peut effectuer dans un NavigationView hiérarchique :

| Clé      |      En mode Left      |  En mode Top | Dans le menu volant  |
|----------|------------------------|--------------|------------|
| Haut |Déplace le focus vers l’élément situé juste au-dessus de l’élément actif. | Ne fait rien. |Déplace le focus vers l’élément situé juste au-dessus de l’élément actif.|
| Bas|Déplace le focus juste au-dessous de l’élément actif.* | Ne fait rien. | Déplace le focus juste au-dessous de l’élément actif.* |
| Droit |Ne fait rien.  |Déplace le focus vers l’élément situé juste à droite de l’élément actif. |Ne fait rien.|
| Gauche |Ne fait rien. | Déplace le focus vers l’élément situé juste à gauche de l’élément actif.  |Ne fait rien. |
| Espace/Entrée |Si l’élément a des enfants, développe/réduit l’élément et ne change pas le focus.   | Si l’élément a des enfants, développe les enfants dans un menu volant et place le focus sur le premier élément du menu. | Appelle/sélectionne un élément et ferme le menu volant. |
| Échap | Ne fait rien. | Ne fait rien. | Ferme le menu volant.|

La touche Espace ou Entrée appelle/sélectionne toujours un élément.

*Notez que les éléments n’ont pas besoin d’être visuellement adjacents, car le focus se déplace du dernier élément de la liste du volet vers l’élément relatif aux paramètres. 

## <a name="navigation-view-customization"></a>Personnalisation de l’affichage de navigation

### <a name="pane-backgrounds"></a>Arrière-plans de volet de navigation

Par défaut, le volet NavigationView utilise un arrière-plan différent selon le mode d’affichage :

- le volet est d'une couleur grise unie lorsqu'il est agrandi à gauche, côte à côte avec le contenu (en mode Left) ;
- le volet utilise l'acrylique dans l’application lorsqu'il est ouvert sous forme de superposition par rapport au contenu (en mode Top, Minimal ou Compact).

Pour modifier l'arrière-plan du volet, vous pouvez remplacer les ressources du thème XAML utilisées l’affichage de l'arrière-plan dans chaque mode. (Cette technique est privilégiée par rapport à une propriété PaneBackground unique car elle prend en charge différents arrière-plans pour différents modes d'affichage.)

Ce tableau indique quelle ressource de thème est utilisée dans chaque mode d'affichage.

| Mode d’affichage | Ressource de thème |
| ------------ | -------------- |
| Gauche | NavigationViewExpandedPaneBackground |
| LeftCompact<br/>LeftMinimal | NavigationViewDefaultPaneBackground |
| Haut | NavigationViewTopPaneBackground |

Cet exemple montre comment remplacer les ressources de thème dans App.xaml. Lorsque vous remplacez des ressources de thème, vous devez toujours fournir au minimum les dictionnaires de ressources « Default » et « HighContrast» , et les dictionnaires pour les ressources « Light » ou « Dark » si nécessaire. Pour plus d’informations, consultez [ResourceDictionary.ThemeDictionaries](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries).

> [!IMPORTANT]
> Ce code montre comment utiliser la version [Bibliothèque d’interface utilisateur Windows](/uwp/toolkits/winui/) d’AcrylicBrush. Si vous utilisez la version plateforme d’AcrylicBrush à la place, la version minimale pour votre projet d'application doit être SDK 16299 ou supérieure. Pour utiliser la version plateforme, supprimez toutes les références à `muxm:`.

```xaml
<Application ... xmlns:muxm="using:Microsoft.UI.Xaml.Media" ...>
    <Application.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>
                <ResourceDictionary>
                    <ResourceDictionary.ThemeDictionaries>
                        <ResourceDictionary x:Key="Default">
                            <!-- The "Default" theme dictionary is used unless a specific
                                 light, dark, or high contrast dictionary is provided. These
                                 resources should be tested with both the light and dark themes,
                                 and specific light or dark resources provided as needed. -->
                            <muxm:AcrylicBrush x:Key="NavigationViewDefaultPaneBackground"
                                   BackgroundSource="Backdrop"
                                   TintColor="LightSlateGray"
                                   TintOpacity=".6"/>
                            <muxm:AcrylicBrush x:Key="NavigationViewTopPaneBackground"
                                   BackgroundSource="Backdrop"
                                   TintColor="{ThemeResource SystemAccentColor}"
                                   TintOpacity=".6"/>
                            <LinearGradientBrush x:Key="NavigationViewExpandedPaneBackground"
                                     StartPoint="0.5,0" EndPoint="0.5,1">
                                <GradientStop Color="LightSlateGray" Offset="0.0" />
                                <GradientStop Color="White" Offset="1.0" />
                            </LinearGradientBrush>
                        </ResourceDictionary>
                        <ResourceDictionary x:Key="HighContrast">
                            <!-- Always include a "HighContrast" dictionary when you override
                                 theme resources. This empty dictionary ensures that the 
                                 default high contrast resources are used when the user
                                 turns on high contrast mode. -->
                        </ResourceDictionary>
                    </ResourceDictionary.ThemeDictionaries>
                </ResourceDictionary>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

### <a name="top-whitespace"></a>Espace supérieur

> La propriété `IsTitleBarAutoPaddingEnabled` nécessite la [bibliothèque d’IU Windows](/uwp/toolkits/winui/) 2.2 ou version ultérieure.

Certaines applications choisissent de [personnaliser la barre de titre de leur fenêtre](../shell/title-bar.md), en étendant éventuellement leur contenu dans la zone de barre de titre. Quand NavigationView est l’élément racine dans les applications qui étendent dans la barre de titre  **en utilisant l’API [ExtendViewIntoTitleBar](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.extendviewintotitlebar)** ,le contrôle ajuste automatiquement la position de ses éléments interactifs pour éviter le chevauchement avec la [zone pouvant être glissée](../shell/title-bar.md#draggable-regions).

![Application qui s’étend dans la barre de titre](images/navigation-view-with-titlebar-padding.png)

Si votre application spécifie la région qui peut être déplacée par glisser en appelant la méthode [Window.SetTitleBar](/uwp/api/windows.ui.xaml.window.settitlebar) et que vous préférez que les boutons précédent et de menu soient plus près du haut de la fenêtre de votre application, définissez [IsTitleBarAutoPaddingEnabled](/uwp/api/microsoft.ui.xaml.controls.navigationview.istitlebarautopaddingenabled) sur **false**.

![Application qui s’étend dans la barre de titre sans remplissage supplémentaire](images/navigation-view-no-titlebar-padding.png)

```Xaml
<muxc:NavigationView x:Name="NavView" IsTitleBarAutoPaddingEnabled="False">
```

#### <a name="remarks"></a>Remarks
Pour ajuster davantage la position de la zone d’en-tête de NavigationView, remplacez la ressource de thème XAML *NavigationViewHeaderMargin*, par exemple dans vos ressources de page.

```Xaml
<Page.Resources>
    <Thickness x:Key="NavigationViewHeaderMargin">12,0</Thickness>
</Page.Resources>
```

Cette ressource de thème modifie la marge autour de [NavigationView.Header](/uwp/api/windows.ui.xaml.controls.navigationview.header).

## <a name="related-topics"></a>Rubriques connexes

- [Classe NavigationView](/uwp/api/windows.ui.xaml.controls.navigationview)
- [Maître/détails](master-details.md)
- [Notions de base sur la navigation](../basics/navigation-basics.md)
- [Vue d’ensemble de Fluent Design](/windows/apps/fluent-design-system)