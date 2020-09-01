---
title: Développement Android natif sur Windows
description: Commencez à développer des applications Android natives sur Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Android, Windows, Android Studio, Visual Studio, jeu Android c++, Windows Defender, émulateur, appareil virtuel, installation, Java, Kotlin
ms.date: 04/28/2020
ms.openlocfilehash: c9c718d2cccc6a38ac75d3220a79c7b2ec757f54
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166843"
---
# <a name="get-started-with-native-android-development-on-windows"></a>Prise en main du développement Android natif sur Windows

Ce guide vous aidera à utiliser Windows pour créer des applications Android natives. Si vous préférez une solution multiplateforme, consultez [vue d’ensemble du développement Android sur Windows](./overview.md) pour obtenir un bref résumé de certaines options.

La façon la plus simple de créer une application Android Native consiste à utiliser des Android Studio avec [Java ou Kotlin](#java-or-kotlin), bien qu’il soit également possible d' [utiliser C ou C++ pour le développement Android](#use-c-or-c-for-android-game-development) si vous avez un but spécifique. Les outils du kit de développement logiciel (SDK) Android Studio compilent votre code, vos données et vos fichiers de ressources dans un fichier. apk d’archive. Un fichier APK contient tout le contenu d’une application Android et est le fichier utilisé par les appareils Android pour installer l’application.

## <a name="install-android-studio"></a>Installer Android Studio

Android Studio est l’environnement de développement intégré officiel pour le système d’exploitation Android de Google. [Téléchargez la dernière version de Android Studio pour Windows](https://developer.android.com/studio).

- Si vous avez téléchargé un fichier. exe (recommandé), double-cliquez dessus pour le lancer.
- Si vous avez téléchargé un fichier. zip, décompressez le fichier ZIP, copiez le dossier Android-Studio dans le dossier Program Files, puis ouvrez le dossier Android-Studio > bin et lancez studio64.exe (pour les ordinateurs 64 bits) ou studio.exe (pour les ordinateurs 32 bits).

Suivez l’Assistant Installation de Android Studio et installez les packages du kit de développement logiciel (SDK) recommandés. À mesure que de nouveaux outils et d’autres API deviennent disponibles, Android Studio vous avertit de l’affichage d’une fenêtre contextuelle ou de la recherche de mises à jour en sélectionnant **aide**  >  **Rechercher les mises à jour**.

## <a name="create-a-new-project"></a>Création d'un projet

Sélectionnez **fichier**  >  **nouveau**  >  **nouveau projet**.

Dans la fenêtre **choisir votre projet** , vous pouvez choisir entre ces modèles :

- **Activité de base**: crée une application simple avec une barre d’application, un bouton d’action flottant et deux fichiers de disposition : un pour l’activité et un pour séparer le contenu de texte.

- **Activité vide**: crée une activité vide et un seul fichier de disposition avec un exemple de contenu de texte.

- **Activité de navigation inférieure**: crée une barre de navigation inférieure standard pour une activité. Consultez le [composant de navigation inférieur](https://material.io/guidelines/components/bottom-navigation.html) dans les règles de conception de matériel de Google.

- En savoir plus sur la [sélection d’un modèle d’activité](https://developer.android.com/studio/projects/templates#SelectTemplate) dans la documentation Android Studio.

Les modèles sont couramment utilisés pour ajouter des activités à des modules d’application nouveaux et existants. Par exemple, pour créer un écran de connexion pour les utilisateurs de votre application, ajoutez une activité avec le [modèle d’activité de connexion](https://developer.android.com/studio/projects/templates#LoginActivity).

> [!NOTE]
> Le système d’exploitation Android est basé sur l’idée des **composants** et utilise l' **activité** des termes et l' **intention** de définir des interactions. Une **activité** représente une tâche unique et ciblée qu’un utilisateur peut faire. Une **activité** fournit une fenêtre pour créer l’interface utilisateur à l’aide de classes basées sur la classe de **vue** . Il existe un cycle de vie des **activités** dans le système d’exploitation Android, défini par un ensemble de six rappels : `onCreate()` ,,,, `onStart()` `onResume()` `onPause()` `onStop()` et `onDestroy()` . Les composants d’activité interagissent les uns avec les autres à l’aide d’objets d' **intention** . Intention définit l’activité pour démarrer ou décrit le type d’action à effectuer (et le système sélectionne l’activité appropriée pour vous, ce qui peut même provenir d’une application différente). En savoir plus sur les [activités](https://developer.android.com/reference/android/app/Activity), le [cycle de vie des activités](https://developer.android.com/guide/components/activities/activity-lifecycle)et les [intentions](https://developer.android.com/reference/android/content/Intent.html) dans les documents Android.

### <a name="java-or-kotlin"></a>Java ou Kotlin

**Java** est devenu un langage dans 1991, développé par ce qui a été suivi de Sun Microsystems, mais qui est maintenant détenu par Oracle. Il est devenu l’un des langages de programmation les plus populaires et les plus puissants, avec l’une des plus grandes communautés de support au monde. Java est basé sur les classes et orienté objet, conçu pour avoir le moins de dépendances d’implémentation possible. La syntaxe est similaire à C et C++, mais elle a moins de fonctionnalités de bas niveau que l’une ou l’autre.

**Kotlin** a été annoncé pour la première fois en tant que nouveau langage Open source par JetBrains dans 2011 et a été inclus comme alternative à Java dans Android Studio depuis 2017. En mai 2019, Google a annoncé Kotlin comme langage préféré pour les développeurs d’applications Android. en dépit du fait qu’il s’agit d’un langage plus récent, il dispose également d’une communauté de support solide et a été identifié comme l’un des langages de programmation les plus rapides. Kotlin est multiplateforme, typée statiquement et conçu pour interagir entièrement avec Java.

Java est plus largement utilisé pour un plus grand nombre d’applications et offre certaines fonctionnalités que Kotlin ne fait pas, telles que les exceptions vérifiées, les types primitifs qui ne sont pas des classes, les membres statiques, les champs non privés, les types génériques et les opérateurs ternaires. Kotlin est spécifiquement conçu pour et recommandé par Android. Il offre également certaines fonctionnalités que Java ne fait pas, telles que les références null contrôlées par le système de type, aucun type brut, aucun tableau invariant, les types de fonction appropriés (par opposition aux conversions SAM de Java), l’utilisation de la variance de site sans caractères génériques, les conversions intelligentes et bien plus encore. La documentation Kotlin offre [une analyse plus approfondie de la comparaison avec Java](https://kotlinlang.org/docs/reference/comparison-to-java.html).

### <a name="minimum-api-level"></a>Niveau d’API minimal

Vous devrez déterminer le niveau d’API minimal pour votre application. Cela détermine la version d’Android que votre application prendra en charge. Les niveaux d’API inférieurs sont plus anciens et, par conséquent, prennent généralement en charge davantage d’appareils, mais les plus hauts niveaux d’API sont plus récents et offrent plus de fonctionnalités.

![Écran de sélection de l’API Android Studio minimal](../images/android-minimum-api-selection.png)

Sélectionnez le lien **m’aider à choisir** pour ouvrir un tableau de comparaison présentant les fonctionnalités de distribution et de clé de la prise en charge des appareils associées à la version de la plateforme.

![Android Studio écran de comparaison d’API minimum](../images/android-minimum-api-selection-2.png)

### <a name="instant-app-support-and-androidx-artifacts"></a>Prise en charge instantanée des applications et artefacts Androidx

Vous pouvez remarquer une case à cocher pour **prendre en charge les applications instantanées** et une autre pour **utiliser les artefacts androidx** dans les options de création de votre projet. La *prise en charge des applications instantanées* n’est pas activée et la valeur par défaut *androidx* est activée.

Google Play les **applications instantanées** permettent aux utilisateurs d’essayer une application ou un jeu sans l’installer préalablement. Ces applications instantanées peuvent être exposées dans l’Play Store, la recherche Google, les réseaux sociaux et partout où vous partagez un lien. En activant la case à cocher **prendre en charge les applications instantanées** , vous demandez Android Studio d’inclure le kit de développement logiciel (SDK) Google Play instant Development avec votre projet. Pour en savoir plus sur la [Google Play des applications instantanées](https://developer.android.com/topic/google-play-instant) et la [création d’un bundle d’applications à extension instantanée](https://developer.android.com/topic/google-play-instant/getting-started/instant-enabled-app-bundle), consultez la documentation Android.

Les **artefacts AndroidX** représentent la nouvelle version de la bibliothèque de prise en charge Android et offrent une compatibilité descendante entre les versions d’Android. AndroidX fournit un espace de noms cohérent à partir de la chaîne AndroidX pour tous les packages disponibles.

> [!NOTE]
> AndroidX est désormais la bibliothèque par défaut. Pour décocher cette case et utiliser la bibliothèque de support précédente, vous devez supprimer le dernier Kit de développement logiciel (SDK) Android Q. Pour obtenir des instructions, consultez [décocher utiliser les artefacts Androidx](https://stackoverflow.com/questions/56580980/uncheck-use-androidx-artifacts) sur StackOverflow, mais notez d’abord que les packages de bibliothèque de prise en charge précédents ont été mappés aux packages Androidx. * correspondants. Pour un mappage complet de toutes les anciennes classes et des artefacts de build aux nouveaux, consultez [migration vers AndroidX](https://developer.android.com/jetpack/androidx/migrate).

## <a name="project-files"></a>Fichiers projet

La fenêtre Android Studio **projet** , qui contient les fichiers suivants (Assurez-vous que la vue Android est sélectionnée dans le menu déroulant) :

**application > Java > com. example. myfirstapp > MainActivity**

Activité principale et point d’entrée de votre application. Lorsque vous générez et exécutez votre application, le système lance une instance de cette activité et charge sa disposition.

**mise en page de l’application > res > > activity_main.xml**

Fichier XML définissant la disposition de l’interface utilisateur de l’activité. Elle contient un élément TextView avec le texte « Hello World »

**manifestes d' > d’application > AndroidManifest.xml**

Fichier manifeste décrivant les caractéristiques fondamentales de l’application et de chacun de ses composants.

**Scripts Gradle > Build. Gradle**

Il existe deux fichiers portant ce nom : « Project : My First App », pour l’ensemble du projet et « module : App », pour chaque module d’application. Un nouveau projet n’aura initialement qu’un seul module. Utilisez le fichier Build. file du module pour contrôler la façon dont le plug-in Gradle génère votre application. Pour plus d’informations, consultez les documents Android, [configurer votre](https://developer.android.com/studio/build/index) article de Build.

## <a name="use-c-or-c-for-android-game-development"></a>Utiliser C ou C++ pour le développement de Jeux Android

Le système d’exploitation Android est conçu pour prendre en charge les applications écrites en Java ou Kotlin, en bénéficiant d’outils intégrés dans l’architecture du système. De nombreuses fonctionnalités système, telles que l’interface utilisateur Android et le traitement des intentions, sont uniquement exposées via des interfaces Java. Il existe quelques cas où vous voudrez peut-être **utiliser du code C ou C++ via le kit de développement natif (NDK) Android** en dépit de certains des défis associés. Le développement de jeux est un exemple, car les jeux utilisent généralement une logique de rendu personnalisée écrite en OpenGL ou Vulkan et tirent parti d’une multitude de bibliothèques C axées sur le développement de jeux. L’utilisation de C ou C++ *peut* également vous aider à réduire les performances supplémentaires d’un appareil pour atteindre une faible latence ou exécuter des applications gourmandes en calcul, telles que des simulations physiques. Toutefois, le NDK **n’est pas approprié pour la plupart des programmeurs Android débutants** . À moins que vous n’ayez un objectif spécifique pour l’utilisation du NDK, nous vous recommandons d’utiliser Java, Kotlin ou l’un des [frameworks multiplateforme](./overview.md).

Pour créer un nouveau projet avec prise en charge de C/C++ :

- Dans la section **choisir votre projet** de l’Assistant Android Studio, sélectionnez le type de projet *C++ * natif*. Sélectionnez **suivant**, renseignez les champs restants, puis sélectionnez à nouveau **suivant** .

- Dans la section personnalisation de la **prise en charge de c++** de l’Assistant, vous pouvez personnaliser votre projet à l’aide du champ **standard c++** . Utilisez la liste déroulante pour sélectionner la normalisation de C++ que vous souhaitez utiliser. La sélection de **chaîne d’outils par défaut** utilise le paramètre cmake par défaut. Sélectionnez **Terminer**.

- Une fois que Android Studio créé votre projet, vous pouvez trouver un dossier **CPP** dans le volet **projet** qui contient les fichiers sources natifs, les en-têtes, les scripts de génération pour cmake ou NDK-Build, ainsi que les bibliothèques prégénérées qui font partie de votre projet. Vous trouverez également un exemple de fichier source C++, `native-lib.cpp` , dans le `src/main/cpp/` dossier qui fournit une fonction simple qui `stringFromJNI()` retourne la chaîne « Hello from C++ ». En outre, vous trouverez un script de génération CMake, [`CMakeLists.txt`](https://developer.android.com/studio/projects/configure-cmake.html) , dans le répertoire racine de votre module, requis pour la génération de votre bibliothèque native.

Pour plus d’informations, consultez la rubrique docs Android : [Ajouter du code C et C++ à votre projet](https://developer.android.com/studio/projects/add-native-code). Pour obtenir des exemples, consultez les [exemples d’Android NDK avec Android Studio intégration C++ référentiel](https://github.com/android/ndk-samples) sur GitHub. Pour compiler et exécuter un jeu C++ sur Android, utilisez l' [API Google Play Game services](https://developers.google.com/games/services/cpp/gettingStartedAndroid).

## <a name="design-guidelines"></a>Instructions de conception

Les utilisateurs de périphériques attendent que les applications s’affichent et se comportent d’une certaine façon... qu’il s’agisse de balayer ou de toucher ou d’utiliser des commandes vocales, les utilisateurs contiendront des attentes spécifiques quant à ce à quoi votre application doit ressembler et comment l’utiliser. Ces attentes doivent rester cohérentes afin de réduire la confusion et la frustration. Android offre un guide sur les attentes en matière de plateformes et d’appareils, qui combine la base de conception de documents Google pour les modèles visuels et de navigation, ainsi que des instructions de qualité pour la compatibilité, les performances et la sécurité.

Pour en savoir plus, consultez la [documentation de conception Android](https://developer.android.com/design).

### <a name="fluent-design-system-for-android"></a>Système de conception Fluent pour Android

Microsoft propose également des conseils de conception dans le but de fournir une expérience transparente dans tout le portefeuille des applications mobiles de Microsoft.

[Système de conception Fluent pour](https://www.microsoft.com/design/fluent/#/android) la conception Android et créer des applications personnalisées qui sont Android en mode natif, tout en continuant à Fluent.

- [Boîte à outils d’esquisse](https://aka.ms/fluenttoolkits/android/sketch)
- [Boîte à outils Figma](https://aka.ms/fluenttoolkits/android/figma)
- [Police Android](https://fonts.google.com/specimen/Roboto)
- [Instructions relatives à l’interface utilisateur Android](https://developer.android.com/design/)
- [Instructions pour les icônes d’application Android](https://developer.android.com/guide/practices/ui_guidelines/icon_design)

## <a name="additional-resources"></a>Ressources supplémentaires

- [Notions de base des applications Android](https://developer.android.com/guide/components/fundamentals)

- [Développer des applications à deux écrans pour Android et obtenir le kit de développement logiciel (SDK) d’appareil surface Duo](/dual-screen/android/)

- [Ajouter des exclusions Windows Defender pour améliorer les performances](defender-settings.md)

- [Activer la prise en charge de la virtualisation pour améliorer les performances de l’émulateur](emulator.md#enable-virtualization-support)