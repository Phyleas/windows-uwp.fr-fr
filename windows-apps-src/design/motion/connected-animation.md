---
description: Les animations connectées vous permettent de créer une expérience de navigation dynamique et intéressante en animant la transition d’un élément entre deux vues.
title: Animation connectée
template: detail.hbs
ms.date: 10/04/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: conrwi
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 6e17b1c18fc8e643ac788e5e13ac78cae49a35ef
ms.sourcegitcommit: 6d743cf9c3e09f87ea2879b8e1f2dc4a1b1a16fe
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/19/2019
ms.locfileid: "74166078"
---
# <a name="connected-animation-for-uwp-apps"></a>Animation connectée pour les applications UWP

Les animations connectées vous permettent de créer une expérience de navigation dynamique et intéressante en animant la transition d’un élément entre deux vues. Cela permet à l’utilisateur de conserver son contexte et d'assurer la continuité entre les vues.

Dans une animation connectée, un élément semble « continuer » entre deux vues au cours d’une modification du contenu de l’interface utilisateur, volant à travers l’écran à partir de son emplacement dans la vue source jusqu’à sa destination dans la nouvelle vue. Cela met en évidence le contenu commun entre les vues et crée un effet magnifique et dynamique dans le cadre d’une transition.

> **API importantes**: [classe ConnectedAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimation), [classe ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)


## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Si vous avez installé l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong> , cliquez ici pour <a href="xamlcontrolsgallery:/item/ConnectedAnimation">ouvrir l’application et voir animation connectée en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Dans cette brève vidéo, une application utilise une animation connectée pour animer une image d’élément quand elle « continue » pour faire partie de l’en-tête de la page suivante. L’effet aide à conserver le contexte de l’utilisateur pendant toute la transition.

![Animation connectée](images/connected-animations/example.gif)

<!-- 
<iframe width=640 height=360 src='https://microsoft.sharepoint.com/portals/hub/_layouts/15/VideoEmbedHost.aspx?chId=552c725c%2De353%2D4118%2Dbd2b%2Dc2d0584c9848&amp;vId=b2daa5ee%2Dbe15%2D4503%2Db541%2D1328a6587c36&amp;width=640&amp;height=360&amp;autoPlay=false&amp;showInfo=true' allowfullscreen></iframe>
-->

## <a name="video-summary"></a>Résumé de la vidéo

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev005/player]

## <a name="connected-animation-and-the-fluent-design-system"></a>Animation connectée et Fluent Design System

 Le système Fluent Design vous aide à créer une interface utilisateur moderne et claire, qui incorpore de la lumière, de la profondeur, du mouvement, des matériaux et une mise à l’échelle. Animation connectée est un composant de Fluent Design System qui ajoute du mouvement à votre application. Pour plus d’informations, consultez [Vue d’ensemble de Fluent Design pour UWP](/windows/apps/fluent-design-system).

## <a name="why-connected-animation"></a>Pourquoi utiliser l'animation connectée ?

Lorsqu'il navigue entre des pages, il est important que l’utilisateur comprenne le nouveau contenu qui lui est présenté après la navigation et en quoi celui-ci est pertinent avec ses choix de navigation. Les animations connectées offrent une métaphore visuelle puissante qui met l’accent sur la relation entre les deux affichages en attirant l'attention de l’utilisateur sur le contenu commun entre eux. En outre, les animations connectées renforcent l'intérêt visuel et donne un aspect soigné à la navigation entre les pages qui permet de différencier le modèle de mouvement de votre application.

## <a name="when-to-use-connected-animation"></a>Quand utiliser une animation connectée

Les animations connectées sont généralement utilisées lorsque l'utilisateur change de page, bien qu'elles puissent être appliquées à n'importe quelle expérience qui implique une modification du contenu d'une interface utilisateur et qui exige que l'utilisateur préserve le contexte. Envisagez d’utiliser une animation connectée au lieu d’une [transition de navigation d'exploration](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition) chaque fois qu’il existe une image ou un autre élément d’interface utilisateur commun entre des affichages source et de destination.

## <a name="configure-connected-animation"></a>Configurer l’animation connectée

> [!IMPORTANT]
> Cette fonctionnalité requiert que la version cible de votre application soit Windows 10, version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou version ultérieure. La propriété de configuration n’est pas disponible dans les kits de développement logiciel précédents. Vous pouvez cibler une version minimale inférieure à celle du kit de développement logiciel (SDK) 17763 à l’aide de code adaptatif ou XAML conditionnel. Pour plus d’informations, consultez [version adaptative Apps](/windows/uwp/debug-test-perf/version-adaptive-apps).

