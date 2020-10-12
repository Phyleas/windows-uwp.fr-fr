---
ms.assetid: 63A9EDCF-A418-476C-8677-D8770B45D1D7
description: Le kit de développement logiciel (SDK) Microsoft Advertising vous offre plusieurs façons de monétiser votre application avec des publicités.
title: Afficher des publicités dans votre application avec le SDK Microsoft Advertising
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, ADS, publicité, bannière, contrôle publicitaire, interstitiel
ms.localizationpriority: medium
ms.openlocfilehash: c12d79b97010826b05bf42a9de46780dd2f93756
ms.sourcegitcommit: 53c00939b20d4b0a294936df3d395adb0c13e231
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91933120"
---
# <a name="display-ads-in-your-app-with-the-microsoft-advertising-sdk"></a>Afficher des publicités dans votre application avec le SDK Microsoft Advertising

>[!WARNING]
> Depuis le 1er juin 2020, la plateforme de monétisation Microsoft AD pour les applications Windows UWP sera arrêtée. [En savoir plus](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Augmentez vos opportunités de revenus en plaçant des publicités dans votre application plateforme Windows universelle (UWP) pour Windows 10 à l’aide du kit de développement logiciel (SDK) Microsoft Advertising. Notre plateforme de monétisation ad offre un large éventail de formats ad qui peuvent être intégrés en toute transparence à vos applications et qui prend en charge la médiation avec de nombreux réseaux Active Directory populaires. Notre plateforme est conforme aux normes OpenRTB, VAST 2.x, MRAID 2, et VPAID 3 et est compatible avec MOAT et IAS. 

<br/>

<table style="border: none !important;">
<colgroup>
<col width="10%" />
<col width="23%" />
<col width="10%" />
<col width="23%" />
<col width="10%" />
<col width="23%" />
</colgroup>
<tbody>
<tr>
<td align="left"><img src="images/install-sdk.png" alt="Install SDK icon" /></td>
<td align="left"><b>Prise en main</b><br/><br/>
    <a href="https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK">Installer le kit de développement logiciel (SDK) Microsoft Advertising</a>
</td>
<td align="left"><img src="images/write-code.png" alt="Develop icon" /></td>
<td align="left"><b>Guides pour les développeurs</b><br/><br/>
    <a href="banner-ads.md">Bannières publicitaires</a>
    <br/>
    <a href="interstitial-ads.md">Publicités interstitielles</a>
    <br/>
    <a href="native-ads.md">Publicités natives</a>
    </td>
<td align="left"><img src="images/api-reference.png" alt="API ref icon" /></td>
<td align="left"><b>Autres ressources</b><br/><br/>
    <a href="set-up-ad-units-in-your-app.md">Configurer des unités ad dans votre application</a>
    <br/>
    <a href="best-practices-for-ads-in-apps.md">Meilleures pratiques</a>
    <br/>
    <a href="/uwp/api/overview/advertising">Référence d’API</a>
    </td>
</tr>
</tbody>
</table>

## <a name="step-1-install-the-microsoft-advertising-sdk"></a>Étape 1 : installer le kit de développement logiciel (SDK) Microsoft Advertising

Pour commencer, installez le kit de développement [logiciel (SDK) Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) sur l’ordinateur de développement que vous utilisez pour créer votre application. Pour obtenir des instructions d’installation, consultez [cet article](install-the-microsoft-advertising-libraries.md).

## <a name="step-2-implement-ads-in-your-app"></a>Étape 2 : implémenter des publicités dans votre application

Le kit de développement logiciel (SDK) Microsoft Advertising fournit différents types de contrôles Active Directory que vous pouvez utiliser dans votre application. Choisissez les types de publicités les plus adaptés à votre scénario, puis ajoutez du code à votre application pour afficher ces publicités. Au cours de cette étape, vous allez utiliser une unité ad de test afin de voir comment votre application affiche les publicités pendant le test.

### <a name="banner-ads"></a>Bannières publicitaires

Il s’agit d’affichages de publicités statiques qui utilisent une partie rectangulaire d’une page de votre application pour afficher le contenu promotionnel. Ces publicités peuvent être actualisées automatiquement à intervalles réguliers. Il s’agit d’un bon point de départ si vous débutez avec la publication dans votre application.

Pour obtenir des instructions et des exemples de code, consultez [cet article](adcontrol-in-xaml-and--net.md).

![Image illustrant une publicité de bannière sur une tablette.](images/banner-ad.png)

### <a name="interstitial-video-and-interstitial-banner-ads"></a>Vidéos interstitielles et publicités de bannières interstitielles

Il s’agit de publicités plein écran qui requièrent généralement que l’utilisateur regarde une vidéo ou clique dessus pour continuer dans l’application ou le jeu. Nous prenons en charge deux types de publicités interstitielles : vidéo et bannière.

Pour obtenir des instructions et des exemples de code, consultez [cet article](interstitial-ads.md).

![Image représentant une publicité interstitielle dans un jeu qui est joué sur une tablette.](images/interstitial-ad.png)

### <a name="native-ads"></a>Publicités natives

Il s’agit de publicités basées sur les composants. Chaque élément créatif (par exemple, le titre, l’image, la description et le texte de l’appel à l’action) est remis à votre application sous la forme d’un élément individuel que vous pouvez intégrer à votre application à l’aide de vos propres polices, couleurs et autres composants de l’interface utilisateur.

Pour obtenir des instructions et des exemples de code, consultez [cet article](native-ads.md).

![Image représentant une publication native qui peut être affichée sur différents appareils.](images/native-ad.png)

<span id="ad-mediation"/>

## <a name="step-3-create-an-ad-unit-and-configure-mediation"></a>Étape 3 : créer une unité ad et configurer la médiation

Une fois que vous avez terminé le test de votre application et que vous êtes prêt à la soumettre au Store, créez une unité publicitaire sur la page [ADS dans l’application](../publish/in-app-ads.md) dans l’espace partenaires. Ensuite, mettez à jour votre code d’application pour utiliser cette unité ad afin que votre application reçoive des publicités en direct. Pour plus d’informations, voir [Configurer des unités publicitaires dans votre application](set-up-ad-units-in-your-app.md#live-ad-units).

Par défaut, votre application affiche les publicités du réseau de Microsoft pour les publicités payantes. Pour optimiser votre chiffre d’affaires, vous pouvez activer la [médiation ad](ad-mediation-service.md) pour votre unité ad afin d’afficher des publicités de réseaux publicitaires payants supplémentaires, tels que Taboola et Smaato. Vous pouvez également augmenter les capacités de promotion de votre application en fournissant des publicités à partir des campagnes de promotion des applications Microsoft.

Pour commencer à utiliser la médiation ad dans votre application UWP, [configurez les paramètres ad Mediation](../publish/in-app-ads.md#mediation-settings) pour votre unité ad. Par défaut, nous configurerons automatiquement les paramètres de médiation à l’aide d’algorithmes d’apprentissage automatique pour vous aider à maximiser vos revenus de publicité sur les marchés pris en charge par votre application. Toutefois, vous avez également la possibilité de choisir manuellement les réseaux que vous souhaitez utiliser. Dans les deux cas, les paramètres de médiation sont configurés entièrement sur nos serveurs. vous n’avez pas besoin d’apporter de modifications au code dans votre application.    

## <a name="step-4-submit-your-app-and-review-performance"></a>Étape 4 : soumettre votre application et examiner les performances

Une fois que vous avez terminé le développement de votre application avec ADS, vous pouvez [Envoyer votre application mise à jour](../publish/app-submissions.md) dans l’espace partenaires pour la rendre disponible dans le Store. Les applications qui affichent des publicités doivent répondre aux exigences supplémentaires spécifiées dans [la section 10,10 des stratégies de Microsoft Store](/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) et [présenter le contrat de développeur d’applications](/legal/windows/agreements/app-developer-agreement).

Une fois votre application publiée et disponible dans le Windows Store, vous pouvez consulter vos [rapports de performances publicitaires](../publish/advertising-performance-report.md) dans l’espace partenaires et continuer à apporter des modifications à vos paramètres de médiation pour optimiser les performances de vos publicités. Vos revenus publicitaires sont inclus dans votre [Résumé de paiement](../publish/payout-summary.md).

<span id="additional-help" />

## <a name="additional-help"></a>Aide supplémentaire

Pour obtenir une aide supplémentaire à l’aide du kit de développement logiciel (SDK) Microsoft Advertising, utilisez les ressources suivantes.

|  Tâche    | Ressource |               
|----------|-------|
| Signaler un bogue ou obtenir un support assisté en matière de publicité     | Visitez la [page de support](https://developer.microsoft.com/windows/support) et choisissez **ADS-en-applications**.        |
| Bénéficier du support de la communauté     | Consultez le [forum](https://social.msdn.microsoft.com/forums/windowsapps/en-US/home?category=windowsapps).       |
| Télécharger des exemples de projet qui montrent comment ajouter des bannières et des spots publicitaires aux applications.     | Voir [Exemples de publicité sur GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).       |
| Découvrir les dernières opportunités de monétisation pour les applications Windows     | Visitez [Monétiser vos applications](https://developer.microsoft.com/store/monetize).        |

## <a name="windows-81-and-windows-phone-8x-apps"></a>Applications Windows 8.1 et Windows Phone 8. x

Pour les applications Windows 8.1 et Windows Phone 8. x, nous fournissons le [Kit de développement logiciel (SDK) Microsoft Advertising pour Windows et Windows Phone 8. x](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDKforWindowsandWindowsPhone8x). Pour plus d’informations sur l’utilisation de ce kit de développement logiciel (SDK) pour afficher les publicités dans les applications Windows 8.1 et Windows Phone 8. x, consultez [cet article](/previous-versions/windows/apps/dn792120(v=win.10)).

## <a name="related-topics"></a>Rubriques connexes

* [SDK Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK)
* [Rapport sur les performances publicitaires](../publish/advertising-performance-report.md)
* [Programme Windows Premium pour les éditeurs de publicité](windows-premium-ads-publishers-program.md)