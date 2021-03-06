---
title: Technologies de jeu des applications pour la plateforme Windows universelle (UWP)
description: Ce guide décrit les technologies disponibles pour le développement de jeux UWP.
ms.assetid: bc4d4648-0d6e-efbb-7608-80bd09decd6e
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, jeux, technologie, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: a8fcf2b7a621b7d76411834826bf5db43ddbcfb7
ms.sourcegitcommit: 0f2ae8f97daac440c8e86dc07d11d356de29515c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/13/2020
ms.locfileid: "83280209"
---
# <a name="game-technologies-for-uwp-apps"></a>Technologies de jeu des applications UWP

Ce guide décrit les technologies disponibles pour le développement de jeux UWP.

##  <a name="benefits-of-windows10-for-game-development"></a>Avantages de Windows 10 pour le développement de jeux

L’introduction de la plateforme Windows universelle (UWP) dans Windows 10 vous permet d’étendre vos titres Windows 10 à toutes les plateformes Microsoft. La migration gratuite à partir des versions précédentes de Windows génère un nombre sans cesse croissant de clients Windows 10. La combinaison de ces deux facteurs signifie que vos titres Windows 10 seront en mesure d’atteindre un grand nombre de clients par le biais du Microsoft Store.

En outre, Windows 10 présente de nombreuses nouveautés particulièrement avantageuses pour les jeux, à savoir :

-   Réduction de la pagination de la mémoire et de la taille globale du système de mémoire
-   Amélioration de la gestion de la mémoire graphique qui alloue et protège activement davantage de mémoire pour le jeu au premier plan

## <a name="uwp-games-with-c-and-directx"></a>Jeux UWP avec C++ et DirectX

Les jeux en temps réel exigeant des performances élevées doivent utiliser des API DirectX. DirectX est un ensemble d’API natives pour la création de jeux et d’applications multimédias qui exigent des performances élevées, tels les jeux 3D.

## <a name="development-environment"></a>Environnement de développement

Pour créer des jeux pour UWP, vous devez configurer votre environnement de développement en installant Visual Studio 2015 ou une version ultérieure. Nous vous recommandons d’installer la dernière version de Visual Studio, en vous donnant accès aux dernières mises à jour de sécurité et de développement. Visual Studio vous permet de créer des applications UWP et fournit des outils pour le développement de jeux :

-   Outils Visual Studio pour la programmation de jeux DX - Visual Studio offre des outils permettant de créer, modifier, prévisualiser et exporter des ressources d’image, de modèle et de nuanceur. Il existe également des outils que vous pouvez utiliser pour convertir des ressources au moment de la création et pour déboguer le code graphique DirectX. Pour plus d’informations, voir [Utiliser Visual Studio Tools pour la programmation de jeux](set-up-visual-studio-for-game-development.md).
-   Fonctionnalités de Graphics Diagnostics dans Visual Studio - Les outils de diagnostic de graphiques sont désormais disponibles dans Windows en tant que fonctionnalités facultatives. Les outils de diagnostic vous permettent d’effectuer des tâches de débogage graphique et d’analyse des frames graphiques, ainsi que de surveiller l’utilisation du processeur graphique (GPU) en temps réel. Pour plus d’informations, voir [Utiliser les fonctionnalités de diagnostic de graphiques de Visual Studio et du runtime DirectX](use-the-directx-runtime-and-visual-studio-graphics-diagnostic-features.md).

Pour plus d’informations, consultez préparer votre plateforme Windows universelle et [programmation DirectX](directx-programming.md).

## <a name="getting-started-with-directx-game-project-templates"></a>Prise en main des modèles de projet de jeu DirectX

Après avoir configuré votre environnement de développement, vous pouvez utiliser l’un des modèles de projet DirectX associés pour créer votre jeu DirectX UWP. Visual Studio 2015 comporte trois modèles disponibles pour la création de projets DirectX UWP : **Application DirectX 11 (Windows universel)**, **Application DirectX 12 (Windows universel)** et **Application DirectX 11 et XAML (Windows universel)**. Pour plus d’informations, voir [Créer un projet de jeux DirectX et Plateforme Windows universelle à partir d’un modèle](user-interface.md).

## <a name="windows-10-apis"></a>API Windows 10

