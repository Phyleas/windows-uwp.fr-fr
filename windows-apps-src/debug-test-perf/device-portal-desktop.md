---
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: Portail d’appareil Windows pour les postes de travail
description: Découvrez de quelle manière l’outil Windows Device Portal fournit des paramètres, des diagnostics et des fonctionnalités d’automatisation sur votre ordinateur de bureau.
ms.date: 01/08/2021
ms.topic: article
ms.custom: contperf-fy21q3
keywords: windows 10, uwp, portail d’appareil
ms.localizationpriority: medium
ms.openlocfilehash: 11bfc81f045887dfdbba9d380eedd6b9ff40709a
ms.sourcegitcommit: 02d220ef0ec0ecd7ed733086ba164ee9653d9602
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/09/2021
ms.locfileid: "98055982"
---
# <a name="windows-device-portal-for-desktop"></a>Portail d’appareil Windows pour les postes de travail

Windows Device Portal (WDP) est un outil de gestion et de débogage des appareils qui vous permet de configurer et gérer les paramètres des appareils ainsi que d’accéder à des informations de diagnostic sur HTTP à partir d’un navigateur web. Pour des détails à propos de WDP sur d’autres appareils, consultez [Vue d’ensemble de Windows Device Portal](device-portal.md).

Vous pouvez utiliser WDP pour :

- gérer les paramètres des appareils (comme avec l’application **Paramètres Windows**) ;
- voir et manipuler une liste de processus en cours d’exécution ;
- installer, supprimer, démarrer et arrêter des applications ;
- modifier les profils de connexion WiFi, afficher la force du signal et voir les détails ipconfig ;
- afficher les graphiques d’utilisation du processeur, de la mémoire, des E/S, du réseau et du GPU en temps réel ;
- collecter les vidages de processus ;
- collecter les traces ETW ;
- manipuler le stockage isolé des applications de version test chargée.

## <a name="set-up-windows-device-portal-on-a-desktop-device"></a>Configurer Windows Device Portal sur un ordinateur de bureau

### <a name="turn-on-developer-mode"></a>Activer le mode développeur

À compter de Windows 10, version 1607, certaines des dernières fonctionnalités pour ordinateurs de bureau sont destinées uniquement au mode développeur. Pour en savoir plus sur la façon d’activer le mode développeur, consultez [Activer votre appareil pour le développement](/windows/apps/get-started/enable-your-device-for-development).

