---
ms.assetid: 9146212C-8480-4C16-B74C-D7F08C7086AF
description: Cet article montre comment énumérer des périphériques MIDI et envoyer et recevoir des messages MIDI à partir d’une application Windows universelle.
title: MIDI
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 26026a0aadb52de27f98dea8a7c21bdc2d1c44f1
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363852"
---
# <a name="midi"></a>MIDI



Cet article montre comment énumérer des périphériques MIDI et envoyer et recevoir des messages MIDI à partir d’une application Windows universelle. Windows 10 prend en charge MIDI sur USB (pilotes conformes à la classe et les plus propriétaires), MIDI sur Bluetooth LE (édition anniversaire Windows 10 et versions ultérieures) et les produits tiers disponibles gratuitement, MIDI sur Ethernet et MIDI routé.

## <a name="enumerate-midi-devices"></a>Énumérer des périphériques MIDI

Avant d’énumérer et d’utiliser des périphériques MIDI, ajoutez les espaces de noms suivants à votre projet.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MainPage.xaml.cs" id="SnippetUsing":::

Ajoutez un contrôle [**ListBox**](/uwp/api/Windows.UI.Xaml.Controls.ListBox) à votre page XAML qui permettra à l’utilisateur de sélectionner l’un des périphériques d’entrée MIDI attachés au système. Ajoutez-en un autre pour répertorier les périphériques de sortie MIDI.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MainPage.xaml" id="SnippetMidiListBoxes":::

La classe [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) de la méthode [**FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) est utilisée pour énumérer de nombreux types différents d’appareils reconnus par Windows. Pour spécifier que la méthode doit uniquement Rechercher les périphériques d’entrée MIDI, utilisez la chaîne de sélecteur retournée par [**MidiInPort. GetDeviceSelector**](/uwp/api/windows.devices.midi.midiinport.getdeviceselector). **FindAllAsync** renvoie une classe [**DeviceInformationCollection**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationCollection) qui contient une classe **DeviceInformation** pour chaque périphérique d’entrée MIDI enregistré auprès du système. Si la collection renvoyée ne contient aucun élément, cela signifie qu’il n’y a aucun périphérique d’entrée MIDI disponible. Si la collection comporte des éléments, parcourez les objets **DeviceInformation** et ajoutez le nom de chaque périphérique au périphérique d’entrée MIDI **ListBox**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MainPage.xaml.cs" id="SnippetEnumerateMidiInputDevices":::

L’énumération des appareils de sortie MIDI fonctionne exactement comme l’énumération des appareils d’entrée, excepté le fait que vous devez spécifier la chaîne de sélecteur renvoyée par [**MidiOutPort.GetDeviceSelector**](/uwp/api/windows.devices.midi.midioutport.getdeviceselector) lors de l’appel de **FindAllAsync**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MainPage.xaml.cs" id="SnippetEnumerateMidiOutputDevices":::



## <a name="create-a-device-watcher-helper-class"></a>Créer une classe d’assistance d’observateur de périphériques

L’espace de noms [**Windows. Devices. Enumeration**](/uwp/api/Windows.Devices.Enumeration) fournit le [**DeviceWatcher**](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) qui peut notifier votre application si des appareils sont ajoutés ou supprimés du système, ou si les informations d’un appareil sont mises à jour. Étant donné que les applications compatibles MIDI sont généralement intéressées par les périphériques d’entrée et de sortie, cet exemple crée une classe d’assistance qui implémente le modèle **DeviceWatcher**, afin que le même code puisse être utilisé sur ces deux types de périphériques sans avoir besoin d’être dupliqué.

Ajoutez une nouvelle classe à votre projet utilisée comme observateur de périphériques. Dans cet exemple, la classe est nommée **MyMidiDeviceWatcher**. Le reste du code de cette section est utilisé pour implémenter la classe d’assistance.

Ajoutez des variables de membre à la classe :

-   Un objet [**DeviceWatcher**](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) qui surveille les changements de périphérique.
-   Une chaîne de sélecteur de périphérique qui contiendra la chaîne de sélecteur de port d’entrée MIDI pour une instance et la chaîne de sélecteur de port de sortie MIDI pour une autre instance.
-   Un contrôle [**ListBox**](/uwp/api/Windows.UI.Xaml.Controls.ListBox) qui comportera les noms des périphériques disponibles.
-   Une classe [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) requise pour mettre à jour l’interface utilisateur à partir d’un thread autre que le thread d’interface utilisateur.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MyMidiDeviceWatcher.cs" id="SnippetWatcherVariables":::

