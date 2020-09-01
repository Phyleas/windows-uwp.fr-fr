---
Description: Recevoir, traiter et gérer des données d’entrée à partir d’appareils de pointage tels que Touch, Mouse, Pen/Stylus et Touchpad, dans vos applications Windows.
title: Gérer les entrées du pointeur
ms.assetid: BDBC9E33-4037-4671-9596-471DCF855C82
label: Handle pointer input
template: detail.hbs
keywords: stylet, souris, pavé tactile, entrées tactiles, pointeur, entrées, interactions avec l’utilisateur
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f544b73e069827f3c680db45797081605ce41b63
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173453"
---
# <a name="handle-pointer-input"></a>Gérer les entrées du pointeur

Recevoir, traiter et gérer des données d’entrée à partir d’appareils de pointage (tels que Touch, Mouse, Pen/Stylus et touchpad) dans vos applications Windows.

> [!Important]
> Créez des interactions personnalisées uniquement s’il existe une exigence claire et bien définie et que les interactions prises en charge par les contrôles de plateforme ne prennent pas en charge votre scénario.  
> Si vous personnalisez les expériences d’interaction dans votre application Windows, les utilisateurs s’attendent à ce qu’ils soient cohérents, intuitifs et détectables. Pour ces raisons, nous vous recommandons de modéliser vos interactions personnalisées sur celles prises en charge par les [contrôles de plateforme](../controls-and-patterns/controls-by-function.md). Les contrôles de plateforme fournissent l’expérience d’interaction utilisateur de l’application Windows complète, y compris les interactions standard, les effets physiques animés, les commentaires visuels et l’accessibilité. 

## <a name="important-apis"></a>API importantes
- [Windows.Devices.Input](/uwp/api/Windows.Devices.Input)
- [Windows.UI.Input](/uwp/api/Windows.UI.Core)
- [Windows.UI.Xaml.Input](/uwp/api/Windows.UI.Input)

## <a name="pointers"></a>Pointeurs
La plupart des expériences d’interaction impliquent généralement que l’utilisateur identifie l’objet avec lequel il souhaite interagir en pointant sur lui par le biais de périphériques d’entrée tels que Touch, Mouse, Pen/Stylus et Touchpad. Comme les données d’appareil HID fournies par ces périphériques d’entrée incluent de nombreuses propriétés communes, les données sont promues et consolidées dans une pile d’entrée unifiée et exposées comme données de pointeur indépendantes du périphérique. Vos applications Windows peuvent ensuite utiliser ces données sans se soucier de l’utilisation de l’appareil d’entrée.

> [!NOTE]
> Les informations spécifiques à l’appareil sont également promues à partir des données HID brutes si votre application en a besoin.

Chaque point d’entrée (ou contact) sur la pile d’entrée est représenté par un objet [**pointeur**](/uwp/api/Windows.UI.Xaml.Input.Pointer) exposé via le paramètre [**PointerRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.PointerRoutedEventArgs) dans les différents gestionnaires d’événements de pointeur. Dans le cas d’une entrée multi-PEN ou multipoint, chaque contact est traité comme un pointeur d’entrée unique.

## <a name="pointer-events"></a>Événements de pointeur

Les événements de pointeur exposent des informations de base telles que le type d’appareil d’entrée et l’état de détection (dans la plage ou le contact), ainsi que des informations étendues telles que l’emplacement, la pression et la géométrie de contact. D’autres propriétés de périphérique spécifiques sont également disponibles. Elles indiquent sur quel bouton de souris l’utilisateur a appuyé ou si la gomme du stylet a été utilisée. Si une différenciation entre les périphériques d’entrée et leurs fonctionnalités est nécessaire dans le cadre de votre application, voir [Identifier des périphériques d’entrée](identify-input-devices.md).

Les applications Windows peuvent écouter les événements de pointeur suivants :

