---
author: mithom
title: Boîtier de commande et vibration
description: Utilisez les API de boîtier de commande Windows.Gaming.Input pour détecter et lire les commandes de vibration et d’impulsion, et les envoyer aux boîtiers de commande.
ms.assetid: BB03BB8E-255F-4AE8-AC43-1E519CA860FE
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, jeux, boîtier de commande, vibration
ms.openlocfilehash: d09bfcd3dae004f07e07401f4e6ba65ac5027b32
ms.sourcegitcommit: ae93435e1f9c010a054f55ed7d6bd2f268223957
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2017
ms.locfileid: "771546"
---
# <a name="gamepad-and-vibration"></a>Boîtier de commande et vibration

Cet article explique les notions de base de la programmation pour les boîtiers de commande Xbox One avec l’API [Windows.Gaming.Input.Gamepad][gamepad] et les API associées pour la plateforme Windows universelle (UWP).

Voici ce que vous allez apprendre à la lecture de cet article:
* Obtenir une liste des boîtiers de commande connectés et de leurs utilisateurs
* Détecter l’ajout ou la suppression d’un boîtier de commande
* Lire les entrées provenant d’un ou de plusieurs boîtiers de commande
* Envoyer des commandes de vibration et d’impulsion
* Comprendre le comportement des boîtiers de commande utilisés comme périphériques de navigation


## <a name="gamepad-overview"></a>Vue d’ensemble des boîtiers de commande

Les boîtiers de commande tels que les manettes sans fil Xbox Wireless Controller et Xbox Wireless ControllerS sont des périphériques d’entrée de jeu à usage général. Ils représentent le périphérique d’entrée standard sur XboxOne et ils sont couramment choisis par les joueurs Windows qui ne préfèrent pas utiliser un clavier et une souris. Les boîtiers de commande sont pris en charge dans les applications UWP Windows10 et Xbox par l’espace de noms [Windows.Gaming.Input][].

Les boîtiers de commande XboxOne se composent d’un bouton multidirectionnel (appelé également pavé directionnel); des boutons **A**, **B**, **X**, **Y**, **Afficher** et **Menu**; de sticks analogiques gauche et droit; de gâchettes hautes et gâchettes gauche et droite; et de quatre moteurs de vibration. Les deux sticks analogiques fournissent des valeurs analogiques doubles sur les axesX etY, et fonctionnent également comme un bouton quand l’utilisateur appuie dessus vers l’intérieur. Chaque gâchette fournit une valeur analogique qui représente sa position vers l’arrière.

> **Remarque** La manette sans fil Xbox Elite Wireless Controller comporte quatre **palettes** supplémentaires à l’arrière. Ces palettes permettent de fournir un accès redondant à des commandes de jeu difficiles à utiliser simultanément (par exemple, appuyer en même temps sur le stick analogique droit et sur l’un des boutons **A**, **B**, **X** ou **Y**) ou fournir un accès dédié à des commandes supplémentaires.

> **Remarque** `Windows.Gaming.Input.Gamepad` prend également en charge les boîtiers de commande Xbox360, qui présentent la même disposition de contrôles que les boîtiers de commande XboxOne standard.

### <a name="vibration-and-impulse-triggers"></a>Gâchettes de vibration et à impulsions

Les boîtiers de commande XboxOne ont deuxmoteurs indépendants qui déclenchent des vibrations fortes et légères, ainsi que deuxmoteurs dédiés qui produisent des vibrations rapides en série au niveau de chaque gâchette (d’où le nom de _gâchette à impulsions_, qui est une fonctionnalité spécifique au boîtier de commande XboxOne).

> **Remarque** Les boîtiers de commande Xbox360 n’ont pas de _gâchettes à impulsions_.

