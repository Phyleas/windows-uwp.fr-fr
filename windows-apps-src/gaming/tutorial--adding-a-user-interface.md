---
title: Ajouter une interface utilisateur
description: Découvrez comment utiliser les API Direct2D pour ajouter une superposition d’interface utilisateur 2D avec des menus d’affichage et d’état de jeu de haut en haut dans un jeu DirectX UWP.
ms.assetid: fa40173e-6cde-b71b-e307-db90f0388485
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, UWP, jeux, interface utilisateur, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: eca248887985fc6d33ca6d6b552a0b61a98ce428
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173103"
---
# <a name="add-a-user-interface"></a>Ajouter une interface utilisateur

> [!NOTE]
> Cette rubrique fait partie de la série de didacticiels [créer un jeu de plateforme Windows universelle simple (UWP) avec DirectX](tutorial--create-your-first-uwp-directx-game.md) . La rubrique de ce lien définit le contexte de la série.

Maintenant que nos visuels 3D sont en place, il est temps de se concentrer sur l’ajout de certains éléments 2D afin que le jeu puisse fournir des commentaires sur l’état du jeu au joueur. Pour ce faire, vous pouvez ajouter des options de menu simples et des composants d’affichage en haut de la sortie de pipeline graphique 3D.

>[!Note]
>Si vous n’avez pas téléchargé le dernier code de jeu pour cet exemple, accédez à l' [exemple de jeu Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Cet exemple fait partie d’une grande collection d’exemples de fonctionnalités UWP. Pour obtenir des instructions sur le téléchargement de l’exemple, consultez [obtenir les exemples UWP à partir de GitHub](../get-started/get-app-samples.md).

## <a name="objective"></a>Objectif

À l’aide de Direct2D, ajoutez un certain nombre de graphiques d’interface utilisateur et de comportements à notre jeu DirectX UWP, y compris :
- Affichage des têtes, y compris des rectangles de délimitation [de déplacement-regarder](tutorial--adding-controls.md)
- Menus de l’état du jeu


## <a name="the-user-interface-overlay"></a>Superposition de l’interface utilisateur


Bien qu’il existe de nombreuses façons d’afficher du texte et des éléments d’interface utilisateur dans un jeu DirectX, nous allons nous concentrer sur l’utilisation de [Direct2D](/windows/desktop/Direct2D/direct2d-portal). Nous utiliserons également [DirectWrite](/windows/desktop/DirectWrite/direct-write-portal) pour les éléments de texte.


Direct2D est un ensemble d’API de dessin 2D utilisé pour dessiner des primitives et des effets basés sur des pixels. Quand vous commencez avec Direct2D, il est préférable de garder les choses simples. Des comportements d’interface et dispositions complexes nécessitent du temps et une certaine planification. Si votre jeu requiert une interface utilisateur complexe, comme celles qui se trouvent dans les jeux de simulation et de stratégie, envisagez d’utiliser XAML à la place.

> [!NOTE]
> Pour plus d’informations sur le développement d’une interface utilisateur avec XAML dans un jeu DirectX UWP, consultez [extension de l’exemple de jeu](tutorial-resources.md).

Direct2D n’est pas spécifiquement conçu pour les interfaces utilisateur ou les dispositions telles que HTML et XAML. Elle ne fournit pas de composants d’interface utilisateur tels que des listes, des zones ou des boutons. Elle ne fournit pas non plus de composants de disposition tels que les divs, les tables ou les grilles.


Pour cet exemple de jeu, nous avons deux principaux composants de l’interface utilisateur.
1. Un affichage de tête pour le score et les contrôles in-game.
2. Superposition utilisée pour afficher le texte et les options de l’état du jeu, telles que les options de pause et les options de démarrage.

### <a name="using-direct2d-for-a-heads-up-display"></a>Utilisation de Direct2D pour l’affichage à tête haute

