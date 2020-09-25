---
title: Navigation en mode focus sans souris
Description: Apprenez à utiliser la navigation de focus pour fournir des expériences d’interaction complètes et cohérentes dans vos applications Windows et vos contrôles personnalisés pour les utilisateurs de l’utilisation du clavier, ceux souffrant de handicaps et d’autres exigences d’accessibilité, ainsi que l’expérience de 10 pieds des écrans de télévision et de Xbox.
label: ''
template: detail.hbs
keywords: clavier, contrôleur de jeu, contrôle à distance, navigation, navigation interne directionnelle, zone directionnelle, stratégie de navigation, entrée, interaction utilisateur, accessibilité, convivialité
ms.date: 09/24/2020
ms.topic: article
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 6119a6b7d7621857e3317589a3b4a64ba3d5d2ea
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91217222"
---
# <a name="focus-navigation-for-keyboard-gamepad-remote-control-and-accessibility-tools"></a>Navigation centrée sur le clavier, le boîtier, le contrôle à distance et les outils d’accessibilité

![Clavier, à distance et à D-Pad](images/dpad-remote/dpad-remote-keyboard.png)

Utilisez la navigation de focus pour fournir des expériences d’interaction complètes et cohérentes dans vos applications Windows et vos contrôles personnalisés pour les utilisateurs de l’utilisation du clavier, ceux qui présentent des handicaps et d’autres exigences d’accessibilité, ainsi que l’expérience de 10 pieds des écrans de télévision et de Xbox One.

## <a name="overview"></a>Vue d’ensemble

La navigation de focus fait référence au mécanisme sous-jacent qui permet aux utilisateurs de naviguer et d’interagir avec l’interface utilisateur d’une application Windows à l’aide d’un clavier, d’un boîtier de commande ou d’un contrôle à distance.

> [!NOTE]
> Les périphériques d’entrée sont généralement classés comme des dispositifs de pointage, tels que la touche tactile, le pavé tactile, le stylet et la souris, et les appareils sans pointage, tels que le clavier, le boîtier de commande et le contrôle à distance.

Cette rubrique décrit comment optimiser une application Windows et créer des expériences d’interaction personnalisées pour les utilisateurs qui reposent sur des types d’entrée sans pointage. 

Même si nous nous concentrons sur l’entrée au clavier pour les contrôles personnalisés dans les applications Windows sur les PC, une expérience clavier bien conçue est également importante pour les claviers logiciels tels que le clavier tactile et le clavier visuel (OSK), prenant en charge des outils d’accessibilité tels que le narrateur Windows et prenant en charge l’expérience de 10 pieds.

Pour obtenir des conseils sur la création d’expériences personnalisées dans les applications Windows pour les périphériques de pointage, consultez [gérer l’entrée du pointeur](handle-pointer-input.md) .

Pour plus d’informations générales sur la création d’applications et d’expériences pour le clavier, consultez [interaction avec le clavier](keyboard-interactions.md).

## <a name="general-guidance"></a>Règle générale

Seuls les éléments d’interface utilisateur qui requièrent une interaction de l’utilisateur doivent prendre en charge la navigation du focus, les éléments qui ne nécessitent pas d’action, tels que les images statiques, n’ont pas besoin de focus clavier. Les lecteurs d’écran et les outils d’accessibilité similaires annoncent toujours ces éléments statiques, même s’ils ne sont pas inclus dans la navigation au focus. 

Il est important de se souvenir que contrairement à la navigation avec un dispositif de pointage tel qu’une souris ou une touche tactile, la navigation de focus est linéaire. Lors de l’implémentation de la navigation du focus, réfléchissez à la façon dont un utilisateur interagit avec votre application et ce que doit être la navigation logique. Dans la plupart des cas, nous vous recommandons de suivre le comportement de navigation de focus personnalisé à la suite du modèle de lecture par défaut de la culture de l’utilisateur.

Voici quelques autres considérations relatives à la navigation axée :

- Les contrôles sont-ils regroupés logiquement ?
- Existe-t-il des groupes de contrôles de plus grande importance ?
  - Si oui, ces groupes contiennent-ils des sous-groupes ?
