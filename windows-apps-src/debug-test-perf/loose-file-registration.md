---
title: Déployer une application par le biais de l’inscription de fichiers libres
description: Ce guide montre comment utiliser la disposition de fichiers libres pour valider et partager des applications Windows 10 sans avoir à les empaqueter.
ms.date: 06/01/2018
ms.topic: article
keywords: windows 10, uwp, portail d’appareil, gestionnaire d’applications, déploiement, sdk
ms.localizationpriority: medium
ms.openlocfilehash: 0fd5bf6be691974d956de0c71f4a1d11aa1a229f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166093"
---
# <a name="deploy-an-app-through-loose-file-registration"></a>Déployer une application par le biais de l’inscription de fichiers libres 

Ce guide montre comment utiliser la disposition de fichiers libres pour valider et partager des applications Windows 10 sans avoir à les empaqueter. L’inscription de dispositions de fichiers libres permet aux développeurs de valider rapidement leurs applications sans avoir à les empaqueter et les installer. 

## <a name="what-is-a-loose-file-layout"></a>Qu’est-ce qu’une disposition de fichier libre ?

Une disposition de fichier libre est simplement l’acte de placer le contenu de l’application dans un dossier au lieu de passer par le processus d’empaquetage. Le contenu du package est « librement » disponible dans un dossier, et non empaqueté. 

> [!WARNING]
> L’inscription de disposition de fichier libre est destinée aux développeurs et aux concepteurs pour leur permettre de valider rapidement leurs applications pendant le développement actif. Cette approche ne doit pas être utilisée pour une version de test ou d’évaluation de l’application. Nous vous recommandons d’effectuer la validation finale sur une application empaquetée et signée avec un certificat approuvé. 

## <a name="advantages-of-loose-file-registration"></a>Avantages de l’inscription de fichier libre

- **Validation rapide** : étant donné que les fichiers de l’application sont déjà décompressés, les utilisateurs peuvent rapidement inscrire la disposition de fichier libre et lancer l’application. Comme pour une application ordinaire, l’utilisateur peut utiliser l’application telle qu’elle a été conçue. 
- **Distribution facile dans le réseau** : si les fichiers libres se trouvent dans un partage réseau plutôt que sur un lecteur local, les développeurs peuvent envoyer l’emplacement du partage réseau à d’autres utilisateurs ayant accès au réseau, puis inscrire la disposition de fichier libre et exécuter l’application. Cela permet à plusieurs utilisateurs de valider l’application simultanément. 
- **Collaboration** : l’inscription de fichier libre permet aux développeurs et aux concepteurs de continuer à travailler sur des éléments visuels alors que l’application est inscrite. Les utilisateurs verront ces modifications lors du lancement de l’application. Notez que vous pouvez modifier uniquement des ressources statiques de cette manière. Si vous avez besoin de modifier du code ou du contenu créé de façon dynamique, vous devez recompiler l’application.

## <a name="how-to-register-a-loose-file-layout"></a>Comment inscrire une disposition de fichier libre

