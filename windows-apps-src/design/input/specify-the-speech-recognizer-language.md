---
description: Découvrez comment sélectionner une langue installée à utiliser pour la reconnaissance vocale.
title: Spécifier la langue de reconnaissance vocale
ms.assetid: 4C463A1B-AF6A-46FD-A839-5D6724955B38
label: Specify the speech recognizer language
template: detail.hbs
keywords: voix, vocal, reconnaissance vocale, langage naturel, dictée, saisie, interaction utilisateur
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fa8137b5bf05bb8099a803fedd7e056fc14d9d70
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031322"
---
# <a name="specify-the-speech-recognizer-language"></a>Spécifier la langue de reconnaissance vocale


Découvrez comment sélectionner une langue installée à utiliser pour la reconnaissance vocale.

> **API importantes** : [**SupportedTopicLanguages**](/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedtopiclanguages), [**SupportedGrammarLanguages**](/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedgrammarlanguages), [**Language**](/uwp/api/Windows.Globalization.Language)


Nous avons répertorié les langues installées sur un système. Identifiez la langue par défaut et sélectionnez une autre langue pour la reconnaissance.

**Configuration requise :**

Cette rubrique s’appuie sur l’article [Reconnaissance vocale](speech-recognition.md).

Vous devez posséder des connaissances de base sur la reconnaissance vocale et ses contraintes.

Si vous débutez dans le développement d’applications Windows, consultez ces rubriques pour vous familiariser avec les technologies abordées ici.

-   [Créer votre première application](../../get-started/your-first-app.md)
-   Découvrir les événements avec [Vue d’ensemble des événements et des événements routés](../../xaml-platform/events-and-routed-events-overview.md).

**Instructions relatives à l’expérience utilisateur :**

Pour obtenir de précieux conseils concernant la conception d’une application dotée de fonctions vocales à la fois utile et conviviale, voir [Recommandations en matière de conception](./speech-interactions.md).

## <a name="identify-the-default-language"></a>Identifiez la langue par défaut.


La reconnaissance vocale utilise la langue du système en tant que langue de reconnaissance par défaut. Cette langue est définie par l’utilisateur sur l’écran Paramètres &gt; Système &gt; Parole &gt; Langue vocale de l’appareil.

Nous identifions la langue par défaut en vérifiant la propriété statique [**SystemSpeechLanguage**](/uwp/api/windows.media.speechrecognition.speechrecognizer.systemspeechlanguage) .

```CSharp
var language = SpeechRecognizer.SystemSpeechLanguage; 
```

## <a name="confirm-an-installed-language"></a>Confirmer une langue installée


Les langues installées peuvent varier entre les appareils. Vérifiez l’existence d’une langue avant de vous en servir pour une contrainte particulière.

**Remarque**  Un redémarrage est nécessaire après l’installation d’un nouveau module linguistique. Une exception avec le code d’erreur SPERR \_ \_ introuvable (0x8004503a) est levée si la langue spécifiée n’est pas prise en charge ou n’a pas terminé l’installation de.

 

Déterminez les langues prises en charge sur un appareil en vérifiant l’une des deux propriétés statiques de la classe [**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) :

-   [**SupportedTopicLanguages**](/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedtopiclanguages): collection d’objets de [**langage**](/uwp/api/Windows.Globalization.Language) utilisée avec les grammaires prédéfinies et les grammaires de recherche Web.

-   [**supportedGrammarLanguages**](/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedgrammarlanguages) : collection des objets [**Language**](/uwp/api/Windows.Globalization.Language) utilisés avec une contrainte de liste ou un fichier SRGS (Speech Recognition Grammar Specification).

## <a name="specify-a-language"></a>Spécifier une langue


Pour spécifier un langage, transmettez un objet de [**langage**](/uwp/api/Windows.Globalization.Language) dans le constructeur [**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) .

Ici, nous spécifions « en-US » comme langue de reconnaissance.


```CSharp
var language = new Windows.Globalization.Language("en-US"); 
var recognizer = new SpeechRecognizer(language); 
```

## <a name="remarks"></a>Remarques


Une contrainte de rubrique peut être configurée en ajoutant une [**SpeechRecognitionTopicConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint) à la collection [**Constraints**](/uwp/api/windows.media.speechrecognition.speechrecognizer.constraints) de la [**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer), puis en appelant [**CompileConstraintsAsync**](/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync). Un [**speechRecognitionResultStatus**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus) de **TopicLanguageNotSupported** est renvoyé si le module de reconnaissance n’est pas initialisé avec une langue de rubrique prise en charge.

Une contrainte de liste peut être configurée en ajoutant une [**SpeechRecognitionListConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint) à la collection [**Constraints**](/uwp/api/windows.media.speechrecognition.speechrecognizer.constraints) de la [**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer), puis en appelant [**CompileConstraintsAsync**](/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync). Vous ne pouvez pas spécifier directement la langue d’une liste personnalisée. La liste est traitée à l’aide de la langue du module de reconnaissance.

Une grammaire SRGS est un format XML standard ouvert représenté par la classe [**SpeechRecognitionGrammarFileConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint). Contrairement aux listes personnalisées, vous pouvez spécifier la langue de la grammaire dans le balisage SRGS. [**CompileConstraintsAsync**](/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync) échoue avec un [**SpeechRecognitionResultStatus**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus) de **TopicLanguageNotSupported** si le module de reconnaissance n’est pas initialisé pour la même langue que le balisage SRGS.

## <a name="related-articles"></a>Articles connexes

* [Interactions vocales](speech-interactions.md)

**Exemples**

* [Reconnaissance vocale et exemple de synthèse vocale](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)
 

 
