---
title: Dépannage des applications UWP ARM32
description: Problèmes courants avec les applications ARM32 lorsqu’elles sont exécutées sur ARM et comment les corriger.
ms.date: 01/03/2019
ms.topic: article
keywords: Windows 10 s, Always connected, ARM32 Apps on ARM, Windows 10 on ARM, résolution des problèmes
ms.localizationpriority: medium
ms.openlocfilehash: 60c7a129844d69d18287ea7885a0acaf01f1f625
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171223"
---
# <a name="troubleshooting-arm-uwp-apps"></a>Dépannage des applications UWP ARM

Si votre application ARM32 ou ARM64 UWP ne fonctionne pas correctement sur ARM, voici quelques conseils qui peuvent vous aider.

>[!NOTE]
> Pour créer votre application UWP en vue de cibler la plateforme ARM64 en mode natif, vous devez disposer de Visual Studio 2017 version 15,9 ou ultérieure, ou de Visual Studio 2019. Pour plus d’informations, consultez [ce billet de blog](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development).


## <a name="common-issues"></a>Problèmes courants
Voici quelques problèmes courants à prendre en compte lors de la résolution des problèmes liés aux applications ARM32 et ARM64.

### <a name="using-windows-10-mobile-only-apis-on-arm-based-processors"></a>Utilisation des API Windows 10 Mobile uniquement sur les processeurs ARM
Les applications ARM peuvent rencontrer des problèmes lors de l’utilisation des API mobiles uniquement (par exemple, **HardwareButtons**). Pour atténuer ce risque, vous pouvez détecter de manière dynamique si votre application s’exécute sur Windows 10 Mobile avant d’appeler ces API. Suivez les instructions du billet de blog, [détection dynamique des fonctionnalités avec des contrats d’API](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/).

### <a name="including-dependencies-not-supported-by-uwp-apps"></a>Inclusion de dépendances non prises en charge par les applications UWP
Les applications plateforme Windows universelle (UWP) qui ne sont pas correctement créées avec Visual Studio et le kit de développement logiciel (SDK) UWP peuvent avoir des dépendances avec les composants du système d’exploitation qui ne sont pas disponibles pour les applications ARM exécutées sur un système ARM64. Voici quelques exemples de ces dépendances :

- Certaines parties de la .NET Framework sont censées être disponibles.
- Référencement de composants .NET tiers qui ne sont pas compatibles avec UWP.

Ces problèmes peuvent être résolus en supprimant les dépendances non disponibles et en reconstruisant l’application à l’aide des dernières versions de Microsoft Visual Studio et du kit de développement logiciel (SDK) UWP. en dernier recours, en supprimant l’application ARM du Microsoft Store, afin que la version x86 de l’application (si disponible) soit téléchargée sur les ordinateurs des utilisateurs.

Pour plus d’informations sur les API .NET disponibles pour les applications UWP, consultez [.net pour les applications UWP](/dotnet/api/index?view=dotnet-uwp-10.0) .

### <a name="compiling-an-app-with-an-older-version-of-visual-studio-and-sdk"></a>Compilation d’une application avec une version antérieure de Visual Studio et du kit de développement logiciel (SDK)
Si vous rencontrez des problèmes, veillez à utiliser les dernières versions de Microsoft Visual Studio et le SDK Windows pour compiler votre application. Les applications compilées avec une version antérieure de Visual Studio et le kit de développement logiciel (SDK) peuvent avoir des problèmes qui ont été résolus dans les versions ultérieures.

## <a name="debugging"></a>Débogage
Vous pouvez utiliser des outils existants pour développer des applications pour la plateforme ARM. Voici quelques ressources utiles.

- Visual Studio 15,5 Preview 1 et versions ultérieures prennent en charge l’exécution d’applications ARM32 à l’aide du mode d’authentification universelle. Cela amorce automatiquement les outils de débogage à distance nécessaires.
- Consultez [débogage sur ARM64](/windows-hardware/drivers/debugger/debugging-arm64) pour en savoir plus sur les outils et les stratégies de débogage sur ARM.