---
title: REACT native pour le développement Android sur Windows
description: Commencez à développer des applications Android à l’aide de Xamarin native sur Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Android, Windows, REACT native, émulateur, exposer, groupe Metro, terminal
ms.date: 04/28/2020
ms.openlocfilehash: db49e3ed12fee8e7ced7680e305a84afb89f32a2
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255233"
---
# <a name="get-started-developing-for-android-using-react-native"></a>Prise en main du développement pour Android avec REACT Native

Ce guide vous aidera à vous familiariser avec l’utilisation de REACT native sur Windows pour créer une application multiplateforme qui fonctionnera sur des appareils Android.

## <a name="overview"></a>Vue d’ensemble

REACT native est une infrastructure d’application mobile [Open source](https://github.com/facebook/react-native) créée par Facebook. Il est utilisé pour développer des applications pour Android, iOS, Web et UWP (Windows) fournissant des contrôles d’interface utilisateur natifs et un accès complet à la plateforme native. L’utilisation de REACT Native nécessite une compréhension des notions de base de JavaScript.

## <a name="get-started-with-react-native-by-installing-required-tools"></a>Prise en main de REACT Native en installant les outils nécessaires

1. [Installez Visual Studio code](https://code.visualstudio.com) (ou votre éditeur de code de votre choix).

2. [Installez Android Studio pour Windows](https://developer.android.com/studio). Android Studio installe le dernier Android SDK par défaut. REACT Native requiert le kit de développement logiciel (SDK) Android 6,0 (Marshmallow) ou une version ultérieure. Nous vous recommandons d’utiliser le dernier Kit de développement logiciel (SDK).

3. Créer des chemins de variables d’environnement pour le kit de développement logiciel (SDK) Java et Android SDK :
    - Dans le menu de recherche de Windows, entrez : « modifier les variables d’environnement système » pour ouvrir la fenêtre **Propriétés système** .
    - Choisissez les **variables d’environnement...** , puis choisissez **nouveau...** sous **variables utilisateur**.
    - Entrez le nom et la valeur de la variable (chemin d’accès). Les chemins d’accès par défaut pour les kits de développement logiciel (SDK) Java et Android sont les suivants. Si vous avez choisi un emplacement spécifique pour installer les kits de développement logiciel (SDK) Java et Android, veillez à mettre à jour les chemins d’accès aux variables en conséquence.
        - JAVA_HOME : C:\Program Files\Android\Android Studio\jre\jre
        - ANDROID_HOME : C:\Users\username\AppData\Local\Android\Sdk

    ![Capture d’écran de l’ajout du chemin des variables environnementales](../images/add-environmental-variable-path.png)

4. [Installer NodeJS pour Windows](https://nodejs.org/en/) Vous pouvez envisager d’utiliser [node version Manager (NVM) pour Windows](https://github.com/coreybutler/nvm-windows#node-version-manager-nvm-for-windows) si vous prévoyez d’utiliser plusieurs projets et la version de NodeJS. Nous vous recommandons d’installer la dernière version de LTS pour les nouveaux projets.

> [!NOTE]
> Vous pouvez également envisager d’installer et d’utiliser le [terminal Windows](https://www.microsoft.com/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab) pour travailler avec votre interface de ligne de commande (CLI) préférée, ainsi que [git pour le contrôle de version](https://git-scm.com/downloads). Le [JDK Java](https://www.oracle.com/java/technologies/javase-downloads.html) est fourni avec Android Studio v 2.2 +, mais si vous devez mettre à jour votre JDK séparément de Android Studio, utilisez le [programme d’installation de Windows x64](https://www.oracle.com/java/technologies/javase-jdk14-downloads.html).

## <a name="create-a-new-project-with-react-native"></a>Créer un nouveau projet avec REACT Native

1. Utilisez NPM pour installer l’utilitaire de ligne de commande d’affichage de l' [interface CLI](https://docs.expo.io/versions/latest/) à partir de l’invite de commandes Windows, de PowerShell, de [Windows Terminal](https://www.microsoft.com/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab)ou du terminal intégré dans vs code (afficher > terminal intégré).

    ```powershell
    npm install -g expo-cli
    ```

2. Utilisez Expo pour créer une application REACT native qui s’exécute sur iOS, Android et Web. Vous devez ensuite choisir entre les modèles de projet, y compris les **espaces vides**, **vides (machine à écrire)** et les **tabulations** (exemples d’écrans utilisant REACT-navigation), **minimal**ou **minimal (machine à écrire)**.

    ```powershell
    expo init my-new-app
    ```

    > [!NOTE]
    > Si vous êtes utilisé pour utiliser `npx create-react-native-app`, cela fonctionnera toujours, mais l’initialisation de l’interface de commande expose présente [quelques autres avantages](https://github.com/react-native-community/discussions-and-proposals/issues/23).

3. Ouvrez votre nouveau répertoire « My-New-App » :

    ```powershell
    cd my-new-app
    ```

4. Pour exécuter votre projet, entrez la commande suivante. Une fenêtre localhost s’ouvre dans votre navigateur Internet par défaut. l’offre groupée de nœuds Metro est alors affichée. Il affiche également un code QR à la fois dans la ligne de commande et dans la fenêtre de navigateur de l’offreur de métro. * Vous pouvez également utiliser la commande `npm start` : `npm run android` ou.

     ```powershell
    expo start
    ```

    ![Capture d’écran de l’offre groupée Metro dans le navigateur](../images/metro-bundler.png)

5. Pour afficher votre projet en cours d’exécution sur un appareil Android, vous devez d’abord [installer l’application cliente exposer avec le Google Play Store](https://play.google.com/store/apps/details?id=host.exp.exponent&hl=en_US) sur votre appareil Android. Une fois l’application exposer client installée, ouvrez-la sur votre appareil et sélectionnez **analyser le code QR**. Une fois le code QR inscrit, vous pouvez voir la génération du package sur votre appareil et dans la fenêtre du Bundleeur Metro s’exécutant sur localhost dans votre navigateur.

6. Pour afficher votre projet s’exécutant sur un émulateur Android, vous devez d’abord ouvrir Android Studio, puis créer et démarrer un appareil virtuel. **Outils** > **AVD Manager** > **[+ créer un appareil virtuel...](https://developer.android.com/studio/run/managing-avds#createavd)**. Une fois votre appareil virtuel créé, sélectionnez le bouton de lancement ▷ sous la colonne **actions** de la Device Manager virtuelle Android pour démarrer l’émulation de l’appareil. Une fois l’appareil virtuel ouvert, revenez à la fenêtre du Bundleeur Metro s’exécutant dans la fenêtre de votre navigateur Internet et sélectionnez « exécuter sur un appareil/émulateur Android » dans la colonne de gauche. Vous devriez voir une fenêtre contextuelle vous informant que Metro Bundle tente d’ouvrir un simulateur... puis, l’application exposer client s’ouvre sur votre appareil Android émulé et, une fois que vous avez terminé le téléchargement de l’ensemble JavaScript, vous verrez votre application native REACT s’afficher. (Si vous rencontrez des problèmes, [activez la case à cocher exposer les documents de l’émulateur Android](https://docs.expo.io/workflow/android-studio-emulator/).)

7. Ouvrez votre projet Native REACT pour commencer à travailler sur votre application. Vos modifications doivent être mises à jour automatiquement dans l’application en cours d’exécution via le client Expo sur votre appareil ou dans votre Émulateur Android.

8. Essayez de modifier le texte de la page d’accueil pour indiquer : « Hello World ! ». Vous pouvez le faire dans l’IDE de votre choix. (Nous vous recommandons VS Code ou Android Studio.) Le fichier de la page d’accueil varie en fonction du modèle que vous avez choisi. Il peut s' `App.js`agir `App.tsx`de, `HomeScreen.js`ou.

    ```typescript
    export default function App() {
      return (
        <View style={styles.container}>
          <Text>Hello World!</Text>
        </View>
      );
    }
    ```

9. Essayez d’ajouter une image. Tout d’abord, vous devez créer un dossier au même niveau que les dossiers « Android » et « iOS » dans votre application, appelons-le « images ». Placez une image dans ce dossier, `your-image.png` par exemple. Utilisez le format ci-dessous pour référencer votre image et lui appliquer un style de hauteur et de largeur.

     ```typescript
    export default function App() {
      return (
        <View style={styles.container}>
          <Text>Hello World!</Text>
          <Image source={require('./images/your-image.png')} style = {{height: 200, width: 250, }} />
        </View>
      );
    }
    ```

> [!TIP]
> Si vous souhaitez ajouter la prise en charge de votre application native REACT afin qu’elle s’exécute en tant qu’application Windows 10, consultez la page prise en [main de REACT native pour Windows](https://microsoft.github.io/react-native-windows/docs/getting-started) docs.

## <a name="additional-resources"></a>Ressources supplémentaires

- [Développer des applications à deux écrans pour Android et obtenir le kit de développement logiciel (SDK) d’appareil surface Duo](https://docs.microsoft.com/dual-screen/android/)

- [Ajouter des exclusions Windows Defender pour améliorer les performances](defender-settings.md)

- [Activer la prise en charge de la virtualisation pour améliorer les performances de l’émulateur](emulator.md#enable-virtualization-support)
