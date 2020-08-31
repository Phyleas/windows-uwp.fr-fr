---
title: Prise en main avec le scanneur de codes-barres de l’appareil photo
description: Utilisez ces instructions pas à pas et extraits de code pour commencer à utiliser un scanneur de codes-barres de l’appareil photo.
ms.date: 09/02/2019
ms.topic: article
keywords: Windows 10, UWP, point de service, pos
ms.localizationpriority: medium
ms.openlocfilehash: f7206250b2495ae2c905d2aece2b8cd1b85d6859
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168473"
---
# <a name="getting-started-with-a-camera-barcode-scanner"></a>Prise en main d’un scanneur de codes-barres de l’appareil photo

Les extraits de code utilisés ici sont fournis à des fins de démonstration uniquement. Pour obtenir un exemple fonctionnel, consultez l' [exemple de scanneur de codes-barres](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner).

## <a name="step-1-add-capability-declarations-to-your-app-manifest"></a>Étape 1 : ajouter des déclarations de fonctionnalité à votre manifeste d’application

1. Dans Microsoft Visual Studio, dans l’**Explorateur de solutions**, ouvrez le concepteur pour le manifeste de l’application en double-cliquant sur l’élément **package.appxmanifest**.
2. Sélectionnez l’onglet **capacités**
3. Cochez les cases **webcam** et **PointOfService**

>[!NOTE]
> La fonction **webcam** est requise pour permettre au décodeur logiciel de recevoir des frames de la caméra à décoder, ainsi que de fournir un aperçu de votre application

## <a name="step-2-add-using-directives"></a>Étape 2 : ajouter des directives using

```Csharp
using Windows.Devices.Enumeration;
using Windows.Devices.PointOfService;
```

## <a name="step-3-define-your-device-selector"></a>Étape 3 : définir votre sélecteur d’appareil

### <a name="option-a-find-all-barcode-scanners"></a>**Option A : Rechercher tous les scanneurs de codes-barres**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector();
```

### <a name="option-b-scoping-device-selector-to-connection-type"></a>**Option B : portée du sélecteur d’appareil à un type de connexion**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector(PosConnectionTypes.Local);
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

## <a name="step-4-enumerate-all-barcode-scanners"></a>Étape 4 : énumérer tous les scanneurs de codes-barres

Si vous ne vous attendez pas à ce que la liste des appareils change sur la durée de vie de votre application, vous pouvez énumérer une capture instantanée une seule fois avec [DeviceInformation. FindAllAsync](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync), mais si vous pensez que la liste des scanneurs de codes-barres peut changer pendant la durée de vie de votre application, vous devez utiliser un [DeviceWatcher](/uwp/api/windows.devices.enumeration.devicewatcher) à la place.  

> [!Important]
> L’utilisation de GetDefaultAsync pour énumérer les appareils PointOfService peut entraîner un comportement incohérent, car elle retourne simplement le premier périphérique trouvé dans la classe, ce qui peut changer d’une session à l’autres.

### <a name="option-a-enumerate-a-snapshot-of-barcode-scanners"></a>**Option A : énumérer un instantané des scanneurs de codes-barres**

```Csharp
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

