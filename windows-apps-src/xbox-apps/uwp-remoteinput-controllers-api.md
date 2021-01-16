---
title: Informations de référence sur l’API contrôleurs du portail des appareils
description: Découvrez comment obtenir le nombre de contrôleurs physiques attachés et les désactiver par programmation.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 535846d7dbeb2d29328b5c5d01b06d4449a53790
ms.sourcegitcommit: b0a82c2a132212eb5fb72b67f0789cac1014642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254184"
---
# <a name="controller-api-reference"></a>Référence de l’API du contrôleur

Vous pouvez récupérer le nombre de contrôleurs physiques attachés et les désactiver à l’aide de cette API REST.

## <a name="determine-the-number-of-attached-physical-controllers"></a>Déterminer le nombre de contrôleurs physiques attachés

**Requête**

Vous pouvez vérifier le nombre de contrôleurs physiques attachés sur l’appareil à l’aide de la requête suivante.

Méthode | URI de demande |
-------|-------------|
| GET | /ext/remoteinput/controllers |

**Paramètres d’URI**

- None

**En-têtes de requête**

- None

**Corps de la demande**   

- None

**Réponse**   

- Propriété de nombre JSON ConnectedControllerCount qui spécifie le nombre de contrôleurs physiques attachés.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d'état HTTP | Description |
|------------------|-------------|
| 200 | Succès |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

## <a name="disconnect-all-physical-controllers-on-the-devkit"></a>Déconnectez tous les contrôleurs physiques sur le DevKit

**Requête**

Vous pouvez déconnecter tous les contrôleurs physiques sur l’appareil à l’aide de la requête suivante.

| Méthode | URI de demande |
|--------|-------------|
| Suppression | /ext/remoteinput/controllers |

**Paramètres d’URI**

- None

**En-têtes de requête**

- None

**Corps de la demande**   

- None

**Réponse**   

- None 

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d'état HTTP | Description |
|------------------|-------------|
| 204 | La demande de déconnexion des contrôleurs s’est correctement déroulée. |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Xbox
