---
title: Planifier votre projet de portage d’OpenGL ES 2.0 vers Direct3D
description: Si vous portez un jeu à partir des plateformes iOS ou Android, vous avez probablement effectué un investissement considérable dans OpenGL ES 2.0.
ms.assetid: a31b8c5a-5577-4142-fc60-53217302ec3a
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, jeux, OpenGL, Direct3D
ms.localizationpriority: medium
ms.openlocfilehash: dddbf3a1009ccbaeb7fd0695d995cd17ba6182d3
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165343"
---
# <a name="plan-your-port-from-opengl-es-20-to-direct3d"></a>Planifier votre projet de portage d’OpenGL ES 2.0 vers Direct3D




**API importantes**

-   [Direct3D 11](/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11)
-   [Visual C++](/previous-versions/60k1461a(v=vs.140))

Si vous portez un jeu à partir des plateformes iOS ou Android, vous avez probablement effectué un investissement considérable dans OpenGL ES 2.0. Avant de déplacer le code base de votre pipeline graphique vers Direct3D 11 et le Windows Runtime, certains points sont à prendre en compte.

La plupart des efforts de portage concernent généralement le passage du code base et le mappage des API et modèles courants entre les deux modèles. Vous trouverez ce processus un peu plus simple après avoir lu cette rubrique.

Voici certains points à prendre en considération dans le portage des graphiques d’OpenGL ES 2.0 vers Direct3D 11.

## <a name="notes-on-specific-opengl-es-20-providers"></a>Remarques sur les fournisseurs OpenGL ES 2.0 spécifiques


Les rubriques qui abordent le portage dans cette section font référence à l’implémentation Windows des spécifications OpenGL ES 2.0 créées par Khronos Group. Tous les exemples de code OpenGL ES 2.0 ont été développés à l’aide de Visual Studio 2012 et de la syntaxe C Windows de base. Si vous effectuez un portage à partir d’un code base Objective-C (iOS) ou Java (Android), les exemples de code OpenGL ES 2.0 peuvent ne pas utiliser la même syntaxe ou les mêmes paramètres d’appel des API. Ces recommandations tentent d’adopter un point de vue aussi indépendant que possible du type de plateforme utilisé.

Cette documentation n’utilise que les API de spécification 2.0 pour le code et les références OpenGL ES. Si vous effectuez un portage à partir d’OpenGL ES 1.1 ou 3.0, ce contenu peut vous être utile, mais certains des exemples de code ou de contexte OpenGL ES 2.0 ne vous seront peut-être pas familiers.

Les exemples Direct3D 11 de ces rubriques utilisent Microsoft Windows C++ avec extensions de composant (CX). Pour plus d’informations sur cette version de la syntaxe C++, consultez [Visual C++](/previous-versions/60k1461a(v=vs.140)), [extensions de composant pour les plateformes Runtime](/cpp/windows/component-extensions-for-runtime-platforms)et [aide-mémoire (C++ \\ CX)](/cpp/cppcx/quick-reference-c-cx).

## <a name="understand-your-hardware-requirements-and-resources"></a>Explorer la configuration requise et vos ressources matérielles


L’ensemble des fonctionnalités de traitement graphique prises en charge par OpenGL ES 2.0 correspondent à peu près aux fonctionnalités fournies dans Direct3D 9.1. Si vous voulez tirer parti des fonctionnalités plus avancées proposées dans Direct3D 11, consultez la documentation [Direct3D 11](/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11) au moment de planifier votre portage ou les rubriques [Portage de DirectX 9 vers le Windows Store](porting-your-directx-9-game-to-windows-store.md) une fois la mise en place terminée.

Pour simplifier la mise en place de votre portage, commencez avec un modèle Visual Studio Direct3D. Il fournit un rendu basique pré-configuré et prend en charge des fonctionnalités d’application Windows Store comme la recréation de ressources suite à des modifications de fenêtre et les niveaux de fonctionnalité Direct3D.

## <a name="understand-direct3d-feature-levels"></a>Explorer les niveaux de fonctionnalité Direct3D


Direct3D 11 prend en charge le matériel « niveaux de fonctionnalité » de 9 \_ 1 (Direct3D 9,1) pour 11 \_ 1. Ces niveaux de fonctionnalité indiquent la disponibilité de certaines fonctionnalités et ressources graphiques. En règle générale, la plupart des plateformes OpenGL ES 2,0 prennent en charge un ensemble de fonctionnalités Direct3D 9,1 (Feature Level 9 \_ 1).

## <a name="review-directx-graphics-features-and-apis"></a>Parcourir les fonctionnalités graphiques et les API DirectX


