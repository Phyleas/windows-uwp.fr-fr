---
title: FAQ sur le Bluetooth pour les développeurs
description: Cet article contient des réponses aux questions fréquentes relatives à l’API de bluetooth UWP.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e7dee32d-3756-430d-a026-32c1ee288a85
ms.localizationpriority: medium
ms.openlocfilehash: 7ff826be0f5b0b8e9a6723fbb1593663f1748c3d
ms.sourcegitcommit: d708ac4ec4fac0135dafc0d8c5161ef9fd945ce7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/18/2020
ms.locfileid: "85069469"
---
# <a name="bluetooth-developer-faq"></a>FAQ sur le Bluetooth pour les développeurs

Cet article contient des réponses aux questions les plus fréquentes concernant les API Bluetooth UWP.

## <a name="what-apis-do-i-use-bluetooth-classic-rfcomm-or-bluetooth-low-energy-gatt"></a>Quelles API dois-je utiliser ? Bluetooth Classic (RFCOMM) ou Bluetooth Low Energy (GATT) ?
Il y a plusieurs discussions en ligne sur cette rubrique générale. nous allons donc conserver cette réponse sur la différence par rapport à Windows. Voici quelques recommandations générales à prendre en compte :

### <a name="bluetooth-le-windowsdevicesbluetoothgenericattributeprofile"></a>Bluetooth LE (Windows. Devices. Bluetooth. GenericAttributeProfile)

Utilisez les API du GATT lorsque vous communiquez avec un appareil qui prend en charge Bluetooth Low Energy. Si votre cas d’utilisation est peu fréquent, une faible bande passante ou une alimentation faible, Bluetooth Low Energy est la solution. L’espace de noms principal qui comprend cette fonctionnalité est [Windows. Devices. Bluetooth. GenericAttributeProfile](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile). 

**Quand ne pas utiliser Bluetooth LE**
- Large bande passante, scénarios à fréquence élevée. Si vous avez besoin de conserver constamment une synchronisation avec de grandes quantités de données, envisagez d’utiliser Bluetooth Classic ou peut-être même WiFi. 

### <a name="bluetooth-classic-windowsdevicesbluetoothrfcomm"></a>Bluetooth classique (Windows. Devices. Bluetooth. RFCOMM)

Les API RFCOMM offrent aux développeurs un socket pour effectuer une communication bidirectionnelle de type port série. Une fois que vous disposez d’un socket, les méthodes d’écriture et de lecture sont relativement standard. Une implémentation de ce est présentée dans l' [exemple de conversation RFCOMM](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat). 

**Quand ne pas utiliser Bluetooth RFCOMM** 
- Fonctionnalité. Le protocole Bluetooth GATT a une commande spécifique pour cela, ce qui se traduira par un temps de réponse beaucoup moins important et des temps de réponse plus rapides. 
- Vérification de la détection de proximité ou de présence. Il est préférable d’utiliser les [API de publication](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement) et de se connecter via Bluetooth le. 


## <a name="why-does-my-bluetooth-le-device-stop-responding-after-a-disconnect"></a>Pourquoi mon périphérique Bluetooth LE ne répond plus après une déconnexion ?

La raison la plus courante est que l’appareil distant a perdu des informations de jumelage. Un grand nombre d’appareils Bluetooth plus anciens ne nécessitent pas d’authentification. Pour protéger l’utilisateur, toutes les transactions de jumelage effectuées à partir de l’application des paramètres nécessitent une authentification, et certains périphériques n’ont pas été conçus en tenant compte de cela. 

À compter de Windows 10 version 1511, les développeurs contrôlent la négociation d’appariement. L’[exemple d’énumération des périphériques et de jumelage](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing) détaille les différents aspects liés à l’association de nouveaux périphériques.

Dans cet exemple, nous générons le jumelage avec un périphérique sans chiffrement. Notez que cela est possible uniquement si le périphérique à distance ne nécessite pas de chiffrement ou d’authentification pour fonctionner.

