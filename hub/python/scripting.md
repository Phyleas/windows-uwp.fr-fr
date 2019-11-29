---
title: Écriture de scripts et automatisation avec Python sur Windows
description: Comment prendre en main Python pour l’écriture de scripts, l’automatisation et l’administration de systèmes sur Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, microsoft, administration de système python, automatisation de fichier python, scripts python sur windows, configurer python sur windows, environnement de développement python sur windows, environnement de développement python sur windows, python avec powershell, scripts python pour tâches du système de fichiers
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 3f8a17de8121fed27e69442d5560f702a04c8e42
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314863"
---
# <a name="get-started-using-python-on-windows-for-scripting-and-automation"></a>Bien démarrer avec Python pour l’écriture de scripts et l’automatisation sur Windows

Voici un guide pas à pas pour la configuration de votre environnement de développement et la prise en main de Python pour l’écriture de scripts et l’automatisation des opérations de système de fichiers sur Windows.

> [!NOTE]
> Cet article traite de la configuration de votre environnement pour utiliser certaines bibliothèques utiles dans Python capables d’automatiser des tâches sur plusieurs plateformes, telles que la recherche dans votre système de fichiers, l’accès à Internet, l’analyse des types de fichiers, etc., selon une approche centrée sur Windows. Pour les opérations spécifiques de Windows, consultez les sections relatives à [ctypes](https://docs.python.org/3/library/ctypes.html), une bibliothèque de fonctions étrangères compatible C pour Python, à [winreg](https://docs.python.org/3/library/winreg.html), les fonctions qui exposent l’API du Registre Windows à Python, et à [Python/WinRT](https://pypi.org/project/winrt/) permettant d’accéder aux API Windows Runtime API à partir de Python.

## <a name="set-up-your-development-environment"></a>Configurer votre environnement de développement

Lorsque vous utilisez Python pour écrire des scripts qui effectuent des opérations sur le système de fichiers, nous vous recommandons d’[installer Python à partir du Microsoft Store](https://www.microsoft.com/en-us/p/python-37/9nj46sx7x90p?activetab=pivot:overviewtab). L’installation via le Microsoft Store utilise l’interpréteur de base Python3, mais gère la configuration de vos paramètres PATH pour l’utilisateur actuel (évitant ainsi le recours à un accès administrateur), en plus de fournir des mises à jour automatiques.

Si vous utilisez Python pour **le développement web** sous Windows, nous vous recommandons une configuration différente à l’aide du sous-système Windows pour Linux. Vous trouverez une procédure pas à pas dans notre guide : [Bien démarrer avec Python pour le développement web sur Windows](./web-frameworks.md). Si vous êtes novice en Python, essayez notre guide : [Prise en main de Python sur Windows pour les débutants](./beginners.md). Pour certains scénarios avancés (nécessité d’accéder aux fichiers installés de Python ou de les modifier, de créer des copies de fichiers binaires ou d’utiliser directement des DLL Python), vous pouvez télécharger une version de Python spécifique directement depuis [python.org](https://www.python.org/downloads/) ou installer une [autre solution](https://www.python.org/download/alternatives), telle que Anaconda, Jython, PyPy, WinPython, IronPython, etc. Nous vous recommandons de le faire uniquement si vous êtes un programmeur Python plus avancé possédant une raison spécifique de choisir un autre type d’implémentation.

## <a name="install-python"></a>Installer Python

Pour installer Python à partir du Microsoft Store :

1. Accédez à votre menu **Démarrer** (icône Windows en bas à gauche), tapez « Microsoft Store », puis cliquez sur le lien pour ouvrir le Store.

2. Une fois le Store ouvert, sélectionnez **Rechercher** dans le menu supérieur droit, puis entrez « Python ». Ouvrez « Python 3.7 » dans les résultats sous Applications. Sélectionnez **Obtenir**.

3. Une fois que Python a terminé le processus de téléchargement et d’installation, ouvrez Windows PowerShell à l’aide du menu **Démarrer** (icône Windows en bas à gauche). Une fois PowerShell ouvert, entrez `Python --version` pour vérifier que Python3 est installé sur votre ordinateur.

4. L’installation Microsoft Store de Python comprend **pip**, le gestionnaire de package standard. pip vous permet d’installer et de gérer des packages supplémentaires qui ne font pas partie de la bibliothèque standard Python. Pour confirmer que pip est également disponible pour installer et gérer des packages, entrez `pip --version`.

## <a name="install-visual-studio-code"></a>Installation de Visual Studio Code

En utilisant VS Code comme éditeur de texte/environnement de développement intégré (IDE), vous pouvez tirer parti d’[IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) (aide pour l’exécution du code), de [Linting](https://code.visualstudio.com/docs/python/linting) (permet d’éviter les erreurs dans votre code), de la [prise en charge du débogage](https://code.visualstudio.com/docs/python/debugging) (aide à trouver les erreurs dans votre code après l’avoir exécuté), des [extraits de code](https://code.visualstudio.com/docs/editor/userdefinedsnippets) (modèles pour les petits blocs de code réutilisables) et des [tests unitaires](https://code.visualstudio.com/docs/python/unit-testing) (tests de l’interface du code avec différents types d’entrée).

Téléchargez VS Code pour Windows et suivez les instructions d’installation : [https://code.visualstudio.com](https://code.visualstudio.com).

## <a name="install-the-microsoft-python-extension"></a>Installer l’extension Python Microsoft

Vous devez installer l’extension Python Microsoft afin de tirer parti des fonctionnalités de prise en charge de VS Code. [En savoir plus](https://code.visualstudio.com/docs/languages/python).

1. Ouvrez la fenêtre Extensions de VS Code en saisissant **Ctrl + Maj + X** (ou utilisez le menu pour accéder à **Afficher** > **Extensions**).

2. Dans la zone **Rechercher des extensions dans Marketplace**, saisissez :  **Python**.

3. Recherchez l’extension **Python (ms-python.python) par Microsoft** et sélectionnez le bouton vert **Installer**.

## <a name="open-the-integrated-powershell-terminal-in-vs-code"></a>Ouvrez le terminal PowerShell intégré dans VS Code

VS Code contient également un [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal) qui vous permet d’ouvrir une ligne de commande Python avec PowerShell en établissant un flux de travail homogène entre votre éditeur de code et la ligne de commande.

1. Ouvrez le terminal dans VS Code, sélectionnez **Afficher** > **Terminal** ou utilisez le raccourci **Ctrl+`** (à l’aide du guillemet inversé).

    > [!NOTE]
    > Le terminal par défaut doit être PowerShell. Toutefois, si vous devez le modifier, utilisez **Ctrl + Maj + P** pour accéder à la palette de commandes. Entrez **Terminal : Sélectionnez le** shell par défaut et une liste d’options de terminal s’affiche sur laquelle figure PowerShell, l’invite de commandes, WSL, etc. Sélectionnez celle que vous souhaitez utiliser et entrez **Ctrl + Maj + ’** (à l’aide de l’accent grave) pour créer un nouveau terminal.

2. À l’intérieur de votre terminal VS Code, ouvrez Python en entrant :`python`

3. Essayez l’interpréteur Python en entrant : `print("Hello World")`. Python renverra l’instruction « Hello World ».

    ![Ligne de commande Python dans VS Code](../images/python-in-vscode.png)

4. Pour quitter Python, vous pouvez entrer `exit()`, `quit()` ou sélectionner Ctrl-Z.

## <a name="install-git-optional"></a>Installer Git (facultatif)

Si vous envisagez de collaborer avec d’autres personnes sur votre code Python ou d’héberger votre projet sur un site open source (comme GitHub), VS Code prend en charge le [contrôle de version avec Git](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support). L’onglet Contrôle de code source de VS Code assure le suivi de toutes vos modifications et contient des commandes Git courantes (ajouter, valider, pousser, extraire) intégrées directement dans l’interface utilisateur. Vous devez d’abord installer Git pour gérer le panneau Contrôle de code source.

1. Téléchargez et installez Git pour Windows sur le [site web git-scm](https://git-scm.com/download/win).

2. Vous trouverez un assistant d’installation qui vous posera une série de questions sur les paramètres de votre installation Git. Nous vous recommandons d’utiliser tous les paramètres par défaut, sauf si vous avez une raison particulière de les modifier.

3. Si vous n’avez jamais travaillé avec Git, les [guides GitHub](https://guides.github.com/) peuvent vous aider à démarrer.

## <a name="example-script-to-display-the-structure-of-your-file-system-directory"></a>Exemple de script pour afficher la structure de votre répertoire de système de fichiers

Les tâches courantes d’administration du système peuvent prendre beaucoup de temps, toutefois, un script Python, vous permet d’automatiser ces tâches afin qu’elles deviennent totalement transparentes. Par exemple, Python peut lire le contenu du système de fichiers de votre ordinateur et effectuer des opérations, telles que l’impression d’un plan de vos fichiers et répertoires, le déplacement de dossiers d’un répertoire vers un autre, ou le changement de nom de centaines de fichiers. Normalement, ces types de tâches peuvent prendre beaucoup de temps si vous deviez les exécuter manuellement. Utilisez plutôt un script Python !

Commençons par un script simple qui parcourt une arborescence de répertoires et affiche la structure de répertoires.

1. Ouvrez PowerShell à l’aide du menu **Démarrer** (icône Windows située en bas à gauche).

2. Créez un répertoire pour votre projet : `mkdir python-scripts`, puis ouvrez ce répertoire : `cd python-scripts`.

3. Créez quelques répertoires à utiliser avec notre exemple de script :

    ```powershell
    mkdir food, food\fruits, food\fruits\apples, food\fruits\oranges, food\vegetables
    ```

4. Créez quelques fichiers dans ces répertoires à utiliser avec notre script :

    ```powershell
    new-item food\fruits\banana.txt, food\fruits\strawberry.txt, food\fruits\blueberry.txt, food\fruits\apples\honeycrisp.txt, food\fruits\oranges\mandarin.txt, food\vegetables\carrot.txt
    ```

5. Créez un nouveau fichier python dans votre répertoire python-scripts :

    ```powershell
    mkdir src
    new-item src\list-directory-contents.py
    ```

6. Ouvrez votre projet dans VS Code en entrant : `code .`

7. Ouvrez la fenêtre de l’explorateur de fichiers VS Code en entrant **Ctrl + Maj + E** (ou utilisez le menu pour accéder à **Afficher** > **Explorer**) et sélectionnez le fichier list-directory-contents.py que vous venez de créer. L’extension Python Microsoft chargera automatiquement un interpréteur Python. Vous pouvez voir quel interpréteur a été chargé en bas de votre fenêtre VS Code.

    > [!NOTE]
    > Python est un langage interprété, ce qui signifie qu’il joue le rôle de machine virtuelle en émulant un ordinateur physique. Vous pouvez utiliser différents types d’interpréteurs Python : Python 2, Python 3, Anaconda, PyPy, etc. Pour exécuter du code Python et obtenir Python IntelliSense, vous devez indiquer à VS Code l’interpréteur à utiliser. Nous vous recommandons d’utiliser l’interpréteur que VS Code choisit par défaut (Python 3 dans notre cas), à moins que vous n’ayez une raison particulière de choisir un autre nom. Pour modifier l’interpréteur Python, sélectionnez l’interpréteur actuellement affiché dans la barre bleue au bas de la fenêtre VS Code ou ouvrez la **palette de commandes** (Ctrl + Maj + P) et saisissez la commande **Python : Sélectionnez l’interpréteur**. Cette opération affiche une liste des interpréteurs Python que vous avez installés actuellement. [En savoir plus sur la configuration des environnements Python](https://code.visualstudio.com/docs/python/environments).

    ![Sélectionner l’interpréteur Python dans VS Code](../images/interpreterselection.gif)

8. Collez le code suivant dans votre fichier list-directory-contents.py, puis sélectionnez **Enregistrer** :

    ```python
    import os

    root = os.path.join('..', 'food')
    for directory, subdir_list, file_list in os.walk(root):
        print('Directory:', directory)
        for name in subdir_list:
            print('Subdirectory:', name)
        for name in file_list:
            print('File:', name)
        print()
    ```

9. Ouvrez le terminal intégré à VS Code (**Ctrl + ’** , à l’aide de l’accent grave) et entrez le répertoire src dans lequel vous venez d’enregistrer votre script Python :

    ```powershell
    cd src
    ```

10. Exécutez le script dans PowerShell avec :

    ```powershell
    python3 .\list-directory-contents.py
    ```

    Vous devez normalement voir une sortie similaire à celle-ci :

    ```powershell
    Directory: ..\food
    Subdirectory: fruits
    Subdirectory: vegetables

    Directory: ..\food\fruits
    Subdirectory: apples
    Subdirectory: oranges
    File: banana.txt
    File: blueberry.txt
    File: strawberry.txt

    Directory: ..\food\fruits\apples
    File: honeycrisp.txt

    Directory: ..\food\fruits\oranges
    File: mandarin.txt

    Directory: ..\food\vegetables
    File: carrot.txt
    ```

11. Utilisez Python pour imprimer la sortie de répertoire du système de fichiers dans son propre fichier texte en entrant cette commande directement dans votre terminal PowerShell : `python3 list-directory-contents.py > food-directory.txt`

Félicitations ! Vous venez d’écrire un script d’administration de systèmes automatisé qui lit le répertoire et les fichiers que vous avez créés et utilise Python pour afficher, puis imprimer, la structure de répertoires dans son propre fichier texte.

## <a name="example-script-to-modify-all-files-in-a-directory"></a>Exemple de script permettant de modifier tous les fichiers d’un répertoire

Cet exemple utilise les fichiers et répertoires que vous venez de créer, en renommant chacun des fichiers en ajoutant la date de dernière modification du fichier au début du nom de fichier.

1. Dans le dossier **src** de votre répertoire **python-scripts**, créez un fichier Python pour votre script :

    ```powershell
    new-item update-filenames.py
    ```

2. Ouvrez le fichier update-filenames.py, collez le code suivant dans le fichier, puis enregistrez-le :

    > [!NOTE]
    > os.getmtime renvoie un horodatage en graduations, qui n’est pas facilement lisible. Il doit d’abord être converti en chaîne dateTime standard.

    ```python
    import datetime
    import os

    root = os.path.join('..', 'food')
    for directory, subdir_list, file_list in os.walk(root):
        for name in file_list:
            source_name = os.path.join(directory, name)
            timestamp = os.path.getmtime(source_name)
            modified_date = str(datetime.datetime.fromtimestamp(timestamp)).replace(':', '.')
            target_name = os.path.join(directory, f'{modified_date}_{name}')

            print(f'Renaming: {source_name} to: {target_name}')

            os.rename(source_name, target_name)
    ```

3. Testez votre script update-filenames.py en l’exécutant : `python3 update-filenames.py`. Puis, exécutez à nouveau votre script list-directory-contents.py : `python3 list-directory-contents.py`

4. Vous devez normalement voir une sortie similaire à celle-ci :

    ```powershell
    Renaming: ..\food\fruits\banana.txt to: ..\food\fruits\2019-07-18 12.24.46.385185_banana.txt
    Renaming: ..\food\fruits\blueberry.txt to: ..\food\fruits\2019-07-18 12.24.46.391170_blueberry.txt
    Renaming: ..\food\fruits\strawberry.txt to: ..\food\fruits\2019-07-18 12.24.46.389174_strawberry.txt
    Renaming: ..\food\fruits\apples\honeycrisp.txt to: ..\food\fruits\apples\2019-07-18 12.24.46.395160_honeycrisp.txt
    Renaming: ..\food\fruits\oranges\mandarin.txt to: ..\food\fruits\oranges\2019-07-18 12.24.46.398151_mandarin.txt
    Renaming: ..\food\vegetables\carrot.txt to: ..\food\vegetables\2019-07-18 12.24.46.402496_carrot.txt

    PS C:\src\python-scripting\src> python3 .\list-directory-contents.py
    ..\food\
    Directory: ..\food
    Subdirectory: fruits
    Subdirectory: vegetables

    Directory: ..\food\fruits
    Subdirectory: apples
    Subdirectory: oranges
    File: 2019-07-18 12.24.46.385185_banana.txt
    File: 2019-07-18 12.24.46.389174_strawberry.txt
    File: 2019-07-18 12.24.46.391170_blueberry.txt

    Directory: ..\food\fruits\apples
    File: 2019-07-18 12.24.46.395160_honeycrisp.txt

    Directory: ..\food\fruits\oranges
    File: 2019-07-18 12.24.46.398151_mandarin.txt

    Directory: ..\food\vegetables
    File: 2019-07-18 12.24.46.402496_carrot.txt

    ```

5. Utilisez Python pour imprimer les noms de répertoire du système de fichiers contenant le préfixe d’horodatage de dernière modification dans son propre fichier texte en entrant cette commande directement dans votre terminal PowerShell : `python3 list-directory-contents.py > food-directory-last-modified.txt`

Nous espérons que vous avez pris du plaisir à découvrir l’utilisation de scripts Python pour automatiser les tâches d’administration des systèmes de base. Bien sûr tout cela n’est qu’un bref aperçu de toutes les choses à savoir, mais vous permet de bien démarrer. Nous avons partagé quelques ressources supplémentaires afin de vous permettre d’en savoir plus.

## <a name="additional-resources"></a>Ressources supplémentaires

- [Documents Python : Accès aux fichiers et aux répertoires](https://docs.python.org/3.7/library/filesys.html) : Documentation Python sur l’utilisation des systèmes de fichiers et l’utilisation de modules pour lire les propriétés des fichiers, manipuler des chemins d’accès de manière portable et créer des fichiers temporaires.
- [Découvrez Python : tutoriel String_Formatting](https://www.learnpython.org/en/String_Formatting) : En savoir plus sur l’utilisation de l’opérateur « % » pour la mise en forme des chaînes.
- [Les 10 méthodes du système de fichiers Python à connaître](https://towardsdatascience.com/10-python-file-system-methods-you-should-know-799f90ef13c2) : Article de taille moyenne sur la manipulation de fichiers et de dossiers avec `os` et `shutil`.
- [Guide Hitchhikers pour Python : Administration de systèmes](https://docs.python-guide.org/scenarios/admin/) : Un « guide de consignes strictes » proposant des vues d’ensemble et des meilleures pratiques sur les sujets relatifs à Python. Cette section traite des outils et infrastructures d’administration système. Ce guide est hébergé sur GitHub pour vous permettre de créer des fichiers et de faire des contributions.