| Famille d’API                                                | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-----------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [DXGI](/windows/desktop/direct3ddxgi/dx-graphics-dxgi)                     | L’infrastructure DXGI (DirectX Graphics Infrastructure) constitue une interface entre le matériel vidéo et Direct3D. Il définit l’adaptateur pour appareil mobile et la configuration matérielle à l’aide des interfaces COM [**IDXGIAdapter**](/windows/desktop/api/dxgi/nn-dxgi-idxgiadapter) et [**IDXGIDevice1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgidevice2) . Utilisez-la pour créer et configurer vos tampons et autres ressources Windows. De toute évidence, le modèle de fabrique [**IDXGIFactory2**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2) est utilisé pour acquérir des ressources graphiques, y compris la chaîne de permutation (ensemble de tampons de trame). Dans la mesure où DXGI détient la chaîne de permutation, l’interface [**IDXGISwapChain1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1) sert à présenter les trames à l’écran. |
| [Direct3D](/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11)       | Direct3D est l’ensemble d’API qui fournit une représentation virtuelle de l’interface graphique et vous permet de dessiner des graphiques. Version 11 est à peu près comparable à OpenGL 4.3 du point de vue des fonctionnalités. (OpenGL ES 2.0, pour sa part, est similaire à DirectX9, du point de vue des fonctionnalités, et à OpenGL 2.0, mais accompagné du pipeline nuanceur unifié d’OpenGL 3.0). Une grande partie du travail est effectuée dans les interfaces ID3D11Device1 et ID3D11DeviceContext1 qui fournissent respectivement un accès aux ressources et sous-ressources individuelles, et au contexte de rendu.                                                                                                                                          |
| [Direct2D](/windows/desktop/Direct2D/direct2d-portal)                      | Direct2D fournit un ensemble d’API pour le rendu 2D à accélération graphique. Sa fonction est comparable à celle d’OpenVG.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| [DirectWrite](/windows/desktop/DirectWrite/direct-write-portal)            | DirectWrite fournit un ensemble d’API pour le rendu de polices haute qualité, à accélération graphique.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| [DirectXMath](/windows/desktop/dxmath/directxmath-portal)                  | DirectXMath fournit un ensemble d’API et de macros pour la gestion des types, valeurs et fonctions courants d’algèbre linéaire et de trigonométrie. Ces types et fonctions sont conçus pour fonctionner correctement avec Direct3D et ses opérations de nuanceur.                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| [DirectX HLSL](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-common-core) | Syntaxe HLSL actuelle utilisée par les nuanceurs Direct3D. Elle implémente le modèle de nuanceur 5.0 de Direct3D.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |

 

## <a name="review-the-windows-runtime-apis-and-template-library"></a>Parcourir les API et la bibliothèque de modèles de Windows Runtime


Les API Windows Runtime fournissent l’infrastructure générale des applications UWP. Parcourez-les [ici](/uwp/api/).

Les API Windows Runtime essentielles utilisées dans le portage de votre pipeline graphique sont notamment les suivantes :

-   [**Windows :: UI :: Core :: CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow)
-   [**Windows :: UI :: Core :: CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher)
-   [**Windows::ApplicationModel::Core::IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView)
-   [**Windows::ApplicationModel::Core::CoreApplicationView**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView)

Par ailleurs, la bibliothèque de modèles C++ Windows Runtime (WRL) est une bibliothèque de modèles qui fournit un moyen de bas niveau pour créer et utiliser des composants Windows Runtime. Les API Direct3D 11 pour les applications Windows Store doivent être utilisées de préférence avec les interfaces et types de cette bibliothèque, comme les pointeurs intelligents ([ComPtr](/cpp/windows/comptr-class)). Pour plus d’informations sur WRL, voir [Bibliothèque de modèles C++ Windows Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl).

## <a name="change-your-coordinate-system"></a>Changer votre système de coordonnées


La différence qui perturbe parfois les premiers efforts de portage est le passage d’un système de coordonnées droitier traditionnel d’OpenGL au système de coordonnées gaucher par défaut de Direct3D. Ce changement de modélisation des coordonnées affecte plusieurs parties de votre jeu, depuis l’installation et la configuration de vos tampons de vertex jusqu’à de nombreuses fonctions mathématiques de votre matrice. Les deux changements les plus importants à effectuer sont les suivants :

