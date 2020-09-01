---
title: Connecter des appareils par le biais de sessions à distance
description: Créez des expériences partagées sur plusieurs appareils en les rejoignant dans une session à distance.
ms.assetid: 1c8dba9f-c933-4e85-829e-13ad784dd3e2
ms.date: 06/28/2017
ms.topic: article
keywords: Windows 10, UWP, appareils connectés, systèmes distants, Rome, projet Rome
ms.localizationpriority: medium
ms.openlocfilehash: f88f44d26c3a6f4971422074e855ffca53935c7f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164823"
---
# <a name="connect-devices-through-remote-sessions"></a>Connecter des appareils par le biais de sessions à distance

La fonctionnalité sessions à distance permet à une application de se connecter à d’autres appareils par le biais d’une session, soit pour la messagerie d’application explicite, soit pour l’échange répartie de données gérées par le système, telles que le **[SpatialEntityStore](/uwp/api/windows.perception.spatial.spatialentitystore)** pour le partage holographique entre des appareils Windows holographiques.

Les sessions à distance peuvent être créées par n’importe quel appareil Windows, et tout appareil Windows peut demander à rejoindre (même si les sessions peuvent avoir une visibilité invitation uniquement), y compris les appareils connectés par d’autres utilisateurs. Ce guide fournit un exemple de code de base pour tous les principaux scénarios qui utilisent des sessions à distance. Ce code peut être incorporé dans un projet d’application existant et modifié en fonction des besoins. Pour une implémentation de bout en bout, consultez l' [exemple d’application de jeu quiz](https://github.com/microsoft/Windows-appsample-remote-system-sessions).

## <a name="preliminary-setup"></a>Installation préliminaire

### <a name="add-the-remotesystem-capability"></a>Ajouter la fonctionnalité remoteSystem

Pour que votre application puisse lancer une application sur un appareil distant, vous devez ajouter la fonctionnalité `remoteSystem` dans le manifeste de votre package d’application. Vous pouvez utiliser le concepteur de manifeste de package pour l’ajouter en sélectionnant **système distant** sous l’onglet **fonctionnalités** , ou vous pouvez ajouter manuellement la ligne suivante au fichier _Package. appxmanifest_ de votre projet.

``` xml
<Capabilities>
   <uap3:Capability Name="remoteSystem"/>
</Capabilities>
```

### <a name="enable-cross-user-discovering-on-the-device"></a>Activer la découverte des utilisateurs croisés sur l’appareil
Les sessions à distance sont destinées à la connexion de plusieurs utilisateurs différents. par conséquent, les appareils concernés doivent avoir le partage entre utilisateurs activé. Il s’agit d’un paramètre système qui peut être interrogé à l’aide d’une méthode statique dans la classe **RemoteSystem** :

```csharp
if (!RemoteSystem.IsAuthorizationKindEnabled(RemoteSystemAuthorizationKind.Anonymous)) {
    // The system is not authorized to connect to cross-user devices. 
    // Inform the user that they can discover more devices if they
    // update the setting to "Everyone nearby".
}
```

Pour modifier ce paramètre, l’utilisateur doit ouvrir l’application **paramètres** . Dans le **System**menu Shared  >  **Experiences**Shared from  >  **Devices** , il existe une zone de liste déroulante dans laquelle l’utilisateur peut spécifier les appareils avec lesquels il peut partager le système.

![page des paramètres des expériences partagées](images/shared-experiences-settings.png)

### <a name="include-the-necessary-namespaces"></a>Inclure les espaces de noms nécessaires
Pour utiliser tous les extraits de code de ce guide, vous aurez besoin des `using` instructions suivantes dans votre ou vos fichiers de classe.

```csharp
using System.Runtime.Serialization.Json;
using Windows.Foundation.Collections;
using Windows.System.RemoteSystems;
```

## <a name="create-a-remote-session"></a>Créer une session à distance

