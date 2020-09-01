---
title: Utilisation du scanneur de codes-barres symbologies
description: Utilisez les API du scanneur de codes-barres UWP pour traiter les symbologies de code-barres sans configurer manuellement le scanneur.
ms.date: 08/29/2018
ms.topic: article
keywords: Windows 10, UWP, point de service, pos
ms.localizationpriority: medium
ms.openlocfilehash: a556178adcb7c2188787c04318a1eed4a3c35df4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159703"
---
# <a name="working-with-symbologies"></a>Utilisation des symbologies
Un [symbolisme de code-barres](/uwp/api/windows.devices.pointofservice.barcodesymbologies) correspond au mappage de données à un format de code-barres spécifique. Certaines symbologies courantes incluent UPC, Code 128, code QR, et ainsi de suite.  Les API de scanneur de codes-barres plateforme Windows universelle permettent à une application de contrôler la façon dont le scanneur traite ces symbologies sans configurer manuellement le scanneur. 

## <a name="determine-which-symbologies-are-supported"></a>Déterminer les symbologies prises en charge 
Étant donné que votre application peut être utilisée avec des modèles de scanneur de codes-barres différents de plusieurs fabricants, vous souhaiterez peut-être interroger le scanneur pour déterminer la liste des symbologies qu’il prend en charge.  Cela peut être utile si votre application nécessite un symbolisme spécifique qui n’est peut-être pas pris en charge par tous les scanneurs ou si vous devez activer des symbologies qui ont été désactivés manuellement ou par programmation sur le scanneur.

