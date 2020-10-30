---
description: La reconnaissance vocale permet de fournir une saisie vocale, de spécifier une action ou une commande et d’accomplir différentes tâches.
title: Reconnaissance vocale
ms.assetid: 553C0FB7-35BC-4894-9EF1-906139E17552
label: Speech recognition
template: detail.hbs
keywords: voix, vocal, reconnaissance vocale, langage naturel, dictée, saisie, interaction utilisateur
ms.date: 10/25/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ad721bc64de87fc8bb1a56f687860738bebed56c
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030052"
---
# <a name="speech-recognition"></a>Reconnaissance vocale


La reconnaissance vocale permet de fournir une saisie vocale, de spécifier une action ou une commande et d’accomplir différentes tâches.

> **API importantes** : [ **Windows. Media. SpeechRecognition**](/uwp/api/Windows.Media.SpeechRecognition)

La reconnaissance vocale se compose d’un runtime de fonctions vocales, d’API de reconnaissance vocale pour programmer le runtime, de grammaires prêtes à l’emploi pour la dictée et la recherche web, ainsi que d’une interface utilisateur système par défaut permettant aux utilisateurs de découvrir et d’utiliser les fonctionnalités de reconnaissance vocale.

## <a name="configure-speech-recognition"></a>Configurer la reconnaissance vocale

Pour prendre en charge la reconnaissance vocale avec votre application, l’utilisateur doit se connecter et activer un microphone sur son appareil, et accepter la politique de confidentialité Microsoft accordant à votre application l’autorisation de l’utiliser.

Pour demander automatiquement à l’utilisateur une boîte de dialogue système qui demande l’autorisation d’accéder et d’utiliser le flux audio du microphone (à partir de l’exemple de [reconnaissance vocale et de synthèse vocale](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis) illustrée ci-dessous), il vous suffit de définir la [fonctionnalité de périphérique](/uwp/schemas/appxpackage/appxmanifestschema/element-devicecapability) du **microphone** dans le manifeste du [package d’application](/uwp/schemas/appxpackage/appx-package-manifest). Pour plus d’informations, consultez [déclarations de fonctionnalités d’application](../../packaging/app-capability-declarations.md).

![Politique de confidentialité pour l’accès au microphone](images/speech/privacy.png)

Si l’utilisateur clique sur Oui pour accorder l’accès au microphone, votre application est ajoutée à la liste des applications approuvées dans la page Paramètres-> confidentialité-> microphone. Toutefois, étant donné que l’utilisateur peut choisir de désactiver ce paramètre à tout moment, vous devez vérifier que votre application a accès au microphone avant de tenter de l’utiliser.

