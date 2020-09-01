---
title: Ajout d’entrées et d’interactivité à l’exemple Marble Maze
description: En savoir plus sur les principales pratiques à prendre en compte lorsque vous travaillez avec des appareils d’entrée.
ms.assetid: b946bf62-c0ca-f9ec-1a87-8195b89a5ab4
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, jeux, entrée, exemple
ms.localizationpriority: medium
ms.openlocfilehash: d4c3742ed843deca9d7d8edba033addd2e4888fe
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172073"
---
# <a name="adding-input-and-interactivity-to-the-marble-maze-sample"></a>Ajout d’entrées et d’interactivité à l’exemple Marble Maze




Les jeux plateforme Windows universelle (UWP) s’exécutent sur un large éventail d’appareils, tels que les ordinateurs de bureau, les ordinateurs portables et les tablettes. Un appareil peut avoir une multitude de mécanismes d’entrée et de contrôle. Ce document décrit les pratiques clés à prendre en compte quand vous utilisez des périphériques d’entrée et comment appliquer ces pratiques à l’exemple Marble Maze.

> [!NOTE]
> L’exemple de code qui correspond à ce document se trouve dans l' [exemple de jeu de labyrinthe DirectX Marble](https://github.com/microsoft/Windows-appsample-marble-maze).

 
Voici quelques éléments clés présentés dans ce document que vous devez prendre en compte quand vous travaillez avec les entrées dans votre jeu :

-   Quand cela est possible, il est important de prendre en charge différents périphériques d’entrée afin que votre jeu puisse fonctionner avec un plus grand nombre de préférences et fonctionnalités choisies par vos utilisateurs. Bien que l’utilisation d’un contrôleur de jeu et d’un capteur soit facultative, nous recommandons cette utilisation pour améliorer l’expérience des joueurs. Nous avons conçu le contrôleur de jeu et les API de capteur pour vous aider à intégrer plus facilement ces périphériques d’entrée.

-   Pour initialiser les entrées tactiles, vous devez enregistrer des événements de fenêtre, par exemple quand le pointeur est activé, libéré et déplacé. Pour initialiser l’accéléromètre, créez un objet [Windows::Devices::Sensors::Accelerometer](/uwp/api/Windows.Devices.Sensors.Accelerometer) quand vous initialisez l’application. Le contrôleur Xbox n’a pas besoin d’être initialisé.

-   Pour les jeux à un seul joueur, déterminez s’il faut combiner l’entrée à partir de tous les contrôleurs Xbox possibles. De cette façon, vous n’avez pas besoin d’effectuer un suivi pour savoir de quel contrôleur provient l’entrée. Ou, suivez simplement l’entrée du contrôleur le plus récemment ajouté, comme nous le faisons dans cet exemple.

-   Traitez les événements de fenêtre avant de traiter les périphériques d’entrée.

-   Le contrôleur Xbox et l’accéléromètre prennent en charge l’interrogation. C’est-à-dire que vous pouvez demander les données dont vous avez besoin. Pour les entrées tactiles, enregistrez les événements tactiles dans des structures de données disponibles à votre code de traitement des entrées.

-   Envisagez de normaliser les valeurs d’entrée pour utiliser un format unique. Cela permet de simplifier l’interprétation des entrées par d’autres composants de votre jeu, par exemple des simulations physiques, et de faciliter l’écriture de jeux qui fonctionnent sur différentes résolutions d’écran.

## <a name="input-devices-supported-by-marble-maze"></a>Périphériques d’entrée pris en charge par Marble Maze


Le labyrinthe de marbre prend en charge le contrôleur Xbox, la souris et le toucher pour sélectionner les éléments de menu, et le contrôleur Xbox, la souris, le toucher et l’accéléromètre pour contrôler la lecture du jeu. Le labyrinthe de marbre utilise les API [Windows :: Gaming :: Input](/uwp/api/windows.gaming.input) pour interroger le contrôleur en vue d’une entrée. Les fonctions tactiles permettent aux applications de suivre et de répondre aux entrées tactiles. Un accéléromètre est un capteur qui mesure la force appliquée le long des axes X, Y et Z. En utilisant Windows Runtime, vous pouvez interroger l’état actuel de l’accéléromètre, et recevoir des événements tactiles via le mécanisme de gestion des événements du Windows Runtime.

