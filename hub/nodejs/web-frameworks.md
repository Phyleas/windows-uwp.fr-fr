---
title: Prise en main des infrastructures web Node.js sur Windows
description: Guide de prise en main des infrastructures web Node.js sur Windows
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, windows 10, microsoft, découvrir nodejs, node sur windows, node sur wsl, node sur linux sur windows, installer node sur windows, nodejs avec vs code, développer avec node sur windows, développer avec nodejs sur windows, installer node sur WSL, NodeJS sur le sous-système Windows pour Linux
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: a8ce1d08136a74504e1b3bad26feadd61b72068f
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "72517791"
---
# <a name="get-started-with-nodejs-web-frameworks-on-windows"></a>Prise en main des infrastructures web Node.js sur Windows

Guide pas à pas pour vous aider à utiliser les infrastructures web Node.js sur Windows, notamment Next.js, Nuxt.js et Gatsby.

## <a name="prerequisites"></a>Prérequis

Ce guide part du principe que vous avez déjà suivi les étapes de [configuration de votre environnement de développement Node.js avec WSL 2](./setup-on-wsl2.md), y compris les étapes suivantes :

- Installer Windows 10 Insider Preview, build 18932 ou ultérieure.
- Activez la fonctionnalité WSL 2 sur Windows.
- Installer une distribution Linux (Ubuntu 18.04 dans les exemples). Pour le vérifier, exécutez : `wsl lsb_release -a`.
- Vérifier que votre distribution Ubuntu 18.04 s’exécute en mode WSL 2. (WSL peut exécuter les distributions en mode v1 ou v2.) Pour le vérifier, ouvrez PowerShell et entrez : `wsl -l -v`.
- À l’aide de PowerShell, définissez Ubuntu 18.04 en tant que distribution par défaut, avec : `wsl -s ubuntu 18.04`

## <a name="get-started-with-nextjs"></a>Bien démarrer avec Next.js

Next.js est une infrastructure permettant de créer des applications JavaScript rendues côté serveur basées sur React.js, Node.js, WebPack et Babel.js. Il s'agit en fait d'un texte réutilisable de projet pour React, conçu en tenant compte des meilleures pratiques, qui permet de créer des applications web « universelles » de manière simple et cohérente, avec pratiquement aucune configuration. Ces applications web rendues côté serveur « universelles » sont parfois appelées « isomorphes », ce qui signifie que le code est partagé entre le client et le serveur.

Pour créer un projet Next.js, incluant l’installation de next, react et react-dom :

1. Ouvrez votre terminal WSL (par ex. Ubuntu 18.04).

2. Créez un dossier pour le nouveau projet : `mkdir NextProjects`, puis accédez à ce répertoire : `cd NextProjects`.

3. Installez Next.js et créez un projet (en remplaçant « my-next-app » par le nom que vous souhaitez donner à votre application) : `npm create next-app my-next-app`.

4. Une fois le package installé, modifiez les répertoires dans votre nouveau dossier d’application, `cd my-next-app`, puis utilisez `code .` pour ouvrir votre projet Next.js dans VS Code. Vous pourrez ainsi examiner l’infrastructure Next.js créée pour votre application. Notez que VS Code a ouvert votre application dans un environnement WSL-Remote (comme indiqué dans l’onglet vert situé en bas à gauche de votre fenêtre de VS Code). Cela signifie que lorsque vous utilisez VS Code à des fins d'édition sur le système d’exploitation Windows, il continue d'exécuter votre application sur le système d’exploitation Linux.

    ![Extension WSL-Remote](../images/wsl-remote-extension.png)

