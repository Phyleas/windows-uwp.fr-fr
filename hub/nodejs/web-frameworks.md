---
title: Prise en main des infrastructures Web node. js sur Windows
description: Guide pour vous aider à prendre en main les infrastructures Web node. js sur Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, node. js, Windows 10, Microsoft, formation NodeJS, node sur Windows, node sur WSL, node sur Linux sur Windows, installer le nœud sur Windows, NodeJS avec vs code, développer avec un nœud sur Windows, développer avec NodeJS sur Windows, installer le nœud sur WSL, NodeJS sur Windows Sous-système pour Linux
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: a8ce1d08136a74504e1b3bad26feadd61b72068f
ms.sourcegitcommit: 60d2d15dd0d365f82e4e90e4bc34b40cf5b4a247
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517791"
---
# <a name="get-started-with-nodejs-web-frameworks-on-windows"></a>Prise en main des infrastructures Web node. js sur Windows

Guide pas à pas pour vous aider à commencer à utiliser les infrastructures Web node. js sur Windows, notamment Next. js, Nuxt. js et Gatsby.

## <a name="prerequisites"></a>Conditions préalables

Ce guide part du principe que vous avez déjà effectué les étapes pour [configurer votre environnement de développement node. js avec WSL 2](./setup-on-wsl2.md), notamment :

- Installez Windows 10 Insider preview version 18932 ou ultérieure.
- Activez la fonctionnalité WSL 2 sur Windows.
- Installez une distribution Linux (Ubuntu 18,04 pour nos exemples). Vous pouvez le vérifier avec : `wsl lsb_release -a`
- Assurez-vous que votre distribution Ubuntu 18,04 s’exécute en mode WSL 2. (WSL peut exécuter des distributions en mode v1 ou v2.) Vous pouvez le vérifier en ouvrant PowerShell et en entrant : `wsl -l -v`
- À l’aide de PowerShell, définissez Ubuntu 18,04 comme distribution par défaut, avec : `wsl -s ubuntu 18.04`

## <a name="get-started-with-nextjs"></a>Bien démarrer avec Next.js

Next. js est une infrastructure qui vous aide à créer des applications JavaScript à rendu serveur basées sur REACT. js, sur Node. js, sur WebPack et sur Babel. js. En fait, il s’agit d’un projet souvent utilisé pour réagir, conçu avec une attention particulière aux meilleures pratiques, qui vous permet de créer des applications Web « universelles » de manière simple et cohérente, avec peu de configuration. Ces applications Web « universelles » de rendu de serveur sont parfois appelées « isomorphes », ce qui signifie que le code est partagé entre le client et le serveur.

Pour créer un projet. js suivant, qui comprend l’installation de Next, REACT et REACT-DOM :

1. Ouvrez votre terminal WSL (par ex. Ubuntu 18,04).

2. Créez un dossier de projet : `mkdir NextProjects`, puis entrez ce répertoire : `cd NextProjects`.

3. Installez Next. js et créez un projet (en remplaçant « My-Next-app » par ce que vous souhaitez appeler votre application) : `npm create next-app my-next-app`.

4. Une fois le package installé, modifiez les répertoires dans votre nouveau dossier d’application, `cd my-next-app`, puis utilisez `code .` pour ouvrir votre projet. js suivant dans VS Code. Cela vous permettra d’examiner l’infrastructure. js suivante qui a été créée pour votre application. Notez que VS Code ouvert votre application dans un environnement distant WSL (comme indiqué dans l’onglet vert en bas à gauche de la fenêtre de VS Code). Cela signifie que lorsque vous utilisez VS Code pour la modification sur le système d’exploitation Windows, il exécute toujours votre application sur le système d’exploitation Linux.

    ![WSL-extension distante](../images/wsl-remote-extension.png)

