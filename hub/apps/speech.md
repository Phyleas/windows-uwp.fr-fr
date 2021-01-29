---
title: Discours, voix et conversation dans Windows 10
description: Cette page fournit des informations vous permettant de commencer à développer des applications Windows avec reconnaissance vocale.
ms.topic: article
ms.date: 09/12/2019
keywords: Reconnaissance vocale dans Windows 10, discours, voix, conversation, applications vocales Win32, applications vocales UWP, applications vocales WPF, applications vocales WinForms
ms.author: kbridge
author: Karl-Bridge-Microsoft
ms.openlocfilehash: d157d33316925db6112ab892e13b2c849cfcaa5d
ms.sourcegitcommit: d51c3dd64d58c7fa9513ba20e736905f12df2a9a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98988701"
---
# <a name="speech-voice-and-conversation-in-windows-10"></a>Discours, voix et conversation dans Windows 10

![Image de héros de reconnaissance vocale](images/hero-speech-composite-small.png)

La reconnaissance vocale peut être un moyen efficace, naturel et agréable pour les utilisateurs d’interagir avec vos applications Windows, en complément ou même en remplaçant les expériences d’interaction traditionnelles basées sur la souris, le clavier, le toucher, le contrôleur ou les gestes.

Les fonctionnalités vocales telles que la reconnaissance vocale, la dictée, la synthèse vocale (également appelées synthèse vocale) et les assistants vocaux de conversation (tels que Cortana ou Alexa) peuvent fournir des expériences utilisateur accessibles et inclusifs qui permettent aux utilisateurs d’utiliser vos applications lorsque d’autres périphériques d’entrée peuvent ne pas suffire.

Cette page fournit des informations sur la façon dont les différentes infrastructures de développement Windows assurent la reconnaissance vocale, la synthèse vocale et la prise en charge de la conversation pour les développeurs qui créent des applications Windows.

## <a name="platform-specific-documentation"></a>Documentation spécifique à la plateforme

:::row:::
   :::column:::
      ![Plateforme Windows universelle (UWP)](images/platform-uwp.png)

      **Plateforme Windows universelle (UWP)**

      Créez des applications vocales sur la plateforme moderne pour les applications et les jeux Windows 10, sur n’importe quel appareil Windows (y compris les PC, les téléphones, Xbox One, HoloLens, etc.) et publiez-les sur le Microsoft Store.

      [Interactions vocales](/windows/uwp/design/input/speech-interactions)

      [Reconnaissance vocale](/windows/uwp/design/input/speech-recognition)

      [Dictée continue](/windows/uwp/design/input/enable-continuous-dictation)

      [Synthèse vocale](/uwp/api/windows.media.speechsynthesis)

      [Agents de conversation](/uwp/api/windows.applicationmodel.conversationalagent)

      [Commandes vocales Cortana](/cortana/voice-commands/vcd)
   :::column-end:::
   :::column:::
      ![Applications de plateforme Win32](images/platform-win32.png)

      **plateforme Win32**

      Développez des applications vocales pour Windows Desktop et Windows Server à l’aide des outils, des informations et des exemples de moteurs et d’applications fournis ici.

      [Microsoft Speech Platform - SDK (version 11)](https://www.microsoft.com/download/details.aspx?id=27226)
      
      [SDK Microsoft Speech version 5.1](https://www.microsoft.com/download/details.aspx?id=10121)
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      ![.NET](images/platform-dotnet.png)

      **.NET Framework**

      Développez des applications et des outils accessibles sur la plateforme établie pour les applications Windows gérées avec un modèle d’interface utilisateur XAML et le .NET Framework.

      [Guide de programmation System.Speech pour le .NET Framework](/previous-versions/office/developer/speech-technologies/hh361625(v=office.14))
   :::column-end:::
   :::column:::
      ![Services de reconnaissance vocale Azure](images/platform-azure-speech.png)

      **Services de reconnaissance vocale Azure**

      Intégrer le traitement vocal dans des applications et des services.

      [Reconnaissance vocale](https://azure.microsoft.com/services/cognitive-services/speech-to-text/)

      [Synthèse vocale](https://azure.microsoft.com/services/cognitive-services/text-to-speech/)
      
      [Traduction vocale](https://azure.microsoft.com/services/cognitive-services/speech-translation/)

      [Reconnaissance de l’orateur](https://azure.microsoft.com/en-us/services/cognitive-services/speaker-recognition/)

      [Assistants virtuels de la voix en premier](/azure/cognitive-services/speech-service/voice-first-virtual-assistants)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="2":::
      **Fonctionnalités héritées**

      Versions héritées, déconseillées et/ou non prises en charge de Microsoft Speech and conversation Technology.
   :::column-end:::
:::row-end:::
:::row:::
   :::column:::
      [Cortana Skills Kit](/cortana/skills/) (Kit de compétences Cortana)

      Dans le cadre de notre objectif de transformer les expériences de productivité modernes en incorporant Cortana profondément dans Microsoft 365, nous allons mettre à niveau le kit de compétences Cortana pour les consommateurs (plateforme de développement) et toutes les compétences basées sur cette plateforme.
   :::column-end:::
   :::column:::

      [Agent Microsoft](/windows/win32/lwef/microsoft-agent)

      [Kit de développement logiciel (SDK) Microsoft Speech application (SASDK) version 1,0](https://www.microsoft.com/download/details.aspx?id=2200)

      [API Microsoft Speech (SAPI) 5,3](/previous-versions/windows/desktop/ms723627(v=vs.85))

      [API Microsoft Speech (SAPI) 5,4](/previous-versions/windows/desktop/ee125663(v=vs.85))

      [Contrôle de reconnaissance Reconnaissance vocale Bing](/previous-versions/bing/speech/dn434583(v=msdn.10))
   :::column-end:::
:::row-end:::

## <a name="samples"></a>exemples

Téléchargez et exécutez des exemples Windows complets qui illustrent diverses fonctionnalités d’accessibilité.

:::row:::
   :::column:::
      [Navigateur d’exemples de code](/samples/browse/?term=speech)

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