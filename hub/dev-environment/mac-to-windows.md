---
title: Aide relative à la transition de Mac (Unix) vers Windows
description: Guide pour vous aider à passer d'un environnement de développement Mac (UNIX) à Windows, comprenant notamment un mappage des touches de raccourci et une vue d’ensemble des concepts qui diffèrent entre Mac et Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Mac vers Windows, mappage des touches de raccourci, transition d'Unix vers Windows, transition de Mac vers Windows, aide relative à la transition de MacBook vers Surface, utilisation de Windows pour un utilisateur Macintosh, transition de Macintosh vers Windows, aide relative au changement d'environnement de développement, Mac OS X vers Windows, aide relative à la transition de Mac vers PC
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 457abcec97247afcc0d63c983c8a6cda2de51c66
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81643698"
---
# <a name="guide-for-changing-your-dev-environment-from-mac-to-windows"></a>Guide relatif au changement d'environnement de développement de Mac vers Windows

Les conseils et contrôles équivalents suivants devraient faciliter votre transition entre un environnement de développement Mac et Windows (ou WSL/Linux).

En termes de développement d’applications, l’équivalent le plus proche de Xcode est [Visual Studio](https://visualstudio.microsoft.com). Il existe également une version de [Visual Studio pour Mac](https://visualstudio.microsoft.com/vs/mac/), si besoin. En termes de modification de code source multiplateforme (et pour un grand nombre de plug-ins), [Visual Studio Code](https://code.visualstudio.com/?wt.mc_id=DX_841432) est la solution la plus utilisée.

## <a name="keyboard-shortcuts"></a>Raccourcis clavier

| **Opération** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Copier | Commande+C | Ctrl+C |
| Couper | Commande+X | Ctrl+X |
| Coller | Commande+V | Ctrl+V |
| Annuler | Commande+V | Ctrl+Z |
| Enregistrer | Commande+S | Ctrl+S |
| Ouvrir | Commande+O | Ctrl+O |
| Verrouillage de l’ordinateur | Commande+CTRL+Q | Touche Windows+L |
| Afficher le bureau | Commande+F3 | Touche Windows+D |
| Ouvrir l'Explorateur de fichiers | Commande+N | Touche Windows+E |
| Réduire les fenêtres | Commande+M | Touche Windows+M |
| Rechercher | Commande+Espace | Touche Windows |
| Fermer la fenêtre active | Commande+W | CTRL+W |
| Basculer vers la tâche actuelle | Commande+Tab | Alt+Tab |
| Afficher une fenêtre en plein écran | Ctrl+Commande+F | Touche Windows+Haut |
| Écran d’enregistrement (capture d’écran) | Commande+Maj+3 | Touche Windows+Maj+S |
| Enregistrer une fenêtre | Commande+Maj+4 | Touche Windows+Maj+S |
| Afficher les informations ou propriétés d’un élément | Commande+I | Alt+Entrée |
 | Sélectionner tous les éléments | Commande+A | Ctrl+A |
| Sélectionner plusieurs éléments dans une liste (non contigus) | Commande, puis cliquer sur chaque élément | Contrôle, puis cliquer sur chaque élément |
| Entrer des caractères spéciaux | Option+touche de caractère | Alt+touche de caractère|

## <a name="trackpad-shortcuts"></a>Raccourcis sur pavé tactile

Remarque : Certains de ces raccourcis requièrent un « pavé tactile de précision », tel que le pavé tactile des appareils Surface et de certains ordinateurs portables tiers.

 **Opération** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Scroll | Balayage vertical avec deux doigts | Balayage vertical avec deux doigts |
| Zoom | Resserrer et écarter deux doigts | Resserrer et écarter deux doigts |
| Balayage vers l'avant et l'arrière entre les vues | Balayage latéral avec deux doigts | Balayage latéral avec deux doigts |
| Changer d’espace de travail virtuel | Balayage latéral avec quatre doigts | Balayage latéral avec quatre doigts |
| Afficher les applications ouvertes | Balayage vers le haut avec quatre doigts | Balayage vers le haut avec trois doigts |
| Basculer entre les applications | NON APPLICABLE | Balayage latéral lent avec trois doigts |
| Accéder au bureau | Écarter quatre doigts | Balayage vers le bas avec trois doigts |
| Ouvrir Cortana / Centre de notifications | Balayage avec deux doigts à partir de la droite | Appui avec trois doigts |
| Ouvrir des informations supplémentaires | Appui avec trois doigts | NON APPLICABLE |
|Afficher Launchpad / Démarrer une application | Pincer avec quatre doigts | Appuyer avec quatre doigts |

Remarque : Les options du pavé tactile peuvent être configurées sur les deux plateformes.

## <a name="terminal-and-shell"></a>Terminal et shell

Windows offre plusieurs alternatives à l’émulateur de terminal Mac.

1. Ligne de commande Windows

La ligne de commande Windows accepte les commandes DOS et constitue l'outil de ligne de commande le plus utilisé sur Windows. Pour l'ouvrir : Appuyez sur **Touche Windows+R** pour ouvrir la zone **Exécuter**, entrez **cmd**, puis cliquez sur **OK**. Pour ouvrir une ligne de commande d’administrateur, entrez **cmd**, puis appuyez sur **Ctrl+Maj+Entrée**.

2. PowerShell

[PowerShell](https://docs.microsoft.com/powershell/scripting/overview?view=powershell-6) est un « interpréteur de ligne de commande basé sur les tâches et un langage de script reposant sur .NET. PowerShell permet aux administrateurs système et aux utilisateurs avec pouvoir de rapidement automatiser les tâches qui gèrent les systèmes d’exploitation ». En d’autres termes, il s’agit d’une ligne de commande très puissante, particulièrement appréciée des administrateurs système.

De plus, PowerShell est [également disponible pour Mac](https://docs.microsoft.com/powershell/scripting/install/installing-powershell-core-on-macos?view=powershell-6).

3. Sous-système Windows pour Linux (WSL)

WSL vous permet d’exécuter un shell Linux dans Windows. Ainsi, vous pouvez exécuter **bash** ou un autre shell, selon vos besoin et la distribution Linux spécifique installée. WSL offre aux utilisateurs de Mac un type d'environnement qui leur est familier. À titre d'exemple, vous utiliserez **ls** pour répertorier les fichiers d'un répertoire actif, et non **dir** comme vous le feriez avec la ligne de commande Windows. Pour en savoir plus sur l’installation et l’utilisation de WSL, consultez le [Guide d’installation du sous-système Windows pour Linux pour Windows 10](https://docs.microsoft.com/windows/wsl/install-win10).

4. Terminal Windows (préversion)

L'application Terminal Windows combine des outils en ligne de commande et des shells issus de différentes sources, notamment la ligne de commande Windows classique, PowerShell et le sous-système Windows pour Linux. Actuellement en préversion, elle propose néanmoins différentes fonctionnalités très utiles, telles qu’une prise en charge de plusieurs onglets, des volets de fractionnement, des thèmes et des styles personnalisés, ainsi qu’une prise en charge complète d’Unicode. L'application Terminal Windows peut être installée à partir du [Microsoft Store sur Windows 10](https://www.microsoft.com/en-us/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab).

## <a name="apps-and-utilities"></a>Applications et utilitaires

 **Application** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Paramètres et préférences | Préférences système | Paramètres |
| Gestionnaire des tâches | Moniteur d'activité | Gestionnaire des tâches |
| Formatage de disque | Utilitaire de disque | Gestion des disques |
| Modification de texte | TextEdit | Bloc-notes |
| Affichage des événements | Console | Observateur d’événements |
| Rechercher des fichiers/applications | Commande+Espace | Touche Windows |
