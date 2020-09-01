---
title: Énumération des appareils PointOfService
description: Découvrez quatre méthodes d’utilisation d’un sélecteur d’appareil pour interroger et énumérer les appareils PointOfService disponibles pour votre système.
ms.date: 10/08/2018
ms.topic: article
keywords: Windows 10, UWP, point de service, pos
ms.localizationpriority: medium
ms.openlocfilehash: 6d1767e49e14f424b7d21424fbfc02b76acd546a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159673"
---
# <a name="enumerating-point-of-service-devices"></a>Énumération des appareils de point de service
Dans cette section, vous allez apprendre à [définir un sélecteur d’appareil](./build-a-device-selector.md) utilisé pour interroger les appareils disponibles pour le système et à utiliser ce sélecteur pour énumérer les appareils de point de service à l’aide de l’une des méthodes suivantes :

**Méthode 1 :** [utiliser un sélecteur d’appareils](#method-1-use-a-device-picker)
<br/>
Afficher une interface utilisateur du sélecteur d’appareils et demander à l’utilisateur de choisir un appareil connecté. Cette méthode gère la mise à jour de la liste lorsque des appareils sont attachés et supprimés, et est plus simple et plus sûr que les autres méthodes.

**Méthode 2 :** [récupérer le premier appareil disponible](#method-2-get-first-available-device)<br />Utilisez [GetDefaultAsync](/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync) pour accéder au premier appareil disponible dans une classe de périphérique de point de service spécifique.

**Méthode 3 :** [capture instantanée des appareils](#method-3-snapshot-of-devices)<br />Énumérer un instantané des appareils de point de service présents sur le système à un moment donné. Cela est utile lorsque vous souhaitez créer votre propre interface utilisateur ou avoir besoin d’énumérer des appareils sans afficher d’interface utilisateur pour l’utilisateur. [FindAllAsync](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) conserve les résultats jusqu’à ce que l’énumération entière soit terminée.

**Méthode 4 :** [énumérer et surveiller](#method-4-enumerate-and-watch)<br />[DeviceWatcher](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) est un modèle d’énumération plus puissant et flexible qui vous permet d’énumérer les appareils actuellement présents et de recevoir des notifications lorsque des appareils sont ajoutés ou supprimés du système.  Cela est utile lorsque vous souhaitez conserver une liste actuelle d’appareils en arrière-plan pour afficher dans votre interface utilisateur au lieu d’attendre qu’un instantané se produise.

## <a name="define-a-device-selector"></a>Définir un sélecteur d’appareil
Un sélecteur d’appareils vous permet de limiter les appareils que vous recherchez lors de l’énumération des appareils.  Cela vous permettra d’obtenir uniquement les résultats pertinents et de réduire le temps nécessaire à l’énumération des appareils souhaités.

Vous pouvez utiliser la méthode **GetDeviceSelector** pour le type d’appareil que vous recherchez pour obtenir le sélecteur d’appareil pour ce type. Par exemple, l’utilisation de [PosPrinter. GetDeviceSelector](/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector#Windows_Devices_PointOfService_PosPrinter_GetDeviceSelector) vous fournira un sélecteur pour énumérer tous les [PosPrinters](/uwp/api/windows.devices.pointofservice.posprinter) attachés au système, y compris les imprimantes USB, réseau et de PDV Bluetooth.

```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector();
```

Les méthodes **GetDeviceSelector** pour les différents types d’appareils sont les suivantes :

* [BarcodeScanner.GetDeviceSelector](/uwp/api/windows.devices.pointofservice.barcodescanner.getdeviceselector)
* [CashDrawer.GetDeviceSelector](/uwp/api/windows.devices.pointofservice.cashdrawer.getdeviceselector)
* [LineDisplay.GetDeviceSelector](/uwp/api/windows.devices.pointofservice.linedisplay.getdeviceselector)
* [MagneticStripeReader.GetDeviceSelector](/uwp/api/windows.devices.pointofservice.magneticstripereader.getdeviceselector)
* [PosPrinter.GetDeviceSelector](/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector)

À l’aide d’une méthode **GetDeviceSelector** qui prend une valeur [PosConnectionTypes](/uwp/api/windows.devices.pointofservice.posconnectiontypes) en tant que paramètre, vous pouvez limiter votre sélecteur à l’énumération des périphériques de PDV locaux, réseau ou Bluetooth, ce qui réduit le temps nécessaire à l’exécution de la requête.  L’exemple ci-dessous illustre une utilisation de cette méthode pour définir un sélecteur qui ne prend en charge que les imprimantes POS attachées localement.

 ```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector(PosConnectionTypes.Local);
```

> [!TIP]
> Consultez [créer un sélecteur d’appareil](./build-a-device-selector.md) pour créer des chaînes de sélecteur plus avancées.

## <a name="method-1-use-a-device-picker"></a>Méthode 1 : utiliser un sélecteur d’appareils

La classe [DevicePicker](/uwp/api/windows.devices.enumeration.devicepicker) vous permet d’afficher un menu volant du sélecteur qui contient une liste d’appareils que l’utilisateur peut choisir. Vous pouvez utiliser la propriété [filtre](/uwp/api/windows.devices.enumeration.devicepicker.filter) pour choisir les types d’appareils à afficher dans le sélecteur. Cette propriété est de type [DevicePickerFilter](/uwp/api/windows.devices.enumeration.devicepickerfilter). Vous pouvez ajouter des types d’appareils au filtre à l’aide de la propriété [SupportedDeviceClasses](/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceclasses) ou [SupportedDeviceSelectors](/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceselectors) .

Lorsque vous êtes prêt à afficher le sélecteur d’appareil, vous pouvez appeler la méthode [PickSingleDeviceAsync](/uwp/api/windows.devices.enumeration.devicepicker.picksingledeviceasync) , qui affiche l’interface utilisateur du sélecteur et retourne l’appareil sélectionné. Vous devez spécifier un [Rect](/uwp/api/windows.foundation.rect) qui déterminera l’emplacement du lanceur. Cette méthode retourne un objet [DeviceInformation](/uwp/api/windows.devices.enumeration.deviceinformation) . par conséquent, pour l’utiliser avec les API point of service, vous devez utiliser la méthode **FromIdAsync** pour la classe d’appareil particulière souhaitée. Vous devez passer la propriété [DeviceInformation.ID](/uwp/api/windows.devices.enumeration.deviceinformation.id) comme paramètre *deviceId* de la méthode et obtenir une instance de la classe Device comme valeur de retour.

L’extrait de code suivant crée un **DevicePicker**, lui ajoute un filtre de scanneur de codes-barres, permet à l’utilisateur de sélectionner un appareil, puis crée un objet **BarcodeScanner** basé sur l’ID de l’appareil :

```cs
private async Task<BarcodeScanner> GetBarcodeScanner()
{
    DevicePicker devicePicker = new DevicePicker();
    devicePicker.Filter.SupportedDeviceSelectors.Add(BarcodeScanner.GetDeviceSelector());
    Rect rect = new Rect();
    DeviceInformation deviceInformation = await devicePicker.PickSingleDeviceAsync(rect);
    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(deviceInformation.Id);
    return barcodeScanner;
}
```

## <a name="method-2-get-first-available-device"></a>Méthode 2 : récupérer le premier appareil disponible

Le moyen le plus simple d’obtenir un appareil de point de service consiste à utiliser **GetDefaultAsync** pour obtenir le premier appareil disponible dans une classe de périphérique de point de service. 

L’exemple ci-dessous illustre l’utilisation de [GetDefaultAsync](/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync#Windows_Devices_PointOfService_BarcodeScanner_GetDefaultAsync) pour [BarcodeScanner](/uwp/api/windows.devices.pointofservice.barcodescanner). Le modèle de codage est similaire pour toutes les classes de périphérique de point de service.

```Csharp
using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();
```

> [!CAUTION]
> **GetDefaultAsync** doit être utilisé avec précaution, car il peut retourner un autre appareil d’une session à la suivante. De nombreux événements peuvent influencer cette énumération, ce qui se traduit par un autre périphérique disponible en premier, y compris : 
> - Modification des appareils photo connectés à votre ordinateur 
> - Modification des appareils de point de service attachés à votre ordinateur
> - Modification des appareils de point de service attachés au réseau disponibles sur votre réseau
> - Modification des appareils de point de service Bluetooth au sein de la plage de votre ordinateur 
> - Modifications apportées à la configuration du point de service 
> - Installation de pilotes ou d’objets de service OPOS
> - Installation des extensions de point de service
> - Mise à jour vers le système d’exploitation Windows

## <a name="method-3-snapshot-of-devices"></a>Méthode 3 : capture instantanée des appareils

Dans certains scénarios, vous souhaiterez peut-être créer votre propre interface utilisateur ou vous devez énumérer les appareils sans afficher d’interface utilisateur pour l’utilisateur.  Dans ces situations, vous pouvez énumérer un instantané des appareils actuellement connectés ou associés au système à l’aide de [DeviceInformation. FindAllAsync](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync).  Cette méthode retiendra les résultats jusqu’à ce que la totalité de l’énumération soit terminée.

> [!TIP]
> Il est recommandé d’utiliser la méthode **GetDeviceSelector** avec le paramètre **PosConnectionTypes** lors de l’utilisation de **FindAllAsync** pour limiter votre requête au type de connexion souhaité.  Les connexions réseau et Bluetooth peuvent retarder les résultats, car leurs énumérations doivent se terminer avant que les résultats de **FindAllAsync** soient retournés.

> [!CAUTION] 
> **FindAllAsync** retourne un tableau d’appareils.  L’ordre de ce tableau peut passer d’une session à l’autres. par conséquent, il n’est pas recommandé de s’appuyer sur un ordre spécifique à l’aide d’un index codé en dur dans le tableau.  Utilisez les propriétés de [DeviceInformation](/uwp/api/windows.devices.enumeration.deviceinformation) pour filtrer vos résultats ou fournissez une interface utilisateur pour permettre à l’utilisateur de choisir.

Cet exemple utilise le sélecteur défini ci-dessus pour prendre un instantané des appareils à l’aide de **FindAllAsync** , puis il énumère chacun des éléments retournés par la collection et écrit le nom et l’ID de l’appareil dans la sortie de débogage. 

```Csharp
using Windows.Devices.Enumeration;

DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
}
```

> [!TIP] 
> Lorsque vous utilisez les API [Windows. Devices. Enumeration](/uwp/api/Windows.Devices.Enumeration) , vous devez fréquemment utiliser des objets [DeviceInformation](/uwp/api/windows.devices.enumeration.deviceinformation) pour obtenir des informations sur un appareil spécifique. Par exemple, la propriété [DeviceInformation.ID](/uwp/api/windows.devices.enumeration.deviceinformation.id) peut être utilisée pour récupérer et réutiliser le même appareil s’il est disponible dans une session ultérieure et que la propriété [DeviceInformation.Name](/uwp/api/windows.devices.enumeration.deviceinformation.name) peut être utilisée à des fins d’affichage dans votre application.  Pour plus d’informations sur les propriétés supplémentaires disponibles, consultez la page de référence [DeviceInformation](/uwp/api/windows.devices.enumeration.deviceinformation) .

## <a name="method-4-enumerate-and-watch"></a>Méthode 4 : énumérer et surveiller

Une méthode plus puissante et plus flexible d’énumération des appareils consiste à créer un [DeviceWatcher](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher).  Un observateur d’appareils énumère les appareils de manière dynamique, de sorte que l’application reçoit des notifications si des appareils sont ajoutés, supprimés ou modifiés une fois l’énumération initiale terminée.  Un **DeviceWatcher** vous permet de détecter quand un appareil connecté au réseau est mis en ligne, qu’un périphérique Bluetooth est à portée de fonction et qu’un appareil connecté localement est débranché afin que vous puissiez prendre les mesures appropriées au sein de votre application.

Cet exemple utilise le sélecteur défini ci-dessus pour créer un **DeviceWatcher** et définit des gestionnaires d’événements pour les notifications [ajoutées](/uwp/api/windows.devices.enumeration.devicewatcher.added), [supprimées](/uwp/api/windows.devices.enumeration.devicewatcher.removed)et [mises à jour](/uwp/api/windows.devices.enumeration.devicewatcher.updated) . Vous devrez renseigner les détails des actions que vous souhaitez effectuer sur chaque notification.

```Csharp
using Windows.Devices.Enumeration;

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

> [!TIP]
> Pour plus d’informations sur l’utilisation d’un **DeviceWatcher** [, consultez énumérer et surveiller des appareils]( ./enumerate-devices.md#enumerate-and-watch-devices) .

## <a name="see-also"></a>Voir aussi
* [Prise en main du point de service](pos-basics.md)
* [DeviceInformation, classe](/uwp/api/windows.devices.enumeration.deviceinformation)
* [PosPrinter, classe](/uwp/api/windows.devices.pointofservice.posprinter)
* [Énumération PosConnectionTypes](/uwp/api/windows.devices.pointofservice.posconnectiontypes)
* [BarcodeScanner, classe](/uwp/api/windows.devices.pointofservice.barcodescanner)
* [DeviceWatcher, classe](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)

[!INCLUDE [feedback](./includes/pos-feedback.md)]