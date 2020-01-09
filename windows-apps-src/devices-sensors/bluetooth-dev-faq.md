---
title: FAQ sur le Bluetooth pour les développeurs
description: Cet article contient des réponses aux questions fréquentes relatives à l’API de bluetooth UWP.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e7dee32d-3756-430d-a026-32c1ee288a85
ms.localizationpriority: medium
ms.openlocfilehash: 584d327fc4882db6d3bf8d0cfd2a84b13023c6f4
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684840"
---
# <a name="bluetooth-developer-faq"></a>FAQ sur le Bluetooth pour les développeurs

Cet article contient des réponses aux questions les plus fréquentes concernant les API Bluetooth UWP.

## <a name="what-apis-do-i-use-bluetooth-classic-rfcomm-or-bluetooth-low-energy-gatt"></a>Quelles API dois-je utiliser ? Bluetooth Classic (RFCOMM) ou Bluetooth Low Energy (GATT) ?
Cette question générale fait l’objet de nombreuses discussions en ligne. Nous examinerons donc la différence entre ces deux types d’API strictement du point de vue de Windows. Voici quelques recommandations générales :

### <a name="bluetooth-le-windowsdevicesbluetoothgenericattributeprofile"></a>Bluetooth LE (Windows.Devices.Bluetooth.GenericAttributeProfile)

Utilisez les API GATT lorsque vous communiquez avec un appareil qui prend en charge la technologie Bluetooth Low Energy. Si votre cas d’utilisation est peu fréquent, une faible bande passante ou une alimentation faible, Bluetooth Low Energy est la solution. L’espace de noms principal incluant cette fonctionnalité est [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile). 

**Quand ne pas utiliser Bluetooth LE**
- Scénarios d’utilisation très fréquente, impliquant une bande passante élevée. Si vous devez vous synchroniser continuellement avec de grandes quantités de données, envisagez d’utiliser Bluetooth Classic ou encore le WiFi. 

### <a name="bluetooth-classic-windowsdevicesbluetoothrfcomm"></a>Bluetooth Classic (Windows.Devices.Bluetooth.Rfcomm)

Les API RFCOMM offrent aux développeurs un socket pour effectuer une communication bidirectionnelle de type port série. Une fois que vous disposez d’un socket, les méthodes d’écriture et de lecture sont relativement standard. Une implémentation de cette approche est présentée dans l’[exemple de conversation RFCOMM](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat). 

**Quand ne pas utiliser Bluetooth RFCOMM** 
- Notifications : le protocole GATT Bluetooth dispose d’une commande spécifique à cet effet et entraînera une économie d’énergie substantielle, ainsi que des temps de réponse beaucoup plus courts. 
- Vérification de proximité ou détection de présence : il est préférable d’utiliser les [API Advertisement](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement) et de se connecter par le biais de Bluetooth LE. 


## <a name="why-does-my-bluetooth-le-device-stop-responding-after-a-disconnect"></a>Pourquoi mon appareil Bluetooth LE ne répond-il plus après une déconnexion ?

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

**(14393 et antérieur)** Cette fonctionnalité n’est pas disponible pour Bluetooth Low Energy (client GATT). Vous devrez donc continuer le couplage par le biais de la page Paramètres ou à l’aide des API [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/windows.devices.enumeration) pour accéder à ces appareils.

**(15030 et ultérieur)** Le couplage d’appareils Bluetooth n’est plus nécessaire. Pour rechercher l’état actuel de l’appareil distant, utilisez les nouvelles API Async, telles que GetGattServicesAsync et GetCharacteristicsAsync. Pour plus d’informations, consultez la [documentation relative au client](gatt-client.md). 

## <a name="when-should-i-pair-with-a-device-before-communicating-with-it"></a>Quand dois-je effectuer un couplage à un appareil avant de communiquer avec ce dernier ?
En règle générale, si vous avez besoin d’une obligation de confiance à long terme avec un appareil, associez-le en dirigeant l’utilisateur vers la page paramètres ou en utilisant l’énumération d’appareil et les API de couplage. Si vous avez simplement besoin de lire des informations à partir de l’appareil exposé publiquement (un capteur de température ou une balise), connectez-vous ou écoutez des publications sans avoir à associer l’appareil. Cela empêche les problèmes d’interopérabilité à long terme, car un grand nombre d’appareils ne prennent pas en charge le couplage. 

## <a name="do-all-windows-devices-support-peripheral-role"></a>Tous les appareils Windows prennent-ils en charge le rôle périphérique ?

Non. Il s’agit d’une fonctionnalité dépendante du matériel, mais une méthode est fournie, BluetoothAdapter. IsPeripheralRoleSupported, pour demander si elle est prise en charge ou non.  Les appareils actuellement pris en charge comprennent Windows Phone sur 8992+ et RPi3 (Windows IoT). 

## <a name="can-i-access-these-apis-from-win32"></a>Puis-je accéder à ces API à partir de Win32 ?

Oui, toutes ces API doivent fonctionner. Ce blog décrit la procédure à suivre pour appeler des [API Windows à partir d’applications de bureau](https://blogs.windows.com/buildingapps/2017/01/25/calling-windows-10-apis-desktop-application/). 
## <a name="is-this-functionality-supposed-to-exist-on--insert-sku-here-"></a>Cette fonctionnalité est-elle censée exister sur *-insérer la référence SKU ici-*  ?

**Bluetooth LE** : oui, toutes les fonctionnalités figurent dans OneCore et doivent être disponibles sur les appareils les plus récents avec une pile Bluetooth LE opérationnelle. 
> Inconvénient : le rôle périphérique dépend du matériel et certaines éditions de Windows Server ne prennent pas en charge Bluetooth. 

**Bluetooth br/EDR (Classic)** : certaines variantes existent mais, en général, elles ont une prise en charge de niveau de profil très similaire. Consultez la documentation sur [RFCOMM](send-or-receive-files-with-rfcomm.md) et les documents de ce profil pris en charge pour [PC](https://support.microsoft.com/help/10568/windows-10-supported-bluetooth-profiles) et [téléphone](https://support.microsoft.com/help/10569/windows-10-mobile-supported-bluetooth-profiles)
