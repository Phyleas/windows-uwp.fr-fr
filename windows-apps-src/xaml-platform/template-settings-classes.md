---
description: Découvrez comment utiliser les classes TemplateSettings pour fournir un ensemble de propriétés qui définissent un nouveau modèle de contrôle.
title: Classes de paramètres du modèle
ms.assetid: CAE933C6-EF13-465A-9831-AB003AF23907
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 23418233769d893e755ee8981aa9f66aa9404758
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154963"
---
# <a name="template-settings-classes"></a>Classes de paramètres du modèle


## <a name="prerequisites"></a>Prérequis

Il est supposé que vous savez ajouter des contrôles à une interface utilisateur, définir leurs propriétés et joindre des gestionnaires d’événements. Pour obtenir des instructions sur l’ajout de contrôles à une application, voir [Ajouter des contrôles et gérer les événements](../design/controls-and-patterns/controls-and-events-intro.md). Nous partons également du principe que vous connaissez les bases de la définition d’un modèle personnalisé d’un contrôle en modifiant une copie du modèle par défaut. Pour plus d’informations, voir [Démarrage rapide : modèles de contrôles](/previous-versions/windows/apps/hh465374(v=win.10)).

## <a name="the-scenario-for-templatesettings-classes"></a>Scénario relatif aux classes **TemplateSettings**

Les classes **TemplateSettings** fournissent un ensemble de propriétés qui sont utilisées lorsque vous définissez un nouveau modèle pour un contrôle. Les propriétés possèdent des valeurs, par exemple des mesures en pixels, correspondant à la taille de certains éléments d’interface utilisateur. Ces valeurs sont parfois des valeurs calculées qui proviennent de la logique de contrôle qui n’est généralement pas facile à remplacer ni même accessible. Certaines propriétés se veulent des valeurs **From** et **To** qui contrôlent les transitions et les animations des parties. Les propriétés **TemplateSettings** appropriées sont donc fournies par paires.

Voici plusieurs classes **TemplateSettings**. Elles figurent toutes dans l’espace de noms [**Windows.UI.Xaml.Controls.Primitives**](/uwp/api/Windows.UI.Xaml.Controls.Primitives). Voici une liste des classes et un lien vers la propriété **TemplateSettings** du contrôle approprié. Cette propriété **TemplateSettings** indique comment accéder aux valeurs **TemplateSettings** du contrôle et établir des liaisons de modèle à ses propriétés :

-   [**ComboBoxTemplateSettings**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ComboBoxTemplateSettings): valeur de [ **ComboBox. TemplateSettings**](/uwp/api/windows.ui.xaml.controls.combobox.templatesettings)
-   [**GridViewItemTemplateSettings**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.GridViewItemTemplateSettings) : valeur [**GridViewItem.TemplateSettings**](/uwp/api/windows.ui.xaml.controls.gridviewitem.templatesettings)
-   [**ListViewItemTemplateSettings**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ListViewItemTemplateSettings): valeur de [ **ListViewItem. TemplateSettings**](/uwp/api/windows.ui.xaml.controls.listviewitem.templatesettings)
-   [**ProgressBarTemplateSettings**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ProgressBarTemplateSettings): valeur de [ **ProgressBar. TemplateSettings**](/uwp/api/windows.ui.xaml.controls.progressbar.templatesettings)
-   [**ProgressRingTemplateSettings**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ProgressRingTemplateSettings): valeur de [ **ProgressRing. TemplateSettings**](/uwp/api/windows.ui.xaml.controls.progressring.templatesettings)
-   [**SettingsFlyoutTemplateSettings**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.SettingsFlyoutTemplateSettings): valeur de [ **SettingsFlyout. TemplateSettings**](/uwp/api/windows.ui.xaml.controls.settingsflyout.templatesettings)
-   [**ToggleSwitchTemplateSettings**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleSwitchTemplateSettings) : valeur [**ToggleSwitch.TemplateSettings**](/uwp/api/windows.ui.xaml.controls.toggleswitch.templatesettings)
-   [**ToolTipTemplateSettings**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToolTipTemplateSettings): valeur de [ **ToolTip. TemplateSettings**](/uwp/api/windows.ui.xaml.controls.tooltip.templatesettings)

Les propriétés **TemplateSettings** sont toujours destinées à être utilisées en XAML. Ce sont des sous-propriétés en lecture seule d’une propriété **TemplateSettings** en lecture seule d’un contrôle parent. Pour un scénario de contrôle personnalisé avancé, lorsque vous créez une classe basée sur [**Control**](/uwp/api/Windows.UI.Xaml.Controls.Control) et pouvez donc influer sur la logique de contrôle, pensez à définir une propriété **TemplateSettings** personnalisée sur le contrôle pour communiquer des informations pouvant être utiles à toute personne qui recrée le modèle du contrôle. Pour la valeur de cette propriété en lecture seule, définissez une nouvelle classe **TemplateSettings** liée à votre contrôle qui possède des propriétés en lecture seule pour chacune des informations qui sont pertinentes pour les mesures de modèle, le positionnement d’animation, etc. et qui donnent aux appelants l’instance d’exécution de cette classe, qui est initialisée à l’aide de votre logique de contrôle. Les classes **TemplateSettings** sont dérivées de [**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject) afin que les propriétés puissent utiliser le système de propriétés de dépendance pour les rappels de modification de propriété. Mais les identificateurs de propriété de dépendance pour les propriétés ne sont pas exposés comme API publique, car les propriétés **TemplateSettings** sont censées être en lecture seule pour les appelants.

