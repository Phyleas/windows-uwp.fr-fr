---
title: Prise en main de la connexion des applications node. js à une base de données
description: Commencez à connecter des applications node. js à une base de données sur Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, node. js, Windows 10, Microsoft, formation NodeJS, node sur Windows, node sur WSL, node sur Linux sur Windows, installer le nœud sur Windows, NodeJS avec vs code, développer avec un nœud sur Windows, développer avec NodeJS sur Windows, installer le nœud sur WSL, NodeJS sur Windows Sous-système pour Linux
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: bdc3e3c944c4aeb25f5cf880fc4d31df1019da5a
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72315113"
---
# <a name="get-started-connecting-nodejs-apps-to-a-database"></a>Prise en main de la connexion des applications node. js à une base de données

Les applications node. js doivent souvent conserver les données, ce qui peut se produire par le biais de fichiers, d’un stockage local, de services Cloud ou de bases de données. Ce guide pas à pas vous permet de commencer à connecter votre application node. js à une base de données. Nous avons choisi de nous concentrer sur deux options populaires : MongoDB et PostgreSQL.

## <a name="prerequisites"></a>Prérequis

Ce guide part du principe que vous avez déjà effectué les étapes pour [configurer votre environnement de développement node. js avec WSL 2](./setup-on-wsl2.md), notamment :

- Installez Windows 10 Insider preview version 18932 ou ultérieure.
- Activez la fonctionnalité WSL 2 sur Windows.
- Installez une distribution Linux (Ubuntu 18,04 pour nos exemples). Vous pouvez le vérifier avec : `wsl lsb_release -a`
- Assurez-vous que votre distribution Ubuntu 18,04 s’exécute en mode WSL 2. (WSL peut exécuter des distributions en mode v1 ou v2.) Vous pouvez le vérifier en ouvrant PowerShell et en entrant : `wsl -l -v`
- À l’aide de PowerShell, définissez Ubuntu 18,04 comme distribution par défaut, avec : `wsl -s ubuntu 18.04`

## <a name="differences-between-mongodb-and-postgresql"></a>Différences entre MongoDB et PostgreSQL

[!INCLUDE [Postgres vs Mongo](../includes/postgres-v-mongo.md)]

## <a name="install-mongodb"></a>Installer MongoDB

[!INCLUDE [Install and run Mongo](../includes/install-and-run-mongo.md)]

### <a name="vs-code-support-for-mongodb"></a>Prise en charge de VS Code pour MongoDB

VS Code prend en charge l’utilisation des bases de données MongoDB à l’aide de l' [extension Azure CosmosDB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb), vous pouvez créer, gérer et interroger des bases de données MongoDB à partir d’vs code.

Pour plus d’informations, consultez les documents VS Code : [Utilisation de MongoDB](https://code.visualstudio.com/docs/azure/mongodb).

Pour en savoir plus, consultez la documentation MongoDB :

- [Présentation de l’utilisation de MongoDB](https://docs.mongodb.com/manual/introduction/)
- [Créer des utilisateurs](https://docs.mongodb.com/manual/tutorial/create-users/)
- [Se connecter à une instance MongoDB sur un hôte distant](https://docs.mongodb.com/manual/mongo/#mongodb-instance-on-a-remote-host)
- @NO__T 0CRUD : Créer, lire, mettre à jour, supprimer @ no__t-0
- [Documentation de référence](https://docs.mongodb.com/manual/reference/)

## <a name="install-postgresql"></a>Installer PostgreSQL

[!INCLUDE [Install and run PostgresQL](../includes/install-and-run-postgres.md)]

### <a name="vs-code-support-for-postgresql"></a>Prise en charge de VS Code pour PostgreSQL

VS Code prend en charge l’utilisation des bases de données PostgreSQL à l’aide de l' [extension PostgreSQL](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql), vous pouvez créer, vous connecter à, gérer et interroger des bases de données PostgreSQL à partir de vs code.

## <a name="set-up-profile-aliases"></a>Configurer des alias de profil

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]
