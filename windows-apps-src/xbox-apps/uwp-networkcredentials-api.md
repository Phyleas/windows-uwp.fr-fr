---
title: Informations de référence sur l’API Network Portal
description: Découvrez comment ajouter, supprimer ou mettre à jour les informations d’identification réseau par programmation.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 7ba9e25031590da02881276ff9a10a9f952c78ec
ms.sourcegitcommit: b0a82c2a132212eb5fb72b67f0789cac1014642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254214"
---
# <a name="network-credentials-api-reference"></a>Informations de référence sur l’API informations d’identification réseau

Vous pouvez ajouter, supprimer ou mettre à jour les informations d’identification réseau stockées sur votre DevKit à l’aide de cette API REST.

## <a name="get-existing-credentials"></a>Récupérer les informations d’identification existantes

**Requête**

Vous pouvez obtenir la liste des partages stockés ainsi que le nom d’utilisateur de l’utilisateur qui dispose des informations d’identification de ce partage réseau.

| Méthode | URI de demande |
|--------|-------------|
| GET | /ext/networkcredential |

**Paramètres d’URI**

- None

**En-têtes de requête**

- None

**Corps de la demande**   

- None

**Réponse**   

Tableau JSON au format suivant :

* Informations d'identification
  * NetworkPath : chemin d’accès au partage réseau.
  * Nom d’utilisateur : nom d’utilisateur qui contient les informations d’identification stockées.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d'état HTTP | Description |
|------------------|-------------|
| 200 | Succès |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

## <a name="add-or-update-stored-credentials-for-a-user"></a>Ajouter ou mettre à jour les informations d’identification stockées pour un utilisateur

**Requête**

| Méthode | URI de demande |
|--------|-------------|
| POST | /ext/networkcredential |

**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI      | Description     |
| ------------------ |-----------------|
| NetworkPath        | Chemin d’accès réseau au partage auquel vous ajoutez des informations d’identification. |

**En-têtes de requête**

- None

**Corps de la demande**

Éléments JSON suivants :
* NetworkPath : chemin d’accès au partage réseau.
* Nom d’utilisateur : nom d’utilisateur dans lequel stocker les informations d’identification.
* Mot de passe : mot de passe nouveau ou mis à jour pour cet utilisateur.

**Réponse**   

- None  

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d'état HTTP | Description |
|------------------|-------------|
| 204 | Succès |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

## <a name="remove-stored-credentials-for-a-share"></a>Supprimer les informations d’identification stockées pour un partage.

**Requête**

| Méthode | URI de demande |
|--------|-------------|
| Suppression | /ext/networkcredential |

**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI      | Description     |
| ------------------ |-----------------|
| NetworkPath        | Chemin d’accès réseau au partage à partir duquel vous supprimez les informations d’identification stockées. |

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
| 204 | La demande d’informations d’identification a réussi. |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Xbox