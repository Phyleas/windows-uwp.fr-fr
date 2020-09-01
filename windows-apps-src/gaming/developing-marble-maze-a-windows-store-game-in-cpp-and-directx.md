---
title: Développement *Marble Maze* &mdash; d’un jeu de plateforme Windows universelle (UWP) généré avec C++ pour DirectX
description: Cette section de la documentation explique comment utiliser DirectX et C++ pour créer un jeu en 3D plateforme Windows universelle (UWP).
ms.assetid: 43f1977a-7e1d-614c-696e-7669dd8a9cc7
ms.date: 08/10/2017
ms.topic: article
keywords: Windows 10, UWP, jeux, exemple, DirectX, 3D
ms.localizationpriority: medium
ms.openlocfilehash: 155bd4bce77cd976cfab11ca4c5c8f184c562912
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165333"
---
# <a name="developing-marble-mazemdasha-universal-windows-platform-uwp-game-built-with-c-for-directx"></a>Développement *Marble Maze* &mdash; d’un jeu de plateforme Windows universelle (UWP) généré avec C++ pour DirectX

Cette rubrique explique comment utiliser DirectX et C++ pour créer un jeu en 3D plateforme Windows universelle (UWP). Le jeu, appelé *labyrinthe de marbre*, prend en considération plusieurs facteurs de forme tels que les tablettes, les ordinateurs de bureau traditionnels et les PC portables.

> [!NOTE]
> Pour télécharger le code source du *labyrinthe marbré* , consultez l' [exemple sur GitHub](https://github.com/microsoft/Windows-appsample-marble-maze).

> [!IMPORTANT]
> Le *labyrinthe* illustre les modèles de conception que nous considérons comme étant les meilleures pratiques pour la création de jeux UWP. Vous pouvez personnaliser de nombreux détails d’implémentation pour les adapter à vos propres méthodes et aux critères spécifiques du jeu que vous développez. Vous êtes libre d’utiliser des techniques ou des bibliothèques différentes si elles sont mieux adaptées à vos besoins. (Toutefois, veillez toujours à ce que votre code passe le [Kit de certification des applications Windows](../debug-test-perf/windows-app-certification-kit.md).) Lorsque nous considérons une implémentation utilisée ici comme essentielle pour réussir le développement de jeux, nous l’insistons dans cette documentation.

## <a name="introducing-marble-maze"></a>Présentation du *labyrinthe marbre*

Nous avons choisi le *labyrinthe de marbre* , car il est relativement basique, mais il illustre encore l’ampleur des fonctionnalités qui se trouvent dans la plupart des jeux. Il indique comment utiliser les graphiques et gérer les données d’entrée et audio. Il montre également la mécanique du jeu, par exemple, les règles et les objectifs.

Le *labyrinthe de marbre* ressemble au jeu labyrinthe Table-Top qui est généralement construit à partir d’une boîte qui contient des trous et un marbre en acier ou en verre. L’objectif du *labyrinthe de marbre* est le même que celui de la version de table : incliner le labyrinthe pour guider le marbre du début à la fin du labyrinthe le plus peu possible, sans laisser le marbre tomber dans l’un des trous. Le *labyrinthe de marbre* ajoute le concept de points de contrôle. Si la bille tombe dans un trou, le jeu est redémarré au dernier emplacement du point de contrôle sur lequel la bille est passée.

Le *labyrinthe de marbre* offre plusieurs moyens permettant à un utilisateur d’interagir avec le tableau de bord. Si vous avez un appareil tactile ou activé par accéléromètre, vous pouvez les utiliser pour bouger le tableau de jeu. Vous pouvez également utiliser un contrôleur Xbox One ou une souris pour contrôler la lecture du jeu.

![capture d’écran du jeu Marble Maze.](images/marblemaze-2.png)

## <a name="prerequisites"></a>Prérequis

