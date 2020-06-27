---
title: Gestion de flux de jeu
description: Découvrez comment initialiser des États de jeu, gérer des événements et configurer la boucle de mise à jour du jeu.
ms.assetid: 6c33bf09-b46a-4bb5-8a59-ca83ce257eb3
ms.date: 06/24/2020
ms.topic: article
keywords: windows 10, uwp, jeux, directx
ms.localizationpriority: medium
ms.openlocfilehash: 181eca743a9ccdc76ebfc1302e8bb04d85a32269
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409628"
---
# <a name="game-flow-management"></a>Gestion de flux de jeu

> [!NOTE]
> Cette rubrique fait partie de la série de didacticiels [créer un jeu de plateforme Windows universelle simple (UWP) avec DirectX](tutorial--create-your-first-uwp-directx-game.md) . La rubrique de ce lien définit le contexte de la série.

Le jeu possède maintenant une fenêtre, a inscrit des gestionnaires d’événements et a chargé des ressources de manière asynchrone. Cette rubrique explique comment utiliser les États de jeu, comment gérer des États de jeu clés spécifiques et comment créer une boucle de mise à jour pour le moteur de jeu. Ensuite, nous allons étudier le déroulement de l’interface utilisateur et, enfin, en savoir plus sur les gestionnaires d’événements qui sont nécessaires pour un jeu UWP.

## <a name="game-states-used-to-manage-game-flow"></a>États de jeu utilisés pour gérer le workflow de jeu

Nous utilisons les États de jeu pour gérer le déroulement du jeu.

Lorsque l’exemple de jeu **Simple3DGameDX** s’exécute pour la première fois sur un ordinateur, il est dans un État où aucun jeu n’a été démarré. La prochaine fois que le jeu s’exécute, il peut être dans l’un de ces États.

- Aucun jeu n’a été démarré ou le jeu est compris entre les niveaux (le score élevé est zéro).
- La boucle de jeu est en cours d’exécution et se trouve au milieu d’un niveau.
- La boucle de jeu n’est pas en cours d’exécution en raison d’un jeu terminé (le score élevé a une valeur différente de zéro).

Votre jeu peut avoir autant d’États que nécessaire. Mais n’oubliez pas qu’il peut être arrêté à tout moment. Et lorsqu’il reprend, l’utilisateur s’attend à ce qu’il reprend dans l’État où il se trouvait lorsqu’il a été arrêté.

## <a name="game-state-management"></a>Gestion de l’état du jeu

Ainsi, lors de l’initialisation du jeu, vous devez prendre en charge le jeu à froid et reprendre le jeu après l’avoir arrêté en vol. L’exemple **Simple3DGameDX** enregistre toujours son état de jeu afin d’obtenir l’impression qu’il n’a jamais cessé.

En réponse à un événement d’interruption, le jeu est suspendu, mais les ressources du jeu sont toujours en mémoire. De même, l’événement Resume est géré pour s’assurer que l’exemple de jeu se trouve dans l’État où il se trouvait lorsqu’il a été suspendu ou arrêté. Selon l’état, différentes options sont présentées au joueur. 

- Si le jeu reprend un niveau moyen, il apparaît en pause et la superposition offre la possibilité de continuer.
- Si le jeu reprend dans un État où le jeu est terminé, il affiche les scores élevés et une option pour jouer à un nouveau jeu.
- Enfin, si le jeu reprend avant le début d’un niveau, la superposition présente une option de démarrage à l’utilisateur.

L’exemple de jeu ne fait pas la distinction entre le jeu et le démarrage à froid, le lancement pour la première fois sans événement d’interruption, ou la reprise d’un état suspendu. Il s’agit de la conception appropriée pour toute application UWP.