L’illustration suivante montre l’affichage des têtes dans le jeu pour l’exemple. C’est simple et sans encombrement, ce qui permet au joueur de se concentrer sur la navigation dans le monde 3D et les cibles de tir. Une bonne interface ou un affichage de tête haut ne doit jamais compliquer la capacité du joueur à traiter et à réagir aux événements du jeu.

![Capture d’écran de la superposition du jeu](images/simple-dx-game-ui-overlay.png)

La superposition se compose des primitives de base suivantes.
- Texte [**DirectWrite**](/windows/desktop/DirectWrite/direct-write-portal) dans l’angle supérieur droit qui informe le joueur de 
    - Accès réussis
    - Nombre de captures effectuées par le joueur
    - Temps restant dans le niveau
    - Numéro de niveau actuel 
- Deux segments de ligne d’intersection permettant de former un réticule
- Deux rectangles aux angles inférieurs pour le [contrôleur](tutorial--adding-controls.md) les limites. 


L’état d’affichage de la superposition dans le jeu est dessiné dans la méthode [**GameHud :: Render**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L234-L358) de la classe [**GameHud**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.h) . Dans cette méthode, la superposition Direct2D qui représente notre interface utilisateur est mise à jour pour refléter les modifications du nombre d’accès, du temps restant et du numéro de niveau.

Si le jeu a été initialisé, ajoutez `TotalHits()` , `TotalShots()` et `TimeRemaining()` à une mémoire tampon de [**swprintf_s**](/cpp/c-runtime-library/reference/sprintf-s-sprintf-s-l-swprintf-s-swprintf-s-l) et spécifiez le format d’impression. Nous pouvons ensuite la dessiner à l’aide de la méthode [**DrawText**](/windows/desktop/Direct2D/id2d1rendertarget-drawtext) . Nous faisons la même chose pour l’indicateur de niveau actuel, en dessinant des nombres vides pour afficher des niveaux non terminés comme ➀ et des nombres pleins comme ➊ pour montrer que le niveau spécifique est terminé.


L’extrait de code suivant parcourt le processus de la méthode **GameHud :: Render** pour 
- Création d’une image bitmap à l’aide de [* * ID2D1RenderTarget ::D rawbitmap * *](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-drawbitmap(id2d1bitmap_constd2d1_rect_f__float_d2d1_bitmap_interpolation_mode_constd2d1_rect_f_))
- Section pour désactiver les zones de l’interface utilisateur dans des rectangles à l’aide de [ **d2d1 :: RectF**](/windows/desktop/api/dcommon/ns-dcommon-d2d_rect_f)
- Utilisation de **DrawText** pour créer des éléments de texte

