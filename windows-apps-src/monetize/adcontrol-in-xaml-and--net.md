---
ms.assetid: 4e7c2388-b94e-4828-a104-14fa33f6eb2d
description: Découvrez comment utiliser la classe classe AdControl pour afficher des bannières publicitaires dans une application XAML pour Windows 10 (UWP).
title: AdControl en XAML et .NET
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, ADS, publicité, classe AdControl, contrôle AD, XAML, .net, procédure pas à pas
ms.localizationpriority: medium
ms.openlocfilehash: 8d1d1e75754bc9f3bed0be862a38ede43d55ad52
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155663"
---
# <a name="adcontrol-in-xaml-and-net"></a>AdControl en XAML et .NET

>[!WARNING]
> Depuis le 1er juin 2020, la plateforme de monétisation Microsoft AD pour les applications Windows UWP sera arrêtée. [En savoir plus](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Cette procédure pas à pas montre comment utiliser la classe [classe AdControl](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) pour afficher des bannières publicitaires dans une application XAML plateforme Windows universelle (UWP) pour Windows 10 qui est implémentée à l’aide de C#.

> [!NOTE]
> Le kit de développement logiciel (SDK) Microsoft Advertising prend également en charge les applications XAML implémentées à l’aide de C++. Pour obtenir un exemple de projet complet, consultez les [exemples de publicité sur GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).

## <a name="prerequisites"></a>Prérequis

* Installez le [Kit de développement logiciel (SDK) Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) avec visual studio 2015 ou une version ultérieure de Visual Studio. Pour obtenir des instructions d’installation, consultez [cet article](install-the-microsoft-advertising-libraries.md).

## <a name="integrate-a-banner-ad-into-your-app"></a>Intégrer une bannière publicitaire dans votre application

1. Dans Visual Studio, ouvrez votre projet ou créez-en un.

    > [!NOTE]
    > Si vous utilisez un projet existant, ouvrez le fichier Package. appxmanifest dans votre projet et assurez-vous que la fonctionnalité **Internet (client)** est sélectionnée. Votre application a besoin de cette fonctionnalité pour recevoir des publicités de test et des publicités en direct.

2. Si votre projet cible **Toute UC**, mettez-le à jour pour utiliser une sortie de génération propre à l’architecture (par exemple, **x86**). Si votre projet cible **Toute UC**, vous ne pourrez pas ajouter une référence à la bibliothèque de publicités Microsoft dans les étapes suivantes. Pour plus d’informations, consultez [Erreurs de référence provoquées par le ciblage de Toute UC dans votre projet](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Ajoutez une référence au kit de développement logiciel (SDK) Microsoft Advertising dans votre projet :

    1. Dans la fenêtre **Explorateur de solutions** , cliquez avec le bouton droit sur **références**, puis sélectionnez **Ajouter une référence...**
    2.  Dans **Gestionnaire de références**, développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour XAML** (version 10.0).
    3.  Dans **Gestionnaire de références**, cliquez sur OK.

4.  Modifiez le code XAML de la page où vous incorporez des publicités pour inclure l’espace de noms **Microsoft.Advertising.WinRT.UI**. Par exemple, dans l’exemple d’application par défaut généré par Visual Studio (nommé, dans cette application, MyAdFundedWindows10AppXAML), la page XAML est **MainPage.XAML**.

    La section **Page** du fichier MainPage.xaml généré par Visual Studio contient le code suivant.

    ``` xml
    <Page
      x:Class="MyAdFundedWindows10AppXAML.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:MyAdFundedWindows10AppXAML"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      mc:Ignorable="d">
      <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      </Grid>
    </Page>
    ```

    Ajoutez la référence d’espace de noms **Microsoft.Advertising.WinRT.UI** pour que la section **Page** du fichier MainPage.xaml contienne le code suivant.

    ``` xml
    <Page
      x:Class="MyAdFundedWindows10AppXAML.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:MyAdFundedWindows10AppXAML"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      xmlns:UI="using:Microsoft.Advertising.WinRT.UI"
      mc:Ignorable="d">
      <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      </Grid>
    </Page>
    ```

5. Dans la balise **Grid**, ajoutez le code correspondant à la classe **AdControl**. Assignez les propriétés  [AdUnitId](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.adunitid) et [ApplicationID](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) aux valeurs de l' [unité ad du test](set-up-ad-units-in-your-app.md#test-ad-units). Ajustez également la **hauteur** et la **largeur** du contrôle afin qu’il corresponde à l’une des [tailles de publicité prises en charge pour les bannières publicitaires](supported-ad-sizes-for-banner-ads.md).

    > [!NOTE]
    > Chaque **classe AdControl** a une *unité publicitaire* correspondante qui est utilisée par nos services pour servir les publicités au contrôle, et chaque unité ad se compose d’un *ID d’unité Active Directory* et d’un *ID d’application*. Dans ces étapes, vous attribuez des valeurs d’ID d’unité ad et d’ID d’application de test à votre contrôle. Ces valeurs de test ne peuvent être utilisées que dans une version test de votre application. Avant de publier votre application dans le Windows Store, vous devez [remplacer ces valeurs de test par des valeurs dynamiques](#release) de l’espace partenaires.

    La balise **Grid** complète ressemble à ce code.

    ``` xml
    <Grid Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
        <UI:AdControl ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
            AdUnitId="test"
            HorizontalAlignment="Left"
            Height="250"
            VerticalAlignment="Top"
            Width="300"/>
    </Grid>
    ```

    Le code complet du fichier MainPage.xaml doit ressembler à ceci.

    ``` xml
    <Page
      x:Class="MyAdFundedWindows10AppXAML.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:MyAdFundedWindows10AppXAML"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      xmlns:UI="using:Microsoft.Advertising.WinRT.UI"
      mc:Ignorable="d">
      <Grid Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
            <UI:AdControl ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
                  AdUnitId="test"
                  HorizontalAlignment="Left"
                  Height="250"
                  VerticalAlignment="Top"
                  Width="300"/>
      </Grid>
    </Page>
    ```

6.  Compilez et exécutez l’application pour la voir avec une publicité.

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>Publier votre application avec des publicités en direct

1. Assurez-vous que votre utilisation des bannières publicitaires dans votre application suit nos [recommandations pour les bannières publicitaires](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads).

2.  Dans l’espace partenaires, accédez à la page [annonces dans l’application](../publish/in-app-ads.md) et [créez une unité ad](set-up-ad-units-in-your-app.md#live-ad-units). Pour le type d’unité publicitaire, spécifiez **Bannière**. Prenez note de l’ID d’unité publicitaire et de l’ID de l’application.
    > [!NOTE]
    > Les valeurs d’ID d’application pour les unités AD de test et les unités ad UWP activent des formats différents. Les valeurs d’ID d’application de test sont des GUID. Lorsque vous créez une unité ad UWP en temps réel dans l’espace partenaires, la valeur de l’ID d’application pour l’unité ad correspond toujours à l’ID de magasin de votre application (un exemple de valeur d’ID de magasin ressemble à 9NBLGGH4R315).

3. Vous pouvez éventuellement activer la médiation AD pour les **classe AdControl** en configurant les paramètres de la section [paramètres de médiation](../publish/in-app-ads.md#mediation) sur la page [ADS dans l’application](../publish/in-app-ads.md) . La médiation ad vous permet d’optimiser votre chiffre d’affaires et vos fonctionnalités de promotion d’applications en affichant des publicités de plusieurs réseaux Active Directory, notamment des publicités provenant d’autres réseaux Active Directory payants, tels que Taboola et Smaato et ADS pour les campagnes de promotion des applications Microsoft.

4.  Dans votre code, remplacez les valeurs de l’unité ad du test (**ApplicationID** et **AdUnitId**) par les valeurs dynamiques que vous avez générées dans l’espace partenaires.

5.  [Soumettez votre application](../publish/app-submissions.md) au magasin à l’aide de l’espace partenaires.

6.  Passez en revue les [rapports de performances publicitaires](../publish/advertising-performance-report.md) dans l’espace partenaires.

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>Gérer les unités AD pour plusieurs contrôles Active Directory dans votre application

Vous pouvez utiliser plusieurs objets **classe AdControl** dans une même application (par exemple, chaque page de votre application peut héberger un objet **classe AdControl** différent). Dans ce scénario, nous vous recommandons d’affecter une unité ad différente à chaque contrôle. L’utilisation de différentes unités AD pour chaque contrôle vous permet de [configurer séparément les paramètres de médiation](../publish/in-app-ads.md#mediation) et d’obtenir des données de [rapport](../publish/advertising-performance-report.md) discrètes pour chaque contrôle. Cela permet également à nos services de mieux optimiser les publicités que nous avons utilisées dans votre application.

> [!IMPORTANT]
> Vous pouvez utiliser chaque unité ad dans une seule application. Si vous utilisez une unité ad dans plusieurs applications, les publicités ne sont pas prises en charge pour cette unité Active Directory.

## <a name="related-topics"></a>Rubriques connexes

* [Recommandations pour les bannières publicitaires](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads)
* [Gestion des erreurs dans la procédure pas à pas XAML/C#](error-handling-in-xamlc-walkthrough.md).
* [Exemples de publicité sur GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
* [Configurer des unités AD pour votre application](set-up-ad-units-in-your-app.md)