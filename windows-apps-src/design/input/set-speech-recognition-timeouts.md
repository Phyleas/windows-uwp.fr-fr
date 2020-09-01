---
Description: Définissez la durée pendant laquelle un moteur de reconnaissance vocale ignore les silences ou les sons incompréhensibles (brouhaha) et continue à écouter la saisie vocale.
title: Définir des délais d’expiration de reconnaissance vocale
ms.assetid: 58F446AC-4A56-454D-8125-62A2C4DBFCC8
label: Speech recognition timeouts
template: detail.hbs
keywords: voix, vocal, reconnaissance vocale, langage naturel, dictée, saisie, interaction utilisateur
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c68b8aeb71ed4269b3a7fc52c6b616a8b0760b0b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173323"
---
# <a name="set-speech-recognition-timeouts"></a>Définir des délais d’expiration de reconnaissance vocale


Définissez la durée pendant laquelle un moteur de reconnaissance vocale ignore les silences ou les sons incompréhensibles (brouhaha) et continue à écouter la saisie vocale.

> **API importantes**: [**délais d’attente**](/uwp/api/windows.media.speechrecognition.speechrecognizer.timeouts), [**SpeechRecognizerTimeouts**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizerTimeouts)

## <a name="set-a-timeout"></a>Définir un délai d’expiration


Ici, nous spécifions plusieurs valeurs de [**délai d’attente**](/uwp/api/windows.media.speechrecognition.speechrecognizer.timeouts) :

-   InitialSilenceTimeout : durée pendant laquelle un SpeechRecognizer détecte du silence (avant que les résultats de la reconnaissance vocale aient été générés) avant de supposer qu’aucune saisie vocale ne va être effectuée.
-   BabbleTimeout : durée pendant laquelle un SpeechRecognizer continue à écouter les sons incompréhensibles (brouhaha) avant de supposer que la saisie vocale est terminée et de mettre fin à l’opération de reconnaissance vocale.
-   EndSilenceTimeout : durée pendant laquelle un SpeechRecognizer détecte du silence (après que les résultats de la reconnaissance vocale ont été générés) avant de supposer que la saisie vocale est terminée.

**Remarque**    Les délais d’attente peuvent être définis sur une base par reconnaissance.

 

```CSharp
// Set timeout settings.
recognizer.Timeouts.InitialSilenceTimeout = TimeSpan.FromSeconds(6.0);
recognizer.Timeouts.BabbleTimeout = TimeSpan.FromSeconds(4.0);
recognizer.Timeouts.EndSilenceTimeout = TimeSpan.FromSeconds(1.2);
```

## <a name="related-articles"></a>Articles connexes

* [Interactions vocales](speech-interactions.md)

**Exemples**

* [Reconnaissance vocale et exemple de synthèse vocale](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)