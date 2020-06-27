---
title: Présentation du rendu
description: Découvrez comment développer le pipeline de rendu pour afficher des graphiques. Présentation du rendu.
ms.assetid: 1da3670b-2067-576f-da50-5eba2f88b3e6
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, UWP, jeux, rendu
ms.localizationpriority: medium
ms.openlocfilehash: d1abb324c5e9e16babbbf8d3650adc39cb995137
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409638"
---
# <a name="rendering-framework-i-intro-to-rendering"></a>Infrastructure de rendu I : présentation du rendu

> [!NOTE]
> Cette rubrique fait partie de la série de didacticiels [créer un jeu de plateforme Windows universelle simple (UWP) avec DirectX](tutorial--create-your-first-uwp-directx-game.md) . La rubrique de ce lien définit le contexte de la série.

Jusqu’à présent, nous avons abordé la structure d’un jeu de plateforme Windows universelle (UWP) et comment définir une machine à États pour gérer le déroulement du jeu. À présent, il est temps d’apprendre à développer l’infrastructure de rendu. Examinons comment l’exemple de jeu affiche la scène de jeu à l’aide de Direct3D 11.

Direct3D 11 contient un ensemble d’API qui permettent d’accéder aux fonctionnalités avancées du matériel graphique à hautes performances qui peuvent être utilisées pour créer des graphiques en 3D pour des applications gourmandes en graphiques, telles que des jeux.

Le rendu des graphiques de jeu à l’écran signifie simplement afficher une séquence de frames à l’écran. Dans chaque frame, vous devez restituer les objets qui sont visibles dans la scène, en fonction de la vue.

Pour afficher un frame, vous devez transmettre les informations de scène requises au matériel afin qu’il puisse être affiché à l’écran. Si vous souhaitez afficher des éléments à l’écran, vous devez commencer le rendu dès que le jeu commence à s’exécuter.

## <a name="objectives"></a>Objectifs

Pour configurer un Framework de rendu de base afin d’afficher la sortie graphique pour un jeu DirectX UWP. Vous pouvez le décomposer en trois étapes.

1. Établissez une connexion à l’interface graphique.
2. Créez les ressources nécessaires pour dessiner les graphiques.
3. Affichez les graphiques en affichant le frame.

Cette rubrique explique comment les graphiques sont rendus, couvrant les étapes 1 et 3.

[Infrastructure de rendu II : le rendu du jeu](tutorial-game-rendering.md) couvre l’étape 2 : &mdash; Comment configurer l’infrastructure de rendu et comment les données sont préparées avant que le rendu puisse se produire.

## <a name="get-started"></a>Bien démarrer

