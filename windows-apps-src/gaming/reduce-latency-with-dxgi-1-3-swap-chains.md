---
title: Réduire la latence avec des chaînes d’échange DXGI 1.3
description: Utilisez DXGI 1.3 pour réduire la latence d’image effective en attendant que la chaîne d’échange indique le moment approprié pour débuter le rendu d’une nouvelle image.
ms.assetid: c99b97ed-a757-879f-3d55-7ed77133f6ce
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, jeux, latence, DXGI, chaînes de permutation, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: 27ecce9d95d3c2e852b049e3cac9579850022df9
ms.sourcegitcommit: d2aabe027a2fff8a624111a00864d8986711cae6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2020
ms.locfileid: "82880859"
---
# <a name="reduce-latency-with-dxgi-13-swap-chains"></a>Réduire la latence avec des chaînes d’échange DXGI 1.3

Utilisez DXGI 1.3 pour réduire la latence d’image effective en attendant que la chaîne d’échange indique le moment approprié pour débuter le rendu d’une nouvelle image. Normalement, les jeux doivent offrir la latence la plus faible possible entre le moment où l’entrée du joueur est reçue et le moment où le jeu répond à cette entrée en mettant à jour l’affichage. Cette rubrique décrit une technique disponible à partir de Direct3D 11.2, qui vous permet de réduire la latence d’image effective dans votre jeu.

## <a name="how-does-waiting-on-the-back-buffer-reduce-latency"></a>Comment la mise en file d’attente en mémoire tampon d’arrière-plan peut-elle réduire la latence ?

Avec la chaîne de permutation du modèle, les « retournements » de la mémoire tampon d’arrière-plan sont mis en file d’attente chaque fois que votre jeu appelle [**IDXGISwapChain ::P renvoyé**](/windows/win32/api/dxgi/nf-dxgi-idxgiswapchain-present). Quand la boucle de rendu appelle Present(), le système bloque le thread jusqu’à ce qu’il ait fini de présenter une image précédente, ce qui libère de l’espace pour la mise en file d’attente de la nouvelle image, avant la présentation réelle. Cela entraîne une latence supplémentaire entre le moment où le jeu dessine une image et le moment où le système lui permet d’afficher cette image. Bien souvent, le système atteint un état d’équilibre quand le jeu attend une image supplémentaire complète entre le moment du rendu et la présentation de chaque image. Il est préférable d’attendre que le système soit prêt à accepter une nouvelle image, puis d’effectuer le rendu en fonction des données actuelles et de mettre immédiatement l’image en file d’attente.

Créer une chaîne de permutation qui est attendue avec l’indicateur d' [**\_\_\_objet d’attente de la latence de la\_chaîne\_\_\_d’échange dxgi**](/windows/win32/api/dxgi/ne-dxgi-dxgi_swap_chain_flag) . Les chaînes d’échange créées de cette manière peuvent informer votre boucle de rendu, une fois que le système est prêt à accepter une nouvelle image. Cela permet à votre jeu d’effectuer le rendu en fonction des données actuelles, puis de placer le résultat immédiatement en file d’attente de présentation.

## <a name="step-1-create-a-waitable-swap-chain"></a>Étape 1. Créer une chaîne de permutation qui est attendue

Spécifiez l’indicateur d' [**\_\_\_\_objet\_de\_\_latence**](/windows/win32/api/dxgi/ne-dxgi-dxgi_swap_chain_flag) d’un frame de la chaîne d’échange dxgi quand vous appelez [**CreateSwapChainForCoreWindow**](/windows/win32/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow).

```cpp
swapChainDesc.Flags = DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT; // Enable GetFrameLatencyWaitableObject().
```

> [!NOTE]
> Contrairement à certains indicateurs, cet indicateur ne peut pas être ajouté ou supprimé à l’aide de [**ResizeBuffers**](/windows/win32/api/dxgi/nf-dxgi-idxgiswapchain-resizebuffers). DXGI renvoie un code d’erreur si cet indicateur n’est pas le même qu’au moment de la création de la chaîne d’échange.

```cpp
// If the swap chain already exists, resize it.
HRESULT hr = m_swapChain->ResizeBuffers(
    2, // Double-buffered swap chain.
    static_cast<UINT>(m_d3dRenderTargetSize.Width),
    static_cast<UINT>(m_d3dRenderTargetSize.Height),
    DXGI_FORMAT_B8G8R8A8_UNORM,
    DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT // Enable GetFrameLatencyWaitableObject().
    );
```

## <a name="step-2-set-the-frame-latency"></a>Étape 2. Définir la latence de trame

Définissez la latence de frame avec l’API [**IDXGISwapChain2 :: SetMaximumFrameLatency**](/windows/win32/api/dxgi1_3/nf-dxgi1_3-idxgiswapchain2-setmaximumframelatency) , au lieu d’appeler [**IDXGIDevice1 :: SetMaximumFrameLatency**](/windows/win32/api/dxgi/nf-dxgi-idxgidevice1-setmaximumframelatency).

