---
title: Gérer des scénarios de suppression de périphériques dans Direct3D 11
description: Cette rubrique explique comment recréer la chaîne d’interface d’appareils Direct3D et DXGI quand la carte graphique est supprimée ou réinitialisée.
ms.assetid: 8f905acd-08f3-ff6f-85a5-aaa99acb389a
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, jeux, DirectX 11, appareil perdu
ms.localizationpriority: medium
ms.openlocfilehash: 49356b910879f96d607a8bfeb2586b8da0ea3c69
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173153"
---
# <a name="span-iddev_gaminghandling_device-lost_scenariosspanhandle-device-removed-scenarios-in-direct3d-11"></a><span id="dev_gaming.handling_device-lost_scenarios"></span>Gérer des scénarios de suppression d’appareils dans Direct3D 11



Cette rubrique explique comment recréer la chaîne d’interface d’appareils Direct3D et DXGI quand la carte graphique est supprimée ou réinitialisée.

Dans DirectX 9, les applications peuvent présenter un état de type « [périphérique perdu](/windows/desktop/direct3d9/lost-devices) » quand un périphérique D3D passe à un état non opérationnel. Par exemple, lorsqu’une application Direct3D 9 en mode plein écran perd le focus, le périphérique Direct3D est « perdu ». Toute tentative de dessin avec un périphérique perdu entraîne un échec sans avertissement. Direct3D 11 utilise des interfaces GDI (Graphics Device Interface) virtuelles, ce qui permet à plusieurs programmes de partager le même périphérique graphique physique et d’éviter les situations où les applications perdent le contrôle du périphérique Direct3D. Cependant, la disponibilité de la carte graphique peut toujours changer. Par exemple :

-   Le pilote graphique est mis à jour.
-   Le système passe d’une carte graphique axée sur l’économie d’énergie à une carte graphique axée sur les performances.
-   Le périphérique graphique cesse de répondre et est réinitialisé.
-   Une carte graphique est attachée ou supprimée physiquement.

Dans ces circonstances, DXGI retourne un code d’erreur indiquant que le périphérique Direct3D doit être réinitialisé et que les ressources de périphérique doivent être recréées. Cette procédure pas à pas montre comment les applications et les jeux Direct3D 11 peuvent détecter ces situations et réagir de manière appropriée quand la carte graphique est réinitialisée, supprimée ou modifiée. Des exemples de code sont fournis à partir des modèles d’applications DirectX 11 (Windows universelles) inclus dans Microsoft Visual Studio 2015.

## <a name="instructions"></a>Instructions

### <a name="spanspanstep-1"></a><span></span>Étape 1 :

Incluez une recherche de l’erreur relative à une suppression d’appareil dans la boucle de rendu. Présentez le frame en appelant [**IDXGISwapChain ::P renvoyée**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) (ou [**Present1**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1), etc.). Vérifiez ensuite s’il a retourné [**dxgi \_ erreur de \_ périphérique \_ supprimé**](/windows/desktop/direct3ddxgi/dxgi-error) ou la ** \_ \_ \_ réinitialisation du périphérique d’erreur dxgi**.

Tout d’abord, le modèle stocke le HRESULT retourné par la chaîne de permutation DXGI :

```cpp
HRESULT hr = m_swapChain->Present(1, 0);
```

Après avoir géré les autres travaux de présentation de l’image, le modèle vérifie s’il existe une erreur relative à la suppression de périphérique. Si nécessaire, il appelle une méthode pour gérer l’état de suppression du périphérique :

```cpp
// If the device was removed either by a disconnection or a driver upgrade, we
// must recreate all device resources.
if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
{
    HandleDeviceLost();
}
else
{
    DX::ThrowIfFailed(hr);
}
```

### <a name="step-2"></a>Étape 2 :

Incluez également une recherche de l’erreur relative à une suppression de périphérique en réponse aux changements de taille de fenêtre. Il s’agit d’un bon emplacement pour vérifier la suppression de l' [** \_ appareil d’erreur \_ \_ dxgi**](/windows/desktop/direct3ddxgi/dxgi-error) ou la ** \_ \_ \_ réinitialisation de l’appareil dxgi** pour plusieurs raisons :

