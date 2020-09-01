---
title: Partage d’objets nommés
description: Cette rubrique explique comment partager des objets nommés entre des applications plateforme Windows universelle (UWP) et des applications Win32.
ms.date: 04/06/2020
ms.topic: article
keywords: windows 10, uwp
ms.openlocfilehash: 38d08e71c44945a7b22f124d15507c7889f8589d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175613"
---
# <a name="sharing-named-objects"></a>Partage d’objets nommés

Cette rubrique explique comment partager des objets nommés entre des applications plateforme Windows universelle (UWP) et des applications Win32.

## <a name="named-objects-in-packaged-applications"></a>Objets nommés dans des applications empaquetées

Les [objets nommés](/windows/win32/sync/object-names) offrent un moyen simple pour les processus de partager des handles d’objets. Une fois qu’un processus a créé un objet nommé, les autres processus peuvent utiliser le nom pour appeler la fonction appropriée afin d’ouvrir un handle vers l’objet. Les objets nommés sont couramment utilisés pour la [synchronisation des threads](/windows/win32/sync/interprocess-synchronization) et la [communication entre processus](./interprocess-communication.md).

Par défaut, les applications empaquetées peuvent accéder uniquement aux objets nommés qu’ils ont créés. Pour partager des objets nommés avec des applications empaquetées, les autorisations doivent être définies lors de la création des objets, et les noms doivent être qualifiés lorsque des objets sont ouverts.

## <a name="creating-named-objects"></a>Création d’objets nommés

Les objets nommés sont créés avec une `Create` API correspondante :

* [CreateEvent](/windows/win32/api/synchapi/nf-synchapi-createeventexw)
* [CreateFileMapping](/windows/win32/api/memoryapi/nf-memoryapi-createfilemappingw)
* [CreateMutex](/windows/win32/api/synchapi/nf-synchapi-createmutexexw)
* [CreateSemaphore,](/windows/win32/api/synchapi/nf-synchapi-createsemaphoreexw)
* [CreateWaitableTimer](/windows/win32/api/synchapi/nf-synchapi-createwaitabletimerexw)

Toutes ces API partagent un `LPSECURITY_ATTRIBUTES` paramètre qui permet à l’appelant de spécifier des [listes de contrôle d’accès (ACL)](/previous-versions/windows/desktop/legacy/aa379560(v=vs.85)) pour contrôler les processus qui peuvent accéder à l’objet. Pour partager des objets nommés avec des applications empaquetées, l’autorisation doit être accordée dans les listes de contrôle d’accès lors de la création des objets nommés.

Les identificateurs de sécurité (SID) représentent des identités dans les listes de contrôle d’accès. Chaque application empaquetée a son propre SID en fonction de son nom de famille de packages. Vous pouvez générer le SID pour une application empaquetée en passant son nom de famille de packages à [DeriveAppContainerSidFromAppContainerName](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername).

> [!NOTE]
> Le nom de la famille de packages est accessible via l’éditeur de manifeste de package dans Visual Studio pendant le développement, via l' [espace partenaires](../publish/view-app-identity-details.md) pour les applications publiées via le Microsoft Store, ou via la commande PowerShell [AppxPackage](/powershell/module/appx/get-appxpackage?view=win10-ps) pour les applications qui sont déjà installées.

