---
title: Volant de course et retour de force
description: Utilisez les API de volant de course Windows.Gaming.Input pour détecter, déterminer les fonctionnalités, lire et envoyer des commandes de retour de force aux volants de course.
ms.assetid: 6287D87F-6F2E-4B67-9E82-3D6E51CBAFF9
ms.date: 05/09/2018
ms.topic: article
keywords: Windows 10, UWP, jeux, volant de course, retour de force
ms.localizationpriority: medium
ms.openlocfilehash: 0d92fe45a008ee0c3864355faf8133ab8e1ebefb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163063"
---
# <a name="racing-wheel-and-force-feedback"></a>Volant de course et retour de force

Cette page décrit les principes fondamentaux de la programmation pour Xbox One Racing Wheels à l’aide de [Windows. Gaming. Input. RacingWheel][racingwheel] et des API associées pour la plateforme Windows universelle (UWP).

Voici ce que vous allez apprendre à la lecture de cet article :

* Comment rassembler une liste de roues de course connectées et leurs utilisateurs
* Comment détecter qu’une roue de course a été ajoutée ou supprimée
* lecture d’entrées à partir d’une ou de plusieurs roues de course
* Envoyer des commandes de retour de force
* Comment les roues de course se comportent comme des appareils de navigation d’interface utilisateur

## <a name="racing-wheel-overview"></a>Volant de course : présentation

Un volant de course est un périphérique d’entrée dont l’utilisation rappelle celle du volant d’une voiture de course réelle. Les volants de course sont idéaux pour les jeux de course de type arcade ou simulation, qui mettent en scène des voitures ou des camions. Ils sont pris en charge dans les applications UWP Windows 10 et Xbox One, via l’espace de noms [Windows.Gaming.Input](/uwp/api/windows.gaming.input).

Les roues d’une course Xbox One sont proposées à un large éventail de tarifs. en général, ils disposent de plus en plus de fonctionnalités d’entrée et de retour en force à mesure que leurs prix augmentent. Toutes les roues de course sont équipées d’une roue de direction analogique, d’une limitation analogique et de contrôles de frein, ainsi que de certains boutons sur la roue. Certaines roues de course sont également équipées d’un embrayage analogique et de contrôles de HandBrake, de décalages de motifs et de fonctionnalités de retour de force. Les roues de course ne sont pas toutes équipées des mêmes ensembles de fonctionnalités. elles peuvent également varier dans leur prise en charge de certaines fonctionnalités &mdash; . par exemple, les roues directrices peuvent prendre en charge des plages de rotation et des décalages de motifs différents.

### <a name="device-capabilities"></a>Fonctionnalités de l’appareil

Les différentes roues d’une course Xbox One offrent différents ensembles de fonctionnalités d’appareil facultatives et différents niveaux de prise en charge de ces fonctionnalités. ce niveau de variation entre un type d’appareil d’entrée unique est unique parmi les appareils pris en charge par l’API [Windows. Gaming. Input](/uwp/api/windows.gaming.input) . De plus, la plupart des périphériques que vous gérerez prendront en charge au moins quelques fonctionnalités ou autres variantes en option. Pour cette raison, il est important de déterminer les fonctionnalités de chaque roue de course connectée individuellement et de prendre en charge la variation complète des fonctionnalités qui sont pertinentes pour votre jeu.

