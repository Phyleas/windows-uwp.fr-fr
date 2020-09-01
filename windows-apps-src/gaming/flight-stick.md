---
title: Manche à balai
description: Utilisez les API Flight Stick Windows. Gaming. Input pour lire les entrées des bâtons de vol.
ms.assetid: DC633F6B-FDC9-4D6E-8401-305861F31192
ms.date: 03/06/2017
ms.topic: article
keywords: Windows 10, UWP, jeux, entrée, Flight Stick
ms.localizationpriority: medium
ms.openlocfilehash: b9b4353a2736feb7cdfde9871c29f61de52e0c9a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156423"
---
# <a name="flight-stick"></a>Manche à balai

Cette page décrit les principes fondamentaux de la programmation pour les bâtons de vol certifiés par une Xbox à l’aide de [Windows. Gaming. Input. Flightstick](/uwp/api/windows.gaming.input.flightstick) et des API associées pour la plateforme Windows universelle (UWP).

Voici ce que vous allez apprendre à la lecture de cet article :

* Comment rassembler une liste de bâtons de vol connectés et leurs utilisateurs
* Comment détecter qu’un bâton de vol a été ajouté ou supprimé
* lecture d’entrées à partir d’un ou de plusieurs bâtons de vol
* comportement des bâtons de vol comme des appareils de navigation de l’interface utilisateur

## <a name="overview"></a>Vue d’ensemble

Les bâtons de vol sont des périphériques d’entrée de jeu qui sont évalués pour reproduire l’apparence des bâtons de vol qui se trouvent dans le cockpit d’un plan ou d’un espacement. Il s’agit d’un appareil d’entrée parfait pour un contrôle rapide et précis des vols. Les bâtons de vol sont pris en charge dans les applications Windows 10 et Xbox One UWP par l’espace de noms [Windows. Gaming. Input](/uwp/api/windows.gaming.input) .

Les bâtons Xbox One Flight sont équipés des contrôles suivants :

* Une manette de jeu analogique avec torsion, qui prend en charge le roulement, le tangage et le lacet
* Une limitation analogique
* Deux boutons Fire
* Commutateur numérique à 8 voies
* Boutons **Afficher** et **menu**

> [!NOTE]
> Les boutons de **vue** et de **menu** sont utilisés pour prendre en charge la navigation dans l’interface utilisateur, pas pour les commandes de jeu et, par conséquent, ne peuvent pas être facilement accessibles en tant que boutons de manette

### <a name="ui-navigation"></a>Navigation d’interface utilisateur

Pour faciliter la prise en charge des différents périphériques d’entrée pour la navigation dans l’interface utilisateur et encourager la cohérence entre les jeux et les appareils, la plupart des périphériques d’entrée _physiques_ jouent simultanément le rôle de périphériques d’entrée _logiques_ distincts appelés [contrôleurs de navigation d’interface utilisateur](ui-navigation-controller.md). Le contrôleur de navigation d’interface utilisateur fournit un vocabulaire commun pour les commandes de navigation dans l’interface utilisateur, sur plusieurs périphériques d’entrée.

