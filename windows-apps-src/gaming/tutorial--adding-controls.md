---
title: Ajouter des contrôles
description: À présent, nous voyons comment l’exemple de jeu implémente les contrôles de déplacement et d’apparence dans un jeu 3D et comment développer des contrôles tactiles, de souris et de contrôleurs de jeu de base.
ms.assetid: f9666abb-151a-74b4-ae0b-ef88f1f252f8
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, UWP, jeux, contrôles, entrée
ms.localizationpriority: medium
ms.openlocfilehash: dfe864f0b8c16cce9cc8d413c41a4e3324cf2e9b
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409658"
---
# <a name="add-controls"></a>Ajouter des contrôles

> [!NOTE]
> Cette rubrique fait partie de la série de didacticiels [créer un jeu de plateforme Windows universelle simple (UWP) avec DirectX](tutorial--create-your-first-uwp-directx-game.md) . La rubrique de ce lien définit le contexte de la série.

\[Mise à jour pour les applications UWP sur Windows 10. Pour les articles Windows 8. x, consultez l' [Archive](/previous-versions/windows/apps/mt244353(v=win.10)?redirectedfrom=MSDN)\]

Un bon jeu de plateforme Windows universelle (UWP) prend en charge un large éventail d’interfaces. Un joueur potentiel peut avoir Windows 10 sur une tablette sans boutons physiques, un PC équipé d’un contrôleur Xbox ou la dernière plate-forme Desktop Gaming, avec une souris et un clavier de jeu hautes performances. Dans notre jeu, les contrôles sont implémentés dans la classe [**MoveLookController**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp) . Cette classe agrège les trois types d’entrée (souris et clavier, Touch et manette) en un seul contrôleur. Le résultat final est un tir de première personne qui utilise des contrôles de déplacement standard de genre qui fonctionnent avec plusieurs appareils.

> [!NOTE]
> Pour plus d’informations sur les contrôles, consultez [contrôles Move-look pour les jeux](tutorial--adding-move-look-controls-to-your-directx-game.md) et les [contrôles tactiles pour les jeux](tutorial--adding-touch-controls-to-your-directx-game.md).


## <a name="objective"></a>Objectif

À ce stade, nous avons un jeu qui s’affiche, mais nous ne pouvons pas faire tourner le joueur ou faire tourner les cibles. Nous allons voir comment notre jeu implémente les contrôles de déplacement de la première personne pour les types d’entrée suivants dans notre jeu DirectX UWP.
- Souris et clavier
- Entrées tactiles
- Boîtier de commande

>[!Note]
>Si vous n’avez pas téléchargé le dernier code de jeu pour cet exemple, accédez à l' [exemple de jeu Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Cet exemple fait partie d’une grande collection d’exemples de fonctionnalités UWP. Pour obtenir des instructions sur le téléchargement de l’exemple, consultez [obtenir les exemples UWP à partir de GitHub](/windows/uwp/get-started/get-uwp-app-samples).

## <a name="common-control-behaviors"></a>Comportements de contrôles communs


Les contrôles tactiles et les contrôles de souris/clavier ont une implémentation de base très semblable. Dans une application UWP, un pointeur est simplement un point sur l’écran. Vous pouvez le déplacer en faisant glisser la souris ou votre doigt sur l’écran tactile. Par conséquent, vous pouvez opter pour un seul ensemble d’événements sans vous demander si le joueur utilise une souris ou un écran tactile pour déplacer le pointeur et appuyer dessus.

Lorsque la classe **MoveLookController** dans l’exemple de jeu est initialisée, elle s’inscrit pour quatre événements spécifiques au pointeur et un événement spécifique à la souris :

Événement | Description
:------ | :-------
[**CoreWindow ::P ointerPressed**](/uwp/api/windows.ui.core.corewindow.pointerpressed) | Le bouton gauche ou droit de la souris a été enfoncé (et maintenu), ou la surface tactile a été touchée.
[**CoreWindow ::P ointerMoved**](/uwp/api/windows.ui.core.corewindow.pointermoved) |La souris a été déplacée, ou une action de déplacement a été effectuée sur la surface tactile.
[**CoreWindow ::P ointerReleased**](/uwp/api/windows.ui.core.corewindow.pointerreleased) |Le bouton gauche de la souris a été relâché, ou l’objet au contact de la surface tactile a été retiré.
[**CoreWindow ::P ointerExited**](/uwp/api/windows.ui.core.corewindow.pointerexited) |Le pointeur est sorti de la fenêtre principale.
[**Windows ::D evices :: Input :: MouseMoved**](/uwp/api/windows.devices.input.mousedevice.mousemoved) | La souris a parcouru une certaine distance. N’oubliez pas que nous sommes uniquement intéressés par les valeurs Delta de déplacement de la souris, et non par la position X-Y actuelle.


Ces gestionnaires d’événements sont configurés pour démarrer l’écoute des entrées d’utilisateur dès que le **MoveLookController** est initialisé dans la fenêtre d’application.
```cppwinrt
void MoveLookController::InitWindow(_In_ CoreWindow const& window)
{
    ResetState();

    window.PointerPressed({ this, &MoveLookController::OnPointerPressed });

    window.PointerMoved({ this, &MoveLookController::OnPointerMoved });

    window.PointerReleased({ this, &MoveLookController::OnPointerReleased });

    window.PointerExited({ this, &MoveLookController::OnPointerExited });

    ...

    // There is a separate handler for mouse-only relative mouse movement events.
    MouseDevice::GetForCurrentView().MouseMoved({ this, &MoveLookController::OnMouseMoved });

    ...
}
```

Le code complet pour [**InitWindow**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L68-L92) peut être consulté sur GitHub.


Pour déterminer quand le jeu doit écouter certaines entrées, la classe **MoveLookController** a trois États spécifiques au contrôleur, quel que soit le type de contrôleur :

State | Description
:----- | :-------
**Aucun** | Il s’agit de l’état initialisé pour la manette. Toutes les entrées sont ignorées, car le jeu n’anticipe aucune entrée de contrôleur.
**WaitForInput** | Le contrôleur attend que le lecteur accuse réception d’un message du jeu en utilisant un clic gauche de la souris, un événement tactile, à l’aide du bouton de menu sur un boîtier de commande.
**Actif** | Le contrôleur est en mode de lecture de jeu actif.



### <a name="waitforinput-state-and-pausing-the-game"></a>État de WaitForInput et suspension du jeu

Le jeu passe à l’état **WaitForInput** quand le jeu a été suspendu. Cela se produit lorsque le joueur déplace le pointeur à l’extérieur de la fenêtre principale du jeu, ou appuie sur le bouton pause (touche P ou bouton **Démarrer** du boîtier). Le **MoveLookController** inscrit la presse et informe la boucle du jeu lorsqu’il appelle la méthode [**IsPauseRequested**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L107-L127) . À ce stade, si **IsPauseRequested** retourne la **valeur true**, la boucle de jeu appelle **WaitForPress** sur le **MoveLookController** pour déplacer le contrôleur dans l’état **WaitForInput** . 


Une fois dans l’état **WaitForInput** , le jeu cesse de traiter presque tous les événements d’entrée de jeu jusqu’à ce qu’il retourne à l’état **actif** . L’exception est le bouton pause, avec une pression pour que le jeu revient à l’état actif. En dehors du bouton pause, pour que le jeu repasse à l’état **actif** , le joueur doit sélectionner un élément de menu. 



### <a name="the-active-state"></a>État actif

Dans l’état **actif** , l’instance **MoveLookController** traite les événements de tous les périphériques d’entrée activés et interprète les intentions du joueur. Par conséquent, il met à jour la rapidité et l’apparence de la vue du joueur et partage les données mises à jour avec le jeu après que la **mise à jour** est appelée à partir de la boucle du jeu.


Toutes les entrées de pointeur sont suivies dans l’état **actif** , avec différents ID de pointeur correspondant à différentes actions de pointeur.
Lorsqu’un événement [**PointerPressed**](/uwp/api/windows.ui.core.corewindow.pointerpressed) est reçu, **MoveLookController** obtient la valeur d’ID de pointeur créée par la fenêtre. L’ID de pointeur représente un type spécifique d’entrée. Par exemple, sur un périphérique tactile multipoint, plusieurs entrées actives différentes peuvent exister en même temps. Les ID permettent d’assurer le suivi de l’entrée utilisée par le joueur. Si un événement se trouve dans le rectangle de déplacement de l’écran tactile, un ID de pointeur est assigné pour effectuer le suivi des événements de pointeur dans le rectangle de déplacement. D’autres événements de pointeur dans le rectangle de tir sont suivis séparément, avec un ID de pointeur distinct.


> [!NOTE]
> Les entrées de la souris et du joystick droit d’un boîtier de soucodeur ont également des ID qui sont gérés séparément.

Une fois que les événements de pointeur ont été mappés sur une action de jeu spécifique, il est temps de mettre à jour les données que l’objet **MoveLookController** partage avec la boucle de jeu principale.

Quand elle est appelée, la méthode de [**mise à jour**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1005-L1096) dans l’exemple de jeu traite l’entrée et met à jour les variables de vitesse et de sens de recherche (**m \_ Velocity** et **m \_ LookDirection**), que la boucle de jeu récupère en appelant les méthodes [**Velocity**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L906-L909) public et [**LookDirection**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L913-L923) .

> [!NOTE]
> Pour plus d’informations sur la méthode de [**mise à jour**](#the-update-method) , voir plus loin dans cette page.




La boucle de jeu peut tester si le joueur tire en appelant la méthode **IsFiring** sur l’instance **MoveLookController**. L’instance **MoveLookController** vérifie si le joueur a appuyé sur le bouton de tir de l’un des trois types d’entrées.

```cppwinrt
bool MoveLookController::IsFiring()
{
    if (m_state == MoveLookControllerState::Active)
    {
        if (m_autoFire)
        {
            return (m_fireInUse || (m_mouseInUse && m_mouseLeftInUse) || PollingFireInUse());
        }
        else
        {
            if (m_firePressed)
            {
                m_firePressed = false;
                return true;
            }
        }
    }
    return false;
}
```

Examinons maintenant l’implémentation de chacun des trois types de contrôle d’un peu plus en détail.

## <a name="adding-relative-mouse-controls"></a>Ajout de contrôles Mouse relatifs


Si le déplacement de la souris est détecté, nous voulons utiliser ce mouvement pour déterminer le nouveau pas et le lacet de l’appareil photo. Nous y parvenons en implémentant les contrôles de souris relatifs, avec lesquels nous gérons la distance relative parcourue par la souris (écart entre le début et la fin du déplacement) par opposition à l’enregistrement des coordonnées des pixels x-y absolues du mouvement.

Pour ce faire, nous obtenons les modifications dans X (déplacement horizontal) et les coordonnées Y (déplacement vertical) en examinant les champs [**MouseDelta::X**](/uwp/api/Windows.Devices.Input.MouseDelta) et **MouseDelta::Y** sur l’objet argument [**Windows::Device::Input::MouseEventArgs::MouseDelta**](/uwp/api/windows.devices.input.mouseeventargs.mousedelta) renvoyé par l’événement [**MouseMoved**](/uwp/api/windows.devices.input.mousedevice.mousemoved).

```cppwinrt
void MoveLookController::OnMouseMoved(
    _In_ MouseDevice const& /* mouseDevice */,
    _In_ MouseEventArgs const& args
    )
{
    // Handle Mouse Input via dedicated relative movement handler.

    switch (m_state)
    {
    case MoveLookControllerState::Active:
        XMFLOAT2 mouseDelta;
        mouseDelta.x = static_cast<float>(args.MouseDelta().X);
        mouseDelta.y = static_cast<float>(args.MouseDelta().Y);

        XMFLOAT2 rotationDelta;
        // Scale for control sensitivity.
        rotationDelta.x = mouseDelta.x * MoveLookConstants::RotationGain;
        rotationDelta.y = mouseDelta.y * MoveLookConstants::RotationGain;

        // Update our orientation based on the command.
        m_pitch -= rotationDelta.y;
        m_yaw += rotationDelta.x;

        // Limit pitch to straight up or straight down.
        float limit = XM_PI / 2.0f - 0.01f;
        m_pitch = __max(-limit, m_pitch);
        m_pitch = __min(+limit, m_pitch);

        // Keep longitude in sane range by wrapping.
        if (m_yaw > XM_PI)
        {
            m_yaw -= XM_PI * 2.0f;
        }
        else if (m_yaw < -XM_PI)
        {
            m_yaw += XM_PI * 2.0f;
        }
        break;
    }
}
```

## <a name="adding-touch-support"></a>Ajout de la prise en charge tactile

Les contrôles tactiles sont très utiles pour la prise en charge des utilisateurs avec tablettes. Ce jeu rassemble les entrées tactiles en désegmentant certaines zones de l’écran, en les alignant sur des actions spécifiques dans le jeu.
L’entrée tactile de ce jeu utilise trois zones.

![déplacer la disposition Touch look](images/simple-dx-game-controls-touchzones.png)

Les commandes suivantes résument le comportement de notre contrôle tactile.
Entrée utilisateur | Action
:------- | :--------
Déplacer le rectangle | L’entrée tactile est convertie en manette virtuelle où le mouvement vertical est traduit en mouvement de position vers l’avant/vers l’arrière et le mouvement horizontal est traduit en mouvement de position gauche/droite.
Rectangle d’incendie | Déclencher une sphère.
Toucher en dehors du rectangle de déplacement et du feu | Modifiez la rotation (le tangage et le lacet) de la vue caméra.

**MoveLookController** vérifie l’ID de pointeur pour déterminer où l’événement s’est produit, et entreprend l’une des actions suivantes :

-   Si l’événement [**PointerMoved**](/uwp/api/windows.ui.core.corewindow.pointermoved) s’est produit dans le rectangle de déplacement ou de tir, la position du pointeur est mise à jour pour la manette.
-   Si l’événement [**PointerMoved**](/uwp/api/windows.ui.core.corewindow.pointermoved) se produit quelque part dans le reste de l’écran (défini en tant que contrôles look), calculez la modification de la hauteur et du lacets du vecteur de sens de la recherche.


Une fois que nous avons implémenté nos contrôles tactiles, les rectangles que nous avons dessinés précédemment à l’aide de Direct2D indiquent aux joueurs où se trouvent les zones de déplacement, d’incendie et de recherche.


![contrôles tactiles](images/simple-dx-game-controls-showzones.png)


Jetons maintenant un coup d’œil sur la façon dont nous implémentons chaque contrôle.


### <a name="move-and-fire-controller"></a>Déplacer et déclencher le contrôleur
Le rectangle de déplacement du contrôleur dans le quadrant inférieur gauche de l’écran est utilisé comme un pavé directionnel. Le fait de faire glisser le curseur vers la gauche et vers la droite dans cet espace déplace le joueur vers la gauche et la droite, tandis que monter et descendre déplace l’appareil photo vers l’avant et vers l’arrière
Après l’avoir configuré, l’activation du contrôleur Fire dans le coin inférieur droit de l’écran déclenche une sphère.

Les méthodes [**SetMoveRect**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L843-L853) et [**SetFireRect**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L857-L867) créent nos rectangles d’entrée, en prenant deux vecteurs 2D pour spécifier les positions des angles supérieur gauche et inférieur droit de chaque rectangle sur l’écran. 


Les paramètres sont ensuite attribués à **m \_ fireUpperLeft** et **m \_ fireLowerRight** qui nous aident à déterminer si l’utilisateur se touche dans les rectangles. 
```cppwinrt
m_fireUpperLeft = upperLeft;
m_fireLowerRight = lowerRight;
```

Si l’écran est redimensionné, ces rectangles sont redessinés à la taille approperiate.

Maintenant que nous avons divisé nos contrôles, il est temps de déterminer quand un utilisateur les utilise réellement.
Pour ce faire, nous avons configuré certains gestionnaires d’événements dans la méthode **MoveLookController :: InitWindow** lorsque l’utilisateur appuie sur le pointeur, le déplace ou le relâche.

```cppwinrt
window.PointerPressed({ this, &MoveLookController::OnPointerPressed });

window.PointerMoved({ this, &MoveLookController::OnPointerMoved });

window.PointerReleased({ this, &MoveLookController::OnPointerReleased });
```

Nous allons commencer par déterminer ce qui se passe quand l’utilisateur appuie sur les rectangles de déplacement ou d’incendie pour la première fois à l’aide de la méthode [**OnPointerPressed**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L179-L313) .
Ici, nous vérifions où ils touchent un contrôle et si un pointeur se trouve déjà dans ce contrôleur. S’il s’agit du premier doigt pour toucher le contrôle spécifique, nous procédons comme suit.
- Stockez l’emplacement de l’appel dans **m \_ moveFirstDown** ou **m \_ FireFirstDown** en tant que vecteur 2D.
- Affectez l’ID de pointeur à **m \_ movePointerID** ou **m \_ firePointerID**.
- Définissez l’indicateur **Inuse** approprié (**m \_ moveInUse** ou **m \_ fireInUse**) sur, `true` car nous avons maintenant un pointeur actif pour ce contrôle.

```cppwinrt
PointerPoint point = args.CurrentPoint();
uint32_t pointerID = point.PointerId();
Point pointerPosition = point.Position();
PointerPointProperties pointProperties = point.Properties();
auto pointerDevice = point.PointerDevice();
auto pointerDeviceType = pointerDevice.PointerDeviceType();

XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);

...
case MoveLookControllerState::Active:
    switch (pointerDeviceType)
    {
    case winrt::Windows::Devices::Input::PointerDeviceType::Touch:
        // Check to see if this pointer is in the move control.
        if (position.x > m_moveUpperLeft.x &&
            position.x < m_moveLowerRight.x &&
            position.y > m_moveUpperLeft.y &&
            position.y < m_moveLowerRight.y)
        {
            // If no pointer is in this control yet.
            if (!m_moveInUse)
            {
                // Process a DPad touch down event.
                // Save the location of the initial contact
                m_moveFirstDown = position;
                // Store the pointer using this control
                m_movePointerID = pointerID;
                // Set InUse flag to signal there is an active move pointer
                m_moveInUse = true;
            }
        }
        // Check to see if this pointer is in the fire control.
        else if (position.x > m_fireUpperLeft.x &&
            position.x < m_fireLowerRight.x &&
            position.y > m_fireUpperLeft.y &&
            position.y < m_fireLowerRight.y)
        {
            if (!m_fireInUse)
            {
                // Save the location of the initial contact
                m_fireLastPoint = position;
                // Store the pointer using this control
                m_firePointerID = pointerID;
                // Set InUse flag to signal there is an active fire pointer
                m_fireInUse = true;
                ...
            }
        }
        ...
```

Maintenant que nous avons déterminé si l’utilisateur touche un contrôle Move ou Fire, nous voyons si le joueur effectue des mouvements avec son doigt appuyé.
À l’aide de la méthode [**MoveLookController :: OnPointerMoved**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L317-L395) , nous vérifions le pointeur déplacé, puis nous stockons sa nouvelle position en tant que vecteur 2D.  

```cppwinrt
PointerPoint point = args.CurrentPoint();
uint32_t pointerID = point.PointerId();
Point pointerPosition = point.Position();
PointerPointProperties pointProperties = point.Properties();
auto pointerDevice = point.PointerDevice();

// convert to allow math
XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);

switch (m_state)
{
case MoveLookControllerState::Active:
    // Decide which control this pointer is operating.

    // Move control
    if (pointerID == m_movePointerID)
    {
        // Save the current position.
        m_movePointerPosition = position;
    }
    // Look control
    else if (pointerID == m_lookPointerID)
    {
        ...
    }
    // Fire control
    else if (pointerID == m_firePointerID)
    {
        m_fireLastPoint = position;
    }
    ...
```

Une fois que l’utilisateur a effectué ses mouvements dans les contrôles, il libère le pointeur. À l’aide de la méthode [**MoveLookController :: OnPointerReleased**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L441-L500) , nous déterminons quel pointeur a été relâché et effectuons une série de réinitialisations.


Si le contrôle Move a été relâché, nous procédons comme suit.
- Définissez la vélocité du joueur sur `0` dans toutes les directions pour les empêcher de se déplacer dans le jeu.
- Basculez **m \_ moveInUse** vers `false` , car l’utilisateur n’est plus en contact avec le contrôleur de déplacement.
- Définissez l’ID du pointeur de déplacement sur `0` , car il n’y a plus de pointeur dans le contrôleur de déplacement.

```cppwinrt
if (pointerID == m_movePointerID)
{
    // Stop on release.
    m_velocity = XMFLOAT3(0, 0, 0);
    m_moveInUse = false;
    m_movePointerID = 0;
}
```

Pour le contrôle Fire, s’il a été libéré, il suffit de basculer l’indicateur **m_fireInUse** sur `false` et l’ID du pointeur Fire sur, `0` car il n’y a plus de pointeur dans le contrôle Fire.
```cppwinrt
else if (pointerID == m_firePointerID)
{
    m_fireInUse = false;
    m_firePointerID = 0;
}
```

### <a name="look-controller"></a>Contrôleur de recherche
Nous traitons les événements de pointeur d’appareil tactile pour les régions inutilisées de l’écran en tant que contrôleur de présentation. Le fait de faire glisser votre doigt autour de cette zone modifie le tangage et le lacet (rotation) de l’appareil photo Player.

Si l’événement **MoveLookController :: OnPointerPressed** est déclenché sur un appareil tactile dans cette région et que l’état du jeu est défini sur **actif**, il reçoit un ID de pointeur.

```cppwinrt
// If no pointer is in this control yet.
if (!m_lookInUse)
{
    // Save point for later move.
    m_lookLastPoint = position;
    // Store the pointer using this control.
    m_lookPointerID = pointerID;
    // These are for smoothing.
    m_lookLastDelta.x = m_lookLastDelta.y = 0;
    m_lookInUse = true;
}
```

Ici, **MoveLookController** assigne l’ID de pointeur pour le pointeur qui a déclenché l’événement à une variable spécifique qui correspond à la zone de recherche. Dans le cas d’une pression tactile dans la région de recherche, la variable **m \_ lookPointerID** est définie sur l’ID de pointeur qui a déclenché l’événement. Une variable booléenne, **m \_ lookInUse**, est également définie pour indiquer que le contrôle n’a pas encore été libéré.

À présent, examinons comment l’exemple de jeu gère l’événement [**PointerMoved**](/uwp/api/windows.ui.core.corewindow.pointermoved) Touch écran.

Dans la méthode **MoveLookController :: OnPointerMoved** , nous vérifions le type d’ID de pointeur qui a été assigné à l’événement. S’il est **m_lookPointerID**, nous calculons la modification de la position du pointeur.
Nous utilisons ensuite ce Delta pour calculer le degré de modification de la rotation. Enfin, nous sommes à l’endroit où nous pouvons mettre à jour le ** \_ tangage m** et le ** \_ lacet m** à utiliser dans le jeu pour modifier la rotation du joueur. 

```cppwinrt
// This is the look pointer.
else if (pointerID == m_lookPointerID)
{
    // Look control.
    XMFLOAT2 pointerDelta;
    // How far did the pointer move?
    pointerDelta.x = position.x - m_lookLastPoint.x;
    pointerDelta.y = position.y - m_lookLastPoint.y;

    XMFLOAT2 rotationDelta;
    // Scale for control sensitivity.
    rotationDelta.x = pointerDelta.x * MoveLookConstants::RotationGain;
    rotationDelta.y = pointerDelta.y * MoveLookConstants::RotationGain;
    // Save for next time through.
    m_lookLastPoint = position;

    // Update our orientation based on the command.
    m_pitch -= rotationDelta.y;
    m_yaw += rotationDelta.x;

    // Limit pitch to straight up or straight down.
    float limit = XM_PI / 2.0f - 0.01f;
    m_pitch = __max(-limit, m_pitch);
    m_pitch = __min(+limit, m_pitch);
    ...
}
```

La dernière partie que nous allons examiner est la manière dont l’exemple de jeu gère l’événement [**PointerReleased**](/uwp/api/windows.ui.core.corewindow.pointerreleased) Touch écran.
Une fois que l’utilisateur a terminé le mouvement tactile et supprimé son doigt de l’écran, [**MoveLookController :: OnPointerReleased**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L441-L500) est initié.
Si l’ID du pointeur qui a déclenché l’événement [**PointerReleased**](/uwp/api/windows.ui.core.corewindow.pointerreleased) est l’ID du pointeur de déplacement enregistré précédemment, **MoveLookController** définit la vélocité sur `0` parce que le joueur a cessé de toucher la zone d’apparence.

```cppwinrt
else if (pointerID == m_lookPointerID)
{
    m_lookInUse = false;
    m_lookPointerID = 0;
}
```

## <a name="adding-mouse-and-keyboard-support"></a>Ajout de la prise en charge de la souris et du clavier

Ce jeu possède la disposition de contrôle suivante pour le clavier et la souris.

Entrée utilisateur | Action
:------- | :--------
W | Déplacer le lecteur vers l’avant
Un | Déplacer le joueur vers la gauche
S | Déplacer le joueur vers l’arrière
D | Déplacer le joueur vers la droite
X | Déplacer l’affichage vers le haut
Barre d’espace | Déplacer l’affichage vers le dessous
P | Interrompre le jeu
Mouvement de la souris | Modifier la rotation (la tonalité et le lacet) de la vue caméra
Bouton gauche de la souris | Déclencher une sphère


Pour utiliser le clavier, l’exemple de jeu inscrit deux nouveaux événements, [**CoreWindow :: KeyUp**](/uwp/api/windows.ui.core.corewindow.keyup) et [**CoreWindow :: keyvers**](/uwp/api/windows.ui.core.corewindow.keydown)le début, dans la méthode [**MoveLookController :: InitWindow**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L84-L88) . Ces événements gèrent la pression et la libération d’une clé.

```cppwinrt
window.KeyDown({ this, &MoveLookController::OnKeyDown });

window.KeyUp({ this, &MoveLookController::OnKeyUp });
```

La souris est traitée un peu différemment des contrôles tactiles, même s’il utilise un pointeur. Pour s’aligner sur la disposition de votre contrôle, le **MoveLookController** fait pivoter l’appareil photo chaque fois que la souris est déplacée et se déclenche lorsque le bouton gauche de la souris est enfoncé.

Cela est géré dans la méthode [**OnPointerPressed**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L179-L313) de **MoveLookController**.

Dans cette méthode, nous vérifions le type de périphérique de pointeur utilisé avec l' [`Windows::Devices::Input::PointerDeviceType`](/uwp/api/Windows.Devices.Input.PointerDeviceType) énumération. Si le jeu est **actif** et que le **PointerDeviceType** n’est pas **Touch**, nous supposons qu’il s’agit de l’entrée de la souris.

```cppwinrt
case MoveLookControllerState::Active:
    switch (pointerDeviceType)
    {
    case winrt::Windows::Devices::Input::PointerDeviceType::Touch:
        // Behavior for touch controls
        ...

    default:
        // Behavior for mouse controls
        bool rightButton = pointProperties.IsRightButtonPressed();
        bool leftButton = pointProperties.IsLeftButtonPressed();

        if (!m_autoFire && (!m_mouseLeftInUse && leftButton))
        {
            m_firePressed = true;
        }

        if (!m_mouseInUse)
        {
            m_mouseInUse = true;
            m_mouseLastPoint = position;
            m_mousePointerID = pointerID;
            m_mouseLeftInUse = leftButton;
            m_mouseRightInUse = rightButton;
            // These are for smoothing.
            m_lookLastDelta.x = m_lookLastDelta.y = 0;
        }
        break;
    }
    break;
```

Lorsque le joueur arrête d’appuyer sur l’un des boutons de la souris, l’événement de souris [CoreWindow ::P ointerreleased](/uwp/api/Windows.UI.Core.CoreWindow.PointerReleased) est déclenché, en appelant la méthode [MoveLookController :: OnPointerReleased](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L441-L500) et l’entrée est terminée. À ce stade, les sphères cessent de se déclencher si le bouton gauche de la souris était enfoncé et est maintenant relâché. Comme look est toujours activé, le jeu continue d’utiliser le même pointeur de souris pour suivre les événements d’apparence en cours.

```cppwinrt
case MoveLookControllerState::Active:
    // Touch points
    if (pointerID == m_movePointerID)
    {
        // Stop movement
        ...
    }
    else if (pointerID == m_lookPointerID)
    {
        // Stop look rotation
        ...
    }
    // Fire button has been released
    else if (pointerID == m_firePointerID)
    {
        // Stop firing
        ...
    }
    // Mouse point
    else if (pointerID == m_mousePointerID)
    {
        bool rightButton = pointProperties.IsRightButtonPressed();
        bool leftButton = pointProperties.IsLeftButtonPressed();

        // Mouse no longer in use so stop firing
        m_mouseInUse = false;

        // Don't clear the mouse pointer ID so that Move events still result in Look changes.
        // m_mousePointerID = 0;
        m_mouseLeftInUse = leftButton;
        m_mouseRightInUse = rightButton;
    }
    break;
```

Examinons maintenant le dernier type de contrôle que nous allons prendre en charge : les boîtiers de commande. Les boîtiers de commande sont gérés séparément des contrôles tactiles et de la souris, car ils n’utilisent pas l’objet pointeur. Pour cette raison, quelques nouveaux gestionnaires d’événements et méthodes devront être ajoutés.

## <a name="adding-gamepad-support"></a>Ajout de la prise en charge de la manette de jeu

Pour ce jeu, la prise en charge du jeu de boîtiers est ajoutée par les appels aux API [Windows. Gaming. Input](/uwp/api/windows.gaming.input) . Cet ensemble d’API permet d’accéder à des entrées de contrôleur de jeu, comme des roues de course et des bâtons de vol. 

Les éléments suivants sont nos contrôles de manette de commande.

Entrée utilisateur | Action
:------- | :--------
Stick analogique gauche | Déplacer le joueur
Stick analogique droite | Modifier la rotation (la tonalité et le lacet) de la vue caméra
Gâchette droite | Déclencher une sphère
Bouton démarrer/menu | Suspendre ou reprendre le jeu

Dans la méthode [**InitWindow**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L68-L103) , nous ajoutons deux nouveaux événements pour déterminer si un boîtier de soucoursment a été [ajouté](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1100-L1105) ou [supprimé](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1109-L1114). Ces événements mettent à jour la propriété **m_gamepadsChanged** . Ce nom est utilisé dans la méthode **UpdatePollingDevices** pour vérifier si la liste des boîtiers de la liste a été modifiée. 

```cppwinrt
// Detect gamepad connection and disconnection events.
Gamepad::GamepadAdded({ this, &MoveLookController::OnGamepadAdded });

Gamepad::GamepadRemoved({ this, &MoveLookController::OnGamepadRemoved });
```

> [!NOTE]
> Les applications UWP ne peuvent pas recevoir d’entrée d’un contrôleur Xbox 1 alors que l’application n’est pas active.

### <a name="the-updatepollingdevices-method"></a>Méthode UpdatePollingDevices

La méthode [**UpdatePollingDevices**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L654-L782) de l’instance **MoveLookController** vérifie immédiatement si un boîtier de connexion est connecté. Si c’est le cas, nous commencerons à lire son état avec le [**boîtier de GetCurrentReading**](/uwp/api/windows.gaming.input.gamepad.GetCurrentReading). Cela retourne le struct [**GamepadReading**](/uwp/api/Windows.Gaming.Input.GamepadReading) , ce qui nous permet de vérifier les boutons sur lesquels un clic a été effectué ou le déplacement du Thumbsticks.

Si l’état du jeu est **WaitForInput**, nous écoutons uniquement le bouton démarrer/menu du contrôleur afin que le jeu puisse reprendre.

S’il est **actif**, nous vérifions l’entrée de l’utilisateur et déterminons l’action dans le jeu qui doit se produire.
Par exemple, si l’utilisateur a déplacé le stick analogique gauche dans une direction spécifique, cela permet au jeu de savoir que nous devons déplacer le joueur dans le sens où le levier est déplacé. Le mouvement du bâton dans une direction spécifique doit s’inscrire plus grand que le rayon de la **zone morte**; dans le cas contraire, rien ne se produira. Ce rayon de zone morte est nécessaire pour empêcher la « dérive », ce qui est le cas lorsque le contrôleur choisit de petits mouvements à partir du pouce du joueur lorsqu’il se trouve sur le bâton. Sans zones mortes, les contrôles peuvent apparaître trop sensibles pour l’utilisateur.

L’entrée de stick analogique est comprise entre-1 et 1 pour l’axe des x et y. Le conseur ci-dessous spécifie le rayon de la zone de stick analogique mort.

```cppwinrt
#define THUMBSTICK_DEADZONE 0.25f
```

À l’aide de cette variable, nous allons commencer à traiter l’entrée de stick analogique à action. Le mouvement se produit avec une valeur de [-1,-.26] ou [. 26, 1] sur l’un des axes.

![zone morte pour Thumbsticks](images/simple-dx-game-controls-deadzone.png)

Cette partie de la méthode **UpdatePollingDevices** gère les Thumbsticks de gauche et de droite.
Les valeurs X et Y de chaque Stick sont vérifiées pour voir si elles se trouvent en dehors de la zone morte. Si l’une ou l’autre est, nous mettrons à jour le composant correspondant.
Par exemple, si le joystick gauche est déplacé vers la gauche le long de l’axe X, nous ajouterons-1 au composant **x** du vecteur **m_moveCommand** . Ce vecteur est ce qui sera utilisé pour regrouper tous les mouvements sur tous les appareils et sera utilisé ultérieurement pour calculer où le joueur doit se déplacer. 

```cppwinrt
// Use the left thumbstick to control the eye point position
// (position of the player).

// Check if left thumbstick is outside of dead zone on x axis
if (reading.LeftThumbstickX > THUMBSTICK_DEADZONE ||
    reading.LeftThumbstickX < -THUMBSTICK_DEADZONE)
{
    // Get value of left thumbstick's position on x axis
    float x = static_cast<float>(reading.LeftThumbstickX);
    // Set the x of the move vector to 1 if the stick is being moved right.
    // Set to -1 if moved left. 
    m_moveCommand.x -= (x > 0) ? 1 : -1;
}

// Check if left thumbstick is outside of dead zone on y axis
if (reading.LeftThumbstickY > THUMBSTICK_DEADZONE ||
    reading.LeftThumbstickY < -THUMBSTICK_DEADZONE)
{
    // Get value of left thumbstick's position on y axis
    float y = static_cast<float>(reading.LeftThumbstickY);
    // Set the y of the move vector to 1 if the stick is being moved forward.
    // Set to -1 if moved backwards.
    m_moveCommand.y += (y > 0) ? 1 : -1;
}
```

À l’instar de la façon dont le Stick de gauche contrôle le mouvement, le levier droit contrôle la rotation de l’appareil photo.

Le comportement de la barre de défilement droite s’aligne sur le comportement du mouvement de la souris dans la configuration de la souris et du clavier.
Si le Stick est en dehors de la zone morte, nous calculons la différence entre la position actuelle du pointeur et l’emplacement où l’utilisateur tente à présent de regarder.
Cette modification de la position du pointeur (**pointerDelta**) est ensuite utilisée pour mettre à jour le pas et le lacet de la rotation de l’appareil photo, qui sont ensuite appliqués dans notre méthode de **mise à jour** .
Le vecteur **pointerDelta** peut paraître familier, car il est également utilisé dans la méthode [MoveLookController :: OnPointerMoved](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L318-L395) pour effectuer le suivi de la modification de la position du pointeur pour nos entrées tactiles et de souris.

```cppwinrt
// Use the right thumbstick to control the look at position

XMFLOAT2 pointerDelta;

// Check if right thumbstick is outside of deadzone on x axis
if (reading.RightThumbstickX > THUMBSTICK_DEADZONE ||
    reading.RightThumbstickX < -THUMBSTICK_DEADZONE)
{
    float x = static_cast<float>(reading.RightThumbstickX);
    // Register the change in the pointer along the x axis
    pointerDelta.x = x * x * x;
}
// No actionable thumbstick movement. Register no change in pointer.
else
{
    pointerDelta.x = 0.0f;
}
// Check if right thumbstick is outside of deadzone on y axis
if (reading.RightThumbstickY > THUMBSTICK_DEADZONE ||
    reading.RightThumbstickY < -THUMBSTICK_DEADZONE)
{
    float y = static_cast<float>(reading.RightThumbstickY);
    // Register the change in the pointer along the y axis
    pointerDelta.y = y * y * y;
}
else
{
    pointerDelta.y = 0.0f;
}

XMFLOAT2 rotationDelta;
// Scale for control sensitivity.
rotationDelta.x = pointerDelta.x * 0.08f;
rotationDelta.y = pointerDelta.y * 0.08f;

// Update our orientation based on the command.
m_pitch += rotationDelta.y;
m_yaw += rotationDelta.x;

// Limit pitch to straight up or straight down.
m_pitch = __max(-XM_PI / 2.0f, m_pitch);
m_pitch = __min(+XM_PI / 2.0f, m_pitch);
```

Les contrôles du jeu ne sont pas complets sans la possibilité de déclencher des sphères !

Cette méthode **UpdatePollingDevices** vérifie également si le déclencheur approprié est activé. Si c’est le cas, la propriété **m_firePressed** est retournée à true, ce qui signale au jeu qu’il doit commencer à déclencher.
```cppwinrt
if (reading.RightTrigger > TRIGGER_DEADZONE)
{
    if (!m_autoFire && !m_gamepadTriggerInUse)
    {
        m_firePressed = true;
    }

    m_gamepadTriggerInUse = true;
}
else
{
    m_gamepadTriggerInUse = false;
}
```

## <a name="the-update-method"></a>Méthode Update

Pour encapsuler les choses, nous allons approfondir la méthode de [**mise à jour**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1005-L1096) .
Cette méthode fusionne les mouvements ou les rotations effectués par le joueur avec toute entrée prise en charge pour générer un vecteur de vélocité et mettre à jour nos valeurs de tangage et de lacet pour accéder à notre boucle de jeu.

La méthode **Update** lance des tâches en appelant [**UpdatePollingDevices**](#the-updatepollingdevices-method) pour mettre à jour l’état du contrôleur. Cette méthode collecte également toute entrée à partir d’un boîtier et ajoute ses mouvements au vecteur de **m_moveCommand** . 

Dans notre méthode de **mise à jour** , nous effectuons les vérifications d’entrée suivantes.
- Si le joueur utilise le rectangle déplacer le contrôleur, nous déterminerons ensuite la modification de la position du pointeur et nous l’utiliserons pour calculer si l’utilisateur a déplacé le pointeur hors de la zone morte du contrôleur. En dehors de la zone morte, la propriété Vector **m_moveCommand** est ensuite mise à jour avec la valeur de la manette de jeu virtuelle.
- Si l’une des entrées de clavier de déplacement est enfoncée, la valeur `1.0f` ou `-1.0f` est ajoutée dans le composant correspondant du vecteur **m_moveCommand** &mdash; `1.0f` pour l’avant et `-1.0f` pour l’arrière.

Une fois que toutes les entrées de mouvement ont été prises en compte, nous exécutons le vecteur de **m_moveCommand** par le biais de calculs pour générer un nouveau vecteur qui représente la direction du joueur en ce qui concerne le monde du jeu.
Nous prenons ensuite nos mouvements par rapport au monde et nous les appliquons au joueur en tant que rapidité dans cette direction.
Enfin, nous définissons le vecteur de **m_moveCommand** sur `(0.0f, 0.0f, 0.0f)` afin que tout soit prêt pour le cadre de jeu suivant.

```cppwinrt
void MoveLookController::Update()
{
    // Get any gamepad input and update state
    UpdatePollingDevices();

    if (m_moveInUse)
    {
        // Move control.
        XMFLOAT2 pointerDelta;

        pointerDelta.x = m_movePointerPosition.x - m_moveFirstDown.x;
        pointerDelta.y = m_movePointerPosition.y - m_moveFirstDown.y;

        // Figure out the command from the virtual joystick.
        XMFLOAT3 commandDirection = XMFLOAT3(0.0f, 0.0f, 0.0f);
        // Leave 32 pixel-wide dead spot for being still.
        if (fabsf(pointerDelta.x) > 16.0f)
            m_moveCommand.x -= pointerDelta.x / fabsf(pointerDelta.x);

        if (fabsf(pointerDelta.y) > 16.0f)
            m_moveCommand.y -= pointerDelta.y / fabsf(pointerDelta.y);
    }

    // Poll our state bits set by the keyboard input events.
    if (m_forward)
    {
        m_moveCommand.y += 1.0f;
    }
    if (m_back)
    {
        m_moveCommand.y -= 1.0f;
    }
    if (m_left)
    {
        m_moveCommand.x += 1.0f;
    }
    if (m_right)
    {
        m_moveCommand.x -= 1.0f;
    }
    if (m_up)
    {
        m_moveCommand.z += 1.0f;
    }
    if (m_down)
    {
        m_moveCommand.z -= 1.0f;
    }

    // Make sure that 45deg cases are not faster.
    if (fabsf(m_moveCommand.x) > 0.1f ||
        fabsf(m_moveCommand.y) > 0.1f ||
        fabsf(m_moveCommand.z) > 0.1f)
    {
        XMStoreFloat3(&m_moveCommand, XMVector3Normalize(XMLoadFloat3(&m_moveCommand)));
    }

    // Rotate command to align with our direction (world coordinates).
    XMFLOAT3 wCommand;
    wCommand.x = m_moveCommand.x * cosf(m_yaw) - m_moveCommand.y * sinf(m_yaw);
    wCommand.y = m_moveCommand.x * sinf(m_yaw) + m_moveCommand.y * cosf(m_yaw);
    wCommand.z = m_moveCommand.z;

    // Scale for sensitivity adjustment.
    // Our velocity is based on the command. Y is up.
    m_velocity.x = -wCommand.x * MoveLookConstants::MovementGain;
    m_velocity.z = wCommand.y * MoveLookConstants::MovementGain;
    m_velocity.y = wCommand.z * MoveLookConstants::MovementGain;

    // Clear movement input accumulator for use during next frame.
    m_moveCommand = XMFLOAT3(0.0f, 0.0f, 0.0f);
}
```

## <a name="next-steps"></a>Étapes suivantes

Maintenant que nous avons ajouté nos contrôles, vous devez ajouter une autre fonctionnalité pour créer un jeu immersif : le son !
Les effets sonores et musicaux sont importants pour n’importe quel jeu. nous allons donc aborder l' [Ajout de son](tutorial--adding-sound.md) suivant.
