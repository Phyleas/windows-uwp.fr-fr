---
ms.assetid: 60fc48dd-91a9-4dd6-a116-9292a7c1f3be
title: Vue d’ensemble de Windows Device Portal
description: Découvrez comment Windows Device Portal vous permet de configurer et de gérer à distance votre appareil par le biais d’une connexion réseau ou USB.
ms.date: 04/09/2019
ms.topic: article
keywords: windows 10, uwp, portail d’appareil
ms.localizationpriority: medium
ms.openlocfilehash: 2292d97166d34905bb895aa3f53f864510a21f46
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "74254760"
---
# <a name="windows-device-portal-overview"></a>Vue d’ensemble de Windows Device Portal

Windows Device Portal vous permet de configurer et de gérer à distance votre appareil par le biais d’une connexion réseau ou USB. Il fournit également des outils de diagnostic avancés pour vous permettre de résoudre les problèmes et d’afficher les performances en temps réel de votre appareil Windows.

Le portail d'appareil Windows est un serveur web sur l’appareil auquel vous pouvez vous connecter à partir d’un navigateur web sur un PC. Si votre appareil dispose d’un navigateur web, vous pouvez également vous connecter localement avec le navigateur sur cet appareil.

Windows Device Portal est disponible sur chaque famille d’appareils. Toutefois, les fonctionnalités et la configuration varient en fonction des exigences de chaque appareil. Cet article fournit une description générale de Device Portal et des liens vers des articles contenant des informations plus spécifiques pour chaque famille d’appareils.

La fonctionnalité du portail d'appareil Windows est implémentée avec des [API REST](device-portal-api-core.md) que vous pouvez utiliser directement pour accéder aux données et contrôler programmatiquement votre appareil.

## <a name="setup"></a>Installation

Chaque appareil possède des instructions spécifiques concernant la connexion à Device Portal. Toutefois, chacun nécessite d’effectuer les étapes générales suivantes.

1. Activez le mode développeur et le portail d'appareil sur votre appareil (configuré dans l’application Paramètres).

2. Connectez votre appareil et votre PC via un réseau local ou USB.

3. Accéder à la page Device Portal dans votre navigateur. Le tableau suivant répertorie les ports et protocoles utilisés par chaque famille d’appareils.

Famille d’appareils | Activé par défaut ? | HTTP | HTTPS | USB
--------------|----------------|------|-------|----
HoloLens | Oui, en mode de développement | 80 (par défaut) | 443 (par défaut) | http://127.0.0.1:10080
IoT | Oui, en mode de développement | 8080 | Activer via la clé de registre | NON APPLICABLE
Xbox | Activer dans le mode de développement | Désactivé | 11443 | NON APPLICABLE
Desktop (Expérience utilisateur)| Activer dans le mode de développement | 50080\* | 50043\* | NON APPLICABLE
Téléphone | Activer dans le mode de développement | 80| 443 | http://127.0.0.1:10080

\* Cela n’est pas toujours le cas, car le portail d'appareil sur le bureau revendique des ports dans la plage éphémère (> 50 000) afin d’éviter les collisions avec les déclarations de port existant sur l’appareil. Pour plus d’informations, consultez la section [Paramètres de port](device-portal-desktop.md#registry-based-configuration-for-device-portal) pour le bureau.  

Pour obtenir des instructions d’installation propres à chaque appareil, consultez :

