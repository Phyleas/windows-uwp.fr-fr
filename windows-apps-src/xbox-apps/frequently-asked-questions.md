---
title: Forum aux questions
description: Si les choses ne fonctionnent pas comme prévu, consultez cette page des questions fréquemment posées sur UWP sur Xbox.
ms.date: 03/29/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 265fe827-bd4a-48d4-b362-8793b9b25705
ms.localizationpriority: medium
ms.openlocfilehash: 7da3b6a6507f2652eb2357ab80897d9ba35d7484
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170733"
---
# <a name="frequently-asked-questions"></a>Forum aux questions

Vous n’obtenez pas les résultats escomptés ? Parcourez cette page répertoriant les questions fréquemment posées. Consultez également la rubrique [Problèmes connus](known-issues.md) et le forum [Développement d’applications Windows universelles](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop). 

### <a name="why-arent-my-games-and-apps-working"></a>Pourquoi mes jeux et applications ne fonctionnent-ils pas ?

Si vos jeux et vos applications ne fonctionnent pas, ou si vous n’avez pas accès au Store ou aux services en direct, vous êtes probablement en mode développeur. Pour déterminer le mode dans lequel vous êtes en cours, appuyez sur le bouton **page d’hébergement** de votre contrôleur. Si vous accédez à la page d’hébergement de développement au lieu de l’expérience de vente au détail, vous êtes en mode développeur. Si vous souhaitez jouer à des jeux, vous pouvez ouvrir l’outil Accueil du développeur et revenir au mode commercial à l’aide du bouton **Quitter le mode développeur**.

### <a name="why-cant-i-connect-to-my-xbox-one-using-visual-studio"></a>Pourquoi ne puis-je pas me connecter à ma Xbox à l’aide de Visual Studio ?

Commencez par vérifier que vous travaillez en mode développeur, et non en mode commercial. Vous ne pouvez pas vous connecter à la console Xbox One en mode commercial. Pour déterminer le mode dans lequel vous êtes en cours, appuyez sur le bouton **page d’hébergement** de votre contrôleur. Si vous voyez un contenu Gold/en direct au lieu de develope orig, vous êtes en mode de vente au détail et vous devez exécuter l’application d’activation en mode dev pour passer en mode développeur.

> [!NOTE]
> Un utilisateur doit être connecté afin de pouvoir déployer une application.

