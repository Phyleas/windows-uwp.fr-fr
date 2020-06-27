---
title: Client GATT Bluetooth
description: Cet article fournit une vue d’ensemble du client du profil d’attribut générique Bluetooth pour les applications de plateforme Windows universelle (UWP), ainsi que des exemples de code pour les cas d’utilisation courants.
ms.date: 06/26/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5c17351cf964ffb05dc60dbaf5c6ced1db467f78
ms.sourcegitcommit: 015291bdf2e7d67076c1c85fc025f49c840ba475
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85469534"
---
# <a name="bluetooth-gatt-client"></a>Client GATT Bluetooth

Cet article illustre l’utilisation des API clientes de l’attribut générique Bluetooth (GATT) pour les applications de plateforme Windows universelle (UWP), ainsi que des exemples de code pour les tâches courantes du client GATT :

- Interroger des appareils proches
- Se connecter à l’appareil
- Énumérer les services et caractéristiques pris en charge de l’appareil
- Lire et écrire dans une caractéristique
- S’abonner aux notifications lorsque la valeur caractéristique change

> [!Important]
> Vous devez déclarer la fonctionnalité « Bluetooth » dans *Package. appxmanifest*.
>
> `<Capabilities> <DeviceCapability Name="bluetooth" /> </Capabilities>`

> **API importantes**
>
> - [**Windows. Devices. Bluetooth**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth)
> - [**Windows. Devices. Bluetooth. GenericAttributeProfile**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile)

## <a name="overview"></a>Vue d’ensemble

Les développeurs peuvent utiliser les API de l’espace de noms [**Windows. Devices. Bluetooth. GenericAttributeProfile**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile) pour accéder aux périphériques Bluetooth le. Les appareils Bluetooth LE exposent leurs fonctionnalités par le biais d’une collection de :

- Services
- Caractéristiques
- Descripteurs

Les services définissent le contrat fonctionnel du périphérique le et contiennent un ensemble de caractéristiques qui définissent le service. Ces caractéristiques, à leur tour, contiennent des descripteurs qui décrivent les caractéristiques. Ces trois termes sont génériques appelés attributs d’un appareil.

Les API du GATT Bluetooth le exposent des objets et des fonctions, plutôt que d’accéder au transport brut. Les API du GATT permettent également aux développeurs de travailler avec des appareils Bluetooth le, avec la possibilité d’effectuer les tâches suivantes :

- Effectuer la détection des attributs
- Lire et écrire des valeurs d’attribut
- Inscrire un rappel pour l’événement ValueChanged caractéristique

Pour créer une implémentation utile, un développeur doit avoir une connaissance préalable des services et caractéristiques du GATT que l’application envisage de consommer et de traiter les valeurs caractéristiques spécifiques de telle sorte que les données binaires fournies par l’API soient transformées en données utiles avant d’être présentées à l’utilisateur. Les API GATT Bluetooth exposent uniquement les primitives de base nécessaires pour communiquer avec un périphérique Bluetooth LE. Pour interpréter les données, un profil d’application doit être défini, soit par un profil Bluetooth SIG standard, soit par un profil personnalisé implémenté par un fournisseur de périphérique. Un profil crée un contrat de liaison entre l’application et le périphérique, qui définit ce que représentent les données échangées et la façon de les interpréter.

