---
Description: Le mouvement panoramique ou défilement permet à l’utilisateur de naviguer au sein d’une vue unique pour afficher le contenu de la vue qui ne tient pas entièrement dans la fenêtre d’affichage. Parmi les exemples de vues figurent la structure de dossiers d’un ordinateur, une bibliothèque de documents ou un album photo.
title: Mouvement panoramique
ms.assetid: b419f538-c7fb-4e7c-9547-5fb2494c0b71
label: Panning
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e77485bd2febe2ee032f9a0c3ceada31c2807ee0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172493"
---
# <a name="guidelines-for-panning"></a>Recommandations en matière de mouvement panoramique


Le mouvement panoramique ou défilement permet à l’utilisateur de naviguer au sein d’une vue unique pour afficher le contenu de la vue qui ne tient pas entièrement dans la fenêtre d’affichage. Parmi les exemples de vues figurent la structure de dossiers d’un ordinateur, une bibliothèque de documents ou un album photo.

> **API importantes**: [**Windows. UI. Input**](/uwp/api/Windows.UI.Input), [**Windows. UI. Xaml. Input**](/uwp/api/Windows.UI.Xaml.Input)


## <a name="dos-and-donts"></a>Choses à faire et à ne pas faire


**Indicateurs de mouvement panoramique et barres de défilement**

-   Vérifiez que le mouvement panoramique/défilement est possible avant de charger du contenu dans votre application.

-   Affichez les indicateurs panoramiques et les barres de défilement pour fournir des indications sur l’emplacement et la taille. Masquez-les si vous fournissez une fonctionnalité de navigation personnalisée.

    **Remarque**    Contrairement aux barres de défilement standard, les indicateurs panoramiques sont purement informatifs. Ils ne sont pas exposés aux périphériques d’entrée et ne peuvent pas être manipulés de quelque manière que ce soit.

     

**Mouvement panoramique sur un seul axe (dépassement unidimensionnel)**

-   Utilisez le mouvement panoramique sur un axe pour les régions de contenu qui s’étendent au-delà des contours de la fenêtre d’affichage (vertical ou horizontal).

    -   Mouvement panoramique vertical pour afficher une liste d’éléments unidimensionnelle.
    -   Mouvement panoramique horizontal pour afficher une grille d’éléments.
-   Si l’utilisateur doit pouvoir faire défiler et s’arrêter entre des points d’ancrage, n’utilisez pas de points d’ancrage obligatoires avec le mouvement panoramique sur un seul axe. Les points d’ancrage obligatoires permettent de garantir que l’utilisateur sera arrêté à ces points. Privilégiez, dans ce cas, les points d’ancrage de proximité.

**Mouvement panoramique libre (dépassement en deux dimensions)**

-   Utilisez le mouvement panoramique sur deux axes pour les régions de contenu qui s’étendent au-delà des contours de la fenêtre d’affichage (à la fois vertical et horizontal).

    -   Annulez le comportement de rails par défaut et utilisez le mouvement panoramique libre pour afficher du contenu non structuré dans lequel l’utilisateur sera probablement amené à se déplacer dans plusieurs directions.
-   Le mouvement panoramique libre est tout à fait adapté à la navigation entre les images ou les cartes.

**Vue paginée**

-   Utilisez des points d’ancrage obligatoires lorsque le contenu est composé d’éléments distincts ou si vous souhaitez afficher un élément dans son intégralité. Par exemple, les pages d’un livre ou d’un magazine, une colonne d’articles ou des images individuelles.

    -   Placez un point d’ancrage à chaque limite logique.
    -   Dimensionnez ou mettez à l’échelle chaque élément pour qu’il tienne dans la vue.

**Points logiques et points clés**

-   Utilisez des points d’ancrage de proximité si le contenu comporte des points clés ou des points logiques auxquels l’utilisateur sera susceptible de s’arrêter. Par exemple, un en-tête de section.

-   Si des limites ou des contraintes de tailles maximale et minimale sont définies, utilisez le retour visuel pour indiquer quand l’utilisateur atteint ou dépasse ces limites.

**Chaînage de contenu incorporé ou imbriqué**

