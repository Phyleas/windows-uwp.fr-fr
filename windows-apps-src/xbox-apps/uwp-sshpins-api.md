---
title: Informations de référence sur l’API des pin SSH du portail d’appareil
description: Découvrez comment supprimer tous les codes confidentiels de Secure Shell sécurisés (SSH) par programmation à l’aide de l’API REST du portail d’appareils Xbox/ext/App/sshpins.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 999a77051b0eebe6ae5d8be9e4c2640709431b6d
ms.sourcegitcommit: b0a82c2a132212eb5fb72b67f0789cac1014642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254204"
---
# <a name="ssh-pins-api-reference"></a>Informations de référence sur l’API pin SSH

Vous pouvez supprimer tous les codes confidentiels SSH de confiance sur vos DevKit à l’aide de cette API REST.

## <a name="remove-trusted-ssh-pins"></a>Supprimer les codes confidentiels SSH approuvés

**Requête**

| Méthode | URI de demande |
|--------|-------------|
| Suppression | /ext/app/sshpins |

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
| 204 | La demande d’effacement des codes confidentiels s’est déroulée correctement. |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Xbox
