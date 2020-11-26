---
title: install, commande
description: Installe l’application spécifiée.
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 5daae6dabee1201dd9df0b83dc56f98b06b15487
ms.sourcegitcommit: 4df27104a9e346d6b9fb43184812441fe5ea3437
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96025169"
---
# <a name="install-command-winget"></a>install, commande (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

La commande **install** de l’outil [winget](index.md) installe l’application spécifiée. Utilisez la commande [**search**](search.md) pour identifier l’application que vous souhaitez installer.  

Avec la commande **install**, vous devez entrer la chaîne exacte à utiliser pour l’installation. En cas d’ambiguïté, vous êtes invité à filtrer plus précisément la commande **install** sur une application exacte.

## <a name="usage"></a>Utilisation

`winget install [[-q] \<query>] [\<options>]`

![Commande search](images\install.png)

## <a name="arguments"></a>Arguments

Les arguments suivants sont disponibles.

| Argument      | Description |
|-------------|-------------|  
| **-q,--query**  |  Requête utilisée pour rechercher une application. |
| **-?, --help** |  Fournit de l’aide supplémentaire sur cette commande. |

## <a name="options"></a>Options

Les options vous permettent de personnaliser l’expérience d’installation en fonction de vos besoins.

| Option      | Description |
|-------------|-------------|  
| **-m, --manifest** |   Doit être suivie du chemin du fichier manifeste (YAML). Vous pouvez utiliser le manifeste pour exécuter l’installation à partir d’un [fichier YAML local](#local-install). |
| **--id**    |  Limite l’installation à l’ID de l’application.   |  
| **--name**   |  Limite la recherche au nom de l’application. |  
| **--moniker**   | Limite la recherche au moniker listé pour l’application. |  
| **-v, --version**  |  Vous permet de spécifier une version précise à installer. Si aucune version n’est spécifiée, la version la plus récente de l’application est installée. |  
| **-s, --source**   |  Limite la recherche au nom de source spécifié. Doit être suivi du nom de la source. |  
| **-e, --exact**   |   Utilise la chaîne exacte dans la requête, y compris la vérification du respect de la casse. Elle n’utilise pas le comportement par défaut d’une sous-chaîne. |  
| **-i, --interactive** |  Exécute le programme d’installation en mode interactif. L’expérience par défaut montre la progression du programme d’installation. |  
| **-h, --silent** |  Exécute le programme d’installation en mode silencieux. Vous ne voyez aucune invite s’afficher. L’expérience par défaut montre la progression du programme d’installation. |  
| **-o, --log**  |  Dirige la journalisation vers un fichier journal spécifique. Vous devez fournir le chemin d’un fichier sur lequel vous disposez des droits d’écriture. |
| **--override** | Chaîne qui sera passée directement au programme d’installation.    |
| **-l, --location** |    Emplacement d’installation (si pris en charge). |

### <a name="example-queries"></a>Exemples de requêtes

L’exemple suivant installe une version spécifique d’une application.

```CMD
winget install powertoys --version 0.15.2
```

L’exemple suivant installe une application à partir de son ID.

```CMD
winget install --id Microsoft.PowerToys
```

L’exemple suivant installe une application par version et ID.

```CMD
winget install --id Microsoft.PowerToys --version 0.15.2
```

## <a name="multiple-selections"></a>Sélections multiples

Si la requête fournie à **winget** détecte plusieurs applications, **winget** affiche les résultats de la recherche. Vous avez alors les données supplémentaires nécessaires pour affiner la recherche d’une installation correcte.

La meilleure façon de limiter la sélection à un seul fichier est d’utiliser l’**id** de l’application et le combiner à l’option de requête **exact**.  Par exemple :

```CMD
winget install --id Git.Git -e 
```

## <a name="local-install"></a>Installation locale

L’option **manifest** vous permet d’installer une application en passant un fichier YAML directement au client. L’option **manifest** s’utilise de la manière suivante.

Utilisation : `winget install --manifest \<file>`

| Option  | Description |
|-------------|-------------|  
|  **-m, --manifest** | Chemin du manifeste de l’application à installer. |

### <a name="log-files"></a>Fichiers journaux

Les fichiers journaux de winget, sauf s’ils ont été redirigés, se trouvent dans le dossier suivant :  **\%temp%\\AICLI\\*.log**

## <a name="related-topics"></a>Rubriques connexes

* [Utiliser l’outil winget pour installer et gérer des applications](index.md)