À compter de Windows 10, version 1809, les animations connectées incorporent davantage la conception Fluent en fournissant des configurations d’animation adaptées spécifiquement pour la navigation vers l’avant et vers l’arrière.

Vous spécifiez une configuration d’animation en définissant la propriété de configuration sur ConnectedAnimation. (Nous vous présenterons des exemples dans la section suivante.)

Ce tableau décrit les configurations disponibles. Pour plus d’informations sur les principes de mouvement appliqués dans ces animations, consultez [direction et gravité](index.md).

| [GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration) |
| - |
| Il s’agit de la configuration par défaut, qui est recommandée pour la navigation vers l’avant. |
Au fur et à mesure que l’utilisateur navigue vers l’avant dans l’application (A à B), l’élément connecté semble être physiquement « en dehors de la page ». Dans ce cas, l’élément semble avancer dans l’espace z et dépose un bit comme effet de la suspension de la gravité. Pour surmonter les effets de la gravité, l’élément gagne la vélocité et accélère sa position finale. Le résultat est une animation « Scale and DIP ». |

| [DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration) |
| - |
| Lorsque l’utilisateur navigue vers l’arrière dans l’application (B à A), l’animation est plus directe. L’élément connecté effectue une translation linéaire de B à A à l’aide d’une fonction d’accélération de Bézier cubique ralentie. La valeur visuelle descendante permet de renvoyer l’utilisateur à son état précédent aussi rapidement que possible tout en conservant le contexte du workflow de navigation. |

| [BasicConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.basicconnectedanimationconfiguration) |
| - |
| Il s’agit de l’animation par défaut (et uniquement) utilisée dans les versions antérieures à Windows 10, version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)). |

### <a name="connectedanimationservice-configuration"></a>Configuration de ConnectedAnimationService

La classe [ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice) a deux propriétés qui s’appliquent aux animations individuelles plutôt qu’au service global.

- [DefaultDuration](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaultduration)
- [DefaultEasingFunction](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaulteasingfunction)

Pour obtenir les différents effets, certaines configurations ignorent ces propriétés sur ConnectedAnimationService et utilisent leurs propres valeurs à la place, comme décrit dans ce tableau.

| Configuration | Qu’est-ce que DefaultDuration ? | Qu’est-ce que DefaultEasingFunction ? |
| - | - | - |
| Gravité | Oui | Oui* <br/> **la traduction de base de a à B utilise cette fonction d’accélération, mais le « DIP de gravité » a sa propre fonction d’accélération.*  |
| Directe | non <br/> *Anime plus de 150 mètres.*| non <br/> *Utilise la fonction d’accélération de décélération.* |
| De base | Oui | Oui |

## <a name="how-to-implement-connected-animation"></a>Comment implémenter une animation connectée

La configuration d’une animation connectée comprend deux étapes :

1. *Préparez* un objet d’animation sur la page source, ce qui indique au système que l’élément source participera à l’animation connectée.
1. *Démarrez* l’animation sur la page de destination, en passant une référence à l’élément de destination.

Lors de la navigation à partir de la page source, appelez [ConnectedAnimationService. GetForCurrentView](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getforcurrentview) pour accéder à une instance de ConnectedAnimationService. Pour préparer une animation, appelez [PrepareToAnimate](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.preparetoanimate) sur cette instance et passez une clé unique et l’élément d’interface utilisateur que vous souhaitez utiliser dans la transition. La clé unique vous permet de récupérer l’animation ultérieurement sur la page de destination.

```csharp
ConnectedAnimationService.GetForCurrentView()
    .PrepareToAnimate("forwardAnimation", SourceImage);
```

Lorsque la navigation se produit, démarrez l’animation dans la page de destination. Pour démarrer l’animation, appelez [ConnectedAnimation.TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart). Vous pouvez récupérer l’instance d’animation appropriée en appelant [ConnectedAnimationService.GetAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getanimation) avec la clé unique que vous avez fournie lors de la création de l’animation.

```csharp
ConnectedAnimation animation =
    ConnectedAnimationService.GetForCurrentView().GetAnimation("forwardAnimation");
if (animation != null)
{
    animation.TryStart(DestinationImage);
}
```

### <a name="forward-navigation"></a>Navigation vers l’avant

Cet exemple montre comment utiliser ConnectedAnimationService pour créer une transition pour la navigation vers l’avant entre deux pages (Page_A à Page_B).

