---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: aa1da2a65f95d92e895533ed37426b5e454c255b
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314893"
---
Pour installer PostgreSQL :

1. Ouvrez votre terminal WSL (par ex. Ubuntu 18,04).
2. Mettre à jour vos packages Ubuntu : `sudo apt update`
3. Une fois que les packages ont été mis à jour, installez PostgreSQL (et le package de contribution qui comporte des utilitaires utiles) avec : `sudo apt install postgresql postgresql-contrib`
4. Confirmez l’installation et récupérez le numéro de version : `psql --version`

Il y a 3 commandes que vous devez savoir une fois que PostgreSQL est installé :

1. `sudo service postgresql status` pour vérifier l’état de votre base de données.
2. `sudo service postgresql start` pour commencer à exécuter votre base de données.
3. `sudo service postgresql stop` pour arrêter l’exécution de votre base de données.

### <a name="postgresql-user-setup"></a>Configuration utilisateur PostgreSQL

L’utilisateur administrateur par défaut, `postgres`, a besoin d’un mot de passe attribué afin de se connecter à une base de données. Pour définir un mot de passe :

1. Entrez la commande : `sudo passwd postgres`
2. Vous êtes invité à entrer votre nouveau mot de passe.
3. Fermez et rouvrez votre terminal.

### <a name="run-postgresql-with-psql-shell"></a>Exécuter PostgreSQL avec l’interpréteur de commandes psql

[psql](https://www.postgresql.org/docs/10/app-psql.html) est un serveur frontal basé sur un terminal vers PostgreSQL. Elle vous permet de taper des requêtes de manière interactive, de les émettre à PostgreSQL et de consulter les résultats de la requête. En guise d’alternative, l’entrée peut provenir d’un fichier. En outre, il fournit un certain nombre de méta-commandes et diverses fonctionnalités de type Shell pour faciliter l’écriture de scripts et l’automatisation d’un large éventail de tâches.

Pour démarrer l’interpréteur de commandes psql :

1. Démarrez votre service Postgres : `sudo service postgresql start`
2. Connectez-vous au service postgres et ouvrez l’interpréteur de commandes psql : `sudo -u postgres psql`

Une fois que vous avez correctement entré l’interpréteur de commandes psql, la modification de la ligne de commande se présente comme suit : `postgres=#`

> [!NOTE]
> Vous pouvez également ouvrir l’interpréteur de commandes psql en basculant vers l’utilisateur postgres avec : `su - postgres`, puis en entrant la commande : `psql`.

Pour quitter postgres = # Enter : `\q` ou utilisez la touche de raccourci : Ctrl+D

Pour connaître les comptes d’utilisateur qui ont été créés sur votre installation PostgreSQL, utilisez à partir de votre terminal WSL : `psql -c "\du"`... ou simplement `\du` si l’interpréteur de commandes psql est ouvert. Cette commande affiche les colonnes suivantes : Nom d’utilisateur du compte, liste des attributs des rôles et membre du ou des groupes de rôles. Pour revenir à la ligne de commande, entrez : `q`.
