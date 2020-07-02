---
title: show, commande
description: Affiche les détails de l’application spécifiée, y compris les détails sur la source de l’application ainsi que les métadonnées associées à l’application.
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 9443bb31133ee9643571a4861af7027272b35d94
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334501"
---
# <a name="show-command-winget"></a>show, commande (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

La commande **show** de l’outil [winget](index.md) affiche les détails de l’application spécifiée, y compris les détails sur la source de l’application ainsi que les métadonnées associées à l’application.

La commande **show** affiche uniquement les métadonnées qui ont été envoyées avec l’application. Si l’application envoyée exclut certaines métadonnées, les données ne seront pas affichées.

## <a name="usage"></a>Utilisation

`winget show [[-q] \<query>] [\<options>]`

![Commande show](images\show.png)

## <a name="arguments"></a>Arguments

Les arguments suivants sont disponibles.

| Argument  | Description |
|--------------|-------------|
| **-q,--query** |  Requête utilisée pour rechercher une application. |
| **-?, --help** |  Fournit de l’aide supplémentaire sur cette commande. |

## <a name="options"></a>Options

Les options suivantes sont disponibles.

| Option  | Description |
|--------------|-------------|
| **-m,--manifest** | Chemin du manifeste de l’application à installer. |
| **--id**         |  Filtre les résultats par ID. |
| **--name**   |      Filtre les résultats par nom. |
| **--moniker**   |  Filtre les résultats par moniker d’application. |
| **-v,--version** |  Utilise la version spécifiée. La version la plus récente est utilisée par défaut. |
| **-s,--source** |   Recherche l’application en utilisant la [source](source.md) spécifiée. |
| **-e,--exact**     | Recherche l’application en utilisant la correspondance exacte. |
| **--versions**    | Affiche les versions disponibles de l’application. |

## <a name="multiple-selections"></a>Sélections multiples

Si la requête fournie à **winget** détecte plusieurs applications, **winget** affiche les résultats de la recherche. Vous avez alors les données supplémentaires nécessaires pour affiner la recherche.

## <a name="results-of-show"></a>Résultats de la commande show

Si une seule application est détectée, les données suivantes sont retournées.

### <a name="metadata"></a>Métadonnées

| Value  | Description |
|--------------|-------------|
| **Id**   | ID de l’application. |
| **Nom**  | Nom de l’application. |
| **Publisher** | Éditeur de l’application. |
| **Version** | Version de l’application. |
| **Author**  | Auteur de l’application. |
| **AppMoniker** | Moniker de l’application. |
| **Description** | Description de l’application. |
| **Licence**  | Licence de l’application. |
| **LicenseUrl** | URL du fichier de licence de l’application. |
| **Homepage**  | Page d’accueil de l’application. |
| **Tags** | Étiquettes fournies pour faciliter la recherche.  |
| **Commande** | Commandes prises en charge par l’application. |
| **Channel**  | Information indiquant si l’application est une préversion ou une version finale.  |
| **Minimum OS Version** | Version minimale du système d’exploitation prise en charge par l’application. |

### <a name="installer-details"></a>Détails du programme d’installation

| Value  | Description |
|--------------|-------------|
| **Arch**   | Architecture du programme d’installation. |
| **Langue**  | Langue du programme d’installation. |
| **Installer Type**  | Type du programme d’installation. |
| **Download Url** | URL du programme d’installation. |
| **Hash** | Sha-256 du programme d’installation.  |
| **Étendue** | Indique si le programme d’installation est exécuté par ordinateur ou par utilisateur. |

## <a name="related-topics"></a>Rubriques connexes

* [Utiliser l’outil winget pour installer et gérer des applications](index.md)
