---
title: Monétisation pour les jeux
description: Implémentez des bannières publicitaires, des spots vidéo publicitaires et des achats in-app pour les jeux de la plateforme Windows universelle (UWP) sur Windows 10.
ms.assetid: 79f4e177-d8e7-45d3-8a78-31d4c2fe298a
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, jeux, monétisation
ms.localizationpriority: medium
ms.openlocfilehash: 009d4740fed47c7cde392d41bf52384071715106
ms.sourcegitcommit: afc4ff2c89f148d32073ab1cc42063ccdc573a8c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104620"
---
#  <a name="monetization-for-games"></a>Monétisation pour les jeux

En tant que développeur de jeux, vous devez connaître vos options de monétisation afin de maintenir votre activité et de continuer à faire ce qui vous passionne : créer des jeux exceptionnels. Cet article fournit une vue d’ensemble des méthodes de monétisation pour un jeu de la plateforme Windows universelle (UWP) et de la manière de les implémenter.

Par le passé, vous auriez simplement fixé un prix pour votre jeu, puis attendu que des clients l’achètent dans un magasin. Aujourd'hui, des options supplémentaires s’offrent à vous. Vous pouvez choisir de distribuer un jeu dans des magasins « physiques », de vendre le jeu en ligne (copies physiques ou logicielles), ou permettre à tout le monde de jouer au jeu gratuitement, tout en intégrant une forme de publicité ou des articles pouvant être achetés dans le jeu. En outre, les jeux ne sont plus des produits autonomes. Ils sont souvent fournis avec du contenu supplémentaire qui peut être acheté en plus du jeu principal.

