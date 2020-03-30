---
title: Développement web via Python sur Windows
description: Comment prendre en main Python pour le développement web sur Windows, y compris pour la configuration d’infrastructures comme Flask et Django.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, microsoft, python sur windows, application web python avec wsl, application web python avec un sous-système windows pour linux, développement web python sur windows, application flask sur windows, application django sur windows, application web python, développement web flask sur windows, développement web django sur windows, développement web windows avec python et développement web de code python, extension wsl-remote, ubuntu, wsl, venv, pip, extension python microsoft, exécuter python sur windows, utiliser python sur windows, création avec python sur windows
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 8cbc8343764e4de57bd418ecdb36bd606b037c68
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218479"
---
# <a name="get-started-using-python-for-web-development-on-windows"></a>Bien démarrer avec Python pour le développement web sur Windows

Vous trouverez ci-dessous un guide pas à pas pour vous familiariser avec l’utilisation de Python pour le développement Web sur Windows, à l’aide du sous-système Windows pour Linux (WSL).

## <a name="set-up-your-development-environment"></a>Configurer votre environnement de développement

Nous vous recommandons d’installer Python sur WSL lors de la création d’applications web. Un grand nombre des didacticiels et instructions relatifs au développement web Python sont écrits pour les utilisateurs Linux et reposent sur des outils d’installation et d’empaquetage basés sur Linux. La plupart des applications web sont également déployées sur Linux, ce qui permet de garantir la cohérence entre vos environnements de développement et de production.