Par défaut, la valeur de latence d’image pour les chaînes d’échange d’attente est égale à 1, ce qui correspond à la latence la plus faible. Toutefois, cela réduit également le parallélisme entre l’UC et le processeur graphique. Si vous avez besoin d’un parallélisme plus important entre l’UC et le processeur graphique afin d’atteindre 60 FPS (en d’autres termes, si l’UC et le processeur graphique consacrent chacun moins de 16,7 ms au rendu d’une image, mais qu’ils consacrent à eux deux plus de 16,7 ms), affectez la valeur 2 à la latence d’image. Cela permet au processeur graphique de traiter les travaux mis en file d’attente par l’UC durant le traitement de l’image précédente, tout en permettant à l’UC d’envoyer les commandes de rendu de l’image actuelle de façon indépendante.

```cpp
// Swapchains created with the DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT flag use their
// own per-swapchain latency setting instead of the one associated with the DXGI device. The
// default per-swapchain latency is 1, which ensures that DXGI does not queue more than one frame
// at a time. This both reduces latency and ensures that the application will only render after
// each VSync, minimizing power consumption.
//DX::ThrowIfFailed(
//    swapChain2->SetMaximumFrameLatency(1)
//    );
```

## <a name="step-3-get-the-waitable-object-from-the-swap-chain"></a>Étape 3. Obtient l’objet d’attente à partir de la chaîne de permutation

Appelez [**IDXGISwapChain2 :: GetFrameLatencyWaitableObject**](/windows/win32/api/dxgi1_3/nf-dxgi1_3-idxgiswapchain2-getframelatencywaitableobject) pour récupérer le handle d’attente. Le handle d’attente est un pointeur vers l’objet d’attente. Conservez ce handle afin qu’il soit utilisé par votre boucle de rendu.

```cpp
// Get the frame latency waitable object, which is used by the WaitOnSwapChain method. This
// requires that swap chain be created with the DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT
// flag.
m_frameLatencyWaitableObject = swapChain2->GetFrameLatencyWaitableObject();
```

## <a name="step-4-wait-before-rendering-each-frame"></a>Étape 4. Attendre avant de restituer chaque frame

Votre boucle de rendu doit attendre le signal de la chaîne de permutation via l’objet d’attente avant de commencer à effectuer le rendu de chaque image. Cela inclut la première image rendue avec la chaîne d’échange. Utilisez [**WaitForSingleObjectEx**](/windows/win32/api/synchapi/nf-synchapi-waitforsingleobjectex), en fournissant le handle d’attente récupéré à l’étape 2, pour signaler le début de chaque image.

L’exemple ci-après illustre la boucle de rendu de l’exemple DirectXLatency :

```cpp
while (!m_windowClosed)
{
    if (m_windowVisible)
    {
        // Block this thread until the swap chain is finished presenting. Note that it is
        // important to call this before the first Present in order to minimize the latency
        // of the swap chain.
        m_deviceResources->WaitOnSwapChain();

        // Process any UI events in the queue.
        CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

        // Update app state in response to any UI events that occurred.
        m_main->Update();

        // Render the scene.
        m_main->Render();

        // Present the scene.
        m_deviceResources->Present();
    }
    else
    {
        // The window is hidden. Block until a UI event occurs.
        CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
    }
}
```

L’exemple suivant montre l’appel de WaitForSingleObjectEx à partir de l’exemple DirectXLatency :

```cpp
// Block the current thread until the swap chain has finished presenting.
void DX::DeviceResources::WaitOnSwapChain()
{
    DWORD result = WaitForSingleObjectEx(
        m_frameLatencyWaitableObject,
        1000, // 1 second timeout (shouldn't ever occur)
        true
        );
}
```

## <a name="what-should-my-game-do-while-it-waits-for-the-swap-chain-to-present"></a>Que doit faire mon jeu pendant qu’il attend la présentation de la chaîne de permutation ?

Si votre jeu n’a aucune tâche qui bloque la boucle de rendu, il peut être judicieux de le laisser attendre la présentation de la chaîne de permutation, car cela permet d’économiser de l’énergie, ce qui est particulièrement important sur les appareils mobiles. Sinon, utilisez le multithreading pour exécuter le travail pendant que votre jeu attend la présentation de la chaîne de permutation. Voici quelques tâches que votre jeu peut effectuer :

-   Traitement des événements réseau
-   Mise à jour de l’IA
-   Calculs de physique basés sur l’UC
-   Rendu de contexte différé (sur les appareils compatibles)
-   Chargement de ressources

Pour plus d’informations sur la programmation multithread dans Windows, voir les rubriques connexes suivantes.

## <a name="related-topics"></a>Rubriques connexes

* [Exemple d’application de latence DirectX](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample/DirectX%20latency%20sample)
* [**IDXGISwapChain2::GetFrameLatencyWaitableObject**](/windows/win32/api/dxgi1_3/nf-dxgi1_3-idxgiswapchain2-getframelatencywaitableobject)
* [**WaitForSingleObjectEx**](/windows/win32/api/synchapi/nf-synchapi-waitforsingleobjectex)
* [**Windows.System.Threading**](/uwp/api/Windows.System.Threading)
* [Programmation asynchrone en C++](/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps)
* [Processus et threads](/windows/win32/procthread/processes-and-threads)
* [Synchronization](/windows/win32/sync/synchronization)
* [Utilisation d’objets Event (Windows)](/windows/win32/sync/using-event-objects)