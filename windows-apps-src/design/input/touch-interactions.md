---
Description: Créez des applications Windows avec des expériences d’interaction utilisateur intuitives et distinctives, optimisées pour la fonctionnalité tactile, mais cohérentes de façon fonctionnelle sur les périphériques d’entrée.
title: Interactions tactiles
ms.assetid: DA6EBC88-EB18-4418-A98A-457EA1DEA88A
label: Touch interactions
template: detail.hbs
keywords: entrées tactiles, pointeur, entrées, interactions avec l’utilisateur
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 302a1f39d85902b229642b60f483b40f51209a72
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750385"
---
# <a name="touch-interactions"></a>Interactions tactiles


Concevez votre application en partant du principe que l’entrée tactile sera la principale méthode d’entrée de vos utilisateurs. Si vous utilisez des contrôles UWP, la prise en charge du pavé tactile, de la souris et du stylet ne nécessite pas de programmation supplémentaire, car les applications UWP proposent cette fonctionnalité gratuitement.

Sachez cependant qu’une interface utilisateur optimisée pour les entrées tactiles ne se révèle pas toujours supérieure à une interface utilisateur classique. Les deux présentent des avantages et des inconvénients qui sont propres à une technologie et une application. Lors du passage à une interface utilisateur tactile, il est important de comprendre les principales différences entre les entrées tactiles, les pavés tactiles, le stylet, la souris et le clavier.

> **API importantes**: [**Windows. UI. Xaml. Input**](/uwp/api/Windows.UI.Xaml.Input), [**Windows. UI. Core**](/uwp/api/Windows.UI.Core), [**Windows. Devices. Input**](/uwp/api/Windows.Devices.Input)


De nombreux appareils sont équipés d’écrans à interaction tactile multipoint qui prennent en charge l’utilisation de plusieurs doigts (ou contacts tactiles) en tant qu’entrée. Les contacts tactiles et leurs déplacements, sont interprétés comme des mouvements et des manipulations tactiles pour prendre en charge diverses interactions utilisateur.

L’application Windows comprend un certain nombre de mécanismes différents pour gérer les entrées tactiles, ce qui vous permet de créer une expérience immersive que vos utilisateurs peuvent explorer en toute confiance. Ici, nous aborderons les bases de l’utilisation des entrées tactiles dans une application Windows.

Les interactions tactiles nécessitent trois éléments :

-   Un écran tactile.
-   Le contact direct (ou la fonctionnalité de proximité, si l’écran est doté de capteurs de proximité et prend en charge le pointage détection) d’un ou plusieurs doigts sur cet écran.
-   Déplacement des contacts tactiles (ou absence de déplacement, basé sur un seuil de temps).

Les données d’entrée fournies par le capteur tactile peuvent :

-   Être interprétées comme un mouvement physique de manipulation directe d’un ou plusieurs éléments d’interface utilisateur (par exemple, mouvement panoramique, rotation, redimensionnement ou déplacement). En revanche, l’interaction avec un élément par le biais de sa fenêtre de propriétés ou d’une autre boîte de dialogue ou interface utilisateur est considérée comme étant une manipulation indirecte.
-   Faire office de méthode d’entrée alternative, à la manière d’une souris ou d’un stylet.
-   Compléter ou modifier des aspects d’autres méthodes d’entrée, par exemple en maculant un trait d’encre dessiné avec un stylet.

En règle générale, l’entrée tactile implique la manipulation directe d’un élément à l’écran. L’élément répond immédiatement à n’importe quel contact tactile dans sa zone de test et réagit de manière appropriée pour tous les mouvements des contacts tactiles qui s’ensuivent, notamment la suppression.

Les interactions et mouvements tactiles personnalisés doivent être conçus avec soin. Ils doivent être détectables, réactifs et intuitifs et permettre aux utilisateurs d’explorer votre application en toute confiance.

Vérifiez que les fonctionnalités de l’application sont exposées de façon cohérente sur chaque type de périphérique d’entrée pris en charge. Si nécessaire, utilisez une forme de mode d’entrée indirect, par exemple la saisie de texte pour les interactions avec le clavier ou les fonctions d’une interface utilisateur pour la souris et le stylet.

N’oubliez pas que les périphériques d’entrée traditionnels (comme la souris et clavier), sont familiers et accrocheurs pour de nombreux utilisateurs. Ils peuvent proposer la vitesse, la précision et le retour tactile que les entrées tactiles n’offrent pas.

En offrant des expériences d’interactions uniques et différenciées pour tous ces périphériques d’entrée, vous prendrez en charge le plus large éventail possible de fonctionnalités et de préférences, vous vous adresserez au plus grand nombre d’utilisateurs et vous attirerez ainsi davantage de clients vers votre application.

## <a name="compare-touch-interaction-requirements"></a>Comparer les critères de l’interaction tactile

Le tableau suivant présente quelques-unes des différences entre les périphériques d’entrée que vous devez prendre en compte lors de la conception d’applications Windows optimisées tactiles.