Pour créer une instance de session distante, vous devez commencer par un objet **[RemoteSystemSessionController](/uwp/api/windows.system.remotesystems.remotesystemsessioncontroller)** . Utilisez l’infrastructure suivante pour créer une nouvelle session et gérer les demandes de jointure d’autres appareils.

```csharp
public async void CreateSession() {
    
    // create a session controller
    RemoteSystemSessionController manager = new RemoteSystemSessionController("Bob’s Minecraft game");
    
    // register the following code to handle the JoinRequested event
    manager.JoinRequested += async (sender, args) => {
        // Get the deferral
        var deferral = args.GetDeferral();
        
        // display the participant (args.JoinRequest.Participant) on UI, giving the 
        // user an opportunity to respond
        // ...
        
        // If the user chooses "accept", accept this remote system as a participant
        args.JoinRequest.Accept();
    };
    
    // create and start the session
    RemoteSystemSessionCreationResult createResult = await manager.CreateSessionAsync();
    
    // handle the creation result
    if (createResult.Status == RemoteSystemSessionCreationStatus.Success) {
        // creation was successful, get a reference to the session
        RemoteSystemSession currentSession = createResult.Session;
        
        // optionally subscribe to the disconnection event
        currentSession.Disconnected += async (sender, args) => {
            // update the UI, using args.Reason
            //...
        };
    
        // Use session (see later section)
        //...
    
    } else if (createResult.Status == RemoteSystemSessionCreationStatus.SessionLimitsExceeded) {
        // creation failed. Optionally update UI to indicate that there are too many sessions in progress
    } else {
        // creation failed for an unknown reason. Optionally update UI
    }
}
```

### <a name="make-a-remote-session-invite-only"></a>Créer une invitation de session à distance uniquement

Si vous souhaitez que votre session à distance reste publiquement détectable, vous pouvez l’inviter uniquement. Seuls les appareils qui reçoivent une invitation peuvent envoyer des demandes de jointure. 

La procédure est essentiellement la même que ci-dessus, mais lors de la construction de l’instance **[RemoteSystemSessionController](/uwp/api/windows.system.remotesystems.remotesystemsessioncontroller)** , vous transmettez un objet **[RemoteSystemSessionOptions](/uwp/api/windows.system.remotesystems.RemoteSystemSessionOptions)** configuré.

```csharp
// define the session options with the invite-only designation
RemoteSystemSessionOptions sessionOptions = new RemoteSystemSessionOptions();
sessionOptions.IsInviteOnly = true;

// create the session controller
RemoteSystemSessionController manager = new RemoteSystemSessionController("Bob's Minecraft game", sessionOptions);

//...
```

Pour envoyer une invitation, vous devez disposer d’une référence au système distant récepteur (acquisition par le biais d’une découverte normale du système distant). Transmettez simplement cette référence dans la méthode **[SendInvitationAsync](/uwp/api/windows.system.remotesystems.remotesystemsession.sendinvitationasync)** de l’objet de session. Tous les participants d’une session ont une référence à la session à distance (voir la section suivante), de sorte que tout participant peut envoyer une invitation.

```csharp
// "currentSession" is a reference to a RemoteSystemSession.
// "guestSystem" is a previously discovered RemoteSystem instance
currentSession.SendInvitationAsync(guestSystem); 
```

## <a name="discover-and-join-a-remote-session"></a>Découvrir et rejoindre une session à distance

Le processus de découverte des sessions à distance est géré par la classe **[RemoteSystemSessionWatcher](/uwp/api/windows.system.remotesystems.remotesystemsessionwatcher)** et est semblable à la découverte de systèmes distants individuels.

