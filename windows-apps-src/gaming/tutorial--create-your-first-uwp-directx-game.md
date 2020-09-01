---
title: Créer un jeu de plateforme Windows universelle (UWP) DirectX
description: Dans cet ensemble de didacticiels, vous apprendrez à utiliser DirectX et [C++/WinRT](../cpp-and-winrt-apis/index.md) pour créer l’exemple de jeu de base plateforme Windows universelle (UWP) nommé **Simple3DGameDX**.
ms.assetid: 9edc5868-38cf-58cc-1fb3-8fb85a7ab2c9
keywords: Exemple de jeu DirectX, exemple de jeu, plateforme Windows universelle (UWP), jeu Direct3D 11
ms.date: 06/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 284aa821cc58a49f45bed3b0d7e28c20f9d19ba1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163023"
---
# <a name="create-a-simple-universal-windows-platform-uwp-game-with-directx"></a>Créer un jeu de plateforme Windows universelle (UWP) simple avec DirectX

Dans cet ensemble de didacticiels, vous apprendrez à utiliser DirectX et [C++/WinRT](../cpp-and-winrt-apis/index.md) pour créer l’exemple de jeu de base plateforme Windows universelle (UWP) nommé **Simple3DGameDX**. Le jeu a lieu dans une simple galerie de tournage 3D de première personne.

> [!NOTE]
> Le lien à partir duquel vous pouvez télécharger l’exemple de jeu **Simple3DGameDX** lui-même est le [jeu d’exemples Direct3D](/samples/microsoft/windows-universal-samples/simple3dgamedx/). Le code source C++/WinRT se trouve dans le dossier nommé `cppwinrt` . Pour plus d’informations sur d’autres exemples d’applications UWP, consultez [obtenir des exemples](../get-started/get-app-samples.md)d’applications UWP.

Ces didacticiels couvrent tous les aspects majeurs d’un jeu, y compris les processus de chargement des ressources telles que les arts et les mailles, la création d’une boucle de jeu principale, l’implémentation d’un pipeline de rendu simple et l’ajout de sons et de contrôles.

Vous y verrez également les techniques et les considérations relatives au développement de jeux UWP. Nous nous concentrerons sur les concepts clés du développement de jeux DirectX UWP et appelons les considérations spécifiques à l’exécution de Windows autour de ces concepts.

## <a name="objective"></a>Objectif

Pour en savoir plus sur les concepts de base et les composants d’un jeu DirectX UWP, et pour vous familiariser avec la conception de jeux UWP avec DirectX.

## <a name="what-you-need-to-know"></a>Ce que vous devez savoir

Pour ce didacticiel, vous devez vous familiariser avec ces sujets.

- [/WinRT C++](../cpp-and-winrt-apis/index.md). C++/WinRT est une projection de langage C++ 17 moderne standard pour les API Windows Runtime (WinRT), implémentée en tant que bibliothèque basée sur un fichier d’en-tête et conçue pour vous fournir un accès de première classe aux API Windows modernes.
- Algèbre linéaire de base et concepts physiques newtoniens.
- Terminologie de programmation de graphiques de base.
- Concepts de programmation Windows de base.
- Connaissances de base des API [Direct2D](/windows/desktop/Direct2D/direct2d-portal) et [Direct3D 11](/windows/desktop/direct3d11/how-to-use-direct3d-11).

##  <a name="direct3d-uwp-shooting-gallery-sample"></a>Exemple de Galerie de tir UWP Direct3D

L’exemple de jeu **Simple3DGameDX** implémente une simple galerie de tournage 3D de première personne, où le joueur déclenche des billes sur les cibles mobiles. Le joueur gagne un certain nombre de points en atteignant chaque cible et il peut traverser 6 niveaux de défis de plus en plus difficiles. À la fin des niveaux, les points sont comptés et un score final est attribué au joueur.

L’exemple illustre ces concepts de jeu.

- Interopérabilité entre DirectX 11.1 et Windows Runtime
- Caméra et perspective 3D subjectives
- Effets 3D stéréoscopiques
- Collision-détection entre des objets en 3D
- Gestion des entrées de joueur pour les contrôles de souris, de toucher et de contrôleur Xbox
- Mixage audio et lecture
- Un état de jeu de base-machine

![l’exemple de jeu en action](images/simple-dx-game-overview.png)

|Rubrique|Description|
|-------|-------------|
|[Configurer le projet de jeu](tutorial--setting-up-the-games-infrastructure.md)|La première étape du développement de votre jeu consiste à configurer un projet dans Microsoft Visual Studio. Une fois que vous avez configuré un projet spécifiquement pour le développement de jeux, vous pouvez le réutiliser ultérieurement comme type de modèle.|
|[Définir l’infrastructure d’application UWP du jeu](tutorial--building-the-games-uwp-app-framework.md)|La première étape du codage d’un jeu de plateforme Windows universelle (UWP) est la création de l’infrastructure qui permet à l’objet d’application d’interagir avec Windows.|
|[Gestion de flux de jeu](tutorial-game-flow-management.md)|Définissez l’ordinateur d’état de haut niveau pour activer l’interaction du joueur et du système. Découvrez comment l’interface utilisateur interagit avec la machine d’État du jeu global et comment créer des gestionnaires d’événements pour les jeux UWP.|
|[Définir l’objet jeu principal](tutorial--defining-the-main-game-loop.md)|À présent, nous examinons les détails de l’objet principal de l’exemple de jeu et de la façon dont les règles qu’il implémente traduisent en interactions avec le monde du jeu.|
|[Infrastructure de rendu I : présentation du rendu](tutorial--assembling-the-rendering-pipeline.md)|Découvrez comment développer le pipeline de rendu pour afficher des graphiques. Présentation du rendu.|
|[Infrastructure de rendu II : rendu de jeu](tutorial-game-rendering.md)|Découvrez comment assembler le pipeline de rendu pour afficher des graphiques. Le rendu du jeu, la configuration et la préparation des données.|
|[Ajouter une interface utilisateur](tutorial--adding-a-user-interface.md)|Découvrez comment ajouter une superposition d’interface utilisateur 2D à un jeu DirectX UWP.|
|[Ajouter des contrôles](tutorial--adding-controls.md)|À présent, nous voyons comment l’exemple de jeu implémente les contrôles de déplacement et d’apparence dans un jeu 3D et comment développer des contrôles tactiles, de souris et de contrôleurs de jeu de base.|
|[Ajouter du son](tutorial--adding-sound.md)|Développez un moteur audio simple à l’aide des API XAudio2 pour lire la musique et les effets sonores du jeu.|
|[Développer l’exemple de jeu](tutorial-resources.md)|Découvrez comment implémenter une superposition XAML pour un jeu DirectX UWP.|