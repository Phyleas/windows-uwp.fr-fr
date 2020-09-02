---
ms.assetid: ''
description: Découvrez comment gérer la diffusion de jeux pour une application plateforme Windows universelle (UWP) à l’aide de l’interface utilisateur du système Windows et des extensions de bureau Windows pour UWP.
title: Gérer la diffusion d’un jeu
ms.date: 09/27/2017
ms.topic: article
keywords: Windows 10, jeu, diffusion
ms.localizationpriority: medium
ms.openlocfilehash: 87c35bb4612ad970f01853b2ace46b44b1882781
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362492"
---
# <a name="manage-game-broadcasting"></a>Gérer la diffusion d’un jeu
Cet article explique comment gérer la diffusion de jeux pour une application UWP. Les utilisateurs doivent lancer la diffusion à l’aide de l’interface utilisateur système intégrée à Windows, mais à partir de la version 1709 de Windows 10, les applications peuvent lancer l’interface utilisateur de diffusion du système et recevoir des notifications au démarrage et à l’arrêt de la diffusion.

## <a name="add-the-windows-desktop-extensions-for-the-uwp-to-your-app"></a>Ajouter les extensions de bureau Windows pour la série UWP à votre application
Les API de gestion de la diffusion d’applications, qui se trouvent dans l’espace de noms **[Windows. Media. AppBroadcasting](/uwp/api/windows.media.appbroadcasting)** , ne sont pas incluses dans le contrat d’API universel. Pour accéder aux API, vous devez ajouter une référence aux extensions de bureau Windows pour la série UWP à votre application en suivant les étapes ci-dessous.

1. Dans Visual Studio, dans **Explorateur de solutions**, développez votre projet UWP et cliquez avec le bouton droit sur **références** , puis sélectionnez **Ajouter une référence...**. 
2. Développez le nœud **Windows universel** et sélectionnez **Extensions**.
3. Dans la liste des extensions, cochez la case en regard des **extensions du bureau Windows pour l’entrée UWP** correspondant à la build cible de votre projet. Pour les fonctionnalités de diffusion d’application, la version doit être supérieure ou égale à 1709.
4. Cliquez sur **OK**.

## <a name="launch-the-system-ui-to-allow-the-user-to-initiate-broadcasting"></a>Lancer l’interface utilisateur du système pour permettre à l’utilisateur de lancer la diffusion
Il existe plusieurs raisons pour lesquelles votre application peut ne pas être en mesure de diffuser actuellement, y compris si l’appareil actuel ne répond pas à la configuration matérielle requise pour la diffusion ou si une autre application est en cours de diffusion. Avant de lancer l’interface utilisateur du système, vous pouvez vérifier si votre application est actuellement en mesure de diffuser. Tout d’abord, vérifiez si les API de diffusion sont disponibles sur l’appareil actuel. Les API ne sont pas disponibles sur les appareils exécutant une version du système d’exploitation antérieure à Windows 10, version 1709. Au lieu de vérifier une version spécifique du système d’exploitation, utilisez la méthode **[ApiInformation. IsApiContractPresent](/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** pour rechercher la version de *Windows. Media. AppBroadcasting. AppBroadcastingContract* version 1,0. Si ce contrat est présent, les API de diffusion sont disponibles sur l’appareil.

Ensuite, récupérez une instance de la classe **[AppBroadcastingUI](/uwp/api/windows.media.appbroadcasting.appbroadcastingui)** en appelant la méthode de fabrique **[GetDefault](/uwp/api/windows.media.appbroadcasting.appbroadcastingui.GetDefault)** sur PC, où un seul utilisateur est connecté à la fois. Sur Xbox, où plusieurs utilisateurs peuvent être connectés, appelez **[GetForUser](/uwp/api/windows.media.appbroadcasting.appbroadcastingui.getforuser)** à la place. Appelez ensuite **[GetStatus](/uwp/api/windows.media.appbroadcasting.appbroadcastingui.GetStatus)** pour recevoir l’état de diffusion de votre application.

La propriété **[CanStartBroadcast](/uwp/api/windows.media.appbroadcasting.appbroadcastingstatus.CanStartBroadcast)** de la classe **AppBroadcastingStatus** vous indique si l’application peut actuellement démarrer la diffusion. Si ce n’est pas le cas, vous pouvez vérifier la propriété **[Details](/uwp/api/windows.media.appbroadcasting.appbroadcastingstatus.Details)** pour déterminer la raison pour laquelle la diffusion n’est pas disponible. Selon la raison, vous souhaiterez peut-être afficher l’état de l’utilisateur ou afficher les instructions d’activation de la diffusion.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp" id="SnippetCanStartBroadcast":::

Demandez à ce que l’interface utilisateur de diffusion d’application soit affichée par le système en appelant **[ShowBroadcastUI](/uwp/api/windows.media.appbroadcasting.appbroadcastingui.ShowBroadcastUI)**.

> [!NOTE] 
> La méthode **ShowBroadcastUI** représente une demande qui peut échouer, en fonction de l’état actuel du système. Votre application ne doit pas supposer que la diffusion a commencé après l’appel de cette méthode. Utilisez l’événement **[IsCurrentAppBroadcastingChanged](/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor.IsCurrentAppBroadcastingChanged)** pour être averti lorsque la diffusion démarre ou s’arrête.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp" id="SnippetLaunchBroadcastUI":::

## <a name="receive-notifications-when-broadcasting-starts-and-stops"></a>Recevoir des notifications au démarrage et à l’arrêt de la diffusion
Inscrivez-vous pour recevoir des notifications lorsque l’utilisateur utilise l’interface utilisateur du système pour démarrer ou arrêter la diffusion de votre application en initialisant une instance de la classe **[AppBroadcastingMonitor](/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor)** et en inscrivant un gestionnaire pour l’événement  **[IsCurrentAppBroadcastingChanged](/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor.IsCurrentAppBroadcastingChanged)** . Comme indiqué dans la section précédente, veillez à utiliser **[ApiInformation. IsApiContractPresent](/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** à un moment donné pour vérifier que les API de diffusion sont présentes sur l’appareil avant d’essayer de les utiliser. 

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp" id="SnippetAppBroadcastingRegisterChangedHandler":::

Dans le gestionnaire de l’événement **IsCurrentAppBroadcastingChanged** , vous souhaiterez peut-être mettre à jour l’interface utilisateur de votre application pour refléter l’état actuel de la diffusion.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp" id="SnippetAppBroadcastingChangedHandler":::

## <a name="related-topics"></a>Rubriques connexes

* [Jeux](index.md)

 

 
