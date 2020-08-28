---
title: Obtenir et comprendre les données de code-barres
description: Découvrez comment obtenir des données à partir d’un scanneur de codes-barres dans un objet BarcodeScannerReport et comment comprendre son format et son contenu.
ms.date: 08/29/2018
ms.topic: article
keywords: Windows 10, UWP, point de service, pos
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 5d2ab873774ce8b48252b82ce6cf7521165554d4
ms.sourcegitcommit: cb5af00af05e838621c270173e7fde1c5d2168ef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89043441"
---
# <a name="obtain-and-understand-barcode-data"></a>Obtenir et comprendre les données de code-barres

Une fois que vous avez configuré votre scanneur de codes-barres, vous devez bien entendu avoir un moyen de comprendre les données que vous analysez. Lorsque vous analysez un code-barres, l’événement [DataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.datareceived) est déclenché. [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) doit s’abonner à cet événement. L’événement **DataReceived** passe un objet [BarcodeScannerDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs) , que vous pouvez utiliser pour accéder aux données de code-barres.

## <a name="subscribe-to-the-datareceived-event"></a>S’abonner à l’événement DataReceived

Une fois que vous disposez d’un **ClaimedBarcodeScanner**, abonnez-vous à l’événement **DataReceived** :

```cs
claimedBarcodeScanner.DataReceived += ClaimedBarcodeScanner_DataReceived;
```

