---
title: Définir l’infrastructure d’application UWP du jeu
description: La première étape du codage d’un jeu de plateforme Windows universelle (UWP) est la création de l’infrastructure qui permet à l’objet d’application d’interagir avec Windows.
ms.assetid: 7beac1eb-ba3d-e15c-44a1-da2f5a79bb3b
ms.date: 06/24/2020
ms.topic: article
keywords: windows 10, uwp, jeux, directx
ms.localizationpriority: medium
ms.openlocfilehash: 2d762aebeaa5c97c1b23a91f0765cb5c09a91b46
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163053"
---
#  <a name="define-the-games-uwp-app-framework"></a>Définir l’infrastructure d’application UWP du jeu

> [!NOTE]
> Cette rubrique fait partie de la série de didacticiels [créer un jeu de plateforme Windows universelle simple (UWP) avec DirectX](tutorial--create-your-first-uwp-directx-game.md) . La rubrique de ce lien définit le contexte de la série.

La première étape du codage d’un jeu de plateforme Windows universelle (UWP) est la création de l’infrastructure qui permet à l’objet d’application d’interagir avec Windows, y compris les fonctionnalités Windows Runtime telles que la gestion des événements de suspension et de reprise, les modifications du focus de la fenêtre et l’alignement.

## <a name="objectives"></a>Objectifs

* Configurez le Framework pour un jeu DirectX plateforme Windows universelle (UWP) et implémentez la machine à États qui définit l’ensemble du workflow de jeu.

> [!NOTE]
> Pour suivre cette rubrique, consultez le code source de l’exemple de jeu [Simple3DGameDX](/samples/microsoft/windows-universal-samples/simple3dgamedx/) que vous avez téléchargé.

## <a name="introduction"></a>Introduction

Dans la rubrique [configurer le projet de jeu](tutorial--setting-up-the-games-infrastructure.md) , nous avons introduit la fonction **wWinMain** , ainsi que les interfaces [**IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) et [**IFrameworkView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) . Nous avons appris que la classe **app** (que vous pouvez voir définie dans le `App.cpp` fichier de code source dans le projet **Simple3DGameDX** ) sert à la fois de *fabrique de fournisseurs* et de *fournisseur*d’affichage.

Cette rubrique reprend à partir de là et décrit plus en détail la façon dont la classe **app** dans un jeu doit implémenter les méthodes de **IFrameworkView**.

## <a name="the-appinitialize-method"></a>La méthode App :: Initialize

Lors du lancement de l’application, la première méthode appelée par Windows est notre implémentation de [**IFrameworkView :: Initialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize).

Votre implémentation doit gérer les comportements les plus fondamentaux d’un jeu UWP, par exemple pour s’assurer que le jeu peut gérer un événement d’interruption (et un éventuel CV ultérieur) en s’abonnant à ces événements. Nous avons également accès à l’appareil de carte d’affichage ici. nous pouvons donc créer des ressources graphiques qui dépendent de l’appareil.

```cppwinrt
void Initialize(CoreApplicationView const& applicationView)
{
    applicationView.Activated({ this, &App::OnActivated });

    CoreApplication::Suspending({ this, &App::OnSuspending });

    CoreApplication::Resuming({ this, &App::OnResuming });

    // At this point we have access to the device. 
    // We can create the device-dependent resources.
    m_deviceResources = std::make_shared<DX::DeviceResources>();
}
```

Évitez autant que possible les pointeurs bruts (et c’est presque toujours possible).

- Pour les types de Windows Runtime, vous pouvez très souvent éviter les pointeurs entièrement et créer simplement une valeur sur la pile. Si vous avez besoin d’un pointeur, utilisez [**WinRT :: com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) (nous verrons un exemple de la prochaine fois).
- Pour les pointeurs uniques, utilisez [**std :: unique_ptr**](/cpp/cpp/how-to-create-and-use-unique-ptr-instances) et **std :: make_unique**.
- Pour les pointeurs partagés, utilisez [**std :: shared_ptr**](/cpp/cpp/how-to-create-and-use-shared-ptr-instances) et **std :: make_shared**.

## <a name="the-appsetwindow-method"></a>La méthode App :: SetWindow

