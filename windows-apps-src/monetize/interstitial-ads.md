---
ms.assetid: 1f970d38-2338-470e-b5ba-811402752fc4
description: Découvrez comment inclure des publicités interstitielles dans une application UWP pour Windows 10 à l’aide du kit de développement logiciel (SDK) Microsoft Advertising.
title: Spots publicitaires
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, ADS, publicité, contrôle publicitaire, interstitiel
ms.localizationpriority: medium
ms.openlocfilehash: eedb3f12bfc9da51afb2d5205122cbd42d7b31bf
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363092"
---
# <a name="interstitial-ads"></a>Spots publicitaires

>[!WARNING]
> Depuis le 1er juin 2020, la plateforme de monétisation Microsoft AD pour les applications Windows UWP sera arrêtée. [En savoir plus](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Cette procédure pas à pas montre comment inclure des publicités interstitielles dans des applications et des jeux plateforme Windows universelle (UWP) pour Windows 10. Pour obtenir des exemples complets de projet qui montrent comment ajouter des spots publicitaires à des applications JavaScript/HTML et XAML en C# et C++, voir [Exemples de publicité sur GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).

<span id="whatareinterstitialads10"/>

## <a name="what-are-interstitial-ads"></a>Que sont les spots publicitaires ?

Contrairement aux publicités de bannière standard, qui sont confinées à une partie d’une interface utilisateur dans une application ou un jeu, les publicités interstitielles s’affichent sur l’écran entier. Deux formes de base sont fréquemment utilisées dans les jeux.

* Avec les publicités de type *Paywall*, l’utilisateur doit regarder une publicité à intervalles réguliers. Par exemple entre les niveaux de jeu :

    ![whatisaninterstitial](images/13-ed0a333b-0fc8-4ca9-a4c8-11e8b4392831.png)

* Avec les publicités *basées sur les récompenses* , l’utilisateur cherche explicitement un avantage, par exemple une indication ou un temps supplémentaire pour terminer le niveau, et initialise la publicité via l’interface utilisateur de l’application.

Nous fournissons deux types de publicités interstitielles à utiliser dans vos applications et vos jeux : les **publicités vidéo interstitielles** et les **bannières de bannière interstitielles**.

> [!NOTE]
> L’API pour les publicités interstitielles ne gère pas les interfaces utilisateur, sauf au moment de la lecture vidéo. Reportez-vous aux [meilleures pratiques Spots](ui-and-user-experience-guidelines.md#interstitialbestpractices10) pour obtenir des recommandations sur les choses à faire et à ne pas faire, si vous envisagez d’intégrer des spots publicitaires dans votre application.

## <a name="prerequisites"></a>Prérequis

* Installez le [Kit de développement logiciel (SDK) Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) avec visual studio 2015 ou une version ultérieure de Visual Studio. Pour obtenir des instructions d’installation, consultez [cet article](install-the-microsoft-advertising-libraries.md).

## <a name="integrate-an-interstitial-ad-into-your-app"></a>Intégrer une publicité interstitielle à votre application

Pour afficher des spots publicitaires dans votre application, suivez les instructions correspondant au type de projet.

* [XAML/.NET](#interstitialadsxaml10)
* [HTML/JavaScript](#interstitialadshtml10)
* [C++ (technologie interop DirectX)](#interstitialadsdirectx10)

<span id="interstitialadsxaml10"/>

### <a name="xamlnet"></a>XAML/.NET

Cette section fournit des exemples en C#, mais Visual Basic et C++ sont également pris en charge pour les projets XAML/.NET. Pour voir un exemple de code complet en C#, consultez [Exemple de code pour spot publicitaire en C#](interstitial-ad-sample-code-in-c.md).

1. Ouvrez votre projet dans Visual Studio.
    > [!NOTE]
    > Si vous utilisez un projet existant, ouvrez le fichier Package. appxmanifest dans votre projet et assurez-vous que la fonctionnalité **Internet (client)** est sélectionnée. Votre application a besoin de cette fonctionnalité pour recevoir des publicités de test et des publicités en direct.

2. Si votre projet cible **Toute UC**, mettez-le à jour pour utiliser une sortie de génération propre à l’architecture (par exemple, **x86**). Si votre projet cible **Toute UC**, vous ne pourrez pas ajouter une référence à la bibliothèque de publicités Microsoft dans les étapes suivantes. Pour plus d’informations, consultez [Erreurs de référence provoquées par le ciblage de Toute UC dans votre projet](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Ajoutez une référence au kit de développement logiciel (SDK) Microsoft Advertising dans votre projet :

    1. Dans la fenêtre **Explorateur de solutions** , cliquez avec le bouton droit sur **références**, puis sélectionnez **Ajouter une référence...**
    2.  Dans **Gestionnaire de références**, développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour XAML** (version 10.0).
    3.  Dans **Gestionnaire de références**, cliquez sur OK.

3.  Dans le fichier de code approprié de votre application (par exemple, dans MainPage.xaml.cs ou dans un fichier de code d’une autre page), ajoutez la référence d’espace de noms ci-dessous.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs" id="Snippet1":::

4.  À un emplacement approprié de votre application (par exemple, dans ```MainPage``` ou dans une autre page), déclarez un objet [InterstitialAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad) et plusieurs champs de chaîne représentant l’ID de l’application et l’ID d’unité publicitaire de votre spot publicitaire. L’exemple de code suivant affecte les `myAppId` `myAdUnitId` champs et aux [valeurs de test](set-up-ad-units-in-your-app.md#test-ad-units) pour les publicités interstitielles.

    > [!NOTE]
    > Chaque **InterstitialAd** a une *unité publicitaire* correspondante qui est utilisée par nos services pour servir les publicités au contrôle, et chaque unité ad se compose d’un *ID d’unité Active Directory* et d’un *ID d’application*. Dans ces étapes, vous attribuez des valeurs d’ID d’unité ad et d’ID d’application de test à votre contrôle. Ces valeurs de test ne peuvent être utilisées que dans une version test de votre application. Avant de publier votre application dans le Windows Store, vous devez [remplacer ces valeurs de test par des valeurs dynamiques](#release) de l’espace partenaires.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs" id="Snippet2":::

5.  Dans le code exécuté au démarrage (par exemple, dans le constructeur de la page), instanciez l’objet **InterstitialAd** et connectez les gestionnaires d’événements correspondant aux événements de l’objet.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs" id="Snippet3":::

6.  Si vous souhaitez afficher une publicité *vidéo interstitielle* : environ 30-60 secondes avant d’avoir besoin de la publicité, utilisez la méthode [RequestAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) pour prérécupérer la publicité. Cela laisse suffisamment de temps pour demander et préparer la publicité avant qu’elle ne soit affichée. Veillez à spécifier **AdType. Video** pour le type de publicité.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs" id="Snippet4":::

    Si vous souhaitez afficher une *bannière publicitaire interstitielle* : environ 5-8 secondes avant d’avoir besoin de la publicité, utilisez la méthode [RequestAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) pour prérécupérer la publicité. Cela laisse suffisamment de temps pour demander et préparer la publicité avant qu’elle ne soit affichée. Veillez à spécifier **AdType. Display** pour le type AD.

    ```csharp
    myInterstitialAd.RequestAd(AdType.Display, myAppId, myAdUnitId);
    ```

6.  À l’endroit où vous souhaitez afficher la vidéo interstitielle ou la bannière de la bannière interstitielle, vérifiez que le **InterstitialAd** est prêt à être affiché, puis affichez-le à l’aide de la méthode [Show](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show) .

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs" id="Snippet5":::

7.  Définissez les gestionnaires d’événements pour l’objet **InterstitialAd**.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs" id="Snippet6":::

8.  Générez et testez votre application pour vérifier qu’elle affiche les publicités de test.

<span id="interstitialadshtml10"/>

### <a name="htmljavascript"></a>HTML/JavaScript

Les instructions suivantes supposent que vous avez créé un projet Windows universel pour JavaScript dans Visual Studio et que vous ciblez un processeur spécifique. Pour voir un exemple de code complet, consultez [Exemple de code pour spot publicitaire en JavaScript](interstitial-ad-sample-code-in-javascript.md).

1. Ouvrez votre projet dans Visual Studio.

2. Si votre projet cible **Toute UC**, mettez-le à jour pour utiliser une sortie de génération propre à l’architecture (par exemple, **x86**). Si votre projet cible **Toute UC**, vous ne pourrez pas ajouter une référence à la bibliothèque de publicités Microsoft dans les étapes suivantes. Pour plus d’informations, consultez [Erreurs de référence provoquées par le ciblage de Toute UC dans votre projet](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Ajoutez une référence au kit de développement logiciel (SDK) Microsoft Advertising dans votre projet :

    1. Dans la fenêtre **Explorateur de solutions** , cliquez avec le bouton droit sur **références**, puis sélectionnez **Ajouter une référence...**
    2.  Dans **Gestionnaire de références**, développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour JavaScript** (version 10.0).
    3.  Dans **Gestionnaire de références**, cliquez sur OK.

3.  Dans la section ** &lt; Head &gt; ** du fichier HTML du projet, après les références JavaScript du projet default. CSS et default.js, ajoutez la référence à ad.js.

    ``` HTML
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

4.  Dans un fichier .js de votre projet, déclarez un objet [InterstitialAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad) et plusieurs champs contenant l’ID de l’application et l’ID d’unité publicitaire de votre spot publicitaire. L’exemple de code suivant affecte les `applicationId` `adUnitId` champs et aux [valeurs de test](set-up-ad-units-in-your-app.md#test-ad-units) pour les publicités interstitielles.

    > [!NOTE]
    > Chaque **InterstitialAd** a une *unité publicitaire* correspondante qui est utilisée par nos services pour servir les publicités au contrôle, et chaque unité ad se compose d’un *ID d’unité Active Directory* et d’un *ID d’application*. Dans ces étapes, vous attribuez des valeurs d’ID d’unité ad et d’ID d’application de test à votre contrôle. Ces valeurs de test ne peuvent être utilisées que dans une version test de votre application. Avant de publier votre application dans le Windows Store, vous devez [remplacer ces valeurs de test par des valeurs dynamiques](#release) de l’espace partenaires.

    :::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/script.js" id="Snippet1":::

5.  Dans le code exécuté au démarrage (par exemple, dans le constructeur de la page), instanciez l’objet **InterstitialAd** et connectez les gestionnaires d’événements de l’objet.

    :::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/script.js" id="Snippet2":::

5. Si vous souhaitez afficher une publicité *vidéo interstitielle* : environ 30-60 secondes avant d’avoir besoin de la publicité, utilisez la méthode [RequestAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) pour prérécupérer la publicité. Cela laisse suffisamment de temps pour demander et préparer la publicité avant qu’elle ne soit affichée. Veillez à spécifier **InterstitialAdType. Video** pour le type de publicité.

    :::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/script.js" id="Snippet3":::

    Si vous souhaitez afficher une *bannière publicitaire interstitielle* : environ 5-8 secondes avant d’avoir besoin de la publicité, utilisez la méthode [RequestAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) pour prérécupérer la publicité. Cela laisse suffisamment de temps pour demander et préparer la publicité avant qu’elle ne soit affichée. Veillez à spécifier **InterstitialAdType. Display** pour le type AD.

    ```js
    if (interstitialAd) {
        interstitialAd.requestAd(MicrosoftNSJS.Advertising.InterstitialAdType.display, applicationId, adUnitId);
    }
    ```

6.  Dans votre code, là où vous souhaitez afficher la publicité, vérifiez que l’objet **InterstitialAd** est prêt à être affiché, puis affichez-le à l’aide de la méthode [Show](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show).

    :::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/samples.js" id="Snippet4":::

7.  Définissez les gestionnaires d’événements pour l’objet **InterstitialAd**.

    :::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/js/samples.js" id="Snippet5":::

9.  Générez et testez votre application pour vérifier qu’elle affiche les publicités de test.

<span id="interstitialadsdirectx10"/>

### <a name="c-directx-interop"></a>C++ (technologie interop DirectX)

Cet exemple suppose que vous avez créé un projet C++ **DirectX et application XAML (Windows universel)** dans Visual Studio et que vous ciblez une architecture d’UC spécifique.
 
1. Ouvrez votre projet dans Visual Studio.

3. Ajoutez une référence au kit de développement logiciel (SDK) Microsoft Advertising dans votre projet :

    1. Dans la fenêtre **Explorateur de solutions** , cliquez avec le bouton droit sur **références**, puis sélectionnez **Ajouter une référence...**
    2.  Dans **Gestionnaire de références**, développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour XAML** (version 10.0).
    3.  Dans **Gestionnaire de références**, cliquez sur OK.

2.  Dans un fichier d’en-tête approprié de votre application (par exemple, DirectXPage.xaml.h), déclarez un objet [InterstitialAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad) et les méthodes de gestionnaire d’événements associées.  

    :::code language="cpp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h" id="Snippet1":::

3.  Dans le même fichier d’en-tête, déclarez plusieurs champs de chaîne représentant l’ID de l’application et l’ID d’unité publicitaire de votre spot publicitaire. L’exemple de code suivant affecte les `myAppId` `myAdUnitId` champs et aux [valeurs de test](set-up-ad-units-in-your-app.md#test-ad-units) pour les publicités interstitielles.

    > [!NOTE]
    > Chaque **InterstitialAd** a une *unité publicitaire* correspondante qui est utilisée par nos services pour servir les publicités au contrôle, et chaque unité ad se compose d’un *ID d’unité Active Directory* et d’un *ID d’application*. Dans ces étapes, vous attribuez des valeurs d’ID d’unité ad et d’ID d’application de test à votre contrôle. Ces valeurs de test ne peuvent être utilisées que dans une version test de votre application. Avant de publier votre application dans le Windows Store, vous devez [remplacer ces valeurs de test par des valeurs dynamiques](#release) de l’espace partenaires.

    :::code language="cpp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h" id="Snippet2":::

4.  Dans le fichier .cpp dans lequel vous voulez ajouter du code pour afficher un spot publicitaire, ajoutez la référence d’espace de noms ci-dessous. Les exemples suivants partent du principe que vous ajoutez le code dans le fichier DirectXPage.xaml.cpp de votre application.

    :::code language="cpp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp" id="Snippet3":::

6.  Dans le code exécuté au démarrage (par exemple, dans le constructeur de la page), instanciez l’objet **InterstitialAd** et connectez les gestionnaires d’événements correspondant aux événements de l’objet. Dans l’exemple suivant, ```InterstitialAdSamplesCpp``` est l’espace de noms de votre projet ; modifiez ce nom au besoin pour votre code.

    :::code language="cpp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp" id="Snippet4":::

7. Si vous souhaitez afficher une publicité *vidéo interstitielle* : environ 30-60 secondes avant d’avoir besoin de la publicité interstitielle, utilisez la méthode [RequestAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) pour prérécupérer la publicité. Cela laisse suffisamment de temps pour demander et préparer la publicité avant qu’elle ne soit affichée. Veillez à spécifier **AdType :: Video** pour le type de publicité.

    :::code language="cpp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp" id="Snippet5":::

    Si vous souhaitez afficher une *bannière publicitaire interstitielle* : environ 5-8 secondes avant d’avoir besoin de la publicité, utilisez la méthode [RequestAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) pour prérécupérer la publicité. Cela laisse suffisamment de temps pour demander et préparer la publicité avant qu’elle ne soit affichée. Veillez à spécifier **AdType ::DSE** pour le type de publicité.

    ```cpp
    m_interstitialAd->RequestAd(AdType::Display, myAppId, myAdUnitId);
    ```

7.  Dans votre code, là où vous souhaitez afficher la publicité, vérifiez que l’objet **InterstitialAd** est prêt à être affiché, puis affichez-le à l’aide de la méthode [Show](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show).

    :::code language="cpp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp" id="Snippet6":::

8.  Définissez les gestionnaires d’événements pour l’objet **InterstitialAd**.

    :::code language="cpp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp" id="Snippet7":::

9. Générez et testez votre application pour vérifier qu’elle affiche les publicités de test.

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>Publier votre application avec des publicités en direct

1. Assurez-vous que votre utilisation des publicités interstitielles dans votre application suit nos [recommandations pour les publicités interstitielles](ui-and-user-experience-guidelines.md#interstitialbestpractices10).

2.  Dans l’espace partenaires, accédez à la page [annonces dans l’application](../publish/in-app-ads.md) et [créez une unité ad](set-up-ad-units-in-your-app.md#live-ad-units). Pour le type d’unité ad, choisissez la **vidéo** ou la **bannière**, en fonction du type de publicité interstitielle affichée. Prenez note de l’ID d’unité publicitaire et de l’ID de l’application.
    > [!NOTE]
    > Les valeurs d’ID d’application pour les unités AD de test et les unités ad UWP activent des formats différents. Les valeurs d’ID d’application de test sont des GUID. Lorsque vous créez une unité ad UWP en temps réel dans l’espace partenaires, la valeur de l’ID d’application pour l’unité ad correspond toujours à l’ID de magasin de votre application (un exemple de valeur d’ID de magasin ressemble à 9NBLGGH4R315).

3. Vous pouvez éventuellement activer la médiation AD pour les **InterstitialAd** en configurant les paramètres de la section [paramètres de médiation](../publish/in-app-ads.md#mediation) sur la page [ADS dans l’application](../publish/in-app-ads.md) . La médiation ad vous permet d’optimiser votre chiffre d’affaires et vos fonctionnalités de promotion d’applications en affichant des publicités de plusieurs réseaux Active Directory, notamment des publicités provenant d’autres réseaux Active Directory payants, tels que Taboola et Smaato et ADS pour les campagnes de promotion des applications Microsoft.

4.  Dans votre code, remplacez les valeurs de l’unité ad du test par les valeurs dynamiques que vous avez générées dans l’espace partenaires.

5.  [Soumettez votre application](../publish/app-submissions.md) au magasin à l’aide de l’espace partenaires.

6.  Passez en revue les [rapports de performances publicitaires](../publish/advertising-performance-report.md) dans l’espace partenaires.

<span id="manage" />

## <a name="manage-ad-units-for-multiple-interstitial-ad-controls-in-your-app"></a>Gérer les unités AD pour plusieurs contrôles interstitiels Active Directory dans votre application

Vous pouvez utiliser plusieurs contrôles **InterstitialAd** dans une même application. Dans ce scénario, nous vous recommandons d’affecter une unité ad différente à chaque contrôle. L’utilisation de différentes unités AD pour chaque contrôle vous permet de [configurer séparément les paramètres de médiation](../publish/in-app-ads.md#mediation) et d’obtenir des données de [rapport](../publish/advertising-performance-report.md) discrètes pour chaque contrôle. Cela permet également à nos services de mieux optimiser les publicités que nous avons utilisées dans votre application.

> [!IMPORTANT]
> Vous pouvez utiliser chaque unité ad dans une seule application. Si vous utilisez une unité ad dans plusieurs applications, les publicités ne sont pas prises en charge pour cette unité Active Directory.

## <a name="related-topics"></a>Rubriques connexes

* [Recommandations pour les spots publicitaires](ui-and-user-experience-guidelines.md#interstitialbestpractices10)
* [Exemple de code pour spot publicitaire en C#](interstitial-ad-sample-code-in-c.md)
* [Exemple de code pour spot publicitaire en JavaScript](interstitial-ad-sample-code-in-javascript.md)
* [Exemples de publicité sur GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
* [Configurer des unités AD pour votre application](set-up-ad-units-in-your-app.md)
