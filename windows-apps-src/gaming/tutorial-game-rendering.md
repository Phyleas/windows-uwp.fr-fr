---
title: Configurer
description: Découvrez comment assembler le pipeline de rendu pour afficher des graphiques. Le rendu du jeu, la configuration et la préparation des données.
ms.assetid: 7720ac98-9662-4cf3-89c5-7ff81896364a
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, UWP, jeux, rendu
ms.localizationpriority: medium
ms.openlocfilehash: 1a3f689f86e629ce81946927fa732a3ab692b219
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409508"
---
# <a name="rendering-framework-ii-game-rendering"></a>Infrastructure de rendu II : rendu de jeu

> [!NOTE]
> Cette rubrique fait partie de la série de didacticiels [créer un jeu de plateforme Windows universelle simple (UWP) avec DirectX](tutorial--create-your-first-uwp-directx-game.md) . La rubrique de ce lien définit le contexte de la série.

Dans l' [infrastructure de rendu I](tutorial--assembling-the-rendering-pipeline.md), nous avons abordé la manière dont nous prenons les informations de scène et nous la présentons sur l’écran d’affichage. Nous allons maintenant effectuer un pas à pas détaillé et apprendre à préparer les données pour le rendu.

>[!Note]
>Si vous n’avez pas téléchargé le dernier code de jeu pour cet exemple, accédez à l' [exemple de jeu Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Cet exemple fait partie d’une grande collection d’exemples de fonctionnalités UWP. Pour obtenir des instructions sur le téléchargement de l’exemple, consultez [obtenir les exemples UWP à partir de GitHub](/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Objectif

Récapitulatif rapide sur l’objectif. Il doit comprendre comment configurer une infrastructure de rendu de base pour afficher la sortie graphique pour un jeu DirectX UWP. Nous pouvons les regrouper librement en trois étapes.

 1. Établir une connexion à notre interface graphique
 2. Préparation : créer les ressources dont nous avons besoin pour dessiner les graphiques
 3. Afficher les graphiques : restituer le frame

[Infrastructure de rendu I : introduction au rendu explication du](tutorial--assembling-the-rendering-pipeline.md) rendu des graphiques, couvrant les étapes 1 et 3. 

Cet article explique comment configurer d’autres parties de cette infrastructure et préparer les données requises avant de pouvoir effectuer le rendu, à savoir l’étape 2 du processus.

## <a name="design-the-renderer"></a>Concevoir le convertisseur

Le convertisseur est responsable de la création et de la gestion de tous les objets D3D11 et D2D utilisés pour générer les éléments visuels du jeu. La classe __GameRenderer__ est le convertisseur de cet exemple de jeu et est conçu pour répondre aux besoins de rendu du jeu.

