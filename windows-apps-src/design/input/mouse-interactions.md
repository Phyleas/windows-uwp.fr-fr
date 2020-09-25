---
Description: Répondez à l’entrée de souris dans vos applications en gérant les mêmes événements de pointeur de base que ceux utilisés pour l’entrée tactile et du stylo.
title: Interactions avec la souris
ms.assetid: C8A158EF-70A9-4BA2-A270-7D08125700AC
label: Mouse
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 84390e7b929412e4058c1a7e6507ff171344bd53
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91218932"
---
# <a name="mouse-interactions"></a>Interactions avec la souris

Optimisez votre conception d’application Windows pour l’entrée tactile et bénéficiez d’une prise en charge de la souris de base par défaut. 

![souris](images/input-patterns/input-mouse.jpg)

Les entrées de souris conviennent mieux aux interactions utilisateur qui demandent de la précision comme le pointage et le clic. Cette précision inhérente est naturellement prise en charge par l’interface utilisateur de Windows qui permet de gérer la nature imprécise de l’entrée tactile.

Les entrées tactiles et de la souris divergent en raison de la capacité de l’entrée tactile à émuler plus fidèlement la manipulation directe d’éléments d’interface utilisateur par le biais de mouvements physiques effectués directement sur ces objets (comme le balayage, le glissement, la rotation, etc.). Les manipulations avec une souris nécessitent généralement une autre affordance d’interface utilisateur, telle l’utilisation de poignées pour redimensionner ou faire pivoter un objet.

Cette rubrique décrit les considérations relatives à la conception pour les interactions avec la souris.

## <a name="the-uwp-app-mouse-language"></a>Langage de souris d’application UWP

Un ensemble concis d’interactions avec la souris est utilisé de façon uniforme dans l’ensemble du système.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Terme</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>Pointer pour apprendre</p></td>
<td align="left"><p>Pointez sur un élément pour afficher des informations détaillées ou des éléments visuels didactiques (tels qu’une info-bulle) ne requérant aucune action.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Cliquer avec le bouton gauche pour effectuer l’action principale</p></td>
<td align="left"><p>Cliquez avec le bouton gauche sur un élément pour appeler son action principale (par exemple, le lancement d’une application ou l’exécution d’une commande).</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Faire défiler l’affichage pour changer de vue</p></td>
<td align="left"><p>Affichez des barres de défilement pour monter, descendre, aller à gauche et à droite dans une zone de contenu. Les utilisateurs peuvent faire défiler l’affichage en cliquant sur les barres de défilement ou en actionnant la roulette de la souris. Les barres de défilement peuvent indiquer l’emplacement de la vue actuelle dans la zone de contenu (un mouvement panoramique avec interaction tactile affiche une interface utilisateur similaire).</p></td>
</tr>
<tr class="even">
<td align="left"><p>Cliquer avec le bouton droit pour sélectionner une commande</p></td>
<td align="left"><p>Cliquez avec le bouton droit sur la barre de navigation (si elle est disponible) et la barre de l’application avec des commandes globales. Cliquez avec le bouton droit sur un élément pour le sélectionner et afficher la barre de l’application contenant des commandes contextuelles pour l’élément sélectionné.</p>
<div class="alert">
<strong>Remarque</strong>    Cliquez avec le bouton droit pour afficher un menu contextuel si les commandes de sélection ou de barre d’application ne sont pas des comportements d’interface utilisateur appropriés. Toutefois, nous vous recommandons vivement d’utiliser la barre de l’application pour tous les comportements des commandes.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>Commandes d’interface utilisateur pour le zoom</p></td>
<td align="left"><p>Affichez des commandes d’interface utilisateur dans la barre de l’application (telles que + et -) ou appuyez sur Ctrl et actionnez la roulette de la souris pour émuler des mouvements de pincement et d’étirement pour le zoom.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Commandes d’interface utilisateur pour la rotation</p></td>
<td align="left"><p>Affichez des commandes d’interface utilisateur dans la barre de l’application ou appuyez sur Ctrl+Maj et actionnez la roulette de la souris pour émuler un mouvement de rotation. Faites pivoter l’appareil lui-même pour faire pivoter l’écran tout entier.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Cliquer avec le bouton gauche et faire glisser pour réorganiser</p></td>
<td align="left"><p>Cliquez avec le bouton gauche sur un élément et faites-le glisser pour le déplacer.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Cliquer avec le bouton gauche et faire glisser pour sélectionner du texte</p></td>
<td align="left"><p>Cliquez avec le bouton gauche dans du texte sélectionnable et faites glisser le curseur pour sélectionner du texte. Double-cliquez pour sélectionner un mot.</p></td>
</tr>
</tbody>
</table>