<table>
<tbody><tr><th>Factor</th><th>Interactions tactiles</th><th>Interactions à l’aide de la souris, du clavier, du stylo/stylet</th><th>Pavé tactile</th></tr>
<tr><td rowspan="3">Precision</td><td>La zone de contact au bout du doigt est plus importante qu’une simple coordonnées x-y, ce qui augmente le risque d’activations involontaires de commandes.</td><td>La souris et le stylo/stylet répondent à une coordonnée x-y précise.</td><td>Comme la souris.</td></tr>
<tr><td>La forme de la zone de contact change tout au long du mouvement.  </td><td>Les mouvements de la souris et les traits du stylo/stylet répondent à des coordonnées x-y précises. Le focus du clavier est explicite.</td><td>Comme la souris.</td></tr>
<tr><td>Il n’y a pas de curseur de souris pour aider au ciblage.</td><td>Le curseur de la souris, le curseur du stylo/stylet et le focus du clavier constituent tous une aide au ciblage.</td><td>Comme la souris.</td></tr>
<tr><td rowspan="3">Anatomie humaine</td><td>Les mouvements effectués avec le bout du doigt sont imprécis, car le traçage d’une ligne droite avec un ou plusieurs doigts est difficile à réaliser. Cela s’explique par la courbure des articulations de la main et le nombre d’articulations impliquées dans le mouvement.</td><td>Il est plus facile de tracer un mouvement de ligne droite avec la souris ou le stylo/stylet, car la main qui les contrôle parcourt une distance plus courte que le curseur sur l’écran.</td><td>Comme la souris.</td></tr>
<tr><td>Certaines zones situées sur la surface tactile d’un périphérique d’affichage peuvent être difficiles à atteindre en raison de la posture des doigts et de la prise en main du périphérique par l’utilisateur.</td><td>La souris et le stylo/stylet peuvent accéder à toutes les parties de l’écran, et n’importe quel contrôle est accessible par le clavier via l’ordre des onglets. </td><td>La posture des doigts et la prise en main peuvent poser problème.</td></tr>
<tr><td>Le bout des doigts ou la main de l’utilisateur peuvent masquer des objets. C’est ce que l’on appelle l’« occlusion ».</td><td>Les périphériques d’entrée indirects ne provoquent pas d’occlusion.</td><td>Comme la souris.</td></tr>
<tr><td>État de l’objet</td><td>L’interaction tactile utilise un modèle à deux états : la surface tactile du périphérique d’affichage est touchée (activée) ou non touchée (désactivée) par l’utilisateur. Il n’existe pas d’état de pointage susceptible de déclencher un retour visuel supplémentaire.</td><td>
<p>Une souris, un stylo/stylet et un clavier exposent tous un modèle à trois états : soulevé (activé), appuyé (activé) et pointé (focus).</p>
<p>Le pointage permet à l’utilisateur d’explorer et de découvrir les éléments à l’aide d’info-bulles associées aux éléments de l’interface utilisateur. Les effets de pointage et de focus peuvent transmettre les objets qui sont interactifs et aident également au ciblage. 
</p>
</td><td>Comme la souris.</td></tr>
<tr><td rowspan="2">Interaction évoluée</td><td>Prend en charge l’interaction tactile multipoint : plusieurs points d’entrée (bout des doigts) sur une surface tactile.</td><td>Prend en charge un point d’entrée unique.</td><td>Comme l’entrée tactile.</td></tr>
<tr><td>Prend en charge la manipulation directe des objets par le biais de gestes tels que l’appui, le glissement, le pincement et la rotation.</td><td>Ne prend pas en charge la manipulation directe, car la souris, le stylo/stylet et le clavier sont des périphériques d’entrée indirects.</td><td>Comme la souris.</td></tr>
</tbody></table>

> [!NOTE]
> L’entrée indirecte a bénéficié de plus de 25 ans d’amélioration. Les fonctions comme les info-bulles déclenchées par le pointage ont été conçues pour résoudre les problèmes d’exploration de l’interface utilisateur spécifiques aux entrées à l’aide du pavé tactile, de la souris, du stylo/stylet et du clavier. Les fonctionnalités d’interface utilisateur de ce genre ont été repensées pour enrichir l’expérience de la saisie tactile, sans compromettre l’expérience utilisateur sur les autres appareils.

 

## <a name="use-touch-feedback"></a>Utiliser le retour tactile

Les commentaires visuels appropriés lors des interactions avec votre application aident les utilisateurs à reconnaître, apprendre et s’adapter à la façon dont leurs interactions sont interprétées par l’application et la plate-forme Windows. Le retour visuel peut indiquer les interactions réussies, transmettre l’état du système, améliorer le sentiment de contrôle, réduire les erreurs, aider les utilisateurs à comprendre le système et le périphérique d’entrée et encourager l’interaction.

Le retour visuel est essentiel quand l’utilisateur doit réaliser, avec la fonction tactile, des activités qui demandent de l’exactitude et de la précision selon l’endroit concerné. Affichez le retour, quels que soient l’emplacement et le moment de la détection de l’entrée tactile, pour aider l’utilisateur à comprendre toutes les méthodes de ciblage personnalisé qui sont définies par votre application et ses contrôles.


