---
title: Prise en main de NodeJS sous Windows pour les débutants
description: Guide conçu pour aider les débutants à se familiariser avec le développement Node.js sous Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, windows 10, microsoft, découvrir nodejs, node sous windows, node sous windows pour les débutants, développer avec node sous windows, développer avec nodejs sous windows
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 433eb5701696f590f10d8b3276481098b9ec073d
ms.sourcegitcommit: 8f9cea69f33b06166fec22677eaa43466352c14d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/04/2020
ms.locfileid: "75657081"
---
# <a name="get-started-using-nodejs-on-windows-for-beginners"></a>Prise en main de Node.js sous Windows pour les débutants

Si vous n'avez jamais utilisé Node.js, ce guide vous aidera à vous familiariser avec quelques notions de base.

## <a name="prerequisites"></a>Prérequis

Ce guide part du principe que vous avez déjà suivi les étapes de [configuration de votre environnement de développement Node.js sous la version native de Windows](./setup-on-windows.md), y compris les étapes suivantes :

- Installation d'un gestionnaire de versions Node.js
- Installation de Visual Studio Code

L'installation de Node.js sous Windows est la méthode la plus simple pour commencer à effectuer les opérations Node.js de base avec un minimum de configuration.

Dès que vous êtes prêt à utiliser Node.js pour développer des applications destinées à la production, ce qui implique généralement un déploiement sur un serveur Linux, nous vous recommandons de [configurer votre environnement de développement Node.js avec WSL2](./setup-on-wsl2.md). Bien qu'il soit possible de déployer des applications web sur des serveurs Windows, on [utilise généralement des serveurs Linux pour héberger les applications Node.js](https://azure.microsoft.com/develop/nodejs/).

## <a name="types-of-nodejs-applications"></a>Types d’applications Node.js

Node.js est un runtime JavaScript principalement utilisé pour créer des applications web. En d'autres termes, il s'agit d'une implémentation côté serveur de JavaScript utilisée pour écrire le backend d'une application. (Bien que de nombreux frameworks Node.js puissent également gérer le frontend.) Voici quelques exemples de ce que vous pouvez créer avec Node.js.

- **Applications monopages (SPA)**  : il s'agit d'applications web exécutées au sein d'un navigateur et qui n'ont pas besoin de recharger une page à chaque utilisation pour obtenir de nouvelles données. Par exemple, les applications de réseaux sociaux, les applications de messagerie électronique ou de cartographie, ou encore les outils de texte ou de dessin en ligne sont des applications monopages.
- **Applications en temps réel (RTA)**  : il s'agit d'applications web qui permettent aux utilisateurs de recevoir des informations dès qu'elles sont publiées par un auteur, évitant ainsi aux utilisateurs (ou aux logiciels) de rechercher périodiquement les mises à jour. Par exemple, les applications de messagerie instantanée ou les salles de conversation, les jeux multijoueurs en ligne exécutés dans un navigateur, les documents de collaboration en ligne, les espaces de stockage communautaires et les applications de visioconférence sont des applications en temps réel.
- **Applications de diffusion de données en continu** : il s'agit d'applications (ou de services) qui envoient des données/contenus à mesure qu'ils arrivent (ou qu'ils sont créés) tout en maintenant la connexion ouverte pour continuer à télécharger d'autres données, contenus ou composants, selon les besoins. Les applications de diffusion de vidéos et d'audio en continu en sont des exemples.
- **API REST** : il s'agit d'interfaces qui fournissent des données avec lesquelles l'application web de quelqu'un d'autre peut interagir. Par exemple, un service d'API Calendrier peut fournir les dates et heures d'un concert et celles-ci peuvent être utilisées par le site web d'événements locaux de quelqu'un d'autre.
- **Applications rendues côté serveur (SSR)**  : ces applications web peuvent à la fois s'exécuter sur le client (dans votre navigateur/le frontend) et sur le serveur (le backend), ce qui permet aux pages dynamiques d'afficher (générer du code HTML pour) le contenu connu et de saisir rapidement le contenu non connu à mesure qu'il est disponible. Celles-ci sont souvent appelées applications « isomorphes » ou « universelles ». Les applications SSR utilisent des méthodes des applications monopages (SPA) en ce sens qu'elles n'ont pas besoin d'être rechargées à chaque fois que vous les utilisez. Les applications SSR offrent des avantages qui peuvent ou non avoir de l'importance pour vous, comme le fait de faire apparaître le contenu de votre site dans les résultats de recherche Google et de fournir une image d'aperçu lorsque des liens d'accès à votre application sont partagés sur des réseaux sociaux tels que Twitter ou Facebook. Elles présentent néanmoins l'inconvénient potentiel de nécessiter un serveur Node.js qui doit être exécuté en permanence. À titre d'exemple, le type SSR peut convenir à application de réseau social qui prend en charge des événements que les utilisateurs souhaitent voir apparaître dans les résultats de recherche et sur les réseaux sociaux, tandis que le type SPA conviendra à une application de messagerie électronique. Vous pouvez également exécuter des applications rendues côté serveur autres que SPA, comme un blog WordPress. Comme vous pouvez le constater, les choses ne sont pas toujours simples. À vous de déterminer ce qui est important.
- **Outils en ligne de commande** : ceux-ci vous permettent d'automatiser les tâches répétitives, puis de distribuer votre outil au sein du vaste écosystème Node.js. cURL est un exemple d'outil en ligne de commande. Il permet de télécharger du contenu à partir d'une URL Internet. cURL est souvent utilisé pour installer des éléments comme Node.js ou, dans le cas présent, un gestionnaire de versions Node.js.
- **Programmation matérielle** : bien que moins populaire que les applications web, Node.js gagne toutefois en popularité pour les utilisations IoT, telles que la collecte de données à partir de capteurs, de balises, d'émetteurs, de moteurs ou de tout ce qui génère de grandes quantités de données. Node.js peut permettre la collecte de données, l'analyse de ces données, la communication entre un appareil et un serveur, et la prise de mesures basées sur l'analyse. NPM contient plus de 80 packages pour les contrôleurs Arduino, Raspberry Pi, Intel IoT Edison, divers capteurs et des appareils Bluetooth.

