---
ms.assetid: 3b75d881-bdcf-402b-a330-23cd29d68e53
description: Cet article répertorie les propriétés DeviceInformation liées aux appareils audio
title: Propriétés d’informations des appareils audio
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6181625690b4abddd4fdd4cbd9032bee64b6658d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161203"
---
# <a name="audio-device-information-properties"></a>Propriétés d’informations des appareils audio

Cet article répertorie les propriétés d’informations liées aux appareils audio Sur Windows, chaque périphérique matériel a des propriétés [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) associées qui fournissent des informations détaillées sur un appareil que vous pouvez utiliser lorsque vous avez besoin d’informations spécifiques sur l’appareil ou lorsque vous créez un sélecteur d’appareil. Pour obtenir des informations générales sur l’énumération des appareils sur Windows, voir [Énumérer les appareils](../devices-sensors/enumerate-devices.md) et [Propriétés d’informations d’appareil](../devices-sensors/device-information-properties.md).


|Nom|Type|Description|
|------------------------------------------------------------|------------|------------------------------------------------------|
|**System.Devices.AudioDevice.Microphone.SensitivityInDbfs**|Double|Spécifie la sensibilité du microphone en décibels par rapport aux décibels pleine échelle (dBFS).|
|**System. Devices. AudioDevice. microphone. SignalToNoiseRatioInDb**|Double|Spécifie le ratio entre le signal du microphone  et le bruit mesuré en décibels (dB).|
|**System.Devices.AudioDevice.SpeechProcessingSupported**|Boolean|Indique si l’appareil audio prend en charge le traitement de la parole.|
|**System.Devices.AudioDevice.RawProcessingSupported**|Boolean|Indique si l’appareil audio prend en charge le traitement des fichiers bruts.|
|**System.Devices.MicrophoneArray.Geometry**|unsigned char[]|Données géométriques pour un réseau de microphones.|

## <a name="related-topics"></a>Rubriques connexes

* [Énumérer les appareils](../devices-sensors/enumerate-devices.md)
* [Propriétés d’informations de périphérique](../devices-sensors/device-information-properties.md)
* [Créer un sélecteur d’appareil](../devices-sensors/build-a-device-selector.md)
* [Lecture de contenu multimédia](media-playback.md)