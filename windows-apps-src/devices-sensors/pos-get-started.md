---
title: Prise en main du point de service
description: Cet article contient des informations sur la prise en main du point de service Windows Runtime API.
ms.date: 05/02/2018
ms.topic: article
keywords: Windows 10, UWP, point de service, pos
ms.localizationpriority: medium
ms.openlocfilehash: f5f19d1337a7ae49f46ab65d8420fedb775eeb2f
ms.sourcegitcommit: ef723e3d6b1b67213c78da696838a920c66d5d30
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/02/2020
ms.locfileid: "82730384"
---
# <a name="getting-started-with-point-of-service"></a>Prise en main du point de service

Le point de service, le point de vente ou le point de service sont des périphériques informatiques utilisés pour faciliter les transactions de vente au détail. Parmi les exemples de périphériques de point de service figurent les registres de trésorerie électronique, les scanneurs de codes-barres, les lecteurs de bandes magnétiques et les imprimantes de réception.

Vous découvrirez ici les principes fondamentaux de l’interfaçage avec les périphériques de point de service à l’aide des API de point de service Windows Runtime. Nous allons aborder l’énumération des appareils, la vérification des fonctionnalités des appareils, la revendication des appareils et le partage des appareils. Nous utilisons un périphérique de scanneur de codes-barres comme exemple, mais presque tous les conseils ici s’appliquent à tous les appareils de point de service compatibles UWP. (Pour obtenir la liste des appareils pris en charge, voir [prise en charge des appareils de point de service](pos-device-support.md)).

## <a name="finding-and-connecting-to-point-of-service-peripherals"></a>Recherche et connexion aux périphériques de point de service

Avant qu’un point de service puisse être utilisé par une application, il doit être associé au PC sur lequel l’application s’exécute. Il existe plusieurs façons de se connecter à des appareils de point de service, soit par programme, soit par le biais de l’application paramètres.

### <a name="connecting-to-devices-by-using-the-settings-app"></a>Connexion à des appareils à l’aide de l’application paramètres
Lorsque vous connectez un appareil de point de service comme un scanneur de codes-barres à un PC, il s’affiche comme n’importe quel autre appareil. Vous pouvez le trouver dans la section **appareils > Bluetooth & autres appareils** de l’application paramètres. Vous pouvez associer un appareil de point de service en sélectionnant **Ajouter Bluetooth ou autre périphérique**.

Certains périphériques de point de service peuvent ne pas apparaître dans l’application paramètres jusqu’à ce qu’ils soient énumérés par programme à l’aide des API point of service.

### <a name="getting-a-single-point-of-service-device-with-getdefaultasync"></a>Obtention d’un point de service unique avec GetDefaultAsync
Dans un cas d’utilisation simple, vous pouvez avoir un seul périphérique de point de service connecté au PC sur lequel l’application s’exécute et que vous souhaitez le configurer le plus rapidement possible. Pour ce faire, récupérez l’appareil « par défaut » avec la méthode **GetDefaultAsync** , comme illustré ici.

```Csharp
using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();
```

Si le périphérique par défaut est trouvé, l’objet appareil récupéré est prêt à être réclamé. « Revendiquer » un appareil donne à une application un accès exclusif à celui-ci, ce qui empêche les commandes conflictuelles à partir de plusieurs processus.

> [!NOTE] 
> Si plusieurs appareils de point de service sont connectés au PC, **GetDefaultAsync** retourne le premier périphérique qu’il trouve. Pour cette raison, utilisez **FindAllAsync** , sauf si vous êtes certain qu’un seul appareil de point de service est visible pour l’application.

### <a name="enumerating-a-collection-of-devices-with-findallasync"></a>Énumération d’une collection d’appareils avec FindAllAsync

Quand vous êtes connecté à plusieurs appareils, vous devez énumérer la collection d’objets d’appareil **PointOfService** pour trouver celui que vous souhaitez revendiquer. Par exemple, le code suivant crée une collection de tous les scanneurs de codes-barres actuellement connectés, puis recherche un scanneur portant un nom spécifique dans la collection.

```Csharp
using Windows.Devices.Enumeration;
using Windows.Devices.PointOfService;

string selector = BarcodeScanner.GetDeviceSelector();       
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
    if (devInfo.Name.Contains("1200G"))
    {
        Debug.WriteLine(" Found one");
    }
}
```

