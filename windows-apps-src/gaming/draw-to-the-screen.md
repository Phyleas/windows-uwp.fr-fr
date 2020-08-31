---
title: Dessiner à l’écran
description: Découvrez comment utiliser les API GXDI et Direct3D pour porter le code OpenGL vers DirectX et comment dessiner un cube tournant à l’écran.
ms.assetid: cc681548-f694-f613-a19d-1525a184d4ab
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, jeux, DirectX, graphiques
ms.localizationpriority: medium
ms.openlocfilehash: 7380ede77eeb14f8b1865d4c948387df7e072453
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89054499"
---
# <a name="draw-to-the-screen"></a>Dessiner à l’écran




**API importantes**

-   [**ID3D11Texture2D**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d)
-   [**ID3D11RenderTargetView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview)
-   [**IDXGISwapChain1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1)

Pour finir, nous portons le code qui trace le cube tournant à l’écran.

Dans OpenGL ES 2.0, votre contexte de dessin est défini par le type EGLContext. Ce type contient les paramètres de fenêtre et de surface, ainsi que les ressources nécessaires pour dessiner dans les cibles de rendu qui seront utilisées pour composer l’image finale affichée dans la fenêtre. Vous utilisez ce contexte pour configurer les ressources graphiques et afficher correctement les résultats de votre pipeline nuanceur à l’écran. L’une des principales ressources est le « tampon d’arrière-plan » (ou « objet tampon de trame ») qui contient les cibles de rendu composées finales, prêtes pour la présentation à l’écran.

Avec Direct3D, le processus de configuration des ressources graphiques pour le dessin à l’écran est plus didactique et requiert quelques API supplémentaires. (Un modèle Microsoft Visual Studio Direct3D peut cependant simplifier considérablement ce processus !) Pour obtenir un contexte (appelé contexte de périphérique Direct3D), vous devez d’abord obtenir un objet [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) et l’utiliser pour créer et configurer un objet [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) . Ces deux objets servent ensemble à configurer les ressources spécifiques dont vous avez besoin pour le dessin à l’écran.

Pour résumer, les API DXGI contiennent principalement des API pour gérer les ressources qui appartiennent directement à la carte graphique et Direct3D contient les API qui servent d’interface entre le processeur graphique et le programme principal qui s’exécute sur le processeur.

Pour établir une comparaison dans le cadre de cet exemple, voici les types pertinents de chaque API :

-   [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1): fournit une représentation virtuelle du périphérique graphique et de ses ressources.
-   [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1): fournit l’interface permettant de configurer les mémoires tampons et d’émettre des commandes de rendu.
-   [**IDXGISwapChain1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1): la chaîne de permutation est analogue à la mémoire tampon d’arrière-plan dans OpenGL ES 2,0. Il s’agit de la zone de mémoire de la carte graphique qui contient la ou les images de rendu final à afficher. Elle est appelée « chaîne de permutation », car elle contient plusieurs tampons modifiables et « permutables » pour présenter le dernier rendu à l’écran.
-   [**ID3D11RenderTargetView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) : cette API contient le tampon de bitmap 2D dans lequel l’appareil Direct3D écrit le contexte et qui est présenté par la chaîne de permutation. Comme dans OpenGL ES 2.0, vous pouvez avoir plusieurs cibles de rendu, dont certaines ne sont pas liées à la chaîne de permutation mais utilisées pour les techniques d’ombrage multipasse.

Dans le modèle, l’objet de rendu contient les champs suivants :

Direct3D 11 : déclarations de périphérique et de contexte de périphérique

``` syntax
Platform::Agile<Windows::UI::Core::CoreWindow>       m_window;

Microsoft::WRL::ComPtr<ID3D11Device1>                m_d3dDevice;
Microsoft::WRL::ComPtr<ID3D11DeviceContext1>          m_d3dContext;
Microsoft::WRL::ComPtr<IDXGISwapChain1>                      m_swapChainCoreWindow;
Microsoft::WRL::ComPtr<ID3D11RenderTargetView>          m_d3dRenderTargetViewWin;
```

