---
title: Lancer l’application Paramètres Windows
description: Découvrez comment lancer l’application Paramètres Windows à partir de votre application. Cette rubrique décrit le schéma d’URI ms-settings. Utilisez ce schéma d’URI pour lancer l’application Paramètres Windows en ouvrant des pages de paramètres spécifiques.
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
ms.date: 04/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: 19H1
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: d90669e03ae15acdc826d9e0b227f12d4ecf3cbc
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304711"
---
# <a name="launch-the-windows-settings-app"></a>Lancer l’application Paramètres Windows

**API importantes**

-   [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync)
-   [**PreferredApplicationPackageFamilyName**](/uwp/api/windows.system.launcheroptions.preferredapplicationpackagefamilyname)
-   [**DesiredRemainingView**](/uwp/api/windows.system.launcheroptions.desiredremainingview)

Découvrez comment lancer l’application Paramètres Windows. Cette rubrique décrit le schéma **MS-Settings :** URI. Utilisez ce schéma d’URI pour lancer l’application Paramètres Windows en ouvrant des pages de paramètres spécifiques.

Le lancement de l’application Paramètres est une partie importante de l’écriture d’une application prenant en charge la confidentialité. Si votre application ne peut pas accéder à une ressource sensible, nous vous recommandons de fournir à l’utilisateur un lien pratique lui permettant d’accéder aux paramètres de confidentialité relatifs à cette ressource. Pour plus d’informations, voir [Recommandations en matière d’applications prenant en charge la confidentialité](../security/index.md).

## <a name="how-to-launch-the-settings-app"></a>Comment lancer l’application Paramètres

Pour lancer l’application **Paramètres**, utilisez le schéma d’URI `ms-settings:` comme illustré dans les exemples suivants.

Dans cet exemple, un contrôle Hyperlink XAML est utilisé pour ouvrir la page des paramètres de confidentialité relatifs au microphone en utilisant l’URI `ms-settings:privacy-microphone`.

```xml
<!--Set Visibility to Visible when access to the microphone is denied -->
<TextBlock x:Name="LocationDisabledMessage" FontStyle="Italic"
                 Visibility="Collapsed" Margin="0,15,0,0" TextWrapping="Wrap" >
          <Run Text="This app is not able to access the microphone. Go to " />
              <Hyperlink NavigateUri="ms-settings:privacy-microphone">
                  <Run Text="Settings" />
              </Hyperlink>
          <Run Text=" to check the microphone privacy settings."/>
</TextBlock>
```

Votre application peut également appeler la méthode [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync) pour lancer l’application **paramètres** . Cet exemple montre comment ouvrir la page des paramètres de confidentialité relatifs à l’appareil photo en utilisant l’URI `ms-settings:privacy-webcam`.

```cs
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-webcam"));
```
```cppwinrt
bool result = co_await Windows::System::Launcher::LaunchUriAsync(Windows::Foundation::Uri(L"ms-settings:privacy-webcam"));
```

Le code ci-dessus ouvre la page des paramètres de confidentialité relatifs à l’appareil photo :

![paramètres de confidentialité de l’appareil photo.](images/privacyawarenesssettingsapp.png)

Pour plus d’informations sur les URI de lancement, voir [Lancer l’application par défaut pour un URI](launch-default-app.md).

## <a name="ms-settings-uri-scheme-reference"></a>Référence de schéma d’URI ms-settings:

Utilisez les URI suivants pour ouvrir diverses pages de l’application Paramètres.

> Notez que la disponibilité d’une page de paramètres varie en fonction de la référence SKU Windows. Toutes les pages de paramètres disponibles sur Windows 10 pour PC sont disponibles sur Windows 10 mobile, et vice versa. La colonne notes capture également des exigences supplémentaires qui doivent être remplies pour qu’une page soit disponible.

<!-- TODO: 
* ms-settings:controlcenter
* ms-settings:holographic
* ms-settings:keyboard-advanced
* ms-settings:regionlanguage-adddisplaylanguage (crashed)
* ms-settings:regionlanguage-setdisplaylanguage (crashed)
* ms-settings:signinoptions-launchpinenrollment
* ms-settings:storagecleanup
* ms-settings:update-security -->

## <a name="accounts"></a>Comptes

