---
title: Contrôleur de jeu brut
description: Utilisez les API du contrôleur de jeu brut Windows. Gaming. Input pour lire les entrées de presque n’importe quel type de contrôleur de jeu.
ms.assetid: 2A466C16-1F51-4D8D-AD13-704B6D3C7BEC
ms.date: 03/08/2017
ms.topic: article
keywords: Windows 10, UWP, jeux, entrée, contrôleur de jeu brut
ms.localizationpriority: medium
ms.openlocfilehash: d21b411965da874cfb324fc2ee867e39bdcd0ded
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171983"
---
# <a name="raw-game-controller"></a>Contrôleur de jeu brut

Cette page décrit les principes fondamentaux de la programmation pour presque n’importe quel type de contrôleur de jeu à l’aide de [Windows. Gaming. Input. RawGameController](/uwp/api/windows.gaming.input.rawgamecontroller) et des API associées pour le plateforme Windows universelle (UWP).

Voici ce que vous allez apprendre à la lecture de cet article :

* Comment rassembler une liste de contrôleurs de jeu bruts connectés et leurs utilisateurs
* Comment détecter qu’un contrôleur de jeu brut a été ajouté ou supprimé
* Comment obtenir les fonctionnalités d’un contrôleur de jeu brut
* lecture d’entrées à partir d’un contrôleur de jeu brut

## <a name="overview"></a>Vue d’ensemble

Un contrôleur de jeu brut est une représentation générique d’un contrôleur de jeu, avec des entrées trouvées dans de nombreux types de contrôleurs de jeu courants. Ces entrées sont exposées sous forme de tableaux simples de boutons, de commutateurs et d’axes sans nom. À l’aide d’un contrôleur de jeu brut, vous pouvez autoriser les clients à créer des mappages d’entrée personnalisés, quel que soit le type de contrôleur qu’ils utilisent.

La classe [RawGameController](/uwp/api/windows.gaming.input.rawgamecontroller) est vraiment destinée aux scénarios où les autres classes d’entrée ([ArcadeStick](/uwp/api/windows.gaming.input.arcadestick), [Flightstick](/uwp/api/windows.gaming.input.flightstick), etc.) ne répondent pas à vos besoins &mdash; si vous souhaitez un modèle plus générique, anticipant que les clients utiliseront de nombreux types de contrôleurs de jeu, alors cette classe est pour vous.

## <a name="detect-and-track-raw-game-controllers"></a>Détecter et suivre les contrôleurs de jeu bruts

La détection et le suivi des contrôleurs de jeu bruts fonctionnent exactement de la même façon que pour les boîtiers de la classe, à l’exception de la classe [RawGameController](/uwp/api/windows.gaming.input.rawgamecontroller) au lieu de la classe de [boîtier](/uwp/api/Windows.Gaming.Input.Gamepad) . Pour plus d’informations, consultez [boîtier et vibration](gamepad-and-vibration.md) .

<!-- Raw game controllers are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected raw game controllers and events to notify you when a raw game controller is added or removed.

### The raw game controller list

The [RawGameController](/uwp/api/windows.gaming.input.rawgamecontroller) class provides a static property, [RawGameControllers](/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_RawGameControllers), which is a read-only list of raw game controllers that are currently connected. Because you might only be interested in some of the connected raw game controllers, we recommend that you maintain your own collection instead of accessing them through the **RawGameControllers** property.

The following example copies all connected raw game controllers into a new collection:

```cpp
auto myRawGameControllers = ref new Vector<RawGameController^>();

for (auto rawGameController : RawGameController::RawGameControllers)
{
    // This code assumes that you're interested in all raw game controllers.
    myRawGameControllers->Append(rawGameController);
}
```

### Adding and removing raw game controllers

When a raw game controller is added or removed, the [RawGameControllerAdded](/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_RawGameControllerAdded) and [RawGameControllerRemoved](/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_RawGameControllerRemoved) events are raised. You can register handlers for these events to keep track of the raw game controllers that are currently connected.

The following example starts tracking a raw game controller that's been added:

```cpp
RawGameController::RawGameControllerAdded += 
    ref new EventHandler<RawGameController^>(
        [] (Platform::Object^, RawGameController^ args)
{
    // This code assumes that you're interested in all new raw game controllers.
    myRawGameControllers->Append(args);
});
```

The following example stops tracking a raw game controller that's been removed:

```cpp
RawGameController::RawGameControllerRemoved +=
    ref new EventHandler<RawGameController^>(
        [] (Platform::Object^, RawGameController^ args)
{
    unsigned int indexRemoved;

    if (myRawGameControllers->IndexOf(args, &indexRemoved))
    {
        myRawGameControllers->RemoveAt(indexRemoved);
    }
});
```

### Users and headsets

Each raw game controller can be associated with a user account to link their identity to their gameplay, and can have a headset attached to facilitate voice chat or in-game features. To learn more about working with users and headsets, see [Tracking users and their devices](input-practices-for-games.md#tracking-users-and-their-devices) and [Headset](headset.md). -->

## <a name="get-the-capabilities-of-a-raw-game-controller"></a>Obtenir les fonctionnalités d’un contrôleur de jeu brut

Après avoir identifié le contrôleur de jeu brut qui vous intéresse, vous pouvez recueillir des informations sur les fonctionnalités du contrôleur. Vous pouvez récupérer le nombre de boutons sur le contrôleur de jeu brut avec [RawGameController. ButtonCount](/uwp/api/windows.gaming.input.rawgamecontroller.ButtonCount), le nombre d’axes analogiques avec [RawGameController. AxisCount](/uwp/api/windows.gaming.input.rawgamecontroller.AxisCount)et le nombre de commutateurs avec [RawGameController. SwitchCount](/uwp/api/windows.gaming.input.rawgamecontroller.SwitchCount). En outre, vous pouvez obtenir le type d’un commutateur à l’aide de [RawGameController. GetSwitchKind](/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetSwitchKind_System_Int32_).

L’exemple suivant obtient le nombre d’entrées d’un contrôleur de jeu brut :

```cpp
auto rawGameController = myRawGameControllers->GetAt(0);
int buttonCount = rawGameController->ButtonCount;
int axisCount = rawGameController->AxisCount;
int switchCount = rawGameController->SwitchCount;
```

L’exemple suivant détermine le type de chaque commutateur :

```cpp
for (uint32_t i = 0; i < switchCount; i++)
{
    GameControllerSwitchKind mySwitch = rawGameController->GetSwitchKind(i);
}
```

## <a name="reading-the-raw-game-controller"></a>Lecture du contrôleur de jeu brut

Une fois que vous connaissez le nombre d’entrées sur un contrôleur de jeu brut, vous êtes prêt à collecter des données à partir de celui-ci. Toutefois, contrairement à d’autres types d’entrées que vous pouvez utiliser, un contrôleur de jeu brut ne communique pas les changements d’État en déclenchant des événements. Au lieu de cela, vous prenez des lectures régulières de son état actuel en l' _interrogeant_ .

### <a name="polling-the-raw-game-controller"></a>Interrogation du contrôleur de jeu brut

L’interrogation capture un instantané du contrôleur de jeu brut à un point précis dans le temps. Cette approche de la collecte des entrées est adaptée à la plupart des jeux, car leur logique s’exécute généralement dans une boucle déterministe plutôt que d’être pilotée par des événements. En général, il est généralement plus simple d’interpréter les commandes de jeu provenant d’entrées rassemblées en une seule fois par rapport à de nombreuses entrées collectées au fil du temps.

Vous interrogez un contrôleur de jeu brut en appelant [RawGameController. GetCurrentReading](/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetCurrentReading_System_Boolean___Windows_Gaming_Input_GameControllerSwitchPosition___System_Double___). Cette fonction remplit des tableaux pour les boutons, les commutateurs et les axes qui contiennent l’état du contrôleur de jeu brut.

L’exemple suivant interroge un contrôleur de jeu brut pour son état actuel :

```cpp
Platform::Array<bool>^ currentButtonReading =
    ref new Platform::Array<bool>(buttonCount);

Platform::Array<GameControllerSwitchPosition>^ currentSwitchReading =
    ref new Platform::Array<GameControllerSwitchPosition>(switchCount);

Platform::Array<double>^ currentAxisReading = ref new Platform::Array<double>(axisCount);

rawGameController->GetCurrentReading(
    currentButtonReading,
    currentSwitchReading,
    currentAxisReading);
```

Il n’existe aucune garantie quant à la position dans chaque tableau qui contiendra la valeur d’entrée parmi les différents types de contrôleurs. vous devez donc vérifier l’entrée qui utilise les méthodes [RawGameController. GetButtonLabel](/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetButtonLabel_System_Int32_) et [RawGameController. GetSwitchKind](/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetSwitchKind_System_Int32_).

**GetButtonLabel** indique le texte ou le symbole qui est imprimé sur le bouton physique, plutôt que la fonction du bouton &mdash; , il est donc préférable d’utiliser l’interface utilisateur pour les cas où vous souhaitez donner des indications sur les boutons qui exécutent quelles fonctions. **GetSwitchKind** indique le type de commutateur (c’est-à-dire le nombre de positions qu’il a), mais pas le nom.

Il n’existe pas de méthode standardisée pour obtenir l’étiquette d’un axe ou d’un commutateur. vous devez donc les tester vous-même pour déterminer quelle entrée est.

Si vous avez un contrôleur spécifique que vous souhaitez prendre en charge, vous pouvez obtenir [RawGameController. HardwareProductId](/uwp/api/windows.gaming.input.rawgamecontroller.HardwareProductId) et [RawGameController. HardwareVendorId](/uwp/api/windows.gaming.input.rawgamecontroller.HardwareVendorId) et vérifier s’ils correspondent à ce contrôleur. La position de chaque entrée dans chaque tableau est la même pour chaque contrôleur ayant les mêmes **HardwareProductId** et **HardwareVendorId**. vous n’avez donc pas à vous soucier de la logique potentiellement incohérente parmi les différents contrôleurs du même type.

En plus de l’état de contrôleur de jeu brut, chaque lecture retourne un horodateur qui indique précisément quand l’État a été récupéré. Cet horodatage est utile pour faire le lien avec le minutage des valeurs précédentes ou de la simulation de jeu.

### <a name="reading-the-buttons-and-switches"></a>Lecture des boutons et des commutateurs

Chacun des boutons du contrôleur de jeu brut fournit une lecture numérique qui indique s’il est appuyé (enfoncé) ou relâché (haut). Les lectures de bouton sont représentées sous forme de valeurs booléennes individuelles dans un tableau unique. L’étiquette de chaque bouton est disponible à l’aide de [RawGameController. GetButtonLabel](/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetButtonLabel_System_Int32_) avec l’index de la valeur booléenne dans le tableau. Chaque valeur est représentée sous la forme d’un [GameControllerButtonLabel](/uwp/api/windows.gaming.input.gamecontrollerbuttonlabel).

L’exemple suivant détermine si le bouton **xboxa** est enfoncé :

```cpp
for (uint32_t i = 0; i < buttonCount; i++)
{
    if (currentButtonReading[i])
    {
        GameControllerButtonLabel buttonLabel = rawGameController->GetButtonLabel(i);

        if (buttonLabel == GameControllerButtonLabel::XboxA)
        {
            // XboxA is pressed.
        }
    }
}
```

Parfois, vous souhaiterez peut-être déterminer quand un bouton passe de enfoncé à relâché ou relâché, si plusieurs boutons sont enfoncés ou relâché, ou si un ensemble de boutons est organisé d’une façon particulière &mdash; , d’autres non. Pour plus d’informations sur la détection de chacun de ces états, consultez [Détecter les changements d’état des boutons](input-practices-for-games.md#detecting-button-transitions) et [Détecter les dispositions de boutons complexes](input-practices-for-games.md#detecting-complex-button-arrangements).

Les valeurs de commutateur sont fournies sous la forme d’un tableau de [GameControllerSwitchPosition](/uwp/api/windows.gaming.input.gamecontrollerswitchposition). Étant donné que cette propriété est un champ de bits, le masquage au niveau du bit est utilisé pour isoler la direction du commutateur.

L’exemple suivant détermine si chaque commutateur se trouve à l’emplacement le plus haut :

```cpp
for (uint32_t i = 0; i < switchCount; i++)
{
    if (GameControllerSwitchPosition::Up ==
        (currentSwitchReading[i] & GameControllerSwitchPosition::Up))
    {
        // The switch is in the up position.
    }
}
```

### <a name="reading-the-analog-inputs-sticks-triggers-throttles-and-so-on"></a>Lecture des entrées analogiques (bâtons, déclencheurs, limitations, etc.)

Un axe analogique fournit une lecture entre 0,0 et 1,0. Cela comprend chaque dimension sur un bâton tel que X et Y pour les bâtons standard, voire les axes X, Y et Z (Roll, brai et lacet, respectivement) pour les bâtons de vol.

Les valeurs peuvent représenter des déclencheurs analogiques, des limitations ou tout autre type d’entrée analogique. Ces valeurs ne sont pas fournies avec des étiquettes. nous vous suggérons donc de tester votre code avec divers périphériques d’entrée pour vous assurer qu’ils correspondent correctement à vos hypothèses.

Dans tous les axes, la valeur est d’environ 0,5 pour un bâton lorsqu’il se trouve dans la position centrale, mais il est normal que la valeur précise varie, même entre les lectures suivantes. les stratégies d’atténuation de cette variation sont décrites plus loin dans cette section.

L’exemple suivant montre comment lire les valeurs analogiques à partir d’un contrôleur Xbox :

```cpp
// Xbox controllers have 6 axes: 2 for each stick and one for each trigger.
float leftStickX = currentAxisReading[0];
float leftStickY = currentAxisReading[1];
float rightStickX = currentAxisReading[2];
float rightStickY = currentAxisReading[3];
float leftTrigger = currentAxisReading[4];
float rightTrigger = currentAxisReading[5];
```

Lors de la lecture des valeurs du Stick, vous remarquerez qu’elles ne produisent pas de façon fiable une lecture neutre de 0,5 au repos au centre. au lieu de cela, elles produisent des valeurs différentes près de 0,5 à chaque fois que le levier est déplacé et renvoyé à la position centrale. Pour compenser ces variations, vous pouvez implémenter une petite _zone morte_, qui correspond à une plage de valeurs proches de la position centrale idéale à ignorer.

L’une des façons d’implémenter un deadzone consiste à déterminer le degré de déplacement du Stick à partir du centre et à ignorer les lectures qui sont plus proches que la distance de votre choix. Vous pouvez calculer la distance &mdash; de façon à ce qu’elle ne soit pas exacte, car les lectures des bâtons sont essentiellement des valeurs polaires, non planaires, &mdash; simplement à l’aide du Pythagorean. Vous obtenez ainsi une zone morte radiale.

L’exemple suivant illustre un deadzone radial de base à l’aide du Pythagorean :

```cpp
// Choose a deadzone. Readings inside this radius are ignored.
const float deadzoneRadius = 0.1f;
const float deadzoneSquared = deadzoneRadius * deadzoneRadius;

// Pythagorean theorem: For a right triangle, hypotenuse^2 = (opposite side)^2 + (adjacent side)^2
float oppositeSquared = leftStickY * leftStickY;
float adjacentSquared = leftStickX * leftStickX;

// Accept and process input if true; otherwise, reject and ignore it.
if ((oppositeSquared + adjacentSquared) < deadzoneSquared)
{
    // Input accepted, process it.
}
```

<!--## Run the RawGameControllerUWP sample

The [RawGameControllerUWP sample (GitHub)](TODO: Link) demonstrates how to use raw game controllers. TODO: More information-->

## <a name="see-also"></a>Voir aussi

* [Entrées pour les jeux](input-for-games.md)
* [Pratiques d’entrée pour les jeux](input-practices-for-games.md)
* [Espace de noms Windows. Gaming. Input](/uwp/api/windows.gaming.input)
* [Classe Windows. Gaming. Input. RawGameController](/uwp/api/windows.gaming.input.rawgamecontroller)
* [Interface Windows. Gaming. Input. IGameController](/uwp/api/windows.gaming.input.igamecontroller)
* [Interface Windows. Gaming. Input. IGameControllerBatteryInfo](/uwp/api/windows.gaming.input.igamecontrollerbatteryinfo)