---
title: Serveur du GATT Bluetooth
description: Cet article fournit une vue d’ensemble du serveur de profil d’attribut générique Bluetooth (GATT) pour les applications de plateforme Windows universelle (UWP), ainsi que des exemples de code pour les cas d’utilisation courants.
ms.date: 06/26/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b4cf26d4f4fe5fa33f9f214da32263031188c5f0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172253"
---
# <a name="bluetooth-gatt-server"></a>Serveur du GATT Bluetooth

Cet article présente les API de serveur de l’attribut générique Bluetooth (GATT) pour les applications de plateforme Windows universelle (UWP), ainsi que des exemples de code pour les tâches courantes du serveur GATT :

- Définir les services pris en charge
- Publier le serveur afin qu’il puisse être découvert par les clients distants
- Publier la prise en charge du service
- Répondre aux demandes de lecture et d’écriture
- Envoyer des notifications à des clients abonnés

> [!Important]
> Vous devez déclarer la fonctionnalité « Bluetooth » dans *Package. appxmanifest*.
>
> `<Capabilities> <DeviceCapability Name="bluetooth" /> </Capabilities>`

**API importantes**

- [**Windows.Devices.Bluetooth**](/uwp/api/Windows.Devices.Bluetooth)
- [**Windows. Devices. Bluetooth. GenericAttributeProfile**](/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile)

## <a name="overview"></a>Vue d’ensemble

Windows fonctionne généralement dans le rôle client. Toutefois, de nombreux scénarios se produisent, qui nécessitent également que Windows fasse office de serveur du GATT Bluetooth LE. Presque tous les scénarios pour les appareils IoT, ainsi que la plupart des communications entre plateformes, nécessitent que Windows soit un serveur GATT. En outre, l’envoi de notifications à des appareils mobiles à proximité est devenu un scénario populaire qui requiert également cette technologie.  

Les opérations sur le serveur tournent autour du fournisseur de services et du GattLocalCharacteristic. Ces deux classes fournissent les fonctionnalités nécessaires à la déclaration, à l’implémentation et à l’exposition d’une hiérarchie de données à un appareil distant.

## <a name="define-the-supported-services"></a>Définir les services pris en charge
Votre application peut déclarer un ou plusieurs services qui seront publiés par Windows. Chaque service est identifié de manière unique par un UUID.

### <a name="attributes-and-uuids"></a>Attributs et UUID
Chaque service, caractéristique et descripteur est défini par son unique UUID 128 bits.
> Les API Windows utilisent toutes le terme GUID, mais la norme Bluetooth définit ces identificateurs comme UUID. Dans le cadre de nos objectifs, ces deux termes sont interchangeables, nous allons continuer à utiliser le terme UUID. 

Si l’attribut est standard et défini par le SIG Bluetooth, il aura également un ID abrégé de 16 bits correspondant (par exemple, l’UUID de niveau de batterie est 0000**2A19**-0000-1000-8000-00805F9B34FB et l’ID abrégé est 0x2A19). Ces UUID standard peuvent être consultés dans [GattServiceUuids](/uwp/api/windows.devices.bluetooth.genericattributeprofile.gattserviceuuids) et [GattCharacteristicUuids](/uwp/api/windows.devices.bluetooth.genericattributeprofile.gattcharacteristicuuids).

Si votre application implémente son propre service personnalisé, un UUID personnalisé devra être généré. Cette opération est facile dans Visual Studio via les outils-> CreateGuid (utilisez l’option 5 pour l’afficher dans le-... xxxxxxxx-xxxx xxxx»). Cet UUID peut maintenant être utilisé pour déclarer de nouveaux services locaux, caractéristiques ou descripteurs.

#### <a name="restricted-services"></a>Services restreints
Les services suivants sont réservés par le système et ne peuvent pas être publiés pour le moment :
1. Device information service (DIS)
2. Service de profil d’attribut générique (GATT)
3. Service de profil d’accès générique (GAP)
4. Service d’appareil de l’interface utilisateur (HOGP)
5. Service de paramètres d’analyse (SCP)

> Toute tentative de création d’un service bloqué entraîne le retour de BluetoothError. DisabledByPolicy à partir de l’appel à CreateAsync.

