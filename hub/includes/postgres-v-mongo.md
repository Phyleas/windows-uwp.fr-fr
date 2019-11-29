---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: 0e144789531e43e3561e7b82830c2b78249c294b
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314873"
---
Les deux [systèmes de base de données les plus populaires](https://insights.stackoverflow.com/survey/2019#technology-_-databases) sont [MongoDB](https://www.mongodb.com/what-is-mongodb) et [PostgreSQL](https://www.postgresql.org/about/). 

MongoDB est une base de données de documents NoSQL conçue pour fonctionner avec JSON et stocker des données sans schéma. C’est un système flexible adapté pour les données non structurées, la mise en cache de l’analytique en temps réel et la montée en charge horizontale. 

PostgreSQL (parfois appelé Postgres) est une base de données relationnelle SQL qui met l’accent sur l’extensibilité et la conformité aux normes. Il peut désormais gérer JSON, mais il est généralement recommandé pour les données structurées, la montée en charge verticale et les besoins ACID, tels que le commerce électronique et les transactions financières.

Schemas :

**PostgreSQL :** Table | Colonne | Valeur | Enregistrements.

**MongoDB (NoSQL) :** Collection | Clé | Valeur | Document.

Le tri de la base de données que vous choisissez doit dépendre du type d’application avec lequel vous allez utiliser la base de données. Nous vous recommandons de rechercher les avantages et les inconvénients des bases de données structurées et non structurées et de choisir en fonction de votre cas d’utilisation. Il existe également plusieurs autres systèmes de base de données à prendre en compte au-delà de PostgreSQL et MongoDB.