Si vous utilisez Python pour une autre fin que le développement web, nous vous recommandons d’installer Python directement sur Windows 10 à l’aide du Microsoft Store. WSL ne prend pas en charge les applications ou les ordinateurs de bureau GUI (par exemple PyGame, GNOME, KDE, etc.) Dans ce cas, installez et utilisez Python directement sur Windows. Si vous ne maîtrisez pas Python, consultez notre guide : [Prise en main de Python sur Windows pour les débutants](./beginners.md). Si vous souhaitez automatiser des tâches courantes sur votre système d’exploitation, consultez notre guide : [Bien démarrer avec Python pour l’écriture de scripts et l’automatisation sur Windows](./scripting.md). Pour certains scénarios avancés, vous pouvez télécharger une version de Python spécifique directement depuis [python.org](https://www.python.org/downloads/windows/) ou installer une [autre solution](https://www.python.org/download/alternatives), telle qu’Anaconda, Jython, PyPy, WinPython, IronPython, etc. Nous vous recommandons de le faire uniquement si vous êtes un programmeur Python plus avancé, qui a une raison spécifique de choisir un autre type d’implémentation.

## <a name="enable-windows-subsystem-for-linux"></a>Activer le sous-système Windows pour Linux

WSL permet d’exécuter directement sur Windows un environnement GNU/Linux comprenant la plupart des outils en ligne de commande, utilitaires et applications, tels quels et entièrement intégrés au système de fichiers Windows et aux outils habituels comme Visual Studio Code. Avant d’activer WSL, vérifiez que vous disposez de la [version la plus récente de Windows 10](https://www.microsoft.com/software-download/windows10).

Pour activer WSL sur votre ordinateur, vous devez procéder comme suit :

1. Accédez à votre menu **Démarrer** (icône Windows en bas à gauche), tapez « Activer ou désactiver des fonctionnalités Windows », puis sélectionnez le lien vers le **Panneau de configuration** pour ouvrir la fenêtre contextuelle des **fonctionnalités Windows**. Recherchez « Sous-système Windows pour Linux » dans la liste et cochez la case correspondante pour activer la fonctionnalité.

2. Redémarrez votre ordinateur lorsque vous y êtes invité.

## <a name="install-a-linux-distribution"></a>Installer une distribution Linux

Plusieurs distributions Linux peuvent s’exécuter sur WSL. Vous trouverez votre distribution préférée dans le Microsoft Store. Nous vous recommandons de commencer par la distribution [Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q) actuelle, bien connue et prise en charge.

1. Ouvrez le lien [Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q), ouvrez le Microsoft Store, puis sélectionnez **Obtenir**. *(Il s’agit d’un téléchargement assez volumineux, dont l’installation peut prendre un certain temps.)*

2. Une fois le téléchargement terminé, sélectionnez **Lancer** à partir du Microsoft Store ou tapant « Ubuntu 18.04 LTS » dans le menu **Démarrer** pour lancer l’application.

3. Vous êtes invité à créer un nom de compte et un mot de passe lors de la première exécution d’une distribution. Après cela, vous serez automatiquement connecté comme l’utilisateur par défaut ainsi défini. Vous pouvez choisir n’importe quels noms d’utilisateur et mot de passe. Ils sont sans lien avec votre nom d’utilisateur Windows.

Vous pouvez identifier la distribution Linux que vous utilisez actuellement en saisissant ce qui suit : `lsb_release -d`. Pour mettre à jour votre distribution Ubuntu, utilisez : `sudo apt update && sudo apt upgrade`. Nous vous recommandons de procéder à une mise à jour régulière, afin de vous assurer que vous disposez des packages les plus récents. Windows ne gère pas automatiquement cette mise à jour. Pour obtenir des liens vers d’autres distributions Linux disponibles dans le Microsoft Store, ainsi que d’autres méthodes d’installation ou procédures de résolution des problèmes, voir [Guide d’installation du sous-système Windows pour Linux pour Windows 10](https://docs.microsoft.com/windows/wsl/install-win10).

> [!TIP]
> Essayez le nouveau [terminal Windows](https://github.com/microsoft/terminal/blob/master/doc/user-docs/index.md) si vous prévoyez d’utiliser plusieurs lignes de commande (Ubuntu, PowerShell, invite de commandes Windows, etc.) ou si vous souhaitez [personnaliser votre terminal](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md) (texte, couleurs d’arrière-plan, combinaisons de touches, etc.).

## <a name="set-up-visual-studio-code"></a>Configurer Visual Studio Code

Tirez parti de [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense), [la vérification (linting)](https://code.visualstudio.com/docs/python/linting), [la prise en charge du débogage](https://code.visualstudio.com/docs/python/debugging), des [extraits de code](https://code.visualstudio.com/docs/editor/userdefinedsnippets) et des [tests unitaires](https://code.visualstudio.com/docs/python/unit-testing) à l’aide de VS Code. VS Code s’intègre parfaitement dans le sous-système Windows pour Linux, en fournissant un [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal) pour établir un flux de travail transparent entre votre éditeur de code et votre ligne de commande, en plus de prendre en charge [Git pour le contrôle de version](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support) avec les commandes git courantes (ajout, validation, push, extraction) intégrées directement dans l’interface utilisateur.

1. [Téléchargez et installez VS Code pour Windows](https://code.visualstudio.com). VS Code est également disponible pour Linux, mais le sous-système Windows pour Linux ne prend pas en charge les applications GUI. Nous devons donc l’installer sur Windows. Ne vous inquiétez pas, vous serez toujours en mesure de l’intégrer dans vos outils et dans la ligne de commande Linux à l’aide de l’extension WSL-Remote.

2. Installez [l’extension WSL-Remote](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) sur VS Code. Cela vous permet d’utiliser WSL comme environnement de développement intégré et de gérer la compatibilité et les chemins d’accès pour vous. [En savoir plus](https://code.visualstudio.com/docs/remote/remote-overview)

> [!IMPORTANT]
> Si vous avez déjà installé VS Code, vous devez vous assurer que vous disposez de la version [1.35 de mai](https://code.visualstudio.com/updates/v1_35) ou plus pour installer [l’extension WSL-Remote](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl). Nous vous déconseillons d’utiliser WSL dans VS Code sans l’extension WSL-Remote, car vous perdrez la prise en charge de la saisie semi-automatique, du débogage, de la vérification (linting), etc. Le saviez-vous ? Cette extension WSL est installée dans le répertoire $HOME/.vscode-server/extensions.

## <a name="create-a-new-project"></a>Create a new project (Créer un projet)

Nous allons créer un nouveau répertoire de projet sur notre système de fichiers Linux (Ubuntu), sur lequel nous travaillerons avec les applications et les outils Linux à l’aide de VS Code.

1. Fermez VS Code et ouvrez Ubuntu 18.04 (votre ligne de commande WSL) en accédant à votre menu **Démarrer** (icône Windows en bas à gauche) et en saisissant ce qui suit : « Ubuntu 18.04 ».

2. Dans votre ligne de commande Ubuntu, accédez à l’emplacement auquel placer votre projet et créez un répertoire pour celui-ci : `mkdir HelloWorld`.

![Terminal Ubuntu](../images/ubuntu-terminal.png)

> [!TIP]
> Ce qu’il faut retenir lors de l’utilisation du sous-système Windows pour Linux (WSL), c’est que **vous utilisez maintenant deux systèmes de fichiers différents** : 1) Votre système de fichiers Windows et 2) votre système de fichiers Linux (WSL), soit Ubuntu pour notre exemple. Vous devez faire attention à l’emplacement d’installation des packages et des fichiers de stockage. Vous pouvez installer une version d’un outil ou d’un package dans le système de fichiers Windows, et une autre version dans le système de fichiers Linux. La mise à jour de l’outil dans le système de fichiers Windows n’a aucun effet sur l’outil dans le système de fichiers Linux, et réciproquement. WSL monte les lecteurs fixes sur votre ordinateur dans le dossier `/mnt/<drive>` de votre distribution Linux. Par exemple, votre lecteur C: Windows est monté sous `/mnt/c/`. Vous pouvez accéder à vos fichiers Windows à partir du terminal Ubuntu et utiliser des applications et des outils Linux sur ces fichiers, et vice-versa. Nous vous recommandons de travailler dans le système de fichiers Linux pour le développement web Python, étant donné que la plupart des outils web sont écrits à l’origine pour Linux et déployés dans un environnement de production Linux. Il évite également de mélanger la sémantique du système de fichiers (comme le fait que Windows ne respecte pas la casse dans les noms de fichiers). Cela dit, WSL prend désormais en charge la transition entre les systèmes de fichiers Linux et Windows, ce qui vous permet d’héberger vos fichiers sur l’un ou l’autre. [En savoir plus](https://devblogs.microsoft.com/commandline/do-not-change-linux-files-using-windows-apps-and-tools/) Nous sommes également ravis d’annoncer que [WSL2 sera bientôt disponible pour Windows](https://devblogs.microsoft.com/commandline/wsl-2-is-now-available-in-windows-insiders/) et offrira des améliorations exceptionnelles. Vous pouvez [essayer cette application dès à présent, en vous procurant le build 18917 de Windows Insiders](https://docs.microsoft.com/windows/wsl/wsl2-install).

## <a name="install-python-pip-and-venv"></a>Installer Python, pip et venv

Le logiciel Ubuntu 18.04 LTS est fourni avec Python 3.6 déjà installé, mais il n’est pas fourni avec certains des modules que vous pouvez vous attendre à obtenir avec d’autres installations Python. Nous devons toujours installer **pip**, le gestionnaire de package standard pour Python, et **venv**, le module standard utilisé pour créer et gérer des environnements virtuels légers.  

1. Vérifiez que Python3 est déjà installé en ouvrant votre terminal Ubuntu et en saisissant ce qui suit : `python3 --version`. Le numéro de votre version de Python devrait vous être renvoyé. Si vous devez mettre à jour votre version de Python, commencez par mettre à jour votre version d’Ubuntu en saisissant la chaîne `sudo apt update && sudo apt upgrade`, puis mettez à jour Python à l’aide de `sudo apt upgrade python3`.

2. Installez **pip** en saisissant `sudo apt install python3-pip`. Pip vous permet d’installer et de gérer des packages supplémentaires qui ne font pas partie de la bibliothèque standard Python.

3. Installez **venv** en saisissant la chaîne `sudo apt install python3-venv`.

## <a name="create-a-virtual-environment"></a>Créer un environnement virtuel

L’utilisation d’environnements virtuels est une bonne pratique recommandée pour les projets de développement Python. En créant un environnement virtuel, vous pouvez isoler vos outils de projet et éviter les conflits de contrôle de version avec les outils associés à vos autres projets. Par exemple, vous pouvez conserver un ancien projet web qui requiert l’infrastructure web Django 1.2, mais utiliser Django 2.2 pour un nouveau projet passionnant. Si vous mettez à jour Django globalement, en dehors d’un environnement virtuel, vous risquez de rencontrer des problèmes de contrôle de version plus tard. En plus de prévenir les conflits de contrôle de version accidentels, les environnements virtuels vous permettent d’installer et de gérer des packages sans privilèges administratifs.

1. Ouvrez votre terminal et, à l’intérieur de votre dossier de projet *HelloWorld*, utilisez la commande suivante pour créer un environnement virtuel nommé **.venv** : `python3 -m venv .venv`.

2. Pour activer l’environnement virtuel, entrez la chaîne `source .venv/bin/activate`. Si cela fonctionne, vous devez voir s’afficher **(.venv)** avant l’invite de commandes. Vous disposez maintenant d’un environnement autonome, prêt pour l’écriture de code et l’installation de packages. Lorsque vous avez fini d’utiliser votre environnement virtuel, entrez la commande suivante pour le désactiver : `deactivate`.

    ![Créer un environnement virtuel](../images/wsl-venv.png)

> [!TIP]
> Nous vous recommandons de créer l’environnement virtuel dans le répertoire dans lequel vous prévoyez d’installer votre projet. Étant donné que chaque projet doit avoir son propre répertoire distinct, chacun d’eux aura son propre environnement virtuel ; il n’est donc pas nécessaire de lui attribuer un nom unique. Nous vous suggérons d’utiliser le nom **.venv** pour suivre la convention Python. Certains outils (tels que pipenv) ont également ce nom par défaut si vous les installez dans le répertoire de votre projet. Évitez d’utiliser **.env**, car cet élément entre en conflit avec les fichiers de définition de variable d’environnement. En règle générale, nous ne recommandons pas les noms ne commençant pas par un point, car vous n’avez pas besoin que le paramètre `ls` vous rappelle en permanence que le répertoire existe. Nous vous recommandons également d’ajouter **.venv** à votre fichier .gitignore. (voici le [modèle gitignore par défaut de GitHub pour Python](https://github.com/github/gitignore/blob/50e42aa1064d004a5c99eaa72a2d8054a0d8de55/Python.gitignore#L99-L106), à titre de référence.) Pour en savoir plus sur l’utilisation des environnements virtuels dans VS Code, consultez la section relative à [l’utilisation d’environnements Python dans VS Code](https://code.visualstudio.com/docs/python/environments).

## <a name="open-a-wsl---remote-window"></a>Ouvrir une fenêtre WSL-Remote

VS Code utilise l’extension WSL-Remote (installée précédemment) pour traiter votre sous-système Linux en tant que serveur distant. Cela vous permet d’utiliser WSL en tant qu’environnement de développement intégré. [En savoir plus](https://code.visualstudio.com/docs/remote/wsl) 

1. Ouvrez votre dossier de projet dans VS Code à partir de votre terminal Ubuntu en saisissant la chaîne `code .` (le « . » indique à VS Code d’ouvrir le dossier actif).

2. Une alerte de sécurité s’affiche à partir de Windows Defender, sélectionnez « Autoriser l’accès ». Lorsque VS Code s’ouvre, l’indicateur de l’hôte de connexion à distance doit apparaître dans le coin inférieur gauche, ce qui vous indique que vous êtes en train d’apporter des modifications à un élément sur **WSL: Ubuntu-18.04**.

    ![Indicateur de l’hôte de connexion à distance VS Code](../images/wsl-remote-extension.png)

3. Fermez votre terminal Ubuntu. À partir de maintenant, nous utiliserons le terminal WSL intégré dans VS Code.

4. Ouvrez le terminal WSL dans VS Code en appuyant sur **Ctrl + ’** (en utilisant le caractère d’impulsion) ou en sélectionnant **Afficher** > **Terminal**. Cela affiche une ligne de commande Bash (WSL) ouverte dans le chemin d’accès au dossier du projet que vous avez créé dans votre terminal Ubuntu.

    ![Code VS avec le terminal WSL](../images/vscode-bash-remote.png)

## <a name="install-the-microsoft-python-extension"></a>Installer l’extension Python Microsoft

Vous devez installer les extensions VS Code pour votre instance WSL-Remote. Les extensions déjà installées localement sur VS Code ne seront pas automatiquement disponibles. [En savoir plus](https://code.visualstudio.com/docs/remote/wsl#_managing-extensions)

1. Ouvrez la fenêtre Extensions de VS Code en entrant **Ctrl + Maj + X** (ou utilisez le menu pour accéder à **Afficher** > **Extensions**).

2. Dans la zone **Rechercher des extensions dans Marketplace**, entrez :  **Python**.

3. Trouvez l’extension **Python (ms-python.python) par Microsoft** et sélectionnez le bouton vert **Installer**.

4. Une fois l’installation de l’extension terminée, vous devrez sélectionner le bouton bleu **Reload Required** (Rechargement obligatoire). Cela permet de recharger VS Code et d’afficher une section **WSL : UBUNTU-18.04 - Installé** dans votre fenêtre Extensions VS Code, qui indique que l’extension Python est installée.

## <a name="run-a-simple-python-program"></a>Exécuter un programme Python simple

Python est un langage interprété qui prend en charge différents types d’interpréteurs (Python2, Anaconda, PyPy, etc.). La valeur par défaut de VS Code doit correspondre à l’interpréteur associé à votre projet. Si vous devez le modifier, sélectionnez l’interpréteur actuellement affiché dans la barre bleue au bas de la fenêtre VS Code ou ouvrez la **palette de commandes** (Ctrl + Maj + P) et entrez la commande **Python : Sélectionner l’interpréteur**. Cette opération affiche une liste des interpréteurs Python que vous avez installés actuellement. [En savoir plus sur la configuration des environnements Python](https://code.visualstudio.com/docs/python/environments).

Nous allons créer et exécuter un programme Python simple à titre de test et vérifier que l’interpréteur Python correct est sélectionné.

1. Ouvrez la fenêtre Explorateur de fichiers de VS Code en entrant **Ctrl + Maj + E** (ou utilisez le menu pour accéder à **Afficher** > **Explorateur**).

2. S’il n’est pas déjà ouvert, ouvrez votre terminal WSL intégré en entrant **Ctrl + Maj + ’** et assurez-vous que le dossier de projet Python **HelloWorld** est sélectionné.

3. Créez un fichier Python en saisissant la chaîne `touch test.py`. Le fichier que vous venez de créer s’affiche dans la fenêtre Explorateur, sous les dossiers .venv et .vscode qui se trouvent déjà dans le répertoire de votre projet.

4. Sélectionnez le fichier **test.py** que vous venez de créer dans votre fenêtre Explorateur pour l’ouvrir dans VS Code. Étant donné que la chaîne « .py » figurant dans notre nom de fichier indique à VS Code qu’il s’agit d’un fichier Python, l’extension Python que vous avez chargée précédemment choisit et charge automatiquement un interpréteur Python, qui s’affichera en bas de votre fenêtre VS Code.

    ![Sélectionner l’interpréteur Python dans VS Code](../images/interpreterselection.gif)

5. Collez ce code Python dans votre fichier test.py, puis enregistrez le fichier (Ctrl + S) : 

    ```python
    print("Hello World")
    ```

6. Pour exécuter le programme « Hello World » en Python que nous venons de créer, sélectionnez le fichier **test.py** dans la fenêtre Explorateur de VS Code, puis cliquez avec le bouton droit sur le fichier pour afficher un menu d’options. Sélectionnez **Exécuter le fichier Python dans le terminal**. Vous pouvez également entrer `python test.py` dans votre fenêtre de terminal WSL intégrée pour exécuter votre programme « Hello World ». L’interpréteur Python affiche « Hello World » dans votre fenêtre de terminal.

Félicitations ! Vous êtes désormais prêt à créer et à exécuter des programmes Python ! Essayons à présent de créer une application Hello World avec deux des infrastructures web Python les plus populaires : Flask et Django.

## <a name="hello-world-tutorial-for-flask"></a>Didacticiel Hello World pour Flask

[Flask](http://flask.pocoo.org/) est une infrastructure d’application web pour Python. Dans ce bref didacticiel, vous allez créer une petite application Flask appelée « Hello World », à l’aide de VS Code et WSL.

1. Ouvrez Ubuntu 18.04 (votre ligne de commande WSL) en accédant à votre **menu Démarrer** (icône Windows en bas à gauche) et en saisissant ce qui suit : « Ubuntu 18.04 ».

2. Créez un répertoire pour votre projet : `mkdir HelloWorld-Flask`, puis entrez `cd HelloWorld-Flask` pour accéder au répertoire.

3. Créez un environnement virtuel pour installer les outils de votre projet : `python3 -m venv .venv`

4. Ouvrez votre projet **HelloWorld-Flask** dans VS Code en entrant la commande suivante : `code .`

5. Dans VS Code, ouvrez votre terminal WSL intégré (appelé Bash) en entrant **Ctrl + Maj + ’** (votre dossier de projet **HelloWorld-Flask** doit déjà être sélectionné). *Fermez votre ligne de commande Ubuntu, car nous allons désormais utiliser terminal WSL intégré à VS Code.*

6. Activez l’environnement virtuel que vous avez créé à l’étape #3 à l’aide de votre terminal Bash dans VS Code : `source .venv/bin/activate`. Si cela fonctionne, vous devez voir s’afficher (.venv) avant l’invite de commandes.

7. Installez Flask dans l’environnement virtuel en saisissant ce qui suit : `python3 -m pip install flask`. Vérifiez qu’il est installé en saisissant : `python3 -m flask --version`.

8. Créez un fichier pour votre code Python : `touch app.py`

9. Ouvrez votre fichier **app.py** dans la fenêtre Explorateur de fichiers de VS Code (`Ctrl+Shift+E`, puis sélectionnez votre fichier app.py). L’extension Python est alors activée pour choisir un interpréteur. La valeur par défaut doit être la suivante : **Python 3.6.8 64-bit ('.venv': venv)** . Notez qu’elle a également détecté votre environnement virtuel.

    ![Environnement virtuel activé](../images/virtual-environment.png)

10. Dans **app.py**, ajoutez du code pour importer Flask et créez une instance de l’objet Flask :

    ```python
    from flask import Flask
    app = Flask(__name__)
    ```

11. En outre, dans le fichier **app.py**, ajoutez une fonction qui renvoie du contenu, dans ce cas une chaîne simple. Utilisez l’élément décorateur **app.route** de Flask pour mapper le signe « / » de l’itinéraire de l’URL sur cette fonction :

    ```python
    @app.route("/")
    def home():
        return "Hello World! I'm using Flask."
    ```

    > [!TIP]
    > Vous pouvez utiliser plusieurs éléments décoratifs sur la même fonction, un par ligne, selon le nombre d’itinéraires que vous souhaitez mapper sur la même fonction.

12. Enregistrez le fichier **app.py** (**Ctrl + S**).

13. Dans le terminal, exécutez l’application en entrant la commande suivante :

    ```python
    python3 -m flask run
    ```

    Le serveur de développement Flask est alors exécuté. Ce dernier recherche le fichier **app.py** par défaut. Lorsque vous exécutez Flask, la sortie doit indiquer des informations similaires à celles-ci :

    ```bash
    (env) user@USER:/mnt/c/Projects/HelloWorld$ python3 -m flask run
     * Environment: production
       WARNING: This is a development server. Do not use it in a production deployment.
       Use a production WSGI server instead.
     * Debug mode: off
     * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
    ```

14. Ouvrez votre navigateur web par défaut sur la page rendue, et appuyez sur l’URL de http://127.0.0.1:5000/ dans le terminal, en saisissant **Ctrl + clic**. Votre navigateur doit afficher le message suivant :

    ![Hello, Flask !](../images/hello-flask.png)

15. Notez que lorsque vous visitez une URL du type « / », un message s’affiche dans le terminal de débogage, affichant la requête HTTP :

    ```bash
    127.0.0.1 - - [19/Jun/2019 13:36:56] "GET / HTTP/1.1" 200 -
    ```

16. Arrêtez l’application en appuyant sur **Ctrl + C** dans le terminal.

> [!TIP]
> Si vous souhaitez utiliser un nom de fichier différent de **app.py**, par exemple **program.py**, définissez une variable d’environnement nommée **FLASK_APP** et définissez sa valeur sur le fichier que vous avez choisi. Le serveur de développement de Flask utilise ensuite la valeur de **FLASK_APP** au lieu du fichier **app.py** par défaut. Pour plus d’informations, consultez la [documentation relative à l’interface de ligne de commande de Flask](http://flask.pocoo.org/docs/1.0/cli/).

Félicitations, vous avez créé une application web Flask à l’aide de Visual Studio Code et du sous-système Windows pour Linux ! Pour obtenir un didacticiel plus détaillé sur l’utilisation de VS Code et de Flask, consultez le [didacticiel Flask dans Visual Studio Code](https://code.visualstudio.com/docs/python/tutorial-flask).

## <a name="hello-world-tutorial-for-django"></a>Didacticiel Hello World pour Django

[Django](https://www.djangoproject.com) est une infrastructure d’application web pour Python. Dans ce bref didacticiel, vous allez créer une petite application Django appelée « Hello World », à l’aide de VS Code et de WSL.

1. Ouvrez Ubuntu 18.04 (votre ligne de commande WSL) en accédant à votre **menu Démarrer** (icône Windows en bas à gauche) et en saisissant ce qui suit : « Ubuntu 18.04 ».

2. Créez un répertoire pour votre projet : `mkdir HelloWorld-Django`, puis entrez `cd HelloWorld-Django` pour accéder au répertoire.

3. Créez un environnement virtuel pour installer les outils de votre projet : `python3 -m venv .venv`

4. Ouvrez votre projet **HelloWorld-Django** dans VS Code en entrant la commande suivante : `code .`

5. Dans VS Code, ouvrez votre terminal WSL intégré (appelé Bash) en entrant **Ctrl + Maj + ’** (votre dossier de projet **HelloWorld-Django** doit déjà être sélectionné). *Fermez votre ligne de commande Ubuntu, car nous allons désormais utiliser terminal WSL intégré à VS Code.*

6. Activez l’environnement virtuel que vous avez créé à l’étape #3 à l’aide de votre terminal Bash dans VS Code : `source .venv/bin/activate`. Si cela fonctionne, vous devez voir s’afficher (.venv) avant l’invite de commandes.

7. Installez Django dans l’environnement virtuel à l’aide de la commande : `python3 -m pip install django`. Vérifiez qu’il est installé en saisissant la commande suivante : `python3 -m django --version`.

8. Ensuite, exécutez la commande suivante pour créer le projet Django :

    ```bash
    django-admin startproject web_project .
    ```

    La commande `startproject` suppose (en utilisant `.` à la fin) que le dossier actif est votre dossier de projet, et crée les éléments suivants dans celui-ci :

    - `manage.py` : utilitaire d’administration en ligne de commande Django pour le projet. Vous exécutez les commandes d’administration du projet à l’aide de `python manage.py <command> [options]`.

    - Un sous-dossier nommé `web_project` est créé, qui contient les fichiers suivants :
        - `__init__.py` : un fichier vide qui indique à Python que ce dossier est un package Python.
        - `wsgi.py` : un point d’entrée pour les serveurs web compatibles WSGI afin de traiter votre projet. En règle générale, ce fichier est laissé tel quel, car il fournit les raccordements pour les serveurs web de production.
        - `settings.py` : contient les paramètres du projet Django, que vous modifiez au cours du développement d’une application web.
        - `urls.py` : contient une table des matières pour le projet Django, que vous modifiez également au cours du développement.

9. Pour vérifier le projet Django, démarrez le serveur de développement Django à l’aide de la commande `python3 manage.py runserver`. Le serveur s’exécute sur le port par défaut 8000, et une sortie similaire à ce qui suit doit s’afficher dans la fenêtre de terminal :

    ```output
    Performing system checks...

    System check identified no issues (0 silenced).

    June 20, 2019 - 22:57:59
    Django version 2.2.2, using settings 'web_project.settings'
    Starting development server at http://127.0.0.1:8000/
    Quit the server with CONTROL-C.
    ```

    Lorsque vous exécutez le serveur la première fois, il crée une base de données SQLite par défaut dans le fichier `db.sqlite3`, laquelle est destinée au développement, mais peut être utilisée en production pour les applications web à faible volume. En outre, le serveur web intégré de Django est destiné *uniquement* au développement local. Toutefois, lorsque vous effectuez le déploiement sur un hôte web, Django utilise le serveur web de l’hôte à la place. Le module `wsgi.py` du projet Django prend en charge le raccordement aux serveurs de production.

    Si vous souhaitez utiliser un autre port que le port 8000, spécifiez le numéro de port sur la ligne de commande, par exemple `python3 manage.py runserver 5000`.

10. Appuyez sur `Ctrl+click` sur l’URL `http://127.0.0.1:8000/` dans la fenêtre de sortie du terminal pour ouvrir votre navigateur par défaut à cette adresse. Si Django est installé correctement et que le projet est valide, une page par défaut s’affiche. La fenêtre de sortie du terminal VS Code affiche également le journal du serveur.

11. Lorsque vous avez terminé, fermez la fenêtre du navigateur et arrêtez le serveur dans VS Code à l’aide de la commande `Ctrl+C`, comme indiqué dans la fenêtre sortie de terminal.

12. Maintenant, pour créer une application Django, exécutez la commande `startapp` de l’utilitaire d’administration dans le dossier de votre projet (où se trouve `manage.py`) :

    ```bash
    python3 manage.py startapp hello
    ```

    La commande crée un dossier appelé `hello`, qui contient un certain nombre de fichiers de code et un sous-dossier. Parmi celles-ci, vous travaillez fréquemment avec `views.py` (qui contient les fonctions qui définissent des pages dans votre application web) et `models.py` (qui contient des classes définissant vos objets de données). Le dossier `migrations` est utilisé par l’utilitaire d’administration de Django pour gérer les versions de base de données, comme indiqué plus loin dans ce didacticiel. Il y a également les fichiers `apps.py` (configuration de l’application), `admin.py` (pour la création d’une interface d’administration) et `tests.py` (pour les tests), qui ne sont pas abordés ici.

13. Modifiez l’élément `hello/views.py` pour qu’il corresponde au code suivant, qui crée une vue unique pour la page d’hébergement de l’application :

    ```python
    from django.http import HttpResponse

    def home(request):
        return HttpResponse("Hello, Django!")
    ```

14. Créez un fichier `hello/urls.py` avec le contenu ci-dessous. Le fichier `urls.py` est l’emplacement dans lequel vous spécifiez des modèles pour acheminer différentes URL vers leurs vues appropriées. Le code ci-dessous contient un itinéraire pour mapper l’URL racine de l’application (`""`) vers la fonction `views.home` que vous venez d’ajouter à `hello/views.py` :

    ```python
    from django.urls import path
    from hello import views

    urlpatterns = [
        path("", views.home, name="home"),
    ]
    ```

15. Le dossier `web_project` contient également un fichier `urls.py`, dans lequel le routage d’URL est réellement géré. Ouvrez l’élément `web_project/urls.py` et modifiez-le pour qu’il corresponde au code suivant (vous pouvez conserver les commentaires instructifs si vous le souhaitez). Ce code extrait l’élément `hello/urls.py` de l’application à l’aide de `django.urls.include`, ce qui permet de conserver les itinéraires de l’application dans l’application. Cette séparation est utile lorsqu’un projet contient plusieurs applications.

    ```python
    from django.contrib import admin
    from django.urls import include, path

    urlpatterns = [
        path("", include("hello.urls")),
    ]
    ```

16. Enregistrez tous les fichiers modifiés.

17. Dans le terminal VS Code, exécutez le serveur de développement avec la commande `python3 manage.py runserver` et ouvrez un navigateur pour `http://127.0.0.1:8000/` afficher une page qui affiche « Hello, Django ».

Félicitations, vous avez créé une application web Django à l’aide de Visual Studio Code et du sous-système Windows pour Linux ! Pour obtenir un didacticiel plus détaillé sur l’utilisation de VS Code et de Django, consultez le [didacticiel Django dans Visual Studio Code](https://code.visualstudio.com/docs/python/tutorial-django).

## <a name="additional-resources"></a>Ressources supplémentaires

- [Didacticiel Python avec VS Code](https://code.visualstudio.com/docs/python/python-tutorial) : didacticiel d’introduction à VS Code en tant qu’environnement Python, expliquant principalement comment modifier, exécuter et déboguer du code.
- [Prise en charge de git dans VS Code](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support) : découvrez comment utiliser les concepts de base du contrôle de version git dans VS Code.  
- [En savoir plus sur les mises à jour bientôt disponibles avec WSL 2 !](https://docs.microsoft.com/windows/wsl/wsl2-index) : cette nouvelle version modifie la façon dont les distributions Linux interagissent avec Windows, ce qui améliore les performances du système de fichiers et ajoute la compatibilité complète des appels système.
- [Utilisation de plusieurs distributions Linux sur Windows](https://docs.microsoft.com/windows/wsl/wsl-config) : découvrez comment gérer plusieurs distributions Linux différentes sur votre machine Windows.