#### <a name="generated-attributes"></a>Attributs générés
Les descripteurs suivants sont générés automatiquement par le système, en fonction du GattLocalCharacteristicParameters fourni lors de la création de la caractéristique :
1. Configuration des caractéristiques du client (si la caractéristique est marquée comme indicatable ou notifiable).
2. Description de l’utilisateur caractéristique (si la propriété UserDescription est définie). Pour plus d’informations, consultez la propriété GattLocalCharacteristicParameters. UserDescription.
3. Format caractéristique (un descripteur pour chaque format de présentation spécifié).  Pour plus d’informations, consultez la propriété GattLocalCharacteristicParameters. PresentationFormats.
4. Format d’agrégation caractéristique (si plusieurs formats de présentation sont spécifiés).  GattLocalCharacteristicParameters. pour plus d’informations, consultez la propriété PresentationFormats.
5. Propriétés étendues caractéristiques (si la caractéristique est marquée avec le bit des propriétés étendues).

> La valeur du descripteur des propriétés étendues est déterminée via les propriétés de caractéristiques ReliableWrites et WritableAuxiliaries.

> Toute tentative de création d’un descripteur réservé entraîne une exception.

> Notez que la diffusion n’est pas prise en charge pour l’instant.  La spécification de la GattCharacteristicProperty de diffusion entraîne une exception.

### <a name="build-up-the-hierarchy-of-services-and-characteristics"></a>Développer la hiérarchie des services et caractéristiques
Le GattServiceProvider est utilisé pour créer et publier la définition du service principal racine.  Chaque service requiert son propre objet ServiceProvider qui accepte un GUID : 

```csharp
GattServiceProviderResult result = await GattServiceProvider.CreateAsync(uuid);

if (result.Error == BluetoothError.Success)
{
    serviceProvider = result.ServiceProvider;
    // 
}
```
> Les services principaux constituent le niveau supérieur de l’arborescence du GATT. Les services principaux contiennent des caractéristiques et d’autres services (appelés « inclus » ou services secondaires). 

À présent, renseignez le service avec les caractéristiques et les descripteurs requis :

```csharp
GattLocalCharacteristicResult characteristicResult = await serviceProvider.Service.CreateCharacteristicAsync(uuid1, ReadParameters);
if (characteristicResult.Error != BluetoothError.Success)
{
    // An error occurred.
    return;
}
_readCharacteristic = characteristicResult.Characteristic;
_readCharacteristic.ReadRequested += ReadCharacteristic_ReadRequested;

characteristicResult = await serviceProvider.Service.CreateCharacteristicAsync(uuid2, WriteParameters);
if (characteristicResult.Error != BluetoothError.Success)
{
    // An error occurred.
    return;
}
_writeCharacteristic = characteristicResult.Characteristic;
_writeCharacteristic.WriteRequested += WriteCharacteristic_WriteRequested;

characteristicResult = await serviceProvider.Service.CreateCharacteristicAsync(uuid3, NotifyParameters);
if (characteristicResult.Error != BluetoothError.Success)
{
    // An error occurred.
    return;
}
_notifyCharacteristic = characteristicResult.Characteristic;
_notifyCharacteristic.SubscribedClientsChanged += SubscribedClientsChanged;
```
Comme indiqué ci-dessus, il s’agit également d’un bon emplacement pour déclarer les gestionnaires d’événements pour les opérations prises en charge par chaque caractéristique.  Pour répondre correctement aux demandes, une application doit définir et définir un gestionnaire d’événements pour chaque type de demande pris en charge par l’attribut.  L’échec de l’inscription d’un gestionnaire entraînera la fin immédiate de la demande avec *UnlikelyError* par le système.

### <a name="constant-characteristics"></a>Caractéristiques constantes
Parfois, il existe des valeurs caractéristiques qui ne changent pas au cours de la durée de vie de l’application. Dans ce cas, il est recommandé de déclarer une caractéristique constante pour empêcher l’activation d’application inutile : 

```csharp
byte[] value = new byte[] {0x21};
var constantParameters = new GattLocalCharacteristicParameters
{
    CharacteristicProperties = (GattCharacteristicProperties.Read),
    StaticValue = value.AsBuffer(),
    ReadProtectionLevel = GattProtectionLevel.Plain,
};

var characteristicResult = await serviceProvider.Service.CreateCharacteristicAsync(uuid4, constantParameters);
if (characteristicResult.Error != BluetoothError.Success)
{
    // An error occurred.
    return;
}
```
## <a name="publish-the-service"></a>Publier le service
Une fois le service entièrement défini, l’étape suivante consiste à publier la prise en charge du service. Cela indique au système d’exploitation que le service doit être renvoyé lorsque des appareils distants effectuent une découverte de service.  Vous devez définir deux propriétés : IsDiscoverable et IsConnectable :  

