---
ms.assetid: 60fc48dd-91a9-4dd6-a116-9292a7c1f3be
title: Vue d’ensemble du portail d’appareil Windows
description: Découvrez comment Windows Device Portal vous permet de configurer et de gérer à distance votre appareil par le biais d’une connexion réseau ou USB.
ms.date: 04/09/2019
ms.topic: article
keywords: Windows 10, UWP, portail des appareils
ms.localizationpriority: medium
ms.openlocfilehash: 2292d97166d34905bb895aa3f53f864510a21f46
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74254760"
---
# <a name="windows-device-portal-overview"></a>Vue d’ensemble du portail d’appareil Windows

Windows Device Portal vous permet de configurer et de gérer à distance votre appareil par le biais d’une connexion réseau ou USB. Il fournit également des outils de diagnostic avancés pour vous aider à dépanner et à afficher les performances en temps réel de votre appareil Windows.

Le portail d’appareils Windows est un serveur Web sur votre appareil auquel vous pouvez vous connecter à partir d’un navigateur Web sur un PC. Si votre appareil dispose d’un navigateur Web, vous pouvez également vous connecter localement avec le navigateur sur cet appareil.

Le portail de périphériques Windows est disponible sur chaque famille d’appareils, mais les fonctionnalités et l’installation varient en fonction des besoins de chaque appareil. Cet article fournit une description générale de Device Portal et des liens vers des articles contenant des informations plus spécifiques pour chaque famille d’appareils.

La fonctionnalité du portail de périphériques Windows est implémentée avec les [API REST](device-portal-api-core.md) que vous pouvez utiliser directement pour accéder aux données et contrôler votre appareil par programme.

## <a name="setup"></a>Installation

Chaque appareil possède des instructions spécifiques concernant la connexion à Device Portal. Toutefois, chacun nécessite d’effectuer les étapes générales suivantes.

1. Activez le mode développeur et le portail des appareils sur votre appareil (configuré dans l’application Paramètres).

2. Connectez votre appareil et votre PC via un réseau local ou USB.

3. Accéder à la page Device Portal dans votre navigateur. Ce tableau montre les ports et les protocoles utilisés par chaque famille d’appareils.

Famille d’appareils | Activé par défaut ? | HTTP | HTTPS | USB
--------------|----------------|------|-------|----
HoloLens | Oui, en mode de développement | 80 (par défaut) | 443 (par défaut) | http://127.0.0.1:10080
IoT | Oui, en mode de développement | 8080 | Activer via la clé de registre | N/A
Xbox | Activer dans le mode de développement | Désactivé | 11443 | N/A
Bureau| Activer dans le mode de développement | 50080\* | 50043\* | N/A
Phone | Activer dans le mode de développement | 80| 443 | http://127.0.0.1:10080

