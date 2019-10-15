---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: 0e144789531e43e3561e7b82830c2b78249c294b
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314873"
---
Les deux choix les plus [populaires](https://insights.stackoverflow.com/survey/2019#technology-_-databases) pour un système de base de données sont [MongoDB](https://www.mongodb.com/what-is-mongodb) et [PostgreSQL](https://www.postgresql.org/about/). 

MongoDB est une base de données de documents NoSQL conçue pour fonctionner avec JSON et stocker des données sans schéma. C’est parfait pour la flexibilité et les données non structurées, la mise en cache des analyses en temps réel et la mise à l’échelle horizontale. 

PostgreSQL (parfois appelé Postgres) est une base de données relationnelle SQL qui met l’accent sur l’extensibilité et la conformité aux normes. Il peut désormais gérer JSON, mais il est généralement préférable pour les données structurées, la mise à l’échelle verticale et les besoins en matière d’ACID tels que le commerce électronique et les transactions financières.

Schémas

**PostgreSQL** Table | Colonne | Valeur | Documents.

**MongoDB (NoSQL) :** Collection | Clé | Valeur | Document.

Le tri de la base de données que vous choisissez doit dépendre du type d’application avec lequel vous allez utiliser la base de données. Nous vous recommandons de rechercher les avantages et les inconvénients des bases de données structurées et non structurées et de choisir en fonction de votre cas d’utilisation. Il existe également plusieurs autres systèmes de base de données à prendre en compte au-delà de PostgreSQL et MongoDB.