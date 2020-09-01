---
ms.assetid: 646977ed-1705-4ea7-a3db-a6b9aac70703
description: Découvrez comment lancer une publicité interstitielle à l’aide d’une application plateforme Windows universelle (UWP) écrite en JavaScript et HTML.
title: Exemple de code pour spot publicitaire en JavaScript
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, ADS, publicité, interstitiel, JavaScript, exemple de code
ms.localizationpriority: medium
ms.openlocfilehash: 764d5dd3f48afa7c446c51ff88090cba88654eba
ms.sourcegitcommit: e273e5901bfa6596dfef4cc741bb1c42614c25ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89238264"
---
# <a name="interstitial-ad-sample-code-in-javascript"></a>Exemple de code pour spot publicitaire en JavaScript

>[!WARNING]
> Depuis le 1er juin 2020, la plateforme de monétisation Microsoft AD pour les applications Windows UWP sera arrêtée. [En savoir plus](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Cette rubrique indique l’exemple de code complet pour une application de plateforme Windows universelle (UWP) de base en JavaScript et HTML, qui comporte un spot publicitaire. Pour connaître les instructions pas à pas pour configurer votre projet pour qu’il utilise ce code, voir [Spots publicitaires](interstitial-ads.md). Pour obtenir un exemple de projet complet, consultez les [exemples de publicité sur GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).

## <a name="code-example"></a>Exemple de code

Cette section affiche le contenu des fichiers HTML et JavaScript d’une application de base, qui comporte un spot publicitaire. Pour utiliser ces exemples, copiez ce code dans un projet d' **application WinJS JavaScript (Windows universel)** dans Visual Studio.

Cet exemple d’application utilise deux boutons pour demander, puis lancer un spot publicitaire. Les fichiers main.js et index.html générés par Visual Studio ont été modifiés et sont présentés ci-dessous. Le fichier script.js affiché ci-dessous contient la plupart du code contenu dans l’exemple, et vous devez ajouter ce fichier dans le dossier **js** de votre projet.

Remplacez les valeurs des ```applicationId``` variables et ```adUnitId``` par des valeurs dynamiques de l’espace partenaires avant de soumettre votre application au Windows Store. Pour plus d’informations, voir [Configurer des unités publicitaires dans votre application](set-up-ad-units-in-your-app.md#live-ad-units).

> [!NOTE]
> Pour modifier cet exemple afin d’afficher une bannière publicitaire interstitielle au lieu d’une publicité de vidéo interstitielle, transmettez la valeur **InterstitialAdType. Display** au premier paramètre de la méthode [RequestAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) au lieu de **InterstitialAdType. Video**. Pour plus d’informations, consultez les [publicités interstitielles](interstitial-ads.md).

### <a name="indexhtml"></a>index.html

> [!div class="tabbedCodeSnippets"]
[!code-html[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/index.html#L1-L21)]

### <a name="scriptjs"></a>script.js

> [!div class="tabbedCodeSnippets"]
[!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#script)]

### <a name="mainjs"></a>main.js

> [!div class="tabbedCodeSnippets"]
[!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/main.js#main)]

## <a name="related-topics"></a>Rubriques connexes

* [Exemples de publicité sur GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)

 