Vous pouvez promouvoir et monétiser un jeu UWP de l’une ou de plusieurs des manières suivantes :
* Mettez votre jeu dans le Microsoft Store, qui est un magasin en ligne sécurisé qui offre une [distribution mondiale](#worldwide-distribution-channel). Dans le monde entier, les joueurs peuvent acheter votre jeu en ligne au [prix que vous avez fixé](#set-a-price-for-your-game).
* Utilisez des API dans le SDK Windows pour créer des [achats dans le jeu](#in-game-purchases). Les joueurs peuvent acheter des articles à partir de votre jeu, ou acheter du contenu supplémentaire tel que des équipements supplémentaires, des apparences, des cartes ou des niveaux de jeu.
* Utilisez les API du [Kit de développement logiciel (SDK) Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) pour afficher les publicités des réseaux Active Directory. Vous pouvez [afficher des publicités dans votre jeu](#display-ads-in-your-game) et offrir aux joueurs la possibilité de regarder des publicités vidéo en échange de récompenses dans le jeu.
* [Optimisez le potentiel de votre jeu par le biais de campagnes de publicité](#maximize-your-games-potential-through-ad-campaigns). Faites la promotion de votre jeu à l’aide de campagnes de publicité payantes, communautaires (gratuites) ou maison (gratuites) pour développer la base des utilisateurs.

## <a name="worldwide-distribution-channel"></a>Canal de distribution mondial

La Microsoft Store peut rendre votre jeu disponible en téléchargement dans plus de 200 pays et régions dans le monde entier, avec la prise en charge de la facturation par le biais de différentes formes de paiement, notamment Visa, MasterCard et PayPal. Pour obtenir la liste complète des pays et régions, consultez [définir la sélection du marché](../publish/define-market-selection.md).

## <a name="set-a-price-for-your-game"></a>Définir un prix pour votre jeu

Les jeux UWP publiés dans le Windows Store peuvent être _payants_ ou _gratuits_. Avec un jeu payant, vous facturez les joueurs à l’avance pour votre jeu à un prix que vous avez défini, alors qu’un jeu gratuit permet aux utilisateurs de télécharger le jeu et d’y jouer sans payer pour cela.

Voici quelques concepts importants concernant la tarification de votre jeu dans le Windows Store.

### <a name="base-price"></a>Prix de base

Le prix de base du jeu est l’élément qui détermine si votre jeu est classé comme _payant_ ou _gratuit_. Vous pouvez utiliser l' [espace partenaires](https://partner.microsoft.com/dashboard) pour configurer le prix de base en fonction du pays et de la région.
Le processus de détermination du prix peut inclure vos [responsabilités fiscales lors de la vente à différents pays](/partner-center/tax-details-marketplace) et des [considérations relatives au coût pour des marchés spécifiques](../publish/define-market-selection.md). En outre, vous pouvez [définir des prix personnalisés pour des marchés spécifiques](../publish/set-and-schedule-app-pricing.md#override-base-price-for-specific-markets).

### <a name="sale-price"></a>Prix de vente

Une façon de promouvoir votre jeu consiste à réduire son prix pendant une période limitée. Vous pouvez également définir le prix de vente comme étant __gratuit__ pour permettre le téléchargement de votre jeu sans paiement.
Vous pouvez planifier des campagnes de vente à l’avance, en définissant à la fois la date de début et la date de fin de la vente. Pour plus d’informations, voir [Vendre des applications et des composants additionnels à prix réduit](../publish/put-apps-and-add-ons-on-sale.md).

## <a name="in-game-purchases"></a>Achats dans le jeu

Les achats dans le jeu sont des produits achetés au sein d’un jeu. Ils sont également appelés de manière générique _achats in-app_. Dans le Microsoft Store, ces produits sont appelés _modules_ complémentaires. [Les modules complémentaires sont publiés](../publish/add-on-submissions.md) via l’espace partenaires. Vous devrez également activer les composants additionnels dans le code de votre jeu.

### <a name="types-of-add-ons"></a>Types de composants additionnels

Vous pouvez créer deux types de composants additionnels dans le Windows Store : _Durables_ ou _Consommables_. Les composants additionnels durables sont des articles qui persistent pendant un laps de temps spécifié et ne peuvent être achetés qu’une seule fois avant expiration. Les composants additionnels consommables sont des articles qui peuvent être achetés et utilisés encore et encore.

Lorsque vous créez des consommables, décidez de la manière dont vous souhaitez les suivre &mdash;, selon qu’ils sont _gérés par le développeur_ ou _gérés par le Windows Store_ (cette fonctionnalité est disponible à partir de Windows 10, version 1607). Avec un consommable géré par le développeur, vous êtes chargé de suivre le solde de l’élément pour le joueur ; avec un consommable géré par le magasin, le Microsoft Store effectue le suivi de l’équilibre de l’article. Pour plus d’informations, voir [Vue d’ensemble des composants additionnels consommables](../monetize/enable-consumable-add-on-purchases.md).

### <a name="create-in-game-purchases"></a>Créer des achats dans le jeu

Les achats in-app les plus récents et les API d’informations de licence font partie de l’espace de noms [Windows.Services.Store](/uwp/api/windows.services.store) dans le SDK Windows (à partir de Windows 10, version 1607). Si vous développez un nouveau jeu qui cible la version 1607 ou une version ultérieure, nous vous recommandons d’utiliser l’espace de noms __Windows.Services.Store__, car il prend en charge les types de composants additionnels les plus récents et offre de meilleures performances.
Il est également conçu pour être compatible avec les futurs types de produits et fonctionnalités pris en charge par l’espace partenaires et le Store. Lors du développement pour de précédentes versions de Windows 10, utilisez l’espace de noms [Windows.ApplicationModel.Store](/uwp/api/windows.applicationmodel.store) à la place.

Pour plus d’informations, accédez à [Achats in-app et versions d’évaluation](../monetize/in-app-purchases-and-trials.md).

#### <a name="simplified-purchase-example"></a>Exemple d’achat simplifié

Cette section présente un exemple d’achat simplifié pour illustrer l’utilisation d’appels de méthode différents afin d’implémenter le flux d’achat.

|Actions / activité dans le jeu                                                | Tâches en arrière-plan dans le jeu                  |
|--------------------------------------------------------------------------|----------------------------------------|
|Le joueur entre dans un magasin. Le menu Magasin s’ouvre pour afficher les composants additionnels disponibles et le prix d’achat |  Le jeu [récupère les informations de produit](../monetize/get-product-info-for-apps-and-add-ons.md) sur les composants additionnels, [détermine si les composants additionnels disposent de la licence appropriée](../monetize/get-license-info-for-apps-and-add-ons.md), et affiche les composants additionnels qui sont disponibles pour l’achat par le joueur sur le menu Magasin.                           |
|Le joueur clique sur __Acheter__ pour acheter un article             |L’action __Acheter__ envoie une demande d’achat de l’article et démarre le processus de paiement pour l’acquérir. L’implémentation varie selon le type d’article. S’il s’agit [d’un article durable ou d’un achat ponctuel](../monetize/enable-in-app-purchases-of-apps-and-add-ons.md), le client peut uniquement posséder un seul article jusqu’à son expiration. Si l’article est un [consommable](../monetize/enable-consumable-add-on-purchases.md), le client peut en posséder un ou plusieurs. |

### <a name="test-in-game-purchases-during-game-development"></a>Tester les achats dans le jeu pendant le développement de jeux

Dans la mesure où un composant additionnel doit être créé en association avec un jeu, votre jeu doit être publié et disponible dans le Windows Store. Les étapes décrites dans cette section montrent comment créer des composants additionnels tandis que votre jeu est toujours en développement.
(Si votre jeu fini est déjà actif dans le Windows Store, vous pouvez ignorer les trois premières étapes et aller directement à la section [Créer un composant additionnel dans le Windows Store](#create-an-add-on-in-the-store).)

Pour créer des composants additionnels tandis que votre jeu est toujours en développement :
1. [Créer un package](#create-a-package)
2. [Publier le jeu comme étant masqué](#publish-the-game-as-hidden)
3. [Associer votre solution de jeu dans Visual Studio avec le Windows Store](#associate-your-game-solution-with-the-store)
4. [Créer un composant additionnel dans le Windows Store](#create-an-add-on-in-the-store)

#### <a name="create-a-package"></a>Créer un package

Tout jeu à publier doit respecter la configuration minimale du Kit de certification des applications Windows. Vous pouvez utiliser le [Kit de certification des applications Windows](../debug-test-perf/windows-app-certification-kit.md), qui fait partie du Kit de développement logiciel Windows 10, pour exécuter des tests sur le jeu afin de s’assurer qu’il est prêt pour la publication dans le Windows Store. Si vous n’avez pas déjà téléchargé le Kit de développement logiciel Windows 10 qui inclut le Kit de certification des applications Windows, accédez au [Kit de développement logiciel Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk).

Pour créer un package qui peut être chargé dans le Windows Store :

1. Ouvrez votre solution de jeu dans Visual Studio.
2. Dans Visual Studio, accédez à __projet__  >  __Store__  >  __créer des packages d’application...__
3. Pour l’option __voulez-vous générer des packages à télécharger vers l’Microsoft Store ?__ , sélectionnez __Oui__.
4. Connectez-vous à votre compte de développeur de l' [espace partenaires](https://partner.microsoft.com/dashboard) . Si vous n’avez pas de compte de développeur, [inscrivez-vous](https://developer.microsoft.com/store/register) pour en obtenir un.
5. Sélectionnez une application pour laquelle créer le package de chargement. Si vous n’avez pas encore créé de soumission d’application, fournissez un nouveau nom d’application pour créer une soumission. Pour plus d’informations, voir [Créer votre application en réservant un nom](../publish/create-your-app-by-reserving-a-name.md).
6. Une fois le package correctement créé, cliquez sur __Lancer le Kit de certification des applications Windows__ pour démarrer le processus de test.
7. Corrigez toutes les erreurs pour créer un package de jeu.

#### <a name="publish-the-game-as-hidden"></a>Publier le jeu comme étant masqué

1. Accédez à l' [espace partenaires](https://partner.microsoft.com/dashboard) et connectez-vous.
2. Depuis la __page de présentation du tableau de bord__ ou la page __Toutes les applications__, cliquez sur l’application avec laquelle vous souhaitez travailler. Si vous n’avez pas encore créé de soumission d’application, cliquez sur __Créer une application__ et réservez un nom.
3. Sur la page __Vue d’ensemble de l’application__, cliquez sur __Démarrer votre soumission__.
4. Configurez cette nouvelle soumission. Sur la page de la soumission :
    * Cliquez sur __Tarification et disponibilité__. Dans la section __visibilité__ , choisissez «__masquer cette application et empêcher l’acquisition...__» pour vous assurer que seul votre équipe de développement a accès au jeu. Pour plus d’informations, accédez à [Distribution et visibilité](../publish/set-app-pricing-and-availability.md).
    * Cliquez sur __Propriétés__. Dans la section __Catégorie et sous-catégorie__, choisissez __Jeux__, puis sélectionnez une sous-catégorie adaptée à votre jeu.
    * Cliquez sur __Classifications par âge__. Remplissez le questionnaire avec précision.
    * Cliquez sur __Packages__. Chargez le package de jeu créé à l’étape précédente.
5. Suivez les autres invites de soumission dans le tableau de bord afin de publier correctement ce jeu qui reste masqué au public.
6. Cliquez sur __Soumettre dans le Windows Store__.

Pour plus d’informations, accédez à [Soumissions des applications](../publish/app-submissions.md).

Une fois que votre jeu est soumis dans le Windows Store, il entre dans le [processus de certification d’application](../publish/the-app-certification-process.md). Ce processus peut prendre jusqu’à 16 heures avant que le jeu soit répertorié.

#### <a name="associate-your-game-solution-with-the-store"></a>Associer votre solution de jeu avec le Windows Store

Avec votre solution de jeu ouverte dans Visual Studio :

1. Accéder à   >    >  __l’application associer le magasin de projets au Store...__
2. Connectez-vous à votre compte de développeur de l’espace partenaires et sélectionnez le nom de l’application avec laquelle associer cette solution.
3. Double-cliquez sur le __fichier Package.appxmanifest.xml__ et accédez à l’onglet __Empaquetage__ afin de vérifier que le jeu est correctement associé.

Si vous avez associé la solution à un jeu publié qui est actif et répertorié dans le Windows Store, votre solution dispose d’une licence active et vous vous rapprochez de la création de composants additionnels pour votre jeu. Pour plus d’informations, consultez [Empaquetage d’applications](../packaging/index.md).

#### <a name="create-an-add-on-in-the-store"></a>Créer un composant additionnel dans le Windows Store

Lorsque vous créez des composants additionnels, assurez-vous que vous les associez à la soumission de jeu appropriée. Pour plus d’informations sur la configuration de toutes les informations diverses associées à un composant additionnel, voir [Soumissions de composants additionnels](../publish/add-on-submissions.md).

1. Accédez à l' [espace partenaires](https://partner.microsoft.com/dashboard) et connectez-vous.
2. Depuis la __page de présentation du tableau de bord__ ou la page __Toutes les applications__, cliquez sur l’application pour laquelle vous souhaitez créer le composant additionnel.
3. Sur la page __Vue d’ensemble de l’application__, dans la section __Composants additionnels__, sélectionnez __Créer un composant additionnel__.
4. Sélectionnez le type de produit pour le composant additionnel : __consommable géré par le développeur__, __consommable géré par le Windows Store__ ou __durable__.
5. Entrez un ID de produit unique qui sera utilisé comme une variable de chaîne lors de l’intégration de ce composant additionnel dans le code de votre jeu. Cet ID ne sera pas visible par les consommateurs. Pour plus d’informations, consultez [Définir le type et l’ID de produit de votre application](../publish/set-your-add-on-product-id.md).

Autres configurations pour les composants additionnels :
* [Propriétés](../publish/enter-add-on-properties.md)
* [Tarification et disponibilité](../publish/set-add-on-pricing-and-availability.md)
* [Liste des boutiques](../publish/create-add-on-store-listings.md)

Si votre jeu comporte de nombreux modules complémentaires, vous pouvez les créer par programme à l’aide de l' __API de soumission Microsoft Store__. Pour plus d’informations, consultez [créer et gérer des envois à l’aide des services Microsoft Store](../monetize/create-and-manage-submissions-using-windows-store-services.md).

## <a name="display-ads-in-your-game"></a>Afficher des publicités dans votre jeu

Les bibliothèques et les outils du kit de développement logiciel (SDK) Microsoft Advertising vous aident à configurer un service dans votre jeu pour recevoir des publicités d’un réseau Active Directory. Vos joueurs verront des publicités en direct et vous gagnerez de l’argent auprès des annonceurs lorsque vos joueurs verront les publicités affichées ou interagiront avec celles-ci.
Pour plus d’informations, consultez [afficher des publicités dans votre application](../monetize/display-ads-in-your-app.md).

### <a name="ad-formats"></a>Formats de publicité

Plusieurs types de publicités peuvent être affichées à l’aide du kit de développement logiciel (SDK) Microsoft Advertising :

* Bannières publicitaires &mdash; Publicités qui occupent une partie de votre écran de jeu et sont généralement placées dans un jeu.
* Spots vidéo publicitaires &mdash; Publicités plein écran, qui peuvent se révéler très efficaces lorsqu’elles sont utilisées entre les niveaux. Si elles sont correctement implémentées, elles peuvent être moins intrusives que des bannières publicitaires.
* Publicités natives &mdash; basées sur les composants, où chaque élément créatif (par exemple, le titre, l’image, la description et le texte d’appel à l’action) est remis à votre application sous la forme d’un élément individuel que vous pouvez intégrer à votre application.

### <a name="which-ads-are-displayed"></a>Quelles publicités s’affichent ?

Par défaut, votre application affiche les publicités du réseau de Microsoft pour les publicités payantes. Pour optimiser le chiffre d’affaires de votre publicité, vous pouvez activer la médiation AD pour votre unité ad afin d’afficher les publicités de réseaux publicitaires payants supplémentaires. Pour plus d’informations sur les offres actuelles, consultez notre guide de [médiation ad](../publish/in-app-ads.md#mediation) .

### <a name="which-markets-allow-ads-to-be-displayed"></a>Quels marchés autorisent l’affichage des publicités ?

Pour obtenir la liste complète des pays et régions qui prennent en charge les publicités, consultez [marchés pris en charge pour les réseaux Active Directory](../publish/in-app-ads.md#network-markets).

### <a name="apis-for-displaying-ads"></a>API pour afficher des publicités

Les classes [classe AdControl](/uwp/api/microsoft.advertising.winrt.ui.adcontrol), [InterstitialAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad)et [NativeAd](/uwp/api/microsoft.advertising.winrt.ui.nativead) dans le kit de développement logiciel (SDK) Microsoft Advertising sont utilisées pour faciliter l’affichage des publicités dans les jeux.

Pour commencer, téléchargez et installez le [Kit de développement logiciel (SDK) Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) avec Visual Studio 2015 ou une version ultérieure. Pour plus d’informations, consultez [installer le kit de développement logiciel (SDK) Microsoft Advertising](../monetize/install-the-microsoft-advertising-libraries.md).

#### <a name="implementation-guides"></a>Guides d’implémentation

Ces procédures pas à pas montrent comment implémenter des publicités à l’aide de __classe AdControl__, __InterstitialAd__ et __NativeAd__:

* [Créer des bannières publicitaires en XAML et .NET](../monetize/adcontrol-in-xaml-and--net.md)
* [Créer des bannières publicitaires en HTML5 et JavaScript](../monetize/adcontrol-in-html-5-and-javascript.md)
* [Créer des publicités interstitielles](../monetize/interstitial-ads.md)
* [Créer des publicités natives](../monetize/native-ads.md)

Pendant le développement, vous pouvez utiliser les valeurs de l' [unité ad de test](../monetize/set-up-ad-units-in-your-app.md) pour voir comment les publicités sont affichées. Ces valeurs d’unité ad de test sont également utilisées dans les procédures pas à pas ci-dessus.

Voici quelques meilleures pratiques pour vous aider dans le processus de conception et d’implémentation.

* [Meilleures pratiques pour les bannières publicitaires](../monetize/ui-and-user-experience-guidelines.md)
* [Meilleures pratiques pour les spots vidéo publicitaires](../monetize/ui-and-user-experience-guidelines.md)

Pour des solutions aux problèmes de développement courants, tels que le non-affichage des publicités, le clignotement et la disparition de la boîte noire ou la non-actualisation des publicités, voir [Guides de dépannage](../monetize/known-issues-for-the-advertising-libraries.md).

### <a name="prepare-for-release-by-replacing-ad-unit-test-values"></a>Se préparer au lancement en remplaçant les valeurs de test de l’unité publicitaire

Lorsque vous êtes prêt à passer au test en direct ou à recevoir des publicités dans les jeux publiés, vous devez mettre à jour les valeurs de test de l’unité publicitaire vers les valeurs réelles fournies pour votre jeu. Pour créer des unités publicitaires pour votre jeu, voir [Configurer des unités publicitaires dans votre application](../monetize/set-up-ad-units-in-your-app.md).

### <a name="other-ad-networks"></a>Autres réseaux publicitaires

Il s’agit d’autres réseaux Active Directory qui fournissent des kits de développement logiciel (SDK) pour les applications et les jeux UWP.

#### <a name="vungle"></a>Vungle

Le Kit de développement logiciel (SDK) Vungle pour Windows offre des publicités vidéo dans les jeux et les applications. Pour télécharger le Kit de développement logiciel, accédez au [Kit de développement logiciel (SDK) Vungle](https://publisher.vungle.com/sdk/).

#### <a name="smaato"></a>Smaato

Smaato permet aux bannières publicitaires d’être incorporées aux jeux et aux applications UWP. Téléchargez le [Kit de développement logiciel](https://www.smaato.com/resources/sdks/) et pour plus d’informations, voir la [documentation](https://wiki.smaato.com/display/SPX/Windows+Phone).

#### <a name="adduplex"></a>AdDuplex

Vous pouvez utiliser AdDuplex pour mettre en œuvre les bannières ou spots publicitaires dans votre jeu.

Pour en savoir plus sur l’intégration de AdDuplex directement dans un projet XAML Windows 10, accédez au site web AdDuplex :
* Bannières publicitaires : [Kit de développement logiciel Windows 10 pour XAML](https://adduplex.zendesk.com/hc/articles/204849031-Windows-10-SDK-for-XAML-apps-installation-and-usage)
* Spots publicitaires : [Installation et utilisation des spots publicitaires AdDuplex XAML Windows 10](https://adduplex.zendesk.com/hc/articles/204849091-Windows-10-XAML-AdDuplex-Interstitial-Ad-Installation-and-Usage)

Pour plus d’informations sur l’intégration du Kit de développement logiciel AdDuplex dans les jeux UWP Windows 10 créés à l’aide de Unity, voir [Installation et utilisation du Kit de développement logiciel (SDK) Windows 10 pour les applications Unity](https://adduplex.zendesk.com/hc/articles/207279435-Windows-10-SDK-for-Unity-apps-installation-and-usage).

## <a name="maximize-your-games-potential-through-ad-campaigns"></a>Optimiser le potentiel de votre jeu par le biais de campagnes de publicité

Passez au niveau supérieur de la promotion de votre jeu à l’aide des publicités. Lorsque vous [créez une campagne de publicité](../monetize/index.md) pour votre jeu, les autres applications et jeux affichent des publicités de promotion de votre jeu.

Faites votre choix parmi plusieurs types de campagnes qui peuvent aider à augmenter votre base de joueurs.

|Type de campagne             | Les publicités pour votre jeu s’affichent dans...                                                                                                                                                                   |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|Payant                      |Applications qui correspondent à l’appareil ou à la catégorie de votre jeu.                                                                                                                                                   |
|Communauté gratuite            |Applications publiées par d’autres développeurs qui ont également adhéré aux campagnes publicitaires de la communauté. Pour en savoir plus, voir [À propos des annonces de la communauté](../monetize/index.md).|
|Campagne gratuite                |Seulement les applications que vous avez publiées. Pour plus d’informations, voir [À propos des publicités maison](../monetize/index.md).                                                            |

## <a name="related-links"></a>Liens connexes

* [Rémunération](/partner-center/marketplace-get-paid)
* [Types de comptes, lieux géographiques et frais](../publish/account-types-locations-and-fees.md)
* [Analyse](../publish/analytics.md)
* [Globalisation et localisation](../design/globalizing/globalizing-portal.md)
* [Implémenter une version d’évaluation de votre application](../monetize/implement-a-trial-version-of-your-app.md)
* [Exécuter des expériences d’application avec des tests A/B](../monetize/run-app-experiments-with-a-b-testing.md)