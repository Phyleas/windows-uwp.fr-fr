---
title: Prise en main de Python sur Windows pour les débutants
description: Un guide pour vous aider à démarrer si vous êtes novice dans l’utilisation de Python sur Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, microsoft, learning python, python sur windows pour les débutants, installer python avec le microsoft store, python avec vs code, pygame sur windows
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 688ae004dad8653e70d86b3b91652b6898c1e9d3
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "74881286"
---
# <a name="get-started-using-python-on-windows-for-beginners"></a>Prise en main de Python sur Windows pour les débutants

Voici un guide pas à pas pour les débutants qui souhaitent découvrir Python avec Windows 10.

## <a name="set-up-your-development-environment"></a>Configurer votre environnement de développement

Pour les débutants qui découvrent Python, nous vous recommandons d’[installer Python à partir du Microsoft Store](https://www.microsoft.com/p/python-37/9nj46sx7x90p?activetab=pivot:overviewtab). L’installation via le Microsoft Store utilise l’interpréteur de base Python3, mais gère la configuration de vos paramètres PATH pour l’utilisateur actuel (évitant ainsi le recours à un accès administrateur), en plus de fournir des mises à jour automatiques. Elle s’avère particulièrement utile si vous êtes dans un environnement éducatif ou dans une organisation qui restreint les autorisations ou l’accès administratif sur votre ordinateur.

Si vous utilisez Python sur Windows pour le **développement web**, nous vous recommandons d’utiliser une configuration différente pour votre environnement de développement. Au lieu d’installer Python directement sur Windows, nous vous recommandons de l’installer et de l’utiliser via le sous-système Windows pour Linux. Pour obtenir de l’aide, consultez : [Bien démarrer avec Python pour le développement web sur Windows](./web-frameworks.md). Si vous souhaitez automatiser des tâches courantes sur votre système d’exploitation, consultez notre guide : [Bien démarrer avec Python pour l’écriture de scripts et l’automatisation sur Windows](./scripting.md). Pour certains scénarios avancés (nécessité d’accéder aux fichiers installés de Python ou de les modifier, de créer des copies de fichiers binaires ou d’utiliser directement des DLL Python), vous pouvez télécharger une version de Python spécifique directement depuis [python.org](https://www.python.org/downloads/) ou installer une [autre solution](https://www.python.org/download/alternatives), telle que Anaconda, Jython, PyPy, WinPython, IronPython, etc. Nous vous recommandons de le faire uniquement si vous êtes un programmeur Python plus avancé possédant une raison spécifique de choisir un autre type d’implémentation.

## <a name="install-python"></a>Installer Python

Pour installer Python à partir du Microsoft Store :

1. Accédez à votre menu **Démarrer** (icône Windows en bas à gauche), tapez « Microsoft Store », puis cliquez sur le lien pour ouvrir le Store.

2. Une fois le Store ouvert, sélectionnez **Rechercher** dans le menu supérieur droit, puis entrez « Python ». Ouvrez « Python 3.7 » dans les résultats sous Applications. Sélectionnez **Obtenir**.

3. Une fois que Python a terminé le processus de téléchargement et d’installation, ouvrez Windows PowerShell à l’aide du menu **Démarrer** (icône Windows en bas à gauche). Une fois PowerShell ouvert, entrez `Python --version` pour vérifier que Python3 est installé sur votre ordinateur.

4. L’installation Microsoft Store de Python comprend **pip**, le gestionnaire de package standard. pip vous permet d’installer et de gérer des packages supplémentaires qui ne font pas partie de la bibliothèque standard Python. Pour confirmer que pip est également disponible pour installer et gérer des packages, entrez `pip --version`.

## <a name="install-visual-studio-code"></a>Installation de Visual Studio Code

En utilisant VS Code comme éditeur de texte/environnement de développement intégré (IDE), vous pouvez tirer parti d’[IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) (aide pour l’exécution du code), de [Linting](https://code.visualstudio.com/docs/python/linting) (permet d’éviter les erreurs dans votre code), de la [prise en charge du débogage](https://code.visualstudio.com/docs/python/debugging) (aide à trouver les erreurs dans votre code après l’avoir exécuté), des [extraits de code](https://code.visualstudio.com/docs/editor/userdefinedsnippets) (modèles pour les petits blocs de code réutilisables) et des [tests unitaires](https://code.visualstudio.com/docs/python/unit-testing) (tests de l’interface du code avec différents types d’entrée).

VS Code contient également un [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal) qui vous permet d’ouvrir une ligne de commande Python à l’aide de l’invite de commandes Windows, de PowerShell ou autre, selon votre préférence, en établissant un flux de travail homogène entre votre éditeur de code et la ligne de commande.

1. Pour installer VS Code, téléchargez VS Code pour Windows : [https://code.visualstudio.com](https://code.visualstudio.com).

2. Une fois VS Code installé, vous devez également installer l’extension Python. Pour installer l’extension Python, vous pouvez sélectionner le [lien VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-python.python) ou ouvrir VS Code et rechercher **Python** dans le menu d’extensions (Ctrl+Maj+X).

3. Python est un langage interprété. Pour exécuter du code Python, vous devez indiquer à VS Code l’interpréteur à utiliser. Nous vous recommandons d’utiliser Python 3.7, sauf si vous avez une raison particulière de choisir un autre interpréteur. Une fois que vous avez installé l’extension Python, sélectionnez un interpréteur Python 3 en ouvrant la **palette de commandes** (Ctrl+Maj+P) et commencez à taper la commande **Python : Sélectionner un interpréteur** pour rechercher, puis sélectionnez la commande. Vous pouvez également utiliser l’option **Sélectionner un environnement Python** dans la barre d’état en bas si elle est disponible (un interpréteur peut déjà être sélectionné). La commande présente la liste des interpréteurs disponibles que VS Code peut trouver automatiquement, y compris les environnements virtuels. Si vous ne voyez pas l’interpréteur souhaité, consultez [Configuration des environnements Python](https://code.visualstudio.com/docs/python/environments).

    ![Sélectionner l’interpréteur Python dans VS Code](../images/interpreterselection.gif)

4. Pour ouvrir le terminal dans VS Code, sélectionnez **Afficher** > **Terminal** ou utilisez le raccourci **Ctrl+`** (à l’aide du guillemet inversé). Le terminal par défaut est PowerShell.

5. À l’intérieur de votre terminal VS Code, ouvrez Python en entrant simplement la commande : `python`

6. Essayez l’interpréteur Python en entrant : `print("Hello World")`. Python renverra l’instruction « Hello World ».

    ![Ligne de commande Python dans VS Code](../images/python-in-vscode.png)

## <a name="install-git-optional"></a>Installer Git (facultatif)

Si vous envisagez de collaborer avec d’autres personnes sur votre code Python ou d’héberger votre projet sur un site open source (comme GitHub), VS Code prend en charge le [contrôle de version avec Git](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support). L’onglet Contrôle de code source de VS Code assure le suivi de toutes vos modifications et contient des commandes Git courantes (ajouter, valider, pousser, extraire) intégrées directement dans l’interface utilisateur. Vous devez d’abord installer Git pour gérer le panneau Contrôle de code source.

1. Téléchargez et installez Git pour Windows sur le [site web git-scm](https://git-scm.com/download/win).

2. Vous trouverez un assistant d’installation qui vous posera une série de questions sur les paramètres de votre installation Git. Nous vous recommandons d’utiliser tous les paramètres par défaut, sauf si vous avez une raison particulière de les modifier.

3. Si vous n’avez jamais travaillé avec Git, les [guides GitHub](https://guides.github.com/) peuvent vous aider à démarrer.

## <a name="hello-world-tutorial-for-some-python-basics"></a>Tutoriel Hello World concernant certaines notions de base Python

Python, selon son créateur Guido van Rossum, est un « langage de programmation de haut niveau, et sa philosophie de conception principale concerne la lisibilité du code et une syntaxe qui permet aux programmeurs d’exprimer des concepts avec quelques lignes de code. »

Python est un langage interprété. Contrairement aux langages compilés, dans lesquels le code que vous écrivez doit être traduit en code machine afin d’être exécuté par le processeur de votre ordinateur, le code Python est transmis à un interpréteur et s’exécute directement. Il vous suffit de taper votre code et de l’exécuter. Essayons !

1. Une fois votre ligne de commande PowerShell ouverte, entrez `python` pour exécuter l’interpréteur Python 3. (Certaines instructions préfèrent utiliser la commande `py` ou `python3`, elles fonctionnent également). Vous saurez que vous aurez réussi lorsqu’une invite >>> avec trois symboles supérieurs s’affichera.

2. Il existe plusieurs méthodes intégrées qui vous permettent d’apporter des modifications aux chaînes dans Python. Création d’une variable, avec : `variable = 'Hello World!'`. Appuyez sur Entrée pour créer une nouvelle ligne.

3. Imprimez votre variable avec : `print(variable)`. Le texte « Hello World ! » s’affiche.

4. Découvrez la longueur et le nombre de caractères utilisés de votre variable de chaîne avec : `len(variable)`. L’invite doit indique que 12 caractères sont utilisés. (Notez que l’espace blanc est compté comme un caractère dans la longueur totale.)

5. Convertissez votre variable de chaîne en lettres majuscules : `variable.upper()`. Convertissez maintenant votre variable de chaîne en lettres minuscules : `variable.lower()`.

6. Comptez le nombre de fois que la lettre «I» est utilisée dans votre variable de chaîne : `variable.count("l")`.

7. Recherchez un caractère spécifique dans votre variable de chaîne, par exemple, le point d’exclamation, avec : `variable.find("!")`. L’invite indique que le point d’exclamation correspond au caractère en onzième position dans la chaîne.

8. Remplacez le point d’exclamation par un point d’interrogation : `variable.replace("!", "?")`.

9. Pour quitter Python, vous pouvez entrer `exit()`, `quit()` ou sélectionner Ctrl-Z.

![Capture d’écran PowerShell de ce tutoriel](../images/hello-world-basics.png)

Nous espérons que vous avez apprécié utiliser certaines des méthodes de modification de chaîne intégrées de Python. Essayez maintenant de créer un fichier programme Python et de l’exécuter avec VS Code.

## <a name="hello-world-tutorial-for-using-python-with-vs-code"></a>Tutoriel Hello World pour l’utilisation de Python avec VS Code

L’équipe VS Code a réalisé un excellent tutoriel de [Prise en main avec Python](https://code.visualstudio.com/docs/python/python-tutorial#_start-vs-code-in-a-project-workspace-folder) qui aborde la création d’un programme Hello World avec Python, l’exécution du fichier programme, la configuration et l’exécution du débogueur et l’installation de packages tels que *matplotlib* et *numpy* pour créer un diagramme dans un environnement virtuel.

1. Ouvrez PowerShell et créez un dossier vide appelé « hello », accédez à ce dossier et ouvrez-le dans VS Code :

    ```console
    mkdir hello
    cd hello
    code .
    ```

2. Une fois VS Code ouvert, affichant votre nouveau dossier *hello* dans la fenêtre **Explorateur** à gauche, ouvrez une fenêtre de ligne de commande dans le panneau inférieur de VS Code en appuyant sur **Ctrl+`** (à l’aide du guillemet inversé) ou en sélectionnant **Affichage** > **Terminal**. En démarrant VS Code dans un dossier, ce dossier devient votre « espace de travail ». VS Code stocke les paramètres spécifiques à cet espace de travail à l’emplacement .vscode/settings.json. Ils sont distincts des paramètres utilisateur stockés globalement.

3. Poursuivez le tutoriel dans la documentation VS Code : [Créez un fichier de code source Python Hello World](https://code.visualstudio.com/docs/python/python-tutorial#_create-a-python-hello-world-source-code-file).

## <a name="create-a-simple-game-with-pygame"></a>Créer un jeu simple avec Pygame

![Pygame exécutant un exemple de jeu](../images/pygame-shmup.jpg)

Pygame est un package Python populaire pour l’écriture de jeux. Il encourage les étudiants à apprendre la programmation tout en créant des choses amusantes. Pygame affiche les graphiques dans une nouvelle fenêtre. Il ne fonctionne donc pas avec l’approche de ligne de commande seule de WSL. Toutefois, si vous avez installé Python via le Microsoft Store comme indiqué dans ce tutoriel, il fonctionnera correctement.

1. Une fois que vous avez installé Python, installez Pygame à partir de la ligne de commande (ou du terminal dans VS Code) en tapant `python -m pip install -U pygame --user`.

2. Testez l’installation en exécutant un exemple de jeu : `python -m pygame.examples.aliens`

3. Si tout se passe bien, le jeu ouvre une fenêtre. Lorsque vous avez terminé, fermez la fenêtre.

Voici comment démarrer l’écriture de votre propre jeu.

1. Ouvrez PowerShell (ou l’invite de commandes Windows) et créez un dossier vide appelé « bounce ». Accédez à ce dossier et créez un fichier nommé « bounce.py ». Ouvrez le dossier dans VS Code :

    ```powershell
    mkdir bounce
    cd bounce
    new-item bounce.py
    code .
    ```

2. À l’aide de VS Code, entrez le code Python suivant (ou copiez-collez-le) :

    ```python
    import sys, pygame

    pygame.init()

    size = width, height = 640, 480
    dx = 1
    dy = 1
    x= 163
    y = 120
    black = (0,0,0)
    white = (255,255,255)

    screen = pygame.display.set_mode(size)

    while 1:

        for event in pygame.event.get():
            if event.type == pygame.QUIT: sys.exit()

        x += dx
        y += dy

        if x < 0 or x > width:   
            dx = -dx

        if y < 0 or y > height:
            dy = -dy

        screen.fill(black)

        pygame.draw.circle(screen, white, (x,y), 8)

        pygame.display.flip()
    ```

3. Enregistrez-le sous : `bounce.py`.

4. À partir du terminal PowerShell, exécutez-le en entrant : `python bounce.py`.

    ![Pygame exécutant la prochaine nouveauté](../images/pygame.jpg)

Essayez de modifier certains nombres pour voir l’effet qu’ils ont sur votre boule rebondissante.

Pour en savoir plus sur l’écriture de jeux avec Pygame, consultez [pygame.org](http://www.pygame.org).

## <a name="resources-for-continued-learning"></a>Ressources pour l’apprentissage continu

Nous vous recommandons de consulter les ressources suivantes pour vous familiariser avec le développement Python sur Windows.

### <a name="online-courses-for-learning-python"></a>Cours en ligne pour apprendre à utiliser Python

- [Présentation de Python sur Microsoft Learn](https://docs.microsoft.com/learn/modules/intro-to-python/) : essayez la plateforme interactive Microsoft Learn et gagnez des points d’expérience en complétant ce module, qui couvre les notions de base concernant l’écriture de code Python de base, la déclaration de variables et l’utilisation des entrées et sorties de la console. L’environnement de bac à sable interactif permet aux personnes dont l’environnement de développement Python n’est pas encore installé de démarrer facilement.

- [Python sur Pluralsight : 8 cours, 29 heures](https://app.pluralsight.com/paths/skills/python) : le parcours d’apprentissage Python sur Pluralsight offre des cours en ligne couvrant une variété de sujets liés à Python, notamment un outil permettant de mesurer vos compétences et de trouver vos lacunes.

- [Didacticiels LearnPython.org](https://www.learnpython.org/) : apprenez à utiliser Python sans installer ou définir d’autres éléments à l’aide de ces tutoriels Python interactifs gratuits, réalisés par DataCamp.

- [Didacticiels Python.org](https://docs.python.org/3/tutorial/index.html) : présente au lecteur les concepts et fonctionnalités de base du langage et du système Python, de manière informelle.

- [Découvrir Python sur Lynda.com](https://www.lynda.com/Python-tutorials/Learning-Python/661773-2.html) : Une introduction de base Python.

### <a name="working-with-python-in-vs-code"></a>Utilisation de Python dans VS Code

- [Modification du code Python dans VS Code](https://code.visualstudio.com/docs/python/editing) : Découvrez comment tirer parti de la complétion automatique de VS Code et de la prise en charge d’IntelliSense pour Python, comment personnaliser leurs comportements ou simplement les désactiver.

- [Linting Python](https://code.visualstudio.com/docs/python/linting) : Le linting (vérification) est le processus qui consiste à exécuter un programme qui analysera le code pour identifier les erreurs potentielles. En savoir plus sur les différentes formes de prise en charge du linting fournies par VS Code pour Python et comment les configurer.

- [Débogage de Python](https://code.visualstudio.com/docs/python/debugging) : Le débogage est le processus qui consiste à identifier et à supprimer les erreurs d’un programme informatique. Cet article explique comment initialiser et configurer le débogage de Python avec VS Code, comment définir et valider des points d’arrêt, comment joindre un script local, effectuer un débogage pour différents types d’applications ou sur un ordinateur distant, et donne des conseils de base relatifs à la résolution des problèmes.

- [Effectuer des tests unitaires sur Python](https://code.visualstudio.com/docs/python/unit-testing) : Fournit des explications sur le fonctionnement des tests unitaires, un exemple de procédure pas à pas, l’activation d’une infrastructure de test, la création et l’exécution de vos tests, le débogage des tests et les paramètres de configuration de test. 
