---
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: Portail d’appareil pour Windows Desktop
description: Découvrez comment Windows Device Portal ouvre les diagnostics et l’automatisation sur votre bureau Windows.
ms.date: 02/06/2019
ms.topic: article
keywords: Windows 10, UWP, portail des appareils
ms.localizationpriority: medium
ms.openlocfilehash: 73f7e827c0ec8ca289d3523da06601de978a91d2
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75681970"
---
# <a name="device-portal-for-windows-desktop"></a>Portail d’appareil pour Windows Desktop

Le portail d’appareil Windows vous permet d’afficher les informations de diagnostic et d’interagir avec votre ordinateur de bureau via une connexion HTTP, dans une fenêtre de votre navigateur. Vous pouvez utiliser le portail d’appareil pour :
- voir et manipuler une liste de processus en cours d’exécution ;
- installer, supprimer, démarrer et arrêter des applications ;
- modifier les profils de connexion Wi-Fi, afficher la force du signal et voir ipconfig ;
- afficher les graphiques d’utilisation du processeur, de la mémoire, des E/S, du réseau et du GPU en temps réel ;
- collecter les vidages de processus ;
- collecter les traces ETW ; 
- Manipuler le stockage isolé des applications de version test chargée

## <a name="set-up-device-portal-on-windows-desktop"></a>Configurer le portail d’appareil sous Windows Desktop

### <a name="turn-on-developer-mode"></a>Activer le mode développeur

À compter de Windows 10, version 1607, certaines des dernières fonctionnalités pour ordinateurs de bureau sont destinées uniquement au mode développeur. Pour en savoir plus sur la façon d’activer le mode développeur, voir [Activer votre appareil pour le développement](../get-started/enable-your-device-for-development.md).