Le gestionnaire d’événements reçoit le **ClaimedBarcodeScanner** et un objet **BarcodeScannerDataReceivedEventArgs** . Vous pouvez accéder aux données de code-barres par le biais de la propriété de [rapport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs.report#Windows_Devices_PointOfService_BarcodeScannerDataReceivedEventArgs_Report) de cet objet, qui est de type [BarcodeScannerReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport).

```cs
private async void ClaimedBarcodeScanner_DataReceived(ClaimedBarcodeScanner sender, BarcodeScannerDataReceivedEventArgs args)
{
    // Parse the data
}
```

## <a name="get-the-data"></a>Obtenir les données

Une fois que vous disposez de la **BarcodeScannerReport**, vous pouvez accéder aux données de code-barres et les analyser. **BarcodeScannerReport** a trois propriétés :

* [ScanData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandata): données de code-barres brutes complètes.
* [ScanDataLabel](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatalabel): étiquette de code-barres décodée, qui n’inclut pas l’en-tête, la somme de contrôle et d’autres informations diverses.
* [ScanDataType](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatatype): type d’étiquette de code-barres décodé. Les valeurs possibles sont définies dans la classe [BarcodeSymbologies](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies) .

Si vous souhaitez accéder à **ScanDataLabel** ou **ScanDataType**, vous devez d’abord définir [IsDecodeDataEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdecodedataenabled#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDecodeDataEnabled) sur **true**.

```cs
claimedBarcodeScanner.IsDecodeDataEnabled = true;
```

### <a name="get-the-scan-data-type"></a>Obtient le type de données de l’analyse

L’obtention du type d’étiquette de code-barres décodé est assez simple &mdash; . nous appelons simplement [GetName](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname) sur **ScanDataType**.

```cs
private string GetSymbology(BarcodeScannerDataReceivedEventArgs args)
{
    return BarcodeSymbologies.GetName(args.Report.ScanDataType);
}
```

### <a name="get-the-scan-data-label"></a>Obtient l’étiquette des données d’analyse

Pour obtenir l’étiquette de code-barres décodée, vous devez connaître quelques éléments. Seuls certains types de données contiennent du texte encodé. par conséquent, vous devez d’abord vérifier si la symbolisme peut être convertie en chaîne, puis convertir la mémoire tampon obtenue à partir de **ScanDataLabel** en une chaîne encodée en UTF-8.

```cs
private string GetDataLabel(BarcodeScannerDataReceivedEventArgs args)
{
    uint scanDataType = args.Report.ScanDataType;

    // Only certain data types contain encoded text.
    // To keep this simple, we'll just decode a few of them.
    if (args.Report.ScanDataLabel == null)
    {
        return "No data";
    }

    // This is not an exhaustive list of symbologies that can be converted to a string.
    else if (scanDataType == BarcodeSymbologies.Upca ||
        scanDataType == BarcodeSymbologies.UpcaAdd2 ||
        scanDataType == BarcodeSymbologies.UpcaAdd5 ||
        scanDataType == BarcodeSymbologies.Upce ||
        scanDataType == BarcodeSymbologies.UpceAdd2 ||
        scanDataType == BarcodeSymbologies.UpceAdd5 ||
        scanDataType == BarcodeSymbologies.Ean8 ||
        scanDataType == BarcodeSymbologies.TfStd)
    {
        // The UPC, EAN8, and 2 of 5 families encode the digits 0..9
        // which are then sent to the app in a UTF8 string (like "01234").
        return CryptographicBuffer.ConvertBinaryToString(BinaryStringEncoding.Utf8, args.Report.ScanDataLabel);
    }

    // Some other symbologies (typically 2-D symbologies) contain binary data that
    // should not be converted to text.
    else
    {
        return "Decoded data unavailable.";
    }
}
```

### <a name="get-the-raw-scan-data"></a>Récupérer les données d’analyse brutes

Pour obtenir les données brutes complètes du code-barres, nous convertissons simplement la mémoire tampon que nous obtenons de **ScanData** dans une chaîne.

```cs
private string GetRawData(BarcodeScannerDataReceivedEventArgs args)
{
    // Get the full, raw barcode data.
    if (args.Report.ScanData == null)
    {
        return "No data";
    }

    // Just to show that we have the raw data, we'll print the value of the bytes.
    else
    {
        return CryptographicBuffer.ConvertBinaryToString(BinaryStringEncoding.Utf8, args.Report.ScanData);
    }
}
```

Ces données sont, en général, au format fourni par le scanneur. Les informations d’en-tête de message et de code de fin sont toutefois supprimées, car elles ne contiennent pas d’informations utiles pour une application et sont susceptibles d’être spécifiques au scanneur.

Les informations d’en-tête courantes sont un caractère de préfixe (tel qu’un caractère STX). Les informations de code de fin courantes sont un caractère de marque de fin (par exemple, un caractère ETX ou CR) et un caractère de vérification de bloc s’il est généré par le scanneur.

Cette propriété doit inclure un symbole de symbolisme s’il est retourné par le scanneur (par exemple, **un pour UPC** -a). Il doit également inclure des chiffres de vérification s’ils sont présents dans l’étiquette et retournés par le scanneur. (Notez que les caractères de symbolisme et les chiffres de vérification peuvent être présents ou non, selon la configuration de l’analyseur. Le scanneur les retourne s’il est présent, mais ne les génère pas ou les calcule s’ils sont absents.

Certaines marchandises peuvent être marquées avec un code-barres supplémentaire. Ce code-barres est généralement placé à droite du code-barres principal et se compose d’un ou de cinq caractères supplémentaires. Si le scanner lit des marchandises qui contiennent à la fois des codes-barres principal et des codes-barres supplémentaires, les caractères supplémentaires sont ajoutés aux caractères principaux et le résultat est remis à l’application sous la forme d’une étiquette. (Notez qu’un scanneur peut prendre en charge une configuration qui active ou désactive la lecture de codes supplémentaires.)

Certaines marchandises peuvent être marquées avec plusieurs étiquettes, parfois appelées *étiquettes multisymboles* ou étiquettes à plusieurs *niveaux*. Ces codes-barres sont généralement organisés verticalement et peuvent être du même symbolisme ou d’un autre symbolisme. Si le scanner lit des marchandises qui contiennent plusieurs étiquettes, chaque code-barres est remis à l’application sous la forme d’une étiquette distincte. Cela est nécessaire en raison de l’absence de normalisation actuelle de ces types de codes-barres. L’un n’est pas en mesure de déterminer toutes les variations en fonction des données de code-barres individuelles. Par conséquent, l’application doit déterminer quand un code-barres à plusieurs étiquettes a été lu en fonction des données retournées. (Notez qu’un scanneur peut ou non prendre en charge la lecture de plusieurs étiquettes.)

Cette valeur est définie avant qu’un événement **DataReceived** soit déclenché à l’application.

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>Voir aussi
* [Scanneur de codes-barres](pos-barcodescanner.md)
* [ClaimedBarcodeScanner, classe](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname)
* [BarcodeScannerDataReceivedEventArgs, classe](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs)
* [BarcodeScannerReport, classe](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport)
* [BarcodeSymbologies, classe](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)
