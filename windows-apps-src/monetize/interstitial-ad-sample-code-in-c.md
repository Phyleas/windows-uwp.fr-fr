---
ms.assetid: 7a16b0ca-6b8e-4ade-9853-85690e06bda6
description: Apprenez à afficher et à lancer une publicité interstitielle dans une application plateforme Windows universelle (UWP) à l’aide de C# et XAML.
title: Exemple de code pour spot publicitaire en C#
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, ADS, publicité, interstitiel, c#, exemple de code
ms.localizationpriority: medium
ms.openlocfilehash: 3d80e53fd8d122e01588ffcbd16f487e1526c08d
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363682"
---
# <a name="interstitial-ad-sample-code-in-c"></a>Exemple de code de publicité interstitielle en C\# #  

>[!WARNING]
> Depuis le 1er juin 2020, la plateforme de monétisation Microsoft AD pour les applications Windows UWP sera arrêtée. [En savoir plus](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Cette rubrique fournit l’exemple de code complet pour une application de base C# et XAML plateforme Windows universelle (UWP) qui affiche une publicité vidéo interstitielle. Pour connaître les instructions pas à pas pour configurer votre projet pour qu’il utilise ce code, voir [Spots publicitaires](interstitial-ads.md). Pour obtenir un exemple de projet complet, consultez les [exemples de publicité sur GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).

## <a name="code-example"></a>Exemple de code

Cette section affiche le contenu des fichiers MainPage.xaml et MainPage.xaml.cs d’une application de base, qui comporte un spot publicitaire. Pour utiliser ces exemples, copiez le code dans un projet d' **application vide Visual C# (Windows universel)** dans Visual Studio.

Cet exemple d’application utilise deux boutons pour demander, puis lancer un spot publicitaire. Remplacez les valeurs des ```myAppId``` champs et ```myAdUnitId``` par les valeurs dynamiques de l’espace partenaires avant de soumettre votre application au Windows Store. Pour plus d’informations, voir [Configurer des unités publicitaires dans votre application](set-up-ad-units-in-your-app.md#live-ad-units).

> [!NOTE]
> Pour modifier cet exemple afin d’afficher une bannière publicitaire interstitielle au lieu d’une publicité de vidéo interstitielle, transmettez la valeur **AdType. Display** au premier paramètre de la méthode [RequestAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) au lieu de **AdType. Video**. Pour plus d’informations, consultez les [publicités interstitielles](interstitial-ads.md).

### <a name="mainpagexaml"></a>MainPage.xaml

> [!div class="tabbedCodeSnippets"]
:::code language="xml" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml" range="1-13":::

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs" id="CompleteSample":::

 
## <a name="related-topics"></a>Rubriques connexes

* [Exemples de publicité sur GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
 