|Page de paramètres| URI |
|-------------|-----|
| Accès Professionnel ou Scolaire | ms-settings:workplace |
| Comptes de messagerie et d'application  | ms-settings:emailandaccounts |
| Famille et autres personnes | ms-settings:otherusers |
| Configurer une borne | MS-paramètres : assignedaccess |
| Options de connexion | ms-settings:signinoptions<br>MS-Settings : signinoptions-dynamiclock |
| Synchroniser vos paramètres | ms-settings:sync |
| Programme d’installation de Windows Hello | MS-Settings : signinoptions-launchfaceenrollment<br>MS-Settings : signinoptions-launchfingerprintenrollment |
| Vos informations | ms-settings:yourinfo |

## <a name="apps"></a>Apps

|Page de paramètres| URI |
|-------------|-----|
| Applications et fonctionnalités | MS-paramètres : appsfeatures |
| Fonctionnalités de l’application | MS-Settings : appsfeatures-App (réinitialiser, gérer le module complémentaire & le contenu téléchargeable, etc. pour l’application)|
| Applications pour les sites web | MS-paramètres : appsforwebsites |
| Applications par défaut | MS-paramètres : defaultapps |
| Gérer les fonctionnalités facultatives | MS-paramètres : OptionalFeatures |
| Cartes hors connexion | ms-settings:maps<br/>MS-Settings : Maps-downloadmaps (télécharger les mappages) |
| Applications de démarrage | MS-paramètres : startupapps |
| Lecture de vidéo | MS-paramètres : VideoPlayback |

## <a name="cortana"></a>Cortana

|Page de paramètres| URI |
|-------------|-----|
| Cortana sur mes appareils | MS-paramètres : Cortana-notifications |
| Détails supplémentaires | MS-paramètres : Cortana-moredetails |
| Historique des autorisations & | MS-Settings : Cortana-autorisations |
| Recherche de fenêtres | MS-paramètres : Cortana-windowssearch |
| Communiquer avec Cortana | MS-Settings : Cortana-langue<br/>MS-paramètres : Cortana<br/>MS-paramètres : Cortana-talktocortana |

> [!NOTE] 
> Cette section de paramètres sur le bureau sera appelée Rechercher lorsque le PC est défini sur des régions où Cortana n’est pas actuellement disponible ou que Cortana a été désactivée. Les pages spécifiques à Cortana (Cortana sur mes appareils et communiquer avec Cortana) ne sont pas listées dans ce cas. 

## <a name="devices"></a>Appareils

|Page de paramètres| URI |
|-------------|-----|
| Lecture automatique | MS-Settings : exécution automatique |
| Bluetooth | ms-settings:bluetooth |
| Appareils connectés | ms-settings:connecteddevices |
| Appareil photo par défaut | MS-Settings : appareil photo (**déconseillé dans Windows 10, version 1809 et versions ultérieures**) |
| Souris et pavé tactile | MS-Settings : mousetouchpad (paramètres du pavé tactile disponibles uniquement sur les appareils dotés d’un pavé tactile) |
| Stylet & Windows Ink | MS-Settings : stylet |
| Imprimantes & scanneurs | MS-paramètres : imprimantes |
| Pavé tactile | MS-Settings : appareils-Touchpad (disponible uniquement si le matériel du pavé tactile est présent) |
| Saisie | MS-Settings : frappe |
| USB | MS-paramètres : USB |
| Roulette | MS-Settings : Wheel (disponible uniquement si la numérotation est couplée) |
| Votre téléphone | MS-Settings : appareils mobiles  |

## <a name="ease-of-access"></a>Options d’ergonomie

|Page de paramètres| URI |
|-------------|-----|
| Audio | MS-Settings : easeofaccess-audio |
| Sous-titres | ms-settings:easeofaccess-closedcaptioning |
| Filtres de couleur | MS-Settings : easeofaccess-colorfilter |
| Taille du curseur et du pointeur | MS-Settings : easeofaccess-cursorandpointersize |
| Afficher | MS-Settings : easeofaccess-Display |
| Contrôle visuel | MS-Settings : easeofaccess-eyecontrol |
| Polices | MS-paramètres : polices |
| Contraste élevé | ms-settings:easeofaccess-highcontrast |
| Clavier | ms-settings:easeofaccess-keyboard |
| Loupe | ms-settings:easeofaccess-magnifier |
| Souris | ms-settings:easeofaccess-mouse |
| Narrateur | ms-settings:easeofaccess-narrator |
| Autres options | MS-Settings : easeofaccess-otheroptions (**déconseillé dans Windows 10, version 1809 et versions ultérieures**) |
| Voix | MS-Settings : easeofaccess-speechrecognition |

