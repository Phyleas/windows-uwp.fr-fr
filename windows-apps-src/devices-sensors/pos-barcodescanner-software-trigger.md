---
title: Utiliser un déclencheur logiciel
description: Découvrez comment contrôler le processus d’analyse de codes-barres par programme à l’aide d’un déclencheur logiciel asynchrone.
ms.date: 08/29/2018
ms.topic: article
keywords: Windows 10, UWP, point de service, pos
ms.localizationpriority: medium
ms.openlocfilehash: ee61a428115522717f8ff57ef7a0fc04f19aae41
ms.sourcegitcommit: cb5af00af05e838621c270173e7fde1c5d2168ef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89043381"
---
# <a name="use-a-software-trigger"></a>Utiliser un déclencheur logiciel

Il peut être utile de contrôler l’action d’analyse à partir d’un logiciel si vous utilisez un scanneur de codes-barres en mode de présentation ou si le scanneur n’a pas de déclencheur physique, tel qu’un scanneur de codes-barres basés sur une caméra. Vous pouvez lancer le processus d’analyse en appelant [StartSoftwareTriggerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync).

Selon la valeur de [IsDisabledOnDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) , le scanneur ne peut analyser qu’un code-barres, puis l’arrêter ou l’analyser en continu jusqu’à ce que vous appeliez [StopSoftwareTriggerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync).

Définissez la valeur souhaitée de [IsDisabledOnDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) pour contrôler le comportement de l’analyseur lorsqu’un code-barres est décodé.

| Value | Description |
| ----- | ----------- |
| True   | Analyser un seul code-barres puis arrêter |
| Faux  | Analyser en continu les codes-barres sans les arrêter |


> [!Important]
> Vérifiez que votre scanneur de codes-barres prend en charge l’utilisation du déclencheur de logiciel en vérifiant d’abord la propriété [IsSoftwareTriggerSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannercapabilities.issoftwaretriggersupported#Windows_Devices_PointOfService_BarcodeScannerCapabilities_IsSoftwareTriggerSupported).

L’exemple suivant montre comment lancer une analyse à l’aide d’un déclencheur de logiciel, qui arrête l’analyse une fois qu’il a analysé un code-barres :

```cs
private void SoftwareTrigger(BarcodeScanner barcodeScanner, ClaimedBarcodeScanner claimedBarcodeScanner) 
{
    if (barcodeScanner.Capabilities.IsSoftwareTriggerSupported)
    {
        claimedBarcodeScanner.IsDisabledOnDataReceived = true;
        await claimedBarcodeScanner.StartSoftwareTriggerAsync();
    }
}
```

[!INCLUDE [feedback](./includes/pos-feedback.md)]