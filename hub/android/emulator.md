---
title: Exécution de l’appareil ou de l’émulateur Android à partir de Windows
description: Testez votre application sur un appareil ou un émulateur Android à partir de Windows et activez la virtualisation avec Hyper-v et la plateforme d’hyperviseur Windows (WHPX).
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Android, Windows, émulateur, appareil virtuel, configuration de l’appareil, activer l’appareil, développeur, configuration, virtualisation, Visual Studio, Hyper-v, Intel, haxm, AMD, plateforme d’hyperviseur Windows, WHPX
ms.date: 04/28/2020
ms.openlocfilehash: c651661d573695902368ffa595ce5d3014791a9a
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255153"
---
# <a name="test-on-an-android-device-or-emulator"></a>Tester sur un appareil ou un émulateur Android

Il existe plusieurs façons de tester et de déboguer votre application Android à l’aide d’un appareil ou d’un émulateur réel sur votre ordinateur Windows. Nous avons présenté quelques recommandations dans ce guide.

## <a name="run-on-a-real-android-device"></a>Exécuter sur un appareil Android réel

Pour exécuter votre application sur un appareil Android réel, vous devez d’abord activer votre appareil Android pour le développement. Les options de développeur sur Android ont été masquées par défaut, car la version 4,2 et leur activation peuvent varier en fonction de la version d’Android.

### <a name="enable-your-device-for-development"></a>Activer votre appareil pour le développement

Pour un appareil exécutant une version récente d’Android 9.0 + :

1. Connectez votre appareil à votre ordinateur de développement Windows à l’aide d’un câble USB. Vous pouvez recevoir une notification pour installer un pilote USB.
2. Ouvrez l’écran des **paramètres** sur votre appareil Android.
3. Sélectionnez **à propos de téléphone**.
4. Faites défiler vers le bas et appuyez sur **numéro de build** sept fois, jusqu’à ce **que vous soyez maintenant développeur !** est visible.
5. Revenez à l’écran précédent, sélectionnez **système**.
6. Sélectionnez **avancé**, faites défiler vers le bas, puis appuyez sur **Options du développeur**.
7. Dans la fenêtre **Options du développeur** , faites défiler la liste pour rechercher et activer le **débogage USB**.

