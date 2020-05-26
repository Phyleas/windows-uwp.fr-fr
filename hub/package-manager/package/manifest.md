---
title: Créer votre manifeste de package
description: ''
author: denelon
ms.author: denelon
ms.date: 04/29/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 054c8cf7fc104b78f0397f4d1536e1130668f4f8
ms.sourcegitcommit: 645cb099128072cfc905ec80b38bd280b98c9037
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83825140"
---
# <a name="create-your-package-manifest"></a>Créer votre manifeste de package

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

Si vous souhaitez envoyer un package logiciel au [dépôt du Gestionnaire de package Windows](repository.md), commencez par créer un manifeste de package. Le manifeste est un fichier YAML qui décrit l’application à installer.

Cet article décrit le contenu d’un manifeste de package pour le Gestionnaire de package Windows.

## <a name="yaml-basics"></a>Notions de base concernant YAML

Le format YAML a été choisi pour les manifestes de packages parce qu’il est relativement facile à lire et cohérent avec d’autres outils de développement Microsoft. Si vous ne connaissez pas la syntaxe YAML, vous pouvez découvrir les principes fondamentaux sur le site [Learn YAML in Y Minutes](https://learnxinyminutes.com/docs/yaml/).

> [!NOTE]
> Les manifestes pour le Gestionnaire de package Windows ne prennent actuellement pas en charge toutes les fonctionnalités YAML. Les fonctionnalités YAML non prises en charge incluent les ancres, les clés complexes et les jeux.

## <a name="conventions"></a>Conventions

Ces conventions sont utilisées dans cet article :

* À gauche de `:` figure un mot clé littéral utilisé dans les définitions de manifeste.
* À droite de `:` figure un type de données. Le type de données peut être un type primitif, par exemple une **chaîne** ou une référence à une structure riche définie ailleurs dans cet article.
* La notation `[` *type_données* `]` indique un tableau du type de données mentionné. Par exemple, `[ string ]` est un tableau de chaînes.
* La notation `{` *type_données* `:` *type_données* `}` indique une correspondance d’un type de données à un autre. Par exemple, `{ string: string }` est une correspondance de chaînes à chaînes.

## <a name="manifest-contents"></a>Contenu du manifeste

Un manifeste de package doit inclure un ensemble d’éléments obligatoires, et peut également inclure d’autres éléments facultatifs susceptibles d’aider à améliorer l’expérience d’installation de votre logiciel par le client. Cette section fournit des résumés succincts du schéma de manifeste obligatoire et des schémas de manifeste complets, ainsi que des exemples de chacun d’eux.

Chaque champ du fichier manifeste doit être en « Pascal case » (première lettre de chaque mot en majuscule) et ne peut pas être dupliqué.

Pour obtenir une liste complète et une description des éléments d’un manifeste, consultez la spécification du manifeste dans le dépôt [https://github.com/microsoft/winget-cli](https://github.com/microsoft/winget-cli).

### <a name="minimal-required-schema"></a>Schéma obligatoire minimal

#### <a name="minimal-required-schema"></a>[Schéma obligatoire minimal](#tab/minschema/)

```yaml
Id: string # publisher.package format
Publisher: string # the name of the publisher
Name: string # the name of the application
Version: string # version numbering format
License: string # the open source license or copyright
InstallerType: string # enumeration of supported installer types (exe, msi, msix, inno, wix, nullsoft, appx)
Installers:
  - Arch: string # enumeration of supported architectures
  - URL: string # path to download installation file
  - Sha256: string # SHA256 calculated from installer
ManifestVersion: 0.1.0
```

#### <a name="example"></a>[Exemple](#tab/minexample/)

```yaml
Id: microsoft.teams
Publisher: Microsoft Corporation
Name: Microsoft Teams
Version: 1.3.0.4461
License: Copyright (c) Microsoft Corporation. All rights reserved.
InstallerType: exe
Installers:
  - Arch: x64
    Url: https://statics.teams.cdn.office.net/production-windows-x64/1.3.00.4461/Teams_windows_x64.exe
    Sha256: 712f139d71e56bfb306e4a7b739b0e1109abb662dfa164192a5cfd6adb24a4e1
ManifestVersion: 0.1.0
```

* * *

### <a name="complete-schema"></a>Schéma complet

#### <a name="complete-schema"></a>[Schéma complet](#tab/compschema/)

```yaml
Id: string # publisher.package format
Publisher: string # the name of the publisher
Name: string # the name of the application
AppMoniker: string # the common name someone may use to search for the package
Version: string # version numbering format for package version
Channel: string # a string representing the flight ring
License: string # the open source license or copyright
LicenseUrl: string # valid secure URL to license
MinOSVersion: string # version numbering format for minimum version of Windows supported
Description: string # description of the package
Homepage: string # valid secure URL for the package
Tags: list # additional strings a user would use to search for the package
FileExtensions: list # list of file extensions the package could support
Protocols: list # list of protocols the package provides a handler for
Commands: list # list of commands or aliases the user would use to run the package
InstallerType: string # enumeration of supported installer types (exe, msi, msix)
Custom: string # custom switches passed to the installer
Silent: string # switches passed to the installer for silent installation
SilentWithProgress: string # switches passed to the installer for non-interactive install
Interactive: string # experimental
Language: string # experimental
Log: string # specifies log redirection switches and path
InstallLocation: string # specifies alternate location to install package
Installers: # nested map of keys for specific installer
  - Arch: string # enumeration of supported architectures
  - URL: string # path to download installation file
  - Sha256: string # SHA256 calculated from installer
  - SignatureSha256: string # SHA256 calculated from signature file's hash of MSIX file
  - Switches: # collection of entries to override root keys
  - Scope: string # experimental
  - SystemAppId: string # experimental
Localization: # nested map of keys for localization
  - Language: string # locale for display fields and localized URLs
ManifestVersion: string # version number format for manifest version
```

#### <a name="good-example"></a>[Bon exemple](#tab/good/)

```yaml
Id: microsoft.teams
Publisher: Microsoft Corporation
Name: Microsoft Teams
Version: 1.3.0.4461
License: Copyright (c) Microsoft Corporation. All rights reserved.
LicenseUrl: https://docs.microsoft.com/en-us/MicrosoftTeams/assign-teams-licenses
InstallerType: exe
Installers:
  - Arch: x64
    Url: https://statics.teams.cdn.office.net/production-windows-x64/1.3.00.4461/Teams_windows_x64.exe
    Sha256: 712f139d71e56bfb306e4a7b739b0e1109abb662dfa164192a5cfd6adb24a4e1
ManifestVersion: 0.1.0
```

#### <a name="better-example"></a>[Meilleur exemple](#tab/better/)

```yaml
Id: microsoft.teams
Publisher: Microsoft Corporation
Name: Microsoft Teams
Version: 1.3.0.4461
AppMoniker: teams
MinOSVersion: 10.0.0.0
Description: The hub for teamwork in Microsoft 365
Homepage: https://www.microsoft.com/microsoft/teams
License: Copyright (c) Microsoft Corporation. All rights reserved.
LicenseUrl: https://docs.microsoft.com/en-us/MicrosoftTeams/assign-teams-licenses
InstallerType: exe
Installers:
  - Arch: x64
    Url: https://statics.teams.cdn.office.net/production-windows-x64/1.3.00.4461/Teams_windows_x64.exe
    Sha256: 712f139d71e56bfb306e4a7b739b0e1109abb662dfa164192a5cfd6adb24a4e1
ManifestVersion: 0.1.0
```

* * *

## <a name="tips-and-best-practices"></a>Conseils et bonnes pratiques

* Afin d’optimiser l’expérience utilisateur lors de la recherche et de l’installation de votre logiciel, nous vous recommandons d’inclure le plus d’éléments facultatifs possible au-delà du schéma obligatoire. Par exemple, le champ `AppMoniker` est facultatif. Toutefois, si vous incluez ce champ, les clients verront les résultats associés à la valeur `AppMoniker` lors de l’exécution de la commande [search](../winget/search.md) (par exemple, **vscode** pour **Visual Studio Code**). S’il n’existe qu’une seule application avec la valeur `AppMoniker` spécifiée, les clients peuvent installer votre application en spécifiant le moniker au lieu de l’ID complet.
* L’`Id` doit être unique. Vous ne pouvez pas avoir plusieurs envois avec le même identificateur de package. Évitez les espaces parce que vous obligeriez les utilisateurs à placer des guillemets autour de l’`Id` lors de l’utilisation du client [winget](../index.md).
* Évitez de créer plusieurs dossiers d’éditeur. Par exemple, ne créez pas « Contoso Ltd » s’il existe déjà un dossier « Contoso ». Évitez également les espaces lors de la création de dossiers.
* Tous les packages doivent être envoyés avec une installation sans assistance dans la mesure du possible. Si vous avez un fichier exécutable qui ne prend pas en charge l’installation sans assistance, l’expérience utilisateur sera réduite.
* Limitez la longueur des chaînes dans votre manifeste à 100 caractères avant un saut de ligne.
* Quand il existe plusieurs types de programme d’installation pour la version spécifiée du package, une instance de `InstallerType` peut être placée sous chacun des `Installers`.