```csharp
public void DiscoverSessions() {
    
    // create a watcher for remote system sessions
    RemoteSystemSessionWatcher sessionWatcher = RemoteSystemSession.CreateWatcher();
    
    // register a handler for the "added" event
    sessionWatcher.Added += async (sender, args) => {
        
        // get a reference to the info about the discovered session
        RemoteSystemSessionInfo sessionInfo = args.SessionInfo;
        
        // Optionally update the UI with the sessionInfo.DisplayName and 
        // sessionInfo.ControllerDisplayName strings. 
        // Save a reference to this RemoteSystemSessionInfo to use when the
        // user selects this session from the UI
        //...
    };
    
    // Begin watching
    sessionWatcher.Start();
}
```

Lorsqu’une instance **[RemoteSystemSessionInfo](/uwp/api/windows.system.remotesystems.remotesystemsessioninfo)** est obtenue, elle peut être utilisée pour émettre une demande de jointure à l’appareil qui contrôle la session correspondante. Une demande de jointure acceptée retourne de manière asynchrone un objet **[RemoteSystemSessionJoinResult](/uwp/api/windows.system.remotesystems.remotesystemsessionjoinresult)** qui contient une référence à la session jointe.

```csharp
public async void JoinSession(RemoteSystemSessionInfo sessionInfo) {

    // issue a join request and wait for result.
    RemoteSystemSessionJoinResult joinResult = await sessionInfo.JoinAsync();
    if (joinResult.Status == RemoteSystemSessionJoinStatus.Success) {
        // Join request was approved

        // RemoteSystemSession instance "currentSession" was declared at class level.
        // Assign the value obtained from the join result.
        currentSession = joinResult.Session;
        
        // note connection and register to handle disconnection event
        bool isConnected = true;
        currentSession.Disconnected += async (sender, args) => {
            isConnected = false;

            // update the UI with args.Reason value
        };
        
        if (isConnected) {
            // optionally use the session here (see next section)
            //...
        }
    } else {
        // Join was unsuccessful.
        // Update the UI, using joinResult.Status value to show cause of failure.
    }
}
```

Un appareil peut être joint à plusieurs sessions en même temps. Pour cette raison, il peut être souhaitable de séparer la fonctionnalité de jointure de l’interaction réelle avec chaque session. Tant qu’une référence à l’instance **[RemoteSystemSession](/uwp/api/windows.system.remotesystems.remotesystemsession)** est conservée dans l’application, la communication peut être tentée sur cette session.

## <a name="share-messages-and-data-through-a-remote-session"></a>Partager des messages et des données par le biais d’une session à distance

### <a name="receive-messages"></a>Recevoir des messages

Vous pouvez échanger des messages et des données avec d’autres appareils participants dans la session à l’aide d’une instance **[RemoteSystemSessionMessageChannel](/uwp/api/windows.system.remotesystems.remotesystemsessionmessagechannel)** , qui représente un canal de communication unique à l’ensemble de la session. Dès qu’elle est initialisée, elle commence à écouter les messages entrants.

>[!NOTE]
>Les messages doivent être sérialisés et désérialisés à partir des tableaux d’octets lors de l’envoi et de la réception. Cette fonctionnalité est incluse dans les exemples suivants, mais elle peut être implémentée séparément pour une meilleure modularité du code. Pour obtenir un exemple, consultez l' [exemple d’application](https://github.com/microsoft/Windows-appsample-remote-system-sessions)).

```csharp
public async void StartReceivingMessages() {
    
    // Initialize. The channel name must be known by all participant devices 
    // that will communicate over it.
    RemoteSystemSessionMessageChannel messageChannel = new RemoteSystemSessionMessageChannel(currentSession, 
        "Everyone in Bob's Minecraft game", 
        RemoteSystemSessionMessageChannelReliability.Reliable);
    
    // write the handler for incoming messages on this channel
    messageChannel.ValueSetReceived += async (sender, args) => {
        
        // Update UI: a message was received from the participant args.Sender
        
        // Deserialize the message 
        // (this app must know what key to use and what object type the value is expected to be)
        ValueSet receivedMessage = args.Message;
        object rawData = receivedMessage["appKey"]);
        object value = new ExpectedType(); // this must be whatever type is expected

        using (var stream = new MemoryStream((byte[])rawData)) {
            value = new DataContractJsonSerializer(value.GetType()).ReadObject(stream);
        }
        
        // do something with the "value" object
        //...
    };
}
```

