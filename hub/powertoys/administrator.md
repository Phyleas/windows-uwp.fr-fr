---
title: Mode administrateur PowerToys pour Windows 10
description: Pour que les PowerToys fonctionnent avec une application exécutée en mode d’administration élevée, les PowerToys doivent également être exécutés en mode administrateur.
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2eb514c25c135f8d58641232523a32f5d35153d4
ms.sourcegitcommit: 46a7e9db64e17a645ee6e888f62a9b04632c56af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97618538"
---
# <a name="powertoys-running-with-administrator-elevated-permissions"></a>PowerToys s’exécutant avec des autorisations élevées par l’administrateur

Si vous exécutez une application en tant qu’administrateur (également appelée « autorisations élevées »), les PowerToys peuvent ne pas fonctionner correctement lorsque les applications avec élévation de privilèges sont activées ou qu’elles essaient d’interagir avec une fonctionnalité PowerToys comme FancyZones. Pour ce faire, vous pouvez également exécuter PowerToys en tant qu’administrateur.

## <a name="options"></a>Options

Il existe deux options pour que les PowerToys prennent en charge les applications qui s’exécutent en tant qu’administrateur (avec des autorisations élevées) :

- **[Recommandé]**: les PowerToys affichent une invite quand un processus élevé est détecté. Ouvrez les **paramètres PowerToys**. Dans l’onglet **général** , sélectionnez « redémarrer en tant qu’administrateur ».

- Activez « toujours exécuter en tant qu’administrateur » dans les **paramètres PowerToys**.

## <a name="run-as-administrator-elevated-processes-explained"></a>Processus d’exécution en tant qu’administrateur avec élévation de privilèges

Les applications Windows s’exécutent en **mode utilisateur** par défaut. Pour exécuter une application en **mode administratif** ou en tant que *processus avec élévation de privilèges* , l’application s’exécute avec un accès supplémentaire au système d’exploitation.

La façon la plus simple d’exécuter une application ou un programme en mode d’administration consiste à cliquer avec le bouton droit sur le programme et à sélectionner **exécuter en tant qu’administrateur**. Si l’utilisateur actuel n’est pas un administrateur, Windows vous demandera le nom d’utilisateur et le mot de passe de l’administrateur.

La plupart des applications n’ont pas besoin de s’exécuter avec des autorisations élevées. Toutefois, un scénario courant pour exiger une autorisation d’administrateur serait d’exécuter certaines commandes PowerShell ou de modifier le registre.

Si vous voyez cette invite (utilisateur Access Control invite), l’application demande une autorisation élevée au niveau de l’administrateur :

![Capture d’écran de l’invite d’autorisation Windows avec élévation de privilèges](../images/pt-admin-prompt.png)

Dans le cas d’une ligne de commande avec élévation de privilèges, le titre « administrateur » est généralement ajouté à la barre de titre.

![Capture d’écran de la ligne de commande d’administration Windows](../images/pt-admin-terminal.png)

## <a name="support-for-admin-mode-with-powertoys"></a>Prise en charge du mode administrateur avec les PowerToys

PowerToys a uniquement besoin d’une autorisation d’administrateur élevée lors de l’interaction avec d’autres applications qui s’exécutent en mode administrateur. Si ces applications sont activées, les PowerToys peuvent ne pas fonctionner, sauf s’il est également élevé.

Voici les deux scénarios que nous ne traiterons pas dans :

- Interception de certains types de frappes de clavier
- Redimensionner/déplacer des fenêtres

### <a name="affected-powertoys-utilities"></a>Utilitaires PowerToys affectés

Les autorisations en mode administrateur peuvent être nécessaires dans les scénarios suivants :

- FancyZones
  - Alignement d’une fenêtre avec élévation de privilèges (par exemple, une invite de commandes) dans une zone fantaisie
  - Déplacement de la fenêtre avec élévation de privilèges vers une autre zone
- Guide de raccourci
  - Afficher le raccourci
- Remappeur de clavier
  - Clé de remappage de clé
  - Remappage des raccourcis de niveau global
  - Remappage des raccourcis ciblés par l’application
- Exécution des PowerToys
  - Afficher le raccourci
