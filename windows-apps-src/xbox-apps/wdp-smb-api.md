---
title: Informations de référence sur les API SMB Device Portal
description: Découvrez comment utiliser l’API REST du portail d’appareils Xbox/ext/SMB/developerfolder pour accéder au dossier développeur sur votre console Xbox One via l’Explorateur de fichiers.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 1f0eb76e-fe3e-4674-a27e-229beec7e63d
ms.localizationpriority: medium
ms.openlocfilehash: 80a49d324c27754a2686ba4d954b47e7529df330
ms.sourcegitcommit: eb725a47c700131f5975d737bd9d8a809e04943b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88970207"
---
# <a name="developer-folder-api-reference"></a>Informations de référence sur les API du dossier de développement

Vous pouvez accéder aux fichiers de développement sur votre Xbox One à l’aide d’un explorateur de fichiers standard. Cela vous permet d’afficher et de remplacer facilement des fichiers sur la console, à partir de votre PC.

**Requête**

Vous pouvez accéder au dossier de développement en utilisant la requête suivante. La requête renvoie :

* L’emplacement du partage de fichiers. Cet emplacement peut être saisi dans la barre d’adresses d’un explorateur de fichiers.
* Le nom d’utilisateur pour accéder au partage de fichiers.
* Le mot de passe pour accéder au partage de fichiers.

Méthode      | URI de demande
:------     | :-----
GET | /ext/smb/developerfolder

**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la demande**

- Aucun

**Lutte**   
Path : le chemin d’accès au partage des fichiers de développement.   
Username : le nom d’utilisateur requis pour accéder au partage des fichiers de développement.   
Password : le mot de passe requis pour accéder au partage des fichiers de développement.   

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d'état HTTP      | Description
:------     | :-----
200 | La requête pour accéder aux informations d’identification pour le partage de fichiers a été accordée.
4XX | Codes d’erreur
5XX | Codes d’erreur

**Familles d’appareils disponibles**

* Windows Xbox