Pour en savoir plus, voir [Identification des fonctionnalités de volant de course](#determining-racing-wheel-capabilities).

### <a name="force-feedback"></a>Retour de force

Certaines roues de course Xbox One offrent des retours de force véritables &mdash; , elles peuvent appliquer des forces réelles sur un axe de contrôle, comme leur volant, et &mdash; non simplement des vibrations simples. Les jeux utilisent cette possibilité pour créer un plus grand sens de l’immersion (_simulation d’endommagement des pannes_, « sensation de route ») et pour accroître le défi de la bonne marche.

Pour en savoir plus, voir [Présentation du retour de force](#force-feedback-overview).

### <a name="ui-navigation"></a>Navigation d’interface utilisateur

Pour réduire la difficulté associée à la prise en charge de plusieurs périphériques d’entrée différents lors de la navigation dans l’interface utilisateur, et pour favoriser la cohérence entre les jeux et les périphériques, la plupart des périphériques d’entrée _physiques_ agissent en tant que périphérique d’entrée _logique_ distinct, appelé [contrôleur de navigation d’interface utilisateur](ui-navigation-controller.md). Le contrôleur de navigation d’interface utilisateur fournit un vocabulaire commun pour les commandes de navigation dans l’interface utilisateur, sur plusieurs périphériques d’entrée.

En raison de leur point de vue unique sur les contrôles analogiques et du degré de variation entre les différentes roues de course, elles sont généralement équipées d’un pavé numérique D-Pad, **View**, **menu**, **a**, **B**, **X**et **Y** , qui ressemblent à ceux d’un [boîtier](gamepad-and-vibration.md)de commande Ces boutons ne sont pas conçus pour prendre en charge les commandes de jeu et ne peuvent pas être facilement accessibles en tant que boutons de roulette de course.

En tant que contrôleur de navigation de l’interface utilisateur, les roues de course mappent le jeu de commandes de navigation [requis](ui-navigation-controller.md#required-set) sur les boutons du joystick gauche, du pavé D-Pad, de l' **affichage**, du **menu**, **d’un**et de **B** .

| Commande de navigation | Entrée volant de course |
| ------------------:| ------------------ |
|                 Haut | Bouton multidirectionnel vers le haut           |
|               Descendre | Bouton multidirectionnel vers le bas         |
|               Gauche | Bouton multidirectionnel vers la gauche         |
|              Right | Bouton multidirectionnel vers la droite        |
|               Affichage | Bouton Afficher        |
|               Menu | Bouton Menu        |
|             Acceptation | Bouton A           |
|             Annuler | Bouton B           |

De plus, certains volants de course peuvent mapper certaines commandes d’un [ensemble de commandes de navigation en option](ui-navigation-controller.md#optional-set) à d’autres entrées prises en charge, mais ces mappages peuvent varier d’un appareil à l’autre. Envisagez également la prise en charge de ces commandes, en vous assurant qu’elles ne sont pas essentielles à la navigation au sein de l’interface de votre jeu.

| Commande de navigation | Entrée volant de course    |
| ------------------:| --------------------- |
|            Page précédente | _diffère_              |
|          Page suivante | _diffère_              |
|          Page vers la gauche | _diffère_              |
|         Page vers la droite | _diffère_              |
|          Faire défiler vers le haut | _diffère_              |
|        Faire défiler vers le bas | _diffère_              |
|        Faire défiler vers la gauche | _diffère_              |
|       Faire défiler vers la droite | _diffère_              |
|          Contexte 1 | Bouton X (_généralement_) |
|          Contexte 2 | Bouton Y (_en général_) |
|          Contexte 3 | _diffère_              |
|          Contexte 4 | _diffère_              |

## <a name="detect-and-track-racing-wheels"></a>Détecter et effectuer le suivi des volants de course

La détection et le suivi des roues de course fonctionnent exactement de la même façon que pour les boîtiers de manette, à l’exception de la classe [RacingWheel](/uwp/api/windows.gaming.input.racingwheel) au lieu de la classe de [boîtier](/uwp/api/Windows.Gaming.Input.Gamepad) . Pour plus d’informations, consultez [boîtier et vibration](gamepad-and-vibration.md) .

<!-- Racing wheels are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected racing wheels and events to notify you when a racing wheel is added or removed.

### The racing wheels list

The [RacingWheel](/uwp/api/windows.gaming.input.racingwheel) class provides a static property, [RacingWheels](/uwp/api/windows.gaming.input.racingwheel.racingwheels#Windows_Gaming_Input_RacingWheel_RacingWheels), which is a read-only list of racing wheels that are currently connected. Because you might only be interested in some of the connected racing wheels, it's recommended that you maintain your own collection instead of accessing them through the `RacingWheels` property.

The following example copies all connected racing wheels into a new collection.
```cpp
auto myRacingWheels = ref new Vector<RacingWheel^>();

for (auto racingwheel : RacingWheel::RacingWheels)
{
    // This code assumes that you're interested in all racing wheels.
    myRacingWheels->Append(racingwheel);
}
```

### Adding and removing racing wheels

When a racing wheel is added or removed the [RacingWheelAdded](/uwp/api/windows.gaming.input.racingwheel.racingwheeladded) and [RacingWheelRemoved](/uwp/api/windows.gaming.input.racingwheel.racingwheelremoved) events are raised. You can register handlers for these events to keep track of the racing wheels that are currently connected.

The following example starts tracking an racing wheels that's been added.
```cpp
RacingWheel::RacingWheelAdded += ref new EventHandler<RacingWheel^>(Platform::Object^, RacingWheel^ args)
{
    // This code assumes that you're interested in all new racing wheels.
    myRacingWheels->Append(args);
}
```

The following example stops tracking a racing wheel that's been removed.
```cpp
RacingWheel::RacingWheelRemoved += ref new EventHandler<RacingWheel^>(Platform::Object^, RacingWheel^ args)
{
    unsigned int indexRemoved;

    if(myRacingWheels->IndexOf(args, &indexRemoved))
    {
        myRacingWheels->RemoveAt(indexRemoved);
    }
}
```

### Users and headsets

Each racing wheel can be associated with a user account to link their identity to their gameplay, and can have a headset attached to facilitate voice chat or in-game features. To learn more about working with users and headsets, see [Tracking users and their devices](input-practices-for-games.md#tracking-users-and-their-devices) and [Headset](headset.md). -->

## <a name="reading-the-racing-wheel"></a>Lecture des données du volant de course

Après avoir identifié les roues de course qui vous intéressent, vous êtes prêt à recueillir des informations à partir de celles-ci. Toutefois, contrairement à d’autres sortes d’entrées que vous connaissez peut-être, les volants de course ne communiquent pas les changements d’état en déclenchant des événements. Au lieu de cela, vous prenez des lectures régulières de leurs États actuels en les _interrogeant_ .

### <a name="polling-the-racing-wheel"></a>Interrogation du volant de course

Le processus d’interrogation capture un instantané du volant de course à un moment précis. Cette approche de la collecte des entrées est adaptée à la plupart des jeux, car leur logique s’exécute généralement dans une boucle déterministe plutôt que d’être pilotée par des événements. en général, il est généralement plus simple d’interpréter les commandes de jeu provenant d’entrées rassemblées en une seule fois par rapport à de nombreuses entrées collectées au fil du temps.

Vous interroger une roue de course en appelant [GetCurrentReading](/uwp/api/windows.gaming.input.racingwheel.getcurrentreading#Windows_Gaming_Input_RacingWheel_GetCurrentReading); Cette fonction retourne un [RacingWheelReading](/uwp/api/windows.gaming.input.racingwheelreading) qui contient l’état de la roue de course.

L’exemple de code suivant interroge un volant de course pour obtenir son état actuel.

```cpp
auto racingwheel = myRacingWheels[0];

RacingWheelReading reading = racingwheel->GetCurrentReading();
```

En plus de l’état du volant de course, chaque valeur comprend un horodatage qui indique précisément le moment d’extraction de cet état. Cet horodatage est utile pour faire le lien avec le minutage des valeurs précédentes ou de la simulation de jeu.

### <a name="determining-racing-wheel-capabilities"></a>Identification des fonctionnalités de volant de course

La plupart des contrôles du volant de course sont facultatifs ou prennent en charge des variantes, même dans les contrôles requis ; vous devez donc déterminer les fonctionnalités de chaque volant de course avant de pouvoir traiter les entrées regroupées dans chaque valeur du volant de course.

Les contrôles en option sont les suivants : frein à main, embrayage et leviers de vitesses ; vous pouvez déterminer si un volant de course connecté gère ces contrôles en lisant les propriétés [HasHandbrake](/uwp/api/windows.gaming.input.racingwheel.hashandbrake#Windows_Gaming_Input_RacingWheel_HasHandbrake), [HasClutch](/uwp/api/windows.gaming.input.racingwheel.hasclutch#Windows_Gaming_Input_RacingWheel_HasClutch) et [HasPatternShifter](/uwp/api/windows.gaming.input.racingwheel.haspatternshifter#Windows_Gaming_Input_RacingWheel_HasPatternShifter) du volant de course, respectivement. Le contrôle est pris en charge si la valeur de la propriété est **true**; dans le cas contraire, il n’est pas pris en charge.

```cpp
if (racingwheel->HasHandbrake)
{
    // the handbrake is supported
}

if (racingwheel->HasClutch)
{
    // the clutch is supported
}

if (racingwheel->HasPatternShifter)
{
    // the pattern shifter is supported
}
```

De plus, les contrôles susceptibles de varier sont le volant et le levier de vitesses. La variation du volant peut correspondre au degré de rotation physique qu’un volant réel peut prendre en charge, alors que celle du levier de vitesses peut correspondre au nombre de vitesses avant qu’il gère. Vous pouvez déterminer l’angle de rotation le plus étendu pris en charge par le volant réel en lisant la propriété `MaxWheelAngle` du volant de course ; cette valeur correspond à l’angle physique maximal pris en charge, en degrés, dans le sens des aiguilles d’une montre (positif), également géré dans le sens contraire (négatif). Vous pouvez déterminer le plus grand engrenage que le changement de modèle prend en charge en lisant la `MaxPatternShifterGear` propriété de la roue de course. sa valeur est l’engrenage le plus élevé pris en charge, inclus &mdash; , si sa valeur est 4, le décalage de motif prend en charge les vitesses inverse, neutre, premier, deuxième, troisième et quatrième.

```cpp
auto maxWheelDegrees = racingwheel->MaxWheelAngle;
auto maxShifterGears = racingwheel->MaxPatternShifterGear;
```

Pour finir, certains volants de course prennent en charge le retour de force. Vous pouvez déterminer si un volant connecté gère le retour de force en lisant la propriété [WheelMotor](/uwp/api/windows.gaming.input.racingwheel.wheelmotor#Windows_Gaming_Input_RacingWheel_WheelMotor) associée au volant. Le retour de force est pris en charge si `WheelMotor` n’est pas **null**; sinon, il n’est pas pris en charge.

```cpp
if (racingwheel->WheelMotor != nullptr)
{
    // force feedback is supported
}
```

Pour en savoir plus sur l’utilisation de la fonction de retour de force des volants de course qui la prennent en charge, voir [Présentation du retour de force](#force-feedback-overview).

### <a name="reading-the-buttons"></a>Lecture des boutons

Chacun des boutons de la roulette de course &mdash; les quatre directions du BMD, les boutons de l' **engrenage précédent** et de l' **engrenage suivant** , ainsi que les 16 boutons supplémentaires &mdash; fournissent une lecture numérique qui indique s’il est appuyé (enfoncé) ou relâché (haut). Pour plus d’efficacité, les entrées de bouton ne sont pas représentées individuellement sous forme de valeurs booléennes. Elles sont toutes regroupées dans un seul champ de bits représenté par l’énumération [RacingWheelButtons](/uwp/api/windows.gaming.input.racingwheelbuttons).

> [!NOTE]
> Les roues de course sont équipées de boutons supplémentaires utilisés pour la navigation de l’interface utilisateur, tels que les boutons de **menu** et d' **affichage** . Ces boutons ne font pas partie de l’énumération `RacingWheelButtons` ; vous ne pouvez lire leurs valeurs qu’en accédant au volant de navigation en tant que périphérique de navigation dans l'interface utilisateur. Pour plus d’informations, consultez [Périphérique de navigation d’interface utilisateur](ui-navigation-controller.md).

Les valeurs des boutons sont lues à partir de la propriété `Buttons` de la structure [RacingWheelReading](/uwp/api/windows.gaming.input.racingwheelreading). Comme cette propriété est un champ de bits, un masquage au niveau du bit est effectué pour isoler la valeur du bouton qui vous intéresse. Le bouton est enfoncé (enfoncé) lorsque le bit correspondant est défini ; dans le cas contraire, elle est libérée (haut).

L’exemple suivant détermine si le bouton **Vitesse suivante** est à l’état appuyé.

```cpp
if (RacingWheelButtons::NextGear == (reading.Buttons & RacingWheelButtons::NextGear))
{
    // Next Gear is pressed
}
```

L’exemple suivant détermine si le bouton Vitesse suivante est à l’état relâché.

```cpp
if (RacingWheelButtons::None == (reading.Buttons & RacingWheelButtons::NextGear))
{
    // Next Gear is released (not pressed)
}
```

Parfois, vous souhaiterez peut-être déterminer quand un bouton passe de enfoncé à relâché ou relâché, si plusieurs boutons sont enfoncés ou relâché, ou si un ensemble de boutons est organisé d’une façon particulière &mdash; , d’autres non. Pour plus d’informations sur la détection de ces états, consultez [Détecter les changements d’état des boutons](input-practices-for-games.md#detecting-button-transitions) et [Détecter les dispositions de boutons complexes](input-practices-for-games.md#detecting-complex-button-arrangements).

### <a name="reading-the-wheel"></a>Lecture des données du volant

Le volant est un contrôle requis, qui fournit une valeur analogique située entre -1.0 et +1.0. Une valeur de -1.0 correspond à la position de volant la plus à gauche ; une valeur de +1.0 indique la position la plus à droite. La valeur du volant est lue à partir de la propriété `Wheel` de la structure [RacingWheelReading](/uwp/api/windows.gaming.input.racingwheelreading).

```cpp
float wheel = reading.Wheel;  // returns a value between -1.0 and +1.0.
```

Même si les valeurs du volant correspondent à différents degrés de rotation physique dans le volant réel, selon la plage de rotations prise en charge par le volant de course physique, il n’est généralement pas nécessaire de mettre les valeurs du volant à l’échelle ; les volants gérant des degrés de rotation plus étendus permettent simplement une plus grande précision.

### <a name="reading-the-throttle-and-brake"></a>Lecture des valeurs d’accélération et de freinage

La limitation et le frein sont des contrôles nécessaires qui fournissent chacun des lectures analogiques entre 0,0 (entièrement relâché) et 1,0 (entièrement activées) représentées sous forme de valeurs à virgule flottante. La valeur du contrôle d’accélération est lue à partir de la propriété `Throttle` de la structure [RacingWheelReading](/uwp/api/windows.gaming.input.racingwheelreading) ; la valeur du contrôle de freinage est lue à partir de la propriété `Brake`.

```cpp
float throttle = reading.Throttle;  // returns a value between 0.0 and 1.0
float brake    = reading.Brake;     // returns a value between 0.0 and 1.0
```

### <a name="reading-the-handbrake-and-clutch"></a>Lecture des valeurs de frein à main et d’embrayage

Le frein à main et l’embrayage sont des contrôles en option, qui fournissent des valeurs analogiques incluses entre 0.0 (élément entièrement relâché) et 1.0 (élément entièrement enclenché) représentées en tant que valeurs à virgule flottante. La valeur du contrôle de frein à main est lue à partir de la propriété `Handbrake` de la structure [RacingWheelReading](/uwp/api/windows.gaming.input.racingwheelreading) ; la valeur du contrôle d’embrayage est lue à partir de la propriété `Clutch`.

```cpp
float handbrake = 0.0;
float clutch = 0.0;

if(racingwheel->HasHandbrake)
{
    handbrake = reading.Handbrake;  // returns a value between 0.0 and 1.0
}

if(racingwheel->HasClutch)
{
    clutch = reading.Clutch;        // returns a value between 0.0 and 1.0
}
```

### <a name="reading-the-pattern-shifter"></a>Lecture de la valeur du levier de vitesses

Le levier de vitesses est un contrôle en option, qui fournit une valeur numérique incluse entre -1 et [MaxPatternShifterGear](/uwp/api/windows.gaming.input.racingwheel.maxpatternshiftergear), représentée en tant que valeur entière signée. La valeur-1 ou 0 correspond aux engrenages _inverses_ et _neutres_ , respectivement. les valeurs de plus en plus positives correspondent à des vitesses de transfert supérieures à **MaxPatternShifterGear**, incluses. La valeur du décalage de motif est lue à partir de la propriété [PatternShifterGear](/uwp/api/windows.gaming.input.racingwheelreading.patternshiftergear) de la structure [RacingWheelReading](/uwp/api/windows.gaming.input.racingwheelreading) .

```cpp
if (racingwheel->HasPatternShifter)
{
    gear = reading.PatternShifterGear;
}
```

> [!NOTE]
> Le changement de motif, le cas échéant, existe avec l' **engrenage précédent** requis et les boutons d' **engrenage suivant** qui affectent également l’engrenage actuel de la voiture du joueur. Une stratégie simple pour unifier ces entrées lorsque les deux sont présentes est d’ignorer le changement de motif (et l’embrayage) quand un joueur choisit une transmission automatique pour son véhicule, et d’ignorer les boutons d’engrenage **précédents** et **suivants** lorsqu’un joueur choisit une transmission manuelle pour leur voiture uniquement si leur roue de course est équipée d’un contrôle de changement de motif. Vous pouvez implémenter une autre stratégie d’unification si celle que nous avons décrite n'est pas adaptée à votre jeu.

## <a name="run-the-inputinterfacing-sample"></a>Exécuter l’exemple InputInterfacing

L’[exemple InputInterfacingUWP _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) indique comment utiliser les volants de course et différents types de périphériques d’entrée en parallèle, et décrit leur comportement de ces périphériques en tant que contrôleurs de navigation dans l’interface de navigation.

## <a name="force-feedback-overview"></a>Présentation du retour de force

De nombreuses roues de course ont une capacité de retour de force pour offrir une expérience de conduite plus immersive et plus complexe. Les volants de course prenant en charge le retour de force sont généralement dotés d’un seul moteur, qui applique une force au volant le long d’un axe unique, celui de la rotation de la roue. Les commentaires forcés sont pris en charge dans les applications Windows 10 et Xbox One UWP par l’espace de noms [Windows. Gaming. Input. ForceFeedback](/uwp/api/windows.gaming.input.forcefeedback) .

> [!NOTE]
> Les API de retour de force peuvent prendre en charge plusieurs axes de force, mais aucune roue de course Xbox One ne prend actuellement en charge les axes de commentaires autres que ceux de la rotation de la roue.

## <a name="using-force-feedback"></a>Utilisation du retour de force

Ces sections décrivent les bases de la programmation des effets de retour de force pour les volants de course Xbox One. Ce retour est appliqué via différents effets, qui sont d’abord chargés sur l’appareil devant appliquer ce retour. Les effets peuvent être démarrés, interrompus, repris et arrêtés de la même manière que les effets sonores, mais vous devez d’abord déterminer les fonctionnalités de retour du volant de course.

### <a name="determining-force-feedback-capabilities"></a>Identification des fonctionnalités de retour de force

Vous pouvez déterminer si un volant connecté gère le retour de force en lisant la propriété [WheelMotor](/uwp/api/windows.gaming.input.racingwheel.wheelmotor#Windows_Gaming_Input_RacingWheel_WheelMotor) associée au volant. Les commentaires de force ne sont pas pris en charge si `WheelMotor` est **null**; sinon, les commentaires forcés sont pris en charge et vous pouvez procéder à la détermination des fonctions de commentaires spécifiques du moteur, telles que les axes qu’il peut affecter.

```cpp
if (racingwheel->WheelMotor != nullptr)
{
    auto axes = racingwheel->WheelMotor->SupportedAxes;

    if(ForceFeedbackEffectAxes::X == (axes & ForceFeedbackEffectAxes::X))
    {
        // Force can be applied through the X axis
    }

    if(ForceFeedbackEffectAxes::Y == (axes & ForceFeedbackEffectAxes::Y))
    {
        // Force can be applied through the Y axis
    }

    if(ForceFeedbackEffectAxes::Z == (axes & ForceFeedbackEffectAxes::Z))
    {
        // Force can be applied through the Z axis
    }
}
```

### <a name="loading-force-feedback-effects"></a>Chargement des effets de retour de force

Ces effets sont chargés sur l’appareil devant les appliquer, puis « exécutés » de manière autonome lorsque le jeu le commande. Un certain nombre d’effets de base sont fournis. des effets personnalisés peuvent être créés par le biais d’une classe qui implémente l’interface [IForceFeedbackEffect](/uwp/api/windows.gaming.input.forcefeedback.iforcefeedbackeffect) .

| Classe d’effet         | Description de l’effet                                                                     |
| -------------------- | -------------------------------------------------------------------------------------- |
| ConditionForceEffect | Effet appliquant une force variable en réponse à un capteur dans l’appareil. |
| ConstantForceEffect  | Effet appliquant une force constante le long d’un vecteur.                                  |
| PeriodicForceEffect  | Effet appliquant une force variable définie par une forme d’onde, le long d’un vecteur.           |
| RampForceEffect      | Effet appliquant une force qui augmente ou baisse de manière linéaire, le long d’un vecteur.          |

```cpp
using FFLoadEffectResult = ForceFeedback::ForceFeedbackLoadEffectResult;

auto effect = ref new Windows.Gaming::Input::ForceFeedback::ConstantForceEffect();
auto time = TimeSpan(10000);

effect->SetParameters(Windows::Foundation::Numerics::float3(1.0f, 0.0f, 0.0f), time);

// Here, we assume 'racingwheel' is valid and supports force feedback

IAsyncOperation<FFLoadEffectResult>^ request
    = racingwheel->WheelMotor->LoadEffectAsync(effect);

auto loadEffectTask = Concurrency::create_task(request);

loadEffectTask.then([this](FFLoadEffectResult result)
{
    if (FFLoadEffectResult::Succeeded == result)
    {
        // effect successfully loaded
    }
    else
    {
        // effect failed to load
    }
}).wait();
```

### <a name="using-force-feedback-effects"></a>Utilisation des effets de retour de force

Une fois chargés, les effets peuvent être démarrés, interrompus, repris et arrêtés de manière synchrone, grâce à un appel à des fonctions sur la propriété `WheelMotor` du volant de course, ou de manière individuelle, grâce à un appel à des fonctions sur l’effet de retour lui-même. En général, il est recommandé de charger tous les effets que vous souhaitez utiliser sur l’appareil devant les exécuter avant que le jeu ne se lance, puis d’utiliser les fonctions `SetParameters` correspondantes pour mettre à jour les effets au fur et à mesure de la progression du jeu.

```cpp
if (ForceFeedbackEffectState::Running == effect->State)
{
    effect->Stop();
}
else
{
    effect->Start();
}
```

Pour finir, vous pouvez activer, désactiver ou réinitialiser l’ensemble du système de retour de force sur un volant de course spécifique, de manière asynchrone, quand vous le souhaitez.

## <a name="see-also"></a>Voir aussi

* [Windows.Gaming.Input.UINavigationController](/uwp/api/windows.gaming.input.uinavigationcontroller)
* [Windows.Gaming.Input.IGameController](/uwp/api/windows.gaming.input.igamecontroller)
* [Pratiques d’entrée pour les jeux](input-practices-for-games.md)

[Windows.Gaming.Input]: /uwp/api/Windows.Gaming.Input
[Windows.Gaming.Input.UINavigationController]: /uwp/api/Windows.Gaming.Input.UINavigationController
[Windows.Gaming.Input.IGameController]: /uwp/api/Windows.Gaming.Input.IGameController
[racingwheel]: /uwp/api/Windows.Gaming.Input.RacingWheel
[racingwheels]: /uwp/api/Windows.Gaming.Input.RacingWheel
[racingwheeladded]: /uwp/api/Windows.Gaming.Input.RacingWheel
[racingwheelremoved]: /uwp/api/Windows.Gaming.Input.RacingWheel
[haspatternshifter]: /uwp/api/Windows.Gaming.Input.RacingWheel
[hashandbrake]: /uwp/api/Windows.Gaming.Input.RacingWheel
[hasclutch]: /uwp/api/Windows.Gaming.Input.RacingWheel
[maxpatternshiftergear]: /uwp/api/Windows.Gaming.Input.RacingWheel
[maxwheelangle]: /uwp/api/Windows.Gaming.Input.RacingWheel
[getcurrentreading]: /uwp/api/Windows.Gaming.Input.RacingWheel
[wheelmotor]: /uwp/api/Windows.Gaming.Input.RacingWheel
[racingwheelreading]: /uwp/api/Windows.Gaming.Input.RacingWheelReading
[racingwheelbuttons]: /uwp/api/Windows.Gaming.Input.RacingWheelButtons