## <a name="how-to-use-templatesettings-in-a-control-template"></a>Comment utiliser **TemplateSettings** dans un modèle de contrôle

Voici un exemple qui provient des modèles de contrôles XAML par défaut de départ. Celui-ci particulier provient du modèle par défaut de [**ProgressRing**](/uwp/api/Windows.UI.Xaml.Controls.ProgressRing):

```xml
<Ellipse
    x:Name="E1"
    Style="{StaticResource ProgressRingEllipseStyle}"
    Width="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Height="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Margin="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseOffset}"
    Fill="{TemplateBinding Foreground}"/>
```

Comme le code XAML complet du modèle [**ProgressRing**](/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) contient des centaines de lignes, il s’agit là d’un extrait minuscule. Ce code XAML définit une partie de contrôle qui est l’un des 6 éléments [**ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) qui illustrent l’animation en rotation pour une progression indéterminée. En tant que développeur, vous n’aimez peut-être pas les cercles et vous souhaitez peut-être utiliser une autre primitive graphique ou une autre forme de base pour la progression de l’animation. Par exemple, vous pouvez composer à la place un **ProgressRing** utilisant un ensemble d’éléments [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) organisés dans un carré. Si tel est le cas, chaque composant **Rectangle** individuel de votre nouveau modèle peut ressembler à ceci :

```xml
<Rectangle
    x:Name="R1"
    Width="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Height="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Margin="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseOffset}"
    Fill="{TemplateBinding Foreground}"/>
```

Les propriétés **TemplateSettings** sont utiles ici, car ce sont des valeurs calculées à partir de la logique de contrôle de base de [**ProgressRing**](/uwp/api/Windows.UI.Xaml.Controls.ProgressRing). Le calcul se divise en [**ActualWidth**](/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) et [**ActualHeight**](/uwp/api/windows.ui.xaml.frameworkelement.actualheight) de **ProgressRing**, et alloue une mesure calculée pour chacun des éléments de mouvement dans ses modèles pour que les composants de modèle puissent s’adapter au contenu.

Voici un autre exemple d’utilisation des modèles de contrôles XAML par défaut, qui montre cette fois l’un des ensembles de propriétés qui sont le **From** et la **To** d’une animation. Il provient du modèle [**ComboBox**](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) par défaut :

```xml
<VisualStateGroup x:Name="DropDownStates">
    <VisualState x:Name="Opened">
        <Storyboard>
            <SplitOpenThemeAnimation
               OpenedTargetName="PopupBorder"
               ContentTargetName="ScrollViewer"
               ClosedTargetName="ContentPresenter"
               ContentTranslationOffset="0"
               OffsetFromCenter="{Binding RelativeSource={RelativeSource TemplatedParent}, 
                 Path=TemplateSettings.DropDownOffset}"
               OpenedLength="{Binding RelativeSource={RelativeSource TemplatedParent}, 
                 Path=TemplateSettings.DropDownOpenedHeight}"
               ClosedLength="{Binding RelativeSource={RelativeSource TemplatedParent},
                 Path=TemplateSettings.DropDownClosedHeight}" />
        </Storyboard>
   </VisualState>
...
</VisualStateGroup>
```

À nouveau, comme le code XAML contient de nombreuses lignes dans le modèle, il ne s’agit là que d’un extrait. Et il ne s’agit que d’un des nombreux États et animations de thème qui utilisent chacun les mêmes propriétés [**ComboBoxTemplateSettings**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ComboBoxTemplateSettings) . Pour [**ComboBox**](/uwp/api/Windows.UI.Xaml.Controls.ComboBox), l’utilisation des valeurs **ComboBoxTemplateSettings** par le biais des liaisons impose que les animations associées dans le modèle s’arrêteront et commenceront aux positions qui sont basées sur les valeurs partagées. La transition sera donc parfaite.

**Remarque**    Quand vous utilisez des valeurs **TemplateSettings** dans le cadre de votre modèle de contrôle, veillez à définir des propriétés qui correspondent au type de la valeur. Sinon, vous devrez peut-être créer un convertisseur de valeur pour la liaison afin que le type cible de la liaison puisse être converti à partir d’un type de source différent de la valeur **TemplateSettings**. Pour plus d’informations, voir [**IValueConverter**](/uwp/api/Windows.UI.Xaml.Data.IValueConverter).

## <a name="related-topics"></a>Rubriques connexes

* [Démarrage rapide : modèles de contrôle](/previous-versions/windows/apps/hh465374(v=win.10))