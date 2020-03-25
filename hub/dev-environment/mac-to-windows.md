---
title: Aide sur le passage de Mac (UNIX) à Windows
description: Guide pour vous aider à passer d’un Mac (UNIX) à un environnement de développement Windows, y compris le mappage de touches de raccourci et une brève vue d’ensemble des concepts qui diffèrent entre Mac et Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Mac vers Windows, mappage des touches de raccourci, passage d’UNIX vers Windows, transition de Mac vers Windows, aide à passer de MacBook à la surface, utilisation de Windows pour un utilisateur Macintosh, passage de Macintosh à Windows, aide à la modification des environnements de développement, Mac OS X à Windows, aide passage de Mac à PC
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 8c23fa3e6791a3cd78d259b40e68606a30fd9395
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218439"
---
# <a name="guide-for-changing-your-dev-environment-from-mac-to-windows"></a>Guide pour la modification de votre environnement de développement de Mac à Windows

Les conseils et les équivalents de contrôle suivants devraient vous aider dans votre transition entre un environnement de développement Mac et Windows (ou WSL/Linux).

Pour le développement d’applications, l’équivalent le plus proche de Xcode serait [Visual Studio](https://visualstudio.microsoft.com). Il existe également une version de [Visual Studio pour Mac](https://visualstudio.microsoft.com/vs/mac/), si vous avez toujours besoin de revenir en arrière. Pour la modification de code source multiplateforme (et un grand nombre de plug-ins) [Visual Studio code](https://code.visualstudio.com/?wt.mc_id=DX_841432) est le choix le plus populaire.

## <a name="keyboard-shortcuts"></a>raccourcis clavier

| **Opération** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Copier | Commande + C | Ctrl+C |
| Couper | Commande + X | Ctrl+X |
| Coller | Commande + V | Ctrl+V |
| Annuler | Commande + Z | Ctrl+Z |
| Enregistrer | Commande + S | Ctrl+S |
| Ouvrir | Commande + O | Ctrl+O |
| Verrouiller l’ordinateur | Commande + CTRL + Q | WindowsKey + L |
| Afficher le Bureau | Commande + F3 | WindowsKey + D |
| Ouvrir l’Explorateur de fichiers | Commande + N | WindowsKey + E |
| Réduire les fenêtres | Commande + M | WindowsKey + M |
| Recherche | Commande + espace | WindowsKey |
| Fermer la fenêtre active | Commande + W | CTRL + W |
| Basculer la tâche active | Commande + Tab | Alt+Tabulation |
| Agrandir une fenêtre en plein écran | Ctrl + Commande + F | WindowsKey + haut |
| Écran d’enregistrement (capture d’écran) | Commande + Maj + 3 | WindowsKey + Maj + S |
| Enregistrer la fenêtre | Commande + Maj + 4 | WindowsKey + Maj + S |
| Afficher les informations ou les propriétés d’un élément | Commande + I | Alt+Entrée |
 | Sélectionner tous les éléments | Commande + A | Ctrl+A |
| Sélectionner plusieurs éléments dans une liste (non contigus) | , Puis cliquez sur chaque élément. | , Puis cliquez sur chaque élément |
| Taper des caractères spéciaux | Option + touche de caractère | ALT + touche de caractère|

## <a name="trackpad-shortcuts"></a>Raccourcis sur pavé tactile

Remarque : certains de ces raccourcis requièrent un « pavé tactile de précision », tel que les pavé tactile sur les appareils surface et d’autres ordinateurs portables tiers.

 **Opération** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Scroll | Balayage vertical à deux doigts | Balayage vertical à deux doigts |
| Zoom | Deux doigts de pincement et de sortie | Deux doigts de pincement et de sortie |
| Faire glisser vers l’avant et vers l’arrière entre les vues | Balayage latéral à deux doigts | Balayage latéral à deux doigts |
| Changer d’espace de travail virtuel | Balayage latéral à quatre doigts | Balayage latéral à quatre doigts |
| Afficher les applications actuellement ouvertes | Balayage vers le haut à quatre doigts | Balayage vers le haut à trois doigts |
| Basculer entre les applications | N/A | Balayage latéral à trois doigts lent |
| Accéder au bureau | Étaler quatre doigts | Balayage à trois doigts vers le bas |
| Ouvrir Cortana/Centre de maintenance | Diapositive à deux doigts à partir de la droite | Robinet à trois doigts |
| Ouvrir des informations supplémentaires | Robinet à trois doigts | N/A |
|Afficher Launchpad/démarrer une application | Pincer avec quatre doigts | Appuyer avec quatre doigts |

Remarque : les options pavé tactile sont configurables sur les deux plateformes.

## <a name="terminal-and-shell"></a>Terminal et shell

Windows fournit plusieurs alternatives à l’émulateur de terminal de Mac.

1. La ligne de commande Windows

La ligne de commande Windows accepte les commandes DOS et est l’outil de ligne de commande le plus couramment utilisé sur Windows. Pour l’ouvrir : Appuyez sur **WindowsKey + R** pour ouvrir la zone **exécuter** , tapez **cmd** , puis cliquez sur **OK**. Pour ouvrir une ligne de commande d’administrateur, tapez **cmd** , puis appuyez sur **Ctrl + Maj + Entrée**.

2. PowerShell

[PowerShell](https://docs.microsoft.com/powershell/scripting/overview?view=powershell-6) est un «interpréteur de ligne de commande et langage de script basé sur des tâches basé sur .net. PowerShell permet aux administrateurs système et aux utilisateurs d’alimenter d’automatiser rapidement les tâches qui gèrent les systèmes d’exploitation. En d’autres termes, il s’agit d’une ligne de commande très puissante, qui est particulièrement appréciée par les administrateurs système.

Par ailleurs, PowerShell est [également disponible pour Mac](https://docs.microsoft.com/powershell/scripting/install/installing-powershell-core-on-macos?view=powershell-6).

3. Sous-système Windows pour Linux (WSL)

WSL vous permet d’exécuter un interpréteur de commandes Linux dans Windows. Cela signifie que vous pouvez exécuter *bash** ou un autre shell, en fonction de votre choix et de la distribution Linux spécifique installée. L’utilisation de WSL fournira le type d’environnement le plus familier aux utilisateurs Mac. Par exemple, vous pouvez **créer une liste de fichiers** dans un répertoire actif, et **non pas** sur la ligne de commande Windows. Pour en savoir plus sur l’installation et l’utilisation de WSL, consultez le [Guide d’installation du sous-système Windows pour Linux pour Windows 10](https://docs.microsoft.com/windows/wsl/install-win10).

4. Terminal Windows (version préliminaire)

Windows Terminal est une application qui combine des outils en ligne de commande et des shells provenant de plusieurs sources, notamment la ligne de commande Windows traditionnelle, PowerShell et le sous-système Windows pour Linux. Bien qu’il soit actuellement en préversion, il alreaedy contient plusieurs fonctionnalités utiles, telles que la prise en charge de plusieurs onglets, des volets fractionnés, des thèmes et des styles personnalisés et une prise en charge complète d’Unicode. Windows Terminal peut être installé à partir du [Microsoft Store sur Windows 10](https://www.microsoft.com/en-us/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab).

## <a name="apps-and-utilities"></a>Applications et utilitaires

 **Lancement** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Paramètres et préférences | Préférences système | Paramètres |
| Gestionnaire des tâches | Moniteur d'activité | Task Manager |
| Mise en forme du disque | Utilitaire de disque | Gestion des disques |
| Modification de texte | TextEdit | Bloc-notes |
| Affichage des événements | Console | Observateur d’événements |
| Rechercher des fichiers/applications | Commande + espace | touche Windows |