-   Windows 10 Creators Update
-   [Microsoft Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
-   Connaissance de la programmation en C++
-   Connaissance de la terminologie DirectX et DirectX
-   Connaissances de base de COM

## <a name="who-should-read-this"></a>Qui doit lire ce document ?

Si vous souhaitez créer des jeux 3D ou d’autres applications gourmandes en graphiques pour Windows 10, c’est pour vous. Nous espérons que vous utiliserez les principes et les pratiques décrits dans cette documentation pour créer votre propre jeu pour UWP. Une expérience ou un intérêt prononcé pour la programmation en C++ et DirectX vous aidera à exploiter au mieux cette documentation. Si vous n’avez pas d’expérience avec DirectX, vous pouvez toujours tirer parti de l’expérience avec les environnements de programmation graphique 3D similaires.

Le document [procédure pas à pas : créer un jeu UWP simple avec DirectX](tutorial--create-your-first-uwp-directx-game.md) décrit un autre exemple qui implémente un jeu de tir 3D de base à l’aide de DirectX et C++.

## <a name="what-this-documentation-covers"></a>Points traités dans cette documentation

Cette documentation explique comment :

-   utiliser l’API Windows Runtime et DirectX pour créer un jeu pour UWP ;
-   Utilisez [Direct3D](/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11) et [Direct2D](/windows/desktop/Direct2D/direct2d-portal) pour travailler avec du contenu visuel, tel que des modèles, des textures, des nuanceurs vertex et de pixels, et des superpositions 2D.
-   Intégrez des mécanismes d’entrée tels que Touch, accéléromètre et le contrôleur Xbox One.
-   utiliser [XAudio2](/windows/desktop/xaudio2/xaudio2-apis-portal) pour incorporer de la musique et des effets sonores.

## <a name="what-this-documentation-does-not-cover"></a>Points non traités dans cette documentation

Cette documentation ne traite pas les aspects suivants du développement de jeux. Ils sont suivis par d’autres ressources qui les traitent.

-   principes de conception de jeu 3D.
-   Notions de base de la programmation en C++ ou DirectX.
-   Procédure de conception des ressources telles que les textures, les modèles ou les données audio.
-   Procédure de dépannage des problèmes liés au comportement ou aux performances de votre jeu.
-   Procédure de préparation de votre jeu pour une utilisation dans d’autres parties du monde.
-   Comment certifier et publier votre jeu sur le Microsoft Store.

Le *labyrinthe de marbre* utilise également la bibliothèque [DirectXMath](/windows/desktop/dxmath/directxmath-portal) pour travailler avec la géométrie 3D et effectuer des calculs physiques, tels que des collisions. DirectXMath n’est pas abordé en profondeur dans cette section. Pour plus d’informations sur la façon dont le *labyrinthe* utilise DirectXMath, reportez-vous au code source.

Bien que le *labyrinthe de marbre* fournisse de nombreux composants réutilisables, il ne s’agit pas d’une infrastructure de développement de jeux complète. Lorsque nous considérons un composant de *labyrinthe de marbre* qui peut être réutilisable dans votre jeu, nous l’insistons dans la documentation.

## <a name="next-steps"></a>Étapes suivantes

Nous vous recommandons de commencer avec les [exemples de notions de base du labyrinthe](marble-maze-sample-fundamentals.md) de marbre pour en savoir plus sur la structure de *labyrinthe* et les indications *Marble Maze* de codage et de style qui suivent. Le tableau suivant récapitule les documents de cette section pour vous permettre de vous y référer plus facilement.

## <a name="in-this-section"></a>Contenu de cette section

| Titre                                                                                                                    | Description                                                                                                                                                                                                                                        |
|--------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Notions de base de l’exemple Marble Maze](marble-maze-sample-fundamentals.md)                                                   | Présente la structure de jeu et les instructions de codage et de style que le code source applique.                                                                                                                                 |
| [Structure de l’application Marble Maze](marble-maze-application-structure.md)                                               | Décrit comment le code de l’application de *labyrinthe de marbre* est structuré et comment la structure d’une application DirectX UWP diffère de celle d’une application de bureau traditionnelle.                                                                                    |
| [Ajout de contenu visuel à l’exemple Marble Maze](adding-visual-content-to-the-marble-maze-sample.md)                   | Décrit certaines pratiques importantes à connaître quand vous utilisez Direct3D et Direct2D. Décrit également comment le *labyrinthe de marbre* applique ces pratiques pour le contenu visuel.                                                                           |
| [Ajout d’entrées et d’interactivité à l’exemple Marble Maze](adding-input-and-interactivity-to-the-marble-maze-sample.md) | Décrit le fonctionnement du *labyrinthe de marbre* avec les entrées d’accéléromètre, tactile et Xbox d’un contrôleur pour permettre aux utilisateurs de parcourir les menus et d’interagir avec le tableau de bord. Décrit également quelques-unes des meilleures pratiques à connaître quand vous utilisez une entrée. |
| [Ajout d’audio à l’exemple Marble Maze](adding-audio-to-the-marble-maze-sample.md)                                     | Décrit le fonctionnement du *labyrinthe* avec un son pour ajouter de la musique et des effets sonores à l’expérience du jeu.                                                                                                                                                  |