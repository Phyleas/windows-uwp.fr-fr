---
description: Entrées et propriétés d’une feuille de style de carte
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Référence sur les feuilles de style de carte
ms.date: 03/19/2017
ms.topic: article
keywords: Windows 10, UWP, Maps, feuille de style cartographique
ms.localizationpriority: medium
ms.openlocfilehash: ec30fd5d63dfa522ef721d2d8a2e4950e6e8e854
ms.sourcegitcommit: c9edb164356c0843d8a9b19ab43707d486e392e1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85377930"
---
# <a name="map-style-sheet-reference"></a>Référence sur les feuilles de style de carte

Les technologies de mappage Microsoft utilisent des [feuilles de style de carte](https://docs.microsoft.com/BingMaps/styling/map-style-sheets) pour définir l’apparence des cartes, y compris celles affichées dans le [collection MapControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol)d’une application du Windows Store.  Une feuille de style cartographique est définie à l’aide de JavaScript Object Notation (JSON) par le biais de la méthode [MapStyleSheet. ParseFromJson](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet.parsefromjson#Windows_UI_Xaml_Controls_Maps_MapStyleSheet_ParseFromJson_System_String_) .

Une feuille de style se compose d' [entrées](https://docs.microsoft.com/BingMaps/styling/map-style-sheet-entries) dont les propriétés sont remplacées pour modifier l’apparence finale de la carte.

Par exemple, le code JSON suivant peut être utilisé pour faire apparaître les zones d’eau en rouge, les étiquettes d’eau s’affichent en vert et les zones terrestres en bleu :

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000","labelColor":"#00FF00"}}
    }
```

Les feuilles de style peuvent être créées de manière interactive à l’aide de l’application [éditeur de feuille de style de carte](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft) .