## <a name="extras"></a>Extras

|Page de paramètres| URI |
|-------------|-----|
| Extras | MS-paramètres : Extras (disponible uniquement si les « applications de paramètres » sont installées, par exemple, par un tiers) |

## <a name="gaming"></a>Jeux

|Page de paramètres| URI |
|-------------|-----|
| Diffusion | MS-paramètres : diffusion de jeux |
| Barre de jeu | MS-Settings : jeu-gamebar |
| Jeux DVR | MS-Settings : jeu-gamedvr |
| Mode jeu | MS-Settings : jeu-GameMode |
| Jouer en mode plein écran | MS-paramètres : quietmomentsgame |
| TruePlay | MS-Settings : jeu-trueplay (**à partir de Windows 10, version 1809 (10,0 ; Build 17763), cette fonctionnalité est supprimée de Windows**) |
| Réseau Xbox | MS-Settings : jeu-xboxnetworking |

## <a name="home-page"></a>page d'accueil

|Page de paramètres| URI |
|-------------|-----|
| Page d’hébergement des paramètres | ms-settings: |

## <a name="mixed-reality"></a>Réalité mixte

> [!NOTE]
> Ces paramètres sont disponibles uniquement si l’application portail de réalité mixte est installée.

| Page de paramètres | URI |
|---------------|-----|
| Audio et discours | MS-Settings : holographique-audio |
| Environnement | MS-Settings : confidentialité-holographique-environnement |
| Affichage du casque | MS-Settings : holographique-casque |
| Désinstaller l’interface | MS-Settings : holographique-Management |

## <a name="network--internet"></a>Réseau & Internet

|Page de paramètres| URI |
|-------------|-----|
| Mode avion | ms-settings:network-airplanemode<br/>ms-settings:proximity |
| Réseau cellulaire et SIM | ms-settings:network-cellular |
| Utilisation des données | ms-settings:datausage |
| Accès à distance | ms-settings:network-dialup |
| DirectAccess | MS-Settings : réseau-DirectAccess (disponible uniquement si DirectAccess est activé) |
| Ethernet | ms-settings:network-ethernet |
| Gérer les réseaux connus | MS-Settings : réseau-WiFiSettings |
| Point d’accès sans fil mobile | ms-settings:network-mobilehotspot |
| NFC | ms-settings:nfctransactions |
| Proxy | ms-settings:network-proxy |
| État | ms-settings:network-status<br/>MS-Settings : réseau |
| VPN | MS-Settings : réseau-VPN |
| Wi-Fi | MS-Settings : Network-WiFi (disponible uniquement si l’appareil possède un adaptateur Wi-Fi) |
| Appel Wi-Fi | MS-Settings : Network-wificalling (disponible uniquement si l’appel Wi-Fi est activé) |

## <a name="personalization"></a>Personnalisation

|Page de paramètres| URI |
|-------------|-----|
| Arrière-plan | ms-settings:personalization-background |
| Choisir les dossiers à afficher au démarrage | MS-paramètres : personnalisation-Start-places |
| Couleurs | ms-settings:personalization-colors<br/>MS-paramètres : couleurs |
| Immédiatement | MS-Settings : personnalisation-aperçu (**déconseillé dans Windows 10, version 1809 et versions ultérieures**) |
| Écran de verrouillage | ms-settings:lockscreen |
| Barre de navigation | MS-Settings : personnalisation-barre**de navigation (déconseillée dans Windows 10, version 1809 et versions ultérieures**) |
| Personnalisation (catégorie) | ms-settings:personalization |
| Démarrer | MS-Settings : personnalisation-démarrer |
| Barre des tâches | MS-Settings : barre des tâches |
| Thèmes | MS-paramètres : thèmes |

## <a name="phone"></a>Téléphone

|Page de paramètres| URI |
|-------------|-----|
| Votre téléphone | MS-Settings : appareils mobiles<br/>MS-Settings : Mobile-Devices-addphone<br/>MS-Settings : Mobile-Devices-addphone-direct (ouvre **votre application téléphonique** ) |

