---
title: Discours, voix et conversation dans Windows 10
description: Cette page fournit des informations vous permettant de commencer à développer des applications Windows avec reconnaissance vocale.
ms.topic: article
ms.date: 09/12/2019
keywords: Reconnaissance vocale dans Windows 10, discours, voix, conversation, applications vocales Win32, applications vocales UWP, applications vocales WPF, applications vocales WinForms
ms.author: kbridge
author: Karl-Bridge-Microsoft
ms.openlocfilehash: 7ac8d782591ce8f3716e491714c4cbf241e80b6c
ms.sourcegitcommit: 8a88a05ad89aa180d41a93152632413694f14ef8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726549"
---
# <a name="speech-voice-and-conversation-in-windows-10"></a>Discours, voix et conversation dans Windows 10

![Image de héros de reconnaissance vocale](images/hero-speech-composite-small.png)

La reconnaissance vocale peut être un moyen efficace, naturel et agréable pour les utilisateurs d’interagir avec vos applications Windows, en complément ou même en remplaçant les expériences d’interaction traditionnelles basées sur la souris, le clavier, le toucher, le contrôleur ou les gestes.

Les fonctionnalités vocales telles que la reconnaissance vocale, la dictée, la synthèse vocale (également appelée conversion de texte par synthèse vocale ou TTS) et les assistants vocaux de conversation (tels que Cortana ou Alexa) peuvent fournir des expériences utilisateur accessibles et inclusifs qui permettent aux utilisateurs d’utiliser vos applications lorsque d’autres périphériques d’entrée peuvent ne pas suffire.

Cette page fournit des informations sur la façon dont les différentes infrastructures de développement Windows assurent la reconnaissance vocale, la synthèse vocale et la prise en charge de la conversation pour les développeurs qui créent des applications Windows.

## <a name="platform-specific-documentation"></a>Documentation spécifique à la plateforme

