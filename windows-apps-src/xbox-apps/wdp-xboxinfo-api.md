---
title: Informations de référence sur l’API Xbox info du portail d’appareil
description: Découvrez comment accéder aux informations d’un appareil Xbox One à l’aide de la méthode obtenir de l’API REST du portail d’appareils Xbox.
ms.date: 04/18/2019
ms.topic: article
keywords: Windows 10, UWP, Xbox, portail des appareils
ms.localizationpriority: medium
ms.openlocfilehash: c2a4cecaf3340818b3679dfd3f64b9759ea46752
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174653"
---
# <a name="xbox-info-api-reference"></a>Informations de référence sur l’API Xbox info   
Vous pouvez accéder aux informations de l’appareil Xbox One à l’aide de cette API.

## <a name="get-xbox-one-device-information"></a>Procurez-vous des informations sur l’appareil Xbox One

## <a name="request"></a>Requête

Vous pouvez obtenir des informations sur l’appareil sur votre Xbox.

Méthode      | URI de demande
:------     | :-----
GET | /ext/xbox/info

**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la demande**

- Aucune

## <a name="response"></a>response
Objet JSON avec les champs suivants :

* OsVersion-(chaîne) version du système d’exploitation.
* OsEdition-(chaîne) édition du système d’exploitation, telle que « mars 2017 » ou « mars 2017 QFE 1 ».
* ConsoleId-(chaîne) ID de la console.
* DeviceId-(chaîne) ID d’appareil Xbox Live de la console.
* SerialNumber-(String) Numéro de série de la console.
* DevMode-(chaîne) le mode développeur actuel de la console, tel que « aucun » ou « vente au détail ».
* ConsoleType-(chaîne) le type de la console, par exemple « Xbox One » ou « Xbox One S ».
* DevkitCertificateExpirationTime-(numéro) heure UTC en secondes de l’expiration du certificat du kit de développement de la console.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d'état HTTP      | Description
:------     | :-----
200 | Demande réussie
4XX | Codes d’erreur
5XX | Codes d’erreur

**Familles d’appareils disponibles**

* Windows Xbox