## <a name="targeting"></a>Ciblage

Le ciblage est optimisé par les éléments suivants :

-   Taille des cibles tactiles

    Des instructions claires concernant les tailles garantissent une interface utilisateur confortable contenant des objets et des contrôles que l’utilisateur peut cibler facilement et en toute sécurité.

-   Géométrie de contact

    La totalité de la zone de contact du doigt détermine l’objet cible le plus probable.

-   Nettoyage

    L’utilisateur peut facilement recibler les éléments au sein d’un groupe en glissant le doigt entre eux (par exemple, des cases d’option). L’élément actif est activé lorsque l’utilisateur relâche le doigt.

-   Va-et-vient

    L’utilisateur peut facilement recibler des éléments compacts (par exemple, des liens hypertexte) en appuyant avec le doigt et, sans le faire glisser, en effectuant un mouvement de va-et-vient sur les éléments. Pour éviter l’occlusion, l’élément est identifié par une info-bulle ou la barre d’état. Il est activé dès que l’utilisateur relâche le doigt.

## <a name="accuracy"></a>Précision

Pour les interactions imprécises, utilisez :

-   des points d’ancrage qui permettent à l’utilisateur de s’arrêter plus facilement aux emplacements souhaités quand il interagit avec le contenu ;
-   « Rails » directionnels qui peuvent vous aider avec le panoramique vertical ou horizontal, même lorsque la main se déplace dans un petit arc. Pour plus d’informations, consultez [instructions pour le panorama](guidelines-for-panning.md).

## <a name="occlusion"></a>Occlusion

Pour éviter l’occlusion du doigt et de la main, respectez les recommandations suivantes :

-   Taille et positionnement des éléments d’interface utilisateur

    Créez des éléments d’interface utilisateur suffisamment grands pour qu’ils ne soient pas complètement recouverts par la zone de contact du doigt.

    Positionnez autant que possible les menus et les fenêtres indépendantes au-dessus de la zone de contact.

-   Info-bulles

    Affichez des info-bulles quand un utilisateur maintient son doigt sur un objet. Cela est utile pour décrire la fonctionnalité d’un objet. L’utilisateur peut retirer le bout de son doigt de l’objet pour éviter d’appeler l’info-bulle.

    Pour les petits objets, décalez les info-bulles afin qu’elles ne soient pas recouvertes par la zone de contact du doigt. Cela permet d’améliorer le ciblage.

-   Poignées de précision

    Pour les actions de précision (par exemple, la sélection de texte), insérez des poignées de sélection décalées afin d’augmenter le degré d’exactitude. Pour plus d’informations, voir [Recommandations en matière de sélection de texte et d’images (applications Windows Runtime)](guidelines-for-textselection.md).

## <a name="timing"></a>Minutage

Évitez les modifications en mode chronométré au profit de la manipulation directe. Celle-ci simule le maniement direct et en temps réel d’un objet. L’objet réagit directement au mouvement du doigt.

À l’inverse, en mode chronométré, l’interaction se produit après le geste tactile. Généralement, les interactions chronométrées dépendent de seuils invisibles, tels que le temps, la distance ou la vitesse, pour déterminer la commande à effectuer. Elles ne produisent aucun retour visuel tant que le système n’a pas effectué l’action.

La manipulation directe offre un certain nombre d’avantages par rapport aux interactions chronométrées :

-   Le retour visuel instantané au cours de l’interaction permet à l’utilisateur de se sentir davantage impliqué, confiant et en contrôle.
-   Les manipulations directes permettent de sécuriser l’exploration d’un système, car elles sont réversibles, c’est-à-dire que l’utilisateur peut facilement revenir en arrière et annuler ses actions d’une manière logique et intuitive.
-   Les interactions qui affectent directement les objets et qui imitent les gestes réels sont plus intuitives, plus visibles et plus faciles à retenir. Elles ne dépendent pas d’interactions obscures ou abstraites.
-   Les interactions chronométrées peuvent être difficiles à effectuer, étant donné que l’utilisateur doit atteindre des seuils arbitraires et invisibles.

En outre, nous vous encourageons vivement à tenir compte des recommandations suivantes :

-   Ne classez pas les manipulations en fonction du nombre de doigts utilisés.
-   Les interactions doivent prendre en charge les manipulations composées. Par exemple, resserrez les doigts pour zoomer tout en les faisant glisser pour effectuer un mouvement panoramique.
-   Ne classez pas les interactions en fonction du temps. Une même interaction doit avoir le même résultat, quel que soit le temps pris pour l’effectuer. Les activations temporelles impliquent des délais obligatoires à respecter par l’utilisateur. Par ailleurs, elles portent atteinte non seulement à la nature immersive des manipulations directes, mais également à la perception de la réactivité du système.

    > [!NOTE]
    > Une exception à cette question est l’endroit où vous utilisez des interactions chronométrées spécifiques pour faciliter l’apprentissage et l’exploration (par exemple, appuyez et maintenez).

