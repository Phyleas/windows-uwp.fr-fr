---
title: Informations de référence sur l’API des pin SSH du portail d’appareil
description: Découvrez comment supprimer tous les codes confidentiels de Secure Shell sécurisés (SSH) par programmation à l’aide de l’API REST du portail d’appareils Xbox/ext/App/sshpins.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 307af4cdd4e998832f4a2fe7a8f874615fe10ad7
ms.sourcegitcommit: cb5af00af05e838621c270173e7fde1c5d2168ef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89043531"
---
# <a name="ssh-pins-api-reference"></a>Informations de référence sur l’API pin SSH
Vous pouvez supprimer tous les codes confidentiels SSH de confiance sur vos DevKit à l’aide de cette API REST.

## <a name="remove-trusted-ssh-pins"></a>Supprimer les codes confidentiels SSH approuvés

**Requête**

Méthode      | URI de demande
:------     | :-----
Suppression | /ext/app/sshpins
<br />
**Paramètres d’URI**

- Aucun

**En-têtes de requête**

- Aucun

**Corps de la demande**   

- Aucun

**Réponse**   

- Aucun 

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d'état HTTP      | Description
:------     | :-----
204 | La demande d’effacement des codes confidentiels s’est déroulée correctement.
4XX | Codes d’erreur
5XX | Codes d’erreur

<br />
**Familles d’appareils disponibles**

* Windows Xbox