Windows fournit plusieurs outils de développement pour inscrire des dispositions de fichiers libres sur des appareils locaux et distants. Vous avez le choix entre `WinDeployAppCmd` (Outil SDK Windows), Portail d’appareil Windows, PowerShell et [Visual Studio](./deploying-and-debugging-uwp-apps.md#register-layout-from-network). Nous allons voir comment inscrire des fichiers libres à l’aide de ces outils. Mais d’abord, assurez-vous que vous avez la configuration suivante :

- Vos appareils doivent être sur Windows 10 Creators Update (Build 14965) ou version ultérieure.
- Vous devez activer le [mode développeur](../get-started/enable-your-device-for-development.md) et la [découverte d’appareils](../get-started/enable-your-device-for-development.md#device-discovery) sur tous les appareils.

> [!IMPORTANT]
> L’inscription de fichier libre est disponible uniquement sur les des appareils qui prennent en charge le protocole de partage réseau (SMB) : Ordinateur de bureau et Xbox. 

### <a name="register-with-windeployappcmd"></a>Inscrire auprès de WinDeployAppCmd

Si vous utilisez les outils du kit de développement logiciel (SDK) correspondant à Windows 10 Creators Update (Build 14965) ou une version ultérieure, vous pouvez utiliser la commande `WinDeployAppCmd` dans une invite de commandes.

```cmd
WinAppDeployCmd.exe registerfiles -remotedeploydir <Network Path> -ip <IP Address> -pin <target machine PIN>
```

**Chemin réseau** : chemin des fichiers libres de l’application.

**Adresse IP** : adresse IP de la machine cible.

**PIN de la machine cible** : code confidentiel, si nécessaire, pour établir une connexion avec l’appareil cible. Vous serez invité à réessayer avec l’option `-pin` si une authentification est requise. Pour savoir comment obtenir un code PIN, consultez [Découverte d’appareils](../get-started/enable-your-device-for-development.md#device-discovery).

### <a name="windows-device-portal"></a>Portail d’appareil Windows

Le Portail d’appareil Windows est disponible sur tous les appareils Windows 10 pour permettre aux développeurs de tester et valider leur travail. Il répond aux besoins de tous les publics de la communauté des développeurs avec l’expérience utilisateur de son navigateur et ses points de terminaison REST. Pour plus d’informations sur le Portail d’appareil, consultez l’[Vue d’ensemble du Portail d’appareil Windows](device-portal.md).

Pour inscrire la disposition de fichier libre dans le Portail d’appareil, procédez comme suit.

1. Connectez-vous au Portail d’appareil en procédant de la manière décrite dans la section **Configuration** de la [Vue d’ensemble du portail d’appareil Windows](device-portal.md).
1. Sous l’onglet Gestionnaire d’applications, sélectionnez **Inscrire à partir d’un partage réseau**.
1. Entrez le chemin d’accès du partage réseau vers la disposition de fichier libre. 
1. Si l’appareil hôte n’a pas accès au partage réseau, une invite s’affiche pour vous permettre d’entrer les informations d’identification requises.
1. Une fois l’inscription terminée, vous pouvez lancer l’application.

Dans la page Gestionnaire d’applications du Portail d’appareil, vous pouvez également inscrire des dispositions de fichiers libres facultatives pour votre application principale en activant la case à cocher **Je souhaite spécifier des packages facultatifs**, puis en spécifiant les chemins d’accès de partage réseau des applications facultatives. 

### <a name="powershell"></a>PowerShell 

Windows PowerShell vous permet également d’inscrire des dispositions de fichiers libres, mais uniquement sur l’appareil local. Si vous devez inscrire une disposition sur un appareil distant, vous devez utiliser l’une des autres méthodes. 

Pour inscrire la disposition de fichier libre, lancez PowerShell, puis entrez ce qui suit.

```PowerShell
Add-AppxPackage -Register <path to manifest file>
```

## <a name="troubleshooting"></a>Résolution des problèmes

### <a name="mapped-network-drives"></a>Lecteurs réseau mappés
Actuellement, les lecteurs réseau mappés ne sont pas pris en charge pour les inscriptions de fichiers libres. Reportez-vous au lecteur mappé avec le chemin d’accès complet du partage réseau.

### <a name="registration-failure"></a>Échec d’inscription
L’appareil sur lequel l’inscription a lieu doit avoir accès à la disposition du fichier. Si la disposition du fichier est hébergée sur un partage réseau, assurez-vous que l’appareil y a accès. 

### <a name="modifications-to-visual-assets-arent-being-loaded-in-the-app"></a>Les modifications apportées aux ressources visuelles ne sont pas chargées dans l’application 
L’application charge ses ressources visuelles au moment du lancement. Si des modifications ont été apportées aux ressources visuels après le lancement de l’application, vous devez relancer l’application pour voir les modifications les plus récentes.