Windows 10 fournit une collection complète d’API qui sont utiles pour le développement de jeux. Il existe des API pour quasiment tous les aspects des jeux : graphiques 3D, graphiques 2D, audio, entrée, ressources texte, interface utilisateur et réseau.

De nombreuses API sont liées au développement de jeux, mais tous les jeux ne doivent utiliser toutes les API. Par exemple, certains jeux utilisant uniquement des éléments graphiques 3D n’ont besoin que de Direct3D, d’autres utilisant uniquement des éléments graphiques 2D n’ont besoin que de Direct2D, et d’autres encore utilisent les deux. Le diagramme suivant présente les API liées au développement de jeux, regroupées par type de fonctionnalité.

![technologies de plateforme de jeu](images/gameplatformtechnologies.png)

-   Graphiques 3D : Windows 10 prend en charge deux jeux d’API graphiques 3D, Direct3D 11 et [Direct3D 12](/windows/win32/direct3d12/directx-12-programming-guide). Ces API permettent de créer des éléments graphiques 2D et 3D. Les jeux d’API graphiques Direct3D 11 et Direct3D 12 ne sont pas utilisés ensemble, mais chacun peut être utilisé avec toute API du groupe d’interfaces utilisateur et d’éléments graphiques 2D. Pour plus d’informations sur l’utilisation des API graphiques dans votre jeu, voir [Graphismes 3D de base pour jeux DirectX](an-introduction-to-3d-graphics-with-directx.md).

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">Description</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td align="left">Direct3D 12</td>
    <td align="left"><p>Direct3D 12 introduit la nouvelle version de Direct3D, l’API graphique 3D au cœur de DirectX. Cette version de Direct3D est conçue pour être plus rapide et plus efficace que les versions précédentes. La contrepartie de la vitesse accrue de l’API Direct3D 12 est qu’étant de niveau inférieur, elle requiert que vous gériez vous-même vos ressources graphiques, et disposiez d’une plus vaste expérience en matière de programmation d’éléments graphiques pour tirer parti de cette vitesse accrue.</p>
    <p><strong>Quand l’utiliser</strong></p>
    <p>Utilisez Direct3D 12 lorsque vous devez optimiser les performances de votre jeu et que celui-ci utilise le processeur de manière intensive.</p>
    <p><strong>Pour plus d’informations</strong></p>
    <p>Voir la documentation concernant <a href="/windows/win32/direct3d12/directx-12-programming-guide">Direct3D 12</a>.</p></td>
    </tr>
    <tr>
    <td align="left">Direct3D 11</td>
    <td align="left"><p>Direct3D 11 est la version précédente de Direct3D. Elle permet de créer des éléments graphiques 3D avec un niveau d’abstraction matérielle supérieur à celui de Direct3D 12.</p>
    <p><strong>Quand l’utiliser</strong></p>
    <p>Utilisez Direct3D 11 si vous avez du code Direct3D 11 existant, si votre jeu n’utilise pas le processeur de manière intensive, ou si voulez bénéficier de l’avantage de ne pas à avoir à gérer vous-même vos ressources.</p>
    <p><strong>Pour plus d’informations</strong></p>
    <p>Voir la documentation concernant <a href="/windows/win32/direct3d11/atoc-dx-graphics-direct3d-11">Direct3D 11</a>.</p></td>
    </tr>
    </tbody>
    </table>

     

