---
ms.assetid: adb2fa45-e18f-4254-bd8b-a749a386e3b4
description: Découvrez comment utiliser la classe classe AdControl pour afficher des bannières publicitaires dans une application JavaScript/HTML pour Windows 10 (UWP).
title: AdControl en HTML 5 et JavaScript
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, ADS, publicité, classe AdControl, contrôle AD, JavaScript, HTML
ms.localizationpriority: medium
ms.openlocfilehash: b99432c40bf2b4633e8902a5bb7b7eedab3119dc
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364052"
---
# <a name="adcontrol-in-html-5-and-javascript"></a>AdControl en HTML 5 et JavaScript

>[!WARNING]
> Depuis le 1er juin 2020, la plateforme de monétisation Microsoft AD pour les applications Windows UWP sera arrêtée. [En savoir plus](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Cette procédure pas à pas montre comment utiliser la classe [classe AdControl](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) pour afficher les bannières publicitaires dans une application JavaScript/html plateforme Windows universelle (UWP) pour Windows 10.

Pour un exemple de projet complet illustrant l’ajout de bannières publicitaires à une application HTML/JavaScript, voir [Exemples de publicité sur GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).

## <a name="prerequisites"></a>Prérequis

* Installez le [Kit de développement logiciel (SDK) Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) avec visual studio 2015 ou une version ultérieure de Visual Studio. Pour obtenir des instructions d’installation, consultez [cet article](install-the-microsoft-advertising-libraries.md).

> [!NOTE]
> Si vous avez installé le kit de développement logiciel (SDK) Windows 10 version 10.0.14393 (mise à jour anniversaire) ou une version ultérieure du SDK Windows, vous devez également installer la bibliothèque [WinJS](https://github.com/winjs/winjs) . Cette bibliothèque a été utilisée dans les versions précédentes du SDK Windows pour Windows 10, mais à compter de la version 10.0.14393 du SDK Windows 10 (mise à jour anniversaire), cette bibliothèque doit être installée séparément. 

## <a name="integrate-a-banner-ad-into-your-app"></a>Intégrer une bannière publicitaire dans votre application

1. Dans Visual Studio, ouvrez votre projet ou créez-en un.

    > [!NOTE]
    > Si vous utilisez un projet existant, ouvrez le fichier Package. appxmanifest dans votre projet et assurez-vous que la fonctionnalité **Internet (client)** est sélectionnée. Votre application a besoin de cette fonctionnalité pour recevoir des publicités de test et des publicités en direct.

2. Si votre projet cible **Toute UC**, mettez-le à jour pour utiliser une sortie de génération propre à l’architecture (par exemple, **x86**). Si votre projet cible **Toute UC**, vous ne pourrez pas ajouter une référence à la bibliothèque de publicités Microsoft dans les étapes suivantes. Pour plus d’informations, consultez [Erreurs de référence provoquées par le ciblage de Toute UC dans votre projet](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Ajoutez une référence au kit de développement logiciel (SDK) Microsoft Advertising dans votre projet :

    1. Dans la fenêtre **Explorateur de solutions** , cliquez avec le bouton droit sur **références**, puis sélectionnez **Ajouter une référence...**
    2.  Dans **Gestionnaire de références**, développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour JavaScript** (version 10.0).
    3.  Dans **Gestionnaire de références**, cliquez sur OK.

6.  Ouvrez le fichier index.html (ou un autre fichier html en fonction de votre projet).

7.  Dans la section ** &lt; Head &gt; ** , après les références JavaScript du projet default. CSS et main.js, ajoutez la référence à ad.js.

    ``` HTML
    <!-- Advertising required references -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

    > [!NOTE]
    > Cette ligne doit être placée dans la section ** &lt; Head &gt; ** après l’inclusion de main.js ; dans le cas contraire, vous rencontrerez une erreur lors de la génération de votre projet.

8.  Modifiez la section du ** &lt; corps &gt; ** dans le fichier default.html (ou un autre fichier HTML en fonction de votre projet) pour inclure la **balise div** pour le **classe AdControl**. Assignez les propriétés **ApplicationID** et **adUnitId** de **classe AdControl** aux [valeurs de l’unité ad du test](set-up-ad-units-in-your-app.md#test-ad-units). Ajustez également la **hauteur** et la **largeur** de sorte que le contrôle soit une des [tailles de publicité prises en charge pour les bannières publicitaires](supported-ad-sizes-for-banner-ads.md).

    > [!NOTE]
    > Chaque **classe AdControl** a une *unité publicitaire* correspondante qui est utilisée par nos services pour servir les publicités au contrôle, et chaque unité ad se compose d’un *ID d’unité Active Directory* et d’un *ID d’application*. Dans ces étapes, vous attribuez des valeurs d’ID d’unité ad et d’ID d’application de test à votre contrôle. Ces valeurs de test ne peuvent être utilisées que dans une version test de votre application. Avant de publier votre application dans le Windows Store, vous devez [remplacer ces valeurs de test par des valeurs dynamiques](#release) de l’espace partenaires.

    ``` HTML
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
        data-win-control="MicrosoftNSJS.Advertising.AdControl"
        data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    ```

9.  Compilez et exécutez l’application pour la voir avec une publicité.

L’exemple suivant montre l’intégralité du index.html pour une application simple.

``` HTML
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>AdControlExampleApp</title>
    <!-- WinJS references -->
    <link href="lib/winjs-4.0.1/css/ui-light.css" rel="stylesheet" />
    <script src="lib/winjs-4.0.1/js/base.js"></script>
    <script src="lib/winjs-4.0.1/js/ui.js"></script>
    <!-- AdControlExampleApp references -->
    <link href="css/default.css" rel="stylesheet" />
    <script src="js/main.js"></script>
    <!-- Required reference for AdControl -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
</head>
<body>
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    <p>Content goes here</p>
</body>
</html>
```

### <a name="create-an-adcontrol-programmatically-in-javascript"></a>Créer un classe AdControl par programmation dans JavaScript

Les étapes précédentes montrent comment déclarer un **classe AdControl** dans votre balisage HTML. Vous pouvez également créer par programmation un **classe AdControl** à l’aide de JavaScript. Cet exemple suppose que vous utilisez une **balise div** existante dans votre code HTML avec l’ID **myAd**.

> [!div class="tabbedCodeSnippets"]
:::code language="javascript" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/AdControlSamples/js/main.js" id="DeclareAdControl":::

Cet exemple suppose que vous avez déjà déclaré les méthodes de gestionnaires d’événements **myAdError**, **myAdRefreshed** et **myAdEngagedChanged**.

Si vous utilisez ce code et que vous ne voyez pas de publicités, vous pouvez essayer d’insérer un attribut de **position:relative** dans l’élément **div** qui contient le **AdControl**. Cela remplace le paramètre par défaut de l’élément **IFrame**. Les publicités apparaissent correctement, sauf si elles ne sont pas affichées en raison de la valeur de cet attribut. Notez que les nouvelles unités publicitaires peuvent ne pas être disponibles pendant 30 minutes.

> [!NOTE]
> Les valeurs *ApplicationID* et *adUnitId* affichées dans cet exemple sont des valeurs de [mode de test](set-up-ad-units-in-your-app.md#test-ad-units). Vous devez [remplacer ces valeurs par des valeurs dynamiques](set-up-ad-units-in-your-app.md#live-ad-units) de l’espace partenaires avant de soumettre votre application pour envoi.

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>Publier votre application avec des publicités en direct

1. Assurez-vous que votre utilisation des bannières publicitaires dans votre application suit nos [recommandations pour les bannières publicitaires](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads).

1.  Dans l’espace partenaires, accédez à la page [annonces dans l’application](../publish/in-app-ads.md) et [créez une unité ad](set-up-ad-units-in-your-app.md#live-ad-units). Pour le type d’unité publicitaire, spécifiez **Bannière**. Prenez note de l’ID d’unité publicitaire et de l’ID de l’application.
    > [!NOTE]
    > Les valeurs d’ID d’application pour les unités AD de test et les unités ad UWP activent des formats différents. Les valeurs d’ID d’application de test sont des GUID. Lorsque vous créez une unité ad UWP en temps réel dans l’espace partenaires, la valeur de l’ID d’application pour l’unité ad correspond toujours à l’ID de magasin de votre application (un exemple de valeur d’ID de magasin ressemble à 9NBLGGH4R315).

2. Vous pouvez éventuellement activer la médiation AD pour les **classe AdControl** en configurant les paramètres de la section [paramètres de médiation](../publish/in-app-ads.md#mediation) sur la page [ADS dans l’application](../publish/in-app-ads.md) . La médiation ad vous permet d’optimiser votre chiffre d’affaires et vos fonctionnalités de promotion d’applications en affichant des publicités de plusieurs réseaux Active Directory, notamment des publicités provenant d’autres réseaux Active Directory payants, tels que Taboola et Smaato et ADS pour les campagnes de promotion des applications Microsoft.

3.  Dans votre code, remplacez les valeurs de l’unité ad du test (**ApplicationID** et **adUnitId**) par les valeurs dynamiques que vous avez générées dans l’espace partenaires.

4.  [Soumettez votre application](../publish/app-submissions.md) au magasin à l’aide de l’espace partenaires.

5.  Passez en revue les [rapports de performances publicitaires](../publish/advertising-performance-report.md) dans l’espace partenaires.             

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>Gérer les unités AD pour plusieurs contrôles Active Directory dans votre application

Vous pouvez utiliser plusieurs objets **classe AdControl** dans une même application (par exemple, chaque page de votre application peut héberger un objet **classe AdControl** différent). Dans ce scénario, nous vous recommandons d’affecter une unité ad différente à chaque contrôle. L’utilisation de différentes unités AD pour chaque contrôle vous permet de [configurer séparément les paramètres de médiation](../publish/in-app-ads.md#mediation) et d’obtenir des données de [rapport](../publish/advertising-performance-report.md) discrètes pour chaque contrôle. Cela permet également à nos services de mieux optimiser les publicités que nous avons utilisées dans votre application.

> [!IMPORTANT]
> Vous pouvez utiliser chaque unité ad dans une seule application. Si vous utilisez une unité ad dans plusieurs applications, les publicités ne sont pas prises en charge pour cette unité Active Directory.

## <a name="related-topics"></a>Rubriques connexes

* [Recommandations pour les bannières publicitaires](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads)
* [Exemples de publicité sur GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
* [Configurer des unités AD pour votre application](set-up-ad-units-in-your-app.md)
* [Gestion des erreurs dans la procédure pas à pas pour JavaScript](error-handling-in-javascript-walkthrough.md)