Dans cet exemple, l’initialisation des États du jeu se produit dans [**GameMain :: InitializeGameState**](#the-gamemaininitializegamestate-method) (une structure de cette méthode est affichée dans la section suivante).

Voici un organigramme pour vous aider à visualiser le flux. Il couvre à la fois l’initialisation et la boucle de mise à jour.

- L’initialisation commence au nœud de **démarrage** lorsque vous vérifiez l’état actuel du jeu. Pour le code de jeu, consultez [**GameMain :: InitializeGameState**](#the-gamemaininitializegamestate-method) dans la section suivante.
* Pour plus d’informations sur la boucle de mise à jour, consultez [mettre à jour le moteur de jeu](#update-game-engine). Pour code de jeu, accédez à [**GameMain :: Update**](#the-gamemainupdate-method).

![Machine à états principale de notre jeu](images/simple-dx-game-flow-statemachine.png)

### <a name="the-gamemaininitializegamestate-method"></a>La méthode GameMain :: InitializeGameState

La méthode **GameMain :: InitializeGameState** est appelée indirectement via le constructeur de la classe **GameMain** , qui est le résultat de la création d’une instance **GameMain** dans **App :: Load**.

```cppwinrt
GameMain::GameMain(std::shared_ptr<DX::DeviceResources> const& deviceResources) : ...
{
    m_deviceResources->RegisterDeviceNotify(this);
    ...
    ConstructInBackground();
}

winrt::fire_and_forget GameMain::ConstructInBackground()
{
    ...
    m_renderer->FinalizeCreateGameDeviceResources();

    InitializeGameState();
    ...
}

void GameMain::InitializeGameState()
{
    // Set up the initial state machine for handling Game playing state.
    if (m_game->GameActive() && m_game->LevelActive())
    {
        // The last time the game terminated it was in the middle
        // of a level.
        // We are waiting for the user to continue the game.
        ...
    }
    else if (!m_game->GameActive() && (m_game->HighScore().totalHits > 0))
    {
        // The last time the game terminated the game had been completed.
        // Show the high score.
        // We are waiting for the user to acknowledge the high score and start a new game.
        // The level resources for the first level will be loaded later.
        ...
    }
    else
    {
        // This is either the first time the game has run or
        // the last time the game terminated the level was completed.
        // We are waiting for the user to begin the next level.
        ...
    }
    m_uiControl->ShowGameInfoOverlay();
}
```

## <a name="update-game-engine"></a>Mettre à jour le moteur de jeu

La méthode **App :: Run** appelle **GameMain :: Run**. Dans **GameMain :: Run** est un ordinateur d’état de base pour gérer toutes les actions majeures qu’un utilisateur peut effectuer. Le niveau le plus élevé de cet ordinateur d’État concerne le chargement d’un jeu, la diffusion d’un niveau spécifique ou la poursuite d’un niveau après que le jeu a été suspendu (par le système ou par l’utilisateur).

Dans l’exemple de jeu, il existe 3 États principaux (représentés par l’énumération **UpdateEngineState** ) dans lesquels le jeu peut se trouver.

1. **UpdateEngineState :: WaitingForResources**. La boucle de jeu effectue une itération, incapable de procéder à la transition tant que les ressources (en particulier, les ressources graphiques) ne sont pas disponibles. Une fois les tâches de chargement de ressources Async terminées, nous mettons à jour l’État en **UpdateEngineState :: ResourcesLoaded**. Cela se produit généralement entre les niveaux lorsque le niveau charge de nouvelles ressources à partir du disque, d’un serveur de jeux ou d’un backend Cloud. Dans l’exemple de jeu, nous simulons ce comportement, car l’exemple n’a pas besoin de ressources par niveau supplémentaires à ce moment-là.
2. **UpdateEngineState :: WaitingForPress**. La boucle de jeu effectue une itération, en attente d’une entrée utilisateur spécifique. Cette entrée est une action de joueur permettant de charger un jeu, de démarrer un niveau ou de continuer un niveau. L’exemple de code fait référence à ces sous-États via l’énumération **PressResultState** .
3. **UpdateEngineState ::D ynamics**. La boucle de jeu est en cours d’exécution et l’utilisateur joue. Pendant que l’utilisateur est en train de jouer, le jeu vérifie les 3 conditions sur lesquelles il peut passer : 
 - **GameState :: TimeExpired**. Expiration de la limite de temps pour un niveau.
 - **GameState :: LevelComplete**. Achèvement d’un niveau par le joueur.
 - **GameState :: GameComplete**. Achèvement de tous les niveaux par le joueur.

Un jeu est simplement une machine à États contenant plusieurs machines à États plus petites. Chaque État spécifique doit être défini par des critères très spécifiques. Les transitions d’un État à un autre doivent reposer sur une entrée utilisateur discrète ou sur des actions système (telles que le chargement des ressources graphiques).

Lors de la planification de votre jeu, pensez à dessiner l’ensemble du jeu pour vous assurer que vous avez résolu toutes les actions possibles que l’utilisateur ou le système peut effectuer. Un jeu peut être très compliqué, donc un ordinateur d’État est un outil puissant qui vous permet de visualiser cette complexité et de le rendre plus gérable.

Jetons un coup d’œil au code de la boucle de mise à jour.

### <a name="the-gamemainupdate-method"></a>GameMain :: Update, méthode

Il s’agit de la structure de l’ordinateur d’État utilisé pour mettre à jour le moteur de jeu.

```cppwinrt
void GameMain::Update()
{
    // The controller object has its own update loop.
    m_controller->Update(); 

    switch (m_updateState)
    {
    case UpdateEngineState::WaitingForResources:
        ...
        break;

    case UpdateEngineState::ResourcesLoaded:
        ...
        break;

    case UpdateEngineState::WaitingForPress:
        if (m_controller->IsPressComplete())
        {
            ...
        }
        break;

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
            case GameState::TimeExpired:
                ...
                break;

            case GameState::LevelComplete:
                ...
                break;

            case GameState::GameComplete:
                ...
                break;
            }
        }

        if (m_updateState == UpdateEngineState::WaitingForPress)
        {
            // Transitioning state, so enable waiting for the press event.
            m_controller->WaitForPress(
                m_renderer->GameInfoOverlayUpperLeft(),
                m_renderer->GameInfoOverlayLowerRight());
        }
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // Transitioning state, so shut down the input controller
            // until resources are loaded.
            m_controller->Active(false);
        }
        break;
    }
}
```

## <a name="update-the-user-interface"></a>Mettre à jour l’interface utilisateur

Nous devons conserver le lecteur informé de l’état du système et autoriser le changement de l’état du jeu en fonction des actions du joueur et des règles qui définissent le jeu. De nombreux jeux, y compris cet exemple de jeu, utilisent généralement des éléments d’interface utilisateur pour présenter ces informations au joueur. L’interface utilisateur contient des représentations de l’état du jeu et d’autres informations spécifiques à la lecture, telles que le score, le AMMO ou le nombre de chances restant. L’interface utilisateur est également appelée superposition, car elle est rendue séparément du pipeline graphique principal et placée au-dessus de la projection 3D.

Certaines informations de l’interface utilisateur sont également présentées sous la forme d’un affichage de tête à haut (HUD) pour permettre à l’utilisateur de voir ces informations sans avoir à tenir leurs yeux entièrement dans la zone de jeu principale. Dans l’exemple de jeu, nous créons cette superposition à l’aide des API Direct2D. Vous pouvez également créer cette superposition à l’aide de XAML, que nous aborderons pour [étendre l’exemple de jeu](tutorial-resources.md).

Il existe deux composants à l’interface utilisateur.

- Le HUD qui contient le score et les informations sur l’état actuel du jeu.
- La bitmap de pause, qui est un rectangle noir avec un texte superposé lorsque le jeu est dans l’état de pause/suspension. Il s’agit de la superposition du jeu. Nous en parlons plus tard dans [Ajout d’une interface utilisateur](tutorial--adding-a-user-interface.md).

Rien d’étonnant à cela, la superposition a également une machine à états. La superposition peut afficher un message de début ou de jeu. Il s’agit essentiellement d’un canevas sur lequel nous pouvons générer des informations sur l’état du jeu que nous souhaitons afficher au joueur pendant que le jeu est suspendu ou suspendu.

La superposition affichée peut être l’un de ces six écrans, en fonction de l’état du jeu.

1. Écran de progression du chargement des ressources au début du jeu.
2. Écran des statistiques de jeu.
3. Écran de démarrage du message de niveau.
4. Écran de jeu lorsque tous les niveaux sont terminés sans délai d’exécution.
5. À l’écran de jeu lorsque le temps est écoulé.
6. Arrêtez l’écran du menu.

La séparation de l’interface utilisateur du pipeline graphique de votre jeu vous permet de l’utiliser indépendamment du moteur de rendu graphique du jeu et de réduire considérablement la complexité du code de votre jeu.

Voici comment l’exemple de jeu structure la machine à États de la superposition.

```cppwinrt
void GameMain::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_uiControl->SetGameLoading(m_loadingCount);
        break;

    case GameInfoOverlayState::GameStats:
        ...
        break;

    case GameInfoOverlayState::LevelStart:
        ...
        break;

    case GameInfoOverlayState::GameOverCompleted:
        ...
        break;

    case GameInfoOverlayState::GameOverExpired:
        ...
        break;

    case GameInfoOverlayState::Pause:
        ...
        break;
    }
}
```

## <a name="event-handling"></a>Gestion des événements

Comme nous l’avons vu dans la rubrique [définir la structure d’application UWP du jeu](tutorial--building-the-games-uwp-app-framework.md) , la plupart des méthodes de fournisseur d’affichage de la classe **app** inscrivent des gestionnaires d’événements. Ces méthodes doivent gérer correctement ces événements importants avant d’ajouter des mécanismes de jeu ou de démarrer le développement graphique.

La gestion appropriée des événements en question est fondamentale pour l’expérience de l’application UWP. Étant donné qu’une application UWP peut à tout moment être activée, désactivée, redimensionnée, alignée, désactivée, suspendue ou reprise, le jeu doit s’inscrire à ces événements dès que possible et les gérer de manière à ce que l’expérience soit harmonieuse et prévisible pour le joueur.

Ce sont les gestionnaires d’événements utilisés dans cet exemple et les événements qu’ils gèrent.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Gestionnaire d'événements</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">OnActivated</td>
<td align="left">Gère <a href="/uwp/api/windows.applicationmodel.core.coreapplicationview.activated"><strong>CoreApplicationView :: Activated</strong></a>. L’application de jeu ayant été amenée au premier plan, la fenêtre principale est activée.</td>
</tr>
<tr class="even">
<td align="left">OnDpiChanged</td>
<td align="left">Gère les <a href="/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_DpiChanged"><strong>graphiques ::DSE ::D isplayinformation ::D pichanged</strong></a>. La résolution de l’affichage a changé et le jeu ajuste ses ressources en conséquence.
<div class="alert">
<strong>Notez</strong>que les coordonnées <a href="/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow"><strong>CoreWindow</strong></a> sont en pixels indépendants du périphérique (DIP) pour <a href="/windows/desktop/Direct2D/direct2d-overview">Direct2D</a>. Par conséquent, vous devez indiquer à Direct2D la modification des PPP afin d’afficher correctement les primitives ou composants 2D.
</div>
<div>
</div></td>
</tr>
<tr class="odd">
<td align="left">OnOrientationChanged</td>
<td align="left">Gère les <a href="/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_OrientationChanged"><strong>graphiques ::DSE ::D isplayinformation :: OrientationChanged</strong></a>. L’orientation des modifications et du rendu de l’affichage doit être mise à jour.</td>
</tr>
<tr class="even">
<td align="left">OnDisplayContentsInvalidated</td>
<td align="left">Gère les <a href="/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_DisplayContentsInvalidated"><strong>graphiques ::DSE ::D isplayinformation ::D isplaycontentsinvalidated</strong></a>. L’affichage nécessite un redessin et votre jeu doit être à nouveau restitué.</td>
</tr>
<tr class="odd">
<td align="left">OnResuming</td>
<td align="left">Gère <a href="/uwp/api/windows.applicationmodel.core.coreapplication.resuming"><strong>CoreApplication ::</strong></a>Reversion. L’application de jeu restaure le jeu qui est dans un état suspendu.</td>
</tr>
<tr class="even">
<td align="left">OnSuspending</td>
<td align="left">Gère <a href="/uwp/api/windows.applicationmodel.core.coreapplication.suspending"><strong>CoreApplication :: suspending</strong></a>. L’application de jeu enregistre son état sur disque. Elle dispose de 5 secondes pour enregistrer l’état dans le dispositif de stockage.</td>
</tr>
<tr class="odd">
<td align="left">OnVisibilityChanged</td>
<td align="left">Gère <a href="/uwp/api/windows.ui.core.corewindow.visibilitychanged"><strong>CoreWindow :: VisibilityChanged</strong></a>. L’application de jeu a modifié la visibilité : elle est devenue soit visible, soit invisible car une autre application est devenue visible.</td>
</tr>
<tr class="even">
<td align="left">OnWindowActivationChanged</td>
<td align="left">Gère <a href="/uwp/api/windows.ui.core.corewindow.activated"><strong>CoreWindow :: Activated</strong></a>. La fenêtre principale de l’application de jeu ayant été activée ou désactivée, elle doit supprimer le focus et interrompre le jeu, ou regagner le focus. Dans les deux cas, la superposition indique que le jeu est suspendu.</td>
</tr>
<tr class="odd">
<td align="left">OnWindowClosed</td>
<td align="left">Gère <a href="/uwp/api/windows.ui.core.corewindow.closed"><strong>CoreWindow :: Closed</strong></a>. L’application de jeu ferme la fenêtre principale et suspend le jeu.</td>
</tr>
<tr class="even">
<td align="left">OnWindowSizeChanged</td>
<td align="left">Gère <a href="/uwp/api/windows.ui.core.corewindow.sizechanged"><strong>CoreWindow :: SizeChanged</strong></a>. L’application de jeu réaffecte les ressources graphiques et la superposition pour tenir compte de la modification de la taille, puis met à jour la cible de rendu.</td>
</tr>
</tbody>
</table>

## <a name="next-steps"></a>Étapes suivantes

Dans cette rubrique, nous avons vu comment le circuit de jeu global est géré à l’aide des États de jeu et qu’un jeu est composé de plusieurs machines d’État différentes. Nous avons également vu comment mettre à jour l’interface utilisateur et gérer les gestionnaires d’événements d’application clés. Nous sommes maintenant prêts à plonger dans la boucle de rendu, le jeu et son mécanisme.
 
Vous pouvez parcourir les autres rubriques qui documentent ce jeu dans n’importe quel ordre.

- [Définir l’objet jeu principal](tutorial--defining-the-main-game-loop.md)
- [Infrastructure de rendu I : présentation du rendu](tutorial--assembling-the-rendering-pipeline.md)
- [Infrastructure de rendu II : rendu de jeu](tutorial-game-rendering.md)
- [Ajouter une interface utilisateur](tutorial--adding-a-user-interface.md)
- [Ajouter des contrôles](tutorial--adding-controls.md)
- [Ajouter du son](tutorial--adding-sound.md)
