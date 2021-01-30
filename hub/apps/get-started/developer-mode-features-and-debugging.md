---
title: Fonctionnalités et débogage du mode développeur
description: Découvrez les caractéristiques des fonctionnalités du mode développeur dans Windows 10 et les informations sur les erreurs d’installation.
keywords: Commencer avec une licence de développeur Visual Studio, appareil avec licence de développeur activée
ms.date: 10/13/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 85c64cd4baed7be9edde0dfc008ab90c1d537ca4
ms.sourcegitcommit: d0eef123b167dc63f482a9f4432a237c1c6212db
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99077227"
---
# <a name="developer-mode-features-and-debugging"></a>Fonctionnalités et débogage du mode développeur

Si vous êtes uniquement intéressé par les principes de base de l’installation du mode développeur sur votre application, vous pouvez simplement suivre les instructions fournies dans [activer votre appareil pour le développement](enable-your-device-for-development.md) pour commencer. Cet article aborde les fonctionnalités avancées du mode développeur, du mode développeur sur les versions précédentes de Windows 10 et des échecs de débogage en mode développeur.

## <a name="additional-developer-mode-features"></a>Fonctionnalités supplémentaires du mode développeur

Pour chaque famille d’appareils, des fonctionnalités de développement supplémentaires peuvent être disponibles. Ces fonctionnalités sont disponibles uniquement quand le mode développeur est activé sur l’appareil, et peuvent varier selon la version de votre système d’exploitation.

Cette image montre les fonctionnalités du mode développeur pour Windows 10 :

![Options du mode développeur](images/devmode-mob-options.png)

### <a name="span-iddevice-discovery-and-pairingspandevice-portal"></a><span id="device-discovery-and-pairing"></span>Portail d’appareil

Pour en savoir plus sur Portail d’appareil, consultez [Vue d’ensemble du portail d’appareil Windows](/windows/uwp/debug-test-perf/device-portal.md).


Pour obtenir des instructions d’installation spécifiques pour l’appareil, voir :
- [Portail d’appareil pour Bureau](/windows/uwp/debug-test-perf/device-portal-desktop)
- [Portail d’appareil pour HoloLens](/windows/mixed-reality/using-the-windows-device-portal)
- [Portail d’appareil pour IoT](/windows/iot-core/manage-your-device/deviceportal)
- [Portail d’appareil pour appareils mobiles](/windows/uwp/debug-test-perf/device-portal-mobile)
- [Portail d’appareil pour Xbox](/windows/uwp/xbox-apps/device-portal-xbox)

