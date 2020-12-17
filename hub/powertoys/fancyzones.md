---
title: Utilitaire PowerToys FancyZones pour Windows 10
description: Utilitaire de gestion de fenêtres permettant d’organiser et d’aligner des fenêtres dans des dispositions efficaces
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 00b849e19d3ae8fcf76e1f2a63dc1915353aad30
ms.sourcegitcommit: 5dac88ad541b71ebe85b78951e6b357a3db176cc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/16/2020
ms.locfileid: "97612121"
---
# <a name="fancyzones-utility"></a>Utilitaire FancyZones

FancyZones est un utilitaire de gestion de fenêtres permettant d’organiser et d’aligner des fenêtres dans des dispositions efficaces afin d’améliorer la vitesse de votre flux de travail et de restaurer rapidement les dispositions. FancyZones permet à l’utilisateur de définir un ensemble d’emplacements de fenêtre pour un ordinateur de bureau qui sont des cibles de glissement pour Windows.  Quand l’utilisateur fait glisser une fenêtre dans une zone, la fenêtre est redimensionnée et repositionnée pour remplir cette zone.  

![Capture d’écran FancyZones](../images/pt-fancy-zones2.png)

## <a name="getting-started"></a>Prise en main

### <a name="enable"></a>Activer

Pour commencer à utiliser FancyZones, vous devez activer l’utilitaire dans les paramètres PowerToys, puis appeler l’interface utilisateur de l’éditeur FancyZones.  

### <a name="launch-zones-editor"></a>Éditeur de zones de lancement

