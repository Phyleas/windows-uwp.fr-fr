---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: 2f7a57f1652ecab81a70c39faa1b70c42ed6a3de
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "72314903"
---
La saisie de `sudo service mongodb start` ou de `sudo service postgres start` et de `sudo -u postgrest psql` peut être fastidieuse.  Toutefois, vous pouvez envisager de configurer des alias dans votre fichier `.profile` sur WSL pour accélérer l’utilisation de ces commandes et vous en souvenir plus facilement. 

Pour configurer votre propre alias personnalisé, ou raccourci, pour l’exécution de ces commandes :

1. Ouvrez votre terminal WSL et entrez `cd ~` pour vous assurer d’être dans le répertoire racine.
2. Ouvrez le fichier `.profile`, qui contrôle les paramètres de votre terminal, avec l’éditeur de texte de terminal, Nano : `sudo nano .profile`
3. En bas du fichier (ne modifiez pas les paramètres de `# set PATH`), ajoutez ce qui suit :

    ```bash
    # My Aliases
    alias start-pg='sudo service postgresql start'
    alias run-pg='sudo -u postgres psql'
    ```

Cela permettra d’entrer `start-pg` pour commencer à exécuter le service postgresql et `run-pg` pour ouvrir le shell psql. Vous pouvez modifier `start-pg` et `run-pg` et choisir n’importe quel nom. Veillez simplement à ne pas remplacer une commande que postgres utilise déjà !

4. Une fois les nouveaux alias ajoutés, quittez l’éditeur de texte Nano à l’aide de la combinaison de touches **Ctrl + X**. Sélectionnez `Y` (Oui) lorsque vous êtes invité à enregistrer et à appuyer sur la touche Entrée (en laissant `.profile` comme nom de fichier).
5. Fermez et rouvrez votre terminal WSL, puis essayez vos nouvelles commandes d’alias.
