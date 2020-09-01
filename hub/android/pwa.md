---
title: Approche de PWA du développement Android sur Windows
description: Commencez à développer des applications Android à l’aide de l’approche PWA sur Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Android sur Windows, PWA, Android, Cordova, ionique, PhoneGap, application Web hybride
ms.date: 04/28/2020
ms.openlocfilehash: 482fd02ed7b5d978d81ec52309006034f70b7e47
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163983"
---
# <a name="get-started-developing-a-pwa-or-hybrid-web-app-for-android"></a>Prise en main du développement d’une application Web PWA ou hybride pour Android

Ce guide vous aidera à créer une application Web hybride ou une application Web progressive (PWA) sur Windows à l’aide d’un seul code base HTML/CSS/JavaScript qui peut être utilisé sur le Web et sur des plateformes d’appareils (Android, iOS, Windows).

En utilisant les frameworks et les composants appropriés, les applications Web peuvent fonctionner sur un appareil Android de façon à ce que les utilisateurs aient une apparence très similaire à celle d’une application native.

## <a name="features-of-a-pwa-or-hybrid-web-app"></a>Fonctionnalités d’une application Web PWA ou hybride

Il existe deux principaux types d’applications Web qui peuvent être installés sur des appareils Android. La principale différence réside dans le fait que votre code d’application est incorporé dans un package d’application (hybride) ou hébergé sur un serveur Web (PWA).

- **Applications Web hybrides**: le code (html, js, CSS) est empaqueté dans un apk et peut être distribué via le Google Play Store. Le moteur d’affichage est isolé du navigateur Internet des utilisateurs, sans partage de session ou de cache.

- **Progressif Web Apps (PWA)**: le code (html, js, CSS) vit sur le Web et n’a pas besoin d’être empaqueté en tant que apk. Les ressources sont téléchargées et mises à jour en fonction des besoins à l’aide d’un Worker service. Le navigateur Chrome affiche et affiche votre application, mais apparaît comme étant natif et n’inclut pas la barre d’adresses de navigateur normale, etc. Vous pouvez partager du stockage, du cache et des sessions avec le navigateur. Cela revient à installer un raccourci vers le navigateur Chrome en mode spécial. PWA peut également être listé dans le Google Play Store à l’aide de l’activité Web approuvée.

PWA et les applications Web hybrides sont très similaires à celles d’une application Android Native :

- Peut être installé via l’App Store (Google Play Store et/ou Microsoft Store)
- Avoir accès aux fonctionnalités natives des appareils tels que l’appareil photo, le GPS, le Bluetooth, les notifications et la liste des contacts
- Travailler hors connexion (pas de connexion Internet)

PWA dispose également de quelques fonctionnalités uniques :

