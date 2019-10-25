---
title: Revendication d’appareil PointOfService et activer le modèle
description: En savoir plus sur la revendication PointOfService et l’activation du modèle
ms.date: 06/19/2018
ms.topic: article
keywords: windows 10, uwp, point de vente, pdv
ms.localizationpriority: medium
ms.openlocfilehash: 0f3fc2b2aa10fedf143c55158e521b2c1cd5b75d
ms.sourcegitcommit: 6fbf645466278c1f014c71f476408fd26c620e01
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72816693"
---
# <a name="point-of-service-device-claim-and-enable-model"></a>Revendication d’appareil de point de service et activer le modèle

## <a name="claiming-for-exclusive-use"></a>Demande d’utilisation exclusive

Une fois que vous avez créé un objet d’appareil PointOfService, vous devez le revendiquer à l’aide de la méthode de revendication appropriée pour le type de périphérique avant de pouvoir utiliser le périphérique pour l'entrée ou la sortie.  La revendication accorde à l’application un accès exclusif à de nombreuses fonctions du périphérique pour vous assurer qu’une application n’interfère pas avec l’utilisation de l’appareil par une autre application.  Une seule application peut revendiquer un périphérique PointOfService pour une utilisation exclusive à la fois. 

> [!Note]
> L’action claim établit un verrou exclusif sur un appareil, mais ne le met pas dans un état opérationnel.  Pour plus d’informations, consultez [activer l’appareil pour les opérations d’e/s](#enable-device-for-io-operations) .

### <a name="apis-used-to-claim--release"></a>API utilisées pour revendiquer/libérer

|Appareil|Revendication | Publier | 
|-|:-|:-|
|BarcodeScanner | [BarcodeScanner.ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync) | [ClaimedBarcodeScanner. Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.close) |
|CashDrawer | [CashDrawer.ClaimDrawerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.claimdrawerasync) | [ClaimedCashDrawer. Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.close) | 
|LineDisplay | [LineDisplay.ClaimAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.claimasync) |  [ClaimedineDisplay. Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.close) | 
|MagneticStripeReader | [MagneticStripeReader.ClaimReaderAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.claimreaderasync) |  [ClaimedMagneticStripeReader. Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.close) | 
|PosPrinter | [PosPrinter.ClaimPrinterAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.claimprinterasync) |  [ClaimedPosPrinter. Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.close) | 
 | 

## <a name="enable-device-for-io-operations"></a>Activer l’appareil pour les opérations d’e/s

L’action de revendication établit simplement des droits exclusifs sur l’appareil, mais ne le met pas dans un état opérationnel.  Pour recevoir des événements ou effectuer des opérations de sortie, vous devez activer l’appareil à l’aide de **EnableAsync**.  Inversement, vous pouvez appeler **DisableAsync** pour arrêter d’écouter les événements de l’appareil ou effectuer une sortie.  Vous pouvez également utiliser **IsEnabled** pour déterminer l’état de votre appareil.

### <a name="apis-used-enable--disable"></a>API utilisées-activer/désactiver

| Appareil | Enable | Désactivez | IsEnabled? |
|-|:-|:-|:-|
|ClaimedBarcodeScanner | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isenabled) | 
|ClaimedCashDrawer | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.isenabled) |
|ClaimedLineDisplay | Non applicable ¹ | Non applicable ¹ | Non applicable ¹ | 
|ClaimedMagneticStripeReader | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.isenabled) |  
|ClaimedPosPrinter | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.isenabled) |
|

¹ l’affichage de ligne ¹ ne vous oblige pas à activer explicitement l’appareil pour les opérations d’e/s.  L’activation de est effectuée automatiquement par les API PointOfService LineDisplay qui effectuent des e/s.

## <a name="code-sample-claim-and-enable"></a>Exemple de code : revendication et activer

Cet exemple montre comment revendiquer un scanneur de code-barres après avoir créé avec succès un objet scanneur de code-barres.

