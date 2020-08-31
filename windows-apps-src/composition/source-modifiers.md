---
title: Extraction à actualiser avec des modificateurs de source
description: Découvrez comment utiliser la fonctionnalité SourceModifier d’un InteractionTracker pour créer un contrôle d’extraction à l’actualisation personnalisé.
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP, animation
ms.localizationpriority: medium
ms.openlocfilehash: b20b4b22d1de2252864287b97bedc4a1fc176602
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89053959"
---
# <a name="pull-to-refresh-with-source-modifiers"></a>Extraction à actualiser avec des modificateurs de source

Dans cet article, nous examinons plus en détail comment utiliser la fonctionnalité SourceModifier d’un InteractionTracker et illustrons son utilisation en créant un contrôle d’extraction personnalisé.

## <a name="prerequisites"></a>Prérequis

Ici, nous partons du principe que vous êtes familiarisé avec les concepts abordés dans les articles suivants :

- [Animations pilotées par une entrée](input-driven-animations.md)
- [Expériences de manipulation personnalisées avec InteractionTracker](interaction-tracker-manipulations.md)
- [Animations basées sur les relations](relation-animations.md)

## <a name="what-is-a-sourcemodifier-and-why-are-they-useful"></a>Qu’est-ce qu’un SourceModifier et pourquoi sont-ils utiles ?

Comme [InertiaModifiers](inertia-modifiers.md), SourceModifiers vous donne un contrôle plus précis sur le mouvement d’un InteractionTracker. Mais contrairement à InertiaModifiers qui définissent le mouvement après que InteractionTracker entre en inertie, SourceModifiers définir le mouvement alors que InteractionTracker est toujours dans son état d’interaction. Dans ce cas, vous avez besoin d’une expérience différente de celle du « doigt » traditionnel.

Un exemple classique est l’expérience d’extraction vers l’actualisation : lorsque l’utilisateur extrait la liste pour actualiser le contenu et la liste panoramiques à la même vitesse que le doigt et s’arrête au bout d’une certaine distance, le mouvement peut sembler abrupt et mécanique. Une expérience plus naturelle serait d’introduire une sensation de résistance pendant que l’utilisateur interagit activement avec la liste. Cette petite nuance aide l’utilisateur final à interagir avec une liste plus dynamique et attrayante. Dans la section exemple, nous allons plus de détails sur la façon de générer ce.

Il existe 2 types de modificateurs sources :

- DeltaPosition : il s’agit du delta entre la position actuelle du frame et la position précédente du doigt du doigt pendant l’interaction avec le panoramique tactile. Ce modificateur de source vous permet de modifier la position Delta de l’interaction avant de l’envoyer en vue d’un traitement supplémentaire. Il s’agit d’un paramètre de type Vector3 et le développeur peut choisir de modifier les attributs X, Y ou Z de la position avant de la passer au InteractionTracker.
- DeltaScale : il s’agit du delta entre l’échelle de l’image actuelle et l’échelle de l’image précédente qui a été appliquée pendant l’interaction du zoom tactile. Ce modificateur de source vous permet de modifier le niveau de zoom de l’interaction. Il s’agit d’un attribut de type float que le développeur peut modifier avant de le passer à InteractionTracker.

Quand InteractionTracker est dans son état d’interaction, il évalue chaque modificateur source qui lui est assigné et détermine si l’un d’eux s’applique. Cela signifie que vous pouvez créer et assigner plusieurs modificateurs de source à un InteractionTracker. Toutefois, lors de la définition de chaque, vous devez effectuer les opérations suivantes :

1. Définir la condition : expression qui définit l’instruction conditionnelle lorsque ce modificateur de source spécifique doit être appliqué.
1. Définissez DeltaPosition/DeltaScale – expression de modificateur de source qui modifie le DeltaPosition ou le DeltaScale lorsque la condition définie ci-dessus est remplie.

## <a name="example"></a>Exemple

