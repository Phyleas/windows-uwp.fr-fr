---
title: Configurer NodeJS sur WSL 2
description: Guide conçu pour vous aider à configurer votre environnement de développement Node.js sur le Sous-système Windows pour Linux.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, windows 10, microsoft, découvrir nodejs, node sur windows, node sur wsl, node sur linux sur windows, installer node sur windows, nodejs avec vs code, développer avec node sur windows, développer avec nodejs sur windows, installer node sur WSL, NodeJS sur le Sous-système Windows pour Linux
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: c987f5bea387c630a1b9ef23c928d7a1bb8fadfc
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75835381"
---
# <a name="set-up-your-nodejs-development-environment-with-wsl-2"></a>Configurer votre environnement de développement Node.js avec WSL 2

Vous trouverez ci-dessous un guide pas à pas conçu pour vous aider à configurer votre environnement de développement Node.js à l’aide du Sous-système Windows pour Linux. Ce guide vous oblige actuellement à installer et à exécuter une build Windows Insider Preview pour installer et d’utiliser [WSL 2](https://devblogs.microsoft.com/commandline/wsl-2-is-now-available-in-windows-insiders/). WSL 2 offre des améliorations significatives en termes de vitesse et de performances par rapport à WSL 1, en particulier en ce qui concerne Node.js. De nombreux modules et tutoriels npm pour le développement web en Node.js sont écrits pour les utilisateurs de Linux et se servent d’outils d’installation et d’empaquetage basés sur Linux. La plupart des applications Web étant également déployées sur Linux. par conséquent, l’utilisation de WSL 2 garantit la cohérence entre vos environnements de développement et de production.

> [!NOTE]
> Si vous êtes engagé dans l’utilisation de Node.js directement sur Windows ou si vous envisagez d’utiliser un environnement de production Windows Server, consultez notre guide pour [configurer votre environnement de développement Node.js directement sur Windows](./setup-on-windows.md).

## <a name="install-windows-10-insider-preview-build"></a>Installer une build Windows 10 Insider Preview

1. **[Installez la version la plus récente de Windows 10](https://www.microsoft.com/software-download/windows10)**  : Sélectionnez **Mettre à jour maintenant** pour télécharger l’Assistant de mise à jour. Une fois l’Assistant téléchargé, ouvrez-le pour voir si vous exécutez actuellement la dernière version de Windows. Si ce n’est pas le cas, sélectionnez **Mettre à jour maintenant** dans la fenêtre de l’Assistant pour mettre à jour votre machine *(cette étape est facultative si vous exécutez une version assez récente de Windows 10).*

    ![Assistant Windows Update](../images/windows-update-assistant2019.png)

2. **[Accédez à démarrer > Paramètres > Programme Windows Insider](ms-settings:windowsinsider)**  : Dans la fenêtre du Programme Windows Insider, sélectionnez **Prise en main**, puis **lier un compte**.

    ![Paramètres du Programme Windows Insider](../images/windows-insider-program-settings.png)

3. **[Inscrivez-vous en tant que Windows Insider](https://insider.windows.com/getting-started/#register)**  : Si vous n’êtes pas inscrit au programme Insider, vous devez le faire avec votre [compte Microsoft](https://account.microsoft.com/account).

    ![Inscription à Windows Insider](../images/windows-insider-account.png)

4. Choisissez **Anneau Rapide** pour recevoir des mises à jour ou **Avance rapide** pour passer au contenu de la version suivante de Windows. Confirmez et choisissez de **Redémarrer plus tard**. Nous allons devoir modifier quelques paramètres supplémentaires avant de redémarrer.

    ![Anneau Rapide de Windows Insider](../images/windows-insider-fast.png)

## <a name="enable-windows-subsystem-for-linux-and-virtual-machine-platform"></a>Activer le Sous-système Windows pour Linux et la Plateforme de machine virtuelle

1. Toujours dans **Paramètres Windows**, recherchez **Activer ou désactiver des fonctionnalités Windows**.
2. Lorsque la liste **Fonctionnalités Windows** s’affiche, faites-la défiler pour trouver les options **Plateforme de machine virtuelle** et **Sous-système Windows pour Linux**, vérifiez que les deux cases à cocher sont activées, puis cliquez sur **OK**.
3. Redémarrez votre ordinateur lorsque vous y êtes invité.

    ![Activer les Fonctionnalités Windows](../images/windows-feature-settings.png)

## <a name="install-a-linux-distribution"></a>Installer une distribution Linux

Plusieurs distributions Linux peuvent s’exécuter sur WSL. Vous trouverez votre distribution préférée dans le Microsoft Store. Nous vous recommandons de commencer par la distribution [Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q) actuelle, bien connue et prise en charge.

1. Ouvrez le lien [Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q), ouvrez le Microsoft Store, puis sélectionnez **Obtenir**. *(Il s’agit d’un téléchargement assez volumineux, dont l’installation peut prendre un certain temps.)*

2. Une fois le téléchargement terminé, sélectionnez **Lancer** à partir du Microsoft Store ou tapant « Ubuntu 18.04 LTS » dans le menu **Démarrer** pour lancer l’application.

3. Vous êtes invité à créer un nom de compte et un mot de passe lors de la première exécution d’une distribution. Après cela, vous serez automatiquement connecté comme l’utilisateur par défaut ainsi défini. Vous pouvez choisir les nom d’utilisateur et mot de passe que vous voulez. Ils sont sans lien avec votre nom d’utilisateur Windows.

    ![Distributions Linux dans le Microsoft Store](../images/store-linux-distros.png)

Vous pouvez identifier la distribution Linux que vous utilisez actuellement en saisissant ce qui suit : `lsb_release -dc`. Pour mettre à jour votre distribution Ubuntu, utilisez : `sudo apt update && sudo apt upgrade`. Nous vous recommandons de procéder à une mise à jour régulière, afin de vous assurer que vous disposez des packages les plus récents. Windows ne gère pas automatiquement cette mise à jour. Pour obtenir des liens vers d’autres distributions Linux disponibles dans le Microsoft Store, ainsi que d’autres méthodes d’installation ou procédures de résolution des problèmes, voir [Guide d’installation du sous-système Windows pour Linux pour Windows 10](https://docs.microsoft.com/windows/wsl/install-win10).

## <a name="install-wsl-2"></a>Installer WSL 2

WSL 2 est une [nouvelle version de l’architecture](https://docs.microsoft.com/windows/wsl/wsl2-about) présente dans WSL, qui change la façon dont les distributions Linux interagissent avec Windows, en améliorant les performances et ajoutant une compatibilité complète des appels système.

1. Dans PowerShell, entrez la commande `wsl -l` pour afficher la liste des distributions WSL que vous avez installées sur votre ordinateur. Vous devriez maintenant voir Ubuntu-18.04 dans cette liste.
2. À présent, entrez la commande `wsl --set-version Ubuntu-18.04 2` pour configurer votre installation Ubuntu afin qu’elle utilise WSL 2.
3. Vérifiez la version de WSL que chacune des distributions installées utilise avec la commande `wsl --list --verbose` (ou `wsl -l -v`).

    ![Version installée du Sous-système Windows pour Linux](../images/wsl-versions.png)

> [!TIP]
> Vous pouvez définir une distribution Linux que vous avez installée sur WSL 2 en suivant les mêmes instructions (à l’aide de PowerShell), et en remplaçant simplement « Ubuntu-18.04 » par le nom de la distribution installée que vous souhaitez cibler. Pour revenir à WSL 1, exécutez la même commande que ci-dessus, mais en remplaçant le chiffre « 2 » par le chiffre « 1 ».  Vous pouvez également définir WSL 2 comme valeur par défaut pour toute distribution nouvellement installée en entrant `wsl --set-default-version 2`.

## <a name="install-nvm-nodejs-and-npm"></a>Installer nvm, node.js et npm

Plusieurs méthodes sont disponibles pour installer Node.js. Nous vous recommandons d'utiliser un gestionnaire de versions car les versions changent très rapidement. Vous devrez probablement passer d’une version à une autre en fonction des besoins des différents projets sur lesquels vous travaillez. Node Version Manager, également appelé nvm, est la méthode la plus couramment utilisée pour installer plusieurs versions de Node.js. Nous allons donc suivre les étapes d’installation de nvm et l’utiliser pour installer Node.js et Node Package Manager (npm). D’[autres gestionnaires de versions](#alternative-version-managers) sont également à prendre en compte. Ceux-ci sont couverts à la section suivante.

> [!IMPORTANT]
> Veillez à supprimer toutes les installations existantes de Node.js ou de npm de votre système d’exploitation avant d’installer un gestionnaire de versions, car les différents types d’installation peuvent entraîner des conflits déroutants. Par exemple, la version de Node qui peut être installée avec la commande `apt-get` d’Ubuntu est actuellement obsolète. Pour obtenir de l’aide sur la suppression d’installations précédentes, consultez [Comment supprimer Node.js d’Ubuntu](https://askubuntu.com/questions/786015/how-to-remove-nodejs-from-ubuntu-16-04).

1. Ouvrez votre ligne de commande Ubuntu 18.04.
2. Installez cURL (outil utilisé pour télécharger du contenu à partir d’Internet dans la ligne de commande) avec la commande `sudo apt-get install curl`
3. Installez nvm avec la commande `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.2/install.sh | bash`
4. Pour vérifier l’installation, entrez la commande `command -v nvm`... qui devrait retourner « nvm ». Si vous recevez le message « commande introuvable » ou aucune réponse, fermez votre terminal, rouvrez-le, puis réessayez. [Pour en savoir plus, consultez le dépôt GitHub nvm](https://github.com/nvm-sh/nvm).
5. Répertoriez les versions de Node actuellement installées (il ne doit y en avoir aucune à ce stade) : `nvm ls`

    ![Liste NVM ne montrant aucune version de Node](../images/nvm-no-node.png)

6. Installez la version actuelle de Node.js (pour tester les améliorations les plus récentes apportées aux fonctionnalités, mais plus probablement rencontrer des problèmes) : `nvm install node`
7. Installez la dernière version LTS stable de Node.js (recommandé) : `nvm install --lts`
8. Répertoriez les versions de Node installées : `nvm ls`... Les deux versions que vous venez d’installer devraient maintenant être répertoriées.

    ![Liste NVM avec LTS et les versions actuelles de Node](../images/nvm-node-installed.png)

9. Vérifiez que Node.js est installé et que la version par défaut actuelle est la suivante : `node --version`. Vérifiez ensuite que vous disposez également de npm, avec la commande : `npm --version` (vous pouvez également utiliser `which node` ou `which npm` pour voir le chemin d’accès utilisé pour les versions par défaut).
10. Pour changer la version de Node.js à utiliser pour un projet, créez un répertoire de projet `mkdir NodeTest`, accédez au répertoire `cd NodeTest`, puis entrez `nvm use node` pour basculer vers la version actuelle ou `nvm use --lts` pour basculer vers la version LTS. Vous pouvez également utiliser le numéro spécifique de toute version supplémentaire que vous avez installée, comme `nvm use v8.2.1` (pour répertorier toutes les versions de Node.js disponibles, utilisez la commande `nvm ls-remote`).

> [!TIP]
> Si vous utilisez NVM pour installer Node.js et NPM, vous n’avez pas besoin d’utiliser la commande SUDO pour installer de nouveaux packages.

> [!NOTE]
> Au moment de la publication, NVM v0.35.2 était la version la plus récente disponible. Vous pouvez consulter la [page du projet GitHub pour voir la dernière version de NVM](https://github.com/nvm-sh/nvm), et ajuster la commande ci-dessus afin d’inclure la version la plus récente.
L’installation de la version plus récente de NVM à l’aide de cURL a pour effet de remplacer la plus ancienne, en laissant intacte la version de Node que vous avez utilisée pour installer NVM. Par exemple : `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.36.0/install.sh | bash`

## <a name="alternative-version-managers"></a>Autres gestionnaires de versions

Si nvm est actuellement le gestionnaire de versions le plus couramment utilisé pour Node, il en existe d’autres :

- [n](https://www.npmjs.com/package/n#installation) est une alternative classique de `nvm`, qui remplit la même fonction avec des commandes légèrement différentes, et qui est installée via `npm` au lieu d’un script bash.
- [fnm](https://github.com/Schniz/fnm#using-a-script) est un gestionnaire de versions plus récent, supposé être beaucoup plus rapide que `nvm` (il utilise également [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/what-is-azure-pipelines?view=azure-devops)).
- [Volta](https://github.com/volta-cli/volta#installing-volta) est un nouveau gestionnaire de versions de l’équipe LinkedIn, présenté comme plus rapide et offrant une prise en charge multiplateforme.
- [asdf-VM](https://asdf-vm.com/#/core-manage-asdf-vm) est une interface de ligne de commande unique tout en un pour plusieurs langages tels que gvm, nvm, rbenv et pyenv (entre autres).
- [nvs](https://github.com/jasongin/nvs) (Node Version Switcher) est une alternative multiplateforme à `nvm`, offrant la possibilité d’[intégration avec VS Code](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md).

## <a name="install-your-favorite-code-editor"></a>Installer votre éditeur de code favori

Nous vous recommandons d’utiliser **Visual Studio Code** avec l’**extension Remote-WSL** pour les projets Node.js. Cela a pour effet de diviser VS Code en une architecture « client-serveur », avec le client (l’interface utilisateur) s’exécutant sur votre ordinateur Windows, et le serveur (votre code, Git, plug-in, etc.) s’exécutant à distance.

- Les solutions IntelliSense et Linting basées sur Linux sont prises en charge.
- Votre projet sera automatiquement généré dans Linux.
- Vous pouvez utiliser toutes vos extensions s’exécutant sur Linux ([ES Lint, NPM Intellisense, extraits de code ES6, etc.](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack)).

Les éditeurs de texte basés sur un terminal (VIM, Emacs, nano) sont également utiles pour apporter des modifications rapides directement sur la console ([cet article](https://medium.com/linode-cube/emacs-nano-or-vim-choose-your-terminal-based-text-editor-wisely-8f3826c92a68) explique judicieusement la différence entre elles et leur utilisation).

> [!NOTE]
> Certains éditeurs d’interface utilisateur graphique (Atom, Sublime Text, Eclipse) peuvent rencontrer des problèmes lors de l’accès à l’emplacement réseau partagé WSL (\\wsl$\Ubuntu\home\) et tenter de générer vos fichiers Linux à l’aide d’outils Windows, ce que vous ne souhaitez peut-être pas. Dans VS Code, l’extension Remote-WSL gère cette compatibilité.

Pour installer VS Code et l’extension Remote-WSL :

1. [Téléchargez et installez VS Code pour Windows](https://code.visualstudio.com). VS Code est également disponible pour Linux, mais le sous-système Windows pour Linux ne prend pas en charge les applications GUI. Nous devons donc l’installer sur Windows. Ne vous inquiétez pas, vous serez toujours en mesure de l’intégrer dans vos outils et dans la ligne de commande Linux à l’aide de l’extension WSL-Remote.

2. Installez [l’extension WSL-Remote](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) sur VS Code. Cela vous permet d’utiliser WSL comme environnement de développement intégré et de gérer la compatibilité et les chemins d’accès pour vous. [En savoir plus](https://code.visualstudio.com/docs/remote/remote-overview)

> [!IMPORTANT]
> Si vous avez déjà installé VS Code, vous devez vous assurer que vous disposez de la version [1.35 de mai](https://code.visualstudio.com/updates/v1_35) ou plus pour installer [l’extension WSL-Remote](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl). Nous vous déconseillons d’utiliser WSL dans VS Code sans l’extension WSL-Remote, car vous perdrez la prise en charge de la saisie semi-automatique, du débogage, de la vérification (linting), etc. Le saviez-vous ? Cette extension WSL est installée dans le répertoire $HOME/.vscode-server/extensions.

### <a name="helpful-vs-code-extensions"></a>Extensions VS Code utiles

Si VS Code intègre de nombreuses fonctionnalités pour le développement en Node.js, il existe des extensions utiles disponibles dans le [Pack d’extension Node.js](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack) dont vous pouvez envisager l’installation. Installez-les toutes ou choisissez celles qui vous semblent les plus utiles.

Pour installer le pack d'extension Node.js :

1. Ouvrez la fenêtre **Extensions** (Ctrl + Maj + X) dans VS Code.

    La fenêtre Extensions est divisée en trois sections (parce que vous avez installé l’extension Remote-WSL).
    - « Local - Installed » : Extensions installées pour une utilisation avec votre système d’exploitation Windows.
    - « WSL:Ubuntu-18.04-Installed » : Extensions installées pour une utilisation avec votre système d’exploitation Ubuntu (WSL).
    - « Recommended » : Extensions recommandées par VS Code en fonction des types de fichiers dans votre projet actuel.

    ![Extensions VS Code locales et distantes](../images/vscode-extensions-local-remote.png)

2. Dans la zone de recherche du haut de la fenêtre Extensions, entrez : **Pack d’extension Node** (ou le nom d’une extension que vous recherchez). L’extension sera installée pour vos instances locales ou WSL de VS Code, selon l’emplacement où vous avez ouvert le projet actuel. Vous pouvez le déterminer en sélectionnant le lien distant dans l’angle inférieur gauche de la fenêtre de VS Code (en vert). Elle vous donne la possibilité d’ouvrir ou de fermer une connexion à distance. Installez vos extensions Node.js dans l’environnement « WSL:Ubuntu-18.04 ».

    ![Lien distant de VS Code](../images/wsl-remote-extension.png)

D’autres extensions sont disponibles :

- [Debugger pour Chrome](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code) : lorsque vous aurez terminé le développement côté serveur avec Node.js, vous devrez développer et tester le côté client. Cette extension intègre votre éditeur VS Code au service de débogage de votre navigateur Chrome, pour plus d'efficacité.
- [Mappages de touches d'autres éditeurs](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads) : ces extensions peuvent être utiles si vous utilisiez auparavant un autre éditeur de texte (comme Atom, Sublime, Vim, eMacs, Notepad++, etc.).
- [Synchronisation des paramètres](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync) : vous permet de synchroniser vos paramètres VS Code entre différentes installations à l'aide de GitHub. Si vous travaillez sur plusieurs ordinateurs, cela permet de garantir la cohérence de votre environnement.

## <a name="install-windows-terminal-optional"></a>Installer le Terminal Windows (facultatif)

Le nouveau Terminal Windows active plusieurs onglets (pour le basculement rapide entre l’invite de commandes, PowerShell ou diverses distributions Linux), les combinaisons de touches personnalisées (créez vos propres touches de raccourci pour ouvrir ou fermer les onglets, copier-coller, etc.), les emoji ☺ et les thèmes personnalisés (modèles de couleurs, styles et tailles de police, image d’arrière-plan/flou/transparence). [En savoir plus](https://devblogs.microsoft.com/commandline/)

1. Procurez-vous le [Terminal Windows (préversion) dans le Microsoft Store](https://www.microsoft.com/store/apps/9n0dx20hk701) : En installant via le Store, les mises à jour sont gérées automatiquement.

2. Une fois l’installation terminée, ouvrez le Terminal Windows, puis sélectionnez **Paramètres** pour personnaliser votre terminal à l’aide du fichier `profile.json`. [En savoir plus sur la modification des paramètres du Terminal Windows](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md).

    ![Paramètres du Terminal Windows](../images/windows-terminal-settings.png)

## <a name="set-up-git-optional"></a>Configurer Git (facultatif)

Si vous envisagez de collaborer avec d’autres personnes ou d’héberger votre projet sur un site open source (comme GitHub), VS Code prend en charge le [contrôle de version avec Git](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support). L’onglet Contrôle de code source de VS Code assure le suivi de toutes vos modifications et contient des commandes Git courantes (ajouter, valider, pousser, extraire) intégrées directement dans l’interface utilisateur.

1. Git est installé avec les distributions du Sous-système Windows pour Linux. Vous devez cependant configurer votre fichier de configuration git. Pour ce faire, dans votre terminal, entrez `git config --global user.name "Your Name"`, puis `git config --global user.email "youremail@domain.com"`. Si vous n’avez pas encore de compte Git, vous pouvez [vous inscrire pour en obtenir un sur GitHub](https://github.com/join). Si vous n’avez jamais travaillé avec Git, les [guides GitHub](https://guides.github.com/) peuvent vous aider à démarrer. Si vous avez besoin de modifier votre configuration git, vous pouvez le faire avec un éditeur de texte intégré tel que Nano : `nano ~/.gitconfig`.

2. Nous vous recommandons d’ajouter un fichier [.gitignore](https://help.github.com/en/articles/ignoring-files) à vos projets Node. Voici le [modèle gitignore par défaut de GitHub pour Node.js](https://github.com/github/gitignore/blob/master/Node.gitignore). Si vous choisissez de [créer un dépôt à l’aide du site web GitHub](https://help.github.com/articles/create-a-repo), des cases à cocher sont disponibles pour initialiser votre dépôt à l’aide d’un fichier README, d’un fichier. gitignore configuré pour les projets Node.js et d’options permettant d’ajouter une licence au besoin.

## <a name="next-steps"></a>Étapes suivantes

Vous disposez maintenant d’un environnement de développement Node.js configuré. Pour commencer à utiliser votre environnement Node.js, envisagez d’essayer l’un des tutoriels suivants :

- [Bien démarrer avec Node.js pour les débutants](./beginners.md)
- [Prise en main des frameworks web Node.js sous Windows](./web-frameworks.md)
- [Prise en main de la connexion des applications Node.js à une base de données](./databases.md)
- [Prise en main des conteneurs Docker avec Node.js](./containers.md)
