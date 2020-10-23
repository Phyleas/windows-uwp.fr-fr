---
title: Utiliser l’outil winget pour installer et gérer des applications
description: Avec l’outil en ligne de commande winget, les développeurs peuvent découvrir, installer, mettre à niveau, supprimer et configurer des applications sur des ordinateurs Windows 10.
ms.date: 10/22/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 0dbd2aa76fa6a9b787e73c0bbd5ce7e56b5e6a4a
ms.sourcegitcommit: c105eb358bf693d34dfdd7a44255af69c1d5a3cd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92434459"
---
# <a name="use-the-winget-tool-to-install-and-manage-applications"></a>Utiliser l’outil winget pour installer et gérer des applications

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

Avec l’outil en ligne de commande **winget**, les développeurs peuvent découvrir, installer, mettre à niveau, supprimer et configurer des applications sur des ordinateurs Windows 10. Cet outil est l’interface cliente du service Gestionnaire de package Windows.

L’outil **winget** étant encore en préversion, certaines des fonctionnalités prévues ne sont pas disponibles pour l’instant.

## <a name="install-winget"></a>Installer winget

Il existe plusieurs façons d’installer l’outil **winget** :

* L’outil **winget** est inclus dans la préversion ou version d’évaluation du [programme d’installation d’application pour Windows](https://www.microsoft.com/p/app-installer/9nblggh4nns1?ocid=9nblggh4nns1_ORSEARCH_Bing&rtc=1&activetab=pivot:overviewtab). Vous devez installer la préversion du **programme d’installation d’application** pour utiliser **winget**. Pour obtenir un accès en avant-première, envoyez votre demande d’inscription au [Programme Windows Insider du Gestionnaire de package Windows](https://aka.ms/AppInstaller_InsiderProgram). La participation à l’anneau de version d’évaluation du package vous garantit d’obtenir les dernières mises à jour de la préversion.

* Participez à l’[anneau de version d’évaluation du Programme Windows Insider](https://insider.windows.com).

* Installez le package du programme d’installation d’application de bureau pour Windows situé dans le dossier Release du [dépôt winget](https://github.com/microsoft/winget-cli).

> [!NOTE]
> L’outil **winget** nécessite Windows 10 version 1709 (10.0.16299) ou une version ultérieure de Windows 10.

## <a name="administrator-considerations"></a>Considérations relatives aux administrateurs

Le comportement du programme d’installation peut être différent selon que vous exécutez **winget** avec ou sans privilèges d’administrateur.

* Si vous exécutez **winget** sans privilèges d’administrateur, certaines applications peuvent [nécessiter une élévation de privilèges](https://docs.microsoft.com/windows/security/identity-protection/user-account-control/) pour s’installer. À l’exécution du programme d’installation, Windows affiche une invite d’[élévation](https://docs.microsoft.com/windows/security/identity-protection/user-account-control). Si vous refusez l’élévation, l’installation de l’application échoue.  

* Si vous exécutez **winget** à partir d’une invite de commandes administrateur, vous ne verrez pas d’[invites d’élévation](/windows/security/identity-protection/user-account-control/how-user-account-control-works) même si l’application nécessite une élévation. Exécutez toujours une invite de commandes administrateur avec prudence et installez uniquement des applications approuvées.

## <a name="use-winget"></a>Utiliser winget

Après avoir installé le **programme d’installation d’application**, vous pouvez exécuter **winget** en tapant « winget » dans une invite de commandes.

La recherche et l’installation d’un outil favori constitue l’un des scénarios d’usage les plus courants.

1. Pour [rechercher](search.md) un outil, tapez `winget search \<appname>`.
2. Si l’outil recherché est disponible, vous pouvez l’[installer](install.md) en tapant `winget install \<appname>`. L’outil **winget** lance le programme d’installation et installe l’application sur votre PC.
    ![winget commandline](images\install.png)

3. En plus des commandes de recherche et d’installation d’applications, **winget** fournit d’autres commandes pour [afficher les détails](show.md) des applications, [changer les sources](source.md) et [valider les packages](validate.md). Pour obtenir la liste complète des commandes, tapez : `winget --help`.
    ![winget help](images\help.png)

### <a name="commands"></a>Commandes

La préversion actuelle de l’outil **winget** prend en charge les commandes suivantes.

| Commande | Description |
|---------|-------------|
| [hash](hash.md) | Génère le hachage SHA256 pour le programme d’installation. |
| [help](help.md) | Affiche l’aide relative aux commandes de l’outil **winget**. |
| [install](install.md) | Installe l’application spécifiée. |
| [search](search.md) | Recherche une application. |
| [show](show.md) | Affiche les détails de l’application spécifiée. |
| [source](source.md) | Ajoute, supprime et met à jour les dépôts du Gestionnaire de package Windows auxquels l’outil **winget** accède. |
| [validate](validate.md) | Valide un fichier manifeste pour l’envoi dans le dépôt du Gestionnaire de package Windows. |

### <a name="options"></a>Options

La préversion actuelle de l’outil **winget** prend en charge les options suivantes.

| Option | Description |
|--------------|-------------|
| **-v,--version** | Cette option retourne la version actuelle de winget. |
| **--info** |  Fournit des informations détaillées sur winget, y compris les liens vers la licence et la déclaration de confidentialité. |
| **-?, --help** |  Fournit une aide supplémentaire sur winget. |

## <a name="supported-installer-formats"></a>Formats de programmes d’installation pris en charge

La préversion actuelle de l’outil **winget** prend en charge les types suivants de programmes d’installation.

* EXE
* MSIX
* MSI

## <a name="scripting-winget"></a>Scripts winget

Vous pouvez créer des scripts de commandes par lot et des scripts PowerShell pour installer plusieurs applications à la fois.

``` CMD
@echo off  
Echo Install Powertoys and Terminal  
REM Powertoys  
winget install Microsoft.Powertoys  
if %ERRORLEVEL% EQU 0 Echo Powertoys installed successfully.  
REM Terminal  
winget install Microsoft.WindowsTerminal  
if %ERRORLEVEL% EQU 0 Echo Terminal installed successfully.   %ERRORLEVEL%
```

> [!NOTE]
> Avec de tels scripts, **winget** lance les programmes d’installation des applications dans l’ordre spécifié. Quand un programme d’installation retourne un message de réussite ou d’échec, **winget** lance le programme d’installation suivant. Si un programme d’installation lance un autre processus, il peut être retourné à **winget** prématurément. Dans ce cas, **winget** commence à installer le programme d’installation suivant avant d’avoir fini d’installer le programme d’installation précédent.

## <a name="missing-tools"></a>Outils manquants

Si le [dépôt de la communauté](../package/repository.md) ne contient pas votre outil ou application, envoyez un package dans notre [dépôt](https://github.com/microsoft/winget-pkgs). Une fois votre outil favori ajouté, il sera mis à la disposition de tous les utilisateurs, vous compris.

## <a name="customize-winget-settings"></a>Personnaliser les paramètres winget

Vous pouvez configurer l’expérience de ligne de commande **winget** en modifiant le fichier **settings.json**. Pour plus d’informations, consultez [https://aka.ms/winget-settings](https://aka.ms/winget-settings). Notez que les paramètres sont toujours dans un état expérimental et qu’ils n’ont pas encore été finalisés pour la préversion de l’outil.

## <a name="open-source-details"></a>Détails sur l’open source

L’outil **winget** est un logiciel open source qui est disponible dans le dépôt [https://github.com/microsoft/winget-cli/](https://github.com/microsoft/winget-cli/) sur GitHub. La source utilisée pour générer le client se trouve dans le [dossier src](https://github.com/microsoft/winget-cli/tree/master/src).

La source **winget** est contenue dans une solution Visual Studio 2019 pour C++. Pour générer la solution correctement, installez la dernière version de [Visual Studio avec la charge de travail C++](https://visualstudio.microsoft.com/downloads/).

Nous vous encourageons à contribuer à la source **winget** sur GitHub. Vous pourrez le faire après avoir accepté et signé le CLA Microsoft.
