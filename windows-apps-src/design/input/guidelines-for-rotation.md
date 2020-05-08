---
Description: Cette rubrique décrit la nouvelle interface utilisateur Windows pour la rotation et fournit des instructions d’expérience utilisateur qui doivent être prises en compte lors de l’utilisation de ce nouveau mécanisme d’interaction dans votre application Windows.
title: Rotation
ms.assetid: f098bc05-35b3-46b2-9e9b-9ff292d067ca
label: Rotation
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 08d0eb18d59c9a5c19826eb7b6e8d4b65179b6fd
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970104"
---
# <a name="rotation"></a>Rotation


Cet article décrit la nouvelle interface utilisateur Windows pour la rotation et fournit des instructions d’expérience utilisateur qui doivent être prises en compte lors de l’utilisation de ce nouveau mécanisme d’interaction dans votre application Windows.

> **API importantes**: [**Windows. UI. Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [**Windows. UI. Xaml. Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input)

## <a name="dos-and-donts"></a>Choses à faire et à ne pas faire

-   Utilisez la rotation pour permettre aux utilisateurs de faire pivoter directement des éléments d’interface utilisateur.

## <a name="additional-usage-guidance"></a>Indications d’utilisation supplémentaires


**Vue d’ensemble de la rotation**

La rotation est la technique tactile utilisée par les applications Windows pour permettre aux utilisateurs de transformer un objet en sens circulaire (dans le sens horaire ou dans le sens inverse des aiguilles d’une montre).

Selon le périphérique d’entrée utilisé, l’interaction de rotation s’effectue via :

-   une souris ou un stylo/stylet actif pour déplacer la barre de redimensionnement de rotation d’un objet sélectionné ;
-   un stylo/stylet tactile ou passif pour tourner l’objet dans la direction souhaitée à l’aide du mouvement de rotation.

**Quand utiliser la rotation**

Utilisez la rotation pour permettre aux utilisateurs de faire pivoter directement des éléments d’interface utilisateur. Les schémas suivants montrent un certain nombre de positions de doigts prises en charge pour l’interaction de rotation.

![schéma des différentes positions de doigts prises en charge par la rotation.](images/ux-rotate-positions.png)

**Notez**    intuitivement, et dans la plupart des cas, le point de rotation est l’un des deux points tactiles, sauf si l’utilisateur peut spécifier un point de rotation non lié aux points de contact (par exemple, dans une application de dessin ou de disposition). Les images suivantes montrent comment l’expérience utilisateur peut être altérée si le point de rotation n’est pas contraint dans ce sens.

Cette première image montre le premier (le pouce) et le deuxième (l’index) points tactiles : l’index touche un arbre et le pouce touche un rondin de bois.

![image des deux premiers points tactiles du geste de rotation.](images/ux-rotate-points1.png)
Dans ce deuxième dessin, la rotation s’effectue autour du premier point tactile (le pouce). Après la rotation, l’index touche encore le tronc de l’arbre et le pouce touche encore le rondin de bois (le point de rotation).

![image du dessin pivoté avec le point de rotation contraint à l’un des deux premiers points tactiles.](images/ux-rotate-points2.png)
Dans ce troisième dessin, le centre de la rotation a été défini par l’application (ou par l’utilisateur) comme devant être le point central du dessin. Après la rotation, étant donné que le dessin n’a pas pivoté autour de l’un des doigts, l’illusion de manipulation directe est rompue (à moins que cela ne soit la décision de l’utilisateur).

![image du dessin pivoté avec le point de rotation contraint au centre du dessin et non à l’un des deux premiers points tactiles.](images/ux-rotate-points3.png)
Dans ce dernier dessin, le centre de la rotation a été défini par l’application (ou par l’utilisateur) comme devant être un point situé au milieu du bord gauche du dessin. Là encore, à moins que cela ne soit la décision de l’utilisateur, l’illusion de manipulation directe a été rompue.

![image du dessin pivoté avec le point de rotation contraint au centre le plus à gauche du dessin et non à l’un des deux premiers points tactiles.](images/ux-rotate-points4.png)

 

Windows 10 prend en charge trois types de rotation : gratuit, restreint et combiné.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Type</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Rotation libre</td>
<td align="left"><p>La rotation libre permet à un utilisateur de faire pivoter le contenu librement n’importe où dans un arc de 360 degrés. Lorsque l’utilisateur relâche l’objet, l’objet reste à la position choisie. La rotation libre est utile pour les applications de dessin et de mise en page telles que Microsoft PowerPoint, Word, Visio et Paint ; ainsi que Adobe Photoshop, Illustrator et Flash.</p></td>
</tr>
<tr class="even">
<td align="left">Rotation contrainte</td>
<td align="left"><p>La rotation contrainte prend en charge la rotation libre au cours de la manipulation, mais applique des points d’ancrage par incréments de 90 degrés (0, 90, 180 et 270) après le relâchement de l’objet. Lorsque l’utilisateur relâche l’objet, celui-ci pivote automatiquement vers le point d’ancrage le plus proche.</p>
<p>La rotation contrainte est la méthode de rotation la plus courante et elle fonctionne de la même manière que le défilement de contenu. Les points d’ancrage permettent à l’utilisateur d’atteindre son objectif tout en restant imprécis dans ses mouvements. La rotation contrainte est utile pour des applications telles que les navigateurs Web et les albums photo.</p></td>
</tr>
<tr class="odd">
<td align="left">Rotation combinée</td>
<td align="left"><p>La rotation combinée prend en charge la rotation libre avec des zones (semblables aux rails dans les <a href="guidelines-for-panning.md">Recommandations en matière de mouvement panoramique</a>) à chacun des points d’ancrage à 90 degrés appliqués par la rotation contrainte. Si l’utilisateur relâche l’objet en dehors de l’une des zones à 90 degrés, l’objet reste dans cette position ; sinon, il pivote automatiquement vers un point d’ancrage.</p>
<div class="alert">
<strong>Remarque</strong>  un rail d’interface utilisateur est une fonctionnalité dans laquelle une zone autour d’une cible limite le mouvement vers une valeur ou un emplacement spécifique pour influencer sa sélection.
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

## <a name="related-topics"></a>Rubriques connexes

### <a name="samples"></a>Exemples

- [Exemple d’entrée de base](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Exemple d’entrée à faible latence](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Exemple de mode d’interaction utilisateur](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Exemples de visuels de focus](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>Exemples d’archive

- [Entrée : exemple d’événements d’entrée utilisateur XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [Entrée : exemple de fonctionnalités de périphériques](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [Entrée : exemple de test de positionnement tactile](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20desktop%20samples/%5BC%2B%2B%5D-Windows%208%20desktop%20samples/C%2B%2B/Windows%208%20desktop%20samples/Input%20Touch%20hit%20testing%20sample)
- [Exemple de défilement XAML, panoramique et zoom](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [Entrée : exemple d’entrée manuscrite simplifiée](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Simplified%20ink%20sample)
- [Entrée : Mouvements et manipulations avec GestureRecognizer](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
- [Entrée : manipulations et gestes, exemple](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [Exemple d’entrée tactile DirectX](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/DirectX%20touch%20input%20sample%20(Windows%208))