> [!NOTE]
> Contraindre l’entrée de pointeur à un élément d’interface utilisateur spécifique en appelant  [**CapturePointer**](/uwp/api/windows.ui.xaml.uielement.capturepointer) sur cet élément dans un gestionnaire d’événements de pointeur. Lorsqu’un pointeur est capturé par un élément, seul cet objet reçoit des événements d’entrée de pointeur, même lorsque le pointeur se déplace à l’extérieur de la zone englobante de l’objet. Le [**IsInContact**](/uwp/api/windows.ui.xaml.input.pointer.isincontact) (bouton de la souris enfoncé, tactile ou Stylus dans le contact) doit avoir la valeur true pour que **CapturePointer** aboutisse.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Événement</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercanceled"><strong>PointerCanceled</strong></a></p></td>
<td align="left"><p>Se produit lorsqu’un pointeur est annulé par la plateforme. Cela peut se produire dans les circonstances suivantes :</p>
<ul>
<li>Les pointeurs tactiles sont annulés quand un stylet est détecté dans la plage de la surface d’entrée.</li>
<li>Aucun contact actif n’est détecté pendant plus de 100 ms.</li>
<li>Le moniteur/l’écran est modifié (résolution, paramètres, configuration à plusieurs écrans).</li>
<li>Le bureau est verrouillé ou l’utilisateur s’est déconnecté.</li>
<li>Le nombre de contacts simultanés a dépassé le nombre pris en charge par le périphérique.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercapturelost"><strong>PointerCaptureLost</strong></a></p></td>
<td align="left"><p>Se produit lorsqu’un autre élément d’interface utilisateur capture le pointeur, lorsque le pointeur est libéré ou lorsqu’un autre pointeur est capturé par programme.</p>
<div class="alert">
<strong>Remarque</strong>    Il n’y a pas d’événement de capture de pointeur correspondant.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered"><strong>PointerEntered</strong></a></p></td>
<td align="left"><p>Se produit lorsqu’un pointeur entre dans la zone de délimitation d’un élément. Cela peut se produire de manière légèrement différente pour les entrées par interaction tactile, pavé tactile, souris et stylet.</p>
<ul>
<li>Pour déclencher cet événement, l’interaction tactile requiert un contact du doigt directement sur l’élément ou par déplacement dans la zone de délimitation de l’élément.</li>
<li>La souris et le pavé tactile ont un curseur à l’écran qui est toujours visible et qui déclenche cet événement même si aucun bouton de la souris ni aucun bouton du pavé tactile n'est enfoncé.</li>
<li>Comme l’interaction tactile, le stylet déclenche cet événement par contact direct du stylet sur l’élément ou par déplacement dans la zone de délimitation de l’élément. Toutefois, Pen a également un état de survol (<a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointer.isinrange">IsInRange</a>) qui, lorsqu’il a la valeur true, déclenche cet événement.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited"><strong>PointerExited</strong></a></p></td>
<td align="left"><p>Se produit lorsqu’un pointeur quitte la zone de délimitation d’un élément. Cela peut se produire de manière légèrement différente pour les entrées par interaction tactile, pavé tactile, souris et stylet.</p>
<ul>
<li>L’interaction tactile nécessite un contact du doigt et déclenche cet événement lorsque le pointeur se déplace hors de la zone de délimitation de l’élément.</li>
<li>La souris et le pavé tactile ont un curseur à l’écran qui est toujours visible et qui déclenche cet événement même si aucun bouton de la souris ni aucun bouton du pavé tactile n'est enfoncé.</li>
<li>Comme l’interaction tactile, le stylet déclenche cet événement lorsqu’il sort de la zone de délimitation de l’élément. Toutefois, Pen a également un état de survol (<a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointer.isinrange">IsInRange</a>) qui déclenche cet événement lorsque l’état passe de true à false.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointermoved"><strong>PointerMoved</strong></a></p></td>
<td align="left"><p>Se produit lorsqu’un pointeur change les coordonnées, l’état d’un bouton, la pression, l’inclinaison ou la géométrie de contact (par exemple, largeur et hauteur) dans la zone de délimitation d’un élément. Cela peut se produire de manière légèrement différente pour les entrées par interaction tactile, pavé tactile, souris et stylet.</p>
<ul>
<li>L’interaction tactile nécessite un contact du doigt et déclenche cet événement uniquement en cas de contact au sein de la zone de délimitation de l’élément.</li>
<li>La souris et le pavé tactile ont un curseur à l’écran qui est toujours visible et qui déclenche cet événement même si aucun bouton de la souris ni aucun bouton du pavé tactile n'est enfoncé.</li>
<li>Comme l’interaction tactile, le stylet déclenche cet événement en cas de contact au sein de la zone de délimitation de l’élément. Toutefois, Pen a également un état de survol (<a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointer.isinrange">IsInRange</a>) qui, lorsque la valeur est true et dans la zone englobante de l’élément, déclenche cet événement.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed"><strong>PointerPressed</strong></a></p></td>
<td align="left"><p>Se produit lorsque le pointeur indique une action d’appui (par exemple, une pression par interaction tactile, sur un bouton de souris, sur un stylet ou sur un bouton du pavé tactile) dans la zone de délimitation d’un élément.</p>
<p><a href="/uwp/api/windows.ui.xaml.uielement.capturepointer">CapturePointer</a> doit être appelé à partir du gestionnaire pour cet événement.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased"><strong>PointerReleased</strong></a></p></td>
<td align="left"><p>Se produit lorsque le pointeur indique une action de libération (par exemple, arrêt de l’interaction tactile, relâchement du bouton de la souris, du stylet ou du bouton du pavé tactile) dans la zone de délimitation d’un élément ou, lorsque le pointeur est capturé, en dehors de la zone de délimitation.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged"><strong>PointerWheelChanged</strong></a></p></td>
<td align="left"><p>Se produit lorsque la roulette de la souris est actionnée.</p>
<p>L’entrée de la souris est associée à un seul pointeur affecté lors de la première détection de l’entrée de la souris. Lorsque vous cliquez sur le bouton de la souris (gauche, roulette ou droite), une association secondaire est créée entre le pointeur et ce bouton via l’événement <a href="/uwp/api/windows.ui.xaml.uielement.pointermoved">PointerMoved</a> .</p></td>
</tr>
</tbody>
</table> 

## <a name="pointer-event-example"></a>Exemple d’événement de pointeur

Voici quelques extraits de code d’une application de suivi de pointeur de base qui montrent comment écouter et gérer les événements de plusieurs pointeurs, et obtenir diverses propriétés pour les pointeurs associés.

![Interface utilisateur de l’application pointeur](images/pointers/pointers1.gif)

**Télécharger cet exemple à partir de l' [exemple d’entrée de pointeur (Basic)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers.zip)**

### <a name="create-the-ui"></a>Créer l’interface utilisateur

Pour cet exemple, nous utilisons un [rectangle](/uwp/api/windows.ui.xaml.shapes.rectangle) ( `Target` ) comme entrée de pointeur consommant l’objet. La couleur de la cible change lorsque l’état du pointeur change.

Les détails de chaque pointeur sont affichés dans un [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) flottant qui suit le pointeur au fur et à mesure qu’il se déplace. Les événements de pointeur sont eux-mêmes signalés dans le [RichTextBlock](/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock) à droite du rectangle.

Il s’agit de l’Extensible Application Markup Language (XAML) de l’interface utilisateur dans cet exemple. 

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="*" />
        <ColumnDefinition Width="250"/>
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="*" />
        <RowDefinition Height="320" />
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <Canvas Name="Container" 
            Grid.Column="0"
            Grid.Row="1"
            HorizontalAlignment="Center" 
            VerticalAlignment="Center" 
            Margin="245,0" 
            Height="320"  Width="640">
        <Rectangle Name="Target" 
                    Fill="#FF0000" 
                    Stroke="Black" 
                    StrokeThickness="0"
                    Height="320" Width="640" />
    </Canvas>
    <Grid Grid.Column="1" Grid.Row="0" Grid.RowSpan="3">
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        <Button Name="buttonClear" 
                Grid.Row="0"
                Content="Clear"
                Foreground="White"
                HorizontalAlignment="Stretch" 
                VerticalAlignment="Stretch">
        </Button>
        <ScrollViewer Name="eventLogScrollViewer" Grid.Row="1" 
                        VerticalScrollMode="Auto" 
                        Background="Black">                
            <RichTextBlock Name="eventLog"  
                        TextWrapping="Wrap" 
                        Foreground="#FFFFFF" 
                        ScrollViewer.VerticalScrollBarVisibility="Visible" 
                        ScrollViewer.HorizontalScrollBarVisibility="Disabled"
                        Grid.ColumnSpan="2">
            </RichTextBlock>
        </ScrollViewer>
    </Grid>
</Grid>
```

### <a name="listen-for-pointer-events"></a>Écouter des événements de pointeur

Dans la plupart des cas, nous vous conseillons d’obtenir les informations sur le pointeur via la classe [**PointerRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.PointerRoutedEventArgs) du gestionnaire d’événements.

Si l’argument d’événement n’expose pas les détails de pointeur requis, vous pouvez accéder aux informations [**PointerPoint**](/uwp/api/Windows.UI.Input.PointerPoint) étendues exposées via les méthodes [**GetCurrentPoint**](/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.getcurrentpoint) et [**GetIntermediatePoints**](/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.getintermediatepoints) de [**PointerRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.PointerRoutedEventArgs).

Le code suivant configure l’objet dictionnaire global pour le suivi de chaque pointeur actif et identifie les différents écouteurs d’événements de pointeur pour l’objet cible.

```CSharp
// Dictionary to maintain information about each active pointer. 
// An entry is added during PointerPressed/PointerEntered events and removed 
// during PointerReleased/PointerCaptureLost/PointerCanceled/PointerExited events.
Dictionary<uint, Windows.UI.Xaml.Input.Pointer> pointers;

public MainPage()
{
    this.InitializeComponent();

    // Initialize the dictionary.
    pointers = new Dictionary<uint, Windows.UI.Xaml.Input.Pointer>();

    // Declare the pointer event handlers.
    Target.PointerPressed += 
        new PointerEventHandler(Target_PointerPressed);
    Target.PointerEntered += 
        new PointerEventHandler(Target_PointerEntered);
    Target.PointerReleased += 
        new PointerEventHandler(Target_PointerReleased);
    Target.PointerExited += 
        new PointerEventHandler(Target_PointerExited);
    Target.PointerCanceled += 
        new PointerEventHandler(Target_PointerCanceled);
    Target.PointerCaptureLost += 
        new PointerEventHandler(Target_PointerCaptureLost);
    Target.PointerMoved += 
        new PointerEventHandler(Target_PointerMoved);
    Target.PointerWheelChanged += 
        new PointerEventHandler(Target_PointerWheelChanged);

    buttonClear.Click += 
        new RoutedEventHandler(ButtonClear_Click);
}
```

### <a name="handle-pointer-events"></a>Gérer des événements de pointeur

Nous allons maintenant utiliser le retour d’interface utilisateur dans le cadre de la démonstration des gestionnaires d’événements de pointeur de base.

-   Ce gestionnaire gère l’événement [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed) . Nous allons ajouter l’événement au journal des événements, ajouter le pointeur au dictionnaire de pointeur actif et afficher les détails du pointeur.

    > [!NOTE]
    > Les événements [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed) et [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased) ne se produisent pas toujours par paires. Votre application doit écouter et gérer tout événement qui peut conclure un pointeur vers le dessous (par exemple, [**PointerExited**](/uwp/api/windows.ui.xaml.uielement.pointerexited), [**PointerCanceled**](/uwp/api/windows.ui.xaml.uielement.pointercanceled)et [**PointerCaptureLost**](/uwp/api/windows.ui.xaml.uielement.pointercapturelost)).      

```csharp
/// <summary>
/// The pointer pressed event handler.
/// PointerPressed and PointerReleased don't always occur in pairs. 
/// Your app should listen for and handle any event that can conclude 
/// a pointer down (PointerExited, PointerCanceled, PointerCaptureLost).
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
void Target_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Down: " + ptrPt.PointerId);

    // Lock the pointer to the target.
    Target.CapturePointer(e.Pointer);

    // Update event log.
    UpdateEventLog("Pointer captured: " + ptrPt.PointerId);

    // Check if pointer exists in dictionary (ie, enter occurred prior to press).
    if (!pointers.ContainsKey(ptrPt.PointerId))
    {
        // Add contact to dictionary.
        pointers[ptrPt.PointerId] = e.Pointer;
    }

    // Change background color of target when pointer contact detected.
    Target.Fill = new SolidColorBrush(Windows.UI.Colors.Green);

    // Display pointer details.
    CreateInfoPop(ptrPt);
}
```

-   Ce gestionnaire gère l’événement [**PointerEntered**](/uwp/api/windows.ui.xaml.uielement.pointerentered) . L’événement est ajouté au journal des événements, le pointeur est ajouté à la collection de pointeurs et les détails de pointeur sont affichés.

```csharp
/// <summary>
/// The pointer entered event handler.
/// We do not capture the pointer on this event.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerEntered(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Entered: " + ptrPt.PointerId);

    // Check if pointer already exists (if enter occurred prior to down).
    if (!pointers.ContainsKey(ptrPt.PointerId))
    {
        // Add contact to dictionary.
        pointers[ptrPt.PointerId] = e.Pointer;
    }

    if (pointers.Count == 0)
    {
        // Change background color of target when pointer contact detected.
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
    }

    // Display pointer details.
    CreateInfoPop(ptrPt);
}
```

-   Ce gestionnaire gère l’événement [**PointerMoved**](/uwp/api/windows.ui.xaml.uielement.pointermoved) . L’événement est ajouté au journal des événements et les détails de pointeur sont mis à jour.

    > [!Important]
    > L’entrée de la souris est associée à un seul pointeur affecté lors de la première détection de l’entrée de la souris. Lorsque vous cliquez sur le bouton de la souris (gauche, roulette ou droite), une association secondaire est créée entre le pointeur et ce bouton via l’événement [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed) . L’événement [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased) est déclenché uniquement lorsque ce même bouton de souris est relâché (aucun autre bouton ne peut être associé au pointeur tant que cet événement n’est pas terminé). En raison de cette association exclusive, les autres clics de bouton de souris sont routés via l’événement [**PointerMoved**](/uwp/api/windows.ui.xaml.uielement.pointermoved).     

```csharp
/// <summary>
/// The pointer moved event handler.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerMoved(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Multiple, simultaneous mouse button inputs are processed here.
    // Mouse input is associated with a single pointer assigned when 
    // mouse input is first detected. 
    // Clicking additional mouse buttons (left, wheel, or right) during 
    // the interaction creates secondary associations between those buttons 
    // and the pointer through the pointer pressed event. 
    // The pointer released event is fired only when the last mouse button 
    // associated with the interaction (not necessarily the initial button) 
    // is released. 
    // Because of this exclusive association, other mouse button clicks are 
    // routed through the pointer move event.          
    if (ptrPt.PointerDevice.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse)
    {
        if (ptrPt.Properties.IsLeftButtonPressed)
        {
            UpdateEventLog("Left button: " + ptrPt.PointerId);
        }
        if (ptrPt.Properties.IsMiddleButtonPressed)
        {
            UpdateEventLog("Wheel button: " + ptrPt.PointerId);
        }
        if (ptrPt.Properties.IsRightButtonPressed)
        {
            UpdateEventLog("Right button: " + ptrPt.PointerId);
        }
    }

    // Display pointer details.
    UpdateInfoPop(ptrPt);
}
```

-   Ce gestionnaire gère l’événement [**PointerWheelChanged**](/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged) . L’événement est ajouté au journal des événements, le pointeur est ajouté au tableau de pointeurs (si nécessaire) et les détails de pointeur sont affichés.

```csharp
/// <summary>
/// The pointer wheel event handler.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerWheelChanged(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Mouse wheel: " + ptrPt.PointerId);

    // Check if pointer already exists (for example, enter occurred prior to wheel).
    if (!pointers.ContainsKey(ptrPt.PointerId))
    {
        // Add contact to dictionary.
        pointers[ptrPt.PointerId] = e.Pointer;
    }

    // Display pointer details.
    CreateInfoPop(ptrPt);
}
```

-   Ce gestionnaire gère l’événement [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased) où le contact avec le digitaliseur est terminé. L’événement est ajouté au journal des événements, le pointeur est supprimé de la collection de pointeurs et les détails de pointeur sont mis à jour.

```csharp
/// <summary>
/// The pointer released event handler.
/// PointerPressed and PointerReleased don't always occur in pairs. 
/// Your app should listen for and handle any event that can conclude 
/// a pointer down (PointerExited, PointerCanceled, PointerCaptureLost).
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
void Target_PointerReleased(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Up: " + ptrPt.PointerId);

    // If event source is mouse or touchpad and the pointer is still 
    // over the target, retain pointer and pointer details.
    // Return without removing pointer from pointers dictionary.
    // For this example, we assume a maximum of one mouse pointer.
    if (ptrPt.PointerDevice.PointerDeviceType != Windows.Devices.Input.PointerDeviceType.Mouse)
    {
        // Update target UI.
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);

        DestroyInfoPop(ptrPt);

        // Remove contact from dictionary.
        if (pointers.ContainsKey(ptrPt.PointerId))
        {
            pointers[ptrPt.PointerId] = null;
            pointers.Remove(ptrPt.PointerId);
        }

        // Release the pointer from the target.
        Target.ReleasePointerCapture(e.Pointer);

        // Update event log.
        UpdateEventLog("Pointer released: " + ptrPt.PointerId);
    }
    else
    {
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
    }
}
```

-   Ce gestionnaire gère l’événement [**PointerExited**](/uwp/api/windows.ui.xaml.uielement.pointerexited) (lorsque le contact avec le digitaliseur est conservé). L’événement est ajouté au journal des événements, le pointeur est supprimé du tableau de pointeurs et les détails de pointeur sont mis à jour.

```csharp
/// <summary>
/// The pointer exited event handler.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerExited(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Pointer exited: " + ptrPt.PointerId);

    // Remove contact from dictionary.
    if (pointers.ContainsKey(ptrPt.PointerId))
    {
        pointers[ptrPt.PointerId] = null;
        pointers.Remove(ptrPt.PointerId);
    }

    if (pointers.Count == 0)
    {
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);
    }

    // Update the UI and pointer details.
    DestroyInfoPop(ptrPt);
}
```

-   Ce gestionnaire gère l’événement [**PointerCanceled**](/uwp/api/windows.ui.xaml.uielement.pointercanceled) . L’événement est ajouté au journal des événements, le pointeur est supprimé du tableau de pointeurs et les détails de pointeur sont mis à jour.

```csharp
/// <summary>
/// The pointer canceled event handler.
/// Fires for various reasons, including: 
///    - Touch contact canceled by pen coming into range of the surface.
///    - The device doesn't report an active contact for more than 100ms.
///    - The desktop is locked or the user logged off. 
///    - The number of simultaneous contacts exceeded the number supported by the device.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerCanceled(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Pointer canceled: " + ptrPt.PointerId);

    // Remove contact from dictionary.
    if (pointers.ContainsKey(ptrPt.PointerId))
    {
        pointers[ptrPt.PointerId] = null;
        pointers.Remove(ptrPt.PointerId);
    }

    if (pointers.Count == 0)
    {
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
    }

    DestroyInfoPop(ptrPt);
}
```

-   Ce gestionnaire gère l’événement [**PointerCaptureLost**](/uwp/api/windows.ui.xaml.uielement.pointercapturelost) . L’événement est ajouté au journal des événements, le pointeur est supprimé du tableau de pointeurs et les détails de pointeur sont mis à jour.

    > [!NOTE]
    > [**PointerCaptureLost**](/uwp/api/windows.ui.xaml.uielement.pointercapturelost) peut se produire au lieu de [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased). La capture de pointeur peut être perdue pour diverses raisons, notamment l’interaction de l’utilisateur, la capture par programmation d’un autre pointeur, l’appel de [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased).     

```csharp
/// <summary>
/// The pointer capture lost event handler.
/// Fires for various reasons, including: 
///    - User interactions
///    - Programmatic capture of another pointer
///    - Captured pointer was deliberately released
// PointerCaptureLost can fire instead of PointerReleased. 
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerCaptureLost(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Pointer capture lost: " + ptrPt.PointerId);

    if (pointers.Count == 0)
    {
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
    }

    // Remove contact from dictionary.
    if (pointers.ContainsKey(ptrPt.PointerId))
    {
        pointers[ptrPt.PointerId] = null;
        pointers.Remove(ptrPt.PointerId);
    }

    DestroyInfoPop(ptrPt);
}
```

### <a name="get-pointer-properties"></a>Obtenir les propriétés du pointeur

Comme indiqué précédemment, vous devez obtenir les informations de pointeur les plus détaillées via un objet [**Windows.UI.Input.PointerPoint**](/uwp/api/Windows.UI.Input.PointerPoint) obtenu par le biais des méthodes [**GetCurrentPoint**](/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.getcurrentpoint) et [**GetIntermediatePoints**](/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.getintermediatepoints) de [**PointerRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.PointerRoutedEventArgs). Les extraits de code suivants montrent comment procéder.

-   Nous commençons par créer un nouvel élément [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) pour chaque pointeur.

```csharp
/// <summary>
/// Create the pointer info popup.
/// </summary>
/// <param name="ptrPt">Reference to the input pointer.</param>
void CreateInfoPop(PointerPoint ptrPt)
{
    TextBlock pointerDetails = new TextBlock();
    pointerDetails.Name = ptrPt.PointerId.ToString();
    pointerDetails.Foreground = new SolidColorBrush(Windows.UI.Colors.White);
    pointerDetails.Text = QueryPointer(ptrPt);

    TranslateTransform x = new TranslateTransform();
    x.X = ptrPt.Position.X + 20;
    x.Y = ptrPt.Position.Y + 20;
    pointerDetails.RenderTransform = x;

    Container.Children.Add(pointerDetails);
}
```

-   Nous fournissons ensuite un moyen de mettre à jour les informations du pointeur dans un [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) existant associé à ce pointeur.

```csharp
/// <summary>
/// Update the pointer info popup.
/// </summary>
/// <param name="ptrPt">Reference to the input pointer.</param>
void UpdateInfoPop(PointerPoint ptrPt)
{
    foreach (var pointerDetails in Container.Children)
    {
        if (pointerDetails.GetType().ToString() == "Windows.UI.Xaml.Controls.TextBlock")
        {
            TextBlock textBlock = (TextBlock)pointerDetails;
            if (textBlock.Name == ptrPt.PointerId.ToString())
            {
                // To get pointer location details, we need extended pointer info.
                // We get the pointer info through the getCurrentPoint method
                // of the event argument. 
                TranslateTransform x = new TranslateTransform();
                x.X = ptrPt.Position.X + 20;
                x.Y = ptrPt.Position.Y + 20;
                pointerDetails.RenderTransform = x;
                textBlock.Text = QueryPointer(ptrPt);
            }
        }
    }
}
```

-   Pour finir, diverses propriétés de pointeur font l’objet d’une requête.

```csharp
/// <summary>
/// Get pointer details.
/// </summary>
/// <param name="ptrPt">Reference to the input pointer.</param>
/// <returns>A string composed of pointer details.</returns>
String QueryPointer(PointerPoint ptrPt)
{
    String details = "";

    switch (ptrPt.PointerDevice.PointerDeviceType)
    {
        case Windows.Devices.Input.PointerDeviceType.Mouse:
            details += "\nPointer type: mouse";
            break;
        case Windows.Devices.Input.PointerDeviceType.Pen:
            details += "\nPointer type: pen";
            if (ptrPt.IsInContact)
            {
                details += "\nPressure: " + ptrPt.Properties.Pressure;
                details += "\nrotation: " + ptrPt.Properties.Orientation;
                details += "\nTilt X: " + ptrPt.Properties.XTilt;
                details += "\nTilt Y: " + ptrPt.Properties.YTilt;
                details += "\nBarrel button pressed: " + ptrPt.Properties.IsBarrelButtonPressed;
            }
            break;
        case Windows.Devices.Input.PointerDeviceType.Touch:
            details += "\nPointer type: touch";
            details += "\nrotation: " + ptrPt.Properties.Orientation;
            details += "\nTilt X: " + ptrPt.Properties.XTilt;
            details += "\nTilt Y: " + ptrPt.Properties.YTilt;
            break;
        default:
            details += "\nPointer type: n/a";
            break;
    }

    GeneralTransform gt = Target.TransformToVisual(this);
    Point screenPoint;

    screenPoint = gt.TransformPoint(new Point(ptrPt.Position.X, ptrPt.Position.Y));
    details += "\nPointer Id: " + ptrPt.PointerId.ToString() +
        "\nPointer location (target): " + Math.Round(ptrPt.Position.X) + ", " + Math.Round(ptrPt.Position.Y) +
        "\nPointer location (container): " + Math.Round(screenPoint.X) + ", " + Math.Round(screenPoint.Y);

    return details;
}
```

## <a name="primary-pointer"></a>Pointeur principal
Certains périphériques d’entrée, tels qu’un digitaliseur tactile ou un pavé tactile, prennent en charge plus que le pointeur simple classique d’une souris ou d’un stylet (dans la plupart des cas, comme le Surface Hub prend en charge deux entrées de stylet). 

Utilisez la propriété **[IsPrimary](/uwp/api/windows.ui.input.pointerpointproperties.IsPrimary)** en lecture seule de la classe **[PointerPointerProperties](/uwp/api/windows.ui.input.pointerpointproperties)** pour identifier et différencier un seul pointeur principal (le pointeur principal est toujours le premier pointeur détecté au cours d’une séquence d’entrée). 

En identifiant le pointeur principal, vous pouvez l’utiliser pour émuler l’entrée de la souris ou du stylet, personnaliser les interactions ou fournir d’autres fonctionnalités ou interface utilisateur spécifiques.

> [!NOTE]
> Si le pointeur principal est libéré, annulé ou perdu pendant une séquence d’entrée, un pointeur d’entrée principal n’est pas créé tant qu’une nouvelle séquence d’entrée n’a pas été lancée (une séquence d’entrée se termine lorsque tous les pointeurs ont été libérés, annulés ou perdus).

## <a name="primary-pointer-animation-example"></a>Exemple d’animation du pointeur principal

Ces extraits de code montrent comment vous pouvez fournir des commentaires visuels spéciaux pour aider un utilisateur à faire la différence entre les entrées de pointeur dans votre application.

Cette application particulière utilise à la fois la couleur et l’animation pour mettre en surbrillance le pointeur principal.

![Application de pointeur avec retour visuel animé](images/pointers/pointers-usercontrol-animation.gif)

**Télécharger cet exemple à partir de l' [exemple d’entrée de pointeur (UserControl avec animation)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers-animation.zip)**

### <a name="visual-feedback"></a>Retour visuel

Nous définissons un **[UserControl](/uwp/api/windows.ui.xaml.controls.usercontrol)**, basé sur un objet **[ellipse](/uwp/api/windows.ui.xaml.shapes.ellipse)** XAML, qui met en évidence l’emplacement de chaque pointeur sur la zone de dessin et utilise une **[table de montage séquentiel](/uwp/api/windows.ui.xaml.media.animation.storyboard)** pour animer l’ellipse qui correspond au pointeur principal.

**Voici le code XAML :**

```xaml
<UserControl
    x:Class="UWP_Pointers.PointerEllipse"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:UWP_Pointers"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    d:DesignHeight="100"
    d:DesignWidth="100">

    <UserControl.Resources>
        <Style x:Key="EllipseStyle" TargetType="Ellipse">
            <Setter Property="Transitions">
                <Setter.Value>
                    <TransitionCollection>
                        <ContentThemeTransition/>
                    </TransitionCollection>
                </Setter.Value>
            </Setter>
        </Style>
        
        <Storyboard x:Name="myStoryboard">
            <!-- Animates the value of a Double property between 
            two target values using linear interpolation over the 
            specified Duration. -->
            <DoubleAnimation
              Storyboard.TargetName="ellipse"
              Storyboard.TargetProperty="(RenderTransform).(ScaleTransform.ScaleY)"  
              Duration="0:0:1" 
              AutoReverse="True" 
              RepeatBehavior="Forever" From="1.0" To="1.4">
            </DoubleAnimation>

            <!-- Animates the value of a Double property between 
            two target values using linear interpolation over the 
            specified Duration. -->
            <DoubleAnimation
              Storyboard.TargetName="ellipse"
              Storyboard.TargetProperty="(RenderTransform).(ScaleTransform.ScaleX)"  
              Duration="0:0:1" 
              AutoReverse="True" 
              RepeatBehavior="Forever" From="1.0" To="1.4">
            </DoubleAnimation>

            <!-- Animates the value of a Color property between 
            two target values using linear interpolation over the 
            specified Duration. -->
            <ColorAnimation 
                Storyboard.TargetName="ellipse" 
                EnableDependentAnimation="True" 
                Storyboard.TargetProperty="(Fill).(SolidColorBrush.Color)" 
                From="White" To="Red"  Duration="0:0:1" 
                AutoReverse="True" RepeatBehavior="Forever"/>
        </Storyboard>
    </UserControl.Resources>

    <Grid x:Name="CompositionContainer">
        <Ellipse Name="ellipse" 
        StrokeThickness="2" 
        Width="{x:Bind Diameter}" 
        Height="{x:Bind Diameter}"  
        Style="{StaticResource EllipseStyle}" />
    </Grid>