5. Trois commandes sont à connaître une fois Next.js installé :

    - `npm run dev` pour exécuter une instance de développement avec rechargement à chaud, surveillance des fichiers et réexécution des tâches.
    - `npm run build` pour compiler votre projet.
    - `npm start` pour démarrer votre application en mode de production.

    Ouvrez le terminal WSL intégré à VS Code (**Afficher > Terminal**). Assurez-vous que le chemin du terminal pointe vers le répertoire de votre projet (par ex. `~/NextProjects/my-next-app$`). Essayez ensuite d’exécuter une instance de développement de votre nouvelle application Next.js à l’aide de : `npm run dev`

6. Le serveur de développement local démarre et une fois les pages de votre projet générées, votre terminal affiche « compilé avec succès - prêt sur [http://localhost:3000](http://localhost:3000) ». Sélectionnez ce lien localhost pour ouvrir votre nouvelle application Next.js dans un navigateur web.

    ![Votre application Next.js en cours d'exécution dans localhost:3000](../images/next-app.png)

7. Ouvrez le fichier `pages/index.js` dans votre éditeur VS Code. Recherchez le titre de la page `<h1 className='title'>Welcome to Next.js!</h1>` et remplacez-le par `<h1 className='title'>This is my new Next.js app!</h1>`. Avec votre navigateur Web toujours ouvert sur localhost:3000, enregistrez la modification. Vous pouvez constater que la fonctionnalité de rechargement à chaud compile et met à jour automatiquement votre modification dans le navigateur.

8. Penchons-nous sur la manière dont Next.js gère les erreurs. Supprimez la balise de fermeture `</h1>` de manière à ce que votre code de titre se présente comme suit : `<h1 className='title'>This is my new Next.js app!`. Enregistrez cette modification. Vous pouvez constater qu'une erreur « Échec de la compilation » s’affiche dans votre navigateur et sur votre terminal pour vous indiquer qu'une balise de fermeture est attendue pour `<h1>`. Remplacez la balise de fermeture `</h1>` et enregistrez pour recharger la page.

Pour utiliser le débogueur de VS Code avec votre application Next.js, sélectionnez la touche F5, ou accédez à **Afficher >** Déboguer (Ctrl+Maj+D), puis **Afficher > Console de débogage** (Ctrl+Maj+Y) dans la barre de menus. Si vous sélectionnez l’icône d’engrenage dans la fenêtre de débogage, un fichier de configuration de lancement (`launch.json`) est créé pour vous permettre d’enregistrer les détails de configuration du débogage. Pour plus d’informations, consultez [Débogage VS Code](https://code.visualstudio.com/docs/nodejs/nodejs-debugging).

![Fenêtre de débogage VS Code et icône de configuration de launch.json](../images/vscode-debug-launch-configuration.png)

Pour plus d'informations sur Next.js, consultez la [documentation Next.js](https://nextjs.org/docs).

## <a name="get-started-with-nuxtjs"></a>Bien démarrer avec Nuxt.js

Nuxt.js est une infrastructure permettant de créer des applications JavaScript rendues côté serveur basées sur Vue.js, Node.js, WebPack et Babel.js. Elle s'inspire de Next.js. Il s’agit en fait d'un texte réutilisable de projet pour Vue. À l'instar de Next.js, l'infrastructure est conçue en tenant compte des meilleures pratiques et vous permet de créer des applications web « universelles » de manière simple et cohérente, avec pratiquement aucune configuration. Ces applications web rendues côté serveur « universelles » sont parfois appelées « isomorphes », ce qui signifie que le code est partagé entre le client et le serveur.

Pour créer un projet Nuxt.js, incluant les réponses à une série de questions sur le type d’infrastructure côté serveur intégrée, l’infrastructure d’interface utilisateur, l’infrastructure de test, le mode, les modules et les linters à installer :

1. Ouvrez votre terminal WSL (par ex. Ubuntu 18.04).

2. Créez un dossier pour le nouveau projet : `mkdir NuxtProjects`, puis accédez à ce répertoire : `cd NuxtProjects`.

3. Installez Nuxt.js et créez un projet (en remplaçant « my-nuxt-app » par le nom que vous souhaitez donner à votre application) : `npm create nuxt-app my-nuxt-app`.

4. Le programme d’installation Nuxt.js vous invite à répondre aux questions suivantes :
    - Nom du projet : my-nuxtjs-app
    - Description du projet : Description de mon application Nuxt.js.
    - Nom de l’auteur : J’utilise mon alias GitHub.
    - Gestionnaire de package : Yarn ou **Npm** - nous utilisons NPM pour les exemples.
    - Infrastructure d’interface utilisateur : Aucune, Ant Design Vue, Bootstrap Vue, etc. Dans cet exemple, nous allons opter pour **Vuetify**, mais la communauté Vue a créé un [document particulièrement intéressant qui compare ces infrastructures d’interface utilisateur](https://vue-community.org/guide/ecosystem/ui-libraries.html#summary-tldr) afin de faciliter votre choix.
    - Infrastructure de serveur personnalisée : Aucune, AdonisJs, Express, Fastify, etc. Dans cet exemple, nous allons opter pour **Aucune**, mais une [comparaison des infrastructures serveur 2019-2020](https://dev.to/santypk4/introducing-the-best-10-node-js-frameworks-for-2019-and-2020-mcm) est disponible sur le site Dev.to.
    - Choisissez les modules Nuxt.js (utilisez la barre d’espace pour sélectionner des modules ou entrée si vous ne souhaitez pas en utiliser) : Axios (pour simplifier les requêtes HTTP) ou [prise en charge PWA](https://pwa.nuxtjs.org/) (pour l’ajout d’un worker de service, d'un fichier manifest.json, etc.). Dans cet exemple, nous n'ajoutons pas de module.
    - Outils de linting : **ESLint**, Prettier, fichiers intermédiaires Lint. Nous allons opter pour **ESLint** (un outil permettant d'analyser votre code et de vous prévenir en cas d'erreurs).
    - Infrastructure de test : **Aucune**, Jest, AVA. Nous allons opter pour **Aucune**, car nous n’aborderons pas les tests dans ce guide de démarrage rapide.
    - Mode de rendu : **Universal (SSR)** ou Single Page App (SPA). Dans cet exemple, nous allons opter pour **Universal (SSR)** , mais la documentation [Nuxt.js](https://nuxtjs.org/guide#server-rendered-universal-ssr-) souligne des différences -- SSR requiert un serveur Node.js en cours d'exécution pour rendre côté serveur votre application et SPA pour l’hébergement statique.
    - Outils de développement : **jsconfig.json** (recommandé pour VS Code à des fins de complétion du code IntelliSense)

5. Une fois votre projet créé, `cd my-nuxtjs-app` pour accéder au répertoire de votre projet Nuxt.js, puis entrez `code .` pour ouvrir le projet dans l’environnement WSL-Remote VS Code.

    ![Extension WSL-Remote](../images/wsl-remote-extension.png)

6. Trois commandes sont à connaître une fois Nuxt.js installé :

    - `npm run dev` pour exécuter une instance de développement avec rechargement à chaud, surveillance des fichiers et réexécution des tâches.
    - `npm run build` pour compiler votre projet.
    - `npm start` pour démarrer votre application en mode de production.

    Ouvrez le terminal WSL intégré à VS Code (**Afficher > Terminal**). Assurez-vous que le chemin du terminal pointe vers le répertoire de votre projet (par ex. `~/NuxtProjects/my-nuxt-app$`). Essayez ensuite d’exécuter une instance de développement de votre nouvelle application Nuxt.js à l’aide de : `npm run dev`

6. Le serveur de développement local démarre (et affiche plusieurs barres de progression pour les compilations client et serveur). Une fois votre projet généré, le terminal affiche « Compilé avec succès », ainsi que la durée de la compilation. Pointez votre navigateur web vers [http://localhost:3000](http://localhost:3000) pour ouvrir votre nouvelle application Nuxt.js.

    ![Votre application Nuxt.js en cours d'exécution dans localhost:3000](../images/nuxt-app.png)

7. Ouvrez le fichier `pages/index.vue` dans votre éditeur VS Code. Recherchez le titre de la page `<v-card-title class="headline">Welcome to the Vuetify + Nuxt.js template</v-card-title>` et remplacez-le par `<v-card-title class="headline">This is my new Nuxt.js app!</v-card-title>`. Avec votre navigateur Web toujours ouvert sur localhost:3000, enregistrez la modification. Vous pouvez constater que la fonctionnalité de rechargement à chaud compile et met à jour automatiquement votre modification dans le navigateur.

8. Penchons-nous sur la manière dont Nuxt.js gère les erreurs. Supprimez la balise de fermeture `</v-card-title>` de manière à ce que votre code de titre se présente comme suit : `<v-card-title class="headline">This is my new Nuxt.js app!`. Enregistrez cette modification. Vous pouvez constater qu'une erreur de compilation s’affiche dans votre navigateur et sur votre terminal pour vous indiquer qu'une balise de fermeture pour `<v-card-title>` est manquante, ainsi que les numéros de ligne où se situe l'erreur dans votre code. Remplacez la balise de fermeture `</v-card-title>` et enregistrez pour recharger la page.

Pour utiliser le débogueur de VS Code avec votre application Nuxt.js, sélectionnez la touche F5, ou accédez à **Afficher >** Déboguer (Ctrl+Maj+D), puis **Afficher > Console de débogage** (Ctrl+Maj+Y) dans la barre de menus. Si vous sélectionnez l’icône d’engrenage dans la fenêtre de débogage, un fichier de configuration de lancement (`launch.json`) est créé pour vous permettre d’enregistrer les détails de configuration du débogage. Pour plus d’informations, consultez [Débogage VS Code](https://code.visualstudio.com/docs/nodejs/nodejs-debugging).

![Fenêtre de débogage VS Code et icône de configuration de launch.json](../images/vscode-debug-launch-configuration.png)

Pour plus d'informations sur Nuxt.js, consultez le [guide Nuxt.js](https://nuxtjs.org/guide).

## <a name="get-started-with-gatsbyjs"></a>Bien démarrer avec Gatsby.js

Gatsby.js est une infrastructure de génération de site statique basée sur React.js, sans rendu côté serveur comme Next.js ou Nuxt.js. Un générateur de site statique génère du code HTML statique. Il ne requiert aucun serveur. Next.js et Nuxt.js génèrent du code HTML au moment de l’exécution (chaque fois qu’une nouvelle requête arrive). Leur exécution requiert un serveur. Gatsby dicte également la manière dont les données sont gérées dans votre application (avec GraphQL), tandis qu'avec Next.js et Nuxt.js, cette décision vous appartient.

Pour créer un projet Gatsby.js :

1. Ouvrez votre terminal WSL (par ex. Ubuntu 18.04).
2. Créez un dossier pour le nouveau projet : `mkdir GatsbyProjects`, puis accédez à ce répertoire : `cd GatsbyProjects`
3. Utilisez npm pour installer l'interface de ligne de commande Gatsby : `npm install -g gatsby-cli`. Lorsque c'est chose faite, vérifiez la version à l'aide de `gatsby --version`.
4. Créez votre projet Gatsby.js : `gatsby new my-gatsby-app`
5. Une fois le package installé, modifiez les répertoires dans votre nouveau dossier d’application, `cd my-gatsby-app`, puis utilisez `code .` pour ouvrir votre projet Gatsby dans VS Code. Vous pouvez ainsi examiner l'infrastructure Gatsby.js créée pour votre application à l’aide de l’Explorateur de fichiers de VS Code. Notez que VS Code a ouvert votre application dans un environnement WSL-Remote (comme indiqué dans l’onglet vert situé en bas à gauche de votre fenêtre de VS Code). Cela signifie que lorsque vous utilisez VS Code à des fins d'édition sur le système d’exploitation Windows, il continue d'exécuter votre application sur le système d’exploitation Linux.

    ![Extension WSL-Remote](../images/wsl-remote-extension.png)

6. Trois commandes sont à connaître une fois Gatsby installé :

    - `gatsby develop` pour exécuter une instance de développement avec rechargement à chaud.
    - `gatsby build` pour créer une build de production.
    - `gatsby serve` pour démarrer votre application en mode de production.

    Ouvrez le terminal WSL intégré à VS Code (**Afficher > Terminal**). Assurez-vous que le chemin du terminal pointe vers le répertoire de votre projet (par ex. `~/GatsbyProjects/my-gatsby-app$`). Essayez ensuite d’exécuter une instance de développement de votre nouvelle application à l’aide de : `gatsby develop`

7. Une fois la compilation de votre nouveau projet Gatsby terminée, votre terminal indique « Vous pouvez maintenant afficher gatsby-start-default dans le navigateur. [http://localhost:8000/](http://localhost:8000/) ». Sélectionnez ce lien localhost pour afficher votre nouveau projet intégré dans un navigateur web.

> [!NOTE]
> Vous noterez que la sortie de votre terminal indique également que vous pouvez « Afficher GraphiQL, un IDE du navigateur, pour explorer les données et le schéma de votre site : [http://localhost:8000/___graphql](http://localhost:8000/___graphql) ». GraphQL regroupe vos API au sein d'un IDE auto-documenté (GraphiQL) intégré à Gatsby. En plus d'explorer les données et le schéma de votre site, vous pouvez effectuer des opérations GraphQL comme des requêtes, mutations et abonnements. Pour plus d’informations, consultez [Présentation de GraphiQL](https://www.gatsbyjs.org/docs/running-queries-with-graphiql/).

8. Ouvrez le fichier `src/pages/index.js` dans votre éditeur VS Code. Recherchez le titre de la page `<h1 >Hi people</h1>` et remplacez-le par `<h1 >Hi (Your Name)!</h1>`. Avec votre navigateur Web toujours ouvert sur localhost:8000, enregistrez la modification. Vous pouvez constater que la fonctionnalité de rechargement à chaud compile et met à jour automatiquement votre modification dans le navigateur.

    ![Votre application Gatsby.js en cours d'exécution dans localhost:3000](../images/gatsby-app.png)

9. Penchons-nous sur la manière dont Next.js gère les erreurs. Supprimez la balise de fermeture `</h1>` de manière à ce que votre code de titre se présente comme suit : `<h1>Hi (Your Name)!`. Enregistrez cette modification. Vous pouvez constater qu'une erreur « Échec de la compilation » s’affiche dans votre navigateur et sur votre terminal pour vous indiquer qu'une balise de fermeture est attendue pour `<h1>`. Remplacez la balise de fermeture `</h1>` et enregistrez pour recharger la page.

Pour utiliser le débogueur de VS Code avec votre application Next.js, sélectionnez la touche F5, ou accédez à **Afficher >** Déboguer (Ctrl+Maj+D), puis **Afficher > Console de débogage** (Ctrl+Maj+Y) dans la barre de menus. Si vous sélectionnez l’icône d’engrenage dans la fenêtre de débogage, un fichier de configuration de lancement (`launch.json`) est créé pour vous permettre d’enregistrer les détails de configuration du débogage. Pour plus d’informations, consultez [Débogage VS Code](https://code.visualstudio.com/docs/nodejs/nodejs-debugging).

![Fenêtre de débogage VS Code et icône de configuration de launch.json](../images/vscode-debug-launch-configuration.png)

Pour en savoir plus sur Gatsby, consultez la [documentation Gatsby.js](https://www.gatsbyjs.org/docs/).
