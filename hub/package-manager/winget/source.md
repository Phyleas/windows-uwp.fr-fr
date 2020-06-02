---
title: source, commande
description: Gère les dépôts auxquels le Gestionnaire de package Windows accède.
author: KevinLaMS
ms.author: kevinla
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: cb897f25324ab8a516d18f5defe7cffa3e6a0109
ms.sourcegitcommit: 5a145eda92b5915393e58006867cdd8b98e922f5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84166246"
---
# <a name="source-command-winget"></a>source, commande (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

> [!NOTE]
> La commande **source** est actuellement réservée à un usage interne. Aucune autre source n’est prise en charge pour l’instant.

La commande **source** de l’outil [winget](index.md) gère les dépôts auxquels le Gestionnaire de package Windows accède. Avec la commande **source**, vous pouvez **ajouter**, **supprimer**, **lister** et **mettre à jour** les dépôts.

Une source fournit les données dont vous avez besoin pour découvrir et installer des applications. Ajoutez une nouvelle source uniquement si vous l’approuvez en tant qu’emplacement sécurisé.

## <a name="usage"></a>Utilisation

`winget source \<sub command> \<options>`

![Image source](images\source.png)

## <a name="arguments"></a>Arguments

Les arguments suivants sont disponibles.

| Argument  | Description |
|--------------|-------------|
| **-?, --help** |  Fournit de l’aide supplémentaire sur cette commande. |

## <a name="sub-commands"></a>Sous-commandes

La commande source prend en charge les sous-commandes suivantes pour la manipulation des sources.

| Sous-commande  | Description |
|--------------|-------------|
|  **add** |  Ajoute une nouvelle source. |
|  **list** | Dresse la liste des sources activées. |
|  **update** | Met à jour une source. |
|  **remove** | Supprime une source. |
|  **reset** | Rétablit la configuration initiale de **winget**.  |

## <a name="options"></a>Options

La commande **source** prend en charge les options suivantes.

| Option  | Description |
|--------------|-------------|
|  **-n,--name** | Nom permettant d’identifier la source. |
|  **-a,--arg** | URL ou UNC de la source. |
|  **-t,--type** | Type de la source. |
| **-?, --help** |  Fournit de l’aide supplémentaire sur cette commande. |

## <a name="add"></a>ajouter

La sous-commande **add** ajoute une nouvelle source. Avec cette sous-commande, l’option **--name** et l’argument **name** sont obligatoires.

Utilisation : `winget source add [-n, --name] \<name> [-a] \<url> [[-t] \<type>]`

Exemple : `winget source add --name Contoso  https://www.contoso.com/cache`

La sous-commande **add** prend également en charge le paramètre **type** facultatif. Le paramètre **type** communique au client le type du dépôt auquel il se connecte. Les types suivants sont pris en charge.

| Type  | Description |
|--------------|-------------|
| **Microsoft.PreIndexed.Package** | Type de la source \<default>. |

## <a name="list"></a>list

La sous-commande **list** énumère les sources actuellement activées. Elle fournit également des détails sur une source spécifique.

Utilisation : `winget source list [-n, --name] \<name>`

### <a name="list-all"></a>list all

La sous-commande **list** utilisée seule affiche la liste complète des sources prises en charge. Par exemple :

```CMD
> C:\winget source list
> Name   Arg
> -----------------------------------------
> winget https://winget.azureedge.net/cache

```

### <a name="list-source-details"></a>list source details

Pour obtenir des détails complets sur la source, passez le nom utilisé pour identifier la source. Par exemple :

```CMD
> C:\winget source list --name contoso  
> Name   : contoso  
> Type   : Microsoft.PreIndexed.Package  
> Arg    : https://pkgmgr-int.azureedge.net/cache  
> Data   : AppInstallerSQLiteIndex-int_g4ype1skzj3jy  
> Updated: 2020-4-14 17:45:32.000
```

**Name** affiche le nom utilisé pour identifier la source.
**Type** affiche le type du dépôt.
**Arg** affiche l’URL ou le chemin utilisé par la source.
**Data** affiche le nom du package facultatif utilisé, le cas échéant.
**Updated** affiche la date et l’heure de la dernière mise à jour de la source.

## <a name="update"></a>mise à jour

La sous-commande **update** force la mise à jour d’une source individuelle ou de toutes les sources.

Utilisation : `winget source update [-n, --name] \<name>`

### <a name="update-all"></a>update all

La sous-commande **update** utilisée seule demande les mises à jour et les applique dans chaque dépôt. Par exemple : `C:\winget update`

### <a name="update-source"></a>source de mise à jour

La sous-commande **update** associée à l’option **--name** peut cibler et mettre à jour une source individuelle. Par exemple : `C:\winget source update --name contoso`

## <a name="remove"></a>suppression

La sous-commande **remove** supprime une source. Avec cette sous-commande, l’option **--name** et l’**argument name** sont obligatoires pour identifier la source.

Utilisation : `winget source add [-n, --name] \<name>`

Par exemple : `winget source remove --name Contoso`

## <a name="reset"></a>reset

La sous-commande **reset** rétablit la configuration d’origine du client. La sous-commande **reset** supprime toutes les sources et définit la source sur la valeur par défaut. Excepté dans de rares cas, cette sous-commande ne doit pas être utilisée.

Utilisation : `winget source reset`

Par exemple : `winget source reset`

## <a name="default-repository"></a>Dépôt par défaut

Le Gestionnaire de package Windows spécifie un dépôt par défaut. Vous pouvez identifier le dépôt en utilisant la commande **list**. Par exemple : `winget source list`

## <a name="related-topics"></a>Rubriques connexes

* [Utiliser l’outil winget pour installer et gérer des applications](index.md)
