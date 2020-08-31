---
title: Améliorer la vitesse des performances en mettant à jour les paramètres Defender
description: Découvrez comment améliorer la vitesse des performances et les temps de génération en mettant à jour les paramètres de Windows Defender pour exclure la vérification des types de fichiers spécifiés.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Android, Windows, Windows Defender, paramètres, configuration, exclusions,% USERPROFILE%, devenv.exe, performances, vitesse, Build, gradle
ms.date: 04/28/2020
ms.openlocfilehash: 0437ffc263c618e52c7a3e4dc3256e9fcd502c8e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154863"
---
# <a name="update-windows-defender-settings-to-improve-performance"></a>Mettre à jour les paramètres Windows Defender pour améliorer les performances

Ce guide explique comment configurer des exclusions dans vos paramètres de sécurité Windows Defender afin d’améliorer le temps de génération et la vitesse de performances globale de votre ordinateur Windows.

## <a name="windows-defender-overview"></a>Vue d’ensemble de Windows Defender

Dans Windows 10, version 1703 et versions ultérieures, l’application [antivirus Windows Defender](/windows/security/threat-protection/windows-defender-antivirus/windows-defender-security-center-antivirus) fait partie de la sécurité Windows. Windows Defender a pour objectif de protéger votre PC grâce à une protection en temps réel intégrée contre les virus, les ransomware, les logiciels espions et autres menaces de sécurité.

**Toutefois**, la protection en temps réel de Windows Defender peut également ralentir considérablement l’accès au système de fichiers et la vitesse de génération lors du développement d’applications Android.

Pendant le processus de génération Android, de nombreux fichiers sont créés sur votre ordinateur. Lorsque l’analyse antivirus en temps réel est activée, le processus de génération s’arrêtera chaque fois qu’un nouveau fichier sera créé pendant que l’antivirus analysera ce fichier.

Heureusement, Windows Defender a la possibilité d’exclure des fichiers, des répertoires de projet ou des types de fichiers que vous savez être sécurisés du processus d’analyse antivirus.

> [!WARNING]
> Pour vous assurer que votre ordinateur est protégé contre les logiciels malveillants, vous ne devez pas désactiver complètement l’analyse en temps réel ou votre logiciel antivirus Windows Defender.

## <a name="add-exclusions-to-windows-defender"></a>Ajouter des exclusions à Windows Defender

Pour améliorer la vitesse de génération d’Android, ajoutez des exclusions dans [Windows defender Security Center](windowsdefender://) en procédant comme suit :

1. Sélectionner le menu Windows bouton **Démarrer**
2. Entrer dans la **sécurité Windows**
3. Sélectionner la **protection contre les virus et les menaces**
4. Sélectionnez **gérer les paramètres** sous **virus & les paramètres de protection contre les menaces**
5. Faites défiler jusqu’à l’en-tête **exclusions** , puis sélectionnez **Ajouter ou supprimer des exclusions** .
6. Sélectionnez **+ Ajouter une exclusion**. Vous devrez ensuite choisir si l’exclusion que vous souhaitez ajouter est un **fichier**, un **dossier**, un **type de fichier**ou un **processus**.

![Capture d’écran de l’ajout d’exclusion Windows Defender](../images/windows-defender-exclusions.png)

## <a name="recommended-exclusions"></a>Exclusions recommandées

La liste suivante indique l’emplacement par défaut de chaque Android Studio Répertoire recommandé à ajouter comme exclusion de l’analyse en temps réel de Windows Defender :

- Cache Gradle : `%USERPROFILE%\.gradle`
- Projets de Android Studio : `%USERPROFILE%\AndroidStudioProjects`
- Android SDK: `%USERPROFILE%\AppData\Local\Android\SDK`
- Android Studio les fichiers système : `%USERPROFILE%\.AndroidStudio<version>\system`

Ces emplacements de répertoires peuvent ne pas s’appliquer à votre projet si vous n’avez pas utilisé les emplacements par défaut définis par Android Studio ou si vous avez téléchargé un projet à partir de GitHub (par exemple). Envisagez d’ajouter une exclusion au répertoire de votre projet de développement Android actuel, quel que soit l’emplacement où il se trouve.

Les exclusions supplémentaires que vous pouvez prendre en compte sont les suivantes :

- Processus de l’environnement de développement Visual Studio : `devenv.exe`
- Processus de génération Visual Studio : `msbuild.exe`
- Répertoire JetBrains : `%LOCALAPPDATA%\JetBrains\<Transient directory (folder)>`

Pour plus d’informations sur l’ajout d’exclusions d’analyse antivirus, notamment sur la personnalisation des emplacements de répertoire pour les environnements contrôlés par stratégie de groupe, consultez la section [impact sur l’antivirus](https://developer.android.com/studio/intro/studio-config#antivirus-impact) de la documentation Android Studio.

> [!Note]
> Daniel Knoodle a configuré un référentiel GitHub avec les scripts recommandés pour ajouter [des exclusions Windows Defender pour Visual Studio 2017](https://gist.github.com/dknoodle/5a66b8b8a3f2243f4ca5c855b323cb7b#file-windows-defender-exclusions-vs-2017-ps1-L10).

## <a name="additional-resources"></a>Ressources supplémentaires

- [Développer des applications à deux écrans pour Android et obtenir le kit de développement logiciel (SDK) d’appareil surface Duo](/dual-screen/android/)

- [Ajouter des exclusions Windows Defender pour améliorer les performances](./defender-settings.md)

- [Activer la prise en charge de la virtualisation pour améliorer les performances de l’émulateur](./emulator.md#enable-virtualization-support)