> [!IMPORTANT]
> Parfois, en raison de problèmes réseau ou de compatibilité, le mode développeur ne s’installe pas correctement. Pour résoudre ces problèmes, voir la section correspondante [Activer votre appareil pour le développement](/windows/apps/get-started/enable-your-device-for-development#failure-to-install-developer-mode-package).

### <a name="turn-on-windows-device-portal"></a>Activer Windows Device Portal

Vous pouvez activer WDP dans la section **Pour les développeurs** de l’application **Paramètres Windows**. Lorsque vous l’activez, vous devez également créer un nom d’utilisateur et un mot de passe. N’utilisez pas votre compte Microsoft ou d’autres informations d’identification Windows.

![Section Windows Device Portal de l’application Paramètres](images/device-portal/device-portal-desk-settings.png)

Une fois WDP activé, des liens web permettant d’y accéder apparaissent en bas de cette section. Notez le numéro de port indiqué à la fin des URL listées : ce numéro est généré de manière aléatoire lorsque WDP est activé, mais il doit rester cohérent entre deux redémarrages de l’ordinateur.

Ces liens offrent deux modes de connexion à WDP : sur le réseau local (y compris le VPN) ou via l’hôte local. Une fois connecté, voici à quoi cela doit ressembler :

![Portail d’appareil Windows](images/device-portal/device-portal-example.png)

### <a name="turn-off-windows-device-portal"></a>Désactiver Windows Device Portal

Vous pouvez désactiver WDP dans la section **Pour les développeurs** de l’application **Paramètres Windows**.

### <a name="connect-to-windows-device-portal"></a>Se connecter au Portail d'appareil Windows

Pour vous connecter par le biais de l’hôte local, ouvrez une fenêtre de navigateur et entrez l’un des URI indiqués ici (selon le type de connexion que vous utilisez).

- Hôte local : `http://127.0.0.1:<PORT>` ou `http://localhost:<PORT>`
- Réseau local : `https://<IP address of the desktop>:<PORT>`

Une connexion HTTPS est requise pour l’authentification et la communication sécurisée.

Si vous utilisez WDP dans un environnement protégé, par exemple un laboratoire de test, où vous faites confiance à tous les utilisateurs du réseau local, si aucune information personnelle n’est présente sur votre appareil et si vous présentez des exigences uniques, vous pouvez désactiver l’option Authentification. Cela permet une communication non chiffrée. Toute personne connaissant l’adresse IP de votre ordinateur pourra s’y connecter et le contrôler.

## <a name="windows-device-portal-content"></a>Contenu de Windows Device Portal

WDP fournit l’ensemble de pages suivantes.

- Gestionnaire d’applications
- Xbox Live
- Explorateur de fichiers
- Processus actifs
- Performances
- Débogage
- Journalisation du suivi d’événements pour Windows (ETW)
- Suivi des performances
- Gestionnaire de périphériques
- Bluetooth
- Mise en réseau
- Données d’incident
- Fonctionnalités
- Mixed Reality
- Débogueur d’installation en continu
- Emplacement
- Vide

## <a name="using-windows-device-portal-to-test-and-debug-msix-apps"></a>Utilisation de Windows Device Portal pour tester et déboguer des applications MSIX


> [!VIDEO https://www.youtube.com/embed/PdgXeOMt4hk]


## <a name="more-windows-device-portal-options"></a>Autres options de Windows Device Portal

### <a name="registry-based-configuration"></a>Configuration basée sur le Registre

Si vous souhaitez sélectionner des numéros de port pour WDP (par exemple, 80 et 443), vous pouvez définir les clés de Registre suivantes :

- Sous `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WebManagement\Service`
    - `UseDynamicPorts` : Un DWORD requis. Définissez ce paramètre sur 0 pour conserver les numéros de port que vous avez choisis.
    - `HttpPort` : Un DWORD requis. Contient le numéro du port sur lequel WDP écoute les connexions HTTP.
    - `HttpsPort` : Un DWORD requis. Contient le numéro du port sur lequel WDP écoute les connexions HTTPS.
    
Dans le chemin d’accès de la même clé de Registre, vous pouvez également désactiver l’obligation d’authentification :
- `UseDefaultAuthorizer` - `0` pour désactivé, `1` pour activé.  
    - Ce paramètre contrôle les deux exigences d’authentification de base pour chaque connexion et le transfert de HTTP vers HTTPS.  
    
### <a name="command-line-options-for-windows-device-portal"></a>Options de ligne de commande pour Windows Device Portal

À partir d’une invite de commandes d’administration, vous pouvez activer et configurer des composants de WDP. Pour afficher le dernier jeu de commandes pris en charge sur votre version, vous pouvez exécuter `webmanagement /?`

- `sc start webmanagement` ou `sc stop webmanagement`
    - Activez ou désactivez le service. Le mode développeur doit toujours être activé. 
- `-Credentials <username> <password>` 
    - Définissez un nom d’utilisateur et un mot de passe pour WDP. Le nom d’utilisateur doit être conforme aux normes de l’authentification de base. Par conséquent, il ne peut pas contenir de signe deux-points (:). Il doit, par ailleurs, comporter des caractères ASCII standard, par exemple [a-zA-Z0-9], car les navigateurs n’analysent pas le jeu de caractères complet de manière standard.  
- `-DeleteSSL` 
    - Cela réinitialise le cache de certificat SSL utilisé pour les connexions HTTPS. Si vous rencontrez des erreurs de connexion TLS qui ne peuvent pas être évitées (par opposition à l’avertissement de certificat attendu), cette option peut résoudre le problème à votre place. 
- `-SetCert <pfxPath> <pfxPassword>`
    - Pour plus de détails, consultez [Provisionner Windows Device Portal avec un certificat SSL personnalisé](./device-portal-ssl.md).  
    - Vous pouvez ainsi installer votre propre certificat SSL pour corriger la page d’avertissement SSL qui s’affiche généralement dans WDP. 
- `-Debug <various options for authentication, port selection, and tracing level>`
    - Exécutez une version autonome de WDP avec une configuration spécifique et des messages de débogage visibles. Cela est particulièrement utile pour la création d’un [plug-in empaqueté](./device-portal-plugin.md). 
    - Pour plus d’informations sur cette exécution en tant que système pour tester complètement votre plug-in empaqueté, voir [l’article du MSDN Magazine](/archive/msdn-magazine/2017/october/windows-device-portal-write-a-windows-device-portal-packaged-plug-in).

## <a name="troubleshooting"></a>Résolution des problèmes

Voici quelques-unes des erreurs courantes que vous pouvez rencontrer lors de la configuration de Windows Device Portal.

### <a name="windowsupdatesearch-returns-invalid-number-of-updates-0x800f0950-cbs_e_invalid_windows_update_count"></a>WindowsUpdateSearch renvoie un nombre incorrect de mises à jour (0x800f0950 CBS_E_INVALID_WINDOWS_UPDATE_COUNT)

Cette erreur peut s’afficher si vous tentez d’installer les packages de développement sur une préversion de Windows 10. Ces packages de fonctionnalités à la demande (Feature-on-Demand ou FoD) sont hébergés sur Windows Update, et leur téléchargement sur des préversions nécessite un abonnement à la distribution de version d’évaluation. Si votre installation ne bénéficie pas de la distribution de version d’évaluation pour la version et la combinaison appropriées, la charge utile ne pourra pas être téléchargée. Vérifiez les éléments suivants :

1. Accédez à **Paramètres > Mise à jour et sécurité > Programme Windows Insider** et vérifiez que la section **Compte Windows Insider** affiche vos informations de compte correctes. Si vous ne voyez pas cette section, sélectionnez **Associer un compte Windows Insider**, ajoutez votre compte de messagerie, puis vérifiez qu’il s’affiche sous l’en-tête **Compte Windows Insider** (vous devrez peut-être sélectionner à nouveau **Associer un compte Windows Insider** pour lier un compte récemment ajouté).
 
2. Sous **Quel type de contenu voulez-vous recevoir ?** , vérifiez que l’option **Développement actif de Windows** est sélectionnée.
 
3. Sous **À quel rythme souhaitez-vous recevoir de nouvelles versions ?** , vérifiez que l’option **Windows Insider - Rapide** est sélectionnée.
 
4. Vous devriez maintenant pouvoir installer les fonctionnalités à la demande. Si vous avez vérifié que l’option Windows Insider - Rapide est sélectionnée, mais que vous ne pouvez toujours pas installer les fonctionnalités à la demande, décrivez le problème et joignez les fichiers journaux figurant sous **C:\Windows\Logs\CBS**.

### <a name="sc-startservice-openservice-failed-1060-the-specified-service-does-not-exist-as-an-installed-service"></a>[SC] StartService : OpenService FAILED 1060 : Le service spécifié n’existe pas en tant que service installé

Cette erreur peut s’afficher si les packages de développement ne sont pas installés. Sans les packages de développement, il n’existe aucun service de gestion Web. Essayez de réinstaller les packages de développement.

### <a name="cbs-cannot-start-download-because-the-system-is-on-metered-network-cbs_e_metered_network"></a>CBS ne peut pas démarrer le téléchargement car le système est sur un réseau limité (CBS_E_METERED_NETWORK)

Cette erreur peut s’afficher si vous utilisez une connexion Internet limitée. Vous ne pourrez pas télécharger les packages de développement sur une connexion limitée.

## <a name="see-also"></a>Voir aussi

* [Vue d’ensemble du Portail d'appareil Windows](device-portal.md)
* [Informations de référence sur les API principales de Windows Device Portal](./device-portal-api-core.md)