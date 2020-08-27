---
title: Scanneur de codes-barres de l’appareil symbologies
description: Affichez des exemples de codes-barres pour chacun des symbologies pris en charge par le décodeur de codes-barres logiciel fourni avec Windows 10.
ms.date: 05/02/2018
ms.topic: article
keywords: Windows 10, UWP, point de service, pos
ms.localizationpriority: medium
ms.openlocfilehash: a9618402a6ee76a20ff5f95418ee7280b39db4a2
ms.sourcegitcommit: 8e0e4cac79554e86dc7f035c4b32cb1f229142b0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88943129"
---
# <a name="symbologies"></a>Symbologies

Cette rubrique fournit des exemples de codes-barres pour chaque symbologies pris en charge par le décodeur de code-barres logiciel fourni avec Windows 10, notamment : UPC/EAN, code 39, Code 128, entrelacé 2 sur 5, barre omnidirectionnel, barre empilé, code QR et GS1DWCode.

Windows 10 utilise une caméra d’objectif standard associée à un décodeur logiciel pour générer un scanneur de codes-barres. Cet article fait référence au symbologies pris en charge par le décodeur logiciel. Des symbologies supplémentaires peuvent être pris en charge par les périphériques de scanneur de codes-barres dédiés qui comportent des décodeurs matériels intégrés. pour plus d’informations, contactez le fabricant de votre scanneur de codes-barres. Les symbologies répertoriés sont pris en charge dans toutes les éditions de Windows 10 Build 17134 ou version ultérieure, sauf indication contraire.

Utilisez [GetSupportedSymbologiesAsync](/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync) pour déterminer le symbologies spécifique pris en charge par un scanneur de codes-barres.

> [!NOTE]
> Le décodeur logiciel intégré à Windows 10 est fourni par la [*société Digimarc*](https://www.digimarc.com/).

## <a name="1d-symbologies"></a>1J symbologies

### <a name="code-39"></a>Code 39
![Exemple de code-barres-Code 39](images/pos/sample-barcode-code39.png)

### <a name="code-128"></a>Code 128
![Exemple de code-barres-Code 128](images/pos/sample-barcode-code128.png)

### <a name="databar-omnidirectional"></a>Barre omnidirectionnel
![Exemple de code-barres-barre omnidirectionnel](images/pos/sample-barcode-databar-omnidirectional.png) 
### <a name="databar-stacked"></a>Barre empilé
![Exemple de code-barres-barre empilé](images/pos/sample-barcode-databar-stacked.png)

### <a name="ean-8"></a>EAN-8
![Exemple de code-barres-EAN-8](images/pos/sample-barcode-ean8.png)

### <a name="ean-13"></a>EAN-13
![Exemple de code-barres-EAN-13](images/pos/sample-barcode-ean13.png)

### <a name="interleaved-2-of-5"></a>Entrelacé 2/5
![Exemple de code-barres-entrelacé 2 sur 5](images/pos/sample-barcode-interleaved-2-of-5.png)

### <a name="upc-a"></a>UPC-A
![Exemple de code-barres-UPC A](images/pos/sample-barcode-upca.png)

### <a name="upc-e"></a>UPC-E
![Exemple de code-barres-UPC E](images/pos/sample-barcode-upce.png)

## <a name="2d-symbologies"></a>Symbologies 2D
### <a name="qr-code"></a>Code QR
![Exemple de code-barres-code QR](images/pos/sample-barcode-qrcode.png)

## <a name="digital-watermark"></a>Filigrane numérique
### <a name="gs1-dwcode"></a>GS1-DWCode

Scanner l’image d’un package ci-dessous avec votre application de scanneur de codes-barres de l’appareil photo pour voir GS1DWCode en action.  L’image est encodée avec UPCA 856107006854.  http://www.digimarc.comPour plus d’informations sur les fonctionnalités de GS1DWCode, consultez.

![Exemple de code-barres-GS1DWCode](images/pos/Rice-Box-V7.jpg)

## <a name="see-also"></a>Voir aussi

### <a name="samples"></a>exemples

- [Exemple de scanneur de codes-barres](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
