---
title: Configurer NodeJS sur des fenêtres natives
description: Guide pour vous aider à configurer votre environnement de développement node. js directement sur Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Node. js, Windows 10, Windows natif, directement sur Windows
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 18a8d07f790c391a6e10577ff512347106e1cf21
ms.sourcegitcommit: 60d2d15dd0d365f82e4e90e4bc34b40cf5b4a247
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517831"
---
# <a name="set-up-your-nodejs-development-environment-directly-on-windows"></a>Configurer votre environnement de développement node. js directement sur Windows

Vous trouverez ci-dessous un guide pas à pas pour vous familiariser avec node. js dans un environnement de développement Windows natif.

> [!NOTE]
> Bien que l’utilisation de node. js sur Windows soit certainement une option viable, nous vous recommandons généralement d’utiliser le sous-système Windows pour Linux (WSL) pour le développement d’applications Web node. js. De nombreux packages et infrastructures node. js sont créés avec un environnement * nix à l’esprit, et la plupart des applications node. js étant déployées sur Linux, le développement sur WSL garantit la cohérence entre vos environnements de développement et de production. Pour configurer un environnement de développement WSL, consultez [configurer votre environnement de développement node. js avec WSL 2](./setup-on-wsl2.md).

## <a name="install-nvm-windows-nodejs-and-npm"></a>Installer NVM-Windows, node. js et NPM

Il existe plusieurs façons d’installer node. js. Nous vous recommandons d’utiliser un gestionnaire de version au fur et à mesure que les versions changent très rapidement. Vous devrez probablement basculer entre plusieurs versions en fonction des besoins des différents projets sur lesquels vous travaillez. Node version Manager, plus communément appelé NVM, est la façon la plus courante d’installer plusieurs versions de node. js, mais n’est disponible que pour Mac/Linux et n’est pas prise en charge sur Windows. Au lieu de cela, nous allons parcourir les étapes d’installation de NVM-Windows, puis l’utiliser pour installer node. js et node Package Manager (NPM). D' [autres gestionnaires de versions](#alternative-version-managers) sont également pris en compte dans la section suivante.

