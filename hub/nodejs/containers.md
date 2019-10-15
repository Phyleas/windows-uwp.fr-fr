---
title: Prise en main des conteneurs de l’arrimeur avec node. js
description: Guide pas à pas pour vous aider à prendre en main les conteneurs de l’arrimeur avec vos applications node. js.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: ''
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 16b1421606d3c8271141256b80ae2600ec9ca49d
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72315123"
---
# <a name="get-started-using-docker-containers-with-nodejs"></a>Prise en main des conteneurs de l’arrimeur avec node. js

Guide pas à pas pour vous aider à prendre en main les conteneurs de l’arrimeur avec vos applications node. js.

## <a name="prerequisites"></a>Prérequis

Ce guide part du principe que vous avez déjà effectué les étapes pour [configurer votre environnement de développement node. js avec WSL 2](./setup-on-wsl2.md), notamment :

- Installez Windows 10 Insider preview version 18932 ou ultérieure.
- Activez la fonctionnalité WSL 2 sur Windows.
- Installez une distribution Linux (Ubuntu 18,04 pour nos exemples). Vous pouvez le vérifier avec : `wsl lsb_release -a`.
- Assurez-vous que votre distribution Ubuntu 18,04 s’exécute en mode WSL 2. (WSL peut exécuter des distributions en mode v1 ou v2.) Vous pouvez vérifier cela en ouvrant PowerShell et en entrant : `wsl -l -v`.
- À l’aide de PowerShell, définissez Ubuntu 18,04 comme distribution par défaut, avec : `wsl -s ubuntu 18.04`.

## <a name="overview-of-docker-containers"></a>Vue d’ensemble des conteneurs d’ancrage

La **station d’accueil** est un outil utilisé pour créer, déployer et exécuter des applications à l’aide de conteneurs. Les conteneurs permettent aux développeurs d’empaqueter une application avec tous les éléments dont elle a besoin (bibliothèques, infrastructures, dépendances, etc.) et de la livrer comme un seul package. L’utilisation d’un conteneur garantit que l’application s’exécutera de la même manière, quels que soient les paramètres personnalisés ou les bibliothèques précédemment installées sur l’ordinateur qui l’exécute, et qui pourrait différer de l’ordinateur utilisé pour écrire et tester le code de l’application. Cela permet aux développeurs de se concentrer sur l’écriture de code sans se soucier du système sur lequel s’exécute le code.

Les conteneurs d’ancrage sont similaires aux machines virtuelles, mais ne créent pas un système d’exploitation virtuel entier. Au lieu de cela, l’arrimeur permet à l’application d’utiliser le même noyau Linux que le système sur lequel elle s’exécute. Cela permet au package d’application d’exiger uniquement des parties qui ne sont pas déjà sur l’ordinateur hôte, ce qui réduit la taille du package et améliore les performances.

