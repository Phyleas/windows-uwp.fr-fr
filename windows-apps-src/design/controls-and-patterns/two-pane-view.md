---
Description: TwoPaneView est un contrôle de disposition vous permettant de gérer l’affichage d’applications ayant 2 zones de contenu distinctes.
title: Vue à deux volets
template: detail.hbs
ms.date: 01/22/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 67b97aec970cc655700729743f10c63c666ab0a6
ms.sourcegitcommit: 09571e1c6a01fabed773330aa7ead459a47d94f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76929259"
---
# <a name="two-pane-view"></a>Vue à deux volets

[TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview) est un contrôle de disposition qui vous permet de gérer l’affichage d’applications ayant 2 zones de contenu distinctes, comme une vue maître/détail.

> [!IMPORTANT]
> Les fonctionnalités et l’aide décrites dans cet article sont en préversion publique et peuvent faire l’objet de modifications importantes avant leur lancement en disponibilité générale. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.

Bien qu’il fonctionne sur tous les appareils Windows, le contrôle TwoPaneView est conçu pour vous aider à tirer pleinement parti des appareils double écran de manière automatique, sans aucune programmation spéciale. Sur un appareil double écran, la vue à deux volets garantit que l’interface utilisateur est correctement divisée pour prendre en charge l’espace entre les écrans et que le contenu est présenté de chaque côté de cet espace.

> **REMARQUE :** Un _appareil double écran_ est un type spécial d’appareil qui présente des fonctionnalités uniques. Il ne s’agit pas d’un appareil de bureau équipé de plusieurs moniteurs. Pour plus d’informations, consultez cette [présentation des appareils double écran](/dual-screen/introduction).
>
>Dans cet article, nous utilisons les termes _appareil monoécran_ et _affichage monoécran_ pour désigner un appareil qui n’est pas un appareil double écran, qu’il s’agisse d’un simple moniteur ou d’un moniteur faisant partie d’une configuration à plusieurs écrans. Sur un appareil monoécran, le contrôle TwoPaneView se comporte de la même façon que les autres contrôles XAML. Pour plus d’informations sur les façons dont vous pouvez optimiser votre application pour plusieurs moniteurs, consultez [Afficher plusieurs vues](/windows/uwp/design/layout/show-multiple-views).

| **Obtenir la bibliothèque d’interface utilisateur Windows** |
| - |
| Ce contrôle est inclus dans la bibliothèque d’interface utilisateur Windows, package NuGet qui contient les nouveaux contrôles et fonctionnalités d’interface utilisateur pour les applications UWP. Pour plus d’informations, notamment des instructions d’installation, consultez [Vue d’ensemble de la bibliothèque d’interface utilisateur Windows](/uwp/toolkits/winui/). |

| **API de plateforme** | **API de la bibliothèque d’interface utilisateur Windows** |
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

Ces images montrent une application en cours d’exécution sur un appareil monoécran et sur un appareil double écran. La vue à deux volets adapte l’interface utilisateur de l’application à différentes configurations de volet sur chaque appareil.

![tpv-single.png](images/two-pane-view/tpv-single.png)

_Application sur un appareil monoécran._

![tpv-dual-wide.png](images/two-pane-view/tpv-dual-wide.png)

_Application sur un appareil double écran en mode horizontal._

![tpv-dual-tall.png](images/two-pane-view/tpv-dual-tall.png)

_Application sur un appareil double écran en mode vertical._

## <a name="how-it-works"></a>Fonctionnement

La vue à deux volets se compose de deux volets où vous placez votre contenu. Elle ajuste la taille et la disposition des volets en fonction de l’espace dont bénéficie la fenêtre. Les dispositions de volet possibles sont définies par l’énumération [TwoPaneViewMode](/uwp/api/microsoft.ui.xaml.controls.twopaneviewmode) :

- **SinglePane** : un seul volet est affiché, comme spécifié par la propriété [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority).
- **Wide** : les volets sont affichés côte à côte ou un seul volet est affiché, comme spécifié par la propriété [WideModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.widemodeconfiguration).
- **Tall** : les volets sont affichés de haut en bas ou un seul volet est affiché, comme spécifié par la propriété [TallModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.tallmodeconfiguration).