## <a name="mouse-input-events"></a>Événements d’entrée de la souris

La plupart des entrées de la souris peuvent être gérées par le biais des événements d’entrée routés communs pris en charge par tous les objets [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) . Elles incluent notamment :

- [**BringIntoViewRequested**](/uwp/api/windows.ui.xaml.uielement.bringintoviewrequested)
- [**CharacterReceived**](/uwp/api/windows.ui.xaml.uielement.characterreceived)
- [**ContextCanceled**](/uwp/api/windows.ui.xaml.uielement.contextcanceled)
- [**ContextRequested**](/uwp/api/windows.ui.xaml.uielement.contextrequested)
- [**DoubleTapped**](/uwp/api/windows.ui.xaml.uielement.doubletapped)
- [**DragEnter**](/uwp/api/windows.ui.xaml.uielement.dragenter)
- [**DragLeave**](/uwp/api/windows.ui.xaml.uielement.dragleave)
- [**DragOver**](/uwp/api/windows.ui.xaml.uielement.dragover)
- [**DragStarting**](/uwp/api/windows.ui.xaml.uielement.dragstarting)
- [**Déplacez**](/uwp/api/windows.ui.xaml.uielement.drop)
- [**DropCompleted**](/uwp/api/windows.ui.xaml.uielement.dropcompleted)
- [**GettingFocus**](/uwp/api/windows.ui.xaml.uielement.gettingfocus)
- [**GotFocus**](/uwp/api/windows.ui.xaml.uielement.gotfocus)
- [**Holding**](/uwp/api/windows.ui.xaml.uielement.holding)
- [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown)
- [**Événementiel**](/uwp/api/windows.ui.xaml.uielement.keyup)
- [**LosingFocus**](/uwp/api/windows.ui.xaml.uielement.losingfocus)
- [**LostFocus**](/uwp/api/windows.ui.xaml.uielement.lostfocus)
- [**ManipulationCompleted**](/uwp/api/windows.ui.xaml.uielement.manipulationcompleted)
- [**ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta)
- [**ManipulationInertiaStarting**](/uwp/api/windows.ui.xaml.uielement.manipulationinertiastarting)
- [**ManipulationStarted**](/uwp/api/windows.ui.xaml.uielement.manipulationstarted)
- [**ManipulationStarting**](/uwp/api/windows.ui.xaml.uielement.manipulationstarting)
- [**NoFocusCandidateFound**](/uwp/api/windows.ui.xaml.uielement.nofocuscandidatefound)
- [**PointerCanceled**](/uwp/api/windows.ui.xaml.uielement.pointercanceled)
- [**PointerCaptureLost**](/uwp/api/windows.ui.xaml.uielement.pointercapturelost)
- [**PointerEntered**](/uwp/api/windows.ui.xaml.uielement.pointerentered)
- [**PointerExited**](/uwp/api/windows.ui.xaml.uielement.pointerexited)
- [**PointerMoved**](/uwp/api/windows.ui.xaml.uielement.pointermoved)
- [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed)
- [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased)
- [**PointerWheelChanged**](/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)
- [**PreviewKeyDown**](/uwp/api/windows.ui.xaml.uielement.previewkeydown)
- [**PreviewKeyUp**](/uwp/api/windows.ui.xaml.uielement.previewkeyup)
- [**RightTapped**](/uwp/api/windows.ui.xaml.uielement.righttapped)
- [**Tapped**](/uwp/api/windows.ui.xaml.uielement.tapped)

Toutefois, vous pouvez tirer parti des fonctionnalités spécifiques de chaque appareil (telles que les événements de roulette de la souris) à l’aide des événements pointeur, geste et manipulation dans [Windows. UI. Input](/uwp/api/windows.ui.input).

