---
Description: Le mode de sélection permet aux utilisateurs de sélectionner et d’exécuter une action sur un ou plusieurs éléments.
title: Mode de sélection
label: Selection mode
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: anawish
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: d7b781e6074468fbe73446e4057e36ff31266d05
ms.sourcegitcommit: 9625f8fb86ff6473ac2851e600bc02e996993660
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72165096"
---
# <a name="selection-mode-overview"></a>Vue d’ensemble du mode de sélection

Le mode de sélection permet aux utilisateurs de sélectionner et d’exécuter une action sur un ou plusieurs éléments. Il peut être appelé par le biais d’un menu contextuel, à l’aide de la combinaison CTRL+clic ou MAJ+clic sur un élément, ou par la substitution d’une cible sur un élément dans un affichage Galerie. Quand le mode de sélection est actif, des cases à cocher s’affichent à côté de chaque élément de la liste, et des actions peuvent apparaître en haut ou en bas de l’écran.

## <a name="different-selection-modes"></a>Différents modes de sélection
Il existe trois modes de sélection :

- Unique : l’utilisateur ne peut sélectionner qu’un seul élément à la fois.
- Multiple : l’utilisateur peut sélectionner plusieurs éléments sans utiliser de modificateur.
- Étendu : l’utilisateur peut sélectionner plusieurs éléments avec un modificateur, par exemple, en maintenant la touche MAJ enfoncée.

## <a name="selection-mode-examples"></a>Exemples de mode de sélection
### <a name="here-is-a-listview-with-single-selection-mode-enabled"></a>Voici un ListView pour lequel le mode de sélection unique est activé.
![Affichage de liste avec mode de sélection unique](images/listview-selection-single.png)

L’utilisateur a sélectionné l’élément Art Venere et pointe actuellement sur l’élément Mitsue Tollner.

### <a name="here-is-a-listview-with-multiple-selection-mode-enabled"></a>Voici un ListView pour lequel le mode de sélection multiple est activé :
![Affichage de liste avec mode de sélection multiple](images/listview-selection-multiple.png)

L’utilisateur a sélectionné trois éléments et pointe actuellement sur l’élément Donette Foller.

### <a name="here-is-a-gridview-with-single-selection-mode-enabled"></a>Voici un GridView pour lequel le mode de sélection unique est activé :
![Affichage de grille avec mode de sélection unique](images/gridview-selection-single.png)

L’utilisateur a sélectionné l’élément 7 et pointe actuellement sur l’élément 3.

### <a name="here-is-a-gridview-with-multiple-selection-mode-enabled"></a>Voici un GridView pour lequel le mode de sélection multiple est activé :
![Affichage de grille avec mode de sélection multiple](images/gridview-selection-multiple.png)

L’utilisateur a sélectionné les éléments 2, 4 et 5 et pointe actuellement sur l’élément 7.

## <a name="behavior-and-interaction"></a>Comportement et interaction
L’appui sur un emplacement quelconque d’un élément entraîne la sélection de celui-ci. L’appui sur une action de la barre de commandes affecte tous les éléments sélectionnés. Si aucun élément n’est sélectionné, toutes les actions de la barre de commandes doivent être inactives, à l’exception de l’action Sélectionner tout.

Le mode Sélection ne comporte pas de modèle d’abandon interactif ; l’appui sur une zone à l’extérieur du cadre dans lequel le mode Sélection est actif ne permet pas d’annuler ce mode. Ceci empêche toute désactivation accidentelle du mode. En cliquant sur le bouton Précédent, vous fermez le mode de sélection multiple.

Affichez une confirmation visuelle lors de la sélection d’une action. Envisagez d’afficher une boîte de dialogue de confirmation pour certaines actions, notamment pour les opérations destructrices, comme la suppression.

Le mode Sélection est limité à la page dans laquelle il est actif et ne peut pas affecter les éléments situés à l’extérieur de cette page.

Le point d’entrée du mode Sélection doit être juxtaposé au contenu concerné.

Pour des recommandations relatives à la barre de commandes, voir [Recommandations en matière de barres de commandes](app-bars.md).

Pour en savoir plus sur les modes de sélection et la gestion des événements de sélection pour certains contrôles, consultez la page d’aide du contrôle qui vous intéresse.
