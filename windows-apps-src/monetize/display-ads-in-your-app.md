---
ms.assetid: 63A9EDCF-A418-476C-8677-D8770B45D1D7
description: Le SDK Microsoft Advertising vous offre plusieurs moyens de monétiser votre application grâce aux publicités.
title: Afficher des publicités dans votre application avec le SDK Microsoft Advertising
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, publicités, publicité, bannière, contrôle de publicité, spot
ms.localizationpriority: medium
ms.openlocfilehash: d693b8a4e07e66eded7d1580291c552e7ab9ca64
ms.sourcegitcommit: 71f9013c41fc1038a9d6c770cea4c5e481c23fbc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77507223"
---
# <a name="display-ads-in-your-app-with-the-microsoft-advertising-sdk"></a>Afficher des publicités dans votre application avec le SDK Microsoft Advertising

>[!WARNING]
> Depuis le 1er juin 2020, la plateforme de monétisation Microsoft AD pour les applications Windows UWP sera arrêtée. [En savoir plus](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Augmentez vos opportunités de revenus en plaçant des publicités dans votre application pour plateforme Windows universelle (UWP) sous Windows 10 à l’aide du SDK Microsoft Advertising. Notre plateforme de monétisation ad offre un large éventail de formats ad qui peuvent être intégrés en toute transparence à vos applications et qui prend en charge la médiation avec de nombreux réseaux Active Directory populaires. Notre plateforme est conforme aux normes OpenRTB, VAST 2.x, MRAID 2, et VPAID 3 et est compatible avec MOAT et IAS. 

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
    <a href="https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK">Installez le kit de développement logiciel (SDK) Microsoft Advertising</a>
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
    <a href="set-up-ad-units-in-your-app.md">Configurer des unités Active Directory dans votre application</a>
    <br/>
    <a href="best-practices-for-ads-in-apps.md">Meilleures pratiques</a>
    <br/>Informations de référence sur l' 
    <a href="https://docs.microsoft.com/uwp/api/overview/advertising">API</a>
    </td>
</tr>
</tbody>
</table>

## <a name="step-1-install-the-microsoft-advertising-sdk"></a>Étape 1 : Installer le SDK Microsoft Advertising

Pour commencer, installez le [kit SDK Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) sur l’ordinateur de développement que vous utilisez pour créer votre application. Pour des instructions d’installation, voir [cet article](install-the-microsoft-advertising-libraries.md).

## <a name="step-2-implement-ads-in-your-app"></a>Étape 2 : Intégrer des publicités dans votre application

Le SDK Microsoft Advertising fournit plusieurs types de contrôles de publicités différents que vous pouvez utiliser dans votre application. Choisissez les types de publicités qui conviennent le mieux à votre scénario, puis ajoutez du code à votre application pour afficher ces publicités. Pendant cette étape, vous allez utiliser une unité publicitaire de test pour voir comment votre application restitue les publicités au cours du test.

### <a name="banner-ads"></a>Bannières publicitaires

Il s'agit de publicités à affichage statique qui utilisent une partie rectangulaire d’une page de votre application pour afficher du contenu publicitaire. Ces publicités peuvent s'actualiser automatiquement à intervalles réguliers. Il s’agit d’un bon point de départ si vous débutez avec les publicités dans votre application.

Pour obtenir des instructions et des exemples de code, voir [cet article](adcontrol-in-xaml-and--net.md).

![addreferences](images/banner-ad.png)

### <a name="interstitial-video-and-interstitial-banner-ads"></a>Spots vidéo et spots de bannières publicitaires

Ce sont des publicités plein écran qui obligent en général l’utilisateur à visionner une vidéo ou à cliquer dessus pour poursuivre sa progression dans l'application ou dans le jeu. Nous prenons en charge deux types de spots publicitaires : bannières et vidéo.

Pour obtenir des instructions et des exemples de code, voir [cet article](interstitial-ads.md).

![addreferences](images/interstitial-ad.png)

### <a name="native-ads"></a>Publicités natives

Il s’agit de publicités basées sur un composant. Chaque élément de l'annonce publicitaire (par exemple, le titre, l’image, la description et le texte de l’appel à l’action) est fourni à votre application sous forme d'élément individuel que vous pouvez intégrer dans votre application en utilisant vos propres polices, couleurs et autres composants d’interface utilisateur.

Pour obtenir des instructions et des exemples de code, voir [cet article](native-ads.md).

![addreferences](images/native-ad.png)

<span id="ad-mediation"/>

## <a name="step-3-create-an-ad-unit-and-configure-mediation"></a>Étape 3 : Créer une unité publicitaire et configurer une médiation

Une fois que vous avez terminé le test de votre application et que vous êtes prêt à la soumettre au Store, créez une unité publicitaire sur la page [ADS dans l’application](../publish/in-app-ads.md) dans l’espace partenaires. Ensuite, mettez à jour le code de votre application pour utiliser cette unité publicitaire afin que votre application reçoit des publicités dynamiques. Pour plus d’informations, voir [Configurer des unités publicitaires dans votre application](set-up-ad-units-in-your-app.md#live-ad-units).

Par défaut, votre application affiche des publicités issues du réseau Microsoft pour les publicités payées. Pour optimiser vos revenus publicitaires, vous pouvez activer la [médiation publicitaire](ad-mediation-service.md) pour votre unité publicitaire afin d'afficher des publicités à partir de réseaux de publicités payées supplémentaires, tels que Taboola et Smaato. Vous pouvez également augmenter vos capacités de promotion d'application en affichant des publicités provenant des campagnes de promotion d'applications Microsoft.

Pour commencer à utiliser la médiation publicitaire dans votre application UWP, [configurez les paramètres de médiation publicitaire](../publish/in-app-ads.md#mediation-settings) de votre unité publicitaire. Par défaut, nous configurons automatiquement les paramètres de médiation à l’aide d’algorithmes d’apprentissage automatique pour vous aider à optimiser vos revenus publicitaires sur les marchés pris en charge par votre application. Cependant, vous avez également la possibilité de sélectionner manuellement les réseaux que vous souhaitez utiliser. Dans tous les cas, les paramètres de médiation sont entièrement configurés sur nos serveurs, par conséquent vous n'avez pas besoin de modifier le code dans votre app.    

## <a name="step-4-submit-your-app-and-review-performance"></a>Étape 4 : Soumettre votre app et évaluer ses performances

Une fois que vous avez terminé le développement de votre application avec ADS, vous pouvez [Envoyer votre application mise à jour](https://docs.microsoft.com/windows/uwp/publish/app-submissions) dans l’espace partenaires pour la rendre disponible dans le Store. Les apps qui affichent des publicités doivent respecter les exigences supplémentaires qui sont spécifiées dans la [section 10.10 des politiques du Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) et dans l'[Annexe E du Contrat du développeur d'application](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement).

Une fois votre application publiée et disponible dans le Windows Store, vous pouvez consulter vos [rapports de performances publicitaires](../publish/advertising-performance-report.md) dans l’espace partenaires et continuer à apporter des modifications à vos paramètres de médiation pour optimiser les performances de vos publicités. Vos revenus publicitaires sont inclus dans votre [résumé du paiement](../publish/payout-summary.md).

<span id="additional-help" />

## <a name="additional-help"></a>Aide supplémentaire

Pour obtenir une aide supplémentaire sur l'utilisation du SDK Microsoft Advertising, utilisez les ressources suivantes.

|  Tâche    | Resource |               
|----------|-------|
| Signaler un bogue ou obtenir un support assisté en matière de publicité     | Visitez la [page de support](https://developer.microsoft.com/windows/support) et choisissez **Publicités dans l’app**.        |
| Bénéficier du support de la communauté     | Consultez le [forum](https://social.msdn.microsoft.com/forums/windowsapps/en-US/home?category=windowsapps).       |
| Télécharger des exemples de projet qui montrent comment ajouter des bannières et des spots publicitaires aux applications.     | Voir [Exemples de publicité sur GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).       |
| Découvrir les dernières opportunités de monétisation pour les applications Windows     | Visitez [Monétiser vos applications](https://developer.microsoft.com/store/monetize).        |

## <a name="windows-81-and-windows-phone-8x-apps"></a>Applications Windows 8.1 et Windows Phone 8.x

Pour les applications Windows 8.1 et Windows Phone 8.x, nous fournissons le [SDK Microsoft Advertising pour Windows et Windows Phone 8.x](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDKforWindowsandWindowsPhone8x). Pour plus d’informations concernant l’utilisation de ce SDK pour afficher des publicités dans des applications Windows 8.1 et Windows Phone 8.x, consultez [cet article](https://docs.microsoft.com/previous-versions/windows/apps/dn792120(v=win.10)).

## <a name="related-topics"></a>Rubriques connexes

* [SDK Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK)
* [Rapport sur les performances publicitaires](../publish/advertising-performance-report.md)
* [Programme des éditeurs Windows Premium ads](windows-premium-ads-publishers-program.md)
