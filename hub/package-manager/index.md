---
title: Gestionnaire de package Windows
description: Le Gestionnaire de package Windows est une solution de gestion de package complète qui se compose d’un outil en ligne de commande et d’un ensemble de services conçus pour l’installation des applications sur Windows 10.
ms.date: 05/03/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 5b25f2c651e11a5ff97a630bb802b236771f5441
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334626"
---
# <a name="windows-package-manager-preview"></a>Gestionnaire de package Windows (préversion)

[!INCLUDE [preview-note](../includes/package-manager-preview.md)]

Le Gestionnaire de package Windows est une [solution de gestion de package](#understanding-package-managers) complète qui se compose d’un outil en ligne de commande et d’un ensemble de services conçus pour l’installation des applications sur Windows 10.

## <a name="windows-package-manager-for-developers"></a>Gestionnaire de package Windows pour les développeurs

Les développeurs utilisent l’outil en ligne de commande **winget** pour découvrir, installer, mettre à niveau, supprimer et configurer un ensemble organisé d’applications. Une fois l’outil **winget** installé, les développeurs peuvent y accéder par le biais du Terminal Windows, de PowerShell ou de l’invite de commandes.

Pour plus d’informations, consultez [Utiliser l’outil winget pour installer et gérer des applications](winget/index.md).

## <a name="windows-package-manager-for-isvs"></a>Gestionnaire de package Windows pour les ISV

Les éditeurs de logiciels indépendants (ISV) peuvent utiliser le Gestionnaire de package Windows comme canal de distribution des packages logiciels qui contiennent leurs outils et applications. Pour l’envoi de packages logiciels (contenant des programmes d’installation .msix, .msi ou .exe) au Gestionnaire de package Windows, nous fournissons le **dépôt de manifestes de package de la communauté Microsoft** sur GitHub. Dans ce dépôt open source, les ISV peuvent charger des [manifestes de package](package/manifest.md) pour faire évaluer leurs packages logiciels en vue de les ajouter dans le Gestionnaire de package Windows. Les manifestes sont validés automatiquement, mais peuvent aussi être vérifiés manuellement.

Pour plus d’informations, consultez [Envoyer des packages au Gestionnaire de package Windows](package/repository.md).

## <a name="understanding-package-managers"></a>Présentation des gestionnaires de package

Un gestionnaire de package est un système ou un ensemble d’outils qui permet d’automatiser l’installation, la mise à niveau, la configuration et l’utilisation des logiciels. La plupart des gestionnaires de package sont conçus pour découvrir et installer des outils de développement.

Dans l’idéal, les développeurs utilisent un gestionnaire de package pour spécifier les prérequis des outils dont ils ont besoin pour développer des solutions pour un projet donné. Le gestionnaire de package suit ensuite les instructions déclaratives pour installer et configurer les outils. Le gestionnaire de package réduit le temps de préparation d’un environnement et contribue à garantir que les mêmes versions des packages sont installées sur toutes les machines.

Les gestionnaires de package tiers peuvent utiliser le [dépôt de manifestes de package de la communauté Microsoft](package/repository.md) pour augmenter la taille de leur catalogue de logiciels.

## <a name="related-topics"></a>Rubriques connexes

* [Utiliser l’outil winget pour installer et gérer des packages logiciels](winget/index.md)
* [Envoyer des packages au Gestionnaire de package Windows](package/index.md)