La configuration d’animation recommandée pour la navigation vers l’avant est [GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration). Il s’agit de la valeur par défaut. vous n’avez donc pas besoin de définir la propriété de [configuration](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.configuration) , sauf si vous souhaitez spécifier une configuration différente.

Configurez l’animation dans la page source.

```xaml
<!-- Page_A.xaml -->

<Image x:Name="SourceImage"
       HorizontalAlignment="Left" VerticalAlignment="Top"
       Width="200" Height="200"
       Stretch="Fill"
       Source="Assets/StoreLogo.png"
       PointerPressed="SourceImage_PointerPressed"/>
```

```csharp
// Page_A.xaml.cs

private void SourceImage_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    // Navigate to detail page.
    // Suppress the default animation to avoid conflict with the connected animation.
    Frame.Navigate(typeof(Page_B), null, new SuppressNavigationTransitionInfo());
}

protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
{
    ConnectedAnimationService.GetForCurrentView()
        .PrepareToAnimate("forwardAnimation", SourceImage);
    // You don't need to explicitly set the Configuration property because
    // the recommended Gravity configuration is default.
    // For custom animation, use:
    // animation.Configuration = new BasicConnectedAnimationConfiguration();
}
```

Démarrez l’animation dans la page de destination.

```xaml
<!-- Page_B.xaml -->

<Image x:Name="DestinationImage"
       Width="400" Height="400"
       Stretch="Fill"
       Source="Assets/StoreLogo.png" />
```

```csharp
// Page_B.xaml.cs

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    ConnectedAnimation animation =
        ConnectedAnimationService.GetForCurrentView().GetAnimation("forwardAnimation");
    if (animation != null)
    {
        animation.TryStart(DestinationImage);
    }
}
```

### <a name="back-navigation"></a>Navigation arrière

Pour la navigation arrière (Page_B à Page_A), vous suivez les mêmes étapes, mais les pages source et destination sont inversées.

Lorsque l’utilisateur navigue en arrière, il s’attend à ce que l’application soit retournée à l’état précédent dès que possible. Par conséquent, la configuration recommandée est [DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration). Cette animation est plus rapide, plus directe et utilise l’accélération de décélération.

Configurez l’animation dans la page source.

```csharp
// Page_B.xaml.cs

protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
{
    if (e.NavigationMode == NavigationMode.Back)
    {
        ConnectedAnimation animation = 
            ConnectedAnimationService.GetForCurrentView().PrepareToAnimate("backAnimation", DestinationImage);

        // Use the recommended configuration for back animation.
        animation.Configuration = new DirectConnectedAnimationConfiguration();
    }
}
```

Démarrez l’animation dans la page de destination.

```csharp
// Page_A.xaml.cs

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    ConnectedAnimation animation =
        ConnectedAnimationService.GetForCurrentView().GetAnimation("backAnimation");
    if (animation != null)
    {
        animation.TryStart(SourceImage);
    }
}
```

Entre le moment où l’animation est configurée et le moment où elle est démarrée, l’élément source apparaît figé au-dessus d’une autre interface utilisateur dans l’application. Cela vous permet d’effectuer toutes les autres animations de transition simultanément. Pour cette raison, vous ne devez pas attendre plus d’environ 250 millisecondes entre les deux étapes, car la présence de l’élément source peut devenir gênante. Si vous préparez une animation et que vous ne la démarrez pas dans les trois secondes, le système la supprimera et tous les appels subséquents vers [TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart) échoueront.

## <a name="connected-animation-in-list-and-grid-experiences"></a>Expériences d'animation connectée dans des listes et des grilles

Souvent, vous devrez créer une animation connectée depuis ou vers un contrôle de liste ou de grille. Vous pouvez utiliser les deux méthodes sur [ListView](/uwp/api/windows.ui.xaml.controls.listview) et [GridView](/uwp/api/windows.ui.xaml.controls.gridview), [PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation) et [TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync), pour simplifier ce processus.

Par exemple, supposons que vous avez une **ListView** qui contient un élément nommé « PortraitEllipse » dans son modèle de données.

