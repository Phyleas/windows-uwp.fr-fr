---
ms.assetid: ''
description: Découvrez comment capturer des vidéos et des captures d’écran de jeu, et comment envoyer des métadonnées incorporées par le système dans des médias capturés et de diffusion.
title: Capturer l’audio, la vidéo, les captures d’écran et les métadonnées d’un jeu
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, jeu, capture, audio, vidéo, métadonnées
ms.localizationpriority: medium
ms.openlocfilehash: ea01139d4945d1e1b7e9a49078cd93725388e946
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364142"
---
# <a name="capture-game-audio-video-screenshots-and-metadata"></a>Capturer l’audio, la vidéo, les captures d’écran et les métadonnées d’un jeu
Cet article explique comment capturer des vidéos et des captures d’écran de jeu, et comment envoyer des métadonnées incorporées par le système dans des médias capturés et diffusés, ce qui permet à votre application et à d’autres de créer des expériences dynamiques qui sont synchronisées avec les événements de jeu. 

Il existe deux façons de capturer un jeu de jeu dans une application UWP. L’utilisateur peut lancer la capture à l’aide de l’interface utilisateur système intégrée. Les supports capturés à l’aide de cette technique sont ingérés dans l’écosystème de jeux Microsoft, peuvent être affichés et partagés via des expériences internes telles que l’application Xbox et ne sont pas directement disponibles pour votre application ou pour les utilisateurs. Les premières sections de cet article vous expliquent comment activer et désactiver la capture d’application implémentée par le système et comment recevoir des notifications lorsque la capture de l’application démarre ou s’arrête.

L’autre façon de capturer des médias consiste à utiliser les API de l’espace de noms **[Windows. Media. AppRecording](/uwp/api/windows.media.apprecording)** . Si la capture est activée sur l’appareil, votre application peut commencer à capturer le jeu, puis, après un certain temps, vous pouvez arrêter la capture, auquel cas le média est écrit dans un fichier. Si l’utilisateur a activé la capture d’historique, vous pouvez également enregistrer le jeu qui s’est déjà produit en spécifiant une heure de début dans le passé et une durée à enregistrer. Ces deux techniques produisent un fichier vidéo accessible par votre application, et selon l’emplacement où vous avez choisi d’enregistrer les fichiers, par l’utilisateur. Les sections du milieu de cet article vous guident tout au long de la implémentation de ces scénarios.

L’espace de noms **[Windows. Media. capture](/uwp/api/windows.media.capture)** fournit des API pour créer des métadonnées qui décrivent le jeu qui est capturé ou diffusé. Cela peut inclure du texte ou des valeurs numériques, avec une étiquette de texte qui identifie chaque élément de données. Les métadonnées peuvent représenter un « événement » qui se produit à un moment donné, par exemple lorsque l’utilisateur termine un tour dans un jeu de course, ou qu’il peut représenter un « État » qui persiste sur une période de temps, telle que la carte de jeu actuelle dans laquelle l’utilisateur est joué. Les métadonnées sont écrites dans un cache qui est alloué et géré pour votre application par le système. Les métadonnées sont incorporées dans des flux de diffusion et des fichiers vidéo capturés, y compris les techniques intégrées de capture de système ou de capture d’application personnalisée. Les dernières sections de cet article vous montrent comment écrire des métadonnées de jeu.

> [!NOTE] 
> Étant donné que les métadonnées de jeu peuvent être incorporées dans des fichiers multimédias qui peuvent potentiellement être partagés sur le réseau, en dehors du contrôle de l’utilisateur, vous ne devez pas inclure d’informations d’identification personnelle ou d’autres données potentiellement sensibles dans les métadonnées.