-   Utilisez le mouvement panoramique sur un seul axe (généralement horizontal) et les dispositions en colonne pour le texte et le contenu basé sur la grille. Dans ce cas, le contenu est généralement encapsulé et transmis naturellement d’une colonne à l’autre, ce qui garantit la cohérence et la découverte de l’expérience utilisateur dans les applications Windows.

-   N’utilisez pas des régions de mouvement panoramique incorporées pour afficher du texte ou des listes d’éléments. Il ne s’agit pas d’une expérience utilisateur intuitive ou détectable car les indicateurs de mouvement panoramique et les barres de défilement sont uniquement affichés lorsque le contact d’entrée est détecté au sein de la région.

-   Ne chaînez ni ne placez une région de mouvement panoramique dans une autre région de mouvement panoramique si elles défilent toutes les deux dans la même direction, comme illustré ici. Dans ce cas, dès lors qu’une limite de la zone enfant est atteinte, cela risque d’entraîner le défilement involontaire de la zone parente. Pensez à rendre l’axe de mouvement panoramique perpendiculaire.

    ![image d’une zone de mouvement panoramique qui défile dans la même direction que son conteneur.](images/scrolling-embedded3.png)

## <a name="additional-usage-guidance"></a>Indications d’utilisation supplémentaires

Le mouvement panoramique tactile, par un mouvement de glissement ou de balayage avec un ou plusieurs doigts, ressemble à un défilement à l’aide de la souris. L’interaction de type panoramique ressemble plus à la rotation de la roulette de la souris ou au glissement de la case de défilement qu’à un clic sur la barre de défilement. À moins qu’une distinction ne soit établie dans une API ou nécessaire pour une interface utilisateur propre à un appareil Windows, nous utilisons le mouvement panoramique pour faire référence aux deux interactions.

> <div id="main">
> <strong>Mise à jour des créateurs de automne Windows 10-modification du comportement</strong> Par défaut, au lieu de sélectionner du texte, un stylet actif défile à présent/panoramiques dans les applications Windows (par exemple, Touch, pavé tactile et stylet passif).  
> Si votre application repose sur le comportement précédent, vous pouvez remplacer le défilement du stylet et rétablir le comportement précédent. Pour plus d’informations, consultez la rubrique de référence de l’API pour la <a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer">classe ScrollViewer</a>.
> </div>

Selon le périphérique d’entrée utilisé, l’utilisateur effectue un mouvement panoramique au sein d’une région de mouvement panoramique à l’aide de l’un des éléments suivants :

-   Une souris, un pavé tactile ou un stylo/stylet actif pour cliquer sur les flèches de défilement, faire glisser la case de défilement ou cliquer dans la barre de défilement.
-   La roulette de la souris pour émuler le glissement de la case de défilement.
-   Les boutons étendus (XBUTTON1 et XBUTTON2), s’ils sont pris en charge par la souris.
-   Les touches de direction du clavier pour émuler le glissement de la case de défilement ou les touches de page pour émuler le clic dans la barre de défilement.
-   Une entrée tactile, un pavé tactile ou un stylo/stylet passif pour faire glisser ou balayer les doigts dans la direction souhaitée.

Le glissement implique un déplacement lent des doigts dans la direction du mouvement panoramique. Cela crée une relation directe dans laquelle le contenu défile à la même vitesse et à la même distance que les doigts. Le balayage, qui implique un glissement rapide suivi d’une levée des doigts, entraîne l’application des mouvements physiques suivants sur l’animation de mouvement panoramique :

-   Décélération (inertie) : la levée des doigts génère le démarrage de la décélération du mouvement panoramique. Cette action s’apparente à glisser sur une surface glissante jusqu’à l’arrêt.
-   Absorption : l’impulsion du mouvement panoramique au cours de la décélération provoque un léger effet de rebond au contact d’un point d’ancrage ou d’une limite de la zone de contenu.

**Types de défilements panoramiques**

Windows 8 prend en charge trois types de défilements panoramiques :

-   Un seul axe : le mouvement panoramique est pris en charge dans une seule direction (horizontale ou verticale).
-   Rails : le mouvement panoramique est pris en charge dans toutes les directions. En revanche, dès lors que l’utilisateur croise un seuil de distance dans une direction spécifique, le défilement est limité à cet axe.
-   Libre : le mouvement panoramique est pris en charge dans toutes les directions.

