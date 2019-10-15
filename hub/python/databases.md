---
title: Prise en main de Python sur Windows avec une base de données
description: Guide pour vous aider à commencer à utiliser PostgreSQL ou MongoDB avec Python sur Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Python, Windows 10, PostgreSQL, MongoDB, Postgres, Mongo, Microsoft, Python sur Windows, installer PostgreSQL sur Windows, installer MongoDB sur Windows, utiliser PostgreSQL avec Python, utiliser MongoDB avec Python, PostgreSQL sur WSL, MongoDB sur WSL
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 42a257361cffec974d060a6518dfdf5254d62082
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314853"
---
# <a name="get-started-using-databases-with-python-on-windows"></a>Prise en main des bases de données avec Python sur Windows

Les applications python doivent souvent conserver les données, ce qui peut se produire par le biais de fichiers, de stockage local, de services Cloud ou de bases de données. Ce guide pas à pas vous aidera à vous familiariser avec la connexion de votre application Python à une base de données. Nous avons choisi de nous concentrer sur deux options populaires : PostgreSQL et MongoDB.

## <a name="differences-between-mongodb-and-postgresql"></a>Différences entre MongoDB et PostgreSQL

[!INCLUDE [Postgres vs Mongo](../includes/postgres-v-mongo.md)]

> [!NOTE]
> Vous pouvez également prendre en compte l’intégration de l’infrastructure et des outils que vous utilisez avec un système de base de données particulier. L' [infrastructure Web Django](./web-frameworks.md#hello-world-tutorial-for-django) semble être mieux intégrée à PostgreSQL (consultez les [documents Django](https://docs.djangoproject.com/en/2.2/ref/contrib/postgres/) et [psycopg2](https://github.com/psycopg/psycopg2)). L' [infrastructure Web](./web-frameworks.md#hello-world-tutorial-for-flask) de la fiole semble être mieux intégrée à MongoDB (voir [MongoEngine](https://github.com/MongoEngine/flask-mongoengine) et [PyMongo](https://github.com/dcrosta/flask-pymongo)).

## <a name="install-postgresql"></a>Installer PostgreSQL

[!INCLUDE [Install and run PostgresQL](../includes/install-and-run-postgres.md)]

### <a name="vs-code-support-for-postgresql"></a>Prise en charge de VS Code pour PostgreSQL

VS Code prend en charge l’utilisation des bases de données PostgreSQL à l’aide de l' [extension PostgreSQL](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql), vous pouvez créer, vous connecter à, gérer et interroger des bases de données PostgreSQL à partir de vs code.

## <a name="install-mongodb"></a>Installer MongoDB

[!INCLUDE [Install and run Mongo](../includes/install-and-run-mongo.md)]

### <a name="vs-code-support-for-mongodb"></a>Prise en charge de VS Code pour MongoDB

VS Code prend en charge l’utilisation des bases de données MongoDB à l’aide de l' [extension Azure CosmosDB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb), vous pouvez créer, vous connecter à, gérer et interroger des bases de données MongoDB à partir d’vs code.

Pour plus d’informations, consultez les documents VS Code : [Utilisation de MongoDB](https://code.visualstudio.com/docs/azure/mongodb).

## <a name="set-up-profile-aliases"></a>Configurer des alias de profil

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]
