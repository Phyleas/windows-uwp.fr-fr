---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: f594600991f08a7dfda784ae127be2e6438dacbd
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314883"
---
Pour installer MongoDB :

1. Ouvrez votre terminal WSL (par ex. Ubuntu 18.04).
2. Mettez à jour vos packages Ubuntu : `sudo apt update`
3. Une fois les packages mis à jour, installez MongoDB avec : `sudo apt-get install mongodb`
4. Confirmez l’installation et récupérez le numéro de version : `mongod --version`

3 commandes sont à connaître une fois MongoDB installée :

1. `sudo service mongodb status` pour vérifier l’état de votre base de données.
2. `sudo service mongodb start` pour commencer à exécuter votre base de données.
3. `sudo service mongodb stop` pour arrêter l’exécution de votre base de données.

> [!NOTE]
> Vous pouvez voir la commande `sudo systemctl status mongodb` utilisée dans des tutoriels ou des articles. Afin d’alléger le système, WSL n’inclut pas `systemd` (système de gestion de service dans Linux). Au lieu de cela, il utilise SysVinit pour démarrer les services sur votre ordinateur. Vous ne devriez pas remarquer de différence. Toutefois, si un tutoriel recommande l’utilisation de `sudo systemctl`, utilisez plutôt `sudo /etc/init.d/`. Par exemple, `sudo systemctl status mongodb`, pour WSL ce serait `sudo /etc/inid.d/mongodb status`. Vous pouvez également utiliser `sudo service mongodb status`.

### <a name="run-your-mongo-database-in-a-local-server"></a>Exécuter votre base de données Mongo sur un serveur local

1. Vérifiez l’état de votre base de données : `sudo service mongodb status` Une réponse [Fail] doit s’afficher, sauf si vous avez déjà démarré votre base de données.

2. Démarrez votre base de données : `sudo service mongodb start` Vous devez maintenant voir une réponse [OK].

3. Vérifiez en vous connectant au serveur de base de données et en exécutant une commande de diagnostic : `mongo --eval 'db.runCommand({ connectionStatus: 1 })'` Cela génère la version actuelle de la base de données, l’adresse et le port du serveur, ainsi que la sortie de la commande relative à l’état. La valeur `1` du champ « OK » dans la réponse indique que le serveur fonctionne.

4. Pour arrêter l’exécution du service MongoDB, entrez : `sudo service mongodb stop`

> [!NOTE]
> MongoDB possède plusieurs paramètres par défaut, notamment le stockage des données dans/Data/DB et leur exécution sur le port 27017. En outre, `mongod` est le démon (processus hôte pour la base de données) et `mongo` est le shell de ligne de commande qui se connecte à une instance spécifique de `mongod`.
