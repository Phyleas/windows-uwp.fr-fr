---
Description: TwoPaneView est un contrôle de disposition vous permettant de gérer l’affichage d’applications ayant 2 zones de contenu distinctes.
title: Vue à deux volets
template: detail.hbs
ms.date: 01/22/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7a070a72324408746f67b8814554160a76ee0ce4
ms.sourcegitcommit: e4b48989c91cd77ba73c90d9eb9cd67b88d52f21
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2020
ms.locfileid: "79191652"
---
# <a name="two-pane-view"></a>Vue à deux volets

[TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview) est un contrôle de disposition qui vous permet de gérer l’affichage d’applications ayant 2 zones de contenu distinctes, comme une vue maître/détail.

> [!IMPORTANT]
> Les fonctionnalités et l’aide décrites dans cet article sont en préversion publique et peuvent faire l’objet de modifications importantes avant leur lancement en disponibilité générale. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.

Bien qu’il fonctionne sur tous les appareils Windows, le contrôle TwoPaneView est conçu pour vous aider à tirer pleinement parti des appareils double écran de manière automatique, sans aucune programmation spéciale. Sur un appareil double écran, la vue à deux volets garantit que l’interface utilisateur est correctement divisée pour prendre en charge l’espace entre les écrans et que le contenu est présenté de chaque côté de cet espace.

> [!NOTE]
> Un _appareil double écran_ est un type spécial d’appareil qui présente des fonctionnalités uniques. Il ne s’agit pas d’un appareil de bureau équipé de plusieurs moniteurs. Pour plus d’informations, consultez cette [présentation des appareils double écran](/dual-screen/introduction). (Pour plus d’informations sur les façons dont vous pouvez optimiser votre application pour plusieurs moniteurs, consultez [Afficher plusieurs vues](/windows/uwp/design/layout/show-multiple-views).)

| Obtenir la bibliothèque d’interface utilisateur Windows |
| - |
| Ce contrôle est inclus dans la bibliothèque d’interface utilisateur Windows, package NuGet qui contient les nouveaux contrôles et fonctionnalités d’interface utilisateur pour les applications UWP. Pour plus d’informations, notamment des instructions d’installation, consultez [Vue d’ensemble de la bibliothèque d’interface utilisateur Windows](/uwp/toolkits/winui/). |

| API de plateforme | API de la bibliothèque d’interface utilisateur Windows |
| - | - |
| [TwoPaneView, classe](/uwp/api/windows.ui.xaml.controls.twopaneview) | [TwoPaneView, classe](/uwp/api/microsoft.ui.xaml.controls.twopaneview) |

Tout au long de ce document, nous allons utiliser l’alias **muxc** en XAML pour représenter les API de la bibliothèque d’interface utilisateur Windows que nous avons incluses dans notre projet. Nous l’avons ajouté à notre élément [Page](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page) :

```xaml
xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
```

Dans le code-behind, nous allons également utiliser l’alias **muxc** en C# pour représenter les API de la bibliothèque d’interface utilisateur Windows que nous avons incluses dans notre projet. Nous avons ajouté cette instruction **using** en haut du fichier :

```csharp
using muxc = Microsoft.UI.Xaml.Controls;
```

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

Utilisez la vue à deux volets lorsque vous avez 2 zones de contenu distinctes et que :

- Le contenu doit être automatiquement réorganisé et redimensionné pour s’adapter au mieux à la fenêtre.
- La zone de contenu secondaire doit être affichée ou non en fonction de l’espace disponible.
- Le contenu doit être réparti correctement entre les 2 écrans d’un appareil double écran.

## <a name="examples"></a>Exemples

Ces images montrent une application exécutée sur un seul écran et étendue sur deux écrans. La vue à deux volets adapte l’interface utilisateur de l’application aux différentes configurations d’écran.

![Vue à deux volets d’une application sur un seul écran](images/two-pane-view/tpv-single.png)

> _Application sur un seul écran._

![Vue à deux volets d’une application sur deux écrans en mode horizontal](images/two-pane-view/tpv-dual-wide.png)

> _Application sur un appareil double écran en mode horizontal._