-   Éléments graphiques et interfaces utilisateur 2D - API concernant les éléments graphiques 2D, tels que des textes et des interfaces utilisateur. Toutes les API pour éléments graphiques et interfaces utilisateur 2D sont facultatives.

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">Description</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td align="left">Direct2D</td>
    <td align="left"><p>Direct2D est une API graphique 2D à accélération matérielle et en mode immédiat, qui offre des performances élevées et un rendu de grande qualité pour les éléments géométriques, bitmaps et textes 2D. L’API Direct2D repose sur Direct3D. Elle est conçue pour fonctionner correctement avec GDI, GDI+ et Direct3D.</p>
    <p><strong>Quand l’utiliser</strong></p>
    <p>Vous pouvez utiliser Direct2D soit à la place de Direct3D afin de produire des graphismes pour des jeux purement 2D, tels des jeux à défilement horizontal ou de plateau, soit avec Direct3D pour simplifier la création d’éléments graphiques 2D dans des jeux 3D tels que des interfaces utilisateur ou des affichages tête haute.</p>
    <p><strong>Pour plus d’informations</strong></p>
    <p>Voir la documentation concernant <a href="/windows/win32/Direct2D/direct2d-portal">Direct2D</a>.</p></td>
    </tr>
    <tr>
    <td align="left">DirectWrite</td>
    <td align="left"><p>DirectWrite apporte des fonctionnalités supplémentaires de manipulation de texte. Il peut être utilisé avec Direct3D ou Direct2D afin de fournir une sortie de texte pour les interfaces utilisateur ou d’autres zones où du texte est requis. DirectWrite prend en charge la mesure, le dessin et le test de résultats de texte multiformat. DirectWrite gère le texte dans toutes les langues prises en charge pour les applications localisées et globales. DirectWrite fournit également une API de rendu de glyphe de bas niveau pour les développeurs désireux de réaliser leurs propres disposition et traitement d’Unicode à glyphe.</p>
    <p><strong>Quand l’utiliser</strong></p>
    <p></p>
    <p><strong>Pour plus d’informations</strong></p>
    <p>Voir la documentation concernant <a href="/windows/win32/DirectWrite/direct-write-portal">DirectWrite</a>.</p></td>
    </tr>
    <tr>
    <td align="left">DirectComposition</td>
    <td align="left"><p>DirectComposition est un composant Windows qui permet de composer des bitmaps complexes avec des transformations, des effets et des animations. Les développeurs d’applications peuvent utiliser l’API DirectComposition pour créer des interfaces utilisateur visuellement attrayantes avec des transitions riches et fluides d’un élément visuel à l’autre.</p>
    <p><strong>Quand l’utiliser</strong></p>
    <p>DirectComposition est conçue pour simplifier le processus de composition d’éléments visuels et de création de transitions animées. Si votre jeu nécessite des interfaces utilisateur complexes, vous pouvez utiliser DirectComposition pour simplifier la création et la gestion de l’interface utilisateur.</p>
    <p><strong>Pour plus d’informations</strong></p>
    <p>Voir la documentation concernant <a href="/windows/win32/directcomp/directcomposition-portal">DirectComposition</a>.</p></td>
    </tr>
    </tbody>
    </table>

     

-   Audio - API concernant la lecture audio et l’application d’effets audio. Pour plus d’informations sur l’utilisation des API audio dans votre jeu, voir [Audio pour les jeux](working-with-audio-in-your-directx-game.md).

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">Description</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td align="left">XAudio2</td>
    <td align="left"><p>XAudio2 est une API audio de bas niveau qui constitue le fondement du traitement et du mixage du signal. XAudio est conçue pour être très réactive aux moteurs audio de jeu tout en conservant la possibilité de créer des effets audio personnalisés et des chaînes complexes d’effets et de filtres audio.</p>
    <p><strong>Quand l’utiliser</strong></p>
    <p>Utilisez XAudio2 quand votre jeu doit lire des sons en n’occasionnant qu’un minimum de surcharge et de ralentissement.</p>
    <p><strong>Pour plus d’informations</strong></p>
    <p>Voir la documentation concernant <a href="/windows/win32/xaudio2/xaudio2-apis-portal">XAudio2</a>.</p></td>
    </tr>
    <tr>
    <td align="left">Graphiques audio</td>
    <td align="left"><p>Pour les fonctionnalités que vous pouvez implémenter avec XAudio2, vous avez la possibilité d’utiliser à la place les API graphiques audio Windows Runtime. Pour vous aider à choisir entre les deux alternatives, consultez <a href="/windows/uwp/audio-video-camera/audio-graphs#choosing-windows-runtime-audiograph-or-xaudio2">choix de Windows Runtime AudioGraph ou XAudio2</a>.</p>
    <p><strong>Quand l’utiliser</strong></p>
    <p>Utilisez des graphiques audio lorsque votre jeu doit jouer des sons avec une surcharge et un délai minimaux, mais avec une API beaucoup plus facile à utiliser que XAudio2 et avec l’option de prise en charge de C#.</p>
    <p><strong>Pour plus d’informations</strong></p>
    <p>Consultez la documentation sur les <a href="/windows/uwp/audio-video-camera/audio-graphs">graphiques audio</a> .</p></td>
    </tr>
    <tr>
    <td align="left">Media Foundation</td>
    <td align="left"><p>Microsoft Media Foundation est conçue pour la lecture de fichiers multimédias et de flux audio et vidéo, mais peut également servir dans des jeux quand des fonctionnalités de niveau supérieur à XAudio2 sont requises et qu’une certaine surcharge supplémentaire est acceptable.</p>
    <p><strong>Quand l’utiliser</strong></p>
    <p>Media Foundation est particulièrement utile pour les scènes cinématographiques ou les composants non-interactifs de votre jeu. Media Foundation est également utile pour le décodage de fichiers audio avant leur lecture à l’aide de XAudio2.</p>
    <p><strong>Pour plus d’informations</strong></p>
    <p>Consultez la vue d’ensemble de <a href="/windows/win32/medfound/microsoft-media-foundation-sdk">Microsoft Media Foundation</a> .</p></td>
    </tr>
    </tbody>
    </table>

     