### <a name="scoping-the-device-selection"></a>Définition de la portée de la sélection des appareils
Quand vous vous connectez à un appareil, vous souhaiterez peut-être limiter votre recherche à un sous-ensemble de périphériques de point de service auxquels votre application a accès. À l’aide de la méthode **GetDeviceSelector** , vous pouvez étendre la sélection pour récupérer des appareils connectés uniquement par une certaine méthode (Bluetooth, USB, etc.). Vous pouvez créer un sélecteur qui recherche des appareils sur **les types de connexion** **Bluetooth**, **IP**, **local**ou All. Cela peut être utile, car la détection des périphériques sans fil prend beaucoup de temps par rapport à la découverte locale (filaire). Vous pouvez garantir un temps d’attente déterministe pour la connexion de l’appareil local en limitant **FindAllAsync** aux types de connexion **locaux** . Par exemple, ce code récupère tous les scanneurs de codes-barres accessibles via une connexion locale. 

```Csharp
string selector = BarcodeScanner.GetDeviceSelector(PosConnectionTypes.Local);
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

### <a name="reacting-to-device-connection-changes-with-devicewatcher"></a>Réaction aux modifications des connexions des appareils avec DeviceWatcher

Lorsque votre application s’exécute, les appareils sont parfois déconnectés ou mis à jour, ou de nouveaux appareils doivent être ajoutés. Vous pouvez utiliser la classe **DeviceWatcher** pour accéder aux événements liés à l’appareil, afin que votre application puisse répondre en conséquence. Voici un exemple d’utilisation de **DeviceWatcher**, avec des stubs de méthode à appeler si un appareil est ajouté, supprimé ou mis à jour.

```Csharp
DeviceWatcher deviceWatcher = DeviceInformation.CreateWatcher(selector);
deviceWatcher.Added += DeviceWatcher_Added;
deviceWatcher.Removed += DeviceWatcher_Removed;
deviceWatcher.Updated += DeviceWatcher_Updated;

void DeviceWatcher_Added(DeviceWatcher sender, DeviceInformation args)
{
    // TODO: Add the DeviceInformation object to your collection
}

void DeviceWatcher_Removed(DeviceWatcher sender, DeviceInformationUpdate args)
{
    // TODO: Remove the item in your collection associated with DeviceInformationUpdate
}

void DeviceWatcher_Updated(DeviceWatcher sender, DeviceInformationUpdate args)
{
    // TODO: Update your collection with information from DeviceInformationUpdate
}
```

## <a name="checking-the-capabilities-of-a-point-of-service-device"></a>Vérification des fonctionnalités d’un appareil de point de service
Même dans une classe d’appareils, comme les scanneurs de codes-barres, les attributs de chaque périphérique peuvent varier considérablement d’un modèle à l’autre. Si votre application nécessite un attribut d’appareil spécifique, vous devrez peut-être inspecter chaque objet d’appareil connecté pour déterminer si l’attribut est pris en charge. Par exemple, votre entreprise a peut-être besoin de créer des étiquettes à l’aide d’un modèle d’impression de codes-barres spécifique. Voici comment vérifier si un scanneur de codes-barres connecté prend en charge un symbolisme. 

> [!NOTE]
> Un symbolisme est le mappage de langage utilisé par un code-barres pour encoder les messages.

```Csharp
try
{
    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(deviceId);
    if (await barcodeScanner.IsSymbologySupportedAsync(BarcodeSymbologies.Code32))
    {
        Debug.WriteLine("Has symbology");
    }
}
catch (Exception ex)
{
    Debug.WriteLine("FromIdAsync() - " + ex.Message);
}
```

### <a name="using-the-devicecapabilities-class"></a>Utilisation de la classe Device. Capabilities
La classe **Device. Capabilities** est un attribut de toutes les classes de périphérique de service et peut être utilisée pour obtenir des informations générales sur chaque appareil. Par exemple, cet exemple détermine si un appareil prend en charge les rapports de statistiques et, le cas échéant, récupère des statistiques pour tous les types pris en charge.

```Csharp
try
{
    if (barcodeScanner.Capabilities.IsStatisticsReportingSupported)
    {
        Debug.WriteLine("Statistics reporting is supported");

        string[] statTypes = new string[] {""};
        IBuffer ibuffer = await barcodeScanner.RetrieveStatisticsAsync(statTypes);
    }
}
catch (Exception ex)
{
    Debug.WriteLine("EX: RetrieveStatisticsAsync() - " + ex.Message);
}
```

## <a name="claiming-a-point-of-service-device"></a>Revendication d’un point de service
Avant de pouvoir utiliser un appareil de point de service pour une entrée ou une sortie active, vous devez le réclamer, en accordant à l’application un accès exclusif à la plupart de ses fonctions. Ce code montre comment revendiquer un périphérique de scanneur de codes-barres, une fois que vous avez trouvé l’appareil à l’aide de l’une des méthodes décrites précédemment.

```Csharp
try
{
    claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();
}
catch (Exception ex)
{
    Debug.WriteLine("EX: ClaimScannerAsync() - " + ex.Message);
}
```

### <a name="retaining-the-device"></a>Conservation de l’appareil
Lorsque vous utilisez un appareil de point de service sur un réseau ou une connexion Bluetooth, vous souhaiterez peut-être partager l’appareil avec d’autres applications sur le réseau. (Pour plus d’informations à ce sujet, consultez [partage d’appareils](#sharing-a-device-between-apps).) Dans d’autres cas, vous pouvez souhaiter conserver l’appareil pour une utilisation prolongée. Cet exemple montre comment conserver un scanneur de codes-barres revendiqué après qu’une autre application a demandé que l’appareil soit libéré.

```Csharp
claimedBarcodeScanner.ReleaseDeviceRequested += claimedBarcodeScanner_ReleaseDeviceRequested;