- Peut être installé sur l’écran d’accueil Android directement à partir du Web (sans App Store)
- Peut également être installé via le Google Play Store [à l’aide d’une activité Web approuvée](https://css-tricks.com/how-to-get-a-progressive-web-app-into-the-google-play-store/)
- Peut être découvert via une recherche Web ou partagé via un lien URL
- S’appuyer sur un [travail de service](https://developers.google.com/web/fundamentals/primers/service-workers) pour éviter d’avoir à empaqueter le code natif

Vous n’avez pas besoin d’une infrastructure pour créer une application hybride ou PWA, mais il existe quelques frameworks populaires qui seront couverts dans ce guide, y compris PhoneGap (avec Cordova) et ionique (avec Cordova ou condensateur utilisant l’angle ou la réaction).

## <a name="apache-cordova"></a>Apache Cordova

[Apache Cordova](https://cordova.apache.org/) est un Framework Open source qui peut simplifier la communication entre votre code JavaScript vivant dans une [WebView](https://developer.android.com/reference/android/webkit/WebView) native et la plateforme Android native à l’aide de [plug-ins](https://cordova.apache.org/plugins/?platforms=cordova-android). Ces plug-ins exposent des points de terminaison JavaScript qui peuvent être appelés à partir de votre code et utilisés pour appeler des API d’appareil Android natives. Voici quelques exemples de plug-ins Cordova : l’accès aux services d’appareils tels que l’état de la batterie, l’accès aux fichiers, les vibrations/sonneries, etc. Ces fonctionnalités ne sont généralement pas disponibles pour les navigateurs ou les applications Web.

Il existe deux distributions populaires de Cordova :

- [PhoneGap](https://phonegap.com/)

- [Ionic](https://ionicframework.com/)

## <a name="adobe-phonegap"></a>Adobe PhoneGap

[PhoneGap](https://phonegap.com/): Framework pris en charge par Adobe qui prend en charge Cordova avec des outils supplémentaires, comme une [ligne de commande](http://docs.phonegap.com/getting-started/1-install-phonegap/cli/), une [application de bureau](https://phonegap.com/products#desktop-app-section)et une [Build PhoneGap](https://build.phonegap.com/), un service qui vous permet de charger votre code sur un serveur Adobe qui créera des applications natives sans avoir besoin d’installer des kits de développement logiciel (SDK) natifs sur votre ordinateur local. Cela vous permet d’effectuer des opérations telles que la génération d’une application iOS à l’aide de votre ordinateur Windows.

### <a name="install-phonegap"></a>Installer PhoneGap

Pour commencer à créer une application Web PWA ou hybride avec PhoneGap, vous devez d’abord installer les outils suivants :

- Node.js pour interagir avec l’écosystème ionique. [Téléchargez NodeJS pour Windows](https://nodejs.org/en/) ou suivez le [Guide d’installation de NodeJS](../nodejs/setup-on-wsl2.md) à l’aide du sous-système Windows pour Linux (WSL). Vous pouvez envisager d’utiliser [node version Manager (NVM)](../nodejs/setup-on-wsl2.md#install-nvm-nodejs-and-npm) si vous prévoyez d’utiliser plusieurs projets et la version de NodeJS.

Installez PhoneGap en entrant ce qui suit dans la ligne de commande :

```bash
npm install -g phonegap
```

Pour créer un projet PhoneGap, suivez les étapes de la section [prise en main](https://phonegap.com/getstarted/). Visitez la section [fonctionnalités de PWA](http://stage.docs.phonegap.com/tutorials/stockpile/911-pwa-features/) de la documentation PhoneGap pour découvrir comment déplacer votre application d’un hybride à un PWA.  

## <a name="ionic"></a>Ionic

[Ionique](https://ionicframework.com/) est une infrastructure qui ajuste l’interface utilisateur (IU) de votre application pour qu’elle corresponde au langage de conception de chaque plateforme (Android, iOS, Windows). Ionique vous permet d’utiliser l' [angle](https://ionicframework.com/docs/developer-resources/guides/first-app-v4/intro) ou la [réaction](https://ionicframework.com/react).

> [!NOTE]
> Il existe une nouvelle version d’ionique qui utilise une alternative à Cordova, appelée [condensateur](https://capacitor.ionicframework.com/). Cette alternative utilise des conteneurs pour rendre votre application [plus conviviale](https://ionicframework.com/blog/announcing-capacitor-1-0/).

### <a name="get-started-with-ionic-by-installing-required-tools"></a>Prise en main d’ionique en installant les outils nécessaires

Pour commencer à créer une application Web PWA ou hybride avec ionique, vous devez d’abord installer les outils suivants :

- Node.js pour interagir avec l’écosystème ionique. [Téléchargez NodeJS pour Windows](https://nodejs.org/en/) ou suivez le [Guide d’installation de NodeJS](../nodejs/setup-on-wsl2.md) à l’aide du sous-système Windows pour Linux (WSL). Vous pouvez envisager d’utiliser [node version Manager (NVM)](../nodejs/setup-on-wsl2.md#install-nvm-nodejs-and-npm) si vous prévoyez d’utiliser plusieurs projets et la version de NodeJS.

- VS Code pour l’écriture de votre code. [Téléchargez vs code pour Windows](https://code.visualstudio.com/). Vous pouvez également installer l' [extension distante WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) si vous préférez générer votre application avec une ligne de commande Linux.

- Windows Terminal pour travailler avec votre interface de ligne de commande (CLI) par défaut. [Installez le terminal Windows à partir de Microsoft Store](https://www.microsoft.com/en-us/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab).

- Git pour le contrôle de version. [Téléchargez git](https://git-scm.com/downloads).

## <a name="create-a-new-project-with-ionic-cordova-and-angular"></a>Créer un nouveau projet avec Cordova ionique et angle

Installez ionique et Cordova en entrant ce qui suit dans la ligne de commande :

```bash
npm install -g @ionic/cli cordova
```

Créez une application angulaire ionique à l’aide du modèle d’application « Tabs » en entrant la commande suivante :

```bash
ionic start photo-gallery tabs
```

Accédez au dossier de l’application :

```bash
cd photo-gallery
```

Exécutez l’application dans votre navigateur Web :

```bash
ionic serve
```

Pour plus d’informations, consultez les [documents angulaires ionique Cordova](https://ionicframework.com/docs/developer-resources/guides/first-app-v4/intro). Visitez la section [création d’une application angulaire a PWA](https://ionicframework.com/docs/angular/pwa) des documents ioniques pour savoir comment faire passer votre application d’un hybride à un PWA.

## <a name="create-a-new-project-with-ionic-capacitor-and-angular"></a>Créer un nouveau projet avec un condensateur ionique et un angle

Installez ionique et Cordova-res en entrant ce qui suit dans la ligne de commande :

```bash
npm install -g @ionic/cli native-run cordova-res
```

Créez une application angulaire ionique à l’aide du modèle d’application « Tabs » et ajoutez un condensateur en entrant la commande suivante :

```bash
ionic start photo-gallery tabs --type=angular --capacitor
```

Accédez au dossier de l’application :

```bash
cd photo-gallery
```

Ajouter des composants pour rendre l’application PWA :

```bash
npm install @ionic/pwa-elements
```

Importez @ionic/pwa-elements en ajoutant le code suivant à votre `src/main.ts` fichier :

```typescript
import { defineCustomElements } from '@ionic/pwa-elements/loader';

// Call the element loader after the platform has been bootstrapped
defineCustomElements(window);
```

Exécutez l’application dans votre navigateur Web :

```bash
ionic serve
```

Pour plus d’informations, consultez les [documents angulaires du condensateur ionique](https://ionicframework.com/docs/angular/your-first-app). Visitez la section [création d’une application angulaire a PWA](https://ionicframework.com/docs/angular/pwa) des documents ioniques pour savoir comment faire passer votre application d’un hybride à un PWA.  

## <a name="create-a-new-project-with-ionic-and-react"></a>Créer un nouveau projet avec ionique et REACT

Installez l’interface de ligne de commande ionique en entrant ce qui suit dans la ligne de commande :

```bash
npm install -g @ionic/cli
```

Créez un nouveau projet avec REACT en entrant la commande suivante :

```bash
ionic start myApp blank --type=react
```

Accédez au dossier de l’application :

```bash
cd myApp
```

Exécutez l’application dans votre navigateur Web :

```bash
ionic serve
```

Pour plus d’informations, consultez les [documents ionique REACT](https://ionicframework.com/docs/react/quickstart). Visitez la section [création de votre application REACT a PWA](https://ionicframework.com/docs/react/pwa) des documents ioniques pour savoir comment faire passer votre application d’un hybride à un PWA.

## <a name="test-your-ionic-app-on-a-device-or-emulator"></a>Tester votre application ionique sur un appareil ou un émulateur

Pour tester votre application ionique sur un appareil Android, connectez votre appareil ([Assurez-vous qu’il est activé pour le développement](emulator.md#enable-your-device-for-development)), puis, dans la ligne de commande, entrez :

```bash
ionic cordova run android
```

Pour tester votre application ionique sur un émulateur d’appareil Android, vous devez :

1. [Installez les composants requis--Java Development Kit (JDK), Gradle et le Android SDK](https://cordova.apache.org/docs/en/latest/guide/platforms/android/#installing-the-requirements).

2. [Créer un appareil virtuel Android (AVD)](https://developer.android.com/studio/run/managing-avds.html).

3. Entrez la commande pour ionique pour générer et déployer votre application sur l’émulateur : `ionic cordova emulate [<platform>] [options]` . Dans ce cas, la commande doit être :

```bash
ionic cordova emulate android --list
```

Pour plus d’informations, consultez l' [émulateur Cordova](https://ionicframework.com/docs/cli/commands/cordova-emulate) dans les documents ioniques.

## <a name="additional-resources"></a>Ressources supplémentaires

- [Développer des applications à deux écrans pour Android et obtenir le kit de développement logiciel (SDK) d’appareil surface Duo](/dual-screen/android/)

- [Ajouter des exclusions Windows Defender pour améliorer les performances](defender-settings.md)

- [Activer la prise en charge de la virtualisation pour améliorer les performances de l’émulateur](emulator.md#enable-virtualization-support)