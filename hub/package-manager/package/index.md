---
title: Envoyer des packages au Gestionnaire de package Windows
description: ''
author: denelon
ms.author: denelon
ms.date: 04/29/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: e83088c5a6b2755a8ce7f08e513d09f877580db8
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580086"
---
# <a name="submit-packages-to-windows-package-manager"></a>Envoyer des packages au Gestionnaire de package Windows

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

Si vous êtes éditeur de logiciels indépendant (ISV), vous pouvez utiliser le Gestionnaire de package Windows comme canal de distribution pour les packages logiciels contenant vos applications. Le Gestionnaire de package Windows prend actuellement en charge les programmes d’installation aux formats suivants : MSIX, MSI et EXE.

Pour envoyer des packages logiciels au Gestionnaire de package Windows, effectuez les étapes suivantes :

1. [Créez un manifeste de package qui fournit des informations sur votre application](manifest.md). Les manifestes sont des fichiers YAML qui adhèrent au schéma du Gestionnaire de package Windows.
2. [Envoyez votre manifeste au dépôt du Gestionnaire de package Windows](repository.md). Il s’agit d’un dépôt open source sur GitHub qui contient une collection de manifestes accessibles par l’outil **winget**.

## <a name="related-topics"></a>Rubriques connexes

* [Utiliser l’outil winget](../winget/index.md)
* [Créer votre manifeste de package](manifest.md)
* [Envoyer votre manifeste au dépôt](repository.md)