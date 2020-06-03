---
Description: Découvrez les réponses à certaines questions de base sur Windows 10X.
title: FAQ développeur Windows 10X
ms.topic: article
ms.date: 06/02/2020
ms.localizationpriority: medium
ms.author: quradic
author: QuinnRadich
ms.openlocfilehash: 3ba14e33c098d3515522a9a5907065751fafba87
ms.sourcegitcommit: 13bda6040988461a61b1b5561fde2f7a54835ccd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84318235"
---
# <a name="windows-10x-developer-faq"></a>FAQ développeur Windows 10X

> [!IMPORTANT]
> Nous avons récemment annoncé la modification de la définition de priorités pour Windows 10 et Windows 10X.
> Ces annonces incluent les modifications apportées aux priorités de facteur de forme Windows 10X. [Apprenez-en davantage ici](https://blogs.windows.com/windowsexperience/2020/05/04/accelerating-innovation-in-windows-10-to-meet-customers-where-they-are/).

Windows 10X est une gamme de produits de la famille Windows optimisée pour une utilisation sur des appareils à deux écrans. En tant que développeur, vous pouvez atteindre un public plus large en optimisant votre application pour Windows 10X, en tirant parti des nouvelles fonctionnalités spécifiques à un public et un public à deux écrans tout en bénéficiant de la même gamme de fonctionnalités Windows 10 et d’un support de bureau riche. [Nous avons annoncé Windows 10 à la fin du 2019](https://blogs.windows.com/windowsexperience/2019/10/02/introducing-windows-10x-enabling-dual-screen-pcs-in-2020/#6qxkItE2XMPu24uw.97), et nous sommes impatients de le publier au plus tard le 2020.

![Appareils exécutant Windows 10x](images/windows-10x-devices.png)
 
*[Produit en version préliminaire affiché, écrans simulés et sujets à modification]*

Pour plus d’informations sur la création d’expériences à deux écrans et de Windows 10X, consultez les sessions virtuelles du [Microsoft 365 du jour de développement](https://developer.microsoft.com/microsoft-365/virtual-events)ou des documents pour développeurs à [deux écrans](https://docs.microsoft.com/dual-screen/). Pour obtenir des informations en un clin d’œil, vous trouverez ici les réponses à certaines questions que vous pouvez rencontrer.

### <a name="how-is-this-different-from-developing-for-windows-10"></a>Comment est-ce différent du développement pour Windows 10 ?

Pour la majorité des applications, ce n’est pas du tout différent. L’écriture d’applications Windows 10X est prise en charge via le kit de développement logiciel Windows 10. En tant qu’expression de Windows 10, Windows 10X prend en charge les API Windows Runtime (WinRT) et exécute des applications Win32 via un conteneur natif. Vous pouvez ensuite appeler de nouvelles API spécifiquement conçues pour les appareils à deux écrans à partir de vos applications UWP ou Win32 nouvelles ou existantes, ce qui leur permet d’accéder aux fonctionnalités et aux avantages de cette nouvelle plateforme.

### <a name="does-this-replace-desktop-windows-10"></a>Remplace-t-il le bureau Windows 10 ?

Non. Windows 10X sera commercialisé en parallèle aux versions de bureau de Windows 10. Les versions de bureau de Windows 10 continueront à fournir des améliorations et des améliorations à l’histoire des applications de bureau modernes. Windows 10X est une autre plateforme optimisée pour prendre en charge les plateformes à deux écrans.

### <a name="when-will-windows-10x-be-released"></a>Quand Windows va-t-il être publié ?

Windows 10X sera relâché pour accompagner la surface Neo et les autres appareils tiers à deux écrans à la fin du 2020.

### <a name="when-can-i-start-development-for-windows-10x"></a>Quand puis-je commencer le développement pour Windows 10X ?

Vous pouvez télécharger l' [émulateur Microsoft et l’image de l’émulateur Windows 10x](https://docs.microsoft.com/dual-screen/windows/get-dev-tools) dès aujourd’hui. Nous continuerons à améliorer cet émulateur et à l’ajouter en complément de la prise en charge d’autres appareils Windows 10X. Ces émulateurs, combinés à des versions préliminaires de la SDK Windows, vous permettent de développer pour Windows 10 fois avant que le premier appareil à deux écrans soit publié publiquement.

### <a name="will-my-universal-windows-platform-uwp-apps-run-on-windows-10x"></a>Mes applications plateforme Windows universelle (UWP) s’exécutent-elles sur Windows 10X ?

La plupart des applications UWP sont entièrement prises en charge sur Windows 10X, et fonctionnent sur les appareils qui exécutent Windows 10X sans aucune modification. Toutes les API WinRT sont prises en charge, tout comme la plupart des autres fonctionnalités auxquelles les applications UWP ont accès. À mesure que le développement de la version préliminaire continue, nous publierons la documentation détaillant d’autres fonctionnalités non prises en charge.

### <a name="will-my-win32-apps-run-on-windows-10x"></a>Mes applications Win32 s’exécutent-elles sur Windows 10X ?

Windows 10X offre une prise en charge native de l’exécution d’applications Win32 dans un environnement à relation contenant-contenu. La plupart des applications Win32 peuvent être exécutées et déboguées sur un appareil Windows 10X sans incident, et vous pouvez également utiliser les nouvelles API Win32 pour ajouter la prise en charge de deux écrans à votre application.

### <a name="are-there-any-features-of-my-app-that-wont-work-on-windows-10x"></a>Existe-t-il des fonctionnalités de mon application qui ne fonctionneront pas sur Windows 10X ?

Dans la mesure où Windows 10X poursuit son développement en version préliminaire, nous publierons la documentation mettant en évidence ses limitations spécifiques. Toutefois, l’environnement contenu utilisé pour exécuter des applications Win32 n’inclut pas le shell Windows et, par conséquent, les extensions de Shell et les fonctionnalités similaires ne sont pas prises en charge. De même, les appareils Windows 10X ne prennent pas en charge les API liées à certains paramètres système, tels que les options d’utilisation de l’alimentation.

### <a name="if-i-enhance-my-app-with-windows-10x-features-will-it-still-run-on-devices-running-desktop-windows-10"></a>Si j’améliore mon application avec des fonctionnalités Windows 10X, est-il toujours exécuté sur les appareils exécutant Desktop Windows 10 ?

Les applications conçues pour Windows 10X fonctionnent toujours sur les appareils qui exécutent la version de bureau de Windows 10, bien que ces nouvelles API Windows ne soient pas ajoutées aux versions de bureau de Windows 10 jusqu’à la prochaine mise à jour majeure de la version. Comme si vous développiez une application prise en charge sur plusieurs versions de postes de travail Windows 10, suivez les [meilleures pratiques de codage adaptatif](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code) pour vous assurer que votre application fonctionne correctement à la fois sur 10 et sur le bureau Windows 10. 
