---
title: winget hash, commande
description: Génère le hachage SHA256 pour un programme d’installation.
ms.date: 04/28/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4b6599a2b538829c6d9107b20f5f22d22f646542
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334564"
---
# <a name="hash-command-winget"></a>hash, commande (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

La commande **hash** de l’outil [winget](index.md) génère le hachage SHA256 pour un programme d’installation. Utilisez cette commande si vous devez créer un [fichier manifeste](../package/manifest.md) pour l’envoi de logiciels dans le **dépôt de manifestes de package de la communauté Microsoft** sur GitHub. Par ailleurs, la commande **hash** prend également en charge la génération d’un hachage de certificat SHA256 pour les fichiers MSIX.

## <a name="usage"></a>Utilisation

`winget hash [-f] \<file> [\<options>]`

La sous-commande **hash** peut s’exécuter uniquement sur un fichier local. Pour utiliser la sous-commande **hash**, téléchargez votre programme d’installation à un emplacement connu. Passez ensuite le chemin du fichier comme argument à la sous-commande **hash**.

## <a name="arguments"></a>Arguments

Les arguments suivants sont disponibles :

| Argument  | Description |
|--------------|-------------|
| **-f,--file** |  Chemin complet du fichier à hacher. |
| **-m,--msix**  | Spécifie que la commande hash doit aussi créer la chaîne SignatureSha256 SHA256 pour une utilisation avec des programmes d’installation MSIX. |
| **-?, --help** |  Fournit de l’aide supplémentaire sur cette commande. |

## <a name="related-topics"></a>Rubriques connexes

* [Utiliser l’outil winget pour installer et gérer des applications](index.md)
* [Envoyer des packages au Gestionnaire de package Windows](../package/index.md)