-   Entrée - API concernant diverses sources d’entrée d’utilisateurs telles que le clavier, la souris, le boîtier de commande et autres.

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">Description</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td align="left">XInput</td>
    <td align="left"><p>L’API de contrôleur de jeu XInput permet aux applications de recevoir des entrées de contrôleurs de jeu.</p>
    <p><strong>Quand l’utiliser</strong></p>
    <p>Si votre jeu doit prendre en charge l’entrée par le biais du boîtier de commande et que vous avez du code XInput existant, vous pouvez continuer à utiliser XInput. XInput ayant été remplacé par Windows.Gaming.Input pour UWP, si vous écrivez un nouveau code d’entrée, vous devez utiliser Windows.Gaming.Input au lieu de XInput.</p>
    <p><strong>Pour plus d’informations</strong></p>
    <p>Voir la documentation concernant <a href="/windows/win32/xinput/xinput-game-controller-apis-portal">XInput</a>.</p></td>
    </tr>
    <tr>
    <td align="left">Windows.Gaming.Input</td>
    <td align="left"><p>L’API Windows.Gaming.Input remplaçant XInput offre les mêmes fonctionnalités que celle-ci, avec les avantages suivants :</p>
    <ul>
    <li>Moindre utilisation des ressources</li>
    <li>Moindre latence d’appel d’API pour récupérer l’entrée</li>
    <li>Possibilité d’utiliser plus de 4 boîtiers de commande à la fois</li>
    <li>La possibilité d’accéder à d’autres fonctionnalités de boîtier de manette de l’un des autres, telles que les moteurs vibrants de déclenchement</li>
    <li>Possibilité d’être averti quand des contrôleurs se connectent/déconnectent via un événement au lieu d’une interrogation</li>
    <li>Possibilité d’attribuer une entrée à un utilisateur spécifique (Windows.System.User)</li>
    </ul>
    <p><strong>Quand l’utiliser</strong></p>
    <p>Si votre jeu doit prendre en charge l’entrée de boîtier de commande et n’utilise pas de code XInput existant, ou si vous avez besoin d’un des avantages répertoriés ci-dessus, vous devez utiliser Windows.Gaming.Input.</p>
    <p><strong>Pour plus d’informations</strong></p>
    <p>Voir la documentation concernant <a href="/uwp/api/Windows.Gaming.Input">Windows.Gaming.Input</a>.</p></td>
    </tr>
    <tr>
    <td align="left">Windows.UI.Core.CoreWindow</td>
    <td align="left"><p>La classe Windows.UI.Core.CoreWindow fournit des événements pour suivre les actions d’activation et de déplacement du pointeur, ainsi que des touches Haut et Bas.</p>
    <p><strong>Quand l’utiliser</strong></p>
    <p>Utilisez des événements Windows.UI.Core.CoreWindows quand vous devez suivre les actions de la souris ou des touches dans votre jeu.</p>
    <p><strong>Pour plus d’informations</strong></p>
    <p>Pour plus d’informations sur l’utilisation de la souris ou du clavier dans votre jeu, voir <a href="/windows/uwp/gaming/tutorial--adding-move-look-controls-to-your-directx-game">Contrôles de déplacement/vue pour les jeux</a>.</p></td>
    </tr>
    </tbody>
    </table>

     