\* ce n’est pas toujours le cas, en tant que portail des appareils sur les ports de revendications de bureau dans la plage éphémère (> 50000), afin d’éviter les collisions avec les revendications de port existantes sur l’appareil. Pour plus d’informations, consultez la section [Paramètres de port](device-portal-desktop.md#registry-based-configuration-for-device-portal) pour le bureau.  

Pour obtenir des instructions d’installation propres à chaque appareil, consultez :

- [Portail d’appareil pour HoloLens](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-hololens)
- [Portail d’appareil pour IoT](https://docs.microsoft.com/windows/iot-core/manage-your-device/DevicePortal)
- [Portail d’appareil pour appareils mobiles](device-portal-mobile.md)
- [Portail d’appareil pour Xbox](../xbox-apps/device-portal-xbox.md)
- [Portail d’appareil pour Bureau](device-portal-desktop.md#set-up-device-portal-on-windows-desktop)

## <a name="features"></a>Fonctionnalités

### <a name="toolbar-and-navigation"></a>Barre d’outils et navigation

La barre d’outils en haut de la page permet d’accéder aux fonctionnalités couramment utilisées.

- **Puissance**: accéder aux options d’alimentation.
  - **Arrêt** : éteint l’appareil.
  - **Redémarrer** : mise sous tension de l’appareil par cycle.
- **Aide** : ouvre la page d’aide.

Utilisez les liens du volet de navigation sur le côté gauche de la page pour naviguer vers les outils d’analyse et de gestion disponibles pour votre appareil.

Les outils qui sont communs à toutes les familles d’appareils sont décrits ici. D’autres options peuvent être disponibles selon l’appareil. Pour plus d’informations, consultez la page spécifique de votre type d’appareil.

### <a name="apps-manager"></a>App Manager (Gestionnaire d’applications)

Le gestionnaire des applications fournit des fonctionnalités d’installation/désinstallation et de gestion pour les packages d’application et les offres groupées sur l’appareil hôte.

![Page Gestionnaire d’applications du portail des appareils](images/device-portal/WDP_AppsManager2.png)

* **Déployer des applications**: déployez des applications empaquetées à partir d’hôtes locaux, réseau ou Web et inscrivez des fichiers libres à partir de partages réseau.
* **Applications installées**: utilisez le menu déroulant pour supprimer ou démarrer les applications qui sont installées sur l’appareil.
* **Applications en cours d’exécution**: obtenir des informations sur les applications en cours d’exécution et les fermer en fonction des besoins.

#### <a name="install-sideload-an-app"></a>Installer (chargement) une application

Vous pouvez chargement des applications pendant le développement à l’aide du portail des appareils Windows :

1. Une fois que vous avez créé un package d’application, vous pouvez l’installer à distance sur votre appareil. Une fois créé dans Visual Studio, un dossier de sortie est généré.

    ![Installation d’applications](images/device-portal/iot-installapp0.png)

2. Dans le portail d’appareils Windows, accédez à la page **Gestionnaire d’applications** .

3. Dans la section **déployer des applications** , sélectionnez **stockage local**.

4. Sous **Sélectionner le package d’application**, sélectionnez **choisir un fichier** et accédez au package d’application que vous souhaitez chargement.

5. Sous **Sélectionner un fichier de certificat (. cer) utilisé pour signer le package d’application**, sélectionnez **choisir un fichier** et accédez au certificat associé à ce package d’application.

6. Activez les cases à cocher respectives si vous souhaitez installer des packages facultatifs ou d’infrastructure avec l’installation de l’application, puis sélectionnez **suivant** pour les choisir.

7. Sélectionnez **installer** pour lancer l’installation.

8. Si l’appareil exécute Windows 10 en mode S et qu’il s’agit de la première fois que le certificat donné a été installé sur l’appareil, redémarrez l’appareil.

#### <a name="install-a-certificate"></a>Installer un certificat

Vous pouvez également installer le certificat via le portail de périphériques Windows et installer l’application par le biais d’autres moyens :

1. Dans le portail d’appareils Windows, accédez à la page **Gestionnaire d’applications** .

2. Dans la section **déployer des applications** , sélectionnez Installer le **certificat**.

3. Sous **Sélectionner un fichier de certificat (. cer) utilisé pour signer un package d’application**, sélectionnez **choisir un fichier** et recherchez le certificat associé au package d’application que vous souhaitez chargement.

4. Sélectionnez **installer** pour lancer l’installation.

5. Si l’appareil exécute Windows 10 en mode S et qu’il s’agit de la première fois que le certificat donné a été installé sur l’appareil, redémarrez l’appareil.

#### <a name="uninstall-an-app"></a>Désinstaller une application

1. Assurez-vous que votre application n’est pas en cours d’exécution.
2. Si c’est le cas, accédez à **exécution des applications** et fermez-la. Si vous essayez de désinstaller alors que l’application est en cours d’exécution, cela entraînera des problèmes lorsque vous tenterez de réinstaller l’application.
3. Sélectionnez l’application dans la liste déroulante, puis cliquez sur **supprimer**.

### <a name="running-processes"></a>Processus en cours d’exécution

Cette page affiche des détails sur les processus en cours d’exécution sur l’appareil hôte. Cela comprend les processus relatifs aux applications au système. Sur certaines plateformes (Desktop, IoT et HoloLens), vous pouvez terminer les processus.

![Page processus du portail de l’appareil en cours d’exécution](images/device-portal/mob-device-portal-processes.png)

### <a name="file-explorer"></a>Explorateur de fichiers

Cette page vous permet d’afficher et de manipuler des fichiers stockés par n’importe quelle application faisant. Consultez le billet de blog [utilisation de l’Explorateur de fichiers d’application](https://blogs.windows.com/buildingapps/2016/06/08/using-the-app-file-explorer-to-see-your-app-data/) pour en savoir plus sur l’Explorateur de fichiers et son utilisation.

![Page Explorateur de fichiers du portail de l’appareil](images/device-portal/mob-device-portal-AppFileExplorer.png)

### <a name="performance"></a>Performances

La page performances affiche des graphiques en temps réel des informations de diagnostic du système telles que l’utilisation de l’alimentation, la fréquence d’images et la charge de l’UC.

Voici les mesures disponibles :

- **UC**: pourcentage du total d’utilisation du processeur disponible
- **Mémoire**: total, en cours d’utilisation, disponible, validé, paginé et non paginé
- **E/s**: lire et écrire les quantités de données
- **Réseau**: données reçues et envoyées
- **GPU**: pourcentage du total d’utilisation du moteur GPU disponible

![Page performance du portail de l’appareil](images/device-portal/mob-device-portal-perf.png)

### <a name="event-tracing-for-windows-etw-logging"></a>Journalisation Suivi d’v nements pour Windows (ETW)

La page journalisation ETW gère les informations de Suivi d’v nements pour Windows en temps réel (ETW) sur l’appareil.

![Page journalisation ETW du portail d’appareils](images/device-portal/mob-device-portal-etw.png)

Cochez la case **Masquer les fournisseurs** pour n’afficher que la liste des événements.

- **Fournisseurs inscrits**: sélectionnez le fournisseur d’événements et le niveau de suivi. Le niveau de suivi est l’une des valeurs suivantes :
  1. Sortie ou arrêt anormal
  2. Erreurs graves
  3. Avertissements
  4. Avertissements sans erreur
  5. Suivi détaillé

  Cliquez ou appuyez sur **Activer** pour démarrer le suivi. Le fournisseur est ajouté à la liste déroulante **Fournisseurs activés**.
- **Fournisseurs personnalisés** sélectionnez un fournisseur ETW personnalisé et le niveau de suivi. Identifiez le fournisseur par son GUID. N’incluez pas de crochets dans le GUID.
- **Fournisseurs activés**: répertorie les fournisseurs activés. Sélectionnez un fournisseur dans la liste déroulante, puis cliquez sur ou appuyez sur **Désactiver** pour arrêter le suivi. Cliquez ou appuyez sur **Arrêter tout** pour suspendre tout le suivi.
- **Historique des fournisseurs**: affiche les fournisseurs ETW qui ont été activés au cours de la session active. Cliquez ou appuyez sur **Activer** pour activer un fournisseur qui a été désactivé. Cliquez ou appuyez sur **Effacer** pour supprimer l’historique.
- **Filtres/événements**: la section **événements** répertorie les événements ETW des fournisseurs sélectionnés sous forme de table. La table est mise à jour en temps réel. Utilisez le menu **filtres** pour configurer des filtres personnalisés pour les événements à afficher. Cliquez sur le bouton **Effacer** pour supprimer tous les événements ETW de la table. Cela ne désactive pas les fournisseurs. Vous pouvez cliquer sur **enregistrer dans un fichier** pour exporter les événements ETW actuellement collectés dans un fichier CSV local.

Pour plus d’informations sur l’utilisation de la journalisation ETW, consultez le billet [de blog utiliser le portail de l’appareil pour afficher les journaux de débogage](https://blogs.windows.com/buildingapps/2016/06/10/using-device-portal-to-view-debug-logs-for-uwp/) .

### <a name="performance-tracing"></a>Suivi des performances

La page suivi des performances vous permet d’afficher les traces de l' [enregistreur de performances Windows (WPR)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh448205(v=win.10)) à partir de l’appareil hôte.

![Page de suivi des performances du portail d’appareils](images/device-portal/mob-device-portal-perf-tracing.png)

- **Profils disponibles** : sélectionnez le profil WPR dans la liste déroulante, puis cliquez ou appuyez sur **Démarrer** pour commencer le suivi.
- **Profils personnalisés** : cliquez ou appuyez sur **Parcourir** pour choisir un profil WPR depuis votre PC. Cliquez ou appuyez sur **Charger et démarrer** pour commencer le suivi.

Pour arrêter le suivi, cliquez sur **Arrêter**. Restez sur cette page jusqu’au fichier de trace (. ETL) a terminé le téléchargement.

Renvoyé. Les fichiers ETL peuvent être ouverts à des fins d’analyse dans l' [Analyseur de performances Windows](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh448170(v=win.10)).

### <a name="device-manager"></a>Gestionnaire de périphériques

La page Gestionnaire de périphériques énumère tous les périphériques connectés à votre appareil. Vous pouvez cliquer sur les icônes de paramètres pour afficher les propriétés de chacune d’elles.

![Page Device Portal Device Manager](images/device-portal/mob-device-portal-devices.png)

### <a name="networking"></a>Mise en réseau

La page mise en réseau gère les connexions réseau sur l’appareil. À moins que vous ne soyez connecté au portail des appareils via USB, la modification de ces paramètres vous déconnectera probablement du portail de l’appareil.

- **Réseaux disponibles**: affiche les réseaux Wi-Fi disponibles pour l’appareil. Appuyez ou cliquez sur un réseau pour vous y connecter et fournir une clé d’accès si nécessaire. Le portail des appareils ne prend pas encore en charge l’authentification d’entreprise. Vous pouvez également utiliser la liste déroulante des **profils** pour tenter de vous connecter à l’un des profils WiFi connus de l’appareil.
- **Configuration IP**: affiche des informations d’adresse sur chacun des ports réseau de l’appareil hôte.

![Page mise en réseau du portail des appareils](images/device-portal/mob-device-portal-network.png)

## <a name="service-features-and-notes"></a>Fonctionnalités du service et notes

### <a name="dns-sd"></a>DNS-SD

Device Portal signale sa présence sur le réseau local à l’aide de DNS-SD. Toutes les instances Device Portal, quel que soit le type d’appareil, sont signalées sous « WDP._wdp._tcp.local ». Les enregistrements TXT relatifs à l’instance de service fournissent les éléments suivants :

Clé | Type | Description
----|------|-------------
S | entier | Port sécurisé pour Device Portal. Si la valeur est égale à 0 (zéro), Device Portal ne détecte pas les connexions HTTPS.
D | chaîne | Type d’appareil. Le format est « Windows. * », par exemple, Windows. Xbox ou Windows. Desktop
A | chaîne | Architecture d’appareil. Par exemple ARM, x 86 ou AMD64.  
T | liste de chaînes délimitées par des caractères Null | Balises appliquées par l’utilisateur pour l’appareil. Reportez-vous à l’API REST Tags pour apprendre à l’utiliser. La liste se termine par deux caractères Null.  

La connexion au port HTTPS est suggérée, car les appareils ne sont pas tous détectés sur le port HTTP signalé par l’enregistrement DNS-SD.

### <a name="csrf-protection-and-scripting"></a>Protection CSRF et écriture de scripts

Afin d’offrir une protection contre les [attaques CSRF](https://en.wikipedia.org/wiki/Cross-site_request_forgery), un jeton unique est requis pour toutes les demandes non GET. Ce jeton, qui est l’en-tête de demande X-CSRF-Token, dérive d’un cookie de session, CSRF-Token. Dans l’interface utilisateur Device Portal, le cookie CSRF-Token est copié dans l’en-tête X-CSRF-Token à chaque demande.

> [!IMPORTANT]
> Cette protection empêche les utilisations des API REST à partir d’un client autonome (tels que les utilitaires de ligne de commande). Cette situation peut être résolue de 3 manières différentes :
> - Utilisez un nom d’utilisateur « automatique ». Les clients qui font précéder leur nom d’utilisateur du préfixe « auto-» contournent la protection CSRF. Ce nom d’utilisateur ne doit pas servir à se connecter à Device Portal par le biais du navigateur, car il rend le service vulnérable aux attaques CSRF. Exemple : Si le nom d’utilisateur Device Portal est « admin », ```curl -u auto-admin:password <args>``` doit être utilisé pour contourner la protection CSRF.
> - Implémentez le schéma de type cookie vers en-tête dans le client. Cette opération nécessite une requête GET afin d’établir le cookie de session, puis l’inclusion de l’en-tête et du cookie sur toutes les requêtes ultérieures.
> - Désactivez l’authentification et utilisez le protocole HTTP. La protection CSRF s’applique uniquement aux points de terminaison HTTPS : les connexions au niveau des points de terminaison HTTP n’ont donc pas besoin de satisfaire les conditions ci-dessus.

#### <a name="cross-site-websocket-hijacking-cswsh-protection"></a>Protection CSWSH (Cross-Site WebSocket Hijacking)

Afin d’éliminer les risques d’[attaques CSWSH](https://www.christian-schneider.net/CrossSiteWebSocketHijacking.html), tous les clients ouvrant une connexion WebSocket à Device Portal doivent également fournir un en-tête Origin correspondant à l’en-tête Host. Cela prouve à Device Portal que la requête provient soit de l’interface utilisateur de Device Portal, soit d’une application cliente valide. Si la requête ne présente pas d’en-tête Origin, elle sera rejetée.
