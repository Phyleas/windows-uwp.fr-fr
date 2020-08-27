---
title: Dépannage des applications de bureau x86
description: Découvrez comment dépanner et résoudre les problèmes courants liés à une application de bureau x86 s’exécutant sur ARM64, y compris des informations sur les pilotes, les extensions de Shell et le débogage.
ms.date: 05/09/2018
ms.topic: article
keywords: Windows 10 s, toujours connecté, émulation x86 sur ARM, résolution des problèmes
ms.localizationpriority: medium
ms.openlocfilehash: 4dbb3c485d3f6ba3ba410e2a960162880b6f3660
ms.sourcegitcommit: eb725a47c700131f5975d737bd9d8a809e04943b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88970267"
---
# <a name="troubleshooting-x86-desktop-apps"></a>Dépannage des applications de bureau x86
>[!IMPORTANT]
> Le kit de développement logiciel (SDK) ARM64 est désormais disponible dans le cadre de Visual Studio 15,8 Preview 1. Nous vous recommandons de recompiler votre application en ARM64 pour que votre application s’exécute à une vitesse native complète. Pour plus d’informations, consultez le billet [de blog sur la prise en charge préliminaire de Visual Studio pour Windows 10 sur ARM Development](https://blogs.windows.com/buildingapps/2018/05/08/visual-studio-support-for-windows-10-on-arm-development/) .

Si une application de bureau x86 ne fonctionne pas comme elle le fait sur un ordinateur x86, voici quelques conseils pour vous aider à résoudre les problèmes.

|Problème|Solution|
|-----|--------|
| Votre application s’appuie sur un pilote qui n’est pas conçu pour ARM. | Recompilez votre pilote x86 sur ARM64. Consultez [création de pilotes ARM64 avec le kit WDK](https://docs.microsoft.com/windows-hardware/drivers/develop/building-arm64-drivers). |
| Votre application est uniquement disponible pour x64. | Si vous développez pour Microsoft Store, soumettez une version ARM de votre application. Pour plus d’informations, consultez [architectures de package d’application](/windows/msix/package/device-architecture). Si vous êtes un développeur Win32, nous vous recommandons de recompiler votre application en ARM64. Pour plus d’informations, consultez la version préliminaire [de la prise en charge de Visual Studio pour Windows 10 pour le développement ARM](https://blogs.windows.com/buildingapps/2018/05/08/visual-studio-support-for-windows-10-on-arm-development/). |
| Votre application utilise une version OpenGL ultérieure à 1,1 ou nécessite une accélération matérielle OpenGL. | Utilisez le mode DirectX de l’application, si elle est disponible. les applications x86 qui utilisent DirectX 9, DirectX 10, DirectX 11 et DirectX 12 fonctionnent sur ARM. Pour plus d’informations, consultez [graphiques et jeux DirectX](https://docs.microsoft.com/windows/desktop/directx). |
| Votre application x86 ne fonctionne pas comme prévu. | Essayez d’utiliser l’utilitaire de résolution des problèmes de compatibilité en suivant les instructions de l' [utilitaire de résolution des problèmes de compatibilité des programmes sur ARM](apps-on-arm-program-compat-troubleshooter.md). Pour connaître d’autres étapes de dépannage, consultez l’article [Dépannage des applications x86 sur ARM](apps-on-arm-troubleshooting-x86.md) . |

## <a name="best-practices-for-wow"></a>Meilleures pratiques pour WOW
Un problème courant se produit lorsqu’une application découvre qu’elle s’exécute sous WOW, puis suppose qu’elle est sur un système x64. Une fois cette supposition effectuée, l’application peut effectuer les opérations suivantes :

- Essayez d’installer la version x64 de elle-même, ce qui n’est pas pris en charge sur ARM.
- Recherchez d’autres logiciels dans la vue du Registre natif.
- Supposons qu’un .NET Framework 64 bits est disponible.

En règle générale, une application ne doit pas faire d’hypothèses sur le système hôte lorsqu’elle est déterminée pour s’exécuter sous WOW. Évitez d’interagir autant que possible avec les composants natifs du système d’exploitation.

Une application peut placer des clés de Registre sous la vue du Registre natif ou exécuter des fonctions en fonction de la présence de WOW. Le **IsWow64Process**  d’origine indique uniquement si l’application s’exécute sur un ordinateur x64. Les applications doivent maintenant utiliser [IsWow64Process2](https://docs.microsoft.com/windows/desktop/api/wow64apiset/nf-wow64apiset-iswow64process2) pour déterminer si elles s’exécutent sur un système avec prise en charge de wow. 

## <a name="drivers"></a>Pilotes 
Tous les pilotes en mode noyau, les pilotes [UMDF (user-mode Driver Framework)](https://docs.microsoft.com/windows-hardware/drivers/wdf/overview-of-the-umdf) et les pilotes d’impression doivent être compilés pour correspondre à l’architecture du système d’exploitation. Si une application x86 a un pilote, ce pilote doit être recompilé pour ARM64. L’application x86 peut s’exécuter de façon correcte sous émulation. Toutefois, son pilote doit être recompilé pour ARM64 et toute expérience d’application qui dépend du pilote n’est pas disponible. Pour plus d’informations sur la compilation de votre pilote pour ARM64, consultez [Building ARM64 drivers with the WDK](https://docs.microsoft.com/windows-hardware/drivers/develop/building-arm64-drivers).

## <a name="shell-extensions"></a>Extensions d'environnement 
Les applications qui essaient de raccorder des composants Windows ou de charger leurs dll dans des processus Windows devront recompiler ces dll pour qu’elles correspondent à l’architecture du système. par exemple, ARM64. En règle générale, ils sont utilisés par les éditeurs de méthode d’entrée (IME), les technologies d’assistance et les applications d’extension de Shell (par exemple, pour afficher les icônes de stockage cloud dans l’Explorateur ou un menu contextuel de clic droit). Pour savoir comment recompiler vos applications ou dll vers ARM64, consultez le billet [de blog sur la version préliminaire du support Visual Studio pour Windows 10 sur ARM](https://blogs.windows.com/buildingapps/2018/05/08/visual-studio-support-for-windows-10-on-arm-development/) . 

## <a name="debugging"></a>Débogage
Pour examiner plus en détail le comportement de votre application, consultez [débogage sur ARM](https://docs.microsoft.com/windows-hardware/drivers/debugger/debugging-arm64) pour en savoir plus sur les outils et les stratégies de débogage sur ARM.

## <a name="virtual-machines"></a>Virtual Machines
La plateforme de l’hyperviseur Windows n’est pas prise en charge sur la plateforme PC mobile Qualcomm Snapdragon 835. Par conséquent, l’exécution d’ordinateurs virtuels à l’aide d’Hyper-V ne fonctionnera pas. Nous continuons à faire des investissements dans ces technologies sur les chipsets Qualcomm à venir. 

## <a name="dynamic-code-generation"></a>Génération de code dynamique
Les applications de bureau x86 sont émulées sur ARM64 par le système générant des instructions ARM64 au moment de l’exécution. Cela signifie que si une application de bureau x86 empêche la génération ou la modification de code dynamique dans son processus, cette application ne peut pas être prise en charge pour s’exécuter en tant que x86 sur ARM64. 

Il s’agit d’une atténuation de la sécurité que certaines applications activent sur leur processus à l’aide de l’API [SetProcessMitigationPolicy](https://docs.microsoft.com/windows/desktop/api/processthreadsapi/nf-processthreadsapi-setprocessmitigationpolicy) avec l' `ProcessDynamicCodePolicy` indicateur. Pour s’exécuter correctement sur ARM64 en tant que processus x86, cette stratégie d’atténuation doit être désactivée. 
