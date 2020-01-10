---
title: Déployer une application par le biais de l’inscription de fichiers libres
description: Ce guide montre comment utiliser la disposition de fichiers libres pour valider et partager des applications Windows 10 sans avoir à les empaqueter.
ms.date: 06/01/2018
ms.topic: article
keywords: Windows 10, UWP, portail d’appareils, gestionnaire d’applications, déploiement, kit de développement logiciel (SDK)
ms.localizationpriority: medium
ms.openlocfilehash: 7bf3dab97be67a3b97aca4b3132bd9fe18691d15
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75681930"
---
# <a name="deploy-an-app-through-loose-file-registration"></a>Déployer une application par le biais de l’inscription de fichiers libres 

Ce guide montre comment utiliser la disposition de fichiers libres pour valider et partager des applications Windows 10 sans avoir à les empaqueter. L’inscription de dispositions de fichiers libres permet aux développeurs de valider rapidement leurs applications sans avoir à empaqueter et installer les applications. 

## <a name="what-is-a-loose-file-layout"></a>Qu’est-ce qu’une disposition de fichier faible ?

La disposition des fichiers libres est simplement l’acte de placer le contenu de l’application dans un dossier au lieu de passer par le processus d’empaquetage. Le contenu du package est « faiblement disponible » dans un dossier et non empaqueté. 

> [!WARNING]
> L’inscription de la disposition des fichiers libre est destinée aux développeurs et aux concepteurs pour valider rapidement leurs applications pendant le développement actif. Cette approche ne doit pas être utilisée pour « Dogfood » ou pour le vol de l’application. Nous vous recommandons d’effectuer la validation finale sur une application empaquetée qui est signée avec un certificat approuvé. 

## <a name="advantages-of-loose-file-registration"></a>Avantages de l’enregistrement de fichiers libres

- **Validation rapide** : étant donné que les fichiers d’application sont déjà décompressés, les utilisateurs peuvent rapidement inscrire la disposition de fichier libre et lancer l’application. Tout comme une application normale, l’utilisateur peut utiliser l’application telle qu’elle a été conçue. 
- **Distribution simple dans le réseau** : si les fichiers libres se trouvent dans un partage réseau au lieu d’un lecteur local, les développeurs peuvent envoyer l’emplacement du partage réseau à d’autres utilisateurs qui ont accès au réseau, et ils peuvent inscrire la disposition de fichier libre et exécuter l’application. Cela permet à plusieurs utilisateurs de valider l’application simultanément. 
- **Collaboration** : l’inscription de fichiers libre permet aux développeurs et aux concepteurs de continuer à travailler sur des éléments visuels pendant que l’application est inscrite. Les utilisateurs verront ces modifications lors du lancement de l’application. Notez que vous pouvez uniquement modifier les ressources statiques de cette manière. Si vous avez besoin de modifier du code ou du contenu créé dynamiquement, vous devez recompiler l’application.

## <a name="how-to-register-a-loose-file-layout"></a>Comment inscrire une disposition de fichier libre

Windows fournit plusieurs outils de développement pour inscrire des dispositions de fichiers libres sur des appareils locaux et distants. Vous pouvez choisir parmi `WinDeployAppCmd` (outil SDK Windows), portail des appareils Windows, PowerShell et [Visual Studio](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps#register-layout-from-network). Nous allons voir comment inscrire des fichiers libres à l’aide de ces outils. Mais tout d’abord, assurez-vous que vous avez le programme d’installation suivant :

- Vos appareils doivent être dans Windows 10 Creators Update (Build 14965) ou version ultérieure.
- Vous devez activer le [mode développeur](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) et la [découverte des appareils](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#device-discovery) sur tous les appareils.

> [!IMPORTANT]
> L’inscription de fichiers libre est disponible uniquement sur les appareils qui prennent en charge le protocole de partage réseau (SMB) : Desktop et Xbox. 

### <a name="register-with-windeployappcmd"></a>S’inscrire auprès de WinDeployAppCmd

Si vous utilisez les outils du kit de développement logiciel (SDK) correspondant à Windows 10 Creators Update (Build 14965) ou une version ultérieure, vous pouvez utiliser la commande `WinDeployAppCmd` dans une invite de commandes.

```cmd
WinAppDeployCmd.exe registerfiles -remotedeploydir <Network Path> -ip <IP Address> -pin <target machine PIN>
```

**Chemin réseau** : chemin d’accès aux fichiers libres de l’application.

**Adresse IP** : adresse IP de l’ordinateur cible.

**code PIN de l’ordinateur cible** : code confidentiel, si nécessaire, pour établir une connexion avec l’appareil cible. Vous serez invité à effectuer une nouvelle tentative avec l’option `-pin` si l’authentification est requise. Consultez [découverte d’appareils](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#device-discovery) pour savoir comment obtenir un code confidentiel.

### <a name="windows-device-portal"></a>Windows Device Portal

Le portail de périphériques Windows est disponible sur tous les appareils Windows 10 et est utilisé par les développeurs pour tester et valider leur travail. Elle répond à tous les publics de la communauté des développeurs avec l’expérience utilisateur du navigateur et les points de terminaison REST. Pour plus d’informations sur le portail des appareils, consultez [vue d’ensemble du portail de périphériques Windows](device-portal.md).

Pour inscrire la disposition de fichier libre dans le portail de l’appareil, procédez comme suit.

1. Connectez-vous au portail de l’appareil en suivant les étapes décrites dans la section **configuration** de la [vue d’ensemble du portail de périphériques Windows](device-portal.md).
1. Sous l’onglet Gestionnaire d’applications, sélectionnez **inscrire à partir du partage réseau**.
1. Entrez le chemin d’accès du partage réseau vers la disposition de fichier libre. 
1. Si l’appareil hôte n’a pas accès au partage réseau, une invite s’affiche pour vous permettre d’entrer les informations d’identification requises.
1. Une fois l’inscription terminée, vous pouvez lancer l’application.

Dans la page Gestionnaire d’applications du portail de l’appareil, vous pouvez également enregistrer des dispositions de fichiers libres facultatives pour votre application principale en activant la case à cocher **je souhaite spécifier des packages facultatifs** , puis en spécifiant les chemins d’accès de partage réseau des applications facultatives. 

### <a name="powershell"></a>PowerShell 

Windows PowerShell vous permet également d’inscrire des dispositions de fichiers libres, mais uniquement sur l’appareil local. Si vous devez inscrire une disposition sur un appareil distant, vous devez utiliser l’une des autres méthodes. 

Pour inscrire la disposition de fichier libre, lancez PowerShell et entrez ce qui suit.

```PowerShell
Add-AppxPackage -Register <path to manifest file>
```

## <a name="troubleshooting"></a>Dépannage

### <a name="mapped-network-drives"></a>Lecteurs réseau mappés
Actuellement, les lecteurs réseau mappés ne sont pas pris en charge pour les inscriptions de fichiers libres. Reportez-vous au lecteur mappé avec la totalité du chemin d’accès au partage réseau.

### <a name="registration-failure"></a>Échec de l’inscription
L’appareil sur lequel l’inscription a lieu doit avoir accès à la disposition du fichier. Si la mise en page du fichier est hébergée sur un partage réseau, assurez-vous que l’appareil y a accès. 

### <a name="modifications-to-visual-assets-arent-being-loaded-in-the-app"></a>Les modifications apportées aux ressources visuelles ne sont pas chargées dans l’application 
L’application chargera ses ressources visuelles au moment du lancement. Si des modifications ont été apportées aux éléments visuels après le lancement de l’application, vous devez relancer l’application pour afficher les modifications les plus récentes.