Une fois que vous avez un objet [BarcodeScanner](/uwp/api/windows.devices.pointofservice.barcodescanner) à l’aide de [BarcodeScanner. FromIdAsync](/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync), appelez [GetSupportedSymbologiesAsync](/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync#Windows_Devices_PointOfService_BarcodeScanner_GetSupportedSymbologiesAsync) pour obtenir la liste des symbologies prises en charge par l’appareil.

L’exemple suivant obtient une liste des symbologies pris en charge du scanneur de codes-barres et les affiche dans un bloc de texte :

```cs
private void DisplaySupportedSymbologies(BarcodeScanner barcodeScanner, TextBlock textBlock) 
{
    var supportedSymbologies = await barcodeScanner.GetSupportedSymbologiesAsync();

    foreach (uint item in supportedSymbologies)
    {
        string symbology = BarcodeSymbologies.GetName(item);
        textBlock.Text += (symbology + "\n");
    }
}
```

## <a name="determine-if-a-specific-symbology-is-supported"></a>Déterminer si un symbolisme spécifique est pris en charge
Pour déterminer si le scanneur prend en charge un symbolisme spécifique, vous pouvez appeler [IsSymbologySupportedAsync](/uwp/api/windows.devices.pointofservice.barcodescanner.issymbologysupportedasync#Windows_Devices_PointOfService_BarcodeScanner_IsSymbologySupportedAsync_System_UInt32_).

L’exemple suivant vérifie si le scanneur de codes-barres prend en charge le symbolisme **Code32** :

```cs
bool symbologySupported = await barcodeScanner.IsSymbologySupportedAsync(BarcodeSymbologies.Code32);
```

## <a name="change-which-symbologies-are-recognized"></a>Changer les symbologies reconnus
Dans certains cas, vous souhaiterez peut-être utiliser un sous-ensemble de symbologies pris en charge par le scanneur de codes-barres.  Cela est particulièrement utile pour bloquer les symbologies que vous n’envisagez pas d’utiliser dans votre application. Par exemple, pour s’assurer qu’un utilisateur analyse le code-barres droit, vous pouvez limiter l’analyse aux codes UPC ou EAN lors de l’acquisition des références SKU d’élément et limiter l’analyse au Code 128 lors de l’acquisition de numéros de série.

Une fois que vous connaissez le symbologies que votre scanneur prend en charge, vous pouvez définir le symbologies que vous souhaitez qu’il reconnaisse.  Cela peut être fait après avoir établi un objet [ClaimedBarcodeScanner](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) à l’aide de [ClaimScannerAsync](/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync). Vous pouvez appeler [SetActiveSymbologiesAsync](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setactivesymbologiesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetActiveSymbologiesAsync_Windows_Foundation_Collections_IIterable_System_UInt32__) pour activer un ensemble spécifique de symbologies alors que ceux qui sont omis de votre liste sont désactivés.

L’exemple suivant définit le symbologies actif d’un scanneur de codes-barres demandé sur [Code39](/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39#Windows_Devices_PointOfService_BarcodeSymbologies_Code39) et [Code39Ex](/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39ex):

```cs
private async void SetSymbologies(ClaimedBarcodeScanner claimedBarcodeScanner) 
{
    var symbologies = new List<uint>{ BarcodeSymbologies.Code39, BarcodeSymbologies.Code39Ex };
    await claimedBarcodeScanner.SetActiveSymbologiesAsync(symbologies);
}
```

## <a name="barcode-symbology-attributes"></a>Attributs de symbolisme de code-barres
Les différents codes-barres symbologies peuvent avoir des attributs différents, tels que la prise en charge de plusieurs longueurs de décodage, la transmission du chiffre de vérification à l’hôte dans le cadre des données brutes et la vérification de la validation des chiffres. Avec la classe [BarcodeSymbologyAttributes](/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes) , vous pouvez obtenir et définir ces attributs pour un [ClaimedBarcodeScanner](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) donné et un symbolisme de code-barres.

Vous pouvez obtenir les attributs d’une symbologie donnée avec [GetSymbologyAttributesAsync](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_). L’extrait de code suivant obtient les attributs du symbolisme UPCA pour un **ClaimedBarcodeScanner**.

```cs
BarcodeSymbologyAttributes barcodeSymbologyAttributes = 
    await claimedBarcodeScanner.GetSymbologyAttributesAsync(BarcodeSymbologies.Upca);
```

Une fois que vous avez fini de modifier les attributs et que vous êtes prêt à les définir, vous pouvez appeler [SetSymbologyAttributesAsync](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync). Cette méthode retourne une valeur **booléenne**qui est **true** si les attributs ont été correctement définis.

```cs
bool success = await claimedBarcodeScanner.SetSymbologyAttributesAsync(
    BarcodeSymbologies.Upca, barcodeSymbologyAttributes);
```

### <a name="restrict-scan-data-by-data-length"></a>Limiter les données d’analyse par longueur de données
Certains symbologies sont de longueur variable, comme le code 39 ou le Code 128.  Les codes-barres de ces symbologies peuvent être situés à proximité les uns des autres, contenant des données différentes, souvent de longueur spécifique. La définition de la longueur spécifique des données dont vous avez besoin peut empêcher des analyses non valides.

Avant de définir la longueur de décodage, vérifiez si la symbolisme de code-barres prend en charge plusieurs longueurs avec [IsDecodeLengthSupported](/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.isdecodelengthsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsDecodeLengthSupported). Une fois que vous savez qu’il est pris en charge, vous pouvez définir [DecodeLengthKind](/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelengthkind#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_DecodeLengthKind), qui est de type [BarcodeSymbologyDecodeLengthKind](/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind). Cette propriété peut prendre l’une des valeurs suivantes :

* **AnyLength**: décoder les longueurs de n’importe quel nombre.
* **Discret**: décodage des longueurs des caractères codés sur un octet [DecodeLength1](/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength1) ou [DecodeLength2](/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength2) .
* **Plage**: décoder les longueurs entre les caractères codés sur un octet **DecodeLength1** et **DecodeLength2** . L’ordre des **DecodeLength1** et des **DecodeLength2** n’a pas d’importance (peut être supérieur ou inférieur à l’autre).

Enfin, vous pouvez définir les valeurs de **DecodeLength1** et **DecodeLength2** pour contrôler la longueur des données dont vous avez besoin.

L’extrait de code suivant illustre la définition de la longueur de décodage :

```cs
private async Task<bool> SetDecodeLength(
    ClaimedBarcodeScanner scanner,
    uint symbology, 
    BarcodeSymbologyDecodeLengthKind kind, 
    uint decodeLength1, 
    uint decodeLength2)
{
    bool success = false;
    BarcodeSymbologyAttributes attributes = await scanner.GetSymbologyAttributesAsync(symbology);

    if (attributes.IsDecodeLengthSupported)
    {
        attributes.DecodeLengthKind = kind;
        attributes.DecodeLength1 = decodeLength1;
        attributes.DecodeLength2 = decodeLength2;
        success = await scanner.SetSymbologyAttributesAsync(symbology, attributes);
    }

    return success;
}
```

### <a name="check-digit-transmission"></a>Vérifier la transmission des chiffres

Un autre attribut que vous pouvez définir sur un symbolisme est de savoir si le chiffre de contrôle sera transmis à l’hôte dans le cadre des données brutes. Avant de définir ce paramètre, assurez-vous que le symbolisme prend en charge la transmission de la transmission digit avec [IsCheckDigitTransmissionSupported](/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionsupported). Ensuite, déterminez si l’option vérifier la transmission des chiffres est activée avec [IsCheckDigitTransmissionEnabled](/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionenabled).

L’extrait de code suivant illustre la définition de la transmission des chiffres de vérification :

```cs
private async Task<bool> SetCheckDigitTransmission(ClaimedBarcodeScanner scanner, uint symbology, bool isEnabled)
{
    bool success = false;
    BarcodeSymbologyAttributes attributes = await scanner.GetSymbologyAttributesAsync(symbology);

    if (attributes.IsCheckDigitTransmissionSupported)
    {
        attributes.IsCheckDigitTransmissionEnabled = isEnabled;
        success = await scanner.SetSymbologyAttributesAsync(symbology, attributes);
    }

    return success;
}
```

### <a name="check-digit-validation"></a>Vérifier la validation des chiffres

Vous pouvez également définir si le chiffre de contrôle de code-barres doit être validé. Avant de définir ce paramètre, assurez-vous que le symbolisme prend en charge la validation des chiffres avec [IsCheckDigitValidationSupported](/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationsupported). Ensuite, définissez si la validation de la vérification des chiffres est activée avec [IsCheckDigitValidationEnabled](/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationenabled).

L’extrait de code suivant illustre la définition de la validation des chiffres de vérification :

```cs
private async Task<bool> SetCheckDigitValidation(ClaimedBarcodeScanner scanner, uint symbology, bool isEnabled)
{
    bool success = false;
    BarcodeSymbologyAttributes attributes = await scanner.GetSymbologyAttributesAsync(symbology);

    if (attributes.IsCheckDigitValidationSupported)
    {
        attributes.IsCheckDigitValidationEnabled = isEnabled;
        success = await scanner.SetSymbologyAttributesAsync(symbology, attributes);
    }

    return success;
}
```

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>Voir aussi

* [Scanneur de codes-barres](pos-barcodescanner.md)
* [BarcodeSymbologies, classe](/uwp/api/windows.devices.pointofservice.barcodesymbologies)
* [BarcodeScanner, classe](/uwp/api/windows.devices.pointofservice.barcodescanner)
* [ClaimedBarcodeScanner, classe](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)
* [BarcodeSymbologyAttributes, classe](/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes)
* [Énumération BarcodeSymbologyDecodeLengthKind](/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind)