---
title: Microsoft PowerToys
description: Microsoft PowerToys est un ensemble d’utilitaires destinés à personnaliser Windows 10. Parmi ces utilitaires figurent ColorPicker (capture d’une valeur de couleur en un clic), FancyZones (raccourcis permettant de positionner les fenêtres dans une disposition de grille), les extensions de l’Explorateur de fichiers (aperçu des SVG ou des fichiers Markdown), Image Resizer (redimensionnement d’une ou plusieurs images d’un simple clic droit), Keyboard Manager (remappage des touches ou création de raccourcis personnalisés), PowerRename (renommage en bloc avec les fonctions recherche et remplacement), PowerToys Run (lancement des applications via Alt + Espace), Shortcut Guide et ceux à venir.
ms.date: 12/02/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 5e7e88e8ff179ebbb63aa7369c22149b645c9838
ms.sourcegitcommit: afc4ff2c89f148d32073ab1cc42063ccdc573a8c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104580"
---
# <a name="microsoft-powertoys-utilities-to-customize-windows-10"></a>Microsoft PowerToys : Utilitaires de personnalisation de Windows 10

Microsoft PowerToys est un ensemble d’utilitaires qui permet aux utilisateurs chevronnés de régler et de simplifier leur expérience Windows 10 pour une productivité accrue.

> [!div class="nextstepaction"]
> [Installer les PowerToys](install.md)

## <a name="processor-support"></a>Prise en charge des processeurs

