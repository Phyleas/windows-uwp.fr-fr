---
title: Informations de référence sur l’API moniteur HTTP du portail de l’appareil
description: Découvrez comment accéder au trafic HTTP en temps réel à partir de l’application ciblée sur une Xbox à l’aide de l’API REST du portail d’appareils/ext/httpmonitor/sessions Xbox.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 6ea53f6356aa89a83f3b267a65f65b32aad5749d
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304531"
---
# <a name="http-monitor-api-reference"></a>Informations de référence sur l’API du moniteur HTTP   
Vous pouvez accéder au trafic HTTP en temps réel pour l’application ciblée à l’aide de cette API si le moniteur HTTP a été activé sur la console Xbox en activant la case à cocher dans dev orig.

## <a name="get-if-the-http-monitor-is-enabled"></a>Obtient si le moniteur HTTP est activé

**Requête**

Vous pouvez savoir si le moniteur HTTP a été activé dans la page d’hébergement dev.

Méthode      | URI de demande
:------     | :-----
GET | /ext/httpmonitor/sessions

**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la demande**

- Aucun

**Lutte**   
Objet JSON avec les champs suivants :

* Activé : (bool) indique si le moniteur HTTP a été activé sur la console Xbox en activant la case à cocher dans la page d’hébergement dev.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d'état HTTP      | Description
:------     | :-----
200 | Demande réussie
4XX | Codes d’erreur
5XX | Codes d’erreur

## <a name="get-http-traffic-from-the-focused-app"></a>Recevoir le trafic HTTP à partir de l’application ciblée

**Requête**

Obtenir le trafic HTTP à partir de l’application ciblée sur la Xbox, à condition qu’il ne s’agit pas d’une application système, en temps réel, si le moniteur HTTP a été activé à partir de la page d’hébergement dev.

Méthode      | URI de demande
:------     | :-----
WebSocket | /ext/httpmonitor/sessions

**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la demande**

- Aucun

**Lutte**   
Objet JSON avec les champs suivants :

* Sessions
    * RequestHeaders-(objet JSON) les en-têtes de requête de la requête HTTP.
    * RequestContentHeaders-(objet JSON) les en-têtes de contenu de la demande de la requête HTTP.
    * RequestURL-(chaîne) URL de la requête.
    * RequestMethod-(chaîne) méthode de demande.
    * RequestMessage : (chaîne) le message de demande, qui prend uniquement en charge JSON et le contenu texte.
    * ResponseHeaders-(objet JSON) les en-têtes de réponse de la réponse HTTP.
    * ResponseContentHeaders-(objet JSON) les en-têtes de contenu de la réponse de la réponse HTTP.
    * StatusCode-(Number) code d’état de la réponse.
    * ReasponsePhrase-(chaîne) expression de motif de la réponse.
    * ResponseMessage : (chaîne) message de réponse, qui prend uniquement en charge le contenu JSON et texte.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d'état HTTP      | Description
:------     | :-----
200 | Demande réussie
4XX | Codes d’erreur
403 | Moniteur HTTP désactivé, doit être activé dans la page d’hébergement dev
5XX | Codes d’erreur


**Familles d’appareils disponibles**

* Windows Xbox