Voyons maintenant comment vous pouvez utiliser des modificateurs de source pour créer une expérience d’extraction en extraction personnalisée avec un contrôle ListView XAML existant. Nous utiliserons un canevas comme « panneau d’actualisation » qui sera empilé sur un ListView XAML pour créer cette expérience.

Pour l’expérience de l’utilisateur final, nous voulons créer l’effet de la « résistance », car l’utilisateur parcourt activement la liste (avec Touch) et arrête le panoramique une fois que la position va au-delà d’un certain point.

![Liste avec extraction pour l’actualisation](images/animation/city-list.gif)

Vous trouverez le code de travail de cette expérience dans la [fenêtre Windows UI dev Labs référentiel sur GitHub](https://github.com/microsoft/WindowsCompositionSamples). Voici la procédure pas à pas de la création de cette expérience.
Dans votre code de balisage XAML, vous disposez des éléments suivants :

```xaml
<StackPanel Height="500" MaxHeight="500" x:Name="ContentPanel" HorizontalAlignment="Left" VerticalAlignment="Top" >
 <Canvas Width="400" Height="100" x:Name="RefreshPanel" >
<Image x:Name="FirstGear" Source="ms-appx:///Assets/Loading.png" Width="20" Height="20" Canvas.Left="200" Canvas.Top="70"/>
 </Canvas>
 <ListView x:Name="ThumbnailList"
 MaxWidth="400"
 Height="500"
ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.IsScrollInertiaEnabled="False" ScrollViewer.IsVerticalScrollChainingEnabled="True" >
 <ListView.ItemTemplate>
 ……
 </ListView.ItemTemplate>
 </ListView>
</StackPanel>
```

Étant donné que ListView ( `ThumbnailList` ) est un contrôle XAML qui fait déjà l’objet d’un défilement, vous devez faire en sorte que le défilement soit chaîné à son parent ( `ContentPanel` ) lorsqu’il atteint l’élément le plus haut et ne peut plus défiler. (ContentPanel est l’endroit où vous allez appliquer les modificateurs sources.) Pour que cela se produise, vous devez définir ScrollViewer. IsVerticalScrollChainingEnabled sur **true** dans le balisage ListView. Vous devrez également définir le mode de chaînage sur VisualInteractionSource sur **Always**.

Vous devez définir le gestionnaire PointerPressedEvent avec le paramètre _handledEventsToo_ avec la **valeur true**. Sans cette option, le PointerPressedEvent ne sera pas chaîné au ContentPanel, car le contrôle ListView marquera ces événements comme étant gérés et ils ne seront pas envoyés en haut de la chaîne visuelle.

```csharp
//The PointerPressed handler needs to be added using AddHandler method with the //handledEventsToo boolean set to "true"
//instead of the XAML element's "PointerPressed=Window_PointerPressed",
//because the list view needs to chain PointerPressed handled events as well.
ContentPanel.AddHandler(PointerPressedEvent, new PointerEventHandler( Window_PointerPressed), true);
```

Maintenant, vous êtes prêt à lier cela avec InteractionTracker. Commencez par configurer InteractionTracker, VisualInteractionSource et l’expression qui va tirer parti de la position de InteractionTracker.

```csharp
// InteractionTracker and VisualInteractionSource setup.
_root = ElementCompositionPreview.GetElementVisual(Root);
_compositor = _root.Compositor;
_tracker = InteractionTracker.Create(_compositor);
_interactionSource = VisualInteractionSource.Create(_root);
_interactionSource.PositionYSourceMode = InteractionSourceMode.EnabledWithInertia;
_interactionSource.PositionYChainingMode = InteractionChainingMode.Always;
_tracker.InteractionSources.Add(_interactionSource);
float refreshPanelHeight = (float)RefreshPanel.ActualHeight;
_tracker.MaxPosition = new Vector3((float)Root.ActualWidth, 0, 0);
_tracker.MinPosition = new Vector3(-(float)Root.ActualWidth, -refreshPanelHeight, 0);

// Use the Tacker's Position (negated) to apply to the Offset of the Image.
// The -{refreshPanelHeight} is to hide the refresh panel
m_positionExpression = _compositor.CreateExpressionAnimation($"-tracker.Position.Y - {refreshPanelHeight} ");
m_positionExpression.SetReferenceParameter("tracker", _tracker);
_contentPanelVisual.StartAnimation("Offset.Y", m_positionExpression);
```

Avec cette configuration, le panneau d’actualisation est en dehors de la fenêtre d’affichage à sa position de départ et tout l’utilisateur voit le listView lorsque le panoramique atteint le ContentPanel, l’événement PointerPressed est déclenché, où vous demandez au système d’utiliser InteractionTracker pour piloter l’expérience de manipulation.

```csharp
private void Window_PointerPressed(object sender, PointerRoutedEventArgs e)
{
if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Touch) {
 // Tell the system to use the gestures from this pointer point (if it can).
 _interactionSource.TryRedirectForManipulation(e.GetCurrentPoint(null));
 }
}
```

> [!NOTE]
> Si le chaînage des événements gérés n’est pas nécessaire, l’ajout du gestionnaire PointerPressedEvent peut être effectué directement par le biais du balisage XAML à l’aide de l’attribut ( `PointerPressed="Window_PointerPressed"` ).

L’étape suivante consiste à configurer les modificateurs de source. Vous utiliserez deux modificateurs de source pour bénéficier de ce comportement. _Résistance_ et _arrêt_.

- Résistance : déplacez DeltaPosition. Y à la moitié de la vitesse jusqu’à ce qu’il atteigne la hauteur du RefreshPanel.

```csharp
CompositionConditionalValue resistanceModifier = CompositionConditionalValue.Create (_compositor);
ExpressionAnimation resistanceCondition = _compositor.CreateExpressionAnimation(
 $"-tracker.Position.Y < {pullToRefreshDistance}");
resistanceCondition.SetReferenceParameter("tracker", _tracker);
ExpressionAnimation resistanceAlternateValue = _compositor.CreateExpressionAnimation(
 "source.DeltaPosition.Y / 3");
resistanceAlternateValue.SetReferenceParameter("source", _interactionSource);
resistanceModifier.Condition = resistanceCondition;
resistanceModifier.Value = resistanceAlternateValue;
```

- Arrêter : arrêter le déplacement après que le RefreshPanel entier est sur l’écran.

```csharp
CompositionConditionalValue stoppingModifier = CompositionConditionalValue.Create (_compositor);
ExpressionAnimation stoppingCondition = _compositor.CreateExpressionAnimation(
 $"-tracker.Position.Y >= {pullToRefreshDistance}");
stoppingCondition.SetReferenceParameter("tracker", _tracker);
ExpressionAnimation stoppingAlternateValue = _compositor.CreateExpressionAnimation("0");
stoppingModifier.Condition = stoppingCondition;
stoppingModifier.Value = stoppingAlternateValue;
Now add the 2 source modifiers to the InteractionTracker.
List<CompositionConditionalValue> modifierList = new List<CompositionConditionalValue>()
{ resistanceModifier, stoppingModifier };
_interactionSource.ConfigureDeltaPositionYModifiers(modifierList);
```

Ce diagramme donne une visualisation de la configuration de SourceModifiers.

![Diagramme panoramique](images/animation/source-modifiers-diagram.png)

Désormais, avec le SourceModifiers, vous remarquerez que lorsque vous affichez le ListView et atteignez l’élément le plus haut, le panneau d’actualisation est retiré à la moitié du rythme du panoramique jusqu’à ce qu’il atteigne la hauteur du RefreshPanel, puis cesse de se déplacer.

Dans l’exemple complet, une animation d’image clé est utilisée pour faire tourner une icône pendant l’interaction dans le canevas RefreshPanel. Tout contenu peut être utilisé à sa place ou utiliser la position de InteractionTracker pour piloter cette animation séparément.
