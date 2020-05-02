---
title: Prise en main des conteneurs Docker avec Node.js
description: Guide pas à pas pour vous aider à prendre en main l’utilisation des conteneurs Docker avec vos applications Node.js.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: ''
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 9467224814b1e26f18031662f5e8d994a8fae1ac
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75683672"
---
# <a name="get-started-using-docker-containers-with-nodejs"></a>Prise en main des conteneurs Docker avec Node.js

Guide pas à pas pour vous aider à prendre en main l’utilisation des conteneurs Docker avec vos applications Node.js.

## <a name="prerequisites"></a>Prérequis

Ce guide part du principe que vous avez déjà suivi les étapes de [configuration de votre environnement de développement Node.js avec WSL 2](./setup-on-wsl2.md), y compris les étapes suivantes :

- Installer Windows 10 Insider Preview, build 18932 ou ultérieure.
- Activez la fonctionnalité WSL 2 sur Windows.
- Installez une distribution Linux (Ubuntu 18.04 dans les exemples). Pour le vérifier : `wsl lsb_release -a`.
- Assurez-vous que votre distribution Ubuntu 18.04 s’exécute en mode WSL 2. (WSL peut exécuter des distributions en mode v1 ou v2.) Pour le vérifier, ouvrez PowerShell et entrez : `wsl -l -v`.
- À l’aide de PowerShell, définissez Ubuntu 18.04 en tant que distribution par défaut, avec : `wsl -s ubuntu 18.04`.

## <a name="overview-of-docker-containers"></a>Vue d’ensemble des conteneurs Docker

**Docker** est un outil utilisé pour créer, déployer et exécuter des applications à l’aide de conteneurs. Les conteneurs permettent aux développeurs de créer un package d’application avec tous les éléments nécessaires (bibliothèques, frameworks, dépendances, etc.) et de le livrer comme un seul package. L’utilisation d’un conteneur garantit que l’application s’exécute de la même manière quels que soient les paramètres personnalisés ou les bibliothèques précédemment installées sur l’ordinateur qui l’exécute, qui peuvent être différents de ceux de l’ordinateur utilisé pour écrire et tester le code de l’application. Cela permet aux développeurs de se concentrer sur l’écriture de code sans se soucier du système sur lequel le code sera exécuté.

Les conteneurs Docker sont similaires aux machines virtuelles, mais ne créent pas un système d’exploitation virtuel complet. Au lieu de cela, Docker permet à l’application d’utiliser le même noyau Linux que le système sur lequel elle s’exécute. Le package de l’application peut ainsi demander uniquement les éléments qui ne sont pas déjà sur l’ordinateur hôte, ce qui réduit la taille du package et améliore les performances.

