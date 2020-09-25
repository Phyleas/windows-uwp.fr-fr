---
Description: Si votre application affiche des publicités à l’aide du kit de développement logiciel (SDK) Microsoft Advertising, utilisez la page ADS dans l’application de l’espace partenaires pour gérer votre utilisation des publicités.
title: Publicités dans l’application
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c8d01042fec7435652c819f29e3a791a5623a947
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220262"
---
# <a name="in-app-ads"></a>Publicités dans l’application

>[!WARNING]
> Depuis le 1er juin 2020, la plateforme de monétisation Microsoft AD pour les applications Windows UWP sera arrêtée. [En savoir plus](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Utilisez la page **monétis** &gt; **in-app ADS** dans l' [espace partenaires](https://partner.microsoft.com/dashboard) pour créer et gérer des unités AD pour :

* Applications plateforme Windows universelle (UWP) qui utilisent le [Kit de développement logiciel (SDK) Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK).
* A publié précédemment des applications Windows 8. x et Windows Phone 8. x qui utilisent le [Kit de développement logiciel (SDK) Microsoft Advertising pour Windows et Windows Phone 8. x](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDKforWindowsandWindowsPhone8x).

> [!IMPORTANT]
> Vous ne pouvez plus charger de nouveaux packages XAP générés à l’aide du ou des kits de développement logiciel (SDK) Windows Phone 8. x. Les applications qui sont déjà dans Store avec des packages XAP continuent de fonctionner sur les appareils Windows 10 mobile. Pour plus d’informations, consultez ce billet de [blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

Pour plus d’informations sur l’intégration de ces kits de développement logiciel (SDK) à vos applications pour afficher des publicités, consultez [afficher des publicités dans votre application avec le kit de développement logiciel (SDK) Microsoft Advertising](../monetize/display-ads-in-your-app.md).

<span id="create-ad-unit" />

## <a name="create-ad-units"></a>Créer des unités ad

Pour créer une unité ad pour une [bannière](../monetize/banner-ads.md)publicitaire, une publicité [interstitielle](../monetize/interstitial-ads.md)ou une [publicité Native](../monetize/native-ads.md) dans votre application :

1.  Accédez à la page **monétis** &gt; **in-app ADS** dans l’espace partenaires, puis cliquez sur **Create ad Unit**.
2.  Dans la liste déroulante nom de l' **application** , sélectionnez l’application dans laquelle votre unité ad sera utilisée.
3.  Dans le champ nom de l' **unité ad** , entrez un nom pour l’unité ad. Il peut s’agir de n’importe quelle chaîne descriptive que vous souhaitez utiliser pour identifier l’unité ad à des fins de création de rapports.
4.  Dans la liste déroulante **type d’unité ad** , sélectionnez le type de publicité.

    * Si vous utilisez une bannière publicitaire dans votre application, sélectionnez **bannière**.
    * Si vous affichez une publicité de vidéo interstitielle ou une publicité de bannière interstitielle dans votre application, sélectionnez une **vidéo** ou une **bannière** (veillez à sélectionner l’option appropriée pour le type de publicité interstitielle que vous souhaitez afficher).
    * Si vous montrez une publicité native dans votre application, sélectionnez **natif**.

5. Dans la liste déroulante **famille d’appareils** , sélectionnez la famille d’appareils ciblée par l’application dans laquelle votre unité ad sera utilisée. Les options disponibles sont les suivantes : **UWP (Windows 10)**, **PC/tablette (Windows 8.1)** ou **mobile (Windows Phone 8. x)**.

6. Configurez les paramètres supplémentaires suivants comme vous le souhaitez :

    * Si vous sélectionnez la famille d’appareils **UWP (Windows 10)** pour l’unité ad, vous pouvez éventuellement configurer des [paramètres de médiation](#mediation) pour l’unité ad.
    * Si vous sélectionnez la famille d’appareils **PC/tablette (Windows 8.1)** ou **mobile (Windows Phone 8. x)** pour une unité publicitaire à bannière, vous pouvez éventuellement sélectionner **afficher les publicités de la Communauté dans votre application** pour vous abonner aux publicités de la [communauté](about-community-ads.md).

7.  Si vous n’avez pas encore défini la compatibilité COPPA pour l’application sélectionnée, choisissez une option dans la section conformité de la réglementation [Coppa](#coppa) .
8.  Cliquez sur **Créer une publicité**.

Une fois que vous avez créé la nouvelle unité ad, elle apparaît dans la table des unités ad disponibles dans la page **monétis** &gt; **in-app ADS** .

<span id="available-ad-units" />

## <a name="review-and-edit-ad-units"></a>Examiner et modifier les unités ad

Une fois que vous avez créé des unités AD pour une ou plusieurs applications de votre compte, celles-ci apparaissent dans un tableau en bas **de la** page de publicité &gt; **dans les annonces dans l’application** . Cette table affiche l' **ID d’application** et l' **ID d’unité ad** pour chaque unité ad, ainsi que d’autres informations. Pour afficher les publicités dans votre application, vous devez utiliser ces valeurs dans votre code. Pour plus d’informations, voir [Configurer des unités publicitaires dans votre application](../monetize/set-up-ad-units-in-your-app.md).

* Si votre application affiche des [bannières publicitaires](../monetize/banner-ads.md), attribuez ces valeurs aux propriétés [ApplicationID](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) et [AdUnitId](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.adunitid) de votre objet [classe AdControl](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) .
* Si votre application affiche des [publicités interstitielles](../monetize/interstitial-ads.md), transmettez ces valeurs à la méthode [RequestAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) de votre objet [InterstitialAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad) .
* Si votre application affiche des [publicités natives](../monetize/native-ads.md), transmettez ces valeurs au constructeur **NativeAdsManagerV2** .
  > [!IMPORTANT]
  > Vous pouvez utiliser chaque unité ad dans une seule application. Si vous utilisez une unité ad dans plusieurs applications, les publicités ne sont pas prises en charge pour cette unité Active Directory.

  > [!NOTE]
  > Vous pouvez utiliser plusieurs commandes de bannière, interstitielles et natives Active Directory dans une même application. Dans ce scénario, nous vous recommandons d’affecter une unité ad différente à chaque contrôle. L’utilisation de différentes unités AD pour chaque contrôle vous permet de [configurer séparément les paramètres de médiation](../publish/in-app-ads.md#mediation) et d’obtenir des données de [rapport](../publish/advertising-performance-report.md) discrètes pour chaque contrôle. Cela permet également à nos services de mieux optimiser les publicités que nous avons utilisées dans votre application.

Pour modifier les [paramètres de médiation](#mediation) pour une unité ad UWP ou la conformité à la réglementation [Coppa](#coppa) pour l’application dans laquelle l’unité ad est utilisée, cliquez sur le nom de l’unité ad.

> [!NOTE]
> Si une unité ad n’a pas d’activité au cours des six derniers mois, nous l’étiqueterons comme **inactive**et, par la suite, elle est supprimée de l’espace partenaires. Vous pouvez utiliser des filtres pour afficher uniquement les unités **Active Directory actives** ou **inactives** . Si vous voyez des unités ad que vous pensez être marquées comme **inactives**, [Contactez le support technique](https://developer.microsoft.com/windows/support).

<span id="mediation" />

## <a name="mediation-settings"></a>Paramètres de médiation

Lorsque vous [créez une unité ad UWP](#create-ad-unit) ou [modifiez une unité ad UWP existante](#available-ad-units), utilisez les options de cette section pour configurer la [médiation ad](../monetize/ad-mediation-service.md) pour l’unité ad. La médiation ad vous permet d’optimiser votre chiffre d’affaires et vos fonctionnalités de promotion d’applications en affichant des publicités de plusieurs réseaux Active Directory, notamment des publicités provenant d’autres réseaux Active Directory payants et des publicités non génératrices de revenus pour les campagnes de promotion des applications Microsoft. Nous prenons en charge les demandes d’enchaînement de bannières à partir des réseaux Active Directory que vous choisissez. Si vous avez une unité ad UWP qui est déjà associée à une bannière, interstitielle ou une publicité native dans votre application, l’activation de la médiation ad ne nécessite aucune modification de code dans votre application.

> [!NOTE]
> Lorsque vous activez la médiation AD pour une unité ad UWP, vous n’avez pas besoin d’obtenir une unité publicitaire auprès de réseaux ad tiers. Notre service AD médiation crée automatiquement toutes les unités ad tierces nécessaires.

Pour configurer les paramètres de médiation AD pour une unité ad UWP dans votre application :

1. [Créez une unité ad](#create-ad-unit) ou [Sélectionnez une unité ad existante](#available-ad-units).
2. Dans la page **annonces dans l’application** , accédez à la section **paramètres de médiation** et configurez vos paramètres.

    * Par défaut, la case à cocher **laisser Microsoft optimiser mes paramètres** est activée. Nous vous recommandons d’utiliser cette option. Cette option utilise des algorithmes d’apprentissage automatique pour choisir automatiquement les paramètres de médiation AD pour votre application afin de vous aider à maximiser vos revenus Active Directory sur les marchés pris en charge par votre application. Lorsque vous utilisez cette option, vous pouvez également choisir les réseaux Active Directory que vous souhaitez utiliser dans la configuration. Décochez les réseaux Active Directory que vous ne souhaitez pas faire partie de la configuration et notre algorithme garantira que votre application reçoit uniquement les annonces des réseaux Active Directory sélectionnés.
    * Si vous souhaitez choisir vos propres paramètres de médiation ad, choisissez **modifier les paramètres par défaut**.

    > [!NOTE]
    > Les étapes restantes de cette section s’appliquent uniquement si vous choisissez **modifier les paramètres par défaut**.

3. Dans la liste déroulante **cible** , choisissez **ligne de base** pour configurer la configuration par défaut de vos paramètres de médiation ad. Cette configuration par défaut sera appliquée à tous les marchés, à l’exception des marchés où vous définissez des configurations spécifiques au marché.
4. Ensuite, spécifiez le taux de publicités que vous souhaitez afficher dans votre contrôle à partir de réseaux payants (qui paient vos revenus pour les impressions) et d’autres réseaux Active Directory (qui ne paient pas de revenu pour les impressions). Pour ce faire, entrez une valeur comprise entre 0 et 100 dans les champs **poids** des **réseaux Active Directory payants** et d' **autres réseaux Active Directory**.  
5. Dans la section **réseaux ad payants** , activez la case à cocher dans la colonne **actif** pour chaque [réseau payant](#paid-networks) que vous souhaitez utiliser, puis utilisez les flèches de la colonne **classement** pour classer les réseaux par rang (cela indique la fréquence à laquelle chaque réseau doit être utilisé par votre contrôle).
6. Si vous avez sélectionné une unité publicitaire **bannière** ou **bannière** , vous verrez également une section intitulée **autres réseaux Active Directory**. Les réseaux de cette section ne vous sont pas facturés pour l’impression des publicités. Au lieu de cela, ces réseaux affichent des publicités provenant de sources telles que des campagnes de promotion d’applications.

    Dans la section **autres réseaux Active Directory** , activez la case à cocher dans la colonne **actif** pour chaque [autre réseau](#other-networks) que vous souhaitez utiliser, puis utilisez les flèches de la colonne **classement** pour classer les réseaux par rang (spécifie la fréquence à laquelle chaque réseau doit être utilisé par votre contrôle). Les autres réseaux suivants sont actuellement pris en charge :

7. Pour chaque marché sur lequel vous souhaitez remplacer la configuration de médiation par défaut, sélectionnez le marché dans la liste déroulante **cible** et mettez à jour les sélections et le classement du réseau Active Directory.
8. Cliquez sur créer une unité **ad** (si vous créez une nouvelle unité ad) ou sur **Enregistrer** (si vous modifiez une unité ad existante).

<span id="paid-networks" />

### <a name="supported-paid-ad-networks"></a>Réseaux ad payants pris en charge

Le tableau suivant répertorie les réseaux payants actuellement pris en charge pour chaque type de publicité. Notez que certains de ces réseaux ne sont [pas disponibles sur tous les marchés](#network-markets).

|  Réseau ad  |  Description  |  Types ad pris en charge  |
|--------------|---------------|---------------------|
| Serment et AppNexus |  Il s’agit d’un réseau ad géré par Microsoft qui fournit des publicités via nos réseaux partenaires, serment et AppNexus.<p/>**Remarque**: serment et AppNexus sont toujours classés en premier dans la liste des **réseaux ad payants** pour les unités publicitaires de bannières, et il n’est pas possible de les remplacer par un classement plus bas pour ces types de publicités. | Bannière, vidéo |
| AppNexus (direct) | Sélectionnez cette option pour fournir des publicités à partir de [AppNexus](https://www.appnexus.com). | Vidéo, en mode natif  |
| Annonces d’installation d’applications Microsoft | Sélectionnez cette option pour traiter les publicités d’installation d’application ou les publicités de réengagement d’applications créées par d’autres développeurs dans l’écosystème Windows qui [créent des campagnes publicitaires promotionnelles pour leurs applications](create-an-ad-campaign-for-your-app.md).  |  Bannière, interstitiel de bannière, Native  |
| Recommandations relatives au contenu MSN |  Sélectionnez cette option pour traiter les publicités des recommandations du contenu MSN. |  Bannière, bannière  |
| Cerveau |  Sélectionnez cette option pour traiter les publicités contre le [cerveau](https://www.outbrain.com/). |  Bannière, bannière  |
| Revcontent |  Sélectionnez cette option pour fournir des publicités à partir de [Revcontent](https://www.revcontent.com/). |  Bannière, Native  |
| Smaato |  Sélectionnez cette option pour fournir des publicités à partir de [Smaato](https://www.smaato.com/). |  Bannière  |
| smartclip |  Sélectionnez cette option pour fournir des publicités à partir de [smartclip](http://www.smartclip.com/). |  Vidéo  |
| SpotX |  Sélectionnez cette option pour fournir des publicités à partir de [SpotX](https://www.spotx.tv/). |  Vidéo  |
| Taboola |  Sélectionnez cette option pour fournir des publicités à partir de [Taboola](https://www.taboola.com/). |  Bannière  |
| Vungle | Sélectionnez cette option pour fournir des publicités à partir de [Vungle](https://vungle.com/) | Vidéo |
| Undertone | Sélectionnez cette option pour fournir des publicités à partir de [undertone](https://www.undertone.com/). | Bannière |


<span id="other-networks" />

### <a name="other-ad-networks"></a>Autres réseaux publicitaires

Le tableau suivant répertorie les autres réseaux actuellement pris en charge pour chaque type de publicité.

|  Réseau ad  |  Description  |  Types ad pris en charge  |
|--------------|---------------|---------------------|
| Annonces de la communauté Microsoft |  Si vous [créez une campagne publicitaire promotionnelle pour l’une de vos applications](create-an-ad-campaign-for-your-app.md) et configurez cette campagne en tant que [campagne ad](about-community-ads.md)de la Communauté, sélectionnez cette option pour afficher les publicités de cette campagne. | Bannière, bannière |
| Publicités maison Microsoft | Si vous [créez une campagne publicitaire promotionnelle pour l’une de vos applications](create-an-ad-campaign-for-your-app.md) et configurez cette campagne comme une [campagne publicitaire maison](about-house-ads.md), sélectionnez cette option pour afficher les publicités de cette campagne. | Bannière, bannière  |


<span id="network-markets" />

### <a name="supported-markets-for-ad-networks"></a>Marchés pris en charge pour les réseaux Active Directory

Les réseaux Active Directory disponibles offrent des publicités sur tous les [marchés pris en charge](define-market-selection.md#microsoft-store-consumer-markets), avec les exceptions suivantes.

|  Réseau ad  |  Marchés pris en charge  |
|--------------|---------------------|
| Revcontent | Brésil, Canada, France, Allemagne, Italie, Japon, Espagne, Royaume-Uni, États-Unis  |
| Smaato | Brésil, Canada, France, Allemagne, Italie, Japon, Espagne, Royaume-Uni, États-Unis |
| smartclip | Autriche, Belgique, Danemark, Finlande, Allemagne, Italie, Pays-Bas, Norvège, Suède, Suisse  |
| Undertone | États-Unis |

<span id="coppa" />

## <a name="coppa-compliance"></a>Conformité avec la réglementation COPPA

Lorsque vous [créez une unité ad](#create-ad-unit) ou [Sélectionnez une unité ad existante](#available-ad-units), la section **conformité** de la Coppa s’affiche au bas de la page si l’application sélectionnée pour l’unité Active Directory a au moins une soumission qui a atteint le au cours de l’étape du [Store](../publish/the-app-certification-process.md#in-the-store) dans le processus de certification de l’application.

Dans le cadre de la Loi sur la protection de la confidentialité en ligne (« COPPA ») de l’enfant, vous devez sélectionner **cette application est dirigée vers les enfants âgés de** plus de 13 ans dans cette section si votre application est dirigée vers les enfants âgés de plus de 13 ans. Si vous sélectionnez cette option, Microsoft prend les mesures nécessaires pour désactiver ses services de publicité comportementale lors de la diffusion de publicités dans votre application.

Le paramètre de **conformité Coppa** que vous choisissez est appliqué automatiquement à toutes les unités AD de l’application sélectionnée.

> [!IMPORTANT]
> Si votre application s’adresse aux enfants de moins de 13 ans, la réglementation COPPA vous impose certaines obligations. Pour plus d’informations sur vos obligations, consultez [cette page](https://www.ftc.gov/enforcement/rules/rulemaking-regulatory-reform-proceedings/childrens-online-privacy-protection-rule).