Ajoutez une propriété [**DeviceInformationCollection**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationCollection) utilisée pour accéder à la liste actuelle des périphériques depuis l’extérieur de la classe d’assistance.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MyMidiDeviceWatcher.cs" id="SnippetDeclareDeviceInformationCollection":::

Dans le constructeur de classe, l’appelant transmet la chaîne de sélecteur de périphérique MIDI, l’élément **ListBox** nécessaire pour répertorier les périphériques et l’élément **Dispatcher** requis pour la mise à jour de l’interface utilisateur.

Appelez [**DeviceInformation.CreateWatcher**](/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher) pour créer une nouvelle instance de la classe **DeviceWatcher**, en transmettant la chaîne de sélecteur de périphérique MIDI.

Enregistrez des gestionnaires pour les gestionnaires d’événements de l’observateur.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MyMidiDeviceWatcher.cs" id="SnippetWatcherConstructor":::

L’élément **DeviceWatcher** présente les événements suivants :

-   [**Added**](/uwp/api/windows.devices.enumeration.devicewatcher.added) - Déclenché lorsqu’un nouveau périphérique est ajouté au système.
-   [**Removed**](/uwp/api/windows.devices.enumeration.devicewatcher.removed) - Déclenché lorsqu’un périphérique est supprimé du système.
-   [**Mise à jour**](/uwp/api/windows.devices.enumeration.devicewatcher.updated) : déclenché lorsque les informations associées à un appareil existant sont mises à jour.
-   [**EnumerationCompleted**](/uwp/api/windows.devices.enumeration.devicewatcher.enumerationcompleted) - Déclenché lorsque l’observateur a terminé son énumération du type de périphérique demandé.

Dans le gestionnaire d’événements, pour chacun de ces événements, une méthode d’assistance **UpdateDevices** est appelée pour mettre à jour l’élément **ListBox** en tenant compte de la liste actuelle des périphériques. Dans la mesure où **UpdateDevices** met à jour les éléments d’interface utilisateur et que ces gestionnaires d’événements ne sont pas appelés sur le thread d’interface utilisateur, chaque appel doit être encapsulé dans un appel à [**RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync), ce qui entraîne l’exécution du code sur le thread d’interface utilisateur.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MyMidiDeviceWatcher.cs" id="SnippetWatcherEventHandlers":::

La méthode d’assistance **UpdateDevices** appelle [**DeviceInformation.FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) et met à jour l’élément **ListBox** en tenant compte des noms des périphériques renvoyés comme décrit précédemment dans cet article.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MyMidiDeviceWatcher.cs" id="SnippetWatcherUpdateDevices":::

Ajoutez des méthodes pour démarrer l’observateur, à l’aide de la méthode [**Start**](/uwp/api/windows.devices.enumeration.devicewatcher.start) de l’objet **DeviceWatcher** et pour arrêter l’observateur à l’aide de la méthode [**Stop**](/uwp/api/windows.devices.enumeration.devicewatcher.stop).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MyMidiDeviceWatcher.cs" id="SnippetWatcherStopStart":::

Fournissez un destructeur pour annuler l’enregistrement des gestionnaires d’événements de l’observateur et définir l’observateur de périphériques sur null.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MyMidiDeviceWatcher.cs" id="SnippetWatcherDestructor":::

## <a name="create-midi-ports-to-send-and-receive-messages"></a>Créer des ports MIDI pour envoyer et recevoir des messages

Dans le code-behind de votre page, déclarez des variables de membre destinées à contenir deux instances de la classe d’assistance **MyMidiDeviceWatcher**, une pour les périphériques d’entrée et une autre pour les périphériques de sortie.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MainPage.xaml.cs" id="SnippetDeclareDeviceWatchers":::

Créez une instance des classes d’assistance de l’observateur en transmettant la chaîne de sélecteur de périphérique, le **ListBox** à remplir et l’objet **CoreDispatcher** accessible par le biais de la propriété **Dispatcher** de la page. Appelez ensuite la méthode pour démarrer l’élément **DeviceWatcher** de chaque objet.

Peu après avoir démarré chaque **DeviceWatcher**, elle terminera l’énumération des périphériques actuels connectés au système et générera son événement [**EnumerationCompleted**](/uwp/api/windows.devices.enumeration.devicewatcher.enumerationcompleted), qui entraînera la mise à jour de chaque **ListBox** avec les périphériques MIDI en cours.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MainPage.xaml.cs" id="SnippetStartWatchers":::

La sélection par l’utilisateur d’un élément dans l’élément **ListBox** de l’entrée MIDI, déclenche l’événement [**SelectionChanged**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged). Dans le gestionnaire, pour cet événement, accédez à la propriété **DeviceInformationCollection** de la classe d’assistance pour obtenir la liste des périphériques actuels. Si la liste comporte des entrées, sélectionnez l’objet **DeviceInformation** avec l’index correspondant à la propriété [**SelectedIndex**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex) du contrôle **ListBox**.