</UserControl>
```

Et voici le code-behind :
```csharp
using Windows.Foundation;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

// The User Control item template is documented at 
// https://go.microsoft.com/fwlink/?LinkId=234236

namespace UWP_Pointers
{
    /// <summary>
    /// Pointer feedback object.
    /// </summary>
    public sealed partial class PointerEllipse : UserControl
    {
        // Reference to the application canvas.
        Canvas canvas;

        /// <summary>
        /// Ellipse UI for pointer feedback.
        /// </summary>
        /// <param name="c">The drawing canvas.</param>
        public PointerEllipse(Canvas c)
        {
            this.InitializeComponent();
            canvas = c;
        }

        /// <summary>
        /// Gets or sets the pointer Id to associate with the PointerEllipse object.
        /// </summary>
        public uint PointerId
        {
            get { return (uint)GetValue(PointerIdProperty); }
            set { SetValue(PointerIdProperty, value); }
        }
        // Using a DependencyProperty as the backing store for PointerId.  
        // This enables animation, styling, binding, etc...
        public static readonly DependencyProperty PointerIdProperty =
            DependencyProperty.Register("PointerId", typeof(uint), 
                typeof(PointerEllipse), new PropertyMetadata(null));


        /// <summary>
        /// Gets or sets whether the associated pointer is Primary.
        /// </summary>
        public bool PrimaryPointer
        {
            get { return (bool)GetValue(PrimaryPointerProperty); }
            set
            {
                SetValue(PrimaryPointerProperty, value);
            }
        }
        // Using a DependencyProperty as the backing store for PrimaryPointer.  
        // This enables animation, styling, binding, etc...
        public static readonly DependencyProperty PrimaryPointerProperty =
            DependencyProperty.Register("PrimaryPointer", typeof(bool), 
                typeof(PointerEllipse), new PropertyMetadata(false));


