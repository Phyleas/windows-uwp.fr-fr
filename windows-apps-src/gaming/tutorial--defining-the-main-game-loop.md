---
title: Définir l’objet jeu principal
description: À présent, nous examinons les détails de l’objet principal de l’exemple de jeu et de la façon dont les règles qu’il implémente traduisent en interactions avec le monde du jeu.
ms.assetid: 6afeef84-39d0-cb78-aa2e-2e42aef936c9
ms.date: 06/24/2020
ms.topic: article
keywords: Windows 10, UWP, jeux, objet principal
ms.localizationpriority: medium
ms.openlocfilehash: 9a6d087be6df93ee6798c29147f7fd1c820bd225
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409558"
---
# <a name="define-the-main-game-object"></a>Définir l’objet jeu principal

> [!NOTE]
> Cette rubrique fait partie de la série de didacticiels [créer un jeu de plateforme Windows universelle simple (UWP) avec DirectX](tutorial--create-your-first-uwp-directx-game.md) . La rubrique de ce lien définit le contexte de la série.

Une fois que vous avez disposé l’infrastructure de base de l’exemple de jeu et implémenté une machine à États qui gère les comportements utilisateur et système de haut niveau, vous souhaiterez examiner les règles et les mécanismes qui transforment l’exemple de jeu en jeu. Examinons les détails de l’objet principal du jeu d’exemples et comment traduire les règles de jeu en interactions avec le monde du jeu.

## <a name="objectives"></a>Objectifs

- Découvrez comment appliquer des techniques de développement de base pour implémenter des règles de jeu et des mécanismes pour un jeu DirectX UWP.

## <a name="main-game-object"></a>Objet de jeu principal

Dans l’exemple de jeu **Simple3DGameDX** , **Simple3DGame** est la classe d’objets de jeu principale. Une instance de **Simple3DGame** est construite, indirectement, via la méthode **App :: Load** .

Voici quelques-unes des fonctionnalités de la classe **Simple3DGame** .

- Contient l’implémentation de la logique de jeu.
- Contient des méthodes qui communiquent ces détails.
  - Modifications de l’état du jeu sur l’ordinateur d’état défini dans l’infrastructure de l’application.
  - Modifications de l’état du jeu de l’application à l’objet de jeu lui-même.
  - Détails de la mise à jour de l’interface utilisateur du jeu (superposition et tête d’affichage), des animations et de la physique (Dynamics).
  > [!NOTE]
  > La mise à jour des graphiques est gérée par la classe **GameRenderer** , qui contient des méthodes pour obtenir et utiliser des ressources de périphériques graphiques utilisées par le jeu. Pour plus d’informations, consultez [rendu de l’infrastructure I : introduction au rendu](tutorial--assembling-the-rendering-pipeline.md).
- Sert de conteneur pour les données qui définissent une session de jeu, un niveau ou une durée de vie, en fonction de la façon dont vous définissez votre jeu à un niveau élevé. Dans ce cas, les données d’État du jeu sont pour la durée de vie du jeu et sont initialisées une fois lorsqu’un utilisateur lance le jeu.

