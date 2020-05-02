---
title: Bien démarrer avec la connexion des applications Node.js à une base de données
description: Découvrez comment connecter des applications Node.js à une base de données sur Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, Windows 10, Microsoft, découvrir NodeJS, Node sur Windows, Node sur WSL, Node sur Linux sur Windows, installer Node sur Windows, NodeJS avec VS Code, développer avec Node sur Windows, développer avec NodeJS sur Windows, installer Node sur WSL, NodeJS sur le Sous-système Windows pour Linux
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 63c47107538d8744201f83ea1be24cfaf3193f4f
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "72517819"
---
# <a name="get-started-using-mongodb-or-postgresql-with-nodejs-on-windows"></a>Bien démarrer avec MongoDB ou PostgreSQL à l’aide de Node.js sur Windows

Les applications Node.js doivent souvent conserver des données, ce qui peut se produire par le biais de fichiers, d’un stockage local, de services cloud ou de bases de données. Ce guide pas à pas vous permet de bien démarrer pour connecter votre application Node.js à une base de données. Nous avons choisi de nous concentrer sur deux options populaires : MongoDB et PostgreSQL.

## <a name="prerequisites"></a>Prérequis

Ce guide part du principe que vous avez déjà suivi les étapes de [configuration de votre environnement de développement Node.js avec WSL 2](./setup-on-wsl2.md), y compris les étapes suivantes :

- Installer Windows 10 Insider Preview, build 18932 ou ultérieure.
- Activez la fonctionnalité WSL 2 sur Windows.
- Installer une distribution Linux (Ubuntu 18.04 dans les exemples). Pour le vérifier, exécutez : `wsl lsb_release -a`.
- Vérifier que votre distribution Ubuntu 18.04 s’exécute en mode WSL 2. (WSL peut exécuter les distributions en mode v1 ou v2.) Pour le vérifier, ouvrez PowerShell et entrez : `wsl -l -v`.
- À l’aide de PowerShell, définissez Ubuntu 18.04 comme distribution par défaut, avec : `wsl -s ubuntu 18.04`.

## <a name="differences-between-mongodb-and-postgresql"></a>Différences entre MongoDB et PostgreSQL

[!INCLUDE [Postgres vs Mongo](../includes/postgres-v-mongo.md)]

## <a name="install-mongodb"></a>Installer MongoDB

[!INCLUDE [Install and run Mongo](../includes/install-and-run-mongo.md)]

### <a name="vs-code-support-for-mongodb"></a>Prise en charge de VS Code pour MongoDB

VS Code prend en charge l’utilisation de bases de données MongoDB par le biais de [l’extension Azure Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb). Il est ainsi possible de créer des bases de données MongoDB, de les gérer et de les interroger dans VS Code.

Pour en savoir plus, consultez le document VS Code suivant : [Utilisation de MongoDB](https://code.visualstudio.com/docs/azure/mongodb).

Pour plus d’informations, consultez la documentation de MongoDB :

- [Présentation de l’utilisation de MongoDB](https://docs.mongodb.com/manual/introduction/)
- [Création d’utilisateurs](https://docs.mongodb.com/manual/tutorial/create-users/)
- [Connexion à une instance MongoDB sur un hôte distant](https://docs.mongodb.com/manual/mongo/#mongodb-instance-on-a-remote-host)
- [CRUD : créer, lire, mettre à jour, supprimer](https://docs.mongodb.com/manual/crud/)
- [Documents de référence](https://docs.mongodb.com/manual/reference/)

## <a name="install-postgresql"></a>Installer PostgreSQL

[!INCLUDE [Install and run PostgresQL](../includes/install-and-run-postgres.md)]

### <a name="vs-code-support-for-postgresql"></a>Prise en charge de VS Code pour PostgreSQL

VS Code prend en charge l’utilisation de bases de données PostgreSQL par le biais de [l’extension PostgreSQL](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql), vous pouvez créer et vous connecter aux bases de données PostgreSQL, mais aussi les gérer et les interroger à partir de VS Code.

## <a name="set-up-profile-aliases"></a>Configurer des alias de profil

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]