-   Le redimensionnement de la chaîne de permutation nécessite un appel à l’adaptateur DXGI sous-jacent, ce qui peut retourner l’erreur relative à une suppression de périphérique.
-   L’application peut avoir été déplacée vers un moniteur attaché à un autre périphérique graphique.
-   Quand un appareil graphique est supprimé ou réinitialisé, la résolution du Bureau change le plus souvent, ce qui entraîne un changement de taille de fenêtre.

Le modèle vérifie le HRESULT retourné par [**ResizeBuffers**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-resizebuffers):

```cpp
// If the swap chain already exists, resize it.
HRESULT hr = m_swapChain->ResizeBuffers(
    2, // Double-buffered swap chain.
    static_cast<UINT>(m_d3dRenderTargetSize.Width),
    static_cast<UINT>(m_d3dRenderTargetSize.Height),
    DXGI_FORMAT_B8G8R8A8_UNORM,
    0
    );

if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
{
    // If the device was removed for any reason, a new device and swap chain will need to be created.
    HandleDeviceLost();

    // Everything is set up now. Do not continue execution of this method. HandleDeviceLost will reenter this method 
    // and correctly set up the new device.
    return;
}
else
{
    DX::ThrowIfFailed(hr);
}
```

### <a name="step-3"></a>Étape 3 :

Chaque fois que votre application reçoit l’erreur [**dxgi \_ erreur de \_ \_ suppression**](/windows/desktop/direct3ddxgi/dxgi-error) de l’appareil, elle doit réinitialiser l’appareil Direct3D et recréer toutes les ressources dépendantes de l’appareil. Libérez les références aux ressources d’appareil graphique créées avec le précédent appareil Direct3D. Ces ressources ne sont plus valides. Toutes les références à la chaîne de permutation doivent être libérées pour en permettre la création d’une autre.

La méthode HandleDeviceLost libère la chaîne d’échange et indique aux composants d’application de libérer les ressources de périphérique :

```cpp
m_swapChain = nullptr;

if (m_deviceNotify != nullptr)
{
    // Notify the renderers that device resources need to be released.
    // This ensures all references to the existing swap chain are released so that a new one can be created.
    m_deviceNotify->OnDeviceLost();
}
```

Elle crée ensuite une chaîne d’échange, puis réinitialise les ressources dépendantes du périphérique contrôlées par la classe de gestion de périphérique :

```cpp
// Create the new device and swap chain.
CreateDeviceResources();
m_d2dContext->SetDpi(m_dpi, m_dpi);
CreateWindowSizeDependentResources();
```

Une fois que le périphérique et la chaîne d’échange ont été rétablis, la méthode indique aux composants d’application de réinitialiser les ressources dépendantes du périphérique :

```cpp
// Create the new device and swap chain.
CreateDeviceResources();
m_d2dContext->SetDpi(m_dpi, m_dpi);
CreateWindowSizeDependentResources();

if (m_deviceNotify != nullptr)
{
    // Notify the renderers that resources can now be created again.
    m_deviceNotify->OnDeviceRestored();
}
```

À la fin de la méthode HandleDeviceLost, le contrôle retourne à la boucle de rendu, qui continue de dessiner l’image suivante.

## <a name="remarks"></a>Remarques


### <a name="investigating-the-cause-of-device-removed-errors"></a>Recherche de l’origine des erreurs relatives à une suppression de périphérique

La répétition d’erreurs relatives à une suppression de périphérique DXGI peut indiquer que votre code graphique crée des conditions non valides durant une routine de dessin. Elle peut également indiquer une défaillance matérielle ou un bogue dans le pilote graphique. Pour rechercher l’origine des erreurs relatives à une suppression de périphérique, appelez [**ID3D11Device::GetDeviceRemovedReason**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-getdeviceremovedreason) avant de libérer le périphérique Direct3D. Cette méthode retourne l’un des six codes d’erreur DXGI possibles qui indiquent l’origine de l’erreur relative à une suppression de périphérique :