:::row:::
   :::column:::
      ![Plateforme Windows universelle (UWP)](images/platform-uwp.png)

      **Plateforme Windows universelle (UWP)**

      Créez des applications vocales sur la plateforme moderne pour les applications et les jeux Windows 10, sur n’importe quel appareil Windows (y compris les PC, les téléphones, Xbox One, HoloLens, etc.) et publiez-les sur le Microsoft Store.

      [Interactions vocales](https://docs.microsoft.com/windows/uwp/design/input/speech-interactions)

      [Reconnaissance vocale](https://docs.microsoft.com/windows/uwp/design/input/speech-recognition)

      [Dictée continue](https://docs.microsoft.com/windows/uwp/design/input/enable-continuous-dictation)

      [Synthèse vocale](https://docs.microsoft.com/uwp/api/windows.media.speechsynthesis)

      [Agents de conversation](https://docs.microsoft.com/uwp/api/windows.applicationmodel.conversationalagent)

      [Commandes vocales Cortana](https://docs.microsoft.com/cortana/voice-commands/vcd)
   :::column-end:::
   :::column:::
      ![Applications de plateforme Win32](images/platform-win32.png)

      **plateforme Win32**

      Développez des applications vocales pour Windows Desktop et Windows Server à l’aide des outils, des informations et des exemples de moteurs et d’applications fournis ici.

      [Plateforme Microsoft Speech-Kit de développement logiciel (SDK) (version 11)](https://www.microsoft.com/download/details.aspx?id=27226)
      
      [Microsoft Speech SDK, version 5,1](https://www.microsoft.com/download/details.aspx?id=10121)
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      ![.NET](images/platform-dotnet.png)

      **.NET Framework**

      Développez des applications et des outils accessibles sur la plateforme établie pour les applications Windows gérées avec un modèle d’interface utilisateur XAML et le .NET Framework.

      [Guide de programmation System. Speech pour .NET Framework](https://docs.microsoft.com/previous-versions/office/developer/speech-technologies/hh361625(v=office.14))
   :::column-end:::
   :::column:::
      ![Services de reconnaissance vocale Azure](images/platform-azure-speech.png)

      **Services de reconnaissance vocale Azure**

      Concevez, créez et testez des sites Web accessibles avec Azure Speech services.

      [Parole en texte](https://azure.microsoft.com/services/cognitive-services/speech-to-text/)

      [Conversion de texte par synthèse vocale](https://azure.microsoft.com/services/cognitive-services/text-to-speech/)
      
      [Traduction vocale](https://azure.microsoft.com/services/cognitive-services/speech-translation/)

      [Assistants virtuels de la voix en premier](https://docs.microsoft.com/azure/cognitive-services/speech-service/voice-first-virtual-assistants)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      **Fonctionnalités héritées**

      Versions héritées, déconseillées et/ou non prises en charge de la technologie Microsoft Speech.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Agent Microsoft](https://docs.microsoft.com/windows/win32/lwef/microsoft-agent)

      [Kit de développement logiciel (SDK) Microsoft Speech application (SASDK) version 1,0](https://www.microsoft.com/download/details.aspx?id=2200)
   :::column-end:::
   :::column:::
      [API Microsoft Speech (SAPI) 5,3](https://docs.microsoft.com/previous-versions/windows/desktop/ms723627(v=vs.85))

      [API Microsoft Speech (SAPI) 5,4](https://docs.microsoft.com/previous-versions/windows/desktop/ee125663(v=vs.85))

      [Contrôle de reconnaissance Reconnaissance vocale Bing](https://docs.microsoft.com/previous-versions/bing/speech/dn434583(v%3dmsdn.10))
   :::column-end:::
:::row-end:::

## <a name="samples"></a>exemples

Téléchargez et exécutez des exemples Windows complets qui illustrent diverses fonctionnalités d’accessibilité.

:::row:::
   :::column:::
      [Navigateur d’exemples de code](https://docs.microsoft.com/samples/browse/?term=speech)

      Le nouveau navigateur d’exemples (remplace la Galerie de code MSDN).
   :::column-end:::
   :::column:::
      [Exemples classiques Windows sur GitHub](https://github.com/microsoft/Windows-classic-samples/search?q=speech&unscoped_q=speech)

      Ces exemples illustrent les fonctionnalités et le modèle de programmation de Windows et Windows Server. 
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Exemples de la plateforme Windows universelle (UWP) sur GitHub](https://github.com/microsoft/Windows-universal-samples/search?q=speech&unscoped_q=speech)

      Ces exemples illustrent les modèles d’utilisation des API pour la plateforme Windows universelle (UWP) dans le kit de développement logiciel (SDK) Windows pour Windows 10.
   :::column-end:::
   :::column:::
      [Galerie de contrôles XAML](https://github.com/microsoft/Xaml-Controls-Gallery)

      Cette application présente les différents contrôles XAML pris en charge dans le système Fluent Design.
   :::column-end:::
:::row-end:::

## <a name="videos"></a>Vidéos

Différentes vidéos qui décrivent comment créer des applications Windows qui incorporent des interactions vocales.

:::row:::
   :::column:::
      **Découverte approfondie de Cortana et de la plateforme de reconnaissance vocale**
   :::column-end:::
   :::column:::
      **Extensibilité de Cortana dans les applications Windows universelles**
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2015/3-716/player]
   :::column-end:::
   :::column:::
      > [!VIDEO https://channel9.msdn.com/Events/Build/2015/2-691/player]
   :::column-end:::
:::row-end:::

## <a name="other-resources"></a>Autres ressources

:::row:::
   :::column:::
      **Blogs et actualités**

      La dernière version du monde de Microsoft Speech.
   :::column-end:::
   :::column:::
      **Communauté et support**

      Où les développeurs et les utilisateurs de Windows rencontrent et apprennent ensemble.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [À la une](https://news.microsoft.com/?s=speech)

      [Blogs vocaux](https://blogs.windows.com/windowsdeveloper/?s=speech)
   :::column-end:::
   :::column:::
      [Communauté Windows-reconnaissance vocale](https://community.windows.com/search?q=speech)

      [Forum du développeur Windows Speech](https://social.msdn.microsoft.com/Forums/home?filter=alltypes&sort=firstpostdesc&searchTerm=speech)
   :::column-end:::
:::row-end:::