-   Les descriptions appropriées et les signaux visuels influent très favorablement sur l’utilisation des interactions avancées.


## <a name="app-views"></a>Vues d’applications


Ajustez l’expérience d’interaction utilisateur par le biais des paramètres de panoramique/défilement et zoom de vos vues d’applications. La vue d’une application régit la manière dont un utilisateur accède à cette dernière et manipule votre application et son contenu. Les vues fournissent également des comportements tels que l’inertie, le rebond de limite de zone de contenu et les points d’ancrage.

Les paramètres de panoramique et de défilement du contrôle [**ScrollViewer**](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) déterminent la façon dont les utilisateurs naviguent dans une vue unique, lorsque le contenu de la vue ne tient pas dans la fenêtre d’affichage. Une vue unique est par exemple la page d’un magazine ou d’un livre, la structure de dossiers d’un ordinateur, une bibliothèque de documents ou un album photo.

Les paramètres de zoom s’appliquent à la fois au zoom optique (pris en charge par le contrôle [**ScrollViewer**](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer)) et au contrôle [**Semantic Zoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom). Le zoom sémantique est une technique optimisée pour l’interaction tactile applicable à la présentation et à la navigation de grands ensembles de contenus ou de données associés dans une même vue. Il fonctionne à l’aide de deux modes de classification (ou niveaux de zoom) distincts. Cette fonctionnalité est identique au mouvement panoramique et au défilement simple au sein d’une même vue. Ces types de défilements peuvent être utilisés en association avec le zoom sémantique.

Utilisez les événements et les vues de l’application pour modifier les comportements de panoramique/défilement et de zoom. Vous pouvez ainsi offrir une expérience d’interaction aussi fluide que possible via la gestion de pointeur et les événements de mouvement.

Pour plus d’informations concernant les vues d’applications, voir [Contrôles, dispositions et texte](../basics/index.md).

## <a name="custom-touch-interactions"></a>Personnaliser des interactions tactiles


Si vous implémentez votre propre prise en charge d’interaction, gardez à l’esprit que les utilisateurs s’attendent à disposer d’une expérience intuitive impliquant une interaction directe avec les éléments d’interface utilisateur de votre application. Nous vous recommandons de modeler vos interactions personnalisées sur les bibliothèques de contrôles de plateforme pour des raisons de cohérence et de simplicité de détection. Les contrôles de ces bibliothèques fournissent une expérience d’interaction utilisateur complète, notamment pour les interactions standard, les effets physiques animés, le retour visuel et l’accessibilité. Ne créez des interactions personnalisées que pour répondre à des exigences claires et bien définies, notamment en l’absence d’interactions de base prenant en charge votre scénario.

Pour assurer une prise en charge personnalisée des entrées tactiles, vous pouvez gérer divers événements [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement). Ces événements sont regroupés en trois niveaux d’abstraction.

-   Les événements de mouvement statique sont déclenchés une fois que l’interaction se termine. Les événements de mouvement incluent [**taraudé**](/uwp/api/windows.ui.xaml.uielement.tapped), [**DoubleTapped**](/uwp/api/windows.ui.xaml.uielement.doubletapped), [**RightTapped**](/uwp/api/windows.ui.xaml.uielement.righttapped)et [**Holding**](/uwp/api/windows.ui.xaml.uielement.holding).

    Vous pouvez désactiver les événements de mouvement sur des éléments spécifiques en définissant [**IsTapEnabled**](/uwp/api/windows.ui.xaml.uielement.istapenabled), [**IsDoubleTapEnabled**](/uwp/api/windows.ui.xaml.uielement.isdoubletapenabled), [**IsRightTapEnabled**](/uwp/api/windows.ui.xaml.uielement.isrighttapenabled) et [**IsHoldingEnabled**](/uwp/api/windows.ui.xaml.uielement.isholdingenabled) sur **false**.

-   Les événements de pointeur tels que [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed) et [**PointerMoved**](/uwp/api/windows.ui.xaml.uielement.pointermoved) fournissent des détails de bas niveau pour chaque contact tactile, y compris le mouvement du pointeur et la possibilité de distinguer les événements d’appui et de déclenchement.

    Un pointeur est un type d’entrée générique avec un mécanisme d’événements unifiés. Il expose les informations de base (telles que la position de l’écran) sur la source d’entrée active (entrée tactile, pavé tactile, souris ou stylet).

-   Les événements de mouvement de manipulation, tels que [**ManipulationStarted**](/uwp/api/windows.ui.xaml.uielement.manipulationstarted), indiquent une interaction en cours. L’utilisateur les déclenche en touchant un élément. Ils se poursuivent jusqu’à ce que l’utilisateur mette fin au contact ou que la manipulation soit annulée.

    Les événements de manipulation comprennent les interactions tactiles multipoint, telles que le zoom, le mouvement panoramique ou la rotation, et des interactions qui utilisent des données d’inertie et de vitesse, telles que le glissement. Les informations fournies par les événements de manipulation ne reflètent pas l’interaction qui s’est produite, mais comprennent des données, telles que la position, le delta de translation et la vitesse. Vous pouvez utiliser ces données tactiles pour déterminer le type d’interaction qui doit être produit.