La disponibilité continue, à l’aide de conteneurs d’ancrage avec des outils tels que [Kubernetes](https://docs.microsoft.com/azure/aks/), est une autre raison pour la popularité des conteneurs. Cela permet de créer plusieurs versions de votre conteneur d’application à des moments différents. Au lieu de devoir mettre en place un système complet pour les mises à jour ou la maintenance, chaque conteneur (et ses microservices spécifiques) peut être remplacé à la volée. Vous pouvez préparer un nouveau conteneur avec toutes vos mises à jour, configurer le conteneur pour la production et pointer simplement vers le nouveau conteneur une fois qu’il est prêt. Vous pouvez également archiver différentes versions de votre application à l’aide de conteneurs et, si nécessaire, les laisser s’exécuter comme filet de sécurité.

## <a name="install-docker-desktop-wsl-2-tech-preview"></a>Installer Docker Desktop WSL 2 Tech Preview

WSL 1 ne pouvait pas exécuter directement le démon Docker. Ce n’est plus le cas avec WSL 2, ce qui a entraîné des améliorations notables de la vitesse et des performances avec Docker Desktop pour WSL 2.

Pour installer et exécuter Docker Desktop WSL 2 Tech Preview :

1. Télécharger le [programme d’installation Docker Desktop WSL 2 Tech Preview](https://download.docker.com/win/edge/36883/Docker%20Desktop%20Installer.exe). (Vous pouvez référencer la [documentation du programme d’installation](https://docs.docker.com/docker-for-windows/wsl-tech-preview/) si nécessaire.)

2. Ouvrez le programme d’installation de Docker que vous venez de télécharger. L’Assistant d’installation vous demande si vous souhaitez « Utiliser des conteneurs Windows au lieu de conteneurs Linux ». N’activez pas cette option, car nous utiliserons le sous-système Linux. Docker sera installé dans un répertoire géré dans votre distribution WSL 2 par défaut et inclura le démon Docker, l’interface de ligne de commande et Compose CLI.

    ![Démarrage de Docker Desktop](../images/install-docker-1.png)

3. Si vous n’avez pas encore d’ID Docker, vous devez en créer un en visitant : [https://hub.docker.com/signup](https://hub.docker.com/signup). Votre ID doit entièrement être composé de caractères alphanumériques en minuscules.

4. Une fois l’installation terminée, démarrez Docker Desktop en sélectionnant l’icône de raccourci sur votre bureau ou en le recherchant dans le menu Démarrer de Windows. L’icône Docker s’affiche dans le menu des icônes masquées de votre barre des tâches. Cliquez avec le bouton droit sur l’icône pour afficher le menu des commandes Docker et sélectionnez « WSL 2 Tech Preview ».

5. Une fois que la fenêtre Tech Preview s’ouvre, sélectionnez **Démarrer** pour commencer à exécuter le démon Docker (processus en arrière-plan) dans WSL 2. Quand le démon Docker WSL 2 démarre, un contexte de l’interface de ligne de commande de Docker est automatiquement créé pour lui.

    ![Démarrage de Docker Desktop](../images/start-docker.gif)

6. Pour confirmer que Docker a été installé et afficher le numéro de version, ouvrez une ligne de commande (WSL ou PowerShell) et entrez : `docker --version`

7. Vérifiez que votre installation fonctionne bien en exécutant une image Docker intégrée simple : `docker run hello-world`

Voici quelques commandes Docker que vous devez connaître :

- Répertoriez les commandes disponibles dans l’interface de ligne de commande de Docker en entrant : `docker`
- Répertoriez les informations pour une commande spécifique avec : `docker <COMMAND> --help`
- Répertoriez les images de Docker sur votre ordinateur (il s’agit simplement l’image Hello World à ce stade) avec : `docker image ls --all`
- Répertoriez les conteneurs sur votre ordinateur avec : `docker container ls --all`
- Répertoriez les statistiques système et les ressources Docker (processeur et mémoire) qui sont à votre disposition dans le contexte WSL 2 avec : `docker info`
- Affichez l’emplacement d’exécution actuel de Docker avec : `docker context ls`

Vous pouvez voir que Docker est en cours d’exécution dans deux contextes, à savoir `default` (le démon Docker classique) et `wsl` (notre recommandation, avec Tech Preview). (De plus, la commande `ls` est la forme abrégée de `list` et peut être utilisée de façon interchangeable.)

![Contexte d’affichage de Docker dans PowerShell](../images/docker-context.png)

> [!TIP]
> <Essayez de créer un exemple d’image Docker avec ce [tutoriel sur Docker Hub](https://hub.docker.com/?overlay=onboarding). Docker Hub contient également des milliers d’images open source qui peuvent correspondre au type d’application pour lequel vous souhaitez créer des conteneurs. Vous pouvez télécharger des images, telles que ce [conteneur de framework Gatsby.js](https://hub.docker.com/r/gatsbyjs/gatsby-dev-builds) ou ce [conteneur de framework Nuxt.js](https://hub.docker.com/r/hobord/nuxtexpress), et les étendre avec votre propre code d’application. Vous pouvez effectuer une recherche dans le registre à l’aide de [Docker à partir de votre ligne de commande](https://docs.docker.com/engine/reference/commandline/search/) ou du [site web de Docker Hub](https://hub.docker.com/search/?type=image).

## <a name="install-the-docker-extension-on-vs-code"></a>Installer l’extension Docker sur VS Code

L’extension Docker facilite la création, la gestion et le déploiement d’applications en conteneur à partir de Visual Studio Code.

1. Ouvrez la fenêtre **Extensions** (Ctrl + Maj + X) dans VS Code et recherchez **Docker**.

2. Sélectionnez l’[extension Microsoft Docker](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker) et **installez**. Vous devrez recharger VS Code après l’installation pour activer l’extension.

    ![Extension Docker sur VS Code dans Remote-WSL](../images/docker-vscode-extension.png)

En installant l’extension Docker sur VS Code, vous pouvez désormais afficher une liste de commandes `Dockerfile` utilisée dans la section suivante avec le raccourci : `Ctrl+Space`

Découvrez plus d'informations sur l'[utilisation de Docker dans VS Code](https://code.visualstudio.com/docs/azure/docker).

## <a name="create-a-container-image-with-dockerfile"></a>Créer une image conteneur avec DockerFile

Une **image de conteneur** stocke votre code d’application, les bibliothèques, les fichiers de configuration, les variables d’environnement et le runtime. L’utilisation d’une image garantit que l’environnement de votre conteneur est standardisé et contient uniquement ce qui est nécessaire pour créer et exécuter votre application.

Un fichier **Dockerfile** contient les instructions nécessaires pour créer une image de conteneur. En d’autres termes, ce fichier génère l’image de conteneur qui définit l’environnement de votre application afin qu’il puisse être reproduit n’importe où.

Créons une image de conteneur à l’aide de l’application Next.js configurée dans le guide de [frameworks web](./web-frameworks.md).

1. Ouvrez votre application Next.js dans VS Code (en veillant à ce que Remote-WSL s’exécute comme indiqué dans l’onglet vert en bas à gauche). Ouvrez le terminal WSL intégré dans VS Code (**Afficher > Terminal**) et vérifiez que le chemin du terminal est pointé vers votre répertoire de projet Next.js (c.-à-d. `~/NextProjects/my-next-app$`).

2. Créez un fichier appelé `Dockerfile` à la racine de votre projet Next.js et ajoutez ce qui suit :

    ```docker
    # Specifies where to get the base image (Node v12 in our case) and creates a new container for it
    FROM node:12
    
    # Set working directory. Paths will be relative this WORKDIR.
    WORKDIR /usr/src/app
    
    # Install dependencies
    COPY package*.json ./
    RUN npm install
    
    # Copy source files from host computer to the container
    COPY . .
    
    # Build the app
    RUN npm run build
    
    # Specify port app runs on
    EXPOSE 3000

    # Run the app
    CMD [ "npm", "start" ]
    ```

3. Pour créer l’image Docker, exécutez la commande suivante à partir de la racine de votre projet (en remplaçant `<your_docker_username>` par le nom d’utilisateur que vous avez créé sur Docker Hub) : `docker build -t <your_docker_username>/my-nextjs-app .`

> [!NOTE]
> Pour que cette commande fonctionne, Docker doit s’exécuter avec WSL Tech Preview. Pour vous rappeler comment démarrer Docker, consultez l’[étape 4](#install-docker-desktop-wsl-2-tech-preview) de la section d’installation. L’indicateur `-t` spécifie le nom de l’image à créer, dans notre cas « my-nextjs-app:v1 ». Nous vous recommandons de toujours [utiliser un numéro de version sur vos noms de balises](https://medium.com/@mccode/the-misunderstood-docker-tag-latest-af3babfd6375) lors de la création d’une image. Veillez à inclure le point à la fin de la commande. Il spécifie que le répertoire de travail actuel doit être utilisé pour rechercher et copier les fichiers de build de votre application Next.js.

4. Pour exécuter cette nouvelle image Docker de votre application Next.js dans un conteneur, entrez la commande : `docker run -d -p 3333:3000 <your_docker_username>/my-nextjs-app:v1`

5. La balise `-p` lie le port « 3000 » (le port sur lequel l’application s’exécute à l’intérieur du conteneur) au port local « 3333 » sur votre ordinateur. Vous pouvez donc maintenant pointer votre navigateur web vers [http://localhost:3333](http://localhost:3333) et voir votre application Next.js affichée côté serveur qui s’exécute en tant qu’image de conteneur Docker.

> [!TIP]
> Nous avons créé notre image de conteneur avec `FROM node:12`, qui référence l’image par défaut de la version 12 de Node.js stockée sur Docker Hub. Cette image Node.js par défaut est basée sur un système Debian/Ubuntu. Cependant, vous pouvez choisir entre beaucoup d’images Node.js différentes et vouloir envisager quelque chose de plus léger ou de mieux adapté à vos besoins. Pour en savoir plus, consultez le [Registre d’images Node.js sur le Docker Hub](https://hub.docker.com/_/node/).

## <a name="upload-your-container-image-to-a-repository"></a>Charger votre image conteneur dans un référentiel

Un **référentiel de conteneurs** stocke votre image de conteneur dans le cloud. Souvent, un référentiel de conteneur contient en fait une collection d’images associées, telles que des versions différentes, qui sont toutes disponibles pour une configuration facile et un déploiement rapide. En règle générale, vous pouvez accéder à des images sur des référentiels de conteneurs via des points de terminaison HTTPS sécurisés, ce qui vous permet d’extraire, de transmettre ou de gérer des images quels que soient le système, le matériel ou l’instance de machine virtuelle.

D’autre part, un **Registre de conteneur** stocke une collection de référentiels tels que des index, des règles de contrôle d’accès et des chemins d’API. Ils peuvent être hébergés publiquement ou en privé. [Docker Hub](https://hub.docker.com/) est un Registre Docker open source et le service par défaut utilisé lors de l’exécution des commandes `docker push` et `docker pull`. Il est gratuit pour les référentiels publics et payant pour les référentiels privés.

Pour charger votre nouvelle image de conteneur sur un référentiel hébergé sur Docker Hub :

1. Connectez-vous à Docker Hub. Vous serez invité à entrer le nom d’utilisateur et le mot de passe que vous avez utilisés pour créer votre compte Docker Hub lors de l’étape d’installation. Pour vous connecter à Docker sur votre terminal, entrez : `docker login`

2. Pour obtenir une liste des images de conteneur Docker que vous avez créées sur votre ordinateur, entrez : `docker image ls --all`

3. Transmettez votre image de conteneur à Docker Hub en créant pour elle un référentiel à l’aide de cette commande : `docker push <your_docker_username>/my-nextjs-app:v1`

4. Vous pouvez maintenant afficher votre référentiel sur Docker Hub, entrer une description et lier votre compte GitHub (si vous le souhaitez), en visitant : https://cloud.docker.com/repository/list

5. Vous pouvez également afficher une liste de vos conteneurs Docker actifs avec : `docker container ls` (ou `docker ps`)

6. Vous devez voir que votre conteneur « my-nextjs-app:v1 » est actif sur le port 3333 -> 3000/tcp. Vous pouvez également voir votre « ID DE CONTENEUR » répertorié ici. Pour arrêter l’exécution de votre conteneur, entrez la commande : `docker stop <container ID>`

7. Généralement, une fois qu’un conteneur est arrêté, il doit également être supprimé. La suppression d’un conteneur nettoie toutes les ressources qu’il laisse. Une fois que vous avez supprimé un conteneur, toutes les modifications que vous avez apportées à son système de fichiers image sont définitivement perdues. Vous devrez créer une image pour représenter les modifications. Pour supprimer votre conteneur, utilisez la commande : `docker rm <container ID>`

En savoir plus sur la [création d’une application web conteneurisée avec Docker](https://docs.microsoft.com/learn/modules/intro-to-containers/).

## <a name="deploy-to-azure-container-registry"></a>Déployer sur Azure Container Registry

[**Azure Container Registry**](https://azure.microsoft.com/services/container-registry/) (ACR) vous permet de stocker, de gérer et de conserver vos images de conteneur en toute sécurité dans des référentiels privés et authentifiés. Compatible avec les commandes Docker standard, ACR peut gérer pour vous des tâches critiques, telles que la maintenance et la surveillance de l’intégrité des conteneurs et le couplage avec [Kubernetes](https://docs.microsoft.com/azure/aks/intro-kubernetes) pour créer des systèmes d’orchestration évolutifs. Créez à la demande, ou automatisez entièrement les builds avec des déclencheurs tels que les validations de code source et des mises à jour de l’image de base. ACR tire également partie du vaste réseau cloud Azure pour gérer la latence réseau, les déploiements globaux, et créer une expérience native transparente pour toute personne utilisant [Azure App Service](https://docs.microsoft.com/azure/app-service/) (pour l’hébergement web, les back-ends mobiles, les API REST) ou [d’autres services cloud Azure](https://azure.microsoft.com/product-categories/containers/).

> [!IMPORTANT]
> Vous avez besoin de votre propre abonnement Azure pour déployer un conteneur sur Azure et cela peut vous être facturé. Si vous n’avez pas encore d’abonnement Azure, [créez un compte gratuit](https://azure.microsoft.com/free/) avant de commencer.

Pour obtenir de l’aide lors de la création d’un Registre de conteneurs Azure et déployer votre image de conteneur d’application, consultez l’exercice : [Déployer une image Docker sur une instance de conteneur Azure](https://docs.microsoft.com/learn/modules/intro-to-containers/7-exercise-deploy-docker-image-to-container-instance).

## <a name="additional-resources"></a>Ressources supplémentaires

- [Node.js sur Azure](https://azure.microsoft.com/develop/nodejs/)
- Démarrage rapide : [Créer une application web Node.js dans Azure](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-nodejs)
- Cours en ligne : [Gérer des conteneurs dans Azure](https://docs.microsoft.com/learn/paths/administer-containers-in-azure/)
- Utilisation de VS Code : [Utilisation avec Docker](https://code.visualstudio.com/docs/azure/docker)
- Documentation de Docker : [Docker Desktop WSL 2 Tech Preview](https://docs.docker.com/docker-for-windows/wsl-tech-preview/)
