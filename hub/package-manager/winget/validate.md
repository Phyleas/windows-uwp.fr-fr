---
title: winget validate, commande
description: Valide un fichier manifeste pour l’envoi de logiciels dans le dépôt de manifestes de package de la communauté Microsoft sur GitHub.
ms.date: 04/28/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ec1f15ef9086c1083430c9bbe55ea52827ae4bfb
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334460"
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