Lancez l’éditeur de zones à l’aide du bouton du menu des paramètres PowerToys ou en appuyant sur <kbd>Win</kbd> + <kbd>`</kbd> (Notez que ce raccourci peut être modifié dans la boîte de dialogue Paramètres).  

![Interface utilisateur des paramètres FancyZones](../images/pt-fancyzones-settings1.png) 

### <a name="elevated-permission-admin-apps"></a>Applications d’administration des autorisations élevées

Si vous avez des applications qui sont élevées, exécutez en mode administrateur, lisez [PowerToys et en tant qu’administrateur](administrator.md) pour plus d’informations.

## <a name="choose-your-layout-layout-editor"></a>Choisir votre disposition (éditeur de disposition)

Lors du premier lancement, l’éditeur de zones présente une liste de mises en page qui peuvent être ajustées par le nombre de fenêtres sur le moniteur. Le choix d’une disposition présente un aperçu de cette disposition sur l’analyse. La sélection de appliquer définit la disposition du moniteur.  

![Capture d’écran du sélecteur FancyZones](../images/pt-fancyzones-picker.png)

Si plusieurs affichages sont en cours d’utilisation, l’éditeur détecte les moniteurs disponibles et les affiche pour l’utilisateur. L’analyse choisie est alors la cible de la disposition sélectionnée lorsqu’elle est appliquée.

![Moniteurs multiples du sélecteur FancyZones](../images/pt-fancyzones-multimon.png)

### <a name="space-around-zones"></a>Espace autour des zones

La case à cocher **afficher l’espace autour des zones** vous permet de déterminer le type de bordure ou de marge entourant chaque fenêtre FancyZone. Le champ **espace autour des zones** vous permet de définir une valeur personnalisée pour la largeur de la bordure.

La **distance pour mettre en surbrillance les zones adjacentes** vous permet de définir une valeur personnalisée pour la quantité d’espace entre les fenêtres FancyZone jusqu’à ce qu’elles soient réparties ou avant que les deux soient mises en surbrillance, ce qui leur permet de fusionner.

Avec l’éditeur de zones ouvert, activez et désactivez la case à cocher **afficher l’espace autour des zones** après avoir modifié les valeurs pour afficher la nouvelle valeur appliquée.

![Capture d’écran FancyZones autour des zones](../images/pt-fancyzones-spacearound.png)

### <a name="creating-a-custom-layout"></a>Création d’une disposition personnalisée

L’éditeur de zones prend également en charge la création et l’enregistrement de dispositions personnalisées. Sélectionnez l’onglet **personnalisé** en regard de **modèles** dans le menu supérieur de l’éditeur de zones.
  
Il existe deux façons de créer des dispositions de zone personnalisées : la disposition de fenêtre et la disposition de table. Ils peuvent également être considérés comme des modèles d’addition et de soustraction.  

Le modèle de disposition de fenêtre additive commence par une disposition vierge et prend en charge l’ajout de zones qui peuvent être déplacées et redimensionnées de façon similaire à Windows.

![Mode de l’éditeur de fenêtre FancyZones](../images/pt-fancyzones-windoweditor.png)

Le modèle de disposition de table soustrait commence par une disposition de table et permet de créer des zones en fractionnant et en fusionnant des zones, puis en redimensionnant la reliure entre les zones.

Pour fusionner deux zones, sélectionnez et maintenez le bouton gauche de la souris et faites glisser la souris jusqu’à ce qu’une deuxième zone soit sélectionnée, puis relâchez le bouton et un menu contextuel s’affiche.

![Mode de l’éditeur de table FancyZones](../images/pt-fancyzones-tableeditor.png)

## <a name="snapping-a-window-to-two-or-more-zones"></a>Alignement d’une fenêtre sur au moins deux zones

Si deux zones sont contiguës, une fenêtre peut être alignée sur la somme de leur zone (arrondie au rectangle minimal qui contient les deux). Lorsque le curseur de la souris se trouve près du bord commun de deux zones, les deux zones sont activées simultanément, ce qui vous permet de déposer la fenêtre dans les deux zones.  

Il est également possible d’effectuer un alignement sur un nombre quelconque de zones : tout d’abord, faites glisser la fenêtre jusqu’à ce qu’une zone soit activée, puis appuyez sur la touche et maintenez-la enfoncée `Control` tout en faisant glisser la fenêtre pour sélectionner plusieurs zones.

Pour aligner une fenêtre sur plusieurs zones à l’aide du clavier, commencez par activer les deux options `Override Windows Snap hotkeys (Win+Arrow) to move between zones` et `Move windows based on their position` . Après avoir aligné une fenêtre dans une zone, utilisez <kbd></kbd>  +  le<kbd>contrôle</kbd>Win  +  <kbd>ALT</kbd> + flèches pour développer la fenêtre jusqu’à plusieurs zones.

![Capture d’écran d’activation de deux zones](../images/pt-fancyzones-twozones.png)

## <a name="shortcut-keys"></a>Touches de raccourci

| Raccourci      | Action |
| ----------- | ----------- |
| Win ⊞ + \` | La touche Windows + touche enfoncée (⊞ + \` ) lance l’éditeur (ce raccourci est modifiable dans la boîte de dialogue Paramètres) |
| Win ⊞ + flèche gauche/droite | Déplacer la fenêtre focuse entre les zones (uniquement si `Override Windows Snap hotkeys` le paramètre est activé, dans ce cas, seuls les `Windows ⊞ key + Left Arrow` et `Windows key ⊞ + Right Arrow` sont remplacés, tandis que `Win ⊞ + Up Arrow` et `Win ⊞ + Down Arrow` continuent de fonctionner comme d’habitude) |

FancyZones ne remplace pas Windows 10 `Win ⊞ + Shift + Arrow` pour déplacer rapidement une fenêtre vers une analyse adjacente.

## <a name="settings"></a>Paramètres

| Paramètre | Description |
| --------- | ------------- |
| Configurer le raccourci clavier de l’éditeur de zone | Pour modifier la touche d’insertion automatique par défaut, cliquez sur la zone de texte (il n’est pas nécessaire de sélectionner ou de supprimer le texte), puis appuyez sur le clavier sur la combinaison de touches souhaitée. |
| Maintenir la touche Maj enfoncée pour activer les zones lors de leur déplacement | Bascule entre le mode d’alignement automatique et la touche Maj désactivant l’alignement lors d’un déplacement et le mode d’alignement manuel, où l’utilisation de la touche Maj pendant une opération glisser permet l’alignement |
| Utiliser un bouton de souris non principal pour activer/désactiver l’activation de zone | Lorsque cette option est activée, le fait de cliquer sur un bouton non principal de la souris active ou désactive l’activation des zones |
| Remplacer les touches d’accrochage Windows (Win ⊞ + flèche) pour vous déplacer entre les zones | Lorsque cette option est activée et que FancyZones est en cours d’exécution, elle remplace deux clés Windows Snap : `Win ⊞ + Left Arrow` et `Win ⊞ + Right Arrow` |
| Déplacer des fenêtres en fonction de leur position | Permet d’utiliser les flèches Win ⊞ + haut/bas/droite/gauche pour aligner une fenêtre en fonction de sa position relativement à la disposition de la zone |
| Déplacer des fenêtres entre des zones dans toutes les analyses | Lorsque cette option est désactivée, l’alignement avec Win ⊞ + flèche permet de parcourir la fenêtre à travers les zones de l’analyse actuelle, lorsque est activé, elle parcourt la fenêtre dans toutes les zones de toutes les analyses |
| Conserver les fenêtres dans leurs zones lorsque la résolution de l’écran change | Après une modification de la résolution de l’écran, si ce paramètre est activé, FancyZones redimensionne et repositionne les fenêtres dans les zones où elles se trouvaient précédemment |
| Pendant les modifications de disposition de zone, les fenêtres affectées à une zone correspondent aux nouvelles tailles/positions | Lorsque cette option est activée, FancyZones redimensionne et positionne les fenêtres dans la nouvelle disposition de zone en conservant l’emplacement du numéro de zone précédent de chaque fenêtre |
| Déplacer les fenêtres nouvellement créées vers la dernière zone connue | Déplacer automatiquement une fenêtre récemment ouverte dans l’emplacement de la dernière zone dans laquelle l’application se trouvait |
| Déplacer les fenêtres nouvellement créées vers le moniteur actif actuel [EXPÉRIMENTal] | Lorsque cette option est activée et que l’option « déplacer les fenêtres nouvellement créées vers la dernière zone connue » est désactivée ou que l’application n’a pas de dernière zone connue, elle conserve l’application sur le moniteur actif actuel. |
| Restaurer la taille d’origine des fenêtres en cas d’alignement | Lorsque cette option est activée, le fait de supprimer l’alignement d’une fenêtre restaure sa taille avant d’être alignée |
| Suivre le curseur de la souris au lieu du focus lors du lancement de l’éditeur dans un environnement à plusieurs écrans | Lorsque cette option est activée, le raccourci clavier de l’éditeur lance l’éditeur sur le moniteur où se trouve le curseur de la souris. Lorsque cette option est désactivée, le raccourci clavier de l’éditeur lance l’éditeur sur l’analyse où la fenêtre active actuelle est  |
| Afficher les zones sur toutes les analyses tout en faisant glisser une fenêtre | Par défaut, FancyZones affiche uniquement les zones disponibles sur le moniteur actuel, cette fonctionnalité peut avoir un impact sur les performances quand elle est activée. |
| Autoriser les zones à s’étendre sur plusieurs moniteurs (toutes les analyses doivent avoir la même mise à l’échelle PPP) | Cette option permet de traiter toutes les analyses connectées comme un grand écran. Pour fonctionner correctement, il faut que toutes les analyses aient le même facteur de mise à l’échelle DPI |
| Rendre la fenêtre glissée transparente | Lorsque les zones sont activées, la fenêtre glissée est rendue transparente pour améliorer la visibilité des zones |
| Couleur de surbrillance de zone (#008CFF par défaut) | Couleur d’une zone lorsqu’il s’agit de la cible de déplacement active lors du glissement d’une fenêtre |
| Couleur inactive de la zone (#F5FCFF par défaut) | Couleur d’apparition des zones lorsqu’elles ne sont pas une suppression active pendant le glissement d’une fenêtre |
| Couleur de la bordure de zone (#FFFFFF par défaut) | Couleur de la bordure des zones actives et inactives |
| Opacité des zones (%) (Par défaut 50%) | Pourcentage d’opacité des zones actives et inactives |
| Exclure les applications de l’alignement aux zones | Ajoutez le nom de l’application ou une partie du nom, un par ligne (par exemple, l’ajout `Notepad` correspond `Notepad.exe` `Notepad++.exe` à la fois à et à, pour correspondre uniquement à `Notepad.exe` l’ajout de l' `.exe` extension) |

![Capture d’écran en bas des paramètres FancyZones](../images/pt-fancyzones-settings2.png)
