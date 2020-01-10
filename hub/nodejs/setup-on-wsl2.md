---
title: Configurer NodeJS sur WSL 2
description: Guide pour vous aider à configurer votre environnement de développement node. js sur le sous-système Windows pour Linux (WSL).
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, node. js, Windows 10, Microsoft, formation NodeJS, node sur Windows, node sur WSL, node sur Linux sur Windows, installer le nœud sur Windows, NodeJS avec vs code, développer avec un nœud sur Windows, développer avec NodeJS sur Windows, installer le nœud sur WSL, NodeJS sur Windows Sous-système pour Linux
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: c987f5bea387c630a1b9ef23c928d7a1bb8fadfc
ms.sourcegitcommit: cf4bf0ab4ea9019c1edc2bb96387ce6cedbe91dd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/10/2020
ms.locfileid: "75835381"
---
# <a name="set-up-your-nodejs-development-environment-with-wsl-2"></a>Configurer votre environnement de développement node. js avec WSL 2

Vous trouverez ci-dessous un guide pas à pas pour vous aider à configurer votre environnement de développement node. js à l’aide du sous-système Windows pour Linux (WSL). Ce guide vous oblige actuellement à installer et à exécuter une version préliminaire de Windows Insider afin d’installer et d’utiliser [WSL 2](https://devblogs.microsoft.com/commandline/wsl-2-is-now-available-in-windows-insiders/). WSL 2 offre des améliorations significatives en termes de vitesse et de performances par rapport à WSL 1, en particulier en ce qui concerne node. js. De nombreux didacticiels et modules NPM pour le développement Web node. js sont écrits pour les utilisateurs Linux et utilisent des outils d’installation et d’empaquetage basés sur Linux. La plupart des applications Web sont également déployées sur Linux. par conséquent, l’utilisation de WSL 2 garantit la cohérence entre vos environnements de développement et de production.

> [!NOTE]
> Si vous êtes engagé à utiliser node. js directement sur Windows ou si vous envisagez d’utiliser un environnement de production Windows Server, consultez notre guide pour [configurer votre environnement de développement node. js directement sur Windows](./setup-on-windows.md).

## <a name="install-windows-10-insider-preview-build"></a>Installer la version préliminaire de Windows 10 Insider

1. **[Installez la version la plus récente de Windows 10](https://www.microsoft.com/software-download/windows10)** : sélectionnez **mettre à jour maintenant** pour télécharger l’Assistant Mise à jour. Une fois le téléchargement terminé, ouvrez l’Assistant Mise à jour pour voir si vous exécutez actuellement la dernière version de Windows et, si ce n’est pas le cas, sélectionnez **mettre à jour maintenant** dans la fenêtre de l’Assistant pour mettre à jour votre machine. *(Cette étape est facultative si vous exécutez une version assez récente de Windows 10.)*

    ![Assistant Windows Update](../images/windows-update-assistant2019.png)

2. **[Accédez à démarrer > paramètres > programme Windows Insider](ms-settings:windowsinsider)** : dans la fenêtre du programme Windows Insider, sélectionnez **prise en main** , puis **liez un compte**.

    ![Paramètres du programme Windows Insider](../images/windows-insider-program-settings.png)

3. **[Inscrivez-vous en tant que Windows Insider](https://insider.windows.com/getting-started/#register)** : Si vous n’êtes pas inscrit auprès du programme Insider, vous devez le faire avec votre [compte Microsoft](https://account.microsoft.com/account).

    ![Inscription à Windows Insider](../images/windows-insider-account.png)

4. Choisissez de recevoir des mises à jour de **sonnerie rapide** ou de **passer au contenu de la version Windows suivante** . Confirmez et choisissez de **redémarrer ultérieurement**. Nous allons devoir modifier quelques paramètres supplémentaires avant de redémarrer.

    ![Sonnerie rapide Windows Insider](../images/windows-insider-fast.png)

## <a name="enable-windows-subsystem-for-linux-and-virtual-machine-platform"></a>Activer le sous-système Windows pour Linux et la plateforme de machine virtuelle

1. Toujours dans les **Paramètres Windows**, recherchez **activer ou désactiver des fonctionnalités Windows**.
2. Une fois la liste des **fonctionnalités Windows** affichée, faites défiler la liste pour rechercher la plateforme de l' **ordinateur virtuel** et le **sous-système Windows pour Linux**, assurez-vous que la case est cochée pour activer les deux, puis sélectionnez **OK**.
3. Redémarrez votre ordinateur lorsque vous y êtes invité.

    ![Activer les fonctionnalités Windows](../images/windows-feature-settings.png)

## <a name="install-a-linux-distribution"></a>Installer une distribution Linux

Plusieurs distributions Linux peuvent s’exécuter sur WSL. Vous trouverez votre distribution préférée dans le Microsoft Store. Nous vous recommandons de commencer par la distribution [Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q) actuelle, bien connue et prise en charge.

1. Ouvrez le lien [Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q), ouvrez le Microsoft Store, puis sélectionnez **Obtenir**. *(Il s’agit d’un téléchargement assez volumineux, dont l’installation peut prendre un certain temps.)*

2. Une fois le téléchargement terminé, sélectionnez **Lancer** à partir du Microsoft Store ou tapant « Ubuntu 18.04 LTS » dans le menu **Démarrer** pour lancer l’application.

3. Vous êtes invité à créer un nom de compte et un mot de passe lors de la première exécution d’une distribution. Après cela, vous serez automatiquement connecté comme l’utilisateur par défaut ainsi défini. Vous pouvez choisir n’importe quel nom d’utilisateur et mot de passe. Ils sont sans lien avec votre nom d’utilisateur Windows.

    ![Distributions Linux dans le Microsoft Store](../images/store-linux-distros.png)

Vous pouvez identifier la distribution Linux que vous utilisez actuellement en saisissant ce qui suit : `lsb_release -dc`. Pour mettre à jour votre distribution Ubuntu, utilisez : `sudo apt update && sudo apt upgrade`. Nous vous recommandons de procéder à une mise à jour régulière, afin de vous assurer que vous disposez des packages les plus récents. Windows ne gère pas automatiquement cette mise à jour. Pour obtenir des liens vers d’autres distributions Linux disponibles dans le Microsoft Store, ainsi que d’autres méthodes d’installation ou procédures de résolution des problèmes, voir [Guide d’installation du sous-système Windows pour Linux pour Windows 10](https://docs.microsoft.com/windows/wsl/install-win10).

## <a name="install-wsl-2"></a>Installer WSL 2

WSL 2 est une [nouvelle version de l’architecture](https://docs.microsoft.com/windows/wsl/wsl2-about) dans WSL qui modifie la manière dont Linux distributions interagit avec Windows, ce qui améliore les performances et ajoute une compatibilité complète des appels système.

1. Dans PowerShell, entrez la commande : `wsl -l` pour afficher la liste des distributions WSL que vous avez installées sur votre ordinateur. Vous devez maintenant voir Ubuntu-18,04 dans cette liste.
2. À présent, entrez la commande : `wsl --set-version Ubuntu-18.04 2` pour configurer votre installation Ubuntu afin qu’elle utilise WSL 2.
3. Vérifiez la version de WSL que chacune des distributions installées utilise avec : `wsl --list --verbose` (ou `wsl -l -v`).

    ![Sous-système Windows pour Linux, version définie](../images/wsl-versions.png)

> [!TIP]
> Vous pouvez définir une distribution Linux que vous avez installée sur WSL 2 en suivant les mêmes instructions (à l’aide de PowerShell), et simplement remplacer « Ubuntu-18,04 » par le nom des distribution installés que vous souhaitez cibler. Pour revenir à WSL 1, exécutez la même commande que ci-dessus, mais en remplaçant le « 2 » par un « 1 ».  Vous pouvez également définir WSL 2 comme valeur par défaut pour toutes les distributions nouvellement installées en entrant : `wsl --set-default-version 2`.

## <a name="install-nvm-nodejs-and-npm"></a>Installer NVM, node. js et NPM

Il existe plusieurs façons d’installer node. js. Nous vous recommandons d’utiliser un gestionnaire de version au fur et à mesure que les versions changent très rapidement. Vous devrez probablement basculer entre plusieurs versions en fonction des besoins des différents projets sur lesquels vous travaillez. Node version Manager, plus communément appelé NVM, est la façon la plus courante d’installer plusieurs versions de node. js. Nous allons examiner les étapes d’installation de NVM, puis l’utiliser pour installer node. js et node Package Manager (NPM). D' [autres gestionnaires de versions](#alternative-version-managers) sont également pris en compte dans la section suivante.

> [!IMPORTANT]
> Il est toujours recommandé de supprimer les installations existantes de node. js ou NPM de votre système d’exploitation avant d’installer un gestionnaire de version, car les différents types d’installation peuvent entraîner des conflits étranges et confus. Par exemple, la version du nœud qui peut être installée avec la commande `apt-get` d’Ubuntu est actuellement obsolète. Pour obtenir de l’aide sur la suppression des installations précédentes, consultez [Suppression de NodeJS d’Ubuntu](https://askubuntu.com/questions/786015/how-to-remove-nodejs-from-ubuntu-16-04).)

1. Ouvrez votre ligne de commande Ubuntu 18,04.
2. Installation d’une boucle (outil utilisé pour télécharger du contenu à partir d’Internet sur la ligne de commande) avec : `sudo apt-get install curl`
3. Installer NVM, avec : `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.2/install.sh | bash`
4. Pour vérifier l’installation, entrez : `command -v nvm`... le message « NVM » doit s’afficher, si vous recevez « commande introuvable » ou aucune réponse, fermez votre terminal actuel, rouvrez-le, puis réessayez. [Pour en savoir plus, consultez le référentiel NVM GitHub](https://github.com/nvm-sh/nvm).
5. Répertorier les versions de nœud actuellement installées (ne doit pas être à ce stade) : `nvm ls`

    ![Liste NVM qui n’indique aucune version de nœud](../images/nvm-no-node.png)

6. Installer la version actuelle de node. js (pour tester les améliorations apportées aux fonctionnalités les plus récentes, mais plus susceptibles de rencontrer des problèmes) : `nvm install node`
7. Installer la dernière version stable de LTS de node. js (recommandé) : `nvm install --lts`
8. Répertorier les versions du nœud installées : `nvm ls`... vous devez maintenant voir les deux versions que vous venez d’installer.

    ![Liste NVM avec LTS et les versions de nœud actuelles](../images/nvm-node-installed.png)

9. Vérifiez que node. js est installé et que la version actuelle par défaut est la suivante : `node --version`. Vérifiez ensuite que vous disposez également de NPM, avec : `npm --version` (vous pouvez également utiliser `which node` ou `which npm` pour voir le chemin d’accès utilisé pour les versions par défaut).
10. Pour modifier la version de node. js que vous souhaitez utiliser pour un projet, créez un répertoire de projet `mkdir NodeTest`et entrez le répertoire `cd NodeTest`, puis entrez `nvm use node` pour basculer vers la version actuelle ou `nvm use --lts` pour basculer vers la version LTS. Vous pouvez également utiliser le numéro spécifique pour toutes les versions supplémentaires que vous avez installées, comme `nvm use v8.2.1`. (Pour répertorier toutes les versions de node. js disponibles, utilisez la commande : `nvm ls-remote`).

> [!TIP]
> Si vous utilisez NVM pour installer node. js et NPM, vous n’avez pas besoin d’utiliser la commande SUDO pour installer de nouveaux packages.

> [!NOTE]
> Au moment de la publication, NVM v 0.35.2 était la version la plus récente disponible. Vous pouvez consulter la [page de projet GitHub pour obtenir la dernière version d’NVM](https://github.com/nvm-sh/nvm)et ajuster la commande ci-dessus pour inclure la version la plus récente.
L’installation de la version la plus récente d’NVM à l’aide de la boucle remplacera celle qui était la plus ancienne, en laissant la version du nœud que vous avez utilisée pour installer l’État inchangé. Par exemple : `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.36.0/install.sh | bash`

## <a name="alternative-version-managers"></a>Gestionnaires de versions alternatifs

Si NVM est actuellement le gestionnaire de version le plus populaire pour node, il existe quelques alternatives à prendre en compte :

- [n](https://www.npmjs.com/package/n#installation) est une alternative `nvm` longue qui remplit la même fonction avec des commandes légèrement différentes et est installée via `npm` plutôt qu’un script bash.
- [FNM](https://github.com/Schniz/fnm#using-a-script) est un gestionnaire de version plus récent, qui prétend être beaucoup plus rapide que `nvm`. (Il utilise également [Azure pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/what-is-azure-pipelines?view=azure-devops).)
- [Volta](https://github.com/volta-cli/volta#installing-volta) est un nouveau gestionnaire de versions de l’équipe LinkedIn qui prétend améliorer la vitesse et la prise en charge multiplateforme.
- [asdf-VM](https://asdf-vm.com/#/core-manage-asdf-vm) est une interface de commande unique pour plusieurs langues, telle que IKE GVM, NVM, rbenv & pyenv (et plus), le tout en une seule.
- [NVS](https://github.com/jasongin/nvs) (node version changer) est une alternative `nvm` multiplateforme avec la possibilité de [s’intégrer avec vs code](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md).

## <a name="install-your-favorite-code-editor"></a>Installer votre éditeur de code favori

Nous vous recommandons d’utiliser **Visual Studio code** avec l' **extension Remote-WSL** pour les projets node. js. Cela divise VS Code en architecture « client-serveur », avec le client (l’interface utilisateur) exécuté sur votre ordinateur Windows et le serveur (votre code, git, plug-ins, etc.) exécuté à distance.

- La prise en charge des fonctionnalités IntelliSense et des linters basés sur Linux est prise en charge.
- Votre projet sera automatiquement créé dans Linux.
- Vous pouvez utiliser toutes vos extensions qui s’exécutent sur Linux ([es Lint, NPM IntelliSense, les extraits de code ES6, etc.](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack)).

Les éditeurs de texte basés sur les terminaux (VIM, Emacs, nano) sont également utiles pour apporter des modifications rapides directement à partir de la console. ([Cet article](https://medium.com/linode-cube/emacs-nano-or-vim-choose-your-terminal-based-text-editor-wisely-8f3826c92a68) est un bon travail expliquant la différence et l’utilisation de chacune d’elles.)

> [!NOTE]
> Certains éditeurs d’interface utilisateur graphique (Atom, sublime Text, Eclipse) peuvent rencontrer des problèmes d’accès à l’emplacement réseau partagé WSL (\\WSL $ \Ubuntu\home\) et tentent de générer vos fichiers Linux à l’aide d’outils Windows, ce qui peut ne pas être ce que vous souhaitez. Dans VS Code, l’extension Remote-WSL gère cette compatibilité.

Pour installer VS Code et l’extension WSL distante :

1. [Téléchargez et installez VS Code pour Windows](https://code.visualstudio.com). VS Code est également disponible pour Linux, mais le sous-système Windows pour Linux ne prend pas en charge les applications GUI. Nous devons donc l’installer sur Windows. Ne vous inquiétez pas, vous serez toujours en mesure de l’intégrer dans vos outils et dans la ligne de commande Linux à l’aide de l’extension WSL-Remote.

2. Installez [l’extension WSL-Remote](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) sur VS Code. Cela vous permet d’utiliser WSL comme environnement de développement intégré et de gérer la compatibilité et les chemins d’accès pour vous. [En savoir plus](https://code.visualstudio.com/docs/remote/remote-overview).

> [!IMPORTANT]
> Si vous avez déjà installé VS Code, vous devez vous assurer que vous disposez de la version [1.35 de mai](https://code.visualstudio.com/updates/v1_35) ou plus pour installer [l’extension WSL-Remote](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl). Nous vous déconseillons d’utiliser WSL dans VS Code sans l’extension WSL distante, car vous perdrez la prise en charge de la saisie semi-automatique, du débogage, du découpage, etc. Fait amusant : cette extension WSL est installée dans $HOME/.vscode-Server/Extensions.

### <a name="helpful-vs-code-extensions"></a>Extensions VS Code utiles

Si VS Code est fourni avec de nombreuses fonctionnalités pour le développement node. js, il existe des extensions utiles pour envisager l’installation de disponibles dans le [Pack d’extension node. js](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack). Installez-les tous ou choisissez ce qui vous semble le plus utile.

Pour installer le pack d’extension node. js :

1. Ouvrez la fenêtre **Extensions** (Ctrl + Maj + X) dans vs code.

    La fenêtre extensions est désormais divisée en trois sections (car vous avez installé l’extension WSL distante).
    - « Local-installed » : les extensions installées pour une utilisation avec votre système d’exploitation Windows.
    - « WSL : Ubuntu-18,04-installed » : les extensions installées pour une utilisation avec votre système d’exploitation Ubuntu (WSL).
    - « Recommandé » : extensions recommandées par VS Code en fonction des types de fichiers dans votre projet actuel.

    ![VS Code extensions locales et distantes](../images/vscode-extensions-local-remote.png)

2. Dans la zone de recherche en haut de la fenêtre extensions, entrez : **node extension Pack** (ou nom de l’extension que vous recherchez). L’extension sera installée pour vos instances locales ou WSL de VS Code selon l’endroit où vous avez ouvert le projet actuel. Vous pouvez le savoir en sélectionnant le lien distant dans le coin inférieur gauche de la fenêtre de VS Code (en vert). Elle vous donne la possibilité d’ouvrir ou de fermer une connexion à distance. Installez vos extensions node. js dans l’environnement « WSL : Ubuntu-18,04 ».

    ![Lien VS Code distant](../images/wsl-remote-extension.png)

Voici quelques-unes des extensions supplémentaires que vous pouvez envisager :

- [Débogueur pour Chrome](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code): une fois que vous avez terminé le développement côté serveur avec node. js, vous devez développer et tester le côté client. Cette extension intègre votre éditeur de VS Code avec votre service de débogage de navigateur Chrome, ce qui rend les choses un peu plus efficaces.
- [Cartes à partir d’autres éditeurs](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads): ces extensions peuvent vous aider dans votre environnement à la racine si vous effectuez une transition à partir d’un autre éditeur de texte (comme Atom, Subvert, vim, EMacs, Notepad + +, etc.).
- [Synchronisation des paramètres](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync): vous permet de synchroniser vos paramètres de vs code sur différentes installations à l’aide de github. Si vous travaillez sur plusieurs ordinateurs, cela permet de garantir la cohérence de votre environnement.

## <a name="install-windows-terminal-optional"></a>Installer Windows Terminal (facultatif)

Le nouveau terminal Windows active plusieurs onglets (basculer rapidement entre l’invite de commandes, PowerShell ou plusieurs distributions Linux), les combinaisons de touches personnalisées (créez vos propres touches de raccourci pour ouvrir ou fermer les onglets, copier + coller, etc.), les Emoji ☺ et les thèmes personnalisés (modèles de couleurs, styles et tailles de police, image d’arrière-plan/flou [En savoir plus](https://devblogs.microsoft.com/commandline/).

1. Obtenir [Windows Terminal (version préliminaire) dans le Microsoft Store](https://www.microsoft.com/store/apps/9n0dx20hk701): en installant via le Store, les mises à jour sont gérées automatiquement.

2. Une fois l’installation terminée, ouvrez Windows Terminal et sélectionnez **paramètres** pour personnaliser votre terminal à l’aide du fichier `profile.json`. [En savoir plus sur la modification des paramètres de Windows Terminal Server](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md).

    ![Paramètres Windows Terminal Server](../images/windows-terminal-settings.png)

## <a name="set-up-git-optional"></a>Configurer Git (facultatif)

Si vous envisagez de collaborer avec d’autres personnes ou d’héberger votre projet sur un site Open source (comme GitHub), VS Code prend en charge [le contrôle de version avec git](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support). L’onglet Contrôle de code source de VS Code assure le suivi de toutes vos modifications et contient des commandes Git courantes (ajouter, valider, pousser, extraire) intégrées directement dans l’interface utilisateur.

1. Git est installé avec le sous-système Windows pour Linux distributions. Toutefois, vous devrez configurer votre fichier de configuration git. Pour ce faire, dans votre terminal, entrez : `git config --global user.name "Your Name"` puis, `git config --global user.email "youremail@domain.com"`. Si vous n’avez pas encore de compte git, vous pouvez [en créer un sur GitHub](https://github.com/join). Si vous n’avez jamais travaillé avec Git, les [guides GitHub](https://guides.github.com/) peuvent vous aider à démarrer. Si vous avez besoin de modifier votre configuration git, vous pouvez le faire avec un éditeur de texte intégré tel que Nano : `nano ~/.gitconfig`.

2. Nous vous recommandons d’ajouter un [fichier. gitignore](https://help.github.com/en/articles/ignoring-files) à vos projets de nœud. Voici le [modèle gitignore par défaut de GitHub pour node. js](https://github.com/github/gitignore/blob/master/Node.gitignore). Si vous choisissez de [créer un référentiel à l’aide du site Web GitHub](https://help.github.com/articles/create-a-repo), des cases à cocher sont disponibles pour initialiser votre référentiel avec un fichier Lisez-moi, un fichier. gitignore configuré pour les projets node. js et des options pour ajouter une licence si vous en avez besoin.

## <a name="next-steps"></a>Étapes suivantes

Vous disposez maintenant d’un environnement de développement node. js configuré. Pour commencer à utiliser votre environnement node. js, pensez à essayer l’un de ces didacticiels :

- [Prise en main de node. js pour les débutants](./beginners.md)
- [Prise en main des infrastructures Web node. js sur Windows](./web-frameworks.md)
- [Prise en main de la connexion des applications node. js à une base de données](./databases.md)
- [Prise en main des conteneurs de l’arrimeur avec node. js](./containers.md)