> [!NOTE]
> Ce document utilise Touch pour faire référence à l’entrée tactile et au pointeur pour faire référence à n’importe quel appareil qui utilise des événements de pointeur. Dans la mesure où l’interaction tactile et la souris utilisent des événements de pointeur standards, vous pouvez utiliser l’un ou l’autre périphérique pour sélectionner les éléments de menu et contrôler le jeu.

 

> [!NOTE]
> Le manifeste du package définit le **paysage** comme étant la seule rotation prise en charge pour le jeu pour empêcher la modification de l’orientation lorsque vous faites pivoter l’appareil pour faire tourner le marbre. Pour afficher le manifeste du package, ouvrez **Package. appxmanifest** dans le **Explorateur de solutions** dans Visual Studio.

 

## <a name="initializing-input-devices"></a>Initialisation des périphériques d’entrée


Le contrôleur Xbox ne requiert pas d’initialisation. Pour initialiser Touch, vous devez vous inscrire pour les événements de fenêtrage comme lorsque le pointeur est activé (par exemple, le joueur appuie sur le bouton de la souris ou touche l’écran), relâché et déplacé. Pour initialiser l’accéléromètre, vous devez créer un objet [Windows::Devices::Sensors::Accelerometer](/uwp/api/Windows.Devices.Sensors.Accelerometer) quand vous initialisez l’application.

L’exemple suivant montre comment la méthode **App :: SetWindow** s’inscrit aux événements de pointeur [Windows :: UI :: Core :: CoreWindow ::P ointerpressed](/uwp/api/windows.ui.core.corewindow.PointerPressed), [Windows :: UI :: core :: CoreWindow ::P OINTERRELEASED](/uwp/api/windows.ui.core.corewindow.PointerReleased)et [Windows :: UI :: Core :: CoreWindow ::P ointermoved](/uwp/api/windows.ui.core.corewindow.PointerMoved) . Ces événements sont inscrits pendant l’initialisation de l’application et avant la boucle du jeu.

Ils sont gérés dans un thread distinct chargé d’appeler les gestionnaires d’événements.

Pour plus d’informations sur l’initialisation de l’application, voir [Structure de l’application Marble Maze](marble-maze-application-structure.md).

```cpp
window->PointerPressed += ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(
    this, 
    &App::OnPointerPressed);

window->PointerReleased += ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(
    this, 
    &App::OnPointerReleased);

window->PointerMoved += ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(
    this, 
    &App::OnPointerMoved);
```

La classe **MarbleMazeMain** crée également un objet **std :: Map** pour contenir des événements tactiles. La clé de cet objet map est une valeur qui identifie de façon unique le pointeur d’entrée. Chaque clé mappe la distance entre chaque point tactile et le centre de l’écran. Marble Maze utilise ensuite ces valeurs pour calculer l’inclinaison du labyrinthe.

```cpp
typedef std::map<int, XMFLOAT2> TouchMap;
TouchMap        m_touches;
```

La classe **MarbleMazeMain** contient également un objet [accéléromètre](/uwp/api/Windows.Devices.Sensors.Accelerometer) .

```cpp
Windows::Devices::Sensors::Accelerometer^           m_accelerometer;
```

L’objet **accéléromètre** est initialisé dans le constructeur **MarbleMazeMain** , comme indiqué dans l’exemple suivant. La méthode [Windows ::D evices :: sensors :: accéléromètre :: GetDefault](/uwp/api/Windows.Devices.Sensors.Accelerometer.GetDefault) retourne une instance de l’accéléromètre par défaut. S’il n’existe aucun accéléromètre par défaut, **accéléromètre :: GetDefault** retourne **nullptr**.

```cpp
// Returns accelerometer ref if there is one; nullptr otherwise.
m_accelerometer = Windows::Devices::Sensors::Accelerometer::GetDefault();
```

##  <a name="navigating-the-menus"></a>Navigation dans les menus

Vous pouvez utiliser la souris, le toucher ou le contrôleur Xbox pour naviguer dans les menus, comme suit :

-   Utilisez le pavé directionnel pour changer l’élément de menu actif.
-   Utilisez Touch, le bouton A ou le bouton de menu pour sélectionner un élément de menu ou fermer le menu actuel, tel que le tableau des scores élevés.
-   Utilisez le bouton de menu pour suspendre ou reprendre le jeu.
-   Cliquez sur un élément de menu avec la souris pour choisir cette action.

