---
title: Comment étirer l’IU vers le bord de l’écran
description: Apprenez à désactiver les bordures par défaut placées aux bords de la fenêtre d’affichage et à dessiner votre interface utilisateur sur les bords de l’écran.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 1adb221f-6f70-4255-9329-2046a486ca45
ms.localizationpriority: medium
ms.openlocfilehash: d34da1bbf129358f4549b3a4a04a7c3f84f872ab
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154843"
---
# <a name="how-to-draw-ui-to-the-edge-of-the-screen"></a>Comment étirer l’IU vers le bord de l’écran   
Par défaut, des bordures sont placées autour de la fenêtre d’affichage de l’application. Elles permettent de tenir compte de la zone adaptée à l’écran de télévision. Pour plus d’informations, voir [Conception pour Xbox et TV](../design/devices/designing-for-tv.md#tv-safe-area). 

Nous vous recommandons de désactiver cette fonctionnalité et d’étirer l’IU vers le bord de l’écran. Pour ce faire, ajoutez le code suivant lorsque l’application démarre :
   
```
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```
   
> [!NOTE]
> Les applications C++/DirectX n’ont pas besoin de prendre ce facteur en compte. Le système affiche toujours votre application jusqu’au bord de l’écran.

## <a name="see-also"></a>Voir aussi
- [Bonnes pratiques pour Xbox](tailoring-for-xbox.md)
- [UWP sur Xbox One](index.md)