### <a name="send-messages"></a>Envoyer des messages

Lorsque le canal est établi, l’envoi d’un message à tous les participants de session est simple.

```csharp
public async void SendMessageToAllParticipantsAsync(RemoteSystemSessionMessageChannel messageChannel, object value){

    // define a ValueSet message to send
    ValueSet message = new ValueSet();
    
    // serialize the "value" object to send
    using (var stream = new MemoryStream()){
        new DataContractJsonSerializer(value.GetType()).WriteObject(stream, value);
        byte[] rawData = stream.ToArray();
            message["appKey"] = rawData;
    }
    
    // Send message to all participants. Ordering is not guaranteed.
    await messageChannel.BroadcastValueSetAsync(message);
}
```

Pour envoyer un message uniquement à certains participants, vous devez commencer par lancer un processus de découverte afin d’obtenir des références aux systèmes distants participant à la session. Cela est similaire au processus de découverte de systèmes distants en dehors d’une session. Utilisez une instance **[RemoteSystemSessionParticipantWatcher](/uwp/api/windows.system.remotesystems.remotesystemsessionparticipantwatcher)** pour rechercher les appareils participants d’une session.

```csharp
public void WatchForParticipants() {
    // "currentSession" is a reference to a RemoteSystemSession.
    RemoteSystemSessionParticipantWatcher watcher = currentSession.CreateParticipantWatcher();

    watcher.Added += (sender, participant) => {
        // save a reference to "participant"
        // optionally update UI
    };   

    watcher.Removed += (sender, participant) => {
        // remove reference to "participant"
        // optionally update UI
    };

    watcher.EnumerationCompleted += (sender, args) => {
        // Apps can delay data model render up until this point if they wish.
    };

    // Begin watching for session participants
    watcher.Start();
}
```

Quand une liste de références à des participants de session est obtenue, vous pouvez envoyer un message à n’importe quel ensemble d’entre eux.

Pour envoyer un message à un seul participant (idéalement sélectionné à l’écran par l’utilisateur), transmettez simplement la référence dans une méthode comme suit.

```csharp
public async void SendMessageToParticipantAsync(RemoteSystemSessionMessageChannel messageChannel, RemoteSystemSessionParticipant participant, object value) {
    
    // define a ValueSet message to send
    ValueSet message = new ValueSet();
    
    // serialize the "value" object to send
    using (var stream = new MemoryStream()){
        new DataContractJsonSerializer(value.GetType()).WriteObject(stream, value);
        byte[] rawData = stream.ToArray();
            message["appKey"] = rawData;
    }

    // Send message to the participant
    await messageChannel.SendValueSetAsync(message,participant);
}
```

Pour envoyer un message à plusieurs participants (idéalement sélectionnés sur l’écran par l’utilisateur), ajoutez-les à un objet de liste, puis transmettez la liste dans une méthode similaire à la suivante.

```csharp
public async void SendMessageToListAsync(RemoteSystemSessionMessageChannel messageChannel, IReadOnlyList<RemoteSystemSessionParticipant> myTeam, object value){

    // define a ValueSet message to send
    ValueSet message = new ValueSet();
    
    // serialize the "value" object to send
    using (var stream = new MemoryStream()){
        new DataContractJsonSerializer(value.GetType()).WriteObject(stream, value);
        byte[] rawData = stream.ToArray();
            message["appKey"] = rawData;
    }

    // Send message to specific participants. Ordering is not guaranteed.
    await messageChannel.SendValueSetToParticipantsAsync(message, myTeam);   
}
```

## <a name="related-topics"></a>Rubriques connexes
* [Applications et appareils connectés (projet Rome)](connected-apps-and-devices.md)
* [Référence sur l’API Systèmes distants](/uwp/api/Windows.System.RemoteSystems)