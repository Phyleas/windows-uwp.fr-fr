---
title: Graphismes 3D de base pour jeux DirectX
description: Nous vous expliquons comment utiliser la programmation DirectX pour implémenter les concepts fondamentaux des graphismes 3D.
ms.assetid: 2989c91f-7b45-7377-4e83-9daa0325e92e
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, jeux, DirectX, graphiques
ms.localizationpriority: medium
ms.openlocfilehash: 68ee694c036d4bc92f76ea75f7c5e5e530e26334
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173193"
---
# <a name="basic-3d-graphics-for-directx-games"></a>Graphismes 3D de base pour jeux DirectX



Nous vous expliquons comment utiliser la programmation DirectX pour implémenter les concepts fondamentaux des graphismes 3D.

**Objectif :** découvrir comment programmer une application de graphisme 3D.

## <a name="prerequisites"></a>Prérequis


Nous partons du principe que vous êtes familiarisé avec C++. Vous avez également besoin d’une expérience de base dans les concepts de programmation graphique.

**Durée de réalisation totale :** 30 minutes.

## <a name="where-to-go-from-here"></a>Où aller à partir d’ici


Ici, nous parlerons du développement de graphiques 3D avec DirectX et C++ \\ CX. Ce didacticiel en cinq parties présente l’API [Direct3D](/windows/desktop/direct3d), ainsi que les concepts et le code qui sont également employés dans de nombreux autres exemples DirectX. Ces parties s’appuient les unes sur les autres, de la configuration de DirectX pour votre application du Windows Store en C++ jusqu’à l’application de textures aux primitives et à l’ajout d’effets.

> **Remarque**    Ce didacticiel utilise un système de coordonnées droitier avec des vecteurs de colonne. De nombreux exemples et applications DirectX utilisent un système de coordonnées pour gaucher avec des vecteurs lignes. Pour une solution de calcul graphique plus complète et qui prend en charge un système de coordonnées pour gaucher avec des vecteurs lignes, songez à utiliser [DirectXMath](/windows/desktop/dxmath/directxmath-portal). Pour plus d’informations, voir [Utilisation de DirectXMath avec Direct3D](/windows/desktop/dxmath/pg-xnamath-migration-d3dx).

 

Les opérations suivantes sont abordées :

-   Initialiser les interfaces [Direct3D](/windows/desktop/direct3d) à l’aide de Windows Runtime
-   Appliquer d’opérations par vertex shader
-   Configurer la géométrie
-   Rastériser la scène (aplanissement de la scène 3D en projection 2D)
-   Éliminer les surfaces masquées

> **Remarque**  

 

Nous créons ensuite un appareil Direct3D, une chaîne d’échange et une vue de cible de rendu, puis présentons l’image rendue à l’écran.

[Démarrage rapide : configuration de ressources DirectX et affichage d’une image](setting-up-directx-resources.md)

## <a name="related-topics"></a>Rubriques connexes


* [Graphismes Direct3D 11](/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11)
* [DXGI](/windows/desktop/direct3ddxgi/dx-graphics-dxgi)
* [HLSL](/windows/desktop/direct3dhlsl/dx-graphics-hlsl)

 

 