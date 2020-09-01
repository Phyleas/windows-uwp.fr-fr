---
title: Partage d’appareils PointOfService
description: Découvrez comment partager des périphériques réseau ou Bluetooth connectés avec d’autres ordinateurs dans un environnement où plusieurs PC s’appuient sur des périphériques partagés.
ms.date: 06/14/2018
ms.topic: article
keywords: Windows 10, UWP, point de service, pos
ms.localizationpriority: medium
ms.openlocfilehash: 4fad9bc75ed0ff79be1596a3c99445c7e9f97b1f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163263"
---
# <a name="pointofservice-device-sharing"></a>Partage d’appareils PointOfService

Découvrez comment partager des périphériques réseau ou Bluetooth connectés avec d’autres ordinateurs dans un environnement où plusieurs PC s’appuient sur des périphériques partagés plutôt que sur des périphériques dédiés connectés à chaque ordinateur.

## <a name="device-sharing"></a>Partage d’appareils

Les périphériques PointOfService connectés réseau et Bluetooth sont généralement utilisés dans un environnement wheere plusieurs périphériques clients partagent les mêmes périphériques tout au long de la journée.  Dans un environnement de vente au détail ou dans le secteur de l’alimentation, tout délai de la possibilité pour un appareil client de s’attacher à un périphérique a un impact sur l’efficacité dans laquelle un partenaire Associate peut fermer une transaction avec le client et passer à la suivante. Dans un scénario de restaurant de service rapide où une imprimante de réception est utilisée comme imprimante de cuisine pour transférer les détails de la commande d’un client à la cuisine en vue de sa préparation, plusieurs appareils clients prennent des commandes de la part des clients.  Une fois la commande terminée, chaque appareil client doit pouvoir revendiquer l’imprimante partagée et imprimer immédiatement la commande pour la cuisine.

Dans ces environnements, il est important que l’application **supprime** complètement l’objet appareil pour qu’un autre puisse revendiquer le même appareil.

Suppression d’un PosPrinter à la fin d’un bloc’Using'

```Csharp 
using Windows.Devices.PointOfService;
using(PosPrinter printer = await PosPrinter.FromIdAsync("Device ID"))
{
    if (printer != null)
    {
        // Exercise the printer.
    }

    // When leaving this scope, printer.Dispose() is automatically invoked, 
    // releasing the session we have with the printer.
}
```


Suppression d’un PosPrinter en appelant la méthode Dispose () explicitement

```Csharp 
using Windows.Devices.PointOfService;

PosPrinter printer = await PosPrinter.FromIdAsync("Device ID");
if (printer != null)
{
    // Exercise the printer, then dispose of the printer explicitly.
    printer.Dispose();
}
```

## <a name="api-methods-used"></a>Méthodes d’API utilisées 

+ [BarcodeScanner. dispose](/uwp/api/windows.devices.pointofservice.barcodescanner.dispose) 
+ [CashDrawer. dispose](/uwp/api/windows.devices.pointofservice.cashdrawer.dispose) 
+ [LineDisplay. dispose](/uwp/api/windows.devices.pointofservice.linedisplay.dispose) 
+ [MagneticStripeReader. dispose](/uwp/api/windows.devices.pointofservice.magneticstripereader.dispose)  
+ [PosPrinter. dispose](/uwp/api/windows.devices.pointofservice.posprinter.dispose) 


[!INCLUDE [feedback](./includes/pos-feedback.md)]