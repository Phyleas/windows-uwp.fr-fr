---
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: Découvrez comment ajouter l’ID d’application et les valeurs d’ID d’unité ad de l’espace partenaires à votre application avant de soumettre votre application au Windows Store.
title: Configurer des unités publicitaires dans votre application
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, ADS, publicité, unités ad, test
ms.localizationpriority: medium
ms.openlocfilehash: c7bafdc7d21814a03d6f7da7132d8017d7f238e5
ms.sourcegitcommit: c2e4bbe46c7b37be1390cdf3fa0f56670f9d34e9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/20/2020
ms.locfileid: "92253633"
---
# <a name="set-up-ad-units-in-your-app"></a>Configurer des unités publicitaires dans votre application

>[!WARNING]
> Depuis le 1er juin 2020, la plateforme de monétisation Microsoft AD pour les applications Windows UWP sera arrêtée. [En savoir plus](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Chaque contrôle d’Active Directory de votre application plateforme Windows universelle (UWP) a une *unité publicitaire* correspondante qui est utilisée par nos services pour servir les publicités au contrôle. Chaque unité ad se compose d’un *ID d’unité Active Directory* et d’un *ID d’application* que vous devez attribuer au code de votre application.

Nous fournissons des [valeurs d’unité ad de test](#test-ad-units) que vous pouvez utiliser pendant le test pour confirmer que votre application affiche des publicités de test. Ces valeurs de test ne peuvent être utilisées que dans une version test de votre application. Si vous essayez d’utiliser des valeurs de test dans votre application après l’avoir publiée, votre application dynamique ne recevra pas de publicités.

Une fois que vous avez terminé de tester votre application UWP et que vous êtes prêt à la soumettre à l’espace partenaires, vous devez [créer une unité ad active](#live-ad-units) à partir de la page [ADS dans l’application](../publish/in-app-ads.md) dans l’espace partenaires, puis mettre à jour votre code d’application pour utiliser les valeurs ID d’application et ID d’unité Active Directory pour cette unité Active Directory.

Pour plus d’informations sur l’affectation de l’ID d’application et des valeurs d’ID d’unité ad dans le code de votre application, consultez les articles suivants :
* [AdControl en XAML et .NET](adcontrol-in-xaml-and--net.md)
* [Classe AdControl en HTML 5 et JavaScript](adcontrol-in-html-5-and-javascript.md)
* [Spots publicitaires](../monetize/interstitial-ads.md)
* [Publicités natives](../monetize/native-ads.md)

<span id="test-ad-units" />

## <a name="test-ad-units"></a>Unités AD de test

Pendant le développement de votre application, utilisez les valeurs ID d’application de test et ID d’unité ad de cette section pour voir comment votre application affiche les publicités pendant le test.

### <a name="banner-ads-using-the-adcontrol-class"></a>Bannières publicitaires (à l’aide de la classe classe AdControl)

* ID de l’unité Active Directory : ```test```
* ID de l’application :  ```3f83fe91-d6be-434d-a0ae-7351c5a997f1```

    > [!IMPORTANT]
    > Pour un **classe AdControl**, la taille d’une publicité active est définie par les propriétés **Width** et **Height** . Pour obtenir de meilleurs résultats, vérifiez que les propriétés **Width** et **Height** de votre code font partie des [tailles de bannières publicitaires prises en charge](supported-ad-sizes-for-banner-ads.md). Les propriétés **Width** et **Height** ne changent pas en fonction de la taille d’une publicité dynamique.

### <a name="interstitial-ads-and-native-ads"></a>Publicités interstitielles et publicités natives

* ID de l’unité Active Directory : ```test```
* ID de l’application :  ```d25517cb-12d4-4699-8bdc-52040c712cab```

<span id="live-ad-units" />

## <a name="live-ad-units"></a>Unités ad actives

Pour obtenir une unité ad active de l’espace partenaires et l’utiliser dans votre application :

1.  [Créez une unité ad](../publish/in-app-ads.md#create-ad-unit) sur la page **ADS dans l’application** dans l’espace partenaires. Veillez à spécifier le type d’unité ad approprié pour le contrôle Active Directory que vous utilisez dans votre application.
    > [!NOTE]
    > Vous pouvez éventuellement activer la médiation AD pour votre unité ad en configurant les paramètres dans la section [paramètres de médiation](../publish/in-app-ads.md#mediation) . La médiation ad vous permet d’optimiser votre chiffre d’affaires et vos fonctionnalités de promotion d’applications en affichant les publicités de plusieurs réseaux Active Directory, y compris les publicités d’autres réseaux et publicités payants pour les campagnes de promotion des applications Microsoft. Par défaut, nous choisissons automatiquement les paramètres de médiation AD pour votre application à l’aide d’algorithmes d’apprentissage automatique pour vous aider à maximiser vos revenus de publicité sur les marchés pris en charge par votre application, mais vous pouvez éventuellement configurer vos paramètres de médiation manuellement.

2.  Une fois que vous avez créé la nouvelle unité ad, récupérez l’ID de l' **application** et l’ID de l' **unité** AD pour l’unité ad **dans la table** des unités ad disponibles dans la &gt; page **publicités dans l’application** .
    > [!NOTE]
    > Les valeurs d’ID d’application pour les unités AD de test et les unités ad UWP activent des formats différents. Les valeurs d’ID d’application de test sont des GUID. Lorsque vous créez une unité ad UWP en temps réel dans l’espace partenaires, la valeur de l’ID d’application pour l’unité ad correspond toujours à l’ID de magasin de votre application (un exemple de valeur d’ID de magasin ressemble à 9NBLGGH4R315).

3.  Affectez les valeurs ID d’application et ID d’unité ad dans le code de votre application. Pour plus d’informations, consultez les articles suivants :
    * [AdControl en XAML et .NET](adcontrol-in-xaml-and--net.md)
    * [Classe AdControl en HTML 5 et JavaScript](adcontrol-in-html-5-and-javascript.md)
    * [Spots publicitaires](../monetize/interstitial-ads.md)
    * [Publicités natives](../monetize/native-ads.md)

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>Gérer les unités AD pour plusieurs contrôles Active Directory dans votre application

Vous pouvez utiliser plusieurs commandes de bannière, interstitielles et natives Active Directory dans une même application. Dans ce scénario, nous vous recommandons d’affecter une unité ad différente à chaque contrôle. L’utilisation de différentes unités AD pour chaque contrôle vous permet de [configurer séparément les paramètres de médiation](../publish/in-app-ads.md#mediation) et d’obtenir des données de [rapport](../publish/advertising-performance-report.md) discrètes pour chaque contrôle. Cela permet également à nos services de mieux optimiser les publicités que nous avons utilisées dans votre application.

> [!IMPORTANT]
> Vous pouvez utiliser chaque unité ad dans une seule application. Si vous utilisez une unité ad dans plusieurs applications, les publicités ne sont pas prises en charge pour cette unité Active Directory.

## <a name="related-topics"></a>Rubriques connexes

* [AdControl en XAML et .NET](adcontrol-in-xaml-and--net.md)
* [Classe AdControl en HTML 5 et JavaScript](adcontrol-in-html-5-and-javascript.md)
* [Spots publicitaires](interstitial-ads.md)
* [Publicités natives](native-ads.md)


 

 