Si vous souhaitez également prendre en charge la dictée, Cortana ou d’autres services de reconnaissance vocale (par exemple, une [grammaire prédéfinie](#predefined-grammars) définie dans une contrainte de rubrique), vous devez également vérifier que la **reconnaissance vocale en ligne** (paramètres-> confidentialité-> Speech) est activée.

Cet extrait de code montre comment votre application peut vérifier si un microphone est présent et s’il a l’autorisation de l’utiliser.

```csharp
public class AudioCapturePermissions
{
    // If no microphone is present, an exception is thrown with the following HResult value.
    private static int NoCaptureDevicesHResult = -1072845856;

    /// <summary>
    /// Note that this method only checks the Settings->Privacy->Microphone setting, it does not handle
    /// the Cortana/Dictation privacy check.
    ///
    /// You should perform this check every time the app gets focus, in case the user has changed
    /// the setting while the app was suspended or not in focus.
    /// </summary>
    /// <returns>True, if the microphone is available.</returns>
    public async static Task<bool> RequestMicrophonePermission()
    {
        try
        {
            // Request access to the audio capture device.
            MediaCaptureInitializationSettings settings = new MediaCaptureInitializationSettings();
            settings.StreamingCaptureMode = StreamingCaptureMode.Audio;
            settings.MediaCategory = MediaCategory.Speech;
            MediaCapture capture = new MediaCapture();

            await capture.InitializeAsync(settings);
        }
        catch (TypeLoadException)
        {
            // Thrown when a media player is not available.
            var messageDialog = new Windows.UI.Popups.MessageDialog("Media player components are unavailable.");
            await messageDialog.ShowAsync();
            return false;
        }
        catch (UnauthorizedAccessException)
        {
            // Thrown when permission to use the audio capture device is denied.
            // If this occurs, show an error or disable recognition functionality.
            return false;
        }
        catch (Exception exception)
        {
            // Thrown when an audio capture device is not present.
            if (exception.HResult == NoCaptureDevicesHResult)
            {
                var messageDialog = new Windows.UI.Popups.MessageDialog("No Audio Capture devices are present on this system.");
                await messageDialog.ShowAsync();
                return false;
            }
            else
            {
                throw;
            }
        }
        return true;
    }
}
```

```cpp
/// <summary>
/// Note that this method only checks the Settings->Privacy->Microphone setting, it does not handle
/// the Cortana/Dictation privacy check.
///
/// You should perform this check every time the app gets focus, in case the user has changed
/// the setting while the app was suspended or not in focus.
/// </summary>
/// <returns>True, if the microphone is available.</returns>
IAsyncOperation<bool>^  AudioCapturePermissions::RequestMicrophonePermissionAsync()
{
    return create_async([]() 
    {
        try
        {
            // Request access to the audio capture device.
            MediaCaptureInitializationSettings^ settings = ref new MediaCaptureInitializationSettings();
            settings->StreamingCaptureMode = StreamingCaptureMode::Audio;
            settings->MediaCategory = MediaCategory::Speech;
            MediaCapture^ capture = ref new MediaCapture();

            return create_task(capture->InitializeAsync(settings))
                .then([](task<void> previousTask) -> bool
            {
                try
                {
                    previousTask.get();
                }
                catch (AccessDeniedException^)
                {
                    // Thrown when permission to use the audio capture device is denied.
                    // If this occurs, show an error or disable recognition functionality.
                    return false;
                }
                catch (Exception^ exception)
                {
                    // Thrown when an audio capture device is not present.
                    if (exception->HResult == AudioCapturePermissions::NoCaptureDevicesHResult)
                    {
                        auto messageDialog = ref new Windows::UI::Popups::MessageDialog("No Audio Capture devices are present on this system.");
                        create_task(messageDialog->ShowAsync());
                        return false;
                    }

                    throw;
                }
                return true;
            });
        }
        catch (Platform::ClassNotRegisteredException^ ex)
        {
            // Thrown when a media player is not available. 
            auto messageDialog = ref new Windows::UI::Popups::MessageDialog("Media Player Components unavailable.");
            create_task(messageDialog->ShowAsync());
            return create_task([] {return false; });
        }
    });
}
```

```js
var AudioCapturePermissions = WinJS.Class.define(
    function () { }, {},
    {
        requestMicrophonePermission: function () {
            /// <summary>
            /// Note that this method only checks the Settings->Privacy->Microphone setting, it does not handle
            /// the Cortana/Dictation privacy check.
            ///
            /// You should perform this check every time the app gets focus, in case the user has changed
            /// the setting while the app was suspended or not in focus.
            /// </summary>
            /// <returns>True, if the microphone is available.</returns>
            return new WinJS.Promise(function (completed, error) {

                try {
                    // Request access to the audio capture device.
                    var captureSettings = new Windows.Media.Capture.MediaCaptureInitializationSettings();
                    captureSettings.streamingCaptureMode = Windows.Media.Capture.StreamingCaptureMode.audio;
                    captureSettings.mediaCategory = Windows.Media.Capture.MediaCategory.speech;

                    var capture = new Windows.Media.Capture.MediaCapture();
                    capture.initializeAsync(captureSettings).then(function () {
                        completed(true);
                    },
                    function (error) {
                        // Audio Capture can fail to initialize if there's no audio devices on the system, or if
                        // the user has disabled permission to access the microphone in the Privacy settings.
                        if (error.number == -2147024891) { // Access denied (microphone disabled in settings)
                            completed(false);
                        } else if (error.number == -1072845856) { // No recording device present.
                            var messageDialog = new Windows.UI.Popups.MessageDialog("No Audio Capture devices are present on this system.");
                            messageDialog.showAsync();
                            completed(false);
                        } else {
                            error(error);
                        }
                    });
                } catch (exception) {
                    if (exception.number == -2147221164) { // REGDB_E_CLASSNOTREG
                        var messageDialog = new Windows.UI.Popups.MessageDialog("Media Player components not available on this system.");
                        messageDialog.showAsync();
                        return false;
                    }
                }
            });
        }
    })
```

## <a name="recognize-speech-input"></a>Reconnaître la saisie vocale

Une *contrainte* définit les mots et expressions (vocabulaire) qu’une application reconnaît dans la saisie vocale. Les contraintes sont au cœur de la reconnaissance vocale et offrent à votre application un meilleur contrôle de la précision de la reconnaissance vocale.

Vous pouvez utiliser les types de contraintes suivants pour la reconnaissance de l’entrée vocale.

### <a name="predefined-grammars"></a>Grammaires prédéfinies

Les grammaires de dictée et de recherche web prédéfinies vous permettent d’activer la reconnaissance vocale dans votre application sans créer votre propre grammaire. Si vous optez pour ces grammaires, la reconnaissance vocale est effectuée par un service web distant qui renvoie les résultats à l’appareil.

La grammaire de dictée de texte libre par défaut peut reconnaître la plupart des mots et expressions prononcés par un utilisateur dans une langue spécifique. Elle est optimisée pour reconnaître les expressions courtes. La syntaxe de dictée prédéfinie est utilisée si vous ne spécifiez aucune contrainte pour votre objet [**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) . La dictée de texte libre est utile si vous ne souhaitez pas limiter ce qu’un utilisateur peut dire. Elle est généralement utilisée pour créer des notes ou dicter le contenu d’un message.

La grammaire de recherche web, comme une grammaire de dictée, contient un grand nombre de mots et expressions qu’un utilisateur peut dire. Toutefois, elle est optimisée pour reconnaître les termes que les personnes utilisent généralement lors des recherches sur le web.

> [!NOTE]
> Étant donné que les grammaires de dictée et de recherche web prédéfinies peuvent être volumineuses et qu’elles sont hébergées en ligne (elles ne se trouvent pas sur l’appareil), les performances obtenues peuvent ne pas être aussi bonnes qu’avec des grammaires personnalisées qui sont installées sur l’appareil.     

Ces grammaires prédéfinies peuvent être utilisées pour reconnaître jusqu’à 10 secondes de saisie vocale et ne nécessitent aucun effort de création de votre part. Toutefois, elles requièrent une connexion à un réseau.

Pour utiliser les contraintes de service Web, l’entrée vocale et la prise en charge de la dictée doivent être activées dans les **paramètres** en activant l’option « obtenir un savoir-faire » dans  **paramètres-> la confidentialité > la reconnaissance vocale, l’entrée manuscrite et la saisie** .

Ici, nous montrons comment tester si les entrées vocales sont activées et ouvrir la page Paramètres-> confidentialité-> la parole, l’entrée manuscrite et la frappe, si ce n’est pas le cas.

Nous commençons par initialiser une variable globale (HResultPrivacyStatementDeclined) sur la valeur HResult de 0x80045509. Consultez [gestion des exceptions pour dans C \# ou Visual Basic](/previous-versions/windows/apps/dn532194(v=win.10)).

```csharp
private static uint HResultPrivacyStatementDeclined = 0x80045509;
```

Nous interceptons ensuite toutes les exceptions standard pendant recogntion et testons si la valeur [**HRESULT**](/uwp/api/Windows.Foundation.HResult) est égale à la valeur de la variable HResultPrivacyStatementDeclined. Si c’est le cas, nous affichons un avertissement et appelons `await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-accounts"));` pour ouvrir la page des paramètres.

```csharp
catch (Exception exception)
{
  // Handle the speech privacy policy error.
  if ((uint)exception.HResult == HResultPrivacyStatementDeclined)
  {
    resultTextBlock.Visibility = Visibility.Visible;
    resultTextBlock.Text = "The privacy statement was declined." + 
      "Go to Settings -> Privacy -> Speech, inking and typing, and ensure you" +
      "have viewed the privacy policy, and 'Get To Know You' is enabled.";
    // Open the privacy/speech, inking, and typing settings page.
    await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-accounts")); 
  }
  else
  {
    var messageDialog = new Windows.UI.Popups.MessageDialog(exception.Message, "Exception");
    await messageDialog.ShowAsync();
  }
}
```

Consultez [**SpeechRecognitionTopicConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint).

### <a name="programmatic-list-constraints"></a>Contraintes de liste de programmation 

Les contraintes de liste de programmation permettent de créer facilement une grammaire simple sous la forme d’une liste de mots ou d’expressions. Une contrainte de liste fonctionne correctement pour la reconnaissance d’expressions distinctes courtes. En indiquant explicitement des mots dans une grammaire, vous améliorez également la précision de la reconnaissance, car le traitement de la parole par le moteur de reconnaissance se limite à la confirmation d’une correspondance. La liste peut également être mise à jour par programme.

Une contrainte de liste se compose d’un tableau de chaînes qui correspondent à la saisie vocale acceptée par votre application en vue d’une opération de reconnaissance vocale. Pour créer une contrainte de liste dans votre application, vous devez créer un objet de contrainte de liste de reconnaissance vocale et lui passer un tableau de chaînes. Vous devez ensuite ajouter cet objet à la collection de contraintes du moteur de reconnaissance vocale. La reconnaissance vocale fonctionne quand le moteur de reconnaissance vocale reconnaît l’une des chaînes du tableau.

Consultez [**SpeechRecognitionListConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint).

### <a name="srgs-grammars"></a>Grammaires SRGS

Contrairement à une contrainte de liste de programmation, une grammaire SRGS (Speech Recognition Grammar Specification) est un document statique au format XML défini par la norme [SRGS version 1.0](https://www.w3.org/TR/speech-grammar/). Une grammaire SRGS permet de contrôler au maximum l’expérience de la reconnaissance vocale en capturant plusieurs significations sémantiques dans une même reconnaissance.

 Consultez [**SpeechRecognitionGrammarFileConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint).

### <a name="voice-command-constraints"></a>Contraintes de commande vocale

Utilisez un fichier XML VCD (Voice Command Definition) pour définir les commandes que l’utilisateur peut prononcer pour lancer des actions au moment de l’activation de votre application. Pour plus d’informations, consultez [activer une application de premier plan avec des commandes vocales via Cortana](/cortana/voice-commands/launch-a-foreground-app-with-voice-commands-in-cortana).

Voir [ **SpeechRecognitionVoiceCommandDefinitionConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionVoiceCommandDefinitionConstraint)/

**Remarque**  Le type de contrainte que vous utilisez dépend de la complexité de l’expérience de reconnaissance que vous souhaitez créer. Un type de contrainte peut être mieux adapté à une tâche de reconnaissance vocale particulière, mais vous pouvez aussi combiner tous les types de contrainte dans votre application.
Pour apprendre à utiliser des contraintes, voir [Définir des contraintes de reconnaissance vocale personnalisées](define-custom-recognition-constraints.md).

La grammaire de dictée prédéfinie d’une application Windows universelle reconnaît la plupart des mots et des expressions courtes dans une langue donnée. Elle est activée par défaut lorsqu’un objet du moteur de reconnaissance vocale est instancié sans contraintes personnalisées.

Dans cet exemple, nous montrons comment effectuer les opérations suivantes :

- créer un moteur de reconnaissance vocale ;
- compiler les contraintes de l’application Windows universelle par défaut (aucune grammaire n’a été ajoutée à l’ensemble de grammaires du moteur de reconnaissance vocale) ;
- Commencez à écouter la parole à l’aide de l’interface utilisateur de reconnaissance de base et des commentaires TTS fournis par la méthode [**RecognizeWithUIAsync**](/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizewithuiasync) . Utilisez la méthode [**RecognizeAsync**](/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizeasync) si l’interface utilisateur par défaut n’est pas requise.

```CSharp
private async void StartRecognizing_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // Compile the dictation grammar by default.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

## <a name="customize-the-recognition-ui"></a>Personnaliser l’interface utilisateur de reconnaissance vocale


Lorsque votre application tente de reconnaître la reconnaissance vocale en appelant [**SpeechRecognizer. RecognizeWithUIAsync**](/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizewithuiasync), plusieurs écrans sont affichés dans l’ordre suivant.

Si vous utilisez une contrainte basée sur une grammaire prédéfinie (dictée ou recherche web) :

-   Écran **Écoute**
-   Écran **Réflexion**
-   Écran **Nous vous avons entendu dire** ou écran de notification d’erreur

Si vous utilisez une contrainte basée sur une liste de mots ou d’expressions ou une contrainte basée sur un fichier de grammaire SRGS :

-   Écran **Écoute**
-   Écran **Nous vous avons entendu dire** , si l’interprétation de ce que l’utilisateur a prononcé pourrait avoir plusieurs résultats éventuels
-   Écran **Nous vous avons entendu dire** ou écran de notification d’erreur

L’image suivante présente un exemple du flux entre des écrans d’un moteur de reconnaissance vocale qui utilise une contrainte basée sur un fichier de grammaire SGRS. Dans cet exemple, la reconnaissance vocale a réussi.

![Écran initial de la reconnaissance vocale correspondant à une contrainte basée sur un fichier de grammaire SGRS](images/speech-listening-initial.png)

![Écran intermédiaire de la reconnaissance vocale correspondant à une contrainte basée sur un fichier de grammaire SGRS](images/speech-listening-intermediate.png)

![Écran final de la reconnaissance vocale correspondant à une contrainte basée sur un fichier de grammaire SGRS](images/speech-listening-complete.png)

L’écran **Listening** peut fournir des exemples de mot ou d’expression que l’application peut reconnaître. Nous montrons ici comment utiliser les propriétés de la classe [**SpeechRecognizerUIOptions**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizerUIOptions) (obtenue en appelant la propriété [**SpeechRecognizer.UIOptions**](/uwp/api/windows.media.speechrecognition.speechrecognizer.uioptions)) pour personnaliser le contenu de l’écran **Listening** .

```CSharp
private async void WeatherSearch_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // Listen for audio input issues.
    speechRecognizer.RecognitionQualityDegrading += speechRecognizer_RecognitionQualityDegrading;

    // Add a web search grammar to the recognizer.
    var webSearchGrammar = new Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint(Windows.Media.SpeechRecognition.SpeechRecognitionScenario.WebSearch, "webSearch");


    speechRecognizer.UIOptions.AudiblePrompt = "Say what you want to search for...";
    speechRecognizer.UIOptions.ExampleText = @"Ex. 'weather for London'";
    speechRecognizer.Constraints.Add(webSearchGrammar);

    // Compile the constraint.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();
    //await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

## <a name="related-articles"></a>Articles connexes

* [Interactions vocales](speech-interactions.md)

**Exemples**

* [Reconnaissance vocale et exemple de synthèse vocale](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)
