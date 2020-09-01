---
title: Windows 10 sur ARM
description: Cet article fournit une vue d’ensemble de la façon dont les expériences et les applications s’exécutent sur ARM, les limitations, et où vous pouvez accéder pour en savoir plus.
ms.date: 05/22/2020
ms.topic: article
keywords: Windows 10 s, Always connected, ARM, ARM64, émulation x86
ms.localizationpriority: medium
ms.openlocfilehash: 39ff5b2aa6c72feaeaea0a7a61100196c109257c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162293"
---
# <a name="windows-10-on-arm"></a>Windows 10 sur ARM
À l’origine, Windows 10 (distingué de Windows 10 mobile) pouvait s’exécuter uniquement sur des PC qui étaient équipés de processeurs x86 et x64. Désormais, Windows 10 Desktop peut s’exécuter sur des machines qui sont alimentées par des processeurs ARM64 avec la mise à jour des créateurs de automne ou une version plus récente. La nature de l’économie d’énergie de l’architecture de l’UC ARM permet à ces PC d’avoir une autonomie de batterie et une prise en charge de tous les jours pour les réseaux de données mobiles. Ces PC offrent une excellente compatibilité des applications et vous permettent d’exécuter vos applications Win32 x86 existantes sans les modifier. Pour plus d’informations ou pour obtenir une démonstration, consultez la [vidéo Channel 9 pour le PC Always Connected](https://channel9.msdn.com/Events/Build/2017/P4171).

Nous utilisons le terme *ARM* ici comme raccourci pour les PC qui exécutent la version de bureau de Windows 10 sur ARM64 (également appelés processeurs *AArch64*).  Nous utilisons le terme *ARM32* ici comme raccourci pour l’architecture ARM 32 bits (communément appelée *ARM* dans d’autres documents).

## <a name="apps-and-experiences-on-arm"></a>Applications et expériences sur ARM

### <a name="built-in-windows-10-experiences-apps-and-drivers"></a>Expériences, applications et pilotes Windows 10 intégrés
Les expériences Windows 10 intégrées, telles que Edge, Cortana, le menu Démarrer et l’Explorateur sont toutes natives et s’exécutent en tant que ARM64. Cela comprend également tous les pilotes de périphérique, tels que les graphiques, la mise en réseau ou le disque dur. Cela vous permet de bénéficier de la meilleure expérience utilisateur et de la durée de vie de la batterie de votre appareil en cours d’exécution à la vitesse Native totale du processeur Snapdragon Qualcomm.

### <a name="universal-windows-platform-uwp-apps"></a>Applications plateforme Windows universelle (UWP)
Windows 10 sur ARM exécute toutes les [applications UWP](../get-started/universal-application-platform-guide.md) x86, ARM32 et ARM64 à partir du Microsoft Store. Les applications ARM32 et ARM64 s’exécutent en mode natif sans émulation, tandis que les applications x86 s’exécutent sous une émulation. Si vous êtes un développeur UWP, veillez à soumettre un package ARM pour votre application, car cela fournira la meilleure expérience utilisateur pour l’appareil. Pour plus d’informations, consultez [architectures de package d’application](/windows/msix/package/device-architecture).

>[!NOTE]
> Pour créer votre application UWP en vue de cibler la plateforme ARM64 en mode natif, vous devez disposer de Visual Studio 2017 version 15,9 ou ultérieure, ou de Visual Studio 2019. Pour plus d’informations, consultez [ce billet de blog](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development).


>[!IMPORTANT]
> Windows 10 sur ARM prend en charge les applications UWP x86, ARM32 et ARM64 à partir du Store sur les appareils ARM64. Lorsqu’un utilisateur télécharge votre application UWP sur un appareil ARM64, le système d’exploitation installe automatiquement la version optimale de votre application qui est disponible. Si vous envoyez des versions x86, ARM32 et ARM64 de votre application au Windows Store, le système d’exploitation installe automatiquement la version ARM64 de votre application. Si vous envoyez uniquement des versions x86 et ARM32 de votre application, le système d’exploitation installe la version de ARM32. Si vous envoyez uniquement la version x86 de votre application, le système d’exploitation installe cette version et l’exécute sous émulation. Pour plus d’informations sur les architectures, consultez [architectures de package d’application](/windows/msix/package/device-architecture).

### <a name="win32-apps"></a>Applications Win32
En plus des applications UWP, Windows 10 sur ARM peut également exécuter vos applications Win32 Win32 sans modification, avec de bonnes performances et une expérience utilisateur transparente, tout comme n’importe quel PC. Ces applications Win32 x86 n’ont pas besoin d’être recompilées pour ARM et ne se rendent même pas compte qu’elles s’exécutent sur un processeur ARM. Notez que les applications Win32 x64 64 bits ne sont pas prises en charge, mais la plupart des applications ont des versions x86 disponibles.  Quand vous avez le choix de l’architecture d’application, choisissez simplement la version 32 bits x86 pour exécuter l’application sur un PC Windows 10 sur ARM.

## <a name="downloads"></a>Téléchargements

Visual Studio 2019 fournit plusieurs téléchargements d’outils pour Windows 10 sur ARM. Les utilisateurs qui ont recours à Visual Studio 2017 peuvent utiliser le programme d’installation pour rechercher et installer des outils et des packages comparables. Notez que pour effectuer cette procédure, vous devez utiliser Visual Studio 2019.

### <a name="visual-c-redistributable"></a>Redistributable Visual C++

Le package Redist Visual C++ est disponible pour les applications ARM. Visitez la [page de téléchargements de Visual Studio](https://visualstudio.microsoft.com/downloads/) pour accéder à **tous les téléchargements**, ouvrir d' **autres outils et infrastructures**, puis accédez à l’entrée **redistribuable Microsoft Visual C++ pour Visual Studio 2019** . Sélectionnez la case d’option **ARM64** , puis **Téléchargez**.

### <a name="remote-tools"></a>Outils de contrôle à distance

Outils de contrôle à distance de Visual Studio sont disponibles pour les applications ARM. Visitez la [page de téléchargements de Visual Studio](https://visualstudio.microsoft.com/downloads/) pour accéder à **tous les téléchargements**, ouvrez **outils pour Visual Studio 2019**, puis accédez à l’entrée **outils de contrôle à distance de Visual Studio 2019** . Sélectionnez la case d’option **ARM64* , puis **Téléchargez**.


## <a name="in-this-section"></a>Contenu de cette section
|Rubrique | Description |
|-----|-----|
|[Fonctionnement de l’émulation x86 sur ARM](apps-on-arm-x86-emulation.md)|Vue d’ensemble détaillant la manière dont les applications x86 sont émulées sur ARM.|
|[Résolution des problèmes relatifs aux applications x86 sur ARM](apps-on-arm-troubleshooting-x86.md)|Problèmes courants avec les applications x86 lorsqu’elles sont exécutées sur ARM et comment les corriger. |
|[Dépannage des applications ARM sur ARM](apps-on-arm-troubleshooting-arm32.md)|Problèmes courants avec les applications ARM32 et ARM64 lors de l’exécution sur ARM, et comment les corriger. |
|[Utilitaire de résolution de problèmes de compatibilité des programmes sur ARM](apps-on-arm-program-compat-troubleshooter.md)|Conseils pour l’ajustement des paramètres de compatibilité si votre application ne fonctionne pas correctement sur ARM. |

## <a name="related-topics"></a>Rubriques connexes
|Rubrique | Description |
|-----|-----|
|[Génération de pilotes ARM64 avec le kit WDK](/windows-hardware/drivers/develop/building-arm64-drivers)|Instructions pour la création d’un pilote ARM64. |
| [Débogage des applications x86 sur ARM](/windows-hardware/drivers/debugger/debugging-arm64) | Conseils pour le débogage des applications x86 sur ARM. |