-   **Dxgi \_ Périphérique d’erreur \_ \_ bloqué**: le pilote Graphics a cessé de répondre en raison d’une combinaison non valide de commandes graphiques envoyées par l’application. Si vous obtenez cette erreur à plusieurs reprises, cela indique que votre application est probablement à l’origine de la défaillance du périphérique et qu’elle doit être déboguée.
-   **Dxgi \_ Périphérique d’erreur \_ \_ supprimé**: le périphérique graphique a été physiquement supprimé, désactivé ou une mise à niveau du pilote s’est produite. Cela arrive parfois et c’est une situation normale. Votre application ou votre jeu doit recréer les ressources de périphérique, comme cela est décrit dans cette rubrique.
-   **Dxgi \_ ERREUR \_ de \_ réinitialisation de périphérique**: le périphérique graphique a échoué en raison d’une commande incorrecte. Si vous obtenez cette erreur à plusieurs reprises, cela peut signifier que votre code envoie des commandes de dessin non valides.
-   **Dxgi \_ Erreur \_ \_ interne du \_ pilote d’erreur**: le pilote Graphics a rencontré une erreur et a réinitialisé l’appareil.
-   **Dxgi \_ ERREUR \_ d' \_ appel non valide**: l’application a fourni des données de paramètre non valides. Si vous obtenez cette erreur, même une seule fois, cela signifie que votre code est à l’origine de l’état de suppression du périphérique et qu’il doit être débogué.
-   **S \_ OK**: retourné lorsqu’un périphérique graphique a été activé, désactivé ou réinitialisé sans invalider le périphérique graphique actuel. Par exemple, ce code d’erreur peut être retourné si une application utilise [WARP (Windows Advanced Rasterization Platform)](/windows/desktop/direct3darticles/directx-warp) et si un adaptateur matériel est disponible.

Le code suivant récupère le code d’erreur [**dxgi \_ Error \_ Device \_ supprimé**](/windows/desktop/direct3ddxgi/dxgi-error) et l’imprime dans la console de débogage. Insérez ce code au début de la méthode HandleDeviceLost :

```cpp
    HRESULT reason = m_d3dDevice->GetDeviceRemovedReason();

#if defined(_DEBUG)
    wchar_t outString[100];
    size_t size = 100;
    swprintf_s(outString, size, L"Device removed! DXGI_ERROR code: 0x%X\n", reason);
    OutputDebugStringW(outString);
#endif
```

Pour plus d’informations, consultez [**GetDeviceRemovedReason**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-getdeviceremovedreason) et [**dxgi \_ Error**](/windows/desktop/direct3ddxgi/dxgi-error).

### <a name="testing-device-removed-handling"></a>Gestion de l’appareil de test supprimé

L’invite de commandes de développeur de Visual Studio prend en charge un outil de ligne de commande « dxcap » pour la capture et la lecture d’événement Direct3D en rapport avec les diagnostics des graphiques Visual Studio. Vous pouvez utiliser l’option de ligne de commande « -forcetdr » pendant que votre application est en cours d’exécution, ce qui force un événement de détection et de récupération du délai GPU, déclenchant ainsi un \_ appareil d’erreur dxgi \_ \_ supprimé et vous permettant de tester votre code de gestion des erreurs.

> **Remarque** DXCap et ses DLL de prise en charge sont installés sur system32/syswow64 en tant qu’outils graphiques pour Windows 10, qui ne sont plus distribués via le Kit de développement logiciel (SDK) Windows. Ils sont désormais fournis via la fonctionnalité Outils graphiques à la demande, un composant facultatif de système d’exploitation qui doit être installé afin d’activer et d’utiliser les outils graphiques sous Windows 10. Vous trouverez plus d’informations sur l’installation des outils Graphics pour Windows 10 ici : <https://msdn.microsoft.com/library/mt125501.aspx#InstallGraphicsTools>