Voici l’ensemble de mouvements tactiles de base pris en charge par la plateforme UWP.

| Nom           | Type                 | Description                                                                            |
|----------------|----------------------|----------------------------------------------------------------------------------------|
| Taper            | Action statique       | Brève pression de l’écran avec un doigt.                                            |
| Appuyer de manière prolongée | Action statique       | Pression prolongée de l’écran avec un doigt.                                      |
| Diapositive          | Action de manipulation | Pression de l’écran avec un ou plusieurs doigts et déplacement dans une même direction.                   |
| Balayer          | Action de manipulation | Pression de l’écran avec un ou plusieurs doigts et déplacement sur une courte distance dans une même direction.  |
| Tourner           | Action de manipulation | Pression de l’écran avec deux doigts ou plus et mouvement en arc de cercle de haut en bas ou de bas en haut. |
| Pincer          | Action de manipulation | Pression de l’écran avec deux doigts ou plus, puis rapprochement des doigts.                         |
| Stretch        | Action de manipulation | Pression de l’écran avec deux doigts ou plus, puis étirement des doigts.                           |

 

<!-- mijacobs: Removing for now. We don't have a real page to link to yet. 
For more info about gestures, manipulations, and interactions, see [Custom user interactions](custom-user-input-portal.md).
-->

## <a name="gesture-events"></a>Événements de mouvement


Pour plus d’informations concernant les contrôles individuels, voir [Liste de contrôles](../controls-and-patterns/index.md).

## <a name="pointer-events"></a>Événements de pointeur


Les événements de pointeur sont déclenchés par diverses sources d’entrée actives, y compris les entrées tactiles, le pavé tactile, le stylet et la souris (ils remplacent les événements de souris classiques.)

Les événements de pointeur sont basés sur un point d’entrée unique (doigt, pointe du stylet, curseur de la souris) et ne prennent pas en charge les interactions basées sur la vitesse.

Voici une liste des événements de pointeur et leur argument d’événement associé.

| Événement ou classe                                                       | Description                                                   |
|----------------------------------------------------------------------|---------------------------------------------------------------|
| [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed)             | Se produit lorsqu’un seul doigt touche l’écran.               |
| [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased)           | Se produit quand ce même contact tactile est relâché.                |
| [**PointerMoved**](/uwp/api/windows.ui.xaml.uielement.pointermoved)                 | Se produit lorsque l’on fait glisser le pointeur sur l’écran.         |
| [**PointerEntered**](/uwp/api/windows.ui.xaml.uielement.pointerentered)             | Se produit lorsqu’un pointeur entre dans la zone de test de positionnement d’un élément. |
| [**PointerExited**](/uwp/api/windows.ui.xaml.uielement.pointerexited)               | Se produit lorsqu’un pointeur quitte la zone de test de positionnement d’un élément.  |
| [**PointerCanceled**](/uwp/api/windows.ui.xaml.uielement.pointercanceled)           | Se produit lorsqu’un contact tactile est anormalement perdu.               |
| [**PointerCaptureLost**](/uwp/api/windows.ui.xaml.uielement.pointercapturelost)     | Se produit lorsqu’une capture du pointeur est effectuée par un autre élément.    |
| [**PointerWheelChanged**](/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)   | Se produit lorsque la valeur delta d’une roulette de la souris change et lorsque le pavé tactile est pinceé.         |
| [**PointerRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.PointerRoutedEventArgs) | Fournit des données pour tous les événements de pointeur.                         |

 

L’exemple suivant montre comment utiliser les événements [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed), [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased)et [**PointerExited**](/uwp/api/windows.ui.xaml.uielement.pointerexited) pour gérer une interaction TAP sur un objet [**rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) .

Pour commencer, un élément [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) nommé `touchRectangle` est créé en XAML (Extensible Application Markup Language).

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Rectangle Name="touchRectangle"
           Height="100" Width="200" Fill="Blue" />
</Grid>
```
Ensuite, les écouteurs pour les événements [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed), [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased)et [**PointerExited**](/uwp/api/windows.ui.xaml.uielement.pointerexited) sont spécifiés.

```cpp
MainPage::MainPage()
{
    InitializeComponent();

    // Pointer event listeners.
    touchRectangle->PointerPressed += ref new PointerEventHandler(this, &MainPage::touchRectangle_PointerPressed);
    touchRectangle->PointerReleased += ref new PointerEventHandler(this, &MainPage::touchRectangle_PointerReleased);
    touchRectangle->PointerExited += ref new PointerEventHandler(this, &MainPage::touchRectangle_PointerExited);
}
```

```cs
public MainPage()
{
    this.InitializeComponent();

    // Pointer event listeners.
    touchRectangle.PointerPressed += touchRectangle_PointerPressed;
    touchRectangle.PointerReleased += touchRectangle_PointerReleased;
    touchRectangle.PointerExited += touchRectangle_PointerExited;
}
```

```vb
Public Sub New()

    ' This call is required by the designer.
    InitializeComponent()

    ' Pointer event listeners.
    AddHandler touchRectangle.PointerPressed, AddressOf touchRectangle_PointerPressed
    AddHandler touchRectangle.PointerReleased, AddressOf Me.touchRectangle_PointerReleased
    AddHandler touchRectangle.PointerExited, AddressOf touchRectangle_PointerExited