## <a name="try-using-nodejs-in-vs-code"></a>Essayer d'utiliser Node.js dans VS Code

1. Ouvrez l'interface de ligne de commande de votre choix (invite de commande, PowerShell ou autre) et créez un nouveau répertoire : `mkdir HelloNode`. Puis accédez au répertoire : `cd HelloNode`

2. Créez un fichier JavaScript nommé « app.js » comportant une variable « msg » : `echo var msg > app.js`

3. Ouvrez le répertoire et votre fichier app.js dans VS Code : `code .`

4. Ajoutez une variable de chaîne simple (« Hello World »), puis envoyez le contenu de la chaîne à votre console en l'entrant dans votre fichier « app.js » :

    ```js
    var msg = 'Hello World';
    console.log(msg);
    ```

5. Pour exécuter votre fichier « app.js » avec Node.js : ouvrez votre terminal dans VS Code en sélectionnant **Afficher** > **Terminal** (ou sélectionnez Ctrl + `, en utilisant le caractère d'impulsion). Si vous devez changer le terminal par défaut, sélectionnez le menu déroulant et choisissez **Sélectionner l'interpréteur de commandes par défaut**.

6. Dans le terminal, entrez : `node app.js`. La sortie suivante doit s'afficher : « Hello World ».

> [!NOTE]
> Notez que lorsque vous entrez `console` dans votre fichier « app.js », VS Code affiche les options prises en charge pour l'objet [`console`](https://developer.mozilla.org/docs/Web/API/Console) afin de vous permettre d'effectuer votre choix à l'aide d'IntelliSense. Essayez IntelliSense à l'aide d'autres [objets JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects).

> [!TIP]
> Essayez le nouveau [terminal Windows](https://github.com/microsoft/terminal/blob/master/doc/user-docs/index.md) si vous prévoyez d'utiliser plusieurs lignes de commande (Ubuntu, PowerShell, invite de commandes Windows, etc.) ou si vous souhaitez [personnaliser votre terminal](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md) (texte, couleurs d'arrière-plan, combinaisons de touches, volets multiples, etc.).

## <a name="set-up-a-basic-web-app-framework-by-using-express"></a>Configurer un framework d'application web de base à l'aide d'Express

Express est un framework Node.js minimal, flexible et rationalisé qui facilite le développement d'une application web capable de traiter plusieurs types de demandes, comme GET, PUT, POST et DELETE. Express est fourni avec un générateur d'application qui crée automatiquement une architecture de fichiers pour votre application.

Pour créer un projet avec Express.js :

1. Ouvrez l'interface de ligne de commande de votre choix (invite de commandes, PowerShell ou autre).
2. Créez un dossier pour le nouveau projet : `mkdir ExpressProjects`. Puis accédez à ce répertoire : `cd ExpressProjects`
3. Utilisez Express pour créer un modèle de projet HelloWorld : `npx express-generator HelloWorld --view=pug`

>[!NOTE]
> Nous utilisons ici la commande `npx` pour exécuter le package Node Express.js sans l'installer réellement (ou en l'installant temporairement selon le point de vue). Si vous essayez d'utiliser la commande `express` ou de vérifier la version d'Express installée à l'aide de `express --version`, vous recevrez une réponse indiquant qu'Express est introuvable. Si vous souhaitez procéder à une installation globale d'Express afin de pouvoir l'utiliser à volonté, utilisez : `npm install -g express-generator`. Pour consulter la liste des packages qui ont été installés par npm, utilisez : `npm list`. Ceux-ci seront répertoriés par profondeur (nombre de répertoires imbriqués). Les packages que vous avez installés seront à la profondeur 0. Les dépendances de ces packages seront à la profondeur 1, d'autres à la profondeur 2, et ainsi de suite. Pour plus d'informations, consultez [Différence entre npx et npm](https://stackoverflow.com/questions/50605219/difference-between-npx-and-npm) sur StackOverflow.

4. Examinez les fichiers et dossiers inclus dans le projet Express en ouvrant celui-ci dans VS Code : `code .`

   Les fichiers générés par Express créent une application web dont l'architecture qui peut sembler impressionnante au premier abord. La fenêtre **Explorer** de VS Code (accessible via Ctrl + Maj + E) montre que les fichiers et dossiers suivants ont été générés :

   - `bin`. Contient le fichier exécutable qui démarre votre application. Il démarre un serveur (sur le port 3000 si aucune alternative n'est fournie) et configure la gestion des erreurs de base. 
   - `public`. Contient tous les fichiers accessibles au public, y compris les fichiers JavaScript, les feuilles de style CSS, les fichiers de polices, les images et toute autre ressource dont les utilisateurs ont besoin lorsqu'ils se connectent à votre site web.
   - `routes`. Contient tous les gestionnaires de routage de l'application. Deux fichiers, `index.js` et `users.js`, sont automatiquement générés dans ce dossier pour servir d'exemples de séparation de la configuration de routage de votre application.
   - `views`. Contient les fichiers utilisés par votre moteur de modèle. Express est configuré pour rechercher une vue correspondante lorsque la méthode Render est appelée. Le moteur de modèle par défaut est Jade, mais Jade a été déconseillé en faveur de Pug. Nous avons donc utilisé l'indicateur `--view` pour modifier le moteur view (modèle). Pour afficher les options d'indicateur `--view`, entre autres, utilisez `express --help`.
   - `app.js`. Point de départ de votre application. Il charge tout et commence à servir les demandes des utilisateurs. Il s'agit en quelque sorte de la colle qui permet de maintenir les différentes pièces ensemble.
   - `package.json`. Contient la description du projet, le gestionnaire de scripts et le manifeste de l'application. Son rôle consiste principalement à de suivre les dépendances de votre application et leurs versions respectives.

5. Vous devez maintenant installer les dépendances qu'Express utilise pour créer et exécuter votre application Express HelloWorld (packages utilisés pour des tâches telles que l'exécution du serveur, comme défini dans le fichier `package.json`). Dans VS Code, ouvrez votre terminal en sélectionnant **Afficher** > **Terminal** (ou sélectionnez Ctrl + `, en utilisant le caractère d'impulsion), et assurez-vous que vous êtes toujours dans le répertoire du projet « HelloWorld ». Installez les dépendances du package Express :

```bash
npm install
```

6. À ce stade, le framework est configuré pour une application web de plusieurs pages qui a accès à une grande variété d'API, d'intergiciels et de méthodes utilitaires HTTP, ce qui facilite la création d'une API robuste. Démarrez l'application Express sur un serveur virtuel en entrant :

```bash
npx cross-env DEBUG=HelloWorld:* npm start
```

> [!TIP]
> Dans la partie `DEBUG=myapp:*` de la commande ci-dessus, vous indiquez à Node.js que vous souhaitez activer la journalisation à des fins de débogage. N'oubliez pas de remplacer « MyApp » par le nom de votre application. Vous trouverez le nom de votre application dans le fichier `package.json`, sous la propriété « Name ». `npx cross-env` permet de définir la variable d'environnement `DEBUG` sur n'importe quel terminal, mais vous pouvez également la définir en utilisant une méthode propre à votre terminal. La commande `npm start` indique à npm d'exécuter les scripts dans votre fichier `package.json`.

7. Vous pouvez maintenant afficher l'application exécutée en ouvrant un navigateur web et en accédant à : **localhost: 3000**

   ![Capture d'écran de l'application Express exécutée dans un navigateur](../images/express-app.png)

8. Maintenant que votre application Express HelloWorld s'exécute localement dans votre navigateur, essayez d'y apporter une modification en ouvrant le dossier « views » dans le répertoire de votre projet et en sélectionnant le fichier « index.pug ». Une fois le fichier ouvert, remplacez `h1= title` par `h1= "Hello World!"` et sélectionnez **Enregistrer** (Ctrl + S). Affichez votre modification en actualisant l'URL **localhost: 3000** sur votre navigateur web.

9. Pour arrêter l'exécution de votre application Express, sur votre terminal, entrez : **Ctrl + C**

## <a name="try-using-a-nodejs-module"></a>Essayer d’utiliser un module Node.js

Node.js dispose d'outils qui vous permettent de développer des applications web côté serveur, dont certaines sont intégrées et beaucoup d'autres sont disponibles via npm. Ces modules peuvent vous aider à accomplir de nombreuses tâches :

|Outil               |Rôle                                                                                                  |
|:----------------- |:---------------------------------------------------------------------------------------------------------|
|gm, sharp          |Manipulation d'images (modification, redimensionnement, compression, etc.) directement dans votre code JavaScript |
|PDFKit             |Génération de fichiers PDF                                                                                            |
|validator.js       |Validation de chaîne                                                                                         |
|imagemin, UglifyJS2|Minimisation                                                                                              |
|spritesmith        |Génération de feuilles Sprite                                                                                   |
|winston            |Journalisation                                                                                                  |
|commander.js       |Création d'applications en ligne de commande                                                                       |

Utilisons le module OS intégré pour obtenir des informations sur le système d'exploitation de votre ordinateur :

1) Sur votre ligne de commande, ouvrez l'interface CLI de Node.js. Vous verrez l'invite `>` indiquant que vous utilisez Node.js après avoir entré : `node`