-   Math - API concernant la simplification d’opérations mathématiques couramment utilisées.

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">Description</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td align="left">DirectXMath</td>
    <td align="left"><p>L’API DirectXMath fournit des types et fonctions C++ compatibles SIMD pour des opérations mathématiques courantes d’algèbre linéaire et de graphiques communes aux jeux.</p>
    <p><strong>Quand l’utiliser</strong></p>
    <p>L’utilisation de DirectXMath est facultative. Elle simplifie les opérations mathématiques courantes.</p>
    <p><strong>Pour plus d’informations</strong></p>
    <p>Voir la documentation concernant <a href="/windows/win32/dxmath/directxmath-portal">DirectXMath</a>.</p></td>
    </tr>
    </tbody>
    </table>

     

-   Réseau - API concernant la communication avec d’autres ordinateurs et appareils par le biais d’Internet ou de réseaux privés.

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">Description</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td align="left">Windows.Networking.Sockets</td>
    <td align="left"><p>L’espace de noms Windows.Networking.Sockets fournit des sockets TCP et UDP qui permettent une communication réseau fiable ou non fiable.</p>
    <p><strong>Quand l’utiliser</strong></p>
    <p>Utilisez Windows.Networking.Sockets si votre jeu doit communiquer avec d’autres ordinateurs ou appareils via le réseau.</p>
    <p><strong>Pour plus d’informations</strong></p>
    <p>Voir <a href="/windows/uwp/gaming/work-with-networking-in-your-directx-game">Utiliser le réseau dans votre jeu</a>.</p></td>
    </tr>
    <tr>
    <td align="left">Windows.Web.HTTP</td>
    <td align="left"><p>L’espace de noms Windows.Web.HTTP fournit aux serveurs HTTP une connexion fiable qui peut être utilisée pour accéder à un site web.</p>
    <p><strong>Quand l’utiliser</strong></p>
    <p>Utilisez Windows.Web.HTTP quand votre jeu doit accéder à un site web pour récupérer ou stocker des informations.</p>
    <p><strong>Pour plus d’informations</strong></p>
    <p>Voir <a href="/windows/uwp/gaming/work-with-networking-in-your-directx-game">Utiliser le réseau dans votre jeu</a>.</p></td>
    </tr>
    </tbody>
    </table>

     

-   Utilitaires de support - Bibliothèques qui s’appuient sur les API de Windows 10.

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">Bibliothèque</th>
    <th align="left">Description</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td align="left">Kit de ressources DirectX</td>
    <td align="left"><p>Le Kit de ressources DirectX (DirectXTK) est une collection de classes d’assistance pour l’écriture de code DirectX 11.x en C++.</p>
    <p><strong>Quand l’utiliser</strong></p>
    <p>Utilisez le Kit de ressources DirectX si vous êtes un développeur en C++ cherchant une alternative moderne au code hérité des utilitaires D3DX, ou un développeur en XNA Game Studio désireux de passer à C++ natif.</p>
    <p><strong>Pour plus d’informations</strong></p>
    <p>Consultez la page projet du kit d’outils DirectX, <a href="https://github.com/Microsoft/DirectXTK">https://github.com/Microsoft/DirectXTK</a> .</p></td>
    </tr>
    <tr>
    <td align="left">Win2D</td>
    <td align="left"><p>Win2D est une API Windows Runtime facile à utiliser pour le rendu d’éléments graphiques 2D en mode immédiat.</p>
    <p><strong>Quand l’utiliser</strong></p>
    <p>Utilisez Win2D si vous êtes un développeur en C++ désireux de disposer d’un wrapper WinRT plus facile à utiliser pour Direct2D et DirectWrite, ou si vous êtes un développeur en C# souhaitant utiliser Direct2D et DirectWrite.</p>
    <p><strong>Pour plus d’informations</strong></p>
    <p>Consultez la page du projet Win2D, <a href="https://github.com/Microsoft/Win2D">https://github.com/Microsoft/Win2D</a> .</p></td>
    </tr>
    </tbody>
    </table>

## <a name="xbox-live-services"></a>Services Xbox Live