End Sub
```

Enfin, le gestionnaire d’événements [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed) augmente les valeurs [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) et [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) de [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle), tandis que les gestionnaires d’événements [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased) et [**PointerExited**](/uwp/api/windows.ui.xaml.uielement.pointerexited) redéfinissent **Height** et **Width** sur leurs valeurs de départ.

```cpp
// Handler for pointer exited event.
void MainPage::touchRectangle_PointerExited(Object^ sender, PointerRoutedEventArgs^ e)
{
    Rectangle^ rect = (Rectangle^)sender;

    // Pointer moved outside Rectangle hit test area.
    // Reset the dimensions of the Rectangle.
    if (nullptr != rect)
    {
        rect->Width = 200;
        rect->Height = 100;
    }
}

// Handler for pointer released event.
void MainPage::touchRectangle_PointerReleased(Object^ sender, PointerRoutedEventArgs^ e)
{
    Rectangle^ rect = (Rectangle^)sender;

    // Reset the dimensions of the Rectangle.
    if (nullptr != rect)
    {
        rect->Width = 200;
        rect->Height = 100;
    }
}

// Handler for pointer pressed event.
void MainPage::touchRectangle_PointerPressed(Object^ sender, PointerRoutedEventArgs^ e)
{
    Rectangle^ rect = (Rectangle^)sender;

    // Change the dimensions of the Rectangle.
    if (nullptr != rect)
    {
        rect->Width = 250;
        rect->Height = 150;
    }
}
```

```cs
// Handler for pointer exited event.
private void touchRectangle_PointerExited(object sender, PointerRoutedEventArgs e)
{
    Rectangle rect = sender as Rectangle;

    // Pointer moved outside Rectangle hit test area.
    // Reset the dimensions of the Rectangle.
    if (null != rect)
    {
        rect.Width = 200;
        rect.Height = 100;
    }
}
// Handler for pointer released event.
private void touchRectangle_PointerReleased(object sender, PointerRoutedEventArgs e)
{
    Rectangle rect = sender as Rectangle;

    // Reset the dimensions of the Rectangle.
    if (null != rect)
    {
        rect.Width = 200;
        rect.Height = 100;
    }
}

// Handler for pointer pressed event.
private void touchRectangle_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    Rectangle rect = sender as Rectangle;

    // Change the dimensions of the Rectangle.
    if (null != rect)
    {
        rect.Width = 250;
        rect.Height = 150;
    }
}
```

```vb
' Handler for pointer exited event.
Private Sub touchRectangle_PointerExited(sender As Object, e As PointerRoutedEventArgs)
    Dim rect As Rectangle = CType(sender, Rectangle)

    ' Pointer moved outside Rectangle hit test area.
    ' Reset the dimensions of the Rectangle.
    If (rect IsNot Nothing) Then
        rect.Width = 200
        rect.Height = 100
    End If
End Sub

' Handler for pointer released event.
Private Sub touchRectangle_PointerReleased(sender As Object, e As PointerRoutedEventArgs)
    Dim rect As Rectangle = CType(sender, Rectangle)

    ' Reset the dimensions of the Rectangle.
    If (rect IsNot Nothing) Then
        rect.Width = 200
        rect.Height = 100
    End If
End Sub

' Handler for pointer pressed event.
Private Sub touchRectangle_PointerPressed(sender As Object, e As PointerRoutedEventArgs)
    Dim rect As Rectangle = CType(sender, Rectangle)

    ' Change the dimensions of the Rectangle.
    If (rect IsNot Nothing) Then
        rect.Width = 250
        rect.Height = 150
    End If