```csharp
GattServiceProviderAdvertisingParameters advParameters = new GattServiceProviderAdvertisingParameters
{
    IsDiscoverable = true,
    IsConnectable = true
};
serviceProvider.StartAdvertising(advParameters);
```
- **IsDiscoverable**: publie le nom convivial sur les appareils distants dans la publication, ce qui rend l’appareil détectable.
- **IsConnectable**: publie une publication connectable à utiliser dans le rôle périphérique.

> Lorsqu’un service est à la fois détectable et connectable, le système ajoute l’UUID de service au paquet de publication.  Il n’y a que 31 octets dans le paquet de publication et un UUID 128 bits en occupe 16.

> Notez que lorsqu’un service est publié au premier plan, une application doit appeler StopAdvertising quand l’application s’interrompt.

## <a name="respond-to-read-and-write-requests"></a>Répondre aux demandes de lecture et d’écriture
Comme nous l’avons vu plus haut lors de la déclaration des caractéristiques requises, GattLocalCharacteristics a 3 types d’événements : ReadRequested, WriteRequested et SubscribedClientsChanged.

### <a name="read"></a>Lecture
Lorsqu’un appareil distant tente de lire une valeur à partir d’une caractéristique (et qu’il ne s’agit pas d’une valeur constante), l’événement ReadRequested est appelé. La caractéristique sur laquelle la lecture a été appelée, ainsi que les arguments (contenant des informations sur le périphérique distant) sont passés au délégué : 

```csharp
characteristic.ReadRequested += Characteristic_ReadRequested;
// ... 

async void ReadCharacteristic_ReadRequested(GattLocalCharacteristic sender, GattReadRequestedEventArgs args)
{
    var deferral = args.GetDeferral();
    
    // Our familiar friend - DataWriter.
    var writer = new DataWriter();
    // populate writer w/ some data. 
    // ... 

    var request = await args.GetRequestAsync();
    request.RespondWithValue(writer.DetachBuffer());
    
    deferral.Complete();
}
``` 

### <a name="write"></a>Write
Lorsqu’un appareil distant tente d’écrire une valeur dans une caractéristique, l’événement WriteRequested est appelé avec des détails sur le périphérique distant, caractéristique à laquelle écrire et la valeur elle-même : 

```csharp
characteristic.ReadRequested += Characteristic_ReadRequested;
// ...

async void WriteCharacteristic_WriteRequested(GattLocalCharacteristic sender, GattWriteRequestedEventArgs args)
{
    var deferral = args.GetDeferral();
    
    var request = await args.GetRequestAsync();
    var reader = DataReader.FromBuffer(request.Value);
    // Parse data as necessary. 

    if (request.Option == GattWriteOption.WriteWithResponse)
    {
        request.Respond();
    }
    
    deferral.Complete();
}
```
Il existe 2 types d’écritures, avec et sans réponse. Utilisez GattWriteOption (une propriété sur l’objet GattWriteRequest) pour déterminer le type d’écriture effectué par le périphérique distant. 

## <a name="send-notifications-to-subscribed-clients"></a>Envoyer des notifications à des clients abonnés
Les plus fréquentes des opérations du serveur GATT, les notifications exécutent la fonction critique de transmission de données aux appareils distants. Parfois, vous souhaiterez peut-être notifier tous les clients abonnés, mais vous pouvez choisir les appareils auxquels vous souhaitez envoyer la nouvelle valeur : 

```csharp
async void NotifyValue()
{
    var writer = new DataWriter();
    // Populate writer with data
    // ...
    
    await notifyCharacteristic.NotifyValueAsync(writer.DetachBuffer());
}
```

Lorsqu’un nouvel appareil s’abonne à des notifications, l’événement SubscribedClientsChanged est appelé : 

```csharp
characteristic.SubscribedClientsChanged += SubscribedClientsChanged;
// ...

void _notifyCharacteristic_SubscribedClientsChanged(GattLocalCharacteristic sender, object args)
{
    List<GattSubscribedClient> clients = sender.SubscribedClients;
    // Diff the new list of clients from a previously saved one 
    // to get which device has subscribed for notifications. 

    // You can also just validate that the list of clients is expected for this app.  
}

```
> Notez qu’une application peut obtenir la taille de notification maximale pour un client particulier avec la propriété MaxNotificationSize.  Toutes les données dont la taille est supérieure à la taille maximale seront tronquées par le système.