C’est la façon dont le tampon d’arrière-plan est configuré en tant que cible de rendu et transmis à la chaîne de permutation.

``` syntax
ComPtr<ID3D11Texture2D> backBuffer;
m_swapChainCoreWindow->GetBuffer(0, IID_PPV_ARGS(backBuffer));
m_d3dDevice->CreateRenderTargetView(
  backBuffer.Get(),
  nullptr,
  &m_d3dRenderTargetViewWin);
```

Le runtime Direct3D crée implicitement un [**IDXGISurface1**](https://docs.microsoft.com/windows/desktop/api/dxgi/nn-dxgi-idxgisurface1) pour le [**ID3D11Texture2D**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d), qui représente la texture sous la forme d’une « mémoire tampon d’arrière-plan » que la chaîne de permutation peut utiliser pour l’affichage.

L’initialisation et la configuration du périphérique et du contexte de périphérique Direct3D, de même que les cibles de rendu, figurent dans les méthodes personnalisées **CreateDeviceResources** et **CreateWindowSizeDependentResources** du modèle Direct3D.

Pour plus d’informations sur le contexte de périphérique Direct3D par rapport au code EGL et au type EGLContext, voir [Porter du code EGL vers DXGI et Direct3D](moving-from-egl-to-dxgi.md).

## <a name="instructions"></a>Instructions

### <a name="step-1-rendering-the-scene-and-displaying-it"></a>Étape 1 : Rendu de la scène et affichage

Après la mise à jour des données de cube (dans cet exemple, en le faisant pivoter légèrement autour de l’axe y), la méthode Render définit la fenêtre d’affichage sur les dimensions du contexte de dessin (EGLContext). Ce contexte contient le tampon de couleur qui sera affiché à la surface de la fenêtre (EGLSurface) sur l’écran configuré (EGLDisplay). À ce stade, l’exemple met à jour les attributs des données de vertex, relie le tampon d’index, dessine le cube et permute le tampon de couleur dessiné par le pipeline d’ombrage sur la surface de la fenêtre.

OpenGL ES 2.0 : rendu d’une trame pour l’affichage

``` syntax
void Render(GraphicsContext *drawContext)
{
  Renderer *renderer = drawContext->renderer;

  int loc;
   
  // Set the viewport
  glViewport ( 0, 0, drawContext->width, drawContext->height );
   
   
  // Clear the color buffer
  glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
  glEnable(GL_DEPTH_TEST);


  // Use the program object
  glUseProgram (renderer->programObject);

  // Load the a_position attribute with the vertex position portion of a vertex buffer element
  loc = glGetAttribLocation(renderer->programObject, "a_position");
  glVertexAttribPointer(loc, 3, GL_FLOAT, GL_FALSE, 
      sizeof(Vertex), 0);
  glEnableVertexAttribArray(loc);

  // Load the a_color attribute with the color position portion of a vertex buffer element
  loc = glGetAttribLocation(renderer->programObject, "a_color");
  glVertexAttribPointer(loc, 4, GL_FLOAT, GL_FALSE, 
      sizeof(Vertex), (GLvoid*) (sizeof(float) * 3));
  glEnableVertexAttribArray(loc);

  // Bind the index buffer
  glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->indexBuffer);

  // Load the MVP matrix
  glUniformMatrix4fv(renderer->mvpLoc, 1, GL_FALSE, (GLfloat*) &renderer->mvpMatrix.m[0][0]);

  // Draw the cube
  glDrawElements(GL_TRIANGLES, renderer->numIndices, GL_UNSIGNED_INT, 0);

  eglSwapBuffers(drawContext->eglDisplay, drawContext->eglSurface);
}
```

Dans Direct3D 11, le processus est très similaire. (Nous supposons que vous utilisez la configuration de fenêtre d’affichage et de cible de rendu du modèle Direct3D).

-   Mettez à jour les mémoires tampons constantes (la matrice Model-View-projection, dans ce cas) avec des appels à [**ID3D11DeviceContext1 :: UpdateSubresource**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-updatesubresource1).
-   Définissez le tampon de vertex avec [**ID3D11DeviceContext1::IASetVertexBuffers**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetvertexbuffers).
-   Définissez le tampon d’index avec [**ID3D11DeviceContext1 :: IASetIndexBuffer**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetindexbuffer).
-   Définissez la topologie de triangle spécifique (une liste de triangles) avec [**ID3D11DeviceContext1::IASetPrimitiveTopology**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetprimitivetopology).
-   Définissez la disposition d’entrée de la mémoire tampon de vertex avec [**ID3D11DeviceContext1 :: IASetInputLayout**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout).
-   Liez le nuanceur de sommets à [**ID3D11DeviceContext1 :: VSSetShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetshader).
-   Liez le nuanceur de fragments avec [**ID3D11DeviceContext1::PSSetShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader).
-   Envoyer les vertex indexés par le biais des nuanceurs et générer la sortie des résultats de la couleur dans la mémoire tampon de la cible de rendu avec [**ID3D11DeviceContext1 ::D rawindexed**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed).
-   Affichez le tampon de cible de rendu avec [**IDXGISwapChain1::Present1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1).

Direct3D 11 : rendu d’une trame pour l’affichage

``` syntax
void RenderObject::Render()
{
  // ...

  // Only update shader resources that have changed since the last frame.
  m_d3dContext->UpdateSubresource(
    m_constantBuffer.Get(),
    0,
    NULL,
    &m_constantBufferData,
    0,
    0);

  // Set up the IA stage corresponding to the current draw operation.
  UINT stride = sizeof(VertexPositionColor);
  UINT offset = 0;
  m_d3dContext->IASetVertexBuffers(
    0,
    1,
    m_vertexBuffer.GetAddressOf(),
    &stride,
    &offset);

  m_d3dContext->IASetIndexBuffer(
    m_indexBuffer.Get(),
    DXGI_FORMAT_R16_UINT,
    0);

  m_d3dContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
  m_d3dContext->IASetInputLayout(m_inputLayout.Get());

  // Set up the vertex shader corresponding to the current draw operation.
  m_d3dContext->VSSetShader(
    m_vertexShader.Get(),
    nullptr,
    0);

  m_d3dContext->VSSetConstantBuffers(
    0,
    1,
    m_constantBuffer.GetAddressOf());

  // Set up the pixel shader corresponding to the current draw operation.
  m_d3dContext->PSSetShader(
    m_pixelShader.Get(),
    nullptr,
    0);

  m_d3dContext->DrawIndexed(
    m_indexCount,
    0,
    0);

    // ...

  m_swapChainCoreWindow->Present1(1, 0, &parameters);
}

```

Une fois [**IDXGISwapChain1 ::P resent1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1) est appelé, votre frame est généré dans l’affichage configuré.

## <a name="previous-step"></a>Étape précédente


[Porter le langage GLSL](port-the-glsl.md)

## <a name="remarks"></a>Remarques

Cet exemple ne s’attarde pas sur la grande complexité de la configuration des ressources de périphérique, en particulier pour les applications DirectX de plateforme Windows universelle (UWP). Nous vous conseillons de revoir l’intégralité du code du modèle, notamment les parties correspondant à la configuration et à la gestion des fenêtres et des ressources de périphérique. Les applications UWP doivent aussi prendre en charge les événements de rotation et les événements de suspension/reprise. Le modèle démontre les meilleures pratiques pour gérer la perte d’une interface ou une modification des paramètres d’affichage.

## <a name="related-topics"></a>Rubriques connexes


* [Comment : porter un convertisseur OpenGL ES 2,0 simple vers Direct3D 11](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)
* [Porter les objets nuanceur](port-the-shader-config.md)
* [Porter le langage GLSL](port-the-glsl.md)
* [Dessiner à l’écran](draw-to-the-screen.md)

 

 