End Sub
```

## <a name="manipulation-events"></a>Événements de manipulation


Si vous souhaitez que votre application prenne en charge les interactions impliquant plusieurs doigts ou les interactions qui utilisent des données de rapidité, alors dans ce cas utilisez les événements de manipulation.

Vous pouvez utiliser des événements de manipulation pour détecter des interactions telles que les actions de faire glisser, de zoomer et de maintenir.

> [!NOTE]
> Le pavé tactile ne déclenche pas d’événements de manipulation. Au lieu de cela, les événements de pointeur seront déclenchés pour l’entrée du pavé tactile.

Voici une liste des événements de manipulation et de leur argument d’événement associé.

| Événement ou classe                                                                                               | Description                                                                                                                               |
|--------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| [**Événement ManipulationStarting**](/uwp/api/windows.ui.xaml.uielement.manipulationstarting)                                   | Se produit lorsque le processeur de manipulation est créé initialement.                                                                                  |
| [**Événement ManipulationStarted**](/uwp/api/windows.ui.xaml.uielement.manipulationstarted)                                     | Se produit lorsqu’un périphérique d’entrée entame une manipulation sur l’objet [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement).                                            |
| [**Événement ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta)                                         | Se produit lorsque le périphérique d'entrée change de position pendant une manipulation.                                                                      |
| [**Événement ManipulationInertiaStarting**](/uwp/api/windows.ui.xaml.uielement.manipulationinertiastartingevent)                | Se produit lorsque l’appareil d’entrée perd le contact avec l’objet [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) pendant une manipulation et que l’inertie commence. |
| [**Événement ManipulationCompleted**](/uwp/api/windows.ui.xaml.uielement.manipulationcompleted)                                 | Se produit lorsque les opérations de manipulation et d’inertie sont terminées sur l’objet [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement).                                          |
| [**ManipulationStartingRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.ManipulationStartingRoutedEventArgs)               | Fournit des données pour l’événement [**ManipulationStarting**](/uwp/api/windows.ui.xaml.uielement.manipulationstarting).                                         |
| [**ManipulationStartedRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.ManipulationStartedRoutedEventArgs)                 | Fournit des données pour l’événement [**ManipulationStarted**](/uwp/api/windows.ui.xaml.uielement.manipulationstarted).                                           |
| [**ManipulationDeltaRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.ManipulationDeltaRoutedEventArgs)                     | Fournit des données pour l’événement [**ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta) .                                               |
| [**ManipulationInertiaStartingRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.ManipulationInertiaStartingRoutedEventArgs) | Fournit des données pour l’événement [**ManipulationInertiaStarting**](/uwp/api/windows.ui.xaml.uielement.manipulationinertiastarting) .                           |
| [**ManipulationVelocities**](/uwp/api/Windows.UI.Input.ManipulationVelocities)                                              | Décrit la vitesse à laquelle les manipulations se produisent.                                                                                         |
| [**ManipulationCompletedRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.ManipulationCompletedRoutedEventArgs)             | Fournit des données pour l’événement [**ManipulationCompleted**](/uwp/api/windows.ui.xaml.uielement.manipulationcompleted) .                                       |

 

Un mouvement se compose d’une série d’événements de manipulation. Chaque mouvement débute par un événement [**ManipulationStarted**](/uwp/api/windows.ui.xaml.uielement.manipulationstarted), par exemple lorsqu’un utilisateur appuie sur l’écran.

Ensuite, un ou plusieurs événements [**ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta) sont déclenchés. Par exemple, si vous appuyez sur l’écran et faites glisser votre doigt sur celui-ci. Enfin, un événement [**ManipulationCompleted**](/uwp/api/windows.ui.xaml.uielement.manipulationcompleted) est déclenché lorsque l’interaction se termine.

> [!NOTE]
> Si vous n’avez pas de moniteur tactile, vous pouvez tester votre code d’événement de manipulation dans le simulateur à l’aide de la souris et de l’interface de roulette de la souris.

 

L’exemple suivant montre comment utiliser les événements [**ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta) pour gérer une interaction de diapositive sur un [**rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) et la déplacer sur l’écran.

Tout d’abord, un [**rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) nommé `touchRectangle` est créé en XAML avec une [**hauteur**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) et une [**largeur**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) de 200.

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Rectangle Name="touchRectangle"
               Width="200" Height="200" Fill="Blue" 
               ManipulationMode="All"/>
</Grid>
```

Ensuite, un [**TranslateTransform**](/uwp/api/Windows.UI.Xaml.Media.TranslateTransform) global nommé `dragTranslation` est créé pour la traduction du [**rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle). Un détecteur d’événements [**ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta) est spécifié sur l’élément **Rectangle**, et `dragTranslation` est ajouté à la propriété [**RenderTransform**](/uwp/api/windows.ui.xaml.uielement.rendertransform) de **Rectangle**.

```cpp
// Global translation transform used for changing the position of 
// the Rectangle based on input data from the touch contact.
Windows::UI::Xaml::Media::TranslateTransform^ dragTranslation;
```

```cs
// Global translation transform used for changing the position of 
// the Rectangle based on input data from the touch contact.
private TranslateTransform dragTranslation;
```

```vb
' Global translation transform used for changing the position of 
' the Rectangle based on input data from the touch contact.
Private dragTranslation As TranslateTransform
```

```cpp
MainPage::MainPage()
{
    InitializeComponent();

    // Listener for the ManipulationDelta event.
    touchRectangle->ManipulationDelta += 
        ref new ManipulationDeltaEventHandler(
            this, 
            &MainPage::touchRectangle_ManipulationDelta);
    // New translation transform populated in 
    // the ManipulationDelta handler.
    dragTranslation = ref new TranslateTransform();
    // Apply the translation to the Rectangle.
    touchRectangle->RenderTransform = dragTranslation;
}
```