5. Il y a 3 commandes que vous devez savoir après l’installation de Next. js :

    - `npm run dev` pour exécuter une instance de développement avec rechargement à chaud, surveillance des fichiers et réexécution des tâches.
    - `npm run build` pour la compilation de votre projet.
    - `npm start` pour le démarrage de votre application en mode production.

    Ouvrez le terminal WSL intégré dans VS Code (**afficher > terminal**). Assurez-vous que le chemin d’accès du terminal pointe vers le répertoire de votre projet (par ex. `~/NextProjects/my-next-app$`). Essayez ensuite d’exécuter une instance de développement de votre nouvelle application Next. js à l’aide de : `npm run dev`

6. Le serveur de développement local démarre et une fois que les pages de votre projet sont terminées, votre terminal affiche « compilé avec succès sur [http://localhost:3000](http://localhost:3000)». Sélectionnez ce lien localhost pour ouvrir votre nouvelle application Next. js dans un navigateur Web.

    ![Votre application. js suivante s’exécutant dans localhost : 3000](../images/next-app.png)

7. Ouvrez le fichier `pages/index.js` dans votre éditeur de VS Code. Recherchez le titre de la page `<h1 className='title'>Welcome to Next.js!</h1>` et remplacez-le par `<h1 className='title'>This is my new Next.js app!</h1>`. Avec votre navigateur Web toujours ouvert à localhost : 3000, enregistrez votre modification et notez que la fonctionnalité de rechargement à chaud compile et met à jour automatiquement vos modifications dans le navigateur.

8. Voyons comment Next. js gère les erreurs. Supprimez la balise de fermeture `</h1>` afin que votre code de titre ressemble maintenant à ceci : `<h1 className='title'>This is my new Next.js app!`. Enregistrez cette modification et Notez qu’une erreur « Échec de la compilation » s’affiche dans votre navigateur, et dans votre terminal, vous savez qu’une balise de fermeture pour `<h1>` est attendue. Remplacez la balise de fermeture `</h1>`, Save, et la page se recharge.

Vous pouvez utiliser le débogueur de VS Code avec votre application. js suivante en sélectionnant la touche F5, ou en accédant à **afficher > débogage** (Ctrl + Maj + D) et **Afficher > console de débogage** (CTRL + MAJ + Y) dans la barre de menus. Si vous sélectionnez l’icône d’engrenage dans la fenêtre de débogage, un fichier de configuration de lancement (`launch.json`) sera créé pour vous permettre d’enregistrer les détails du programme d’installation du débogage. Pour plus d’informations, consultez [vs code le débogage](https://code.visualstudio.com/docs/nodejs/nodejs-debugging).

![Icône de débogage VS Code et de configuration de Launch. JSON](../images/vscode-debug-launch-configuration.png)

Pour en savoir plus sur Next. js, consultez les [documents suivants. js](https://nextjs.org/docs).

## <a name="get-started-with-nuxtjs"></a>Bien démarrer avec Nuxt.js

Nuxt. js est une infrastructure permettant de créer des applications JavaScript à rendu serveur basées sur vue. js, sur Node. js, sur WebPack et sur Babel. js. Il a été inspiré par Next. js. Il s’agit fondamentalement d’un projet réutilisable pour la vue. Tout comme Next. js, il est conçu avec une attention particulière aux meilleures pratiques et vous permet de créer des applications Web « universelles » de manière simple et cohérente, avec peu de configuration. Ces applications Web « universelles » de rendu de serveur sont parfois appelées « isomorphes », ce qui signifie que le code est partagé entre le client et le serveur.

Pour créer un projet Nuxt. js, qui inclut la réponse à une série de questions sur le type d’infrastructure intégrée côté serveur, l’infrastructure d’interface utilisateur, l’infrastructure de test, le mode, les modules et les linters que vous souhaitez installer :

1. Ouvrez votre terminal WSL (par ex. Ubuntu 18,04).

2. Créez un dossier de projet : `mkdir NuxtProjects`, puis entrez ce répertoire : `cd NuxtProjects`.

3. Installez Nuxt. js et créez un projet (en remplaçant « My-Nuxt-app » par ce que vous souhaitez appeler dans votre application) : `npm create nuxt-app my-nuxt-app`

4. Le programme d’installation de Nuxt. js vous demandera à présent les questions suivantes :
    - Nom du projet : My-nuxtjs-App
    - Description du projet : description de mon application Nuxt. js.
    - Nom de l’auteur : J’utilise mon alias GitHub.
    - Choisissez le gestionnaire de package : fils ou **NPM** -nous utilisons NPM pour nos exemples.
    - Choisissez l’infrastructure d’interface utilisateur : aucune, conception ant. vue, bootstrap vue, etc. Nous allons choisir **Vuetify** pour cet exemple, mais la communauté vue a créé un [récapitulatif intéressant qui compare ces FRAMEWORKS d’interface utilisateur](https://vue-community.org/guide/ecosystem/ui-libraries.html#summary-tldr) pour vous aider à choisir le mieux adapté à votre projet.
    - Choisissez des frameworks de serveur personnalisés : None, AdonisJs, Express, Fastify, etc. Nous allons choisir **aucun** pour cet exemple, mais vous trouverez une [comparaison 2019-2020 Server Framework](https://dev.to/santypk4/introducing-the-best-10-node-js-frameworks-for-2019-and-2020-mcm) sur le site dev.to.
    - Choisissez les modules Nuxt. js (utilisez la barre d’espace pour sélectionner des modules ou simplement entrer si vous ne souhaitez pas) : Axios (pour simplifier les requêtes HTTP) ou la [prise en charge de PWA](https://pwa.nuxtjs.org/) (pour ajouter un service-Worker, un fichier manifest. JSON, etc.). Nous allons ajouter un module pour cet exemple.
    - Choisissez des outils de non-entropie : ESLint, améliorer, fichiers **pas**. Nous allons choisir **ESLint** (un outil pour analyser votre code et vous avertir des erreurs potentielles).
    - Choisissez une infrastructure de test : **None**, Jest, Ava. Nous allons choisir **aucun** , car nous n’aborderons pas les tests dans ce guide de démarrage rapide.
    - Choisissez mode de rendu : **universel (SSR)** ou application à page unique (Spa). Pour notre exemple, nous allons choisir **Universal (SSR)** . Toutefois, les [documents Nuxt. js](https://nuxtjs.org/guide#server-rendered-universal-ssr-) signalent certaines des différences entre les différences entre un serveur node. js et un serveur node. js qui exécutent votre application et Spa pour l’hébergement statique.
    - Choisir les outils de développement : **jsconfig. JSON** (recommandé pour vs code pour que la saisie semi-automatique du code IntelliSense fonctionne)

5. Une fois votre projet créé, `cd my-nuxtjs-app` pour accéder au répertoire de votre projet Nuxt. js, puis entrez `code .` pour ouvrir le projet dans l’environnement distant VS Code WSL.

    ![WSL-extension distante](../images/wsl-remote-extension.png)

6. Il y a 3 commandes que vous devez savoir une fois Nuxt. js installé :

    - `npm run dev` pour exécuter une instance de développement avec rechargement à chaud, surveillance des fichiers et réexécution des tâches.
    - `npm run build` pour la compilation de votre projet.
    - `npm start` pour le démarrage de votre application en mode production.

    Ouvrez le terminal WSL intégré dans VS Code (**afficher > terminal**). Assurez-vous que le chemin d’accès du terminal pointe vers le répertoire de votre projet (par ex. `~/NuxtProjects/my-nuxt-app$`). Essayez ensuite d’exécuter une instance de développement de votre nouvelle application Nuxt. js à l’aide de : `npm run dev`

6. Le serveur de développement local démarre (en affichant un certain type de barres de progression intéressantes pour les compilations du client et du serveur). Une fois votre projet créé, votre terminal affiche « compilé avec succès », ainsi que le temps nécessaire à la compilation. Pointez votre navigateur Web sur [http://localhost:3000](http://localhost:3000) pour ouvrir votre nouvelle application Nuxt. js.

    ![Votre application Nuxt. js s’exécutant dans localhost : 3000](../images/nuxt-app.png)

7. Ouvrez le fichier `pages/index.vue` dans votre éditeur de VS Code. Recherchez le titre de la page `<v-card-title class="headline">Welcome to the Vuetify + Nuxt.js template</v-card-title>` et remplacez-le par `<v-card-title class="headline">This is my new Nuxt.js app!</v-card-title>`. Avec votre navigateur Web toujours ouvert à localhost : 3000, enregistrez votre modification et notez que la fonctionnalité de rechargement à chaud compile et met à jour automatiquement vos modifications dans le navigateur.

8. Voyons comment Nuxt. js gère les erreurs. Supprimez la balise de fermeture `</v-card-title>` afin que votre code de titre ressemble maintenant à ceci : `<v-card-title class="headline">This is my new Nuxt.js app!`. Enregistrez cette modification et remarquez qu’une erreur de compilation s’affiche dans votre navigateur, et dans votre terminal, vous savez qu’une balise de fermeture pour `<v-card-title>` est manquante, ainsi que les numéros de ligne où l’erreur se trouve dans votre code. Remplacez la balise de fermeture `</v-card-title>`, Save, et la page se recharge.

Vous pouvez utiliser le débogueur de VS Code avec votre application Nuxt. js en sélectionnant la touche F5, ou en accédant à **afficher > débogage** (Ctrl + Maj + D) et **Afficher > console de débogage** (CTRL + MAJ + Y) dans la barre de menus. Si vous sélectionnez l’icône d’engrenage dans la fenêtre de débogage, un fichier de configuration de lancement (`launch.json`) sera créé pour vous permettre d’enregistrer les détails du programme d’installation du débogage. Pour plus d’informations, consultez [vs code le débogage](https://code.visualstudio.com/docs/nodejs/nodejs-debugging).

![Icône de débogage VS Code et de configuration de Launch. JSON](../images/vscode-debug-launch-configuration.png)

Pour en savoir plus sur Nuxt. js, consultez le [Guide Nuxt. js](https://nuxtjs.org/guide).

## <a name="get-started-with-gatsbyjs"></a>Bien démarrer avec Gatsby.js

Gatsby. js est une infrastructure de génération de site statique basée sur REACT. js, par opposition à un rendu serveur comme Next. js et Nuxt. js. Un générateur de site statique génère du code HTML statique au moment de la génération. Il ne nécessite pas de serveur. Next. js et Nuxt. js génèrent du code HTML au moment de l’exécution (chaque fois qu’une nouvelle requête arrive). Ils requièrent l’exécution d’un serveur. Gatsby détermine également comment gérer les données dans votre application (avec GraphQL), tandis que Next. js et Nuxt. js laissent cette décision à vous.

Pour créer un projet Gatsby. js :

1. Ouvrez votre terminal WSL (par ex. Ubuntu 18,04).
2. Créez un dossier de projet : `mkdir GatsbyProjects`, puis entrez ce répertoire : `cd GatsbyProjects`
3. Utilisez NPM pour installer Gatsby CLI : `npm install -g gatsby-cli`. Une fois l’installation terminée, vérifiez la version avec `gatsby --version`.
4. Créez votre projet Gatsby. js : `gatsby new my-gatsby-app`
5. Une fois le package installé, modifiez les répertoires dans votre nouveau dossier d’application, `cd my-gatsby-app`, puis utilisez `code .` pour ouvrir votre projet Gatsby dans VS Code. Cela vous permettra d’examiner l’infrastructure Gatsby. js créée pour votre application à l’aide de l’Explorateur de fichiers de VS Code. Notez que VS Code ouvert votre application dans un environnement distant WSL (comme indiqué dans l’onglet vert en bas à gauche de la fenêtre de VS Code). Cela signifie que lorsque vous utilisez VS Code pour la modification sur le système d’exploitation Windows, il exécute toujours votre application sur le système d’exploitation Linux.

    ![WSL-extension distante](../images/wsl-remote-extension.png)

6. Il y a 3 commandes que vous devez savoir une fois Gatsby installé :

    - `gatsby develop` pour exécuter une instance de développement avec un rechargement à chaud.
    - `gatsby build` pour la création d’une build de production.
    - `gatsby serve` pour le démarrage de votre application en mode production.

    Ouvrez le terminal WSL intégré dans VS Code (**afficher > terminal**). Assurez-vous que le chemin d’accès du terminal pointe vers le répertoire de votre projet (par ex. `~/GatsbyProjects/my-gatsby-app$`). Essayez ensuite d’exécuter une instance de développement de votre nouvelle application à l’aide de : `gatsby develop`

7. Une fois votre nouveau projet Gatsby terminé la compilation, votre terminal affiche la valeur «vous pouvez maintenant afficher Gatsby-Start-default dans le navigateur. [http://localhost:8000/](http://localhost:8000/).» Sélectionnez ce lien localhost pour afficher votre nouveau projet généré dans un navigateur Web.

> [!NOTE]
> Vous remarquerez que la sortie de votre terminal vous informe également que vous pouvez afficher GraphiQL, un IDE dans le navigateur, pour explorer les données et le schéma de votre site : [http://localhost:8000/___graphql](http://localhost:8000/___graphql)». GraphQL consolide vos API dans un IDE (GraphiQL) intégré à Gatsby. Outre l’exploration des données et du schéma de votre site, vous pouvez effectuer des opérations GraphQL telles que des requêtes, des mutations et des abonnements. Pour plus d’informations, consultez [Présentation de GraphiQL](https://www.gatsbyjs.org/docs/running-queries-with-graphiql/).

8. Ouvrez le fichier `src/pages/index.js` dans votre éditeur de VS Code. Recherchez le titre de la page `<h1 >Hi people</h1>` et remplacez-le par `<h1 >Hi (Your Name)!</h1>`. Avec votre navigateur Web toujours ouvert à localhost : 8000, enregistrez votre modification et notez que la fonctionnalité de rechargement à chaud compile et met à jour automatiquement vos modifications dans le navigateur.

    ![Votre application Gatsby. js s’exécutant dans localhost : 3000](../images/gatsby-app.png)

9. Voyons comment Next. js gère les erreurs. Supprimez la balise de fermeture `</h1>` afin que votre code de titre ressemble maintenant à ceci : `<h1>Hi (Your Name)!`. Enregistrez cette modification et Notez qu’une erreur « Échec de la compilation » s’affiche dans votre navigateur, et dans votre terminal, vous savez qu’une balise de fermeture pour `<h1>` est attendue. Remplacez la balise de fermeture `</h1>`, Save, et la page se recharge.

Vous pouvez utiliser le débogueur de VS Code avec votre application. js suivante en sélectionnant la touche F5, ou en accédant à **afficher > débogage** (Ctrl + Maj + D) et **Afficher > console de débogage** (CTRL + MAJ + Y) dans la barre de menus. Si vous sélectionnez l’icône d’engrenage dans la fenêtre de débogage, un fichier de configuration de lancement (`launch.json`) sera créé pour vous permettre d’enregistrer les détails du programme d’installation du débogage. Pour plus d’informations, consultez [vs code le débogage](https://code.visualstudio.com/docs/nodejs/nodejs-debugging).

![Icône de débogage VS Code et de configuration de Launch. JSON](../images/vscode-debug-launch-configuration.png)

Pour en savoir plus sur Gatsby, consultez les [documents Gatsby. js](https://www.gatsbyjs.org/docs/).
