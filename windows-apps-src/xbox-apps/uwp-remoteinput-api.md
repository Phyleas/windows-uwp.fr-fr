---
title: Référence de l’API d’entrée distante du portail de périphérique
description: Découvrez comment envoyer des entrées de contrôleur, de clavier et de souris à distance sur une Xbox.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 34546f5aa7a1fd5bddd9122d24fc38dd2026700d
ms.sourcegitcommit: b0a82c2a132212eb5fb72b67f0789cac1014642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254244"
---
# <a name="remote-input-api-reference"></a>Référence de l’API d’entrée distante   
Vous pouvez envoyer des entrées de contrôleur, de clavier et de souris en temps réel à distance par le biais de cette API.

**Requête**

| Méthode | URI de demande |
|--------|-------------|
| WebSocket | /ext/remoteinput |

**Paramètres d’URI**

- None

**En-têtes de requête**

- None

**Requête**

WebSocket une série de messages de tableau d’octets. Pour chaque message, le format est le suivant.

Le premier octet indique le type d’entrée. Les types d’entrée suivants sont pris en charge :

| Type d’entrée | Valeur d’octet |
|------------|------------|
| Codes de touches du clavier | 0x01 |
| ScanCodes clavier | 0x02 |
| Souris | 0x03 |
| Effacer tout | 0x04 |

Pour KeyboardKeyCodes et KeyboardScanCodes, le deuxième octet est la valeur de keycode ou scancode et le troisième octet est 0x01 pour une pression vers l’avant et 0x00 pour une mise en sortie.

Pour un message de souris, la valeur suivante est un UINT16 dans l’ordre du réseau (2 octets) indiquant le type d’événement de souris :

| Type d’action | Valeur UINT16 |
|-------------|--------------|
| Déplacer | 0x0001 |
| LeftDown | 0x0002 |
| LeftUp | 0x0004 |
| RightDown | 0x0008 |
| RightUp | 0x0010 |
| MiddleDown | 0x0020 |
| MiddleUp | 0x0040 |
| X1Down | 0x0080 |
| X1Up | 0x0100 |
| X2Down | 0x0200 |
| X2Up | 0x0400 |
| VerticalWheelMoved | 0x0800 |
| HorizontalWheelMoved | 0x1000 |

Cet octet est suivi de deux valeurs UINT32 dans l’ordre des réseaux et d’un troisième valeur facultative pour les actions de roulette. Les deux premières valeurs sont les coordonnées X et Y, tandis que la troisième est le delta de la roue. Les coordonnées X et Y sont supposées être une valeur comprise entre 0 et 65535 indiquant la position relative de la souris dans les plans horizontaux et verticaux.

ClearAll indique que toutes les touches en cours de maintien doivent être libérées. Aucun autre octet n’est attendu.

Pour l’envoi d’une entrée de manette, les valeurs de keycode qui représentent des enfoncements de bouton de manette de main peuvent être utilisées avec KeyboardKeyCodes. Ces valeurs sont les suivantes :

| Type de boîtier | Valeur d’octet |
|--------------|------------|
| VK_GAMEPAD_A                       |  0xC3 |
| VK_GAMEPAD_B                       |  0xC4 |
| VK_GAMEPAD_X                       |  0xC5 |
| VK_GAMEPAD_Y                       |  0xC6 |
| VK_GAMEPAD_RIGHT_SHOULDER          |  0xC7 |
| VK_GAMEPAD_LEFT_SHOULDER           |  0xC8 |
| VK_GAMEPAD_LEFT_TRIGGER            |  0xC9 |
| VK_GAMEPAD_RIGHT_TRIGGER           |  0xCA |
| VK_GAMEPAD_DPAD_UP                 |  0xCB |
| VK_GAMEPAD_DPAD_DOWN               |  0xCC |
| VK_GAMEPAD_DPAD_LEFT               |  0xCD |
| VK_GAMEPAD_DPAD_RIGHT              |  0xCE |
| VK_GAMEPAD_MENU                    |  0xCF |
| VK_GAMEPAD_VIEW                    |  0xD0 |
| VK_GAMEPAD_LEFT_THUMBSTICK_BUTTON  |  0xD1 |
| VK_GAMEPAD_RIGHT_THUMBSTICK_BUTTON |  0xD2 |
| VK_GAMEPAD_LEFT_THUMBSTICK_UP      |  0xD3 |
| VK_GAMEPAD_LEFT_THUMBSTICK_DOWN    |  0xD4 |
| VK_GAMEPAD_LEFT_THUMBSTICK_RIGHT   |  0xD5 |
| VK_GAMEPAD_LEFT_THUMBSTICK_LEFT    |  0xD6 |
| VK_GAMEPAD_RIGHT_THUMBSTICK_UP     |  0xD7 |
| VK_GAMEPAD_RIGHT_THUMBSTICK_DOWN   |  0xD8 |
| VK_GAMEPAD_RIGHT_THUMBSTICK_RIGHT  |  0xD9 |
| VK_GAMEPAD_RIGHT_THUMBSTICK_LEFT   |  0xDA |

**Réponse**   

- None

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d'état HTTP | Description |
|----------------|-------------|
| 200 | Demande réussie |
| 4XX | Codes d’erreur |
| 5XX | Codes d’erreur |

**Familles d’appareils disponibles**

* Windows Xbox
