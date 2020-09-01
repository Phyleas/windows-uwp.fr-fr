---
title: API REST du portail d’appareils Xbox
description: Consultez la liste des rubriques de référence sur l’API REST du portail d’appareils Xbox, qui permet de configurer et de gérer à distance votre console.
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 5ae8e953-0465-487b-81dd-54a85c904daf
ms.localizationpriority: medium
ms.openlocfilehash: be8558daa39b126061caaa132fb134c8bdef15c1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172773"
---
# <a name="xbox-device-portal-rest-api"></a>API REST du portail d’appareils Xbox

Cette section contient des rubriques de référence sur l’API REST du portail d’appareils Xbox, qui permet de configurer et de gérer à distance votre console.

| URI        | Description |
|------------|-------------|
|[/api/app/packagemanager/register](wdp-loose-folder-register-api.md)| Inscrit une application qui est contenue dans un dossier isolé. |
|[/api/app/packagemanager/upload](wdp-folder-upload.md)| Charge un dossier complet sur la console. |
|[/ext/app/sshpins](uwp-sshpins-api.md)| Effacez tous les codes confidentiels SSH approuvés à distance. Nécessite à nouveau le couplage de code confidentiel pour le développement UWP Visual Studio. |
|[/ext/app/deployinfo](uwp-deployinfo-api.md)| Demande des informations de déploiement pour un ou plusieurs packages installés. |
|[/ext/fiddler](wdp-fiddler-api.md)| Active et désactive le traçage réseau Fiddler. |
|[/ext/httpmonitor/sessions](wdp-httpMonitor-api.md)| Obtient le trafic HTTP à partir de l’application ciblée sur Xbox. |
|[/ext/networkcredential](uwp-networkcredentials-api.md)| Ajoutez, supprimez ou mettez à jour les informations d’identification réseau. |
|[/ext/remoteinput](uwp-remoteinput-api.md)| Envoyer un clavier, une souris ou une entrée de contrôleur à distance à une Xbox. |
|[/ext/remoteinput/controllers](uwp-remoteinput-controllers-api.md)| Récupérez le nombre de contrôleurs physiques attachés ou désactivez tous les contrôleurs physiques. |
|[/ext/screenshot](wdp-media-capture-api.md)| Capture une représentation PNG de l’écran actuellement affiché sur la console. |
|[/ext/settings](wdp-xboxsettings-api.md)| Accède aux paramètres du développeur Xbox One. |
|[/ext/smb/developerfolder](wdp-smb-api.md)| Accède au dossier de développement sur votre console via l’Explorateur de fichiers sur votre ordinateur de développement. |
|[/ext/user](wdp-user-management.md)| Gère les utilisateurs sur la console Xbox One. |
|[/ext/xbox/info](wdp-xboxinfo-api.md)| Donne des informations sur l’appareil Xbox One. |
|[/ext/xboxlive/sandbox](wdp-sandbox-api.md)| Gère votre sandbox Xbox Live. |

## <a name="see-also"></a>Voir aussi

- [UWP sur Xbox One](index.md)
- [Portail d’appareil Windows](../debug-test-perf/device-portal.md)