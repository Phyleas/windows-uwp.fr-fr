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
ms.openlocfilehash: fa137ab51f0bb53e2907fa319d79ed77eb7ed655
ms.sourcegitcommit: 1e06168ada5ce6013b1d07c428548f084464a286
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87363708"
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

## <a name="command-line-shells-and-terminals"></a>Terminaux et shells en ligne de commande

Windows prend en charge plusieurs terminaux et shells en ligne de commande qui fonctionnent parfois un peu différemment du shell BASH de Mac et des applications de l’émulateur de terminal comme Terminal et iTerm.

### <a name="windows-shells"></a>Shells Windows

Windows dispose de deux shells en ligne de commande principaux :

1. **[PowerShell](https://docs.microsoft.com/powershell/scripting/overview?view=powershell-7)** - PowerShell est un framework multiplateforme de gestion de la configuration et de l’automatisation des tâches, composé d’un shell en ligne de commande et d’un langage de script reposant sur .NET. À l’aide de PowerShell, les administrateurs, les développeurs et les utilisateurs avec pouvoir peuvent rapidement contrôler et automatiser des tâches qui gèrent des processus complexes et divers aspects de l’environnement et du système d’exploitation où il est exécuté. PowerShell est [entièrement open source](https://github.com/powershell/powershell) et, comme il s’agit d’un framework multiplateforme, est également [disponible pour Mac et Linux](https://docs.microsoft.com/powershell/scripting/install/installing-powershell?view=powershell-7).

    **Utilisateurs du shell BASH de Mac et Linux** : PowerShell prend également en charge de nombreux alias de commande que vous connaissez déjà. Par exemple :
    - Lister le contenu du répertoire actif avec : `ls`
    - Déplacer des fichiers avec : `mv`
    - Accéder à un nouveau répertoire avec : `cd <path>`

    Certaines commandes et certains arguments sont différents dans PowerShell et dans BASH. Pour en savoir plus, entrez [`get-help`](https://docs.microsoft.com/powershell/scripting/learn/ps101/02-help-system?view=powershell-7) dans PowerShell ou consultez les [alias de compatibilité](https://docs.microsoft.com/powershell/scripting/samples/appendix-1---compatibility-aliases?view=powershell-7) dans la documentation.

    Pour exécuter PowerShell en tant qu’administrateur, entrez « PowerShell » dans le menu Démarrer de Windows, puis sélectionnez « Exécuter en tant qu’administrateur ».

2. **Ligne de commande Windows (Cmd)**  : Windows est toujours livré avec l’invite de commandes classique (et la console – voir ci-dessous), assurant ainsi la compatibilité avec les commandes et les fichiers de commandes compatibles MS-DOS actuels et hérités. Cmd est utile lors de l’exécution d’opérations en ligne de commande ou de fichiers de commandes existants/anciens, mais il est en général recommandé aux utilisateurs d’apprendre et d’utiliser PowerShell, car Cmd est maintenant en maintenance et ne bénéficiera pas d’améliorations ni de nouvelles fonctionnalités à l’avenir.

### <a name="linux-shells"></a>Shells Linux

Le sous-système Windows pour Linux (WSL) peut désormais être installé pour prendre en charge l’exécution d’un shell Linux dans Windows. Cela signifie que vous pouvez exécuter **bash**, avec la distribution Linux spécifique que vous choisissez, intégrée directement dans Windows. WSL offre aux utilisateurs de Mac un type d'environnement qui leur est familier. À titre d’exemple, vous utiliserez **ls** pour lister les fichiers d’un répertoire actif, et non **dir** comme vous le feriez avec l’interface de commande Windows classique. Pour en savoir plus sur l’installation et l’utilisation de WSL, consultez le [Guide d’installation du sous-système Windows pour Linux pour Windows 10](https://docs.microsoft.com/windows/wsl/install-win10). Les distributions Linux qui peuvent être installées sur Windows avec WSL sont notamment les suivantes :

1. [Ubuntu 20.04 LTS](https://www.microsoft.com/store/apps/9n6svws3rx71)
2. [Kali Linux](https://www.microsoft.com/store/apps/9PKR34TNCV07)
3. [Debian GNU/Linux](https://www.microsoft.com/store/apps/9MSVKQC78PK6)
4. [openSUSE Leap 15.1](https://www.microsoft.com/store/apps/9NJFZK00FGKV)
5. [SUSE Linux Enterprise Server 15 SP1](https://www.microsoft.com/store/apps/9PN498VPMF3Z)

Pour n’en nommer que quelques-unes. Trouvez-en d’autres dans la [documentation sur l’installation de WSL](https://docs.microsoft.com/windows/wsl/install-win10#install-your-linux-distribution-of-choice) et installez-les directement à partir du [Microsoft Store](https://www.microsoft.com/search/shop/apps?q=linux&category=Developer+tools).

## <a name="windows-terminals"></a>Terminaux Windows

En plus de nombreuses offres tierces, Microsoft propose deux « terminaux », des applications GUI qui fournissent l’accès aux shells et applications en ligne de commande.

1. **[Terminal Windows](https://docs.microsoft.com/windows/terminal/)**  : Le Terminal Windows est une nouvelle application de terminal de ligne de commande moderne et hautement configurable qui offre des performances très élevées, une expérience utilisateur de ligne de commande à faible latence, plusieurs onglets, des volets de fenêtre fractionnés, des thèmes et des styles personnalisés, plusieurs « profils » pour différents shells ou applications en ligne de commande ainsi que des possibilités considérables pour configurer et personnaliser de nombreux aspects de votre expérience utilisateur.

    Vous pouvez utiliser le Terminal Windows pour ouvrir des onglets connectés à PowerShell, des shells WSL (comme Ubuntu ou Debian), l’invite de commandes Windows classique ou toute autre application en ligne de commande (par exemple SSH, Azure CLI, Git Bash).

2. **[Console](https://docs.microsoft.com/windows/console/)**  : Sur Mac et Linux, les utilisateurs démarrent généralement leur application de terminal préférée qui crée ensuite le shell par défaut de l’utilisateur (par exemple, BASH) et s’y connecte.

    Toutefois, en raison d’une particularité de l’histoire, les utilisateurs de Windows démarrent généralement leur shell, et Windows démarre et connecte automatiquement une application console GUI.

    Alors qu’il est toujours possible de lancer des shells directement et d’utiliser la console Windows héritée, il est vivement recommandé aux utilisateurs d’installer et d’utiliser le Terminal Windows pour bénéficier de l’expérience de ligne de commande la plus efficace, la plus rapide et la plus productive.

## <a name="apps-and-utilities"></a>Applications et utilitaires

 **Application** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Paramètres et préférences | Préférences système | Paramètres |
| Gestionnaire des tâches | Moniteur d'activité | Gestionnaire des tâches |
| Formatage de disque | Utilitaire de disque | Gestion des disques |
| Modification de texte | TextEdit | Bloc-notes |
| Affichage des événements | Console | Observateur d’événements |
| Rechercher des fichiers/applications | Commande+Espace | Touche Windows |