```cppwinrt
void GameHud::Render(_In_ std::shared_ptr<Simple3DGame> const& game)
{
    auto d2dContext = m_deviceResources->GetD2DDeviceContext();
    auto windowBounds = m_deviceResources->GetLogicalSize();

    if (m_showTitle)
    {
        d2dContext->DrawBitmap(
            m_logoBitmap.get(),
            D2D1::RectF(
                GameUIConstants::Margin,
                GameUIConstants::Margin,
                m_logoSize.width + GameUIConstants::Margin,
                m_logoSize.height + GameUIConstants::Margin
                )
            );
        d2dContext->DrawTextLayout(
            Point2F(m_logoSize.width + 2.0f * GameUIConstants::Margin, GameUIConstants::Margin),
            m_titleHeaderLayout.get(),
            m_textBrush.get()
            );
        d2dContext->DrawTextLayout(
            Point2F(GameUIConstants::Margin, m_titleBodyVerticalOffset),
            m_titleBodyLayout.get(),
            m_textBrush.get()
            );
    }

    // Draw text for number of hits, total shots, and time remaining
    if (game != nullptr)
    {
        // This section is only used after the game state has been initialized.
        static const int bufferLength = 256;
        static wchar_t wsbuffer[bufferLength];
        int length = swprintf_s(
            wsbuffer,
            bufferLength,
            L"Hits:\t%10d\nShots:\t%10d\nTime:\t%8.1f",
            game->TotalHits(),
            game->TotalShots(),
            game->TimeRemaining()
            );

        // Draw the upper right portion of the HUD displaying total hits, shots, and time remaining
        d2dContext->DrawText(
            wsbuffer,
            length,
            m_textFormatBody.get(),
            D2D1::RectF(
                windowBounds.Width - GameUIConstants::HudRightOffset,
                GameUIConstants::HudTopOffset,
                windowBounds.Width,
                GameUIConstants::HudTopOffset + (GameUIConstants::HudBodyPointSize + GameUIConstants::Margin) * 3
                ),
            m_textBrush.get()
            );

        // Using the unicode characters starting at 0x2780 ( ➀ ) for the consecutive levels of the game.
        // For completed levels start with 0x278A ( ➊ ) (This is 0x2780 + 10).
        uint32_t levelCharacter[6];
        for (uint32_t i = 0; i < 6; i++)
        {
            levelCharacter[i] = 0x2780 + i + ((static_cast<uint32_t>(game->LevelCompleted()) == i) ? 10 : 0);
        }
        length = swprintf_s(
            wsbuffer,
            bufferLength,
            L"%lc %lc %lc %lc %lc %lc",
            levelCharacter[0],
            levelCharacter[1],
            levelCharacter[2],
            levelCharacter[3],
            levelCharacter[4],
            levelCharacter[5]
            );
        // Create a new rectangle and draw the current level info text inside
        d2dContext->DrawText(
            wsbuffer,
            length,
            m_textFormatBodySymbol.get(),
            D2D1::RectF(
                windowBounds.Width - GameUIConstants::HudRightOffset,
                GameUIConstants::HudTopOffset + (GameUIConstants::HudBodyPointSize + GameUIConstants::Margin) * 3 + GameUIConstants::Margin,
                windowBounds.Width,
                GameUIConstants::HudTopOffset + (GameUIConstants::HudBodyPointSize + GameUIConstants::Margin) * 4
                ),
            m_textBrush.get()
            );

        if (game->IsActivePlay())
        {
            // Draw the move and fire rectangles
            ...
            // Draw the crosshairs
            ...
        }
    }
}
```

En décomposant davantage la méthode, cette partie de la méthode [**GameHud :: Render**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L320-L358) dessine nos rectangles de déplacement et d’incendie avec [**ID2D1RenderTarget ::D rawrectangle**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-drawrectangle(constd2d1_rect_f__id2d1brush_float_id2d1strokestyle))et le réticule à l’aide de deux appels à [**ID2D1RenderTarget ::D rawline**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-drawline).

```cppwinrt
// Check if game is playing
if (game->IsActivePlay())
{
    // Draw a rectangle for the touch input for the move control.
    d2dContext->DrawRectangle(
        D2D1::RectF(
            0.0f,
            windowBounds.Height - GameUIConstants::TouchRectangleSize,
            GameUIConstants::TouchRectangleSize,
            windowBounds.Height
            ),
        m_textBrush.get()
        );
    // Draw a rectangle for the touch input for the fire control.
    d2dContext->DrawRectangle(
        D2D1::RectF(
            windowBounds.Width - GameUIConstants::TouchRectangleSize,
            windowBounds.Height - GameUIConstants::TouchRectangleSize,
            windowBounds.Width,
            windowBounds.Height
            ),
        m_textBrush.get()
        );

    // Draw the cross hairs
    d2dContext->DrawLine(
        D2D1::Point2F(windowBounds.Width / 2.0f - GameUIConstants::CrossHairHalfSize,
            windowBounds.Height / 2.0f),
        D2D1::Point2F(windowBounds.Width / 2.0f + GameUIConstants::CrossHairHalfSize,
            windowBounds.Height / 2.0f),
        m_textBrush.get(),
        3.0f
        );
    d2dContext->DrawLine(
        D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f -
            GameUIConstants::CrossHairHalfSize),
        D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f +
            GameUIConstants::CrossHairHalfSize),
        m_textBrush.get(),
        3.0f
        );
}
```