**Interface utilisateur de mouvement panoramique**

L’expérience d’interaction pour le mouvement panoramique est propre au périphérique d’entrée tout en étant similaire en termes de fonctionnalité.

**Régions Pannable** Les comportements de région Pannable sont exposés à l’application Windows à l’aide des développeurs JavaScript au moment de la conception via feuilles de style en cascade (CSS).

Il existe deux modes d’affichage du mouvement panoramique qui dépendent du périphérique d’entrée détecté :

-   Des indicateurs de mouvement panoramique pour l’interaction tactile.
-   Des barres de défilement pour d’autres périphériques d’entrée comme la souris, le pavé tactile, le clavier et le stylet.

**Remarque**    Les indicateurs panoramiques sont visibles uniquement lorsque le contact tactile est dans la région pannable. De même, la barre de défilement est seulement visible quand le curseur de la souris, le curseur du stylo/stylet ou le focus du clavier se trouve dans la région de défilement.

 

**Indicateurs de mouvement panoramique** Les indicateurs de mouvement panoramique sont semblables à la case de défilement d’une barre de défilement. Ils indiquent la proportion de contenu affichée par rapport à l’intégralité de la zone de mouvement panoramique et la position relative du contenu affiché dans cette zone.

Le schéma suivant montre deux zones de mouvement panoramique de différentes longueurs et leurs indicateurs.

![images de deux zones de mouvement panoramique de différentes longueurs avec leurs indicateurs.](images/scrolling-indicators.png)

**Comportements** 
 panoramiques **Points d’alignement** Le panoramique avec le geste de balayage présente le comportement d’inertie dans l’interaction lorsque le contact tactile est levé. Avec l’inertie, le contenu continue de défiler jusqu’à ce qu’un seuil de distance soit atteint sans entrée directe de l’utilisateur. Utilisez des points d’ancrage pour modifier ce comportement inertiel.

Les points d’ancrage spécifient des arrêts logiques dans le contenu de votre application. Sur le plan cognitif, les points d’ancrage font office de mécanisme de pagination pour l’utilisateur et réduisent la fatigue résultant de l’exécution excessive de mouvements de glissement ou de balayage dans des régions de mouvement panoramiques importantes. Ils vous permettent de gérer une entrée utilisateur imprécise et garantissent l’affichage d’un sous-ensemble spécifique de contenu ou d’informations clés dans la fenêtre d’affichage.

Il existe deux types de points d’ancrage :

-   De proximité : lorsque l’utilisateur met fin au contact, un point d’ancrage est sélectionné si l’inertie s’arrête au sein du seuil de distance du point d’ancrage. Le mouvement panoramique peut aussi s’arrêter entre deux points d’ancrage de proximité.
-   Obligatoire : le point d’ancrage sélectionné est celui qui précède ou qui suit immédiatement le dernier point d’ancrage rencontré avant que l’utilisateur ait mis fin au contact (en fonction de la direction et de la vitesse du mouvement). Le mouvement panoramique doit prendre fin sur un point d’ancrage obligatoire.

Les points d’ancrage de mouvement panoramique sont utiles pour des applications comme des navigateurs Web et des albums photo qui émulent du contenu paginé ou qui contiennent des groupes logiques d’éléments pouvant être regroupés de manière dynamique afin de tenir dans une fenêtre d’affichage.

Le schéma suivant montre comment l’action qui consiste à effectuer un mouvement panoramique jusqu’à un certain point, puis à relâcher le mouvement entraîne le contenu à défiler en panoramique jusqu’à un emplacement logique.

|                                                                |                                                                                         |                                                                                                                 |
|----------------------------------------------------------------|-----------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| ![image d’une zone de mouvement panoramique.](images/ux-panning-snap1.png) | ![image d’une zone de mouvement panoramique en cours de défilement vers la gauche.](images/ux-panning-snap2.png) | ![image d’une zone de mouvement panoramique qui a arrêté le défilement à un point d’ancrage logique.](images/ux-panning-snap3.png) |
| Balayez pour faire défiler.                                                  | Relâchez le contact.                                                                     | La région de mouvement panoramique prend fin au point d’ancrage, et non à l’endroit où le contact a été relâché.                                |

 