![Vue à deux volets d’une application sur deux écrans en mode vertical](images/two-pane-view/tpv-dual-tall.png)

> _Application sur un appareil double écran en mode vertical._

## <a name="how-it-works"></a>Fonctionnement

La vue à deux volets se compose de deux volets où vous placez votre contenu. Elle ajuste la taille et la disposition des volets en fonction de l’espace dont bénéficie la fenêtre. Les dispositions de volet possibles sont définies par l’énumération [TwoPaneViewMode](/uwp/api/microsoft.ui.xaml.controls.twopaneviewmode) :

| Enum&nbsp;value | Description |
| - | - |
| `SinglePane` | Un seul volet est affiché, comme spécifié par la propriété [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority). |
| `Wide` | Les volets sont affichés côte à côte ou un seul volet est affiché, comme spécifié par la propriété [WideModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.widemodeconfiguration). |
| `Tall` | Les volets sont affichés de haut en bas ou un seul volet est affiché, comme spécifié par la propriété [TallModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.tallmodeconfiguration). |

Pour configurer la vue à deux volets, commencez par définir [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) pour spécifier le volet à afficher s’il n’y a de la place que pour un seul volet. Indiquez ensuite si le volet `Pane1` apparaît en haut ou en bas dans les fenêtres en mode vertical, ou à gauche ou à droite dans les fenêtres en mode horizontal.

La vue à deux volets gère la taille et la disposition des volets, mais vous devez toujours faire en sorte que le contenu à l’intérieur du volet s’adapte aux changements de taille et d’orientation. Pour plus d’informations sur la création d’une interface utilisateur adaptative, consultez [Dispositions dynamiques avec XAML](/windows/uwp/design/layout/layouts-with-xaml) et [Panneaux de disposition](/windows/uwp/design/layout/layout-panels).

[TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview) gère l’affichage des volets en fonction de l’état de répartition de l’application.

- Sur un seul écran

    Si votre application s’affiche sur un seul écran, `TwoPaneView` adapte la taille et la position de ses volets d’après les paramètres des propriétés que vous spécifiez. Ces propriétés sont traitées plus en détail dans la section suivante. La seule différence entre les appareils tient au fait que certains appareils, comme les PC de bureau, autorisent les fenêtres redimensionnables alors que d’autres ne les autorisent pas.

- Répartie sur deux écrans

    `TwoPaneView` facilite l’optimisation de votre interface utilisateur en la répartissant sur les appareils double écran. La fenêtre se redimensionne automatiquement afin d’utiliser tout l’espace disponible sur les écrans. Si votre application couvre les deux écrans d’un appareil double écran, chaque écran affiche le contenu de l’un des volets en tenant compte de l’espace entre les écrans. La vue à deux volets intègre la reconnaissance de l’étendue. Il vous suffit de définir la configuration verticale/horizontale pour spécifier quel volet s’affiche sur quel écran. La vue à deux volets se charge du reste.

## <a name="how-to-use-the-two-pane-view-control"></a>Comment utiliser le contrôle de vue à deux volets

[TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview) ne doit pas nécessairement être l’élément racine de votre mise en page. En fait, il est souvent placé à l’intérieur d’un contrôle [NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview) qui fournit la navigation globale pour votre application. Le contrôle `TwoPaneView` s’adapte en conséquence, quel que soit son emplacement dans l’arborescence XAML. Toutefois, nous vous déconseillons d’imbriquer un contrôle `TwoPaneView` dans un autre contrôle `TwoPaneView`. (Si vous le faites, seul le contrôle `TwoPaneView` externe prend en compte la répartition.)

### <a name="add-content-to-the-panes"></a>Ajouter du contenu aux volets

Chaque volet d’une vue à deux volets peut contenir un seul `UIElement` XAML. Pour ajouter du contenu, vous placez généralement un panneau de disposition XAML dans chaque volet, puis vous ajoutez d’autres contrôles et du contenu au panneau. Les volets pouvant changer de taille et alterner entre mode horizontal et mode vertical, vous devez vérifier que le contenu de chaque volet peut s’adapter à ces changements. Pour plus d’informations sur la création d’une interface utilisateur adaptative, consultez [Dispositions dynamiques avec XAML](/windows/uwp/design/layout/layouts-with-xaml) et [Panneaux de disposition](/windows/uwp/design/layout/layout-panels).

