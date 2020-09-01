---
title: Informations de référence sur les API Fiddler Device Portal
description: Découvrez comment activer et désactiver le traçage réseau Fiddler sur vos DevKit à l’aide de l’API REST du portail d’appareils Xbox.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e7d4225e-ac2c-41dc-aca7-9b1a95ec590b
ms.localizationpriority: medium
ms.openlocfilehash: f431adae41021432dfcfca6b4e79df5237fcb283
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163993"
---
# <a name="fiddler-settings-api-reference"></a>Informations de référence sur les API des paramètres Fiddler   
Vous pouvez activer et désactiver le suivi réseau de Fiddler sur votre kit de développement à l’aide de cette API REST.

## <a name="determine-if-fiddler-tracing-is-enabled"></a>Déterminer si le suivi Fiddler est activé

**Requête**

Vous pouvez vérifier si le suivi Fiddler est activé sur l’appareil à l’aide de la requête suivante.

Méthode      | URI de demande
:------     | :-----
GET | /ext/fiddler


**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la demande**   

- Aucune

**Réponse**   

- Propriété JSON bool IsProxyEnabled qui spécifie si le proxy est activé ou non.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d'état HTTP      | Description
:------     | :-----
200 | Succès
4XX | Codes d’erreur
5XX | Codes d’erreur

## <a name="enable-fiddler-tracing"></a>Activer le suivi de Fiddler

**Requête**

Vous pouvez activer le suivi de Fiddler pour le kit de développement à l’aide de la demande suivante.  Notez que l’appareil doit être redémarré avant que cela ne prenne effet.

Méthode      | URI de demande
:------     | :-----
POST | /ext/fiddler

**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI      | Description     | 
| ------------------ |-----------------|
| proxyAddress       | L’adresse IP ou le nom d’hôte de l’appareil exécutant Fiddler |
| proxyPort          | Le port que Fiddler utilise pour la surveillance du trafic. Par défaut : 8888 |
| updateCert (facultatif)| Une valeur booléenne indiquant si le certificat Fiddler racine est fourni. Cette valeur doit être true si Fiddler n’a jamais été configuré sur ce kit de développement ou a été configuré pour un autre hôte.  |


**En-têtes de requête**

- Aucune

**Corps de la demande**

- Aucun si updateCert est false ou n’est pas fourni. Corps HTTP à parties multiples conforme contenant le fichier FiddlerRoot.cer dans le cas contraire.

**Réponse**   

- Aucune  

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d'état HTTP      | Description
:------     | :-----
204 | La demande d’activation de Fiddler a été acceptée. Fiddler va être activé au prochain redémarrage de l’appareil.
4XX | Codes d’erreur
5XX | Codes d’erreur

## <a name="disable-fiddler-tracing-on-the-devkit"></a>Désactiver le suivi de Fiddler sur le kit de développement

**Requête**

Vous pouvez désactiver le suivi de Fiddler sur l’appareil à l’aide de la demande suivante. Notez que l’appareil doit être redémarré avant que cela ne prenne effet.

Méthode      | URI de demande
:------     | :-----
Suppression | /ext/fiddler

**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la demande**   

- Aucune

**Réponse**   

- Aucune 

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d'état HTTP      | Description
:------     | :-----
204 | La demande de désactivation du suivi de Fiddler a réussi. Le suivi va être désactivé au prochain redémarrage de l’appareil.
4XX | Codes d’erreur
5XX | Codes d’erreur


**Familles d’appareils disponibles**

* Windows Xbox

## <a name="see-also"></a>Voir aussi
- [Configuration de Fiddler pour UWP sur Xbox](uwp-fiddler.md)