Pour configurer la vue à deux volets, commencez par définir [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) pour spécifier le volet à afficher s’il n’y a de la place que pour un seul volet. Indiquez ensuite si le premier volet (Pane1) apparaît en haut ou en bas pour les fenêtres en mode vertical, ou à gauche ou à droite pour les fenêtres en mode horizontal.

La vue à deux volets gère la taille et la disposition des volets, mais vous devez toujours faire en sorte que le contenu à l’intérieur du volet s’adapte aux changements de taille et d’orientation. Pour plus d’informations sur la création d’une interface utilisateur adaptative, consultez [Dispositions dynamiques avec XAML](/windows/uwp/design/layout/layouts-with-xaml) et [Panneaux de disposition](/windows/uwp/design/layout/layout-panels).

La vue à deux volets gère l’affichage des volets en fonction du type d’appareil sur lequel l’application s’exécute :

- Sur un appareil double écran

    La vue à deux volets est conçue pour vous aider à optimiser votre interface utilisateur sur les appareils double écran. La fenêtre se redimensionne automatiquement afin d’utiliser tout l’espace disponible sur les écrans. Si votre application ne se trouve que sur l’un des écrans de l’appareil, un seul volet s’affiche, comme spécifié par la propriété [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority).

    Si votre application couvre les deux écrans d’un appareil double écran, chaque écran affiche le contenu de l’un des volets en tenant compte de l’espace entre les écrans. La vue à deux volets intègre la reconnaissance de l’étendue. Il vous suffit de définir la configuration verticale/horizontale pour spécifier quel volet s’affiche sur quel écran. La vue à deux volets se charge du reste.


- Sur les appareils monoécrans

    Quand vous utilisez un appareil monoécran comme un ordinateur portable ou un ordinateur de bureau, la vue à deux volets se comporte comme tout autre contrôle XAML. Si la fenêtre est redimensionnée, la vue à deux volets ajuste la taille et la position de ses volets en fonction de la taille de la fenêtre. Des propriétés supplémentaires vous permettent de définir le comportement du contrôle quand il est affiché sur un seul écran dans une fenêtre redimensionnable. Ces propriétés sont traitées plus en détail dans la section suivante.

## <a name="how-to-use-the-two-pane-view-control"></a>Comment utiliser le contrôle de vue à deux volets

[TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview) ne doit pas nécessairement être l’élément racine de votre mise en page. En fait, il est souvent placé à l’intérieur d’un contrôle [NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview) qui fournit la navigation globale pour votre application. TwoPaneView adapte l’affichage en conséquence, quel que soit son emplacement dans l’arborescence XAML. Toutefois, nous vous déconseillons d’imbriquer un contrôle TwoPaneView dans un autre contrôle TwoPaneView.

### <a name="add-content-to-the-panes"></a>Ajouter du contenu aux volets

Chaque volet d’une vue à deux volets peut contenir un seul UIElement XAML. Pour ajouter du contenu, vous placez généralement un panneau de disposition XAML dans chaque volet, puis vous ajoutez d’autres contrôles et du contenu au panneau. Les volets pouvant changer de taille et alterner entre mode horizontal et mode vertical, vous devez vérifier que le contenu de chaque volet peut s’adapter à ces changements. Pour plus d’informations sur la création d’une interface utilisateur adaptative, consultez [Dispositions dynamiques avec XAML](/windows/uwp/design/layout/layouts-with-xaml) et [Panneaux de disposition](/windows/uwp/design/layout/layout-panels).

L’exemple présenté ici crée l’interface utilisateur d’une application simple qui contient une image et des informations. S’il y a de la place pour deux volets, l’image et les informations sont affichées dans des volets distincts. (S’il n’y a de la place que pour un seul volet, le contenu de Pane2 est déplacé dans Pane1 et l’utilisateur doit faire défiler le volet pour voir le contenu masqué. Ce code est présenté plus loin dans la section _Répondre aux changements de mode_.)

![tpv-add-content.png](images/two-pane-view/tpv-add-content.png)