Pour afficher les méthodes et les données définies par cette classe, consultez [la classe Simple3DGame](#the-simple3dgame-class) ci-dessous.

## <a name="initialize-and-start-the-game"></a>Initialiser et démarrer le jeu

Lorsqu’un joueur démarre le jeu, l’objet jeu doit initialiser son état, créer et ajouter la superposition, définir les variables qui suivent les performances du joueur et instancier les objets qui seront utilisés pour générer les niveaux. Dans cet exemple, cette opération est effectuée lorsque l’instance **GameMain** est créée dans **App :: Load**.

L’objet de jeu, de type **Simple3DGame**, est créé dans le constructeur **GameMain :: GameMain** . Elle est ensuite initialisée à l’aide de la méthode **Simple3DGame :: Initialize** pendant la Coroutine de **GameMain :: ConstructInBackground** Fire-and-oublier, qui est appelée à partir de **GameMain :: GameMain**.

### <a name="the-simple3dgameinitialize-method"></a>Méthode Simple3DGame :: Initialize

L’exemple de jeu définit ces composants dans l’objet de jeu.

- Un nouvel objet lecture audio est créé.
- Des tableaux pour les primitives graphiques du jeu sont créés, notamment des tableaux pour les primitives de niveau, les munitions et les obstacles.
- Un emplacement pour enregistrer les données de l’état du jeu est créé : il est nommé *Game* et est placé dans l’emplacement de stockage des paramètres des données d’application spécifié par [**ApplicationData::Current**](/uwp/api/windows.storage.applicationdata.current).
- Un minuteur de jeu et la bitmap de superposition initiale, intégrée au jeu, sont créés.
- Une nouvelle caméra est créée avec un ensemble spécifique de paramètres de vue et de projection.
- Le périphérique d’entrée (contrôleur) étant défini sur les mêmes tangage et lacet de départ que la caméra, le joueur a une correspondance un-à-un entre la position du contrôle de départ et la position de la caméra.
- L’objet joueur est créé et associé à l’état actif. Nous utilisons un objet Sphere pour détecter la proximité du joueur aux murs et obstacles et pour empêcher l’appareil photo de se placer dans une position susceptible de briser l’immersion.
- La primitive du monde du jeu est créée.
- Les obstacles sous forme de cylindres sont créés.
- Les cibles (objets **Face**) sont créées et numérotées.
- Les sphères de munitions sont créées.
- Les niveaux sont créés.
- Les meilleurs scores sont chargés.
- Tout état de jeu précédemment enregistré est chargé.

Le jeu possède maintenant des instances de tous les principaux composants &mdash; du monde, le joueur, les obstacles, les cibles et le AMMO. Il a également des instances des niveaux, qui représentent les configurations de tous les composants ci-dessus ainsi que leurs comportements pour chaque niveau spécifique. Voyons maintenant comment le jeu crée les niveaux.

## <a name="build-and-load-game-levels"></a>Générer et charger des niveaux de jeu

La majeure partie du travail important pour la construction du niveau est effectuée dans les `Level[N].h/.cpp` fichiers figurant dans le dossier **GameLevels** de l’exemple de solution. Étant donné qu’elle se concentre sur une implémentation très spécifique, nous ne les présenterons pas ici. L’important est que le code de chaque niveau soit exécuté sous la forme d’un objet de **niveau [N]** distinct. Si vous souhaitez étendre le jeu, vous pouvez créer un objet **Level [N]** qui accepte un nombre affecté comme paramètre et place de façon aléatoire les obstacles et les cibles. Ou bien, vous pouvez lui demander des données de configuration de niveau de charge à partir d’un fichier de ressources, ou même d’Internet.

## <a name="define-the-gameplay"></a>Définir le jeu

À ce stade, nous disposons de tous les composants dont nous avons besoin pour développer le jeu. Les niveaux ont été construits en mémoire à partir des primitives et sont prêts pour que le joueur commence à interagir avec.

Les meilleurs jeux réagissent instantanément à la saisie des joueurs et fournissent des commentaires immédiats. Cela est vrai pour tout type de jeu, des Twitch à l’action, des pousseurs de la première personne en temps réel à des jeux de stratégie réfléchis et basés sur l’activation.

### <a name="the-simple3dgamerungame-method"></a>La méthode Simple3DGame :: RunGame

Lorsqu’un niveau de jeu est en cours, le jeu est dans l’état **Dynamics** . 

**GameMain :: Update** est la boucle de mise à jour principale qui met à jour l’état de l’application une fois par Frame, comme indiqué ci-dessous. La boucle Update appelle la méthode **Simple3DGame :: RunGame** pour gérer le travail si le jeu est dans l’état **Dynamics** .

```cppwinrt
// Updates the application state once per frame.
void GameMain::Update()
{
    // The controller object has its own update loop.
    m_controller->Update();

    switch (m_updateState)
    {
    ...
    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested())
        {
            ...
        }
        else
        {
            // When the player is playing, work is done by Simple3DGame::RunGame.
            GameState runState = m_game->RunGame();
            switch (runState)
            {
                ...
```
          
**Simple3DGame :: RunGame** gère le jeu de données qui définit l’état actuel de la lecture du jeu pour l’itération actuelle de la boucle de jeu.

Voici la logique du workflow de jeu dans **Simple3DGame :: RunGame**.

- La méthode met à jour la minuterie qui compte les secondes jusqu’à ce que le niveau soit terminé, et vérifie si l’heure du niveau a expiré. Il s’agit de l’une des règles du jeu &mdash; lorsque le temps est écoulé, si toutes les cibles n’ont pas été capturées, alors la partie est terminée.
- Si le temps s’est écoulé, la méthode définit l’état du jeu **TimeExpired** et retourne à la méthode **Update** dans le code précédent.
- Si le temps reste, alors le contrôleur de déplacement-look est interrogé pour obtenir une mise à jour de la position de la caméra. plus précisément, une mise à jour de l’angle de la vue projetait normalement à partir du plan de l’appareil photo (où le joueur regarde) et de la distance à laquelle l’angle a été déplacé depuis la dernière interrogation du contrôleur.
- La caméra est mise à jour en fonction des nouvelles données du contrôleur de déplacement/vue.
- La dynamique, ou les animations et comportements des objets du monde du jeu indépendants du contrôle du joueur, sont mis à jour. Dans cet exemple de jeu, la méthode **Simple3DGame :: UpdateDynamics** est appelée pour mettre à jour le mouvement des sphères AMMO qui ont été déclenchées, l’animation des obstacles de pilier et le déplacement des cibles. Pour plus d’informations, consultez [mettre à jour le monde du jeu](#update-the-game-world).
- La méthode vérifie si les critères de réussite d’un niveau ont été atteints. Si c’est le cas, il finalise le score pour le niveau et vérifie s’il s’agit du dernier niveau (de 6). S’il s’agit du dernier niveau, la méthode retourne l’état du jeu **GameState :: GameComplete** ; Sinon, elle retourne l’état du jeu **GameState :: LevelComplete** .
- Si le niveau n’est pas terminé, la méthode définit l’état du jeu sur **GameState :: active**et retourne.

## <a name="update-the-game-world"></a>Mettre à jour le monde du jeu

Dans cet exemple, lorsque le jeu est en cours d’exécution, la méthode **Simple3DGame :: UpdateDynamics** est appelée à partir de la méthode **Simple3DGame :: RunGame** (qui est appelée à partir de **GameMain :: Update**) pour mettre à jour les objets qui sont rendus dans une scène de jeu.

Une boucle telle que **UpdateDynamics** appelle toutes les méthodes utilisées pour définir le monde du jeu en mouvement, indépendamment de l’entrée du joueur, pour créer une expérience de jeu immersif et rendre le niveau actif. Cela inclut les graphiques qui doivent être rendus et l’exécution de boucles d’animation pour se déplacer sur un monde dynamique, même lorsqu’il n’y a pas d’entrée de joueur. Dans votre jeu, il peut s’agir d’arbres en ligne dans le vent, d’ondulations sur terre, de machines à fumer et d’étirement et de déplacement de monstres étrangers. Il englobe également l’interaction entre les objets, y compris les collisions entre la sphère du joueur et le monde, ou entre les munitions et les obstacles et cibles.

Sauf lorsque le jeu est en pause, la boucle de jeu doit continuer à mettre à jour le monde du jeu ; Si cette fonction est basée sur une logique de jeu, sur des algorithmes physiques ou si elle est simplement aléatoire.

Dans l’exemple de jeu, ce principe est appelé *Dynamics*et il englobe la hausse et la chute des obstacles de fondement, et les comportements et les comportements physiques des Ammos à mesure qu’ils sont déclenchés et en mouvement.

### <a name="the-simple3dgameupdatedynamics-method"></a>La méthode Simple3DGame :: UpdateDynamics 

Cette méthode gère ces quatre ensembles de calculs.

- Les positions des sphères de munitions tirées dans le monde.
- L’animation des colonnes d’obstacles.
- L’intersection du joueur et des limites du monde.
- Les collisions entre les sphères de munitions et les obstacles, les cibles, d’autres sphères de munitions et le monde.

L’animation des obstacles a lieu dans une boucle définie dans les fichiers de code source **Animate. h/. cpp** . Le comportement des AMMO et des collisions est défini par des algorithmes physiques simplifiés, fournis dans le code et paramétrés par un ensemble de constantes globales pour le monde du jeu, y compris les propriétés de gravité et de matériau. Tout cela est calculé dans l’espace de coordonnées du monde du jeu.

### <a name="review-the-flow"></a>Examiner le Flow

Maintenant que nous avons mis à jour tous les objets de la scène et calculé les collisions, nous devons utiliser ces informations pour dessiner les modifications visuelles correspondantes. 

Une fois **GameMain :: Update** a terminé l’itération actuelle de la boucle de jeu, l’exemple appelle immédiatement **GameRenderer :: Render** pour prendre les données d’objet mises à jour et générer une nouvelle scène à présenter au joueur, comme indiqué ci-dessous.

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
                ...
                // Otherwise, fall through and do normal processing to perform rendering.
            default:
                CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(
                    CoreProcessEventsOption::ProcessAllIfPresent);
                // GameMain::Update calls Simple3DGame::RunGame. If game is in Dynamics
                // state, uses Simple3DGame::UpdateDynamics to update game world.
                Update();
                // Render is called immediately after the Update loop.
                m_renderer->Render();
                m_deviceResources->Present();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(
                CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close, so save state.
}
```

## <a name="render-the-game-worlds-graphics"></a>Restituer les graphiques du monde du jeu

Nous recommandons que les graphiques d’un jeu soient mis à jour souvent, idéalement aussi souvent que la boucle de jeu principale. À mesure que la boucle itère, l’état du monde du jeu est mis à jour, avec ou sans entrée de joueur. Cela permet d’afficher correctement les animations et les comportements calculés. Imaginez si nous avions une scène simple d’eau qui a été déplacée uniquement lorsque le joueur a appuyé sur un bouton. Cela ne serait pas réaliste ; un bon jeu semble lisse et fluide tout le temps.

Souvenez-vous de la boucle de l’exemple de jeu comme indiqué ci-dessus dans **GameMain :: Run**. Si la fenêtre principale du jeu est visible et n’est pas alignée ou désactivée, le jeu continue à mettre à jour et à restituer les résultats de cette mise à jour. La méthode **GameRenderer :: Render** que nous examinons ensuite affiche une représentation de cet État. Cette opération est effectuée immédiatement après un appel à **GameMain :: Update**, qui comprend **Simple3DGame :: RunGame** pour mettre à jour des États, comme indiqué dans la section précédente.

**GameRenderer :: Render** dessine la projection du monde 3D, puis dessine la superposition de Direct2D par-dessus. Une fois terminé, elle présente la chaîne de permutation finale avec les tampons combinés à afficher.

> [!NOTE]
> Il existe deux États pour la superposition de Direct2D du jeu &mdash; , où le jeu affiche la superposition des informations de jeu qui contient l’image bitmap pour le menu pause, et l’autre où le jeu affiche la Croix avec les rectangles pour le contrôleur de déplacement-look de l’écran tactile. Le texte des scores est écrit dans les deux états. Pour plus d’informations, consultez [rendu de l’infrastructure I : introduction au rendu](tutorial--assembling-the-rendering-pipeline.md).

### <a name="the-gamerendererrender-method"></a>Méthode GameRenderer :: Render

```cppwinrt
void GameRenderer::Render()
{
    bool stereoEnabled{ m_deviceResources->GetStereoState() };

    auto d3dContext{ m_deviceResources->GetD3DDeviceContext() };
    auto d2dContext{ m_deviceResources->GetD2DDeviceContext() };

    ...
        if (m_game != nullptr && m_gameResourcesLoaded && m_levelResourcesLoaded)
        {
            // This section is only used after the game state has been initialized and all device
            // resources needed for the game have been created and associated with the game objects.
            ...
            for (auto&& object : m_game->RenderObjects())
            {
                object->Render(d3dContext, m_constantBufferChangesEveryPrim.get());
            }
        }

        d3dContext->BeginEventInt(L"D2D BeginDraw", 1);
        d2dContext->BeginDraw();

        // To handle the swapchain being pre-rotated, set the D2D transformation to include it.
        d2dContext->SetTransform(m_deviceResources->GetOrientationTransform2D());

        if (m_game != nullptr && m_gameResourcesLoaded)
        {
            // This is only used after the game state has been initialized.
            m_gameHud.Render(m_game);
        }

        if (m_gameInfoOverlay.Visible())
        {
            d2dContext->DrawBitmap(
                m_gameInfoOverlay.Bitmap(),
                m_gameInfoOverlayRect
                );
        }
        ...
    }
}
```

## <a name="the-simple3dgame-class"></a>La classe Simple3DGame

Il s’agit des méthodes et des membres de données définis par la classe **Simple3DGame** .

### <a name="member-functions"></a>Fonctions Membre

Les fonctions membres publiques définies par **Simple3DGame** incluent celles ci-dessous.

- **Initialisation**en cours. Définit les valeurs de départ des variables globales et initialise les objets de jeu. Ce sujet est abordé dans la section [Initialize and Start The Game](#initialize-and-start-the-game) .
- **LoadGame**. Initialise un nouveau niveau et commence à le charger.
- **LoadLevelAsync**. Coroutine qui initialise le niveau, puis appelle une autre Coroutine sur le convertisseur pour charger les ressources de niveau spécifique à l’appareil. Cette méthode s’exécute dans un thread séparé ; seules les méthodes [**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device) (par opposition aux méthodes [**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)) peuvent être appelées à partir de ce thread. Toutes les méthodes de contexte de périphérique sont appelées dans la méthode **FinalizeLoadLevel**. Si vous débutez en programmation asynchrone, consultez [opérations simultanées et asynchrones avec C++/WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency).
- **FinalizeLoadLevel**. Finit tout travail de chargement de niveau à effectuer sur le thread principal. Cela comprend tout appel aux méthodes ([**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)) de contexte de périphérique Direct3D 11.
- **StartLevel**. Démarre le jeu pour un nouveau niveau.
- **PauseGame**. Suspend le jeu.
- **RunGame**. Exécute une itération de la boucle de jeu. Cette méthode est appelée à partir d’**App::Update** une fois par itération de la boucle de jeu si le jeu présente l’état **Active**.
- **OnSuspending** et **OnResuming**. Suspendez/reprenez l’audio du jeu, respectivement.

Voici les fonctions membres privées.

- **LoadSavedState** et **SaveState**. Chargez/enregistrez l’état actuel du jeu, respectivement.
- **LoadHighScore** et **SaveHighScore**. Chargez/enregistrez le score élevé entre les jeux, respectivement.
- **InitializeAmmo**. Redéfinit l’état de chaque objet sphère utilisé comme munition dans son état d’origine au début de chaque partie.
- **UpdateDynamics**. Il s’agit d’une méthode importante, car elle met à jour tous les objets de jeu en fonction des routines d’animation consorties, de la physique et de l’entrée de contrôle. Elle représente le cœur de l’interactivité qui définit le jeu. Ce sujet est abordé dans la section [mettre à jour le monde du jeu](#update-the-game-world) .

Les autres méthodes publiques sont l’accesseur de propriété qui retourne des informations spécifiques au jeu et à la superposition dans l’infrastructure d’application pour l’affichage.

### <a name="data-members"></a>Membres de données

Ces objets sont mis à jour au fur et à mesure de l’exécution de la boucle du jeu.

- Objet **MoveLookController** . Représente l’entrée du lecteur. Pour plus d’informations, consultez [Ajout de contrôles](tutorial--adding-controls.md).
- Objet **GameRenderer** . Représente un convertisseur Direct3D 11 qui gère tous les objets spécifiques à l’appareil et leur rendu. Pour plus d’informations, consultez rendu de l' [infrastructure I](tutorial--assembling-the-rendering-pipeline.md).
- Objet **audio** . Contrôle la lecture audio pour le jeu. Pour plus d’informations, consultez [Ajout de sons](tutorial--adding-sound.md).

Les autres variables de jeu contiennent les listes des primitives et leurs quantités respectives, ainsi que des données et des contraintes spécifiques aux jeux.

## <a name="next-steps"></a>Étapes suivantes

Nous n’avons pas encore parlé du moteur de rendu réel dans &mdash; lequel les appels aux méthodes **Render** sur les primitives mises à jour sont convertis en pixels sur votre écran. Ces aspects sont traités dans l’infrastructure de rendu de deux parties &mdash; [I : présentation du rendu](tutorial--assembling-the-rendering-pipeline.md) et du [rendu Framework II : rendu de jeu](tutorial-game-rendering.md). Si vous êtes plus intéressé par la manière dont le lecteur contrôle la mise à jour de l’état du jeu, consultez [Ajout de contrôles](tutorial--adding-controls.md).