###  <a name="tracking-xbox-controller-input"></a>Suivi de l’entrée du contrôleur Xbox

Pour effectuer le suivi des boîtiers de connexion actuellement connectés à l’appareil, **MarbleMazeMain** définit une variable membre, **m_myGamepads**, qui est une collection d’objets [Windows :: Gaming :: Input :: manette](/uwp/api/windows.gaming.input.gamepad) . Cela est initialisé dans le constructeur comme suit :

```cpp
m_myGamepads = ref new Vector<Gamepad^>();

for (auto gamepad : Gamepad::Gamepads)
{
    m_myGamepads->Append(gamepad);
}
```

En outre, le constructeur **MarbleMazeMain** inscrit des événements pour l’ajout ou la suppression de boîtiers :

```cpp
Gamepad::GamepadAdded += 
    ref new EventHandler<Gamepad^>([=](Platform::Object^, Gamepad^ args)
{
    m_myGamepads->Append(args);
    m_currentGamepadNeedsRefresh = true;
});

Gamepad::GamepadRemoved += 
    ref new EventHandler<Gamepad ^>([=](Platform::Object^, Gamepad^ args)
{
    unsigned int indexRemoved;

    if (m_myGamepads->IndexOf(args, &indexRemoved))
    {
        m_myGamepads->RemoveAt(indexRemoved);
        m_currentGamepadNeedsRefresh = true;
    }
});
```

Lorsqu’un boîtier de l’ajout est ajouté, il est ajouté à **m_myGamepads**; Quand un boîtier de l’option est supprimé, nous vérifions si le boîtier de l’installation est en **m_myGamepads**, et si c’est le cas, nous le supprimons. Dans les deux cas, nous définissons **m_currentGamepadNeedsRefresh** sur **true**, ce qui indique que nous devons réassigner **m_gamepad**.

Enfin, nous attribuons un manette de jeu à **m_gamepad** et définissons **m_currentGamepadNeedsRefresh** sur **false**:

```cpp
m_gamepad = GetLastGamepad();
m_currentGamepadNeedsRefresh = false;
```

Dans la méthode **Update** , nous vérifions si **m_gamepad** doit être réaffecté :

```cpp
if (m_currentGamepadNeedsRefresh)
{
    auto mostRecentGamepad = GetLastGamepad();

    if (m_gamepad != mostRecentGamepad)
    {
        m_gamepad = mostRecentGamepad;
    }

    m_currentGamepadNeedsRefresh = false;
}
```

Si **m_gamepad** doit être réaffecté, nous lui attribuons le boîtier de la dernière fois ajouté, à l’aide de **GetLastGamepad**, qui est défini comme suit :

```cpp
Gamepad^ MarbleMaze::MarbleMazeMain::GetLastGamepad()
{
    Gamepad^ gamepad = nullptr;

    if (m_myGamepads->Size > 0)
    {
        gamepad = m_myGamepads->GetAt(m_myGamepads->Size - 1);
    }

    return gamepad;
}
```

Cette méthode retourne simplement le dernier boîtier de **m_myGamepads**.

Vous pouvez connecter jusqu’à quatre contrôleurs Xbox à un appareil Windows 10. Pour éviter d’avoir à déterminer quel contrôleur est le contrôleur actif, nous n’avons simplement suivi que le dernier boîtier ajouté. Si votre jeu prend en charge plusieurs joueurs, vous devez suivre les entrées de chaque joueur séparément.

La méthode **MarbleMazeMain :: Update** interroge le boîtier de l’entrée :

```cpp
if (m_gamepad != nullptr)
{
    m_oldReading = m_newReading;
    m_newReading = m_gamepad->GetCurrentReading();
}
```

Nous effectuons le suivi de la lecture des entrées que nous avons rencontrées dans le dernier frame avec **m_oldReading**, et les dernières entrées de lecture avec **m_newReading**, que nous obtenons en appelant le contrôle de [boîtier :: GetCurrentReading](/uwp/api/windows.gaming.input.gamepad.GetCurrentReading). Cela retourne un objet [GamepadReading](/uwp/api/windows.gaming.input.gamepadreading) , qui contient des informations sur l’état actuel du boîtier de l’exécution de la manette.