-   Inversez l’ordre des vertex des triangles de sorte que Direct3D les parcoure dans le sens des aiguilles d’une montre depuis l’avant. Par exemple, si vos vertex sont indexés en tant que 0, 1 et 2 dans votre pipeline OpenGL, passez-les à Direct3D en tant que 0, 2, 1.
-   Utilisez la matrice globale pour la mise à l’échelle de l’espace du monde à -1.0f dans la direction de l’axe z, en inversant les coordonnées de l’axe z. Pour ce faire, inversez le signe des valeurs des positions M31, M32 et M33 dans votre matrice globale (lors de son portage vers le type [**Matrix**](/windows/desktop/direct3d9/matrix4x4)). Si M34 n’est pas égal à zéro, inversez aussi son signe.

Toutefois, Direct3D peut prendre en charge un système de coordonnées droitier. DirectXMath fournit un nombre de fonctions compatibles avec les systèmes de coordonnées droitier et gaucher. Elles peuvent être utilisées pour préserver certaines de vos données de maillage d’origine et le traitement des matrices. Elles comprennent :

| Fonction de matrice DirectXMath                                                   | Description                                                                                                                 |
|-------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|
| [**XMMatrixLookAtLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixlookatlh)                               | Crée une matrice globale pour un système de coordonnées gaucher à l’aide d’une position de caméra, une direction vers le haut et un point focal.       |
| [**XMMatrixLookAtRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixlookatrh)                               | Crée une matrice globale pour un système de coordonnées droitier à l’aide d’une position de caméra, une direction vers le haut et un point focal.      |
| [**XMMatrixLookToLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixlooktolh)                               | Crée une matrice globale pour un système de coordonnées gaucher à l’aide d’une position de caméra, une direction vers le haut et une direction de caméra.  |
| [**XMMatrixLookToRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixlooktorh)                               | Crée une matrice globale pour un système de coordonnées droitier à l’aide d’une position de caméra, une direction vers le haut et une direction de caméra. |
| [**XMMatrixOrthographicLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixorthographiclh)                   | Crée une matrice de projection orthogonale pour un système de coordonnées gaucher.                                                 |
| [**XMMatrixOrthographicOffCenterLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixorthographicoffcenterlh) | Crée une matrice de projection orthogonale personnalisée pour un système de coordonnées gaucher.                                           |
| [**XMMatrixOrthographicOffCenterRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixorthographicoffcenterrh) | Crée une matrice de projection orthogonale personnalisée pour un système de coordonnées droitier.                                          |
| [**XMMatrixOrthographicRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixorthographicrh)                   | Crée une matrice de projection orthogonale pour un système de coordonnées droitier.                                                |
| [**XMMatrixPerspectiveFovLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectivefovlh)               | Crée une matrice de projection de perspective pour un système gaucher en fonction d’un champ de vue.                                                |
| [**XMMatrixPerspectiveFovRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectivefovrh)               | Crée une matrice de projection de perspective pour un système droitier en fonction d’un champ de vue.                                               |
| [**XMMatrixPerspectiveLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectivelh)                     | Crée une matrice de projection de perspective pour un système gaucher.                                                                         |
| [**XMMatrixPerspectiveOffCenterLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectiveoffcenterlh)   | Crée une version personnalisée d’une matrice de projection de perspective pour un système gaucher.                                                     |
| [**XMMatrixPerspectiveOffCenterRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectiveoffcenterrh)   | Crée une version personnalisée d’une matrice de projection de perspective pour un système droitier.                                                    |
| [**XMMatrixPerspectiveRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectiverh)                     | Crée une matrice de projection de perspective pour un système droitier.                                                                        |

 

## <a name="opengl-es20-to-direct3d-11-porting-frequently-asked-questions"></a>Portage OpenGL ES 2.0 vers Direct3D 11 : Forum Aux Questions


-   Question : « En général, puis-je rechercher des chaînes ou modèles dans mon code OpenGL et les remplacer par les équivalents Direct3D ? »
-   Réponse : non. OpenGL ES 2.0 et Direct3D 11 sont issus de différentes générations de modélisation de pipeline graphique. Bien qu’il existe certaines similarités superficielles entre les concepts et les API, telles que le contexte de rendu et l’instanciation des nuanceurs, vous pouvez consulter ces recommandations ainsi que les informations de référence sur Direct3D 11 pour vous aider à faire les meilleurs choix lors de la recréation de votre pipeline au lieu de tenter un mappage 1 à 1. Toutefois, si vous effectuez un portage de GLSL à HLSL, la création d’un ensemble d’alias communs pour les variables, les intrinsèques et les fonctions GLSL peut non seulement simplifier le portage, mais aussi vous permettre de maintenir un seul ensemble de fichiers de code de nuanceur.

 

 