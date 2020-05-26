---
title: winget validate, commande
description: Valide un fichier manifeste pour l’envoi de logiciels dans le dépôt de manifestes de package de la communauté Microsoft sur GitHub.
author: KevinLaMS
ms.author: kevinla
ms.date: 04/28/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a5772e144a4226be9fbb4a4949aaac1e3d4408e6
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83824930"
---
# <a name="validate-command-winget"></a>validate, commande (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

La commande **validate** de l’outil [winget](index.md) valide un [fichier manifeste](../package/manifest.md) pour l’envoi de logiciels dans le **dépôt de manifestes de package de la communauté Microsoft** sur GitHub. Le manifeste doit être un fichier YAML conforme à la [spécification](https://github.com/microsoft/winget-pkgs/YamlSpec.md).

## <a name="usage"></a>Utilisation

`winget validate [--manifest] \<manifest>`

## <a name="arguments"></a>Arguments

Les arguments suivants sont disponibles.

| Argument  | Description |
|--------------|-------------|
| **--manifest** |  Chemin du manifeste à valider. |
| **-?, --help** |  Fournit de l’aide supplémentaire sur cette commande. |

## <a name="related-topics"></a>Rubriques connexes

* [Utiliser l’outil winget pour installer et gérer des applications](index.md)
* [Envoyer des packages au Gestionnaire de package Windows](../package/index.md)