- La disposition nécessite-t-elle une navigation directionnelle personnalisée (touches de direction) et un ordre de tabulation ?

Le [logiciel d’ingénierie pour l’ebook d’accessibilité](https://www.microsoft.com/download/details.aspx?id=19262) présente un excellent chapitre sur *la conception de la hiérarchie logique*.

## <a name="2d-directional-navigation-for-keyboard"></a>navigation directionnelle 2D pour le clavier

La région de navigation interne 2D d’un contrôle, ou groupe de contrôles, est appelée « zone directionnelle ». Lorsque le focus se déplace sur cet objet, les touches de direction du clavier (gauche, droite, haut et bas) peuvent être utilisées pour naviguer entre les éléments enfants dans la zone directionnelle.

![](images/keyboard/directional-area-small.png)
*région de navigation interne 2D de la zone de direction, ou zone directionnelle, d’un groupe de contrôles*

Vous pouvez utiliser la propriété [XYFocusKeyboardNavigation](/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_XYFocusKeyboardNavigation) (dont les valeurs possibles sont [auto](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode), [Enabled](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)ou [Disabled](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)) pour gérer la navigation interne 2D à l’aide des touches de direction du clavier.

> [!NOTE]
> L’ordre de tabulation n’est pas affecté par cette propriété. Pour éviter une expérience de navigation confuse, nous vous recommandons de *ne pas* spécifier explicitement les éléments enfants d’une zone directionnelle dans l’ordre de navigation de l’onglet de votre application. Pour plus d’informations sur le comportement de tabulation d’un élément, consultez les propriétés [UIElement. TabFocusNavigation](/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) et [TabIndex](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex) .

### <a name="auto-default-behavior"></a>[Automatique](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode) (comportement par défaut)

Lorsqu’il est défini sur auto, le comportement de navigation directionnelle est déterminé par le ascendance de l’élément ou la hiérarchie d’héritage. Si tous les ancêtres sont en mode par défaut (défini sur **auto**), la navigation directionnelle avec le clavier n’est *pas* prise en charge.

### <a name="disabled"></a>[Désactivé](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)

Affectez à **XYFocusKeyboardNavigation** la valeur **Disabled** pour bloquer la navigation directionnelle vers le contrôle et ses éléments enfants.

![Comportement désactivé XYFocusKeyboardNavigation ](images/keyboard/xyfocuskeyboardnav-disabled.gif)
 *XYFocusKeyboardNavigation désactivé*

Dans cet exemple, **XYFocusKeyboardNavigation** ( [ContainerPrimary](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) ) principal a la valeur **activé**. Tous les éléments enfants héritent de ce paramètre et peuvent être parcourus à l’aide des touches de direction. Toutefois, les éléments B3 et B4 se trouvent dans un [StackPanel](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) secondaire (ContainerSecondary) avec **XYFocusKeyboardNavigation** défini sur **Disabled**, qui remplace le conteneur principal et désactive la navigation dans la touche de direction pour lui-même et entre ses éléments enfants.

```XAML
<Grid 
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" 
    TabFocusNavigation="Cycle">
    <Grid.RowDefinitions>
        <RowDefinition Height="40"/>
        <RowDefinition Height="75"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <TextBlock Name="KeyPressed"
                Grid.Row="0" 
                FontWeight="ExtraBold" 
                HorizontalTextAlignment="Center"
                TextWrapping="Wrap" 
                Padding="10" />
    <StackPanel Name="ContainerPrimary" 
                XYFocusKeyboardNavigation="Enabled" 
                KeyDown="ContainerPrimary_KeyDown" 
                Orientation="Horizontal" 
                BorderBrush="Green" 
                BorderThickness="2" 
                Grid.Row="1" 
                Padding="10" 
                MaxWidth="200">
        <Button Name="B1" 
                Content="B1" 
                GettingFocus="Btn_GettingFocus" />
        <Button Name="B2" 
                Content="B2" 
                GettingFocus="Btn_GettingFocus" />
        <StackPanel Name="ContainerSecondary" 
                    XYFocusKeyboardNavigation="Disabled" 
                    Orientation="Horizontal" 
                    BorderBrush="Red" 
                    BorderThickness="2">
            <Button Name="B3" 
                    Content="B3" 
                    GettingFocus="Btn_GettingFocus" />
            <Button Name="B4" 
                    Content="B4" 
                    GettingFocus="Btn_GettingFocus" />
        </StackPanel>
    </StackPanel>
</Grid>
```

### <a name="enabled"></a>[Activé](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)

Affectez la valeur **activé** à **XYFocusKeyboardNavigation** pour prendre en charge la navigation directionnelle 2D vers un contrôle et chacun de ses objets enfants [UIElement](/uwp/api/Windows.UI.Xaml.UIElement) .

Quand il est défini, la navigation à l’aide des touches de direction est limitée aux éléments de la zone directionnelle. La navigation par onglets n’est pas affectée, car tous les contrôles restent accessibles par le biais de leur hiérarchie d’ordre de tabulation.

![Comportement activé pour XYFocusKeyboardNavigation ](images/keyboard/xyfocuskeyboardnav-enabled.gif)
 *XYFocusKeyboardNavigation* Behavior

Dans cet exemple, **XYFocusKeyboardNavigation** ( [ContainerPrimary](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) ) principal a la valeur **activé**. Tous les éléments enfants héritent de ce paramètre et peuvent être parcourus à l’aide des touches de direction. Les éléments B3 et B4 se trouvent dans un [StackPanel](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) secondaire (ContainerSecondary) où **XYFocusKeyboardNavigation** n’est pas défini, ce qui hérite ensuite du paramètre de conteneur principal. L’élément B5 n’est pas dans une zone de direction déclarée et ne prend pas en charge la navigation par touche de direction, mais prend en charge le comportement de navigation par onglet standard.

```xaml
<Grid
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
    TabFocusNavigation="Cycle">
    <Grid.RowDefinitions>
        <RowDefinition Height="40"/>
        <RowDefinition Height="100"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <TextBlock Name="KeyPressed"
               Grid.Row="0"
               FontWeight="ExtraBold"
               HorizontalTextAlignment="Center"
               TextWrapping="Wrap"
               Padding="10" />
    <StackPanel Grid.Row="1"
                Orientation="Horizontal"
                HorizontalAlignment="Center">
        <StackPanel Name="ContainerPrimary"
                    XYFocusKeyboardNavigation="Enabled"
                    KeyDown="ContainerPrimary_KeyDown"
                    Orientation="Horizontal"
                    BorderBrush="Green"
                    BorderThickness="2"
                    Padding="5" Margin="5">
            <Button Name="B1"
                    Content="B1"
                    GettingFocus="Btn_GettingFocus" Margin="5" />
            <Button Name="B2"
                    Content="B2"
                    GettingFocus="Btn_GettingFocus" />
            <StackPanel Name="ContainerSecondary"
                        Orientation="Horizontal"
                        BorderBrush="Red"
                        BorderThickness="2"
                        Margin="5">
                <Button Name="B3"
                        Content="B3"
                        GettingFocus="Btn_GettingFocus"
                        Margin="5" />
                <Button Name="B4"
                        Content="B4"
                        GettingFocus="Btn_GettingFocus"
                        Margin="5" />
            </StackPanel>
        </StackPanel>
        <Button Name="B5"
                Content="B5"
                GettingFocus="Btn_GettingFocus"
                Margin="5" />
    </StackPanel>
</Grid>
```

Vous pouvez avoir plusieurs niveaux de zones directionnelles imbriquées. Si tous les éléments parents ont XYFocusKeyboardNavigation défini sur activé, les limites de la région de navigation interne sont ignorées.

Voici un exemple de deux zones directionnelles imbriquées dans un élément qui ne prend pas explicitement en charge la navigation directionnelle 2D. Dans ce cas, la navigation directionnelle n’est pas prise en charge entre les deux zones imbriquées.

![XYFocusKeyboardNavigation activé et le comportement imbriqué ](images/keyboard/xyfocuskeyboardnav-enabled-nested1.gif)
 *XYFocusKeyboardNavigation activé et comportement imbriqué*

Voici un exemple plus complexe de trois zones directionnelles imbriquées dans lesquelles :

-   Lorsque B1 a le focus, seul B5 peut être parcouru (et inversement), car il y a une limite de zone directionnelle où XYFocusKeyboardNavigation a la valeur Disabled, rendant B2, B3 et B4 inaccessibles à l’aide des touches de direction
-   Lorsque B2 a le focus, seul B3 peut être parcouru (et inversement), car la limite de la zone directionnelle empêche la navigation de la touche de direction sur B1, B4 et B5
-   Quand B4 a le focus, la touche Tab doit être utilisée pour naviguer entre les contrôles

![XYFocusKeyboardNavigation activé et comportement imbriqué complexe](images/keyboard/xyfocuskeyboardnav-enabled-nested2.gif)

*XYFocusKeyboardNavigation activé et comportement imbriqué complexe*

## <a name="tab-navigation"></a>Navigation par onglets

Si vous pouvez utiliser les touches de direction pour la navigation directionnelle 2D witin un contrôle ou un groupe de contrôles, la touche Tab peut être utilisée pour naviguer entre tous les contrôles d’une application Windows. 

Tous les contrôles interactifs prennent en charge la navigation par touche Tab par défaut (la propriété[IsEnabled](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_IsEnabled) et [IsTabStop](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) ont la **valeur true**), avec l’ordre de tabulation logique dérivé de la disposition des contrôles dans votre application. Toutefois, l’ordre par défaut ne correspond pas nécessairement à l’ordre visuel. La position d’affichage réelle peut dépendre du conteneur de disposition parent et de certaines propriétés que vous pouvez définir sur les éléments enfants pour influencer la disposition.

Évitez un ordre de tabulation personnalisé qui permet de se concentrer sur votre application. Par exemple, une liste de contrôles dans un formulaire doit avoir un ordre de tabulation qui s’enchaîne de haut en bas et de gauche à droite (en fonction des paramètres régionaux).

Dans cette section, nous décrivons comment l’ordre des onglets peut être entièrement personnalisé pour s’adapter à votre application.

### <a name="set-the-tab-navigation-behavior"></a>Définir le comportement de navigation par onglet

La propriété [TabFocusNavigation](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_TabFocusNavigation) de [UIElement](/uwp/api/Windows.UI.Xaml.UIElement) spécifie le comportement de navigation par onglets pour l’intégralité de son arborescence d’objets (ou zone directionnelle).

> [!NOTE]
> Utilisez cette propriété à la place de la propriété [Control. TabNavigation](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabNavigation) pour les objets qui n’utilisent pas un [ControlTemplate](/uwp/api/windows.ui.xaml.controls.controltemplate) pour définir leur apparence.

Comme nous l’avons vu dans la section précédente, pour éviter une utilisation confuse de la navigation, nous recommandons que les éléments enfants d’une zone directionnelle *ne soient pas* explicitement spécifiés dans l’ordre de navigation de l’onglet de votre application. Pour plus d’informations sur le comportement de tabulation d’un élément, consultez les propriétés [UIElement. TabFocusNavigation](/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) et [TabIndex](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex) .   
> Pour les versions antérieures à Windows 10 Creators Update (Build 10.0.15063), les paramètres de tabulation étaient limités aux objets [ControlTemplate](/uwp/api/windows.ui.xaml.controls.controltemplate) . Pour plus d’informations, consultez [Control. TabNavigation](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabNavigation).

[TabFocusNavigation](/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) a une valeur de type [KeyboardNavigationMode](/uwp/api/windows.ui.xaml.input.keyboardnavigationmode) avec les valeurs possibles suivantes (Notez que ces exemples ne sont pas des groupes de contrôles personnalisés et ne nécessitent pas de navigation interne avec les touches de direction) :

- **Local** (par défaut)   
  Les index d’onglets sont reconnus dans la sous-arborescence locale à l’intérieur du conteneur. Pour cet exemple, l’ordre de tabulation est B1, B2, B3, B4, B5, B6, B7, B1.

   ![Comportement de navigation par onglet « local »](images/keyboard/tabnav-local.gif)

   *Comportement de navigation par onglet « local »*

- **Une fois**  
  Le conteneur et tous les éléments enfants reçoivent le focus une fois. Pour cet exemple, l’ordre de tabulation est B1, B2, B7, B1 (la navigation interne avec la touche de direction est également illustrée).

   ![Comportement de navigation par onglet « une fois »](images/keyboard/tabnav-once.gif)

   *Comportement de navigation par onglet « une fois »*

- **Corbeille**   
  Le focus revient à l’élément de focus initial à l’intérieur d’un conteneur. Pour cet exemple, l’ordre de tabulation est B1, B2, B3, B4, B5, B6, B2...

   ![Comportement de navigation par onglet « cycle »](images/keyboard/tabnav-cycle.gif)

   *Comportement de navigation par onglet « cycle »*

Voici le code pour les exemples précédents (avec TabFocusNavigation = "cycle").

```XAML
<Grid 
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" 
    TabFocusNavigation="Cycle">
    <Grid.RowDefinitions>
        <RowDefinition Height="40"/>
        <RowDefinition Height="300"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <TextBlock Name="KeyPressed"
               Grid.Row="0" 
               FontWeight="ExtraBold" 
               HorizontalTextAlignment="Center"
               TextWrapping="Wrap" 
               Padding="10" />
    <StackPanel Name="ContainerPrimary"
                KeyDown="Container_KeyDown" 
                Orientation="Horizontal" 
                HorizontalAlignment="Center"
                BorderBrush="Green" 
                BorderThickness="2" 
                Grid.Row="1" 
                Padding="10" 
                MaxWidth="200">
        <Button Name="B1" 
                Content="B1" 
                GettingFocus="Btn_GettingFocus" 
                Margin="5"/>
        <StackPanel Name="ContainerSecondary" 
                    KeyDown="Container_KeyDown"
                    XYFocusKeyboardNavigation="Enabled" 
                    TabFocusNavigation ="Cycle"
                    Orientation="Vertical" 
                    VerticalAlignment="Center"
                    BorderBrush="Red" 
                    BorderThickness="2"
                    Padding="5" Margin="5">
            <Button Name="B2" 
                    Content="B2" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
            <Button Name="B3" 
                    Content="B3" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
            <Button Name="B4" 
                    Content="B4" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
            <Button Name="B5" 
                    Content="B5" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
            <Button Name="B6" 
                    Content="B6" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
        </StackPanel>
        <Button Name="B7" 
                Content="B7" 
                GettingFocus="Btn_GettingFocus" 
                Margin="5"/>
    </StackPanel>
</Grid>
```

### <a name="tabindex"></a>[TabIndex](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex)

Utilisez [TabIndex](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex) pour spécifier l’ordre dans lequel les éléments reçoivent le focus lorsque l’utilisateur parcourt les contrôles à l’aide de la touche Tab. Un contrôle avec un index de tabulation inférieur reçoit le focus avant un contrôle avec un index plus élevé.

Lorsqu’un contrôle n’a pas de [TabIndex](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) spécifié, il reçoit une valeur d’index supérieure à la valeur d’index la plus élevée actuelle (et la priorité la plus basse) de tous les contrôles interactifs dans l’arborescence d’éléments visuels, en fonction de l’étendue. 

Tous les éléments enfants d’un contrôle sont considérés comme une portée, et si l’un de ces éléments a également des éléments enfants, ils sont considérés comme une autre portée. Toute ambiguïté est résolue en choisissant le premier élément de l’arborescence d’éléments visuels de l’étendue. 

Pour exclure un contrôle de l’ordre de tabulation, affectez la valeur **false**à la propriété [IsTabStop](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) .

Remplacez l’ordre de tabulation par défaut en définissant la propriété [TabIndex](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) .

> [!NOTE] 
> [TabIndex](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) fonctionne de la même façon avec [UIElement. TabFocusNavigation](/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) et [Control. TabNavigation](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabNavigation).


Ici, nous montrons comment la navigation du Focus peut être affectée par la propriété [TabIndex](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) sur des éléments spécifiques. 

![Navigation à partir de l’onglet « local » avec le comportement TabIndex](images/keyboard/tabnav-tabindex.gif)

*Navigation à partir de l’onglet « local » avec le comportement TabIndex*

Dans l’exemple précédent, il existe deux étendues : 
- B1, zone directionnelle (B2-B6) et B7
- zone directionnelle (B2-B6)

Lorsque B3 (dans la zone directionnelle) reçoit le focus, l’étendue change et les transferts de navigation par tabulation vers la zone de direction où le meilleur candidat pour le focus suivant est identifié. Dans ce cas, a2 suivi de B4, B5 et B6. L’étendue est ensuite de nouveau modifiée et le focus se déplace vers B1.

Voici le code de cet exemple.

```xaml
<Grid
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
    TabFocusNavigation="Cycle">
    <Grid.RowDefinitions>
        <RowDefinition Height="40"/>
        <RowDefinition Height="300"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <TextBlock Name="KeyPressed"
               Grid.Row="0"
               FontWeight="ExtraBold"
               HorizontalTextAlignment="Center"
               TextWrapping="Wrap"
               Padding="10" />
    <StackPanel Name="ContainerPrimary"
                KeyDown="Container_KeyDown"
                Orientation="Horizontal"
                HorizontalAlignment="Center"
                BorderBrush="Green"
                BorderThickness="2"
                Grid.Row="1"
                Padding="10"
                MaxWidth="200">
        <Button Name="B1"
                Content="B1"
                TabIndex="1"
                ToolTipService.ToolTip="TabIndex = 1"
                GettingFocus="Btn_GettingFocus"
                Margin="5"/>
        <StackPanel Name="ContainerSecondary"
                    KeyDown="Container_KeyDown"
                    TabFocusNavigation ="Local"
                    Orientation="Vertical"
                    VerticalAlignment="Center"
                    BorderBrush="Red"
                    BorderThickness="2"
                    Padding="5" Margin="5">
            <Button Name="B2"
                    Content="B2"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
            <Button Name="B3"
                    Content="B3"
                    TabIndex="3"
                    ToolTipService.ToolTip="TabIndex = 3"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
            <Button Name="B4"
                    Content="B4"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
            <Button Name="B5"
                    Content="B5"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
            <Button Name="B6"
                    Content="B6"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
        </StackPanel>
        <Button Name="B7"
                Content="B7"
                TabIndex="2"
                ToolTipService.ToolTip="TabIndex = 2"
                GettingFocus="Btn_GettingFocus"
                Margin="5"/>
    </StackPanel>
</Grid>
```

## <a name="2d-directional-navigation-for-keyboard-gamepad-and-remote-control"></a>navigation directionnelle 2D pour le clavier, le boîtier de commande et le contrôle à distance

Les types d’entrée qui ne sont pas des pointeurs tels que le clavier, le boîtier de commande, le contrôle à distance et les outils d’accessibilité tels que Windows Narrator, partagent un mécanisme commun et sous-jacent pour naviguer et interagir avec l’interface utilisateur de votre application Windows.

Dans cette section, nous décrivons comment spécifier une stratégie de navigation préférée et affiner la navigation au sein de votre application via un ensemble de propriétés de stratégie de navigation qui prennent en charge tous les types d’entrée non pointeur basés sur le focus.

Pour plus d’informations générales sur la création d’applications et d’expériences pour la Xbox/TV, consultez [interaction du clavier](keyboard-interactions.md), [conception pour la Xbox et la télévision](../devices/designing-for-tv.md), et boîtier de [commande et les interactions de contrôle à distance](gamepad-and-remote-interactions.md).

### <a name="navigation-strategies"></a>Stratégies de navigation

> Les stratégies de navigation s’appliquent au clavier, au boîtier de commande, au contrôle à distance et à divers outils d’accessibilité.

Les propriétés de stratégie de navigation suivantes vous permettent d’influencer le contrôle qui reçoit le focus en fonction de la touche de direction, du bouton de pavé directionnel (D-PAD) ou d’un clic similaire. 

-   XYFocusUpNavigationStrategy
-   XYFocusDownNavigationStrategy
-   XYFocusLeftNavigationStrategy
-   XYFocusRightNavigationStrategy

Ces propriétés ont les valeurs possibles [auto](/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy) (valeur par défaut), [NavigationDirectionDistance](/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy), [projection](/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy)ou [RectilinearDistance ](/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy).

Si la valeur est **auto**, le comportement de l’élément est basé sur les ancêtres de l’élément. Si tous les éléments ont la valeur **auto**, la **projection** est utilisée.

> [!NOTE]
> D’autres facteurs, tels que l’élément ayant précédemment le focus ou la proximité de l’axe de la direction de navigation, peuvent influencer le résultat.

### <a name="projection"></a>Projection

La stratégie de projection déplace le focus sur le premier élément rencontré lorsque le bord de l’élément ayant actuellement le focus est *projeté* dans le sens de navigation.

Dans cet exemple, la direction de navigation du focus est définie sur projection. Notez que le focus passe de B1 à B4, en ignorant B3. Cela est dû au fait que B3 ne figure pas dans la zone de projection. Notez également qu’un candidat au focus n’est pas identifié lors du déplacement vers la gauche à partir de B1. Cela est dû au fait que la position de B2 par rapport à B1 élimine B3 comme candidat. Si B3 se trouvait dans la même ligne que B2, il s’agit d’un candidat viable pour la navigation à gauche. B2 est un candidat viable en raison de sa proximité non obstruée de l’axe de direction de navigation.

![Stratégie de navigation de projection](images/keyboard/xyfocusnavigationstrategy-projection.gif)

*Stratégie de navigation de projection*

### <a name="navigationdirectiondistance"></a>NavigationDirectionDistance

La stratégie NavigationDirectionDistance déplace le focus sur l’élément le plus proche de l’axe de la direction de navigation.

Le bord du rectangle englobant qui correspond à la direction de navigation est *étendu* et *projeté* pour identifier les cibles candidates. Le premier élément rencontré est identifié comme cible. Dans le cas de plusieurs candidats, l’élément le plus proche est identifié comme cible. S’il existe encore plusieurs candidats, l’élément le plus haut/le plus à gauche est identifié comme candidat.

![Stratégie de navigation NavigationDirectionDistance](images/keyboard/xyfocusnavigationstrategy-navigationdirectiondistance.gif)

*Stratégie de navigation NavigationDirectionDistance*

### <a name="rectilineardistance"></a>RectilinearDistance

La stratégie RectilinearDistance déplace le focus sur l’élément le plus proche en se basant sur une distance rectiligne 2D ([Geometry Taxicab](https://en.wikipedia.org/wiki/Taxicab_geometry)).

La somme de la distance principale et de la distance secondaire pour chaque candidat potentiel est utilisée pour identifier le meilleur candidtate. Dans une égalité, le premier élément à gauche est sélectionné si la direction demandée est vers le haut ou vers le bas, et le premier élément vers le haut est sélectionné si la direction demandée est gauche ou droite.

![Stratégie de navigation RectilinearDistance](images/keyboard/xyfocusnavigationstrategy-rectilineardistance.gif)

*Stratégie de navigation RectilinearDistance*

Cette image montre comment, lorsque B1 a le focus et que le bas est la direction demandée, B3 est le candidat au focus RectilinearDistance. Cela est basé sur les calcualations suivants pour cet exemple :
-   La distance (B1, B3, baisse) est 10 + 0 = 10
-   La distance (B1, B2, vers le dessous) est 0 + 40 = 30
-   La distance (B1, D, en aval) est de 30 + 0 = 30


## <a name="related-articles"></a>Articles connexes
- [Navigation en mode focus programmé](focus-navigation-programmatic.md)
- [Interactions avec le clavier](keyboard-interactions.md)
- [Accessibilité du clavier](../accessibility/keyboard-accessibility.md)