L’exemple présenté ici crée l’interface utilisateur d’une application simple avec une image et des informations (celle que vous avez vue précédemment dans la section _Exemples_). Quand l’application est répartie sur deux écrans, l’image et les informations sont affichées sur des écrans distincts. Sur un seul écran, le contenu peut être affiché dans deux volets ou combiné dans un volet unique en fonction de la place disponible. (S’il n’y a de la place que pour un seul volet, le contenu de Pane2 est déplacé dans Pane1 et l’utilisateur doit faire défiler le volet pour voir le contenu masqué. Ce code est présenté plus loin dans la section _Répondre aux changements de mode_.)

![Petite image d’un exemple d’application répartie sur deux écrans](images/two-pane-view/tpv-left-right.png)

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" MinHeight="40"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>

    <CommandBar DefaultLabelPosition="Right">
        <AppBarButton x:Name="Share" Icon="Share" Label="Share" Click="Share_Click"/>
        <AppBarButton x:Name="Print" Icon="Print" Label="Print" Click="Print_Click"/>
    </CommandBar>

    <muxc:TwoPaneView
        x:Name="MyTwoPaneView"
        Grid.Row="1"
        MinWideModeWidth="959"
        MinTallModeHeight="863"
        ModeChanged="TwoPaneView_ModeChanged">

        <muxc:TwoPaneView.Pane1>
            <Grid x:Name="Pane1Root">
                <ScrollViewer>
                    <StackPanel x:Name="Pane1StackPanel">
                        <Image Source="Assets\LandscapeImage8.jpg"
                               VerticalAlignment="Top" HorizontalAlignment="Center"
                               Margin="16,0"/>
                    </StackPanel>
                </ScrollViewer>
            </Grid>
        </muxc:TwoPaneView.Pane1>

        <muxc:TwoPaneView.Pane2>
            <Grid x:Name="Pane2Root">
                <ScrollViewer x:Name="DetailsContent">
                    <StackPanel Padding="16">
                        <TextBlock Text="Mountain.jpg" MaxLines="1"
                                       Style="{ThemeResource HeaderTextBlockStyle}"/>
                        <TextBlock Text="Date Taken:"
                                       Style="{ThemeResource SubheaderTextBlockStyle}"
                                       Margin="0,24,0,0"/>
                        <TextBlock Text="8/29/2019 9:55am"
                                       Style="{ThemeResource SubtitleTextBlockStyle}"/>
                        <TextBlock Text="Dimensions:"
                                       Style="{ThemeResource SubheaderTextBlockStyle}"
                                       Margin="0,24,0,0"/>
                        <TextBlock Text="800x536"
                                       Style="{ThemeResource SubtitleTextBlockStyle}"/>
                        <TextBlock Text="Resolution:"
                                       Style="{ThemeResource SubheaderTextBlockStyle}"
                                       Margin="0,24,0,0"/>
                        <TextBlock Text="96 dpi"
                                       Style="{ThemeResource SubtitleTextBlockStyle}"/>
                        <TextBlock Text="Description:"
                                       Style="{ThemeResource SubheaderTextBlockStyle}"
                                       Margin="0,24,0,0"/>
                        <TextBlock Text="Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Maecenas porttitor congue massa. Fusce posuere, magna sed pulvinar ultricies, purus lectus malesuada libero, sit amet commodo magna eros quis urna."
                                       Style="{ThemeResource SubtitleTextBlockStyle}"
                                       TextWrapping="Wrap"/>
                    </StackPanel>
                </ScrollViewer>
            </Grid>
        </muxc:TwoPaneView.Pane2>
    </muxc:TwoPaneView>

    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="TwoPaneViewStates">
            <VisualState x:Name="Normal"/>
            <VisualState x:Name="Wide">
                <VisualState.Setters>
                    <Setter Target="MyTwoPaneView.Pane1Length"
                            Value="2*"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>
