---
title: Qu’est-ce qu’une application de plateforme Windows universelle (UWP) ?
description: Découvrez les applications de plateforme Windows universelle (UWP) qui peuvent s’exécuter sur un large éventail d’appareils exécutant Windows 10.
ms.assetid: 59849197-B5C7-493C-8581-ADD6F5F8800B
ms.date: 09/15/2020
ms.topic: article
ms.custom: contperf-fy21q1
keywords: windows 10, uwp, universelle
ms.localizationpriority: medium
ms.openlocfilehash: e0d6991f261a9ee207a7e36f3dc5ad6a145f3b12
ms.sourcegitcommit: afc4ff2c89f148d32073ab1cc42063ccdc573a8c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104670"
---
# <a name="whats-a-universal-windows-platform-uwp-app"></a>Qu’est-ce qu’une application de plateforme Windows universelle (UWP) ?

UWP est l’un des nombreux moyens de créer des applications clientes pour Windows. Les applications UWP utilisent des API WinRT pour fournir une interface utilisateur puissante et des fonctionnalités asynchrones avancées qui sont idéales pour les appareils connectés à Internet.

Pour télécharger les outils dont vous avez besoin afin de commencer à créer des applications UWP, consultez [Se préparer](/windows/apps/get-set-up), puis [Développer votre première application](your-first-app.md).


## <a name="where-does-uwp-fit-in-the-microsoft-development-story"></a>Quelle est la place de UWP dans le développement Microsoft ?

UWP est l’un des choix possibles pour créer des applications qui s’exécuteront sur des appareils Windows 10, et peut être combiné avec d’autres plateformes. Les applications UWP peuvent utiliser des API Win32 et des classes .NET (consultez [Jeux d’API pour les applications UWP](/previous-versions/mt186421(v=vs.85)), [DLL pour les applications UWP](/previous-versions/mt186422(v=vs.85)) et [.NET pour les applications UWP](/dotnet/api/index?view=dotnet-uwp-10.0)).

