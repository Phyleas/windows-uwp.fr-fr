---
ms.assetid: ca92bed1-ad9e-4947-ad91-87d12de727c0
description: Consultez les notes de publication pour les bibliothèques de publication Microsoft qui prennent en charge les applications XAML et JavaScript/HTML pour Windows 10, Windows 8.1, Windows Phone 8,1 et Windows Phone 8.
title: Notes de publication pour les bibliothèques de publicité
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, ADS, publicité, notes de publication
ms.localizationpriority: medium
ms.openlocfilehash: 8faf352040b9d7bdc9fc8bc79804d5903573d41d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174963"
---
# <a name="release-notes-for-the-advertising-libraries"></a>Notes de publication pour les bibliothèques de publicité

>[!WARNING]
> Depuis le 1er juin 2020, la plateforme de monétisation Microsoft AD pour les applications Windows UWP sera arrêtée. [En savoir plus](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Cette section fournit des notes de publication pour la version actuelle des bibliothèques de publication Microsoft. Ces bibliothèques prennent en charge les applications XAML et HTML/JavaScript pour Windows 10, Windows 8.1, Windows Phone 8.1 et Windows Phone 8.

## <a name="installation"></a>Installation


Les bibliothèques de publication Microsoft sont disponibles dans le cadre du [Kit de développement logiciel (SDK) Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK). Pour plus d’informations sur l’installation du kit de développement logiciel (SDK), consultez [installer le kit de développement logiciel Microsoft Advertising](install-the-microsoft-advertising-libraries.md).

## <a name="uninstall-previous-versions"></a>Désinstaller les versions précédentes

Avant d’installer la dernière version du kit de développement logiciel (SDK) Microsoft Advertising, il est fortement recommandé de désinstaller toutes les instances antérieures du kit de développement logiciel (SDK). Pour plus d’informations, consultez [installer le kit de développement logiciel (SDK) Microsoft Advertising](install-the-microsoft-advertising-libraries.md).

## <a name="target-architecture-specific-build-outputs"></a>Cibler des sorties de génération propres à une architecture

Lorsque vous utilisez les bibliothèques de publicités Microsoft, vous ne pouvez pas cibler **Toute UC** dans votre projet. Si votre projet cible la plateforme **Toute UC**, vous pouvez voir un message d’avertissement dans votre projet une fois que vous avez ajouté une référence aux bibliothèques de publicités Microsoft. Pour supprimer cet avertissement, mettez à jour votre projet pour utiliser une sortie de génération propre à l’architecture (par exemple, **x86**). Pour plus d’informations, consultez [problèmes connus](known-issues-for-the-advertising-libraries.md).

## <a name="c-support"></a>Support C++

Les bibliothèques de publicités Microsoft (qui incluent les classes **AdControl** et **InterstitialAd**) prennent en charge les applications écrites en C++ et DirectX à l’aide de l’interopérabilité de Windows Runtime (*interop*). Pour plus d’informations et d’exemples sur la programmation en C++ et XAML, voir [Système de types](/cpp/cppcx/type-system-c-cx).

## <a name="no-toolbox-control"></a>Aucun contrôle de boîte à outils

Dans la version actuelle des bibliothèques de publication Microsoft dans le [Kit de développement logiciel (SDK) Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK), il n’existe aucun contrôle de boîte à outils pour faire glisser un **classe AdControl** ou un **InterstitialAd** vers une aire de conception dans votre application. Pour obtenir des instructions sur l’ajout de ces contrôles dans votre balisage et votre code, voir [Procédures pas à pas de développement](developer-walkthroughs.md).

## <a name="latitude-and-longitude-properties-no-longer-available"></a>Les propriétés latitude et longitude ne sont plus disponibles

La classe **AdControl** ne possède plus les propriétés **Latitude** et **Longitude** pour les applications UWP. À la place, le code intégré au contrôle de publicité détecte et envoie ces valeurs aux serveurs de publicités pour le compte de l’application.


 

 