Le [programme de créateurs de Xbox Live](https://developer.microsoft.com/games/xbox/xboxlive/creator) permet aux développeurs d’intégrer Xbox Live dans leur jeu UWP et de les publier sur Xbox One et Windows 10. Intégrez des expériences sociales Xbox Live, telles que la connexion, la présence, la Leaderboards, et bien plus encore dans votre titre, avec un minimum de temps de développement. Les fonctionnalités sociales de Xbox Live sont conçues pour augmenter de manière naturelle votre audience, en étendant la sensibilisation à plus de 55 millions joueurs actifs.

Si vous souhaitez accéder à d’autres fonctionnalités Xbox Live, à une prise en charge du marketing et du développement dédié, et à la possibilité de figurer dans le magasin Xbox principal, appliquez le [ID@Xbox](https://www.xbox.com/developers/id) programme. Pour connaître les fonctionnalités disponibles pour le programme et le programme des créateurs Xbox Live ID@Xbox , consultez le [tableau des fonctionnalités](/gaming/xbox-live/developer-program-overview.md#feature-table).

Pour plus d’informations, consultez [Ajout de Xbox Live à votre jeu](e2e.md#adding-xbox-live-to-your-game).

##  <a name="alternatives-to-writing-games-with-directx-and-uwp"></a>Alternatives à l’écriture de jeux avec DirectX et UWP

### <a name="uwp-games-without-directx"></a>Jeux UWP sans DirectX

Des jeux plus simples, sans exigences de performances minimales, tels des jeux de carte ou de plateau, peuvent être écrits sans DirectX et ne nécessitent pas nécessairement une écriture en C++. Ces types de jeux peuvent utiliser tout langage pris en charge par UWP, tel que C#, Visual Basic, C++ et HTML/JavaScript. Si votre jeu ne requiert pas une performance particulière ni un graphisme intensif, consultez l’[exemple de jeu tactile en JavaScript et HTML5](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample/JavaScript%20and%20HTML5%20touch%20game%20sample/%5BJavaScript%5D-JavaScript%20and%20HTML5%20touch%20game%20sample/JavaScript).

### <a name="game-engines"></a>Moteurs de jeu

Comme alternative à l’écriture de votre propre moteur de jeu à l’aide des API de développement de jeux Windows, de nombreux moteurs de jeu de haute qualité qui s’appuient sur les API de développement de jeux Windows sont disponibles pour le développement de jeux sur les plateformes Windows. Lorsque vous envisagez un moteur de jeu ou une bibliothèque de jeux, plusieurs options s’offrent à vous :

-   Moteur de jeu complet - Un moteur de jeu complet encapsule la plupart ou la totalité des API Windows 10 utiles lors de l’écriture d’un moteur de jeu à partir de rien, par exemple, en rapport avec les graphiques, l’audio, les entrées et l’utilisation du réseau. Les moteurs de jeu complets peuvent également offrir des fonctionnalités logiques de jeu, telles que l’intelligence artificielle et la recherche de chemin d’accès.
-   Moteur graphique - Les moteurs graphiques encapsulent les API graphiques Windows 10, gèrent les ressources graphiques, et prennent en charge une variété de formats de modèle et d’environnement.
-   Moteur audio - Les moteurs audio encapsulent les API audio Windows 10, gèrent les ressources audio et fournissent un traitement et des effets audio avancés.
-   Moteur réseau - Les moteurs réseau encapsulent les API de réseau Windows 10 pour l’ajout à votre jeu de la prise en charge du mode multijoueur de pair à pair ou basé sur un serveur, et peuvent inclure des fonctionnalités de réseau avancées pour prendre en charge un grand nombre de joueurs.
-   Intelligence artificielle et moteur de recherche de chemin d’accès - L’IA et les moteurs de recherche de chemin d’accès fournissent une infrastructure pour contrôler le comportement d’agents dans votre jeu.
-   Moteurs spéciaux - Divers moteurs supplémentaires existent pour la gestion de toute tâche de développement de jeu vous pourriez être amené à exécuter, telle que la création de systèmes de stock et d’arborescences de boîte de dialogue.

## <a name="submitting-a-game-to-the-microsoft-store"></a>Envoi d’un jeu au Microsoft Store

Une fois que vous êtes prêt à publier votre jeu, vous devez créer un compte de développeur et soumettre votre jeu au Microsoft Store.

Pour plus d’informations sur l’envoi de votre jeu à la Microsoft Store, consultez [envoi et publication de votre jeu](e2e.md#submitting-and-publishing-your-game).