## <a name="enable-and-disable-system-app-capture"></a>Activer et désactiver la capture d’applications système
La capture d’application système est lancée par l’utilisateur avec l’interface utilisateur système intégrée. Les fichiers sont ingérés par l’écosystème de jeux Windows et ne sont pas accessibles à votre application ou à l’utilisateur, à l’exception des premières expériences telles que l’application Xbox. Votre application peut désactiver et activer la capture d’application initiée par le système, ce qui vous permet d’empêcher l’utilisateur de capturer un contenu ou un jeu de données. 

Pour activer ou désactiver la capture d’application système, appelez simplement la méthode statique **[AppCapture. SetAllowedAsync](/uwp/api/windows.media.capture.appcapture.setallowedasync)** et passez **false** pour désactiver la capture ou **true** pour activer la capture.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetSetAppCaptureAllowed":::


## <a name="receive-notifications-when-system-app-capture-starts-and-stops"></a>Recevoir des notifications lorsque la capture de l’application système démarre et s’arrête
Pour recevoir une notification lorsque la capture de l’application système commence ou se termine, commencez par obtenir une instance de la classe **[AppCapture](/uwp/api/windows.media.capture.appcapture)** en appelant la méthode de fabrique **[GetForCurrentView](/uwp/api/windows.media.capture.appcapture.GetForCurrentView)**. Ensuite, inscrivez un gestionnaire pour l’événement **[CapturingChanged](/uwp/api/windows.media.capture.appcapture.CapturingChanged)** .

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetRegisterCapturingChanged":::

Dans le gestionnaire de l’événement **CapturingChanged** , vous pouvez vérifier les propriétés **[IsCapturingAudio](/uwp/api/windows.media.capture.appcapture.IsCapturingAudio)** et **[IsCapturingVideo](/uwp/api/windows.media.capture.appcapture.IsCapturingVideo)** pour déterminer si l’audio ou la vidéo est capturée, respectivement. Vous souhaiterez peut-être mettre à jour l’interface utilisateur de votre application pour indiquer l’état de capture actuel.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetOnCapturingChanged":::

## <a name="add-the-windows-desktop-extensions-for-the-uwp-to-your-app"></a>Ajouter les extensions de bureau Windows pour la série UWP à votre application
Les API d’enregistrement audio et vidéo et de capture de captures d’écran directement à partir de votre application, qui se trouvent dans l’espace de noms **[Windows. Media. AppRecording](/uwp/api/windows.media.apprecording)** , ne sont pas incluses dans le contrat d’API universel. Pour accéder aux API, vous devez ajouter une référence aux extensions de bureau Windows pour la série UWP à votre application en suivant les étapes ci-dessous.

1. Dans Visual Studio, dans **Explorateur de solutions**, développez votre projet UWP et cliquez avec le bouton droit sur **références** , puis sélectionnez **Ajouter une référence...**. 
2. Développez le nœud **Windows universel** et sélectionnez **Extensions**.
3. Dans la liste des extensions, cochez la case en regard des **extensions du bureau Windows pour l’entrée UWP** correspondant à la build cible de votre projet. Pour les fonctionnalités de diffusion d’application, la version doit être supérieure ou égale à 1709.
4. Cliquez sur **OK**.

