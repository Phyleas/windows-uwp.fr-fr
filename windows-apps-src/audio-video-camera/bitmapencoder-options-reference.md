---
ms.assetid: 98BD79B3-F420-43C5-98D3-52EBDDB479A0
description: Cet article répertorie les options de codage qui peuvent être utilisées avec BitmapEncoder.
title: Référence des options BitmapEncoder
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 212f405c943354618a4ae2bf6f2ab9c4925e7085
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161083"
---
# <a name="bitmapencoder-options-reference"></a>Référence des options BitmapEncoder


Cet article répertorie les options d’encodage qui peuvent être utilisées avec [**BitmapEncoder**](/uwp/api/Windows.Graphics.Imaging.BitmapEncoder). Une option d’encodage est définie par son nom, qui est une chaîne, et une valeur dans un type de données particulier ([**Windows. Foundation. PropertyType**](/uwp/api/Windows.Foundation.PropertyType)). Pour plus d’informations sur l’utilisation des images, voir [Créer, modifier et enregistrer des images bitmap](imaging.md).

| Nom                    | Type de propriété | Notes d’utilisation                                                                                        | Formats valides |
|-------------------------|--------------|----------------------------------------------------------------------------------------------------|---------------|
| ImageQuality            | unique       | Valeurs valides de 0 à 1.0. Des valeurs plus élevées indiquent une meilleure qualité                                 | JPEG, JPEG-XR |
| CompressionQuality      | unique       | Valeurs valides de 0 à 1.0. Des valeurs plus élevées indiquent un schéma de compression plus efficace et plus lent. | TIFF          |
| Lossless                | boolean      | Si cette propriété a la valeur true, l’option ImageQuality est ignorée                                        | JPEG-XR       |
| InterlaceOption         | boolean      | Indique s’il faut ou non entrelacer l’image.                                                                    | PNG           |
| FilterOption            | uint8        | Utiliser l’énumération [**PngFilterMode**](/uwp/api/Windows.Graphics.Imaging.PngFilterMode)                                | PNG           |
| TiffCompressionMethod   | uint8        | Utiliser l’énumération [**TiffCompressionMode**](/uwp/api/Windows.Graphics.Imaging.TiffCompressionMode)                    | TIFF          |
| Luminance               | uint32Array  | Tableau de 64 éléments comprenant des constantes de quantification de la luminance                               | JPEG          |
| Chrominance             | uint32Array  | Tableau de 64 éléments comprenant des constantes de quantification de la chrominance                             | JPEG          |
| JpegYCrCbSubsampling    | uint8        | Utiliser l’énumération [**JpegSubsamplingMode**](/uwp/api/Windows.Graphics.Imaging.JpegSubsamplingMode)                    | JPEG          |
| SuppressApp0            | boolean      | Indique s’il faut ou non supprimer la création d’un bloc de métadonnées App0.                                        | JPEG          |
| EnableV5Header32bppBGRA | boolean      | Indique s’il faut ou non coder dans une version 5 BMP avec prise en charge alpha.                                         | BMP           |

 

## <a name="related-topics"></a>Rubriques connexes

* [Créer, modifier et enregistrer des images bitmap](imaging.md)
* [Codecs pris en charge](supported-codecs.md)

 