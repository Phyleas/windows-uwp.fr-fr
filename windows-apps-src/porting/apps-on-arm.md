---
title: Windows 10 sur ARM
description: Cet article présente comment les expériences et applications sont exécutées sur ARM ainsi que leurs limites concernées. Vous découvrirez également les références présentant de plus amples informations.
ms.date: 02/15/2018
ms.topic: article
keywords: windows 10 s, toujours connecté, ARM, ARM64, émulation x86
ms.localizationpriority: medium
ms.openlocfilehash: 1a72fbaf4982f2a053298f10279eacba6a46d05d
ms.sourcegitcommit: 8a88a05ad89aa180d41a93152632413694f14ef8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726002"
---
# <a name="windows-10-on-arm"></a>Windows 10 sur ARM
À l’origine, Windows 10 (par comparaison avec Windows 10 Mobile) peut s’exécuter uniquement sur les ordinateurs munis de processeurs x86 et x64. Désormais, Windows 10 Desktop peut s’exécuter sur des machines qui sont alimentées par des processeurs ARM64 avec la mise à jour des créateurs de automne ou une version plus récente. La nature axée sur l'économie d'énergie de l'architecture CPU ARM permet à tous les PC de profiter d'une autonomie de batterie durant toute la journée et de la prise en charge de réseaux de données mobiles. Ces PC fournissent une excellente compatibilité d'application et vous permette d'exécuter les applications win32 x86 héritées et non modifiées. Pour plus d’informations ou pour obtenir une démonstration, consultez la [vidéo Channel 9 pour le PC Always Connected](https://channel9.msdn.com/Events/Build/2017/P4171).

Nous utilisons le terme *ARM* ici comme raccourci pour les PC qui exécutent la version bureau de Windows 10 sur les processeurs ARM64 (également communément appelés *AArch64*) processeurs.  Nous utilisons le terme *ARM32* ici comme raccourci pour l’architecture ARM 32 bits (communément appelé *ARM* dans d'autres documentations).

## <a name="apps-and-experiences-on-arm"></a>Applications et expériences sur ARM

### <a name="built-in-windows-10-experiences-apps-and-drivers"></a>Expériences, applications et pilotes intégrés à Windows 10
Les expériences Windows 10 intégrées, telles que Edge, Cortana, le menu Démarrer et l’Explorateur sont toutes natives et s’exécutent en tant que ARM64. Cela comprend également tous les pilotes de périphérique, tels que les graphiques, la mise en réseau ou le disque dur. Cela vous permet de bénéficier de la meilleure expérience utilisateur et de la durée de vie de la batterie de votre appareil en cours d’exécution à la vitesse Native totale du processeur Snapdragon Qualcomm.

### <a name="universal-windows-platform-uwp-apps"></a>Applications de plateforme Windows universelle (UWP)
Windows 10 sur ARM exécute toutes les [applications UWP](../get-started/universal-application-platform-guide.md) x86, ARM32 et ARM64 à partir du Microsoft Store. Les applications ARM32 et ARM64 s’exécutent en mode natif sans émulation, tandis que les applications x86 s’exécutent sous une émulation. Si vous êtes un développeur UWP, assurez-vous de soumettre un package ARM pour vos applications dans la mesure où cela permettra d'offrir la meilleure expérience utilisateur pour l'appareil. Pour plus d’informations, consultez [Architecture de package de l'application](/windows/msix/package/device-architecture).

>[!NOTE]
> Pour créer votre application UWP en vue de cibler la plateforme ARM64 en mode natif, vous devez disposer de Visual Studio 2017 version 15,9 ou ultérieure, ou de Visual Studio 2019. Pour plus d’informations, consultez ce billet de [blog](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development).


>[!IMPORTANT]
> Windows 10 sur ARM prend en charge les applications UWP x86, ARM32 et ARM64 à partir du Store sur les appareils ARM64. Lorsqu’un utilisateur télécharge votre application UWP sur un appareil ARM64, le système d’exploitation installe automatiquement la version optimale de votre application qui est disponible. Si vous envoyez des versions x86, ARM32 et ARM64 de votre application au Windows Store, le système d’exploitation installe automatiquement la version ARM64 de votre application. Si vous envoyez uniquement des versions x86 et ARM32 de votre application, le système d’exploitation installe la version de ARM32. Si vous envoyez uniquement la version x86 de votre application, le système d’exploitation installe cette version et l’exécute sous émulation. Pour plus d’informations sur les architectures, consultez [Architecture de package de l'application](/windows/msix/package/device-architecture).

### <a name="win32-apps"></a>Applications Win32
En plus des applications UWP, Windows 10 sur ARM peut également exécuter vos applications Win32 Win32 sans modification, avec de bonnes performances et une expérience utilisateur transparente, tout comme n’importe quel PC. Ces applications Win32 x86 n’ont pas besoin d’être recompilées pour ARM et ne se rendent même pas compte qu’elles s’exécutent sur un processeur ARM. Notez que les applications Win32 x64 64 bits ne sont pas prises en charge, mais la plupart des applications ont des versions x86 disponibles.  Quand vous avez le choix de l’architecture d’application, choisissez simplement la version 32 bits x86 pour exécuter l’application sur un PC Windows 10 sur ARM.

## <a name="in-this-section"></a>Dans cette section
|Rubrique | Description |
|-----|-----|
|[Fonctionnement de l’émulation x86 sur ARM](apps-on-arm-x86-emulation.md)|Une présentation détaillée du mode d'émulation es applications x86 sur ARM.|
|[Résolution des problèmes relatifs aux applications x86 sur ARM](apps-on-arm-troubleshooting-x86.md)|Problèmes courants avec les applications x86 lors de l'exécution sur ARM, et comment les résoudre. |
|[Dépannage des applications ARM sur ARM](apps-on-arm-troubleshooting-arm32.md)|Problèmes courants avec les applications ARM32 et ARM64 lors de l’exécution sur ARM, et comment les corriger. |
|[Utilitaire de résolution de problèmes de compatibilité des programmes sur ARM](apps-on-arm-program-compat-troubleshooter.md)|Conseils de réglage des paramètres de compatibilité si votre application ne fonctionne pas correctement sur ARM. |

## <a name="related-topics"></a>Rubriques connexes
|Rubrique | Description |
|-----|-----|
|[Génération de pilotes ARM64 avec le kit WDK](https://docs.microsoft.com/windows-hardware/drivers/develop/building-arm64-drivers)|Instructions pour la création d’un pilote ARM64. |
| [Débogage des applications x86 sur ARM](https://docs.microsoft.com/windows-hardware/drivers/debugger/debugging-arm64) | Conseils pour le débogage des applications x86 sur ARM. |