        /// <summary>
        /// Gets or sets the ellipse style based on whether the pointer is Primary.
        /// </summary>
        public bool PrimaryEllipse 
        {
            get { return (bool)GetValue(PrimaryEllipseProperty); }
            set
            {
                SetValue(PrimaryEllipseProperty, value);
                if (value)
                {
                    SolidColorBrush fillBrush = 
                        (SolidColorBrush)Application.Current.Resources["PrimaryFillBrush"];
                    SolidColorBrush strokeBrush = 
                        (SolidColorBrush)Application.Current.Resources["PrimaryStrokeBrush"];

                    ellipse.Fill = fillBrush;
                    ellipse.Stroke = strokeBrush;
                    ellipse.RenderTransform = new CompositeTransform();
                    ellipse.RenderTransformOrigin = new Point(.5, .5);
                    myStoryboard.Begin();
                }
                else
                {
                    SolidColorBrush fillBrush = 
                        (SolidColorBrush)Application.Current.Resources["SecondaryFillBrush"];
                    SolidColorBrush strokeBrush = 
                        (SolidColorBrush)Application.Current.Resources["SecondaryStrokeBrush"];
                    ellipse.Fill = fillBrush;
                    ellipse.Stroke = strokeBrush;
                }
            }
        }
        // Using a DependencyProperty as the backing store for PrimaryEllipse.  
        // This enables animation, styling, binding, etc...
        public static readonly DependencyProperty PrimaryEllipseProperty =
            DependencyProperty.Register("PrimaryEllipse", 
                typeof(bool), typeof(PointerEllipse), new PropertyMetadata(false));