> [!IMPORTANT]
> Parfois, en raison de problèmes réseau ou de compatibilité, le mode développeur ne s’installe pas correctement. Pour résoudre ces problèmes, voir la section correspondante [Activer votre appareil pour le développement](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#failure-to-install-developer-mode-package).

### <a name="turn-on-device-portal"></a>Activer le portail d’appareil

Vous pouvez activer le portail d’appareil dans la section **Pour les développeurs** des **paramètres**. Lorsque vous l’activez, vous devez également créer un nom d’utilisateur et un mot de passe. N’utilisez pas votre compte Microsoft ou d’autres informations d’identification Windows. 

![Section Portail d’appareil de l’application Paramètres](images/device-portal/device-portal-desk-settings.png) 

Une fois le portail d’appareil activé, des liens Web permettant d’y accéder apparaissent en bas de cette section. Notez le numéro de port apparaissant à la fin des URL : ce numéro est généré de manière aléatoire lorsque le portail d’appareil est activé, mais il doit rester cohérent entre deux redémarrages de l’ordinateur. 

Ces liens offrent deux modes de connexion au portail d’appareil : sur le réseau local (y compris le VPN) ou via l’hôte local.

### <a name="connect-to-device-portal"></a>Connexion au portail d’appareil

Pour une connexion via l’hôte local, ouvrez une fenêtre de navigateur et entrez l’adresse indiquée ici pour le type de connexion que vous utilisez.

* Localhost : `http://127.0.0.1:<PORT>` ou `http://localhost:<PORT>`
* Réseau local : `https://<IP address of the desktop>:<PORT>`

Une connexion HTTPS est requise pour l’authentification et la communication sécurisée.

Si vous utilisez le portail d’appareil dans un environnement protégé, comme un laboratoire de test, où vous faites confiance à tous les utilisateurs du réseau local, si aucune information personnelle n’est présente sur votre appareil et si vous présentez des exigences uniques, vous pouvez désactiver l’option Authentification. Cela permet une communication non chiffrée. Toute personne connaissant l’adresse IP de votre ordinateur pourra s’y connecter et le contrôler.

## <a name="device-portal-content-on-windows-desktop"></a>Contenu du portail d’appareil sur Windows Desktop

Le portail d’appareil sur Windows Desktop propose les ensembles de pages standard. Pour en obtenir une description détaillée, voir [Vue d’ensemble du portail d’appareil Windows](device-portal.md).

- App Manager (Gestionnaire d’applications)
- Explorateur de fichiers
- Processus en cours d’exécution
- Performances
- Journal de débogage
- Suivi d’événements pour Windows (ETW)
- Suivi des performances
- Gestionnaire de périphériques
- Mise en réseau
- Données d’incident
- Fonctionnalités
- Réalité mixte
- Débogueur d’installation en continu
- Services de localisation
- Vide

## <a name="more-device-portal-options"></a>Autres options du portail d’appareil

### <a name="registry-based-configuration-for-device-portal"></a>Configuration basée sur le registre pour le portail d’appareil

Si vous souhaitez sélectionner des numéros de port pour le portail d’appareil (par exemple, 80 et 443), vous pouvez définir les clés de Registre suivantes :

- Sous `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WebManagement\Service`
    - `UseDynamicPorts`: DWORD requis. Définissez ce paramètre sur 0 pour conserver les numéros de port que vous avez choisis.
    - `HttpPort`: DWORD requis. Contient le numéro de port que le portail d’appareil va écouter pour les connexions HTTP.    
    - `HttpsPort`: DWORD requis. Contient le numéro de port que le portail d’appareil va écouter pour les connexions HTTPS.
    
Dans le chemin d’accès de la même clé de Registre, vous pouvez également désactiver l’obligation d’authentification :
- `UseDefaultAuthorizer` - `0` pour désactivé, `1` pour activé.  
    - Ce paramètre contrôle les deux exigences d’authentification de base pour chaque connexion et le transfert de HTTP vers HTTPS.  
    
### <a name="command-line-options-for-device-portal"></a>Options de ligne de commande pour le portail d’appareil
À partir d’une invite de commandes d’administration, vous pouvez activer et configurer des parties du portail d’appareil. Pour afficher le dernier jeu de commandes pris en charge sur votre Build, vous pouvez exécuter `webmanagement /?`

- `sc start webmanagement` ou `sc stop webmanagement` 
    - Activez ou désactivez le service. Le mode développeur doit toujours être activé. 
- `-Credentials <username> <password>` 
    - Définissez un nom d’utilisateur et un mot de passe pour le portail d’appareil. Le nom d’utilisateur doit être conforme aux normes d’authentification de base, et ne peut donc pas contenir de deux-points ( :) et doivent être créés à partir de caractères ASCII standard, par exemple, [a-zA-z0-9], car les navigateurs n’analysent pas le jeu de caractères complet de manière standard.  
- `-DeleteSSL` 
    - Cela réinitialise le cache de certificat SSL utilisé pour les connexions HTTPS. Si vous rencontrez des erreurs de connexion TLS qui ne peuvent pas être évitées (par opposition à l’avertissement de certificat attendu), cette option peut résoudre le problème à votre place. 
- `-SetCert <pfxPath> <pfxPassword>`
    - Pour plus de détails, voir [Fourniture d’un certificat SSL personnalisé au portail d’appareil](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-ssl).  
    - Vous pouvez ainsi installer votre propre certificat SSL pour corriger la page d’avertissement SSL qui s’affiche généralement dans le portail d’appareil. 
- `-Debug <various options for authentication, port selection, and tracing level>`
    - Exécutez une version autonome du portail d’appareil avec une configuration spécifique et des messages de débogage visibles. Cela est particulièrement utile pour la création d’un [plug-in empaqueté](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-plugin). 
    - Pour plus d’informations sur cette exécution en tant que système pour tester complètement votre plug-in empaqueté, voir [l’article du MSDN Magazine](https://msdn.microsoft.com/magazine/mt826332.aspx).

## <a name="common-errors-and-issues"></a>Erreurs et problèmes courants

Voici quelques-unes des erreurs courantes que vous pouvez rencontrer lors de la configuration du portail des appareils.

### <a name="windowsupdatesearch-returns-invalid-number-of-updates-0x800f0950-cbs_e_invalid_windows_update_count"></a>WindowsUpdateSearch retourne un nombre non valide de mises à jour (0x800f0950 CBS_E_INVALID_WINDOWS_UPDATE_COUNT)

Vous pouvez obtenir cette erreur lorsque vous tentez d’installer les packages de développement dans une version préliminaire de Windows 10. Ces packages de fonctionnalités à la demande (DOM) sont hébergés sur Windows Update et leur téléchargement sur des versions préliminaires requiert que vous vous abonniez à la fonctionnalité de vol. Si votre installation n’est pas en cours de vol pour la combinaison de construction et de sonnerie appropriée, la charge utile ne sera pas téléchargeable. Vérifiez les éléments suivants :

1. Accédez à **paramètres > mettre à jour & Security > programme Windows Insider** et vérifiez que la section **compte Windows Insider** contient vos informations de compte correctes. Si vous ne voyez pas cette section, sélectionnez **lier un compte Windows Insider**, ajoutez votre compte de messagerie et confirmez qu’il s’affiche sous le titre du **compte Windows Insider** (vous devrez peut-être sélectionner **lier un compte Windows Insider** une deuxième fois pour lier un compte récemment ajouté).
 
2. Sous **quel type de contenu souhaitez-vous recevoir ?** , assurez-vous que l’option **développement actif de Windows** est sélectionnée.
 
3. Sous **quel rythme voulez-vous obtenir de nouvelles builds ?** , assurez-vous que **Windows Insider Fast** est sélectionné.
 
4. Vous devez maintenant être en mesure d’installer FoDs. Si vous avez confirmé que vous êtes sur Windows Insider rapidement et que vous ne pouvez toujours pas installer le FoDs, fournissez des commentaires et joignez les fichiers journaux sous **C:\Windows\Logs\CBS**.

### <a name="sc-startservice-openservice-failed-1060-the-specified-service-does-not-exist-as-an-installed-service"></a>SC StartService : échec de OpenService 1060 : le service spécifié n’existe pas en tant que service installé

Vous pouvez recevoir cette erreur si les packages de développement ne sont pas installés. Sans les packages de développement, il n’existe aucun service de gestion Web. Essayez de réinstaller les packages de développement.

### <a name="cbs-cannot-start-download-because-the-system-is-on-metered-network-cbs_e_metered_network"></a>CBS ne peut pas démarrer le téléchargement, car le système est sur un réseau limité (CBS_E_METERED_NETWORK)

Vous pouvez obtenir cette erreur si vous utilisez une connexion Internet facturée à l’accès. Vous ne pourrez pas télécharger les packages de développement sur une connexion limitée.

## <a name="see-also"></a>Articles associés

* [Vue d’ensemble du portail de périphériques Windows](device-portal.md)
* [Informations de référence sur l’API principale du portail des appareils](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-api-core)