```

### <a name="specify-which-pane-to-display"></a>Spécifier le volet à afficher

Si le contrôle de vue à deux volets ne peut afficher qu’un seul volet, il utilise la propriété [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) pour déterminer quel volet afficher. Par défaut, PanePriority a la valeur **Pane1**. Voici comment vous pouvez définir cette propriété en XAML ou dans le code.

```xaml
<muxc:TwoPaneView x:Name="MyTwoPaneView" PanePriority="Pane2">
```

```csharp
MyTwoPaneView.PanePriority = Microsoft.UI.Xaml.Controls.TwoPaneViewPriority.Pane2;
```

### <a name="pane-sizing"></a>Dimensionnement des volets

Sur un seul écran, la taille des volets est déterminée par les propriétés [Pane1Length](/uwp/api/microsoft.ui.xaml.controls.twopaneview.pane1length) et [Pane2Length](/uwp/api/microsoft.ui.xaml.controls.twopaneview.pane2length). Celles-ci utilisent les valeurs de [GridLength](/uwp/api/windows.ui.xaml.gridlength) qui prennent en charge les redimensionnements _auto_ et \* (_étoile_ ou proportionnel). Pour obtenir une explication des redimensionnements auto et *, consultez la section _Propriétés de disposition_ de l’article [Dispositions dynamiques avec XAML](/windows/uwp/design/layout/layouts-with-xaml#layout-properties).

Par défaut, `Pane1Length` est défini sur `Auto` et se redimensionne automatiquement en fonction de son contenu. `Pane2Length` est défini sur `*` et utilise tout l’espace restant.

![Vue à deux volets définis à des tailles par défaut](images/two-pane-view/tpv-size-default.png)

> _Volets avec redimensionnement par défaut_

Les valeurs par défaut sont utiles pour une disposition standard de type maître/détail, avec une liste d’éléments dans `Pane1` et de nombreux détails dans `Pane2`. Toutefois, en fonction de votre contenu, vous préférerez peut-être diviser l’espace différemment. Dans ce cas, `Pane1Length` est défini sur `2*` pour avoir deux fois plus d’espace que `Pane2`.

```xaml
<muxc:TwoPaneView x:Name="MyTwoPaneView" Pane1Length="2*">
```

![Vue à deux volets avec le volet 1 sur les deux tiers de l’écran et le volet 2 sur le tiers restant](images/two-pane-view/tpv-size-2.png)

> _Volets dimensionnés avec 2* et *_

> [!NOTE]
> Comme mentionné précédemment, quand l’application est répartie sur deux écrans, ces propriétés sont ignorées et chaque volet remplit l’un des écrans.

Si vous définissez un volet avec redimensionnement automatique, vous pouvez contrôler la taille en définissant la hauteur et la largeur du panneau où se trouve le contenu du volet. Dans ce cas, vous devrez peut-être gérer l’événement ModeChanged et définir les contraintes de hauteur et de largeur du contenu en fonction du mode actuel.

### <a name="display-in-wide-or-tall-mode"></a>Afficher en mode vertical ou horizontal

Sur un seul écran, le [mode](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mode) d’affichage de la vue à deux volets est déterminé par les propriétés [MinWideModeWidth](/uwp/api/microsoft.ui.xaml.controls.twopaneview.minwidemodewidth) et [MinTallModeHeight](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mintallmodeheight). Les deux propriétés ont une valeur par défaut de 641px, comme [NavigationView.CompactThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth).

Ce tableau montre de quelle façon les propriétés Height et Width du TwoPaneView déterminent le mode d’affichage utilisé.

| Condition TwoPaneView  | Mode |
|---------|---------|
| `Width` > `MinWideModeWidth` | Le mode `Wide` est utilisé |
| `Width` <= `MinWideModeWidth`, et `Height` > `MinTallModeHeight` | Le mode `Tall` est utilisé |
| `Width` <= `MinWideModeWidth`, et `Height` <= `MinTallModeHeight` | Le mode `SinglePane` est utilisé |


> [!NOTE]
> Comme mentionné précédemment, quand l’application est répartie sur deux écrans, ces propriétés sont ignorées et le mode d’affichage est déterminé en fonction de la _posture_ de l’appareil.

#### <a name="wide-configuration-options"></a>Options de configuration du mode horizontal

La vue à deux volets passe en mode `Wide` quand la largeur d’une fenêtre est supérieure à la valeur `MinWideModeWidth`. `MinWideModeWidth` contrôle à quel moment la vue à deux volets passe en mode horizontal. La valeur par défaut est 641px, mais vous pouvez la remplacer par la valeur de votre choix. En général, vous définissez cette propriété pour qu’elle soit égale à la largeur minimale souhaitée de votre volet.

Quand la vue à deux volets est en mode horizontal, la propriété [WideModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.widemodeconfiguration) détermine ce qu’il faut afficher :

| [Enum&nbsp;value](/uwp/api/microsoft.ui.xaml.controls.twopaneviewwidemodeconfiguration) | Description |
|---------|---------|
| `SinglePane` | Un seul volet (déterminé par `PanePriority`). Le volet occupe toute la taille du `TwoPaneView` (redimensionnement proportionnel dans les deux directions). |
| `LeftRight` | `Pane1` à gauche et `Pane2` à droite. Les deux volets sont redimensionnés verticalement de manière proportionnelle ; la largeur de `Pane1` est redimensionnée automatiquement et la largeur de `Pane2` est redimensionnée de manière proportionnelle. |
| `RightLeft` | `Pane1` à droite et `Pane2` à gauche. Les deux volets sont redimensionnés verticalement de manière proportionnelle ; la largeur de `Pane2` est redimensionnée automatiquement et la largeur de `Pane1` est redimensionnée de manière proportionnelle. |

La valeur par défaut est `LeftRight`.

| LeftRight | RightLeft |
| - | - |
| ![Vue à deux volets avec une configuration gauche-droite](images/two-pane-view/tpv-left-right.png)  | ![Vue à deux volets avec une configuration droite-gauche](images/two-pane-view/tpv-right-left.png)  |

> **CONSEIL :** Quand l’appareil utilise une langue qui s’écrit de droite à gauche (DàG), la vue à deux volets change automatiquement l’ordre : `RightLeft` devient `LeftRight` et `LeftRight` devient `RightLeft`.

#### <a name="tall-configuration-options"></a>Options de configuration du mode vertical

La vue à deux volets passe en mode `Tall` quand la largeur d’une fenêtre est inférieure à la valeur `MinWideModeWidth` et que sa hauteur dépasse la valeur `MinTallModeHeight`. La valeur par défaut est 641px, mais vous pouvez la remplacer par la valeur de votre choix. En général, vous définissez cette propriété pour qu’elle soit égale à la hauteur minimale souhaitée de votre volet.

Quand la vue à deux volets est en mode vertical, la propriété [TallModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.tallmodeconfiguration) détermine ce qu’il faut afficher :

| [Enum&nbsp;value](/uwp/api/microsoft.ui.xaml.controls.twopaneviewtallmodeconfiguration) | Description |
|---------|---------|
| `SinglePane` | Un seul volet (déterminé par `PanePriority`). Le volet occupe toute la taille du `TwoPaneView` (redimensionnement proportionnel dans les deux directions). |
| `TopBottom` | `Pane1` en haut et `Pane2` en bas. Les deux volets sont redimensionnés horizontalement de manière proportionnelle ; la hauteur de `Pane1` est redimensionnée automatiquement et la hauteur de `Pane2` est redimensionnée de manière proportionnelle. |
| `BottomTop` | `Pane1` en bas et `Pane2` en haut. Les deux volets sont redimensionnés horizontalement de manière proportionnelle ; la hauteur de `Pane2` est redimensionnée automatiquement et la hauteur de `Pane1` est redimensionnée de manière proportionnelle. |

La valeur par défaut est `TopBottom`.

| TopBottom | BottomTop |
| - | - |
| ![Vue à deux volets avec une configuration haut-bas](images/two-pane-view/tpv-top-bottom.png)  | ![Vue à deux volets avec une configuration bas-haut](images/two-pane-view/tpv-bottom-top.png)  |

#### <a name="special-values-for-minwidemodewidth-and-mintallmodeheight"></a>Valeurs spéciales pour MinWideModeWidth et MinTallModeHeight

Vous pouvez utiliser la propriété `MinWideModeWidth` pour empêcher la vue à deux volets d’entrer en mode Wide (horizontal). Pour cela, définissez simplement `MinWideModeWidth` sur [Double.PositiveInfinity](/dotnet/api/system.double.positiveinfinity?view=dotnet-uwp-10.0).

Si vous définissez `MinTallModeHeight` sur [Double.PositiveInfinity](/dotnet/api/system.double.positiveinfinity?view=dotnet-uwp-10.0), la vue à deux volets n’entre pas en mode Tall (vertical).

Si vous définissez `MinTallModeHeight` sur zéro (0), la vue à deux volets n’entre pas en mode `SinglePane`.

#### <a name="responding-to-mode-changes"></a>Répondre aux changements de mode

Vous pouvez utiliser la propriété en lecture seule [Mode](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mode) pour obtenir le mode d’affichage actuel. Chaque fois que la vue à deux volets change le ou les volets affichés, l’événement [ModeChanged](/uwp/api/microsoft.ui.xaml.controls.twopaneview.modechanged) se produit avant l’affichage du contenu mis à jour. Vous pouvez gérer l’événement pour répondre aux changements du mode d’affichage.

> [!TIP]
> L’événement `ModeChanged` ne se produit pas au moment du chargement initial de la page. Votre code XAML par défaut doit donc représenter l’interface utilisateur telle qu’elle est censée apparaître au premier chargement.

Vous pouvez notamment utiliser cet événement pour mettre à jour l’interface utilisateur de votre application afin que les utilisateurs puissent voir tout le contenu en mode `SinglePane`. Par exemple, l’exemple d’application a un volet principal (l’image) et un volet d’informations.

![Petite image d’un exemple d’application répartie en mode Tall (vertical)](images/two-pane-view/tpv-top-bottom.png)

> _Mode Tall_ (vertical)

S’il n’y a de la place que pour un seul volet, vous pouvez déplacer le contenu de `Pane2` vers `Pane1` afin que l’utilisateur puisse faire défiler le volet pour voir tout le contenu. Cette dernière se présente comme suit.

![Image d’un exemple d’application sur un écran avec tout le contenu défilant dans un seul volet](images/two-pane-view/tpv-single-pane.png)

> _Mode SinglePane_

N’oubliez pas que les propriétés `MinWideModeWidth` et `MinTallModeHeight` déterminent quand changer le mode d’affichage. Vous pouvez donc changer le moment où le contenu est déplacé entre les volets en ajustant les valeurs de ces propriétés.

Voici le code du gestionnaire d’événements `ModeChanged` qui déplace le contenu de `Pane1` vers `Pane2`. Il définit également un [VisualState](/uwp/api/windows.ui.xaml.visualstate) qui force l’affichage de l’image en mode Wide (horizontal).

```csharp
private void TwoPaneView_ModeChanged(Microsoft.UI.Xaml.Controls.TwoPaneView sender, object args)
{
    // Remove details content from it's parent panel.
    ((Panel)DetailsContent.Parent).Children.Remove(DetailsContent);
    // Set Normal visual state.
    Windows.UI.Xaml.VisualStateManager.GoToState(this, "Normal", true);

    // Single pane
    if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.SinglePane)
    {
        // Add the details content to Pane1.
        Pane1StackPanel.Children.Add(DetailsContent);
    }
    // Dual pane.
    else
    {
        // Put details content in Pane2.
        Pane2Root.Children.Add(DetailsContent);

        // If also in Wide mode, set Wide visual state
        // to constrain the width of the image to 2*.
        if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.Wide)
        {
            Windows.UI.Xaml.VisualStateManager.GoToState(this, "Wide", true);
        }
    }
}
```

## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées

- Utilisez la vue à deux volets dans la mesure du possible pour que votre application s’affiche de manière optimale sur des appareils à double écran et à grand écran.
- Ne placez pas une vue à deux volets à l’intérieur d’une autre vue à deux volets.

## <a name="related-articles"></a>Articles connexes

- [Vue d’ensemble de la disposition](../layout/index.md)
- [Développement pour appareils double écran](/dual-screen)
- [Présentation des appareils double écran](/dual-screen/introduction)