Créez l’objet [**MidiInPort**](/uwp/api/Windows.Devices.Midi.MidiInPort) représentant le périphérique d’entrée sélectionné en appelant [**MidiInPort.FromIdAsync**](/uwp/api/windows.devices.midi.midiinport.fromidasync) et en transmettant la propriété [**Id**](/uwp/api/windows.devices.enumeration.deviceinformation.id) du périphérique sélectionné.

Inscrire un gestionnaire pour l’événement [**MessageReceived**](/uwp/api/windows.devices.midi.midiinport.messagereceived) , qui est déclenché chaque fois qu’un message MIDI est reçu via l’appareil spécifié.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MainPage.xaml.cs" id="SnippetDeclareMidiPorts":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MainPage.xaml.cs" id="SnippetInPortSelectionChanged":::

Lorsque le gestionnaire **MessageReceived** est appelé, le message est contenu dans la propriété [**Message**](/uwp/api/Windows.Devices.Midi.MidiMessageReceivedEventArgs) de la classe **MidiMessageReceivedEventArgs**. Le [**type**](/uwp/api/windows.devices.midi.imidimessage.type) de l’objet message est une valeur de l’énumération [**MidiMessageType**](/uwp/api/Windows.Devices.Midi.MidiMessageType) indiquant le type de message qui a été reçu. Les données du message dépendent de son type. Cet exemple vérifie si le message est une note figurant sur un message et, si tel est le cas, génère le canal midi, la note et la vitesse du message.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MainPage.xaml.cs" id="SnippetMessageReceived":::

Le gestionnaire [**SelectionChanged**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) de l’élément **ListBox** du périphérique de sortie fonctionne comme le gestionnaire des périphériques d’entrée, excepté qu’aucun gestionnaire d’événements n’est enregistré.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MainPage.xaml.cs" id="SnippetOutPortSelectionChanged":::

Une fois le périphérique de sortie créé, vous pouvez envoyer un message en créant un nouveau [**IMidiMessage**](/uwp/api/Windows.Devices.Midi.IMidiMessage) pour le type de message que vous souhaitez envoyer. Dans cet exemple, le message est un [**NoteOnMessage**](/uwp/api/Windows.Devices.Midi.MidiNoteOnMessage). La méthode [**SendMessage**](/uwp/api/windows.devices.midi.imidioutport.sendmessage) de l’objet [**IMidiOutPort**](/uwp/api/Windows.Devices.Midi.IMidiOutPort) est appelée pour envoyer le message.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MainPage.xaml.cs" id="SnippetSendMessage":::

Lorsque votre application est désactivée, veillez à nettoyer les ressources de vos applications. Annulez l’enregistrement de vos gestionnaires d’événements et définissez les objets des ports MIDI d’entrée et de sortie sur null. Arrêtez les observateurs de périphériques et affectez-leur la valeur null.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MIDIWin10/cs/MainPage.xaml.cs" id="SnippetCleanUp":::

## <a name="using-the-built-in-windows-general-midi-synth"></a>Utilisation du synthétiseur General MIDI Windows intégré

Lors de l’énumération des périphériques de sortie MIDI à l’aide de la technique décrite ci-dessus, votre application détecte un périphérique MIDI appelé « Synthé. de table de sons Microsoft GS ». Il s’agit d’un synthétiseur General MIDI intégré que vous pouvez utiliser à partir de votre application. Toutefois, toute tentative de création d’un port de sortie MIDI pour ce périphérique échouera, sauf si vous avez inclus l’extension du Kit de développement logiciel (SDK) pour le synthétiseur intégré dans votre projet.

**Pour inclure l’extension du Kit de développement logiciel (SDK) pour le synthétiseur General MIDI dans votre projet d’application**

1.  Dans l’**Explorateur de solutions**, sous votre projet, cliquez avec le bouton droit sur **Références**, puis sélectionnez **Ajouter une référence…**.
2.  Développez le nœud **Windows universel**.
3.  Sélectionnez **Extensions**.
4.  Dans la liste des extensions, sélectionnez **DLS General MIDI Microsoft pour les applications Windows universelles**.
    > [!NOTE] 
    > S’il existe plusieurs versions de l’extension, veillez à sélectionner la version qui correspond à la cible de votre application. Pour connaître la version du Kit de développement logiciel (SDK) ciblée par votre application, cliquez sur l’onglet **Application** des propriétés du projet.

 

 