## <a name="get-an-instance-of-apprecordingmanager"></a>Obtient une instance de AppRecordingManager
La classe **[AppRecordingManager](/uwp/api/windows.media.apprecording.apprecordingmanager)** est l’API centrale que vous allez utiliser pour gérer l’enregistrement de l’application. Récupérez une instance de cette classe en appelant la méthode de fabrique **[GetDefault](/uwp/api/windows.media.apprecording.apprecordingmanager.GetDefault)**. Avant d’utiliser les API de l’espace de noms **Windows. Media. AppRecording** , vous devez vérifier leur présence sur l’appareil actuel. Les API ne sont pas disponibles sur les appareils exécutant une version du système d’exploitation antérieure à Windows 10, version 1709. Au lieu de vérifier une version spécifique du système d’exploitation, utilisez la méthode **[ApiInformation. IsApiContractPresent](/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** pour rechercher la version de *Windows. Media. AppBroadcasting. AppRecordingContract* version 1,0. Si ce contrat est présent, les API d’enregistrement sont disponibles sur l’appareil. L’exemple de code de cet article vérifie les API une seule fois, puis vérifie si la valeur de **AppRecordingManager** est null avant les opérations suivantes.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetGetAppRecordingManager":::

## <a name="determine-if-your-app-can-currently-record"></a>Déterminer si votre application peut enregistrer actuellement
Il existe plusieurs raisons pour lesquelles votre application peut ne pas être en mesure de capturer des données audio ou vidéo, notamment si l’appareil actuel ne répond pas à la configuration matérielle requise pour l’enregistrement ou si une autre application est en cours de diffusion. Avant de lancer un enregistrement, vous pouvez vérifier si votre application est actuellement en mesure de l’enregistrer. Appelez la méthode **[GetStatus](/uwp/api/windows.media.apprecording.apprecordingmanager.GetStatus)** de l’objet **AppRecordingManager** , puis vérifiez la propriété **[CanRecord](/uwp/api/windows.media.apprecording.apprecordingstatus.CanRecord)** de l’objet **[AppRecordingStatus](/uwp/api/windows.media.apprecording.apprecordingstatus)** retourné. Si **CanRecord** retourne **false**, ce qui signifie que votre application ne peut pas enregistrer actuellement, vous pouvez vérifier la propriété **[Details](/uwp/api/windows.media.apprecording.apprecordingstatus.Details)** pour déterminer la raison. Selon la raison, vous souhaiterez peut-être afficher l’état de l’utilisateur ou afficher les instructions d’activation de l’enregistrement de l’application.



:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetCanRecord":::

## <a name="manually-start-and-stop-recording-your-app-to-a-file"></a>Démarrer et arrêter manuellement l’enregistrement de votre application dans un fichier

Après avoir vérifié que votre application est en mesure d’enregistrer, vous pouvez démarrer un nouvel enregistrement en appelant la méthode **[StartRecordingToFileAsync](/uwp/api/windows.media.apprecording.apprecordingmanager.startrecordingtofileasync)** de l’objet **AppRecordingManager** .

Dans l’exemple suivant, le premier bloc **Then** s’exécute lorsque la tâche asynchrone échoue. Le deuxième bloc tente **ensuite** d’accéder au résultat de la tâche et, si le résultat est null, la tâche est terminée. Dans les deux cas, la méthode d’assistance **OnRecordingComplete** , illustrée ci-dessous, est appelée pour gérer le résultat. 

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetStartRecordToFile":::

Une fois l’opération d’enregistrement terminée, vérifiez la propriété **[Succeeded](/uwp/api/windows.media.apprecording.apprecordingresult.Succeeded)** de l’objet **[AppRecordingResult](/uwp/api/windows.media.apprecording.apprecordingresult)** retourné pour déterminer si l’opération d’enregistrement a réussi. Si c’est le cas, vous pouvez vérifier la propriété **[IsFileTruncated](/uwp/api/windows.media.apprecording.apprecordingresult.IsFileTruncated)** pour déterminer si, pour des raisons de stockage, le système a été contraint de tronquer le fichier capturé. Vous pouvez vérifier la propriété **[Duration](/uwp/api/windows.media.apprecording.apprecordingresult.Duration)** pour découvrir la durée réelle du fichier enregistré qui, si le fichier est tronqué, peut être plus petite que la durée de l’opération d’enregistrement.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetOnRecordingComplete":::

Les exemples suivants illustrent un code de base pour démarrer et arrêter l’opération d’enregistrement indiquée dans l’exemple précédent.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetCallStartRecordToFile":::

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetFinishRecordToFile":::

## <a name="record-a-historical-time-span-to-a-file"></a>Enregistrer un intervalle de temps historique dans un fichier
Si l’utilisateur a activé l’enregistrement de l’historique de votre application dans les paramètres système, vous pouvez enregistrer un laps de temps de jeu qui s’est déjà écoulé. Un exemple précédent de cet article A montré comment confirmer que votre application peut enregistrer actuellement des procédures de jeu. Il existe une vérification supplémentaire pour déterminer si l’historique de capture est activé. Une fois encore, appelez **[GetStatus](/uwp/api/windows.media.apprecording.apprecordingmanager.GetStatus)** et vérifiez la propriété **[CanRecordTimeSpan](/uwp/api/windows.media.apprecording.apprecordingstatus.CanRecordTimeSpan)** de l’objet **AppRecordingStatus** retourné. Cet exemple retourne également la propriété **[HistoricalBufferDuration](/uwp/api/windows.media.apprecording.apprecordingstatus.HistoricalBufferDuration)** de l' **AppRecordingStatus** qui sera utilisée pour déterminer une heure de début valide pour l’opération d’enregistrement.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetCanRecordTimeSpan":::

Pour capturer un intervalle de temps historique, vous devez spécifier une heure de début pour l’enregistrement et une durée. L’heure de début est fournie en tant que struct **[DateTime](/uwp/api/windows.foundation.datetime)** . L’heure de début doit être une heure antérieure à l’heure actuelle, dans la longueur de la mémoire tampon d’enregistrement historique. Pour cet exemple, la longueur de la mémoire tampon est récupérée dans le cadre du contrôle pour voir si l’enregistrement historique est activé, ce qui est indiqué dans l’exemple de code précédent. La durée de l’enregistrement historique est fournie sous forme de struct  **[TimeSpan](/uwp/api/windows.foundation.timespan)** , qui doit également être inférieure ou égale à la durée de la mémoire tampon d’historique. Une fois que vous avez déterminé l’heure de début et la durée de votre choix, appelez **[RecordTimeSpanToFileAsync](/uwp/api/windows.media.apprecording.apprecordingmanager.recordtimespantofileasync)** pour démarrer l’opération d’enregistrement.

Comme pour l’enregistrement avec un démarrage et un arrêt manuels, quand un enregistrement historique est terminé, vous pouvez vérifier la propriété **[Succeeded](/uwp/api/windows.media.apprecording.apprecordingresult.Succeeded)** de l’objet **[AppRecordingResult](/uwp/api/windows.media.apprecording.apprecordingresult)** retourné pour déterminer si l’opération d’enregistrement a réussi, et vous pouvez vérifier la propriété **[IsFileTruncated](/uwp/api/windows.media.apprecording.apprecordingresult.IsFileTruncated)** et **[Duration](/uwp/api/windows.media.apprecording.apprecordingresult.Duration)** pour découvrir la durée réelle du fichier enregistré qui, si le fichier est tronqué, peut être plus petite que la durée de la fenêtre de temps demandée.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetRecordTimeSpanToFile":::

L’exemple suivant montre un code de base pour initialiser l’opération d’enregistrement d’historique indiquée dans l’exemple précédent.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetCallRecordTimeSpanToFile":::

## <a name="save-screenshot-images-to-files"></a>Enregistrer les images de capture d’écran dans des fichiers
Votre application peut lancer une capture de capture d’écran qui permet d’enregistrer le contenu actuel de la fenêtre de l’application dans un fichier image ou dans plusieurs fichiers image avec des encodages d’image différents. Pour spécifier les encodages d’image que vous souhaitez utiliser, créez une liste de chaînes où chacune représente un type d’image. Les propriétés du **[ImageEncodingSubtypes](/uwp/api/windows.media.mediaproperties.mediaencodingsubtypes)** fournissent la chaîne correcte pour chaque type d’image pris en charge, par exemple **MediaEncodingSubtypes.Png** ou **MediaEncodingSubtypes. JpegXr**.

Lancez la capture d’écran en appelant la méthode **[SaveScreenshotToFilesAsync](/uwp/api/windows.media.apprecording.apprecordingmanager.savescreenshottofilesasync)** de l’objet **AppRecordingManager** . Le premier paramètre de cette méthode est un **StorageFolder** dans lequel les fichiers image seront enregistrés. Le deuxième paramètre est un préfixe de nom de fichier auquel le système ajoute l’extension pour chaque type d’image enregistré, par exemple « . png ».

Le troisième paramètre de **SaveScreenshotToFilesAsync** est nécessaire pour que le système puisse effectuer la conversion colorspace appropriée si la fenêtre en cours à capturer affiche le contenu HDR. Si le contenu HDR est présent, ce paramètre doit être défini sur **AppRecordingSaveScreenshotOption. HdrContentVisible**. Sinon, utilisez **AppRecordingSaveScreenshotOption. None**. Le dernier paramètre de la méthode est la liste des formats d’image sur lesquels l’écran doit être capturé.

Lorsque l’appel asynchrone à **SaveScreenshotToFilesAsync** se termine, il retourne un objet **[AppRecordingSavedScreenshotInfo](/uwp/api/windows.media.apprecording.apprecordingsavedscreenshotinfo)** qui fournit le **StorageFile** et la valeur **MediaEncodingSubtypes** associée indiquant le type d’image pour chaque image enregistrée.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetSaveScreenShotToFiles":::

L’exemple suivant montre un code de base pour initialiser l’opération de capture d’écran illustrée dans l’exemple précédent.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetCallSaveScreenShotToFiles":::

## <a name="add-game-metadata-for-system-and-app-initiated-capture"></a>Ajouter des métadonnées de jeu pour la capture initiée par le système et l’application
Les sections suivantes de cet article décrivent comment fournir des métadonnées incorporées par le système dans le flux MP4 de la capture ou du jeu de diffusion. Les métadonnées peuvent être incorporées dans un média capturé à l’aide de l’interface utilisateur et du média intégrés capturés par l’application avec **AppRecordingManager**. Ces métadonnées peuvent être extraites par votre application et d’autres applications pendant la lecture du média afin de fournir des expériences contextuelles qui sont synchronisées avec le jeu capturé ou diffusé.

### <a name="get-an-instance-of-appcapturemetadatawriter"></a>Obtient une instance de AppCaptureMetadataWriter
La classe principale pour la gestion des métadonnées de capture d’application est **[AppCaptureMetadataWriter](/uwp/api/windows.media.capture.appcapturemetadatawriter)**. Avant d’initialiser une instance de cette classe, utilisez la méthode **[ApiInformation. IsApiContractPresent](/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** pour rechercher la version 1,0 de *Windows. Media. capture. AppCaptureMetadataContract* afin de vérifier que l’API est disponible sur l’appareil actuel.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetGetMetadataWriter":::

### <a name="write-metadata-to-the-system-cache-for-your-app"></a>Écrire des métadonnées dans le cache système de votre application
Chaque élément de métadonnées a une étiquette de chaîne, identifiant l’élément de métadonnées, une valeur de données associée qui peut être une chaîne, un entier ou une valeur double, et une valeur de l’énumération **[AppCaptureMetadataPriority](/uwp/api/windows.media.capture.appcapturemetadatapriority)** indiquant la priorité relative de l’élément de données. Un élément de métadonnées peut être considéré comme un « événement », qui se produit à un point unique dans le temps, ou un « État » qui maintient une valeur sur une fenêtre de temps. Les métadonnées sont écrites dans un cache mémoire qui est alloué et géré pour votre application par le système. Le système applique une limite de taille sur le cache mémoire des métadonnées et, lorsque la limite est atteinte, purge les données en fonction de la priorité avec laquelle chaque élément de métadonnées a été écrit. La section suivante de cet article explique comment gérer l’allocation de mémoire de métadonnées de votre application.

Une application classique peut choisir d’écrire des métadonnées au début de la session de capture pour fournir un contexte pour les données suivantes. Pour ce scénario, il est recommandé d’utiliser les données instantanées « événement ». Cet exemple appelle **[AddStringEvent](/uwp/api/windows.media.capture.appcapturemetadatawriter.addstringevent)**, **[AddDoubleEvent](/uwp/api/windows.media.capture.appcapturemetadatawriter.adddoubleevent)** et **[AddInt32Event](/uwp/api/windows.media.capture.appcapturemetadatawriter.addint32event)** pour définir des valeurs instantanées pour chaque type de données.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetStartSession":::

Un scénario courant pour l’utilisation de données d’État qui persistent au fil du temps consiste à effectuer le suivi de la carte de jeu dans laquelle se trouve actuellement le joueur. Cet exemple appelle **[StartStringState](/uwp/api/windows.media.capture.appcapturemetadatawriter.startstringstate)** pour définir la valeur d’État. 

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetStartMap":::

Appelez **[StopState](/uwp/api/windows.media.capture.appcapturemetadatawriter.stopstate)** pour enregistrer qu’un état particulier s’est terminé.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetEndMap":::

Vous pouvez remplacer un État en définissant une nouvelle valeur avec une étiquette d’État existante.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetLevelUp":::

Vous pouvez terminer tous les États actuellement ouverts en appelant **[StopAllStates](/uwp/api/windows.media.capture.appcapturemetadatawriter.StopAllStates)**.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetRaceComplete":::

### <a name="manage-metadata-cache-storage-limit"></a>Gérer la limite de stockage du cache des métadonnées
Les métadonnées que vous écrivez avec **AppCaptureMetadataWriter** sont mises en cache par le système jusqu’à ce qu’elles soient écrites dans le flux multimédia associé. Le système définit une limite de taille pour le cache des métadonnées de chaque application. Une fois la limite de taille du cache atteinte, le système commence à purger les métadonnées mises en cache. Le système supprimera les métadonnées écrites avec la valeur de priorité **[AppCaptureMetadataPriority. informatif](/uwp/api/windows.media.capture.appcapturemetadatapriority)** avant de supprimer les métadonnées avec la priorité **[AppCaptureMetadataPriority. important](/uwp/api/windows.media.capture.appcapturemetadatapriority)** .

À tout moment, vous pouvez vérifier le nombre d’octets disponibles dans le cache de métadonnées de votre application en appelant **[RemainingStorageBytesAvailable](/uwp/api/windows.media.capture.appcapturemetadatawriter.RemainingStorageBytesAvailable)**. Vous pouvez choisir de définir votre propre seuil défini par l’application après lequel vous pouvez choisir de réduire la quantité de métadonnées que vous écrivez dans le cache. L’exemple suivant illustre une implémentation simple de ce modèle.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetCheckMetadataStorage":::

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetComboExecuted":::

### <a name="receive-notifications-when-the-system-purges-metadata"></a>Recevoir des notifications lorsque le système purge les métadonnées
Vous pouvez vous inscrire pour recevoir une notification lorsque le système commence à purger les métadonnées de votre application en inscrivant un gestionnaire pour l’événement **[MetadataPurged](/uwp/api/windows.media.capture.appcapturemetadatawriter.MetadataPurged)** .

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetRegisterMetadataPurged":::

Dans le gestionnaire de l’événement **MetadataPurged** , vous pouvez libérer de la place dans le cache des métadonnées en terminant les États de priorité inférieure, vous pouvez implémenter une logique définie par l’application pour réduire la quantité de métadonnées que vous écrivez dans le cache, ou vous pouvez faire en sorte que le système continue de vider le cache en fonction de la priorité avec laquelle il a été écrit.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetOnMetadataPurged":::

## <a name="related-topics"></a>Rubriques connexes

* [Jeux](index.md)
 

 
