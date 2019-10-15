---
title: Prise en main de NodeJS sur Windows pour les débutants
description: Guide permettant aux débutants de commencer à utiliser le développement node. js sur Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, node. js, Windows 10, Microsoft, formation NodeJS, node sur Windows, node sur Windows pour les débutants, développer avec node sur Windows, Developer avec NodeJS sur Windows
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 24a2ea5288a627ed884d549ca3b6f9c6930ce34e
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72315133"
---
# <a name="get-started-using-nodejs-on-windows-for-beginners"></a>Prise en main de node. js sur Windows pour les débutants

Si vous êtes novice en utilisant node. js, ce guide vous aidera à vous familiariser avec certains concepts de base.

## <a name="prerequisites"></a>Prérequis

Ce guide part du principe que vous avez déjà effectué les étapes pour [configurer votre environnement de développement node. js avec WSL 2](./setup-on-wsl2.md), notamment :

- Installez Windows 10 Insider preview version 18932 ou ultérieure.
- Activez la fonctionnalité WSL 2 sur Windows.
- Installez une distribution Linux (Ubuntu 18,04 pour nos exemples). Vous pouvez le vérifier avec : `wsl lsb_release -a`
- Assurez-vous que votre distribution Ubuntu 18,04 s’exécute en mode WSL 2. (WSL peut exécuter des distributions en mode v1 ou v2.) Vous pouvez le vérifier en ouvrant PowerShell et en entrant : `wsl -l -v`
- Définir Ubuntu 18,04 comme distribution par défaut, à l’aide de PowerShell, avec : `wsl -s ubuntu 18.04`