En tant que contrôleur de navigation de l’interface utilisateur, un bâton de vol mappe l’ensemble des commandes de navigation requises sur les boutons de la manette de [jeu](ui-navigation-controller.md#required-set) et de l' **affichage**, du **menu**, du **FirePrimary**et du **FireSecondary** .

| Commande de navigation | Entrée Flight Stick                  |
| ------------------:| ----------------------------------- |
|                 Haut | Manette de jeu                         |
|               Descendre | Manette de jeu                       |
|               Gauche | Manette de jeu à gauche                       |
|              Right | Manette de jeu droite                      |
|               Affichage | Bouton **Afficher**                     |
|               Menu | Bouton de **menu**                     |
|             Acceptation | Bouton **FirePrimary**              |
|             Annuler | Bouton **FireSecondary**            |

Les bâtons de vol ne mappent aucun des [jeux facultatifs](ui-navigation-controller.md#optional-set) de commandes de navigation.

## <a name="detect-and-track-flight-sticks"></a>Détecter et suivre les bâtons de vol

La détection et le suivi des bâtons de vol fonctionnent exactement de la même façon que pour les boîtiers de soumanchement, sauf avec la classe [Flightstick](/uwp/api/windows.gaming.input.flightstick) au lieu de la classe de [boîtier](/uwp/api/Windows.Gaming.Input.Gamepad) . Pour plus d’informations, consultez [boîtier et vibration](gamepad-and-vibration.md) .

<!-- Flight sticks are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected flight sticks and events to notify you when a flight stick is added or removed.

### The flight stick list

The [FlightStick](/uwp/api/windows.gaming.input.flightstick) class provides a static property, [FlightSticks](/uwp/api/windows.gaming.input.flightstick#Windows_Gaming_Input_FlightStick_FlightSticks), which is a read-only list of flight sticks that are currently connected. Because you might only be interested in some of the connected flight sticks, we recommend that you maintain your own collection instead of accessing them through the `FlightSticks` property.

The following example copies all connected flight sticks into a new collection:

```cpp
auto myFlightSticks = ref new Vector<FlightStick^>();

for (auto flightStick : FlightStick::FlightSticks)
{
    // This code assumes that you're interested in all flight sticks.
    myFlightSticks->Append(flightStick);
}
```

### Adding and removing flight sticks

When a flight stick is added or removed, the [FlightStickAdded](/uwp/api/windows.gaming.input.flightstick#Windows_Gaming_Input_FlightStick_FlightStickAdded) and [FlightStickRemoved](/uwp/api/windows.gaming.input.flightstick#Windows_Gaming_Input_FlightStick_FlightStickRemoved) events are raised. You can register handlers for these events to keep track of the flight sticks that are currently connected.

The following example starts tracking a flight stick that's been added:

```cpp
FlightStick::FlightStickAdded += 
    ref new EventHandler<FlightStick^>([] (Platform::Object^, FlightStick^ args)
{
    // This code assumes that you're interested in all new flight sticks.
    myFlightSticks->Append(args);
});
```

The following example stops tracking a flight stick that's been removed:

```cpp
FlightStick::FlightStickRemoved += 
    ref new EventHandler<FlightStick^>([] (Platform::Object^, FlightStick^ args)
{
    unsigned int indexRemoved;

    if (myFlightSticks->IndexOf(args, &indexRemoved))
    {
        myFlightSticks->RemoveAt(indexRemoved);
    }
});
```

### Users and headsets

Each flight stick can be associated with a user account to link their identity to their gameplay, and can have a headset attached to facilitate voice chat or in-game features. To learn more about working with users and headsets, see [Tracking users and their devices](input-practices-for-games.md#tracking-users-and-their-devices) and [Headset](headset.md). -->

## <a name="reading-the-flight-stick"></a>Lecture du bâton de vol

Une fois que vous avez identifié le bâton de vol qui vous intéresse, vous êtes prêt à recueillir des informations à partir de celui-ci. Toutefois, contrairement à d’autres types d’entrées que vous pouvez utiliser, les bâtons de vol ne communiquent pas les changements d’État en déclenchant des événements. À la place, vous devez _interroger_ régulièrement ces boîtiers de commande pour connaître leur état actuel.

### <a name="polling-the-flight-stick"></a>Interrogation du bâton de vol

L’interrogation capture un instantané du bâton de vol à un point précis dans le temps. Cette approche de la collecte des entrées est adaptée à la plupart des jeux, car leur logique s’exécute généralement dans une boucle déterministe plutôt que d’être pilotée par des événements. En général, il est généralement plus simple d’interpréter les commandes de jeu provenant d’entrées rassemblées en une seule fois par rapport à de nombreuses entrées collectées au fil du temps.

Vous interrogez un bâton de vol en appelant [Flightstick. GetCurrentReading](/uwp/api/windows.gaming.input.flightstick.GetCurrentReading). Cette fonction renvoie un [FlightStickReading](/uwp/api/windows.gaming.input.flightstickreading) qui contient l’état du bâton de vol.

L’exemple suivant interroge un bâton de vol pour son état actuel :

```cpp
auto flightStick = myFlightSticks->GetAt(0);
FlightStickReading reading = flightStick->GetCurrentReading();
```

En plus de l’état du bâton de vol, chaque lecture comprend un horodateur qui indique précisément quand l’État a été récupéré. Cet horodatage est utile pour faire le lien avec le minutage des valeurs précédentes ou de la simulation de jeu.

### <a name="reading-the-joystick-and-throttle-input"></a>Lecture de la manette de jeu et accélération de l’entrée

La manette de jeu fournit une lecture analogique comprise entre-1,0 et 1,0 dans les axes X, Y et Z (Roll, brai et lacet, respectivement). Pour la fonction Roll, la valeur-1,0 correspond à la position de la manette de jeu la plus à gauche, tandis que la valeur 1,0 correspond à la position la plus à droite. Pour le pas, la valeur-1,0 correspond à la position inférieure de la manette de jeu, tandis que la valeur 1,0 correspond à la position supérieure. Pour le lacet, la valeur-1,0 correspond à la position croisée la plus à gauche, tandis que la valeur 1,0 correspond à la position la plus à droite.

Dans tous les axes, la valeur est approximativement de 0,0 lorsque la manette de jeu est dans la position centrale, mais il est normal que la valeur précise varie, même entre les lectures suivantes. Les stratégies d’atténuation de cette variation sont décrites plus loin dans cette section.

La valeur du rouleau de la manette de jeu est lue à partir de la propriété [FlightStickReading. Roll](/uwp/api/windows.gaming.input.flightstickreading.Roll) , la valeur de la hauteur est lue à partir de la propriété [FlightStickReading. brai](/uwp/api/windows.gaming.input.flightstickreading.Pitch) et la valeur du lacet est lue à partir de la propriété [FlightStickReading. lacet](/uwp/api/windows.gaming.input.flightstickreading.Yaw) :

```cpp
// Each variable will contain a value between -1.0 and 1.0.
float roll = reading.Roll;
float pitch = reading.Pitch;
float yaw = reading.Yaw;
```

Lors de la lecture des valeurs de la manette de jeu, vous remarquerez qu’elles ne produisent pas de façon fiable une lecture neutre de 0,0 lorsque la manette de jeu est au repos au centre. au lieu de cela, elles produisent des valeurs différentes près de 0,0 à chaque fois que la manette de jeu est déplacée et retournée à la position centrale. Pour compenser ces variations, vous pouvez implémenter une petite _zone morte_, qui correspond à une plage de valeurs proches de la position centrale idéale à ignorer.

L’une des façons d’implémenter un deadzone consiste à déterminer le degré de déplacement de la manette de la manette à partir du centre et à ignorer les lectures qui sont plus proches que la distance de votre choix. Vous pouvez calculer la distance &mdash; de façon à ce qu’elle ne soit pas exacte, car les lectures de la manette de jeu sont essentiellement des valeurs polaires, non planaires, &mdash; simplement à l’aide du Pythagorean. Vous obtenez ainsi une zone morte radiale.

L’exemple suivant illustre un deadzone radial de base à l’aide du Pythagorean :

```cpp
// Choose a deadzone. Readings inside this radius are ignored.
const float deadzoneRadius = 0.1f;
const float deadzoneSquared = deadzoneRadius * deadzoneRadius;

// Pythagorean theorem: For a right triangle, hypotenuse^2 = (opposite side)^2 + (adjacent side)^2
float oppositeSquared = pitch * pitch;
float adjacentSquared = roll * roll;

// Accept and process input if true; otherwise, reject and ignore it.
if ((oppositeSquared + adjacentSquared) < deadzoneSquared)
{
    // Input accepted, process it.
}
```

### <a name="reading-the-buttons-and-hat-switch"></a>Lecture des boutons et du commutateur Hat

Chacun des deux boutons d’incendie du manche de avion fournit une lecture numérique qui indique s’il est appuyé (enfoncé) ou relâché (haut). Pour des performances optimales, les lectures de bouton ne sont pas représentées en tant que valeurs booléennes individuelles &mdash; . elles sont toutes compressées dans un seul champ de type de champ représenté par l’énumération [FlightStickButtons](/uwp/api/windows.gaming.input.flightstickbuttons) . En outre, le commutateur à 8 voies fournit un sens dans un seul champ de bits représenté par l’énumération [GameControllerSwitchPosition](/uwp/api/windows.gaming.input.gamecontrollerswitchposition) .

> [!NOTE]
> Les bâtons de vol sont équipés de boutons supplémentaires utilisés pour la navigation dans l’interface utilisateur, tels que les boutons d' **affichage** et de **menu** . Ces boutons ne font pas partie de l' `FlightStickButtons` énumération et peuvent uniquement être lus en accédant au point de vol en tant qu’appareil de navigation de l’interface utilisateur. Pour plus d’informations, consultez [contrôleur de navigation de l’interface utilisateur](ui-navigation-controller.md).

Les valeurs de bouton sont lues à partir de la propriété [FlightStickReading. Buttons](/uwp/api/windows.gaming.input.flightstickreading.Buttons) . Comme cette propriété est un champ de bits, un masquage au niveau du bit est effectué pour isoler la valeur du bouton qui vous intéresse. Le bouton est enfoncé (enfoncé) lorsque le bit correspondant est défini ; dans le cas contraire, elle est libérée (haut).

L’exemple suivant détermine si le bouton **FirePrimary** est enfoncé :

```cpp
if (FlightStickButtons::FirePrimary == (reading.Buttons & FlightStickButtons::FirePrimary))
{
    // FirePrimary is pressed.
}
```

L’exemple suivant détermine si le bouton **FirePrimary** est relâché :

```cpp
if (FlightStickButtons::None == (reading.Buttons & FlightStickButtons::FirePrimary))
{
    // FirePrimary is released (not pressed).
}
```

Parfois, vous souhaiterez peut-être déterminer quand un bouton passe de enfoncé à relâché ou relâché, si plusieurs boutons sont enfoncés ou relâché, ou si un ensemble de boutons est organisé d’une façon particulière &mdash; , d’autres non. Pour plus d’informations sur la détection de chacun de ces états, consultez [Détecter les changements d’état des boutons](input-practices-for-games.md#detecting-button-transitions) et [Détecter les dispositions de boutons complexes](input-practices-for-games.md#detecting-complex-button-arrangements).

La valeur du commutateur Hat est lue à partir de la propriété [FlightStickReading. HatSwitch](/uwp/api/windows.gaming.input.flightstickreading.HatSwitch) . Étant donné que cette propriété est également un champ de bits, le masquage au niveau du bit est encore utilisé pour isoler la position du commutateur Hat.

L’exemple suivant détermine si le commutateur Hat se trouve à l’emplacement suivant :

```cpp
if (GameControllerSwitchPosition::Up == (reading.HatSwitch & GameControllerSwitchPosition::Up))
{
    // The hat switch is in the up position.
}
```

L’exemple suivant détermine si le commutateur Hat se trouve dans la position centrale :

```cpp
if (GameControllerSwitchPosition::Center == (reading.HatSwitch & GameControllerSwitchPosition::Center))
{
    // The hat switch is in the center position.
}
```

<!--## Run the InputInterfacingUWP sample

The [InputInterfacingUWP sample _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) demonstrates how to use flight sticks and different kinds of input devices in tandem, as well as how these input devices behave as UI navigation controllers.-->

## <a name="see-also"></a>Voir aussi

* [Classe Windows. Gaming. Input. UINavigationController](/uwp/api/windows.gaming.input.uinavigationcontroller)
* [Interface Windows. Gaming. Input. IGameController](/uwp/api/windows.gaming.input.igamecontroller)
* [Pratiques d’entrée pour les jeux](input-practices-for-games.md)