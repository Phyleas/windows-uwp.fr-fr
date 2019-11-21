---
Description: Cette rubrique décrit la nouvelle interface utilisateur Windows pour la rotation et fournit des instructions d’expérience utilisateur qui doivent être prises en compte lors de l’utilisation de ce nouveau mécanisme d’interaction dans votre application UWP.
title: Rotation
ms.assetid: f098bc05-35b3-46b2-9e9b-9ff292d067ca
label: Rotation
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 71a09d304b0d68bf01012166173360ec6304fb2c
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257926"
---
# <a name="rotation"></a>Rotation


Cet article décrit la nouvelle interface utilisateur Windows pour la rotation et fournit des recommandations en matière d’expérience utilisateur à prendre en compte lors de l’utilisation de ce nouveau mécanisme d’interaction dans votre application UWP.

> **API importantes** : [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [**Windows.UI.Xaml.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input)

## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées

-   Utilisez la rotation pour permettre aux utilisateurs de faire pivoter directement des éléments d’interface utilisateur.

## <a name="additional-usage-guidance"></a>Indications d’utilisation supplémentaires


**Vue d’ensemble de la rotation**

La rotation est une technique optimisée pour l’interaction tactile utilisée par les applications UWP pour permettre aux utilisateurs de faire tourner un objet dans une direction circulaire (dans le sens des aiguilles d’une montre ou contraire).

Selon le périphérique d’entrée utilisé, l’interaction de rotation s’effectue via :

-   une souris ou un stylo/stylet actif pour déplacer la barre de redimensionnement de rotation d’un objet sélectionné ;
-   un stylo/stylet tactile ou passif pour tourner l’objet dans la direction souhaitée à l’aide du mouvement de rotation.

**Quand utiliser la rotation**

Utilisez la rotation pour permettre aux utilisateurs de faire pivoter directement des éléments d’interface utilisateur. Les schémas suivants montrent un certain nombre de positions de doigts prises en charge pour l’interaction de rotation.

![schéma des différentes positions de doigts prises en charge par la rotation.](images/ux-rotate-positions.png)

**Notez**   intuitivement, et dans la plupart des cas, le point de rotation est l’un des deux points tactiles, sauf si l’utilisateur peut spécifier un point de rotation non lié aux points de contact (par exemple, dans une application de dessin ou de disposition). Les images suivantes montrent comment l’expérience utilisateur peut être altérée si le point de rotation n’est pas contraint dans ce sens.

Cette première image montre le premier (le pouce) et le deuxième (l’index) points tactiles : l’index touche un arbre et le pouce touche un rondin de bois.

image ![représentant les deux points tactiles initiaux pour le mouvement de rotation.](images/ux-rotate-points1.png)
Dans ce deuxième dessin, la rotation s’effectue autour du premier point tactile (le pouce). Après la rotation, l’index touche encore le tronc de l’arbre et le pouce touche encore le rondin de bois (le point de rotation).

![image présentant une image pivotée avec le point de rotation restreint à l’un des deux points tactiles initiaux.](images/ux-rotate-points2.png)
Dans ce troisième dessin, le centre de la rotation a été défini par l’application (ou par l’utilisateur) comme devant être le point central du dessin. Après la rotation, étant donné que le dessin n’a pas pivoté autour de l’un des doigts, l’illusion de manipulation directe est rompue (à moins que cela ne soit la décision de l’utilisateur).

![image présentant une image pivotée avec le point de rotation restreint au centre de l’image plutôt qu’à l’un des deux points tactiles initiaux.](images/ux-rotate-points3.png)
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
<td align="left"><p>La rotation libre permet à l’utilisateur de faire pivoter librement du contenu n’importe où dans un arc de 360 degré. Lorsque l’utilisateur relâche l’objet, celui-ci reste dans la position choisie. La rotation libre est utile pour les applications de dessin et de mise en page telles que Microsoft PowerPoint, Word, Visio et Paint ; ainsi que Adobe Photoshop, Illustrator et Flash.</p></td>
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
<strong>Notez</strong>  un rail d’interface utilisateur est une fonctionnalité dans laquelle une zone autour d’une cible limite le mouvement vers une valeur ou un emplacement spécifique pour influencer sa sélection.
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

## <a name="related-topics"></a>Rubriques connexes


**Exemples**
* [Exemple d’entrée de base](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
* [Exemple d’entrée à faible latence](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
* [Exemple de mode d’interaction utilisateur](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
* [Exemples de visuels de focus](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

**Exemples d’archives**
* [Entrée : exemple d’événements d’entrée d’utilisateur XAML](https://code.msdn.microsoft.com/windowsapps/Input-3dff271b)
* [Entrée : exemple de fonctionnalités de l’appareil](https://code.msdn.microsoft.com/windowsapps/Input-device-capabilities-31b67745)
* [Entrée : exemple de test de positionnement tactile](https://code.msdn.microsoft.com/windowsapps/Touch-Hit-Testing-sample-5e35c690)
* [Exemple de défilement XAML, panoramique et zoom](https://code.msdn.microsoft.com/windowsapps/xaml-scrollviewer-pan-and-949d29e9)
* [Entrée : exemple d’encre simplifiée](https://code.msdn.microsoft.com/windowsapps/Input-simplified-ink-sample-11614bbf)
* [Entrée : mouvements et manipulations avec GestureRecognizer](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
* [Entrée : manipulations et mouvements (C++), exemple](https://code.msdn.microsoft.com/windowsapps/Manipulations-and-gestures-362b6b59)
* [Exemple d’entrée tactile DirectX](https://code.msdn.microsoft.com/windowsapps/Simple-Direct3D-Touch-f98db97e)
 

 