Dans la méthode **GameHud :: Render** , nous stockons la taille logique de la fenêtre de jeu dans la `windowBounds` variable. La [`GetLogicalSize`](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.h#L41) méthode de la classe **DeviceResources** est utilisée. 

```cppwinrt
auto windowBounds = m_deviceResources->GetLogicalSize();
```

L’obtention de la taille de la fenêtre de jeu est essentielle pour la programmation de l’interface utilisateur. La taille de la fenêtre est donnée dans une mesure appelée DIP (Device Independent Pixel), où une DIP est définie comme 1/96 de pouce. Direct2D met à l’échelle les unités de dessin sur les pixels réels lorsque le dessin se produit, en utilisant le paramètre ppp (points par pouce) Windows. De même, lorsque vous dessinez du texte à l’aide de [**DirectWrite**](/windows/desktop/DirectWrite/direct-write-portal), vous spécifiez des DIP plutôt que des points pour la taille de la police. Les DIP sont exprimés sous la forme de nombres à virgule flottante. 

### <a name="displaying-game-state-info"></a>Affichage des informations sur l’état du jeu

Outre l’affichage des têtes, l’exemple de jeu présente un chevauchement qui représente six États de jeu. Tous les États comportent une primitive de rectangle noir de grande taille avec du texte à lire par le joueur. Les rectangles et les croix du contrôleur de déplacement-look ne sont pas dessinés, car ils ne sont pas actifs dans ces États.

La superposition est créée à l’aide de la classe [**GameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.h) , ce qui nous permet d’extraire le texte affiché pour l’aligner sur l’état du jeu.

![État et action de superposition](images/simple-dx-game-ui-finaloverlay.png)

La superposition est divisée en deux sections : **État** et **action**. Le Secton d' **État** est ensuite divisé en rectangles de **titre** et de **corps** . La section **action** n’a qu’un seul rectangle. Chaque rectangle a un objectif différent.

-   `titleRectangle` contient le texte du titre.
-   `bodyRectangle` contient le texte du corps.
-   `actionRectangle` contient le texte qui informe le joueur à prendre une mesure spécifique.

Le jeu comprend six États qui peuvent être définis. État du jeu acheminé à l’aide de la partie **État** de la superposition. Les rectangles d' **État** sont mis à jour à l’aide d’un certain nombre de méthodes correspondant aux États suivants.

- Chargement
- Statistiques de début/de score élevé
- Début du niveau
- Jeu suspendu
- Fin du jeu
- Jeu gagné


La partie **action** de la superposition est mise à jour à l’aide de la méthode [**GameInfoOverlay :: SetAction**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564) , ce qui permet de définir le texte d’action sur l’un des éléments suivants.
- « Appuyez pour rejouer... »
- « Niveau de chargement, veuillez patienter... »
- « Appuyez pour continuer... »
- Aucune

> [!NOTE]
> Ces deux méthodes seront abordées plus en détail dans la section [représentant l’état du jeu](#representing-game-state) .

En fonction de ce qui se passe dans le jeu, les champs de texte de la section **État** et **action** sont ajustés.
Voyons comment nous allons initialiser et dessiner la superposition pour ces six États.

### <a name="initializing-and-drawing-the-overlay"></a>Initialisation et tracé de la superposition

Les six États d' **État** ont quelques éléments en commun, ce qui rend les ressources et les méthodes dont ils ont besoin de manière très similaire.
    - Ils utilisent tous un rectangle noir au centre de l’écran comme arrière-plan.
    - Le texte affiché est un **titre** ou un **corps** de texte.
    - Le texte utilise la police Segoe UI et est dessiné au-dessus du rectangle d’arrière-plan. 


L’exemple de jeu présente quatre méthodes qui entrent en jeu lors de la création de la superposition.
 

#### <a name="gameinfooverlaygameinfooverlay"></a>GameInfoOverlay::GameInfoOverlay
Le constructeur [**GameInfoOverlay :: GameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L30-L78) initialise la superposition, en maintenant la surface de la bitmap que nous allons utiliser pour afficher des informations sur le lecteur. Le constructeur obtient une fabrique à partir de l’objet [**ID2D1Device**](/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1device) qui lui est passé, qu’il utilise pour créer un [**ID2D1DeviceContext**](/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1devicecontext) sur lequel l’objet de superposition lui-même peut dessiner. [IDWriteFactory :: CreateTextFormat](/windows/desktop/api/dwrite/nf-dwrite-idwritefactory-createtextformat) 


#### <a name="gameinfooverlaycreatedevicedependentresources"></a>GameInfoOverlay::CreateDeviceDependentResources
[**GameInfoOverlay :: CreateDeviceDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104) est notre méthode permettant de créer des pinceaux qui seront utilisés pour dessiner du texte. Pour ce faire, nous obtenons un objet [**ID2D1DeviceContext2**](/windows/desktop/api/d2d1_3/nn-d2d1_3-id2d1devicecontext2) qui permet la création et le dessin de géométrie, ainsi que des fonctionnalités telles que le rendu d’encre et de maillage de dégradé. Nous créons ensuite une série de pinceaux colorés à l’aide de [**ID2D1SolidColorBrush**](/windows/desktop/api/d2d1/nn-d2d1-id2d1solidcolorbrush) pour dessiner les éléments d’interface utilisateur folling.
- Pinceau noir pour les arrière-plans des rectangles
- Pinceau blanc pour le texte d’État
- Pinceau orange pour le texte d’action

#### <a name="deviceresourcessetdpi"></a>DeviceResources :: SetDpi

La méthode [**DeviceResources :: SetDpi**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L514-L527) définit les points par pouce de la fenêtre. Cette méthode est appelée lorsque la résolution PPP est modifiée et doit être réajustée, ce qui se produit lorsque la fenêtre du jeu est redimensionnée. Après la mise à jour de la résolution PPP, cette méthode appelle également[**DeviceResources :: CreateWindowSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L214-L487) pour s’assurer que les ressources nécessaires sont recréées chaque fois que la fenêtre est redimensionnée.

#### <a name="gameinfooverlaycreatewindowssizedependentresources"></a>GameInfoOverlay::CreateWindowsSizeDependentResources

La méthode [**GameInfoOverlay :: CreateWindowsSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L108-L225) est l’endroit où s’effectue l’ensemble de notre dessin. Voici un aperçu des étapes de la méthode.
- Trois rectangles sont créés pour découper le texte de l’interface utilisateur pour le **titre**, le **corps**et le texte de l' **action** .
    ```cppwinrt 
    m_titleRectangle = D2D1::RectF(
        GameInfoOverlayConstant::SideMargin,
        GameInfoOverlayConstant::TopMargin,
        overlaySize.width - GameInfoOverlayConstant::SideMargin,
        GameInfoOverlayConstant::TopMargin + GameInfoOverlayConstant::TitleHeight
        );
    m_actionRectangle = D2D1::RectF(
        GameInfoOverlayConstant::SideMargin,
        overlaySize.height - (GameInfoOverlayConstant::ActionHeight + GameInfoOverlayConstant::BottomMargin),
        overlaySize.width - GameInfoOverlayConstant::SideMargin,
        overlaySize.height - GameInfoOverlayConstant::BottomMargin
        );
    m_bodyRectangle = D2D1::RectF(
        GameInfoOverlayConstant::SideMargin,
        m_titleRectangle.bottom + GameInfoOverlayConstant::Separator,
        overlaySize.width - GameInfoOverlayConstant::SideMargin,
        m_actionRectangle.top - GameInfoOverlayConstant::Separator
        );
    ```

- Une image bitmap est créée `m_levelBitmap` , qui prend en compte les PPP actuels à l’aide de **CreateBitmap**.
- `m_levelBitmap` est défini en tant que cible de rendu 2D à l’aide de [**ID2D1DeviceContext :: SetTarget**](/windows/desktop/api/d2d1_1/nf-d2d1_1-id2d1devicecontext-settarget).
- La bitmap est effacée chaque pixel noir à l’aide de [**ID2D1RenderTarget :: Clear**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-clear).
- [**ID2D1RenderTarget :: BeginDraw**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-begindraw) est appelé pour initier le dessin. 
- **DrawText** est appelé pour dessiner le texte stocké dans `m_titleString` , `m_bodyString` et `m_actionString` dans le rectangle Approperiate à l’aide du **ID2D1SolidColorBrush**correspondant.
- [**ID2D1RenderTarget :: EndDraw**](ID2D1RenderTarget::EndDraw) est appelé pour arrêter toutes les opérations de dessin sur `m_levelBitmap` .
- Une autre bitmap est créée à l’aide de **CreateBitmap** nommé `m_tooSmallBitmap` pour utiliser comme secours, en affichant uniquement si la configuration d’affichage est trop petite pour le jeu.
- Répétez le processus de dessin sur `m_levelBitmap` pour `m_tooSmallBitmap` , cette fois uniquement la chaîne `Paused` dans le corps.




Nous avons maintenant besoin de six méthodes pour remplir le texte de nos six États de superposition.

### <a name="representing-game-state"></a>Représentant l’état du jeu


Chacun des six États de superposition dans le jeu a une méthode correspondante dans l’objet **GameInfoOverlay** . Ces méthodes tracent une variation de la superposition pour communiquer des informations explicites au joueur sur le jeu proprement dit. Cette communication est représentée par un **titre** et une chaîne de **corps** . Étant donné que l’exemple a déjà configuré les ressources et la disposition de ces informations lorsqu’il a été initialisé et avec la méthode [**GameInfoOverlay :: CreateDeviceDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104) , il n’a besoin de fournir que des chaînes spécifiques à l’état de superposition.

La partie **État** de la superposition est définie avec un appel à l’une des méthodes suivantes.

État du jeu | Status Set (méthode) | Champs d’État
:----- | :------- | :---------
Chargement | [GameInfoOverlay::SetGameLoading](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L254-L306) |**Titre**</br>Chargement des ressources </br>**Corps**</br> Imprime de manière incrémentielle « . » pour impliquer l’activité de chargement.
Statistiques de début/de score élevé | [GameInfoOverlay::SetGameStats](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L310-L354) |**Titre**</br>Score élevé</br> **Corps**</br> Niveaux terminés # </br>Total des points #</br>Nombre total de captures #
Début du niveau | [GameInfoOverlay::SetLevelStart](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L413-L471) |**Titre**</br>Niveau #</br>**Corps**</br>Description de l’objectif de niveau.
Jeu suspendu | [GameInfoOverlay::SetPause](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L475-L502) |**Titre**</br>Jeu suspendu</br>**Corps**</br>Aucune
Fin du jeu | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**Titre**</br>Fin du jeu</br> **Corps**</br> Niveaux terminés # </br>Total des points #</br>Nombre total de captures #</br>Niveaux terminés #</br>Score élevé #
Jeu gagné | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**Titre**</br>Vous avez gagné !</br> **Corps**</br> Niveaux terminés # </br>Total des points #</br>Nombre total de captures #</br>Niveaux terminés #</br>Score élevé #

Avec la méthode [**GameInfoOverlay :: CreateWindowSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L117-L134) , l’exemple a déclaré trois zones rectangulaires correspondant à des régions spécifiques de la superposition.

En gardant ces zones à l’esprit, examinons l’une des méthodes propres à l’état, **GameInfoOverlay::SetGameStats**, et voyons comment la superposition est tracée.

```cppwinrt
void GameInfoOverlay::SetGameStats(int maxLevel, int hitCount, int shotCount)
{
    int length;

    auto d2dContext = m_deviceResources->GetD2DDeviceContext();

    d2dContext->SetTarget(m_levelBitmap.get());
    d2dContext->BeginDraw();
    d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());
    d2dContext->FillRectangle(&m_titleRectangle, m_backgroundBrush.get());
    d2dContext->FillRectangle(&m_bodyRectangle, m_backgroundBrush.get());
    m_titleString = L"High Score";

    d2dContext->DrawText(
        m_titleString.c_str(),
        m_titleString.size(),
        m_textFormatTitle.get(),
        m_titleRectangle,
        m_textBrush.get()
        );
    length = swprintf_s(
        wsbuffer,
        bufferLength,
        L"Levels Completed %d\nTotal Points %d\nTotal Shots %d",
        maxLevel,
        hitCount,
        shotCount
        );
    m_bodyString = std::wstring(wsbuffer, length);
    d2dContext->DrawText(
        m_bodyString.c_str(),
        m_bodyString.size(),
        m_textFormatBody.get(),
        m_bodyRectangle,
        m_textBrush.get()
        );

    // We ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        // The D2DERR_RECREATE_TARGET indicates there has been a problem with the underlying
        // D3D device. All subsequent rendering will be ignored until the device is recreated.
        // This error will be propagated and the appropriate D3D error will be returned from the
        // swapchain->Present(...) call. At that point, the sample will recreate the device
        // and all associated resources. As a result, the D2DERR_RECREATE_TARGET doesn't
        // need to be handled here.
        winrt::check_hresult(hr);
    }
}
```

À l’aide du contexte de périphérique Direct2D que l’objet **GameInfoOverlay** a initialisé, cette méthode remplit le titre et le corps des rectangles avec le noir à l’aide du pinceau d’arrière-plan. Elle écrit le texte pour la chaîne « Meilleur score » dans le rectangle de titre, ainsi qu’une chaîne contenant les informations mises à jour sur l’état du jeu dans le rectangle de corps en utilisant le pinceau de texte blanc.


Le rectangle d’action est mis à jour par un appel ultérieur à [**GameInfoOverlay :: SetAction**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564) à partir d’une méthode sur l’objet **GameMain** , qui fournit les informations d’État du jeu requises par **GameInfoOverlay :: setactement** pour déterminer le message approprié au lecteur, par exemple « TAP to continue ».

La superposition d’un État donné est choisie dans la méthode [**GameMain :: SetGameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/GameMain.cpp#L606-L661) comme suit :

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
        m_uiControl->SetGameStats(
            m_game->HighScore().levelCompleted + 1,
            m_game->HighScore().totalHits,
            m_game->HighScore().totalShots
            );
        break;

    case GameInfoOverlayState::LevelStart:
        m_uiControl->SetLevelStart(
            m_game->LevelCompleted() + 1,
            m_game->CurrentLevel()->Objective(),
            m_game->CurrentLevel()->TimeLimit(),
            m_game->BonusTime()
            );
        break;

    case GameInfoOverlayState::GameOverCompleted:
        m_uiControl->SetGameOver(
            true,
            m_game->LevelCompleted() + 1,
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::GameOverExpired:
        m_uiControl->SetGameOver(
            false,
            m_game->LevelCompleted(),
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::Pause:
        m_uiControl->SetPause(
            m_game->LevelCompleted() + 1,
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->TimeRemaining()
            );
        break;
    }
}
```

À présent, le jeu est un moyen de communiquer des informations textuelles au joueur en fonction de l’état du jeu. nous avons ainsi un moyen de changer ce qui est affiché dans le jeu.

### <a name="next-steps"></a>Étapes suivantes

Dans la rubrique suivante, [Ajout de contrôles](tutorial--adding-controls.md), nous examinons comment le joueur interagit avec l’exemple de jeu et comment l’entrée modifie l’état du jeu.