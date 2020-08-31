---
title: Pratiques d’entrée pour les jeux
description: Découvrez les modèles et les techniques permettant d’utiliser efficacement des appareils d’entrée dans les jeux plateforme Windows universelle (UWP).
ms.assetid: CBAD3345-3333-4924-B6D8-705279F52676
ms.date: 11/20/2017
ms.topic: article
keywords: Windows 10, UWP, jeux, entrée
ms.localizationpriority: medium
ms.openlocfilehash: eb543e86221f8f1a37565c2e6e6bf1fe4a8d3635
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89054489"
---
# <a name="input-practices-for-games"></a>Pratiques d’entrée pour les jeux

Cette rubrique décrit les modèles et les techniques permettant d’utiliser efficacement des appareils d’entrée dans les jeux plateforme Windows universelle (UWP).

En lisant cette rubrique, vous apprendrez à :

* Comment suivre les joueurs et les périphériques d’entrée et de navigation qu’ils utilisent
* Comment détecter les transitions de bouton (appuyé à relâché, relâché à appuyé)
* Comment détecter les dispositions de boutons complexes à l’aide d’un seul et même test

## <a name="choosing-an-input-device-class"></a>Choix d’une classe de périphérique d’entrée

De nombreux types d’API d’entrée sont disponibles, tels que [ArcadeStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.arcadestick), [Flightstick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick)et [manette](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad). Comment choisir l’API à utiliser pour votre jeu ?

Vous devez choisir l’API qui vous donne l’entrée la plus appropriée pour votre jeu. Par exemple, si vous effectuez un jeu de plateforme 2D, vous pouvez probablement utiliser simplement la classe de **manette** de jeu et ne pas vous soucier des fonctionnalités supplémentaires disponibles via d’autres classes. Cela limiterait le jeu à la prise en charge des boîtiers de soucodeurs et fournira une interface cohérente qui fonctionnera sur différents boîtiers de manette sans nécessiter de code supplémentaire.

En revanche, pour les simulations de course et de vol complexes, vous pouvez énumérer tous les objets [RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller) comme ligne de base pour vous assurer qu’ils prennent en charge tout appareil de niche que les joueurs passionnés peuvent avoir, y compris des appareils tels que des pédales ou des accélérateurs distincts qui sont toujours utilisés par un joueur unique. 

À partir de là, vous pouvez utiliser la méthode **FromGameController** d’une classe d’entrée, telle que le dispositif de formatage [. FromGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad.fromgamecontroller), pour voir si chaque appareil dispose d’une vue plus organisée. Par exemple, si l’appareil est également un **boîtier**de sélection, vous souhaiterez peut-être ajuster l’interface utilisateur de mappage de bouton pour refléter cela et fournir des mappages de boutons par défaut cohérents. (Par opposition, il est nécessaire que le lecteur configure manuellement les entrées du boîtier de si vous utilisez uniquement **RawGameController**.) 

Vous pouvez également consulter l’ID de fournisseur (VID) et l’ID de produit (PID) d’un **RawGameController** (à l’aide de [HardwareVendorId](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.HardwareVendorId) et de [HardwareProductId](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.HardwareProductId), respectivement) et fournir des mappages de boutons suggérés pour les périphériques populaires, tout en restant compatibles avec les appareils inconnus qui arrivent à l’avenir par le biais de mappages manuels par le joueur.

## <a name="keeping-track-of-connected-controllers"></a>Suivi des contrôleurs connectés