[Cet exemple](/windows/win32/api/securityappcontainer/nf-securityappcontainer-getappcontainernamedobjectpath#examples) illustre le modèle de base requis pour la liste de contrôle d’accès d’un objet nommé. Pour partager des objets nommés avec des applications empaquetées, créez une structure [EXPLICIT_ACCESS](/windows/win32/api/accctrl/ns-accctrl-explicit_access_w) pour chaque application :

* `grfAccessMode = GRANT_ACCESS`
* `grfAccessPermissions =` autorisations appropriées en fonction de l’objet et de l’utilisation prévue
    * [Droits d’accès génériques](/windows/win32/secauthz/generic-access-rights)
    * [Sécurité de l’objet de synchronisation et droits d’accès](/windows/win32/sync/synchronization-object-security-and-access-rights)
    * [Sécurité du mappage de fichiers et droits d’accès](/windows/win32/memory/file-mapping-security-and-access-rights)
* `grfInheritance = NO_INHERITANCE`
* `Trustee.TrusteeForm = TRUSTEE_IS_SID`
* `Trustee.TrusteeType = TRUSTEE_IS_USER`
* `Trustee.ptstrName =` SID acquis à partir de [DeriveAppContainerSidFromAppContainerName](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername)

En remplissant le `LPSECURITY_ATTRIBUTES` paramètre dans les `Create` appels avec des `EXPLICIT_ACCESS` règles pour les applications empaquetées, vous pouvez accorder l’accès à ces applications pour ouvrir l’objet nommé.

> [!NOTE]
> Les applications Win32 peuvent accéder à tous les objets nommés créés par des applications empaquetées, à condition qu’elles qualifient les noms des objets lors de [leur ouverture](#opening-named-objects). Ils n’ont pas besoin d’avoir accès.

## <a name="opening-named-objects"></a>Ouverture d’objets nommés

Les objets nommés sont ouverts en passant un nom à une `Open` API correspondante :

* [OpenEvent](/windows/win32/api/synchapi/nf-synchapi-openeventw)
* [OpenFileMapping](/windows/win32/api/memoryapi/nf-memoryapi-openfilemappingw)
* [OpenMutex](/windows/win32/api/synchapi/nf-synchapi-openmutexw)
* [OpenSemaphore](/windows/win32/api/synchapi/nf-synchapi-opensemaphorew)
* [OpenWaitableTimer](/windows/win32/api/synchapi/nf-synchapi-openwaitabletimerw)

Les objets nommés créés par une application empaquetée sont créés dans l’espace de noms de l’application, également appelé chemin d’accès de l’objet nommé. Lors de l’ouverture d’objets nommés créés par une application empaquetée, les noms d’objets doivent être préfixés avec le chemin d’accès de l’objet nommé de l’application de création.

[GetAppContainerNamedObjectPath](/windows/win32/api/securityappcontainer/nf-securityappcontainer-getappcontainernamedobjectpath) retourne le chemin d’accès de l’objet nommé pour une application empaquetée en fonction de son SID. Vous pouvez générer le SID pour une application empaquetée en passant son nom de famille de packages à [DeriveAppContainerSidFromAppContainerName](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername).

> [!NOTE]
> Le nom de la famille de packages est accessible via l’éditeur de manifeste de package dans Visual Studio pendant le développement, via l' [espace partenaires](../publish/view-app-identity-details.md) pour les applications publiées via le Microsoft Store, ou via la commande PowerShell [AppxPackage](/powershell/module/appx/get-appxpackage?view=win10-ps) pour les applications qui sont déjà installées.

Quand vous ouvrez des objets nommés créés par une application empaquetée, utilisez le format `<PATH>\<NAME>` :

* Remplacez `<PATH>` par le chemin d’accès de l’objet nommé de l’application de création.
* Remplacez `<NAME>` par le nom de l’objet.

> [!NOTE]
> Le préfixe des noms d’objets avec `<PATH>` est requis uniquement si une application empaquetée a créé l’objet. Les objets nommés créés par des applications Win32 n’ont pas besoin d’être qualifiés, bien que l’accès doive toujours être accordé lors de la [création](#creating-named-objects)des objets.

## <a name="remarks"></a>Remarques

Les objets nommés dans les applications empaquetées sont isolés par défaut pour préserver la sécurité et garantir la prise en charge des événements de cycle de vie des applications tels que la suspension et l’arrêt. Le partage d’objets nommés entre les applications introduit des contraintes strictes de liaison et de contrôle de version et requiert que chaque application soit résiliente au cycle de vie des autres. Pour ces raisons, il est recommandé de partager uniquement des objets nommés entre les applications du même éditeur.