---
title: Bluetooth Low Energy
description: En savoir plus sur la spécification Bluetooth Low Energy (LE) dans les applications UWP qui définit des protocoles pour la découverte et la communication entre des appareils économes en énergie.
ms.date: 03/19/2017
ms.topic: article
keywords: Windows 10, UWP, Bluetooth, Bluetooth les, faible énergie, GATT, Gap, central, périphérique, client, serveur, observateur, serveur de publication
ms.localizationpriority: medium
ms.openlocfilehash: 566e8e26e1a219a07dd5e7b539d96878a79f9163
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159763"
---
# <a name="bluetooth-low-energy"></a>Bluetooth Low Energy
Bluetooth Low Energy (LE) est une spécification qui définit des protocoles pour la découverte et la communication entre des appareils économes en énergie. La détection des appareils s’effectue par le biais du protocole de profil d’accès générique (GAP). Après la découverte, la communication appareil-à-appareil s’effectue via le protocole d’attribut générique (GATT). Cette rubrique fournit une vue d’ensemble rapide de Bluetooth LE dans les applications UWP. Pour plus d’informations sur Bluetooth le, consultez la [spécification Bluetooth Core](https://www.bluetooth.com/specifications/bluetooth-core-specification/) version 4,0, où Bluetooth le a été introduit. 

![Rôles Bluetooth LE](images/gatt-roles.png)

*Les rôles GATT et GAP ont été introduits dans Windows 10 version 1703*

Les protocoles GATT et GAP peuvent être implémentés dans votre application UWP à l’aide des espaces de noms suivants.
- [Windows.Devices.Bluetooth.GenericAttributeProfile](/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](/uwp/api/windows.devices.bluetooth.advertisement)

## <a name="central-and-peripheral"></a>Central et périphérique
Les deux principaux rôles de découverte sont appelés central et périphérique. En général, Windows fonctionne en mode central et se connecte à différents périphériques. 

## <a name="attributes"></a>Attributs
Un acronyme commun que vous verrez dans les API Bluetooth de Windows est l’attribut générique (GATT). Le profil GATT définit la structure des données et les modes d’opération par lesquels deux périphériques Bluetooth LE communiquent. L’attribut est le principal bloc de construction du GATT. Les principaux types d’attributs sont les services, les caractéristiques et les descripteurs. Ces attributs s’exécutent différemment entre les clients et les serveurs. il est donc plus utile d’aborder leur interaction dans les sections correspondantes. 

![Hiérarchie d’attributs standard dans un profil commun](images/gatt-service.png)

*Le service de taux cardiaque est exprimé dans le formulaire de l’API du serveur GATT*

## <a name="client-and-server"></a>Client et serveur
Une fois qu’une connexion a été établie, l’appareil qui contient les données (généralement un petit capteur IoT ou portable) est appelé serveur. L’appareil qui utilise ces données pour exécuter une fonction est connu sous le nom de client. Par exemple, un PC Windows (client) lit les données à partir d’une analyse de taux cardiaque (serveur) pour effectuer le suivi de l’utilisation optimale d’un utilisateur. Pour plus d’informations, consultez les rubriques relatives au [client GATT](gatt-client.md) et au [serveur GATT](gatt-server.md) .

## <a name="watchers-and-publishers-beacons"></a>Observateurs et éditeurs (balises)
Outre les rôles centraux et périphériques, il existe des rôles observateurs et diffuseurs. Les diffuseurs sont communément appelés balises, mais ils ne communiquent pas sur le GATT, car ils utilisent l’espace limité fourni dans le paquet de publication pour la communication. De même, un observateur n’a pas besoin d’établir une connexion pour recevoir des données, il recherche les publications voisines. Pour configurer Windows afin d’observer les publications à proximité, utilisez la classe [BluetoothLEAdvertisementWatcher](/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher) . Pour diffuser des signaux, utilisez la classe [BluetoothLEAdvertisementPublisher](/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher) . Pour plus d’informations, consultez la rubrique [publication](ble-beacon.md) .

## <a name="see-also"></a>Voir aussi
- [Windows.Devices.Bluetooth.GenericAttributeProfile](/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](/uwp/api/windows.devices.bluetooth.advertisement)
- [Spécification du noyau Bluetooth](https://www.bluetooth.com/specifications/bluetooth-core-specification)