La disponibilité continue, à l’aide de conteneurs d’ancrage avec des outils tels que [Kubernetes](https://docs.microsoft.com/azure/aks/), est une autre raison de la popularité des conteneurs. Cela permet de créer plusieurs versions de votre conteneur d’application à des moments différents. Au lieu de devoir mettre en place un système entier pour les mises à jour ou la maintenance, chaque conteneur (et ses microservices) peut être remplacé à la volée. Vous pouvez préparer un nouveau conteneur avec toutes vos mises à jour, configurer le conteneur pour la production et pointer simplement vers le nouveau conteneur une fois qu’il est prêt. Vous pouvez également archiver différentes versions de votre application à l’aide de conteneurs et les maintenir en cours d’exécution comme un secours de sécurité, si nécessaire.

## <a name="install-docker-desktop-wsl-2-tech-preview"></a>Installer la version d’évaluation de la technologie de Desktop WSL 2

Auparavant, WSL 1 n’a pas pu exécuter le démon de la station d’accueil directement, mais cela a été modifié avec WSL 2 et a entraîné des améliorations significatives de la vitesse et des performances avec le Bureau de l’ordinateur d’accueil pour WSL 2.

Pour installer et exécuter la version d’évaluation de la technique de la version bêta de Desktop WSL 2 :

1. Téléchargez le [programme d’installation de dockr Desktop WSL 2 Tech Preview](https://download.docker.com/win/edge/36883/Docker%20Desktop%20Installer.exe). (Vous pouvez référencer les [documents du programme d’installation](https://docs.docker.com/docker-for-windows/wsl-tech-preview/) si nécessaire).

2. Ouvrez le programme d’installation de Dockr que vous venez de télécharger. L’Assistant installation vous demande si vous souhaitez « utiliser des conteneurs Windows au lieu de conteneurs Linux »-laissez cette option désactivée, car nous utiliserons le sous-système Linux. L’ancrage sera installé dans un répertoire géré dans votre distribution WSL 2 par défaut et inclura le démon, l’interface CLI et l’interface CLI compose.

    ![Démarrage du Bureau de l’arrimeur](../images/install-docker-1.png)

3. Si vous n’avez pas encore d’ID d’ancrage, vous devez en définir un en visitant : [https://hub.docker.com/signup](https://hub.docker.com/signup). Votre ID doit être composé de caractères alphanumériques en minuscules.

4. Une fois l’installation terminée, démarrez le Bureau de l’ancrage en sélectionnant l’icône de raccourci sur votre bureau ou en le recherchant dans le menu Démarrer de Windows. L’icône de l’ancrage s’affiche dans le menu icônes masquées de la barre des tâches. Cliquez avec le bouton droit sur l’icône pour afficher le menu de commandes de l’ancrage et sélectionnez « WSL 2 Tech Preview ».

5. Une fois que vous avez ouvert la version d’évaluation de la technologie, sélectionnez **Démarrer** pour commencer à exécuter le démon de l’ancrage (processus en arrière-plan) dans WSL 2. Quand le démon d’ancrage WSL 2 démarre, un contexte de l’interface de commande de l’ancrage est automatiquement créé pour celui-ci.

    ![Démarrage du Bureau de l’arrimeur](../images/start-docker.gif)

6. Pour confirmer que l’arrimeur a été installé et afficher le numéro de version, ouvrez une ligne de commande (WSL ou PowerShell) et entrez : `docker --version`

7. Testez le fonctionnement correct de votre installation en exécutant une simple image d’ancrage intégrée : `docker run hello-world`

Voici quelques-unes des commandes de l’ancrage que vous devez connaître :

- Répertoriez les commandes disponibles dans l’interface CLI de l’Ancreur en entrant : `docker`
- Répertorie des informations pour une commande spécifique avec : `docker <COMMAND> --help`
- Répertorier les images de l’arrimeur sur votre ordinateur (qui est simplement l’image Hello-World à ce stade), avec : `docker image ls --all`
- Répertorier les conteneurs sur votre ordinateur, avec : `docker container ls --all`
- Répertoriez les statistiques système et les ressources du système d’ancrage (UC & mémoire) à votre disposition dans le contexte WSL 2, avec : `docker info`
- Affichage où l’ancrageur est en cours d’exécution, avec : `docker context ls`

Vous pouvez voir qu’il y a deux contextes que l’ancrer s’exécute dans--`default` (le démon d’ancrage classique) et `wsl` (notre recommandation à l’aide de la version d’évaluation technique). (En outre, la commande `ls` est short pour `list` et peut être utilisée de façon interchangeable).

![Contexte d’affichage de l’ancreur dans PowerShell](../images/docker-context.png)

> [!TIP]
> Essayez de créer un exemple d’image de l’Ancreur avec ce [didacticiel sur le hub d’ancrage](https://hub.docker.com/?overlay=onboarding). Le hub d’ancrage contient également de nombreuses milliers d’images Open source qui peuvent correspondre au type d’application que vous souhaitez déconteneurr. Vous pouvez télécharger des images, telles que ce [conteneur d’infrastructure Gatsby. js](https://hub.docker.com/r/gatsbyjs/gatsby-dev-builds) ou ce [conteneur d’infrastructure Nuxt. js](https://hub.docker.com/r/hobord/nuxtexpress), et l’étendre avec votre propre code d’application. Vous pouvez effectuer une recherche dans le registre à l’aide [de dockr à partir de votre ligne de commande](https://docs.docker.com/engine/reference/commandline/search/) ou du [site Web de l’ancreur](https://hub.docker.com/search/?type=image).

## <a name="install-the-docker-extension-on-vs-code"></a>Installer l’extension d’ancrage sur VS Code

L’extension d’ancrage facilite la création, la gestion et le déploiement d’applications en conteneur à partir de Visual Studio Code.

1. Ouvrez la fenêtre **Extensions** (Ctrl + Maj + X) dans vs code et recherchez **dockr**.

2. Sélectionnez l' [extension Microsoft Ancrable](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker) et **Installez**. Vous devrez recharger VS Code après l’installation pour activer l’extension.

    ![Extension d’ancrage sur VS Code dans WSL à distance](../images/docker-vscode-extension.png)

En installant l’extension Dockr sur VS Code, vous pouvez désormais afficher une liste de commandes `Dockerfile` utilisées dans la section suivante avec le raccourci : `Ctrl+Space`

En savoir plus sur [l’utilisation de l’amarrage dans vs code](https://code.visualstudio.com/docs/azure/docker).

## <a name="create-a-container-image-with-dockerfile"></a>Créer une image de conteneur avec fichier dockerfile

Une **image de conteneur** stocke le code, les bibliothèques, les fichiers de configuration, les variables d’environnement et le runtime de votre application. L’utilisation d’une image permet de s’assurer que l’environnement de votre conteneur est standardisé et contient uniquement ce qui est nécessaire pour générer et exécuter votre application.

Un **fichier dockerfile** contient les instructions nécessaires pour générer la nouvelle image de conteneur. En d’autres termes, ce fichier génère l’image conteneur qui définit l’environnement de votre application afin qu’elle puisse être reproduite n’importe où.

Nous allons créer une image de conteneur à l’aide de l’application. js suivante configurée dans le guide des [frameworks web](./web-frameworks.md) .

1. Ouvrez votre application. js suivante dans VS Code (en veillant à ce que l’extension WSL distante s’exécute comme indiqué dans l’onglet vert en bas à gauche). Ouvrez le terminal WSL intégré dans VS Code (**afficher > terminal**) et assurez-vous que le chemin d’accès du terminal est pointé vers le répertoire du projet. js suivant (par ex. `~/NextProjects/my-next-app$`).

2. Créez un fichier appelé `Dockerfile` à la racine de votre prochain projet. js et ajoutez ce qui suit :

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

3. Pour générer l’image de l’ancreur, exécutez la commande suivante à partir de la racine de votre projet (en remplaçant `<your_docker_username>` par le nom d’utilisateur que vous avez créé sur le hub de l’Ancreur) : `docker build -t <your_docker_username>/my-nextjs-app .`

> [!NOTE]
> Pour que cette commande fonctionne, vous devez exécuter l’ordinateur de veille avec WSL Tech Preview. Pour obtenir un rappel sur la procédure de démarrage de l’ancrage, consultez l' [étape #4](#install-docker-desktop-wsl-2-tech-preview) de la section installer. L’indicateur `-t` spécifie le nom de l’image à créer, « My-nextjs-App : V1 » dans notre cas. Nous vous recommandons de toujours [utiliser une version # sur vos noms de balises lors de la](https://medium.com/@mccode/the-misunderstood-docker-tag-latest-af3babfd6375) création d’une image. Veillez à inclure le point à la fin de la commande, qui spécifie que le répertoire de travail actuel doit être utilisé pour rechercher et copier les fichiers de build de votre application. js suivante.

4. Pour exécuter cette nouvelle image de l’ancrer de votre application Next. js dans un conteneur, entrez la commande : `docker run -d -p 3333:3000 <your_docker_username>/my-nextjs-app:v1`

5. L’indicateur `-p` lie le port « 3000 » (le port sur lequel l’application s’exécute à l’intérieur du conteneur) au port local « 3333 » sur votre ordinateur, ce qui vous permet désormais de faire pointer votre navigateur Web sur [http://localhost:3333](http://localhost:3333) et de voir votre application. js. js côté serveur qui s’exécute en tant qu’ancreur image de conteneur.

> [!TIP]
> Nous avons créé notre image conteneur à l’aide de `FROM node:12`, qui fait référence à l’image par défaut node. js version 12 stockée sur le concentrateur de l’amarrage. Cette image node. js par défaut est basée sur un système Debian/Ubuntu Linux. Toutefois, il existe de nombreuses images node. js différentes à choisir, et vous pouvez envisager d’utiliser un élément plus léger ou adapté à vos besoins. Pour en savoir plus, consultez le [Registre d’images node. js sur le hub d’ancrage](https://hub.docker.com/_/node/).

## <a name="upload-your-container-image-to-a-repository"></a>Charger votre image conteneur dans un référentiel

Un **référentiel de conteneurs** stocke votre image de conteneur dans le Cloud. Souvent, un référentiel de conteneur contient en fait une collection d’images associées, telles que des versions différentes, qui sont toutes disponibles pour faciliter la configuration et le déploiement rapide. En règle générale, vous pouvez accéder à des images sur des référentiels de conteneurs via des points de terminaison HTTPs sécurisés, ce qui vous permet d’extraire, de transmettre ou de gérer des images via n’importe quel système, matériel ou instance de machine virtuelle.

Un **Registre de conteneurs**, en revanche, stocke un ensemble de référentiels, ainsi que des index, des règles de contrôle d’accès et des chemins d’API. Ils peuvent être hébergés publiquement ou en privé. Le [hub d’ancrage](https://hub.docker.com/) est un registre d’ancrage Open source et le paramètre par défaut utilisé lors de l’exécution des commandes `docker push` et `docker pull`. Il est gratuit pour les dépôts publics et requiert des frais pour les dépôts privés.

Pour charger votre nouvelle image de conteneur sur un référentiel hébergé sur le hub d’ancrage :

1. Connectez-vous au hub d’ancrage. Vous serez invité à entrer le nom d’utilisateur et le mot de passe que vous avez utilisés pour créer votre compte de hub d’ancrage au cours de l’étape d’installation. Pour vous connecter à Dockr sur votre terminal, entrez : `docker login`

2. Pour obtenir une liste des images de conteneur d’ancrage que vous avez créées sur votre ordinateur, entrez : `docker image ls --all`

3. Poussez votre image de conteneur vers le hub d’ancrage, en créant un nouveau référentiel à cet emplacement à l’aide de la commande suivante : `docker push <your_docker_username>/my-nextjs-app:v1`

4. Vous pouvez maintenant afficher votre dépôt sur le hub d’ancrage, entrer une description et lier votre compte GitHub (si vous le souhaitez), en visitant : https://cloud.docker.com/repository/list

5. Vous pouvez également afficher la liste de vos conteneurs d’ancrage actifs avec : `docker container ls` (ou `docker ps`)

6. Vous devez voir que votre conteneur « My-nextjs-App : v1 » est actif sur le port 3333-> 3000/TCP. Vous pouvez également voir votre « ID de conteneur » répertorié ici. Pour arrêter l’exécution de votre conteneur, entrez la commande : `docker stop <container ID>`

7. En règle générale, une fois qu’un conteneur est arrêté, il doit également être supprimé. La suppression d’un conteneur nettoie toutes les ressources qu’il laisse. Une fois que vous avez supprimé un conteneur, toutes les modifications que vous avez apportées dans son système de fichiers image sont définitivement perdues. Vous devrez créer une nouvelle image pour représenter les modifications. POUR supprimer votre conteneur, utilisez la commande : `docker rm <container ID>`

En savoir plus sur [la création d’une application Web en conteneur avec l’arrimeur](https://docs.microsoft.com/learn/modules/intro-to-containers/).

## <a name="deploy-to-azure-container-registry"></a>Déployer sur Azure Container Registry

[**Azure Container Registry**](https://azure.microsoft.com/services/container-registry/) (ACR) vous permet de stocker, de gérer et de sécuriser vos images de conteneur dans des référentiels privés et authentifiés. Compatible avec les commandes standard de l’arrimeur, ACR peut gérer des tâches critiques pour vous, comme la maintenance et la surveillance de l’intégrité des conteneurs, le couplage avec [Kubernetes](https://docs.microsoft.com/azure/aks/intro-kubernetes) pour créer des systèmes d’orchestration évolutifs. Générez à la demande ou automatisez entièrement les builds avec des déclencheurs tels que les validations du code source et les mises à jour de l’image de base. ACR tire également parti de l’énorme réseau Cloud Azure pour gérer la latence du réseau, les déploiements globaux et créer une expérience Native transparente pour toute personne utilisant [Azure App service](https://docs.microsoft.com/azure/app-service/) (pour l’hébergement Web, les back-ends mobiles, les API REST) ou d' [autres services Cloud Azure ](https://azure.microsoft.com/product-categories/containers/).

> [!IMPORTANT]
> Vous avez besoin de votre propre abonnement Azure pour déployer un conteneur sur Azure et vous pouvez recevoir des frais. Si vous n’avez pas encore d’abonnement Azure, [créez un compte gratuit](https://azure.microsoft.com/free/) avant de commencer.

Pour obtenir de l’aide sur la création d’une Azure Container Registry et le déploiement de votre image de conteneur d’application, consultez l’exercice : [Déployez une image de l’amarrage sur une instance de conteneur Azure](https://docs.microsoft.com/learn/modules/intro-to-containers/7-exercise-deploy-docker-image-to-container-instance).

## <a name="additional-resources"></a>Ressources supplémentaires

- [Node. js sur Azure](https://azure.microsoft.com/en-us/develop/nodejs/)
- Démarrage rapide : [Créer une application Web node. js dans Azure](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-nodejs)
- Cours en ligne : [Administrer des conteneurs dans Azure](https://docs.microsoft.com/learn/paths/administer-containers-in-azure/)
- Utilisation de VS Code : [Utilisation de l’arrimeur](https://code.visualstudio.com/docs/azure/docker)
- Documentation de l’arrimeur : [Station d’accueil Desktop WSL 2, version d’évaluation](https://docs.docker.com/docker-for-windows/wsl-tech-preview/)
