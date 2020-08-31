---
ms.assetid: ECE848C2-33DE-46B0-BAE7-647DB62779BB
title: Étalonner les capteurs
description: Dans un appareil basé sur le magnétomètre (la boussole, l’inclinomètre et le capteur d’orientation), il peut s’avérer nécessaire d’étalonner les capteurs en raison de facteurs environnementaux.
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0ef2c1cfbe949e5f850a494e4f1ed9c79faf6050
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168573"
---
# <a name="calibrate-sensors"></a>Étalonner les capteurs


**API importantes**

-   [**Windows.Devices.Sensors**](/uwp/api/Windows.Devices.Sensors)
-   [**Windows. Devices. Sensors. Custom**](/uwp/api/Windows.Devices.Sensors.Custom)

Dans un appareil basé sur le magnétomètre (la boussole, l’inclinomètre et le capteur d’orientation), il peut s’avérer nécessaire d’étalonner les capteurs en raison de facteurs environnementaux. L’énumération [**MagnetometerAccuracy**](/uwp/api/Windows.Devices.Sensors.MagnetometerAccuracy) peut vous aider à déterminer la marche à suivre lorsque votre appareil est en cours d’étalonnage.

## <a name="when-to-calibrate-the-magnetometer"></a>Quand étalonner le magnétomètre

L’énumération [**MagnetometerAccuracy**](/uwp/api/Windows.Devices.Sensors.MagnetometerAccuracy) a quatre valeurs qui vous aident à déterminer si l’appareil sur lequel votre application s’exécute doit être étalonné. Si un appareil doit être étalonné, vous devez faire savoir à l’utilisateur que cette opération est nécessaire. Toutefois, vous ne devez pas demander trop souvent à l’utilisateur d’effectuer un étalonnage. Nous recommandons une fréquence maximale d’une fois toutes les 10 minutes.

| Valeur           | Description    |
| ----------------- | ------------------- |
| **Unknown**     | Le pilote de capteur n’a pas pu signaler la précision actuelle. Cela ne signifie pas nécessairement que l’appareil n’est pas étalonné. Votre application doit décider de la meilleure marche à suivre si la valeur **Unknown** est retournée. Si votre application dépend d’une lecture précise des capteurs, vous pouvez demander à l’utilisateur d’étalonner l’appareil. |
| **Non fiable**  | Il existe actuellement un degré élevé d’inexactitude dans le magnétomètre. Les applications doivent toujours demander un étalonnage par l’utilisateur quand cette valeur est retournée pour la première fois. |
| **Approximatif** | Les données sont suffisamment précises pour certaines applications. Une application de réalité virtuelle, qui doit uniquement savoir si l’utilisateur a déplacé l’appareil vers le haut/bas ou vers la gauche/droite, peut continuer sans étalonnage. Les applications qui ont besoin d’un en-tête absolu, comme une application de navigation qui doit connaître la direction dans laquelle vous conduisez pour vous indiquer des directions, doivent demander un étalonnage. |
| **Importante**        | Les données sont précises. Aucun étalonnage n’est nécessaire, même pour les applications qui doivent connaître un en-tête absolu, telles que la réalité augmentée ou les applications de navigation. |