## <a name="privacy"></a>Confidentialité

|Page de paramètres| URI |
|-------------|-----|
| Applications pour accessoires | MS-Settings : privacy-accessoryapps (**déconseillé dans Windows 10, version 1809 et versions ultérieures**) |
| Informations sur le compte | ms-settings:privacy-accountinfo |
| Historique d'activités | MS-Settings : confidentialité-activityhistory |
| Identifiant de publicité | MS-Settings : privacy-advertisingid (**déconseillé dans Windows 10, version 1809 et versions ultérieures**) |
| Diagnostics des applications | MS-Settings : confidentialité-appdiagnostics |
| Téléchargements automatiques de fichiers | MS-Settings : confidentialité-automaticfiledownloads |
| Applications en arrière-plan | ms-settings:privacy-backgroundapps |
| Calendrier | ms-settings:privacy-calendar |
| Historique des appels | ms-settings:privacy-callhistory |
| Appareil photo | ms-settings:privacy-webcam |
| Contacts | ms-settings:privacy-contacts |
| Documents | MS-Settings : confidentialité-documents |
| Courrier | ms-settings:privacy-email |
| Dispositif de suivi oculaire | MS-Settings : privacy-Eyetracker (nécessite un matériel Eyetracker) |
| Commentaires et diagnostics | ms-settings:privacy-feedback |
| Système de fichiers | MS-Settings : confidentialité-broadfilesystemaccess |
| Général | MS-Settings : confidentialité ou MS-Settings : confidentialité-général |
| Saisie de & manuscrite |ms-settings:privacy-speechtyping |
| Emplacement | ms-settings:privacy-location |
| Messagerie | ms-settings:privacy-messaging |
| Microphone | ms-settings:privacy-microphone |
| Mouvement | ms-settings:privacy-motion |
| Notifications | MS-Settings : confidentialité-notifications |
| Autres appareils | ms-settings:privacy-customdevices |
| Appels téléphoniques | MS-Settings : confidentialité-phonecalls |
| Images | MS-Settings : confidentialité-images |
| Radios | ms-settings:privacy-radios |
| Voix | MS-Settings : confidentialité-reconnaissance vocale |
| Tâches | MS-Settings : confidentialité-tâches |
| Vidéos | MS-Settings : confidentialité-vidéos |
| Activation vocale | MS-Settings : confidentialité-voiceactivation |

## <a name="surface-hub"></a>Surface Hub

|Page de paramètres| URI |
|-------------|-----|
| Comptes | MS-Settings : surfacehub-comptes |
| Nettoyage de session | MS-Settings : surfacehub-sessioncleanup |
| Conférence d’équipe | MS-Settings : surfacehub-Calling |
| Gestion des appareils de l’équipe | MS-Settings : surfacehub-devicemanagenent |
| Écran d’accueil | MS-Settings : surfacehub-Welcome |

## <a name="system"></a>Système

|Page de paramètres| URI |
|-------------|-----|
| À propos de | ms-settings:about |
| Paramètres d’affichage avancés | MS-Settings : Affichage-avancé (disponible uniquement sur les appareils qui prennent en charge les options d’affichage avancées) |
| Paramètres du volume et de l’appareil de l’application | MS-Settings : apps-volume (**ajouté dans Windows 10, version 1903**)|
| Économiseur de batterie | MS-Settings : BatterySaver (disponible uniquement sur les appareils qui ont une batterie, telle qu’une tablette) |
| Paramètres de l’économiseur de batterie | MS-Settings : BatterySaver-Settings (disponible uniquement sur les appareils qui ont une batterie, telle qu’une tablette) |
| Utilisation de la batterie | MS-Settings : BatterySaver-UsageDetails (disponible uniquement sur les appareils qui ont une batterie, telle qu’une tablette) |
| Presse-papiers | MS-Settings : presse-papiers |
| Afficher | ms-settings:display |
| Emplacements d’enregistrement par défaut | MS-Settings : savelocitys |
| Afficher | ms-settings:screenrotation |
| Duplication de l’affichage | MS-paramètres : quietmomentspresentation |
| Pendant ces heures | MS-paramètres : quietmomentsscheduled |
| Chiffrement | ms-settings:deviceencryption |
| Aide sur le focus | MS-paramètres : quiethours <br> MS-paramètres : quietmomentshome |
| Paramètres graphiques | MS-Settings : Display-advancedgraphics (disponible uniquement sur les appareils qui prennent en charge les options graphiques avancées) |
| Messagerie | ms-settings:messaging |
| Multitâche | MS-Settings : multitâche |
| Paramètres d’éclairage nocturne | MS-paramètres : Nightlight |
| Téléphone | MS-Settings : téléphone-defaultapps |
| Projection sur ce PC | MS-paramètres : projet |
| Expériences partagées | MS-paramètres : crossdevice |
| Mode Tablette | MS-paramètres : tabletmode |
| Barre des tâches | MS-Settings : barre des tâches |
| Notifications et actions | ms-settings:notifications |
| Bureau à distance | MS-paramètres : RemoteDesktop |
| Téléphone | MS-Settings : téléphone (**déconseillé dans Windows 10, version 1809 et versions ultérieures**) |
| Alimentation et mise en veille | ms-settings:powersleep |
| Son | MS-Settings : son |
| Stockage | ms-settings:storagesense |
| Assistant Stockage | MS-paramètres : storagepolicies |