Pour plus d’informations, voir [Résolution des problèmes de déploiement](#fixing-deployment-failures) plus loin sur cette page.

### <a name="how-do-i-switch-between-retail-mode-and-developer-mode"></a>Comment basculer entre les modes commercial et développeur ?

Suivez les instructions de la rubrique [Activation du Mode développeur Xbox One](devkit-activation.md) pour obtenir une description de ces deux modes.

### <a name="how-do-i-know-if-i-am-in-retail-mode-or-developer-mode"></a>Comment savoir si je suis en mode commercial ou en mode développeur ?

Suivez les instructions de la rubrique [Activation du Mode développeur Xbox One](devkit-activation.md) pour obtenir une description de ces deux modes. 

Pour déterminer le mode dans lequel vous êtes en cours, appuyez sur le bouton **page d’hébergement** de votre contrôleur. 
- Si vous accédez à la page de développement, vous êtes en mode développeur.
- Si vous voyez du contenu Gold/Live, vous êtes en mode de vente au détail.

### <a name="will-my-games-and-apps-still-work-if-i-activate-developer-mode"></a>Mes jeux et applications continueront-ils de fonctionner si j’active le mode développeur ?

Oui. Lorsque vous vous trouvez en mode développeur, vous pouvez basculer vers le mode commercial qui vous permet de jouer à vos jeux. Pour plus d’informations, voir la page [Activation du Mode développeur Xbox One](devkit-activation.md). 

### <a name="can-i-develop-and-publish-x86-apps-for-xbox"></a>Puis-je développer et publier des applications x86 pour Xbox ?
Xbox ne prend plus en charge le développement d’applications x86 ou les envois d’application x86 au magasin. 

### <a name="will-i-lose-my-games-and-apps-or-saved-changes"></a>Vais-je perdre mes jeux et applications ou les modifications que j’ai enregistrées ?

Si vous décidez de quitter le programme pour les développeurs, vous ne perdrez pas les applications et les jeux que vous avez installés. En outre, tant que vous étiez en ligne lors de la lecture, vos jeux enregistrés sont enregistrés dans votre profil de Cloud de compte actif, ce qui vous empêche de les perdre.

### <a name="how-do-i-leave-the-developer-program"></a>Comment quitter le programme pour les développeurs ?

Pour plus d’informations la manière de quitter le programme pour les développeurs, voir [Désactivation du mode développeur Xbox One](devkit-deactivation.md).

### <a name="i-sold-my-xbox-one-and-left-it-in-developer-mode-how-do-i-deactivate-developer-mode"></a>J’ai vendu ma console Xbox One en la laissant en mode développeur. Comment désactiver le mode développeur ?

Si vous n’avez plus accès à votre Xbox, vous pouvez le désactiver dans l’espace partenaires Windows. Pour plus d’informations, consultez la section **désactiver votre console à l’aide de l’espace partenaires** dans la rubrique sur la [désactivation de Xbox One Developer mode](devkit-deactivation.md#deactivate-your-console-using-partner-center) . 

### <a name="i-left-the-developer-program-using-partner-center-but-im-in-still-developer-mode-what-do-i-do"></a>J’ai laissé le programme pour développeurs à l’aide de l’espace partenaires, mais je suis en mode développeur. Que faire ?

Démarrez l’outil Accueil du développeur et sélectionnez le bouton **Quitter le mode développeur**. Cette action a pour effet de redémarrer votre console en mode commercial. 

### <a name="can-i-publish-my-app"></a>Puis-je publier mon application ?

Vous pouvez [publier des applications](../publish/index.md) via l’espace partenaires si vous disposez d’un [compte de développeur](https://developer.microsoft.com/store/register). Les applications UWP créées et testées sur une console Xbox de la vente au détail présentent les mêmes processus d’ingestion, de révision et de publication que Windows, avec des révisions supplémentaires pour répondre aux normes Xbox One actuelles.

### <a name="can-i-publish-my-game"></a>Puis-je publier mon jeu ?

Vous pouvez utiliser UWP et votre console Xbox One en mode développeur pour créer et tester vos jeux sur Xbox One. Pour publier des jeux UWP, vous devez vous inscrire auprès du [ID@XBOX](https://www.xbox.com/Developers/id) [programme Xbox Live Creators](https://developer.microsoft.com/games/xbox/xboxlive/creator)ou faire partie de celui-ci. Pour plus d’informations, consultez [vue d’ensemble du programme de développement](https://developer.microsoft.com/games/xbox/docs/xboxlive/get-started/developer-program-overview.html).

### <a name="will-the-standard-game-engines-work"></a>Les moteurs de jeu standard fonctionneront-ils ?

Consultez la page [Problèmes connus](known-issues.md) concernant cette version.

### <a name="what-capabilities-and-system-resources-are-available-to-uwp-games-on-xbox-one"></a>Quelles sont les fonctionnalités et ressources système disponibles pour les jeux UWP sur Xbox One ? 

Pour plus d’informations, voir [Ressources système pour les applications et jeux UWP sur Xbox One](system-resource-allocation.md).

### <a name="if-i-create-a-directx-12-uwp-game-will-it-run-on-my-xbox-one-in-developer-mode"></a>Si je crée un jeu UWP DirectX 12 , s’exécutera-t-il sur ma console Xbox One en mode développeur ?

Pour plus d’informations, voir [Ressources système pour les applications et jeux UWP sur Xbox One](system-resource-allocation.md).

### <a name="will-the-entire-uwp-api-surface-be-available-on-xbox"></a>La surface d’API UWP sera-t-elle disponible en totalité sur Xbox ?

Consultez la page [Problèmes connus](known-issues.md) concernant cette version.

### <a name="fixing-deployment-failures"></a>Résolution des problèmes de déploiement

Si vous ne pouvez pas déployer votre application à partir de Visual Studio, ces étapes peuvent vous aider à résoudre le problème. Si vous êtes bloqué, demandez de l’aide sur le forum.

> [!NOTE]
> Un utilisateur doit être connecté afin de pouvoir déployer une application. Si vous recevez un message d’erreur 0x87e10008, assurez-vous qu’un utilisateur est connecté, puis essayez de nouveau.

Si Visual Studio ne peut pas se connecter à votre console Xbox One :

1. Assurez-vous que vous êtes en mode développeur (comme décrit précédemment sur cette page).
2. Vérifiez que vous avez correctement configuré votre PC de développement. Avez-vous suivi *toutes* les instructions de la rubrique [Prise en main du développement d’applications UWP sur Xbox One](getting-started.md) ? 

3. Si vous ne l’avez pas encore fait, consultez la rubrique installation de l' [environnement de développement](development-environment-setup.md) et la rubrique [Présentation des outils Xbox One](introduction-to-xbox-tools.md) .

4. Vérifiez que vous pouvez « effectuer un test ping » sur l’adresse IP de votre console à partir de votre PC de développement.
  > [!NOTE]
  > Pour des performances de déploiement optimales, nous vous recommandons d’utiliser une connexion câblée à votre console.

5. Assurez-vous que vous utilisez le protocole universel (protocole non chiffré) dans la liste déroulante authentification de l’onglet **Déboguer** . Pour plus d’informations, consultez Configuration de l' [environnement de développement](development-environment-setup.md).


### <a name="if-im-building-an-app-using-htmljavascript-how-do-i-enable-gamepad-navigation"></a>Si je crée une application à l’aide de HTML/JavaScript, comment puis-je activer la navigation dans le boîtier de configuration ?

TVHelpers est un ensemble d’exemples et de bibliothèques JavaScript et XAML/C# pour vous aider à créer des applications réussies pour Xbox One et la télévision en JavaScript et C#. TVJS est une bibliothèque qui vous permet de créer des applications UWP de grande qualité pour Xbox One. TVJS inclut la prise en charge de la navigation automatique sur manette, la lecture de contenus multimédias riches, la recherche et plus encore. Vous pouvez utiliser TVJS avec votre application web hébergée tout aussi facilement qu’avec une application UWP web empaqueté avec accès complet aux API Windows Runtime.

Pour plus d’informations, voir le projet [TVHelpers](https://github.com/Microsoft/TVHelpers) et le projet [wiki](https://github.com/Microsoft/TVHelpers/wiki).

## <a name="see-also"></a>Voir aussi
- [Problèmes connus avec UWP sur Xbox One](known-issues.md)
- [UWP sur Xbox One](index.md)
- [UWP sur Xbox One](index.md)
