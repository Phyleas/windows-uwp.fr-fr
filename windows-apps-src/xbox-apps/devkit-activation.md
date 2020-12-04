---
title: Activation du Mode développeur Xbox One
description: Comment activer le Mode développeur afin de pouvoir basculer du Mode commercial au Mode développeur et inversement.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: ade80769-17ae-46e9-9c2f-bf08ae5a51ee
ms.localizationpriority: medium
ms.openlocfilehash: d93fef1b06fa52616b7e5eddf7b3ac0530856dc2
ms.sourcegitcommit: a15bc17aa0640722d761d0d33f878cb2a822e8ed
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96577101"
---
# <a name="xbox-one-developer-mode-activation"></a>Activation du Mode développeur Xbox One

## <a name="how-developer-mode-works"></a>Fonctionnement du Mode développeur
Cet article s’applique uniquement à Xbox One et à la série Xbox X | Les consoles ont été acquises via des canaux de vente au détail. Pour le matériel du kit de développement acquis via un programme de développement managé, consultez la remarque à la fin de l’article.

Les consoles de vente au détail Xbox peuvent avoir deux modes : le mode vente au détail (1) et le mode développeur (2). En mode vente au détail, la console est dans un état normal : vous pouvez jouer à des jeux et exécuter des applications acquises via le magasin Xbox. En mode développeur, vous pouvez développer et tester des logiciels pour la console, mais vous ne pouvez pas jouer à des jeux de vente au détail ou exécuter des applications de vente au détail.

Le mode développeur peut être activé sur n’importe quelle console Xbox de vente au détail, via l’application « conversion du kit de vente au détail » qui se trouve dans le magasin Xbox. Une fois que le mode développeur est activé sur votre console de vente au détail, vous pouvez passer de l’un à l’autre au détail (2a) et aux modes développeur (2b).

> [!NOTE]
> N’exécutez pas cette application de conversion sur un matériel de développement Xbox acquis via un programme géré par Xbox (par exemple, ID@Xbox ) ou vous risquez d’introduire des erreurs et des retards lors du développement de votre jeu. Si vous êtes un partenaire géré, vous pouvez obtenir plus d’informations sur l’activation du matériel de développement. Atteindre https://developer.microsoft.com/en-us/games/xbox/docs/gdk/provisioning-role.

<br></br>

![Modes Xbox One](images/dev-mode-flow.png)

## <a name="activate-developer-mode-on-your-retail-xbox-console"></a>Activer le mode développeur sur votre console Xbox de vente au détail

1.  Démarrez votre console Xbox.

2.  Recherchez et installez l’application d' **activation en mode dev** à partir du magasin Xbox One.

    ![Installer l’application Dev Mode Activation](images/devkit-activation-1.png)

3.  Lancez l’application à partir de la page du Windows Store.

    ![Application Dev Mode Activation](images/devkit-activation-2.png)

4.  Notez le code affiché dans l’application Dev Mode Activation.

    ![Activation - Étape 5](images/activation-step-5.png)  
    
5.  [Inscrire un compte de développeur d’applications dans l’espace partenaires](https://developer.microsoft.com/store/register).  C’est également la première étape de la publication de votre jeu.

6.  Connectez-vous à l' [espace partenaires](https://partner.microsoft.com/dashboard) à l’aide de votre compte de développeur d’applications de l’espace partenaires valide.  Si vous ne voyez pas plusieurs options dans le volet de navigation de gauche ou si vous ne voyez pas l’option **créer une nouvelle application** dans la section **vue d’ensemble** , les étapes et les liens d’activation suivants ne _fonctionnent pas_. Assurez-vous d’avoir inscrit entièrement votre compte de développeur d’applications à l’étape précédente.

7.  Accédez à [Partner.Microsoft.com/xboxconfig/Devices](https://partner.microsoft.com/xboxconfig/devices).

8.  Entrez le code d’activation affiché dans l’application Dev Mode Activation. Vous disposez d’un nombre limité d’activations associées à votre compte. Une fois le mode développeur activé, l’espace partenaires indique que vous avez utilisé l’une des activations associées à votre compte.

    ![Activation - Étape 8](images/activation-step-8-rs2.png)    
    
9.  Cliquez sur **Accepter et activer**. La page sera rechargée et votre appareil apparaîtra dans le tableau. Pour plus d’informations, voir [Programme d’activation du mode développeur Xbox One](/legal/windows/agreements/xbox-one-developer-mode-activation).

10. Après avoir saisi votre code d’activation, votre console affiche un écran de progression pour le processus d’activation.  
    
11. Une fois l’activation est terminée, ouvrez l’application Dev Mode Activation, puis cliquez sur **Basculer et redémarrer** pour accéder au Mode développeur. Notez que cette opération peut être plus longue que d’habitude.

    ![Activation - Étape 12](images/activation-step-12.png)   

## <a name="switch-between-retail-and-developer-mode"></a>Basculer entre le Mode commercial et développeur
Une fois le Mode développeur activé sur votre console, utilisez **Dev Home** pour basculer entre le Mode commercial et développeur. Pour en savoir plus sur le démarrage et l’utilisation de la base de développement, consultez [Présentation des outils Xbox One](introduction-to-xbox-tools.md).

* Pour basculer en mode vente au détail, ouvrez la **page d’hébergement dev**. Sous **actions rapides**, sélectionnez **conserver le mode dev**. Cette action a pour effet de redémarrer votre console en mode commercial.    

  ![Activation - Étape 13](images/activation-step-13-rs4.png)  
  
* Pour basculer vers le Mode développeur, utilisez l’application Dev Mode Activation. Ouvrez l’application et sélectionnez **basculement et redémarrage**. Cette action a pour effet de redémarrer votre console en Mode développeur.  

  ![Activation - Étape 14](images/activation-step-12.png)  

## <a name="see-also"></a>Voir aussi
- [Désactivation du Mode développeur Xbox One](devkit-deactivation.md)
- [UWP sur Xbox One](index.md)