        /// <summary>
        /// Gets or sets the diameter of the PointerEllipse object.
        /// </summary>
        public int Diameter
        {
            get { return (int)GetValue(DiameterProperty); }
            set { SetValue(DiameterProperty, value); }
        }
        // Using a DependencyProperty as the backing store for Diameter.  This enables animation, styling, binding, etc...
        public static readonly DependencyProperty DiameterProperty =
            DependencyProperty.Register("Diameter", typeof(int), 
                typeof(PointerEllipse), new PropertyMetadata(120));
    }
}
```

### <a name="create-the-ui"></a>Créer l’interface utilisateur
L’interface utilisateur dans cet exemple est limitée à la **[zone de dessin](/uwp/api/windows.ui.xaml.controls.canvas)** d’entrée où nous effectuons le suivi de tous les pointeurs et rendez les indicateurs de pointeur et l’animation du pointeur principal (le cas échéant), ainsi qu’une barre d’en-tête contenant un compteur de pointeur et un identificateur de pointeur principal.

Voici le MainPage. xaml :

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" 
                Orientation="Horizontal" 
                Grid.Row="0">
        <StackPanel.Transitions>
            <TransitionCollection>
                <AddDeleteThemeTransition/>
            </TransitionCollection>
        </StackPanel.Transitions>
        <TextBlock x:Name="Header" 
                    Text="Basic pointer tracking sample - IsPrimary" 
                    Style="{ThemeResource HeaderTextBlockStyle}" 
                    Margin="10,0,0,0" />
        <TextBlock x:Name="PointerCounterLabel"
                    VerticalAlignment="Center"                 
                    Style="{ThemeResource BodyTextBlockStyle}"
                    Text="Number of pointers: " 
                    Margin="50,0,0,0"/>
        <TextBlock x:Name="PointerCounter"
                    VerticalAlignment="Center"                 
                    Style="{ThemeResource BodyTextBlockStyle}"
                    Text="0" 
                    Margin="10,0,0,0"/>
        <TextBlock x:Name="PointerPrimaryLabel"
                    VerticalAlignment="Center"                 
                    Style="{ThemeResource BodyTextBlockStyle}"
                    Text="Primary: " 
                    Margin="50,0,0,0"/>
        <TextBlock x:Name="PointerPrimary"
                    VerticalAlignment="Center"                 
                    Style="{ThemeResource BodyTextBlockStyle}"
                    Text="n/a" 
                    Margin="10,0,0,0"/>
    </StackPanel>
    
    <Grid Grid.Row="1">
        <!--The canvas where we render the pointer UI.-->
        <Canvas x:Name="pointerCanvas"/>
    </Grid>
</Grid>
```

