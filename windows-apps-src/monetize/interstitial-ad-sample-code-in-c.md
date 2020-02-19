---
ms.assetid: 7a16b0ca-6b8e-4ade-9853-85690e06bda6
description: Découvrez comment lancer un spot publicitaire en C#.
title: Exemple de code pour spot publicitaire en C#
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, pub, publicités, spots, c#, exemple de code
ms.localizationpriority: medium
ms.openlocfilehash: 4179d0dde5577093a2ae30c539adf0a8c544d56b
ms.sourcegitcommit: 6af7ce0e3c27f8e52922118deea1b7aad0ae026e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77463361"
---
# <a name="interstitial-ad-sample-code-in-c"></a>Exemple de code de publicité interstitielle en C\# #  

>[!WARNING]
> Depuis le 1er juin 2020, la plateforme de monétisation Microsoft AD pour les applications Windows UWP sera arrêtée. [En savoir plus](https://aka.ms/ad-monetization-shutdown)

Cette rubrique indique l’exemple de code complet pour une application de plateforme Windows universelle (UWP) de base en C# et XAML, qui comporte un spot vidéo publicitaire. Pour connaître les instructions pas à pas pour configurer votre projet pour qu’il utilise ce code, voir [Spots publicitaires](interstitial-ads.md). Pour obtenir un exemple de projet complet, consultez les [exemples de publicité sur GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).

## <a name="code-example"></a>Exemple de code

Cette section affiche le contenu des fichiers MainPage.xaml et MainPage.xaml.cs d’une application de base, qui comporte un spot publicitaire. Pour utiliser ces exemples, copiez le code dans un projet Visual C# **Application vide (Windows universelle)** au sein de Visual Studio.

Cet exemple d’application utilise deux boutons pour demander, puis lancer un spot publicitaire. Remplacez les valeurs des champs ```myAppId``` et ```myAdUnitId``` par des valeurs dynamiques de l’espace partenaires avant de soumettre votre application au Windows Store. Pour plus d’informations, voir [Configurer des unités publicitaires dans votre application](set-up-ad-units-in-your-app.md#live-ad-units).

> [!NOTE]
> Pour modifier cet exemple de sorte à afficher une bannière au lieu d’un spot vidéo, transmettez la valeur **AdType.Display** au premier paramètre de la méthode [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) au lieu de **AdType.Video**. Pour plus d’informations, voir [Spots](interstitial-ads.md).

### <a name="mainpagexaml"></a>MainPage.xaml

> [!div class="tabbedCodeSnippets"]
[!code-xml[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml#L1-L13)]

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

> [!div class="tabbedCodeSnippets"]
[!code-csharp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#CompleteSample)]

 
## <a name="related-topics"></a>Rubriques connexes

* [Exemples de publicité sur GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
 
