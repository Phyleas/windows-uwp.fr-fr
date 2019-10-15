---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: 2f7a57f1652ecab81a70c39faa1b70c42ed6a3de
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314903"
---
La saisie de `sudo service mongodb start` ou `sudo service postgres start` et `sudo -u postgrest psql` peut être fastidieuse.  Toutefois, vous pouvez envisager de configurer des alias dans votre fichier `.profile` sur WSL pour que ces commandes soient plus rapides à utiliser et faciles à mémoriser. 

Pour configurer votre propre alias personnalisé, ou raccourci, pour l’exécution de ces commandes :

1. Ouvrez votre terminal WSL et entrez `cd ~` pour vous assurer que vous êtes dans le répertoire racine.
2. Ouvrez le fichier `.profile`, qui contrôle les paramètres de votre terminal, avec l’éditeur de texte de terminal, nano : `sudo nano .profile`
3. En bas du fichier (ne modifiez pas les paramètres `# set PATH`), ajoutez ce qui suit :

    ```bash
    # My Aliases
    alias start-pg='sudo service postgresql start'
    alias run-pg='sudo -u postgres psql'
    ```

Cela vous permettra d’entrer `start-pg` pour commencer à exécuter le service PostgreSQL et `run-pg` pour ouvrir l’interpréteur de commandes psql. Vous pouvez remplacer `start-pg` et `run-pg` par le nom de votre choix. Veillez simplement à ne pas remplacer une commande que postgres utilise déjà !

4. Une fois que vous avez ajouté vos nouveaux alias, quittez l’éditeur de texte nano à l’aide de la **combinaison de touches Ctrl + X** --Select `Y` (oui) quand vous êtes invité à enregistrer et à entrer (en laissant le nom de fichier `.profile`).
5. Fermez et rouvrez votre terminal WSL, puis essayez vos nouvelles commandes d’alias.
