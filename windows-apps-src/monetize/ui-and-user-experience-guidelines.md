---
ms.assetid: 7a38a352-6e54-4949-87b1-992395a959fd
description: Consultez les instructions pour fournir une interface utilisateur et des expériences utilisateur exceptionnelles avec les bannières publicitaires, les publicités interstitielles et les publicités natives dans vos applications.
title: Instructions relatives à l’interface utilisateur et à l’expérience utilisateur pour les publicités
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, ADS, publicité, recommandations, meilleures pratiques
ms.localizationpriority: medium
ms.openlocfilehash: 014cc5a7be370ed5fd579af9bc4ab7600cb96689
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158353"
---
# <a name="ui-and-user-experience-guidelines-for-ads"></a>Instructions relatives à l’interface utilisateur et à l’expérience utilisateur pour les publicités

>[!WARNING]
> Depuis le 1er juin 2020, la plateforme de monétisation Microsoft AD pour les applications Windows UWP sera arrêtée. [En savoir plus](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Cet article fournit des instructions pour fournir des expériences intéressantes avec les bannières publicitaires, les publicités interstitielles et les publicités natives dans vos applications. Pour obtenir des conseils généraux sur la conception de l’apparence et du style des applications, consultez [Conception et interface utilisateur](https://developer.microsoft.com/windows/apps/design).

> [!IMPORTANT]
> Toute utilisation de la publicité dans votre application doit se conformer aux stratégies de Microsoft Store, y compris, sans limitation, la [stratégie 10,10](/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) (réalisation et contenu de la publicité). En particulier, l’implémentation de votre application de bannières publicitaires ou de publicités interstitielles doit répondre aux exigences de Microsoft Store stratégie de stratégie [10.10.1](/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content). Cet article fournit des exemples d’implémentations qui ne respectent pas cette politique. Ces exemples sont fournis à titre d’information uniquement, pour vous aider à comprendre comment respecter la politique. Ces exemples ne sont pas exhaustifs, et il peut y avoir de nombreuses autres façons de violer les stratégies de Microsoft Store qui ne sont pas répertoriées dans cet article.

## <a name="general-best-practices"></a>Bonnes pratiques générales

Avant de passer en revue nos instructions pour les différents types d’annonces dans cet article, commencez par consulter les meilleures pratiques générales pour améliorer votre chiffre d’affaires.

* [Planifiez vos placements d’annonces avec précaution](https://blogs.windows.com/buildingapps/2017/04/10/monetizing-app-advertisement-placement/). Pour plus d’informations sur [l’optimisation de la vue de vos unités ad](optimize-ad-unit-viewability.md), consultez nos conseils.
* [Utilisez des bannières publicitaires interstitielles comme solution de secours pour les publicités vidéo](https://blogs.windows.com/buildingapps/2017/04/17/monetizing-app-use-interstitial-banner-fallback-interstitial-video/).
* [Vous savez que vos utilisateurs peuvent obtenir de meilleures publicités ciblées](https://blogs.windows.com/buildingapps/2017/05/17/monetize-app-know-user-serve-better-targeted-ads/).
* [Utilisez les bibliothèques de publicité les plus récentes](https://blogs.windows.com/buildingapps/2017/05/22/earn-money-moving-latest-advertising-libraries/).
* [Définissez les paramètres Coppa corrects pour votre application](https://blogs.windows.com/buildingapps/2017/06/21/monetizing-app-set-coppa-settings-app/).


## <a name="guidelines-for-banner-ads"></a>Recommandations pour les bannières publicitaires

Les sections suivantes fournissent des recommandations sur la façon d’implémenter des [bannières publicitaires](banner-ads.md) dans votre application à l’aide de [classe AdControl](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) et des exemples d’implémentations qui violent la [stratégie 10.10.1](/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) des stratégies de Microsoft Store.

### <a name="best-practices"></a>Meilleures pratiques

Nous vous recommandons de suivre ces bonnes pratiques quand vous implémentez des bannières publicitaires dans votre application :

* [Utilisez des tailles de bureau de publicité interactives](https://blogs.windows.com/buildingapps/2017/04/03/monetizing-app-use-interactive-advertising-bureau-ad-sizes/) qui s’adaptent parfaitement à la disposition de l’appareil.

* Réservez la majeure partie de l’interface utilisateur de votre application aux contrôles et au contenu fonctionnels.

* Intégrez des publicités à votre expérience. Donnez à vos concepteurs un exemple d’annonce pour planifier ce à quoi ressemblera la publicité. La disposition de publicités en tant que contenu et la disposition scindée sont deux exemples de publicités bien planifiées dans les applications.

  Pour voir comment différentes tailles de publicité s’affichent et fonctionnent au sein de votre application pendant la phase de développement et de test, vous pouvez utiliser nos [unités AD de test](set-up-ad-units-in-your-app.md#test-ad-units). Lorsque vous avez terminé le test, n’oubliez pas de [mettre à jour votre application avec des unités ad actives](set-up-ad-units-in-your-app.md#live-ad-units) avant de soumettre l’application à la certification.

* Planifiez pour les périodes au cours desquelles aucune annonce ne sera disponible. Il peut arriver à certains moments qu’aucune annonce ne soit envoyée à votre application. Disposez vos pages de telle façon qu’elles s’affichent de manière optimale avec ou sans annonce. Pour plus d’informations, consultez [gérer les erreurs ad](error-handling-with-advertising-libraries.md).

* Si, dans votre scénario, la gestion des alertes envoyées à l’utilisateur est plus efficace avec une superposition, appelez [AdControl.Suspend](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.suspend) quand la superposition s’affiche, puis appelez [AdControl.Resume](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.resume) à la fin de l’alerte.

### <a name="practices-to-avoid"></a>Pratiques à éviter

Nous vous recommandons d’éviter ces pratiques quand vous implémentez des bannières publicitaires dans votre application :

* Ne bloquez pas les publicités dans les espaces ouverts. L’espace publicitaire ne doit pas être placé dans le premier espace libre que vous trouvez. Au lieu de cela, il doit être incorporé dans la conception globale de votre application.

* Ne surchargez pas votre application avec une publicité excessive. Trop de publicités dans votre application détournent l’attention de son apparence et de sa facilité d’utilisation. Vous souhaitez gagner de l’argent avec les publicités, mais pas au détriment de l’application elle-même.

* Ne distrayez pas l’utilisateur de ses tâches de base. L’axe principal doit toujours être l’application. L’espace publicitaire doit être incorporé de manière à rester secondaire.

### <a name="examples-of-policy-violations"></a>Exemples de non-respect de la politique

Cette section fournit des exemples de scénarios d’enchaînement d’annonces qui violent la [stratégie 10.10.1](/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) des stratégies de Microsoft Store. Ces exemples sont fournis à titre d’information uniquement, pour vous aider à comprendre comment respecter la politique. Ces exemples ne sont pas exhaustifs. Il existe beaucoup d’autres cas possibles de non-respect de la politique 10.10.1 qui ne sont pas répertoriés ici.

* Empêcher d’une façon ou d’une autre l’utilisateur d’afficher la bannière publicitaire, comme modifier l’opacité de l’objet [AdControl](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) ou placer un autre contrôle sur **AdControl** (sans appeler au préalable [AdControl.Suspend](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.suspend)).

* Obliger les utilisateurs à cliquer sur une bannière publicitaire pour pouvoir effectuer une tâche dans votre application, ou concevoir votre application de sorte que les utilisateurs n’aient pas d’autre choix que cliquer sur des bannières publicitaires.

* Contourner le minuteur d’actualisation intégré minimum pour les bannières publicitaires par quelque moyen que ce soit, y compris, mais sans s’y limiter, en remplaçant des objets [AdControl](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) ou en actualisant automatiquement une page sans aucune interaction utilisateur.

* À l’aide d’unités ad actives (c’est-à-dire les unités ad que vous obtenez à partir de l’espace partenaires) pendant le développement et le test, ou dans un émulateur.

* Écrire ou distribuer du code qui appelle des services publicitaires par d’autres moyens que les bibliothèques de publicités Microsoft exécutées dans le contexte de votre application.

* Interagir avec des interfaces non documentées ou des objets enfants créés par les bibliothèques de publicités Microsoft, comme **WebView** ou **MediaElement**.

* Placer des publicités dans un Viewbox pour réduire la taille des publicités afin d’autoriser plus de publicités sur une page que la normale.

<span id="interstitialbestpractices10" />

## <a name="guidelines-for-interstitial-ads"></a>Recommandations pour les spots publicitaires

Lorsqu’ils sont utilisés de façon élégante, les [publicités interstitielles](interstitial-ads.md) peuvent considérablement augmenter le chiffre d’affaires de votre application, sans nuire à la satisfaction des utilisateurs. En cas d’utilisation incorrecte, ils peuvent produire l’effet inverse exact.

Les sections suivantes fournissent des recommandations sur la façon d’implémenter des publicités vidéo interstitielles et des bannières de bannière interstitielles dans votre application à l’aide de [InterstitialAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad), ainsi que des exemples d’implémentations qui violent la [stratégie 10.10.1](/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) des stratégies de Microsoft Store. Étant donné que vous connaissez votre application mieux que quiconque, sauf en matière de stratégie, nous vous laissons prendre la meilleure décision finale. Ce qu’il faut absolument retenir c’est que les évaluations de vos applications et le montant de vos recettes sont étroitement liés.

### <a name="best-practices"></a>Meilleures pratiques

Nous vous recommandons de suivre ces bonnes pratiques quand vous implémentez des spots publicitaires dans votre application :

* Intégrez des spots publicitaires dans le flux naturel de l’application, par exemple entre les niveaux de jeu.

* Associez des annonces avec des avantages concrets, tels que :

    * Conseils concernant l’achèvement de niveau.

    * Temps supplémentaire pour réessayer un niveau.

    * Fonctionnalités d’avatar personnalisé, par exemple avec un tatouage ou un chapeau.

* Si votre application exige qu’une publicité de vidéo interstitiel soit surveillée jusqu’à son terme, mentionnez-la de façon à ce qu’elle ne soit pas surpris par un message d’erreur lorsque vous appuyez sur le bouton Fermer.

* Récupérez d’abord la publicité (en appelant la méthode [InterstitialAd.RequestAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad)), dans l’idéal, 30 à 60 secondes avant que vous ayez besoin de l’afficher.

* Abonnez-vous aux quatre événements exposés dans la classe [InterstitialAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad) (**Canceled**, **Completed**, **AdReady** et **ErrorOccurred**) et utilisez-les pour prendre les bonnes décisions pour votre application.

* Bénéficiez d’une expérience intégrée au lieu d’annonces proposées par le serveur. Vous trouverez cela utile dans certains scénarios :

    * En mode hors connexion, lorsque les serveurs publicitaires sont injoignables.

    * Lorsque l’événement **ErrorOccurred** est déclenché.

    * Si vous choisissez d’économiser la bande passante de l’utilisateur en fonction de [ConnectionProfile](/uwp/api/Windows.Networking.Connectivity.ConnectionProfile), il existe des API dans la classe **ConnectionProfile** qui peuvent vous aider.

* Utilisez le délai d’expiration par défaut (30 secondes), sauf si vous avez une raison valable de faire autrement, auquel cas n’allez pas en dessous de 10 secondes. Les publicités interstitielles prennent beaucoup plus de temps à télécharger que les bannières standard, en particulier sur les marchés qui n’ont pas de connexions haut débit.

* Gardez à l’esprit les forfaits de données des utilisateurs. Par exemple, ne pas afficher ou avertir l’utilisateur avant de servir une publicité vidéo interstitielle sur un appareil mobile qui est proche/au-dessus de la limite de ses données. Il existe des API dans la classe [ConnectionProfile](/uwp/api/Windows.Networking.Connectivity.ConnectionProfile) qui peuvent vous aider.

* Améliorez en permanence votre application après la soumission initiale. Examinez les [rapports ad](../publish/advertising-performance-report.md) et apportez des modifications à la conception pour améliorer les taux d’achèvement des vidéos et des remplissages.

### <a name="practices-to-avoid"></a>Pratiques à éviter

Nous vous recommandons d’éviter ces pratiques quand vous implémentez des spots publicitaires dans votre application :

* N’intégrez pas de spots publicitaires de manière excessive dans votre application. N’envoyez pas de publicités à une fréquence supérieure à toutes les 5 minutes environ, à moins que l’utilisateur ne bénéficie d’un avantage concret en option, au-delà du jeu.

* N’affichez pas les interstitiels au lancement de l’application, car les utilisateurs peuvent penser qu’ils ont cliqué sur la mauvaise vignette.

* N’affichez pas les interstitiels à la sortie. Il s’agit d’un mauvais calcul, car les taux d’achèvement seront proches de zéro.

* N’affichez pas plusieurs spots publicitaires d’affilée. Les utilisateurs seront contrariés de voir la barre de progression de l’annonce revenir au point de départ. Beaucoup penseront qu’il s’agit d’un bogue de codage ou d’affichage de publicité.

* N’extrayez pas une publicité vidéo de plus de 5 minutes avant d’appeler [InterstitialAd. Show](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show). Un bon inventaire va optimiser la conversion de publicités récupérées en impressions facturables.

* Ne pénalisez pas un utilisateur pour les défaillances d’affichage de publicités, par exemple quand aucune publicité n’est disponible. Par exemple, si vous affichez une option d’interface utilisateur de type « Regarder une publicité pour obtenir *xxx* », vous devez fournir *xxx* si l’utilisateur a rempli sa part du contrat. Deux options à envisager :

    * N’incluez pas l’option, sauf si l’événement [InterstitialAd.AdReady](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.adready) a été déclenché.

    * Proposez une application incluant une expérience intégrée qui procure les mêmes avantages qu’une annonce réelle.

* N’utilisez pas de spots publicitaires permettant à un utilisateur de gagner un avantage compétitif dans un jeu multijoueur. Par exemple, n’affichez pas de spot publicitaire incitant l’utilisateur à se servir d’une meilleure arme dans un jeu de tir. Une chemise personnalisée sur l’avatar du joueur pourrait faire l’affaire, tant qu’elle ne sert pas de camouflage !

### <a name="examples-of-policy-violations"></a>Exemples de non-respect de la politique

Cette section fournit des exemples de scénarios de publicité interstitielle qui violent la [stratégie 10.10.1](/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) des stratégies de Microsoft Store. Ces exemples sont fournis à titre d’information uniquement, pour vous aider à comprendre comment respecter la politique. Ces exemples ne sont pas exhaustifs. Il existe beaucoup d’autres cas possibles de non-respect de la politique 10.10.1 qui ne sont pas répertoriés ici.

* Placer un élément d’interface utilisateur sur le conteneur de spot publicitaire.

* Appeler [InterstitialAd.Show](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show) quand l’utilisateur interagit avec l’application.

* Utiliser des spots publicitaires pour obtenir tout ce qui peut être utilisé comme une devise ou échangé avec d’autres utilisateurs.

* Demander un nouveau spot publicitaire dans le contexte du gestionnaire d’événements pour l’événement [InterstitialAd.ErrorOccurred](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.erroroccurred). Cela peut produire une boucle infinie et provoquer des problèmes de fonctionnement du service publicitaire.

* Demander un spot publicitaire simplement pour avoir une publicité de secours à une cascade de publicités. Si vous demandez un spot publicitaire et que vous recevez l’événement [InterstitialAd.AdReady](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.adready), le spot publicitaire suivant affiché dans votre application doit être celui qui est prêt à être affiché par la méthode [InterstitialAd.Show](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show).

* À l’aide d’unités ad actives (c’est-à-dire les unités ad que vous obtenez à partir de l’espace partenaires) pendant le développement et le test, ou dans un émulateur.

* Écrire ou distribuer du code qui appelle des services publicitaires par d’autres moyens que les bibliothèques de publicités Microsoft exécutées dans le contexte de votre application.

* Interagir avec des interfaces non documentées ou des objets enfants créés par les bibliothèques de publicités Microsoft, comme **WebView** ou **MediaElement**.

## <a name="guidelines-for-native-ads"></a>Instructions pour les publicités natives

Les [publicités natives](native-ads.md) vous donnent un grand contrôle sur la façon dont vous présentez le contenu publicitaire à vos utilisateurs. Suivez ces exigences et ces recommandations pour vous assurer que le message de l’annonceur parvient à atteindre vos utilisateurs tout en aidant à éviter de diffuser une expérience ADS native à vos utilisateurs.

### <a name="register-the-container-for-your-native-ad"></a>Inscrire le conteneur pour votre ad natif

Dans votre code, vous devez appeler la méthode [RegisterAdContainer](/uwp/api/microsoft.advertising.winrt.ui.nativeadv2.registeradcontainer) de l’objet [NativeAdV2](/uwp/api/microsoft.advertising.winrt.ui.nativeadv2) pour inscrire l’élément d’interface utilisateur qui joue le rôle de conteneur pour la publicité native et, éventuellement, tous les contrôles spécifiques que vous souhaitez inscrire comme cibles interactives pour la publicité. Cela est nécessaire pour suivre correctement les impressions et les clics sur les annonces.

Il existe deux surcharges pour la méthode **RegisterAdContainer** que vous pouvez utiliser :

* Si vous souhaitez que le conteneur entier pour tous les éléments ad natifs individuels soit cliquable, appelez la méthode **RegisterAdContainer (FrameworkElement)** et transmettez le contrôle conteneur à la méthode. Par exemple, si vous affichez tous les éléments ad natifs dans des contrôles distincts qui sont tous hébergés dans un **StackPanel** et que vous souhaitez que l’élément **StackPanel** entier soit cliquable, transmettez le **StackPanel** à cette méthode.

* Si vous souhaitez que seuls certains éléments ad natifs soient cliquables, appelez la méthode **RegisterAdContainer (FrameworkElement, IVector (FrameworkElement))** . Seuls les contrôles que vous transmettez au deuxième paramètre sont accessibles en un clic.

### <a name="required-native-ad-elements"></a>Éléments ad natifs requis

Au minimum, vous devez toujours afficher les éléments ad natifs suivants fournis par les propriétés de l’objet [NativeAdV2](/uwp/api/microsoft.advertising.winrt.ui.nativeadv2) à l’utilisateur dans votre conception ad native. Si vous ne parvenez pas à inclure ces éléments, vous pouvez constater des performances médiocres et de faibles rendements pour votre unité ad.

1. Affichez toujours le titre de la publicité native (disponible dans la propriété **titre** ). Fournissez suffisamment d’espace pour afficher au moins 25 caractères. Si le titre est plus long, remplacez le texte supplémentaire par des points de suspension.
2. Affichez toujours au moins l’un des éléments suivants pour différencier l’expérience ad native du reste de votre application et rappelez clairement que le contenu est fourni par un annonceur :
    * Icône *ad* pouvant être distinguée (disponible dans la propriété **AdIcon** ). Cette icône est fournie par Microsoft.
    * Le texte *sponsorisé par* (disponible dans la propriété **SponsoredBy** ). Ce texte est fourni par l’annonceur.
    * Comme alternative au texte *sponsorisé par* , vous pouvez choisir d’afficher un autre texte qui permet de différencier l’expérience ad native du reste de votre application, par exemple le « contenu sponsorisé », le « contenu promotionnel », le « contenu recommandé », etc.

### <a name="user-experience"></a>Expérience utilisateur

Votre publicité Native doit être clairement délimitée du reste de votre application et comporter de l’espace pour empêcher les clics accidentels. Utilisez des bordures, des arrière-plans différents ou une autre interface utilisateur pour séparer le contenu publicitaire du reste de votre application. N’oubliez pas que les clics publicitaires accidentels ne sont pas bénéfiques pour votre chiffre d’affaires basé sur l’annuaire Active Directory ou votre expérience utilisateur final à long terme.

### <a name="description"></a>Description

Si vous choisissez d’afficher la description de la publicité (disponible dans la propriété **Description** de l’objet **NativeAdV2** ), fournissez suffisamment d’espace pour afficher au moins 75 caractères. Nous vous recommandons d’utiliser une animation pour afficher le contenu complet de la description de la publicité.

### <a name="call-to-action"></a>Invite à l’action

L' *appel de* texte d’action (disponible dans la propriété **CallToAction** de l’objet **NativeAdV2** ) est un composant essentiel de la publicité. Si vous choisissez d’afficher ce texte, suivez ces instructions :

* Affiche toujours l' *appel* du texte d’action à l’utilisateur sur un contrôle cliquable, tel qu’un bouton ou un lien hypertexte.
* Affiche toujours l' *appel au* texte de l’action dans son intégralité.
* Assurez-vous que l' *appel au* texte d’action est séparé du reste du texte promotionnel de l’annonceur.