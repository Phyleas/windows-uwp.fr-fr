---
ms.assetid: 54973C62-9669-4988-934E-9273FB0425FD
title: Activer votre appareil pour le développement
description: Découvrez comment activer votre appareil Windows 10 pour le développement et le débogage en activant le mode Développeur dans Visual Studio.
keywords: Commencer avec une licence de développeur Visual Studio, appareil avec licence de développeur activée
ms.date: 10/13/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 642dd2ac6db5509e9bc4ea6ff8cb756312c3e90b
ms.sourcegitcommit: 56e9cab45d1c6e54841d61fdf23044fa01f50c43
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/13/2020
ms.locfileid: "92011865"
---
# <a name="enable-your-device-for-development"></a>Activer votre appareil pour le développement

## <a name="activate-developer-mode-sideload-apps-and-access-other-developer-features"></a>Activer le Mode développeur, charger une version test des applications et accéder à d’autres fonctionnalités de développement

![Activer vos appareils pour le développement](images/developer-poster.png)

> [!IMPORTANT]
> Si vous ne créez pas vos propres applications sur votre ordinateur, vous n’avez pas besoin d’activer le mode développeur. Si vous essayez de résoudre un problème sur votre ordinateur, consultez l’[aide de Windows](https://support.microsoft.com/hub/4338813/windows-help?os=windows-10). Si vous effectuez un développement pour la première fois, vous souhaiterez également [vous préparer](get-set-up.md) en téléchargeant les outils dont vous avez besoin.

Si vous utilisez votre ordinateur pour des activités quotidiennes ordinaires, comme les jeux, la navigation sur le web, la messagerie ou les applications Office, vous n’avez *pas* besoin d’activer le Mode développeur et vous ne devez pas le faire. Les autres informations de cette page ne sont pas importantes pour vous, et vous pouvez en toute sécurité revenir à ce que vous faisiez précédemment. Merci de votre attention !

Toutefois, si vous utilisez Visual Studio sur un ordinateur pour créer un logiciel pour la première fois, vous *devez* activer le Mode développeur sur le PC de développement, ainsi que sur tous les appareils que vous allez utiliser pour tester votre code. Si vous ouvrez un projet UWP alors que le Mode développeur n’est pas activé, la page des paramètres **Pour les développeurs** s’ouvre ou la boîte de dialogue suivante s’affiche dans Visual Studio :

![Boîte de dialogue d’activation du mode développeur affichée dans Visual Studio](images/latestenabledialog.png)

Si cette boîte de dialogue s’affiche, cliquez sur **paramètres pour les développeurs** afin d’ouvrir la page de paramètres **Pour les développeurs**.

> [!NOTE]
> Vous pouvez à tout moment accéder à la page **Pour les développeurs** en vue d’activer ou de désactiver le mode développeur : entrez simplement « pour les développeurs » dans la zone de recherche de Cortana, dans la barre des tâches.

## <a name="accessing-settings-for-developers"></a>Accès aux paramètres pour les développeurs

Pour activer le mode développeur ou accéder à d’autres paramètres :

1.  À partir de la boîte de dialogue des paramètres **Pour les développeurs**, choisissez le niveau d’accès dont vous avez besoin.
2.  Lisez la clause d’exclusion de responsabilité pour le paramètre choisi, puis cliquez sur **Oui** pour accepter la modification.

> [!NOTE]
> L’activation du mode développeur requiert un accès administrateur. Si votre appareil appartient à votre organisation, il se peut que cette option soit désactivée.

### <a name="developer-mode-features"></a>Fonctionnalités du mode développeur

Le mode développeur remplace l’exigence de Windows 8.1 relative à la détention d’une licence de développeur.  Le paramètre Mode développeur est proposé en plus du chargement indépendant. Il offre une fonction de débogage et d’autres options de déploiement, notamment le démarrage d’un service SSH pour permettre le déploiement de cet appareil. Pour arrêter ce service, vous devez désactiver le mode développeur.

Quand vous activez le mode développeur sur le bureau, un ensemble de fonctionnalités est installé, à savoir :
- Portail d’appareil Windows. Device Portal est activé et les règles de pare-feu associées sont configurées seulement si l’option **Activer Device Portal** est activée.
- Installation et configuration des règles de pare-feu pour les services SSH qui permettent l’installation à distance des applications. L’activation de l’option **Découverte d’appareils** active le serveur SSH.

Pour plus d’informations sur ces fonctionnalités, ou si vous rencontrez des difficultés dans le processus d’installation, consultez [fonctionnalités du mode développeur et débogage](developer-mode-features-and-debugging.md).

## <a name="see-also"></a>Voir aussi

* [Créer un compte Windows](sign-up.md)
* [Fonctionnalités et débogage du mode développeur](developer-mode-features-and-debugging.md).