```Csharp

    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(DeviceId);

    if(barcodeScanner != null)
    {
        // after successful creation, claim the scanner for exclusive use 
        claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();

        if(claimedBarcodeScanner != null)
        {
            // after successful claim, enable scanner for data events to fire
            await claimedBarcodeScanner.EnableAsync();
        }
        else
        {
            Debug.WriteLine("Failure to claim barcodeScanner");
        }
    }
    else
    {
        Debug.WriteLine("Failure to create barcodeScanner object");
    }
    
```

> [!Warning]
> Une revendication peut se perdre dans les circonstances suivantes :
> 1. Une autre application a demandé une revendication du même périphérique et votre application n’a pas émis de **RetainDevice** en réponse à l'événement **ReleaseDeviceRequested**.  (Voir [Négociation de revendication](#Claim-negotiation) ci-dessous pour plus d’informations.)
> 2. Votre application a été suspendue, ce qui a entraîné la fermeture de l'objet de périphérique et par conséquent la revendication n’est plus valide. (Voir [Cycle de vie de l'objet appareil](pos-basics-deviceobject.md#device-object-lifecycle) pour plus d’informations.)


## <a name="claim-negotiation"></a>Négociation de revendication

Comme Windows est un environnement multitâche, il est possible que plusieurs applications sur le même ordinateur exigent un accès aux périphériques de manière coopérative.  Les API PointOfService fournissent un modèle de négociation qui permet à plusieurs applications de partager des périphériques connectés à l’ordinateur.

Lorsqu’une deuxième application sur le même ordinateur demande une revendication pour un périphérique PointOfService déjà réclamé par une autre application, une notification d’événements **ReleaseDeviceRequested** est publiée. L’application avec la revendication active doit répondre à la notification d’événements en appelant **RetainDevice** si elle utilise actuellement le périphérique afin d'éviter de perdre la revendication. 

Si l’application avec la revendication active ne répond pas avec **RetainDevice** immédiatement, il est supposé que l’application a été suspendue ou n’a pas besoin du périphérique. La revendication est alors révoquée et donnée à la nouvelle application. 

La première étape consiste à créer un gestionnaire d’événements qui répond à l’événement **ReleaseDeviceRequested** avec **RetainDevice**.  

```Csharp
    /// <summary>
    /// Event handler for the ReleaseDeviceRequested event which occurs when 
    /// the claimed barcode scanner receives a Claim request from another application
    /// </summary>
    void claimedBarcodeScanner_ReleaseDeviceRequested(object sender, ClaimedBarcodeScanner myScanner)
    {
        // Retain exclusive access to the device
        myScanner.RetainDevice();
    }
```

Puis inscrire le gestionnaire d’événements en association avec votre appareil revendiqué

```Csharp
    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(DeviceId);

    if(barcodeScanner != null)
    {
        // after successful creation, claim the scanner for exclusive use 
        claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();

        if(claimedBarcodeScanner != null)
        {
            // register a release request handler to prevent loss of scanner during active use
            claimedBarcodeScanner.ReleaseDeviceRequested += claimedBarcodeScanner_ReleaseDeviceRequested;

            // after successful claim, enable scanner for data events to fire
            await claimedBarcodeScanner.EnableAsync();          
        }
        else
        {
            Debug.WriteLine("Failure to claim barcodeScanner");
        }
    }
    else
    {
        Debug.WriteLine("Failure to create barcodeScanner object");
    }
```



### <a name="apis-used-for-claim-negotiation"></a>API utilisées pour la négociation de revendication

|Périphérique revendiqué|Notification de libération| Conserver l’appareil |
|-|:-|:-|
|ClaimedBarcodeScanner | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.retaindevice)
|ClaimedCashDrawer | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.retaindevice)
|ClaimedLineDisplay | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.retaindevice)
|ClaimedMagneticStripeReader | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.retaindevice)
|ClaimedPosPrinter | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.retaindevice)
|
