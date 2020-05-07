---
title: Configurer NodeJS dans un environnement Windows natif
description: Guide conçu pour vous aider à configurer votre environnement de développement Node.js directement sous Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Node.js, windows 10, environnement windows natif, directement sous windows
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: fe1943da8c1de4f4fced5dec67079522d83f9a19
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82173465"
---
# <a name="set-up-your-nodejs-development-environment-directly-on-windows"></a>Configurer votre environnement de développement Node.js directement sous Windows

Vous trouverez ci-dessous un guide pas à pas qui vous permettra de vous familiariser avec Node.js dans un environnement de développement Windows natif.

## <a name="install-nvm-windows-nodejs-and-npm"></a>Installer nvm-windows, node.js et npm

Différentes méthodes sont disponibles pour installer Node.js. Nous vous recommandons d'utiliser un gestionnaire de versions car les versions changent très rapidement. Vous devrez probablement passer d'une version à une autre en fonction des besoins des différents projets sur lesquels vous travaillez. Node Version Manager, également appelé nvm, est la méthode la plus couramment utilisée pour installer plusieurs versions de Node.js, mais elle est uniquement disponible sous Mac/Linux ; elle n'est pas prise en charge par Windows. Nous allons donc suivre les étapes d'installation de l'outil nvm-windows, que nous utiliserons pour installer Node.js et Node Package Manager (npm). D'[autres gestionnaires de versions](#alternative-version-managers) sont également à prendre en compte. Ceux-ci sont couverts à la section suivante.

> [!IMPORTANT]
> Veillez à supprimer toutes les installations existantes de Node.js ou de npm de votre système d'exploitation avant d'installer un gestionnaire de versions car les différents types d'installation peuvent entraîner des conflits. Cela inclut la suppression de tous les répertoires d'installation existants de nodejs (par exemple, « C:\Program Files\nodejs ») qui pourraient subsister. Le lien symbolique généré par NVM n'écrasera pas un répertoire d'installation existant (même vide). Pour obtenir de l'aide sur la suppression des installations précédentes, consultez [Supprimer complètement node.js de Windows](https://stackoverflow.com/questions/20711240/how-to-completely-remove-node-js-from-windows).

1. Ouvrez le [référentiel windows-nvm](https://github.com/coreybutler/nvm-windows#node-version-manager-nvm-for-windows) dans votre navigateur Internet et sélectionnez le lien **Télécharger maintenant**.
2. Téléchargez le fichier **nvm-setup.zip** de la version la plus récente.
3. Une fois le fichier zip téléchargé, ouvrez-le, puis ouvrez le fichier **nvm-setup.exe**.
4. L'Assistant d'installation Setup-NVM-for-Windows vous guide tout au long du processus d'installation, y compris pour le choix du répertoire dans lequel nvm-windows et Node.js doivent être installés.

    ![Assistant d'installation de NVM pour Windows](../images/install-nvm-for-windows-wizard.png)

5. Au terme de l'installation, ouvrez PowerShell et essayez d'utiliser windows-nvm pour répertorier les versions de Node actuellement installées (il ne doit y en avoir aucune à ce stade) : `nvm ls`

    ![Liste NVM ne montrant aucune version de Node](../images/windows-nvm-powershell-no-node.png)

6. Installez la version actuelle de Node.js (pour tester les dernières améliorations apportées aux fonctionnalités, par exemple) : `nvm install latest`

7. Installez la dernière version LTS stable de Node.js (recommandé) en recherchant d'abord le numéro de la version LTS actuelle, `nvm list available`, puis en installant le numéro de la version LTS avec `nvm install <version>` (en remplaçant `<version>` par le numéro, à savoir : `nvm install 12.14.0`).

    ![Liste NVM des versions disponibles](../images/windows-nvm-list.png)

8. Répertoriez les versions de Node installées : `nvm ls`... Les deux versions que vous venez d'installer doivent maintenant être répertoriées.

    ![Liste NVM répertoriant les versions de Node installées](../images/windows-nvm-node-installs.png)

9. Après avoir installé les versions de Node.js dont vous avez besoin, sélectionnez celle que vous souhaitez utiliser en entrant `nvm use <version>` (en remplaçant `<version>` par le numéro, par exemple : `nvm use 12.9.0`).

10. Pour changer la version de Node.js à utiliser pour un projet, créez un nouveau répertoire de projet, `mkdir NodeTest`, accédez au répertoire, `cd NodeTest`, puis entrez `nvm use <version>` en remplaçant `<version>` par le numéro de la version que vous souhaitez utiliser (à savoir v10.16.3`).

11. Pour savoir quelle version de npm est installée, entrez : `npm --version`. Ce numéro de version sera automatiquement remplacé par la version de npm associée à votre version actuelle de Node.js.

## <a name="alternative-version-managers"></a>Autres gestionnaires de versions

windows-nvm est actuellement le gestionnaire de versions le plus couramment utilisé pour Node, mais ce n'est pas le seul :

- [nvs](https://github.com/jasongin/nvs) (Node Version Switcher) est une alternative multiplateforme à `nvm`, avec possibilité d'[intégration à VS Code](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md).

- [Volta](https://github.com/volta-cli/volta#installing-volta) est un nouveau gestionnaire de versions de l'équipe LinkedIn présenté comme plus rapide et offrant une prise en charge multiplateforme.

Pour installer Volta en tant que gestionnaire de versions (plutôt que windows-nvm), accédez à la section **Installation sous Windows** de son [Guide de démarrage](https://docs.volta.sh/guide/getting-started), puis téléchargez et exécutez le programme d'installation Windows, en suivant les instructions d'installation.

> [!IMPORTANT]
> Avant d'installer Volta, vous devez vous assurer que le [Mode développeur](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#accessing-settings-for-developers) est activé sur votre ordinateur Windows.

Pour en savoir plus sur l'utilisation de Volta pour installer plusieurs versions de Node.js sous Windows, consultez la [documentation Volta](https://docs.volta.sh/guide/understanding#managing-your-toolchain).

## <a name="install-your-favorite-code-editor"></a>Installer votre éditeur de code favori

Nous vous recommandons d'[installer VS Code](https://code.visualstudio.com), ainsi que le [pack d'extension Node.js](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack), pour développer avec Node.js sous Windows. Installez tout ou choisissez ce qui vous semble le plus utile.

Pour installer le pack d'extension Node.js :

1. Ouvrez la fenêtre **Extensions** (Ctrl + Maj + X) dans VS Code.
2. Dans la zone de recherche du haut de la fenêtre Extensions, entrez : « Pack d'extension Node » (ou le nom de l'extension que vous recherchez).
3. Sélectionnez **Installer**. Une fois installée, votre extension apparaîtra dans le dossier « Activé » de votre fenêtre **Extensions**. Vous pouvez désactiver, désinstaller ou configurer les paramètres en sélectionnant l'icône d'engrenage en regard de la description de votre nouvelle extension.

D'autres extensions sont disponibles :

- [Debugger pour Chrome](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code) : lorsque vous aurez terminé le développement côté serveur avec Node.js, vous devrez développer et tester le côté client. Cette extension intègre votre éditeur VS Code au service de débogage de votre navigateur Chrome, pour plus d'efficacité.
- [Mappages de touches d'autres éditeurs](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads) : ces extensions peuvent être utiles si vous utilisiez auparavant un autre éditeur de texte (comme Atom, Sublime, Vim, eMacs, Notepad++, etc.).
- [Synchronisation des paramètres](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync) : vous permet de synchroniser vos paramètres VS Code entre différentes installations à l'aide de GitHub. Si vous travaillez sur plusieurs ordinateurs, cela permet de garantir la cohérence de votre environnement.

## <a name="install-git-optional"></a>Installer Git (facultatif)

Si vous envisagez de collaborer avec d'autres personnes ou d'héberger votre projet sur un site open source (comme GitHub), VS Code prend en charge le [contrôle de version avec Git](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support). L’onglet Contrôle de code source de VS Code assure le suivi de toutes vos modifications et contient des commandes Git courantes (ajouter, valider, pousser, extraire) intégrées directement dans l’interface utilisateur. Vous devez d’abord installer Git pour gérer le panneau Contrôle de code source.

1. Téléchargez et installez Git pour Windows sur le [site web git-scm](https://git-scm.com/download/win).

2. Vous trouverez un assistant d’installation qui vous posera une série de questions sur les paramètres de votre installation Git. Nous vous recommandons d’utiliser tous les paramètres par défaut, sauf si vous avez une raison particulière de les modifier.

3. Si vous n’avez jamais travaillé avec Git, les [guides GitHub](https://guides.github.com/) peuvent vous aider à démarrer.

4. Nous vous recommandons d'ajouter un [fichier .gitignore](https://help.github.com/en/articles/ignoring-files) à vos projets Node. Voici le [modèle gitignore par défaut de GitHub pour Node.js](https://github.com/github/gitignore/blob/master/Node.gitignore).

## <a name="use-windows-subsystem-for-linux-for-production"></a>Utiliser le Sous-système Windows pour Linux à des fins de production

L'utilisation directe de Node.js sous Windows est idéale pour l'apprentissage et l'expérimentation des possibilités qui s'offrent à vous. Si vous souhaitez créer des applications web prêtes pour la production, lesquelles sont généralement déployées sur un serveur Linux, nous vous recommandons d'utiliser le Sous-système Windows pour Linux version 2 (WSL 2) afin de développer des applications web Node.js. De nombreux packages et frameworks Node.js sont créés avec un environnement *nix à l'esprit, et la plupart des applications Node.js sont déployées sous Linux. Par conséquent, le développement sous WSL garantit la cohérence entre vos environnements de développement et de production. Pour configurer un environnement de développement WSL, consultez [Configurer votre environnement de développement Node.js avec WSL 2](./setup-on-wsl2.md).

> [!NOTE]
> Si vous devez héberger une application Node.js sur un serveur Windows (ce qui est peu fréquent), le scénario le plus courant semble être l'[utilisation d'un proxy inverse](https://medium.com/intrinsic/why-should-i-use-a-reverse-proxy-if-node-js-is-production-ready-5a079408b2ca). Deux méthodes sont disponibles : 1) [utilisation d'iisnode](https://harveywilliams.net/blog/installing-iisnode) ou [méthode directe](https://dev.to/petereysermans/hosting-a-node-js-application-on-windows-with-iis-as-reverse-proxy-397b). Nous ne fournissons pas ces ressources et vous recommandons d'[utiliser des serveurs Linux pour héberger vos applications Node.js](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-nodejs).
