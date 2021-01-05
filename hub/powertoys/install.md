---
title: Installer les PowerToys
description: Installez les PowerToys, un ensemble d’utilitaires pour la personnalisation de Windows 10, à l’aide d’un fichier exécutable ou d’un gestionnaire de package (WinGet, chocolaty, Scoop).
ms.date: 12/02/2020
ms.topic: quickstart
ms.localizationpriority: medium
ms.openlocfilehash: c695be3784ff2145788e4887b5f9022fcc7a6999
ms.sourcegitcommit: ea1115b921d18c7bbddc95dba9275568ff57af02
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/28/2020
ms.locfileid: "97794225"
---
# <a name="install-powertoys"></a>Installer les PowerToys

Il existe plusieurs façons d’installer les PowerToys :

- **[Fichier exécutable Windows. exe](#install-with-windows-executable-file)** *(recommandé)*
- [Gestionnaire de package Windows](#install-with-windows-package-manager-preview) *(version préliminaire)*
- [Outils d’installation pilotés par la communauté](#community-driven-install-tools) *(non pris en charge officiellement)*

## <a name="requirements"></a>Configuration requise

- Windows 10 1803 (Build 17134) ou version ultérieure.
- [Runtime de bureau .net Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/thank-you/runtime-desktop-3.1.4-windows-x64-installer). Le programme d’installation des PowerToys gère cette exigence.

Pour vous assurer que votre ordinateur répond à ces exigences, vérifiez votre version de Windows 10 et votre numéro de build en sélectionnant la **⊞ Win** *(touche Windows)*  +  **R**, puis tapez **winver**, puis sélectionnez **OK**. (Ou entrez la commande `ver` dans l’invite de commandes Windows). Vous pouvez [mettre à jour vers la dernière version de Windows](ms-settings:windowsupdate) dans le menu **paramètres** .

## <a name="install-with-windows-executable-file"></a>Installer avec le fichier exécutable Windows

Pour installer les PowerToys à l’aide d’un fichier exécutable Windows :

1. Visitez la [page des versions de Microsoft PowerToys GitHub](https://github.com/microsoft/PowerToys/releases/).
2. Parcourez la liste des versions stable et expérimentale des PowerToys disponibles.
3. Sélectionnez le menu déroulant **ressources** pour afficher les fichiers de la version.
4. Sélectionnez le `PowerToysSetup-0.##.#-x64.exe` fichier pour télécharger le programme d’installation de l’exécutable PowerToys.
5. Une fois téléchargé, ouvrez le fichier exécutable et suivez les invites d’installation.

**Actuellement, il s’agit de la méthode d’installation recommandée.**

## <a name="install-with-windows-package-manager-preview"></a>Installer avec le gestionnaire de package Windows (version préliminaire)

Pour installer les PowerToys à l’aide de la version préliminaire du gestionnaire de package Windows (WinGet) :

1. Téléchargez les PowerToys à partir du [Gestionnaire de package Windows](https://github.com/microsoft/winget-cli/releases).
2. Exécutez la commande suivante à partir de la ligne de commande/PowerShell :

```powershell
WinGet install powertoys
```

## <a name="community-driven-install-tools"></a>Outils d’installation pilotés par la communauté

Ces autres méthodes d’installation basées sur la Communauté ne sont pas officiellement prises en charge et l’équipe PowerToys ne met pas à jour ou ne gère pas ces packages.

### <a name="install-with-chocolatey"></a>Installer avec le chocolat

Pour installer les PowerToys à l’aide de [Chocolate](https://chocolatey.org/), exécutez la commande suivante à partir de votre ligne de commande/PowerShell :

```powershell
choco install powertoys
```

Pour mettre à niveau PowerToys, exécutez :

```powershell
choco upgrade powertoys
```

Si vous rencontrez des problèmes lors de l’installation ou de la mise à niveau, consultez le [package PowerToys sur chocolatey.org](https://chocolatey.org/packages/powertoys) et suivez le [processus de triage en chocolat](https://chocolatey.org/docs/package-triage-process).

### <a name="install-with-scoop"></a>Installer avec Scoop

Pour installer les PowerToys à l’aide de [Scoop](https://scoop.sh/), exécutez la commande suivante à partir de la ligne de commande/PowerShell :

```powershell
scoop install powertoys
```

Pour mettre à jour les PowerToys, exécutez la commande suivante à partir de la ligne de commande/PowerShell :

```powershell
scoop update powertoys
```

Si vous rencontrez des problèmes lors de l’installation/la mise à jour, envoyez un problème dans l' [référentiel scoop sur GitHub](https://github.com/lukesampson/scoop/issues).