2) Pour identifier le système d'exploitation que vous utilisez actuellement (qui doit renvoyer une réponse indiquant que vous êtes sous Windows), entrez : `os.platform()`

3) Pour vérifier l'architecture de votre processeur, entrez : `os.arch()`

4) Pour afficher les processeurs disponibles sur votre système, entrez : `os.cpus()`

5) Quittez l'interface CLI de Node.js en entrant `.exit` ou en sélectionnant deux fois Ctrl + C.

   > [!TIP]
   > Vous pouvez notamment utiliser le module OS de Node.js pour vérifier la plateforme et renvoyer une variable spécifique à celle-ci : Win32/.bat pour le développement de Windows, darwin/.sh pour Mac/unix, Linux, SunOS, etc. (par exemple, `var isWin = process.platform === "win32";`).

## <a name="next-steps"></a>Étapes suivantes

Dans ce guide, vous avez découvert les bases de ce que vous pouvez accomplir avec Node.js, vous avez essayé d'utiliser l'interface de ligne de commande de Node.js dans VS Code, vous avez créé une application web simple à l'aide d'Express.js avant de l'exécuter localement dans votre navigateur web, puis vous avez essayé d'utiliser quelques-uns des modules Node.js intégrés. Pour en savoir plus sur l'installation et l'utilisation des frameworks web les plus populaires de Node.js, passez au guide suivant qui couvre Next.js (framework web de rendu coté serveur basé sur React), Nuxt.js (framework web de rendu coté serveur basé sur Vue) et Gatsby (framework web de rendu statique basé sur React). Vous pouvez également apprendre à utiliser des bases de données MongoDB ou PostgreSQL, ou des conteneurs Docker.

- [Prise en main des frameworks web Node.js sous Windows](./web-frameworks.md)
- [Prise en main de la connexion des applications Node.js à une base de données](./databases.md)
- [Prise en main des conteneurs Docker avec Node.js](./containers.md)
