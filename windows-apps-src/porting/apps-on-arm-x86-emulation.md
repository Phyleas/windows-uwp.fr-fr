---
title: Fonctionnement de l’émulation x86 et ARM32 sur ARM
description: Découvrez comment l’émulation pour les applications x86 rend l’écosystème enrichi d’applications Win32 existantes disponible sur les appareils ARM.
ms.date: 02/15/2018
ms.topic: article
keywords: Windows 10 s, toujours connecté, émulation x86 sur ARM
ms.localizationpriority: medium
ms.openlocfilehash: 61d994a4a022da671b4141ded80c6235ae38bae6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162323"
---
# <a name="how-x86-emulation-works-on-arm"></a>Fonctionnement de l’émulation x86 sur ARM
L’émulation pour les applications x86 rend l’écosystème enrichi d’applications Win32 disponible sur ARM. Cela permet à l’utilisateur d’exécuter une application Win32 x86 existante sans aucune modification de l’application. L’application ne sait même pas qu’elle s’exécute sur un PC Windows sur ARM, sauf si elle appelle des API spécifiques ([IsWoW64Process2](/windows/desktop/api/wow64apiset/nf-wow64apiset-iswow64process2)).

La couche [WOW64](/windows/desktop/WinProg64/running-32-bit-applications) de Windows 10 permet au code x86 de s’exécuter sur la version ARM64 de Windows 10. l’émulation x86 fonctionne en compilant des blocs d’instructions x86 dans des instructions ARM64 avec des optimisations pour améliorer les performances. Un service met en cache ces blocs de code traduits pour réduire la charge de la traduction d’instructions et permettre l’optimisation lors de l’exécution du code. Les caches sont générés pour chaque module afin que d’autres applications puissent les utiliser lors du premier lancement. 

Pour plus d’informations sur ces technologies, consultez la vidéo [Windows 10 sur ARM](https://channel9.msdn.com/Events/Build/2017/P4171) channel9.