Voici quelques concepts que vous pouvez utiliser pour vous aider à concevoir le convertisseur pour votre jeu :
* Étant donné que les API Direct3D 11 sont définies en tant qu’API [com](/windows/desktop/com/the-component-object-model) , vous devez fournir des références [ComPtr](/cpp/windows/comptr-class) aux objets définis par ces API. Ces objets sont automatiquement libérés lorsque leur dernière référence sort du contexte une fois l’exécution de l’application terminée. Pour plus d’informations, consultez [ComPtr](https://github.com/Microsoft/DirectXTK/wiki/ComPtr). Exemple de ces objets : mémoires tampons constantes, objets nuanceur- [nuanceur de sommets](tutorial--assembling-the-rendering-pipeline.md#vertex-shaders-and-pixel-shaders), [nuanceur de pixels](tutorial--assembling-the-rendering-pipeline.md#vertex-shaders-and-pixel-shaders)et objets de ressource de nuanceur.
* Les mémoires tampons constantes sont définies dans cette classe pour contenir différentes données nécessaires au rendu.
    * Utilisez plusieurs mémoires tampons constantes avec différentes fréquences pour réduire la quantité de données qui doivent être envoyées au GPU par trame. Cet exemple sépare les constantes dans des mémoires tampons différentes en fonction de la fréquence à laquelle elles doivent être mises à jour. Il s’agit d’une recommandation pour la programmation Direct3D. 
    * Dans cet exemple de jeu, 4 mémoires tampons constantes sont définies.
        1. __m \_ constantBufferNeverChanges__ contient les paramètres d’éclairage. Elle est définie une fois dans la méthode __FinalizeCreateGameDeviceResources__ et ne change jamais.
        2. __m \_ constantBufferChangeOnResize__ contient la matrice de projection. La matrice de projection dépend de la taille et des proportions de la fenêtre. Elle est définie dans [__CreateWindowSizeDependentResources__](#createwindowsizedependentresource-method) , puis mise à jour après le chargement des ressources dans la méthode [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method) . Si le rendu est en 3D, il est également modifié deux fois par cadre.
        3. __m \_ constantBufferChangesEveryFrame__ contient la matrice de vue. Cette matrice dépend de la position de la caméra et de la direction de l’apparence (la normale à la projection) et change une fois par Frame dans la méthode __Render__ . Cela a été abordé plus haut dans l' __infrastructure de rendu I : introduction au rendu__, sous la [méthode __GameRenderer :: Render__ ](tutorial--assembling-the-rendering-pipeline.md#gamerendererrender-method).
        4. __m \_ constantBufferChangesEveryPrim__ contient les propriétés de matériau et de matrice de modèle de chaque primitive. La matrice de modèle transforme les vertex des coordonnées locales en coordonnées universelles. Ces constantes sont spécifiques à chaque primitive et sont mises à jour pour chaque appel de dessin. Cela a été abordé plus haut dans l' __infrastructure de rendu I : introduction au rendu__, sous le [rendu primitif](tutorial--assembling-the-rendering-pipeline.md#primitive-rendering).
* Les objets de ressource de nuanceur qui contiennent des textures pour les primitives sont également définis dans cette classe.
    * Certaines textures sont prédéfinies ([DDS](/windows/desktop/direct3ddds/dx-graphics-dds-pguide) est un format de fichier qui peut être utilisé pour stocker des textures compressées et non compressées. Les textures DDS sont utilisées pour les murs et le plancher du monde, ainsi que pour les AMMO.)
    * Dans cet exemple de jeu, les objets de ressource de nuanceur sont : __m \_ sphereTexture__, m __ \_ cylinderTexture__, m __ \_ ceilingTexture__, __m \_ floorTexture__, __m \_ wallsTexture__.
* Les objets nuanceur sont définis dans cette classe pour calculer nos primitives et leurs textures. 
    * Dans cet exemple de jeu, les objets de nuanceur sont __m \_ vertexShader__, __m \_ vertexShaderFlat__et __m \_ pixelShader__, __m \_ pixelShaderFlat__.
    * Le nuanceur de vertex traite les primitives et l’éclairage de base, tandis que le nuanceur de pixels (parfois appelé nuanceur de fragments) traite les textures et tous les effets par pixel.
    * Il existe deux versions de ces nuanceurs (régulier et plat) pour rendre des primitives différentes. La raison pour laquelle nous avons des versions différentes est que les versions plates sont bien plus simples et n’effectuent pas de surbrillances spéculaires ou d’effets d’éclairage par pixel. Ces nuanceurs sont utilisés pour les murs et accélèrent le rendu sur des périphériques de faible puissance.

## <a name="gamerendererh"></a>GameRenderer.h

Examinons à présent le code de l’objet de classe de convertisseur de l’exemple de jeu.

```cppwinrt
// Class handling the rendering of the game
class GameRenderer : public std::enable_shared_from_this<GameRenderer>
{
public:
    GameRenderer(std::shared_ptr<DX::DeviceResources> const& deviceResources);

    void CreateDeviceDependentResources();
    void CreateWindowSizeDependentResources();
    void ReleaseDeviceDependentResources();
    void Render();
    // --- end of async related methods section

    winrt::Windows::Foundation::IAsyncAction CreateGameDeviceResourcesAsync(_In_ std::shared_ptr<Simple3DGame> game);
    void FinalizeCreateGameDeviceResources();
    winrt::Windows::Foundation::IAsyncAction LoadLevelResourcesAsync();
    void FinalizeLoadLevelResources();

    Simple3DGameDX::IGameUIControl* GameUIControl() { return &m_gameInfoOverlay; };

    DirectX::XMFLOAT2 GameInfoOverlayUpperLeft()
    {
        return DirectX::XMFLOAT2(m_gameInfoOverlayRect.left, m_gameInfoOverlayRect.top);
    };
    DirectX::XMFLOAT2 GameInfoOverlayLowerRight()
    {
        return DirectX::XMFLOAT2(m_gameInfoOverlayRect.right, m_gameInfoOverlayRect.bottom);
    };
    bool GameInfoOverlayVisible() { return m_gameInfoOverlay.Visible(); }
    // --- end of rendering overlay section
...
private:
    // Cached pointer to device resources.
    std::shared_ptr<DX::DeviceResources>        m_deviceResources;

    ...

    // Shader resource objects
    winrt::com_ptr<ID3D11ShaderResourceView>    m_sphereTexture;
    winrt::com_ptr<ID3D11ShaderResourceView>    m_cylinderTexture;
    winrt::com_ptr<ID3D11ShaderResourceView>    m_ceilingTexture;
    winrt::com_ptr<ID3D11ShaderResourceView>    m_floorTexture;
    winrt::com_ptr<ID3D11ShaderResourceView>    m_wallsTexture;

    // Constant buffers
    winrt::com_ptr<ID3D11Buffer>                m_constantBufferNeverChanges;
    winrt::com_ptr<ID3D11Buffer>                m_constantBufferChangeOnResize;
    winrt::com_ptr<ID3D11Buffer>                m_constantBufferChangesEveryFrame;
    winrt::com_ptr<ID3D11Buffer>                m_constantBufferChangesEveryPrim;

    // Texture sampler
    winrt::com_ptr<ID3D11SamplerState>          m_samplerLinear;

    // Shader objects: Vertex shaders and pixel shaders
    winrt::com_ptr<ID3D11VertexShader>          m_vertexShader;
    winrt::com_ptr<ID3D11VertexShader>          m_vertexShaderFlat;
    winrt::com_ptr<ID3D11PixelShader>           m_pixelShader;
    winrt::com_ptr<ID3D11PixelShader>           m_pixelShaderFlat;
    winrt::com_ptr<ID3D11InputLayout>           m_vertexLayout;
};
```

## <a name="constructor"></a>Constructeur

Nous allons ensuite examiner le constructeur __GameRenderer__ du jeu d’exemples et le comparer au constructeur __Sample3DSceneRenderer__ fourni dans le modèle d’application DirectX 11.

```cppwinrt
// Constructor method of the main rendering class object
GameRenderer::GameRenderer(std::shared_ptr<DX::DeviceResources> const& deviceResources) : ...
    m_gameInfoOverlay(deviceResources),
    m_gameHud(deviceResources, L"Windows platform samples", L"DirectX first-person game sample")
{
    // m_gameInfoOverlay is a GameHud object to render text in the top left corner of the screen.
    // m_gameHud is Game info rendered as an overlay on the top-right corner of the screen,
    // for example hits, shots, and time.

    CreateDeviceDependentResources();
    CreateWindowSizeDependentResources();
}
```

## <a name="create-and-load-directx-graphic-resources"></a>Créer et charger des ressources graphiques DirectX

Dans l’exemple de jeu (et dans le modèle d' __application DirectX 11 (Windows universel)__ de Visual Studio), la création et le chargement des ressources de jeu sont implémentées à l’aide de ces deux méthodes qui sont appelées à partir du constructeur __GameRenderer__ :

* [__CreateDeviceDependentResources__](#createdevicedependentresources-method)
* [__CreateWindowSizeDependentResources__](#createwindowsizedependentresource-method)

## <a name="createdevicedependentresources-method"></a>Méthode CreateDeviceDependentResources

Dans le modèle d’application DirectX 11, cette méthode est utilisée pour charger le nuanceur vertex et pixel de manière asynchrone, créer le nuanceur et la mémoire tampon constante, créer un maillage avec des sommets qui contiennent des informations sur la position et la couleur. 

Dans l’exemple de jeu, ces opérations des objets de scène sont à la place fractionnées entre les méthodes [__CreateGameDeviceResourcesAsync__](#creategamedeviceresourcesasync-method) et [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method) . 

Pour cet exemple de jeu, qu’est-ce qui va dans cette méthode ?

* Variables instanciées (__m \_ gameResourcesLoaded__ = false et __m \_ levelResourcesLoaded__ = false) qui indiquent si les ressources ont été chargées avant le rendu, puisque nous les chargeons de manière asynchrone. 
* Étant donné que le rendu HUD et superposition se trouvent dans des objets de classe distincts, appelez ici les méthodes __GameHud :: CreateDeviceDependentResources__ et __GameInfoOverlay :: CreateDeviceDependentResources__ .

Voici le code de __GameRenderer :: CreateDeviceDependentResources__.

```cppwinrt
// This method is called in GameRenderer constructor when it's created in GameMain constructor.
void GameRenderer::CreateDeviceDependentResources()
{
    // instantiate variables that indicate whether resources were loaded.
    m_gameResourcesLoaded = false;
    m_levelResourcesLoaded = false;

    // game HUD and overlay are design as separate class objects.
    m_gameHud.CreateDeviceDependentResources();
    m_gameInfoOverlay.CreateDeviceDependentResources();
}
```

Vous trouverez ci-dessous une liste des méthodes utilisées pour créer et charger des ressources.

- CreateDeviceDependentResources
  - CreateGameDeviceResourcesAsync (ajouté)
  - FinalizeCreateGameDeviceResources (ajouté)
- CreateWindowSizeDependentResources

Avant de plonger dans les autres méthodes utilisées pour créer et charger des ressources, commençons par créer le convertisseur et voyons comment il s’intègre à la boucle de jeu.

## <a name="create-the-renderer"></a>Créer le convertisseur

Le __GameRenderer__ est créé dans le constructeur de __GameMain__. Il appelle également les deux autres méthodes, [__CreateGameDeviceResourcesAsync__](#creategamedeviceresourcesasync-method) et [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method) , qui sont ajoutées pour aider à créer et charger des ressources.

```cppwinrt
GameMain::GameMain(std::shared_ptr<DX::DeviceResources> const& deviceResources) : ...
{
    m_deviceResources->RegisterDeviceNotify(this);

    // Creation of GameRenderer
    m_renderer = std::make_shared<GameRenderer>(m_deviceResources);

    ...

    ConstructInBackground();
}

winrt::fire_and_forget GameMain::ConstructInBackground()
{
    ...

    // Asynchronously initialize the game class and load the renderer device resources.
    // By doing all this asynchronously, the game gets to its main loop more quickly
    // and in parallel all the necessary resources are loaded on other threads.
    m_game->Initialize(m_controller, m_renderer);

    co_await m_renderer->CreateGameDeviceResourcesAsync(m_game);

    // The finalize code needs to run in the same thread context
    // as the m_renderer object was created because the D3D device context
    // can ONLY be accessed on a single thread.
    // co_await of an IAsyncAction resumes in the same thread context.
    m_renderer->FinalizeCreateGameDeviceResources();

    InitializeGameState();

    ...
}
```

## <a name="creategamedeviceresourcesasync-method"></a>Méthode CreateGameDeviceResourcesAsync

__CreateGameDeviceResourcesAsync__ est appelé à partir de la méthode de constructeur __GameMain__ dans la boucle __Create \_ Task__ , car nous chargeons les ressources de jeu de manière asynchrone.
        
__CreateDeviceResourcesAsync__ est une méthode qui exécute un ensemble de tâches asynchrones afin de charger les ressources de jeu. Étant donné qu’il est supposé s’exécuter sur un thread distinct, il a uniquement accès aux méthodes d’appareil Direct3D 11 (celles définies sur __ID3D11Device__) et non aux méthodes de contexte de périphérique (les méthodes définies sur __ID3D11DeviceContext__), de sorte qu’il n’effectue aucun rendu.

La méthode __FinalizeCreateGameDeviceResources__ s’exécute sur le thread principal et a accès aux méthodes de contexte de périphérique Direct3D 11.

En principe :
* Utilisez uniquement les méthodes __ID3D11Device__ dans __CreateGameDeviceResourcesAsync__ , car elles sont libres de thread, ce qui signifie qu’elles peuvent s’exécuter sur n’importe quel thread. Il est également prévu qu’ils ne s’exécutent pas sur le même thread que celui sur lequel le __GameRenderer__ a été créé. 
* N’utilisez pas de méthodes dans __ID3D11DeviceContext__ ici, car elles doivent s’exécuter sur un thread unique et sur le même thread que __GameRenderer__.
* Utilisez cette méthode pour créer des mémoires tampons constantes.
* Utilisez cette méthode pour charger des textures (comme les fichiers. DDS) et des informations de nuanceur (comme les fichiers. CSO) dans les [nuanceurs](tutorial--assembling-the-rendering-pipeline.md#shaders).

Cette méthode permet d’effectuer les opérations suivantes :
* Créez les 4 [mémoires tampons constantes](tutorial--assembling-the-rendering-pipeline.md#buffer): __m \_ constantBufferNeverChanges__, m __ \_ constantBufferChangeOnResize__, m __ \_ constantBufferChangesEveryFrame__, __m \_ constantBufferChangesEveryPrim__
* Créer un objet d' [État](tutorial--assembling-the-rendering-pipeline.md#sampler-state) d’échantillonnage qui encapsule les informations d’échantillonnage pour une texture
* Créez un groupe de tâches qui contient toutes les tâches asynchrones créées par la méthode. Elle attend la fin de toutes ces tâches Async, puis appelle __FinalizeCreateGameDeviceResources__.
* Créez un chargeur à l’aide du [chargeur de base](tutorial--assembling-the-rendering-pipeline.md#the-basicloader-class). Ajoutez les opérations de chargement asynchrone du chargeur en tant que tâches dans le groupe de tâches créé précédemment.
* Des méthodes telles que __BasicLoader :: LoadShaderAsync__ et __BasicLoader :: LoadTextureAsync__ sont utilisées pour charger :
    * objets de nuanceur compilés (VertextShader. CSO, VertexShaderFlat. CSO, PixelShader. CSO et PixelShaderFlat. CSO). Pour plus d’informations, accédez à [différents formats de fichiers de nuanceur](tutorial--assembling-the-rendering-pipeline.md#various-shader-file-formats).
    * textures spécifiques aux jeux (biens de \\ surface. DDS, metal_texture. DDS, cellceiling. DDS, cellfloor. DDS, cellwall. DDS).

```cppwinrt
IAsyncAction GameRenderer::CreateGameDeviceResourcesAsync(_In_ std::shared_ptr<Simple3DGame> game)
{
    auto lifetime = shared_from_this();

    // Create the device dependent game resources.
    // Only the d3dDevice is used in this method. It is expected
    // to not run on the same thread as the GameRenderer was created.
    // Create methods on the d3dDevice are free-threaded and are safe while any methods
    // in the d3dContext should only be used on a single thread and handled
    // in the FinalizeCreateGameDeviceResources method.
    m_game = game;

    auto d3dDevice = m_deviceResources->GetD3DDevice();

    // Define D3D11_BUFFER_DESC. See
    // https://docs.microsoft.com/windows/win32/api/d3d11/ns-d3d11-d3d11_buffer_desc
    D3D11_BUFFER_DESC bd;
    ZeroMemory(&bd, sizeof(bd));

    // Create the constant buffers.
    bd.Usage = D3D11_USAGE_DEFAULT;
    ...

    // Create the constant buffers: m_constantBufferNeverChanges, m_constantBufferChangeOnResize,
    // m_constantBufferChangesEveryFrame, m_constantBufferChangesEveryPrim
    // CreateBuffer is used to create one of these buffers: vertex buffer, index buffer, or 
    // shader-constant buffer. For CreateBuffer API ref info, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11device-createbuffer.
    winrt::check_hresult(
        d3dDevice->CreateBuffer(&bd, nullptr, m_constantBufferNeverChanges.put())
        );

    ...

    // Define D3D11_SAMPLER_DESC. For API ref, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/ns-d3d11-d3d11_sampler_desc.
    D3D11_SAMPLER_DESC sampDesc;

    // ZeroMemory fills a block of memory with zeros. For API ref, see
    // https://docs.microsoft.com/previous-versions/windows/desktop/legacy/aa366920(v=vs.85).
    ZeroMemory(&sampDesc, sizeof(sampDesc));

    sampDesc.Filter = D3D11_FILTER_MIN_MAG_MIP_LINEAR;
    sampDesc.AddressU = D3D11_TEXTURE_ADDRESS_WRAP;
    sampDesc.AddressV = D3D11_TEXTURE_ADDRESS_WRAP;
    ...

    // Create a sampler-state object that encapsulates sampling information for a texture.
    // The sampler-state interface holds a description for sampler state that you can bind to any 
    // shader stage of the pipeline for reference by texture sample operations.
    winrt::check_hresult(
        d3dDevice->CreateSamplerState(&sampDesc, m_samplerLinear.put())
        );

    // Start the async tasks to load the shaders and textures.

    // Load compiled shader objects (VertextShader.cso, VertexShaderFlat.cso, PixelShader.cso, and PixelShaderFlat.cso).
    // The BasicLoader class is used to convert and load common graphics resources, such as meshes, textures, 
    // and various shader objects into the constant buffers. For more info, see
    // https://docs.microsoft.com/windows/uwp/gaming/complete-code-for-basicloader.
    BasicLoader loader{ d3dDevice };

    std::vector<IAsyncAction> tasks;

    uint32_t numElements = ARRAYSIZE(PNTVertexLayout);

    // Load shaders asynchronously with the shader and pixel data using the
    // BasicLoader::LoadShaderAsync method. Push these method calls into a list of tasks.
    tasks.push_back(loader.LoadShaderAsync(L"VertexShader.cso", PNTVertexLayout, numElements, m_vertexShader.put(), m_vertexLayout.put()));
    tasks.push_back(loader.LoadShaderAsync(L"VertexShaderFlat.cso", nullptr, numElements, m_vertexShaderFlat.put(), nullptr));
    tasks.push_back(loader.LoadShaderAsync(L"PixelShader.cso", m_pixelShader.put()));
    tasks.push_back(loader.LoadShaderAsync(L"PixelShaderFlat.cso", m_pixelShaderFlat.put()));

    // Make sure the previous versions if any of the textures are released.
    m_sphereTexture = nullptr;
    ...

    // Load Game specific textures (Assets\\seafloor.dds, metal_texture.dds, cellceiling.dds,
    // cellfloor.dds, cellwall.dds).
    // Push these method calls also into a list of tasks.
    tasks.push_back(loader.LoadTextureAsync(L"Assets\\seafloor.dds", nullptr, m_sphereTexture.put()));
    ...

    // Simulate loading additional resources by introducing a delay.
    tasks.push_back([]() -> IAsyncAction { co_await winrt::resume_after(GameConstants::InitialLoadingDelay); }());

    // Returns when all the async tasks for loading the shader and texture assets have completed.
    for (auto&& task : tasks)
    {
        co_await task;
    }
}
```

## <a name="finalizecreategamedeviceresources-method"></a>Méthode FinalizeCreateGameDeviceResources

La méthode __FinalizeCreateGameDeviceResources__ est appelée après l’exécution de toutes les tâches de chargement des ressources qui se trouvent dans la méthode __CreateGameDeviceResourcesAsync__ . 

* Initialisez constantBufferNeverChanges avec les positions et la couleur de l’éclairage. Charge les données initiales dans les mémoires tampons constantes à l’aide d’un appel de méthode de contexte de périphérique à __ID3D11DeviceContext :: UpdateSubresource__.
* Étant donné que le chargement des ressources chargées de façon asynchrone est terminé, il est temps de les associer aux objets de jeu appropriés.
* Pour chaque objet de jeu, créez le maillage et le matériau à l’aide des textures qui ont été chargées. Associez ensuite le maillage et le matériau à l’objet de jeu.
* Pour l’objet de jeu targets, la texture composée d’anneaux colorés concentriques, avec une valeur numérique en haut, n’est pas chargée à partir d’un fichier de texture. Au lieu de cela, elle est générée de façon procédurale à l’aide du code dans __TargetTexture. cpp__. La classe __TargetTexture__ crée les ressources nécessaires pour dessiner la texture dans une ressource hors écran au moment de l’initialisation. La texture qui en résulte est ensuite associée aux objets de jeu cibles appropriés.

__FinalizeCreateGameDeviceResources__ et [__CreateWindowSizeDependentResources__](#createwindowsizedependentresource-method) partagent des portions similaires de code pour les éléments suivants :
* Utilisez __SetProjParams__ pour vous assurer que la caméra possède la matrice de projection appropriée. Pour plus d’informations, accédez à la [caméra et](tutorial--assembling-the-rendering-pipeline.md#camera-and-coordinate-space)à l’espace de coordonnées.
* Gérer la rotation d’écran en multipliant la matrice de rotation 3D à la matrice de projection de la caméra. Ensuite, mettez à jour la mémoire tampon constante __ConstantBufferChangeOnResize__ avec la matrice de projection résultante.
* Définissez la variable globale __booléenne__ __m \_ gameResourcesLoaded__ pour indiquer que les ressources sont maintenant chargées dans les tampons, prêt pour l’étape suivante. Rappelez-vous que nous avons d’abord initialisé cette variable avec la __valeur false__ dans la méthode du constructeur de __GameRenderer__, par le biais de la méthode __GameRenderer :: CreateDeviceDependentResources__ . 
* Lorsque ce __ \_ GameResourcesLoaded__ est __true__, le rendu des objets de scène peut avoir lieu. Cela a été abordé dans l' __infrastructure de rendu I : introduction au rendu de__ l’article, sous la [__méthode GameRenderer :: Render__](tutorial--assembling-the-rendering-pipeline.md#gamerendererrender-method).

```cppwinrt
// This method is called from the GameMain constructor.
// Make sure that 2D rendering is occurring on the same thread as the main rendering.
void GameRenderer::FinalizeCreateGameDeviceResources()
{
    // All asynchronously loaded resources have completed loading.
    // Now associate all the resources with the appropriate game objects.
    // This method is expected to run in the same thread as the GameRenderer
    // was created. All work will happen behind the "Loading ..." screen after the
    // main loop has been entered.

    // Initialize the Constant buffer with the light positions
    // These are handled here to ensure that the d3dContext is only
    // used in one thread.

    auto d3dDevice = m_deviceResources->GetD3DDevice();

    ConstantBufferNeverChanges constantBufferNeverChanges;
    constantBufferNeverChanges.lightPosition[0] = XMFLOAT4(3.5f, 2.5f, 5.5f, 1.0f);
    ...
    constantBufferNeverChanges.lightColor = XMFLOAT4(0.25f, 0.25f, 0.25f, 1.0f);

    // CPU copies data from memory (constantBufferNeverChanges) to a subresource 
    // created in non-mappable memory (m_constantBufferNeverChanges) which was created in the earlier 
    // CreateGameDeviceResourcesAsync method. For UpdateSubresource API ref info, 
    // go to: https://msdn.microsoft.com/library/windows/desktop/ff476486.aspx
    // To learn more about what a subresource is, go to:
    // https://msdn.microsoft.com/library/windows/desktop/ff476901.aspx

    m_deviceResources->GetD3DDeviceContext()->UpdateSubresource(
        m_constantBufferNeverChanges.get(),
        0,
        nullptr,
        &constantBufferNeverChanges,
        0,
        0
        );

    // For the objects that function as targets, they have two unique generated textures.
    // One version is used to show that they have never been hit and the other is 
    // used to show that they have been hit.
    // TargetTexture is a helper class to procedurally generate textures for game
    // targets. The class creates the necessary resources to draw the texture into 
    // an off screen resource at initialization time.

    TargetTexture textureGenerator(
        d3dDevice,
        m_deviceResources->GetD2DFactory(),
        m_deviceResources->GetDWriteFactory(),
        m_deviceResources->GetD2DDeviceContext()
        );

    // CylinderMesh is a class derived from MeshObject and creates a ID3D11Buffer of
    // vertices and indices to represent a canonical cylinder (capped at
    // both ends) that is positioned at the origin with a radius of 1.0,
    // a height of 1.0 and with its axis in the +Z direction.
    // In the game sample, there are various types of mesh types:
    // CylinderMesh (vertical rods), SphereMesh (balls that the player shoots), 
    // FaceMesh (target objects), and WorldMesh (Floors and ceilings that define the enclosed area)

    auto cylinderMesh = std::make_shared<CylinderMesh>(d3dDevice, (uint16_t)26);
    ...

    // The Material class maintains the properties that represent how an object will
    // look when it is rendered.  This includes the color of the object, the
    // texture used to render the object, and the vertex and pixel shader that
    // should be used for rendering.

    auto cylinderMaterial = std::make_shared<Material>(
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(0.8f, 0.8f, 0.8f, .5f),
        XMFLOAT4(1.0f, 1.0f, 1.0f, 1.0f),
        15.0f,
        m_cylinderTexture.get(),
        m_vertexShader.get(),
        m_pixelShader.get()
        );

    ...

    // Attach the textures to the appropriate game objects.
    // We'll loop through all the objects that need to be rendered.
    for (auto&& object : m_game->RenderObjects())
    {
        if (object->TargetId() == GameConstants::WorldFloorId)
        {
            // Assign a normal material for the floor object.
            // This normal material uses the floor texture (cellfloor.dds) that was loaded asynchronously from
            // the Assets folder using BasicLoader::LoadTextureAsync method in the earlier 
            // CreateGameDeviceResourcesAsync loop

            object->NormalMaterial(
                std::make_shared<Material>(
                    XMFLOAT4(0.5f, 0.5f, 0.5f, 1.0f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 1.0f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    150.0f,
                    m_floorTexture.get(),
                    m_vertexShaderFlat.get(),
                    m_pixelShaderFlat.get()
                    )
                );
            // Creates a mesh object called WorldFloorMesh and assign it to the floor object.
            object->Mesh(std::make_shared<WorldFloorMesh>(d3dDevice));
        }
        ...
        else if (auto cylinder = dynamic_cast<Cylinder*>(object.get()))
        {
            cylinder->Mesh(cylinderMesh);
            cylinder->NormalMaterial(cylinderMaterial);
        }
        else if (auto target = dynamic_cast<Face*>(object.get()))
        {
            const int bufferLength = 16;
            wchar_t str[bufferLength];
            int len = swprintf_s(str, bufferLength, L"%d", target->TargetId());
            auto string{ winrt::hstring(str, len) };

            winrt::com_ptr<ID3D11ShaderResourceView> texture;
            textureGenerator.CreateTextureResourceView(string, texture.put());
            target->NormalMaterial(
                std::make_shared<Material>(
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    5.0f,
                    texture.get(),
                    m_vertexShader.get(),
                    m_pixelShader.get()
                    )
                );

            texture = nullptr;
            textureGenerator.CreateHitTextureResourceView(string, texture.put());
            target->HitMaterial(
                std::make_shared<Material>(
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.8f, 0.8f, 0.8f, 0.5f),
                    XMFLOAT4(0.3f, 0.3f, 0.3f, 1.0f),
                    5.0f,
                    texture.get(),
                    m_vertexShader.get(),
                    m_pixelShader.get()
                    )
                );

            target->Mesh(targetMesh);
        }
        ...
    }

    // The SetProjParams method calculates the projection matrix based on input params and
    // ensures that the camera has been initialized with the right projection
    // matrix.  
    // The camera is not created at the time the first window resize event occurs.

    auto renderTargetSize = m_deviceResources->GetRenderTargetSize();
    m_game->GameCamera().SetProjParams(
        XM_PI / 2,
        renderTargetSize.Width / renderTargetSize.Height,
        0.01f,
        100.0f
        );

    // Make sure that the correct projection matrix is set in the ConstantBufferChangeOnResize buffer.

    // Get the 3D rotation transform matrix. We are handling screen rotations directly to eliminate an unaligned 
    // fullscreen copy. So it is necessary to post multiply the 3D rotation matrix to the camera's projection matrix
    // to get the projection matrix that we need.

    auto orientation = m_deviceResources->GetOrientationTransform3D();

    ConstantBufferChangeOnResize changesOnResize;

    // The matrices are transposed due to the shader code expecting the matrices in the opposite
    // row/column order from the DirectX math library.

    // XMStoreFloat4x4 takes a matrix and writes the components out to sixteen single-precision floating-point values at the given address. 
    // The most significant component of the first row vector is written to the first four bytes of the address, 
    // followed by the second most significant component of the first row, and so on. The second row is then written out in a 
    // like manner to memory beginning at byte 16, followed by the third row to memory beginning at byte 32, and finally 
    // the fourth row to memory beginning at byte 48. For more API ref info, go to: 
    // https://msdn.microsoft.com/library/windows/desktop/microsoft.directx_sdk.storing.xmstorefloat4x4.aspx

    XMStoreFloat4x4(
        &changesOnResize.projection,
        XMMatrixMultiply(
            XMMatrixTranspose(m_game->GameCamera().Projection()),
            XMMatrixTranspose(XMLoadFloat4x4(&orientation))
            )
        );

    // UpdateSubresource method instructs CPU to copy data from memory (changesOnResize) to a subresource 
    // created in non-mappable memory (m_constantBufferChangeOnResize ) which was created in the earlier 
    // CreateGameDeviceResourcesAsync method.

    m_deviceResources->GetD3DDeviceContext()->UpdateSubresource(
        m_constantBufferChangeOnResize.get(),
        0,
        nullptr,
        &changesOnResize,
        0,
        0
        );

    // Finally we set the m_gameResourcesLoaded as TRUE, so we can start rendering.
    m_gameResourcesLoaded = true;
}
```

## <a name="createwindowsizedependentresource-method"></a>Méthode CreateWindowSizeDependentResource

Les méthodes CreateWindowSizeDependentResources sont appelées chaque fois que la taille de la fenêtre, l’orientation, le rendu stéréo ou la résolution change. Dans l’exemple de jeu, elle met à jour la matrice de projection dans __ConstantBufferChangeOnResize__.

Les ressources de taille de fenêtre sont mises à jour de cette manière : 
* L’infrastructure d’application obtient l’un des différents événements possibles indiquant une modification de l’état de la fenêtre. 
* Votre principale boucle de jeu est ensuite informée de l’événement et appelle __CreateWindowSizeDependentResources__ sur l’instance de classe principale (__GameMain__), qui appelle ensuite l’implémentation __CreateWindowSizeDependentResources__ dans la classe de convertisseur de jeu (__GameRenderer__).
* Le travail principal de cette méthode consiste à s’assurer que les éléments visuels ne sont pas confondus ou non valides en raison d’une modification des propriétés de la fenêtre.

Pour cet exemple de jeu, un certain nombre d’appels de méthode sont les mêmes que la méthode [__FinalizeCreateGameDeviceResources__](#finalizecreategamedeviceresources-method) . Pour la procédure pas à pas de code, consultez la section précédente.

Les réglages de la taille de la fenêtre superposition et de la fenêtre superposée sont traités dans [Ajouter une interface utilisateur](tutorial--adding-a-user-interface.md).

```cppwinrt
// Initializes view parameters when the window size changes.
void GameRenderer::CreateWindowSizeDependentResources()
{
    // Game HUD and overlay window size rendering adjustments are done here
    // but they'll be covered in the UI section instead.

    m_gameHud.CreateWindowSizeDependentResources();

    ...

    auto d3dContext = m_deviceResources->GetD3DDeviceContext();
    // In Sample3DSceneRenderer::CreateWindowSizeDependentResources, we had:
    // Size outputSize = m_deviceResources->GetOutputSize();

    auto renderTargetSize = m_deviceResources->GetRenderTargetSize();

    ...

    m_gameInfoOverlay.CreateWindowSizeDependentResources(m_gameInfoOverlaySize);

    if (m_game != nullptr)
    {
        // Similar operations as the last section of FinalizeCreateGameDeviceResources method
        m_game->GameCamera().SetProjParams(
            XM_PI / 2, renderTargetSize.Width / renderTargetSize.Height,
            0.01f,
            100.0f
            );

        XMFLOAT4X4 orientation = m_deviceResources->GetOrientationTransform3D();

        ConstantBufferChangeOnResize changesOnResize;
        XMStoreFloat4x4(
            &changesOnResize.projection,
            XMMatrixMultiply(
                XMMatrixTranspose(m_game->GameCamera().Projection()),
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
}
```

## <a name="next-steps"></a>Étapes suivantes

Il s’agit du processus de base pour implémenter l’infrastructure de rendu graphique d’un jeu. Plus votre jeu est grand, plus vous devez mettre en place des abstractions pour gérer les hiérarchies de types d’objets et les comportements d’animation. Vous devez implémenter des méthodes plus complexes pour le chargement et la gestion des ressources telles que les maillages et les textures. Voyons ensuite comment [Ajouter une interface utilisateur](tutorial--adding-a-user-interface.md).