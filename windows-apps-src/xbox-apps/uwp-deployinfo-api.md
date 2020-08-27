---
title: Informations de référence sur l’API de déploiement du portail de périphérique
description: Découvrez comment utiliser l’API REST du portail d’appareils Xbox DeployInfo pour demander des informations de déploiement pour un ou plusieurs packages installés.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 5260125625ced6c258a683bcfb9b552e57d07f06
ms.sourcegitcommit: 8e0e4cac79554e86dc7f035c4b32cb1f229142b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88942999"
---
# <a name="requests-deployment-information-for-one-or-more-installed-packages"></a>Demande des informations de déploiement pour un ou plusieurs packages installés.

**Requête**

Méthode      | URI de demande
:------     | :------
POST | /ext/app/deployinfo
<br />
**Paramètres d’URI**

 - Aucun

**En-têtes de requête**

- Aucun

**Corps de la demande**

Tableau JSON au format suivant :

* DeployInfo
  * PackageFullName : nom du package sur lequel nous demandons des informations.
  * OverlayFolder : chemin d’accès facultatif à un chemin d’accès de dossier de superposition si vous utilisez cette fonctionnalité.

###<a name="response"></a>response

**Corps de la réponse**

Tableau JSON au format suivant (certains champs sont facultatifs) :

* DeployInfo
  * PackageFullName : nom du package sur lequel nous recevons des informations.
  * DeployType : type de déploiement.
  * DeployPathOrSpecifiers : chemin de déploiement pour les déploiements libres ou les spécificateurs installés pour les déploiements empaquetés.
  * DeployDrive : le lecteur sur lequel le package est déployé pour les types de déploiement applicables.
  * DeploySizeInBytes : taille en octets du package pour les types de déploiement applicables.
  * OverlayFolder : dossier de superposition pour les déploiements qui prennent en charge cette fonctionnalité.

**Code d'état**

Cette API comporte les codes d’état attendus suivants.

Code d'état HTTP      | Description
:------     | :-----
200 | Succès
4XX | Codes d’erreur
5XX | Codes d’erreur
<br />

**Familles d’appareils disponibles**

* Windows Xbox