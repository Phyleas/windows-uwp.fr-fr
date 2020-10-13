---
title: search, commande
description: Interroge les sources pour rechercher les applications disponibles à installer
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 30d519b5038dfa3d13e7c6f7ba3fead439ce9e3c
ms.sourcegitcommit: 6cb20dca1cb60b4f6b894b95dcc2cc3a166165ad
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/01/2020
ms.locfileid: "91636499"
---
# <a name="search-command-winget"></a>search, commande (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

La commande **search** de l’outil [winget](index.md) interroge les sources pour rechercher les applications disponibles à installer.  

La commande **search** peut montrer toutes les applications disponibles, ou être filtrée pour rechercher une application particulière. La commande **search** sert généralement à identifier la chaîne à utiliser pour installer une application spécifique.

## <a name="usage"></a>Utilisation

`winget search [[-q] \<query>] [\<options>]`

![Capture d’écran de la fenêtre Windows PowerShell avec les résultats de la recherche winget.](images\search.png)

## <a name="arguments"></a>Arguments

Les arguments suivants sont disponibles.

| Argument  | Description |
 --------------|-------------|
| **-q,--query** |  Requête utilisée pour rechercher une application. |
| **-?, --help** |  Fournit de l’aide supplémentaire sur cette commande. |

## <a name="show-all"></a>Afficher tout

Si la commande search n’inclut ni filtre ni option, elle affiche toutes les applications disponibles dans la source par défaut. Vous pouvez également rechercher toutes les applications dans une autre source en passant uniquement l’option **source**.

## <a name="search-strings"></a>Chaînes de recherche

Les chaînes de recherche peuvent être filtrées à l’aide des options suivantes.

| Option  | Description |
 --------------|-------------|
| **--id**        |   Limite la recherche à l’ID de l’application. L’ID comprend le nom de l’éditeur et le nom de l’application. |
| **--name**      |  Limite la recherche au nom de l’application. |
| **--moniker**  |    Limite la recherche au moniker spécifié. |
| **--tag**    |  Limite la recherche aux étiquettes listées pour l’application. |
| **--command**   |   Limite la recherche aux commandes listées pour l’application. |

La chaîne sera traitée comme une sous-chaîne. La recherche par défaut n’est pas sensible à la casse. Par exemple, `winget search micro` peut retourner les résultats suivants :

* Microsoft
* microscope
* MyMicro

## <a name="search-options"></a>Options de recherche

La commande search prend en charge plusieurs options ou filtres qui permettent de limiter les résultats.

| Option  | Description |
 --------------|-------------|
| **-e, --exact**  |     Utilise la chaîne exacte dans la requête, y compris la vérification du respect de la casse. Elle n’utilise pas le comportement par défaut d’une sous-chaîne.  |  
| **-n, --count**      |  Limite le nombre de résultats affichés au nombre spécifié. |
| **-s, --source**     |  Limite la recherche au nom de [source](source.md) spécifié.  |

## <a name="related-topics"></a>Rubriques connexes

* [Utiliser l’outil winget pour installer et gérer des applications](index.md)
