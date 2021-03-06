---
description: Découvrez les outils et les techniques qui peuvent vous aider à écrire des applications qui prennent en charge Windows, iOS et Android.
title: Sélection d’une approche d’iOS et développement d’applications pour UWP
ms.assetid: 5CDAB313-07B7-4A32-A49B-026361DCC853
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b7a8920b13b7e28947138a000fd46165abcb58b7
ms.sourcegitcommit: afc4ff2c89f148d32073ab1cc42063ccdc573a8c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104680"
---
# <a name="selecting-an-approach-to-ios-and-uwp-app-development"></a>Sélection d’une approche d’iOS et développement d’applications pour UWP


Quels sont les choix possibles lors du développement d’applications multiplateformes ?

## <a name="whats-the-best-way-to-support-both-ios-and-windows"></a>Quel est le meilleur moyen de prise en charge des applications iOS et Windows ?

Windows et iOS peuvent sembler très différents, mais de plus en plus d'outils et de techniques peuvent vraiment vous aider pour l’écriture d’applications qui prennent en charge les deux plateformes (et même Android). La meilleure solution dépend du type d'application que vous écrivez et du mode de création : à partir de zéro ou par le portage d’un projet existant.

## <a name="writing-a-new-app"></a>Écriture d’une nouvelle application

Avec une nouvelle tablette, vous disposez de nombreuses options à votre disposition, notamment :

-   [Xamarin](https://xamarin.com/)

    Avec Xamarin, vous pouvez écrire votre application en C#, l’exécuter sous Windows et créer également des applications iOS natives. La prise en charge de Xamarin est intégrée à Visual Studio ; sélectionnez simplement le type de projet approprié.

-   [Apache Cordova](https://www.microsoft.com/?ref=go)

    Si vous préférez Javascript et HTML, Apache Cordova (également appelé PhoneGap) vous aidera à créer des applications multiplateformes pour iOS, Windows et Android. Ce type de projet est également intégré à Visual Studio.

-   Moteurs de jeu

    Avec des outils comme [Unity3D](https://www.unity3d.com/) et [Unreal Engine](https://www.unrealengine.com/en-US/) à votre disposition, vous pouvez écrire des jeux de qualité AAA pour Windows et de nombreuses autres plateformes, y compris iOS. Unity prend en charge l’écriture de script en C#, Unreal utilise C++.

-   [MonoGame](http://www.monogame.net/)

    Le successeur spirituel de XNA. C’est maintenant une infrastructure multiplateforme open source, ce qui signifie que vous pouvez écrire des applications en C# pour de nombreuses plateformes avec prise en charge des moteurs physiques et des graphismes 2D et 3D.

## <a name="adapting-an-existing-app"></a>Adaptation d’une application existante

Avec une application iOS existante, les options sont un peu plus limitées. Néanmoins, tout n’est pas perdu.

-   [Pont Windows pour iOS](https://github.com/Microsoft/WinObjC)

    Il s’agit d’un outil toujours en développement qui peut importer des projets Xcode directement dans Visual Studio. Le code Objective-C peut être généré et débogué dans Visual Studio. Si votre projet utilise des bibliothèques telles que Cocos pour les graphiques, cela peut s'avérer un moyen utile pour un portage rapide de votre application.

-   Réutilisez votre code C++.

    Si votre logique d'entreprise est écrite en C++, plutôt qu’en Objective-C ou Swift, vous pouvez souvent utiliser ce code avec des modifications mineures de votre projet. Vous pouvez ensuite utiliser XAML pour définir votre interface utilisateur, comme avec d'autres applications Windows, et appeler le code C++ lorsque cela est nécessaire.

-   [Utiliser ANGLE pour exécuter OpenGL ES sous Windows](https://github.com/microsoft/angle/wiki)

    Une étape intermédiaire pour le portage de votre projet OpenGL ES 2.0 consiste à utiliser ANGLE. ANGLE pour Windows Store vous permet d’exécuter le contenu OpenGL ES sous Windows en convertissant les appels d’API OpenGL ES en appels d’API DirectX 11.

## <a name="other-cross-platform-authoring-tools"></a>Autres outils de création multiplateformes

-   [GameSalad](https://gamesalad.com/)

    Environnement de création de jeux.

-   [Construct 2]( https://www.scirra.com/)

    Environnement de création de jeux.

-   [Titanium Studio](https://www.appcelerator.com/platform/titanium-studio/)

    Environnement de création multiplateforme.

-   [Cocos2D-x](https://www.cocos2d-x.org/)

    Bibliothèque de code interplateforme pour la gestion des sprites et la modélisation physique.

-   [Impact.js](https://impactjs.com/)

    Bibliothèque de jeux HTML.

-   [Marmalade](http://madewithmarmalade.com/)

    Kit de développement logiciel interplateforme.

-   [OpenFL](https://www.openfl.org/)

    Outil de développement multiplateforme.

-   [GameMaker](https://www.yoyogames.com/gamemaker/studio)

    Environnement de création spécifique aux jeux.

-   [PlayCanvas](https://playcanvas.com/)

    Outil de développement de jeux HTML.