### <a name="handle-pointer-events"></a>Gérer des événements de pointeur

Enfin, nous définissons nos gestionnaires d’événements de pointeurs de base dans le code-behind MainPage.xaml.cs. Nous ne reproduirons pas le code ici, car les concepts de base ont été abordés dans l’exemple précédent, mais vous pouvez télécharger l’exemple de travail à partir de l' [exemple d’entrée de pointeur (UserControl avec animation)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers-animation.zip).

## <a name="related-articles"></a>Articles connexes

### <a name="topic-samples"></a>Exemples de la rubrique

- [Exemple d’entrée de pointeur (Basic)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers.zip)
- [Exemple d’entrée de pointeur (UserControl avec animation)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers-animation.zip)

### <a name="other-samples"></a>Autres exemples

- [Exemple d’entrée de base](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Exemple d’entrée à faible latence](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Exemple de mode d’interaction utilisateur](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Exemples de visuels de focus](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>Exemples d’archive

- [Entrée : exemple d’événements d’entrée utilisateur XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [Entrée : exemple de fonctionnalités de périphériques](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [Entrée : manipulations et gestes, exemple](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [Entrée : exemple de test de positionnement tactile](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20desktop%20samples/%5BC%2B%2B%5D-Windows%208%20desktop%20samples/C%2B%2B/Windows%208%20desktop%20samples/Input%20Touch%20hit%20testing%20sample)
- [Exemple de défilement XAML, panoramique et zoom](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [Entrée : exemple d’entrée manuscrite simplifiée](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Simplified%20ink%20sample)