Il est judicieux de vous familiariser avec les concepts de base et de rendu des graphiques. Si vous débutez avec Direct3D et le rendu, consultez [termes et concepts](#terms-and-concepts) pour obtenir une brève description des termes graphiques et de rendu utilisés dans cette rubrique.

Pour ce jeu, la classe **GameRenderer** représente le convertisseur pour cet exemple de jeu. Il est responsable de la création et de la gestion de tous les objets Direct3D 11 et Direct2D utilisés pour générer les éléments visuels du jeu. Il gère également une référence à l’objet **Simple3DGame** utilisé pour récupérer la liste des objets à restituer, ainsi que l’état du jeu pour l’affichage de tête (HUD).

Dans cette partie du didacticiel, nous allons nous concentrer sur le rendu des objets 3D dans le jeu.

## <a name="establish-a-connection-to-the-graphics-interface"></a>Établir une connexion à l’interface graphique

Pour plus d’informations sur l’accès au matériel pour le rendu, consultez la rubrique [définir l’infrastructure d’application UWP du jeu](tutorial--building-the-games-uwp-app-framework.md#the-appinitialize-method) .

### <a name="the-appinitialize-method"></a>La méthode App :: Initialize

La fonction **std :: make_shared** , comme indiqué ci-dessous, est utilisée pour créer un **shared_ptr** à **DX ::D eviceresources**, qui permet également d’accéder à l’appareil.

Dans Direct3D 11, un [appareil](#device) est utilisé pour allouer et détruire des objets, pour restituer des primitives et pour communiquer avec la carte graphique par le biais du pilote Graphics.

```cppwinrt
void Initialize(CoreApplicationView const& applicationView)
{
    ...

    // At this point we have access to the device. 
    // We can create the device-dependent resources.
    m_deviceResources = std::make_shared<DX::DeviceResources>();
}
```

## <a name="display-the-graphics-by-rendering-the-frame"></a>Afficher les graphiques en affichant le frame

La scène de jeu doit être rendue lorsque le jeu est lancé. Les instructions pour le rendu démarrent dans la méthode [**GameMain :: Run**](#gamemainrun-method) , comme indiqué ci-dessous.

Le simple fluide est This.

1. **Update**
2. **Render**
3. **Présent**

### <a name="gamemainrun-method"></a>GameMain :: Run, méthode

```cppwinrt
void GameMain::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible) // if the window is visible
        {
            switch (m_updateState)
            {
            ...
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

### <a name="update"></a>Update

Pour plus d’informations sur la façon dont les États de jeu sont mis à jour dans la méthode [**GameMain :: Update**](tutorial-game-flow-management.md#the-gamemainupdate-method) , consultez la rubrique [gestion du passage de jeux](tutorial-game-flow-management.md) .

### <a name="render"></a>Rendu

Le rendu est implémenté en appelant la méthode [**GameRenderer :: Render**](#gamerendererrender-method) de **GameMain :: Run**.

Si le [rendu stéréo](#stereo-rendering) est activé, il y a deux passes de rendu, &mdash; une pour l’œil gauche et une pour la droite. Dans chaque passe de rendu, nous lions la cible de rendu et la vue de stencil de profondeur à l’appareil. Nous effacons également l’affichage du stencil de profondeur par la suite.

> [!NOTE]
> Le rendu stéréo peut être obtenu à l’aide d’autres méthodes, telles que le stéréo passe unique, à l’aide d’instances de vertex ou de nuanceurs de géométrie. La méthode Two-Rendering-Pass est un moyen plus lent mais plus pratique d’obtenir un rendu stéréo.

Une fois que le jeu est en cours d’exécution et que les ressources sont chargées, nous mettons à jour la [matrice de projection](#projection-transform-matrix), une fois par passe de rendu. Les objets sont légèrement différents de chaque vue. Nous allons ensuite configurer le [pipeline de rendu Graphics](#rendering-pipeline). 

> [!NOTE]
> Pour plus d’informations sur la façon dont les ressources sont chargées [, consultez créer et charger des ressources graphiques DirectX](tutorial-game-rendering.md#create-and-load-directx-graphic-resources) .

Dans cet exemple de jeu, le convertisseur est conçu pour utiliser une disposition de vertex standard sur tous les objets. Cela simplifie la conception du nuanceur et permet de modifier facilement les nuanceurs, indépendamment de la géométrie des objets.

#### <a name="gamerendererrender-method"></a>GameRenderer :: Render, méthode

Nous définissons le contexte Direct3D pour utiliser une disposition de vertex d’entrée. Les objets de disposition d’entrée décrivent comment les données de la mémoire tampon de vertex sont diffusées dans le [pipeline de rendu](#rendering-pipeline). 

Ensuite, nous définissons le contexte Direct3D pour utiliser les mémoires tampons constantes définies précédemment, qui sont utilisées par l’étape de pipeline du [nuanceur de sommets](#vertex-shaders-and-pixel-shaders) et l’étape de pipeline de [nuanceur de pixels](#vertex-shaders-and-pixel-shaders) . 

> [!NOTE]
> Pour plus d’informations sur la définition des mémoires tampons constantes, consultez rendu de l' [infrastructure II : rendu du jeu](tutorial-game-rendering.md) .

Étant donné que la même disposition d’entrée et le même ensemble de mémoires tampons constantes sont utilisés pour tous les nuanceurs qui se trouvent dans le pipeline, ils sont configurés une fois par frame.

```cppwinrt
void GameRenderer::Render()
{
    bool stereoEnabled{ m_deviceResources->GetStereoState() };

    auto d3dContext{ m_deviceResources->GetD3DDeviceContext() };
    auto d2dContext{ m_deviceResources->GetD2DDeviceContext() };

    int renderingPasses = 1;
    if (stereoEnabled)
    {
        renderingPasses = 2;
    }

    for (int i = 0; i < renderingPasses; i++)
    {
        // Iterate through the number of rendering passes to be completed.
        // 2 rendering passes if stereo is enabled.
        if (i > 0)
        {
            // Doing the Right Eye View.
            ID3D11RenderTargetView* const targets[1] = { m_deviceResources->GetBackBufferRenderTargetViewRight() };

            // Resets render targets to the screen.
            // OMSetRenderTargets binds 2 things to the device.
            // 1. Binds one render target atomically to the device.
            // 2. Binds the depth-stencil view, as returned by the GetDepthStencilView method, to the device.
            // For more info, see
            // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets

            d3dContext->OMSetRenderTargets(1, targets, m_deviceResources->GetDepthStencilView());

            // Clears the depth stencil view.
            // A depth stencil view contains the format and buffer to hold depth and stencil info.
            // For more info about depth stencil view, go to: 
            // https://docs.microsoft.com/windows/uwp/graphics-concepts/depth-stencil-view--dsv-
            // A depth buffer is used to store depth information to control which areas of 
            // polygons are rendered rather than hidden from view. To learn more about a depth buffer,
            // go to: https://docs.microsoft.com/windows/uwp/graphics-concepts/depth-buffers
            // A stencil buffer is used to mask pixels in an image, to produce special effects. 
            // The mask determines whether a pixel is drawn or not,
            // by setting the bit to a 1 or 0. To learn more about a stencil buffer,
            // go to: https://docs.microsoft.com/windows/uwp/graphics-concepts/stencil-buffers

            d3dContext->ClearDepthStencilView(m_deviceResources->GetDepthStencilView(), D3D11_CLEAR_DEPTH, 1.0f, 0);

            // Direct2D -- discussed later
            d2dContext->SetTarget(m_deviceResources->GetD2DTargetBitmapRight());
        }
        else
        {
            // Doing the Mono or Left Eye View.
            // As compared to the right eye:
            // m_deviceResources->GetBackBufferRenderTargetView instead of GetBackBufferRenderTargetViewRight
            ID3D11RenderTargetView* const targets[1] = { m_deviceResources->GetBackBufferRenderTargetView() };

            // Same as the Right Eye View.
            d3dContext->OMSetRenderTargets(1, targets, m_deviceResources->GetDepthStencilView());
            d3dContext->ClearDepthStencilView(m_deviceResources->GetDepthStencilView(), D3D11_CLEAR_DEPTH, 1.0f, 0);

            // d2d -- Discussed later under Adding UI
            d2dContext->SetTarget(m_deviceResources->GetD2DTargetBitmap());
        }

        const float clearColor[4] = { 0.5f, 0.5f, 0.8f, 1.0f };

        // Only need to clear the background when not rendering the full 3D scene since
        // the 3D world is a fully enclosed box and the dynamics prevents the camera from
        // moving outside this space.
        if (i > 0)
        {
            // Doing the Right Eye View.
            d3dContext->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetViewRight(), clearColor);
        }
        else
        {
            // Doing the Mono or Left Eye View.
            d3dContext->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetView(), clearColor);
        }

        // Render the scene objects
        if (m_game != nullptr && m_gameResourcesLoaded && m_levelResourcesLoaded)
        {
            // This section is only used after the game state has been initialized and all device
            // resources needed for the game have been created and associated with the game objects.
            if (stereoEnabled)
            {
                // When doing stereo, it is necessary to update the projection matrix once per rendering pass.

                auto orientation = m_deviceResources->GetOrientationTransform3D();

                ConstantBufferChangeOnResize changesOnResize;
                // Apply either a left or right eye projection, which is an offset from the middle
                XMStoreFloat4x4(
                    &changesOnResize.projection,
                    XMMatrixMultiply(
                        XMMatrixTranspose(
                            i == 0 ?
                            m_game->GameCamera().LeftEyeProjection() :
                            m_game->GameCamera().RightEyeProjection()
                            ),
                        XMMatrixTranspose(XMLoadFloat4x4(&orientation))
                        )
                    );

                d3dContext->UpdateSubresource(
                    m_constantBufferChangeOnResize.get(),
                    0,
                    nullptr,
                    &changesOnResize,
                    0,
                    0
                    );
            }

            // Update variables that change once per frame.
            ConstantBufferChangesEveryFrame constantBufferChangesEveryFrameValue;
            XMStoreFloat4x4(
                &constantBufferChangesEveryFrameValue.view,
                XMMatrixTranspose(m_game->GameCamera().View())
                );
            d3dContext->UpdateSubresource(
                m_constantBufferChangesEveryFrame.get(),
                0,
                nullptr,
                &constantBufferChangesEveryFrameValue,
                0,
                0
                );

            // Set up the graphics pipeline. This sample uses the same InputLayout and set of
            // constant buffers for all shaders, so they only need to be set once per frame.
            // For more info about the graphics or rendering pipeline, see
            // https://docs.microsoft.com/windows/win32/direct3d11/overviews-direct3d-11-graphics-pipeline

            // IASetInputLayout binds an input-layout object to the input-assembler (IA) stage. 
            // Input-layout objects describe how vertex buffer data is streamed into the IA pipeline stage.
            // Set up the Direct3D context to use this vertex layout. For more info, see
            // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout
            d3dContext->IASetInputLayout(m_vertexLayout.get());

            // VSSetConstantBuffers sets the constant buffers used by the vertex shader pipeline stage.
            // Set up the Direct3D context to use these constant buffers. For more info, see
            // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-vssetconstantbuffers

            ID3D11Buffer* constantBufferNeverChanges{ m_constantBufferNeverChanges.get() };
            d3dContext->VSSetConstantBuffers(0, 1, &constantBufferNeverChanges);
            ID3D11Buffer* constantBufferChangeOnResize{ m_constantBufferChangeOnResize.get() };
            d3dContext->VSSetConstantBuffers(1, 1, &constantBufferChangeOnResize);
            ID3D11Buffer* constantBufferChangesEveryFrame{ m_constantBufferChangesEveryFrame.get() };
            d3dContext->VSSetConstantBuffers(2, 1, &constantBufferChangesEveryFrame);
            ID3D11Buffer* constantBufferChangesEveryPrim{ m_constantBufferChangesEveryPrim.get() };
            d3dContext->VSSetConstantBuffers(3, 1, &constantBufferChangesEveryPrim);

            // Sets the constant buffers used by the pixel shader pipeline stage. 
            // For more info, see
            // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-pssetconstantbuffers

            d3dContext->PSSetConstantBuffers(2, 1, &constantBufferChangesEveryFrame);
            d3dContext->PSSetConstantBuffers(3, 1, &constantBufferChangesEveryPrim);
            ID3D11SamplerState* samplerLinear{ m_samplerLinear.get() };
            d3dContext->PSSetSamplers(0, 1, &samplerLinear);

            for (auto&& object : m_game->RenderObjects())
            {
                // The 3D object render method handles the rendering.
                // For more info, see Primitive rendering below.
                object->Render(d3dContext, m_constantBufferChangesEveryPrim.get());
            }
        }

        // Start of 2D rendering
        ...
    }
}
```

### <a name="primitive-rendering"></a>Rendu primitif

Lors du rendu de la scène, vous parcourez tous les objets qui doivent être rendus. Les étapes ci-dessous sont répétées pour chaque objet (primitive).

- Mettez à jour la mémoire tampon constante (**m_constantBufferChangesEveryPrim**) avec la [matrice de transformation universelle](#world-transform-matrix) et les informations matérielles du modèle.
- Le **m_constantBufferChangesEveryPrim** contient des paramètres pour chaque objet. Il comprend la matrice de transformation objet-à-entier, ainsi que des propriétés matérielles telles que la couleur et l’exposant spéculaire pour les calculs d’éclairage.
- Définissez le contexte Direct3D pour utiliser la disposition du vertex d’entrée pour les données de l’objet de maillage à diffuser dans l’étape d’assembleur d’entrée (IA) du [pipeline de rendu](#rendering-pipeline).
- Définissez le contexte Direct3D pour utiliser un [tampon d’index](#index-buffer) dans l’étape IA. Fournissez les informations de la primitive : type, ordre des données.
- Envoyer un appel de dessin pour dessiner la primitive non instanciée indexée. La méthode **gameobject :: Render** met à jour la [mémoire tampon constante](#constant-buffer-or-shader-constant-buffer) primitive avec les données spécifiques à une primitive donnée. Cela entraîne un appel **DrawIndexed** sur le contexte pour dessiner la géométrie de chaque primitive. Plus précisément, ce dessin appelle des commandes et des données en file d’attente dans l’unité de traitement graphique (GPU), comme paramétré par les données de mémoire tampon constantes. Chaque appel de dessin exécute le nuanceur de sommets une fois par vertex, puis le [nuanceur de pixels](#vertex-shaders-and-pixel-shaders) une fois pour chaque pixel de chaque triangle dans la primitive. Les textures font partie de l’état utilisé par le nuanceur de pixels pour effectuer le rendu.

Voici les raisons d’utiliser plusieurs mémoires tampons constantes.

- Le jeu utilise plusieurs mémoires tampons constantes, mais il ne doit mettre à jour ces mémoires tampons qu’une seule fois par primitive. Comme mentionné précédemment, les mémoires tampons constantes sont similaires aux entrées des nuanceurs qui s’exécutent pour chaque primitive. Certaines données sont statiques (**m_constantBufferNeverChanges**); certaines données sont constantes sur le cadre (**m_constantBufferChangesEveryFrame**), telles que la position de l’appareil photo ; certaines données sont spécifiques à la primitive, telles que la couleur et les textures (**m_constantBufferChangesEveryPrim**).
- Le convertisseur de jeu répartit ces données entrantes entre différentes mémoires tampons constantes pour optimiser la bande passante de mémoire utilisée par l’UC et le GPU. Cette approche permet également de réduire la quantité de données dont le GPU a besoin pour assurer le suivi. Le GPU dispose d’une grande file d’attente de commandes, et chaque fois que le jeu appelle **Draw**, cette commande est mise en file d’attente avec les données qui lui sont associées. Lorsque le jeu met à jour la mémoire tampon constante de primitives et émet la commande **Draw** suivante, le pilote graphique ajoute cette commande suivante et les données associées à la file d’attente. Si le jeu dessine 100 primitives, la file d’attente peut contenir 100 copies des données de la mémoire tampon constante. Pour réduire la quantité de données que le jeu envoie au GPU, le jeu utilise une mémoire tampon constante primitive distincte qui contient uniquement les mises à jour pour chaque primitive.

#### <a name="gameobjectrender-method"></a>GameObject :: Render, méthode

```cppwinrt
void GameObject::Render(
    _In_ ID3D11DeviceContext* context,
    _In_ ID3D11Buffer* primitiveConstantBuffer
    )
{
    if (!m_active || (m_mesh == nullptr) || (m_normalMaterial == nullptr))
    {
        return;
    }

    ConstantBufferChangesEveryPrim constantBuffer;

    // Put the model matrix info into a constant buffer, in world matrix.
    XMStoreFloat4x4(
        &constantBuffer.worldMatrix,
        XMMatrixTranspose(ModelMatrix())
        );

    // Check to see which material to use on the object.
    // If a collision (a hit) is detected, GameObject::Render checks the current context, which 
    // indicates whether the target has been hit by an ammo sphere. If the target has been hit, 
    // this method applies a hit material, which reverses the colors of the rings of the target to 
    // indicate a successful hit to the player. Otherwise, it applies the default material 
    // with the same method. In both cases, it sets the material by calling Material::RenderSetup, 
    // which sets the appropriate constants into the constant buffer. Then, it calls 
    // ID3D11DeviceContext::PSSetShaderResources to set the corresponding texture resource for the 
    // pixel shader, and ID3D11DeviceContext::VSSetShader and ID3D11DeviceContext::PSSetShader 
    // to set the vertex shader and pixel shader objects themselves, respectively.

    if (m_hit && m_hitMaterial != nullptr)
    {
        m_hitMaterial->RenderSetup(context, &constantBuffer);
    }
    else
    {
        m_normalMaterial->RenderSetup(context, &constantBuffer);
    }

    // Update the primitive constant buffer with the object model's info.
    context->UpdateSubresource(primitiveConstantBuffer, 0, nullptr, &constantBuffer, 0, 0);

    // Render the mesh.
    // See MeshObject::Render method below.
    m_mesh->Render(context);
}
```

#### <a name="meshobjectrender-method"></a>MeshObject :: Render, méthode

```cppwinrt
void MeshObject::Render(_In_ ID3D11DeviceContext* context)
{
    // PNTVertex is a struct. stride provides us the size required for all the mesh data
    // struct PNTVertex
    //{
    //  DirectX::XMFLOAT3 position;
    //  DirectX::XMFLOAT3 normal;
    //  DirectX::XMFLOAT2 textureCoordinate;
    //};
    uint32_t stride{ sizeof(PNTVertex) };
    uint32_t offset{ 0 };

    // Similar to the main render loop.
    // Input-layout objects describe how vertex buffer data is streamed into the IA pipeline stage.
    ID3D11Buffer* vertexBuffer{ m_vertexBuffer.get() };
    context->IASetVertexBuffers(0, 1, &vertexBuffer, &stride, &offset);

    // IASetIndexBuffer binds an index buffer to the input-assembler stage.
    // For more info, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-iasetindexbuffer.
    context->IASetIndexBuffer(m_indexBuffer.get(), DXGI_FORMAT_R16_UINT, 0);

    // Binds information about the primitive type, and data order that describes input data for the input assembler stage.
    // For more info, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-iasetprimitivetopology.
    context->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);

    // Draw indexed, non-instanced primitives. A draw API submits work to the rendering pipeline.
    // For more info, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed.
    context->DrawIndexed(m_indexCount, 0, 0);
}
```

### <a name="deviceresourcespresent-method"></a>DeviceResources ::P méthode reenvoyé

Nous appelons la méthode **DeviceResources ::P** replaced pour afficher le contenu que nous avons placé dans les mémoires tampons.

Nous utilisons le terme chaîne d’échange pour une collection de mémoires tampons utilisées pour afficher des frames à l’utilisateur. Chaque fois qu’une application présente un nouveau Frame à afficher, la première mémoire tampon de la chaîne de permutation remplace la mémoire tampon affichée. Ce processus est appelé échange ou basculement. Pour plus d’informations, consultez [chaînes de permutation](../graphics-concepts/swap-chains.md).

- La méthode **présente** de l’interface **IDXGISwapChain1** indique à [dxgi](#dxgi) de bloquer jusqu’à ce que la synchronisation verticale (VSync) ait lieu, en plaçant l’application en veille jusqu’au Vsync suivant. Cela garantit que vous ne gaspillez pas les images de rendu de cycles qui ne seront jamais affichées à l’écran.
- La méthode **DiscardView** de l’interface **ID3D11DeviceContext3** ignore le contenu de la [cible de rendu](#render-target). Il s’agit d’une opération valide uniquement lorsque le contenu existant sera entièrement remplacé. Si l’intégrité ou le défilement des rectangles sont utilisés, cet appel doit être supprimé.
* À l’aide de la même méthode **DiscardView** , ignorez le contenu du [stencil de profondeur](#depth-stencil).
* La méthode **HandleDeviceLost** est utilisée pour gérer le scénario de l' [appareil](#device) en cours de suppression. Si l’appareil a été supprimé par une mise à niveau de déconnexion ou de pilote, vous devez recréer toutes les ressources de l’appareil. Pour plus d’informations, consultez [gérer les scénarios de suppression d’appareil dans Direct3D 11](handling-device-lost-scenarios.md).

> [!TIP]
> Pour obtenir une fréquence d’images lisse, vous devez vous assurer que la quantité de travail nécessaire pour le rendu d’un cadre est comprise entre les VSyncs.

```cppwinrt
// Present the contents of the swap chain to the screen.
void DX::DeviceResources::Present()
{
    // The first argument instructs DXGI to block until VSync, putting the application
    // to sleep until the next VSync. This ensures we don't waste any cycles rendering
    // frames that will never be displayed to the screen.
    HRESULT hr = m_swapChain->Present(1, 0);

    // Discard the contents of the render target.
    // This is a valid operation only when the existing contents will be entirely
    // overwritten. If dirty or scroll rects are used, this call should be removed.
    m_d3dContext->DiscardView(m_d3dRenderTargetView.get());

    // Discard the contents of the depth stencil.
    m_d3dContext->DiscardView(m_d3dDepthStencilView.get());

    // If the device was removed either by a disconnection or a driver upgrade, we 
    // must recreate all device resources.
    if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
    {
        HandleDeviceLost();
    }
    else
    {
        winrt::check_hresult(hr);
    }
}
```

## <a name="next-steps"></a>Étapes suivantes

Cette rubrique a expliqué comment les graphiques sont affichés à l’écran et fournit une brève description de certains termes de rendu utilisés (ci-dessous). En savoir plus sur le rendu dans l' [infrastructure de rendu II :](tutorial-game-rendering.md) rubrique de rendu de jeu et apprenez à préparer les données nécessaires avant le rendu.

## <a name="terms-and-concepts"></a>Termes et concepts

### <a name="simple-game-scene"></a>Scène de jeu simple

Une scène de jeu simple est constituée de quelques objets avec plusieurs sources de lumière.

La forme d’un objet est définie par un ensemble de coordonnées X, Y, Z dans l’espace. L’emplacement de rendu réel dans le monde du jeu peut être déterminé en appliquant une matrice de transformation aux coordonnées de position X, Y, Z. Il peut également avoir un ensemble de coordonnées de texture &mdash; que vous et V &mdash; qui spécifient comment un matériau est appliqué à l’objet. Cela définit les propriétés de surface de l’objet et vous donne la possibilité de déterminer si un objet a une surface rugueuse (par exemple, une balle de tennis) ou une surface brillante lisse (comme une boule de bowling).

Les informations relatives à la scène et à l’objet sont utilisées par l’infrastructure de rendu pour recréer le frame de scène par Frame, ce qui le rend actif sur votre moniteur d’affichage.

### <a name="rendering-pipeline"></a>Pipeline de rendu

Le pipeline de rendu est le processus par lequel les informations de scène 3D sont traduites en une image affichée à l’écran. Dans Direct3D 11, ce pipeline est programmable. Vous pouvez adapter les étapes pour répondre à vos besoins de rendu. Les étapes qui présentent des cœurs de nuanceurs courants sont programmables à l’aide du langage de programmation HLSL. Il est également connu sous le nom de *pipeline de rendu graphique*, ou simplement *pipeline*.

Pour vous aider à créer ce pipeline, vous devez vous familiariser avec ces détails.

- [HLSL](#hlsl). Nous vous recommandons d’utiliser le modèle de nuanceur HLSL 5,1 et versions ultérieures pour les jeux DirectX UWP.
- [Nuanceurs](#shaders).
- [Nuanceurs de vertex et nuanceurs de pixels](#vertex-shaders-and-pixel-shaders).
- [Étapes du nuanceur](#shader-stages).
- [Différents formats de fichiers de nuanceur](#various-shader-file-formats).

Pour plus d’informations, consultez [comprendre le pipeline de rendu Direct3D 11 et le](/windows/desktop/direct3dgetstarted/understand-the-directx-11-2-graphics-pipeline) [pipeline de graphiques](/windows/desktop/direct3d11/overviews-direct3d-11-graphics-pipeline).

#### <a name="hlsl"></a>HLSL

HLSL est le langage de nuanceur de haut niveau pour DirectX. En HLSL, vous pouvez créer des nuanceurs programmables de type C pour le pipeline Direct3D. Pour plus d’informations, consultez [HLSL](/windows/desktop/direct3dhlsl/dx-graphics-hlsl).

#### <a name="shaders"></a>Nuanceurs

Un nuanceur peut être considéré comme un ensemble d’instructions qui déterminent le mode d’affichage de la surface d’un objet lorsqu’il est rendu. Ceux qui sont programmés à l’aide du langage HLSL sont appelés nuanceurs HLSL. Les fichiers de code source pour les nuanceurs [HLSL]) (#hlsl) ont l' `.hlsl` extension de fichier. Ces nuanceurs peuvent être compilés au moment de la génération ou au moment de l’exécution et définis au moment de l’exécution dans l’étape de pipeline appropriée. Un objet Shader compilé a une `.cso` extension de fichier.

Les nuanceurs Direct3D 9 peuvent être conçus à l’aide du nuanceur modèle 1, du nuanceur modèle 2 et du nuancier modèle 3 ; Les nuanceurs Direct3D 10 peuvent être conçus uniquement sur le nuanceur modèle 4. Les nuanceurs Direct3D 11 peuvent être conçus sur le nuancier Model 5. Direct3D 11,3 et Direct3D 12 peuvent être conçus sur le modèle de nuanceur 5,1, et Direct3D 12 peut également être conçu sur le modèle de nuanceur 6.

#### <a name="vertex-shaders-and-pixel-shaders"></a>Nuanceurs de vertex et nuanceurs de pixels

Les données entrent dans le pipeline graphique en tant que flux de primitives et sont traitées par divers nuanceurs, tels que les nuanceurs de sommets et les nuanceurs de pixels. 

Les nuanceurs vertex traitent les vertex, en effectuant généralement des opérations telles que les transformations, l’apparence et l’éclairage. Les nuanceurs de pixels permettent des techniques d’ombrage riches, telles que l’éclairage par pixel et le traitement des messages. Il combine des variables constantes, des données de texture, des valeurs par vertex interpolées et d’autres données pour produire des sorties par pixel. 

#### <a name="shader-stages"></a>Étapes du nuanceur

Une séquence de ces différents nuanceurs définis pour traiter ce flux de primitives est appelée étapes de nuanceur dans un pipeline de rendu. Les étapes réelles dépendent de la version de Direct3D, mais incluent généralement les étapes vertex, Geometry et pixel. Il existe également d’autres étapes, telles que les nuanceurs de la coque et du domaine pour le pavage et le nuanceur de calcul. Toutes ces étapes sont entièrement programmables à l’aide du [langage HLSL](#hlsl). Pour plus d’informations, consultez [pipeline Graphics](/windows/desktop/direct3d11/overviews-direct3d-11-graphics-pipeline).

#### <a name="various-shader-file-formats"></a>Différents formats de fichiers de nuanceur

Voici les extensions de fichier de code du nuanceur.

- Un fichier avec l' `.hlsl` extension contient le code source [HLSL] (#hlsl).
- Un fichier avec l' `.cso` extension contient un objet de nuanceur compilé.
- Un fichier avec l' `.h` extension est un fichier d’en-tête, mais dans un contexte de code de nuanceur, ce fichier d’en-tête définit un tableau d’octets qui contient les données de nuanceur.
- Un fichier avec l' `.hlsli` extension contient le format des mémoires tampons constantes. Dans l’exemple de jeu, le fichier est **shaders**  >  **ConstantBuffers. hlsli**.

> [!NOTE]
> Vous incorporez un nuanceur en chargeant un `.cso` fichier au moment de l’exécution ou en ajoutant un `.h` fichier dans votre code exécutable. Mais vous n’utiliseriez pas les deux pour le même nuanceur.

### <a name="deeper-understanding-of-directx"></a>Compréhension plus approfondie de DirectX

Direct3D 11 est un ensemble d’API qui peut nous aider à créer des graphiques pour des applications gourmandes en graphiques, telles que des jeux, où nous souhaitons disposer d’une bonne carte graphique pour traiter le calcul intensif. Cette section décrit brièvement les concepts de programmation graphique de Direct3D 11 : ressources, sous-ressources, périphérique et contexte de périphérique.

#### <a name="resource"></a>Ressource

Vous pouvez considérer les ressources (également appelées ressources de l’appareil) comme des informations relatives au rendu d’un objet, tel que la texture, la position ou la couleur. Les ressources fournissent des données au pipeline et définissent ce qui est rendu au cours de votre scène. Les ressources peuvent être chargées à partir de votre média de jeu ou créées dynamiquement au moment de l’exécution.

Une ressource est, en fait, une zone en mémoire accessible par le [pipeline](#rendering-pipeline)Direct3D. Pour permettre un accès efficace du pipeline à la mémoire, les données fournies au pipeline (géométrie d’entrée, ressources des nuanceurs et textures) doivent être stockées dans une ressource. Il existe deux types de ressource dont dérivent l’ensemble des ressources Direct3D : la mémoire tampon et la texture. Jusqu’à 128 ressources peuvent être actives à chaque phase du pipeline. Pour plus d’informations, consultez [Ressources](../graphics-concepts/resources.md).

#### <a name="subresource"></a>Sous-ressource

Le terme sous-ressource fait référence à un sous-ensemble d’une ressource. Direct3D peut faire référence à une ressource entière ou référencer des sous-ensembles d’une ressource. Pour plus d’informations, consultez [Resource](../graphics-concepts/resource-types.md#subresources).

#### <a name="depth-stencil"></a>Stencil-profondeur

Une ressource de stencil de profondeur contient le format et la mémoire tampon pour stocker les informations de profondeur et de stencil. Elle est créée à l’aide d’une ressource de texture. Pour plus d’informations sur la création d’une ressource de stencil de profondeur, consultez Configuration de la [fonctionnalité de stencil de profondeur](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-depth-stencil). Nous allons accéder à la ressource de stencil de profondeur à l’aide de la vue de stencil de profondeur implémentée à l’aide de l’interface [ID3D11DepthStencilView](/windows/desktop/api/d3d11/nn-d3d11-id3d11depthstencilview) .

Les informations de profondeur nous indiquent les zones de polygones qui se trouvent derrière d’autres, ce qui nous permet de déterminer celles qui sont masquées. Les informations du stencil nous indiquent quels pixels sont masqués. Il peut être utilisé pour produire des effets spéciaux, car il détermine si un pixel est dessiné ou non ; définit le bit sur 1 ou 0. 

Pour plus d’informations, consultez [profondeur-affichage du gabarit](../graphics-concepts/depth-stencil-view--dsv-.md), [mémoire tampon de profondeur](../graphics-concepts/depth-buffers.md)et [mémoire tampon du stencil](../graphics-concepts/stencil-buffers.md).

#### <a name="render-target"></a>Cible de rendu

Une cible de rendu est une ressource que nous pouvons écrire à la fin d’une passe de rendu. Elle est généralement créée à l’aide de la méthode [ID3D11Device :: CreateRenderTargetView](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createrendertargetview) à l’aide de la mémoire tampon d’annulation de la chaîne de permutation (qui est également une ressource) comme paramètre d’entrée. 

Chaque cible de rendu doit également avoir une vue de stencil de profondeur correspondante, car quand nous utilisons [OMSetRenderTargets](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) pour définir la cible de rendu avant de l’utiliser, elle requiert également une vue de stencil de profondeur. Nous allons accéder à la ressource cible de rendu via la vue de cible de rendu implémentée à l’aide de l’interface [ID3D11RenderTargetView](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) . 

#### <a name="device"></a>Appareil

Vous pouvez imaginer un appareil comme un moyen d’allouer et de détruire des objets, de restituer des primitives et de communiquer avec la carte graphique par le biais du pilote Graphics. 

Pour obtenir une explication plus précise, un appareil Direct3D est le composant de rendu de Direct3D. Un périphérique encapsule et stocke l’état de rendu, effectue des transformations et des opérations d’éclairage, et rastérise une image sur une surface. Pour plus d’informations, consultez [appareils](../graphics-concepts/devices.md)

Un appareil est représenté par l’interface [**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device) . En d’autres termes, l’interface **ID3D11Device** représente un adaptateur d’affichage virtuel et est utilisée pour créer des ressources détenues par un appareil. 

Il existe différentes versions de ID3D11Device. [**ID3D11Device5**](/windows/desktop/api/d3d11_4/nn-d3d11_4-id3d11device5) est la version la plus récente et ajoute de nouvelles méthodes à celles de **ID3D11Device4**. Pour plus d’informations sur la façon dont Direct3D communique avec le matériel sous-jacent, consultez [architecture du modèle de pilote de périphérique Windows (WDDM)](/windows-hardware/drivers/display/windows-vista-and-later-display-driver-model-architecture).

Chaque application doit avoir au moins un périphérique. la plupart des applications en créent une seule. Créez un appareil pour l’un des pilotes matériels installés sur votre ordinateur en appelant **D3D11CreateDevice** ou **D3D11CreateDeviceAndSwapChain** et en spécifiant le type de pilote avec l’indicateur **D3D_DRIVER_TYPE** . Chaque périphérique peut utiliser un ou plusieurs contextes de périphérique, en fonction de la fonctionnalité souhaitée. Pour plus d’informations, consultez [fonction D3D11CreateDevice](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice).

#### <a name="device-context"></a>Contexte d’appareil

Un contexte de périphérique est utilisé pour définir l’état du [pipeline](#rendering-pipeline) et générer des commandes de rendu à l’aide des [ressources](#resource) détenues par un [appareil](#device). 

Direct3D 11 implémente deux types de contextes de périphérique, un pour le rendu immédiat et l’autre pour le rendu différé ; les deux contextes sont représentés par une interface [ID3D11DeviceContext](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) . 

Les interfaces **ID3D11DeviceContext** ont des versions différentes ; **ID3D11DeviceContext4** ajoute de nouvelles méthodes à celles de **ID3D11DeviceContext3**.

**ID3D11DeviceContext4** est introduite dans Windows 10 Creators Update et est la dernière version de l’interface **ID3D11DeviceContext** . Les applications ciblant Windows 10 Creators Update et versions ultérieures doivent utiliser cette interface au lieu des versions antérieures. Pour plus d’informations, consultez [ID3D11DeviceContext4](/windows/desktop/api/d3d11_3/nn-d3d11_3-id3d11devicecontext4).

#### <a name="dxdeviceresources"></a>DX ::D eviceResources

La classe **DX ::D eviceresources** se trouve dans les fichiers **DeviceResources. cpp** / **. h** et contrôle toutes les ressources de l’appareil DirectX.

### <a name="buffer"></a>Buffer

Une ressource de mémoire tampon est une collection de données entièrement typées regroupées en éléments. Vous pouvez utiliser des mémoires tampons pour stocker un large éventail de données, notamment des vecteurs de position, des vecteurs normaux, des coordonnées de texture dans une mémoire tampon de vertex, des index dans une mémoire tampon d’index ou un état de périphérique. Les éléments de mémoire tampon peuvent inclure des valeurs de données compressées (telles que les valeurs de surface **R8G8B8A8** ), des entiers 8 bits uniques ou des valeurs à virgule flottante 4 32 bits.

Il existe trois types de mémoires tampons disponibles : la mémoire tampon de vertex, le tampon d’index et la mémoire tampon constante.

#### <a name="vertex-buffer"></a>Mémoire tampon de vertex

Contient les données de vertex utilisées pour définir votre géométrie. Les données de vertex incluent les coordonnées de position, les données de couleur, les données de coordonnées de texture, les données normales, etc. 

#### <a name="index-buffer"></a>Mémoire tampon d’index

Contient des décalages d’entiers dans les mémoires tampons de vertex et sont utilisés pour restituer les primitives plus efficacement. Une mémoire tampon d’index contient un ensemble séquentiel d’index 16 bits ou 32 bits ; chaque index est utilisé pour identifier un vertex dans une mémoire tampon de vertex.

#### <a name="constant-buffer-or-shader-constant-buffer"></a>Mémoire tampon constante, ou mémoire tampon constante de nuanceur

Vous permet de fournir efficacement des données de nuanceur au pipeline. Vous pouvez utiliser des mémoires tampons constantes comme entrées pour les nuanceurs qui exécutent pour chaque primitive et les résultats de magasin de l’étape de flux de sortie du pipeline de rendu. D’un côté, une mémoire tampon constante se présente comme une mémoire tampon de vertex à un seul élément.

#### <a name="design-and-implementation-of-buffers"></a>Conception et implémentation de mémoires tampons

Vous pouvez concevoir des mémoires tampons basées sur le type de données, par exemple, comme dans notre exemple de jeu, une mémoire tampon est créée pour les données statiques, une autre pour les données qui sont constantes sur le frame et une autre pour les données spécifiques à une primitive.

Tous les types de mémoires tampons sont encapsulés par l’interface **ID3D11Buffer** et vous pouvez créer une ressource de mémoire tampon en appelant **ID3D11Device :: CreateBuffer**. Toutefois, une mémoire tampon doit être liée au pipeline avant de pouvoir y accéder. Les tampons peuvent être liés à plusieurs étapes de pipeline simultanément pour la lecture. Une mémoire tampon peut également être liée à une étape de pipeline unique pour l’écriture. Toutefois, la même mémoire tampon ne peut pas être liée pour la lecture et l’écriture simultanément.

Vous pouvez lier les mémoires tampons de l’une des manières suivantes.

- À l’étape assembleur d’entrée en appelant des méthodes **ID3D11DeviceContext** telles que **ID3D11DeviceContext :: IASetVertexBuffers** et **ID3D11DeviceContext :: IASetIndexBuffer**.
- À l’étape de flux de sortie en appelant **ID3D11DeviceContext :: SOSetTargets**.
- À l’étape du nuanceur en appelant des méthodes de nuanceur, telles que **ID3D11DeviceContext :: VSSetConstantBuffers**.

Pour plus d’informations, consultez [Introduction aux mémoires tampons dans Direct3D 11](/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-intro).

### <a name="dxgi"></a>DXGI

Microsoft DirectX Graphics infrastructure (DXGI) est un sous-système qui encapsule certaines des tâches de bas niveau qui sont nécessaires à Direct3D. Une attention particulière doit être prise lors de l’utilisation de DXGI dans une application multithread pour s’assurer que les blocages ne se produisent pas. Pour plus d’informations, consultez [Multithreading et dxgi](/windows/win32/direct3darticles/dxgi-best-practices#multithreading-and-dxgi)

### <a name="feature-level"></a>Niveau de fonctionnalité

Le niveau de fonctionnalité est un concept introduit dans Direct3D 11 pour gérer la diversité des cartes vidéo des machines nouvelles et existantes. Un niveau de fonctionnalité est un ensemble bien défini de fonctionnalités de l’unité de traitement graphique (GPU). 

Chaque carte vidéo implémente un certain niveau de fonctionnalité DirectX en fonction des GPU installés. Dans les versions antérieures de Microsoft Direct3D, vous pouviez découvrir la version de Direct3D mise en œuvre par la carte vidéo, puis programmer votre application en conséquence. 

Avec le niveau de fonctionnalité, lorsque vous créez un appareil, vous pouvez essayer de créer un appareil pour le niveau de fonctionnalité que vous souhaitez demander. Si la création de l’appareil fonctionne, ce niveau de fonctionnalité existe. si ce n’est pas le cas, le matériel ne prend pas en charge ce niveau de fonctionnalité. Vous pouvez essayer de recréer un appareil à un niveau de fonctionnalité inférieur, ou vous pouvez choisir de quitter l’application. Par exemple, le niveau de fonctionnalité 12_0 requiert Direct3D 11,3 ou Direct3D 12, et le modèle de nuanceur 5,1. Pour plus d’informations, consultez [niveaux de fonctionnalité Direct3D : vue d’ensemble pour chaque niveau de fonctionnalité](/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro).

À l’aide des niveaux de fonctionnalité, vous pouvez développer une application pour Direct3D 9, Microsoft Direct3D 10 ou Direct3D 11, puis l’exécuter sur un matériel 9, 10 ou 11 (à quelques exceptions près). Pour plus d’informations, consultez [niveaux de fonctionnalité Direct3D](/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro).

### <a name="stereo-rendering"></a>Rendu stéréo

Le rendu stéréo est utilisé pour améliorer l’illusion de profondeur. Elle utilise deux images, une à gauche et l’autre à partir de l’œil droit pour afficher une scène sur l’écran d’affichage. 

Mathématiquement, nous appliquons une matrice de projection stéréo, qui est un décalage horizontal léger à droite et à gauche de la matrice de projection mono normale pour y parvenir.

Nous avons effectué deux passes de rendu pour obtenir un rendu stéréo dans cet exemple de jeu.

- Liez à la cible de rendu de droite, appliquez la projection de droite, puis dessinez l’objet primitif.
- Lier à la cible de rendu gauche, appliquer la projection de gauche, puis dessiner l’objet primitif.

### <a name="camera-and-coordinate-space"></a>Appareil photo et espace de coordonnées

Le jeu a le code en place pour mettre à jour le monde dans son propre système de coordonnées (parfois appelé espace du monde ou espace de scène). Tous les objets, y compris la caméra, sont positionnés et orientés dans cet espace. Pour plus d’informations, consultez [systèmes de coordonnées](../graphics-concepts/coordinate-systems.md).

Un nuanceur vertex effectue le gros de la conversion des coordonnées du modèle en coordonnées de l’appareil à l’aide de l’algorithme suivant (où V est un vecteur et M est une matrice).

`V(device) = V(model) x M(model-to-world) x M(world-to-view) x M(view-to-device)`

- `M(model-to-world)`est une matrice de transformation pour les coordonnées du modèle en coordonnées universelles, également appelée [matrice de transformation universelle](#world-transform-matrix). Elle est fournie par la primitive.
- `M(world-to-view)`matrice de transformation pour les coordonnées universelles des coordonnées de la vue, également appelée [matrice de transformation de vue](#view-transform-matrix).
  - Elle est fournie par la matrice globale de la caméra. Elle est définie par la position de l’appareil photo avec les vecteurs d’apparence (l' *œil au* vecteur qui pointe directement dans la scène de l’appareil photo et le vecteur de *recherche* qui est le plus perpendiculaire).
  - Dans l’exemple de jeu, **m_viewMatrix** est la matrice de transformation de la vue et est calculée à l’aide de **Camera :: SetViewParams**.
- `M(view-to-device)`est une matrice de transformation pour les coordonnées d’affichage des coordonnées de l’appareil, également appelée [matrice de transformation de projection](#projection-transform-matrix).
  - Elle est fournie par la projection de la caméra. Il fournit des informations sur la quantité d’espace réellement visible dans la scène finale. Le champ de vue (l’angle d’affichage), le proportions et les plans de découpage définissent la matrice de transformation de projection.
  - Dans l’exemple de jeu, **m_projectionMatrix** définit la transformation des coordonnées de projection, calculée à l’aide de **Camera :: SetProjParams** (pour la projection stéréo, vous utilisez deux matrices &mdash; de projection une pour chaque œil).

Le code du nuanceur dans `VertexShader.hlsl` est chargé avec ces vecteurs et matrices à partir des mémoires tampons constantes, et effectue cette transformation pour chaque vertex.

### <a name="coordinate-transformation"></a>Transformation de coordonnée

Direct3D utilise trois transformations pour modifier vos coordonnées de modèle 3D en coordonnées de pixels (espace à l’écran). Ces transformations sont la transformation universelle, la transformation de vue et la transformation de projection. Pour plus d’informations, consultez [vue d’ensemble des transformations](../graphics-concepts/transform-overview.md).

#### <a name="world-transform-matrix"></a>Matrice de transformation universelle

Une transformation universelle change les coordonnées de l’espace de modèle, où les vertex sont définis par rapport à l’origine locale d’un modèle, à l’espace universel, où les vertex sont définis par rapport à une origine commune à tous les objets d’une scène. En résumé, la transformation universelle place un modèle dans le monde entier. donc son nom. Pour plus d’informations, consultez [transformation universelle](../graphics-concepts/world-transform.md).

#### <a name="view-transform-matrix"></a>Afficher la matrice de transformation

La transformation de vue localise la visionneuse dans l’espace universel, en transformant les vertex en espace de caméra. Dans l’espace de l’appareil photo, l’appareil photo, ou la visionneuse, est à l’origine, en regardant l’axe z positif. Pour plus d’informations, accédez à [afficher la transformation](../graphics-concepts/view-transform.md).

####  <a name="projection-transform-matrix"></a>Matrice de transformation de projection

La transformation de projection convertit l’affichage frustum en forme cuboid. Un frustum d’affichage est un volume 3D dans une scène positionnée par rapport à l’appareil photo de la fenêtre d’affichage. Une fenêtre d’affichage est un rectangle 2D dans lequel une scène 3D est projetée. Pour plus d’informations, consultez [Viewports et découpage](../graphics-concepts/viewports-and-clipping.md) .

Étant donné que la fin de la frustum d’affichage est inférieure à la fin, cela a pour effet d’étendre les objets qui sont proches de l’appareil photo. C’est ainsi que la perspective est appliquée à la scène. Ainsi, les objets qui sont plus proches du joueur apparaissent plus grands ; les objets qui sont plus éloignés s’affichent plus petit.

Mathématiquement, la transformation de projection est une matrice qui est généralement une projection d’échelle et de perspective. Elle fonctionne comme la lentille d’une caméra. Pour plus d’informations, consultez [projection Transform](../graphics-concepts/projection-transform.md).

### <a name="sampler-state"></a>État de l’échantillonneur

L’état de l’échantillonneur détermine comment les données de texture sont échantillonnées à l’aide des modes d’adressage de texture, du filtrage et du niveau de détail. L’échantillonnage s’effectue chaque fois qu’un pixel de texture (ou Texel) est lu à partir d’une texture.

Une texture contient un tableau de texels. La position de chaque Texel est indiquée par `(u,v)` , où `u` est la largeur et `v` est la hauteur, et est mappée entre 0 et 1 en fonction de la largeur et de la hauteur de la texture. Les coordonnées de texture résultantes sont utilisées pour traiter un Texel lors de l’échantillonnage d’une texture.

Lorsque les coordonnées de texture sont inférieures à 0 ou supérieur à 1, le mode d’adresse de texture définit la manière dont la coordonnée de texture adresse un emplacement Texel. Par exemple, lors de l’utilisation de **TextureAddressMode. Clamp**, toute coordonnée en dehors de la plage 0-1 est ancrée à une valeur maximale de 1 et la valeur minimale 0 avant l’échantillonnage.

Si la texture est trop grande ou trop petite pour le polygone, la texture est filtrée pour s’ajuster à l’espace. Un filtre d’agrandissement agrandit une texture, un filtre de minimisation réduit la texture pour s’ajuster à une plus petite zone. L’agrandissement de la texture répète l’exemple de Texel pour une ou plusieurs adresses qui génèrent une image blurrier. La minimisation de la texture est plus compliquée, car elle nécessite la combinaison de plusieurs valeurs texels en une seule valeur. Cela peut entraîner des contours d’alias ou des bords en escalier en fonction des données de texture. L’approche la plus populaire pour la minimisation consiste à utiliser un mipmap. Un mipmap est une texture à plusieurs niveaux. La taille de chaque niveau est une puissance de 2 inférieure au niveau précédent jusqu’à une texture 1x1. Lorsque la minimisation est utilisée, un jeu choisit le niveau de mipmap le plus proche de la taille nécessaire au moment du rendu. 

### <a name="the-basicloader-class"></a>La classe BasicLoader

**BasicLoader** est une classe de chargeur simple qui prend en charge le chargement des nuanceurs, des textures et des mailles à partir de fichiers sur le disque. Il fournit à la fois des méthodes synchrones et asynchrones. Dans cet exemple de jeu, les `BasicLoader.h/.cpp` fichiers se trouvent dans le dossier **utilitaires** .

Pour plus d’informations, consultez la page relative au [chargeur de base](complete-code-for-basicloader.md).