Pour vérifier si un bouton vient d’être appuyé ou relâché, nous définissons **MarbleMazeMain :: ButtonJustPressed** et **MarbleMazeMain :: ButtonJustReleased**, qui comparent les lectures de bouton à partir de ce frame et de la dernière trame. De cette façon, nous pouvons effectuer une action uniquement au moment où un bouton est initialement enfoncé ou relâché, et non lorsqu’il est maintenu :

```cpp
bool MarbleMaze::MarbleMazeMain::ButtonJustPressed(GamepadButtons selection)
{
    bool newSelectionPressed = (selection == (m_newReading.Buttons & selection));
    bool oldSelectionPressed = (selection == (m_oldReading.Buttons & selection));
    return newSelectionPressed && !oldSelectionPressed;
}

bool MarbleMaze::MarbleMazeMain::ButtonJustReleased(GamepadButtons selection)
{
    bool newSelectionReleased = 
        (GamepadButtons::None == (m_newReading.Buttons & selection));

    bool oldSelectionReleased = 
        (GamepadButtons::None == (m_oldReading.Buttons & selection));

    return newSelectionReleased && !oldSelectionReleased;
}
```

Les lectures [GamepadButtons](/uwp/api/windows.gaming.input.gamepadbuttons) sont comparées à l’aide d’opérations au niveau du bit &mdash; . nous vérifions si un bouton est activé à l’aide du *bit and* (&). Nous déterminons si un bouton vient d’être appuyé ou relâché en comparant l’ancienne lecture et la nouvelle lecture.

À l’aide des méthodes ci-dessus, nous vérifions si certains boutons ont été activés et effectuons les actions correspondantes qui doivent se produire. Par exemple, quand vous appuyez sur le bouton de menu (**GamepadButtons :: menu**), l’état du jeu passe de actif à suspendu ou suspendu à actif.

```cpp
if (ButtonJustPressed(GamepadButtons::Menu) || m_pauseKeyPressed)
{
    m_pauseKeyPressed = false;

    if (m_gameState == GameState::InGameActive)
    {
        SetGameState(GameState::InGamePaused);
    }  
    else if (m_gameState == GameState::InGamePaused)
    {
        SetGameState(GameState::InGameActive);
    }
}
```

Nous vérifions également si le joueur appuie sur le bouton d’affichage. dans ce cas, nous redémarrons le jeu ou effacons le tableau des scores élevés :

```cpp
if (ButtonJustPressed(GamepadButtons::View) || m_homeKeyPressed)
{
    m_homeKeyPressed = false;

    if (m_gameState == GameState::InGameActive ||
        m_gameState == GameState::InGamePaused ||
        m_gameState == GameState::PreGameCountdown)
    {
        SetGameState(GameState::MainMenu);
        m_inGameStopwatchTimer.SetVisible(false);
        m_preGameCountdownTimer.SetVisible(false);
    }
    else if (m_gameState == GameState::HighScoreDisplay)
    {
        m_highScoreTable.Reset();
    }
}
```

Si le menu principal est actif, l’élément de menu actif change lors d’un appui sur le pavé directionnel. Si l’utilisateur choisit la sélection actuelle, l’élément de l’interface utilisateur approprié est marqué comme ayant été choisi.

```cpp
// Handle menu navigation.
bool chooseSelection = 
    (ButtonJustPressed(GamepadButtons::A) 
    || ButtonJustPressed(GamepadButtons::Menu));

bool moveUp = ButtonJustPressed(GamepadButtons::DPadUp);
bool moveDown = ButtonJustPressed(GamepadButtons::DPadDown);

switch (m_gameState)
{
case GameState::MainMenu:
    if (chooseSelection)
    {
        m_audio.PlaySoundEffect(MenuSelectedEvent);
        if (m_startGameButton.GetSelected())
        {
            m_startGameButton.SetPressed(true);
        }
        if (m_highScoreButton.GetSelected())
        {
            m_highScoreButton.SetPressed(true);
        }
    }
    if (moveUp || moveDown)
    {
        m_startGameButton.SetSelected(!m_startGameButton.GetSelected());
        m_highScoreButton.SetSelected(!m_startGameButton.GetSelected());
        m_audio.PlaySoundEffect(MenuChangeEvent);
    }
    break;

case GameState::HighScoreDisplay:
    if (chooseSelection || anyPoints)
    {
        SetGameState(GameState::MainMenu);
    }
    break;

case GameState::PostGameResults:
    if (chooseSelection || anyPoints)
    {
        SetGameState(GameState::HighScoreDisplay);
    }
    break;

case GameState::InGamePaused:
    if (m_pausedText.IsPressed())
    {
        m_pausedText.SetPressed(false);
        SetGameState(GameState::InGameActive);
    }
    break;
}
```