**Exemples :** Consultez notre [exemple BasicInput](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)pour.

## <a name="guidelines-for-visual-feedback"></a>Recommandations en matière de retour visuel

- Quand des événements de déplacement ou de pointage permettent de détecter une souris, affichez une interface utilisateur propre à la souris pour indiquer les fonctionnalités exposées par l’élément. Si la souris ne bouge pas pendant un certain temps ou si l’utilisateur commence une interaction tactile, estompez progressivement l’interface utilisateur de la souris. Cela maintient l’interface utilisateur propre et aérée.
- N’utilisez pas le curseur pour le retour de pointage, car le retour fourni par l’élément est suffisant (voir la section Curseurs, ci-dessous).
- N’affichez pas de retour visuel si un élément ne prend pas en charge l’interaction (tel que le texte statique).
- N’utilisez pas de rectangles de sélection avec les interactions avec la souris. Réservez ceux-ci aux interactions avec le clavier.
- Affichez un retour visuel simultanément pour tous les éléments qui représentent la même cible d’entrée.
- Fournissez des boutons (tels que + et -) pour émuler des manipulations tactiles, telles que le mouvement panoramique, la rotation, le zoom, etc.

Pour obtenir des recommandations plus générales sur le retour visuel, voir [Recommandations en matière de retour visuel](guidelines-for-visualfeedback.md).

## <a name="cursors"></a>Curseurs

Un ensemble de curseurs standard est disponible pour servir de pointeurs de souris. Ces derniers sont utilisés pour indiquer l’action principale d’un élément.

Chaque curseur standard possède une image par défaut correspondante qui lui est associée. L’utilisateur ou une application peut remplacer à tout moment l’image par défaut associée à n’importe quel curseur standard. Spécifiez une image de curseur à l’aide de la fonction [**PointerCursor**](/uwp/api/windows.ui.core.corewindow.pointercursor) .

Si vous avez besoin de personnaliser le curseur de la souris :

- Utilisez toujours le curseur en forme de flèche (![Curseur en forme de flèche](images/cursor-arrow.png)) pour les éléments interactifs. N’utilisez pas le curseur en forme de main (![Curseur en forme de main](images/cursor-pointinghand.png)) pour les liens ou pour d’autres éléments interactifs. À la place, utilisez les effets de pointage (décrits précédemment).
- Utilisez le curseur texte (![Curseur texte](images/cursor-text.png)) pour le texte sélectionnable.
- Utilisez le curseur de déplacement (![Curseur de déplacement](images/cursor-move.png)) lorsque l’action principale correspond à un déplacement (par exemple, un glisser-déplacer ou un rognage). N’utilisez pas le curseur de déplacement pour des éléments lorsque l’action principale correspond à une navigation (tels que les vignettes de l’écran de démarrage).
- Utilisez les curseurs de redimensionnement horizontal, vertical et diagonal (![Curseur de redimensionnement vertical](images/cursor-vertical.png), ![Curseur de redimensionnement horizontal](images/cursor-horizontal.png), ![Curseur de redimensionnement diagonal (du coin inférieur gauche au coin supérieur droit)](images/cursor-diagonal2.png), ![Curseur de redimensionnement diagonal (du coin supérieur gauche au coin inférieur droit)](images/cursor-diagonal1.png)) lorsqu’un objet est redimensionnable.
- Utilisez les curseurs en forme de main de saisie (![Curseur en forme de main de saisie (ouverte)](images/cursor-pan1.png), ![Curseur en forme de main de saisie (fermée)](images/cursor-pan2.png)) lors d’un mouvement panoramique de contenu au sein d’une zone de dessin fixe (telle qu’une carte).

## <a name="related-articles"></a>Articles connexes

- [Gestion des entrées du pointeur](handle-pointer-input.md)
- [Identification des périphériques d’entrée](identify-input-devices.md)
- [Vue d’ensemble des événements et des événements routés](../../xaml-platform/events-and-routed-events-overview.md)

### <a name="samples"></a>Exemples

- [Exemple d’entrée de base](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Exemple d’entrée à faible latence](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Exemple de mode d’interaction utilisateur](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Exemples de visuels de focus](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)