void claimedBarcodeScanner_ReleaseDeviceRequested(object sender, ClaimedBarcodeScanner e)
{
    e.RetainDevice();  // Retain exclusive access to the device
}
```

## <a name="input-and-output"></a>Entrée et sortie

Une fois que vous avez demandé un appareil, vous êtes presque prêt à l’utiliser. Pour recevoir l’entrée de l’appareil, vous devez configurer et permettre à un délégué de recevoir des données. Dans l’exemple ci-dessous, nous revendiquons un périphérique de scanneur de codes-barres, nous définissons sa propriété Decode, puis nous appelons **EnableAsync** pour activer l’entrée décodée à partir de l’appareil. Ce processus varie selon les classes d’appareils. ainsi, pour obtenir des conseils sur la configuration d’un délégué pour les appareils non-code-barres, reportez-vous à l' [exemple d’application UWP](https://github.com/Microsoft/Windows-universal-samples#devices-and-sensors)appropriée.

```Csharp
try
{
    claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();
    if (claimedBarcodeScanner != null)
    {
        claimedBarcodeScanner.DataReceived += claimedBarcodeScanner_DataReceived;
        claimedBarcodeScanner.IsDecodeDataEnabled = true;
        await claimedBarcodeScanner.EnableAsync();
    }
}
catch (Exception ex)
{
    Debug.WriteLine("EX: ClaimScannerAsync() - " + ex.Message);
}


void claimedBarcodeScanner_DataReceived(ClaimedBarcodeScanner sender, BarcodeScannerDataReceivedEventArgs args)
{
    string symbologyName = BarcodeSymbologies.GetName(args.Report.ScanDataType);
    var scanDataLabelReader = DataReader.FromBuffer(args.Report.ScanDataLabel);
    string barcode = scanDataLabelReader.ReadString(args.Report.ScanDataLabel.Length);
}
```

## <a name="sharing-a-device-between-apps"></a>Partage d’un appareil entre les applications

Les appareils de point de service sont souvent utilisés dans les cas où plusieurs applications devront y accéder sur une courte période.  Un appareil peut être partagé lorsqu’il est connecté à plusieurs applications localement (USB ou autre connexion câblée), ou via un réseau Bluetooth ou IP. En fonction des besoins de chaque application, un processus peut avoir besoin de supprimer sa revendication sur l’appareil. Ce code supprime notre périphérique de scanneur de codes-barres demandé, ce qui permet à d’autres applications de les revendiquer et de les utiliser.

```Csharp
if (claimedBarcodeScanner != null)
{
    claimedBarcodeScanner.Dispose();
    claimedBarcodeScanner = null;
}
```

> [!NOTE]
> Les classes d’appareils de point de service réclamées et non revendiquées implémentent l' [interface IClosable](https://docs.microsoft.com/uwp/api/windows.foundation.iclosable). Si un appareil est connecté à une application via réseau ou Bluetooth, les objets revendiqués et non revendiqués doivent être supprimés avant qu’une autre application puisse se connecter.

## <a name="see-also"></a>Voir aussi
+ [Exemple de scanneur de codes-barres](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [Exemple de tiroir-caisse]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [Exemple d’affichage de ligne](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [Exemple de lecteur de bande magnétique](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [Exemple POSPrinter](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)

