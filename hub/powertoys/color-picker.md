---
title: Utilitaire du sélecteur de couleurs PowerToys pour Windows 10
description: Utilitaire de sélection de couleurs à l’ensemble du système pour Windows 10 qui vous permet de choisir des couleurs de toute application en cours d’exécution et de copier automatiquement les valeurs HEX ou RVB dans le presse-papiers.
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3c76753d455d28dd44045edf9482b3de9ccd97b8
ms.sourcegitcommit: 46a7e9db64e17a645ee6e888f62a9b04632c56af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97618568"
---
# <a name="color-picker-utility"></a>Utilitaire sélecteur de couleurs

Utilitaire de sélection de couleurs à l’ensemble du système pour Windows 10, qui vous permet de choisir des couleurs de toute application en cours d’exécution et de la copier automatiquement dans un format configurable dans le presse-papiers.

![ColorPicker](../images/pt-colorpicker-hex-editor.png)

## <a name="getting-started"></a>Prise en main

### <a name="enable"></a>Activer

Pour commencer à utiliser le sélecteur de couleurs, vous devez d’abord vous assurer qu’il est activé dans les paramètres PowerToys (section sélecteur de couleurs).

### <a name="activate"></a>Activer

Une fois activé, vous pouvez choisir l’un des trois comportements suivants à exécuter lors du lancement du sélecteur de couleurs avec le raccourci d’activation <kbd>Win</kbd> + <kbd>Shift</kbd> + <kbd>C</kbd> (Notez que ce raccourci peut être modifié dans la boîte de dialogue Paramètres) :

:::image type="content" source="../images/pt-colorpicker-behaviors.png" alt-text="Comportements du composant ColorPicker.":::

- **Sélecteur de couleurs avec le mode éditeur activé** : ouvre le sélecteur de couleurs. après avoir sélectionné une couleur, l’éditeur est ouvert et la couleur sélectionnée est copiée dans le presse-papiers (dans le format par défaut, configurable dans la boîte de dialogue Paramètres).
- **Éditeur** : ouvre l’éditeur directement, à partir de là, vous pouvez choisir une couleur dans l’historique, ajuster une couleur sélectionnée ou capturer une nouvelle couleur en ouvrant le sélecteur de couleurs.
- **Sélecteur de couleurs uniquement** : ouvre uniquement le sélecteur de couleurs et la couleur sélectionnée est copiée dans le presse-papiers.

### <a name="select-color"></a>Sélectionner une couleur

Une fois le sélecteur de couleurs activé, placez le curseur de la souris sur la couleur que vous souhaitez copier et cliquez avec le bouton gauche de la souris pour sélectionner une couleur. Si vous souhaitez afficher la zone autour de votre curseur plus en détail, faites défiler vers le haut pour effectuer un zoom avant.

La couleur copiée est stockée dans le presse-papiers au format configuré dans les paramètres (HEX par défaut).

![Sélection d’une couleur](../images/pt-colorpicker.gif)

## <a name="editor-usage"></a>Utilisation de l’éditeur

L’éditeur vous permet de voir l’historique des couleurs choisies (jusqu’à 20) et de copier leur représentation dans n’importe quel format de chaîne prédéfini. Vous pouvez configurer les formats de couleur visibles dans l’éditeur, ainsi que l’ordre dans lequel ils apparaissent. Cette configuration se trouve dans les paramètres PowerToys.

L’éditeur vous permet également de régler les couleurs choisies ou d’obtenir une nouvelle couleur similaire. L’éditeur affiche un aperçu des différentes nuances de la couleur-2 plus claire et des 2 plus sombres actuellement sélectionnées.

En cliquant sur l’une de ces autres nuances, vous ajouterez la sélection à l’historique des couleurs choisies (apparaît en haut de la liste historique des couleurs). La couleur du milieu représente la couleur actuellement sélectionnée dans l’historique des couleurs. En cliquant dessus, le contrôle de configuration de réglage précis s’affiche, qui vous permet de modifier la TEINTe ou les valeurs RVB de la couleur actuelle. En appuyant sur OK, vous ajoutez la couleur nouvellement configurée dans l’historique des couleurs.

![Éditeur ColorPicker](../images/pt-colorpicker-editor.gif)

Pour supprimer une couleur de l’historique des couleurs, cliquez avec le bouton droit sur la couleur souhaitée, puis sélectionnez *supprimer*.

## <a name="settings"></a>Paramètres

Le sélecteur de couleurs vous permet de modifier les paramètres suivants :

- Raccourci d’activation
- Comportement du raccourci d’activation
- Format d’une couleur copiée (HEX, RVB, etc.)
- Ordre et apparence des formats de couleur dans l’éditeur

![Capture d’écran des paramètres ColorPicker](../images/pt-colorpicker-settings.png)

## <a name="limitations"></a>Limites

- Le sélecteur de couleurs ne peut pas être affiché en haut du menu Démarrer ou du centre de maintenance (vous pouvez toujours choisir une couleur).
- Si l’application actuellement axée sur le système a été démarrée avec une élévation d’administrateur (exécuter en tant qu’administrateur), le raccourci d’activation du sélecteur de couleurs ne fonctionne pas, sauf si PowerToys a également été démarré avec une élévation d’administrateur.
