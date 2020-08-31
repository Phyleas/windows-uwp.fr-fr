---
ms.assetid: cb7380d0-bc14-4936-aa1c-206304b3dc70
description: Découvrez comment gérer les erreurs générées par la classe AdControl dans les bibliothèques de publicités Microsoft.
title: Gérer des erreurs dans les publicités
ms.date: 02/18/2020
ms.topic: article
keywords: 'Windows 10, UWP, ADS, publicité, gestion des erreurs, JavaScript, XAML, c #'
ms.localizationpriority: medium
ms.openlocfilehash: 2a1a82c9977bfbe61712d39e4d23fdd68cd598ae
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171553"
---
# <a name="handle-ad-errors"></a>Gérer des erreurs dans les publicités

>[!WARNING]
> Depuis le 1er juin 2020, la plateforme de monétisation Microsoft AD pour les applications Windows UWP sera arrêtée. [En savoir plus](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Les classes [classe AdControl](/uwp/api/microsoft.advertising.winrt.ui.adcontrol),  [InterstitialAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad)et [NativeAdsManagerV2](/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2) ont chacune un événement **ErrorOccurred** qui est déclenché si une erreur liée à Active Directory se produit. Votre code d’application peut gérer cet événement et examiner les propriétés [ErrorCode](/uwp/api/microsoft.advertising.winrt.ui.aderroreventargs.errorcode) et [ErrorMessage](/uwp/api/microsoft.advertising.winrt.ui.aderroreventargs.errormessage) de l’objet d’arguments d’événement pour aider à déterminer la cause de l’erreur.

<span id="bkmk-dotnet"/>

## <a name="xaml-apps"></a>Applications XAML

Pour gérer les erreurs liées à ad dans une application XAML :

1. Assignez l’événement **ErrorOccurred** de votre objet **classe AdControl**, **InterstitialAd**ou **NativeAdsManagerV2** au nom d’un délégué de gestionnaire d’événements.

2. Codez le délégué de gestion des événements d’erreur afin qu’il prenne deux paramètres : un **Object** pour l’expéditeur et un objet [AdErrorEventArgs](/uwp/api/microsoft.advertising.winrt.ui.aderroreventargs).

Voici un exemple qui affecte un délégué nommé **OnAdError** à l’événement **ErrorOccurred** d’un objet **classe AdControl** nommé *myBannerAdControl*.

> [!div class="tabbedCodeSnippets"]
``` csharp
myBannerAdControl.ErrorOccurred = OnAdError;
```

Voici un exemple de définition du délégué **OnAdError** qui écrit des informations d’erreur dans la fenêtre Sortie de Visual Studio.

> [!div class="tabbedCodeSnippets"]
``` csharp
private void OnAdError(object sender, AdErrorEventArgs e)
{
    System.Diagnostics.Debug.WriteLine("AdControl error (" + ((AdControl)sender).Name + "): " + e.Error +
        " ErrorCode: " + e.ErrorCode.ToString());
}
```

Voir [Gestion des erreurs dans la procédure pas à pas pour XAML/C#](error-handling-in-xamlc-walkthrough.md) pour consulter une procédure pas à pas montrant la gestion des erreurs **AdControl** en XAML et C#.

<span id="bkmk-javascript"/>

## <a name="javascripthtml-apps"></a>Applications JavaScript/HTML

Pour gérer les erreurs de **ErrorOccur** dans une application JavaScript :

1.  Affectez l’événement **onErrorOccurred** à un gestionnaire d’événements.

2.  Codez le gestionnaire d’événements.

Voici un exemple qui affecte un gestionnaire d’événements nommé **errorLogger** à l’événement **ErrorOccurred** d’un objet **classe AdControl** .

> [!div class="tabbedCodeSnippets"]
``` html
<div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 250px; height: 250px; z-index: 1"
     data-win-control="MicrosoftNSJS.Advertising.AdControl"
     data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test', onErrorOccurred: errorLogger}">
</div>
```

La fonction de gestion des erreurs est déclarative et doit être incorporée à la fonction [markSupportedForProcessing](/previous-versions/windows/apps/hh967819(v=win.10)).

Le gestionnaire d’erreurs intercepte l’objet d’erreur JavaScript lorsqu’une erreur se produit. Cet objet fournit deux arguments au gestionnaire d’erreurs. Pour plus d’informations, voir [Propriétés d’erreur particulières des méthodes Windows Runtime asynchrones](/scripting/jswinrt/special-error-properties-from-asynchronous-windows-runtime-methods).

Voici un exemple de fonction de gestion des erreurs nommé **errorLogger** qui gère l’événement **onErrorOccurred**.

> [!div class="tabbedCodeSnippets"]
``` javascript
WinJS.Utilities.markSupportedForProcessing(
window.errorLogger = function (sender, evt) {
    console.log(new Date()).toLocaleTimeString() + ": " + sender.element.id + " error: " + evt.errorMessage +
    " error code: " + evt.errorCode + \n");
});
```

Voir [Gestion des erreurs dans la procédure pas à pas pour JavaScript](error-handling-in-javascript-walkthrough.md) pour consulter une procédure pas à pas montrant la gestion des erreurs **AdControl** en JavaScript.