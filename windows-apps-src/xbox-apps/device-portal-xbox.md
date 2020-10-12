---
ms.assetid: bf0a8b01-79f1-4944-9d78-9741e235dbe9
title: Device Portal pour Xbox
description: Découvrez comment activer le portail d’appareil Xbox pour Xbox One, qui vous permet d’accéder à distance à votre Xbox de développement.
ms.date: 04/09/2019
ms.topic: article
keywords: windows 10, uwp, portail d’appareil
ms.localizationpriority: medium
ms.openlocfilehash: a7df29b6c1446d65c8e5224eede3030a25888364
ms.sourcegitcommit: 53c00939b20d4b0a294936df3d395adb0c13e231
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91933170"
---
# <a name="device-portal-for-xbox"></a>Device Portal pour Xbox

## <a name="set-up-device-portal-on-xbox"></a>Configurer Device Portal pour Xbox

Les étapes suivantes montrent comment activer le portail de périphérique Xbox, qui vous permet d’accéder à distance à votre Xbox de développement.

1. Ouvrez Dev Home. Cela devrait s’ouvrir par défaut lorsque vous démarrez votre Xbox de développement, mais vous pouvez également l’ouvrir à partir de l’écran d’accueil.

    ![Device Portal DevHome](images/device-portal-xbox-1.png)

2. Dans la page d’hébergement dev, sous l’onglet dossier de **démarrage** , sous **accès à distance**, sélectionnez **paramètres d’accès à distance**.

    ![Outil RemoteManagement du portail de l’appareil](images/device-portal-xbox-15.png)

3. Cochez la case **activer le portail du périphérique Xbox** .

4. Sous **authentification**, sélectionnez **définir le nom d’utilisateur et le mot de passe**. Entrez un **nom d’utilisateur** et un **mot de passe** à utiliser pour authentifier l’accès à votre kit de développement à partir d’un navigateur, puis **Enregistrez** -les.

5. **Fermez** la page **accès à distance** et notez l’URL listée sous **accès à distance** sous l’onglet dossier de **démarrage** .

6. Entrez l’URL dans votre navigateur, puis connectez-vous avec les informations d’identification que vous avez configurées.

7. Vous recevrez un avertissement sur le certificat fourni, comme indiqué ci-dessous. Dans Edge, cliquez sur **Détails** , puis **accédez à la page Web** pour accéder au portail de l’appareil Xbox. Dans la boîte de dialogue qui s’affiche, entrez le nom d’utilisateur et le mot de passe que vous avez entrés précédemment sur votre Xbox.

    ![Erreur de certificat Device Portal](images/device-portal-xbox-3.png)

## <a name="device-portal-pages"></a>Pages Device Portal

Le portail d’appareils Xbox fournit un ensemble de pages standard similaires à celles disponibles sur le portail des appareils Windows, ainsi que plusieurs pages uniques. Pour obtenir une description détaillée de la première, consultez [vue d’ensemble du portail de périphériques Windows](../debug-test-perf/device-portal.md). Les sections suivantes décrivent les pages qui sont propres au portail de périphérique Xbox.

### <a name="home"></a>page d'accueil

À l’instar de la page **Gestionnaire d’applications** du portail de périphérique Windows, **la page d’installation du portail** de périphérique Xbox affiche la liste des jeux et applications installés sous **mes jeux & applications**. Vous pouvez cliquer sur le nom d’un jeu ou d’une application pour afficher plus de détails, tels que le nom de la **famille de packages**. Dans la liste déroulante **actions** , vous pouvez agir sur le jeu ou l’application, par exemple le **lancer** .

Sous **comptes Xbox Live test**, vous pouvez gérer les comptes associés à votre Xbox. Vous pouvez ajouter des utilisateurs et des comptes invités, créer des utilisateurs, signer des utilisateurs et supprimer des comptes.