Pour un appareil exécutant une version antérieure d’Android, consultez [configurer un appareil pour le développement](https://docs.microsoft.com/xamarin/android/get-started/installation/set-up-device-for-development).

### <a name="run-your-app-on-the-device"></a>Exécuter votre application sur l’appareil

1. Dans la barre d’outils Android Studio, sélectionnez votre application dans le menu déroulant **exécuter les configurations** .

    ![Menu Android Studio exécuter la configuration](../images/android-run-config-menu.png)

2. Dans le menu déroulant **périphérique cible** , sélectionnez l’appareil sur lequel vous souhaitez exécuter votre application.

    ![Menu Android Studio périphérique cible](../images/android-target-device-menu.png)

3. Sélectionnez Exécuter ▷. Cette opération lance l’application sur votre appareil connecté.

## <a name="run-your-app-on-a-virtual-android-device-using-an-emulator"></a>Exécuter votre application sur un appareil Android virtuel à l’aide d’un émulateur

La première chose à savoir sur l’exécution d’un émulateur Android sur votre ordinateur Windows est que, quel que soit votre IDE (Android Studio, Visual Studio, etc.), les performances de l’émulateur sont considérablement améliorées grâce à l’activation de la prise en charge de la virtualisation.

### <a name="enable-virtualization-support"></a>Activer la prise en charge de la virtualisation

Avant de créer un appareil virtuel avec l’émulateur Android, il est recommandé d’activer la virtualisation en activant les fonctionnalités Hyper-V et Windows Hypervisor Platform (WHPX). Cela permettra au processeur de votre ordinateur d’améliorer considérablement la vitesse d’exécution de l’émulateur.

> Pour exécuter la plateforme Hyper-V et Windows hyperviseur, votre ordinateur doit :
>
> * 4 Go de mémoire disponible
> * Avoir un processeur Intel 64 bits ou un processeur AMD Ryzen avec une traduction d’adresses de second niveau (SLAT)
> * Exécuter Windows 10 Build 1803 + ([vérifier votre Build #](ms-settings:about))
> * Avoir mis à jour les pilotes graphiques (Device Manager > cartes d’affichage > pilote de mise à jour)
>
> Si votre ordinateur ne répond pas à ce critère, vous pourrez peut-être exécuter [Intel HAXM](https://github.com/intel/haxm/wiki/Installation-Instructions-on-Windows) ou l' [hyperviseur AMD](https://github.com/google/android-emulator-hypervisor-driver-for-amd-processors). Pour plus d’informations, consultez l’article : [accélération matérielle pour les performances de l’émulateur](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator/hardware-acceleration) ou la documentation de l' [émulateur de Android Studio](https://developer.android.com/studio/run/emulator).

1. Vérifiez que le matériel et le logiciel de votre ordinateur sont compatibles avec Hyper-V en ouvrant une invite de commandes et en entrant la commande suivante :`systeminfo`

    ![Configuration requise pour Hyper-V à partir de SystemInfo dans l’invite de commandes](../images/systeminfo.png)

2. Dans la zone de recherche Windows (en bas à gauche), entrez « fonctionnalités Windows ». Sélectionnez **activer ou désactiver des fonctionnalités Windows** dans les résultats de la recherche.

3. Une fois la liste des **fonctionnalités Windows** affichée, faites défiler l’affichage pour accéder à **Hyper-V** (y compris les outils d’administration et la plateforme) et à la **plateforme d’hyperviseur Windows**, assurez-vous que la case est cochée pour activer les deux, puis sélectionnez **OK**.

4. Redémarrez votre ordinateur lorsque vous y êtes invité.

### <a name="emulator-for-native-development-with-android-studio"></a>Émulateur pour le développement natif avec Android Studio

Lors de la génération et du test d’une application Android native, nous vous recommandons d' [utiliser Android Studio](./native-android.md). Une fois que votre application est prête pour le test, vous pouvez générer et exécuter votre application en procédant comme suit :

1. Dans la barre d’outils Android Studio, sélectionnez votre application dans le menu déroulant **exécuter les configurations** .

    ![Menu Android Studio exécuter la configuration](../images/android-run-config-menu.png)

2. Dans le menu déroulant **périphérique cible** , sélectionnez l’appareil sur lequel vous souhaitez exécuter votre application.

    ![Menu Android Studio périphérique cible](../images/android-target-device-menu.png)

3. Sélectionnez Exécuter ▷. Cette opération lance le [émulateur Android](https://developer.android.com/studio/run/emulator).

> [!TIP]
> Une fois que votre application est installée sur l’appareil de l’émulateur, vous pouvez utiliser [Apply changes](https://developer.android.com/studio/run#apply-changes) pour déployer certaines modifications de code et de ressources sans créer de nouvelle apk.

### <a name="emulator-for-cross-platform-development-with-visual-studio"></a>Émulateur pour le développement multiplateforme avec Visual Studio

De nombreuses options de l' [émulateur Android](https://www.androidauthority.com/best-android-emulators-for-pc-655308/) sont disponibles pour les PC Windows. Nous vous recommandons d’utiliser l' [émulateur Android](https://developer.android.com/studio/run/emulator)de Google, car il offre un accès aux dernières images du système d’exploitation Android et aux services de Google Play.

### <a name="install-android-emulator-with-visual-studio"></a>Installer l’émulateur Android avec Visual Studio

1. Si vous ne l’avez pas encore fait, téléchargez [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/). Utilisez la Visual Studio Installer pour [modifier vos charges de travail](https://docs.microsoft.com/visualstudio/install/modify-visual-studio?view=vs-2019#modify-workloads) et vous assurer que vous disposez du **développement mobile avec la charge de travail .net**.

2. Créez un projet. Une fois que vous avez [configuré le émulateur Android](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator/), vous pouvez utiliser la [Android Device Manager](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator/device-manager?tabs=windows&pivots=windows#requirements) pour créer, dupliquer, personnaliser et lancer un large éventail d’appareils virtuels Android. Lancez le Android Device Manager à partir du menu outils avec : **Tools** > **Android** > **Android Device Manager**.

3. Une fois que le Android Device Manager s’ouvre, sélectionnez **+ nouveau** pour créer un nouvel appareil.

4. Vous devez attribuer un nom à l’appareil, choisir le type d’appareil de base dans un menu déroulant, choisir un processeur et une version du système d’exploitation, ainsi que plusieurs autres variables pour l’appareil virtuel. Pour plus d’informations, [Android Device Manager écran principal](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator/device-manager?tabs=windows&pivots=windows#main-screen).

5. Dans la barre d’outils de Visual Studio, choisissez entre le **débogage** (joint au processus d’application en cours d’exécution dans l’émulateur après le démarrage de votre application) ou le mode de **mise** en service (désactive le débogueur). Choisissez ensuite un appareil virtuel dans le menu déroulant appareil, puis sélectionnez le bouton de **lecture** ▷ pour exécuter votre application dans l’émulateur.

    ![Émulateur Android de lancement de Visual Studio](../images/vs-target-device-menu.png)

## <a name="additional-resources"></a>Ressources supplémentaires

- [Développer des applications à deux écrans pour Android et obtenir le kit de développement logiciel (SDK) d’appareil surface Duo](https://docs.microsoft.com/dual-screen/android/)

- [Ajouter des exclusions Windows Defender pour améliorer les performances](defender-settings.md)
