---
ms.assetid: 404783BA-8859-4BFB-86E3-3DD2042E66F5
title: BlueTooth
description: Cette section contient des articles sur l’intégration du Bluetooth aux applications de la plateforme Windows universelle (UWP), notamment sur l’utilisation des API RFCOMM, GATT et des publications Bluetooth Low Energy (LE).
ms.date: 06/26/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: aad368adf500c5968bf6374a5855a7e0dab0a044
ms.sourcegitcommit: 015291bdf2e7d67076c1c85fc025f49c840ba475
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85469564"
---
# <a name="bluetooth"></a>BlueTooth
Cette section contient des articles sur l’intégration de Bluetooth dans des applications plateforme Windows universelle (UWP). Il existe deux technologies Bluetooth différentes que vous pouvez choisir d’implémenter dans votre application.

> [!Important]
> Vous devez déclarer la fonctionnalité « Bluetooth » dans *Package. appxmanifest*.
>
> `<Capabilities> <DeviceCapability Name="bluetooth" /> </Capabilities>`

## <a name="classic-bluetooth-rfcomm"></a>Bluetooth classique (RFCOMM)
Avant Bluetooth LE, les appareils utilisaient couramment ce protocole pour communiquer à l’aide de Bluetooth. Ce protocole est simple et utile pour la communication appareil-à-appareil sans avoir recours à des économies d’énergie. Pour plus d’informations sur ce protocole, y compris des exemples de code, consultez la rubrique [Bluetooth RFCOMM](send-or-receive-files-with-rfcomm.md) .

## <a name="bluetooth-low-energy-le"></a>Bluetooth basse énergie (LE)
Bluetooth Low Energy (LE) est une spécification qui définit des protocoles pour la découverte et la communication entre les appareils qui ont des besoins d’utilisation énergétique efficaces. Pour plus d’informations, y compris des exemples de code, consultez la rubrique relative à la [faible énergie Bluetooth](bluetooth-low-energy-overview.md) .

## <a name="see-also"></a>Voir aussi
- [FAQ sur le Bluetooth pour les développeurs](bluetooth-dev-faq.md)