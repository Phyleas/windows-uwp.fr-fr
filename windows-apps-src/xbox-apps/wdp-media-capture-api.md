---
title: Informations de référence sur les API de capture multimédia
description: Découvrez comment capturer une représentation PNG de l’écran actuel à l’aide de l’API REST du portail d’appareils Xbox.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 3f92c8fd-4096-4972-97da-01ae5db6423c
ms.localizationpriority: medium
ms.openlocfilehash: 5d5cd87b95f923fc0303d5cd4ed7ecbff70b8209
ms.sourcegitcommit: b0a82c2a132212eb5fb72b67f0789cac1014642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254194"
---
# <a name="media-capture-api-reference"></a>Informations de référence sur les API de capture multimédia

## <a name="request"></a>Requête

Vous pouvez capturer une représentation PNG de l’écran actuel en utilisant le format de requête suivant.

| Méthode        | URI de demande     |
| ------------- |-----------------|
| GET           | /ext/screenshot |

**Paramètres d’URI**

Vous pouvez spécifier les paramètres supplémentaires suivants dans l’URI de requête :

| Paramètre d’URI       | Description     |
| ------------------- |-----------------|
| download (facultatif) | Valeur booléenne qui indique si les en-têtes de réponse HTTP doivent définir que le navigateur hôte doit télécharger la capture d’écran en tant que pièce jointe, plutôt que de l’afficher dans le navigateur. |

**En-têtes de requête**

* None

**Corps de la demande**

* None

## <a name="response"></a>response

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

| Code d'état HTTP   | Description     |
| ------------------ |-----------------|
| 200                | Demande de capture d’écran réussie et capture renvoyée |
| 5XX                | Codes d’erreur des échecs inattendus |

**Familles d’appareils disponibles**

* Windows Xbox
