---
title: Configurer NodeJS sur WSL 2
description: Guide conçu pour vous aider à configurer votre environnement de développement Node.js sur le Sous-système Windows pour Linux.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, windows 10, microsoft, découvrir nodejs, node sur windows, node sur wsl, node sur linux sur windows, installer node sur windows, nodejs avec vs code, développer avec node sur windows, développer avec nodejs sur windows, installer node sur WSL, NodeJS sur le Sous-système Windows pour Linux
ms.localizationpriority: medium
ms.date: 07/28/2020
ms.openlocfilehash: ce4e736751d5586c6ab4489e976fc397b1be0301
ms.sourcegitcommit: 6b83f1854a113490dcd4f52425ecade9e66e0b44
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87333793"
---
# <a name="set-up-your-nodejs-development-environment-with-wsl-2"></a>Configurer votre environnement de développement Node.js avec WSL 2

Vous trouverez ci-dessous un guide pas à pas conçu pour vous aider à configurer votre environnement de développement Node.js à l’aide du Sous-système Windows pour Linux.

Nous vous recommandons d’installer et d’exécuter la version mise à jour de WSL 2, car elle vous permettra de tirer parti des améliorations importantes qui ont été apportées au niveau des performances et de la compatibilité des appels système, notamment la possibilité d’exécuter [Docker Desktop](https://docs.docker.com/docker-for-windows/wsl-tech-preview/#download). De nombreux modules et tutoriels npm pour le développement web en Node.js sont écrits pour les utilisateurs de Linux et se servent d’outils d’installation et d’empaquetage basés sur Linux. La plupart des applications Web étant également déployées sur Linux. par conséquent, l’utilisation de WSL 2 garantit la cohérence entre vos environnements de développement et de production.

> [!NOTE]
> Si vous êtes engagé dans l’utilisation de Node.js directement sur Windows ou si vous envisagez d’utiliser un environnement de production Windows Server, consultez notre guide pour [configurer votre environnement de développement Node.js directement sur Windows](./setup-on-windows.md).

## <a name="install-wsl-2"></a>Installer WSL 2

Pour activer et installer WSL 2, suivez les étapes fournies dans la [documentation sur l’installation de WSL](https://docs.microsoft.com/windows/wsl/install-win10). Ces étapes permettent de choisir une distribution Linux (par exemple, Ubuntu).

Une fois que vous avez installé WSL 2 et une distribution Linux, ouvrez la distribution Linux (celle-ci est accessible à partir du menu Démarrer de Windows), puis vérifiez sa version et son nom de code à l’aide de la commande : `lsb_release -dc`.

Nous vous recommandons de mettre régulièrement à jour votre distribution Linux, notamment juste après son installation, pour être sûr que vous disposez des packages les plus récents. Windows ne gère pas automatiquement cette mise à jour. Pour mettre à jour votre distribution, utilisez la commande suivante : `sudo apt update && sudo apt upgrade`.  

## <a name="install-windows-terminal-optional"></a>Installer le Terminal Windows (facultatif)

Le nouveau terminal Windows permet d’activer plusieurs onglets (basculement rapide d’une ligne de commande Linux à l’autre, invite de commandes Windows, PowerShell, Azure CLI, etc.), de créer des combinaisons de touches personnalisées (touches de raccourci pour ouvrir ou fermer les onglets, Copier + Coller, etc.), d’utiliser la fonctionnalité de recherche et de configurer des thèmes personnalisés (modèles de couleurs, styles et tailles de police, image d’arrière-plan/flou/transparence). [En savoir plus](https://docs.microsoft.com/windows/terminal)

1. Procurez-vous le [Terminal Windows dans le Microsoft Store](https://www.microsoft.com/store/apps/9n0dx20hk701) : En installant via le Store, les mises à jour sont gérées automatiquement.

2. Une fois l’installation terminée, ouvrez le Terminal Windows, puis sélectionnez **Paramètres** pour personnaliser votre terminal à l’aide du fichier `settings.json`.

    ![Paramètres du Terminal Windows](../images/windows-terminal-settings.png)

## <a name="install-nvm-nodejs-and-npm"></a>Installer nvm, node.js et npm

Plusieurs méthodes sont disponibles pour installer Node.js. Nous vous recommandons d'utiliser un gestionnaire de versions car les versions changent très rapidement. Vous devrez probablement passer d’une version à une autre en fonction des besoins des différents projets sur lesquels vous travaillez. Node Version Manager, également appelé nvm, est la méthode la plus couramment utilisée pour installer plusieurs versions de Node.js. Nous allons donc suivre les étapes d’installation de nvm et l’utiliser pour installer Node.js et Node Package Manager (npm). D’[autres gestionnaires de versions](#alternative-version-managers) sont également à prendre en compte. Ceux-ci sont couverts à la section suivante.

> [!IMPORTANT]
> Veillez à supprimer toutes les installations existantes de Node.js ou de npm de votre système d’exploitation avant d’installer un gestionnaire de versions, car les différents types d’installation peuvent entraîner des conflits déroutants. Par exemple, la version de Node qui peut être installée avec la commande `apt-get` d’Ubuntu est actuellement obsolète. Pour obtenir de l’aide sur la suppression d’installations précédentes, consultez [Comment supprimer Node.js d’Ubuntu](https://askubuntu.com/questions/786015/how-to-remove-nodejs-from-ubuntu-16-04).

1. Ouvrez votre ligne de commande Ubuntu 18.04.
2. Installez cURL (outil utilisé pour télécharger du contenu à partir d’Internet dans la ligne de commande) avec la commande `sudo apt-get install curl`
3. Installez nvm avec la commande `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash`

> [!NOTE]
> Au moment de la publication, NVM v0.35.3 était la version la plus récente disponible. Vous pouvez consulter la [page du projet GitHub pour voir la dernière version de NVM](https://github.com/nvm-sh/nvm), et ajuster la commande ci-dessus afin d’inclure la version la plus récente.
L’installation de la version plus récente de NVM à l’aide de cURL a pour effet de remplacer la plus ancienne, en laissant intacte la version de Node que vous avez utilisée pour installer NVM. Par exemple : `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.36.0/install.sh | bash`

4. Pour vérifier l’installation, entrez la commande `command -v nvm`... qui devrait retourner « nvm ». Si vous recevez le message « commande introuvable » ou aucune réponse, fermez votre terminal, rouvrez-le, puis réessayez. [Pour en savoir plus, consultez le dépôt GitHub nvm](https://github.com/nvm-sh/nvm).
5. Répertoriez les versions de Node actuellement installées (il ne doit y en avoir aucune à ce stade) : `nvm ls`

    ![Liste NVM ne montrant aucune version de Node](../images/nvm-no-node.png)

6. Installez la version actuelle de Node.js (pour tester les améliorations les plus récentes apportées aux fonctionnalités, mais plus probablement rencontrer des problèmes) : `nvm install node`
7. Installez la dernière version LTS stable de Node.js (recommandé) : `nvm install --lts`
8. Répertoriez les versions de Node installées : `nvm ls`... Les deux versions que vous venez d’installer devraient maintenant être répertoriées.

    ![Liste NVM avec LTS et les versions actuelles de Node](../images/nvm-node-installed.png)

9. Vérifiez que Node.js est installé et que la version par défaut actuelle est la suivante : `node --version`. Vérifiez ensuite que vous disposez également de npm, avec la commande : `npm --version` (vous pouvez également utiliser `which node` ou `which npm` pour voir le chemin d’accès utilisé pour les versions par défaut).
10. Pour changer la version de Node.js à utiliser pour un projet, créez un répertoire de projet `mkdir NodeTest`, accédez au répertoire `cd NodeTest`, puis entrez `nvm use node` pour basculer vers la version actuelle ou `nvm use --lts` pour basculer vers la version LTS. Vous pouvez également utiliser le numéro spécifique de toute version supplémentaire que vous avez installée, comme `nvm use v8.2.1` (pour répertorier toutes les versions de Node.js disponibles, utilisez la commande `nvm ls-remote`).

Si vous utilisez NVM pour installer Node.js et NPM, vous n’avez pas besoin d’utiliser la commande SUDO pour installer de nouveaux packages.

## <a name="alternative-version-managers"></a>Autres gestionnaires de versions

Si nvm est actuellement le gestionnaire de versions le plus couramment utilisé pour Node, il en existe d’autres :

- [n](https://www.npmjs.com/package/n#installation) est une alternative classique de `nvm`, qui remplit la même fonction avec des commandes légèrement différentes, et qui est installée via `npm` au lieu d’un script bash.
- [fnm](https://github.com/Schniz/fnm#using-a-script) est un gestionnaire de versions plus récent, supposé être beaucoup plus rapide que `nvm` (il utilise également [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/what-is-azure-pipelines?view=azure-devops)).
- [Volta](https://github.com/volta-cli/volta#installing-volta) est un nouveau gestionnaire de versions de l’équipe LinkedIn, présenté comme plus rapide et offrant une prise en charge multiplateforme.
- [asdf-VM](https://asdf-vm.com/#/core-manage-asdf-vm) est une interface de ligne de commande unique tout en un pour plusieurs langages tels que gvm, nvm, rbenv et pyenv (entre autres).
- [nvs](https://github.com/jasongin/nvs) (Node Version Switcher) est une alternative multiplateforme à `nvm`, offrant la possibilité d’[intégration avec VS Code](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md).

## <a name="install-your-favorite-code-editor"></a>Installer votre éditeur de code favori

Nous vous recommandons d’utiliser Visual Studio Code avec l’extension Remote-WSL pour les projets Node.js. Cela a pour effet de diviser VS Code en une architecture « client-serveur », avec le client (l’interface utilisateur VS Code) s’exécutant sur votre système d’exploitation Windows, et le serveur (votre code, Git, plug-in, etc.) s’exécutant « à distance » sur votre distribution Linux WSL. 

> [!NOTE]
> Ce scénario « distant » est un peu différent de ce à quoi vous pouvez être habitué. WSL prend en charge une distribution Linux réelle dans laquelle le code de votre projet est exécuté, indépendamment de votre système d’exploitation Windows, mais toujours sur votre ordinateur local. L’extension Remote-WSL se connecte à votre sous-système Linux comme s’il s’agissait d’un serveur distant, même s’il n’est pas en cours d’exécution dans le cloud. Il est toujours en cours d’exécution sur votre ordinateur local dans l’environnement WSL que vous avez activé pour s’exécuter avec Windows. 

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

## <a name="set-up-git-optional"></a>Configurer Git (facultatif)

Pour configurer Git pour un projet NodeJS sur WSL, consultez l’article [Commencer à utiliser Git sur le sous-système Windows pour Linux](https://docs.microsoft.com/windows/wsl/tutorials/wsl-git) dans la documentation WSL.

## <a name="next-steps"></a>Étapes suivantes

Vous disposez maintenant d’un environnement de développement Node.js configuré. Pour commencer à utiliser votre environnement Node.js, envisagez d’essayer l’un des tutoriels suivants :

- [Bien démarrer avec Node.js pour les débutants](./beginners.md)
- [Prise en main des frameworks web Node.js sous Windows](./web-frameworks.md)
- [Prise en main de la connexion des applications Node.js à une base de données](https://docs.microsoft.com/windows/wsl/tutorials/wsl-database)
- [Prise en main des conteneurs Docker avec Node.js](./containers.md)
