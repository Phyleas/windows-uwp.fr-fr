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
ms.openlocfilehash: 433eb5701696f590f10d8b3276481098b9ec073d
ms.sourcegitcommit: 8f9cea69f33b06166fec22677eaa43466352c14d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/04/2020
ms.locfileid: "75657081"
---
# <a name="get-started-using-nodejs-on-windows-for-beginners"></a>Prise en main de node. js sur Windows pour les débutants

Si vous êtes novice en utilisant node. js, ce guide vous aidera à vous familiariser avec certains concepts de base.

## <a name="prerequisites"></a>Prérequis

Ce guide part du principe que vous avez déjà effectué les étapes pour [configurer votre environnement de développement node. js sur des fenêtres natives](./setup-on-windows.md), notamment :

- Installez un gestionnaire de version node. js.
- Installez Visual Studio Code.

L’installation de node. js directement sur Windows est la méthode la plus simple pour commencer à effectuer des opérations node. js de base avec une quantité minimale de configuration.

Une fois que vous êtes prêt à utiliser node. js pour développer des applications pour la production, ce qui implique généralement le déploiement sur un serveur Linux, nous vous recommandons de [configurer votre environnement de développement node. js avec WSL2](./setup-on-wsl2.md). Bien qu’il soit possible de déployer des applications Web sur des serveurs Windows, il est bien plus courant d' [utiliser des serveurs Linux pour héberger vos applications node. js](https://azure.microsoft.com/develop/nodejs/).

## <a name="types-of-nodejs-applications"></a>Types d’applications Node.js

Node. js est un Runtime JavaScript principalement utilisé pour créer des applications Web. En d’autres termes, il s’agit d’une implémentation côté serveur de JavaScript utilisée pour écrire le backend d’une application. (Bien que de nombreuses infrastructures node. js puissent également gérer le serveur frontal.) Voici quelques exemples de ce que vous pouvez créer avec node. js.

- **Applications à page unique (spas)** : il s’agit d’applications Web qui fonctionnent à l’intérieur d’un navigateur et qui n’ont pas besoin de recharger une page chaque fois que vous l’utilisez pour obtenir de nouvelles données. Par exemple, les applications de réseau social, les applications de messagerie ou de carte, les outils de texte ou de dessin en ligne, etc.
- **Applications en temps réel (RTA)** : il s’agit d’applications Web qui permettent aux utilisateurs de recevoir des informations dès qu’elles sont publiées par un auteur, plutôt que de demander à l’utilisateur (ou au logiciel) de vérifier périodiquement une source pour les mises à jour. Voici quelques exemples de RTA : les applications de messagerie instantanée ou les salles de conversation, les jeux en ligne multijoueurs qui peuvent être lus dans le navigateur, les documents de collaboration en ligne, le stockage de la Communauté, les applications de conférence vidéo, etc.
- **Applications de streaming des données**: il s’agit d’applications (ou de services) qui envoient des données/contenu au fur et à mesure de leur arrivée (ou qui sont créées) tout en maintenant la connexion ouverte pour continuer à télécharger des données, du contenu ou des composants en fonction des besoins. Voici quelques exemples d’applications de diffusion vidéo et audio.
- **API REST**: il s’agit d’interfaces qui fournissent des données pour que l’application Web d’une autre personne interagisse avec. Par exemple, un service d’API de calendrier peut fournir des dates et des heures pour une salle de concert qui pourrait être utilisée par le site Web des événements locaux d’un autre utilisateur.
- **Applications rendues côté serveur (SSRS)** : ces applications Web peuvent s’exécuter à la fois sur le client (dans votre navigateur/le frontal) et sur le serveur (le serveur principal), ce qui permet aux pages qui sont dynamiques à s’afficher (générer du code HTML pour) le contenu connu et de récupérer rapidement le contenu qui n’est pas connu comme disponible. Ils sont souvent appelés « isomorphes » ou « applications universelles ». SSRs utilise les méthodes SPA dans le fait qu’elles n’ont pas besoin d’être rechargées à chaque fois que vous l’utilisez. SSRs, cependant, offre quelques avantages qui peuvent ou non être importants pour vous, tels que le fait que le contenu de votre site apparaisse dans les résultats de recherche Google et la fourniture d’une image d’aperçu lorsque des liens vers votre application sont partagés sur des réseaux sociaux tels que Twitter ou Facebook. Un des inconvénients potentiels est qu’ils nécessitent un serveur node. js en cours d’exécution. En termes d’exemples, une application de réseau social qui prend en charge les événements que les utilisateurs souhaitent voir apparaître dans les résultats de recherche et les réseaux sociaux peut tirer parti d’SSR, tandis qu’une application de courrier électronique peut être correcte. Vous pouvez également exécuter des applications non-SPA rendues par le serveur, ce qui me ressemble à un blog WordPress. Comme vous pouvez le voir, les choses peuvent être compliquées, vous devez simplement décider de ce qui est important.
- **Outils en ligne de commande**: ils vous permettent d’automatiser les tâches répétitives, puis de distribuer votre outil sur l’écosystème node. js de grande échelle. Un exemple d’outil en ligne de commande est la boucle, qui est l’URL du client et qui est utilisée pour télécharger le contenu à partir d’une URL Internet. la boucle est souvent utilisée pour installer des éléments tels que node. js ou, dans le cas présent, un gestionnaire de version node. js.
- **Programmation matérielle**: bien qu’elles ne soient pas très populaires en ce qui concerne les applications Web, node. js augmente la popularité des utilisations IOT, telles que la collecte de données à partir de capteurs, de balises, d’émetteurs, de moteurs ou tout ce qui génère de grandes quantités de données. Node. js peut activer la collecte de données, analyser ces données, communiquer entre un appareil et un serveur et prendre des mesures en fonction de l’analyse. NPM contient plus de 80 packages pour les contrôleurs Arduino, Raspberry pi, Intel IoT Edison, divers capteurs et périphériques Bluetooth.