![page d'accueil](images/device-portal-xbox-16.png)

### <a name="xbox-live-game-saves"></a>Xbox Live (enregistrements de jeux)

Le portail de l’appareil Windows et le portail de l’appareil Xbox disposent d’une page **Xbox Live** . Toutefois, le portail de l’appareil Xbox a une section unique, **Xbox Live, enregistre**, dans laquelle vous pouvez enregistrer des données pour les jeux installés sur votre Xbox. Entrez l' **ID de configuration de service (SCID)** (consultez [configuration du service Xbox Live](/gaming/xbox-live/xbox-live-service-configuration.md#get-your-ids) pour plus d’informations), **MemberName (MSA)** et nom de la **famille de packages (PFN)** associés au titre et à l’enregistrement du jeu, recherchez le **fichier d’entrée (. JSON ou. Xml)**, puis sélectionnez l’un des boutons (**Réinitialiser**, **Importer**, **Exporter**et **supprimer**).

Dans la section **générer** , vous pouvez générer des données factices et les enregistrer dans le fichier d’entrée spécifié. Entrez simplement les **conteneurs (par défaut 2)**, les **objets BLOB (valeur par défaut 3)** et la taille de l' **objet BLOB (par défaut 1024)**, puis sélectionnez **générer**.

![Xbox Live](images/device-portal-xbox-17.png)

### <a name="http-monitor"></a>Moniteur HTTP

Le moniteur HTTP vous permet d’afficher le trafic HTTP et HTTPs déchiffré à partir de votre application ou de votre jeu lorsqu’il est exécuté sur votre Xbox.

![Moniteur HTTP](images/device-portal-xbox-18.png)

Pour l’activer, ouvrez la page d’hébergement dev sur votre Xbox, accédez à l’onglet **paramètres** et, dans la zone **paramètres du moniteur http** , cochez **activer le moniteur http**.

![Page d’hébergement dev : mise en réseau](images/device-portal-xbox-14.png)

Une fois activé, dans le portail de l’appareil Xbox, vous pouvez **arrêter**, **Effacer**et enregistrer le trafic HTTP et https dans le **fichier** en sélectionnant les boutons correspondants.

### <a name="network-fiddler-tracing"></a>Réseau (suivi Fiddler)

La page **réseau** dans le portail de l’appareil Xbox est presque identique à la page **mise en réseau** dans le portail des appareils Windows, à l’exception du **suivi Fiddler**, qui est unique sur le portail de l’appareil Xbox. Cela vous permet d’exécuter Fiddler sur votre PC pour consigner et inspecter le trafic HTTP et HTTPs entre votre Xbox et Internet. Pour plus d’informations, consultez [comment utiliser Fiddler avec Xbox One lors du développement pour UWP](../xbox-apps/uwp-fiddler.md) .

![Réseau](images/device-portal-xbox-19.png)

### <a name="media-capture"></a>Capture multimédia

Sur la page **capture du média** , vous pouvez sélectionner capturer la capture d' **écran** pour prendre une capture d’écran de votre Xbox. Une fois que vous avez fait, votre navigateur vous invite à télécharger le fichier. Vous pouvez vérifier **préférer HDR** si vous souhaitez prendre la capture d’écran en HDR (si la console le prend en charge).

![Capture multimédia](images/device-portal-xbox-12.png)

### <a name="settings"></a>Paramètres

Sur la page **paramètres** , vous pouvez afficher et modifier plusieurs paramètres pour votre Xbox. En haut, vous pouvez sélectionner **Importer** pour importer les paramètres d’un fichier et les **Exporter** pour exporter les paramètres actuels dans un fichier. txt. L’importation de paramètres peut faciliter la modification en bloc, en particulier lors de la configuration de plusieurs consoles. Pour créer un fichier de paramètres à importer, définissez les paramètres sur la manière dont vous souhaitez qu’ils soient, puis exportez les paramètres. Vous pouvez ensuite utiliser ce fichier pour importer des paramètres rapidement et facilement pour d’autres consoles.

Il existe plusieurs sections avec différents paramètres pour afficher et/ou modifier, qui sont expliqués ci-dessous.

![Capture d’écran de la page Paramètres affichant les sections informations sur l’appareil et paramètres d’affichage.](images/device-portal-xbox-20.png)

![Capture d’écran de la page Paramètres montrant les sections paramètres de localisation, paramètres d’alimentation et paramètres utilisateur. ](images/device-portal-xbox-21.png)

#### <a name="device-information"></a>Informations sur l’appareil

* **Nom**de l’appareil : nom de l’appareil. Pour modifier, modifiez le nom dans la zone, puis sélectionnez **Enregistrer**.

* **Version du système d’exploitation**: en lecture seule. Numéro de version du système d’exploitation.

* **Édition du système d’exploitation**: en lecture seule. Nom de la version principale du système d’exploitation.

* **ID de périphérique Xbox Live**: en lecture seule.

* **ID de console**: en lecture seule.

* **Numéro de série**: en lecture seule.

* **Type de console**: en lecture seule. Type d’appareil Xbox One (Xbox One, Xbox 1 S ou Xbox One X).

* **Mode dev**: lecture seule. Mode développeur dans lequel se trouve l’appareil.

#### <a name="audio-settings"></a>Paramètres audio

* **Format de flux binaire audio**: format des données audio.

* **HDMI audio**: type de l’audio par le biais du port HDMI.

* **Format du casque**: format de l’audio fourni par le casque.

* **Audio optique**: type de l’audio par le biais du port optique.

* **Utiliser HDMI ou casque audio optique**: Cochez cette case si vous utilisez un casque connecté via une connexion HDMI ou optique.

#### <a name="display-settings"></a>Paramètres d'affichage

* **Profondeur de couleur**: nombre de bits utilisés pour chaque composant de couleur d’un pixel unique.

* **Espace colorimétrique**: la gamme de couleurs disponible pour l’affichage.

* **Résolution d’affichage**: résolution de l’affichage.

* **Afficher la connexion**: type de connexion à l’affichage.

* **Autoriser la plage dynamique élevée (HDR)**: active HDR sur l’écran. Disponible uniquement pour les affichages compatibles.

* **Autoriser 4k**: active la résolution 4k sur l’affichage. Disponible uniquement pour les affichages compatibles.

* **Autoriser le taux d’actualisation des variables (VRR)**: activer VRR sur l’affichage. Disponible uniquement pour les affichages compatibles.

#### <a name="kinect-settings"></a>Paramètres Kinect

Un capteur Kinect doit être connecté à la console pour pouvoir modifier ces paramètres.

* **Enable Kinect**: active le capteur Kinect attaché.

* **Forcer le rechargement des Kinect lors de la modification de l’application**: recharger le capteur Kinect attaché chaque fois qu’une application ou un jeu différent est exécuté.

#### <a name="localization-settings"></a>Paramètres de localisation

* **Région géographique**: région géographique à laquelle l’appareil est défini. Il doit s’agir du code de pays à 2 caractères spécifique (par exemple, **US** pour États-Unis).

* **Langue (s) par défaut**: langue sur laquelle l’appareil est défini.

* **Fuseau horaire**: fuseau horaire sur lequel l’appareil est défini.

#### <a name="network-settings"></a>Paramètres réseau

* **Paramètres radio sans fil**: paramètres sans fil de l’appareil (si certains aspects tels que le réseau local sans fil sont activés ou désactivés).

#### <a name="power-settings"></a>Paramètres d’alimentation

* **En cas d’inactivité, écran sombre après (minutes)**: l’écran est estompé une fois que l’appareil est resté inactif pendant cette durée. Affectez la valeur **0** pour ne jamais estomper l’écran.

* **En cas d’inactivité,** désactivez après : l’appareil s’arrêtera une fois qu’il est resté inactif pendant ce laps de temps.

* **Mode d’alimentation**: mode d’alimentation de l’appareil. Pour plus d’informations, consultez [à propos de l’économie d’énergie et des modes d’alimentation instantanés](https://support.xbox.com/xbox-one/console/learn-about-power-modes) .

* **Démarrer automatiquement la console lorsqu’elle est connectée à**une source d’alimentation : l’appareil s’allume automatiquement lorsqu’il est connecté à une source d’alimentation.

#### <a name="preference-settings"></a>Paramètres de préférence

* **Expérience d’accueil par défaut**: définit l’écran d’accueil qui s’affiche lorsque l’appareil est allumé.

* **Autoriser les connexions à partir de l’application Xbox**: l’application Xbox sur un autre appareil (tel qu’un PC Windows 10) peut se connecter à cette console.

* **Considérer les applications UWP comme des jeux par défaut**: les jeux et les applications obtiennent différentes ressources qui leur sont alloués sur Xbox. Si vous activez cette case à cocher, tous les packages UWP seront identifiés comme des jeux et obtiendront ainsi plus de ressources.

#### <a name="user-settings"></a>Paramètres utilisateur

* **Utilisateur de connexion automatique**: se connecte automatiquement à l’utilisateur sélectionné lorsque l’appareil est allumé.

* **Connexion automatique au contrôleur utilisateur**: associe automatiquement un type de contrôleur particulier à un utilisateur particulier.

#### <a name="xbox-live-sandbox"></a>Bac à sable Xbox Live

Ici, vous pouvez modifier le bac à sable Xbox Live dans lequel se trouve l’appareil. Entrez le nom du bac à sable (sandbox) dans la zone, puis sélectionnez **modifier**.

### <a name="scratch"></a>Vide

Il s’agit d’un espace de travail vide que vous pouvez personnaliser à votre convenance. Vous pouvez utiliser le menu (cliquez sur le bouton de menu en haut à gauche) pour ajouter des outils (sélectionnez **Ajouter des outils à l’espace de travail**, puis les outils que vous souhaitez ajouter, puis **Ajouter**). Notez que vous pouvez utiliser ce menu pour ajouter des outils à n’importe quel espace de travail, ainsi que pour gérer les espaces de travail eux-mêmes.

![Ajouter des outils à l’espace de travail](images/device-portal-xbox-13.png)

### <a name="game-event-data"></a>Données d’événement de jeu

Sur la page **données d’événement de jeu** , vous pouvez afficher un graphique en temps réel qui diffuse le nombre d’événements de jeu de suivi d’V nements pour Windows (ETW) actuellement enregistrés sur votre Xbox. Si des événements de jeu sont enregistrés sur le système, vous pouvez également afficher des détails (nom de l’événement, occurrence de l’événement et titre du jeu) décrivant chaque événement dans une table de données sous le graphique de données. La table est disponible uniquement si des événements sont enregistrés.

![Données d’événement de jeu](images/device-portal-xbox-22.PNG)

## <a name="see-also"></a>Voir aussi

* [Vue d’ensemble du Portail d'appareil Windows](../debug-test-perf/device-portal.md)
* [Informations de référence sur les API principales Device Portal](../debug-test-perf/device-portal-api-core.md)