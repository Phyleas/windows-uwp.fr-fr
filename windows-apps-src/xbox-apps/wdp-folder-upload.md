---
title: Infos de référence sur les API de chargement du dossier Device Portal
description: Découvrez comment vous pouvez télécharger un dossier dans le répertoire de développement à l’aide de l’API REST du portail d’appareils Xbox.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e1a2c7f0-0040-4ce7-94de-17224736e20b
ms.localizationpriority: medium
ms.openlocfilehash: b71f60350bf5c8318adb2a4741bb1a275a4b0276
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157753"
---
# <a name="upload-a-folder-to-the-development-directory"></a>Télécharger un dossier dans le répertoire de développement

**Requête**

Vous pouvez charger un dossier complet en une fois vers l’ID de dossier connu pour les fichiers de développement (ou dans un sous-dossier de ce dossier).

Méthode      | URI de demande
:------     | :------
POST | /api/app/packagemanager/upload 

**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

Paramètre d’URI      | Description
:------     | :-----
destinationFolder (obligatoire) | Le nom du dossier de destination du dossier à charger. Ce dossier sera placé sous d:\developmentfiles\LooseApps sur la console. Ce nom de dossier doit être codé en base 64, dans la mesure où il peut contenir des séparateurs de chemin d’accès si le dossier est un sous-dossier de LooseApps.


**En-têtes de requête**

- Aucune

**Corps de la demande**

- Corps HTTP à parties multiples conforme du contenu du répertoire.

**Réponse**

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d'état HTTP      | Description
:------     | :-----
200 | Succès
4XX | Codes d’erreur
5XX | Codes d’erreur

**Familles d’appareils disponibles**

* Windows Xbox

