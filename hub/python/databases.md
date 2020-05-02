---
title: Prise en main de Python sur Windows avec une base de données
description: Guide qui vous permettra de commencer à utiliser PostgreSQL ou MongoDB avec Python sur Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, postgresql, mongodb, postgres, mongo, microsoft, python sur windows, installer postgresql sur windows, installer mongodb sur windows, utilisation de postgresql avec python, utilisation de mongodb avec python, postgresql sur WSL, mongodb sur WSL
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 9b1bdea86739f3d58b39cf7f0e6b8090474886f3
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "72517780"
---
# <a name="get-started-using-postgresql-or-mongodb-with-python-on-windows"></a>Prise en main de PostgreSQL ou MongoDB avec Python sur Windows

Ce guide pas à pas vous aidera à vous familiariser avec la connexion de votre application Python à une base de données. Nous avons choisi de nous concentrer sur deux options populaires : PostgreSQL et MongoDB.

## <a name="differences-between-mongodb-and-postgresql"></a>Différences entre MongoDB et PostgreSQL

[!INCLUDE [Postgres vs Mongo](../includes/postgres-v-mongo.md)]

> [!NOTE]
> Vous pouvez également prendre en compte l’intégration de l’infrastructure et des outils que vous utilisez dans un système de base de données particulier. [L’infrastructure web Django](./web-frameworks.md#hello-world-tutorial-for-django) semble être mieux intégrée dans PostgreSQL (consultez la [documentation sur Django](https://docs.djangoproject.com/en/2.2/ref/contrib/postgres/) et [psycopg2](https://github.com/psycopg/psycopg2)). [L’infrastructure web Flask](./web-frameworks.md#hello-world-tutorial-for-flask) semble être mieux intégrée dans MongoDB (voir [MongoEngine](https://github.com/MongoEngine/flask-mongoengine) et [PyMongo](https://github.com/dcrosta/flask-pymongo)).

## <a name="install-postgresql"></a>Installer PostgreSQL

[!INCLUDE [Install and run PostgresQL](../includes/install-and-run-postgres.md)]

### <a name="vs-code-support-for-postgresql"></a>Prise en charge de VS Code pour PostgreSQL

VS Code prend en charge l’utilisation de bases de données PostgreSQL par le biais de [l’extension PostgreSQL](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql), vous pouvez créer et vous connecter aux bases de données PostgreSQL, mais aussi les gérer et les interroger à partir de VS Code.

## <a name="install-mongodb"></a>Installer MongoDB

[!INCLUDE [Install and run Mongo](../includes/install-and-run-mongo.md)]

### <a name="vs-code-support-for-mongodb"></a>Prise en charge de VS Code pour MongoDB

VS Code prend en charge l’utilisation de bases de données PostgreSQL par le biais de [l’extension Azure Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb), vous pouvez créer et vous connecter aux bases de données MongoDB, mais aussi les gérer et les interroger à partir de VS Code.

Pour en savoir plus, consultez le document VS Code suivant : [Utilisation de MongoDB](https://code.visualstudio.com/docs/azure/mongodb).

## <a name="set-up-profile-aliases"></a>Configurer des alias de profil

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]