Après **l’initialisation**, Windows appelle notre implémentation de [**IFrameworkView :: SetWindow**](/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow), en passant un objet [**CoreWindow**](/uwp/api/windows.ui.core.corewindow) représentant la fenêtre principale du jeu.

Dans **App :: SetWindow**, nous nous abonnons aux événements liés à la fenêtre et configurez des comportements de fenêtre et d’affichage. Par exemple, nous construisons un pointeur de souris (via la classe [**CoreCursor**](/uwp/api/windows.ui.core.corecursor) ), qui peut être utilisé par les contrôles Mouse et Touch. Nous transmettons également l’objet Window à notre objet de ressources dépendantes de l’appareil.

Pour plus d’informations sur la gestion des événements, consultez la rubrique [gestion du passage de jeux](tutorial-game-flow-management.md#event-handling) .

```cppwinrt
void SetWindow(CoreWindow const& window)
{
    //CoreWindow window = CoreWindow::GetForCurrentThread();
    window.Activate();

    window.PointerCursor(CoreCursor(CoreCursorType::Arrow, 0));

    PointerVisualizationSettings visualizationSettings{ PointerVisualizationSettings::GetForCurrentView() };
    visualizationSettings.IsContactFeedbackEnabled(false);
    visualizationSettings.IsBarrelButtonFeedbackEnabled(false);

    m_deviceResources->SetWindow(window);

    window.Activated({ this, &App::OnWindowActivationChanged });

    window.SizeChanged({ this, &App::OnWindowSizeChanged });

    window.Closed({ this, &App::OnWindowClosed });

    window.VisibilityChanged({ this, &App::OnVisibilityChanged });

    DisplayInformation currentDisplayInformation{ DisplayInformation::GetForCurrentView() };

    currentDisplayInformation.DpiChanged({ this, &App::OnDpiChanged });

    currentDisplayInformation.OrientationChanged({ this, &App::OnOrientationChanged });

    currentDisplayInformation.StereoEnabledChanged({ this, &App::OnStereoEnabledChanged });

    DisplayInformation::DisplayContentsInvalidated({ this, &App::OnDisplayContentsInvalidated });
}
```

## <a name="the-appload-method"></a>Méthode App :: Load

Maintenant que la fenêtre principale est définie, notre implémentation de [**IFrameworkView :: Load**](/uwp/api/windows.applicationmodel.core.iframeworkview.load) est appelée. La **charge** est un meilleur emplacement pour pré-extraire des données de jeu ou des ressources que **Initialize** et **SetWindow**.

```cppwinrt
void Load(winrt::hstring const& /* entryPoint */)
{
    if (!m_main)
    {
        m_main = winrt::make_self<GameMain>(m_deviceResources);
    }
}
```

Comme vous pouvez le voir, le travail réel est délégué au constructeur de l’objet **GameMain** que nous faisons ici. La classe **GameMain** est définie dans `GameMain.h` et `GameMain.cpp` .

### <a name="the-gamemaingamemain-constructor"></a>Constructeur GameMain :: GameMain

Le constructeur **GameMain** (et les autres fonctions membres qu’il appelle) commence un ensemble d’opérations de chargement asynchrones pour créer les objets de jeu, charger des ressources graphiques et initialiser la machine à États du jeu. Nous effectuons également toutes les préparations nécessaires avant le début du jeu, par exemple la définition d’États de départ ou de valeurs globales.

Windows impose une limite au moment où votre jeu peut être nécessaire avant de commencer le traitement de l’entrée. L’utilisation de asynchrone, comme nous le faisons ici, signifie que la **charge** peut retourner rapidement pendant que le travail qu’elle a commencé continue en arrière-plan. Si le chargement prend beaucoup de temps, ou s’il y a beaucoup de ressources, fournir à vos utilisateurs une barre de progression fréquemment mise à jour est une bonne idée. 

Si vous débutez en programmation asynchrone, consultez [opérations simultanées et asynchrones avec C++/WinRT](../cpp-and-winrt-apis/concurrency.md).

```cppwinrt
GameMain::GameMain(std::shared_ptr<DX::DeviceResources> const& deviceResources) :
    m_deviceResources(deviceResources),
    m_windowClosed(false),
    m_haveFocus(false),
    m_gameInfoOverlayCommand(GameInfoOverlayCommand::None),
    m_visible(true),
    m_loadingCount(0),
    m_updateState(UpdateEngineState::WaitingForResources)
{
    m_deviceResources->RegisterDeviceNotify(this);

    m_renderer = std::make_shared<GameRenderer>(m_deviceResources);
    m_game = std::make_shared<Simple3DGame>();

    m_uiControl = m_renderer->GameUIControl();

    m_controller = std::make_shared<MoveLookController>(CoreWindow::GetForCurrentThread());

    auto bounds = m_deviceResources->GetLogicalSize();

    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, bounds.Height - GameUIConstants::TouchRectangleSize),
        XMFLOAT2(GameUIConstants::TouchRectangleSize, bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(bounds.Width - GameUIConstants::TouchRectangleSize, bounds.Height - GameUIConstants::TouchRectangleSize),
        XMFLOAT2(bounds.Width, bounds.Height)
        );

    SetGameInfoOverlay(GameInfoOverlayState::Loading);
    m_uiControl->SetAction(GameInfoOverlayCommand::None);
    m_uiControl->ShowGameInfoOverlay();

    // Asynchronously initialize the game class and load the renderer device resources.
    // By doing all this asynchronously, the game gets to its main loop more quickly
    // and in parallel all the necessary resources are loaded on other threads.
    ConstructInBackground();
}

winrt::fire_and_forget GameMain::ConstructInBackground()
{
    auto lifetime = get_strong();

    m_game->Initialize(m_controller, m_renderer);

    co_await m_renderer->CreateGameDeviceResourcesAsync(m_game);

    // The finalize code needs to run in the same thread context
    // as the m_renderer object was created because the D3D device context
    // can ONLY be accessed on a single thread.
    // co_await of an IAsyncAction resumes in the same thread context.
    m_renderer->FinalizeCreateGameDeviceResources();

    InitializeGameState();

    if (m_updateState == UpdateEngineState::WaitingForResources)
    {
        // In the middle of a game so spin up the async task to load the level.
        co_await m_game->LoadLevelAsync();

        // The m_game object may need to deal with D3D device context work so
        // again the finalize code needs to run in the same thread
        // context as the m_renderer object was created because the D3D
        // device context can ONLY be accessed on a single thread.
        m_game->FinalizeLoadLevel();
        m_game->SetCurrentLevelToSavedState();
        m_updateState = UpdateEngineState::ResourcesLoaded;
    }
    else
    {
        // The game is not in the middle of a level so there aren't any level
        // resources to load.
    }

    // Since Game loading is an async task, the app visual state
    // may be too small or not have focus. Put the state machine
    // into the correct state to reflect these cases.

    if (m_deviceResources->GetLogicalSize().Width < GameUIConstants::MinPlayableWidth)
    {
        m_updateStateNext = m_updateState;
        m_updateState = UpdateEngineState::TooSmall;
        m_controller->Active(false);
        m_uiControl->HideGameInfoOverlay();
        m_uiControl->ShowTooSmall();
        m_renderNeeded = true;
    }
    else if (!m_haveFocus)
    {
        m_updateStateNext = m_updateState;
        m_updateState = UpdateEngineState::Deactivated;
        m_controller->Active(false);
        m_uiControl->SetAction(GameInfoOverlayCommand::None);
        m_renderNeeded = true;
    }
}

void GameMain::InitializeGameState()
{
    // Set up the initial state machine for handling Game playing state.
    ...
}
```

Voici un aperçu de la séquence de travail qui est lancée par le constructeur.

- Créez et initialisez un objet de type **GameRenderer**. Pour plus d’informations, consultez [rendu de l’infrastructure I : introduction au rendu](tutorial--assembling-the-rendering-pipeline.md).
- Créez et initialisez un objet de type **Simple3DGame**. Pour plus d’informations, consultez [définir l’objet de jeu principal](tutorial--defining-the-main-game-loop.md).
- Créez l’objet de contrôle de l’interface utilisateur du jeu et affichez la superposition des informations de jeu pour afficher une barre de progression à mesure de la charge des fichiers de ressources. Pour plus d’informations, consultez [Ajout d’une interface utilisateur](tutorial--adding-a-user-interface.md).
- Créez un objet contrôleur pour lire l’entrée à partir du contrôleur (tactile, souris ou contrôleur sans fil Xbox). Pour plus d’informations, consultez [Ajout de contrôles](tutorial--adding-controls.md).
- Définissez deux zones rectangulaires dans l’angle inférieur gauche et le coin inférieur droit de l’écran pour les contrôles de déplacement et d’appareil photo tactile, respectivement. Le joueur utilise le rectangle inférieur gauche (défini dans l’appel à **SetMoveRect**) comme un panneau de contrôle virtuel pour déplacer l’appareil photo vers l’avant et vers l’arrière, et de côté à côté. Le rectangle inférieur droit (défini par la méthode **SetFireRect** ) est utilisé comme un bouton virtuel pour déclencher l’AMMO.
- Utilisez des coroutines pour scinder le chargement des ressources en étapes distinctes. L’accès au contexte de périphérique Direct3D est limité au thread sur lequel le contexte de périphérique a été créé ; alors que l’accès à l’appareil Direct3D pour la création d’objets est libre de threads. Par conséquent, la Coroutine **GameRenderer :: CreateGameDeviceResourcesAsync** peut s’exécuter sur un thread distinct à partir de la tâche d’achèvement (**GameRenderer :: FinalizeCreateGameDeviceResources**), qui s’exécute sur le thread d’origine.
- Nous utilisons un modèle similaire pour le chargement des ressources de niveau avec **Simple3DGame :: LoadLevelAsync** et **Simple3DGame :: FinalizeLoadLevel**.

Pour plus d’informations sur **GameMain :: InitializeGameState** , consultez la rubrique suivante ([gestion du workflow de jeu](tutorial-game-flow-management.md)).

## <a name="the-apponactivated-method"></a>La méthode App :: OnActivated

Ensuite, l’événement [**CoreApplicationView :: Activated**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) est déclenché. Par conséquent, n’importe quel gestionnaire d’événements **OnActivated** (tel que notre méthode **App :: OnActivated** ) est appelé.

```cppwinrt
void OnActivated(CoreApplicationView const& /* applicationView */, IActivatedEventArgs const& /* args */)
{
    CoreWindow window = CoreWindow::GetForCurrentThread();
    window.Activate();
}
```

Le seul travail que nous faisons ici est d’activer le [**CoreWindow**](/uwp/api/windows.ui.core.corewindow)principal. Vous pouvez également choisir de le faire dans **App :: SetWindow**.

## <a name="the-apprun-method"></a>Méthode App :: Run

**Initialize**, **SetWindow**et **Load** ont défini l’étape. Maintenant que le jeu est en cours d’exécution, notre implémentation de [**IFrameworkView :: Run**](/uwp/api/windows.applicationmodel.core.iframeworkview.run) est appelée.

```cppwinrt
void Run()
{
    m_main->Run();
}
```

Là encore, le travail est délégué à **GameMain**.

### <a name="the-gamemainrun-method"></a>La méthode GameMain :: Run

**GameMain :: Run** est la principale boucle du jeu ; vous pouvez le trouver dans `GameMain.cpp` . La logique de base est que, lorsque la fenêtre de votre jeu reste ouverte, distribuez tous les événements, mettez à jour la minuterie, puis affichez et présentez les résultats du pipeline Graphics. En outre, les événements utilisés pour effectuer la transition entre les États du jeu sont distribués et traités.

Le code ici concerne également deux des États de la machine à États du moteur de jeu.

- **UpdateEngineState ::D eactivated**. Cela spécifie que la fenêtre de jeu est désactivée (a perdu le focus) ou est alignée. 
- **UpdateEngineState :: TooSmall**. Cela spécifie que la zone cliente est trop petite pour le rendu du jeu.

Dans l’un ou l’autre de ces États, le jeu interrompt le traitement des événements et attend que la fenêtre se focalise, qu’elle soit désalignée ou redimensionnée.

Lorsque votre jeu a le focus, vous devez gérer chaque événement qui arrive dans la file d’attente de messages, et vous devez donc appeler [**CoreWindowDispatch.ProcessEvents**](/uwp/api/windows.ui.core.coredispatcher.processevents) avec l’option **ProcessAllIfPresent**. D’autres options peuvent entraîner des retards dans le traitement des événements de message, ce qui peut rendre votre jeu peu réactif ou entraîner des comportements tactiles lents.

Lorsque le jeu n’est pas visible, est suspendu ou n’est pas aligné, vous ne voulez pas qu’il utilise des ressources cycliques pour distribuer les messages qui n’arrivent jamais. Dans ce cas, votre jeu doit utiliser l’option **ProcessOneAndAllPending** . Cette option bloque jusqu’à ce qu’elle récupère un événement, puis traite cet événement (ainsi que les autres qui arrivent dans la file d’attente de traitement pendant le traitement du premier). **CoreWindowDispatch. ProcessEvents** retourne immédiatement une valeur après le traitement de la file d’attente.

```cppwinrt
void GameMain::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible)
        {
            switch (m_updateState)
            {
            case UpdateEngineState::Deactivated:
            case UpdateEngineState::TooSmall:
                if (m_updateStateNext == UpdateEngineState::WaitingForResources)
                {
                    WaitingForResourceLoading();
                    m_renderNeeded = true;
                }
                else if (m_updateStateNext == UpdateEngineState::ResourcesLoaded)
                {
                    // In the device lost case, we transition to the final waiting state
                    // and make sure the display is updated.
                    switch (m_pressResult)
                    {
                    case PressResultState::LoadGame:
                        SetGameInfoOverlay(GameInfoOverlayState::GameStats);
                        break;

                    case PressResultState::PlayLevel:
                        SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
                        break;

                    case PressResultState::ContinueLevel:
                        SetGameInfoOverlay(GameInfoOverlayState::Pause);
                        break;
                    }
                    m_updateStateNext = UpdateEngineState::WaitingForPress;
                    m_uiControl->ShowGameInfoOverlay();
                    m_renderNeeded = true;
                }

                if (!m_renderNeeded)
                {
                    // The App is not currently the active window and not in a transient state so just wait for events.
                    CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
                    break;
                }
                // otherwise fall through and do normal processing to get the rendering handled.
            default:
                CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update();
                m_renderer->Render();
                m_deviceResources->Present();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close, so save state.
}
```

## <a name="the-appuninitialize-method"></a>La méthode App :: Uninitialize

Une fois le jeu terminé, notre implémentation de [**IFrameworkView :: Uninitialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.uninitialize) est appelée. Il s’agit de l’opportunité d’effectuer un nettoyage. La fermeture de la fenêtre de l’application n’entraîne pas l’arrêt du processus de l’application. au lieu de cela, il écrit l’état du singleton de l’application dans la mémoire. Si un problème particulier doit se produire lorsque le système récupère cette mémoire, y compris un nettoyage spécial des ressources, mettez le code pour ce nettoyage dans **Uninitialize**.

Dans notre cas, **App :: Uninitialize** est un non-op.

```cpp
void Uninitialize()
{
}
```

## <a name="tips"></a>Conseils

Lorsque vous développez votre propre jeu, concevez votre code de démarrage autour des méthodes décrites dans cette rubrique. Voici une simple liste de suggestions de base pour chaque méthode.

- Utilisez **Initialize** pour allouer vos classes principales et connectez-vous aux gestionnaires d’événements de base.
- Utilisez **SetWindow** pour vous abonner à n’importe quel événement spécifique à une fenêtre et pour passer votre fenêtre principale à votre objet de ressources dépendantes de l’appareil afin qu’il puisse utiliser cette fenêtre lors de la création d’une chaîne de permutation.
- Utilisez **Load** pour gérer toute configuration restante et pour initier la création asynchrone d’objets et le chargement des ressources. Si vous devez créer des fichiers temporaires ou des données, tels que des ressources générées de manière procédurale, faites-le ici également.

## <a name="next-steps"></a>Étapes suivantes

Cette rubrique a traité une partie de la structure de base d’un jeu UWP qui utilise DirectX. Il est judicieux de garder ces méthodes à l’esprit, car nous y reviendrons dans des rubriques ultérieures.

Dans la rubrique suivante &mdash; [gestion du workflow de jeu](tutorial-game-flow-management.md) &mdash; , nous examinerons en détail comment gérer les États de jeu et la gestion des événements afin de conserver le jeu.