Si vous rencontrez des difficultés pour activer le mode développeur ou le portail d’appareil, consultez le forum [Problèmes connus](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22) pour chercher des solutions à ces problèmes, ou visitez la page relatif à l’[échec de l’installation du package Mode développeur](#failure-to-install-developer-mode-package) pour plus d’informations et pour savoir quelles bases de connaissances WSUS autoriser afin de débloquer le package Mode développeur.

### <a name="sideload-apps"></a>Charger la version test des applications

> [!IMPORTANT]
> À compter de la dernière mise à jour de Windows 10, ce paramètre n’est pas visible, car chargement est activé par défaut. Si vous utilisez une version précédente de Windows 10, vos paramètres par défaut vous permettent uniquement d’exécuter les applications du Microsoft Store, et vous devez activer le chargement indépendant pour installer des applications issues d’autres sources que Microsoft.

Le paramètre Charger la version test des applications est généralement utilisé par des sociétés ou des écoles qui ont besoin d’installer des applications personnalisées sur des appareils gérés, sans passer par le Microsoft Store, ou par toute personne devant exécuter des applications à partir de sources tierces. Dans ce cas, l’organisation applique généralement une stratégie visant à désactiver le paramètre *Applications UWP*, comme le montre l’image précédente de la page des paramètres. L’organisation fournit aussi le certificat nécessaire et l’emplacement d’installation pour le chargement indépendant des applications. Pour plus d’informations, consultez les articles TechNet [Chargement indépendant d’applications dans Windows 10](/windows/deploy/sideload-apps-in-windows-10) et [Notions de base de Microsoft Intune](/mem/intune/fundamentals/).

Informations spécifiques à la famille d’appareils

-   Pour la famille d’appareils de bureau : Vous pouvez installer un package d’application (.appx) et tout certificat nécessaire à l’exécution de l’application en exécutant le script Windows PowerShell créé avec le package (« Add-AppDevPackage.ps1 »). Pour plus d’informations, voir [Création de packages d’application UWP](/windows/msix/package/packaging-uwp-apps).

-   Pour la famille d’appareils mobiles : Si le certificat requis est déjà installé, vous pouvez appuyer sur le fichier pour installer tout fichier .appx reçu par courrier électronique ou sur une carte SD.


Le paramètre **Charger la version test des applications** est une option plus sécurisée que le mode développeur, car vous ne pouvez pas installer d’applications sans certificat approuvé sur l’appareil.

> [!NOTE]
> Si vous effectuez un chargement indépendant des applications, veillez à ce que les applications que vous installez proviennent toujours de sources fiables. Quand vous procédez au chargement d’une version test d’une application qui n’a pas été certifiée par le Microsoft Store, vous indiquez que vous avez obtenu l’ensemble des droits nécessaires au chargement d’une version test de cette application et que vous êtes l’unique responsable des dommages résultant de l’installation et de l’exécution de cette application. Voir la section Windows &gt; Microsoft Store de cette [déclaration de confidentialité](https://privacy.microsoft.com/privacystatement).


### <a name="ssh"></a>SSH

Les services SSH sont activés dès lors que vous activez la découverte d’appareils sur votre appareil.  Ils sont utilisés lorsque votre appareil est une cible de déploiement distant pour des applications UWP.   Ces services se nomment « SSH Server Broker » et « SSH Server Proxy ».

> [!NOTE]
> Il ne s’agit pas de l’implémentation OpenSSH de Microsoft, que vous pouvez trouver sur [GitHub](https://github.com/PowerShell/Win32-OpenSSH).  

Pour tirer parti des services SSH, vous pouvez activer la découverte d’appareils pour permettre le couplage de code PIN. Si vous avez l’intention d’exécuter un autre service SSH, vous pouvez le configurer sur un autre port ou désactiver les services SSH du mode développeur. Pour désactiver les services SSH, désactivez la découverte d’appareils.  

La connexion SSH s’effectue par le biais du compte DevToolsUser, qui accepte un mot de passe pour l’authentification.  Ce mot de passe est le code PIN qui s’affiche sur l’appareil après que l’utilisateur a appuyé sur le bouton de couplage de la découverte d’appareils. Il est valide uniquement lorsque le code PIN est affiché.  Un sous-système SFTP est également activé pour la gestion manuelle du dossier DevelopmentFiles, dans lequel les déploiements de fichiers isolés sont installés à partir de Visual Studio.

#### <a name="caveats-for-ssh-usage"></a>Avertissements concernant l’utilisation de SSH
Le serveur SSH existant utilisé dans Windows n’est pas encore conforme au protocole. De ce fait, l’utilisation d’un client SFTP ou SSH peut nécessiter une configuration spéciale.  En particulier, le sous-système SFTP exécutant la version 3 ou inférieure, tout client qui se connecte doit être configuré de façon à anticiper un ancien serveur.  Sur des appareils plus anciens, le serveur SSH utilise `ssh-dss` pour l’authentification de clé publique, ce qui est déconseillé par OpenSSH.  Pour se connecter à ces appareils, le client SSH doit être configuré manuellement pour accepter `ssh-dss`.  

### <a name="device-discovery"></a>Détection du périphérique

Quand vous activez la découverte d’appareils, vous consentez à rendre votre appareil visible des autres appareils du réseau via mDNS.  Cette fonctionnalité vous permet également d’obtenir le code PIN SSH pour effectuer le couplage à cet appareil, en appuyant sur le bouton de couplage affiché lorsque la découverte d’appareils est activée.  Cette invite de code PIN doit être affichée sur l’écran afin de terminer votre premier déploiement Visual Studio ciblant l’appareil.  

![Couplage de code PIN](images/devmode-pc-pinpair.PNG)

N’activer la découverte d’appareils que si vous envisagez de faire de l’appareil une cible de déploiement. Par exemple, si vous utilisez Device Portal pour déployer une application sur un téléphone à des fins de test, vous devez activer la découverte d’appareils sur le téléphone, mais pas sur votre PC de développement.

### <a name="optimizations-for-windows-explorer-remote-desktop-and-powershell-desktop-only"></a>Optimisations pour l’Explorateur Windows, Bureau à distance et PowerShell (Bureau uniquement)

 Pour la famille d’appareils de bureau, la page de paramètres **Pour les développeurs** propose des raccourcis vers les paramètres qui vous permettent d’optimiser votre PC pour les tâches de développement. Pour chaque paramètre, vous pouvez cocher la case correspondante et cliquer sur **Appliquer**, ou cliquez sur le lien **Afficher les paramètres** pour ouvrir la page de paramètres de cette option.


## <a name="notes"></a>Remarques
Dans les versions antérieures de Windows 10 Mobile, une option de vidage sur incident était présente dans le menu Paramètres de développeur.  Elle a été déplacée vers [Portail d’appareil](/windows/uwp/debug-test-perf/device-portal.md) afin de pouvoir être utilisée à distance, plutôt que simplement par port USB.  

Vous pouvez utiliser plusieurs outils pour déployer une application à partir d’un PC Windows 10 sur un appareil Windows 10. Les deux appareils doivent être connectés au même sous-réseau du réseau par une connexion filaire ou sans fil, ou ils doivent être connectés par USB. Dans les deux cas, seul le package d’application (.appx/.appxbundle) est installé, et non les certificats.

-   Utilisez l’outil de déploiement d’applications Windows 10 (WinAppDeployCmd). En savoir plus sur [l’outil WinAppDeployCmd](/previous-versions/windows/apps/mt203806(v=vs.140)).
-   Vous pouvez utiliser [Portail d’appareil](/windows/uwp/debug-test-perf/device-portal.md) pour effectuer un déploiement de votre navigateur vers un appareil mobile exécutant Windows 10 version 1511 ou ultérieure. Utilisez la page **[Applications](/windows/uwp/debug-test-perf/device-portal.md#apps-manager)** dans Device Portal pour charger un package d’application (.appx) sur le serveur et l’installer sur l’appareil.

## <a name="failure-to-install-developer-mode-package"></a>Échec de l’installation du package Mode développeur
Parfois, en raison de problèmes réseau ou d’administration, le Mode développeur ne s’installe pas correctement. Le package Mode développeur est nécessaire pour un déploiement **à distance** sur ce PC (à l’aide du Portail d’appareil à partir d’un navigateur ou de la fonction Découverte d’appareils pour activer SSH), mais pas pour un développement local.  Même si vous rencontrez ces problèmes, vous pouvez toujours déployer votre application localement à l’aide de Visual Studio, ou à partir de cet appareil sur un autre appareil.

Voir le forum [Problèmes connus](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22) pour rechercher des solutions de contournement à ces problèmes et bien plus encore.

> [!NOTE]
> Si le mode développeur ne s’installe pas correctement, nous vous encourageons à formuler une demande de commentaire. Dans l’application **Hub de commentaires**, sélectionnez **Ajouter un commentaire**, puis choisissez la catégorie **Plateforme de développement** ainsi que la sous-catégorie **Mode développeur**. L’envoi de commentaires permettra à Microsoft de résoudre le problème que vous avez rencontré.

### <a name="failed-to-locate-the-package"></a>Échec de la localisation du package

« Le package Mode développeur n’a pas pu être localisé dans la mise à jour Windows. Code d’erreur 0x80004005. En savoir plus   

Cette erreur peut se produire en raison d’un problème de connectivité réseau, des paramètres d’Entreprise ou d’un package manquant.

Pour résoudre ce problème :

1. Assurez-vous que votre ordinateur est connecté à Internet.
2. Si vous utilisez un ordinateur appartenant à un domaine, adressez-vous à votre administrateur réseau. Le package Mode développeur, comme toutes les fonctionnalités à la demande, est bloqué par défaut dans WSUS.
2.1. Pour débloquer le package Mode développeur dans les versions précédentes et actuelles, les bases de connaissances suivantes doivent être autorisées dans WSUS : 4016509, 3180030, 3197985  
3. Recherchez les mises à jour de Windows dans Paramètres > Mises à jour et sécurité > Mises à jour Windows.
4. Vérifiez que le package Mode développeur Windows est présent dans Paramètres > Système > Applications et fonctionnalités > Gérer les fonctionnalités facultatives > Ajouter une fonctionnalité. S’il n’est pas présent, Windows ne peut pas trouver le package approprié pour votre ordinateur.

Après avoir suivi les étapes ci-dessus, désactivez puis réactivez le Mode développeur pour vérifier le correctif.


### <a name="failed-to-install-the-package"></a>Échec de l’installation du package

« Échec d’installation du package Mode développeur. Code d’erreur 0x80004005. En savoir plus

Cette erreur peut se produire en raison d’incompatibilités entre votre version de Windows et le package Mode développeur.

Pour résoudre ce problème :

1. Recherchez les mises à jour de Windows dans Paramètres > Mises à jour et sécurité > Mises à jour Windows.
2. Redémarrez votre ordinateur pour vérifier que toutes les mises à jour sont appliquées.


## <a name="use-group-policies-or-registry-keys-to-enable-a-device"></a>Utiliser des stratégies de groupe ou des clés de Registre pour activer un appareil

Pour la plupart des développeurs, vous pouvez utiliser l’application Paramètres pour activer votre appareil pour le débogage. Dans certains scénarios, comme les tests automatisés, vous pouvez employer d’autres méthodes pour activer votre appareil de bureau Windows 10 pour le développement.  Ces étapes n’activent pas le serveur SSH et n’autorisent pas l’appareil à être ciblé pour le déploiement distant et le débogage.

Vous pouvez utiliser gpedit.msc pour définir les stratégies de groupe visant à activer l’appareil, sauf si vous disposez de Windows 10 Famille. Si vous disposez de Windows 10 Famille, vous devez exécuter des commandes regedit ou PowerShell pour définir les clés de Registre directement en vue d’activer votre appareil.

**Utiliser gpedit afin d’activer votre appareil**

1.  Exécutez **Gpedit.msc**.
2.  Accédez à Stratégie de l’ordinateur local &gt; Configuration ordinateur &gt; Modèles d’administration &gt; Composants Windows &gt; Déploiement du package d’application
3.  Pour activer le chargement indépendant, modifiez les stratégies afin d’activer :

    -   **Autoriser l’installation des applications approuvées**

    OU

    Pour activer le mode développeur, modifiez les stratégies pour activer les deux options suivantes :

    -   **Autoriser l’installation des applications approuvées**
    -   **Autorise le développement d’applications UWP et leur installation à partir d’un environnement de développement intégré**

4.  Redémarrez votre machine.

**Utiliser regedit pour activer votre appareil**

1.  Exécutez **regedit**.
2.  Pour activer le chargement indépendant, définissez cette valeur DWORD sur 1 :

    -   `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\AppModelUnlock\AllowAllTrustedApps`

    OU

    Pour activer le mode développeur, définissez ces valeurs DWORD sur 1 :

    -   `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\AppModelUnlock\AllowDevelopmentWithoutDevLicense`

**Utiliser PowerShell pour activer votre appareil**

1.  Exécutez PowerShell avec des privilèges administrateur.
2.  Pour activer le chargement indépendant, exécutez cette commande :

    ```powershell
    PS C:\WINDOWS\system32> reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\AppModelUnlock" /t REG_DWORD /f /v "AllowAllTrustedApps" /d "1"
    ```

    OU

    Pour activer le mode développeur, exécutez cette commande :

    ```powershell
    PS C:\WINDOWS\system32> reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\AppModelUnlock" /t REG_DWORD /f /v "AllowDevelopmentWithoutDevLicense" /d "1"
    ```

## <a name="upgrade-your-device-from-windows-81-to-windows-10"></a>Mettre à niveau votre appareil de Windows 8.1 vers Windows 10

Après avoir créé des applications ou effectué un chargement indépendant d’applications sur votre appareil Windows 8.1, vous devez installer une licence de développeur. Si vous mettez à niveau votre appareil de Windows 8.1 vers Windows 10, ces informations sont conservées. Exécutez la commande suivante pour les supprimer de votre appareil Windows 10 mis à niveau. Cette étape n’est pas obligatoire si vous effectuez une mise à niveau directement de Windows 8.1 vers Windows 10 version 1511 ou ultérieure.

**Pour annuler l’inscription d’une licence de développeur**

1.  Exécutez PowerShell avec des privilèges administrateur.
2.  Exécutez la commande suivante : `unregister-windowsdeveloperlicense`.

Après cela, vous devez activer votre appareil pour le développement, comme décrit dans cette rubrique, afin de pouvoir continuer à développer dessus. Si vous ne le faites, vous risquez d’obtenir une erreur quand vous déboguez votre application ou tentez de créer un package pour celle-ci. Voici un exemple de cette erreur :

Erreur : DEP0700 : Échec de l’inscription de l’application.