## <a name="time-and-language"></a>Heure et langue

|Page de paramètres| URI |
|-------------|-----|
| Date et heure | ms-settings:dateandtime |
| Paramètres IME du Japon | MS-Settings : regionlanguage-jpnime (disponible si l’éditeur de méthode d’entrée Microsoft Japan est installé) |
| Région | MS-paramètres : regionformatting |
| Langage | MS-Settings : clavier<br/>ms-settings:regionlanguage<br/>MS-Settings : regionlanguage-bpmfime<br/>MS-Settings : regionlanguage-cangjieime<br/>MS-Settings : regionlanguage-chsime-pinyin-domainlexicon<br/>MS-Settings : regionlanguage-chsime-pinyin-keyconfig<br/>MS-Settings : regionlanguage-chsime-pinyin-UDP<br/>MS-Settings : regionlanguage-chsime-Wubi-UDP<br/>MS-Settings : regionlanguage-quickime |
| Paramètres IME pinyin | MS-Settings : regionlanguage-chsime-pinyin (disponible si l’éditeur de méthode d’entrée pinyin Microsoft est installé) |
| Voix | ms-settings:speech |
| Paramètres IME Wubi  | MS-Settings : regionlanguage-chsime-Wubi (disponible si l’éditeur de méthode d’entrée Microsoft Wubi est installé) |

## <a name="update--security"></a>Mise à jour et sécurité

|Page de paramètres| URI |
|-------------|-----|
| Activation | MS-Settings : activation |
| Sauvegarde | MS-paramètres : sauvegarde |
| Optimisation de la distribution | MS-Settings : livraison-optimisation |
| Localiser mon appareil | MS-paramètres : findmydevice |
| Pour les développeurs | ms-settings:developers |
| Récupération | MS-paramètres : récupération |
| Dépanner | MS-paramètres : résoudre les problèmes |
| Sécurité Windows | MS-paramètres : Windowsdefender |
| Programme Windows Insider | MS-Settings : windowsinsider (présent uniquement si l’utilisateur est inscrit dans WIP)<br/>MS-paramètres : windowsinsider-optin |
| Windows Update | ms-settings:windowsupdate<br>MS-paramètres : windowsupdate-action |
| Windows Update-Options avancées | MS-Settings : windowsupdate-options |
| Options de redémarrage Windows Update | MS-Settings : windowsupdate-restartoptions |
| Windows Update-afficher l’historique des mises à jour | MS-paramètres : windowsupdate-historique |

## <a name="user-accounts"></a>Comptes d'utilisateurs

|Page de paramètres| URI |
|-------------|-----|
| Approvisionnement | MS-Settings : mise en service de l’espace de travail (disponible uniquement si Enterprise a déployé un package d’approvisionnement) |
| Approvisionnement | MS-Settings : approvisionnement (disponible uniquement sur les appareils mobiles et si l’entreprise a déployé un package d’approvisionnement) |
| Windows Anywhere | MS-Settings : windowsanywhere (l’appareil doit être conforme à Windows Anywhere) |