Pour des raisons pratiques, le Bluetooth SIG gère une [liste de profils publics](https://www.bluetooth.com/specifications/adopted-specifications#gattspec) disponibles.

## <a name="query-for-nearby-devices"></a>Interroger des appareils proches

Il existe deux méthodes principales pour interroger des appareils proches :

- DeviceWatcher dans Windows. Devices. Enumeration
- AdvertisementWatcher dans Windows. Devices. Bluetooth. publication

La deuxième méthode est abordée dans la documentation de la [publication](ble-beacon.md) . elle ne sera donc pas abordée dans la plupart des cas, mais l’idée de base est de trouver l’adresse Bluetooth des appareils proches qui satisfont au [filtre de publication](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.advertisementfilter)particulier. Une fois que vous avez l’adresse, vous pouvez appeler [BluetoothLEDevice. FromBluetoothAddressAsync](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.bluetoothledevice.frombluetoothaddressasync) pour obtenir une référence à l’appareil.

À présent, revenez à la méthode DeviceWatcher. Un appareil Bluetooth LE est similaire à tout autre appareil dans Windows et peut être interrogé à l’aide des [API d’énumération](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration). Utilisez la classe [DeviceWatcher](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher) et transmettez une chaîne de requête spécifiant les appareils à Rechercher :

```csharp
// Query for extra properties you want returned
string[] requestedProperties = { "System.Devices.Aep.DeviceAddress", "System.Devices.Aep.IsConnected" };

DeviceWatcher deviceWatcher =
            DeviceInformation.CreateWatcher(
                    BluetoothLEDevice.GetDeviceSelectorFromPairingState(false),
                    requestedProperties,
                    DeviceInformationKind.AssociationEndpoint);

// Register event handlers before starting the watcher.
// Added, Updated and Removed are required to get all nearby devices
deviceWatcher.Added += DeviceWatcher_Added;
deviceWatcher.Updated += DeviceWatcher_Updated;
deviceWatcher.Removed += DeviceWatcher_Removed;

// EnumerationCompleted and Stopped are optional to implement.
deviceWatcher.EnumerationCompleted += DeviceWatcher_EnumerationCompleted;
deviceWatcher.Stopped += DeviceWatcher_Stopped;

// Start the watcher.
deviceWatcher.Start();
```

Une fois que vous avez démarré le DeviceWatcher, vous recevrez [DeviceInformation](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) pour chaque appareil qui répond à la requête dans le gestionnaire pour l’événement [ajouté](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added) pour les appareils en question. Pour plus d’informations sur DeviceWatcher, consultez l’exemple complet [sur GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing).

## <a name="connecting-to-the-device"></a>Connexion à l’appareil

Une fois qu’un appareil souhaité est découvert, utilisez [DeviceInformation.ID](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) pour obtenir l’objet appareil Bluetooth le pour l’appareil en question :

```csharp
async void ConnectDevice(DeviceInformation deviceInfo)
{
    // Note: BluetoothLEDevice.FromIdAsync must be called from a UI thread because it may prompt for consent.
    BluetoothLEDevice bluetoothLeDevice = await BluetoothLEDevice.FromIdAsync(deviceInfo.Id);
    // ...
}
```

D’un autre côté, en supprimant toutes les références à un objet BluetoothLEDevice pour un appareil (et si aucune autre application sur le système n’a de référence à l’appareil), vous déclenchez une déconnexion automatique après un délai d’expiration réduit.

```csharp
bluetoothLeDevice.Dispose();
```

Si l’application doit à nouveau accéder à l’appareil, il suffit de recréer l’objet d’appareil et d’accéder à une caractéristique (décrite dans la section suivante) pour que le système d’exploitation se reconnecte en cas de besoin. Si l’appareil est proche, vous obtiendrez un accès à l’appareil. sinon, il renverra une erreur DeviceUnreachable.  

## <a name="enumerating-supported-services-and-characteristics"></a>Énumération des services et caractéristiques pris en charge

Maintenant que vous avez un objet BluetoothLEDevice, l’étape suivante consiste à découvrir les données exposées par l’appareil. Pour ce faire, la première étape consiste à interroger les services :

```csharp
GattDeviceServicesResult result = await bluetoothLeDevice.GetGattServicesAsync();

if (result.Status == GattCommunicationStatus.Success)
{
    var services = result.Services;
    // ...
}
```

Une fois le service d’intérêt identifié, l’étape suivante consiste à interroger les caractéristiques.

```csharp
GattCharacteristicsResult result = await service.GetCharacteristicsAsync();

if (result.Status == GattCommunicationStatus.Success)
{
    var characteristics = result.Characteristics;
    // ...
}
```

Le système d’exploitation retourne une liste en lecture seule des objets GattCharacteristic sur lesquels vous pouvez ensuite effectuer des opérations.

## <a name="perform-readwrite-operations-on-a-characteristic"></a>Effectuer des opérations de lecture/écriture sur une caractéristique

La caractéristique est l’unité fondamentale de communication basée sur le GATT. Elle contient une valeur qui représente un élément de données distinct sur l’appareil. Par exemple, la caractéristique niveau de batterie a une valeur qui représente le niveau de batterie de l’appareil.

Lisez les propriétés de caractéristiques pour déterminer les opérations prises en charge :

```csharp
GattCharacteristicProperties properties = characteristic.CharacteristicProperties

if(properties.HasFlag(GattCharacteristicProperties.Read))
{
    // This characteristic supports reading from it.
}
if(properties.HasFlag(GattCharacteristicProperties.Write))
{
    // This characteristic supports writing to it.
}
if(properties.HasFlag(GattCharacteristicProperties.Notify))
{
    // This characteristic supports subscribing to notifications.
}
```

Si Read est pris en charge, vous pouvez lire la valeur :

```csharp
GattReadResult result = await selectedCharacteristic.ReadValueAsync();
if (result.Status == GattCommunicationStatus.Success)
{
    var reader = DataReader.FromBuffer(result.Value);
    byte[] input = new byte[reader.UnconsumedBufferLength];
    reader.ReadBytes(input);
    // Utilize the data as needed
}
```

L’écriture dans une caractéristique suit un modèle similaire :

```csharp
var writer = new DataWriter();
// WriteByte used for simplicity. Other common functions - WriteInt16 and WriteSingle
writer.WriteByte(0x01);

GattCommunicationStatus result = await selectedCharacteristic.WriteValueAsync(writer.DetachBuffer());
if (result == GattCommunicationStatus.Success)
{
    // Successfully wrote to device
}
```

> **Conseil**: les éléments [DataReader](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader) et [DataWriter](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter) sont indispensable quand vous travaillez avec les mémoires tampons brutes que vous recevez à partir de nombreuses API Bluetooth.

## <a name="subscribing-for-notifications"></a>Abonnement à des notifications

Assurez-vous que la caractéristique prend en charge l’indication ou la notification (Vérifiez les propriétés caractéristiques pour vous en assurer).

> À **part**: l’indication est considérée comme plus fiable, car chaque événement de modification de valeur est associé à un accusé de réception de l’appareil client. La notification est plus répandue, car la plupart des transactions GATT préfèrent économiser de l’énergie plutôt que d’être extrêmement fiables. Dans tous les cas, tout ce qui est géré au niveau de la couche de contrôleur afin que l’application ne soit pas impliquée. Nous y reviendrons collectivement comme « notifications », mais vous le savez maintenant.

Il y a deux choses à prendre en charge avant d’obtenir des notifications :

- Écrire dans le descripteur de configuration de caractéristiques du client (CCCD)
- Gérer l’événement caractéristique. ValueChanged

L’écriture dans le CCCD indique à l’appareil serveur que ce client souhaite connaître chaque fois que cette valeur caractéristique particulière change. Pour ce faire :

```csharp
GattCommunicationStatus status = await selectedCharacteristic.WriteClientCharacteristicConfigurationDescriptorAsync(
                        GattClientCharacteristicConfigurationDescriptorValue.Notify);
if(status == GattCommunicationStatus.Success)
{
    // Server has been informed of clients interest.
}
```

À présent, l’événement ValueChanged de GattCharacteristic est appelé chaque fois que la valeur est modifiée sur le périphérique distant. Tout cela reste à implémenter le gestionnaire :

```csharp
characteristic.ValueChanged += Characteristic_ValueChanged;

...

void Characteristic_ValueChanged(GattCharacteristic sender,
                                    GattValueChangedEventArgs args)
{
    // An Indicate or Notify reported that the value has changed.
    var reader = DataReader.FromBuffer(args.CharacteristicValue)
    // Parse the data however required.
}
```