> [!TIP]
> Pour plus d’informations sur l’utilisation de *FindAllAsync*, consultez [*énumérer un instantané de périphériques*](./enumerate-devices.md#enumerate-a-snapshot-of-devices) .

### <a name="option-b-enumerate-available-barcode-scanners-and-watch-for-changes-to-the-available-scanners"></a>**Option B : énumérer les scanneurs de codes-barres disponibles et surveiller les modifications apportées aux scanneurs disponibles**

```Csharp
DeviceWatcher deviceWatcher = DeviceInformation.CreateWatcher(selector);
watcher.Added += Watcher_Added;
watcher.Removed += Watcher_Removed;
watcher.Updated += Watcher_Updated;
watcher.Start();
```

> [!TIP]
> Pour plus d’informations, consultez [*énumérer et surveiller les modifications des appareils*](./enumerate-devices.md#enumerate-and-watch-devices) et [*DeviceWatcher*](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) .

## <a name="step-5-identify-camera-barcode-scanners"></a>Étape 5 : identifier les scanneurs de codes-barres de l’appareil photo

Un scanneur de codes-barres de l’appareil photo est créé dynamiquement lorsque Windows associe la ou les caméras attachées à votre ordinateur à un décodeur logiciel.  Chaque paire décodeur de caméra est un scanneur de codes-barres entièrement fonctionnel.

Pour chaque scanneur de codes-barres du regroupement d’appareils obtenu, vous pouvez différencier les scanneurs de code-barres de l’appareil photo des scanneurs de codes-barres physiques en activant la propriété [*BarcodeScanner. VideoDeviceID*](/uwp/api/windows.devices.pointofservice.barcodescanner.videodeviceid#Windows_Devices_PointOfService_BarcodeScanner_VideoDeviceId) .  Un VideoDeviceID non NULL indique que l’objet de scanneur de codes-barres de votre collection de périphériques est un scanneur de codes-barres de l’appareil photo.  Si vous avez plusieurs scanneurs de codes-barres d’appareil photo, vous souhaiterez peut-être créer un regroupement distinct qui exclut les scanneurs de codes-barres physiques.

Les scanneurs de codes-barres de l’appareil photo utilisant le décodeur fourni avec Windows sont identifiés comme suit :

> Microsoft BarcodeScanner (*nom de votre appareil photo ici*)

Si vous disposez de plusieurs caméras et que celles-ci sont intégrées au châssis de votre ordinateur, le nom peut faire la différence entre les caméras de l' *avant* et l' *arrière* .

> [!NOTE]
> À l’avenir, des décodeurs logiciels supplémentaires avec des schémas de nommage différents pourront être publiés.

Quand le DeviceWatcher démarre (étape 4), il énumère chaque appareil connecté. Ici, nous allons ajouter les scanneurs disponibles à une collection de scanneurs de codes-barres et lier la collection à une zone de liste.

```csharp
ObservableCollection<BarcodeScannerInfo> barcodeScanners = new ObservableCollection<BarcodeScannerInfo>();

private async void Watcher_Added(DeviceWatcher sender, DeviceInformation args)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        barcodeScanners.Add(new BarcodeScannerInfo(args.Name, args.Id));

        // Select the first scanner by default.
        if (barcodeScanners.Count == 1)
        {
            ScannerListBox.SelectedIndex = 0;
        }
    });
}
```

Lorsque la SelectedIndex du ListBox change (le premier élément est sélectionné par défaut dans l’extrait de code précédent), nous interrogeons les informations de l’appareil.

```csharp
private async void ScannerSelection_Changed(object sender, SelectionChangedEventArgs args)
{
    var selectedScannerInfo = (BarcodeScannerInfo)args.AddedItems[0];
    var deviceId = selectedScannerInfo.DeviceId;

    await SelectScannerAsync(deviceId);
}
```

## <a name="step-6-claim-the-camera-barcode-scanner"></a>Étape 6 : demander le scanneur de codes-barres de l’appareil photo

Utilisez [BarcodeScanner. ClaimScannerAsync](/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync) pour obtenir l’utilisation exclusive du scanneur de codes-barres de l’appareil photo.

```csharp
private async Task SelectScannerAsync(string scannerDeviceId)
{
    selectedScanner = await BarcodeScanner.FromIdAsync(scannerDeviceId);

    if (selectedScanner != null)
    {
        claimedScanner = await selectedScanner.ClaimScannerAsync();
        if (claimedScanner != null)
        {
            await claimedScanner.EnableAsync();
        }
        else
        {
            rootPage.NotifyUser("Failed to claim the selected barcode scanner", NotifyType.ErrorMessage);
        }
    }
    else
    {
        rootPage.NotifyUser("Failed to create a barcode scanner object", NotifyType.ErrorMessage);
    }
}
```

## <a name="step-7-system-provided-preview"></a>Étape 7 : version préliminaire fournie par le système

Une préversion de l’appareil photo est nécessaire pour que l’utilisateur cherche correctement l’appareil photo dans les codes-barres.  Windows fournit un aperçu de caméra simple qui lance une boîte de dialogue de contrôle de base du scanneur de codes-barres de l’appareil photo.  Appelez simplement [ClaimedBarcodeScanner. ShowVideoPreview](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.showvideopreviewasync) pour ouvrir la boîte de dialogue et [ClaimedBarcodeScanner. HideVideoPreview](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.hidevideopreview) pour la fermer une fois terminé.

> [!TIP]
> Consultez la version [préliminaire d’hébergement](pos-camerabarcode-hosting-preview.md) pour héberger la version préliminaire du scanneur de code-barres de l’appareil dans votre application.

## <a name="step-8-initiate-scan"></a>Étape 8 : lancer l’analyse

Vous pouvez lancer le processus d’analyse en appelant [**StartSoftwareTriggerAsync**](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync).

Selon la valeur de [**IsDisabledOnDataReceived**](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) , le scanneur peut analyser un seul code-barres, l’arrêter ou l’analyser en continu jusqu’à ce que vous appeliez [**StopSoftwareTriggerAsync**](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync).

Définissez la valeur souhaitée de [**IsDisabledOnDataReceived**](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) pour contrôler le comportement de l’analyseur lorsqu’un code-barres est décodé.

| Valeur | Description |
| ----- | ----------- |
| True   | Analyser un seul code-barres puis arrêter |
| Faux  | Analyser en continu les codes-barres sans les arrêter |

## <a name="see-also"></a>Voir aussi

### <a name="samples"></a>Exemples

- [Exemple de scanneur de codes-barres](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)