Pour plus d’informations, consultez [Vue d’ensemble des gâchettes de vibration et à impulsions](#vibration-and-impulse-triggers-overview).

### <a name="thumbstick-deadzones"></a>Zones mortes (ou «dead zones») des sticks analogiques

Quand un stick analogique se trouve à la position centrale, il devrait idéalement toujours fournir la même valeur neutre sur les axesX etY. Or, en raison de certaines forces mécaniques et de la sensibilité du stick analogique, les valeurs réelles fournies à la position centrale ne correspondent pas exactement à la valeur neutre idéale et peuvent varier d’une lecture de valeur à une autre. C’est pourquoi vous devez toujours utiliser une petite _zone morte_, qui correspond à une plage de valeurs proches de la position centrale idéale à ignorer, pour compenser les écarts dus aux différences de fabrication, à l’usure mécanique ou à d’autres problèmes du boîtier de commande.

L’utilisation de zones mortes plus larges est une solution simple pour faire la distinction entre les entrées intentionnelles et les entrées accidentelles.

Pour plus d’informations, consultez [Lecture des entrées des sticks analogiques](#reading-the-thumbsticks).

### <a name="ui-navigation"></a>Navigation d’interface utilisateur

Pour réduire la difficulté associée à la prise en charge de plusieurs périphériques d’entrée différents lors de la navigation dans l’interface utilisateur, et pour favoriser la cohérence entre les jeux et les périphériques, la plupart des périphériques d’entrée _physiques_ agissent en tant que périphérique d’entrée _logique_ distinct, appelé [contrôleur de navigation d’interface utilisateur](ui-navigation-controller.md). Le contrôleur de navigation d’interface utilisateur fournit un vocabulaire commun pour les commandes de navigation d’interface utilisateur utilisées sur les différents périphériques d’entrée.

Quand ils sont utilisés comme contrôleurs de navigation d’interface, les boîtiers de commande mappent l’[ensemble obligatoire](ui-navigation-controller.md#required-set) de commandes de navigation au stick analogique gauche, au bouton multidirectionnel, et aux boutons **Afficher**, **Menu**, **A** et **B**.

| Commande de navigation | Entrée boîtier de commande                       |
| ------------------:| ----------------------------------- |
|                 Up (Haut) | Stick analogique gauche vers le haut/ bouton multidirectionnel vers le haut       |
|               Down (Bas) | Stick analogique gauche vers le bas/ bouton multidirectionnel vers le bas   |
|               Left (Gauche) | Stick analogique gauche vers la gauche/ bouton multidirectionnel vers la gauche   |
|              Right (Droite) | Stick analogique gauche vers la droite/ bouton multidirectionnel vers la droite |
|               View (Afficher) | Bouton Afficher                         |
|               Menu | Bouton Menu                         |
|             Accept (Accepter) | BoutonA                            |
|             Annuler | BoutonB                            |

De plus, les boîtiers de commande mappent toutes les commandes de navigation de l’[ensemble facultatif](ui-navigation-controller.md#optional-set) aux entrées restantes.

| Commande de navigation | Entrée boîtier de commande          |
| ------------------:| ---------------------- |
|            Page précédente | Gâchette gauche           |
|          Page suivante | Gâchette droite          |
|          Page vers la gauche | Gâchette haute gauche            |
|         Page vers la droite | Gâchette haute droite           |
|          Faire défiler vers le haut | Stick analogique droit vers le haut    |
|        Faire défiler vers le bas | Stick analogique droit vers le bas  |
|        Faire défiler vers la gauche | Stick analogique droit vers la gauche  |
|       Faire défiler vers la droite | Stick analogique droit vers la droite |
|          Contexte1 | BoutonX               |
|          Contexte2 | BoutonY               |
|          Contexte3 | Appui sur stick analogique gauche  |
|          Contexte4 | Appui sur stick analogique droit |


## <a name="detect-and-track-gamepads"></a>Détecter et suivre des boîtiers de commande

Comme les boîtiers de commande sont gérés par le système, vous n’avez pas à les créer ni à les initialiser. Le système fournit une liste des boîtiers de commande connectés et des événements de notification d’ajout ou de suppression d’un boîtier de commande.

### <a name="the-gamepads-list"></a>Liste des boîtiers de commande

La classe [Gamepad][] fournit la propriété statique [Gamepads][], qui correspond à une liste en lecture seule de tous les boîtiers de commande qui sont actuellement connectés. Vous serez certainement intéressé uniquement par certains des boîtiers de commande connectés. C’est pourquoi nous vous recommandons de gérer votre propre collection de boîtiers de commande au lieu d’utiliser ceux répertoriés par la propriété `Gamepads`.

L’exemple de code suivant copie tous les boîtiers de commande connectés dans une nouvelle collection.

```cpp
auto myGamepads = ref new Vector<Gamepad^>();

for (auto gamepad : Gamepad::Gamepads)
{
    // This code assumes that you're interested in all gamepads.
    myGamepads->Append(gamepad);
}
```

### <a name="adding-and-removing-gamepads"></a>Ajout et suppression de boîtiers de commande

Quand un boîtier de commande est ajouté ou supprimé, les événements [GamepadAdded][] et [GamepadRemoved][] sont déclenchés. Vous pouvez inscrire des gestionnaires pour ces deux événements afin d’effectuer le suivi des boîtiers de commande actuellement connectés.

L’exemple de code suivant démarre le suivi d’un boîtier de commande qui a été ajouté.

```cpp
Gamepad::GamepadAdded += ref new EventHandler<Gamepad^>(Platform::Object^, Gamepad^ args)
{
    // This code assumes that you're interested in all new gamepads.
    myGamepads->Append(args);
}
```

L’exemple de code suivant arrête le suivi d’un stick arcade qui a été supprimé.

```cpp
Gamepad::GamepadRemoved += ref new EventHandler<Gamepad^>(Platform::Object^, Gamepad^ args)
{
    unsigned int indexRemoved;

    if(myGamepads->IndexOf(args, &indexRemoved))
    {
        myGamepads->RemoveAt(indexRemoved);
    }
}
```

### <a name="users-and-headsets"></a>Utilisateurs et casques

Chaque boîtier de commande peut être associé à un compte d’utilisateur pour lier l’identité de l’utilisateur à son jeu, et peut être branché à un casque permettant d’utiliser plus facilement les fonctionnalités de chat vocal et dans le jeu. Pour en savoir plus sur le suivi des utilisateurs et l’utilisation de casques, consultez [Suivi des utilisateurs et de leurs périphériques](input-practices-for-games.md#tracking-users-and-their-devices) et [Casques](headset.md).

## <a name="reading-the-gamepad"></a>Lecture des entrées du boîtier de commande

Une fois que vous avez identifié le boîtier de commande qui vous intéresse, vous pouvez commencer à collecter les entrées de ce boîtier. Toutefois, contrairement à d’autres sortes d’entrées que vous connaissez peut-être, les boîtiers de commande ne communiquent pas les changements d’état en déclenchant des événements. À la place, vous devez _interroger_ régulièrement ces boîtiers de commande pour connaître leur état actuel.

### <a name="polling-the-gamepad"></a>Interrogation du boîtier de commande

Le processus d’interrogation capture un instantané du périphérique de navigation à un moment précis. Cette approche de collecte des entrées convient à la plupart des jeux, dont la logique s’exécute généralement selon une boucle déterministe plutôt que sur la base des événements. Par ailleurs, elle facilite l’interprétation des commandes de jeu, car toutes les entrées sont collectées en même temps, et non pas les unes après les autres.

Vous interrogez un boîtier de commande en appelant la fonction [GetCurrentReading][]. Cette fonction renvoie un élément [GamepadReading][] qui contient l’état du boîtier de commande.

L’exemple de code suivant interroge un boîtier de commande pour obtenir son état actuel.

```cpp
auto gamepad = myGamepads[0];

GamepadReading reading = gamepad->GetCurrentReading();
```

En plus de l’état du boîtier de commande, chaque valeur comprend un horodatage qui indique précisément le moment d’extraction de cet état. Cet horodatage est utile pour faire le lien avec le minutage des valeurs précédentes ou de la simulation de jeu.

### <a name="reading-the-thumbsticks"></a>Lecture des entrées des sticks analogiques

Chaque stick analogique fournit une valeur analogique située entre-1.0 et+1.0 sur les axesX etY. Sur l’axeX, une valeur de-1.0 correspond à la position de stick analogique la plus à gauche; une valeurde +1.0 indique la position la plus à droite. Sur l’axeY, une valeur de-1.0 correspond à la position de stick analogique la plus en bas; une valeurde +1.0 indique la position la plus en haut. Sur les deux axes, la valeur est proche de0.0 quand le stick analogique se trouve à la position centrale, mais la valeur exacte peut varier d’une lecture à l’autre. Les stratégies de compensation de cette variation sont décrites plus loin, dans cette section.

La valeur de l’axeX du stick analogique gauche est lue à partir de la propriété `LeftThumbstickX` de la structure [GamepadReading][]; la valeur de l’axeY est lue à partir de la propriété `LeftThumbstickY`. La valeur de l’axeX du stick analogique droit est lue à partir de la propriété `RightThumbstickX`; la valeur de l’axeY est lue à partir de la propriété `RightThumbstickY`.

```cpp
float leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
float leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0
float rightStickX = reading.RightThumbstickX; // returns a value between -1.0 and +1.0
float rightStickY = reading.RightThumbstickY; // returns a value between -1.0 and +1.0
```

Si vous examinez les valeurs que les sticks analogiques fournissent, vous remarquerez qu’ils ne fournissent pas toujours la valeur neutre0.0 quand ils se trouvent à la position centrale. Les sticks analogiques fournissent différentes valeurs proches de0.0 chaque fois qu’ils sont déplacés, puis remis à la position centrale. Pour compenser ces variations, vous pouvez implémenter une petite _zone morte_, qui correspond à une plage de valeurs proches de la position centrale idéale à ignorer. Pour implémenter une zone morte, vous pouvez déterminer de quelle distance le stick analogique a été déplacé à partir de sa position centrale, en ignorant les valeurs qui sont plus proches que la distance de référence spécifiée. Vous pouvez calculer la distance de manière approximative (elle n’est pas exacte du fait que les lectures des entrées de stick analogique sont essentiellement des valeurs polaires, et non planaires) en utilisant simplement le théorème de Pythagore. Vous obtenez ainsi une zone morte radiale.

L’exemple suivant illustre une zone morte radiale simple calculée à l’aide du théorème de Pythagore.

```cpp
float leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
float leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0

// choose a deadzone -- readings inside this radius are ignored.
const float deadzoneRadius = 0.1;
const float deadzoneSquared = deadzoneRadius * deadzoneRadius;

// Pythagorean theorem -- for a right triangle, hypotenuse^2 = (opposite side)^2 + (adjacent side)^2
auto oppositeSquared = leftStickY * leftStickY;
auto adjacentSquared = leftStickX * leftStickX;

// accept and process input if true; otherwise, reject and ignore it.
if((oppositeSquared + adjacentSquared) > deadzoneSquared)
{
    // input accepted, process it
}
```

Chaque stick analogique fonctionne également comme un bouton quand l’utilisateur appuie dessus vers l’intérieur. Pour plus d’informations sur la lecture de cette entrée, consultez [Lecture des entrées des boutons](#reading-the-buttons).

### <a name="reading-the-triggers"></a>Lecture des entrées des gâchettes

Les gâchettes sont représentées par des valeurs à virgule flottante comprises entre0.0 (gâchette entièrement relâchée) et1.0 (gâchette entièrement enclenchée). La valeur de la gâchette gauche est lue à partir de la propriété `LeftTrigger` de la structure [GamepadReading][]; la valeur de la gâchette droite est lue à partir de la propriété `RightTrigger`.

```cpp
float leftTrigger  = reading.LeftTrigger;  // returns a value between 0.0 and 1.0
float rightTrigger = reading.RightTrigger; // returns a value between 0.0 and 1.0
```

### <a name="reading-the-buttons"></a>Lecture des entrées des boutons

Chacun des boutons du boîtier de commande (le bouton à quatre directions, les gâchettes hautes gauche et droite, les sticks analogiques gauche et droit, et les boutons **A**, **B**, **X**, **Y**, **Afficher** et **Menu**) fournit une valeur numérique qui indique si le bouton est à l’état appuyé (position basse) ou relâché (position haute). Pour plus d’efficacité, les entrées de bouton ne sont pas représentées individuellement sous forme de valeurs booléennes. Elles sont toutes regroupées dans un seul champ de bits représenté par l’énumération [GamepadButtons][].

> **Remarque** La manette sans fil Xbox Elite Wireless Controller comporte quatre **palettes** supplémentaires à l’arrière. Ces boutons sont également représentés dans l’énumération `GamepadButtons` et leurs valeurs sont lues de la même façon que celles des boutons standard du boîtier de commande.

Les valeurs des boutons sont lues à partir de la propriété `Buttons` de la structure [GamepadReading][]. Comme cette propriété est un champ de bits, un masquage au niveau du bit est effectué pour isoler la valeur du bouton qui vous intéresse. Le bouton est à l’état appuyé (position basse) quand le bit correspondant est défini; sinon, il est à l’état relâché (position haute).

L’exemple suivant détermine si le boutonA est à l’état appuyé.

```cpp
if (GamepadButtons::A == (reading.Buttons & GamepadButtons::A))
{
    // button A is pressed
}
```

L’exemple suivant détermine si le boutonA est à l’état relâché.

```cpp
if (GamepadButtons::None == (reading.Buttons & GamepadButtons::A))
{
    // button A is pressed
}
```

Vous pouvez avoir besoin de savoir quand un bouton passe de l’état appuyé à l’état relâché, ou inversement, si plusieurs boutons sont à l’état appuyé ou relâché, ou si un groupe de boutons présente une disposition particulière, certains présentant l’état appuyé et d’autres l’état relâché. Pour plus d’informations sur la détection de chacun de ces états, consultez [Détecter les changements d’état des boutons](input-practices-for-games.md#detecting-button-transitions) et [Détecter les dispositions de boutons complexes](input-practices-for-games.md#detecting-complex-button-arrangements).

## <a name="run-the-gamepad-input-sample"></a>Exécuter l’exemple d’entrée de boîtier de commande

L’[exemple GamepadUWP _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/GamepadUWP) montre comment connecter un boîtier de commande et lire son état.


## <a name="vibration-and-impulse-triggers-overview"></a>Vue d’ensemble des gâchettes de vibration et à impulsions

Les moteurs de vibration intégrés au boîtier de commande sont conçus pour offrir une expérience tactile au joueur. Les jeux utilisent cette fonctionnalité pour renforcer la sensation d’immersion, mieux communiquer les informations d’état (par exemple, les dommages subis), signaler la proximité d’objets importants, ou pour d’autres usages créatifs.

Les boîtiers de commande XboxOne comportent au total quatre moteurs de vibration indépendants. Deux de ces moteurs sont de gros moteurs installés à l’intérieur du boîtier de commande. Celui de gauche fournit une forte vibration de grande amplitude, tandis que celui de droite fournit une vibration plus subtile et de plus faible amplitude. Les deux autres moteurs sont des petits moteurs intégrés à chaque gâchette. Ils déclenchent des vibrations rapides en série qui sont directement ressenties par le joueur qui manipulent les gâchettes (d’où le nom de _gâchette à impulsions_, qui est une fonctionnalité spécifique au boîtier de commande Xbox One). L’utilisation combinée de ces quatre moteurs permet de recréer une large palette de sensations tactiles.


## <a name="using-vibration-and-impulse"></a>Utilisation des vibrations et des impulsions

Les vibrations du boîtier de commande sont contrôlées par la propriété [Vibration][] de la classe [Gamepad][]. `Vibration` est une instance de la structure [GamepadVibration][] contenant quatre valeurs à virgule flottante, chacune d’elles représentant l’intensité d’un moteur.

Vous pouvez modifier directement les membres de la propriété `Gamepad.Vibration`, mais nous vous recommandons plutôt d’initialiser une instance `GamepadVibration` distincte avec les valeurs de votre choix, puis de la copier dans la propriété `Gamepad.Vibration` pour modifier simultanément toutes les intensités de moteur actuelles.

L’exemple suivant montre comment modifier simultanément toutes les intensités de moteur.

```cpp
// get the first gamepad
Gamepad^ gamepad = Gamepad::Gamepads->GetAt(0);

// create an instance of GamepadVibration
GamepadVibration vibration;

// ... set vibration levels on vibration struct here

// copy the GamepadVibration struct to the gamepad
gamepad.Vibration = vibration.
```

### <a name="using-the-vibration-motors"></a>Utilisation des moteurs de vibration

Les moteurs de vibration gauche et droite acceptent des valeurs à virgule flottante comprises entre0.0 (aucune vibration) et1.0 (intensité de vibration la plus élevée). L’intensité du moteur gauche est définie par la propriété `LeftMotor` de la structure [GamepadVibration][]; celle du moteur droit est définie par la propriété `RightMotor`.

L’exemple suivant définit l’intensité des deux moteurs de vibration et active la vibration du boîtier de commande.

```cpp
GamepadVibration vibration;
vibration.LeftMotor = 0.80;  // sets the intensity of the left motor to 80%
vibration.RightMotor = 0.25; // sets the intensity of the right motor to 25%
gamepad.Vibration = vibration;
```

N’oubliez pas que ces deux moteurs ne sont pas identiques. La définition de leurs propriétés à la même valeur ne produit donc pas la même vibration dans ces moteurs. Quelle que soit la valeur définie, le moteur gauche produit une vibration plus forte à faible fréquence que le moteur droit, lequel, pour la même valeur, produit une vibration plus subtile à haute fréquence. Même avec une valeur maximale, le moteur gauche ne peut pas produire une vibration à des fréquences aussi élevées que le moteur droit, et le moteur droit ne peut pas produire une vibration aussi forte que le moteur gauche. Toutefois, du fait que les deux moteurs sont connectés physiquement par le même boîtier de commande, les joueurs ne ressentent pas les vibrations de chaque moteur de manière totalement indépendante, et ce même si les moteurs présentent des caractéristiques distinctes et des intensités de vibration différentes. Cette disposition permet d’offrir aux joueurs une plus grande palette de sensations tactiles qu’avec deux moteurs identiques.

### <a name="using-the-impulse-triggers"></a>Utilisation des gâchettes à impulsions

Chaque moteur de gâchette à impulsions accepte une valeur à virgule flottante comprise entre0.0 (aucune vibration) et1.0 (intensité de vibration la plus élevée). L’intensité du moteur de la gâchette gauche est définie par la propriété `LeftTrigger` de la structure [GamepadVibration][]; celle du moteur de la gâchette droite est définie par la propriété `RightTrigger`.

L’exemple suivant définit l’intensité des deux moteurs des gâchettes à impulsions, et les active.

```cpp
GamepadVibration vibration;
vibration.LeftTrigger = 0.75;  // sets the intensity of the left trigger to 75%
vibration.RightTrigger = 0.50; // sets the intensity of the right trigger to 50%
gamepad.Vibration = vibration;
```

Contrairement aux autres moteurs, les deux moteurs de vibration intégrés aux gâchettes sont identiques et fournissent chacun la même vibration pour une valeur identique. Toutefois, du fait que ces moteurs ne sont pas connectés physiquement, les joueurs ressentent les vibrations de chaque moteur de manière indépendante. Cette disposition permet au joueur de ressentir simultanément des sensations distinctes au niveau des deux gâchettes, tout en facilitant la communication d’informations plus précises que les moteurs installés physiquement dans le boîtier de commande.


## <a name="run-the-gamepad-vibration-sample"></a>Exécuter l’exemple de vibration du boîtier de commande

L’[exemple GamepadVibrationUWP _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/GamepadVibrationUWP) montre comment utiliser les moteurs de vibration et les gâchettes à impulsions du boîtier de commande pour produire des effets variés.

## <a name="see-also"></a>Voir aussi
[Windows.Gaming.Input.UINavigationController][]
[Windows.Gaming.Input.IGameController][]


[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[Windows.Gaming.Input.UINavigationController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.aspx
[Windows.Gaming.Input.IGameController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[gamepad]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.aspx
[gamepads]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.gamepads.aspx
[gamepadadded]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.gamepadadded.aspx
[gamepadremoved]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.gamepadremoved.aspx
[getcurrentreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.getcurrentreading.aspx
[vibration]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.vibration.aspx
[gamepadreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepadreading.aspx
[gamepadbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepadbuttons.aspx
[gamepadvibration]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepadvibration.aspx