```xaml
<ListView x:Name="ContactsListView" Loaded="ContactsListView_Loaded">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="vm:ContactsItem">
            <Grid>
                …
                <Ellipse x:Name="PortraitEllipse" … />
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Pour préparer une animation connectée avec l’ellipse correspondant à un élément de liste donné, appelez la méthode [PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation) avec une clé unique, l’élément et le nom « PortraitEllipse ».

```csharp
void PrepareAnimationWithItem(ContactsItem item)
{
     ContactsListView.PrepareConnectedAnimation("portrait", item, "PortraitEllipse");
}
```

Pour démarrer une animation avec cet élément comme destination, par exemple lors de la navigation vers l’arrière à partir d’une vue détaillée, utilisez [TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync). Si vous venez de charger la source de données pour ListView, TryStartConnectedAnimationAsync attendra de démarrer l’animation jusqu’à ce que le conteneur d’éléments correspondant ait été créé.

```csharp
private void ContactsListView_Loaded(object sender, RoutedEventArgs e)
{
    ContactsItem item = GetPersistedItem(); // Get persisted item
    if (item != null)
    {
        ContactsListView.ScrollIntoView(item);
        ConnectedAnimation animation =
            ConnectedAnimationService.GetForCurrentView().GetAnimation("portrait");
        if (animation != null)
        {
            await ContactsListView.TryStartConnectedAnimationAsync(
                animation, item, "PortraitEllipse");
        }
    }
}
```

## <a name="coordinated-animation"></a>Animation coordonnée

![Animation coordonnée](images/connected-animations/coordinated_example.gif)

<!--
<iframe width=640 height=360 src='https://microsoft.sharepoint.com/portals/hub/_layouts/15/VideoEmbedHost.aspx?chId=552c725c%2De353%2D4118%2Dbd2b%2Dc2d0584c9848&amp;vId=9066bbbe%2Dcf58%2D4ab4%2Db274%2D595616f5d0a0&amp;width=640&amp;height=360&amp;autoPlay=false&amp;showInfo=true' allowfullscreen></iframe>
-->

Une *animation coordonnée* est un type spécial d’animation d’entrée où un élément apparaît avec la cible de l’animation connectée, en animant en tandem avec l’élément d’animation connecté lorsqu’il se déplace sur l’écran. Les animations coordonnées peuvent renforcer l'effet visuel d'une transition et attirer davantage l’attention de l’utilisateur sur le contexte commun entre les affichages source et destination. Dans ces images, l’interface utilisateur de la légende pour l’élément s'anime à l'aide d’une animation coordonnée.

Lorsqu’une animation coordonnée utilise la configuration de gravité, la gravité s’applique à la fois à l’élément d’animation connecté et aux éléments coordonnés. Les éléments coordonnés sont « coup » en plus de l’élément connecté afin que les éléments restent véritablement coordonnés.

Utilisez la surcharge de deux paramètres de **TryStart** pour ajouter des éléments coordonnés à une animation connectée. Cet exemple montre une animation coordonnée d’une disposition de grille nommée « DescriptionRoot » qui entre en tandem avec un élément d’animation connecté nommé « CoverImage ».

```xaml
<!-- DestinationPage.xaml -->
<Grid>
    <Image x:Name="CoverImage" />
    <Grid x:Name="DescriptionRoot" />
</Grid>
```

```csharp
// DestinationPage.xaml.cs
void OnNavigatedTo(NavigationEventArgs e)
{
    var animationService = ConnectedAnimationService.GetForCurrentView();
    var animation = animationService.GetAnimation("coverImage");

    if (animation != null)
    {
        // Don’t need to capture the return value as we are not scheduling any subsequent
        // animations
        animation.TryStart(CoverImage, new UIElement[] { DescriptionRoot });
     }
}
```

## <a name="dos-and-donts"></a>À faire et à ne pas faire

- Utilisez une animation connectée dans les transitions de page où un élément est commun aux pages source et destination.
- Utilisez [GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration) pour la navigation vers l’avant.
- Utilisez [DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration) pour la navigation arrière.
- N’attendez pas les requêtes réseau ou d’autres opérations asynchrones de longue durée entre la préparation et le démarrage d’une animation connectée. Vous devrez peut-être charger au préalable les informations nécessaires pour exécuter la transition à l'avance, ou utiliser une image d'espace réservé basse résolution pendant le chargement d’une image haute résolution dans l’affichage de destination.
- Utilisez [SuppressNavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) pour empêcher une animation de transition dans un **Frame** si vous utilisez **ConnectedAnimationService**, car les animations connectées ne sont pas censées être utilisées simultanément avec les transitions de navigation par défaut. Consultez [NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition) pour en savoir plus sur l’utilisation des transitions de navigation.

## <a name="related-articles"></a>Articles associés

[ConnectedAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimation)

[ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)

[NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition)