> [!IMPORTANT]
> Il est toujours recommandé de supprimer les installations existantes de node. js ou NPM de votre système d’exploitation avant d’installer un gestionnaire de version, car les différents types d’installation peuvent entraîner des conflits étranges et confus. Cela comprend la suppression des répertoires d’installation NodeJS existants (par exemple, « C:\Program Files\nodejs ») qui peuvent être conservés. Le lien symbolique généré par NVM ne remplacera pas un répertoire d’installation existant (même vide). Pour obtenir de l’aide sur la suppression des installations précédentes, consultez [Comment supprimer complètement node. js de Windows](https://stackoverflow.com/questions/20711240/how-to-completely-remove-node-js-from-windows).)

1. Ouvrez le [référentiel Windows-NVM](https://github.com/coreybutler/nvm-windows#node-version-manager-nvm-for-windows) dans votre navigateur Internet, puis cliquez sur le lien **Télécharger maintenant** .
2. Téléchargez le fichier **NVM-Setup. zip** pour la version la plus récente.
3. Une fois téléchargé, ouvrez le fichier zip, puis ouvrez le fichier **NVM-Setup. exe** .
4. L’Assistant Installation de configuration-NVM-pour-Windows vous guide tout au long des étapes de configuration, notamment le choix du répertoire dans lequel NVM-Windows et node. js seront installés.

    ![Assistant Installation de NVM for Windows](../images/install-nvm-for-windows-wizard.png)

5. Une fois l’installation terminée. Ouvrez PowerShell et essayez d’utiliser Windows-NVM pour répertorier les versions de nœud actuellement installées (ne doit pas être à ce stade) : `nvm ls`

    ![Liste NVM qui n’indique aucune version de nœud](../images/windows-nvm-powershell-no-node.png)

6. Installer la version actuelle de node. js (pour tester les améliorations apportées aux fonctionnalités les plus récentes, mais plus susceptible d’avoir des problèmes que la version LTS) : `nvm install latest`
7. Installez la dernière version stable de LTS de node. js (recommandé) en examinant d’abord le numéro de version LTS actuel : `nvm list available`, puis en installant le numéro de version LTS avec : `nvm install <version>` (en remplaçant `<version>` par le nombre, IE : `nvm install 10.16.3`).

    ![Liste NVM des versions disponibles](../images/windows-nvm-list.png)

8. Répertorier les versions du nœud installées : `nvm ls`... vous devez maintenant voir les deux versions que vous venez d’installer.

    ![Liste NVM répertoriant les versions des nœuds installés](../images/windows-nvm-node-installs.png)

9. Pour vérifier quelle version de node. js est actuellement la version par défaut, entrez : `node --version`
10. Pour modifier la version de node. js que vous souhaitez utiliser pour un projet, créez un répertoire de projet `mkdir NodeTest`, puis entrez le répertoire `cd NodeTest`, puis entrez `nvm use <version>` en remplaçant `<version>` par le numéro de version que vous souhaitez utiliser (IE v 10.16.3 ').
11. Vérifiez quelle version de NPM est installée avec : `npm --version`, ce numéro de version sera automatiquement modifié en fonction de la version de NPM associée à votre version actuelle de node. js.

## <a name="alternative-version-managers"></a>Gestionnaires de versions alternatifs

Alors que Windows-NVM est actuellement le gestionnaire de version le plus populaire pour node, il existe des alternatives à prendre en compte :

- [NVS](https://github.com/jasongin/nvs) (node version changer) est une alternative multiplateforme `nvm` avec la possibilité d' [intégration à vs code](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md).
- 
- [Volta](https://github.com/volta-cli/volta#installing-volta) est un nouveau gestionnaire de versions de l’équipe LinkedIn qui prétend améliorer la vitesse et la prise en charge multiplateforme.

Pour installer Volta comme gestionnaire de version (plutôt que Windows-NVM), accédez à la section **installation de Windows** de leur guide d' [prise en main](https://docs.volta.sh/guide/getting-started), puis téléchargez et exécutez le programme d’installation de Windows, en suivant les instructions d’installation.

> [!IMPORTANT]
> Vous devez vous assurer que le [mode développeur](https://docs.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development#accessing-settings-for-developers) est activé sur votre ordinateur Windows avant d’installer Volta.

Pour en savoir plus sur l’utilisation de Volta pour installer plusieurs versions de node. js sur Windows, consultez la [documentation Volta](https://docs.volta.sh/guide/understanding#managing-your-toolchain).

## <a name="install-your-favorite-code-editor"></a>Installer votre éditeur de code favori

Nous vous recommandons d' [installer vs code](https://code.visualstudio.com), ainsi que le [Pack d’extension node. js](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack), pour le développement avec node. js sur Windows. Installez-les tous ou choisissez ce qui vous semble le plus utile.

Pour installer le pack d’extension node. js :

1. Ouvrez la fenêtre **Extensions** (Ctrl + Maj + X) dans vs code.
2. Dans la zone de recherche en haut de la fenêtre extensions, entrez « node extension Pack » (ou le nom de l’extension que vous recherchez).
3. Sélectionnez **installer**. Une fois l’installation terminée, votre extension s’affiche dans le dossier « activé » de votre fenêtre **Extensions** . Vous pouvez désactiver, désinstaller ou configurer des paramètres en sélectionnant l’icône d’engrenage en regard de la description de votre nouvelle extension.

Voici quelques-unes des extensions supplémentaires que vous pouvez envisager :

- [Débogueur pour Chrome](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code): une fois que vous avez terminé le développement côté serveur avec node. js, vous devez développer et tester le côté client. Cette extension intègre votre éditeur de VS Code avec votre service de débogage de navigateur Chrome, ce qui rend les choses un peu plus efficaces.
- [Cartes à partir d’autres éditeurs](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads): ces extensions peuvent vous aider dans votre environnement à la racine si vous effectuez une transition à partir d’un autre éditeur de texte (comme Atom, Subvert, vim, EMacs, Notepad + +, etc.).
- [Synchronisation des paramètres](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync): vous permet de synchroniser vos paramètres de vs code sur différentes installations à l’aide de github. Si vous travaillez sur des ordinateurs différents, cela permet de garantir la cohérence de votre environnement entre eux.

## <a name="install-git-optional"></a>Installer Git (facultatif)

Si vous envisagez de collaborer avec d’autres personnes ou d’héberger votre projet sur un site Open source (comme GitHub), VS Code prend en charge [le contrôle de version avec git](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support). L’onglet contrôle de code source de VS Code effectue le suivi de toutes vos modifications et contient des commandes git communes (ajouter, valider, pousser, tirer) intégrées directement dans l’interface utilisateur. Vous devez d’abord installer Git pour alimenter le panneau de configuration de la source.

1. Téléchargez et installez git pour Windows à partir [du site Web git-SCM](https://git-scm.com/download/win).

2. Vous trouverez un assistant d’installation qui vous posera une série de questions sur les paramètres de votre installation git. Nous vous recommandons d’utiliser tous les paramètres par défaut, sauf si vous avez une raison particulière de modifier un objet.

3. Si vous n’avez jamais travaillé avec git, les [guides GitHub](https://guides.github.com/) peuvent vous aider à commencer.

4. Nous vous recommandons d’ajouter un [fichier. gitignore](https://help.github.com/en/articles/ignoring-files) à vos projets de nœud. Voici le [modèle gitignore par défaut de GitHub pour node. js](https://github.com/github/gitignore/blob/master/Node.gitignore).
