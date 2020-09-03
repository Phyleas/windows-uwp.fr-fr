---
ms.assetid: A5320094-DF53-42FC-A6BA-A958F8E9210B
title: Tester les applications de Surface Hub à l’aide de Visual Studio
description: Le simulateur de Visual Studio fournit un environnement permettant de concevoir, développer, déboguer et tester des applications UWP, y compris les applications générées pour Surface Hub.
ms.date: 10/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5a10a8a6e5b4e5188d28c0f75aace50f7465e5f4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163483"
---
# <a name="test-surface-hub-apps-using-visual-studio"></a>Tester les applications de Surface Hub à l’aide de Visual Studio
Le simulateur de Visual Studio fournit un environnement dans lequel vous pouvez concevoir, développer, déboguer et tester des applications de plateforme Windows universelle (UWP), y compris les applications que vous avez conçues pour Microsoft Surface Hub. Le simulateur n’utilise pas la même interface utilisateur que Surface Hub, mais il permet de tester l’apparence et le comportement de votre application par rapport à la taille d’écran et à la résolution du Surface Hub.

Pour plus d’informations sur l’outil simulateur en général, consultez [Exécuter des applications UWP dans le simulateur](/visualstudio/debugger/run-windows-store-apps-in-the-simulator).

## <a name="add-surface-hub-resolutions-to-the-simulator"></a>Ajouter des résolutions de Surface Hub au simulateur
Pour ajouter des résolutions de Surface Hub au simulateur :

1. Créez une configuration pour le Surface Hub de 55 pouces en enregistrant le code XML suivant dans un fichier nommé *HardwareConfigurations-SurfaceHub55.xml*.  

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ArrayOfHardwareConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                                  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <HardwareConfiguration>
            <Name>SurfaceHub55</Name>
            <DisplayName>Surface Hub 55"</DisplayName>
            <Resolution>
                <Height>1080</Height>
                <Width>1920</Width>
            </Resolution>
            <DeviceSize>55</DeviceSize>
            <DeviceScaleFactor>100</DeviceScaleFactor>
        </HardwareConfiguration>
    </ArrayOfHardwareConfiguration>
    ```

2. Créez une configuration pour le Surface Hub de 84 pouces en enregistrant le code XML suivant dans un fichier nommé *HardwareConfigurations-SurfaceHub84.xml*.

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ArrayOfHardwareConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                                  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <HardwareConfiguration>
            <Name>SurfaceHub84</Name>
            <DisplayName>Surface Hub 84"</DisplayName>
            <Resolution>
                <Height>2160</Height>
                <Width>3840</Width>
            </Resolution>
            <DeviceSize>84</DeviceSize>
            <DeviceScaleFactor>150</DeviceScaleFactor>
        </HardwareConfiguration>
    </ArrayOfHardwareConfiguration>
    ```

3. Copiez ces deux fichiers XML dans *C:\Program Files (x86)\Common Files\Microsoft Shared\Windows Simulator\\\&lt;numéro de version&gt;\HardwareConfigurations*.

   > [!NOTE]
   > L’enregistrement de fichiers dans ce dossier nécessite des privilèges d’administration.

4. Exécutez votre application dans le simulateur Visual Studio. Cliquez sur le bouton **Modifier la résolution** de la palette et sélectionnez une configuration de Surface Hub dans la liste.

    ![Résolutions du simulateur Visual Studio](images/vs-simulator-resolutions.png)

   > [!TIP]
   > [Activez le mode tablette](https://support.microsoft.com/help/17210/windows-10-use-your-pc-like-a-tablet) pour mieux simuler l’expérience d’un Surface Hub.

## <a name="deploy-apps-to-a-surface-hub-device-from-visual-studio"></a>Déployer des applications sur un appareil Surface Hub à partir de Visual Studio
Le déploiement manuel d’une application sur un Surface Hub est un processus simple.

### <a name="enable-developer-mode"></a>Activer le mode développeur
Par défaut, les applications installées sur Surface Hub sont issues uniquement du Microsoft Store. Pour installer des applications signées par d’autres sources, vous devez activer le mode développeur.

> [!NOTE]
> Une fois que le mode développeur a été activé, vous devez réinitialiser le Surface Hub pour pouvoir le désactiver à nouveau. La réinitialisation de l’appareil supprime l’ensemble des configurations et des fichiers utilisateur locaux, puis réinstalle Windows.

1. À partir du menu **Démarrer** de l’appareil Surface Hub, ouvrez l’application Paramètres.

   > [!NOTE]
   > L’accès à l’application Paramètres du Surface Hub nécessite des privilèges d’administration.

2. Accédez à **Mise à jour et sécurité\> Pour les développeurs**.

3. Choisissez **Mode développeur** et acceptez le message d’avertissement.

### <a name="deploy-your-app-from-visual-studio"></a>Déployer votre application à partir de Visual Studio
Pour plus d’informations sur le processus de déploiement en général, consultez [Déploiement et débogage d’applications UWP](./deploying-and-debugging-uwp-apps.md).

   > [!NOTE]
   > Cette fonctionnalité nécessite Visual Studio 2015 Update 1 ou ultérieur, mais nous vous recommandons d’utiliser la version la plus récente de Visual Studio avec les dernières mises à jour. Une instance de Visual Studio à jour vous fera bénéficier de toutes les dernières mises à jour de sécurité et de développement.

1. Accédez à la liste déroulante des cibles de débogage à côté du bouton **Démarrer le débogage**, puis sélectionnez **Ordinateur distant**.

    <!--lcap: in your screenshot, you have local machine selected-->

   ![Liste déroulante des cibles de débogage Visual Studio](images/vs-debug-target.png)

2. Entrez l’adresse IP de l’appareil Surface Hub. Vérifiez que le mode d’authentification **Universel** est sélectionné.

   > [!TIP] 
   > Une fois que vous avez activé le mode développeur, vous pouvez trouver l’adresse IP du Surface Hub dans l’écran d’accueil.

3. Sélectionnez **Démarrer le débogage (F5)** pour déployer et déboguer votre application sur le Surface Hub, ou appuyez sur Ctrl+F5 pour déployer uniquement votre application.

   > [!TIP]
   > Si le Surface Hub affiche l’écran d’accueil, vous pouvez le faire disparaître en appuyant sur n’importe quel bouton.