- **x64** : Prise en charge
- **x86** : En cours de développement (voir [problème #602](https://github.com/microsoft/PowerToys/issues/602))
- **ARM** : En cours de développement (voir [problème #490](https://github.com/microsoft/PowerToys/issues/490))

## <a name="current-powertoy-utilities"></a>Utilitaires PowerToys actuels

Les utilitaires actuellement disponibles sont les suivants :

### <a name="color-picker"></a>Sélecteur de couleurs

:::row:::
    :::column:::
        [![Capture d’écran de ColorPicker](../images/pt-color-picker.png)](color-picker.md)
    :::column-end:::
    :::column span="2":::
        [ColorPicker](color-picker.md) est un utilitaire de sélection de couleurs à l’échelle du système qui s’active avec <kbd>Win</kbd>+<kbd>Maj</kbd>+<kbd>C</kbd>. Sélectionnez une couleur dans n’importe quelle application en cours d’exécution. Le sélecteur la copie alors automatiquement dans le Presse-papiers dans un format configurable. Par ailleurs, ColorPicker contient un éditeur qui présente un historique des couleurs déjà sélectionnées, ce qui vous permet d’ajuster la couleur sélectionnée et de copier différentes représentations de chaîne. Ce code est basé sur [Color Picker de Martin Chrzan](https://github.com/martinchrzan/ColorPicker).
    :::column-end:::
:::row-end:::

### <a name="fancy-zones"></a>Fancy Zones

:::row:::
    :::column:::
        [![Capture d’écran de FancyZones](../images/pt-fancy-zones.png)](fancyzones.md)
    :::column-end:::
    :::column span="2":::
        [FancyZones](fancyzones.md) est un gestionnaire de fenêtres qui permet de créer facilement des dispositions de fenêtres complexes et de positionner rapidement des fenêtres dans ces dispositions.
    :::column-end:::
:::row-end:::

### <a name="file-explorer-add-ons"></a>Extensions de l’Explorateur de fichiers

:::row:::
    :::column:::
        [![Capture d’écran de l’Explorateur de fichiers](../images/pt-file-explorer.png)](file-explorer.md)
    :::column-end:::
    :::column span="2":::
        Les extensions de l’[Explorateur de fichiers](file-explorer.md) permettent d’afficher le volet de visualisation dans l’Explorateur de fichiers pour afficher des aperçus d’icônes SVG (.svg) et de fichiers Markdown (.md). Pour activer le volet de visualisation, sélectionnez l’onglet « Affichage » dans l’Explorateur de fichiers, puis « Volet de visualisation ».
    :::column-end:::
:::row-end:::

### <a name="image-resizer"></a>Image Resizer

:::row:::
    :::column:::
        [![Capture d’écran d’Image Resizer](../images/pt-image-resizer.png)](image-resizer.md)
    :::column-end:::
    :::column span="2":::
        [Image Resizer](image-resizer.md) est une extension de Windows Shell qui permet de redimensionner rapidement les images.  Dans l’Explorateur de fichiers, vous pouvez redimensionner une ou plusieurs images instantanément d’un simple clic droit. Ce code est basé sur [Image Resizer de Brice Lambson](https://github.com/bricelam/ImageResizer).
    :::column-end:::
:::row-end:::

### <a name="keyboard-manager"></a>Keyboard Manager

:::row:::
    :::column:::
        [![Capture d’écran de Keyboard Manager](../images/pt-keyboard-manager.png)](keyboard-manager.md)
    :::column-end:::
    :::column span="2":::
        [Keyboard Manager](keyboard-manager.md) vous permet de personnaliser le clavier à des fins de productivité en remappant les touches et en créant vos propres raccourcis clavier. Ce PowerToy nécessite Windows 10 1903 (build 18362) ou version ultérieure.
    :::column-end:::
:::row-end:::

### <a name="powerrename"></a>PowerRename

:::row:::
    :::column:::
        [![Capture d’écran de PowerRename](../images/pt-rename.png)](powerrename.md)
    :::column-end:::
    :::column span="2":::
        [PowerRename](powerrename.md) vous permet d’effectuer des renommages en bloc et de rechercher et remplacer des noms de fichiers. Doté de fonctionnalités avancées, il permet d’utiliser des expressions régulières, de cibler des types de fichiers spécifiques, de prévisualiser les résultats attendus et d’annuler les modifications. Ce code est basé sur [SmartRename de Chris Davis](https://github.com/chrdavis/SmartRename).
    :::column-end:::
:::row-end:::

### <a name="powertoys-run"></a>PowerToys Run

:::row:::
    :::column:::
        [![Capture d’écran de PowerToys Run](../images/pt-run.png)](run.md)
    :::column-end:::
    :::column span="2":::
        [PowerToys Run](run.md) peut vous aider à rechercher et lancer votre application instantanément : entrez simplement le raccourci <kbd>Alt</kbd>+<kbd>Espace</kbd> et commencez à taper. Il s’agit d’un utilitaire open source et modulaire qui peut recevoir des plug-ins supplémentaires. Window Walker est désormais également inclus. Ce PowerToy nécessite Windows 10 1903 (build 18362) ou version ultérieure.
    :::column-end:::
:::row-end:::

### <a name="shortcut-guide"></a>Shortcut Guide

:::row:::
    :::column:::
        [![Capture d’écran de Shortcut Guide](../images/pt-shortcut-guide.png)](shortcut-guide.md)
    :::column-end:::
    :::column span="2":::
        Le [guide des raccourcis de la touche Windows](shortcut-guide.md) s’affiche quand un utilisateur laisse la touche Windows enfoncée pendant plus d’une seconde. Les raccourcis disponibles pour l’état actuel du bureau s’affichent alors à l’écran.
    :::column-end:::
:::row-end:::

## <a name="powertoys-video-walk-through"></a>Démonstration vidéo PowerToys

Dans cette vidéo, Clint Rutkas (chef de projet PowerToys) vous montre comment installer et utiliser les différents utilitaires disponibles, vous fait part de quelques conseils, vous indique comment contribuer et bien plus encore.

> [!VIDEO https://channel9.msdn.com/Shows/Tabs-vs-Spaces/PowerToys-Utilities-to-customize-Windows-10/player?format=ny]

## <a name="future-powertoy-utilities"></a>Futurs utilitaires PowerToys

### <a name="experimental-powertoys"></a>PowerToys expérimentaux

Installez la version préliminaire expérimentale de PowerToys pour essayer les derniers utilitaires expérimentaux, à savoir :

#### <a name="video-conference-mute-experimental"></a>Video Conference Mute (expérimental)

:::row:::
    :::column:::
        [![Capture d’écran de Video Conference Mute](../images/pt-video-conference-mute.png)](video-conference-mute.md)
    :::column-end:::
    :::column span="2":::
        [Video Conference Mute](video-conference-mute.md) offre un moyen rapide de désactiver globalement le microphone et la caméra de votre ordinateur à l’aide du raccourci <kbd>⊞ Win</kbd>+<kbd>N</kbd> au cours d’une conférence téléphonique, quelle que soit l’application qui a le focus. Cet utilitaire est inclus uniquement dans la [version préliminaire/expérimentale de PowerToys](https://github.com/microsoft/PowerToys/releases/) et nécessite Windows 10 1903 (build 18362) ou version ultérieure.
    :::column-end:::
:::row-end:::

## <a name="known-issues"></a>Problèmes connus

Faites une recherche sur les problèmes connus ou signalez un nouveau problème sous l’onglet [Issues](https://github.com/microsoft/PowerToys/issues) (Problèmes) du dépôt PowerToys sur GitHub.

## <a name="contribute-to-powertoys-open-source"></a>Contribuer aux PowerToys (open source)

Les contributions aux PowerToys sont les bienvenues ! L’équipe de développement PowerToys se réjouit de collaborer avec la communauté d’utilisateurs chevronnés pour créer des outils qui permettent aux utilisateurs d’encore mieux exploiter Windows. Il existe plusieurs façons de contribuer :

- Rédiger des [spécifications techniques](https://codeburst.io/on-writing-tech-specs-6404c9791159)
- Soumettre un [concept de design ou une recommandation](https://www.microsoft.com/design/inclusive/)
- [Contribuer à la documentation](/contribute/)
- Identifier et corriger les bogues dans le [code source](https://github.com/microsoft/PowerToys/tree/master/src)
- [Coder de nouvelles fonctionnalités et des utilitaires PowerToys](https://github.com/microsoft/PowerToys/tree/master/doc/devdocs)

Avant de commencer à travailler sur la fonctionnalité que vous souhaitez proposer, **consultez [Contributor's Guide](https://github.com/microsoft/PowerToys/blob/master/CONTRIBUTING.md)** . L’équipe PowerToys se fera un plaisir de vous aider à suivre la meilleure approche, de vous livrer des conseils et des recommandations pendant le développement de la fonctionnalité et de vous éviter des efforts vains ou inutiles.

## <a name="powertoys-release-notes"></a>Notes de publication PowerToys

Les [notes de publication ](https://github.com/microsoft/PowerToys/releases/) de PowerToys sont listées dans la page d’installation du dépôt GitHub. Pour référence, vous trouverez également la [liste de vérification de publication](https://github.com/microsoft/PowerToys/wiki/Release-check-list) sur le wiki PowerToys.

## <a name="powertoys-history"></a>Historique des PowerToys

Inspirée du [projet PowerToys sous l’ère Windows 95](https://en.wikipedia.org/wiki/Microsoft_PowerToys), cette réédition permet aux utilisateurs chevronnés d’encore mieux exploiter le shell Windows 10 et de le personnaliser pour des workflows individuels.  Une trouverez une présentation remarquable de PowerToys pour Windows 95 [ici](https://socket3.wordpress.com/2016/10/22/using-windows-95-powertoys/).

## <a name="powertoys-roadmap"></a>Feuille de route de PowerToys

PowerToys est une équipe open source très réactive qui a pour objectif de permettre aux utilisateurs chevronnés d’encore mieux exploiter le shell Windows 10 et de le personnaliser pour des workflows individuels. Les priorités de travail seront régulièrement examinées, réévaluées et ajustées afin d’améliorer la productivité de nos utilisateurs.

- [Nouvelles spécifications pour les utilitaires PowerToys potentiels](https://github.com/microsoft/PowerToys/wiki/Specs)
- [Liste de priorités du Backlog](https://github.com/microsoft/PowerToys/wiki/Roadmap#backlog-priority-list-in-order)
- [Spécifications de la stratégie de la version 1.0](https://github.com/microsoft/PowerToys/wiki/Version-1.0-Strategy), février 2020