**Rails** Le contenu peut être plus large et plus grand que les dimensions et la résolution d’un périphérique d’affichage. C’est pourquoi le mouvement panoramique en deux dimensions (horizontale et verticale) est souvent nécessaire. Des rails améliorent l’expérience utilisateur dans ces cas précis en mettant l’accent sur le mouvement panoramique le long de l’axe de mouvement (vertical ou horizontal).

Le schéma suivant explique le concept des rails.

![diagramme d’un écran doté de rails qui restreignent le mouvement panoramique](images/ux-panning-rails.png)

**Chaînage de contenu incorporé ou imbriqué**

Une fois qu’un utilisateur atteint une limite de zoom ou de défilement sur un élément qui a été imbriqué dans un autre élément avec zoom ou défilement, vous pouvez spécifier si cet élément parent doit continuer l’opération de zoom ou de défilement commencée dans son élément enfant. Cela s’appelle le chaînage de zoom ou de défilement.

Le chaînage est utilisé pour le mouvement panoramique au sein d’une zone de contenu sur un seul axe contenant une ou plusieurs régions de mouvement panoramique sur un seul axe ou libre (lorsque le contact se trouve dans l’une de ces régions enfants). Dès lors que la limite de mouvement panoramique de la région enfant est atteinte dans une direction spécifique, le mouvement panoramique est activé sur la région parente dans la même direction.

Quand une région de mouvement panoramique est imbriquée dans une autre région de mouvement panoramique, il est important de spécifier suffisamment d’espace entre le conteneur et la région incorporée. Dans les schémas suivants, une région de mouvement panoramique est placée à l’intérieur d’une autre région de mouvement panoramique, chacune d’elle orientée perpendiculairement à l’autre. Chaque région offre à l’utilisateur une grande surface de mouvement panoramique.

![image d’une zone de mouvement panoramique incorporée.](images/scrolling-embedded.png)

Si la surface est insuffisante, comme dans le schéma suivant, la région de mouvement panoramique risque d’interférer avec celle du conteneur, provoquant ainsi un défilement involontaire dans une ou plusieurs régions.

![image d’un espacement insuffisant pour une zone de mouvement panoramique incorporée.](images/ux-panning-embedded-wrong.png)

Ces recommandations s’avèrent utiles pour des applications telles que les albums photo ou les applications de mappage qui prennent en charge en même temps le mouvement panoramique libre dans chaque image et le mouvement panoramique sur un seul axe dans l’album (vers les images précédentes ou suivantes) ou la zone de détails. Dans les applications qui fournissent une zone de détails ou d’options correspondant à une image de défilement libre ou carte, nous recommandons que la disposition de la page commence avec la même zone de détails et d’options. En effet, la zone de défilement libre de l’image ou carte peut interférer avec le mouvement panoramique vers la zone de détails.

## <a name="related-articles"></a>Articles connexes

- [Interactions utilisateur personnalisées](../layout/index.md)
- [Optimiser les contrôles ListView et GridView](../../debug-test-perf/optimize-gridview-and-listview.md)
- [Accessibilité du clavier](../accessibility/keyboard-accessibility.md)

**Exemples**
- [Exemple d’entrée de base](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Exemple d’entrée à faible latence](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Exemple de mode d’interaction utilisateur](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Exemples de visuels de focus](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

**Exemples d’archives**
- [Entrée : exemple d’événements d’entrée utilisateur XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [Entrée : exemple de fonctionnalités de périphériques](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [Entrée : exemple de test de positionnement tactile](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20desktop%20samples/%5BC%2B%2B%5D-Windows%208%20desktop%20samples/C%2B%2B/Windows%208%20desktop%20samples/Input%20Touch%20hit%20testing%20sample)
- [Exemple de défilement XAML, panoramique et zoom](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [Entrée : exemple d’entrée manuscrite simplifiée](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Simplified%20ink%20sample)
- [Entrée : exemple de mouvements Windows 8](/samples/browse/?redirectedfrom=MSDN-samples)
- [Entrée : manipulations et gestes, exemple](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [Exemple d’entrée tactile DirectX](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/DirectX%20touch%20input%20sample%20(Windows%208))