### <a name="tracking-touch-and-mouse-input"></a>Suivi des entrées tactiles et de la souris

Pour les entrées tactiles et de la souris, un élément de menu est choisi quand l’utilisateur le touche ou clique dessus. L’exemple suivant montre comment la méthode **MarbleMazeMain :: Update** traite l’entrée de pointeur pour sélectionner les éléments de menu. La variable membre **m \_ pointQueue** effectue le suivi des emplacements où l’utilisateur a touché ou cliqué sur l’écran. La façon dont le labyrinthe de marbre collecte les entrées de pointeur est décrite plus en détail plus loin dans ce document, dans la section [traitement des entrées de pointeur](#processing-pointer-input).

```cpp
// Check whether the user chose a button from the UI. 
bool anyPoints = !m_pointQueue.empty();
while (!m_pointQueue.empty())
{
    UserInterface::GetInstance().HitTest(m_pointQueue.front());
    m_pointQueue.pop();
}
```

La méthode **UserInterface::HitTest** détermine si le point fourni est situé dans les limites d’un élément d’interface. Les éléments d’interface qui réussissent ce test sont marqués comme ayant été touchés. Cette méthode utilise la fonction d’assistance **PointInRect** pour déterminer si le point fourni se trouve dans les limites de chaque élément d’interface utilisateur.

```cpp
void UserInterface::HitTest(D2D1_POINT_2F point)
{
    for (auto iter = m_elements.begin(); iter != m_elements.end(); ++iter)
    {
        if (!(*iter)->IsVisible())
            continue;

        TextButton* textButton = dynamic_cast<TextButton*>(*iter);
        if (textButton != nullptr)
        {
            D2D1_RECT_F bounds = (*iter)->GetBounds();
            textButton->SetPressed(PointInRect(point, bounds));
        }
    }
}
```

### <a name="updating-the-game-state"></a>Mise à jour de l’état du jeu

Une fois que la méthode **MarbleMazeMain :: Update** traite le contrôleur et l’entrée tactile, elle met à jour l’état du jeu si un bouton a été enfoncé.

```cpp
// Update the game state if the user chose a menu option. 
if (m_startGameButton.IsPressed())
{
    SetGameState(GameState::PreGameCountdown);
    m_startGameButton.SetPressed(false);
}
if (m_highScoreButton.IsPressed())
{
    SetGameState(GameState::HighScoreDisplay);
    m_highScoreButton.SetPressed(false);
}
```

##  <a name="controlling-game-play"></a>Contrôle du jeu


La boucle de jeu et la méthode **MarbleMazeMain :: Update** fonctionnent ensemble pour mettre à jour l’état des objets de jeu. Si votre jeu accepte des entrées de plusieurs périphériques, vous pouvez cumuler les entrées de tous ces périphériques dans un ensemble unique de variables, ce qui vous permet d’écrire du code plus facile à mettre à jour. La méthode **MarbleMazeMain :: Update** définit un ensemble de variables qui accumulent le déplacement de tous les appareils.

```cpp
float combinedTiltX = 0.0f;
float combinedTiltY = 0.0f;
```

Le mécanisme d’entrée n’est pas toujours le même d’un périphérique à l’autre. Par exemple, l’entrée de pointeur est gérée en utilisant le modèle de gestion des événements du Windows Runtime. Inversement, vous interrogez les données d’entrée à partir du contrôleur Xbox lorsque vous en avez besoin. Nous recommandons de toujours suivre le mécanisme d’entrée conseillé pour un périphérique donné. Cette section décrit comment Marble Maze lit les entrées de chaque périphérique, comment il met à jour les valeurs d’entrée combinées et comment il utilise ces valeurs pour mettre à jour l’état du jeu.

###  <a name="processing-pointer-input"></a>Traitement des entrées de pointeur

Quand vous utilisez des entrées de pointeur, appelez la méthode [Windows::UI::Core::CoreDispatcher::ProcessEvents](/uwp/api/windows.ui.core.coredispatcher.processevents) pour traiter les événements de fenêtre. Appelez cette méthode dans la boucle de votre jeu avant de mettre à jour ou de générer le rendu de la scène. Le labyrinthe de marbre appelle This dans la méthode **App :: Run** : 

```cpp
while (!m_windowClosed)
{
    if (m_windowVisible)
    {
        CoreWindow::GetForCurrentThread()->
            Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

        m_main->Update();

        if (m_main->Render())
        {
            m_deviceResources->Present();
        }
    }
    else
    {
        CoreWindow::GetForCurrentThread()->
            Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
    }
}
```

Si la fenêtre est visible, nous transmettons **CoreProcessEventsOption ::P rocessallifpresent** à **ProcessEvents** pour traiter tous les événements mis en file d’attente et retourner immédiatement. dans le cas contraire, nous passons **CoreProcessEventsOption ::P rocessoneandallpending** pour traiter tous les événements mis en file d’attente et attendre le nouvel événement suivant. Une fois tous les événements traités, Marble Maze génère un rendu de l’image suivante et la dévoile.

Windows Runtime appelle le gestionnaire enregistré pour chaque événement qui se produit. La méthode **App :: SetWindow** s’inscrit pour les événements et transfère les informations de pointeur à la classe **MarbleMazeMain** .

```cpp
void App::OnPointerPressed(
    Windows::UI::Core::CoreWindow^ sender, 
    Windows::UI::Core::PointerEventArgs^ args)
{
    m_main->AddTouch(args->CurrentPoint->PointerId, args->CurrentPoint->Position);
}

void App::OnPointerReleased(
    Windows::UI::Core::CoreWindow^ sender, 
    Windows::UI::Core::PointerEventArgs^ args)
{
    m_main->RemoveTouch(args->CurrentPoint->PointerId);
}

void App::OnPointerMoved(
    Windows::UI::Core::CoreWindow^ sender, 
    Windows::UI::Core::PointerEventArgs^ args)
{
    m_main->UpdateTouch(args->CurrentPoint->PointerId, args->CurrentPoint->Position);
}
```

La classe **MarbleMazeMain** réagit aux événements pointeur en mettant à jour l’objet Map qui contient les événements tactiles. La méthode **MarbleMazeMain :: AddTouch** est appelée quand le pointeur est appuyé pour la première fois, par exemple, lorsque l’utilisateur touche initialement l’écran sur un appareil tactile. La méthode **MarbleMazeMain :: UpdateTouch** est appelée lorsque la position du pointeur est déplacée. La méthode **MarbleMazeMain :: RemoveTouch** est appelée quand le pointeur est relâché, par exemple lorsque l’utilisateur cesse de toucher l’écran.

```cpp
void MarbleMazeMain::AddTouch(int id, Windows::Foundation::Point point)
{
    m_touches[id] = PointToTouch(point, m_deviceResources->GetLogicalSize());

    m_pointQueue.push(D2D1::Point2F(point.X, point.Y));
}

void MarbleMazeMain::UpdateTouch(int id, Windows::Foundation::Point point)
{
    if (m_touches.find(id) != m_touches.end())
        m_touches[id] = PointToTouch(point, m_deviceResources->GetLogicalSize());
}

void MarbleMazeMain::RemoveTouch(int id)
{
    m_touches.erase(id);
}
```

La fonction **PointToTouch** convertit la position actuelle du pointeur afin que l’origine se trouve au centre de l’écran, puis adapte les coordonnées afin qu’elles soient comprises entre-1,0 et + 1,0. Cela permet de calculer l’inclinaison du labyrinthe, quelle que soit la méthode d’entrée.

```cpp
inline XMFLOAT2 PointToTouch(Windows::Foundation::Point point, Windows::Foundation::Size bounds)
{
    float touchRadius = min(bounds.Width, bounds.Height);
    float dx = (point.X - (bounds.Width / 2.0f)) / touchRadius;
    float dy = ((bounds.Height / 2.0f) - point.Y) / touchRadius;

    return XMFLOAT2(dx, dy);
}
```

La méthode **MarbleMazeMain :: Update** met à jour les valeurs d’entrée combinées en incrémentant le facteur d’inclinaison à l’aide d’une valeur de mise à l’échelle constante. Cette valeur de mise à l’échelle a été identifiée en testant différentes valeurs.

```cpp
// Account for touch input.
for (TouchMap::const_iterator iter = m_touches.cbegin(); 
    iter != m_touches.cend(); 
    ++iter)
{
    combinedTiltX += iter->second.x * m_touchScaleFactor;
    combinedTiltY += iter->second.y * m_touchScaleFactor;
}
```

### <a name="processing-accelerometer-input"></a>Traitement des entrées d’accéléromètre

Pour traiter l’entrée de l’accéléromètre, la méthode **MarbleMazeMain :: Update** appelle la méthode [Windows ::D evices :: sensors :: accéléromètre :: GetCurrentReading](/uwp/api/windows.devices.sensors.accelerometer.getcurrentreading) . Cette méthode renvoie un objet [Windows::Devices::Sensors::AccelerometerReading](/uwp/api/Windows.Devices.Sensors.AccelerometerReading), qui représente une lecture de l’accéléromètre. Les propriétés **Windows ::D evices :: sensors :: AccelerometerReading :: AccelerationX** et **Windows ::D evices :: sensors :: AccelerometerReading :: Acceleration** sont respectivement associées à l’accélération g-force sur les axes X et Y.

L’exemple suivant montre comment la méthode **MarbleMazeMain :: Update** interroge l’accéléromètre et met à jour les valeurs d’entrée combinées. Quand vous inclinez l’appareil, la bille va plus vite en raison de la gravité.

```cpp
// Account for sensors.
if (m_accelerometer != nullptr)
{
    Windows::Devices::Sensors::AccelerometerReading^ reading =
        m_accelerometer->GetCurrentReading();

    if (reading != nullptr)
    {
        combinedTiltX += 
            static_cast<float>(reading->AccelerationX) * m_accelerometerScaleFactor;

        combinedTiltY += 
            static_cast<float>(reading->AccelerationY) * m_accelerometerScaleFactor;
    }
}
```

Étant donné que vous ne pouvez pas être sûr qu’un accéléromètre est présent sur l’ordinateur de l’utilisateur, vérifiez toujours que vous disposez d’un objet [accéléromètre](/uwp/api/Windows.Devices.Sensors.Accelerometer) valide avant d’interroger l’accéléromètre.

### <a name="processing-xbox-controller-input"></a>Traitement de l’entrée du contrôleur Xbox

Dans la méthode **MarbleMazeMain :: Update** , nous utilisons **m_newReading** pour traiter l’entrée à partir du stick analogique gauche :

```cpp
float leftStickX = static_cast<float>(m_newReading.LeftThumbstickX);
float leftStickY = static_cast<float>(m_newReading.LeftThumbstickY);

auto oppositeSquared = leftStickY * leftStickY;
auto adjacentSquared = leftStickX * leftStickX;

if ((oppositeSquared + adjacentSquared) > m_deadzoneSquared)
{
    combinedTiltX += leftStickX * m_controllerScaleFactor;
    combinedTiltY += leftStickY * m_controllerScaleFactor;
}
```

Nous vérifions si l’entrée du stick analogique gauche est en dehors de la zone morte, et si c’est le cas, nous l’ajoutons à **combinedTiltX** et **combinedTiltY** (multiplié par un facteur d’échelle) pour incliner l’étape.

> [!IMPORTANT]
> Quand vous travaillez avec le contrôleur Xbox, vous devez toujours tenir compte de la zone morte. La zone morte fait référence aux différences de sensibilité au mouvement initial des boîtiers de commande. Pour certaines manettes, un mouvement léger peut ne pas générer de lecture, pour d’autres, une lecture mesurable peut être obtenue. Pour tenir compte de cela dans votre jeu, créez une zone de non-mouvement pour le mouvement initial du stick. Pour plus d’informations sur la zone morte, consultez [lecture du Thumbsticks](gamepad-and-vibration.md#reading-the-thumbsticks).

 

###  <a name="applying-input-to-the-game-state"></a>Application des entrées à l’état du jeu

Les périphériques indiquent les valeurs d’entrée de différentes manières. Par exemple, les entrées du pointeur peuvent être des coordonnées d’écran, tandis que les entrées de manette peuvent être dans un format complètement différent. L’un des problèmes liés à la combinaison des entrées de plusieurs périphériques en un ensemble de valeurs d’entrée est la normalisation, c’est-à-dire la conversion de valeurs dans un format commun. Le labyrinthe de marbre normalise les valeurs en les mettant à l’échelle jusqu’à la plage \[ -1,0, 1,0 \] . La fonction **PointToTouch** , qui est décrite précédemment dans cette section, convertit les coordonnées d’écran en valeurs normalisées qui sont approximativement comprises entre-1,0 et + 1,0.

> [!TIP]
> Même si votre application utilise une méthode d’entrée, nous vous recommandons de toujours normaliser les valeurs d’entrée. Cela permet de simplifier l’interprétation des entrées par d’autres composants de votre jeu, par exemple des simulations physiques, et de faciliter l’écriture de jeux qui fonctionnent sur différentes résolutions d’écran.

 

Une fois que la méthode **MarbleMazeMain :: Update** traite l’entrée, elle crée un vecteur qui représente l’effet de l’inclinaison du labyrinthe sur le marbre. L’exemple suivant montre comment Marble Maze utilise la fonction [XMVector3Normalize](/windows/desktop/api/directxmath/nf-directxmath-xmvector3normalize) pour créer un vecteur de gravité normalisé. La variable **maxTilt** limite la quantité d’inclinaison du labyrinthe et empêche le labyrinthe de pivoter de son côté.

```cpp
const float maxTilt = 1.0f / 8.0f;

XMVECTOR gravity = XMVectorSet(
    combinedTiltX * maxTilt, 
    combinedTiltY * maxTilt, 
    1.0f, 
    0.0f);

gravity = XMVector3Normalize(gravity);
```

Pour terminer la mise à jour des objets de la scène, Marble Maze transmet le vecteur de gravité mis à jour à la simulation physique, met à jour la simulation physique pour le temps qui s’est écoulé depuis l’image précédente, puis met à jour la position et l’orientation de la bille. Si le marbre est tombé dans le labyrinthe, la méthode **MarbleMazeMain :: Update** place le marbre au dernier point de contrôle que le marbre a touché et réinitialise l’état de la simulation physique.

```cpp
XMFLOAT3A g;
XMStoreFloat3(&g, gravity);
m_physics.SetGravity(g);

if (m_gameState == GameState::InGameActive)
{
    // Only update physics when gameplay is active.
    m_physics.UpdatePhysicsSimulation(static_cast<float>(m_timer.GetElapsedSeconds()));

    // ...Code omitted for simplicity...

}

// ...Code omitted for simplicity...

// Check whether the marble fell off of the maze. 
const float fadeOutDepth = 0.0f;
const float resetDepth = 80.0f;
if (marblePosition.z >= fadeOutDepth)
{
    m_targetLightStrength = 0.0f;
}
if (marblePosition.z >= resetDepth)
{
    // Reset marble.
    memcpy(&marblePosition, &m_checkpoints[m_currentCheckpoint], sizeof(XMFLOAT3));
    oldMarblePosition = marblePosition;
    m_physics.SetPosition((const XMFLOAT3&)marblePosition);
    m_physics.SetVelocity(XMFLOAT3(0, 0, 0));
    m_lightStrength = 0.0f;
    m_targetLightStrength = 1.0f;

    m_resetCamera = true;
    m_resetMarbleRotation = true;
    m_audio.PlaySoundEffect(FallingEvent);
}
```

Cette section ne décrit pas le fonctionnement de la simulation physique. Pour plus d’informations à ce sujet, consultez **physique. h** et **physique. cpp** dans les sources du labyrinthe de marbre.

## <a name="next-steps"></a>Étapes suivantes


Consultez [Ajout d’audio à l’exemple Marble Maze](adding-audio-to-the-marble-maze-sample.md) pour obtenir des informations sur les pratiques clés en matière d’audio. Ce document décrit comment Marble Maze utilise Microsoft Media Foundation et XAudio2 pour charger, mixer et lire des ressources audio.

## <a name="related-topics"></a>Rubriques connexes


* [Ajout d’audio à l’exemple Marble Maze](adding-audio-to-the-marble-maze-sample.md)
* [Ajout de contenu visuel à l’exemple Marble Maze](adding-visual-content-to-the-marble-maze-sample.md)
* [Développement de Marble Maze, jeu pour UW en C++ et DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 