Le développement Microsoft continue à évoluer et, avec des initiatives telles que [WinUI](/windows/apps/winui/), [MSIX](/windows/msix/) et [Project Reunion](https://github.com/microsoft/ProjectReunion), UWP est un outil puissant pour la création d’applications clientes.


## <a name="features-of-a-uwp-app"></a>Caractéristiques d’une application UWP

Description :

- Sécurisée : les applications UWP déclarent à quelles ressources d’appareil et à quelles données elles ont accès. L’utilisateur doit autoriser cet accès.
- Possibilité d’utiliser une API commune sur tous les appareils qui exécutent Windows 10.
- Possibilité d’utiliser des fonctionnalités spécifiques des appareils et d’adapter l’interface utilisateur à plusieurs tailles d’écran, résolutions et valeurs PPP.
- Disponible à partir du Microsoft Store sur tous les appareils (ou uniquement ceux que vous spécifiez) qui s’exécutent sur Windows 10. Le Microsoft Store offre plusieurs moyens de gagner de l’argent sur votre application.
- Possibilité d’installation et de désinstallation sans risque pour l’ordinateur et sans « détérioration» de ce dernier.
- Attrayante : possibilité d’utiliser des vignettes dynamiques, des notifications push et des activités des utilisateurs qui interagissent avec la Chronologie de Windows et la fonctionnalité Pick Up Where I Left Off de Cortana pour fidéliser les utilisateurs.
- Programmable en C#, C++, Visual Basic et JavaScript. Pour l’interface utilisateur, vous avez la possibilité d’utiliser WinUI, XAML, HTML ou DirectX.

Examinons cela plus en détail.

### <a name="secure"></a>Sécurisé

Les applications UWP déclarent dans leur manifeste les fonctionnalités de l’appareil dont elles ont besoin, comme l’accès au microphone, l’emplacement, la webcam, les périphériques USB, les fichiers et ainsi de suite. L’utilisateur doit confirmer et autoriser cet accès pour que la fonctionnalité soit accordée à l’application.

### <a name="a-common-api-surface-across-all-devices"></a>Surface d’API commune à tous les appareils

Windows 10 introduit la plateforme Windows universelle (UWP), qui fournit une plateforme d’application commune sur chaque appareil exécutant Windows 10. Les API au cœur d’UWP sont les mêmes pour toutes les familles d’appareils Windows. Si votre application utilise uniquement les API principales, elle s’exécutera sur n’importe quel appareil Windows 10, que vous cibliez un PC de bureau, une Xbox, un casque de réalité mixte, etc.

Une application UWP ou un composant Windows Runtime écrits en C++/WinRT ou C++/CX ont accès aux API Win32 faisant partie de l’UWP. Ces API Win32 sont implémentées par tous les appareils Windows 10.

### <a name="extension-sdks-expose-the-unique-capabilities-of-specific-device-types"></a>Les kits de développement logiciel (SDK) d’extension exposent les fonctionnalités uniques des types spécifiques d’appareils

Si vous ciblez des API universelles, votre application peut être lancée sur tous les appareils exécutant Windows 10. Si vous souhaitez que votre application UWP tire parti d’API propres aux appareils, c’est possible également.

Les SDK d’extension vous permettent d’appeler des API spécifiques à différents appareils. Par exemple, si votre application UWP cible un appareil IoT, vous pouvez ajouter le SDK d’extension IoT à votre projet pour cibler des fonctionnalités spécifiques des appareils IoT. Pour plus d’informations sur l’ajout de SDK d’extension, consultez la section **Kits de développement logiciel (SDK) d’extension** dans [Programmation avec les kits SDK d’extension](/uwp/extension-sdks/device-families-overvieww#extension-sdks).

Vous pouvez écrire votre application afin qu’elle s’exécute uniquement sur un type particulier d’appareil, puis limiter sa distribution à partir du Microsoft Store simplement à ce type d’appareil. Ou bien, vous pouvez tester de manière conditionnelle la présence d’une API lors de l’exécution et adapter le comportement de votre application en conséquence. Pour plus d’informations, consultez la section **Écriture de code** dans [Programmation avec les kits SDK d’extension](/uwp/extension-sdks/device-families-overview#writing-code).<br>

La vidéo suivante présente les familles d’appareils et le code adaptatif :
<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Introduction-to-UWP-and-Device-Families/player" width="640" height="360" allowFullScreen frameBorder="0"></iframe>

### <a name="adaptive-controls-and-input"></a>Contrôles adaptatifs et entrée

Les éléments d’interface utilisateur s’adaptent à la taille et à la résolution de l’écran de l’application en cours d’exécution en ajustant leur disposition et leur mise à l’échelle. Les applications UWP fonctionnent bien avec plusieurs types d’entrée, par exemple, le clavier, la souris, les impulsions tactiles, le stylet et les manettes Xbox One. Si vous avez besoin d’adapter davantage votre interface utilisateur à une taille d’écran ou à un appareil spécifique, de nouveaux panneaux de disposition sont là pour vous aider à adapter votre interface utilisateur aux appareils et aux facteurs de forme sur lesquels votre application s’exécute.

![Appareils avec Windows](images/1894834-hig-device-primer-01-500.png)

Windows vous aide à cibler votre interface utilisateur sur plusieurs appareils avec les fonctionnalités suivantes :

- Les contrôles universels et les panneaux de disposition vous aident à optimiser votre interface utilisateur pour la résolution d’écran de l’appareil. Par exemple, des contrôles tels que des boutons et des curseurs s’adaptent automatiquement à la taille d’écran de l’appareil et à la densité PPP. Les panneaux de disposition aident à ajuster la disposition du contenu en fonction de la taille de l’écran. La mise à l’échelle adaptative s’ajuste en fonction des différentes résolutions des appareils.
- La gestion commune des entrées vous permet de recevoir des entrées par le biais d’impulsions tactiles, d’un stylet, d’une souris, d’un clavier ou d’un contrôleur tel qu’une manette Microsoft Xbox.
- Les outils qui vous permettent de concevoir une interface utilisateur peuvent s’adapter à différentes résolutions d’écran.

Certains aspects de l’interface utilisateur de votre application s’adaptent automatiquement aux différents appareils. La conception de l’expérience utilisateur de votre application peut cependant nécessiter une adaptation en fonction de l’appareil sur lequel l’application s’exécute. Par exemple, une application de photos doit adapter son interface utilisateur quand elle s’exécute sur un appareil portatif de petite taille, afin d’être utilisable d’une seule main. Quand une application de photographie s’exécute sur un ordinateur de bureau, l’interface utilisateur doit s’adapter pour tirer parti de l’espace d’écran supplémentaire.

### <a name="theres-one-store-for-all-devices"></a>Il existe un seul magasin pour tous les appareils

Un magasin d’applications unifié rend votre application disponible sur les appareils Windows 10 tels que PC, tablette, Xbox, HoloLens, Surface Hub, et Internet des objets (IoT). Vous pouvez soumettre votre application au Store et la rendre disponible sur tous les types d’appareils ou uniquement sur ceux que vous choisissez. Vous soumettez et vous gérez toutes vos applications pour appareils Windows au même endroit. Vous avez une application de bureau C++ que vous souhaitez moderniser grâce aux fonctionnalités UWP et commercialiser dans le Microsoft Store ? Cela ne pose aucun problème.

À des fins de télémétrie et d’analyse détaillées, les applications UWP s’intègrent avec [Application Insights](https://azure.microsoft.com/services/application-insights/) : un outil crucial pour comprendre vos utilisateurs et améliorer vos applications.

Les applications UWP peuvent être empaquetées avec [MSIX](/windows/msix/) et distribuées via le Microsoft Store ou par un autre moyen. MSIX permet la mise à jour des applications, quel que soit leur mode de distribution. Consultez [Mettre à jour des packages d’applications publiés hors Store à partir de votre code](/windows/msix/non-store-developer-updates).

### <a name="monetize-your-app"></a>Monétiser votre application

Vous pouvez choisir la manière de monétiser votre application. Il existe plusieurs moyens de gagner de l’argent avec votre application. Il vous suffit simplement de choisir le procédé qui vous convient le mieux, par exemple :

- L’option la plus simple est de faire payer le téléchargement. Il vous suffit de fixer le prix.
- Les périodes d’essai permettent à l’utilisateur d’essayer votre application avant de l’acheter. Il peut ainsi la découvrir et l’adopter plus facilement que si vous aviez choisi l’option plus classique du « freemium ».
- Prix de vente à prix réduit pour inciter les utilisateurs.
- Achats dans l’application.


### <a name="deliver-relevant-real-time-info-to-your-users-to-keep-them-coming-back"></a>Possibilité de fournir des informations pertinentes en temps réel à vos utilisateurs afin de les fidéliser

Il existe plusieurs façons de fidéliser les utilisateurs avec votre application UWP :

- Les vignettes dynamiques et l’écran de verrouillage permettent de voir d’un seul coup d’œil, à partir de l’application, des informations fournies au moment opportun et adaptées au contexte.
- Les notifications Push permettent d’alerter vos utilisateurs en temps réel.
- Les activités de l’utilisateur permettent aux utilisateurs de reprendre là où ils s’étaient arrêtés dans l’application, même sur différents appareils.
- Le Centre de maintenance organise les notifications à partir de votre application.
- L’exécution en arrière-plan et les déclencheurs activent votre application au moment où l’utilisateur en a besoin.
- Votre application peut utiliser des appareils vocaux et Bluetooth LE pour permettre aux utilisateurs d’interagir avec leur environnement.
- Vous pouvez intégrer Cortana pour ajouter la fonctionnalité de commande vocale à votre application.

##  <a name="use-a-language-you-already-know"></a>Utilisez un langage que vous connaissez déjà

Les applications UWP peuvent utiliser Windows Runtime, l’API native fournie par le système d’exploitation. Cette API est implémentée en C++ et est prise en charge dans C#, Visual Basic, C++ et JavaScript. Parmi les options d’écriture d’applications UWP on trouve :

- XAML UI et C#, VB ou C++
- DirectX UI et C++
- JavaScript et HTML
- WinUI

## <a name="links-to-help-you-get-going"></a>Liens pour vous aider à commencer

### <a name="get-set-up"></a>Se préparer

Consultez [Se préparer](/windows/apps/get-started/get-set-up) pour télécharger les outils dont vous avez besoin afin de commencer à créer des applications, puis à [écrire votre première application](your-first-app.md).

### <a name="design-your-app"></a>Concevoir votre application

Le nom du système de conception Microsoft est Fluent. Le système Fluent Design est un ensemble de fonctionnalités UWP combinées avec les meilleures pratiques pour créer des applications qui s’exécutent parfaitement sur tous les types d’appareils fonctionnant sous Windows. Les expériences Fluent s’adaptent parfaitement aux différents appareils (tablettes ou ordinateurs portables, PC ou téléviseurs, ainsi que sur des appareils de réalité virtuelle). Consultez la section [Système Fluent Design pour les applications UWP](/windows/uwp/design/fluent-design-system) pour obtenir une présentation du système Fluent Design.

Une bonne [conception](http://design.windows.com/) est le processus consistant à décider comment les utilisateurs doivent interagir avec votre application, ainsi que la manière dont cette dernière se présentera et fonctionnera. L’expérience utilisateur jouant un rôle considérable dans la satisfaction que procurera votre application, ne négligez pas cette étape. La rubrique [Notions de base de la conception](https://developer.microsoft.com/windows/apps/design) présente la conception d’une application Windows universelle. Pour plus d’informations sur la conception d’applications UWP susceptibles de plaire aux utilisateurs, consultez [Présentation des applications de la plateforme Windows universelle (UWP) pour les concepteurs](../design/basics/design-and-ui-intro.md). Avant de commencer à coder, consultez [Notions fondamentales sur les appareils](../design/devices/index.md) pour réfléchir à l’expérience d’interaction de l’utilisation de votre application sur les différents facteurs de forme que vous voulez cibler.

En plus de l’interaction sur différents appareils, [planifiez votre application](./plan-your-app.md) pour bénéficier des avantages liés à l’utilisation de plusieurs appareils. Par exemple :

- Concevez votre workflow d’après les [Informations de base relatives à la conception de la navigation pour les applications UWP](../design/basics/navigation-basics.md) afin d’adapter votre application aux appareils mobiles, petit et grand écran. [Concevez votre interface utilisateur](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md) pour l’adapter à différentes tailles et résolutions d’écran.

- Songez à la manière de prendre en charge plusieurs types d’entrées. Pour savoir comment les utilisateurs peuvent interagir avec votre application via [Cortana](/cortana/skills/), la fonction [Voix](../design/input/speech-interactions.md), des [interactions tactiles](../design/input/touch-interactions.md), du [clavier tactile](../design/input/keyboard-interactions.md) et bien plus encore, voir les [Recommandations en matière d’interactions](../design/layout/index.md).  Pour des expériences d’interaction plus classiques, voir les [Recommandations en matière de texte et de saisie de texte](../design/controls-and-patterns/text-controls.md).

### <a name="add-services"></a>Ajouter des services

- Utilisez les [services cloud](https://azure.microsoft.com/documentation/services/cloud-services) pour synchroniser les données entre les appareils.
- Découvrez comment vous [connecter aux services web](/previous-versions/windows/apps/hh761504(v=win.10)) pour renforcer l’expérience de votre application.
- Incluez des [notifications Push](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md) et des [achats dans l’application](../monetize/enable-in-app-product-purchases.md) dans votre planification. Ces fonctionnalités doivent opérer sur tous les appareils.

### <a name="submit-your-app-to-the-store"></a>Soumettre votre application au Store

L’[Espace partenaires](https://partner.microsoft.com/dashboard) vous permet de gérer et de soumettre toutes vos applications pour les appareils Windows à partir d’un seul et même emplacement. Consultez [Publier des applications et jeux Windows](../publish/index.md) afin de savoir comment soumettre vos applications pour la publication dans le Microsoft Store.

De nouvelles fonctionnalités simplifient les processus tout en vous offrant un contrôle accru. Le nouveau tableau de bord vous propose également des [rapports d’analyse détaillés](../publish/analytics.md), des [informations sur les revenus](/partner-center/payout-statement), des méthodes pour [promouvoir votre application et susciter l’intérêt de vos clients](../publish/attract-customers-and-promote-your-apps.md), et bien davantage.

Pour obtenir une introduction plus générale, voir [Introduction au développement d’applications Windows pour appareils Windows 10](/archive/msdn-magazine/2015/may/windows-10-an-introduction-to-building-windows-apps-for-windows-10-devices)

### <a name="more-advanced-topics"></a>Rubriques plus avancées

- Découvrez comment utiliser les [activités de l’utilisateur](https://blogs.windows.com/buildingapps/2017/12/19/application-engagement-windows-timeline-user-activities/#tHuZ6tLPtCXqYKvw.97) afin que l’activité utilisateur dans votre application apparaisse dans la Chronologie de Windows et dans la fonctionnalité Pick Up Where I Left Off de Cortana.
- Découvrez comment utiliser les [vignettes, badges et notifications pour les applications UWP](../design/shell/tiles-and-notifications/index.md).
- Pour obtenir la liste complète des API Win32 disponibles pour les applications UWP, consultez [Ensembles d’API pour les applications UWP](/previous-versions/mt186421(v=vs.85)) et [DLL pour les applications UWP](/previous-versions/mt186422(v=vs.85)).
- Voir [Universal Windows apps in .NET (Applications Windows universelles dans .NET)](https://devblogs.microsoft.com/dotnet/universal-windows-apps-in-net/) pour obtenir une vue d’ensemble de l’écriture des applications UWP .NET.
- Pour obtenir la liste des types .NET que vous pouvez utiliser dans une application UWP, voir [.NET pour les applications UWP](/dotnet/api/index?view=dotnet-uwp-10.0)
- [Compilation d’applications avec .NET Native](/dotnet/framework/net-native/)
- Découvrez comment ajouter des expériences modernes pour les utilisateurs Windows 10 à votre application de bureau et comment les distribuer dans le Microsoft Store avec le [Pont du bureau](https://developer.microsoft.com/windows/bridges/desktop).

## <a name="how-the-universal-windows-platform-relates-to-windows-runtime-apis"></a>Liens entre la plateforme Windows universelle et les API Windows Runtime
Si vous créez une application de plateforme Windows universelle (UWP), vous pouvez obtenir un excellent taux d’utilisation et plus de facilité en traitant les termes « Plateforme Windows universelle (UWP) » et « Windows Runtime (WinRT) » plus ou moins comme des synonymes. Mais il *est* possible de chercher plus loin et de définir simplement la différence entre ces idées. Si vous voulez en savoir plus, cette dernière section s’adresse à vous.

Windows Runtime et les API WinRT sont une évolution des API Windows. À l’origine, Windows a été programmé par le biais d’API Win32 plates de style C. Puis se sont ajoutées les API COM (avec notamment [DirectX](/windows/desktop/directx)). Windows Forms, WPF, .NET et les langages gérés ont apporté leur propre style d’écriture des applications Windows et leur propre version de la technologie d’API. Windows Runtime est en fait l’étape suivante de COM. Dans la couche d’interface binaire (ABI) de l’application, ses racines COM deviennent visibles. Mais Windows Runtime a été conçu pour être appelé à partir d’une large gamme de langages de programmation. Et d’une manière très naturelle pour chacun de ces langages. À cette fin, Windows Runtime est rendu accessible par ce que l’on appelle des projections de langage. Il existe une projection de langage Windows Runtime dans C#, Visual Basic, le langage C++ standard, JavaScript, etc. En outre, une fois l’empaquetage correctement effectué (voir [Pont du bureau](/windows/msix/desktop/source-code-overview)), vous pouvez appeler les API WinRT à partir d’une application créée dans l’une des vastes gammes de modèles d’application : Win32, .NET, WinForms et WPF.

Et, bien sûr, vous pouvez appeler les API WinRT à partir de votre application UWP. UWP est un modèle d’application reposant sur Windows Runtime. Techniquement, le modèle d’application UWP repose sur [CoreApplication](/uwp/api/windows.applicationmodel.core.coreapplication), bien que ce détail puisse vous être caché, selon le langage de programmation que vous avez choisi. Comme indiqué dans cette section, en termes de proposition de valeur, la plateforme Windows universelle est parfaite pour l’écriture d’un fichier binaire unique qui peut, si vous le souhaitez, être publié dans le Microsoft Store et s’exécuter sur n’importe lequel des facteurs de forme d’une vaste gamme d’appareils. La portée de l’appareil de votre application UWP varie en fonction du sous-ensemble d’API Windows Runtime pour lesquelles vous limitez les appels de votre application, ou que vous appelez de manière conditionnelle.

Nous espérons que cette section a expliqué clairement la différence entre la technologie sous-jacente des API Windows Runtime d’une part, et le mécanisme et la valeur métier de la plateforme Windows universelle d’autre part.