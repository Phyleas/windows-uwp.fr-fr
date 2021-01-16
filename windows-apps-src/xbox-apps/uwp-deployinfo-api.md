---
title: Informations de référence sur l’API de déploiement du portail de périphérique
description: Découvrez comment utiliser l’API REST du portail d’appareils Xbox DeployInfo pour demander des informations de déploiement pour un ou plusieurs packages installés.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 2819b21e12d25aca941808e1feeb8a7539750a91
ms.sourcegitcommit: b0a82c2a132212eb5fb72b67f0789cac1014642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254224"
---
# <a name="requests-deployment-information-for-one-or-more-installed-packages"></a>Demande des informations de déploiement pour un ou plusieurs packages installés.

**Requête**

| Méthode | URI de demande |
|--------|-------------|
| POST | /ext/app/deployinfo |

**Paramètres d’URI**

 - None

**En-têtes de requête**

- None

**Corps de la demande**

Tableau JSON au format suivant :

* DeployInfo
  * PackageFullName : nom du package sur lequel nous demandons des informations.
  * OverlayFolder : chemin d’accès facultatif à un chemin d’accès de dossier de superposition si vous utilisez cette fonctionnalité.

### <a name="response"></a>response

**Response body**

Tableau JSON au format suivant (certains champs sont facultatifs) :

* DeployInfo
  * PackageFullName : nom du package sur lequel nous recevons des informations.
  * DeployType : type de déploiement.
  * DeployPathOrSpecifiers : chemin de déploiement pour les déploiements libres ou les spécificateurs installés pour les déploiements empaquetés.
  * DeployDrive : le lecteur sur lequel le package est déployé pour les types de déploiement applicables.
  * DeploySizeInBytes : taille en octets du package pour les types de déploiement applicables.
  * OverlayFolder : dossier de superposition pour les déploiements qui prennent en charge cette fonctionnalité.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d'état HTTP | Description |
|------------------|-------------|
| 200 | Succès |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Xbox