```cs
public MainPage()
{
    this.InitializeComponent();

    // Listener for the ManipulationDelta event.
    touchRectangle.ManipulationDelta += touchRectangle_ManipulationDelta;
    // New translation transform populated in 
    // the ManipulationDelta handler.
    dragTranslation = new TranslateTransform();
    // Apply the translation to the Rectangle.
    touchRectangle.RenderTransform = this.dragTranslation;
}
```

```vb
Public Sub New()

    ' This call is required by the designer.
    InitializeComponent()

    ' Listener for the ManipulationDelta event.
    AddHandler touchRectangle.ManipulationDelta,
        AddressOf testRectangle_ManipulationDelta
    ' New translation transform populated in 
    ' the ManipulationDelta handler.
    dragTranslation = New TranslateTransform()
    ' Apply the translation to the Rectangle.
    touchRectangle.RenderTransform = dragTranslation

End Sub
```

Enfin, dans le gestionnaire d’événements [**ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta), la position de [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) est mise à jour à l’aide de l’objet [**TranslateTransform**](/uwp/api/Windows.UI.Xaml.Media.TranslateTransform) sur la propriété [**Delta**](/uwp/api/windows.ui.xaml.input.manipulationdeltaroutedeventargs.delta).

```cpp
// Handler for the ManipulationDelta event.
// ManipulationDelta data is loaded into the
// translation transform and applied to the Rectangle.
void MainPage::touchRectangle_ManipulationDelta(Object^ sender,
    ManipulationDeltaRoutedEventArgs^ e)
{
    // Move the rectangle.
    dragTranslation->X += e->Delta.Translation.X;
    dragTranslation->Y += e->Delta.Translation.Y;
    
}
```

```cs
// Handler for the ManipulationDelta event.
// ManipulationDelta data is loaded into the
// translation transform and applied to the Rectangle.
void touchRectangle_ManipulationDelta(object sender,
    ManipulationDeltaRoutedEventArgs e)
{
    // Move the rectangle.
    dragTranslation.X += e.Delta.Translation.X;
    dragTranslation.Y += e.Delta.Translation.Y;
}
```

```vb
' Handler for the ManipulationDelta event.
' ManipulationDelta data Is loaded into the
' translation transform And applied to the Rectangle.
Private Sub testRectangle_ManipulationDelta(
    sender As Object,
    e As ManipulationDeltaRoutedEventArgs)

    ' Move the rectangle.
    dragTranslation.X = (dragTranslation.X + e.Delta.Translation.X)
    dragTranslation.Y = (dragTranslation.Y + e.Delta.Translation.Y)

End Sub
```

## <a name="routed-events"></a>Événements routés


Tous les événements de pointeur, événements de mouvement et événements de manipulation mentionnés ici sont implémentés en tant qu’*événements routés*. Cela signifie que l’événement peut éventuellement être géré par des objets autres que celui qui a initialement déclenché l’événement. Les parents successifs dans une arborescence d’objets, tels que les conteneurs parents d’un [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) ou la [**page**](/uwp/api/Windows.UI.Xaml.Controls.Page) racine de votre application, peuvent choisir de gérer ces événements même si ce n’est pas le cas de l’élément d’origine. À l’inverse, tout objet qui gère l’événement peut marquer l’événement géré afin qu’il n’accède plus à aucun élément parent. Pour plus d’informations concernant le concept des événements routés et la façon dont il affecte votre manière d’écrire des gestionnaires pour les événements routés, voir [Vue d’ensemble des événements et des événements routés](/previous-versions/windows/apps/hh758286(v=win.10)).

## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées


-   Concevez des applications en utilisant l’interaction tactile comme méthode d’entrée principale.
-   Fournissez un retour visuel pour les interactions de tous types (entrée tactile, stylo, stylet, souris, etc.)
-   Optimisez le ciblage en ajustant la taille de la cible tactile, la géométrie de contact, ainsi que les mouvements de frottement et de va-et-vient.
-   Optimisez la précision grâce à l’utilisation de points d’ancrage et de « rails » d’orientation.
-   Fournissez des info-bulles et des poignées pour améliorer la précision tactile quand les éléments d’interface sont serrés entre eux.
-   Évitez dans la mesure du possible d’utiliser des interactions chronométrées (exemple d’utilisation appropriée : maintenir appuyé).
-   Évitez d’utiliser le nombre de doigts servant à distinguer la manipulation.

## <a name="related-articles"></a>Articles connexes

- [Gestion des entrées du pointeur](handle-pointer-input.md)
- [Identification des périphériques d’entrée](identify-input-devices.md)

### <a name="samples"></a>Exemples

- [Exemple d’entrée de base](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Exemple d’entrée à faible latence](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Exemple de mode d’interaction utilisateur](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Exemples de visuels de focus](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>Exemples d’archive

- [Entrée : exemple de fonctionnalités de périphériques](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [Entrée : exemple d’événements d’entrée utilisateur XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [Exemple de défilement XAML, panoramique et zoom](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [Entrée : Mouvements et manipulations avec GestureRecognizer](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