Bien que chaque type de contrôleur comprenne une liste de contrôleurs connectés (tels que les [boîtiers](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad.Gamepads)de commande), il est judicieux de tenir à jour votre propre liste de contrôleurs. Pour plus d’informations, consultez [la liste des boîtiers](gamepad-and-vibration.md#the-gamepads-list) de commande (chaque type de contrôleur a une section portant le même nom dans sa propre rubrique).

Toutefois, que se passe-t-il lorsque le joueur déconnecte son contrôleur ou en fait une nouvelle ? Vous devez gérer ces événements et mettre à jour votre liste en conséquence. Pour plus d’informations, consultez [Ajout et suppression de boîtiers](gamepad-and-vibration.md#adding-and-removing-gamepads) de commande (là encore, chaque type de contrôleur a une section portant le même nom dans sa propre rubrique).

Étant donné que les événements ajoutés et supprimés sont déclenchés de façon asynchrone, vous risquez d’obtenir des résultats incorrects lors de la gestion de votre liste de contrôleurs. Par conséquent, chaque fois que vous accédez à votre liste de contrôleurs, vous devez mettre en place un verrou pour qu’un seul thread puisse y accéder à la fois. Pour ce faire, vous pouvez utiliser le [Runtime d’accès concurrentiel](https://docs.microsoft.com/cpp/parallel/concrt/concurrency-runtime), en particulier la [classe critical_section](https://docs.microsoft.com/cpp/parallel/concrt/reference/critical-section-class), dans ** &lt; &gt; ppl. h**.

Une autre chose à prendre en compte est que la liste des contrôleurs connectés est initialement vide et prend une deuxième ou deux à remplir. Par conséquent, si vous assignez uniquement le boîtier en mode en cours dans la méthode Start, il sera **null**!

Pour remédier à cela, vous devez disposer d’une méthode qui « actualise » le boîtier principal (dans un jeu à un seul joueur ; les jeux multijoueur nécessitent des solutions plus sophistiquées). Vous devez ensuite appeler cette méthode à la fois dans votre contrôleur ajouté et dans votre méthode de mise à jour.

La méthode suivante retourne simplement le premier boîtier de commande dans la liste (ou **nullptr** si la liste est vide). Ensuite, n’oubliez pas de vérifier si **nullptr** chaque fois que vous faites quoi que ce soit avec le contrôleur. C’est à vous de décider si vous souhaitez bloquer le jeu quand aucun contrôleur n’est connecté (par exemple, en suspendant le jeu) ou que le jeu se poursuit, tout en ignorant l’entrée.

```cpp
#include <ppl.h>

using namespace Platform::Collections;
using namespace Windows::Gaming::Input;
using namespace concurrency;

Vector<Gamepad^>^ m_myGamepads = ref new Vector<Gamepad^>();

Gamepad^ GetFirstGamepad()
{
    Gamepad^ gamepad = nullptr;
    critical_section::scoped_lock{ m_lock };

    if (m_myGamepads->Size > 0)
    {
        gamepad = m_myGamepads->GetAt(0);
    }

    return gamepad;
}
```

En plaçant tous les éléments, voici un exemple de gestion des entrées à partir d’un boîtier de souscription :

```cpp
#include <algorithm>
#include <ppl.h>

using namespace Platform::Collections;
using namespace Windows::Foundation;
using namespace Windows::Gaming::Input;
using namespace concurrency;

static Vector<Gamepad^>^ m_myGamepads = ref new Vector<Gamepad^>();
static Gamepad^          m_gamepad = nullptr;
static critical_section  m_lock{};

void Start()
{
    // Register for gamepad added and removed events.
    Gamepad::GamepadAdded += ref new EventHandler<Gamepad^>(&OnGamepadAdded);
    Gamepad::GamepadRemoved += ref new EventHandler<Gamepad^>(&OnGamepadRemoved);

    // Add connected gamepads to m_myGamepads.
    for (auto gamepad : Gamepad::Gamepads)
    {
        OnGamepadAdded(nullptr, gamepad);
    }
}

void Update()
{
    // Update the current gamepad if necessary.
    if (m_gamepad == nullptr)
    {
        auto gamepad = GetFirstGamepad();

        if (m_gamepad != gamepad)
        {
            m_gamepad = gamepad;
        }
    }

    if (m_gamepad != nullptr)
    {
        // Gather gamepad reading.
    }
}

// Get the first gamepad in the list.
Gamepad^ GetFirstGamepad()
{
    Gamepad^ gamepad = nullptr;
    critical_section::scoped_lock{ m_lock };

    if (m_myGamepads->Size > 0)
    {
        gamepad = m_myGamepads->GetAt(0);
    }

    return gamepad;
}

void OnGamepadAdded(Platform::Object^ sender, Gamepad^ args)
{
    // Check if the just-added gamepad is already in m_myGamepads; if it isn't, 
    // add it.
    critical_section::scoped_lock lock{ m_lock };
    auto it = std::find(begin(m_myGamepads), end(m_myGamepads), args);

    if (it == end(m_myGamepads))
    {
        m_myGamepads->Append(args);
    }
}

void OnGamepadRemoved(Platform::Object^ sender, Gamepad^ args)
{
    // Remove the gamepad that was just disconnected from m_myGamepads.
    unsigned int indexRemoved;
    critical_section::scoped_lock lock{ m_lock };

    if (m_myGamepads->IndexOf(args, &indexRemoved))
    {
        if (m_gamepad == m_myGamepads->GetAt(indexRemoved))
        {
            m_gamepad = nullptr;
        }

        m_myGamepads->RemoveAt(indexRemoved);
    }
}
```

## <a name="tracking-users-and-their-devices"></a>Suivi des utilisateurs et de leurs périphériques

Tous les périphériques d’entrée sont associés à un [utilisateur](https://docs.microsoft.com/uwp/api/windows.system.user) afin que son identité puisse être liée à sa séquence de jeu, ses succès, ses modifications de paramètres et ses autres activités. Les utilisateurs peuvent se connecter ou se déconnecter de, et il est courant qu’un autre utilisateur se connecte sur un appareil d’entrée restant connecté au système après la déconnexion de l’utilisateur précédent. Quand un utilisateur se connecte ou sort, l’événement [IGameController. UserChanged](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller.UserChanged) est déclenché. Vous pouvez inscrire un gestionnaire d’événements pour cet événement afin d’effectuer le suivi des joueurs et des périphériques qu’ils utilisent.

L’identité de l’utilisateur est également la manière dont un appareil d’entrée est associé à son [contrôleur de navigation d’interface utilisateur](ui-navigation-controller.md)correspondant.

Pour ces raisons, les entrées de lecteur doivent être suivies et corrélées avec la propriété [User](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller.User) de la classe Device (héritée de l’interface [IGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller) ).

L’exemple [UserGamepadPairingUWP](/samples/microsoft/xbox-atg-samples/usergamepadpairinguwp/) montre comment vous pouvez effectuer le suivi des utilisateurs et des appareils qu’ils utilisent.

## <a name="detecting-button-transitions"></a>Détection de transitions de boutons

Vous souhaiterez savoir parfois quand un bouton est d’abord enfoncé ou relâché, autrement dit lorsque l’état du bouton passe de relâché à appuyé, ou inversement. Pour le déterminer, vous devez mémoriser la lecture précédente du périphérique et la comparer à la lecture actuelle pour voir ce qui a changé.

L’exemple suivant illustre une approche de base pour mémoriser la lecture précédente. les boîtiers de soumanchement sont présentés ici, mais les principes sont les mêmes pour les deux types d’appareils d’entrée.

```cpp
Gamepad gamepad;
GamepadReading newReading();
GamepadReading oldReading();

// Called at the start of the game.
void Game::Start()
{
    gamepad = Gamepad::Gamepads[0];
}

// Game::Loop represents one iteration of a typical game loop
void Game::Loop()
{
    // move previous newReading into oldReading before getting next newReading
    oldReading = newReading, newReading = gamepad.GetCurrentReading();

    // process device readings using buttonJustPressed/buttonJustReleased (see below)
}
```

Avant toute autre action, `Game::Loop` déplace la valeur existante de `newReading` (lecture du boîtier de commande de l’itération de boucle précédente) dans `oldReading`, puis renseigne `newReading` avec une nouvelle lecture du boîtier de commande correspondant à l’itération actuelle. Vous disposez alors des informations nécessaires pour détecter les transitions de boutons.

L’exemple suivant illustre une approche de base pour la détection des transitions de bouton :

```cpp
bool ButtonJustPressed(const GamepadButtons selection)
{
    bool newSelectionPressed = (selection == (newReading.Buttons & selection));
    bool oldSelectionPressed = (selection == (oldReading.Buttons & selection));

    return newSelectionPressed && !oldSelectionPressed;
}

bool ButtonJustReleased(GamepadButtons selection)
{
    bool newSelectionReleased =
        (GamepadButtons.None == (newReading.Buttons & selection));

    bool oldSelectionReleased =
        (GamepadButtons.None == (oldReading.Buttons & selection));

    return newSelectionReleased && !oldSelectionReleased;
}
```

Ces deux fonctions dérivent d’abord l’état booléen de la sélection de bouton à partir de `newReading` et `oldReading` , puis exécutent une logique booléenne pour déterminer si la transition cible s’est produite. Ces fonctions retournent **true** uniquement si la nouvelle lecture contient l’état cible (appuyé ou relâché, respectivement) *et* si l’ancienne lecture ne contient pas également l’état cible. Dans le cas contraire, elles retournent **false**.

## <a name="detecting-complex-button-arrangements"></a>Détection des dispositions de boutons complexes

Chaque bouton d’un appareil d’entrée fournit une lecture numérique qui indique s’il est appuyé (enfoncé) ou relâché (haut). Pour plus d’efficacité, les entrées de bouton ne sont pas représentées individuellement sous forme de valeurs booléennes. Elles sont toutes regroupées dans des champs de bits représentés par des énumérations propres aux périphériques, par exemple [GamepadButtons](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepadbuttons). Pour lire des boutons spécifiques, un masquage au niveau du bit est effectué pour isoler les valeurs qui vous intéressent. Un bouton est enfoncé (enfoncé) lorsque son bit correspondant est défini ; dans le cas contraire, elle est libérée (haut).

Rappelez-vous que les boutons uniques sont activés ou relâchés ; les boîtiers de soumanchement sont présentés ici, mais les principes sont les mêmes pour les deux types d’appareils d’entrée.

```cpp
GamepadReading reading = gamepad.GetCurrentReading();

// Determines whether gamepad button A is pressed.
if (GamepadButtons::A == (reading.Buttons & GamepadButtons::A))
{
    // The A button is pressed.
}

// Determines whether gamepad button A is released.
if (GamepadButtons::None == (reading.Buttons & GamepadButtons::A))
{
    // The A button is released (not pressed).
}
```

Comme vous pouvez le voir, la détermination de l’état d’un seul bouton est simple, mais il peut arriver que vous souhaitiez déterminer si plusieurs boutons sont enfoncés ou libérés, ou si un ensemble de boutons est organisé d’une façon particulière &mdash; , d’autres non. Le test de plusieurs boutons est plus complexe que le test de boutons uniques &mdash; , en particulier avec le potentiel d’état de bouton mixte &mdash; , mais il existe une formule simple pour ces tests qui s’applique aux tests à un ou plusieurs boutons.

L’exemple suivant détermine si les boutons du boîtier de commande A et B sont appuyés :

```cpp
if ((GamepadButtons::A | GamepadButtons::B) == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // The A and B buttons are both pressed.
}
```

L’exemple suivant détermine si les boutons du boîtier de commande A et B sont tous deux publiés :

```cpp
if ((GamepadButtons::None == (reading.Buttons & GamepadButtons::A | GamepadButtons::B))
{
    // The A and B buttons are both released (not pressed).
}
```

L’exemple suivant détermine si le bouton de manette a est enfoncé alors que le bouton B est relâché :

```cpp
if (GamepadButtons::A == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // The A button is pressed and the B button is released (B is not pressed).
}
```

Dans la formule que ces cinq exemples ont en commun, la disposition des boutons à tester est spécifiée par l’expression située à gauche de l’opérateur d’égalité tandis que les boutons à examiner sont sélectionnés par l’expression de masquage à droite.

L’exemple suivant illustre cette formule plus clairement en réécrivant l’exemple précédent :

```cpp
auto buttonArrangement = GamepadButtons::A;
auto buttonSelection = (reading.Buttons & (GamepadButtons::A | GamepadButtons::B));

if (buttonArrangement == buttonSelection)
{
    // The A button is pressed and the B button is released (B is not pressed).
}
```

Cette formule peut être appliquée pour tester n’importe quel nombre de boutons, avec toutes les dispositions de leur état.

## <a name="get-the-state-of-the-battery"></a>Récupération de l’état de la batterie

Pour tout contrôleur de jeu qui implémente l’interface [IGameControllerBatteryInfo](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontrollerbatteryinfo) , vous pouvez appeler [TryGetBatteryReport](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontrollerbatteryinfo.TryGetBatteryReport) sur l’instance de contrôleur pour obtenir un objet [BatteryReport](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport) qui fournit des informations sur la batterie dans le contrôleur. Vous pouvez obtenir des propriétés telles que la fréquence de chargement de la batterie ([ChargeRateInMilliwatts](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport.ChargeRateInMilliwatts)), la capacité énergétique estimée d’une nouvelle batterie ([DesignCapacityInMilliwattHours](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport.DesignCapacityInMilliwattHours)) et la capacité énergétique entièrement facturée de la batterie actuelle ([FullChargeCapacityInMilliwattHours](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport.FullChargeCapacityInMilliwattHours)).

Pour les contrôleurs de jeu qui prennent en charge la création de rapports de batterie détaillés, vous pouvez obtenir ces informations et plus d’informations sur la batterie, comme indiqué dans [obtenir des informations](../devices-sensors/get-battery-info.md)sur la batterie. Toutefois, la plupart des contrôleurs de jeu ne prennent pas en charge ce niveau de signalement de batterie et utilisent à la place un matériel à faible coût. Pour ces contrôleurs, vous devez garder à l’esprit les points suivants :

* **ChargeRateInMilliwatts** et **DesignCapacityInMilliwattHours** auront toujours la **valeur null**.

* Vous pouvez récupérer le pourcentage de batterie en calculant [RemainingCapacityInMilliwattHours](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport.RemainingCapacityInMilliwattHours)  /  **FullChargeCapacityInMilliwattHours**. Vous devez ignorer les valeurs de ces propriétés et traiter uniquement le pourcentage calculé.

* Le pourcentage du point de puce précédent sera toujours l’un des suivants :

    * 100% (complet)
    * 70% (moyen)
    * 40% (faible)
    * 10% (critique)

Si votre code effectue une action (comme l’interface utilisateur de dessin) en fonction du pourcentage de durée de vie de la batterie restante, assurez-vous qu’il est conforme aux valeurs ci-dessus. Par exemple, si vous souhaitez avertir le joueur lorsque la batterie du contrôleur est faible, faites-le quand il atteint 10%.

## <a name="see-also"></a>Voir aussi

* [Windows.SysTEM. Classe d’utilisateur](https://docs.microsoft.com/uwp/api/windows.system.user)
* [Interface Windows. Gaming. Input. IGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller)
* [Enum Windows. Gaming. Input. GamepadButtons](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepadbuttons)
* [Exemple UserGamepadPairingUWP](/samples/microsoft/xbox-atg-samples/usergamepadpairinguwp/)