## <a name="try-using-nodejs-in-vs-code"></a>Essayez d’utiliser node. js dans VS Code

1. Ouvrez la ligne de commande (invite de commandes, PowerShell ou autre que vous préférez) et créez un répertoire : `mkdir HelloNode`, puis entrez le répertoire : `cd HelloNode`

2. Créez un fichier JavaScript nommé « App. js » avec une variable nommée « MSG » à l’intérieur de : `echo var msg > app.js`

3. Ouvrez le répertoire et votre fichier app. js dans VS Code :: `code .`

4. Ajoutez une variable de chaîne simple (« Hello World »), puis envoyez le contenu de la chaîne à votre console en entrant celui-ci dans votre fichier « app. js » :

    ```js
    var msg = 'Hello World';
    console.log(msg);
    ```

5. Pour exécuter votre fichier « app. js » avec node. js. Ouvrez votre terminal directement à l’intérieur de VS Code en sélectionnant **Afficher** le **Terminal** > (ou appuyez sur Ctrl + ', en utilisant le caractère d’impulsion). Si vous avez besoin de modifier le terminal par défaut, sélectionnez le menu déroulant et choisissez **Sélectionner l’interpréteur de commandes par défaut**.

6. Dans le terminal, entrez : `node app.js`. La sortie doit s’afficher : « Hello World ».

> [!NOTE]
> Notez que lorsque vous tapez `console` dans votre fichier « app. js », VS Code affiche les options prises en charge relatives à l’objet [`console`](https://developer.mozilla.org/docs/Web/API/Console) pour que vous choisissiez d’utiliser IntelliSense. Essayez d’expérimenter avec IntelliSense à l’aide d’autres [objets JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects).

> [!TIP]
> Essayez le nouveau [terminal Windows](https://github.com/microsoft/terminal/blob/master/doc/user-docs/index.md) si vous envisagez d’utiliser plusieurs lignes de commande (Ubuntu, PowerShell, invite de commandes Windows, etc.) ou si vous souhaitez [personnaliser votre terminal](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md), y compris le texte, les couleurs d’arrière-plan, les combinaisons de touches, les volets de fenêtre multiples, etc.

## <a name="set-up-a-basic-web-app-framework-by-using-express"></a>Configurer une infrastructure d’application web de base avec Express

Express est une infrastructure Node.js minimale, flexible et rationalisée qui facilite le développement d’une application web pouvant traiter plusieurs types de requêtes, notamment GET, PUT, POST et DELETE. Express est fourni avec un générateur d’applications qui crée automatiquement une architecture de fichiers pour votre application.

Pour créer un projet avec Express. js :

1. Ouvrez la ligne de commande (invite de commandes, PowerShell ou tout ce que vous préférez).
2. Créez un dossier de projet : `mkdir ExpressProjects` et entrez ce répertoire : `cd ExpressProjects`
3. Utilisez Express pour créer un modèle de projet HelloWorld : `npx express-generator HelloWorld --view=pug`

>[!NOTE]
> Nous utilisons la commande `npx` ici pour exécuter le package de nœud Express. js sans réellement l’installer (ou en l’installant temporairement en fonction de la façon dont vous souhaitez le considérer). Si vous essayez d’utiliser la commande `express` ou de vérifier la version d’Express installée à l’aide de : `express --version`, vous recevrez une réponse indiquant que Express est introuvable. Si vous souhaitez installer Express globalement pour une utilisation excessive, utilisez : `npm install -g express-generator`. Vous pouvez afficher la liste des packages qui ont été installés par NPM à l’aide de `npm list`. Ils sont triés par profondeur (nombre de répertoires imbriqués). Les packages que vous avez installés seront à la profondeur 0. Les dépendances de ce package seront à la profondeur 1, les dépendances supplémentaires à la profondeur 2, et ainsi de suite. Pour plus d’informations, consultez [différence entre npx et NPM ?](https://stackoverflow.com/questions/50605219/difference-between-npx-and-npm) sur StackOverflow.

4. Examinez les fichiers et les dossiers inclus dans le projet en ouvrant le projet dans VS Code, avec : `code .`

   Les fichiers générés par Express créent une application web qui utilise une architecture en apparence un peu massive. Vous verrez dans votre fenêtre d' **Explorateur** de vs code (Ctrl + Maj + E pour afficher) que les fichiers et dossiers suivants ont été générés :

   - `bin`. contient le fichier exécutable qui lance l’application. Il déclenche un serveur (sur le port 3000 si aucun autre n’est indiqué) et configure la gestion des erreurs de base. 
   - `public`. contient tous les fichiers publiquement accessibles, notamment les fichiers JavaScript, les feuilles de style CSS, les fichiers de police, les images et toutes les autres ressources nécessaires à tous ceux qui se connectent au site web.
   - `routes`. contient tous les gestionnaires de routage de l’application. Deux fichiers, `index.js` et `users.js`, sont automatiquement générés dans ce dossier pour servir d’exemples illustrant la séparation de la configuration du routage de l’application.
   - `views`. contient les fichiers utilisés par le moteur de modèle. Express est configuré pour rechercher à cet endroit une vue correspondante lorsque la méthode de rendu est appelée. Le moteur de modèle par défaut est Jade, mais il a été déprécié au profit de Pug. Nous utilisons donc l’indicateur `--view` pour modifier le moteur d’affichage (de modèle). Pour voir les options d’indicateur `--view`, entre autres, utilisez `express --help`.
   - `app.js`. point de départ de votre application. Il charge tout et commence à traiter les demandes des utilisateurs. C’est en somme le ciment qui maintient toutes les pièces ensemble.
   - `package.json`. contient la description du projet, le gestionnaire de scripts et le manifeste de l’application. Son rôle principal est d’assurer le suivi des dépendances de l’application et de leurs versions respectives.

5. Vous devez maintenant installer les dépendances utilisées par Express pour générer et exécuter votre application HelloWorld Express (les packages utilisés pour des tâches telles que l’exécution du serveur, comme défini dans le fichier `package.json`). Dans VS Code, ouvrez votre terminal en sélectionnant **afficher** > **Terminal** (ou appuyez sur Ctrl + ', en utilisant le caractère d’espacement), assurez-vous que vous êtes toujours dans le répertoire du projet « HelloWorld ». Installez les dépendances du package Express avec :

```bash
npm install
```

6. L’infrastructure est maintenant définie pour une application web de plusieurs pages ayant accès à un large éventail d’API, de méthodes utilitaires HTTP et d’intergiciels (middleware), ce qui facilite la création d’une API robuste. Démarrez l’application Express sur un serveur virtuel en entrant :

```bash
npx cross-env DEBUG=HelloWorld:* npm start
```

> [!TIP]
> La `DEBUG=myapp:*` partie de la commande ci-dessus signifie que vous indiquez à node. js que vous souhaitez activer la journalisation à des fins de débogage. N’oubliez pas de remplacer « MyApp » par le nom de votre application. Vous pouvez trouver le nom de votre application dans le fichier `package.json` sous la propriété « Name ». L’utilisation de `npx cross-env` définit la variable d’environnement `DEBUG` dans n’importe quel terminal, mais vous pouvez également la définir avec votre méthode de terminal spécifique. La commande `npm start` indique à NPM d’exécuter les scripts dans votre fichier `package.json`.

7. Vous pouvez maintenant afficher l’application en cours d’exécution en ouvrant un navigateur Web et en accédant à : **localhost : 3000**

   ![Capture d’écran de l’application Express en cours d’exécution dans un navigateur](../images/express-app.png)

8. Maintenant que votre application Express HelloWorld est exécutée localement dans votre navigateur, essayez d’effectuer une modification en ouvrant le dossier « views » dans le répertoire de votre projet et en sélectionnant le fichier « index. pug ». Une fois ouvert, remplacez `h1= title` par `h1= "Hello World!"` et en sélectionnant **Enregistrer** (Ctrl + S). Affichez vos modifications en actualisant l’URL **localhost : 3000** sur votre navigateur Web.

9. Pour arrêter l’exécution de votre application Express, sur votre terminal, entrez : **Ctrl + C**

## <a name="try-using-a-nodejs-module"></a>Essayer d’utiliser un module Node.js

Node.js comprend des outils servant à faciliter le développement d’applications web côté serveur, dont certains sont intégrés et beaucoup d’autres disponibles avec npm. Ces modules sont utiles pour de nombreuses tâches :

|Outil               |Rôle                                                                                                  |
|:----------------- |:---------------------------------------------------------------------------------------------------------|
|gm, sharp          |Manipulation d’images (modification, redimensionnement, compression, etc.) directement dans le code JavaScript |
|PDFKit             |Génération de PDF                                                                                            |
|validator.js       |Validation de chaînes                                                                                         |
|imagemin, UglifyJS2|Minimisation                                                                                              |
|spritesmith        |Génération de feuilles de sprites                                                                                   |
|winston            |Journalisation                                                                                                  |
|commander.js       |Création d’applications en ligne de commande                                                                       |

Utilisons le module de système d’exploitation intégré pour obtenir des informations sur le système d’exploitation de l’ordinateur :

1) Dans votre ligne de commande, ouvrez l’interface CLI node. js. Vous verrez l' `>` invite vous indiquant que vous utilisez node. js après avoir entré : `node`

2) Pour identifier le système d’exploitation que vous utilisez actuellement (qui doit retourner une réponse vous informant que vous êtes sous Windows), entrez : `os.platform()`

3) Pour vérifier l’architecture de votre UC, entrez : `os.arch()`

4) Pour afficher les unités centrales disponibles sur votre système, entrez : `os.cpus()`

5) Quittez l’interface CLI Node.js en entrant `.exit` ou en sélectionnant Ctrl-C deux fois.

   > [!TIP]
   > Vous pouvez utiliser le module de système d’exploitation node. js pour effectuer des opérations telles que vérifier la plateforme et retourner une variable spécifique à une plateforme : Win32/. bat pour le développement Windows, Darwin/. sh pour Mac/UNIX, Linux, SunOS, etc. (par exemple, `var isWin = process.platform === "win32";`).

## <a name="next-steps"></a>Étapes suivantes

Dans ce guide, vous avez appris quelques notions de base sur ce que vous pouvez faire avec node. js, tenté d’utiliser la ligne de commande node. js dans VS Code, créé une application Web simple avec Express. js et l’ai exécutée localement dans votre navigateur Web, puis essayé d’utiliser quelques modules node. js intégrés. Pour en savoir plus sur la façon d’installer et d’utiliser des infrastructures Web node. js populaires, passez au guide suivant qui couvre Next. js (une infrastructure Web de type serveur), Nuxt. js (une infrastructure Web avec affichage serveur basée sur vue) et Gatsby (a Framework Web rendu statiquement basé sur REACT). Vous pouvez également passer à l’apprentissage de l’utilisation des bases de données MongoDB ou PostgreSQL ou des conteneurs de l’Ancrable.

- [Prise en main des infrastructures Web node. js sur Windows](./web-frameworks.md)
- [Prise en main de la connexion des applications node. js à une base de données](./databases.md)
- [Prise en main des conteneurs de l’arrimeur avec node. js](./containers.md)
