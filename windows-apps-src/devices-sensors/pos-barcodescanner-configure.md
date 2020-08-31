---
title: Configurer un scanneur de code-barres
description: Découvrez comment configurer un scanneur de codes-barres pour l’application prévue.
ms.date: 08/29/2018
ms.topic: article
keywords: Windows 10, UWP, point de service, pos
ms.localizationpriority: medium
ms.openlocfilehash: 96deeb82f0aa04929ac33001b1aee0603e6474c7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168503"
---
# <a name="configure-a-barcode-scanner"></a>Configurer un scanneur de code-barres

Les scanneurs de codes-barres peuvent être configurés en plusieurs modes différents.  Il est important que votre scanneur de codes-barres soit configuré correctement pour l’application prévue.

De nombreux scanneurs de codes-barres peuvent être configurés en mode de **coin du clavier** , ce qui fait apparaître le scanneur de codes-barres comme un clavier pour Windows.  Cela vous permet d’analyser les codes-barres dans des applications qui ne prennent pas en charge le scanneur de codes-barres, telles que le bloc-notes.  Lorsque vous Scannez un code-barres dans ce mode, les données décodées du scanneur de codes-barres sont insérées à votre point d’insertion comme si vous tapiiez les données à l’aide du clavier.  Si vous souhaitez davantage de contrôle sur votre scanneur de codes-barres à partir de votre application UWP, vous devez le configurer en mode de coin non-clavier.

## <a name="usb-barcode-scanner"></a>Scanneur de codes-barres USB
Un scanneur de codes-barres connecté USB doit être configuré en mode **scanneur de PDV HID** pour fonctionner avec le pilote de scanneur de codes-barres inclus dans Windows. Ce pilote est une implémentation de la spécification de **tables d’utilisation de point de vente HID** publiée sur [HID USB](https://www.usb.org/hid).  Reportez-vous à la documentation de votre scanner de codes-barres ou contactez le fabricant de votre scanneur de codes-barres pour obtenir des instructions sur l’activation du mode d' **analyse HID POS** .  Une fois configuré en tant que **scanneur de PDV HID** , votre scanneur de codes-barres s’affiche dans Device Manager sous le nœud du **scanneur de codes-barres POS** en tant que **scanneur de codes-barres HID POS**.

Le fabricant de votre scanneur de codes-barres peut également avoir un pilote spécifique au fournisseur qui prend en charge les API du scanneur de codes-barres UWP à l’aide d’un autre mode que le **scanneur de PDV HID**.  Si vous avez déjà installé un pilote fourni par le fabricant et compatible avec les API du scanneur de codes-barres UWP, vous pouvez voir un appareil spécifique au fournisseur dans la liste sous **scanneur de codes-barres POS** dans Device Manager.

## <a name="bluetooth-barcode-scanner"></a>Scanneur de codes-barres Bluetooth
Un scanneur connecté Bluetooth doit être configuré en mode de l' **interface série simple (spp-standard) du protocole de port série** pour fonctionner avec les API du scanneur de codes-barres UWP.  Reportez-vous à la documentation de votre scanner de codes-barres ou contactez le fabricant de votre scanneur de codes-barres pour obtenir des instructions pour activer le **mode spp-SSI**.

Avant de pouvoir utiliser votre scanneur de codes-barres Bluetooth, vous devez le coupler à l’aide de **paramètres > appareils > bluetooth & d’autres appareils > ajouter Bluetooth ou un autre appareil**.

Vous pouvez lancer et contrôler le processus de jumelage à l’aide de l’espace de noms [Windows. Devices. Enumeration](/uwp/api/windows.devices.enumeration) .  Pour plus d’informations, consultez [associer des appareils](./pair-devices.md) .

[!INCLUDE [feedback](./includes/pos-feedback.md)]