- [Portail d’appareil pour HoloLens](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-hololens)
- [Portail d’appareil pour IoT](https://docs.microsoft.com/windows/iot-core/manage-your-device/DevicePortal)
- [Portail d’appareil pour appareils mobiles](device-portal-mobile.md)
- [Portail d’appareil pour Xbox](../xbox-apps/device-portal-xbox.md)
- [Portail d’appareil pour Bureau](device-portal-desktop.md#set-up-device-portal-on-windows-desktop)

## <a name="features"></a>Fonctionnalités

### <a name="toolbar-and-navigation"></a>Barre d’outils et navigation

La barre d’outils se trouvant en haut de la page permet d’accéder aux fonctionnalités couramment utilisées.

- **Alimentation** : permet d’accéder aux options d’alimentation.
  - **Arrêt** : éteint l’appareil.
  - **Redémarrer** : mise sous tension de l’appareil par cycle.
- **Aide** : ouvre la page d’aide.

Utilisez les liens du volet de navigation sur le côté gauche de la page pour naviguer vers les outils d’analyse et de gestion disponibles pour votre appareil.

Les outils courants des familles d’appareils sont décrits ici. D’autres options peuvent être disponibles selon l’appareil. Pour plus d’informations, voir la page spécifique de votre type d’appareil.

### <a name="apps-manager"></a>Gestionnaire d’applications

Le gestionnaire d’applications propose des fonctionnalités d’installation/de désinstallation et de gestion pour les packages d’applications et les offres groupées sur l’appareil hôte.

![Page Gestionnaire d’applications du portail d’appareil](images/device-portal/WDP_AppsManager2.png)

* **Déployer des applications** : déployez des applications packagées à partir d’hôtes locaux, réseau ou web et inscrivez des fichiers libres à partir de partages réseau.
* **Applications installées** : utilisez le menu déroulant pour supprimer ou démarrer les applications installées sur l’appareil.
* **Applications en cours d’exécution** : obtenez des informations sur les applications en cours d’exécution et fermez-les si nécessaire.

#### <a name="install-sideload-an-app"></a>Installer une application (charger une version test)

Vous pouvez charger des versions test d’applications pendant le développement à l’aide du portail d’appareils Windows :

1. Lorsque vous avez créé un package d’application, vous pouvez l’installer à distance sur votre appareil. Une fois créé dans Visual Studio, un dossier de sortie est généré.

    ![Installation d’applications](images/device-portal/iot-installapp0.png)

2. Dans le portail d’appareils Windows, accédez à la page **Gestionnaire d’applications**.

3. Dans la section **Déployer des applications**, sélectionnez **Stockage local**.

4. Sous **Sélectionner le package d’application**, sélectionnez **Choisir un fichier** et accédez au package d’application dont vous souhaitez charger une version test.

5. Sous **Sélectionnez le fichier de certificat (.cer) utilisé pour signer le package d’application**, sélectionnez **Choisir un fichier** et accédez au certificat associé à ce package d’application.

6. Activez les cases à cocher respectives si vous souhaitez installer des packages facultatifs ou de framework en même temps que l’application, puis sélectionnez **Suivant** pour les choisir.

7. Sélectionnez **Installer** pour démarrer l'installation.

8. Si l’appareil exécute Windows 10 en mode S et que ce certificat a été installé pour la première fois sur l’appareil, redémarrez le service.

#### <a name="install-a-certificate"></a>Installer un certificat

Vous pouvez également installer le certificat via le Portail d'appareil Windows et installer l’application par d’autres moyens :

1. Dans le portail d’appareils Windows, accédez à la page **Gestionnaire d’applications**.

2. Dans la section **Déployer des applications**, sélectionnez **Installer un certificat**.

3. Sous **Sélectionnez le fichier de certificat (.cer) utilisé pour signer un package d’application**, sélectionnez **Choisir un fichier** et accédez au certificat associé au package d’application dont vous souhaitez charger une version test.

4. Sélectionnez **Installer** pour démarrer l'installation.

5. Si l’appareil exécute Windows 10 en mode S et que ce certificat a été installé pour la première fois sur l’appareil, redémarrez le service.

#### <a name="uninstall-an-app"></a>Désinstaller une application

1. Assurez-vous que votre application n’est pas en cours d’exécution.
2. Si c’est le cas, accédez à **Applications en cours d’exécution** et fermez l’application en question. Si vous essayez de désinstaller une application en cours d’exécution, celle-ci posera des problèmes lorsque vous essaierez de la réinstaller.
3. Sélectionnez l’application dans la liste déroulante, puis cliquez sur **Supprimer**.

### <a name="running-processes"></a>Processus en cours d’exécution

Cette page affiche des détails sur les processus en cours d’exécution sur l’appareil hôte. Cela comprend les processus relatifs aux applications au système. Sur certaines plateformes (Desktop, IoT et HoloLens), vous pouvez mettre fin aux processus.

![Page des processus d’exécution du portail d’appareil](images/device-portal/mob-device-portal-processes.png)

### <a name="file-explorer"></a>Explorateur de fichiers

Cette page vous permet d’afficher et de manipuler les fichiers stockés par vos applications dont une version test a été chargée. Pour en savoir plus sur l’Explorateur de fichiers et comment l’utiliser, consultez le billet de blog [Utilisation de l’Explorateur de fichiers de l’application](https://blogs.windows.com/buildingapps/2016/06/08/using-the-app-file-explorer-to-see-your-app-data/).

![Page Explorateur de fichiers du portail d’appareil](images/device-portal/mob-device-portal-AppFileExplorer.png)

### <a name="performance"></a>Performances

La page des performances présente des graphiques en temps réel des informations de diagnostic système, telles que la consommation d’énergie, la fréquence d’images et la charge du processeur.

Voici les mesures disponibles :

- **Processeur** : pourcentage de l’utilisation totale du processeur disponible
- **Mémoire** : totale, en cours d’utilisation, disponible, validée, paginée et non paginée
- **E/S** : lire et écrire des quantités de données
- **Réseau** : données reçues et envoyées
- **GPU** : pourcentage de l’utilisation totale du moteur GPU disponible

![Page des performances du Portail d'appareil](images/device-portal/mob-device-portal-perf.png)

### <a name="event-tracing-for-windows-etw-logging"></a>Journalisation du suivi d’événements pour Windows (ETW)

La page de journalisation ETW gère les informations de suivi d’événements pour Windows (ETW) en temps réel sur l’appareil.

![Page de journalisation ETW du Portail d’appareil](images/device-portal/mob-device-portal-etw.png)

Cochez la case **Masquer les fournisseurs** pour n’afficher que la liste des événements.

- **Fournisseurs inscrits** : sélectionnez le fournisseur d’événements et le niveau de suivi. Le niveau de suivi est l’une des valeurs suivantes :
  1. Sortie ou arrêt anormal
  2. Erreurs graves
  3. Avertissements
  4. Avertissements sans erreur
  5. Suivi détaillé

  Cliquez ou appuyez sur **Activer** pour démarrer le suivi. Le fournisseur est ajouté à la liste déroulante **Fournisseurs activés**.
- **Fournisseurs personnalisés** : sélectionnez un fournisseur ETW personnalisé et le niveau de suivi. Identifiez le fournisseur par son GUID. N’insérez pas de crochets dans le GUID.
- **Fournisseurs activés** : fournit une liste des fournisseurs activés. Sélectionnez un fournisseur dans la liste déroulante, puis cliquez sur ou appuyez sur **Désactiver** pour arrêter le suivi. Cliquez ou appuyez sur **Arrêter tout** pour suspendre tout le suivi.
- **Historique des fournisseurs** : affiche les fournisseurs ETW activés pendant la session active. Cliquez ou appuyez sur **Activer** pour activer un fournisseur qui a été désactivé. Cliquez ou appuyez sur **Effacer** pour supprimer l’historique.
- **Filtres/Événements** : la section **Événements** répertorie les événements ETW des fournisseurs sélectionnés sous forme de tableau. Le tableau est mis à jour en temps réel. Utilisez le menu **Filtres** pour configurer des filtres personnalisés pour les événements à afficher. Cliquez sur le bouton **Effacer** pour supprimer tous les événements ETW du tableau. Cela ne désactive pas les fournisseurs. Vous pouvez cliquer sur **Enregistrer dans le fichier** pour exporter les événements ETW actuellement collectés vers un fichier CSV local.

Pour plus d’informations sur l’utilisation de la journalisation ETW, consultez le billet de blog [Utiliser le Portail d’appareil pour afficher les journaux de débogage](https://blogs.windows.com/buildingapps/2016/06/10/using-device-portal-to-view-debug-logs-for-uwp/).

### <a name="performance-tracing"></a>Suivi des performances

La page Suivi des performances vous permet d’afficher les suivis de l’[Enregistreur de performances Windows (WPR)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh448205(v=win.10)) à partir de l’appareil hôte.

![Page de suivi des performances du Portail d'appareil](images/device-portal/mob-device-portal-perf-tracing.png)

- **Profils disponibles** : sélectionnez le profil WPR dans la liste déroulante, puis cliquez ou appuyez sur **Démarrer** pour commencer le suivi.
- **Profils personnalisés** : cliquez ou appuyez sur **Parcourir** pour choisir un profil WPR sur votre PC. Cliquez ou appuyez sur **Charger et démarrer** pour commencer le suivi.

Pour arrêter le suivi, cliquez sur **Arrêter**. Restez sur cette page jusqu’à ce que le fichier de suivi (.ETL) soit entièrement téléchargé.

Les fichiers .ETL capturés peuvent être ouverts pour analyse dans [Windows Performance Analyzer](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh448170(v=win.10)).

### <a name="device-manager"></a>Gestionnaire de périphériques

La page Gestionnaire d’appareils répertorie tous les périphériques connectés à votre appareil. Vous pouvez cliquer sur les icônes de paramètres pour afficher les propriétés de chacun d’eux.

![Page Gestionnaire d’appareils du Portail d’appareil](images/device-portal/mob-device-portal-devices.png)

### <a name="networking"></a>Mise en réseau

La page Réseaux gère les connexions réseau sur l’appareil. Sauf si vous êtes connecté à Device Portal via USB, la modification de ces paramètres entraînera certainement la déconnexion de Device Portal.

- **Réseaux disponibles** : affiche les réseaux Wi-Fi disponibles sur l’appareil. Appuyez ou cliquez sur un réseau pour vous y connecter et fournir une clé d’accès si nécessaire. Le Portail d'appareil ne prend pas encore en charge l’authentification d’entreprise. Vous pouvez également utiliser la liste déroulante **Profils** pour tenter de vous connecter à l’un des profils Wi-Fi connus de l’appareil.
- **Configuration IP** : affiche des informations d’adresse sur chacun des ports réseau de l’appareil hôte.

![Page Mise en réseau du Portail d'appareil](images/device-portal/mob-device-portal-network.png)

## <a name="service-features-and-notes"></a>Remarques et fonctionnalités de service

### <a name="dns-sd"></a>DNS-SD

Device Portal signale sa présence sur le réseau local à l’aide de DNS-SD. Toutes les instances Device Portal, quel que soit le type d’appareil, sont signalées sous « WDP._wdp._tcp.local ». Les enregistrements TXT relatifs à l’instance de service fournissent les éléments suivants :

Clé | Type | Description
----|------|-------------
S | int | Port sécurisé pour Device Portal. Si la valeur est égale à 0 (zéro), Device Portal ne détecte pas les connexions HTTPS.
D | string | Type d’appareil. Cette information sera au format « Windows.* », par exemple Windows.Xbox ou Windows.Desktop
A | string | Architecture d’appareil. Par exemple ARM, x 86 ou AMD64.  
T | liste de chaînes délimitées par des caractères Null | Balises appliquées par l’utilisateur pour l’appareil. Reportez-vous à l’API REST Tags pour apprendre à l’utiliser. La liste se termine par deux caractères Null.  

La connexion au port HTTPS est suggérée, car les appareils ne sont pas tous détectés sur le port HTTP signalé par l’enregistrement DNS-SD.

### <a name="csrf-protection-and-scripting"></a>Protection CSRF et écriture de scripts

Afin d’offrir une protection contre les [attaques CSRF](https://en.wikipedia.org/wiki/Cross-site_request_forgery), un jeton unique est requis pour toutes les demandes non GET. Ce jeton, qui est l’en-tête de demande X-CSRF-Token, dérive d’un cookie de session, CSRF-Token. Dans l’interface utilisateur Device Portal, le cookie CSRF-Token est copié dans l’en-tête X-CSRF-Token à chaque demande.

> [!IMPORTANT]
> Cette protection empêche toute utilisation des API REST à partir d’un client autonome (tel que les utilitaires de ligne de commande). Cette situation peut être résolue de 3 manières différentes :
> - Utilisez un nom d’utilisateur « auto- ». Les clients qui font précéder leur nom d’utilisateur du préfixe « auto-» contournent la protection CSRF. Ce nom d’utilisateur ne doit pas servir à se connecter à Device Portal par le biais du navigateur, car il rend le service vulnérable aux attaques CSRF. Exemple : Si le nom d’utilisateur du Portail d'appareil est « admin », ```curl -u auto-admin:password <args>``` doit être utilisé pour contourner la protection CSRF.
> - Implémentez le schéma de type cookie vers en-tête dans le client. Cette opération nécessite une requête GET afin d’établir le cookie de session, puis l’inclusion de l’en-tête et du cookie sur toutes les requêtes ultérieures.
> - Désactivez l’authentification et utilisez le protocole HTTP. La protection CSRF s’applique uniquement aux points de terminaison HTTPS : les connexions au niveau des points de terminaison HTTP n’ont donc pas besoin de satisfaire les conditions ci-dessus.

#### <a name="cross-site-websocket-hijacking-cswsh-protection"></a>Protection CSWSH (Cross-Site WebSocket Hijacking)

Afin d’éliminer les risques d’[attaques CSWSH](https://www.christian-schneider.net/CrossSiteWebSocketHijacking.html), tous les clients ouvrant une connexion WebSocket à Device Portal doivent également fournir un en-tête Origin correspondant à l’en-tête Host. Cela prouve à Device Portal que la requête provient soit de l’interface utilisateur de Device Portal, soit d’une application cliente valide. Si la requête ne présente pas d’en-tête Origin, elle sera rejetée.