> [!NOTE]
> Bien qu’il existe des étapes de configuration supplémentaires pour l’utilisation du sous-système Windows pour Linux, l’utilisation de WSL 2, avec VS Code et l’extension WSL distante, fournira le flux de travail de développement node. js le plus lisse, ainsi que l’alignement avec la majorité des outils, comment des articles, des didacticiels et des [environnements de déploiement](https://docs.microsoft.com/en-us/azure/javascript/tutorial-vscode-azure-app-service-node-01) . Toutefois, si vous êtes engagé à utiliser node. js sur Windows, consultez notre guide pour [configurer votre environnement de développement node. js directement sur Windows](./setup-on-windows.md). Si vous êtes dans la situation (peu rare) de la nécessité d’héberger une application node. js sur un serveur Windows, le scénario le plus courant semble [utiliser un proxy inverse](https://medium.com/intrinsic/why-should-i-use-a-reverse-proxy-if-node-js-is-production-ready-5a079408b2ca). Il existe deux façons de procéder : 1) [à l’aide de iisnode](https://harveywilliams.net/blog/installing-iisnode) ou [directement](https://dev.to/petereysermans/hosting-a-node-js-application-on-windows-with-iis-as-reverse-proxy-397b). Nous ne maintenons pas ces ressources et nous vous recommandons [d’utiliser des serveurs Linux pour héberger vos applications node. js](https://azure.microsoft.com/en-us/develop/nodejs/).

## <a name="types-of-nodejs-applications"></a>Types d’applications node. js

Node. js est un Runtime JavaScript principalement utilisé pour créer des applications Web. En d’autres termes, il s’agit d’une implémentation côté serveur de JavaScript utilisée pour écrire le backend d’une application. (Bien que de nombreuses infrastructures node. js puissent également gérer le serveur frontal.) Voici quelques exemples de ce que vous pouvez créer avec node. js.

- **Applications à page unique (spas)** : Il s’agit d’applications Web qui fonctionnent à l’intérieur d’un navigateur et qui n’ont pas besoin de recharger une page chaque fois que vous l’utilisez pour obtenir de nouvelles données. Par exemple, les applications de réseau social, les applications de messagerie ou de carte, les outils de texte ou de dessin en ligne, etc.
- **Applications en temps réel (RTA)** : Il s’agit d’applications Web qui permettent aux utilisateurs de recevoir des informations dès qu’elles sont publiées par un auteur, plutôt que de demander à l’utilisateur (ou au logiciel) de vérifier périodiquement une source pour les mises à jour. Voici quelques exemples de RTA : les applications de messagerie instantanée ou les salles de conversation, les jeux en ligne multijoueurs qui peuvent être lus dans le navigateur, les documents de collaboration en ligne, le stockage de la Communauté, les applications de conférence vidéo, etc.
- **Applications de streaming de données**: Il s’agit d’applications (ou de services) qui envoient des données/contenu au fur et à mesure de leur arrivée (ou qui sont créées) tout en maintenant la connexion ouverte pour poursuivre le téléchargement de données, de contenu ou de composants supplémentaires en fonction des besoins. Voici quelques exemples d’applications de diffusion vidéo et audio.
- **API REST**: Il s’agit d’interfaces qui fournissent des données pour que l’application Web d’un autre utilisateur interagisse avec. Par exemple, un service d’API de calendrier peut fournir des dates et des heures pour une salle de concert qui pourrait être utilisée par le site Web des événements locaux d’un autre utilisateur.
- **Applications rendues côté serveur (SSRS)** : Ces applications Web peuvent s’exécuter à la fois sur le client (dans votre navigateur/le frontal) et sur le serveur (le serveur principal), ce qui permet aux pages qui sont dynamiques de s’afficher (générer du code HTML pour) le contenu connu et de récupérer rapidement le contenu qui n’est pas connu comme disponible. Ils sont souvent appelés « isomorphes » ou « applications universelles ». SSRs utilise les méthodes SPA dans le fait qu’elles n’ont pas besoin d’être rechargées à chaque fois que vous l’utilisez. SRAs, toutefois, offrent quelques avantages qui peuvent ou non être importants pour vous, tels que le fait que le contenu de votre site apparaisse dans les résultats de recherche Google et la fourniture d’une image d’aperçu lorsque des liens vers votre application sont partagés sur des réseaux sociaux tels que Twitter ou Facebook. Un des inconvénients potentiels est qu’ils nécessitent un serveur node. js en cours d’exécution. En termes d’exemples, une application de réseau social qui prend en charge les événements que les utilisateurs souhaitent voir apparaître dans les résultats de recherche et les réseaux sociaux peut tirer parti d’SSR, tandis qu’une application de courrier électronique peut être correcte. Vous pouvez également exécuter des applications non-SPA rendues par le serveur, ce qui me ressemble à un blog WordPress. Comme vous pouvez le voir, les choses peuvent être compliquées, vous devez simplement décider de ce qui est important.
- **Outils en ligne de commande**: Elles vous permettent d’automatiser les tâches répétitives, puis de distribuer votre outil sur l’écosystème node. js de grande échelle. Un exemple d’outil en ligne de commande est la boucle, qui est l’URL du client et qui est utilisée pour télécharger le contenu à partir d’une URL Internet. la boucle est souvent utilisée pour installer des éléments tels que node. js ou, dans le cas présent, un gestionnaire de version node. js.
- **Programmation matérielle**: Bien qu’elles ne soient pas très populaires en ce qui concerne les applications Web, node. js augmente la popularité des utilisations IoT, telles que la collecte de données à partir de capteurs, de balises, d’émetteurs, de moteurs ou tout ce qui génère de grandes quantités de données. Node. js peut activer la collecte de données, analyser ces données, communiquer entre un appareil et un serveur et prendre des mesures en fonction de l’analyse. NPM contient plus de 80 packages pour les contrôleurs Arduino, Raspberry pi, Intel IoT Edison, divers capteurs et périphériques Bluetooth.

## <a name="try-using-nodejs-in-vs-code"></a>Essayez d’utiliser node. js dans VS Code

1. Ouvrez votre terminal Ubuntu et créez un répertoire : `mkdir HelloNode`, puis entrez le répertoire : `cd HelloNode`

2. Créez un fichier JavaScript vide nommé « App. js » : `touch app.js`

3. Ouvrez le répertoire et le fichier vide dans VS Code :: `code .`

4. Créez une variable de chaîne simple dans App. js et envoyez le contenu de la chaîne à la console en l’entrant dans votre fichier’App. js' :

    ```js
    var msg = 'Hello World';
    console.log(msg);
    ```

5. Pour exécuter votre fichier « app. js » avec node. js. Ouvrez votre terminal Ubuntu directement à l’intérieur de VS Code en sélectionnant **Afficher**le**Terminal**  >  (ou appuyez sur Ctrl + ', en utilisant le caractère d’impulsion). Si vous avez besoin de modifier le terminal par défaut, sélectionnez le menu déroulant et choisissez **Sélectionner l’interpréteur de commandes par défaut**. Sélectionnez ensuite **WSL** comme Shell terminal par défaut à utiliser avec vs code.

6. Dans le terminal, entrez : `node app.js`. La sortie doit s’afficher : « Hello World ».

> [!NOTE]
> Notez que, étant donné que vous avez déjà installé l’extension WSL distante, votre répertoire s’ouvre dans un environnement distant qui s’exécute sur le système Ubuntu Linux comme indiqué par l’onglet vert dans la partie inférieure gauche de la fenêtre de VS Code. En outre, Notez que lorsque vous tapez `console` dans votre fichier « app. js », VS Code affiche les options prises en charge relatives à l’objet [`console`](https://developer.mozilla.org/docs/Web/API/Console) pour que vous choisissiez d’utiliser IntelliSense. Essayez d’expérimenter avec IntelliSense à l’aide d’autres [objets JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects).

> [!TIP]
> Envisagez d’essayer le nouveau [terminal Windows](https://github.com/microsoft/terminal/blob/master/doc/user-docs/index.md) si vous envisagez d’utiliser plusieurs lignes de commande (Ubuntu, PowerShell, invite de commandes Windows, etc.) ou si vous souhaitez [personnaliser votre terminal](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md), y compris le texte, les couleurs d’arrière-plan, les combinaisons de touches, etc.

## <a name="set-up-a-basic-web-app-framework-by-using-express"></a>Configurer une infrastructure d’application Web de base à l’aide de Express

Express est un Framework node. js minimal, flexible et rationalisé qui facilite le développement d’une application Web capable de gérer plusieurs types de requêtes, par exemple obtenir, placer, poster et supprimer. Express est fourni avec un générateur d’applications qui créera automatiquement une architecture de fichiers pour votre application.

Pour créer un projet avec Express. js :

1. Ouvrez votre terminal WSL (Ubuntu 18,04).
2. Créez un dossier de projet : `mkdir ExpressProjects`, puis entrez ce répertoire : `cd ExpressProjects`.
3. Utilisez Express pour créer un modèle de projet HelloWorld : `npx express-generator HelloWorld --view=pug`.

>[!NOTE]
> Nous utilisons la commande `npx` ici pour exécuter le package de nœud Express. js sans réellement l’installer (ou en l’installant temporairement en fonction de la façon dont vous souhaitez le considérer). Si vous essayez d’utiliser la commande `express` ou de vérifier la version d’Express installée à l’aide de : `express --version`, vous recevrez une réponse indiquant que Express est introuvable. Si vous souhaitez installer Express globalement pour une utilisation excessive, utilisez : `npm install -g express-generator`. Vous pouvez afficher la liste des packages qui ont été installés par NPM à l’aide de `npm list`. Elles sont classées par profondeur (le nombre de répertoires imbriqués). Les packages que vous avez installés seront à la profondeur 0. Les dépendances de ce package seront à la profondeur 1, d’autres dépendances à la profondeur 2, et ainsi de suite. Pour plus d’informations, consultez [différence entre npx et NPM ?](https://stackoverflow.com/questions/50605219/difference-between-npx-and-npm) sur StackOverflow.

4. Examinez les fichiers et les dossiers inclus dans l’ouverture du projet dans VS Code, avec : `code .`

   Les fichiers générés par Express créeront une application Web qui utilise une architecture qui peut paraître un peu écrasant dans un premier temps. Vous verrez dans votre fenêtre d' **Explorateur** de vs code (Ctrl + Maj + E pour afficher) que les fichiers et dossiers suivants ont été générés :

   - `bin` . Contient le fichier exécutable qui démarre votre application. Il active un serveur (sur le port 3000 si aucune alternative n’est fournie) et configure la gestion des erreurs de base. 
   - `public` . Contient tous les fichiers accessibles publiquement, y compris les fichiers JavaScript, les feuilles de style CSS, les fichiers de polices, les images et toutes les autres ressources dont les utilisateurs ont besoin lorsqu’ils se connectent à votre site Web.
   - `routes` . Contient tous les gestionnaires d’itinéraires pour l’application. Deux fichiers, `index.js` et `users.js`, sont générés automatiquement dans ce dossier pour servir d’exemples montrant comment séparer la configuration de l’itinéraire de votre application.
   - `views` . Contient les fichiers utilisés par votre moteur de modèle. Express est configuré pour rechercher une vue correspondante lorsque la méthode Render est appelée. Le moteur de modèle par défaut est Jade, mais Jade a été déconseillé en faveur de Pug. nous avons donc utilisé l’indicateur `--view` pour changer le moteur de vue (modèle). Vous pouvez voir les options d’indicateur `--view`, etc., à l’aide de `express --help`.
   - `app.js` . Point de départ de votre application. Il charge tout et commence à traiter les demandes des utilisateurs. C’est fondamentalement le collage qui contient toutes les parties.
   - `package.json` . Contient la description du projet, le gestionnaire de scripts et le manifeste de l’application. Son objectif principal est de suivre les dépendances de votre application et leurs versions respectives.

5. Vous devez maintenant installer les dépendances utilisées par Express pour générer et exécuter votre application HelloWorld Express (les packages utilisés pour des tâches telles que l’exécution du serveur, comme défini dans le fichier `package.json`). Dans VS Code, ouvrez votre terminal WSL en sélectionnant **Afficher**le**Terminal**  >  (ou appuyez sur Ctrl + ', en utilisant le caractère d’impulsion), vérifiez que vous êtes toujours dans le répertoire du projet « HelloWorld ». Installez les dépendances du package Express avec :

```bash
npm install
```

6. À ce stade, le Framework est configuré pour une application Web de plusieurs pages qui a accès à un grand nombre d’API et de méthodes d’utilitaire HTTP et d’intergiciels (middleware), ce qui facilite la création d’une API robuste. Démarrez l’application Express sur un serveur virtuel en entrant :

```bash
DEBUG=HelloWorld:* npm start
```

> [!TIP]
> La partie `DEBUG=myapp:*` de la commande ci-dessus signifie que vous indiquez à node. js que vous souhaitez activer la journalisation à des fins de débogage. N’oubliez pas de remplacer « MyApp » par le nom de votre application. Vous pouvez trouver le nom de votre application dans le fichier Package. JSON sous la propriété « Name ». La commande `npm start` indique à NPM d’exécuter les scripts dans votre fichier Package. JSON.

7. Vous pouvez maintenant afficher l’application en cours d’exécution en ouvrant un navigateur Web et en accédant à : **localhost : 3000**

   ![Capture d’écran de l’application Express s’exécutant dans un navigateur](../images/express-app.png)

8. Maintenant que votre application Express HelloWorld est exécutée localement dans votre navigateur, essayez d’effectuer une modification en ouvrant le dossier « views » dans le répertoire de votre projet et en sélectionnant le fichier « index. pug ». Une fois ouvert, remplacez `h1= title` par `h1= "Hello World!"` et en sélectionnant **Enregistrer** (Ctrl + S). Affichez vos modifications en actualisant l’URL **localhost : 3000** sur votre navigateur Web.

9. Pour arrêter l’exécution de votre application Express, sur votre terminal, entrez : **Ctrl + C**

## <a name="try-using-a-nodejs-module"></a>Essayez d’utiliser un module node. js

Node. js contient des outils pour vous aider à développer des applications Web côté serveur, des fonctionnalités intégrées et beaucoup plus disponibles via NPM. Ces modules peuvent vous aider dans de nombreuses tâches :

|Tool               |Rôle                                                                                                  |
|:----------------- |:---------------------------------------------------------------------------------------------------------|
|GM, dièse          |Manipulation d’images, y compris la modification, le redimensionnement, la compression, etc., directement dans votre code JavaScript |
|PDFKit             |Génération PDF                                                                                            |
|validateur. js       |Validation de chaîne                                                                                         |
|imagemin, UglifyJS2|Réduction                                                                                              |
|spritesmith        |Génération de la feuille Sprite                                                                                   |
|Winston            |Journalisation                                                                                                  |
|commandant. js       |Création d’applications en ligne de commande                                                                       |

Nous allons utiliser le module de système d’exploitation intégré pour obtenir des informations sur le système d’exploitation de votre ordinateur :

1) Dans votre terminal WSL (Ubuntu 18,04), ouvrez l’interface CLI node. js. Vous verrez l’invite de `>` vous indiquant que vous utilisez node. js après avoir entré : `node`

2) Pour identifier le système d’exploitation que vous utilisez actuellement (qui doit retourner une réponse vous informant que vous êtes sous Windows), entrez : `os.platform()`

3) Pour vérifier l’architecture de votre UC, entrez : `os.arch()`

4) Pour afficher les unités centrales disponibles sur votre système, entrez : `os.cpus()`

5) Laissez l’interface CLI node. js en entrant `.exit` ou en sélectionnant Ctrl + C à deux reprises.

   > [!TIP]
   > Vous pouvez utiliser le module de système d’exploitation node. js pour effectuer des opérations telles que vérifier la plateforme et retourner une variable spécifique à la plateforme : Win32/. bat pour le développement Windows, Darwin/. sh pour Mac/UNIX, Linux, SunOS, etc. (par exemple, `var isWin = process.platform === "win32";`).

## <a name="next-steps"></a>Étapes suivantes

Dans ce guide, vous avez appris quelques notions de base sur ce que vous pouvez faire avec node. js, tenté d’utiliser la ligne de commande node. js dans VS Code, créé une application Web simple avec Express. js et l’ai exécutée localement dans votre navigateur Web, puis essayé d’utiliser quelques modules node. js intégrés. Pour en savoir plus sur la façon d’installer et d’utiliser des infrastructures Web node. js populaires, passez au guide suivant qui couvre Next. js (une infrastructure Web de type serveur), Nuxt. js (une infrastructure Web avec affichage serveur basée sur vue) et Gatsby (a Framework Web rendu statiquement basé sur REACT). Vous pouvez également passer à l’apprentissage de l’utilisation des bases de données MongoDB ou PostgreSQL ou des conteneurs de l’Ancrable.

- [Prise en main des infrastructures Web node. js sur Windows](./web-frameworks.md)
- [Prise en main de la connexion des applications node. js à une base de données](./databases.md)
- [Prise en main des conteneurs de l’arrimeur avec node. js](./containers.md)