```xaml
<muxc:TwoPaneView
    Pane1Length="*"
    ModeChanged="TwoPaneView_ModeChanged">

    <muxc:TwoPaneView.Pane1>
        <Grid x:Name="Pane1Root">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto" MinHeight="40"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>

            <CommandBar x:Name="MyCommandBar" DefaultLabelPosition="Right">
                <AppBarButton x:Name="Share" Icon="Share" Label="Share"/>
                <AppBarButton x:Name="Print" Icon="Print" Label="Print"/>
            </CommandBar>

            <ScrollViewer Grid.Row="1">
                <StackPanel x:Name="Pane1StackPanel">
                    <Image x:Name="TheImage" Source="Assets\LandscapeImage8.jpg"
                           VerticalAlignment="Top" HorizontalAlignment="Center" 
                           Margin="16,0"/>
                </StackPanel>
            </ScrollViewer>

        </Grid>
    </muxc:TwoPaneView.Pane1>

    <muxc:TwoPaneView.Pane2>
        <Grid x:Name="Pane2Root">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto" MinHeight="40"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <StackPanel x:Name="DetailsContent" Grid.Row="1"
                Orientation="Vertical" Padding="16">

                <TextBlock Text="Mountain.jpg" Margin="0,0,0,12"
                   FontWeight="SemiBold" FontSize="18"/>

                <TextBlock Text="Date Taken:" FontWeight="SemiBold"/>
                <TextBlock Text="8/29/2019 9:55am" Margin="0,0,0,12"/>

                <TextBlock Text="Dimensions:" FontWeight="SemiBold"/>
                <TextBlock Text="1000x750" Margin="0,0,0,12"/>

                <TextBlock Text="Resolution:" FontWeight="SemiBold"/>
                <TextBlock Text="96 dpi" Margin="0,0,0,12"/>

                <TextBlock Text="Description:" FontWeight="SemiBold"/>
                <TextBlock TextWrapping="Wrap" 
                           Text="Lorem ipsum dolor sit amet."/>
            </StackPanel>
        </Grid>
    </muxc:TwoPaneView.Pane2>
</muxc:TwoPaneView>
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

La taille des volets sur un appareil monoécran est déterminée par les propriétés [Pane1Length](/uwp/api/microsoft.ui.xaml.controls.twopaneview.pane1length) et [Pane2Length](/uwp/api/microsoft.ui.xaml.controls.twopaneview.pane2length). Celles-ci utilisent les valeurs de [GridLength](/uwp/api/windows.ui.xaml.gridlength) qui prennent en charge les redimensionnements _auto_ et \* (_étoile_ ou proportionnel). Pour obtenir une explication des redimensionnements auto et *, consultez la section _Propriétés de disposition_ de l’article [Dispositions dynamiques avec XAML](/windows/uwp/design/layout/layouts-with-xaml#layout-properties).

Par défaut, Pane1Length a la valeur **Auto** et se redimensionne en fonction de son contenu. Pane2Length a la valeur * et utilise tout l’espace restant.

![tpv-size-default.png](images/two-pane-view/tpv-size-default.png)

_Volets avec redimensionnement par défaut_

Les valeurs par défaut sont utiles pour une disposition standard de type maître/détail, avec une liste d’éléments dans Pane1 et de nombreux détails dans Pane2. Toutefois, en fonction de votre contenu, vous préférerez peut-être diviser l’espace différemment. Ici, Pane1Length a la valeur 2* et a deux fois plus d’espace que Pane2.

```xaml
<muxc:TwoPaneView x:Name="MyTwoPaneView" Pane1Length="2*">
```

![tpv-size-2.png](images/two-pane-view/tpv-size-2.png)

_Volets dimensionnés avec 2* et *_

> **REMARQUE :** Comme mentionné précédemment, sur un appareil double écran, ces propriétés sont ignorées et les volets sont redimensionnés et disposés automatiquement en fonction de la _posture_ de l’appareil.

Si vous définissez un volet avec redimensionnement automatique, vous pouvez contrôler la taille en définissant la hauteur et la largeur du panneau où se trouve le contenu du volet. Dans ce cas, vous devrez peut-être gérer l’événement ModeChanged et définir les contraintes de hauteur et de largeur du contenu en fonction du mode actuel.

### <a name="display-in-wide-or-tall-mode"></a>Afficher en mode vertical ou horizontal

Sur un ordinateur de bureau, le [mode](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mode) d’affichage de la vue à deux volets est déterminé par les propriétés [MinWideModeWidth](/uwp/api/microsoft.ui.xaml.controls.twopaneview.minwidemodewidth) et [MinTallModeHeight](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mintallmodeheight). Les deux propriétés ont une valeur par défaut de 641px, comme [NavigationView.CompactThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth).

Si la fenêtre est :

- Plus large que MinWideModeWidth, le mode **Wide** (horizontal) est utilisé.
- Plus étroite que MinWideModeWidth et plus haute que MinTallModeHeight, le mode **Tall** (vertical) est utilisé.
- Plus étroite que MinWideModeWidth et plus petite que MinTallModeHeight, le mode **SinglePane** (un seul volet) est utilisé.

> **REMARQUE :** Comme mentionné précédemment, sur un appareil double écran, ces propriétés sont ignorées et les volets sont redimensionnés et disposés automatiquement en fonction de la _posture_ de l’appareil.

#### <a name="wide-configuration-options"></a>Options de configuration du mode horizontal

La vue à deux volets passe en mode horizontal quand la largeur d’une fenêtre est supérieure à la propriété MinWideModeWidth. MinWideModeWidth contrôle à quel moment la vue à deux volets passe en mode horizontal. La valeur par défaut est 641px, mais vous pouvez la remplacer par la valeur de votre choix. En général, vous définissez cette propriété pour qu’elle soit égale à la largeur minimale souhaitée de votre volet.

Quand la vue à deux volets est en mode horizontal, la propriété WideModeConfiguration détermine ce qu’il faut afficher :

- **SinglePane** : un seul volet (tel que déterminé par PanePriority). Le volet occupe toute la taille de TwoPaneView (redimensionnement proportionnel dans les deux directions).
- **LeftRight** : Pane1 à gauche et Pane2 à droite. Les deux volets sont redimensionnés verticalement de manière proportionnelle, la largeur de Pane1 est redimensionnée automatiquement et la largeur de Pane2 est redimensionnée de manière proportionnelle.
- **RightLeft** : Pane1 à droite et Pane2 à gauche. Les deux volets sont redimensionnés verticalement de manière proportionnelle, la largeur de Pane2 est redimensionnée automatiquement et la largeur de Pane1 est redimensionnée de manière proportionnelle.

Le paramètre par défaut est **LeftRight**.

| LeftRight | RightLeft |
| - | - |
| ![tpv-left-right.png](images/two-pane-view/tpv-left-right.png)  | ![tpv-right-left.png](images/two-pane-view/tpv-right-left.png)  |

> **CONSEIL :** Quand l’appareil utilise une langue qui s’écrit de droite à gauche (DàG), la vue à deux volets change automatiquement l’ordre : RightLeft devient LeftRight, et LeftRight devient RightLeft.

#### <a name="tall-configuration-options"></a>Options de configuration du mode vertical

La vue à deux volets passe en mode vertical quand une fenêtre est plus étroite que MinWideModeWidth et plus haute que MinTallModeHeight. La valeur par défaut est 641px, mais vous pouvez la remplacer par la valeur de votre choix. En général, vous définissez cette propriété pour qu’elle soit égale à la hauteur minimale souhaitée de votre volet.

Quand la vue à deux volets est en mode vertical, la propriété TallLayout détermine ce qu’il faut afficher :

- **SinglePane** : un seul volet (tel que déterminé par PanePriority). Le volet occupe toute la taille de TwoPaneView (redimensionnement proportionnel dans les deux directions).
- **TopBottom** : Pane1 en haut et Pane2 en bas. Les deux volets sont redimensionnés horizontalement de manière proportionnelle, la hauteur de Pane1 est redimensionnée automatiquement et la hauteur de Pane2 est redimensionnée de manière proportionnelle.
- **BottomTop** : Pane1 en bas et Pane2 en haut. Les deux volets sont redimensionnés horizontalement de manière proportionnelle, la hauteur de Pane2 est redimensionnée automatiquement et la hauteur de Pane1 est redimensionnée de manière proportionnelle.

Le paramètre par défaut est **TopBottom**.

| TopBottom | BottomTop |
| - | - |
| ![tpv-top-bottom.png](images/two-pane-view/tpv-top-bottom.png)  | ![tpv-bottom-top.png](images/two-pane-view/tpv-bottom-top.png)  |

#### <a name="special-values-for-minwidemodewidth-and-mintallmodeheight"></a>Valeurs spéciales pour MinWideModeWidth et MinTallModeHeight

Vous pouvez utiliser la propriété MinWideModeWidth pour empêcher la vue à deux volets de passer en mode Wide (horizontal). Pour cela, affectez simplement [Double.PositiveInfinity](/dotnet/api/system.double.positiveinfinity?view=dotnet-uwp-10.0) à MinWideModeWidth.

Si vous affectez [Double.PositiveInfinity](/dotnet/api/system.double.positiveinfinity?view=dotnet-uwp-10.0) à MinTallModeHeight, la vue à deux volets n’entre pas en mode Tall (vertical).

Si vous affectez 0 à MinTallModeHeight, la vue à deux volets n’entre pas en mode SinglePane.

#### <a name="responding-to-mode-changes"></a>Répondre aux changements de mode

Vous pouvez utiliser la propriété en lecture seule [Mode](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mode) pour obtenir le mode d’affichage actuel. Chaque fois que la vue à deux volets change le ou les volets affichés, l’événement [ModeChanged](/uwp/api/microsoft.ui.xaml.controls.twopaneview.modechanged) se produit avant l’affichage du contenu mis à jour. Vous pouvez gérer l’événement pour répondre aux changements du mode d’affichage.

Vous pouvez notamment utiliser cet événement pour mettre à jour l’interface utilisateur de votre application afin que les utilisateurs puissent voir tout le contenu en mode SinglePane. Par exemple, l’exemple d’application a un volet principal (l’image) et un volet d’informations.

![tpv-add-content.png](images/two-pane-view/tpv-add-content.png)

_Mode horizontal_

S’il n’y a de la place que pour un seul volet, le contenu de Pane2 est déplacé dans Pane1 et l’utilisateur doit faire défiler le volet pour voir tout le contenu. Cette dernière se présente comme suit.

![tpv-mode-change.png](images/two-pane-view/tpv-mode-change.png)

_Mode SinglePane_

```csharp
 private void TwoPaneView_ModeChanged(Microsoft.UI.Xaml.Controls.TwoPaneView sender, object args)
 {
     ((Panel)DetailsContent.Parent).Children.Remove(DetailsContent);
     ((Panel)MyCommandBar.Parent).Children.Remove(MyCommandBar);

     // Single pane
     if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.SinglePane)
     {
         // Add the command bar and details content to Pane1.
         Pane1StackPanel.Children.Add(DetailsContent);
         Pane1Root.Children.Add(MyCommandBar);
     }
     // Dual pane.
     else
     {
         // Wide mode.
         if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.Wide)
         {
             // Put the command bar in Pane2.
             Pane2Root.Children.Add(MyCommandBar);
         }
         // Tall mode.
         else if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.Tall)
         {
             // Put the command bar in Pane1
             Pane1Root.Children.Add(MyCommandBar);
         }

         // Put details content in Pane2.
         Pane2Root.Children.Add(DetailsContent);
     }
 }
```

## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées

- Utilisez la vue à deux volets dans la mesure du possible pour que votre application puisse tirer parti des appareils double écran et des grands écrans.
- Ne placez pas une vue à deux volets à l’intérieur d’une autre vue à deux volets.

## <a name="related-articles"></a>Articles connexes

- [Vue d’ensemble de la disposition](../layout/index.md)
- [Développement pour appareils double écran](/dual-screen)
- [Présentation des appareils double écran](/dual-screen/introduction)
