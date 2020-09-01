---
title: Vue d’ensemble du développement Android sur Windows
description: Guide pour vous aider à commencer à développer pour Android sur Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Android sur Windows, xamarin. Android, REACT native, Cordova, ionique, PhoneGap, c++ Android Game, Windows Defender, Emulator
ms.date: 04/28/2020
ms.openlocfilehash: e215d9e08fcef7ddb1caae40bd8f3a83e183d197
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157703"
---
# <a name="overview-of-android-development-on-windows"></a>Vue d’ensemble du développement Android sur Windows

Il existe plusieurs chemins d’accès pour le développement d’une application d’appareil Android à l’aide du système d’exploitation Windows. Ces chemins d’accès se répartissent en trois types principaux : le **[développement Android natif](#native-android)**, **[le développement multiplateforme](#cross-platform)** et le **[développement de Jeux Android](#game-development)**. Cette vue d’ensemble vous permet de choisir le chemin de développement à suivre pour développer une application Android, puis de fournir les [étapes suivantes](#next-steps) pour vous aider à commencer à utiliser Windows pour développer avec :

- [Android natif](native-android.md)
- [Xamarin.Android](xamarin-android.md)
- [Xamarin.Forms](xamarin-forms.md)
- [React Native](react-native.md)
- [Cordova, ionique ou PhoneGap](pwa.md)
- [C/C++ pour le développement de jeux](native-android.md#use-c-or-c-for-android-game-development)

En outre, ce guide fournit des conseils sur l’utilisation de Windows pour :

- [Tester sur un appareil ou un émulateur Android](emulator.md)
- [Mettre à jour les paramètres Windows Defender pour améliorer les performances](defender-settings.md)
- [Développer des applications à deux écrans pour Android et obtenir le kit de développement logiciel (SDK) d’appareil surface Duo](/dual-screen/android/)

## <a name="native-android"></a>Android natif

Le [développement Android natif sur Windows](./native-android.md) signifie que votre application cible uniquement Android (et non des appareils iOS ou Windows). Vous pouvez utiliser [Android Studio](https://developer.android.com/studio/install#windows) ou [Visual Studio](https://visualstudio.microsoft.com/vs/android/) pour développer au sein de l’écosystème conçu spécifiquement pour le système d’exploitation Android. Les performances sont optimisées pour les appareils Android, l’apparence de l’interface utilisateur est cohérente avec les autres applications natives sur l’appareil, et toutes les fonctionnalités ou fonctionnalités de l’appareil de l’utilisateur sont en avance pour l’accès et l’utilisation. Le développement de votre application dans un format natif vous aidera à le faire juste, car elle suit tous les modèles d’interaction et les normes d’expérience utilisateur établies spécifiquement pour les appareils Android.

## <a name="cross-platform"></a>Système multiplateforme

Les infrastructures multiplateforme fournissent un code base unique qui peut (principalement) être partagé entre des appareils Android, iOS et Windows. L’utilisation d’une infrastructure multiplateforme peut aider votre application à conserver la même apparence et la même expérience sur les plateformes d’appareils, ainsi qu’à tirer parti du déploiement automatique des mises à jour et des correctifs. Au lieu d’avoir besoin de comprendre une variété de langages de code spécifiques aux appareils, l’application est développée dans un code base partagé, généralement dans un langage.

Bien que les infrastructures multiplateformes visent à se présenter aussi près que possible des applications natives, elles ne seront jamais intégrées en toute transparence comme une application développée en mode natif et peuvent être affectées par une vitesse réduite et des performances dégradées. En outre, les outils utilisés pour générer des applications multiplateformes peuvent ne pas avoir toutes les fonctionnalités offertes par chaque plateforme d’appareil, ce qui peut nécessiter des solutions de contournement.

Un code base est généralement constitué de code d' **interface**utilisateur, pour la création de l’interface utilisateur, comme des pages, des contrôles boutons, des étiquettes, des listes, etc. et du **code logique**, pour l’appel de services Web, l’accès à une base de données, l’appel de fonctionnalités matérielles et la gestion de l’État. En moyenne, 90% de ce peut être réutilisé, bien qu’il soit généralement nécessaire de personnaliser le code pour chaque plateforme d’appareil. Cette généralisation dépend en grande partie du type d’application que vous créez, mais fournit un peu de contexte qui devrait vous aider dans votre prise de décision.  

## <a name="choosing-a-cross-platform-framework"></a>Choix d’une infrastructure multiplateforme

[Xamarin native (Xamarin. Android)](xamarin-android.md)

- Code d’interface utilisateur : XML avec Android Designer et thème de matériau
- Code logique : C# ou F #
- Toujours en mesure d’exploiter certains éléments Android natifs, mais convient pour la réutilisation de la base de code pour d’autres plateformes (iOS, Windows).
- Seul le code logique est partagé entre les plateformes, et non le code d’interface utilisateur.
- Idéal pour les applications plus complexes avec une interface utilisateur spécifique à l’appareil.

[Xamarin Forms (Xamarin. Forms)](xamarin-forms.md)

- Code d’interface utilisateur : XAML et .NET (avec Visual Studio)
- Code de la logique : C #
- Partage environ 60 à 90% de la logique et du code d’interface utilisateur dans les applications d’appareils Android, iOS et Windows. 
- Utilise des contrôles utilisateur courants tels que Button, label, Entry, ListView, StackLayout, Calendar, TabbedPage, etc. Créer un bouton et des formulaires Xamarin déterminera comment appeler le bouton natif pour chaque plateforme à l’aide de la bibliothèque de liaison pour appeler du code Java ou SWIFT à partir de C#.
- Idéal pour les applications simples, telles que les applications internes ou métier, les prototypes ou les MVP. Toute application qui peut paraître quelque peu standard ou générique, à l’aide d’une interface utilisateur simple.

[React Native](react-native.md)

- Code d’interface utilisateur : JavaScript
- Code logique : JavaScript
- L’objectif de rereact Native n’est pas d’écrire le code une fois et de l’exécuter sur n’importe quelle plate-forme, plutôt que d’apprendre-une une seule fois (réaction) et Write-Anywhere.
- La Communauté a ajouté des outils tels que l’exposition et la création d’une application native REACT pour aider ceux qui souhaitent créer des applications sans utiliser Xcode ou Android Studio.
- Semblable à Xamarin (C#), REACT native (JavaScript) appelle des éléments d’interface utilisateur natifs (sans avoir à écrire Java/Kotlin ou SWIFT).

[Applications web progressives (PWA)](pwa.md)

- Code d’interface utilisateur : HTML, CSS, JavaScript
- Code logique : JavaScript
- Les PWA sont des applications Web créées avec des modèles standard pour leur permettre de tirer parti des fonctionnalités d’application Web et natives. Elles peuvent être générées sans Framework, mais quelques frameworks populaires à prendre en compte sont [ioniques](https://ionicframework.com/docs/intro) et [PhoneGap](https://phonegap.com/about/).
- PWA peut être installé sur un appareil (Android, iOS ou Windows) et peut travailler hors connexion grâce à l’incorporation d’un service-Worker.
- Les PWA peuvent être distribués et installés sans App Store en utilisant uniquement une URL Web. Les Microsoft Store et Google Play Store autorisent la liste de PWA, mais le magasin Apple ne le fait pas, bien qu’ils puissent toujours être installés sur n’importe quel appareil iOS exécutant 12,2 ou une version ultérieure.
- Pour en savoir plus, consultez cette [Présentation de PWA](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Introduction) sur MDN.

## <a name="game-development"></a>Développement de jeux

Le développement de jeux pour Android est souvent unique dans le développement d’une application Android standard, car les jeux utilisent généralement une logique de rendu personnalisée, souvent écrite en OpenGL ou Vulkan. Pour cette raison, et en raison des nombreuses bibliothèques C disponibles qui prennent en charge le développement de jeux, il est courant pour les développeurs d’utiliser [C/C++ avec Visual Studio](/cpp/cross-platform/?view=vs-2019), ainsi que le [Kit de développement natif Android (NDK)](/cpp/cross-platform/create-an-android-native-activity-app?view=vs-2019), pour créer des jeux pour Android. [Prise en main de C/C++ pour le développement de jeux](native-android.md#use-c-or-c-for-android-game-development).

Un autre chemin d’accès courant pour le développement de jeux pour Android est l’utilisation d’un moteur de jeu. De nombreux moteurs libres et open source sont disponibles, comme [Unity avec Visual Studio](/visualstudio/cross-platform/visual-studio-tools-for-unity?view=vs-2019), le [moteur inréel](https://docs.unrealengine.com/en-US/Platforms/Mobile/Android/GettingStarted/index.html), le [Xamarin](/xamarin/graphics-games/monogame/introduction/), [UrhoSharp avec Xamarin](/xamarin/graphics-games/urhosharp/introduction), [SkiaSharp avec Xamarin. Forms](/xamarin/xamarin-forms/user-interface/graphics/skiasharp/) CocoonJS, app Game Kit, fusion, Corona SDK, Cocos 2D et bien plus encore.

## <a name="next-steps"></a>Étapes suivantes

- [Prise en main du développement Android natif sur Windows](native-android.md)
- [Prise en main du développement pour Android à l’aide de Xamarin. Android](xamarin-android.md)
- [Prise en main du développement pour Android à l’aide de Xamarin. Forms](xamarin-forms.md)
- [Prise en main du développement pour Android avec REACT Native](react-native.md)
- [Prise en main du développement d’un PWA pour Android](pwa.md)
- [Développer des applications à deux écrans pour Android et obtenir le kit de développement logiciel (SDK) d’appareil surface Duo](/dual-screen/android/)
- [Ajouter des exclusions Windows Defender pour améliorer les performances](defender-settings.md)
- [Activer la prise en charge de la virtualisation pour améliorer les performances de l’émulateur](emulator.md#enable-virtualization-support)