```csharp
// Get ceremony type and protection level selections
// You must select at least ConfirmOnly or the pairing attempt will fail
    DevicePairingKinds ceremonySelected = DevicePairingKinds.ConfirmOnly;

//  Workaround remote devices losing pairing information
    DevicePairingProtectionLevel protectionLevel = DevicePairingProtectionLevel.None

    DeviceInformationCustomPairing customPairing = deviceInfoDisp.DeviceInformation.Pairing.Custom;

// Declare an event handler - you don't need to do much in PairingRequestedHandler since the ceremony is "None"
    customPairing.PairingRequested += PairingRequestedHandler;
    DevicePairingResult result = await customPairing.PairAsync(ceremonySelected, protectionLevel);
```

## <a name="do-i-have-to-pair-bluetooth-devices-before-using-them"></a>Dois-je jumeler des périphériques Bluetooth avant de les utiliser ?

Vous n’avez pas besoin de coupler des appareils avant de les utiliser si vous tirez parti de Bluetooth RFCOMM (Classic). À compter de Windows 10 version 1607, vous pouvez simplement interroger les appareils à proximité et vous y connecter. L’[exemple de discussion RFCOMM](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat) mis à jour présente cette fonctionnalité. 

**(14393 et versions antérieures)** Cette fonctionnalité n’est pas disponible pour l’énergie Bluetooth faible (client GATT). vous devrez donc toujours effectuer une paire via la page paramètres ou les API [Windows. Devices. Enumeration](https://docs.microsoft.com/uwp/api/windows.devices.enumeration) pour accéder à ces appareils.

**(15030 et versions ultérieures)** Le couplage des appareils Bluetooth n’est plus nécessaire. Utilisez les nouvelles API Async comme GetGattServicesAsync et GetCharacteristicsAsync afin d’interroger l’état actuel de l’appareil distant. Pour plus d’informations, consultez les [documents du client](gatt-client.md) . 

## <a name="when-should-i-pair-with-a-device-before-communicating-with-it"></a>Quand dois-je associer un appareil avant de communiquer avec lui ?
En règle générale, si vous avez besoin d’une obligation de confiance à long terme avec un appareil, associez-le en dirigeant l’utilisateur vers la page paramètres ou en utilisant l’énumération d’appareil et les API de couplage. Si vous avez simplement besoin de lire des informations à partir de l’appareil exposé publiquement (un capteur de température ou une balise), connectez-vous ou écoutez des publications sans avoir à associer l’appareil. Cela empêche les problèmes d’interopérabilité à long terme, car un grand nombre d’appareils ne prennent pas en charge le couplage. 

## <a name="do-all-windows-devices-support-peripheral-role"></a>Tous les appareils Windows prennent-ils en charge le rôle périphérique ?

Non. Il s’agit d’une fonctionnalité dépendante du matériel, mais une méthode est fournie, BluetoothAdapter. IsPeripheralRoleSupported, pour demander si elle est prise en charge ou non.  Les appareils actuellement pris en charge incluent Windows Phone sur 8992 + et RPi3 (Windows IoT). 

## <a name="can-i-access-these-apis-from-win32"></a>Puis-je accéder à ces API à partir de Win32 ?

Oui, toutes ces API doivent fonctionner. Ce blog détaille la façon d’appeler [des API Windows à partir d’applications de bureau](https://blogs.windows.com/buildingapps/2017/01/25/calling-windows-10-apis-desktop-application/). 
## <a name="is-this-functionality-supposed-to-exist-on--insert-sku-here-"></a>Cette fonctionnalité est-elle censée exister sur *-Insert SKU ici-*?

**Bluetooth le**: Oui, toutes les fonctionnalités se trouvent dans OneCore et doivent être disponibles sur les appareils les plus récents w/a qui fonctionnent avec la pile Bluetooth le. 
> Inconvénient : le rôle périphérique dépend du matériel et certaines éditions de Windows Server ne prennent pas en charge Bluetooth. 

**Bluetooth br/EDR (Classic)**: certaines variantes existent mais, en général, elles ont une prise en charge de niveau de profil très similaire. Consultez la documentation sur [RFCOMM](send-or-receive-files-with-rfcomm.md) et les documents de ce profil pris en charge pour [PC](https://support.microsoft.com/help/10568/windows-10-supported-bluetooth-profiles) et [téléphone](https://support.microsoft.com/help/10569/windows-10-mobile-supported-bluetooth-profiles)
